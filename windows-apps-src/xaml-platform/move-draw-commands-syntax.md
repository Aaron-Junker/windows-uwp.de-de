---
description: Informieren Sie sich über die Verschieben- und Zeichnen-Befehle (eine Minisprache), mit denen Sie Pfadgeometrien als XAML-Attributwert angeben können.
title: Syntax für die Verschieben- und Zeichnen-Befehle
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 40b959feed09546791840dafe15ab98d65f0ea09
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371155"
---
# <a name="move-and-draw-commands-syntax"></a>Syntax für die Verschieben- und Zeichnen-Befehle


Informieren Sie sich über die Verschieben- und Zeichnen-Befehle (eine Minisprache), mit denen Sie Pfadgeometrien als XAML-Attributwert angeben können. Verschieben- und Zeichnen-Befehle werden von vielen Design- und Grafiktools, die eine Vektorgrafik oder Form ausgeben können, als Serialisierungs- und Austauschformat verwendet.

## <a name="properties-that-use-move-and-draw-command-strings"></a>Eigenschaften, die Verschieben- und Zeichnen-Befehlszeichenfolgen verwenden

Die Syntax für die Verschieben- und Zeichnen-Befehle wird von einem internen Typkonverter für XAML verwendet, der die Befehle analysiert und eine Laufzeit-Grafikdarstellung erzeugt. Diese Darstellung ist im Grunde ein fertiger Satz von Vektoren, die zur Anzeige bereit sind. Mit den Vektoren selbst sind die Darstellungsdetails noch nicht vollständig. Sie müssen weitere Werte für die Elemente festlegen. Für ein [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)-Objekt benötigen Sie auch Werte für [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), [**Stroke**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.stroke) und andere Eigenschaften. Anschließend muss der **Path** mit der visuellen Struktur verbunden werden. Für ein [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon)-Objekt legen Sie die [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.iconelement.foreground)-Eigenschaft fest.

Es gibt zwei Eigenschaften in der Windows-Runtime, die kann eine Zeichenfolge, die Verschiebung und draw-Befehle aus: [**Path.Data** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) und [ **PathIcon.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon.data). Wenn Sie eine dieser Eigenschaften festlegen, indem Sie Verschieben- und Zeichnen-Befehle angeben, legen Sie dafür in der Regel einen XAML-Attributwert sowie weitere erforderliche Attribute dieses Elements fest. Ohne auf die Einzelheiten einzugehen, sieht das Ganze so aus:

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry.Figures** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures) auch verwenden, verschieben und draw-Befehle. Sie können ein [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekt, das Verschieben- und Zeichnen-Befehle verwendet, mit anderen [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry)-Typen in einem [**GeometryGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GeometryGroup)-Objekt kombinieren, das Sie dann als Wert für [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) nutzen. Weitaus gängiger ist aber die Verwendung von Verschieben- und Zeichnen-Befehlen für durch Attribute definierte Daten.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>Verwenden von Verschieben- und Zeichnen-Befehlen oder einer **PathGeometry**-Klasse

Für Windows-Runtime-XAML erzeugen die Verschieben- und Zeichnen-Befehle eine [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Klasse mit einem einzigen [**PathFigure**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathFigure)-Objekt, das über einen [**Figures**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures)-Eigenschaftswert verfügt. Jeder Zeichnen-Befehl erzeugt eine von der [**PathSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathSegment)-Klasse abgeleitete Klasse in der [**Segments**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.segments)-Collection dieses einen **PathFigure**-Objekts. Der Verschieben-Befehl ändert die [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.startpoint)-Eigenschaft. Zudem wird durch das Vorhandensein eines Schließen-Befehls [**IsClosed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) auf **true** festgelegt. In dieser Struktur können Sie dann wie in einem Objektmodell navigieren, wenn Sie zur Laufzeit die **Data**-Werte untersuchen.

## <a name="the-basic-syntax"></a>Grundlegende Syntax

Hier ein Überblick der Syntax für Verschieben- und Zeichnen-Befehle:

1.  Beginnen Sie mit einer optionalen Füllregel. Normalerweise geben Sie diese Füllregel nur an, wenn Sie den **EvenOdd**-Standard nicht verwenden möchten. (**EvenOdd** wird später erläutert.)
2.  Geben Sie genau einen Verschieben-Befehl an.
3.  Geben Sie mindestens einen Zeichnen-Befehl an.
4.  Geben Sie einen Schließen-Befehl an. Sie können den Schließen-Befehl weglassen, aber dann bleibt die Figur geöffnet (das wäre ungewöhnlich).

Allgemeine Regeln für diese Syntax:

-   Jeder Befehl wird durch genau einen Buchstaben dargestellt.
-   Dabei kann es sich um einen Groß- oder Kleinbuchstaben handeln. Die Groß-/Kleinschreibung spielt eine Rolle. Darauf gehen wir noch ein.
-   Auf jeden Befehl mit Ausnahme des Schließen-Befehls folgt normalerweise mindestens eine Zahl.
-   Wenn Sie für einen Befehl mehrere Zahlen angeben, trennen Sie diese durch Komma oder Leerzeichen.

**\[** _FillRule_ **\]** _MoveCommand_ _DrawCommand_ **\[**  _DrawCommand_ **\* \]** **\[** _CloseCommand_ **\]**

Viele der Zeichnen-Befehle verwenden Punkte, für die Sie einen _x,y_-Wert angeben. Jedes Mal, wenn Sie sehen eine \* _Punkte_ Platzhalter können Sie davon ausgehen Sie geben zwei Dezimalwerte für die _X, y_ Wert eines Punkts.

Leerzeichen können bei eindeutigen Ergebnissen häufig weggelassen werden. Tatsächlich können Sie alle Leerzeichen weglassen, wenn Sie Kommas als Trennzeichen für alle Zahlengruppen (Punkte und Größe) verwenden. Diese Verwendung ist zum Beispiel gültig: `F1M0,58L2,56L6,60L13,51L15,53L6,64z`. Typischer ist allerdings die Verwendung von Leerzeichen zwischen Befehlen, um die Übersichtlichkeit zu verbessern.

Verwenden Sie als Dezimalzeichen für Dezimalzahlen kein Komma. Die Befehlszeichenfolge wird von XAML interpretiert und berücksichtigt keine länderspezifischen Konventionen für die Zahlenformatierung, die von den Konventionen im Gebietsschema **en-us** abweichen.

## <a name="syntax-specifics"></a>Einzelheiten zur Syntax

**Geben Sie die Regel**

Es gibt zwei mögliche Werte für die optionale-Füllregel fest: **F0** oder **F1**. (Die **F** ist immer groß geschrieben.) **F0** ist der Standardwert; es erzeugt **EvenOdd** Verhalten ausfüllen, damit Sie in der Regel es nicht angeben. Verwenden Sie **F1**, um das Füllverhalten für **Nonzero** abzurufen. Diese Füllwerte sind an die Werte der [**FillRule**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.FillRule)-Aufzählung angepasst.

**Move-Befehl**

Gibt den Anfangspunkt einer neuen Figur an.

| Syntax |
|--------|
| `M ` _startPoint_ <br/>- Oder -<br/>`m` _startPoint_|

| Begriff | Beschreibung |
|------|-------------|
| _startPoint_ | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/>Der Anfangspunkt einer neuen Figur|

Der Großbuchstabe **M** gibt an, dass *startPoint* eine absolute Koordinate ist. Der Kleinbuchstabe **m** gibt an, dass *startPoint* ein Offset zum vorherigen Punkt ist. Wenn kein vorheriger Punkt vorhanden ist, wird (0,0) angegeben.

**Beachten Sie**  ist zulässig, mehrere Punkte nach dem Move-Befehl angeben. Es wird eine Linie zu diesen Punkten gezeichnet, als hätten Sie den Linienbefehl angegeben. Dieser Stil wird aber nicht empfohlen, verwenden Sie stattdessen den speziellen Linienbefehl.

**Zeichnen-Befehle**

Ein Zeichnen-Befehl kann aus mehreren Formbefehlen bestehen: Linie, horizontale Linie, vertikale Linie, kubische Bézierkurve, quadratische Bézierkurve, glatte Bézierkurve, glatte quadratische Bézierkurve und Ellipsenbogen.

Bei allen Zeichnen-Befehlen spielt die Groß-/Kleinschreibung eine Rolle. Großbuchstaben geben absolute Koordinaten an und Kleinbuchstaben geben relative Koordinaten zum vorherigen Befehl an.

Die Kontrollpunkte für ein Segment sind relativ zum Endpunkt des vorherigen Segments. Wenn Sie nacheinander mehrere Befehle des gleichen Typs eingeben, müssen Sie die Befehle nicht doppelt eingeben. So ist zum Beispiel `L 100,200 300,400` gleichwertig mit `L 100,200 L 300,400`.

**Line-Befehl**

Erstellt eine gerade Linie zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. `l 20 30` und `L 20,30` sind Beispiele für gültige Linienbefehle. Definiert das Äquivalent eines [**LineGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LineGeometry)-Objekts.

| Syntax |
|--------|
| `L` _endPoint_ <br/>- Oder -<br/>`l` _endPoint_ |

| Begriff | Beschreibung |
|------|-------------|
| endPoint | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/>Der Endpunkt der Linie|

**Befehl für eine horizontale Linie**

Erstellt eine horizontale Linie zwischen dem aktuellen Punkt und der angegebenen x-Koordinate. `H 90` ist ein Beispiel für einen gültigen Befehl für eine horizontale Linie.

| Syntax |
|--------|
| `H ` _x_ <br/> - Oder - <br/>`h ` _x_ |

| Begriff | Beschreibung |
|------|-------------|
| x | [**Double-Wert**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) <br/> Die x-Koordinate am Endpunkt der Linie |

**Befehl für vertikale Linie**

Erstellt eine vertikale Linie zwischen dem aktuellen Punkt und der angegebenen y-Koordinate. `v 90` ist ein Beispiel für einen gültigen Befehl für eine vertikale Linie.

| Syntax |
|--------|
| `V ` _y_ <br/> - Oder - <br/> `v ` _y_ |

| Begriff | Beschreibung |
|------|-------------|
| *y* | [**Double-Wert**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) <br/> Die y-Koordinate am Endpunkt der Linie |

**Kubische Bézier kurvenbefehl**

Erstellt eine kubische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Dabei werden die beiden angegebenen Kontrollpunkte (*controlPoint1* und *controlPoint2*) verwendet. `C 100,200 200,400 300,200` ist ein Beispiel für einen gültigen Kurvenbefehl. Definiert das Äquivalent eines [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekts mit einem [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment)-Objekt.

| Syntax |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> - Oder - <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| Begriff | Beschreibung |
|------|-------------|
| *controlPoint1* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der erste Kontrollpunkt der Kurve, der die Anfangstangente der Kurve bestimmt. |
| *controlPoint2* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der zweite Kontrollpunkt der Kurve, der die Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der Punkt, zu dem die Kurve gezeichnet wird. | 

**Befehl für eine quadratische Bézier**

Erstellt eine quadratische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Dabei wird der angegebene Kontrollpunkt (*controlPoint*) verwendet. `q 100,200 300,200` ist ein Beispiel für einen gültigen Befehl für eine quadratische Bézierkurve. Definiert das Äquivalent eines [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekts mit einem [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment)-Objekt.

| Syntax |
|--------|
| `Q ` *ControlPoint-Endpunkt* <br/> - Oder - <br/> `q ` *ControlPoint-Endpunkt* |

| Begriff | Beschreibung |
|------|-------------|
| *controlPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der Kontrollpunkt der Kurve, der die Anfangs- und Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Smooth kubische Bézier kurvenbefehl**

Erstellt eine kubische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Als erster Kontrollpunkt wird die Spiegelung des zweiten Kontrollpunkts des vorherigen Befehls relativ zum aktuellen Punkt angenommen. Wenn kein vorheriger Befehl vorhanden ist oder der vorherige Befehl kein Befehl für eine kubische Bézierkurve oder eine glatte kubische Bézierkurve war, können Sie annehmen, dass der erste Kontrollpunkt mit dem aktuellen Punkt deckungsgleich ist. Der zweite Kontrollpunkt – der Kontrollpunkt für das Ende der Kurve – wird mit *controlPoint2* angegeben. `S 100,200 200,300` ist zum Beispiel ein gültiger Befehl für eine glatte kubische Bézierkurve. Dieser Befehl definiert das Äquivalent eines [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekts mit einem [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment)-Objekt, wobei ein vorhergehendes Kurvensegment vorhanden war.

| Syntax |
|--------|
| `S` *controlPoint2* *endPoint* <br/> - Oder - <br/>`s` *controlPoint2-Endpunkt* |

| Begriff | Beschreibung |
|------|-------------|
| *controlPoint2* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der Kontrollpunkt der Kurve, der die Endtangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Smooth-Befehl für eine quadratische Bézier**

Erstellt eine quadratische Bézierkurve zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Als Kontrollpunkt wird die Spiegelung des Kontrollpunkts des vorherigen Befehls relativ zum aktuellen Punkt angenommen. Wenn kein vorheriger Befehl vorhanden ist oder der vorherige Befehl kein Befehl für eine quadratische Bézierkurve oder eine glatte quadratische Bézierkurve war, ist der Kontrollpunkt mit dem aktuellen Punkt deckungsgleich. Dieser Befehl definiert das Äquivalent eines [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekts mit einem [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment)-Objekt, wobei ein vorhergehendes Kurvensegment vorhanden war.

| Syntax |
|--------|
| `T` *controlPoint* *endPoint* <br/> - Oder - <br/> `t` *controlPoint* *endPoint* |

| Begriff | Beschreibung |
|------|-------------|
| *controlPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Der Kontrollpunkt der Kurve, der die Anfangstangente der Kurve bestimmt. |
| *endPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Der Punkt, zu dem die Kurve gezeichnet wird. |

**Befehl für elliptischen Bogen**

Erstellt einen Ellipsenbogen zwischen dem aktuellen Punkt und dem angegebenen Endpunkt. Definiert das Äquivalent eines [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry)-Objekts mit einem [**ArcSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ArcSegment)-Objekt.

| Syntax |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> - Oder - <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| Begriff | Beschreibung |
|------|-------------|
| *size* | [**Größe**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size)<br/>Der x-Radius und y-Radius des Bogens |
| *rotationAngle* | [**Double-Wert**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) <br/> Die Drehung der Ellipse in Grad |
| *isLargeArcFlag* | Legen Sie „1“ fest, wenn der Winkel des Bogens mindestens 180 Grad entsprechen soll. Legen Sie anderenfalls „0“ fest. |
| *sweepDirectionFlag* | Legen Sie „1“ fest, wenn der Bogen in Richtung eines positiven Winkels gezeichnet wird. Legen Sie anderenfalls „0“ fest. |
| *endPoint* | [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Der Punkt, zu dem die Kurve gezeichnet wird.|
 
**Close-Befehl**

Beendet die aktuelle Figur und erstellt eine Linie, die den aktuellen Punkt mit dem Anfangspunkt der Figur verbindet. Dieser Befehl erstellt eine Linienverbindung (Ecke) zwischen dem letzten Segment und dem ersten Segment der Figur.

| Syntax |
|--------|
| `Z` <br/> - Oder - <br/> `z ` |

**Punkt-syntax**

Beschreibt die x-Koordinate und y-Koordinate eines Punkts. Weitere Informationen finden Sie auch unter [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point).

| Syntax |
|--------|
| *x*,*y*<br/> - Oder - <br/>*x* *y* |

| Begriff | Beschreibung |
|------|-------------|
| *x* | [**Double-Wert**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) <br/> Die x-Koordinate des Punkts |
| *y* | [**Double-Wert**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN) <br/> Die y-Koordinate des Punkts |

**Zusätzliche Hinweise**

Anstelle eines standardmäßigen numerischen Werts können Sie auch die folgenden speziellen Werte verwenden. Bei diesen Werten wird zwischen Groß-/Kleinschreibung unterschieden.

-   **Unendlich**: Stellt **PositiveInfinity**.
-   **\-Unendlich**: Stellt **NegativeInfinity**.
-   **NaN**: Stellt **NaN**.

Anstelle von Dezimalzahlen oder Ganzzahlen können Sie die wissenschaftliche Notation verwenden. `+1.e17` ist zum Beispiel ein gültiger Wert.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>Designtools zum Erzeugen von Verschieben- und Zeichnen-Befehlen

Mithilfe der **Stift** Tool und anderen Zeichentools in Blend für Microsoft Visual Studio 2015 erzeugt in der Regel eine [ **Pfad** ](/uwp/api/Windows.UI.Xaml.Shapes.Path) Objekt, mit verschieben und draw-Befehle.

Möglicherweise sehen Sie vorhandene Daten für Verschieben- und Zeichnen-Befehle in einigen Steuerelementkomponenten, die in den Standardvorlagen für Steuerelemente in Windows-Runtime-XAML definiert sind. So verwenden zum Beispiel einige Steuerelemente ein [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon)-Objekt, dessen Daten als Verschieben- und Zeichnen-Befehle definiert sind.

Für andere häufig verwendete Vektorgrafik-Designtools, die den Vektor in XAML-Form ausgeben können, sind Exporter oder Plug-Ins verfügbar. Diese erstellen gewöhnlich [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)-Objekte in einem Layoutcontainer mit Verschieben- und Zeichnen-Befehlen für die [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data)-Eigenschaft. XAML kann mehrere **Path**-Elemente enthalten, sodass verschiedene Pinsel angewendet werden können. Viele dieser Ausführer oder-Plug-ins die ursprünglich für Windows Presentation Foundation (WPF), XAML oder Silverlight geschrieben wurden, aber die XAML-Pfad-Syntax ist identisch mit XAML für Windows-Runtime. In der Regel können Sie XAML-Abschnitte aus einem Exporter verwenden und direkt in eine Windows-Runtime-XAML-Seite einfügen. (Es ist aber nicht möglich, einen **RadialGradientBrush**-Pinsel zu verwenden, wenn dieser Bestandteil der konvertierten XAML war, da Windows-Runtime-XAML diesen Pinsel nicht unterstützt.)

## <a name="related-topics"></a>Verwandte Themen

* [Zeichnen von Formen](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)
* [Verwenden von Pinseln](https://docs.microsoft.com/windows/uwp/graphics/using-brushes)
* [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon)

