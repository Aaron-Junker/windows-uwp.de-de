---
author: Karl-Bridge-Microsoft
Description: Respond to keystroke actions from hardware or software keyboards in your apps using both keyboard and class event handlers.
title: Tastaturereignisse
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: Tastatur, Gamepad, Fernbedienung, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen, NACH-OBEN-TASTE, NACH-UNTEN-TASTE
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 78448081b81e7e28c4b97fcfdd7aa71ae32aeb0c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "5158330"
---
# <a name="keyboard-events"></a>Tastaturereignisse

## <a name="keyboard-events-and-focus"></a>Tastaturereignisse und Fokus

Die folgenden Tastaturereignisse können sowohl für Hardware- als auch für Touch-Bildschirmtastaturen eintreten.

| Ereignis                                      | Beschreibung                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | Tritt auf, wenn eine Taste gedrückt wird.  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | Tritt auf, wenn eine Taste losgelassen wird. |

> [!IMPORTANT]
> Einige Windows-Runtime-Steuerelemente verarbeiten Eingabeereignisse intern. In diesen Fällen tritt ein Eingabeereignis möglicherweise nicht ein, weil der Ereignislistener den zugehörigen Handler nicht aufruft. Normalerweise wird diese Tastenteilmenge vom Klassenhandler verarbeitet, um eine integrierte Unterstützung für die Barrierefreiheit des einfachen Tastaturzugriffs bereitzustellen. Die Klasse [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) setzt beispielsweise die [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982)-Ereignisse für die LEERTASTE und die EINGABETASTE (sowie für [**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989)) außer Kraft und leitet sie an das Ereignis [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) des Steuerelements weiter. Wenn von der Steuerelementklasse ein Tastendruck verarbeitet wird, werden die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) nicht ausgelöst.  
> Dadurch wird ein integriertes Tastaturäquivalent zum Aufrufen der Schaltfläche bereitgestellt, das dem Tippen mit dem Finger oder Klicken mit einer Maus ähnelt. Andere Tasten als die LEER- oder EINGABETASTE lösen die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) trotzdem aus. Weitere Informationen zur Funktionsweise der klassenbasierten Verarbeitung von Ereignissen (insbesondere den Abschnitt „Eingabe-Ereignishandler in Steuerelementen“) finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).


Die Steuerelemente der UI generieren nur dann Tastaturereignisse, wenn sie den Eingabefokus aufweisen. Ein einzelnes Steuerelement steht im Fokus, wenn Benutzer im Layout auf das Steuerelement klicken oder tippen oder mit der TAB-Taste im Inhaltsbereich eine Aktivierreihenfolge durchlaufen.

Sie können auch die [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161)-Methode eines Steuerelements aufrufen, um den Fokus zu erzwingen. Dies ist notwendig, wenn Sie Tastenkombinationen implementieren, da der Tastaturfokus beim Laden der UI nicht standardmäßig festgelegt wird. Weitere Informationen finden Sie weiter unten in diesem Thema unter **Beispiel für Tastenkombinationen**.

Damit ein Steuerelement den Eingabefokus erhält, muss es aktiviert und sichtbar sein. Außerdem müssen die Eigenschaften [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) und [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) den Wert **true** haben. Dies ist der Standardzustand der meisten Steuerelemente. Wenn ein Steuerelement den Eingabefokus aufweist, kann es Tastatureingabeereignisse auslösen und auf diese reagieren. Dies wird weiter unten in diesem Thema beschrieben. Mit den Ereignissen [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) und [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) können Sie außerdem reagieren, wenn ein Steuerelement den Fokus erhält oder verliert.

Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der Reihenfolge, in der sie in der Extensible Application Markup Language (XAML) angezeigt werden. Mit der Eigenschaft [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) können Sie diese Reihenfolge jedoch ändern. Weitere Informationen finden Sie unter [Implementieren von Barrierefreiheit für den Tastaturzugriff](https://msdn.microsoft.com/library/windows/apps/hh868161).

## <a name="keyboard-event-handlers"></a>Tastaturereignishandler


Ein Eingabe-Ereignishandler implementiert einen Delegaten, der die folgenden Informationen bereitstellt:

-   Den Sender des Ereignisses. Der Sender meldet das Objekt, dem der Ereignishandler angefügt ist.
-   Ereignisdaten. Bei Tastaturereignissen sind diese Daten eine Instanz von [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072). [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904) ist der Delegat für Handler. Die wichtigsten Eigenschaften von **KeyRoutedEventArgs** für die meisten Handler-Szenarien sind [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) und möglicherweise [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075).
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810). Da es sich bei Tastaturereignissen um Routingereignisse handelt, stellen die Ereignisdaten **OriginalSource** bereit. Wenn Sie bewusst das Bubbling von Ereignissen in der Objektstruktur zulassen, ist **OriginalSource** manchmal eher das fragliche Objekt als der Sender. Das hängt jedoch vom Design ab. Weitere Informationen dazu, wie Sie **OriginalSource** anstelle des Senders verwenden können, finden Sie im Abschnitt „Routingereignisse der Tastatur“ in diesem Thema oder unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

### <a name="attaching-a-keyboard-event-handler"></a>Anfügen eines Tastaturereignishandlers

Sie können Funktionen von Tastatur-Ereignishandlern für jedes Objekt anfügen, das das Ereignis als Member einschließt. Dazu gehört jede von [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) abgeleitete Klasse. Das folgende XAML-Beispiel zeigt, wie Sie Handler für das Ereignis [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) für [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) anfügen.

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

Sie können einen Ereignishandler auch manuell im Code anfügen. Weitere Informationen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

### <a name="defining-a-keyboard-event-handler"></a>Definieren eines Tastaturereignishandlers

Das folgende Beispiel zeigt eine unvollständige Ereignishandler-Definition für den im vorherigen Beispiel hinzugefügten Ereignishandler [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942).

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>Verwenden von KeyRoutedEventArgs

Alle Tastaturereignisse verwenden [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) für Ereignisdaten. **KeyRoutedEventArgs** enthält die folgenden Eigenschaften:

-   [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) (geerbt von [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809))

### <a name="key"></a>Key

Das Ereignis [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) wird ausgelöst, wenn eine Taste gedrückt wird. Entsprechend wird das Ereignis [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) ausgelöst, wenn eine Taste losgelassen wird. In der Regel lauschen Sie auf Ereignisse, um einen bestimmten Tastenwert zu verarbeiten. Überprüfen Sie den Wert [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) in den Ereignisdaten, um die gedrückte oder losgelassene Taste zu ermitteln. **Key** gibt einen [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812)-Wert zurück. Die **VirtualKey**-Enumeration umfasst alle unterstützten Tasten.

### <a name="modifier-keys"></a>Zusatztasten

Zusatztasten sind Tasten wie beispielsweise STRG oder UMSCHALT, die Benutzer normalerweise in Kombination mit anderen Tasten drücken. Ihre App kann diese Kombinationen als Tastenkombinationen zum Aufrufen von App-Befehlen nutzen.

Sie erkennen Tastenkombinationen mithilfe des Codes in den Ereignishandlern [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942). Sie können dann den Zustand "Gedrückt" der Zusatztasten überwachen, die für Sie von Interesse sind. Wenn für eine Nichtzusatztaste ein Tastaturereignis auftritt, können Sie überprüfen, ob gleichzeitig eine Zusatztaste gedrückt wurde.

> [!NOTE]
> Die ALT-Taste wird durch den Wert **VirtualKey.Menu** dargestellt.

 

### <a name="shortcut-keys-example"></a>Beispiel für Tastenkombinationen


Im folgenden Beispiel wird die Implementierung von Tastenkombinationen gezeigt. In diesem Beispiel können Benutzer die Medienwiedergabe mit den Schaltflächen „Play“, „Pause“ und „Stop“ oder mit den Tastenkombinationen STRG+P, STRG+A und STRG+S steuern. Das Schaltflächen-XAML zeigt die Tastenkombinationen in Form von QuickInfos und [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081)-Eigenschaften in den Schaltflächenbeschriftungen. Diese Selbstdokumentation ist wichtig, um die Benutzerfreundlichkeit und Barrierefreiheit der App zu erhöhen. Weitere Informationen finden Sie unter [Barrierefreiheit für den Tastaturzugriff](https://msdn.microsoft.com/library/windows/apps/mt244347).

Beachten Sie auch, dass die Seite den Eingabefokus auf sich selbst festlegt, wenn sie geladen wird. Ohne diesen Schritt hat kein Steuerelement anfangs den Eingabefokus, und die App löst erst Eingabeereignisse aus, wenn Benutzer den Eingabefokus manuell festlegen (beispielsweise durch Drücken der TAB-TASTE oder Klicken auf ein Steuerelement).

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> Durch das Festlegen der [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762)- oder [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763)-Eigenschaft in XAML werden Zeichenfolgeninformationen zum Dokumentieren der Tastenkombination zum Auslösen der jeweiligen Aktion bereitgestellt. Die Informationen werden von Microsoft-Benutzeroberflächenautomatisierungsclients wie der Sprachausgabe erfasst und normalerweise direkt für den Benutzer bereitgestellt.
>
> Mit dem Festlegen der Eigenschaft **AutomationProperties.AcceleratorKey** oder **AutomationProperties.AccessKey** ist keine eigene Aktion verknüpft. Sie müssen weiterhin Handler für [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)- oder [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Ereignisse anhängen, um das Verhalten der Tastenkombination tatsächlich in die App zu implementieren. Außerdem wird der Unterstrichzusatz für eine Zugriffstaste nicht automatisch bereitgestellt. Sie müssen den Text für die jeweilige Taste in Ihrem mnemonischen Zeichen explizit als [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982)-Formatierung unterstreichen, wenn in der UI unterstrichener Text angezeigt werden soll.

 

## <a name="keyboard-routed-events"></a>Routingereignisse der Tastatur


Bestimmte Ereignisse gelten als Routingereignisse, wie z.B. [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942). Routingereignisse verwenden die Bubbling-Routingstrategie. Bei der Bubbling-Routingstrategie geht ein Ereignis von einem untergeordneten Objekt aus und wird jeweils an die übergeordneten Objekte in der Struktur weitergeleitet. Dadurch ergibt sich eine weitere Möglichkeit, dasselbe Ereignis zu behandeln und mit denselben Ereignisdaten zu interagieren.

Im folgenden XAML-Beispiel werden [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Ereignisse für ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Objekt und zwei [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Objekte definiert. Wenn Sie in diesem Fall eine Taste loslassen, während der Fokus auf einem der **Button**-Objekte liegt, wird das Ereignis **KeyUp** ausgelöst. Das Ereignis wird per Bubbling an die übergeordnete **Canvas** weitergeleitet.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

Das folgende Beispiel zeigt, wie der Ereignishandler [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) für den entsprechenden XAML-Inhalt im vorherigen Beispiel implementiert wird.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

Beachten Sie die Verwendung der Eigenschaft [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) im vorherigen Handler. Hier meldet **OriginalSource** das Objekt, das das Ereignis ausgelöst hat. Bei dem Objekt kann es sich nicht um [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) handeln, da **StackPanel** kein Steuerelement ist und nicht im Fokus stehen kann. Nur eine der beiden Schaltflächen in **StackPanel** kann das Ereignis ausgelöst haben. Die Frage ist, welche? Mit **OriginalSource** können Sie das tatsächliche Quellobjekt des Ereignisses unterscheiden, wenn Sie das Ereignis für ein übergeordnetes Objekt verarbeiten.

### <a name="the-handled-property-in-event-data"></a>Die Handled-Eigenschaft in Ereignisdaten

Je nach Ihrer Strategie für die Ereignisverarbeitung empfiehlt sich vielleicht nur ein Ereignishandler, der auf ein Bubbling-Ereignis reagiert. Wenn beispielsweise ein bestimmter [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)-Handler an eines der [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Steuerelemente angefügt ist, hat dieses zuerst Gelegenheit, das Ereignis zu verarbeiten. In diesem Fall ist es nicht sinnvoll, dass auch das übergeordnete Panel das Ereignis behandelt. Verwenden Sie in diesem Szenario die Eigenschaft [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) in den Ereignisdaten.

Der Zweck der Eigenschaft [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) in der Datenklasse eines Routingereignisses besteht darin, zu melden, dass ein anderer Handler, den Sie an früherer Stelle in der Ereignisroute registriert haben, bereits tätig war. Dies beeinflusst das Verhalten des Routingereignissystems. Wenn Sie **Handled** in einem Ereignishandler auf **true** festlegen, ist das Routing eines Ereignisses beendet, und das Ereignis wird nicht mehr an nachfolgende übergeordnete Elemente gesendet.

### <a name="addhandler-and-already-handled-keyboard-events"></a>"AddHandler " und bereits behandelte Tastaturereignisse

Sie können eine spezielle Technik verwenden, um Handler anzufügen, die auf bereits als verarbeitet markierte Ereignisse reagieren können. Bei dieser Technik wird mit der Methode [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) ein Handler registriert, und es werden keine XAML-Attribute und keine sprachspezifische Syntax verwendet, die Handler wie „+=“ in C\# hinzufügen.

Eine generelle Einschränkung dieser Technik besteht darin, dass die API **AddHandler** einen Parameter vom Typ [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) verwendet, der das fragliche Routingereignis identifiziert. Nicht alle Routingereignisse bieten einen **RoutedEvent**-Bezeichner. Dieser Umstand wirkt sich somit darauf aus, welche Routingereignisse im Fall [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) noch verarbeitet werden können. Die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) haben Routingereignisbezeichner ([**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702416) und [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418)) in [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911). Andere Texteingabe-Ereignisse, wie [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706), besitzen jedoch keine Routingereignisbezeichner und können deshalb für die Technik **AddHandler** nicht verwendet werden.

### <a name="overriding-keyboard-events-and-behavior"></a>Überschreiben von Tastaturereignissen und Verhalten

Sie können die wichtigsten Ereignisse für bestimmte Steuerelemente überschreiben (z.B. [**GridView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridView)), um eine konsistente Fokusnavigation für verschiedene Eingabegeräte (darunter Tastatur und Gamepad) bereitzustellen.

Im folgenden Beispiel wird das Steuerelement und überschreiben das KeyDown-Verhalten Fokus zum GridView Inhalte, wenn eine Pfeiltaste betätigt wird.

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> Wenn ein GridView nur für Layoutzwecke verwendet wird, sollten Sie möglicherweise andere Steuerelemente verwenden, wie z.B. [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl) mit [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsWrapGrid).

## <a name="commanding"></a>Befehle

Eine geringe Anzahl von UI-Elementen verfügt über integrierte Unterstützung für die Steuerung. Die Steuerung verwendet eingabebezogene Routingereignisse in der zugrundeliegenden Implementierung. Sie ermöglicht die Verarbeitung verwandter UI-Eingaben (eine bestimmte Zeigeraktion oder Zugriffstaste) durch das Aufrufen eines einzelnen Befehlshandlers.

Wenn die Steuerung für ein UI-Element verfügbar ist, sollten Sie dessen Steuerungs-APIs anstelle einzelner Eingabeereignisse verwenden. Weitere Informationen finden Sie unter [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740).

Sie können auch [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) implementieren, um die Befehlsfunktionalität zu kapseln, die Sie über normale Ereignishandler aufrufen. Auf diese Weise können Sie die Steuerung auch verwenden, wenn keine **Command**-Eigenschaft verfügbar ist.

## <a name="text-input-and-controls"></a>Texteingabe und Steuerelemente

Bestimmte Steuerelemente reagieren mit einer eigenen Verarbeitung auf Tastaturereignisse. So ist beispielsweise das Steuerelement [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) dazu gedacht, über die Tastatur eingegebenen Text zu erfassen und visuell darzustellen. Es verwendet in seiner eigenen Logik [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) und [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) zum Erfassen von Tastaturanschlägen und löst dann auch sein eigenes [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)-Ereignis aus, wenn der Text tatsächlich geändert wurde.

Trotzdem können Sie generell einer [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br208942) oder einem verwandten Steuerelement, das der Verarbeitung von Texteingaben dient, Handler für [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br209683) hinzufügen. Jedoch reagiert ein Steuerelement möglicherweise im Rahmen seines Designs nicht auf alle Tastenwerte, die es über Tastaturereignisse empfängt. Das Verhalten hängt ganz vom jeweiligen Steuerelement ab.

Beispielsweise verarbeitet [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) (die Basisklasse von [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)) [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) so, dass eine Überprüfung für die LEERTASTE oder für die EINGABETASTE durchgeführt werden kann. **ButtonBase** verarbeitet **KeyUp** analog zu einem Ereignis für das Drücken der linken Maustaste, um ein [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737)-Ereignis auszulösen. Diese Verarbeitung des Ereignisses wird beim Überschreiben der virtuellen Methode [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) durch **ButtonBase** erreicht. In der Implementierung wird [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) auf **true** festgelegt. Als Ergebnis empfängt kein übergeordnetes Element einer Schaltfläche, die bei der LEERTASTE auf ein Tastaturereignis lauscht, das bereits verarbeitete Element für seine eigenen Handler.

[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) ist ein weiteres Beispiel. Einige Tasten, wie die Pfeiltasten, werden von **TextBox** nicht als Text angesehen und gelten stattdessen als spezifisch für das UI-Steuerelementverhalten **TextBox** markiert diese Ereignisse als verarbeitet.

Benutzerdefinierte Steuerelemente können ein ähnliches eigenes Überschreibungsverhalten für Tastaturereignisse implementieren und [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) überschreiben. Wenn Ihr benutzerdefiniertes Steuerelement bestimmte Zugriffstasten verarbeitet oder ein Steuerelement- oder Fokusverhalten besitzt, das mit dem für [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) geschilderten Szenario vergleichbar ist, sollten Sie diese Logik in Ihre eigenen **OnKeyDown** / **OnKeyUp**-Überschreibungen aufnehmen.

## <a name="the-touch-keyboard"></a>Die Touch-Bildschirmtastatur

Steuerelemente für die Texteingabe bieten automatische Unterstützung für die Touch-Bildschirmtastatur. Wenn Benutzer den Eingabefokus per Fingereingabe auf ein Textsteuerelement festlegen, wird automatisch die Bildschirmtastatur angezeigt. Wenn sich der Eingabefokus nicht auf einem Textsteuerelement befindet, ist die Bildschirmtastatur ausgeblendet.

Wenn die Bildschirmtastatur angezeigt wird, wird Ihre UI automatisch umpositioniert, um sicherzustellen, dass das Element mit dem Fokus sichtbar bleibt. Dies kann dazu führen, dass andere wichtige Bereiche der UI nicht mehr auf dem Bildschirm angezeigt werden. Sie können das Standardverhalten jedoch deaktivieren und eigene UI-Anpassungen vornehmen, wenn die Bildschirmtastatur eingeblendet wird. Weitere Informationen finden Sie im [Beispiel für die Bildschirmtastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Wenn Sie ein benutzerdefiniertes Steuerelement erstellen, das zwar Texteingabe erfordert, aber nicht von einem Standardsteuerelement für die Texteingabe abgeleitet ist, können Sie Unterstützung für die Touch-Bildschirmtastatur hinzufügen, indem Sie die richtigen Steuerelementmuster der Benutzeroberflächenautomatisierung implementieren. Weitere Informationen finden Sie im [Beispiel für die Bildschirmtastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Durch Drücken von Tasten auf der Touch-Bildschirmtastatur werden genau wie beim Drücken von Tasten auf Hardwaretastaturen die Ereignisse [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) und [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) ausgelöst. Die Touch-Bildschirmtastatur löst jedoch keine Eingabeereignisse für STRG+A, STRG+Z, STRG+X, STRG+C und STRG+V aus, da diese Tastenkombinationen für Textänderungen im Eingabesteuerelement reserviert sind.

Benutzer können Daten in Ihre App schneller und einfacher eingeben, wenn Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen. Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die Eigenschaft [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) auf [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028) fest. Dies weist das System an, das Layout der Zehnertastatur anzuzeigen, das dem Benutzer die Eingabe der PIN erleichtert. Weitere Informationen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/mt280229).

## <a name="related-articles"></a>Verwandte Artikel

**Entwickler**
* [Tastaturinteraktionen](keyboard-interactions.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
* [Reagieren auf das Vorhandensein der Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md)

**Designer**
* [Tastaturentwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/hh972345)

**Beispiele**
* [Beispiel für die Bildschirmtastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Einfaches Eingabebeispiel](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Beispiel für Eingabe mit niedriger Latenz](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für visuelle Fokuselemente](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabebeispiel](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Beispiel für Gerätefunktionen](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel für die Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Beispiel für die Reaktion auf die Anzeige der Bildschirmtastatur](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Beispiel für die XAML-Textbearbeitung](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 
