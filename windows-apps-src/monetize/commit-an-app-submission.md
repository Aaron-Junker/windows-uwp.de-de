---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine neue oder aktualisierte APP-Übermittlung an Partner Center zu übermitteln.
title: Ausführen eines Commit für eine App-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Commit für App-Übermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 83d9527b93e60af144b8c38d98a4f2e4af7852d9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171664"
---
# <a name="commit-an-app-submission"></a>Ausführen eines Commit für eine App-Übermittlung

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine neue oder aktualisierte APP-Übermittlung an Partner Center zu übermitteln. Die Commit-Aktion warnt Partner Center, dass die Übermittlungs Daten hochgeladen wurden (einschließlich aller zugehörigen Pakete und Images). Als Antwort führt Partner Center die Änderungen an den Übermittlungs Daten für die Erfassung und Veröffentlichung aus. Nachdem der Commitvorgang erfolgreich ausgeführt wurde, werden die Änderungen an der Übermittlung im Partner Center angezeigt.

Weitere Informationen dazu, wie der Commitvorgang in den Prozess der Übermittlung einer App mithilfe der Microsoft Store Übermittlungs-API passt, finden Sie unter [Verwalten von App](manage-app-submissions.md)-Übermittlungen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* [Erstellen Sie eine App-Übermittlung](create-an-app-submission.md), und [aktualisieren Sie die Übermittlung](update-an-app-submission.md) mit allen erforderlichen Änderungen der Übermittlungsdaten.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, die die Übermittlung enthält, die Sie übernehmen möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| submissionId | Zeichenfolge | Erforderlich. Die ID der Übermittlung, die Sie übernehmen möchten. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |

### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie eine App-Übermittlung übernommen wird.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | Zeichenfolge  | Der Status der Übermittlung. Mögliche Werte: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Veröffentlichung</li><li>Veröffentlicht</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Zertifizierung</li><li>CertificationFailed</li><li>Freigabe</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 409  | Die angegebene Übermittlung wurde gefunden, es konnte jedoch kein Commit in Ihrem aktuellen Zustand ausgeführt werden, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen einer App-Übermittlung](get-an-app-submission.md)
* [Erstellen einer App-Übermittlung](create-an-app-submission.md)
* [Aktualisieren einer App-Übermittlung](update-an-app-submission.md)
* [Löschen einer App-Übermittlung](delete-an-app-submission.md)
* [Abrufen des Status einer App-Übermittlung](get-status-for-an-app-submission.md)