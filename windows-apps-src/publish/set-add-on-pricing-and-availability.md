---
Description: Beim Übermitteln eines Add-Ons bestimmen die Optionen auf der Seite „Preise und Verfügbarkeit“, zu welchem Preis und wie das Add-On Kunden angeboten werden soll.
title: Festlegen der Preise und Verfügbarkeit von Add-Ons
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-Ons, IAP, Preis
ms.localizationpriority: medium
ms.openlocfilehash: 803164c395602313bcb84331e30376efd6832731
ms.sourcegitcommit: 912146681b1befc43e6db6e06d1e3317e5987592
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79295723"
---
# <a name="set-add-on-pricing-and-availability"></a>Festlegen der Preise und Verfügbarkeit von Add-Ons

Wenn Sie ein Add-on im [Partner Center](https://partner.microsoft.com/dashboard)einreichen, legen die Optionen auf der Seite **Preise und Verfügbarkeit** fest, wie viel Kunden für Ihr Add-on abgerechnet werden und wie es für Kunden angeboten werden soll.

## <a name="markets"></a>Märkte

Ihr Add-On wird standardmäßig in allen möglichen Märkten, einschließlich zukünftigen Märkten, die möglicherweise später hinzukommen, zum Grundpreis eingetragen.

Allerdings können Sie genauso wie bei einer App bestimmen, in welchen Märkten Ihr Add-On angeboten werden soll. In den meisten Fällen werden Sie dieselben Märkte wie für die App auswählen, haben aber die Flexibilität, nach Bedarf Änderungen vorzunehmen. 

Weitere Informationen und eine vollständige Liste der verfügbaren Märkte finden Sie unter [Festlegen der Märkte](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Sichtbarkeit

Sie können festlegen, ob Ihr Add-On Kunden zum Kauf angeboten werden soll. 

Die Standardoption ist **Can be displayed in the parent product’s Store listing**. Lassen Sie diese Option für Add-Ons aktiviert, die für alle Kunden verfügbar gemacht werden. 

Wählen Sie für Add-Ons, die Sie nicht allgemein verfügbar machen möchten, **Hidden in the Store** und eine der folgenden Optionen aus:

-   **Nur für den Kauf über das übergeordnete Produkt verfügbar: Wenn**Sie diese Option auswählen, können alle Kunden das Add-on innerhalb Ihrer APP erwerben, aber das Add-on wird nicht in der Store-Auflistung Ihrer App angezeigt, oder Sie kann im Store nicht gefunden werden. Verwenden Sie diese Option nur, wenn das Angebot nicht allgemein verfügbar ist, z. B. im Anfangszeitraum interner Tests.
-   **Beenden des Erwerbs: Alle Kunden mit einem direkten Link können den Produkt-Store-Eintrag sehen, aber sie können ihn nur herunterladen, wenn sie das Produkts vorher erworben haben oder einen Werbecode und ein Windows 10-Gerät verwenden. Dieses Add-On wird nicht im übergeordneten Produkteintrag angezeigt**: Wenn Sie diese Option auswählen, wird das Add-On nicht im App Eintrag angezeigt, und neuen Kunden können das Add-On nicht erwerben. **Diese Option wird jedoch für Kunden unter Windows 8.1 oder früher nicht unterstützt**. Wenn Ihre bereits veröffentlichte App auf Windows 8.1 oder früher verfügbar ist, ist das Add-on weiterhin für den Erwerb an diese Kunden verfügbar. Sie müssen Ihre APP aktualisieren, um den Code zu entfernen, der das Add-on anbietet, und dann eine neue Übermittlung für die APP veröffentlichen, um das Add-on für Kunden auf Windows 8.1 oder früher nicht mehr anbieten zu können. Dies wird auch dann empfohlen, wenn Ihre APP Windows 8.1 oder früher nicht als Zielversion gilt. Es ist eine bessere Erfahrung für Ihre Kunden, wenn Ihnen nie ein Add-on angeboten wird, das Sie für die Nichtverfügbarkeit ausgewählt haben.
    
 > [!NOTE] 
 > Wenn Sie die Option zum **Beenden der Übernahme** auswählen und/oder ein App-Update übermitteln, das das Add-on aus Ihrer APP entfernt, wird verhindert, dass Kunden das Add-on verwenden, wenn Sie es bereits erworben haben. Vorhandene Abonnements können nicht erneuert werden und anschließend abgebrochen werden, nachdem der aktuelle Begriff beendet wurde.


## <a name="schedule"></a>Zeitplan

Standardmäßig (es sei denn, Sie haben eine der Optionen für **Hidden in the Store** im Abschnitt **Sichtbarkeit** ausgewählt) wird Ihre Add-On für Kunden zur Verfügung gestellt, sobald sie die Zertifizierung bestanden und den Veröffentlichungsprozess abgeschlossen hat. Um ein anderes Datum auszuwählen, wählen Sie **Optionen anzeigen**, um diesen Abschnitt zu erweitern. 

Weitere Informationen finden Sie unter [Konfigurieren des genauen Veröffentlichungszeitplans](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preise

Sie müssen einen Basispreis für das Add-on auswählen (es sei denn, Sie haben im Bereich " **Sichtbarkeit** " die Option zum **Abbrechen der Übernahme** ausgewählt). Die Standardauswahl ist **kostenlos**. Wenn Sie also Geld für das Add-on berechnen möchten, stellen Sie sicher, dass Sie einen der verfügbaren Preisstufen (beginnend bei. 99 USD) auswählen.

Sie können auch Preisänderungen planen, um das Datum und die Uhrzeit anzugeben, an dem bzw. zu der sich der Preis Ihrer Add-On ändern soll. Darüber hinaus haben Sie die Möglichkeit, diese Änderungen für bestimmte Märkte anzupassen. 

> [!TIP]
> Bei Add-ons für Abonnements können Sie den Preis nach der Veröffentlichung des Add-ons nicht erhöhen, indem Sie entweder einen höheren Basispreis in einer späteren Übermittlung auswählen oder eine Preisänderung planen, die den Preis erhöht. Sie können einen niedrigeren Preis mithilfe einer dieser Methoden auswählen, aber sobald der Preis gesenkt wird, können Sie ihn nicht mehr als diesen neuen Preis erhöhen. Aus diesem Grund ist es besonders wichtig, sicherzustellen, dass Sie die richtige Preisstufe für Abonnement-Add-ons auswählen. 

Weitere Informationen finden Sie unter [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Sonderangebotsverkaufspreise

Wenn Sie Ihr Add-On zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen. Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](put-apps-and-add-ons-on-sale.md).



