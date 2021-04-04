---
title: WinUI 3 Project Reunion 0.5 (März 2021)
description: Übersicht über WinUI 3 Project Reunion 0.5.
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 92437934b7a07c6409d5d44325094f8dd024f443
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730464"
---
# <a name="windows-ui-library-3---project-reunion-05-march-2021"></a>Windows-UI-Bibliothek 3 – Project Reunion 0.5 (März 2021)

Windows-UI-Bibliothek (WinUI) 3 ist eine native Benutzererfahrungs-Framework (User Experience, UX) zur Erstellung moderner Windows-Apps.  Es wird unabhängig vom Windows-Betriebssystem als Teil von [Project Reunion](../../project-reunion/index.md) ausgeliefert.  Das Project Reunion 0.5-Release stellt [Visual Studio-Projektvorlagen](https://aka.ms/projectreunion/vsixdownload) bereit, die Sie beim Erstellen von Apps mit einer WinUI 3-basierten Benutzeroberfläche unterstützen.

**WinUI 3 Project Reunion 0.5** ist die erste stabile, unterstützte Version von WinUI 3, die zum Erstellen von Produktions-Apps verwendet werden kann, die im Microsoft Store veröffentlicht werden können. Diese Version umfasst Stabilitätsupdates und allgemeine Verbesserungen, die es ermöglichen, dass WinUI 3 aufwärtskompatibel ist und für die Produktion eingesetzt werden kann.


## <a name="install-winui-3---project-reunion-05"></a>Installieren von WinUI 3 – Project Reunion 0.5

Diese neue Version von WinUI 3 ist als Teil von Project Reunion 0.5 verfügbar. Weitere Informationen zur Installation finden Sie unter:

**[Installationsanweisungen für Project Reunion 0.5](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)**

Da WinUI nun als Teil von Project Reunion ausgeliefert wird, laden Sie für den Einstieg die Visual Studio-Erweiterung (VSIX) für Project Reunion herunter, die eine Reihe von Entwicklertools und -komponenten enthält. Weitere Informationen zum Project Reunion-Paket finden Sie unter [Bereitstellen von Apps, die Project Reunion verwenden](../../project-reunion/deploy-apps-that-use-project-reunion.md). Die Project Reunion-VSIX enthält [WinUI-Projektvorlagen](winui-project-templates-in-visual-studio.md), die Sie zum Erstellen Ihrer WinUI 3-App verwenden. 

> [!NOTE]
> Um die WinUI 3-Steuerelemente und -Features in Aktion zu sehen, können Sie die WinUI 3-Version des [XAML-Steuerelementekatalogs](#winui-3-controls-gallery) von GitHub klonen und erstellen.

> [!NOTE]
> Um die WinUI 3-Tools wie Live Visual Tree, Hot Reload und Live Property Explorer verwenden zu können, müssen Sie die WinUI 3-Tools mit den Visual Studio Preview-Features, wie [hier in der Anleitung](https://github.com/microsoft/microsoft-ui-xaml/issues/4140) beschrieben.

Nachdem Sie Ihre Entwicklungsumgebung eingerichtet haben, machen Sie sich unter [WinUI 3-Projektvorlagen in Visual Studio](winui-project-templates-in-visual-studio.md) mit den verfügbaren Vorlagen für Visual Studio-Projekte und -Elemente vertraut. 

Weitere Informationen zu den ersten Schritten beim Erstellen einer WinUI 3-App finden Sie in den folgenden Artikeln:

- [Erste Schritte mit WinUI 3 für Desktop-Apps](get-started-winui3-for-desktop.md)
- [Erstellen einer einfachen WinUI 3-App für den Desktop](desktop-build-basic-winui3-app.md)

Abgesehen von den [Einschränkungen und bekannten Problemen](#limitations-and-known-issues) ist das Erstellen einer App mithilfe der WinUI-Projekte vergleichbar mit dem Erstellen einer UWP-App mit XAML und WinUI 2.x. Daher lassen sich die meisten [Anleitungsdokumentationen](/windows/uwp/design/) für UWP-Apps und die **Windows.UI** WinRT-Namespaces im Windows SDK anwenden.

Die WinUI 3-API-Referenzdokumentation ist hier verfügbar: [WinUI 3 API-Referenz](/windows/winui/api)

### <a name="webview2"></a>Webansicht2

Um WebView2 mit diesem WinUI 3-Release zu verwenden, laden Sie Evergreen Bootstrapper oder Evergreen Standalone Installer herunter. Sofern Sie die WebView2-Runtime noch nicht installiert haben, finden Sie diese Installationsprogramme [hier](https://developer.microsoft.com/microsoft-edge/webview2/). 

### <a name="windows-community-toolkit"></a>Windows-Community-Toolkit

Wenn Sie das Windows Community Toolkit verwenden, [laden Sie die neueste Version herunter](https://aka.ms/wct-winui3).

### <a name="visual-studio-support"></a>Visual Studio-Unterstützung

Um die neuesten, in WinUI 3 hinzugefügten Toolfeatures wie Hot Reload, Live Visual Tree und Live Property Explorer nutzen zu können, müssen Sie die neueste Vorschauversion von Visual Studio verwenden und sicherstellen, dass die WinUI-Tools in den Visual Studio Preview-Funktionen aktiviert sind, wie [hier in der Anleitung](https://github.com/microsoft/microsoft-ui-xaml/issues/4140) beschrieben. Die folgende Tabelle zeigt die Kompatibilität von Visual Studio 2019-Versionen mit WinUI 3 – Project Reunion 0.5:

| VS-Version  | WinUI 3 – Project Reunion 0.5  |
|---|---|
| 16.8  | Nein   |
| 16,9  | Ja, aber ohne Hot Reload, Live Visual Tree oder Live Property Explorer  |
| 16.10 Vorschauen  | Ja, mit allen WinUI 3-Tools   |

## <a name="updating-your-existing-winui-3-app"></a>Aktualisieren Ihrer vorhandenen WinUI 3-App

Sie können eine App, die eine Vorschauversion von WinUI 3 verwendet hat, so aktualisieren, dass sie diese neue unterstützte Version von WinUI 3 verwendet. Anweisungen finden Sie unten, basierend auf Ihrem App-Typ.

> [!NOTE] 
> Aufgrund der Einzigartigkeit jedes App-Szenarios kann es bei Verwendung dieser Anweisungen zu Problemen kommen. Bitte befolgen Sie die Anweisungen sorgfältig, und wenn Sie Probleme finden, [melden Sie einen Fehler in unserem GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose). 

### <a name="updating-a-winui-3-preview-4-app-to-use-winui-3---project-reunion-05"></a>Aktualisieren einer WinUI 3 Preview 4-App zur Verwendung von WinUI 3 – Project Reunion 0.5

Bevor Sie beginnen, stellen Sie sicher, dass alle erforderlichen Komponenten für WinUI 3 – Project Reunion 0.5 installiert sind, einschließlich der Project Reunion-VSIX und des NuGet-Pakets. Die Installationsanweisungen finden Sie [hier](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment).

Führen Sie zunächst unbedingt die folgenden Schritte durch: 
- Wenn Ihre TargetPlatformMinVersion in der WAPPROJ-Datei älter als 10.0.17763.0 ist, aktualisieren Sie sie auf 10.0.17763.0 

- Wenn Ihre App das `Application.Suspending`-Ereignis verwendet, müssen Sie diese Zeile entfernen oder ändern, da `Application.Suspending` für Desktop-Apps nicht mehr aufgerufen wird. Weitere Informationen finden Sie in der [API-Referenzdokumentation](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true).

  Beachten Sie, dass die Standardprojektvorlagen für C++-und C#-Apps die folgenden Zeilen enthielten. Entfernen Sie diese Zeilen unbedingt, wenn Sie im Code noch vorhanden sind:

    C#: `this.Suspending += OnSuspending;`
  
    C++: `Suspending({ this, &App::OnSuspending });` 

Nehmen Sie nun einige Änderungen am Projekt vor: 
  
1. Navigieren Sie in Visual Studio zu **Extras** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
2. Geben Sie ```uninstall-package Microsoft.WinUI -ProjectName {yourProject}``` ein
3. Geben Sie ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}``` ein
4. Nehmen Sie die folgenden Änderungen in Ihrer WAPPROJ-Datei der Anwendung (Paket) vor:

    Fügen Sie diesen Abschnitt hinzu:

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

    Entfernen Sie dann die folgenden Zeilen:

    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(AppxTargetsLocation)Microsoft.WinUI.AppX.targets" />
    ```

5. Löschen Sie die vorhandene `Microsoft.WinUI.AppX.targets`-Datei im Ordner „{IhrProjekt} (package)/build/“ Ihres Projekts.

### <a name="updating-a-winui-3---project-reunion-05-preview-app-to-use-winui-3---project-reunion-05-stable"></a>Aktualisieren einer WinUI 3 – Project Reunion 0.5 Preview-App zur Verwendung von WinUI 3 – Project Reunion 0.5 (Stable)

Bevor Sie beginnen, stellen Sie sicher, dass alle erforderlichen Komponenten für WinUI 3 – Project Reunion 0.5 installiert sind, einschließlich der Project Reunion-VSIX und des NuGet-Pakets. Die Installationsanweisungen finden Sie [hier](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment).

Führen Sie zunächst unbedingt die folgenden Schritte durch:
- Wenn Ihre TargetPlatformMinVersion in der WAPPROJ-Datei älter als 10.0.17763.0 ist, aktualisieren Sie sie auf 10.0.17763.0 

- Wenn Ihre App das `Application.Suspending`-Ereignis verwendet, müssen Sie diese Zeile entfernen oder ändern, da `Application.Suspending` für Desktop-Apps nicht mehr aufgerufen wird. Weitere Informationen finden Sie in der [API-Referenzdokumentation](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true).

  Beachten Sie, dass die Standardprojektvorlagen für C++-und C#-Apps die folgenden Zeilen enthielten. Entfernen Sie diese Zeilen unbedingt, wenn Sie im Code noch vorhanden sind:

    C#: `this.Suspending += OnSuspending;`
  
    C++: `Suspending({ this, &App::OnSuspending });` 

Nehmen Sie nun einige Änderungen am Projekt vor: 

1. Navigieren Sie in Visual Studio zu **Extras** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
2. Geben Sie ```uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}``` ein
3. Geben Sie ```uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}``` ein
4. Geben Sie ```uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}``` ein
5. Geben Sie ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}``` ein
6. Nehmen Sie die folgenden Änderungen in Ihrer WAPPROJ-Datei der Anwendung (Paket) vor:
  
    Fügen Sie diesen Abschnitt hinzu:

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```
    Entfernen Sie dann die folgenden Zeilen (sofern vorhanden):
    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
    <Import Project="$(Microsoft_WinUI_AppX_targets)" />
    ```

    Und entfernen Sie diese Elementgruppe:

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
      <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

5. Löschen Sie die vorhandene `Microsoft.WinUI.AppX.targets`-Datei im Ordner „{IhrProjekt} (package)/build/“ Ihres Projekts.

## <a name="major-changes-introduced-in-this-release"></a>Wichtige Änderungen in diesem Release

### <a name="stable-features"></a>Stabile Features

Dieses Release bietet die erforderliche Stabilität und Unterstützung damit WinUI 3 für Produktions-Apps geeignet ist, die an die Microsoft Store ausgeliefert werden können. Es umfasst Unterstützung und Aufwärtskompatibilität für die meisten Features, die in früheren Vorschauversionen eingeführt wurden:

- Möglichkeit zum Erstellen von Desktop-Apps mit WinUI, einschließlich [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) für Win32-Apps
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView-Aktualisierungen](/windows/uwp/design/controls-and-patterns/tab-view)
- Aktualisierungen am dunklen Design
- Verbesserungen und Aktualisierungen für [WebView2](/microsoft-edge/hosting/webview2)
  - Unterstützung hoher DPI-Werte
  - Unterstützung des Änderns der Größe und Verschiebens von Fenstern
  - Für eine neuere Version von Microsoft Edge aktualisiert
  - Nicht mehr erforderlich, um auf ein WebView2-spezifisches NuGet-Paket zu verweisen
- SwapChainPanel
- Unterstützung für MRT Core
  - Das macht Apps beim Start schneller und effizienter, und beschleunigt die Ressourcensuche.
- ARM64-Unterstützung
- Drag & Drop innerhalb und außerhalb von Apps
- RenderTargetBitmap (derzeit nur XAML-Inhalte, keine SwapChainPanel-Inhalte)
- Benutzerdefinierte Cursorunterstützung
- Hintergrundthread-Eingabe
- Verbesserungen an unserer Tools/Entwicklererfahrung:
  - Live Visual Tree, Hot Reload, Live Property Explorer und ähnliche Tools
  - Intellisense für WinUI 3
- Erforderliche Verbesserungen für Open-Source-Migration
- Funktionen für benutzerdefinierte Titelleisten: neue [Window.ExtendsContentIntoTitleBar](/windows/winui/api/microsoft.ui.xaml.window.extendscontentintotitlebar)- und [Window.SetTitleBar](/windows/winui/api/microsoft.ui.xaml.window.settitlebar)-APIs, mit denen Entwickler benutzerdefinierte Titelleisten in Desktop-Apps erstellen können.
- Unterstützung für VirtualSurfaceImageSource
- In-App-Acryl

### <a name="preview-features"></a>Previewfunktionen

Da es sich um ein Stable-Release handelt, wurden Previewfunktionen in dieser Version von WinUI 3 entfernt. Mithilfe der aktuellen [Vorschauversion von WinUI 3](release-notes/winui3-project-reunion-0.5-preview.md)können Sie weiterhin auf diese Features zugreifen. Beachten Sie, dass sich die folgenden wichtigen Features noch in der Vorschauphase befinden und noch an deren Stabilisierung gearbeitet wird:

- UWP-Unterstützung
  - Dies bedeutet, dass Sie eine UWP-App nicht mithilfe der VSIX für WinUI 3 – Project Reunion 0.5 erstellen oder ausführen können. Sie müssen die [WinUI 3 – Project Reunion 0.5 Preview-VSIX](https://aka.ms/projectreunion/previewdownload)verwenden und die restlichen Anweisungen in [Erste Schritte mit Project Reunion](../../project-reunion/get-started-with-project-reunion.md) zum Einrichten Ihrer Entwicklungsumgebung befolgen. Weitere Informationen finden Sie in den [Versionshinweisen zu Version Windows-UI-Bibliothek 3 – Project Reunion 0.5 Preview (März 2021)](release-notes/winui3-project-reunion-0.5-preview.md).

- Unterstützung für mehrere Fenster in Desktop-Apps

- Eingabeüberprüfung

### <a name="provide-feedback-and-suggestions"></a>Bereitstellen von Feedback und Vorschlägen

Wir freuen uns über dein Feedback im [GitHub-Repository von WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="whats-coming-next"></a>Neue Entwicklungen

Weitere Informationen darüber, wann bestimmte Features geplant sind, finden Sie in der [Featureroadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap) auf GitHub.

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Für WinUI 3 – Project Reunion 0.5 sind die folgenden Probleme bekannt. Wenn Sie ein Problem finden, das im Folgenden nicht aufgeführt ist, informieren Sie uns, indem Sie Feedback zu einem vorhandenen oder neuen Problem über das [WinUI-GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) bereitstellen.

### <a name="platform-and-os-support"></a>Plattform- und Betriebssystemunterstützung

WinUI 3 – Project Reunion 0.5 ist mit PCs kompatibel, auf denen mindestens das Windows 10-Update vom Oktober 2018 (Version 1809, Build 17763) ausgeführt wird.

### <a name="developer-tools"></a>Entwicklertools

- Es werden nur C#- und C++ /WinRT-Apps unterstützt.
- Desktop-Apps unterstützen .NET 5 und C# 9 und müssen in einer MSIX-App gepackt sein.
- Keine Unterstützung für XAML-Designer
- Neue C++/CX-Apps werden nicht unterstützt. Deine bestehenden Apps funktionieren jedoch weiterhin (steige so schnell wie möglich auf C++/WinRT um)
- Nicht gepackte Desktopbereitstellung nicht unterstützt
- Wenn Sie eine Desktop-App mit F5 ausführen, stellen Sie sicher, dass Sie das Paketerstellungsprojekt ausführen. Wenn Sie F5 für das App-Projekt drücken, wird eine ungepackte App ausgeführt, die WinUI 3 noch nicht unterstützt.

### <a name="missing-platform-features"></a>Fehlende Plattformfeatures

- Xbox-Unterstützung
- HoloLens-Unterstützung
- Windows-Popups
  - Genauer gesagt verhält sich die `ShouldConstrainToRootBounds`-Eigenschaft immer so, als ob Sie auf `true` festgelegt wäre, unabhängig vom Eigenschaftswert.
- Freihandunterstützung, einschließlich:
  - [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)
  - [HandwritingView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HandwritingView)
  - [InkPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)
- Hintergrund-Acryl
- MediaElement und MediaPlayerElement
- MapControl
- SwapChainPanel unterstützt keine Transparenz
- Global Reveal nutzt Fallbackverhalten, einen einfarbigen Pinsel
- XAML Islands werden in diesem Release nicht unterstützt
- Für die direkte Verwendung von WinUI 3 in einer bestehenden Nicht-WinUI-Desktop-App gilt die folgende Einschränkung: Der derzeit verfügbare Migrationspfad für eine bestehende App besteht darin, ein **neues** WinUI 3-Projekt zu Ihrer Lösung hinzuzufügen und Ihre Logik nach Bedarf anzupassen oder umzugestalten.

- „Application.Suspending“ wird in Desktop-Apps nicht aufgerufen. Weitere Informationen finden Sie in der API-Referenzdokumentation für das [Ereignis „Application.Suspending](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true ). 

- CoreWindow, ApplicationView, CoreApplicationView, CoreDispatcher und deren Abhängigkeiten werden in Desktop-Apps nicht unterstützt (siehe unten)

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>CoreWindow, ApplicationView, CoreApplicationView und CoreDispatcher in Desktop-Apps

Neu in WinUI 3 Preview 4 und ab jetzt standardmäßig: [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow), [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView), [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) und deren Abhängigkeiten sind in Desktop-Apps nicht verfügbar. Beispielsweise ist die Eigenschaft [Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) immer **leer (Null)** , aber die **Window.DispatcherQueue**-Eigenschaft kann als Alternative verwendet werden.

Diese APIs funktionieren nur in UWP-Apps. In früheren Vorschauen konnten sie teilweise auch in Desktop-Apps verwendet werden, aber seit Preview 4 wurden sie vollständig deaktiviert. Diese APIs sind für den UWP-Fall konzipiert, in dem es nur ein Fenster pro Thread gibt, und ein Feature von WinUI3 besteht darin, künftig mehrere Fenster zu ermöglichen.

Es gibt APIs, die intern vom Vorhandensein dieser APIs abhängen. Diese APIs werden folglich in einer Desktop-App auch nicht unterstützt. Diese APIs verfügen in der Regel über eine statische `GetForCurrentView`-Methode. Beispielsweise [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView).

Weitere Informationen zu betroffenen APIs sowie Problemumgehungen und Ersatz für diese APIs finden Sie unter [WinRT-API-Änderungen für Desktop-Apps](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md).

### <a name="known-issues"></a>Bekannte Probleme

- ALT+F4 schließt Desktop-App-Fenster nicht.

- Die Ereignisse [UISettings.ColorValuesChanged Event](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) und [AccessibilitySettings.HighContrastChanged Event](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) werden in Desktop-Apps nicht mehr unterstützt. Dies kann zu Problemen führen, wenn Sie sie verwenden, um Änderungen in Windows-Designs zu ermitteln. 

- Um eine CompositionCapabilities-Instanz abzurufen, mussten Sie bisher [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview) aufrufen. Die von diesem Aufruf zurückgegebenen Funktionen waren jedoch *nicht* von der Ansicht abhängig. Um dies zu behandelt und widerzuspiegeln, haben wir die statische Methode „GetForCurrentView() in dieser Version gelöscht, sodass Sie ein [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities)-Objekt jetzt direkt erstellen können.

- Der Acryl-Pinsel wird transparent gerendert. 

- Aufgrund eines C#/WinRT-Problems kann das Abonnieren einiger Frameworkelement-Ereignisse und die Seitennavigation zu Arbeitsspeicherlecks führen. 

- Es gibt weitere Probleme mit C#/WinRT, die in diesem Release auftreten können, wie z. B. GC/ObjectDisposedExceptions, Marshallingwert und Typen mit Nullwert (TimeSpan, IReference<Vector3>, usw.) 
  - Diese werden in der bevorstehenden .NET 5 SDK-Wartungsversion korrigiert, die Mitte April ausgeliefert wird. Sie können dieses Update abrufen, indem Sie es explizit (z. B. in Buildpipelines) oder implizit über das Visual Studio-Update herunterladen und installieren.

## <a name="winui-3-controls-gallery"></a>WinUI 3-Steuerelementekatalog

Im WinUI 3-Steuerelementekatalog (früher _XAML-Steuerelementekatalog – WinUI 3-Version_) finden Sie eine Beispiel-App, die alle Steuerelemente und Features enthält, die Bestandteil von WinUI 3 – Project Reunion 0.5 sind.

![WinUI 3-Steuerelementekatalog-App](images/WinUI3XamlControlsGallery.png)<br/>
*Beispiel für eine WinUI 3-Steuerelementskatalog-App*

Sie können das Beispiel herunterladen, indem Sie das GitHub-Repository klonen. Dazu klonen Sie den **winui3**-Branch mit dem folgenden Befehl:

```
git clone --single-branch --branch winui3 https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Vergewissere dich nach dem Klonen, dass du in der lokalen Git-Umgebung zum Branch **winui3** wechselst: 

```
git checkout winui3
```
