---
Description: Erfahren Sie, wie Fluent Motion-Grundlagen in Ihrer app zusammenkommen.
title: Bewegung in der Praxis – Animationen in UWP-Apps
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bc879e43d7e117a4e61e8b6de4bb5437d126e84b
ms.sourcegitcommit: 7effecb544952b493250337fc622848232fa5995
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67325855"
---
# <a name="bringing-it-together"></a>Alles zusammenführen

Timing, Geschwindigkeitsverlauf, Direktionalität und Schwerkraft bilden zusammen die Grundlage für fließende Bewegungen. Jede Komponente muss im Zusammenhang mit den anderen betrachtet und dem Kontext Ihrer App entsprechend angewendet werden.

Im Folgenden finden Sie 3 Möglichkeiten, die Grundlagen für fließende Bewegung in Ihrer App anzuwenden.

:::row:::
    :::column:::
        **Implicit animation**
        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**
        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**
        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

**Transition-Beispiel**

![funktionelle Animation](images/pageRefresh.gif)

:::row:::
    :::column:::
        <b>Direction Forward Out:</b><br>
        Fade out: 150m; Easing: Default Accelerate
        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate
        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

**Beispiel für ein Wertobjekt**

 ![300 ms Bewegung](images/control.gif)

:::row:::
    :::column:::
        <b>Direction Expand:</b><br>
        Grow: 300ms; Easing: Standard
    :::column-end:::
    :::column:::
        <b>Direction Contract:</b><br>
        Grow: 150ms; Easing: Default Accelerate
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie haben die <strong style="font-weight: semi-bold">XAML-Steuerelementsammlungen</strong> app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ImplicitTransition">öffnen Sie die app und implizite Übergänge in Aktion</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Implizite Animationen

> Implizite Animationen müssen Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher.

Implizite Animationen sind, eine einfache Möglichkeit, Fluent während der Übertragung zu erzielen, indem Sie automatisch interpolieren zwischen den alten und neuen Werte während der Parameter geändert wird.

Sie können implizit animieren, Änderungen an den folgenden Eigenschaften:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **Drehung**
  - **Skalieren**
  - **Translation**

- [Rahmen](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter), oder [Bereich](/uwp/api/windows.ui.xaml.controls.panel)
  - **Hintergrund**

Jede Eigenschaft, die Änderungen, die implizit animiert haben, kann verfügt über eine entsprechende _Übergang_ Eigenschaft. Zum Animieren der Eigenschaft, weisen Sie ein Übergang in das entsprechende _Übergang_ Eigenschaft. In dieser Tabelle sind die _Übergang_ Eigenschaften und den Übergangstyp für jede einzelne verwenden.

| Animierte Eigenschaft | Transition-Eigenschaft | Implizite Übergangstyp |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Dieses Beispiel zeigt, wie Sie mit die Opacity-Eigenschaft und der Übergang auf eine Schaltfläche eingeblendet, wenn das Steuerelement aktiviert ist und ausblenden, wenn sie deaktiviert ist.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Motion-Übersicht](index.md)
- [Zeitsteuerungssystem und vereinfachen der](timing-and-easing.md)
- [Direktionalität und die Schwerkraft](directionality-and-gravity.md)
