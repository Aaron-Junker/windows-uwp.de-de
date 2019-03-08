---
title: Komposition Beleuchtung
description: Die Komposition Beleuchtung-APIs können zum Hinzufügen von dynamischen 3D Beleuchtung Ihrer Anwendung verwendet werden.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 733ce75942a05482ade88c1510e788f1cbd515d4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602205"
---
# <a name="using-lights-in-windows-ui"></a>Mithilfe der Beleuchtung in Windows-Benutzeroberfläche

Die Windows.UI.Composition-APIs ermöglichen Ihnen die Erstellung von Echtzeit-Animation und Effekte. Komposition Beleuchtung kann 3D Beleuchtung in 2D-Anwendungen. In diesem Abschnitt führen wir über die Funktionalität zum Komposition Lichter einrichten, identifizieren visuelle Elemente, um jede LED zu erhalten und verwenden Effekte Materialien für Ihre Inhalte zu definieren.

> [!NOTE]
> Wie Lesen [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) Objekte gelten [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) um XAML-UIElements beleuchten, finden Sie unter [XAML Beleuchtung](xaml-lighting.md).

Komposition Beleuchtung können Sie die Benutzeroberfläche-Konfigurationsrichtlinien interessant zu erstellen:

- Die Transformation von einem hellen unabhängig von anderen Objekten in der Szene immersive Szenarien wie Musik Wiedergabe im Hintergrund zu aktivieren.
- Die Möglichkeit, ein Objekt mit einem Licht zu koppeln, damit sie zusammen verschoben unabhängig vom Rest der Szene-Szenarien wie Fluent [Offenlegen](/windows/uwp/design/style/reveal) markieren.
- Die Transformation die Lichtgeschwindigkeit und die gesamte Szene als Gruppe auf die Materialien und Tiefe zu erstellen.

Komposition Beleuchtung unterstützt drei Hauptkonzepte: **Light**, **Ziele**, und **SceneLightingEffect**.

## <a name="light"></a>Hell

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) ermöglicht Ihnen das Erstellen der verschiedenen Lichter und platzieren Sie diese im Koordinatenraum. Diese LEDs Ziel Visuals, die erkennen, wie vom Licht beleuchtet werden sollen.

### <a name="light-types"></a>Einfache Typen

| Typ | Beschreibung |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Eine Lichtquelle, die nicht direktionale Licht ausgibt, die angezeigt wird, die von alles, was in der Szene reflektiert werden. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Eine unendlich große Lichtquelle, die Licht in eine Richtung ausgibt. Wie die Sonne. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Eine Punkt-Quelle von Licht, das Licht in alle Richtungen ausgibt. Wie eine Glühbirne. |
| [SpotLight](/uwp/api/windows.ui.composition.spotlight) | Einer Lichtquelle, die inneren und äußeren Kegel des Lichts ausgibt. Wie eine Taschenlampe. |

## <a name="targets"></a>Ziele

Wenn das Licht ein visuelles Ziel (hinzufügen [Ziele](/uwp/api/windows.ui.composition.compositionlight.targets) Liste), das visuelle Element und all seine Nachfolger erkennen und reagieren auf diese Lichtquelle. Dies kann einfach sein, z.B. eine Einstellung, die Quelle PointLight am Stamm einer Struktur und alle visuellen Elemente, die folgenden reagieren auf die Animation von der Richtung der Punkt Lichter sein.

**ExclusionsFromTargets** bietet Ihnen die Möglichkeit, die Beleuchtung eines visuellen Objekts oder einer Unterstruktur von visuellen Elementen auf ähnliche Weise wie das Hinzufügen von Zielen zu entfernen. Untergeordnete Elemente in der Struktur Rooting manipuliert wurde, indem Sie das visuelle Element, das ausgeschlossen wurde daher nicht leuchtet.

### <a name="sample-targets"></a>Beispiel (Ziele)

Im folgenden Beispiel verwenden wir eine CompositionPointLight ein XAML-TextBlock-Element als Ziel.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

Der Offset des der Punktuelles Licht Animation hinzufügen, ist eine schimmernde Auswirkung ganz einfach.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Die vollständigen [Text Schimmer](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) unter die Küche WindowUIDevLabs Beispiel, um mehr zu erfahren.

## <a name="restrictions"></a>Einschränkungen

Es gibt mehrere Faktoren zu berücksichtigen, welche Inhalte bestimmen von CompositionLight leuchtet.

Konzept | Details
--- | ---
**Umgebungslicht** | Ein nicht Umgebend Licht zu Ihrer Szene hinzufügen werden alle vorhandenen Licht deaktivieren.  Elemente, die nicht von einer nicht-Umgebungslicht betroffen werden schwarz angezeigt.  Verwenden Sie zum umgebenden Visuals, die für die das Licht auf natürliche Weise zu beleuchten, ein Umgebungslicht in Verbindung mit anderen Lichter.
**Anzahl der Beleuchtung** | Sie können keine zwei nicht Umgebend Komposition Lichter in beliebiger Kombination verwenden, die Benutzeroberfläche als Ziel. Ambient Lichter sind nicht eingeschränkt. erkennen, sind Point und entfernten Beleuchtung.
**Lifetime** | CompositionLight wird möglicherweise eine Lebensdauer Bedingungen (Beispiel: der Garbage Collector möglicherweise die light-Objekt wiederverwendet, bevor sie verwendet wird).  Es wird empfohlen, einen Verweis auf Ihrer Beleuchtung beibehält, durch das Hinzufügen von Licht als Mitglied die Anwendung, die Lebensdauer verwalten können.
**Transformationen** | Lichter platziert werden müssen, in einem Knoten über die Benutzeroberfläche, die Effekte wie verwendet [Perspektive Transformationen](/windows/uwp/design/layout/3-d-perspective-effects) in Ihrer visuellen Struktur ordnungsgemäß gezeichnet werden soll.
**Ziele und Koordinatenbereich** | CoordinateSpace ist der visuellen Bereich, in dem alle die Lichter-Eigenschaften festgelegt werden müssen. CompositionLight.Targets muss innerhalb der Struktur CoordinateSpace sein.

## <a name="lighting-properties"></a>Beleuchtungseigenschaften

Je nach Art des Lichts verwendet kann ein Licht-Eigenschaften Attenuation und Speicherplatz haben. Nicht alle Lichtarten verwenden alle Eigenschaften.

Eigenschaft | Beschreibung
--- | ---
**Farbe** | Die [Farbe](/uwp/api/windows.ui.color) des Lichts. Beleuchtung RGB-Werte werden durch definiert [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) "Diffus" Ambient und Glanz, die definiert, die Farbe, die ausgegeben wird. Beleuchtung RGBA-Werte, die für die Beleuchtung verwendet wird; die Alphafarbe-Komponente wird nicht verwendet.
**Richtung** | Die Richtung des Lichts. Die Richtung, in dem das Licht zeigt, angegeben ist, relativ zu dessen [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual.
**Koordinatenbereich** | Jedes Visual verfügt über eine implizite 3D-Koordinatensystem. X-Richtung ist von links nach rechts. Y-Richtung wird von oben nach unten. Z-Richtung ist eine aus der Ebene. Der ursprüngliche Punkt der dieser Koordinate ist die linke obere Ecke der Visualisierung aus, und Geräte geräteunabhängigen Pixel (DIP) als Maßeinheit verwendet werden. In dieser Koordinate definierten des Lichts-Offset.
**Innere und äußere Kegel** | Spotlights strahlen einen zweitteiligen Lichtkegel ab: einen hellen inneren Kegel und einen äußeren Kegel. Komposition ermöglicht, dass Sie die Kontrolle über die inneren und äußeren Kegelwinkel und Farbe.
**Offset** | Der Offset von der Lichtquelle relativ zu dessen Koordinatenbereich Visual.

> [!NOTE]
> Wenn mehrere Lichter dasselbe visuelle erreicht oder wenn Farbwert des Lichts groß genug, um 1.0 überschreiten erhält, kann die Farbe des Lichts aufgrund eines Kanals Farbe Lichter clamping ändern.

### <a name="advanced-lighting-properties"></a>Erweiterte Eigenschaften Beleuchtung

Eigenschaft | Beschreibung
--- | ---
**Intensität** | Steuert die Helligkeit des Lichts.
**Attenuation** | Die Dämpfung steuert, wie die Intensität eines Lichts gegenüber der maximalen Entfernung, angegeben durch die Eigenschaft „Reichweite“, abnimmt.  Konstanten, können Quadradic und lineare Attenuation-Eigenschaften verwendet werden.

## <a name="getting-started-with-lighting"></a>Erste Schritte mit Beleuchtung

Führen Sie die folgenden allgemeinen Schritte aus, um Lichter hinzuzufügen:

- Erstellen und Lichter platzieren: Erstellen Sie Beleuchtung, und fügen Sie sie in einem angegebenen Koordinatenraum.
- Identifizieren Sie Objekte als Licht: Kurz vor relevanten visuellen Elemente als Ziel verwenden.
- [Optional] Definieren, wie einzelne Objekte Lichter reagieren: Verwenden Sie SceneLightingEffect mit einer EffectBrush, um einfache Reflektion zum Anzeigen der SpriteVisual anzupassen. Reflektion Standardwerte unterstützen die Beleuchtung der untergeordneten Elemente von einer Lichtquelle CoordinateSpace.  Eine Visualisierung, die mit einem SceneLightingEffect gezeichnet überschreibt die standardmäßige Beleuchtung für diese Visualisierung.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) dient zum Ändern der Standard-Beleuchtung in den Inhalt des eine [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) Ziel eine [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) wird häufig für die Erstellung von Material verwendet. Eine SceneLightingEffect kommt zu einer Auswirkung verwendet, wenn Sie etwas komplexer, z. B. das Aktivieren der reflektierenden Eigenschaften in einem Bild bzw. eine Illusion von Tiefe mit einer Normalmap bereitstellen erreichen möchten. Eine SceneLightingEffect bietet die Möglichkeit zum Anpassen Ihrer Benutzeroberflächenautomatisierungs mithilfe der Beleuchtungseigenschaften wie glänzende und diffusen Mengen. Sie können weiter anpassen, dass Beleuchtungseffekten mit dem Rest der Pipeline Effekte können Sie einzeln in blend und kombinieren Sie verschiedene Beleuchtung Reaktionen mit Ihren Inhalten.

> [!NOTE]
> Szene Beleuchtung ergibt keinen Shadows; Es ist mit dem Schwerpunkt 2D-Rendering Auswirkungen.  Es dauert nicht berücksichtigt 3D Beleuchtung Szenarios, die echte beleuchtungsmodelle, einschließlich der Schatten enthalten.


Eigenschaft | Beschreibung
--- | ---
**Normalmap** | NormalMaps erstellen Sie einen Effekt einer Textur, in denen eine normale auf das Licht hellere, und ein normaler zeigt sofort wird beleuchtet. Hinzufügen einer NormalMap für die Verwendung der entsprechenden visual eine ["compositionsurfacebrush"](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) LoadedImageSurface zum Laden einer NormalMap-Asset verwenden.
**Ambient** | Umgebungseigenschaften werden hauptsächlich verwendet, um die gesamte Farbe Reflektion zu steuern.
**Specular** | Glänzende Reflektion erstellt Highlights für Objekte, die somit shiny angezeigt werden. Sie können die Ebene der glänzenden Reflektion sowie die Ebene der Glanz steuern.  Diese Eigenschaften bearbeitet werden, um wesentliche Auswirkungen wie Shinny Metalle oder glänzendes Papier zu erstellen.
**Diffuse** | Diffuses Reflektion Streut Licht in alle Richtungen.
**Das Reflexionsvermögen-Modell** | [Das Reflexionsvermögen Modell](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) können Sie auswählen zwischen [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) und physisch basierten Blinn Phong.  Wählen Sie physisch basierten Blinn Phong wenn Glanzlichter komprimiert haben sollen.

### <a name="sample-scenelightingeffect"></a>Beispiel (SceneLightingEffect)

Das folgende Beispiel zeigt, wie eine SceneLightingEffect eine normalen Zuordnung hinzugefügt wird.

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

- [Erstellen von Materialien und Licht in der visuellen Ebene](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Übersicht über die Beleuchtung](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Mathematik der Beleuchtung](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub Repo](https://github.com/Microsoft/WindowsUIDevLabs)
