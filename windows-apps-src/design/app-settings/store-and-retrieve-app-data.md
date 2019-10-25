---
Description: Erfahren Sie, wie Sie lokale Daten, Roamingdaten und temporäre App-Daten speichern und abrufen.
title: Speichern und Abrufen von Einstellungen und anderen APP-Daten
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690365"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Speichern und Abrufen von Einstellungen und anderen APP-Daten

*App-Daten* sind änderbare Daten, die von einer bestimmten App erstellt und verwaltet werden. Es umfasst den Lauf Zeit Zustand, App-Einstellungen, Benutzereinstellungen, Referenz Inhalte (z. b. die Wörterbuchdefinitionen in einer Wörterbuch-App) und andere Einstellungen. App-Daten unterscheiden sich von *Benutzerdaten*, Daten, die der Benutzer erstellt und verwaltet, wenn Sie eine APP verwenden. Zu den Benutzerdaten gehören Dokument-oder Mediendateien, e-Mail-oder kommunikationstranskriptionen oder Datenbankeinträge, die vom Benutzer erstellte Inhalte enthalten. Benutzerdaten können für mehr als eine App nützlich oder sinnvoll sein. Häufig handelt es sich hierbei um Daten, die der Benutzer als Entität unabhängig von der APP selbst (z. b. ein Dokument) bearbeiten oder übertragen möchte.

**Wichtiger Hinweis zu app-Daten:** Die Lebensdauer der APP-Daten ist an die Lebensdauer der APP gebunden. Wenn die APP entfernt wird, gehen alle App-Daten als Folge verloren. Verwenden Sie keine app-Daten zum Speichern von Benutzerdaten oder etwas, das Benutzer als wertvoll und unersetzlich betrachten können. Es wird empfohlen, dass die Bibliotheken des Benutzers und Microsoft onedrive verwendet werden, um diese Art von Informationen zu speichern. App-Daten eignen sich ideal zum Speichern von App-spezifischen Benutzereinstellungen, Einstellungen und Favoriten.

## <a name="types-of-app-data"></a>Typen von App-Daten

Es gibt zwei Arten von App-Daten: Einstellungen und Dateien.

### <a name="settings"></a>Einstellungen

Verwenden Sie die Einstellungen, um Benutzereinstellungen und Anwendungs Zustandsinformationen zu speichern. Mit der APP Data-API können Sie problemlos Einstellungen erstellen und abrufen (einige Beispiele finden Sie weiter unten in diesem Artikel).

Die folgenden Datentypen können Sie für App-Einstellungen verwenden:

- **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
- **Boolescher Wert**
- **Char16**, **Zeichenfolge**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime), [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - Verwenden C#Sie für/.net Folgendes: [**System. DateTimeOffset**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0), [**System. TimeSpan**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**, [**Punkt**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Größe**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**Applicationdatacompositevalue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue): ein Satz verwandter App-Einstellungen, die atomarisch serialisiert und deserialisiert werden müssen. Verwenden Sie zusammengesetzte Einstellungen, um atomarische Updates von voneinander abhängigen Einstellungen problemlos zu verarbeiten. Das System stellt die Integrität der zusammengesetzten Einstellungen während des gleichzeitigen Zugriffs und Roamings sicher. Zusammengesetzte Einstellungen sind für kleine Datenmengen optimiert, und die Leistung kann gering sein, wenn Sie für große Datasets verwendet werden.

### <a name="files"></a>Dateien

Verwenden Sie Dateien zum Speichern von Binärdaten oder zum Aktivieren eigener, angepasster serialisierter Typen.

## <a name="storing-app-data-in-the-app-data-stores"></a>Speichern von App-Daten in App-Daten speichern


Wenn eine APP installiert ist, gibt das System Ihr eigene benutzerspezifische Datenspeicher für Einstellungen und Dateien an. Sie müssen nicht wissen, wo oder wie diese Daten vorhanden sind, da das System für die Verwaltung des physischen Speichers zuständig ist, und sicherstellen, dass die Daten von anderen apps und anderen Benutzern isoliert werden. Das System behält auch den Inhalt dieser Datenspeicher bei, wenn der Benutzer ein Update für Ihre APP installiert und den Inhalt dieser Datenspeicher vollständig und ordnungsgemäß entfernt, wenn Ihre APP deinstalliert wird.

Innerhalb des App-Datenspeicher verfügt jede APP über System definierte Stamm Verzeichnisse: eine für lokale Dateien, eine für roamingdateien und eine für temporäre Dateien. Ihre APP kann jedem dieser Stamm Verzeichnisse neue Dateien und neue Container hinzufügen.

## <a name="local-app-data"></a>Lokale App-Daten


Lokale App-Daten sollten für alle Informationen verwendet werden, die zwischen App-Sitzungen beibehalten werden müssen und nicht für Roaming von App-Daten geeignet sind. Daten, die auf anderen Geräten nicht anwendbar sind, sollten ebenfalls hier gespeichert werden. Es gibt keine allgemeine Größenbeschränkung für lokale Daten. Verwenden Sie den lokalen app-Datenspeicher für Daten, die für das Roaming und für große Datasets nicht sinnvoll sind.

### <a name="retrieve-the-local-app-data-store"></a>Abrufen des lokalen app-Datenspeicher

Bevor Sie lokale App-Daten lesen oder schreiben können, müssen Sie den lokalen app-Datenspeicher abrufen. Zum Abrufen des lokalen app-Datenspeicher verwenden Sie die [**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) -Eigenschaft, um die lokalen Einstellungen der App als [**applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) -Objekt abzurufen. Verwenden Sie die [**ApplicationData. localfolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) -Eigenschaft, um die Dateien in einem [**storagefolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) -Objekt abzurufen. Verwenden Sie die [**ApplicationData. localcachefolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder) -Eigenschaft, um den Ordner im lokalen app-Datenspeicher zu erhalten, in dem Sie Dateien speichern können, die nicht in der Sicherung und Wiederherstellung enthalten sind.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Erstellen und Abrufen einer einfachen lokalen Einstellung

Um eine Einstellung zu erstellen oder zu schreiben, verwenden Sie die [**applicationdatacontainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) -Eigenschaft, um auf die Einstellungen im `localSettings`-Container zuzugreifen, den wir im vorherigen Schritt erhalten haben. In diesem Beispiel wird eine Einstellung mit dem Namen `exampleSetting`erstellt.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Zum Abrufen der Einstellung verwenden Sie die gleiche [**applicationdatacontainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) -Eigenschaft, die Sie zum Erstellen der Einstellung verwendet haben. Dieses Beispiel zeigt, wie die soeben erstellte Einstellung abgerufen wird.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Erstellen und Abrufen eines lokalen zusammengesetzten Werts

Um einen zusammengesetzten Wert zu erstellen oder zu schreiben, erstellen Sie ein [**applicationdatacompositevalue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) -Objekt. In diesem Beispiel wird eine zusammengesetzte Einstellung mit dem Namen `exampleCompositeSetting` erstellt und dem Container `localSettings` hinzugefügt.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

Dieses Beispiel zeigt, wie der zusammengesetzte Wert abgerufen wird, den wir soeben erstellt haben.

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

Um eine Datei im lokalen app-Datenspeicher zu erstellen und zu aktualisieren, verwenden Sie die Datei-APIs, wie z [ **. b. Windows. Storage. storagefolder. kreatefileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows. Storage. FileIO. Write tetextasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). In diesem Beispiel wird eine Datei mit dem Namen `dataFile.txt` im `localFolder`-Container erstellt und das aktuelle Datum und die aktuelle Uhrzeit in die Datei geschrieben. Der **replacevorhandene** -Wert aus der-Enumeration von " [**kreationcollisionoption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) " zeigt an, dass die Datei ersetzt werden soll, wenn Sie bereits vorhanden

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

Verwenden Sie zum Öffnen und Lesen einer Datei im lokalen app-Datenspeicher die Datei-APIs, z. b [ **. Windows. Storage. storagefolder. getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. storagefile. getfilefromapplicationuriasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)und [**Windows. Storage. FileIO. infotextasync.** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). In diesem Beispiel wird die `dataFile.txt` Datei geöffnet, die im vorherigen Schritt erstellt wurde, und das Datum wird aus der Datei gelesen. Ausführliche Informationen zum Laden von Datei Ressourcen aus verschiedenen Speicherorten finden Sie unter Vorgehens [Weise beim Laden von Datei Ressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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


Wenn Sie in Ihrer APP Roamingdaten verwenden, können Ihre Benutzer Ihre APP-Daten auf mehreren Geräten problemlos synchron halten. Wenn ein Benutzer die APP auf mehreren Geräten installiert, hält das Betriebssystem die APP-Daten synchron, wodurch die Menge der Setup Vorgänge verringert wird, die der Benutzer für Ihre APP auf dem zweiten Gerät ausführen muss. Das Roaming ermöglicht Ihren Benutzern außerdem das Fortsetzen einer Aufgabe (z. b. das Verfassen einer Liste), wo Sie auch auf einem anderen Gerät verbleibt. Das Betriebssystem repliziert Roamingdaten bei der Aktualisierung in die Cloud und synchronisiert die Daten mit den anderen Geräten, auf denen die APP installiert ist.

Das Betriebssystem schränkt die Größe der APP-Daten ein, die von jeder App möglicherweise gewechselt werden. Siehe [**ApplicationData. roamingstoragequota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota). Wenn die APP diesen Grenzwert erreicht, wird keine app-Daten der app in die Cloud repliziert, bis die APP-Daten der APP insgesamt kleiner als der Grenzwert ist. Aus diesem Grund ist es eine bewährte Vorgehensweise, Roamingdaten nur für Benutzereinstellungen, Verknüpfungen und kleine Datendateien zu verwenden.

Roamingdaten für eine APP sind in der Cloud verfügbar, solange der Benutzer von einem Gerät innerhalb des erforderlichen Zeitintervalls darauf zugreifen kann. Wenn der Benutzer eine APP nicht länger als dieses Zeitintervall ausgeführt hat, werden die Roamingdaten aus der Cloud entfernt. Wenn ein Benutzer eine APP deinstalliert, werden die Roamingdaten nicht automatisch aus der Cloud entfernt. Wenn der Benutzer die APP innerhalb des Zeitintervalls erneut installiert, werden die Roamingdaten aus der Cloud synchronisiert.

### <a name="roaming-data-dos-and-donts"></a>Roamingdaten und "TS"

- Verwenden Sie Roaming für Benutzereinstellungen und Anpassungen, Links und kleine Datendateien. Verwenden Sie z. b. Roaming, um die Hintergrund Farbeinstellungen eines Benutzers auf allen Geräten beizubehalten.
- Verwenden Sie Roaming, um Benutzern das Fortsetzen einer Aufgabe auf Geräten zu ermöglichen. Beispielsweise können Sie App-Daten wie den Inhalt einer entworfenen e-Mail oder die zuletzt angezeigte Seite in einer Reader-App übertragen.
- Behandeln Sie das [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) -Ereignis, indem Sie App-Daten aktualisieren. Dieses Ereignis tritt auf, wenn die Synchronisierung von App-Daten aus der Cloud soeben abgeschlossen ist.
- Übertragen Sie Verweise auf den Inhalt anstelle von Rohdaten. Wechseln Sie z. b. eine URL anstatt den Inhalt eines Online Artikels.
- Verwenden Sie für wichtige, zeitkritische Einstellungen die mit [**roamingsettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)verknüpfte *HighPriority* -Einstellung.
- Roaming von App-Daten, die für ein Gerät spezifisch sind. Einige Informationen sind nur lokal relevant, z. b. ein Pfadname zu einer lokalen Datei Ressource. Wenn Sie sich entscheiden, lokale Informationen zu übertragen, stellen Sie sicher, dass die APP wieder hergestellt werden kann, wenn die Informationen auf dem sekundären Gerät nicht gültig sind.
- Wechseln Sie nicht zu großen Mengen von App-Daten. Die Menge der APP-Daten, die von einer APP möglicherweise gewechselt werden, ist begrenzt. Verwenden Sie die [**roamingstoragequota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) -Eigenschaft, um diesen maximalen Wert zu erhalten. Wenn eine APP diese Beschränkung erreicht, können keine Daten mehr Roaming durchlaufen, bis die Größe des App-Datenspeicher nicht mehr über dem Grenzwert liegt. Wenn Sie Ihre APP entwerfen, sollten Sie in Erwägung gezogen, dass Sie eine Grenze für größere Daten festlegen, damit der Grenzwert nicht überschritten wird. Wenn z. b. das Speichern eines Spiel Zustands jeweils 10 KB erfordert, gestattet die APP dem Benutzer möglicherweise nur bis zu 10 Spiele.
- Verwenden Sie Roaming nicht für Daten, die sofort synchronisiert werden. Eine sofortige Synchronisierung wird von Windows nicht gewährleistet. das Roaming kann erheblich verzögert werden, wenn ein Benutzer offline ist oder sich in einem Netzwerk mit hoher Latenz befindet. Stellen Sie sicher, dass die Benutzeroberfläche nicht von der sofortigen Synchronisierung abhängt.
- Verwenden Sie Roaming nicht für häufig geänderte Daten. Wenn Ihre APP z. b. häufig wechselnde Informationen nachverfolgt, z. b. die Position in einem Song, können Sie diese nicht als roaminganwendungsdaten speichern. Wählen Sie stattdessen eine seltener dargestellte Darstellung aus, die immer noch eine gute Benutzer Darstellung bietet, wie z. b. den aktuell Wiedergabe

### <a name="roaming-pre-requisites"></a>Erforderliche roamingvoraussetzungen

Jeder Benutzer kann von roamingdatendaten profitieren, wenn er eine Microsoft-Konto verwendet, um sich bei seinem Gerät anzumelden. Benutzer und Gruppenrichtlinien Administratoren können jedoch jederzeit roaminganwendungsdaten auf einem Gerät deaktivieren. Wenn ein Benutzer sich dafür entscheidet, keine Microsoft-Konto zu verwenden oder die roamingdatenfunktionen zu deaktivieren, kann er die APP weiterhin verwenden, aber die APP-Daten werden lokal auf jedem Gerät angezeigt.

Die in [**passwordvault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault) gespeicherten Daten werden nur dann übertragen, wenn ein Benutzer ein Gerät als vertrauenswürdig eingestuft hat. Wenn ein Gerät nicht vertrauenswürdig ist, werden die in diesem Tresor gesicherten Daten nicht übertragen.

### <a name="conflict-resolution"></a>Konfliktlösung

Roaminganwendungsdaten sind nicht für die gleichzeitige Verwendung auf mehr als einem Gerät gleichzeitig vorgesehen. Wenn während der Synchronisierung ein Konflikt auftritt, weil eine bestimmte Dateneinheit auf zwei Geräten geändert wurde, wird der zuletzt geschriebene Wert vom System immer bevorzugt. Dadurch wird sichergestellt, dass die APP die aktuellsten Informationen verwendet. Wenn die Dateneinheit eine zusammengesetzte Einstellung ist, wird die Konfliktlösung weiterhin auf der Ebene der Einstellungs Einheit ausgeführt, was bedeutet, dass das zusammengesetzte mit der aktuellen Änderung synchronisiert wird.

### <a name="when-to-write-data"></a>Wann Daten geschrieben werden sollen

Abhängig von der erwarteten Lebensdauer der Einstellung müssen Daten zu unterschiedlichen Zeiten geschrieben werden. Daten, die selten oder langsam geändert werden, sollten sofort geschrieben werden. App-Daten, die sich häufig ändern, sollten jedoch regelmäßig in regelmäßigen Abständen (z. b. alle 5 Minuten) geschrieben werden, und wenn die APP angehalten wird. Eine Musik-app könnte z. b. die "aktuellen Song"-Einstellungen schreiben, wenn ein neuer Song wiedergegeben wird. die tatsächliche Position im Song sollte jedoch nur auf Suspend geschrieben werden.

### <a name="excessive-usage-protection"></a>Übermäßiger Nutzungs Schutz

Das System verfügt über verschiedene Schutzmechanismen, um eine ungeeignete Verwendung von Ressourcen zu vermeiden. Wenn App-Daten nicht erwartungsgemäß wechseln, ist es wahrscheinlich, dass das Gerät vorübergehend eingeschränkt wurde. Wenn Sie einige Zeit warten, wird diese Situation in der Regel automatisch behoben, und es ist keine Aktion erforderlich.

### <a name="versioning"></a>Versionsverwaltung

App-Daten können die Versionsverwaltung nutzen, um ein Upgrade von einer Datenstruktur auf eine andere durchzuführen. Die Versionsnummer unterscheidet sich von der App-Version und kann bei der Ausführung festgelegt werden. Obwohl Sie nicht erzwungen wird, wird dringend empfohlen, dass Sie steigende Versionsnummern verwenden, da unerwünschte Komplikationen (einschließlich Datenverlust) auftreten können, wenn Sie versuchen, zu einer niedrigeren Daten Versionsnummer zu wechseln, die neuere Daten darstellt.

App-Daten wechseln nur zwischen installierten apps mit derselben Versionsnummer. Beispielsweise werden Geräte in Version 2 miteinander übertragen, und die Geräte in Version 3 werden identisch sein, aber zwischen einem Gerät mit Version 2 und einem Gerät, auf dem Version 3 ausgeführt wird, erfolgt kein Roaming. Wenn Sie eine neue App installieren, die verschiedene Versionsnummern auf anderen Geräten verwendet hat, synchronisiert die neu installierte APP die APP-Daten, die mit der höchsten Versionsnummer verknüpft sind.

### <a name="testing-and-tools"></a>Tests und Tools

Entwickler können Ihr Gerät sperren, um eine Synchronisierung der roamingdatenapp-Daten zu initiieren. Wenn die APP-Daten anscheinend nicht innerhalb eines bestimmten Zeitraums übergehen, überprüfen Sie die folgenden Elemente, und stellen Sie Folgendes sicher:

- Ihre Roamingdaten überschreiten die maximal zulässige Größe (Weitere Informationen finden Sie unter [**roamingstoragequota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) ).
- Ihre Dateien werden geschlossen und ordnungsgemäß freigegeben.
- Es gibt mindestens zwei Geräte, auf denen dieselbe Version der app ausgeführt wird.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>Bei Änderungen an Roamingdaten registrieren

Wenn Sie roamingdatendaten verwenden möchten, müssen Sie sich für roamingdatenänderungen registrieren und die roamingdatencontainer abrufen, damit Sie Einstellungen lesen und schreiben können.

1.  Registrieren Sie sich, um Benachrichtigungen zu erhalten, wenn Roamingdaten

    Das [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) -Ereignis benachrichtigt Sie, wenn Roamingdaten geändert werden. In diesem Beispiel wird `DataChangeHandler` als Handler für roamingdatenänderungen festgelegt.

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

2.  Sie erhalten die Container für die App-Einstellungen und-Dateien.

    Verwenden Sie die [**ApplicationData. roamingsettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) -Eigenschaft, um die Einstellungen und die [**ApplicationData. roamingfolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) -Eigenschaft zum erhalten der Dateien zu erhalten.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Erstellen und Abrufen von Roamingeinstellungen

Verwenden Sie die [**applicationdatacontainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) -Eigenschaft, um auf die Einstellungen in dem `roamingSettings`-Container zuzugreifen, den wir im vorherigen Abschnitt erhalten haben. Dieses Beispiel erstellt eine einfache Einstellung mit dem Namen `exampleSetting` und einen zusammengesetzten Wert mit dem Namen `composite`.

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

In diesem Beispiel werden die soeben erstellten Einstellungen abgerufen.

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

### <a name="create-and-retrieve-roaming-files"></a>Erstellen und Abrufen von roamingdateien

Verwenden Sie die Datei-APIs, z. b [ **. Windows. Storage. storagefolder. anatefileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows. Storage. FileIO. beschreitetextasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync), um eine Datei im Datenspeicher für Roaming-apps zu erstellen und zu aktualisieren. In diesem Beispiel wird eine Datei mit dem Namen `dataFile.txt` im `roamingFolder`-Container erstellt und das aktuelle Datum und die aktuelle Uhrzeit in die Datei geschrieben. Der **replacevorhandene** -Wert aus der-Enumeration von " [**kreationcollisionoption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) " zeigt an, dass die Datei ersetzt werden soll, wenn Sie bereits vorhanden

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

Verwenden Sie zum Öffnen und Lesen einer Datei im roamingapp-Datenspeicher die Datei-APIs, wie z [ **. b. Windows. Storage. storagefolder. getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. storagefile. getfilefromapplicationuriasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync), und [ **Windows. Storage. fleio. Read textasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). In diesem Beispiel wird die `dataFile.txt` Datei geöffnet, die im vorherigen Abschnitt erstellt wurde, und das Datum wird aus der Datei gelesen. Ausführliche Informationen zum Laden von Datei Ressourcen aus verschiedenen Speicherorten finden Sie unter Vorgehens [Weise beim Laden von Datei Ressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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


Der temporäre App-Datenspeicher funktioniert wie ein Cache. Die Dateien werden nicht gewechselt und können jederzeit entfernt werden. Der Task "System Wartung" kann Daten, die an diesem Speicherort gespeichert sind, jederzeit automatisch löschen. Der Benutzer kann auch Dateien aus dem temporären Datenspeicher mithilfe der Datenträger Bereinigung löschen. Temporäre App-Daten können verwendet werden, um temporäre Informationen während einer App-Sitzung zu speichern. Es gibt keine Garantie, dass diese Daten nach dem Ende der App-Sitzung beibehalten werden, da das System ggf. den genutzten Speicherplatz freigeben könnte. Der Speicherort ist über die [**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) -Eigenschaft verfügbar.

### <a name="retrieve-the-temporary-data-container"></a>Abrufen des temporären Daten Containers

Verwenden Sie die [**ApplicationData. temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) -Eigenschaft, um die Dateien zu erhalten. In den nächsten Schritten wird die `temporaryFolder` Variable aus diesem Schritt verwendet.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Erstellen und Lesen von temporären Dateien

Um eine Datei im temporären App-Datenspeicher zu erstellen und zu aktualisieren, verwenden Sie die Datei-APIs, wie z [ **. b. Windows. Storage. storagefolder. kreatefileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) und [**Windows. Storage. FileIO. Write tetextasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). In diesem Beispiel wird eine Datei mit dem Namen `dataFile.txt` im `temporaryFolder`-Container erstellt und das aktuelle Datum und die aktuelle Uhrzeit in die Datei geschrieben. Der **replacevorhandene** -Wert aus der-Enumeration von " [**kreationcollisionoption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) " zeigt an, dass die Datei ersetzt werden soll, wenn Sie bereits vorhanden


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

Um eine Datei im temporären App-Datenspeicher zu öffnen und zu lesen, verwenden Sie die Datei-APIs, wie z [ **. b. Windows. Storage. storagefolder. getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. storagefile. getfilefromapplicationuriasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync), und [ **Windows. Storage. fleio. Read textasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). In diesem Beispiel wird die `dataFile.txt` Datei geöffnet, die im vorherigen Schritt erstellt wurde, und das Datum wird aus der Datei gelesen. Ausführliche Informationen zum Laden von Datei Ressourcen aus verschiedenen Speicherorten finden Sie unter Vorgehens [Weise beim Laden von Datei Ressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

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


Zur Unterstützung beim Organisieren von App-Daten Einstellungen und-Dateien erstellen Sie Container (dargestellt durch [**applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) -Objekte), anstatt direkt mit Verzeichnissen zu arbeiten. Sie können Container zu den lokalen, Roaming-und temporären App-Daten speichern hinzufügen. Container können bis zu 32 Ebenen tief verschachtelt werden.

Um einen Einstellungs Container zu erstellen, rufen Sie die [**applicationdatacontainer. kreatecontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer) -Methode auf. In diesem Beispiel wird ein lokaler Einstellungs Container mit dem Namen `exampleContainer` erstellt und eine Einstellung mit dem Namen `exampleSetting`hinzugefügt. Der **Always** -Wert aus der [**applicationdatacreatedisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition) -Enumeration gibt an, dass der Container erstellt wird, wenn er nicht bereits vorhanden ist.

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

## <a name="delete-app-settings-and-containers"></a>App-Einstellungen und Container löschen


Verwenden Sie die [**applicationdatacontainersettings. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove) -Methode, um eine einfache Einstellung zu löschen, die von der APP nicht mehr benötigt wird. In diesem Beispiel wird die `exampleSetting` lokale Einstellung, die wir zuvor erstellt haben, angezeigt.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Um eine zusammengesetzte Einstellung zu löschen, verwenden Sie die [**applicationdatacompositevalue. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove) -Methode. In diesem Beispiel wird die lokale `exampleCompositeSetting` die zusammengesetzte Einstellung gelöscht, die wir in einem früheren Beispiel erstellt haben.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Um einen Container zu löschen, müssen Sie die [**applicationdatacontainer. deletecontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer) -Methode aufrufen. In diesem Beispiel wird der zuvor erstellte Container "local `exampleContainer` Settings" gelöscht.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Versionierung Ihrer APP-Daten


Optional können Sie die APP-Daten für Ihre APP versionsversionen. Dies ermöglicht es Ihnen, eine zukünftige Version Ihrer APP zu erstellen, die das Format Ihrer APP-Daten ändert, ohne Kompatibilitätsprobleme mit der vorherigen Version Ihrer APP zu verursachen. Die APP überprüft die Version der APP-Daten im Datenspeicher. wenn die Version niedriger ist als die Version, die von der APP erwartet wird, sollte die APP die APP-Daten auf das neue Format aktualisieren und die Version aktualisieren. Weitere Informationen finden Sie unter der[**Application. Version**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version) -Eigenschaft und der [**ApplicationData. setversionasync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) -Methode.

## <a name="related-articles"></a>Verwandte Artikel

* [**Windows. Storage. ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**Windows. Storage. ApplicationData. roamingsettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**Windows. Storage. ApplicationData. roamingfolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**Windows. Storage. ApplicationData. roamingstoragequota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**Windows. Storage. applicationdatacompositevalue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
