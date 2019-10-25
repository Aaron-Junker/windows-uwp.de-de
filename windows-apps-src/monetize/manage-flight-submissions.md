---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API, um die Übertragung von Paketen für apps zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Übermittlungen von Paketen
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Flug Übermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690376"
---
# <a name="manage-package-flight-submissions"></a>Verwalten von Übermittlungen von Paketen

Die Microsoft Store Übermittlungs-API stellt Methoden bereit, die Sie zum Verwalten von paketflügen für Ihre apps verwenden können, einschließlich der schrittweisen Paket Rollouts. Eine Einführung in die Microsoft Store Übermittlungs-API, einschließlich der Voraussetzungen für die Verwendung der API, finden [Sie unter Erstellen und Verwalten von Übermittlungen mithilfe Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Wenn Sie die Microsoft Store Übermittlungs-API verwenden, um eine Übermittlung für einen paketflight zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur über die API anstelle von Partner Center vornehmen. Wenn Sie das Dashboard verwenden, um eine Übermittlung zu ändern, die Sie ursprünglich mit der API erstellt haben, können Sie diese Übermittlung nicht mehr mithilfe der API ändern oder übertragen. In einigen Fällen kann die Übermittlung in einem Fehlerzustand belassen werden, in dem Sie im Übermittlungs Prozess nicht fortgesetzt werden kann. Wenn dies auftritt, müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Methoden zum Verwalten von Übermittlungen von Paketen

Verwenden Sie die folgenden Methoden, um die Übermittlung von Paketen zu übermitteln, zu erstellen, zu aktualisieren, zu übernehmen oder zu löschen. Bevor Sie diese Methoden verwenden können, muss der paketflug bereits im Partner Center vorhanden sein. Sie können einen paketflug [in Partner Center](https://docs.microsoft.com/windows/uwp/publish/package-flights) oder mithilfe der Methoden zur Microsoft Store Übermittlung von APIs in erstellen, die unter [Verwalten von Paket Flügen](manage-flights.md)beschrieben werden.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Methode</th>
<th align="left">URI</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">Eine vorhandene paketübertragungs Übermittlung erhalten</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">Gibt den Status einer vorhandenen paketübermittlungs Übermittlung an.</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">Erstellen einer neuen paketflight-Übermittlung</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">Aktualisieren einer vorhandenen Paket-Flight-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">Commit für eine neue oder aktualisierte paketflight-Übermittlung</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">Löschen einer paketflight-Übermittlung</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>Erstellen einer paketflight-Übermittlung

Gehen Sie folgendermaßen vor, um eine Übermittlung für einen paketflug zu erstellen.

1. Wenn Sie dies noch nicht getan haben, müssen Sie die unter [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Diensten](create-and-manage-submissions-using-windows-store-services.md)beschriebenen Voraussetzungen erfüllen. dazu gehören das Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto und das Abrufen der Client-ID und des Schlüssels. Dies ist nur einmal erforderlich. Nachdem Sie die Client-ID und den Schlüssel erstellt haben, können Sie Sie jederzeit wieder verwenden, wenn Sie ein neues Azure AD Zugriffs Token erstellen müssen.  

2. Rufen Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)ab. Sie müssen dieses Zugriffs Token an die Methoden in der Microsoft Store Übermittlungs-API übergeben. Nachdem Sie ein Zugriffs Token erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie ein neues abrufen.

3. [Erstellen Sie eine Package Flight-Übermittlung](create-a-flight-submission.md) , indem Sie die folgende Methode in der Microsoft Store Übermittlungs-API ausführen. Diese Methode erstellt eine neue, in Bearbeitung befindliche Übermittlung, bei der es sich um eine Kopie der letzten veröffentlichten Übermittlung handelt.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    Der Antworttext enthält eine Ressource für die [Flug Übermittlung](#flight-submission-object) , die die ID der neuen Übermittlung, den SAS-URI (Shared Access Signature) zum Hochladen von Paketen für die Übermittlung in Azure BLOB Storage und die Daten für die neue Übermittlung (einschließlich all) enthält. die Listen und Preisinformationen).

    > [!NOTE]
    > Ein SAS-URI ermöglicht den Zugriff auf eine sichere Ressource in Azure Storage, ohne dass Kontoschlüssel erforderlich sind. Hintergrundinformationen zu SAS-URIs und deren Verwendung mit Azure BLOB Storage finden Sie unter [Shared Access Signature, Part 1: Grundlegendes zum SAS-Modell](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) und [Shared Access Signature, Teil 2: Erstellen und Verwenden einer SAS mit BLOB Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, [bereiten Sie die Pakete](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) vor, und fügen Sie Sie einem ZIP-Archiv hinzu.

5. Überarbeiten Sie die [Flug](#flight-submission-object) Übermittlungs Daten mit den erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um [die paketflight-Übermittlung zu aktualisieren](update-a-flight-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Wenn Sie neue Pakete für die Übermittlung hinzufügen, stellen Sie sicher, dass Sie die Übermittlungs Daten aktualisieren, um auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv zu verweisen.

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, laden Sie das ZIP-Archiv mit dem SAS-URI in [Azure BLOB Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) hoch, der im Antworttext der Post-Methode bereitgestellt wurde, die Sie zuvor aufgerufen haben. Es gibt verschiedene Azure-Bibliotheken, die Sie für eine Vielzahl von Plattformen verwenden können, darunter:

    * [Azure Storage-Clientbibliothek für .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage-SDK für Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK für Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    Im folgenden C# Codebeispiel wird veranschaulicht, wie Sie ein ZIP-Archiv mithilfe der [cloudblockblob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) -Klasse in der Azure Storage-Client Bibliothek für .net in Azure BLOB Storage hochladen. In diesem Beispiel wird davon ausgegangen, dass das ZIP-Archiv bereits in ein Datenstrom Objekt geschrieben wurde.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Führen Sie die folgende Methode aus, um [die paketübertragungs Übermittlung](commit-a-flight-submission.md) auszuführen. Dadurch wird Partner Center benachrichtigt, dass Sie Ihre Übermittlung abgeschlossen haben und dass Ihre Updates jetzt auf Ihr Konto angewendet werden sollen.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Überprüfen Sie den Commit-Status, indem Sie die folgende Methode ausführen, um [den Status der paketflight-Übermittlung zu erhalten](get-status-for-a-flight-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Überprüfen Sie den *Status* Wert im Antworttext, um den Übermittlungs Status zu bestätigen. Dieser Wert sollte von **commitstarted** entweder in **Vorverarbeitung** geändert werden, wenn die Anforderung erfolgreich ist, oder **commitfailed** , wenn die Anforderung Fehler enthält. Wenn Fehler auftreten, enthält das Feld " *Status Details* " Weitere Details zu diesem Fehler.

7. Nachdem der Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Erfassung an den Speicher gesendet. Sie können den Übermittlungs Fortschritt weiterhin mithilfe der vorherigen Methode oder durch Besuch von Partner Center überwachen.

<span/>

## <a name="code-examples"></a>Codebeispiele

In den folgenden Artikeln finden Sie ausführliche Codebeispiele, die veranschaulichen, wie Sie eine paketflight-Übermittlung in verschiedenen Programmiersprachen erstellen:

* [C#Codebeispiele](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java-Codebeispiele](java-code-examples-for-the-windows-store-submission-api.md)
* [Python-Codebeispiele](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Storebroker-PowerShell-Modul

Als Alternative zum direkten Aufrufen der Microsoft Store Übermittlungs-API stellen wir auch ein Open-Source-PowerShell-Modul bereit, das eine Befehlszeilenschnittstelle zusätzlich zur API implementiert. Dieses Modul heißt [storebroker](https://aka.ms/storebroker). Sie können dieses Modul verwenden, um Ihre APP-, Flug-und Add-on-Übermittlungen von der Befehlszeile aus zu verwalten, anstatt die Microsoft Store Übermittlungs-API direkt aufzurufen, oder Sie können einfach die Quelle durchsuchen, um weitere Beispiele zum Aufrufen dieser API anzuzeigen. Das storebroker-Modul wird in Microsoft aktiv als die primäre Art und Weise verwendet, dass viele Anwendungen von erst Anbietern an den Store übermittelt werden.

Weitere Informationen finden Sie [auf der Seite storebroker auf GitHub](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Verwalten einer schrittweisen Paket Bereitstellung für eine Package Flight-Übermittlung

Sie können die aktualisierten Pakete in einem paketübertragungs-Übertragung in einen Prozentsatz Ihrer APP-Kunden unter Windows 10 schrittweise einführen. Auf diese Weise können Sie Feedback-und Analysedaten für bestimmte Pakete überwachen, um sicherzustellen, dass Sie sich vor dem Rollout der Aktualisierung für das Update sicher sind. Sie können den Rollout Prozentsatz (oder das Update anhalten) für eine veröffentlichte Übermittlung ändern, ohne eine neue Übermittlung erstellen zu müssen. Weitere Informationen, einschließlich Anleitungen zum Aktivieren und Verwalten einer schrittweisen Paket Bereitstellung in Partner Center, finden Sie in [diesem Artikel](../publish/gradual-package-rollout.md).

Wenn Sie Programm gesteuert eine schrittweise Paket Bereitstellung für eine Package Flight-Übermittlung aktivieren möchten, befolgen Sie diesen Prozess mithilfe der Methoden in der Microsoft Store Übermittlungs-API:

  1. [Erstellen Sie eine paketübertragungs Übermittlung](create-a-flight-submission.md) , oder senden [Sie eine paketübertragungs Übermittlung](get-a-flight-submission.md).
  2. Suchen Sie in den Antwortdaten die Ressource " [packagerollout](#package-rollout-object) ", legen Sie das Feld " *ispackagerollout* " auf "true" fest, und legen Sie das Feld " *packagerolloutprozent* " auf den Prozentsatz der Kunden der APP fest, der die aktualisierten Pakete erhalten soll.
  3. Übergeben Sie die aktualisierten paketflight-Übermittlungs Daten an die Methode zum Aktualisieren der Methode zum [Aktualisieren eines Pakets](update-a-flight-submission.md) .

Nachdem ein schrittweises Paket Rollout für eine paketübertragungs Übermittlung aktiviert wurde, können Sie die folgenden Methoden verwenden, um das schrittweise Rollout Programm gesteuert zu erhalten, zu aktualisieren, anzuhalten oder zu finalisieren.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Methode</th>
<th align="left">URI</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">Informationen zum schrittweisen Rollout für eine Package Flight-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">Aktualisieren des prozentualen Rollout Prozentsatzes für die Übermittlung eines paketflugs</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">Schrittweises Rollout für die Übermittlung eines paketflugs anhalten</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">Abschließen des schrittweisen Rollouts für eine Paket Flug Übermittlung</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>Datenressourcen

Die Methoden zur Übermittlung von Microsoft Store Übermittlungs-API für die Verwaltung von Paket Flügen verwenden die folgenden JSON-Datenressourcen.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Flight-Übermittlungs Ressource

Diese Ressource beschreibt die Übermittlung eines paketflugs.

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

Diese Ressource verfügt über die folgenden Werte:

| Wert      | Typ   | Beschreibung              |
|------------|--------|------------------------------|
| id            | string  | Die ID für die Übermittlung.  |
| flightid           | string  |  Die ID des paketflugs, dem die Übermittlung zugeordnet ist.  |  
| status           | string  | Der Status der Übermittlung. Mögliche Werte: <ul><li>Keine</li><li>Canceled</li><li>Commit-Commit</li><li>Commitstarted</li><li>Commitfailed</li><li>""</li><li>Veröffentlichung</li><li>Veröffentlicht</li><li>Veröffentlichter Fehler</li><li>Vorverarbeitung</li><li>Preprocessingfailed</li><li>Zertifizierung</li><li>Certificationfailed</li><li>Release</li><li>Releasefehler</li></ul>   |
| aktivierten           | objekt  |  Eine [Ressource mit Status Details](#status-details-object) , die zusätzliche Details zum Status der Übermittlung enthält, einschließlich Informationen zu Fehlern.  |
| flightpackages           | array  | Enthält [Flight Package-Ressourcen](#flight-package-object) , die Details zu den einzelnen Paketen in der Übermittlung bereitstellen.   |
| packagedeliveryoptions    | objekt  | Eine Ressource für die Paket Übermittlungs Optionen, die eine schrittweise Paket [Bereitstellung](#package-delivery-options-object) und verbindliche Update Einstellungen für die Übermittlung enthält.   |
| fileuploadurl           | string  | Der SAS-URI (Shared Access Signature) zum Hochladen von Paketen für die Übermittlung. Wenn Sie neue Pakete für die Übermittlung hinzufügen, laden Sie das ZIP-Archiv, das die Pakete enthält, in diesen URI hoch. Weitere Informationen finden Sie unter [Erstellen einer paketflight-Übermittlung](#create-a-package-flight-submission).  |
| targetpublishmode           | string  | Der Veröffentlichungs Modus für die Übermittlung. Mögliche Werte: <ul><li>Unmittelbar</li><li>Manuell</li><li>Spezificdate</li></ul> |
| targetpublishdate           | string  | Das Veröffentlichungsdatum für die Übermittlung im ISO 8601-Format, wenn " *targetpublishmode* " auf "specificdate" festgelegt ist.  |
| notesforcertification           | string  |  Bietet zusätzliche Informationen für die Zertifizierungs Tester, wie z. b. Test Konto-Anmelde Informationen und Schritte zum Zugreifen auf und Überprüfen von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Status Details-Ressource

Diese Ressource enthält weitere Details zum Status einer Übermittlung. Diese Ressource verfügt über die folgenden Werte:

| Wert           | Typ    | Beschreibung                   |
|-----------------|---------|------|
|  errors               |    objekt     |   Ein Array von [Status Detail Ressourcen](#status-detail-object) , die Fehlerdetails für die Übermittlung enthalten.   |     
|  Hinweise               |   objekt      | Ein Array von [Status Detail Ressourcen](#status-detail-object) , die Warnungs Details für die Übermittlung enthalten.     |
|  certificationreports               |     objekt    |   Ein Array von [Zertifizierungs Berichts Ressourcen](#certification-report-object) , die den Zugriff auf die Zertifizierungs Berichtsdaten für die Übermittlung ermöglichen. Wenn die Zertifizierung fehlschlägt, können Sie diese Berichte auf Weitere Informationen überprüfen.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Status Detail Ressource

Diese Ressource enthält zusätzliche Informationen zu verwandten Fehlern oder Warnungen für eine Übermittlung. Diese Ressource verfügt über die folgenden Werte:

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|------|
|  Code               |    string     |   Ein Übermittlungs [Statuscode](#submission-status-code) , der den Typ des Fehlers oder der Warnung beschreibt. |  
|  Details               |     string    |  Eine Meldung mit weiteren Details zum Problem.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Zertifikat Bericht Ressource

Diese Ressource ermöglicht den Zugriff auf die Zertifizierungs Berichtsdaten für eine Übermittlung. Diese Ressource verfügt über die folgenden Werte:

| Wert           | Typ    | Beschreibung         |
|-----------------|---------|------|
|     date            |    string     |  Das Datum und die Uhrzeit, zu der der Bericht generiert wurde, im ISO 8601-Format.    |
|     reporturl            |    string     |  Die URL, auf der Sie auf den Bericht zugreifen können.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Flight Package-Ressource

Diese Ressource stellt Details zu einem Paket in einer Übermittlung bereit.

```json
{
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
}
```

Diese Ressource verfügt über die folgenden Werte:

> [!NOTE]
> Beim Aufrufen der Methode zum [Aktualisieren einer paketübertragungs](update-a-flight-submission.md) -Methode ist nur der *Dateiname*, der *fileStatus*-Wert, der *minimumdirectxversion*-Wert und der *minimumsystemram* -Wert dieses Objekts im Anforderungs Text erforderlich. Die anderen Werte werden von Partner Center aufgefüllt.

| Wert           | Typ    | Beschreibung              |
|-----------------|---------|------|
| fileName   |   string      |  Der Name des Pakets.    |  
| Dateistatus    | string    |  Der Status des Pakets. Mögliche Werte: <ul><li>Keine</li><li>"Pdingupload"</li><li>Hochgeladen</li><li>"PendingDelete"</li></ul>    |  
| id    |  string   |  Eine ID, die das Paket eindeutig identifiziert. Dieser Wert wird von Partner Center verwendet.   |     
| version    |  string   |  Die Version des App-Pakets. Weitere Informationen finden Sie unter [Paket Versionsnummerierung](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| Architektur    |  string   |  Die Architektur des App-Pakets (z. b. Arm).   |     
| languages    | array    |  Ein Array von Sprachcodes für die Sprachen, die von der App unterstützt werden. Weitere Informationen finden Sie [unter Unterstützte Sprachen](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Ein Array von Funktionen, die für das Paket erforderlich sind. Weitere Informationen zu Funktionen finden Sie unter [Deklarationen von App](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)-Funktionen.   |     
| minimumdirectxversion    |  string   |  Die minimale DirectX-Version, die vom APP-Paket unterstützt wird. Dies kann nur für apps festgelegt werden, die auf Windows 8. x abzielen. Sie wird für apps ignoriert, die auf andere Versionen abzielen. Mögliche Werte: <ul><li>Keine</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumsystemram    | string    |  Der minimale Arbeitsspeicher, der für das App-Paket erforderlich ist. Dies kann nur für apps festgelegt werden, die auf Windows 8. x abzielen. Sie wird für apps ignoriert, die auf andere Versionen abzielen. Mögliche Werte: <ul><li>Keine</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressourcen für Paket Übermittlungs Optionen

Diese Ressource enthält ein schrittweises Paket Rollout und verbindliche Update Einstellungen für die Übermittlung.

```json
{
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
}
```

Diese Ressource verfügt über die folgenden Werte:

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| packagerollout   |   objekt      |   Eine [Package Rollout-Ressource](#package-rollout-object) , die schrittweise paketrollout-Einstellungen für die Übermittlung enthält.    |  
| ismandatoryupdate    | Boolescher Wert    |  Gibt an, ob Sie die Pakete in dieser Übermittlung als obligatorisch für die Selbstinstallation von APP-Updates behandeln möchten. Weitere Informationen zu obligatorischen Paketen für die Selbstinstallation von APP-Updates finden [Sie unter herunterladen und Installieren von Paket Updates für Ihre APP](../packaging/self-install-package-updates.md).    |  
| mandatoryupdateeffectivedate    |  date   |  Das Datum und die Uhrzeit, zu der die Pakete in dieser Übermittlung obligatorisch werden, im ISO 8601-Format und in der UTC-Zeitzone.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Paket Rollout Ressource

Diese Ressource enthält schrittweise [paketrollout-Einstellungen](#manage-gradual-package-rollout) für die Übermittlung. Diese Ressource verfügt über die folgenden Werte:

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| ispackagerollout   |   Boolescher Wert      |  Gibt an, ob das schrittweise Rollout des Pakets für die Übermittlung aktiviert ist.    |  
| packagerolloutprozentsatz    | float    |  Der Prozentsatz der Benutzer, die die Pakete beim schrittweisen Rollout erhalten.    |  
| packagerolloutstatus    |  string   |  Eine der folgenden Zeichen folgen, die den Status des schrittweisen Paket Rollouts angibt: <ul><li>Packagerolloutnotstarted</li><li>Packagerolloutinprogress</li><li>Packagerolloutcomplete</li><li>Packagerolloutbeendet</li></ul>  |  
| fallbacksubmissionid    |  string   |  Die ID der Übermittlung, die von Kunden empfangen wird, die die schrittweise Rollout Pakete nicht erhalten.   |          

> [!NOTE]
> Die Werte *packagerolloutstatus* und *fallbacksubmissionid* werden von Partner Center zugewiesen und sind nicht für die Festlegung durch den Entwickler vorgesehen. Wenn Sie diese Werte in einen Anforderungs Text einfügen, werden diese Werte ignoriert.

<span/>

## <a name="enums"></a>Enumerationen

Diese Methoden verwenden die folgenden enumeren.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Statuscode der Übermittlung

Die folgenden Codes stellen den Status einer Übermittlung dar.

| Code           |  Beschreibung      |
|-----------------|---------------|
|  Keine            |     Es wurde kein Code angegeben.         |     
|      Invalidarchive        |     Das ZIP-Archiv, das das Paket enthält, ist ungültig oder weist ein unbekanntes Archivformat auf.  |
| MissingFiles | Das ZIP-Archiv enthält nicht alle Dateien, die in den Übermittlungs Daten aufgelistet waren, oder Sie befinden sich an der falschen Stelle im Archiv. |
| Packagevalidationfailed | Mindestens ein Paket in der Übermittlung konnte nicht überprüft werden. |
| Invalidparametervalue | Einer der Parameter im Anforderungs Text ist ungültig. |
| InvalidOperation | Der Vorgang, den Sie versucht haben, ist ungültig. |
| Invalidstate | Der Vorgang, den Sie versucht haben, ist nicht gültig für den aktuellen Status des paketflugs. |
| ResourceNotFound | Der angegebene paketflug konnte nicht gefunden werden. |
| ServiceError | Ein interner Dienst Fehler hat die erfolgreiche Anforderung verhindert. Wiederholen Sie die Anforderung. |
| Listingoptoutwarning | Der Entwickler hat eine Auflistung aus einer vorherigen Übermittlung entfernt oder enthielt keine Auflistungs Informationen, die vom Paket unterstützt werden. |
| Listingoptinwarning  | Der Entwickler hat eine Liste hinzugefügt. |
| Updateonlywarning | Der Entwickler versucht, etwas einzufügen, das nur Aktualisierungs Unterstützung hat. |
| Sonstiges  | Die Übermittlung befindet sich in einem nicht erkannten oder nicht kategorisierten Zustand. |
| Packagevalidationwarning | Der Paket Überprüfungsprozess führte zu einer Warnung. |

<span/>

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Paket Flügen mithilfe der Microsoft Store Übermittlungs-API](manage-flights.md)
* [Get a Package Flight Submission](get-a-flight-submission.md)
* [Erstellen einer paketflight-Übermittlung](create-a-flight-submission.md)
* [Aktualisieren einer paketflight-Übermittlung](update-a-flight-submission.md)
* [Commit für eine Paket-Flight-Übermittlung](commit-a-flight-submission.md)
* [Löschen einer paketflight-Übermittlung](delete-a-flight-submission.md)
* [Den Status einer paketflight-Übermittlung erhalten](get-status-for-a-flight-submission.md)
