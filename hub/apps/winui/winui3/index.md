---
title: WinUI 3 Vorschau 2 (Juli 2020)
description: Übersicht über das Release von WinUI 3 Vorschau 2.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: c57132ec5219ef32f2b2b69168592e07f49d904b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168774"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Windows-UI-Bibliothek 3 Vorschau 2 (Juli 2020)

Die Windows-UI-Bibliothek 3(WinUI) ist ein natives Benutzererfahrungs-Framework (User Experience, UX) für Windows-Desktop- als auch UWP-Apps.

**WinUI 3 Vorschau 2** ist ein Qualitäts- und Stabilitätsrelease mit dem Fokus auf der Korrektur von Fehlern und bekannten Problemen aus dem Release Vorschau 1.

**Weitere Informationen finden Sie unter [Einschränkungen und bekannte Probleme in Vorschau 2](#preview-2-limitations-and-known-issues)** .

> [!Important]
> Diese WinUI 3-Vorschauversion ist für eine frühe Evaluierung und zum Sammeln von Feedback aus der Entwicklercommunity vorgesehen. Diese Version sollte **NICHT** für Produktions-Apps verwendet werden.
>
> Wir werden weiterhin die Vorschauversionen von WinUI 3 in ganz 2020 und bis ins frühe 2021 veröffentlichen. Danach wird das erste offizielle Release verfügbar gemacht.
>
> Verwenden Sie das [GitHub-Repository für WinUI](https://github.com/microsoft/microsoft-ui-xaml), um Feedback zu geben und Vorschläge und Probleme zu protokollieren.

## <a name="install-winui-3-preview-2"></a>Installieren von WinUI 3 Vorschau 2

WinUI 3 Vorschau 2 enthält Visual Studio-Projektvorlagen, die Ihnen beim Erstellen von Apps mit einer WinUI-basierten Benutzeroberfläche helfen, sowie ein NuGet-Paket, das die WinUI-Bibliotheken enthält. Um WinUI 3 Vorschau 2 zu installieren, führen Sie die folgenden Schritte aus.

> [!NOTE]
> Sie können auch die WinUI 3-Vorschauversion 2 des [XAML-Steuerelementekatalogs](#xaml-controls-gallery-winui-3-preview-2-branch) klonen und erstellen.

1. Stellen Sie sicher, dass auf Ihrem Entwicklungscomputer mindestens Windows 10, Version 1803 (Build 17134) oder höher installiert ist.

2. Installieren von [Visual Studio 2019, Version 16.7.2](https://visualstudio.microsoft.com/vs/)

    Bei der Installation von Visual Studio müssen Sie die folgenden Workloads einschließen:
    - .NET-Desktopentwicklung
    - Entwicklung für die universelle Windows-Plattform

    Zum Erstellen von C++-Apps müssen Sie außerdem die folgenden Workloads einschließen:
    - Desktopentwicklung mit C++
    - Die optionale Komponente *C++ (v142) UWP-Tools (Universelle Windows-Plattform)* für die UWP-Workload (siehe „Installationsdetails“ im Abschnitt „Entwicklung mit der universellen Windows-Plattform“ im rechten Bereich).

    Nach dem Herunterladen von Visual Studio müssen Sie die .NET-Vorschauversionen innerhalb des Programms aktivieren: 
    - Wechseln Sie zu „Extras“ > „Optionen“ > „Previewfunktionen“, und wählen Sie „Vorschauversionen des .NET Core SDK verwenden (Neustart erforderlich)“ aus. 

3. Wenn Sie Desktop-WinUI-Projekte für C#/.NET 5- und C++/Win32-Apps erstellen möchten, müssen Sie auch die x64- und x86-Versionen von .NET 5 Vorschau 5 installieren. **Beachten Sie, dass .NET 5 Preview 5 derzeit die einzige unterstützte .NET 5-Vorschauversion für WinUI 3 ist**:

    - x64: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x64.exe)
    - x86: [https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview5/Sdk/dotnet-sdk-win-x86.exe)

4. Laden Sie das [WinUI 3 Vorschau 2-VSIX-Paket](https://aka.ms/winui3/previewdownload) herunter und installieren Sie es. Das VSIX-Paket fügt Visual Studio 2019 die WinUI 3-Projektvorlagen und das NuGet-Paket mit den WinUI 3-Bibliotheken hinzu.

    Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Suchen nach und Verwenden von Visual Studio-Erweiterungen](/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).


## <a name="create-winui-projects"></a>Erstellen von WinUI-Projekten

Nachdem Sie das WinUI 3 Vorschau 2 VSIX-Paket installiert haben, können Sie mithilfe einer der WinUI-Projektvorlagen in Visual Studio ein neues Projekt erstellen. Um auf die WinUI-Projektvorlagen im Dialogfeld **Neues Projekt erstellen** zuzugreifen, filtern Sie die Sprache auf **C++** oder **C#** , die Plattform auf **Windows** und den Projekttyp auf **WinUI**. Alternativ können Sie nach *WinUI* suchen und eine der verfügbaren C#- oder C++-Vorlagen auswählen.

![WinUI-Projektvorlagen](images/winui-projects-csharp.png)

Weitere Informationen zu den ersten Schritten mit den WinUI-Projektvorlagen finden Sie in den folgenden Artikeln:

- [Erste Schritte mit WinUI 3 für Desktop-Apps](get-started-winui3-for-desktop.md)
- [Erste Schritte mit WinUI 3 für UWP-Apps](get-started-winui3-for-uwp.md)

Abgesehen von den [Einschränkungen und bekannten Problemen](#preview-2-limitations-and-known-issues) ist das Erstellen einer App mithilfe der WinUI-Projekte vergleichbar mit dem Erstellen einer UWP-App mit XAML und WinUI 2.x. Daher lassen sich die meisten Anleitungen und Dokumentationen für UWP-Apps sowie die **Windows.UI** WinRT-Namespaces im Windows SDK anwenden.

Wenn Sie ein Projekt mit WinUI 3 Vorschau 1 erstellt haben, können Sie Ihr Projekt auf die Verwendung von Vorschau 2 upgraden. Eine ausführliche Anleitung finden Sie in [unserem GitHub-Repository](https://aka.ms/winui3/upgrade-instructions).

### <a name="project-templates-for-winui-3"></a>Projektvorlagen für WinUI 3

Sie können diese WinUI-Projektvorlagen verwenden, um Apps zu erstellen.

| Vorlage | Language | BESCHREIBUNG |
|----------|----------|-------------|
| Leere App, Gepackt (WinUI in Desktop) | C# und C++ | Erstellt eine .NET 5 (C#)-Desktop- oder native Win32 (C++ )-App mit einer WinUI-basierten Benutzeroberfläche. Das generierte Projekt enthält ein einfaches Fenster, das von der **Microsoft.UI.Xaml.Window**-Klasse in der WinUI-Bibliothek abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. Weitere Informationen zu diesem Projekttyp finden Sie unter [Erste Schritte mit WinUI für Desktop-Apps](get-started-winui3-for-desktop.md).<p></p>Die Lösung umfasst außerdem ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr.  |
| Leere App (WinUI in UWP)  | C# und C++ | Erstellt eine UWP-App mit einer WinUI-basierten Benutzeroberfläche. Das generierte Projekt enthält eine einfache Seite, die von der **Microsoft.UI.Xaml.Controls.Page**-Klasse in der WinUI-Bibliothek abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. Weitere Informationen zu diesem Projekttyp finden Sie unter [Erste Schritte mit WinUI für UWP-Apps](get-started-winui3-for-uwp.md). |

Sie können diese WinUI-Projektvorlagen verwenden, um Komponenten zu erstellen, die von einer WinUI-basierten App geladen und verwendet werden können.

| Vorlage | Language | BESCHREIBUNG |
|----------|----------|-------------|
| Klassenbibliothek (WinUI in Desktop) | Nur C# | Erstellt eine verwaltete .NET 5-Klassenbibliothek (DLL) in C# , die von anderen .NET 5-Desktop-Apps mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann.  |
| Klassenbibliothek (WinUI in UWP)  | Nur C# | Erstellt eine verwaltete Klassenbibliothek (DLL) in C#, die von anderen UWP-Apps mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann. |
| Windows-Runtime-Komponente (WinUI in UWP) | C# und C++ | Erstellt eine [Windows-Runtime-Komponente](/windows/uwp/winrt-components/), die in C# oder C++/WinRT geschrieben ist, die von einer UWP-App mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |

### <a name="item-templates-for-winui-3"></a>Elementvorlagen für WinUI 3

Die folgenden Elementvorlagen stehen für die Verwendung in einem WinUI-Projekt zur Verfügung. Um auf diese WinUI-Projektvorlagen zuzugreifen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Hinzufügen** -> **Neues Element** aus, und klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **WinUI**.

![WinUI-Elementvorlagen](images/winui-items-csharp.png)

| Vorlage | Language | Beschreibung |
|----------|----------|-------------|
| Leere Seite (WinUI) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die eine neue Seite definieren, die von der **Microsoft.UI.Xaml.Controls.Page**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Leeres Fenster (WinUI in Desktop) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die ein neues Fenster definiert, das von der **Microsoft.UI.Xaml.Window**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Benutzerdefiniertes Steuerelement (WinUI) | C# und C++ | Fügt eine Codedatei zum Erstellen eines Steuerelements mit Vorlagen mit einem Standardstil hinzu. Das Steuerelement mit Vorlagen wird von der **Microsoft.UI.Xaml.Controls.Control**-Klasse in der WinUI-Bibliothek abgeleitet.<p></p>Eine exemplarische Vorgehensweise, in der die Verwendung dieser Elementvorlage veranschaulicht wird, finden Sie unter [XAMl-Steuerelemente mit Vorlagen für UWP- und WinUI 3-Apps mit C++/WinRT](xaml-templated-controls-cppwinrt-winui3.md). Weitere Informationen zu Steuerelementen mit Vorlagen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Ressourcenverzeichnis (WinUI) | C# und C++ | Fügt eine leere, mit Schlüsseln versehene Sammlung von XAML-Ressourcen hinzu. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Ressourcendatei (WinUI) | C# und C++ | Fügt eine Datei zum Speichern von Zeichenfolgen- und bedingten Ressourcen für Ihre App hinzu. Mithilfe dieses Elements können Sie Ihre App lokalisieren. Weitere Informationen finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im Paketmanifest der App](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Benutzersteuerelement (WinUI) | C# und C++ | Fügt eine XAML- und Codedatei zum Erstellen eines Benutzersteuerelements hinzu, das von der **Microsoft.UI.Xaml.Controls.UserControl**-Klasse in der WinUI-Bibliothek abgeleitet ist. In der Regel kapselt ein Benutzersteuerelement zugehörige vorhandene Steuerelemente und stellt eine eigene Logik bereit.<p></p>Weitere Informationen zu Benutzersteuerelementen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>Fehlerkorrekturen und andere Verbesserungen in WinUI 3 Preview 2

Dies ist eine umfassende Liste von Fehlerkorrekturen und anderen Updates für Preview 2. Eine Liste der wichtigsten Fehlerkorrekturen, die in diesem Release behoben wurden, finden Sie in unserer [Release-Ankündigung](https://aka.ms/winui3/preview2-announcement).

> [!NOTE]
> WinUI 3 Preview 2 verwendet Version 2.4.2 der WinUI 2-Bibliothek. 

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged?view=net-5.0) und [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=net-5.0) funktionieren jetzt erwartungsgemäß in C#-Desktop-Apps.
  - Dadurch wurden einige andere Probleme behoben, die sich auf Auflistungssteuerelemente bezogen, die zwar im Back-End, aber nicht in der Benutzeroberfläche aktualisiert wurden.
  - *Dank geht an @hshristov, der ein [ähnliches Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/2490) auf GitHub gemeldet hat!*
- Vorschau 2 ist jetzt mit [.NET 5 Vorschau 5](/dotnet/api/?view=net-5.0) für Desktop-Apps kompatibel.
- WinUI 3 verfügt jetzt über Parität mit [WinUI 2.4](../winui2/release-notes/winui-2.4.md), das neue Steuerelemente und Features wie [hierarchische NavigationView](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) und [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring) umfasst.
- Absturz behoben: Verwenden von [TabView](/windows/uwp/design/controls-and-patterns/tab-view) mit Toucheingabe
- [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) im [XAML-Steuerelementekatalog-Beispiel](#xaml-controls-gallery-winui-3-preview-2-branch) verwendet jetzt den „Left“-Modus anstelle des „Left-compact“-Modus.
- Absturz behoben: Zu schnelle Eingabe in Eingabeüberprüfung und [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box)
  - *Dank geht an @paulovilla, der [dieses Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/2563) auf GitHub gemeldet hat!*
- Absturz behoben: Interagieren mit der XAML-Benutzeroberfläche bei angezeigtem [TextBox](/windows/uwp/design/controls-and-patterns/text-box)-Menü
- Titeltext im [XAML-Steuerelementekatalog-Beispiel](#xaml-controls-gallery-winui-3-preview-2-branch) ist nach dem Navigieren zu mehreren Seiten nicht mehr durcheinander
- Die Verwendung von Toucheingabe mit [WebView2](/microsoft-edge/webview2/) führt nicht mehr zu einem leichten Versatz bei der Positionierung
- Klassen in „WinUIEdit.dll“ wurden aus dem „Windows.UI.Text“-Namespace in den „Microsoft.UI.Text“-Namespace verschoben.
- Absturz behoben: Auswählen eines Elements in [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) im Mehrfachauswahlmodus (in Windows 10, Version 1803)
- Die Member „Point“, „Rect“ und „Size“ sind nun vom Typ „Double“ in der C#-Projektion der APIs für Desktop-Apps.
  - *Dank geht an @dotMorten, der [dieses Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/2474) auf GitHub gemeldet hat!*
- Absturz behoben: Verwenden von [RichEditBox-](/windows/uwp/design/controls-and-patterns/rich-edit-box) mit einer RTF-Datei
- Die QuickInfo der Schaltfläche „Schließen“ von [TabView](/windows/uwp/design/controls-and-patterns/tab-view) ist nicht mehr leer
- Das [Image](/windows/uwp/design/controls-and-patterns/images-imagebrushes)-Steuerelement rendert SVG-Dateien jetzt korrekt
  - *Dank geht an @mqudsi, der [dieses Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/2565) auf GitHub gemeldet hat!*
- Absturz behoben: Verwenden des/Navigieren zum Page-Element
- Die Verwendung der Toucheingabe zum Auswählen von Elementen in einer [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) deaktiviert jetzt alle anderen Elemente (im Einzelauswahlmodus)
- Absturz behoben: „LayoutSliderException“ wegen eines Werts, der auf einem [Schieberegler](/windows/uwp/design/controls-and-patterns/slider) von spezifischer Größe festgelegt ist, tritt nicht mehr auf 
  - *Dank geht an @hig-dev, der [dieses Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/477) auf GitHub gemeldet hat!*
- Absturz behoben: Die Verwendung von [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) hat zu einem Absturz beim Herunterfahren geführt
- Absturz behoben: Die Verwendung von [Pivot](/windows/uwp/design/controls-and-patterns/pivot) hat zu einem Absturz beim Herunterfahren geführt
- Absturz behoben: Absturz von [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) verursacht durch fehlende Ressource in Windows 10, Version 1803
- Absturz behoben: Setzen des Fokus auf [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) im benutzerdefinierten Editor 
- Absturz behoben: [SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- Die Bindung funktioniert nun erwartungsgemäß im Markup, wobei „Mode=OneWay“ implizit ist
  - *Dank geht an @tomasfabian, der [dieses Problem](https://github.com/microsoft/microsoft-ui-xaml/issues/2630) auf GitHub gemeldet hat!*
- Korrigierte Animation: Neuerungen in [XAML-Steuerelementekatalog-Beispiel](#xaml-controls-gallery-winui-3-preview-2-branch)

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>Neue Features und Funktionen, die in WinUI 3 Vorschau 1 eingeführt wurden

Die folgenden Features und Funktionen wurden in WinUI 3 Vorschau 1 eingeführt und werden in WinUI 3 Vorschau 2 weiterhin unterstützt.

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
- Erforderliche Verbesserungen für Open-Source-Migration

Weitere Informationen zu den Vorteilen von WinUI 3 und denen der WinUI-Roadmap finden Sie in der [Roadmap der Windows-UI-Bibliothek](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) auf GitHub.

### <a name="provide-feedback-and-suggestions"></a>Bereitstellen von Feedback und Vorschlägen

Wir freuen uns über dein Feedback im [GitHub-Repository von WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="preview-2-limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme in Vorschau 2

Das Vorschau 2-Release ist, wie der Name schon sagt, lediglich eine Vorschau. Die Szenarien um Win32-Desktop-Apps sind ganz neu. Es sind Fehler, Einschränkungen und andere Probleme zu erwarten.

Für WinUI 3 Vorschau 2 sind die folgenden Probleme bekannt. Wenn du ein Problem findest, das im Folgenden nicht aufgeführt ist, informiere uns, indem du Feedback zu einem vorhandenen oder neuen Problem im [WinUI-GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) bereitstellst.

### <a name="platform-and-os-support"></a>Plattform- und Betriebssystemunterstützung

WinUI 3 Vorschau 2 ist mit PCs kompatibel, auf denen mindestens das Windows 10-Update vom April 2018 (Version 1803, Build 17134) ausgeführt wird.

### <a name="developer-tools"></a>Entwicklungstools

- Zurzeit werden nur C#- und C++ /WinRT-Apps unterstützt.
- Desktop-Apps unterstützen .NET 5 und C# 8 und müssen gepackt sein
- UWP-Apps unterstützen .NET Native und C# 7.3
- IntelliSense ist unvollständig
- Kein visueller Designer
- Kein Hot Reload
- Keine visuelle Livestruktur
- Entwicklung mit VS Code noch nicht unterstützt
- Neue C++/CX-Apps werden nicht unterstützt. Deine bestehenden Apps funktionieren jedoch weiterhin (steige so schnell wie möglich auf C++/WinRT um)
- WinUI 3-Inhalte können sich nur in einem Fenster pro Prozess oder in einer ApplicationView pro App befinden
- Nicht gepackte Desktopbereitstellung nicht unterstützt
- Keine ARM64-Unterstützung
- Benutzerdefinierte C#-Steuerelemente in UWP-Apps: `Themes/Generic.xaml` wird nicht automatisch generiert. Sie können dieses Problem umgehen, indem Sie manuell einen Ordner „Themes“ (Designs) in Ihrer Klasse erstellen und darin eine XAML-Datei namens `Generic.xaml` ablegen.
- Nachdem Sie Ihrem Projekt ein benutzerdefiniertes WinUI-Steuerelement hinzugefügt haben, fehlt in Ihren Dateien möglicherweise der Header „CustomControl.h“. Dieses Problem können Sie umgehen, indem Sie diesen Header manuell in Ihrer Datei `pch.h` hinzufügen.
- Das Hinzufügen des DataGrid-Steuerelements, anderer Windows Community Toolkit-Steuerelemente und von Bibliothekssteuerelementen von Drittanbietern kann zu Fehlern im Build führen. Um dieses Problem zu umgehen, fügen Sie Ihrer Datei `App.xaml` dieses zusammengeführte Wörterbuch hinzu:
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>Fehlende Plattformfeatures

- Xbox-Unterstützung
- HoloLens-Unterstützung
- Windows-Popups
- Unterstützung für Freihandeingaben
- Hintergrund-Acryl
- MediaElement und MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel unterstützt keine Transparenz
- Global Reveal nutzt Fallbackverhalten, einen einfarbigen Pinsel
- XAML Islands werden in diesem Release nicht unterstützt
- Bibliotheken aus dem Ökosystem von Drittanbietern funktionieren nicht in vollem Umfang
- IMEs funktionieren nicht

### <a name="known-issues"></a>Bekannte Probleme


- C# UWP-Apps:

  Das WinUI 3-Framework ist ein Satz von WinRT-Komponenten, und obwohl WinRT über ähnliche Typen und Objekte verfügt wie .NET, sind sie nicht grundsätzlich kompatibel.  Die C#/WinRT-Projektionen handhaben die Interoperabilität zwischen .NET und WinRT in .NET 5 so, dass Sie heute .NET-Schnittstellen in Ihrer .NET 5-App frei verwenden können. 
  
  Allerdings ist C#/WinRT nicht in der Lage, die Interoperabilität in .NET Native-Apps zu verarbeiten, sodass die WinUI 3-APIs direkt in UWP-Apps projiziert werden. Daher können Sie nicht mehr die gleichen .NET-Schnittstellen verwenden. **Sobald UWP-Apps nicht mehr .NET Native verwenden, wird diese Einschränkung wegfallen**.

  Beispielsweise wird die `INotifyPropertyChanged`-API in Desktop-Apps in den `System.ComponentModel`-Namespace für WinUI3 projiziert, in UWP-Apps (und allen anderen C++-Apps) aber im `Microsoft.UI.Xaml.Data`-Namespace für WinUI3 angezeigt. 
  
  Dieses Problem betrifft:
    - `INotifyPropertyChanged` (und verwandte Typen)
    - `INotifyCollectionChanged`
    - `ICommand`

> [!Note] 
> Wenn `INotifyPropertyChanged` und `INotifyCollectionChanged` nicht erwartungsgemäß funktionieren, wirkt sich dies auch auf die `ObservableCollection<T>`-Klasse aus. Ein Beispiel für die Implementierung Ihrer eigenen Version von `ObservableCollection<T>` finden Sie [hier](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs). 


## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML-Steuerelementekatalog (Branch „WinUI 3 Vorschau 2“)

Eine Beispiel-App, die alle WinUI 3 Vorschau 2-Steuerelemente und -Features enthält, finden Sie im [Branch „WinUI 3 Vorschau 2“ des XAML-Steuerelementekatalogs](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

![XAML-Steuerelementekatalog-App für WinUI 3 Vorschau 2](images/WinUI3XamlControlsGalleryP2.png)<br/>
*Beispiel der XAML-Steuerelementekatalog-App für WinUI 3 Vorschau 2*

Um das Beispiel herunterzuladen, klone den Branch **winui3preview** mit dem folgenden Befehl:

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Vergewissere dich nach dem Klonen, dass du in der lokalen Git-Umgebung zum Branch **winui3preview** wechselst:

```
git checkout winui3preview
```