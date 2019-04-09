---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Bereitstellen eines Geräteportals mit einem benutzerdefinierten SSL-Zertifikat
description: TBD
ms.date: 4/8/2019
ms.topic: article
keywords: Windows 10, Uwp, Device-portal
ms.localizationpriority: medium
ms.openlocfilehash: cbe813a58124b1cd80f352ae11e9dcff59b21da4
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244336"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Bereitstellen eines Geräteportals mit einem benutzerdefinierten SSL-Zertifikat

Im Windows 10 Creators Update fügte das Windows Device Portal eine Methode hinzu, wodurch Administratoren ein benutzerdefiniertes Zertifikat für die Verwendung in HTTPS-Kommunikation erstellen können.

Diese Funktion ist zwar auf Ihrem eigenen PC möglich, sie ist allerdings hauptsächlich für Unternehmen vorgesehen, die über eine Zertifikatinfrastruktur verfügen.  

Beispielsweise verwendet ein Unternehmen eine Zertifizierungsstelle (CA) für das Signieren von Zertifikaten für Intranetwebsites, die über HTTPS platziert werden. Dieses Feature steht an der Spitze der Infrastruktur.

## <a name="overview"></a>Übersicht

Standardmäßig generiert das Geräteportal eine selbstsignierte Stamm-CA, und nutzt dieses zum Signieren von SSL-Zertifikaten für jeden Endpunkt, den es überwacht. Dazu gehören `localhost`, `127.0.0.1`, und `::1` (IPv6 localhost).

Dazu gehören ebenfalls Hostname des Geräts (z. B. `https://LivingRoomPC`) und jede verbindungslokale IP-Adresse, die dem Gerät zugewiesen ist (bis zu zwei [IPv4, IPv6] pro Netzwerkadapter).
Sie können die verbindungslokale IP-Adressen für ein Gerät anhand des Netzwerk-Tools im Geräteportal finden. Die beginnen mit `10.` oder `192.` für IPv4 oder `fe80:` für IPv6.

In der Standardeinstellung wird möglicherweise eine Zertifikatwarnung in Ihrem Browser aufgrund von nicht vertrauenswürdigen Stamm-CAs angezeigt. Genauer gesagt wird das vom Geräteportal bereitgestellte SSL-Zertifikat von einer Stammzertifizierungsstelle signiert, die der Browser oder PC als nicht vertrauenswürdig einstuft. Dies kann durch Erstellen einer neuen vertrauenswürdigen Stammzertifizierungsstelle behoben werden.

## <a name="create-a-root-ca"></a>Erstellen der Stammzertifizierungsstelle

Dies sollte nur ausgeführt werden, wenn Ihr Unternehmen (oder zu Hause) keine Zertifikatinfrastruktur besitzt und der Vorgang sollte nur einmal durchgeführt werden. Das folgende PowerShell-Skript erstellt eine Stammzertifizierungsstelle, die _WdpTestCA.cer_ genannt wird. Das Installieren dieser Datei auf die vertrauenswürdige Stammzertifizierungsstelle des lokalen Computers bewirkt, dass Ihr Gerät SSL-Zertifikaten vertrauet, die von dieser Stammzertifizierungsstelle signiert sind. Sie können (und sollten) diese CER-Datei auf jedem PC installieren, die Sie mit dem Windows Device Portal verbinden möchten.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Nachdem sie erstellt wurde, können Sie diese _WdpTestCA.cer_-Datei verwenden, um SSL-Zertifikate zu signieren.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Erstellen Sie ein SSL-Zertifikat mit der Stammzertifizierungsstelle

SSL-Zertifikate haben zwei wichtige Funktionen: Sichern der Verbindungs durch Verschlüsselung und Überprüfung, dass Sie tatsächlich mit der im Browser angezeigten Adresse (Bing.com, 192.168.1.37 usw.) und keinem böswilligen Drittanbieter kommunizieren.

Das folgende PowerShell-Skript erstellt ein SSL-Zertifikat für den `localhost`-Endpunkt. Jeder Endpunkt, der vom Geräteportal überwacht wird, benötigt ein eigenes Zertifikat. Ersetzen Sie das `$IssuedTo`-Argument im Skript mit allen anderen Endpunkte für Ihr Gerät: Hostname, "localhost" und IP-Adressen.

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

Wenn Sie über mehrere Geräte verfügen, können Sie die PFX-Dateien von "localhost" wiederverwenden, aber Sie müssen weiterhin Zertifikate für die IP-Adresse und den Hostnamen für jedes Gerät separat erstellen.

Wenn das Bündel von PFX-Dateien generiert wird, müssen Sie diese in das Windows Device Portal laden.

## <a name="provision-device-portal-with-the-certifications"></a>Bereitstellen eines Geräteportals mit Zertifikat(en)

Für jede PFX-Datei, die Sie für ein Gerät erstellt haben, müssen Sie den folgenden Befehl von einer Eingabeaufforderung mit erhöhten Rechten ausführen.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Beispiel zur Verwendung weiter unten:

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Nachdem Sie die Zertifikate installiert haben, starten Sie den Dienst erneut, damit die Änderungen wirksam werden:

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP-Adressen können sich im Laufe der Zeit ändern.
Viele Netzwerke verwenden DHCP, um IP-Adressen zu erstellen, damit Geräte nicht immer die gleiche IP-Adresse erhalten, die sie zuvor verwendeten. Wenn Sie ein Zertifikat für eine IP-Adresse auf einem Gerät erstellt haben, und die Adresse des Geräts hat sich geändert, erstellt Windows Device Portal ein neues Zertifikat mit dem vorhandenen selbstsignierten Zertifikat, und verwendet das von Ihnen erstellte Zertifikat nicht mehr. Dadurch wird die Seite mit der Zertifikatswarnung in Ihrem Browser erneut angezeigt. Aus diesem Grund empfehlen wir, eine Verbindung mit Ihren Geräten über ihren Hostnamen zu erstellen, den Sie im Geräteportal festlegen können. Diese bleiben unabhängig von der IP-Adressen unverändert.
