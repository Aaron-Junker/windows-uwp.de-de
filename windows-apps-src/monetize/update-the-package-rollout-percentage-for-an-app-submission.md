---
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um den Paket Rollout Prozentsatz für eine APP-Übermittlung zu aktualisieren.
title: Aktualisieren des Prozentsatzes eines Rollouts einer App-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Paket Rollout, App-Übermittlung, Update, Prozentsatz
ms.assetid: 4c82d837-7a25-4f3a-997e-b7be33b521cc
ms.localizationpriority: medium
ms.openlocfilehash: 20bfbec102f60a9f6335a819a01772ea831f4c44
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158294"
---
# <a name="update-the-rollout-percentage-for-an-app-submission"></a>Aktualisieren des Prozentsatzes eines Rollouts einer App-Übermittlung


Verwenden Sie diese Methode im Microsoft Store Übermittlungs-API, um [den Rollout Prozentsatz](../publish/gradual-package-rollout.md#setting-the-rollout-percentage) für eine APP-Übermittlung zu aktualisieren. Weitere Informationen zum Prozess der Erstellung einer APP-Übermittlung mithilfe der Microsoft Store Übermittlungs-API finden Sie unter [Verwalten von App](manage-app-submissions.md)-Übermittlungen.


## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Übermittlung für eine Ihrer Apps. Dies können Sie in Partner Center tun, oder Sie können dies mithilfe der Methode zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md) tun.
* Ermöglichen Sie einen schrittweisen Paketrollout für die Übermittlung. Sie können dies in [Partner Center](../publish/gradual-package-rollout.md)tun, oder Sie können dies [mithilfe der Microsoft Store](manage-app-submissions.md#manage-gradual-package-rollout)Übermittlungs-API tun.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Headers und der Anforderungsparameter.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App mit der Übermittlung, deren Paketrollout-Prozentsatz aktualisiert werden soll. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| submissionId | Zeichenfolge | Erforderlich. Die ID der Übermittlung, deren Paketrollout-Prozentsatz aktualisiert werden soll. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.   |
| Prozentwert  |  float  |  Erforderlich. Der Prozentsatz der Benutzer, die das Paket für den schrittweisen Rollout erhalten.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird das Aktualisieren des Paketrollout-Prozentsatzes einer App-Übermittlung veranschaulicht.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie unter der [Ressource zum Paketrollout](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 404  | Die Übermittlung konnte nicht gefunden werden. |
| 409  | Dieser Code weist auf einen der folgenden Fehler hin:<br/><br/><ul><li>Die Übermittlung befindet sich nicht in einem gültigen Status für den schrittweisen Rollout (vor dem Aufrufen dieser Methode muss die Übermittlung veröffentlicht und der Wert für [PackageRolloutStatus](manage-app-submissions.md#package-rollout-object) auf **PackageRolloutInProgress** festgelegt werden).</li><li>Die Übermittlung gehört nicht zur angegebenen App.</li><li>Die APP verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird.</li></ul> |   


## <a name="related-topics"></a>Zugehörige Themen

* [Schrittweiser Paketrollout](../publish/gradual-package-rollout.md)
* [Verwalten von Microsoft Store App](manage-app-submissions.md)
* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)