---
description: Beim Übermitteln eines Add-Ons bestimmen die Optionen auf der Seite „Preise und Verfügbarkeit“, zu welchem Preis und wie das Add-On Kunden angeboten werden soll.
title: Festlegen der Preise und Verfügbarkeit von Add-Ons
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-ons, IAP, Preis
ms.localizationpriority: medium
ms.openlocfilehash: 5aa63307a232eed0e3fdb2e6d1355665d964212b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029959"
---
# <a name="set-add-on-pricing-and-availability"></a>Festlegen der Preise und Verfügbarkeit von Add-Ons

Wenn Sie ein Add-on im [Partner Center](https://partner.microsoft.com/dashboard)einreichen, legen die Optionen auf der Seite **Preise und Verfügbarkeit** fest, wie viel Kunden für Ihr Add-on abgerechnet werden und wie es für Kunden angeboten werden soll.

## <a name="markets"></a>Märkte

Ihr Add-On wird standardmäßig in allen möglichen Märkten, einschließlich zukünftigen Märkten, die möglicherweise später hinzukommen, zum Grundpreis eingetragen.

Allerdings können Sie genauso wie bei einer App bestimmen, in welchen Märkten Ihr Add-On angeboten werden soll. In den meisten Fällen werden Sie dieselben Märkte wie für die App auswählen, haben aber die Flexibilität, nach Bedarf Änderungen vorzunehmen. 

Weitere Informationen und eine vollständige Liste der verfügbaren Märkte finden Sie unter [Definieren der Markt Auswahl](./define-market-selection.md).

## <a name="visibility"></a>Sichtbarkeit

Sie können festlegen, ob Ihr Add-On Kunden zum Kauf angeboten werden soll. 

Die Standardoption **kann in der Store-Liste des übergeordneten Produkts angezeigt werden** . Lassen Sie diese Option für Add-Ons aktiviert, die für alle Kunden verfügbar gemacht werden. 

Wählen Sie für Add-ons, die Sie nicht allgemein verfügbar machen möchten, **im Store ausgeblendet** und eine der folgenden Optionen aus:

-   **Nur für den Kauf über das übergeordnete Produkt verfügbar: Wenn** Sie diese Option auswählen, können alle Kunden das Add-on innerhalb Ihrer APP erwerben, aber das Add-on wird nicht in der Store-Auflistung Ihrer App angezeigt, oder Sie kann im Store nicht gefunden werden. Verwenden Sie diese Option nur, wenn das Angebot nicht allgemein verfügbar ist, z. B. im Anfangszeitraum interner Tests.
-   **Übernahme aufheben: jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts sehen, Sie kann ihn jedoch nur herunterladen, wenn er sich zuvor im Besitz des Produkts befand, oder einen Werbe Code hat und ein Windows 10-Gerät verwendet. Dieses Add-on wird nicht in der Liste der übergeordneten Produkte angezeigt** : Wenn Sie diese Option auswählen, wird das Add-on nicht in der Liste Ihrer Apps angezeigt, und es können keine neuen Kunden das Add-on erwerben. Allerdings wird **diese Option für Kunden mit Windows 8.1 oder einer früheren Version nicht unterstützt** . Wenn Ihre bereits veröffentlichte App auf Windows 8.1 oder früher verfügbar ist, ist das Add-on weiterhin für den Erwerb an diese Kunden verfügbar. Um das Add-On-Angebot für Kunden mit Windows 8.1 oder einer früheren Version einzustellen, müssen Sie die App aktualisieren, indem Sie den Code entfernen, durch den das Add-On angeboten wird, und eine neue Übermittlung für die App veröffentlichen. Dieser Schritt wird selbst dann empfohlen, wenn Ihre App keine Unterstützung für Windows 8.1 oder frühere Versionen bietet, da die Kundenerfahrung beeinträchtigt werden könnte, wenn Sie erst ein Add-On anbieten, das Sie später zurückziehen.
    
 > [!NOTE] 
 > Wenn Sie die Option zum **Beenden der Übernahme** auswählen und/oder ein App-Update übermitteln, das das Add-on aus Ihrer APP entfernt, wird verhindert, dass Kunden das Add-on verwenden, wenn Sie es bereits erworben haben. Vorhandene Abonnements können nicht erneuert werden und anschließend abgebrochen werden, nachdem der aktuelle Begriff beendet wurde.


## <a name="schedule"></a>Zeitplan

Standardmäßig (es sei denn, Sie haben eine der ausgeblendeten Optionen in **den Speicher** Optionen im **Sichtbarkeits** Abschnitt ausgewählt), ist das Add-on für Kunden verfügbar, sobald es die Zertifizierung übergibt und den Veröffentlichungsprozess abschließt. Um andere Datumsangaben auszuwählen, wählen Sie **Optionen anzeigen** aus, um diesen Abschnitt zu erweitern. 

Weitere Informationen finden Sie unter [Konfigurieren der genauen Releaseplanung](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preise

Sie müssen einen Basispreis für das Add-on auswählen (es sei denn, Sie haben im Bereich " **Sichtbarkeit** " die Option zum **Abbrechen der Übernahme** ausgewählt). Die Standardauswahl ist **kostenlos** . Wenn Sie also Geld für das Add-on berechnen möchten, stellen Sie sicher, dass Sie einen der verfügbaren Preisstufen (beginnend bei. 99 USD) auswählen.

Sie können auch Preisänderungen planen, um das Datum und die Uhrzeit anzugeben, zu denen sich der Preis des Add-ons ändern sollte. Außerdem haben Sie die Möglichkeit, diese Änderungen für bestimmte Märkte anzupassen. 

> [!TIP]
> Bei Add-ons für Abonnements können Sie den Preis nach der Veröffentlichung des Add-ons nicht erhöhen, indem Sie entweder einen höheren Basispreis in einer späteren Übermittlung auswählen oder eine Preisänderung planen, die den Preis erhöht. Sie können einen niedrigeren Preis mithilfe einer dieser Methoden auswählen, aber sobald der Preis gesenkt wird, können Sie ihn nicht mehr als diesen neuen Preis erhöhen. Aus diesem Grund ist es besonders wichtig, sicherzustellen, dass Sie die richtige Preisstufe für Abonnement-Add-ons auswählen. 

Weitere Informationen finden Sie unter [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Sonderangebotsverkaufspreise

Wenn Sie Ihr Add-On zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen. Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](put-apps-and-add-ons-on-sale.md).
