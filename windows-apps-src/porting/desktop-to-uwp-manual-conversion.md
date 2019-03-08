---
Description: Zeigt, wie Sie eine Windows-Desktopanwendung (z. B. Win32, WPF und Windows Forms) für Windows 10 manuell verpacken.
Search.Product: eADQiWindows 10XVcnh
title: Manuelles Packen einer Anwendung (Desktop-Brücke)
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1dd159b7cd04a7641bf3f89605e054a00a0bad58
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651175"
---
# <a name="package-a-desktop-application-manually"></a>Packen Sie manuell eine desktop-Anwendung

In diesem Thema erfahren Sie, wie Sie Ihre Anwendung zu packen, ohne Verwendung von Tools wie Visual Studio oder den Desktop App Converter (DAC).

Um Ihre App manuell zu verpacken, erstellen Sie eine Paketmanifestdatei, und führen Sie dann ein Befehlszeilentool aus, um ein Windows-App-Paket zu generieren.

Betrachten Sie die manuelle Verpacken, wenn Sie die Anwendung mithilfe des Befehls Xcopy installieren oder Sie kennen sich mit den Änderungen, die Ihrer app-Installer an das System sendet, und möchten Sie mehr Kontrolle über den Prozess.

Wenn Sie sich nicht darüber sicher sind, welche Änderungen an das System durch Ihren Installer vorgenommen werden oder wenn Sie lieber automatisierte Tools für das Generieren Ihres Paketmanifestes verwenden möchten, sollten Sie eine [dieser](desktop-to-uwp-root.md#convert) Optionen erwägen.

>[!IMPORTANT]
>Die Fähigkeit zum Erstellen einer Windows-app-Paket für die desktop-Anwendung (auch bekannt als die Desktop-Brücke) wurde in Windows 10 Version 1607 eingeführt, und es kann nur in Projekten, die Windows 10 Anniversary Update (10.0; als Ziel verwendet werden Build 14393) oder eine neuere Version in Visual Studio.

## <a name="first-prepare-your-application"></a>Vorbereiten Ihrer Anwendung

Überprüfen Sie dieses Handbuch, bevor Sie beginnen, erstellen ein Paket für Ihre Anwendung: [Packen eine desktop-Anwendung vorbereiten](desktop-to-uwp-prepare.md).

## <a name="create-a-package-manifest"></a>Erstellen eines Paketmanifests

Erstellen Sie eine Datei, nennen Sie sie **appxmanifest.xml**, und fügen Sie diese XML-Datei hinzu.

Es ist eine einfache Vorlage, die die vom Paket benötigten Elemente und Attribute enthält. Wir werden im nächsten Abschnitt die Werte hinzufügen.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Füllen Sie die Paketelemente in der Datei aus.

Geben Sie in diese Vorlage Informationen ein, die das Paket beschreiben.

### <a name="identity-information"></a>Identitätsinformationen

Hier ist ein Beispiel für ein **Identitäts**-Element mit Platzhaltertext für die Attribute. Sie können das ``ProcessorArchitecture``-Attributs auf ``x64``oder ``x86`` festlegen.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Wenn Sie den Anwendungsnamen in der Microsoft Store reserviert haben, erhalten Sie den Namen und Herausgeber mit [Partner Center](https://partner.microsoft.com/dashboard). Wenn Sie, Ihrer Anwendung auf anderen Systemen querladen möchten beabsichtigen, können Sie Ihre eigenen Namen für diese bereitstellen, solange der Name des Verlegers, den Sie auswählen, mit dem Namen des Zertifikats übereinstimmt, die Sie zum Signieren von Apps verwenden.

### <a name="properties"></a>Eigenschaften

Das [Eigenschaften](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties)-Element hat 3 erforderliche untergeordnete Elemente. Hier ist ein Beispiel für einen **Eigenschaften**-Knoten mit Platzhaltertext für die Elemente. Die **"DisplayName"** ist der Name der Anwendung, die Sie in den Store, für apps reservieren, die auf den Store hochgeladen werden.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Ressourcen

Hier ist ein Beispiel für einen [Ressourcen](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources)-Knoten.

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Abhängigkeiten

Für desktop-apps, die Sie ein Paket erstellen, legen Sie immer die ``Name`` Attribut ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Funktionen
Für desktop-apps, die Sie ein Paket erstellen, für die Sie hinzufügen müssen die ``runFullTrust`` Funktion.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Ausfüllen der Elemente auf Anwendungsebene

Geben Sie in diese Vorlage Informationen ein, die Ihre App beschreiben.

### <a name="application-element"></a>Anwendungselemente

Für desktop-apps, die Sie ein Paket, erstellen die ``EntryPoint`` Attributs des Anwendungselements ist immer ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Visuelle Elemente

Hier ist ein Beispiel für einen [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)-Knoten.

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(Optional) Zielbasierte Ressourcen ohne Anpassung hinzufügen

Zielbasierte Ressourcen gelten für Symbole und Kacheln, die in der Windows-Taskleiste, in der Aufgabenansicht, über ALT+TAB, in der Andockhilfe und in der unteren rechten Ecke von Startkacheln angezeigt werden. Erhalten Sie [hier](https://docs.microsoft.com/windows/uwp/design/style/app-icons-and-logos#unplated-assets) weitere Informationen.

1. Rufen Sie die richtigen 44x44-Bilder ab, und kopieren Sie sie dann in den Ordner, der Ihre Bilder (d. h. Ressourcen) enthält.

2. Erstellen Sie für jedes 44x44-Bild eine Kopie im selben Ordner, und hängen Sie **.targetsize-44_altform-unplated** an den Dateinamen an. Sie sollten zwei Kopien von jedem Symbol haben, jeweils spezifisch benannt. Nach Abschluss des Prozesses könnte Ihr Ressourcen-Ordner beispielsweise **MYAPP_44x44.png** und **MYAPP_44x44.targetsize-44_altform-unplated.png** enthalten.

   > [!NOTE]
   > In diesem Beispiel ist das Symbol mit dem Namen **MYAPP_44x44.png** das Symbol, auf das Sie im ``Square44x44Logo`` -Logo-Attribut des Windows-App-Pakets verweisen.

3.  Legen Sie im Windows-App-Paket die ``BackgroundColor`` für jedes Symbol, das Sie transparent machen, fest.

4. Fahren Sie mit dem nächste Unterabschnitt fort, um eine neue Paketressourcendateien zu generieren.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Paketressourcendateien (Package Resource Index, PRI) generieren

Wenn Sie Ziel-basierte Ressourcen erstellen, wie im vorherigen Abschnitt beschrieben, oder Sie die visuellen Ressourcen Ihrer Anwendung ändern nach der Erstellung des Pakets, müssen Sie eine neue PRI-Datei zu generieren.

1.  Öffnen Sie eine **Developer-Eingabeaufforderung für VS 2017**.

    ![Developer-Eingabeaufforderung](images/desktop-to-uwp/developer-command-prompt.png)

2.  Ändern Sie das Verzeichnis in den Stammordner des Pakets, und erstellen Sie dann mit dem Befehl ``makepri createconfig /cf priconfig.xml /dq en-US`` eine priconfig.xml-Datei.

5.  Erstellen Sie die resources.pri-Dateien mit dem Befehl ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Der Befehl für Ihre Anwendung könnte beispielsweise folgendermaßen aussehen: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Verpacken Sie Ihre Windows-App-Datei mithilfe der Anweisungen im nächsten Schritt.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Erstellen eines Windows-App-Pakets

Verwenden Sie **MakeAppx.exe**, um ein Windows-App-Paket für Ihr Projekt zu generieren. Ist es mit im Windows 10 SDK enthalten, und wenn Sie Visual Studio installiert haben, können Sie ganz einfach über Developer-Eingabeaufforderung für Visual Studio zugreifen.

Weitere Informationen finden Sie in [Erstellen eines App-Pakets mit dem Tool „MakeAppx.exe“](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>Ausführung der verpackten App

Sie können Ihre Anwendung lokal, ohne dass ein Zertifikat zu erhalten, und signieren Sie es testen ausführen. Führen Sie einfach dieses PowerShell-Cmdlet aus:

```Add-AppxPackage –Register AppxManifest.xml```

Ersetzen Sie zum Aktualisieren der EXE- oder DLL-Dateien Ihrer App die vorhandenen Dateien in Ihrem Paket durch die neuen, vergrößern Sie die Versionsnummer in der Datei „AppxManifest.xml“, und führen Sie den oben genannten Befehl erneut aus.

> [!NOTE]
> Eine gepackte Anwendung immer ausgeführt wird, als interaktiver Benutzer aus, und alle Festplatten, die Sie bei die gepackte Anwendung installieren muss NTFS-Format formatiert sein.

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Sie können [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) Fragen dazu stellen.

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Code schrittweise durchlaufen / suchen und Beheben von Problemen**

Finden Sie unter [ausführen, Debuggen und testen eine App-Pakete desktop-Anwendung](desktop-to-uwp-debug.md)

**Signieren Sie Ihrer Anwendung und dann verteilen**

Finden Sie unter [verteilen eine App-Pakete desktop-Anwendung](desktop-to-uwp-distribute.md)
