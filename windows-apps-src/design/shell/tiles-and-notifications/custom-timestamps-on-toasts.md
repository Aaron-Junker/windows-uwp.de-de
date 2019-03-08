---
Description: Erfahren Sie, wie Sie benutzerdefinierte Zeitstempel in Ihrer Popupbenachrichtigungen zu verwenden.
title: Popup mit benutzerdefiniertem Zeitstempel
label: Custom timestamps on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, benutzerdefinierte Zeitstempel, Zeitstempel, Benachrichtigungen, Info-Center
ms.localizationpriority: medium
ms.openlocfilehash: c18c32e1dcee5486ff6545a1db0ec8f0cd67bfae
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625815"
---
# <a name="custom-timestamps-on-toasts"></a>Popup mit benutzerdefiniertem Zeitstempel

Standardmäßig wird der Zeitstempel in Popupbenachrichtigungen (im Info-Center sichtbar) auf die Zeit festgelegt, an der die Benachrichtigung gesendet wurde.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Sie können optional den Zeitstempel durch Ihr eigenes benutzerdefiniertes Datum und Uhrzeit überschreiben, sodass der Zeitstempel die Zeit darstellt, zu der die Meldung/Informationen/Inhalt erstellt wurde, anstatt der Zeit, zu der die Benachrichtigung gesendet wurde. Dadurch wird auch sichergestellt, dass Ihre Benachrichtigungen in der richtigen Reihenfolge im Info-Center angezeigt werden (nach Zeit sortiert). Es wird empfohlen, dass die meisten Apps einen benutzerdefinierten Zeitstempel angeben.

> [!IMPORTANT]
> **Erfordert die Creators Update und 1.4.0 von benachrichtigungsbibliothek**: Sie müssen Build 15063 oder höher, um benutzerdefinierte Zeitstempel finden Sie unter ausgeführt werden. Sie müssen Version 1.4.0 oder höher der [UWP Community Toolkit Benachrichtigungen NuGet-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)verwenden, um Zeitstempel auf Popup-Inhalte anzuwenden.

Um einen benutzerdefinierten Zeitstempel zu verwenden, weisen Sie einfach die **DisplayTimestamp**-Eigenschaft auf Ihren **ToastContent** zu.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

Wenn Sie XML verwenden, muss das Datum nach [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatiert sein.

> [!NOTE]
> Sie können nur maximal 3 Dezimalstellen pro Sekunden verwenden (obwohl diese Details realistisch gesehen unwichtig sind). Wenn Sie mehr bereitstellen, wird die Nutzlast ungültig, und Sie erhalten die Benachrichtigung "neue Benachrichtigung".


## <a name="usage-guidance"></a>Informationen zur Verwendung

Es wird generell empfohlen, dass die meisten Apps einen benutzerdefinierten Zeitstempel angeben. Dadurch wird sichergestellt, dass der Zeitstempel der Benachrichtigung das exakte Erstellen der Nachricht/Informationen/Inhalt angibt, unabhängig von Verzögerungen im Netzwerk, Flugzeugmodus oder der festen Intervallen von regelmäßigen Hintergrundaufgaben.

Eine Nachrichten-App kann z. B. eine Hintergrundaufgabe alle 15 Minuten ausführen, die nach neuen Artikeln sucht und Benachrichtigungen anzeigt. Vor benutzerdefinierten Zeitstempeln entsprach der Zeitstempel dem Moment, wenn die Popupbenachrichtigung generiert wurde (daher ist der Intervall alle 15 Minuten). Jetzt kann die App jedoch den Zeitstempel auf die Zeit festlegen, an der der Artikel tatsächlich veröffentlicht wurde. Auf ähnliche Weise können E-Mail-Apps und soziale Netzwerk-Apps von dieser Funktion profitieren, wenn ein ähnliches Muster mit regelmäßigem Ziehen für ihre Benachrichtigungen verwendet wird.

Durch das Bereitstellen eines benutzerdefinierten Zeitstempels wird außerdem sichergestellt, dass der Zeitstempel richtig ist, auch wenn der Benutzer vom Internet getrennt wurde. Wenn der Benutzer seinen Computer einschaltet und die Hintergrundaufgabe ausgeführt wird, stellt der Zeitstempel Ihrer Benachrichtigungen die Zeit dar, als der Benutzer die Benachrichtigung gesendet hat, anstatt dem Einschalten des Computers.


## <a name="default-timestamp"></a>Standard-Zeitstempel

Wenn Sie keinen benutzerdefinierten Zeitstempel bereitstellen, verwenden wir die Zeit, zu der die Benachrichtigung gesendet wurde.

Wenn Sie eine Pushbenachrichtigungen über WNS senden, verwenden wir die Zeit, zu der die Benachrichtigung vom WNS-Server empfangen wurde, (damit eine Verzögerung der Benachrichtigung den Zeitstempel auf dem Gerät nicht beeinträchtigt).

Wenn Sie eine lokale Benachrichtigung senden, verwenden wir die Uhrzeit, zu der die Benachrichtigungsplattform die Benachrichtigung erhalten hat (die sofort sein sollte).


## <a name="related-topics"></a>Verwandte Themen

- [Senden Sie einen lokalen toast](send-local-toast.md)
- [Toast-Content-Dokumentation](adaptive-interactive-toasts.md)