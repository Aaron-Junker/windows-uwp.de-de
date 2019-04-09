---
Description: Hier erfahren Sie, wie Zugriffstasten die Benutzerfreundlichkeit und Barrierefreiheit von UWP-Apps verbessern können.
title: Zugriffstasten
label: Keyboard accelerators
template: detail.hbs
keywords: Tastatur, Zugriffstaste, Zugriffstasten, Tastenkombinationen, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen, Gamepad, Fernbedienung
ms.date: 10/10/2017
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 9cc696746de837c4883ae4a9ee8ebcf42cb78b12
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57822885"
---
# <a name="keyboard-accelerators"></a>Zugriffstasten

![Surface Tastatur](images/accelerators/accelerators_hero2.png)

Zugriffstasten (oder Tastaturkürzel) sind Tastenkombinationen, die die Benutzerfreundlichkeit und Barrierefreiheit Ihrer Windows-Anwendungen verbessern, indem Sie eine intuitive Möglichkeit für Benutzer bereitstellen, häufige Aktionen oder Befehle aufzurufen, ohne die App-Benutzeroberfläche zu verwenden.

Weitere Details über die Navigation der Benutzeroberfläche einer Windows-Anwendung mit Tastenkombinationen finden Sie im Thema [Zugriffstasten](access-keys.md).

> [!NOTE]
> Eine Tastatur ist unentbehrlich für Benutzer mit bestimmten körperlichen Einschränkungen (siehe [Barrierefreiheit der Tastaturnavigation](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)) und auch ein wichtiges Tool für Benutzer, die effizienter mit einer App interagieren möchten.

## <a name="overview"></a>Übersicht

Zugriffstasten umfassen generell die Funktionstasten F1 bis F12 oder eine Kombination aus einer Standardtaste zusammen mit einer oder mehreren Zusatztasten (STRG, UMSCHALTTASTE).

> [!NOTE]
> Die UWP-Plattform-Steuerelemente verfügen über integrierte Zugriffstasten. ListView unterstützt beispielsweise STRG+A zum Auswählen aller Elemente der Liste und RichEditBox unterstützt STRG+TAB zum Einfügen eines Tabstopps in das Textfeld. Diese integrierten Zugriffstasten werden als **Steuerungsabkürzungen** bezeichnet und werden nur ausgeführt, wenn der Fokus auf einem Element oder einem seiner untergeordneten Elemente liegt. Von Ihnen definierte Zugriffstaste, die mithilfe der hier erläuterten Zugriffstasten APIs verwendet werden, werden als **App-Zugriffstasten** bezeichnet.

Zugriffstasten sind nicht für jede Aktion verfügbar, gehören allerdings häufig zu Befehlen, die in Menüs verfügbar gemacht werden (und sollten im Inhalt des Menüelements angegeben werden). Zugriffstasten können ebenfalls Aktionen zugeordnet werden, die nicht über entsprechende Menüelemente verfügen. Da der Benutzer von den Menüs der Anwendungen abhängt, um den verfügbaren Befehlssatz zu ermitteln und zu erfahren, sollten Sie die Ermittlung der Zugriffstasten so einfach wie möglich machen (die Verwendung von Beschriftungen oder festgelegten Mustern kann dabei hilfreich sein).

![Zugriffstasten, die in einer elementbezeichnung Menü beschrieben](images/accelerators/accelerators_menuitemlabel.png)  
*Zugriffstasten, die in einer elementbezeichnung Menü beschrieben*

## <a name="when-to-use-keyboard-accelerators"></a>Verwendung von Zugriffstasten

Es wird empfohlen, dass Sie Zugriffstasten überall in Ihrer UI angegeben, wo dies sinnvoll ist, und Zugriffstasten für alle benutzerdefinierten Steuerelemente unterstützen.

- Zugriffstasten machen Ihre app für Benutzer Körperbehinderte Menschen, die Benutzer, die nur einen Schlüssel zu einem Zeitpunkt drücken oder schwierigkeiten bei der Verwendung einer mouse.* * einschließlich zugänglicher

  Eine gut durchdachte Tastatur-UI ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u. U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie etwa Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen. Für diese Benutzer ist eine vollständige Befehlsabdeckung entscheidend.

- Zugriffstasten stellen die Ihrer app, besser verwendbar für erfahrene Benutzer, die über die Tastatur interagieren möchten.

  Erfahrene Benutzer haben oftmals eine starke Vorliebe für die Verwendung der Tastatur, da tastaturbasierte Befehle viel schneller eingegeben werden können. Zudem ist es dafür nicht erforderlich, die Hände von der Tastatur wegzubewegen. Für diese Benutzer sind Effizienz und Konsistenz entscheidend. Die Vollständigkeit hingegen ist nur für am häufigsten verwendeten Befehle wichtig.

## <a name="specify-a-keyboard-accelerator"></a>Angeben einer Zugriffstaste

Verwenden Sie die [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor)-APIs zum Erstellen von Zugriffstasten in UWP-Apps. Mit diesen APIs müssen Sie keine verschiedenen KeyDown-Ereignisse behandeln, um die gedrückte Tastenkombination zu entdecken, und Sie können die Zugriffstasten in ihren App-Ressourcen lokalisieren.

Es wird empfohlen, dass Sie Zugriffstasten für die am häufigsten verwendeten Aktionen in Ihrer App festlegen und sie mit den Bezeichnungen des Menüelements oder durch QuickInfos dokumentieren. In diesem Beispiel deklarieren wir Zugriffstasten nur für die Befehle „Umbenennen” und „Kopieren”.

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

![Beschrieben in einer QuickInfo eine Zugriffstaste](images/accelerators/accelerators_tooltip.png)  
***Beschrieben in einer QuickInfo eine Zugriffstaste***

Das [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)-Objekt verfügt über eine Sammlung an [Zugriffstasten](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator), d. h. [Zugriffstasten](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), über die Sie Ihre benutzerdefinierten Zugriffstasten-Objekte angeben und die Tastenkombinationen für die Zugriffstaste festlegen:

-   **[Schlüssel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)**  : die [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey) für die Zugriffstaste verwendet.

-   **[Modifizierer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)**  – die [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers) für die Zugriffstaste verwendet. Wird der Modifizierer nicht angegeben, ist der Standardwert „None“.

> [!NOTE]
> Es werden Zugriffstasten, die aus einer einzelnen Taste bestehen (A, Löschen, F2, Leertaste, Esc, Multimediataste) und Zugriffstasten mit mehreren Tasten (STRG+UMSCHALT+M) unterstützt. Virtuelle Tasten in Gamepad werden jedoch nicht unterstützt.

## <a name="scoped-accelerators"></a>Bereichsbezogene Zugriffstasten

Einige Zugriffstasten können nur in bestimmten Bereichen verwendet werden, während andere auf App-Ebene funktionieren.

Microsoft Outlook enthält beispielsweise die folgenden Zugriffstasten:
-   Die Tasten STRG+B, STRG+I und ESC funktionieren nur für den Bereich des Formulars „E-Mail senden”
-   Die Tasten STRG+1 und STRG+2 funktionieren auf App-Ebene

### <a name="context-menus"></a>Kontextmenüs

Aktionen im Kontextmenü beeinflussen nur bestimmte Bereiche oder Elemente wie z. B. ausgewählte Zeichen in einem Text-Editor oder Musiktitel in einer Wiedergabeliste. Aus diesem Grund wird empfohlen, den Umfang der Zugriffstasten für Elemente des Kontextmenüs auf das übergeordnete Element des Kontextmenüs festzulegen.

Verwenden Sie die [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner)-Eigenschaft, um den Bereich der Zugriffstaste anzugeben. Dieser Code veranschaulicht, wie Sie ein Kontextmenü auf ListView mit bereichsbezogenen Zugriffstasten implementieren:

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

Das ScopeOwner-Attribut des Elements MenuFlyoutItem.KeyboardAccelerators kennzeichnet die Zugriffstaste als bereichsbezogen, anstatt als global (der Standard ist null oder global). Weitere Details finden Sie im Abschnitt **Beseitigen von Zugriffstasten** weiter unten in diesem Thema.

## <a name="invoke-a-keyboard-accelerator"></a>Aufrufen einer Zugriffstaste 

Das [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator)-Objekt verwendet das [Steuerungsmuster der Benutzeroberflächenautomatisierung (UIA)](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-controlpatternsoverview), um Maßnahmen zu ergreifen, wenn eine Zugriffstaste aufgerufen wird.

Die UIA-Steuerungsmuster machen die häufig verwendeten Steuerelement-Funktionen verfügbar. Z. B. das Schaltflächen-Steuerelement implementiert die [Invoke](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinginvoke) -Steuerelementmuster unterstützen das Click-Ereignis (in der Regel ein Steuerelement wird aufgerufen, indem klicken, doppelklicken oder drücken Sie, eine der vordefinierten Tastenkombination oder ein anderes Kombination von Tastatureingaben). Wenn eine Zugriffstaste verwendet wird, um ein Steuerelement aufzurufen, sucht das XAML-Framework danach, ob das Steuerelement das Invoke-Steuerungsmuster implementiert und wenn dies der Fall ist, wird es aktiviert (es ist nicht erforderlich, das KeyboardAcceleratorInvoked-Ereignis durchzuführen).

Im folgenden Beispiel löst STRG+S das Click-Ereignis aus, da die Schaltfläche das Invoke-Muster implementiert.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Wenn ein Element mehrere Steuerungsmuster implementiert, kann nur ein einziges über die Zugriffstaste aktiviert werden. Die Prioritätsstufe der Steuerungsmuster ist wie folgt:
1.  Aufrufen (Taste)
2.  Ein-/Ausschalten (Kontrollkästchen)
3.  Auswahl (ListView)
4.  Erweitern/Reduzieren (ComboBox) 

Wenn keine Übereinstimmung identifiziert wird, wird die Zugriffstaste ungültig und es wird eine Debugmeldung angezeigt („Es wurden keine Automatisierungsmuster für diese Komponente gefunden. Implementieren Sie das gewünschte Verhalten im Invoke-Ereignis. Wenn Sie „Handled“ im Ereignishandler auf „true“ festlegen, wird diese Meldung unterdrückt.“)

## <a name="custom-keyboard-accelerator-behavior"></a>Benutzerdefiniertes Verhalten der Zugriffstasten

Das Invoke-Ereignis des [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator)-Objekts wird ausgelöst, wenn die Zugriffstaste ausgeführt wird. Das [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs)-Ereignisobjekt enthält die folgenden Eigenschaften:

- [**Behandelt** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.handled) (boolesch): Festlegung auf "true" wird verhindert, dass das Ereignis, das das Steuerelementmuster auslösen und beendet die Zugriffstaste Eventbubbling. Der Standardwert ist FALSE.
- [**Element** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.element) (DependencyObject): Das Objekt, mit der Zugriffstaste verknüpft ist.
- [**KeyboardAccelerator**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.keyboardaccelerator): Die Zugriffstaste verwendet, um das Invoked-Ereignis auszulösen.

Hier demonstriert, wie eine Auflistung von Zugriffstasten für Elemente in einer ListView definiert, und wie das Invoked-Ereignis für jede Zugriffstaste behandelt.

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

## <a name="override-default-keyboard-behavior"></a>Überschreiben des Standardverhaltens der Tastatur

In einigen Fällen müssen Sie möglicherweise das Standardverhalten von bestimmten Schlüsseln, z. B. die RÜCKTASTE oder die EINGABETASTE zu überschreiben. Beispiel: 

## <a name="disable-a-keyboard-accelerator"></a>Deaktivieren einer Zugriffstaste 

Wenn ein Steuerelement deaktiviert ist, wird die zugehörige Zugriffstaste ebenfalls deaktiviert. Im folgenden Beispiel ist die IsEnabled-Eigenschaft von ListView auf "false" festgelegt. Daher kann die zugehörige STRG+A-Zugriffstaste nicht aufgerufen werden.

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

Übergeordnete und untergeordnete Steuerelemente können die gleiche Zugriffstaste haben. In diesem Fall kann das übergeordnete Steuerelement auch aufgerufen werden, wenn das untergeordnete Element den Fokus hat und die Zugriffstaste deaktiviert ist.

## <a name="screen-readers-and-keyboard-accelerators"></a>Sprachausgaben und Zugriffstasten 

Bildschirmleseprogramme wie die die Sprachausgabe können dem Benutzer die Tastenkombination für die Zugriffstasten melden. Standardmäßig ist dies jeder Modifizierer (in der Reihenfolge der VirtualModifiers-Enumeration), gefolgt von der Taste (und durch das "+"-Zeichen getrennt). Sie können dies durch die [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties angefügten Eigenschaft anpassen. Wenn mehr als eine Zugriffstaste angegeben wird, wird nur die erste Taste gemeldet.

In diesem Beispiel gibt AutomationProperty.AcceleratorKey die Zeichenfolge "STRG + UMSCHALT+A" zurück:

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
> Das Festlegen der AutomationProperties.AcceleratorKey-Funktion aktiviert nicht die Tastaturfunktion, es gibt es nur der UIA-Framework an, welche Tasten verwendet werden.

## <a name="common-keyboard-accelerators"></a>Allgemeine Zugriffstasten

Es wird empfohlen, dass Sie Zugriffstasten für UWP-Apps konsistent sind. Benutzer müssen sich die Zugriffstasten merken und das gleiche (oder ähnliche) Ergebnis erwarten.

Aufgrund der unterschiedlichen Funktionen ist dies allerdings nicht immer in allen Apps möglich.

| **Bearbeiten** | **Allgemeine Zugriffstaste** |
| ------------- | ----------------------------------- |
| Den Bearbeitungsmodus beginnen | STRG+E |
| Alle Elemente in einem Steuerelement mit angezeigtem Fokus oder Fenster auswählen | STRG+A |
| Suchen und Ersetzen | STRG+H |
| Rückgängig machen | STRG+Z |
| Redo | STRG+Y |
| Die Auswahl löschen und in die Zwischenablage kopieren | STRG+X |
| Auswahl in die Zwischenablage kopieren | STRG+C, STRG+EINFG |
| Inhalte aus der Zwischenablage einfügen | STRG+V, UMSCHALT+EINFG |
| Den Inhalt der Zwischenablage einfügen (mit Optionen). | STRG+ALT+V |
| Element umzubenennen | F2 |
| Neues Element hinzufügen | STRG+N |
| Neues zweites Element hinzufügen | STRG+UMSCHALT+N |
| Ausgewähltes Element löschen (mit Rückgängigmachen) | ENTF, STRG+D |
| Ausgewähltes Element löschen (ohne Rückgängigmachen) | UMSCHALT+ENTF |
| Bold | STRG+B |
| Underline | STRG+U |
| Italic | STRG+I |

| **Navigation** | |
| ------------- | ----------------------------------- |
| Nach Inhalten in einem Steuerelement mit Fokus oder Fenster suchen | STRG+F |
| Das nächste Suchergebnis anzeigen | F3 |

| **Andere Aktionen** | |
| ------------- | ----------------------------------- |
| Favoriten hinzufügen | STRG+D | 
| Aktualisieren | F5 oder STRG+R | 
| Vergrößern | STRG++ | 
| Verkleinern | STRG+- | 
| Auf Standardansicht zoomen | STRG + 0 | 
| Speichern | STRG+S | 
| Schließen | STRG+W | 
| Print | STRG+P | 

Beachten Sie, dass einige der Kombinationen nicht für lokalisierte Versionen von Windows gelten. In der spanischen Version von Windows wird STRG+N anstelle von STRG+B zur Fettformatierung verwendet. Es wird empfohlen, lokalisierte Zugriffstasten bereitzustellen, wenn die App lokalisiert ist.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Nutzbarkeitsangebote für Zugriffstasten

### <a name="tooltips"></a>QuickInfos

Da Zugriffstasten in der Regel nicht direkt in der Benutzeroberfläche Ihrer UWP-Anwendung beschrieben sind, können Sie die Auffindbarkeit durch [QuickInfos](../controls-and-patterns/tooltips.md) verbessern, die automatisch angezeigt werden, wenn der Benutzer den Fokus auf ein Steuerelement setzt bzw. die Maus drückt und hält oder mit dem Mauszeiger darauf zeigt. Die QuickInfo kann erkennen, ob ein Steuerelement über eine zugeordnete Zugriffstaste verfügt und, wenn ja, welche Tastenkombination als Zugriffstaste verwendet wird.

**Windows 10, Version 1803 (April 2018 Update) und höher**

Standardmäßig wird bei Zugriffstasten deklariert sind, werden alle Steuerelemente (mit Ausnahme von [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) und [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) stellen Sie die entsprechenden Tastenkombination in einer QuickInfo.

> [!NOTE] 
> Wenn ein Steuerelement definiert (mehr als eine Accelerator) ist, wird nur die ersten angezeigt.

![QuickInfo für Zugriffstasten](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Accelerator-Tastenkombination in QuickInfo*

Für [Schaltfläche](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), und [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) Objekten, die Zugriffstaste an das Steuerelement des Standard-QuickInfo angefügt wird. Für [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) und [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) Objekte, die Zugriffstaste mit dem Flyout-Text angezeigt wird.

> [!NOTE]
> Angeben einer QuickInfo angezeigt wird (siehe "Button1" im folgenden Beispiel) dieses Verhalten überschrieben.

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

![QuickInfo für Zugriffstasten](images/accelerators/accelerators-button-small.png)

*Accelerator-Tastenkombination der Schaltfläche Standard-QuickInfo hinzugefügt*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![QuickInfo für Zugriffstasten](images/accelerators/accelerators-appbarbutton-small.png)

*Accelerator-Tastenkombination AppBarButtons Standard-QuickInfo hinzugefügt*

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

![QuickInfo für Zugriffstasten](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Accelerator-Tastenkombination, die an MenuFlyoutItems Text angefügt*

Die Präsentation-Verhalten steuern, indem Sie mit der [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) -Eigenschaft, die zwei Werte akzeptiert: [Automatische](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) oder [ausgeblendet](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

In einigen Fällen müssen Sie möglicherweise eine QuickInfo in Bezug zu einem anderen Element darstellen (in der Regel ein Containerobjekt). Beispielsweise ein Pivot-Steuerelement, das die QuickInfo für ein PivotItem mit dem Pivot-Header anzeigt. 

Im Folgenden ist gezeigt, wie Sie die Eigenschaft KeyboardAcceleratorPlacementTarget verwenden, um die Tastenkombination für die Zugriffstaste für eine Schaltfläche zum Speichern mit dem Grid-Container anstelle der Schaltfläche anzeigen.

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

### <a name="labels"></a>Beschriftungen

In einigen Fällen wird empfohlen, die Beschriftung eines Steuerelements zu verwenden, um zu identifizieren, ob das Steuerelement über eine zugeordnete Zugriffstaste verfügt und, wenn ja, welche Tastenkombination als Zugriffstaste verwendet wird. 

Bei einigen Plattformsteuerelementen geschieht dies standardmäßig, insbesondere bei den Objekten [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) und [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), während es bei [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) und [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) dazu kommt, wenn sie im Überlaufmenü der [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) angezeigt werden.

![Zugriffstasten, die in einer elementbezeichnung Menü beschrieben](images/accelerators/accelerators_menuitemlabel.png)  
*Zugriffstasten, die in einer elementbezeichnung Menü beschrieben*

Sie können den Standardtext für die Beschriftung der Zugriffstaste über die Eigenschaft [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) der Steuerelemente [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) und [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) überschreiben (verwenden Sie ein Leerzeichen, wenn kein Text vorhanden sein soll). 

> [!NOTE] 
> Der überschreibende Text wird nicht angezeigt, wenn das System keine angeschlossene Tastatur erkennen kann (Sie können dies selbst über die Eigenschaft [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) überprüfen).

## <a name="advanced-concepts"></a>Erweiterte Konzepte

In diesem Abschnitt erläutern wir einige einfache Aspekte der Zugriffstasten.

### <a name="when-an-accelerator-is-invoked"></a>Wenn eine Zugriffstaste aufgerufen wird

Zugriffstasten bestehen aus zwei Arten von Tasten: Modifizierer und Nicht-Modifizierer. Zu den Zusatztasten gehören die Umschalttaste, Menü, STRG und die Windows-Taste, die über [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/Windows.System.VirtualKeyModifiers) verfügbar gemacht werden. Nichtzusatztasten sind alle virtuellen Schlüssel wie z. B. Löschen, F3, die Leertaste, Esc und alle alphanumerischen und Satzzeichen-Tasten. Eine Zugriffstaste wird aufgerufen, wenn Benutzer eine andere Taste drücken, während sie eine oder mehrere Zusatztasten gedrückt halten. Wenn der Benutzer beispielsweise STRG+UMSCHALT+M drückt und dann M drückt, überprüft das Framework die Modifizierer (STRG und UMSCHALTTASTE) und die Zugriffstaste wird ausgelöst, wenn sie vorhanden ist.

> [!NOTE]
> Standardmäßig wird die Zugriffstaste automatisch wiederholt (z. B., wenn der Benutzer STRG+UMSCHALT drückt und dann M gedrückt hält, wird die Zugriffstaste wiederholt aufgerufen, bis M losgelassen wird). Dieses Verhalten kann nicht geändert werden.

### <a name="input-event-priority"></a>Priorität des Eingabeereignises
Eingabeereignisse treten in einer bestimmten Reihenfolge auf, die Sie je nach Anforderungen der App abfangen und behandeln können. 

#### <a name="the-keydownkeyup-bubbling-event"></a>Das KeyDown/KeyUp-Bubbling-Ereignis 

Im XAML wird ein Tastendruck so behandelt, als ob es nur eine Eingabe-Bubblingpipeline gibt. Diese Eingabepipeline wird vom KeyDown/KeyUp-Ereignis und der Zeicheneingabe verwendet. Wenn ein Element den Fokus hat, und der Benutzer eine NACH-UNTEN-TASTE drückt, wird ein KeyDown-Ereignis für das Element ausgelöst, gefolgt vom übergeordneten Element des Elements, und so weiter bis ganz nach oben in der Struktur, bis die args.Handle-Eigenschaft auf "true" gelangt.

Das KeyDown-Ereignis wird auch von einigen Steuerelementen verwendet, um die integrierten Steuerungszugriffstasten zu implementieren. Wenn eine Steuerung über eine Zugriffstaste verfügt, verarbeitet sie das KeyDown-Ereignis, das bedeutet, dass kein KeyDown-Eventbubbling auftritt. Beispielsweise unterstützt die RichEditBox das Kopieren mit STRG+C. Wenn STRG gedrückt wird, wird das KeyDown-Ereignis ausgelöst, und durchläuft ein Bubbling. Wenn der Benutzer allerdings gleichzeitig C drückt, wird das KeyDown-Ereignis als „Handled” gekennzeichnet und nicht mehr ausgelöst (es sei denn, der handledEventsToo-Parameter des [UIElement.AddHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) ist auf "true" festgelegt).

#### <a name="the-characterreceived-event"></a>CharacterReceived-Ereignis

Wenn das [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived)-Ereignis nach dem [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) -Ereignis für Textsteuerelemente wie z. B. TextBox ausgelöst wird, können Sie die Zeicheneingabe im KeyDown-Ereignishandler abbrechen.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Die PreviewKeyDown- und PreviewKeyUp-Ereignisse

Die Vorschaueingabeereignisse werden vor allen anderen Ereignissen ausgelöst. Wenn Sie diese Ereignisse nicht behandeln, wird die Zugriffstaste für das Element, das den Fokus hat ausgelöst, gefolgt vom KeyDown-Ereignis. Beide Ereignisse durchlaufen ein Bubbling, bis der Vorgang durchgeführt wird.


![Ereignis schlüsselsequenzspalte](images/accelerators/accelerators_keyevents.png)
***Schlüsselsequenzspalte-Ereignis***

Reihenfolge der Ereignisse:

KeyDown-Vorschauereignisse...
App-Zugriffstaste OnKeyDown-Methode KeyDown-Ereignis App-Zugriffstaste auf die übergeordnete OnKeyDown-Methode für das übergeordnete KeyDown-Ereignis für das übergeordnete Ereignis (Bubbling bis zum Stammverzeichnis)...
CharacterReceived-Ereignis PreviewKeyUp-Ereignis KeyUpEvents

Wenn das Zugriffstasten-Ereignis behandelt wird, wird das KeyDown-Ereignis ebenfalls als behandelt markiert. Das KeyUp-Ereignis wird nicht behandelt.

### <a name="resolving-accelerators"></a>Beseitigen von Zugriffstasten

Das Zugriffstasten-Ereignis durchläuft ein Bubblingereignis vom Element mit Fokus bis hin zum Stammverzeichnis. Wenn das Ereignis nicht behandelt wird, sucht das XAML-Framework unbegrenzte App-Zugriffstasten außerhalb des Bereichs der App außerhalb der Bubbling-Pfads.

Wenn zwei Zugriffstasten mit der gleichen Tastenkombination definiert sind, wird die erste Zugriffstaste der visuellen Struktur aufgerufen.

Bereichsbezogene Zugriffstasten werden nur aufgerufen, wenn sich der Fokus innerhalb eines bestimmten Bereichs befindet. In einem Raster, das zahlreiche Steuerelemente enthält, kann z. B. eine Zugriffstaste für ein Steuerelement nur dann aufgerufen werden, wenn sich der Fokus innerhalb des Rasters („Bereichsbesitzer“) befindet.

### <a name="scoping-accelerators-programmatically"></a>Eingrenzen programmgesteuerter Zugriffstasten

Die [UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator)-Methode ruft alle übereinstimmenden Zugriffstasten in die Unterstruktur des Elements auf.

Die [UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) -Methode wird vor der Zugriffstaste ausgeführt. Diese Methode übergibt ein [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs)-Objekt, das die Taste, den Modifizierer und einen booleschen Wert enthält, der angibt, ob die Zugriffstaste behandelt wird. Wenn dieser als behandelt markiert ist, durchläuft die Zugriffstaste ein Bubbling (wobei der externe Tastenkürzel nie aufgerufen wird).

> [!NOTE]
> OnProcessKeyboardAccelerators werden immer ausgelöst, ob behandelt oder nicht (ähnlich wie das OnKeyDown-Ereignis). Sie müssen überprüfen, ob das Ereignis als behandelt markiert wurde.

In diesem Beispiel verwenden wir OnProcessKeyboardAccelerators und TryInvokeKeyboardAccelerator, um den Bereich der Zugriffstasten auf das Seitenobjekt auszuwählen:

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

### <a name="localize-the-accelerators"></a>Lokalisieren der Zugriffstasten

Es wird empfohlen, alle Zugriffstasten zu lokalisieren. Sie können dies anhand der standardmäßigen UWP-Ressourcendatei (.resw) und des x: Uid-Attributs in Ihrer XAML-Deklarationen durchführen. In diesem Beispiel lädt Windows-Runtime automatisch die Ressourcen.

![Tastatur Accelerator Lokalisierung mit UWP-Ressourcendatei](images/accelerators/accelerators_localization.png)
***Accelerator Lokalisierung mit UWP-Ressourcendatei der Tastatur***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Einrichten einer programmgesteuerten Zugriffstaste

Hier ist ein Beispiel für das Definieren einer programmgesteuerten Zugriffstaste:

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
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator kann nicht freigegeben werden, der gleiche KeyboardAccelerator kann nicht mehreren Elementen hinzugefügt werden.

### <a name="override-keyboard-accelerator-behavior"></a>Außerkraftsetzen des Verhaltens von Zugriffstasten

Sie können das Ereignis [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) bearbeiten, um das KeyboardAccelerator-Standardverhalten außer Kraft zu setzen.

In diesem Beispiel wird veranschaulicht, wie Sie den Befehl „Alle auswählen“ (Zugriffstaste STRG+A) in einem benutzerdefinierten ListView-Steuerelement außer Kraft setzen. Wir haben außerdem die Handled-Eigenschaft auf „true“ festgelegt, um das weitere Bubbling des Ereignisses zu beenden.

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

- [Tastenkombinationen](keyboard-interactions.md)
- [Zugriffsschlüssel](access-keys.md)

### <a name="samples"></a>Proben

- [XAML-Steuerelementsammlungen](https://github.com/Microsoft/Xaml-Controls-Gallery)
