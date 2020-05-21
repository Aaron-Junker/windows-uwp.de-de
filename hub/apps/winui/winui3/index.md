---
title: WinUI 3.0 Vorschau 1 (Mai 2020)
description: Übersicht über die Vorschauversion von WinUI 3.0.
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 9141fe7ff215f28557020c7f76dd35c3560edfe5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580137"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Windows-UI-Bibliothek 3.0 Vorschau 1 (Mai 2020)

Die Windows-UI-Bibliothek (WinUI) 3.0 ist ein größeres Update, das WinUI in ein vollständiges UX-Framework für alle Arten von Windows-Apps – von Win32 bis UWP – umgestaltet.

> [!Important]
> Die WinUI 3.0-Vorschauversion ist für eine frühe Evaluierung und zum Sammeln von Feedback von der Entwicklercommunity vorgesehen. Diese Version darf **NICHT** für Produktions-Apps verwendet werden.
>
> **Weitere Informationen findest du unter [Einschränkungen und bekannte Probleme in Vorschauversion 1](#preview-1-limitations-and-known-issues)** .
## <a name="new-features-in-winui-30-preview-1"></a>Neue Features in WinUI 3.0 Version 1

- Möglichkeit zum Erstellen von Desktop-Apps mit WinUI, einschließlich [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) für Win32-Apps
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView-Aktualisierungen](/windows/uwp/design/controls-and-patterns/tab-view)
- Aktualisierungen am dunklen Design
- Verbesserungen und Aktualisierungen für [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2)
  - Unterstützung hoher DPI-Werte
  - Unterstützung des Änderns der Größe und Verschiebens von Fenstern
  - Für eine neuere Version von Edge aktualisiert
  - Nicht mehr erforderlich, um auf ein WebView2-spezifisches NuGet-Paket zu verweisen
- SwapChainPanel
- Erforderliche Verbesserungen für Open-Source-Migration

Weitere Informationen zu den Vorteilen von WinUI 3.0 und die WinUI-Roadmap findest du auf GitHub unter [Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

### <a name="provide-feedback-and-suggestions"></a>Bereitstellen von Feedback und Vorschlägen

Wir freuen uns über dein Feedback im [GitHub-Repository von WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="try-winui-30-preview-1"></a>Testen von WinUI 3.0 Vorschau 1

Konfiguriere deine Entwicklungsumgebung (detaillierte Anweisungen findest du unter [Konfigurieren der Entwicklungsumgebung](#configure-your-dev-environment)). Installiere die VSIX „WinUI 3.0 Vorschau 1“ über den folgenden Link, und probiere die WinUI 3.0-Projektvorlagen aus.

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

Du kannst auch die WinUI 3.0-Vorschauversion 1 im [XAML-Steuerelementkatalog](#xaml-controls-gallery-winui-30-preview-1-branch) klonen und einen Build dafür ausführen.

### <a name="configure-your-dev-environment"></a>Konfigurieren der Entwicklungsumgebung

Stelle sicher, dass auf deinem Entwicklungscomputer mindestens Windows 10 mit Update vom April 2018 (Version 1803, Build 17134) installiert ist.

Installiere Visual Studio 2019, Version 16.7 Vorschau 1. Du kannst diese Version von [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview) herunterladen.

Bei der Installation von Visual Studio Vorschau musst du die folgenden Workloads einschließen:

- Win32-Entwicklung mit .NET
- Entwicklung für die universelle Windows-Plattform

Zum Erstellen von C++-Apps musst du auch die folgenden Workloads einschließen:

- Win32-Entwicklung mit C++
- Die optionale Komponente *C++ (v142) UWP-Tools (Universelle Windows-Plattform)* für die UWP-Workload

### <a name="visual-studio-project-templates"></a>Visual Studio-Projektvorlagen

Um auf WinUI 3.0 Vorschau 1 und Projektvorlagen zuzugreifen, gehe zu **https://aka.ms/winui3/previewdownload**

Lade die Visual Studio-Extension (`.vsix`) herunter, um die WinUI-Projektvorlagen und das NuGet-Paket zu Visual Studio 2019 hinzuzufügen.

Anweisungen zum Hinzufügen von `.vsix` zu Visual Studio findest du unter [Suchen nach und Verwenden von Visual Studio-Erweiterungen](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).

Nachdem du die `.vsix`-Erweiterung installiert hast, kannst du ein neues WinUI 3.0-Projekt erstellen, indem du nach „WinUI“ suchst und dann eine der verfügbaren C#- oder C++-Vorlagen auswählst.

![WinUI 3.0 Visual Studio-Vorlagen](images/WinUI3Templates.png)<br/>
*Beispiel für die WinUI 3.0 Visual Studio-Vorlagen*

### <a name="visual-studio-project-configuration"></a>Visual Studio-Projektkonfiguration

Wenn du ein Projekt mit einer der WinUI 3.0 Vorschau 1-Vorlagen erstellst, lege die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest.

Um diese Werte zu ändern, nachdem du ein Projekt erstellt hast, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wähle **Eigenschaften** aus.

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>Erstellen einer Win32-Desktop-App mithilfe von WinUI 3.0 Vorschau 1

Weitere Informationen findest du unter [Erste Schritte mit WinUI 3.0 für Desktop-Apps](get-started-winui3-for-desktop.md).

Abgesehen von den Einschränkungen und bekannten Problemen, die nachstehend beschrieben werden, entspricht das Entwickeln einer App mithilfe von WinUI 3.0 Vorschau 1 sehr ähnlich dem Entwickeln einer UWP-App mit XAML und WinUI 2.x. Daher gelten die meisten Anleitungen und die Dokumentation für UWP-Apps und `Windows.UI`-APIs.

## <a name="preview-1-limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme in Vorschauversion 1

Das Vorschau 1-Release ist, wie der Name schon sagt, eine Vorschau. Die Szenarien für Win32-Desktop-Apps sind ganz neu, weshalb Fehler, Einschränkungen und Probleme zu erwarten sind .

Für WinUI 3.0 Vorschau 1 sind die folgenden Probleme bekannt. Wenn du ein Problem findest, das im Folgenden nicht aufgeführt ist, informiere uns, indem du Feedback zu einem vorhandenen oder neuen Problem im [WinUI-GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose) bereitstellst.

### <a name="platform-and-os-support"></a>Plattform- und Betriebssystemunterstützung

WinUI 3.0 Vorschau 1 ist mit PCs kompatibel, auf denen mindestens das Windows 10-Update vom April 2018 (Version 1803, Build 17134) ausgeführt wird.

### <a name="developer-tools"></a>Entwicklungstools

- Desktop-Apps unterstützen .NET 5 und C# 8 und müssen gepackt werden
- UWP-Apps unterstützen .NET Native und C# 7.3
- IntelliSense ist unvollständig
- Kein visueller Designer
- Kein Hot Reload
- Keine visuelle Livestruktur
- Entwicklung mit VS Code noch nicht unterstützt
- Neue C++/CX-Apps werden nicht unterstützt. Deine bestehenden Apps funktionieren jedoch weiterhin (steige so schnell wie möglich auf C++/WinRT um)
- WinUI 3.0-Inhalte können sich nur in einem Fenster pro Prozess oder in einer ApplicationView pro App befinden
- Nicht gepackte Desktopbereitstellung nicht unterstützt
- Keine ARM64-Unterstützung

### <a name="missing-platform-features"></a>Fehlende Plattformfeatures

- Keine Unterstützung für Xbox
- Keine Unterstützung für HoloLens
- Keine Unterstützung für Popups in Fenstern
- Keine Unterstützung für Freihandeingaben
- Hintergrund-Acryl
- MediaElement und MediaPlayerElement
- RenderTargetBitmap
- MapControl
- Hierarchische Navigation mit NavigationView
- SwapChainPanel unterstützt keine Transparenz
- In C# musst du `WinRT.WeakReference<T>` anstelle von `System.WeakReference<T>`verwenden
- Global Reveal nutzt Fallbackverhalten, einen einfarbigen Pinsel
- XAML Islands werden in diesem Release nicht unterstützt
- Bibliotheken aus dem Ökosystem von Drittanbietern funktionieren nicht in vollem Umfang
- IMEs funktionieren nicht
- Methoden für den Namespace Windows.UI.Text können nicht aufgerufen werden
  
## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>XAML-Steuerelementkatalog (Branch „WinUI 3.0 Vorschau 1“)

Eine Beispiel-App, die alle WinUI 3.0 Vorschau 1-Steuerelemente und -Features enthält, findest du im [Branch „WinUI 3.0 Vorschau 1“ des XAML-Steuerelementkatalogs](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview1).

![XAML-Steuerelementkatalog-App für WinUI 3.0 Vorschau 1](images/WinUI3XamlControlsGallery.png)<br/>
*Beispiel der XAML-Steuerelementkatalog-App für WinUI 3.0 Vorschau 1*

Um das Beispiel herunterzuladen, klone den Branch **winui3preview1** mit dem folgenden Befehl:

> `git clone --single-branch --branch winui3preview1 https://github.com/microsoft/Xaml-Controls-Gallery.git`

Vergewissere dich nach dem Klonen, dass du in der lokalen Git-Umgebung zum Branch **winui3preview1** wechselst:

> `git checkout winui3preview1`
