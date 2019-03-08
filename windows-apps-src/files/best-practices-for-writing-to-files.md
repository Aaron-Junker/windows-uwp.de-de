---
title: Bewährte Methoden zum Schreiben in Dateien
description: Erlernen Sie bewährte Methoden für die Verwendung von verschiedenen Methoden der Klassen FileIO und PathIO Datei aus.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f8bed97e060015f92ff95c9f7d797bbcb83db431
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605835"
---
# <a name="best-practices-for-writing-to-files"></a>Bewährte Methoden zum Schreiben in Dateien

**Wichtige APIs**

* [**FileIO-Klasse**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO-Klasse**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Entwickler, die gelegentlich in einen Satz von allgemeinen Problemen führen, bei Verwendung der **schreiben** Methoden der [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) Klassen für Datei e/a-Vorgänge. Beispielsweise enthalten häufig auftretende Probleme:

• Eine Datei teilweise geschrieben, dass • die app eine Ausnahme empfängt, wenn eine der Methoden aufrufen. • Die Vorgänge hinter sich lassen. TMP-Dateien mit einem Dateinamen, die den Zieldateinamen ähnelt.

Die **schreiben** Methoden der [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) Klassen umfassen Folgendes:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Dieser Artikel enthält Details zur Funktionsweise dieser Methoden so Entwickler besser wann und wie deren Verwendung verstehen. Dieser Artikel enthält Richtlinien und wird nicht versucht, eine Lösung für alle möglichen Datei-e/a-Probleme bereitzustellen. 

> [!NOTE]
> Dieser Artikel konzentriert sich auf die **FileIO** Methoden in den Beispielen und Diskussionen. Allerdings die **PathIO** Methoden folgen einem ähnlichen Muster und der Großteil der Anleitung in diesem Artikel gilt für diese Methoden zu. 

## <a name="conveience-vs-control"></a>Conveience im Vergleich zu Steuerelement

Ein [ **"storagefile"** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) Objekt ist kein Dateihandle, wie das systemeigene Win32-Programmiermodell. Stattdessen eine [ **"storagefile"** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) ist eine Darstellung einer Datei mit den Methoden, um den Inhalt zu bearbeiten.

Dieses Konzept zu verstehen ist nützlich, beim Durchführen von e/as mit einem **"storagefile"**. Z. B. die [Schreiben in eine Datei](quickstart-reading-and-writing-files.md#writing-to-a-file) Abschnitt werden drei Möglichkeiten, die in eine Datei schreiben:

* Mithilfe der [ **FileIO.WriteTextAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) Methode.
* Durch Erstellen eines Puffers und dem anschließenden Aufrufen der [ **FileIO.WriteBufferAsync** ](https://docs.microsoft.com/en-us/uwp/api/windows.storage.fileio.writebufferasync) Methode.
* Das Modell für die vier Schritten mithilfe eines Streams:
  1. [Open](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) Datei, die einen Datenstrom zu erhalten.
  2. [Erste](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) einen Ausgabestream.
  3. Erstellen Sie eine [ **DataWriter** ](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) Objekt aus, und rufen Sie die entsprechende **schreiben** Methode.
  4. [Commit](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) die Daten in den Datenschreiber und [leeren](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) den Ausgabestream.

Die ersten beiden Szenarios sind die am häufigsten verwendeten Apps. Schreiben in die Datei in einem einzigen Vorgang ist einfacher zu programmieren und pflegen, und es entfernt auch die Verantwortung für die app über den Umgang mit den Großteil der Komplexität der Datei-e/a. Aber diese Vereinfachung ist mit Nachteilen verbunden: der Verlust der Kontrolle über den gesamten Vorgang sowie die Möglichkeit zum Abfangen von Fehlern an bestimmten Punkten.

## <a name="the-transactional-model"></a>Das transaktionale Modell

Die **schreiben** Methoden der [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) Klassen zu umschließen die Schritte auf der dritten Schreibvorgang Modell mit einer hinzugefügten oben beschriebenen. Diese Ebene wird in eine Speichertransaktion gekapselt.

Um die Integrität der ursprünglichen Datei zu schützen, falls etwas schief, beim Schreiben von Daten geht, die **schreiben** Methoden verwenden ein Modell für Transaktions-, öffnen Sie die Datei mit [ **OpenTransactedWriteAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync). Dieser Vorgang erstellt eine [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) Objekt. Nach diesem Transaktionsobjekt erstellt wurde, Schreiben Sie die APIs die Daten, die ähnlich wie folgt die [Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) Beispieldaten oder das Codebeispiel in die [ **StorageStreamTransaction** ](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) Artikel.

Das folgende Diagramm veranschaulicht den zugrunde liegenden Tasks ausgeführt werden, durch die die **WriteTextAsync** -Methode in einer erfolgreichen Schreibvorgang. Die folgende Abbildung bietet eine vereinfachte Ansicht des Vorgangs. Beispielsweise wird in verschiedenen Threads Schritte aus, z. B. das Text-Codierung "und" Async-abschließen übersprungen.

![UWP-API-Aufruf Sequenzdiagramm zum Schreiben in eine Datei](images/file-write-call-sequence.svg)

Die Vorteile der Verwendung der **schreiben** Methoden der [ **FileIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [ **PathIO** ](https://docs.microsoft.com/uwp/api/windows.storage.pathio) stattdessen die Klassen das komplexere vier Schritten-Modell mithilfe eines Datenstroms sind:

* Ein API-Aufruf, alle Zwischenschritte, aus, einschließlich Fehler zu behandeln.
* Die ursprüngliche Datei bleibt, wenn ein Fehler auftritt.
* Der Systemstatus versucht, möglichst sauber beibehalten werden.

Mit so vielen möglichen intermediate Fehlerquellen ist es jedoch eine höhere Wahrscheinlichkeit eines Ausfalls. Tritt ein Fehler ist es möglicherweise schwierig zu verstehen, in dem der Vorgang fehlgeschlagen ist. Die folgenden Abschnitte enthalten einige der Fehler auftreten, wenn Sie mit der **schreiben** Methoden und bieten mögliche Lösungen.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Allgemeine Fehlercodes für Write-Methoden der Klassen FileIO und PathIO

Diese Tabelle enthält allgemeine Fehlercodes, die app-Entwickler auftreten, wenn Sie verwenden die **schreiben** Methoden. Die Schritte in der Tabelle entsprechen den Schritten in der vorherigen Abbildung.

|  Fehlernamen (Wert)  |  Schritte  |  Ursachen  |  Lösungen  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  Die ursprüngliche Datei kann zum Löschen, möglicherweise aus einem vorherigen Vorgang markiert werden.  |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  Die ursprüngliche Datei wird von einem anderen exklusiven Schreibzugriff geöffnet.   |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  Die ursprüngliche Datei (file.txt) konnte nicht ersetzt werden, da er verwendet wird. Einem anderen Prozess oder Vorgang wurde Zugriff auf die Datei, bevor es ersetzt werden kann.  |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  Transaktive Modell wird eine zusätzliche Datei erstellt, und diese zusätzlichen Speicherplatz beansprucht.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Dies kann aufgrund von mehrere ausstehende e/a-Vorgänge oder großen Dateien geschehen.  |  Durch das Steuern von Streams ein präziser Ansatz möglicherweise den Fehler zu beheben.  |
|  E_FAIL (0x80004005) |  Beliebig  |  Sonstiges  |  Wiederholen Sie den Vorgang. Wenn es weiterhin fehlschlägt, ist es möglicherweise eine Platform-Fehler, und die app beendet werden soll, da es in einem inkonsistenten Zustand ist. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Weitere Überlegungen für Datei-Zustände, die zu Fehlern führen kann

Abgesehen von zurückgegebenen Fehler von der **schreiben** Methoden, hier sind einige Richtlinien auf, was eine app, beim Schreiben in eine Datei erwarten kann.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Daten wurde in die Datei geschrieben, wenn es abgeschlossen wurde

Ihre app sollten keine vermutungen über Daten in der Datei vornehmen, während ein Schreibvorgang ausgeführt wird. Versuch, den Zugriff auf die Datei, bevor ein Vorgang abgeschlossen ist, kann zu inkonsistenten Daten führen. Ihre app sollte der nachverfolgung von ausstehenden e/as verantwortlich sein.

### <a name="readers"></a>Leser

Wenn die Datei geschrieben wird auch von einem Netzwerkschnittstellenbibliothek Reader verwendet wird (d. h. mit geöffnet [ **FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode), nachfolgende Lesevorgänge werden beim Auftreten eines Fehlers ERROR_OPLOCK_HANDLE_CLOSED (0x80070323). Manchmal apps wiederholen Sie die Datei zum Lesen und erneut öffnen. die **schreiben** Vorgang wird ausgeführt. Dadurch kann eine Racebedingung auf dem die **schreiben** letztendlich versagt, wenn Sie versuchen, auf die ursprüngliche Datei überschrieben werden, da er nicht ersetzt werden kann.

### <a name="files-from-knownfolders"></a>Dateien aus KnownFolders

Ihre app möglicherweise nicht die einzige app, die versucht, eine Datei zuzugreifen, die auf einem der befinden die [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Es gibt keine Garantie, dass wenn der Vorgang erfolgreich ist, den Inhalt, die eine app auf die Datei geschrieben habe, konstant bleibt das nächste Mal versucht wird, die Datei zu lesen. Freigeben oder Zugriff verweigert darüber hinaus werden in diesem Szenario häufiger Fehler auf.

### <a name="conflicting-io"></a>In Konflikt stehende e/a

Die Wahrscheinlichkeit von Parallelitätsfehlern können verringert werden, wenn unsere app verwendet die **schreiben** Methoden für die Dateien in die lokalen Daten, aber Vorsicht ist weiterhin erforderlich. Wenn mehrere **schreiben** Vorgänge gesendet werden gleichzeitig in der Datei, es gibt keine Garantie dafür, welche Daten in der Datei endet. Um dies zu vermeiden, wird empfohlen, dass Ihre app serialisiert **schreiben** Vorgänge in der Datei.

### <a name="tmp-files"></a>~ TMP-Dateien

In einigen Fällen, wenn das der Vorgang abgebrochen wurde (z. B., wenn die app wurde angehalten oder wird, vom Betriebssystem beendet), die Transaktion kein Commit oder ordnungsgemäß geschlossen. Dadurch lassen sich hinter der Dateien mit einem (. ~ TMP) Erweiterung. Erwägen Sie diese temporären Dateien zu löschen (Wenn sie in der app lokale Daten vorhanden sind) bei der Verarbeitung der app-Aktivierung.

## <a name="considerations-based-on-file-types"></a>Überlegungen zur basierend auf Dateitypen

Einige Fehler können je nach Art der Dateien, die Häufigkeit an, auf denen sie Zugriff auf, und die Dateigröße weiter verbreitet werden. Es gibt im Allgemeinen drei Kategorien von Dateien aus, die Ihre app zugreifen kann:

* Dateien erstellt und bearbeitet werden, indem der Benutzer in Ihrer app lokale Datenordner. Diese werden erstellt und bearbeitet, die nur bei der Verwendung Ihrer app, und sie nur innerhalb der app vorhanden sind.
* App-Metadaten. Die app verwendet diese Dateien zum Nachverfolgen seinen eigenen Status.
* Andere Dateien an Speicherorten File System, in dem Ihre app Funktionen, die Zugriff auf deklariert wurde. Diese befinden sich am häufigsten in eines der [ **KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Ihrer app hat vollständige Kontrolle über die ersten beiden Kategorien von Dateien, da sie Teil der app-Paketdateien und ausschließlich von Ihrer app zugegriffen werden. Für Dateien in der letzten Kategorie muss Ihrer app bewusst sein, dass andere apps und die Betriebssystemdienste Dateien gleichzeitig zugreifen können.

Abhängig von der app kann Zugriff auf die Dateien nach Häufigkeit abweichen:

* Sehr niedrig. In der Regel sind dies Dateien, die geöffnet werden, sobald die app startet und Sie werden beim Speichern, wenn die app angehalten wird.
* Niedrig. Hierbei handelt es sich um Dateien, die der Benutzer speziell eine Aktion (z. behandelt wird b. speichern oder laden).
* Mittel oder hoch. Hierbei handelt es sich um Dateien, die in denen die app Daten (z. B. Autosave-Funktionen oder Konstanten Metadaten nachverfolgen) ständig aktualisieren muss.

Dateigröße, erwägen Sie die Leistungsdaten in der folgenden Tabelle sind die **WriteBytesAsync** Methode. In diesem Diagramm vergleicht die Zeit für einen Vorgang Vs-Dateigröße, über eine durchschnittliche Leistung von 10.000 Vorgänge pro Dateigröße in einer kontrollierten Umgebung.

![WriteBytesAsync-Leistung](images/writebytesasync-performance.png)

Die Time-Werten auf der y-Achse werden aus dem Diagramm absichtlich ausgelassen, da unterschiedliche Hardware und Konfigurationen verschiedene absolute Zeitwerte ergeben. Allerdings haben wir diese Trends konsistent bei unseren Tests untersucht:

* Für sehr kleine Dateien (< = 1 MB): Die Zeit zum Durchführen der Vorgänge ist konsistent schnelle.
* Für größere Dateien (> 1 MB): Die Zeit zum Abschluss der Vorgänge startet exponentiell steigen.

## <a name="io-during-app-suspension"></a>E/a während der Unterbrechung der app

Ihre app muss entworfen, Unterbrechung zu verarbeiten, wenn zum Beibehalten von Zustandsinformationen oder Metadaten für die Verwendung in späteren Sitzungen werden sollen. Hintergrundinformationen zum Anhalten der app, finden Sie unter [App-Lebenszyklus](../launch-resume/app-lifecycle.md) und [in diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

Wenn das Betriebssystem erweiterten Ausführung zu Ihrer app gewährt, wenn Ihre app angehalten wird sie 5 Sekunden, um alle ihre Ressourcen freizugeben, und speichern Sie ihre Daten hat. Treten Sie für optimale Zuverlässigkeit und Benutzer auf, wird immer davon ausgehen Sie, dass die Zeit, die Sie anhalten Aufgaben behandeln müssen, beschränkt ist. Bedenken Sie die folgenden Richtlinien während der 5-Minuten-Zeitraum für die Behandlung von Unterbrechung Aufgaben:

* Versuchen Sie es zu e/a auf ein Minimum zu leeren und Release-Vorgänge zurückzuführen Racebedingungen zu vermeiden.
* Vermeiden Sie das Schreiben von Dateien, die mehrere hundert Millisekunden oder mehr schreiben zu müssen.
* Wenn Ihre app verwendet die **schreiben** Methoden sollten Sie Bedenken alle Zwischenschritte, aus, die diese Methoden erfordern.

Wenn Ihre app auf eine kleine Menge von Daten während der Unterbrechung ausgeführt wird, in den meisten Fällen können Sie mithilfe der **schreiben** Methoden, die Daten zu leeren. Wenn Ihre app eine große Menge von Daten verwendet, sollten Sie jedoch Datenströme, um die Daten direkt zu speichern. Dies kann helfen, die vom transaktionalen Modell der Verzögerung zu reduzieren die **schreiben** Methoden. 

Ein Beispiel finden Sie unter den [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) Beispiel.

## <a name="other-examples-and-resources"></a>Weitere Beispiele und Ressourcen

Hier sind einige Beispiele für und andere Ressourcen für bestimmte Szenarien.

### <a name="code-example-for-retrying-file-io-example"></a>Codebeispiel für die Wiederholung von e/a-Beispiel

Im folgenden finden eine Pseudocode für die Vorgehensweise beim Wiederholen eines Schreibvorgangs (C#), sofern der Write ausgeführt werden, nachdem der Benutzer eine Datei zum Speichern ausgewählt werden:

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>Synchronisieren des Zugriffs auf die Datei

Die [zur parallelen Programmierung mit .NET Blog](https://blogs.msdn.microsoft.com/pfxteam/) ist eine großartige Quelle für Anleitungen zur parallelen Programmierung. Insbesondere die [Posten Sie über AsyncReaderWriterLock](https://blogs.msdn.microsoft.com/pfxteam/2012/02/12/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) beschreibt, wie Sie exklusiven Zugriff auf eine Datei als Schreibvorgängen, während gleichzeitig der Lesezugriff ermöglichen zu gewährleisten. Bedenken Sie, dass das Serialisieren, die sich auf die e/a auswirken Leistung.

## <a name="see-also"></a>Siehe auch

* [Erstellen, schreiben und Lesen einer Datei](quickstart-reading-and-writing-files.md)
