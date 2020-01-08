---
Description: Die Auszahlungsübersicht enthält Details zu den mit Ihren Apps und Add-Ons erzielten Erlösen. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.
title: Auszahlungsübersicht
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: Windows 10 UWP, Auszahlungszusammenfassung, Anweisung, Zahlungen, Einnahmen, Auszahlung, Einnahmen
ms.localizationpriority: medium
ms.openlocfilehash: aff36ace40317ff0b2ff54a8ca75381fe2c24a4e
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684994"
---
# <a name="payout-summary"></a>Auszahlungsübersicht

Die **Zahlungsübersicht** enthält Einzelheiten zu dem Betrag, den Sie mit Ihrem Angebot eingenommen haben. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.

Wenn Sie Produkte im Azure Marketplace verkaufen, werden Ihnen in der **Auszahlungsübersicht** auch Informationen zu erfolgreichen Auszahlungen angezeigt. Weitere Informationen zur Bezahlung im Azure Marketplace finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://docs.microsoft.com/legal/marketplace/participation-policy) und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Qualifiziert für eine Auszahlung sind Beträge, die den [Zahlungsschwellenwert](payment-thresholds-methods-and-timeframes.md) von 50 US-Dollar erreichen. Weitere Informationen zum Schwellenwert für die Zahlung finden Sie auf dieser Seite, und lesen Sie die App-Entwickler Vereinbarung.

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

Wählen Sie den Pfeil nach unten auf der rechten Seite der Seite aus, um weitere Details zu einem Erwerbs Bereich anzuzeigen. Dadurch werden der Hebel, der Umsatz und das Produkt angezeigt. Wenn eine dieser Daten aus irgendeinem Grund nicht verfügbar ist, Sie aber darauf zugreifen müssen, wenden Sie sich an den [Support](https://developer.microsoft.com/windows/support). Wenn das Ergebnis einer Anpassung und nicht einer Transaktion entspricht, werden die Produktfelder nicht angezeigt.

Wählen Sie zum Exportieren der Transaktionsdaten auf dieser Seite die Option Exportieren aus, und befolgen Sie dann die Anweisungen auf der Seite Daten exportieren. Dateien, die aus der Seite Transaktionsverlauf exportiert wurden, zeigen Daten in Transaktionswährung, Einnahmen in Transaktionswährung und US-Dollar sowie den bezahlten Wert in Zahlung in Währung an.

## <a name="payment-status"></a>Zahlungsstatus

| Erwerbsstatus           | Grund                                                                                                                                      | Partner Aktion erforderlich?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Nicht verarbeitet              | Die Einnahme ist für eine Auszahlung qualifiziert. Sie behält diesen Status für die Dauer der sogenannten „Cooling Period“, wie sie im Programmhandbuch für das Incentive-Programm definiert ist. | Nein                                                         |
| Anstehend                 | Der Zahlungsauftrag hat ausstehende interne Überprüfungen generiert, bevor die Zahlung verarbeitet wird.                                                               | Nein                                                         |
| Ausstehende Steuerrechnung      | Ihre Steuerrechnung ist unvollständig oder ungültig.                                                                                                  | Sie müssen ihre Steuerrechnung aktualisieren, bevor Sie bezahlen können. |
| Ablehnung während der Überprüfung   | Die Zahlung wurde während der Überprüfung abgelehnt.                                                                                                     | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/windows/support) .                      |
| Failed                   | Die Zahlung ist aufgrund eines Microsoft-Systemfehlers fehlgeschlagen.                                                                                         | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/windows/support) .                      |
| In Bearbeitung              | Die Zahlung wird ausgeführt.                                                                                                                 | Nein                                                         |
| Falsche Zahlung        | Der Zahlungsvorgang wird wiederholt.                                                                                                       | Nein                                                         |
| Sent                     | Die Zahlung wurde an Ihre Bank gesendet.                                                                                                     | Nein                                                         |
| Reprocessing             | Bei der Zahlung ist ein Microsoft-Systemfehler aufgetreten, der erneut verarbeitet wird.                                                                  | Nein                                                         |
| Reserviert                 | Die Zahlung wurde von Ihrer Bank rückgängig gemacht und wird im nächsten Zahlungszeitraum erneut gesendet.                                                     | Nein                                                         |
| Finanzrechnung abgelehnt     | Ihre Steuerrechnung wurde während der Überprüfung abgelehnt. Alle ausstehenden Zahlungen werden angehalten, bis die Überprüfung der Steuerrechnung abgeschlossen ist.                 | Weitere Informationen erhalten Sie vom [Microsoft Support](https://developer.microsoft.com/windows/support) .                      |
| Steuerrechnung unter Review | Ihre Steuerrechnung wird geprüft. Ihre Zahlung wird nach Genehmigung der Steuerrechnung freigegeben.                                   | Nein                                                         |
| Rejected                 | Die Zahlung wurde von Ihrer Bank abgelehnt.                                                                                                      | Weitere Informationen erhalten Sie von der Bank.                             |

## <a name="export-data-page"></a>Seite "Daten exportieren"

Befolgen Sie die Anweisungen auf dieser Seite, um die gewünschten Daten zu exportieren.

Anmerkungen:

- Die Seite "Daten exportieren" wird nicht eigenständig aktualisiert. Möglicherweise müssen Sie die Seite manuell aktualisieren, um die neuesten Daten anzuzeigen.
- Der Filter führt möglicherweise zu einem Fehler, der nicht verfügbar ist. Dies bedeutet wahrscheinlich, dass Sie den Standard Zeitraum in drei Monaten ausgewählt haben und dann eine Zahlungs-ID aus einem Verdienst außerhalb dieses Zeitraums ausgewählt haben. Erweitern Sie den Zeitraum, und versuchen Sie es noch mal.

## <a name="payment-download-export"></a>Zahlungs Download Export

Mit dieser Option können Sie die Zahlungen, die Sie in Ihrer Bank für ein bestimmtes Programm erhalten haben, sowie die zugehörigen Steuern und den aggregierten Erwerbs Betrag herunterladen. Dieser Bericht wird für viele Partner Center-Programme verwendet, sodass einige Spalten für den Bericht möglicherweise nicht zutreffend sind. Diese Spalten sind unten markiert.

| Name der Spalte              | Beschreibung                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt                                                                             |
| participantidtype        | Normalerweise Programm-ID für Incentive-Programme und Verkäufer-ID für Store-Programme                                                                |
| participantName          | Name des Partners, der die Einnahme erzielt                                                                                                               |
| programName              | Name des Incentive-/Store-Programms                                                                                                              |
| erzielten                   | Betrag, der in der Zahlen-zu-Währung für dieses Programm/diese Element-ID verdient wurde                                                                       |
| earnetzdusd                | Betrag für die Programm/Teilnehmer-ID in USD                                                                                      |
| withheldtax              | Betrag der in der Zahlen-zu-Währung für das Programm/participantid gehaltenen Steuerelement                                                               |
| salesTax                 | Gesamtbetrag der Umsatzsteuer in der Zahlen-zu-Währung für das Programm/die participantid (gilt nur für Incentive-Programme)                   |
| servicefeetax            | Gesamtmenge der servicefeetax in "Pay-to-Currency" für das Programm/die "participantid" (gilt nur für Store-Programme und Azure Marketplace) |
| totalpayment             | Gesamtzahlung in der lokalen Währung ohne die zurück Haltungs Steuern und einschließlich der Umsatzsteuer (falls zutreffend) für die Programm/participantid   |
| currencyCode             | Bezahlen an Währungscode                                                                                                                      |
| PaymentMethod            | Die Methode, mit der der Partner bezahlt wird, z. b. elektronischer Bank Transfer, Bonitäts Hinweis                                                             |
| PaymentID                | Eindeutige ID für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar. (gilt nur für SAP-Zahlungen)              |
| paymentStatus            | Zahlungsstatus                                                                                                                            |
| paymentStatusDescription | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                    |
| paymentdate              | Die Datums Zahlung wurde von Microsoft gesendet.                                                                                                      |

## <a name="transaction-history-download-export"></a>Download des exportierten Transaktionsverlaufs

Mit dieser Option können Sie jedes auf der Seite Transaktionsverlauf gelaufene Element auf der Seite Transaktionsverlauf herunterladen, den Typ, das Datum, die zugehörige Transaktions Menge, den Kunden, das Produkt und andere Transaktionsdetails, die für Ihre Programme gelten, erwerben.

| Name der Spalte                    | Beschreibung                                                                                                                              | Anwendbarkeit für Incentives/Store/Azure Marketplace           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Eindeutiger Bezeichner für jede Einnahme                                                                                                       | Alle                                                            |
| participantID                  | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt                                                                            | Alle                                                            |
| participantIdType              | Meistens eine Programm-ID für Incentive-Programme und Verkäufer, WENN es sich um Store-Programme und Azure Marketplace handelt                                          | Alle                                                            |
| participantName                | Name des Partners, der die Einnahme erzielt                                                                                                              | Alle                                                            |
| partnerCountryCode             | Standort/Land des Partners, der die Einnahme erzielt                                                                                                  | Alle                                                            |
| programName                    | Name des Incentive-/Store-Programms                                                                                                             | Alle                                                            |
| transactionId                  | Eindeutige ID für die Transaktion                                                                                                    | Alle                                                            |
| transactionCurrency            | Währung, in der die ursprüngliche Kunden Transaktion aufgetreten ist (Dies ist keine Währung für den Partnerstandort)                                     | Alle                                                            |
| transactionDate                | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einer Einnahme beitragen                                           | Alle                                                            |
| transactionExchangeRate        | Wechselkurs Datum zum Anzeigen der entsprechenden Transaktion in USD                                                                 | Alle                                                            |
| transactionAmount              | Transaktionsbetrag in der ursprünglichen Transaktionswährung, auf deren Basis die Einnahme generiert wird                                              | Alle                                                            |
| transactionAmountUSD           | Transaktionsbetrag in US-Dollar                                                                                                                | Alle                                                            |
| lever                          | Geschäftsregel für die Einnahme                                                                                                  | Alle                                                            |
| earningrate                    | Auf den Transaktionsbetrag angewendete Incentive-Rate zum Generieren einer Einnahme                                                                      | Alle                                                            |
| quantity                       | Abhängig vom Programm. Gibt die in Rechnung gestellte Menge für Transaktionsprogramme an                                                            | Alle                                                            |
| quantityType                   | Gibt den Typ der Menge an, z. b. berechnete Menge, Mau                                                                                     | Alle                                                            |
| earningType                    | Gibt an, ob es sich um eine Gebühr, einen Rabatt, Coop, Verkauf usw. handelt.                                                                                          | Alle                                                            |
| earningamount                  | Einnahmebetrag in der ursprünglichen Transaktionswährung                                                                                      | Alle                                                            |
| earningAmountUSD               | Einnahmebetrag in US-Dollar                                                                                                                    | Alle                                                            |
| earningDate                    | Datum der Einnahme                                                                                                                      | Alle                                                            |
| calculationDate                | Datum der Berechnung der Einnahme im System                                                                                            | Alle                                                            |
| earningExchangeRate            | Wechselkurs zum Anzeigen des entsprechenden Betrags in US-Dollar                                                                                  | Alle                                                            |
| exchangeRateDate               | Datum des Wechselkurses, der zum Berechnen des Einnahmebetrags in US-Dollar verwendet wurde                                                                                   | Alle                                                            |
| paymentAmountWOTax             | Einnahmebetrag (ohne Steuern) in der Auszahlungswährung nur für Zahlungen mit dem Status „Gesendet“                                                                 | Alle                                                            |
| paymentCurrency                | Vom Partner im Zahlungsprofil ausgewählte Auszahlungswährung. Wird nur für gesendete Zahlungen angezeigt                                                   | Alle                                                            |
| paymentExchangeRate            | Wechselkurs zur Berechnung von „paymentAmountWOTax“ in der Auszahlungswährung mithilfe von „ExchangeRateDate“                                            | Alle                                                            |
| claimid                        | Eindeutiger Bezeichner für Anspruch                                                                                                              | Incentives: nur einige Programme                                |
| planId                         | Eindeutiger Bezeichner für den Plan                                                                                                               | Incentives: nur einige Programme                                |
| paymentId                      | Eindeutige ID für die Zahlung. Diese Zahl ist normalerweise in der Bank-Anweisung sichtbar.                                                 | Nur SAP-Zahlungen                                              |
| paymentStatus                  | Zahlungsstatus                                                                                                                           | Alle                                                            |
| paymentStatusDescription       | Benutzerfreundliche Beschreibung des Zahlungsstatus                                                                                                   | Alle                                                            |
| customerId                     | Ist immer leer                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| customerName                   | Ist immer leer                                                                                                                     | Nur Incentive-Programme (Ausnahme: OEM) und Azure Marketplace |
| partNumber                     | Ist immer leer                                                                                                                     | Einige Incentive-und Store-Programme und Azure Marketplace        |
| productName                    | Mit der Transaktion verknüpfter Produktname                                                                                                       | Alle                                                            |
| Produkt-ID                      | Eindeutige Produkt-ID                                                                                                                | Speichern und Azure Marketplace                                    |
| parentProductId                | Eindeutige übergeordnete Produkt-ID. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“. | Speichern und Azure Marketplace                                    |
| parentProductName              | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.   | Speichern und Azure Marketplace                                    |
| Produkttyp                    | Art des Produkts (z. B. App, Add-On, Spiel usw.)                                                                                        | Speichern und Azure Marketplace                                    |
| InvoiceNumber                  | Rechnungsnummer (gilt nur für EA)                                                                                                  | Incentive und Azure Marketplace: nur einige Programme           |
| subscriptionId                 | Dem Kunden zugeordneter Abonnement Bezeichner                                                                                         | Incentive: nur einige Programme                                 |
| Abonnement StartDate          | Startdatum des Abonnements                                                                                                                  | Incentive: nur einige Programme                                 |
| Abonnement Enddatum            | Enddatum des Abonnements                                                                                                                    | Incentive: nur einige Programme                                 |
| resellerId                     | ID des Handelspartners                                                                                                                      | Incentive: nur einige Programme                                 |
| resellerName                   | Name des Wiederverkäufers                                                                                                                            | Incentive: nur einige Programme                                 |
| verteilbare d                  | Verteiler Bezeichner                                                                                                                   | Incentive: nur einige Programme                                 |
| Distributor Name                | Verteiler Name                                                                                                                         | Incentive: nur einige Programme                                 |
| AgreementNumber                | Vertragsnummer                                                                                                                         | Incentive: nur einige Programme                                 |
| agreementstartdate             | Startdatum des Vertrags                                                                                                                     | Incentive: nur einige Programme                                 |
| agreementenddate               | Enddatum des Vertrags                                                                                                                       | Incentive: nur einige Programme                                 |
| workload                       | Arbeitsauslastung                                                                                                                                 | Incentive: nur einige Programme                                 |
| transactionType                | Art der Transaktion (z. B. Einkauf, Erstattung, Rückbuchung, Ausgleich usw.)                                                               | Speichern und Azure Marketplace                                    |
| localProviderSeller            | Eingetragener lokaler Anbieter/Verkäufer                                                                                                          | Nur speichern                                                     |
| taxRemitted                    | Höhe der bezahlten Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer).                                                                                   | Speichern und Azure Marketplace                                    |
| taxRemitModel                  | Für die Überweisung von Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer) zuständige Partei.                                                                    | Nur speichern                                                     |
| storeFee                       | Der von Microsoft als Gebühr für die Bereitstellung der App oder des Add-Ins im Store einbehaltene Betrag.                                           | Nur speichern                                                     |
| transactionPaymentMethod       | Kundenzahlungsmittel, das für die Transaktion verwendet wird (z. B. Kreditkarte, Mobilfunkanbieterrechnung, PayPal usw.)                                | Speichern und Azure Marketplace                                    |
| tpan                           | Gibt das Ad Network (Werbenetzwerk) des Drittanbieters an                                                                                                     | Nur Store-ADS                                               |
| customerCountry                | Land des Kunden                                                                                                                         | Speichern und Azure Marketplace                                    |
| CustomerCity                   | Stadt des Kunden                                                                                                                            | Speichern und Azure Marketplace                                    |
| customerState                  | Bundesland/Kanton des Kunden                                                                                                                           | Speichern und Azure Marketplace                                    |
| customerzip                    | Postleitzahl des Kunden                                                                                                                 | Speichern und Azure Marketplace                                    |
| purchasetypeer Code               | Ist immer leer                                                                                                                     | Incentive Program-CRI                                        |
| PurchaseOrderType              | Ist immer leer                                                                                                                     | Incentive Program-CRI                                        |
| purchaseordercoveragestartdate | Ist immer leer                                                                                                                     | Incentive Program-CRI                                        |
| purchaseordercoverageenddate   | Ist immer leer                                                                                                                     | Incentive Program-CRI                                        |
| programufferinglevel           |                                                                                                                                          | Incentive Program-CRI                                        |
| Mandanten-ID                       |                                                                                                                                          | Incentive-Programme                                             |
| externalReferenceId            | Eindeutige ID für das Programm                                                                                                        | Direct Pay-Programme (Incentive und Store)                      |
| externalReferenceIdLabel       | Beschriftung der eindeutigen ID                                                                                                                  | Direct Pay-Programme (Incentive und Store)                      |
