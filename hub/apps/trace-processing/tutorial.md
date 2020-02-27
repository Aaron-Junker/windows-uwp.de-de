---
title: Access Trace Data-.net traceprocessing
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von traceprocessor auf Ablauf Verfolgungs Daten zugreifen.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 170a8c3084e180714a319d67dca2b6a5756ea474
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629111"
---
# <a name="access-trace-data"></a>Zugreifen auf Ablauf Verfolgungs Daten

.Net traceprocessing ist über [nuget](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) mit der folgenden Paket-ID verfügbar:

Microsoft. Windows. eventtracing. processing. all

Dieses Paket ermöglicht den Zugriff auf Daten in einer Ablauf Verfolgungs Datei. Wenn Sie noch nicht über eine Ablauf Verfolgungs Datei verfügen, können Sie diese mit [Windows Performance Recorder](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) erstellen.

Die folgende Beispiel Konsolen-APP zeigt, wie Sie auf die Befehlszeilen aller in der Ablauf Verfolgung enthaltenen Prozesse zugreifen können:

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using System;

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
            IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

            trace.Process();

            IProcessDataSource processData = pendingProcessData.Result;

            foreach (IProcess process in processData.Processes)
            {
                Console.WriteLine(process.CommandLine);
            }
        }
    }
}
```

## <a name="using-traceprocessor"></a>Verwenden von traceprocessor

Rufen Sie [traceprocessor. Create](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create)auf, um eine Ablauf Verfolgung zu verarbeiten. Die Kernschnittstelle ist [itraceprocessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itraceprocessor), und die Verwendung dieser Schnittstelle umfasst das folgende Muster:

1. Teilen Sie dem Prozessor zunächst mit, welche Daten Sie aus einer Ablauf Verfolgung verwenden möchten.
2. Zum anderen verarbeiten Sie die Ablauf Verfolgung. immer
3. Schließlich greifen Sie auf die Ergebnisse zu.

Wenn Sie dem Prozessor mitteilen, welche Arten von Daten Sie vorab benötigen, bedeutet dies, dass Sie keine Zeit für die Verarbeitung großer Mengen aller möglichen Arten von Ablauf Verfolgungs Daten aufwenden müssen. Stattdessen erledigt [traceprocessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) nur die Arbeit, die Sie benötigen, um die spezifischen Arten von Daten bereitzustellen, die Sie anfordern.

## <a name="recommended-project-settings"></a>Empfohlene Projekteinstellungen

Es gibt einige Projekteinstellungen, die Sie mit traceprocessor verwenden sollten:

1. Es empfiehlt sich, exe als 64-Bit-Version zu ausführen.

    Der Standardwert von Visual Studio für C# eine neue .NET Framework Konsolenanwendung ist eine beliebige CPU, bei der "32-Bit" bevorzugt aktiviert ist. Der Standardwert für .net Core hat möglicherweise bereits die empfohlene Einstellung.

    Die Ablauf Verfolgungs Verarbeitung kann Speicher intensiv sein, insbesondere bei größeren Ablauf Verfolgungen, und es wird empfohlen, das Ziel der Plattform in den exe 32 zu ändern, die traceprocessor verwenden. Informationen zum Ändern dieser Einstellungen finden Sie auf der Registerkarte "Build" unter "Eigenschaften" für das Projekt. Um diese Einstellungen für alle Konfigurationen zu ändern, stellen Sie sicher, dass die Dropdown Liste Konfiguration auf alle Konfigurationen und nicht auf die Standardeinstellung der aktuellen Konfiguration festgelegt ist.

2. Wir empfehlen die Verwendung von nuget im neueren packagereferenzierungsmodus anstelle des älteren Packages. config-Modus.

    Informationen zum Ändern der Standardeinstellung für neue Projekte finden Sie unter Tools, nuget-Paket-Manager, Paket-Manager-Einstellungen, Paketverwaltung, Standardpaket Verwaltungs Format.

## <a name="built-in-data-sources"></a>Integrierte Datenquellen

Eine ETL-Datei kann viele Arten von Daten in einer Ablauf Verfolgung erfassen. Beachten Sie, dass die Daten in einer ETL-Datei davon abhängen, welche Anbieter beim Erfassen der Ablauf Verfolgung aktiviert wurden. In der folgenden Liste werden die Arten der von traceprocessor verfügbaren Ablauf Verfolgungs Daten angezeigt:

| Code                                      | Beschreibung                                                                                                                | Verwandte WPA-Elemente                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| Ablauf Verfolgungs. Useclassicevents ()                  | Stellt klassische ETW-Ereignisse aus einer Ablauf Verfolgung bereit, die keine Schema Informationen enthalten.                                         | Tabelle für generische Ereignisse (bei einem Ereignistyp "klassisch" oder "WPP")             |
| Ablauf Verfolgungs. Useconnectedstandbydata ()           | Stellt Daten aus einer Ablauf Verfolgung bereit, über die das System in den verbundenen Standbymodus wechselt.                                        | CS-Zusammenfassungs Tabelle                                                     |
| Ablauf Verfolgungs. Usecpuidlestates ()                  | Stellt Daten aus einer Ablauf Verfolgung zu CPU-C-Zuständen bereit.                                                                             | CPU-Leerlauf Statustabelle (bei tatsächlichem Typ)                          |
| Ablauf Verfolgungs. Usecpusamplingdata ()                | Stellt Daten aus einer Ablauf Verfolgung zur CPU-Auslastung basierend auf der periodischen Stichprobenentnahme des Anweisungs Zeigers bereit.                          | CPU-Auslastungs Tabelle (Stichprobe)                                            |
| Ablauf Verfolgungs. Usecpuschedulingdata ()              | Stellt Daten aus einer Ablauf Verfolgung zur CPU-Thread Planung bereit, einschließlich Kontextwechsel und bereitgestellten Thread Ereignissen.                | CPU-Auslastungs Tabelle (präzise Tabelle)                                            |
| Ablauf Verfolgungs. Usedevicepowerdata ()                | Stellt Daten aus einer Ablauf Verfolgung über Geräte-D-Zustände bereit.                                                                          | Dstate-Tabelle des Geräts                                                  |
| Ablauf Verfolgungs. Usedirectxdata ()                    | Stellt Daten aus einer Ablauf Verfolgung über die DirectX-Aktivität bereit.                                                                         | Tabelle für GPU-Auslastung                                                |
| traceutardiskiodata ()                      | Stellt Daten aus einer Ablauf Verfolgung über die Datenträger-e/a-Aktivität bereit.                                                                        | Tabelle für Datenträger Verwendung                                                     |
| Ablauf Verfolgungs. Useenergyestimationdata ()           | Stellt Daten aus einer Ablauf Verfolgung zum geschätzten pro-Prozess-Energieverbrauch von der Energie Schätzung-Engine bereit.                         | Zusammenfassungs Tabelle (nach Prozess) der Energie Schätzung-Engine                  |
| Ablauf Verfolgungs. Useenergymeterdata ()                | Stellt Daten aus einer Ablauf Verfolgung zum gemessenen Energieverbrauch von der Energieverbrauchs Schnittstelle (Energy Meter Interface, EMI) bereit.                                  | Tabelle der Energie Schätzung-Engine (von EMI)                              |
| Ablauf Verfolgungs. Usefleiodata ()                     | Stellt Daten aus einer Ablauf Verfolgung über die Datei-e/a-Aktivität bereit.                                                                        | Datei-e/a-Tabelle                                                       |
| Ablauf Verfolgungs. Usegenericevents ()                  | Stellt manifestierte Ereignisse und tracelogging-Ereignisse aus einer Ablauf Verfolgung bereit.                                                                  | Tabelle generischer Ereignisse (wenn der Ereignistyp angezeigt wird oder tracelogging) |
| Ablauf Verfolgungs. Usehandles ()                        | Stellt partielle Daten aus einer Ablauf Verfolgung zu aktiven Kernel Handles bereit.                                                            | Behandelt die Tabelle.                                                        |
| Ablauf Verfolgungs. Usehardfaults ()                     | Stellt Daten aus einer Ablauf Verfolgung über schwerwiegende Seiten Fehler bereit.                                                                         | Tabelle mit festen Fehlern                                                    |
| Ablauf Verfolgungs. Useheapshots ()                  | Stellt Daten aus einer Ablauf Verfolgung zur Prozess Heap Verwendung bereit.                                                                       | Heap-Momentaufnahme Tabelle                                                  |
| Ablauf Verfolgungs. Usehypercalls ()                     | Stellt Daten zu Hyper-V-hyperaufrufen bereit, die während einer Ablauf Verfolgung aufgetreten sind.                                                        |                                                                      |
| Ablauf Verfolgungs. "-Magesections" ()                  | Stellt Daten aus einer Ablauf Verfolgung zu den Abschnitten eines Bilds bereit.                                                                 | Abschnitts Namensspalte der CPU-Auslastungs Tabelle (Stichprobe)                 |
| Ablauf Verfolgungs. Useingeterrupthandlingdata ()          | Stellt Daten aus einer Ablauf Verfolgung über die ISR-Aktivität (Interrupt Service Routine) und DPC-Aktivität (verzögerte Prozedur Aufrufe) bereit.               | DPC/ISR-Tabelle                                                        |
| Ablauf Verfolgungs. Usemarks ()                          | Stellt die Markierungen (mit der Bezeichnung Timestamps) aus einer Ablauf Verfolgung bereit.                                                                      | Tabelle "Markierungen"                                                          |
| Ablauf Verfolgungs. Usememoryutilizationdata ()          | Stellt Daten aus einer Ablauf Verfolgung zur Gesamtauslastung des System Arbeitsspeichers bereit.                                                          | Speicher Auslastungs Tabelle                                             |
| Ablauf Verfolgungs. Usemetadata ()                       | Stellt die verfügbaren Ablauf Verfolgungs Metadaten ohne weitere Verarbeitung bereit.                                                              | System Konfiguration, Ablauf Verfolgungen und allgemein                             |
| Ablauf Verfolgungs. Useplatformidlestates ()             | Stellt Daten aus einer Ablauf Verfolgung zum Ziel und zu den tatsächlichen Platt Form Leerlauf Zuständen eines Systems bereit.                                   | Statustabelle für Platt Form Leerlauf                                            |
| Ablauf Verfolgungs. Usepoolbelegungen ()                | Stellt Daten aus einer Ablauf Verfolgung zur Speicherauslastung des Kernel Pools bereit.                                                                 | Pool Zusammenfassungs Tabelle                                                   |
| Ablauf Verfolgungs. Usepowerconfigurationdata ()         | Stellt Daten aus einer Ablauf Verfolgung zur System Energie Konfiguration bereit.                                                               | System Konfiguration, Energie Einstellungen                                 |
| Ablauf Verfolgungs. Usepowerdependencycoordinatordata () | Stellt Daten aus einer Ablauf Verfolgung zu aktiven Power Coordinator-koordinatorphasen bereit.                                               | Tabelle mit Benachrichtigungs Phasen Zusammenfassung                                     |
| Ablauf Verfolgungs. Useprocesses ()                      | Stellt Daten zu Prozessen bereit, die während einer Ablauf Verfolgung aktiv sind, sowie deren Bilder und pdsb.                                      | Tabelle verarbeitet; Abbild Tabelle; Symbolhub                           |
| Ablauf Verfolgungs. Useprocessorcounters ()              | Stellt Daten aus einer Ablauf Verfolgung zu Werten des Prozessor Leistungs Zählers aus dem Prozessor Leistungsindikatoren-Monitor (PCM) bereit.                |                                                                      |
| Ablauf Verfolgungs. Useprocessorfrequencydata ()         | Stellt Daten aus einer Ablauf Verfolgung in Bezug auf die Häufigkeit bereit, mit der Prozessoren ausgeführt wurden.                                                    | Prozessorhäufigkeits-Tabelle (bei tatsächlichem Typ)                      |
| Ablauf Verfolgungs. Useprocessorprofiledata ()           | Stellt Daten aus einer Ablauf Verfolgung zum aktiven Prozessor Strom Profil bereit.                                                       | Tabelle "Prozessor profile"                                             |
| Ablauf Verfolgungs. Useprocessorparamekingdata ()           | Stellt Daten aus einer Ablauf Verfolgung bereit, über die die Prozessoren geparkt oder nicht geparkt werden.                                                 | Tabelle für den Status des Prozessors                                        |
| Ablauf Verfolgungs. Useprocessorparamekinglimits ()         | Stellt Daten aus einer Ablauf Verfolgung über die maximal zulässige Anzahl nicht geparkter Prozessoren bereit.                                        | Haupttabelle für die Status festgrenze                                         |
| Ablauf Verfolgungs. Useprocessorqualityofservicedata ()  | Stellt Daten aus einer Ablauf Verfolgung über die Quality of Service Level für jeden Prozessor bereit.                                          | QoS-Prozessor Klassen Tabelle                                            |
| Ablauf Verfolgungs. Useprocessorthrottlingdata ()        | Stellt Daten aus einer Ablauf Verfolgung bereit, die die maximale Frequenz Drosselung des Prozessors darstellt.                                                   | Tabelle mit Prozessor Einschränkungen                                          |
| Ablauf Verfolgungs. Usereadybootdata ()                  | Stellt Daten aus einer Ablauf Verfolgung bereit, bei der die Aktivität vor dem Start vorab abgerufen wird.                                                | Tabelle für fertige Start Ereignisse                                              |
| Ablauf Verfolgungs. Usereferencesetdata ()               | Stellt Daten aus einer Ablauf Verfolgung zu den von den einzelnen Prozessen verwendeten virtuellen Speicherseiten bereit.                                             | Verweis Satz Tabelle                                                  |
| Ablauf Verfolgungs. Useregionsofinterest ()              | Stellt benannte Bereiche von Interessen Intervallen aus einer Ablauf Verfolgung bereit, wie in einer XML-Konfigurationsdatei angegeben.                       | Regionen der relevanten Tabelle                                            |
| Ablauf Verfolgungs. Useregistrydata ()                   | Stellt während einer Ablauf Verfolgung Daten über die Registrierungs Aktivität bereit.                                                                      | Registrierungs Tabelle                                                       |
| Ablauf Verfolgungs. Useresidentsetdata ()                | Stellt Daten aus einer Ablauf Verfolgung zu den virtuellen Speicherseiten für jeden Prozess bereit, der im physischen Speicher ansässig war.       | Tabelle für residente Sätze                                                   |
| Ablauf Verfolgungs. Userundowndata ()                    | Stellt Daten aus einer Ablauf Verfolgung zu Intervallen bereit, in denen die Datensammlung für Ablauf Verfolgungs Daten aufgetreten ist.                            | Schattierte Bereiche in der Diagramm Zeitachse                                 |
| Ablauf Verfolgungs. Usescheduledtasks ()                 | Stellt Daten zu geplanten Tasks bereit, die während einer Ablauf Verfolgung ausgeführt wurden.                                                               | Tabelle für geplante Aufgaben                                                |
| Ablauf Verfolgungs. "-EServices ()"                       | Stellt Daten zu Diensten bereit, die aktiv waren oder deren Zustand während einer Ablauf Verfolgung aufgezeichnet wurde.                                  | Dienstetabelle; System Konfiguration, Dienste                       |
| Ablauf Verfolgungs. Usestacks ()                         | Stellt Daten zu Stapeln bereit, die während einer Ablauf Verfolgung aufgezeichnet wurden.                                                                        |                                                                      |
| Ablauf Verfolgungs. Usestackevents ()                    | Stellt Daten zu Ereignissen bereit, die den während einer Ablauf Verfolgung aufgezeichneten Stapeln zugeordnet sind.                                                 | Stapel Tabelle                                                         |
| Ablauf Verfolgungs. Usestacktags ()                      | Stellt einen Mapper bereit, der Stapel aus einer Ablauf Verfolgung in Stapel Tags gruppiert, wie in einer XML-Konfigurationsdatei angegeben.               | Spalten wie stacktag und Stack (Frame-Tags)                     |
| Ablauf Verfolgungs. "US-mbols ()"                        | Bietet die Möglichkeit zum Laden von Symbolen für eine Ablauf Verfolgung.                                                                          | Symbol Pfade konfigurieren Symbole laden                                 |
| Ablauf Verfolgungs. "US-Systeme" ()                       | Stellt Daten zu syscalls bereit, die während einer Ablauf Verfolgung aufgetreten sind.                                                                 | Syscalls-Tabelle                                                       |
| Ablauf Verfolgungs. "US-System Metadaten" ()                 | Bietet allgemeine, systemweite Metadaten aus einer Ablauf Verfolgung.                                                                       | Systemkonfiguration                                                 |
| Ablauf Verfolgungs. "US-System-powersourcedata ()"          | Stellt Daten aus einer Ablauf Verfolgung über die aktive System Stromquelle (AC vs DC) bereit.                                                | System Energiequellen-Tabelle                                            |
| Ablauf Verfolgungs. "US-System-sleepdata ()"                | Stellt Daten aus einer Ablauf Verfolgung zum Gesamtsystem Energiezustand bereit.                                                               | Tabelle für den Energie Übergang                                               |
| Ablauf Verfolgungs. Usetargetcpuidlestates ()            | Stellt Daten aus einer Ablauf Verfolgung über die Ziel-CPU-C-Zustände bereit.                                                                      | CPU-Leerlauf Statustabelle (wenn der Typ "target" ist)                          |
| Ablauf Verfolgungs. Usetargetprocessorfrequencydata ()   | Stellt Daten aus einer Ablauf Verfolgung zu den Zielprozessor Frequenzen bereit.                                                             | Prozessor Häufigkeits Tabelle (wenn der Typ "target" ist)                      |
| Ablauf Verfolgungs. Usethreads ()                        | Stellt Daten zu Threads bereit, die während einer Ablauf Verfolgung aktiv sind.                                                                         | Tabelle "Thread Lebensdauer"                                               |
| Ablauf Verfolgungs. Usetracestatistics ()                | Stellt Statistiken zu den Ereignissen in einer Ablauf Verfolgung bereit.                                                                           | System Konfiguration, Ablauf Verfolgungs Statistik                               |
| Ablauf Verfolgungs. Useutcdata ()                        | Stellt Daten aus einer Ablauf Verfolgung über die Microsoft-telemetrieaktivität mithilfe von Universal Telemetrie Client (UTC) bereit.                      | UTC-Tabelle                                                            |
| Ablauf Verfolgungs. Usewindowinfocus ()                  | Stellt Daten aus einer Ablauf Verfolgung zu Änderungen im aktiven Benutzeroberflächen Fenster bereit.                                                 | Fenster in der Fokus Tabelle                                                |
| Ablauf Verfolgungs. Usewindowstracepreprocessorevents () | Stellt WPP (Windows Software Trace Präprozessor)-Ereignisse aus einer Ablauf Verfolgung bereit.                                                    | WPP-Ablauf Verfolgungs Tabelle; Tabelle generischer Ereignisse (bei Ereignistyp WPP)       |
| Ablauf Verfolgungs. Usewininetdata ()                    | Stellt Daten aus einer Ablauf Verfolgung über Internetaktivität über Windows Internet (WinInet) bereit.                                         | Detail Tabelle herunterladen                                               |
| Ablauf Verfolgungs. Useworkingsetdata ()                 | Stellt Daten aus einer Ablauf Verfolgung zu virtuellen Arbeitsspeicher Seiten bereit, die im Workingset für die einzelnen Prozess-oder Kernel Kategorien enthalten waren. | Tabelle mit Momentaufnahmen des virtuellen Speichers                                       |

Weitere Informationen finden Sie auch in den Erweiterungs Methoden für [itracesource](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itracesource) für alle verfügbaren Ablauf Verfolgungs Daten, oder untersuchen Sie die in "Trace" verfügbare Methode. wird von IntelliSense angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In dieser Übersicht haben Sie erfahren, wie Sie mithilfe von traceprocessor und den integrierten Datenquellen, auf die zugegriffen werden kann, auf Ablauf Verfolgungs Daten zugreifen können.

Der nächste Schritt besteht darin, zu erfahren, wie Sie traceprocessor [erweitern](extensibility.md) , um auf benutzerdefinierte Ablauf Verfolgungs Daten zuzugreifen.
