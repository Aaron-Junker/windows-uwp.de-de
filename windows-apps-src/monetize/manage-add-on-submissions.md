---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API, um Add-on-Übermittlungen für apps zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Add-On-Übermittlungen
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Add-on-Einreichungen, in-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 5b5926fbd55c215eccf3517454c972edc86f3dfd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164574"
---
# <a name="manage-add-on-submissions"></a>Verwalten von Add-On-Übermittlungen

Die Microsoft Store Übermittlungs-API stellt Methoden bereit, die Sie zum Verwalten von Add-on-Einreichungen (auch als in-App-Produkt oder IAP bezeichnet) für Ihre apps verwenden können. Eine Einführung in die Microsoft Store Übermittlungs-API, einschließlich der Voraussetzungen für die Verwendung der API, finden [Sie unter Erstellen und Verwalten von Übermittlungen mithilfe Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Wenn Sie die Microsoft Store Übermittlungs-API verwenden, um eine Übermittlung für ein Add-on zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur mithilfe der API vornehmen, anstatt Änderungen in Partner Center vorzunehmen. Wenn Sie Partner Center verwenden, um eine Übermittlung zu ändern, die Sie ursprünglich mit der API erstellt haben, können Sie diese Übermittlung nicht mehr mithilfe der API ändern oder ein Commit dafür durchführen. In einigen Fällen kann die Übermittlung in einem Fehlerzustand belassen werden, in dem Sie im Übermittlungs Prozess nicht fortgesetzt werden kann. Wenn dies auftritt, müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Methoden zum Verwalten von Add-on-Übermittlungen

Verwenden Sie die folgenden Methoden zum Abrufen, Erstellen, Aktualisieren, Committen oder Löschen einer Add-On-Übermittlung. Bevor Sie diese Methoden verwenden können, muss das Add-on bereits in Ihrem Partner Center-Konto vorhanden sein. Sie können ein Add-on im Partner Center erstellen, indem Sie den [Produkttyp und die Produkt-ID definieren](../publish/set-your-add-on-product-id.md) oder indem Sie die Methoden der Microsoft Store Übermittlungs-API verwenden, die unter [Verwalten von Add-ons](manage-add-ons.md)beschrieben sind

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
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Abrufen einer vorhandenen Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Abrufen des Status einer vorhandenen Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Erstellen einer neuen Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Aktualisieren einer vorhandenen Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Committen einer neuen oder aktualisierten Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">Delete</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Löschen einer Add-On-Übermittlung</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Gehen Sie folgendermaßen vor, um eine Übermittlung für ein Add-On zu erstellen.

1. Wenn Sie dies noch nicht getan haben, müssen Sie die unter [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Diensten](create-and-manage-submissions-using-windows-store-services.md)beschriebenen Voraussetzungen erfüllen. dazu gehören das Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto und das Abrufen der Client-ID und des Schlüssels. Sie müssen dies nur einmal durchführen. nachdem Sie Client-ID und Schlüssel erhalten haben, können Sie diese jedes Mal wiederverwenden, wenn Sie ein neues Azure AD-Token erstellen müssen.  

2. Rufen Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)ab. Sie müssen dieses Zugriffs Token an die Methoden in der Microsoft Store Übermittlungs-API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. Führen Sie die folgende Methode in der Microsoft Store Übermittlungs-API aus. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    Der Antworttext enthält eine [Add-on-](#add-on-submission-object) Übermittlungs Ressource, die die ID der neuen Übermittlung enthält, den SAS-URI (Shared Access Signature) zum Hochladen von Add-on-Symbolen für die Übermittlung an Azure BLOB Storage und alle Daten für die neue Übermittlung (z. b. die Auflistungen und Preisinformationen).

    > [!NOTE]
    > Ein SAS-URI ermöglicht den Zugriff auf eine sichere Ressource in Azure Storage, ohne dass Kontoschlüssel erforderlich sind. Hintergrundinformationen zu SAS-URIs und deren Verwendung mit Azure Blob Storage finden Sie unter [Shared Access Signatures, Teil 1: Verstehen des SAS-Modells](/azure/storage/common/storage-sas-overview) und [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit BLOB-Speicher](/azure/storage/common/storage-sas-overview).

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie [die Symbole vorbereiten](../publish/create-add-on-store-listings.md) und einem ZIP-Archiv hinzufügen.

5. Aktualisieren Sie die Übermittlungs Daten für das [Add-on](#add-on-submission-object) mit allen erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um die Übermittlung zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Wenn Sie neue Symbole für die Übermittlung hinzufügen, stellen Sie sicher, dass Sie die Übermittlungs Daten aktualisieren, um auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv zu verweisen.

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv mit dem SAS-URI auf [Azure Blob Storage](/azure/storage/storage-introduction#blob-storage) hochladen, der im Antworttext der POST-Methode bereitgestellt wurde, die Sie zuvor aufgerufen haben. Zu diesem Zweck können Sie verschiedene Azure-Bibliotheken auf unterschiedlichen Plattformen verwenden, darunter:

    * [Azure Storage-Client Bibliothek für .net](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage-SDK für Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK für python](/azure/storage/storage-python-how-to-use-blob-storage)

    Das folgende C#-Codebeispiel zeigt, wie Sie ein ZIP-Archiv mithilfe der [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob)-Klasse in der Azure Storage-Clientbibliothek für .NET auf Azure Blob Storage hochladen. Im Beispiel wird davon ausgegangen, dass das ZIP-Archiv bereits in ein Datenstromobjekt geschrieben wurde.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Führen Sie die folgende Methode aus, um die Übermittlung zu committen. Dadurch wird Partner Center benachrichtigt, dass Sie Ihre Übermittlung abgeschlossen haben und dass Ihre Updates jetzt auf Ihr Konto angewendet werden sollen. Weitere Informationen finden Sie unter [Ausführen eines Commits einer Add-On-Übermittlung](commit-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Sie überprüfen den Status des Commit durch Ausführen der folgenden Methode. Weitere Informationen finden Sie unter [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können den Übermittlungs Fortschritt weiterhin mithilfe der vorherigen Methode oder durch Besuch von Partner Center überwachen.

<span/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie eine Add-On-Übermittlung in verschiedenen Programmiersprachen erstellen:

* [C#-Codebeispiele](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java-Codebeispiele](java-code-examples-for-the-windows-store-submission-api.md)
* [Python-Codebeispiele](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Storebroker-PowerShell-Modul

Als Alternative zum direkten Aufrufen der Microsoft Store Übermittlungs-API stellen wir auch ein Open-Source-PowerShell-Modul bereit, das eine Befehlszeilenschnittstelle zusätzlich zur API implementiert. Dieses Modul heißt [storebroker](https://github.com/Microsoft/StoreBroker). Sie können dieses Modul verwenden, um Ihre APP-, Flug-und Add-on-Übermittlungen von der Befehlszeile aus zu verwalten, anstatt die Microsoft Store Übermittlungs-API direkt aufzurufen, oder Sie können einfach die Quelle durchsuchen, um weitere Beispiele zum Aufrufen dieser API anzuzeigen. Das storebroker-Modul wird in Microsoft aktiv als die primäre Art und Weise verwendet, dass viele Anwendungen von erst Anbietern an den Store übermittelt werden.

Weitere Informationen finden Sie [auf der Seite storebroker auf GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Datenressourcen

Die Methoden der Microsoft Store Übermittlungs-API zum Verwalten von Add-on-Übermittlungen verwenden die folgenden JSON-Datenressourcen.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>Ressource für Add-On-Übermittlungen

Diese Ressource beschreibt eine Add-On-Übermittlung.

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

Die Ressource hat die folgenden Werte.

| Wert      | Typ   | BESCHREIBUNG        |
|------------|--------|----------------------|
| id            | Zeichenfolge  | Die ID der Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer Add-on-Übermittlung](create-an-add-on-submission.md), zum [erhalten aller Add-ons](get-all-add-ons.md)und zum [erhalten eines Add-](get-an-add-on.md)ons verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |
| contentType           | Zeichenfolge  |  Der [Inhaltstyp](../publish/enter-add-on-properties.md#content-type), der im Add-On bereitgestellt wird. Mögliche Werte: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Ein Array von Zeichenfolgen, das bis zu 10 [Schlüsselwörter](../publish/enter-add-on-properties.md#keywords) für das Add-On enthalten kann. Die App kann mit diesen Schlüsselwörter Add-Ons abfragen.   |
| lifetime           | Zeichenfolge  |  Die Lebensdauer des Add-Ons. Mögliche Werte: <ul><li>Endlos</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | Objekt (object)  |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode und jeder Wert eine [Eintragsressource](#listing-object) ist, die Eintragsinformationen für das Add-On enthält.  |
| Preise           | Objekt (object)  | Eine [Preisressource](#pricing-object), die Preisinformationen für das Add-On enthält.   |
| targetPublishMode           | Zeichenfolge  | Der Veröffentlichungsmodus für die Übermittlung. Mögliche Werte: <ul><li>Unmittelbar</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | Zeichenfolge  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| tag           | Zeichenfolge  |  Die [benutzerdefinierten Entwicklerdaten](../publish/enter-add-on-properties.md#custom-developer-data) für das Add-On (diese Informationen wurden zuvor als *tag* bezeichnet).   |
| Sichtbarkeit  | Zeichenfolge  |  Die Sichtbarkeit des Add-Ons. Mögliche Werte: <ul><li>Ausgeblendet</li><li>Öffentlich</li><li>Privat</li><li>NotSet</li></ul>  |
| status  | Zeichenfolge  |  Der Status der Übermittlung. Mögliche Werte: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Veröffentlichung</li><li>Veröffentlicht</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Zertifizierung</li><li>CertificationFailed</li><li>Freigabe</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | Objekt (object)  |  Eine [Ressource für Statusdetails](#status-details-object), die zusätzliche Details über den Status der Übermittlung enthält, einschließlich Fehlerinformationen. |
| fileUploadUrl           | Zeichenfolge  | Der Shared Access Signature (SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](#create-an-add-on-submission).  |
| friendlyName  | Zeichenfolge  |  Der Anzeige Name der Übermittlung, wie in Partner Center gezeigt. Dieser Wert wird beim Erstellen der Übermittlung für Sie generiert.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Eintragsressource

Diese Ressource enthält [Auflistungs Informationen für ein Add-on](../publish/create-add-on-store-listings.md). Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG       |
|-----------------|---------|------|
|  description               |    Zeichenfolge     |   Die Beschreibung für den Add-On-Eintrag.   |     
|  icon               |   Objekt (object)      |Eine [Symbolressource](#icon-object), die Daten zum Symbol für den Add-On-Eintrag enthält.    |
|  title               |     Zeichenfolge    |   Der Titel für den Add-On-Eintrag.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Symbolressource

Diese Ressource enthält Symboldaten für einen Add-On-Eintrag. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG     |
|-----------------|---------|------|
|  fileName               |    Zeichenfolge     |   Der Name der Symboldatei im ZIP-Archiv, das Sie für die Übermittlung hochgeladen haben. Das Symbol muss eine PNG-Datei sein, die genau 300 x 300 Pixel misst.   |     
|  fileStatus               |   Zeichenfolge      |  Der Status der Symboldatei. Mögliche Werte: <ul><li>Keine</li><li>PendingUpload</li><li>Hochgeladen</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Preisressource

Diese Ressource enthält Preisinformationen für das Add-On. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG    |
|-----------------|---------|------|
|  marketSpecificPricings               |    Objekt (object)     |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](../publish/set-add-on-pricing-and-availability.md) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *priceId* angegebenen Basispreis für den angegebenen Markt.     |     
|  Vertrieb               |   array      |  **Veraltet**. Ein Array von [Verkaufsressourcen](#sale-object), die Verkaufsinformationen für das Add-On enthalten.     |     
|  priceId               |   Zeichenfolge      |  Ein [Preisniveau](#price-tiers), das den [Basispreis](../publish/set-add-on-pricing-and-availability.md) für das Add-On angibt.    |    
|  isadvancedpricingmodel               |   boolean      |  **True**gibt an, dass Ihr Entwicklerkonto über Zugriff auf die erweiterten Preisstufen von. 99 USD auf 1999,99 USD verfügt. Bei " **false**" hat ihr Entwicklerkonto Zugriff auf den ursprünglichen Satz von Preisstufen von. 99 USD auf 999,99 USD. Weitere Informationen zu den verschiedenen Tarifen finden Sie unter [Preisstufen](#price-tiers).<br/><br/>**Note** &nbsp; Hinweis &nbsp; Dieses Feld ist schreibgeschützt.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Verkaufsressource

Diese Ressource enthält die Verkaufsinformationen für ein Add-On.

> [!IMPORTANT]
> Die **Verkaufs** Ressource wird nicht mehr unterstützt, und Sie können die Verkaufsdaten für eine Add-on-Übermittlung nicht mehr mithilfe der Microsoft Store Übermittlungs-API erhalten oder ändern. In der Zukunft aktualisieren wir die Microsoft Store Übermittlungs-API, um eine neue Methode für den programmgesteuerten Zugriff auf Vertriebsinformationen für Add-on-Einreichungen einzuführen.
>    * Nach dem Aufrufen der [GET-Methode zum Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md) ist der Wert *sales* leer. Sie können das Partner Center weiterhin verwenden, um die Verkaufsdaten für Ihre Add-on-Übermittlung zu erhalten.
>    * Beim Aufrufen der [PUT-Methode zum Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md) werden die Informationen im Wert *sales* ignoriert. Sie können das Partner Center weiterhin verwenden, um die Verkaufsdaten für Ihre Add-on-Übermittlung zu ändern.

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG           |
|-----------------|---------|------|
|  name               |    Zeichenfolge     |   Der Name des Verkaufs.    |     
|  basePriceId               |   Zeichenfolge      |  Das [Preisniveau](#price-tiers), das für den Basispreis des Verkaufs verwendet werden soll.    |     
|  startDate               |   Zeichenfolge      |   Das Startdatum für den Verkauf im Format ISO 8601.  |     
|  endDate               |   Zeichenfolge      |  Das Enddatum für den Verkauf im Format ISO 8601.      |     
|  marketSpecificPricings               |   Objekt (object)      |   Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](../publish/set-add-on-pricing-and-availability.md) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *basePriceId* angegebenen Basispreis für den angegebenen Markt.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource für Statusdetails

Diese Ressource enthält weitere Informationen über den Status einer Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG       |
|-----------------|---------|------|
|  errors               |    Objekt (object)     |   Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Fehlerdetails zur Übermittlung enthalten.   |     
|  warnings               |   Objekt (object)      | Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Warnungsdetails zur Übermittlung enthalten.     |
|  certificationReports               |     Objekt (object)    |   Ein Array von [Ressourcen für Zertifizierungsberichte](#certification-report-object), die den Zugriff auf die Zertifizierungsberichtsdaten für die Übermittlung ermöglichen. Sie können diese Berichte auf weitere Informationen überprüfen, wenn die Zertifizierung nicht erfolgreich ist.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource für einzelne Statusdetails

Diese Ressource enthält weitere Informationen zu Fehlern oder Warnungen für eine Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG    |
|-----------------|---------|------|
|  code               |    Zeichenfolge     |   Ein [Übermittlungsstatuscode](#submission-status-code), der den Fehler- oder Warnungstyp beschreibt.   |     
|  Details               |     Zeichenfolge    |  Eine Meldung mit weiteren Details zum Problem.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource für Zertifizierungsberichte

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtsdaten für eine Übermittlung bereit. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG               |
|-----------------|---------|------|
|     date            |    Zeichenfolge     |  Das Datum und die Uhrzeit, zu der der Bericht generiert wurde, im ISO 8601-Format.    |
|     reportUrl            |    Zeichenfolge     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |

## <a name="enums"></a>Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.

<span id="price-tiers" />

### <a name="price-tiers"></a>Preisstufen

Die folgenden Werte stellen die verfügbaren Preisstufen in der Ressourcen Ressource " [Preise](#pricing-object) " für eine Add-on-Übermittlung dar.

| Wert           | BESCHREIBUNG       |
|-----------------|------|
|  Basis               |   Das Preisniveau ist nicht festgelegt. Verwenden Sie den Basispreis für das Add-On.      |     
|  NotAvailable              |   Das Add-On ist für die angegebene Region nicht verfügbar.    |     
|  Free              |   Das Add-On ist kostenlos.    |    
|  Ebene*xxxx*               |   Eine Zeichenfolge, die den Preistarif für das Add-on im Format **Tier<em>xxxx</em>** angibt. Derzeit werden die folgenden Preisstufen Bereiche unterstützt:<br/><br/><ul><li>Wenn der Wert *isadvancedpricingmodel* der [Preis Ressource](#pricing-object) **true**lautet, sind die verfügbaren Preisstufen Werte für Ihr Konto **Tier1012**  -  **Tier1424**.</li><li>Wenn der Wert *isadvancedpricingmodel* der [Preis Ressource](#pricing-object) **false**lautet, sind die verfügbaren Preisstufen Werte für Ihr Konto **Instanzen**  -  **Tier96**.</li></ul>Wenn Sie die gesamte Tabelle mit den Preisstufen anzeigen möchten, die für Ihr Entwicklerkonto verfügbar sind, einschließlich der marktspezifischen Preise **für die einzelnen** Tarife, besuchen Sie die Seite mit den Preisen **und Verfügbarkeit** Ihrer APP-Übermittlungen im Partner Center, und klicken Sie im Abschnitt " **Märkte und benutzerdefinierte Preise** " auf den Link " **Tabelle anzeigen** ".     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Übermittlungsstatuscode

Die folgenden Werte stellen den Statuscode einer Übermittlung dar.

| Wert           |  BESCHREIBUNG      |
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
| Sonstiges  | Die Übermittlung befindet sich in einem nicht erkannten oder nicht kategorisierten Zustand. |
| PackageValidationWarning | Der Paketüberprüfungsvorgang hat zu einer Warnung geführt. |

<span/>

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-ons mithilfe der Microsoft Store Übermittlungs-API](manage-add-ons.md)
* [Add-on-Einreichungen im Partner Center](../publish/add-on-submissions.md)