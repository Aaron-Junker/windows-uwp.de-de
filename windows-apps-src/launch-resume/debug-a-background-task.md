---
title: Debuggen einer Hintergrundaufgabe
description: Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 5696d3c5ffb28ee8dc6ebd51e678894ee78ae420
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192980"
---
# <a name="debug-a-background-task"></a>Debuggen einer Hintergrundaufgabe


**Wichtige APIs**
-   [Windows.ApplicationModel.Background](/uwp/api/Windows.ApplicationModel.Background)

Hier erfahren Sie, wie Sie eine Hintergrundaufgabe einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung im Windows-Ereignisprotokoll debuggen.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Debuggen von Out-of-Process- im Vergleich zu In-Process-Hintergrundaufgaben
In diesem Thema werden hauptsächlich Hintergrundaufgaben behandelt, die in einem von der Host-App separaten Prozess ausgeführt werden. Wenn Sie eine In-Process-Hintergrundaufgabe debuggen, verfügen Sie über kein separates Hintergrundaufgabenprojekt und können einen Haltepunkt auf **OnBackgroundActivated()** (wo Ihr In-Process-Hintergrundcode ausgeführt wird) festlegen. Anleitungen zum Auslösen des Hintergrundcodes finden Sie unter Schritt 2 in [Manuelles Auslösen von Hintergrundaufgaben, um Hintergrundaufgabencode zu debuggen](#trigger-background-tasks-manually-to-debug-background-task-code) unten.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>Sicherstellen, dass das Hintergrundaufgabenprojekt richtig eingerichtet wurde

In diesem Thema wird vorausgesetzt, dass Sie bereits über eine App mit einer Hintergrundaufgabe verfügen, die Sie debuggen möchten. Der folgende Code ist speziell für Hintergrundaufgaben geeignet, die außerhalb des Prozesses ausgeführt werden, und gilt nicht für In-Process-Hintergrundaufgaben.

-   Stellen Sie für C# und C++ sicher, dass das Hauptprojekt auf das Hintergrundaufgabenprojekt verweist. Ist dieser Verweis nicht vorhanden, wird die Hintergrundaufgabe nicht in das App-Paket eingebunden.
-   Stellen Sie für C# und C++ sicher, dass der **Ausgabetyp** des Hintergrundaufgabenprojekts „Komponente für Windows-Runtime“ lautet.
-   Die Background-Klasse muss im-Attribut für den Einstiegspunkt im Paket Manifest deklariert werden.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>Manuelles Auslösen von Hintergrundaufgaben, um Hintergrundaufgabencode zu debuggen

Hintergrundaufgaben können mit Microsoft Visual Studio manuell ausgelöst werden. Anschließend können Sie den Code schrittweise durchlaufen und ihn debuggen.

1.  Setzen Sie in C# einen Haltepunkt in der Run-Methode der Hintergrundklasse (für In-Process-Aufgaben den Haltepunkt in App.OnBackgroundActivated()) festlegen), und/oder schreiben Sie mithilfe von [**System.Diagnostics**](/dotnet/api/system.diagnostics) eine Debuggerausgabe.

    Setzen Sie in C++ einen Haltepunkt in der Run-Funktion der Hintergrundklasse (für In-Process-Aufgaben den Haltepunkt in App.OnBackgroundActivated()) festlegen, und/oder schreiben Sie mithilfe von [**OutputDebugString**](/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw) eine Debuggerausgabe.

2.  Führen Sie Ihre Anwendung im Debugger aus, und lösen Sie dann die Hintergrundaufgabe über die Symbolleiste **Zyklusereignisse** aus. In diesem Dropdownmenü werden die Namen der Hintergrundaufgaben angezeigt, die von Visual Studio aktiviert werden können.

    > [!NOTE]
    > Die Symbolleisten Optionen für Lebenszyklus Ereignisse werden in Visual Studio standardmäßig nicht angezeigt. Um diese Optionen anzuzeigen, klicken Sie mit der rechten Maustaste auf die aktuelle Symbolleiste in Visual Studio, und stellen Sie sicher, dass **Debugspeicherort** aktiviert ist.

    Dies funktioniert nur dann, wenn die Hintergrundaufgabe bereits registriert ist und noch auf den Auslöser wartet. Wenn eine Hintergrundaufgabe z. B. mit einem einmaligen TimeTrigger registriert wurde und der Trigger bereits ausgelöst wurde, hat ein Start der Aufgabe mit Visual Studio keine Wirkung.

    > [!Note]
    > Hintergrundaufgaben, die folgende Trigger verwenden, können nicht auf diese Weise aktiviert werden: [**Anwendungs-Trigger**](/uwp/api/windows.applicationmodel.background.applicationtrigger), [**MediaProcessing Trigger**](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger), [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) und Hintergrundaufgaben, die einen [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) mit dem Triggertyp [**SmsReceived**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) verwenden.  
    > **Anwendungs-Trigger** und **MediaProcessingTrigger** können im Code mit `trigger.RequestAsync()` manuell signalisiert werden.

    ![Debuggen von Hintergrundaufgaben](images/debugging-activation.png)

3.  Wenn die Hintergrundaufgabe aktiviert wird, wird sie vom Debugger übernommen, der die Debuggerausgabe dann in VS anzeigt.

## <a name="debug-background-task-activation"></a>Debuggen der Aktivierung der Hintergrundaufgabe

> [!NOTE]
> Dieser Abschnitt behandelt speziell Hintergrundaufgaben, die außerhalb des Prozesses ausgeführt werden, und gilt nicht für In-Process-Hintergrundaufgaben.

Die Aktivierung von Hintergrundaufgaben hängt von drei Faktoren ab:

-   Der Name und Namespace der Hintergrundaufgabenklasse
-   Das im Paketmanifest angegebene Einstiegspunktattribut
-   Der Einstiegspunkt, der von der App bei der Registrierung der Hintergrundaufgabe angegeben wird

1.  Verwenden Sie Visual Studio, um den Einstiegspunkt der Hintergrundaufgabe zu notieren:

    -   Notieren Sie für C# und C++, den Namen und den Namespace der Hintergrundaufgabenklasse, die im Hintergrundaufgabenprojekt angegeben ist.

2.  Verwenden Sie den Manifest-Designer, um zu überprüfen, ob die Hintergrundaufgabe ordnungsgemäß im Paketmanifest deklariert wurde:

    -   Für C# und C++ muss das Einstiegspunktattribut dem Namespace der Hintergrundaufgabe gefolgt vom Klassennamen entsprechen. Beispiel: RuntimeComponent1.MyBackgroundTask.
    -   Alle Triggerarten, die mit der Aufgabe verwendet werden, müssen ebenfalls angegeben sein.
    -   Die ausführbare Datei DARF NICHT angegeben werden, es sei denn, Sie verwenden [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) oder [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger).

3.  Nur Windows Durch Aktivieren der Debugablaufverfolgung und Verwendung des Windows-Ereignisprotokolls können Sie den Einstiegspunkt ermitteln, der von Windows zum Aktivieren der Hintergrundaufgabe verwendet wird.

    Wenn Sie sich an dieses Verfahren halten und das Ereignisprotokoll den falschen Einstiegspunkt oder Trigger für die Hintergrundaufgabe anzeigt, wird die Hintergrundaufgabe nicht ordnungsgemäß von der App registriert. Weitere Informationen zu dieser Aufgabe finden Sie unter [Registrieren einer Hintergrundaufgabe](register-a-background-task.md).

    1.  Öffnen Sie die Ereignisanzeige, indem Sie zum Startbildschirm wechseln und nach „eventvwr.exe“ suchen.
    2.  Wechseln Sie zu **Anwendungs-und Dienst Protokolle**  - &gt; **Microsoft**  - &gt; **Windows**  - &gt; **backgroundtaskinfrastructure** in der Ereignisanzeige.
    3.  Wählen Sie im Aktionsbereich **Ansicht**  - &gt; **analytische und Debugprotokolle anzeigen** aus, um die Diagnoseprotokollierung zu aktivieren.
    4.  Wählen Sie die Option **Diagnoseprotokoll** aus, und klicken Sie auf **Protokoll aktivieren**.
    5.  Versuchen Sie nun, die App zu verwenden, um die Hintergrundaufgabe erneut zu registrieren und zu aktivieren.
    6.  Zeigen Sie detaillierte Fehlerinformationen im Diagnoseprotokoll an. Dies schließt den Einstiegspunkt ein, der für die Hintergrundaufgabe registriert ist.

![Ereignisanzeige für Debuginformationen für Hintergrundaufgaben](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>Hintergrundaufgaben und Bereitstellung des Visual Studio-Pakets

Wenn eine APP, die Hintergrundaufgaben verwendet, mithilfe von Visual Studio bereitgestellt wird und die im Manifest-Designer angegebene Version (Haupt-und/oder neben Version) dann aktualisiert wird, kann das erneute Bereitstellen der APP mit Visual Studio dazu führen, dass die Hintergrundaufgaben der APP nicht mehr ausgeführt werden. Dieses Problem kann wie folgt behoben werden:

-   Stellen Sie die aktualisierte App mit Windows PowerShell (anstelle von Visual Studio) bereit, indem Sie das für das Paket erstellte Skript ausführen.
-   Wenn Sie die APP bereits mithilfe von Visual Studio bereitgestellt haben und ihre Hintergrundaufgaben jetzt angehalten wurden, starten Sie neu, oder melden Sie sich ab, und melden Sie sich an, um die Hintergrundaufgaben der APP erneut auszuführen
-   Sie können dies in C#-Projekten vermeiden, indem Sie die Debugoption "Paket immer neu installieren" auswählen.
-   Warten Sie, bis die APP bereit für die endgültige Bereitstellung ist, um die Paketversion zu erhöhen (ändern Sie Sie beim Debuggen nicht).

## <a name="remarks"></a>Bemerkungen

-   Stellen Sie sicher, dass Ihre App nach vorhandenen Registrierungen von Hintergrundaufgaben sucht, bevor sie die Hintergrundaufgabe erneut registriert. Eine mehrfache Registrierung derselben Hintergrundaufgabe kann zu unerwarteten Ergebnissen führen, da die Hintergrundaufgabe mehrfach ausgelöst und ausgeführt wird.
-   Wenn für die Hintergrundaufgabe der Zugriff auf den Sperrbildschirm erforderlich ist, müssen Sie dafür sorgen, dass die App auf dem Sperrbildschirm platziert wird, bevor Sie versuchen, die Hintergrundaufgabe zu debuggen. Weitere Informationen zum Angeben von Manifestoptionen für sperrbildschirmfähige Apps finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).
-   Parameter für die Registrierung von Hintergrundaufgaben werden zum Zeitpunkt der Registrierung überprüft. Bei ungültigen Registrierungsparametern wird ein Fehler zurückgegeben. Stellen Sie sicher, dass Ihre App problemlos mit Szenarien ohne erfolgreiche Registrierung von Hintergrundaufgaben zurechtkommt. Andernfalls stürzt die App unter Umständen ab, wenn sie so konzipiert ist, dass nach dem Versuch, eine Aufgabe zu registrieren, ein gültiges Registrierungsobjekt vorhanden sein muss.

Weitere Informationen zum Verwenden von vs zum Debuggen einer Hintergrundaufgabe finden Sie unter Vorgehens [Weise beim Ausführen von Suspend-, Resume-und Background-Ereignissen in UWP-apps](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio).

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb von Prozessen](create-and-register-an-inproc-background-task.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)
* [So können Sie Ereignisse in Windows UWP-Apps auslösen, anhalten, fortsetzen und im Hintergrund ausführen](/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Analysieren der Codequalität von UWP-apps mit der Code Analyse von Visual Studio](/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 