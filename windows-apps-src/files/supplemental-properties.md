---
title: Verwenden von Zusatzeigenschaften
description: Enthält eine Einführung in die Verwendung von Zusatzeigenschaften und Details zur Implementierung in Windows.
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, WinRT-API, Indexer, Suche
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369265"
---
# <a name="using-supplemental-properties"></a>Verwenden von Zusatzeigenschaften  

## <a name="summary"></a>Zusammenfassung  
- Zusatzeigenschaften ermöglichen Apps das Kennzeichnen von Dateien mit Eigenschaften, ohne die Datei zu ändern. 
- Dies ist in Fällen hilfreich, in denen Eigenschaften schwierig zu berechnen sind oder die Datei nicht geändert werden kann. 
- Die Verwendung von Zusatzeigenschaften entspricht der Vorgehensweise für alle anderen Eigenschaften im Windows-Eigenschaftensystem.  

## <a name="introduction"></a>Einführung 
Für viele der aufregenden neuen Apps ist in den letzten Jahren die Durchführung von CPU-intensiven Vorgängen für Benutzerdateien erforderlich gewesen, um nützliche Eigenschaften aus den Dateien zu extrahieren, die über grundlegende Informationen wie das Erstellungsdatum hinausgehen. In diesen Apps wird die Objekterkennung in Bildern, die Extraktion von Absichten aus E-Mails und die Textanalyse zum Gruppieren von Dokumenten durchgeführt. Der Auslöser hierfür ist, dass mittlerweile die meisten Heim-PCs über eine hohe Rechenleistung verfügen.   

Benutzer können deutlich produktiver arbeiten, wenn sie die Möglichkeit erhalten, diese Metadaten schnell zu durchsuchen. Es ist gut, wenn ein Benutzer weiß, dass seine Tochter auf einem Bild zu sehen ist. Viel nützlicher ist es aber, wenn er nach einem Bild suchen kann, auf dem seine Tochter mit ihrer Großmutter abgebildet ist. Die Nutzung eines Computers wird hierdurch persönlicher, und der Benutzer ist stärker eingebunden. Es ist so, als ob der Computer sich direkt an den Benutzer wendet, um ihm bei der Suche nach schönen Erinnerungen zu helfen. 

Mehrere Jahrzehnte lang war der Indexer die Lösung für das schnelle Suchen unter Windows. Im Rahmen des Creators Update wurde er aktualisiert, um diese neuen Szenarien zu unterstützen. Apps können Dateien jetzt mit zusätzlichen Eigenschaften kennzeichnen, die über die vom System extrahierten Eigenschaften hinausgehen. Diese Eigenschaften werden vorrangig behandelt.  

## <a name="windows-properties"></a>Windows-Eigenschaften 
Das [Windows-Eigenschaftensystem](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system) war viele Jahre lang ein wichtiger Bestandteil der Interaktion mit Dateien. Es ermöglicht Apps das Lesen von Eigenschaften aus Dateien, ohne dass Informationen zu allen internen Merkmalen der unterschiedlichen Dateiformate oder Sprachen vorliegen müssen, die für eine Datei gelten können. All dies wird für dich als Entwickler abstrahiert, und du musst nur noch eine Liste anfordern und „Aufsteigend“ oder „Absteigend“ angeben.  

Das Eigenschaftensystem ist eng mit dem Indexer von Windows verknüpft. Es liest alle Eigenschaften aus den Dateien im jeweiligen Bereich aus und speichert sie. Wenn eine App später dann eine nach Änderungsdatum sortierte Liste mit allen DOCX-Dateien eines Ordners anfordert, die nicht von „John Smith“ erstellt wurden, kann diese Liste vom Indexer sofort zurückgegeben werden.  

Der Nachteil bei der Zusammenarbeit dieser Systeme besteht darin, dass für den Indexer alle Eigenschaften, die er für eine Datei speichern muss, sofort verfügbar sein mussten. Aufgrund der strengen Zeitbeschränkung wurde verhindert, dass er Informationen zu interessanteren Eigenschaften mit längerer Berechnungsdauer erhalten hat.  

Die Verwendung von Eigenschaften ist aber einfach. Die App kann entweder – ähnlich wie bei einer Datenbank – einen sortierten Eigenschaftensatz zu einer Datei anfordern, oder sie kann eine Abfrage übergeben, z. B. zur Nutzung einer Suchmaschine. Der Indexer verarbeitet die Abfrage und gibt die Ergebnisse zurück. Entwickler können ihre Filter (z. B. ausschließliche Suche nach JPG-Dateien) so flexibel mit der Abfrage des Benutzers (Dateiname, der mit „bird“ beginnt) kombinieren. 

## <a name="supplemental-properties"></a>Zusatzeigenschaften 

Zusatzeigenschaften verhalten sich identisch zu regulären Windows-Eigenschaften, aber es gibt einen wichtigen Unterschied: Sie werden nicht geschrieben, wenn die Datei dem Indexer hinzugefügt wird. Eine Zusatzeigenschaft muss später von einer anderen App des Systems hinzugefügt werden. Dies kann zwei Minuten später durchgeführt werden, nachdem die Objekterkennung abgeschlossen ist, oder es kann einige Tage später erfolgen. 

Nachdem die Eigenschaft geschrieben wurde, kann sie wie alle anderen Eigenschaften des Systems durchsucht, gefiltert, sortiert oder gruppiert werden. Darüber hinaus kann sie in kombinierten Abfragen mit anderen Eigenschaften des Systems verwendet werden (Zusatz- oder reguläre Eigenschaften). Hierdurch kannst du Zusatzeigenschaften flexibel mit deinem vorhandenen Dateisystemcode kombinieren, ohne ihn umschreiben zu müssen.  

### <a name="example-scenarios"></a>Beispielszenarien 

Es gibt Tausende von unterschiedlichen Eigenschaften, die du in eine Zusatzeigenschaft schreiben kannst. In diesem Tutorial wird aber immer wieder auf zwei Hauptszenarien verwiesen:  

#### <a name="tagging-pictures-with-extracted-properties"></a>Kennzeichnen von Bildern mit extrahierten Eigenschaften 
Für diese Apps kann ein per ML trainiertes Modell genutzt werden, um Merkmale aus einem Bild zu extrahieren, die dem System nicht bekannt sind (z. B. ein Objekt im Bild). Die im Bild identifizierten Objekte können dann dem Eigenschaftensystem zum späteren Durchsuchen oder Gruppieren hinzugefügt werden.  

#### <a name="tagging-files-with-an-app-specific-id"></a>Kennzeichnen von Dateien mit einer App-spezifischen ID 
Viele Apps für die Dateisynchronisierung nutzen eine eigene eindeutige ID, um Dateien nachzuverfolgen, während sie zwischen dem Server und den verschiedenen Clientgeräten verschoben werden. Der Synchronisierungsclient kann diese ID in das Eigenschaftensystem schreiben, ohne dass sich dies auf die Datei auswirkt. Diese ID ist dann später für die App verfügbar, um den schnellen Zugriff zu ermöglichen. Außerdem kann sie auch von allen anderen Apps des Systems gelesen werden, wenn diese mit dem Synchronisierungsanbieter kommunizieren. 

Es gibt noch viele andere Optionen zur Nutzung von Zusatzeigenschaften. Dies sind aber zwei gute Beispiele, da sie eine schnelle Suche erfordern, für das System unbekannte Informationen darstellen und nicht der Datei selbst hinzugefügt werden können.  

### <a name="using-supplemental-properties"></a>Verwenden von Zusatzeigenschaften 
Die Verwendung der Zusatzeigenschaften entspricht dem Schreiben einer regulären Eigenschaft in das Dateisystem. Falls du dich mit der Nutzung von StorageFiles und Eigenschaften auskennst, kannst du diesen Abschnitt überspringen. Sieh dir andernfalls unten das kurze Beispiel für das Schreiben einer einzelnen Eigenschaft in eine Datei und das spätere Auslesen derselben Eigenschaft an.  

### <a name="writing-supplemental-properties"></a>Schreiben von Zusatzeigenschaften  
Im Beispiel wird der Einfachheit halber nur die erste gefundene Datei geändert, aber im Allgemeinen fügt eine App die Eigenschaft jeder gefundenen Datei hinzu.  

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

Es wird eine wichtige Prüfung durchgeführt, wenn der Speicherort indiziert ist, bevor eine Eigenschaft geschrieben wird. In diesem Beispiel verwenden wir die Abfrageoptionen, um nur nach indizierten Speicherorten zu filtern. Falls dies nicht möglich ist, kannst du den Indizierungszustand des übergeordneten Ordners überprüfen (file.GetParentAsync().GetIndexedStateAsync()). In beiden Fällen erhältst du die gleichen Ergebnisse. 

### <a name="reading-supplemental-properties"></a>Lesen von Zusatzeigenschaften 
Auch hier entspricht das Lesen einer Zusatzeigenschaft wieder dem Lesen aller anderen Dateisystemeigenschaften. In diesem Beispiel liest die App nur eine Eigenschaft aus einer Datei, für die sie bereits über eine StorageFile verfügt, aber sie kann bei Bedarf auch gleichzeitig andere Eigenschaften lesen.  

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
Du kannst anhand einer Überprüfung sicherstellen, dass der Wert, den du vom Eigenschaftensystem erhältst, dem erwarteten Wert entspricht. Die Wahrscheinlichkeit ist zwar gering, aber es kann sein, dass der Wert gelöscht wurde, seitdem er von deiner App geschrieben wurde. Dies wird weiter unten ausführlich beschrieben.  

### <a name="implementation-notes"></a>Hinweise zur Implementierung 
Am Design der Zusatzeigenschaften wurden einige geringfügige Änderungen vorgenommen. Als Hilfe bei der Implementierung wurden die folgenden Abschnitte aus den Engineering-Designspezifikationen für das Feature kopiert. Sie enthalten Informationen zum Entwurf des Features und Angaben zu den Gründen für die Einschränkungen. 

### <a name="supplemental-properties-available"></a>Verfügbare Zusatzeigenschaften 
Ursprünglich sind nur zwei Eigenschaften vorhanden, die für Apps verwendet werden können: „System.Supplemental.ResourceId“ und „System.Supplemental.AlbumID“. Falls weitere Eigenschaften benötigt werden, können sie hinzugefügt werden. Die Album-ID ist eine Zeichenfolge mit mehreren Werten, die für viele unterschiedliche Anwendungen verwendet werden kann, und die ResourceId wird als eindeutige ID für Cloudsynchronisierungsanbieter verwendet. 

#### <a name="file-system-support"></a>Dateisystemunterstützung 
Da Wechselmedien mit FAT-Formatierung ein wichtiges Szenario darstellen, werden für Zusatzeigenschaften FAT- und NTFS-Laufwerke unterstützt. Hierdurch wird sichergestellt, dass die Zusatzeigenschaften unabhängig vom Gerätetyp für alle Benutzer verfügbar sind.   

### <a name="non-indexed-locations"></a>Nicht indizierte Orte  
Auf dem Desktop gibt es einige Ordner, die nicht indiziert werden. In diesen Fällen kann es trotzdem sein, dass Apps Zugriff auf Zusatzeigenschaften benötigen. Zusatzeigenschaften sind außerhalb von indizierten Speicherorten aber nicht verfügbar. Diese Kompromisslösung hat verschiedene Gründe:  

- Alle Bibliotheken und Cloudspeicherorte werden standardmäßig indiziert.   
  Dies sind die Speicherorte, die von UWP-Apps hauptsächlich genutzt werden. Es gibt auch andere Speicherorte, die nicht indiziert sind (System- oder Netzlaufwerke), aber diese werden weniger häufig zum Speichern von Benutzerdaten verwendet. 

- Beim Entwurf der WinRT-API-Oberfläche wird vorausgesetzt, dass der Indexer fast immer verfügbar ist.  
  Der Indexer steht also bereits an den meisten Speicherorten zur Verfügung, die für Apps interessant sind. Wenn sich herausstellt, dass Benutzer Daten an nicht indizierten Speicherorten speichern, besteht die einfachste Lösung darin, den jeweiligen Speicherort dem Index hinzuzufügen. Dies führt dazu, dass die Zusatzeigenschaften funktionieren, die Enumeration schneller durchgeführt wird und der Speicherort von Apps nachverfolgt werden kann.

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>Lesen oder Schreiben von Zusatzeigenschaften aus einer Datei an einem nicht indizierten Speicherort 
Falls eine App versucht, eine Zusatzeigenschaft an einen derzeit nicht indizierten Speicherort zu schreiben, löst der API-Aufruf eine Ausnahme aus. Dies ist die gleiche Ausnahme, die beim Versuch ausgelöst wird, das System.Music.AlbumArtist-Element für eine DOCX-Datei zu aktualisieren (Invalid Args).  
 
### <a name="change-notifications"></a>Änderungsbenachrichtigungen:  
Die UWP-Änderungsbenachrichtigungen und -Änderungsnachverfolgung funktionieren für die Zusatzeigenschaften weiterhin genauso wie für die Standardeigenschaften. So können Apps, die Daten bereitstellen, alle Änderungen nachverfolgen, die für eine ihrer Apps vorgenommen werden. 
  
### <a name="invalidating-properties"></a>Ungültigmachen von Eigenschaften:  
Die Zusatzeigenschaften einer Datei gelten unter Umständen als veraltet, wenn eine Datei geändert oder auf dem System verschoben wird. Apps, die den Pushvorgang für die Daten durchführen, verfügen über die Informationen darüber, ob die Daten gültig sind oder aktualisiert werden müssen. Vom System werden daher nur die Tools bereitgestellt, mit denen die Apps den entsprechenden Vorgang selbst ermitteln können.  
 
Falls die Datei geändert, aber nicht verschoben oder umbenannt wird, bleiben alle Zusatzeigenschaften der Datei unverändert. Die Apps können sich für Änderungsbenachrichtigungen über die vorhandene API-Oberfläche registrieren und die Eigenschaften je nach Bedarf aktualisieren. 
 
Wenn die Datei verschoben wird, werden die Eigenschaften für ungültig erklärt. Die App empfängt die Änderungsbenachrichtigungen für das Löschen, Erstellen, Umbenennen oder Verschieben. Dies hängt davon ab, wie der Vorgang genau durchgeführt wird. Nachdem die App die Änderungsbenachrichtigung empfangen hat, kann sie die Datei untersuchen und die Zusatzeigenschaften der Datei je nach Bedarf aktualisieren. 
 
### <a name="indexer-rebuilds"></a>Neuerstellungen des Indexers  
Von Zeit zu Zeit muss der Systemindex aus verschiedenen Gründen neu erstellt werden: das Eigenschaftsschema kann sich geändert haben, der Benutzer hat ggf. den Unternehmensdatenschutz aktiviert oder die Datenbankdatei ist einfach beschädigt. In diesen Fällen werden die Zusatzeigenschaften nicht beibehalten. Wir haben den Versuch unternommen, die Zusatzeigenschaften bei der Neuerstellung des Index beizubehalten. Dies wurde aber durch einige wichtige Aspekte verhindert:  

### <a name="protecting-the-data"></a>Schützen der Daten 
Falls die Datenbankdatei beschädigt ist – entweder aufgrund von Datenträgerfehlern oder nicht autorisierter Software –, ist es unmöglich, die in dieser Datei gespeicherten Daten zu schützen. Sie müssen an einem anderen Ort im System gespeichert oder von den restlichen Daten der Datenbank isoliert werden. 

Da wir bereits viel Arbeit investieren, um die Anfälligkeit des Index für Beschädigungen zu reduzieren, sollte sich die Anzahl von Vorfällen entsprechend verringern.  
Beibehalten der Zuordnung von Dateien zu den zugehörigen Metadaten bei Neuerstellungen 

Die Daten können vom Index während einer Neuerstellung zwar geschützt werden, aber es kann nicht ermittelt werden, ob sich die Datei während der Neuerstellung des Index geändert hat. Die Daten der Datei, die vom Index geschützt werden, sind ggf. nicht mehr gültig, wenn die Datei geändert oder verschoben wird.  
Verhalten 

Bei einer Neuerstellung des Indexers gehen alle Zusatzdaten verloren. Die Apps sind dafür verantwortlich, die Daten wieder in den Indexer einzufügen, die während der Neuerstellung verloren gegangen sind. Dies ist eine zusätzliche Belastung für die Apps, die aber als angemessen angesehen wird, da diese immer den Überblick über den Masterstatus ihrer gesamten Daten behalten müssen.  

### <a name="recovering"></a>Wiederherstellung 
Nachdem die Apps erkannt haben, dass der Index neu erstellt wird, sind sie für die Aktualisierung der Zusatzeigenschaften nach eigenem Ermessen verantwortlich.  
### <a name="privacy"></a>Vertraulichkeit 
Für einige Eigenschaften, die ggf. in die Dateien geschrieben werden, kann die Anforderung gelten, dass Benutzer die gemeinsame Verwendung mit anderen Anwendungen nicht wünschen. Apps sollten darauf hinweisen können, dass die in die Eigenschaften geschriebenen Informationen entweder für ihre Anwendungen privat, nur für einige andere Anwendungen freigegeben oder für jede App des Systems öffentlich zugänglich sind.  

Auch wenn dies für einige „Early Adopters“ ggf. ein interessantes Feature ist, so sind sie doch auch der Meinung, dass öffentliche Eigenschaften für den Entwurf trotzdem sehr nützlich wären. Aus diesem Grund ist dieses Feature als optional gekennzeichnet. Wir arbeiten weiter an seiner Entwicklung, aber ohne Unterstützung zum Ausblenden der Werte bei Bedarf. Wenn das Feature später hinzugefügt wird, ergeben sich noch weitere Szenarien, und es wird wichtig sein, die Nutzung für alle Entwürfe zu erwägen.  

## <a name="conclusions"></a>Schlussfolgerung 
Das ist alles. Zusatzeigenschaften stellen eine einfache Möglichkeit zum Speichern von mehr Dateieigenschaften im System dar. Die Nutzung ist natürlich optional, aber du kannst deiner App hiermit einen Vorteil gegenüber anderen Apps verschaffen, mit denen Daten nicht so schnell sortiert und durchsucht werden können. 

Wir freuen uns auf den Einsatz dieser Eigenschaften in Apps. Falls du Fragen zur Verwendung des Headers hast, kannst du unten einen Kommentar eingeben. 
