---
ms.assetid: F912161D-3767-4F35-88C0-E1ECDED692A2
title: Verbessern der Leistung bei der Garbage Collection
description: In C# und Visual Basic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NET Garbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NET Garbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fdb4e80d7f8da022e2ceb5496cbad592d7d22716
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339625"
---
# <a name="improve-garbage-collection-performance"></a>Verbessern der Leistung bei der Garbage Collection


In C# und Visual Basic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NET Garbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NET Garbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps. Weitere Informationen zur Funktionsweise des .NET Garbage Collectors sowie zu Debugging- und Leistungsanalysetools für den Garbage Collector finden Sie unter [Garbage Collection](https://docs.microsoft.com/dotnet/standard/garbage-collection/index).

**Beachten Sie**, dass   , der in das Standardverhalten des Garbage Collector eingreifen muss, stark auf allgemeine Speicherprobleme mit Ihrer APP hinweist. Weitere Informationen finden Sie unter [Speichernutzungstool beim Debuggen in Visual Studio 2015](https://devblogs.microsoft.com/devops/memory-usage-tool-while-debugging-in-visual-studio-2015/). Dieses Thema betrifft ausschließlich C# und Visual Basic.

 

Der Garbage Collector bestimmt den Moment der Ausführung, indem er den Speicherverbrauch des verwalteten Heaps mit dem Aufwand für die Garbage Collection vergleicht. Zur Ermittlung dieses Zeitpunkts unterteilt der Garbage Collector unter anderem den Heap in Generationen und erfasst größtenteils nur einen Teil des Heaps. Der verwaltete Heap enthält drei Generationen:

-   Generation 0. Diese Generation enthält neu zugeordnete Objekte mit einer Größe von 85 KB oder mehr. Ab dieser Größe gehören die Objekte dem Heap für große Objekte an. Der Heap für große Objekte wird im Rahmen der Generation 2 bereinigt. Bereinigungen der Generation 0 werden am häufigsten ausgeführt und bereinigen kurzlebige Objekte wie lokale Variablen.
-   Generation 1. Diese Generation enthält Objekte, die nach Generation 0 noch vorhanden sind. Sie hat die Funktion eines Puffers zwischen den Generationen 0 und 2. Bereinigungen der Generation 1 sind seltener als Bereinigungen der Generation 0 und bereinigen temporäre Objekte, die bei vorangegangenen Bereinigungen der Generation 0 aktiv waren. Eine Bereinigung der Generation 1 bereinigt auch die Generation 0.
-   Generation 2. Diese Generation enthält langlebige Objekte, die nach den Generationen 0 und 1 noch vorhanden sind. Collections der Generation 2 sind am seltensten und erfassen den gesamten verwalteten Heap – einschließlich des Heaps für große Objekte, der Objekte ab einer Größe von 85 KB enthält.

Zur Ermittlung der Leistung des Garbage Collectors können zwei Aspekte betrachtet werden: die zum Ausführen der Garbage Collection erforderliche Zeit und die Arbeitsspeichernutzung des verwalteten Heaps. Konzentrieren Sie sich bei einer kleinen App mit einer Heapgröße von weniger als 100 MB auf die Verringerung der Arbeitsspeichernutzung. Konzentrieren Sie sich nur dann auf die Verringerung der für die Garbage Collection benötigten Zeit, wenn der verwaltete Heap Ihrer App eine Größe von 100 MB übersteigt. Hier erfahren Sie, wie der.NET Garbage Collector eine bessere Leistung erzielen kann.

## <a name="reduce-memory-consumption"></a>Verringern der Arbeitsspeichernutzung

### <a name="release-references"></a>Versionsverweise

Ein Verweis auf ein Objekt in Ihrer App verhindert die Bereinigung dieses Objekts sowie aller Objekte, auf die es verweist. Der .NET-Compiler erkennt sehr zuverlässig, wann eine Variable nicht mehr verwendet wird, sodass Objekte, die von dieser Variablen gehalten werden, der Collection zugeführt werden können. Manchmal ist es jedoch möglicherweise nicht ohne Weiteres ersichtlich, dass Objekte auf andere Objekte verweisen, da sich ein Teil des Objektgraphs unter Umständen im Besitz von Bibliotheken befindet, die Ihre App verwendet. Informationen zu den Tools und Techniken, mit denen Sie ermitteln können, welche Objekte nach einer Garbage Collection noch vorhanden sind, finden Sie unter [Garbage Collection und Leistung](https://docs.microsoft.com/dotnet/standard/garbage-collection/performance).

### <a name="induce-a-garbage-collection-if-its-useful"></a>Auslösen einer Garbage Collection bei Bedarf

Lösen Sie eine Garbage Collection nur aus, nachdem Sie die Leistung Ihrer App ermittelt haben und zu dem Schluss gekommen sind, dass sich eine Bereinigung positiv auf das Ergebnis auswirkt.

Rufen Sie zum Auslösen der Garbage Collection einer Generation [**GC.Collect(n)** ](https://docs.microsoft.com/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) auf, wobei „n“ für die Generation steht, die Sie erfassen möchten (0, 1 oder 2).

**Hinweis**   Es wird empfohlen, dass Sie keine Garbage Collection in der APP erzwingen, da der Garbage Collector viele Heuristik verwendet, um den besten Zeitpunkt für die Durchführung einer Auflistung zu bestimmen. das Erzwingen einer Auflistung ist in vielen Fällen eine unnötige Verwendung der CPU. Falls Ihre App allerdings eine große Anzahl von Objekten enthält, die nicht mehr verwendet werden, und Sie den entsprechenden Arbeitsspeicher wieder für das System freigeben möchten, ist die Erzwingung einer Garbage Collection unter Umständen dennoch angemessen. So können Sie beispielsweise in einem Spiel eine Bereinigung am Ende einer Ladesequenz auslösen, um vor Spielbeginn Arbeitsspeicher freizugeben.
 
Damit Sie nicht versehentlich zu viele Garbage Collections auslösen, können Sie [**GCCollectionMode**](https://docs.microsoft.com/dotnet/api/system.gccollectionmode) auf **Optimized** festlegen. Dadurch wird der Garbage Collector angewiesen, nur dann eine Garbage Collection zu starten, wenn eine ausreichend hohe Produktivität dies rechtfertigt.

## <a name="reduce-garbage-collection-time"></a>Verringern der erforderlichen Zeit für die Garbage Collection

Dieser Abschnitt gilt, wenn Sie Ihre App analysiert und eine hohe Garbage Collection-Dauer festgestellt haben. Die Pausenzeiten für die Garbage Collection werden von folgenden Aspekten beeinflusst: der erforderlichen Zeit zum Ausführen eines einzelnen Garbage Collection-Durchlaufs und der Zeit, die die App insgesamt für Garbage Collections aufwendet. Die Dauer einer Bereinigung hängt davon ab, wie viele Livedaten der Collector analysieren muss. Die Größe der Generationen 0 und 1 ist zwar begrenzt, die Größe der Generation 2 nimmt jedoch mit steigender Anzahl aktiver langlebiger Objekte in Ihrer App immer weiter zu. Das bedeutet, dass auch die Bereinigungsdauer für die Generationen 0 und 1 begrenzt ist, während Collections der Generation 2 ggf. mehr Zeit in Anspruch nehmen. Da eine Garbage Collection Arbeitsspeicher freigibt, um Zuordnungsanforderungen zu befriedigen, hängt die Ausführungshäufigkeit von Garbage Collections in erster Linie davon ab, wie viel Arbeitsspeicher Sie zuordnen.

Der Garbage Collector hält Ihre App von Zeit zu Zeit an, um Arbeiten auszuführen. Dabei wird die App aber nicht zwingend für die gesamte Dauer der Bereinigung angehalten. Die Pausenzeiten sind für den Benutzer Ihrer App üblicherweise nicht wahrnehmbar. Dies gilt besonders bei Bereinigungen der Generationen 0 und 1. Die [Hintergrund-Garbage Collection](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals) des .NET Garbage Collectors ermöglicht die parallele Ausführung von Bereinigungen der Generation 2 während der App-Ausführung. Die App wird dabei jeweils nur ganz kurz angehalten. Bereinigungen der Generation 2 können allerdings nicht immer im Hintergrund ausgeführt werden. In diesem Fall ist die Pause unter Umständen für den Benutzer wahrnehmbar, sofern der Heap entsprechend groß (über 100 MB) ist.

Häufig ausgeführte Garbage Collections können eine höhere CPU-Auslastung (und damit einen höheren Energiebedarf) sowie längere Ladezeiten oder geringere Bildfrequenzen in Ihrer Anwendung zur Folge haben. Im Anschluss finden Sie einige Techniken, mit denen Sie die Dauer der Garbage Collection sowie Pausenzeiten für die Garbage Collection einer verwalteten UWP-App verringern können.

### <a name="reduce-memory-allocations"></a>Verringern von Arbeitsspeicherzuordnungen

Wenn Sie überhaupt keine Objekte zuordnen, wird der Garbage Collector nur ausgeführt, wenn im System wenig Arbeitsspeicher vorhanden ist. Eine Verringerung der zugeordneten Arbeitsspeichermenge führt unmittelbar zu einer selteneren Ausführung von Garbage Collections.

Sollten Pausen in bestimmten Bereichen Ihrer App absolut unerwünscht sein, können Sie die benötigten Objekte während einer weniger leistungskritischen Phase bereits vorab zuordnen. So kann ein Spiel beispielsweise alle zum Spielen benötigten Objekte zuordnen, während der Ladebildschirm eines Levels angezeigt wird, damit beim Spielen keine Zuordnungen mehr erforderlich sind. Dadurch werden Pausen während des Spielverlaufs vermieden und ggf. höhere und einheitlichere Bildfrequenzen erzielt.

### <a name="reduce-generation-2-collections-by-avoiding-objects-with-a-medium-length-lifetime"></a>Verringern von Bereinigungen der Generation 2 durch Vermeidung von Objekten mittlerer Lebensdauer

Generationsbasierte Garbage Collections funktionieren am besten, wenn Ihre App deutlich kurzlebige und/oder deutlich langlebige Objekte enthält. Kurzlebige Objekte werden im Rahmen der weniger aufwendigen Generationen 0 und 1 bereinigt, während Objekte mit langer Lebensdauer der seltener ausgeführten Bereinigung der Generation 2 vorbehalten sind. Langlebige Objekte werden während der gesamten Laufzeit Ihrer App (oder über einen längeren Zeitraum) verwendet – beispielsweise während der Anzeige einer bestimmten Seite oder während eines bestimmten Levels.

Wenn Sie häufig temporäre Objekte erstellen, die lange genug verwendet werden, um der Generation 2 zugeordnet zu werden, werden mehr der aufwendigeren Bereinigungen der Generation 2 ausgeführt. Die Anzahl von Collections der Generation 2 lässt sich gegebenenfalls durch Wiederverwenden vorhandener Objekte oder durch schnelleres Freigeben der Objekte verringern.

Ein gängiges Beispiel für Objekte mit mittlerer Lebensdauer sind Objekte, die zum Anzeigen von Elementen in einer Liste dienen, in der der Benutzer einen Bildlauf ausführt. Wenn Objekte erstellt werden, sobald die Elemente in der Liste in den sichtbaren Bereich rücken, und nicht mehr auf sie verwiesen wird, wenn die Elemente in der Liste den sichtbaren Bereich wieder verlassen, fällt in der App üblicherweise eine große Anzahl von Bereinigungen der Generation 2 an. In solchen Situationen können Sie für die Daten, die dem Benutzer aktiv angezeigt werden, einen Satz von Objekten vorab zuordnen und wiederverwenden. Dabei können Sie die Infos unter Verwendung kurzlebiger Objekte laden, wenn die Listenelemente in den sichtbaren Bereich rücken.

### <a name="reduce-generation-2-collections-by-avoiding-large-sized-objects-with-short-lifetimes"></a>Verringern von Bereinigungen der Generation 2 durch Vermeidung großer Objekte mit kurzer Lebensdauer

Wie bereits erwähnt, wird jedes Objekt ab einer Größe von 85 KB dem Heap für große Objekte (Large Object Heap, LOH) zugeordnet und im Rahmen der Generation 2 erfasst. Falls Sie über temporäre Variablen (beispielsweise Puffer) mit einer Größe von mehr als 85 KB verfügen, werden diese durch eine Bereinigung der Generation 2 bereinigt. Indem Sie die Größe der temporären Variablen auf unter 85 KB beschränken, können Sie in Ihrer App die Anzahl von Collections der Generation 2 verringern. Eine gängige Technik ist die Erstellung eines Pufferpools und die Wiederverwendung von Objekten aus dem Pool, um umfangreiche temporäre Zuordnungen zu vermeiden.

### <a name="avoid-reference-rich-objects"></a>Vermeiden von Objekten mit vielen Verweisen

Der Garbage Collector folgt den Verweisen zwischen Objekten, um zu ermitteln, bei welchen Objekten es sich um Liveobjekte handelt. Den Ausgangspunkt bildet dabei jeweils der Stamm in Ihrer App. Weitere Informationen finden Sie unter [Vorgänge während einer Garbage Collection](https://docs.microsoft.com/dotnet/standard/garbage-collection/fundamentals). Enthält ein Objekt eine Vielzahl von Verweisen, bedeutet dies einen höheren Aufwand für den Garbage Collector. Eine gängige Technik (besonders bei großen Objekten) ist das Konvertieren von Objekten mit vielen Verweisen in Objekte ohne Verweise (beispielsweise, indem anstelle eines Verweises ein Index gespeichert wird). Diese Technik funktioniert natürlich nur, wenn diese Vorgehensweise logisch möglich ist.

Das Ersetzen von Objektverweisen durch Indizes kann sich für Ihre App als komplizierte Änderung mit Störpotenzial erweisen und ist besonders für große Objekte mit einer Vielzahl von Verweisen gedacht. Führen Sie diesen Schritt nur aus, wenn Sie in Ihrer App eine hohe Garbage Collection-Dauer feststellen, die auf Objekte mit vielen Verweisen zurückzuführen ist.

 

 




