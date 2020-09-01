---
title: Unbegrenzte Ausführung im Hintergrund
description: Verwenden Sie die Funktion "extendedexecutionuneingeschränkter", um eine Hintergrundaufgabe oder eine erweiterte Ausführungs Sitzung unbegrenzt im Hintergrund auszuführen.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: Hintergrundaufgabe, erweiterte Ausführung, Ressourcen, Limits, Hintergrundaufgabe
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ec77b0f4777f12d20ec13bcfbac864993afd441
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175114"
---
# <a name="run-in-the-background-indefinitely"></a>Unbegrenzte Ausführung im Hintergrund

Um Benutzern eine optimale Benutzer Leistung zu bieten, erzwingt Windows Ressourcen Limits für universelle Windows-Plattform-Apps (UWP). Vordergrund-apps erhalten den meisten Arbeitsspeicher und die Ausführungszeit. Hintergrund-apps werden weniger angezeigt. Die Benutzer sind daher vor schlechten Leistung der Vordergrund-APP und hohem Akku Aufwand geschützt.

Entwickler, die UWP-Apps für den persönlichen Gebrauch schreiben (d. h. apps, die nicht im Microsoft Store veröffentlicht werden), oder Entwickler, die Enterprise-UWP-apps schreiben, möchten möglicherweise alle Ressourcen verwenden, die auf dem Gerät verfügbar sind, ohne dass eine Hintergrund Einschränkung oder eine erweiterte Ausführungs Drosselung möglich ist. Line-of-Business-und private UWP-Anwendungen können APIs in Windows Creators Update (Version 1703) verwenden, um die Drosselung zu deaktivieren. Beachten Sie, dass Sie eine APP nicht in den Microsoft Store platzieren können, wenn diese APIs verwendet werden.

## <a name="run-while-minimized"></a>Ausführen bei Minimierung

UWP-apps werden in den Zustand "angehalten" versetzt, wenn Sie nicht im Vordergrund ausgeführt werden. Auf dem Desktop tritt dieses Problem auf, wenn ein Benutzer die APP minimiert. Apps verwenden eine Sitzung für die erweiterte Ausführung, um die Ausführung weiterhin zu minimieren. Die erweiterten Ausführungs-APIs, die vom Microsoft Store akzeptiert werden, werden unter [Verschieben der APP-Unterbrechung mit erweiterter Ausführung](./run-minimized-with-extended-execution.md)ausführlich erläutert.

Wenn Sie eine App entwickeln, die nicht an die Microsoft Store übermittelt werden soll, können Sie die [extendedexecutionforegroundsession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) mit der `extendedExecutionUnconstrained` eingeschränkten Funktion verwenden, damit Ihre APP während der Minimierung weiterhin ausgeführt werden kann, unabhängig vom Energiezustand des Geräts.  

Die `extendedExecutionUnconstrained` Funktion wird als eingeschränkte Funktion im Manifest ihrer app hinzugefügt. Weitere Informationen zu eingeschränkten Funktionen finden Sie unter APP-Funktions [Deklarationen](../packaging/app-capability-declarations.md) .

> **Hinweis:** Fügen Sie die XML-Namespace Deklaration *xmlns: rescap* hinzu, und verwenden Sie das cmdx *-Präfix zum* Deklarieren der Funktion.

_Package.appxmanifest_
```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

Wenn Sie die `extendedExecutionUnconstrained` Funktion verwenden, werden " [extendedexecutionforegroundsession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) " und " [extendedexecutionforegroundreason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) " anstelle von " [extendedexecutionsession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) " und " [extendedexecutionreason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason)" verwendet. Das gleiche Muster zum Erstellen der Sitzung, zum Festlegen von Membern und zum asynchronen anfordern der Erweiterung gilt weiterhin: 

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

Sie können diese erweiterte Ausführungs Sitzung anfordern, sobald die APP im Vordergrund steht. Nicht eingeschränkte erweiterte Ausführungs Sitzungen werden nicht durch Energie Kontingente oder den Akku Schoner des Betriebssystems beschränkt. Solange ein Verweis auf das Sitzungs Objekt vorhanden ist, bleibt die APP im Ausführungs Status und wird nicht in den Zustand "angehalten" versetzt. Wenn die APP vom Benutzer geschlossen wird, wird die Sitzung widerrufen.

Wenn Sie sich für das **widerrufene** Ereignis registrieren, kann Ihre APP alle erforderlichen Bereinigungs Arbeiten ausführen. Im Zustand "wird angehalten" können Sie eine erweiterte Ausführungs Sitzung mit "   [extendedexecutionreason. savingdata](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) " erstellen, um Benutzerdaten zu speichern, bevor die APP beendet und aus dem Arbeitsspeicher entfernt wird.

## <a name="run-background-tasks-indefinitely"></a>Ausführen von Hintergrundaufgaben auf unbestimmte Zeit

Im universelle Windows-Plattform handelt es sich bei Hintergrundaufgaben um Prozesse, die ohne Benutzeroberfläche im Hintergrund ausgeführt werden. Hintergrundaufgaben können in der Regel für maximal 20 bis fünf Sekunden ausgeführt werden, bevor Sie abgebrochen werden. Einige der Tasks mit längerer Ausführungszeit verfügen auch über eine Prüfung, um sicherzustellen, dass sich die Hintergrundaufgabe nicht im Leerlauf befindet oder Arbeitsspeicher verwendet. In Windows Creators Update (Version 1703) wurde die eingeschränkte [extendedbackgroundtasktime](../packaging/app-capability-declarations.md) -Funktion eingeführt, um diese Limits zu entfernen. Die **extendedbackgroundtasktime** -Funktion wird als eingeschränkte Funktion in der Manifest-Datei Ihrer app hinzugefügt:

> **Hinweis:** Fügen Sie die XML-Namespace Deklaration *xmlns: rescap* hinzu, und verwenden Sie das cmdx *-Präfix zum* Deklarieren der Funktion.

_Package.appxmanifest_
```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

Diese Funktion entfernt Einschränkungen der Ausführungszeit und den Task Watchdog im Leerlauf. Nachdem eine Hintergrundaufgabe gestartet wurde (ob durch einen-oder App Service-Befehl), kann Sie nach einer Verzögerung für [backgroundtaskinstance](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) , die von der **Run** -Methode bereitgestellt wird, unbegrenzt ausgeführt werden. Wenn die APP auf die **Verwaltung durch Windows**festgelegt ist, kann auf Sie trotzdem ein Energie Kontingent angewendet werden, und die Hintergrundaufgaben werden nicht aktiviert, wenn Akku Schoner aktiv ist.Dies kann mit den Betriebs Systemeinstellungen geändert werden. Weitere Informationen finden Sie unter [Optimieren von Hintergrund Aktivitäten](../debug-test-perf/optimize-background-activity.md).

Der universelle Windows-Plattform überwacht die Ausführung im Hintergrund Task, um eine gute Akku Lebensdauer und eine reibungslose Vordergrund-App-Erfahrung Persönliche apps und branchenspezifische Branchen Anwendungen können jedoch die erweiterte Ausführung und die **extendedbackgroundtasktime** -Funktion zum Erstellen von Apps verwenden, die unabhängig von der Ressourcen Verfügbarkeit des Geräts so lange ausgeführt werden, wie erforderlich.

Beachten Sie, dass die Funktionen " **extendedexecutionuneingeschränkter** " und " **extendedbackgroundtasktime** " die Standard Richtlinie für UWP-apps überschreiben können und einen erheblichen Akku Ausgleich verursachen können. Vergewissern Sie sich vor der Verwendung dieser Funktionen zunächst, dass die standardmäßigen erweiterten Ausführungs-und Hintergrund Task Zeit Richtlinien Ihre Anforderungen nicht erfüllen, und führen Sie Tests in Bezug auf Akku Bedingungen aus, um zu verstehen, welche Auswirkung Ihre APP auf ein Gerät hat.

## <a name="see-also"></a>Weitere Informationen

[Ressourceneinschränkungen für Hintergrundaufgaben entfernen](/windows/application-management/enterprise-background-activity-controls)