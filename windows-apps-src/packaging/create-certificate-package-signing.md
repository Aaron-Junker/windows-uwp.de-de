---
title: Erstellen eines Paketsignaturzertifikats
description: PowerShell-Tools helfen bei der Erstellung und beim Export eines App-Paketsignaturzertifikats.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 1476410c96900eff7ba4b8d0ad34c9d7b5599434
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372735"
---
# <a name="create-a-certificate-for-package-signing"></a>Erstellen eines Paketsignaturzertifikats


In diesem Artikel wird die Erstellung und der Export eines App-Paketsignaturzertifikats mithilfe von PowerShell-Tools erläutert. Es wird empfohlen, Visual Studio zum [Verpacken von UWP-Apps](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps) zu verwenden. Sie können allerdings auch weiterhin App-Pakete für den Store manuell verpacken, wenn Sie Ihre App nicht mit Visual Studio erstellt haben.

> [!IMPORTANT] 
> Wenn Sie Visual Studio zum Entwickeln der App verwendet haben, wird empfohlen, dass Sie den Visual Studio-Assistenten zum Importieren eines Zertifikats und Signieren des App-Pakets verwenden. Weitere Informationen finden Sie unter [Verpacken einer UWP-App mit Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="prerequisites"></a>Vorraussetzungen

- **Eine App-Pakete oder entpackt-app**  
Eine App mit einer AppxManifest.xml-Datei. Sie müssen während der Erstellung des Zertifikats auf die Manifestdatei verweisen, die zum Signieren der endgültigen Version des App-Pakets verwendet wird. Details zum manuellen Verpacken von Apps finden Sie unter [Erstellen eines App-Pakets mit dem Tool „MakeAppx.exe“](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Public Key-Infrastruktur (PKI)-Cmdlets**  
Sie benötigen die PKI-Cmdlets zum Erstellen und Exportieren Ihres Signaturzertifikats. For more information, see [Public Key-Infrastruktur-Cmdlets](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats

Ein selbst signiertes Zertifikat eignet sich für Ihre app testen, bevor Sie sie in den Store veröffentlichen möchten. Führen Sie die Schritte in diesem Abschnitt, um ein selbstsigniertes Zertifikat zu erstellen.

> [!NOTE]
> Selbstsignierte Zertifikate dienen ausschließlich zu Testzwecken. Wenn Sie bereit zum Veröffentlichen Ihrer app auf den Store oder von anderen Veranstaltungsorte sind, wechseln Sie das Zertifikat zu seriösen Quelle fest. Dazu kann die Unfähigkeit, damit Ihre app von Ihren Kunden installiert führen.

### <a name="determine-the-subject-of-your-packaged-app"></a>Festlegen des Betreffs Ihres App-Pakets  

Zur Verwendung eines Zertifikats für die Signatur des App-Pakets **muss** der Betreff im Zertifikat dem Abschnitt „Herausgeber” des App-Manifests entsprechen.

Beispielsweise sollte der Abschnitt „Identität” in der „AppxManifest.xml”-Datei Ihrer App etwa wie folgt aussehen:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

Der „Herausgeber” ist in diesem Fall „CN=Contoso Software, O=Contoso Corporation, C=US", was zum Erstellen eines Zertifikats verwendet werden muss.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Verwenden von **New-SelfSignedCertificate** zum Erstellen eines Zertifikats

Verwenden Sie das PowerShell-Cmdlet **New-SelfSignedCertificate** zum Erstellen eines selbstsignierten Zertifikats. **New-SelfSignedCertificate** verfügt über mehrere anpassbare Parameter. Bei diesem Artikel konzentrieren wir uns allerdings auf das Erstellen eines einfachen Zertifikats, das mit **SignTool** funktioniert. Weitere Beispiele und Verwendungen dieses Cmdlets finden Sie unter [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

Basierend auf der „AppxManifest.xml”-Datei aus dem vorherigen Beispiel, sollten Sie folgende Syntax verwenden, um ein Zertifikat zu erstellen. In einer PowerShell-Eingabeaufforderung mit erhöhten Rechten:

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\LocalMachine\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Beachten Sie die folgenden Details zu einigen der Parameter ein:

- **KeyUsage**: Dieser Parameter definiert, was das Zertifikat verwendet werden kann. Für ein selbstsigniertes Zertifikat, dieser Parameter festgelegt werden soll, um **DigitalSignature**.

- **TextExtension**: Dieser Parameter enthält die Einstellungen für die folgenden Erweiterungen:

  - Erweiterte Schlüsselverwendung (EKU): Diese Erweiterung gibt an, weitere Zwecke, die für die der zertifizierte öffentliche Schlüssel verwendet werden kann. Für ein selbstsigniertes Zertifikat verwenden, sollte dieser Parameter die Erweiterungszeichenfolge enthalten **"2.5.29.37={text}1.3.6.1.5.5.7.3.3"** , was bedeutet, dass das Zertifikat ist zum Signieren von Code verwendet werden.

  - Grundlegende Einschränkungen: Diese Erweiterung gibt an, ob das Zertifikat einer Zertifizierungsstelle (Certificate Authority, CA) ist. Für ein selbstsigniertes Zertifikat verwenden, sollte dieser Parameter die Erweiterungszeichenfolge enthalten **"2.5.29.19={text}"** , was bedeutet, dass das Zertifikat eine Endentität (nicht von einer CA) ist.

Nach dem Ausführen dieses Befehls wird das Zertifikat dem lokalen Zertifikatspeicher hinzugefügt, wie im Parameter ‑CertStoreLocation angegeben. Das Ergebnis des Befehls wird den Fingerabdruck des Zertifikats auch erstellt werden.  

Sie können das Zertifikat mithilfe der folgenden Befehle in einem PowerShell-Fenster anzeigen:

```powershell
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Dies zeigt alle Zertifikate in Ihrem lokalen Speicher an.

## <a name="export-a-certificate"></a>Exportieren eines Zertifikats 

Verwenden Sie das **Export-PfxCertificate**-Cmdlet, um das Zertifikat im lokalen Speicher auf eine private Informationsaustausch-Datei (PFX) zu exportieren.

Sie müssen bei der Verwendung von **Export-PfxCertificate** entweder ein Kennwort erstellen und verwenden oder den Parameter „‑ProtectTo” verwenden, um anzugeben, welche Benutzer oder Gruppen ohne Kennwort auf die Datei zugreifen können. Es wird eine Fehlermeldung angezeigt, wenn Sie weder den „-Kennwort”-, noch den „-ProtectTo”-Parameter verwenden.

### <a name="password-usage"></a>Verwendung von Kennwörtern

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>Verwendung von „ProtectTo”

```powershell
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Nachdem Sie Ihr Zertifikat erstellt und exportiert haben, sind Sie zum Signieren des App-Pakets mit **SignTool** bereit. Informationen zum nächsten Schritt bei der manuellen Verpackung finden Sie unter [Signieren eines App-Pakets mithilfe von SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Sicherheitsüberlegungen

Das Hinzufügen eines Zertifikats zum [Zertifikatspeicher des lokalen Computers](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) wirkt sich auf die Zertifikatvertrauensstellung für alle Benutzer des Computers aus. Sie sollten die Zertifikate entfernen, wenn sie nicht mehr erforderlich sind, um zu verhindern, dass diese die Systemvertrauensstellung gefährden.
