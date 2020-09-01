---
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene Package Flight-Übermittlung zu löschen.
title: Löschen einer Flight-Paket-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Übermittlung von Flügen, löschen, paketflug
ms.localizationpriority: medium
ms.openlocfilehash: 9cfac0958e5f4b4175fafe1d22a42b2b6eae08c2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171654"
---
# <a name="delete-a-package-flight-submission"></a>Löschen einer Flight-Paket-Übermittlung

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene Package Flight-Übermittlung zu löschen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| Delete    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, die die zu löschende Flight-Paketübermittlung enthält. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| flightId | Zeichenfolge | Erforderlich. Die ID des Flight-Pakets, das die zu löschende Übermittlung enthält. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines paketflugs](create-a-flight.md) und [zum erhalten von Paket Flügen für eine APP](get-flights-for-an-app.md)verfügbar. Für einen Flug, der in Partner Center erstellt wurde, ist diese ID auch in der URL für die Flight-Seite im Partner Center verfügbar.  |
| submissionId | Zeichenfolge | Erforderlich. Die ID der zu löschenden Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer paketflight-Übermittlung](create-a-flight-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |


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

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 400  | Die Anforderungsparameter sind ungültig. |
| 404  | Die angegebene Übermittlung konnte nicht gefunden werden. |
| 409  | Die angegebene Übermittlung wurde gefunden, konnte aber nicht im aktuellen Zustand gelöscht werden, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paket-Übermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md)
* [Aktualisieren einer Flight-Paket-Übermittlung](update-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)