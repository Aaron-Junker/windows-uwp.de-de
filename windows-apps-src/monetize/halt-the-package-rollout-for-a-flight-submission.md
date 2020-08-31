---
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um einen Paket Rollout für einen paketflug anzuhalten.
title: Anhalten des Rollouts für einen Flug
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Paket Rollout, Flug Übermittlung, anhalten
ms.assetid: f8ee0687-a421-48e7-a6eb-3fd5633c352b
ms.localizationpriority: medium
ms.openlocfilehash: b4013042d9d0322b73f7386d0d9509774f769ab6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158514"
---
# <a name="halt-the-rollout-for-a-flight"></a>Anhalten des Rollouts für einen Flug

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um [das Rollout](../publish/gradual-package-rollout.md#completing-the-rollout) für eine paketflight-Übermittlung anzuhalten. Weitere Informationen zum Prozess der Erstellung einer paketflight-Übermittlung mithilfe der Microsoft Store Übermittlungs-API finden Sie unter [Verwalten von paketübertragungs](manage-flight-submissions.md)-Übermittlungen.

> [!NOTE]
> Wenn Sie das Rollout für eine Paket-Übertragung anhalten und dann [eine neue paketflight-Übermittlung erstellen](create-a-flight-submission.md), ist die neue Übermittlung ein Klon der angehaltenen Übermittlung.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Übermittlung für eine Ihrer Apps. Dies können Sie in Partner Center tun, oder Sie können dies mithilfe der Methode zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md) tun.
* Ermöglichen Sie einen schrittweisen Paketrollout für die Übermittlung. Dies können Sie [in Partner Center](../publish/gradual-package-rollout.md)tun, oder Sie können dies [mithilfe der Microsoft Store](manage-flight-submissions.md#manage-gradual-package-rollout)Übermittlungs-API tun.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Headers und der Anforderungsparameter.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App mit der Flight-Paket-Übermittlung, deren Paketrollout angehalten werden soll. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| flightId | Zeichenfolge | Erforderlich. Die Store-ID des Flight-Pakets mit der Übermittlung, deren Paketrollout angehalten werden soll. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines paketflugs](create-a-flight.md) und [zum erhalten von Paket Flügen für eine APP](get-flights-for-an-app.md)verfügbar. Für einen Flug, der in Partner Center erstellt wurde, ist diese ID auch in der URL für die Flight-Seite im Partner Center verfügbar.   |
| submissionId | Zeichenfolge | Erforderlich. Die ID der Übermittlung mit dem Paketrollout, das angehalten werden soll. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer paketflight-Übermittlung](create-a-flight-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird das Anhalten des Paketrollouts einer Flight-Paket-Übermittlung veranschaulicht.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie unter der [Ressource zum Paketrollout](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 404  | Die angegebene Flight-Paket-Übermittlung konnte nicht gefunden werden. |
| 409  | Dieser Code weist auf einen der folgenden Fehler hin:<br/><br/><ul><li>Die Übermittlung befindet sich nicht in einem gültigen Status für den schrittweisen Rollout (vor dem Aufrufen dieser Methode muss die Übermittlung veröffentlicht und der Wert für [PackageRolloutStatus](manage-flight-submissions.md#package-rollout-object) auf **PackageRolloutInProgress** festgelegt werden).</li><li>Die Übermittlung gehört nicht zur angegebenen App.</li><li>Die APP verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird.</li></ul> |   


## <a name="related-topics"></a>Zugehörige Themen

* [Schrittweiser Paketrollout](../publish/gradual-package-rollout.md)
* [Verwalten von Übermittlungen von Paketen mithilfe der Microsoft Store Übermittlungs-API](manage-flight-submissions.md)
* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)