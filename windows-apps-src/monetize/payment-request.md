---
description: Die Zahlung anfordern-API bietet eine integrierte Lösung für UWP-apps, die einen Benutzer auf die Eingabe von Zahlungsinformationen, und wählen die Protokollversand-Methoden erfordern den Prozess zu umgehen.
title: Vereinfachen von Zahlungen mit der Payment Request-API
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, Uwp, Zahlung-Anforderung
ms.openlocfilehash: e5fb5cead7833b8cc213c6633cae6cee0da3466b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607865"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Vereinfachen von Zahlungen mit der Payment Request-API
Die Zahlung anfordern-API für UWP-apps basiert auf der [W3C Zahlung: Anfordern von API-Spezifikation](https://w3c.github.io/browser-payment-api/). Er Ihnen die Möglichkeit, den Kassenvorgang in UWP-apps zu optimieren. Benutzer können über Auschecken beschleunigen, indem Zahlungsoptionen und Lieferadressen vorliegen, die bereits mit ihrem Microsoft-Konto gespeichert. Sie können Ihre Conversion-Rate zu erhöhen und verringern das Risiko von sicherheitsverletzungen von Daten, da die Zahlungsinformationen mit Token versehen werden. Ab Windows 10 Creators Update können können Benutzer ihre gespeicherten Zahlungsoptionen einfach über verschiedene Umgebungen in UWP-apps hinweg zu bezahlen.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit der Zahlung anfordern-API beginnen, gibt es einige Dinge, die müssen Sie tun oder achten.

### <a name="getting-a-merchant-id"></a>Einen Händler-ID abrufen
Als Teil der Zahlung-Request-Prozess fordert Microsoft Payment-Token in Ihrem Namen bei Ihrem Dienstanbieter an. Bevor Sie die API verwenden können, benötigen wir also Ihre Autorisierung, um diese Token anzufordern.  Sie müssen einige Schritte zum Registrieren für ein Verkäuferkonto und die erforderliche Autorisierung befolgen. Wechseln Sie zu diesem Zweck auf [Microsoft Seller Center](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp). Sobald Sie dies getan haben, kopieren Sie den resultierenden Händler-ID aus der Partner Center in Ihrer app beim Erstellen der Anforderung für die Zahlung. Klicken Sie dann, wenn Ihre Anwendung eine Zahlung-Anforderung sendet, ein Token für die Zahlung erhalten Sie von den Prozessor die Sie benötigen, um Ihre Zahlung zu übermitteln.

### <a name="geographic-restrictions-and-language-support"></a>Geografische Beschränkungen und sprachunterstützung
Die Zahlung anfordern-API kann nur von US-amerikanischen Unternehmen zum Verarbeiten von Transaktionen in den Vereinigten Staaten verwendet werden.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Verwenden die Zahlung anfordern-API in Ihrer app: Schritt für Schritt
In diesem Abschnitt wird veranschaulicht, wie Sie mit der [UWP Zahlung anfordern-API-](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments) in Ihrer app. Wir verwenden hier die API in ihrer einfachsten Form aus Gründen der Klarheit. Ein Beispiel für die erweiterte Verwendung dieser APIs, finden Sie unter den [Warenkorb von UWP-app-Beispiel auf GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Erstellen Sie einen Satz von allen Zahlungsoptionen, die Sie akzeptieren.
> [!Note]
> Ersetzen Sie die **Händler-Id-aus--Portal des Verkäufers** Text mit der Händler-ID, die Sie aus dem Verkäufer Center empfangen.

[!code-cs[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Rufen Sie die Zahlungsdetails zusammen. 

Diese Details werden in der app für die Zahlung an den Benutzer angezeigt werden. 

[!code-cs[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. Enthalten Sie die Mehrwertsteuer. 

> [!Important]
> Die API nicht Elemente hinzufügen oder die Mehrwertsteuer berechnen möchten. Denken Sie daran, dass die Steuersätze variieren. Aus Gründen der Übersichtlichkeit verwenden wir einen hypothetische 9.5 % Steuersatz.

[!code-cs[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (Optional)  Fügen Sie mit dem gesamten Rabatte oder andere Modifizierer hinzu. 

Hier ist ein Beispiel für einen Rabatt für die Verwendung einer bestimmten Contoso Kreditkarte, die anzeigen-Elemente hinzufügen. (*Contoso* ist ein fiktiver Name.)

[!code-cs[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Zusammenstellen der Payment-Details an.

[!code-cs[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-cs[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. Die Zahlung anfordern. 

Rufen Sie die **SubmitPaymentRequestAsync** Methode, um die Zahlung anfordern. Dadurch wird die Zahlung-app zeigt die verfügbaren Optionen.

[!code-cs[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

Der Benutzer wird aufgefordert, sich mit ihrem Microsoft-Konto anzumelden.

Nachdem der Benutzer anmeldet, können sie auswählen, Zahlungsoptionen und Lieferadresse an, die zuvor gespeichert wurden.

![Zahlung Anforderung UI](./images/33.png "Zahlungsaufforderung UI")

Ihre app wartet, bis der Benutzer tippen **Zahlen**, schließt dann die Reihenfolge.

[!code-cs[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Nach Abschluss der Zahlung erhält der Benutzer mit einem **Bestellung bestätigt** Bildschirm.

![Bestellung bestätigt](./images/44.png "Bestellung bestätigt ")

## <a name="see-also"></a>Siehe auch
- [Windows.ApplicationModel.Payments-Referenzdokumentation](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [UWP shopping-app-Beispiel auf GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C Zahlung: Anfordern von API-Spezifikation](https://www.w3.org/TR/payment-request/)
- [Zahlung Anforderung API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

