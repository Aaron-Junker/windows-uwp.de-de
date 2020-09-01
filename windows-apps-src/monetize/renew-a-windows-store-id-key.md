---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Erfahren Sie, wie Sie einen abgelaufenen Microsoft Store ID-Schlüssel mithilfe der Renew-Methode in den Microsoft Store Collection-und Purchase-APIs erneuern.
title: Verlängern eines Microsoft Store-ID-Schlüssels
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Collection-API, Microsoft Store Purchase-API, Microsoft Store ID-Schlüssel, erneuern
ms.localizationpriority: medium
ms.openlocfilehash: fe19b446f88e16b87ff40288f5d4480f9230469e
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238255"
---
# <a name="renew-a-microsoft-store-id-key"></a>Verlängern eines Microsoft Store-ID-Schlüssels


Verwenden Sie diese Methode, um einen Microsoft Store Schlüssel zu erneuern. Wenn Sie [einen Microsoft Store ID-Schlüssel generieren](view-and-grant-products-from-a-service.md#step-4), ist der Schlüssel 90 Tage gültig. Nach Ablauf des Schlüssels können Sie anhand des abgelaufenen Schlüssels einen neuen Schlüssel aushandeln, indem Sie diese Methode anwenden.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD Zugriffs Token, das den Wert für den Zielgruppen-Uri aufweist `https://onestore.microsoft.com` .
* Ein abgelaufener Microsoft Store ID-Schlüssel, der [aus Client seitigem Code in Ihrer APP generiert](view-and-grant-products-from-a-service.md#step-4)wurde.

Weitere Informationen finden Sie unter [Verwalten von Produkt Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Schlüsseltyp    | Methode | Anforderungs-URI                                              |
|-------------|--------|----------------------------------------------------------|
| Sammlungen | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Purchase    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>Anforderungsheader

| Header         | type   | BESCHREIBUNG                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | Zeichenfolge | Muss auf den Wert **collections.mp.microsoft.com** oder **purchase.mp.microsoft.com** festgelegt werden.           |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | Zeichenfolge | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter     | type   | BESCHREIBUNG                       | Erforderlich |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | Zeichenfolge | Das Azure AD-Zugriffstoken.        | Ja      |
| Schlüssel           | Zeichenfolge | Der abgelaufene Microsoft Store ID-Schlüssel. | Ja       |


### <a name="request-example"></a>Anforderungsbeispiel

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Parameter | type   | BESCHREIBUNG                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| Schlüssel       | Zeichenfolge | Der aktualisierte Microsoft Store Schlüssel, der in zukünftigen Aufrufen der Microsoft Store Collections-API oder der Kauf-API verwendet werden kann. |


### <a name="response-example"></a>Antwortbeispiel

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>Fehlercodes


| Code | Fehler        | Interner Fehlercode           | BESCHREIBUNG   |
|------|--------------|----------------------------|---------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *ClientID-* Anspruch im Microsoft Store ID-Schlüssel und der *AppID* -Anspruch im Azure AD Zugriffs Token stimmen nicht ab.                                                                     |


## <a name="related-topics"></a>Zugehörige Themen


* [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md)
* [Produktabfrage](query-for-products.md)
* [Melden von konsumierbaren Produkten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
