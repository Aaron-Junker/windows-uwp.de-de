---
title: Neuerungen in Windows 10, Build 19041
description: Windows 10, Build 18362 und neue Entwicklertools stellen Tools, Features und Funktionen zur Verfügung, die von Windows 10 unterstützt werden.
keywords: Neuigkeiten, Neuerungen, Windows, Windows 10, Aktualisierung, Updates, Features, neu, neueste, Entwickler, 19041, Mai
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bb7630afd6cc69497494a2e86e6c5e3544acefec
ms.sourcegitcommit: f806d5f3b0c1e046c903d3388092c0e059d21858
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790987"
---
# <a name="whats-new-for-developers-in-windows-10-build-19041"></a>Neuerungen für Entwickler in Windows 10, Build 19041

Windows 10, Build 19041 (auch als SDK-Version 2004 bekannt) bietet Ihnen in Kombination mit Visual Studio 2019 sowie zugehörigen Tools und Features alles, was Sie benötigen, um außergewöhnliche Windows-Apps zu erstellen. Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

Dies ist eine Sammlung von neuen und verbesserten Features und Richtlinien, die in dieser Version für Windows-Entwickler interessant sind. Eine vollständige Liste mit neuen Namespaces, die dem Windows SDK hinzugefügt wurden, finden Sie in den [API-Änderungen für Windows 10, Build 19041](windows-10-build-19041-api-diff.md). Weitere Informationen zu den Highlights von Windows 10 finden Sie unter [Die Highlights in Windows 10](https://developer.microsoft.com/windows/windows-10-for-developers).

## <a name="windows-10-apps"></a>Windows 10-Apps

Feature | Beschreibung
:------ | :------
Bluetooth-Audiowiedergabe | Unter [Aktivieren der Audiowiedergabe auf Geräten mit Remoteverbindung über Bluetooth](../audio-video-camera/enable-remote-audio-playback.md) erhalten Sie Informationen dazu, wie Sie [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) verwenden können, damit Geräte mit Remoteverbindung über Bluetooth Audioinhalte auf dem lokalen Computer wiedergeben können. Dies ermöglicht Szenarios wie das Konfigurieren eines Computers, sodass sich dieser wie ein Bluetooth-Lautsprecher verhält, oder das Abspielen von Audioinhalten auf den Smartphones von Benutzern.
Portieren von C#-Apps | Wir haben den Portierungsprozess von C#-Anwendungen in C++/WinRT dokumentiert. Der Artikel [Portieren des Beispiels „Zwischenablage“ von C# in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) ist kontextbezogen und basiert auf einem bestimmten Portierungsszenario. Im Begleitartikel [Umstellen von C# auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp) werden die technischen Details und die einzelnen Schritte des Portierungsvorgangs umfassend erläutert. 
C++/WinRT | Unter [Rollup aktueller Verbesserungen/Ergänzungen](/windows/uwp/cpp-and-winrt-apis/news#rollup-of-recent-improvementsadditions-as-of-march-2020) erhalten Sie weitere Informationen zu den Updates für C++/WinRT in Bezug auf Verbesserungen der Buildzeit und der Runtime (in Zusammenarbeit mit dem Visual C++-Compiler-Team entstanden). </br> Für C++/WinRT haben wir weitere Informationen zu den folgenden Themen hinzugefügt: [Portieren von C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#boxing-and-unboxing), [Portieren von C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp#boxing-and-unboxing), [Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek](/windows/uwp/cpp-and-winrt-apis/simple-winui-example), [Parallelität](/windows/uwp/cpp-and-winrt-apis/concurrency), [get_unknown()](/uwp/cpp-ref-for-winrt/get-unknown) und [Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl).
DirectX | Wir haben mehrere Artikel mit Neuerungen zu DirectX für frühere Windows-Versionen aktualisiert, angefangen beim Creators Update bis hin zu Windows 10, Version 1903. [Neuerungen für DirectWrite](/windows/win32/directwrite/what-s-new-in-directwrite-for-windows-8-consumer-preview), [Verbesserungen für DXGI 1.6](/windows/win32/direct3ddxgi/dxgi-1-6-improvements) und [Neuerungen für Direct3D 12](/windows/win32/direct3d12/new-releases).
DirectXMath | Wir haben 21 neue Artikel zu DirectXMath veröffentlicht, die zwei Matrixstrukturen und ihre Memberfunktionen sowie kostenlose Funktionen abdecken. Beispiel: die [XMFLOAT3X4-Struktur](/windows/win32/api/directxmath/ns-directxmath-xmfloat3x4).
Direct3D | Unter [Verwenden von DirectX mit High Dynamic Range-Anzeigen (HDR) und erweiterten Farben](/windows/win32/direct3darticles/high-dynamic-range) finden Sie eine Liste bewährter Methoden für HDR-Windows-Apps. </br> Durch die neue [ID3D11On12Device2](/windows/win32/api/d3d11on12/nn-d3d11on12-id3d11on12device2)-Schnittstelle und ihre Methoden können Sie über Direct3D 11-APIs erstellte Ressourcen auch in Direct3D 12 verwenden.
Direct3D 12 | [Die Featureebene Direct3D 12 Core 1.0](/windows/win32/direct3d12/core-feature-levels) wurde zur Verwendung von *Nur-Compute*-Geräten hinzugefügt. </br> Für die [Schnittstelle ID3D12Debug3](/windows/win32/api/d3d12sdklayers/nn-d3d12sdklayers-id3d12debug3) wurden neue Artikel hinzugefügt.
Direct ML | Es wurden 18 Operatoren zu DirectML hinzugefügt, der Low-Level-API mit Hardwarebeschleunigung, auf die WinML aufbaut. Ein Beispiel ist die [DML_ACTIVATION_SHRINK_OPERATOR_DESC-Struktur](/windows/win32/api/directml/ns-directml-dml_activation_shrink_operator_desc).
Fehlerberichterstattung | Die RoFailFastWithErrorContextInternal2-Funktion wurde zu Win32 hinzugefügt. Diese löst eine Ausnahme aus, die zusätzlichen Fehlerkontext enthalten kann.
Maschinelles Lernen | Windows Machine Learning [unterstützt jetzt die ONNX-Version 1.4 und opset 9](/windows/ai/windows-ml/release-notes). </br>  Mit der [CloseModelOnSessionCreation](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions.closemodelonsessioncreation?view=winrt-19041)-API können Sie Arbeitsspeicher einsparen, indem Lernmodelle automatisch geschlossen werden, sobald sie nicht mehr benötigt werden.
WLAN | Es wurden mehrere neue native WLAN-Funktionen und -Strukturen hinzugefügt, z. B. die [Funktion WlanDeviceServiceCommand](/windows/win32/api/wlanapi/nf-wlanapi-wlandeviceservicecommand).
Wi-Fi Hotspot 2 | Unter [Bereitstellen eines WLAN-Profils über eine Website](/windows/win32/nativewifi/prov-wifi-profile-via-website) werden die neuen Funktionen von Wi-Fi Hotspot 2 erläutert.
Windows Holographic Interop | Der Header [`windows.graphics.holographic.interop.h`](/windows/win32/api/windows.graphics.holographic.interop) wurde zusammen mit 17 Win32-APIs hinzugefügt. Die APIs unterstützen die Interaktion zwischen Win32 und der Windows-Runtime. Die APIs wurden bereits in Windows 10, Build 18362 hinzugefügt, aber der Header ist für Build 19041 neu.
Windows Sockets | Es wurden Verbesserungen an den Inhalten zu Windows Sockets 2 SPI vorgenommen. Unter anderem wurde der Artikel [LPWSPEVENTSELECT-Rückruffunktion](/windows/win32/api/ws2spi/nc-ws2spi-lpwspeventselect) verbessert und erweitert.
XAML Islands: Grundlagen | Mithilfe von XAML Islands können Sie XAML-Steuerelemente für die Universelle Windows-Plattform in Ihren Windows-Desktop-Apps hosten. Weitere Informationen finden Sie unter [Hosten eines UWP-Standardsteuerelements in einer WPF-App](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands) und [Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App](/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands-cpp).
XAML Islands: benutzerdefinierte Steuerelemente | Die NuGet-Pakete [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) und [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) erleichtern das Hosten von benutzerdefinierten XAML-Steuerelementen in .NET.- und C++-Win32-Apps. </br> Detaillierte Anweisungen finden Sie unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands) und unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App](/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands-cpp). </br> Informationen zu komplizierteren C++-Win32-Szenarios finden Sie unter [Erweiterte Szenarios für XAML Islands](/windows/apps/desktop/modernize/advanced-scenarios-xaml-islands-cpp).

## <a name="build-with-windows"></a>Erstellen mit Windows

Feature | Beschreibung
:------ | :------
Windows-Entwicklungsumgebung | In der Dokumentation zur [Windows-Entwicklungsumgebung](/windows/dev-environment/) werden Ressourcen für die Verwendung von Windows für die Entwicklung auf einer Vielzahl von Plattformen bereitgestellt, mit denen Sie die gewünschten Entwicklungsziele erreichen.
Python unter Windows | Der Abschnitt [Python unter Windows](/windows/python/) enthält Informationen für Python-Anfänger sowie für Fortgeschrittene, die zusätzliche unter Windows verfügbare Tools für die Python-Entwicklung verwenden möchten. Informationen zum Einrichten Ihrer Python-Umgebung finden Sie unter [Webentwicklung](/windows/python/web-frameworks) und [Datenbankinteraktion](/windows/python/databases).
Node.js unter Windows | Unter [Einrichten einer Node.js-Entwicklungsumgebung](/windows/nodejs/setup-on-wsl2) finden Sie detaillierte Richtlinien für fortgeschrittene Entwickler, die Anwendungen auf Linux-Servern bereitstellen möchten. Außerdem stehen Anweisungen zur Einrichtung für [beliebte Node.js-Webframeworks](/windows/nodejs/web-frameworks), die [Datenbankinteraktion](/windows/nodejs/databases) und [Docker-Container](/windows/nodejs/containers) zur Verfügung.
Mac zu Windows | Unser [Leitfaden zum Ändern der Entwicklungsumgebung](/windows/dev-environment/mac-to-windows) richtet sich an Benutzer, die von einer Mac- zu einer Windows-Entwicklungsumgebung wechseln möchten, und enthält Zuordnungen für vergleichbare Tastenkombinationen und Hilfsprogramme für die Entwicklung.
Windows-Terminal | Eine [moderne Terminalanwendung](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab) für Benutzer von Befehlszeilentools und Shells wie die Eingabeaufforderung, PowerShell und das Windows-Subsystem für Linux (WSL). Zu den Hauptmerkmalen gehören mehrere Registerkarten, Bereiche, Unterstützung für Unicode- und UTF-8-Zeichen, eine GPU-beschleunigte Engine zum Rendern von Text sowie die Möglichkeit, eigene Designs zu erstellen und Text, Farben, Hintergründe und Tastenzuordnungen anzupassen.
WSL 2 | [Eine neue Version des Windows-Subsystems für Linux (WSL)](/windows/wsl/wsl2-about) ist jetzt verfügbar. WSL 2 umfasst eine neu konfigurierte Architektur zum Ausführen eines echten Linux-Kernels unter Windows. Dadurch wird die Leistung des Dateisystems optimiert und die vollständige Kompatibilität von Systemaufrufen hinzugefügt. Diese neue Architektur führt zu einer Änderung der Interaktion von Linux-Binärdateien mit Windows und der Hardware Ihres Computers, bietet aber die gleichen Funktionen wie die Vorgängerversion von WSL. Jede einzelne Linux-Distribution kann parallel oder als WSL 1-oder WSL 2-Distribution ausgeführt und jederzeit geändert werden. </br> [Installieren Sie WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-install), um loszulegen. </br> Informieren Sie sich zusätzlich über die [unterschiedlichen Funktionen von WSL 1 und WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-ux-changes). </br> Lesen Sie sich die [häufig gestellte Fragen zu WSL 2](https://docs.microsoft.com/windows/wsl/wsl2-faq) durch.

## <a name="msix-packaging-and-deployment"></a>MSIX, Paketerstellung und Bereitstellung

Feature | Beschreibung
:------ | :------
MSIX | Seit dem letzten Release des Windows 10 SDK wurden bedeutende Aktualisierungen an dem [MSIX-Paketformat](/windows/msix/overview) vorgenommen. 
Paketerstellung mit Diensten | MSIX und das MSIX Packaging Tool [unterstützen jetzt App-Pakete, die Dienste enthalten](/windows/msix/packaging-tool/convert-an-installer-with-services).
Skripts in MSIX-Paketen | Sie können [PSF (Package Support Framework) verwenden, um Skripts in einem MSIX-App-Paket](/windows/msix/psf/run-scripts-with-package-support-framework) auszuführen. So können IT-Experten Anwendungen dynamisch an die Benutzerumgebung anpassen, nachdem diese mithilfe von MSIX gepackt wurden.
Erzwungene Paketintegrität | Sie können jetzt die Paketintegrität für die Inhalte von MSIX-Paketen erzwingen, indem Sie das [uap10:PackageIntegrity-Element](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity) in Ihrem Paketmanifest verwenden. Ebenso können Sie die Paketintegrität erzwingen, indem Sie MSIX-Pakete über das MSIX Packaging Tool erstellen.
Sparse Packages | Sie können Desktop-Apps, die nicht in ein MSIX-Paket gepackt sind, durch Erstellen und Registrieren eines *Sparse-Pakets* mit Ihrer App eine [Paketidentität gewähren](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Dieses Feature ermöglicht es Desktop-Apps, die noch nicht für die MSIX-Paketerstellung zur Bereitstellung geeignet sind, Windows 10-Erweiterbarkeitsfeatures zu verwenden, die eine Paketidentität erfordern.
Gehostete Apps | Sie können nun [gehostete Apps erstellen](/windows/uwp/launch-resume/hosted-apps). Gehostete Apps verwenden dieselbe ausführbare Datei und Definition wie die jeweilige übergeordnete Host-App, aber sie sehen wie eine separate App aus und verhalten sich auch als solche. Gehostete Apps sind nützlich für Szenarios, in denen sich eine Komponente (z. B. eine ausführbare Datei oder eine Skriptdatei) wie eine eigenständige Windows 10-App verhalten soll. Die Komponente erfordert jedoch einen Hostprozess, um ausgeführt werden zu können. Eine gehostete App kann über eine eigene Startkachel, eine eigene Identität und eine umfassende Integration in Windows 10-Features verfügen, z. B. Hintergrundaufgaben, Benachrichtigungen, Kacheln und Freigabeziele.

## <a name="windows-ui-library-winui"></a>Windows UI Library (WinUI)

Feature | Beschreibung
:------ | :------
WinUI 2.4 | [WinUI 2.4](/uwp/toolkits/winui/release-notes/winui-2.4) ist die neuste öffentliche Version von Windows UI Library. Alle Versionen von WinUI bieten eine große Auswahl an offiziellen Steuerelementen für die Benutzeroberfläche für Ihre Windows-Apps und werden als NuGet-Paket bereitgestellt, das unabhängig vom Windows SDK ist und dadurch auch auf früheren Versionen von Windows 10 funktioniert. [Befolgen Sie diese Anweisungen](/uwp/toolkits/winui), um WinUI zu installieren.
RadialGradientBrush | In WinUI 2.4 wurde neu hinzugefügt, dass eine [RadialGradientBrush](../design/style/brushes.md#radial-gradient-brushes)-Klasse innerhalb einer Ellipse gezeichnet wird, die durch die Eigenschaften „Center“, „RadiusX“ und „RadiusY“ definiert wird. Farben für den Farbverlauf beginnen in der Mitte der Ellipse und enden am Radius.
ProgressRing | In WinUI 2.4 wurde neu hinzugefügt, dass das [ProgressRing](../design/controls-and-patterns/progress-controls.md)-Steuerelement für modale Interaktionen verwendet wird, bei denen die Aktivitäten des Benutzers blockiert werden, bis das ProgressRing-Element nicht mehr angezeigt wird. Verwenden Sie dieses Steuerelement, wenn es für einen Vorgang erforderlich ist, dass die meisten Interaktionen mit der App angehalten werden, bis der Vorgang beendet ist.
TabView | Aktualisierungen des [TabView-Steuerelements](../design/controls-and-patterns/tab-view.md) bieten Ihnen mehr Kontrolle über die Darstellung von Registerkarten. Sie können die Breite von nicht ausgewählten Registerkarten festlegen und nur ein Symbol anzeigen, um den Bildschirmbereich zu speichern. Außerdem können Sie die Schaltfläche „Schließen“ auf nicht ausgewählten Registerkarten ausblenden, bis der Benutzer mit der Maus auf die jeweilige Registerkarte zeigt.
TextBox-Steuerelemente | Wenn das dunkle Design aktiviert ist, bleibt die Hintergrundfarbe der Steuerelemente der TextBox-Familie beim Einfügen von Text standardmäßig dunkel. Dies betrifft die folgenden Steuerelemente: [TextBox](/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock), [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox), [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox) und [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).
NavigationView | Das [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)-Steuerelement unterstützt nun die hierarchische Navigation und umfasst die Anzeigemodi „Left“, „Top“ und „LeftCompact“. Ein hierarchisches NavigationView-Steuerelement ist beim Anzeigen der Kategorien von Seiten, beim Identifizieren von Seiten mit zugeordneten Unterseiten oder bei der Verwendung in Apps mit Seiten im Hubstil nützlich, die Verknüpfungen zu vielen weiteren Seiten aufweisen.
Windows UI Gallery | Beispiele für die einzelnen WinUI-Features finden Sie in der XAML Controls Gallery. Laden Sie diese im [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) herunter, oder [rufen Sie den Quellcode auf GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) ab.
Vorherige Versionen | Seit der letzten Hauptversion des Windows 10 SDK wurden ebenfalls [WinUI 2.3](/uwp/toolkits/winui/release-notes/winui-2.3) und [WinUI 2.2](/uwp/toolkits/winui/release-notes/winui-2.2) veröffentlicht. Diese Releases umfassen weitere neue Benutzeroberflächenfeatures für Windows-Entwickler.

## <a name="samples"></a>Beispiele

Die folgenden Beispiel-Apps wurden für Windows 10, Build 19041 aktualisiert.

* [Remote Sessions (Quiz Game)](https://github.com/microsoft/Windows-appsample-remote-system-sessions)
* [Datenbank für Kundenaufträge](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
* [RSS Reader](https://github.com/Microsoft/Windows-appsample-rssreader)
* [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze)
* [Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor)
* [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
* [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Hue Light Controller](https://github.com/Microsoft/Windows-appsample-huelightcontroller)
* [Photo Lab](https://github.com/Microsoft/Windows-appsample-photo-lab)
* [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes)

## <a name="videos"></a>Videos

### <a name="windows-terminal-the-secret-to-command-line-happiness"></a>Optimierte Verwendung der Befehlszeile mithilfe von Windows-Terminal

Informieren Sie sich darüber, wie Sie Windows-Terminal für Ihren Workflow anpassen, und sehen Sie sich Featuredemos an. [Sehen Sie sich zuerst das Video an](https://www.youtube.com/watch?v=2dsnwlnNBzs). Anschließend finden Sie weitere Informationen in der [Dokumentation](https://github.com/microsoft/terminal#terminal--console-overview).

### <a name="wsl2-code-faster-on-the-windows-subsystem-for-linux"></a>WSL 2: Schnelleres Programmieren mit dem Windows-Subsystem für Linux

Sie erhalten alle wichtigen Informationen zu WSL 2, der neuen Version des Windows-Subsystems für Linux, und zu den vorgenommenen Änderungen zur Verbesserung der Leistung. [Sehen Sie sich zuerst das Video an](https://www.youtube.com/watch?v=MrZolfGm8Zk). Anschließend finden Sie weitere Informationen in der [Dokumentation](/windows/wsl/wsl2-about).

### <a name="msix-package-desktop-apps-for-windows-10-replace-outdated-installers"></a>MSIX: Packen von Desktop-Apps für Windows 10 und Ersetzen von veralteten Installationsprogrammen

Erfahren Sie mehr über MSIX, das Paketformat für die Installation von Windows-Apps, einschließlich dem Packen Ihres vorhandenen Codes mit Visual Studio und der Bereitstellung und Verteilung Ihrer App. [Sehen Sie sich zuerst das Video an](https://www.youtube.com/watch?v=yhOnClQrvBk). Anschließend finden Sie weitere Informationen in der [Dokumentation](/windows/msix/).
