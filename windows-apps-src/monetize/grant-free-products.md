---
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um einem bestimmten Benutzer eine kostenlose App oder ein kostenloses Add-On zu gewähren.
title: Gewähren kostenloser Produkte
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Einkaufs-API, Produkte gewähren
ms.localizationpriority: medium
ms.openlocfilehash: 957958891b1052be4ac9ae65d90f97ff8a44ef36
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613195"
---
# <a name="grant-free-products"></a>Gewähren kostenloser Produkte

Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um einem Benutzer eine kostenlose App oder ein kostenloses Add-On (auch In-App-Produkt oder IAP genannt) zu gewähren.

Derzeit können Sie nur kostenlose Produkte gewähren. Wenn Ihr Dienst versucht, diese Methode zum Gewähren eines nicht kostenlosen Produkts zu verwenden, gibt diese Methode einen Fehler zurück.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
* Ein Microsoft Store-ID-Schlüssel, der die Identität des Benutzers darstellt, dem Sie ein kostenloses Produkt gewähren möchten.

Weitere Informationen finden Sie unter [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorisierung  | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.                           |
| Host           | string | Muss auf den Wert **purchase.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter      | Typ   | Beschreibung        | Erforderlich |
|----------------|--------|---------------------|----------|
| availabilityId | string | Die Verfügbarkeits-ID des Produkts, das aus dem Microsoft Store-Katalog gewährt werden soll.         | Ja      |
| b2bKey         | string | Der [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, dem Sie ein kostenloses Produkt gewähren möchten.    | Ja      |
| devOfferId     | string | Eine vom Entwickler angegebene Angebotskennung, die nach dem Kauf im Auflistungselement angezeigt wird.        |
| language       | string | Die Sprache des Benutzers.  | Ja      |
| market         | string | Der Markt des Benutzers.       | Ja      |
| orderId        | guid   | Eine für den Auftrag generierte GUID. Dieser Wert muss für den Benutzer eindeutig sein, aber nicht auftragsübergreifend.    | Ja      |
| productId      | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities) aus dem Microsoft Store-Katalog. Ein Beispiel für eine Store-ID für ein Produkt ist 9NBLGGH42CFD. | Ja      |
| quantity       | int    | Die Kaufmenge. Derzeit wird als einziger Wert 1 unterstützt. Ohne Angabe wird standardmäßig der Wert 1 verwendet.   | Nein       |
| skuId          | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für die [Produkt-SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) aus dem Microsoft Store-Katalog. Ein Beispiel für eine Store-ID für eine SKU ist 0010.     | Ja      |


### <a name="request-example"></a>Anforderungsbeispiel

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Parameter                 | Typ                        | Beschreibung             | Erforderlich |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | Informationen zum Clientkontext für diesen Auftrag. Sie werden dem *clientID*-Wert aus dem Azure AD-Token zugewiesen.    | Ja      |
| createdtime               | datetimeoffset              | Der Zeitpunkt der Auftragserstellung.         | Ja      |
| currencyCode              | string                      | Der Währungscode für *totalAmount* und *totalTaxAmount*. Für kostenlose Artikel nicht relevant.     | Ja      |
| friendlyName              | string                      | Der Anzeigename für den Auftrag. Für Aufträge über die Microsoft Store-Einkaufs-API nicht relevant. | Ja      |
| isPIRequired              | boolean                     | Gibt an, ob für die Bestellung ein Zahlungsmittel (Payment Instrument, PI) erforderlich ist.  | Ja      |
| language                  | string                      | Die Sprach-ID für den Auftrag (z. B. „de“).       | Ja      |
| market                    | string                      | Die Markt-ID für den Auftrag (z. B. „DEU“).  | Ja      |
| orderId                   | string                      | Eine ID, die den Auftrag für einen bestimmten Benutzer identifiziert.                | Ja      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | Die Liste mit den Auftragspositionen. In der Regel gibt es 1 Position pro Auftrag.       | Ja      |
| orderState                | string                      | Der Zustand des Auftrags. Gültige Zustände sind **Editing**, **CheckingOut**, **Pending**, **Purchased**, **Refunded**, **ChargedBack** und **Cancelled**. | Ja      |
| orderValidityEndTime      | string                      | Der Zeitpunkt, bis zu dem der Preis des Auftrags vor der Übermittlung gültig ist. Für kostenlose Apps nicht relevant.      | Ja      |
| orderValidityStartTime    | string                      | Der Zeitpunkt, ab dem der Preis des Auftrags vor der Übermittlung gültig ist. Für kostenlose Apps nicht relevant.          | Ja      |
| purchaser                 | IdentityV6                  | Ein Objekt, das die Identität des Käufers beschreibt.       | Ja      |
| totalAmount               | decimal                     | Der Gesamtkaufbetrag aller im Auftrag enthaltenen Artikel (inklusive Steuern).       | Ja      |
| totalAmountBeforeTax      | decimal                     | Der Gesamtkaufbetrag aller im Auftrag enthaltenen Artikel (vor Steuern).      | Ja      |
| totalChargedToCsvTopOffPI | decimal                     | Bei Verwendung eines separaten Zahlungsmittels und eines gespeicherten Werts (CSV) der für CSV in Rechnung gestellte Betrag.            | Ja      |
| totalTaxAmount            | decimal                     | Die Gesamtsteuerbetrag für alle Positionen.    | Ja      |


Das ClientContext-Objekt enthält die folgenden Parameter.

| Parameter | Typ   | Beschreibung                           | Erforderlich |
|-----------|--------|---------------------------------------|----------|
| Client    | string | Die Client-ID, unter der der Auftrag erstellt wurde. | Nein       |


Das OrderLineItemV6-Objekt enthält die folgenden Parameter.

| Parameter               | Typ           | Beschreibung                                                                                                  | Erforderlich |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| Agent                   | IdentityV6     | Der Agent, der die Position zuletzt bearbeitet hat. Weitere Informationen zu diesem Objekt finden Sie in der folgenden Tabelle.       | Nein       |
| availabilityId          | string         | Die Verfügbarkeits-ID des Produkts, das aus dem Microsoft Store-Katalog erworben werden soll.                           | Ja      |
| beneficiary             | IdentityV6     | Die Identität des Begünstigten für den Auftrag.                                                                | Nein       |
| billingState            | string         | Der Abrechnungszustand des Auftrags. Dieser wird bei Beendigung auf **Charged** festgelegt.                                   | Nein       |
| campaignId              | string         | Die Kampagnen-ID für diesen Auftrag.                                                                              | Nein       |
| currencyCode            | string         | Der verwendete Währungscode für die Preisdetails.                                                                    | Ja      |
| description             | string         | Eine lokalisierte Beschreibung der Position.                                                                    | Ja      |
| devofferId              | string         | Die Auftrags-ID für diesen Auftrag, sofern vorhanden.                                                           | Nein       |
| fulfillmentDate         | datetimeoffset | Das Erfüllungsdatum.                                                                           | Nein       |
| fulfillmentState        | string         | Der Erfüllungszustand dieses Artikels. Dieser wird bei Beendigung auf **Fulfilled** festgelegt.                      | Nein       |
| isPIRequired            | boolean        | Gibt an, ob für diese Position ein Zahlungsmittel erforderlich ist.                                       | Ja      |
| isTaxIncluded           | boolean        | Gibt an, ob in den Preisdetails des Artikels Steuern enthalten sind.                                        | Ja      |
| legacyBillingOrderId    | string         | Die alte Abrechnungs-ID.                                                                                       | Nein       |
| lineItemId              | string         | Die Positions-ID für den Artikel in diesem Auftrag.                                                                 | Ja      |
| listPrice               | decimal        | Der Listenpreis des Artikels in diesem Auftrag.                                                                    | Ja      |
| productId               | string         | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities), das die Position im Microsoft Store-Katalog darstellt. Ein Beispiel für eine Store-ID für ein Produkt ist 9NBLGGH42CFD.   | Ja      |
| productType             | string         | Die Art des Produkts. Die unterstützten Werte sind **Durable**, **Application** und **UnmanagedConsumable**. | Ja      |
| quantity                | int            | Die Menge des bestellten Artikels.                                                                            | Ja      |
| retailPrice             | decimal        | Der Einzelhandelspreis des bestellten Artikels.                                                                        | Ja      |
| revenueRecognitionState | string         | Der Zustand der Umsatzrealisierung.                                                                               | Ja      |
| skuId                   | string         | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) der Position im Microsoft Store-Katalog. Ein Beispiel für eine Store-ID für eine SKU ist 0010.                                                                   | Ja      |
| taxAmount               | decimal        | Der Steuerbetrag für die Position.                                                                            | Ja      |
| taxType                 | string         | Der Steuertyp für die anfallenden Steuern.                                                                       | Ja      |
| Titel                   | string         | Der lokalisierte Titel der Position.                                                                        | Ja      |
| totalAmount             | decimal        | Der Gesamtkaufbetrag der Position (inklusive Steuern).                                                    | Ja      |


Das IdentityV6-Objekt enthält die folgenden Parameter.

| Parameter     | Typ   | Beschreibung                                                                        | Erforderlich |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Enthält den Wert **"pub"**.                                                      | Ja      |
| identityValue | string | Der Zeichenfolgenwert von *publisherUserId* aus dem angegebenen Microsoft Store-ID-Schlüssel. | Ja      |


### <a name="response-example"></a>Antwortbeispiel

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>Fehlercodes


| Code | Fehler        | Interner Fehlercode           | Beschreibung   |
|------|--------------|----------------------------|----------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | PartnerAadTicketRequired   | An den Dienst wurde im Autorisierungsheader kein Azure AD-Zugriffstoken übergeben.   |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *clientId*-Anspruch im Microsoft Store-ID-Schlüssel im Anforderungstext und der *appid*-Anspruch im Azure AD-Zugriffstoken im Autorisierungsheader stimmen nicht überein.       |
| 400  | BadRequest   | InvalidParameter           | Die Details enthalten Informationen zum Anforderungstext und zu Feldern mit ungültigem Wert.           |

<span/> 

## <a name="related-topics"></a>Verwandte Themen

* [Verwalten von Product-Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md)
* [Abfrage von Produkten](query-for-products.md)
* [Verbrauchsartikeln gemeldet werden, weil erfüllt](report-consumable-products-as-fulfilled.md)
* [Erneuern Sie einen Microsoft Store-ID-Schlüssel](renew-a-windows-store-id-key.md)
