---
description: Zeigen Sie, dass der Fokus einen Beleuchtungseffekt ist, der den Rahmen des Fokus Elemente animiert, wenn der Benutzer Gamepad oder die Tastatur deren Fokus.
title: Zeigen Sie den Fokus
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 2cb91b37b1d2a6924a80bbe666129343ad778d8e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370468"
---
# <a name="reveal-focus"></a>Zeigen Sie den Fokus

![Favoritenbild](images/header-reveal-focus.svg)

Anzeigen-Schwerpunkt liegt eine Auswirkung der Beleuchtung für [10-Fuß Erfahrungen](/windows/uwp/design/devices/designing-for-tv), z. B. Xbox One und TV-Bildschirm. Sie animieren den Rahmen des fokussierbaren Elementes wie beispielsweise Schaltflächen, wenn der Benutzer den Fokus des Gamepad oder der Tastatur darauf lenken. Es ist standardmäßig deaktiviert, lässt sich allerdings ganz einfach aktivieren. 

(Die Auswirkungen anzuzeigen markieren eine Beleuchtung auswirken, die interaktive Elemente hervorhebt finden Sie unter der [Offenlegen markieren Artikel](/windows/uwp/design/style/reveal).)


> **Wichtige APIs:** [Application.FocusVisualKind Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind Enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Funktionsweise
Zeigen Sie Aufrufe der Aufmerksamkeit auf fokussierte Elemente durch Hinzufügen einer animierten Lichteffekts rund um den Rahmen des Elements:

![Reveal Visual](images/traveling-focus-fullscreen-light-rf.gif)

Dies ist besonders hilfreich, in 10 Fuß Szenarien, in denen der Benutzer keine volle Aufmerksamkeit zu den gesamten TV-Bildschirm bezahlen kann. 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RevealFocus">öffnen Sie die app, und zeigen den Fokus in Aktion erleben</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Verwendung

Zeigen Sie, dass der Fokus standardmäßig deaktiviert ist. So aktivieren Sie es:
1. Rufen Sie im Konstruktor der App die Eigenschaft [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) auf, und überprüfen Sie, ob die aktuellen Gerätefamilie `Windows.Xbox` ist.
2. Wenn die Gerätefamilie `Windows.Xbox` ist, legen Sie die Eigenschaft [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) auf `FocusVisualKind.Reveal` fest. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Nach dem Festlegen der **FocusVisualKind** -Eigenschaft, das System automatisch den Fokus anzeigen ab, wendet alle Steuerelemente, deren [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) -Eigenschaftensatz auf **"true"** (der Standardwert für die meisten Steuerelemente). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Warum wird nicht zeigen den Fokus auf standardmäßig ein? 
Wie Sie sehen können, ist es relativ einfach, die im Fokus anzeigen zu aktivieren, wenn die app erkennt, dass er auf eine Xbox ausgeführt wird. Warum aktiviert das System die Funktion nicht einfach automatisch? Da Offenlegen Fokus das visuelle Fokuselement vergrößert, kann die Probleme mit das Layout der Benutzeroberfläche führen. In einigen Fällen sollten Sie zum Anpassen des Effekt Fokus anzuzeigen, um es für Ihre app zu optimieren.

## <a name="customizing-reveal-focus"></a>Anpassen der anzeigen-Fokus

Sie können die Wirkung zeigen den Fokus durch Ändern der Eigenschaften der Fokus visual für jedes Steuerelement anpassen: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), und [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Dank dieser Eigenschaften können Sie die Farbe und Breite des Fokusrechtecks anpassen. (Dies sind die gleichen Eigenschaften, die Sie zum Erstellen der [Fokuselemente mit hoher Sichtbarkeit](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals) verwenden.) 

Aber vor der Installation anpassen, es ist hilfreich, ein wenig mehr über die Komponenten zu wissen, aus denen Fokus anzuzeigen.

Es gibt drei Teile der standardmäßigen Offenlegen Fokuselemente: den Rahmen des primären, sekundären Rahmen und dem aufdecken, leuchten. Der primäre Rahmen ist **2 Pixel** breit und verläuft an der *Außenseite* des sekundären Rahmens. Der sekundäre Rahmen ist **1 Pixel** breit und verläuft an der *Innenseite* des primären Rahmens. Der Leuchteffekt zeigen den Fokus verfügt über eine Stärke, die proportional zu die Stärke des Rahmens primären und ausgeführt wird, um die *außerhalb* primären Rahmenseite.

Zusätzlich zu den statischen Elementen feature anzeigen Fokuselemente eine animierte Licht, das pulsates, wenn bei der Fußstützen und in Richtung der Fokus verschoben wird, wenn Sie den Fokus zu verschieben.

![Fokus Ebenen anzeigen](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Anpassen der Stärke des Rahmens

Verwenden Sie diese Eigenschaften, um die Breite der Rahmentypen eines Steuerelements zu ändern:

| Rahmentyp | Eigenschaft |
| --- | --- |
| Primär, Schein   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Das Ändern des primären Rahmens ändert die Breite des Scheins proportional.)   |
| Sekundärer   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


In diesem Beispiel wird die Breite des Rahmens des visuellen Fokus für eine Schaltfläche geändert:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Anpassen des Rands

Der Rand ist der Abstand zwischen den visuellen Grenzen des Steuerelements und dem Beginn des sekundären Rahmens der visuellen Fokuselemente. Der standardmäßige Rand hat eine Breite von 1 Pixel außerhalb der Grenzen des Steuerelements. Sie können diesen Rand pro Steuerelement bearbeiten, indem Sie die Eigenschaft [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) ändern:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Ein negativer Rand verschiebt den Rahmen weiter weg von der Mitte des Steuerelements. Ein positiver Rand verschiebt den Rahmen näher zur Mitte des Steuerelements.

## <a name="customize-the-color"></a>Anpassen der Farbe

Verwenden Sie zum Ändern der Farbe des das visuelle Fokuselement Anzeigen der [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) und [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) Eigenschaften.

| Eigenschaft | Standardressource | Standardressourcewert |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(Die Eigenschaft des FocusPrimaryBrush wird standardmäßig als **SystemControlRevealFocusVisualBrush**-Ressourcen verwendet, wenn **FocusVisualKind** auf **Einblenden**  festgelegt ist. Andernfalls wird **SystemControlFocusVisualPrimaryBrush** verwendet.)


Um die Farbe des visuellen Fokus eines einzelnen Steuerelements zu ändern, legen Sie die Eigenschaften direkt im Steuerelement fest. In diesem Beispiel wird die Farbe der Fokusanzeige einer Schaltfläche außer Kraft gesetzt.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

Um die Farbe aller Fokusanzeigen in der gesamten App zu ändern, setzen Sie die Ressourcen **SystemControlRevealFocusVisualBrush** und **SystemControlFocusVisualSecondaryBrush** mit Ihrer eigenen Definition außer Kraft:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Weitere Informationen zum Ändern der Farbe der Fokusanzeige finden Sie unter [Farbbranding und -anpassung](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Nur den Schein anzeigen

Wenn Sie nur den Schein ohne die primäre oder sekundäre Fokusanzeige verwenden möchten, legen Sie einfach die Eigenschaften des Steuerelements [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush)`Transparent`und [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) auf `0` fest. In diesem Fall übernimmt der Schein die Farbe des Hintergrunds des Steuerelements für ein randlos Verhalten. Sie können die Breite des Scheins mit [FocusVisualPrimaryThickness ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) ändern.

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Verwenden Sie Ihre eigenen visuellen Fokuselemente

Eine weitere Möglichkeit zum Anpassen der Fokus anzuzeigen ist, die vom System bereitgestellte visuelle Fokuselemente deaktivieren, zeichnen Sie Ihre eigenen visuellen Zustände mithilfe. Weitere Informationen finden Sie unter [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Zeigen Sie den Fokus und die Fluent-Entwurfssystem

Zeigen Sie, dass der Fokus eine Fluent-Entwurfssystem-Komponente handelt, die Ihre app Licht hinzufügt. Weitere Informationen zum Fluent Design-Systems und den zugehörigen Komponenten finden Sie unter [Fluent Design für UWP-Übersicht](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Verwandte Artikel

- [Markierung anzeigen](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Entwerfen für Xbox und Fernsehgeräte](/windows/uwp/design/devices/designing-for-tv)
- [Gamepad und Remotesteuerung Interaktionen](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Auswirkungen der Komposition](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Die Wissenschaft im System: Fluent Design und der Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Die Wissenschaft im System: Fluent Design und Licht](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
