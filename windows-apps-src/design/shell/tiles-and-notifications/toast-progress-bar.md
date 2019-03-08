---
Description: Erfahren Sie, wie eine Statusanzeige in Ihrer Toast-Benachrichtigung verwenden.
title: Popup-Statusanzeige und Datenbindungen
label: Toast progress bar and data binding
template: detail.hbs
ms.date: 12/7/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, Statusanzeige, Popup-Statusanzeige, Benachrichtigungen, Datenbindung der Popups
ms.localizationpriority: medium
ms.openlocfilehash: f955f2a71fed6444c65f9550e1f4fa3baeabe092
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624825"
---
# <a name="toast-progress-bar-and-data-binding"></a>Popup-Statusanzeige und Datenbindungen

Durch die Verwendung einer Statusanzeige innerhalb Ihrer Popupbenachrichtigung können Sie den Status der zeitintensiven Vorgänge für den Benutzer vermitteln, z. B. Downloads, Videowiedergabe, Übungen und vieles mehr.

> [!IMPORTANT]
> **Erfordert die Creators Update und 1.4.0 von benachrichtigungsbibliothek**: Sie müssen SDK 15063 ausgerichtet und Build 15063 oder höher, um Statusanzeigen für Popups verwenden ausgeführt werden. Sie müssen Version 1.4.0 oder höher der [UWP Community Toolkit Benachrichtigungen NuGet-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um die Statusanzeige der Popup-Inhalte zu erstellen.

Eine Statusanzeige in einem Popup kann entweder "unbestimmt" sein (keine bestimmten Wert, animierte Punkte als Hinweis auf ein Vorgang stattfindet) oder "bestimmte" (einem angegebenen Prozentsatz des Balkens wird aufgefüllt, z. B. 60 %).

> **Wichtige APIs**: [NotificationData Klasse](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata), [ToastNotifier.Update Methode](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [ToastNotification-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Nur Desktop unterstützt Statusanzeigen in Popupbenachrichtigungen. Auf anderen Geräten wird die Statusanzeige aus der Benachrichtigung gelöscht.

Die folgende Abbildung zeigt eine exakte Statusanzeige mit allen entsprechenden Eigenschaften mit Bezeichnung.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Titel** | Zeichenfolge oder [BindableString](toast-schema.md#bindablestring) | false | Ruft einen optionalen Zeichenfolgetitel auf oder legt diesen fest. Unterstützt die Datenbindung. |
| **Wert** | doppelt oder [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) oder [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | Ruft den Wert der Statusanzeige auf oder legt diesen fest. Unterstützt die Datenbindung. Die Standardeinstellung ist 0. Kann entweder ein Double zwischen 0,0 und 1,0, `AdaptiveProgressBarValue.Indeterminate` oder `new BindableProgressBarValue("myProgressValue")` sein. |
| **ValueStringOverride** | Zeichenfolge oder [BindableString](toast-schema.md#bindablestring) | false | Ruft eine optionale Zeichenfolge auf oder legt diese fest, damit sie anstelle der Standardzeichenfolge in Prozent angezeigt wird. Wenn dies nicht bereitgestellt ist, wird etwas wie "70 %" angezeigt. |
| **Status** | Zeichenfolge oder [BindableString](toast-schema.md#bindablestring) | true | Ruft eine Statuszeichenfolge auf oder legt diese fest (erforderlich), die unter der Statusanzeige auf der linken Seite angezeigt wird. Diese Zeichenfolge sollte den Status des Vorgangs widerspiegeln, z. B. "Herunterladen..." oder "Installieren von..." |


So würde die oben dargestellte Benachrichtigung generiert...

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

Allerdings müssen Sie die Werte der Statusanzeige dynamisch aktualisieren, da diese tatsächlich "live" ist. Dies kann durch die Verwendung einer Datenbindung erfolgen, um das Popup zu aktualisieren.


## <a name="using-data-binding-to-update-a-toast"></a>Verwenden der Datenbindung zum Aktualisieren eines Popups

Das Verwenden der Datenbindung umfasst die folgenden Schritte...

1. Erstellen von Popup-Inhalten, die Datenbildungsfelder verwenden
2. Fügen Sie einen **Tag** (und optional eine **Gruppe**) auf **ToastNotification** hinzu
3. Definieren Sie Ihre ersten **Daten**werte für Ihre **ToastNotification**
4. Senden Sie das Popup
5. Nutzen Sie **Tag** und **Gruppe** zum Aktualisieren der **Daten** mit neuen Werten

Der folgende Codeausschnitt zeigt die Schritte 1 bis 4. Der nächste Codeausschnitt zeigt, wie das Popup **Daten**werte aktualisiert.

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

Wenn Sie Ihre **Daten**werte ändern möchten, verwenden Sie die [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update)-Methode, um die neuen Daten bereitzustellen, ohne die gesamte Popupnutzlast neu zu erstellen.

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

Statt das gesamte Popup zu ersetzen, sorgt die **Update**-Methode dafür, dass die Popupbenachrichtigung in der gleichen Position im Info-Center bleibt und nicht nach oben oder unten verschoben wird. Es wäre verwirrend für den Benutzer, wenn das Popup alle paar Sekunden an den Anfang des Info-Centers verschoben würde, während die Statusanzeige gefüllt wird.

Die **Update**-Methode gibt eine Enumeration wieder, [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult), und teilt Ihnen mit, ob das Update erfolgreich war oder ob die Benachrichtigung nicht gefunden werden konnte (d. h. der Benutzer hat möglicherweise die Benachrichtigung geschlossen und Sie müssen aufhören, Updates zu senden). Es wird nicht empfohlen, ein anderes Popup einzublenden, bis der Vorgang abgeschlossen ist (z. B. wenn der Download abgeschlossen ist).


## <a name="elements-that-support-data-binding"></a>Elemente, die die Datenbindung unterstützen
Folgende Elemente in Popupbenachrichtigungen unterstützen die Datenbindung

- Alle Eigenschaften für **AdaptiveProgress**
- Die **Text**-Eigenschaft auf der obersten Ebene des **AdaptiveText**-Elements


## <a name="update-or-replace-a-notification"></a>Aktualisieren oder Ersetzen einer Benachrichtigung

Ab Windows 10 könnten Sie immer eine Benachrichtigung **ersetzen**, indem Sie eine neue Popupbenachrichtigung mit dem gleichen **Tag** und der gleichen **Gruppe** senden. Worin liegt der Unterschied zwischen dem **Ersetzen** des Popups und dem **Aktualisieren** der Popupdaten?

| | Ersetzen | Aktualisieren |
| -- | -- | --
| **Position im Info-Center** | Verschiebt die Benachrichtigung oben auf das Info-Center. | Behält die Stelle der Benachrichtigung im Info-Center bei. |
| **Ändern von Inhalt** | Alle Inhalte/Layout des Popups können dadurch vollständig geändert werden. | Kann nur Eigenschaften ändern, die die Datenbindung unterstützen (Statusanzeige und Text der obersten Ebene). |
| **Wieder als Popup angezeigt.** | Kann als Popup eingeblendet werden, wenn Sie **SuppressPopup** auf `false` festgelegt haben (oder auf „wahr”, um es lautlos im Hintergrund an das Info-Center zu senden) | Wird nicht erneut als Popup angezeigt; die Popup-Daten werden im Info-Center automatisch aktualisiert. |
| **Benutzer, die geschlossen** | Unabhängig davon, ob der Benutzer die vorherige Benachrichtigung verworfen hat, wird der Popup-Ersatz immer gesendet | Wenn der Benutzer das Popup verworfen hat, schlägt das Popup-Update fehl |

Allgemein: **Aktualisierung ist besonders nützlich für...**

- Informationen, die sich häufig und in kurzer Zeit ändern und die nicht die Aufmerksamkeit des Benutzers erfordern
- Kleine Verbesserungen am Popupinhalt, z. B. das Ändern von 50 % auf 65 %

Nachdem die Sequenz der Updates abgeschlossen ist, (z. B. die Datei heruntergeladen wurden) empfehlen wir den letzten Schritt zu ersetzen, da...

- Die letzte Benachrichtigung hat wahrscheinlich drastische Layoutänderungen, z. B. ein Entfernen der Statusanzeige, das Hinzufügen neuer Schaltflächen usw.
- Der Benutzer hat möglicherweise die ausstehenden Statusbenachrichtigung verworfen, da er den Download nicht ansehen möchte, er sollte allerdings weiterhin über ein Popup benachrichtigt werden, wenn der Vorgang abgeschlossen ist


## <a name="related-topics"></a>Verwandte Themen

- [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)