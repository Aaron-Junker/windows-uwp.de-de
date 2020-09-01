---
description: Verwenden Sie eine Statusanzeige innerhalb der Popup Benachrichtigung, um dem Benutzer den Status der Vorgänge mit langer Ausführungsdauer zu übermitteln.
title: Statusanzeige für Popup und Datenbindung
label: Toast progress bar and data binding
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, Statusanzeige, Popup Statusanzeige, Benachrichtigung, Popup Datenbindung
ms.localizationpriority: medium
ms.openlocfilehash: 4219154a3fe3241b9c1871c07a1fbbb2b63f2348
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174604"
---
# <a name="toast-progress-bar-and-data-binding"></a>Statusanzeige für Popup und Datenbindung

Durch die Verwendung einer Statusanzeige in der Popup Benachrichtigung können Sie den Status von Vorgängen mit langer Ausführungsdauer an den Benutzer weitergeben, z. b. Downloads, Video Rendering, Testziele und vieles mehr.

> [!IMPORTANT]
> **Erfordert Creators Update und 1.4.0 der Benachrichtigungs Bibliothek**: Sie müssen das SDK 15063 als Ziel verwenden und Build 15063 oder höher ausführen, um Status anzeigen in den Toasts zu verwenden. Um die Statusanzeige im Inhalt Ihres Popups zu erstellen, müssen Sie die Version 1.4.0 oder eine höhere Version der [nuget-Bibliothek der UWP Community Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden.

Eine Statusanzeige in einem Popup kann entweder "unbestimmt" (kein spezifischer Wert, animierte Punkte zeigen, dass ein Vorgang auftritt) oder "determinate" lauten (ein bestimmter Prozentsatz des Balkens wird aufgefüllt, z. b. 60%).

> **Wichtige APIs**: [notificationdata-Klasse](/uwp/api/windows.ui.notifications.notificationdata), " [tastnotifier. Update](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update)"-Methode, " [tastnotification](/uwp/api/Windows.UI.Notifications.ToastNotification) "-Klasse

> [!NOTE]
> Nur der Desktop unterstützt Status leisten in Popup Benachrichtigungen. Auf anderen Geräten wird die Statusanzeige aus der Benachrichtigung gelöscht.

Die folgende Abbildung zeigt eine bestimmte-Statusanzeige mit allen zugehörigen Eigenschaften mit der Bezeichnung.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Eigenschaft | Typ | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Titel** | String oder [bindablestring](toast-schema.md#bindablestring) | false | Ruft eine optionale Zeichenfolge ab oder legt Sie fest. Unterstützt die Datenbindung. |
| **Wert** | Double oder [adaptiveprogressbarvalue](toast-schema.md#adaptiveprogressbarvalue) oder [bindableprogressbarvalue](toast-schema.md#bindableprogressbarvalue) | false | Ruft den Wert der Statusanzeige ab oder legt ihn fest. Unterstützt die Datenbindung. Der Standardwert ist 0. Kann entweder ein Double zwischen 0,0 und 1,0, `AdaptiveProgressBarValue.Indeterminate` oder sein `new BindableProgressBarValue("myProgressValue")` . |
| **Valuestringoverride** | String oder [bindablestring](toast-schema.md#bindablestring) | false | Ruft eine optionale Zeichenfolge ab, die anstelle der standardmäßigen Prozent Zeichenfolge angezeigt wird, oder legt diese fest Wenn dies nicht angegeben wird, wird etwa "70%" angezeigt. |
| **Status** | String oder [bindablestring](toast-schema.md#bindablestring) | true | Ruft eine Status Zeichenfolge (erforderlich) ab, die unterhalb der Statusleiste auf der linken Seite angezeigt wird, oder legt Sie fest. Diese Zeichenfolge sollte den Status des Vorgangs widerspiegeln, wie z. b. "herunterladen..." oder "wird installiert..." |


Hier sehen Sie, wie Sie die oben gezeigte Benachrichtigung generieren...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

Allerdings müssen Sie die Werte der Statusanzeige dynamisch aktualisieren, damit Sie tatsächlich "Live" ist. Dies kann mithilfe der Datenbindung zum Aktualisieren des Popups erreicht werden.


## <a name="using-data-binding-to-update-a-toast"></a>Verwenden der Datenbindung zum Aktualisieren eines Popups

Die Verwendung der Datenbindung umfasst die folgenden Schritte...

1. Erstellen von Popup Inhalten, die Daten gebundene Felder verwendet
2. Zuweisen eines **Tags** (und optional einer **Gruppe**) zu Ihrer **Anmelde Benachrichtigung**
3. Definieren Sie Ihre anfänglichen **Daten** Werte in Ihrer **Benachrichtigung** .
4. Toast senden
5. Verwenden von **Tag** und **Gruppe** zum Aktualisieren der **Daten** Werte mit neuen Werten

Der folgende Code Ausschnitt zeigt die Schritte 1-4. Der nächste Code Ausschnitt zeigt, wie die Popup **Daten** Werte aktualisiert werden.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

Wenn Sie dann die **Daten** Werte ändern möchten, verwenden Sie die [**Update**](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) -Methode, um die neuen Daten bereitzustellen, ohne die gesamte Toast Nutzlast neu zu erstellen.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

Wenn Sie die **Update** -Methode verwenden, anstatt den gesamten Toast zu ersetzen, wird auch sichergestellt, dass die Popup Benachrichtigung an der gleichen Position im Aktions Center bleibt und nicht nach oben oder unten verschoben wird. Es wäre ziemlich verwirrend für den Benutzer, wenn der Toast alle paar Sekunden auf den Anfang des Aktions Centers springt, während die Statusanzeige gefüllt ist.

Die **Update** -Methode gibt eine Enum- [**notificationupdateresult**](/uwp/api/windows.ui.notifications.notificationupdateresult)zurück, mit der Sie feststellen können, ob die Aktualisierung erfolgreich war oder ob die Benachrichtigung nicht gefunden werden konnte (was bedeutet, dass der Benutzer die Benachrichtigung wahrscheinlich verworfen hat und Sie das Senden von Updates nicht beenden sollten). Es wird nicht empfohlen, einen anderen Toast zu löschen, bis der Status Vorgang abgeschlossen ist (z. b. wenn der Download abgeschlossen ist).


## <a name="elements-that-support-data-binding"></a>Elemente, die die Datenbindung unterstützen
Die folgenden Elemente in Toast Benachrichtigungen unterstützen die Datenbindung.

- Alle Eigenschaften bei **adaptiveprogress**
- Die **Text** -Eigenschaft für die **adaptivetext** -Elemente der obersten Ebene.


## <a name="update-or-replace-a-notification"></a>Aktualisieren oder Ersetzen einer Benachrichtigung

Seit Windows 10 könnten Sie eine Benachrichtigung immer **ersetzen** , indem Sie einen neuen Toast mit dem gleichen **Tag** und der gleichen **Gruppe**senden. Worin besteht der Unterschied zwischen dem **austauschen** und dem **Aktualisieren** der Popup Daten?

| | Setzten | Wird aktualisiert |
| -- | -- | --
| **Position im Aktions Center** | Verschiebt die Benachrichtigung an den oberen Rand des Aktions Centers. | Verbleibt die Benachrichtigung innerhalb des Aktions Centers. |
| **Ändern von Inhalten** | Kann den gesamten Inhalt/das Layout des Popups vollständig ändern. | Es können nur Eigenschaften geändert werden, die die Datenbindung unterstützen (Statusanzeige und Text der obersten Ebene). |
| **Wiedergeben als Popup** | Kann als Popup Popup angezeigt werden, wenn Sie **suppresspopup** auf festlegen `false` (oder auf true festlegen, um Sie automatisch an das Aktions Center zu senden). | Wird nicht erneut als Popup angezeigt. die Popup Daten werden im Aktions Center automatisch aktualisiert. |
| **Benutzer wurde verworfen.** | Unabhängig davon, ob der Benutzer die vorherige Benachrichtigung verworfen hat, wird der Ersatz Toast immer gesendet. | Wenn der Benutzer den Popup-Vorgang verworfen hat, schlägt das Popup Update fehl |

Im allgemeinen **ist die Aktualisierung nützlich für...**

- Informationen, die sich häufig in kurzer Zeit ändern und nicht vor dem Eingreifen des Benutzers herangezogen werden müssen
- Feine Änderungen am Popup Inhalt, z. b. Ändern von 50% in 65%

Häufig wird empfohlen, nach Abschluss der Abfolge der Updates (z. b. wenn die Datei heruntergeladen wurde) für den letzten Schritt zu ersetzen, weil...

- Ihre endgültige Benachrichtigung weist wahrscheinlich drastische Layoutänderungen auf, wie z. b. das Entfernen der Statusanzeige, das Hinzufügen neuer Schaltflächen usw.
- Möglicherweise hat der Benutzer Ihre ausstehende Fortschritts Benachrichtigung verworfen, da er sich nicht um das Herunterladen kümmert, sondern dennoch mit einem Popup Popup benachrichtigt werden soll, wenn der Vorgang abgeschlossen ist.


## <a name="related-topics"></a>Zugehörige Themen

- [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)