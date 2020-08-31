---
title: Kompositions Beleuchtung
description: Die Kompositions Beleuchtungs-APIs können verwendet werden, um Ihrer Anwendung dynamische 3D-Beleuchtung hinzuzufügen.
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154704"
---
# <a name="using-lights-in-windows-ui"></a>Verwenden von Lichtern in der Windows

Die Windows. UI. Composition-APIs ermöglichen Ihnen das Erstellen von Echt Zeit Animationen und-Effekten. Die Kompositions Beleuchtung ermöglicht 3D-Beleuchtung in 2D-Anwendungen. In dieser Übersicht werden die Funktionen der Einrichtung von Kompositions Lichtern, das Identifizieren von visuellen Elementen zum Empfangen der einzelnen Licht und die Verwendung von Effekten zum Definieren von Material für Ihre Inhalte erläutert.

> [!NOTE]
> Informationen dazu, wie [xamllight](/uwp/api/windows.ui.xaml.media.xamllight) -Objekte [compositionlights](/uwp/api/Windows.UI.Composition.CompositionLight) anwenden, um XAML-UIElements zu beleuchten, finden Sie unter [XAML-Beleuchtung](xaml-lighting.md).

Mit der Kompositions Beleuchtung können Sie eine interessante Benutzeroberfläche erstellen.

- Transformation für ein Licht unabhängig von anderen Objekten in der Szene, um immersive Szenarios wie Musikwiedergabe Szenen zu ermöglichen.
- Die Möglichkeit, ein Objekt mit einem Licht zu koppeln, sodass Sie unabhängig vom Rest der Szene voneinander bewegt werden, um Szenarios wie [eine fließende hervor](../design/style/reveal.md) Hebung zu ermöglichen
- Transformation der hellen und ganzen Szene als Gruppe, um Material und Tiefe zu schaffen.

Die Kompositions Beleuchtung unterstützt drei Schlüsselkonzepte: **Light**, **Targets**und **scenelightingeffect**.

## <a name="light"></a>Hell

Mit [compositionlight](/uwp/api/windows.ui.composition.compositionlight) können Sie verschiedene Leuchten erstellen und in den Koordinaten Bereich platzieren. Diese Leuchten sind visuelle Elemente, die Sie als Beleuchtung identifizieren möchten.

### <a name="light-types"></a>Helle Typen

| Typ | BESCHREIBUNG |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Eine Lichtquelle, die nicht Direktionales Licht ausgibt, das von allen in der Szene reflektierten angezeigt wird. |
| [Distantlight](/uwp/api/windows.ui.composition.distantlight) | Eine unendlich große, entfernte Lichtquelle, die ein Licht in eine einzelne Richtung ausgibt. Wie die Sonne. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Eine Punktquelle des Lichts, das Licht in alle Richtungen ausgibt. Wie eine Glühbirne. |
| [Gerückt](/uwp/api/windows.ui.composition.spotlight) | Eine Lichtquelle, die innere und äußere Lichtkegel ausgibt. Wie eine Taschenlampe. |

## <a name="targets"></a>Ziele

Wenn Beleuchtung eine Visualisierung als Ziel hat (zu [Zielliste hinzu](/uwp/api/windows.ui.composition.compositionlight.targets) fügen), erkennen die visuellen Elemente und deren Nachfolger diese Lichtquelle und reagieren darauf. Dabei kann es sich um ein einfaches Festlegen einer PointLight-Quelle im Stammverzeichnis einer Struktur handeln, und alle visuellen Elemente werden auf die Animation der Punkt Beleuchtungs Richtung reagieren.

**Exclusionsfromtargets** bietet Ihnen die Möglichkeit, die Beleuchtung eines visuellen Elements oder einer Teilstruktur von visuellen Elementen auf ähnliche Weise wie das Hinzufügen von Zielen zu entfernen. Untergeordnete Elemente in der Struktur, die von dem ausgeschlossenen visuellen Element Stamm sind, werden nicht als Ergebnis beleuchtet.

### <a name="sample-targets"></a>Sample (Ziele)

Im folgenden Beispiel wird ein compositionpointlight-Objekt verwendet, um ein XAML-TextBlock als Ziel festzustellen.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Durch Hinzufügen von Animationen zum Offset des Punkt Lichts kann ein schillernder Effekt problemlos erreicht werden.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Weitere Informationen finden Sie im Beispiel zum vollständigen [textshimmer](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) unter windowuidevlabs-Beispiel-galley.

## <a name="restrictions"></a>Beschränkungen

Es gibt mehrere Faktoren, die Sie berücksichtigen müssen, wenn Sie bestimmen, welche Inhalte durch compositionlight beleuchtet werden.

Konzept | Details
--- | ---
**Umgebungslicht** | Wenn Sie Ihrer Szene ein nicht Ambient-Licht hinzufügen, wird das gesamte vorhandene Licht ausgeschaltet.  Elemente, die nicht von einem nicht-Ambient-Licht betroffen sind, werden schwarz angezeigt.  Verwenden Sie zum beleuchten der umgebenden visuellen Elemente, für die das Licht nicht auf natürliche Weise bestimmt ist, ein Umgebungslicht in Verbindung mit anderen Lichtern.
**Anzahl von Lichtern** | Sie können zwei nicht Ambient-Kompositions Leuchten in einer beliebigen Kombination verwenden, um Ihre Benutzeroberfläche als Ziel zu verwenden. Umgebungs Leuchten sind nicht eingeschränkt. Spot-, Punkt-und entfernte Beleuchtung sind.
**Gültigkeitsdauer** | In compositionlight treten möglicherweise Lebensdauer Bedingungen auf (Beispiel: der Garbage Collector kann das helle Objekt wieder verwenden, bevor es verwendet wird).  Wir empfehlen Ihnen, einen Verweis auf Ihre Beleuchtung beizubehalten, indem Sie Beleuchtung als Member hinzufügen, um die Lebensdauer der Anwendung zu unterstützen.
**Transformationen** | Beleuchtung muss in einem Knoten oberhalb der Benutzeroberfläche platziert werden, der bewirkt, dass Effekte wie [Perspektiven Transformationen](../design/layout/3-d-perspective-effects.md) in der visuellen Struktur ordnungsgemäß gezeichnet werden.
**Ziele und Koordinatenraum** | CoordinateSpace ist der visuelle Bereich, in dem alle Lichter Eigenschaften festgelegt werden müssen. Compositionlight. targets muss sich innerhalb der CoordinateSpace-Struktur befinden.

## <a name="lighting-properties"></a>Beleuchtungs Eigenschaften

Abhängig vom Typ des verwendeten Lichts kann ein Lichteigenschaften für Dämpfung und Leerraum aufweisen. Nicht alle Arten von Lichtern verwenden alle Eigenschaften.

Eigenschaft | BESCHREIBUNG
--- | ---
**Farbe** | Die [Farbe](/uwp/api/windows.ui.color) des Lichts. Beleuchtungs Farbwerte werden durch [D3D](../graphics-concepts/light-properties.md) diffuses, Ambient und specfeiner definiert, das die ausgegebene Farbe definiert. Beleuchtung verwendet RGBA-Werte für Leuchten. die Alpha Farbkomponente wird nicht verwendet.
**Richtung** | Die Richtung des Lichts. Die Richtung, in der das Licht zeigt, wird relativ zum zugehörigen [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) -visuellen Element angegeben.
**Koordinaten Bereich** | Jedes visuelle Element verfügt über einen impliziten 3D-Koordinaten Bereich. Die X-Richtung ist von links nach rechts. Die Y-Richtung ist von oben nach unten. Die Z-Richtung ist ein Punkt außerhalb der Ebene. Der ursprüngliche Punkt dieser Koordinate ist die linke obere Ecke des visuellen Elements, und die Einheit ist geräteunabhängige Pixel (DIP). Der Offset eines Lichts, der in dieser Koordinate definiert ist.
**Innere und äußere Kegel** | Die Scheinwerfer geben einen Lichtkegel aus, der aus zwei Teilen besteht: einem hellen inneren Kegel und einem äußeren Kegel. Die Komposition ermöglicht Ihnen die Steuerung von inneren und äußeren Kegel Winkeln und-Farben.
**Offset** | Offset der Lichtquelle relativ zum visuellen Koordinatenraum Element.

> [!NOTE]
> Wenn mehrere Lichter dasselbe visuelle Element erreichen, oder wenn der Farbwert eines Lichts groß genug ist, um 1,0 zu überschreiten, kann sich die Farbe des Lichts aufgrund der Klammer eines Beleuchtungs Kanals ändern.

### <a name="advanced-lighting-properties"></a>Erweiterte Beleuchtungs Eigenschaften

Eigenschaft | BESCHREIBUNG
--- | ---
**Intensität** | Steuert die Helligkeit des Lichts.
**Dämpfung** | Durch die Dämpfung wird gesteuert, wie sich die Intensität eines Lichts auf den maximalen Abstand verringert, der in der Range-Eigenschaft angegeben ist.  Die Eigenschaften "Constant", "quadradic" und "lineare Dämpfung" können verwendet werden.

## <a name="getting-started-with-lighting"></a>Einstieg in die Beleuchtung

Führen Sie diese allgemeinen Schritte aus, um Beleuchtung hinzuzufügen:

- Erstellen und Platzieren der Beleuchtung: Erstellen Sie Beleuchtung, und platzieren Sie Sie in einem angegebenen Koordinaten Bereich.
- Identifizieren Sie die zu hell Ende Objekte: zielhell in relevanten visuellen Elementen.
- Optionale Hiermit wird definiert, wie einzelne Objekte auf Beleuchtung reagieren: Verwenden Sie scenelightingeffect mit einem effectbrush, um die helle Reflektion für die Anzeige von spritevisual anzupassen. Reflection-Standardwerte unterstützen die Beleuchtung von untergeordneten Elementen des CoordinateSpace einer Lichtquelle.  Eine mit einem scenelightingeffect gezeichnete Visualisierung überschreibt die Standardbeleuchtung für dieses visuelle Element.

## <a name="scenelightingeffect"></a>Scenelightingeffect

[Scenelightingeffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) wird verwendet, um die Standardbeleuchtung zu ändern, die auf den Inhalt eines [spritevisual](/uwp/api/Windows.UI.Composition.SpriteVisual) angewendet wird, das auf einen [compositionlight](/uwp/api/windows.ui.composition.compositionlight)ausgerichtet ist.

[Scenelightingeffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) wird häufig für die Erstellung von Material verwendet. Eine scenelightingeffect ist ein Effekt, der verwendet wird, wenn Sie etwas komplexeres erreichen möchten, z. b. das Aktivieren von reflektierenden Eigenschaften für ein Bild und/oder das Bereitstellen einer Illusion von Tiefe mit einer normalen Karte. Mit einem scenelightingeffect können Sie die Benutzeroberfläche anpassen, indem Sie die Beleuchtungs Eigenschaften wie Glanz-und diffuses Beträge verwenden. Sie können die Beleuchtungseffekte mit dem Rest der Effekt Pipeline weiter anpassen, sodass Sie unterschiedliche Beleuchtungs Reaktionen mit ihren Inhalten kombinieren und zusammenstellen können.

> [!NOTE]
> Szenen Beleuchtung erzeugt keine Schatten der Fokus liegt auf dem 2D-Rendering.  Es berücksichtigt keine 3D-Beleuchtungs Szenarios, die echte Beleuchtungs Modelle einschließlich Schatten enthalten.


Eigenschaft | BESCHREIBUNG
--- | ---
**Normale Karte** | Normalmaps erzeugen eine Auswirkung auf eine Textur, bei der eine normale Richtung auf das Licht heller ist, und eine normale zeige, dass Sie aussteht. Verwenden Sie zum Hinzufügen einer normalmap zu Ihrer Ziel Visualisierung einen [compositionsurfacebrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) mithilfe von loadedimagesurface, um ein normalmap-Medienobjekt zu laden.
**Umgebend** | Ambient-Eigenschaften werden größtenteils verwendet, um die gesamte Farb Reflektion zu steuern.
**Glänzend** | Die Glanz Reflektion erstellt Hervorhebungen für Objekte, sodass Sie glänzend erscheinen. Sie können die Ebene der Glanz Reflektion und die Ebene des Glanz Steuer Elements steuern.  Diese Eigenschaften werden manipuliert, um Materialeffekte wie z. b. "shinny Metals" oder "Hochglanzpapier"
**Diffus** | Diffused-Reflektion streut das Licht in alle Richtungen.
**Reflectance-Modell** | Das [Reflectance-Modell](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) ermöglicht es Ihnen, zwischen [Blink](/visualstudio/designers/how-to-create-a-basic-phong-shader) enden und physisch basierenden Blinn Phong auszuwählen.  Wenn Sie komprimierte Glanzlichter haben möchten, wählen Sie physisch basierende Blinn Phong aus.

### <a name="sample-scenelightingeffect"></a>Sample (scenelightingeffect)

Das folgende Beispiel zeigt, wie einem scenelightingeffect eine normale Karte hinzugefügt wird.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Erstellen von Materialien und Lichtern in der visuellen Ebene](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Übersicht über die Beleuchtung](../graphics-concepts/lighting-overview.md)
- [Compositionfunktionen-API](/uwp/api/windows.ui.composition.compositioncapabilities)
- [Mathematik der Beleuchtung](../graphics-concepts/mathematics-of-lighting.md)
- [Scenelightingeffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [GitHub-Repository windowsuidevlabs](https://github.com/microsoft/WindowsCompositionSamples)