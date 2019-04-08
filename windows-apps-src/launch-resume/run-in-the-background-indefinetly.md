---
title: Unbegrenzte Ausführung im Hintergrund
description: Verwenden Sie die extendedExecutionUnconstrained-Funktion im Hintergrund, um eine Hintergrundaufgabe oder eine erweiterte Ausführungssitzung ohne zeitliche Begrenzung auszuführen.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: Erweiterte Ausführung "," Ressourcen "," Einschränkungen "," Hintergrundaufgabe Hintergrundaufgabe
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: faac1d8d47ddcff4e5ec32d35f2e46bab7a3f4aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630245"
---
# <a name="run-in-the-background-indefinitely"></a>Unbegrenzte Ausführung im Hintergrund

Um Benutzern die bestmögliche Erfahrung zu bieten, erzwingt Windows Ressourcenbeschränkungen für universelle Windows-Plattform (UWP)-Apps. Vordergrund-Apps erhalten den größten Arbeitsspeicher und die meiste Ausführungszeit; bei Hintergrund-Apps wird dies eingeschränkt. Benutzer sind daher vor einer schlechten Vordergrund-App-Leistung und einem hohen Akkuverbrauch geschützt.

Entwickler, die allerdings UWP-Apps für den persönlichen Gebrauch schreiben (d. h. quergeladene Apps, die im Microsoft Store nicht veröffentlicht werden), oder Entwickler von Unternehmens-UWP-Apps sollten alle verfügbaren Ressourcen auf dem Gerät nutzen, ohne Einschränkung für den Hintergrund oder die erweiterte Ausführung. Geschäftssparten und persönliche UWP-Apps können die APIs im Windows Creators Update (Version 1703) verwenden, um die Einschränkung zu deaktivieren. Beachten Sie, dass Sie keine App m Microsoft Store anbieten können, die diese APIs verwendet.

## <a name="run-while-minimized"></a>Ausführen bei Minimierung

UWP-Apps werden in den angehaltenen Zustand versetzt, wenn sie nicht im Vordergrund ausgeführt werden. Auf dem Desktop tritt dies ein, wenn ein Benutzer die App minimiert. Apps verwenden eine erweiterte Ausführungssitzung, um auch während sie minimiert sind ausgeführt zu werden. Die erweiterten Ausführungs-APIs, die vom Microsoft Store akzeptiert werden, werden ausführlich unter [Verschieben der angehaltenen App mithilfe der erweiterten Ausführung](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution) erklärt.

Wenn Sie eine App entwickeln, die nicht für die Übermittlung an den Microsoft Store gedacht ist, können Sie die eingeschränkte Funktion [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) mit der `extendedExecutionUnconstrained` verwenden, damit Ihre App ausgeführt wird, während sie minimiert ist, unabhängig vom Stromverbrauch des Geräts.  

Die `extendedExecutionUnconstrained`-Funktion wird als eingeschränkte Funktion im App-Manifest hinzugefügt. Weitere Informationen zu eingeschränkten Funktionen finden Sie unter [Deklarationen von App-Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

_Datei "Package.appxmanifest"_
```xml
<Package ...>
...
  <Capabilities>  
    <rescap:Capability Name="extendedExecutionUnconstrained"/>  
  </Capabilities>  
</Package>
```

Bei Verwendung der `extendedExecutionUnconstrained`-Funktion, werden [ExtendedExecutionForegroundSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) und [ExtendedExecutionForegroundReason](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) verwendet anstatt von [ExtendedExecutionSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) und [ExtendedExecutionReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Das gleiche Muster zum Erstellen der Sitzung, Einrichten von Mitgliedern und zum Anfordern der asynchronen Erweiterung ist trotzdem gültig: 

```cs
var newSession = new ExtendedExecutionForegroundSession();  
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;  
newSession.Description = "Long Running Processing";  
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

Sie können diese erweiterte Ausführungssitzung anfordern, sobald die App in den Vordergrund geholt wird. Uneingeschränkte erweiterte Ausführungssitzungen sind nicht durch Energie-Kontingente oder durch den Stromsparmodus des Betriebssystems eingeschränkt. Solange ein Verweis auf das Sitzungsobjekt vorhanden ist, bleibt die App im ausgeführten Zustand und geht nicht in den angehaltenen Zustand über. Wenn die App vom Benutzer geschlossen wird, wird die Sitzung gesperrt.

Das Registrieren eines **gesperrten** Ereignisses ermöglicht Ihrer App alle erforderlichen Bereinigungen durchzuführen. Im angehaltenen Zustand können Sie eine erweiterte Ausführungssitzung mit [ExtendedExecutionReason.SavingData](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) erstellen, um Benutzerdaten zu speichern, bevor die App beendet und aus dem Speicher entfernt wird.

## <a name="run-background-tasks-indefinitely"></a>Unbegrenzte Ausführung von Hintergrundaufgaben

In der universellen Windows-Plattform werden Hintergrundaufgaben als Prozesse bezeichnet, die im Hintergrund ohne jede Benutzeroberfläche ausgeführt werden. Hintergrundaufgaben werden in der Regel maximal 25 Sekunden ausgeführt, bevor sie abgebrochen werden. Einige zeitintensivere Aufgaben enthalten ebenfalls eine Überprüfung, um sicherzustellen, dass die Hintergrundaufgabe sich nicht im Leerlauf oder mit Speicher befindet. Im Windows Creators Update (Version 1703), wurde die eingeschränkte Funktion [extendedBackgroundTaskTime](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) eingeführt, um diese Einschränkungen zu entfernen. Die **extendedBackgroundTaskTime**-Funktion wird als eingeschränkte Funktion im App-Manifest hinzugefügt:

_Datei "Package.appxmanifest"_
```xml
<Package ...>
   <Capabilities>  
       <rescap:Capability Name="extendedBackgroundTaskTime"/>  
   </Capabilities>  
</Package>
```

Diese Funktion entfernt die Einschränkungen der Ausführungszeit und den Überwachungsdienst der Leerlaufaufgaben. Sobald eine Hintergrundaufgabe gestartet wurde, sei es durch einen Trigger oder einen Aufruf des App-Diensts, kann diese unbegrenzt ausgeführt werden, nachdem eine Verzögerung auf der [BackgroundTaskInstance](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) durch die **Run**-Methode bereitgestellt wurde. Wenn die App auf **Von Windows verwaltet** festgelegt wurde, kann möglicherweise noch ein Energiekontingent angewendet werden und die Hintergrundaufgaben werden nicht aktiviert, wenn der Stromsparmodus aktiviert ist. Dies kann mit betriebssystemeinstellungen geändert werden. Weitere Informationen finden Sie unter [Optimieren von Hintergrundaktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity).

Die universelle Windows-Plattform überwacht die Aufgabenausführung im Hintergrund, um eine gute Akkulaufzeit und eine reibungslose Vordergrund-App-Umgebung zu gewährleisten. Persönliche Apps und branchenspezifische Unternehmens-Apps können jedoch die erweiterte Ausführung und die **extendedBackgroundTaskTime**-Funktion zum Erstellen von Apps verwenden, die solange wie notwendig und unabhängig von der Verfügbarkeit des Geräts ausgeführt werden.

Beachten Sie, das die **extendedExecutionUnconstrained** und **extendedBackgroundTaskTime**-Funktionen die Standardrichtlinie für UWP-Apps überschreiben können und einen erheblichen Akkuverbrauch verursachen können. Bestätigen Sie vor der Nutzung dieser Funktionen, dass die Richtlinien für die standardmäßige erweiterte Ausführung und die Hintergrundaufgabenzeit nicht Ihren Bedürfnissen entspricht und führen Sie unter Umständen Tests in einer Umgebung mit eingeschränktem Akku aus, um die Auswirkungen zu verstehen, die Ihre App auf ein Gerät hat.

## <a name="see-also"></a>Siehe auch

[Einschränkungen für Hintergrund Aufgabe Ressource aufheben](https://docs.microsoft.com/windows/application-management/enterprise-background-activity-controls)
