---
title: Fokus Navigation ohne Maus
Description: Erfahren Sie, wie Sie mithilfe der Fokus Navigation umfassende und konsistente Interaktionen in ihren UWP-apps und benutzerdefinierten Steuerelementen für Benutzer von Tastatur Anwendern, Benutzer mit Behinderungen und andere Barrierefreiheits Anforderungen sowie die 10-Fuß-Erfahrung von Fernsehbildschirme und die Xbox One.
label: ''
template: detail.hbs
keywords: Tastatur, Gamecontroller, Fernbedienung, Navigation, direktionale interne Navigation, direktionaler Bereich, Navigationsstrategie, Eingabe, Benutzerinteraktion, Bedienungshilfen, Verwendbarkeit
ms.date: 03/02/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cb49ed58f03710d44a5c06c15530936885b9beea
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735095"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Fokusnavigation für Tastatur, Gamepad, Fernbedienung und Bedienungshilfen

![Tastatur, Fernbedienung und Steuerkreuz](images/dpad-remote/dpad-remote-keyboard.png)

Verwenden Sie die Fokusnavigation, um umfassende und einheitliche Interaktionserfahrungen in Ihren UWP-Apps, benutzerdefinierte Steuerelemente für erfahrene Tastaturbenutzer und Personen mit Einschränkungen und anderen Anforderungen an die Barrierefreiheit sowie die 3-Meter-Erfahrung von Fernsehbildschirmen und der Xbox One bereitzustellen.

## <a name="overview"></a>Übersicht

Fokusnavigation bezieht sich auf den zugrunde liegende Mechanismus, mit dem Benutzer mithilfe einer Tastatur, eines Gamepads oder einer Fernbedienung durch die Benutzeroberfläche einer UWP-Anwendung navigieren und mit dieser interagieren können.

> [!NOTE]
> Eingabegeräte werden in der Regel als Zeigegeräte, z. B. Toucheingabe, Touchpad, Stift und Maus, und Nicht-Zeigegeräte wie Tastatur, Gamepad und Fernbedienung klassifiziert.

In diesem Thema wird beschrieben, wie Sie eine UWP-Anwendung optimieren und benutzerdefinierte Interaktionserfahrungen für Benutzer aufbauen, die nicht zeigenden Eingabetypen verwenden. 

Auch wenn wir uns auf Tastatureingaben für benutzerdefinierte Steuerelemente in UWP-Apps auf PCs konzentrieren, ist eine gut durchdachte Tastaturerfahrung ebenso wichtig für Softwaretastaturen wie die Touch- und Bildschirmtastatur, die Eingabehilfen wie die Windows-Sprachausgabe und die 3-Meter-Erfahrung unterstützen.

Anweisungen zum Entwickeln von benutzerdefinierten Erfahrungen in UWP-Anwendungen für Zeigegeräte finden Sie unter [Behandeln von Zeigereingaben](handle-pointer-input.md).

Allgemeinere Informationen zum Erstellen von Apps und Erfahrungen für die Tastatur finden Sie unter [Tastaturinteraktion](keyboard-interactions.md).

## <a name="general-guidance"></a>Allgemeiner Leitfaden

Nur die UI-Elemente, die eine Benutzerinteraktion erfordern, sollten die Fokusnavigation unterstützen. Elemente, für die keine Aktion erforderlich ist, z. B. statische Bilder, benötigen keinen Tastaturfokus. Sprachausgaben und ähnliche Bedienungshilfen geben diese statischen Elemente dennoch bekannt, auch wenn sie nicht in der Fokusnavigation enthalten sind. 

Es ist wichtig, zu beachten, dass im Gegensatz zur Navigation mit einem Zeigegerät wie einer Maus oder einer Toucheingabe die Fokusnavigation linear ist. Bei der Implementierung der Fokusnavigation sollten Sie überlegen, wie ein Benutzer mit Ihrer Anwendung interagiert und wie die logische Navigation aussehen sollte. In den meisten Fällen ist es empfehlenswert, das Verhalten der benutzerdefinierten Fokusnavigation an das bevorzugte Lesemuster der Kultur des Benutzers anzupassen.

Folgende Überlegungen zur Fokusnavigation müssen ebenfalls berücksichtigt werden:

- Sind Steuerelemente logisch gruppiert?
- Gibt es Gruppen von Steuerelementen mit größerer Bedeutung?
  - Falls ja, enthalten diese Gruppen Untergruppen?
- Sind für das Layout eine benutzerdefinierte direktionale Navigation (Pfeiltasten) und eine Aktivierreihenfolge erforderlich?

Das E-Book [Entwickeln von barrierefreier Software](https://www.microsoft.com/download/details.aspx?id=19262) enthält ein hervorragendes Kapitel zum Thema *Entwerfen der logischen Hierarchie*.

## <a name="2d-directional-navigation-for-keyboard"></a>Direktionale 2D-Navigation für die Tastatur

Der interne 2D-Navigationsbereich eines Steuerelements oder einer Steuerelementgruppe wird als „direktionaler Bereich“ bezeichnet. Wenn der Fokus auf dieses Objekt verschoben wird, können die Pfeiltasten der Tastatur (links, rechts, oben und unten) verwendet werden, um zwischen untergeordneten Elementen im direktionalen Bereich zu navigieren.

![direktionaler Bereich](images/keyboard/directional-area-small.png)
*2D-inneren Navigationsbereich oder direktionaler Bereich einer Steuerelement Gruppe*

Die können die Eigenschaft [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (mit den möglichen Werten [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) oder [Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) verwenden, um die interne 2D-Navigation mit den Pfeiltasten der Tastatur zu verwalten.

> [!NOTE]
> Die Aktivierreihenfolge ist von dieser Eigenschaft nicht betroffen. Um eine unübersichtliche Navigationserfahrung zu vermeiden, sollten untergeordnete Elemente eines direktionalen Bereichs *nicht* ausdrücklich in der Aktivierreihenfolge der Navigation Ihrer Anwendung angegeben werden. Weitere Informationen zum Verhalten der Aktivierreihenfolge für ein Element finden Sie unter den Eigenschaften [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex).

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (Standardverhalten)

Bei Festlegung auf Auto wird das Verhalten der direktionalen Navigation von der Herkunft oder Vererbungshierarchie eines Elements bestimmt. Wenn sich alle Vorgänger im Standardmodus (Festlegung auf **Auto**) befinden, wird die direktionale Navigation mit der Tastatur *nicht* unterstützt.

### <a name="disabledhttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode"></a>[Deaktiviert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Legen Sie **XYFocusKeyboardNavigation** auf **Disabled** fest, um die direktionale Navigation für das Steuerelement und seine untergeordneten Elemente zu blockieren.

![das deaktivierte Verhalten von xyfocrokeyboardnavigation](images/keyboard/xyfocuskeyboardnav-disabled.gif)
" *xyfocrokeyboardnavigation" deaktiviert* .

In diesem Beispiel ist beim primären [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) für **XYFocusKeyboardNavigation** der Wert **Enabled** festgelegt. Alle untergeordneten Elemente erben diese Einstellung, und Sie können mit den Pfeiltasten zu diesen navigieren. Die Elemente B3 und B4 befinden sich jedoch in einem sekundären [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary), für den **XYFocusKeyboardNavigation** auf **Disabled** festgelegt wurde, wodurch der primäre Container überschrieben und die Pfeiltastennavigation zu sich selbst und zwischen den untergeordneten Elementen deaktiviert wird.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### <a name="enabledhttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode"></a>[Aktiviert](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Legen Sie **XYFocusKeyboardNavigation** auf **Enabled** fest, um die direktionale 2D-Navigation zu einem Steuerelement und jedem seiner untergeordneten [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)-Objekte zu unterstützen.

Bei Festlegung ist die Navigation mit den Pfeiltasten auf Elemente innerhalb des direktionalen Bereichs beschränkt. Die TAB-Navigation ist nicht betroffen, da alle Steuerelemente über eine Hierarchie für die Aktivierreihenfolge zugänglich bleiben.

!["xyfocrokeyboardnavigation"-fähiges Verhalten](images/keyboard/xyfocuskeyboardnav-enabled.gif)
*xyfocrokeyboardnavigation-fähiges Verhalten*

In diesem Beispiel ist beim primären [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) für **XYFocusKeyboardNavigation** der Wert **Enabled** festgelegt. Alle untergeordneten Elemente erben diese Einstellung, und Sie können mit den Pfeiltasten zu diesen navigieren. Die Elemente B3 und B4 befinden sich in einem sekundären [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary), in dem **XYFocusKeyboardNavigation** nicht festgelegt ist und dann die Einstellung des primären Containers erbt. Das Element B5 befindet sich nicht in einem deklarierten direktionalen Bereich und bietet keine Unterstützung für die Pfeiltastennavigation, unterstützt jedoch das Standardverhalten für die TAB-Navigation.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

Sie können auf mehreren Ebenen verschachtelte direktionale Bereiche verwenden. Wenn für alle übergeordneten Elemente XYFocusKeyboardNavigation auf Aktiviert gesetzt ist, werden die Regionsgrenzen bei der internen Navigation ignoriert.

Im Folgenden sehen Sie ein Beispiel für zwei geschachtelte direktionale Bereiche innerhalb eines Elements, das die direktionale 2D-Navigation nicht ausdrücklich unterstützt. In diesem Fall wird direktionale Navigation zwischen den zwei geschachtelten Bereichen nicht unterstützt.

!["xyfocingkeyboardnavigation" und "netsted Behavior"](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)
" *xyfocrokeyboardnavigation" aktiviert und "Verhalten* "

Nachfolgend sehen Sie ein komplexeres Beispiel für drei verschachtelte direktionale Bereiche, wobei Folgendes gilt:

-   Wenn der Fokus auf B1 gesetzt ist, können Sie nur zu B5 navigieren (und umgekehrt), da es eine direktionale Bereichsgrenze gibt, in der XYFocusKeyboardNavigation auf Disabled festgelegt ist, die es unmöglich macht, B2, B3 und B4 mit den Pfeiltasten zu erreichen.
-   Wenn der Fokus auf B2 gesetzt ist, können Sie nur zu B3 navigieren (und umgekehrt), da die direktionale Bereichsgrenze die Navigation mit den Pfeiltasten zu B1, B4 und B5 verhindert.
-   Wenn der Fokus auf B4 gesetzt ist, muss die TAB-Taste verwendet werden, um zwischen Steuerelementen zu navigieren.

![Verhalten bei aktivierter und komplex verschachtelter XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*"Xyfocrokeyboardnavigation" aktiviert und komplexes Verhalten in der Liste*

## <a name="tab-navigation"></a>TAB-Navigation

Während die Pfeiltasten für die direktionale 2D-Navigation innerhalb eines Steuerelements oder einer Steuerelementgruppe verwendet werden können, können Sie mit der TAB-Taste zwischen allen Steuerelementen in einer UWP-Anwendung navigieren. 

Alle interaktiven Steuerelemente unterstützen standardmäßig die Navigation mit der TAB-Taste (Eigenschaften [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) und [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) sind ist auf **true** festgelegt), wobei die logische TAB-Reihenfolge aus dem Steuerelementlayout in der Anwendung abgeleitet wird. Die Standardreihenfolge ist jedoch nicht notwendigerweise mit der visuellen Reihenfolge identisch. Die tatsächliche Anzeigeposition kann vom übergeordneten Layoutcontainer und bestimmten Eigenschaften abhängen, die Sie für die untergeordneten Elemente festlegen können, um das Layout zu beeinflussen.

Vermeiden Sie eine benutzerdefinierte Aktivierreihenfolge, durch die der Fokus in Ihrer Anwendung wechselt. Beispielsweise sollte eine Liste mit Steuerelementen in einem Formular eine Aktivierreihenfolge aufweisen, die von oben nach unten und von links nach rechts (je nach Gebietsschema) verläuft.

In diesem Abschnitt wird beschrieben, wie diese Aktivierreihenfolge vollständig an Ihre App angepasst werden kann.

### <a name="set-the-tab-navigation-behavior"></a>Festlegen des Verhaltens der TAB-Navigation

Die Eigenschaft [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) von [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) bestimmt das Verhalten der TAB-Navigation für die gesamte zugehörige Objektstruktur (bzw. den direktionalen Bereich).

> [!NOTE]
> Verwenden Sie diese Eigenschaft anstelle der Eigenschaft [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) für Objekte, die Ihr Erscheinungsbild nicht über eine [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) definieren.

Wie im vorherigen Abschnitt erwähnt, sollten untergeordnete Elemente eines direktionalen Bereichs *nicht* ausdrücklich in der Aktivierreihenfolge der Navigation Ihrer Anwendung angegeben werden, um eine unübersichtliche Navigationserfahrung zu vermeiden. Weitere Informationen zum Verhalten der Aktivierreihenfolge für ein Element finden Sie unter den Eigenschaften [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex).   
> Für Versionen vor Windows 10 Creators Update (Build 10.0.15063) waren TAB-Einstellungen auf [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate)-Objekte beschränkt. Weitere Informationen finden Sie unter [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) hat einen Wert vom Typ [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) mit den folgenden möglichen Werten (beachten Sie, dass diese Beispiele keine benutzerdefinierten Steuerelementgruppen sind und keine interne Navigation mit den Pfeiltasten erfordern):

- **Local** (Standard)   
  Tabindizes werden für die lokale Unterstruktur innerhalb des Containers erkannt. In diesem Beispiel ist die Aktivierreihenfolge B1, B2, B3, B4, B5, B6, B7, B1.

   ![Verhalten bei der TAB-Navigation mit der Option „Local“](images/keyboard/tabnav-local.gif)

   *Navigationsverhalten der Registerkarte "local"*

- **Einmal**  
  Der Fokus wird einmal auf den Container und alle untergeordneten Elemente gesetzt. In diesem Beispiel ist die Aktivierreihenfolge B1, B2, B7, B1 (die interne Navigation mit Pfeiltasten wird ebenfalls gezeigt).

   ![Verhalten bei der TAB-Navigation mit der Option „Once“](images/keyboard/tabnav-once.gif)

   *Navigationsverhalten der Registerkarte "Once"*

- Schleifen   
  Der Fokus wird wieder auf das erste fokussierbare Element in einem Container gesetzt. In diesem Beispiel ist die Aktivierreihenfolge B1, B2, B3, B4, B5, B6, B2...

   ![Verhalten bei der TAB-Navigation mit der Option „Cycle“](images/keyboard/tabnav-cycle.gif)

   *Navigationsverhalten der Registerkarte "Cycle"*

Im Folgenden sehen Sie den Code für die vorherigen Beispiele (mit TabFocusNavigation = Cycle).

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### <a name="tabindexhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolscontrolwindows_ui_xaml_controls_control_tabindex"></a>[TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Verwenden Sie [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex), um die Reihenfolge anzugeben, in der der Fokus auf Elemente gesetzt wird, wenn der Benutzer mithilfe der TAB-Taste durch Steuerelemente navigiert. Auf ein Steuerelement mit einem niedrigeren Tabindex wird der Fokus vor einem Steuerelement mit einem höheren Index gesetzt.

Wenn für ein Steuerelement kein [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) angegeben ist, wird ihm ein Indexwert zugewiesen, der über dem aktuell höchsten Indexwert (und der niedrigsten Priorität) aller interaktiven Steuerelemente in der visuellen Struktur basierend auf dem Bereich liegt. 

Alle untergeordneten Elemente eines Steuerelements gelten als ein Bereich; wenn eins dieser Elemente über weitere untergeordnete Elemente verfügt, werden diese als ein anderer Bereich betrachtet. Alle Unklarheiten werden beseitigt, indem das erste Element in der visuellen Struktur des Bereichs ausgewählt wird. 

Um ein Steuerelement aus der Aktivierreihenfolge auszuschließen, legen Sie die Eigenschaft [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) auf **false** fest.

Überschreiben Sie die Standardaktivierreihenfolge, indem Sie die Eigenschaft [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) festlegen.

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) funktioniert mit [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) auf dieselbe Weise.


Im Folgenden sehen Sie, wie die Fokusnavigation von der Eigenschaft [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) für bestimmte Elemente beeinflusst wird. 

![Verhalten bei der TAB-Navigation mit der Option „Local“ und TabIndex](images/keyboard/tabnav-tabindex.gif)

*Lokale Registerkarten Navigation mit TabIndex-Verhalten*

Im vorherigen Beispiel gibt es zwei Bereiche: 
- B1, direktionaler Bereich (B2–B6) und B7
- direktionaler Bereich (B2–B6)

Wenn der Fokus auf B3 (im direktionalen Bereich) gesetzt wird, ändert sich der Bereich und die TAB-Navigation überträgt an den direktionalen Bereich, in dem der beste Kandidat für einen nachfolgenden Fokus identifiziert wird. In diesem Fall heißt das B2, gefolgt von B4, B5 und B6. Der Bereich ändert sich dann erneut, und der Fokus verschiebt sich auf B1.

Hier sehen Sie den Code für dieses Beispiel.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>Direktionale 2D-Navigation für Tastatur, Gamepad und Fernbedienung

Nicht zeigerorientierte Eingabetypen wie Tastatur, Gamepad, Fernbedienung und Bedienungshilfen wie die Windows-Sprachausgabe teilen Sie einen gemeinsamen zugrunde liegende Mechanismus für die Navigation und Interaktion mit der Benutzeroberfläche Ihrer UWP-Anwendung.

In diesem Abschnitt wird erläutert, wie Sie eine bevorzugte Navigationsstrategie angeben und die Fokusnavigation in Ihrer Anwendung durch eine Reihe von Navigationsstrategieeigenschaften optimieren, die alle fokusbasierten, nicht zeigerorientierten Eingabetypen unterstützen.

Weitere allgemeine Informationen zum Entwickeln von apps und Erfahrungen für Xbox/TV finden Sie unter [Tastatur Interaktion](keyboard-interactions.md), [Entwerfen für Xbox und TV](../devices/designing-for-tv.md)und [Gamepad-und Remote Steuerungs Interaktionen](gamepad-and-remote-interactions.md).

### <a name="navigation-strategies"></a>Navigationsstrategien

> Navigationsstrategien gelten für Tastatur, Gamepad, Fernbedienung und verschiedene Bedienungshilfen.

Mit den folgenden Navigationsstrategieeigenschaften können Sie beeinflussen, auf welches Steuerelement der Fokus gesetzt wird, wenn eine Pfeiltaste, eine Steuerkreuztaste oder ähnliches gedrückt wird. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Diese Eigenschaften können die Werte [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (Standard), [NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) oder [RectilinearDistance ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)aufweisen.

Wenn **Auto** festgelegt wird, basiert das Verhalten des Elements auf den Vorgängern des Elements. Wenn alle Elemente auf **Auto** festgelegt werden, wird **Projection** verwendet.

> [!NOTE]
> Andere Faktoren, wie das zuvor fokussierte Element oder die Nähe zur Achse der Navigationsrichtung, können das Ergebnis beeinflussen.

### <a name="projection"></a>Projection

Die Projection-Strategie verlagert den Fokus auf das erste Element, das beim *Projizieren* des Rands des aktuell fokussierten Elements in der Navigationsrichtung aufgefunden wird.

In diesem Beispiel ist jede Fokusnavigationsrichtung auf Projection festgelegt. Beachten Sie, wie sich der Fokus von B1 bis B4 nach unten verschiebt, wobei B3 umgangen wird. Das liegt daran, dass sich B3 nicht in der Projektionszone befindet. Beachten Sie außerdem, wie ein Fokuskandidat beim Verschieben nach links von B1 nicht identifiziert wird. Das liegt daran, dass die relative Position von B2 zu B1 B3 als Kandidat eliminiert. Wenn sich B3 in derselben Zeile wie B2 befinden würde, wäre es ein geeigneter Kandidat für die linke Navigation. B2 ist ein geeigneter Kandidat aufgrund der uneingeschränkten Nähe zur Achse der Navigationsrichtung.

![Projection-Navigationsstrategie](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Projektions Navigations Strategie*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

Die Distanz der Navigationsrichtungsstrategie verlagert den Fokus auf das Element, das sich am nächsten zur Achse der Navigationsrichtung befindet.

Der Rand des umgebenden Rechtecks, das der Richtung der Navigation entspricht, wird *erweitert* und *projiziert*, um Kandidatenziele zu identifizieren. Das erste gefundene Element wird als Ziel identifiziert. Wenn mehrere Kandidaten vorhanden sind, wird das nächste Element als Ziel identifiziert. Sind weiterhin mehrere Kandidaten vorhanden, wird das oberste/ganz linke Element als der Kandidat identifiziert.

![NavigationDirectionDistance-Navigationsstrategie](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Navigations Strategie "navigationdirectiondistance"*

### <a name="rectilineardistance"></a>RectilinearDistance

Die RectilinearDistance-Strategie verlagert den Fokus zum nächsten Element basierend auf dem gradlinigen 2D-Abstand ([Manhattan-Metrik](https://en.wikipedia.org/wiki/Taxicab_geometry)).

Der beste Kandidat wird durch Hinzufügen des primären und sekundären Abstands zu jedem möglichen Kandidaten identifiziert. Bei einem Gleichstand wird das erste Element links ausgewählt, wenn die angeforderte Richtung oben oder unten ist, oder das erste Element oben ausgewählt, wenn die angeforderte Richtung links oder rechts ist.

![RectilinearDistance-Navigationsstrategie](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Navigations Strategie "rectilineardistance"*

Diese Abbildung zeigt, dass B3 der RectilinearDistance-Fokuskandidat ist, wenn der Fokus auf B1 gesetzt ist und eine Richtung nach unten angefordert wird. Das basiert auf den folgenden Berechnungen für dieses Beispiel:
-   Abstand (B1, B3, nach unten) = 10 + 0 = 10
-   Abstand (B1, B2, nach unten) = 0 + 40 = 30
-   Abstand (B1, D, nach unten) = 30 + 0 = 30


## <a name="related-articles"></a>Verwandte Artikel
- [Programmgesteuerte Fokus Navigation](focus-navigation-programmatic.md)
- [Tastatur Interaktionen](keyboard-interactions.md)
- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md) 



