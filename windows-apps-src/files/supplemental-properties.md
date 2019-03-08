---
title: Verwenden die zusätzliche Eigenschaften
description: Einführung in die Verwendung von zusätzlichen Eigenschaften und Details auf, wie sie in Windows implementiert wurden
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, Uwp, WinRT-API, Indexer, Search
localizationpriority: medium
ms.openlocfilehash: b2ac43c9aa2d27f8745e9075abc13d8feaba2370
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592685"
---
# <a name="using-supplemental-properties"></a>Verwenden die zusätzliche Eigenschaften  

## <a name="summary"></a>Zusammenfassung  
- Zusätzliche Eigenschaften können apps Dateien kennzeichnen mit Eigenschaften, ohne die Datei zu ändern 
- Hilfreich für die Fälle, in denen Sie Eigenschaften, die schwer zu berechnen sind oder die Datei nicht geändert werden kann 
- Verwenden die zusätzliche Eigenschaften ist identisch mit der eine andere Eigenschaft des Systems für die Windows-Eigenschaft  

## <a name="introduction"></a>Einführung 
Viele interessante neue Apps in den letzten Jahren erfordern, mit der CPU-intensive Vorgänge für Benutzerdateien, um nützliche Eigenschaften aus den Dateien über grundlegende Dinge wie Erstellungsdatum zu extrahieren. Diese apps in Bildern, beabsichtigte Extrahierung in e-Mails und Textanalyse zu Dokumenten der Gruppe zusammen Objekt. Dies wird gesteuert wird, um wie leistungsstarke computing ist nun auf die meisten Consumer PCs verfügbar.   

Durchsuchbar diese Metadaten sofort ermöglicht Benutzern eine exponentiell höhere Produktivität. Einfach zu wissen, dass Ihre Tochter in einem Bild ist, ist interessant, aber wird für das Bild Ihrer mit ihrem Großmutter durchsuchen können so viel nützlicher ist. Es ist die Verwendung von einem Computer Verhalten persönlichere und mehr aktiv. Wie ein Benutzer auf dem Computer erreicht können Sie Ihre geliebte leistungsanforderungen zu ermitteln. 

Seit Jahrzehnten die Lösung für die schnelle Suche von Windows wurde der Indexer, und in dem Creators Update es wurde aktualisiert, um diese neuen Szenarien zu unterstützen. Apps können jetzt Dateien kennzeichnen mit zusätzlichen Eigenschaften hinaus, die die vom System extrahiert werden. Diese Eigenschaften werden als Bürger erster Klasse behandelt.  

## <a name="windows-properties"></a>Windows-Eigenschaften 
Die [Windows Eigenschaftensystem](https://msdn.microsoft.com/library/windows/desktop/ff728898) wurde ein wichtiger Bestandteil der Interaktion mit Dateien seit Jahren. Sie können apps Eigenschaften aus Dateien lesen, ohne Sie zu den Interna von allen unterschiedliche Dateiformate oder Sprachen, die, denen in eine Datei befinden. All dies abstrahiert für Sie als Entwickler, alles, was man dazu Unternehmen muss stellen Sie eine Liste, und geben Sie Aufsteigend oder Absteigend.  

Das Eigenschaftensystem ist eng verknüpft, mit dem Windows-Indexer – alle Eigenschaften von Dateien in ihrem Gültigkeitsbereich liest und speichert sie. Wenn eine app fordert eine Liste aller DOCX, die sich in einem Ordner nach Datum der Änderung, sortiert werden mit Ausnahme derjenigen von John Smith den Indexer erstellt wurden kann die Liste sofort zurückkommen.  

Der Nachteil dieser Systeme Zusammenspiel ist, der Indexer verwendet, um alle Eigenschaften festzulegen, es zu einer Datei sofort verfügbar sein speichern würde. Dies beschränkt es zu wissen zu Weitere interessante Eigenschaften, die länger zu berechnen, da es schwierig erforderlich ist.  

Mithilfe von Eigenschaften jedoch einfach, die app können entweder einen sortierten Satz von Eigenschaften zu einer Datei, ähnlich wie die Arbeit mit einer Datenbank, oder sie können eine Abfrage wie die Verwendung einer Suchmaschine übergeben. Der Indexer wird die Abfrage verarbeitet und die Ergebnisse zurückgeben. Diese bieten Entwicklern die Flexibilität, kombinieren Sie ihre Filter (z. B. nur Search Jpg-Dateien) mit der Abfrage des Benutzers (beginnend mit "Bird" Dateiname). 

## <a name="supplemental-properties"></a>Zusätzliche Eigenschaften 

Zusätzliche Eigenschaften verhalten sich identisch zu regulären Windows-Eigenschaften mit einem sehr wichtigen Unterschied: sie wollen nicht geschrieben werden, wenn die Datei an den Indexer hinzugefügt wird. Eine zusätzliche Eigenschaft muss von einer anderen app auf dem System zu einem späteren Zeitpunkt hinzugefügt werden. Es könnte sein, objekterkennung in zwei Minuten später einmal abgeschlossen ist oder die Tage später sein. 

Wenn die Eigenschaft geschrieben wird kann es werden durchsucht, gefiltert, sortiert oder gruppiert werden, genau wie jede andere Eigenschaft, auf dem System. Auch verwendet werden in der kombinierten Abfragen mit anderen Eigenschaften auf dem System, entweder ergänzende oder nicht. Dies wird Ihnen die Flexibilität, leicht zusätzliche Eigenschaften mit Ihren vorhandenen Code der Datei System kombinieren, ohne eine Neuerstellung durchgeführt werden.  

### <a name="example-scenarios"></a>Beispielszenarien 

Es gibt unzählige verschiedene Eigenschaften, die Sie auf eine zusätzliche Eigenschaft schreiben können, aber es gibt einige wichtige Szenarios, denen in diesem Tutorial zurück, Dienste weiterhin nutzen möchten, wird:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Markieren Bilder mit den extrahierten Eigenschaften 
Diese apps können ein trainiertes ML-Modells verwenden, um Funktionen aus einem Image zu extrahieren, die das System z. B.-Objekt in der Abbildung nicht bekannt ist. Klicken Sie dann kann die Objekte, die sie in der Abbildung identifiziert werden und das Eigenschaftensystem für später Suche und Gruppierung hinzufügen.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Kennzeichnen von Dateien mit einer bestimmten app-ID 
Viele Datei Sync-apps verwenden ihre eigenen eindeutige ID, um Dateien nachzuverfolgen, wie sie zwischen dem Server und verschiedenen Clientgeräten verschieben. Synchronisierungsclient kann diese ID im Eigenschaftensystem schreiben, ohne die Datei beeinflusst. Diese ID ist jetzt für die app später für den schnellen Zugriff verfügbar und für jede andere app auf dem System gelesen, bei der Kommunikation mit den Synchronisierungsanbieter verfügbar. 

Es gibt viele weitere Optionen für die Verwendung von zusätzlicher Eigenschaften, aber beide gute Beispiele für vorgenommen werden, da sie schnelle Suche oder suchen Sie erfordern, sind Informationen, die das System nicht kennen, und kann nicht hinzugefügt werden, auf die Datei selbst.  

### <a name="using-supplemental-properties"></a>Verwenden die zusätzliche Eigenschaften 
Verwenden die zusätzlichen Eigenschaften ist identisch mit der eine normale Eigenschaft in das Dateisystem schreiben. Wenn Sie mit der Verwendung von unterstütztes und Eigenschaften vertraut sind, können Sie über diese überspringen. Andernfalls, betrachten wir ein Beispiel, das von einer einzelnen Eigenschaft in eine Datei schreiben und Lesen dann später wieder die gleiche Eigenschaft.  

### <a name="writing-supplemental-properties"></a>Schreiben von zusätzlichen Eigenschaften  
Im Beispiel wird nur die erste Datei, die aus Gründen der Einfachheit gefunden ändern, aber in der Regel wird eine app die Eigenschaft hinzuzufügen, zu jeder Datei, die es findet.  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

Es ist eine wichtige Überprüfung auf, wenn die Position vor dem Schreiben einer Eigenschaft indiziert ist. In diesem Beispiel verwenden wir die Abfrageoptionen zum Filtern, sodass nur indizierten Orten. Wenn dies nicht möglich ist, können Sie den indizierten Zustand des übergeordneten Ordners (Datei. Überprüfen GetParentAsync(). GetIndexedStateAsync()). In beiden Fällen werden die gleichen Ergebnisse liefern. 

### <a name="reading-supplemental-properties"></a>Lesen Sie die zusätzliche Eigenschaften 
In diesem Fall entspricht eine zusätzliche Eigenschaft lesen eine andere Datei System-Eigenschaft lesen. In diesem Beispiel die app wird nur eine Eigenschaft aus einer Datei lesen, die, der Sie bereits über eine "storagefile" verfügt, aber es könnte auch andere Eigenschaften lesen, zur gleichen Zeit.  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
Es ist eine Überprüfung, um sicherzustellen, dass der Wert, der von dem Eigenschaftensystem ist, was Sie erwarten. Obwohl es unwahrscheinlich ist, ist es möglich, da Ihre app es geschrieben hat der Wert gelöscht wurde. Dies wird weiter unten ausführlich behandelt.  

### <a name="implementation-notes"></a>Hinweise zur Implementierung 
Es gibt einige geringfügige Optionen, die in den Entwurf der die zusätzlichen Eigenschaften vorgenommen wurden. Um Ihre Implementierung zu vereinfachen wurden in den folgenden Abschnitten aus der technischen Entwurf-Spezifikation für die Funktion kopiert. Sie bieten einen Einblick, wie die Funktion entwickelt wurde, und warum einige der Einschränkungen vorhanden sind. 

### <a name="supplemental-properties-available"></a>Zusätzliche Eigenschaften zur Verfügung stehen 
Es gibt nur zwei Eigenschaften, die ursprünglich für apps verwendet werden: System.Supplemental.ResourceId und System.Supplemental.AlbumID. Wenn besteht die Notwendigkeit mehr, die sie hinzugefügt werden können. Die Album-ID ist eine mehrwertige Zeichenfolge, die für viele verschiedene Anwendungen verwendet werden kann, und die Ressourcen-ID wird als eine eindeutige ID für die Synchronisierung von cloudanbietern verwendet. 

#### <a name="file-system-support"></a>Unterstützung des 
Da FAT-formatierten Wechseldatenträger ist ein wichtiges Szenario, zusätzliche Eigenschaften unterstützt FAT und NTFS-Laufwerke. Dadurch wird sichergestellt, dass die zusätzlichen Eigenschaften werden für alle Benutzer, unabhängig von den jeweiligen Gerätetyp verfügbar sein sollen.   

### <a name="non-indexed-locations"></a>Nicht indizierten Orten  
Es gibt eine Reihe von Ordnern, die nicht indiziert werden, auf dem Desktop. In diesen Fällen sollten apps immer noch Zugriff auf zusätzliche Eigenschaften haben. Zusätzliche Eigenschaften sind jedoch nicht außerhalb der indizierten Orte zur Verfügung steht. Dieser Kompromiss wurde aus verschiedenen Gründen vorgenommen:  

- Alle Bibliotheken und cloudspeicherorten werden standardmäßig indiziert.   
  Dies sind die Speicherorte, UWP-apps in erster Linie verwenden. Es gibt andere Speicherorte, die nicht indizierte (System oder Netzwerk-Laufwerke) sind, aber sie werden weniger häufig zum Speichern von Benutzerdaten verwendet. 

- Die Oberfläche WinRT-API-Entwurf wird davon ausgegangen, dass der Indexer fast immer verfügbar ist.  
  Daher ist der Indexer bereits in den meisten von den Orten, die apps interessiert sind verfügbar. Wenn der Benutzer gefunden werden, um Daten in nicht indizierten Orten speichern, wird die einfachste Lösung sein, um diesen Speicherort zum Index hinzufügen. Klicken Sie dann zusätzliche Eigenschaften arbeiten, Enumeration werden schneller, und apps ist in der Lage, ändern Sie die Position nachzuverfolgen.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Beim Lesen oder Schreiben von zusätzlichen Eigenschaften aus einer Datei an einem Ort Non-Indexed 
Im Fall, den eine app versucht, eine zusätzliche Eigenschaft an einem Speicherort zu schreiben, die derzeit nicht indiziert ist, klicken Sie dann löst der API-Aufruf eine Ausnahme. Dies ist die gleiche Ausnahme werden, die als ausgelöst wird, wenn jemand versucht, den System.Music.AlbumArtist für DOCX-Datei (Ungültiger Args) zu aktualisieren.  
 
### <a name="change-notifications"></a>Benachrichtigungen zu ändern:  
Änderungsbenachrichtigungen für die UWP und änderungsnachverfolgung werden weiterhin für die zusätzlichen Eigenschaften wie bei Standardeigenschaften. Diese ermöglicht apps, die Daten, um alle Änderungen an eine ihrer apps nachverfolgen bereitstellen 
  
### <a name="invalidating-properties"></a>Eigenschaften für ungültig zu erklären:  
Die zusätzlichen Eigenschaften für eine Datei ist möglicherweise veraltet, wenn eine Datei geändert oder werden, auf dem System verschoben. Apps, die mithilfe von Push übertragen die Daten werden diejenigen mit den Informationen zu, wenn die Daten gültig ist oder aktualisiert werden, damit das System nur die Tools für sie es selbst herausfinden, bereitgestellt werden müssen.  
 
Im Fall eine Datei geändert wird, jedoch nicht verschoben oder umbenannt, die zusätzlichen Eigenschaften für die Datei bleibt unverändert. Die apps werden können, registrieren Sie sich für Benachrichtigungen über die vorhandene API-Oberfläche, und aktualisieren Sie die Eigenschaften nach Bedarf. 
 
Wenn die Datei verschoben wird, werden die Eigenschaften für ungültig erklärt werden soll. Die app wird empfangen die änderungsbenachrichtigungen entweder löschen, zu erstellen, umbenannt oder verschoben werden, je nachdem, wie genau der Vorgang abgeschlossen wurde. Die app hat die änderungsbenachrichtigung erhalten werden können, überprüfen Sie die Datei, und aktualisieren die zusätzlichen Eigenschaften für die Datei aus, je nach Bedarf. 
 
### <a name="indexer-rebuilds"></a>Indexer neu erstellt.  
Gelegentlich der System-Index für eine der aus verschiedenen Gründen neu erstellt werden muss – das Eigenschaftsschema ändern kann, der Benutzer konnte EDP aktivieren oder einfach die Datei kann beschädigt werden. In diesen Fällen werden die zusätzlichen Eigenschaften nicht beibehalten. Es berücksichtigt die Arbeit, um zu versuchen, die zusätzlichen Eigenschaften zu erhalten, wenn der Index neu erstellt wird wird, aber es eine Reihe von wichtigen Blockierungen gab:  

### <a name="protecting-the-data"></a>Schutz von Daten 
Im Fall, in dem die Datenbankdatei beschädigt ist, entweder von Datenträgerfehlern oder Rogue-Verwaltungssoftware, wird es nicht möglich, die Daten zu schützen, die in dieser Datei gespeichert wurde. Sie müssen an anderer Stelle auf dem System oder irgendwie isoliert vom Rest der Datenbank gespeichert werden. 

Da wir bereits sehr viel Arbeit, die der Index weniger wahrscheinlich ist möglicherweise beschädigt durchführen, verringert das die Anzahl der diesem Fall für die Häufigkeit dennoch.  
Verwalten die Zuordnung zwischen Dateien und die Metadaten während der Neuerstellung 

Auch wenn der Index der Daten über eine Neuerstellung schützen kann, ist es unmöglich zu wissen, ob die Datei wurde während der Index neu erstellt wird, wurde geändert. Die Daten, die der Index aus der Datei geschützt ist möglicherweise nicht mehr gültig sein, wenn die Datei geändert oder verschoben wird.  
Verhalten 

Bei einer Neuerstellung des Indexers verloren alle zusätzliche Daten. Apps muss selbst wieder einfügen von Daten in den Indexer, der während der Wiederherstellung verloren gegangen. Dies eine zusätzliche Belastung für die apps eingefügt, jedoch wird angemessenen angesehen, da sie immer den master-Status für alle ihre Daten enthält.  

### <a name="recovering"></a>Wiederherstellen von 
Nachdem Sie die apps haben festgestellt, dass der Index neu erstellt wird, werden sie für die Aktualisierung der zusätzlichen Eigenschaften nach Bedarf verantwortlich.  
### <a name="privacy"></a>Vertraulichkeit 
Einige der Eigenschaften, die auf die Dateien geschrieben werden können auch, dass Benutzer nicht für andere Anwendungen freigegeben werden sollen möglicherweise. Apps sollten in der Lage, um anzugeben, dass die Informationen, die sie in den Eigenschaften schreiben sollen, entweder privat sein. ihre Anwendungen, mit nur wenigen anderen Anwendungen gemeinsam genutzten oder öffentlichen für jede app auf dem System.  

Obwohl dies möglicherweise ein interessantes Feature für einige der frühen Anwender der Funktion ist, können sie, dass die erste öffentlicher Eigenschaften immer noch viele Werte für das Design hinzufügen ausgeführt wird. Daher wird dies als eine hilfreich markiert, und wir sollten weiterhin erstellen Sie das Feature ohne Unterstützung für die Werte ausblenden, falls erforderlich. Es später noch Mal hinzugefügt wird weitere Szenarios, geöffnet, damit es in alle Designs berücksichtigt werden.  

## <a name="conclusions"></a>Schlussfolgerungen 
Das war alles, zusätzliche Eigenschaften sind eine einfache Möglichkeit, weitere Eigenschaften der Datei in das System zu speichern. Ihre Verwendung ist natürlich optional, es kann aber zu Ihrer app eine Kante über andere apps, die sortieren und Durchsuchen ihre Daten so schnell können nicht. 

Wir suchen nach vorne auf Ihre apps starten, um diese Eigenschaften verwenden. Wenn Sie haben Fragen zur Verwendung der Header geben uns in den Kommentaren zu informieren 
