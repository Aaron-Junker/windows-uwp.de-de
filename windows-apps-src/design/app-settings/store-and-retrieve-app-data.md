---
Description: Hier erfahren Sie, wie Sie lokale, Roaming- und temporäre App-Daten speichern und abrufen können.
title: Speichern und Abrufen von Einstellungen und anderen App-Daten
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690365"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Speichern und Abrufen von Einstellungen und anderen App-Daten

*App-Daten* sind änderbare Daten, die von einer bestimmten App erstellt und verwaltet werden. Sie beinhalten den Laufzeitstatus, App-Einstellungen, Benutzereinstellungen, Referenzinhalte (beispielsweise die Wörterbuchdefinitionen in einer Wörterbuch-App) und andere Einstellungen. App-Daten unterscheiden sich von *Benutzerdaten*, Daten, die der Benutzer beim Verwenden einer App erstellt und verwaltet. Benutzerdaten umfassen Dokument- oder Mediendateien, E-Mail- oder Kommunikationstranskripte oder Datenbankeinträge, mit vom Benutzer erstellte Inhalte enthalten. Benutzerdaten können für mehrere Apps nützlich oder von Bedeutung sein. Häufig handelt es sich dabei um Daten, die der Benutzer als von der App unabhängige Entität ändern oder übertragen möchte (z. B. ein Dokument).

**Wichtiger Hinweis zu App-Daten:** Die Lebensdauer der App-Daten ist an die Lebensdauer der App gebunden. Wenn die App entfernt wird, gehen auch alle App-Daten verloren. Verwenden Sie App-Daten nicht zum Speichern von Benutzerdaten oder anderen Daten, die Benutzer als wertvoll und unersetzlich betrachten. Solche Informationen sollten in den Bibliotheken des Benutzers und auf Microsoft OneDrive gespeichert werden. App-Daten eignen sich perfekt zum Speichern von App-spezifischen (Benutzer-)Einstellungen und Favoriten.

## <a name="types-of-app-data"></a>App-Datentypen

Es gibt zwei Arten von App-Daten: Einstellungen und Dateien.

### <a name="settings"></a>Einstellungen

Verwenden Sie die Einstellungen zum Speichern von Benutzereinstellungen und Anwendungsstatus-Informationen. Mit der App-Daten-API können Sie ganz einfach Einstellungen erstellen und abrufen (später in diesem Artikel zeigen wir Ihnen einige Beispiele dazu).

Im Folgenden finden Sie Datentypen, die Sie für App-Einstellungen verwenden können:

- **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
- **Boolean**
- **Char16**, **String**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime), [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - Verwenden Sie für C#/.NET Folgendes: [**System.DateTimeOffset**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0), [**System.TimeSpan**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**, [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Size**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue:** ](/uwp/api/Windows.Storage.ApplicationDataCompositeValue) Eine Reihe von verwandten App-Einstellungen, die atomisch serialisiert und deserialisiert werden müssen. Verwenden Sie Verbundeinstellungen, um atomische Aktualisierungen voneinander abhängiger Einstellungen problemlos behandeln zu können. Das System stellt die Integrität von Verbundeinstellungen bei gleichzeitigem Zugriff und Roaming sicher. Verbundeinstellungen sind für kleine Datenmengen vorgesehen. Bei Verwendung für große Datasets kann die Leistung beeinträchtigt werden.

### <a name="files"></a>Dateien

Verwenden Sie Dateien zum Speichern von Binärdaten oder zum Aktivieren Ihrer eigenen, benutzerdefinierten, serialisierten Typen.

## <a name="storing-app-data-in-the-app-data-stores"></a>Speichern von App-Daten in den App-Datenspeichern


Bei der Installation einer App weist das System der App eigene, benutzerspezifische Datenspeicher für Einstellungen und Dateien zu. Sie müssen nicht wissen, wo und wie diese Daten vorhanden ist, da das System für die Verwaltung des physischen Speichers zuständig ist. Damit wird sichergestellt, dass die Daten von anderen Apps und anderen Benutzern isoliert gehalten werden. Das System behält außerdem den Inhalt dieser Datenspeicher bei, wenn der Benutzer ein App-Update installiert, und entfernt den Inhalt dieser Datenspeicher vollständig und sauber, wenn die App deinstalliert wird.

Jede App verfügt in ihrem jeweiligen App-Datenspeicher über systemdefinierte Stammverzeichnisse: eines für lokale Dateien, eines für Roamingdateien und eines für temporäre Dateien. Ihre App kann den Stammverzeichnissen neue Dateien und Container hinzufügen.

## <a name="local-app-data"></a>Lokale App-Daten


Lokale App-Daten sollten für alle Informationen verwendet werden, die zwischen App-Sitzungen beibehalten werden müssen und die nicht als App-Roamingdaten geeignet sind. Daten, die auf anderen Geräten nicht verwendet werden können, sollten ebenfalls dort gespeichert werden. Es gibt keine allgemeinen Größenbeschränkungen für lokal gespeicherte Daten. Verwenden Sie den lokalen App-Datenspeicher für Daten, für die ein Roaming nicht sinnvoll ist, sowie für große Datasets.

### <a name="retrieve-the-local-app-data-store"></a>Abrufen des lokalen App-Datenspeichers

Bevor Sie lokale App-Daten lesen oder schreiben können, müssen Sie den lokalen App-Datenspeicher abrufen. Verwenden Sie zum Abrufen des lokalen App-Datenspeichers die [**ApplicationData.LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)-Eigenschaft, um die lokalen Einstellungen der App als [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)-Objekt abzurufen. Verwenden Sie die [**ApplicationData.LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)-Eigenschaft, um die Dateien aus einem [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)-Objekt abzurufen. Verwenden Sie die [**ApplicationData.LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder)-Eigenschaft, um den Ordner im lokalen App-Datenspeicher abzurufen, in dem Sie Dateien speichern können, die nicht in der Sicherung und Wiederherstellung enthalten sind.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Erstellen und Abrufen einer einfachen lokalen Einstellung

Um eine Einstellung zu erstellen oder zu schreiben, verwenden Sie die [**ApplicationDataContainer.Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)-Eigenschaft, um auf die Einstellungen im `localSettings`-Container zuzugreifen, den wir im vorherigen Schritt abgerufen haben. In diesem Beispiel wird eine Einstellung namens `exampleSetting` erstellt.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Um die Einstellung abzurufen, verwenden Sie dieselbe [**ApplicationDataContainer.Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)-Eigenschaft, mit der Sie die Einstellung erstellt haben. Dieses Beispiel zeigt, wie Sie die gerade erstellte Einstellung abrufen können.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Erstellen und Abrufen eines lokalen zusammengesetzten Werts

Erstellen Sie zum Erstellen oder Schreiben eines zusammengesetzten Werts ein [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)-Objekt. In diesem Beispiel wird eine Verbundeinstellung namens `exampleCompositeSetting` erstellt und dem Container `localSettings` hinzugefügt.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

In diesem Beispiel wird veranschaulicht, wie der gerade erstellte, zusammengesetzte Wert abgerufen werden kann.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>Erstellen und Lesen einer lokalen Datei

Verwenden Sie die Datei-APIs (z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows.Storage.FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)), um eine Datei im lokalen App-Datenspeicher zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `localFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Verwenden Sie die Datei-APIs (z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) und [**Windows.Storage.FileIO.ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync)), um eine Datei im lokalen App-Datenspeicher zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Schritt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>Roamingdaten


Wenn Sie Roamingdaten in Ihrer App verwenden, können Benutzer die App-Daten der App problemlos für mehrere Geräte synchronisieren. Installiert ein Benutzer eine App auf mehreren Geräten, sorgt das Betriebssystem dafür, dass die App-Daten synchronisiert sind, und verringert so den Einrichtungsaufwand für die App auf weiteren Geräten. Mittels Roaming können Benutzer außerdem eine Aufgabe (beispielsweise das Erstellen einer Liste) an genau dem Punkt fortsetzen, an dem sie die Aufgabe auf einem anderen Gerät unterbrochen haben. Das Betriebssystem repliziert aktualisierte Roamingdaten in der Cloud und synchronisiert die Daten mit den anderen Geräten, auf denen die App installiert ist.

Das Betriebssystem begrenzt für jede App die Menge der App-Daten für das Roaming. Siehe [**ApplicationData.RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota). Erreicht die App diese Grenze, werden keine App-Daten der App mehr in die Cloud repliziert, bis die Gesamtmenge der App-Roamingdaten wieder unter dem Grenzwert liegt. Aus diesem Grund ist es empfehlenswert, Roamingdaten nur für Benutzereinstellungen, Links und kleine Datendateien zu verwenden.

Roamingdaten für Apps sind in der Cloud verfügbar, solange der Benutzer innerhalb des erforderlichen Zeitintervalls mit einem Gerät darauf zugreift. Führt ein Benutzer eine App für eine dieses Zeitintervall nicht überschreitende Spanne aus, werden die Roamingdaten aus der Cloud gelöscht. Bei der Deinstallation einer App werden die entsprechenden Roamingdaten in der Cloud nicht automatisch gelöscht, sondern beibehalten. Installiert der Benutzer die App innerhalb des Zeitintervalls wieder, werden die Roamingdaten über die Cloud synchronisiert.

### <a name="roaming-data-dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen für Roamingdaten

- Verwenden Sie Roaming für Benutzereinstellungen und -anpassungen, Links und kleine Datendateien. Sie können mithilfe von Roaming beispielsweise dafür sorgen, dass die vom Benutzer vorgenommene Einstellung für die Hintergrundfarbe auf allen Geräten angewendet wird.
- Verwenden Sie Roaming, damit Benutzer eine Aufgabe auf mehreren Geräten fortsetzen können. Sie können das Roaming für Anwendungsdaten beispielsweise implementieren, um den Inhalt eines E-Mail-Entwurfs oder die zuletzt angezeigte Seite in einer Lese-App zu speichern.
- Behandeln Sie das [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)-Ereignis, indem Sie App-Daten aktualisieren. Dieses Ereignis tritt unmittelbar nach der Synchronisierung der App-Daten mit der Cloud auf.
- Führen Sie Roaming für Verweise auf Inhalte durch, anstelle für unformatierte Daten. Das Roaming für eine URL eines Onlineartikel ist beispielsweise sinnvoller als das Roaming für den Inhalt des Artikels.
- Verwenden Sie für wichtige, zeitkritische Informationen die Einstellung *HighPriority* zusammen mit [**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings).
- Führen Sie kein Roaming für gerätespezifische App-Daten durch. Manche dieser Informationen sind ausschließlich lokal relevant, wie beispielsweise der Pfadname zu einer lokalen Dateiressource. Wenn Sie lokale Informationen als Roamingdaten verwenden möchten, stellen Sie sicher, dass die App wiederhergestellt werden kann, wenn die Informationen auf dem zweiten Gerät nicht gültig sind.
- Führen Sie kein Roaming für große App-Datenmengen durch. Die Menge der App-Daten für das Roaming ist begrenzt. Verwenden Sie die Eigenschaft [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota), um diese maximale Datenmenge zu implementieren. Wenn eine App diese Grenze erreicht, wird so lange kein Datenroaming durchgeführt, bis die Größe des App-Datenspeichers wieder unter die Grenze fällt. Berücksichtigen Sie beim App-Design, wie eine Grenze für größere Datenmengen festgelegt wird, sodass die Begrenzung nicht überschritten wird. Wenn für das Speichern eines Spielstands beispielsweise jeweils 10 KB erforderlich sind, sollte die App unter Umständen höchstens zulassen, dass 10 Spielstände gespeichert werden.
- Verwenden Sie Roaming nicht für Daten, die von einer sofortigen Synchronisierung abhängig sind. Windows garantiert keine sofortige Synchronisierung; die Synchronisierung kann erheblich verzögert werden, wenn Benutzer offline oder mit einem Netzwerk mit hoher Latenz verbunden sind. Stellen Sie sicher, dass die UI nicht von einer sofortigen Synchronisierung abhängig ist.
- Verwenden Sie Roaming nicht für häufig geänderte Daten. Wenn Ihre App beispielsweise Informationen nachverfolgt, die sich häufig ändern (z. B. die Position eines Musiktitels in Sekunden), speichern Sie diese Daten nicht als Roaming-App-Daten. Wählen Sie stattdessen eine weniger häufige angepasste Darstellung, die trotzdem eine gute Benutzererfahrung gewährleistet, z. B. den aktuell wiedergegebenen Titel.

### <a name="roaming-pre-requisites"></a>Voraussetzungen für Roaming

Jeder Benutzer kann von Roaming-App-Daten profitieren, wenn ein Microsoft-Konto zur Anmeldung am Gerät verwendet wird. Benutzer und Gruppenrichtlinienadministratoren haben jedoch die Möglichkeit, das Roaming von App-Daten auf einem Gerät zu deaktivieren. Benutzer, die kein Microsoft-Konto verwenden oder die Datenroamingfunktion deaktivieren, können Ihre App weiterhin verwenden, die App-Daten sind dann jedoch auf jedem Gerät lokal.

Daten, die im [**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault) gespeichert sind, werden nur übertragen, wenn ein Benutzer ein Gerät als "vertrauenswürdig" eingestuft hat. Wird einem Gerät nicht vertraut, werden die in diesem Tresor gespeicherten Daten nicht für das Roaming verwendet.

### <a name="conflict-resolution"></a>Konfliktlösung

Das Roaming von App-Daten ist nicht für eine gleichzeitige Verwendung auf mehreren Geräten vorgesehen Wenn es während der Synchronisierung zu einem Konflikt kommt, weil eine bestimmte Dateneinheit auf beiden Geräten geändert wurde, verwendet das System vorzugsweise immer den zuletzt geschriebenen Wert. Dadurch wird sichergestellt, dass der App immer die aktuellsten Informationen zur Verfügung stehen. Handelt es sich bei der Dateneinheit um eine zusammengesetzte Einstellung, findet die Konfliktlösung trotzdem auf der Ebene der Einstellungseinheit statt, d. h. der zuletzt geänderte Wert der Zusammensetzung wird synchronisiert.

### <a name="when-to-write-data"></a>Zeitpunkt für das Schreiben von Daten

Je nach erwarteter Lebensdauer der Einstellung sollten Daten zu unterschiedlichen Zeitpunkten geschrieben werden. Selten bzw. langsam geänderte App-Daten sollten sofort geschrieben werden. Häufig geänderte App-Daten sollten dagegen nur in bestimmten Zeiträumen regelmäßig (beispielsweise einmal alle fünf Minuten) und bei angehaltener App geschrieben werden. So kann eine Musik-App beispielsweise die Einstellung für den aktuellen Titel schreiben, wenn ein neuer Titel wiedergegeben wird, aber die aktuelle Position im Titel sollte nur in angehaltenem Zustand der Anwendung geschrieben werden.

### <a name="excessive-usage-protection"></a>Schutz vor übermäßiger Nutzung

Das System verfügt über verschiedene Schutzmechanismen, um unangemessene Verwendung der Ressourcen zu verhindern. Falls App-Daten nicht wie erwartet übertragen werden, wurde das Gerät wahrscheinlich vorübergehend beschränkt. Wenn Sie einige Zeit warten, wird dieses Problem normalerweise automatisch behoben, ohne dass eine Aktion erforderlich ist.

### <a name="versioning"></a>Versionsverwaltung

App-Daten können die Versionsinformationen nutzen, um von einer Datenstruktur auf eine andere zu aktualisieren. Die Versionsnummer unterscheidet sich von der App-Version und kann nach Belieben festgelegt werden. Auch wenn es nicht zwingend erforderlich ist, empfiehlt es sich, aufsteigende Versionsnummern zu verwenden, da beim Übergang zu einer kleineren Nummer, die neuere Daten darstellt, unerwünschte Komplikationen (z. B. Datenverluste) entstehen können.

Das Roaming für App-Daten wird nur in installierten Apps mit der gleichen Versionsnummer durchgeführt. Beispiel: Geräte mit der Versionsnummer 2 tauschen Daten untereinander aus. Gleiches gilt für Geräte mit der Version 3. Es erfolgt jedoch kein Roaming zwischen Geräten mit Version 2 und Geräten mit Version 3. Wenn Sie eine neue App installieren, die auf anderen Geräten unterschiedliche Versionsnummern verwendet hat, synchronisiert die neu installierte App die App-Daten, die mit der höchsten Versionsnummer verknüpft sind.

### <a name="testing-and-tools"></a>Tests und Tools

Entwickler können ihr Gerät sperren, um eine Synchronisierung von Roaming-App-Daten auszulösen. Wenn die App-Daten scheinbar nicht innerhalb eines bestimmten Zeitrahmens übertragen werden, überprüfen Sie die folgenden Elemente, und stellen Sie sicher, dass:

- Ihre Roamingdaten nicht die maximal zulässige Größe überschreiten (siehe auch [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)).
- Ihre Dateien geschlossen und ordnungsgemäß veröffentlicht wurden.
- Sie über mindestens zwei Geräte mit derselben App-Version verfügen.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Registrieren, um Benachrichtigungen bei der Änderung von Roamingdaten zu erhalten

Um das Roaming von App-Daten verwenden zu können, müssen Sie die Roamingdatenänderungen registrieren und die Roamingdatencontainer abrufen, damit Sie Einstellungen lesen und schreiben können.

1.  Nehmen Sie eine Registrierung für den Empfang einer Benachrichtigung vor, wenn sich Roamingdaten ändern.

    Das [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)-Ereignis benachrichtigt Sie, wenn sich Roamingdaten ändern. In diesem Beispiel wird `DataChangeHandler` als Handler für Änderungen an Roamingdaten festgelegt.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Rufen Sie die Container für die Einstellungen und Dateien der App ab.

    Rufen Sie mit der [**ApplicationData.RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)-Eigenschaft die Einstellungen und mit der [**ApplicationData.RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)-Eigenschaft die Dateien ab.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Erstellen und Abrufen von Roamingeinstellungen

Verwenden Sie die [**ApplicationDataContainer.Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)-Eigenschaft, um auf die Einstellungen im Container `roamingSettings` zuzugreifen, den wir im vorherigen Abschnitt abgerufen haben. In diesem Beispiel wird eine einfache Einstellung namens `exampleSetting` und ein zusammengesetzter Wert namens `composite` erstellt.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

Dieses Beispiel ruft die gerade erstellten Einstellungen ab.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>Erstellen und Abrufen von Roamingdateien

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows.Storage.FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync), um eine Datei im Roamingspeicher für App-Daten zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `roamingFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) und [**Windows.Storage.FileIO.ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync), um eine Datei im Roamingspeicher für App-Daten zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Abschnitt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>Temporäre App-Daten


Der temporäre App-Datenspeicher funktioniert wie ein Cache. Für diese Dateien erfolgt kein Roaming, und sie können jederzeit gelöscht werden. Daten, die an diesem Speicherort gespeichert werden, können von der Aufgabe zur Systemverwaltung jederzeit automatisch gelöscht werden. Benutzer können Dateien im temporären Datenspeicher auch mit der Datenträgerbereinigung löschen. Temporäre App-Daten können zum Speichern temporärer Informationen während einer App-Sitzung verwendet werden. Es gibt jedoch keine Garantie dafür, dass diese Daten über die App-Sitzung hinaus beibehalten werden, da der verwendete Speicherplatz ggf. vom System zurückgefordert wird. Der Speicherort ist über die [**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)-Eigenschaft verfügbar.

### <a name="retrieve-the-temporary-data-container"></a>Abrufen des temporären Datencontainers

Rufen Sie die Dateien mit der [**ApplicationData.TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)-Eigenschaft ab. In den nächsten Schritten wird die `temporaryFolder`-Variable aus diesem Schritt verwendet.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Erstellen und Lesen von temporären Dateien

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows.Storage.FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync), um eine Datei im temporären App-Datenspeicher zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `temporaryFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) und [**Windows.Storage.FileIO.ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync), um eine Datei im temporären App-Datenspeicher zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Schritt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>Organisieren von App-Daten mit Containern


Um Ihre Einstellungen für App-Daten und Dateien strukturiert zu speichern, erstellen Sie Container (dargestellt durch [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)-Objekte), statt direkt mit Verzeichnissen zu arbeiten. Sie können Container zu den lokalen, Roaming- und temporären App-Datenspeichern hinzufügen. Container können in bis zu 32 Ebenen geschachtelt werden.

Rufen Sie die [**ApplicationDataContainer.CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer)-Methode auf, um einen Einstellungscontainer zu erstellen. In diesem Beispiel wird ein lokaler Einstellungscontainer namens `exampleContainer` erstellt und eine Einstellung namens `exampleSetting` hinzugefügt. Der Wert **Always** aus der [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition)-Enumeration gibt an, dass der Container erstellt wird, sofern er noch nicht vorhanden ist.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>Löschen von App-Einstellungen und Containern


Verwenden Sie die [**ApplicationDataContainerSettings.Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove)-Methode, um eine einfache Einstellung zu löschen, die Ihre App nicht mehr benötigt. In diesem Beispiel wird die zuvor erstellte, lokale `exampleSetting`-Einstellung gelöscht.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Verwenden Sie die [**ApplicationDataCompositeValue.Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove)-Methode, um eine zusammengesetzte Einstellung zu löschen. In diesem Beispiel wird die lokale, zusammengesetzte `exampleCompositeSetting`-Einstellung gelöscht, die in einem früheren Beispiel erstellt wurde.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Rufen Sie die [**ApplicationDataContainer.DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer)-Methode auf, um einen Container zu löschen. In diesem Beispiel wird der zuvor erstellte, lokale `exampleContainer`-Einstellungscontainer gelöscht.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Versionsverwaltung Ihrer App-Daten


Optional können Sie die App-Daten für Ihre App mit einer Versionsnummer versehen. In diesem Fall können Sie neue Versionen der betreffenden App erstellen, mit denen das Format der App-Daten geändert wird, ohne dass es dadurch zu Kompatibilitätsproblemen mit der Vorgängerversion der App kommt. Die App prüft die Version der App-Daten im Dateispeicher. Handelt es sich um eine niedrigere Version als erwartet, aktualisiert die App sowohl die Anwendungsdaten auf das neue Format als auch die Version. Weitere Informationen finden Sie in den Artikeln zur [**Application.Version**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version)-Eigenschaft und zur [**ApplicationData.SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync)-Methode.

## <a name="related-articles"></a>Verwandte Artikel

* [**Windows.Storage.ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
