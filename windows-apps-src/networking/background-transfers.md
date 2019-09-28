---
description: Verwenden Sie die Hintergrundübertragungs-API zum zuverlässigen Kopieren von Dateien im Netzwerk.
title: Hintergrundübertragungen
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7d06d91d2195c483f5453aeadbc5523a8935003c
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340571"
---
# <a name="background-transfers"></a>Hintergrundübertragungen
Verwenden Sie die Hintergrundübertragungs-API zum zuverlässigen Kopieren von Dateien im Netzwerk. Die Hintergrundübertragungs-API bietet erweiterte Upload- und Downloadfeatures, die bei angehaltener App im Hintergrund ausgeführt werden und auch nach Beendigung der App aktiv bleiben. Die API überwacht den Netzwerkstatus und kann Übertragungen automatisch anhalten und fortsetzen, wenn die Verbindung unterbrochen wird. Übertragungen sind außerdem daten- und akkuabhängig – die Downloadaktivität wird also basierend auf dem aktuellen Verbindungs- und Geräteakkustatus angepasst. Die API ist ideal für das Hoch- und Herunterladen von großen Dateien über HTTP(S) geeignet. FTP wird auch unterstützt, allerdings nur für Downloads.

Hintergrundübertragungen werden getrennt von der aufrufenden App ausgeführt und wurden hauptsächlich für lange Übertragungen von Ressourcen wie Videos, Musik und großen Bildern entwickelt. Für diese Szenarien ist die Verwendung der Hintergrundübertragung unverzichtbar, da Downloads im Hintergrund fortgesetzt werden – selbst dann, wenn die App angehalten wurde.

Wenn Sie kleine Ressourcen herunterladen, deren Download in der Regel schnell abgeschlossen ist, sollten Sie anstelle der Hintergrundübertragung [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-APIs verwenden.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Verwenden von Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>Wie funktioniert das Feature für die Hintergrundübertragung?
Wenn eine App die Hintergrundübertragung verwendet, um eine Übertragung zu initiieren, wird die Anforderung mithilfe des Klassenobjekts [**BackgroundDownloader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundDownloader) oder [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) konfiguriert und initialisiert. Alle Übertragungsvorgänge werden vom System einzeln und getrennt von der aufrufenden App behandelt. Statusinformationen sind verfügbar, falls Sie den Benutzer in der Benutzeroberfläche Ihrer App über den Status informieren möchten. Zudem kann Ihre App während der Übertragung angehalten, fortgesetzt und abgebrochen werden oder sogar die Daten lesen. Die Behandlung von Übertragungen durch das System unterstützt den intelligenten Stromverbrauch und verhindert Probleme, die entstehen können, wenn bei einer verbundenen App Ereignisse wie das Anhalten der App, das Beenden der App oder plötzliche Änderungen des Netzwerkstatus auftreten.

> [!NOTE]
> Aufgrund von Ressourcenbeschränkungen pro App sollte eine App nicht mehr als 200 Übertragungen (DownloadOperations + UploadOperations) zu einem bestimmten Zeitpunkt verfügen. Dieses Limit zu überschreiten kann die App-Übertragungswarteschlange in einen nicht wiederherstellbaren Zustand lassen.

Wenn eine Anwendung gestartet wird, muss sie [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) für alle vorhandenen [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation)- und [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadoperation)-Objekte aufrufen. Dies zu unterlassen führt dazu, dass bereits abgeschlossene Übertragungen verloren gehen, und sorgt dafür, dass Ihre Verwendung der Hintergrundübertragungsfunktion letztendlich nutzlos wird.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>Durchführen authentifizierter Dateianforderungen mit Hintergrundübertragung
Das Feature für die Hintergrundübertragung stellt Methoden bereit, die allgemeine Server- und Proxyanmeldeinformationen, Cookies sowie die Verwendung von benutzerdefinierten HTTP-Headern (mithilfe von [**SetRequestHeader**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader)) für einzelne Übertragungen unterstützen.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>Wie reagiert dieses Feature bei Netzwerkstatusänderungen und unerwartetem Herunterfahren?
Das Feature für die Hintergrundübertragung bewahrt die Einheitlichkeit der einzelnen Übertragungen, wenn sich der Netzwerkstatus ändert. Hierzu werden die vom Feature für die [Konnektivität](https://docs.microsoft.com/previous-versions/windows/apps/hh452990(v=win.10)) bereitgestellten Statusinformationen zur Konnektivität und zum Status des Datentarifs des Netzbetreibers intelligent genutzt. Um das Verhalten für unterschiedliche Netzwerkszenarien zu definieren, legt eine App mithilfe der von [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy) definierten Werte eine Kostenrichtlinie für die einzelnen Übertragungen fest.

Beispielsweise kann die für einen Vorgang definierte Kostenrichtlinie vorsehen, dass der Vorgang automatisch angehalten wird, wenn das Gerät in einem getakteten Netzwerk verwendet wird. Die Übertragung wird automatisch fortgesetzt (oder erneut gestartet), wenn eine Verbindung mit einem „uneingeschränkten“ Netzwerk hergestellt wird. Weitere Informationen zur Definition von Netzwerken durch Kosten finden Sie unter [**NetworkCostType**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkCostType).

Obwohl das Feature für die Hintergrundübertragung über eigene Mechanismen zur Behandlung von Netzwerkstatusänderungen verfügt, müssen bei mit einem Netzwerk verbundenen Apps einige allgemeine Punkte im Zusammenhang mit der Konnektivität beachtet werden. Weitere Informationen finden Sie unter [Zugreifen auf den Netzwerkverbindungsstatus und Verwalten von Netzwerkkosten (HTML)](https://docs.microsoft.com/previous-versions/windows/apps/hh452983(v=win.10)).

> **Hinweis**  Mithilfe von Features für auf Mobilgeräten ausgeführte Apps kann der Benutzer die übertragene Datenmenge basierend auf dem Verbindungstyp, Roamingstatus und Datentarif überwachen und einschränken. Aus diesem Grund können Hintergrundübertragungen auf dem Telefon auch dann angehalten werden, wenn die Übertragung gemäß dem [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy)-Wert fortgesetzt werden sollte.

Die folgende Tabelle zeigt für jeden [**BackgroundTransferCostPolicy**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCostPolicy)-Wert, wann Hintergrundübertragungen basierend auf dem aktuellen Status des Telefons zulässig sind. Sie können den aktuellen Status des Telefons auch mithilfe der [**ConnectionCost**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionCost)-Klasse ermitteln.

| Gerätestatus                                                                                                                      | UnrestrictedOnly | Standard | Immer |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Mit WLAN verbunden                                                                                                                 | Zulassen            | Zulassen   | Zulassen  |
| Getaktete Verbindung, kein Roaming, unter dem Datenlimit, Überschreitung des Limits nicht zu erwarten                                                   | Deny             | Zulassen   | Zulassen  |
| Getaktete Verbindung, kein Roaming, unter dem Datenlimit, Überschreitung des Limits zu erwarten                                                       | Deny             | Deny    | Zulassen  |
| Getaktete Verbindung, Roaming, unter dem Datenlimit                                                                                     | Deny             | Deny    | Zulassen  |
| Getaktete Verbindung, Roaming, über dem Datenlimit. Dieser Status tritt nur auf, wenn der Benutzer in der Data Sense-Benutzeroberfläche die Option „Datennutzung im Hintergrund einschränken“ aktiviert. | Deny             | Deny    | Deny   |

## <a name="uploading-files"></a>Hochladen von Dateien
Bei der Hintergrundübertragung erfolgt der Upload als [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation). Dabei wird eine Reihe von Steuerungsmethoden zum Neustarten oder Abbrechen des Vorgangs verfügbar gemacht. App-Ereignisse (z. B. Anhalten oder Beenden) und Konnektivitätsänderungen werden vom System automatisch durch **UploadOperation** behandelt. Uploads werden bei Anhalten oder Unterbrechen einer App und auch nach dem Beenden der App fortgesetzt. Außerdem wird durch Festlegen der [**CostPolicy**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy)-Eigenschaft angegeben, ob die App Uploads startet, wenn für die Internetkonnektivität ein getaktetes Netzwerk verwendet wird.

In den folgenden Beispielen werden die Erstellung und Initialisierung eines einfachen Uploads sowie das Aufzählen und Fortsetzen von in einer vorherigen App-Sitzung gespeicherten Vorgängen erläutert.

### <a name="uploading-a-single-file"></a>Hochladen einer Datei
Die Erstellung eines Uploads beginnt mit [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader). Mit dieser Klasse werden die Methoden bereitgestellt, womit die App den Upload konfigurieren kann, bevor die resultierende [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation)-Instanz erstellt wird. Im folgenden Beispiel wird gezeigt, wie dies mit den erforderlichen [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) und [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekten gemacht wird.

**Identifizieren der Datei und des Ziels für den Upload**

Vor dem Erstellen einer [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) müssen wir den URI des Speicherorts für den Upload und die hochzuladende Datei bestimmen. Im folgenden Beispiel wird der *uriString*-Wert mit einer Zeichenfolge aus der Benutzeroberflächeneingabe und der *file*-Wert mit dem [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Objekt gefüllt, das von einem [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync)-Vorgang zurückgegeben wird.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**Erstellen und Initialisieren des Uploadvorgangs**

Im vorherigen Schritt wurden die Werte *uriString* und *file* an eine Instanz von „UploadOp“ aus dem nächsten Beispiel übergeben. Dort werden sie zum Konfigurieren und Starten des neuen Uploadvorgangs verwendet. Zunächst wird *uriString* analysiert, um das erforderliche [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Objekt zu erstellen.

Dann verwendet [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) die Eigenschaften der bereitgestellten [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)-Klasse (*file*) zum Füllen des Anforderungsheaders und Festlegen der *SourceFile*-Eigenschaft mit dem **StorageFile**-Objekt. Anschließend wird die [**SetRequestHeader**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.setrequestheader)-Methode aufgerufen, um den als Zeichenfolge bereitgestellten Dateinamen und die [**StorageFile.Name**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.name)-Eigenschaft einzufügen.

Schließlich erstellt [**BackgroundUploader**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundUploader) die [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation)-Klasse (*upload*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Beachten Sie die asynchronen Methodenaufrufe, die mit JavaScript-Zusagen definiert sind. Sehen Sie sich die folgende Zeile aus dem letzten Beispiel an:

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

Nach dem Async-Methodenaufruf folgt eine then-Anweisung, die von der App definierte Methoden angibt, die aufgerufen werden, wenn ein Ergebnis aus dem Async-Methodenaufruf zurückgegeben wird. Weitere Informationen zu diesem Programmierungsmuster finden Sie unter [Asynchrone Programmierung in JavaScript mit Zusagen](https://docs.microsoft.com/previous-versions/windows).

### <a name="uploading-multiple-files"></a>Hochladen mehrerer Dateien
**Identifizieren der Dateien und des Ziels für den Upload**

In einem Szenario, in dem mehrere Dateien mit einer einzelnen [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) übertragen werden, beginnt der Vorgang wie üblich mit der Angabe des erforderlichen Ziel-URI und der lokalen Dateiinformationen. Ähnlich wie in dem Beispiel im vorherigen Abschnitt wird der URI als Zeichenfolge vom Endbenutzer angegeben, und mit [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) kann auch das Auswählen von Dateien über die Benutzeroberfläche ermöglicht werden. In diesem Szenario muss die App allerdings stattdessen die [**PickMultipleFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync)-Methode aufrufen, um die Auswahl mehrerer Dateien über die Benutzeroberfläche zu ermöglichen.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**Erstellen von Objekten für die verfügbaren Parameter**

In den nächsten beiden Beispielen wird Code in einer Beispielmethode namens **startMultipart** verwendet, die am Ende des letzten Schritts aufgerufen wurde. Für diese Anleitung wurde der Code in der Methode, der ein Array mit [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart)-Objekten erstellt, von dem Code getrennt, der die resultierende [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) erstellt.

Zuerst wird die vom Benutzer angegebene URI-Zeichenfolge als [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) initialisiert. Danach wird das Array mit [**IStorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageFile)-Objekten (**files**) durchlaufen, die an diese Methode übergeben wurden, und auf der Grundlage der einzelnen Objekte wird jeweils ein neues [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart)-Objekt erstellt und anschließend im **contentParts**-Array platziert.

```javascript
    upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**Erstellen und Initialisieren des mehrteiligen Uploadvorgangs**

Nachdem das contentParts-Array mit allen [**BackgroundTransferContentPart**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart)-Objekten aufgefüllt wurde, die jeweils eine [**IStorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.IStorageFile) zum Hochladen darstellen, kann [**CreateUploadAsync**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.createuploadasync) mit dem [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) aufgerufen werden, um das Ziel für die Anforderung anzugeben.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### <a name="restarting-interrupted-upload-operations"></a>Neustarten von unterbrochenen Uploadvorgängen
Bei Abschluss oder Abbruch von [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) werden alle zugeordneten Systemressourcen freigegeben. Wenn die App allerdings vor Auftreten eines dieser Ereignisse beendet wird, werden aktive Vorgänge angehalten, und die ihnen zugeordneten Ressourcen bleiben belegt. Wenn diese Vorgänge nicht in der nächsten App-Sitzung aufgezählt und fortgesetzt werden, werden sie nicht abgeschlossen und belegen weiterhin Geräteressourcen.

1.  Bevor Sie die Funktion zum Aufzählen beibehaltener Vorgänge definieren, müssen wir ein Array erstellen, das die zurückzugebenden [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation)-Objekte enthält:

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  Im nächsten Schritt definieren Sie die Funktion, die alle beibehaltenen Vorgänge aufzählt und im Array speichert. Die **load**-Methode, die zum Neuzuweisen der Rückrufe von [**UploadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.UploadOperation) aufgerufen wird, wenn der Download nach Beendigung der App fortgesetzt wird, befindet sich in der UploadOp-Klasse, die Sie weiter unten in diesem Abschnitt definieren.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>Herunterladen von Dateien
Bei der Hintergrundübertragung erfolgt jeder Download als [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation). Dabei werden eine Reihe von Steuerungsmethoden zum Anhalten, Fortsetzen, Neustarten und Abbrechen des Vorgangs verfügbar gemacht. App-Ereignisse (z. B. Anhalten oder Beenden) und Konnektivitätsänderungen werden vom System automatisch durch **DownloadOperation** behandelt. Downloads werden bei Anhalten oder Unterbrechen einer App und auch nach dem Beenden der App fortgesetzt. In Szenarien für mobile Netzwerke wird außerdem durch Festlegen der [**CostPolicy**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.costpolicy)-Eigenschaft angegeben, ob die App Downloads startet oder fortsetzt, wenn für die Internetkonnektivität ein getaktetes Netzwerk verwendet wird.

Wenn Sie kleine Ressourcen herunterladen, deren Download in der Regel schnell abgeschlossen ist, sollten Sie anstelle der Hintergrundübertragung [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-APIs verwenden.

In den folgenden Beispielen werden die Erstellung und Initialisierung eines einfachen Downloads sowie das Aufzählen und Fortsetzen von in einer vorherigen App-Sitzung gespeicherten Vorgängen erläutert.

### <a name="configure-and-start-a-background-transfer-file-download"></a>Konfigurieren und Starten eines Dateidownloads mit Hintergrundübertragung
Das folgende Beispiel veranschaulicht, wie mit Zeichenfolgen, die einen URI und einen Dateinamen darstellen, ein [**Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Objekt und die [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) erstellt werden können, die die angeforderte Datei enthalten. Im Beispiel wird die neue Datei automatisch an einem vordefinierten Speicherort abgelegt. Alternativ kann Benutzern mithilfe von [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) ermöglicht werden, den Speicherort der Datei auf dem Gerät anzugeben. Die **load**-Methode, die zum Neuzuweisen der Rückrufe von [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) aufgerufen wird, wenn der Download nach Beendigung der App fortgesetzt wird, befindet sich in der „DownloadOp“-Klasse, die Sie weiter unten in diesem Abschnitt definieren.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Beachten Sie die asynchronen Methodenaufrufe, die mit JavaScript-Zusagen definiert sind. Aus Zeile 17 des vorherigen Codebeispiels geht Folgendes hervor:

```javascript
promise = download.startAsync().then(complete, error, progress);
```

Nach dem Async-Methodenaufruf folgt eine then-Anweisung, die von der App definierte Methoden angibt, die aufgerufen werden, wenn ein Ergebnis aus dem Async-Methodenaufruf zurückgegeben wird. Weitere Informationen zu diesem Programmierungsmuster finden Sie unter [Asynchrone Programmierung in JavaScript mit Zusagen](https://docs.microsoft.com/previous-versions/windows).

### <a name="adding-additional-operation-control-methods"></a>Hinzufügen weiterer Methoden zur Vorgangssteuerung
Der Grad der Steuerung lässt sich durch Implementieren zusätzlicher [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation)-Methoden erhöhen. Wenn Sie beispielsweise dem obigen Beispiel den folgenden Code hinzufügen, wird das Abbrechen des Downloads ermöglicht.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>Aufzählen beibehaltener Vorgänge beim Start
Bei Abschluss oder Abbruch von [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) werden alle zugeordneten Systemressourcen freigegeben. Wenn die App allerdings vor Auftreten eines dieser Ereignisse beendet wird, werden die Downloads angehalten und im Hintergrund beibehalten. In den folgenden Beispielen wird gezeigt, wie Sie beibehaltene Downloads in einer neuen App-Sitzung fortsetzen.

1.  Bevor Sie die Funktion zum Aufzählen beibehaltener Vorgänge definieren, müssen wir ein Array erstellen, das die zurückzugebenden [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation)-Objekte enthält:

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  Im nächsten Schritt definieren Sie die Funktion, die alle beibehaltenen Vorgänge aufzählt und im Array speichert. Die **load**-Methode, die zum Neuzuweisen von Rückrufen für eine beibehaltene [**DownloadOperation**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.DownloadOperation) aufgerufen wird, befindet sich im Beispiel für die DownloadOp-Klasse, die weiter unten in diesem Abschnitt definiert wird.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  Die mit Daten aufgefüllte Liste kann nun zum Neustarten ausstehender Vorgänge verwendet werden.

## <a name="post-processing"></a>Nachverarbeitung
Ein neues Feature in Windows 10 ermöglicht es nach Abschluss einer Hintergrundübertragung, den Anwendungscode auszuführen, selbst wenn die App nicht ausgeführt wird. Ihre App möchte z. B. eine Liste der verfügbaren Filme aktualisieren, nachdem ein Film heruntergeladen wurde, statt bei jedem Start nach neuen Filmen zu suchen. Ihre App möchte ggf. eine fehlerhafte Dateiübertragung behandeln, indem diese unter Verwendung eines anderes Servers oder Ports wiederholt wird. Nachbearbeitung wird für erfolgreiche und nicht erfolgreiche Übertragungen aufgerufen, damit Sie sie zum Implementieren benutzerdefinierter Fehlerbehandlung und Wiederholungslogik verwenden können.

Nachbearbeitung verwendet die vorhandene Hintergrundaufgaben-Infrastruktur. Erstellen Sie eine Hintergrundaufgabe, und ordnen Sie sie Ihren Übertragungen zu, bevor Sie die Übertragung starten. Die Übertragungen werden dann im Hintergrund ausgeführt, und wenn sie abgeschlossen sind, wird die Hintergrundaufgabe aufgerufen, um die Nachbearbeitung auszuführen.

Für die Nachbearbeitung wird als neue Klasse [**BackgroundTransferCompletionGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup) verwendet. Diese Klasse ist mit der vorhandenen [**BackgroundTransferGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup)-Klasse vergleichbar, mit der Sie Hintergrundübertragungen gruppieren können. Die **BackgroundTransferCompletionGroup**-Klasse ermöglicht es jedoch zudem, eine Hintergrundaufgabe anzugeben, die nach Abschluss der Übertragung ausgeführt werden soll.

Initiieren Sie wie folgt eine Hintergrundübertragung mit Nachbearbeitung.

1.  Erstellen Sie ein [**BackgroundTransferCompletionGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferCompletionGroup)-Objekt. Erstellen Sie dann ein [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Objekt. Legen Sie die **Trigger**-Eigenschaft des Generator-Objekts auf das Abschlussgruppenobjekt und die **TaskEntryPoint**-Eigenschaft des Generators auf den Einstiegspunkt der Hintergrundaufgabe fest, die nach Abschluss der Übertragung ausgeführt werden soll. Rufen Sie schließlich die [**BackgroundTaskBuilder.Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register)-Methode auf, um die Hintergrundaufgabe zu registrieren. Beachten Sie, dass viele Abschlussgruppen einen Einstiegspunkt für die Hintergrundaufgabe gemeinsam verwenden können, Sie jedoch nur über eine Abschlussgruppe pro Registrierung einer Hintergrundaufgabe verfügen dürfen.

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  Ordnen Sie als Nächstes die Hintergrundübertragungen mit der Abschlussgruppe zu. Aktivieren Sie nach Erstellen aller Übertragungen die Abschlussgruppe.

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  Der Code in der Hintergrundaufgabe extrahiert die Liste der Vorgänge anhand der Triggerdetails und kann dann die Details für jeden Vorgang überprüfen und die entsprechende Nachbearbeitung für jeden Vorgang ausführen.

```csharp
public class BackgroundDownloadProcessingTask : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {
    var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
    IReadOnlyList<DownloadOperation> downloads = details.Downloads;

    // Do post-processing on each finished operation in the list of downloads
    }
}
```

Die Nachbearbeitungsaufgabe stellt eine reguläre Hintergrundaufgabe dar. Sie ist Teil des Pools aller Hintergrundaufgaben und unterliegt auch derselben Ressourcenverwaltungsrichtlinie wie alle anderen Hintergrundaufgaben.

Beachten Sie, dass die Nachbearbeitung nicht die Vordergrund-Abschlusshandler ersetzt. Wenn Ihre App einen Vordergrund-Abschlusshandler definiert und nach Abschluss der Dateiübertragung App ausgeführt wird, werden der Vordergrund- und Hintergrund-Abschlusshandler aufgerufen. Die Reihenfolge, in der die Vordergrund- und Hintergrundaufgaben aufgerufen werden, kann nicht garantiert werden. Wenn Sie beide Handler definieren, sollten Sie sicherstellen, dass die zwei Aufgaben ordnungsgemäß ausgeführt werden und keine Konflikte aufweisen, wenn sie gleichzeitig ausgeführt werden.

## <a name="request-timeouts"></a>Zeitüberschreitungen bei Anforderungen
Es müssen zwei primäre Szenarien zu Verbindungszeitüberschreitungen berücksichtigt werden:

-   Beim Herstellen einer neuen Verbindung für die Übertragung wird die Verbindungsanforderung abgebrochen, wenn die Verbindung nicht innerhalb von fünf Minuten hergestellt wird.

-   Nach dem Herstellen einer Verbindung wird eine HTTP-Anforderungsnachricht abgebrochen, auf die nicht innerhalb von zwei Minuten reagiert wurde.

> **Hinweis**  Vorausgesetzt, dass eine Internetverbindung besteht, wiederholt die Hintergrundübertragung in beiden Szenarien eine Anforderung automatisch bis zu drei Mal. Wenn keine Internetkonnektivität erkannt wird, warten zusätzliche Anforderungen, bis Konnektivität vorhanden ist.

## <a name="debugging-guidance"></a>Debugging-Leitfaden
Das Beenden einer Debugsitzung in Microsoft Visual Studio ist mit dem Schließen der App vergleichbar; PUT-Uploads werden angehalten und POST-Uploads werden beendet. Auch beim Debuggen sollte die App alle beibehaltenen Uploads auflisten und dann neu starten oder abbrechen. Sie können beispielsweise aufgelistete beibehaltene Uploadvorgänge beim App-Start durch die App abbrechen lassen, wenn kein Interesse an vorherigen Operationen für diese Debugsitzung besteht.

Während Downloads/Uploads beim App-Start in einer Debugsitzung aufgelistet werden, können Sie diese durch Ihre App abbrechen lassen, wenn kein Interesse an vorherigen Operationen für diese Debugsitzung besteht. Beachten Sie, dass [**GetCurrentUploadsAsync**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.getcurrentuploadsasync) Vorgänge nicht auflisten kann, die mithilfe der vorherigen App-Bereitstellung erstellt werden, wenn Visual Studio-Projektupdates, beispielsweise Änderungen am App-Manifest, vorhanden sind und die App deinstalliert und erneut bereitgestellt wird.

Bei der Verwendung von Hintergrundübertragungen während der Entwicklung kann es vorkommen, dass die internen Caches aktiver und abgeschlossener Übertragungsvorgänge nicht mehr synchron sind. Dies kann zur Folge haben, dass keine neuen Übertragungsvorgänge gestartet werden können, oder dass keine Interaktion mit vorhandenen Vorgängen und [**BackgroundTransferGroup**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer.BackgroundTransferGroup)-Objekten mehr möglich ist. In einigen Fällen wird durch den Versuch einer Interaktion mit vorhandenen Vorgängen ein Absturz ausgelöst. Dies kann vorkommen, wenn die [**TransferBehavior**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgroundtransfergroup.transferbehavior)-Eigenschaft auf **Parallel** festgelegt ist. Dieses Problem tritt nur bei bestimmten Szenarien während der Entwicklung auf und betrifft nicht die Endbenutzer Ihrer App.

Vier Szenarien mit Visual Studio können dieses Problem auslösen.

-   Sie erstellen ein neues Projekt mit demselben App-Namen wie ein vorhandenes Projekt, jedoch einer anderen Sprache (z. B. C# statt C++).
-   Sie ändern die Zielarchitektur (z. B. von x86 zu x64) in einem vorhandenen Projekt.
-   Sie ändern die Kultur (z. B. von neutral zu en-US) in einem vorhandenen Projekt.
-   Sie fügen in einem vorhandenen Projekt eine Funktion im Paketmanifest (z. B. **Unternehmensauthentifizierung**) hinzu oder entfernen eine.

Normale App-Wartungen wie z. B. Manifestaktualisierungen, bei denen Funktionen hinzugefügt oder entfernt werden, lösen dieses Problem in den Bereitstellungen Ihrer App für Endbenutzer nicht aus.
Umgehen Sie dieses Problem, indem Sie alle Versionen der App vollständig deinstallieren und sie mit der neuen Sprache, Architektur, Kultur oder Funktion erneut bereitstellen. Dies ist auf der **Startseite** oder mithilfe von PowerShell und dem **Remove-AppxPackage**-Cmdlet möglich.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Ausnahmen in Windows.Networking.BackgroundTransfer
Eine Ausnahme wird ausgelöst, wenn eine ungültige Zeichenfolge für einen Uniform Resource Identifier (URI) an den Konstruktor für das [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Objekt übergeben wird.

**.NET:** Der [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Typ wird in C# und VB als [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri) angezeigt.

In C# und Visual Basic kann dieser Fehler vermieden werden, indem die [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri)-Klasse in .NET 4.5 und eine der [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate#overloads)-Methoden zum Testen der vom App-Benutzer erhaltenen Zeichenfolge verwendet wird, bevor der URI erstellt wird.

In C++ gibt es keine Methode zum Analysieren einer Zeichenfolge für einen URI. Wenn eine App vom Benutzer eine Eingabe für das [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Element erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

Der [**Windows.Networking.backgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer)-Namespace enthält praktische Hilfsmethoden und verwendet im [**Windows.Networking.Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)-Namespace Enumerationen zum Behandeln von Fehlern. Mit ihnen lassen sich spezifische Netzwerkausnahmen in der App unterschiedlich behandeln.

Ein Fehler in einer asynchronen Methode im [**Windows.Networking.backgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer)-Namespace wird als ein **HRESULT**-Wert zurückgegeben. Mit der [**BackgroundTransferError.GetStatus**](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.backgroundtransfererror.getstatus)-Methode wird ein Netzwerkfehler aus einem Hintergrundübertragungsvorgang in einen [**WebErrorStatus**](https://docs.microsoft.com/uwp/api/Windows.Web.WebErrorStatus)-Enumerationswert konvertiert. Die meisten **WebErrorStatus**-Enumerationswerte entsprechen einem vom systemeigenen HTTP- oder FTP-Clientvorgang zurückgegebenen Fehler. Eine App kann nach bestimmten **WebErrorStatus**-Enumerationswerten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Bei Parameterprüfungsfehlern kann eine App den **HRESULT**-Wert aus der Ausnahme auch verwenden, um ausführlichere Informationen zum zugehörigen Fehler zu erhalten. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E\_INVALIDARG** zurückgegeben.

## <a name="important-apis"></a>Wichtige APIs
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
