---
Description: In diesem Abschnitt erfahren Sie, wie bestimmte wichtige Windows-Features auf verschiedenen App-Plattformen unterstützt werden und wie Sie mit der Nutzung dieser Features in Ihrem Code beginnen.
title: Features und Technologien
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: eef7ffd2e84e05bb127120ea7dfa56764782bee9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174164"
---
# <a name="features-and-technologies-for-windows-apps"></a>Features und Technologien für Windows-Apps

Unabhängig davon, welche Art von App Sie entwickeln oder welches Gerät Sie als Ziel haben, unterstützt Windows viele Features, die zentrale Bausteine für wichtige App-Szenarien darstellen. Einige dieser Features werden für die universelle Windows-Plattform (UWP), Win32 (Windows-API) und andere App-Plattformen auf unterschiedliche Weise verfügbar gemacht. In den folgenden Artikeln erfahren Sie, wie bestimmte wichtige Windows-Features auf unterschiedlichen App-Plattformen unterstützt werden und wie Sie diese Features in Ihrem Code verwenden.

Dieser Artikel enthält eine angepasste Liste von Artikeln, in denen Sie erfahren, wie Sie auf den Plattformen UWP, Win32 (Windows API), WPF und Windows Forms auf wichtige Windows-Features und -Technologien zugreifen können. Vollständige Informationen zu den Entwicklungsfeatures der einzelnen Plattformen finden Sie in den folgenden Ressourcen:

* [Dokumentation zu UWP](/windows/uwp/index)
* [Dokumentation zu Win32 (Windows-API)](/windows/desktop/index)
* [Dokumentation zu WPF](/dotnet/framework/wpf/index)
* [Dokumentation zu Windows Forms](/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Wichtige Windows-Features und -Technologien

In den folgenden Abschnitten werden einige wichtige Windows-Features und -Technologien vorgestellt, mit denen Sie für Ihre Kunden moderne und ansprechende Umgebungen bereitstellen können.

### <a name="windows-ink"></a>Windows Ink

![Surface-Stift](images/hero-small.png)  

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Weitere Informationen zu den verschiedenen Möglichkeiten der Verwendung von Windows Ink in Windows-Apps finden Sie unter [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Spracherkennungsinteraktionen

![Erster Erkennungsbildschirm für eine Einschränkung basierend auf einer SGRS-Grammatikdatei](images/speech-listening-initial.png)

![Letzter Erkennungsbildschirm für eine Einschränkung basierend auf einer SGRS-Grammatikdatei](images/speech-listening-complete.png)

Windows bietet zahlreiche Möglichkeiten zum Integrieren von Spracherkennung und Text-zu-Sprache (auch als Text-to-Speech, TTS oder Sprachsynthese bezeichnet) direkt in die Benutzeroberfläche Ihrer App. Über Sprache können Sie Ihren Benutzern eine stabile und angenehme Möglichkeit zur Interaktion mit Ihrer App bieten und zudem die Interaktion per Tastatur, Maus, Toucheingabe und Gesten ergänzen oder sogar ersetzen.

Weitere Informationen zu den verschiedenen Möglichkeiten, Sprachinteraktionen in Windows-Apps zu nutzen, finden Sie unter [Sprache (Speech), Stimme (Voice) und Unterhaltung in Windows 10](speech.md).

### <a name="windows-ai"></a>Windows KI

![Windows KI](images/windows-ai.png)

Wir bieten verschiedene KI-Lösungen, mit denen Sie Ihre Windows-Anwendungen verbessern können. Mit Windows Machine Learning können Sie trainierte Machine Learning-Modelle in Ihre Apps integrieren und lokal auf einem Gerät ausführen. Mit Windows-Bildverarbeitungsskills können Sie mithilfe von vordefinierten Bibliotheken gängige Bildverarbeitungsaufgaben ausführen und eigene benutzerdefinierte Lösungen erstellen. DirectML bietet APIs auf niedriger Ebene im DirectX-Stil, mit denen Sie die Hardware voll nutzen können.

Weitere Informationen zu den verschiedenen Möglichkeiten der Integration von KI in Windows-Apps finden Sie unter [Windows-KI](/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Features und Technologien nach Plattform

Die folgenden Abschnitte enthalten nützliche Links, um mehr über die Integration von Windows-Kernfeatures und -technologien von unseren wichtigsten App-Plattformen zu erfahren: UWP, Win32 (Windows-API), WPF und Windows Forms.

### <a name="user-interface-and-accessibility"></a>Benutzeroberfläche und Barrierefreiheit

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Design](/windows/uwp/design/basics/)<br/><br/>[Layout](/windows/uwp/design/layout/)<br/><br/>[Steuerelemente](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Eingabe](/windows/uwp/design/input/)<br/><br/>[Kacheln](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/)<br/><br/>[Starten, Fortsetzen und Hintergrundaufgaben](/windows/uwp/launch-resume/)<br/><br/>[Windows-Barrierefreiheit](/windows/uwp/design/accessibility/accessibility)<br/><br/>[Sprachinteraktionen](/windows/uwp/design/input/speech-interactions)<br/><br/> |  [Desktop-Benutzeroberfläche](/windows/desktop/windows-application-ui-development)<br/><br/>[Desktopumgebung und Shell](/windows/desktop/user-interface)<br/><br/>[Windows-Steuerelemente](/windows/desktop/controls/window-controls)<br/><br/>[UWP-Steuerelementen in Desktop-Apps (XAML-Inseln)](./desktop/modernize/xaml-islands.md)<br/><br/>[Visuelle UWP-Ebene in Desktop-Apps](./desktop/modernize/visual-layer-in-desktop-apps.md)<br/><br/>[Fenster und Meldungen](/windows/desktop/winmsg/windowing)<br/><br/>[Menüs und weitere Ressourcen](/windows/desktop/menurc/resources)<br/><br/>[Hohe DPI-Werte](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Bedienungshilfen](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform – Software Development Kit (SDK) (Version 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[Microsoft Speech SDK, Version 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [Windows in WPF](/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Übersicht über Navigation](/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML in WPF](/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Steuerelemente](/dotnet/framework/wpf/controls/)<br/><br/>[Programmierung auf visueller Ebene](/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Eingabe](/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Bedienungshilfen](/dotnet/framework/ui-automation/)<br/><br/>[System.Speech-Programmierhandbuch für .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Erstellen eines Windows-Formulars](/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Steuerelemente](/dotnet/framework/winforms/controls/)<br/><br/>[Dialogfelder](/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Benutzereingabe](/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms-Barrierefreiheit](/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[System.Speech-Programmierhandbuch für .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, Video und Grafik

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, Video und Kamera](/windows/uwp/audio-video-camera/)<br/><br/>[Medienwiedergabe](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/) |  [Audio und Video](/windows/desktop/audio-and-video)<br/><br/>[Grafik und Spiele](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Grafiken](/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Grafik und Zeichnungen](/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer-Klasse](/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Datenzugriff und App-Ressourcen

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Datenzugriff](/windows/uwp/data-access/)<br/><br/>[Datenbindung](/windows/uwp/data-binding/)<br/><br/>[Dateien, Ordner und Bibliotheken](/windows/uwp/files/)<br/><br/>[App-Ressourcen](/windows/uwp/app-resources/) |  [Datenzugriff und -speicherung](/windows/desktop/data-access-and-storage)<br/><br/>[Lokale Dateisysteme](/windows/desktop/fileio/file-systems)<br/><br/>[Ressourcenübersichten](/windows/desktop/menurc/resources-overviews)</li>  |  [Daten und Modellierung](/dotnet/framework/data/)<br/><br/>[Datenbindung](/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressourcen in .NET-Apps](/dotnet/framework/resources/)<br/><br/>[Dateien für Anwendungsressourcen, Inhalte und Daten](/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Daten und Modellierung](/dotnet/framework/data/)<br/><br/>[Datenbindung](/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressourcen in .NET-Apps](/dotnet/framework/resources/)<br/><br/>[Anwendungseinstellungen](/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Geräte, Dokumente und Druck

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Aktivieren von Gerätefunktionen](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensoren](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Drucken und Scannen](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [Sensor-API](/windows/desktop/sensorsapi/portal)<br/><br/>[Drucken](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP-APIs](/desktop/upnp/universal-plug-and-play-start-page) |  [Druck- und Drucksystemverwaltung](/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Druckunterstützung](/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>System, Netzwerk und Leistung

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Abrufen von Akkuinformationen](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threading und asynchrone Programmierung](/windows/uwp/threading-async/)<br/><br/>[Netzwerk und Webdienste](/windows/uwp/networking/) | [Systemdienste](/windows/desktop/system-services)<br/><br/>[Speicherverwaltung](/windows/desktop/memory/memory-management)<br/><br/>[Energieverwaltung](/windows/desktop/power/power-management-portal)<br/><br/>[Prozesse und Threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Netzwerke und Internet](/windows/desktop/networking)<br/><br/>[Windows-Systeminformationen](/windows/desktop/sysinfo/windows-system-information) |  [Threadingmodell](/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Netzwerkprogrammierung in .NET Framework](/dotnet/framework/network-programming/)  |  [Systeminformationen](/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Energieverwaltung](/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Netzwerkprogrammierung in .NET Framework](/dotnet/framework/network-programming/)<br/><br/>[Netzwerke in Windows Forms](/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="debugging-and-performance"></a>Debugging und Leistung

|  UWP  |  Win32 (Windows-API) |  WPF und Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Debugging, Tests und Leistung](/windows/uwp/debug-test-perf)<br/><br/>[Bereitstellen und Debuggen von UWP-Apps](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Zertifizierungskit für Windows-Apps](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[Leistung](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [Debugging und Fehlerbehandlung](/windows/win32/debugging-and-error-handling)<br/><br/>[Debuggingtools für Windows](/windows-hardware/drivers/debugger/)<br/><br/>[Ereignisablaufverfolgung für Windows (ETW)](/windows/win32/etw/event-tracing-portal)<br/><br/>[.NET TraceProcessing-API](./trace-processing/index.yml)<br/><br/>[TraceLogging](/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[Leistungsindikatoren](/windows/win32/perfctrs/performance-counters-portal) |  [Debuggen, Ablaufverfolgung und Profilerstellung](/dotnet/framework/debug-trace-profile/)<br/><br/>[Ablaufverfolgung und Instrumentieren von Anwendungen](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[Diagnostizieren von Fehlern mit Assistenten für verwaltetes Debuggen](/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[Laufzeit-Profilerstellung](/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[Leistungsindikatoren](/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[ClickOnce-Bereitstellung für Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>Verpacken und Bereitstellen

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Verpacken von Apps](/windows/uwp/packaging/)<br/><br/>[MSIX](/windows/msix/)<br/><br/>[App-Paketmanifestschema](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Verpacken von Windows-Desktop-Apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Anwendungsinstallation und -wartung](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Verpacken von Windows-Desktop-Apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen von .NET Framework und Anwendungen](/dotnet/framework/deployment/)<br/><br/>[Bereitstellen von WPF-Anwendungen](/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Verpacken von Windows-Desktop-Apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen von .NET Framework und Anwendungen](/dotnet/framework/deployment/)<br/><br/>[ClickOnce-Bereitstellung für Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |