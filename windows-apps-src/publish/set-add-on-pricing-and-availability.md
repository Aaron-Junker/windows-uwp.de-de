---
Description: Beim Übermitteln eines Add-Ons bestimmen die Optionen auf der Seite „Preise und Verfügbarkeit“, zu welchem Preis und wie das Add-On Kunden angeboten werden soll.
title: Festlegen der Preise und Verfügbarkeit von Add-Ons
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-Ons, IAP, Preis
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597805"
---
# <a name="set-add-on-pricing-and-availability"></a>Festlegen der Preise und Verfügbarkeit von Add-Ons

Beim Übermitteln eines Add-Ons im [Partner Center](https://partner.microsoft.com/dashboard), der Optionen auf der **Preise und Verfügbarkeit** Seite, wie sehr zur Belastung der Kunden für das Add-on und wie es für Kunden angeboten werden soll.

## <a name="markets"></a>Märkte

Ihr Add-On wird standardmäßig in allen möglichen Märkten, einschließlich zukünftigen Märkten, die möglicherweise später hinzukommen, zum Grundpreis eingetragen.

Allerdings können Sie genauso wie bei einer App bestimmen, in welchen Märkten Ihr Add-On angeboten werden soll. In den meisten Fällen werden Sie dieselben Märkte wie für die App auswählen, haben aber die Flexibilität, nach Bedarf Änderungen vorzunehmen. 

Weitere Informationen und eine vollständige Liste der verfügbaren Märkte finden Sie unter [Festlegen der Märkte](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Sichtbarkeit

Sie können festlegen, ob Ihr Add-On Kunden zum Kauf angeboten werden soll. 

Die Standardoption ist **Can be displayed in the parent product’s Store listing**. Lassen Sie diese Option für Add-Ons aktiviert, die für alle Kunden verfügbar gemacht werden. 

Wählen Sie für Add-Ons, die Sie nicht allgemein verfügbar machen möchten, **Hidden in the Store** und eine der folgenden Optionen aus:

-   **Zum Kauf von innerhalb des übergeordneten Produkts nur zur Verfügung**: Bei Auswahl dieser Option kann Kunden innerhalb Ihrer app das Add-on kaufen, aber das Add-on werden nicht angezeigt, in Ihrer app-Store-Auflistung oder den Store erkannt werden können. Verwenden Sie diese Option nur, wenn das Angebot nicht allgemein verfügbar ist, z. B. im Anfangszeitraum interner Tests.
-   **Beenden Sie die Übernahme: Jeder Kunde mit einem direkten Link sehen des Produkts Store auflisten, aber sie können nur heruntergeladen, wenn sie im Besitz des Produkts vor, oder haben einen Angebotscode und Windows 10-Gerät verwenden. Dieses Add-on wird nicht angezeigt, in der übergeordneten Produkt Auflistung**: Diese Option bedeutet, dass das Add-On nicht im App-Eintrag angezeigt wird und nicht von neuen Kunden erworben werden kann. Allerdings **diese Option ist nicht für Kunden, die für Windows 8.1 oder früher unterstützt**. Wenn Ihre zuvor veröffentlichte app auf Windows 8.1 oder früher ist, werden das Add-on weiterhin zum Kauf an die Kunden zur Verfügung. Um zu beenden, bietet das Add-on für Kunden, die für Windows 8.1 oder früher, müssen Sie zum Aktualisieren Ihrer app, um den Code zu entfernen, der das Add-on bietet, und klicken Sie dann eine neue Eingabe für die app veröffentlichen. Dies wird empfohlen, auch wenn Ihre app als Ziel Windows 8.1 oder früher nicht. Es ist eine bessere benutzererfahrung für Ihre Kunden, wenn Sie noch nie ein Add-on, die Sie sich entschieden haben bieten, um nicht zur Verfügung steht.
    
 > [!NOTE] 
 > Das Auswählen der Option **Beenden des Erwerbs** und/oder das Übermitteln eines App-Updates, durch das das Add-On aus dem Code der App entfernt wird, wirkt sich nicht auf Kunden aus, die das Add-On bereits erworben haben, und zwar unabhängig von ihrem Betriebssystem.


## <a name="schedule"></a>Zeitplan

Standardmäßig (es sei denn, Sie haben eine der Optionen für **Hidden in the Store** im Abschnitt **Sichtbarkeit** ausgewählt) wird Ihre Add-On für Kunden zur Verfügung gestellt, sobald sie die Zertifizierung bestanden und den Veröffentlichungsprozess abgeschlossen hat. Um ein anderes Datum auszuwählen, wählen Sie **Optionen anzeigen**, um diesen Abschnitt zu erweitern. 

Weitere Informationen finden Sie unter [Konfigurieren des genauen Veröffentlichungszeitplans](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preise

Müssen Sie einem Grundpreis für das Add-on auswählen (es sei denn, die Sie ausgewählt haben die **beenden Übernahme** option die **Sichtbarkeit** Abschnitt). Die Standardauswahl ist **Free**, sodass, wenn Sie Geld für das Add-on berechnen, achten Sie darauf, dass Sie eine der Preis verfügbare Tarife (Beginn um.99 US-Dollar) auswählen möchten.

Sie können auch Preisänderungen planen, um das Datum und die Uhrzeit anzugeben, an dem bzw. zu der sich der Preis Ihrer Add-On ändern soll. Darüber hinaus haben Sie die Möglichkeit, diese Änderungen für bestimmte Märkte anzupassen. 

> [!TIP]
> Für die Abonnement-Add-Ons kann nicht nach dem Veröffentlichen des Add-Ons, entweder durch Auswählen von einem höheren Grundpreis in einer späteren Übermittlung oder durch Planen von preisänderungen, die der Preis steigt den Preis ausgelöst. Sie können einen niedrigeren Preis – mit einer der folgenden Methoden auswählen, aber nach der Preis gesenkt werden, ist nicht möglich, die höher als der neue Preis auslösen. Aus diesem Grund ist es besonders wichtig, um sicherzustellen, dass Sie den entsprechenden Tarif für die Abonnement-Add-Ons auswählen. 

Weitere Informationen finden Sie unter [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Sonderangebotsverkaufspreise

Wenn Sie Ihr Add-On zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen. Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](put-apps-and-add-ons-on-sale.md).



