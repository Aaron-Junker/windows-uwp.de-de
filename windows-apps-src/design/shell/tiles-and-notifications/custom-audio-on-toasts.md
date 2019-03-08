---
Description: Erfahren Sie, wie Sie benutzerdefinierte Audio in Ihre Popupbenachrichtigungen zu verwenden.
title: Benutzerdefiniertes Audio auf Popups
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, benutzerdefiniertes Audio, Benachrichtigungen, Audio, Sound
ms.localizationpriority: medium
ms.openlocfilehash: 982340901d13f17945c1e7ffa11099f52732f619
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644065"
---
# <a name="custom-audio-on-toasts"></a>Benutzerdefiniertes Audio auf Popups

Popupbenachrichtigungen können benutzerdefiniertes Audio verwenden, wodurch Ihre App Ihrer Marke eindeutige Soundeffekte hinzufügt. Z. B. kann eine Nachrichten-App ihren eigenen Nachrichten-Sound in deren Popupbenachrichtigungen haben, damit der Benutzer sofort ermitteln kann, dass eine Benachrichtigung von der App erhalten wurde, anstatt die generische Benachrichtigung.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Installieren des UWP Community Toolkit NuGet-Pakets

Um Benachrichtigungen über Code zu erstellen, empfehlen wir die Verwendung der UWP Community Toolkit Benachrichtigungsbibliothek, die ein Objektmodell für die Benachrichtigung von XML-Inhalten bereitstellt. Sie können die Benachrichtigungs-XML manuell erstellen, jedoch ist diese fehleranfällig und unübersichtlich. Die Benachrichtigungsbibliothek im UWP Community Toolkit wird vom Team erstellt und verwaltet, das die Benachrichtigungen bei Microsoft besitzt.

Installieren Sie [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) von NuGet (wir verwenden Version 1.0.0 in dieser Dokumentation).


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespacedeklarationen

`Windows.UI.Notifications` enthält die Kachel "und die Popupbenachrichtigungs-API. `Microsoft.Toolkit.Uwp.Notifications` enthält die Benachrichtigungen-Bibliothek.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Erstellen der Benachrichtigung

Der Popupbenachrichtigungsinhalt umfasst Text und Bilder, sowie Schaltflächen und Eingaben. Weitere Informationen finden Sie unter [lokale Popupbenachrichtigungen senden](send-local-toast.md), um einen vollständigen Codeausschnitt zu sehen.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Hinzufügen von benutzerdefiniertem Audio

Windows Mobile unterstützt jederzeit benutzerdefinierte Audio-Popupbenachrichtigungen. Allerdings wurde dem Desktop die Unterstützung für benutzerdefiniertes Audio erst in Version 1511 (Build 10586) hinzugefügt. Wenn Sie eine Popupbenachrichtigung senden, die benutzerdefinierte Audiodaten auf einem Desktopgerät vor der Version 1511 enthält, wird das Popup lautlos ausgeführt. Aus diesem Grund sollten Sie für Desktop vor der Version 1511 kein benutzerdefiniertes Audio für Ihre Popupbenachrichtigung enthalten, damit die Benachrichtigung mindestens den Standard-Benachrichtigungssound verwenden.

**Bekanntes Problem**: Wenn Sie Desktop-Version 1511 verwenden, funktioniert das benutzerdefinierte Toast-Audio nur, wenn Ihre app über den Store installiert ist. Dies bedeutet, dass die Ihr benutzerdefiniertes Audio vor der Übermittlung an den Store nicht auf Desktop lokal testen können - die Audiowiedergabe funktioniert allerdings einwandfrei nach der Installation aus dem Store. Wir haben dieses Problem im Anniversary Update behoben, damit benutzerdefiniertes Audio über die lokal bereitgestellte App ordnungsgemäß funktioniert.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Dazu gehören folgende Audiodateitypen...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Senden der Benachrichtigung

Wenn Ihr Popupinhalt abgeschlossen ist, ist das Senden der Benachrichtigung ganz einfach.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Verwandte Themen

- [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Senden Sie einen lokalen toast](send-local-toast.md)
- [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)