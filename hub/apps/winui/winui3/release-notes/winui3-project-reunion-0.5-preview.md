---
title: Versionshinweise zu Windows UI-Bibliothek 3 – Project Reunion 0.5 Preview (März 2021)
description: Versionshinweise zu Release Windows UI-Bibliothek 3 – Project Reunion 0.5 Preview (März 2021).
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: e6d6f1a2007ab09b0295de24b1761d3dbb0c9094
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730665"
---
# <a name="windows-ui-library-3---project-reunion-05-preview-march-2021-release-notes"></a>Versionshinweise zu Windows UI-Bibliothek 3 – Project Reunion 0.5 Preview (März 2021)

Windows-UI-Bibliothek (WinUI) 3 ist eine native Benutzererfahrungs-Plattform (User Experience, UX) zur Erstellung moderner Windows-Apps. Diese Vorschau von WinUI 3 kann für Desktop-/Win32- und UWP-Apps verwendet werden und enthält Visual Studio-Projektvorlagen, die Ihnen beim Erstellen von Apps mit einer WinUI-basierten Benutzeroberfläche helfen, sowie ein NuGet-Paket, das die WinUI-Bibliotheken enthält.

**WinUI 3 – Project Reunion 0.5 Preview** ist das erste Release von WinUI 3 und wird als Teil des Project Reunion-Pakets bereitgestellt. Neben dieser Änderung bietet das Vorschaurelease Korrekturen kritischer Fehler, erhöhte Stabilität und einige andere allgemeine Verbesserungen (siehe **[Funktionen, die in WinUI 3 – Project Reunion 0.5 Preview eingeführt wurden](#major-changes-introduced-in-this-release)** ).

> [!Important]
> Die WinUI 3-Vorschau ist für eine frühe Evaluierung und zum Sammeln von Feedback von der Entwicklercommunity vorgesehen. Diese Version sollte **NICHT** für Produktions-Apps verwendet werden.
>
> Die Auslieferung von Project Reunion 0.5 ist für Ende März geplant. Dabei wird es die erste stabile, unterstützte Version von WinUI 3 enthalten.
>
> Verwenden Sie das [GitHub-Repository für WinUI](https://github.com/microsoft/microsoft-ui-xaml), um Feedback zu geben und Vorschläge und Probleme zu protokollieren.

## <a name="install-winui-3---project-reunion-05-preview"></a>Installieren von WinUI 3 – Project Reunion 0.5 Preview

Diese Version von WinUI 3 ist als Teil von Project Reunion 0.5 Preview verfügbar. 

Befolgen Sie zum Installieren die Anweisungen im Handbuch [Einrichten der Entwicklungsumgebung](../../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment) für Project Reunion. 

Im Gegensatz zu früheren Vorschauversionen von WinUI 3 laden Sie ein Project Reunion-VSIX-Paket anstelle eines WinUI-VSIX-Pakets herunter. Die Project Reunion-VSIX enthält [WinUI-Projektvorlagen](../winui-project-templates-in-visual-studio.md), die Sie zum Erstellen Ihrer WinUI 3-App verwenden. Nach Abschluss der Installation sollte sich die Erfahrung bei der Entwicklung einer WinUI 3-App nicht mehr ändern.

> [!NOTE]
> Sie können auch die WinUI 3 Vorschau-Version des [XAML-Steuerelementekatalogs](#xaml-controls-gallery-winui-3-preview-branch) klonen und erstellen.

> [!NOTE]
> Um die WinUI 3-Tools wie Live Visual Tree, Hot Reload und Live Property Explorer verwenden zu können, müssen Sie die WinUI 3-Tools mit den Visual Studio Preview-Features, wie [hier in der Anleitung](https://github.com/microsoft/microsoft-ui-xaml/issues/4140) beschrieben.

Nachdem Sie Ihre Entwicklungsumgebung eingerichtet haben, machen Sie sich unter [Erstellen von WinUI 3-Projekten](../winui-project-templates-in-visual-studio.md) mit den verfügbaren Vorlagen für Visual Studio-Projekte und -Elemente vertraut. 

Weitere Informationen zu den ersten Schritten mit den WinUI-Projektvorlagen finden Sie in den folgenden Artikeln:

- [Erste Schritte mit WinUI 3 für Desktop-Apps](../get-started-winui3-for-desktop.md)
- [Erste Schritte mit WinUI 3 für UWP-Apps (Vorschau)](../get-started-winui3-for-uwp.md)

Abgesehen von den [Einschränkungen und bekannten Problemen](#limitations-and-known-issues) ist das Erstellen einer App mithilfe der WinUI-Projekte vergleichbar mit dem Erstellen einer UWP-App mit XAML und WinUI 2.x. Daher lassen sich die meisten [Anleitungsdokumentationen](/windows/uwp/design/) für UWP-Apps und die **Windows.UI** WinRT-Namespaces im Windows SDK anwenden.

Die WinUI 3-API-Referenzdokumentation ist hier verfügbar: [WinUI 3 API-Referenz](/windows/winui/api)

Wenn Sie ein Projekt mit WinUI 3 Vorschau 4 erstellt haben, können Sie Ihr Projekt auf die Verwendung von Project Reunion 0.5 Preview upgraden. Im [WinUI GitHub-Repository](https://aka.ms/winui3/upgrade-instructions) finden Sie ausführliche Anleitungen.

### <a name="webview2"></a>Webansicht2
Um WebView2 mit dieser WinUI 3-Vorschau zu verwenden, laden Sie Evergreen Bootstrapper oder Evergreen Standalone Installer herunter. Sofern Sie die WebView2-Runtime noch nicht installiert haben, finden Sie diese Installationsprogramme [hier](https://developer.microsoft.com/microsoft-edge/webview2/). 

### <a name="windows-community-toolkit"></a>Windows-Community-Toolkit

Wenn Sie das Windows Community Toolkit verwenden, [laden Sie die neueste Version herunter](https://aka.ms/wct-winui3).

### <a name="visual-studio-support"></a>Visual Studio-Unterstützung

Um die neuesten, in WinUI 3 hinzugefügten Toolfeatures wie Hot Reload, Live Visual Tree und Live Property Explorer nutzen zu können, müssen Sie die neueste **Vorschauversion** von Visual Studio mit der neuesten WinUI 3-Vorschau verwenden und sicherstellen, dass die WinUI-Tools in den Visual Studio Preview-Funktionen aktiviert sind, wie [hier in der Anleitung](https://github.com/microsoft/microsoft-ui-xaml/issues/4140) beschrieben. Die folgende Tabelle zeigt die Kompatibilität zukünftiger Versionen mit WinUI 3 – Project Reunion 0.5 Preview:

| VS-Version  | WinUI 3 – Project Reunion 0.5 Preview  |
|---|---|
| 16.8 RTM  | Nein   |
| 16.9-Vorschauen  | Ja, mit Tools  | 
| 16.9 RTM  | Ja, aber ohne Hot Reload, Live Visual Tree oder Live Property Explorer   |
| 16.10 Vorschauen  | Ja, mit Tools   |

## <a name="major-changes-introduced-in-this-release"></a>Wichtige Änderungen in diesem Release

- WinUI 3 wird nun als Teil des Project Reunion-Pakets ausgeliefert, was auch der Mechanismus für die Auslieferung unserer zukünftigen unterstützten Releases sein wird.

- In-App-Acryl wird jetzt unterstützt.

- Das Pivot-Steuerelement wird nicht mehr unterstützt und wurde in WinUI 3 als veraltet markiert. Es wird empfohlen, für Ihre In-App-Navigationsszenarien das [Steuerelement „NavigationView“](/windows/uwp/design/controls-and-patterns/navigationview) zu verwenden.

- WinUI 3 und Project Reunion werden abwärts nur bis zur Windows 10 Version 1809 unterstützt – Build 17763 oder höher wird benötigt.

- Previewfunktionen sind nun als experimentell gekennzeichnet. 
  - Eine Previewfunktion ist alles, was weiterhin Teil einer WinUI 3-Vorschau, aber nicht Teil des nächsten von WinUI 3 unterstützten Releases ist. 
  - Previewfunktionen enthalten außerdem alle experimentellen APIs, die Teil der WinUI 2.6-Vorschau sind.
  - Wenn Sie eine App entwickeln, die eine Previewfunktion verwendet, löst Ihre App eine Warnung aus. 


## <a name="list-of-bugs-fixed-in-winui-3---project-reunion-05-preview"></a>Liste der behobenen Fehler in WinUI 3 – Project Reunion 0.5 Preview

Im folgenden finden Sie eine Liste der seit Vorschau 3 durch das Team behobenen Fehler, die für Benutzer auftreten können. Es wurde auch viel an der Stabilisierung und Verbesserung unserer Tests gearbeitet.

- WinUI 3-Fehlermeldung muss umformuliert werden: „'Windows.metadata' kann nicht aufgelöst werden“.  Bitte installieren Sie das Windows SDK (Software Development Kit). Das Windows SDK wird mit Visual Studio installiert.“
- Die App reagiert bei Auswahl des Designs „Standard (Windows)“ auf Designänderungen in Windows erst nach einem Neustart
- Ausnahme bei XamlDirect.CreateInstance-Aufruf
  - Unser Dank geht an @BorzillaR, der dieses [Problem auf GitHub gemeldet hat](https://github.com/microsoft/microsoft-ui-xaml/issues/3509).
- ProgressBar zeigt den Unterschied zwischen „Angehalten“ und „Fehler“ nicht an
- Das Flyout des StandardUICommand-Listenansichtelements wird an falscher Position angezeigt.
- Absturz in Desktop XamlControlsGallery beim Versuch, ListView-Elemente per Touch neu zu sortieren
  - Unser Dank geht an @j0shuams, der dieses [Problem auf GitHub gemeldet hat](https://github.com/microsoft/microsoft-ui-xaml/issues/3694).
- Drücken und Halten von RichTextBlock setzt Flyout an falsche Position
- NACH-LINKS-/NACH-RECHTS-TASTEN bewegen den Fokus nicht durch RadioButtons
  - Unser Dank geht an @vmadurga, der dieses [Problem auf GitHub gemeldet hat](https://github.com/microsoft/microsoft-ui-xaml/issues/3385).
- Sprachausgabe bleibt stumm, wenn der Benutzer die NACH-OBEN-oder NACH-OBEN-TASTE drückt, um in DatePicker zum nächsten/vorherigen Monat/Jahr/Datum zu wechseln

- NavigationView markieren/verwerfen funktioniert in WinUI 3 nicht


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>Neue Features und Funktionen, die in früheren WinUI 3 Vorschauen eingeführt wurden

Die folgenden Features und Funktionen wurden in WinUI 3 Vorschau 1–4 eingeführt und werden in WinUI 3 – Project Reunion 0.5 Preview weiterhin unterstützt.

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

Weitere Informationen zu den Vorteilen von WinUI 3 und denen der WinUI-Roadmap finden Sie in der [Roadmap der Windows-UI-Bibliothek](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) auf GitHub.

### <a name="provide-feedback-and-suggestions"></a>Bereitstellen von Feedback und Vorschlägen

Wir freuen uns über dein Feedback im [GitHub-Repository von WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="whats-coming-next"></a>Neue Entwicklungen

Werfen Sie einen Blick auf unsere detaillierte [Featureroadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap), um zu sehen, wann spezifische Features in WinUI 3 eingeführt werden. 

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

WinUI 3 – Project Reunion 0.5 Preview ist genau das, eine Vorschau. Die Szenarien rund um Desktop-Apps sind ganz neu. Es sind Fehler, Einschränkungen und andere Probleme zu erwarten.

Für WinUI 3 – Project Reunion 0.5 Preview sind die folgenden Probleme bekannt. Wenn Sie ein Problem finden, das im Folgenden nicht aufgeführt ist, informieren Sie uns, indem Sie Feedback zu einem vorhandenen oder neuen Problem über das [WinUI-GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) bereitstellen.

### <a name="platform-and-os-support"></a>Plattform- und Betriebssystemunterstützung

WinUI 3 – Project Reunion 0.5 Preview ist mit PCs kompatibel, auf denen mindestens das Windows 10-Update vom Oktober 2018 (Version 1809, Build 17763) ausgeführt wird.

### <a name="developer-tools"></a>Entwicklertools

- Es werden nur C#- und C++ /WinRT-Apps unterstützt.
- Desktop-Apps unterstützen .NET 5 und C# 9 und müssen in einer MSIX-App gepackt sein.
- UWP-Apps unterstützen .NET Native und C# 7.3
- Entwicklertools und IntelliSense funktionieren in Visual Studio möglicherweise nicht ordnungsgemäß.
- Keine Unterstützung für XAML-Designer
- Neue C++/CX-Apps werden nicht unterstützt. Deine bestehenden Apps funktionieren jedoch weiterhin (steige so schnell wie möglich auf C++/WinRT um)
- Die Unterstützung mehrerer Fenster in Desktop-Apps ist in Entwicklung, aber noch nicht abgeschlossen und stabil.
  - Wenn Sie neue Probleme oder Regressionen mit dem Verhalten mehrerer Fenstern finden, melden Sie diese Fehler in unserem Repository.
- Nicht gepackte Desktopbereitstellung nicht unterstützt
- Wenn Sie eine Desktop-App mit F5 ausführen, stellen Sie sicher, dass Sie das Paketerstellungsprojekt ausführen. Wenn Sie F5 für das App-Projekt drücken, wird eine ungepackte App ausgeführt, die WinUI 3 noch nicht unterstützt.

### <a name="missing-platform-features"></a>Fehlende Plattformfeatures

- Xbox-Unterstützung
- HoloLens-Unterstützung
- Windows-Popups
  - Genauer gesagt verhält sich die `ShouldConstrainToRootBounds`-Eigenschaft immer so, als ob Sie auf `true` festgelegt wäre, unabhängig vom Eigenschaftswert.
- Unterstützung für Freihandeingaben
- Acryl
- MediaElement und MediaPlayerElement
- MapControl
- „RenderTargetBitmap“ für „SwapChainPanel“ und nicht-XAML-Inhalt
- SwapChainPanel unterstützt keine Transparenz
- Global Reveal nutzt Fallbackverhalten, einen einfarbigen Pinsel
- XAML Islands werden in diesem Release nicht unterstützt
- Bibliotheken aus dem Ökosystem von Drittanbietern funktionieren nicht in vollem Umfang
- IMEs funktionieren nicht
- CoreWindow, ApplicationView, CoreApplicationView, CoreDispatcher und deren Abhängigkeiten werden in Desktop-Apps nicht unterstützt (siehe unten)

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>CoreWindow, ApplicationView, CoreApplicationView und CoreDispatcher in Desktop-Apps

Neu in Preview 4 und ab jetzt standardmäßig, [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow), [ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView), [CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) und deren Abhängigkeiten sind in Desktop-Apps nicht verfügbar. Beispielsweise ist die Eigenschaft [Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) immer leer (Null), aber die Window.DispatcherQueue-Eigenschaft kann als Alternative verwendet werden.

Diese APIs funktionieren nur in UWP-Apps. In früheren Vorschauen konnten sie teilweise auch in Desktop-Apps verwendet werden, aber in Preview4 wurden sie vollständig deaktiviert. Diese APIs sind für den UWP-Fall konzipiert, in dem es nur ein Fenster pro Thread gibt, und ein Feature von WinUI3 besteht darin, mehrere Fenster zu ermöglichen.

Es gibt APIs, die intern vom Vorhandensein dieser APIs abhängen. Diese APIs werden folglich in einer Desktop-App auch nicht unterstützt. Diese APIs verfügen in der Regel über eine statische `GetForCurrentView`-Methode. Beispielsweise [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView).

Weitere Informationen zu betroffenen APIs sowie Problemumgehungen und Ersatz für diese APIs finden Sie unter [WinRT-API-Änderungen für Desktop-Apps](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md).

### <a name="known-issues"></a>Bekannte Probleme

- Es gibt ein Problem, das dazu führt, dass UWP-Apps unter Windows 10, Version 1809, nicht gestartet werden. Das Team arbeitet aktiv daran, diesen Fehler für die nächste Vorschauversion zu beheben.

- ALT+F4 schließt Desktop-App-Fenster nicht.

- Die Ereignisse [UISettings.ColorValuesChanged Event](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) und [AccessibilitySettings.HighContrastChanged Event](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) werden in Desktop-Apps nicht mehr unterstützt. Dies kann zu Problemen führen, wenn Sie sie verwenden, um Änderungen in Windows-Designs zu ermitteln. 

- Dieses Release enthält einige experimentelle APIs, die bei der Verwendung Buildwarnungen auslösen. Diese wurden vom Team noch nicht gründlich getestet und können unbekannte Probleme verursachen. Wenn Probleme auftreten, [melden Sie einen Fehler](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=) in unserem Repository. 

- Um eine CompositionCapabilities-Instanz abzurufen, mussten Sie bisher [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview) aufrufen. Die von diesem Aufruf zurückgegebenen Funktionen waren jedoch *nicht* von der Ansicht abhängig. Um dies zu behandelt und widerzuspiegeln, haben wir die statische Methode „GetForCurrentView() in dieser Version gelöscht, sodass Sie ein [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities)-Objekt jetzt direkt erstellen können.

- Für C# UWP-Apps:

  Das WinUI 3-Framework ist ein Satz von WinRT-Komponenten, die aus C++ (mit C++ /WinRT) oder C# verwendet werden können. Bei Verwendung von C# gibt es je nach App-Modell zwei Versionen von .NET: Bei Nutzung von WinUI 3 in einer UWP-App wird .NET Native verwendet, in einer Desktop-App wird .NET 5 (und C#/WinRT) verwendet.

  Bei Verwendung von C# für eine WinUI 3-App in UWP gibt es einige API-Namespaceunterschiede im Vergleich zu C# in einer WinUI 3-Desktop-App oder einer C# WinUI 2-App: Einige Typen befinden sich in einem `Microsoft`-Namespace statt in einem `System`-Namespace. Die `INotifyPropertyChanged`-Schnittstelle befindet sich beispielsweise nicht im `System.ComponentModel`-Namespace, sondern im `Microsoft.UI.Xaml.Data`-Namespace. 

  Dies gilt für:
    - `INotifyPropertyChanged` (und verwandte Typen)
    - `INotifyCollectionChanged`
    - `ICommand`

  Die `System`-Namespaceversionen sind noch vorhanden, können jedoch nicht mit WinUI 3 verwendet werden. Dies bedeutet, dass `ObservableCollection` in der vorliegenden Form nicht in WinUI 3 C# UWP-Apps funktioniert. Eine Problemumgehung finden Sie im [CollectionsInterop-Beispiel](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs) im [Beispiel zum XAML-Steuerelementekatalog](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

## <a name="xaml-controls-gallery-winui-3-preview-branch"></a>XAML-Steuerelementekatalog (Branch „WinUI 3 Vorschau“)

Im [Branch „WinUI 3 Vorschau“ des XAML-Steuerelementekatalogs](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) finden Sie eine Beispiel-App, die alle Steuerelemente und Features enthält, die Teil von WinUI 3 – Project Reunion 0.5 Preview sind.

:::image type="content" source="../images/WinUI3XamlControlsGallery.png" alt-text="XAML-Steuerelementekatalog-App für WinUI 3 Vorschau":::<br/>
*Beispiel der XAML-Steuerelementekatalog-App für WinUI 3 Vorschau*

Um das Beispiel herunterzuladen, klone den Branch **winui3preview** mit dem folgenden Befehl:

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Vergewissere dich nach dem Klonen, dass du in der lokalen Git-Umgebung zum Branch **winui3preview** wechselst: 

```
git checkout winui3preview
```
