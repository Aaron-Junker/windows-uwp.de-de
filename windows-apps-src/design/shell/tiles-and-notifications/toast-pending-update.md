---
Description: Erfahren Sie, wie Sie mit mehreren Schritte Interaktionen in Ihren Benachrichtigungen erstellen.
title: Popups mit ausstehenden Updates in Aktion
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, ausstehende Updates, ausstehendes Update, Interaktivität aus mehreren Schritten, Interaktivitäten aus mehreren Schritten
ms.localizationpriority: medium
ms.openlocfilehash: b1574ee2913bd2889af204aae1089dc170df95b8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648555"
---
# <a name="toast-with-pending-update-activation"></a>Popups mit ausstehenden Updates in Aktion

Verwenden Sie das **PendingUpdate** zum Erstellen der Interaktivität aus mehreren Schritten innerhalb Ihrer Popups. Beispielsweise können Sie, wie unten dargestellt, eine Reihe von Popups erstellen, deren Antworten von den vorherigen Popups der nachfolgenden Popupbenachrichtigungen abhängt.

![Popup mit ausstehendem Update](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Erfordert Desktop Fall Creators Update und 2.0.0 des benachrichtigungsbibliothek**: Sie müssen Desktop Build 16299 oder höher, um die ausstehende Arbeit aktualisieren finden Sie unter ausgeführt werden. Sie müssen Version 2.0.0 oder höher der [UWP Community Toolkit Benachrichtigungen NuGet-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um **PendingUpdate** auf Ihre Schaltflächen anzuwenden. **PendingUpdate** wird nur auf Desktop unterstützt und auf anderen Geräten ignoriert.


## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel erfordert Grundkenntnisse in...

- [Erstellen von toasts Inhalt](adaptive-interactive-toasts.md)
- [Ein Popup senden und Behandeln von Hintergrund-Aktivierung](send-local-toast.md)


## <a name="overview"></a>Übersicht

Um ein Popup zu implementieren, das „ausstehende Updates” als Verhalten nach der Aktivierung verwendet...

1. Geben Sie auf den Schaltflächen für die Hintergrundaktivierung ein **AfterActivationBehavior** des **PendingUpdate** an

2. Weisen Sie beim Senden Ihrer Popups einen **Tag** (und optional eine **Gruppe**) hinzu

3. Wenn der Benutzer die Schaltfläche anklickt, wird die Hintergrundaufgabe aktiviert und das Popup auf dem Bildschirm in einem Zustand des ausstehenden Updates beibehalten

4. Senden Sie in Ihrer Hintergrundaufgabe eine neue Popupbenachrichtigung mit dem neuen Inhalt, mit dem gleichen **Tag** und der **Gruppe**


## <a name="assign-pendingupdate"></a>Zuweisen von PendingUpdate

Setzen Sie auf den Schaltflächen für die Hintergrundaktivierung das **AfterActivationBehavior** auf **PendingUpdate**. Beachten Sie, dass dies nur für Schaltflächen funktioniert, die einen **ActivationType** im **Hintergrund** habe.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>Verwenden Sie ein Tag auf der Benachrichtigung

Um die Benachrichtigung später zu ersetzen, müssen wir den **Tag** (und optional die **Gruppe**) auf die Benachrichtigung zuweisen.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Ersetzen Sie das Popup mit neuem Inhalt

Wenn der Benutzer auf die Schaltfläche klickt, wird die Hintergrundaufgabe ausgelöst, und das Popup muss durch neuen Inhalt ersetzt werden. Ersetzen Sie das Popup durch Senden einer neuen Popupbenachrichtigung mit den gleichen **Tag** und der gleichen **Gruppe**.

Es wird dringend empfohlen, als Antwort auf eine angeklickte Schaltfläche die **Audiowiedergabe auf lautlos festzulegen**, da der Benutzer bereits mit Ihrem Popup interagiert.

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


## <a name="related-topics"></a>Verwandte Themen

- [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Senden Sie eine lokale Popup- und Handle-Aktivierung](send-local-toast.md)
- [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)
- [Toast-Statusanzeige](toast-progress-bar.md)