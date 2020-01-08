---
description: Die Payment Request-API stellt eine integrierte Lösung für UWP-apps bereit, um den Prozess zu umgehen, bei dem ein Benutzer Zahlungsinformationen eingeben und Versandmethoden auswählen muss.
title: Vereinfachen von Zahlungen mit der Payment Request-API
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, Zahlungs Anforderung
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685044"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Vereinfachen von Zahlungen mit der Payment Request-API
Die Payment Request-API für UWP-apps basiert auf der W3C-Spezifikation für die [API-Zahlungsanfrage](https://w3c.github.io/browser-payment-api/). Dadurch haben Sie die Möglichkeit, den Checkout-Prozess in ihren UWP-apps zu optimieren. Benutzer können das Auschecken mithilfe von Zahlungsoptionen und Versandadressen beschleunigen, die bereits mit Ihrem Microsoft-Konto gespeichert wurden. Sie können die Konvertierungsrate erhöhen und das Risiko von Datenverletzungen verringern, da die Zahlungsinformationen tokenisiert werden. Beginnend mit dem Windows 10 Creators Update können Benutzer Ihre gespeicherten Zahlungsoptionen verwenden, um problemlos über Umgebungen in UWP-apps zu bezahlen.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit der Verwendung der Payment Request-API beginnen, müssen Sie einige Dinge tun, die Sie beachten müssen.

### <a name="getting-a-merchant-id"></a>Eine Händler-ID wird erhalten.
Im Rahmen des Zahlungs Anforderungs Prozesses fordert Microsoft Zahlungs Token von Ihrem Dienstanbieter in Ihrem Namen an. Bevor Sie also mit der Verwendung der API beginnen können, benötigen wir Ihre Autorisierung, um diese Token anzufordern.  Sie müssen einige Schritte ausführen, um sich für ein Verkäufer Konto zu registrieren und die erforderliche Autorisierung bereitzustellen. Wechseln Sie hierzu zum Microsoft- [Verkäufer Center](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp). Nachdem Sie dies abgeschlossen haben, kopieren Sie die resultierende Händler-ID aus Partner Center in Ihre APP, wenn Sie die Zahlungsanfrage erstellen. Wenn Ihre Anwendung dann eine Zahlungsanfrage sendet, erhalten Sie ein Zahlungs Token von Ihrem Prozessor, das Sie zur Übermittlung Ihrer Zahlung benötigen.

### <a name="geographic-restrictions-and-language-support"></a>Geografische Einschränkungen und Sprachunterstützung
Die Payment Request-API kann nur von US-basierten Unternehmen verwendet werden, um Transaktionen in der USA zu verarbeiten.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Verwenden der Payment Request-API in Ihrer APP: Schritt für Schritt
In diesem Abschnitt wird die Verwendung der [UWP-Payment Request-API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) in Ihrer APP veranschaulicht. Wir verwenden die API hier in der einfachsten Form aus Gründen der Übersichtlichkeit. Ein Beispiel für eine erweiterte Verwendung dieser APIs finden Sie im Beispiel für eine [UWP-Einkaufs-App auf GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Erstellen Sie eine Gruppe aller Zahlungsoptionen, die Sie akzeptieren.
> [!Note]
> Ersetzen Sie den Text " **Händler-ID-from-Seller-Portal** " durch die Händler-ID, die Sie im Verkäufer Center erhalten haben.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Abrufen der Zahlungsdetails 

Diese Details werden dem Benutzer in der Zahlungs-App angezeigt. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. einschließen der Verkaufssteuer. 

> [!Important]
> Die API fügt keine Elemente hinzu oder berechnet die Verkaufssteuer nicht für Sie. Beachten Sie, dass die Steuersätze je nach Zuständigkeit variieren. Aus Gründen der Übersichtlichkeit verwenden wir einen hypothetischen Preis in Höhe von 9,5%.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (optional) fügen Sie der Summe Rabatte oder andere modifiziererer hinzu. 

Hier ist ein Beispiel für das Hinzufügen eines Rabatts für die Verwendung einer bestimmten "a"-Kreditkarte für die Anzeigeelemente. ( *"Fium"* ist ein fiktiver Name.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. assemblieren Sie alle Zahlungsdetails.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. übermitteln Sie die Zahlungs Anforderung. 

Wenden Sie die **submitpaymentrequestasync** -Methode an, um Ihre Zahlungs Anforderung zu übermitteln. Dadurch wird die Zahlungs-App angezeigt, in der die verfügbaren Zahlungsoptionen angezeigt werden.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

Der Benutzer wird aufgefordert, sich mit seinem Microsoft-Konto anzumelden.

Nachdem sich der Benutzer angemeldet hat, kann er Zahlungsoptionen und Versandadresse auswählen, die zuvor gespeichert wurden.

![Benutzeroberfläche für Zahlungsanforderungen](./images/33.png "Benutzeroberfläche für Zahlungsanforderungen")

Ihre APP wartet darauf, dass der Benutzer auf **bezahlen**tippt, und schließt dann die Bestellung ab.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Nachdem die Zahlung vollständig ist, wird dem Benutzer ein Bildschirm mit der **Bestellbestätigung** angezeigt.

![Bestellung bestätigt](./images/44.png "Bestellung bestätigt")

## <a name="see-also"></a>Weitere Informationen:
- [Referenz Dokumentation zu Windows. applicationmodel. Payment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [Beispiel für UWP-Einkaufs-App auf GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [API-Spezifikation für W3C-Zahlungsanforderungen](https://www.w3.org/TR/payment-request/)
- [Payment Request-API](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

