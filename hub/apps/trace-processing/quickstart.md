---
title: 'Schnellstart: Verarbeiten einer Ablauf Verfolgung: .net-Ablauf Verfolgungs Verarbeitung'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie auf Daten in einer etw-Ablauf Verfolgung zugreifen.
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629102"
---
# <a name="quickstart-process-your-first-trace"></a>Schnellstart: Verarbeiten der ersten Ablauf Verfolgung

Testen Sie traceprocessor, um auf Daten in einer etw-Ablauf Verfolgung (Ereignis Ablauf Verfolgung für Windows) zuzugreifen. Traceprocessor ermöglicht Ihnen den Zugriff auf etw-Ablauf Verfolgungs Daten als .NET-Objekte.

In diesem Schnellstart lernen Sie Folgendes:

1. Installieren Sie das nuget-Paket "traceprocessing".
2. Erstellen Sie einen traceprocessor.
3. Verwenden Sie traceprocessor, um auf die in der Ablauf Verfolgung enthaltenen Prozess Befehlszeilen zuzugreifen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>Installieren des nuget-Pakets "traceprocessing"

.Net traceprocessing ist über [nuget](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) mit der folgenden Paket-ID verfügbar:

Microsoft. Windows. eventtracing. processing. all

Sie können dieses Paket in einer Konsolen-App verwenden, um die Prozess Befehlszeilen aufzulisten, die in einer etw-Ablauf Verfolgung (ETL-Datei) enthalten sind.

1. Erstellen Sie eine neue .net Core-Konsolen-app. Wählen Sie in Visual Studio Datei, neu, Projekt... aus, und wählen Sie die Vorlage Konsolen-app (.net C#Core) für aus.

    Geben Sie einen Projektnamen ein, z. b. traceprocessorquickstart, und klicken Sie auf erstellen.

2. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf Abhängigkeiten, und wählen Sie nuget-Pakete verwalten... aus. und wechseln Sie zur Registerkarte durchsuchen.

3. Geben Sie im Suchfeld den Suchbegriff Microsoft. Windows. eventtracing. processing. all ein, und suchen Sie nach.

    Wählen Sie im nuget-Paket mit diesem Namen installieren aus, und schließen Sie das Fenster nuget.

## <a name="create-a-traceprocessor"></a>Erstellen eines traceprocessor

1. Ändern Sie Program.cs in den folgenden Inhalt:

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. Geben Sie einen Ablauf Verfolgungs Namen an, der beim Ausführen des Projekts verwendet werden soll

    Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie Eigenschaften aus. Wechseln Sie zur Registerkarte Debuggen, und geben Sie den Pfad zu einer Ablauf Verfolgung (ETL-Datei) in Anwendungs Argumente ein.

    Wenn Sie noch nicht über eine Ablauf Verfolgungs Datei verfügen, können Sie diese mit [Windows Performance Recorder](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) erstellen.

3. Führen Sie die Anwendung aus.

    Wählen Sie Debuggen, starten ohne Debugging aus, um den Code auszuführen.

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>Verwenden von traceprocessor für den Zugriff auf die in der Ablauf Verfolgung enthaltenen Prozess Befehlszeilen

1. Ändern Sie Program.cs in den folgenden Inhalt:

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

2. Führen Sie die Anwendung erneut aus.

    Dieses Mal sollten Sie eine Liste der Befehlszeilen aus allen Prozessen sehen, die ausgeführt wurden, während die Ablauf Verfolgung aufgezeichnet wurde.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Konsolenanwendung, die Installation von traceprocessor erstellt und für den Zugriff auf die Verarbeitungs Befehlszeilen aus einer etw-Ablauf Verfolgung verwendet. Nun verfügen Sie über eine Anwendung, die auf Ablauf Verfolgungs Daten zugreift.

Prozessinformationen sind nur eine von vielen Arten von Daten, die in einer etw-Ablauf Verfolgung gespeichert werden, auf die Ihre Anwendung zugreifen kann.

Der nächste Schritt besteht darin, den traceprocessor und die anderen Datenquellen, auf die er zugreifen kann, [genauer anzusehen](tutorial.md) .
