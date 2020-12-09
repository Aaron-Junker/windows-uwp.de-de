---
title: WinUI 3 Vorschau 3 (November 2020)
description: Übersicht über das Release von WinUI 3 Vorschau 3.
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: 69855aea647b608d9253e4f71b6d7d38917def61
ms.sourcegitcommit: a4ca8ba143862411cd1104515cfeb98f1bcdb780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857421"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Windows-UI-Bibliothek 3 Vorschau 3 (November 2020)

Die Windows-UI-Bibliothek 3(WinUI) ist ein natives Benutzererfahrungs-Framework (User Experience, UX) für Windows-Desktop- als auch UWP-Apps.

**WinUI 3 Vorschau 3** enthält neue und verbesserte Funktionen sowie bedeutende Fehlerbehebungen.

**Weitere Informationen finden Sie unter [Einschränkungen und bekannte Probleme in Vorschau 3](#preview-3-limitations-and-known-issues)** .

> [!Important]
> Diese WinUI 3-Vorschauversion ist für eine frühe Evaluierung und zum Sammeln von Feedback aus der Entwicklercommunity vorgesehen. Diese Version sollte **NICHT** für Produktions-Apps verwendet werden.
>
> Wir werden Vorschauversionen von WinUI 3 weiterhin bis ins Jahr 2021 liefern, gefolgt von der ersten offiziellen Veröffentlichung.
>
> Verwenden Sie das [GitHub-Repository für WinUI](https://github.com/microsoft/microsoft-ui-xaml), um Feedback zu geben und Vorschläge und Probleme zu protokollieren.

## <a name="install-winui-3-preview-3"></a>Installieren von WinUI 3 Vorschau 3

WinUI 3 Vorschau 3 enthält Visual Studio-Projektvorlagen, die Ihnen beim Erstellen von Apps mit einer WinUI-basierten Benutzeroberfläche helfen, sowie ein NuGet-Paket, das die WinUI-Bibliotheken enthält. Um WinUI 3 Vorschau 3 zu installieren, führen Sie die folgenden Schritte aus.

> [!NOTE]
> Sie können auch die WinUI 3-Vorschauversion 3 des [XAML-Steuerelementekatalogs](#xaml-controls-gallery-winui-3-preview-3-branch) klonen und erstellen.

1. Stellen Sie sicher, dass auf Ihrem Entwicklungscomputer mindestens Windows 10, Version 1803 (Build 17134) oder höher installiert ist.

2. Installieren von [Visual Studio 2019, Version 16.9, Vorschau](https://visualstudio.microsoft.com/vs/preview/)

    Bei der Installation von Visual Studio müssen Sie die folgenden Workloads einschließen:
    - NET-Desktop-Entwicklung (dadurch wird auch .NET 5 installiert)
    - Entwicklung für die universelle Windows-Plattform

    Zum Erstellen von C++-Apps müssen Sie außerdem die folgenden Workloads einschließen:
    - Desktopentwicklung mit C++
    - Die optionale Komponente *C++ (v142) UWP-Tools (Universelle Windows-Plattform)* für die UWP-Workload (siehe „Installationsdetails“ im Abschnitt „Entwicklung mit der universellen Windows-Plattform“ im rechten Bereich).
3. Stellen Sie sicher, dass auf Ihrem System eine NuGet-Paketquelle für **nuget.org** aktiviert ist. Weitere Informationen finden Sie unter [Allgemeine NuGet-Konfigurationen](/nuget/consume-packages/configuring-nuget-behavior).

4. Laden Sie das [WinUI 3 Vorschau 3-VSIX-Paket](https://aka.ms/winui3/preview3-download) herunter und installieren Sie es. Dadurch werden Visual Studio 2019 die WinUI 3-Projektvorlagen und das NuGet-Paket mit den WinUI 3-Bibliotheken hinzugefügt.

    Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Suchen nach und Verwenden von Visual Studio-Erweiterungen](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).

#### <a name="webview2"></a>Webansicht2

Wenn Sie das WebView2-Steuerelement in Ihrer App verwenden, installieren Sie die **Dev Channel-Version des Microsoft Edge-Browsers** von [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download). Stellen Sie sicher, dass alle vorhandenen Instanzen von Microsoft Edge Beta, Microsoft Edge Dev und Microsoft Edge WebView2 Runtime deinstalliert werden.

#### <a name="windows-community-toolkit"></a>Windows-Community-Toolkit

Wenn Sie das Windows Community Toolkit verwenden, [laden Sie die neueste Version herunter](https://aka.ms/wct-winui3).

## <a name="create-winui-projects"></a>Erstellen von WinUI-Projekten

Nachdem Sie das WinUI 3 Vorschau 3 VSIX-Paket installiert haben, können Sie mithilfe einer der WinUI-Projektvorlagen in Visual Studio ein neues Projekt erstellen. Um auf die WinUI-Projektvorlagen im Dialogfeld **Neues Projekt erstellen** zuzugreifen, filtern Sie die Sprache auf **C++** oder **C#** , die Plattform auf **Windows** und den Projekttyp auf **WinUI**. Alternativ können Sie nach *WinUI* suchen und eine der verfügbaren C#- oder C++-Vorlagen auswählen.

![WinUI-Projektvorlagen](images/winui-projects-csharp.png)

Weitere Informationen zu den ersten Schritten mit den WinUI-Projektvorlagen finden Sie in den folgenden Artikeln:

- [Erste Schritte mit WinUI 3 für Desktop-Apps](get-started-winui3-for-desktop.md)
- [Erste Schritte mit WinUI 3 für UWP-Apps](get-started-winui3-for-uwp.md)

Abgesehen von den [Einschränkungen und bekannten Problemen](#preview-3-limitations-and-known-issues) ist das Erstellen einer App mithilfe der WinUI-Projekte vergleichbar mit dem Erstellen einer UWP-App mit XAML und WinUI 2.x. Daher lassen sich die meisten [Anleitungsdokumentationen](/windows/uwp/design/) für UWP-Apps und die **Windows.UI** WinRT-Namespaces im Windows SDK anwenden.

Mit dieser Version wurde auch [WinUI 3 API-Referenzdokumentation](/windows/winui/api/) für alle auf WinUI 3 portierten WinRT-APIs hinzugefügt.

Wenn Sie ein Projekt mit WinUI 3 Vorschau 2 erstellt haben, können Sie Ihr Projekt auf die Verwendung von Vorschau 3 upgraden. Im [WinUI GitHub-Repository](https://aka.ms/winui3/upgrade-instructions) finden Sie ausführliche Anleitungen.

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
| Komponente für Windows-Runtime (WinUI) | C++ | Erstellt eine [Komponente für Windows-Runtime](/windows/uwp/winrt-components/), die in C++/WinRT geschrieben ist, die von einer UWP- Desktop-App mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |
| Komponente für Windows-Runtime (UWP) | C# | Erstellt eine [Komponente für Windows-Runtime](/windows/uwp/winrt-components/), die in C# geschrieben ist, die von einer UWP-App mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |

### <a name="item-templates-for-winui-3"></a>Elementvorlagen für WinUI 3

Die folgenden Elementvorlagen stehen für die Verwendung in einem WinUI-Projekt zur Verfügung. Um auf diese WinUI-Elementvorlagen zuzugreifen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Hinzufügen** -> **Neues Element** aus, und klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **WinUI**.

![WinUI-Elementvorlagen](images/winui-items-csharp.png)

| Vorlage | Language | Beschreibung |
|----------|----------|-------------|
| Leere Seite (WinUI) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die eine neue Seite definieren, die von der **Microsoft.UI.Xaml.Controls.Page**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Leeres Fenster (WinUI in Desktop) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die ein neues Fenster definiert, das von der **Microsoft.UI.Xaml.Window**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Benutzerdefiniertes Steuerelement (WinUI) | C# und C++ | Fügt eine Codedatei zum Erstellen eines Steuerelements mit Vorlagen mit einem Standardstil hinzu. Das Steuerelement mit Vorlagen wird von der **Microsoft.UI.Xaml.Controls.Control**-Klasse in der WinUI-Bibliothek abgeleitet.<p></p>Eine exemplarische Vorgehensweise, in der die Verwendung dieser Elementvorlage veranschaulicht wird, finden Sie unter [XAML-Steuerelemente mit Vorlagen für UWP- und WinUI 3-Apps mit C++/WinRT](xaml-templated-controls-cppwinrt-winui-3.md) und [XAML-Steuerelemente in Vorlagen für UWP- und WinUI 3-Apps mit C#](xaml-templated-controls-csharp-winui-3.md). Weitere Informationen zu Steuerelementen mit Vorlagen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Ressourcenverzeichnis (WinUI) | C# und C++ | Fügt eine leere, mit Schlüsseln versehene Sammlung von XAML-Ressourcen hinzu. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Ressourcendatei (WinUI) | C# und C++ | Fügt eine Datei zum Speichern von Zeichenfolgen- und bedingten Ressourcen für Ihre App hinzu. Mithilfe dieses Elements können Sie Ihre App lokalisieren. Weitere Informationen finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im Paketmanifest der App](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Benutzersteuerelement (WinUI) | C# und C++ | Fügt eine XAML- und Codedatei zum Erstellen eines Benutzersteuerelements hinzu, das von der **Microsoft.UI.Xaml.Controls.UserControl**-Klasse in der WinUI-Bibliothek abgeleitet ist. In der Regel kapselt ein Benutzersteuerelement zugehörige vorhandene Steuerelemente und stellt eine eigene Logik bereit.<p></p>Weitere Informationen zu Benutzersteuerelementen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

### <a name="visual-studio-support"></a>Visual Studio-Unterstützung

Um die neuesten, in WinUI 3 Vorschau 3 hinzugefügten Toolfeatures wie Hot Reload, Live Visual Tree und Live Property Explorer nutzen zu können, müssen Sie die neueste Vorschauversion von Visual Studio mit der neuesten WinUI 3 Vorschau verwenden. Die folgende Tabelle zeigt die Kompatibilität zukünftiger Versionen mit WinUI 3 Vorschau 3:

| VS-Version  | WinUI 3 Vorschau 3  |
|---|---|
| 16.8 RTM  | Nein   |
| 16.9-Vorschauen  | Ja  | 
| 16.9 RTM  | Nein   |
| 16.10 Vorschauen  | Ja   |


## <a name="new-features-and-capabilities-in-preview-3"></a>Neue Features und Funktionen in Vorschau 3

- ARM64-Unterstützung
- Drag & Drop innerhalb und außerhalb von Apps
- RenderTargetBitmap (derzeit nur XAML-Inhalte, keine SwapChainPanel-Inhalte)
- Verbesserungen an unserer Tools/Entwicklererfahrung:
  - Live Visual Tree, Hot Reload, Live Property Explorer und ähnliche Tools
  - IntelliSense kann jetzt mit WinUI 3 verwendet werden. 
- Unterstützung für MRT Core
  - Das macht Apps beim Start schneller und effizienter, und beschleunigt die Ressourcensuche.
- Benutzerdefinierte Cursorunterstützung
- Hintergrundthread-Eingabe
- Leistungsverbesserungen seit Vorschau 2
- Mehrere Fenster in Desktop-Apps: verbesserte Unterstützung seit Vorschau 2


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>Neue Features und Funktionen, die in früheren WinUI 3 Vorschauen eingeführt wurden

Die folgenden Features und Funktionen wurden in WinUI 3 Vorschau 1 und 2 eingeführt und werden in WinUI 3 Vorschau 3 weiterhin unterstützt.

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

### <a name="whats-coming-next"></a>Neue Entwicklungen

Werfen Sie einen Blick auf unsere detaillierte [Featureroadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap), um zu sehen, wann spezifische Features in WinUI 3 eingeführt werden. 

## <a name="preview-3-limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme in Vorschau 3

Das Vorschau 3-Release ist, wie der Name schon sagt, lediglich eine Vorschau. Die Szenarien um Desktop-Apps sind ganz neu. Es sind Fehler, Einschränkungen und andere Probleme zu erwarten.

Für WinUI 3 Vorschau 3 sind die folgenden Probleme bekannt. Wenn Sie ein Problem finden, das im Folgenden nicht aufgeführt ist, informieren Sie uns, indem Sie Feedback zu einem vorhandenen oder neuen Problem über das [WinUI-GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) bereitstellen.

### <a name="platform-and-os-support"></a>Plattform- und Betriebssystemunterstützung

WinUI 3 Vorschau 3 ist mit PCs kompatibel, auf denen mindestens das Windows 10-Update vom April 2018 (Version 1803, Build 17134) ausgeführt wird.

### <a name="developer-tools"></a>Entwicklertools

- Es werden nur C#- und C++ /WinRT-Apps unterstützt.
- Desktop-Apps unterstützen .NET 5 und C# 9 und müssen in einer MSIX-App gepackt sein.
- UWP-Apps unterstützen .NET Native und C# 7.3
- Entwicklertools und IntelliSense funktionieren in Visual Studio Version 16.8 möglicherweise nicht ordnungsgemäß.
- Keine Unterstützung für XAML-Designer
- Neue C++/CX-Apps werden nicht unterstützt. Deine bestehenden Apps funktionieren jedoch weiterhin (steige so schnell wie möglich auf C++/WinRT um)
- Die Unterstützung mehrerer Fenster in Desktop-Apps ist in Entwicklung, aber noch nicht abgeschlossen und stabil.
  - Wenn Sie neue Probleme oder Regressionen mit dem Verhalten mehrerer Fenstern finden, melden Sie diese Fehler in unserem Repository.
- Nicht gepackte Desktopbereitstellung nicht unterstützt
- Wenn Sie eine Desktop-App mit F5 ausführen, stellen Sie sicher, dass Sie das Paketprojekt ausführen. Wenn Sie F5 für das App-Projekt drücken, wird eine ungepackte App ausgeführt, die WinUI 3 noch nicht unterstützt.

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

### <a name="known-issues"></a>Bekannte Probleme

- Alt+F4 schließt Desktop-App-Fenster nicht.

-   Wenn Sie ein WebView2-Element in Ihrer Anwendung verwenden, dieses aber nicht gerendert oder geladen wird, führen Sie möglicherweise eine inkompatible Version des Microsoft Edge-Browsers aus. [Laden Sie die Dev Channel-Version von Microsoft Edge herunter](https://www.microsoftedgeinsider.com/en-us/download), und stellen Sie sicher, dass alle vorhandenen Instanzen von Microsoft Edge Beta, Microsoft Edge Dev und Microsoft Edge WebView2 Runtime deinstalliert werden.

- Marshal-Funktionen sollten in .NET 5 WinUI-Anwendungen nicht verwendet werden, da sie nicht korrekt mit C#/WinRT interagieren. Weitere Informationen hierzu finden Sie [auf dieser Seite](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md).

- Wenn bei der Festlegung einer URI-Eigenschaft ein Absturz auftritt, z. B. bei Verwenden eines `{Binding}`-Objekts in XAML-Markup, können Sie dies mit `{x:Bind}` oder durch Verwenden einer Vorschauversion von C#/WinRT umgehen. Fügen Sie dazu die folgenden Zeilen in Ihre CSPROJ-Datei ein:

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- Für C# UWP-Apps:

  Das WinUI 3-Framework ist ein Satz von WinRT-Komponenten, die aus C++ (mit C++ /WinRT) oder C# verwendet werden können. Bei Verwendung von C# gibt es je nach App-Modell zwei Versionen von .NET: Bei Nutzung von WinUI 3 in einer UWP-App wird .NET Native verwendet, in einer Desktop-App wird .NET 5 (und C#/WinRT) verwendet.

  Bei Verwendung von C# für eine WinUI 3-App in UWP gibt es einige API-Namespaceunterschiede im Vergleich zu C# in einer WinUI 3-Desktop-App oder einer C# WinUI 2-App: Einige Typen befinden sich in einem `Microsoft`-Namespace statt in einem `System`-Namespace. Die `INotifyPropertyChanged`-Schnittstelle befindet sich beispielsweise nicht im `System.ComponentModel`-Namespace, sondern im `Microsoft.UI.Xaml.Data`-Namespace. 

  Dies gilt für:
    - `INotifyPropertyChanged` (und verwandte Typen)
    - `INotifyCollectionChanged`
    - `ICommand`

  Die `System`-Namespaceversionen sind noch vorhanden, können jedoch nicht mit WinUI 3 verwendet werden. Dies bedeutet, dass `ObservableCollection` in der vorliegenden Form nicht in WinUI 3 C# UWP-Apps funktioniert. Eine Problemumgehung finden Sie im [CollectionsInterop-Beispiel](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs) im [Beispiel zum XAML-Steuerelementekatalog](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML-Steuerelementekatalog (Branch „WinUI 3 Vorschau 3“)

Eine Beispiel-App, die alle WinUI 3 Vorschau 3-Steuerelemente und -Features enthält, finden Sie im [Branch „WinUI 3 Vorschau 3“ des XAML-Steuerelementekatalogs](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

![XAML-Steuerelementekatalog-App für WinUI 3 Vorschau 3](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
*Beispiel der XAML-Steuerelementekatalog-App für WinUI 3 Vorschau 3*

Um das Beispiel herunterzuladen, klone den Branch **winui3preview** mit dem folgenden Befehl:

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Vergewissere dich nach dem Klonen, dass du in der lokalen Git-Umgebung zum Branch **winui3preview** wechselst: 

```
git checkout winui3preview
```
