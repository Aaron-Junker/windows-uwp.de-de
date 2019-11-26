---
title: Hinzufügen von Steuerelementen
description: In diesem Thema befassen wir uns damit, wie das Beispielspiel die Bewegungs-/Blicksteuerung in einem 3D-Spiel implementiert und wie einfache Steuerungen für Toucheingabe, Maus und Gamecontroller entwickelt werden.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Steuerelemente, Eingabe
ms.localizationpriority: medium
ms.openlocfilehash: 9c2b7031bf8afb047fcfc869e23ee1c398218af8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258430"
---
# <a name="add-controls"></a>Hinzufügen von Steuerelementen


\[ für UWP-apps unter Windows 10 aktualisiert. Informationen zu Windows 8. x-Artikeln finden Sie im [Archiv](https://docs.microsoft.com/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN) \]

Ein gutes universelles Windows Plattform (UWP)-Spiel unterstützt viele unterschiedliche Schnittstellen. Ein potenzieller Player kann Windows 10 auf einem Tablet ohne physische Schaltflächen, einem PC mit einem Xbox-Controller oder dem neuesten Desktop Gaming-Gerät mit einer Hochleistungs Tastatur für Maus und Spiele haben. In unserem Spiel werden die Steuerelemente in der [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp)-Klasse implementiert. Diese Klasse aggregiert alle drei Arten von Eingaben (Maus und Tastatur, Fingereingabe und Gamepad) in einem einzelnen Domänencontroller. Das Endergebnis ist ein First-Person-Shooter, der Standard-Bewegungs-/Blicksteuerungen des Genres verwendet, die mit mehreren Geräten funktionieren.

> [!NOTE]
> Weitere Informationen zu Steuerelementen finden Sie unter [Bewegungs-/Blicksteuerungen für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md) und [Toucheingabesteuerelemente für Spiele](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Ziel

Wir haben jetzt ein Spiel, das gerendert wird, aber nicht den Spieler bewegt oder Ziele anvisiert. Wir werden sehen, wie unser Spiel First-Person-Shooter Bewegungs-/Blicksteuerungen für die folgenden Arten von Eingaben in unserem UWP-DirectX -Spiel implementiert.
- Tastatur und Maus
- Toucheingabe
- Gamepad

>[!Note]
>Wenn Sie den neuesten Code für dieses Beispiel noch nicht heruntergeladen haben, wechseln Sie zu [Direct3D-Spielbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel gehört zu einer großen Sammlung von UWP-Featurebeispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter [Abrufen der UWP-Beispiele von GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Allgemeine Steuerungsverhalten


Die Implementierung von Fingereingabesteuerungen und Maus-/Tastatursteuerungen ist im Grunde sehr ähnlich. In einer UWP-App ist ein Zeiger einfach ein Punkt auf dem Bildschirm. Sie können ihn bewegen, indem Sie die Maus oder den Finger auf dem Touchscreen bewegen. Folglich können Sie einen einzelnen Satz von Ereignissen registrieren und müssen sich keine Gedanken darüber machen, ob der Spieler eine Maus oder einen Touchscreen zum Bewegen und Betätigen des Zeigers verwendet.

Beim Initialisieren der **MoveLookController**-Klasse im Beispielspiel werden vier zeigerspezifische Ereignisse und ein mausspezifisches Ereignis registriert:

Ereignis | Beschreibung
:------ | :-------
[**Corewindow::P ointerpressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed) | Die linke oder rechte Maustaste wurde gedrückt (und gedrückt gehalten), oder der Touchscreen wurde berührt.
[**Corewindow::P ointerverschoben**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) |Die Maus wurde bewegt, oder eine Zieh-Aktion wurde auf dem Touchscreen ausgeführt.
[**Corewindow::P ointerreleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) |Die linke Maustaste wurde losgelassen, oder das Objekt, das den Touchscreen berührt, wurde angehoben.
[**Corewindow::P ointerexited**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerexited) |Der Zeiger wurde aus dem Hauptfenster bewegt.
[**Windows::D EVICES:: Input:: mou\verschogs.** ](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved) | Die Maus wurde über eine bestimmte Distanz bewegt. Wir sind aber nur an Deltawerten der Mausbewegungen und nicht an der aktuellen x-y-Position interessiert.


Diese Ereignishandler werden beim Starten der Überwachung für die Benutzereingabe festgelegt, sobald **MoveLookController** im Anwendungsfenster initialisiert wird.
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

Der vollständige Code für [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) wird auf GitHub angezeigt.


Um festzulegen, ob das Spiel auf Eingabe warten soll, verfügt die **MoveLookController**-Klasse unabhängig vom Steuerungstyp über drei Controllerzustände:

Status | Beschreibung
:----- | :-------
**Keine** | Dies ist der Initialisierungszustand für den Controller. Das Spiel erwartet keine Controllereingabe und die Eingabe wird ignoriert.
**WaitForInput** | Der Controller wartet auf die Bestätigung der Nachricht des Spielers über das Spiel, entweder mit einem Mausklick, einem Touchereignis oder der Menütaste auf einem Gamepad.
**Active** | Der Controller ist im aktiven Spiel-Modus.



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput-Status und Anhalten des Spiels

Das Spiel wechselt in den **WaitForInput**-Zustand, wenn das Spiel angehalten wurde. Dies passiert, wenn der Spieler den Zeiger aus dem Hauptfenster des Spiels bewegt oder die Pause-Taste drückt (P-TASTE oder **Start**-Taste auf dem Gamepad), drückt. Die **MoveLookController-Instanz** registriert die Tastenbetätigung und informiert die Spielschleife, wenn sie die [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127)-Methode aufruft. Wenn **IsPauseRequested** jetzt **true** zurückgibt, ruft die Spielschleife **WaitForPress** für die **MoveLookController**-Instanz auf, um den Controller in den Zustand **WaitForInput** zu versetzen. 


Im **WaitForInput**-Zustand beendet das Spiel die Verarbeitung von fast allen Eingabeereignissen, bis der Controller wieder in den **Active**-Zustand übergeht. Eine Ausnahme ist die Pause-Taste, da durch Drücken der Taste das Spiel zum aktiven Zustand zurückkehret. Abgesehen von der Schaltfläche Pause, damit das Spiel wieder in den **aktiven** Zustand wechselt, muss der Spieler ein Menü Element auswählen. 



### <a name="the-active-state"></a>Der aktive Zustand

Im **aktiven** Zustand verarbeitet die **MoveLookController**-Instanz Eingabeereignisse von allen aktivierten Eingabegeräten und interpretiert die Absichten des Spielers. Dadurch aktualisiert sie die Geschwindigkeit und Blickrichtung der Spieleransicht und gibt die aktualisierten Daten an das Spiel weiter, nachdem **Update** in der Spielschleife aufgerufen wurde.


Mauseingaben werden im Zustand **Active** mit verschiedenen Zeiger-IDs für die unterschiedlichen Zeigeraktionen nachverfolgt.
Wenn ein [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed)-Ereignis empfangen wird, ruft die **MoveLookController**-Instanz den vom Fenster erstellten Wert der Zeiger-ID ab. Die Zeiger-ID stellt einen bestimmten Eingabetyp dar. Bei einem Multitouchgerät sind etwa gleichzeitig mehrere unterschiedliche aktive Eingaben möglich. Die IDs werden verwendet, um den vom Spieler verwendeten Eingabetyp nachzuverfolgen. Wenn ein Ereignis im Bewegungsrechteck des Touchscreens stattfindet, wird eine Zeiger-ID zugewiesen, um alle Zeigerereignisse im Bewegungsrechteck nachzuverfolgen. Andere Zeigerereignisse im Schießrechteck werden separat mit einer anderen Zeiger-ID nachverfolgt.


> [!NOTE]
> Eingaben von Maus und dem rechten Ministick auf einem Gamepad verfügen auch über die IDs, die separat behandelt werden.

Nachdem die Zeigerereignisse einer bestimmten Spielaktion zugeordnet wurden, müssen die Daten aktualisiert werden, die das **MoveLookController**-Objekt an die Hauptspielschleife weitergibt.

Wenn Sie aufgerufen wird, verarbeitet die [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) -Methode im Spielbeispiel die Eingabe und aktualisiert die Variablen für die Geschwindigkeit und die Ausrichtung (**m\_Velocity** und **m\_LookDirection**), die von der Spiel Schleife durch Aufrufen der öffentlichen [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) -und [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) -Methoden abgerufen werden.

> [!NOTE]
> Weitere Informationen zur [**Update**](#the-update-method)-Methode finden Sie später auf dieser Seite.




Die Spielschleife kann prüfen, ob der Spieler schießt, indem sie die **IsFiring**-Methode der **MoveLookController**-Instanz aufruft. Die **MoveLookController**-Instanz überprüft, ob der Spieler über einen der drei Eingabetypen die Schießtaste betätigt hat.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```









Jetzt wollen wir uns etwas ausführlicher mit der Implementierung der drei Steuerungstypen beschäftigen.

## <a name="adding-relative-mouse-controls"></a>Hinzufügen relativer Maussteuerungen


Wenn Mausbewegungen erkannt werden, sollen diese Bewegungen zum Ermitteln des neuen Neigungs- und Schwenkwinkels der Kamera verwendet werden. Hierzu implementieren wir relative Maussteuerungen, bei denen nicht die absoluten x-y-Pixelkoordinaten der Bewegung aufgezeichnet werden, sondern die relative Distanz der Mausbewegung (also das Delta zwischen Start und Ende der Bewegung) erfasst wird.

Dazu ermitteln wir die Änderung der X-Koordinate (horizontale Bewegung) und der Y-Koordinate (vertikale Bewegung), indem wir die Felder [**MouseDelta::X**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseDelta) und **MouseDelta::Y** für das vom [**MouseMoved**](https://docs.microsoft.com/uwp/api/windows.devices.input.mouseeventargs.mousedelta)-Ereignis zurückgegebene [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved)-Argumentobjekt überprüfen.

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>Hinzufügen der Toucheingabeunterstützung

Toucheingabesteuerungen sind ideal für Benutzer mit Tablets. Dieses Spiel sammelt Toucheingaben durch die Zonenzuweisung bestimmter Bereiche des Bildschirms mit jeder Ausrichtung für bestimmte in-Spieleaktionen.
Die Toucheingabe des Spiels verwendet drei Zonen.

![Bewegung Ansicht Toucheingabe - Layout](images/simple-dx-game-controls-touchzones.png)

Die folgenden Befehle fassen unser Verhalten für die Toucheingabesteuerelement zusammen.
Benutzereingabe | Aktion
:------- | :--------
Bewegungsrechteck | Die Toucheingabe wird in einen virtuellen Joystick konvertiert, in dem die vertikale Bewegung in die Bewegung zur Vorwärts- und Rückwärtsbewegung übersetzt wird und die horizontale Bewegung in Bewegungen nach links/rechts übersetzt werden.
Schießrechtecke | Mit der Maustaste schießen
Touch außerhalb der Bewegungs- und Schießrechtecke | Ändern Sie die Drehung (Neigungs- und Schwenkwinkel) der Kameraansicht.

Die **MoveLookController**-Instanz überprüft anhand der Zeiger-ID, wo das Ereignis aufgetreten ist, und führt eine der folgenden Aktionen aus:

-   Wenn das [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved)-Ereignis im Bewegungs- oder Schießrechteck aufgetreten ist, wird die Zeigerposition für den Controller aktualisiert.
-   Wenn das [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved)-Ereignis an einer anderen Stelle des Bildschirms (definiert als Blicksteuerung) aufgetreten ist, werden die Änderungen des Nick- und Gierwinkels des Blickrichtungsvektors berechnet.


Nachdem wir unsere Toucheingabesteuerungen implementiert haben, werden die Rechtecke, die wir zuvor mithilfe von Direct2D gezeichnet haben dem Spieler anzeigen, wo er sich in den Bereichen bewegen, schießen und hinsehen soll.


![Toucheingabesteuerungen](images/simple-dx-game-controls-showzones.png)


Sehen wir uns an, wie jedes Steuerelement implementiert wird.


### <a name="move-and-fire-controller"></a>Bewegungs- und Schieß-Controller
Das Bewegungsrechteck des Controllers im unteren linken Quadranten des Bildschirms wird als Steuerkreuz verwendet. Wenn Sie den Daumen nach links und rechts auf dieser Fläche bewegen, wird der Spieler nach links und rechts bewegt, und eine Bewegung nach oben oder unten bewegt die Kamera vorwärts und rückwärts.
Nach dem Einrichten wird durch das Tippen auf den Schieß-Controller im unteren rechten Quadrant des Bildschirms geschossen.

Mit den [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) und [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867)-Methoden werden die Eingaberechtecke mit zwei Methoden erstellt, indem zwei 2D-Vektoren an jedem Rechtecke oben links und unten rechts unten auf dem Bildschirm positioniert werden. 


Die Parameter werden dann **m\_fireupperleft** und **m\_firelowerright** zugewiesen, damit wir ermitteln können, ob der Benutzer innerhalb der Rechtecke berührt. 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

Wenn die Bildschirmgröße geändert wird, werden die Rechtecke neu auf die Größe angepasst.


Nun, da wir unsere Steuerelemente in Zonen haben, ist es Zeit zu bestimmen, ob ein Benutzer diese tatsächlich verwendet.
Hierzu legen wir einige Ereignishandler in der **MoveLookController::InitWindow**-Methode fest, wenn der Benutzer den Mauszeiger drückt, verschiebt oder freigibt.

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


Wir ermitteln, was geschieht, wenn der Benutzer zunächst auf die Bewegungs- oder Schießrechtecke drückt, mithilfe der [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313)-Methode.
Hier überprüfen wir, wo ein Steuerelement berührt wird und ob ein Zeiger bereits in diesem Controller vorhanden ist. Wenn dies der erste Finger ist. der das spezifische Steuerelement berührt, gehen wir wie folgt vor.
- Speichern Sie den Speicherort des Touchdown in **m\_"muvefirstdown** " oder " **m"\_"firefirstdown** " als 2D-Vektor.
- Weisen Sie die Zeiger-ID m\_die Datei " **muvepointerid** " oder " **m\_firepointerid**" zu.
- Legen Sie das richtige **InUse** -Flag (**m\_moveinuse** oder **m\_fireinuse**) auf `true` fest, da wir jetzt über einen aktiven Zeiger für dieses Steuerelement verfügen.


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


Nachdem wir ermittelt haben, ob der Benutzer ein Steuerelement verschiebt oder schießt, sehen wir, ob der Spieler alle Bewegungen mit dem Finger macht.
Mithilfe der [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395)-Methode überprüfen wir, welche Zeiger verschoben wurden, und speichern dann die neue Position als 2D-Vektor.  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


Nachdem der Benutzer seine Gesten in den Steuerelementen vorgenommen hat, lassen sie den Zeiger los. Mithilfe der [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500)-Methode ermitteln wir, welche Zeiger freigegeben wurden und führen eine Reihe von Zurücksetzungen durch.


Wenn das Steuerelement freigegeben wurde, gehen wir wie folgt vor.
- Legen Sie die Geschwindigkeit des Players auf `0` in alle Richtungen fest, um zu verhindern, dass Sie im Spiel verschoben werden.
- Switch **m\_moveinuse** to `false`, da der Benutzer den Verschiebungs Controller nicht mehr berührt.
- Legen Sie die Bewegungszeiger-ID auf `0` fest, da der Bewegungscontroller keinen Zeiger mehr hat.

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


Wenn das Schießsteuerelement ausgelöst wird, legen wir lediglich die **M_fireInUse**-Kennzeichnung auf `false` fest und die Schießzeiger-ID auf `0`, da im Schießsteuerelement kein Zeiger mehr vorhanden ist.
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>Blickrichtungscontroller
Zeigerereignisse von Fingereingabegeräten für die offene Bildschirmbereiche werden als Blickrichtungscontroller behandelt. Das Bewegen Ihrer Finger über diese Zone ändert den Neigungswinkel und Schwenkwinkel (Drehung) der Spieler-Kamera.


Wird in einem dieser Bereiche ein **MoveLookController::OnPointerPressed**-Ereignis von einem Toucheingabegerät ausgelöst, während sich das Spiel im Zustand **Active** befindet, wird ihm wie zuvor erläutert eine Zeiger-ID zugewiesen.

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
Sehen Sie den vollständigen Code für die **MoveLookController::OnPointerPressed**-Methode auf [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259).




Hier weist **MoveLookController** die Zeiger-ID für den Zeiger zu, der das Ereignis für eine bestimmte Variable ausgelöst hat, die dem Blickbereich entspricht. Im Fall einer Berührung, die im Suchbereich auftritt, wird die **m\_lookpointerid** -Variable auf die Zeiger-ID festgelegt, die das Ereignis ausgelöst hat. Eine boolesche Variable, **m\_lookinuse**, wird ebenfalls festgelegt, um anzugeben, dass das Steuerelement noch nicht freigegeben wurde.

Im nächsten Schritt erfahren Sie, wie das Beispielspiel das [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved)-Touchscreenereignis behandelt.


In der **MoveLookController::OnPointerMoved**-Methode überprüfen wir, welche Art von Zeiger-ID dem Ereignis zugewiesen wurde. Ist es **m_lookPointerID**, berechnen wir die Änderung an der Position des Zeigers.
Wir verwenden dann Deltas, um zu berechnen, wie die Drehung geändert werden muss. Schließlich sind wir an einem Punkt, an dem wir die **m\_** -und **m-\_Yaw** aktualisieren können, um die Spieler Drehung zu ändern. 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





Zum Schluss sehen wir uns an, wie das Beispielspiel das [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased)-Touchscreenereignis behandelt.
Nachdem der Benutzer die Geste abgeschlossen und den Finger vom Bildschirm entfernt hat, wird [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) initiiert.
Wenn die ID des Zeigers, von dem das [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased)-Ereignis ausgelöst wurde, die ID des zuvor erfassten Mauszeigers ist, legt die **MoveLookController**-Instanz die Geschwindigkeit auf `0` fest, da der Spieler den Blickbereich nicht mehr berührt.

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>Hinzufügen von Tastatur- und Mausunterstützung


Dieses Spiel hat das folgende Steuerelementlayout für Tastatur und Maus.

Benutzereingabe | Aktion
:------- | :--------
W | Spieler vorwärts
A | Spieler nach links
S | Spieler rückwärts
D | Spieler nach rechts
X | Ansicht nach oben
Leertaste | Ansicht nach unten
P | Hält das Spiel an
Mausbewegung | Ändern Sie die Drehung (Neigungs- und Schwenkwinkel) der Kameraansicht
Linke Maustaste | Mit der Maustaste schießen


Um die Tastatur zu verwenden, registriert das Beispielspiel zwei neue Ereignisse, [**CoreWindow: KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keyup) und [**CoreWindow:: KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keydown) in der [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88)-Methode. Diese Ereignisse behandeln das Drücken und Loslassen einer Taste.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

Die Behandlung der Maus unterscheidet sich etwas von der der Fingereingabesteuerung, obwohl sie einen Zeiger verwendet. Um unsere Steuerelementlayout anzupassen, dreht der **MoveLookController** die Kamera, wenn die Maus bewegt wird, und schießt, wenn die linke Maustaste gedrückt wird.


Dies erfolgt in der [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313)-Methode des **MoveLookController**.

In dieser Methode überprüfen wir, welche Art von Zeigegerät verwendet wird durch Verwendung der [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType)-Enumeration. Wenn das Spiel **aktiv** ist und **PointerDeviceType** nicht **Touch** ist, ist dies die Mauseingabe.

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            break;
        }
```

Wenn der Spieler aufhört, eine der Maustasten zu drücken, wird das [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased)-Mausereignis aufgerufen, indem die [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500)-Methode aufgerufen wird. Die Eingabe ist dann abgeschlossen. An diesem Punkt werden keine Kugeln mehr ausgelöst, wenn die linke Maustaste gedrückt und freigegeben wird. Da die Blicksteuerung immer aktiviert ist, verwendet das Spiel weiter den gleichen Mauszeiger, um die Blickereignisse nachzuverfolgen.

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```



Jetzt sehen wir uns den letzten unterstützen Steuerelementtyp an: Gamepads. Gamepads werden getrennt von den Fingereingabe- und Maussteuerungen behandelt, da sie kein Zeigerobjekt verwenden. Aus diesem Grund müssen ein paar neue Ereignishandler und Methoden hinzugefügt werden.

## <a name="adding-gamepad-support"></a>Hinzufügen von Gamepad-Unterstützung


Für dieses Spiel wird die Gamepad-Unterstützung durch Aufrufen der [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)-APIs hinzugefügt. Dieser Satz von APIs bietet Zugriff auf Gamecontrollereingaben, wie Rennlenkräder und Flight-Sticks. 


Hier sind unsere Gamepad-Steuerelemente.

Benutzereingabe | Aktion
:------- | :--------
Linker Joystick | Spieler bewegen
Rechter Joystick | Ändern Sie die Drehung (Neigungs- und Schwenkwinkel) der Kameraansicht
Rechter Trigger | Mit der Maustaste schießen
Ein-/Aus-Menütaste | Anhalten oder Fortsetzen des Spiels




In der [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103)-Methode fügen wir zwei neue Ereignisse hinzu, um festzustellen, ob ein Gamepad [hinzugefügt](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) oder [entfernt](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114) wurde. Diese Ereignisse aktualisieren die **m_gamepadsChanged**-Eigenschaft. Dies wird in der **updatepollingdevices** -Methode verwendet, um zu überprüfen, ob sich die Liste der bekannten Gamepads geändert hat. 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

> [!NOTE]
> UWP-Apps können keine Eingaben von einem Xbox One-Controller empfangen, während die APP nicht den Fokus hat.

### <a name="the-updatepollingdevices-method"></a>Die UpdatePollingDevices-Methode

Die [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782)-Methode der **MoveLookController**-Instanz sofort überprüft, ob ein Gamepad verbunden ist. Wenn ja, beginnen wir den Zustand mit [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) zu lesen. Dies gibt die [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading)-Struktur zurück, um zu überprüfen, welche Schaltflächen aktiviert oder Ministicks verschoben wurden.


Wenn der Zustand des Spiels **WaitForInput** ist, warten wir auf die Ein-/Aus-Menütaste des Controllers, damit das Spiel fortgesetzt werden kann.


Ist es **aktiv**, überprüfen wir die Eingabe des Benutzers und bestimmen, welche Aktion im Spiel erfolgen muss.
Wenn der Benutzer den linken Joystick in eine bestimmte Richtung verschiebt, weiß das Spiel z. B., dass wir den Spieler in die Richtung zu verschieben müssen, in die der Stick verschoben wird. Die Bewegung des Sticks in eine Richtung muss aber größer sein als der Radius des **inaktiven Bereichs**. Andernfalls passiert nichts. Dieser Radius des inaktiven Bereichs ist erforderlich, um "Drifting" zu vermeiden, d. h. vom Controller erkannte winzige Bewegungen des Daumens, wenn dieser auf dem Stick liegt. Ohne inaktive Zonen können die Steuerelemente für den Benutzer zu sensibel sein.


Ministick-Eingabe ist zwischen-1 und 1 für die x- und y-Achse. Die folgenden Konstanten geben den Radius des inaktiven Ministicks an.

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

Mit dem Verwenden dieser Variable werden wir dann mit der Verarbeitung der Eingaben des relevanten Ministicks beginnen. Die Bewegung tritt bei einem Wert von [-1, 0,26] oder [0,26, 1] auf jeder Achse auf.

![Inaktive Zonen für Ministicks](images/simple-dx-game-controls-deadzone.png)

Dieser Teil der **UpdatePollingDevices**-Methode behandelt den linken und rechten Ministick.
Jede X- und Y-Wert des Sticks wird überprüft, um festzustellen, ob er außerhalb des inaktiven Bereichs ist. Wenn einer oder beide inaktiv sind, aktualisieren wir die entsprechende Komponente.
Wenn beispielsweise der linke Ministick nach links entlang der x-Achse verschoben wird, wird -1 der **x**-Komponente des **m_moveCommand**-Vektors hinzugefügt. Dieser Vektor wird verwendet, um alle Eingänge auf allen Geräten zu aggregieren und wird später verwendet, um zu berechnen, wo der Spieler sich hin bewegen soll. 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

Ähnlich wie der linke Stick, der die Bewegung kontrolliert, steuert der rechte Stick die Drehung der Kamera.


Das Verhalten des rechten Sticks wird dem Verhalten der Mausbewegung im Steuerelement-Setup von Maus und Tastatur ausgerichtet.
Wenn der Stick außerhalb des inaktiven Bereichs ist, berechnen wir die Differenz zwischen der aktuellen Zeigerposition und der Blickposition des Benutzers.
Diese Änderung der Zeigerposition (**PointerDelta**) wird zum Upgrade des Neigungs- und Schwenkwinkels der Kameradrehung verwendet, die zu einem späteren Zeitpunkt auf die **aktualisierte** Methode angewendet wird.
Der **PointerDelta**-Vektor sieht vertraut aus, da er auch in der [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395)-Methode zum Nachverfolgen verwendet wird, um Änderung der Zeigerposition für unsere Maus und Toucheingaben nachzuverfolgen.


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Das Steuerelemente des Spiels wären nicht ohne die Möglichkeit zu Schießen abgeschlossen!


Diese **UpdatePollingDevices**-Methode überprüft auch, ob der rechte Trigger gedrückt wird. Wenn dies der Fall ist, wird unsere **m_firePressed**-Eigenschaft auf "Wahr" festgelegt, um dem Spiel anzuzeigen, dass Kugeln ausgelöst werden sollen.
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>Die Update-Methode

Zum Abschluss befassen wir uns tiefer mit der [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096)-Methode.
Diese Methode führt alle Bewegungen oder Drehungen aus, die der Spieler bei allen unterstützten Eingaben ausführt, um den Geschwindigkeitsvektor zu generieren und unsere Schusshöhe zu aktualisieren, und Werte für den Zugriff auf die Spielschleife vorzunehmen.


Die **Update**-Methode startet Elemente durch Aufrufen von [**UpdatePollingDevices**](#the-updatepollingdevices-method), um den Status des Controllers zu aktualisieren. Diese Methode sammelt auch alle Eingaben von einem Gamepad und fügt die Bewegungen dem **m_moveCommand**-Vektor hinzu. 

In unserer **Update**-Methode überprüfen wir dann die folgende Eingabe.
- Wenn der Spieler das Bewegungsrechteck des Controllers verwendet, wird die Änderung der Zeigerposition ermittelt und wir verwenden diese, um zu berechnen, ob der Benutzer den Mauszeiger aus dem inaktiven Bereich des Controller entfernt hat. Wenn sich der Mauszeiger außerhalb des inaktiven Bereichs befindet, wird die **m_moveCommand**-Vektoreigenschaft mit dem virtuellen Joystick-Wert aktualisiert.
- Wenn eine der Verschiebungs Tastatureingaben gedrückt wird, wird der Wert `1.0f` oder `-1.0f` in der entsprechenden Komponente des **m_moveCommand** Vector hinzugefügt &mdash; `1.0f` für vorwärts und `-1.0f` für rückwärts.


Nachdem alle Bewegungseingaben berücksichtigt wurden, führen wird den **m_moveCommand**-Vektor über einige Berechnungen aus, um einen neuen Vektor zu generieren, der die Richtung des Spielers in der Spielwelt darstellt.
Wir berechnen unsere Bewegungen in Bezug auf die ganze Welt und wenden sie auf den Spieler als Geschwindigkeit in die entsprechende Richtung an.
Schließlich setzen wir den **m_moveCommand**-Vektor auf `(0.0f, 0.0f, 0.0f)` zurück, damit alles für den nächsten Frame des Spiels bereit ist.


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>Nächste Schritte

Nachdem wir unsere Steuerelemente hinzugefügt haben, gibt es eine weitere Funktion, die wir hinzufügen müssen, um ein immersives Spiel zu erstellen: Sound!
Musik und Soundeffekte sind bei jedem Spiel wichtig. Befassen wir uns also mit dem [Hinzufügen von Sound](tutorial--adding-sound.md).
 

 

 




