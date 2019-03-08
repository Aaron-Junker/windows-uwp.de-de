---
title: Headset
description: Verwenden Sie die Windows.Gaming.Input-Headset-APIs, um Headsets zu erkennen, die Stimme von Spielern zu erfassen und Audio wiederzugeben.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Kopfhörer
ms.localizationpriority: medium
ms.openlocfilehash: b3de68cc59c9928a52eba5caeb840e9e825eecf0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627955"
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

Hier wird nicht ausführlich auf XAudio eingegangen. Weitere Informationen finden Sie in der [XAudio2-Programmieranleitung](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) und in der [Referenz zur XAudio2-API](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx).


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx
