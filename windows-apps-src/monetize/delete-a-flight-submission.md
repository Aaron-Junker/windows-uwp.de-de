---
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: Verwenden Sie diese Methode der Microsoft Store-Übermittlungs-API zum Löschen einer vorhandenen Flight-Paketübermittlung.
title: Löschen einer Flight-Paket-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Flight-Übermittlungen, löschen, Flight-Paket
ms.localizationpriority: medium
ms.openlocfilehash: 32f0aa1af140f17275e9023ac345e05412a56212
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335138"
---
# <a name="delete-a-package-flight-submission"></a>Löschen einer Flight-Paket-Übermittlung

Verwenden Sie diese Methode der Microsoft Store-Übermittlungs-API zum Löschen einer vorhandenen Flight-Paketübermittlung.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | String | Erforderlich. Die Store-ID der App, die die zu löschende Flight-Paketübermittlung enthält. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | String | Erforderlich. Die ID des Flight-Pakets, das die zu löschende Übermittlung enthält. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten. Für einen Flug, der im Partner Center erstellt wurde, ist diese ID auch in der URL für die Seite "aktiv" im Partner Center verfügbar.  |
| submissionId | String | Erforderlich. Die ID der zu löschenden Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md) verfügbar. Für eine Eingabe, die im Partner Center erstellt wurde, ist diese ID auch in die URL für die Seite für die Auftragsübermittlung im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie eine Flight-Paketübermittlung löschen.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Wenn dies erfolgreich war, gibt die Methode einen leeren Antworttext zurück.

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 409  | Die angegebene Übermittlung wurde gefunden, aber konnte im aktuellen Zustand nicht gelöscht werden, oder die app verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Paket-Flight-Übermittlungen](manage-flight-submissions.md)
* [Erhalten Sie eine Paket-Flight-Eingabe](get-a-flight-submission.md)
* [Erstellen Sie eine Paket-Flight-Eingabe](create-a-flight-submission.md)
* [Übernehmen Sie eine Paket-Flight-Eingabe](commit-a-flight-submission.md)
* [Aktualisieren Sie eine Paket-Flight-Eingabe](update-a-flight-submission.md)
* [Abrufen des Status einer Paket-Flight-Übermittlung](get-status-for-a-flight-submission.md)
