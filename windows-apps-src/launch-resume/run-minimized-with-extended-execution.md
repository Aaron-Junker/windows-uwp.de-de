---
description: Erfahren Sie, wie Sie die erweiterte Ausführung verwenden, damit Ihre App auch bei Minimierung weiter ausgeführt wird.
title: Verschieben der angehaltenen App mithilfe der erweiterten Ausführung
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, erweiterte Ausführung, minimiert, extendedexecutionsession, Hintergrundaufgabe, Anwendungslebenszyklus, Sperrbildschirm
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 41a084088ab75293cd4f8f70d5d8c248000645b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167744"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>Verschieben der angehaltenen App mithilfe der erweiterten Ausführung

In diesem Artikel erfahren Sie, wie Sie die erweiterte Ausführung verwenden, um zu verschieben, wenn Ihre APP angehalten wird, sodass Sie während der Minimierung oder unter dem Sperrbildschirm ausgeführt werden kann.

Wenn der Benutzer eine App minimiert oder sie verlässt, wird sie in einen angehaltenen Zustand versetzt.  Der Arbeitsspeicher wird nicht gelöscht, der App-Code wird jedoch nicht ausgeführt. Dies gilt für alle Betriebssystemeditionen mit einer grafischen Benutzeroberfläche. Weitere Informationen zum Anhalten von Apps finden Sie unter [Anwendungslebenszyklus](app-lifecycle.md). 

Es gibt Fälle, in denen eine APP möglicherweise weiterhin ausgeführt werden muss, anstatt angehalten zu werden, wenn der Benutzer von der APP weg navigiert oder minimiert wird. Beispielsweise muss ein Schritt zum zählen der APP zum Ausführen und Nachverfolgen von Schritten ausgeführt werden, auch wenn der Benutzer zur Verwendung anderer apps navigiert. 

Wenn eine App weiter ausgeführt muss, kann sie entweder vom Betriebssystem weiter ausgeführt werden, oder sie kann die weitere Ausführung anfordern. Wenn beispielsweise im Hintergrund Audioinhalte wiedergegeben werden, kann das Betriebssystem eine App weiter ausführen, wenn Sie diese Schritte für [Medienwiedergabe im Hintergrund](../audio-video-camera/background-audio.md) ausführen. Andernfalls müssen Sie manuell mehr Zeit anfordern. Die Zeit, die Sie für die Ausführung des Hintergrunds benötigen, kann mehrere Minuten in Anspruch nehmen. Sie müssen jedoch darauf vorbereitet sein, die Sitzung zu verarbeiten, die jederzeit widerrufen wird. Diese Anwendungslebenszyklus-Zeiteinschränkungen sind deaktiviert, während die APP unter einem Debugger ausgeführt wird. Aus diesem Grund ist es wichtig, die erweiterte Ausführung und andere Tools zum Verschieben der APP-Unterbrechung zu testen, während Sie nicht unter einem Debugger ausgeführt wird oder die in Visual Studio verfügbaren Lebenszyklus Ereignisse verwendet werden. 
 
Um mehr Zeit für das Ausführen eines Vorgangs im Hintergrund anzufordern, erstellen Sie eine [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession). Die Art der von Ihnen erstellten **ExtendedExecutionSession** wird durch den [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) festgelegt, den Sie während des Erstellens angeben. Es gibt drei Enumerationswerte für **ExtendedExecutionReason**: **Unspecified, LocationTracking** und **SavingData**. Es kann immer nur eine " **extendedexecutionsession** " angefordert werden. Wenn Sie versuchen, eine andere Sitzung zu erstellen, während eine genehmigte Sitzungs Anforderung derzeit aktiv ist, wird die Ausnahme 0x8007139f vom **extendedexecutionsession** -Konstruktor ausgelöst, die angibt, dass die Gruppe oder die Ressource nicht den richtigen Status aufweist, um den angeforderten Vorgang auszuführen. Verwenden Sie nicht " [extendedexecutionforegroundsession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) " und " [extendedexecutionforegroundreason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason);". Sie benötigen eingeschränkte Funktionen und sind nicht für die Verwendung in Store-Anwendungen verfügbar.

## <a name="run-while-minimized"></a>Ausführen bei Minimierung

Es gibt zwei Fälle, in denen die erweiterte Ausführung verwendet werden kann:
- Zu einem beliebigen Zeitpunkt während der regulären Vordergrund Ausführung, während sich die Anwendung im Zustand wird ausgeführt befindet.
- Nachdem die Anwendung ein Anhalten-Ereignis empfangen hat (das Betriebssystem ist im Begriff, die app in den angehaltenen Zustand zu verschieben), wird der Ereignishandler der Anwendung angehalten.

Der Code für diese beiden Fälle ist identisch, aber die Anwendung verhält sich in jedem wenig anders. Im ersten Fall verbleibt die Anwendung im Status wird ausgeführt, auch wenn ein Ereignis, das normalerweise eine Unterbrechung auslöst, auftritt (z. b. wenn der Benutzer von der Anwendung weg navigiert). Die Anwendung empfängt niemals ein anhaltereignis, während die Ausführungs Erweiterung wirksam ist. Wenn die Erweiterung verworfen wird, kann die Anwendung wieder angehalten werden.

Im zweiten Fall, wenn die Anwendung in den Zustand "angehalten" übergeht, bleibt Sie für den Zeitraum der Erweiterung im Zustand "angehalten". Nach Ablauf der Erweiterung wechselt die Anwendung ohne weitere Benachrichtigung in den Zustand "angehalten".

Verwenden Sie " **extendedexecutionreason. unspezifiziert** ", wenn Sie eine " **extendedexecutionsession** " erstellen, um zusätzliche Zeit anzufordern, bevor die APP für Szenarien wie Medienverarbeitung, Projekt Kompilierung oder Aufrechterhaltung einer Netzwerkverbindung in den Hintergrund wechselt. Auf Desktopgeräten, auf denen Windows 10-Editionen für Desktops ausgeführt werden (Home, Pro, Enterprise und Education), verwenden Sie diesen Ansatz, um zu vermeiden, dass eine App bei Minimierung angehalten wird.

Sie fordern die Erweiterung an, wenn ein über lange Zeit ausgeführter Vorgang gestartet wird, um den Wechsel in den Zustand **Suspending** zu verschieben, der andernfalls beim Verschieben der App in den Hintergrund ausgeführt wird. Auf Desktopgeräten gibt es für mit **ExtendedExecutionReason.Unspecified** erstellte erweiterte Ausführungssitzungen eine akkubasierte zeitliche Begrenzung. Wenn das Gerät an eine Steckdose angeschlossen ist, gibt es keine zeitliche Begrenzung für die erweiterte Ausführung. Wenn das Gerät mit Akku betrieben wird, kann der Zeitraum für die erweiterte Ausführung im Hintergrund bis zu zehn Minuten betragen.

Ein Tablet-oder Laptop Benutzer kann das gleiche Verhalten mit langer Ausführungszeit (auf Kosten der Akku Lebensdauer) erzielen, wenn die Option **für die Ausführung von Hintergrundaufgaben für die App zulassen** unter **Akku Nutzung nach App** -Einstellungen aktiviert ist. (Wechseln Sie zu " **Einstellungen**  >  ", um diese Option auf einem Laptop zu finden. **System**  >  **Akku**  >  **Akku Auslastung nach App** (der Link unter dem Prozentsatz der verbleibenden Akkuleistung) > wählen Sie eine APP aus, > Sie die **Verwaltung durch Windows** ausschalten > wählen Sie **die Option App zum Ausführen von Hintergrundaufgaben zulassen**aus.  

Diese Art der erweiterten Ausführung wird auf allen Betriebssystemeditionen beendet, wenn das Gerät in den Zustand „Verbundener Standbymodus“ versetzt wird. Auf mobilen Geräten mit Windows 10 Mobile wird diese Art von erweiterter Ausführung solange ausgeführt, wie der Bildschirm aktiviert ist. Wenn der Bildschirm deaktiviert wird, versucht das Gerät sofort, in den energiesparenden Zustand „Verbundener Standbymodus“ zu wechseln. Auf Desktopgeräten wird die Sitzung weiter ausgeführt, wenn der Sperrbildschirm angezeigt wird. Das Gerät wechselt nach Deaktivierung des Bildschirms für einen gewissen Zeitraum nicht in den Zustand „Verbundener Standbymodus“. In der Xbox-Betriebssystemedition wechselt das Gerät nach einer Stunde in den Zustand „Verbundener Standbymodus“, wenn der Benutzer die Standardeinstellung nicht geändert hat.

## <a name="track-the-users-location"></a>Abrufen des Standorts eines Benutzers

Geben Sie beim Erstellen einer **ExtendedExecutionSession****ExtendedExecutionReason.LocationTracking** an, wenn Ihre App regelmäßig den Standort aus [GeoLocator](/uwp/api/windows.devices.geolocation.geolocator) protokollieren muss. Apps für Fitnessnachverfolgung und Navigation, die regelmäßig den Standort des Benutzers überwachen müssen, sollten diesen Grund verwenden.

Eine Standort Nachverfolgung der erweiterten Ausführungs Sitzung kann so lange wie nötig ausgeführt werden, auch wenn der Bildschirm auf einem mobilen Gerät gesperrt ist. Pro Gerät kann jedoch nur eine solche Sitzung ausgeführt werden. Eine erweiterte Ausführung zur Standortnachverfolgung kann nur im Vordergrund angefordert werden, und die App muss sich im Zustand **Ausgeführt** befinden. Dadurch wird sichergestellt, dass der Benutzer weiß, dass die App die erweiterte Ausführung zur Standortnachverfolgung initiiert hat. GeoLocator kann mittels einer Hintergrundaufgabe oder eines App-Diensts während der Ausführung der App im Hintergrund weiter verwendet werden, ohne dass eine erweiterte Ausführung zur Standortnachverfolgung angefordert wird.

## <a name="save-critical-data-locally"></a>Lokales Speichern wichtiger Daten

Geben Sie beim Erstellen einer **ExtendedExecutionSession****ExtendedExecutionReason.SavingData** an, wenn Sie in Fällen, in das Nichtspeichern von Daten vor Beenden der App zu Datenverlusten und einer negativen Benutzererfahrung führt, Benutzerdaten speichern möchten.

Verwenden Sie diese Art von Sitzung nicht, um den Lebenszyklus einer App zum Zweck des Hoch- oder Herunterladens von Daten zu verlängern. Fordern Sie eine [Hintergrundübertragung](../networking/background-transfers.md) an, wenn Sie Daten hochladen müssen, oder registrieren Sie einen **MaintenanceTrigger**, um die Übertragung zu verarbeiten, wenn die Stromversorgung verfügbar ist. Eine erweiterte Ausführungssitzung mit dem Grund **ExtendedExecutionReason.SavingData** kann angefordert werden, wenn sich die App im Vordergrund und im Zustand **Ausgeführt** befindet oder wenn sie sich im Hintergrund und im Zustand **Angehalten** befindet.

Der Zustand **Angehalten** ist die letzte Phase während des App-Lebenszyklus, in der die App Aufgaben ausführen kann, bevor sie beendet wird. " **Extendedexecutionreason. savingdata** " ist der einzige Typ von " **extendedexecutionsession** " **, der im anhaltezustand angefordert** werden kann. Das Anfordern einer erweiterten Ausführungssitzung mit dem Grund **ExtendedExecutionReason.SavingData**, während sich die App im Zustand **Angehalten** befindet, kann ein Problem verursachen, das Sie beachten sollten. Wenn im Zustand **Suspending** eine erweiterte Ausführungssitzung angefordert wird, und der Benutzer das erneute Starten der App anfordert, benötigt sie zum Starten scheinbar eine lange Zeit. Der Grund hierfür ist, dass die erweiterte Ausführungssitzung abgeschlossen werden muss, bevor die alte Instanz der App geschlossen und eine neue Instanz der App gestartet werden kann. Es wird auf Leistung beim Starten verzichtet, um sicherzustellen, dass der Benutzerstatus nicht verloren geht.

## <a name="request-disposal-and-revocation"></a>Anfordern, Löschen und Sperren

Es gibt drei grundsätzliche Interaktionen mit erweiterten Ausführungssitzungen: Anfordern, Löschen und Sperren.  Das Anfordern wird im folgenden Codeausschnitt gezeigt.

### <a name="request"></a>Anforderung

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
[Siehe Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

Durch Aufrufen von **RequestExtensionAsync** wird durch das Betriebssystem geprüft, ob der Benutzer Hintergrundaktivitäten für die App genehmigt hat und ob das System über genügend Ressourcen verfügt, um die Hintergrundausführung zu aktivieren. Nur eine Sitzung wird für eine APP zu einem beliebigen Zeitpunkt genehmigt. Dies führt zu weiteren Aufrufen von **requestextensionasync** , um zu verhindern, dass die Sitzung verweigert wird.

Sie können [BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) im Voraus überprüfen, um den [BackgroundAccessStatus](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus?f=255&MSPPError=-2147217396) zu ermitteln. Dies ist die Benutzereinstellung, die festlegt, ob Ihre App im Hintergrund ausgeführt werden kann oder nicht. Weitere Informationen zu diesen Benutzereinstellungen finden Sie unter [Hintergrundaktivitäten und Energieinformationen](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97).

Der **ExtendedExecutionReason** gibt den Vorgang an, der von Ihrer App im Hintergrund ausgeführt wird. Die Zeichenfolge **Beschreibung** ist eine lesbare Zeichenfolge, die beschreibt, warum Ihre App den Vorgang ausführen muss. Diese Zeichenfolge wird dem Benutzer nicht angezeigt, wird jedoch möglicherweise in einer zukünftigen Version von Windows zur Verfügung gestellt. Der Ereignishandler **Revoked** ist erforderlich, damit eine erweiterte Ausführungssitzung ordnungsgemäß angehalten werden kann, wenn der Benutzer oder das System festlegen, dass die App nicht mehr im Hintergrund ausgeführt werden kann.

### <a name="revoked"></a>Widerrufen

Wenn eine APP über eine aktive erweiterte Ausführungs Sitzung verfügt und das System die Hintergrund Aktivität zum Anhalten benötigt, weil eine Vordergrund Anwendung die Ressourcen benötigt, wird die Sitzung widerrufen. Der Zeitraum für eine erweiterte Ausführungssitzung wird nie beendet, ohne dass zuerst der Ereignishandler **Revoked** aufgerufen wird.

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
[Siehe Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Dispose

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
[Siehe Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

Für eine App kann jeweils nur eine **ExtendedExecutionSession** aktiv sein. Viele Apps verwenden asynchrone Aufgaben, um komplexe Vorgänge ausführen, die Zugriff auf Ressourcen wie Speicher, Netzwerk oder netzwerkbasierte Dienste erfordern. Wenn ein Vorgang mehrere asynchrone Vorgänge erfordert, um abgeschlossen zu werden, muss der Zustand jeder dieser Aufgaben berücksichtigt werden, bevor **ExtendedExecutionSession** gelöscht wird und die App angehalten werden kann. Dies erfordert eine Verweiszählung der Anzahl der Aufgaben, die weiter ausgeführt werden. Erst wenn dieser Wert null erreicht, kann die Sitzung gelöscht werden.

Im folgenden finden Sie einen Beispielcode für die Verwaltung mehrerer Tasks während eines Zeitraums für die erweiterte Ausführungs Sitzung. Weitere Informationen zur Verwendung dieses in ihrer App finden Sie im folgenden Codebeispiel:

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
[Siehe Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>Gute Nutzung von Ressourcen durch Ihre App

Das Optimieren von Arbeitsspeicher und Energieverbrauch Ihrer App ist der Schlüssel, um sicherzustellen, dass das Betriebssystem die Ausführung Ihrer App auch dann zulässt, wenn sie sich nicht mehr im Vordergrund befindet. Um zu ermitteln, wie viel Arbeitsspeicher Ihre App verwendet, verwenden Sie die [Speicherverwaltungs-APIs](/uwp/api/windows.system.memorymanager). Je mehr Arbeitsspeicher Ihre App verwendet, desto schwieriger wird es für das Betriebssystem, Ihre App weiter auszuführen, wenn sich eine andere App im Vordergrund befindet. Der Benutzer hat letztendlich die Kontrolle über alle Hintergrundaktivitäten, die Ihre App ausführen kann, und kann die Auswirkungen Ihrer App auf den Akkuverbrauch erkennen.

Um zu ermitteln, ob der Benutzer die Hintergrundaktivität Ihrer App begrenzt hat, verwenden Sie [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager). Achten Sie auf die Akkunutzung, und führen Sie Ihre App nur dann im Hintergrund aus, wenn dies zum Ausführen einer vom Benutzer gewünschten Aktion notwendig ist.

## <a name="see-also"></a>Weitere Informationen

[Beispiel für die erweiterte Ausführung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[Anwendungslebenszyklus](./app-lifecycle.md)  
[App-Lebenszyklus: sorgen Sie für aktive apps mit Hintergrundaufgaben und erweiterter Ausführung](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) 
 [Hintergrund Speicherverwaltung](./reduce-memory-usage.md)  
[Hintergrund Übertragungen](../networking/background-transfers.md)  
[Akku Bewusstsein und Hintergrund Aktivität](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager-Klasse](/uwp/api/windows.system.memorymanager)  
[Wiedergeben von Medien im Hintergrund](../audio-video-camera/background-audio.md)