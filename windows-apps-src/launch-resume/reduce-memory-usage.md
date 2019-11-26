---
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: Dieser Artikel beschreibt, wie Sie Arbeitsspeicher reduzieren, wenn Ihre App in den Hintergrund verschoben wird.
title: Reduzieren der Speicherverwendung bei Verschieben der App in den Hintergrundzustand
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0b5e0ea6deef7dfe3531c8d0406e08bfae80f0e2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260457"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>Freigeben von Speicher, wenn die App in den Hintergrund verschoben wird

Dieser Artikel beschreibt, wie Sie die Größe des Speichers reduzieren, den Ihre App verwendet, wenn sie in den Hintergrundzustand verschoben wird, so dass sie nicht angehalten und möglicherweise sogar beendet wird.

## <a name="new-background-events"></a>Neue Hintergrund-Ereignisse

Windows 10, Version 1607, führt zwei neue App-Lebenszyklusereignisse, [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground), ein. Diese Ereignisse ermöglichen Ihrer App zu wissen, wann sie in den Hintergrund verschoben wird bzw. diesen verlässt.

Wenn Ihre App in den Hintergrund verschoben wird, können sich die vom System erzwungen Speicherbeschränkungen ändern. Verwenden Sie diese Ereignisse, um den aktuellen Speicherverbrauch zu prüfen und Ressourcen freizugeben, um unterhalb des Grenzwerts zu bleiben, so dass Ihre App nicht angehalten und möglicherweise beendet wird, während sie sich im Hintergrund befindet.

### <a name="events-for-controlling-your-apps-memory-usage"></a>Ereignisse zur Steuerung der Speichernutzung Ihrer App

[MemoryManager.AppMemoryUsageLimitChanging](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging) wird ausgeführt, kurz bevor die Obergrenze des Speichers, den die App nutzen kann, erreicht ist. Zum Beispiel: Wenn die App in den Hintergrund verschoben wird und sich die Speichergrenze von 1024 MB zu 128 MB auf der Xbox ändert.  
Dies ist das wichtigste Ereignis, um zu verhindern, dass die Plattform die App anhält oder beendet.

[MemoryManager.AppMemoryUsageIncreased](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) wird ausgeführt, wenn der Speicherverbrauch der App auf einen höheren Wert in der [AppMemoryUsageLevel](https://docs.microsoft.com/uwp/api/windows.system.appmemoryusagelevel)-Enumeration gestiegen ist. Z. B. von **Low** auf **Medium**. Die Verwendung dieses Ereignisses ist optional, wird jedoch empfohlen, da die Anwendung immer noch dafür verantwortlich ist, unter dem Grenzwert zu bleiben.

[MemoryManager.AppMemoryUsageDecreased](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagedecreased) wird ausgeführt, wenn der Speicherverbrauch der App auf einen niedrigeren Wert in der **AppMemoryUsageLevel**-Enumeration gesunken ist. Z. B. von **High** auf **Low**. Die Verwendung dieses Ereignisses ist optional, es zeigt jedoch an, dass die Anwendung bei Bedarf weiteren Speicher zuweisen könnte.

## <a name="handle-the-transition-between-foreground-and-background"></a>Verarbeiten des Übergangs zwischen Vordergrund und Hintergrund

Wenn Ihre App vom Vordergrund in den Hintergrund wechselt, wird das [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst. Wechselt die App dann wieder in den Vordergrund, wird das [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignis ausgelöst. Sie können Handler für diese Ereignisse registrieren, wenn Ihre App erstellt wird. In der Standardprojektvorlage geschieht dies im **App**-Klassenkonstruktor in „App.xaml.cs“.

Da die Ausführung im Hintergrund die Speicherressourcen belastet, die die App behalten kann, sollten Sie die App auch für das [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased)-Ereignis und das [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging)-Ereignis registrieren. Anhand dieser Ereignisse werden die aktuelle Speicherauslastung der App und der aktuelle Grenzwert überprüft. Die Handler für diese Ereignisse werden in den folgenden Beispielen erläutert. Weitere Informationen zum Anwendungslebenszyklus für UWP-Apps finden Sie unter [App-Lebenszyklus](..//launch-resume/app-lifecycle.md).

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

Wenn das [**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground)-Ereignis ausgelöst wird, legen Sie die Nachverfolgungsvariable fest, um anzugeben, dass die App momentan im Hintergrund ausgeführt wird. Dies ist nützlich, wenn Sie Code für die Reduzierung der Speichernutzung schreiben.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

Wenn Ihre App in den Hintergrund wechselt, reduziert das System das Arbeitsspeicherlimit der App, um sicherzustellen, dass die aktuelle Vordergrund-App über genügend Ressourcen verfügt, um ein zufriedenstellendes Reaktionsverhalten der App zu gewährleisten.

Durch den [**AppMemoryUsageLimitChanging**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimitchanging)-Ereignishandler wird der App mitgeteilt, dass der ihr zugewiesene Arbeitsspeicher verringert wurde. Außerdem wird der neue Grenzwert dem Handler in den übergebenen Ereignisargumenten mitgeteilt. Vergleichen Sie die [**MemoryManager.AppMemoryUsage**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusage)-Eigenschaft, die den aktuell von der App genutzten Arbeitsspeicher angibt, mit der [**NewLimit**](https://docs.microsoft.com/uwp/api/windows.system.appmemoryusagelimitchangingeventargs.newlimit)-Eigenschaft der Ereignisargumente, die den neuen Grenzwert angibt. Wenn die Speicherauslastung den Grenzwert überschreitet, müssen Sie die Speicherauslastung verringern.

In diesem Beispiel verwenden Sie zu diesem Zweck die Hilfsmethode **ReduceMemoryUsage**, die später in diesem Artikel beschrieben wird.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> Einige Gerätekonfigurationen lassen es zu, dass eine Anwendung den neuen Grenzwert für die Speicherauslastung überschreit, bis die Systemressourcen knapp werden. Dies gilt jedoch nicht für alle Gerätekonfigurationen. Insbesondere auf der Xbox werden Apps angehalten oder beendet, wenn sie ihre Speicherauslastung nicht innerhalb von 2 Sekunden an die neuen Grenzwerte anpassen. Die beste Erfahrung auf möglichst vielen Geräten erzielen Sie mit diesem Ereignis, durch das die Ressourcenverwendung innerhalb von zwei Sekunden nach Auslösen des Ereignisses unter den Grenzwert gesenkt wird.

Es ist möglich, dass zwar der Speicherbedarf Ihrer App aktuell unter dem Speicherlimit für Hintergrund-Apps liegt, wenn sie in den Hintergrund verschoben wird, sie jedoch ihre Speichernutzung mit der Zeit erhöht und sich auf den Grenzwert zubewegt. Mit dem Handler [**AppMemoryUsageIncreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusageincreased) können Sie die aktuelle Auslastung überprüfen und bei einem Anstieg bei Bedarf Arbeitsspeicher freigeben.

Überprüfen Sie, ob [**AppMemoryUsageLevel**](https://docs.microsoft.com/uwp/api/Windows.System.AppMemoryUsageLevel) den Wert **High** oder **OverLimit** aufweist, und reduzieren Sie ggf. die Speicherauslastung. Der Prozess in diesem Beispiel wird von der Hilfsmethode **ReduceMemoryUsage** verarbeitet. Sie können auch das [**AppMemoryUsageDecreased**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagedecreased)-Ereignis abonnieren und überprüfen, ob sich die App unter dem Grenzwert befindet. Wenn dies der Fall ist, können Sie zusätzliche Ressourcen zuweisen.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

Sie können die Hilfsmethode **ReduceMemoryUsage** implementieren, um Arbeitsspeicher freizugeben, wenn die App den Auslastungsgrenzwert für Hintergrund-Apps überschreitet. Die Vorgehensweise bei der Freigabe von Arbeitsspeicher hängt von den Spezifikationen Ihrer App ab. Eine empfohlene Methode besteht darin, die Benutzeroberfläche und andere mit der App-Ansicht zusammenhängende Ressourcen freizugeben. Dazu stellen Sie sicher, dass Sie die App im Hintergrund ausführen; setzen Sie dann die [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content)-Eigenschaft des App-Fensters auf `null`, heben Sie die Registrierung der UI-Ereignishandler auf und entfernen Sie alle anderen Verweise, die Sie möglicherweise auf der Seite haben. Wenn Sie die Registrierung Ihrer UI-Ereignishandler nicht aufheben und andere Referenzen auf der Seite nicht löschen, kann die Seitenressource nicht freigegeben werden. Rufen Sie dann **GC.Collect** auf, um den freigegebenen Speicherplatz sofort freizugeben. In der Regel ist es nicht erforderlich, eine Garbage Collection zu erzwingen, da das System dies erledigt. In diesem speziellen Fall reduzieren wir die Menge an Arbeitsspeicher, die dieser Anwendung zugewiesen wird, wenn sie in den Hintergrund verschoben wird. So wird die Wahrscheinlichkeit verringert, dass das System bestimmt, dass die App beendet werden muss, um Arbeitsspeicher freizugeben.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

Wenn der Fensterinhalt gesammelt wird, leitet jeder Frame seinen Trennungsprozess ein. Wenn sich unterhalb des Fensterinhalts im visuellen Objektbaum Seiten befinden, lösen diese ihr „Unload“-Ereignis aus. Seiten können erst vollständig aus dem Arbeitsspeicher gelöscht werden, wenn alle Seitenverweise entfernt wurden. Führen Sie im Unloaded-Rückruf die folgenden Schritte aus, um sicherzustellen, dass der Arbeitsspeicher schnell freigegeben wird:
* Löschen Sie große Datenstrukturen auf der Seite, und legen Sie deren Wert auf  `null` fest.
* Heben Sie die Registrierung aller Ereignishandler, die über Rückrufmethoden verfügen, innerhalb der Seite auf. Registrieren Sie diese Rückrufe beim Aufrufen des Loaded-Ereignishandlers für die Seite. Das Loaded-Ereignis wird ausgelöst, nachdem die Benutzeroberfläche wiederhergestellt und die Seite der visuellen Objektstruktur hinzugefügt wurde.
* Rufen Sie `GC.Collect` am Ende des Unloaded-Rückrufs auf, um schnell eine Garbage Collection für große Datenstrukturen durchzuführen, die Sie gerade auf `null` festgelegt haben. Normalerweise ist es wie gesagt nicht erforderlich, eine Garbage Collection zu erzwingen, da das System dies erledigt. In diesem speziellen Fall reduzieren wir die Menge an Arbeitsspeicher, die dieser Anwendung zugewiesen wird, wenn sie in den Hintergrund verschoben wird. So wird die Wahrscheinlichkeit verringert, dass das System bestimmt, dass die App beendet werden muss, um Arbeitsspeicher freizugeben.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

Im [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)-Ereignishandler sollten Sie die Nachverfolgungsvariable (`isInBackgroundMode`) festlegen, um anzugeben, dass die App nicht mehr im Hintergrund ausgeführt wird. Überprüfen Sie als Nächstes, ob der [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) für das aktuelle Fenster `null` ist. Dies ist der Fall, wenn Sie Ihre App-Ansichten geschlossen haben, um bei der Ausführung im Hintergrund Arbeitsspeicher freizugeben. Wenn der Fensterinhalt `null` ist, bauen Sie die App-Ansicht neu auf. In diesem Beispiel wird der Fensterinhalt in der Hilfsmethode **CreateRootFrame** erstellt.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

Durch die **CreateRootFrame**-Hilfsmethode wird der Ansichtsinhalt Ihrer App neu erstellt. Der Code in dieser Methode ist beinahe identisch mit dem [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)-Handlercode in der Standardprojektvorlage. Der einzige Unterschied besteht darin, dass der **Launching**-Handler den vorherigen Ausführungsstatus anhand der [**PreviousExecutionState**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate)-Eigenschaft von [**LaunchActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) ermittelt und die **CreateRootFrame**-Methode einfach den vorherigen, als Argument übergebenen Ausführungsstatus abruft. Um doppelten Code zu minimieren, können Sie den Standardcode für den Ereignishandler **Launching** umgestalten, so dass **CreateRootFrame** aufgerufen wird.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>Richtlinien

### <a name="moving-from-the-foreground-to-the-background"></a>Verschieben vom Vordergrund in den Hintergrund

Wenn eine App vom Vordergrund in den Hintergrund verschoben wird, sorgt das System für die App dafür, dass Ressourcen, die im Hintergrund nicht benötigt werden, freigegeben werden. Beispielsweise leert das UI-Framework zwischengespeicherte Texturen, und das Video-Subsystem gibt für die App Speicher frei. Eine App muss jedoch weiterhin sorgfältig ihre Speichernutzung überwachen, um zu vermeiden, dass sie vom System angehalten oder beendet wird.

Wenn eine App vom Vordergrund in den Hintergrund wechselt, erhält sie zuerst ein **EnteredBackground**-Ereignis und dann ein **AppMemoryUsageLimitChanging**-Ereignis.

- **Verwenden** Sie das **EnteredBackground**-Ereignis, um UI-Ressourcen freizugeben, von denen Sie wissen, dass Ihre App sie im Hintergrund nicht benötigt. Beispielsweise könnten Sie das Cover-Bild für einen Song freigeben.
- **Verwenden** Sie das **AppMemoryUsageLimitChanging**-Ereignis, um sicherzustellen, dass Ihre App weniger Speicher verbraucht als der neue Hintergrundgrenzwert vorgibt. Achten Sie darauf, Ressourcen freizugeben, wenn dies nicht der Fall ist. Wenn Sie dies nicht tun, kann Ihre App je nach der gerätespezifischen Richtlinie angehalten oder beendet werden.
- **Rufen** Sie manuell den Garbage Collector auf, wenn Ihre App die neue Speichergrenze überschreitet, wenn das **AppMemoryUsageLimitChanging**-Ereignis auslöst.
- **Verwenden** Sie das **AppMemoryUsageIncreased**-Ereignis für die weitere Überwachung der Speichernutzung Ihrer App im Hintergrund, wenn Sie erwarten, dass diese sich ändert. Wenn die **AppMemoryUsageLevel** **High** oder **OverLimit** ist, geben Sie Ressourcen frei.
- **Geben Sie eventuell** UI-Ressourcen im **AppMemoryUsageLimitChanging**-Ereignishandler anstelle des **EnteredBackground**-Handlers frei, um die Leistung zu erhöhen. Verwenden Sie einen booleschen Wert in den Ereignishandlern **EnteredBackground/LeavingBackground**, um zu verfolgen, ob sich die App im Vordergrund oder im Hintergrund befindet. Wenn dann im Eventhandler **AppMemoryUsageLimitChanging** **AppMemoryUsage** den Grenzwert überschreitet und sich die App im Hintergrund befindet (nach dem booleschen Wert), können Sie UI-Ressourcen freigeben.
- **Führen Sie keine** Langzeitaufgaben im **EnteredBackground**-Ereignis durch, da dies dazu führen kann, dass der Übergang zwischen Anwendungen für die Benutzer langsam vor sich geht.

### <a name="moving-from-the-background-to-the-foreground"></a>Verschieben vom Hintergrund in den Vordergrund

Wenn eine App vom Hintergrund in den Vordergrund wechselt, erhält sie zuerst ein **AppMemoryUsageLimitChanging**-Ereignis und dann ein **LeavingBackground**-Ereignis.

- **Verwenden** Sie das **LeavingBackground**-Ereignis, um UI-Ressourcen wiederherzustellen, die Ihre App verworfen hat, als Sie in den Hintergrund wechselte.

## <a name="related-topics"></a>Verwandte Themen

* [Abspielbeispiel für Hintergrundmedien](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback) - zeigt, wie Sie Speicher freigeben, wenn ihre App in den Hintergrund wechselt.
* [Diagnosetools](https://devblogs.microsoft.com/devops/diagnostic-tools-debugger-window-in-visual-studio-2015/) - beobachten Sie mit den Diagnosetools die Garbage Collection-Ereignisse, und prüfen Sie, ob Ihre App Speicher in der erwarteten Weise freigibt.
