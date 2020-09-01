---
title: Hinzufügen von Steuerelementen
description: Nun sehen wir uns an, wie das Beispiel Spiel die Steuerelemente für das Verschieben von Steuerelementen in einem 3D-Spiel implementiert und wie einfache Steuerelemente für Finger Eingaben, Maus und Spielcontroller entwickelt werden.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Steuerelemente, Eingabe
ms.localizationpriority: medium
ms.openlocfilehash: 87a56c9213aabce23801f305d8a100f536f0889b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175234"
---
# <a name="add-controls"></a>Hinzufügen von Steuerelementen

> [!NOTE]
> Dieses Thema ist Teil der Tutorial-Reihe zum [Erstellen eines einfachen universelle Windows-Plattform (UWP) mit DirectX](tutorial--create-your-first-uwp-directx-game.md) . Das Thema unter diesem Link legt den Kontext für die Reihe fest.

\[ Aktualisiert für UWP-apps unter Windows 10. Informationen zu Windows 8. x-Artikeln finden Sie im [Archiv](/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

Ein gutes universelle Windows-Plattform-Spiel (UWP) unterstützt eine Vielzahl von Schnittstellen. Ein potenzieller Player kann Windows 10 auf einem Tablet ohne physische Schaltflächen, einem PC mit einem Xbox-Controller oder dem neuesten Desktop Gaming-Gerät mit einer Hochleistungs Tastatur für Maus und Spiele haben. In unserem Spiel werden die [**Steuer**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) Elemente in der Klasse "Klasse" von "Klasse" implementiert. Diese Klasse aggregiert alle drei Eingabetypen (Maus und Tastatur, Touchpad und Gamepad) in einem einzelnen Controller. Das Endergebnis ist ein Erstbenutzer-Shooter, das Genre Standard-Steuerelemente zum Verschieben von Steuerelementen verwendet, die mit mehreren Geräten funktionieren.

> [!NOTE]
> Weitere Informationen zu Steuerelementen finden Sie unter [Move-Look-Steuerelemente für Spiele](tutorial--adding-move-look-controls-to-your-directx-game.md) und [Berührungs Steuerelemente für Spiele](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Ziel

An dieser Stelle haben wir ein Spiel, das rendert, aber wir können den Player nicht um die Ziele herum verschieben. Wir werfen einen Blick darauf, wie unser Spiel die Move-Look-Steuerelemente von First Person-Steuerelementen für die folgenden Typen von Eingaben im UWP DirectX-Spiel implementiert.
- Maus und Tastatur
- Touch
- Gamepad

>[!Note]
>Wenn Sie den neuesten Spiel Code für dieses Beispiel nicht heruntergeladen haben, besuchen Sie das [Direct3D-Beispiel Spiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Dieses Beispiel ist Teil einer großen Auflistung von UWP-Funktions Beispielen. Anweisungen zum Herunterladen des Beispiels finden Sie unter herunterladen [der UWP-Beispiele von GitHub](../get-started/get-app-samples.md).

## <a name="common-control-behaviors"></a>Allgemeine Verhaltensweisen von Steuerelementen


Die Implementierung von Touchsteuerelementen und Maus-/Tastatursteuerelementen ist im Grunde sehr ähnlich. In einer UWP-App ist ein Zeiger einfach ein Punkt auf dem Bildschirm. Sie können ihn bewegen, indem Sie die Maus oder den Finger auf dem Touchscreen bewegen. Folglich können Sie einen einzelnen Satz von Ereignissen registrieren und müssen sich keine Gedanken darüber machen, ob der Spieler eine Maus oder einen Touchscreen zum Bewegen und Betätigen des Zeigers verwendet.

Wenn die **Klasse "** Klasse" im Beispiel Spiel initialisiert wird, registriert Sie sich für vier Zeiger spezifische Ereignisse und ein Maus spezifisches Ereignis:

Ereignis | BESCHREIBUNG
:------ | :-------
[**Corewindow::P ointerpressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed) | Die linke oder rechte Maustaste wurde gedrückt (und gedrückt gehalten), oder der Touchscreen wurde berührt.
[**Corewindow::P ointerverschoben**](/uwp/api/windows.ui.core.corewindow.pointermoved) |Die Maus wurde bewegt, oder eine Zieh-Aktion wurde auf dem Touchscreen ausgeführt.
[**Corewindow::P ointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) |Die linke Maustaste wurde losgelassen, oder das Objekt, das den Touchscreen berührt, wurde angehoben.
[**Corewindow::P ointerexited**](/uwp/api/windows.ui.core.corewindow.pointerexited) |Der Zeiger wurde aus dem Hauptfenster bewegt.
[**Windows::D EVICES:: Input:: mou\verschogs.**](/uwp/api/windows.devices.input.mousedevice.mousemoved) | Die Maus wurde über eine bestimmte Distanz bewegt. Beachten Sie, dass wir nur für die Delta Werte der Mausbewegung und nicht für die aktuelle X-Y-Position interessiert sind.


Diese Ereignishandler werden so festgelegt, dass Sie mit dem lauschen auf Benutzereingaben beginnen, sobald der " **muvelookcontroller** " im Anwendungsfenster initialisiert wird.
```cppwinrt
void MoveLookController::InitWindow(_In_ CoreWindow const& window)
{
    ResetState();

    window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

    window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

    window.PointerReleased({ this, &MoveLookController::OnPointerReleased });

    window.PointerExited({ this, &MoveLookController::OnPointerExited });

    ...

    // There is a separate handler for mouse-only relative mouse movement events.
    MouseDevice::GetForCurrentView().MouseMoved({ this, &MoveLookController::OnMouseMoved });

    ...
}
```

Vollständiger Code für [**initwindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) finden Sie auf GitHub.


Um zu ermitteln, wann das Spiel auf bestimmte Eingaben lauschen soll, verfügt **die Klasse "** Klasse" über drei Controller spezifische Zustände, unabhängig vom Controllertyp:

State | BESCHREIBUNG
:----- | :-------
**None** | Dies ist der Initialisierungszustand für den Controller. Alle Eingaben werden ignoriert, da das Spiel keine Controller Eingaben erwartet.
**WaitForInput** | Der Controller wartet darauf, dass der Spieler eine Nachricht vom Spiel bestätigt, indem er entweder einen linken Mausklick, ein touchereignis, die Menü Schaltfläche in einem Gamepad verwendet.
**Aktiv** | Der Controller befindet sich im Modus für aktive Spiele Wiedergabe.



### <a name="waitforinput-state-and-pausing-the-game"></a>Waitforinput-Status und Anhalten des Spiels

Das Spiel wechselt in den Status " **waitforinput** ", wenn das Spiel angehalten wurde. Dies geschieht, wenn der Player den Mauszeiger außerhalb des Hauptfensters des Spiels bewegt oder die Schaltfläche zum Anhalten drückt (die P-Taste oder die Gamepad-Schaltfläche " **Start** "). Der " **muvelookcontroller** " registriert den Press und informiert die Spiel Schleife, wenn die [**ispauserequessierte**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) Methode aufgerufen wird. Wenn **ispauserequtzig** den Wert " **true**" zurückgibt, ruft die Game-Schleife dann **waitforpress** auf dem **movelookcontroller** auf, um den Controller in den Status " **waitforinput** " zu verschieben. 


Einmal im Zustand " **waitforinput** " hält das Spiel die Verarbeitung fast aller Spiele Eingabeereignisse an, bis es in den **aktiven** Zustand zurückkehrt. Die Ausnahme ist die Pause-Taste, bei der das Spiel wieder in den aktiven Zustand wechselt. Abgesehen von der Schaltfläche Pause, damit das Spiel wieder in den **aktiven** Zustand wechselt, muss der Spieler ein Menü Element auswählen. 



### <a name="the-active-state"></a>Der aktive Status

Während des **aktiven** Zustands verarbeitet die **-Instanz von** "" die-Instanz von "" die-Instanz von allen aktivierten Eingabegeräten und interpretiert die Absichten des Players. Dadurch wird die Geschwindigkeit und die Anzeige Richtung der Player Ansicht aktualisiert, und die aktualisierten Daten werden mit dem Spiel geteilt, nachdem das **Update** von der Spiel Schleife aufgerufen wurde.


Alle Zeiger Eingaben werden im **aktiven** Zustand nachverfolgt, wobei verschiedene Zeiger-IDs verschiedenen Zeiger Aktionen entsprechen.
Wenn ein [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed)-Ereignis empfangen wird, ruft die **MoveLookController**-Instanz den vom Fenster erstellten Wert der Zeiger-ID ab. Die Zeiger-ID stellt einen bestimmten Eingabetyp dar. Bei einem Multitouchgerät sind etwa gleichzeitig mehrere unterschiedliche aktive Eingaben möglich. Die IDs werden verwendet, um den vom Spieler verwendeten Eingabetyp nachzuverfolgen. Wenn sich ein Ereignis im Verschiebungs Rechteck des Touchscreens befindet, wird eine Zeiger-ID zugewiesen, um Zeiger Ereignisse im Verschiebungs Rechteck zu verfolgen. Andere Zeigerereignisse im Schießrechteck werden separat mit einer anderen Zeiger-ID nachverfolgt.


> [!NOTE]
> Eingaben aus dem Mauszeiger und dem rechten Finger Stick eines Gamepad verfügen auch über IDs, die separat behandelt werden.

Nachdem die Zeigerereignisse einer bestimmten Spielaktion zugeordnet wurden, müssen die Daten aktualisiert werden, die das **MoveLookController**-Objekt an die Hauptspielschleife weitergibt.

Wenn Sie aufgerufen wird, verarbeitet die [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) -Methode im Beispiel Spiel die Eingabe und aktualisiert die Variablen für die Geschwindigkeit und die Ausrichtung (**m \_ Velocity** und **m \_ LookDirection**), die von der Spiel Schleife durch Aufrufen der öffentlichen [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) -und [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) -Methoden abgerufen werden.

> [!NOTE]
> Weitere Informationen zur [**Update**](#the-update-method) Methode finden Sie weiter unten auf dieser Seite.




Die Spielschleife kann prüfen, ob der Spieler schießt, indem sie die **IsFiring**-Methode der **MoveLookController**-Instanz aufruft. Die **MoveLookController**-Instanz überprüft, ob der Spieler über einen der drei Eingabetypen die Schießtaste betätigt hat.

```cppwinrt
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

Betrachten wir nun die Implementierung der drei Steuerelement Typen in etwas ausführlicheren Details.

## <a name="adding-relative-mouse-controls"></a>Hinzufügen relativer Maus Steuerelemente


Wenn die Mausbewegung erkannt wird, möchten wir diese Bewegung verwenden, um die neue Tonhöhe und die Kamera zu bestimmen. Hierzu implementieren wir relative Maussteuerungen, bei denen nicht die absoluten x-y-Pixelkoordinaten der Bewegung aufgezeichnet werden, sondern die relative Distanz der Mausbewegung (also das Delta zwischen Start und Ende der Bewegung) erfasst wird.

Dazu ermitteln wir die Änderung der X-Koordinate (horizontale Bewegung) und der Y-Koordinate (vertikale Bewegung), indem wir die Felder [**MouseDelta::X**](/uwp/api/Windows.Devices.Input.MouseDelta) und **MouseDelta::Y** für das vom [**MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved)-Ereignis zurückgegebene [**Windows::Device::Input::MouseEventArgs::MouseDelta**](/uwp/api/windows.devices.input.mouseeventargs.mousedelta)-Argumentobjekt überprüfen.

```cppwinrt
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice const& /* mouseDevice */,
    _In_ MouseEventArgs const& args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args.MouseDelta().X);
        mouseDelta.y = static_cast<float>(args.MouseDelta().Y);

        XMFLOAT2 rotationDelta;
        // Scale for control sensitivity.
        rotationDelta.x = mouseDelta.x * MoveLookConstants::RotationGain;
        rotationDelta.y = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in sane range by wrapping.
        if (m_yaw > XM_PI)
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

## <a name="adding-touch-support"></a>Hinzufügen von Berührungs Unterstützung

Fingereingabe-Steuerelemente eignen sich hervorragend für die Unterstützung von Benutzern Dieses Spiel sammelt toucheingaben, indem bestimmte Bereiche des Bildschirms abgefasst werden, wobei jede auf bestimmte in-Game-Aktionen ausgerichtet wird.
Die Berührungs Eingabe dieses Spiels verwendet drei Zonen.

![Look-Fingerabdruck Layout verschieben](images/simple-dx-game-controls-touchzones.png)

Die folgenden Befehle fassen das Verhalten des Berührungs Steuer Elements zusammen.
Benutzereingabe | Aktion
:------- | :--------
Rechteck verschieben | Die Fingereingabe wird in einen virtuellen Joystick konvertiert, bei dem die vertikale Bewegung in vorwärts-/rückwärts Positions Bewegung übersetzt wird und die horizontale Bewegung in eine Bewegung nach links/rechts übersetzt wird.
Rechteck ausfeuern | Auslösen einer Kugel.
Berühren außerhalb des Verschiebungs-und Feuer Rechtecks | Ändern Sie die Drehung der Kameraansicht (die Tonhöhe und die Drehung).

Die **MoveLookController**-Instanz überprüft anhand der Zeiger-ID, wo das Ereignis aufgetreten ist, und führt eine der folgenden Aktionen aus:

-   Wenn das [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved)-Ereignis im Bewegungs- oder Schießrechteck aufgetreten ist, wird die Zeigerposition für den Controller aktualisiert.
-   Wenn das [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved)-Ereignis an einer anderen Stelle des Bildschirms (definiert als Blicksteuerung) aufgetreten ist, werden die Änderungen des Nick- und Gierwinkels des Blickrichtungsvektors berechnet.


Nachdem wir unsere Berührungs Steuerelemente implementiert haben, geben die Rechtecke, die wir zuvor mithilfe von Direct2D gezeichnet haben, den Playern an, wo die Verschiebungs-, Feuer-und ausgabezonen


![Berührungs Steuerelemente](images/simple-dx-game-controls-showzones.png)


Sehen wir uns nun an, wie die einzelnen Steuerelemente implementiert werden.


### <a name="move-and-fire-controller"></a>Verschieben und Auslösen des Controllers
Das Verschieben des Controller Rechtecks im unteren linken Quadranten des Bildschirms wird als direktionales Pad verwendet. Wenn Sie Ihren Ziehpunkt nach links und rechts in diesem Bereich bewegen, wird der Spieler nach links und rechts verschoben, während die Kamera nach oben und unten bewegt wird.
Nachdem Sie dies festgelegt haben, wird durch Tippen auf den Fire Controller im unteren rechten Quadranten des Bildschirms eine Kugel ausgelöst.

Die [**setmect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) -Methode und die [**setfirup**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) -Methode erstellen unsere Eingabe Rechtecke, die zwei, 2D-Vektoren zum Angeben der Rechte in der oberen linken und unteren rechten Ecke des Bildschirms haben. 


Die Parameter werden dann **m \_ fireupperleft** und **m \_ firelowerright** zugewiesen, sodass wir ermitteln können, ob der Benutzer innerhalb der Rechtecke berührt. 
```cppwinrt
m_fireUpperLeft = upperLeft;
m_fireLowerRight = lowerRight;
```

Wenn die Größe des Bildschirms geändert wird, werden diese Rechtecke auf die approperiate-Größe neu gezeichnet.

Nachdem wir nun die Steuerelemente in Zonen genommen haben, ist es an der Zeit, zu bestimmen, wann Sie von einem Benutzer tatsächlich verwendet werden.
Zu diesem Zweck richten wir einige Ereignishandler in der Methode " **wvelookcontroller:: initwindow** " für ein, wenn der Benutzer den Zeiger drückt, verschiebt oder freigibt.

```cppwinrt
window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

window.PointerReleased({ this, &MoveLookController::OnPointerReleased });
```

Zuerst legen wir fest, was geschieht, wenn der Benutzer mit der [**onpointerpressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) -Methode innerhalb der Verschiebe-oder Feuer Rechtecke drückt.
Hier überprüfen wir, wo Sie ein Steuerelement berühren und ob sich ein Zeiger bereits in diesem Controller befindet. Wenn dies der erste Finger ist, um das jeweilige Steuerelement zu berühren, gehen wir wie folgt vor.
- Speichern Sie den Speicherort der Touchdown-Datei in " **m \_ muvefirstdown** " oder " **m \_ firefirstdown** " als 2D-Vektor.
- Weisen Sie die Zeiger-ID **m "m \_ "** und " ** \_ firepointerid**" zu.
- Legen Sie das richtige **InUse** -Flag (**m \_ moveinuse** oder **m \_ fireinuse**) auf fest, `true` da wir nun über einen aktiven Zeiger für dieses Steuerelement verfügen.

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();
auto pointerDeviceType = pointerDevice.PointerDeviceType();

XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

...
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Check to see if this pointer is in the move control.
        if (position.x > m_moveUpperLeft.x &&
            position.x < m_moveLowerRight.x &&
            position.y > m_moveUpperLeft.y &&
            position.y < m_moveLowerRight.y)
        {
            // If no pointer is in this control yet.
            if (!m_moveInUse)
            {
                // Process a DPad touch down event.
                // Save the location of the initial contact
                m_moveFirstDown = position;
                // Store the pointer using this control
                m_movePointerID = pointerID;
                // Set InUse flag to signal there is an active move pointer
                m_moveInUse = true;
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
                // Save the location of the initial contact
                m_fireLastPoint = position;
                // Store the pointer using this control
                m_firePointerID = pointerID;
                // Set InUse flag to signal there is an active fire pointer
                m_fireInUse = true;
                ...
            }
        }
        ...
```

Nachdem Sie ermittelt haben, ob der Benutzer ein Move-oder Fire-Steuerelement berührt, wird angezeigt, ob der Spieler Bewegungen mit dem gedrückten Finger macht.
Mithilfe der Methode " [**muvelookcontroller:: onpointerverschot**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) " überprüfen wir, welcher Zeiger verschoben wurde, und speichern dann seine neue Position als 2D-Vektor.  

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();

// convert to allow math
XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

switch (m_state)
{
case MoveLookControllerState::Active:
    // Decide which control this pointer is operating.

    // Move control
    if (pointerID == m_movePointerID)
    {
        // Save the current position.
        m_movePointerPosition = position;
    }
    // Look control
    else if (pointerID == m_lookPointerID)
    {
        ...
    }
    // Fire control
    else if (pointerID == m_firePointerID)
    {
        m_fireLastPoint = position;
    }
    ...
```

Sobald der Benutzer seine Gesten innerhalb der Steuerelemente vorgenommen hat, gibt er den Zeiger frei. Mithilfe der Methode " [**wvelookcontroller:: onpointerreleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) " legen wir fest, welcher Zeiger freigegeben wurde, und führen eine Reihe von zurück Stellungen aus.


Wenn das Verschiebungs Steuerelement freigegeben wurde, führen wir die folgenden Schritte aus.
- Legen Sie die Geschwindigkeit des Players `0` in alle Richtungen auf fest, um zu verhindern, dass Sie im Spiel bewegt werden.
- Switch **m \_ moveinuse** in, `false` da der Benutzer den Verschiebungs Controller nicht mehr berührt.
- Legen Sie die ID des Verschiebungs Zeigers auf fest, `0` da es keinen Zeiger mehr im Verschiebungs Controller gibt.

```cppwinrt
if (pointerID == m_movePointerID)
{
    // Stop on release.
    m_velocity = XMFLOAT3(0, 0, 0);
    m_moveInUse = false;
    m_movePointerID = 0;
}
```

Wenn das Fire-Steuerelement freigegeben wurde, ändern wir das **m_fireInUse** -Flag auf `false` und die Fire-Zeiger-ID auf, `0` da es keinen Zeiger mehr im Fire-Steuerelement gibt.
```cppwinrt
else if (pointerID == m_firePointerID)
{
    m_fireInUse = false;
    m_firePointerID = 0;
}
```

### <a name="look-controller"></a>Controller suchen
Touchgeräte-Zeiger Ereignisse werden für die nicht verwendeten Bereiche des Bildschirms als Anzeige Controller behandelt. Wenn Sie den Finger um diese Zone bewegen, ändert sich die Tonhöhe und die Drehung (Drehung) der Player Kamera.

Wenn das " **wvelookcontroller:: onpointerpressed** "-Ereignis auf einem Fingerabdruck Gerät in diesem Bereich ausgelöst wird und der Spielzustand auf " **aktiv**" festgelegt ist, wird ihm eine Zeiger-ID zugewiesen.

```cppwinrt
// If no pointer is in this control yet.
if (!m_lookInUse)
{
    // Save point for later move.
    m_lookLastPoint = position;
    // Store the pointer using this control.
    m_lookPointerID = pointerID;
    // These are for smoothing.
    m_lookLastDelta.x = m_lookLastDelta.y = 0;
    m_lookInUse = true;
}
```

Hier weist der " **muvelookcontroller** " der Zeiger-ID für den Zeiger, der das Ereignis ausgelöst hat, eine bestimmte Variable zu, die dem Look-Bereich entspricht. Wenn eine Fingereingabe im Suchbereich auftritt, wird die m- ** \_ loopointerid** -Variable auf die Zeiger-ID festgelegt, die das Ereignis ausgelöst hat. Eine boolesche Variable, **m \_ lookinuse**, wird ebenfalls festgelegt, um anzugeben, dass das Steuerelement noch nicht freigegeben wurde.

Sehen wir uns nun an, wie das Beispiel Spiel das Ereignis " [**pointerverschoder**](/uwp/api/windows.ui.core.corewindow.pointermoved) Touchscreen" behandelt.

Innerhalb der Methode " **muvelookcontroller:: onpointerverschode** " wird überprüft, welche Art von Zeiger-ID dem Ereignis zugewiesen wurde. Wenn Sie **m_lookPointerID**, berechnen wir die Änderung an der Position des Zeigers.
Wir verwenden dieses Delta dann, um zu berechnen, wie viel sich die Drehung ändern soll. Schließlich sind wir an einem Punkt, an dem wir den **m \_ ** -und den m-Wert für die Verwendung im Spiel aktualisieren können, um die Spieler Drehung zu ändern. ** \_ ** 

```cppwinrt
// This is the look pointer.
else if (pointerID == m_lookPointerID)
{
    // Look control.
    XMFLOAT2 pointerDelta;
    // How far did the pointer move?
    pointerDelta.x = position.x - m_lookLastPoint.x;
    pointerDelta.y = position.y - m_lookLastPoint.y;

    XMFLOAT2 rotationDelta;
    // Scale for control sensitivity.
    rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;
    rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
    // Save for next time through.
    m_lookLastPoint = position;

    // Update our orientation based on the command.
    m_pitch -= rotationDelta.y;
    m_yaw += rotationDelta.x;

    // Limit pitch to straight up or straight down.
    float limit = XM_PI / 2.0f - 0.01f;
    m_pitch = __max(-limit, m_pitch);
    m_pitch = __min(+limit, m_pitch);
    ...
}
```

Im letzten Abschnitt wird erläutert, wie das Beispiel Spiel das [**pointerreleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) -Ereignis für den Touchscreen behandelt.
Nachdem der Benutzer die Touch-Bewegung abgeschlossen und den Finger vom Bildschirm entfernt hat, wird " [**mvelookcontroller:: onpointerreleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) " initiiert.
Wenn die ID des Zeigers, der das Ereignis [**pointerreleased ausgelöst**](/uwp/api/windows.ui.core.corewindow.pointerreleased) hat, die ID des zuvor aufgezeichneten Verschiebungs Zeigers ist, legt **movelookcontroller** die Geschwindigkeit auf fest, `0` da der Spieler den Bereich der Suche beendet hat.

```cppwinrt
else if (pointerID == m_lookPointerID)
{
    m_lookInUse = false;
    m_lookPointerID = 0;
}
```

## <a name="adding-mouse-and-keyboard-support"></a>Hinzufügen von Maus-und Tastatur Unterstützung

Dieses Spiel verfügt über das folgende Steuerelement Layout für Tastatur und Maus.

Benutzereingabe | Aktion
:------- | :--------
W | Player vorwärts verschieben
Ein | Player nach links verschieben
E | Player rückwärts verschieben
D | Player nach rechts verschieben
X | Ansicht nach oben verschieben
Leertaste | Ansicht nach unten verschieben
P | Anhalten des Spiels
Mausbewegung | Ändern der Drehung (der Tonhöhe und der Yaw) der Kameraansicht
Linke Maustaste | Auslösen einer Kugel


Um die Tastatur zu verwenden, [**registriert das Beispiel**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) Spiel zwei neue Ereignisse [**: "corewindow:: KeyUp**](/uwp/api/windows.ui.core.corewindow.keyup) " und " [**corewindow:: KeyDown**](/uwp/api/windows.ui.core.corewindow.keydown)" in der Methode "Methode". Diese Ereignisse behandeln den Press und die Freigabe einer Taste.

```cppwinrt
window.KeyDown({ this, &MoveLookController::OnKeyDown });

window.KeyUp({ this, &MoveLookController::OnKeyUp });
```

Die Maus wird von den Berührungs Steuerelementen etwas anders behandelt, auch wenn Sie einen Zeiger verwendet. Um das Layout des Steuer Elements auszurichten, dreht der " **muvelookcontroller** " die Kamera, wenn der Mauszeiger bewegt wird, und wird ausgelöst, wenn die linke Maustaste gedrückt wird.

Dies wird in der [**onpointerpressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) -Methode von " **muvelookcontroller**" behandelt.

In dieser Methode überprüfen wir, welche Art von Zeiger Gerät mit der-Aufzählung verwendet wird [`Windows::Devices::Input::PointerDeviceType`](/uwp/api/Windows.Devices.Input.PointerDeviceType) . Wenn das Spiel **aktiv** ist und das **pointerde vicetype** -Gerät nicht **berührt**wird, gehen wir davon aus, dass es eine Mauseingabe ist.

```cppwinrt
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Behavior for touch controls
        ...

    default:
        // Behavior for mouse controls
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

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
            // These are for smoothing.
            m_lookLastDelta.x = m_lookLastDelta.y = 0;
        }
        break;
    }
    break;
```

Wenn der Spieler das Drücken einer der Maustasten beendet, wird das " [corewindow::P ointerreleased](/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) -Maus Ereignis ausgelöst, das [Aufrufen der Methode" "der Methode](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) " "wird aufgerufen, und die Eingabe ist fertiggestellt. An diesem Punkt werden die Bereiche nicht mehr ausgelöst, wenn die linke Maustaste gedrückt wurde und nun losgelassen wurde. Da das Aussehen immer aktiviert ist, verwendet das Spiel weiterhin denselben Mauszeiger, um die laufenden Look-Ereignisse zu verfolgen.

```cppwinrt
case MoveLookControllerState::Active:
    // Touch points
    if (pointerID == m_movePointerID)
    {
        // Stop movement
        ...
    }
    else if (pointerID == m_lookPointerID)
    {
        // Stop look rotation
        ...
    }
    // Fire button has been released
    else if (pointerID == m_firePointerID)
    {
        // Stop firing
        ...
    }
    // Mouse point
    else if (pointerID == m_mousePointerID)
    {
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

        // Mouse no longer in use so stop firing
        m_mouseInUse = false;

        // Don't clear the mouse pointer ID so that Move events still result in Look changes.
        // m_mousePointerID = 0;
        m_mouseLeftInUse = leftButton;
        m_mouseRightInUse = rightButton;
    }
    break;
```

Sehen wir uns nun den letzten Steuerelement Typ an, der unterstützt wird: Gamepads. Gamepads werden getrennt von den Berührungs-und Maus Steuerelementen behandelt, da Sie das Zeiger Objekt nicht verwenden. Aus diesem Grund müssen einige neue Ereignishandler und-Methoden hinzugefügt werden.

## <a name="adding-gamepad-support"></a>Hinzufügen von Gamepad-Unterstützung

Für dieses Spiel wird die Gamepad-Unterstützung durch Aufrufe der [Windows. Gaming. Input](/uwp/api/windows.gaming.input) -APIs hinzugefügt. Dieser Satz von APIs bietet Zugriff auf Spiele Controller Eingaben, wie z. b. Rennwagen und Flight-Sticks. 

Im folgenden werden unsere Gamepad-Steuerelemente angezeigt.

Benutzereingabe | Aktion
:------- | :--------
Linker analoger | Player verschieben
Rechter Analog Stift | Ändern der Drehung (der Tonhöhe und der Yaw) der Kameraansicht
Rechter Trigger | Auslösen einer Kugel
Schaltfläche "Start/Menü" | Anhalten oder Fortsetzen des Spiels

In der [**initwindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) -Methode werden zwei neue Ereignisse hinzugefügt, um zu bestimmen, ob ein Gamepad [hinzugefügt](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) oder [entfernt](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114)wurde. Diese Ereignisse aktualisieren die **m_gamepadsChanged** -Eigenschaft. Dies wird in der **updatepollingdevices** -Methode verwendet, um zu überprüfen, ob sich die Liste der bekannten Gamepads geändert hat. 

```cppwinrt
// Detect gamepad connection and disconnection events.
Gamepad::GamepadAdded({ this, &MoveLookController::OnGamepadAdded });

Gamepad::GamepadRemoved({ this, &MoveLookController::OnGamepadRemoved });
```

> [!NOTE]
> UWP-Apps können keine Eingaben von einem Xbox One-Controller empfangen, während die APP nicht den Fokus hat.

### <a name="the-updatepollingdevices-method"></a>Die updatepollingdevices-Methode

Die [**updatepollingdevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) -Methode der **movelookcontroller** -Instanz prüft sofort, ob ein Gamepad angeschlossen ist. Wenn eine solche ist, beginnen wir mit dem Lesen des Zustands mit " [**Gamepad. getcurrentreading**](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)". Dadurch wird die [**gamepadreading**](/uwp/api/Windows.Gaming.Input.GamepadReading) -Struktur zurückgegeben, sodass wir überprüfen können, auf welche Schaltflächen geklickt wurden oder welche Fingerabdrücke verschoben wurden.

Wenn der Status des Spiels **waitforinput**ist, lauschen wir nur auf die Schaltfläche Start/Menü des Controllers, damit das Spiel fortgesetzt werden kann.

Wenn Sie **aktiv**ist, überprüfen wir die Eingabe des Benutzers und legen fest, welche Aktion im Spiel ausgeführt werden muss.
Wenn der Benutzer z. b. den linken analogen Stick in eine bestimmte Richtung verschoben hat, weiß das Spiel, dass der Spieler in die Richtung verschoben werden muss, in der der Stick verschoben wird. Die Verschiebung des Sticks in einer bestimmten Richtung muss sich als der Radius der **toten Zone**registrieren. Andernfalls findet nichts statt. Dieser Zone für unzustellbare Zonen ist erforderlich, um "Abdriften" zu verhindern. Dies ist der Fall, wenn der Controller kleine Bewegungen aus dem Ziehpunkt des Players übernimmt, wenn er auf dem Stick liegt. Ohne unzustellbare Zonen können die Steuerelemente für den Benutzer zu sensibel angezeigt werden.

Die Fingereingabe für die Fingereingabe liegt zwischen-1 und 1 für die x-und die y-Achse. Im folgenden Abschnitt wird der Radius der fingerstick-Zone für unzustellbare Nachrichten angegeben.

```cppwinrt
#define THUMBSTICK_DEADZONE 0.25f
```

Wenn Sie diese Variable verwenden, beginnen wir mit der Verarbeitung von Eingaben, die bearbeitet werden können. Es erfolgt eine Bewegung mit einem Wert von [-1,-.26] oder [. 26, 1] auf beiden Achsen.

![unzustellbare Zone für thumbsticks](images/simple-dx-game-controls-deadzone.png)

Dieser Teil der **updatepollingdevices** -Methode behandelt die linken und rechten fingerstäbchen.
Die X-und Y-Werte jedes Sticks werden geprüft, um festzustellen, ob Sie sich außerhalb der unzustellbaren Zone befinden. Wenn eine oder beide sind, aktualisieren wir die entsprechende Komponente.
Wenn z. b. der linke Finger Stick auf der x-Achse nach links verschoben wird, fügen wir-1 der **x** -Komponente des **m_moveCommand** Vector hinzu. Dieser Vektor dient zum Aggregieren aller Bewegungen auf allen Geräten und wird später verwendet, um die Position des Players zu berechnen. 

```cppwinrt
// Use the left thumbstick to control the eye point position
// (position of the player).

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

Ähnlich wie bei der Bewegung durch den linken Stick steuert der Rechte Strich die Drehung der Kamera.

Das richtige Ziehpunkt Verhalten richtet sich nach dem Verhalten der Mausbewegung bei der Maus-und Tastatur Steuerelement Einrichtung.
Wenn sich der Stab außerhalb der unzustellbaren Zone befindet, berechnen wir den Unterschied zwischen der aktuellen Zeigerposition und dem Ort, an dem der Benutzer nun zu suchen versucht.
Diese Änderung an der Zeigerposition (**pointerdelta**) wird dann verwendet, um die Tonhöhe und den Wert der Kamera Drehung zu aktualisieren, die später in der **Update** -Methode angewendet wird.
Der **pointerdelta** -Vektor kann vertraut aussehen, da er auch in der Methode "" von "" verwendet wird, um die Änderung [an der Zeiger](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) Position für die Maus-und Berührungs Eingaben nachzuverfolgen.

```cppwinrt
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
// Scale for control sensitivity.
rotationDelta.x = pointerDelta.x * 0.08f;
rotationDelta.y = pointerDelta.y * 0.08f;

// Update our orientation based on the command.
m_pitch += rotationDelta.y;
m_yaw += rotationDelta.x;

// Limit pitch to straight up or straight down.
m_pitch = __max(-XM_PI / 2.0f, m_pitch);
m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Die Steuerelemente des Spiels sind nicht komplett, ohne dass Sie Bereiche auslösen können!

Diese **updatepollingdevices** -Methode überprüft auch, ob der richtige-Auslöse Taste gedrückt wird. Wenn dies der Fall ist, wird die **m_firePressed** -Eigenschaft auf true gekippt, was dem Spiel signalisiert, dass die Bereiche ausgelöst werden sollen.
```cppwinrt
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

Um die Dinge zu wrappen, betrachten wir die [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) -Methode ausführlicher.
Mit dieser Methode werden alle Bewegungen oder Rotationen zusammengeführt, die der Player mit einer beliebigen unterstützten Eingabe durchgeführt hat, um einen Geschwindigkeitsvektor zu generieren und unsere Tonhöhe und die GW-Werte für die Spiel Schleife zu aktualisieren

Die **Update** -Methode startet die Dinge, indem [**updatepollingdevices**](#the-updatepollingdevices-method) aufgerufen wird, um den Status des Controllers zu aktualisieren. Diese Methode sammelt auch alle Eingaben von einem Gamepad und fügt deren Bewegungen dem **m_moveCommand** Vektor hinzu. 

In der **Update** -Methode führen wir die folgenden Eingabe Prüfungen aus.
- Wenn der Spieler das Verschiebe Controller Rechteck verwendet, wird die Änderung an der Zeigerposition festgelegt, um zu berechnen, ob der Benutzer den Mauszeiger aus der inaktiven Zone des Controllers verschoben hat. Wenn Sie außerhalb der unzustellbaren Zone liegen, wird die **m_moveCommand** Vector-Eigenschaft mit dem virtuellen Joystick Wert aktualisiert.
- Wenn eine der Bewegungs Tastatureingaben gedrückt wird, wird `1.0f` der Wert oder `-1.0f` in der entsprechenden Komponente des **m_moveCommand** Vektors &mdash; `1.0f` für vorwärts und rückwärts hinzugefügt `-1.0f` .

Nachdem alle Verschiebungs Eingaben berücksichtigt wurden, führen wir den **m_moveCommand** Vektor durch einige Berechnungen aus, um einen neuen Vektor zu generieren, der die Richtung des Players in Bezug auf die Spiel Welt darstellt.
Wir nehmen dann die Bewegungen in Bezug auf die Welt und wenden Sie auf den Player als Geschwindigkeit in dieser Richtung an.
Schließlich haben wir den **m_moveCommand** Vektor auf zurückgesetzt `(0.0f, 0.0f, 0.0f)` , sodass alles für den nächsten Spiel Rahmen bereit ist.

```cppwinrt
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        // Leave 32 pixel-wide dead spot for being still.
        if (fabsf(pointerDelta.x) > 16.0f)
            m_moveCommand.x -= pointerDelta.x / fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y / fabsf(pointerDelta.y);
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
    wCommand.x = m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y = m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z = m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z = wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y = wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem wir nun die Steuerelemente hinzugefügt haben, ist ein weiteres Feature erforderlich, das wir hinzufügen müssen, um ein immersives Spiel zu erstellen!
Musik-und Soundeffekte sind für jedes Spiel wichtig, daher besprechen wir das [Hinzufügen von Sound](tutorial--adding-sound.md) Next.