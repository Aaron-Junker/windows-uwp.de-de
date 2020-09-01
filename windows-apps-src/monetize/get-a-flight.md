---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um Daten für einen paketflug für eine APP zu erhalten, die bei Ihrem Partner Center-Konto registriert ist.
title: Abrufen eines Flight-Pakets
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Flug, paketflug
ms.localizationpriority: medium
ms.openlocfilehash: 3916328f2c48642596c6a0b11401f583957bc973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172904"
---
# <a name="get-a-package-flight"></a>Abrufen eines Flight-Pakets

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um Daten für einen paketflug für eine APP zu erhalten, die bei Ihrem Partner Center-Konto registriert ist.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, die die Flight-Paketübermittlung enthält, die Sie abrufen möchten. Die Store-ID für die APP ist im Partner Center verfügbar.  |
| flightId | Zeichenfolge | Erforderlich. Die ID des abzurufenden Flight-Pakets. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines paketflugs](create-a-flight.md) und [zum erhalten von Paket Flügen für eine APP](get-flights-for-an-app.md)verfügbar. Für einen Flug, der in Partner Center erstellt wurde, ist diese ID auch in der URL für die Flight-Seite im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Stellen Sie keinen Anforderungstext für diese Methode bereit.

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Informationen über ein Flight-Paket mit der ID 43e448df-97c9-4a43-a0bc-2a445e736bcd für eine App mit dem Store-ID-Wert 9WZDNCRD91MD abgerufen werden.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Antworttext

| Wert      | Typ   | BESCHREIBUNG                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | Zeichenfolge  | Die ID für das Flight-Paket. Dieser Wert wird von Partner Center bereitgestellt.  |
| friendlyName           | Zeichenfolge  | Der Name des Flight-Pakets nach Vorgabe des Entwicklers.   |  
| lastPublishedFlightSubmission       | Objekt (object) | Ein Objekt, das Informationen über die letzte veröffentlichte Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlungsobjekt](#submission_object).  |
| pendingFlightSubmission        | Objekt (object)  |  Ein Objekt, das Informationen über die aktuell ausstehende Übermittlung für das Flight-Paket enthält. Weitere Informationen finden Sie unten im Abschnitt [Übermittlungsobjekt](#submission_object).  |   
| groupIds           | array  | Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).   |
| rankHigherThan           | Zeichenfolge  | Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).  |


<span id="submission_object" />

### <a name="submission-object"></a>Übermittlungsobjekt

Die Werte *LastPublishedFlightSubmission* und *PendingFlightSubmission* im Antworttext enthalten Objekte mit Ressourceninformationen über eine Übermittlung für das Flight-Paket. Diese Objekte enthalten folgende Werte.

| Wert           | Typ    | BESCHREIBUNG                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | Zeichenfolge  | Die ID der Übermittlung.    |
| resourceLocation   | Zeichenfolge  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI `https://manage.devcenter.microsoft.com/v1.0/my/` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.               |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG     |
|--------|---------------------  |
| 400  | Die Anforderung ist ungültig. |
| 404  | Das angegebene Flight-Paket konnte nicht gefunden werden.   |   
| 409  | Die APP verwendet ein Partner Center-Feature, das [derzeit von der Microsoft Store Übermittlungs-API nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |                                                                                                 


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Erstellen eines Flight-Pakets](create-a-flight.md)
* [Löschen eines Flight-Pakets](delete-a-flight.md)