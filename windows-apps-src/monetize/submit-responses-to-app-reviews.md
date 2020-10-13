---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Verwenden Sie diese Methode in der Microsoft Store Reviews-API, um Antworten auf Überprüfungen Ihrer APP zu senden.
title: Übermitteln von Antworten auf Rezensionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Reviews API, Add-on-Akquisitionen
ms.localizationpriority: medium
ms.openlocfilehash: 9cf62f5f619eba0431f1398a391eef03b47bb13d
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984636"
---
# <a name="submit-responses-to-reviews"></a>Übermitteln von Antworten auf Rezensionen


Verwenden Sie diese Methode in der Microsoft Store Reviews-API, um Programm gesteuert auf Überprüfungen Ihrer APP zu reagieren. Wenn Sie diese Methode aufgerufen haben, müssen Sie die IDs der Reviews angeben, auf die Sie reagieren möchten. Überprüfungs-IDs sind in den Antwortdaten der Methode [Get App Reviews](get-app-reviews.md) in der Microsoft Store Analytics-API und im [Offline Download](../publish/download-analytic-reports.md) des [Berichts Reviews](../publish/reviews-report.md)verfügbar.

Wenn ein Kunde einen Review sendet, kann er auswählen, dass er keine Antworten auf seine Überprüfung erhält. Wenn Sie versuchen, auf eine Überprüfung zu reagieren, für die der Kunde keinen Empfang von Antworten gewählt hat, gibt der Antworttext dieser Methode an, dass der Antwort Versuch nicht erfolgreich war. Bevor Sie diese Methode aufrufen, können Sie optional ermitteln, ob Sie auf eine bestimmte Überprüfung Antworten können, indem Sie die Methode [Get Response Info for App Reviews](get-response-info-for-app-reviews.md) verwenden.

> [!NOTE]
> Mit dieser Methode können Sie nicht nur Programm gesteuert auf Überprüfungen reagieren, sondern auch [mithilfe von Partner Center](../publish/respond-to-customer-reviews.md)auf Überprüfungen reagieren.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](respond-to-reviews-using-windows-store-services.md#prerequisites) für die Microsoft Store Reviews-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Holen Sie sich die IDs der Reviews, auf die Sie reagieren möchten. Überprüfungs-IDs sind in den Antwortdaten der Methode [Get App Reviews](get-app-reviews.md) in der Microsoft Store Analytics-API und im [Offline Download](../publish/download-analytic-reports.md) des [Berichts Reviews](../publish/reviews-report.md)verfügbar.

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Diese Methode hat keine Anforderungs Parameter.


### <a name="request-body"></a>Anforderungstext

Der Anforderungs Text weist die folgenden Werte auf.

| Wert        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------|
| Antworten | array | Ein Array von-Objekten, die die Antwortdaten enthalten, die Sie übermitteln möchten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |


Jedes Objekt im Array " *Antworten* " enthält die folgenden Werte:

| Wert        | type   | BESCHREIBUNG           |  Erforderlich  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | Zeichenfolge |  Die Speicher-ID der APP mit der Überprüfung, auf die Sie reagieren möchten. Die Store-ID ist auf der [Seite App-Identität](../publish/view-app-identity-details.md) von Partner Center verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8.   |  Ja  |
| Reviewid | Zeichenfolge |  Die ID der Überprüfung, auf die Sie reagieren möchten (Dies ist eine GUID). Überprüfungs-IDs sind in den Antwortdaten der Methode [Get App Reviews](get-app-reviews.md) in der Microsoft Store Analytics-API und im [Offline Download](../publish/download-analytic-reports.md) des [Berichts Reviews](../publish/reviews-report.md)verfügbar.   |  Ja  |
| Response Text | Zeichenfolge | Die Antwort, die Sie übermitteln möchten. Ihre Antwort muss [den folgenden Richtlinien](../publish/respond-to-customer-reviews.md#guidelines-for-responses)entsprechen.   |  Ja  |
| Supportemail | Zeichenfolge | Die Support-e-Mail-Adresse Ihrer APP, die der Kunde verwenden kann, um Sie direkt zu kontaktieren. Dies muss eine gültige e-Mail-Adresse sein.     |  Ja  |
| IsPublic | Boolean |  Wenn Sie " **true**" angeben, wird die Antwort in der Store-Liste Ihrer APP direkt unterhalb der Kunden Überprüfung angezeigt und ist für alle Kunden sichtbar. Wenn Sie " **false** " angeben und der Benutzer keine e-Mail-Antworten erhalten hat, wird die Antwort per e-Mail an den Kunden gesendet und ist nicht für andere Kunden in der Store-Liste Ihrer APP sichtbar. Wenn Sie **false** angeben und der Benutzer keine e-Mail-Antworten erhalten hat, wird ein Fehler zurückgegeben.   |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie diese Methode verwendet wird, um Antworten an verschiedene Überprüfungen zu senden.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": true
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": false
    }
  ]
}
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert        | type   | BESCHREIBUNG            |
|---------------|--------|---------------------|
| Ergebnis | array | Ein Array von-Objekten, die Daten zu jeder von Ihnen übermittelten Antwort enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.  |


Jedes-Objekt im *Ergebnis* Array enthält die folgenden Werte.

| Wert        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | Zeichenfolge |  Die Speicher-ID der APP mit der Überprüfung, auf die Sie geantwortet haben. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8.   |
| Reviewid | Zeichenfolge |  Die ID der Überprüfung, auf die Sie geantwortet haben. Dies ist eine GUID.   |
| Erfolgreich | Zeichenfolge | Der Wert **true** gibt an, dass die Antwort erfolgreich gesendet wurde. Der Wert **false** gibt an, dass die Antwort nicht erfolgreich war.    |
| FailureReason | Zeichenfolge | Wenn **erfolgreich** **false**ist, enthält dieser Wert einen Grund für den Fehler. Wenn **erfolgreich** **true**ist, ist dieser Wert leer.      |


### <a name="response-example"></a>Beispielantwort

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Reagieren auf Kunden Reviews mithilfe von Partner Center](../publish/respond-to-customer-reviews.md)
* [Antworten auf Überprüfungen mithilfe von Microsoft Store Services](respond-to-reviews-using-windows-store-services.md)
* [Antwortinformationen für App-Reviews erhalten](get-response-info-for-app-reviews.md)
* [Abrufen von App-Rezensionen](get-app-reviews.md)
