---
title: Fokusnavigation ohne Maus
description: Erfahren Sie, wie Sie mithilfe der Fokus Navigation umfassende und konsistente Interaktionen in Ihren Windows-apps und benutzerdefinierte Steuerelemente für Benutzer von Tastatur Anwendern, Benutzer mit Behinderungen und andere Barrierefreiheits Anforderungen sowie die 10-Fuß-Erfahrung von Fernsehbildschirmen und der Xbox One bereitstellen.
label: ''
template: detail.hbs
keywords: Tastatur, Spiele Controller, Remote Steuerung, Navigation, direktionale innere Navigation, direktionaler Bereich, Navigations Strategie, Eingabe, Benutzerinteraktion, Barrierefreiheit, Nutzbarkeit
ms.date: 09/24/2020
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3d7b77f627cd09c988c90b44167be7b5e452fe67
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032213"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Fokus Navigation für Tools der Tastatur, Gamepad, Remote Steuerung und Barrierefreiheit

![Tastatur, Remote und D-Pad](images/dpad-remote/dpad-remote-keyboard.png)

Verwenden Sie die Fokus Navigation, um umfassende und konsistente Interaktionen in Ihren Windows-apps und benutzerdefinierte Steuerelemente für Benutzer von Tastatur Anwendern, Benutzer mit Behinderungen und andere Barrierefreiheits Anforderungen sowie die 10-Fuß-Erfahrung von Fernsehbildschirmen und der Xbox One bereitzustellen.

## <a name="overview"></a>Übersicht

Die Fokus Navigation bezieht sich auf den zugrunde liegenden Mechanismus, mit dem Benutzer die Benutzeroberfläche einer Windows-Anwendung mithilfe einer Tastatur, eines Gamepad oder einer Remote Steuerung navigieren und damit interagieren können.

> [!NOTE]
> Eingabegeräte werden normalerweise als Zeigegeräte klassifiziert, wie z. b. Touch, Touchpad, Stift und Maus, und nicht zeigend Geräte, wie z. b. Tastatur, Gamepad und Remote Steuerung.

In diesem Thema wird beschrieben, wie Sie eine Windows-Anwendung optimieren und benutzerdefinierte Interaktionen für Benutzer erstellen, die auf nicht zeigenden Eingabetypen basieren. 

Obwohl wir uns auf die Tastatureingaben für benutzerdefinierte Steuerelemente in Windows-apps auf PCs konzentrieren, ist eine gut entworfene Tastatur Darstellung auch wichtig für Software-Tastaturen, wie z. b. die touchtastatur und die Bildschirmtastatur (OSK), unterstützt Barrierefreiheits Tools wie die Windows-Sprachausgabe und unterstützt die 10-Fuß-Benutzerfreundlichkeit.

Anleitungen zum Entwickeln benutzerdefinierter Benutzeroberflächen in Windows-Anwendungen für Zeigegeräte finden Sie unter [handle-Zeiger Eingabe](handle-pointer-input.md) .

Weitere allgemeine Informationen zum Entwickeln von apps und Erfahrungen für die Tastatur finden Sie unter [Tastatur Interaktion](keyboard-interactions.md).

## <a name="general-guidance"></a>Allgemeine Hinweise

Nur die Benutzeroberflächen Elemente, für die eine Benutzerinteraktion erforderlich ist, sollten die Fokus Navigation unterstützen, Elemente, die keine Aktion erfordern (z. b. statische Bilder), benötigen keinen Tastaturfokus. Sprachausgaben und ähnliche Barrierefreiheits Tools kündigen diese statischen Elemente weiterhin an, auch wenn Sie nicht in der Fokus Navigation enthalten sind. 

Beachten Sie, dass im Gegensatz zur Navigation mit einem Zeiger Gerät (z. b. Maus oder Fingerabdruck) die Fokus Navigation linear ist. Berücksichtigen Sie bei der Implementierung der Fokus Navigation, wie ein Benutzer mit Ihrer Anwendung interagiert und wie die logische Navigation lauten soll. In den meisten Fällen wird das benutzerdefinierte Fokus Navigationsverhalten empfohlen, das dem bevorzugten Lese Muster der Kultur des Benutzers folgt.

Einige weitere Aspekte der Fokus Navigation umfassen Folgendes:

- Werden Steuerelemente logisch gruppiert?
- Gibt es Gruppen von Steuerelementen mit größerer Wichtigkeit?
  - Wenn ja, sind diese Gruppen Untergruppen enthalten?
- Erfordert das Layout eine benutzerdefinierte direktionale Navigation (Pfeiltasten) und Aktivier Reihenfolge?

Das e-Book zur Entwicklung von [Software für Barrierefreiheit](https://www.microsoft.com/download/details.aspx?id=19262) hat ein hervorragendes Kapitel zum *Entwerfen der logischen Hierarchie* .

## <a name="2d-directional-navigation-for-keyboard"></a>2D-direktionale Navigation für Tastatur

Der 2D-innere Navigationsbereich eines Steuer Elements oder einer Steuerelement Gruppe wird als "direktionaler Bereich" bezeichnet. Wenn sich der Fokus auf dieses Objekt verschiebt, können die Tastatur Pfeiltasten (Links, rechts, oben und nach unten) verwendet werden, um zwischen untergeordneten Elementen innerhalb des direktionalen Bereichs zu navigieren.

![direktionaler ](images/keyboard/directional-area-small.png)
 *2D-Navigationsbereich oder direktionaler Bereich einer Steuerelement Gruppe*

Zum Verwalten der 2D-inneren Navigation mit den Tastatur Pfeiltasten können Sie die [xyfocekeyboardnavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) -Eigenschaft verwenden (die die möglichen Werte " [Auto](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)", " [aktiviert](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)" oder " [deaktiviert](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)" aufweist).

> [!NOTE]
> Die Aktivier Reihenfolge ist von dieser Eigenschaft nicht betroffen. Um eine verwirrende Navigation zu vermeiden, wird empfohlen, dass untergeordnete Elemente eines direktionalen Bereichs *nicht* explizit in der Navigations Reihenfolge der Registerkarten der Anwendung angegeben werden. Weitere Informationen zum Tabstopps-Verhalten für ein Element finden Sie unter den Eigenschaften [UIElement. tabfocronavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) .

### <a name="auto-default-behavior"></a>[Auto](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (Standardverhalten)

Wenn die Einstellung auf "Auto" festgelegt ist, wird das direktionale Navigationsverhalten durch die Herkunft oder Vererbungs Hierarchie des Elements bestimmt Wenn alle Vorgänger im Standardmodus sind (festgelegt auf **Auto** ), wird die direktionale Navigation mit der Tastatur *nicht* unterstützt.

### <a name="disabled"></a>[Disabled](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Legen Sie **xyfocquellkeyboardnavigation** auf **deaktiviert** fest, um die direktionale Navigation zum Steuerelement und dessen untergeordneten Elementen zu blockieren.

![Das deaktivierte ](images/keyboard/xyfocuskeyboardnav-disabled.gif)
 *XYFocusKeyboardNavigation disabled behavior* Verhalten von xyfocrokeyboardnavigation ist nicht verfügbar.

In diesem Beispiel ist für das primäre [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (containerprimary) der Wert für " **xyfocus KeyboardNavigation** " auf " **aktiviert** " festgelegt. Alle untergeordneten Elemente erben diese Einstellung und können mit den Pfeiltasten dorthin navigiert werden. Die Elemente "B3" und "B4" befinden sich jedoch in einem sekundären [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (containersecondary), wobei " **xyfocekeyboardnavigation** " auf " **deaktiviert** " festgelegt ist, wodurch der primäre Container überschrieben wird und die Pfeiltasten Navigation in sich selbst und zwischen den untergeordneten Elementen

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

### <a name="enabled"></a>[Enabled](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Legen Sie für **xyfocquellkeyboardnavigation** den Wert **aktiviert** fest, um die 2D-direktionale Navigation zu einem Steuerelement und den einzelnen untergeordneten [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) -Objekten

Wenn festgelegt, ist die Navigation mit den Pfeiltasten auf Elemente innerhalb des direktionalen Bereichs beschränkt. Die Registerkarten Navigation ist nicht betroffen, da alle Steuerelemente über die Hierarchie der Aktivier Reihenfolge zugänglich sind

![Xyfocrokeyboardnavigation-fähiges Verhalten ( ](images/keyboard/xyfocuskeyboardnav-enabled.gif)
 *xyfocrokeyboardnavigation* )

In diesem Beispiel ist für das primäre [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (containerprimary) der Wert für " **xyfocus KeyboardNavigation** " auf " **aktiviert** " festgelegt. Alle untergeordneten Elemente erben diese Einstellung und können mit den Pfeiltasten dorthin navigiert werden. Die Elemente "B3" und "B4" befinden sich in einem sekundären [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (containersecondary), in dem " **xyfocus KeyboardNavigation** " nicht festgelegt ist, was dann die primäre Container Einstellung erbt. Das Element "B5" befindet sich nicht in einem deklarierten direktionalen Bereich und unterstützt keine Pfeiltasten Navigation, unterstützt jedoch das Standardverhalten der Registerkarten Navigation.

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

Sie können über mehrere Ebenen von ngerichteten direktionalen Bereichen verfügen. Wenn für alle übergeordneten Elemente der Wert von xyfocingkeyboardnavigation auf Aktiviert festgelegt ist, werden die Grenzen des inneren Navigationsbereichs ignoriert.

Im folgenden finden Sie ein Beispiel für zwei geschachtelte direktionale Bereiche innerhalb eines Elements, das nicht explizit 2D-direktionale Navigation unterstützt. In diesem Fall wird die direktionale Navigation zwischen den beiden unterstützten Bereichen nicht unterstützt.

!["Xyfocingkeyboardnavigation" aktiviert und das netsted-Verhalten " ](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)
 *xyfocrokeyboardnavigation" aktiviert*

Im folgenden finden Sie ein komplexeres Beispiel für drei eingefügte Richtungs Bereiche, in denen:

-   Wenn B1 den Fokus besitzt, kann nur B5 zu (und umgekehrt) navigiert werden, weil eine direktionale Bereichs Grenze vorhanden ist, in der "xyfocrokeyboardnavigation" auf "deaktiviert" festgelegt ist, sodass B2, B3 und B4 mit den Pfeiltasten nicht erreichbar sind.
-   Wenn B2 den Fokus besitzt, kann nur B3 zu (und umgekehrt) navigiert werden, da die direktionale Bereichsbegrenzung die Pfeiltasten Navigation zu B1, B4 und B5 verhindert.
-   Wenn B4 den Fokus besitzt, muss die Tab-Taste zum Navigieren zwischen Steuerelementen verwendet werden.

!["Xyfocrokeyboardnavigation" aktiviert und komplexes Verhalten in der Liste](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*"Xyfocrokeyboardnavigation" aktiviert und komplexes Verhalten in der Liste*

## <a name="tab-navigation"></a>Tab-Navigation

Während die Pfeiltasten für die 2D-direktionale Navigations-witin eines Steuer Elements oder einer Steuerelement Gruppe verwendet werden können, kann die Tab-Taste verwendet werden, um zwischen allen Steuerelementen in einer Windows-Anwendung zu navigieren. 

Alle interaktiven Steuerelemente unterstützen standardmäßig die Navigation über Tab-Taste ( [isaktivierte](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) und [istabstopp](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) -Eigenschaft sind **true** ), wobei die logische Aktivier Reihenfolge aus dem Steuerelement Layout in der Anwendung abgeleitet ist Die Standardreihenfolge ist jedoch nicht notwendigerweise mit der visuellen Reihenfolge identisch. Die tatsächliche Anzeigeposition kann vom übergeordneten Layoutcontainer und bestimmten Eigenschaften abhängen, die Sie für die untergeordneten Elemente festlegen können, um das Layout zu beeinflussen.

Vermeiden Sie eine benutzerdefinierte Aktivier Reihenfolge, mit der der Fokus in der Anwendung angezeigt wird. Eine Liste der Steuerelemente in einem Formular sollte z. b. eine Aktivier Reihenfolge aufweisen, die von oben nach unten und von links nach rechts (abhängig vom Gebiets Schema) verläuft.

In diesem Abschnitt wird beschrieben, wie diese Aktivier Reihenfolge vollständig an Ihre APP angepasst werden kann.

### <a name="set-the-tab-navigation-behavior"></a>Festlegen des Navigations Verhaltens der Registerkarte

Die [tabfocronavigation](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) -Eigenschaft von [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) gibt das Navigationsverhalten der Registerkarte für die gesamte Objektstruktur (oder den direktionalen Bereich) an.

> [!NOTE]
> Verwenden Sie diese Eigenschaft anstelle der [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) -Eigenschaft für Objekte, die keine [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) verwenden, um ihre Darstellung zu definieren.

Wie bereits im vorherigen Abschnitt erwähnt, wird empfohlen, dass untergeordnete Elemente eines direktionalen Bereichs *nicht* explizit in der Navigations Reihenfolge der Registerkarten der Anwendung angegeben werden, um eine verwirrende Navigation zu vermeiden. Weitere Informationen zum Tabstopps-Verhalten für ein Element finden Sie in den Eigenschaften [UIElement. tabfocronavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) .   
> Für ältere Versionen als Windows 10 Creators Update (Build 10.0.15063) waren die Registerkarten Einstellungen auf [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) -Objekte beschränkt. Weitere Informationen finden Sie unter [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[Tabfocenavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) hat einen Wert vom Typ [KeyboardNavigationMode](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) mit den folgenden möglichen Werten (Beachten Sie, dass es sich bei diesen Beispielen nicht um benutzerdefinierte Steuerelement Gruppen handelt und keine innere Navigation mit den Pfeiltasten erforderlich ist):

- **Local** (Standard)   
  Registerkarten Indizes werden in der lokalen Unterstruktur innerhalb des Containers erkannt. In diesem Beispiel ist die Aktivier Reihenfolge B1, B2, B3, B4, B5, B6, B7, B1.

   ![Navigationsverhalten der Registerkarte "local"](images/keyboard/tabnav-local.gif)

   *Navigationsverhalten der Registerkarte "local"*

- **Einmal**  
  Der Container und alle untergeordneten Elemente erhalten einmal den Fokus. In diesem Beispiel ist die Aktivier Reihenfolge B1, B2, B7, B1 (innere Navigation mit Pfeiltaste wird ebenfalls demonstriert).

   ![Navigationsverhalten der Registerkarte "Once"](images/keyboard/tabnav-once.gif)

   *Navigationsverhalten der Registerkarte "Once"*

- **Zyklus**   
  Der Fokus wird auf das anfängliche Fokus verwendbare Element innerhalb eines Containers zurückgerückt. In diesem Beispiel ist die Aktivier Reihenfolge B1, B2, B3, B4, B5, B6, B2...

   ![Navigationsverhalten der Registerkarte "Cycle"](images/keyboard/tabnav-cycle.gif)

   *Navigationsverhalten der Registerkarte "Cycle"*

Hier ist der Code für die vorangehenden Beispiele (mit tabfocus Navigation = "Cycle").

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

### <a name="tabindex"></a>[TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Verwenden Sie [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) , um die Reihenfolge anzugeben, in der Elemente den Fokus erhalten, wenn der Benutzer mithilfe der Tab-Taste durch Steuerelemente navigiert. Ein Steuerelement mit einem niedrigeren Registerkarten Index erhält den Fokus vor einem Steuerelement mit einem höheren Index.

Wenn für ein Steuerelement kein [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) angegeben ist, wird ihm ein höherer Indexwert zugewiesen als der aktuelle höchste Indexwert (und die niedrigste Priorität) aller interaktiven Steuerelemente in der visuellen Struktur, basierend auf dem Gültigkeitsbereich. 

Alle untergeordneten Elemente eines-Steuer Elements werden als Gültigkeitsbereich betrachtet, und wenn eines dieser Elemente auch untergeordnete Elemente aufweist, werden Sie als ein anderer Bereich betrachtet. Alle Mehrdeutigkeiten werden behoben, indem das erste Element in der visuellen Struktur des Bereichs ausgewählt wird. 

Um ein Steuerelement aus der Aktivier Reihenfolge auszuschließen, legen Sie die Eigenschaft [istabstopp](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) auf **false** fest.

Überschreiben Sie die Standard Aktivier Reihenfolge durch Festlegen der [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) -Eigenschaft.

> [!NOTE] 
> [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) funktioniert auf dieselbe Weise mit [UIElement. tabfocennavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) und [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Hier zeigen wir, wie die [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) -Eigenschaft für bestimmte Elemente die Fokus Navigation beeinflussen kann. 

![Lokale Registerkarten Navigation mit TabIndex-Verhalten](images/keyboard/tabnav-tabindex.gif)

*Lokale Registerkarten Navigation mit TabIndex-Verhalten*

Im vorherigen Beispiel gibt es zwei Bereiche: 
- B1, direktionaler Bereich (B2-B6) und B7
- direktionaler Bereich (B2-B6)

Wenn B3 (im direktionalen Bereich) den Fokus erhält, wird der Bereich geändert und die Registerkarten Navigation in den direktionalen Bereich übertragen, in dem der beste Kandidat für den nachfolgenden Fokus identifiziert wird In diesem Fall ist B2 gefolgt von B4, B5 und B6. Der Bereich wird erneut geändert, und der Fokus wechselt auf B1.

Hier ist der Code für dieses Beispiel.

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

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>2D-direktionale Navigation für Tastatur, Gamepad und Remote Steuerung

Nicht-Zeiger-Eingabetypen wie Tastatur, Gamepad, Remote Steuerung und Barrierefreiheits Tools wie die Windows-Sprachausgabe haben einen gemeinsamen, zugrunde liegenden Mechanismus für die Navigation und Interaktion mit der Benutzeroberfläche Ihrer Windows-Anwendung.

In diesem Abschnitt erfahren Sie, wie Sie eine bevorzugte Navigations Strategie angeben und die Fokus Navigation innerhalb Ihrer Anwendung mithilfe eines Satzes von Eigenschaften der Navigations Strategie optimieren, die alle Fokus basierten, nicht Zeiger-Eingabetypen unterstützen.

Weitere allgemeine Informationen zum Entwickeln von apps und Erfahrungen für Xbox/TV finden Sie unter [Tastatur Interaktion](keyboard-interactions.md), [Entwerfen für Xbox und TV](../devices/designing-for-tv.md)und [Gamepad-und Remote Steuerungs Interaktionen](gamepad-and-remote-interactions.md).

### <a name="navigation-strategies"></a>Navigations Strategien

> Navigations Strategien sind auf Tastatur, Gamepad, Remote Steuerung und verschiedene Barrierefreiheits Tools anwendbar.

Mithilfe der folgenden Eigenschaften der Navigations Strategie können Sie beeinflussen, welches Steuerelement den Fokus auf der Grundlage der Pfeiltaste, der direktionalen Pad-Schaltfläche (D-Pad) oder einer ähnlichen Taste erhält 

-   Xyfocusupnavigationstrategy
-   Xyfocus downnavigationstrategy
-   Xyfocleleftnavigationstrategy
-   Xyfocus rightnavigationstrategy

Diese Eigenschaften haben die möglichen Werte [Auto](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (Standard), [navigationdirectiondistance](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)oder [rectilineardistance ](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Wenn der Wert auf " **Auto** " festgelegt ist, basiert das Verhalten des-Elements auf den übergeordneten Elementen des-Elements. Wenn alle-Elemente auf **Auto** festgelegt sind, wird die **Projektion** verwendet.

> [!NOTE]
> Andere Faktoren, z. b. das zuvor fokussierte Element oder die Nähe der Achse der Navigationsrichtung, können das Ergebnis beeinflussen.

### <a name="projection"></a>Projektion

Die Projektions Strategie verschiebt den Fokus auf das erste Element, das gefunden wird, wenn der Rand des aktuell ausgerichteten Elements in die Navigationsrichtung *projiziert* wird.

In diesem Beispiel ist jede Fokus Navigationsrichtung auf Projektion festgelegt. Beachten Sie, wie sich der Fokus von B1 auf B4 verlagert, wobei B3 umgangen wird. Dies liegt daran, dass sich B3 nicht in der Projektions Zone befindet. Beachten Sie auch, dass ein Fokus Kandidat nicht identifiziert wird, wenn er von B1 nach links verschoben wird. Dies liegt daran, dass die Position von B2 in Bezug auf B1 die B3 als Kandidat ausschließt. Wenn sich B3 in derselben Zeile wie B2 befand, wäre dies ein möglicher Kandidat für den linken Navigationsbereich. B2 ist ein funktionierender Kandidat, da seine Nähe zur Achse der Navigationsrichtung nicht versperrt ist.

![Projektions Navigations Strategie](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Projektions Navigations Strategie*

### <a name="navigationdirectiondistance"></a>Navigationdirectiondistance

Die navigationdirectiondistance-Strategie verschiebt den Fokus auf das Element, das der Achse der Navigationsrichtung am nächsten ist.

Der Rand des umgebenden rects, das der Navigationsrichtung entspricht, wird *erweitert* und *projiziert* , dass Kandidaten Ziele identifiziert werden. Das erste gefundene Element wird als Ziel identifiziert. Bei mehreren Kandidaten wird das nächstgelegene Element als Ziel identifiziert. Wenn noch mehrere Kandidaten vorhanden sind, wird das oberste/linke Element als Kandidat identifiziert.

![Navigations Strategie "navigationdirectiondistance"](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Navigations Strategie "navigationdirectiondistance"*

### <a name="rectilineardistance"></a>Rectilineardistance

Die rectilineardistance-Strategie verschiebt den Fokus auf das nächste Element, das auf einem 2D-multilinearen Abstand ([Taxicab-Geometrie](https://en.wikipedia.org/wiki/Taxicab_geometry)) basiert.

Die Summe aus der primären Distanz und der sekundären Entfernung zu den einzelnen potenziellen Kandidaten wird verwendet, um den besten Kandidaten zu identifizieren. In einem gleichwertigen Element wird das erste Element Links ausgewählt, wenn die angeforderte Richtung nach oben oder unten ist, und das erste Element oben wird ausgewählt, wenn die angeforderte Richtung links oder rechts ist.

![Navigations Strategie "rectilineardistance"](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Navigations Strategie "rectilineardistance"*

Diese Abbildung zeigt, wie die Richtung "B3", wenn "B1" den Fokus hat und die angeforderte Richtung ist. Dies basiert auf den folgenden Berechnungen für dieses Beispiel:
-   Distance (B1, B3, nach unten) ist 10 + 0 = 10
-   Distance (B1, B2, Down) ist 0 + 40 = 30
-   Distance (B1, D, Down) ist 30 + 0 = 30


## <a name="related-articles"></a>Verwandte Artikel
- [Programmgesteuerte Fokusnavigation](focus-navigation-programmatic.md)
- [Tastaturinteraktionen](keyboard-interactions.md)
- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)
