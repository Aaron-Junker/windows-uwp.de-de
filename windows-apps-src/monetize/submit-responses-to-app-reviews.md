---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Verwenden Sie diese Methode in der Microsoft Store-Rezensions-API zum Senden von Antworten auf Rezensionen Ihrer App.
title: Übermitteln von Antworten auf Rezensionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Rezensions-API, Add-On-Käufe
ms.localizationpriority: medium
ms.openlocfilehash: c08dcda52940f0218b6fdb5be147f058eca7479a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623835"
---
# <a name="submit-responses-to-reviews"></a>Übermitteln von Antworten auf Rezensionen


Verwenden Sie diese Methode in der Microsoft Store-Rezensions-API, um Antworten auf Rezensionen Ihrer App zu senden. Wenn Sie diese Methode aufrufen, müssen Sie die IDs der Rezensionen angeben, auf die Sie antworten möchten. Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).

Beim Übermitteln von Rezensionen können Kunden festlegen, dass sie keine Antworten auf ihre Rezension erhalten möchten. Wenn Sie versuchen, auf eine Rezension zu antworten, für die der Kunde ausgewählt hat, keine Antworten zu erhalten, wird im Antworttext dieser Methode angegeben, dass der Antwortversuch fehlgeschlagen ist. Vor dem Aufrufen dieser Methode können Sie mit der Methode [Antwortinformationen für App-Rezensionen abrufen](get-response-info-for-app-reviews.md) ermitteln, ob Sie auf eine bestimmte Rezension antworten dürfen.

> [!NOTE]
> Zusätzlich zur Verwendung dieser Methode um programmgesteuert auf Überprüfungen zu reagieren, Sie können alternativ reagieren auf Überprüfungen [über Partner Center](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](respond-to-reviews-using-windows-store-services.md#prerequisites) für die Microsoft Store-Rezensions-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die IDs der Rezensionen ab, auf die Sie antworten möchten. Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).

## <a name="request"></a>Anfordern

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Diese Methode hat keine Anforderungsparameter.


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Werte:

| Wert        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------|
| Responses | array | Ein Array von Objekten, die die Antwortdaten enthalten, die Sie senden möchten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle. |


Jedes Objekt im *Responses*-Array enthält die folgenden Werte:

| Wert        | Typ   | Beschreibung           |  Erforderlich  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | string |  Die Store-ID der App, auf deren Rezension Sie antworten möchten. Die Store-ID ist verfügbar, auf die [Seite "App-Identität"](../publish/view-app-identity-details.md) von Partner Center. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8.   |  Ja  |
| ReviewId | string |  Die ID der Rezension, auf die Sie antworten möchten (dies ist eine GUID). Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).   |  Ja  |
| ResponseText | string | Die Antwort, die Sie senden möchten. Ihre Antwort muss [diesen Richtlinien](../publish/respond-to-customer-reviews.md#guidelines-for-responses) entsprechen.   |  Ja  |
| SupportEmail | string | Die Support-E-Mail-Adresse Ihrer App, über die der Kunde Sie direkt kontaktieren kann. Dies muss eine gültige E-Mail-Adresse sein.     |  Ja  |
| IsPublic | Boolesch |  Bei Angabe von **"true"**, Ihre Antwort wird in Ihrer app-Store auflisten, direkt unterhalb des Kunden überprüfen, angezeigt und für alle Kunden sichtbar. Bei Angabe von **"false"** und der Benutzer noch nicht Out-of-e-Mail-Antworten empfangen hat, Ihre Antwort wird an den Kunden per e-Mail gesendet werden und wird nicht für andere Kunden in Ihrer app-Store-Liste angezeigt werden. Bei Angabe von **"false"** und der Benutzer hat entschieden, Empfang von e-Mail-Antworten, wird ein Fehler zurückgegeben.   |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie diese Methode verwendet wird, um Antworten auf mehrere Rezensionen zu senden.

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
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert        | Typ   | Beschreibung            |
|---------------|--------|---------------------|
| Ergebnis | array | Ein Array von Objekten, die Daten über jede Antwort enthalten, die Sie gesendet haben. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.  |


Jedes Objekt im *Result*-Array enthält die folgenden Werte:

| Wert        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | string |  Die Store-ID der App, auf deren Rezension Sie geantwortet haben. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8.   |
| ReviewId | string |  Die ID der Rezension, auf die Sie geantwortet haben. Dies ist eine GUID.   |
| Erfolgreich | string | Der Wert **true** gibt an, dass Ihre Antwort erfolgreich gesendet wurde. Der Wert **false** gibt an, dass Ihre Antwort nicht erfolgreich war.    |
| FailureReason | string | Wenn **Successful** **false** ist, dann enthält dieser Wert einen Grund für den Fehler. Wenn **Successful** **true** ist, dann ist dieser Wert leer.      |


### <a name="response-example"></a>Antwortbeispiel

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

## <a name="related-topics"></a>Verwandte Themen

* [Reagieren Sie auf Überprüfungen durch den Kunden über Partner Center](../publish/respond-to-customer-reviews.md)
* [Reagieren Sie auf Überprüfungen mithilfe von Microsoft Store services](respond-to-reviews-using-windows-store-services.md)
* [Abrufen von Antwort-Informationen für Bewertungen zu Apps](get-response-info-for-app-reviews.md)
* [Abrufen von app-Bewertungen](get-app-reviews.md)
