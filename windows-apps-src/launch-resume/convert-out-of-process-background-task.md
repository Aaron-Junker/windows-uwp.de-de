---
title: Portieren einer Out-of-Process Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe
description: Portieren Sie eine Out-of-Process-Hintergrundaufgabe zu einem Prozess internen Hintergrund Task, der in Ihrem Vordergrund-App-Prozess ausgeführt wird.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe, App Service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162654"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Portieren einer Out-of-Process Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe

Die einfachste Möglichkeit zum Portieren ihrer out-of-Process (OOP)-Hintergrund Aktivität auf eine Prozess interne Aktivität besteht darin, Ihren [ibackgroundtask. Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) -Methoden Code in Ihre Anwendung zu bringen und ihn aus [onbackgroundaktiviertem](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)zu initiieren. Das hier beschriebene Verfahren geht nicht auf das Erstellen eines Shims aus einer OOP-Hintergrundaufgabe zu einer Prozess internen Hintergrundaufgabe ein. Dabei geht es um das Umschreiben (oder portieren) einer OOP-Version in eine Prozess interne Version.

Wenn Ihre App mehrere Hintergrundaufgaben aufweist, wird in [Hintergrundaktivierungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) beschrieben, wie Sie mithilfe von `BackgroundActivatedEventArgs.TaskInstance.Task.Name` ermitteln können, welche Aufgabe initiiert wird.

Wenn Sie derzeit zwischen Vordergrund- und Hintergrundprozessen kommunizieren, können Sie diesen Zustandverwaltungs- und Kommunikationscode entfernen.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Hintergrundaufgaben und Triggertypen, die nicht konvertiert werden können

* In-Process-Hintergrundaufgaben unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe.
* Prozess interne Hintergrundaufgaben unterstützen die folgenden Trigger nicht: [deviceusetrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [deviceservicingtrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) und **iotstartuptask** .