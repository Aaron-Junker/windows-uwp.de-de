---
Description: Empfangen, verarbeiten, und Verwalten von Eingabedaten aus Zeigegeräte wie Fingereingabe, Maus, Stift/Stift- und Touchpad, in Ihren Anwendungen für die universelle Windows-Plattform (UWP).
title: Behandeln von Zeigereingaben
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: Stift, Maus, Touchpad, Toucheingabe, Zeiger, Eingabe, Benutzerinteraktionen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 596e9221fac686964b4faaa8a75f112dbb8ddf5a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619365"
---
# <a name="handle-pointer-input"></a>Behandeln von Zeigereingaben

Empfangen, verarbeiten und verwalten Sie Eingabedaten von Zeigegeräten (z. B. Toucheingaben, Maus, Zeichen-/Eingabestift und Touchpad) in Anwendungen für die Universelle Windows-Plattform (UWP).

> [!Important]
> Erstellen Sie benutzerdefinierte Interaktionen nur dann, wenn ein eindeutiger, klar umrissener Bedarf besteht und die von den Plattformsteuerelementen unterstützten Interaktionen Ihr Szenario nicht unterstützen.  
> Wenn Sie die Interaktionserfahrungen in Ihrer Windows-Anwendung anpassen, erwarten Benutzer, dass sie konsistent, intuitiv und leicht auffindbar sind. Aus diesen Gründen wird empfohlen, Ihre benutzerdefinierten Interaktionen nach denen zu modellieren, die von den [Plattformsteuerelementen](../controls-and-patterns/controls-by-function.md) unterstützt werden. Die Plattformsteuerelemente bieten umfassende Funktionen für UWP-Benutzerinteraktionen (Universelle Windows-Plattform) wie Standardinteraktionen, animierte Physikeffekte, visuelles Feedback und Barrierefreiheit. 

## <a name="important-apis"></a>Wichtige APIs
- [Windows.Devices.Input](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>Zeiger
Bei den meisten Interaktionsfunktionen ist typischerweise der Benutzer involviert, der das Objekt identifizieren muss, mit dem er interagieren möchte, indem er mithilfe von Eingabegeräten wie Toucheingabe, Maus, Zeichen-/Eingabestift und Touchpad darauf zeigt. Da die von diesen Eingabegeräten bereitgestellten HID-Rohdaten (Human Interface Device) viele allgemeine Eigenschaften enthalten, werden die Daten an einen einheitlichen Eingabestapel weitergeleitet und in geräteunabhängigen Zeigerdaten konsolidiert und zur Verfügung gestellt. Ihre UWP-Anwendungen können dann diese Daten ohne Rücksicht auf das verwendete Eingabegerät verwenden.

> [!NOTE]
> Gerätespezifische Informationen werden auch von den HID-Rohdaten weitergeleitet, falls dies für die App erforderlich ist.
 

Jeder Eingabepunkt (oder Kontakt) in dem Eingabestapel wird durch ein [**Pointer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.Pointer)-Objekt dargestellt, das über den [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs)-Parameter in den verschiedenen Zeigerereignishandlern zur Verfügung gestellt wird. Bei Verwendung mehrerer Stifte oder der Mehrfingereingabe wird jeder Kontakt als separater Eingabezeiger behandelt.

## <a name="pointer-events"></a>Zeigerereignisse


Zeigerereignisse machen grundlegende Informationen wie den Typ des Eingabegeräts und den Erkennungszustand (im Bereich oder bei Kontakt) sowie erweiterte Informationen wie Position, Druck und Kontaktgeometrie verfügbar. Darüber hinaus sind auch bestimmte gerätespezifische Eigenschaften verfügbar, z. B. welche Maustaste ein Benutzer gedrückt hat oder ob die Radiergummispitze des Zeichenstifts verwendet wird. Wenn die App zwischen Eingabegeräten und ihren Funktionen unterscheiden muss, finden Sie entsprechende Informationen unter [Erkennen von Eingabegeräten](identify-input-devices.md).

UWP-Apps können die folgenden Zeigerereignisse überwachen:

> [!NOTE]
> Schränken Sie Zeigereingaben auf ein bestimmtes UI-Element ein, indem Sie [**CapturePointer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer) für dieses Element in einem Zeigerereignishandler aufrufen. Wenn ein Zeiger von einem Element erfasst wurde, empfängt nur dieses Objekt die Zeigereingabeereignisse, auch wenn sich der Mauszeiger aus dem Begrenzungsbereich des Objekts heraus bewegt. Die Option [**IsInContact**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isincontact) (gedrückte Maustaste, Touch oder Stift in Kontakt) muss „true“ sein, damit **CapturePointer** erfolgreich ausgeführt werden kann.
 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignis</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger von der Plattform abgebrochen wird. Dies kann unter den folgenden Umständen auftreten.</p>
<ul>
<li>Touchzeiger werden abgebrochen, wenn ein Zeichenstift innerhalb des Bereichs der Eingabeoberfläche erkannt wird.</li>
<li>Für mehr als 100 ms wird kein aktiver Kontakt erkannt.</li>
<li>Monitor/Anzeige wird geändert (Auflösung, Einstellungen, Konfigurationen mit mehreren Bildschirmen).</li>
<li>Der Desktop ist gesperrt, oder der Benutzer hat sich abgemeldet.</li>
<li>Die Anzahl gleichzeitiger Kontakte hat die vom Gerät unterstützte Anzahl überschritten.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein anderes Benutzeroberflächenelement den Zeiger erfasst, der Zeiger freigegeben wurde oder ein anderer Zeiger programmgesteuert erfasst wurde.</p>
<div class="alert">
<strong>Beachten Sie</strong>  es gibt kein entsprechendes Zeiger-Capture-Ereignis.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger in den Begrenzungsbereich eines Elements eintritt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Für Toucheingaben ist zur Auslösung dieses Ereignisses eine Fingerkontakt erforderlich, entweder über eine direkte Toucheingabe oder durch Bewegen in den Begrenzungsbereich des Elements.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift das Ereignis über eine direkte Stifteingabe oder durch Bewegen in den Begrenzungsbereich des Elements aus. Der Stift verfügt jedoch auch über einen Hoverzustand (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>), der das Ereignis bei „true“ auslöst.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger den Begrenzungsbereich eines Elements verlässt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis aus, wenn sich der Mauszeiger aus dem Begrenzungsbereich des Elements heraus bewegt.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis beim Bewegen aus dem Begrenzungsbereich des Elements heraus aus. Der Stift verfügt jedoch auch über einen Hoverzustand (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>), der das Ereignis auslöst, wenn sich der Zustand von „true“ in „false“ ändert.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger Koordinaten, Schaltflächenzustand, Druck, Neigung oder Kontaktgeometrie (z. B. Breite und Höhe) innerhalb des Begrenzungsbereichs eines Elements ändert. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis nur aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht. Der Stift verfügt jedoch auch über einen Hoverzustand (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>), der dieses Ereignis bei „true“ und innerhalb des Begrenzungsbereichs des Elements auslöst.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Drückaktion (z. B. eine Fingereingabe, gedrückte Maustaste, Stifteingabe oder gedrückte Touchpadtaste) innerhalb des Begrenzungsbereichs eines Elements angibt.</p>
<p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a> muss aus dem Handler für dieses Ereignis aufgerufen werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Loslass-Aktion (z. B. ein Finger bewegt sich nach oben, Maustaste, Stift oder Touchpadtaste werden losgelassen) innerhalb des Begrenzungsbereichs eines Elements anzeigt, oder wenn der Zeiger außerhalb des Begrenzungsbereichs erfasst wird.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn das Mausrad gedreht wird.</p>
<p>Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Durch das Klicken auf eine Maustaste (links, Mausrad oder rechts) wird über das <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved">PointerMoved</a>-Ereignis eine zweite Zuordnung zwischen dem Zeiger und dieser Taste erstellt.</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Beispiel für Zeigerereignis

Nachfolgend sehen Sie einige Codeausschnitt aus einer einfachen Zeigerverfolgungs-App, in denen gezeigt wird, wie Ereignisse für mehrere Zeiger überwacht und behandelt und verschiedene Eigenschaften für die verbundenen Zeiger abgerufen werden.

![Benutzeroberfläche der Zeigeranwendung](images/pointers/pointers1.gif)

**Laden Sie dieses Beispiel von [Zeiger Eingabe sample (basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Erstellen der Benutzeroberfläche

In diesem Beispiel wird ein [Rechteck](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle) (`Target`) als Objekt verwendet, das Zeigereingaben nutzt. Die Farbe des Ziels ändert sich, wenn sich der Zeigerstatus ändert.

Details zu jedem Zeiger werden in einem schwebenden [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) angezeigt, der sich mit dem Mauszeiger bewegt. Die Zeigerereignisse selbst werden im [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) rechts neben dem Rechteck gemeldet.

Im Folgenden ist der XAML-Code (Extensible Application Markup Language) für die Benutzeroberfläche in diesem Beispiel aufgeführt. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>Lauschen auf Zeigerereignisse

In den meisten Fällen wird empfohlen, Zeigerinformationen über die [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) des Ereignishandlers abzurufen.

Wenn das Ereignisargument die Zeiger-Details, die erforderlich sind verfügbar macht, erhalten Sie Zugriff auf erweiterte [ **PointerPoint** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) Informationen verfügbar gemacht werden, über die [ **GetCurrentPoint** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) und [**GetIntermediatePoints**]https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) Methoden [ **PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs).

Der folgende Code richtet das globale Verzeichnisobjekt für die Verfolgung jedes aktiven Zeigers ein und identifiziert die verschiedenen Zeigerereignislistener für das Zielobjekt.

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>Behandeln von Zeigerereignissen

Im nächsten Schritt wird UI-Feedback verwendet, um die Verwendung einfacher Zeigerereignishandler zu veranschaulichen.

-   Der folgende Handler kontrolliert das [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zum aktiven Zeigerverzeichnis hinzugefügt, und die Zeigerdetails werden angezeigt.

    > [!NOTE]
    > [**PointerPressed** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) und [ **PointerReleased** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) Ereignisse treten nicht immer paarweise auf. Die App sollte auf jedes Ereignis lauschen und dieses behandeln, das einen Zeiger nach unten beenden könnte (beispielsweise [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled) und [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)).      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zur Zeigerauflistung hinzugefügt, und die Zeigerdetails werden angezeigt.

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, und die Zeigerdetails werden aktualisiert.

    > [!Important]
    > Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Durch das Klicken auf eine Maustaste (links, Mausrad oder rechts) wird über das [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignis eine zweite Zuordnung zwischen dem Zeiger und dieser Taste erstellt. Das [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)-Ereignis wird nur ausgelöst, wenn dieselbe Maustaste losgelassen wird (dem Zeiger kann erst eine andere Taste zugeordnet werden, wenn dieses Ereignis abgeschlossen ist). Aufgrund dieser exklusiven Zuordnung werden Klicks auf andere Maustasten über das [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)-Ereignis geleitet.     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zum Zeigerarray hinzugefügt (sofern erforderlich), und die Zeigerdetails werden angezeigt.

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)-Ereignis, bei dem der Kontakt mit dem Digitalisierungsgerät beendet wird. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus der Zeigerauflistung entfernt, und die Zeigerdetails werden aktualisiert.

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   Der folgende Handler kontrolliert das [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)-Ereignis (wenn der Kontakt mit dem Digitizer beibehalten wird). Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   Der folgende Handler kontrolliert das [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)-Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

    > [!NOTE]
    > [**PointerCaptureLost** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost) ist möglich, anstelle von [ **PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased). Die Zeigererfassung kann aus verschiedenen Gründen verloren gehen, darunter eine Benutzerinteraktion, die programmgesteuerte Erfassung von einem anderen Zeiger, das Aufrufen von [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased).     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>Abrufen von Zeigereigenschaften

Wie bereits erwähnt, müssen Sie die erweiterten Zeiger Informationen aus Abrufen einer [ **Windows.UI.Input.PointerPoint** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) Objekt abgerufen wird, über die [ **GetCurrentPoint** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) und [**GetIntermediatePoints**]https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) Methoden [ **PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs). Die folgenden Codeausschnitte zeigen, wie.

-   Zuerst wird ein neues [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Objekt für jeden Zeiger erstellt.

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   Anschließend wird Funktionalität bereitgestellt, um die Zeigerinformationen in einem vorhandenen [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Objekt zu aktualisieren, das diesem Zeiger zugeordnet ist.

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   Abschließend werden verschiedene Zeigereigenschaften abgefragt.

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>Primärer Zeiger
Einige Geräte wie ein Touch-Digitizer oder Touchpad unterstützen mehr als der typische einzelne Zeiger einer Maus oder ein Stift (in den meisten Fällen, da der Surface Hub zwei Stifteingaben unterstützt). 

Verwenden Sie die schreibgeschützte Eigenschaft **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** der **[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)**-Klasse, um einen einzelnen primären Zeiger zu identifizieren und zu unterscheiden (der primäre Zeiger ist immer der erste Zeiger, der während einer Eingabesequenz erkannt wird). 

Durch die Identifizierung des primären Zeigers können Sie diesen verwenden, um Maus- oder Stifteingaben zu emulieren, Interaktionen anzupassen oder eine andere spezifische Funktion oder Benutzeroberfläche bereitzustellen.

> [!NOTE]
> Wenn der primäre Zeiger während einer Eingabesequenz freigegeben oder abgebrochen wird bzw. verloren gegangen ist, wird erst ein primärer Eingabezeiger erstellt, wenn eine neue Eingabesequenz initiiert wird (eine Eingabesequenz endet, wenn alle Zeiger freigegeben oder abgebrochen wurden bzw. verloren gegangen sind).

## <a name="primary-pointer-animation-example"></a>Beispiel für die Animation eines primären Zeigers

Die folgenden Codeausschnitte zeigen, wie Sie spezielles visuelles Feedback bereitstellen können, damit ein Benutzer zwischen den Zeigereingaben in Ihrer Anwendung unterscheiden kann.

Diese bestimmte App verwendet Farbe und Animation, um den primären Zeiger hervorzuheben.

![Zeigeranwendung mit animiertem visuellem Feedback](images/pointers/pointers-usercontrol-animation.gif)

**Laden Sie dieses Beispiel von [Zeiger Eingabebeispiel (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Visuelles Feedback

Wir definieren eine **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)** basierend auf einem XAML-**[Ellipse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)**-Objekt, die hervorhebt, an welcher Stelle sich jeder Zeiger im Zeichenbereich befindet, und ein **[Storyboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard)** verwendet, um die Ellipse zu animieren, die dem primären Zeiger entspricht.

**Hier ist die XAML aus:**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

Und hier die CodeBehind-Datei:
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>Erstellen der Benutzeroberfläche
Die Benutzeroberfläche in diesem Beispiel ist auf den **[Eingabezeichenbereich](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)** beschränkt, in dem wir alle Zeiger nachverfolgen und die Zeigerindikatoren sowie die primäre Zeigeranimation (falls zutreffend) zusammen mit einer Kopfzeilenleiste wiedergeben, die einen Zeigerzähler und einen primären Zeigerbezeichner enthält.

Hier ist die MainPage.xaml-Datei:

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>Behandeln von Zeigerereignissen

Schließlich definieren wir unsere einfachen Zeigerereignishandler in der CodeBehind-Datei MainPage.xaml.cs. Wir werden den Code hier nicht reproduzieren, da die Grundlagen im vorherigen Beispiel behandelt wurden, aber Sie können das Arbeitsbeispiel unter [Beispiel für Zeigereingabe (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip) herunterladen.

## <a name="related-articles"></a>Verwandte Artikel

**Beispiele für Thema**
* [Eingabebeispiel Zeiger (basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
* [Eingabebeispiel für Zeiger (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

**Weitere Beispiele**
* [Grundlegende Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: XAML-benutzerbeispiel Eingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Funktionen-gerätebeispiel](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Manipulationen und Beispiel für Bewegungen (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Eingabe: Beispiel zu Leistungstests in Touch Treffer](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML Bildlauf, schwenken und Zoomen Beispiel](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Vereinfachte Freihand-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
