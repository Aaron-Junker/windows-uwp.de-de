---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Verwenden Sie diese Methode in der Microsoft Store Collection-API, um alle Produkte zu erhalten, die ein Kunde für apps besitzt, die mit ihrer Azure AD Client-ID verknüpft sind. Sie können die Abfrage auf ein bestimmtes Produkt beschränken oder weitere Filter verwenden.
title: Produktabfrage
ms.date: 03/26/2021
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Sammlungs-API, Produkte anzeigen
ms.localizationpriority: medium
ms.openlocfilehash: 25d7e5a67d35287433be56bcab6d4e9142f68084
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619276"
---
# <a name="query-for-products"></a>Produktabfrage


Verwenden Sie diese Methode in der Microsoft Store Collection-API, um alle Produkte zu erhalten, die ein Kunde für apps besitzt, die mit ihrer Azure AD Client-ID verknüpft sind. Sie können die Abfrage auf ein bestimmtes Produkt beschränken oder weitere Filter verwenden.

Diese Methode ist so konzipiert, dass sie von Ihrem Dienst als Reaktion auf eine Nachricht von Ihrer App aufgerufen wird. Der Dienst sollte nicht nach einem regelmäßigen Zeitplan alle Benutzer abfragen.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD Zugriffs Token, das den Wert für den Zielgruppen-Uri aufweist `https://onestore.microsoft.com` .
* Ein Microsoft Store ID-Schlüssel, der die Identität des Benutzers darstellt, dessen Produkte Sie erhalten möchten.

Weitere Informationen finden Sie unter [Verwalten von Produkt Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | type   | BESCHREIBUNG                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;.                           |
| Host           | Zeichenfolge | Muss auf den Wert **collections.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | Zeichenfolge | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter         | type         | BESCHREIBUNG         | Erforderlich |
|-------------------|--------------|---------------------|----------|
| beneficiaries     | &lt;Useridentität auflisten&gt; | Eine Liste von UserIdentity-Objekten, die die Benutzer darstellen, die für Produkte abgefragt werden. Weitere Informationen finden Sie in der folgenden Tabelle.    | Yes      |
| continuationToken | Zeichenfolge       | Bei mehreren Produktgruppen gibt der Antworttext ein Fortsetzungstoken zurück, sobald das Seitenlimit erreicht ist. Geben Sie das Fortsetzungstoken hier bei nachfolgenden Aufrufen an, um die verbleibenden Produkte abzurufen.       | Nein       |
| maxPageSize       | number       | Die maximale Anzahl von Produkten, die in einer Antwort zurückgegeben werden sollen. Der standardmäßige Höchstwert ist 100.                 | Nein       |
| modifiedAfter     | datetime     | Falls angegeben, gibt der Dienst nur Produkte zurück, die nach diesem Datum geändert wurden.        | Nein       |
| parentProductId   | Zeichenfolge       | Falls angegeben, gibt der Dienst nur Add-Ons zurück, die der angegebenen App entsprechen.      | Nein       |
| productSkuIds     | &lt;productskuid auflisten&gt; | Wenn angegeben, gibt der Dienst nur Produkte zurück, die für die angegebenen Product-/SKU-Paare anwendbar sind. Weitere Informationen finden Sie in der folgenden Tabelle.      | Nein       |
| productTypes      | Listen &lt; Zeichenfolge&gt;       | Gibt an, welche Produkttypen in den Abfrage Ergebnissen zurückgegeben werden sollen. Unterstützte Produkttypen sind **Application**, **Durable**, **Game** und **unmanagedconsubar**.     | Yes       |
| validityType      | Zeichenfolge       | Bei Festlegung auf **Alle** werden alle Produkte für einen Benutzer, einschließlich abgelaufener Elemente, zurückgegeben. Bei Festlegung auf **Valid** werden nur Produkte zurückgegeben, die zum aktuellen Zeitpunkt gültig sind (aktiver Status, Startdatum &lt; Istdatum und Enddatum &gt; Istdatum). | Nein       |


Das UserIdentity-Objekt enthält die folgenden Parameter.

| Parameter            | type   |  BESCHREIBUNG      | Erforderlich |
|----------------------|--------|----------------|----------|
| identityType         | Zeichenfolge | Gibt den Zeichenfolgenwert **b2b** an.    | Yes      |
| identityValue        | Zeichenfolge | Der [Microsoft Store ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4) , der die Identität des Benutzers darstellt, für den Sie Produkte Abfragen möchten.  | Yes      |
| localTicketReference | Zeichenfolge | Der angeforderte Bezeichner für die zurückgegebenen Produkte. Die zurückgegebenen Elemente im Antworttext verfügen über ein übereinstimmendes *localTicketReference*-Element. Es wird empfohlen, dass Sie den gleichen Wert wie der *UserID* -Anspruch im Microsoft Store ID-Schlüssel verwenden. | Yes      |


Das ProductSkuId-Objekt enthält die folgenden Parameter.

| Parameter | type   | BESCHREIBUNG          | Erforderlich |
|-----------|--------|----------------------|----------|
| productId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für ein [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities) im Microsoft Store Katalog. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd. | Yes      |
| skuID     | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) eines Produkts im Microsoft Store Katalog. Eine Beispiel-Speicher-ID für eine SKU ist 0010.       | Yes      |


### <a name="request-example"></a>Anforderungsbeispiel

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
  "maxPageSize": 100,
  "beneficiaries": [
    {
      "localTicketReference": "1055521810674918",
      "identityValue": "eyJ0eXAiOiJ……",
      "identityType": "b2b"
    }
  ],
  "modifiedAfter": "\/Date(-62135568000000)\/",
  "productSkuIds": [
    {
      "productId": "9NBLGGH5WVP6",
      "skuId": "0010"
    }
  ],
  "productTypes": [
    "UnmanagedConsumable"
  ],
  "validityType": "All"
}
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Parameter         | type                     | BESCHREIBUNG          | Erforderlich |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | Zeichenfolge                   | Bei mehreren Produktgruppen wird dieses Token zurückgegeben, sobald das Seitenlimit erreicht ist. Sie können dieses Fortsetzungstoken in nachfolgenden Aufrufen angeben, um verbleibende Produkte abzurufen. | Nein       |
| items             | CollectionItemContractV6 | Ein Array von Produkten für den angegebenen Benutzer. Weitere Informationen finden Sie in der folgenden Tabelle.        | Nein       |


Das CollectionItemContractV6-Objekt enthält die folgenden Parameter.

| Parameter            | type               | BESCHREIBUNG            | Erforderlich |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | datetime           | Das Datum, an dem der Benutzer den Artikel erworben hat.                  | Yes      |
| campaignId           | Zeichenfolge             | Die Kampagnen-ID, die beim Kauf dieses Artikels angegeben wurde.                  | Nein       |
| devOfferId           | Zeichenfolge             | Die Angebots-ID aus einem In-App-Kauf.              | Nein       |
| endDate              | datetime           | Das Enddatum des Artikels.              | Yes      |
| fulfillmentData      | List<string>       | –         | Nein       |
| inAppOfferToken      | Zeichenfolge             | Die vom Entwickler angegebene Produkt-ID-Zeichenfolge, die dem Element in Partner Center zugewiesen wird. Ein Beispiel für eine Produkt-ID ist *product123*. | Nein       |
| itemId               | Zeichenfolge             | Eine ID zur Unterscheidung dieses Sammlungselements von anderen Artikeln des Benutzers. Diese ID ist pro Produkt eindeutig.   | Yes      |
| localTicketReference | Zeichenfolge             | Die ID der zuvor bereitgestellten *localticketreferenzierung* im Anforderungs Text.                  | Yes      |
| modifiedDate         | datetime           | Das Datum, an dem dieser Artikel zuletzt geändert wurde.              | Yes      |
| orderId              | Zeichenfolge             | Die Auftrags-ID, mit der dieser Artikel erworben wurde (sofern angegeben).              | Nein       |
| orderLineItemId      | Zeichenfolge             | Die Position des Auftrags, für den dieser Artikel erworben wurde (sofern vorhanden).              | Nein       |
| ownershipType        | Zeichenfolge             | Die Zeichenfolge *ownedbybegünstigter*.   | Yes      |
| productId            | Zeichenfolge             | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities) im Microsoft Store Katalog. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.          | Yes      |
| productType          | Zeichenfolge             | Einer der folgenden Produkttypen: **Application**, **Durable** oder **UnmanagedConsumable**.        | Yes      |
| purchasedCountry     | Zeichenfolge             | –   | Nein       |
| purchaser            | IdentityContractV6 | Die Identität des Artikelkäufers (sofern vorhanden). Details zu diesem Objekt finden Sie weiter unten.        | Nein       |
| quantity             | number             | Die Artikelmenge. Derzeit beträgt die Menge immer 1.      | Nein       |
| skuId                | Zeichenfolge             | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) des Produkts im Microsoft Store Katalog. Eine Beispiel-Speicher-ID für eine SKU ist 0010.     | Yes      |
| skuType              | Zeichenfolge             | Der SKU-Typ. Mögliche Werte sind **Trial**, **Full** und **Rental**.        | Yes      |
| startDate            | datetime           | Das Datum, ab dem der Artikel gültig ist.       | Yes      |
| status               | Zeichenfolge             | Der Status des Elements. Mögliche Werte sind **Active**, **Expired**, **Revoked** und **Banned**.    | Yes      |
| tags                 | List<string>       | N/V    | Ja      |
| transactionId        | guid               | Die aus dem Artikelkauf resultierende Transaktions-ID. Kann verwendet werden, um zu melden, dass der Artikelkauf abgewickelt wurde.      | Yes      |


Das IdentityContractV6-Objekt enthält die folgenden Parameter.

| Parameter     | type   | BESCHREIBUNG                                                                        | Erforderlich |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | Zeichenfolge | Enthält den Wert *pub*.                                                      | Yes      |
| identityValue | Zeichenfolge | Der Zeichen folgen Wert der *publisheruserid* aus dem angegebenen Microsoft Store ID-Schlüssel. | Yes      |


### <a name="response-example"></a>Beispielantwort

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
  "items" : [
    {
      "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
      "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
      "endDate" : "9999-12-31T23:59:59.9999999+00:00",
      "fulfillmentData" : [],
      "inAppOfferToken" : "consumable2",
      "itemId" : "4b8fbb13127a41f299270ea668681c1d",
      "localTicketReference" : "1055521810674918",
      "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
      "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
      "ownershipType" : "OwnedByBeneficiary",
      "productId" : "9NBLGGH5WVP6",
      "productType" : "UnmanagedConsumable",
      "purchaser" : {
        "identityType" : "pub",
        "identityValue" : "user123"
      },
      "skuId" : "0010",
      "skuType" : "Full",
      "startDate" : "2015-09-22T19:22:51.2068724+00:00",
      "status" : "Active",
      "tags" : [],
      "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
    }
  ]
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Melden von konsumierbaren Produkten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
* [Verlängern eines Microsoft Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
