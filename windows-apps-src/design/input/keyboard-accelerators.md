---
Description: Erfahren Sie, wie Tastenkombinationen die Benutzerfreundlichkeit und Barrierefreiheit von Windows-apps verbessern können.
title: Tastaturkürzel
label: Keyboard accelerators
template: detail.hbs
keywords: Tastatur, Accelerator, Zugriffstaste, Tastenkombinationen, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen, Gamepad, Remote
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 8a3231f608984c9d1f67df71de9cab4cfecd9a13
ms.sourcegitcommit: deb2867924ce16efcabfa011892157b7aa4fa2d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89187895"
---
# <a name="keyboard-accelerators"></a>Tastaturkürzel

![Oberfläche Tastatur](images/accelerators/accelerators_hero2.png)

Zugriffstasten (oder Tastaturbeschleuniger) sind Tastenkombinationen, die die Benutzerfreundlichkeit und Barrierefreiheit Ihrer Windows-Anwendungen verbessern, indem Sie Benutzern eine intuitive Möglichkeit bieten, gängige Aktionen oder Befehle aufzurufen, ohne in der App-Benutzeroberfläche zu navigieren.

Weitere Informationen zum Navigieren in der Benutzeroberfläche einer Windows-Anwendung mit Tastenkombinationen finden Sie im Thema [Zugriffsschlüssel](access-keys.md) .

> [!NOTE]
> Eine Tastatur ist für Benutzer mit bestimmten Behinderungen unverzichtbar (siehe [Tastatur Barrierefreiheit](../accessibility/keyboard-accessibility.md)) und ist auch ein wichtiges Tool für Benutzer, die Sie als effizientere Methode für die Interaktion mit einer APP bevorzugen.

## <a name="overview"></a>Übersicht

Zugriffstasten enthalten in der Regel die Funktionstasten F1 bis F12 oder eine Kombination aus einem Standardschlüssel, der mit einer oder mehreren modifiziererschlüsseln gekoppelt ist (STRG, UMSCHALT).

> [!NOTE]
> UWP-Platt Form Steuerelemente verfügen über integrierte Tastaturbeschleuniger. ListView unterstützt z. b. STRG + A, um alle Elemente in der Liste auszuwählen, und richeditbox unterstützt STRG + TAB, um eine Registerkarte in das Textfeld einzufügen. Diese integrierten Tastaturbeschleuniger werden als **Steuerungs Beschleuniger** bezeichnet und nur ausgeführt, wenn sich der Fokus auf dem Element oder einem seiner untergeordneten Elemente befindet. Zugriffstasten, die durch Sie mithilfe der hier beschriebenen Tastatur Zugriffs-APIs definiert werden, werden als **App Accelerators**bezeichnet.

Tastaturbeschleuniger sind nicht für jede Aktion verfügbar, aber häufig mit Befehlen verknüpft, die in Menüs verfügbar gemacht werden (und mit dem Menü Element Inhalt angegeben werden sollten).Accelerators können auch Aktionen zugeordnet werden, die keine entsprechenden Menü Elemente aufweisen. Da sich die Benutzer jedoch auf die Menüs einer Anwendung verlassen, um den verfügbaren Befehlssatz zu ermitteln und zu erlernen, sollten Sie versuchen, die Ermittlung von Accelerators so einfach wie möglich zu machen (mit Bezeichnungen oder eingerichteten Mustern können Sie dies unterstützen).

![In einer Menü Element Bezeichnung beschriebene Tastaturbeschleuniger](images/accelerators/accelerators_menuitemlabel.png)  
*In einer Menü Element Bezeichnung beschriebene Tastaturbeschleuniger*

## <a name="when-to-use-keyboard-accelerators"></a>Verwendung von Tastatur Accelerators

Es wird empfohlen, dass Sie bei Bedarf Tastaturbeschleuniger in der Benutzeroberfläche angeben und Accelerators in allen benutzerdefinierten Steuerelementen unterstützen.

- Mithilfe von Tastenkombinationen können Sie Benutzern mit motorischen Behinderungen den Zugriff auf Ihre APP erleichtern, einschließlich der Benutzer, die jeweils nur einen Schlüssel drücken können oder Schwierigkeiten mit der Maus haben. * *

  Eine gut durchdachte Tastatur-UI ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u. U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie etwa Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen. Für diese Benutzer ist eine umfassende Befehls Abdeckung von entscheidender Bedeutung.

- Mithilfe von Tastatur Accelerators können Sie Ihre APP für Hauptbenutzer verwendbar machen, die die Interaktion mit der Tastatur bevorzugen.

  Erfahrene Benutzer haben oft eine starke Vorliebe für die Verwendung der Tastatur, da Tastatur basierte Befehle schneller eingegeben werden können und Sie nicht verlangen, Ihre Hände von der Tastatur zu entfernen. Für diese Benutzer sind Effizienz und Konsistenz entscheidend. Die Vollständigkeit hingegen ist nur für am häufigsten verwendeten Befehle wichtig.

## <a name="specify-a-keyboard-accelerator"></a>Tastaturbeschleuniger angeben

Verwenden Sie die [keyboardaccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) -APIs zum Erstellen von Tastatur Accelerators in UWP-apps. Mit diesen APIs müssen Sie nicht mehrere KeyDown-Ereignisse behandeln, um die gedrückte Tastenkombination zu erkennen, und Sie können Acceleratoren in den App-Ressourcen lokalisieren.

Es wird empfohlen, dass Sie für die gängigsten Aktionen in der APP Tastaturbeschleuniger festlegen und diese mithilfe der Menü Element Bezeichnung oder der QuickInfo dokumentieren. In diesem Beispiel deklarieren wir Tastaturbeschleuniger nur für die Befehle Rename und Copy.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![In einer QuickInfo beschriebene Tastatur Beschleunigung](images/accelerators/accelerators_tooltip.png)  
***In einer QuickInfo beschriebene Tastatur Beschleunigung***

Das [UIElement](/uwp/api/windows.ui.xaml.uielement) -Objekt verfügt über eine [keyboardaccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) -Auflistung ( [keyboardaccelerators](/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators)), in der Sie die benutzerdefinierten keyboardaccelerator-Objekte angeben und die Tastatureingaben für die Zugriffstaste definieren:

-   **[Key](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** : der [virtualkey](/uwp/api/windows.system.virtualkey) , der für die Zugriffstaste verwendet wird.

-   **[Modifiziererer](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** – die für die Tastatur Zugriffstaste verwendeten [virtualkeymodifier](/uwp/api/windows.system.virtualkeymodifiers) . Wenn Modifizierer nicht festgelegt ist, ist der Standardwert None.

> [!NOTE]
> Die Tastenkombination Single Key (A, DELETE, F2, SPACEBAR, ESC, Multimedia Key) und Multi-Key Accelerators (STRG + UMSCHALT + M) werden unterstützt. Die virtuellen Gamepad-Schlüssel werden jedoch nicht unterstützt.

## <a name="scoped-accelerators"></a>Bereichs bezogene Acceleratoren

Einige Accelerators funktionieren nur in bestimmten Bereichen, während andere APP-weit funktionieren.

Microsoft Outlook enthält z. b. die folgenden Accelerators:
-   STRG + B, STRG + I und ESC arbeiten nur im Bereich des e-Mail-Sende Formulars.
-   STRG + 1 und STRG + 2 arbeiten App-weit

### <a name="context-menus"></a>Kontextmenüs

Kontextmenü Aktionen wirken sich nur auf bestimmte Bereiche oder Elemente aus, z. b. die ausgewählten Zeichen in einem Text-Editor oder ein Lied in einer Wiedergabeliste. Aus diesem Grund wird empfohlen, den Bereich der Tastaturbeschleuniger für Kontextmenü Elemente auf das übergeordnete Element des Kontextmenüs festzulegen.

Verwenden Sie die Eigenschaft [scopeowner](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) , um den Gültigkeitsbereich der Tastatur Zugriffstaste anzugeben. In diesem Code wird veranschaulicht, wie ein Kontextmenü für eine ListView mit Bereichs bezogenen Tastatur Accelerators implementiert wird:

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

Das scopeowner-Attribut des menuflyoutitem. keyboardaccelerators-Elements markiert die Zugriffstaste anstelle von Global (der Standardwert ist NULL oder Global). Weitere Details finden Sie weiter unten in diesem Thema im Abschnitt **Auflösen von Accelerators** .

## <a name="invoke-a-keyboard-accelerator"></a>Aufrufen einer Tastatur Beschleunigung 

Das [keyboardaccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) -Objekt verwendet das [UI Automation (UIA)-Steuerelement Muster](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) , um eine Aktion auszuführen, wenn eine Zugriffstaste aufgerufen wird.

Die UIA [Steuerelement Muster] machen allgemeine Steuerungsfunktionen verfügbar. Beispielsweise implementiert das Schaltflächen-Steuerelement das Start Steuerungs Muster zur Unterstützung des Click-Ereignisses (in der Regel wird ein Steuerelement aufgerufen, indem Sie [auf die Schalt](/windows/desktop/WinAuto/uiauto-implementinginvoke) Fläche klicken, doppelklicken oder drücken, eine vordefinierte Tastenkombination oder eine andere Kombination von Tastatureingaben). Wenn eine Tastenkombination verwendet wird, um ein Steuerelement aufzurufen, sucht das XAML-Framework, ob das Steuerelement das Aufruf Steuerungs Muster implementiert, und aktiviert es, wenn dies der Fall ist.

Im folgenden Beispiel löst Control + S das Click-Ereignis aus, da die Schaltfläche das Aufruf Muster implementiert.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Wenn ein Element mehrere Steuerelement Muster implementiert, kann nur ein Element über eine Zugriffstaste aktiviert werden. Die Steuerelement Muster werden wie folgt priorisiert:
1.  Aufrufen (Schaltfläche)
2.  Umschalten (Kontrollkästchen)
3.  Auswahl (ListView)
4.  Erweitern/reduzieren (ComboBox) 

Wenn keine Entsprechung gefunden wird, ist die Zugriffstaste ungültig, und es wird eine Debugmeldung bereitgestellt ("Es wurden keine Automatisierungs Muster für diese Komponente gefunden. Implementieren Sie das gesamte gewünschte Verhalten im aufgerufenen Ereignis. Wenn die Einstellung in Ihrem Ereignishandler auf true festgelegt wird, wird diese Meldung unterdrückt. ")

## <a name="custom-keyboard-accelerator-behavior"></a>Benutzerdefiniertes Verhalten der Tastatur Beschleunigung

Das aufgerufene Ereignis des [keyboardaccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) -Objekts wird ausgelöst, wenn die Zugriffstaste ausgeführt wird. Das [keyboardacceleratorinvokeabventargs](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) -Ereignis Objekt enthält die folgenden Eigenschaften:

- [**Behandelt**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.handled) (boolesch): Wenn diese Einstellung auf "true" festgelegt wird, wird verhindert, dass das Ereignis das-Steuerelement Muster auslöst, und das Der Standardwert ist false.
- - [**Element**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.element) (DependencyObject): das der Zugriffstaste zugeordnete Objekt.
- [**Keyboardaccelerator**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.keyboardaccelerator): die Tastenkombination, mit der das aufgerufene Ereignis ausgelöst wird.

Hier veranschaulichen wir, wie Sie eine Auflistung von Tastatur Accelerators für Elemente in einem ListView-Element definieren und wie Sie das aufgerufene Ereignis für jede Zugriffstaste verarbeiten.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>
```

``` csharp
void SelectAllInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectAll();
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectionMode = ListViewSelectionMode.None;
  MyListView.SelectionMode = ListViewSelectionMode.Multiple;
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>Standardmäßiges Tastatur Verhalten überschreiben

Einige Steuerelemente unterstützen, wenn Sie den Fokus haben, integrierte Tastaturbeschleuniger, die beliebige App-definierte Zugriffstasten überschreiben. Wenn z. b. ein [Textfeld](/uwp/api/windows.ui.xaml.controls.textbox) den Fokus besitzt, kopiert der Steuerelement-und C-Accelerator nur den aktuell markierten Text (app-definierte Acceleratoren werden ignoriert, und es werden keine weiteren Funktionen ausgeführt).

Obwohl es nicht empfehlenswert ist, das Standardverhalten von Steuerelementen aufgrund von Benutzer Vertrautheit und-Erwartungen zu überschreiben, können Sie die integrierte Tastatur Beschleunigung eines Steuer Elements überschreiben. Im folgenden Beispiel wird gezeigt, wie Sie die Control + C-Tastatur Beschleunigung für ein [Textfeld](/uwp/api/windows.ui.xaml.controls.textbox) über den [PreviewKeyDown](/uwp/api/windows.ui.xaml.uielement.previewkeydown) -Ereignishandler überschreiben: 

``` csharp
 private void TextBlock_PreviewKeyDown(object sender, KeyRoutedEventArgs e)
 {
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(Windows.System.VirtualKey.Control);
    var isCtrlDown = ctrlState == CoreVirtualKeyStates.Down || ctrlState 
        ==  (CoreVirtualKeyStates.Down | CoreVirtualKeyStates.Locked);
    if (isCtrlDown && e.Key == Windows.System.VirtualKey.C)
    {
        // Your custom keyboard accelerator behavior.
        
        e.Handled = true;
    }
 }
```  

## <a name="disable-a-keyboard-accelerator"></a>Deaktivieren einer Zugriffstaste 

Wenn ein Steuerelement deaktiviert ist, wird auch die zugehörige Zugriffstaste deaktiviert. Da die isaktivierte Eigenschaft von ListView im folgenden Beispiel auf false festgelegt ist, kann das zugeordnete Steuerelement und eine Zugriffstaste nicht aufgerufen werden.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A"
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

Übergeordnete und untergeordnete Steuerelemente können dieselbe Zugriffstaste gemeinsam verwenden. In diesem Fall kann das übergeordnete Steuerelement auch aufgerufen werden, wenn das untergeordnete Steuerelement den Fokus besitzt und seine Zugriffstaste deaktiviert ist.

## <a name="screen-readers-and-keyboard-accelerators"></a>Bildschirm Sprachausgaben und Tastaturbeschleuniger 

Sprachausgaben wie die Sprachausgabe können Benutzern die Tastenkombination Tastenkombination ankündigen. Standardmäßig handelt es sich hierbei um jeden Modifizierer (in der Reihenfolge virtualmodifiers Enumeration), gefolgt von der Taste (und durch "+"-Zeichen getrennt). Sie können dies über die angefügte Eigenschaft " [AcceleratorKey](/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties" anpassen. Wenn mehr als eine Zugriffstaste angegeben ist, wird nur der erste angekündigt.

In diesem Beispiel gibt AutomationProperty. AcceleratorKey die Zeichenfolge "Control + Shift + A" zurück:

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> Durch Festlegen von "AutomationProperties. AcceleratorKey" wird keine Tastatur Funktionalität aktiviert, sondern nur das UIA-Framework angegeben, welche Schlüssel verwendet werden.

## <a name="common-keyboard-accelerators"></a>Allgemeine Tastaturbeschleuniger

Es wird empfohlen, Tastaturbeschleuniger in Windows-Anwendungen konsistent zu machen. Benutzer müssen sich Tastaturbeschleuniger merken und die gleichen (oder ähnliche) Ergebnisse erwarten.

Dies ist möglicherweise aufgrund von Unterschieden in der Funktionalität von apps nicht immer möglich.

| **Bearbeitung läuft** | **Allgemeine Tastatur Beschleunigung** |
| ------------- | ----------------------------------- |
| Bearbeitungsmodus starten | STRG + E |
| Alle Elemente in einem Steuerelement oder Fenster mit Fokus auswählen | STRG+A |
| Suchen und ersetzen | STRG + H |
| Rückgängig | STRG+Z |
| Wiederholen | STRG+Y |
| Auswahl löschen und in die Zwischenablage kopieren | STRG+X |
| Auswahl in die Zwischenablage kopieren | STRG + C, STRG + Einfügung |
| Inhalt der Zwischenablage einfügen | STRG + V, UMSCHALT + Einfügung |
| Inhalt der Zwischenablage einfügen (mit Optionen) | STRG+ALT+V |
| Ein Element umbenennen | F2 |
| Neues Element hinzufügen | STRG + N |
| Neues sekundäres Element hinzufügen | STRG + UMSCHALT + N |
| Ausgewähltes Element löschen (mit Rückgängigmachen) | ENTF, STRG + D |
| Ausgewähltes Element löschen (ohne Rückgängigmachen) | UMSCHALT + ENTF |
| Fett | STRG+B |
| Underline | STRG + U |
| Kursiv | STRG+I |

| **Navigation** | |
| ------------- | ----------------------------------- |
| Suchen von Inhalten in einem Steuerelement oder Fenster mit Fokus | STRG + F |
| Zum nächsten Suchergebnis wechseln | F3 |

| **Weitere Aktionen** | |
| ------------- | ----------------------------------- |
| Favoriten hinzufügen | STRG+D | 
| Aktualisieren | F5 oder STRG + R | 
| Vergrößern | STRG + + | 
| Verkleinern | STRG +- | 
| In Standardansicht Zoomen | STRG + 0 | 
| Speichern | STRG+ S | 
| Schließen | STRG + W | 
| Drucken | STRG+P | 

Beachten Sie, dass einige der Kombinationen für lokalisierte Versionen von Windows nicht gültig sind. Beispielsweise wird in der spanischen Version von Windows STRG + N als fett formatiert anstelle von STRG + B verwendet. Es wird empfohlen, lokalisierte Tastaturbeschleuniger bereitzustellen, wenn die APP lokalisiert wird.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Nutzbarkeit für die Benutzerfreundlichkeit von Tastatur Accelerators

### <a name="tooltips"></a>QuickInfos

Da Tastaturbeschleuniger in der Regel nicht direkt in der Benutzeroberfläche der Windows-Anwendung beschrieben werden, können Sie die Auffindbarkeit mithilfe von Quick Infos verbessern, die automatisch angezeigt [werden, wenn](../controls-and-patterns/tooltips.md)der Benutzer den Fokus auf ein Steuerelement verschiebt, drückt, hält oder mit dem Mauszeiger auf ein Steuerelement zeigt. Die QuickInfo kann ermitteln, ob ein Steuerelement über eine zugeordnete Tastatur Zugriffstaste verfügt. wenn dies der Fall ist, ist die Kombination der Tastenkombination.

**Windows 10, Version 1803 (Update April 2018) und neuer**

Beim Deklarieren von Tastatur Accelerators werden standardmäßig alle Steuerelemente (außer [menuflyoutitem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) und [togglemenuflyoutitem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) die entsprechenden Tastenkombinationen in einer QuickInfo angezeigt.

> [!NOTE] 
> Wenn für ein Steuerelement mehr als eine Zugriffstaste definiert ist, wird nur die erste angezeigt.

![QuickInfo für Zugriffstaste](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Kombinations Feld Tastenkombination in QuickInfo*

Für [Schalt](/uwp/api/windows.ui.xaml.controls.button)Flächen-, [appbarbutton](/uwp/api/windows.ui.xaml.controls.appbarbutton)-und [appbartogglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) -Objekte wird die Tastatur Beschleunigung an die Standard-QuickInfo des Steuer Elements angefügt. Für [menuflyoutitem](/uwp/api/windows.ui.xaml.controls.appbarbutton) -und " [degglemenuflyoutitem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) "-Objekte wird die Tastatur Beschleunigung mit dem Flyout-Text angezeigt.

> [!NOTE]
> Durch das Angeben einer QuickInfo (siehe Button1 im folgenden Beispiel) wird dieses Verhalten überschrieben.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![QuickInfo für Zugriffstaste](images/accelerators/accelerators-button-small.png)

*Tastenkombination-Kombinations Feld zur Standard QuickInfo der Schaltfläche angehängt*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![QuickInfo für Zugriffstaste](images/accelerators/accelerators-appbarbutton-small.png)

*Tastenkombination-Kombinations Feld an die Standard-QuickInfo von appbarbutton angehängt*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![QuickInfo für Zugriffstaste](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Kombinations Feld "Tastenkombination" an menuflyoutitem-Text angehängt*

Steuern Sie das Darstellungs Verhalten, indem Sie die [keyboardacceleratorplacementmode](/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) -Eigenschaft verwenden, die zwei Werte akzeptiert: " [Auto](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) " oder " [Hidden](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)".    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

In einigen Fällen müssen Sie möglicherweise eine QuickInfo in Relation zu einem anderen Element (in der Regel ein Container Objekt) darstellen. Beispielsweise ein Pivot-Steuerelement, das die QuickInfo für ein PivotItem mit dem Pivot-Header anzeigt. 

Hier wird gezeigt, wie die keyboardacceleratorplacementtarget-Eigenschaft verwendet wird, um die Tastenkombination für die Tastenkombination für eine Save-Schaltfläche mit dem Raster Container anstelle der Schaltfläche anzuzeigen.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>Bezeichnungen

In einigen Fällen empfiehlt es sich, die Bezeichnung eines Steuer Elements zu verwenden, um zu bestimmen, ob das Steuerelement über eine zugeordnete Tastatur Zugriffstaste verfügt. wenn dies der Fall ist, ist dies die Tastenkombination 

Einige Platt Form Steuerelemente führen dies standardmäßig aus, insbesondere das [menuflyoutitem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) [-Objekt und](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) das-Objekt für das Objekt ""-Objekt, während [appbarbutton](/uwp/api/windows.ui.xaml.controls.appbarbutton) und [appbartogglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) dies tun, wenn Sie im Überlauf Menü der [Befehlsleiste](/uwp/api/windows.ui.xaml.controls.commandbar)angezeigt werden.

![In einer Menü Element Bezeichnung beschriebene Tastaturbeschleuniger](images/accelerators/accelerators_menuitemlabel.png)  
*In einer Menü Element Bezeichnung beschriebene Tastaturbeschleuniger*

Sie können den standardacceleratortext für die Bezeichnung über die [keyboardacceleratortexdeverride](/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) -Eigenschaft der Steuerelemente [menuflyoutitem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [degglemenuflyoutitem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [appbarbutton](/uwp/api/windows.ui.xaml.controls.appbarbutton)und [appbartogglebutton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) überschreiben (verwenden Sie ein einzelnes Leerzeichen für keinen Text). 

> [!NOTE] 
> Der Überschreibungs Text wird nicht angezeigt, wenn das System keine angefügte Tastatur erkennen kann (Sie können dies über die [keyboardpresent](/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) -Eigenschaft überprüfen).

## <a name="advanced-concepts"></a>Weiterführende Konzepte

Hier überprüfen wir einige Aspekte von Tastatur Accelerators auf niedriger Ebene.

### <a name="when-an-accelerator-is-invoked"></a>Wenn eine Zugriffstaste aufgerufen wird

Accelerators bestehen aus zwei Arten von Schlüsseln: Modifizierern und nicht-Modifizierern. Modifizierertasten sind UMSCHALT, Menü, Steuerelement und die Windows-Taste, die über [virtualkeymodifiers](/uwp/api/Windows.System.VirtualKeyModifiers)verfügbar gemacht werden. Nicht-Modifizierer sind eine beliebige virtuelle Taste, z. b. Delete, F3, Leertaste, ESC und alle alphanumerischen Zeichen und Interpunktions Zeichen. Eine Tastenkombination wird aufgerufen, wenn der Benutzer eine nicht-Modifizierertaste drückt, während er eine oder mehrere Modifizierertasten gedrückt hält. Wenn der Benutzer z. b. STRG + UMSCHALT + M drückt, wenn der Benutzer M drückt, prüft das Framework die modifiziererer (STRG und UMSCHALT) und löst die Zugriffstaste aus, sofern vorhanden.

> [!NOTE]
> In der Entwurfs Ansicht wird die Zugriffstaste automatisch angezeigt (wenn der Benutzer z. b. STRG + UMSCHALT drückt und dann m gedrückt hält, wird die Zugriffstaste wiederholt aufgerufen, bis m freigegeben wird). Dieses Verhalten kann nicht geändert werden.

### <a name="input-event-priority"></a>Eingangs Ereignis Priorität
Eingabeereignisse treten in einer bestimmten Reihenfolge auf, die Sie basierend auf den Anforderungen Ihrer APP abfangen und behandeln können. 

#### <a name="the-keydownkeyup-bubbling-event"></a>Das KeyDown/KeyUp-bubblindereignis 

In XAML wird eine Tastatureingabe so verarbeitet, als wäre es nur eine eingabebubtapipeline. Diese Eingabe Pipeline wird von KeyDown/KeyUp-Ereignissen und Zeichen Eingaben verwendet. Wenn ein Element z. b. den Fokus besitzt und der Benutzer eine Taste gedrückt drückt, wird ein KeyDown-Ereignis für das Element ausgelöst, gefolgt vom übergeordneten Element des Elements usw. bis zum args. Behandelte Eigenschaft ist "true".

Das KeyDown-Ereignis wird auch von einigen Steuerelementen verwendet, um die integrierten Steuerelement Accelerators zu implementieren. Wenn ein Steuerelement über eine Zugriffstaste verfügt, wird das KeyDown-Ereignis behandelt, was bedeutet, dass kein KeyDown-Ereignis bubblinden vorhanden ist. Beispielsweise unterstützt das richeditbox Copy mit STRG + C. Wenn STRG gedrückt wird, wird das KeyDown-Ereignis ausgelöst und Blasen ausgelöst. wenn der Benutzer jedoch gleichzeitig C drückt, wird das KeyDown-Ereignis als behandelt markiert und nicht ausgelöst (es sei denn, der Parameter "shanddeventstoo" von " [UIElement. AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler) " ist auf "true" festgelegt).

#### <a name="the-characterreceived-event"></a>Das Ereignis "Merkmal empfangen"

Da das Ereignis " [Merkmal empfangen](/uwp/api/windows.ui.core.corewindow.CharacterReceived) " nach dem [KeyDown](/uwp/api/windows.ui.core.corewindow.KeyDown) -Ereignis für Text Steuerelemente wie z. b. "TextBox" ausgelöst wird, können Sie die Zeicheneingabe im KeyDown-Ereignishandler abbrechen.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Die PreviewKeyDown-und PreviewKeyUp-Ereignisse

Die Vorschau Eingabeereignisse werden vor allen anderen Ereignissen ausgelöst. Wenn Sie diese Ereignisse nicht behandeln, wird die Zugriffstaste für das Element, das den Fokus besitzt, ausgelöst, gefolgt vom KeyDown-Ereignis. Beide Ereignisse Blasen bis zur Behandlung.


![Key Event Sequence ](images/accelerators/accelerators_keyevents.png)
 ***Key-Ereignis Sequenz***

Reihenfolge der Ereignisse:

KeyDown-Ereignis Vorschau...
App Accelerator OnKeyDown-Methode KeyDown Event App Accelerators auf der übergeordneten OnKeyDown-Methode für das übergeordnete KeyDown-Ereignis auf dem übergeordneten Element (Blasen zum Stamm)...
Schlüssel für Ereignis präviewkeyup-Ereignisse keyupeer Vents

Wenn das Accelerator-Ereignis behandelt wird, wird das KeyDown-Ereignis ebenfalls als behandelt markiert. Das KeyUp-Ereignis bleibt unverändert.

### <a name="resolving-accelerators"></a>Auflösen von Accelerators

Eine Tastatur Beschleunigung-Ereignis Blase aus dem Element, das den Fokus auf den Stamm hat. Wenn das Ereignis nicht behandelt wird, sucht das XAML-Framework nach anderen nicht Bereichs bezogenen App-Accelerators außerhalb des bubblinpfades.

Wenn zwei Tastaturbeschleuniger mit der gleichen Tastenkombination definiert sind, wird die erste Zugriffstaste in der visuellen Struktur aufgerufen.

Bereichs bezogene Tastaturbeschleuniger werden nur aufgerufen, wenn sich der Fokus innerhalb eines bestimmten Bereichs befindet. Beispielsweise kann in einem Raster, das Dutzende von Steuerelementen enthält, eine Tastatur Beschleunigung nur für ein Steuerelement aufgerufen werden, wenn sich der Fokus innerhalb des Rasters befindet (der Bereichs Besitzer).

### <a name="scoping-accelerators-programmatically"></a>Programm gesteuertes Festlegen von Bereichs Zugriffstasten

Die [UIElement. tryinvokekeyboardaccelerator](/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) -Methode ruft alle übereinstimmenden Acceleratoren in der Teilstruktur des-Elements auf.

Die [UIElement. onprocesskeyboardaccelerators](/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) -Methode wird vor der Tastatur Beschleunigung ausgeführt. Diese Methode übergibt ein [processkeyboardacceleratorargs](/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) -Objekt, das den Schlüssel, den-Modifizierer und einen booleschen Wert enthält, der angibt, ob der Tastaturbeschleuniger behandelt wird. Wenn die Tastatur Beschleunigung als behandelt markiert ist, wird die Tastatur Beschleunigung aufgerufen (sodass die äußere Tastatur Beschleunigung nie aufgerufen wird).

> [!NOTE]
> Onprocesskeyboardaccelerators werden immer ausgelöst, unabhängig davon, ob Sie behandelt werden oder nicht (ähnlich wie das OnKeyDown-Ereignis). Sie müssen überprüfen, ob das Ereignis als behandelt markiert wurde.

In diesem Beispiel verwenden wir onprocesskeyboardaccelerators und tryinvokekeyboardaccelerator, um Tastaturbeschleuniger auf das Seiten Objekt zu beschränken:

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>Lokalisieren der Accelerators

Es wird empfohlen, alle Tastaturbeschleuniger zu lokalisieren. Dies ist mit der standardmäßigen UWP-Ressourcen Datei (. resw) und dem x:UID-Attribut in den XAML-Deklarationen möglich. In diesem Beispiel lädt der Windows-Runtime die Ressourcen automatisch.

![Lokalisierung der Tastatur Beschleunigung mit UWP-Ressourcen Datei ](images/accelerators/accelerators_localization.png)
 ***Tastaturbeschleuniger Lokalisierung mit UWP-Ressourcen Datei***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Programm gesteuertes Einrichten einer Zugriffstaste

Hier ist ein Beispiel für das programmgesteuerte Definieren einer Zugriffstaste:

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked += handler;
    this.KeyboardAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> "Keyboardaccelerator" ist nicht Share fähig, derselbe keyboardaccelerator kann nicht mehreren Elementen hinzugefügt werden.

### <a name="override-keyboard-accelerator-behavior"></a>Verhalten der Tastatur Beschleunigung überschreiben

Sie können das [keyboardaccelerator. aufgerufene](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) -Ereignis behandeln, um das Standardverhalten von keyboardaccelerator zu überschreiben.

In diesem Beispiel wird gezeigt, wie der Befehl "Alles auswählen" (STRG + eine Tastatur Beschleunigung) in einem benutzerdefinierten ListView-Steuerelement überschrieben wird. Wir legen auch die Eigenschaft behandelt auf true fest, um das Ereignis weiter zu verhindern.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Tastaturinteraktionen](keyboard-interactions.md)
- [Zugriffsschlüssel](access-keys.md)

### <a name="samples"></a>Beispiele

- [XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery)
