---
title: Relationsbasierte Animationen
description: Erstellen sie Bewegungen auf Basis einer Eigenschaft eines anderen Objekts.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, Uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: b6fdc59e8a7203a3bb8c6ad79adabd446b884639
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597715"
---
# <a name="relation-based-animations"></a>Relationsbasierte Animationen

Dieser Artikel gibt einen kurzen Überblick darüber, wie man relationsbasierte Animationen mit Composition-ExpressionAnimations erstellt.

## <a name="dynamic-relation-based-experiences"></a>Erfahrungen auf Basis dynamischer Beziehungen

Beim Erstellen von Bewegungserlebnissen in einer App gibt es Zeiten, in denen die Bewegung nicht zeitabhängig, sondern von einer Eigenschaft eines anderen Objekts abhängig ist. KeyFrameAnimations sind nicht besonders gut in der Lage, diese Art von Bewegungserlebnissen auszudrücken. In diesen speziellen Fällen muss die Bewegung nicht mehr diskret und vordefiniert sein. Stattdessen kann sich die Bewegung dynamisch aufgrund ihrer Beziehung zu anderen Objekteigenschaften anpassen. Beispielsweise können Sie die Deckkraft eines Objekts anhand seiner horizontalen Position animieren. Andere Beispiele sind Bewegungserlebnisse wie Sticky Header und Parallax.

Diese Art von Bewegungserlebnissen ermöglicht es Ihnen, eine Benutzeroberfläche zu schaffen, die sich verbundener anstatt eigenständig und unabhängig anfühlt. Benutzern vermittelt dies den Eindruck eines dynamischen UI-Erlebnisses.

![Um Kreis umlaufend](images/animation/orbit.gif)

![Listenansicht mit Parallax](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Verwenden von ExpressionAnimations

Um relationsbasierte Bewegungserlebnisse zu erstellen, verwenden Sie den Typ ExpressionAnimation. ExpressionAnimations (oder kurz Expressions/Ausdrücke), ist eine neue Art der Animation, die es Ihnen erlaubt, eine mathematische Beziehung auszudrücken – eine Beziehung, die das System verwendet, um den Wert einer animierenden Eigenschaft jedes Frame zu berechnen. Anders ausgedrückt: Ausdrücke sind einfach eine mathematische Gleichung, die den gewünschten Wert einer Animationseigenschaft pro Frame definiert. Ausdrücke sind eine sehr vielseitige Komponente, die in einer Vielzahl von Szenarien verwendet werden kann, einschließlich:

- Relative Größe, Offset-Animationen.
- Sticky Header, Parallax mit ScrollViewer. (Siehe [Erweitern vorhandener ScrollViewer-Erfahrungen](scroll-input-animations.md).)
- Andockpunkte mit InertiaModifiers und InteractionTracker. (Siehe [Erstellen von Andockpunkten mit Inertia-Modifiern](inertia-modifiers.md).)

Bei der Arbeit mit ExpressionAnimations gibt es ein paar Dinge, die im Vorfeld erwähnenswert sind:

- Keine Beendigung – Im Gegensatz zu KeyFrameAnimation haben Ausdrücke keine endliche Dauer. Da Ausdrücke mathematische Beziehungen sind, sind sie Animationen, die ständig „ablaufen“. Sie haben die Möglichkeit, diese Animationen zu stoppen.
- Ausführung ohne ständige Auswertung – Leistung ist immer ein Anliegen bei ständig laufenden Animationen. Das System ist so intelligent, dass der Ausdruck nur dann neu bewertet wird, wenn sich irgendwelche Eingaben oder Parameter geändert haben.
- Auflösen in den richtigen Objekttyp – Da Ausdrücke mathematische Beziehungen sind, ist es wichtig sicherzustellen, dass die Gleichung, die der Ausdruck definiert, in den Eigenschaftstyp aufgelöst wird, auf die die Animation abzielt. Wenn Sie beispielsweise Offset animieren, sollte Ihr Ausdruck in einen Vector3-Typ aufgelöst werden.

### <a name="components-of-an-expression"></a>Bestandteile eines Ausdrucks

Beim Aufbau der mathematischen Beziehung eines Ausdrucks gibt es mehrere Kernkomponenten:

- Parameter – Werte, die konstante Werte darstellen oder Verweise auf andere Composition-Objekte.
- Mathematische Operatoren – Die typischen mathematischen Operatoren plus (+), minus (-), multiplizieren (*), dividieren (/), die Parameter zu einer Gleichung zusammenfügen. Ebenfalls enthalten sind bedingte Operatoren wie Größer als (>), Gleich (==), ternärer Operator (Bedingung ? ifTrue : ifFalse) etc.
- Mathematische Funktionen – Mathematische Funktionen/Verknüpfungen basierend auf System.Numerik. Eine vollständige Liste der unterstützten Funktionen finden Sie unter [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Ausdrücke unterstützen eine Reihe von Schlüsselwörtern – Spezielle Begriffe, die nur innerhalb des ExpressionAnimation-Systems eine eindeutige Bedeutung haben. Diese sind (zusammen mit der vollständigen Liste der mathematischen Funktionen) in der [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation)-Dokumentation aufgeführt.

### <a name="creating-expressions-with-expressionbuilder"></a>Erstellen von Ausdrücken mit ExpressionBuilder

Es gibt zwei Möglichkeiten, Expressions in der UWP-Apps zu erstellen:

1. Erstellen der Gleichung als Zeichenkette über die offizielle, öffentliche API.
1. Erstellen der Gleichung in einem typsicheren Objektmodell mit Hilfe des Open Source ExpressionBuilder-Tools. Siehe [Github-Quellcode und Dokumentation](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder).

In diesem Dokument werden wir unsere Ausdrücke mit Hilfe von ExpressionBuilder definieren.

### <a name="parameters"></a>Parameter

Parameter bilden das Kernstück eines Ausdrucks. Zwei Parametertypen stehen zur Verfügung:

- Konstanten: Dies sind Parameter, die System.Numeric-Variablen repräsentieren. Diese Parameter werden beim Start der Animation einmalig mit Werten belegt.
- Referenzen: Dies sind Parameter, die Verweise auf CompositionObjects darstellen – diese Parameter werden nach dem Start einer Animation fortlaufend aktualisiert.

Im Allgemeinen sind Referenzen dafür verantwortlich, wie sich die Ausgabe eines Ausdrucks dynamisch ändern kann. Wenn sich diese Referenzen ändern, ändert sich dadurch die Ausgabe des Ausdrucks. Wenn Sie Ihren Ausdruck mit Zeichenfolgen erstellen oder in einem Templating-Szenario verwenden (mit Ihrem Ausdruck für mehrere CompositionObjects), müssen Sie die Werte Ihrer Parameter benennen und festlegen. Weitere Informationen finden Sie im Abschnitt „Beispiel“.

### <a name="working-with-keyframeanimations"></a>Arbeiten mit KeyFrameAnimations

Ausdrücke können auch mit KeyFrameAnimations verwendet werden. In diesen Fällen möchten Sie einen Ausdruck verwenden, um den Wert eines KeyFrames zu einem bestimmten Zeitpunkt zu definieren – diese KeyFrame-Typen werden als ExpressionKeyFrames bezeichnet.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Im Gegensatz zu ExpressionAnimations werden ExpressionKeyFrames jedoch nur einmalig beim Start der KeyFrameAnimation ausgewertet. Beachten Sie, dass Sie in einer ExpressionAnimation nicht den Wert des KeyFrame übergeben, sondern eine Zeichenfolge (oder einen ExpressionNode, wenn Sie ExpressionBuilder verwenden).

## <a name="example"></a>Beispiel

Gehen wir nun ein Beispiel für die Verwendung von Ausdrücken durch. Es handelt sich um das PropertySet-Beispiel aus der Windows UI-Beispielgalerie. Wir betrachten den Ausdruck, der das Orbit-Bewegungsverhalten der blauen Kugel steuert.

![Um Kreis umlaufend](images/animation/orbit.gif)

Für die Gesamterfahrung spielen drei Komponenten eine Rolle:

1. Eine KeyFrameAnimation, die den Y-Offset des roten Balls animiert.
1. Ein PropertySet mit einer **Rotation**-Eigenschaft, die den Orbit steuert, animiert durch eine andere KeyFrameAnimation.
1. Eine ExpressionAnimation, die den Offset der blauen Kugel mit Bezug auf den Offset der roten Kugel und die Rotation-Eigenschaft steuert, um eine perfekte Umlaufbahn aufrechtzuerhalten.

Wir konzentrieren uns auf die in Schritt 3 definierte ExpressionAnimation. Wir werden auch die ExpressionBuilder-Klassen verwenden, um diesen Ausdruck zu konstruieren. Eine Kopie des Codes, der verwendet wird, um dieses Erlebnis über Zeichenfolgen zu erzeugen, ist am Ende zu finden.

In dieser Gleichung gibt es zwei Eigenschaften, die Sie aus dem PropertySet referenzieren müssen: die eine ist ein Mittelpunktversatz und die andere die Rotation.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

Als nächstes müssen Sie die Vector3-Komponente definieren, die die tatsächliche Rotation der Umlaufbahn berücksichtigt.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` ist eine Kurzschreibweise für "using" ExpressionBuilder.ExpressionFunctions definieren.

Schließlich kombinieren Sie diese Komponenten und referenzieren die Position der roten Kugel, um die mathematische Beziehung zu definieren.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

Was wäre, wenn Sie denselben Ausdruck verwenden wollten, aber mit zwei anderen Visuals 2 Sätze von kreisenden Kugel verwenden möchten? Mit CompositionAnimations können Sie die Animation wiederverwenden und mehrere CompositionObjects als Zielobjekt auswählen. Das einzige, was Sie ändern müssen, wenn Sie diesen Ausdruck für den zusätzlichen Orbit verwenden, ist der Verweis auf das Visual. Wir nennen das „Templating“.

In diesem Fall modifizieren Sie den zuvor erstellten Ausdruck. Anstatt eine Referenz auf das CompositionObject zu „abzurufen“, erstellen Sie eine Referenz mit einem Namen und vergeben dann andere Werte:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Hier ist der Code, wenn Sie Ihren Ausdruck mit Zeichenfolgen über die öffentliche API definiert haben.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
