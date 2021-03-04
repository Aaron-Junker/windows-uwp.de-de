---
description: Empfangen, verarbeiten und Verwalten von Eingabedaten von Zeige Geräten wie Touch, Mouse, Pen/Stift und Touchpad in Ihren Windows-Anwendungen.
title: Behandeln von Zeigereingaben
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: Stift, Maus, Touchpad, Toucheingabe, Zeiger, Eingabe, Benutzerinteraktionen
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 80360ef30b50229beda813ae211966f398744941
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824674"
---
# <a name="handle-pointer-input"></a>Behandeln von Zeigereingaben

Empfangen, verarbeiten und Verwalten von Eingabedaten von Zeige Geräten (z. b. Touch, Maus, Stift/Tablettstift und Touchpad) in Ihren Windows-Anwendungen.

> [!Important]
> Erstellen Sie benutzerdefinierte Interaktionen nur, wenn eine klare, klar definierte Anforderung vorliegt und die von den Platt Form Steuerelementen unterstützten Interaktionen Ihr Szenario nicht unterstützen.  
> Wenn Sie die Interaktions Umgebungen in Ihrer Windows-Anwendung anpassen, erwarten Benutzer, dass Sie konsistent, intuitiv und auffindbar sind. Aus diesen Gründen wird empfohlen, dass Sie Ihre benutzerdefinierten Interaktionen mit den von den [Platt Form Steuerelementen](../controls-and-patterns/index.md)unterstützten Interaktionen modellieren. Die Platt Form Steuerelemente bieten eine vollständige Benutzerinteraktion mit Windows-apps, einschließlich Standard Interaktionen, animierten Physik Effekten, visuellem Feedback und Barrierefreiheit. 

## <a name="important-apis"></a>Wichtige APIs
- [Windows.Devices.Input](/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>Zeiger
Die meisten Interaktionsmöglichkeiten betreffen in der Regel den Benutzer, der das Objekt identifiziert, mit dem Sie interagieren möchten, indem Sie über Eingabegeräte wie Touch, Mouse, Pen/Stift und Touchpad darauf zeigen. Da die von diesen Eingabegeräten bereitgestellten unformatierten Eingabegeräte Daten viele allgemeine Eigenschaften enthalten, werden die Daten herauf gestuft und in einem vereinheitlichten Eingabe Stapel konsolidiert und als geräteunabhängige Zeiger Daten verfügbar gemacht. Ihre Windows-Anwendungen können diese Daten dann nutzen, ohne sich Gedanken über das verwendete Eingabegerät machen zu müssten.

> [!NOTE]
> Gerätespezifische Informationen werden auch aus den unformatierten HID-Daten herauf gestuft, wenn Sie von Ihrer APP benötigt werden.

Jeder Eingabe Punkt (oder Kontakt) im Eingabe Stapel wird durch ein [**Zeiger**](/uwp/api/Windows.UI.Xaml.Input.Pointer) Objekt dargestellt, das durch den [**pointerroutedebug**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) -Parameter in den verschiedenen Zeiger Ereignis Handlern verfügbar gemacht wird. Im Fall von multistift-oder Multitouch-Eingaben wird jeder Kontakt als eindeutiger Eingabe Zeiger behandelt.

## <a name="pointer-events"></a>Zeigerereignisse

Zeiger Ereignisse machen grundlegende Informationen verfügbar, wie z. b. Eingabe Gerätetyp und Erkennungs Zustand (im Bereich oder in Kontakt) und erweiterte Informationen, wie z. b. Standort, Druck und Kontakt Geometrie. Darüber hinaus sind auch bestimmte gerätespezifische Eigenschaften verfügbar, z. B. welche Maustaste ein Benutzer gedrückt hat oder ob die Radiergummispitze des Zeichenstifts verwendet wird. Wenn die App zwischen Eingabegeräten und ihren Funktionen unterscheiden muss, finden Sie entsprechende Informationen unter [Erkennen von Eingabegeräten](identify-input-devices.md).

Windows-Apps können auf die folgenden Zeiger Ereignisse lauschen:

> [!NOTE]
> Beschränken Sie die Zeiger Eingaben auf ein bestimmtes UI-Element, indem Sie  [**capturepointer**](/uwp/api/windows.ui.xaml.uielement.capturepointer) für dieses Element innerhalb eines Zeiger Ereignis Handlers aufrufen. Wenn ein-Zeiger von einem-Element aufgezeichnet wird, empfängt nur dieses Objekt Zeiger Eingabeereignisse, auch wenn der Zeiger außerhalb des umgebenden Bereichs des-Objekts bewegt wird. Das [**isincontact**](/uwp/api/windows.ui.xaml.input.pointer.isincontact) -Element (mit der Maus Taste gedrückt, berühren oder Tablettstift) muss "true" sein, damit **capturepointer** erfolgreich ist

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ereignis</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger von der Plattform abgebrochen wird. Dies kann in den folgenden Situationen vorkommen:</p>
<ul>
<li>Touchzeiger werden abgebrochen, wenn ein Zeichenstift innerhalb des Bereichs der Eingabeoberfläche erkannt wird.</li>
<li>Für mehr als 100 ms wird kein aktiver Kontakt erkannt.</li>
<li>Monitor/Anzeige wird geändert (Auflösung, Einstellungen, Konfigurationen mit mehreren Bildschirmen).</li>
<li>Der Desktop ist gesperrt, oder der Benutzer hat sich abgemeldet.</li>
<li>Die Anzahl gleichzeitiger Kontakte hat die vom Gerät unterstützte Anzahl überschritten.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein anderes Benutzeroberflächenelement den Zeiger erfasst, der Zeiger freigegeben wurde oder ein anderer Zeiger programmgesteuert erfasst wurde.</p>
<div class="alert">
<strong>Hinweis</strong>  Es ist kein entsprechendes Zeiger Aufzeichnungs Ereignis vorhanden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger in den Begrenzungsbereich eines Elements eintritt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Für Toucheingaben ist zur Auslösung dieses Ereignisses eine Fingerkontakt erforderlich, entweder über eine direkte Toucheingabe oder durch Bewegen in den Begrenzungsbereich des Elements.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift das Ereignis über eine direkte Stifteingabe oder durch Bewegen in den Begrenzungsbereich des Elements aus. Pen hat jedoch auch einen Hover-Zustand (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">isinrange</a>), der dieses Ereignis auslöst, wenn true.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger den Begrenzungsbereich eines Elements verlässt. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis aus, wenn sich der Mauszeiger aus dem Begrenzungsbereich des Elements heraus bewegt.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis beim Bewegen aus dem Begrenzungsbereich des Elements heraus aus. Pen hat jedoch auch einen Hover-Zustand (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">isinrange</a>), der dieses Ereignis auslöst, wenn sich der Status von true in false ändert.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn ein Zeiger Koordinaten, Schaltflächenzustand, Druck, Neigung oder Kontaktgeometrie (z. B. Breite und Höhe) innerhalb des Begrenzungsbereichs eines Elements ändert. Dies kann geringfügig anders für Touch-, Touchpad-, Maus- und Stifteingaben passieren.</p>
<ul>
<li>Die Toucheingabe erfordert einen Fingerkontakt und löst dieses Ereignis nur aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht.</li>
<li>Maus und Touchpad haben beide einen Cursor auf dem Bildschirm, der immer sichtbar ist und dieses Ereignis auslöst, auch wenn keine Maus- oder Touchpadtaste gedrückt wird.</li>
<li>Wie bei der Toucheingabe löst der Stift dieses Ereignis aus, wenn ein Kontakt innerhalb des Begrenzungsbereichs des Elements besteht. Pen hat jedoch auch einen Hover-Zustand (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">isinrange</a>), der bei true und innerhalb des umgebenden Bereichs des Elements dieses Ereignis auslöst.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Drückaktion (z. B. eine Fingereingabe, gedrückte Maustaste, Stifteingabe oder gedrückte Touchpadtaste) innerhalb des Begrenzungsbereichs eines Elements angibt.</p>
<p><a href="/uwp/api/windows.ui.xaml.uielement.capturepointer">Capturepointer</a> muss vom Handler für dieses Ereignis aufgerufen werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn der Zeiger eine Loslass-Aktion (z. B. ein Finger bewegt sich nach oben, Maustaste, Stift oder Touchpadtaste werden losgelassen) innerhalb des Begrenzungsbereichs eines Elements anzeigt, oder wenn der Zeiger außerhalb des Begrenzungsbereichs erfasst wird.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Tritt auf, wenn das Mausrad bewegt wird.</p>
<p>Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Wenn Sie auf eine Maustaste klicken (Links, Rad oder rechts), wird eine sekundäre Zuordnung zwischen dem-Zeiger und dieser Schaltfläche durch das <a href="/uwp/api/windows.ui.xaml.uielement.pointermoved">pointermove</a> -Ereignis erstellt.</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Beispiel für Zeiger Ereignis

Im folgenden finden Sie einige Code Ausschnitte aus einer grundlegenden zeigernachverfolgungs-APP, die zeigen, wie Ereignisse für mehrere Zeiger überwacht und behandelt werden und wie verschiedene Eigenschaften für die zugeordneten Zeiger erhalten werden.

![Benutzeroberfläche der Zeiger Anwendung](images/pointers/pointers1.gif)

**Herunterladen dieses Beispiels aus dem Beispiel für eine [Zeiger Eingabe (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Erstellen der Benutzeroberfläche

In diesem Beispiel verwenden wir ein [Rechteck](/uwp/api/windows.ui.xaml.shapes.rectangle) ( `Target` ) als Objekt, das Zeiger Eingaben verwendet. Die Farbe des Ziels ändert sich, wenn sich der Zeigerstatus ändert.

Details zu den einzelnen Zeigern werden in einem Gleit Komma [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) angezeigt, der dem Zeiger beim Verschieben folgt. Die Zeiger Ereignisse selbst werden im [richtextblock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) auf der rechten Seite des Rechtecks gemeldet.

Dies ist der Extensible Application Markup Language (XAML) für die Benutzeroberfläche in diesem Beispiel. 

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

In den meisten Fällen wird empfohlen, Zeigerinformationen über die [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) des Ereignishandlers abzurufen.

Sollte das Ereignisargument die erforderlichen Zeigerdetails nicht liefern, können Sie über die Methoden [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) und [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) von [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) auf die von einem [**PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint)-Objekt bereitgestellten erweiterten Zeigerdaten zugreifen.

Der folgende Code richtet das globale Dictionary-Objekt für die Nachverfolgung der einzelnen aktiven Zeiger ein und identifiziert die verschiedenen Zeiger Ereignislistener für das Zielobjekt.

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

-   Dieser Handler verwaltet das [**pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) -Ereignis. Das Ereignis wird dem Ereignisprotokoll hinzugefügt, der Zeiger wird dem aktiven Zeiger Wörterbuch hinzugefügt, und die Zeiger Details werden angezeigt.

    > [!NOTE]
    > [**Pointerpressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) -und [**pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) -Ereignisse treten nicht immer paarweise auf. Ihre APP sollte auf jedes Ereignis lauschen und dieses behandeln, das möglicherweise einen Zeiger abschließt (z. b. " [**pointerexited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)", " [**pointerabgeb Rochen**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)" und " [**pointercapturelost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)").
         

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

-   Dieser Handler verwaltet das Ereignis [**pointereingetragen**](/uwp/api/windows.ui.xaml.uielement.pointerentered) . Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zur Zeigerauflistung hinzugefügt, und die Zeigerdetails werden angezeigt.

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

-   Dieser Handler verwaltet das [**pointerverschoderte**](/uwp/api/windows.ui.xaml.uielement.pointermoved) Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, und die Zeigerdetails werden aktualisiert.

    > [!Important]
    > Die Mauseingabe wird einem einzelnen Zeiger zugeordnet, der bei der ersten Ermittlung einer Mauseingabe zugewiesen wird. Durch das Klicken auf eine Maustaste (links, Mausrad oder rechts) wird über das [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignis eine zweite Zuordnung zwischen dem Zeiger und dieser Taste erstellt. Das [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)-Ereignis wird nur ausgelöst, wenn dieselbe Maustaste losgelassen wird (dem Zeiger kann erst eine andere Taste zugeordnet werden, wenn dieses Ereignis abgeschlossen ist). Aufgrund dieser exklusiven Zuordnung werden Klicks auf andere Maustasten über das [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)-Ereignis geleitet.     

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

-   Dieser Handler verwaltet das [**pointerwheelchanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) -Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird zum Zeigerarray hinzugefügt (sofern erforderlich), und die Zeigerdetails werden angezeigt.

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

-   Dieser Handler verwaltet das [**pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) -Ereignis, bei dem der Kontakt mit dem Digitalisierer beendet wird. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus der Zeigerauflistung entfernt, und die Zeigerdetails werden aktualisiert.

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

-   Dieser Handler verwaltet das Ereignis " [**pointerexited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) " (bei Beibehaltung des Kontakts mit dem Digitalisierer). Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

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

-   Dieser Handler verwaltet das Ereignis " [**pointerabgeb Rochen**](/uwp/api/windows.ui.xaml.uielement.pointercanceled) ". Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

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

-   Dieser Handler verwaltet das [**pointercapturelost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) -Ereignis. Das Ereignis wird zum Ereignisprotokoll hinzugefügt, der Zeiger wird aus dem Zeigerarray entfernt, und die Zeigerdetails werden aktualisiert.

    > [!NOTE]
    > [**Pointercapturelost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) kann anstelle von [**pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)auftreten. Die Zeiger Erfassung kann aus verschiedenen Gründen verloren gehen, einschließlich der Benutzerinteraktion, der programmgesteuerten Erfassung eines anderen Zeigers, dem Aufrufen von [**pointerreleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased).     

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

Wie bereits erwähnt, müssen Sie die erweiterten Zeigerinformationen von einem [**Windows.UI.Input.PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint)-Objekt abrufen, das über die Methoden [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) und [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) von [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) bereitgestellt wird. Die folgenden Code Ausschnitte zeigen, wie Sie sehen.

-   Zuerst wird ein neues [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Objekt für jeden Zeiger erstellt.

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

-   Anschließend wird Funktionalität bereitgestellt, um die Zeigerinformationen in einem vorhandenen [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Objekt zu aktualisieren, das diesem Zeiger zugeordnet ist.

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
Einige Eingabegeräte, z. b. ein Touch-Digitalisierungs Modul oder Touchpad, unterstützen mehr als den typischen einzelnen Zeiger einer Maus oder eines Stifts (in den meisten Fällen, da das Surface Hub zwei Stift Eingaben unterstützt). 

Verwenden Sie die schreibgeschützte **[IsPrimary](/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** -Eigenschaft der **[pointerpointerproperties](/uwp/api/windows.ui.input.pointerpointproperties)** -Klasse, um einen einzelnen primären Zeiger zu identifizieren und zu unterscheiden (der primäre Zeiger ist immer der erste Zeiger, der während einer Eingabe Sequenz erkannt wird). 

Indem Sie den primären Zeiger identifizieren, können Sie ihn zum Emulieren von Maus-oder Stift Eingaben, Anpassen von Interaktionen oder Bereitstellen anderer spezifischer Funktionen oder Benutzeroberflächen verwenden.

> [!NOTE]
> Wenn der primäre Zeiger während einer Eingabe Sequenz freigegeben, abgebrochen oder verloren geht, wird erst dann ein primärer Eingabe Zeiger erstellt, wenn eine neue Eingabe Sequenz initiiert wird (eine Eingabe Sequenz endet, wenn alle Zeiger freigegeben, abgebrochen oder verloren gegangen sind).

## <a name="primary-pointer-animation-example"></a>Beispiel für eine Animation im primären Zeiger

Diese Code Ausschnitte zeigen, wie Sie spezielles visuelles Feedback bereitstellen können, um einen Benutzer bei der Unterscheidung zwischen Zeiger Eingaben in der Anwendung zu unterstützen.

Diese spezielle App verwendet sowohl Farbe als auch Animation, um den primären Zeiger hervorzuheben.

![Zeiger Anwendung mit animiertem visuellen Feedback](images/pointers/pointers-usercontrol-animation.gif)

**Herunterladen dieses Beispiels aus dem [Zeiger Eingabe Beispiel (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Visuelles Feedback

Wir definieren ein **[UserControl-Steuer](/uwp/api/windows.ui.xaml.controls.usercontrol)** Element auf Grundlage eines XAML- **[Ellipse](/uwp/api/windows.ui.xaml.shapes.ellipse)** -Objekts, das hervorhebt, wo sich die einzelnen Zeiger auf der Canvas befinden, und verwendet ein **[Storyboard](/uwp/api/windows.ui.xaml.media.animation.storyboard)** zum Animieren der Ellipse, die dem primär Zeiger entspricht.

**Dies ist der XAML-Code:**

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

Und hier ist der Code Behind:
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
Die Benutzeroberfläche in diesem Beispiel ist auf **[den Eingabebereich](/uwp/api/windows.ui.xaml.controls.canvas)** beschränkt, in dem wir alle Zeiger nachverfolgen und die Zeiger Indikatoren und die primäre Zeiger Animation (falls zutreffend) zusammen mit einer Header Leiste mit einem Zeiger-und einem primär Zeiger Bezeichner darstellen.

Dies ist die Datei "MainPage. XAML":

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

Schließlich definieren wir die grundlegenden Zeiger Ereignishandler im MainPage.XAML.cs-Code Behind. Der Code wird hier nicht reproduziert, weil die Grundlagen im vorherigen Beispiel behandelt wurden, aber Sie können das Arbeitsbeispiel aus dem Beispiel für eine [Zeiger Eingabe (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)herunterladen.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="topic-samples"></a>Themenbeispiele

- [Beispiel für Zeiger Eingabe (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
- [Beispiel für Zeiger Eingabe (UserControl mit Animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

### <a name="other-samples"></a>Weitere Beispiele

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Manipulationen und Gesten (Beispiel)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Eingabe: Beispiel für Fingereingabe-Treffertests](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: vereinfachtes Freihandbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)