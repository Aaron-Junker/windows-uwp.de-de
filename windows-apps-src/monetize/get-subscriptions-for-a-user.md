---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Verwenden Sie diese Methode in der Microsoft Store Purchase-API, um die Abonnements zu erhalten, für die ein bestimmter Benutzer berechtigt ist, zu verwenden.
title: Abrufen von Abonnements für einen Benutzer
ms.date: 07/10/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Purchase API, Abonnements
ms.localizationpriority: medium
ms.openlocfilehash: c77218cc7157e3d9a5508146395b284b57397d0f
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941796"
---
# <a name="get-subscriptions-for-a-user"></a>Abrufen von Abonnements für einen Benutzer

Verwenden Sie diese Methode in der Microsoft Store Purchase-API, um die Add-ons für Abonnements zu erhalten, für die ein bestimmter Benutzer berechtigt ist, zu verwenden.

> [!NOTE]
> Diese Methode kann nur von Entwickler Konten verwendet werden, die von Microsoft bereitgestellt wurden, um Abonnement-Add-ons für universelle Windows-Plattform-Apps (UWP) erstellen zu können. Add-ons für Abonnements sind zurzeit für die meisten Entwickler Konten nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD Zugriffs Token, das den Wert für den Zielgruppen-Uri aufweist `https://onestore.microsoft.com` .
* Ein Microsoft Store ID-Schlüssel, der die Identität des Benutzers darstellt, dessen Abonnements Sie erhalten möchten.

Weitere Informationen finden Sie unter [Verwalten von Produkt Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | type   | BESCHREIBUNG      |
|----------------|--------|-------------------|
| Authorization  | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;.                           |
| Host           | Zeichenfolge | Muss auf den Wert " **Purchase.MP.Microsoft.com**" festgelegt werden.                                            |
| Content-Length | Anzahl | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | Zeichenfolge | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter      | type   | BESCHREIBUNG     | Erforderlich |
|----------------|--------|-----------------|----------|
| b2bKey         | Zeichenfolge | Der [Microsoft Store ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4) , der die Identität des Benutzers darstellt, dessen Abonnements Sie erhalten möchten.  | Ja      |
| continuationToken |  Zeichenfolge     |  Wenn der Benutzer über Berechtigungen für mehrere Abonnements verfügt, gibt der Antworttext ein Fortsetzungs Token zurück, wenn das Seiten Limit erreicht ist. Geben Sie das Fortsetzungstoken hier bei nachfolgenden Aufrufen an, um die verbleibenden Produkte abzurufen.    | Nein      |
| pageSize       | Zeichenfolge | Die maximale Anzahl von Abonnements, die in einer Antwort zurückgegeben werden sollen. Der Standard ist 25.     |  Nein      |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie diese Methode verwendet wird, um die Add-ons für Abonnements zu erhalten, für die ein bestimmter Benutzer berechtigt ist, zu verwenden. Ersetzen Sie den *b2bKey* -Wert durch den [Microsoft Store ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4) , der die Identität des Benutzers darstellt, dessen Abonnements Sie erhalten möchten.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Antwort

Diese Methode gibt einen JSON-Antworttext zurück, der eine Auflistung von Datenobjekten enthält, die die Abonnement-Add-ons beschreiben, für die der Benutzer über Berechtigungen zur Verwendung verfügt. Das folgende Beispiel veranschaulicht den Antworttext für einen Benutzer, der über eine Berechtigung für ein Abonnement verfügt.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>Antworttext

Der Antworttext enthält die folgenden Daten.

| Wert        | type   | BESCHREIBUNG            |
|---------------|--------|---------------------|
| items | array | Ein Array von-Objekten, die Daten zu jedem Abonnement-Add-on enthalten, das der angegebene Benutzer verwenden darf. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.  |


Jedes-Objekt im *Items* -Array enthält die folgenden Werte.

| Wert        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------|
| autorumew | Boolesch |  Gibt an, ob das Abonnement für die automatische Verlängerung am Ende des aktuellen Abonnementzeitraums konfiguriert ist.   |
| beneficiary | Zeichenfolge |  Die ID des Empfängers der Berechtigung, die diesem Abonnement zugeordnet ist.   |
| expirationTime | Zeichenfolge | Das Datum und die Uhrzeit bis zum Ablauf des Abonnements im ISO 8601-Format. Dieses Feld ist nur verfügbar, wenn sich das Abonnement in einem bestimmten Zustand befindet. Die Ablaufzeit gibt normalerweise an, wenn der aktuelle Zustand abläuft. Bei einem aktiven Abonnement gibt das Ablaufdatum z. b. an, wann die nächste automatische Erneuerung stattfindet.    |
| expirationtimewithgrace | Zeichenfolge | Das Datum und die Uhrzeit, zu der das Abonnement abläuft, einschließlich der Toleranz Periode im ISO 8601-Format. Dieser Wert gibt an, wann der Benutzer den Zugriff auf das Abonnement verliert, nachdem das Abonnement nicht automatisch verlängert werden konnte.    |
| id | Zeichenfolge |  Die ID des Abonnements. Verwenden Sie diesen Wert, um anzugeben, welches Abonnement Sie ändern möchten, wenn Sie den [Abrechnungs Zustand eines Abonnements für eine Benutzer Methode ändern](change-the-billing-state-of-a-subscription-for-a-user.md) .    |
| istrial | Boolesch |  Gibt an, ob das Abonnement eine Testversion ist.     |
| lastModified | Zeichenfolge |  Das Datum und die Uhrzeit der letzten Änderung des Abonnements im ISO 8601-Format.      |
| market | Zeichenfolge | Der Ländercode (im 2-Buch-Format ISO 3166-1 Alpha-2), in dem der Benutzer das Abonnement erworben hat.      |
| productId | Zeichenfolge |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities) , das das Abonnement-Add-on im Microsoft Store Katalog darstellt. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.     |
| skuId | Zeichenfolge |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) , die das Add-on des Abonnements für den Microsoft Store Katalog darstellt. Eine Beispiel-Speicher-ID für eine SKU ist 0010.    |
| startTime | Zeichenfolge |  Das Startdatum und die Start Uhrzeit für das Abonnement im ISO 8601-Format.     |
| RecurrenceState | Zeichenfolge  |  Einer der folgenden Werte:<ul><li>**None**: &nbsp; &nbsp; gibt ein unbefristete Abonnement an.</li><li>**Aktiv**: &nbsp; &nbsp; das Abonnement ist aktiv, und der Benutzer ist berechtigt, die Dienste zu verwenden.</li><li>**Inaktiv**: &nbsp; &nbsp; das Abonnement liegt nach dem Ablaufdatum, und der Benutzer hat die Option zur automatischen Verlängerung für das Abonnement deaktiviert.</li><li>**Abgebrochen**: &nbsp; &nbsp; das Abonnement wurde absichtlich vor dem Ablaufdatum mit oder ohne Rückerstattung beendet.</li><li>**Indunning**: &nbsp; &nbsp; das Abonnement befindet sich in *Dunning* (das heißt, das Abonnement läuft bald ab, und Microsoft versucht, Geld zu erwerben, um das Abonnement automatisch zu erneuern).</li><li>**Failed** Fehler: &nbsp; &nbsp; der Gültigkeits Zeitraum ist abgelaufen, und das Abonnement konnte nach mehreren versuchen nicht verlängert werden.</li></ul><p>**Hinweis:**</p><ul><li>**Inaktiv** / **Abgebrochen** / **Failed** sind Terminal Zustände. Wenn ein Abonnement in einen dieser Zustände gelangt, muss der Benutzer das Abonnement erneut erwerben, um es erneut zu aktivieren. Der Benutzer ist nicht berechtigt, die Dienste in diesen Zuständen zu verwenden.</li><li>Wenn ein Abonnement **abgebrochen** wird, wird der ExpirationTime-Wert mit dem Datum und der Uhrzeit des Abbruchs aktualisiert.</li><li>Die ID des Abonnements bleibt während der gesamten Lebensdauer unverändert. Wenn die Option für die automatische Verlängerung aktiviert oder deaktiviert ist, wird Sie nicht geändert. Wenn ein Benutzer ein Abonnement nach dem Erreichen eines Endzustands erneut erwirbt, wird eine neue Abonnement-ID erstellt.</li><li>Die ID eines Abonnements sollte verwendet werden, um jeden Vorgang für ein einzelnes Abonnement auszuführen.</li><li>Wenn ein Benutzer ein Abonnement erneut erwirbt, nachdem es abgebrochen oder zurückgesetzt wurde, erhalten Sie beim Abfragen der Ergebnisse für den Benutzer zwei Einträge: einen mit der alten Abonnement-ID in einem Endzustand und einen, der die neue Abonnement-ID im aktiven Status aufweist.</li><li>Es ist immer eine bewährte Vorgehensweise, rekurrencestate und ExpirationTime zu überprüfen, da Aktualisierungen von RecurrenceState möglicherweise um ein paar Minuten (oder gelegentlich Stunden) verzögert werden.       |
| cancellationdate | Zeichenfolge   |  Das Datum und die Uhrzeit, zu der das Abonnement des Benutzers abgebrochen wurde, im ISO 8601-Format.     |


## <a name="related-topics"></a>Verwandte Themen

* [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Ändern des Abrechnungszustands eines Abonnements für einen Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Produktabfrage](query-for-products.md)
* [Melden von konsumierbaren Produkten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Verlängern eines Microsoft Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
