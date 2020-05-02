---
description: Reveal Focus ist ein Lichteffekt, der den Rahmen von fokussierbaren Elementen animiert, wenn der Benutzer den Fokus eines Gamepad oder einer Bildschirmtastatur darauf verschiebt.
title: Reveal Focus
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 824476cb098d0ff561fca67497a896586c70b8fb
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75681961"
---
# <a name="reveal-focus"></a>Reveal Focus

![Herobild](images/header-reveal-focus.svg)

Reveal Focus ist ein Lichteffekt für [10-Foot-Benutzeroberflächen](/windows/uwp/design/devices/designing-for-tv), z.B. Xbox One und Fernsehbildschirme. Er animiert den Rahmen von fokussierbaren Elementen wie z.B. Schaltflächen, wenn der Benutzer den Fokus eines Gamepad oder einer Bildschirmtastatur darauf verschiebt. Standardmäßig ist er deaktiviert, kann aber ganz einfach aktiviert werden. 

(Informationen zum Reveal Highlight-Effekt – einem Lichteffekt, der interaktive Elemente hervorhebt – finden Sie im [Artikel „Reveal Highlight“](/windows/uwp/design/style/reveal).)


> **Wichtige APIs:** [Eigenschaft „Application.FocusVisualKind“](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [Enumeration „FocusVisualKind“](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Eigenschaft „Control.UseSystemFocusVisuals“](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Funktionsweise
Reveal Focus lenkt die Aufmerksamkeit auf Elemente mit dem Fokus, indem ein animiertes Leuchten um den Rahmen des Elements herum eingeblendet wird:

![Visuelle Einblendung](images/traveling-focus-fullscreen-light-rf.gif)

Dies ist besonders hilfreich in 10-Foot-Szenarien, bei denen sich der Benutzer vielleicht nicht auf den ganzen Fernsehbildschirm konzentriert. 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RevealFocus">die App zu öffnen und Reveal Focus in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Verwendung

Reveal Focus ist standardmäßig deaktiviert. So aktivieren Sie es:
1. Rufen Sie im Konstruktor Ihrer App die Eigenschaft [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) auf, und überprüfen Sie, ob `Windows.Xbox` die aktuelle Gerätefamilie ist.
2. Wenn `Windows.Xbox` diese Gerätefamilie ist, legen Sie die Eigenschaft [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) auf `FocusVisualKind.Reveal` fest. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Nachdem Sie die Eigenschaft **FocusVisualKind** festgelegt haben, wendet das System den Reveal Focus-Effekt automatisch auf alle Steuerelemente an, deren Eigenschaft [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) auf **True** festgelegt ist (dem Standardwert für die meisten Steuerelemente). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Warum ist Reveal Focus standardmäßig nicht aktiviert? 
Wie Sie sehen können, lässt sich Reveal Focus relativ einfach aktivieren, wenn die App erkennt, dass sie auf Xbox ausgeführt wird. Warum aktiviert das System die Funktion also nicht einfach automatisch? Reveal Focus vergrößert das visuelle Fokuselement, wodurch Probleme bei Ihrem UI-Layout entstehen können. In einigen Fällen möchten Sie den Reveal Focus-Effekt anpassen, um ihn für Ihre App zu optimieren.

## <a name="customizing-reveal-focus"></a>Anpassen von Reveal Focus

Sie können den Reveal Focus-Effekt anpassen, indem Sie die Eigenschaften der visuellen Fokuselemente bei jedem Steuerelement ändern: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) und [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Mit diesen Eigenschaften können Sie die Farbe und Stärke des Fokusrechtecks anpassen. (Dies sind dieselben Eigenschaften, die Sie zum Erstellen von [visuellen Fokuselementen mit hoher Sichtbarkeit](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals) verwenden.) 

Bevor Sie aber mit dem Anpassen beginnen, sollten Sie etwas mehr zu den Komponenten von Reveal Focus erfahren.

Die standardmäßigen visuellen Reveal Focus-Elemente bestehen aus drei Teilen: dem primären Rahmen, dem sekundären Rahmen und dem Reveal-Leuchteffekt. Der primäre Rahmen ist **2 Pixel** breit und verläuft an der *Außenseite* des sekundären Rahmens. Der sekundäre Rahmen ist **1 Pixel** breit und verläuft an der *Innenseite* des primären Rahmens. Der Reveal Focus-Leuchteffekt hat eine Stärke, die proportional zur Stärke des primären Rahmens ist, und er verläuft um die *Außenseite* des primären Rahmens.

Zusätzlich zu den statischen Elementen bieten visuelle Reveal Focus-Elemente ein animiertes Licht, das bei Stillstand pulsiert und sich in Richtung des Fokus bewegt, wenn er verschoben wird.

![Ebenen von Reveal Focus](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Anpassen der Rahmenstärke

Verwenden Sie diese Eigenschaften, um die Stärke der Rahmentypen für ein Steuerelement zu ändern:

| Rahmentyp | Eigenschaft |
| --- | --- |
| Primärer, Leuchteffekt   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Durch Ändern des primären Rahmens wird die Stärke des Leuchteffekts proportional geändert.)   |
| Sekundärer   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


In diesem Beispiel wird die Rahmenstärke des visuellen Fokuselements für eine Schaltfläche geändert:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Anpassen des Rands

Der Rand ist der Abstand zwischen den visuellen Grenzen des Steuerelements und dem Beginn des sekundären Rahmens der visuellen Fokuselemente. Der Standardrand hat eine Breite von 1 Pixel außerhalb der Grenzen des Steuerelements. Sie können diesen Rand individuell pro Steuerelement bearbeiten, indem Sie die Eigenschaft [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) ändern:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Ein negativer Rand verschiebt den Rahmen weiter weg von der Mitte des Steuerelements, und ein positiver Rand verschiebt ihn näher zur Mitte des Steuerelements.

## <a name="customize-the-color"></a>Anpassen der Farbe

Um die Farbe des visuellen Elements von Reveal Focus zu ändern, verwenden Sie die Eigenschaften [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) und [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush).

| Eigenschaft | Standardressource | Standardwert der Ressource |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(Die Eigenschaft „FocusPrimaryBrush“ wird nur dann standardmäßig als **SystemControlRevealFocusVisualBrush**-Ressourcen verwendet, wenn **FocusVisualKind** auf **Reveal**  festgelegt ist. Andernfalls wird **SystemControlFocusVisualPrimaryBrush** verwendet.)


Um die Farbe des visuellen Fokuselements eines einzelnen Steuerelements zu ändern, legen Sie die Eigenschaften direkt im Steuerelement fest. In diesem Beispiel werden die Farben des visuellen Fokuselements einer Schaltfläche außer Kraft gesetzt.

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

Um die Farbe aller visuellen Fokuselemente in der gesamten App zu ändern, setzen Sie die Ressourcen **SystemControlRevealFocusVisualBrush** und **SystemControlFocusVisualSecondaryBrush** mit Ihrer eigenen Definition außer Kraft:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Weitere Informationen zum Ändern der Farbe von visuellen Fokuselementen finden Sie unter [Farbbranding und -anpassung](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Nur den Leuchteffekt anzeigen

Wenn Sie nur den Leuchteffekt ohne die primären oder sekundären visuellen Fokuselemente verwenden möchten, legen Sie die Eigenschaft [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) des Steuerelements einfach auf `Transparent` und die Eigenschaft [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) auf `0` fest. In diesem Fall übernimmt der Leuchteffekt die Farbe des Steuerelementhintergrunds für ein randloses Erscheinungsbild. Sie können die Stärke des Leuchteffekts mithilfe von [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness) anpassen.

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Verwenden Ihrer eigenen visuellen Fokuselemente

Eine weitere Möglichkeit zum Anpassen von Reveal Fokus ist das Deaktivieren der systemeigenen visuellen Fokuselemente, indem Sie Ihre eigenen Elemente mithilfe von visuellen Zuständen zeichnen. Weitere Informationen finden Sie unter [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Reveal Focus und das Fluent Design-System

Reveal Focus ist eine Komponente des Fluent Design-Systems, die Ihrer App Lichteffekte hinzufügt. Weitere Informationen zum Fluent Design-System und den zugehörigen Komponenten finden Sie unter [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Verwandte Artikel

- [Reveal Highlight](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Entwerfen für Xbox und Fernsehgeräte](/windows/uwp/design/devices/designing-for-tv)
- [Interaktionen mit Gamepad und Fernbedienung](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
- [Kompositionseffekte](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Wissenschaft im System: Fluent Design und Tiefe](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Wissenschaft im System: Fluent Design und Licht](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
