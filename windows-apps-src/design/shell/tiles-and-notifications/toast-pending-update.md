---
description: Erfahren Sie, wie Sie Toast mit ausstehender Update Aktivierung verwenden können, um Interaktionen mit mehreren Schritten in ihren Popup Benachrichtigungen zu erstellen.
title: Toast mit ausstehender Update Aktivierung
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, ausstehendes Update, pdingupdate, Interaktivität mit mehreren Schritten, Interaktionen mit mehreren Schritten
ms.localizationpriority: medium
ms.openlocfilehash: bc77e41ad144c76af4452b2a9a87c183ae84422c
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984576"
---
# <a name="toast-with-pending-update-activation"></a>Toast mit ausstehender Update Aktivierung

Sie können " **" mit "** " "" "mit" "in den Popup Benachrichtigungen mehrstufige Interaktionen erstellen. Beispielsweise können Sie wie unten gezeigt eine Reihe von Popups erstellen, in denen die nachfolgenden Popups von Antworten aus den vorherigen Popups abhängen.

![Toast mit ausstehenden Updates](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Erfordert Desktop Fall Creators Update und 2.0.0 der Benachrichtigungs Bibliothek**: Sie müssen den Desktop-Build 16299 oder höher ausführen, um ausstehende Aktualisierungs arbeiten anzuzeigen. Sie müssen in der [nuget-Bibliothek der UWP-Community-Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) Version 2.0.0 oder höher verwenden, um " **kdingupdate** " auf Ihren Schaltflächen zuzuweisen. " **Pdingupdate** " wird nur auf dem Desktop unterstützt und wird auf anderen Geräten ignoriert.


## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie wissen, dass...

- [Erstellen von Popup Inhalten](adaptive-interactive-toasts.md)
- [Senden eines Toast und behandeln der Hintergrund Aktivierung](send-local-toast.md)


## <a name="overview"></a>Übersicht

So implementieren Sie einen Toast, der das ausstehende Update als nach dem Aktivierungs Verhalten verwendet...

1. Geben Sie auf den Schaltflächen für Popup-Hintergrund Aktivierung ein **afteractivationbehavior** -Ereignis vom Typ "Name **" an.**

2. Zuweisen eines **Tags** (und optional einer **Gruppe**) beim Senden des Toast

3. Wenn der Benutzer auf die Schaltfläche klickt, wird die Hintergrundaufgabe aktiviert, und der Popup wird auf dem Bildschirm in einem ausstehenden Aktualisierungs Status gespeichert.

4. Senden Sie in der Hintergrundaufgabe einen neuen Toast mit Ihrem neuen Inhalt, und verwenden Sie dabei dasselbe **Tag** und dieselbe **Gruppe** .


## <a name="assign-pendingupdate"></a>Zuweisen von "pdingupdate"

Legen Sie in den Schaltflächen für die Hintergrund Aktivierung für " **afteractivationbehavior** " den Wert " **pdingupdate**" fest Beachten Sie, dass dies nur für Schaltflächen funktioniert, die über den **ActivationType** " **Background**" verfügen.

#### <a name="builder-syntax"></a>[Builder-Syntax](#tab/builder-syntax)

```csharp
new ToastContentBuilder()

    .AddText("Would you like to order lunch today?")

    .AddButton(new ToastButton("Yes", "action=orderLunch")
    {
        ActivationType = ToastActivationType.Background,

        ActivationOptions = new ToastActivationOptions()
        {
            AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
        }
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>
  
  <visual>
    <binding template="ToastGeneric">
      <text>Would you like to order lunch today?</text>
    </binding>
  </visual>

  <actions>
    <action
      content="Yes"
      arguments="action=orderLunch"
      activationType="background"
      afterActivationBehavior="pendingUpdate"/>
  </actions>
  
</toast>
```

---


## <a name="use-a-tag-on-the-notification"></a>Verwenden eines Tags für die Benachrichtigung

Um die Benachrichtigung zu einem späteren Zeitpunkt zu ersetzen, müssen wir das **Tag** (und optional die **Gruppe**) der Benachrichtigung zuweisen.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Den Toast durch neuen Inhalt ersetzen

Wenn der Benutzer auf die Schaltfläche klickt, wird die Hintergrundaufgabe ausgelöst, und Sie müssen den Toast durch neuen Inhalt ersetzen. Sie ersetzen den Toast, indem Sie einfach einen neuen Toast mit dem gleichen **Tag** und der gleichen **Gruppe**senden.

Es wird dringend empfohlen, **das Audiogerät** bei Ersetzungen als Reaktion auf einen Schaltflächen Klick festzulegen, da der Benutzer bereits mit dem Toast interagiert.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>Zugehörige Themen

- [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Senden eines lokalen Popups und behandeln der Aktivierung](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
- [Statusanzeige für Popup](toast-progress-bar.md)