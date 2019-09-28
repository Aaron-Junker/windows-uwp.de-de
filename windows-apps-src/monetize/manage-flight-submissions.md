---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API, um die Übertragung von Paketen für apps zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Flight-Paket-Übermittlungen
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Flight-Übermittlungen
ms.localizationpriority: medium
ms.openlocfilehash: 813b1375b3bdba42f0568cb7d961f8108d5828a1
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340416"
---
# <a name="manage-package-flight-submissions"></a>Verwalten von Flight-Paket-Übermittlungen

Mithilfe der Methoden der Microsoft Store-Übermittlungs-API können Sie Flight-Paket-Übermittlungen für Ihre Apps verwalten, einschließlich gradueller Paketrollouts. Eine Einführung in die Microsoft Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Microsoft Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Wenn Sie die Microsoft Store Übermittlungs-API verwenden, um eine Übermittlung für einen paketflight zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur über die API anstelle von Partner Center vornehmen. Wenn Sie das Dashboard zum Ändern einer Übermittlung verwenden, die ursprünglich mit der API erstellt wurde, können Sie die Übermittlung nicht länger mithilfe der API ändern oder übermitteln. In einigen Fällen kann der Fehlerstatus der Übermittlung belassen werden, mit dem die Übermittlung nicht fortgesetzt werden kann. In diesem Fall müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Methoden zum Verwalten von Flight-Paket-Übermittlungen

Verwenden Sie die folgenden Methoden zum Abrufen, Erstellen, Aktualisieren, Committen oder Löschen einer Flight-Paket-Übermittlung. Bevor Sie diese Methoden verwenden können, muss der paketflug bereits im Partner Center vorhanden sein. Sie können einen paketflug [in Partner Center](https://docs.microsoft.com/windows/uwp/publish/package-flights) oder mithilfe der Methoden zur Microsoft Store Übermittlung von APIs in erstellen, die unter [Verwalten von Paket Flügen](manage-flights.md)beschrieben werden.

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

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Gehen Sie folgendermaßen vor, um eine Übermittlung für ein Flight-Paket zu erstellen.

1. Wenn Sie dies noch nicht getan haben, müssen Sie die unter [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Diensten](create-and-manage-submissions-using-windows-store-services.md)beschriebenen Voraussetzungen erfüllen. dazu gehören das Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto und das Abrufen der Client-ID und des Schlüssels. Sie müssen dies nur einmal durchführen. nachdem Sie Client-ID und Schlüssel erhalten haben, können Sie diese jedes Mal wiederverwenden, wenn Sie ein neues Azure AD-Token erstellen müssen.  

2. [Abrufen eines Azure AD-Zugriffstokens](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Sie müssen dieses Zugriffstoken an die Methoden in der Microsoft Store-Übermittlungs-API übergeben. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. [Erstellen Sie eine Flight-Paket-Übermittlung](create-a-flight-submission.md) mithilfe der folgenden Methode in der Microsoft Store-Übermittlungs-API. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
    ```

    Der Antworttext enthält eine [Flight-Übermittlung](#flight-submission-object)-ressource, die eine ID für die neue Übermittlung, die SAS-URI (Shared Access Signatur) zum Hochladen von Paketen für die Übermittlung an Azure Blob Storage und die Daten für die neue Übermittlung (inkl. alle Eintrags- und Preisinfos) enthält.

    > [!NOTE]
    > Ein SAS-URI ermöglicht den Zugriff auf eine sichere Ressource in Azure Storage, ohne dass Kontoschlüssel benötigt werden. Hintergrundinformationen zu SAS-URIs und deren Verwendung mit Azure BLOB Storage finden Sie unter [shared Access Signature, Part 1: Grundlegendes zum SAS-Modell @ no__t-0 und [shared Access Signature, Teil 2: Erstellen und Verwenden einer SAS mit BLOB Storage @ no__t-0.

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie [die Pakete vorbereiten](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) und einem ZIP-Archiv hinzufügen.

5. Revidieren Sie die [Flight-Übermittlungsdaten](#flight-submission-object)mit allen erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um [die Flight-Paket-Übermittlung zu aktualisieren](update-a-flight-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie die Übermittlungsdaten aktualisieren, damit diese auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv verweisen.

4. Wenn Sie neue Pakete für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv mit dem SAS-URI auf [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) hochladen, der im Antworttext der POST-Methode bereitgestellt wurde, die Sie zuvor aufgerufen haben. Zu diesem Zweck können Sie verschiedene Azure-Bibliotheken auf unterschiedlichen Plattformen verwenden, darunter:

    * [Azure Storage-Client Bibliothek für .net](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK für Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK für python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    Das folgende C#-Codebeispiel zeigt, wie Sie ein ZIP-Archiv mithilfe der [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob)-Klasse in der Azure Storage-Clientbibliothek für .NET auf Azure Blob Storage hochladen. Im Beispiel wird davon ausgegangen, dass das ZIP-Archiv bereits in ein Datenstromobjekt geschrieben wurde.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Führen Sie folgende Methode aus, um [die Flight-Paket-Übermittlung zu committen](commit-a-flight-submission.md). Dadurch wird Partner Center benachrichtigt, dass Sie Ihre Übermittlung abgeschlossen haben und dass Ihre Updates jetzt auf Ihr Konto angewendet werden sollen.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Überprüfen Sie den Commit-Status, indem Sie die folgende Methode ausführen, um [den Status der Flight-Paket-Übermittlung abzurufen](get-status-for-a-flight-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können den Übermittlungs Fortschritt weiterhin mithilfe der vorherigen Methode oder durch Besuch von Partner Center überwachen.

<span/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie eine Flight-Paket-Übermittlung in verschiedenen Programmiersprachen erstellen:

* [C#Codebeispiele](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java-Codebeispiele](java-code-examples-for-the-windows-store-submission-api.md)
* [Python-Codebeispiele](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell-Modul

Als Alternative zum direkten Aufruf der Microsoft Store-Übermittlungs-API stellen wir ein PowerShell-Modul (Open Source) bereit, das eine Befehlszeilenschnittstelle über der API implementiert. Dieses Modul heißt [StoreBroker](https://aka.ms/storebroker). Sie können dieses Modul verwenden, um Ihre App-, Flight- und Add-On-Übermittlungen über die Befehlszeile anstatt über die Microsoft Store-Übermittlungs-API direkt zu verwalten. Sie können auch ganz einfach die Quelle durchsuchen, um weitere Beispiele für das Aufrufen dieser API zu erhalten. Das StoreBroker-Modul wird innerhalb von Microsoft aktiv als primäre Methode verwendet, durch die viele Erstanbieter-Apps an den Store übermittelt werden.

Weitere Informationen finden Sie auf unserer [StoreBroker-Seite auf GitHub](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Verwalten eines graduellen Paketrollouts für eine Flight-Paket-Übermittlung

Sie können die aktualisierten Pakete in einer Flight-Paket-Übermittlung graduell für einen bestimmten Prozentsatz der Kunden Ihrer App unter Windows 10 einführen. So können Sie Feedback und Analysedaten für die jeweiligen Pakete überwachen und vor einem umfassenden Rollout sicherstellen, dass das Update ordnungsgemäß funktioniert. Sie können den Rollout-Prozentwert für eine veröffentlichte Übermittlung ändern (oder die Aktualisierung anhalten), ohne dass Sie eine neue Übermittlung erstellen müssen. Weitere Informationen, einschließlich Anleitungen zum Aktivieren und Verwalten einer schrittweisen Paket Bereitstellung in Partner Center, finden Sie in [diesem Artikel](../publish/gradual-package-rollout.md).

Um ein graduelles Paketrollout für eine Flight-Paket-Übermittlung programmgesteuert zu aktivieren, gehen Sie wie folgt vor, und verwenden Sie dabei Methoden in der Microsoft Store-Übermittlungs-API:

  1. [Erstellen Sie eine Flight-Paket-Übermittlung](create-a-flight-submission.md), oder [rufen Sie eine Flight-Paket-Übermittlung ab](get-a-flight-submission.md).
  2. Suchen Sie in den Antwortdaten die [packageRollout](#package-rollout-object)-Ressource, legen Sie das Feld *isPackageRollout* auf „true“ und das Feld *packageRolloutPercentage* auf den Prozentsatz der Kunden Ihrer App fest, die die aktualisierten Pakete erhalten sollen.
  3. Übergeben Sie die aktualisierten Daten der Flight-Paket-Übermittlung an die Methode zur [Aktualisierung einer Flight-Paket-Übermittlung](update-a-flight-submission.md).

Nachdem ein graduelles Paketrollout für eine Flight-Paket-Übermittlung aktiviert wurde, können Sie das graduelle Rollout mithilfe der folgenden Methoden programmgesteuert abrufen, aktualisieren, anhalten oder abschließen.

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

Die Methoden der Microsoft Store-Übermittlungs-API für das Verwalten von Flight-Paket-Übermittlungen verwenden die folgenden JSON-Datenressourcen.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Ressource für Flight-Paket-Übermittlungen

Diese Ressource beschreibt eine Flight-Paket-Übermittlung.

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

Die Ressource hat die folgenden Werte.

| Wert      | Typ   | Beschreibung              |
|------------|--------|------------------------------|
| id            | String  | Die ID für die Übermittlung.  |
| flightId           | String  |  Die ID des Flight-Pakets, dem die Übermittlung zugeordnet ist.  |  
| status           | String  | Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Version</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Eine [Ressource für Statusdetails](#status-details-object), die zusätzliche Details über den Status der Übermittlung enthält, einschließlich Fehlerinformationen.  |
| flightPackages           | array  | Enthält [Ressourcen für Flight-Pakete](#flight-package-object), die Details über die einzelnen Pakete in der Übermittlung bereitstellen.   |
| packageDeliveryOptions    | object  | Eine [Ressource für Paketübermittlungsoptionen](#package-delivery-options-object), die Einstellungen zu graduellen Paketrollouts und zu verpflichtenden Updates für die Übermittlung enthält.   |
| fileUploadUrl           | String  | Der Shared Access Signature (SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer Flight-Paket-Übermittlung](#create-a-package-flight-submission).  |
| targetPublishMode           | String  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | String  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| notesForCertification           | String  |  Enthält zusätzliche Informationen für Zertifizierungstester wie Anmeldeinformationen für Testkonten und Schritte zum Zugriff auf und zur Überprüfung von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource für Statusdetails

Diese Ressource enthält weitere Informationen über den Status einer Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung                   |
|-----------------|---------|------|
|  errors               |    object     |   Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Fehlerdetails zur Übermittlung enthalten.   |     
|  warnings               |   object      | Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Warnungsdetails zur Übermittlung enthalten.     |
|  certificationReports               |     object    |   Ein Array von [Ressourcen für Zertifizierungsberichte](#certification-report-object), die den Zugriff auf die Zertifizierungsberichtsdaten für die Übermittlung ermöglichen. Sie können diese Berichte auf weitere Informationen überprüfen, wenn die Zertifizierung nicht erfolgreich ist.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource für einzelne Statusdetails

Diese Ressource enthält weitere Informationen zu Fehlern oder Warnungen für eine Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|------|
|  code               |    String     |   Ein [Übermittlungsstatuscode](#submission-status-code), der den Fehler- oder Warnungstyp beschreibt. |  
|  details               |     String    |  Eine Meldung mit weiteren Details zum Problem.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource für Zertifizierungsberichte

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtsdaten für eine Übermittlung bereit. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung         |
|-----------------|---------|------|
|     date            |    String     |  Das Datum und die Uhrzeit, zu der der Bericht generiert wurde, im ISO 8601-Format.    |
|     reportUrl            |    String     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Flight-Paket-Ressource

Diese Ressource enthält Details zu einem Paket in einer Übermittlung.

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

Die Ressource hat die folgenden Werte.

> [!NOTE]
> Beim Aufruf der Methode für das [Aktualisieren einer Flight-Paket-Übermittlung](update-a-flight-submission.md) sind im Anforderungstext nur die Werte *fileName*, *fileStatus*, *minimumDirectXVersion* und *minimumSystemRam* dieses Objekts erforderlich. Die anderen Werte werden von Partner Center aufgefüllt.

| Wert           | Typ    | Beschreibung              |
|-----------------|---------|------|
| fileName   |   String      |  Der Name des Pakets.    |  
| fileStatus    | String    |  Der Status des Pakets. Folgende Werte sind möglich: <ul><li>Keine</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  String   |  Eine ID, die das Paket eindeutig identifiziert. Dieser Wert wird von Partner Center verwendet.   |     
| version    |  String   |  Die Version des App-Pakets. Weitere Informationen finden Sie unter [Paketversionsnummern](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  String   |  Die Architektur des App-Pakets (z. B. ARM).   |     
| languages    | array    |  Ein Array von Sprachcodes für die Sprachen, die von der App unterstützt werden. Weitere Informationen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Ein Array von Funktionen, die für das Paket erforderlich sind. Weitere Informationen zu Funktionen finden Sie unter [Deklaration der App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  String   |  Die DirectX-Version, die vom App-Paket mindestens unterstützt wird. Dieser Wert kann nur für Apps festgelegt werden, die für Windows 8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>Keine</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | String    |  Die Menge an RAM, die für das App-Paket mindestens erforderlich ist. Dieser Wert kann nur für Apps festgelegt werden, die für Windows 8.x bestimmt sind. Im Fall von Apps, die für andere Versionen bestimmt sind, wird er ignoriert. Folgende Werte sind möglich: <ul><li>Keine</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource für Paketübermittlungsoptionen

Diese Ressource enthält Einstellungen zu graduellen Paketrollouts und zu verpflichtenden Updates für die Übermittlung.

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

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| packageRollout   |   object      |   Eine [Ressource für Paketrollouts](#package-rollout-object), die Einstellungen zu graduellen Paketrollouts für die Übermittlung enthält.    |  
| isMandatoryUpdate    | boolean    |  Gibt an, ob die Pakete in dieser Übermittlung für automatisch installierte App-Updates als verpflichtend behandelt werden sollen. Weitere Informationen zu verpflichtenden Paketen für automatisch installierte App-Aktualisierungen finden Sie unter [Herunterladen und Installieren von Paketupdates für Ihre App](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  Zeitpunkt (Datum und Uhrzeit), zu dem die Pakete in dieser Übermittlung verpflichtend werden, im ISO 8601-Format und gemäß UTC-Zeitzone.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ressource für Paketrollouts

Diese Ressource enthält [Einstellungen für graduelle Paketrollouts](#manage-gradual-package-rollout) für die Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  Gibt an, ob für die Übermittlung der graduelle Paketrollout aktiviert ist.    |  
| packageRolloutPercentage    | float    |  Der Prozentsatz der Benutzer, die im Rahmen des graduellen Paketrollouts die Pakete erhalten.    |  
| packageRolloutStatus    |  String   |  Eine der folgenden Zeichenfolgen, die den Status des graduellen Paketrollouts angeben: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  String   |  Die ID der Übermittlung, die die Kunden erhalten, die keine Pakete im Rahmen des graduellen Paketrollouts erhalten.   |          

> [!NOTE]
> Die Werte *packagerolloutstatus* und *fallbacksubmissionid* werden von Partner Center zugewiesen und sind nicht für die Festlegung durch den Entwickler vorgesehen. Wenn Sie diese Werte in einen Anforderungstext einfügen, werden diese Werte ignoriert.

<span/>

## <a name="enums"></a>Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Übermittlungsstatuscode

Die folgenden Codes stellen den Status einer Übermittlung dar.

| Code           |  Beschreibung      |
|-----------------|---------------|
|  Keine            |     Es wurde kein Code angegeben.         |     
|      InvalidArchive        |     Das ZIP-Archiv, das das Paket enthält, ist ungültig oder hat ein unbekanntes Archivformat.  |
| MissingFiles | Das ZIP-Archiv enthält nicht alle Dateien, die in den Übermittlungsdaten aufgeführt sind, oder sie befinden sich am falschen Speicherort im Archiv. |
| PackageValidationFailed | Mindestens ein Paket in der Übermittlung konnte nicht überprüft werden. |
| InvalidParameterValue | Einer der Parameter im Anforderungstext ist ungültig. |
| InvalidOperation | Der von Ihnen versuchte Vorgang ist ungültig. |
| InvalidState | Der von Ihnen versuchte Vorgang ist für den aktuellen Zustand des Flight-Pakets ungültig. |
| ResourceNotFound | Das angegebene Flight-Paket konnte nicht gefunden werden. |
| ServiceError | Ein interner Dienstfehler hat verhindert, dass die Anforderung erfolgreich ausgeführt wurde. Führen Sie die Anforderung erneut aus. |
| ListingOptOutWarning | Der Entwickler hat einen Eintrag aus einer vorherigen Übermittlung entfernt oder Eintragsinformationen nicht hinzugefügt, die vom Paket unterstützt werden. |
| ListingOptInWarning  | Der Entwickler hat einen Eintrag hinzugefügt. |
| UpdateOnlyWarning | Der Entwickler versucht, etwas einzufügen, für das nur Aktualisierungsunterstützung verfügbar ist. |
| Andere  | Die Übermittlung befindet sich in einem nicht erkannten oder nicht kategorisierten Zustand. |
| PackageValidationWarning | Der Paketüberprüfungsvorgang hat zu einer Warnung geführt. |

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
