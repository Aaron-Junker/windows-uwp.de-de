---
Description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung so planen, dass Sie zu einem späteren Zeitpunkt angezeigt wird.
title: Planen einer Popup Benachrichtigung
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, geplante Popup Benachrichtigung, scheduledtoastnotification, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: bc80cf04c1e1461612401ef4ced898058e2dd4ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172354"
---
# <a name="schedule-a-toast-notification"></a>Planen einer Popup Benachrichtigung

Mit geplanten Popup Benachrichtigungen können Sie eine Benachrichtigung so planen, dass Sie zu einem späteren Zeitpunkt angezeigt wird, unabhängig davon, ob Ihre APP zu diesem Zeitpunkt ausgeführt wird. Dies ist nützlich für Szenarios wie das Anzeigen von Erinnerungen oder anderen nachfolgenden Aufgaben für den Benutzer, bei denen die Zeit und der Inhalt der Benachrichtigung im Voraus bekannt sind.

Beachten Sie, dass geplante Popup Benachrichtigungen über ein Übermittlungs Fenster von 5 Minuten verfügen. Wenn der Computer während der geplanten Übermittlung ausgeschaltet ist und länger als 5 Minuten deaktiviert bleibt, wird die Benachrichtigung als nicht mehr relevant für den Benutzer angezeigt. Wenn Sie eine garantierte Übermittlung von Benachrichtigungen unabhängig davon benötigen, wie lange der Computer ausgeschaltet war, empfiehlt es sich, wie in [diesem Codebeispiel](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)dargestellt eine Hintergrundaufgabe mit einem Zeit-und Zeit Auslösers zu verwenden.

> [!IMPORTANT]
> Für Desktop Anwendungen (msix/Sparse-Pakete und klassisches Win32) gibt es etwas andere Schritte zum Senden von Benachrichtigungen und zur Handhabung der Aktivierung. Befolgen Sie die nachfolgenden Anweisungen, aber ersetzen Sie durch `ToastNotificationManager` die `DesktopNotificationManagerCompat` -Klasse aus der [Desktop-Apps-](toast-desktop-apps.md) Dokumentation.

> **Wichtige APIs**: [scheduleddeastnotification-Klasse](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Voraussetzungen

Um dieses Thema vollständig zu verstehen, ist Folgendes hilfreich...

* Ein funktionierendes wissen zu den Begriffen und Konzepten von Popup Benachrichtigungen. Weitere Informationen finden Sie unter [Übersicht über das Popup-und Aktions Center](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10).
* Eine Vertrautheit mit Windows 10-Popup Benachrichtigungs Inhalt. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md).
* Ein Windows 10-UWP-App-Projekt


## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

Es wird empfohlen, die beiden folgenden nuget-Pakete in Ihrem Projekt zu installieren. In unserem Codebeispiel werden diese Pakete verwendet.

* [Microsoft. Toolkit. UWP. Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): Generieren Sie Popup Nutzlasten über Objekte anstelle von unformatiertem XML.
* [QueryString.net](https://www.nuget.org/packages/QueryString.NET/): generieren und Analysieren von Abfrage Zeichenfolgen mit C #


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespace-Deklarationen

`Windows.UI.Notifications` enthält die Toast-APIs.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>Erstellen des Popup Inhalts

In Windows 10 wird der Inhalt der Popup Benachrichtigung mit einer adaptiven Sprache beschrieben, die eine hohe Flexibilität bei der Anzeige Ihrer Benachrichtigung ermöglicht. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md) .

Dank der Benachrichtigungs Bibliothek ist das Erstellen des XML-Inhalts einfach. Wenn Sie die Benachrichtigungs Bibliothek nicht von nuget installieren, müssen Sie die XML-Datei manuell erstellen, wodurch Platz für Fehler bleibt.

Sie sollten immer die **Launch** -Eigenschaft festlegen, sodass Ihre APP weiß, welche Inhalte angezeigt werden sollen, wenn der Benutzer auf den Hauptteil des Toast tippt und die APP gestartet wird.

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = title
                },
     
                new AdaptiveText()
                {
                    Text = content
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>Erstellen des geplanten Popup

Nachdem Sie den Popup Inhalt initialisiert haben, erstellen Sie eine neue [scheduledtoastnotification](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) , und übergeben Sie die XML-Datei des Inhalts und die Uhrzeit, zu der die Benachrichtigung gesendet werden soll.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines Primärschlüssels für den Toast

Wenn Sie die geplante Benachrichtigung Programm gesteuert abbrechen, entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung abzubrechen, zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen von bereits übermittelten Popup Benachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popup Benachrichtigungen im Aktions Center (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Die Kombination aus Tag und Gruppe fungiert als zusammengesetzter Primärschlüssel. Die Gruppe ist der allgemeinere Bezeichner, in dem Sie Gruppen wie "wallposts", "Messages", "friendrequests" usw. zuweisen können. Und dann sollte die Kennung die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Mithilfe einer generischen Gruppe können Sie dann alle Benachrichtigungen aus dieser Gruppe entfernen, indem Sie die [removegroup-API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)verwenden.

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>Planen der Benachrichtigung

Erstellen Sie abschließend einen [toastnotifier](/uwp/api/windows.ui.notifications.toastnotifier) , und rufen Sie addtoschedule () auf, und übergeben Sie die geplante Popup Benachrichtigung.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>Abbrechen geplanter Benachrichtigungen

Zum Abbrechen einer geplanten Benachrichtigung müssen Sie zuerst die Liste aller geplanten Benachrichtigungen abrufen.

Suchen Sie anschließend den geplanten Toast, der mit dem zuvor angegebenen Tag (und optional mit der Gruppe) übereinstimmt, und nennen Sie removefromschedule ().

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>Aktivierungs Behandlung

Weitere Informationen zum Behandeln der Aktivierung finden Sie in der Dokumentation zum [senden lokaler](send-local-toast.md) Popup-Dokumente. Die Aktivierung einer geplanten Popup Benachrichtigung erfolgt genauso wie die Aktivierung einer lokalen Popup Benachrichtigung.


## <a name="adding-actions-inputs-and-more"></a>Hinzufügen von Aktionen, Eingaben und mehr

Weitere Informationen zu erweiterten Themen, wie z. b. Aktionen und Eingaben, finden Sie in der Dokumentation [Send a local](send-local-toast.md) Popup. Aktionen und Eingaben funktionieren in lokalen-Einträgen genauso wie in geplanten-Einträgen.


## <a name="resources"></a>Ressourcen

* [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
* [Scheduleddeastnotification-Klasse](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)