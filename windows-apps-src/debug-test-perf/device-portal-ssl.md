---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Bereitstellen eines Windows-Geräteportals mit einem benutzerdefinierten SSL-Zertifikat
description: Erfahre, wie du das Windows-Geräteportal mit einem benutzerdefinierten Zertifikat für die HTTPS-Kommunikation bereitstellst.
ms.date: 01/08/2021
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.custom:
- 19H1
- contperf-fy21q3
ms.openlocfilehash: d1d26edbd2d36cda0d5eb33a700534b55e0c36d0
ms.sourcegitcommit: 49df4993ac6d2e66bfa629555e1831ed99bac93a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2021
ms.locfileid: "98058511"
---
# <a name="provision-windows-device-portal-with-a-custom-ssl-certificate"></a>Bereitstellen eines Windows-Geräteportals mit einem benutzerdefinierten SSL-Zertifikat

Im Windows 10 Creators Update wurde mit dem Windows-Geräteportal (WDP) eine Methode hinzugefügt, mit der Administratoren ein benutzerdefiniertes Zertifikat für die Verwendung bei der HTTPS-Kommunikation erstellen können.

Diese Funktion ist zwar auf deinem eigenen PC möglich, sie ist allerdings hauptsächlich für Unternehmen vorgesehen, die über eine Zertifikatinfrastruktur verfügen.  

Ein Unternehmen kann beispielsweise eine Zertifizierungsstelle (CA) für das Signieren von Zertifikaten für Intranetwebsites verwenden, die über HTTPS verarbeitet werden. Dieses Feature steht an der Spitze dieser Infrastruktur.

## <a name="overview"></a>Übersicht

Standardmäßig generiert das WDP eine selbstsignierte Stamm-CA und nutzt diese zum Signieren von SSL-Zertifikaten für jeden Endpunkt, den es überwacht. Dazu gehören `localhost`, `127.0.0.1` und `::1` (IPv6-Localhost).

Außerdem gehören dazu der Hostname des Geräts (z. B. `https://LivingRoomPC`) und jede verbindungslokale IP-Adresse, die dem Gerät zugewiesen ist (bis zu zwei (IPv4, IPv6) pro Netzwerkadapter).
Sie finden die verbindungslokalen IP-Adressen für ein Gerät anhand des Netzwerktools im WDP. Sie beginnen bei IPv4 mit `10.` oder `192.` und bei IPv6 mit `fe80:`.

In der Standardeinstellung wird im Browser möglicherweise eine Zertifikatwarnung aufgrund von nicht vertrauenswürdigen Stamm-CAs angezeigt. Genauer gesagt wird das vom WDP bereitgestellte SSL-Zertifikat von einer Stammzertifizierungsstelle signiert, die der Browser oder PC als nicht vertrauenswürdig einstuft. Dies kann durch Erstellen einer neuen vertrauenswürdigen Stamm-CA behoben werden.

## <a name="create-a-root-ca"></a>Erstellen einer Stamm-CA

Dies sollte nur ausgeführt werden, wenn dein Unternehmen (oder du zu Hause) keine Zertifikatinfrastruktur besitzt. Außerdem sollte der Vorgang sollte nur einmal durchgeführt werden. Das folgende PowerShell-Skript erstellt eine Stamm-CA namens _WdpTestCA.cer_. Das Installieren dieser Datei in der vertrauenswürdigen Stamm-CA des lokalen Computers bewirkt, dass dein Gerät SSL-Zertifikaten vertraut, die von dieser Stamm-CA signiert sind. Sie können (und sollten) diese CER-Datei auf jedem Computer installieren, den Sie mit dem WDP verbinden möchten.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Nach der Erstellung kannst du diese Datei _WdpTestCA.cer_ verwenden, um SSL-Zertifikate zu signieren.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Erstellen eines SSL-Zertifikats mit der Stamm-CA

SSL-Zertifikate haben zwei wichtige Funktionen: Schützen der Verbindung durch Verschlüsselung und Überprüfen, ob du tatsächlich mit der im Browser angezeigten Adresse (Bing.com, 192.168.1.37 usw.) und nicht mit einem böswilligen Drittanbieter kommunizierst.

Das folgende PowerShell-Skript erstellt ein SSL-Zertifikat für den `localhost`-Endpunkt. Jeder Endpunkt, an dem das WDP lauscht, benötigt ein eigenes Zertifikat. Ersetzen Sie das `$IssuedTo`-Argument im Skript mit allen Endpunkten für Ihr Gerät: Hostname, Localhost und IP-Adressen.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

Wenn du über mehrere Geräte verfügst, kannst du die PFX-Dateien von Localhost wiederverwenden, du musst aber weiterhin Zertifikate für die IP-Adresse und den Hostnamen für jedes Gerät separat erstellen.

Wenn das Paket von PFX-Dateien generiert wird, müssen Sie diese im WDP laden.

## <a name="provision-windows-device-portal-with-the-certifications"></a>Bereitstellen des Windows-Geräteportals mit Zertifikaten

Für jede PFX-Datei, die du für ein Gerät erstellt hast, musst du den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten ausführen.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Das folgende Beispiel zeigt die Verwendung:

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Nachdem du die Zertifikate installiert hast, startest du den Dienst einfach neu, damit die Änderungen wirksam werden:

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP-Adressen können sich im Lauf der Zeit ändern.
Viele Netzwerke verwenden DHCP, um IP-Adressen auszugeben. Aus diesem Grund erhalten Geräte nicht immer die gleiche IP-Adresse wie zuvor. Wenn Sie ein Zertifikat für eine IP-Adresse auf einem Gerät erstellt haben und die Adresse des Geräts geändert wurde, erstellt das WDP ein neues Zertifikat mit dem vorhandenen selbstsignierten Zertifikat und verwendet das von Ihnen erstellte Zertifikat nicht mehr. Dadurch wird die Seite mit der Zertifikatwarnung in deinem Browser erneut angezeigt. Aus diesem Grund wird empfohlen, die Verbindung mit Ihren Geräten über deren Hostnamen zu erstellen, den Sie im Windows-Geräteportal festlegen können. Diese bleiben unabhängig von der IP-Adresse unverändert.
