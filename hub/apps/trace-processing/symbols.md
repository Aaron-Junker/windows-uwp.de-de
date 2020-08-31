---
title: 'Laden von Symbolen: .net traceprocessing'
description: In diesem Tutorial erfahren Sie, wie Sie bei der Verarbeitung von Ablauf Verfolgungen Symbole laden.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 72264b51edcc0b02aa335b8766100c196a0d5090
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168814"
---
# <a name="use-symbols-in-net-traceprocessing"></a>Verwenden von Symbolen in .net traceprocessing

[Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) unterstützt das Laden von Symbolen und das erhalten von Stapeln aus mehreren Datenquellen. Die folgende Konsolenanwendung prüft CPU-Beispiele und gibt die geschätzte Dauer aus, die eine bestimmte Funktion ausgeführt hat (basierend auf der statistischen Stichprobe der CPU-Auslastung der Ablauf Verfolgung).

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

Wenn Sie dieses Programm ausführen, wird eine Ausgabe ähnlich der folgenden erzeugt:

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

(Die Ausgabe Details variieren in Abhängigkeit von der Ablauf Verfolgung).

## <a name="symbols-format"></a>Symbol Format

Intern verwendet traceprocessor das [symcache](/windows-hardware/test/wpt/loading-symbols#symcache-path) -Format, bei dem es sich um einen Cache von einigen Daten handelt, die in einer PDB gespeichert sind. Beim Laden von Symbolen erfordert traceprocessor die Angabe eines Speicher Orts für diese symcache-Dateien (einen symcache-Pfad) und unterstützt optional die Angabe eines SymbolPath für den Zugriff auf pdsb. Wenn ein SymbolPath bereitgestellt wird, erstellt traceprocessor bei Bedarf symcache-Dateien aus PDB-Dateien, und bei der nachfolgenden Verarbeitung derselben Daten können die symcache-Dateien direkt verwendet werden, um die Leistung zu verbessern.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie bei der Verarbeitung von Ablauf Verfolgungen Symbole laden.

Im nächsten Schritt erfahren Sie, wie Sie mithilfe von [Streaming](streaming.md) auf Ablauf Verfolgungs Daten zugreifen, ohne alles im Arbeitsspeicher zu puffern.