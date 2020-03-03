---
Description: Erfahren Sie, wie Sie einer nicht verpackten Desktop-App Identität zuweisen, damit Sie moderne Windows 10-Funktionen in diesen Apps verwenden können.
title: Identitätszuweisen für nicht verpackte Desktop-Apps
ms.date: 02/28/2020
ms.topic: article
keywords: Windows 10, Desktop, Package, Identity, msix, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ae05a00cac19fdd349aa48160b88cde6b84e26b0
ms.sourcegitcommit: 620e4a51e2486ec2cb7190176b3d9bf3d7b5b6af
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78222026"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Identitätszuweisen für nicht verpackte Desktop-Apps

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Viele Windows 10-Erweiterbarkeits Features erfordern eine [Paket Identität](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) , die von nicht-UWP-Desktop-Apps, einschließlich Hintergrundaufgaben, Benachrichtigungen, Live-Kacheln und Freigabe Zielen, verwendet werden kann. Für diese Szenarien erfordert das Betriebssystem eine Identität, damit es den Aufrufer der entsprechenden API identifizieren kann.

In Betriebssystemversionen vor Windows 10 Insider Preview Build 10.0.19000.0 besteht die einzige Möglichkeit zum Gewähren von Identitäten für eine Desktop-App darin, [Sie in einem signierten msix-Paket zu verpacken](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Für diese apps wird die Identität im Paket Manifest angegeben, und die Identitäts Registrierung wird von der msix-Bereitstellungs Pipeline basierend auf den Informationen im Manifest verarbeitet. Alle Inhalte, auf die im Paket Manifest verwiesen wird, sind im msix-Paket vorhanden.

Ab Windows 10 Insider Preview Build 10.0.19000.0 können Sie der Paket Identität Desktop-Apps, die nicht in einem msix-Paket verpackt sind, durch Erstellen und Registrieren eines *sparsepakets* in Ihrer APP gewähren. Diese Unterstützung ermöglicht Desktop-Apps, die noch keine msix-Paket Erstellung für die Bereitstellung durchführen können, um Windows 10-Erweiterbarkeits Funktionen zu verwenden, die eine Paket Identität erfordern. Weitere Hintergrundinformationen finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Gehen Sie folgendermaßen vor, um ein Paket mit geringer Dichte zu erstellen und zu registrieren, das Ihrer Desktop-App eine Paket Identität gewährt.

1. [Erstellen eines Paket Manifests für das sparsespaket](#create-a-package-manifest-for-the-sparse-package)
2. [Erstellen und Signieren des sparsepakets](#build-and-sign-the-sparse-package)
3. [Fügen Sie die Metadaten der Paket Identität Ihrem Desktop Anwendungs Manifest hinzu.](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Registrieren des sparsepakets zur Laufzeit](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Wichtige Konzepte

Mit den folgenden Funktionen können nicht gepackte Desktop-Apps die Paket Identität abrufen.

### <a name="sparse-packages"></a>Pakete mit geringer Dichte

Ein *Paket* mit geringer Dichte enthält ein Paket Manifest, aber keine anderen APP-Binärdateien und keinen Inhalt. Das Manifest eines sparsepakets kann auf Dateien außerhalb des Pakets an einem vordefinierten externen Speicherort verweisen. Dies ermöglicht Anwendungen, die noch keine msix-Paket Erstellung für Ihre gesamte App durchführen können, um die Paket Identität zu erhalten, wie dies für einige Windows 10-Erweiterbarkeits Features erforderlich ist.

> [!NOTE]
> Eine Desktop-App, die ein Paket mit geringer Dichte verwendet, hat keine Vorteile, dass Sie vollständig über ein msix-Paket bereitgestellt wird. Zu diesen Vorteilen gehören der Schutz von Manipulationen, die Installation an einem gesperrten Standort und die vollständige Verwaltung durch das Betriebssystem während der Bereitstellung, der Laufzeit und der Deinstallation.

### <a name="package-external-location"></a>Externer Speicherort des Pakets

Zur Unterstützung von Paketen mit geringer Dichte unterstützt das Paket Manifest-Schema nun ein optionales **\<\>** -Element unter dem [ **\<-Eigenschaften\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) Element. Dadurch kann das Paket Manifest auf Inhalte außerhalb des Pakets an einem bestimmten Speicherort auf dem Datenträger verweisen.

Wenn Sie z. b. über Ihre vorhandene, nicht gepackte Desktop-App verfügen, die die ausführbare APP und andere Inhalte in "c:\programme\mydesktopapp" installiert\, können Sie ein Paket mit geringer Dichte erstellen, das das **\<"Zuge\>** "-Element im Manifest enthält. Während des Installationsvorgangs für Ihre APP oder beim ersten Ausführen Ihrer Apps können Sie das Paket mit geringer Dichte installieren und c:\programme\mydesktopapp\ als externen Speicherort deklarieren, der von Ihrer APP verwendet wird.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Erstellen eines Paket Manifests für das sparsespaket

Bevor Sie ein Paket mit geringer Dichte erstellen können, müssen Sie zunächst ein [Paket Manifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (eine Datei mit dem Namen "appxmanifest. xml") erstellen, das die Metadaten für die Paket Identität für Ihre Desktop-App und andere erforderliche Details deklariert. Die einfachste Möglichkeit zum Erstellen eines Paket Manifests für das Paket mit geringer Dichte besteht darin, das folgende Beispiel zu verwenden und es für Ihre APP mit der [Schema Referenz](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)anzupassen.

Stellen Sie sicher, dass das Paket Manifest die folgenden Elemente enthält:

* Ein [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Element, das die Identitäts Attribute für Ihre Desktop-App beschreibt.
* Ein **\<\>** -Element unter dem [ **\<-Eigenschaften\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) Element. Diesem Element sollte der Wert `true`zugewiesen werden, mit dem das Paket Manifest auf Inhalte außerhalb des Pakets an einem bestimmten Speicherort auf dem Datenträger verweisen kann. In einem späteren Schritt geben Sie den Pfad für den externen Speicherort an, wenn Sie Ihr sparsepaket bei Code registrieren, der in Ihrem Installer oder in der app ausgeführt wird. Alle Inhalte, auf die Sie im Manifest verweisen, die sich nicht im Paket selbst befinden, sollten am externen Speicherort installiert werden.
* Das **MinVersion** -Attribut des [ **\<targetdevicefamily-\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) Elements sollte auf `10.0.19000.0` oder eine spätere Version festgelegt werden.
* Die Attribute **Trust Level = mediumil** und **runtimebehavior = Win32App** der [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) -Element deklarieren, dass die Desktop-App, die dem Sparse-Paket zugeordnet ist, ähnlich wie eine standardmäßige, nicht gepackte Desktop-app ausgeführt wird, ohne dass Registrierung und Datei Systemvirtualisierung und andere Lauf Zeit Änderungen vorgenommen werden.

Das folgende Beispiel zeigt den gesamten Inhalt eines Pakets mit geringer Dichte (appxmanifest. Xml). Dieses Manifest enthält eine `windows.sharetarget` Erweiterung, die eine Paket Identität erfordert.

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

## <a name="build-and-sign-the-sparse-package"></a>Erstellen und Signieren des sparsepakets

Nachdem Sie das Paket Manifest erstellt haben, erstellen Sie das Paket mit geringer Dichte, indem Sie das [Tool makeappx. exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) in der Windows SDK verwenden. Da das sparse-Paket nicht die Dateien enthält, auf die im Manifest verwiesen wird, müssen Sie die `/nv`-Option angeben, die die semantische Validierung für das Paket überspringt.

Im folgenden Beispiel wird veranschaulicht, wie ein Paket mit geringer Dichte von der Befehlszeile aus erstellt wird.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

Bevor das Paket mit geringer Dichte auf einem Bereitstellungs Zielcomputer installiert werden kann, müssen Sie es mit einem Zertifikat signieren, das auf dem Zielcomputer als vertrauenswürdig eingestuft wird. Sie können ein neues selbst signiertes Zertifikat zu Entwicklungszwecken erstellen und Ihr sparsepaket mithilfe von [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)signieren, das in der Windows SDK verfügbar ist.

Im folgenden Beispiel wird veranschaulicht, wie Sie ein Paket mit geringer Dichte von der Befehlszeile aus signieren.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Fügen Sie die Metadaten der Paket Identität Ihrem Desktop Anwendungs Manifest hinzu.

Sie müssen auch ein paralleles [Anwendungs Manifest](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) mit ihrer Desktop-App einschließen und ein [&lt;msix-&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) Element mit Attributen einschließen, die die Identitäts Attribute Ihrer APP deklarieren. Die Werte dieser Attribute werden vom Betriebssystem verwendet, um die Identität ihrer APP zu ermitteln, wenn die ausführbare Datei gestartet wird.

Das folgende Beispiel zeigt ein paralleles Anwendungs Manifest mit einem **\<msix\>** -Element.

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

Die Attribute des **\<msix-\>** -Elements müssen diese Werte im Paket Manifest für das sparsepaket erfüllen:

* Die Attribute " **PackageName** " und " **Publisher** " müssen mit den Attributen " **Name** " und " **Publisher** " in der [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Element im Paket Manifest identisch sein.
* Das **ApplicationId** -Attribut muss mit dem **ID** -Attribut des [ **\<Anwendungs\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) -Elements im Paket Manifest identisch sein.

Das parallele Anwendungs Manifest muss sich im selben Verzeichnis wie die ausführbare Datei für Ihre Desktop-App befinden, und gemäß der Konvention sollte Sie denselben Namen wie die ausführbare Datei Ihrer APP aufweisen, und der `.manifest` Erweiterung angefügt. Wenn beispielsweise der Name der ausführbaren Datei der APP `ContosoPhotoStore`ist, sollte der Dateiname des Anwendungs Manifests `ContosoPhotoStore.exe.manifest`sein.

## <a name="register-your-sparse-package-at-run-time"></a>Registrieren des sparsepakets zur Laufzeit

Um ihrer Desktop-App eine Paket Identität zu erteilen, muss Ihre APP das Paket mit geringer Dichte mithilfe der [packagemanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) -Klasse registrieren. Sie können Ihrer APP Code hinzufügen, um das Paket mit geringer Dichte zu registrieren, wenn Ihre APP zum ersten Mal ausgeführt wird, oder Sie können Code ausführen, um das Paket zu registrieren, während Ihre Desktop-App installiert ist (z. b. Wenn Sie die Desktop-App mithilfe von MSI installieren). können Sie diesen Code von einer benutzerdefinierten Aktion ausführen.)

Im folgenden Beispiel wird das Registrieren eines sparsepakets veranschaulicht. Dieser Code erstellt ein **addpackageoptions** -Objekt, das den Pfad zum externen Speicherort enthält, an dem das Paket Manifest auf Inhalte außerhalb des Pakets verweisen kann. Anschließend übergibt der Code dieses Objekt an die **packagemanager. addpackagebyuriasync** -Methode, um das sparsespaket zu registrieren. Diese Methode empfängt auch den Speicherort des Pakets mit geringer Dichte als URI. Ein ausführeres Beispiel finden Sie in der `StartUp.cs`-Codedatei [im zugehörigen Beispiel](#sample).

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

Eine voll funktionsfähige Beispiel-APP, die veranschaulicht, wie die Paket Identität einer Desktop-App mithilfe eines sparsepakets erteilt wird, finden Sie unter [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages). Weitere Informationen zum Aufbau und zur Ausführung des Beispiels finden Sie in [diesem Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Dieses Beispiel umfasst Folgendes:

* Der Quellcode für eine WPF-App mit dem Namen "photostoredemo". Während des Starts prüft die APP, ob Sie mit Identity ausgeführt wird. Wenn Sie nicht mit Identity ausgeführt wird, wird das Paket mit geringer Dichte registriert, und die APP wird dann neu gestartet. Den Code, der diese Schritte ausführt, finden Sie unter `StartUp.cs`.
* Ein paralleles Anwendungs Manifest mit dem Namen `PhotoStoreDemo.exe.manifest`.
* Ein Paket Manifest mit dem Namen `AppxManifest.xml`.
