---
ms.assetid: DAF92881-6AF6-44C7-B466-215F5226AE04
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um Informationen zu einer bestimmten app abzurufen, die bei Ihrem Partner Center-Konto registriert ist.
title: Abrufen einer App
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, App
ms.localizationpriority: medium
ms.openlocfilehash: 29f4b7fdb196e1463f4e738618694363094933f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171474"
---
# <a name="get-an-app"></a>Abrufen einer App

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um Informationen zu einer bestimmten app abzurufen, die bei Ihrem Partner Center-Konto registriert ist.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der abzurufenden App. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Informationen zu einer APP mit dem Store-ID-Wert 9nblggh4r315 abgerufen werden.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Anwendungsressource](get-app-data.md#application_object).

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": false
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 404  | Die angegebene App konnte nicht gefunden werden. |
| 409  | Die APP verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird.  |


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen aller Apps](get-all-apps.md)
* [Flight-Pakete für eine App abrufen](get-flights-for-an-app.md)
* [Add-Ons für eine App abrufen](get-add-ons-for-an-app.md)