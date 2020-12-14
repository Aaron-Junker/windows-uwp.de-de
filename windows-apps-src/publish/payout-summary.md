---
description: In den Auszahlungs Berichten werden Details zu den Kosten angezeigt, die Sie mit ihren apps und Add-ons erworben haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.
title: Auszahlungsberichte
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: Windows 10, UWP, Auszahlungs Zusammenfassung, Statement, Payment, Gewinn, Auszahlungen, Zahlung, fortschreitet
ms.localizationpriority: medium
ms.openlocfilehash: 996f77db55d959fb1e328e4e722166cc466a8dbd
ms.sourcegitcommit: d5ad11e2289d1d04319405a78dd02aaacf866e3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97390358"
---
# <a name="payout-reports"></a>Auszahlungsberichte

Die **Auszahlungs Zusammenfassung** zeigt Ihnen Details zu dem Geld, das Sie bei Microsoft erworben haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.

Wenn Sie Produkte in der Azure Marketplace verkaufen, sehen Sie auch Informationen zu erfolgreichen Auszahlungen in der **Auszahlungs Zusammenfassung**. Weitere Informationen zur Bezahlung im Azure Marketplace finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](/legal/marketplace/participation-policy) und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Qualifiziert für eine Auszahlung sind Beträge, die den [Zahlungsschwellenwert](payment-thresholds-methods-and-timeframes.md) von 50 US-Dollar erreichen. Weitere Informationen zum Zahlungsschwellenwert finden Sie auf dieser Seite und in der Vereinbarung für App-Entwickler.

> [!NOTE]
> Wenn Sie Unterstützung in Bezug auf Auszahlungen benötigen, einschließlich der Konfiguration von Auszahlungskonten, fehlenden Auszahlungen, dem Zurückhalten von Auszahlungen oder sonstigen Fragen, wenden Sie sich [hier](https://developer.microsoft.com/windows/support) an den Support.

## <a name="access-the-payout-summary-pages"></a>Zugreifen auf die Seiten der Auszahlungszusammenfassung

Wenn Sie eine der Seiten der Auszahlungszusammenfassung öffnen möchten, gehen Sie folgendermaßen vor:

1. Klicken Sie in der oberen rechten Ecke auf das Auszahlungssymbol.
2. Wählen Sie „Transaktionsverlauf“, „Zahlungen“ oder „Daten exportieren“ aus.

## <a name="transaction-history-page"></a>Seite „Transaktionsverlauf“

Auf dieser Seite werden alle Ihre individuellen Einnahmen angezeigt, einschließlich Datum, Typ und jeweiliger Einnahme. Sie können einen Zeitraum auswählen, der angezeigt werden soll. Außerdem können Sie nach Registrierungs-ID, Programm, Zahlungs-ID, Erstellungsart, Hebel, geschätzter Zahlungs Monat, Anpassung und Status filtern. Es sind Daten für das aktuelle Geschäftsjahr (1. Juli bis 30. Juni) und die letzten beiden Geschäftsjahre verfügbar.

Zum Anzeigen weiterer Details zu einer Einnahme klicken Sie rechts auf der Seite auf den Pfeil nach unten. Dadurch werden der Hebel, der Umsatz Betrag, der geschätzte Zahlungs Monat und das Produkt angezeigt. Wenn eine dieser Daten aus irgendeinem Grund nicht verfügbar ist, Sie aber darauf zugreifen müssen, wenden Sie sich an den [Support](https://developer.microsoft.com/windows/support). Wenn das Ergebnis einer Anpassung und nicht einer Transaktion entspricht, werden die Produktfelder nicht angezeigt. Der Anpassungs Grund wird nicht angezeigt, es sei denn, es gibt eine Anpassung, und der Anpassungs Filter ist ausgewählt.

Wählen Sie zum Exportieren der Transaktionsdaten auf dieser Seite die Option Exportieren aus, und befolgen Sie dann die Anweisungen auf der Seite Daten exportieren. Dateien, die aus der Seite Transaktionsverlauf exportiert wurden, zeigen Daten in Transaktionswährung, Einnahmen in Transaktionswährung und US-Dollar sowie den bezahlten Wert in Zahlung in Währung an.

## <a name="payments-page"></a>Seite „Zahlungen“

Die Summen auf dieser Seite stehen für alle Programme, an denen Sie teilnehmen. Sie können nach Teilnehmer-ID, Programm, Zahlungs-ID und Einnahmetyp filtern. Beträge werden in US-Dollar angegeben. Der gezahlte Wert wird auch in der Auszahlungswährung angezeigt.

| Bereich                   | BESCHREIBUNG                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Zahlungen gesamt in diesem Jahr   | Die kombinierte Summe, die Sie in diesem Jahr in US-Dollar für alle Ihre Programme bezahlt haben.       |
| Nächste geschätzte Zahlung | Die nächste nächste Zahlung an Sie (auch wenn andere in Kürze verfügbar sind) in US-Dollar. |
| Letzte Zahlung           | Der Betrag (in US-Dollar), der Programmname und das Programm Ihrer letzten Zahlung.           |
| Zahlungen nach Quelle     | Die Menge der Zahlungen in US-Dollar, die durch das Programm in den letzten 12 Monaten dargestellt wird.           |
| Zahlungen               | Wählen Sie bezahlt oder ausstehend aus, und sortieren Sie Sie nach Ihren Wünschen. Zum Anzeigen weiterer Details einer bestimmten Zahlung wählen Sie Ansicht aus. Wenn Sie eine Kopie der Zahlungsanweisung herunterladen möchten, wählen Sie Herunterladen aus. Beachten Sie, dass es bis zu 24 Stunden dauern kann, bis die Daten für den Transaktionsverlauf angezeigt werden, sodass die zugehörigen Gewinne möglicherweise nicht sofort angezeigt werden. |

Wählen Sie zum Exportieren der Daten auf dieser Seite exportieren aus, und befolgen Sie dann die Anweisungen auf der Seite Daten exportieren.

## <a name="payment-status"></a>Zahlungsstatus

| Einnahmenstatus           | `Reason`                                                                                                                                      | Partneraktion erforderlich?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Nicht verarbeitet              | Die Einnahme ist für eine Auszahlung qualifiziert. Sie behält diesen Status für die Dauer der sogenannten „Cooling Period“, wie sie im Programmhandbuch für das Incentive-Programm definiert ist. | Nein                                                         |
| Anstehend                 | Der Zahlungsauftrag hat ausstehende interne Überprüfungen generiert, bevor die Zahlung verarbeitet wird.                                                               | Nein                                                         |
| Ausstehende Steuerrechnung      | Ihre Steuerrechnung ist unvollständig oder ungültig.                                                                                                  | Sie müssen Ihre Steuerrechnung aktualisieren, bevor Sie bezahlt werden können. |
| Bei Überprüfung abgelehnt   | Die Zahlung wurde während der Überprüfung abgelehnt.                                                                                                     | Wenden Sie sich an den [Microsoft-Support](https://developer.microsoft.com/windows/support), um weitere Informationen zu erhalten.                      |
| Fehler                   | Die Zahlung ist aufgrund eines Microsoft-Systemfehlers fehlgeschlagen.                                                                                         | Wenden Sie sich an den [Microsoft-Support](https://developer.microsoft.com/windows/support), um weitere Informationen zu erhalten.                      |
| In Bearbeitung              | Die Zahlung wird ausgeführt.                                                                                                                 | Nein                                                         |
| Falsche Zahlung        | Der Zahlungsvorgang wird wiederholt.                                                                                                       | Nein                                                         |
| Gesendet                     | Die Zahlung wurde an Ihre Bank gesendet.                                                                                                     | Nein                                                         |
| Erneute Verarbeitung             | Bei der Zahlung ist ein Microsoft-Systemfehler aufgetreten, der erneut verarbeitet wird.                                                                  | Nein                                                         |
| Reversed                 | Die Zahlung wurde von Ihrer Bank rückgängig gemacht und wird im nächsten Zahlungszeitraum erneut gesendet.                                                     | Nein                                                         |
| Steuerrechnung abgelehnt     | Ihre Steuerrechnung wurde während der Überprüfung abgelehnt. Alle ausstehenden Zahlungen werden zurückgehalten, bis die Überprüfung der Steuerrechnung abgeschlossen ist.                 | Wenden Sie sich an den [Microsoft-Support](https://developer.microsoft.com/windows/support), um weitere Informationen zu erhalten.                      |
| Steuerrechnung wird geprüft | Ihre Steuerrechnung wird geprüft. Ihre Zahlung wird freigegeben, sobald die Steuerrechnung genehmigt wurde.                                   | Nein                                                         |
| Rejected (Abgelehnt)                 | Die Zahlung wurde von Ihrer Bank abgelehnt.                                                                                                      | Weitere Informationen erhalten Sie von Ihrer Bank.                             |

## <a name="export-data-page"></a>Seite „Daten exportieren“

Befolgen Sie die Anweisungen auf dieser Seite, um die gewünschten Daten zu exportieren.

Hinweise:

- Die Seite „Daten exportieren“ wird nicht eigenständig aktualisiert. Möglicherweise müssen Sie die Seite manuell aktualisieren, um die neuesten Daten anzuzeigen.
- Ihr Filter bewirkt möglicherweise einen Fehler vom Typ Keine Daten verfügbar. Dies bedeutet wahrscheinlich, dass Sie den Standard Zeitraum in drei Monaten ausgewählt haben und dann eine Zahlungs-ID aus einem Verdienst außerhalb dieses Zeitraums ausgewählt haben. Erweitern Sie den Zeitraum, und versuchen Sie es erneut.

## <a name="payments"></a>Payments

![Exportieren von Zahlungen](images/pc-export-payments.png)

Mit dieser Option können Sie die Zahlungen, die Sie in Ihrer Bank für ein bestimmtes Programm erhalten haben, sowie die zugehörigen Steuern und den aggregierten Einnahmebetrag herunterladen. Dieser Bericht wird für viele Partner Center-Programme verwendet, sodass möglicherweise einige Spalten für Ihren Bericht nicht zutreffen. Diese Spalten sind nachfolgend markiert.

| Spaltenname              | BESCHREIBUNG                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt                                                                             |
| participantIDType        | Normalerweise Programm-ID für Incentive-Programme und Verkäufer-ID für Store-Programme                                                                |
| participantName          | Name des Partners, der die Einnahme erzielt                                                                                                               |
| programName              | Name des Incentive-/Store-Programms                                                                                                              |
| earned                   | Einnahmebetrag in der Auszahlungswährung für diese Programm-/Teilnehmer-ID                                                                       |
| earnedUSD                | Einnahmebetrag für die Programm-/Teilnehmer-ID in US-Dollar                                                                                      |
| withheldTax              | Einbehaltener Steuerbetrag in der Auszahlungswährung für die Programm-/Teilnehmer-ID                                                               |
| salesTax                 | Gesamtbetrag der Verkaufssteuer in der Auszahlungswährung für die Programm-/Teilnehmer-ID (gilt nur für Incentive-Programme)                   |
| serviceFeeTax            | Gesamtbetrag der Steuer auf Servicegebühren in der Auszahlungswährung für die Programm-/Teilnehmer-ID (gilt nur für Store-Programme und Azure Marketplace) |
| totalPayment             | Gesamtzahlung in der lokalen Währung ohne einbehaltene Steuern und einschließlich Verkaufssteuer (falls zutreffend) für die Programm-/Teilnehmer-ID   |
| currencyCode             | Code der Auszahlungswährung                                                                                                                      |
| paymentMethod            | Die Methode, mit der der Partner bezahlt wird, z. b. elektronischer Bank Transfer, Bonitäts Hinweis                                                     |
| paymentID                | Eindeutige ID für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar. (gilt nur für SAP-Zahlungen)              |
| paymentStatus            | Zahlungsstatus                                                                                                                            |
| paymentStatusDescription | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                    |
| paymentDate              | Datum, an dem die Zahlung von Microsoft gesendet wurde                                                                                                      |

## <a name="transaction-history"></a>Transaktionsverlauf

![Exportieren des Transaktionsverlaufs](images/pc-export-transaction.png)

Mit dieser Option können Sie jede Einnahmeposition, die auf der Seite „Transaktionsverlauf“ angezeigt wird, einschließlich Einnahmetyp, Datum, zugehörigem Transaktionsbetrag, Kunde, Produkt und anderen Transaktionsdetails, die für Ihre Programme gelten, herunterladen.

| Spaltenname                    | BESCHREIBUNG                                                                                                                              | Anwendbarkeit für Incentives/Store/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Eindeutiger Bezeichner für jede Einnahme                                                                                                       | All                                                            |
| participantID                  | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt                                                                            | All                                                            |
| participantIdType              | Meistens eine Programm-ID für Incentive-Programme und Verkäufer, WENN es sich um Store-Programme und Azure Marketplace handelt                                          | All                                                            |
| participantName                | Name des Partners, der die Einnahme erzielt                                                                                                              | All                                                            |
| partnerCountryCode             | Standort/Land des Partners, der die Einnahme erzielt                                                                                                  | All                                                            |
| programName                    | Name des Incentive-/Store-Programms                                                                                                             | All                                                            |
| transactionId                  | Eindeutige ID für die Transaktion                                                                                                    | All                                                            |
| transactionCurrency            | Währung, in der die ursprüngliche Kundentransaktion ausgeführt wurde (dies ist nicht die Währung für den Partnerstandort)                                     | All                                                            |
| transactionDate                | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einer Einnahme beitragen                                           | All                                                            |
| transactionExchangeRate        | Datum des Wechselkurses zum Anzeigen des entsprechenden Transaktionsbetrags in US-Dollar                                                                 | All                                                            |
| transactionAmount              | Transaktionsbetrag in der ursprünglichen Transaktionswährung, auf deren Basis die Einnahme generiert wird                                              | All                                                            |
| transactionAmountUSD           | Transaktionsbetrag in US-Dollar                                                                                                                | All                                                            |
| lever                          | Geschäftsregel für die Einnahme                                                                                                  | All                                                            |
| earningrate                    | Auf den Transaktionsbetrag angewendete Incentive-Rate zum Generieren einer Einnahme                                                                      | All                                                            |
| quantity                       | Abhängig vom Programm. Gibt die in Rechnung gestellte Menge für Transaktionsprogramme an                                                            | All                                                            |
| quantityType                   | Gibt den Typ der Menge an, z. b. berechnete Menge, Mau                                                                             | All                                                            |
| earningType                    | Gibt an, ob es sich um eine Gebühr, einen Rabatt, Coop, Verkauf usw. handelt.                                                                                          | All                                                            |
| earningamount                  | Einnahmebetrag in der ursprünglichen Transaktionswährung                                                                                      | All                                                            |
| earningAmountUSD               | Einnahmebetrag in US-Dollar                                                                                                                    | All                                                            |
| earningDate                    | Datum der Einnahme                                                                                                                      | All                                                            |
| calculationDate                | Datum der Berechnung der Einnahme im System                                                                                            | All                                                            |
| earningExchangeRate            | Wechselkurs zum Anzeigen des entsprechenden Betrags in US-Dollar                                                                                  | All                                                            |
| exchangeRateDate               | Datum des Wechselkurses, der zum Berechnen des Einnahmebetrags in US-Dollar verwendet wurde                                                                                   | All                                                            |
| paymentAmountWOTax             | Einnahmebetrag (ohne Steuern) in der Auszahlungswährung nur für Zahlungen mit dem Status „Gesendet“                                                                 | All                                                            |
| paymentCurrency                | Vom Partner im Zahlungsprofil ausgewählte Auszahlungswährung. Wird nur für gesendete Zahlungen angezeigt                                                   | All                                                            |
| paymentExchangeRate            | Wechselkurs zur Berechnung von „paymentAmountWOTax“ in der Auszahlungswährung mithilfe von „ExchangeRateDate“                                            | All                                                            |
| claimId                        | Eindeutiger Bezeichner für den Anspruch                                                                                                              | Incentives – nur einige Programme                                |
| planId                         | Eindeutiger Bezeichner für den Plan                                                                                                               | Incentives – nur einige Programme                                |
| paymentId                      | Eindeutige ID für die Zahlung. Diese Nummer ist normalerweise auf Ihrem Kontoauszug zu sehen                                                 | Nur SAP-Zahlungen                                              |
| paymentStatus                  | Zahlungsstatus                                                                                                                           | All                                                            |
| paymentStatusDescription       | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                   | All                                                            |
| customerId                     | Ist immer leer                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| customerName                   | Ist immer leer                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| partNumber                     | Ist immer leer                                                                                                                     | Einige Incentive- und Store-Programme und Azure Marketplace        |
| ProductName                    | Mit der Transaktion verknüpfter Produktname                                                                                                       | All                                                            |
| productId                      | Eindeutige Produkt-ID                                                                                                                | Store und Azure Marketplace                                    |
| parentProductId                | Eindeutige ID des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“. | Store und Azure Marketplace                                    |
| parentProductName              | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.   | Store und Azure Marketplace                                    |
| productType                    | Produkttyp (z. B. App, Add-On, Spiel usw.)                                                                                        | Store und Azure Marketplace                                    |
| InvoiceNumber                  | Rechnungsnummer (gilt nur für EA)                                                                                                  | Incentive und Azure Marketplace – nur einige Programme           |
| subscriptionId                 | Abonnement-ID, die dem Kunden zugeordnet ist                                                                                         | Incentive – nur einige Programme                                 |
| subscriptionStartDate          | Startdatum des Abonnements                                                                                                                  | Incentive – nur einige Programme                                 |
| subscriptionEndDate            | Enddatum des Abonnements                                                                                                                    | Incentive – nur einige Programme                                 |
| resellerId                     | ID des Handelspartners                                                                                                                      | Incentive – nur einige Programme                                 |
| resellerName                   | Name des Handelspartners                                                                                                                            | Incentive – nur einige Programme                                 |
| distributorId                  | Verteiler-ID                                                                                                                   | Incentive – nur einige Programme                                 |
| distributorName                | Verteilername                                                                                                                         | Incentive – nur einige Programme                                 |
| agreementNumber                | Vereinbarungsnummer                                                                                                                         | Incentive – nur einige Programme                                 |
| agreementStartDate             | Startdatum der Vereinbarung                                                                                                                     | Incentive – nur einige Programme                                 |
| agreementEndDate               | Enddatum der Vereinbarung                                                                                                                       | Incentive – nur einige Programme                                 |
| workload                       | Workload                                                                                                                                 | Incentive – nur einige Programme                                 |
| transactionType                | Transaktionstyp (z. B. Kauf, Erstattung, Stornierung, Rückbelastung usw.)                                                               | Store und Azure Marketplace                                    |
| localProviderSeller            | Lokaler Anbieter/Seller of Record                                                                                                          | Nur Store                                                     |
| taxRemitted                    | Betrag der abgeführten Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer).                                                                                   | Store und Azure Marketplace                                    |
| taxRemitModel                  | Partei, die für das Abführen von Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer) verantwortlich ist.                                                                    | Nur Store                                                     |
| storeFee                       | Der von Microsoft als Gebühr für die Bereitstellung der App oder des Add-Ins im Store einbehaltene Betrag.                                           | Nur Store                                                     |
| transactionPaymentMethod       | Vom Kunden für die Transaktion verwendetes Zahlungsmittel (z.B. Scheckkarte, Mobile Carrier Billing, PayPal usw.)                                | Store und Azure Marketplace                                    |
| tpan                           | Gibt das Ad Network (Werbenetzwerk) des Drittanbieters an                                                                                                     | Store – Nur Werbung                                               |
| customerCountry                | Land des Kunden                                                                                                                         | Store und Azure Marketplace                                    |
| CustomerCity                   | Stadt des Kunden                                                                                                                            | Store und Azure Marketplace                                    |
| customerState                  | Bundesland/Kanton des Kunden                                                                                                                           | Store und Azure Marketplace                                    |
| customerzip                    | Postleitzahl des Kunden                                                                                                                 | Store und Azure Marketplace                                    |
| purchaseTypeCode               | Ist immer leer                                                                                                                     | Incentive-Programm – CRI                                        |
| purchaseOrderType              | Ist immer leer                                                                                                                     | Incentive-Programm – CRI                                        |
| purchaseOrderCoverageStartDate | Ist immer leer                                                                                                                     | Incentive-Programm – CRI                                        |
| purchaseOrderCoverageEndDate   | Ist immer leer                                                                                                                     | Incentive-Programm – CRI                                        |
| programOfferingLevel           |                                                                                                                                          | Incentive-Programm – CRI                                        |
| Mandanten-ID                       |                                                                                                                                          | Incentive-Programme                                             |
| externalReferenceId            | Eindeutige ID für das Programm                                                                                                        | Direct Pay-Programme (Incentive und Store)                      |
| externalReferenceIdLabel       | Beschriftung der eindeutigen ID                                                                                                                  | Direct Pay-Programme (Incentive und Store)                      |

## <a name="historical-statements"></a>Historische Auszüge

![Exportieren historischer Auszüge](images/pc-export-statements.png)

Der Transaktionsverlauf vor dem 1. Juli 2019 wird gesondert behandelt. Für Auszüge werden die folgenden Felder anstelle der aktuellen Felder verwendet.

> [!NOTE]
> Der Legacy-Transaktionsverlauf verfügt über eine Spalte mit dem Namen „Reserviert“, die der Spalte „Einnahmen“ im modernen Verlauf entspricht, mit dem Unterschied, dass alle Einnahmen mit dem Status „Zahlung gesendet“ ausgeschlossen sind.

> [!NOTE]
> Filter (3M, 6M, 12M usw.) gelten nicht für den Abschnitt mit den **historischen Anweisungen** .

| Feldname              | BESCHREIBUNG                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Umsatzquelle          | Die Quelle ihres Umsatzes basierend auf dem Speicherort der Transaktion (z. b. Microsoft Store, Windows Phone Store, Windows Store 8, Werbung usw.)                  |
| Bestell-ID                | Eindeutiger Bestellungsbezeichner. Anhand dieser ID können Sie kaufbezogene Transaktionen und die entsprechenden Transaktionen, die sich nicht auf Käufe beziehen (z. B. Erstattungen, Rückvergütungen usw.), ermitteln. Beide weisen die gleiche Bestell-ID auf. Im Fall einer aufgeteilten Belastung, bei der mehrere Zahlungsmethoden für einen einzelnen Einkauf verwendet wurden, können Sie die kaufbezogenen Transaktionen verknüpfen. |
| TransactionID          | Eindeutiger Transaktionsbezeichner.                                                                                                                                          |
| Datum/Uhrzeit der Transaktion   | Das Datum und die Uhrzeit, zu der die Transaktion stattfand (UTC).                                                                                                                       |
| Übergeordnete Produkt-ID       | Eindeutige ID des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“.                                |
| Product ID              | Eindeutiger Produktbezeichner.                                                                                                                                              |
| Übergeordneter Produktname     | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.                                  |
| Produktname            | Der Name des Produkts.                                                                                                                                                    |
| Produkttyp            | Produkttyp (z. B. App, Add-On, Spiel usw.)                                                                                                                       |
| Menge                | Wenn Microsoft Store für Unternehmen die Umsatzquelle ist, stellt die Menge die Anzahl der erworbenen Lizenzen dar. Bei allen anderen Umsatzquellen ist die Menge immer 1. Hinweis: Auch wenn eine Transaktion in zwei Positionen unterteilt wird, weil zwei verschiedene Zahlungsmethoden verwendet wurden, wird für jede Position die Menge 1 angezeigt. |
| Transaktionstyp        | Transaktionstyp (z. B. Kauf, Erstattung, Stornierung, Rückbelastung usw.)                                                                                              |
| Zahlungsmethode          | Vom Kunden für die Transaktion verwendetes Zahlungsmittel (z.B. Scheckkarte, Mobile Carrier Billing, PayPal usw.)                                                               |
| Land/Region        | Land/Region, in dem die Transaktion durchgeführt wurde.                                                                                                                          |
| Lokaler Anbieter/Verkäufer | Eingetragener lokaler Anbieter/Verkäufer.                                                                                                                                        |
| Transaktionswährung    | Währung der Transaktion.                                                                                                                                            |
| Transaktionsbetrag      | Betrag der Transaktion.                                                                                                                                              |
| Bezahlte Steuern            | Betrag der abgeführten Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer).                                                                                                                  |
| Nettoeinnahmen            | Transaktionsbetrag minus bezahlter Steuern.                                                                                                                                   |
| Store-Gebühr               | Der Prozentsatz der Nettoeinnahmen, die von Microsoft als Gebühr für das Verfügbarmachen der App oder des Add-Ons im Store erhoben wird                                                      |
| App-Erlöse            | Nettoeinnahmen abzüglich der Store-Gebühr.                                                                                                                                       |
| Einbehaltene Steuern          | Höhe der einbehaltenen Einkommensteuer. (Nicht in der **reservierten** CSV-Datei enthalten.)                                                                                                |
| payment                 | App-Erlöse abzüglich aller anwendbaren Einkommenssteuereinbehalte (Betrag in Transaktionswährung) (Nicht in der **reservierten** CSV-Datei enthalten.)                               |
| Wechselkurs                 | Wechselkurs für die Umrechnung der Transaktionswährung in die Auszahlungswährung.                                                                                         |
| Zahlungswährung        | Währung der Auszahlung.                                                                                                                                       |
| Umgerechnete Zahlung       | Basierend auf dem Wechselkurs in die Auszahlungswährung umgerechneter Auszahlungsbetrag.                                                                                                         |
| Steuerzahlungsmodell         | Partei, die für das Abführen von Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer) verantwortlich ist.                                                                                                   |
| Datum/Uhrzeit der Berechtigung   | Das Datum und die Uhrzeit, zu der Transaktionserlöse auszahlungsberechtigt werden (UTC). Wenn eine Auszahlung erstellt wird, enthält diese Transaktionserlöse, die einen Qualifizierungszeitpunkt enthalten, der vor dem Erstellungsdatum der Auszahlung liegt. (Nur enthalten in der **reservierten** .csv-Datei.) |
| Charges                 | Zeigt eine Aufschlüsselung aller Gebührendetails, aggregiert in der Spalte „Transaktionsbetrag“. (Nur für Azure Marketplace; nicht enthalten in der **reservierten** .csv-Datei.) |
