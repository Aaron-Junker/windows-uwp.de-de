---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: Verwenden Sie diese Methode in der Microsoft Store Purchase-API, um den Abrechnungs Status eines Abonnements für einen Benutzer zu ändern.
title: Ändern des Abrechnungszustands eines Abonnements für einen Benutzer
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Purchase API, Abonnements
ms.localizationpriority: medium
ms.openlocfilehash: 4d2558dec8422e198477c30097c20a875addeea4
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619496"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>Ändern des Abrechnungszustands eines Abonnements für einen Benutzer

Verwenden Sie diese Methode in der Microsoft Store Purchase-API, um den Abrechnungs Status eines Abonnement-Add-Ins für einen bestimmten Benutzer zu ändern. Sie können die automatische Verlängerung eines Abonnements abbrechen, erweitern, Rück erstatten oder deaktivieren.

> [!NOTE]
> Diese Methode kann nur von Entwickler Konten verwendet werden, die von Microsoft bereitgestellt wurden, um Abonnement-Add-ons für universelle Windows-Plattform-Apps (UWP) erstellen zu können. Add-ons für Abonnements sind zurzeit für die meisten Entwickler Konten nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD Zugriffs Token, das den Wert für den Zielgruppen-Uri aufweist `https://onestore.microsoft.com` .
* Ein Microsoft Store ID-Schlüssel, der die Identität des Benutzers darstellt, der über eine Berechtigung für das Abonnement verfügt, das Sie ändern möchten.

Weitere Informationen finden Sie unter [Verwalten von Produkt Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | type   | BESCHREIBUNG   |
|----------------|--------|-------------|
| Authorization  | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;.                           |
| Host           | Zeichenfolge | Muss auf den Wert " **Purchase.MP.Microsoft.com**" festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | Zeichenfolge | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name         | type  | BESCHREIBUNG   |  Erforderlich  |
|----------------|--------|-------------|-----------|
| recurrenceid | Zeichenfolge | Die ID des Abonnements, das Sie ändern möchten. Um diese ID abzurufen, nennen Sie die Methode [Get Abonnements for a User](get-subscriptions-for-a-user.md) , identifizieren Sie den Eintrag für den Antworttext, der das zu ändernde Abonnement-Add-on darstellt, und verwenden Sie den Wert des **ID** -Felds für den Eintrag.     | Yes      |


### <a name="request-body"></a>Anforderungstext

| Feld      | type   | BESCHREIBUNG   | Erforderlich |
|----------------|--------|---------------|----------|
| b2bKey         | Zeichenfolge | Der [Microsoft Store ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4) , der die Identität des Benutzers darstellt, dessen Abonnement Sie ändern möchten.     | Yes      |
| ChangeType     | Zeichenfolge |  Eine der folgenden Zeichen folgen, die den Typ der Änderung identifiziert, die Sie vornehmen möchten:<ul><li>**Abbrechen**: bricht das Abonnement ab.</li><li>**Erweitern**: erweitert das Abonnement. Wenn Sie diesen Wert angeben, müssen Sie auch den *extensiontimeindays* -Parameter einschließen.</li><li>**Erstattung**: gibt das Abonnement für den Kunden zurück.</li><li>**Toggleautorenew**: deaktiviert die automatische Verlängerung des Abonnements. Wenn die automatische Verlängerung für das Abonnement zurzeit deaktiviert ist, führt dieser Wert nichts aus.</li></ul>   | Yes      |
| extensiontimin Days  | Zeichenfolge  | Wenn der *ChangeType* -Parameter den Wert **erweitert** hat, gibt dieser Parameter die Anzahl der Tage an, für die das Abonnement erweitert werden soll. |  Ja, wenn *ChangeType* den Wert **erweitert** hat; Andernfalls Nein.  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie diese Methode verwendet wird, um den Abonnementzeitraum um 5 Tage zu verlängern. Ersetzen Sie den *b2bKey* -Wert durch den [Microsoft Store ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4) , der die Identität des Benutzers darstellt, dessen Abonnement Sie ändern möchten.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>Antwort

Diese Methode gibt einen JSON-Antworttext zurück, der Informationen über das geänderte Abonnement-Add-on enthält, einschließlich aller geänderten Felder. Das folgende Beispiel zeigt einen Antworttext für diese Methode.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
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

| Wert        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------|
| autorumew | Boolean |  Gibt an, ob das Abonnement für die automatische Verlängerung am Ende des aktuellen Abonnementzeitraums konfiguriert ist.   |
| beneficiary | Zeichenfolge |  Die ID des Empfängers der Berechtigung, die diesem Abonnement zugeordnet ist.   |
| expirationTime | Zeichenfolge | Das Datum und die Uhrzeit bis zum Ablauf des Abonnements im ISO 8601-Format. Dieses Feld ist nur verfügbar, wenn sich das Abonnement in einem bestimmten Zustand befindet. Die Ablaufzeit gibt normalerweise an, wenn der aktuelle Zustand abläuft. Bei einem aktiven Abonnement gibt das Ablaufdatum z. b. an, wann die nächste automatische Erneuerung stattfindet.    |
| expirationtimewithgrace | Zeichenfolge | Das Datum und die Uhrzeit, zu der das Abonnement abläuft, einschließlich der Toleranz Periode im ISO 8601-Format. Dieser Wert gibt an, wann der Benutzer den Zugriff auf das Abonnement verliert, nachdem das Abonnement nicht automatisch verlängert werden konnte.    |
| id | Zeichenfolge |  Die ID des Abonnements. Verwenden Sie diesen Wert, um anzugeben, welches Abonnement Sie ändern möchten, wenn Sie den [Abrechnungs Zustand eines Abonnements für eine Benutzer Methode ändern](change-the-billing-state-of-a-subscription-for-a-user.md) .    |
| istrial | Boolean |  Gibt an, ob das Abonnement eine Testversion ist.     |
| lastModified | Zeichenfolge |  Das Datum und die Uhrzeit der letzten Änderung des Abonnements im ISO 8601-Format.      |
| market | Zeichenfolge | Der Ländercode (im 2-Buch-Format ISO 3166-1 Alpha-2), in dem der Benutzer das Abonnement erworben hat.      |
| productId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities) , das das Abonnement-Add-on im Microsoft Store Katalog darstellt. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.     |
| skuId | Zeichenfolge |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) , die das Add-on des Abonnements für den Microsoft Store Katalog darstellt. Eine Beispiel-Speicher-ID für eine SKU ist 0010.    |
| startTime | Zeichenfolge |  Das Startdatum und die Start Uhrzeit für das Abonnement im ISO 8601-Format.     |
| RecurrenceState | Zeichenfolge  |  Einer der folgenden Werte:<ul><li>**None**: &nbsp; &nbsp; gibt ein unbefristete Abonnement an.</li><li>**Aktiv**: &nbsp; &nbsp; das Abonnement ist aktiv, und der Benutzer ist berechtigt, die Dienste zu verwenden.</li><li>**Inaktiv**: &nbsp; &nbsp; das Abonnement liegt nach dem Ablaufdatum, und der Benutzer hat die Option zur automatischen Verlängerung für das Abonnement deaktiviert.</li><li>**Abgebrochen**: &nbsp; &nbsp; das Abonnement wurde absichtlich vor dem Ablaufdatum mit oder ohne Rückerstattung beendet.</li><li>**Indunning**: &nbsp; &nbsp; das Abonnement befindet sich in *Dunning* (das heißt, das Abonnement läuft bald ab, und Microsoft versucht, Geld zu erwerben, um das Abonnement automatisch zu erneuern).</li><li>Fehler: &nbsp; &nbsp; der Gültigkeits Zeitraum ist abgelaufen, und das Abonnement konnte nach mehreren versuchen nicht verlängert werden.</li></ul><p>**Hinweis:**</p><ul><li>**Inaktiv** / **Abgebrochen** / **Failed** sind Terminal Zustände. Wenn ein Abonnement in einen dieser Zustände gelangt, muss der Benutzer das Abonnement erneut erwerben, um es erneut zu aktivieren. Der Benutzer ist nicht berechtigt, die Dienste in diesen Zuständen zu verwenden.</li><li>Wenn ein Abonnement **abgebrochen** wird, wird der ExpirationTime-Wert mit dem Datum und der Uhrzeit des Abbruchs aktualisiert.</li><li>Die ID des Abonnements bleibt während der gesamten Lebensdauer unverändert. Wenn die Option für die automatische Verlängerung aktiviert oder deaktiviert ist, wird Sie nicht geändert. Wenn ein Benutzer ein Abonnement nach dem Erreichen eines Endzustands erneut erwirbt, wird eine neue Abonnement-ID erstellt.</li><li>Die ID eines Abonnements sollte verwendet werden, um jeden Vorgang für ein einzelnes Abonnement auszuführen.</li><li>Wenn ein Benutzer ein Abonnement erneut erwirbt, nachdem es abgebrochen oder zurückgesetzt wurde, erhalten Sie beim Abfragen der Ergebnisse für den Benutzer zwei Einträge: einen mit der alten Abonnement-ID in einem Endzustand und einen, der die neue Abonnement-ID im aktiven Status aufweist.</li><li>Es ist immer eine bewährte Vorgehensweise, rekurrencestate und ExpirationTime zu überprüfen, da Aktualisierungen von RecurrenceState möglicherweise um ein paar Minuten (oder gelegentlich Stunden) verzögert werden.       |
| cancellationdate | Zeichenfolge   |  Das Datum und die Uhrzeit, zu der das Abonnement des Benutzers abgebrochen wurde, im ISO 8601-Format.     |


## <a name="related-topics"></a>Zugehörige Themen


* [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Abrufen von Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)
* [Produktabfrage](query-for-products.md)
* [Melden von konsumierbaren Produkten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Verlängern eines Microsoft Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
