---
Description: Learn about receiving payments for your apps, add-ons (in-app products), and advertising earnings.
title: Regelung der Bezahlung
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 03/05/2019
ms.topic: article
keywords: Windows 10, UWP, Bezahlung, App-Verkäufe, App-Erlöse, Auszahlung, Store-Gebühr, Auszahlungssperre, Prozentsatz
ms.localizationpriority: medium
ms.openlocfilehash: 853554a0a3a0507f1a8b9d8994618d16aa44bccc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259993"
---
# <a name="getting-paid"></a>Regelung der Bezahlung
Here’s some important info about receiving payment for your apps, add-ons, and advertising earnings.

> [!IMPORTANT]
> Before you can receive money from app sales in the Microsoft Store, you need to [set up your payout account and fill out the necessary tax forms](setting-up-your-payout-account-and-tax-forms.md).

## <a name="store-fee"></a>Store-Gebühr

Wenn Sie sich [für ein Entwicklerkonto registrieren](https://developer.microsoft.com/store/register), akzeptieren Sie die [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). In dieser Vereinbarung ist die Geschäftsbeziehung zwischen Ihnen und Microsoft in Bezug auf den Verkauf von Apps im Microsoft Store erläutert, einschließlich der Store-Gebühr, die Microsoft für jeden Verkauf erhebt.

Die Gebühren sind in der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) offiziell definiert. Lesen Sie bei Fragen immer in diesem Dokument nach.

Die Microsoft Store-Gebühr gilt für alle vom Microsoft Store erfassten App-Verkäufe einschließlich Add-Ons.


## <a name="price-tiers"></a>Preisniveaus

Das Preisniveau bestimmt den [Verkaufspreis](set-and-schedule-app-pricing.md#base-price) in allen Ländern, in denen Sie Ihre App vertreiben möchten. Sie können auch zusätzliche Preis-Features verwenden, wie z. B. [um in verschiedenen Märkten unterschiedliche Preise zu verlangen](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) oder [Ihre App zum Erwerb anbieten](put-apps-and-add-ons-on-sale.md).

Sie können die App kostenlos anbieten oder einen Preis auswählen, den Kunden zahlen müssen, um die App zu erwerben. Das Preisniveau beginnt bei 0,99 USD und steigen schrittweise (1,09 USD, 1,19 USD, usw.). Die Schritte zwischen den Preisniveaus werden mit der Höhe des Preises größer.

> [!NOTE] 
> Diese Preisniveaus gelten auch für alle Add-Ons, die Sie in der App anbieten.

Jedes Preisniveau hat einen entsprechenden Wert in jeder vom Store angebotenen Währungen. Diese Werte sollten Ihnen helfen, Ihre Apps weltweit zu vergleichbaren Preisen zu verkaufen. Aufgrund von Wechselkursschwankungen kann der genaue Verkaufsbetrag von Währung zu Währung jedoch geringfügig abweichen.

Sie haben auch die Möglichkeit, einen formfreien Preis Ihrer Wahl in der lokalen Währung für einen bestimmten Markt anzugeben. Dadurch wird der Preis nur angepasst (auch wenn sich der Umrechnungskurs ändert), wenn Sie ein Update mit einem neuen Preis übermitteln. 

Beachten Sie, dass der von Ihnen ausgewählte Preis u. U. eine Verkaufs- oder Mehrwertsteuer enthält, die Kunden bezahlen müssen. Weitere Informationen finden Sie unter [Steuerinformationen zu kostenpflichtigen Apps](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Auszahlungsberichte

You can access details about your payment info and download reports in the **Payout summary** of [Partner Center](https://partner.microsoft.com/dashboard). Weitere Informationen zu den aufgeführten Details und zu den Kategorien, in die wir Ihre Erlöse einteilen, finden Sie unter [Auszahlungszusammenfassung](payout-summary.md).


## <a name="payout-timeframe"></a>Auszahlungszeitraum

Zahlungen erfolgen monatlich (vorausgesetzt, der entsprechende Zahlungsschwellenwert wurde erreicht, und Sie haben die Auszahlung nicht gesperrt, wie unten beschrieben). In der Regel senden wir Zahlungen, die in einem bestimmten Monat fällig sind, bis zum 15. Tag dieses Monats. Beachten Sie, dass Zahlungen in der Regel zwischen drei und zehn zusätzliche Werktage benötigen, um Ihr Konto zu erreichen. Weitere Informationen finden Sie unter [Auszahlungsschwellenwerte, Methoden und Zeiträume](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Auszahlungssperre

Standardmäßig senden wir Zahlungen monatlich, wie oben beschrieben. However, you have the option to put your payouts for a program on hold, which will prevent us from sending payments to your account. Wenn Sie Ihre Auszahlungen sperren, zeichnen wir weiterhin alle Ihre Umsätze auf und geben die Details in Ihrer **Auszahlungszusammenfassung** an. Allerdings senden wir so lange keine Zahlungen an Ihr Konto, bis Sie die Sperre entfernen.

To place your payments on hold, go to **Developer settings**. Under **Payout and tax**, in the **Payout and tax profile assignment** section, locate the program for which you would like payments held. Click the **Hold my Payment** checkbox to hold payments for this program. Sie können die Auszahlungssperre jederzeit ändern. Beachten Sie jedoch, dass sich Ihre Auswahl auf die nächste monatliche Auszahlung auswirkt. Wenn Sie beispielsweise die April-Auszahlung sperren möchten, sollten Sie die Auszahlungssperre schon vor Ende März auf **Ein** festlegen.

Once you have set your payout hold status to **On**, all payouts for this program will be on hold until you toggle the slider back to **Off**. Dann werden Sie im nächsten monatlichen Auszahlungszyklus berücksichtigt (vorausgesetzt, der entsprechende Zahlungsschwellenwert wurde erreicht). Wenn Sie zum Beispiel Ihre Auszahlungen gesperrt haben, aber eine Auszahlung im Juni generieren möchten, sollten Sie die Auszahlungssperre noch vor Ende Mai auf **Aus** setzen.

> [!NOTE]
> Your **Payout hold status** applies to each program individually (Microsoft Store, advertising, Azure Marketplace, etc.). If you wish to hold payments on all of your programs, you must hold payments on each program individually.


 

 




