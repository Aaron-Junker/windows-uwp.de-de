---
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Verwenden Sie diese Methode der Microsoft Store-API für Rezensionen, um zu bestimmen, ob Sie auf eine bestimmte Rezension für eine bestimmte App oder auf alle Rezensionen für diese App antworten können.
title: Abrufen von Antwortinformationen für Rezensionen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-API für Rezensionen, Antwortinformationen
ms.localizationpriority: medium
ms.openlocfilehash: 0497b5eec67f9204139cd10d4523b534d6c8779f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595555"
---
# <a name="get-response-info-for-reviews"></a>Abrufen von Antwortinformationen für Rezensionen

Wenn Sie programmgesteuert auf eine Kundenrezension zu Ihrer App antworten möchten, verwenden Sie diese Methode der Microsoft Store-API für Rezensionen, um zunächst zu ermitteln, ob Sie berechtigt sind, auf die Rezension zu antworten. Sie können nicht auf Rezensionen von Kunden antworten, die keine Antwort auf ihre Rezension erhalten möchten. Nachdem sichergestellt ist, dass Sie auf eine Rezension antworten können, verwenden Sie die Methode [Antworten auf App-Rezensionen senden](submit-responses-to-app-reviews.md), um programmgesteuert zu antworten.


## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](respond-to-reviews-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der Rezension ab, für die Sie überprüfen möchten, ob Sie darauf antworten können. Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   | Beschreibung                                     |  Erforderlich  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | string | Die Store-ID der App, welche die Rezension enthält, für die Sie bestimmen möchten, ob Sie antworten können. Die Store-ID ist verfügbar, auf die [Seite "App-Identität"](../publish/view-app-identity-details.md) im Partner Center. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| reviewId | string | Die ID der Rezension, auf die Sie antworten möchten (dies ist eine GUID). Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md). <br/>Wenn Sie diesen Parameter nicht angeben, wird im Antworttext für diese Methode stehen, ob Sie über Berechtigungen zum Beantworten von Rezensionen für die angegebene App verfügen. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt, wie Sie diese Methode verwenden, um festzustellen, ob auf eine bestimmte Rezension antworten können.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung    |  
|------------|--------|-----------------------|
| CanRespond      | Boolesch  | Der Wert **true** gibt an, dass Sie auf die angegebene Rezension antworten können oder dass Sie berechtigt sind, auf eine beliebige Rezensionen für die angegebene App zu antworten. Andernfalls ist dieser Wert **false**.       |
| DefaultSupportEmail  | string |  Die von Ihrer App [unterstütze E-Mail-Adresse](../publish/enter-app-properties.md#support-contact-info) finden Sie im Store-Eintrag Ihrer App. Wenn Sie keine unterstützte E-Mail-Adresse angegeben haben, ist dieses Feld leer.    |

 
### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Senden von Antworten auf Berichte, die mit dem Microsoft Store-Textanalyse-API](submit-responses-to-app-reviews.md)
* [Reagieren Sie auf Überprüfungen durch den Kunden über Partner Center](../publish/respond-to-customer-reviews.md)
* [Reagieren Sie auf Überprüfungen mithilfe von Microsoft Store services](respond-to-reviews-using-windows-store-services.md)
* [Abrufen von app-Bewertungen](get-app-reviews.md)
