---
title: Verwalten von lokalem Speicher mit verbunden
description: Informationen Sie zum Verwalten von lokaler Speicher verbundene Daten in einer Entwicklungsumgebung befindet.
ms.assetid: 630cb5fc-5d48-4026-8d6c-3aa617d75b2e
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 09fce637c50b0a03230d0808e51a9e5cfadc7179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642055"
---
# <a name="managing-local-connected-storage"></a>Verwalten von lokalem Speicher mit verbunden
Während verbundenen Speicher zum Speichern Ihrer spielen Daten in der Cloud verwendet wird, ist auch eine lokaler Speicher-Komponente, der verbundene Speicherdienst. Ob Sie auf einem PC oder -Konsole sind gibt es ein lokaler Cache die verbundenen Daten mit den Daten in die Cloud synchronisiert. Ob einen Titel xdk-Version oder UWP erstellt wird, ist ein Tool, sodass Sie Ihre lokalen Speicher verbundene Daten verwalten können.

Finden Sie in der folgenden Tabelle das geeignete Tool zum Verwalten Ihrer lokal zwischengespeicherten Speicher verbundene Daten zu suchen:

|Titel-Klassifizierung  |Gerät  |Tool für den lokalen Speicher  |
|---------|---------|---------|
|XDK     |Xbox One-Konsole     |*xbstorage*         |
|UWP     |PC         |*gamesaveutil*         |
|UWP     |Xbox One-Konsole     |Xbox-Geräteportal (XDP |)

- *Xbstorage* ist ein Befehlszeilentool, über die Eingabeaufforderung xdk-Version, ausgeführt wird, für verbundene Speicher auf eine Xbox-Konsole verwalten lokal zwischengespeichert werden. Die *Xbstorage* Tool finden Sie in der Xbox One XDK unter dem Dateipfad:   **/Program Dateien (x86)/Microsoft Durango XDK/bin/xbstorage.exe**
- *Gamesaveutil* ist ein Befehlszeilentool, für verbundene Speicher auf PC Verwalten von UWP lokal zwischengespeichert werden. Die *Gamesaveutil* Tool wird zusammen mit den [Windows 10 SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) für den Fall Creators Update und höher (10.0.16299.15 erstellen und höher). Nachdem Sie die entsprechende Version von Windows 10-SDK installiert haben *Gamesaveutil* finden Sie unter dem Ordner: **ProgramFiles(x86)/Windows Kits/10/bin/[SDK Version]/x64/gamesaveutil.exe**.
- *Xbox-Gerät Portal(XDP)* ist ein online-Portal, das Ihnen ermöglicht, die lokal zwischengespeicherten verbundenen Speicher UWP-Daten auf der ein Xbox-Konsole verwalten. Dieser Artikel deckt sich nicht auf XDP Nutzung aus. Lesen Sie Informationen zur Verwendung von XDP [Device Portal für Xbox](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-xbox).

## <a name="xbstorage"></a>Xbstorage

*Xbstorage* ermöglicht das Löschen der lokal zwischengespeicherter Daten verbunden Storage Daten von der Festplatte, als auch importieren und Exportieren von Daten für Benutzer oder Computer aus verbundenen Speicherplätze mit XML-Dateien.

Wenn ein Vorgang ausgeführt wird, auf lokale Daten mithilfe der *Xbstorage* -Tool, das System wird verhält, als ob dieser Vorgang von der app selbst ausgeführt worden wären, daher das Lesen von Daten aus einer verbundenen Speicher in einer lokalen Datei führt Synchronisierung mit der Cloud vor dem Kopieren.

Auf ähnliche Weise wird eine Kopie der Daten aus einer XML-Datei auf dem Entwicklungs-PC in einer verbundenen Storage-Container auf der Xbox One Dev-Kit die beginnen mit dem Hochladen von Daten in die Cloud-Konsole führen. Es gibt jedoch Situationen, in dem dies nicht geschieht: Wenn das Dev-Kit die Sperre kann nicht abgerufen werden oder ist es ein Datenkonflikt zwischen den Containern in der Konsole und den in der Cloud, wird die-Konsole verhält, als ob der Benutzer nicht, zum Auflösen des Konflikts durch beschlossen hatten Auswählen einer Version des Containers behalten, und der-Konsole verhält, als ob der Benutzer offline bis zum nächsten wiedergegeben wird, wenn der Titel gestartet wird.

Die einzige Ausnahme dieser Befehle wird **zurücksetzen** **/force** die löscht des lokalen Speicher gespeicherten Daten für alle SCIDs und Benutzer, jedoch wird in der Cloud gespeicherten Daten nicht geändert. Dies ist nützlich für das Ablegen von einer Konsole in den Status, die, dem es in wäre, wenn ein Benutzer wurde einer Konsole roaming und Herunterladen von Daten aus der Cloud bei der Wiedergabe eines Titels.

### <a name="xbstorage-commands"></a>Xbstorage-Befehle

Xbstorage verfügt über die folgenden sechs Befehle können Entwickler über die Eingabeaufforderung xdk-Version zum Verwalten von lokaler Daten auf ihre Xbox eine Development Kit:

<a id="xbstorage_reset"></a>

|Befehl  |Beschreibung  |
|---------|---------|
|Zurücksetzen    |Führt eine Zurücksetzung auf dem Speicher verbunden.         |
|Importieren   |Importiert Daten aus der angegebenen XML-Datei in einem verbundenen Speicher.         |
|Exportieren   |Exportiert Daten aus einer verbundenen Speicher, in der angegebenen XML-Datei.         |
|löschen   |Löscht Daten aus einem verbundenen Speicherplatz.         |
|Generieren |Dummydaten generiert und speichert in der angegebenen XML-Datei.         |
|Simulieren |Simuliert den Speicherplatzmangel Speicher.         |

### <a name="xbstorage-reset"></a>Xbstorage zurücksetzen

`xbstorage reset [/force]`

Löscht alle lokale Daten in verbundenen Speicher über die lokale Konsole, die auf die werkseitigen Standardeinstellungen wiederherstellen. Daten, die in der Cloud beibehalten wurde nicht geändert und werden erneut heruntergeladen werden, nach Bedarf.

|Option  |Beschreibung  |
|---------|---------|
|/force   |Bestätigt, dass der Speicher angeschlossen zurückgesetzt werden soll. Wird des Befehls zum Zurücksetzen, ohne Angabe **/force** bewirkt, dass die folgende Meldung angezeigt werden:   Als Speicher verbundene Factory-Zurücksetzung wird ein potenziell destruktiver Vorgang, der mit diesem Befehl führt das Zurücksetzen nicht, es sei denn, die **/force** Flag vorhanden ist. |

<a id="xbstorage_import"></a>

### <a name="xbstorage-import"></a>Xbstorage importieren

`xbstorage import *file-name* [/scid:*SCID*] [/machine] [/msa:*account*] [/replace] [/verbose]`

Importieren von Daten, die im angegebenen *Filename* in einem verbundenen Speicher.
Die Datei ist eine XML-Datei, die die Daten enthält. Ein Beispiel finden Sie unter [Xbstorage generieren](#xbstorage_generate). Weitere Informationen zu der Datei XML-Format, finden Sie unter [-Import / Export-Dateiformat](#xbstorage_fileformat)weiter unten in diesem Thema.
Es gibt zwei Möglichkeiten zum Angeben des verbundenen Speicherplatzes:

- Wenn die Eingabedatei wurde eine **ContextDescription** -Abschnitt, der ordnungsgemäß aufgefüllt wurde, und klicken Sie dann diese an die verbundenen Speicherplatz verwendet wird,.
- Der tatsächlich belegte Speicherplatz kann auch teilweise oder vollständig über Befehlszeilenparameter anzugeben, die Vorrang vor der entsprechenden Elemente von der tatsächlich belegte Speicherplatz in der Eingabedatei angegeben haben.

Beispiele für die Verwendung:

```cmd
  xbstorage import mydata.xml
  xbstorage import mydata.xml /replace
  xbstorage import mydata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage import mydata.xml /verbose 
```

> [!NOTE]
> Vor dem Importieren in den angegebenen Speicher verbunden, versucht das System zum Synchronisieren mit der Cloud mit derselben Logik, die ausgeführt wird, wenn Speicherplatz verbunden, die von einer ausgeführten app abgerufen wird.
>
> Wenn eine Anwendung mit den gleichen primären SCID ausgeführt wird, wird dieser Vorgang kann dazu führen, dass eine Racebedingung, und den Inhalt des Speicherplatzes für verbunden ist möglicherweise in einem unbestimmten Zustand.
>
> Wenn **"und" ersetzen** nicht angegeben ist, werden die Container in der Eingabedatei angegeben vor dem Schreiben von Blobs in der Eingabedatei angegeben gelöscht. Container in der verbundenen Speicherplatz, der nicht in der Eingabedatei angegeben bleibt unverändert.

|Option  |Beschreibung  |
|---------|---------|
|*file-name*     |Gibt eine XML-Datei, die die zu importierenden Daten enthält.         |
|/scid:*SCID*    |Gibt den Bezeichner für Service-Konfiguration (SCID).         |
|/ machine        |Gibt einen verbundenen Speicherplatz pro Computer.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/msa** Option.         |
|/msa:*account*  |Gibt ein Konto für die verbundene Speicherung je Benutzer verwenden. Der Benutzer muss in der Konsole für den Speicherplatz zu verwendende angemeldet sein.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/machine** Option.         |
|/Replace        |Löscht alle Container in der angegebenen Verbindung Speicherplatz vor dem Importieren.         |
|/ verbose        |Zeigt den Status der anzuzeigen.         |


 <a id="xbstorage_export"></a>

### <a name="xbstorage-export"></a>Xbstorage exportieren

`xbstorage export *outfile* [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Exportiert Daten aus einem verbundenen Speicherplatz in der angegebenen Datei **Ausgabedatei**.    Die Datei ist eine XML-Datei, die die Daten enthält. Finden Sie unter [Xbstorage generieren](#xbstorage_generate) zu erfahren, wie Sie ein Beispiel zu generieren. Weitere Informationen zu der Datei XML-Format, finden Sie unter [-Import / Export-Dateiformat](#xbstorage_fileformat)weiter unten in diesem Thema. Es gibt zwei Möglichkeiten zum Angeben des verbundenen Speicherplatzes:

- Wenn die **/context** Parameter wird verwendet, und der Dateiname angegeben wird, indem \<Eingabedatei > verfügt über eine **ContextDescription** -Abschnitt, der ordnungsgemäß aufgefüllt wurde, und klicken Sie dann die Datei verwendet wird, um anzugeben, die Verbundene Speicherplatz.
- Der tatsächlich belegte Speicherplatz kann auch teilweise oder vollständig angegeben werden über Befehlszeilenparameter, die Vorrang vor der entsprechenden Elemente des Speicherplatzes, angegeben der **/context** Datei.

Beispiele für die Verwendung:

```cmd
  xbstorage export exporteddata.xml /context:space.xml
  xbstorage export exporteddata.xml /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage export exporteddata.xml /context:space.xml /verbose
```

> [!NOTE]
> Vor dem Exportieren des angegebenen Verbindung Speicherplatzes, versucht das System zum Synchronisieren mit der Cloud mit derselben Logik, die ausgeführt wird, wenn Speicherplatz verbunden, die von einer ausgeführten app abgerufen wird.
>
> Wenn eine Anwendung mit den gleichen primären SCID ausgeführt wird, wird dieser Vorgang kann dazu führen, dass eine Racebedingung, und den Inhalt des Speicherplatzes für verbunden ist möglicherweise in einem unbestimmten Zustand.

|Option  |Beschreibung  |
|---------|---------|
|*outfile*             |XML-Datei, die Daten werden in exportiert werden.         |
|/context:*input-file* |Gibt eine Eingabedatei aus dem die Speicherplatzinformationen gelesen.         |
|/scid:*SCID*          |Gibt den Bezeichner für Service-Konfiguration (SCID).         |
|/ machine              |Gibt einen verbundenen Speicherplatz pro Computer.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/msa** Option.         |
|/msa:*account*        |Gibt ein Konto für die verbundene Speicherung je Benutzer verwenden. Der Benutzer muss in der Konsole für den Speicherplatz zu verwendende angemeldet sein.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/machine** Option.         |
|/ verbose              |Zeigt den Status des Exportvorgangs.         |

<a id="xbstorage_delete"></a>

### <a name="xbstorage-delete"></a>Xbstorage löschen

`xbstorage delete [/context:*input-file*] [/scid:*SCID*] [/machine] [/msa:*account*] [/verbose]`

Löscht alle Daten aus einer verbundenen Speicher.
Es gibt zwei Möglichkeiten zum Angeben des verbundenen Speicherplatzes:

- Wenn die **/context** Parameter wird verwendet, und der Dateiname angegeben wird, indem \<Eingabedatei > verfügt über eine **ContextDescription** -Abschnitt, der ordnungsgemäß aufgefüllt wurde, und klicken Sie dann die Datei verwendet wird, um anzugeben, die Verbundene Speicherplatz.
- Der tatsächlich belegte Speicherplatz kann auch teilweise oder vollständig angegeben werden über Befehlszeilenparameter, die Vorrang vor der entsprechenden Elemente des Speicherplatzes, angegeben der **/context** Datei.

Beispiele für die Verwendung:

```cmd
  xbstorage delete /context:space.xml
  xbstorage delete /machine /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /msa:user@domain.com /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
  xbstorage delete /context:space.xml /verbose
```

> [!NOTE]
> Vor dem Löschen des angegebenen Verbindung Speicherplatzes, versucht das System zum Synchronisieren mit der Cloud mit derselben Logik, die ausgeführt wird, wenn Speicherplatz verbunden, die von einer ausgeführten app abgerufen wird.
>> Wenn eine Anwendung mit den gleichen primären SCID ausgeführt wird, wird dieser Vorgang kann dazu führen, dass eine Racebedingung, und den Inhalt des Speicherplatzes für verbunden ist möglicherweise in einem unbestimmten Zustand.

|Option  |Beschreibung |
|---------|---------|
|/context:*input-file*     |Gibt eine Eingabedatei aus dem die Speicherplatzinformationen gelesen.         |
|/scid:*SCID*              |Gibt den Bezeichner für Service-Konfiguration (SCID).         |
|/ machine                  |Gibt einen verbundenen Speicherplatz pro Computer.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/msa** Option.         |
|/msa:*account*            |Gibt ein Konto für die verbundene Speicherung je Benutzer verwenden. Der Benutzer muss in der Konsole für den Speicherplatz zu verwendende angemeldet sein.  Diese Option kann nicht gleichzeitig verwendet werden, mit der **/machine** Option.         |
|/ verbose                  |Zeigt den Status des Vorgangs zum Löschen.         |

 <a id="xbstorage_generate"></a>

### <a name="xbstorage-generate"></a>Xbstorage generieren

`xbstorage generate *file-name* [/containers:*n*] [/blobs:*n*] [/blobsize:*n*]`

Dummydaten generiert und speichert in einer Datei, die anhand des \<Dateiname >. Weitere Informationen zu der Datei XML-Format, finden Sie unter [-Import / Export-Dateiformat](#xbstorage_fileformat)weiter unten in diesem Thema.    Konfigurations-ID (SCID) des Diensts auf 00000000-0000-0000-0000-000000000000 festgelegt, und das Konto wird für einen verbundenen Speicherplatz pro Computer festgelegt werden. Wenn Sie diese Werte ändern möchten, können Sie die Datei direkt bearbeiten, nach der Erstellung.

Beispiele für die Verwendung:

```cmd
  xbstorage generate dummydata.xml
  xbstorage generate dummydata.xml /containers:4
  xbstorage generate dummydata.xml /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10
  xbstorage generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```

> [!NOTE]
> Die Bytedaten ist eine einfache aufsteigende Sequenz. Beispielsweise müsste ein fünf-Byte-Blob die folgenden Bytes: 00 01 02 03 04. >> Ändern, wenn Sie einen verbundenen Speicherplatz pro Benutzer angeben möchten, die **Konto** Knoten in der XML-Datei in etwa wie folgt:
>  ```
>    <Account msa="user@domain.com"/>
>  ```

|Option  |Beschreibung  |
|---------|---------|
|*file-name*     | XML-Datei, die Daten geschrieben werden soll. |
|/containers:*n* | Gibt die Anzahl, *n*, der Container zu generieren. Die Standardanzahl ist 2.  |
|/blobs:*n*      | Gibt die Anzahl, *n*, der Blobs zu generieren. Die Standardanzahl ist 3.  |
|/blobsize:*n*   | Gibt die Anzahl, *n*, Bytes pro Blob. Die Standardgröße ist 1024 Bytes.  |

 <a id="xbstorage_simulate"></a>

### <a name="xbstorage-simulate"></a>Xbstorage simulieren.

`xbstorage simulate [/reserveremainingspace] [/forceoutoflocalstorage] [/stop] [/verbose]`

Aus lokalem Speicher-Bedingungen für die der verbundene Speicherdienst simuliert.

|Option  |Beschreibung  |
|---------|---------|
|/reserveremainingspace | Reserviert die gesamten verbleibenden Platz im Speicher verbunden. Löschen etwas aus ConnectedStorage wird Speicherplatz geöffnet, die Sie verwenden können. |

| / Forceoutoflocalstorage | Simuliert den verbundenen Storage-Dienst müssen keine verfügbaren Speicherplatz nach links. Löschen etwas aus dem Speicher verbunden ändert den verbundene Speicherdienst sich nicht von der berichterstellung nicht genügend Arbeitsspeicher. |

| / stop | Beendet alle Simulationen. |

| / verbose | Zeigt den Status des simulierten-Vorgangs. |

 <a id="ID4E4MAC"></a>

### <a name="common-options"></a>Allgemeine Optionen

`xbstorage [/?] [/X*:address* [*+accesskey*] ]`

|Option  |Beschreibung  |
|---------|---------|
| /?                           |  Zeigt die Hilfe für xbstorage.exe |
| / X *: Adresse* [*+ Accesskey*]  | Gibt den Hostnamen oder die Adresse (dargestellt als **Tools IP** in der Konsole) einer Ziel-Konsole, aber nicht die Standardkonsole geändert wird. Wenn Sie diese Option nicht verwenden, ist die Standardkonsole verwendet. *Accesskey* ist eine Zeichenfolge, die Sie verwenden können, um den Zugriff auf eine Konsole auf die Personen zu beschränken, die den Zugriffsschlüssel zu kennen. Legen Sie den Zugriffsschlüssel, mit dem Befehl **Xbconfig** **Accesskey = *** your-Key*; wiederholen Sie die Konsole, um den Zugriffsschlüssel effektiv zu gestalten. Um eine Konsole zuzugreifen, die mit einem Zugriffsschlüssel konfiguriert ist, müssen Sie ein Pluszeichen (+) und den Zugriffsschlüssel nach dem IP-Adresse oder den Hostnamen die Namen der Konsole einschließen.
> [!NOTE]
> Wenn eine Zugriffstaste angegeben wird, wenn der Standardkonsole von Xbconnect festgelegt wird, wird dann als Teil der Adresse der Standardkonsole der Zugriffsschlüssel gespeichert.
|

## <a name="gamesaveutil"></a>Gamesaveutil

*Gamesaveutil* ermöglicht Ihnen das Verwalten von lokal zwischengespeicherten verbundenen Speicher für Ihre app mit allen identisch, die Funktionen *Xbstorage* enthält. Wie Sie das Tool Xbstorage *Gamesaveutil* bietet die gleichen sechs Daten, die Verwaltung von Funktionen, mit einigen Unterschieden im Verhalten. Löschen Sie der Import, Export, und Befehle zum Zurücksetzen erfordern einen Xbox Live-Benutzer angemeldet sein. Sie können die Xbox-App unter Windows 10 verwenden, anzeigen und ändern den aktuellen Benutzer.

### <a name="commands"></a>Befehle

|Befehl  |Beschreibung  |
|---------|---------|
|Importieren   |Importiert Daten aus der angegebenen XML-Datei         |
|Exportieren   |Exportiert Daten in der angegebenen XML-Datei         |
|löschen   |Löscht Daten aus der angegebenen app        |
|Zurücksetzen    |Löscht lokale Daten nur für die angegebene Anwendung         |
|Generieren |Dummydaten generiert und speichert die angegebenen XML-Datei         |
|Simulieren |simuliert die besondere Bedingungen, die schwierig zu testen sind.         |

### <a name="gamesaveutil-import"></a>Gamesaveutil importieren

`gamesaveutil import <filename> [/pfn:<PFN>] [/scid:<SCID>] [/replace]`

Importieren von Daten, die im angegebenen \<Dateiname >

Die Datei ist eine XML-Datei, die die Daten enthält. Typ `gamesaveutil help generate` zu erfahren, wie Sie ein Beispiel zu generieren.

Es gibt zwei Möglichkeiten zum Angeben der app PFN und SCID, in dem die Daten gespeichert ist:

Wenn die Eingabedatei weist den Abschnitt ContextDescription, die ordnungsgemäß aufgefüllt wird, wird dies verwendet werden, an die Ziel-app PFN und SCID.

Der PFN und SCID können teilweise oder vollständig über Befehlszeilenparameter angegeben werden die aus der Eingabedatei über die entsprechenden Elemente der angegebenen PFN und SCID Vorrang.

|Option  |Beschreibung  |
|---------|---------|
|/pfn:\<PFN >       |Gibt die Paket-Familie Name(PFN) für die app, um den Importvorgang für auszuführen. Die app muss installiert sein.         |
|/scid:\<SCID >     |Gibt den Bezeichner für Service-Konfiguration (SCID). Dies ist aus Ihrer Xbox Live-Konfiguration.         |
|/Replace         |Löschen Sie alle Container, vor dem Ausführen des Imports.         |

Beispielsyntax:

```cmd
gamesaveutil import mydata.xml
gamesaveutil import mydata.xml /replace
gamesaveutil import mydata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> Die app installiert werden und wurden synchronisiert Daten bereits um Sie zu importieren.
> 
> Wenn /replace nicht angegeben ist, werden vorhandener Container nicht abgefragt werden, es sei denn, sie in der Eingabedatei angegeben sind.

### <a name="gamesaveutil-export"></a>Gamesaveutil exportieren

`gamesaveutil export <outfile> [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Exportiert Daten in die angegebene Datei \<Ausgabedatei >.

Die Datei ist eine XML-Datei, die die Daten enthält. Geben Sie Gamesaveutil Help generieren wie generieren Sie ein Beispiel finden Sie unter.

Es gibt zwei Möglichkeiten zum Angeben des Speicherorts der Daten zu exportieren:

Wenn der /context-Parameter wird verwendet, und der Dateiname angegeben wird, indem \<Eingabedatei > weist den Abschnitt ContextDescription, die ordnungsgemäß aufgefüllt wird, und klicken Sie dann die Datei verwendet wird, um den Speicherort der Quelldaten angeben.

Der Speicherort kann auch über Befehlszeilenparameter angegeben werden, die Vorrang gegenüber der jeweiligen Elemente durch die /context-Datei angegeben.

|Option  |Beschreibung  |
|---------|---------|
|/context:\<infile>     |Geben Sie die app mithilfe der angegebene Datei PFN und SCID.         |
|/pfn:\<PFN >            |Gibt die Paket-Familie Name(PFN) für die app, um den Export aus auszuführen. Die app muss installiert sein.       |
|/scid:\<SCID >          |Gibt den Bezeichner für Service-Konfiguration (SCID). Dies ist aus Ihrer Xbox Live-Konfiguration.        |

Beispielsyntax:

```cmd
gamesaveutil export exporteddata.xml /context:target.xml
gamesaveutil export exporteddata.xml /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> Die app muss installiert sein, und Daten zum Exportieren von bereits synchronisiert zu haben.

### <a name="gamesaveutil-delete"></a>Gamesaveutil löschen

`gamesaveutil delete [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Löscht alle Daten für die angegebene PFN und SCID an.

Es gibt zwei Möglichkeiten, den Speicherort der zu löschenden Daten anzugeben:

Wenn der /context-Parameter wird verwendet, und der Dateiname angegeben wird, indem \<Eingabedatei > weist den Abschnitt ContextDescription, die ordnungsgemäß aufgefüllt wird, und klicken Sie dann die Datei verwendet wird, um den Speicherort der Quelldaten angeben.

Der Speicherort kann auch über Befehlszeilenparameter angegeben werden, die Vorrang gegenüber der jeweiligen Elemente durch die /context-Datei angegeben.

|Option  |Beschreibung  |
|---------|---------|
|/context:\<infile>     |Geben Sie die app mithilfe der angegebene Datei PFN und SCID.         |
|/pfn:\<PFN >            |Gibt die Paket-Familie Name(PFN) für die app zum Löschen von Daten aus. Die app muss installiert sein.       |
|/scid:\<SCID >          |Gibt den Bezeichner für Service-Konfiguration (SCID). Dies ist aus Ihrer Xbox Live-Konfiguration.        |

Beispielsyntax:

```cmd
gamesaveutil delete /context:target.xml
gamesaveutil delete /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> Die app muss installiert sein, um Container zu löschen.

### <a name="gamesaveutil-reset"></a>Gamesaveutil zurücksetzen

`gamesaveutil reset [/pfn:<PFN>] [/scid:<SCID>] [/context:<infile>]`

Löscht alle lokale Daten für die angegebene PFN und SCID an.

Es gibt zwei Möglichkeiten, den Speicherort der zu löschenden Daten anzugeben:

Wenn der /context-Parameter wird verwendet, und der Dateiname angegeben wird, indem \<Eingabedatei > weist den Abschnitt ContextDescription, die ordnungsgemäß aufgefüllt wird, und klicken Sie dann die Datei verwendet wird, um den Speicherort der Quelldaten angeben.

Der Speicherort kann auch über Befehlszeilenparameter angegeben werden, die Vorrang gegenüber der jeweiligen Elemente durch die /context-Datei angegeben.

|Option  |Beschreibung  |
|---------|---------|
|/context:\<infile>     |Geben Sie die app mithilfe der angegebene Datei PFN und SCID.         |
|/pfn:\<PFN >            |Gibt die Paket-Familie Name(PFN) für die app zum Löschen von Daten aus. Die app muss installiert sein.       |
|/scid:\<SCID >          |Gibt den Bezeichner für Service-Konfiguration (SCID). Dies ist aus Ihrer Xbox Live-Konfiguration.        |

Beispielsyntax:

```cmd
gamesaveutil reset /context:target.xml
gamesaveutil reset /pfn:MyGame_xxxxxx /scid:2AAEB34B-DAB2-4879-B625-D970069C1D22
```


> [!NOTE]
> Die app muss installiert sein, um lokale Daten zu löschen.

### <a name="gamesaveutil-generate"></a>Gamesaveutil generieren

`gamesaveutil generate <filename> [/containers:<n>] [/blobs:<n>] [/blobsize:<n>]`

Dummydaten generiert und speichert in einer Datei, die anhand des \<Dateiname >.
Der Service Configuration Bezeichner (SCID) wird auf 00000000-0000-0000-0000-000000000000 festgelegt werden. Bearbeiten Sie die Datei manuell nach Generierung diese Werte zu ändern, wenn Sie wünschen.

|Option  |Beschreibung  |
|---------|---------|
|/Containers:\<n >     |Gibt an, wie viele Container zu generieren. Standard ist 2.         |
|/blobs:\<n>          |Gibt an, wie viele Blobs pro Container zu generieren. Der Standardwert ist 3.        |
|/blobsize:\<n>       |Gibt an, wie viele Bytes pro Blob. Standardwert ist 1024.         |

Beispielsyntax:

```cmd
gamesaveutil generate dummydata.xml
gamesaveutil generate dummydata.xml /containers:4
gamesaveutil generate dummydata.xml /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10
gamesaveutil generate dummydata.xml /containers:4 /blobs:10 /blobsize:512
```


> [!NOTE]
> Die Bytedaten ist eine einfache aufsteigende Sequenz, d. h. müsste ein fünf-Byte-Blob die 00 01 02-03-04-Bytes.

### <a name="gamesaveutil-simulate"></a>Gamesaveutil simulieren.

`gamesaveutil simulate [/markcontainerschanged] [/stop]`

Simuliert die besondere Bedingungen, die für bestimmte Szenarien testen.

|Option  |Beschreibung  |
|---------|---------|
|/markcontainerschanged     |Erzwingt, dass alle Container zu sehen, wie sie eine App geändert wurden aus dem Anhalten wird fortgesetzt, und ruft einen neuen Anbieter. Wirkt sich auf alle apps, die erst mit/Stop deaktiviert.      |
|/stop                      |Beendet alle Simulationen.         |


 <a id="xbstorage_fileformat"></a>

## <a name="import-and-export-file-format"></a>-Import/export-Dateiformat

Die XML-Datei mit der **importieren**, **exportieren**, und **generieren** bei Befehlen mit der *Xbstorage* Tool hat das Format, die angezeigt wird, der folgende Beispiel.

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <XbConnectedStorageSpace>
    <ContextDescription>
      <Account msa="user@domain.com" />
      <Title scid="00000000-0000-0000-0000-000000000000" />
    </ContextDescription>
    <Data>
      <Containers>
        <Container name="Container1" displayName="Optional Display Name">
          <Blobs>
            <Blob name="Blob1">
              <![CDATA[... ] ]>
            </Blob>
            ...
            <Blob name="BlobN">
              <![CDATA[... ] ]>
            </Blob>
          </Blobs>
        </Container>
        ...
        <Container name="ContainerN">
        ...
        </Container>
      </Containers>
    </Data>
  </XbConnectedStorageSpace>
```

Die einzige Änderung, die zum Formatieren dieser XML-Code für **importieren**, **exportieren**, und **generieren** in *Gamesaveutil* ist das Ersetzen der \<Konto > Memberknoten des der \<ContextDescription >-Knoten mit einem \<PackageFamilyName > Knoten.
Dies ändert die \<ContextDescription > Knoten aus diesem:

```xml
<ContextDescription>
    <Account msa="user@domain.com" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

Folgendermaßen:

```xml
<ContextDescription>
    <PackageFamilyName pfn="MyGame_xxxxxx" />
    <Title scid="00000000-0000-0000-0000-000000000000" />
</ContextDescription>
```

> [!NOTE]
> Das Format der Daten in diesen XML-Dateien nicht, was auf der Plattform ist identisch, es ist für den Import und export nur zu. Das Datenformat für diese XML-Dateien könnten sich möglicherweise in Zukunft ändern, damit sie als eine mittlere oder fortgeschrittene Utility-Format, keine Archivformat behandelt werden soll.

Die **ContextDescription** -Knoten ist optional. Wenn Sie einen verbundenen Speicherplatz für einen Computer vornehmen, können Sie `<Account machine="true"/>` anstelle von `<Account msa="user@domain.com"/>`. Andernfalls kann der Kontext in der Befehlszeile für den Import angegeben werden.
BLOBs und-Container müssen die entsprechenden Namen für sie durch das Spiel oder eine Anwendung, die für die die Datei generiert wird.
Den Inhalt der einzelnen BLOBs muss eine Zeichenfolge, die innerhalb einer **CDATA** -Tag, das durch den Aufruf generiert wird **CryptBinaryToStringW** mit dem Flag **CRYPT_STRING_BASE64** die Daten bereitgestellt für dieses Blob als raw Byte-Array. Im Gegensatz dazu für die Blob-Daten konvertiert werden können, wieder durch Aufrufen von **CryptStringToBinary** und die zuvor verschlüsselte Zeichenfolge angeben. Ein Beispiel für diese beiden Funktionen wird angezeigt, [CryptBinaryToString gibt ungültige Bytes](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9acac904-c226-4ae0-9b7f-261993b9fda2/cryptbinarytostring-returns-invalid-bytes?forum=vcgeneral) in der MSDN-Foren für Visual Studio.

<a id="ID4EWBAE"></a>