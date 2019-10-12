---
Description: Die Auszahlungsübersicht enthält Details zu den mit Ihren Apps und Add-Ons erzielten Erlösen. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.
title: Auszahlungsübersicht
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: Windows 10 UWP, Auszahlungszusammenfassung, Anweisung, Zahlungen, Einnahmen, Auszahlung, Einnahmen
ms.localizationpriority: medium
ms.openlocfilehash: 89cb689f0dce4f7dbaec96e9ce109e60d4292f92
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282480"
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
| Failed                   | Die Zahlung ist aufgrund eines Microsoft-Systemfehlers fehlgeschlagen.                                                                                         | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/en-us/windows/support) .                      |
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

Hinweise:

- Die Seite "Daten exportieren" wird nicht eigenständig aktualisiert. Möglicherweise müssen Sie die Seite manuell aktualisieren, um die neuesten Daten anzuzeigen.
- Der Filter führt möglicherweise zu einem Fehler, der nicht verfügbar ist. Dies bedeutet wahrscheinlich, dass Sie den Standard Zeitraum in drei Monaten ausgewählt haben und dann eine Zahlungs-ID aus einem Verdienst außerhalb dieses Zeitraums ausgewählt haben. Erweitern Sie den Zeitraum, und versuchen Sie es noch mal.

## <a name="payment-download-export"></a>Zahlungs Download Export

Mit dieser Option können Sie die Zahlungen, die Sie in Ihrer Bank für ein bestimmtes Programm erhalten haben, sowie die zugehörigen Steuern und den aggregierten Erwerbs Betrag herunterladen. Dieser Bericht wird für viele Partner Center-Programme verwendet, sodass einige Spalten für den Bericht möglicherweise nicht zutreffend sind. Diese Spalten sind unten markiert.

| Spaltenname              | Beschreibung                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantid            | Die primäre Identität des Partners, der sich unter dem Programm verdient                                                                             |
| participantidtype        | Normalerweise Programm-ID für Incentive-Programme und Verkäufer-ID für Store-Programme                                                                |
| participantName          | Name des Erwerbs Partners                                                                                                               |
| Program Name              | Name des Incentive/Store-Programms                                                                                                              |
| erzielten                   | Betrag, der in der Zahlen-zu-Währung für dieses Programm/diese Element-ID verdient wurde                                                                       |
| earnetzdusd                | Betrag für die Programm/Teilnehmer-ID in USD                                                                                      |
| withheldtax              | Betrag der in der Zahlen-zu-Währung für das Programm/participantid gehaltenen Steuerelement                                                               |
| salesTax                 | Gesamtbetrag der Umsatzsteuer in der Zahlen-zu-Währung für das Programm/die participantid (gilt nur für Incentive-Programme)                   |
| servicefeetax            | Gesamtmenge der servicefeetax in "Pay-to-Currency" für das Programm/die "participantid" (gilt nur für Store-Programme und Azure Marketplace) |
| totalpayment             | Gesamtzahlung in der lokalen Währung ohne die zurück Haltungs Steuern und einschließlich der Umsatzsteuer (falls zutreffend) für die Programm/participantid   |
| currencyCode             | Bezahlen an Währungscode                                                                                                                      |
| PaymentMethod            | Die Methode, mit der der Partner bezahlt wird, z. b. elektronischer Bank Transfer, Bonitäts Hinweis                                                             |
| PaymentID                | Eindeutiger Bezeichner für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar. (gilt nur für SAP-Zahlungen)              |
| paymentstatus            | Zahlungsstatus                                                                                                                            |
| paymentstatus Description | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                    |
| paymentdate              | Die Datums Zahlung wurde von Microsoft gesendet.                                                                                                      |

## <a name="transaction-history-download-export"></a>Transaktionsverlauf-Download Export

Mit dieser Option können Sie jedes auf der Seite Transaktionsverlauf gelaufene Element auf der Seite Transaktionsverlauf herunterladen, den Typ, das Datum, die zugehörige Transaktions Menge, den Kunden, das Produkt und andere Transaktionsdetails, die für Ihre Programme gelten, erwerben.

| Spaltenname                    | Beschreibung                                                                                                                              | Anwendbarkeit für Incentives/Store/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningid                      | Eindeutiger Bezeichner für jedes Einkommen                                                                                                       | All                                                            |
| participantid                  | Die primäre Identität des Partners, der sich unter dem Programm verdient                                                                            | All                                                            |
| participantidtype              | Hauptsächlich Programm-ID für Incentive-Programme und Verkäufer, wenn für Store-Programme und Azure Marketplace                                          | All                                                            |
| participantName                | Name des Erwerbs Partners                                                                                                              | All                                                            |
| partnercountrycode             | Standort/Land des Erwerbs Partners                                                                                                  | All                                                            |
| Program Name                    | Name des Incentive/Store-Programms                                                                                                             | All                                                            |
| transactionId                  | Eindeutiger Bezeichner für die Transaktion                                                                                                    | All                                                            |
| transaktioncurrency            | Währung, in der die ursprüngliche Kunden Transaktion aufgetreten ist (Dies ist keine Währung für den Partnerstandort)                                     | All                                                            |
| TransactionDate                | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einem Verdienst beitragen                                           | All                                                            |
| transaktionexchangerate        | Wechselkurs Datum zum Anzeigen der entsprechenden Transaktion in USD                                                                 | All                                                            |
| transaktionamount              | Transaktionsbetrag in der ursprünglichen Transaktionswährung, je nachdem, welche Einnahmen generiert werden                                              | All                                                            |
| transaktionsetusd           | Transaktionsbetrag in USD                                                                                                                | All                                                            |
| Tor                          | Gibt die Geschäftsregel für den Verdienst an.                                                                                                  | All                                                            |
| earningrate                    | Auf Transaktionsbetrag angewendete Incentive-Rate zum Generieren eines Erwerbs                                                                      | All                                                            |
| quantity                       | Variiert je nach Programm. Gibt die in Rechnung gestellte Menge für Transaktions Programme an.                                                            | All                                                            |
| quantitytype                   | Gibt den Typ der Menge an, z. b. berechnete Menge, Mau                                                                                     | All                                                            |
| earningtype                    | Gibt an, ob es sich um Gebühr, Rabatt, Coop, Sell usw. handelt.                                                                                          | All                                                            |
| earningamount                  | Betrag in der ursprünglichen Transaktionswährung                                                                                      | All                                                            |
| earningamountusd               | Betrag in USD                                                                                                                    | All                                                            |
| earningdate                    | Datum des Erwerbs                                                                                                                      | All                                                            |
| calculationdate                | Datum, an dem das Verdienst im System berechnet wurde                                                                                            | All                                                            |
| earningexchangerate            | Wechselkurs, der zum Anzeigen des entsprechenden USD Amount verwendet wird                                                                                  | All                                                            |
| exchangeratedate               | Zum Berechnen von "earningamount USD" verwendetes Wechselkurs Datum                                                                                   | All                                                            |
| paymentpaytwotax             | Betrag (ohne Steuern) in Zahlen an Währungen für "Gesendete" Zahlungen                                                                 | All                                                            |
| paymentcurrency                | Bezahlen Sie die Währung, die vom Partner im Zahlungsprofil gewählt wurde. Nur für gesendete Zahlungen angezeigt                                                   | All                                                            |
| paymentexchangerate            | Wechselkurs zur Berechnung von paymentpaytwotax in der Zahlungswährung mithilfe von exchangeratedate                                            | All                                                            |
| claimid                        | Eindeutiger Bezeichner für Anspruch                                                                                                              | Incentives: nur einige Programme                                |
| planId                         | Eindeutiger Bezeichner für den Plan                                                                                                               | Incentives: nur einige Programme                                |
| PaymentID                      | Eindeutiger Bezeichner für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar.                                                 | Nur SAP-Zahlungen                                              |
| paymentstatus                  | Zahlungsstatus                                                                                                                           | All                                                            |
| paymentstatus Description       | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                   | All                                                            |
| CustomerID                     | Wird immer leer sein.                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| CustomerName                   | Wird immer leer sein.                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| PartNumber                     | Wird immer leer sein.                                                                                                                     | Einige Incentive-und Store-Programme und Azure Marketplace        |
| productName                    | Mit Transaktion verknüpfter Produktname                                                                                                       | All                                                            |
| productId                      | Eindeutige Produkt-ID                                                                                                                | Speichern und Azure Marketplace                                    |
| parentProductId                | Eindeutige übergeordnete Produkt-ID. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“. | Speichern und Azure Marketplace                                    |
| "parametriproductname"              | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.   | Speichern und Azure Marketplace                                    |
| productType                    | Art des Produkts (z. B. App, Add-On, Spiel usw.)                                                                                        | Speichern und Azure Marketplace                                    |
| invoicenumschlag                  | Rechnungsnummer (gilt nur für EA)                                                                                                  | Incentive und Azure Marketplace: nur einige Programme           |
| SubscriptionId                 | Dem Kunden zugeordneter Abonnement Bezeichner                                                                                         | Incentive: nur einige Programme                                 |
| Abonnement StartDate          | Startdatum des Abonnements                                                                                                                  | Incentive: nur einige Programme                                 |
| Abonnement Enddatum            | Enddatum des Abonnements                                                                                                                    | Incentive: nur einige Programme                                 |
| resellerId                     | Reseller-Bezeichner                                                                                                                      | Incentive: nur einige Programme                                 |
| resellerName                   | Name des Wiederverkäufers                                                                                                                            | Incentive: nur einige Programme                                 |
| verteilbare d                  | Verteiler Bezeichner                                                                                                                   | Incentive: nur einige Programme                                 |
| Distributor Name                | Verteiler Name                                                                                                                         | Incentive: nur einige Programme                                 |
| AgreementNumber                | Vereinbarungsnummer                                                                                                                         | Incentive: nur einige Programme                                 |
| agreementstartdate             | Vereinbarungsstartdatum                                                                                                                     | Incentive: nur einige Programme                                 |
| agreementenddate               | Vereinbarungsenddatum                                                                                                                       | Incentive: nur einige Programme                                 |
| beitragen                       | Workload                                                                                                                                 | Incentive: nur einige Programme                                 |
| Transaktionstyp                | Art der Transaktion (z. B. Einkauf, Erstattung, Rückbuchung, Ausgleich usw.)                                                               | Speichern und Azure Marketplace                                    |
| localproviderseller            | Eingetragener lokaler Anbieter/Verkäufer                                                                                                          | Nur speichern                                                     |
| taxreover                    | Höhe der bezahlten Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer).                                                                                   | Speichern und Azure Marketplace                                    |
| taxremitmodel                  | Für die Überweisung von Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer) zuständige Partei.                                                                    | Nur speichern                                                     |
| storefee                       | Der von Microsoft beibehaltene Betrag für die Bereitstellung der APP oder des Add-Ins im Store.                                           | Nur speichern                                                     |
| transaktionpaymentmethod       | Kundenzahlungsmittel, das für die Transaktion verwendet wird (z. B. Kreditkarte, Mobilfunkanbieterrechnung, PayPal usw.)                                | Speichern und Azure Marketplace                                    |
| tpan                           | Gibt das Drittanbieter-Ad-Netzwerk an.                                                                                                     | Nur Store-ADS                                               |
| CustomerCountry                | Land des Kunden                                                                                                                         | Speichern und Azure Marketplace                                    |
| customercity                   | Kunden Stadt                                                                                                                            | Speichern und Azure Marketplace                                    |
| customerstate                  | Kunden Zustand                                                                                                                           | Speichern und Azure Marketplace                                    |
| customerzip                    | Kunden-Zip/Postleitzahl                                                                                                                 | Speichern und Azure Marketplace                                    |
| purchasetypeer Code               | Wird immer leer sein.                                                                                                                     | Incentive Program-CRI                                        |
| PurchaseOrderType              | Wird immer leer sein.                                                                                                                     | Incentive Program-CRI                                        |
| purchaseordercoveragestartdate | Wird immer leer sein.                                                                                                                     | Incentive Program-CRI                                        |
| purchaseordercoverageenddate   | Wird immer leer sein.                                                                                                                     | Incentive Program-CRI                                        |
| programufferinglevel           |                                                                                                                                          | Incentive Program-CRI                                        |
| TenantID                       |                                                                                                                                          | Incentive-Programme                                             |
| externalreferenceid            | Eindeutiger Bezeichner für das Programm                                                                                                        | Direct Pay-Programme (Incentive und Store)                      |
| externalreferenceidlabel       | Eindeutige bezeichnerbezeichnung                                                                                                                  | Direct Pay-Programme (Incentive und Store)                      |
