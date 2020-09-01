---
Description: Mit ihren Kacheln und-aufbildern können Zeichen folgen und Bilder geladen werden, die für die Anzeige Sprache, den Anzeige Skalierungsfaktor, den hohen Kontrast und andere Lauf Zeit Kontexte
title: Unterstützung für Kachel-und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 88bcd5d6ce59d0561e76f46f6291f58ad03ddf3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156734"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Unterstützung für Kachel-und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast

Mit ihren Kacheln und-aufbildern können Zeichen folgen und Bilder geladen werden, die für die Anzeige Sprache, den [Anzeige Skalierungsfaktor](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), den hohen Kontrast und andere Lauf Zeit Kontexte Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen der Ressourcen Dateien finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung und andere Qualifizierer](../../../app-resources/tailor-resources-lang-scale-contrast.md) und [App-Symbole und-Logos](../../style/app-icons-and-logos.md).

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Verweisen auf eine Zeichen folgen Ressource aus einer Vorlage

In ihrer Kachel oder Popup Vorlage können Sie auf eine Zeichen folgen Ressource mit dem `ms-resource` URI-Schema (Uniform Resource Identifier Schema), gefolgt von einem einfachen Zeichen folgen Ressourcen Bezeichner verweisen. Wenn Sie z. b. über eine Datei "Resources. resx" verfügen, die einen Ressourcen Eintrag mit dem Namen "Farewell" enthält, verfügen Sie über eine Zeichen folgen Ressource mit dem Bezeichner "Farewell". Weitere Informationen zu Zeichen folgen Ressourcen bezeichgern und Ressourcen Dateien (. resw) finden Sie unter Lokalisieren von Zeichen folgen [in der Benutzeroberfläche und im App-Paket Manifest](../../../app-resources/localize-strings-ui-manifest.md).

Auf diese Weise würde ein Verweis auf den "Farewell"-Zeichen folgen Ressourcen Bezeichner im [Text](/uwp/schemas/tiles/tilesschema/element-text?branch=live) Körper des Vorlagen Inhalts mithilfe von Aussehen `ms-resource` .

```xml
<text id="1">ms-resource:Farewell</text>
```

Wenn Sie das `ms-resource` URI-Schema weglassen, ist der Textkörper nur ein Zeichenfolgenliteralzeichen und *kein* Verweis auf einen Bezeichner.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Verweisen auf eine Bildressource aus einer Vorlage

In ihrer Kachel oder Popup Vorlage können Sie mithilfe des URI-Schemas (Uniform Resource Identifier) auf eine Bildressource verweisen, `ms-appx` gefolgt vom Namen der Bildressource. Dies ist dieselbe Art und Weise, wie Sie auf eine Bildressource in XAML-Markup verweisen (Weitere Informationen finden Sie unter [Verweis auf ein Bild oder ein anderes Objekt aus XAML-Markup und-Code](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Beispielsweise könnten Sie Ordner wie diese benennen.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

In diesem Fall verfügen Sie über eine einzelne Bildressource und deren Namen (als absoluter Pfad) `/Assets/Images/welcome.png` . Im folgenden wird erläutert, wie Sie diesen Namen in ihrer Vorlage verwenden.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Beachten Sie, dass in diesem Beispiel-URI dem Schema (" `ms-appx` ") `://` ein "" gefolgt von einem absoluten Pfad folgt (ein absoluter Pfad beginnt mit " `/` ").

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hosting und Laden von Images in der Cloud

Die `ms-resource` -und- `ms-appx` URI-Schemas führen automatische qualifiziererübereinstimmung aus, um die Ressource zu ermitteln, die für den aktuellen Kontext Web-URI-Schemas ( `http` z `https` . b., und `ftp` ) führen eine solche automatische Übereinstimmung nicht aus.

Fügen Sie stattdessen an den URI des Bilds eine Abfrage Zeichenfolge an, die den angeforderten qualifiziererwert oder die Werte beschreibt.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

Implementieren Sie dann in der APP Service-Anwendung, die Ihre Images bereitstellt, einen HTTP-Handler, der die Abfrage Zeichenfolge verwendet, um zu bestimmen, welches Bild zurückgegeben werden soll.

Außerdem müssen Sie das [**addimagequery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) -Attribut `true` in der [Kachel](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) -oder Popup [toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live) Benachrichtigungs-XML-Nutzlast auf festlegen. Das **addimagequery** -Attribut wird in `visual` den `binding` Elementen, und `image` der Kachel-und Popup Schemas angezeigt. Wenn **addimagequery** für ein Element explizit festgelegt wird, werden alle auf einem Vorgänger festgelegten Werte überschrieben. Beispielsweise überschreibt ein **addimagequery** -Wert `true` in einem- `image` Element einen **addimagequery** -Wert `false` in seinem übergeordneten `binding` Element.

Dies sind die Abfrage Zeichenfolgen, die Sie verwenden können.

| Qualifizierer | Abfragezeichenfolge | Beispiel |
| --------- | ------------ | ------- |
| Skalieren | MS-skalieren | ? MS-Scale = 400 |
| Language | ms-lang | ? ms-lang = en-US |
| Vergleichen Sie | MS-Kontrast | ? MS-Kontrast = hoch |

Eine Verweis Tabelle mit allen möglichen Qualifiziererwerten, die in den Abfrage Zeichenfolgen verwendet werden können, finden Sie unter [resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>Wichtige APIs

* [Resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Zugehörige Themen

* [Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Passen Sie Ihre Ressourcen für Sprache, Skalierung und andere Qualifizierer an.](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Richtlinien für Kachel-und Symbol Objekte](../../style/app-icons-and-logos.md).
* [Globalisierung und Lokalisierung](../../globalizing/globalizing-portal.md)
* [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](../../../app-resources/localize-strings-ui-manifest.md)
* [Verweisen auf ein Bild oder ein anderes Objekt aus XAML-Markup und Code](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addimagequery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Kachelschema](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Popupbenachrichtigungsschema](/uwp/schemas/tiles/toastschema/schema-root?branch=live)