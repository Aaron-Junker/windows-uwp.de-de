---
description: Erfahren Sie, wie Sie die erweiterte Ausführung verwenden, damit Ihre App auch bei Minimierung weiter ausgeführt wird.
title: Verschieben der angehaltenen App mithilfe der erweiterten Ausführung
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, erweiterte Ausführung, minimiert, ExtendedExecutionSession, Hintergrundaufgabe, Anwendungslebenszyklus, Sperrbildschirm
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 8cc67a7593a340ada8f807fc0fb0c1b846c6f05b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641305"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>Verschieben der angehaltenen App mithilfe der erweiterten Ausführung

In diesem Artikel wird gezeigt, wie Sie mithilfe der erweiterten Ausführung das Anhalten Ihrer App verschieben können, damit sie auch bei Minimierung oder beim Sperrbildschirm weiter ausgeführt wird.

Wenn der Benutzer eine App minimiert oder sie verlässt, wird sie in einen angehaltenen Zustand versetzt.  Der Arbeitsspeicher wird nicht gelöscht, der App-Code wird jedoch nicht ausgeführt. Dies gilt für alle Betriebssystemeditionen mit einer grafischen Benutzeroberfläche. Weitere Informationen zum Anhalten von Apps finden Sie unter [Anwendungslebenszyklus](app-lifecycle.md). 

Es gibt Fälle, in denen eine App bei einer Minimierung möglicherweise weiter ausgeführt werden muss, statt angehalten zu werden, beispielsweise wenn der Benutzer die App wechselt oder diese minimiert wird. Zum Beispiel muss eine App zur Schrittzählung weiterhin ausgeführt werden, auch wenn der Benutzer eine andere App öffnet. 

Wenn eine App weiter ausgeführt muss, kann sie entweder vom Betriebssystem weiter ausgeführt werden, oder sie kann die weitere Ausführung anfordern. Wenn beispielsweise im Hintergrund Audioinhalte wiedergegeben werden, kann das Betriebssystem eine App weiter ausführen, wenn Sie diese Schritte für [Medienwiedergabe im Hintergrund](../audio-video-camera/background-audio.md) ausführen. Andernfalls müssen Sie manuell mehr Zeit anfordern. Sie erhalten möglicherweise mehrere Minuten Zeit für die Ausführung im Hintergrund, aber Sie müssen jederzeit auf die Verarbeitung der widerrufenen Sitzung vorbereitet sein. Diese Einschränkungen für die Lebenszykluszeit einer Anwendung sind deaktiviert, während sie unter einem Debugger ausgeführt wird. Aus diesem Grund ist es wichtig, Extended Execution und andere Tools zum Verschieben von App-Aussetzungen nur zu testen, wenn die App nicht unter einem Debugger ausgeführt wird, oder die in Visual Studio verfügbaren Lifecycle Events zu verwenden. 
 
Um mehr Zeit für das Ausführen eines Vorgangs im Hintergrund anzufordern, erstellen Sie eine [ExtendedExecutionSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionsession.aspx). Die Art der von Ihnen erstellten **ExtendedExecutionSession** wird durch den [ExtendedExecutionReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionreason.aspx) festgelegt, den Sie während des Erstellens angeben. Es gibt drei **ExtendedExecutionReason** Enum-Werte: **Nicht angegeben, handelt es sich bei LocationTracking** und **SavingData**. Es kann jeweils nur eine **ExtendedExecutionSession** angefordert werden. Wenn Sie versuchen, eine neue Sitzung zu erstellen, während eine genehmigte Sitzungsanforderung aktiv ist, wird die Ausnahme 0x8007139F vom **ExtendedExecutionSession**-Konstruktor ausgelöst, die besagt, dass die Gruppe oder Ressource nicht den richtigen Status zum Ausführen der angeforderten Operation aufweist. Verwenden Sie [ExtendedExecutionForegroundSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession.aspx) und [ExtendedExecutionForegroundReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason.aspx) nicht. Sie erfordern eingeschränkte Funktionen und sind nicht zur Verwendung in Store-Anwendungen verfügbar.

## <a name="run-while-minimized"></a>Ausführen bei Minimierung

Es gibt zwei Fälle, in denen eine erweiterter Ausführung verwendet werden kann:
- Zu jedem Zeitpunkt während der regulären Vordergrundausführung, während sich die Anwendung im aktiven Zustand befindet.
- Nachdem die Anwendung ein Halteereignis (das Betriebssystem ist dabei, die App anzuhalten) im entsprechenden Ereignishandler registriert hat.

Der Code ist in beiden Fälle identisch, aber die Anwendung verhält sich in jedem Fall ein wenig anders. Im ersten Fall bleibt die Anwendung im aktiven Zustand, auch wenn ein Ereignis auftritt, das normalerweise einen Halt auslösen würde (z. B. wenn der Benutzer die Anwendung wechselt). Die Anwendung reagiert niemals auf ein Halteereignis, während die Ausführungserweiterung aktiv ist. Sobald die Erweiterung freigegeben wird, kann die Anwendung wieder angehalten werden.

Wenn die Anwendung im zweiten Fall in den Haltezustand wechselt, verbleibt sie darin für den Zeitraum der Erweiterung. Sobald die Erweiterung abgelaufen ist, wechselt die Anwendung ohne weitere Benachrichtigung in den Haltezustand.

Verwenden Sie **ExtendedExecutionReason.Unspecified**, wenn Sie eine **ExtendedExecutionSession** erstellen, um zusätzliche Zeit anzufordern, bevor Ihre App in Szenarien wie Medienverarbeitung, Projektkompilierung oder zum Aufrechterhalten einer Netzwerkverbindung in den Hintergrund verschoben wird. Auf Desktopgeräten, auf denen Windows 10-Editionen für Desktops ausgeführt werden (Home, Pro, Enterprise und Education), verwenden Sie diesen Ansatz, um zu vermeiden, dass eine App bei Minimierung angehalten wird.

Sie fordern die Erweiterung an, wenn ein über lange Zeit ausgeführter Vorgang gestartet wird, um den Wechsel in den Zustand **Suspending** zu verschieben, der andernfalls beim Verschieben der App in den Hintergrund ausgeführt wird. Auf Desktopgeräten gibt es für mit **ExtendedExecutionReason.Unspecified** erstellte erweiterte Ausführungssitzungen eine akkubasierte zeitliche Begrenzung. Wenn das Gerät an eine Steckdose angeschlossen ist, gibt es keine zeitliche Begrenzung für die erweiterte Ausführung. Wenn das Gerät mit Akku betrieben wird, kann der Zeitraum für die erweiterte Ausführung im Hintergrund bis zu zehn Minuten betragen.

Tablet- oder Laptopbenutzer können auf Kosten der Akkulaufzeit den gleichen Zeitraum erhalten, wenn die Option **App Hintergrundaufgaben ausführen lassen** in den Einstellungen **Akkunutzung nach App** ausgewählt wird. (So finden Sie diese Option auf einem Laptop: wechseln Sie zu **Einstellungen** > **System** > **Akku** > **Akkunutzung nach App** (der Link unter dem Prozentsatz der verbleibenden Lebensdauer des Akkus) > wählen Sie eine App aus > deaktivieren Sie **Von Windows verwaltet** > wählen Sie **App Hintergrundaufgaben ausführen lassen** aus.  

Diese Art der erweiterten Ausführung wird auf allen Betriebssystemeditionen beendet, wenn das Gerät in den Zustand „Verbundener Standbymodus“ versetzt wird. Auf mobilen Geräten mit Windows 10 Mobile wird diese Art von erweiterter Ausführung solange ausgeführt, wie der Bildschirm aktiviert ist. Wenn der Bildschirm deaktiviert wird, versucht das Gerät sofort, in den energiesparenden Zustand „Verbundener Standbymodus“ zu wechseln. Auf Desktopgeräten wird die Sitzung weiter ausgeführt, wenn der Sperrbildschirm angezeigt wird. Das Gerät wechselt nach Deaktivierung des Bildschirms für einen gewissen Zeitraum nicht in den Zustand „Verbundener Standbymodus“. In der Xbox-Betriebssystemedition wechselt das Gerät nach einer Stunde in den Zustand „Verbundener Standbymodus“, wenn der Benutzer die Standardeinstellung nicht geändert hat.

## <a name="track-the-users-location"></a>Abrufen des Standorts eines Benutzers

Geben Sie beim Erstellen einer **ExtendedExecutionSession** **ExtendedExecutionReason.LocationTracking** an, wenn Ihre App regelmäßig den Standort aus [GeoLocator](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx) protokollieren muss. Apps für Fitnessnachverfolgung und Navigation, die regelmäßig den Standort des Benutzers überwachen müssen, sollten diesen Grund verwenden.

Eine erweiterte Ausführungssitzung der Standortnachverfolgung kann bei Bedarf auch ausgeführt werden, während der Bildschirm auf einem mobilen Gerät gesperrt ist. Pro Gerät kann jedoch nur eine solche Sitzung ausgeführt werden. Eine erweiterte Ausführung zur Standortnachverfolgung kann nur im Vordergrund angefordert werden, und die App muss sich im Zustand **Ausgeführt** befinden. Dadurch wird sichergestellt, dass der Benutzer weiß, dass die App die erweiterte Ausführung zur Standortnachverfolgung initiiert hat. GeoLocator kann mittels einer Hintergrundaufgabe oder eines App-Diensts während der Ausführung der App im Hintergrund weiter verwendet werden, ohne dass eine erweiterte Ausführung zur Standortnachverfolgung angefordert wird.

## <a name="save-critical-data-locally"></a>Lokales Speichern wichtiger Daten

Geben Sie beim Erstellen einer **ExtendedExecutionSession** **ExtendedExecutionReason.SavingData** an, wenn Sie in Fällen, in das Nichtspeichern von Daten vor Beenden der App zu Datenverlusten und einer negativen Benutzererfahrung führt, Benutzerdaten speichern möchten.

Verwenden Sie diese Art von Sitzung nicht, um den Lebenszyklus einer App zum Zweck des Hoch- oder Herunterladens von Daten zu verlängern. Fordern Sie eine [Hintergrundübertragung](https://msdn.microsoft.com/windows/uwp/networking/background-transfers) an, wenn Sie Daten hochladen müssen, oder registrieren Sie einen **MaintenanceTrigger**, um die Übertragung zu verarbeiten, wenn die Stromversorgung verfügbar ist. Eine erweiterte Ausführungssitzung mit dem Grund **ExtendedExecutionReason.SavingData** kann angefordert werden, wenn sich die App im Vordergrund und im Zustand **Ausgeführt** befindet oder wenn sie sich im Hintergrund und im Zustand **Angehalten** befindet.

Der Zustand **Angehalten** ist die letzte Phase während des App-Lebenszyklus, in der die App Aufgaben ausführen kann, bevor sie beendet wird. **ExtendedExecutionReason.SavingData** ist die einzige Art von **ExtendedExecutionSession**, die im **Suspending**-Zustand angefordert werden kann. Das Anfordern einer erweiterten Ausführungssitzung mit dem Grund **ExtendedExecutionReason.SavingData**, während sich die App im Zustand **Angehalten** befindet, kann ein Problem verursachen, das Sie beachten sollten. Wenn im Zustand **Suspending** eine erweiterte Ausführungssitzung angefordert wird, und der Benutzer das erneute Starten der App anfordert, benötigt sie zum Starten scheinbar eine lange Zeit. Der Grund hierfür ist, dass die erweiterte Ausführungssitzung abgeschlossen werden muss, bevor die alte Instanz der App geschlossen und eine neue Instanz der App gestartet werden kann. Es wird auf Leistung beim Starten verzichtet, um sicherzustellen, dass der Benutzerstatus nicht verloren geht.

## <a name="request-disposal-and-revocation"></a>Anfordern, Löschen und Sperren

Es gibt drei grundsätzliche Interaktionen mit erweiterten Ausführungssitzungen: Anfordern, Löschen und Sperren.  Das Anfordern wird im folgenden Codeausschnitt gezeigt.

### <a name="request"></a>Anfordern

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

Durch Aufrufen von **RequestExtensionAsync** wird durch das Betriebssystem geprüft, ob der Benutzer Hintergrundaktivitäten für die App genehmigt hat und ob das System über genügend Ressourcen verfügt, um die Hintergrundausführung zu aktivieren. Pro App wird jeweils nur eine Sitzung genehmigt, sodass weitere Aufrufe von **RequestExtensionAsync** dazu führen, dass die Sitzung abgelehnt wird.

Sie können [BackgroundExecutionManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) im Voraus überprüfen, um den [BackgroundAccessStatus](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundaccessstatus.aspx?f=255&MSPPError=-2147217396) zu ermitteln. Dies ist die Benutzereinstellung, die festlegt, ob Ihre App im Hintergrund ausgeführt werden kann oder nicht. Weitere Informationen zu diesen Benutzereinstellungen finden Sie unter [Hintergrundaktivitäten und Energieinformationen](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97).

Der **ExtendedExecutionReason** gibt den Vorgang an, der von Ihrer App im Hintergrund ausgeführt wird. Die Zeichenfolge **Beschreibung** ist eine lesbare Zeichenfolge, die beschreibt, warum Ihre App den Vorgang ausführen muss. Diese Zeichenfolge wird dem Benutzer nicht angezeigt, wird aber möglicherweise in zukünftigen Windows-Versionen verfügbar gemacht. Der Ereignishandler **Revoked** ist erforderlich, damit eine erweiterte Ausführungssitzung ordnungsgemäß angehalten werden kann, wenn der Benutzer oder das System festlegen, dass die App nicht mehr im Hintergrund ausgeführt werden kann.

### <a name="revoked"></a>Sperren

Wenn eine App über eine aktive erweiterte Ausführungssitzung verfügt und das System das Anhalten der Hintergrundaktivität erfordert, weil eine Vordergrundanwendung die Ressourcen benötigt, wird die Sitzung widerrufen. Der Zeitraum für eine erweiterte Ausführungssitzung wird nie beendet, ohne dass zuerst der Ereignishandler **Revoked** aufgerufen wird.

Wenn das Ereignis **Revoked** für eine erweiterte Ausführungssitzung mit dem Grund **ExtendedExecutionReason.SavingData** ausgelöst wird, hat die App nur eine Sekunde Zeit, um den gerade ausgeführten Vorgang abzuschließen und den Zustand **Angehalten** zu beenden.

Das Sperren kann aus verschiedenen Gründen erfolgen: das Erreichen der zeitlichen Begrenzung für die Ausführung, das Erreichen der Begrenzung für den Energieverbrauch für Hintergrundaufgaben oder das notwendige Freigeben von Arbeitsspeicher, damit der Benutzer eine neue App im Vordergrund öffnen kann.

Im Folgenden wird Ihnen ein Beispiel für den Ereignishandler „Revoked“ angezeigt:

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Löschen

Der letzte Schritt besteht im Löschen der erweiterten Ausführungssitzung. Sie sollten die Sitzung und alle weiteren, Arbeitsspeicher in Anspruch nehmenden Ressourcen löschen, da andernfalls die von der App beim Warten auf das Beenden der Sitzung verbrauchte Energie auf das Energiekontingent der App angerechnet wird. Um einen möglichst großen Teil des Energiekontingents der App zu bewahren, ist es wichtig, die Sitzung zu löschen, wenn sie nicht mehr benötigt wird, damit die App schneller in den Zustand **Angehalten** wechseln kann.

Wenn Sie die Sitzung selbst löschen, statt auf das Sperrereignis zu warten, wird die Energiekontingentnutzung Ihrer App reduziert. Das bedeutet, dass Ihre App zukünftig länger im Hintergrund ausgeführt werden darf, da ihr hierfür ein größeres Energiekontingent zur Verfügung steht. Sie müssen bis zum Abschluss des Vorgangs einen Verweis auf das Objekt **ExtendedExecutionSession** beibehalten, damit Sie dessen Methode **Dispose** aufrufen können.

Im Folgenden wird ein Codeausschnitt angezeigt, in dem eine erweiterte Ausführungssitzung gelöscht wird:

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

Für eine App kann jeweils nur eine **ExtendedExecutionSession** aktiv sein. Viele Apps verwenden asynchrone Aufgaben, um komplexe Vorgänge ausführen, die Zugriff auf Ressourcen wie Speicher, Netzwerk oder netzwerkbasierte Dienste erfordern. Wenn ein Vorgang mehrere asynchrone Vorgänge erfordert, um abgeschlossen zu werden, muss der Zustand jeder dieser Aufgaben berücksichtigt werden, bevor **ExtendedExecutionSession** gelöscht wird und die App angehalten werden kann. Dies erfordert eine Verweiszählung der Anzahl der Aufgaben, die weiter ausgeführt werden. Erst wenn dieser Wert null erreicht, kann die Sitzung gelöscht werden.

Der folgende Beispielcode dient zum Verwalten mehrerer Aufgaben während einer erweiterten Ausführungssitzung: Weitere Informationen zur Verwendung in Ihrer App finden Sie im folgenden Codebeispiel:

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked, String description)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = reason;
        newSession.Description = description;
        newSession.Revoked += SessionRevoked;

        // Add a revoked handler provided by the app in order to clean up an operation that had to be halted prematurely
        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        
        //If there are no more running tasks than end the extended lifetime by clearing the session
        if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        //The session has been prematurely revoked due to system constraints, ensure the session is disposed
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
        
        taskCount = 0;
    }
}
```
[Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>Gute Nutzung von Ressourcen durch Ihre App

Das Optimieren von Arbeitsspeicher und Energieverbrauch Ihrer App ist der Schlüssel, um sicherzustellen, dass das Betriebssystem die Ausführung Ihrer App auch dann zulässt, wenn sie sich nicht mehr im Vordergrund befindet. Um zu ermitteln, wie viel Arbeitsspeicher Ihre App verwendet, verwenden Sie die [Speicherverwaltungs-APIs](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx). Je mehr Arbeitsspeicher Ihre App verwendet, desto schwieriger wird es für das Betriebssystem, Ihre App weiter auszuführen, wenn sich eine andere App im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist.

## <a name="see-also"></a>Siehe auch

[Beispiel für erweiterte Ausführung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[Anwendungslebenszyklus](https://msdn.microsoft.com/windows/uwp/launch-resume/app-lifecycle)  
[App Lifecycle - Keep Apps Alive with Background Tasks and Extended Execution](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx)
[Geben Sie Speicher frei, wenn Ihre App in den Hintergrund verschoben wird](https://msdn.microsoft.com/windows/uwp/launch-resume/reduce-memory-usage)  
[Übertragungen im Hintergrund](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)  
[Akku Bewusstsein und Hintergrundaktivität](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)  
[Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)  