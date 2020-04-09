---
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Optimieren von Hintergrundaktivitäten
description: Erstellen Sie UWP-Apps, die mit dem System interagieren, um Hintergrundaufgaben auf eine den Akku effizient nutzende Weise auszuführen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eb3ff12e4b616edd7b87cab7f13aa060f301fc52
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683833"
---
# <a name="optimize-background-activity"></a>Optimieren von Hintergrundaktivitäten

Universelle Windows-Apps sollten auf allen Gerätefamilien mit konsistenter Qualität ausgeführt werden. Auf akkubetriebenen Geräten stellt der Energieverbrauch einen wichtigen Faktor in Bezug auf die allgemeine Erfahrung der Benutzer mit Ihrer App dar. Jeder Benutzer wünscht sich eine Akkulaufzeit für den ganzen Tag. Hierfür muss jedoch jede auf dem Gerät installierte Software, einschließlich Ihrer Software, effizient ausgeführt werden. 

Das Verhalten von Hintergrundaufgaben ist der wichtigste Faktor für den Energieverbrauch von Apps. Eine Hintergrundaufgabe ist eine Programmaktivität, die beim System zur Ausführung registriert wurde, ohne dass die App geöffnet ist. Weitere Informationen findest du unter [Erstellen und Registrieren einer Out-of-Process-Hintergrundaufgabe](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

## <a name="background-activity-permissions"></a>Berechtigungen für Hintergrundaktivitäten

Auf Desktop- und Mobilgeräten, die Windows 10 ab Version 1607 ausführen, können Benutzer ihre „Akkunutzung nach App“ im Abschnitt „Akku“ der Einstellungs-App anzeigen. Hier wird ihnen eine Liste von Apps und der Prozentsatz der Akkulaufzeit (als Anteil an der verbrauchten Akkulaufzeit seit dem letzten Aufladen) angezeigt, den die einzelnen Apps verbraucht haben. Benutzer können die App für UWP-Apps in dieser Liste auswählen, um Optionen für Hintergrundaktivitäten anzuzeigen.

![Akkunutzung nach App](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>Hintergrundberechtigungen für Mobilgeräte

Auf Mobilgeräten wird Benutzern eine Liste von Optionsfeldern angezeigt, die die Berechtigungen für Hintergrundaufgaben der App angeben. Hintergrundaktivitäten können auf „Immer zulassen“, „Niemals zulassen“, oder „Von Windows verwaltet“ (die Hintergrundaktivität der App wird anhand verschiedener Faktoren vom System gesteuert) festgelegt werden. 

![Optionsfelder für Hintergrundberechtigungen](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>Hintergrundberechtigungen für Desktopgeräte

Auf Desktopgeräten wird die Option „Von Windows verwaltet“ als Umschalter angezeigt, der standardmäßig auf **Ein** festgelegt ist. Wenn der Benutzer die Option auf **Aus** festlegt, wird ein Kontrollkästchen angezeigt, mit dem die Berechtigungen für Hintergrundaktivitäten manuell definiert werden kann. Wenn das Kontrollkästchen aktiviert ist, kann die App jederzeit Hintergrundaufgaben ausführen. Wenn das Kontrollkästchen deaktiviert ist, werden die Hintergrundaktivitäten deaktiviert.

![Schalter für Hintergrundberechtigungen ein](images/background-task-permissions-on.png)

![Schalter für Hintergrundberechtigungen aus](images/background-task-permissions-off.png)

In deiner App kannst du den Enumerationswert [**BackgroundAccessStatus**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) verwenden, der durch Aufrufen der [**BackgroundExecutionManager.RequestAccessAsync()** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)-Methode zurückgegeben wird, um die aktuell festgelegten Hintergrundberechtigungen anzuzeigen.

Wenn Ihre App keine verantwortliche Verwaltung von Hintergrundaufgaben implementiert, lehnen Benutzer möglicherweise Genehmigungen für Hintergrundaktivitäten für Ihre App insgesamt ab. Dies ist für keine der beiden Parteien vorteilhaft. Wenn deine App keine Berechtigung zum Ausführen von Hintergrundaktivitäten hat, die für eine Aktion des Benutzers erforderlich sind, kann der Benutzer benachrichtigt und auf die Einstellungs-App verwiesen werden. Dies kann durch das [Starten der Einstellungs-App](https://docs.microsoft.com/windows/uwp/launch-resume/launch-settings-app) auf der Seite für Hintergrund-Apps oder der Informationsseite für den Akkuverbrauch erfolgen.

## <a name="work-with-the-battery-saver-feature"></a>Arbeiten mit dem Stromsparmodus-Feature
Der Stromsparmodus ist ein Feature auf Systemebene, das Benutzer in den Einstellungen konfigurieren können. Es deaktiviert alle Hintergrundaktivitäten aller Apps, wenn der Akkustand einen benutzerdefinierten Schwellenwert unterschreitet. *Ausgenommen* sind Hintergrundaktivitäten von Apps, für die „Immer zugelassen“ festgelegt wurde.

Innerhalb deiner App kannst du den Status des Stromsparmodus durch einen Verweis auf die [**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/uwp/api/windows.system.power.energysaverstatus)-Eigenschaft anzeigen. Dieser Enumerationswert lautet **EnergySaverStatus.Disabled**, **EnergySaverStatus.Off** oder **EnergySaverStatus.On**. Wenn deine App Hintergrundaktivitäten erfordert, aber nicht auf „Immer zulassen“ festgelegt ist, sollte die Einstellung **EnergySaverStatus.On** den Benutzer benachrichtigen, dass er den Stromsparmodus deaktivieren muss, damit die betreffende Hintergrundaufgabe ausgeführt werden kann. Auch wenn die Verwaltung von Hintergrundaufgaben den primären Zweck des Stromsparmodus-Features darstellen, kann Ihre App zusätzliche Anpassungen vornehmen, um bei aktiviertem Stromsparmodus zusätzlich Energie zu sparen.  Wenn der Stromsparmodus aktiviert ist, könnte Ihre App die Verwendung von Animationen reduzieren, das Abrufen von Standorten beenden oder Synchronisierungen und Sicherungen verzögern. 

## <a name="further-optimize-background-tasks"></a>Zusätzliches Optimieren von Hintergrundaufgaben
Im Folgenden finden Sie zusätzliche Schritte, die Sie beim Registrieren von Hintergrundaufgaben ausführen können, um zusätzliche Energie zu sparen.

### <a name="use-a-maintenance-trigger"></a>Verwenden eines Wartungsauslösers 
Sie können ein [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.maintenancetrigger)-Objekt anstelle eines [**SystemTrigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.systemtrigger)-Objekts verwenden, um festzulegen, wann eine Hintergrundaufgabe gestartet wird. Aufgaben, die Wartungsauslöser verwenden, werden nur ausgeführt, wenn das Gerät an die Stromversorgung angeschlossen ist, und dürfen länger ausgeführt werden. Weitere Anweisungen finden Sie unter [Verwenden von Wartungsauslösern](https://docs.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger).

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>Verwenden des Systembedingungstyps **BackgroundWorkCostNotHigh**
Die Systembedingungen müssen erfüllt sein, damit Hintergrundaufgaben ausgeführt werden. (Weitere Informationen finden Sie unter [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](https://docs.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task)). Die Kosten für Hintergrundaufgaben stellen eine Messung dar, die die *relativen* Auswirkungen auf den Energieverbrauch angibt, wenn die Hintergrundaufgabe ausgeführt wird. Eine Aufgabe, die ausgeführt wird, während das Gerät an die Stromversorgung angeschlossen ist, würde als **niedrig** markiert werden (wenig/keine Auswirkung auf den Akku). Eine Aufgabe, die ausgeführt wird, wenn das Gerät mit Akkustrom betrieben wird und der Bildschirm ausgeschaltet ist, wird als **hoch** markiert, da zu diesem Zeitpunkt vermutlich nur wenige Programmaktivitäten auf dem Gerät ausgeführt werden. Daher verursacht eine Hintergrundaufgabe höhere relative Kosten. Eine Aufgabe, die ausgeführt wird, während das Gerät mit Akkustrom betrieben wird und der Bildschirm *eingeschaltet* ist, wird als **mittel** markiert, da vermutlich einige Programmaktivitäten bereits ausgeführt werden und die Hintergrundaufgabe etwas mehr zum Energieverbrauch beiträgt. Die **BackgroundWorkCostNotHigh**-Systembedingung verzögert einfach die Fähigkeit Ihrer Aufgabe, ausgeführt zu werden, bis der Bildschirm eingeschaltet ist oder das Gerät an die Stromversorgung angeschlossen ist.

## <a name="test-battery-efficiency"></a>Testen der Akkueffizienz

Teste deine App auf realen Geräten, um Szenarien mit hohem Energieverbrauch zu untersuchen. Es ist sinnvoll, Ihre App auf vielen unterschiedlichen Geräten bei aktiviertem und deaktiviertem Stromsparmodus und in Umgebungen mit unterschiedlichen Netzwerksignalstärken zu testen.

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [Planen der Leistung](https://docs.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

