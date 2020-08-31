---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene paketübertragungs Übermittlung zu aktualisieren.
title: Aktualisieren einer Flight-Paket-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Übermittlung von Flügen, aktualisieren
ms.localizationpriority: medium
ms.openlocfilehash: d2603319e2a1fc242210f79ef38c2ba5362c9c3e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171334"
---
# <a name="update-a-package-flight-submission"></a>Aktualisieren einer Flight-Paket-Übermittlung


Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene paketübertragungs Übermittlung zu aktualisieren. Nachdem Sie mit dieser Methode eine Übermittlung erfolgreich aktualisiert haben, müssen Sie ein [Commit für die Übermittlung](commit-a-flight-submission.md) für Aufnahme und Veröffentlichung durchführen.

Weitere Informationen zur Vorgehensweise bei der Erstellung einer paketflight-Übermittlung mithilfe der Microsoft Store Übermittlungs-API finden Sie unter [Verwalten von paketflight](manage-flight-submissions.md)-Übermittlungen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine paketflight-Übermittlung für eine Ihrer Apps. Dies können Sie in Partner Center tun, oder Sie können dies mithilfe der Methode zum [Erstellen eines Pakets Flight-Übermittlung](create-a-flight-submission.md) tun.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, für die Sie eine Flight Paketübermittlung aktualisieren möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| flightId | Zeichenfolge | Erforderlich. Die ID des Flight-Pakets, für das Sie eine Übermittlung aktualisieren möchten. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines paketflugs](create-a-flight.md) und [zum erhalten von Paket Flügen für eine APP](get-flights-for-an-app.md)verfügbar. Für einen Flug, der in Partner Center erstellt wurde, ist diese ID auch in der URL für die Flight-Seite im Partner Center verfügbar.  |
| submissionId | Zeichenfolge | Erforderlich. Die ID der zu aktualisierenden Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer paketflight-Übermittlung](create-a-flight-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

| Wert      | Typ   | BESCHREIBUNG                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-package-object). Beim Aufruf dieser Methode zur Aktualisierung einer App-Übermittlung sind im Antworttext nur die *fileName*-, *fileStatus*-, *minimumDirectXVersion*- und *minimumSystemRam*-Werte dieser Objekte erforderlich. Die anderen Werte werden von Partner Center aufgefüllt. |
| packageDeliveryOptions    | Objekt (object)  | Enthält den schrittweisen Paketrollout sowie erforderliche Einstellungen für die Übermittlung. Weitere Informationen finden Sie unter [options-Objekt für die Paketübermittlung](manage-flight-submissions.md#package-delivery-options-object).  |
| targetPublishMode           | Zeichenfolge  | Der Veröffentlichungsmodus für die Übermittlung. Mögliche Werte: <ul><li>Unmittelbar</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | Zeichenfolge  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| notesForCertification           | Zeichenfolge  |  Enthält zusätzliche Informationen für Zertifizierungstester wie Anmeldeinformationen für Testkonten und Schritte zum Zugriff auf und zur Überprüfung von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](../publish/notes-for-certification.md). |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird die Aktualisierung einer Flight-Paketübermittlung für eine App veranschaulicht.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur aktualisierten Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 400  | Die Flight-Paketübermittlung konnte nicht aktualisiert werden, da die Anforderung ungültig ist. |
| 409  | Die paketflight-Übermittlung konnte aufgrund des aktuellen Status der APP nicht aktualisiert werden, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |   


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md)
* [Abrufen einer Flight-Paket-Übermittlung](get-a-flight-submission.md)
* [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md)
* [Ausführen eines Commit für eine Flight-Paketübermittlung](commit-a-flight-submission.md)
* [Löschen einer Flight-Paket-Übermittlung](delete-a-flight-submission.md)
* [Abrufen des Status einer Flight-Paketübermittlung](get-status-for-a-flight-submission.md)