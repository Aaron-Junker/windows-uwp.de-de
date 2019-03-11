---
Description: Erfahren Sie, wie universelle verwerfen, die auf Ihre Popupbenachrichtigungen verwenden.
title: Universelles Schließen
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, Info-Center in der Cloud, universelles Schließen, Benachrichtigung, geräteübergreifend, einmal Schließen, überall Schließen
ms.localizationpriority: medium
ms.openlocfilehash: 0dc87e8856e35d60660c2643b70b820b2857b488
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605095"
---
# <a name="universal-dismiss"></a>Universelles Schließen

Universelles Schließen wird vom Info-Center in der Cloud unterstützt und bedeutet, dass Sie beim Schließen einer Benachrichtigung auf einem Gerät die gleiche Benachrichtigung auf allen anderen Geräten ebenfalls Schließen.

> [!IMPORTANT]
> **Erfordert Anniversary Update**: Sie müssen SDK 14393 ausgerichtet und Build 14393 oder höher, um universelle schließen verwenden ausgeführt werden.

Ein gängiges Beispiel für ein solches Szenario sind Kalendererinnerungen... wenn Sie eine Kalender-App auf beiden Geräten haben... erhalten Sie ebenfalls eine Erinnerung auf Ihrem Handy und PC... klicken Sie auf dem Desktop auf „Schließen”... Dank des universellen Schließens, wird ebenfalls die Erinnerung auf Ihrem Handy verworfen! **Aktivieren des Universal schließen erfordert nur eine Codezeile erforderlich!**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

In diesem Szenario ist das Schlüsselelement, das **die gleiche App auf mehreren Geräten installiert ist**, d. h., das **jedes Gerät bereits Benachrichtigungen empfängt**. Eine Kalender-App ist ein Paradebeispiel, da Sie in der Regel die gleichen Kalender-App auf Ihrem Windows-PC und Ihrem Handy installiert haben, und jede Instanz der App bereits Erinnerungen auf beide Geräte sendet. Durch das Hinzufügen von Unterstützung für das universellen Schließen können diese Instanzen für die gleichen Erinnerungen geräteübergreifend verknüpft werden.


## <a name="how-to-enable-universal-dismiss"></a>So aktivieren Sie das universelle Schließen

Als Entwickler ist das Aktivieren des universellen Schließens ganz einfach. Sie müssen lediglich eine ID bereitstellen, die uns ermöglicht, jede Benachrichtigung geräteübergreifend zu verknüpfen. So wird die entsprechende verknüpfte Benachrichtigung auf einem Gerät geschlossen, wenn der Benutzer die Benachrichtigung auf einem anderen Gerät schließt.

![RemoteID-Diagramm für das universelle Schließen](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: Ein Bezeichner, der eine Benachrichtigung eindeutig identifiziert *geräteübergreifend*.

Es muss nur eine Zeile Code für RemoteId hinzugefügt werden, was die Unterstützung für das universelle Schließen unterstützt. Wie Sie Ihre RemoteId generieren, ist Ihre Entscheidung – Sie müssen allerdings sicherstellen, dass sie die Benachrichtigung geräteübergreifend eindeutig identifiziert, und die gleiche ID von verschiedenen Instanzen Ihrer App generiert werden kann, die auf verschiedenen Geräten läuft.

Beispielsweise generiere ich in meiner App für den Hausaufgabenplaner meine RemoteId so, dass Sie vom Typ "Erinnerung" ist. Ich füge anschließend die Online-Konto-ID und die Online-ID des Elements Hausaufgaben hinzu. Ich kann konsistent die exakt gleiche RemoteId konsistent generieren, unabhängig davon, welches Gerät die Benachrichtigung sendet, da diese online-IDs geräteübergreifend freigegeben werden.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

Der folgende Code wird auf meinem Handy und meiner Desktop-App ausgeführt, d. h., dass die Benachrichtigung auf beiden Geräten die gleiche RemoteId hat.

Dies ist alles! Wenn der Benutzer eine Benachrichtigung schließt (oder darauf klickt), wird geprüft, ob es über eine RemoteId verfügt, und wenn dies der Fall ist, fächern wir das Schließen von dieser RemoteId auf allen Geräten des Benutzers aus.

**Bekanntes Problem**: Abrufen der **RemoteId** über die `ToastNotificationHistory.GetHistory()` -API gibt stets leere Zeichenfolge anstelle der **RemoteId** Sie angegeben haben. Keine Sorge, alles funktioniert – es wird nur der Wert abgerufen, der beschädigt ist.

> [!NOTE]
> Wenn der Benutzer oder das Unternehmen die [Benachrichtigungsspiegelung](notification-mirroring.md) für Ihre App deaktiviert (oder die Benachrichtigungsspiegelung vollständig deaktiviert), funktioniert das universelle Schließen nicht, da Ihre Benachrichtigungen nicht in der Cloud vorhanden sind.


## <a name="supported-devices"></a>Unterstützte Geräte

Seit dem Anniversary Update wird das universelle Schließen auf Windows Mobile und Windows Desktop unterstützt. Universelles Schließen funktioniert in beide Richtungen, zwischen PCs, PC und Handy oder Handy und Handy.