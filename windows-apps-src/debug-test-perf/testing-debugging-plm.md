---
description: Tools und Verfahren zum Debuggen und Testen der Kompatibilität Ihrer App mit der Prozesslebensdauer-Verwaltung.
title: Test- und Debugtools für die Prozesslebensdauer-Verwaltung (PLM)
ms.date: 4/8/2019
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 8ac6d127-3475-4512-896d-80d1e1d66ccd
ms.localizationpriority: medium
ms.openlocfilehash: 94cbad6e497ea2f5b36a07a6b039bfc293175c4c
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244346"
---
# <a name="testing-and-debugging-tools-for-process-lifetime-management-plm"></a>Test- und Debugtools für die Prozesslebensdauer-Verwaltung (PLM)

Einer der Hauptunterschiede zwischen UWP-Apps und traditionellen Desktopanwendungen besteht darin, dass UWP-Titel in einem App-Container enthalten sind, der der Prozesslebensdauer-Verwaltung (Process Lifecycle Management, PLM) unterliegt. UWP-Apps können plattformübergreifend durch den Runtime Broker-Dienst angehalten, fortgesetzt oder beendet werden. Um diese Übergänge beim Testen oder Debuggen des Codes, durch den sie behandelt werden, zu erzwingen, stehen Ihnen spezielle Tools zur Verfügung.

## <a name="features-in-visual-studio-2015"></a>Features in Visual Studio 2015

Der integrierte Debugger in Visual Studio 2015 kann Sie bei der Untersuchung potenzieller Probleme unterstützen, die bei Verwendung reiner UWP-Features auftreten können. Mithilfe der Symbolleiste **Ereignisse des Lebenszyklus**, die beim Ausführen und Debuggen Ihres Titels sichtbar wird, können Sie für die Anwendung unterschiedliche PLM-Zustände erzwingen.

![Symbolleiste „Ereignisse des Lebenszyklus“](images/gs-debug-uwp-apps-001.png)

## <a name="the-plmdebug-tool"></a>Tool PLMDebug

Das Befehlszeilentool „PLMDebug.exe“ ist Bestandteil des Windows SDKs und ermöglicht es Ihnen, den PLM-Zustand eines Anwendungspakets zu steuern. Nach der Installation finden Sie das Tool standardmäßig unter *C:\Programme (x86)\Windows Kits\10\Debuggers\x64*.

Mithilfe von PLMDebug können Sie außerdem PLM für beliebige installierte App-Pakete deaktivieren, was für einige Debugger erforderlich ist. Durch das Deaktivieren von PLM wird verhindert, dass der Runtime Broker-Dienst Ihre App beendet, bevor Sie sie debuggen können. Verwenden Sie zum Deaktivieren von PLM die Option **/enableDebug** gefolgt vom *vollständigen Paketnamen* Ihrer UWP-App (der Kurzname, der Paketfamilienname oder die AUMID eines Pakets funktioniert nicht):

```cmd
plmdebug /enableDebug [PackageFullName]
```

Nachdem Sie Ihre UWP-App aus Visual Studio bereitgestellt haben, wird im Ausgabefenster der vollständige Paketname angezeigt. Alternativ können Sie den vollständigen Paketnamen abrufen, indem Sie **Get-AppxPackage** in einer PowerShell-Konsole ausführen.

![Ausführen von Get-AppxPackage](images/gs-debug-uwp-apps-003.png)

Optional können Sie einen absoluten Pfad zu einem Debugger angeben, der beim Aktivieren des App-Pakets automatisch gestartet wird. Wenn Sie zu diesem Zweck Visual Studio verwenden möchten, müssen Sie „VSJITDebugger.exe“ als Debugger angeben. „VSJITDebugger.exe“ erfordert jedoch, dass Sie zusätzlich zur Prozess-ID (PID) der UWP-App die Option „-p“ angeben. Da Sie die PID Ihrer UWP-App im Voraus nicht kennen können, ist dieses Szenario nicht ohne weitere Vorbereitung möglich.

Sie können diese Einschränkung umgehen, indem Sie ein Skript oder Tool schreiben, mit dem der Prozess Ihres Spiels identifiziert wird. Anschließend führt die Shell „VSJITDebugger.exe“ aus und übergibt die PID Ihrer UWP-App. Das folgende C#-Codebeispiel veranschaulicht einen direkten Ansatz für dieses Verfahren.

```cs
using System.Diagnostics;

namespace VSJITLauncher
{
    class Program
    {
        static void Main(string[] args)
        {
            // Name of UWP process, which can be retrieved via Task Manager.
            Process[] processes = Process.GetProcessesByName(args[0]);

            // Get PID of most recent instance
            // Note the highest PID is arbitrary. Windows may recycle or wrap the PID at any time.
            int highestId = 0;
            foreach (Process detectedProcess in processes)
            {
                if (detectedProcess.Id > highestId)
                    highestId = detectedProcess.Id;
            }

            // Launch VSJITDebugger.exe, which resides in C:\Windows\System32
            ProcessStartInfo startInfo = new ProcessStartInfo("vsjitdebugger.exe", "-p " + highestId);
            startInfo.UseShellExecute = true;

            Process process = new Process();
            process.StartInfo = startInfo;
            process.Start();
        }
    }
}
```

Verwendungsbeispiel für dieses Verfahren in Verbindung mit PLMDebug:

```cmd
plmdebug /enableDebug 279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg "\"C:\VSJITLauncher.exe\" Game"
```

`Game` entspricht dabei dem Prozessnamen und `279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg` dem vollständigen Paketnamen des UWP-App-Pakets im Beispiel.

Beachten Sie, dass jeder **/enableDebug**-Aufruf später mit der Option **/disableDebug** an einen weiteren PLMDebug-Aufruf gekoppelt werden muss. Darüber hinaus muss der Pfad zu einem Debugger absolut sein (relative Pfade werden nicht unterstützt).

## <a name="related-topics"></a>Verwandte Themen

- [Bereitstellen und Debuggen von UWP-Apps](deploying-and-debugging-uwp-apps.md)
- [Debugging, Tests und Leistung](index.md)
