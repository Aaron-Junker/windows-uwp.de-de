---
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um paketrollout-Informationen für eine Paket-Übertragung zu erhalten.
title: Abrufen von Rolloutinformationen für eine Flight-Paketübermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Paket Rollout, Übermittlung von Flügen
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
ms.localizationpriority: medium
ms.openlocfilehash: 5f8e39650c5b0d9e9124ed671613e7a2f8e77ed2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171414"
---
# <a name="get-rollout-info-for-a-flight-submission"></a>Abrufen von Rolloutinformationen für eine Flight-Paketübermittlung


Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um [paketrollout](../publish/gradual-package-rollout.md) -Informationen für eine Paket-Übertragung zu erhalten. Weitere Informationen zum Prozess der Erstellung einer paketflight-Übermittlung mithilfe der Microsoft Store Übermittlungs-API finden Sie unter [Verwalten von paketübertragungs](manage-flight-submissions.md)-Übermittlungen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine paketflight-Übermittlung für eine Ihrer Apps. Dies können Sie in Partner Center tun, oder Sie können dies mithilfe der Methode zum [Erstellen eines Pakets Flight-Übermittlung](create-a-flight-submission.md) tun.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Headers und der Anforderungsparameter.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App mit der Flight-Paket-Übermittlung, deren Paketrollout-Informationen abgerufen werden sollen. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| flightId | Zeichenfolge | Erforderlich. Die Store-ID des Flight-Pakets mit der Übermittlung, deren Paketrollout-Informationen abgerufen werden sollen. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines paketflugs](create-a-flight.md) und [zum erhalten von Paket Flügen für eine APP](get-flights-for-an-app.md)verfügbar. Für einen Flug, der in Partner Center erstellt wurde, ist diese ID auch in der URL für die Flight-Seite im Partner Center verfügbar.    |
| submissionId | Zeichenfolge | Erforderlich. Die ID der Übermittlung mit den abzurufenden Paketrollout-Informationen. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer paketflight-Übermittlung](create-a-flight-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.   |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird das Abrufen der Paketrollout-Informationen einer Flight-Paket-Übermittlung veranschaulicht.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode für eine Flight-Paket-Übermittlung mit aktiviertem schrittweisem Paketrollout. Weitere Informationen zu den Werten im Antworttext finden Sie unter der [Ressource zum Paketrollout](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Ist für die Flight-Paket-Übermittlung schrittweises Paketrollout nicht aktiviert, wird folgender Antworttext zurückgegeben.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 404  | Die angegebene Flight-Paket-Übermittlung konnte nicht gefunden werden. |
| 409  | Die paketflight-Übermittlung gehört nicht zum angegebenen paketflug, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |   


## <a name="related-topics"></a>Zugehörige Themen

* [Schrittweiser Paketrollout](../publish/gradual-package-rollout.md)
* [Verwalten von Übermittlungen von Paketen mithilfe der Microsoft Store Übermittlungs-API](manage-flight-submissions.md)
* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)