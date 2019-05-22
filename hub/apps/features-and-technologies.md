---
Description: In diesem Abschnitt können Sie entnehmen, wie bestimmte wichtige Windows-Features in verschiedenen app-Plattformen unterstützt werden und wie Sie die ersten Schritte mit den Features in Ihrem Code.
title: Features und Technologien
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ff91d8c01e6832e645cc857b638851e1833fc3f9
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984980"
---
# <a name="features-and-technologies-for-windows-apps"></a>Features und Technologien für Windows-apps

Unabhängig davon, welche Art von app Sie erstellen oder sind Geräte, die Sie verwenden möchten, unterstützt Windows viele Features, die wichtigsten Bausteine für wichtige app-Szenarien sind. Einige dieser Funktionen sind verfügbar, auf die universelle Windows Plattform (UWP), Win32 (Windows-API), und andere app-Plattformen auf unterschiedliche Weise. Die folgenden Artikel helfen Ihnen zu verstehen, wie bestimmte Windows-Funktionen in verschiedenen app-Plattformen unterstützt werden und wie Sie die ersten Schritte mit den Features in Ihrem Code.

Dieser Artikel bietet eine angepasste Liste von Artikeln, um weitere Informationen dazu, wie Sie wichtige Windows-Features und Technologien in der UWP Win32 zugreifen können (Windows-API), WPF und Windows Forms-app-Plattformen. Vollständige Informationen zu Features für Entwickler für jede Plattform finden Sie unter den folgenden Ressourcen:

* [UWP-Dokumentation](/windows/uwp/index)
* [Win32-Windows-API-Dokumentation](/windows/desktop/index)
* [WPF-Dokumentation](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Forms-Dokumentation](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Schlüssel-Windows-Features und Technologien

Markieren Sie in den folgenden Abschnitten einige wichtige Windows-Features und Technologien, mit denen Sie zum Übermitteln von modernen und ansprechende Benutzeroberflächen für Ihre Kunden bereitzustellen.

### <a name="windows-ink"></a>Windows Ink

![Surface-Stift](images/hero-small.png)  

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Weitere Informationen zu den verschiedenen Methoden zur Verwendung der Windows Ink in Windows-apps finden Sie unter [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Spracherkennungsinteraktionen

![initial Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![final Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-complete.png)

Windows bietet viele Möglichkeiten für die Integration von Spracherkennung und Sprachsynthese (auch TTS oder Sprachsynthese) direkt in die benutzerfreundlichkeit Ihrer App. Spracherkennung kann es sich um eine stabile und angenehme Möglichkeit für Personen, für die Interaktion mit Ihrer app, ergänzen oder sogar ersetzen, Tastatur, Maus, Touch und Gesten sein.

Weitere Informationen zu den verschiedenen Methoden zum Spracherkennung Interaktionen in Windows-apps verwenden, finden Sie unter [Spracherkennung Interaktionen](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

Wir bieten mehrere unterschiedliche KI-Lösungen, die Sie verwenden können, um Ihre Windows-Anwendungen zu verbessern. Mit Windows Machine Learning können Sie trainiertes Machine learning-Modelle in Ihre apps integrieren und führen sie lokal auf dem Gerät. Windows-maschinelles sehen-Fähigkeiten können Sie vorab erstellte Bibliotheken verwenden, um allgemeine Aufgaben der bildverarbeitung auszuführen, oder erstellen Sie Ihren eigenen benutzerdefinierten Lösungen. DirectML stellt Low-Level, können DirectX-Stil-APIs, die Sie von der Hardware nutzen.

Weitere Informationen zu den verschiedenen Möglichkeiten zum Integrieren von AI in Windows-apps, finden Sie unter [Windows AI](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Features und Technologien von Plattform

In den folgenden Abschnitten finden Sie nützliche Verknüpfungen, um weitere Informationen zur Integration mit Windows-Kernfunktionen und -Technologien von unserer Haupt-app-Plattformen: UWP, Win32 (Windows-API), WPF und Windows Forms.

### <a name="user-interface-and-accessibility"></a>Benutzeroberfläche und Barrierefreiheit

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Design](/windows/uwp/design/basics/)<br/><br/>[Layout](/windows/uwp/design/layout/)<br/><br/>[Steuerelemente](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Eingabe](/windows/uwp/design/input/)<br/><br/>[Kacheln](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/)<br/><br/>[Starten, Fortsetzen und Hintergrundaufgaben](/windows/uwp/launch-resume/)<br/><br/>[Windows-Barrierefreiheit](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Desktop-Benutzeroberfläche](/windows/desktop/windows-application-ui-development)<br/><br/>[Desktop-Umgebung und shell](/windows/desktop/user-interface)<br/><br/>[Windows-Steuerelemente](/windows/desktop/controls/window-controls)<br/><br/>[UWP-Steuerelementen in desktop-apps (XAML-Inseln)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[UWP visueller Ebene in desktop-apps](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows und Nachrichten](/windows/desktop/winmsg/windowing)<br/><br/>[Menüs und weitere Ressourcen](/windows/desktop/menurc/resources)<br/><br/>[Hohe DPI-Werte](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Bedienungshilfen](/windows/desktop/accessibility)<br/><br/>  |  [Windows in WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Übersicht über die Navigation](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML in WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Steuerelemente](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programmierung auf visueller Ebene](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Eingabe](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Bedienungshilfen](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Erstellen Sie ein Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Steuerelemente](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Dialogfelder](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Benutzereingaben](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms-Barrierefreiheit](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, Video und Grafiken

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, Video und Kamera](/windows/uwp/audio-video-camera/)<br/><br/>[Medienwiedergabe](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/) |  [Audio und Video](/windows/desktop/audio-and-video)<br/><br/>[Grafiken und-Spiele](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Grafiken](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Grafik und zeichnen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[SoundPlayer-Klasse](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Data Access und app-Ressourcen

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Datenzugriff](/windows/uwp/data-access/)<br/><br/>[Datenbindung](/windows/uwp/data-binding/)<br/><br/>[Dateien, Ordner und Bibliotheken](/windows/uwp/files/)<br/><br/>[App-Ressourcen](/windows/uwp/app-resources/) |  [Datenzugriff und-Speicherung](/windows/desktop/data-access-and-storage)<br/><br/>[Lokale Dateisysteme](/windows/desktop/fileio/file-systems)<br/><br/>[Übersicht über die Ressource](/windows/desktop/menurc/resources-overviews)</li>  |  [Daten und Modellierung](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Datenbindung](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressourcen in .NET apps](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Anwendungsdateien Ressource, Inhalte und Daten](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Daten und Modellierung](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Datenbindung](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressourcen in .NET apps](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[Anwendungseinstellungen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Geräte, Dokumente und Drucken

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Aktivieren von Gerätefunktionen](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensoren](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Drucken und Scannen](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [Sensor-API](/windows/desktop/sensorsapi/portal)<br/><br/>[Drucken](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP-APIs](/desktop/upnp/universal-plug-and-play-start-page) |  [Drucken und eine drucksystemverwaltung](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [-druckunterstützung](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>System, Netzwerk und Leistung

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Abrufen von Akkuinformationen](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threading und asynchrone Programmierung](/windows/uwp/threading-async/)<br/><br/>[Netzwerk und Webdienste](/windows/uwp/networking/) | [Systemdienste](/windows/desktop/system-services)<br/><br/>[Speicherverwaltung](/windows/desktop/memory/memory-management)<br/><br/>[Energieverwaltung](/windows/desktop/power/power-management-portal)<br/><br/>[Prozesse und threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Netzwerk und Internet](/windows/desktop/networking)<br/><br/>[Windows-Systeminformationen](/windows/desktop/sysinfo/windows-system-information) |  [Threading-Modell](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Netzwerkprogrammierung in .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Systeminformationen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Energieverwaltung](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Netzwerkprogrammierung in .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Netzwerkfunktionen in Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Packen und-bereitstellen

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Verpacken von Apps](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[App-Paket-manifest-schema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Verpacken von Windows-desktop-apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Installation der Anwendung und Wartung](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Verpacken von Windows-desktop-apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen von .NET Framework und Anwendungen](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Bereitstellen von WPF-Anwendungen](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Verpacken von Windows-desktop-apps (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen von .NET Framework und Anwendungen](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[ClickOnce-Bereitstellung für Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
