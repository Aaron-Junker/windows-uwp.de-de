---
title: Bewährte Methoden zum Schreiben in Dateien
description: Lernen Sie bewährte Methoden für verschiedene Dateischreibvorgänge mit den Klassen „FileIO“ und „PathIO“ kennen.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dcbeffc7e3db8f3df9c197e8c388f30faf7ad03d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685238"
---
# <a name="best-practices-for-writing-to-files"></a>Bewährte Methoden zum Schreiben in Dateien

**Wichtige APIs**

* [**FileIO-Klasse**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
* [**PathIO-Klasse**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

Bei Entwicklern kann manchmal eine Reihe allgemeiner Probleme auftreten, wenn sie E/A-Vorgänge im Dateisystem mithilfe der **Write**-Methoden der Klassen [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) ausführen. Dies sind beispielsweise häufig auftretende Probleme:

* Eine Datei wird nur teilweise geschrieben.
* Die App empfängt beim Aufrufen einer der Methoden eine Ausnahme.
* Bei den Vorgängen bleiben TMP-Dateien zurück, deren Name ähnlich wie der Name der Zieldatei lautet.

Die **Write**-Methoden der Klassen [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) umfassen Folgendes:

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 Der vorliegende Artikel enthält Details zur Funktionsweise dieser Methoden, damit Entwickler besser verstehen, wann und wie sie verwendet werden. Der Artikel enthält Richtlinien und versucht nicht, eine Lösung für alle möglichen Datei-E/A-Probleme bereitzustellen. 

> [!NOTE]
> Dieser Artikel konzentriert sich in Beispielen und Diskussionen auf die **FileIO**-Methoden. Die **PathIO**-Methoden folgen allerdings einem ähnlichen Muster, und die meisten Anleitungen im Artikel gelten auch für diese Methoden. 

## <a name="convenience-vs-control"></a>Komfort im Vergleich zu Kontrolle

Ein [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)-Objekt ist kein Filehandle wie das einheitliche Win32-Programmiermodell. Stattdessen ist eine [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) eine Darstellung einer Datei mit Methoden zur Bearbeitung von deren Inhalten.

Dieses Konzept zu verstehen, ist hilfreich bei der Ausführung von E/A mit einer **StorageFile**. So stellt beispielsweise der Abschnitt [Schreiben in eine Datei](quickstart-reading-and-writing-files.md#writing-to-a-file) drei Möglichkeiten zum Schreiben in eine Datei vor:

* Mithilfe der [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)-Methode.
* Durch das Erstellen eines Puffers mit anschließendem Aufrufen der [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writebufferasync)-Methode.
* Das Modell aus vier Schritten, bei dem ein Stream verwendet wird:
  1. [Öffnen](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync) Sie die Datei, um einen Stream abzurufen.
  2. [Rufen Sie](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) einen Ausgabestream ab.
  3. Erstellen Sie ein [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter)-Objekt, und rufen Sie die entsprechende **Write**-Methode auf.
  4. [Übernehmen](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync) Sie die Daten in den Datenschreiber, und [leeren](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync) Sie den Ausgabestream.

Die ersten beiden Szenarien werden von Apps am häufigsten verwendet. Das Schreiben in die Datei in einem einzigen Vorgang ist einfacher zum Codieren und Verwalten. Außerdem entfällt dadurch für die App die Verantwortung, viele der Komplexitäten von Datei-E/A verarbeiten zu müssen. Dieser Komfort ist allerdings mit Nachteilen verbunden: der Verlust der Kontrolle über den gesamten Vorgang und der Möglichkeit zum Abfangen von Fehlern an bestimmten Punkten.

## <a name="the-transactional-model"></a>Das transaktionale Modell

Die **Write**-Methoden der Klassen [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) umschließen die Schritte im vorstehenden dritten „Write“-Modell mit einer hinzugefügten Ebene. Diese Ebene wird in einer Speichertransaktion gekapselt.

Um die Integrität der ursprünglichen Datei für den Fall zu schützen, dass beim Schreiben der Daten ein Fehler auftritt, verwenden die **Write**-Methoden ein transaktionales Modell, indem die Datei mit [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync) geöffnet wird. Dieser Vorgang erstellt ein [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction)-Objekt. Nachdem dieses Transaktionsobjekt erstellt wurde, schreiben die APIs die Daten ähnlich wie im [File Access](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)-Beispiel oder wie im Codebeispiel im Artikel zu [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction).

Das folgende Diagramm veranschaulicht die zugrunde liegenden Aufgaben, die von der **WriteTextAsync**-Methode bei einem erfolgreichen Schreibvorgang ausgeführt werden. Diese Abbildung zeigt eine vereinfachte Ansicht des Vorgangs. So werden darin beispielsweise Schritte wie die Textcodierung und der asynchrone Abschluss bei verschiedenen Threads übersprungen.

![UWP-API-Aufruf-Sequenzdiagramm zum Schreiben in eine Datei](images/file-write-call-sequence.svg)

Die Verwendung der **Write**-Methoden für die Klassen [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) und [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) statt des komplexeren Modells aus vier Schritten bietet die folgenden Vorteile:

* Ein einziger API-Aufruf zum Verarbeiten aller Zwischenschritte, einschließlich Fehlern.
* Wenn ein Fehler auftritt, wird die ursprüngliche Datei beibehalten.
* Es wird versucht, den Systemstatus möglichst sauber beizubehalten.

Allerdings besteht bei so vielen möglichen zwischengeschalteten Fehlerquellen eine höhere Wahrscheinlichkeit eines Fehlers. Wenn ein Fehler auftritt, ist es möglicherweise schwer zu verstehen, wo der Prozess fehlgeschlagen ist. In den folgenden Abschnitten werden einige der Fehler beschrieben, die bei Verwendung der **Write**-Methoden auftreten können, und es werden mögliche Lösungen bereitgestellt.

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>Allgemeine Fehlercodes für „Write“-Methoden der Klassen „FileIO“ und „PathIO“

Diese Tabelle enthält allgemeine Fehlercodes, die bei App-Entwicklern auftreten können, wenn sie die **Write**-Methoden verwenden. Die Schritte in der Tabelle entsprechen den Schritten im vorstehenden Diagramm.

|  Fehlername (Wert)  |  Schritte  |  Ursachen  |  Lösungen  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  Die ursprüngliche Datei könnte zum Löschen markiert sein – möglicherweise aus einem vorherigen Vorgang.  |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, dass der Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  Die ursprüngliche Datei wird durch einen anderen exklusiven Schreibvorgang geöffnet.   |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, dass der Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19–20  |  Die ursprüngliche Datei (file.txt) konnte nicht ersetzt werden, weil sie verwendet wird. Ein anderer Prozess oder Vorgang hat auf die Datei zugreifen können, bevor sie ersetzt werden konnte.  |  Wiederholen Sie den Vorgang.</br>Stellen Sie sicher, dass der Zugriff auf die Datei synchronisiert wird.  |
|  ERROR_DISK_FULL (0x80070070)  |  7, 14, 16, 20  |  Das transaktive Modell erstellt eine zusätzliche Datei, die zusätzlichen Speicherplatz belegt.  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14, 16  |  Dies kann Infolge von mehreren ausstehenden E/A-Vorgängen oder großen Dateien geschehen.  |  Möglicherweise lässt sich der Fehler mit einem präziseren Ansatz durch Kontrolle des Streams lösen.  |
|  E_FAIL (0x80004005) |  Any  |  Verschiedenes  |  Wiederholen Sie den Vorgang. Wenn er weiterhin fehlschlägt, könnte ein Plattformfehler vorliegen, und die App sollte beendet werden, weil sie sich in einem inkonsistenten Zustand befindet. |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>Weitere Überlegungen zu Dateizuständen, die zu Fehlern führen können

Abgesehen von Fehlern, die von den **Write**-Methoden zurückgegeben werden, sind hier einige Richtlinien dazu aufgeführt, was eine App beim Schreiben in eine Datei erwarten kann.

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>Daten wurden in die Datei geschrieben, wenn (und nur wenn) der Vorgang abgeschlossen war

Ihre App sollte während eines laufenden Schreibvorgangs keine Annahmen zu Daten in der Datei treffen. Ein Zugriffsversuch auf die Datei vor Abschluss eines Vorgangs könnte zu inkonsistenten Daten führen. Ihre App sollte für die Nachverfolgung von ausstehenden Eingaben/Ausgaben zuständig sein.

### <a name="readers"></a>Leser

Wenn die Datei, in die geschrieben wird, auch von einem „polite“ Leser verwendet (d.h., mit [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode) geöffnet) wird, schlagen nachfolgende Lesevorgänge mit dem Fehler ERROR_OPLOCK_HANDLE_CLOSED (0x80070323) fehl. Manchmal wiederholen Apps während des laufenden **Schreibvorgangs** das Öffnen der Datei zum erneuten Lesen. Dies kann zu einer Racebedingung führen, bei der **Write** beim Versuch, die ursprüngliche Datei zu überschreiben, letztendlich fehlschlägt, weil die Datei nicht ersetzt werden kann.

### <a name="files-from-knownfolders"></a>Dateien aus „KnownFolders“

Ihre App ist möglicherweise nicht die einzige App, die versucht, auf eine Datei in einem der [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) zuzugreifen. Es gibt keine Garantie dafür, dass bei einem erfolgreichen Vorgang die Inhalte, die eine App in die Datei geschrieben hat, bei ihrem nächsten Leseversuch dieser Datei konstant bleiben. Außerdem treten in diesem Szenario Fehler des Typs „Freigabe oder Zugriff verweigert“ häufiger auf.

### <a name="conflicting-io"></a>In Konflikt stehende E/A

Die Wahrscheinlichkeit von Parallelitätsfehlern kann verringert werden, wenn Ihre App die **Write**-Methoden für Dateien in ihren lokalen Daten verwendet; Vorsicht ist aber weiterhin geboten. Wenn mehrere **Schreibvorgänge** gleichzeitig an die Datei gesendet werden, gibt es keine Garantie dafür, welche Daten in der Datei ankommen. Um dieses Problem zu entschärfen, wird empfohlen, dass Ihre App **Schreibvorgänge** in die Datei serialisiert.

### <a name="tmp-files"></a>~TMP-Dateien

Gelegentlich – wenn ein Vorgang erzwungenermaßen abgebrochen wurde (z.B. nachdem die App durch das Betriebssystem angehalten oder beendet wurde) – wird kein Commit für die Transaktion ausgeführt oder sie ordnungsgemäß geschlossen. Dadurch können Dateien mit der Erweiterung (.~TMP) zurückbleiben. Überlegen Sie bei der App-Aktivierung, ob diese temporären Dateien (sofern sie in den lokalen Daten der App vorhanden sind) gelöscht werden sollen.

## <a name="considerations-based-on-file-types"></a>Überlegungen, die auf Dateitypen basieren

Einige Fehler können je nach dem Typ der Dateien, der Häufigkeit des Zugriffs darauf und der Dateigröße häufiger auftreten. Im Allgemeinen gibt es drei Kategorien von Dateien, auf die Ihre App zugreifen kann:

* Dateien, die vom Benutzer im lokalen Datenordner Ihrer App erstellt und bearbeitet werden. Sie werden nur bei Verwendung Ihrer App erstellt und bearbeitet und sind nur innerhalb der App vorhanden.
* App-Metadaten. Ihre App verwendet diese Dateien zum Nachverfolgen ihres eigenen Status.
* Andere Dateien an Speicherorten des Dateisystems, an denen Ihre App über deklarierte Funktionen für den Zugriff verfügt. Sie befinden sich am häufigsten in einem der [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders).

Ihre App hat vollständige Kontrolle über die ersten beiden Kategorien von Dateien, weil sie Teil der App-Paketdateien sind und darauf ausschließlich von Ihrer App zugegriffen wird. Bei Dateien in der letzten Kategorie muss Ihre App beachten, dass andere Apps und Betriebssystemdienste möglicherweise gleichzeitig darauf zugreifen.

Je nach der App kann der Zugriff auf die Dateien nach Häufigkeit variieren:

* Sehr gering. Dies sind normalerweise Dateien, die beim Starten der App einmal geöffnet und beim Anhalten der App gespeichert werden.
* Gering. Dies sind Dateien, bei denen der Benutzer speziell eine Aktion ausführt (z.B. Speichern oder Laden).
* Mittel oder hoch. Dies sind Dateien, in denen die App Daten ständig aktualisieren muss (z.B. Funktionen zum automatischen Speichern oder ständige Nachverfolgung von Metadaten).

Sehen Sie sich im Hinblick auf die Dateigröße die Leistungsdaten im folgenden Diagramm zur **WriteBytesAsync**-Methode an. In diesem Diagramm wird die Zeit zur Ausführung eines Vorgangs mit der Dateigröße verglichen, und zwar über eine durchschnittliche Leistung von 10.000 Vorgängen pro Dateigröße in einer kontrollierten Umgebung.

![WriteBytesAsync-Leistung](images/writebytesasync-performance.png)

Die Zeitwerte auf der y-Achse wurden bei diesem Diagramm absichtlich weggelassen, weil verschiedene Hardware- und Softwarekonfigurationen zu verschiedenen absoluten Zeitwerten führen können. In unseren Tests haben wir jedoch folgende Trends ständig beobachtet:

* Bei sehr kleinen Dateien (<= 1 MB): Die Zeit zur Ausführung der Vorgänge ist durchgängig schnell.
* Bei größeren Dateien (> 1 MB): Die Zeit zur Ausführung der Vorgänge wird exponentiell beschleunigt.

## <a name="io-during-app-suspension"></a>E/A während des Anhaltens von Apps

Ihre App muss so konzipiert sein, dass sie ein Anhalten verarbeitet, wenn Sie Zustandsinformationen oder Metadaten zur Verwendung in späteren Sitzungen beibehalten möchten. Hintergrundinformationen zum Anhalten von Apps finden Sie unter [App-Lebenszyklus](../launch-resume/app-lifecycle.md) und in [diesem Blogbeitrag](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97).

Außer wenn das Betriebssystem eine erweiterte Ausführung Ihrer App gewährt, bleiben Ihrer angehaltenen App 5 Sekunden, um alle ihre Ressourcen freizugeben und ihre Daten zu speichern. Um eine optimale Zuverlässigkeit und Benutzererfahrung zu bieten, nehmen Sie immer an, dass Ihre verfügbare Zeit zur Verarbeitung von Anhalteaufgaben begrenzt ist. Denken Sie während des 5-Sekunden -Zeitraums für die Verarbeitung von Anhalteaufgaben an Folgendes:

* Versuchen Sie, E/A so kurz wie möglich zu halten, um Racebedingungen infolge von Leeren- und Freigabevorgängen zu vermeiden.
* Vermeiden Sie das Schreiben von Dateien, für die Hunderte von Millisekunden oder mehr erforderlich sind.
* Wenn Ihre App die **Write**-Methoden verwendet, denken Sie an alle Zwischenschritte, die bei diesen Methoden erforderlich sind.

Wenn Ihre App während des Anhaltens bei einer kleinen Menge von Statusdaten ausgeführt wird, können Sie die Daten in den meisten Fällen mit den **Write**-Methoden leeren. Wenn Ihre App aber eine große Menge von Statusdaten verwendet, erwägen Sie die Verwendung von Streams zum direkten Speichern Ihrer Daten. Dadurch lässt sich die Verzögerung verringern, die durch das transaktionale Modell der **Write**-Methoden eingeführt wurde. 

Ein Beispiel dazu finden Sie unter [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension).

## <a name="other-examples-and-resources"></a>Weitere Beispiele und Ressourcen

Hier sind einige Beispiele und weitere Ressourcen für bestimmte Szenarien.

### <a name="code-example-for-retrying-file-io-example"></a>Codebeispiel zum Vorgang „Wiederholung von Datei-E/A“

Das nachstehende Pseudocode-Beispiel zeigt, wie ein Schreibvorgang (C#) wiederholt wird. Dabei wird angenommen, dass der Schreibvorgang erfolgen muss, nachdem der Benutzer eine Datei zum Speichern ausgewählt hat:

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

Der [Blog „Parallel Programming with .NET“ (Parallele Programmierung mit .NET)](https://devblogs.microsoft.com/pfxteam/) ist eine hervorragende Ressource für Anleitungen zur parallelen Programmierung. Insbesondere wird im [Beitrag zu „AsyncReaderWriterLock“](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/) beschrieben, wie der exklusive Zugriff auf eine Datei für Schreibvorgänge erhalten bleibt, während der gleichzeitige Lesezugriff erlaubt wird. Denken Sie daran, dass sich die Serialisierung von E/A auf die Leistung auswirkt.

## <a name="see-also"></a>Siehe auch

* [Erstellen, Schreiben und Lesen einer Datei](quickstart-reading-and-writing-files.md)
