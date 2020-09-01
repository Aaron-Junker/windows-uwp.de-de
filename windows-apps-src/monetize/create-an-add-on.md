---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um ein Add-on für eine APP zu erstellen, die bei Ihrem Partner Center-Konto registriert ist.
title: Erstellen eines Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, CREATE Add-on, in-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: a54ad4bcdb0c6fd652d56c767aa8362073b07875
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167724"
---
# <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um ein Add-on (auch als in-App-Produkt oder IAP bezeichnet) für eine APP zu erstellen, die bei Ihrem Partner Center-Konto registriert ist.

> [!NOTE]
> Diese Methode erstellt ein Add-on ohne Übermittlungen. Verwenden Sie zum Erstellen einer Übermittlung für ein Add-On die Methoden unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

|  Parameter  |  Typ  |  BESCHREIBUNG  |  Erforderlich  |
|------|------|------|------|
|  applicationIds  |  array  |  Ein Array, das die Store-ID der App enthält, der dieses Add-On zugeordnet ist. In diesem Array wird nur ein Element unterstützt.   |  Ja  |
|  productId  |  Zeichenfolge  |  Die Produkt-ID des Add-Ons. Dies ist eine ID, die im Code verwendet werden kann, um auf das Add-On zu verweisen. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](../publish/set-your-add-on-product-id.md).  |  Ja  |
|  productType  |  Zeichenfolge  |  Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Durable** und **Consumable**.  |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie ein neues Endverbraucher-Add-On für eine App erstellen.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Add-On-Ressource](manage-add-ons.md#add-on-object).

```json
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
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG                                                                                                                                                                           |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 409  | Das Add-on konnte aufgrund seines aktuellen Status nicht erstellt werden, oder das Add-on verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |   


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Abrufen aller Add-Ons](get-all-add-ons.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Löschen eines Add-Ons](delete-an-add-on.md)