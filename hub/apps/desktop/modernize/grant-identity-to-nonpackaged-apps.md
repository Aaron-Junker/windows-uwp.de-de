---
Description: Erfahre, wie du einer nicht verpackten Desktop-App Identität zuweist, damit du moderne Windows 10-Funktionen in diesen Apps verwenden kannst.
title: Identitätszuweisen für nicht verpackte Desktop-Apps
ms.date: 02/28/2020
ms.topic: article
keywords: Windows 10, Desktop, Paket, Identität, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6b77cc7b2f39a987df4c832f7a8daeb7e2722def
ms.sourcegitcommit: f2f61a43f5bc24b829e8db679ffaca3e663c00e9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80588708"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Identitätszuweisen für nicht verpackte Desktop-Apps

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Viele Windows 10-Erweiterbarkeitsfeatures erfordern, dass [Paketidentität](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) von Nicht-UWP-Desktop-Apps verwendet wird, einschließlich Hintergrundaufgaben, Benachrichtigungen, Live-Kacheln und Freigabezielen. Für diese Szenarien erfordert das Betriebssystem eine Identität, damit es den Aufrufer der entsprechenden API identifizieren kann.

In Betriebssystemversionen vor Windows 10 Insider Preview Build 10.0.19000.0 besteht die einzige Möglichkeit, einer Desktop-App eine Identität zu gewähren, darin, [sie in einem signierten MSIX-Paket zu verpacken](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Für diese Apps wird die Identität im Paketmanifest angegeben, und die Identitätsregistrierung wird von der MSIX-Bereitstellungspipeline auf den Informationen im Manifest basierend behandelt. Alle Inhalte, auf die im Paketmanifest verwiesen wird, sind im MSIX-Paket vorhanden.

Ab Windows 10 Insider Preview Build 10.0.19000.0 kannst du Desktop-Apps, die nicht in einem MSIX-Paket verpackt sind, durch Erstellen und Registrieren eines *Sparse Package* mit deiner App eine Paketidentität gewähren. Diese Unterstützung ermöglicht Desktop-Apps, die noch nicht für die MSIX-Paketerstellung zur Bereitstellung geeignet sind, Windows 10-Erweiterbarkeitsfeatures zu verwenden, die eine Paketidentität erfordern. Weitere Hintergrundinformationen findest du in [diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Befolge diese Schritte, um ein Sparse Package zu erstellen und zu registrieren, das deiner Desktop-App eine Paketidentität gewährt.

1. [Erstellen eines Paketmanifests für das Sparse Package](#create-a-package-manifest-for-the-sparse-package)
2. [Erstellen und Signieren des Sparse Package](#build-and-sign-the-sparse-package)
3. [Hinzufügen der Paketidentität-Metadaten zu deinem Desktopanwendungsmanifest](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Registrieren des Sparse Package zur Runtime](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Wichtige Konzepte

Die folgenden Features ermöglichen nicht gepackten Desktop-Apps, eine Paketidentität erwerben.

### <a name="sparse-packages"></a>Sparse Packages

Ein *Sparse Package* enthält ein Paketmanifest, aber keine anderen App-Binärdateien und -Inhalte. Das Manifest eines Sparse Package kann auf Dateien außerhalb des Pakets an einem vordefinierten externen Speicherort verweisen. Dies ermöglicht Anwendungen, die noch keine MSIX-Paketerstellung für Ihre gesamte App durchführen können, eine Paketidentität zu erhalten, die für einige Windows 10-Erweiterbarkeitsfeatures erforderlich ist.

> [!NOTE]
> Einige Vorteile der vollständigen Bereitstellung über ein MSIX-Paket bleiben einer Desktop-App vorenthalten, die ein Sparse Package verwendet. Zu diesen Vorteilen gehören der Manipulationsschutz, die Installation an einem gesperrten Speicherort und die vollständige Verwaltung durch das Betriebssystem während Bereitstellung, Runtime und Deinstallation.

### <a name="package-external-location"></a>Speicherort außerhalb des Pakets

Zur Unterstützung von Sparse Packages unterstützt das Paketmanifestschema nun ein optionales **\<AllowExternalContent\>** -Element unter dem [ **\<Properties\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)-Element. So kann das Paketmanifest auf Inhalte außerhalb des Pakets an einem bestimmten Speicherort auf dem Datenträger verweisen.

Wenn z. B. deine vorhandene nicht gepackte Desktop-App die ausführbare Datei und andere Inhalte in „C:\Programme\MyDesktopApp“ installiert\, kannst du ein Sparse Package erstellen, das das **\<AllowExternalContent\>** -Element in das Manifest einbezieht. Während des Installationsvorgangs für deine App oder beim ersten Ausführen deiner Apps kannst du das Sparse Package installieren und „C:\Programme\MyDesktopApp“ als externen Speicherort deklarieren, der von deiner App verwendet wird.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Erstellen eines Paketmanifests für das Sparse Package

Bevor du ein Sparse Package erstellen kannst, musst du zunächst ein [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) erstellen (eine Datei mit dem Namen AppxManifest.xml), die Paketidentität-Metadaten für deine Desktop-App und andere erforderliche Details deklariert. Am einfachsten erstellst du ein Paketmanifest für das Sparse Package, indem du das folgende Beispiel verwendest und deiner App gemäß anpasst, indem du den [Schemaverweis](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) verwendest.

Stelle sicher, dass das Paketmanifest die folgenden Elemente enthält:

* Ein [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)-Element, das die Identitätsattribute für deine Desktop-App beschreibt.
* Ein **\<AllowExternalContent\>** -Element unter dem [**Properties\<\>** -Element ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). Diesem Element sollte der Wert `true` zugewiesen werden, sodass das Paketmanifest auf Inhalte außerhalb des Pakets an einem bestimmten Speicherort auf dem Datenträger verweisen kann. In einem späteren Schritt gibst du den Pfad zu dem externen Speicherort an, wenn du dein Sparse Package über Code registrierst, der im Installer oder in der App ausgeführt wird. Alle nicht im Paket selbst befindlichen Inhalte, auf die du im Manifest verweist, sollten am externen Speicherort installiert werden.
* Das **MinVersion**-Attribut des [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)-Elements sollte auf `10.0.19000.0` oder eine spätere Version festgelegt werden.
* Die Attribute **TrustLevel=mediumIL** und **RuntimeBehavior=Win32App** des [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)-Elements deklarieren, dass die Desktop-App, die dem Sparse Package zugeordnet ist, ähnlich wie eine standardmäßige, nicht gepackte Desktop-App ausgeführt wird, ohne Registrierung und Dateisystemvirtualisierung und andere Runtimeänderungen.

Das folgende Beispiel zeigt den gesamten Inhalt eines Sparse Package (AppxManifest.xml). Dieses Manifest enthält eine `windows.sharetarget`-Erweiterung, die eine Paketidentität erfordert.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>Erstellen und Signieren des Sparse Package

Nachdem du das Paketmanifest erstellt hast, erstelle das Sparse Package mithilfe des [MakeAppx.exe-Tools](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) im Windows SDK. Da das Sparse Package nicht die Dateien enthält, auf die im Manifest verwiesen wird, musst du die `/nv`-Option angeben, die die semantische Validierung für das Paket überspringt.

Das folgende Beispiel zeigt das Erstellen eines Sparse Package über die Befehlszeile.  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

Bevor das Sparse Package auf einem Zielcomputer installiert werden kann, musst du es mit einem Zertifikat signieren, das auf dem Zielcomputer als vertrauenswürdig eingestuft wird. Du kannst ein neues selbst signiertes Zertifikat zur Bereitstellung erstellen und dein Sparse Package mit dem [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool) signieren, das in der Windows SDK verfügbar ist.

Das folgende Beispiel zeigt das Signieren eines Sparse Package über die Befehlszeile.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Hinzufügen der Paketidentität-Metadaten zu deinem Desktopanwendungsmanifest

Du musst auch ein [paralleles Anwendungsmanifest](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) in deine Desktop-App einschließen sowie ein [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix)-Element mit Attributen, die die Identitätsattribute deiner App deklarieren. Die Werte dieser Attribute werden vom Betriebssystem verwendet, um die Identität deiner App zu ermitteln, wenn die ausführbare Datei gestartet wird.

Das folgende Beispiel zeigt ein paralleles Anwendungsmanifest mit einem **\<msix\>** -Element.

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

Die Attribute des **\<msix\>** -Elements müssen mit diesen Werten im Paketmanifest für das Sparse Package identisch sein:

* Die Attribute **packageName** und **publisher** müssen mit den Attributen **Name** und **Publisher** im [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)-Element deines Paketmanifests identisch sein.
* Das **applicationId**-Attribut muss mit dem **Id**-Attribut des [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)-Elements im Paketmanifest identisch sein.

Das parallele Anwendungsmanifest muss sich im selben Verzeichnis wie die ausführbare Datei für deine Desktop-App befinden und gemäß der Konvention sollte es denselben Namen wie die ausführbare Datei deiner App mit angefügter Erweiterung `.manifest` haben. Wenn beispielsweise der Name der ausführbaren Datei der App `ContosoPhotoStore` ist, sollte der Dateiname des Anwendungsmanifests `ContosoPhotoStore.exe.manifest` sein.

## <a name="register-your-sparse-package-at-run-time"></a>Registrieren des Sparse Package zur Runtime

Um deiner Desktop-App eine Paketidentität zu gewähren, muss deine App das Sparse Package mithilfe der [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)-Klasse registrieren. Du kannst deiner App Code hinzufügen, um das Sparse Package zu registrieren, wenn sie zum ersten Mal ausgeführt wird, oder du kannst Code ausführen, um das Paket zu registrieren, während die Desktop-App installiert wird (wenn du z. B. MSI zum Installieren der Desktop-App verwendest, kannst du diesen Code über eine benutzerdefinierte Aktion ausführen).

Im folgenden Beispiel wird veranschaulicht, wie das Sparse Package registriert wird. Dieser Code erstellt ein **AddPackageOptions**-Objekt, das den Pfad zum externen Speicherort enthält, an dem das Paketmanifest auf Inhalte außerhalb des Pakets verweisen kann. Anschließend übergibt der Code dieses Objekt an die **PackageManager.AddPackageByUriAsync**-Methode, um das Sparse Package zu registrieren. Diese Methode empfängt auch den Speicherort des signierten Sparse Package als URI. Ein ausführlicheres Beispiel findest du in der `StartUp.cs`-Codedatei im entsprechenden [Beispiel](#sample).

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>Beispiel

Eine voll funktionsfähige Beispiel-App, die veranschaulicht, wie einer Desktop-App die Paketidentität mithilfe eines Sparse Package gewährt wird, findest du unter [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages). Weitere Informationen zum Erstellen und Ausführen des Beispiels findest du in [diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Das Beispiel enthält Folgendes:

* Den Quellcode für eine WPF-App mit dem Namen PhotoStoreDemo. Während des Starts prüft die App, ob sie mit Identität ausgeführt wird. Wenn sie nicht mit Identität ausgeführt wird, registriert sie das Sparse Package, und die App wird dann neu gestartet. Den Code, der diese Schritte ausführt, findest du unter `StartUp.cs`.
* Ein paralleles Anwendungsmanifest mit dem Namen `PhotoStoreDemo.exe.manifest`.
* Ein Paketmanifest mit dem Namen `AppxManifest.xml`.
