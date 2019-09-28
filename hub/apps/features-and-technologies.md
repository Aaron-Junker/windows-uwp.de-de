---
Description: In diesem Abschnitt erfahren Sie, wie bestimmte wichtige Windows-Features auf verschiedenen App-Plattformen unterstützt werden und wie Sie mit den Funktionen in Ihrem Code beginnen.
title: Features und Technologien
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 4fb49b5145350360be3b86c73110762cfa30e9db
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339341"
---
# <a name="features-and-technologies-for-windows-apps"></a>Features und Technologien für Windows-apps

Unabhängig davon, welche Art von APP Sie entwickeln oder welches Gerät Sie als Ziel haben, unterstützt Windows viele Funktionen, bei denen es sich um wichtige Bausteine für wichtige App-Szenarien handelt. Einige dieser Features werden auf unterschiedliche Weise für die universelle Windows-Plattform (UWP), Win32 (Windows-API) und andere APP-Plattformen verfügbar gemacht. Die folgenden Artikel helfen Ihnen, zu verstehen, wie bestimmte Windows-Features auf verschiedenen App-Plattformen unterstützt werden und wie Sie mit den Funktionen in Ihrem Code beginnen.

Dieser Artikel enthält eine maßgeschneiderte Liste von Artikeln, in denen Sie erfahren, wie Sie auf wichtige Windows-Features und-Technologien in den Plattformen UWP, Win32 (Windows API), WPF und Windows Forms App zugreifen können. Umfassende Informationen zu den Entwicklungsfunktionen der einzelnen Plattformen finden Sie in den folgenden Ressourcen:

* [UWP-Dokumentation](/windows/uwp/index)
* [Win32-Dokumentation (Windows-API)](/windows/desktop/index)
* [WPF-Dokumentation](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Dokumentation zu Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Wichtige Windows-Features und-Technologien

In den folgenden Abschnitten werden einige wichtige Windows-Features und-Technologien vorgestellt, mit denen Sie Ihren Kunden eine moderne Umgebung bereitstellen und überzeugende Erfahrungen bereitstellen können.

### <a name="windows-ink"></a>Windows Ink

![Surface-Stift](images/hero-small.png)  

In Verbindung mit einem Zeichengerät bietet die Windows Ink-Plattform eine natürliche Möglichkeit, digitale handschriftliche Notizen, Zeichnungen und Anmerkungen zu erstellen. Sie können auf der Plattform Freihanddaten aus einem Eingabedigitalisierungsgerät erfassen, Freihanddaten generieren, verwalten und als letzte Striche auf dem Ausgabegerät rendern sowie über die Schrifterkennung in Text umwandeln.

Weitere Informationen zu den verschiedenen Möglichkeiten der Verwendung von Windows Ink in Windows-apps finden Sie unter [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

### <a name="speech-interactions"></a>Spracherkennungsinteraktionen

![initial Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![final Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-complete.png)

Windows bietet viele Möglichkeiten, Spracherkennung und Text-zu-Sprache (auch als "TTS" bezeichnet) direkt in die Benutzer Darstellung Ihrer APP zu integrieren. Die Sprache kann eine robuste und angenehme Methode für die Interaktion von Benutzern mit Ihrer APP sein, um Sie zu ergänzen oder sogar zu ersetzen, Tastatur, Maus, Toucheingabe und Gesten zu ersetzen.

Weitere Informationen zu den verschiedenen Möglichkeiten zur Verwendung von sprach Interaktionen in Windows-apps finden Sie unter [sprach Interaktionen](/windows/uwp/design/input/speech-interactions).

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

Wir bieten verschiedene Ki-Lösungen, die Sie verwenden können, um Ihre Windows-Anwendungen zu verbessern. Mit Windows Machine Learning können Sie trainierte Machine Learning-Modelle in Ihre apps integrieren und lokal auf dem Gerät ausführen. Mit den Windows-Bild Verarbeitungs Kenntnissen können Sie vorgefertigte Bibliotheken verwenden, um gängige Bildverarbeitungsaufgaben auszuführen oder eigene benutzerdefinierte Lösungen zu erstellen. Directml bietet APIs auf niedriger Ebene (DirectX-Stil), mit denen Sie die Hardware voll ausschöpfen können.

Weitere Informationen zu den verschiedenen Möglichkeiten zur Integration von Ki in Windows-apps finden Sie unter [Windows AI](https://docs.microsoft.com/windows/ai/).

## <a name="features-and-technologies-by-platform"></a>Features und Technologien nach Plattform

Die folgenden Abschnitte enthalten nützliche Links, um mehr über die Integration von Windows-Kern Features und-Technologien aus unseren Haupt-App-Plattformen zu erfahren: UWP, Win32 (Windows-API), WPF und Windows Forms.

### <a name="user-interface-and-accessibility"></a>Benutzeroberfläche und Barrierefreiheit

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Design](/windows/uwp/design/basics/)<br/><br/>[Layout](/windows/uwp/design/layout/)<br/><br/>[Steuerelemente](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Der](/windows/uwp/design/input/)<br/><br/>[Kacheln](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/)<br/><br/>[Starten, Fortsetzen und Hintergrundaufgaben](/windows/uwp/launch-resume/)<br/><br/>[Windows-Barrierefreiheit](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [Desktop Benutzeroberfläche](/windows/desktop/windows-application-ui-development)<br/><br/>[Desktop Umgebung und Shell](/windows/desktop/user-interface)<br/><br/>[Windows-Steuerelemente](/windows/desktop/controls/window-controls)<br/><br/>[UWP-Steuerelementen in Desktop-Apps (XAML-Inseln)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Visuelle UWP-Schicht in Desktop-Apps](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Fenster und Meldungen](/windows/desktop/winmsg/windowing)<br/><br/>[Menüs und weitere Ressourcen](/windows/desktop/menurc/resources)<br/><br/>[Hoher dpi-](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Bedienungshilfen](/windows/desktop/accessibility)<br/><br/>  |  [Fenster in WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Navigations Übersicht](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[XAML in WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Steuerelemente](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programmierung auf visueller Ebene](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Der](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Bedienungshilfen](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Erstellen eines Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Steuerelemente](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Dialog Felder](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Benutzereingabe](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms Barrierefreiheit](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audiodatei, Video und Grafiken

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, Video und Kamera](/windows/uwp/audio-video-camera/)<br/><br/>[Medienwiedergabe](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Visuelle Ebene](/windows/uwp/composition/visual-layer)<br/><br/>[XAML-Plattform](/windows/uwp/xaml-platform/) |  [Audio und Video](/windows/desktop/audio-and-video)<br/><br/>[Grafiken und Spiele](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows-GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Grafiken](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Grafiken und zeichnen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer-Klasse](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Datenzugriff und App-Ressourcen

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Datenzugriff](/windows/uwp/data-access/)<br/><br/>[Datenbindung](/windows/uwp/data-binding/)<br/><br/>[Dateien, Ordner und Bibliotheken](/windows/uwp/files/)<br/><br/>[App-Ressourcen](/windows/uwp/app-resources/) |  [Datenzugriff und-Speicherung](/windows/desktop/data-access-and-storage)<br/><br/>[Lokale Dateisysteme](/windows/desktop/fileio/file-systems)<br/><br/>[Ressourcen Übersichten](/windows/desktop/menurc/resources-overviews)</li>  |  [Daten und Modellierung](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Datenbindung](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressourcen in .net-apps](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Anwendungs Ressourcen-, Inhalts-und Datendateien](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Daten und Modellierung](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Datenbindung](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressourcen in .net-apps](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Anwendungseinstellungen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Geräte, Dokumente und Druck

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Aktivieren von Gerätefunktionen](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Sensoren](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Drucken und Scannen](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [Sensor-API](/windows/desktop/sensorsapi/portal)<br/><br/>[Drucken](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP-APIs](/desktop/upnp/universal-plug-and-play-start-page) |  [Druck-und Drucksystem Verwaltung](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Druckunterstützung](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>System, Netzwerk und Leistung

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Auflisten von Geräten](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Abrufen von Akkuinformationen](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threading und asynchrone Programmierung](/windows/uwp/threading-async/)<br/><br/>[Netzwerk und Webdienste](/windows/uwp/networking/) | [Systemdienste](/windows/desktop/system-services)<br/><br/>[Speicherverwaltung](/windows/desktop/memory/memory-management)<br/><br/>[Energie Verwaltung](/windows/desktop/power/power-management-portal)<br/><br/>[Prozesse und Threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Netzwerk und Internet](/windows/desktop/networking)<br/><br/>[Windows-Systeminformationen](/windows/desktop/sysinfo/windows-system-information) |  [Threading Modell](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Netzwerk Programmierung in der .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [System Informationen](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Energie Verwaltung](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Netzwerk Programmierung in der .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Netzwerk in Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>Verpacken und Bereitstellung

|  UWP  |  Win32 (Windows-API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Verpacken von Apps](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Schema des App-Paket Manifests](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Verpacken von Windows-Desktop-Apps (msix)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Anwendungs Installation und-Wartung](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Verpacken von Windows-Desktop-Apps (msix)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen der .NET Framework und Anwendungen](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Bereitstellen einer WPF-Anwendung](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Verpacken von Windows-Desktop-Apps (msix)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Bereitstellen der .NET Framework und Anwendungen](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[ClickOnce-Bereitstellung für Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
