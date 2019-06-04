---
title: Neuigkeiten für Entwickler in Windows 10
description: Windows 10, build 18362 und neue Entwicklertools bereitzustellen, die Tools, Funktionen und Oberflächen, die von der universellen Windows-Plattform unterstützt.
keywords: was neu ist, was neu, Update, Updates, neue features, können Sie Windows 10, neueste, Entwickler, 18362,
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 935d7f787d0cc23965c0fd51747b7687adb80a3f
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468315"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>Was ist neu in Windows 10 für Entwickler, Build 18362

Windows 10 Build 18362 (auch bekannt als SDK Version 1903 sein), in Kombination mit Visual Studio-2019, bietet die Tools, Funktionen und Erfahrungen zu bemerkenswerten Windows-apps. Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

Dies ist eine Sammlung von neuen und verbesserten Features und Richtlinien, die in dieser Version für Windows-Entwickler interessant sind. Eine vollständige Liste der neuen Namespaces, die dem Windows SDK hinzugefügt werden soll, finden Sie unter den [18362 API-Änderungen für Windows 10, build](windows-10-build-18362-api-diff.md). Weitere Informationen zu den Highlights von Windows 10 finden Sie unter [Die Highlights in Windows 10](https://go.microsoft.com/fwlink/?LinkId=823181).

## <a name="design--ui"></a>Design und UI

Feature | Beschreibung
:------ | :------
AnimatedVisualPlayer | Die [AnimatedVisualPlayer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer) -API hostet, und die Wiedergabe von animierten Visuals in Ihre app steuern. Diese API wird verwendet, um zu steuern und Anzeigen von Inhalt wie [Lottie](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) visuelle Objekte, die Ihnen ermöglichen, Adobe AfterEffects Animationen systemintern in Ihren Anwendungen zu rendern.
CompactDensity | Aktivieren der [Komprimierungsmodus](../design/style/spacing.md) in Ihrer app können Sie Dichte, informationsreiche eine Gruppe von Steuerelementen. Dies kann helfen, Durchsuchen Sie große Mengen an Inhalt, den sichtbaren Inhalt auf einer Seite maximieren oder Navigation und Interaktion unterstützen, wenn der Benutzer den Zeiger-Eingabe verwendet wird.
Elemente Repeater | Ein [ItemsRepeater](../design/controls-and-patterns/items-repeater.md) Steuerelement können Kreatur eine benutzerdefinierte Oberfläche für die Anzeige von Sammlungen für Ihre Benutzer. ItemsRepeater bietet eine umfassende Benutzeroberfläche oder einen Standardwert UI keine. Stattdessen ist es ein Baustein, den Sie verwenden können, um eigene eindeutige Auflistung basierende Funktionen und benutzerdefinierte Steuerelemente zu erstellen.
Unterrichtstipp | Ein [unterrichten Tipp](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md) ist ein teilweise dauerhaft ist und umfangreicher Flyout, das Kontextinformationen bereitstellt. Sie können dieses Steuerelement verwenden, für die darüber informiert, daran erinnert und spezialisiert, Benutzer zu neuen oder wichtige Funktionen.
UI-Befehle | Mit [Befehle in UWP-apps](../design/controls-and-patterns/commanding.md), verwenden Sie die [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) und [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (zusammen mit der die ICommand-Schnittstelle) freigeben und Verwalten von Befehlen auf verschiedene Klassen Steuerelementtypen Sie, unabhängig vom Gerät und die Eingabe verwendet wird.
Windows-UI-Bibliothek | Die offizielle neueste Version der Windows UI-Bibliothek – [WinUI 2.1](https://docs.microsoft.com/uwp/toolkits/winui/release-notes/winui-2.1) – bietet lebendige neue XAML-Steuerelemente für Ihre Windows-app. WinUI-Bibliotheks-APIs werden auch unter früheren Versionen von Windows 10 ausgeführt. Sie müssen daher keine Versionsüberprüfungen oder bedingte XAML-Befehle verwenden, um Benutzer zu unterstützen, die nicht das neueste Betriebssystem einsetzen.
Visuelle Ebene in Desktop-apps | Sie können jetzt [die UWP-visuellen Ebene APIs in desktop-Anwendungen verwenden](../composition/visual-layer-in-desktop-apps.md). Diese APIs bieten leistungsstarke neu trainiert Mode-API für Grafiken, Animationen und Auswirkungen, und bilden die Grundlage für die Benutzeroberfläche für Windows-Geräte.
Z-Tiefe und Schatten | Verwendung [Z-Tiefe und Schatten](../design/layout/depth-shadow.md) Erhöhung der Rechte in Ihre UWP-app zu erstellen. Diese neue Funktion können Sie die UI Ihrer app zu überprüfen leichter, und Sie besser vermittelt, was für Ihre Benutzer konzentrieren wichtig ist.

## <a name="develop-windows-apps"></a>Entwickeln von Windows-Apps

Feature | Beschreibung
:------ | :------
Antimalware Scan Interface (AMSI) | Erfahren Sie, [wie Antimalware Scan Interface (AMSI) können Sie die Verteidigung gegen Malware](https://docs.microsoft.com/windows/desktop/amsi/how-amsi-helps), sehen Sie sich die [Beispielcode](https://docs.microsoft.com/windows/desktop/amsi/dev-audience) erfahren, wie Sie es in Ihrer Desktop-app zu implementieren.
C++/WinRT 2.0 | Version 2.0 von C++/WinRT wurde freigegeben. Sehen Sie sich [neuerungen in C++"/ WinRT"](../cpp-and-winrt-apis/news.md) für eine umfassende Sammlung an die neue Änderungen und Ergänzungen.
Auswählen Ihrer Plattform | Eine neue desktop-Anwendung erstellen möchten? Sehen Sie sich unsere überarbeitete [wählen Sie Ihre Plattform](https://docs.microsoft.com/windows/desktop/choose-your-technology) Seite ausführliche Beschreibungen und Vergleichen der UWP, WPF und Windows Forms-Plattformen und Weitere Informationen zu den Win32-API.
Conversational-agent | Die [Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent) Namespace können Sie die digitale Unterstützung unterstützt werden, von der Windows-Plattform-Agent-Aktivierung Common Language Runtime (AAR) zu Ihrer Windows-app hinzufügen.
Cloud-Dateien-API | Die *Dateien-API cloud* ermöglicht es Ihnen, [erstellen Sie eine Cloud Synchronisierungs-Engine, die Platzhalterdateien unterstützt](https://docs.microsoft.com/windows/desktop/cfapi/build-a-cloud-file-sync-engine).
Direct3D 12-Beschleunigung | [Direct3D 12 Rendering-Durchläufe](/windows/desktop/direct3d12/direct3d-12-render-passes) können Verbessern der Leistung von Ihrem Renderer, wenn davon Kachel-basierte verzögerte Rendering (TBDR), zu anderen Techniken. Die Technik hilft Ihrem Renderer GPU-Effizienz steigern, indem ermöglicht Ihrer Anwendung, die Ressource Rendering-Reihenfolge von Anforderungen und datenabhängigkeiten besser zu erkennen. Dies reduziert die Arbeitsspeicher-Datenverkehr in und aus-Chip Arbeitsspeicher.
Leiten Sie Machine Learning (DirectML) | [DirectML](https://docs.microsoft.com/windows/desktop/direct3d12/dml) ist eine Low-Level-Hardwarebeschleunigung-API für Machine Learning. Hat eine vertraute (native C++, Nano-COM)-Schnittstelle und Workflow in den Stil der DirectX 12-Programmierung. Sie können Machine Learning integrieren Rückschlüsse-Workloads in Ihr Spiel-Engine, Middleware, Back-End- oder anderen Anwendungen. DirectML wird von allen DirectX 12-kompatibler Hardware unterstützt.
DirectX HLSL | [HLSL-Shader Model 6.4](https://docs.microsoft.com/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) neuen Machine Learning-intrinsischen Funktionen für die Verwendung mit DirectML enthält.
Treiberentwicklung | Neues Audio, Kamera, Anzeige, Netzwerke, mobile Breitband-, drucken, Sensor, Speicher und WLAN-Funktionen wurden für Entwickler von Windows-Treiber hinzugefügt. Sehen Sie sich [Neuigkeiten in Treiberentwicklung](https://docs.microsoft.com/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest) Weitere Details.
Dateisystemvorgänge | Dies [Leitfaden mit bewährten Methoden](../files/best-practices-for-writing-to-files.md) können Sie nutzen die Windows.Storage.FileIO und Windows.Storage.PathIO-Klassen für Datei e/a-Vorgänge.
Interaktionen mit Gamepad und Fernbedienung | Verwendung [Gamepad und Remotesteuerung Interaktionen](../design/input/gamepad-and-remote-interactions.md) Umgebungen verwendet werden und zugänglich Interaktion zu erstellen. Mit diesen Interaktionen kann Ihre Anwendung so intuitiv und einfach zu verwenden, aus zwei Fuß entfernt werden, unverändert aus zehn Fuß entfernt.
Japanische Zeitraum ändern | Wir haben bereitgestellt [diese Anweisungen](../design/globalizing/japanese-era-change.md) soll veranschaulicht werden, wie Sie sicherstellen, dass die Windows-Anwendung ist bereit für die japanische Zeitraum ändern auf 1. Mai 2019 erfolgen soll. [Auf dieser Seite finden Sie auch in Japanisch](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).
Open Sie-Source-WinUI, Windows Forms und WPF | Die WPF, Windows Forms und WinUI UX-Frameworks sind jetzt für Open-Source-Beiträge auf GitHub verfügbar. Weitere Informationen und Links finden Sie unter den [erstellen Windows-apps-Blog](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).
Progressive Web-Apps für Xbox | Mit [Progressive Web-Apps für Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), Sie können Erweitern einer Webanwendung und verfügbar zu machen als Xbox One-app über den Microsoft Store und trotzdem weiterhin verwenden, Ihre vorhandenen Frameworks, CDN und Server-Back-End. Zum größten Teil, können Sie Ihre PWA für Xbox One auf die gleiche Weise packen, die für Windows. Dieses Handbuch wird führen Sie durch den Prozess, und markieren Sie die wichtigsten Unterschiede.
Project Rome | Das Projekt "ROME"-SDK ist jetzt für Android und iOS verfügbar. Erfahren Sie, wie Sie Benachrichtigungen für Graph in jede Plattform integrieren: [Android](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-android) und [iOS](https://docs.microsoft.com/windows/project-rome/notifications/how-to-guide-for-ios).
Remotekameras | Verwenden Sie die DeviceWatcher-Klasse, um [remotekameras Verbindung](../audio-video-camera/connect-to-remote-cameras.md), und Lesen von Frames aus diesen Kameras in Ihrer Windows-app.
UWP-Steuerelementen in desktop-Anwendungen (XAML-Inseln) | Die im Windows SDK für UWP-Steuerelementen in Windows Forms, WPF-hosting-APIs und C++ Win32-desktop-Anwendungen nicht mehr in einer entwicklervorschauversion sind. Weitere Informationen finden Sie unter [UWP-Steuerelementen in desktopanwendungen](../xaml-platform/xaml-host-controls.md).
Visual Studio 2019 | Visual Studio-2019 wurde, mit den neuesten Tools und Dienste für Entwickler, Apps oder Platform veröffentlicht. Sehen Sie sich [Neuigkeiten in Visual Studio-2019](https://docs.microsoft.com/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019) um mehr zu erfahren und um zu beginnen.
Win32 WebView | Unsere [häufig gestellte Fragen](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) finden Sie Antworten auf häufig gestellte Fragen bereitstellen, wenn Sie die Microsoft Edge-WebView in desktop-Anwendungen sowie Links zu Beispielen und zusätzliche Ressourcen verwenden.
Windows-Befehlszeile | [Neuen Konsolenfeatures](https://devblogs.microsoft.com/commandline/new-experimental-console-features/) die experimentelle Terminal Registerkarte mit den Einstellungen für Bildlauf, Cursor-Form und Cursor Farben enthalten. Erfahren Sie mehr über die [Windows Command Line-Tools für Entwickler-Blog](https://devblogs.microsoft.com/commandline/).
Windows-Community Toolkit | Windows-Community-Toolkit V5. 1 bietet interessante Updates für Animationen, Remotegeräte, Bild zuschneiden und Barrierefreiheit. </br> • Die neue [Lottie-Windows-Bibliothek](https://docs.microsoft.com/windows/communitytoolkit/animations/lottie) bietet Unterstützung für hochwertige Animationen auf Windows 10 (1809) durch die Verwendung der Windows.UI.Composition-APIs und ermöglicht die Nutzung der [Bodymovin](https://aescripts.com/bodymovin/) JSON-Dateien oder optimiert für die Wiedergabe in Ihren Windows-apps vom Code generierten Klassen an. Das neue [Lottie-Viewer-app](https://aka.ms/lottieviewer) aus dem Microsoft Store zum Testen von Animationen und optimierten Code für Ihre Windows-apps zu generieren. </br> • Die neue [Remote Gerät Auswahl](https://docs.microsoft.com/windows/communitytoolkit/controls/remotedevicepicker) ermöglicht es einem Benutzer ein Gerät aus (proximally oder in der Cloud zugegriffen werden kann), starten Sie eine app auf dem Gerät oder mit app-Diensten auf dem Remotegerät zu kommunizieren. </br> • Die neue [ImageCropper Steuerelement](https://docs.microsoft.com/windows/communitytoolkit/controls/imagecropper) integriert Zuschneiden Funktionen für die Auswahl von Profilbilder oder für die Verwendung von Tools zum Bearbeiten von Fotos. </br> • Darüber hinaus gab es Verbesserungen der Barrierefreiheit für die Steuerelemente, eine [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0 (Vorschau) paketupdate für WPF und WinForms und weitere Funktionen, die Sie sich vertraut, in machen können der [Anmerkungen zu dieser Version ](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0).
Windows Machine Learning | Wir haben neu entworfen, die Windows-KI-Dokumentation, die in drei Bereiche aufteilen: Windows-Machine Learning-(WinML), Windows Fähigkeiten für maschinelles sehen und Machine Learning (DirectML) weiterzuleiten. Sehen Sie sich die neue [Angebotsseite](https://docs.microsoft.com/windows/ai/) </br> • Die [ *MLGen* auftreten](https://docs.microsoft.com/windows/ai/mlgen) ist in Visual Studio ändern. In Windows 10, Version 1903 und höher, *Mlgen* ist nicht mehr in das Windows 10 SDK enthalten. Wenn Sie Visual Studio 2017 verwenden, Sie sollten stattdessen herunterladen und Installieren der Visual Studio-Erweiterung [Windows Machine Learning-Code-Generator VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen). Wenn Sie Visual Studio-2019 verwenden, müssen Sie installieren die [Windows Machine Learning-Codegenerator](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2) Erweiterung. </br> • Wir freuen ebenfalls neue Unterstützung für das Packen von Gewichtung bekanntgeben zu können. Entwickler nun können weiter verringern, den Datenträgerbedarf seiner ML-Modelle mithilfe einer Technik namens Gewichtung packen, verfügbar über den [WinMLTools Konverter](https://docs.microsoft.com/windows/ai/convert-model-winmltools).
Konsolidierte WinRT-Verweis | Wir haben die vollständige Beschreibung des hinzugefügt der [WinRT-Typsystem](https://docs.microsoft.com/uwp/winrt-cref/winrt-type-system) und [WinMD-Dateien](https://docs.microsoft.com/uwp/winrt-cref/winmd-files), um ausführliche Hinweise zu den Definitionen über die Struktur der WinRT-APIs zu gewähren.
Windows-Subsystem für Linux (WSL) | [Neue Updates für WSL](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/) bieten die Möglichkeit, Windows, die mit Datei-Explorer, und einige neue Befehle für wsl.exe und wslconfig.exe Linux-Dateien zugreifen.
Fähigkeiten für maschinelles Sehen unter Windows | [Windows-maschinelles sehen-Fähigkeiten](https://docs.microsoft.com/windows/ai/windows-vision-skills) ist eine Reihe von APIs, mit der Sie "Fähigkeiten," wie gesichtserkennung, erstellen und Packen sie Sie als NuGet-Paket, die anderen apps nutzen können, ohne auch Modelle des maschinellen Lernens enthalten,.

## <a name="publish--monetize-windows-apps"></a>Veröffentlichen und Monetarisieren von Windows-Apps

Feature | Beschreibung
 :------ | :------
MSIX | [MSIX-Unterstützung unter Windows 10 1709 und 1803-builds](https://docs.microsoft.com/windows/msix/msix-1709-and-1803-support) wird beschrieben, welche MSIX-Features in Versionen vor Windows 10, Version 1809 unterstützt werden.
MSIX-Verpackung und -Bereitstellung | Mehrere vorgestellt [Verbesserungen im Zusammenhang mit der Änderung Pakete](https://docs.microsoft.com/windows/msix/modification-package-insider-preview-build-18312) zum Paket Anpassungen in einem Paket MSIX erleichtern. Diese Verbesserungen umfassen die neue **Rescap6:ModificationPackage** -Element im Paketmanifest, die Möglichkeit, eine Datei in das Hauptpaket mit einem Änderungspaket zu überschreiben und die Möglichkeit, ein Dateisystem-Paket basierend-Plug-in als MSIX Änderung-Paket.
MSIX-Verpackungstool | • Wir haben hinzugefügt [Unterstützung für die Konvertierung auszuführen, auf einem Remotecomputer](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup). Wir außerdem die [MSIX Packaging Tool Insider-Programm](https://docs.microsoft.com/windows/msix/packaging-tool/insider-program) vorabzugriff auf neue Tool-Funktionen anbieten. </br> • [MSIX-Paket-Unterstützung in 1709 und höher](https://docs.microsoft.com/windows/msix/packaging-tool/support-on-1709-and-later) enthält Anleitungen zur Verwendung von das MSIX Packaging Tool zum Erstellen von Paketen speziell für Windows 10, Version 1709 und 1803. </br> • [MSIX Packaging-Umgebung auf Hyper-V-Schnellerfassung](https://docs.microsoft.com/windows/msix/packaging-tool/quick-create-vm) zeigt, wie eine virtuelle Umgebung für MSIX Packaging-Projekte erstellen. </br> • [Bundle MSIX Pakete](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages) enthält Anweisungen zum Erstellen eines Pakets mit dem MSIX Packaging-Tool. </br> • [Änderung-Pakete auf Windows 10, Version 1809](https://docs.microsoft.com/windows/msix/modification-package-1809-update) enthält Anweisungen zum Erstellen eines Pakets Änderungen für Windows 10, Version 1809 und höher Versionen, die mit dem MSIX Packaging Tool und MakeApp.exe.
MSIX-SDK | [Verwenden der MSIX-SDK zum Erstellen eines Pakets für die plattformübergreifende Verwendung](https://docs.microsoft.com/windows/msix/msix-sdk/sdk-guidance), und erfahren, wie Sie die Zielplattformen geben an, die Pakete extrahiert werden sollen.

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft-Learn bietet neue praktisches lernen und schulungsmöglichkeiten für Microsoft-Entwickler an.

* Wenn Sie lernen, wie Sie die Entwicklung von Windows-apps interessieren, lesen Sie [unserer neuen Lernpfad](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) eine umfassende Einführung in die Plattform, die Tools und wie Sie Ihre ersten einige apps zu schreiben.

* Möchten Sie erfahren, wie Sie die Funktionen der Benutzeroberfläche Ihrer Windows-app hinzufügen? Erfahren Sie, wie Sie [Erstellen einer Benutzeroberfläche](https://docs.microsoft.com/learn/modules/create-ui-for-windows-10-apps/), [Hinzufügen von Navigation und Medien auf der Benutzeroberfläche](https://docs.microsoft.com/learn/modules/enhance-ui-of-windows-10-app/), oder [Datenbindung implementieren](https://docs.microsoft.com/learn/modules/implement-data-binding-in-windows-10-app/).

* Wenn Sie die Web-Entwicklung interessiert sind, sehen Sie sich [Entwickeln von Webanwendungen mit Visual Studio Code](https://docs.microsoft.com/learn/modules/develop-web-apps-with-vs-code/) oder [erstellen Sie eine einfache Website](https://docs.microsoft.com/learn/modules/build-simple-website/).

* Alternativ können Sie gerne suchen [alle Windows-Entwickler Module auf der Microsoft-Learn](https://docs.microsoft.com/learn/browse/?products=windows&resource_type=module).

## <a name="videos"></a>Videos

### <a name="progressive-web-apps"></a>Progressive Web Apps

Progressive Web-Apps sind Websites, die wie systemeigene apps für andere Browser und eine Vielzahl von Windows 10-Geräten funktionieren. [Das Video](https://youtu.be/ugAewC3308Y) Weitere Informationen, und klicken Sie dann [Dokumente](https://aka.ms/Windows-PWA) für den Einstieg.

### <a name="vs-code-series"></a>VS Code-Serie

Sehen Sie sich unsere [neue Videoreihe auf Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) Informationen zu den neuerungen von VSCode, wie Sie es verwenden und wie es erstellt wurde.

### <a name="mixed-reality-services"></a>Mixed Reality-Dienste

HoloLens 2 wurde vor kurzem angekündigt. Finden Sie in diesem [Videoreihe auf Mixed Reality](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST) für die neuesten Informationen und wie Sie beteiligen und mit der Entwicklung beginnen können.

### <a name="one-dev-question"></a>Eine Frage für Entwickler

In der Videoreihe Dev Frage behandeln langjährigen Microsoft-Entwicklern eine Reihe von Fragen zu Windows-Entwicklung, Teams und Verlauf.

* [Raymond Chen über Windows-Entwicklung und Verlauf](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7)

* [Larry Osterman auf Windows-Entwicklung und Verlauf](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K)

* [Aaron Gustafsonschem für Progressive Web-Apps](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I)

* [Christian Heilmann im Webhint-Tool](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v)
