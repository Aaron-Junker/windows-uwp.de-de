---
Description: Ein Unterrichtstipp ist ein semipersistentes und inhaltsreiches Flyout, das Kontextinformationen bereitstellt.
title: Unterrichtstipps
template: detail.hbs
ms.date: 04/01/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 997a0c32de9d6ee803095f5c708f6ed8cedaf141
ms.sourcegitcommit: 6009896ead442b378106d82870f249dc8b55b886
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89643839"
---
# <a name="teaching-tip"></a>Unterrichtstipp

Ein Unterrichtstipp ist ein semipersistentes und inhaltsreiches Flyout, das Kontextinformationen bereitstellt. Es wird häufig verwendet, um Benutzer über wichtige und neue Features zu informieren, die die Benutzerfreundlichkeit verbessern, sowie um sie an solche Features zu erinnern und sie entsprechend zu schulen.

Ein Unterrichtstipp ist möglicherweise einfach ausblendbar oder erfordert explizite Aktionen, damit er geschlossen werden kann. Ein Unterrichtstipp weist ggf. mit einer Spitze auf ein spezifisches Element der Benutzeroberfläche oder kann auch ohne Spitze oder Ziel verwendet werden.

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](../images/winui-logo-64x64.png) | Das Steuerelement **TeachingTip** erfordert die Windows-UI-Bibliothek. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **APIs der Bibliothek „Windows UI“** [TeachingTip-Klasse](/uwp/api/microsoft.ui.xaml.controls.teachingtip)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwende ein **TeachingTip**-Steuerelement, um die Aufmerksamkeit eines Benutzers auf neue oder wichtigen Updates und Features zu lenken, ihn an Optionen zu erinnern, die nicht zwingend erforderlich sind, aber die Benutzung optimieren, oder ihm zu erläutern, wie eine Aufgabe abgeschlossen werden soll.

Da Unterrichtstipps vorübergehend sind, sind sie nicht das empfohlene Steuerelement, um Benutzer auf Fehler oder wichtige Statusänderungen hinzuweisen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert hast, klicke hier, um <a href="xamlcontrolsgallery:/item/TeachingTip">die App zu öffnen und TeachingTip in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Ein Unterrichtstipp kann mehrere Konfigurationen haben, u. a. folgende:

Ein Unterrichtstipp weist unter Umständen mit der Spitze auf ein spezifisches Element der Benutzeroberfläche, um den Kontext der dargestellten Informationen zu verbessern.

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-targeted.png)

Wenn die dargestellten Informationen sich nicht auf ein bestimmtes Element der Benutzeroberfläche beziehen, kann ein nicht zielgerichteter Unterrichtstipp erstellt werden, indem die Spitze entfernt wird.

![Eine Beispiel-App mit einem Unterrichtstipp in der unteren rechten Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-non-targeted.png)

Ein Unterrichtstipp erfordert es möglicherweise, dass der Benutzer ihn über die Schaltfläche „X“ in der oberen Ecke oder die Schaltfläche „Schließen“ im unteren Bereich verwirft. Ein Unterrichtstipp ist möglicherweise einfach ausblendbar. In dem Fall gibt es keine Schaltfläche zum Verwerfen, und der Unterrichtstipp wird stattdessen verworfen, wenn der Benutzer einen Bildlauf durchführt oder mit anderen Elementen der Anwendung interagiert. Aufgrund dieser Verhaltensweise sind einfach ausblendbare Tipps die beste Lösung, wenn ein Tipp in einem bildlauffähigen Bereich platziert werden muss.

![Eine Beispiel-App mit einem einfach ausblendbaren Unterrichtstipp in der unteren rechten Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.).](../images/teaching-tip-light-dismiss.png)

### <a name="create-a-teaching-tip"></a>Erstellen eines Unterrichtstipps

Dies ist die XAML für ein zielgerichtetes Unterrichtstipp-Steuerelement, das das standardmäßige Aussehen von TeachingTip mit Titel und Untertitel veranschaulicht.
Beachte dabei, dass der Unterrichtstipp an einer beliebigen Stelle in der Elementstruktur oder im zugrunde liegenden Code angezeigt werden kann. Im folgenden Beispiel befindet er sich in ResourceDictionary.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

```csharp
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

Dies ist das Ergebnis, wenn die Seite mit der Schaltfläche und dem Unterrichtstipp angezeigt wird:

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-targeted.png)

Im obigen Beispiel werden die Eigenschaften [Title](/uwp/api/microsoft.ui.xaml.controls.teachingtip.title) und [Subtitle](/uwp/api/microsoft.ui.xaml.controls.teachingtip.subtitle) verwendet, um den Titel und Untertitel des Unterrichtstipps festzulegen. Die Eigenschaft [Target](/uwp/api/microsoft.ui.xaml.controls.teachingtip.target) wird auf „SaveButton“ festgelegt, um die visuelle Verbindung zwischen ihr und der Schaltfläche herzustellen. Um den Unterrichtstipp anzuzeigen, wird seine Eigenschaft [IsOpen](/uwp/api/microsoft.ui.xaml.controls.teachingtip.isopen) auf `true` festgelegt.

### <a name="non-targeted-tips"></a>Nicht zielgerichtete Tipps

Nicht alle Tipps beziehen sich auf ein Element auf dem Bildschirm. Für diese Szenarien wird kein Ziel festgelegt, und der Unterrichtstipp wird stattdessen relativ zu den Rändern des XAML-Stamms angezeigt. Allerdings kann bei einem Unterrichtstipp die Spitze entfernt werden, während die Platzierung in Bezug zum Element der Benutzeroberfläche durch Festlegen der Eigenschaft [TailVisibility](/uwp/api/microsoft.ui.xaml.controls.teachingtip.tailvisibility) auf „Collapsed“ beibehalten wird. Das folgende Beispiel zeigt einen nicht zielgerichteten Unterrichtstipp.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</muxc:TeachingTip>
```

In diesem Beispiel befindet sich TeachingTip in der Elementstruktur anstatt in ResourceDictionary oder im zugrunde liegenden Code. Dies hat keine Auswirkungen auf das Verhalten; TeachingTip wird nur beim Öffnen angezeigt und nimmt keinen Layoutplatz in Anspruch.

![Eine Beispiel-App mit einem Unterrichtstipp in der unteren rechten Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Bevorzugte Platzierung

Der Unterrichtstipp repliziert das [FlyoutPlacementMode](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)-Platzierungsverhalten des Flyouts mit der Eigenschaft [PreferredPlacement](/uwp/api/microsoft.ui.xaml.controls.teachingtip.preferredplacement). Der Standardmodus für die Platzierung versucht, einen zielgerichteten Unterrichtstipp über dem Ziel zu platzieren, während ein nicht zielgerichteter Unterrichtstipp in der Mitte unter dem XAML-Stamm platziert wird. So wie beim Flyout wird automatisch ein anderer Platzierungsmodus ausgewählt, wenn der bevorzugte Platzierungsmodus nicht genug Platz für die Anzeige des Unterrichtstipps lässt.

Weitere Informationen zu Anwendungen, die Gamepadeingaben vorhersagen, findest du unter [Gamepad und Fernbedienung]( ../../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction). Es wird empfohlen, den Zugriff des Gamepads auf alle Unterrichtstipps mit allen möglichen Konfigurationen der Benutzeroberfläche einer App zu testen.

Ein zielgerichteter Unterrichtstipp, bei dem PreferredPlacement auf „BottomLeft“ festgelegt ist, erscheint mit der Spitze zentriert am unteren Rand des Ziels, und der Text des Unterrichtstipps wird nach links verschoben.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-App mit der Schaltfläche „Save“ (Speichern), auf die ein Unterrichtstipp verweist, darunter in der linken Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-targeted-preferred-placement.png)

Ein nicht zielgerichteter Unterrichtstipp, bei dem PreferredPlacement auf „BottomLeft“ festgelegt ist, wird unten links im XAML-Stamm angezeigt.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</muxc:TeachingTip>
```

![Eine Beispiel-App mit einem Unterrichtstipp in der unteren linken Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-non-targeted-preferred-placement.png)

Das folgende Diagramm zeigt das Ergebnis aller 13 PreferredPlacement-Modi, die für zielgerichtete Unterrichtstipps festgelegt werden können.
![Abbildung mit 13 Unterrichtstipps, die jeweils einen andere Zielplatzierungsmodus zeigen Jeder Unterrichtstipp ist mit dem Modus gekennzeichnet, den er darstellt. Das erste Wort einen Platzierungsmodus gibt die Seite des Ziels an, wo der Unterrichtstipp zentriert angezeigt wird. Die Spitze der Unterrichtstipps befindet sich immer in der Mitte der Seite des Ziels und verweist auf das Ziel. Wenn im Platzierungsmodus ein zweites Wort vorhanden ist, wird der Text des Unterrichtstipps nicht zentriert, sondern wird stattdessen in die angegebene Richtung verschoben. Beispiel: Im Platzierungsmodus „TopRight“ erscheint der Unterrichtstipp über dem Ziel und nach rechts verschoben, wobei die Spitze am oberen Rand des Ziels zentriert nach unten weist. Da der Text nach rechts verschoben wurde, befindet sich die Spitze fast am linken Rand des Texts des Unterrichtstipps, der sich über den rechten Rand des Ziels hinaus erstreckt. Die Platzierungsmodus „Center“ ist eindeutig und platziert die Spitze des Unterrichtstipps in der Mitte des Ziels, und der Unterrichtstipp befindet sich zentriert über der oberen Hälfte des Ziels.](../images/teaching-tip-targeted-preferred-placement-modes.png)

Das folgende Diagramm zeigt das Ergebnis aller 13 PreferredPlacement-Modi, die für nicht zielgerichtete Unterrichtstipps festgelegt werden können.
![Abbildung mit neun Unterrichtstipps, die jeweils einen anderen nicht zielgerichteten Platzierungsmodus zeigen Jeder Unterrichtstipp ist mit dem Modus gekennzeichnet, den er darstellt. Das erste Wort eines Platzierungsmodus gibt die Seite des XAML-Stamms an, wo der Unterrichtstipp zentriert angezeigt wird. Wenn im Platzierungsmodus ein zweites Wort vorhanden ist, wird der Unterrichtstipp zur angegebenen Ecke des XAML-Stamms hin platziert. Beispiel: Der Platzierungsmodus „TopRight“ sorgt dafür, dass der Unterrichtstipp in der oberen rechten Ecke des XAML-Stamms angezeigt wird. Bei nicht zielgerichteten Platzierungsmodi wirkt sich die Reihenfolge der Wörter nicht auf die Platzierung aus. TopRight entspricht RightTop. Der Platzierungsmodus „Center“ ist eindeutig und sorgt dafür, dass der Unterrichtstipp in der vertikalen und horizontalen Mitte des XAML-Stamms angezeigt wird.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Hinzufügen eines Platzierungsrands

Mithilfe der Eigenschaft [PlacementMargin](/uwp/api/microsoft.ui.xaml.controls.teachingtip.placementmargin) können Sie steuern, wie weit ein zielgerichteter Unterrichtstipp vom Ziel entfernt ist und wie weit ein nicht zielgerichteter Unterrichtstipp von den Rändern des XAML-Stamms entfernt ist. PlacementMargin verfügt wie [Margin](/uwp/api/windows.ui.xaml.frameworkelement.margin) über vier Werte (left, right, top und bottom), sodass nur die relevanten Werte verwendet werden. PlacementMargin.Left gilt beispielsweise, wenn sich der Tipp links vom Ziel oder am linken Rand des XAML-Stamms befindet.

Das folgende Beispiel zeigt einen nicht zielgerichteten Tipp, bei dem die PlacementMargin-Werte „Left“/„Top“/„Right“/„Bottom“ jeweils auf 80 festgelegt sind.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</muxc:TeachingTip>
```

![Eine Beispiel-App, bei der der Unterrichtstipp zur unteren rechten Ecke hin platziert ist, sich aber nicht vollständig dort befindet. Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Inhalt hinzufügen

Mithilfe der [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)-Eigenschaft können Sie Inhalt zu einem Unterrichtstipp hinzufügen. Wenn mehr Inhalte angezeigt werden sollen, als die Größe eines Lerntipps zulässt, wird automatisch eine Bildlaufleiste aktiviert, damit ein Benutzer im Inhaltsbereich einen Bildlauf durchführen kann.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Im Inhaltsbereich des Unterrichtstipps befindet sich ein Kontrollkästchen mit der Beschriftung „Don't show tips at startup“ (Tipps beim Start nicht anzeigen), und darunter wird der Text „You can change your tip preferences in Settings if you change your mind“ (Du kannst deine Tippeinstellungen in den Einstellungen ändern, wenn du deine Meinung änderst.) angezeigt, wobei sich „Settings“ (Einstellungen) auf die Einstellungsseite der App bezieht. Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Hinzufügen von Schaltflächen

Standardmäßig wird neben dem Titel eines Unterrichtstipps die Schaltfläche „X“ zum Schließen angezeigt. Die Schließen-Schaltfläche kann mit der Eigenschaft [CloseButtonContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.closebuttoncontent) angepasst werden. In dem Fall wird die Schaltfläche an den unteren Rand des Unterrichtstipps verschoben.

**Hinweis: Bei einfach ausblendbaren Tipps wird keine Schließen-Schaltfläche angezeigt**

Eine Schaltfläche für benutzerdefinierte Aktionen kann durch Festlegen der [ActionButtonContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncontent)-Eigenschaft hinzugefügt werden (und optional den Eigenschaften [ActionButtonCommand](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncommand) und [ActionButtonCommandParameter](/uwp/api/microsoft.ui.xaml.controls.teachingtip.actionbuttoncommandparameter)).

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="{x:Bind DisableAutoSaveCommand}"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Im Inhaltsbereich des Unterrichtstipps befindet sich ein Kontrollkästchen mit der Beschriftung „Don't show tips at startup“ (Tipps beim Start nicht anzeigen), und darunter wird der Text „You can change your tip preferences in Settings if you change your mind“ (Du kannst deine Tippeinstellungen in den Einstellungen ändern, wenn du deine Meinung änderst.) angezeigt, wobei sich „Settings“ (Einstellungen) auf die Einstellungsseite der App bezieht. Unten am Tipp befinden sich zwei Schaltflächen: eine graue auf der linken Seite mit dem Text „Disable“ (Deaktivieren) und eine blaue auf der rechten Seite mit dem Text „Got it!“ (Verstanden).](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero-Inhalt

Mithilfe der Eigenschaft [HeroContent](/uwp/api/microsoft.ui.xaml.controls.teachingtip.herocontent) können randlose Inhalte zu einem Unterrichtstipp hinzugefügt werden. Die Platzierung des Hero-Inhalts kann mit der Eigenschaft [HeroContentPlacement](/uwp/api/microsoft.ui.xaml.controls.teachingtip.herocontentplacement) auf oben oder unten in einem Unterrichtstipp festgelegt werden.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <muxc:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </muxc:TeachingTip.HeroContent>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Unten im Unterrichtstipp befindet sich ein randloses gezeichnetes Bild, auf dem ein Mann Dateien in eine Wolke legt. Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Hinzufügen eines Symbols

Neben dem Titel und dem Untertitel können Sie mithilfe der Eigenschaft [IconSource](/uwp/api/microsoft.ui.xaml.controls.teachingtip.iconsource) ein Symbol hinzufügen. Die empfohlene Symbolgröße ist u. a. 16 px, 24 px und 32 px.

```xaml
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <muxc:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <muxc:TeachingTip.IconSource>
                <muxc:SymbolIconSource Symbol="Save" />
            </muxc:TeachingTip.IconSource>
        </muxc:TeachingTip>
    </Button.Resources>
</Button>
```

![Eine Beispiel-App mit einem Unterrichtstipp, der auf die Schaltfläche „Speicher“ verweist Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Links neben dem Titel und dem Untertitel befindet sich ein Diskettensymbol. Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Einfaches Ausblenden

Die Funktion zum einfachen Ausblenden ist standardmäßig deaktiviert, kann aber durch Festlegen der Eigenschaft [IsLightDismissEnabled](/uwp/api/microsoft.ui.xaml.controls.teachingtip.islightdismissenabled) aktiviert werden, damit ein Unterrichtstipp geschlossen wird, wenn der Benutzer beispielsweise einen Bildlauf durchführt oder mit anderen Elementen der Anwendung interagiert. Aufgrund dieser Verhaltensweise sind einfach ausblendbare Tipps die beste Lösung, wenn ein Tipp in einem bildlauffähigen Bereich platziert werden muss.

Die Schließen-Schaltfläche wird aus einem einfach ausblendbaren Unterrichtstipp automatisch entfernt, damit dieses Verhalten für die Benutzer offensichtlich ist.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</muxc:TeachingTip>
```

![Eine Beispiel-App mit einem einfach ausblendbaren Unterrichtstipp in der unteren rechten Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.).](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Umgehen der Begrenzungen des XAML-Stamms

Ab Windows 10, Version 1903 (Build 18362), kann ein Unterrichtstipp die Begrenzungen des XAML-Stamms und des Bildschirms umgehen. Legen Sie hierfür die Eigenschaft [ShouldConstrainToRootBounds](/uwp/api/microsoft.ui.xaml.controls.teachingtip.shouldconstraintorootbounds) fest. Wenn diese Eigenschaft aktiviert ist, bleibt der Unterrichtstipp nicht innerhalb der Begrenzungen des XAML-Stamms und wird immer dem festgelegten `PreferredPlacement`-Modus entsprechend platziert. Aktiviere die Eigenschaft `IsLightDismissEnabled`, und lege den `PreferredPlacement`-Modus relativ mittig im XAML-Stamm fest, um die größtmögliche Benutzerfreundlichkeit sicherzustellen.

In früheren Versionen von Windows wird diese Eigenschaft ignoriert, und der Unterrichtstipp bleibt immer innerhalb der Begrenzungen des XAML-Stamms.

```xaml
<Button x:Name="SaveButton" Content="Save" />

<muxc:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</muxc:TeachingTip>
```

![Eine Beispiel-App mit einem Unterrichtstipp außerhalb der unteren rechten Ecke Der Tipptitel lautet „Saving automatically“ (Automatisch speichern) und der Untertitel lautet „We save your changes as you go - so you never have to.“ (Wir speichern deine Änderungen während der Arbeit, damit du das nicht tun musst.). Rechts oben im Unterrichtstipp gibt es eine Schaltfläche zum Schließen.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Abbrechen und Verzögern des Vorgangs zum Schließen

Das [Schließen](/uwp/api/microsoft.ui.xaml.controls.teachingtip.closing)-Ereignis kann verwendet werden, um das Schließen eines Unterrichtstipps abzubrechen und/oder zu verzögern. Dadurch kannst du den Unterrichtstipp offen lassen oder etwas Zeit einräumen, damit eine Aktion oder eine benutzerdefinierte Animation ausgeführt wird. Wenn das Schließen eines Unterrichtstipps abgebrochen wird, wechselt IsOpen wieder zu „true“, während der Verzögerung wird jedoch die Einstellung „false“ beibehalten. Programmgesteuertes Schließen kann auch abgebrochen werden.

> [!NOTE]
> Wenn keine Platzierungsoption es erlaubt, dass ein Unterrichtstipp vollständig angezeigt wird, durchläuft dieser seinen Ereignislebenszyklus, um ein Schließen zu erzwingen, wenn auf keine Schließen-Schaltfläche zugegriffen werden kann. Wenn die App das Schließen-Ereignis abbricht, bleibt der Unterrichtstipp u. U. offen, wenn auf keine Schließen-Schaltfläche zugegriffen werden kann.

```xaml
<muxc:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</muxc:TeachingTip>
```

```csharp
private void OnTipClosing(muxc.TeachingTip sender, muxc.TeachingTipClosingEventArgs args)
{
    if (args.Reason == muxc.TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                // We were not able to update the settings!
                // Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="recommendations"></a>Empfehlungen

* Tipps sind dauerhaft und dürfen keine Informationen oder Optionen enthalten, die für die Nutzung einer Anwendung von entscheidender Bedeutung sind.
* Versuche, Unterrichtstipps nicht zu häufig anzuzeigen. Unterrichtstipps werden dann am ehesten beachtet, wenn sie während langer Sitzungen oder über mehrere Sitzungen hinweg gestaffelt angezeigt werden.
* Außerdem sollten sie kompakt und gut verständlich sein. Untersuchungen zeigen, dass Benutzer im Durchschnitt nur drei bis fünf Wörter lesen und zwei bis drei Wörter erfassen, wenn sie entscheiden, ob sie mit einem Tipp interagieren.
* Der Zugriff auf Gamepads ist bei einem Unterrichtstipp nicht garantiert. Weitere Informationen zu Anwendungen, die Gamepadeingaben vorhersagen, findest du unter [Gamepad und Fernbedienung]( ../../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction). Es wird empfohlen, den Zugriff des Gamepads auf alle Unterrichtstipps mit allen möglichen Konfigurationen der Benutzeroberfläche einer App zu testen.
* Wenn du einen Unterrichtstipp aktivierst, um den XAML-Stamm zu umgehen, solltest du auch die Eigenschaft „IsLightDismissEnabled“ aktivieren und den PreferredPlacement-Modus recht mittig im XAML-Stamm festlegen.

## <a name="reconfiguring-an-open-teaching-tip"></a>Neukonfiguration eines geöffneten Unterrichtstipps

Manche Inhalte und Eigenschaften können neu konfiguriert werden, wenn der Unterrichtstipp geöffnet ist, und werden sofort wirksam. Andere Inhalte und Eigenschaften wie etwa die Symboleigenschaft, die Aktions- und Schließen-Schaltfläche und die Neukonfiguration zwischen einfachem Ausblenden und explizitem Ausblenden erfordern es, dass der Unterrichtstipp geschlossen und wieder geöffnet wird, damit die Änderungen der entsprechenden Eigenschaften wirksam werden. Wenn du das Ausblendverhalten von manuellem Ausblenden in einfaches Ausblenden änderst, während ein Unterrichtstipp geöffnet ist, wird die Schließen-Schaltfläche entfernt, bevor das einfache Ausblenden aktiviert wird, und der Tipp kann auf dem Bildschirm verbleiben.

## <a name="related-articles"></a>Verwandte Artikel

* [Dialogfelder und Flyouts](./index.md)
