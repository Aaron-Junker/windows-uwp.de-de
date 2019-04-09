---
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um eine neue Add-on-Eingabe für eine app erstellen, die in Partner Center registriert ist.
title: Erstellen einer Add-On-Übermittlung
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Erstellen einer Add-On-Übermittlung, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: cbb093576badf5cd84b132cfb139db9da7d31991
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334928"
---
# <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um eine neue Add-on (auch bekannt als in-app-Produkt oder IAP) Eingabe für eine app zu erstellen, die mit Ihrem Partner Center-Konto registriert ist. Nachdem Sie erfolgreich eine neue Übermittlung mit dieser Methode erstellt haben, [aktualisieren Sie die Übermittlung](update-an-add-on-submission.md), um erforderliche Änderungen an den Übermittlungsdaten vorzunehmen, und führen Sie ein [Commit für die Übermittlung](commit-an-add-on-submission.md) zur Aufnahme und Veröffentlichung durch.

Weitere Informationen dazu, wie diese Methode zum Erstellen einer Add-On-Übermittlung mithilfe der Microsoft Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

> [!NOTE]
> Durch diese Methode wird eine Übermittlung für ein vorhandenes Add-On erstellt. Um ein Add-On zu erstellen, verwenden Sie die Methode zum [Erstellen eines Add-Ons](create-an-add-on.md).

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie ein Add-on für eine Ihrer apps. Sie können dies im Partner Center, oder Sie erreichen dies, indem die [ein Add-on erstellen](create-an-add-on.md) Methode.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions` |

### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |

### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | String | Erforderlich. Die Store-ID des Add-Ons, für das Sie eine Übermittlung erstellen möchten. Die Store-ID ist im Partner Center zur Verfügung und befindet sich in die Antwortdaten für Anforderungen an [ein Add-on erstellen](create-an-add-on.md) oder [Abrufen von Details des Add-On-](get-all-add-ons.md).  |

### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie eine neue Übermittlung für ein Add-On erstellen.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur neuen Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Add-On-Übermittlungsressource](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Übermittlung konnte nicht erstellt werden, da die Anforderung ungültig ist. |
| 409  | Die Übermittlung konnte aufgrund des aktuellen Status der app nicht erstellt werden, oder die app verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten Sie Add-Ons Übermittlungen](manage-add-on-submissions.md)
* [Erhalten Sie ein Add-on-Übermittlung](get-an-add-on-submission.md)
* [Übernehmen Sie ein Add-on-Übermittlung](commit-an-add-on-submission.md)
* [Aktualisieren einer-Add-On](update-an-add-on-submission.md)
* [Löschen Sie ein Add-on-Übermittlung](delete-an-add-on-submission.md)
* [Abrufen des Status einer Übermittlung-Add-On](get-status-for-an-add-on-submission.md)
