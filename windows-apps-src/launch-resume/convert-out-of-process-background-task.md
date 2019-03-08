---
title: Portieren einer Out-of-Process Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe
description: Portieren Sie eine Out-of-Process-Hintergrundaufgabe eine in-Process Hintergrundaufgabe, die in den Vordergrund-app-Prozess ausgeführt wird.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundtasks, app service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 97dd249165877591743892a136d51e0969dd902a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601205"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Portieren einer Out-of-Process Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe

Die einfachste Möglichkeit, Ihre Out-of-Process (OOP)-Hintergrundaktivität in Aktivität Bearbeitung port ist, schalten Sie Ihre [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) Methode in Ihrer Anwendung code, und initiieren sie über [OnBackgroundActivated ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). Die hier beschriebene Verfahren ist nicht zum Erstellen eines Shims aus einem Hintergrundtask OOP auf einer in-Process-Hintergrundaufgabe; die Informationen zum Umschreiben (oder Portieren) eine OOP-Version auf eine in-Process-Version.

Wenn Ihre App mehrere Hintergrundaufgaben aufweist, wird in [Hintergrundaktivierungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) beschrieben, wie Sie mithilfe von `BackgroundActivatedEventArgs.TaskInstance.Task.Name` ermitteln können, welche Aufgabe initiiert wird.

Wenn Sie derzeit zwischen Vordergrund- und Hintergrundprozessen kommunizieren, können Sie diesen Zustandverwaltungs- und Kommunikationscode entfernen.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Hintergrundaufgaben und Triggertypen, die nicht konvertiert werden können

* In-Process-Hintergrundaufgaben unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe.
* In-Process-Hintergrundaufgaben unterstützen nicht die folgenden Trigger:  [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) und **IoTStartupTask**
