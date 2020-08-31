---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene App-Übermittlung zu aktualisieren.
title: Aktualisieren einer App-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, App-Übermittlung, Update
ms.localizationpriority: medium
ms.openlocfilehash: 393b7c48723409dfc630c6f85770c4c0f7dcd1a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158364"
---
# <a name="update-an-app-submission"></a>Aktualisieren einer App-Übermittlung

Verwenden Sie diese Methode in der Microsoft Store Übermittlungs-API, um eine vorhandene App-Übermittlung zu aktualisieren. Nachdem Sie mit dieser Methode eine Übermittlung erfolgreich aktualisiert haben, müssen Sie ein [Commit für die Übermittlung](commit-an-app-submission.md) für Aufnahme und Veröffentlichung durchführen.

Weitere Informationen dazu, wie diese Methode in den Prozess der Erstellung einer APP-Übermittlung mithilfe der Microsoft Store Übermittlungs-API passt, finden Sie unter [Verwalten von App](manage-app-submissions.md)-Übermittlungen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Übermittlung für eine Ihrer Apps. Dies können Sie in Partner Center tun, oder Sie können dies mithilfe der Methode zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md) tun.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Zeichenfolge | Erforderlich. Die Store-ID der App, für die Sie eine Übermittlung aktualisieren möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).  |
| submissionId | Zeichenfolge | Erforderlich. Die ID der zu aktualisierenden Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer APP-Übermittlung](create-an-app-submission.md)verfügbar. Für eine Übermittlung, die im Partner Center erstellt wurde, ist diese ID auch in der URL für die Übermittlungs Seite im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

| Wert      | Typ   | BESCHREIBUNG                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | Zeichenfolge  |   Eine Zeichenfolge, die [Kategorie und/oder Unterkategorie](../publish/category-and-subcategory-table.md) für Ihre App angibt. Kategorien und Unterkategorien werden mit einem Unterstrich „_“ zu einer einzigen Zeichenfolge zusammengefasst, z. B. **BooksAndReference_EReader**.      |  
| Preise           |  Objekt (object)  | Ein Objekt, das Preisinfos für die App enthält. Weitere Informationen finden Sie im Abschnitt [Preisressource](manage-app-submissions.md#pricing-object).       |   
| Sichtbarkeit           |  Zeichenfolge  |  Die Sichtbarkeit der App. Mögliche Werte: <ul><li>Ausgeblendet</li><li>Öffentlich</li><li>Privat</li><li>NotSet</li></ul>       |   
| targetPublishMode           | Zeichenfolge  | Der Veröffentlichungsmodus für die Übermittlung. Mögliche Werte: <ul><li>Unmittelbar</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | Zeichenfolge  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |  
| listings           |   Objekt (object)  |  Ein Wörterbuch von Schlüssel-Wert-Paaren, wobei ein Schlüssel ein Ländercode und ein Wert eine [Eintragsressourcen](manage-app-submissions.md#listing-object)-Objekt ist, das Eintragsinfos für die App enthält.       |   
| hardwarePreferences           |  array  |   Ein Array von Zeichenfolgen, die die [Hardwareeinstellungen](../publish/enter-app-properties.md) für die App definieren. Mögliche Werte: <ul><li>Touch</li><li>Tastatur</li><li>Maus</li><li>Kamera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Gibt an, ob Windows die App-Daten in automatische Sicherungen auf OneDrive aufnehmen können. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).   |   
| canInstallOnRemovableMedia           |  boolean  |   Gibt an, ob Kunden die App auf Wechselmedien installieren können. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).     |   
| isGameDvrEnabled           |  boolean |   Gibt an, ob game DVR für die App aktiviert ist.    |   
| gamingoptions           |  Objekt (object) |   Ein Array, das eine [Gaming Options-Ressource](manage-app-submissions.md#gaming-options-object) enthält, die die spielbezogenen Einstellungen für die APP definiert.     |   
| hasExternalInAppProducts           |     boolean          |   Gibt an, ob Ihre APP es Benutzern ermöglicht, den Einkauf außerhalb des Microsoft Store Commerce-Systems zu tätigen. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).     |   
| meetAccessibilityGuidelines           |    boolean           |  Gibt an, ob getestet wurde, ob die App die Richtlinien zur Barrierefreiheit erfüllt. Weitere Informationen finden Sie unter [App-Deklarationen](../publish/product-declarations.md).      |   
| notesForCertification           |  Zeichenfolge  |   Enthält [Hinweise zur Zertifizierung](../publish/notes-for-certification.md) für Ihre App.    |    
| applicationPackages           |   array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen finden Sie unten im Abschnitt [Anwendungspaket](manage-app-submissions.md#application-package-object). Beim Aufruf dieser Methode zur Aktualisierung einer App-Übermittlung sind im Antworttext nur die *fileName*-, *fileStatus*-, *minimumDirectXVersion*- und *minimumSystemRam*-Werte dieser Objekte erforderlich. Die anderen Werte werden von Partner Center aufgefüllt.   |    
| packageDeliveryOptions    | Objekt (object)  | Enthält den schrittweisen Paketrollout sowie erforderliche Einstellungen für die Übermittlung. Weitere Informationen finden Sie unter [options-Objekt für die Paketübermittlung](manage-app-submissions.md#package-delivery-options-object).  |
| enterpriseLicensing           |  Zeichenfolge  |  Einer der [Werte für Unternehmenslizenzierung](manage-app-submissions.md#enterprise-licensing), die das Verhalten der Unternehmenslizenzierung für die App angeben.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Gibt an, ob Microsoft [die App für zukünftige Windows 10-Gerätefamilien verfügbar machen](../publish/set-app-pricing-and-availability.md) darf.    |    
| allowTargetFutureDeviceFamilies           | boolean   |  Gibt an, ob die App [auf zukünftige Windows 10-Gerätefamilien abzielen](../publish/set-app-pricing-and-availability.md) darf.     |   
| Bet           |  array |   Ein Array, das bis zu nach Spann Ende [Ressourcen](manage-app-submissions.md#trailer-object) enthält, die videonachspann für die APP-Auflistung darstellen.   |   


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird die Aktualisierung einer App-Übermittlung veranschaulicht.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur aktualisierten Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [App-Übermittlungsressource](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
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
      "fileStatus": "PendingUpload",
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

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  BESCHREIBUNG   |
|--------|------------------|
| 400  | Die Übermittlung konnte nicht aktualisiert werden, da die Anforderung ungültig ist. |
| 409  | Die Übermittlung konnte aufgrund des aktuellen Status der APP nicht aktualisiert werden, oder die APP verwendet ein Partner Center-Feature, das [von der Microsoft Store Übermittlungs-API derzeit nicht unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported)wird. |   


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Abrufen einer App-Übermittlung](get-an-app-submission.md)
* [Erstellen einer App-Übermittlung](create-an-app-submission.md)
* [Ausführen eines Commit für eine App-Übermittlung](commit-an-app-submission.md)
* [Löschen einer App-Übermittlung](delete-an-app-submission.md)
* [Abrufen des Status einer App-Übermittlung](get-status-for-an-app-submission.md)