---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API zum Abrufen von Informationen zu allen apps, die mit Ihrem Partner Center-Konto registriert sind.
title: Abrufen aller Apps
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Apps
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641985"
---
# <a name="get-all-apps"></a>Abrufen aller Apps


Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API zum Abrufen von Daten für die apps, die mit Ihrem Partner Center-Konto registriert sind.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Alle Anforderungsparameter sind optional für diese Methode. Wenn Sie diese Methode ohne Parameter aufrufen, wird die Antwort enthält Daten für die ersten 10-apps, die mit Ihrem Konto registriert sind.

|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d. h. die Anzahl der zurückzugebenden Apps). Wenn Ihr Konto über mehr Apps als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an den URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die ersten 10 Apps abgerufen werden können, die für Ihr Konto registriert sind.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie Informationen über alle Apps abgerufen werden können, die für Ihr Konto registriert wurden. Rufen Sie zuerst die Top 10-apps:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Dann rekursiv aufrufen `GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}` bis `{@nextlink}` ist null oder nicht vorhanden ist, in der Antwort. Zum Beispiel:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Wenn Sie bereits die Gesamtzahl der apps Sie in Ihrem Konto verfügen wissen, können Sie einfach übergeben, auf diese Zahl in die **oben** Parameter zum Abrufen von Informationen zu allen Ihren apps.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Antwort

Im folgenden Beispiel wird der JSON-Antworttext veranschaulicht, der von einer erfolgreichen Anforderung für die ersten 10 Apps zurückgegeben wird, die für ein Entwicklerkonto mit insgesamt 21 Apps registriert wurden. Aus Platzgründen sind in diesem Beispiel nur die Daten für die ersten beiden Apps dargestellt, die von der Anforderung zurückgegeben werden. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | array  | Ein Array von Objekten, die Informationen zu jeder App enthalten, die für Ihr Konto registriert wurde. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unter [Anwendungsressource](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | string | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI `https://manage.devcenter.microsoft.com/v1.0/my/` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 10 festgelegt ist, für Ihr Konto jedoch 20 Apps registriert wurden, enthält der Antworttext den @nextLink-Wert `applications?skip=10&top=10`, der angibt, dass Sie `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10` aufrufen können, um die nächsten 10 Apps anzufordern. |
| totalCount | int    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage (d. h. die Gesamtanzahl von Apps, die für Ihr Konto registriert wurden).                                                |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Apps gefunden. |
| 407  | Partner Center-Features, die von den apps verwendet [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Erhalten Sie eine app](get-an-app.md)
* [Paket Flüge für eine app zu erhalten.](get-flights-for-an-app.md)
* [Abrufen von Add-ons für eine app](get-add-ons-for-an-app.md)
