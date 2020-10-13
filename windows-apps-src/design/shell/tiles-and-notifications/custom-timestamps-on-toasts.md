---
title: Benutzerdefinierte Zeitstempel in den Umfassungen
description: Erfahren Sie, wie Sie den Standardzeit Stempel für eine Popup Benachrichtigung mit einem benutzerdefinierten Zeitstempel überschreiben, der angibt, wann die Meldung/Informationen/der Inhalt generiert wurde.
label: Custom timestamps on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, benutzerdefinierter Zeitstempel, timestamp, Benachrichtigung, Aktions Center
ms.localizationpriority: medium
ms.openlocfilehash: 23e5337fe2ce30e1172aedc034a8b4eb3821a615
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984426"
---
# <a name="custom-timestamps-on-toasts"></a>Benutzerdefinierte Zeitstempel in den Umfassungen

Standardmäßig ist der Zeitstempel für Popup Benachrichtigungen (sichtbar innerhalb des Aktions Centers) auf den Zeitpunkt festgelegt, zu dem die Benachrichtigung gesendet wurde.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Sie können den Zeitstempel optional mit Ihrem eigenen benutzerdefinierten Datum und ihrer eigenen Uhrzeit überschreiben, sodass der Zeitstempel den Zeitpunkt darstellt, zu dem die Nachricht bzw. der Inhalt tatsächlich erstellt wurde, und nicht die Uhrzeit, zu der die Benachrichtigung gesendet wurde. Dadurch wird auch sichergestellt, dass Ihre Benachrichtigungen in der richtigen Reihenfolge im Aktions Center angezeigt werden (die nach Zeit sortiert sind). Es wird empfohlen, dass die meisten apps einen benutzerdefinierten Zeitstempel angeben.

> [!IMPORTANT]
> **Erfordert Creators Update und 1.4.0 der Benachrichtigungs Bibliothek**: Sie müssen Build 15063 oder höher ausführen, um benutzerdefinierte Zeitstempel anzuzeigen. Sie müssen Version 1.4.0 oder höher der [nuget-Bibliothek der UWP-Community Toolkit-Benachrichtigungen](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um den Zeitstempel für den Inhalt Ihres Popup zuzuweisen.

Um einen benutzerdefinierten Zeitstempel zu verwenden, weisen Sie dem **Inhalts**Verzeichnis einfach die **displaytimestamp** -Eigenschaft zu.

#### <a name="builder-syntax"></a>[Builder-Syntax](#tab/builder-syntax)

```csharp
var content = new ToastContent()
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc))
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---

Wenn Sie XML verwenden, muss das Datum in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)formatiert werden.

> [!NOTE]
> Sie können maximal 3 Dezimalstellen in den Sekunden verwenden (obwohl es in der realistischsten Weise keinen Wert gibt, etwas genau zu bieten). Wenn Sie weitere bereitstellen, ist die Nutzlast ungültig, und Sie erhalten die Benachrichtigung "neue Benachrichtigung".


## <a name="usage-guidance"></a>Informationen zur Verwendung

Im Allgemeinen wird empfohlen, dass die meisten apps einen benutzerdefinierten Zeitstempel angeben. Dadurch wird sichergestellt, dass der Zeitstempel der Benachrichtigung genau angibt, wann die Nachricht/Informationen/der Inhalt generiert wurde, unabhängig von den Netzwerkverzögerungen, dem Flugzeug Modus oder dem festgelegten Intervall von periodischen Hintergrundaufgaben.

Beispielsweise kann eine News-App alle 15 Minuten eine Hintergrundaufgabe ausführen, die auf neue Artikel überprüft und Benachrichtigungen anzeigt. Vor benutzerdefinierten Zeitstempeln der Zeitstempel, der beim Generieren der Popup Benachrichtigung entsprach (daher immer in 15-Minuten-Intervallen). Jetzt kann die APP den Zeitstempel jedoch auf den Zeitpunkt festlegen, zu dem der Artikel tatsächlich veröffentlicht wurde. Auf ähnliche Weise können e-Mail-apps und Apps für soziale Netzwerke von dieser Funktion profitieren, wenn ein ähnliches Muster für regelmäßige Pullen für Ihre Benachrichtigungen verwendet wird.

Außerdem wird durch die Bereitstellung eines benutzerdefinierten Zeitstempels sichergestellt, dass der Zeitstempel korrekt ist, auch wenn der Benutzer nicht mit dem Internet getrennt war. Wenn z. b. der Benutzer seinen Computer eingeschaltet und die Hintergrundaufgabe ausgeführt wird, können Sie schließlich sicherstellen, dass der Zeitstempel Ihrer Benachrichtigungen die Zeit darstellt, die die Nachrichten gesendet wurden, und nicht die Zeit, zu der der Benutzer seinen Computer eingeschaltet hat.


## <a name="default-timestamp"></a>Standardzeit Stempel

Wenn Sie keinen benutzerdefinierten Zeitstempel angeben, verwenden wir die Zeit, zu der die Benachrichtigung gesendet wurde.

Wenn Sie eine Pushbenachrichtigung über WNS gesendet haben, verwenden wir den Zeitpunkt, zu dem die Benachrichtigung vom WNS-Server empfangen wurde (daher wirkt sich jede Latenz bei der Übermittlung der Benachrichtigung an das Gerät nicht auf den Zeitstempel aus).

Wenn Sie eine lokale Benachrichtigung gesendet haben, verwenden wir die Zeit, zu der die Benachrichtigungs Plattform die Benachrichtigung erhalten hat (Dies sollte sofort der Fall sein).


## <a name="related-topics"></a>Zugehörige Themen

- [Lokalen Toast senden](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)