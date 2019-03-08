---
title: Kopieren und Zugreifen auf Ressourcendaten
description: Nutzungskennzeichen geben an, wie die Anwendung Ressourcendaten verwendet, um die Ressourcen im leistungsfähigsten Speicherbereich zu platzieren. Ressourcendaten werden von allen Ressourcen kopiert, damit CPU oder GPU ohne eine Beeinträchtigung der Leistung darauf zugreifen können.
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords:
- Kopieren und Zugreifen auf Ressourcendaten
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3d4364bf9973b69587ae042a809d026b553ee2ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662315"
---
# <a name="copying-and-accessing-resource-data"></a>Kopieren und Zugreifen auf Ressourcendaten


Nutzungskennzeichen geben an, wie die Anwendung Ressourcendaten verwendet, um die Ressourcen im leistungsfähigsten Speicherbereich zu platzieren. Ressourcendaten werden von allen Ressourcen kopiert, damit CPU oder GPU ohne eine Beeinträchtigung der Leistung darauf zugreifen können.

Sie brauchen dabei weder zu berücksichtigen, ob die Ressourcen im Videospeicher oder im Systemspeicher erstellt werden, noch müssen Sie entscheiden, ob die Runtime den Speicher verwalten soll. Mit der Architektur von WDDM (Windows Display Driver Model) erstellen Apps Direct3D-Ressourcen mit verschiedenen Nutzungskennzeichen, um anzugeben, wie die Anwendung die Ressourcendaten verwendet. Das Treibermodell virtualisiert den von den Ressourcen verwendeten Speicher. Es liegt an der Verantwortung des Betriebssystem-/Treiber-/Speicher-Managers, die Ressourcen je nach erwarteter Verwendung in den leistungsfähigsten Speicherbereichen unterzubringen.

In der Standardeinstellung stehen Ressourcen der GPU zur Verfügung. Es gibt Fälle, bei denen die Ressourcendaten für die CPU verfügbar sein müssen. Das Kopieren von Ressourcendaten, damit der entsprechende Prozessor ohne Leistungsbeeinträchtigung darauf zugreifen kann, erfordert Kenntnisse zur Funktionsweise der API-Methode.

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>Kopieren von Ressourcendaten


Ressourcen werden im Speicher erstellt, wenn Direct3D einen Aufruf zum Erstellen ausführt. Sie können im Videospeicher, Systemspeicher oder jeder anderen Art von Speicher erstellt werden. Da das WDDM-Treiber-Modell diesen Speicher virtualisiert, müssen Apps nicht mehr nachverfolgen, in welcher Art von Speicher die Ressourcen erstellt werden.

Im Idealfall würden alle Ressourcen im Videospeicher aufbewahrt, damit die GPU direkt darauf zugreifen kann. Manchmal ist es allerdings für die CPU erforderlich, die Ressourcendaten zu lesen oder für die GPU, auf die Ressourcendaten zuzugreifen, die die CPU geschrieben hat. Direct3D verarbeitet diese verschiedenen Szenarien, indem es von der Anwendung die Angaben über die Verwendung anfordert und anschließend bei Bedarf mehrere Methoden zum Kopieren der Ressourcendaten anbietet.

Je nachdem, wie die Ressource erstellt wurde, ist es nicht immer möglich, direkt auf die zugrunde liegenden Daten zuzugreifen. Dies kann bedeuten, dass die Ressourcendaten von der Quell-Ressource zu einer anderen Ressource kopiert werden müssen, auf die vom entsprechenden Prozessor zugegriffen werden kann. Bei Direct3D kann auf die Standardressourcen direkt über die GPU zugegriffen werden, während auf dynamische und Staging-Ressourcen direkt über die CPU zugegriffen werden kann.

Nachdem eine Ressource erstellt wurde, kann dessen Verwendung nicht mehr geändert werden. Kopieren Sie stattdessen den Inhalt einer Ressource auf eine andere Ressource, die für eine andere Verwendung erstellt wurde. Ressourcendaten werden zwischen Ressourcen kopiert und Daten werden vom Speicher auf eine Ressource kopiert.

Es gibt zwei Haupttypen von Ressourcen: zuordbare und nicht zuordbare Ressourcen. Ressourcen, die mit dynamischer oder Staging-Verwendung erstellt wurden sind zuordbar, während Ressourcen, die mit Standard- oder unveränderlicher Verwendungen erstellt wurden, nicht zuordbar sind.

Das Kopieren von Daten auf nicht-zuordbare Ressourcen geht sehr schnell, da dies am häufigsten der Fall ist und optimiert wurde, um gute Ergebnisse zu liefern. Da die CPU nicht auf diese Ressourcen direkt zugreifen kann, werden diese optimiert, damit die GPU diese schnell ändern kann.

Das Kopieren von Daten zwischen zuordbaren Ressourcen ist schwieriger, da die Leistung von der Verwendung abhängt, für die die Ressource erstellt wurde. Die GPU kann z. B. eine dynamische Ressource relativ schnell lesen, aber nicht darauf schreiben, während die GPU auf Staging-Ressourcen weder direkt schreiben noch diese lesen kann.

Anwendungen, die Daten aus einer Ressource mit standardmäßiger Verwendung auf eine Ressource mit Staging-Verwendung kopieren möchten (damit die CPU die Daten lesen kann – ein sogenanntes GPU Readback Problem), müssen dabei vorsichtig sein. Weitere Informationen finden Sie unter [Zugreifen auf Ressourcendaten](#accessing) weiter unten.

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>Zugreifen auf Ressourcen


Der Zugriff auf eine Ressource erfordert die Zuordnung der Ressource. „Zuordnen” bedeutet, dass die Anwendung der CPU Zugriff auf den Speicher gewähren möchte. Der Prozess des Zuordnens einer Ressource, damit die CPU auf den zugrunde liegenden Speicher zugreifen kann, kann zu Leistungsengpässen führen. Aus diesem Grunde muss bei der Ausführung der Aufgabe besondere Vorsicht walten.

Die Leistung kann zum Stillstand kommen, wenn die Anwendung versucht, eine Ressource zur falschen Zeit zuzuordnen. Wenn die Anwendung versucht, Zugriff auf die Ergebnisse eines Vorgangs zu erhalten, bevor der Vorgang abgeschlossen ist, wird die Pipeline zum Stillstand gebracht.

Das Ausführen einer Zuordnung zur falschen Zeit kann möglicherweise zu schwerwiegenden Leistungseinbußen führen, da die GPU und die CPU zur Synchronisierung gezwungen werden. Diese Synchronisierung erfolgt, wenn die Anwendung auf eine Ressource zugreift, bevor die GPU den Kopiervorgang in eine Ressource beendet hat, die der CPU zugeordnet werden kann.

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Überlegungen zur Leistung

Sehen Sie den PC am besten als einen Computer, der eine parallele Architektur mit zwei Haupttypen von Prozessoren ausführt: eine oder mehrere CPUs und eine oder mehrere GPUs. Wie bei jeder parallelen Architektur wird die beste Leistung dann erzielt, wenn genügend Aufgaben für jeden Prozessor geplant sind, damit kein Leerlauf entsteht und wenn die Arbeit eines Prozessors nicht auf die Arbeit des anderen wartet.

Beim GPU/CPU-Parallelismus wird schlimmstenfalls ein Prozessor gezwungen, auf die Ergebnisse der Arbeit eines anderen zu warten. Dies ist bei Direct3D nicht der Fall, da die Kopiermethode asynchron ausgeführt wird. Die Kopie muss nicht unbedingt zu dem Zeitpunkt ausgeführt werden, wenn die Methode zurückgegeben wird.

Der Vorteil ist, dass die Anwendung erst dann Leistungseinbußen durch den eigentlichen Datenkopiervorgang erfährt, wenn die CPU beim tatsächlichen Zuordnungsaufruf auf die Daten zugreift. Wenn die Zuordnungsmethode nach dem eigentlichen Datenkopiervorgang aufgerufen wird, hat dies keine Leistungseinbußen zur Folge. Wenn die Zuordnungsmethode allerdings vor dem eigentlichen Datenkopiervorgang aufgerufen wird, wird die Pipeline zum Stillstand gebracht.

Asynchrone Aufrufe in Direct3D (die den Großteil der Methoden ausmachen, insbesondere Rendering-Aufrufe) werden im sogenannten *Befehlspuffer* gespeichert. Dieser Puffer ist ein internes Element des Grafiktreibers und wird zur Verarbeitung von Aufrufen der zugrunde liegenden Hardware verwendet, damit der aufwendige Wechseln vom Benutzermodus zum Kernel-Modus in Microsoft so selten wie möglich auftritt.

Der Befehlspuffer wird geleert und verursacht so einen Benutzer/Kernel-Moduswechsel in einer der folgenden vier Situationen:

1.  „Gegenwärtig” wird aufgerufen.
2.  „Leerung” wird aufgerufen.
3.  Der Befehlspuffer ist voll, seine Größe ist dynamisch und wird durch das Betriebssystem und den Grafiktreiber festgelegt.
4.  Die CPU erfordert Zugriff auf die Ergebnisse eines Befehls, der im Befehlspuffer auf seine Ausführung wartet.

Von den oben genannten vier Situationen ist die letzte am kritischsten. Wenn die Anwendung einen Aufruf zum Kopieren einer Ressource oder Unterressource erteilt, wird der Aufruf im Befehlspuffer in die Warteschlange eingereiht.

Wenn die Anwendung dann versucht, die Staging-Ressource, die das Ziel des Kopieraufrufs war, vor dem Leeren des Befehlspuffers zuzuordnen, wird die Pipeline zum Stillstand gebracht, da nicht nur der Aufruf für die Kopiermethode ausgeführt werden muss, sondern alle anderen gepufferte Befehle im Befehlspuffer ebenfalls ausgeführt werden müssen. GPU und CPU werden dann synchronisiert, da die CPU darauf wartet, auf die Staging-Ressource zuzugreifen, während die GPU den Befehlspuffer leert und schließlich die Anforderungen der CPU-Ressourcen erfüllt. Nachdem die GPU den Kopiervorgang beendet, beginnt die CPU mit dem Zugriff auf die Staging-Ressourcen. Während dieser Zeit befindet sich die GPU allerdings im Leerlauf.

Wenn der Vorgang häufig während der Laufzeit ausgeführt wird, beeinträchtigt dies die Leistung erheblich. Aus diesem Grund sollte die Zuordnung von Ressourcen in Standardnutzung mit Vorsicht ausgeführt werden. Die Anwendung muss ausreichend lange warten, bis der Befehlspuffer geleert ist und daher alle Befehle abgeschlossen sind, bevor versucht wird, die entsprechenden Ressource zuzuordnen.

Wie lange sollte die Anwendung warten? Mindestens zwei Frames, da hierdurch der Parallelismus zwischen CPU und GPU maximal genutzt wird. So funktioniert die GPU: während die Anwendung Frame N durch die Übermittlung von Aufrufen an den Befehlspuffer verarbeitet, ist die GPU damit beschäftigt, Aufrufe aus dem vorherigen Frame, N-1, auszuführen.

Wenn eine Anwendung daher eine Ressource zuordnen möchte, die aus dem Videospeicher stammt und eine Ressource an Frame N kopiert, beginnt dieser Aufruf die Ausführung auf Frame N+1, wenn die Anwendung Aufrufe für den nächsten Frame übermittelt. Die Kopie sollten beendet werden, wenn die Anwendung Frame N+2 verarbeitet.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Frame</th>
<th align="left">GPU/CPU-Status</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>CPU erteilt Render-Aufrufe für den aktuellen Frame.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>GPU führt die von der CPU gesendete Aufrufe während Frame N aus.</li>
<li>CPU erteilt Render-Aufrufe für den aktuellen Frame.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>GPU beendet die Ausführung der von der CPU gesendeten Aufrufe während Frame N. Ergebnisse stehen bereit.</li>
<li>GPU führt von der CPU gesendete Aufrufe während Frame N+1 aus.</li>
<li>CPU erteilt Render-Aufrufe für den aktuellen Frame.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>GPU beendet die Ausführung der von der CPU gesendeten Aufrufe während Frame N+1. Ergebnisse stehen bereit.</li>
<li>GPU führt von der CPU gesendete Aufrufe während Frame N+2 aus.</li>
<li>CPU erteilt Render-Aufrufe für den aktuellen Frame.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ressourcen](resources.md)

 

 




