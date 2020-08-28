---
title: Universelles Schließen
description: Erfahren Sie, wie Sie mithilfe von Universal verwerfen eine Popup Benachrichtigung von einem Gerät verwerfen und auf anderen Geräten dieselbe Benachrichtigung verworfen wird.
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, Toast, Aktions Center in der Cloud, Universal verwerfen, Benachrichtigung, Geräte übergreifend, verwerfen, sobald Sie alle schließen
ms.localizationpriority: medium
ms.openlocfilehash: fff9315b9a5645d8a981ca899c1c37acb04de07c
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043452"
---
# <a name="universal-dismiss"></a>Universelles Schließen

Universelle verwerfen, die durch das Aktions Center in der Cloud unter Einsatz kommt, bedeutet, dass beim Verwerfen einer Benachrichtigung von einem Gerät die gleiche Benachrichtigung auch auf Ihren anderen Geräten verworfen wird.

> [!IMPORTANT]
> **Erfordert ein Anniversary Update**: Sie müssen das SDK 14393 als Ziel verwenden und Build 14393 oder höher ausführen, um Universal verwerfen zu verwenden.

Das allgemeine Beispiel für dieses Szenario sind Kalender Erinnerungen... Sie haben eine Kalender-APP auf beiden Geräten... Sie erhalten eine Erinnerung auf Ihr Telefon und Ihren Desktop... Klicken Sie auf dem Desktop auf verwerfen... Dank Universal verwerfen wird die Erinnerung auf Ihrem Telefon ebenfalls verworfen! **Zum Aktivieren von Universal verwerfen ist nur eine Codezeile erforderlich.**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

In diesem Szenario besteht der wichtigste Fakt darin, dass **dieselbe App auf mehreren Geräten installiert wird**, was bedeutet, dass **jedes Gerät bereits Benachrichtigungen empfängt**. Eine Kalender-APP ist das legendäre Beispiel, da Sie in der Regel die gleiche Kalender-APP auf Ihrem Windows-PC und Ihrem Telefon installiert haben und jede Instanz der APP Ihnen bereits Erinnerungen an jedes Gerät sendet. Durch Hinzufügen der Unterstützung für Universal verwerfen können diese Instanzen der gleichen Erinnerungen Geräte übergreifend verknüpft werden.


## <a name="how-to-enable-universal-dismiss"></a>Aktivieren von Universal verwerfen

Als Entwickler ist das Aktivieren von Universal verwerfen äußerst einfach. Sie müssen lediglich eine ID angeben, die es uns ermöglicht, jede Benachrichtigung Geräte übergreifend zu verknüpfen, damit die entsprechende verknüpfte Benachrichtigung vom anderen Gerät verworfen wird, wenn der Benutzer eine Benachrichtigung von einem Gerät abschließt.

![Universelles RemoteID-Diagramm verwerfen](images/universal-dismiss-remoteid.jpg)

> **RemoteID**: ein Bezeichner, der eine *Geräte übergreifende*Benachrichtigung eindeutig identifiziert.

t erfordert nur eine Codezeile, um RemoteID hinzuzufügen, und ermöglicht so die Unterstützung für Universal verwerfen! Wie Sie Ihre RemoteID generieren, müssen Sie jedoch sicherstellen, dass Sie Ihre Benachrichtigung Geräte übergreifend eindeutig identifiziert, und dass derselbe Bezeichner von verschiedenen Instanzen Ihrer APP generiert werden kann, die auf unterschiedlichen Geräten ausgeführt werden.

Beispielsweise generiere ich in meiner Aufgabenplaner-App meine RemoteID, indem ich sage, dass Sie den Typ "Erinnerung" hat, und dann füge ich die Onlinekonto-ID und die Online-ID des Eingabeelements ein. Ich kann immer genau dieselbe RemoteID generieren, unabhängig davon, welches Gerät die Benachrichtigung sendet, da diese Online-IDs auf den Geräten gemeinsam genutzt werden.

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

Der folgende Code wird sowohl in der Telefon-als auch in der Desktop-app ausgeführt, was bedeutet, dass die Benachrichtigung auf beiden Geräten dieselbe RemoteID hat.

Das ist alles, was Sie tun müssen! Wenn der Benutzer eine Benachrichtigung ablehnt (oder darauf klickt), überprüfen wir, ob er eine RemoteID aufweist. wenn dies der Fall ist, wird die RemoteID auf allen Geräten des Benutzers geschlossen.

**Bekanntes Problem**: das Abrufen von **RemoteID** über die `ToastNotificationHistory.GetHistory()` API gibt immer eine leere Zeichenfolge anstelle der von Ihnen angegebenen **RemoteID** zurück. Keine Sorge, alles ist funktionsfähig. es wird nur der Wert abgerufen, der beschädigt ist.

> [!NOTE]
> Wenn der Benutzer oder das Unternehmen die [Benachrichtigungs Spiegelung](notification-mirroring.md) für Ihre APP deaktiviert (oder die Benachrichtigungs Spiegelung vollständig deaktiviert), funktioniert das universelle verwerfen nicht, da wir nicht über Ihre Benachrichtigungen in der Cloud verfügen.


## <a name="supported-devices"></a>Unterstützte Geräte

Seit dem Anniversary Update wird Universal verwerfen auf Windows Mobile und Windows Desktop unterstützt. Universal verwerfen funktioniert beide Richtungen, zwischen PC-PC, PC-Telefon und Telefon Telefon.