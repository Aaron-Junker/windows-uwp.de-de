---
title: Toucheingabesteuerelemente für Spiele
description: Hier erfahren Sie, wie Sie Ihrem C++-Spiel für die universelle Windows-Plattform (UWP) mit DirectX einfache touchbasierte Steuerelemente hinzufügen.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Touch, Steuerelemente, Directx, Eingabe
ms.localizationpriority: medium
ms.openlocfilehash: e8892219b485d320bb77f90ac0d172e8e2403392
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618735"
---
# <a name="touch-controls-for-games"></a>Toucheingabesteuerelemente für Spiele



Hier erfahren Sie, wie Sie Ihrem C++-Spiel für die universelle Windows-Plattform (UWP) mit DirectX einfache touchbasierte Steuerelemente hinzufügen. Wir zeigen Ihnen, wie Sie touchbasierte Steuerelemente hinzufügen, um eine Kamera mit fester Ebene in einer Direct3D-Umgebung zu bewegen, indem die Kameraperspektive durch Bewegen des Fingers oder Eingabestifts geändert wird.

Sie können diese Steuerungen in Spiele integrieren, bei denen der Spieler in einer 3D-Umgebung (z. B. einer Karte oder einem Spielfeld) einen Bildlauf ausführen oder die Ansicht schwenken soll. Bei einem Strategie- oder Puzzlespiel kann der Spieler mit diesen Steuerungen z. B. durch Schwenken nach links oder rechts eine Spielumgebung anzeigen, die größer als der Bildschirm ist.

> **Beachten Sie**  unser Code funktioniert auch mit Maus-basierte schwenkansicht Steuerelemente. Die zeigerbezogenen Ereignisse werden von den Windows-Runtime-APIs abstrahiert, sodass sie toucheingabe- oder mausbasierte Zeigerereignisse behandeln können.

 

## <a name="objectives"></a>Ziele


-   Erstellen einer einfachen Fingereingabesteuerung, um eine Kamera mit fester Ebene in einem DirectX-Spiel zu schwenken

## <a name="set-up-the-basic-touch-event-infrastructure"></a>Einrichten der grundlegenden Infrastruktur für Toucheingabeereignisse


Als Erstes definieren wir den grundlegenden Controllertyp – in diesem Fall: **CameraPanController**. Der Controller wird hier als abstrakte Idee definiert, d. h. als Satz von Verhaltensweisen, die der Benutzer ausführen kann.

Die **CameraPanController**-Klasse ist eine regelmäßig aktualisierte Sammlung von Informationen zum Zustand des Kameracontrollers und bietet der App die Möglichkeit, diese Informationen aus ihrer Aktualisierungsschleife abzurufen.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

Jetzt erstellen wir einen Header, der den Zustand des Kameracontrollers definiert, sowie die grundlegenden Methoden und Ereignishandler, die die Interaktionen des Kameracontrollers implementieren.

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

public:

    // Methods to get input from the UI pointers
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

Die privaten Felder enthalten den aktuellen Zustand des Kameracontrollers. Ihre Funktion wird im Folgenden erläutert.

-   **m\_Position** ist die Position der Kamera im Bereich der Szene. In diesem Beispiel ist der Wert der Z-Koordinate unveränderlich auf „0“ festgelegt. Dieser Wert könnte mit „DirectX::XMFLOAT2“ dargestellt werden, für dieses Beispiel – und um zukünftige Erweiterungen zu ermöglichen – verwenden wir hier aber „DirectX::XMFLOAT3“. Wir übergeben Sie diesen Wert über die **erhalten\_Position** Eigenschaft, um die app selbst, damit den Viewport entsprechend aktualisiert werden können.
-   **m\_PanInUse** ist ein boolescher Wert, der angibt, ob ein Pan-Vorgang aktiv ist; oder, genauer gesagt, ob der Spieler den Bildschirm berühren und Bewegen der Kamera.
-   **m\_PanPointerID** ist eine eindeutige ID für den Zeiger. Obwohl die ID im Beispiel nicht verwendet wird, empfiehlt es sich, der Controllerzustandsklasse immer einen bestimmten Zeiger zuzuordnen.
-   **m\_PanFirstDown** ist der Punkt, auf dem Bildschirm, in denen berührt des Bildschirms der Spieler zunächst oder die Maus geklickt, während der Kamera Pan-Aktion. Dieser Wert wird später verwendet, um einen inaktiven Bereich festzulegen, damit die Ansicht nicht flimmert, wenn der Bildschirm berührt oder die Maus leicht bewegt wird.
-   **m\_PanPointerPosition** ist der Punkt, auf dem Bildschirm, in dem der Spieler hat den Zeiger derzeit verschoben. Wir verwenden, um welche Richtung bestimmen möchten, dass der Spieler verschieben, indem er sie relativ zum Untersuchen **m\_PanFirstDown**.
-   **m\_PanCommand** ist der endgültige berechnete-Befehl für den Controller Kamera: oben, unten, links oder rechts. Da die Kamerabewegung hier auf die X-Y-Ebene beschränkt ist, könnte stattdessen ein DirectX::XMFLOAT2-Wert verwendet werden.

Mit den folgenden drei Ereignishandlern aktualisieren wir die Informationen zum Zustand des Kameracontrollers.

-   **OnPointerPressed** ist ein Ereignishandler, der von der App aufgerufen wird, wenn der Spieler den Touchscreen mit dem Finger berührt und der Zeiger zu den Koordinaten des Berührungspunkts bewegt wird.
-   **OnPointerMoved** ist ein Ereignishandler, der von der App aufgerufen wird, wenn der Spieler mit einem Finger eine Wischbewegung über den Touchscreen ausführt. Er aktualisiert den Wert mit den neuen Koordinaten der Bewegung.
-   **OnPointerReleased** ist ein Ereignishandler, der von der App aufgerufen wird, wenn der Spieler den Finger vom Touchscreen nimmt.

Die folgenden Methoden und Eigenschaften verwenden wir, um die Zustandsinformationen des Kameracontrollers zu initialisieren, auf sie zuzugreifen und sie zu aktualisieren.

-   **Initialize** ist ein Ereignishandler, den die App aufruft, um die Steuerelemente zu initialisieren und an das [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt anzufügen, das das Anzeigefenster beschreibt.
-   **SetPosition** ist eine Methode, die die App aufruft, um die Koordinaten (X, Y und Z) der Steuerungen im Szenenbereich festzulegen. Beachten Sie, dass die Z-Koordinate in diesem Lernprogramm immer Null ist.
-   **erste\_Position** ist eine Eigenschaft, die unsere app zugreift, um die aktuelle Position der Kamera im Bereich der Szene zu erhalten. Diese Eigenschaft wird verwendet, um der App die aktuelle Kameraposition mitzuteilen.
-   **erste\_FixedLookPoint** ist eine Eigenschaft, die unsere app zugreift, um den aktuellen Punkt zu erhalten, zu dem die Controller-Kamera sind aufgetreten. In diesem Beispiel ist der Punkt auf die Normale der X-Y-Ebene beschränkt.
-   **Update** ist eine Methode, die den Controllerzustand liest und die Kameraposition aktualisiert. &lt;Etwas&gt; wird in der Hauptschleife der App kontinuierlich aufgerufen, um die Kameracontrollerdaten und die Kameraposition im Szenenbereich zu aktualisieren.

Jetzt haben Sie alle Komponenten, die Sie zum Implementieren von Toucheingabesteuerungen benötigen. Sie können feststellen, wann und wo die Fingereingabe- oder Mauszeigerereignisse stattgefunden haben und um welche Aktion es sich dabei handelt. Sie können die Position und Ausrichtung der Kamera in Bezug zum Szenenbereich festlegen und die Änderungen nachverfolgen. Und schließlich können Sie die neue Kameraposition der aufrufenden App mitteilen.

Als Nächstes setzen wir diese Teile zusammen.

## <a name="create-the-basic-touch-events"></a>Erstellen der grundlegenden Fingereingabeereignisse


Der Ereignisverteiler der Windows-Runtime stellt drei Ereignisse bereit, die von der App behandelt werden sollen:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

Diese Ereignisse sind im [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Typ implementiert. Es wird davon ausgegangen, dass Sie über ein **CoreWindow**-Objekt verfügen, mit dem Sie arbeiten können. Weitere Informationen finden Sie unter [Einrichten der UWP-C++-App zum Anzeigen einer DirectX-Ansicht](https://msdn.microsoft.com/library/windows/apps/hh465077).

Wenn diese Ereignisse während der Ausführung der App ausgelöst werden, aktualisieren die Handler die in den privaten Feldern definierten Zustandsinformationen des Kameracontrollers.

Als Erstes füllen wir die Ereignishandler für den Toucheingabezeiger auf. Im ersten Ereignishandler (**OnPointerPressed**) rufen wir die X- und Y-Koordinaten des Zeigers aus dem [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Objekt ab, das die Anzeige verwaltet, wenn der Benutzer den Bildschirm berührt oder mit der Maus klickt.

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

Wir verwenden diesen Handler können Sie die aktuelle **CameraPanController** Instanz kennen, Kamera Controller behandelt werden als aktiv festlegen **m\_PanInUse** auf "true". Auf diese Weise verwendet die App die aktuellen Positionsdaten zum Aktualisieren des Viewports, wenn sie **Update** aufruft.

Nachdem wir die Basiswerte für die Kamerabewegung beim Berühren des Bildschirms oder Klicken im Anzeigefenster eingerichtet haben, müssen wir bestimmen, was passieren soll, wenn der Benutzer den Finger auf dem Bildschirm bewegt oder die Maus mit gedrückter Maustaste bewegt.

Der **OnPointerMoved**-Ereignishandler wird bei jeder Bewegung des Zeigers ausgelöst – bei jedem Teilstrich, um den der Spieler den Zeiger auf dem Bildschirm bewegt. Wir müssen die App über die aktuelle Position des Zeigers auf dem Laufenden halten. Dazu gehen wir wie folgt vor:

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

Abschließend müssen wir den Kameraschwenk deaktivieren, wenn der Spieler aufhört, den Bildschirm zu berühren. Wir verwenden **OnPointerReleased**, die aufgerufen wird, wenn [ **PointerReleased** ](https://msdn.microsoft.com/library/windows/apps/br208279) ausgelöst wird, entsprechend **m\_PanInUse** auf "false" und Deaktivieren der Kamera Pan-Bewegung und die ID der Zeiger auf 0 festgelegt.

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Initialisieren der Toucheingabesteuerungen und des Controllerzustands


Jetzt verbinden wir die Ereignisse und initialisieren alle grundlegenden Zustandsfelder des Kameracontrollers.

**Initialisieren**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

Die **Initialize**-Methode akzeptiert als Parameter einen Verweis auf die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz der App und registriert die von uns entwickelten Ereignishandler für die entsprechenden Ereignisse in dieser **CoreWindow**-Instanz.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>Abrufen und Festlegen der Position des Kameracontrollers


Als Nächstes definieren wir einige Methoden zum Abrufen und Festlegen der Position des Kameracontrollers im Szenenbereich.

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** ist eine öffentliche Methode, die wir in der App aufrufen können, wenn wir die Position des Kameracontrollers auf einen bestimmten Punkt festlegen müssen.

**erste\_Position** ist die wichtigste öffentliche Eigenschaft: Dies ist die Möglichkeit, unsere app Ruft die aktuelle Position der Kamera-Controllers im Bereich der Szene, damit den Viewport entsprechend aktualisiert werden können.

**erste\_FixedLookPoint** ist eine öffentliche Eigenschaft, die in diesem Beispiel einem Blick Punkt abruft senkrecht zur x-und y-Ebene. Sie können diese Methode ändern, um die Berechnung der X-, Y- und Z-Koordinatenwerte mit den trigonometrischen Funktionen Sinus und Kosinus durchzuführen, wenn Sie schrägere Winkel für die feste Kamera verwenden möchten.

## <a name="updating-the-camera-controller-state-information"></a>Aktualisieren der Zustandsinformationen des Kameracontrollers


Nun führen wir unsere Berechnungen, die die Informationen der Zeiger-Koordinate in nachverfolgt konvertieren **m\_PanPointerPosition** in neuen-Koordinate Informationen, die jeweiligen unsere 3D-Szene Speicherplatz. Die App ruft diese Methode bei jeder Aktualisierung der Hauptschleife auf. Dabei werden die neuen, an die App zu übergebenden Positionsinformationen berechnet, die zum Aktualisieren der Ansichtsmatrix vor der Projektion in den Viewport verwendet werden.

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

Damit die Bewegung bei Verwendung der Finger- oder Mauseingabe nicht "zittert" und die Schwenkbewegung der Kamera dadurch ruckartig wird, legen wir einen inaktiven Bereich mit einem Durchmesser von 32 Pixel um den Zeiger fest. Außerdem haben wir einen Geschwindigkeitswert – in diesem Fall 1:1 – für die Zeigerbewegung außerhalb des inaktiven Bereichs. Sie können dieses Verhalten anpassen, um die Geschwindigkeitsrate zu erhöhen oder zu verringern.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Aktualisieren der Ansichtsmatrix mit der neuen Kameraposition


Jetzt können wir eine Koordinate des Szenenbereichs abrufen, auf die die Kamera ausgerichtet ist und die jedes Mal aktualisiert wird, wenn die App dazu angewiesen wird (z. B. alle 60 Sekunden in der Hauptschleife der App). Dieser Pseudocode zeigt das Aufrufverhalten, das Sie implementieren können:

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

Gratulation! Sie haben in Ihrem Spiel einen einfachen Satz mit Toucheingabesteuerungen zum Schwenken einer Kamera implementiert.


 

 

 




