---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um ein Add-on für eine app zu erstellen, die mit Ihrem Konto PartnerCenter registriert ist.
title: Erstellen eines Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Erstellen eines Add-Ons, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 00eb1a865631ce51cfa065d27ed00b44c66a6757
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371267"
---
# <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um ein Add-on (auch bekannt als in-app-Produkt oder IAP) für eine app zu erstellen, die mit Ihrem Partner Center-Konto registriert ist.

> [!NOTE]
> Durch diese Methode wird ein Add-On ohne Übermittlungen erstellt. Verwenden Sie zum Erstellen einer Übermittlung für ein Add-On die Methoden unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

|  Parameter  |  Typ  |  Beschreibung  |  Erforderlich  |
|------|------|------|------|
|  applicationIds  |  array  |  Ein Array, das die Store-ID der App enthält, der dieses Add-On zugeordnet ist. In diesem Array wird nur ein Element unterstützt.   |  Ja  |
|  productId  |  String  |  Die Produkt-ID des Add-Ons. Dies ist eine ID, die im Code verwendet werden kann, um auf das Add-On zu verweisen. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](https://docs.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Ja  |
|  productType  |  String  |  Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Dauerhafte** und **nutzbar**.  |  Ja  |


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

| Fehlercode |  Beschreibung                                                                                                                                                                           |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 409  | Das Add-on konnte aufgrund von seinem aktuellen Zustand nicht erstellt werden, oder das Add-on verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten Sie Add-Ons Übermittlungen](manage-add-on-submissions.md)
* [Alle Add-ons zu erhalten](get-all-add-ons.md)
* [Ein Add-on abzurufen](get-an-add-on.md)
* [Add-on löschen](delete-an-add-on.md)
