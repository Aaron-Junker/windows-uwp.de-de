---
Description: Um Geld von App-Verkäufen in der Microsoft Store zu erhalten, müssen Sie Ihr Auszahlungs Konto einrichten und die erforderlichen Steuerformulare ausfüllen.
title: Einrichten von Auszahlungskonten und Steuerformularen.
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1827f24e467c113034c5d0303aaebce0e603da2a
ms.sourcegitcommit: 68121f21c899975f3634456a651ae8e1e53c19f7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70841848"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Einrichten von Auszahlungskonten und Steuerformularen.

Um Geld von App-Verkäufen in der Microsoft Store zu erhalten, müssen Sie Ihr Auszahlungs Konto einrichten und die erforderlichen Steuerformulare im [Partner Center](https://partner.microsoft.com/dashboard)ausfüllen.

Falls Sie ausschließlich kostenlose Apps anbieten möchten (und keine In-App-Einkäufe bereitstellen oder Microsoft Advertising verwenden möchten), brauchen Sie kein Auszahlungskonto einzurichten oder Steuerformulare auszufüllen. Wenn Sie Ihre Meinung später ändern und entscheiden, ob Sie Apps (oder Add-ons) verkaufen möchten, können Sie Ihr Auszahlungs Konto einrichten und die Steuerformulare zu diesem Zeitpunkt ausfüllen. Kostenpflichtige Apps und Add-Ons können erst übermittelt werden, wenn Sie Ihr Auszahlungskonto eingerichtet und Ihr Steuerprofil ausgefüllt haben.

> [!NOTE]
> In [bestimmten Märkten](account-types-locations-and-fees.md#developer-account-and-app-submission-markets) können Entwickler nur kostenlose Apps übermitteln. Wenn Ihr Konto in einem dieser Märkte registriert ist, steht die Option zum Einrichten eines Auszahlungskontos nicht zur Verfügung.

Nachdem Sie [Ihr Entwicklerkonto eingerichtet](opening-a-developer-account.md)haben, müssen Sie zwei Dinge tun, bevor Sie Apps (oder Add-ons) im Microsoft Store verkaufen können:

- [Ausfüllen der Steuerformulare](#tax-forms)
- [Einrichten Ihres Auszahlungs Kontos](#payout-account)

> [!NOTE]
> Ausführliche Informationen dazu, wie und wann Sie das mit den Apps verdiente Geld erhalten, finden Sie unter [Regelung der Bezahlung](getting-paid-apps.md).

## <a name="tax-forms"></a>Steuerformulare

### <a name="filling-out-your-tax-forms"></a>Ausfüllen von Steuerformularen

Zunächst müssen Sie ein Steuer Profil erstellen und es den Programmen zuweisen, an denen Sie teilnehmen. Sie können Ihr *Steuer Profil* für die Microsoft Store erstellen, indem Sie die folgenden Schritte ausführen:

- Angeben des Wohnsitzlandes und der Staatsangehörigkeit
- Ausfüllen der entsprechenden Steuerformulare

Sie können Ihre Steuerformulare im Partner Center elektronisch austauschen und übermitteln. in den meisten Fällen müssen Sie keine Formulare drucken und per e-Mail versenden.

> [!IMPORTANT]
> In den verschiedenen Ländern und Regionen gelten unterschiedliche Steuergesetze. Die genaue Höhe der zu zahlenden Steuern hängt von den Ländern und Regionen ab, in denen Sie Ihre Apps verkaufen. In der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) sehen Sie, in welchen Ländern Microsoft für Sie Verkaufs- und Nutzungssteuern überweist. In anderen Ländern müssen Sie u. U. je nachdem, wo Sie registriert sind, Verkaufs- und Nutzungssteuern direkt an die örtliche Steuerbehörde überweisen. Des Weiteren müssen die Erlöse aus Ihren App-Verkäufen unter Umständen als Einkommen versteuert werden. Wir empfehlen Ihnen dringend, die relevante Autorität für Ihr Land oder Ihre Region zu kontaktieren, die Ihnen am besten helfen kann, die richtigen Steuerinformationen für Ihre Microsoft Store-Entwickler Aktivitäten zu ermitteln.

1. Wählen Sie in [Partner Center](https://partner.microsoft.com/dashboard)das Symbol " **Kontoeinstellungen** " in der oberen rechten Ecke aus, und wählen Sie dann " **Entwicklereinstellungen**" aus.
2. Wählen Sie im linken Navigationsmenü die Option **Auszahlung und Steuern**aus, und wählen Sie dann **Auszahlungs-und steuerzuweisungen**aus.

    ![Zuweisung von Auszahlungs-und Steuer Profilen](images/payout-tax-profile-assignment.png)

3. Wählen Sie die Kombination aus Programm-und Verkäufer-ID aus, für die Sie Steuerinformationen konfigurieren möchten.

    ![Auszahlung wählen Sie Verkäufer-ID.](images/payout-select-seller-id.png)

4. Wenn Sie ein vorhandenes Steuer Profil verwenden möchten, wählen Sie es aus der Dropdown Liste aus. Wählen Sie andernfalls **Neues Profil erstellen** aus, und klicken Sie auf **senden**. Sie werden auf die Seite Steuer profile gelangen.
5. Klicken Sie auf die Schaltfläche **Bearbeiten** , um die Steuerinformationen zu bearbeiten.
6. Aktivieren Sie das entsprechende Optionsfeld, und wählen Sie Ihr Land aus, wenn Sie dazu aufgefordert werden Dieser Schritt bestimmt die Microsoft-Geschäftseinheit, die verwendet wird, um die Auszahlungen Ihres Kontos vorzunehmen.

    ![Auszahlung wählen Sie steuerland aus.](images/payout-select-tax-country.png)

7. Abhängig von Ihrer Auswahl in Schritt 6 werden Sie aufgefordert, für Ihr Land erforderliche Steuerinformationen anzugeben.

> [!NOTE]
> Unabhängig von Ihrem Wohnsitzland oder ihrer Bürgerschaft müssen Sie USA Steuerformulare ausfüllen, um apps oder Add-ons über die Microsoft Store zu verkaufen. Entwickler, die bestimmte Kriterien für den Wohnsitz USA erfüllen, müssen ein IRS W-9-Formular ausfüllen. Entwickler außerhalb der USA müssen ein IRS W-8-Formular ausfüllen. Sie können diese Formulare online ausfüllen, wenn Sie Ihr Steuerprofil angeben.

### <a name="withholding-rates"></a>Quellensteuersätze

Die von Ihnen in den Steuerformularen übermittelten Informationen sind ausschlaggebend für die anwendbaren Quellensteuersätze. Der Quellensteuersatz bezieht sich nur auf Verkäufe in die USA. Verkäufe in Länder außer die USA unterliegen nicht der Quellensteuer. Die Quellensteuersätze können variieren, aber für die meisten außerhalb der USA registrierten Entwickler beträgt der Satz standardmäßig 30 %. Sie können diesen Satz senken, wenn Ihr Land Einkommenssteuervereinbarungen mit den USA abgeschlossen hat.

### <a name="tax-treaty-benefits"></a>Vorteile des Steuerabkommens

Wenn Sie nicht in den USA ansässig sind, können Sie unter Umständen von den Vorteilen des Steuerabkommens profitieren. Diese Vorteile sind von Land zu Land unterschiedlich und können es Ihnen ermöglichen, die Menge der Steuern zu verringern, die die Microsoft Store hat. Sie müssen Teil II des W-8BEN-Formulars ausfüllen, um die Vorteile eines Steuerabkommens in Anspruch nehmen zu können. Wir empfehlen Ihnen, sich bei den zuständigen Stellen in Ihrem Land oder Ihrer Region zu erkundigen, ob diese Vorteile auf Sie zutreffen.

> [!NOTE]
> Eine US-amerikanische Steueridentifikationsnummer (Individual Taxpayer Identification Number, ITIN) ist nicht erforderlich, um Zahlungen von Microsoft zu erhalten oder die Vorteile eines Steuerabkommens in Anspruch zu nehmen.

## <a name="payout-account"></a>Auszahlungskonto

Bei einem Auszahlungskonto handelt es sich um das Bankkonto, auf das wir Ihren Verkaufserlös überweisen. Sie können alle Zahlungskonten anzeigen, die Sie auf der Seite Profil eingegeben haben.

> [!NOTE]
> In einigen Märkten kann PayPal als Auszahlungskonto verwendet werden. Weitere Informationen finden [Sie unter](#paypal-info) [Zahlungs Schwellenwerte, Methoden und Zeitrahmen](payment-thresholds-methods-and-timeframes.md) , um herauszufinden, ob PayPal für einen bestimmten Markt unterstützt wird.

### <a name="create-a-payment-profile"></a>Erstellen eines Zahlungs Profils

1. Wählen Sie in [Partner Center](https://partner.microsoft.com/dashboard)in der oberen rechten Ecke das Zahnrad Symbol " **Einstellungen** " und dann " **Entwicklereinstellungen**" aus.
2. Wählen Sie unterhalb der Rubrik *Auszahlungs-und Steuer* Überschrift die Option **Auszahlungs-und Steuer Profil Zuweisung**.

    > [!NOTE]
    > Da es sich um vertrauliche Informationen handelt, können Sie aufgefordert werden, sich erneut anzumelden.

3. Wählen Sie die Zahlungsmethode aus, die Sie konfigurieren möchten.

    ![Auswahl für Auszahlungs Kontotyp](images/payout-account-type-selection.png)

4. Wählen Sie ein vorhandenes Zahlungsprofil aus, oder klicken Sie auf **Neues Zahlungsprofil erstellen** , um ein neues Profil für die ausgewählte Zahlungsmethode zu erstellen.

### <a name="create-a-bank-based-payment-profile"></a>Erstellen eines bankbasierten Zahlungs Profils

Wenn Sie sich für die Verwendung eines Bankkonto für den Empfang von Auszahlungen entschieden haben, führen Sie den folgenden Vorgang aus, um Ihr Bankkonto zu konfigurieren.

1. Geben Sie auf der Seite *Bank Profil* die erforderlichen Informationen zu Ihrer Bank ein.
2. Geben Sie die Details Ihres Bankkontos an.

    > [!NOTE]
    > Für die Felder, in die Sie Ihre Kontoinformationen eingeben, sind nur alphanumerische Zeichen zulässig.

    ![Informationen zur Auszahlungs Bank](images/payout-bank-info.png)

3. Geben Sie Details zum Empfänger an.
4. Wählen Sie auf der Seite *Profil Zuweisung* die Währung aus, die Sie bei der Ausgabe Ihrer Auszahlungen verwenden möchten.

    > [!WARNING]
    > Stellen Sie sicher, dass Ihre Bank die von Ihnen gewählte Auszahlungs Währung akzeptiert.

5. Sie müssen für jedes Programm, an dem Sie teilnehmen, ein Zahlungsprofil auswählen, aber Sie können für mehrere Programme dasselbe Profil verwenden.

    ![Auszahlungs Verwendung von Bank Profil](images/payout-use-bank-profile.png)

6. Klicken Sie auf Submit, um die Änderungen zu speichern.

> [!NOTE]
> Es kann bis zu 48 Stunden dauern, bis Microsoft die Informationen in Ihrem Profil überprüft hat. Wenn dieser Vorgang fertiggestellt ist, wird der *Überprüfungs Status* " **Complete** " angezeigt

Berücksichtigen Sie auch folgende Punkte, um sicherzustellen, dass Ihre Auszahlung erfolgreich ist:

- Der **Name des Kontoinhabers** , der für Ihr Auszahlungs Konto in Partner Center eingegeben wurde, muss exakt dem Namen Ihres Bankkonto zugeordnet sein. Wenn Ihr Bankkontoname beispielsweise einen zweiten Vornamen enthält, geben Sie in **Name des Kontoinhabers** einen zweiten Vornamen ein.
- Auszahlungen werden direkt von Microsoft an Ihr Bankkonto in US-Dollar überwiesen.
- Bank Informationen, die in Partner Center in lateinischen Zeichen eingegeben werden, werden in kyrillische Zeichen übersetzt.

### <a name="editing-existing-payment-profiles"></a>Bearbeiten vorhandener Zahlungs profile

Sie können vorhandene Zahlungs Profile bearbeiten, wenn Sie Änderungen vornehmen oder falsche Informationen korrigieren müssen.

1. Wählen Sie in [Partner Center](https://partner.microsoft.com/dashboard)in der oberen rechten Ecke das Zahnrad Symbol " **Einstellungen** " und dann " **Entwicklereinstellungen**" aus.
2. Wählen Sie unter der Überschrift *Auszahlung und Steuern* die Option **Auszahlungs-und Steuer profile**aus.
3. Ihre Zahlungs Profile werden zusammen mit Ihrem Status aufgeführt. Suchen Sie das Profil, das Sie bearbeiten möchten, und klicken Sie ganz rechts auf **Bearbeiten** .

> [!IMPORTANT]
> Durch die Änderung des Auszahlungskontos werden Ihre Zahlungen unter Umständen um einen Zahlungszyklus verzögert. Diese Verzögerung entsteht, da wir die Kontoänderung genau wie die erste Einrichtung des Auszahlungskontos überprüfen müssen. Sie erhalten dennoch den vollständigen Betrag, nachdem Ihr Konto verifiziert wurde. Zahlungen für den aktuellen Zahlungszyklus werden zum nächsten Zyklus addiert. Weitere Informationen finden Sie unter [Regelung der Bezahlung](getting-paid-apps.md).

### <a name="paypal-info"></a>PayPal-Informationen

In ausgewählten Ländern und Regionen können Sie ein Zahlungskonto erstellen, indem Sie Ihre PayPal-Informationen eingeben. Bevor Sie PayPal als Option für Ihr Zahlungskonto auswählen, sind die folgenden Schritte erforderlich:

- Überprüfen Sie die [Zahlungs Schwellenwerte, Methoden und Zeitrahmen](payment-thresholds-methods-and-timeframes.md) , um zu bestätigen, ob PayPal eine unterstützte Zahlungsmethode in Ihrem Land oder Ihrer Region ist.
- Lesen Sie die folgenden FAQs. Je nach Fall ist ein PayPal-Zahlungskonto möglicherweise nicht die ideale Lösung für Sie. In manchen Situationen ist eine Bankverbindung besser geeignet.

Allgemeine Fragen über PayPal als Zahlungsmethode:

- **Welche PayPal-Einstellungen benötige ich, um Zahlungen zu erhalten?** Sie müssen sicherstellen, dass Ihr PayPal-Konto keine Zahlungen per eCheck blockiert. Diese Einstellung wird über die PayPal-Seite für die bevorzugte Überweisungsmethode verwaltet. Weitere Informationen finden Sie auf der [PayPal-Seite zum Einrichten des Kontos](https://go.microsoft.com/fwlink/p/?linkid=513139).
- **Wird mein Land/Ihre Region unterstützt?** Weitere Informationen finden Sie unter [Zahlungs Schwellenwerte, Methoden und Zeitrahmen](payment-thresholds-methods-and-timeframes.md) , um herauszufinden, wo PayPal eine unterstützte Zahlungsmethode ist.
- **Muss mein PayPal-Konto in demselben Land/derselben Region wie mein Partner Center-Konto registriert werden?** Nein. Beim Einrichten eines PayPal-Kontos können Sie die Standardkonfiguration akzeptieren. Normalerweise treten keine Probleme mit anderen Ländern/Regionen und Währungen auf, sofern die Zahlung in einigen Währungen nicht blockiert ist. Diese Einstellung wird über die PayPal-Seite für die bevorzugte Überweisungsmethode verwaltet.
- **Muss ich PayPal-Zahlungen manuell akzeptieren?** Nein. PayPal-Konten sind dafür ausgelegt, dass Benutzer Zahlungen manuell akzeptieren müssen. Wenn Sie die Zahlung binnen 30 Tagen nicht akzeptieren, wird sie storniert. Sie können diese Einstellung ändern, indem Sie auf der PayPal-Seite mit weiteren Einstellungen die Bestätigungsaufforderung deaktivieren.
- **Welche Währungen werden von PayPal unterstützt?** Die aktuelle Liste finden Sie auf der [Supportseite von PayPal](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) .

### <a name="specific-requirements-for-certain-countriesregions"></a>Spezifische Anforderungen für bestimmte Länder/Regionen

In einigen Ländern und Regionen gelten zusätzliche Anforderungen für Auszahlungskonten. Wenn Sie in Pakistan, der Russischen Föderation oder der Ukraine ansässig sind, sollten Sie die folgenden Anforderungen beachten.

#### <a name="pakistan"></a>Pakistan

„Form-R“ stellt eine bankaufsichtsrechtliche Anforderung in Pakistan dar. Im Formular wird der Zweck und Grund für den Eingang finanzieller Mittel aus dem Ausland dargelegt. Sobald Sie Anspruch auf eine monatliche Auszahlung von Microsoft haben, müssen Sie „Form-R“ bei Ihrer Bank einreichen, bevor die Auszahlung auf Ihr Konto erfolgen kann. Wie Sie „Form-R“ erhalten, erfahren Sie bei Ihrer Bankfiliale vor Ort.

„Form-R“ muss jeden Monat bei der Bank eingereicht werden, in dem Anspruch auf eine Auszahlung besteht. Wenn Sie beispielsweise jeden Monat des Jahres eine Auszahlung erwarten, ist „Form-R“ 12 Mal (einmal monatlich) einzureichen.

Sobald die Auszahlung an Ihre Bank angewiesen wurde, muss „Form-R“ innerhalb von 30 Tagen vorliegen. Verstreicht die 30-tägige Einreichungsfrist, wird der Betrag an Microsoft zurücküberwiesen.

#### <a name="russia"></a>Russland

Wenn Sie Entwickler sind und in Russland leben, müssen Sie möglicherweise Ihrer Bank Belege vorlegen, bevor diese Geldmittel auf Ihr Konto einzahlt. Wenn Sie für eine Zahlung berechtigt sind, erhalten Sie von uns folgende Belege in einer E-Mail-Nachricht:

1. Acceptance Certificate (AC): Enthält den Zahlungsbetrag, der auf Ihr Konto überwiesen wird.
2. App Developer Agreement (ADA): Eine unterschriebene Kopie der Entwicklervereinbarung, die gegengezeichnet werden muss.

Berücksichtigen Sie auch folgende Punkte, um sicherzustellen, dass Ihre Auszahlung erfolgreich ist:

- Der **Name des Kontoinhabers** , der für Ihr Auszahlungs Konto in Partner Center eingegeben wurde, muss exakt dem Namen Ihres Bankkonto zugeordnet sein. Wenn Ihr Bankkontoname beispielsweise einen zweiten Vornamen enthält, geben Sie in **Name des Kontoinhabers** einen zweiten Vornamen ein.
- Auszahlungen werden direkt von Microsoft an Ihr Bankkonto in Rubel (RUB) überwiesen.
- Bank Informationen, die in Partner Center in lateinischen Zeichen eingegeben werden, werden in kyrillische Zeichen übersetzt.
- Auszahlungen müssen auf ein Bankkonto, nicht auf eine Bankkarte erfolgen.

#### <a name="ukraine"></a>Ukraine

Wenn Sie Entwickler und in der Ukraine ansässig sind, müssen Sie Ihre Empfangsberechtigung gegenüber der Bank u. U. mit entsprechenden Belegen nachweisen. Wenn Sie für eine Zahlung berechtigt sind, erhalten Sie von uns folgende Belege in einer E-Mail-Nachricht:

1. Acceptance Certificate (AC): Enthält den Zahlungsbetrag, der auf Ihr Konto überwiesen wird.
2. App Developer Agreement (ADA): Eine unterschriebene Kopie der Entwicklervereinbarung, die gegengezeichnet werden muss.
3. Amendment Agreement (AA): Mit diesem Dokument kann Ihre Bank den Auszahlungsbetrag einfacher identifizieren.

Microsoft stellt alle drei Dokumente beim ersten Auszahlungsversuch zur Verfügung. Für alle nachfolgenden Auszahlungen erhalten Sie nur das AC-Dokument. Bewahren Sie das ADA- und AA-Dokument für den Fall auf, dass Sie sie für zukünftige Auszahlungen Ihrer Bank benötigen.

### <a name="create-a-paypal-payment-profile"></a>Erstellen eines PayPal-Zahlungs Profils

Wenn Sie sich für die Verwendung eines Bankkonto für den Empfang von Auszahlungen entschieden haben, führen Sie den folgenden Vorgang aus, um Ihr Bankkonto zu konfigurieren.

1. Geben Sie auf der Seite *PayPal* die erforderlichen Informationen zu Ihrem PayPal-Konto an.
2. Geben Sie die Details Ihres PayPal-Kontos an.

    > [!NOTE]
    > Für die Felder, in die Sie Ihre Kontoinformationen eingeben, sind nur alphanumerische Zeichen zulässig.

    ![Informationen zur Auszahlungs-PayPal](images/payout-paypal-info.png)

3. Geben Sie Details zum Empfänger an.
4. Wählen Sie auf der Seite *Profil Zuweisung* die Währung aus, die Sie bei der Ausgabe Ihrer Auszahlungen verwenden möchten.
5. Sie müssen für jedes Programm, an dem Sie teilnehmen, ein Zahlungsprofil auswählen, aber Sie können für mehrere Programme dasselbe Profil verwenden.
6. Klicken Sie auf Submit, um die Änderungen zu speichern.