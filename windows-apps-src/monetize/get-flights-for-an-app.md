---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API zum Abrufen von flugverspätungen Paketinformationen für eine app, die mit Ihrem Partner Center-Konto registriert ist.
title: Flight-Pakete für eine App abrufen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Flight, Flight-Pakete
ms.localizationpriority: medium
ms.openlocfilehash: 6f55a218c0cb4f4964fe12eb8e2eeb5b504f6405
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334778"
---
# <a name="get-package-flights-for-an-app"></a>Flight-Pakete für eine App abrufen

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um die Flüge Paket für eine app aufzulisten, die mit Ihrem Partner Center-Konto registriert ist. Weitere Informationen zu Flight-Paketen finden Sie unter [Flight-Pakete](https://msdn.microsoft.com/windows/uwp/publish/package-flights).

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

|  Name  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  applicationId  |  String  |  Die Store-ID der App, für die Flight-Pakete abgerufen werden sollen. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Ja  |
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d. h. die Anzahl der zurückzugebenden Flight-Pakete). Wenn Ihr Konto über mehr Flight-Pakete als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an die URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie alle Flight-Pakete für eine App aufgelistet werden können.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie das erste Flight-Paket für eine App aufgelistet wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel zeigt einen JSON-Antworttext, der von einer erfolgreichen Anforderung für das erste Flight-Paket für eine App mit insgesamt drei Flight-Paketen zurückgegeben wird. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung       |
|------------|--------|---------------------|
| @nextLink  | String | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI `https://manage.devcenter.microsoft.com/v1.0/my/` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 2 festgelegt ist, für die App jedoch 4 Flight-Pakete vorhanden sind, enthält der Antworttext den @nextLink-Wert `applications/{applicationid}/listflights/?skip=2&top=2`, der angibt, dass Sie `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2` aufrufen können, um die nächsten 2 Flight-Pakete anzufordern. |
| Wert      | array  | Ein Array von Objekten, die Informationen zu Flight-Paketen für die angegebene App bereitstellen. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter [Flight-Ressource](get-app-data.md#flight-object).               |
| totalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage (d. h. die Gesamtanzahl der Flight-Pakete für die angegebene App).   |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Flight-Pakete gefunden. |
| 409  | Die app verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Alle apps abrufen](get-all-apps.md)
* [Erhalten Sie eine app](get-an-app.md)
* [Abrufen von Add-ons für eine app](get-add-ons-for-an-app.md)
