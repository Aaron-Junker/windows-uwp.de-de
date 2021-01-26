---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API, um Einsendungen für apps zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Verwalten von App-Übermittlungen
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, App-Übermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 00820f00360575f0a335d37aa0859b94648709e3
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811282"
---
# <a name="manage-app-submissions"></a>Verwalten von App-Übermittlungen

Die Microsoft Store Übermittlungs-API stellt Methoden bereit, mit denen Sie Übermittlungen für Ihre apps verwalten können, einschließlich der schrittweisen Paket Rollouts. Eine Einführung in die Microsoft Store Übermittlungs-API, einschließlich der Voraussetzungen für die Verwendung der API, finden [Sie unter Erstellen und Verwalten von Übermittlungen mithilfe Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Wenn Sie die Microsoft Store Übermittlungs-API verwenden, um eine Übermittlung für eine APP zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur über die API anstelle von Partner Center vornehmen. Wenn Sie Partner Center verwenden, um eine Übermittlung zu ändern, die Sie ursprünglich mit der API erstellt haben, können Sie diese Übermittlung nicht mehr mithilfe der API ändern oder ein Commit dafür durchführen. In einigen Fällen kann die Übermittlung in einem Fehlerzustand belassen werden, in dem Sie im Übermittlungs Prozess nicht fortgesetzt werden kann. Wenn dies auftritt, müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

> [!IMPORTANT]
> Sie können diese API nicht zum Veröffentlichen von Übermittlungen [über die Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen](../publish/organizational-licensing.md) oder Veröffentlichen von Übermittlungen für [LOB-apps](../publish/distribute-lob-apps-to-enterprises.md) direkt an Unternehmen verwenden. Für beide Szenarien müssen Sie das Partner Center verwenden, um die Übermittlung zu veröffentlichen.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Methoden zum Verwalten von App-Übermittlungen

Verwenden Sie die folgenden Methoden zum Abrufen, Erstellen, Aktualisieren, Committen oder Löschen einer App-Übermittlung. Bevor Sie diese Methoden verwenden können, muss die APP bereits in Ihrem Partner Center-Konto vorhanden sein, und Sie müssen zunächst eine Übermittlung für die APP im Partner Center erstellen. Weitere Informationen finden Sie unter [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Abrufen einer vorhandenen App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Abrufen des Status einer vorhandenen App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Erstellen einer neuen App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Aktualisieren einer vorhandenen App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Committen einer neuen oder aktualisierten App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Löschen einer App-Übermittlung</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Gehen Sie folgendermaßen vor, um eine Übermittlung für eine App zu erstellen.

1. Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
    > [!NOTE]
    > Stellen Sie sicher, dass für die APP bereits mindestens eine abgeschlossene Übermittlung mit den Informationen in den [Alters Bewertungs](../publish/age-ratings.md) Informationen vorhanden ist.

2. Rufen Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)ab. Sie müssen dieses Zugriffs Token an die Methoden in der Microsoft Store Übermittlungs-API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. [Erstellen Sie eine APP-Übermittlung](create-an-app-submission.md) , indem Sie die folgende Methode in der Microsoft Store Übermittlungs-API ausführen. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    Der Antworttext enthält eine Ressource zur [App-Übermittlung](#app-submission-object) , die die ID der neuen Übermittlung enthält, den SAS-URI (Shared Access Signature) für das Hochladen verwandter Dateien für die Übermittlung an Azure BLOB Storage (z. b. app-Pakete, Auflistungs Bilder und Nachspann Dateien) und alle Daten für die neue Übermittlung (z. b. die Auflistungen und Preisinformationen).

    > [!NOTE]
    > Ein SAS-URI ermöglicht den Zugriff auf eine sichere Ressource in Azure Storage, ohne dass Kontoschlüssel erforderlich sind. Hintergrundinformationen zu SAS-URIs und deren Verwendung mit Azure BLOB Storage finden Sie unter [Shared Access Signature, Part 1: Grundlegendes zum SAS-Modell](/azure/storage/common/storage-sas-overview) und [Shared Access Signature, Teil 2: Erstellen und Verwenden einer SAS mit BLOB Storage](/azure/storage/common/storage-sas-overview).

4. Wenn Sie neue Pakete, Auflistung von Bildern oder Nachspann Dateien für die Übermittlung hinzufügen, [bereiten Sie die APP-Pakete](../publish/app-package-requirements.md) vor, und bereiten Sie [die APP-Screenshots,-Images und-](../publish/app-screenshots-and-images.md)Auflistungen vor. Fügen Sie all diese Dateien einem ZIP-Archiv hinzu.

5. Überarbeiten Sie die [App](#app-submission-object) -Übermittlungs Daten mit allen erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um [die APP-Übermittlung zu aktualisieren](update-an-app-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Wenn Sie neue Dateien für die Übermittlung hinzufügen, stellen Sie sicher, dass Sie die Übermittlungs Daten aktualisieren, um auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv zu verweisen.

4. Wenn Sie neue Pakete, Auflistung von Bildern oder Nachspann Dateien für die Übermittlung hinzufügen, laden Sie das ZIP-Archiv in [Azure BLOB Storage](/azure/storage/storage-introduction#blob-storage) mit dem SAS-URI hoch, der im Antworttext der Post-Methode bereitgestellt wurde, die Sie zuvor aufgerufen haben. Zu diesem Zweck können Sie verschiedene Azure-Bibliotheken auf unterschiedlichen Plattformen verwenden, darunter:

    * [Azure Storage-Client Bibliothek für .net](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage-SDK für Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK für python](/azure/storage/storage-python-how-to-use-blob-storage)

    Im folgenden c#-Codebeispiel wird veranschaulicht, wie ein ZIP-Archiv in Azure BLOB Storage mithilfe der [cloudblockblob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) -Klasse in der Azure Storage-Client Bibliothek für .net hochgeladen wird. Im Beispiel wird davon ausgegangen, dass das ZIP-Archiv bereits in ein Datenstromobjekt geschrieben wurde.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Führen Sie folgende Methode aus, um [die App-Übermittlung zu committen](commit-an-app-submission.md). Dadurch wird Partner Center benachrichtigt, dass Sie Ihre Übermittlung abgeschlossen haben und dass Ihre Updates jetzt auf Ihr Konto angewendet werden sollen.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Überprüfen Sie den Commit-Status, indem Sie die folgende Methode ausführen, um [den Status der App-Übermittlung abzurufen](get-status-for-an-app-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können den Übermittlungs Fortschritt weiterhin mithilfe der vorherigen Methode oder durch Besuch von Partner Center überwachen.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Methoden zum Verwalten eines graduellen Paketrollouts

Sie können die aktualisierten Pakete in einer App-Übermittlung graduell für einen bestimmten Prozentsatz der Kunden Ihrer App unter Windows 10 einführen. So können Sie Feedback und Analysedaten für die jeweiligen Pakete überwachen und vor einem umfassenden Rollout sicherstellen, dass das Update ordnungsgemäß funktioniert. Sie können den Rollout-Prozentwert für eine veröffentlichte Übermittlung ändern (oder die Aktualisierung anhalten), ohne dass Sie eine neue Übermittlung erstellen müssen. Weitere Informationen, einschließlich Anleitungen zum Aktivieren und Verwalten einer schrittweisen Paket Bereitstellung in Partner Center, finden Sie in [diesem Artikel](../publish/gradual-package-rollout.md).

Wenn Sie eine schrittweise Paket Bereitstellung für eine APP-Übermittlung Programm gesteuert aktivieren möchten, befolgen Sie dieses Verfahren mithilfe der Methoden in der Microsoft Store Übermittlungs-API:

  1. [Erstellen Sie eine App-Übermittlung](create-an-app-submission.md), oder [rufen Sie eine vorhandene App-Übermittlung ab](get-an-app-submission.md).
  2. Suchen Sie in den Antwortdaten die Ressource " [packagerollout](#package-rollout-object) ", legen Sie das Feld " *ispackagerollout* " auf " **true**" fest, und legen Sie das Feld " *packagerolloutprozent* " auf den Prozentsatz der Kunden der APP fest, der die aktualisierten Pakete erhalten soll.
  3. Übergeben Sie die aktualisierten App-Übermittlungsdaten an die Methode zum [Aktualisieren einer App-Übermittlung](update-an-app-submission.md).

Nachdem ein graduelles Paketrollout für eine App-Übermittlung aktiviert wurde, können Sie das graduelle Rollout mithilfe der folgenden Methoden programmgesteuert abrufen, aktualisieren, anhalten oder abschließen.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Abrufen von Informationen zum graduellen Rollout einer App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Aktualisieren des Prozentsatzes eines graduellen Rollouts einer App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Anhalten des graduellen Rollouts einer App-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Abschließen des graduellen Rollouts einer App-Übermittlung</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Codebeispiele für die Verwaltung von App-Übermittlungen

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie eine App-Übermittlung in verschiedenen Programmiersprachen erstellen:

* [C#-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](java-code-examples-for-the-windows-store-submission-api.md)
* [Java-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](python-code-examples-for-the-windows-store-submission-api.md)
* [Python-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Storebroker-PowerShell-Modul

Als Alternative zum direkten Aufrufen der Microsoft Store Übermittlungs-API stellen wir auch ein Open-Source-PowerShell-Modul bereit, das eine Befehlszeilenschnittstelle zusätzlich zur API implementiert. Dieses Modul heißt [storebroker](https://github.com/Microsoft/StoreBroker). Sie können dieses Modul verwenden, um Ihre APP-, Flug-und Add-on-Übermittlungen von der Befehlszeile aus zu verwalten, anstatt die Microsoft Store Übermittlungs-API direkt aufzurufen, oder Sie können einfach die Quelle durchsuchen, um weitere Beispiele zum Aufrufen dieser API anzuzeigen. Das storebroker-Modul wird in Microsoft aktiv als die primäre Art und Weise verwendet, dass viele Anwendungen von erst Anbietern an den Store übermittelt werden.

Weitere Informationen finden Sie [auf der Seite storebroker auf GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Datenressourcen
Die Methoden der Microsoft Store Übermittlungs-API zum Verwalten von App-Übermittlungen verwenden die folgenden JSON-Datenressourcen

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Ressource für App-Übermittlungen

Diese Ressource beschreibt eine App-Übermittlung.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

Die Ressource hat die folgenden Werte.

| Wert      | type   | BESCHREIBUNG      |
|------------|--------|-------------------|
| id            | Zeichenfolge  | Die ID der Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md), zum [erhalten aller apps](get-all-apps.md)und zum [erhalten einer APP](get-an-app.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |
| applicationCategory           | Zeichenfolge  |   Eine Zeichenfolge, die [Kategorie und/oder Unterkategorie](../publish/category-and-subcategory-table.md) für Ihre App angibt. Kategorien und Unterkategorien werden mit einem Unterstrich „_“ zu einer einzigen Zeichenfolge zusammengefasst, z. B. **BooksAndReference_EReader**.      |  
| Preise           |  object  | Eine [Preisressource](#pricing-object), die Preisinformationen für die App enthält.        |   
| Sichtbarkeit           |  Zeichenfolge  |  Die Sichtbarkeit der App. Mögliche Werte: <ul><li>Ausgeblendet</li><li>Öffentlich</li><li>Privat</li><li>NotSet</li></ul>       |   
| targetPublishMode           | Zeichenfolge  | Der Veröffentlichungsmodus für die Übermittlung. Mögliche Werte: <ul><li>Unmittelbar</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | Zeichenfolge  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |  
| listings           |   object  |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei ein Schlüssel ein Ländercode und ein Wert eine [Eintragsressource](#listing-object) ist, die Eintragsinfos für die App enthält.       |   
| hardwarePreferences           |  array  |   Ein Array von Zeichenfolgen, die die [Hardwareeinstellungen](../publish/enter-app-properties.md) für die App definieren. Mögliche Werte: <ul><li>Touch</li><li>Tastatur</li><li>Maus</li><li>Kamera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Gibt an, ob Windows die App-Daten in automatische Sicherungen auf OneDrive aufnehmen können. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).   |   
| canInstallOnRemovableMedia           |  boolean  |   Gibt an, ob Kunden die App auf Wechselmedien installieren können. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).     |   
| isGameDvrEnabled           |  boolean |   Gibt an, ob game DVR für die App aktiviert ist.    |   
| gamingoptions           |  array |   Ein Array, das eine [Gaming Options-Ressource](#gaming-options-object) enthält, die die spielbezogenen Einstellungen für die APP definiert.     |   
| hasExternalInAppProducts           |     boolean          |   Gibt an, ob Ihre APP es Benutzern ermöglicht, den Einkauf außerhalb des Microsoft Store Commerce-Systems zu tätigen. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).     |   
| meetAccessibilityGuidelines           |    boolean           |  Gibt an, ob getestet wurde, ob die App die Richtlinien zur Barrierefreiheit erfüllt. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).      |   
| notesForCertification           |  Zeichenfolge  |   Enthält [Hinweise zur Zertifizierung](../publish/notes-for-certification.md) für Ihre App.    |    
| status           |   Zeichenfolge  |  Der Status der Übermittlung. Mögliche Werte: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Veröffentlichung</li><li>Veröffentlicht</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Zertifizierung</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  | Eine [Ressource für Statusdetails](#status-details-object), die zusätzliche Details über den Status der Übermittlung enthält, einschließlich Fehlerinformationen.       |    
| fileUploadUrl           |   Zeichenfolge  | Der Shared Access Signature (SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete, Auflistung von Bildern oder Nachspann Dateien für die Übermittlung hinzufügen, laden Sie das ZIP-Archiv, das die Pakete und Bilder enthält, in diesen URI hoch. Weitere Informationen finden Sie unter [Erstellen einer App-Übermittlung](#create-an-app-submission).       |    
| applicationPackages           |   array  | Ein Array von [Ressourcen für Anwendungspakete](#application-package-object), die Details über die einzelnen Pakete in der Übermittlung bereitstellen. |    
| packageDeliveryOptions    | object  | Eine [Ressource für Paketübermittlungsoptionen](#package-delivery-options-object), die Einstellungen zu graduellen Paketrollouts und zu verpflichtenden Updates für die Übermittlung enthält.  |
| enterpriseLicensing           |  Zeichenfolge  |  Einer der [Werte für Unternehmenslizenzierung](#enterprise-licensing), die das Verhalten der Unternehmenslizenzierung für die App angeben.  |    
| allowmicrosoftdecideappavailabilitydefuturedevicefamilies           |  boolean   |  Gibt an, ob Microsoft [die App für zukünftige Windows 10-Gerätefamilien verfügbar machen](../publish/set-app-pricing-and-availability.md) darf.    |    
| allowTargetFutureDeviceFamilies           | object   |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel eine [Windows 10-Gerätefamilie](../publish/set-app-pricing-and-availability.md) ist und jeder Wert ein boolescher Wert ist, der angibt, ob Ihre App auf die angegebene Gerätefamilie ausgerichtet werden darf.     |    
| friendlyName           |   Zeichenfolge  |  Der Anzeige Name der Übermittlung, wie in Partner Center gezeigt. Dieser Wert wird beim Erstellen der Übermittlung für Sie generiert.       |  
| Bet           |  array |   Ein Array, das bis zu 15 Nachspann [Ressourcen](#trailer-object) enthält, die videonachspann für die APP-Auflistung darstellen.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Preisressource

Diese Ressource enthält Preisinformationen für die App. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
|  trialPeriod               |    Zeichenfolge     |  Eine Zeichenfolge, die den Testzeitraum für die App angibt. Mögliche Werte: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihre App in bestimmten Märkten](../publish/define-market-selection.md) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *priceId* angegebenen Basispreis für den angegebenen Markt.      |     
|  Vertrieb               |   array      |  **Veraltet**. Ein Array von [Verkaufsressourcen](#sale-object), die Verkaufsinformationen für die App enthalten.   |     
|  priceId               |   Zeichenfolge      |  Ein [Preisniveau](#price-tiers), das den [Basispreis](../publish/define-market-selection.md) für die App angibt.   |     
|  isadvancedpricingmodel               |   boolean      |  **True** gibt an, dass Ihr Entwicklerkonto über Zugriff auf die erweiterten Preisstufen von. 99 USD auf 1999,99 USD verfügt. Bei " **false**" hat ihr Entwicklerkonto Zugriff auf den ursprünglichen Satz von Preisstufen von. 99 USD auf 999,99 USD. Weitere Informationen zu den verschiedenen Tarifen finden Sie unter [Preisstufen](#price-tiers).<br/><br/> &nbsp; Hinweis &nbsp; Dieses Feld ist schreibgeschützt.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Verkaufsressource

Diese Ressource enthält die Verkaufsinformationen für eine App.

> [!IMPORTANT]
> Die **Verkaufs** Ressource wird nicht mehr unterstützt. derzeit können Sie die Verkaufsdaten für eine APP-Übermittlung nicht mithilfe der Microsoft Store Übermittlungs-API übermitteln oder ändern. In der Zukunft aktualisieren wir die Microsoft Store Übermittlungs-API, um eine neue Methode für den programmgesteuerten Zugriff auf Vertriebsinformationen für die Übermittlung von apps einzuführen.
>    * Nach dem Aufrufen der [GET-Methode zum Abrufen einer App-Übermittlung](get-an-app-submission.md) ist der Wert *sales* leer. Sie können das Partner Center weiterhin verwenden, um die Verkaufsdaten für Ihre APP-Übermittlung zu erhalten.
>    * Beim Aufrufen der [PUT-Methode zum Aktualisieren einer App-Übermittlung](update-an-app-submission.md) werden die Informationen im Wert *sales* ignoriert. Sie können das Partner Center weiterhin verwenden, um die Verkaufsdaten für Ihre APP-Übermittlung zu ändern.

Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung    |
|-----------------|---------|------|
|  name               |    Zeichenfolge     |   Der Name des Verkaufs.    |     
|  basePriceId               |   Zeichenfolge      |  Das [Preisniveau](#price-tiers), das für den Basispreis des Verkaufs verwendet werden soll.    |     
|  startDate               |   Zeichenfolge      |   Das Startdatum für den Verkauf im Format ISO 8601.  |     
|  endDate               |   Zeichenfolge      |  Das Enddatum für den Verkauf im Format ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihre App in bestimmten Märkten](../publish/define-market-selection.md) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *basePriceId* angegebenen Basispreis für den angegebenen Markt.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Eintragsressource

Diese Ressource enthält die Eintragsinformationen für eine App. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung                  |
|-----------------|---------|------|
|  baseListing               |   object      |  Die Informationen für den [Basiseintrag](#base-listing-object) für die App, die die standardmäßigen Eintragsinformationen für alle Plattformen definiert.   |     
|  platformOverrides               | object |   Ein Verzeichnis von Schlüssel-Wert-Paaren, in denen jeder Schlüssel eine Zeichenfolge ist, die eine Plattform identifiziert, für die die Eintragsinformationen überschrieben werden sollen, und jeder Wert eine [Basiseintragsressource](#base-listing-object) ist (die nur die Werte von Beschreibung bis Titel enthält), die die Eintragsinformationen angibt, die für die angegebene Plattform überschrieben werden sollen. Die Schlüssel können die folgenden Werte haben: <ul><li>Unbekannt</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Ressource für Basiseinträge

Diese Ressource enthält die Basiseintragsinformationen für eine App. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   Zeichenfolge      |  Optionale [Copyright- und/oder Markeninformationen](../publish/create-app-store-listings.md).  |
|  keywords                |  array       |  Ein Array von [keyword](../publish/create-app-store-listings.md), um die Anzeige Ihrer App in Suchergebnissen zu unterstützen.    |
|  licenseTerms                |    Zeichenfolge     | Die optionalen [Lizenzbestimmungen](../publish/create-app-store-listings.md) für Ihre App.     |
|  privacyPolicy                |   Zeichenfolge      |   Dieser Wert ist veraltet. Um die URL für die Datenschutzrichtlinie für Ihre APP festzulegen oder zu ändern, müssen Sie dies auf der [Eigenschaften](../publish/enter-app-properties.md#privacy-policy-url) Seite im Partner Center vornehmen. Sie können diesen Wert aus den Aufrufen der Übermittlungs-API weglassen. Wenn Sie diesen Wert festlegen, wird er ignoriert.       |
|  supportContact                |   Zeichenfolge      |  Dieser Wert ist veraltet. Um die Support-URL oder e-Mail-Adresse für Ihre APP festzulegen oder zu ändern, müssen Sie dies auf der  [Eigenschaften](../publish/enter-app-properties.md#support-contact-info) Seite im Partner Center vornehmen. Sie können diesen Wert aus den Aufrufen der Übermittlungs-API weglassen. Wenn Sie diesen Wert festlegen, wird er ignoriert.        |
|  websiteUrl                |   Zeichenfolge      |  Dieser Wert ist veraltet. Um die URL der Webseite für Ihre APP festzulegen oder zu ändern, müssen Sie dies auf der  [Eigenschaften](../publish/enter-app-properties.md#website) Seite im Partner Center vornehmen. Sie können diesen Wert aus den Aufrufen der Übermittlungs-API weglassen. Wenn Sie diesen Wert festlegen, wird er ignoriert.      |    
|  description               |    Zeichenfolge     |   Die [Beschreibung](../publish/create-app-store-listings.md) für den App-Eintrag.   |     
|  Features               |    array     |  Ein Array von bis zu 20 Zeichenfolgen, die die [Features](../publish/create-app-store-listings.md) für Ihre App auflisten.     |
|  releaseNotes               |  Zeichenfolge       |  Die [Versionshinweise](../publish/create-app-store-listings.md) für Ihre App.    |
|  images               |   array      |  Ein Array von [Bild- und Symbolressourcen](#image-object) für den App-Eintrag.  |
|  recommendedHardware               |   array      |  Ein Array von bis zu 11 Zeichenfolgen, die die [empfohlenen Hardwarekonfigurationen](../publish/create-app-store-listings.md#additional-information) für Ihre App auflisten.     |
|  minimumhardware               |     Zeichenfolge    |  Ein Array von bis zu 11 Zeichen folgen, die die [minimalen Hardware Konfigurationen](../publish/create-app-store-listings.md#additional-information) für Ihre APP auflisten.    |  
|  title               |     Zeichenfolge    |   Der Titel für den App-Eintrag.   |  
|  Kurzbeschreibung               |     Zeichenfolge    |  Wird nur für Spiele verwendet. Diese Beschreibung wird im Abschnitt **Informationen** des spielhubs auf Xbox One angezeigt und hilft Kunden dabei, mehr über Ihr Spiel zu erfahren.   |  
|  Kurztitel               |     Zeichenfolge    |  Eine kürzere Version des Produkt namens. Falls angegeben, wird dieser kürzere Name möglicherweise an verschiedenen Stellen auf Xbox One (während der Installation, in Errungenschaften usw.) anstelle des vollständigen Titels Ihres Produkts angezeigt.    |  
|  sorttitle               |     Zeichenfolge    |   Wenn Ihr Produkt auf verschiedene Weise alphabetisch sortiert werden kann, können Sie hier eine andere Version eingeben. Dies kann Kunden dabei helfen, das Produkt bei der Suche schneller zu finden.    |  
|  Stimm Titel               |     Zeichenfolge    |   Ein alternativer Name für Ihr Produkt, der ggf. in der Audiodarstellung auf Xbox One bei Verwendung von kinect oder einem Headset verwendet werden kann.    |  
|  DevStudio               |     Zeichenfolge    |   Geben Sie diesen Wert an, wenn Sie ein Feld " **entwickelt von** " in die Auflistung einschließen möchten. (Im Feld **veröffentlicht von** wird der Anzeige Name des Herausgebers aufgelistet, der Ihrem Konto zugeordnet ist, unabhängig davon, ob Sie einen *DevStudio* -Wert angeben.)    |  

<span id="image-object" />

### <a name="image-resource"></a>Bildressource

Diese Ressource enthält Bild- und Symboldaten für einen App-Eintrag. Weitere Informationen zu Bildern und Symbolen für eine APP-Auflistung finden Sie unter [App-Screenshots und Bilder](../publish/app-screenshots-and-images.md). Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung           |
|-----------------|---------|------|
|  fileName               |    Zeichenfolge     |   Der Name der Bilddatei im ZIP-Archiv, das Sie für die Übermittlung hochgeladen haben.    |     
|  fileStatus               |   Zeichenfolge      |  Der Status der Bilddatei. Mögliche Werte: <ul><li>Keine</li><li>PendingUpload</li><li>Hochgeladen</li><li>PendingDelete</li></ul>   |
|  id  |  Zeichenfolge  | Die ID für das Bild. Dieser Wert wird von Partner Center bereitgestellt.  |
|  description  |  Zeichenfolge  | Die Beschreibung für das Bild.  |
|  imageType  |  Zeichenfolge  | Gibt den Typ des Bilds an. Die folgenden Zeichen folgen werden derzeit unterstützt. <p/>[Bildschirm](../publish/app-screenshots-and-images.md#screenshots)Abbildung: <ul><li>Bildschirm Abbildung (verwenden Sie diesen Wert für den Desktop Bildschirm Bildschirm)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Store-Logos](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>Storelogosquare</li><li>Symbol (verwenden Sie diesen Wert für das Logo 1:1 300 x 300 Pixel)</li></ul><p/>[Werbebilder](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Xbox-Bilder](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>Xboxbrandkeyart</li><li>Xboxtitledheroart</li><li>Xboxfeaturedpromotionalart</li></ul><p/>[Optionale Werbebilder](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Gaming Options-Ressource

Diese Ressource enthält spielbezogene Einstellungen für die app. Die Werte in dieser Ressource entsprechen den [Spieleinstellungen](../publish/enter-app-properties.md#game-settings) für Übermittlungen im Partner Center.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
|  genres               |    array     |  Ein Array aus einer oder mehreren der folgenden Zeichen folgen, die die Genres des Spiels beschreiben: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  islocalmultiplayer               |    boolean     |  Gibt an, ob das Spiel lokalen Multiplayer unterstützt.      |     
|  islocalkooperative               |   boolean      |  Gibt an, ob das Spiel das lokale Co-op unterstützt.    |     
|  isonlinemultiplayer               |   boolean      |  Gibt an, ob das Spiel Online-Multiplayer unterstützt.    |     
|  isonlinecooperative               |   boolean      |  Gibt an, ob das Spiel Online-Co-op unterstützt.    |     
|  localmultiplayerminplayers               |   INT      |   Gibt die Mindestanzahl von Playern an, die das Spiel für den lokalen Multiplayer unterstützt.   |     
|  localmultiplayermaxplayers               |   INT      |   Gibt die maximale Anzahl der Spieler an, die das Spiel für den lokalen Multiplayer unterstützt.  |     
|  localkooperativeminplayers               |   INT      |   Gibt die Mindestanzahl von Playern an, die das Spiel für das lokale Co-op unterstützt.  |     
|  localkooperativemaxplayers               |   INT      |   Gibt die maximale Anzahl der Spieler an, die das Spiel für das lokale Co-op unterstützt.  |     
|  isbroadcastingprivilegeerteilt               |   boolean      |  Gibt an, ob das Spiel Broadcasting unterstützt.   |     
|  iscrossplayaktivierte               |   boolean      |   Gibt an, ob das Spiel multiplayersitzungen zwischen Playern auf Windows 10-PCs und Xbox unterstützt.  |     
|  kinectdataforextern               |   Zeichenfolge      |  Einer der folgenden Zeichen folgen Werte, der angibt, ob das Spiel kinect-Daten sammeln und an externe Dienste senden kann: <ul><li>NotSet</li><li>Unbekannt</li><li>Enabled</li><li>Disabled</li></ul>   |

> [!NOTE]
> Die Ressource *gamingoptions* wurde im Mai 2017 hinzugefügt, nachdem die Microsoft Store Übermittlungs-API erstmals für Entwickler freigegeben wurde. Wenn Sie vor der Einführung dieser Ressource eine Übermittlung für eine APP über die Übermittlungs-API erstellt haben und diese Übermittlung noch nicht ausgeführt wird, ist diese Ressource für die Übermittlung der APP NULL, bis Sie erfolgreich ein Commit für die Übermittlung ausführen oder Sie löschen. Wenn die Ressource " *gamingoptions* " für Übermittlungen für eine APP nicht verfügbar ist, ist das Feld *hasadvancedlistingberechtigung* der [Anwendungs Ressource](get-app-data.md#application_object) , die von der Methode [Get an App](get-an-app.md) zurückgegeben wird, auf false festgelegt.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource für Statusdetails

Diese Ressource enthält weitere Informationen über den Status einer Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung         |
|-----------------|---------|------|
|  errors               |    object     |   Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Fehlerdetails zur Übermittlung enthalten.    |     
|  warnings               |   object      | Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Warnungsdetails zur Übermittlung enthalten.      |
|  certificationReports               |     object    |   Ein Array von [Ressourcen für Zertifizierungsberichte](#certification-report-object), die den Zugriff auf die Zertifizierungsberichtsdaten für die Übermittlung ermöglichen. Sie können diese Berichte auf weitere Informationen überprüfen, wenn die Zertifizierung nicht erfolgreich ist.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource für einzelne Statusdetails

Diese Ressource enthält weitere Informationen zu Fehlern oder Warnungen für eine Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
|  code               |    Zeichenfolge     |   Ein [Übermittlungsstatuscode](#submission-status-code), der den Fehler- oder Warnungstyp beschreibt.   |     
|  Details               |     Zeichenfolge    |  Eine Meldung mit weiteren Details zum Problem.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>Ressource für Anwendungspakete

Diese Ressource enthält Details zu einem App-Paket für die Übermittlung.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

Die Ressource hat die folgenden Werte.  

> [!NOTE]
> Beim Aufrufen der [App](update-an-app-submission.md) -Übermittlungs Methode aktualisieren werden nur die Werte *filename*, *File Status*, *minimumdirectxversion* und *minimumsystemram* dieses Objekts im Anforderungs Text benötigt. Die anderen Werte werden von Partner Center aufgefüllt.

| Wert           | type    | Beschreibung                   |
|-----------------|---------|------|
| fileName   |   Zeichenfolge      |  Der Name des Pakets.    |  
| fileStatus    | Zeichenfolge    |  Der Status des Pakets. Mögliche Werte: <ul><li>Keine</li><li>PendingUpload</li><li>Hochgeladen</li><li>PendingDelete</li></ul>    |  
| id    |  Zeichenfolge   |  Eine ID, die das Paket eindeutig identifiziert. Dieser Wert wird von Partner Center bereitgestellt.   |     
| version    |  Zeichenfolge   |  Die Version des App-Pakets. Weitere Informationen finden Sie unter [Paketversionsnummern](../publish/package-version-numbering.md).   |   
| Architektur    |  Zeichenfolge   |  Die Architektur des Pakets (z. B. ARM).   |     
| languages    | array    |  Ein Array von Sprachcodes für die Sprachen, die von der App unterstützt werden. Weitere Informationen finden Sie unter [Unterstützte Sprachen](../publish/supported-languages.md).    |     
| capabilities    |  array   |  Ein Array von Funktionen, die für das Paket erforderlich sind. Weitere Informationen zu Funktionen finden Sie unter [Deklaration der App-Funktionen](../packaging/app-capability-declarations.md).   |     
| minimumDirectXVersion    |  Zeichenfolge   |  Die DirectX-Version, die vom App-Paket mindestens unterstützt wird. Dies kann nur für apps festgelegt werden, die auf Windows 8. x abzielen. Für apps, die auf andere Betriebssystemversionen abzielen, muss dieser Wert vorhanden sein, wenn die APP-Übermittlungs Methode [Aktualisieren](update-an-app-submission.md) aufgerufen wird, der von Ihnen angegebene Wert jedoch ignoriert wird. Mögliche Werte: <ul><li>Keine</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | Zeichenfolge    |  Die Menge an RAM, die für das App-Paket mindestens erforderlich ist. Dies kann nur für apps festgelegt werden, die auf Windows 8. x abzielen. Für apps, die auf andere Betriebssystemversionen abzielen, muss dieser Wert vorhanden sein, wenn die APP-Übermittlungs Methode [Aktualisieren](update-an-app-submission.md) aufgerufen wird, der von Ihnen angegebene Wert jedoch ignoriert wird. Mögliche Werte: <ul><li>Keine</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Ein Array von Zeichenfolgen, die die Gerätefamilien darstellen, auf die das Paket ausgerichtet ist. Dieser Wert wird nur für Pakete verwendet, die für Windows 10 bestimmt sind. Im Fall von Paketen, die für frühere Versionen bestimmt sind, hat dieser Wert den Wert **None**. Die folgenden Gerätefamilien Zeichenfolgen werden derzeit für Windows 10-Pakete unterstützt, wobei *{0}* eine Windows 10-Versions Zeichenfolge ist, z. b. 10.0.10240.0, 10.0.10586.0 oder 10.0.14393.0: <ul><li>Windows. Universal min. Version *{0}*</li><li>Windows. Desktop min. Version *{0}*</li><li>Windows. Mobile min. Version *{0}*</li><li>Windows. Xbox min-Version *{0}*</li><li>Windows. Holographic min. Version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource für Zertifizierungsberichte

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtsdaten für eine Übermittlung bereit. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung             |
|-----------------|---------|------|
|     date            |    Zeichenfolge     |  Das Datum und die Uhrzeit, zu der der Bericht generiert wurde, im ISO 8601-Format.    |
|     reportUrl            |    Zeichenfolge     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource für Paketübermittlungsoptionen

Diese Ressource enthält Einstellungen zu graduellen Paketrollouts und zu verpflichtenden Updates für die Übermittlung.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
| packageRollout   |   object      |  Eine [Ressource für Paketrollouts](#package-rollout-object), die Einstellungen zu graduellen Paketrollouts für die Übermittlung enthält.   |  
| isMandatoryUpdate    | boolean    |  Gibt an, ob die Pakete in dieser Übermittlung für automatisch installierte App-Updates als verpflichtend behandelt werden sollen. Weitere Informationen zu verpflichtenden Paketen für automatisch installierte App-Aktualisierungen finden Sie unter [Herunterladen und Installieren von Paketupdates für Ihre App](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  Zeitpunkt (Datum und Uhrzeit), zu dem die Pakete in dieser Übermittlung verpflichtend werden, im ISO 8601-Format und gemäß UTC-Zeitzone.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ressource für Paketrollouts

Diese Ressource enthält [Einstellungen für graduelle Paketrollouts](#manage-gradual-package-rollout) für die Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  Gibt an, ob für die Übermittlung der graduelle Paketrollout aktiviert ist.    |  
| packageRolloutPercentage    | float    |  Der Prozentsatz der Benutzer, die im Rahmen des graduellen Paketrollouts die Pakete erhalten.    |  
| packageRolloutStatus    |  Zeichenfolge   |  Eine der folgenden Zeichenfolgen, die den Status des graduellen Paketrollouts angeben: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  Zeichenfolge   |  Die ID der Übermittlung, die die Kunden erhalten, die keine Pakete im Rahmen des graduellen Paketrollouts erhalten.   |          

> [!NOTE]
> Die Werte *packagerolloutstatus* und *fallbacksubmissionid* werden von Partner Center zugewiesen und sind nicht für die Festlegung durch den Entwickler vorgesehen. Wenn Sie diese Werte in einen Anforderungs Text einfügen, werden diese Werte ignoriert.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Trailers-Ressource

Diese Ressource stellt einen Video Nachspann für die APP-Auflistung dar. Die Werte in dieser Ressource entsprechen den Nachspann [Optionen für](../publish/app-screenshots-and-images.md#trailers) Übermittlungen im Partner Center.

Sie können bis zu 15 Nachspann *Ressourcen zum Nachspann Array in* einer [App](#app-submission-object)-Übermittlungs Ressource hinzufügen. Fügen Sie diese Dateien dem gleichen ZIP-Archiv hinzu, das die Pakete und die Auflistungs Bilder für die Übermittlung enthält, und laden Sie dieses ZIP-Archiv dann in den SAS-URI (Shared Access Signature) für die Übermittlung hoch, um Videodateien und Miniaturbilder für die Übermittlung hochzuladen. Weitere Informationen zum Hochladen des ZIP-Archivs in den SAS-URI finden Sie unter [Erstellen einer APP-Übermittlung](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

Die Ressource hat die folgenden Werte.

| Wert           | type    | BESCHREIBUNG        |
|-----------------|---------|------|
|  id               |    Zeichenfolge     |   Die ID für den Nachspann. Dieser Wert wird von Partner Center bereitgestellt.   |
|  videofilename               |    Zeichenfolge     |    Der Name der Videodatei im ZIP-Archiv, das Dateien für die Übermittlung enthält.    |     
|  videofilleid               |   Zeichenfolge      |  Die ID für die Nachspann-Videodatei. Dieser Wert wird von Partner Center bereitgestellt.   |     
|  trailerassets               |   object      |  Ein Wörterbuch von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein Sprachcode ist und jeder Wert eine Nachspann [Ressourcen-Ressource](#trailer-assets-object) ist, die zusätzliche Gebiets Schema spezifische Ressourcen für den Nachspann enthält. Weitere Informationen zu den unterstützten Sprachcodes finden Sie [unter Unterstützte Sprachen](../publish/supported-languages.md).    |     

> [!NOTE]
> Die Ressource " *Trailers* " wurde im Mai 2017 hinzugefügt, nachdem die Microsoft Store Übermittlungs-API erstmals für Entwickler freigegeben wurde. Wenn Sie vor der Einführung dieser Ressource eine Übermittlung für eine APP über die Übermittlungs-API erstellt haben und diese Übermittlung noch nicht ausgeführt wird, ist diese Ressource für die Übermittlung der APP NULL, bis Sie erfolgreich ein Commit für die Übermittlung ausführen oder Sie löschen. Wenn die " *Trailers* "-Ressource für Übermittlungen für eine APP nicht verfügbar ist, ist das *hasadvancedlistingberechtigung* -Feld der [Anwendungs Ressource](get-app-data.md#application_object) , das von der Methode [Get an App](get-an-app.md) zurückgegeben wird, false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Nachspann Ressourcen-Ressource

Diese Ressource enthält zusätzliche Gebiets Schema spezifische Assets für einen Nachspann, der in einer nach Spann [Ressource](#trailer-object)definiert ist. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung        |
|-----------------|---------|------|
| title   |   Zeichenfolge      |  Der lokalisierte Titel des Nachspann. Der Titel wird angezeigt, wenn der Benutzer den Nachspann im Vollbildmodus abspielt.     |  
| ImageList    | array    |   Ein Array, das eine [Bild](#image-for-trailer-object) Ressource enthält, die das Miniaturbild für den Nachspann bereitstellt. Sie können nur eine [Bild](#image-for-trailer-object) Ressource in dieses Array einschließen.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Bildressource (für einen Nachspann)

Diese Ressource beschreibt das Miniaturbild für einen Nachspann. Die Ressource hat die folgenden Werte.

| Wert           | type    | Beschreibung           |
|-----------------|---------|------|
|  fileName               |    Zeichenfolge     |   Der Name der Miniaturbild Datei im ZIP-Archiv, den Sie für die Übermittlung hochgeladen haben.    |     
|  id  |  Zeichenfolge  | Die ID für das Miniaturbild. Dieser Wert wird von Partner Center bereitgestellt.  |
|  description  |  Zeichenfolge  | Die Beschreibung des Miniatur Bilds. Dieser Wert ist nur Metadaten und wird Benutzern nicht angezeigt.   |

<span/>

## <a name="enums"></a>Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.

<span id="price-tiers" />

### <a name="price-tiers"></a>Preisstufen

Die folgenden Werte stellen die verfügbaren Tarife in der Ressourcen Ressource " [Preise](#pricing-object) " für eine APP-Übermittlung dar.

| Wert           | Beschreibung        |
|-----------------|------|
|  Basis               |   Das Preisniveau ist nicht festgelegt. Verwenden Sie den Basispreis für die App.      |     
|  NotAvailable              |   Die App ist für die angegebene Region nicht verfügbar.    |     
|  Kostenlos              |   Die Apps ist kostenlos.    |    
|  Ebene *xxx*               |   Eine Zeichenfolge, die die Preis Ebene für die APP im Format **Tier <em>xxxx</em>** angibt. Derzeit werden die folgenden Preisstufen Bereiche unterstützt:<br/><br/><ul><li>Wenn der Wert *isadvancedpricingmodel* der [Preis Ressource](#pricing-object) **true** lautet, sind die verfügbaren Preisstufen Werte für Ihr Konto **Tier1012**  -  **Tier1424**.</li><li>Wenn der Wert *isadvancedpricingmodel* der [Preis Ressource](#pricing-object) **false** lautet, sind die verfügbaren Preisstufen Werte für Ihr Konto **Instanzen**  -  **Tier96**.</li></ul>Wenn Sie die gesamte Tabelle mit den Preisstufen anzeigen möchten, die für Ihr Entwicklerkonto verfügbar sind, einschließlich der marktspezifischen Preise **für die einzelnen** Tarife, besuchen Sie die Seite mit den Preisen **und Verfügbarkeit** Ihrer APP-Übermittlungen im Partner Center, und klicken Sie im Abschnitt " **Märkte und benutzerdefinierte Preise** " auf den Link " **Tabelle anzeigen** ".    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Enterprise-Lizenzwerte

Die folgenden Werte stellen das Organisations Lizenzierungs Verhalten der APP dar. Weitere Informationen zu diesen Optionen finden Sie unter [Lizenzierungsoptionen für Unternehmen](../publish/organizational-licensing.md).

> [!NOTE]
> Obwohl Sie die Organisations Lizenzierungsoptionen für eine APP-Übermittlung über die Übermittlungs-API konfigurieren können, können Sie diese API nicht zum Veröffentlichen von Übermittlungen für [Volumen Einkäufe über die Microsoft Store for Business und Microsoft Store for Education](../publish/organizational-licensing.md)verwenden. Zum Veröffentlichen von Übermittlungen im Microsoft Store für Unternehmen und Microsoft Store for Education müssen Sie Partner Center verwenden.


| Wert           |  Beschreibung      |
|-----------------|---------------|
| Keine            |     Ihre App soll Unternehmen nicht über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) zur Verfügung gestellt werden.         |     
| Online        |     Ihre App soll Unternehmen über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) zur Verfügung gestellt werden.  |
| OnlineAndOffline | Ihre App soll Unternehmen über die Store-verwaltete Volumenlizenzierung (Onlinevolumenlizenzierung) sowie über die Offlinelizenzierung zur Verfügung gestellt werden. |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>Übermittlungsstatuscode

Die folgenden Werte stellen den Statuscode einer Übermittlung dar.

| Wert           |  Beschreibung      |
|-----------------|---------------|
| Keine            |     Es wurde kein Code angegeben.         |     
| InvalidArchive        |     Das ZIP-Archiv, das das Paket enthält, ist ungültig oder hat ein unbekanntes Archivformat.  |
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
* [App-Daten mithilfe der Microsoft Store Übermittlungs-API](get-app-data.md)
* [Übermittlungen von apps im Partner Center](../publish/app-submissions.md)
