---
description: Erfahren Sie, wie Sie benutzerdefiniertes Audio für ihre Popup Benachrichtigungen verwenden, damit Ihre APP die eindeutigen Soundeffekte Ihrer Marke Ausdrücken kann.
title: Benutzerdefiniertes Audiogerät
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, benutzerdefiniertes Audio, Benachrichtigung, Audio, Sound
ms.localizationpriority: medium
ms.openlocfilehash: 905292155dfc43a82c464edb651b2d176aeab960
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103226124"
---
# <a name="custom-audio-on-toasts"></a>Benutzerdefiniertes Audiogerät

Popup Benachrichtigungen können benutzerdefinierte Audiodaten verwenden, mit denen Ihre APP die eindeutigen Soundeffekte Ihrer Marke Ausdrücken kann. Beispielsweise kann eine Messaging-App ihren eigenen Messaging Sound in ihren Popup Benachrichtigungen verwenden, sodass der Benutzer sofort weiß, dass er eine Benachrichtigung von der APP erhalten hat, anstatt den generischen Benachrichtigungs Sound zu hören.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Nuget-Paket für das UWP Community Toolkit installieren

Um Benachrichtigungen per Code zu erstellen, wird dringend empfohlen, die UWP Community Toolkit-Benachrichtigungs Bibliothek zu verwenden, die ein Objektmodell für den Inhalt der Benachrichtigungs-XML bereitstellt. Sie könnten die Benachrichtigungs-XML manuell erstellen, aber das ist fehleranfällig und messy. Die Benachrichtigungs Bibliothek im UWP Community Toolkit wird von dem Team erstellt und verwaltet, das über Benachrichtigungen bei Microsoft verfügt.

Installieren Sie [Microsoft. Toolkit. UWP. Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) von nuget.


## <a name="add-namespace-declarations"></a>Hinzufügen von Namespace-Deklarationen

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
```


## <a name="add-the-custom-audio"></a>Hinzufügen der benutzerdefinierten Audiodatei

Windows Mobile hat benutzerdefinierte Audiodaten in Popup Benachrichtigungen immer unterstützt. Der Desktop hat jedoch nur Unterstützung für benutzerdefinierte Audiodaten in Version 1511 (Build 10586) hinzugefügt. Wenn Sie vor Version 1511 einen Toast, der benutzerdefinierte Audiodaten enthält, an ein Desktop Gerät senden, wird der Toast automatisch angezeigt. Daher sollten Sie für Desktop-Pre-Version 1511 die benutzerdefinierte Audiodatei nicht in ihre Popup Benachrichtigung einschließen, damit die Benachrichtigung zumindest das standardmäßige Benachrichtigungs Sound verwendet.

**Bekanntes Problem**: Wenn Sie die Desktop Version 1511 verwenden, funktioniert die benutzerdefinierte Popup-Audiodatei nur, wenn Ihre APP über den Store installiert wird. Dies bedeutet, dass Sie Ihre benutzerdefinierte Audiodatei nicht lokal auf dem Desktop testen können, bevor Sie Sie an den Speicher senden. das Audiogerät funktioniert jedoch nach der Installation aus dem Store problemlos. Wir haben dies im Anniversary Update korrigiert, sodass die benutzerdefinierte Audiodatei aus Ihrer lokal bereitgestellten App ordnungsgemäß funktioniert.

```csharp
var contentBuilder = new ToastContentBuilder()
    .AddText("New message");

    
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
    contentBuilder.AddAudio(new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a"));
}

// Send the toast
contentBuilder.Show();
```

Zu den unterstützten Audiodateitypen zählen...

- .aac
- . flac
- .m4a
- .mp3
- WAV
- .wma


## <a name="send-the-notification"></a>Senden der Benachrichtigung

Das Senden einer Benachrichtigung mit Audiodaten entspricht dem Senden einer regulären Benachrichtigung (nur die Show-Methode). Weitere Informationen finden Sie unter [Send local Toast](send-local-toast.md) .


## <a name="related-topics"></a>Zugehörige Themen

- [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Lokalen Toast senden](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
