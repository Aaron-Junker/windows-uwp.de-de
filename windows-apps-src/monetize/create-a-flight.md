---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um einen paketflug für eine APP zu erstellen, die bei Ihrem Partner Center-Konto registriert ist.
title: Erstellen eines Flight-Pakets
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Erstellen eines Flugs
ms.localizationpriority: medium
ms.openlocfilehash: ec087d590c0286eb99c3ad2524036d9fd4230508
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172944"
---
# <a name="create-a-package-flight"></a>Erstellen eines Flight-Pakets

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um einen paketflug für eine APP zu erstellen, die bei Ihrem Partner Center-Konto registriert ist.

> [!NOTE]
> Diese Methode erstellt einen paketflug ohne Übermittlungen. Verwenden Sie zum Erstellen einer Übermittlung für ein Flight-Paket die Methoden unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, für die Sie ein Flight-Paket erstellen möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

|  Parameter  |  Typ  |  BESCHREIBUNG  |  Erforderlich  |
|------|------|------|------|
|  friendlyName  |  Zeichenfolge  |  Der Name des Flight-Pakets nach Vorgabe des Entwicklers.  |  Nein  |
|  groupIds  |  array  |  Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).  |  Nein  |
|  rankHigherThan  |  Zeichenfolge  |  Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält. Wenn Sie diesen Parameter nicht festlegen, erhält das neue Flight-Paket den höchsten Rang aller Flight-Pakete. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).    |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie ein neues Flight-Paket für eine App mit der Store-ID 9WZDNCRD911W erstellen.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Weitere Informationen zu den Werten im Antworttext finden Sie in den folgenden Abschnitten.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
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
| friendlyName           | Zeichenfolge  | Der Name des Flight-Pakets gemäß der Angabe in der Anforderung.   |  
| groupIds           | array  | Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind, gemäß der Angabe in der Anforderung. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).   |
| rankHigherThan           | Zeichenfolge  | Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält, gemäß der Angabe in der Anforderung. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](../publish/package-flights.md).  |


## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 400  | Die Anforderung ist ungültig. |
| 409  | Der paketflug konnte aufgrund seines aktuellen Status nicht erstellt werden, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |   


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen eines Flight-Pakets](get-a-flight.md)
* [Löschen eines Flight-Pakets](delete-a-flight.md)