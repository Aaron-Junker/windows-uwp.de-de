---
title: Verwenden von Streaming-.net-traceprocessing
description: In diesem Tutorial erfahren Sie, wie Sie sofort mit dem Streaming auf Ablauf Verfolgungs Daten zugreifen und weniger Arbeitsspeicher verwenden.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 6ad0f5977ed4d739ce3133c9e67c0eefc6e0cbd9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168824"
---
# <a name="use-streaming-with-traceprocessor"></a>Streaming mit traceprocessor verwenden

Standardmäßig greift traceprocessor auf Daten zu, indem Sie in den Arbeitsspeicher geladen werden, während die Ablauf Verfolgung verarbeitet wird. Dieser Puffer Ansatz ist einfach zu verwenden, kann jedoch in Bezug auf die Speicherauslastung teuer sein.

Traceprocessor bietet auch eine Ablauf Verfolgung. Usestreaming (), das den Zugriff auf mehrere Typen von Ablauf Verfolgungs Daten in streamingweise unterstützt (Verarbeitung von Daten beim Lesen aus der Ablauf Verfolgungs Datei, anstatt diese Daten im Arbeitsspeicher zu puffern). Beispielsweise kann eine syscalls-Ablauf Verfolgung recht groß sein, und das Puffern der gesamten Liste der syscalls in einer Ablauf Verfolgung kann sehr kostspielig sein.

## <a name="accessing-buffered-data"></a>Zugreifen auf gepufferte Daten

Der folgende Code zeigt den Zugriff auf syscall-Daten auf der normalen, gepufferten Weise über die Ablauf Verfolgung. "US-Systeme ()":

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>Zugreifen auf Streamingdaten

Bei einer großen syscalls-Ablauf Verfolgung kann der Versuch, die syscall-Daten im Arbeitsspeicher zu puffern, sehr kostspielig sein, oder er ist möglicherweise gar nicht möglich. Der folgende Code zeigt, wie Sie auf die gleichen syscall-Daten zugreifen können, indem Sie die Ablauf Verfolgung ersetzen. "Mit der Ablauf Verfolgung". Usestreaming (). "US-Systeme ()":

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>Funktionsweise des Streamings

Standardmäßig werden alle Streamingdaten während des ersten Durchlaufs der Ablauf Verfolgung bereitgestellt, und gepufferte Daten aus anderen Quellen sind nicht verfügbar. Im obigen Beispiel wird gezeigt, wie Streaming mit Pufferung kombiniert wird – Thread Daten werden vor dem Streamen von syscall-Daten gepuffert. Folglich muss die Ablauf Verfolgung zweimal gelesen werden – einmal, um gepufferte Thread Daten zu erhalten, und ein zweites Mal, um mit den gepufferten Thread Daten auf das Streaming von syscall-Daten zuzugreifen. Um Streaming und Pufferung auf diese Weise zu kombinieren, übergibt das Beispiel consumerschedule. secondpass an Trace. Usestreaming (). "US-Systeme" () bewirkt, dass die syscall-Verarbeitung in einem zweiten Durchlauf durch die Ablauf Verfolgung erfolgt. Durch die Ausführung in einem zweiten Durchlauf kann der syscall-Rückruf von der Ablauf Verfolgung auf das ausstehende Ergebnis zugreifen. Usethreads () bei der Verarbeitung der einzelnen syscall-Vorgänge. Ohne dieses optionale Argument hätte syscall Streaming im ersten Durchlauf der Ablauf Verfolgung ausgeführt werden müssen (es wäre nur ein Durchlauf vorhanden) und das ausstehende Ergebnis aus der Ablauf Verfolgung. Usethreads () sind noch nicht verfügbar. In diesem Fall hätte der Rückruf weiterhin Zugriff auf die ThreadID von syscall, aber er hätte keinen Zugriff auf den Prozess für den Thread (da der Thread zum Verarbeiten von Verknüpfungs Daten über andere Ereignisse bereitgestellt wird, die möglicherweise noch nicht verarbeitet wurden).

Einige wesentliche Unterschiede bei der Verwendung von Pufferung und Streaming:

1. Bei der Pufferung wird ein [ipdingresult &lt; T &gt; ](/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)zurückgegeben, und das Ergebnis, das es enthält, ist nur verfügbar, bevor die Ablauf Verfolgung verarbeitet wurde. Nachdem die Ablauf Verfolgung verarbeitet wurde, können die Ergebnisse mithilfe von Techniken wie foreach und LINQ aufgezählt werden.
2. Streaming gibt void zurück und nimmt stattdessen ein Rückruf Argument an. Der Rückruf wird einmal aufgerufen, sobald jedes Element verfügbar ist. Da die Daten nicht gepuffert werden, gibt es keine Liste der Ergebnisse, die mit foreach oder LINQ aufgezählt werden müssen – der streamingabrückruf muss den zu speichernden Teil der Daten Puffern, nachdem die Verarbeitung abgeschlossen ist.
3. Der Code für die Verarbeitung von gepufferten Daten wird nach dem Aufrufen von Trace angezeigt. Process (), wenn die ausstehenden Ergebnisse verfügbar sind.
4. Der Code für die Verarbeitung von Streamingdaten wird vor dem aufzurufenden Trace angezeigt. Process () als Rückruf für die Ablauf Verfolgung. Usestreaming. use... ()-Methode.
5. Ein streamingconsumer kann auswählen, dass nur ein Teil des Streams verarbeitet und zukünftige Rückrufe durch Aufrufen des Kontexts abgebrochen werden. Abbrechen (). Einem pufferungsconsumer wird immer eine vollständige, gepufferte Liste bereitgestellt.

## <a name="correlated-streaming-data"></a>Korrelierte Streamingdaten

Manchmal werden Ablauf Verfolgungs Daten in einer Sequenz von Ereignissen angezeigt – beispielsweise werden syscalls über separate Enter-und Exit-Ereignisse protokolliert, aber die kombinierten Daten beider Ereignisse können hilfreicher sein. Die Ablauf Verfolgungs Methode der Methode. Usestreaming (). Mit "US-Dateien ()" werden die Daten beider Ereignisse korreliert und bereitgestellt, da das Paar verfügbar wird. Einige Arten von korrelierten Daten sind über die Ablauf Verfolgung verfügbar. Usestreaming ():

| Code                                        | BESCHREIBUNG                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Ablauf Verfolgungs. Usestreaming (). Usecontexenwitchdata () | Streamt korrelierte Kontextwechsel Daten (von kompakten und nicht kompakten Ereignissen mit genaueren switchinthreadids als unkomprimierte unkomprimierte Ereignisse). |
| Ablauf Verfolgungs. Usestreaming (). Usescheduledtasks ()    | Streamt korrelierte geplante Aufgaben Daten.                                                                                                         |
| Ablauf Verfolgungs. Usestreaming (). "US-Systeme" ()          | Streamt korrelierte System Aufrufdaten.                                                                                                            |
| Ablauf Verfolgungs. Usestreaming (). Usewindowinfocus ()     | Streamt korrelierte Fenster im Fokus.                                                                                                        |

## <a name="standalone-streaming-events"></a>Eigenständige streamingereignisse

Führen Sie außerdem die Ablauf Verfolgung aus Usestreaming () stellt analysierte Ereignisse für eine Reihe von verschiedenen eigenständigen Ereignis Typen bereit:

| Code                                               | BESCHREIBUNG                                     |
|----------------------------------------------------|-------------------------------------------------|
| Ablauf Verfolgungs. Usestreaming (). Uselastbranchrecorentvents ()   | Datenströme wurden als LBR-Ereignisse (Last Branch Record) analysiert. |
| Ablauf Verfolgungs. Usestreaming (). Usereadythreadevents ()        | Datenströme wurden für fertige Thread Ereignisse analysiert.             |
| Ablauf Verfolgungs. Usestreaming (). Usethreadkreateevents ()       | Datenströme analysierte Thread Erstellungs Ereignisse.            |
| Ablauf Verfolgungs. Usestreaming (). Usethreadexitevents ()         | In Streams analysierte Ereignisse zum Beenden von Threads.              |
| Ablauf Verfolgungs. Usestreaming (). Usethreadrundownstartevents () | Streams analysierte Thread-Rundown-Start Ereignisse.     |
| Ablauf Verfolgungs. Usestreaming (). Usethreadrundownstopevents ()  | Vom Stream analysierte Thread-rundown-Ereignisse.      |
| Ablauf Verfolgungs. Usestreaming (). Usethreadsetnameevents ()      | Streamt analysierte Thread Satz-namens Ereignisse.          |

## <a name="underlying-streaming-events-for-correlated-data"></a>Zugrunde liegende streamingereignisse für korrelierte Daten

Und schließlich Trace. Usestreaming () stellt auch die zugrunde liegenden Ereignisse bereit, die zum Korrelieren von Daten in der obigen Liste verwendet werden. Diese zugrunde liegenden Ereignisse lauten:

| Code                                                        | BESCHREIBUNG                                                                                | Teil von                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| Ablauf Verfolgungs. Usestreaming (). Usecompactcontexungswitchevents ()        | In Streams analysierte komprimierte Kontextwechsel Ereignisse.                                              | Ablauf Verfolgungs. Usestreaming (). Usecontexenwitchdata () |
| Ablauf Verfolgungs. Usestreaming (). Usecontexungswitchevents ()               | Von Streams analysierte Kontextwechsel Ereignisse. Switchinthreadids ist in einigen Fällen möglicherweise nicht korrekt. | Ablauf Verfolgungs. Usestreaming (). Usecontexenwitchdata () |
| Ablauf Verfolgungs. Usestreaming (). Usefocuschangeevents ()                 | Geänderte Änderungs Ereignisse im Fenster Fokus.                                                 | Ablauf Verfolgungs. Usestreaming (). Usewindowinfocus ()     |
| Ablauf Verfolgungs. Usestreaming (). Usescheduledtaskstartevents ()          | Stream analysierte geplante Start Ereignisse für Aufgaben.                                                | Ablauf Verfolgungs. Usestreaming (). Usescheduledtasks ()    |
| Ablauf Verfolgungs. Usestreaming (). Usescheduledtaskstopevents ()           | In Stream analysierte geplante Aufgaben zum Abbrechen von Ereignissen.                                                 | Ablauf Verfolgungs. Usestreaming (). Usescheduledtasks ()    |
| Ablauf Verfolgungs. Usestreaming (). Usescheduledtasktriggerevents ()        | In Streams analysierte Ereignisse für geplante Aufgaben Auslösers.                                              | Ablauf Verfolgungs. Usestreaming (). Usescheduledtasks ()    |
| Ablauf Verfolgungs. Usestreaming (). "US-essionlayertartactivewindowevents" () | Datenströme, die aktive Fenster Ereignisse auf Sitzungs Ebene festgelegt haben.                                     | Ablauf Verfolgungs. Usestreaming (). Usewindowinfocus ()     |
| Ablauf Verfolgungs. Usestreaming (). "", "".                | In Datenströme analysierte syscall Enter-Ereignisse.                                                       | Ablauf Verfolgungs. Usestreaming (). "US-Systeme" ()          |
| Ablauf Verfolgungs. Usestreaming (). "", "".                 | Datenströme analysierte syscall-Exit-Ereignisse.                                                        | Ablauf Verfolgungs. Usestreaming (). "US-Systeme" ()          |

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie sofort mit dem Streaming auf Ablauf Verfolgungs Daten zugreifen und weniger Arbeitsspeicher verwenden.

Der nächste Schritt besteht darin, auf die gewünschten Daten aus ihren Ablauf Verfolgungen zuzugreifen. Sehen Sie sich die [Beispiele](https://github.com/microsoft/eventtracing-processing-samples) für einige Ideen an. Beachten Sie, dass nicht alle Ablauf Verfolgungen alle unterstützten Datentypen enthalten.