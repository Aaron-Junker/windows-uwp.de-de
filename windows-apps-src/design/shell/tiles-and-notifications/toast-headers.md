---
description: Erfahren Sie, wie Sie mit Headern ihre Popup Benachrichtigungen im Aktions Center visuell gruppieren.
title: Popup Header
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, Header, Popup Header, Benachrichtigung, Gruppen Toasts, Aktions Center
ms.localizationpriority: medium
ms.openlocfilehash: e0da8fbf4d4ad3575fb3e2eca9c738b6c34a01d5
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103226134"
---
# <a name="toast-headers"></a>Popup Header

Sie können einen Satz verwandter Benachrichtigungen innerhalb des Aktions Centers visuell gruppieren, indem Sie einen Popup Header in Ihren Benachrichtigungen verwenden.

> [!IMPORTANT]
> **Erfordert Desktop Creators Update und 1.4.0 of-Benachrichtigungs Bibliothek**: Sie müssen den Desktop-Build 15063 oder höher ausführen, um Popup Header anzuzeigen. Zum Erstellen des Headers im Inhalt Ihres Popups müssen Sie Version 1.4.0 oder eine höhere Version der [nuget-Bibliothek der UWP Community Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden. Header werden nur auf dem Desktop unterstützt.

Wie unten gezeigt, ist diese Gruppen Konversation unter einem einzelnen Header ("Camping!!") einheitlich. Jede einzelne Nachricht in der Konversation ist eine separate Popup Benachrichtigung, die denselben Popup Header gemeinsam nutzen kann.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Sie können Ihre Benachrichtigungen auch nach Kategorie auch visuell gruppieren, wie z. b. Flug Erinnerungen, Paketverfolgung und mehr.

## <a name="add-a-header-to-a-toast"></a>Hinzufügen eines Headers zu einem Toast

Im folgenden wird erläutert, wie Sie einer Popup Benachrichtigung einen Header hinzufügen.

> [!NOTE]
> Header werden nur auf dem Desktop unterstützt. Geräte, die keine Header unterstützen, ignorieren einfach den-Header.

#### <a name="builder-syntax"></a>[Builder-Syntax](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddHeader("6289", "Camping!!", "action=openConversation&id=6289")
    .AddText("Anyone have a sleeping bag I can borrow?");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

---

Zusammenfassung...

1. Fügen Sie den- **Header** zu Ihrem **Inhalts Inhalts Konto** hinzu.
2. Zuweisen der erforderlichen Eigenschaften für **ID**, **Titel** und **Argumente**
3. Senden der Benachrichtigung ([Weitere Informationen](send-local-toast.md))
4. Verwenden Sie bei einer anderen Benachrichtigung dieselbe Header- **ID** , um Sie unter dem Header zu vereinheitlichen. Die **ID** ist die einzige Eigenschaft, die verwendet wird, um zu bestimmen, ob die Benachrichtigungen gruppiert werden sollen, was bedeutet, dass der **Titel** und die **Argumente** abweichen können. Der **Titel** und die **Argumente** der letzten Benachrichtigung innerhalb einer Gruppe werden verwendet. Wenn diese Benachrichtigung entfernt wird, werden der **Titel** und die **Argumente** auf die nächste aktuelle Benachrichtigung zurückgegriffen.


## <a name="handle-activation-from-a-header"></a>Behandeln der Aktivierung über einen Header

Header können von Benutzern abgeklickt werden, damit der Benutzer auf den Header klicken kann, um mehr über Ihre APP zu erfahren.

Daher können apps **Argumente** für den Header bereitstellen, ähnlich wie die Start Argumente für den Toast selbst.

Die Aktivierung erfolgt mit der [normalen Popup Aktivierung](send-local-toast.md#step-3-handling-activation), d. h., Sie können diese Argumente in der **onaktivierten** Methode von abrufen, `App.xaml.cs` wie Sie vorgehen, wenn der Benutzer auf den Hauptteil des Popup-oder auf eine Schaltfläche im Toast klickt.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Zusätzliche Informationen

Der-Header trennt und gruppiert Benachrichtigungen visuell. Es wird keine andere Logistik in Bezug auf die maximale Anzahl von Benachrichtigungen, die eine APP haben kann (20) und das First-in-First-Out-Verhalten der Benachrichtigungsliste ändern.

Die Reihenfolge der Benachrichtigungen in den Headern lautet wie folgt... Für eine bestimmte APP wird die aktuellste Benachrichtigung von der APP (und die gesamte Header Gruppe, wenn Teil eines Headers vorhanden ist) zuerst angezeigt.

Die **ID** kann eine beliebige Zeichenfolge sein, die Sie auswählen. Es gibt keine Längen-oder Zeichen **Einschränkungen für eine der Eigenschaften in "**". Die einzige Einschränkung besteht darin, dass der gesamte XML-Popup Inhalt nicht größer als 5 KB sein darf.

Das Erstellen von Headern ändert nicht die Anzahl der Benachrichtigungen, die im Aktions Center angezeigt werden, bevor die Schaltfläche "Weitere Informationen" angezeigt wird (diese Zahl ist standardmäßig auf 3 festgelegt und kann vom Benutzer für jede app in den Systemeinstellungen für Benachrichtigungen konfiguriert werden).

Wenn Sie auf die Kopfzeile klicken, so wie beim Klicken auf den APP-Titel, werden keine Benachrichtigungen gelöscht, die zu diesem Header gehören (Ihre APP sollte die Popup-APIs verwenden, um die relevanten Benachrichtigungen zu löschen).


## <a name="related-topics"></a>Zugehörige Themen

- [Senden eines lokalen Popups und behandeln der Aktivierung](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
