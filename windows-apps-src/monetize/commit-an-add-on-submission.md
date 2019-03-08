---
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um eine neue oder aktualisierte-Add-On-Eingabe zum Partner Center zu übernehmen.
title: Ausführen eines Commit für eine Add-On-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Übernehmen einer Add-On-Übermittlung, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: efab4412486566ae817eb66e78f5407533a30d5b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608215"
---
# <a name="commit-an-add-on-submission"></a>Ausführen eines Commit für eine Add-On-Übermittlung

Verwenden Sie diese Methode in der Microsoft Store-Übermittlung API, um eine neue oder aktualisierte-Add-On (auch bekannt als in-app-Produkt oder IAP) Eingabe zum Partner Center zu übernehmen. Das Commit Aktion Warnungen Partner Center, dass die Sendungsdaten (einschließlich alle entsprechenden Symbole) hochgeladen wurde. Im Gegenzug übernimmt Partner Center die Änderungen an den Daten der Übermittlung für die Erfassung und Veröffentlichung an. Nachdem der Commitvorgang erfolgreich ist, werden die Änderungen an der Übermittlung im Partner Center angezeigt.

Weitere Informationen dazu, wie der Übernahmevorgang in den Prozess zur Übermittlung eines Add-Ons mit der Microsoft Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* [Erstellen Sie eine Add-On-Übermittlung](create-an-add-on-submission.md), und [aktualisieren Sie die Übermittlung](update-an-add-on-submission.md) mit allen erforderlichen Änderungen der Übermittlungsdaten.

## <a name="request"></a>Anfordern

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Erforderlich. Die Store-ID des Add-Ons, die die Übermittlung enthält, die Sie übernehmen möchten. Die Store-ID ist im Partner Center zur Verfügung und befindet sich in die Antwortdaten für Anforderungen an [erhalten alle Add-ons](get-all-add-ons.md) und [ein Add-on erstellen](create-an-add-on.md). |
| submissionId | string | Erforderlich. Die ID der Übermittlung, die Sie übernehmen möchten. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md) verfügbar. Für eine Eingabe, die im Partner Center erstellt wurde, ist diese ID auch in die URL für die Seite für die Auftragsübermittlung im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie eine Add-On-Übermittlung übernommen wird.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
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

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Version</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 407  | Die angegebene Übermittlung wurde gefunden, aber konnte nicht im aktuellen Zustand ausgeführt werden, oder das Add-on verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Erhalten Sie ein Add-on-Übermittlung](get-an-add-on-submission.md)
* [Erstellen Sie ein Add-on-Übermittlung](create-an-add-on-submission.md)
* [Aktualisieren einer-Add-On](update-an-add-on-submission.md)
* [Löschen Sie ein Add-on-Übermittlung](delete-an-add-on-submission.md)
* [Abrufen des Status einer Übermittlung-Add-On](get-status-for-an-add-on-submission.md)
