---
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API zum Abrufen aller-Add-On-Daten für alle apps, die mit Ihrem Partner Center-Konto registriert sind.
title: Abrufen aller Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Add-Ons, In-App-Produkte, IAPs
ms.localizationpriority: medium
ms.openlocfilehash: 84b55ea8bc62955e3556fcff4e8d608738eb59ce
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334428"
---
# <a name="get-all-add-ons"></a>Abrufen aller Add-Ons

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API zum Abrufen von Daten für alle Add-ons für alle apps, die mit Ihrem Partner Center-Konto registriert sind.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Alle Anforderungsparameter sind optional für diese Methode. Wenn Sie diese Methode ohne Parameter aufrufen, enthält die Antwort Daten für alle Add-Ons für alle Apps, die für Ihr Konto registriert sind.

|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  top  |  int  |  Die Anzahl von Elementen, die in der Anforderung zurückgegeben werden sollen (d. h. die Anzahl der zurückzugebenden-Add-Ons). Wenn Ihr Konto über mehr Add-Ons als der Wert verfügt, den Sie in der Abfrage festlegen, enthält der Antworttext einen relativen URI-Pfad, den Sie an den URI der Methode anfügen können, um die nächste Seite mit Daten anzufordern.  |  Nein  |
|  skip  |  int  |  Die Anzahl der Elemente, die in der Abfrage umgangen werden sollen, bevor die verbleibenden Elemente zurückgegeben werden. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Zum Beispiel werden bei top=10 und skip=0 die Elemente 1 bis 10 abgerufen, bei top=10 und skip=10 die Elemente 11 bis 20 und so weiter.  |  Nein  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie alle Add-On-Daten für alle Apps abgerufen werden können, die für Ihr Konto registriert sind.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

Im folgenden Beispiel wird veranschaulicht, wie nur die ersten 10 Add-Ons abgerufen werden.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Im folgenden Beispiel wird der JSON-Antworttext veranschaulicht, der von einer erfolgreichen Anforderung für die ersten 5 Add-Ons zurückgegeben wird, die für ein Entwicklerkonto mit insgesamt 1072 Add-Ons registriert wurden. Aus Platzgründen sind in diesem Beispiel nur die Daten für die ersten beiden Add-Ons dargestellt, die von der Anforderung zurückgegeben werden. Weitere Informationen zu den Werten im Antworttext finden Sie im folgenden Abschnitt.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | String | Wenn weitere Datenseiten vorhanden sind, enthält diese Zeichenfolge einen relativen Pfad, den Sie an den Basisanforderungs-URI `https://manage.devcenter.microsoft.com/v1.0/my/` zum Anfordern der nächsten Datenseite anfügen können. Wenn beispielsweise der Parameter *top* des anfänglichen Anforderungstexts auf 10 festgelegt ist, für Ihr Konto jedoch 100 Add-Ons registriert wurden, enthält der Antworttext den @nextLink-Wert `inappproducts?skip=10&top=10`, der angibt, dass Sie `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10` aufrufen können, um die nächsten 10 Add-Ons anzufordern. |
| Wert            | array  |  Ein Array, das die Objekte enthält, die Informationen über jedes Add-On bereitstellen. Weitere Informationen finden Sie unter [Add-On-Ressource](manage-add-ons.md#add-on-object).   |
| totalCount   | int  | Die Anzahl der App-Objekte im *value*-Array des Antworttexts.     |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Es wurden keine Add-Ons gefunden. |
| 409  | Verwenden Sie die apps oder -Add-Ons Partner Center-Features, die [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten Sie Add-Ons Übermittlungen](manage-add-on-submissions.md)
* [Ein Add-on abzurufen](get-an-add-on.md)
* [Ein Add-on erstellen](create-an-add-on.md)
* [Add-on löschen](delete-an-add-on.md)
