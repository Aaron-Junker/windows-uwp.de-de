---
Description: Ihre Kacheln und Popups können Zeichenfolgen und Bilder laden, die speziell auf die Sprache, den Skalierungsfaktor für die Anzeige, das Design, den hohen Kontrast und anderen Laufzeitkontexte angepasst wurden.
title: Unterstützte Kachel- und Popupbenachrichtigungen für Sprache, Skalierungsfaktor und hohen Kontrast
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: aa6e93196d30c15374129eee7714604cfab7b82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601475"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Unterstützte Kachel- und Popupbenachrichtigungen für Sprache, Skalierungsfaktor und hohen Kontrast

Ihre Kacheln und Popups können Zeichenfolgen und Bilder laden, die speziell auf die Sprache, den [Skalierungsfaktor für die Anzeige](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), das Design, den hohen Kontrast und anderen Laufzeitkontexte angepasst wurden. Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen der Ressourcendateien finden Sie unter [passen Sie Ihre Ressourcen für die Sprache, Skalierung und andere Kennzeichner](../../../app-resources/tailor-resources-lang-scale-contrast.md) und [-App-Symbole und Logos](/windows/uwp/design/style/app-icons-and-logos).

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Verweisen auf eine Zeichenfolgenressource aus einer Vorlage

In der Kachel- oder Popupbenachrichtigungsvorlage können Sie auf eine Ressource mithilfe des `ms-resource`-URI (Uniform Resource Identifier)-Schemas verweisen, gefolgt von einem einfachen Zeichenfolgenressourcen-Bezeichner. Wenn Sie beispielsweise eine Resources.resx-Datei haben, die einen Ressourceneintrag enthält, deren Name "Farewell" ist, erhalten Sie eine Zeichenfolgenressource mit dem Bezeichner "Farewell". Weitere Informationen zu Zeichenfolgenressourcen-Bezeichner und Ressourcendateien (.resw) finden Sie unter [Lokalisieren der Zeichenfolge im Paketmanifest der Benutzeroberfläche und der App](../../../app-resources/localize-strings-ui-manifest.md).

So würde ein Verweis auf den Zeichenfolgenressourcen-Bezeichner "Farewell" im [Text](/uwp/schemas/tiles/tilesschema/element-text?branch=live)körper des Vorlageninhalts mit `ms-resource` aussehen.

```xml
<text id="1">ms-resource:Farewell</text>
```

Wenn Sie das `ms-resource`-URI-Schema weglassen ist der Textkörper nur ein Zeichenfolgenliteral und *nicht* ein Verweis auf einen Bezeichner.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Verweisen auf eine Bildressource aus einer Vorlage

In der Kachel- oder Popupbenachrichtigungsvorlage können Sie auf eine Bildressource mithilfe des `ms-appx`-URI (Uniform Resource Identifier)-Schemas verweisen, gefolgt vom Namen der Bildressource. Dies entspricht der Art und Weise, in der Sie in XAML-Markup eine Bildressource referenzieren (weitere Informationen finden Sie unter [Referenzieren eines Bilds oder eines anderen Elements in XAML-Markup und Code](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Sie können z. B. Ordner wie folgt benennen.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

In diesem Fall erhalten Sie eine einzelne Bildressource und der Anzeigename (als absoluter Pfad) ist `/Assets/Images/welcome.png`. So verwenden Sie diesen Namen in Ihrer Vorlage.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Beachten Sie, wie auf die Beispielschema-URI ("`ms-appx`") "`://`" folgt, das einem absoluten Pfad folgt (ein absoluter Pfad beginnt mit "`/`").

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hosten und Laden von Bildern in der Cloud

Die `ms-resource` und `ms-appx`-URI-Schemen führen einen automatischen Qualifiziererabgleich aus, um die Ressource zu suchen, die für den aktuellen Kontext am besten geeignet ist. Web-URI-Schemen (z. B. `http`, `https`  und `ftp`) führen keinen derartigen automatischen Abgleich durch.

Fügen Sie stattdessen auf den URI des Bilds eine Abfragezeichenfolge hinzu, die den angeforderten Qualifiziererwert oder die Werte enthält.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

Implementieren Sie anschließend in den App-Dienst, der die Bilder enthält, einen HTTP-Handler, der die Abfragezeichenfolge untersucht und verwendet, um ermitteln, welches Bild zurückgegeben wird.

Auch muss ebenfalls das [**AddImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)-Attribut auf `true` in der [Kachel](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) oder in der [Popup](/uwp/schemas/tiles/toastschema/schema-root?branch=live)-Benachrichtung der XML-Nutzlast gesetzt werden. Das **addImageQuery**-Attribut erscheint in den `visual`, `binding` und `image`-Elementen des Kachel- und Popupschemas. Das explizite Festlegen von **AddImageQuery** für ein Element überschreibt einen beliebigen Wert für ein übergeordnetes Element. Der **addImageQuery**-Wert `true` in einem `image`-Element überschreibt den **addImageQuery**-Wert `false` im übergeordneten `binding`-Element.

Hier sind die Abfragezeichenfolgen, die Sie verwenden können.

| Qualifizierer | Abfragezeichenfolge | Beispiel |
| --------- | ------------ | ------- |
| Skalierung | ms-scale  | ?ms-scale=400  |
| Sprache | ms-lang  | ?ms-lang=en-US  |
| Vergleichen Sie | ms-contrast  | ?ms-contrast=high |

Eine Referenztabelle aller möglichen Qualifiziererwerte, die Sie in der Abfragezeichenfolgen verwenden können, finden Sie unter [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>Wichtige APIs

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Verwandte Themen

* [Bildschirmgrößen und Haltepunkte für reaktionsfähiges design](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Passen Sie Ihre Ressourcen für die Sprache, Skalierung und andere Kennzeichner](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Richtlinien für die Ressourcen für Kacheln und Symbole](app-assets.md)
* [Globalisierung und Lokalisierung](../../globalizing/globalizing-portal.md)
* [Lokalisieren von Zeichenfolgen in Ihre Benutzeroberfläche und app-Paketmanifest](../../../app-resources/localize-strings-ui-manifest.md)
* [Verweisen auf ein Bild oder eine andere Anlage aus XAML-Markup und code](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Kachel "-schema](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Toast-schema](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
