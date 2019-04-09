---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249502"
---
# <a name="teaching-tip"></a>Lehre Tipp

Ein Tipp Lehre ist ein teilweise dauerhaft ist und umfangreicher Flyout, das Kontextinformationen bereitstellt. Es wird häufig verwendet, für die darüber informiert, daran erinnert und spezialisiert, Benutzer über wichtige und neue Features, die benutzerfreundlichkeit verbessern können.

**Wichtig-APIs:** [TeachingTip-Klasse](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

Ein Tipp Lehre möglicherweise Light-dismiss oder explizite erfordern zu schließen. Ein Tipp Lehre kann auf ein bestimmtes UI-Element, mit dessen Ende abzielen und auch ohne Ende oder Ziel verwendet werden.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement? 

Verwenden einer **TeachingTip** Steuerelement, um die Aufmerksamkeit eines Benutzers auf neue oder wichtigen Updates und Features konzentrieren eine Erinnerung von nicht benötigten Optionen, die würde ihre verbessern oder unterrichten eines Benutzers wie eine Aufgabe abgeschlossen werden soll. 

Da Lehre Tipp vorübergehend ist, wäre es nicht das empfohlene Steuerelement für die Eingabeaufforderung von Benutzern zu Fehlern oder wichtige statusänderungen.


## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TeachingTip">öffnen Sie die app, und finden Sie unter den TeachingTip in Aktion</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Ein Tipp Lehre kann mehrere Konfigurationen, einschließlich der folgenden namhafte haben.

Ein Tipp Lehre kann ein bestimmtes UI-Element mit der Randbereich um kontextbezogene Informationen, die klareren, die sie darstellen, wird als Ziel. 

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-targeted.png)

Bei der angezeigten Informationen für ein bestimmtes Element der Benutzeroberfläche nicht relevant, kann ein Trinkgeld nontargeted Lehre erstellt werden, durch das Entfernen des Protokollfragments.

![Eine Beispiel-app mit einer Lehre Spitze in der unteren rechten Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-non-targeted.png)

Ein Tipp Lehre kann erfordern, dass der Benutzer sie über ein "X" die Schaltfläche in einem oberen Ecke oder "Schließen" im unteren Bereich zu schließen. Ein Tipp Lehre kann auch werden Light-dismiss aktiviert es in diesem Fall ist keine Schaltfläche "Schließen" und wird stattdessen der Unterricht-Tipp geschlossen, wenn ein Benutzer einen Bildlauf durchführt oder mit anderen Elementen der Anwendung interagiert. Aufgrund dieses Verhaltens Light-dismiss Tipps sind die beste Lösung, wenn ein Trinkgeld in einem bildlauffähigen Bereich platziert werden muss. 

![Eine Beispiel-app mit einer Light-dismiss Lehre-Tipp in der unteren rechten Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf."](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Erstellen Sie einen Tipp Lehre

Hier ist der XAML für eine gezielte Lehre QuickInfo-Steuerelement, das das standardmäßige Aussehen der TeachingTip mit einem Titel und Untertitel veranschaulicht. Beachten Sie, dass der Tipp Lehre an einer beliebigen Stelle in der Elementstruktur oder Code-behind angezeigt werden kann. In diesem Beispiel befindet sich in einem ResourceDictionary.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

Dies ist das Ergebnis auf, wenn die Seite, enthält die Schaltfläche und unterrichten QuickInfo angezeigt wird:

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Nicht ausgerichtete Tipps

Nicht alle Tipps beziehen sich auf ein Element auf dem Bildschirm. Für diese Szenarios legen Sie nicht die Zieleigenschaft, und zeigt der Unterricht Tipp stattdessen relativ zu den Rändern des XAML-Stamm. Allerdings kann ein Trinkgeld Lehre des Protokollfragments, die Beibehaltung der Platzierung relativ zu einem Element der Benutzeroberfläche durch Festlegen der TailVisibility-Eigenschaft auf "Collapsed" entfernt haben. Das folgende Beispiel ist eine nicht ausgerichtete Lehre-Tipps.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Beachten Sie, dass in diesem Beispiel die TeachingTip in der Elementstruktur statt in einem ResourceDictionary oder in Code-behind ist. Dies hat keine Auswirkungen auf; die TeachingTip nur angezeigt, wenn geöffnet, und keine Layouts verfügbaren Platz beansprucht.

![Eine Beispiel-app mit einer Lehre Spitze in der unteren rechten Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Bevorzugte Platzierung

Lehre Tipp repliziert des Flyout [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) erzielte Platzierungsverhalten mit der TeachingTipPlacementMode-Eigenschaft. Der Standardmodus für die Platzierung versucht, einen gezielten Lehre Tipp oben sein Ziel und eine nicht ausgerichtete Lehre Tipp zentriert am unteren Rand der Xaml-Stamm zu platzieren. Als mit Flyout aus, wird Wenn der der bevorzugte Platzierungsmodus nicht über die Platz für die Lehre-QuickInfo angezeigt würde eine andere Platzierungsmodus automatisch ausgewählt werden. 

Anwendungen, die Gamepad-Eingaben vorhergesagt, finden Sie unter [Gamepad und Remotesteuerung Interaktionen]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Es wird empfohlen, um Gamepad Zugriff auf jede Lehre Tip mit allen möglichen Konfigurationen der Benutzeroberfläche einer app zu testen.

Ein Trinkgeld gezielte Lehre, mit dessen PreferredPlacement "BottomLeft" festgelegt wird mit dem Ende, zentriert am unteren Rand sein Ziel verschoben werden, an der linken Seite im Unterricht Tipp-Text angezeigt.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-app mit einer Schaltfläche "Speichern", die von einem Tipp Lehre unterhalb der linken Ecke vorgesehen ist. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-targeted-preferred-placement.png)


Eine nicht ausgerichtete Lehre-QuickInfo mit seiner PreferredPlacement, legen Sie auf "BottomLeft" wird in der unteren linken Ecke der Stamm-Xaml angezeigt.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Eine Beispiel-app mit einer Lehre Spitze in der unteren linken Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-non-targeted-preferred-placement.png)

Das folgende Diagramm zeigt das Ergebnis der alle 13 PreferredPlacement-Modi, die für Ziel unterrichten Tipps festgelegt werden kann.
![Drei Objekte mit der Bezeichnung "target" Ziel lehrt Tipps, die zum Anzeigen der Lehre die entsprechenden bevorzugten Platzierung methodentippmodi darum verwendet. In der Mitte das erste Ziel zentriert ist ein Ziel Lehre-Tipp, die mit der Bezeichnung "Center" an das Ziel mit der Erweiterung nach unten zeigt. Über das erste Ziel zentriert ist ein Ziel Lehre-Tipp, die mit der Bezeichnung "Top" nach unten zeigt auf das Ziel mit der Erweiterung. Zentriert, rechts neben dem ersten Ziel ist ein Ziel Lehre-Tipp, die mit der Bezeichnung "Right" Links auf das Ziel mit der Erweiterung verweist. Unter dem ersten Ziel zentriert ist ein Ziel Lehre-Tipp, die mit der Bezeichnung "Bottom" nach oben an das Ziel mit der Erweiterung. Zentriert links neben dem ersten Ziel ist ein Ziel Lehre-Tipp, die mit der Bezeichnung "Linken" verweist direkt an das Ziel mit der Erweiterung. Auf der linken Seite das zweite Ziel ist ein Tipp Unterricht mit der Bezeichnung "LeftTop", die momentan im Ziel verweist, und verfügt über Text nach oben verschoben. Über das zweite Ziel ist ein Tipp Unterricht mit der Bezeichnung "TopLeft", die Sie am Ziel verweist, und verfügt über Text links verschoben. Rechts neben der zweite ist Ziel eines Lehre Tipps, die mit der Bezeichnung "RightBottom" nach links zeigt sich das Ziel und verfügt über Text nach unten verschoben. Unter dem das zweite Ziel ist ein Tipp Unterricht mit der Bezeichnung "BottomRight", die am Ziel nach oben zeigt und verfügt über Text rightward verschoben. Über den dritten Ziel ist ein Tipp Unterricht mit der Bezeichnung "Oberen", die Sie am Ziel verweist, und verfügt über Text rightward verschoben. Rechts neben der dritte ist Ziel eines Lehre Tipps, die mit der Bezeichnung "RightTop" nach links zeigt sich das Ziel und verfügt über Text nach oben verschoben. Unter dem dritten Ziel ist ein Tipp Unterricht mit der Bezeichnung "BottomLeft", die am Ziel nach oben zeigt und verfügt über Text links verschoben. Auf der linken Seite der dritten Ziel ist ein Tipp Unterricht mit der Bezeichnung "LeftBottom", die momentan im Ziel verweist, und verfügt über Text nach unten verschoben.](../images/teaching-tip-targeted-preferred-placement-modes.png)

Das folgende Diagramm zeigt das Ergebnis der alle 13 PreferredPlacement-Modi, die für nicht ausgerichtete Lehre Tipps festgelegt werden kann.
![Ein app-Fenster mit neun nicht ausgerichtete Lehre Tipps Lehre des bevorzugten Platzierung nicht ausgerichtete methodentippmodi veranschaulicht. Die Unterricht Spitze in der oberen linken Ecke der app hat die Bezeichnung "TopLeft oder LeftTop." Der Unterricht Tipp zentriert am oberen Rand der app wird mit der Bezeichnung "Top". Die Unterricht Spitze in der oberen rechten Ecke der app hat die Bezeichnung "TopRight oder RightTop." Der Unterricht Tipp zentriert am linken Rand der app wird mit der Bezeichnung "Left". Der Unterricht Tipp zentriert werden, in der Mitte der app wird mit der Bezeichnung "Center".
Der Unterricht Tipp zentriert am rechten Rand der app wird mit der Bezeichnung "Right". Die Unterricht Spitze in der unteren linken Ecke der app hat die Bezeichnung "BottomLeft oder LeftBottom." Der Unterricht Tipp zentriert am unteren Rand von der app wird mit der Bezeichnung "Unten." Die Unterricht Spitze in der unteren rechten Ecke der app hat die Bezeichnung "BottomRight oder RightBottom."](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Fügen Sie einen Rand Platzierung  

Sie können steuern, wie weit ein Trinkgeld gezielte Lehre abgesehen von sein Ziel festgelegt ist und wie weit ein Trinkgeld nicht ausgerichtete Lehre abgesehen von den Rändern des XAML-Stamm festgelegt ist, mithilfe der PlacementMargin-Eigenschaft. Wie [Rand](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin verfügt über vier Werte – left, right, top "und" nach unten – nur die relevanten Werte werden verwendet. Beispielsweise gilt PlacementMargin.Left auf, wenn die Spitze des Ziels oder am linken Rand des XAML-Stamms bleibt.

Das folgende Beispiel zeigt einem nicht ausgerichtete Tipp mit dem PlacementMargins linken/oben/rechts/unten jeweils auf 80 festgelegt.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![Eine Beispiel-app mit einer Lehre Spitze positioniert in Richtung, aber nicht vollständig mit der rechten unteren Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Fügen Sie Inhalt hinzu

Inhalt kann einen Lehre Tipp mit dem Content-Eigenschaft hinzugefügt werden. Liegt mehr Inhalt angezeigt wird, als was die Größe eines Tipps unterrichtet werden kann, wird automatisch eine Bildlaufleiste aktiviert werden, damit der Benutzer den Content-Bereich einen Bildlauf durchführen kann. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Im Inhaltsbereich des Unterrichts Tipps ist ein Kontrollkästchen mit der Bezeichnung "Nicht mehr Tipps beim Start anzeigen", und darunter ist Text, der liest "Sie können Ihre QuickInfo-Einstellungen in den Einstellungen ändern, wenn Sie Ihre Meinung ändern", "Einstellungen" einen Link zu der app Seite "Einstellungen" ist. Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Hinzufügen von Schaltflächen

Standardmäßig wird eine Standard "X" Schaltfläche "Schließen" neben dem Titel eines Lehre-Tipps angezeigt. Die Schaltfläche "Schließen" kann mit der Eigenschaft CloseButtonContent angepasst werden, in dem Fall die Schaltfläche am unteren Rand der Lehre Tipp verschoben wird.

**Hinweis: Keine Schaltfläche "Schließen" erscheint auf Light-dismiss aktiviert Tipps**

Eine Schaltfläche für benutzerdefinierte Aktionen kann durch Festlegen von ActionButtonContent-Eigenschaft (und optional die ActionButtonCommand und die ActionButtonCommandParameter-Eigenschaften) hinzugefügt werden.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Im Inhaltsbereich des Unterrichts Tipps ist ein Kontrollkästchen mit der Bezeichnung "Nicht mehr Tipps beim Start anzeigen", und darunter ist Text, der liest "Sie können Ihre QuickInfo-Einstellungen in den Einstellungen ändern, wenn Sie Ihre Meinung ändern", "Einstellungen" einen Link zu der app Seite "Einstellungen" ist. Am unteren Rand der Lehre befinden sich zwei Schaltflächen, eine graue auf der linken Seite, die liest "Deaktivieren" und eine blaue auf der rechten Seite, die liest "bitteschön!"](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero-Inhalt

Edge auf Edge-Inhalte kann auf einen Tipp Lehre hinzugefügt werden, durch Festlegen der HeroContent-Eigenschaft. Der Speicherort des Hero-Inhalts kann, oben oder unten einen Tipp Lehre festgelegt werden, durch Festlegen der HeroContentPlacement-Eigenschaft.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Ist Sie am unteren Rand der Lehre Tipp ein Border-auf-Rahmen-Image mit einem Cartoon-Mann, das Bereitstellen von Dateien in der Cloud. Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Fügen Sie ein Symbol hinzu.

Ein Symbol kann neben den Titel und Untertitel, die mithilfe der IconSource-Eigenschaft hinzugefügt werden. Empfohlene Symbolgrößen enthalten 16px 24px und 32 Pixel. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-app mit einer Lehre Spitze, die für den Speichervorgang Schaltfläche. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Ist Sie auf der linken Seite, der den Titel und Untertitel einem Diskettensymbol gekennzeichnet. Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Enable-Light-dismiss

Light-dismiss-Funktionalität ist standardmäßig deaktiviert, kann aber aktiviert, damit ein Trinkgeld Lehre, z. B. schließt, wenn ein Benutzer einen Bildlauf durchführt oder mit anderen Elementen der Anwendung interagiert. Aufgrund dieses Verhaltens Light-dismiss Tipps sind die beste Lösung, wenn ein Trinkgeld in einem bildlauffähigen Bereich platziert werden muss. 

Die Schaltfläche "Schließen" wird automatisch aufgehoben, eine Light-Dismiss aktiviert Lehre Tipps zum Identifizieren der Light-dismiss-Verhalten für Benutzer. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Eine Beispiel-app mit einer Light-dismiss Lehre-Tipp in der unteren rechten Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf."](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Escapezeichen für die Begrenzungen des XAML-Stamm

Unter Windows kann Version 19 H-1 und höher ein Lehre-Tipp die Grenzen der XAML-Stammverzeichnis und den Bildschirm mit Escapezeichen versehen durch Festlegen der ShouldConstrainToRootBounds-Eigenschaft. Wenn diese Eigenschaft aktiviert ist, ein Trinkgeld Unterricht wird nicht versucht, behalten Sie die Grenzen des XAML-Stammverzeichnis und den Bildschirm und wird immer Position, an die Gruppe PreferredPlacement-Modus. Es wird empfohlen, aktivieren die IsLightDismissEnabled-Eigenschaft, und legen Sie den PreferredPlacement-Modus am ehesten entsprechen den Mittelpunkt des Xaml-Stamm, um sicherzustellen, dass die beste Lösung für Benutzer.

In früheren Versionen von Windows diese Eigenschaft wird ignoriert, und der Unterricht Tipp bleibt immer innerhalb der Grenzen des XAML-Stamm.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![Eine Beispiel-app mit einer Spitze Lehre außerhalb der app in der unteren rechten Ecke. Der tipptitel liest "Automatisch speichern" und der Untertitel liest "Ihre Änderungen speichern wir alles – müssen Sie niemals auf." Es gibt eine Schaltfläche "Schließen" auf der oberen rechten Ecke des Unterrichts Tipps.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Abbrechen und schließen verzögern

Das Closing-Ereignis kann Abbrechen und/oder verzögern das Schließen eines Tipps Lehre verwendet werden. Dies kann verwendet werden, lassen Sie den Unterricht Tipp geöffnet oder die Zeit für eine Aktion oder eine benutzerdefinierte Animation ausgeführt wird. Wenn das Schließen eines Tipps Lehre abgebrochen wird, IsOpen kehren zurück zu "true", aber es bleibt "false" während der Verzögerung. Eine programmgesteuerte schließen kann auch abgebrochen werden. 

**Hinweis: Wenn keine Platzierungsoption ein Trinkgeld Lehre vollständig angezeigt erlauben würde, durchläuft Lehre Tipp Lebenszyklus Ereignis anzuzeigen, ohne dass ein zugänglich Schaltfläche "Schließen", statt eine erzwingen. Wenn die app das Closing-Ereignis abbricht, kann der Unterricht Tipp ohne eine zugänglich Schaltfläche "Schließen" geöffnet bleiben.**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>Hinweise

### <a name="related-articles"></a>Verwandte Artikel 

* [Dialogfelder und Flyouts](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>Empfehlungen
* Tipps sind impermanent und darf keine Informationen oder Optionen, die auf die Benutzeroberfläche einer Anwendung von entscheidender Bedeutung sind. 
* Versuchen Sie, dass keine Lehre Tipps zu häufig. Lehre Tipps sind wahrscheinlich jedes einzelnen Aufmerksamkeit erhalten, wenn sie in der gesamten langen Sitzungen oder über mehrere Sitzungen hinweg aufgespielt werden.    
* Halten Sie Tipps, die Lage versetzt, kompakte und deren Thema löschen. Untersuchungen zeigen die Benutzer durchschnittlich nur lesen Sie 3 bis 5 Wörter zu und nur verstehen Sie 2 bis 3 Wörter, bevor Sie sich entscheiden, ob ein Trinkgeld zu interagieren.
* Zugriff auf einen Tipp Lehre Gamepad ist nicht garantiert. Anwendungen, die Gamepad-Eingaben vorhergesagt, finden Sie unter [Gamepad und Remotesteuerung Interaktionen]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Es wird empfohlen, um Gamepad Zugriff auf jede Lehre Tip mit allen möglichen Konfigurationen der Benutzeroberfläche einer app zu testen.
* Wenn Sie einen Tipp Lehre im XAML-Stammverzeichnis mit Escapezeichen versehen zu aktivieren, wird empfohlen, auch aktivieren die IsLightDismissEnabled-Eigenschaft, und legen Sie den PreferredPlacement-Modus am ehesten entsprechen den Mittelpunkt des XAML-Stamm. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Neukonfigurieren einer geöffneten Lehre Tipp

Manche Inhalte und Eigenschaften können neu konfiguriert werden während der Lehre Tipp geöffnet ist, und werden sofort wirksam. Andere Inhalt und Eigenschaften, benötigen z. B. die Symboleigenschaft, die Aktion, und schließen Sie Schaltflächen und Neukonfiguration zwischen Light-dismiss und explizite schließen alle den Unterricht-Tipp geschlossen und erneut geöffnet wird, Änderungen an diesen Eigenschaften wirksam werden. Beachten Sie, die Änderung einer Entlassung von manuell-dismiss um Light-dismiss während ein Trinkgeld Lehre geöffnet ist führt dazu, dass den Tipp Unterricht haben Sie die Schaltfläche "Schließen" entfernt, bevor die Light-dismiss Verhalten wird aktiviert, und der QuickInfo kann auf dem Bildschirm unterbrochene bleiben.
