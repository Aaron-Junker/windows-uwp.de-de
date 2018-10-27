---
author: andrewleader
Description: Discover the different options desktop Win32 apps have for sending toast notifications
title: Popupbenachrichtigungen über Desktop-Apps
label: Toast notifications from desktop apps
template: detail.hbs
ms.author: mijacobs
ms.date: 05/01/2018
ms.topic: article
keywords: Windows10, UWP, Win32, Desktop, Popupbenachrichtigungen, Desktop-Brücke, Optionen zum Senden von Popups, COM-Server, COM-Aktivator, COM, gefälschter COM, kein COM, ohne COM, Senden von Popupbenachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: 2ee44d990ef29fa8281a14c8ad03b6c6ff16a4cf
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "5694413"
---
# <a name="toast-notifications-from-desktop-apps"></a>Popupbenachrichtigungen über Desktop-Apps

Desktop-Apps (Desktop-Brücke-Apps und klassische Win32-Apps) können interaktive Popupbenachrichtigungen wie Universelle Windows-Plattform (UWP)-Apps senden. Es gibt jedoch verschiedene Optionen für Desktop-Apps aufgrund der verschiedenen Aktivierungsschemata.

In diesem Artikel sind die Optionen zum Senden einer Popupbenachrichtigung unter Windows10 aufgeführt. Jede Option bietet vollständige Unterstützung für...

* Beibehaltung im Info-Center
* Aktivierbar vom Popup und innerhalb des Info-Centers
* Aktivierbar, während EXE nicht ausgeführt wird

## <a name="all-options"></a>Alle Optionen

Die nachfolgende Tabelle enthält die Optionen für die Unterstützung von Popups in Ihrer Desktop-App mit den entsprechenden unterstützten Features. Sie können in der Tabelle die beste Option für Ihr Szenario auswählen.<br/><br/>

| Option | Visuelle Elemente | Aktionen | Eingaben | Aktiviert im Prozess |
| -- | -- | -- | -- | -- |
| [COM-Aktivator](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Kein COM/Stub-CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Bevorzugte Option – COM-Aktivator

Dies ist die bevorzugte Option, die sowohl für Desktop-Brücke- als auch klassische Win32-Apps funktioniert und alle Benachrichtigungsfeatures unterstützt. Lassen Sie sich von „COM-Aktivator“ nicht einschüchtern: Wir verfügen über eine Bibliothek [für C#-Apps](send-local-toast-desktop.md) und [C++-Apps](send-local-toast-desktop-cpp-wrl.md), die dies sehr einfach gestalten, selbst wenn Sie zuvor noch niemals einen COM-Server geschrieben haben.<br/><br/>

| Visuelle Elemente | Aktionen | Eingaben | Aktiviert im Prozess |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Mit der Option des COM-Aktivators können Sie die folgenden Benachrichtigungsvorlagen und Aktivierungstypen in Ihrer App verwenden.<br/><br/>

| Vorlage und Aktivierungstyp | Desktop-Brücke | Klassisch Win32 |
| -- | -- | -- |
| ToastGeneric-Vordergrund | ✔️ | ✔️ |
| ToastGeneric-Hintergrund | ✔️ | ✔️ |
| ToastGeneric-Protokoll | ✔️ | ✔️ |
| Ältere Vorlagen | ✔️ | ❌ |

> [!NOTE]
> Wenn Sie den COM-Aktivator zu Ihrer vorhandenen Desktop-Brücke-App hinzufügen, aktivieren die Vordergrund/Hintergrund- und älteren Benachrichtigungsaktivierungen nun Ihren COM-Aktivator anstelle der Befehlszeile.

Informationen zur Verwendung dieser Option finden Sie unter [Senden von Popupbenachrichtigungen über C#-Apps](send-local-toast-desktop.md) oder [Senden von Popupbenachrichtigungen über C++ WRL-Apps](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Alternative Option – kein COM/Stub-CLSID

Dies ist eine alternative Option, wenn Sie keinen COM-Aktivator implementieren können. Sie müssen dabei allerdings auf neue Features verzichten, wie z.B. die Eingabeunterstützung (Textfelder in Popups) und die Aktivierung im Prozess.<br/><br/>

| Visuelle Elemente | Aktionen | Eingaben | Aktiviert im Prozess |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Wenn Sie klassisches Win32 unterstützen, sind Sie mit dieser Option in Bezug auf die Benachrichtigungsvorlagen und Aktivierungstypen, die Sie verwenden können, viel eingeschränkter (siehe unten).<br/><br/>

| Vorlage und Aktivierungstyp | Desktop-Brücke | Klassisch Win32 |
| -- | -- | -- |
| ToastGeneric-Vordergrund | ✔️ | ❌ |
| ToastGeneric-Hintergrund | ✔️ | ❌ |
| ToastGeneric-Protokoll | ✔️ | ✔️ |
| Ältere Vorlagen | ✔️ | ❌ |

Wir werden künftig Dokumente veröffentlichen, die zeigen, wie Sie diese Option verwenden. Im Wesentlichen gilt für Desktop-Brücke-Apps: Senden Sie Popupbenachrichtigungen einfach so, wie eine UWP-App es tun würde. Wenn der Benutzer auf das Popup klickt, wird Ihre App mit der Befehlszeile gestartet, und zwar mit den Start-Argumenten, die Sie im Popup angegeben haben.

Richten Sie für klassische Win32-Apps die AUMID so ein, dass Sie Popupbenachrichtigungen versenden können. Geben Sie dann in Ihrer Verknüpfung auch eine CLSID an. Dies kann eine zufällige GUID sein. Fügen Sie nicht den COM-Server/Aktivator hinzu. Sie fügen eine „Stub“-COM-CLSID hinzu, was dazu führt, dass Info-Center die Benachrichtigung beibehält. Beachten Sie, dass Sie nur Protokollaktivierungspopups verwenden können, da die Stub-CLSID die Aktivierung aller anderen Popupaktivierungen unterbricht. Deshalb müssen Sie Ihre App aktualisieren, um die Protokollaktivierung zu unterstützen. Außerdem müssen Sie Ihre eigene App durch das Popupprotokoll aktualisieren.


## <a name="resources"></a>Ressourcen

* [Senden von Popupbenachrichtigungen über C#-Apps](send-local-toast-desktop.md)
* [Senden von Popupbenachrichtigungen über C++ WRL-Apps](send-local-toast-desktop-cpp-wrl.md)
* [Dokumentation zu Popupinhalt](adaptive-interactive-toasts.md)