---
Description: Entdecken Sie die verschiedenen Optionen für Desktop-Win32-Apps zum Senden von Popup Benachrichtigungen.
title: Popup Benachrichtigungen von Win32-apps
label: Toast notifications from Win32 apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Win32, Desktop, Popup Benachrichtigungen, Desktop Bridge, msix, sparsesloadpaket, Optionen für das Senden von Toasts, com-Server, com-Activator, com, gefälschtes com, Nein com, ohne com, Send Toast
ms.localizationpriority: medium
ms.openlocfilehash: 6f02bc7c615643ba0d2ca0ed1b43ecf13641c1c5
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984546"
---
# <a name="toast-notifications-from-win32-apps"></a>Popup Benachrichtigungen von Win32-apps

Win32-Apps (einschließlich gepackter [msix](/windows/msix/desktop/source-code-overview) -apps, apps, die [Pakete](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) mit geringer Dichte zum Abrufen der Paket Identität verwenden, und klassische, nicht gepackte Win32-Apps) können interaktive Popup Benachrichtigungen wie Windows-apps senden. Aufgrund der verschiedenen Aktivierungs Schemas gibt es jedoch einige unterschiedliche Optionen für Win32-apps.

In diesem Artikel werden die Optionen aufgelistet, die Sie zum Senden einer Popup Benachrichtigung unter Windows 10 haben. Jede Option unterstützt vollständig...

* Persistente Speicherung im Aktions Center
* Aktivierbare sowohl im Popup-als auch innerhalb des Aktions Centers
* Aktivierbar, während ihre exe nicht ausgeführt wird

## <a name="all-options"></a>Alle Optionen

In der folgenden Tabelle werden die Optionen für die Unterstützung von Popups in der Win32-APP und die entsprechenden unterstützten Funktionen veranschaulicht. Sie können die Tabelle verwenden, um die beste Option für Ihr Szenario auszuwählen.<br/><br/>

| Option | Visuals | Aktionen | Eingaben | Prozess interne Aktivierung |
| -- | -- | -- | -- | -- |
| [COM-Activator](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Keine com/Stub-CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Bevorzugte Option: com-Activator

Dies ist die bevorzugte Option, die für Win32-apps funktioniert und alle Benachrichtigungs Features unterstützt. Machen Sie sich keine Angst vor "com Activator". Wir verfügen über eine Bibliothek [für c#](send-local-toast-desktop.md) -und [C++-apps](send-local-toast-desktop-cpp-wrl.md) , die dies sehr einfach macht, auch wenn Sie noch nie einen com-Server geschrieben haben.<br/><br/>

| Visuals | Aktionen | Eingaben | Prozess interne Aktivierung |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Mit der com Activator-Option können Sie die folgenden Benachrichtigungs Vorlagen und Aktivierungs Typen in Ihrer APP verwenden.<br/><br/>

| Vorlage und Aktivierungstyp | Msix/sparsespaket | Klassisches Win32 |
| -- | -- | -- |
| Im Vordergrund | ✔️ | ✔️ |
| Hintergrund | ✔️ | ✔️ |
| Toastgeneric-Protokoll | ✔️ | ✔️ |
| Legacy Vorlagen | ✔️ | ❌ |

> [!NOTE]
> Wenn Sie den com-Aktivator zu Ihrer vorhandenen msix/Sparse-Paket-app hinzufügen, aktivieren Vordergrund-/Hintergrund-und Legacy Benachrichtigungs Aktivierungen nun Ihren com-Aktivierer anstelle der Befehlszeile.

Informationen zur Verwendung dieser Option finden Sie unter [Senden einer lokalen Popup Benachrichtigung von Win32 c#-apps](send-local-toast-desktop.md) oder [Senden einer lokalen Popup Benachrichtigung von Win32 C++ WRL-apps](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Alternative Option-keine com/Stub-CLSID

Dies ist eine alternative Option, wenn Sie einen com-Activator nicht implementieren können. Sie werden jedoch einige Features Opfern, wie z. b. Eingabe Unterstützung (Textfelder auf den-Auffassungen) und Prozess interne Aktivierung.<br/><br/>

| Visuals | Aktionen | Eingaben | Prozess interne Aktivierung |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Wenn Sie mit dieser Option klassisches Win32 unterstützen, sind Sie in den Benachrichtigungs Vorlagen und den Aktivierungs Typen, die Sie verwenden können, viel stärker eingeschränkt (siehe unten).<br/><br/>

| Vorlage und Aktivierungstyp | Msix/sparsespaket | Klassisches Win32 |
| -- | -- | -- |
| Im Vordergrund | ✔️ | ❌ |
| Hintergrund | ✔️ | ❌ |
| Toastgeneric-Protokoll | ✔️ | ✔️ |
| Legacy Vorlagen | ✔️ | ❌ |

Für gepackte [msix](/windows/msix/desktop/source-code-overview) -apps und apps, die [Pakete](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)mit geringer Dichte verwenden, senden Sie einfach Popup Benachrichtigungen wie eine UWP-app. Wenn der Benutzer auf den Popup klickt, wird die Befehlszeile der APP mit den startargs gestartet, die Sie im Toast angegeben haben.

Richten Sie für klassische Win32-Apps die aumid so ein, dass Sie die Umfassungen senden können, und geben Sie dann auch eine CLSID für die Verknüpfung an. Dies kann eine beliebige zufällige GUID sein. Fügen Sie den com-Server/-Activator nicht hinzu. Sie fügen eine "Stub"-com-CLSID hinzu, die dazu führt, dass das Aktions Center die Benachrichtigung beibehält. Beachten Sie, dass Sie nur Protokoll Aktivierungs Toasts verwenden können, da die Stub-CLSID die Aktivierung aller anderen Popup Aktivierungen unterbricht. Daher müssen Sie Ihre APP aktualisieren, um die Protokoll Aktivierung zu unterstützen, und das Popups-Protokoll muss Ihre eigene APP aktivieren.


## <a name="resources"></a>Ressourcen

* [Senden einer lokalen Popup Benachrichtigung von Win32 c#-apps](send-local-toast-desktop.md)
* [Senden einer lokalen Popup Benachrichtigung von Win32 C++ WRL-apps](send-local-toast-desktop-cpp-wrl.md)
* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
