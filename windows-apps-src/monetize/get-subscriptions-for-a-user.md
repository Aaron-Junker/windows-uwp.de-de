---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um Abonnements abzurufen, für die ein bestimmter Benutzer Nutzungsberechtigungen hat.
title: Abrufen von Abonnements für einen Benutzer
ms.date: 07/10/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Einkaufs-API, Abonnements
ms.localizationpriority: medium
ms.openlocfilehash: b568531ce0807ebc5be0d27a78b94547e8473ae6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651625"
---
# <a name="get-subscriptions-for-a-user"></a>Abrufen von Abonnements für einen Benutzer

Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um die Abonnement-Add-Ons abzurufen, für die ein bestimmter Benutzer Nutzungsberechtigungen hat.

> [!NOTE]
> Diese Methode kann nur über Entwicklerkonten verwendet werden, die von Microsoft bereitgestellt wurden, um Abonnement-Add-Ons für UWP-Apps (Universelle Windows-Plattform) erstellen zu können. Abonnement-Add-Ons sind derzeit für die meisten Entwicklerkonten nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
* Ein Microsoft Store-ID-Schlüssel, der die Identität des Benutzers darstellt, dessen Abonnements Sie abrufen möchten.

Weitere Informationen finden Sie unter [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | Typ   | Beschreibung      |
|----------------|--------|-------------------|
| Autorisierung  | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.                           |
| Host           | string | Muss auf den Wert **purchase.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter      | Typ   | Beschreibung     | Erforderlich |
|----------------|--------|-----------------|----------|
| b2bKey         | string | Der [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, dessen Abonnements Sie abrufen möchten.  | Ja      |
| continuationToken |  string     |  Wenn der Benutzer über Berechtigungen für mehrere Abonnements verfügt, gibt der Antworttext ein Fortsetzungstoken zurück, wenn das Seitenlimit erreicht wird. Geben Sie das Fortsetzungstoken hier bei nachfolgenden Aufrufen an, um die verbleibenden Produkte abzurufen.    | Nein      |
| pageSize       | string | Die maximale Anzahl von Abonnements, die in einer Antwort zurückgegeben werden sollen. Der Standardwert ist 25.     |  Nein      |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie mit dieser Methode die Abonnement-Add-Ons abrufen, für die ein bestimmter Benutzer Nutzungsberechtigungen hat. Ersetzen Sie den Wert *b2bKey* durch den [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, dessen Abonnements Sie abrufen möchten.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Antwort

Diese Methode gibt einen JSON-Antworttext mit einer Sammlung von Datenobjekten zurück, die die Abonnement-Add-Ons beschreiben, für die der Benutzer Nutzungsberechtigungen hat. Das folgende Beispiel zeigt den Antworttext für einen Benutzer, der über eine Berechtigung für ein Abonnement verfügt.

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

| Wert        | Typ   | Beschreibung            |
|---------------|--------|---------------------|
| items | array | Ein Array von Objekten, die Daten zu den einzelnen Abonnement-Add-Ons enthalten, für die der angegebene Benutzer Nutzungsberechtigungen hat. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.  |


Jedes Objekt im *items*-Array enthält die folgenden Werte:

| Wert        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | Boolesch |  Gibt an, ob das Abonnement für die automatische Verlängerung am Ende des aktuellen Abonnementzeitraums konfiguriert ist.   |
| beneficiary | string |  Die ID des Begünstigten für die Berechtigung, die diesem Abonnement zugeordnet ist.   |
| expirationTime | string | Datum und Uhrzeit, an dem bzw. zu der das Abonnement abläuft, im Format ISO 8601. Dieses Feld ist nur verfügbar, wenn sich das Abonnement in bestimmten Zuständen befindet. Die Ablaufzeit gibt in der Regel an, wann der aktuelle Zustand abläuft. Beispiel: Bei einem aktiven Abonnement gibt das Ablaufdatum an, wann die nächste automatische Verlängerung erfolgt.    |
| expirationTimeWithGrace | string | Das Datum und Uhrzeit, die das Abonnement abläuft, einschließlich der Toleranzperiode im ISO 8601-Format. Dieser Wert gibt an, wenn der Benutzer verlieren Zugriff auf das Abonnement nach dem das Abonnement nicht automatisch verlängert werden konnte.    |
| id | string |  Die ID des Abonnements. Verwenden Sie diesen Wert, um das Abonnement anzugeben, das Sie durch Aufrufen der Methode zum [Ändern des Abrechnungszustands eines Abonnements für einen Benutzer ](change-the-billing-state-of-a-subscription-for-a-user.md) ändern möchten.    |
| isTrial | Boolesch |  Gibt an, ob es sich bei dem Abonnement um eine Testversion handelt.     |
| lastModified | string |  Datum und Uhrzeit, an dem bzw. zu der das Abonnement zuletzt geändert wurde, im Format ISO 8601.      |
| market | string | Der Ländercode (ein aus zwei Buchstaben bestehendes ISO 3166-1-Alpha-2-Format), der vom Benutzer beim Kauf des Abonnement verwendet wurde.      |
| productId | string |  Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities), die das Abonnement-Add-On im Microsoft Store-Katalog darstellt. Ein Beispiel für eine Store-ID für ein Produkt ist 9NBLGGH42CFD.     |
| skuId | string |  Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities), die das Abonnement-Add-On im Microsoft Store-Katalog darstellt. Ein Beispiel für eine Store-ID für eine SKU ist 0010.    |
| startTime | string |  Startdatum und -uhrzeit des Abonnements im Format ISO 8601.     |
| recurrenceState | string  |  Einer der folgenden Werte:<ul><li>**None**:&nbsp;&nbsp;Gibt ein unbefristetes Abonnement an.</li><li>**Active**:&nbsp;&nbsp;Das Abonnement ist aktiv und der Benutzer ist zur Verwendung der Dienste berechtigt.</li><li>**Inactive**:&nbsp;&nbsp;Das Abonnement hat das Ablaufdatum überschritten und der Benutzer hat die Option zur automatischen Verlängerung des Abonnements deaktiviert.</li><li>**Canceled**:&nbsp;&nbsp;Das Abonnement wurde absichtlich vor dem Ablaufdatum beendet, mit oder ohne Rückerstattung.</li><li>**InDunning**:&nbsp;&nbsp;Das Abonnement befindet sich im *Mahnstatus* (d. h. das Abonnement läuft in Kürze ab und Microsoft versucht, die Mittel zur automatischen Verlängerung des Abonnements zu akquirieren).</li><li>**Failed**:&nbsp;&nbsp;Der Mahnungszeitraum ist abgelaufen und das Abonnement konnte trotz mehrerer Versuche nicht verlängert werden.</li></ul><p>**Hinweis:**</p><ul><li>**Inactive**/**Canceled**/**Failed** sind abschließende Zustände. Wenn ein Abonnement in einen dieser Status wechselt, muss der Benutzer das Abonnement erneut kaufen, um es wieder zu aktivieren. Der Benutzer ist nicht berechtigt, die Dienste in diesen Zuständen zu verwenden.</li><li>Wenn ein Abonnement sich im Status **Canceled** befindet, wird die Ablaufzeit mit dem Datum und der Uhrzeit des Abbruchs aktualisiert.</li><li>Die ID des Abonnements bleibt während der gesamten Gültigkeitsdauer unverändert. Sie wird nicht geändert, wenn die Option zur automatischen Verlängerung aktiviert oder deaktiviert wird. Wenn ein Benutzer ein Abonnement nach Erreichen eines abschließenden Zustands erneut kauft, wird eine neue Abonnement-ID erstellt.</li><li>Die Abonnement-ID sollte bei allen Aktionen verwendet werden, die auf ein individuelles Abonnement angewendet werden.</li><li>Wenn ein Benutzer ein Abonnement nach der Kündigung bzw. Einstellung erneut erwirbt, werden beim Anfordern der Ergebnisse zum betreffenden Benutzer zwei Einträge angezeigt: ein Eintrag mit der alten Abonnement-ID in einem abschließenden Zustand und ein Eintrag mit der neuen Abonnement-ID in einem aktiven Zustand.</li><li>Es empfiehlt sich, die Werte „recurrenceState“ und „expirationTime“ stets zu überprüfen, da Aktualisierungen auf „recurrenceState“ u. U. einige Minuten (gelegentlich auch Stunden) verzögert erfolgen.       |
| cancellationDate | string   |  Datum und Uhrzeit, an dem bzw. zu der das Abonnement des Benutzers storniert wurde, im Format ISO 8601.     |


## <a name="related-topics"></a>Verwandte Themen

* [Verwalten von Product-Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md)
* [Ändern Sie den Status eines Abonnements für einen Benutzer, die Gebühren](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Abfrage von Produkten](query-for-products.md)
* [Verbrauchsartikeln gemeldet werden, weil erfüllt](report-consumable-products-as-fulfilled.md)
* [Erneuern Sie einen Microsoft Store-ID-Schlüssel](renew-a-windows-store-id-key.md)
