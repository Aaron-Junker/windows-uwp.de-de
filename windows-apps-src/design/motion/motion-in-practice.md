---
description: Erfahren Sie, wie Sie in Ihrer APP auf grundlegende Grundlagen wie zeitliche Steuerung, Beschleunigung, Direktionalität und Schwerkraft stoßen.
title: 'Bewegung in der Praxis: Animation in Windows-apps'
label: Motion in practice
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebc072a7b358aedfdd2320fa47f238712d7ee92
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220403"
---
# <a name="bringing-it-together"></a>Zusammenbringen

Zeitliche Steuerung, Beschleunigung, Direktionalität und Schwerkraft bilden die Grundlage für den fließenden Bewegungs Aufwand. Jede muss im Kontext der anderen berücksichtigt und entsprechend im Kontext ihrer App angewendet werden.

Im folgenden finden Sie drei Möglichkeiten, um die Grundlagen der Bewegung in Ihrer APP anzuwenden.

:::row:::
    :::column:::
**Implizite Animation** Automatische Tween und zeitliche Steuerung zwischen Werten in einem Parameter ändern sich, um eine sehr einfache fließende Bewegung mithilfe der standardisierten Werte zu erzielen.
    :::column-end:::
    :::column:::
**Integrierte Animation** System Komponenten, z. b. allgemeine Steuerelemente und freigegebene Bewegung, sind standardmäßig "fließend". Grundlagen wurden auf eine Weise angewendet, die mit ihrer impliziten Verwendung konsistent ist.
    :::column-end:::
    :::column:::
**Benutzerdefinierte Animation nach Anleitungen** Es kann vorkommen, dass das System noch keine exakte Bewegungs Lösung für Ihr Szenario bereitstellt. Verwenden Sie in diesen Fällen die grundlegenden Empfehlungen als Ausgangspunkt für Ihre Erfahrungen.
    :::column-end:::
:::row-end:::

**Übergangs Beispiel**

![funktionale Animation](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Vorwärtsrichtung:</b><br>
Ausblenden: 150 min. Beschleunigung: Standard beschleunigt <b>Vorwärtsrichtung in:</b><br>
Nach oben 150px: 300 ms Beschleunigung: Standard Verlangsamung
    :::column-end:::
    :::column:::
<b>Richtung rückwärts ausgehend:</b><br>
Nach unten 150px: 150 ms Beschleunigung: Standard beschleunigt die <b>Richtung rückwärts in:</b><br>
Ausblenden in: 300 ms; Beschleunigung: Standard Verlangsamung
    :::column-end:::
:::row-end:::

**Objekt Beispiel**

 ![300 ms Bewegung](images/control.gif)

:::row:::
    :::column:::
<b>Richtung erweitern:</b><br>
Vergrößern: 300 ms; Beschleunigung: Standard
    :::column-end:::
    :::column:::
<b>Richtung Vertrag:</b><br>
Vergrößern: 150 ms; Beschleunigung: Standard Beschleunigung
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Beispiele

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Wenn Sie die Katalog-App für <strong style="font-weight: semi-bold">XAML</strong> -Steuerelemente installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ImplicitTransition">die APP zu öffnen und die impliziten Übergänge in Aktion anzuzeigen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Implizite Animationen

> Implizite Animationen erfordern Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher.

Implizite Animationen sind eine einfache Möglichkeit, um eine fließende Bewegung zu erreichen, indem Sie während einer Parameter Änderung automatisch zwischen den alten und neuen Werten interpolieren.

Sie können Änderungen an den folgenden Eigenschaften implizit animieren:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **Drehung**
  - **Skalieren**
  - **Übersetzung**

- [Border](/uwp/api/windows.ui.xaml.controls.border)Rahmen, [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)oder [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **Hintergrund**

Jede Eigenschaft, für die Änderungen implizit animiert werden können, verfügt über eine entsprechende _Übergangs_ Eigenschaft. Um die Eigenschaft zu animieren, weisen Sie der entsprechenden _Übergangs_ Eigenschaft einen Übergangstyp zu. In dieser Tabelle werden die _Übergangs_ Eigenschaften und der zu verwendende Übergangstyp angezeigt.

| Animierte Eigenschaft | Übergangs Eigenschaft | Typ des impliziten Übergangs |
| -- | -- | -- |
| [UIElement. Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [Opacitytransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [Rotationtransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [Scaletransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [Translationtransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border. Background](/uwp/api/windows.ui.xaml.controls.border.background) | [Backgroundtransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter. Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [Backgroundtransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel. Hintergrund](/uwp/api/windows.ui.xaml.controls.panel.background) | [Backgroundtransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

In diesem Beispiel wird gezeigt, wie Sie mithilfe der Opacity-Eigenschaft und des Übergangs eine Schaltfläche ausblenden können, wenn das Steuerelement aktiviert und ausgeblendet wird, wenn es deaktiviert ist.

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

- [Übersicht über die Bewegung](index.md)
- [Timing und Beschleunigung](timing-and-easing.md)
- [Richtung und Schwerkraft](directionality-and-gravity.md)
