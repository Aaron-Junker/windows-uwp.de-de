---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Verwenden Sie diese Methoden in der Microsoft Store-Übermittlung API zum Verwalten von Add-on-Übermittlungen für apps, die mit Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Add-On-Übermittlungen
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Add-On-Übermittlungen, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 6a287889f27712d1272dd0a2f8f3d068b376fefe
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320201"
---
# <a name="manage-add-on-submissions"></a>Verwalten von Add-On-Übermittlungen

Mithilfe der Methoden der Microsoft Store-Übermittlungs-API können Sie Add-On-Übermittlungen für Ihre Apps verwalten (Add-Ons werden auch als In-App-Produkt bzw. IAP bezeichnet). Eine Einführung in die Microsoft Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Microsoft Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Wenn Sie die Übermittlung zum Microsoft Store-API verwenden, um eine Eingabe für ein Add-on zu erstellen, achten Sie darauf, dass Sie weitere Änderungen an der Übermittlung zu vornehmen, nur durch Verwendung der API, anstatt die Änderungen im Partner Center. Wenn Sie die Partner Center einen Antrag auf Zulassung zu ändern, den Sie ursprünglich erstellt haben, mit der API verwenden, sind Sie werden nicht mehr in der Lage, ändern oder übernehmen dieser Übermittlung mithilfe der API. In einigen Fällen kann der Fehlerstatus der Übermittlung belassen werden, mit dem die Übermittlung nicht fortgesetzt werden kann. In diesem Fall müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Methoden zum Verwalten von Add-On-Übermittlungen

Verwenden Sie die folgenden Methoden zum Abrufen, Erstellen, Aktualisieren, Committen oder Löschen einer Add-On-Übermittlung. Bevor Sie diese Methoden verwenden können, muss das Add-on bereits in Ihrem Partner Center-Konto vorhanden sein. Sie können ein Add-on erstellen, im Partner Center von [die Produkttyp und die Produkt-ID definieren](../publish/set-your-add-on-product-id.md) oder mithilfe von den Microsoft Store-Übermittlung-API-Methoden, die in beschriebenen [Add-ons verwalten](manage-add-ons.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Erhalten Sie ein vorhandenes Add-on-Übermittlung</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Abrufen des Status einer vorhandenen-Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Erstellen einer neuen Add-on-Übermittlung</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Aktualisieren einer vorhandenen-Add-On</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Übernehmen einer neuen oder aktualisierten-Add-On-Übermittlung</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Löschen Sie ein Add-on-Übermittlung</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Gehen Sie folgendermaßen vor, um eine Übermittlung für ein Add-On zu erstellen.

1. Wenn noch nicht geschehen, vollständige die Voraussetzungen im Abschnitt [erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md), einschließlich eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen und Abrufen Ihrer Client-ID und Schlüssel. Sie müssen dies nur einmal durchführen. nachdem Sie Client-ID und Schlüssel erhalten haben, können Sie diese jedes Mal wiederverwenden, wenn Sie ein neues Azure AD-Token erstellen müssen.  

2. [Abrufen eines Azure AD-Zugriffstokens](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Sie müssen dieses Zugriffstoken an die Methoden in der Microsoft Store-Übermittlungs-API übergeben. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

3. Führen Sie die folgende Methode in der Microsoft Store-Übermittlungs-API aus. Diese Methode erstellt eine neue laufende Übermittlung, die eine Kopie der letzten veröffentlichten Übermittlung ist. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](create-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    Der Antworttext enthält eine [Add-On-Übermittlungs](#add-on-submission-object)-Ressource, die die ID der neuen Übermittlung, die Daten für die neue Übermittlung (einschließlich aller Einträge und Preisinformationen) und den Shared Access Signature (SAS)-URI, um alle Add-On-Symbole für die Übermittlung auf Azure Blob Storage hochzuladen.

    > [!NOTE]
    > Ein SAS-URI ermöglicht den Zugriff auf eine sichere Ressource in Azure Storage, ohne dass Kontoschlüssel benötigt werden. Hintergrundinformationen zu SAS-URIs und ihrer Verwendung mit Azure Blob Storage finden Sie unter [Shared Access Signatures, Teil 1: Grundlagen zum SAS-Modell](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) und [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie [die Symbole vorbereiten](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions) und einem ZIP-Archiv hinzufügen.

5. Aktualisieren Sie die [Add-On-Übermittlungsdaten](#add-on-submission-object) mit alle erforderlichen Änderungen für die neue Übermittlung, und führen Sie die folgende Methode aus, um die Übermittlung zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie die Übermittlungsdaten aktualisieren, damit diese auf den Namen und den relativen Pfad dieser Dateien im ZIP-Archiv verweisen.

4. Wenn Sie neue Symbole für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv mit dem SAS-URI auf [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) hochladen, der im Antworttext der POST-Methode bereitgestellt wurde, die Sie zuvor aufgerufen haben. Zu diesem Zweck können Sie verschiedene Azure-Bibliotheken auf unterschiedlichen Plattformen verwenden, darunter:

    * [Azure Storage-Clientbibliothek für .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage-SDK für Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage-SDK für Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    Das folgende C#-Codebeispiel zeigt, wie Sie ein ZIP-Archiv mithilfe der [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?redirectedfrom=MSDN)-Klasse in der Azure Storage-Clientbibliothek für .NET auf Azure Blob Storage hochladen. Im Beispiel wird davon ausgegangen, dass das ZIP-Archiv bereits in ein Datenstromobjekt geschrieben wurde.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Führen Sie die folgende Methode aus, um die Übermittlung zu committen. Dies wird von Partner Center benachrichtigt, dass die Übermittlung abgeschlossen ist, und Ihre Änderungen jetzt auf Ihr Konto angewendet werden soll. Weitere Informationen finden Sie unter [Ausführen eines Commits einer Add-On-Übermittlung](commit-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Sie überprüfen den Status des Commit durch Ausführen der folgenden Methode. Weitere Informationen finden Sie unter [Abrufen des Status einer Add-On-Übermittlung](get-status-for-an-add-on-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Um den Status der Übermittlung zu überprüfen, zeigen Sie den Wert *status* im Antworttext an. Dieser Wert sollte von **CommitStarted** entweder in **PreProcessing** geändert worden sein, wenn die Anforderung erfolgreich war, oder in **CommitFailed**, wenn die Anforderung Fehler enthalten hat. Wenn Fehler aufgetreten sind, enthält das Feld *StatusDetails* Feld weitere Details zu den Fehlern.

7. Nachdem das Commit erfolgreich abgeschlossen wurde, wird die Übermittlung zur Aufnahme an den Store gesendet. Sie können weiterhin den Status der Übermittlung mithilfe der vorherigen Methode oder besuchen Sie die Partner Center überwacht werden soll.

<span/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie eine Add-On-Übermittlung in verschiedenen Programmiersprachen erstellen:

* [C#Codebeispiele](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java-Codebeispiele](java-code-examples-for-the-windows-store-submission-api.md)
* [Python-Codebeispiele](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell-Modul

Als Alternative zum direkten Aufruf der Microsoft Store-Übermittlungs-API stellen wir ein PowerShell-Modul (Open Source) bereit, das eine Befehlszeilenschnittstelle über der API implementiert. Dieses Modul heißt [StoreBroker](https://aka.ms/storebroker). Sie können dieses Modul verwenden, um Ihre App-, Flight- und Add-On-Übermittlungen über die Befehlszeile anstatt über die Microsoft Store-Übermittlungs-API direkt zu verwalten. Sie können auch ganz einfach die Quelle durchsuchen, um weitere Beispiele für das Aufrufen dieser API zu erhalten. Das StoreBroker-Modul wird innerhalb von Microsoft aktiv als primäre Methode verwendet, durch die viele Erstanbieter-Apps an den Store übermittelt werden.

Weitere Informationen finden Sie auf unserer [StoreBroker-Seite auf GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Datenressourcen

Die Methoden der Microsoft Store-Übermittlungs-API für das Verwalten von Add-On-Übermittlungen verwenden die folgenden JSON-Datenressourcen.

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

| Wert      | Typ   | Beschreibung        |
|------------|--------|----------------------|
| id            | String  | Die ID der Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen verfügbar, um [eine Add-On-Übermittlung zu erstellen](create-an-add-on-submission.md), [alle Add-Ons abzurufen](get-all-add-ons.md) und [ein Add-On abzurufen](get-an-add-on.md). Für eine Eingabe, die im Partner Center erstellt wurde, ist diese ID auch in die URL für die Seite für die Auftragsübermittlung im Partner Center verfügbar.  |
| contentType           | String  |  Der [Inhaltstyp](../publish/enter-add-on-properties.md#content-type), der im Add-On bereitgestellt wird. Folgende Werte sind möglich: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Ein Array von Zeichenfolgen, das bis zu 10 [Schlüsselwörter](../publish/enter-add-on-properties.md#keywords) für das Add-On enthalten kann. Die App kann mit diesen Schlüsselwörter Add-Ons abfragen.   |
| lifetime           | String  |  Die Lebensdauer des Add-Ons. Folgende Werte sind möglich: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode und jeder Wert eine [Eintragsressource](#listing-object) ist, die Eintragsinformationen für das Add-On enthält.  |
| pricing           | object  | Eine [Preisressource](#pricing-object), die Preisinformationen für das Add-On enthält.   |
| targetPublishMode           | String  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | String  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| tag           | String  |  Die [benutzerdefinierten Entwicklerdaten](../publish/enter-add-on-properties.md#custom-developer-data) für das Add-On (diese Informationen wurden zuvor als *tag* bezeichnet).   |
| visibility  | String  |  Die Sichtbarkeit des Add-Ons. Folgende Werte sind möglich: <ul><li>Ausgeblendet</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | String  |  Der Status der Übermittlung. Folgende Werte sind möglich: <ul><li>Keine</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Version</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Eine [Ressource für Statusdetails](#status-details-object), die zusätzliche Details über den Status der Übermittlung enthält, einschließlich Fehlerinformationen. |
| fileUploadUrl           | String  | Der Shared Access Signature (SAS)-URI für das Hochladen der Pakete für die Übermittlung. Wenn Sie neue Pakete oder Bilder für die Übermittlung hinzufügen, müssen Sie das ZIP-Archiv, das die Pakete enthält, zu dieser URI hochladen. Weitere Informationen finden Sie unter [Erstellen einer Add-On-Übermittlung](#create-an-add-on-submission).  |
| friendlyName  | String  |  Der Anzeigename der Übermittlung, wie im Partner Center angezeigt. Dieser Wert wird beim Erstellen der Übermittlung für Sie generiert.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Eintragsressource

Diese Ressource enthält die [Eintragsinformationen für ein Add-On](../publish/create-add-on-store-listings.md). Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|------|
|  description               |    String     |   Die Beschreibung für den Add-On-Eintrag.   |     
|  Symbol               |   object      |Eine [Symbolressource](#icon-object), die Daten zum Symbol für den Add-On-Eintrag enthält.    |
|  title               |     String    |   Der Titel für den Add-On-Eintrag.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Symbolressource

Diese Ressource enthält Symboldaten für einen Add-On-Eintrag. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung     |
|-----------------|---------|------|
|  fileName               |    String     |   Der Name der Symboldatei im ZIP-Archiv, das Sie für die Übermittlung hochgeladen haben. Das Symbol muss eine PNG-Datei mit exakt 300 x 300 Pixeln sein.   |     
|  fileStatus               |   String      |  Der Status der Symboldatei. Folgende Werte sind möglich: <ul><li>Keine</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Preisressource

Diese Ressource enthält Preisinformationen für das Add-On. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *priceId* angegebenen Basispreis für den angegebenen Markt.     |     
|  sales               |   array      |  **Veraltet** Ein Array von [Verkaufsressourcen](#sale-object), die Verkaufsinformationen für das Add-On enthalten.     |     
|  priceId               |   String      |  Ein [Preisniveau](#price-tiers), das den [Basispreis](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability) für das Add-On angibt.    |    
|  isAdvancedPricingModel               |   boolean      |  Bei **true** hat Ihr Entwicklerkonto Zugriff auf die erweiterten Spanne von Preisstufen von 0,99 US-Dollar bis 1.999,99 US-Dollar. Bei **false** hat Ihr Entwicklerkonto Zugriff auf die ursprüngliche Spanne von Preisstufen von 0,99 US-Dollar bis 999,99 US-Dollar. Weitere Informationen zu den verschiedenen Stufen finden Sie unter [Preisstufen](#price-tiers).<br/><br/>**Hinweis**&nbsp;&nbsp;Dieses Feld ist schreibgeschützt.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Verkaufsressource

Diese Ressource enthält die Verkaufsinformationen für ein Add-On.

> [!IMPORTANT]
> Die **Verkaufsressource** wird nicht mehr unterstützt. Zurzeit können Sie die Verkaufsdaten einer Add-On-Übermittlung nicht mithilfe der Microsoft Store-Übermittlungs-API abrufen oder ändern. Die Microsoft Store-Übermittlungs-API wird in der Zukunft aktualisiert werden, um ein neues Verfahren für den programmgesteuerten Zugriff auf Verkaufsinformationen für Add-On-Übermittlungen einzuführen.
>    * Nach dem Aufrufen der [GET-Methode zum Abrufen einer Add-On-Übermittlung](get-an-add-on-submission.md) ist der Wert *sales* leer. Sie können weiterhin auf Partner Center zu verwenden, um die Verkauf-Daten für die Übermittlung Add-on abzurufen.
>    * Beim Aufrufen der [PUT-Methode zum Aktualisieren einer Add-On-Übermittlung](update-an-add-on-submission.md) werden die Informationen im Wert *sales* ignoriert. Sie können weiterhin auf Partner Center verwenden, um die Verkauf-Daten für die Übermittlung Add-on zu ändern.

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung           |
|-----------------|---------|------|
|  NAME               |    String     |   Der Name des Verkaufs.    |     
|  basePriceId               |   String      |  Das [Preisniveau](#price-tiers), das für den Basispreis des Verkaufs verwendet werden soll.    |     
|  startDate               |   String      |   Das Startdatum für den Verkauf im Format ISO 8601.  |     
|  endDate               |   String      |  Das Enddatum für den Verkauf im Format ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Ein Verzeichnis von Schlüssel-Wert-Paaren, wobei jeder Schlüssel ein aus zwei Buchstaben bestehender ISO 3166-1-Alpha-2-Ländercode ist und jeder Wert ein [Preisniveau](#price-tiers) ist. Diese Elemente stellen die [benutzerdefinierten Preise für Ihr Add-On in bestimmten Märkten](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability) dar. Alle Elemente in diesem Verzeichnis überschreiben den durch den Wert *basePriceId* angegebenen Basispreis für den angegebenen Markt.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource für Statusdetails

Diese Ressource enthält weitere Informationen über den Status einer Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|------|
|  errors               |    object     |   Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Fehlerdetails zur Übermittlung enthalten.   |     
|  warnings               |   object      | Ein Array von [Ressourcen für einzelne Statusdetails](#status-detail-object), die Warnungsdetails zur Übermittlung enthalten.     |
|  certificationReports               |     object    |   Ein Array von [Ressourcen für Zertifizierungsberichte](#certification-report-object), die den Zugriff auf die Zertifizierungsberichtsdaten für die Übermittlung ermöglichen. Sie können diese Berichte auf weitere Informationen überprüfen, wenn die Zertifizierung nicht erfolgreich ist.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource für einzelne Statusdetails

Diese Ressource enthält weitere Informationen zu Fehlern oder Warnungen für eine Übermittlung. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung    |
|-----------------|---------|------|
|  code               |    String     |   Ein [Übermittlungsstatuscode](#submission-status-code), der den Fehler- oder Warnungstyp beschreibt.   |     
|  details               |     String    |  Eine Meldung mit weiteren Details zum Problem.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource für Zertifizierungsberichte

Diese Ressource stellt den Zugriff auf die Zertifizierungsberichtsdaten für eine Übermittlung bereit. Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung               |
|-----------------|---------|------|
|     date            |    String     |  Das Datum und Uhrzeit, die der Bericht, im ISO 8601-Format generiert wurde.    |
|     reportUrl            |    String     |  Die URL, unter der Sie auf den Bericht zugreifen können.    |

## <a name="enums"></a>Enumerationen

Diese Methoden verwenden die folgenden Enumerationen.

<span id="price-tiers" />

### <a name="price-tiers"></a>Preisniveaus

Die folgenden Werte stellen die verfügbaren Preisstufen in der [Ressource für Preise](#pricing-object) Ressource für eine Add-On-Übermittlung dar.

| Wert           | Beschreibung       |
|-----------------|------|
|  Base               |   Das Preisniveau ist nicht festgelegt. Verwenden Sie den Basispreis für das Add-On.      |     
|  NotAvailable              |   Das Add-On ist für die angegebene Region nicht verfügbar.    |     
|  Free              |   Das Add-On ist kostenlos.    |    
|  Stufe*xxxx*               |   Eine Zeichenfolge, die die Preisstufe für das Add-On im Format **Stufe<em>xxxx</em>** angibt. Derzeit werden die folgenden Spannen von Preisstufen unterstützt:<br/><br/><ul><li>Wenn der Wert *IsAdvancedPricingModel* für die [Ressource für Preise](#pricing-object)**true** ist, sind die für Ihr Konto verfügbaren Werte für Preisstufen **Stufe1012** - **Stufe1424**.</li><li>Wenn der Wert *IsAdvancedPricingModel* für die [Ressource für Preise](#pricing-object)**false** ist, sind die für Ihr Konto verfügbaren Werte für Preisstufen **Stufe1012** - **Stufe1424**.</li></ul>Wechseln Sie zu, auf der gesamten Tabelle der Preis Ebenen, die für Ihr Entwicklerkonto, einschließlich der marktspezifische-Preise, jedem Tarif zugeordnet sind, verfügbar sind, finden Sie unter den **Preise und Verfügbarkeit** Seite für eine der Übermittlung von Ihrer Apps im Partner Center, und klicken Sie auf die **Sichttabelle** -link in der **Märkte und benutzerdefinierte Preise** Abschnitt (für einige entwicklerkonten Link finden Sie der **Preise** Abschnitt).     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Übermittlungsstatuscode

Die folgenden Werte stellen den Statuscode einer Übermittlung dar.

| Wert           |  Description      |
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

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-ons, die über die Übermittlung zum Microsoft Store-API](manage-add-ons.md)
* [Add-On-Übermittlungen im Partner Center](https://docs.microsoft.com/windows/uwp/publish/iap-submissions)
