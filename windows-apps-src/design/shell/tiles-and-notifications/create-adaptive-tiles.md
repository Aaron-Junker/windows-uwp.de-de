---
Description: Vorlagen für adaptive Kacheln sind ein neues Feature in Windows 10 und ermöglichen den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache, die sich an unterschiedliche Bildschirmdichten anpasst.
title: Erstellen adaptiver Kacheln
ms.assetid: 1246B58E-D6E3-48C7-AD7F-475D113600F9
label: Create adaptive tiles
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7cee236b385b6129e7ab1a9cacd549f217f6e734
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175604"
---
# <a name="create-adaptive-tiles"></a>Erstellen adaptiver Kacheln

Vorlagen für adaptive Kacheln sind ein neues Feature in Windows 10 und ermöglichen den Entwurf eigener Inhalte für Kachelbenachrichtigungen mithilfe einer einfachen, flexiblen Markupsprache, die sich an unterschiedliche Bildschirmdichten anpasst. Dieser Artikel beschreibt, wie Sie adaptive Live-Kacheln für Ihre App für Ihre Windows-App erstellen. Die vollständige Liste adaptiver Elemente und Attribute finden Sie unter [Adaptives Kachelschema](../tiles-and-notifications/tile-schema.md).

(Wenn gewünscht, können Sie weiterhin die voreingestellten Vorlagen aus dem [Windows 8-Kachelvorlagenkatalog](/previous-versions/windows/apps/hh761491(v=win.10)) beim Entwerfen von Benachrichtigungen für Windows 10 verwenden.)


## <a name="getting-started"></a>Erste Schritte

**Installieren Sie die Benachrichtigungsbibliothek.** Wenn Sie C# anstelle von XML verwenden möchten, um Benachrichtigungen zu generieren, installieren Sie das NuGet-Paket mit dem Namen [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). (Suchen Sie nach „Benachrichtigungen UWP“.) Die in diesem Artikel bereitgestellten C#-Beispiele verwenden Version 1.0.0 des NuGet-Pakets.

**Installieren Sie den Notifications Visualizer.** Diese kostenlose Windows-App unterstützt Sie beim Entwerfen von adaptiven Live Kacheln, indem Sie eine schnelle visuelle Vorschau Ihrer Kachel bereitstellen, während Sie Sie bearbeiten, ähnlich wie die XAML-Editor-/Entwurfs Ansicht von Visual Studio Weitere Informationen finden Sie unter [Benachrichtigungs](notifications-visualizer.md) Schnellansicht, oder [Laden Sie die Benachrichtigungs Schnellansicht aus dem Store herunter](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="how-to-send-a-tile-notification"></a>Senden einer Kachelbenachrichtigung

Lesen Sie den [Schnellstart zum Senden von lokalen Kachelbenachrichtigungen](sending-a-local-tile-notification.md). In der nachfolgenden Dokumentation werden alle Möglichkeiten der visuellen Benutzeroberfläche mit adaptiven Kacheln erläutert.


## <a name="usage-guidance"></a>Informationen zur Verwendung


Adaptive Vorlagen werden für die Unterstützung verschiedener Formfaktoren und Benachrichtigungstypen konzipiert. Elemente wie Gruppen und Untergruppen verknüpfen Inhalte miteinander und implizieren selbst kein bestimmtes visuelles Verhalten. Die endgültige Darstellung einer Benachrichtigung sollte auf dem spezifischen Gerät basieren, auf dem sie angezeigt wird, beispielsweise einem Smartphone, Tablet, Desktop oder anderen Gerät.

Hinweise sind optionale Attribute, die Elementen hinzugefügt werden können, um ein bestimmtes visuelles Verhalten zu erzielen. Hinweise können spezifisch für Geräte oder Benachrichtigungen sein.

## <a name="a-basic-example"></a>Ein einfaches Beispiel


Dieses Beispiel veranschaulicht, was Vorlagen für adaptive Kacheln leisten können.

```xml
<tile>
  <visual>
  
    <binding template="TileMedium">
      ...
    </binding>
  
    <binding template="TileWide">
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </binding>
  
    <binding template="TileLarge">
      ...
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = ...
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Jennifer Parker",
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Photos from our trip",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
  
                    new AdaptiveText()
                    {
                        Text = "Check out these awesome photos I took while in New Zealand!",
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },
  
        TileLarge = ...
    }
};
```

**Ergebnis:**

![Kurzes Beispiel für eine Kachel](images/adaptive-tiles-quicksample.png)

## <a name="tile-sizes"></a>Kachelgrößen


Der Inhalt für jede Kachel Größe wird einzeln in separaten [tilebinding](../tiles-and-notifications/tile-schema.md#tilebinding) -Elementen innerhalb der XML-Nutzlast angegeben. Wählen Sie die Zielgröße aus, indem Sie das template-Attribut auf einen der folgenden Werte festlegen:

-   TileSmall
-   TileMedium
-   TileWide
-   TileLarge (nur für Desktop)

Geben Sie für die XML-Nutzlast einer einzelnen Kachelbenachrichtigung &lt;binding&gt;-Elemente für jede zu unterstützende Kachelgröße an, wie im folgenden Beispiel dargestellt:

```xml
<tile>
  <visual>
  
    <binding template="TileSmall">
      <text>Small</text>
    </binding>
  
    <binding template="TileMedium">
      <text>Medium</text>
    </binding>
  
    <binding template="TileWide">
      <text>Wide</text>
    </binding>
  
    <binding template="TileLarge">
      <text>Large</text>
    </binding>
  
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileSmall = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Small" }
                }
            }
        },
  
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Medium" }
                }
            }
        },
  
        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Wide" }
                }
            }
        },
  
        TileLarge = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText() { Text = "Large" }
                }
            }
        }
    }
};
```

**Ergebnis:**

![Größen für adaptive Kacheln: klein, mittel, breit und groß](images/adaptive-tiles-sizes.png)

## <a name="branding"></a>Branding


Sie können das Branding am unteren Rand einer Live-Kachel (den Anzeigenamen und das Cornerlogo) mit dem branding-Attribut in der Benachrichtigungsnutzlast steuern. Mit „none“ wird nichts angezeigt, mit „name“ nur der Name, mit „logo“ nur das Logo, und mit „nameAndLogo“ werden Name und Logo angezeigt.

**Hinweis**    Windows Mobile unterstützt das ecklogo nicht, sodass "Logo" und "nameandlogo" standardmäßig "Name" auf Mobilgeräten sind.

 

```xml
<visual branding="logo">
  ...
</visual>
```

```csharp
new TileVisual()
{
    Branding = TileBranding.Logo,
    ...
}
```

**Ergebnis:**

![Adaptive Kacheln, Name und Logo](images/adaptive-tiles-namelogo.png)

Das Branding kann für bestimmte Kachelgrößen auf zwei Weisen angewendet werden:

1. Durch Anwenden des-Attributs auf das [tilebinding](../tiles-and-notifications/tile-schema.md#tilebinding) -Element
2. Durch Anwenden des-Attributs auf das [tilevisual](../tiles-and-notifications/tile-schema.md#tilevisual) -Element, das sich auf die gesamte Benachrichtigungs Nutzlast auswirkt, wenn Sie kein Branding für eine Bindung angeben, wird das Branding verwendet, das im visuellen Element bereitgestellt wird.

```xml
<tile>
  <visual branding="nameAndLogo">
 
    <binding template="TileMedium" branding="logo">
      ...
    </binding>
 
    <!--Inherits branding from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,

        TileMedium = new TileBinding()
        {
            Branding = TileBranding.Logo,
            ...
        },

        // Inherits branding from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Ergebnis bei Standardbranding:**

![Standardbranding für Kacheln](images/adaptive-tiles-defaultbranding.png)

Wenn Sie in der Benachrichtigungsnutzlast kein Branding angeben, wird das Branding durch die Eigenschaften der Basiskachel bestimmt. Wenn auf der Basiskachel der Anzeigename dargestellt ist, wird für das Branding standardmäßig „name“ verwendet. Wenn kein Anzeigename vorhanden ist, wird für das Branding standardmäßig „none“ verwendet.

**Hinweis**    Dies ist eine Änderung gegenüber Windows 8. x, bei der das Standard Branding "Logo" war.

 

## <a name="display-name"></a>Anzeigename


Sie können den Anzeigenamen einer Benachrichtigung überschreiben, indem Sie für das **displayName**-Attribut die gewünschte Textzeichenfolge eingeben. Wie beim Branding können Sie dies für das [tilevisual](../tiles-and-notifications/tile-schema.md#tilevisual) -Element angeben, das sich auf die gesamte Benachrichtigungs Nutzlast auswirkt, oder auf das [tilebinding](../tiles-and-notifications/tile-schema.md#tilebinding) -Element, das sich nur auf einzelne Kacheln auswirkt.

**Bekanntes Problem**  Wenn Sie unter Windows Mobile „ShortName“ für die Kachel angeben, wird der in der Benachrichtigung angegebene Anzeigename nicht verwendet (stattdessen wird immer „ShortName“ angezeigt). 

```xml
<tile>
  <visual branding="nameAndLogo" displayName="Wednesday 22">
 
    <binding template="TileMedium" displayName="Wed. 22">
      ...
    </binding>
 
    <!--Inherits displayName from visual-->
    <binding template="TileWide">
      ...
    </binding>
 
  </visual>
</tile>
```

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        Branding = TileBranding.NameAndLogo,
        DisplayName = "Wednesday 22",

        TileMedium = new TileBinding()
        {
            DisplayName = "Wed. 22",
            ...
        },

        // Inherits DisplayName from Visual
        TileWide = new TileBinding()
        {
            ...
        }
    }
};
```

**Ergebnis:**

![Anzeigename adaptiver Kacheln](images/adaptive-tiles-displayname.png)

## <a name="text"></a>Text


Das [adaptivetext](../tiles-and-notifications/tile-schema.md#adaptivetext) -Element wird zum Anzeigen von Text verwendet. Mithilfe von Hinweisen können Sie die Darstellung von Text anpassen.

```xml
<text>This is a line of text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of text"
};
```

**Ergebnis:**

![Text adaptiver Kacheln](images/adaptive-tiles-text.png)

## <a name="text-wrapping"></a>Textumbruch


Text wird standardmäßig nicht umbrochen und verläuft über den Kachelrand hinaus. Verwenden Sie das **hint-wrap**-Attribut, um den Textumbruch für ein text-Element festzulegen. Mit **hint-minLines** und **hint-maxLines**, die beide positive ganze Zahlen akzeptieren, können Sie auch die minimale und maximale Zeilenanzahl steuern.

```xml
<text hint-wrap="true">This is a line of wrapping text</text>
```


```csharp
new AdaptiveText()
{
    Text = "This is a line of wrapping text",
    HintWrap = true
};
```

**Ergebnis:**

![Adaptive Kachel mit Textumbruch](images/adaptive-tiles-textwrapping.png)

## <a name="text-styles"></a>Textstile


Stile steuern den Schriftgrad, die Schriftfarbe und Schriftbreite von text-Elementen. Es sind mehrere Stile verfügbar. Zusätzlich gibt es leichte („subtle“) Variationen jedes Stils, durch die die Deckkraft auf 60 % festgelegt und die Textfarbe normalerweise in einem hellgrauen Farbton angezeigt wird.

```xml
<text hint-style="base">Header content</text>
<text hint-style="captionSubtle">Subheader content</text>
```

```csharp
new AdaptiveText()
{
    Text = "Header content",
    HintStyle = AdaptiveTextStyle.Base
},

new AdaptiveText()
{
    Text = "Subheader content",
    HintStyle = AdaptiveTextStyle.CaptionSubtle
}
```

**Ergebnis:**

![Textstile adaptiver Kacheln](images/adaptive-tiles-textstyles.png)

**Hinweis**    Der Stil ist standardmäßig auf Caption festgelegt, wenn Hint-Style nicht angegeben ist.

 

**Allgemeine Textstile**

|                                |                           |             |
|--------------------------------|---------------------------|-------------|
| &lt;Text Hint-Style = " \* "/&gt; | Zeichenhöhe               | Schriftbreite |
| caption                        | 12 effektive Pixel (epx) | Regulär     |
| body                           | 15 Epx                    | Regulär     |
| base                           | 15 Epx                    | Halbfett    |
| subtitle                       | 20 Epx                    | Regulär     |
| title                          | 24 Epx                    | Semilight   |
| subheader                      | 34 Epx                    | Hell       |
| Header                         | 46 Epx                    | Hell       |

 

**Numerische Variationen des Textstils**

Durch diese Variationen wird die Zeilenhöhe verringert, sodass der Abstand zu Inhalten über und unter der Zeile deutlich kleiner wird.

|                  |
|------------------|
| titleNumeral     |
| subheaderNumeral |
| headerNumeral    |

 

**Leichte Variationen des Textstils**

Jeder Stil weist eine leichte Variation auf, durch die der Text eine 60 %-ige Deckkraft erhält und normalerweise in einem hellgrauen Farbton angezeigt wird.

|                        |
|------------------------|
| captionSubtle          |
| bodySubtle             |
| baseSubtle             |
| subtitleSubtle         |
| titleSubtle            |
| titleNumeralSubtle     |
| subheaderSubtle        |
| subheaderNumeralSubtle |
| headerSubtle           |
| headerNumeralSubtle    |

 

## <a name="text-alignment"></a>Textausrichtung


Text kann horizontal, linksbündig, zentriert oder rechtsbündig ausgerichtet sein. Bei einer von links nach rechts gelesenen Sprache, wie Deutsch, ist Text standardmäßig linksbündig ausgerichtet. Bei einer von rechts nach links gelesenen Sprache, wie Arabisch, ist Text standardmäßig rechtsbündig ausgerichtet. Mit dem **hint-align**-Attribut können Sie die Ausrichtung für Elemente manuell festlegen.

```xml
<text hint-align="center">Hello</text>
```


```csharp
new AdaptiveText()
{
    Text = "Hello",
    HintAlign = AdaptiveTextAlign.Center
};
```

**Ergebnis:**

![Textausrichtung adaptiver Kacheln](images/adaptive-tiles-textalignment.png)

## <a name="groups-and-subgroups"></a>Gruppen und Untergruppen


Mit Gruppen können Sie semantisch deklarieren, dass sich Inhalte in der Gruppe aufeinander beziehen und vollständig angezeigt werden müssen, damit sie Sinn ergeben. Beispielsweise können Sie über zwei text-Elemente in Form einer Überschrift und einer Unterüberschrift verfügen, und es würde keinen Sinn ergeben, wenn nur die Überschrift angezeigt würde. Durch die Gruppierung dieser Elemente innerhalb einer Untergruppe werden entweder alle Elemente angezeigt (sofern sie in den Anzeigebereich passen) oder keine Elemente angezeigt (wenn nicht genügend Platz vorhanden ist).

Um optimale Ergebnisse auf unterschiedlichen Geräten und Bildschirmen zu erzielen, sollten Sie mehrere Gruppen bereitstellen. Mit mehreren Gruppen kann sich die Kachel an größere Bildschirme anpassen.

**Hinweis**    Das einzige gültige untergeordnete Element einer Gruppe ist eine Untergruppe.

 

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup>
      <text hint-style="subtitle">Jennifer Parker</text>
      <text hint-style="captionSubtle">Photos from our trip</text>
      <text hint-style="captionSubtle">Check out these awesome photos I took while in New Zealand!</text>
    </subgroup>
  </group>
 
  <text />
 
  <group>
    <subgroup>
      <text hint-style="subtitle">Steve Bosniak</text>
      <text hint-style="captionSubtle">Build 2015 Dinner</text>
      <text hint-style="captionSubtle">Want to go out for dinner after Build tonight?</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            CreateGroup(
                from: "Jennifer Parker",
                subject: "Photos from our trip",
                body: "Check out these awesome photos I took while in New Zealand!"),

            // For spacing
            new AdaptiveText(),

            CreateGroup(
                from: "Steve Bosniak",
                subject: "Build 2015 Dinner",
                body: "Want to go out for dinner after Build tonight?")
        }
    }
}

...

private static AdaptiveGroup CreateGroup(string from, string subject, string body)
{
    return new AdaptiveGroup()
    {
        Children =
        {
            new AdaptiveSubgroup()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },
                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },
                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    };
}
```

**Ergebnis:**

![Gruppen und Untergruppen adaptiver Kacheln](images/adaptive-tiles-groups-subgroups.png)

## <a name="subgroups-columns"></a>Untergruppen (Spalten)


Mithilfe von Untergruppen können Sie Daten außerdem in semantische Abschnitte innerhalb einer Gruppe unterteilen. Bei Live-Kacheln werden Untergruppen visuell als Spalten dargestellt.

Mit dem **hint-weight**-Attribut wird die Breite von Spalten gesteuert. Der **hint-weight**-Wert wird als gewichteter Anteil am verfügbaren Platz ausgedrückt, was mit dem **GridUnitType.Star**-Verhalten übereinstimmt. Weisen Sie jeder Gewichtung 1 zu, um Spalten mit gleicher Breite zu erhalten.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="odd">
<td align="left">1</td>
<td align="left">25%</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 4</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen, Spalten mit gleicher Breite](images/adaptive-tiles-subgroups01.png)

Um eine Spalte doppelt so groß wie eine andere Spalte darzustellen, weisen Sie der kleineren Spalte die Gewichtung 1 und der größeren Spalte die Gewichtung 2 zu.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">33,3 %</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">66,7 %</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 3</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen, eine Spalte ist doppelt so groß wie die andere](images/adaptive-tiles-subgroups02.png)

Wenn Ihre erste Spalte 20 % und die zweite Spalte 80 % der gesamten Breite einnehmen soll, weisen Sie der ersten Gewichtung 20 und der zweiten Gewichtung 80 zu. Wenn die Gewichtungen insgesamt 100 ergeben, werden sie prozentual ausgedrückt.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">hint-weight</td>
<td align="left">Prozentuale Breite</td>
</tr>
<tr class="even">
<td align="left">20</td>
<td align="left">20%</td>
</tr>
<tr class="odd">
<td align="left">80</td>
<td align="left">80 %</td>
</tr>
<tr class="even">
<td align="left">Gesamtgewichtung: 100</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

![Untergruppen mit einer Gesamtgewichtung von 100](images/adaptive-tiles-subgroups03.png)

**Hinweis**    Zwischen den Spalten wird automatisch ein 8-Pixel-Rand hinzugefügt.

 

Wenn Sie über mehr als zwei Untergruppen verfügen, geben Sie **hint-weight** an (akzeptiert nur positive ganze Zahlen). Wenn Sie für die erste Untergruppe „hint-weight“ nicht angeben, wird ihr die Gewichtung 50 zugewiesen. Der nächsten Untergruppe, für die „hint-weight“ nicht angegeben wurde, wird die Gewichtung 100 abzüglich der Summe der vorherigen Gewichtungen oder 1 zugewiesen, wenn das Ergebnis 0 (null) ist. Den übrigen Untergruppen, für die „hint-weight“ nicht angegeben wurde, wird die Gewichtung 1 zugewiesen.

Im Folgenden sehen Sie den Beispielcode für eine Wetter-Kachel, die zeigt, wie Sie eine Kachel mit fünf gleich breiten Spalten erhalten:

```xml
<binding template="TileWide" displayName="Seattle" branding="name">
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Tue</text>
      <image src="Assets\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-align="center" hint-style="captionsubtle">38°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Wed</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">59°</text>
      <text hint-align="center" hint-style="captionsubtle">43°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Thu</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">62°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    <subgroup hint-weight="1">
      <text hint-align="center">Fri</text>
      <image src="Assets\Weather\Sunny.png" hint-removeMargin="true"/>
      <text hint-align="center">71°</text>
      <text hint-align="center" hint-style="captionsubtle">66°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°"),
                    CreateSubgroup("Wed", "Sunny.png", "59°", "43°"),
                    CreateSubgroup("Thu", "Sunny.png", "62°", "42°"),
                    CreateSubgroup("Fri", "Sunny.png", "71°", "66°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Beispiel für eine Wetter-Kachel](images/adaptive-tiles-weathertile.png)

## <a name="images"></a>Bilder


Mithilfe des &lt;image&gt;-Elements werden Bilder auf der Kachelbenachrichtigung angezeigt. Bilder können als Inlinebilder im Kachelinhalt (Standard), als Hintergrundbild hinter dem Inhalt oder als animiertes Vorschaubild, das von oben in die Benachrichtigung hineingleitet, konfiguriert werden.

> [!NOTE]
> Images können aus dem App-Paket, dem lokalen Speicher der APP oder aus dem Web verwendet werden. Im Fall von Creators Update können webimages bei normalen Verbindungen bis zu 3 MB und bei getakteten Verbindungen 1 MB betragen. Auf Geräten, auf denen das Fall Creators Update noch nicht ausgeführt wird, dürfen webimages nicht größer als 200 KB sein.

 

Wenn kein zusätzliches Verhalten angegeben wird, verkleinern bzw. vergrößern sich Bilder gleichmäßig in Anpassung an die verfügbare Breite. Dieses Beispiel zeigt eine Kachel, die zwei Spalten und Inline Bilder verwendet. Die Inlinebilder werden gestreckt, um die Spaltenbreite auszufüllen.

```xml
<binding template="TileMedium" displayName="Seattle" branding="name">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Apps\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Apps\Weather\Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionSubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    DisplayName = "Seattle",
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°"),
                    CreateSubgroup("Tue", "Cloudy.png", "57°", "38°")
                }
            }
        }
    }
}
...
private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Bildbeispiel](images/adaptive-tiles-images01.png)

Bilder, die im &lt;binding&gt;-Stammelement oder in der ersten Gruppe enthalten sind, werden ebenfalls gestreckt, um die verfügbare Höhe auszunutzen.

### <a name="image-alignment"></a>Bildausrichtung

Bilder können mit dem **hint-align**-Attribut linksbündig, zentriert oder rechtsbündig ausgerichtet werden. Dies bewirkt auch, dass Bilder in ihrer systemeigenen Auflösung angezeigt und nicht gestreckt werden, um die gesamte Breite auszufüllen.

```xml
<binding template="TileLarge">
  <image src="Assets/fable.jpg" hint-align="center"/>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveImage()
            {
                Source = "Assets/fable.jpg",
                HintAlign = AdaptiveImageAlign.Center
            }
        }
    }
}
```

**Ergebnis:**

![Beispiel für die Bildausrichtung (linksbündig, zentriert, rechtsbündig)](images/adaptive-tiles-imagealignment.png)

### <a name="image-margins"></a>Bildränder

Zwischen Inlinebildern und darüber oder darunter angeordneten Inhalten befindet sich standardmäßig ein Rand von 8 Pixeln. Dieser Rand kann entfernt werden, indem Sie das **hint-removeMargin**-Attribut für das Bild verwenden. Für Bilder wird allerdings immer der 8-Pixel-Rand vom Rand der Kachel und für Untergruppen (Spalten) immer der 8-Pixel-Abstand zwischen Spalten beibehalten.

```xml
<binding template="TileMedium" branding="none">
  <group>
    <subgroup>
      <text hint-align="center">Mon</text>
      <image src="Assets\Numbers\4.jpg" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-style="captionsubtle" hint-align="center">42°</text>
    </subgroup>
    <subgroup>
      <text hint-align="center">Tue</text>
      <image src="Assets\Numbers\3.jpg" hint-removeMargin="true"/>
      <text hint-align="center">57°</text>
      <text hint-style="captionsubtle" hint-align="center">38°</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.None,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "4.jpg", "63°", "42°"),
                    CreateSubgroup("Tue", "3.jpg", "57°", "38°")
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Numbers/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

![Beispiel für „hint-removeMargin“](images/adaptive-tiles-removemargin.png)

### <a name="image-cropping"></a>Zuschneiden von Bildern

Bilder können mit dem **hint-crop**-Attribut, das derzeit nur die Werte „none“ (Standard) oder „circle“ unterstützt, kreisförmig zugeschnitten werden.

```xml
<binding template="TileLarge" hint-textStacking="center">
  <group>
    <subgroup hint-weight="1"/>
    <subgroup hint-weight="2">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-weight="1"/>
  </group>
 
  <text hint-style="title" hint-align="center">Hi,</text>
  <text hint-style="subtitleSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileLarge = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    new AdaptiveSubgroup() { HintWeight = 1 },
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 2,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },
                    new AdaptiveSubgroup() { HintWeight = 1 }
                }
            },
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Title,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.SubtitleSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

**Ergebnis:**

![Beispiel für das Zuschneiden eines Bilds](images/adaptive-tiles-imagecropping.png)

### <a name="background-image"></a>Hintergrundbild

Um ein Hintergrundbild festzulegen, platzieren Sie ein image-Element im Stamm von &lt;binding&gt; und legen das placement-Attribut auf „background“ fest.

```xml
<binding template="TileWide">
  <image src="Assets\Mostly Cloudy-Background.jpg" placement="background"/>
  <group>
    <subgroup hint-weight="1">
      <text hint-align="center">Mon</text>
      <image src="Assets\Weather\Mostly Cloudy.png" hint-removeMargin="true"/>
      <text hint-align="center">63°</text>
      <text hint-align="center" hint-style="captionsubtle">42°</text>
    </subgroup>
    ...
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg"
        },

        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    CreateSubgroup("Mon", "Mostly Cloudy.png", "63°", "42°")
                    ...
                }
            }
        }
    }
}

...

private static AdaptiveSubgroup CreateSubgroup(string day, string image, string highTemp, string lowTemp)
{
    return new AdaptiveSubgroup()
    {
        HintWeight = 1,
        Children =
        {
            new AdaptiveText()
            {
                Text = day,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveImage()
            {
                Source = "Assets/Weather/" + image,
                HintRemoveMargin = true
            },
            new AdaptiveText()
            {
                Text = highTemp,
                HintAlign = AdaptiveTextAlign.Center
            },
            new AdaptiveText()
            {
                Text = lowTemp,
                HintAlign = AdaptiveTextAlign.Center,
                HintStyle = AdaptiveTextStyle.CaptionSubtle
            }
        }
    };
}
```

**Ergebnis:**

![Beispiel für ein Hintergrundbild](images/adaptive-tiles-backgroundimage.png)

### <a name="peek-image"></a>Vorschaubild

Sie können ein Bild angeben, das von oben in die Kachel hineingleitet. Das Vorschaubild gleitet mithilfe einer Animation von oben in die Kachel hinein, ist kurz auf der Kachel zu sehen und gleitet danach wieder nach oben heraus, um den Blick auf den Hauptinhalt der Kachel freizugeben. Um ein Vorschaubild festzulegen, platzieren Sie ein image-Element im Stamm von &lt;binding&gt; und legen das placement-Attribut auf „peek“ fest.

```xml
<binding template="TileMedium" branding="name">
  <image placement="peek" src="Assets/Apps/Hipstame/hipster.jpg"/>
  <text>New Message</text>
  <text hint-style="captionsubtle" hint-wrap="true">Hey, have you tried Windows 10 yet?</text>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.Name,
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Apps/Hipstame/hipster.jpg"
        },
        Children =
        {
            new AdaptiveText()
            {
                Text = "New Message"
            },
            new AdaptiveText()
            {
                Text = "Hey, have you tried Windows 10 yet?",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintWrap = true
            }
        }
    }
}
```

![Beispiele für Vorschaubilder](images/adaptive-tiles-imagepeeking.png)

**Kreisförmiges Zuschneiden für Vorschau- und Hintergrundbilder**

Wenden Sie das hint-crop-Attribut auf Vorschau- und Hintergrundbilder an, um einen kreisförmigen Zuschnitt zu erhalten:

```xml
<image placement="peek" hint-crop="circle" src="Assets/Apps/Hipstame/hipster.jpg"/>
```

```csharp
new TilePeekImage()
{
    HintCrop = TilePeekImageCrop.Circle,
    Source = "Assets/Apps/Hipstame/hipster.jpg"
}
```

Das Ergebnis sieht so aus:

![Kreisförmiges Zuschneiden für Vorschau- und Hintergrundbilder](images/circlecrop-image.png)

**Verwenden eines Vorschau- und eines Hintergrundbilds**

Zur Verwendung eines Vorschau- und eines Hintergrundbilds auf einer Kachelbenachrichtigung geben Sie in der Benachrichtigungsnutzlast sowohl ein Vorschau- als auch ein Hintergrundbild an.

Das Ergebnis sieht so aus:

![Gleichzeitiges Verwenden von Vorschau- und Hintergrundbild](images/peekandbackground.png)


### <a name="peek-and-background-image-overlays"></a>Overlays von Vorschau- und Hintergrundbild

Sie können mit **hint-overlay** eine schwarze Überlagerung für Hintergrund- und Vorschaubild festlegen. Das Attribut akzeptiert ganze Zahlen von 0 bis 100, wobei 0 keine Überlagerung und 100 eine vollständige schwarze Überlagerung angibt. Sie können das Overlay verwenden, um sicherzustellen, dass der Text auf der Kachel lesbar ist.

**Verwenden von „hint-overlay“ für ein Hintergrundbild**

Das Hintergrundbild wird standardmäßig auf eine Überlagerung von 20 % festgelegt, solange es in der Nutzlast Textelemente gibt. (Andernfalls wird standardmäßig eine Überlagerung von 0 % festgelegt.)

```xml
<binding template="TileWide">
  <image placement="background" hint-overlay="60" src="Assets\Mostly Cloudy-Background.jpg"/>
  ...
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        BackgroundImage = new TileBackgroundImage()
        {
            Source = "Assets/Mostly Cloudy-Background.jpg",
            HintOverlay = 60
        },

        ...
    }
}
```

**Ergebnis von „hint-overlay“:**

![Beispiel für ein Bild mit angewendetem „hint-overlay“](images/adaptive-tiles-image-hintoverlay.png)

**Verwenden von „hint-overlay“ für ein Vorschaubild**

In Version 1511 von Windows 10 unterstützen wir Überlagerungen für Vorschaubilder, genau wie für Hintergrundbilder. Geben Sie „hint-overlay“ für das Vorschaubildelement als ganze Zahl von 0 bis 100 an. Die Standardüberlagerung für Vorschaubilder ist 0 (keine Überlagerung).

```xml
<binding template="TileMedium">
  <image hint-overlay="20" src="Assets\Map.jpg" placement="peek"/>
  ...
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Content = new TileBindingContentAdaptive()
    {
        PeekImage = new TilePeekImage()
        {
            Source = "Assets/Map.jpg",
            HintOverlay = 20
        },
        ...
    }
}
```

Dieses Beispiel zeigt ein Vorschaubild mit 20 % Deckkraft (links) und 0 % Deckkraft (rechts):

![„hint-overlay“ für ein Vorschaubild](images/hintoverlay.png)

## <a name="vertical-alignment-text-stacking"></a>Vertikale Ausrichtung (hint-textStacking)


Sie können die vertikale Ausrichtung des Inhalts auf der Kachel steuern, indem Sie das Attribut **Hint-textstacking** sowohl für das [tilebinding](../tiles-and-notifications/tile-schema.md#tilebinding) -Element als auch das [adaptivesubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) -Element verwenden. Der gesamte Inhalt wird standardmäßig vertikal am oberen Rand ausgerichtet, kann aber auch in der Mitte oder am unteren Rand ausgerichtet werden.

### <a name="text-stacking-on-binding-element"></a>Gestapelter Text für binding-Element

Wenn das Stapeln auf [tilebinding](../tiles-and-notifications/tile-schema.md#tilebinding) -Ebene angewendet wird, legt es die vertikale Ausrichtung des Benachrichtigungs Inhalts als Ganzes fest. dabei wird der verfügbare vertikale Bereich oberhalb des Brandungs-/Badge-Bereichs ausgerichtet.

```xml
<binding template="TileMedium" hint-textStacking="center" branding="logo">
  <text hint-style="base" hint-align="center">Hi,</text>
  <text hint-style="captionSubtle" hint-align="center">MasterHip</text>
</binding>
```

```csharp
TileMedium = new TileBinding()
{
    Branding = TileBranding.Logo,
    Content = new TileBindingContentAdaptive()
    {
        TextStacking = TileTextStacking.Center,
        Children =
        {
            new AdaptiveText()
            {
                Text = "Hi,",
                HintStyle = AdaptiveTextStyle.Base,
                HintAlign = AdaptiveTextAlign.Center
            },

            new AdaptiveText()
            {
                Text = "MasterHip",
                HintStyle = AdaptiveTextStyle.CaptionSubtle,
                HintAlign = AdaptiveTextAlign.Center
            }
        }
    }
}
```

![Gestapelter Text für binding-Element](images/adaptive-tiles-textstack-bindingelement.png)

### <a name="text-stacking-on-subgroup-element"></a>Gestapelter Text für subgroup-Element

Wenn das Stapeln auf [adaptivesubgroup](../tiles-and-notifications/tile-schema.md#adaptivesubgroup) -Ebene angewendet wird, legt es die vertikale Ausrichtung des Untergruppen Inhalts (Spalten Inhalt) fest. dabei wird der verfügbare vertikale Raum innerhalb der gesamten Gruppe ausgerichtet.

```xml
<binding template="TileWide" branding="nameAndLogo">
  <group>
    <subgroup hint-weight="33">
      <image src="Assets/Apps/Hipstame/hipster.jpg" hint-crop="circle"/>
    </subgroup>
    <subgroup hint-textStacking="center">
      <text hint-style="subtitle">Hi,</text>
      <text hint-style="bodySubtle">MasterHip</text>
    </subgroup>
  </group>
</binding>
```

```csharp
TileWide = new TileBinding()
{
    Branding = TileBranding.NameAndLogo,
    Content = new TileBindingContentAdaptive()
    {
        Children =
        {
            new AdaptiveGroup()
            {
                Children =
                {
                    // Image column
                    new AdaptiveSubgroup()
                    {
                        HintWeight = 33,
                        Children =
                        {
                            new AdaptiveImage()
                            {
                                Source = "Assets/Apps/Hipstame/hipster.jpg",
                                HintCrop = AdaptiveImageCrop.Circle
                            }
                        }
                    },

                    // Text column
                    new AdaptiveSubgroup()
                    {
                        // Vertical align its contents
                        TextStacking = TileTextStacking.Center,
                        Children =
                        {
                            new AdaptiveText()
                            {
                                Text = "Hi,",
                                HintStyle = AdaptiveTextStyle.Subtitle
                            },

                            new AdaptiveText()
                            {
                                Text = "MasterHip",
                                HintStyle = AdaptiveTextStyle.BodySubtle
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## <a name="related-topics"></a>Zugehörige Themen
* [Kachelinhaltsschema](../tiles-and-notifications/tile-schema.md)
* [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md)
* [Spezielle Kachelvorlagen](special-tile-templates-catalog.md)
* [UWP-Community-Toolkit-Benachrichtigungen](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Windows-Benachrichtigungen auf GitHub](https://github.com/WindowsNotifications)

 

 