---
ms.assetid: 55315F38-6EC5-4889-A14E-7D8EC282FE98
description: Verwenden Sie diese Methode der Microsoft Store-Übermittlungs-API, um den Status einer Add-On-Übermittlung abzurufen.
title: Abrufen des Status einer Add-On-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Add-On-Übermittlung, Status
ms.localizationpriority: medium
ms.openlocfilehash: a3d5651dfaff451bdb192bd04b090452d527dd92
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334528"
---
# <a name="get-the-status-of-an-add-on-submission"></a>Abrufen des Status einer Add-On-Übermittlung

Verwenden Sie diese Methode der Microsoft Store-Übermittlungs-API zum Abrufen des Status für eine Add-On-Übermittlung (auch als In-App-Produkt oder IAP bezeichnet). Weitere Informationen über den Erstellungsprozess einer Add-On-Übermittlung mithilfe der Microsoft Store-Übermittlungs-API finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie ein Add-on-Übermittlung für eine Ihrer apps. Sie können dies im Partner Center, oder Sie erreichen dies, indem die [erstellen Sie ein Add-on-Übermittlung](create-an-add-on-submission.md) Methode.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | String | Erforderlich. Die Store-ID des Add-Ons mit der Übermittlung, für die der Status abgerufen werden soll. Die Store-ID ist im Partner Center verfügbar.  |
| submissionId | String | Erforderlich. Die ID der Übermittlung, für die der Status abgerufen werden sollen. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) verfügbar. Für eine Eingabe, die im Partner Center erstellt wurde, ist diese ID auch in die URL für die Seite für die Auftragsübermittlung im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie der Status einer Add-On-Übermittlung abgerufen werden kann.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen über die angegebene Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | String  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Version</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Enthält zusätzliche Details über den Status der Übermittlung, einschließlich Informationen zu Fehlern. Weitere Informationen finden Sie unter [Statusdetails-Ressource](manage-add-on-submissions.md#status-details-object). |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 404  | Die Übermittlung konnte nicht gefunden werden. |
| 409  | Das Add-on verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Erhalten Sie ein Add-on-Übermittlung](get-an-add-on-submission.md)
* [Erstellen Sie ein Add-on-Übermittlung](create-an-add-on-submission.md)
* [Übernehmen Sie ein Add-on-Übermittlung](commit-an-add-on-submission.md)
* [Aktualisieren einer-Add-On](update-an-add-on-submission.md)
* [Löschen Sie ein Add-on-Übermittlung](delete-an-add-on-submission.md)
