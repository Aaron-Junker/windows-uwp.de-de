---
title: Neuigkeiten in Windows 10, Build 18362
description: Windows 10, Build 18362, und neue Entwicklertools stellen Tools, Features und Umgebungen zur Verfügung, die durch die neue Universelle Windows-Plattform unterstützt werden.
keywords: Windows 10, 18362, 1903
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bdd003445bed4ad59b21e0b744281651c30bf04f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166884"
---
# <a name="whats-new-in-windows-10-for-developers-build-18362"></a>Neuigkeiten für Entwickler in Windows 10, Build 18362

Windows 10, Build 18362 (auch bekannt als SDK Version 1903) stellt, in Kombination mit Visual Studio-2019, die Tools, Funktionen und Oberflächen bereit, mit denen bemerkenswerte Windows-Apps erstellt werden können. Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

Dies ist eine Sammlung von neuen und verbesserten Features und Richtlinien, die in dieser Version für Windows-Entwickler interessant sind. Eine vollständige Liste mit neuen Namespaces, die dem Windows SDK hinzugefügt wurden, finden Sie in den [API-Änderungen unter Windows 10, Build 18362](windows-10-build-18362-api-diff.md). Weitere Informationen zu den Highlights von Windows 10 finden Sie unter [Die Highlights in Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="design--ui"></a>Design und Benutzeroberfläche

Feature | Beschreibung
:------ | :------
AnimatedVisualPlayer | Die [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)-API hostet und steuert die Wiedergabe von animierten visuellen Darstellungen in Ihrer App. Über diese API können Inhalte wie visuelle [Lottie](/windows/communitytoolkit/animations/lottie)-Objekte gesteuert und angezeigt werden, mit denen Sie Adobe AfterEffects-Animationen nativ in Ihren Anwendungen rendern können.
CompactDensity | Aktivieren des [Kompaktmodus](../design/style/spacing.md) in Ihrer App ermöglicht dichte, informationsreiche Gruppen von Steuerelementen. Dies kann beim Durchsuchen großer Mengen von Inhalten helfen, wobei der sichtbare Inhalt auf einer Seite maximiert wird, oder Navigation und Interaktion unterstützen, wenn der Benutzer Zeigereingaben verwendet.
ItemsRepeater | Mit einem [ItemsRepeater](../design/controls-and-patterns/items-repeater.md)-Steuerelement kann eine benutzerdefinierte Oberfläche für die Anzeige von Sammlungen für deine Benutzer erstellt werden. „ItemsRepeater“ stellt weder eine umfassende Benutzeroberfläche noch eine Standardbenutzeroberfläche bereit. Stattdessen ist das Steuerelement ein Baustein, den Sie dazu verwenden können, Ihre eigenen einzigartigen sammlungsbasierten Oberflächen und benutzerdefinierten Steuerelemente zu erstellen.
Unterrichtstipp | Ein [Unterrichtstipp](../design/controls-and-patterns/dialogs-and-flyouts/teaching-tip.md) ist ein semidauerhaftes und inhaltsreiches Flyout, das Kontextinformationen bereitstellt. Sie können dieses Steuerelement verwenden, um Benutzer über neue oder wichtige Funktionen zu informieren sowie Benutzer an diese zu erinnern.
Befehle auf der Benutzeroberfläche | Mit [Befehlen in UWP-Apps](../design/controls-and-patterns/commanding.md) verwenden Sie die Klassen [XamlUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) und [StandardUICommand](/uwp/api/windows.ui.xaml.input.standarduicommand) (zusammen mit der ICommand-Schnittstelle), um Befehle über verschiedene Steuerelementtypen hinweg freizugeben und zu verwalten, und zwar unabhängig vom verwendeten Geräte- und Eingabetyp.
Windows-UI-Bibliothek | In der neuesten offiziellen Version der Windows UI-Bibliothek – [WinUI 2.1](/uwp/toolkits/winui/release-notes/winui-2.1) – werden lebendige neue XAML-Steuerelemente für Ihre Windows-Apps bereitgestellt. WinUI-Bibliotheks-APIs werden auch unter früheren Versionen von Windows 10 ausgeführt. Sie müssen daher keine Versionsüberprüfungen oder bedingte XAML-Befehle verwenden, um Benutzer zu unterstützen, die nicht das neueste Betriebssystem einsetzen.
Visuelle Ebene in Desktop-Apps | Sie können jetzt [die Visuelle Ebene-UWP-APIs in Desktopanwendungen verwenden](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps). Diese APIs bieten eine leistungsfähige Speichermodus-API für Grafiken, Effekte und Animationen und sind die Grundlage für UI-Elemente für Windows-Geräte.
Z-Tiefe und Schatten | Mit [Z-Tiefe und Schatten](../design/layout/depth-shadow.md) erstellen Sie Erhebungen in Ihrer UWP-App. Diese neuen Funktionen ermöglicht es dir, die Benutzeroberfläche deiner App so zu gestalten, dass sie einfacher auszuwerten ist, und besser vermittelt, was für die Benutzer wichtig ist.

## <a name="develop-windows-apps"></a>Windows-Apps entwickeln

Feature | Beschreibung
:------ | :------
Antimalware Scan Interface (AMSI) | Erfahren Sie, [wie Antimalware Scan Interface (AMSI) Sie beim Schützen vor Schadsoftware unterstützt](/windows/desktop/amsi/how-amsi-helps), und sehen Sie sich dann den [Beispielcode](/windows/desktop/amsi/dev-audience) an, um zu lernen, wie AMSI in Ihrer Desktop-App implementiert werden kann.
C++/WinRT 2.0 | Version 2.0 von C++/WinRT wurde freigegeben. Lesen Sie [Neuigkeiten in C++/WinRT](../cpp-and-winrt-apis/news.md), um einen vollständigen Überblick über alle neuen Änderungen und Ergänzungen zu erhalten.
Auswählen Ihrer Plattform | Du möchtest eine neue Desktopanwendung erstellen? Lesen Sie die überarbeitete Seite [Auswählen Ihrer Plattform](/windows/desktop/choose-your-technology). Dort finden Sie ausführliche Beschreibungen und Vergleiche der Plattformen UWP, WPF und Windows Forms sowie weitere Informationen zur Win32-API.
Konversations-Agent | Mit dem [Windows.ApplicationModel.ConversationalAgent](/uwp/api/windows.applicationmodel.conversationalagent)-Namespace können Sie Ihrer Windows-App jede digitale Unterstützung hinzufügen, die von der Windows-Plattform Agent Activation Runtime (AAR) unterstützt wird.
Clouddateien-API | Über die *Clouddateien-API* können Sie [eine Cloudsynchronisierungs-Engine erstellen, die Platzhalterdateien unterstützt](/windows/desktop/cfapi/build-a-cloud-file-sync-engine).
Direct3D 12 | [Direct3D 12-Renderdurchgänge](/windows/desktop/direct3d12/direct3d-12-render-passes) können die Leistung Ihres Renderers verbessern, sofern dieser, neben anderen Techniken, auf TBDR (TBDR) aufsetzt. Über diese Technik kann Ihr Renderer die GPU-Effizienz steigern, indem Ihre Anwendung in die Lage versetzt wird, Reihenfolgeanforderungen für das Ressourcenrendering und Datenabhängigkeiten besser zu erkennen. Hierdurch wird der Datenverkehr an den/aus dem Chip Arbeitsspeicher verringert.
Direct Machine Learning (DirectML) | [DirectML](/windows/desktop/direct3d12/dml) ist eine systemnahe hardwarebeschleunigte API für maschinelles Lernen (Machine Learning). Sie hat eine vertraute (natives C++, Nano-COM) Schnittstelle und einen Workflow im Stil von DirectX 12. Sie können Machine Learning-Rückschlussworkloads in Ihr Spiel, Ihre Engine, Ihre Middleware, Ihr Back-End oder in eine andere Anwendung integrieren. DirectML wird von jeder DirectX 12-kompatiblen Hardware unterstützt.
DirectX HLSL | [HLSL-Shader Model 6.4](/windows/desktop/direct3dhlsl/hlsl-shader-model-6-4-features-for-direct3d-12) stellt neue intrinsische Machine Learning-Elemente bereit, die mit DirectML verwendet werden können.
Treiberentwicklung | Für Entwickler von Windows-Treibern wurden neue Funktionen für Audio, Kamera, Anzeige, Netzwerk, mobiles Breitband, Drucken, Sensoren, Speicherung und WLAN hinzugefügt. Ausführlichere Informationen finden Sie unter [Neuigkeiten bei der Entwicklung von Treibern](/windows-hardware/drivers/what-s-new-in-driver-development#whats-new-in-windows-10-version-1903-latest).
Dateisystemvorgänge | In diesem [Leitfaden mit bewährten Methoden](../files/best-practices-for-writing-to-files.md) erfahren Sie, wie Sie die Klassen „Windows.Storage.FileIO“ und „Windows.Storage.PathIO“ am besten nutzen können, um Dateisystem-E/A-Vorgänge auszuführen.
Interaktionen mit Gamepad und Fernbedienung | Verwenden Sie [Interaktionen mit Gamepad und Fernbedienung](../design/input/gamepad-and-remote-interactions.md), um nutzbare und zugängliche Interaktionsoberflächen zu erstellen. Mit diesen Interaktionen kann Ihre Anwendung aus 60 cm so intuitiv und einfach zu verwenden sein, wie dies aus 3 m der Fall ist.
Änderung der japanischen Zeitrechnung | Wir haben [diese Anweisungen](../design/globalizing/japanese-era-change.md) bereitgestellt, um Ihnen zu veranschaulichen, wie Sie sicherstellen, dass Ihre Windows-Anwendung auf die Änderung der japanischen Zeitrechnung vorbereitet ist, die am 1. Mai 2019 erfolgen soll. Diese Seite ist auch in Japanisch verfügbar (klicken Sie unten im Artikel auf das Sprachsteuerelement, und wählen Sie „Japanisch“ aus).
Open Source für WPF, Windows Forms und WinUI | Die UX-Frameworks WPF, Windows Forms und WinUI sind jetzt für Open-Source-Beiträge auf GitHub verfügbar. Weitere Informationen und Links findest du im [Blog zur Erstellung von Windows-Apps](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).
Progressive Web-Apps für Xbox | Mit [Progressive Web-Apps für Xbox One](/microsoft-edge/progressive-web-apps/xbox-considerations) können Sie eine Webanwendung erweitern und als Xbox One-App über den Microsoft Store verfügbar machen, während Sie weiterhin Ihre vorhandenen Frameworks, Ihr vorhandenes CDN und Ihr vorhandenes Server-Back-End verwenden. Zum größten Teil können Sie Ihre PWA für Xbox One auf die gleiche Weise verpacken wie für Windows. Dieser Leitfaden führt Sie durch den Prozess, wobei die wesentlichen Unterschiede hervorgehoben werden.
Project Rome | Das Project Rome-SDK ist jetzt für Android und iOS verfügbar. Erfahren Sie, wie Sie Graph-Benachrichtigungen in jede Plattform integrieren: [Android](/windows/project-rome/notifications/how-to-guide-for-android) und [iOS](/windows/project-rome/notifications/how-to-guide-for-ios).
Remotekameras | Verwenden Sie die DeviceWatcher-Klasse, um [Verbindungen mit Remotekameras herzustellen](../audio-video-camera/connect-to-remote-cameras.md) und Frames aus diesen Kameras in Ihre Windows-App zu lesen.
UWP-Steuerelemente in Desktopanwendungen (XAML-Inseln) | Die im Windows SDK enthaltenen APIs zum Hosten von UWP-Steuerelementen in WPF-, Windows Forms-, und C++ Win32-Desktopanwendungen liegen nicht mehr in der Entwicklervorschau vor. Weitere Informationen finden Sie unter [UWP controls in desktop applications](/windows/apps/desktop/modernize/xaml-islands) (Verwenden von Steuerelementen in Desktopanwendungen).
Visual Studio 2019 | Visual Studio-2019 wurde mit den neuesten Tools und Diensten für jeden Entwickler, jede App oder jede Plattform veröffentlicht. Lesen Sie [Neuigkeiten in Visual Studio 2019](/visualstudio/ide/whats-new-visual-studio-2019?view=vs-2019), um das Neueste zu erfahren und zu beginnen.
Win32 WebView | Im Abschnitt [Häufig gestellte Fragen](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) finden Sie sowohl Antworten auf allgemeine Fragen zum Verwenden von Microsoft Edge-WebView in Desktopanwendungen als auch Links zu Beispielen und weiteren Ressourcen.
Windows-Befehlszeile | [Neue Konsolenfeatures](https://devblogs.microsoft.com/commandline/new-experimental-console-features/) umfassen die experimentelle Registerkarte „Terminal“ mit Einstellungen für Scrollen, Cursorform und Cursorfarben. Erfahren Sie mehr im [Blog „Windows Command Line Tools For Developers“](https://devblogs.microsoft.com/commandline/).
Windows-Community-Toolkit | Windows-Community-Toolkit V5. 1 bietet interessante Updates für Animationen, Remotegeräte, Bildzuschneidung und Barrierefreiheit. </br> • Die neue [Lottie-Windows-Bibliothek](/windows/communitytoolkit/animations/lottie) bietet hochwertige Animationsunterstützung unter Windows 10 (1809) durch Verwenden der Windows.UI.Composition-APIs und ermöglicht es, [Bodymovin](https://aescripts.com/bodymovin/)-JSON-Dateien oder optimierte codegenerierte Klassen für Wiedergabe in Ihren Windows-Apps zu verwenden. Testen Sie die neue [Lottie Viewer-App](https://www.microsoft.com/p/lottie-viewer/9p7x9k692tmw) aus dem Microsoft Store, um Animationen auszuprobieren und optimierten Code für Ihre Windows-Apps zu generieren. </br> • Das neue [RemoteDevicePicker](/windows/communitytoolkit/controls/remotedevicepicker)-Steuerelement ermöglicht es einem Benutzer, ein Gerät auszuwählen (über unmittelbaren oder Cloudzugriff), eine App auf diesem Gerät zu starten oder mit App-Diensten auf dem Remotegerät zu kommunizieren. </br> • Das neue [ImageCropper Steuerelement](/windows/communitytoolkit/controls/imagecropper) integriert Zuschnittfunktionalität für Auswählen von Profilbildern oder Verwenden von Tools zur Fotobearbeitung. </br> • Darüber hinaus gibt es Verbesserungen hinsichtlich der Barrierefreiheit in den Steuerelementen, ein Update des [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) 6.0-Vorschaupakets für WPF und WinForms sowie weitere Features, zu denen Sie Informationen in den [Anmerkungen zu dieser Version ](https://github.com/windows-toolkit/WindowsCommunityToolkit/releases/tag/v5.1.0) finden.
Windows Machine Learning | Wir haben die Windows KI-Dokumente umgestaltet, wozu sie in drei Bereiche aufgeteilt wurden: Windows-Machine Learning-(WinML), Fähigkeiten für maschinelles Sehen unter Windows (Windows Vision Skills) und Direct Machine Learning (DirectML). Navigieren Sie zur neuen [Startseite](/windows/ai/). </br> • Die [*MLGen*-Oberfläche](/windows/ai/mlgen) wird in Visual Studio geändert. Ab Windows 10, Version 1903, ist *mlgen* nicht mehr im Windows 10 SDK enthalten. Wenn Sie mit Visual Studio 2017 arbeiten, sollten Sie stattdessen die Visual Studio-Erweiterung [Windows Machine Learning-Code-Generator VS 2017](https://marketplace.visualstudio.com/items?itemName=WinML.mlgen) herunterladen und installieren. Wenn Sie mit Visual Studio-2019 arbeiten, müssen Sie die Erweiterung [Windows Machine Learning Code Generator](https://marketplace.visualstudio.com/items?itemName=WinML.mlgenv2) installieren. </br> • Wir freuen ebenfalls, neue Unterstützung für Gewichtungsverdichten bekanntgeben zu können. Entwickler können nun den Datenträgerbedarf ihrer ML-Modelle verringern, indem sie eine Technik namens Gewichtungsverdichten (weight packing) verwenden, die über den [WinMLTools-Konverter](/windows/ai/convert-model-winmltools) zur Verfügung gestellt wird.
Konsolidierter WinRT-Verweis | Wir haben die vollständige Beschreibung des [WinRT-Typsystems](/uwp/winrt-cref/winrt-type-system) und der [WinMD-Dateien](/uwp/winrt-cref/winmd-files) hinzugefügt, um spezielle ausführliche Beschreibungen der Definitionen für die Struktur von WinRT-APIs bereitzustellen.
Windows-Subsystem für Linux (WSL) | [Neue Updates für WSL](https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/) bieten die Möglichkeit, aus Windows mit dem Datei-Explorer auf Linux-Dateien zuzugreifen, und enthalten einige neue Befehle für „wsl.exe“ und „wslconfig.exe“.
Fähigkeiten für maschinelles Sehen unter Windows | [Windows Vision Skills](/windows/ai/windows-vision-skills) (Fähigkeiten für maschinelles Sehen unter Windows) ist eine Reihe von APIs, mit denen Sie „Fähigkeiten“, etwa Gesichtserkennung, erstellen und diese dann als NuGet-Paket packen können, das von anderen Apps genutzt werden kann, ohne dass es überhaupt erforderlich ist, ein Modell für maschinellen Lernens einzubeziehen.

## <a name="publish--monetize-windows-apps"></a>Veröffentlichen und Monetarisieren von Windows-Apps

Feature | Beschreibung
 :------ | :------
MSIX | In [MSIX-Unterstützung unter Windows 10, Builds 1709 und 1803](/windows/msix/msix-1709-and-1803-support) ist beschrieben, welche MSIX-Features in Versionen vor Windows 10, Version 1809, unterstützt werden.
MSIX-Verpackung und -Bereitstellung | Es wurden mehrere [Verbesserungen im Zusammenhang mit Änderungspaketen](/windows/msix/modification-package-insider-preview-build-18312) eingeführt, um das Einbringen von Anpassungen in ein MSIX-Paket zu vereinfachen. Zu diesen Verbesserungen gehören das neue **rescap6:ModificationPackage**-Element im Paketmanifest, die Möglichkeit, eine Datei im Hauptpaket mit einem Änderungspaket zu überschreiben, und die Möglichkeit, ein Plug-In, das auf einem Dateisystem basiert, als MSIX-Änderungspaket zu verpacken.
MSIX-Verpackungstool | • Wir haben [Unterstützung zum Ausführen von Konvertierungen auf einem Remotecomputer](/windows/msix/packaging-tool/remote-conversion-setup) hinzugefügt. Außerdem haben wir das [Windows-Insider-Programm für das MSIX-Verpackungstool](/windows/msix/packaging-tool/insider-program) eingeführt, um frühzeitigen Zugriff auf neue Toolfunktionen anzubieten. </br> • [Unterstützung von MSIX-Paketen in 1709 und höher](/windows/msix/packaging-tool/support-on-1709-and-later) enthält Anleitungen, wie das MSIX-Verpackungstool verwendet werden kann, um Pakete zu erstellen, die speziell für Windows 10, Version 1709 oder 1803, vorgesehen sind. </br> • [MSIX-Verpackungsumgebung für Hyper-V-Schnellerfassung (Hyper-V Quick Create)](/windows/msix/packaging-tool/quick-create-vm) veranschaulicht, wie eine virtuelle Umgebung für MSIX-Verpackungsprojekte erstellt wird. </br> • [Bündeln von MSIX-Paketen](/windows/msix/packaging-tool/bundle-msix-packages) enthält Anweisungen zum Erstellen eines Pakets mit dem MSIX-Verpackungstool. </br> • [Änderungspakete unter Windows 10, Version 1809](/windows/msix/modification-package-1809-update) enthält Anweisungen zum Erstellen eines Änderungspakets für Windows 10, Version 1809 und höher, mit dem MSIX-Verpackungstool und „MakeApp.exe“.
MSIX SDK | Lesen Sie [Erstellen eines Pakets für die plattformübergreifende Verwendung mithilfe des MSIX SDK](/windows/msix/msix-sdk/sdk-guidance), um zu erfahren, wie Sie die Zielplattformen angeben, in denen Ihre Pakete extrahiert werden sollen.

## <a name="microsoft-learn"></a>Microsoft Learn

Microsoft Learn bietet neue praktische Lern- und Schulungsmöglichkeiten für Microsoft-Entwickler.

* Wenn Sie lernen möchten, wie Windows-Apps entwickelt werden, lesen Sie [unseren neuen Lernpfad](/learn/paths/develop-windows10-apps/). Dort finden Sie eine umfassende Einführung in die Plattform, die Tools und dazu, wie Sie Ihre ersten Apps schreiben.

* Möchten Sie erfahren, wie Sie Ihrer Windows-App Benutzeroberflächenelemente hinzufügen? Erfahren Sie, wie Sie [eine Benutzeroberfläche erstellen](/learn/modules/create-ui-for-windows-10-apps/), [der Benutzeroberfläche Navigation und Medien hinzufügen](/learn/modules/enhance-ui-of-windows-10-app/) oder [Datenbindung implementieren](/learn/modules/implement-data-binding-in-windows-10-app/).

* Wenn Sie an Webentwicklung interessiert sind, lesen Sie [Entwickeln von Webanwendungen mit Visual Studio Code](/learn/modules/develop-web-apps-with-vs-code/) oder [Erstellen einer einfachen Website](/learn/modules/build-simple-website/).

* Alternativ können Sie [alle Windows-Entwicklermodule in Microsoft-Learn](/learn/browse/?products=windows&resource_type=module) durchsuchen.

## <a name="videos"></a>Videos

### <a name="progressive-web-apps"></a>Progressive Web-Apps

Progressive Web-Apps sind Websites, die in unterschiedlichen Browsern und auf einer Vielzahl von Windows 10-Geräten wie native Apps funktionieren. [Sieh dir das Video an](https://youtu.be/ugAewC3308Y), um mehr zu erfahren, und [lies anschließend die Dokumentation](https://developer.microsoft.com/windows/pwa), um loszulegen.

### <a name="vs-code-series"></a>VS Code-Reihe

Sehen Sie sich unsere [neue Videoreihe zu Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) an, wenn Sie wissen möchten, was VSCode ist, wie Sie es verwenden, und wie es erstellt wurde.

### <a name="mixed-reality-services"></a>Mixed Reality-Dienste

HoloLens 2 wurde vor kurzem angekündigt. Sehen Sie sich diese [Videoreihe zu Mixed Reality](https://www.youtube.com/watch?v=pdB7Ukf3u0I&list=PLlrxD0HtieHjh2Nt2BhcluIZeQg0uBZST) an, um die neuesten Informationen zu erhalten und zu erfahren, wie Sie sich beteiligen und mit der Entwicklung beginnen können.

### <a name="one-dev-question"></a>One Dev Question

In der „One Dev Question“-Videoreihe behandeln langjährige Microsoft-Entwickler eine Reihe von Fragen zur Windows-Entwicklung, -Teamkultur und -Geschichte.

* [Raymond Chen on Windows development and history](https://www.youtube.com/playlist?list=PLWs4_NfqMtoxjy3LrIdf2oamq1coolpZ7) (Raymond Chen über Windows-Entwicklung und -Geschichte)

* [Larry Osterman on Windows development and history](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPUkYGpJU0RzvY6PBSEA4K) (Larry Osterman über Windows-Entwicklung und -Geschichte)

* [Aaron Gustafson on Progressive Web Apps](https://www.youtube.com/playlist?list=PLWs4_NfqMtoyPHoI-CIB71mEq-om6m35I) (Aaron Gustafson zu progressiven Web-Apps)

* [Chris Heilmann on the webhint tool](https://www.youtube.com/playlist?list=PLWs4_NfqMtow00LM-vgyECAlMDxx84Q2v) (Christian Heilmann zum Webhint-Tool)