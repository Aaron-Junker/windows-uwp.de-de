---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um paketflight-Informationen für eine APP abzurufen, die bei Ihrem Partner Center-Konto registriert ist.
title: Flight-Pakete für eine App abrufen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Flüge, Paket Flüge
ms.localizationpriority: medium
ms.openlocfilehash: a5cfe7aa579e5d050ddf808e35c06d52c84eeca5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171434"
---
# <a name="get-package-flights-for-an-app"></a>Flight-Pakete für eine App abrufen

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um die Paket Flüge für eine APP aufzulisten, die bei Ihrem Partner Center-Konto registriert ist. Weitere Informationen zu Flight-Paketen finden Sie unter [Flight-Pakete](../publish/package-flights.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

|  Name  |  Typ  |  BESCHREIBUNG  |  Erforderlich  |
|------|------|------|------|
|  applicationId  |  Zeichenfolge  |  Die Store-ID der App, für die Flight-Pakete abgerufen werden sollen. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |  Ja  |
|  top  |  INT  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d. h. die Anzahl der zurückzugebenden Flight-Pakete). Wenn Ihr Konto über mehr Flight-Pakete als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an die URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  INT  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |


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

| Wert      | Typ   | BESCHREIBUNG       |
|------------|--------|---------------------|
| @nextLink  | Zeichenfolge | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI `https://manage.devcenter.microsoft.com/v1.0/my/` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der *Top* -Parameter des ersten Anforderungs Texts auf 2 festgelegt ist, für die APP jedoch vier Paket-Flüge vorhanden sind, enthält der Antworttext den @nextLink Wert. `applications/{applicationid}/listflights/?skip=2&top=2` Dies bedeutet, dass Sie aufrufen können, `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2` um die nächsten zwei Paket-Flüge anzufordern. |
| value      | array  | Ein Array von Objekten, die Informationen zu Flight-Paketen für die angegebene App bereitstellen. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter [Flight-Ressource](get-app-data.md#flight-object).               |
| totalCount | INT    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage (d. h. die Gesamtanzahl der Flight-Pakete für die angegebene App).   |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 404  | Es wurden keine Flight-Pakete gefunden. |
| 409  | Die APP verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird.  |


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen aller Apps](get-all-apps.md)
* [Abrufen einer App](get-an-app.md)
* [Add-Ons für eine App abrufen](get-add-ons-for-an-app.md)