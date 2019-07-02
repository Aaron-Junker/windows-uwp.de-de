---
Description: Die Auszahlungsübersicht enthält Details zu den mit Ihren Apps und Add-Ons erzielten Erlösen. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.
title: Auszahlungsübersicht
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10 UWP, Auszahlungszusammenfassung, Anweisung, Zahlungen, Einnahmen, Auszahlung, Einnahmen
ms.localizationpriority: medium
ms.openlocfilehash: 90238360ecc48beb974546dc5b49ac09c01407eb
ms.sourcegitcommit: 35a511c2b29ae3d5008612a5fc13d3eb6370d2d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67495732"
---
# <a name="payout-summary"></a>Auszahlungsübersicht

Die **auszahlungszusammenfassung** zeigt Informationen über das Geld, die Sie bei Microsoft erworben haben. Sie werden auch darüber informiert, wann Sie Zahlungen erhalten und wie hoch diese Zahlungen sind.

Wenn Sie Produkte im Azure Marketplace verkaufen, werden Ihnen in der **Auszahlungsübersicht** auch Informationen zu erfolgreichen Auszahlungen angezeigt. Weitere Informationen zur Bezahlung im Azure Marketplace finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Um für die Auszahlungen geeignet zu sein, müssen Ihre fortgesetzt erreichen die [Zahlung Schwellenwert](payment-thresholds-methods-and-timeframes.md) von 50 US-Dollar. Weitere Informationen zu den Schwellenwert für die Zahlung finden Sie auf dieser Seite, und überprüfen Sie die Vereinbarung für app-Entwickler.

## <a name="access-the-payout-summary-pages"></a>Zugriff auf den Zusammenfassungsseiten Auszahlung

So öffnen Sie eine der Auszahlungen Zusammenfassung Seiten:

1. Wählen Sie in der oberen rechten Ecke das Symbol "Money".
2. Wählen Sie Zahlungen, Transaktionsverlauf, oder Exportieren von Daten.

## <a name="payments-page"></a>Seite "Zahlungen"

Die Ergebnisse auf dieser Seite stellen dar, alle Programme, an denen Sie beteiligt. Sie können nach Teilnehmer-ID, Programm, Zahlung-ID und Earning Typ filtern. Mengen werden in US-Dollar angegeben. Der kostenpflichtige Wert wird auch in Zahlen, Währung angezeigt.

| Bereich                   | Beschreibung                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| In diesem Jahr bezahlt gesamt   | Die kombinierte Summe zahlte für Sie in diesem Jahr in US-Dollar für alle Programme.       |
| Nächste geschätzte Zahlung | Der einzelnen weiter Zahlungen an Sie (selbst wenn es gibt noch andere in Kürze verfügbar), in US-Dollar. |
| Letzte Zahlung           | Der Wert (in US-Dollar), Name des Programms und Programm Ihrer letzten Zahlung.           |
| Zahlungen von Quelle     | Die Menge von Zahlungen, die in US-Dollar, die vom Programm in den letzten 12 Monaten dargestellt.           |
| Zahlungen               | Wählen Sie bezahlte oder ausstehend, und klicken Sie dann wie gewünscht zu sortieren. Wählen Sie für zusätzliche Details zu einer bestimmten Zahlung Ansicht ein. Wählen Sie zum Herunterladen einer Kopie der Zahlung-überweisungsland Anweisung herunterladen. Beachten Sie, dass transaktionsverlaufsdaten angezeigt werden, bis zu 24 Stunden dauern können, damit Sie nicht zugeordnete Ergebnis sofort sehen können. |

Um die Daten auf dieser Seite zu exportieren, wählen Sie exportieren, und befolgen Sie dann den Anweisungen auf der Seite des Export-Daten.

## <a name="transaction-history-page"></a>Transaktion Seite "Verlauf"

Auf dieser Seite werden alle Ihre individuellen Ergebnis, einschließlich des Datums, Typ und gibt an, dass für jede angezeigt. Sie können einen Zeitraum zur Anzeige auswählen, und Sie können auch nach Registrierungs-ID, Programm, Earning-Typ, Hebel, Zahlung-ID und Status filtern. Daten sind für das aktuelle Geschäftsjahr (1. Juli – 30. Juni) und den vorherigen zwei Geschäftsjahre verfügbar.

Um eine gibt an, dass weitere Details anzuzeigen, wählen Sie den Pfeil nach unten rechts auf der Seite. Dadurch werden der Hebel Umsatzerlös und Produkt angezeigt. Wenn für eine der Grund diese Daten nicht verfügbar sind, aber Sie benötigen Zugriff auf diese, wenden Sie sich an [unterstützen](https://developer.microsoft.com/en-us/windows/support)]. Wenn der gibt an, dass das Ergebnis eine Anpassung, und nicht für eine Transaktion ist, werden die Produkt-Felder nicht angezeigt.

Um die Transaktion Daten auf dieser Seite zu exportieren, wählen Sie exportieren, und befolgen Sie dann den Anweisungen auf der Seite des Export-Daten. Dateien, die auf der Seite Transaktionsverlauf exportiert Anzeigen von Daten in der Transaktion Währungen Ergebnis in Transaktion Währung und US-Dollar angezeigt, und der kostenpflichtige Wert im Zahlen, Währung.

## <a name="payment-status"></a>Zahlungsstatus

| Gibt an, dass status           | Grund                                                                                                                                      | Partner-Aktion erforderlich?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Nicht verarbeitet              | Der gibt an, dass ist berechtigt, für die Zahlung. Es bleibt in diesem Status für einen Zeitraum Kühlung wie in der Programmhandbuch für die anreizprogramm definiert. | Nein                                                         |
| Bevorstehende                 | Zahlung-Reihenfolge, die ausstehende internen Prüfungen generiert wird, bevor Zahlung verarbeitet wird.                                                               | Nein                                                         |
| Ausstehende Steuer-Rechnung      | Ihre Rechnung Tax ist unvollständig oder ungültig.                                                                                                  | Müssen Sie Ihrer Steuer-Rechnung zu aktualisieren, bevor Sie bezahlt werden können |
| Während der Überprüfung abgelehnt   | Die Zahlung wurde während der Überprüfung abgelehnt.                                                                                                     | Wenden Sie sich an [Microsoft-Support](https://developer.microsoft.com/en-us/windows/support) Details                      |
| Failed                   | Die Zahlung konnte aufgrund eines Fehlers der Microsoft-System nicht.                                                                                         | Wenden Sie sich an [Microsoft-Support](https://developer.microsoft.com/en-us/windows/support) Details                      |
| In Bearbeitung              | Die Zahlung wird ausgeführt.                                                                                                                 | Nein                                                         |
| Falsche Zahlung        | Die Zahlung abzusichern wird ausgeführt.                                                                                                       | Nein                                                         |
| Gesendet                     | Die Zahlung wurde an Ihre Bank gesendet.                                                                                                     | Nein                                                         |
| Eine erneute Verarbeitung             | Die Zahlung ist ein Microsoft-System-Fehler aufgetreten, und es wird erneut verarbeitet wird.                                                                  | Nein                                                         |
| Reserviert                 | Die Zahlung von Ihrer Bank umgekehrt wurde und erneut in den nächsten zahlungszyklus gesendet werden.                                                     | Nein                                                         |
| Steuer-Rechnung abgelehnt     | Ihre Rechnung Tax wurde während der Überprüfung abgelehnt. Alle ausstehenden Zahlungen werden angehalten, bis die Steuer Rechnung Überprüfung abgeschlossen ist.                 | Wenden Sie sich an [Microsoft-Support](https://developer.microsoft.com/en-us/windows/support) Details                      |
| Steuer-Rechnung geprüft | Ihre Rechnung steuern werden überprüft. Ihre Zahlung wird freigegeben, nachdem die Steuer-Rechnung genehmigt wurde.                                   | Nein                                                         |
| Abgelehnt                 | Die Zahlung wurde von Ihrer Bank abgelehnt.                                                                                                      | Weitere Informationen erhalten Sie von Ihrer Bank.                             |

## <a name="export-data-page"></a>Exportieren Sie die Seite "Daten"

Befolgen Sie die Anweisungen auf dieser Seite können Sie die Daten exportieren.

Hinweise:

- Wenn Sie diese Seite entweder die Zahlungen oder die Transaktion Seite "Verlauf" zugreifen, werden Ihre Filter nicht übertragen. Sie müssen sie auf der Seite des Export-Daten zu wiederholen.
- Die Export Data-Seite wird nicht eigenständig aktualisiert. Sie müssen die Seite manuell, um die neuesten Daten anzeigen zu aktualisieren.
- Der Filter kann keine Daten verfügbar-Fehler führen. Dies bedeutet wahrscheinlich, Sie bleibt die Standardeinstellung auf drei Monate ausgewählten Zeitraum und dann eine Zahlung-ID aus, die außerhalb dieses Zeitraums ist verdienen ausgewählt haben. Erweitern Sie Ihre Zeitraum aus, und versuchen Sie es erneut.

## <a name="payment-download-export"></a>Zahlung Download exportieren

Mit dieser Option wird einen Download der Zahlungen, die Sie in Ihrer Bank für ein bestimmtes Programm, und die zugehörigen Gebühr empfangen und ertragreiche Menge aggregiert. Dieser Bericht ist für viele Partner Center-Programme verwendet, daher einige Spalten möglicherweise nicht anwendbar auf den Bericht. Diese Spalten werden unten markiert.

| Spaltenname              | Beschreibung                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantID            | Die primäre Identität des Partners gibt an, dass im Rahmen des Programms                                                                           |
| participantIDType        | In der Regel Programm-Id für Incentive Programme und Verkäufer-ID für Store-Programme                                                              |
| participantName          | Name des Partners ertragreiche                                                                                                             |
| programName              | Incentive/Geschäftsname-Programm                                                                                                            |
| Ertragswert                   | Menge, die in der Zahlen, Währung für diese Anwendung/ParticipantID erworben                                                                     |
| earnedUSD                | Menge, die durch die Anwendung/Teilnehmer-ID, in US-Dollar                                                                                    |
| withheldTax              | In der Zahlen, Währung für die Anwendung/ParticipantID Abgaben Steuerbetrag                                                             |
| salesTax                 | Gesamtmenge der Mehrwertsteuer in die Währung bezahlen, für die Anwendung/participantID                                                          |
| totalPayment             | Gesamtbetrag der Zahlung in der lokalen Währung Quellensteuer ausschließen und die Mehrwertsteuer (falls zutreffend) für die Anwendung/participantID |
| currencyCode             | Der Code für Zahlen, Währung                                                                                                                    |
| PaymentMethod            | Die Methode zum Bezahlen des Partners (elektronische Überweisung, Gutschrift)                                                              |
| paymentID                | Eindeutiger Bezeichner für die Zahlung. Diese Zahl wird in der Regel in Ihrem Kontoauszug angezeigt.                                               |
| paymentStatus            | Zahlungsstatus                                                                                                                          |
| paymentStatusDescription | Beschreibung des Zahlungsstatus                                                                                                  |
| paymentDate              | Der Zahlung wurde von Microsoft gesendet.                                                                                                    |

## <a name="transaction-history-download-export"></a>Transaktion Verlauf Download exportieren

Mit dieser Option wird einen Download jedes ertragreiche Zeilenelements in die Transaktion-Verlaufsseite angezeigten gibt an, dass Typ, Datum, zugeordnete Transaktion Menge, Kunde, Produkt und andere transaktionale Details für Ihre Programme.

| Spaltenname                    | Beschreibung                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningId                      | Eindeutiger Bezeichner für jede gibt an, dass                                                                                                       |
| participantId                  | Die primäre Identität des Partners gibt an, dass im Rahmen des Programms                                                                            |
| participantIdType              | ID des Verkäufers                                                                                                                                |
| participantName                | Name des Partners ertragreiche                                                                                                              |
| partnerCountryCode             | Standort/Land des Partners, direkt                                                                                                  |
| programName                    | Incentive/Geschäftsname-Programm                                                                                                             |
| transactionId                  | Eindeutiger Bezeichner für die Transaktion                                                                                                    |
| transactionCurrency            | Währung, in dem die ursprüngliche Transaktion für den Kunden aufgetreten ist.                                                                             |
| transactionDate                | Datum der Transaktion. Nützlich für Programme, in denen viele Transaktionen, die eine gibt an, dass beitragen                                           |
| transactionExchangeRate        | Wechselkursdatum verwendet, um die entsprechende USD-Betrag anzuzeigen.                                                                             |
| transactionAmount              | Betrag in der ursprünglichen Transaktion Währung, die basierend auf der gibt an, dass generiert wird                                              |
| transactionAmountUSD           | Betrag in US-Dollar                                                                                                                |
| Hebel                          | Gibt die Geschäftsregel für die gibt an, dass an                                                                                                  |
| earningRate                    | Incentive Tarif auf Transaktionsbetrags verdienen generieren                                                                      |
| quantity                       | Variiert basierend auf Programm. Berechnete Menge für transaktionale Programme                                                            |
| earningType                    | Gibt an, ob diese Gebühr "," Rebate "," Coop "," Selling usw.                                                                                          |
| earningAmount                  | Gibt an, dass Betrag in der ursprünglichen Transaktion Währung                                                                                      |
| earningAmountUSD               | Gibt an, dass der Umfang in US-Dollar                                                                                                                    |
| earningDate                    | Datum der gibt an, dass                                                                                                                      |
| calculationDate                | Datum, an der gibt an, dass im System berechnet wurde                                                                                            |
| earningExchangeRate            | Wechselkurs verwendet, um die entsprechende USD-Betrag anzuzeigen.                                                                                  |
| exchangeRateDate               | Zum Berechnen von EarningAmount USD von Wechselkursdatum                                                                                   |
| claimId                        | Wird immer leer sein.                                                                                                                     |
| paymentId                      | Eindeutiger Bezeichner für die Zahlung. Diese Zahl wird in Ihrem Konto in der Regel angezeigt.                                                 |
| paymentStatus                  | Zahlungsstatus                                                                                                                           |
| paymentStatusDescription       | Beschreibung des Zahlungsstatus                                                                                                   |
| customerId                     | Wird immer leer sein.                                                                                                                     |
| customerName                   | Wird immer leer sein.                                                                                                                     |
| PartNumber                     | Wird immer leer sein.                                                                                                                     |
| productName                    | Mit der Transaktion verknüpften Produktnamen                                                                                                       |
| productId                      | Eindeutige Produkt-ID                                                                                                                |
| parentProductId                | Eindeutige übergeordnete Produkt-ID. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet die ID des übergeordneten Produkts „Produkt-ID“. |
| parentProductName              | Name des übergeordneten Produkts. Hinweis: Wenn für die Transaktion kein übergeordnetes Produkt vorhanden ist, lautet der Name des übergeordneten Produkts „Produktname“.   |
| productType                    | Art des Produkts (z. B. App, Add-On, Spiel usw.)                                                                                        |
| invoiceNumber                  | Wird immer leer sein.                                                                                                                     |
| subscriptionId                 | Wird immer leer sein.                                                                                                                     |
| subscriptionStartDate          | Wird immer leer sein.                                                                                                                     |
| subscriptionEndDate            | Wird immer leer sein.                                                                                                                     |
| resellerId                     | Wird immer leer sein.                                                                                                                     |
| resellerName                   | Wird immer leer sein.                                                                                                                     |
| distributorId                  | Wird immer leer sein.                                                                                                                     |
| distributorName                | Wird immer leer sein.                                                                                                                     |
| ' AgreementNumber '                | Wird immer leer sein.                                                                                                                     |
| agreementStartDate             | Wird immer leer sein.                                                                                                                     |
| agreementEndDate               | Wird immer leer sein.                                                                                                                     |
| arbeitsauslastung                       | Wird immer leer sein.                                                                                                                     |
| transactionType                | Art der Transaktion (z. B. Einkauf, Erstattung, Rückbuchung, Ausgleich usw.)                                                               |
| localProviderSeller            | Eingetragener lokaler Anbieter/Verkäufer                                                                                                          |
| taxRemitted                    | Höhe der bezahlten Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer).                                                                                   |
| taxRemitModel                  | Für die Überweisung von Steuern (Verkaufssteuer, Gebrauchssteuer, Umsatzsteuer oder Waren-/Dienstleistungssteuer) zuständige Partei.                                                                    |
| storeFee                       | Der Betrag, der von Microsoft als eine Gebühr für das Verfügbarmachen der app oder ein Add-on in der Store beibehalten wird.                                           |
| transactionPaymentMethod       | Kundenzahlungsmittel, das für die Transaktion verwendet wird (z. B. Kreditkarte, Mobilfunkanbieterrechnung, PayPal usw.)                                |
| tpan                           | Gibt das Drittanbieter-Ad-Netzwerk                                                                                                     |
| purchaseTypeCode               | Wird immer leer sein.                                                                                                                     |
| purchaseOrderType              | Wird immer leer sein.                                                                                                                     |
| purchaseOrderCoverageStartDate | Wird immer leer sein.                                                                                                                     |
| purchaseOrderCoverageEndDate   | Wird immer leer sein.                                                                                                                     |
| externalReferenceId            | Wird immer leer sein.                                                                                                                     |
| externalReferenceIdLabel       | Wird immer leer sein.                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Auszahlungen-Anweisung Download Export (Vorgängerversion)

Für einen begrenzten Zeitraum in der alten Auszahlung Seite "Zusammenfassung" werden Auszahlung Anweisungen zum Herunterladen zur Verfügung. Dieser Bericht enthält die folgenden Felder.

> [!NOTE]
> Ältere Transaktionsverlauf verfügt über eine Spalte namens "Reserviert" entspricht der Spalte "Ergebnis" in der modernen Geschichte, außer dass es alle Ergebnis mit dem Status schließt = "Bezahlt".

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
