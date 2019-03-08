---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um den Abrechnungszustand eines Abonnements für einen Benutzer zu ändern.
title: Ändern des Abrechnungszustands eines Abonnements für einen Benutzer
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Einkaufs-API, Abonnements
ms.localizationpriority: medium
ms.openlocfilehash: 9e4cf27331a218c0c0ef06ee1a80c141b889504a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607835"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>Ändern des Abrechnungszustands eines Abonnements für einen Benutzer

Verwenden Sie diese Methode in der Microsoft Store-Einkaufs-API, um den Abrechnungszustand eines Abonnement-Add-Ons für einen bestimmten Benutzer zu ändern. Sie können ein Abonnement abbrechen, erweitern, erstatten oder die automatische Verlängerung deaktivieren.

> [!NOTE]
> Diese Methode kann nur über Entwicklerkonten verwendet werden, die von Microsoft bereitgestellt wurden, um Abonnement-Add-Ons für UWP-Apps (Universelle Windows-Plattform) erstellen zu können. Abonnement-Add-Ons sind derzeit für die meisten Entwicklerkonten nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
* Ein Microsoft Store-ID-Schlüssel, der die Identität des Benutzers darstellt, der über eine Berechtigung für das Abonnement verfügt, das Sie ändern möchten.

Weitere Informationen finden Sie unter [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | Typ   | Beschreibung   |
|----------------|--------|-------------|
| Autorisierung  | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.                           |
| Host           | string | Muss auf den Wert **purchase.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | string | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name         | Typ  | Beschreibung   |  Erforderlich  |
|----------------|--------|-------------|-----------|
| recurrenceId | string | Die ID des Abonnements, das Sie ändern möchten. Rufen Sie zum Abrufen dieser ID den [rufen Sie die Abonnements für einen Benutzer](get-subscriptions-for-a-user.md) Methode, identifizieren Sie den Eintrag der Antwort-Text, der stellt dar, die Abonnement-Add-On, die Sie ändern möchten, und verwenden Sie den Wert der **Id** Feld für den Eintrag.     | Ja      |


### <a name="request-body"></a>Anforderungstext

| Feld      | Typ   | Beschreibung   | Erforderlich |
|----------------|--------|---------------|----------|
| b2bKey         | string | Der [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, dessen Abonnement Sie ändern möchten.     | Ja      |
| changeType     | string |  Eine der folgenden Zeichenfolgen, die die Art der Änderung angibt, die Sie vornehmen möchten:<ul><li>**Abbrechen**: Bricht das Abonnement ab.</li><li>**Erweitern Sie**: Erweitert das Abonnement an. Wenn Sie diesen Wert angeben, müssen Sie auch den Parameter *extensionTimeInDays* hinzufügen.</li><li>**Erstatten Sie**: Rückerstattungen das Abonnement, für dem Kunden.</li><li>**ToggleAutoRenew**: Deaktiviert die automatische Verlängerung des Abonnements. Wenn die automatische Verlängerung für das Abonnement derzeit deaktiviert ist, hat dieser Wert keine Auswirkung.</li></ul>   | Ja      |
| extensionTimeInDays  | string  | Wenn der Parameter *changeType* den Wert **Extend** aufweist, gibt dieser Parameter die Anzahl von Tagen an, um die das Abonnement verlängert wird. |  „Ja“, wenn *changeType* den Wert **Extend** aufweist; andernfalls „Nein“.  ||


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie anhand dieser Methode der Abonnementzeitraum um 5 Tage verlängert wird. Ersetzen Sie den Wert *b2bKey* durch den [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, dessen Abonnement Sie ändern möchten.

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

Diese Methode gibt einen JSON-Antworttext zurück, der Informationen über das geänderte Abonnement-Add-On enthält, einschließlich der bearbeiteten Felder. Das folgende Beispiel zeigt einen Antworttext für diese Methode.

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
| productId | string | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für das [Produkt](in-app-purchases-and-trials.md#products-skus-and-availabilities), die das Abonnement-Add-On im Microsoft Store-Katalog darstellt. Ein Beispiel für eine Store-ID für ein Produkt ist 9NBLGGH42CFD.     |
| skuId | string |  Die [Store-ID](in-app-purchases-and-trials.md#store-ids) für die [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities), die das Abonnement-Add-On im Microsoft Store-Katalog darstellt. Ein Beispiel für eine Store-ID für eine SKU ist 0010.    |
| startTime | string |  Startdatum und -uhrzeit des Abonnements im Format ISO 8601.     |
| recurrenceState | string  |  Einer der folgenden Werte:<ul><li>**None**:&nbsp;&nbsp;Gibt ein unbefristetes Abonnement an.</li><li>**Active**:&nbsp;&nbsp;Das Abonnement ist aktiv und der Benutzer ist zur Verwendung der Dienste berechtigt.</li><li>**Inactive**:&nbsp;&nbsp;Das Abonnement hat das Ablaufdatum überschritten und der Benutzer hat die Option zur automatischen Verlängerung des Abonnements deaktiviert.</li><li>**Canceled**:&nbsp;&nbsp;Das Abonnement wurde absichtlich vor dem Ablaufdatum beendet, mit oder ohne Rückerstattung.</li><li>**InDunning**:&nbsp;&nbsp;Das Abonnement befindet sich im *Mahnstatus* (d. h. das Abonnement läuft in Kürze ab und Microsoft versucht, die Mittel zur automatischen Verlängerung des Abonnements zu akquirieren).</li><li>**Failed**:&nbsp;&nbsp;Der Mahnungszeitraum ist abgelaufen und das Abonnement konnte trotz mehrerer Versuche nicht verlängert werden.</li></ul><p>**Hinweis:**</p><ul><li>**Inactive**/**Canceled**/**Failed** sind abschließende Zustände. Wenn ein Abonnement in einen dieser Status wechselt, muss der Benutzer das Abonnement erneut kaufen, um es wieder zu aktivieren. Der Benutzer ist nicht berechtigt, die Dienste in diesen Zuständen zu verwenden.</li><li>Wenn ein Abonnement sich im Status **Canceled** befindet, wird die Ablaufzeit mit dem Datum und der Uhrzeit des Abbruchs aktualisiert.</li><li>Die ID des Abonnements bleibt während der gesamten Gültigkeitsdauer unverändert. Sie wird nicht geändert, wenn die Option zur automatischen Verlängerung aktiviert oder deaktiviert wird. Wenn ein Benutzer ein Abonnement nach Erreichen eines abschließenden Zustands erneut kauft, wird eine neue Abonnement-ID erstellt.</li><li>Die Abonnement-ID sollte bei allen Aktionen verwendet werden, die auf ein individuelles Abonnement angewendet werden.</li><li>Wenn ein Benutzer ein Abonnement nach der Kündigung bzw. Einstellung erneut erwirbt, werden beim Anfordern der Ergebnisse zum betreffenden Benutzer zwei Einträge angezeigt: ein Eintrag mit der alten Abonnement-ID in einem abschließenden Zustand und ein Eintrag mit der neuen Abonnement-ID in einem aktiven Zustand.</li><li>Es empfiehlt sich, die Werte „recurrenceState“ und „expirationTime“ stets zu überprüfen, da Aktualisierungen auf „recurrenceState“ u. U. einige Minuten (gelegentlich auch Stunden) verzögert erfolgen.       |
| cancellationDate | string   |  Datum und Uhrzeit, an dem bzw. zu der das Abonnement des Benutzers storniert wurde, im Format ISO 8601.     |


## <a name="related-topics"></a>Verwandte Themen


* [Verwalten von Product-Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md)
* [Rufen Sie die Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)
* [Abfrage von Produkten](query-for-products.md)
* [Verbrauchsartikeln gemeldet werden, weil erfüllt](report-consumable-products-as-fulfilled.md)
* [Erneuern Sie einen Microsoft Store-ID-Schlüssel](renew-a-windows-store-id-key.md)
