---
Description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung so planen, dass Sie zu einem späteren Zeitpunkt angezeigt wird.
title: Planen einer Popup Benachrichtigung
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, geplante Popup Benachrichtigung, scheduledtoastnotification, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386429"
---
# <a name="schedule-a-toast-notification"></a>Planen einer Popup Benachrichtigung

Mit geplanten Popup Benachrichtigungen können Sie eine Benachrichtigung so planen, dass Sie zu einem späteren Zeitpunkt angezeigt wird, unabhängig davon, ob Ihre APP zu diesem Zeitpunkt ausgeführt wird. Dies ist nützlich für Szenarios wie das Anzeigen von Erinnerungen oder anderen nachfolgenden Aufgaben für den Benutzer, bei denen die Zeit und der Inhalt der Benachrichtigung im Voraus bekannt sind.

Beachten Sie, dass geplante Popup Benachrichtigungen über ein Übermittlungs Fenster von 5 Minuten verfügen. Wenn der Computer während der geplanten Übermittlung ausgeschaltet ist und länger als 5 Minuten deaktiviert bleibt, wird die Benachrichtigung als nicht mehr relevant für den Benutzer angezeigt. Wenn Sie eine garantierte Übermittlung von Benachrichtigungen unabhängig davon benötigen, wie lange der Computer ausgeschaltet war, empfiehlt es sich, wie in [diesem Codebeispiel](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)dargestellt eine Hintergrundaufgabe mit einem Zeit-und Zeit Auslösers zu verwenden.

> [!IMPORTANT]
> Für Desktop Anwendungen (msix/Sparse-Pakete und klassisches Win32) gibt es etwas andere Schritte zum Senden von Benachrichtigungen und zur Handhabung der Aktivierung. Befolgen Sie die nachfolgenden Anweisungen, aber ersetzen Sie `ToastNotificationManager` durch die `DesktopNotificationManagerCompat`-Klasse aus der [Desktop-Apps-](toast-desktop-apps.md) Dokumentation.

> **Wichtige APIs**: [scheduleddeastnotification-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Thema vollständig zu verstehen, ist Folgendes hilfreich...

* Ein grundlegendes Verständnis der Begriffe und Konzepte rund um Popupbenachrichtigungen. Weitere Informationen finden Sie unter [Übersicht über das Popup-und Aktions Center](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Kenntnisse in Bezug auf den Inhalt von Windows 10-Popupbenachrichtigungen. Weitere Informationen finden Sie in der [Dokumentation zu Popup-Inhalt](adaptive-interactive-toasts.md).
* Ein Windows 10-UWP-App-Projekt


## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

Wir empfehlen die Installation der beiden folgenden NuGet-Pakete für Ihr Projekt. In unserem Codebeispiel werden diese Pakete verwendet.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): Generieren von Popup-Nutzlasten über Objekte anstelle von unformatiertem XML.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): Generieren und Analysieren von Abfragezeichenfolgen mit C#


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespacedeklarationen

`Windows.UI.Notifications` enthält die Toast-APIs.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>Erstellen des Popup Inhalts

In Windows 10 wird der Inhalt der Popupbenachrichtigung mithilfe einer adaptiven Sprache beschrieben, die eine sehr flexible Darstellung Ihrer Benachrichtigung erlaubt. Weitere Informationen finden Sie in der [Dokumentation zu Popupinhalt](adaptive-interactive-toasts.md).

Dank der Benachrichtigungs Bibliothek ist das Erstellen des XML-Inhalts einfach. Wenn Sie die Benachrichtigungsbibliothek von NuGet nicht installieren, müssen Sie den XML-Code manuell erstellen, was zu Fehlern führen kann.

Sie sollten die **Start**-Eigenschaft immer angeben, um festzulegen, welche Art von Aktivierung erfolgen muss, wenn der Benutzer auf den Text der Popupbenachrichtigung tippt.

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

Nachdem Sie den Popup Inhalt initialisiert haben, erstellen Sie eine neue [scheduledtoastnotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) , und übergeben Sie die XML-Datei des Inhalts und die Uhrzeit, zu der die Benachrichtigung gesendet werden soll.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>Angeben eines primären Schlüssels für das Popup

Wenn Sie die geplante Benachrichtigung Programm gesteuert abbrechen, entfernen oder ersetzen möchten, müssen Sie die Tag-Eigenschaft (und optional die Group-Eigenschaft) verwenden, um einen Primärschlüssel für die Benachrichtigung anzugeben. Anschließend können Sie diesen Primärschlüssel in Zukunft verwenden, um die Benachrichtigung abzubrechen, zu entfernen oder zu ersetzen.

Weitere Informationen zum Ersetzen/Entfernen bereits übermittelter Popupbenachrichtigungen finden Sie unter [Schnellstart: Verwalten von Popupbenachrichtigungen im Info-Center (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Tag und Group fungieren kombiniert als ein zusammengesetzter primärer Schlüssel. Group ist die allgemeinere ID, wobei Sie Gruppen wie „WallPosts“, „Nachrichten“, „FriendRequests“ etc. zuweisen können. Und Tag sollte dann die Benachrichtigung selbst innerhalb der Gruppe eindeutig identifizieren. Unter Verwendung einer allgemeinen Gruppe können Sie dann mithilfe der [RemoveGroup-API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) alle Benachrichtigungen aus dieser Gruppe entfernen.

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>Planen der Benachrichtigung

Erstellen Sie abschließend einen [toastnotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) , und rufen Sie addtoschedule () auf, und übergeben Sie die geplante Popup Benachrichtigung.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>Geplante Benachrichtigungen Abbrechen

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
* [Scheduleddeastnotification-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
