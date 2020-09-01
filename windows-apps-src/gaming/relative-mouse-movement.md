---
title: Relative Mausbewegung
description: Verwenden Sie relative Maus Steuerelemente, die nicht den System Cursor verwenden und keine absoluten Bildschirm Koordinaten zurückgeben, um das Pixel Delta zwischen Mausbewegungen in spielen zu verfolgen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Maus, Eingabe
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: 155fed8e4bc1cb12196fcaaca4b7232b84c19032
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159254"
---
# <a name="relative-mouse-movement-and-corewindow"></a>Relative Mausbewegung und CoreWindow

In Spielen ist die Maus eine gängige Steuerungsoption, mit der viele Spieler vertraut sind. Sie ist ebenso für viele Spielgenres unentbehrlich, einschließlich der First-Person- und Third-Person-Shooter sowie der Echtzeitstrategiespiele. In diesem Thema wird die Implementierung relativer Maussteuerungen erläutert, die nicht den Systemcursor verwenden und keine absoluten Bildschirmkoordinaten zurückgeben, sondern stattdessen das Pixeldelta zwischen Mausbewegungen nachverfolgen.

Einige Apps, z. B. Spiele, verwenden die Maus als ein eher allgemeineres Eingabegerät. In einem 3D-Modellierer kann die Mauseingabe zum Ausrichten eines 3D-Objekts verwendet werden, indem ein virtueller Trackball simuliert wird. Oder in einem Spiel kann mit der Maus die Richtung der Kamera über Bewegungs-/Blicksteuerungen geändert werden. 

In diesen Szenarien benötigt die App relative Mausdaten. Relative Mauswerte geben an, welche Entfernung die Maus seit dem letzten Frame zurückgelegt hat, und stellen keine absoluten x- und y-Koordinaten in einem Fenster oder Bildschirm dar. Zudem blenden Apps häufig den Mauszeiger aus, da die Position des Cursors in Bezug auf die Bildschirmkoordinaten beim Bearbeiten eines 3D-Objekts oder einer 3D-Szene nicht relevant ist. 

Wenn der Benutzer eine Aktion durchführt, die die App in einen Bearbeitungsmodus für relative 3D-Objekte/-Szenen versetzt, muss die App folgende Aufgaben ausführen: 
- Ignorieren der standardmäßigen Mausbehandlung
- Aktivieren der relativen Mausbehandlung
- Ausblenden des Mauszeigers durch Festlegen eines NULL-Zeigers ("nullptr") 

Wenn der Benutzer eine Aktion durchführt, die den App-Bearbeitungsmodus für relative 3D-Objekte/-Szenen beendet, muss die App folgende Aufgaben ausführen: 
- Aktivieren der standardmäßigen/absoluten Mausbehandlung
- Deaktivieren der relativen Mausbehandlung 
- Festlegen des Mauszeigers auf einen Wert ungleich NULL (so dass der Mauszeiger sichtbar wird)

> **Hinweis**  
Bei diesem Muster wird die Position des absoluten Mauszeigers beim Wechsel in den relativen Modus ohne Cursor beibehalten. Der Mauszeiger erscheint erneut an einer Position mit den gleichen Bildschirmkoordinaten wie vor der Aktivierung des Modus für die relative Mausbewegung.

 

## <a name="handling-relative-mouse-movement"></a>Behandeln der relativen Mausbewegung


Führen Sie die Registrierung für das [MouseDevice::MouseMoved](/uwp/api/windows.devices.input.mousedevice.mousemoved)-Ereignis aus, um auf die relativen Mausdeltawerte zuzugreifen, so wie hier gezeigt.


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

Der Ereignishandler in diesem Codebeispiel, **OnMouseMoved**, rendert die Ansicht basierend auf den Bewegungen der Maus. Die Position des Mauszeigers wird als [MouseEventArgs](/uwp/api/Windows.Devices.Input.MouseEventArgs)-Objekt an den Handler übergeben. 

Überspringen Sie die Verarbeitung der absoluten Mausdaten aus dem [CoreWindow::PointerMoved](/uwp/api/windows.ui.core.corewindow.pointermoved)-Ereignis, wenn Ihre App in den Modus zur Behandlung relativer Mausbewegungswerte wechselt. Überspringen Sie diese Eingabe jedoch nur, wenn das **CoreWindow::PointerMoved**-Ereignis als Ergebnis einer Mauseingabe (und nicht einer Fingereingabe) aufgetreten ist. Der Cursor wird ausgeblendet, indem [CoreWindow::PointerCursor](/uwp/api/windows.ui.core.corewindow.pointercursor) auf **nullptr** festgelegt wird. 

## <a name="returning-to-absolute-mouse-movement"></a>Zurückkehren zur absoluten Mausbewegung

Wenn die App den Bearbeitungsmodus für 3D-Objekte/-Szenen verlässt und die relative Mausbewegung nicht mehr verwendet (wenn sie z. B. einen Menübildschirm anzeigt), sollte die App zur normalen Verarbeitung der absoluten Mausbewegung zurückkehren. Beenden Sie zu diesem Zeitpunkt das Lesen der relativen Mausdaten, starten Sie die Verarbeitung der standardmäßigen Maus- und Zeigerereignisse neu, und setzen Sie [CoreWindow::PointerCursor](/uwp/api/windows.ui.core.corewindow.pointercursor) auf einen Wert ungleich NULL. 

> **Hinweis**  
Wenn sich Ihre App im Bearbeitungsmodus für 3D-Objekte/-Szenen befindet, in dem relative Mausbewegungen bei deaktiviertem Cursor verarbeitet werden, kann die Maus keine UI am Bildschirmrand aufrufen, z. B. Charms, Stapel für die Rückwärtsnavigation oder App-Leiste. Daher ist es wichtig, einen Mechanismus bereitzustellen, um diesen besonderen Modus zu beenden, z. B. die allgemein verwendete **Esc**-Taste.

## <a name="related-topics"></a>Zugehörige Themen

* [Verschiebungs Steuerelemente für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [Toucheingabesteuerelemente für Spiele](tutorial--adding-touch-controls-to-your-directx-game.md)