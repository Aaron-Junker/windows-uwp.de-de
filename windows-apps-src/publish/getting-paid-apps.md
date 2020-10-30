---
description: Erfahren Sie mehr über das Empfangen von Zahlungen für Ihre apps, Add-ons (in-App-Produkte) und Werbeeinnahmen.
title: Erhalten der Bezahlung
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: Windows 10, UWP, Zahlungen, App-Verkäufe, App-Verkäufe, Auszahlung, geschäftsgebühr, Auszahlungs Aufbewahrung, Prozentsatz
ms.localizationpriority: medium
ms.openlocfilehash: 7138a30abb3e2de46c94de96614cc0e404ede675
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029743"
---
# <a name="getting-paid"></a>Erhalten der Bezahlung
Hier sind einige wichtige Informationen zum Empfangen von Zahlungen für Ihre apps, Add-ons und Werbeeinnahmen.

> [!IMPORTANT]
> Bevor Sie im Microsoft Store Geld von App-Verkäufen erhalten können, müssen Sie [Ihr Auszahlungs Konto einrichten und die erforderlichen Steuerformulare ausfüllen](setting-up-your-payout-account-and-tax-forms.md).

> [!NOTE]
> Wenn Sie Unterstützung in Bezug auf Auszahlungen benötigen, einschließlich der Konfiguration von Auszahlungskonten, fehlenden Auszahlungen, dem Zurückhalten von Auszahlungen oder sonstigen Fragen, wenden Sie sich [hier](https://developer.microsoft.com/windows/support) an den Support.

## <a name="store-fee"></a>Store-Gebühr

Wenn Sie sich [für ein Entwicklerkonto registrieren](https://developer.microsoft.com/store/register), akzeptieren Sie die [Vereinbarung für App-Entwickler](/legal/windows/agreements/app-developer-agreement). In diesem Vertrag wird die Beziehung zwischen Ihnen und Microsoft erläutert, die sich auf das verkaufen von apps im Microsoft Store bezieht, einschließlich der Kosten für den Kauf, den Microsoft für jeden Verkauf berechnet.

Die Gebühren sind in der [Vereinbarung für App-Entwickler](/legal/windows/agreements/app-developer-agreement) offiziell definiert. Schauen Sie sich stets das Dokument an, wenn Fragen auftreten.

Die Speicher Gebühr wird auf alle vom Microsoft Store erfassten App-Verkäufe, einschließlich Add-ons, angewendet.


## <a name="price-tiers"></a>Preisstufen

Die von Ihnen ausgewählten Preisstufen legt den [Verkaufspreis](set-and-schedule-app-pricing.md#base-price) in allen Ländern fest, in denen Sie Ihre APP verteilen möchten. Sie können auch zusätzliche Preis Merkmale nutzen, wie z. b. das  [Auswählen verschiedener Preise für verschiedene Märkte](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) oder [das Platzieren Ihrer APP](put-apps-and-add-ons-on-sale.md).

Sie können die App kostenlos anbieten oder einen Preis auswählen, den Kunden zahlen müssen, um die App zu erwerben. Die Preisstufen beginnen bei 0,99 USD und erhöhen sich dann in weiteren Schritten (1,09 USD, 1,19 USD usw.). Die Schrittweite zwischen den Preisstufen erhöht sich bei zunehmendem Preis.

> [!NOTE] 
> Diese Preisstufen gelten auch für alle Add-ons, die Sie in Ihrer APP anbieten.

Jede Preisstufe hat in allen Währungen, die vom Store angeboten werden, einen entsprechenden Wert. Diese Werte sollten Ihnen helfen, Ihre Apps weltweit zu vergleichbaren Preisen zu verkaufen. Aufgrund von Wechselkursänderungen kann der genaue Umsatz jedoch geringfügig von einer Währung zur anderen abweichen. Die Wechselkurse werden monatlich berechnet. Je nachdem, wann die Transaktion stattfindet, wird der entsprechende Wechselkurs angewendet. Der Austausch Kurs und der Datumsbereich, für den er erzwungen wurde, werden im Auszahlungsbericht in den Spalten ExchangeRate bzw. exchangeratedate angegeben.

Sie haben auch die Möglichkeit, einen Freiform-Preis Ihrer Wahl in der lokalen Währung eines bestimmten Markts einzugeben. Wenn Sie dies tun, wird der Preis nicht angepasst (auch nicht bei Wechselkursänderungen). Dazu müssen Sie ein Update mit einem neuen Preis übermitteln. 

Berücksichtigen Sie, dass der von Ihnen ausgewählte Preis möglicherweise Verkaufs- oder Mehrwertsteuer enthält, die Ihre Kunden bezahlen müssen. Weitere Informationen finden Sie unter [Steuerinformationen zu kostenpflichtigen Apps](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Zahlungsberichte

Sie können auf Details zu ihren Zahlungsinformationen zugreifen und in der **Zahlungsübersicht** im [Partner Center](https://partner.microsoft.com/dashboard) Berichte herunterladen. Weitere Einzelheiten zu den hier gezeigten Informationen und zur Kategorisierung des eingenommenen Betrags finden Sie unter [Zahlungsübersicht](payout-summary.md).


## <a name="payout-timeframe"></a>Zahlungszeitrahmen

Die Zahlungen erfolgen monatlich (sofern der entsprechende Zahlungsschwellenwert erreicht wurde und Sie Ihre Auszahlung nicht, wie unten beschrieben, zurückgehalten haben). In der Regel senden wir alle in einem Monat fälligen Zahlungen am 15. des Monats. Beachten Sie, dass es in der Regel weitere 3 bis 10 Werktage dauert, bis die Zahlung auf Ihrem Auszahlungskonto eingeht. Weitere Informationen finden Sie unter [Zahlungsschwellenwerte, -methoden und -zeitrahmen](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Aufbewahrungsstatus für Auszahlungen

Standardmäßig senden wir Zahlungen monatlich (wie oben beschrieben). Allerdings haben Sie die Möglichkeit, Ihre Auszahlungen für ein Programm zurückzuhalten, sodass keine Zahlungen an Ihr Konto gesendet werden. Wenn Sie sich dafür entscheiden, Ihre Zahlungen anzuhalten, zeichnen wir weiterhin alle Einnahmen auf, die Sie verdienen, und geben die Details in Ihrer **Auszahlungs Zusammenfassung** an. Allerdings werden wir keine Zahlungen an Ihr Konto senden, bis Sie die Haltesperre aufheben.

Um Ihre Zahlungen zurückzuhalten, wechseln Sie zu **Entwicklereinstellungen** . Suchen Sie unter **Auszahlung und Steuern** im Abschnitt **Auszahlung und Steuerprofilzuweisung** das Programm, für das Zahlungen zurückgehalten werden sollen. Aktivieren Sie das Kontrollkästchen **Meine Zahlung zurückhalten** , um die Zahlungen für dieses Programm zurückzuhalten. Sie können den Aufbewahrungsstatus für Auszahlungen jederzeit ändern, aber beachten Sie, dass sich Ihre Entscheidung auf die nächste monatliche Auszahlung auswirkt. Wenn Sie z. b. die Auszahlung von April halten möchten, stellen Sie sicher, dass der Status der Auszahlungs **Aufbewahrung vor Ende März auf ein** festgelegt ist.

Sobald Sie den Aufbewahrungsstatus für Zahlungen auf **Ein** festgelegt haben, werden alle Auszahlungen für dieses Programm zurückgehalten, bis Sie den Schieberegler wieder auf **Aus** stellen. In diesem Fall sind Sie im nächsten monatlichen Auszahlungszyklus eingeschlossen (vorausgesetzt, alle geltenden Zahlungsschwellenwerte sind erfüllt). Wenn Sie z. b. Ihre Zahlungen angenommen haben, aber im Juni eine Auszahlung generieren möchten, müssen Sie sicherstellen, dass der Status der Auszahlungs Aufbewahrung vor dem Ende des Mai auf **Off** festzulegen.

> [!NOTE]
> Der **Aufbewahrungsstatus für Zahlungen** gilt individuell für einzelne Programme (Microsoft Store, Werbung, Azure Marketplace usw.). Wenn Sie die Zahlungen für alle Ihre Programme zurückhalten möchten, müssen Sie den Aufbewahrungsstatus für jedes einzelne Programm aktivieren.


 

 
