---
title: Headset
description: Verwenden Sie die Windows.Gaming.Input-Headset-APIs, um Headsets zu erkennen, die Stimme von Spielern zu erfassen und Audio wiederzugeben.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Headset
ms.localizationpriority: medium
ms.openlocfilehash: 5e5b0894dbab03490cb74bdae8c231b57d397cb4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175294"
---
# <a name="headset"></a>Headset

In diesem Dokument wird die grundlegende Programmierung für Headsets unter Verwendung von [Windows.Gaming.Input.Headset][headset] und zugehöriger APIs für die universelle Windows-Plattform (UWP) beschrieben.

Auf dieser Seite erhalten Sie Informationen zu folgenden Vorgängen:
* Zugreifen auf ein Headset, das mit einem Eingabe- oder Navigationsgerät verbunden ist
* Ermitteln, ob ein Headset verbunden oder getrennt wurde


## <a name="headset-overview"></a>Übersicht über Headsets

Headsets sind Geräte für die Audioaufnahme und -wiedergabe und werden am häufigsten zur Kommunikation mit anderen Spielern in Onlinespielen verwendet, können aber auch im Spiel oder für andere kreative Aufgaben verwendet werden. Headsets werden in Windows 10 und UWP-Apps für Xbox durch den [Windows.Gaming.Input][]-Namespace unterstützt.


## <a name="detect-and-track-headsets"></a>Erkennen und Nachverfolgen von Headsets

Headsets werden vom System verwaltet. Daher müssen Sie diese nicht erstellen oder initialisieren. Das System ermöglicht den Zugriff auf ein Headset über das Eingabegerät, mit dem es verbunden ist. Außerdem stellt es Ereignisse bereit, durch die Sie benachrichtigt werden, wenn ein Headset verbunden oder getrennt wird.

### <a name="igamecontrollerheadset"></a>IGameController.Headset

Alle Eingabegeräte im [Windows.Gaming.Input][]-Namespace implementieren die [IGameController][]-Schnittstelle. Diese sieht vor, dass die [Headset][igamecontroller.headset]-Eigenschaft dem aktuell mit dem Gerät verbundenen Headset entspricht.

### <a name="connecting-and-disconnecting-headsets"></a>Verbinden und Trennen von Headsets

Das [HeadsetConnected][igamecontroller.headsetconnected]-Ereignis bzw. das [HeadsetDisconnected][igamecontroller.headsetdisconnected]-Ereignis wird ausgelöst, wenn ein Headset verbunden bzw. getrennt wird. Sie können Handler für diese Ereignisse registrieren, um nachzuverfolgen, ob derzeit ein Headset an ein Eingabegerät angeschlossen ist.

Das folgende Beispiel zeigt, wie ein Handler für das `HeadsetConnected`-Ereignis registriert wird.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

Das folgende Beispiel zeigt, wie ein Handler für das `HeadsetDisconnected`-Ereignis registriert wird.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>Verwenden des Headsets

Die [Headset][]-Klasse besteht aus zwei Zeichenfolgen, die XAudio-Endpunkt-IDs darstellen – eine für Audioaufnahmen (über das Headsetmikrofon) und eine für das Audiorendering (über das Ohrstück des Headsets).

Hier wird nicht ausführlich auf XAudio eingegangen. Weitere Informationen finden Sie in der [XAudio2-Programmieranleitung](/windows/desktop/xaudio2/programming-guide) und in der [Referenz zur XAudio2-API](/windows/desktop/xaudio2/programming-reference).


[Windows. Gaming. Input]: /uwp/api/Windows.Gaming.Input
[igamecontroller]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headset]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headsetconnected]: /uwp/api/Windows.Gaming.Input.IGameController
[igamecontroller.headsetdisconnected]: /uwp/api/Windows.Gaming.Input.IGameController
[Headset]: /uwp/api/Windows.Gaming.Input.Headset