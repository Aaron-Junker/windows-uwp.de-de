---
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um ein Add-on für eine app zu löschen, die mit Ihrem Partner Center-Konto registriert ist.
title: Löschen eines Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Add-on, löschen, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 5540dfbdc185eae405b2cdde7f18faee0efd0cac
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334618"
---
# <a name="delete-an-add-on"></a>Löschen eines Add-Ons

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um ein Add-on (auch bekannt als in-app-Produkt oder IAP) für eine app zu löschen, die mit Ihrem Partner Center-Konto registriert ist.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | String | Erforderlich. Die Store-ID des zu löschenden Add-Ons. Die Store-ID ist im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird das Löschen eines Add-Ons veranschaulicht.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Wenn dies erfolgreich war, gibt die Methode einen leeren Antworttext zurück.

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung                                                                                                                                                                           |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 404  | Das angegebene Add-On konnte nicht gefunden werden.  |
| 409  | Das angegebene Add-on wurde gefunden, aber konnte im aktuellen Zustand nicht gelöscht werden, oder das Add-on verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Alle Add-ons zu erhalten](get-all-add-ons.md)
* [Ein Add-on abzurufen](get-an-add-on.md)
* [Ein Add-on erstellen](create-an-add-on.md)
