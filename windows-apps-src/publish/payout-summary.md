---
Description: Die Auszahlungsübersicht enthält Details zu den mit Ihren Apps und Add-Ons erzielten Erlösen. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.
title: Auszahlungsübersicht
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: Windows 10 UWP, Auszahlungszusammenfassung, Anweisung, Zahlungen, Einnahmen, Auszahlung, Einnahmen
ms.localizationpriority: medium
ms.openlocfilehash: 777ee4201b435f17cdc4fc3650a2d33645ff56b9
ms.sourcegitcommit: 769ec7811aaaa79fe521e3e984a2e1a2a9671caf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70057821"
---
# <a name="payout-summary"></a>Auszahlungsübersicht

Die **Auszahlungs Zusammenfassung** zeigt Ihnen Details zu dem Geld, das Sie bei Microsoft erworben haben. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.

Wenn Sie Produkte im Azure Marketplace verkaufen, werden Ihnen in der **Auszahlungsübersicht** auch Informationen zu erfolgreichen Auszahlungen angezeigt. Weitere Informationen zur Bezahlung im Azure Marketplace finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Um sich für die Auszahlung zu qualifizieren, muss Ihr fort Betrag den [Zahlungs Schwellenwert](payment-thresholds-methods-and-timeframes.md) von $50 erreichen. Weitere Informationen zum Schwellenwert für die Zahlung finden Sie auf dieser Seite, und lesen Sie die App-Entwickler Vereinbarung.

## <a name="access-the-payout-summary-pages"></a>Zugreifen auf die Auszahlungs Zusammenfassungs Seiten

So öffnen Sie eine der Auszahlungs Zusammenfassungs Seiten:

1. Wählen Sie in der oberen rechten Ecke das Symbol Money aus.
2. Wählen Sie Zahlungen, Transaktionsverlauf oder Daten exportieren aus.

## <a name="payments-page"></a>Seite "Zahlungen"

Die Summen auf dieser Seite stellen alle Programme dar, an denen Sie teilnehmen. Sie können nach Teilnehmer-ID, Programm, Zahlungs-ID und Erstellungs Typ filtern. Beträge werden in US-Dollar angegeben. Der kostenpflichtige Wert wird auch Unterzahlung in Währung angezeigt.

| Bereich                   | Beschreibung                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Summe bezahlt in diesem Jahr   | Die kombinierte Summe, die Sie in diesem Jahr in US-Dollar für alle Ihre Programme bezahlt haben.       |
| Nächste geschätzte Zahlung | Die nächste nächste Zahlung an Sie (auch wenn andere in Kürze verfügbar sind) in US-Dollar. |
| Letzte Zahlung           | Der Betrag (in US-Dollar), der Programmname und das Programm Ihrer letzten Zahlung.           |
| Zahlungen nach Quelle     | Die Menge der Zahlungen in US-Dollar, die durch das Programm in den letzten 12 Monaten dargestellt wird.           |
| Zahlungen               | Wählen Sie bezahlt oder ausstehend aus, und sortieren Sie Sie nach Ihren Wünschen. Weitere Informationen zu einer bestimmten Zahlung finden Sie in der SELECT-Sicht. Wählen Sie herunterladen aus, um eine Kopie der Payment-überweisungserklärung herunterzuladen. Beachten Sie, dass es bis zu 24 Stunden dauern kann, bis die Daten für den Transaktionsverlauf angezeigt werden, sodass die zugehörigen Gewinne möglicherweise nicht sofort angezeigt werden. |

Wählen Sie zum Exportieren der Daten auf dieser Seite exportieren aus, und befolgen Sie dann die Anweisungen auf der Seite Daten exportieren.

## <a name="transaction-history-page"></a>Seite "Transaktionsverlauf"

Auf dieser Seite werden alle individuellen Einnahmen angezeigt, einschließlich des Datums, des Typs und des Erwerbs für jede. Sie können einen Zeitraum auswählen, der angezeigt werden soll. Außerdem können Sie nach Registrierungs-ID, Programm, Zahlungs-ID, erzahltyp, Hebel und Status filtern. Daten sind für das aktuelle Geschäftsjahr (1. Juli – 30. Juni) und die letzten beiden Geschäftsjahre verfügbar.

Wählen Sie den Pfeil nach unten auf der rechten Seite der Seite aus, um weitere Details zu einem Erwerbs Bereich anzuzeigen. Dadurch werden der Hebel, der Umsatz und das Produkt angezeigt. Wenn eine dieser Daten aus irgendeinem Grund nicht verfügbar ist, Sie aber darauf zugreifen müssen, wenden Sie sich an den [Support](https://developer.microsoft.com/en-us/windows/support). Wenn das Ergebnis einer Anpassung und nicht einer Transaktion entspricht, werden die Produktfelder nicht angezeigt.

Wählen Sie zum Exportieren der Transaktionsdaten auf dieser Seite die Option Exportieren aus, und befolgen Sie dann die Anweisungen auf der Seite Daten exportieren. Dateien, die aus der Seite Transaktionsverlauf exportiert wurden, zeigen Daten in Transaktionswährung, Einnahmen in Transaktionswährung und US-Dollar sowie den bezahlten Wert in Zahlung in Währung an.

## <a name="payment-status"></a>Zahlungsstatus

| Erwerbsstatus           | Grund                                                                                                                                      | Partner Aktion erforderlich?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Nicht verarbeitet              | Der Erwerb ist für die Zahlung berechtigt. Sie verbleibt in diesem Zustand für einen kühl Zeitraum, wie im Programmhandbuch für das Incentive-Programm definiert. | Nein                                                         |
| Ansteh                 | Der Zahlungsauftrag hat ausstehende interne Überprüfungen generiert, bevor die Zahlung verarbeitet wird.                                                               | Nein                                                         |
| Ausstehende Steuerrechnung      | Ihre Steuerrechnung ist unvollständig oder ungültig.                                                                                                  | Sie müssen ihre Steuerrechnung aktualisieren, bevor Sie bezahlen können. |
| Ablehnung während der Überprüfung   | Die Zahlung wurde während der Überprüfung abgelehnt.                                                                                                     | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/en-us/windows/support) .                      |
| Fehler                   | Die Zahlung ist aufgrund eines Microsoft-Systemfehlers fehlgeschlagen.                                                                                         | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/en-us/windows/support) .                      |
| In Bearbeitung              | Die Zahlung wird ausgeführt.                                                                                                                 | Nein                                                         |
| Falsche Zahlung        | Der Zahlungsvorgang wird wiederholt.                                                                                                       | Nein                                                         |
| Entsand                     | Die Zahlung wurde an Ihre Bank gesendet.                                                                                                     | Nein                                                         |
| Reprocessing             | Bei der Zahlung ist ein Microsoft-Systemfehler aufgetreten, der erneut verarbeitet wird.                                                                  | Nein                                                         |
| Reserviert                 | Die Zahlung wurde von Ihrer Bank rückgängig gemacht und wird im nächsten Zahlungszeitraum erneut gesendet.                                                     | Nein                                                         |
| Finanzrechnung abgelehnt     | Ihre Steuerrechnung wurde während der Überprüfung abgelehnt. Alle ausstehenden Zahlungen werden angehalten, bis die Überprüfung der Steuerrechnung abgeschlossen ist.                 | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/en-us/windows/support) .                      |
| Steuerrechnung unter Review | Ihre Steuerrechnung wird geprüft. Ihre Zahlung wird nach Genehmigung der Steuerrechnung freigegeben.                                   | Nein                                                         |
| Ablehnung                 | Die Zahlung wurde von Ihrer Bank abgelehnt.                                                                                                      | Weitere Informationen erhalten Sie von der Bank.                             |

## <a name="export-data-page"></a>Seite "Daten exportieren"

Befolgen Sie die Anweisungen auf dieser Seite, um die gewünschten Daten zu exportieren.

Notizen:

- Wenn Sie über die Seite Zahlungen oder Transaktionsverlauf auf diese Seite zugreifen, werden die Filter nicht durchgeführt. Sie müssen Sie auf der Seite Daten exportieren wiederholen.
- Die Seite "Daten exportieren" wird nicht eigenständig aktualisiert. Möglicherweise müssen Sie die Seite manuell aktualisieren, um die neuesten Daten anzuzeigen.
- Der Filter führt möglicherweise zu einem Fehler, der nicht verfügbar ist. Dies bedeutet wahrscheinlich, dass Sie den Standard Zeitraum in drei Monaten ausgewählt haben und dann eine Zahlungs-ID aus einem Verdienst außerhalb dieses Zeitraums ausgewählt haben. Erweitern Sie den Zeitraum, und versuchen Sie es noch mal.

## <a name="payment-download-export"></a>Zahlungs Download Export

Mit dieser Option können Sie die Zahlungen, die Sie in Ihrer Bank für ein bestimmtes Programm erhalten haben, sowie die zugehörigen Steuern und den aggregierten Erwerbs Betrag herunterladen. Dieser Bericht wird für viele Partner Center-Programme verwendet, sodass einige Spalten für den Bericht möglicherweise nicht zutreffend sind. Diese Spalten sind unten markiert.

| Spaltenname              | Beschreibung                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantid            | Die primäre Identität des Partners, der sich unter dem Programm verdient                                                                           |
| participantidtype        | Normalerweise Programm-ID für Incentive-Programme und Verkäufer-ID für Store-Programme                                                              |
| participantName          | Name des Erwerbs Partners                                                                                                             |
| Program Name              | Name des Incentive/Store-Programms                                                                                                            |
| erzielten                   | Betrag, der in der Zahlen-zu-Währung für dieses Programm/diese Element-ID verdient wurde                                                                     |
| earnetzdusd                | Betrag für die Programm/Teilnehmer-ID in USD                                                                                    |
| withheldtax              | Betrag der in der Zahlen-zu-Währung für das Programm/participantid gehaltenen Steuerelement                                                             |
| salesTax                 | Gesamtbetrag der Umsatzsteuer in der Zahlen-zu-Währung für das Programm/die participantid                                                          |
| totalpayment             | Gesamtzahlung in der lokalen Währung ohne die zurück Haltungs Steuern und einschließlich der Umsatzsteuer (falls zutreffend) für die Programm/participantid |
| currencyCode             | Bezahlen an Währungscode                                                                                                                    |
| PaymentMethod            | Die Methode, die verwendet wird, um den Partner zu bezahlen (Electronic Bank Transfer, Bonitäts Hinweis)                                                              |
| PaymentID                | Eindeutiger Bezeichner für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar.                                               |
| paymentstatus            | Zahlungsstatus                                                                                                                          |
| paymentstatus Description | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                  |
| paymentdate              | Die Datums Zahlung wurde von Microsoft gesendet.                                                                                                    |

## <a name="transaction-history-download-export"></a>Transaktionsverlauf-Download Export

Mit dieser Option können Sie jedes auf der Seite Transaktionsverlauf gelaufene Element auf der Seite Transaktionsverlauf herunterladen, den Typ, das Datum, die zugehörige Transaktions Menge, den Kunden, das Produkt und andere Transaktionsdetails, die für Ihre Programme gelten, erwerben.

| Spaltenname                    | Beschreibung                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningid                      | Eindeutiger Bezeichner für jedes Einkommen                                                                                                       |
| participantid                  | Die primäre Identität des Partners, der sich unter dem Programm verdient                                                                            |
| participantidtype              | Verkäufer-ID                                                                                                                                |
| participantName                | Name des Erwerbs Partners                                                                                                              |
| partnercountrycode             | Standort/Land des Erwerbs Partners                                                                                                  |
| Program Name                    | Name des Incentive/Store-Programms                                                                                                             |
| transactionId                  | Eindeutiger Bezeichner für die Transaktion                                                                                                    |
| transaktioncurrency            | Währung, in der die ursprüngliche Kunden Transaktion aufgetreten ist                                                                             |
| TransactionDate                | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einem Verdienst beitragen                                           |
| transaktionexchangerate        | Wechselkurs Datum, das zum Anzeigen des entsprechenden USD Amount verwendet wird                                                                             |
| transaktionamount              | Transaktionsbetrag in der ursprünglichen Transaktionswährung, je nachdem, welche Einnahmen generiert werden                                              |
| transaktionsetusd           | Transaktionsbetrag in USD                                                                                                                |
| Tor                          | Gibt die Geschäftsregel für den Verdienst an.                                                                                                  |
| earningrate                    | Auf Transaktionsbetrag angewendete Incentive-Rate zum Generieren eines Erwerbs                                                                      |
| quantity                       | Variiert je nach Programm. Gibt die in Rechnung gestellte Menge für Transaktions Programme an.                                                            |
| earningtype                    | Gibt an, ob es sich um Gebühr, Rabatt, Coop, Sell usw. handelt.                                                                                          |
| earningamount                  | Betrag in der ursprünglichen Transaktionswährung                                                                                      |
| earningamountusd               | Betrag in USD                                                                                                                    |
| earningdate                    | Datum des Erwerbs                                                                                                                      |
| calculationdate                | Datum, an dem das Verdienst im System berechnet wurde                                                                                            |
| earningexchangerate            | Wechselkurs, der zum Anzeigen des entsprechenden USD Amount verwendet wird                                                                                  |
| exchangeratedate               | Zum Berechnen von "earningamount USD" verwendetes Wechselkurs Datum                                                                                   |
| claimid                        | Wird immer leer sein.                                                                                                                     |
| PaymentID                      | Eindeutiger Bezeichner für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar.                                                 |
| paymentstatus                  | Zahlungsstatus                                                                                                                           |
| paymentstatus Description       | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                   |
| CustomerID                     | Wird immer leer sein.                                                                                                                     |
| CustomerName                   | Wird immer leer sein.                                                                                                                     |
| PartNumber                     | Wird immer leer sein.                                                                                                                     |
| productName                    | Mit Transaktion verknüpfter Produktname                                                                                                       |
| productId                      | Eindeutige Produkt-ID                                                                                                                |
| parentProductId                | Eindeutige übergeordnete Produkt-ID. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“. |
| "parametriproductname"              | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.   |
| productType                    | Art des Produkts (z. B. App, Add-On, Spiel usw.)                                                                                        |
| invoicenumschlag                  | Wird immer leer sein.                                                                                                                     |
| SubscriptionId                 | Wird immer leer sein.                                                                                                                     |
| Abonnement StartDate          | Wird immer leer sein.                                                                                                                     |
| Abonnement Enddatum            | Wird immer leer sein.                                                                                                                     |
| resellerId                     | Wird immer leer sein.                                                                                                                     |
| resellerName                   | Wird immer leer sein.                                                                                                                     |
| verteilbare d                  | Wird immer leer sein.                                                                                                                     |
| Distributor Name                | Wird immer leer sein.                                                                                                                     |
| AgreementNumber                | Wird immer leer sein.                                                                                                                     |
| agreementstartdate             | Wird immer leer sein.                                                                                                                     |
| agreementenddate               | Wird immer leer sein.                                                                                                                     |
| beitragen                       | Wird immer leer sein.                                                                                                                     |
| Transaktionstyp                | Art der Transaktion (z. B. Einkauf, Erstattung, Rückbuchung, Ausgleich usw.)                                                               |
| localproviderseller            | Eingetragener lokaler Anbieter/Verkäufer                                                                                                          |
| taxreover                    | Höhe der bezahlten Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer).                                                                                   |
| taxremitmodel                  | Für die Überweisung von Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer) zuständige Partei.                                                                    |
| storefee                       | Der von Microsoft beibehaltene Betrag für die Bereitstellung der APP oder des Add-Ins im Store.                                           |
| transaktionpaymentmethod       | Kundenzahlungsmittel, das für die Transaktion verwendet wird (z. B. Kreditkarte, Mobilfunkanbieterrechnung, PayPal usw.)                                |
| tpan                           | Gibt das Drittanbieter-Ad-Netzwerk an.                                                                                                     |
| purchasetypeer Code               | Wird immer leer sein.                                                                                                                     |
| PurchaseOrderType              | Wird immer leer sein.                                                                                                                     |
| purchaseordercoveragestartdate | Wird immer leer sein.                                                                                                                     |
| purchaseordercoverageenddate   | Wird immer leer sein.                                                                                                                     |
| externalreferenceid            | Wird immer leer sein.                                                                                                                     |
| externalreferenceidlabel       | Wird immer leer sein.                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Download der Auszahlungs Anweisung Download (Legacy)

Für einen begrenzten Zeitraum auf der Seite alte Auszahlungs Zusammenfassung stehen Auszahlungs Anweisungen zum Download zur Verfügung. Dieser Bericht enthält die folgenden Felder.

> [!NOTE]
> Der Legacy Transaktionsverlauf verfügt über eine Spalte mit dem Namen "reserviert", die der Spalte "Gewinn" im modernen Verlauf entspricht, mit dem Unterschied, dass der gesamte Gewinn mit Status = "Payment sent" ausgeschlossen wird.

| Name des Felds              | Beschreibung                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Umsatzquelle          | Die Quelle der Umsätze, basierend auf dem Transaktionsort (z. B. Microsoft Store, Windows Phone Store, Microsoft Store 8, Werbung usw.).                  |
| Bestellnummer                | Eindeutiger Bezeichner für die Bestellung. Anhand dieser ID können Sie kaufbezogene Transaktionen und die entsprechenden Transaktionen, die sich nicht auf Käufe beziehen (z. B. Erstattungen, Rückvergütungen usw.), ermitteln. Beide Arten von Transaktionen besitzen die gleiche Bestell-ID. Im Fall einer aufgeteilten Belastung, bei der mehrere Zahlungsmethoden für einen einzelnen Einkauf verwendet wurden, können Sie die kaufbezogenen Transaktionen verknüpfen. |
| Transaktions-ID          | Eindeutige Transaktions-ID.                                                                                                                                          |
| Datum/Uhrzeit der Transaktion   | Das Datum und die Uhrzeit der Transaktion (UTC).                                                                                                                       |
| Übergeordnete Produkt-ID       | Eindeutige übergeordnete Produkt-ID. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“.                                |
| Produkt-ID              | Eindeutige Produkt-ID.                                                                                                                                              |
| Name des übergeordneten Produkts     | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.                                  |
| Produktname            | Name des Produkts.                                                                                                                                                    |
| Produkttyp            | Art des Produkts (z. B. App, Add-On, Spiel usw.)                                                                                                                       |
| Anzahl                | Wenn die Umsatzquelle „Microsoft Store für Unternehmen“ ist, gibt die Menge die Anzahl der erworben Lizenzen an. Für alle anderen Umsatzquellen ist die Menge immer 1. Hinweis: Auch wenn eine Transaktion in zwei Positionen unterteilt wird, weil zwei verschiedene Zahlungsmethoden verwendet wurden, wird für jede Position die Menge 1 angezeigt. |
| Transaktionstyp        | Art der Transaktion (z. B. Einkauf, Erstattung, Rückbuchung, Ausgleich usw.)                                                                                              |
| Zahlungsmethode          | Kundenzahlungsmittel, das für die Transaktion verwendet wird (z. B. Kreditkarte, Mobilfunkanbieterrechnung, PayPal usw.)                                                               |
| Land/Region        | Land/Region, in dem die Transaktion durchgeführt wurde.                                                                                                                          |
| Lokaler Anbieter/Verkäufer | Eingetragener lokaler Anbieter/Verkäufer.                                                                                                                                        |
| Transaktionswährung    | Währung der Transaktion.                                                                                                                                            |
| Transaktionsbetrag      | Betrag der Transaktion.                                                                                                                                              |
| Bezahlte Steuern            | Höhe der bezahlten Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer).                                                                                                                  |
| Nettoeinnahmen            | Transaktionsbetrag minus bezahlter Steuern.                                                                                                                                   |
| Store-Gebühr               | Der Prozentsatz der Nettoeinnahmen, die von Microsoft als Gebühr für das Verfügbarmachen der App oder des Add-Ons im Store erhoben wird                                                      |
| App-Erlöse            | Nettoeinnahmen abzüglich der Store-Gebühr.                                                                                                                                       |
| Einbehaltene Steuern          | Höhe der einbehaltenen Einkommensteuer. (Nicht enthalten in der **reservierten** .csv-Datei.)                                                                                                |
| Auszahlung                 | App-Erlöse abzüglich aller geltenden Steuereinbehaltungen (Betrag angezeigt in Transaktionswährung). (Nicht enthalten in der **reservierten** .csv-Datei.)                               |
| Wechselkurs                 | Wechselkurs für die Umrechnung der Transaktionswährung in die Auszahlungswährung.                                                                                         |
| Auszahlungswährung        | Währung der Auszahlung.                                                                                                                                       |
| Umgerechnete Auszahlung       | Basierend auf dem Wechselkurs in die Auszahlungswährung umgerechneter Auszahlungsbetrag.                                                                                                         |
| Steuerzahlungsmodell         | Für die Überweisung von Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer) zuständige Partei.                                                                                                   |
| Qualifikationsdatum/Uhrzeit   | Datum und Uhrzeit, an dem/zu der der Transaktionserlös zur Auszahlung qualifiziert wird (UTC). Wenn eine Auszahlung erstellt wird, enthält diese Transaktionserlöse, die einen Qualifizierungszeitpunkt enthalten, der vor dem Erstellungsdatum der Auszahlung liegt. (Nur enthalten in der **reservierten** .csv-Datei.) |
| Gebühren                 | Zeigt eine Aufschlüsselung aller Gebührendetails, aggregiert in der Spalte „Transaktionsbetrag“. (Nur für Azure Marketplace; nicht enthalten in der **reservierten** .csv-Datei.) |
