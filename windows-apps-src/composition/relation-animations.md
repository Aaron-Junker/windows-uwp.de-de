---
title: Beziehungs basierte Animationen
description: Erfahren Sie, wie Sie expressionanimationen verwenden, um Beziehungs basierte Animationen zu erstellen, wenn Bewegung von einer Eigenschaft eines anderen Objekts abhängig ist.
ms.date: 10/16/2020
ms.topic: article
keywords: Windows 10, UWP, Animation
ms.localizationpriority: medium
ms.openlocfilehash: 75adcd2f762fd4314d7b852811760d523ef522aa
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152412"
---
# <a name="relation-based-animations"></a>Beziehungs basierte Animationen

Dieser Artikel bietet einen kurzen Überblick über die Erstellung von Beziehungs basierten Animationen mithilfe der Komposition expressionanimation.

## <a name="dynamic-relation-based-experiences"></a>Dynamische Beziehungs basierte Erfahrungen

Bei der Erstellung von Bewegungs Erlebnissen in einer APP gibt es Zeiten, in denen die Bewegung nicht Zeit basiert ist, sondern von einer Eigenschaft eines anderen Objekts abhängig ist. Keyframeanimationen sind nicht in der Lage, diese Arten von Bewegungs Erlebnissen sehr leicht auszudrücken. In diesen speziellen Fällen muss Motion nicht mehr diskret und vordefiniert sein. Stattdessen kann die Bewegung dynamisch basierend auf der Beziehung zu anderen Objekteigenschaften angepasst werden. Beispielsweise können Sie die Deckkraft eines Objekts auf der Grundlage seiner horizontalen Position animieren. Weitere Beispiele hierfür sind Bewegungsmöglichkeiten wie z. b. kurzkopfzeilen und die-Element

Diese Art von Bewegungsmöglichkeiten ermöglicht es Ihnen, Benutzeroberflächen zu erstellen, die eine größere Verbindung haben, statt sich als Singular und unabhängig zu fühlen. Für den Benutzer hat dies einen Eindruck von einer dynamischen Benutzeroberfläche.

![Zirkel Umlauf](images/animation/orbit.gif)

![Listenansicht mit "initiallax"](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Verwenden von expressionanimationen

Zum Erstellen von Beziehungs basierten Bewegungs Erlebnissen verwenden Sie den Typ expressionanimation. Expressionanimationen (oder Ausdrücke für Short) sind eine neue Art von Animation, mit der Sie eine mathematische Beziehung Ausdrücken können – eine Beziehung, die das System verwendet, um den Wert einer animierten Eigenschaft jedes Frame zu berechnen. Anders ausgedrückt: Ausdrücke sind einfach eine mathematische Gleichung, die den gewünschten Wert einer animierenden Eigenschaft pro Frame definiert. Ausdrücke sind eine sehr vielseitige Komponente, die in einer Vielzahl von Szenarios verwendet werden kann, einschließlich der folgenden:

- Relative Größe, Offset-Animationen.
- Kurzkopfzeilen, mit ScrollViewer. (Siehe [verbessern vorhandener ScrollViewer-Erfahrungen](scroll-input-animations.md).)
- Andocken von Punkten mit inertiamodifiers und interaktiontracker. (Siehe [Erstellen von Snap-Points mit trägheitmodifizierer](inertia-modifiers.md)

Beim Arbeiten mit expressionanimationen sind einige Dinge erwähnenswert:

- Niemals beenden – im Gegensatz zum gleich geordneten Keyframeanimation-Element haben Ausdrücke keine begrenzte Dauer. Da Ausdrücke mathematische Beziehungen sind, handelt es sich um Animationen, die ständig ausgeführt werden. Wenn Sie auswählen, haben Sie die Möglichkeit, diese Animationen anzuhalten.
- Das Ausführen, aber nicht immer das Auswerten der –-Leistung ist immer ein Problem mit Animationen, die ständig ausgeführt werden. Es ist jedoch nicht erforderlich, dass das System intelligent genug ist, dass der Ausdruck nur neu ausgewertet wird, wenn eine der Eingaben oder Parameter geändert wurde.
- Auflösen in den richtigen Objekttyp – da Ausdrücke mathematische Beziehungen sind, ist es wichtig sicherzustellen, dass die Gleichung, die den Ausdruck definiert, in denselben Typ der Eigenschaft aufgelöst wird, die Ziel der Animation ist. Wenn Sie z. b. den Offset animieren, sollte der Ausdruck in einen Vector3-Typ aufgelöst werden.

### <a name="components-of-an-expression"></a>Komponenten eines Ausdrucks

Beim Aufbau der mathematischen Beziehung eines Ausdrucks gibt es mehrere Kernkomponenten:

- Parameter – Werte, die Konstante Werte oder Verweise auf andere Kompositions Objekte darstellen.
- Mathematische Operatoren – die typischen mathematischen Operatoren Plus (+), minus (-), Multiplikation (*), Division (/), die Parameter zum bilden einer Gleichung zusammenfügen. Die enthaltenen sind auch bedingte Operatoren wie größer als (>), gleich (= =), ternärer Operator (Bedingung? IfTrue: IfFalse) usw.
- Mathematische Funktionen – mathematische Funktionen/Verknüpfungen auf der Grundlage von "System. Numerics". Eine vollständige Liste der unterstützten Funktionen finden Sie unter [expressionanimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Ausdrücke unterstützen auch eine Reihe von Schlüsselwörtern – spezielle Ausdrücke, die nur innerhalb des expressionanimation-Systems unterschiedliche Bedeutungen haben. Diese werden in der [expressionanimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation) -Dokumentation (zusammen mit der vollständigen Liste der mathematischen Funktionen) aufgelistet.

### <a name="creating-expressions-with-expressionbuilder"></a>Erstellen von Ausdrücken mit ExpressionBuilder

Es gibt zwei Optionen zum Entwickeln von Ausdrücken in der UWP-App:

1. Erstellen Sie die Gleichung als Zeichenfolge über die offizielle öffentliche API.
1. Erstellen Sie die Gleichung in einem typsicheren Objektmodell über das Tool ExpressionBuilder, das im [Windows Community Toolkit](/windows/communitytoolkit/animations/expressions)enthalten ist.

Für dieses Dokument definieren wir unsere Ausdrücke mithilfe von ExpressionBuilder.

### <a name="parameters"></a>Parameter

Parameter bilden den Kern eines Ausdrucks. Es gibt zwei Arten von Parametern:

- Konstanten: Dies sind Parameter, die typisierte System. numeric-Variablen darstellen. Diese Parameter erhalten ihre Werte einmal, wenn die Animation gestartet wird.
- Verweise: Hierbei handelt es sich um Parameter, die Verweise auf compositionobjects darstellen – diese Parameter erhalten kontinuierlich ihre Werte, nachdem eine Animation gestartet wurde.

Im Allgemeinen sind Verweise der Hauptaspekt, wie sich die Ausgabe eines Ausdrucks dynamisch ändern kann. Wenn sich diese Verweise ändern, ändert sich die Ausgabe des Ausdrucks als Ergebnis. Wenn Sie den Ausdruck mit Zeichen folgen erstellen oder in einem Vorlagen Szenario verwenden (verwenden Sie den Ausdruck, um mehrere compositionobjects als Ziel festzulegen), müssen Sie die Werte der Parameter benennen und festlegen. Weitere Informationen finden Sie im Abschnitt „Beispiel“.

### <a name="working-with-keyframeanimations"></a>Arbeiten mit keyframeanimationen

Ausdrücke können auch mit Keyframeanimation verwendet werden. In diesen Fällen möchten Sie einen Ausdruck verwenden, um den Wert eines Keyframes zu einem Zeitpunkt zu definieren – diese typkeyframes werden als expressionkeyframes bezeichnet.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Anders als bei expressionanimation werden expressionkeyframes jedoch nur einmal ausgewertet, wenn Keyframeanimation gestartet wird. Beachten Sie, dass Sie eine expressionanimation nicht als Wert für den Keyframe übergeben, sondern eine Zeichenfolge (oder ein ExpressionNode, wenn Sie ExpressionBuilder verwenden).

## <a name="example"></a>Beispiel

Sehen wir uns nun ein Beispiel für die Verwendung von Ausdrücken an, insbesondere das PropertySet-Beispiel aus dem Beispiel Katalog der Windows-Benutzeroberfläche. Wir betrachten den Ausdruck, der das Verhalten der Umlaufbewegung der blauen Kugel verwaltet.

![Zirkel Umlauf](images/animation/orbit.gif)

Es gibt drei Komponenten, die für die gesamte Darstellung verfügbar sind:

1. Eine Keyframeanimation, die den Y-Offset der roten Kugel animiert.
1. Ein PropertySet mit einer **Rotations** Eigenschaft, die die durch eine andere Keyframeanimation animierte Animation unterstützt.
1. Eine expressionanimation, die den Offset der blauen Kugel, die auf den roten Kugel Offset verweist, und die Rotations Eigenschaft steuert, um eine perfekte Umlaufbahn zu erhalten.

Wir konzentrieren uns auf die expressionanimation, die in #3 definiert ist. Wir verwenden auch die ExpressionBuilder-Klassen, um diesen Ausdruck zu erstellen. Eine Kopie des Codes, der zum Erstellen dieser Benutzer Darstellung über Zeichen folgen verwendet wird, wird am Ende aufgeführt.

In dieser Gleichung sind zwei Eigenschaften vorhanden, auf die vom PropertySet verwiesen werden muss. eine ist ein Mittelpunkt Offset, und die andere ist die Drehung.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

Als nächstes müssen Sie die Vector3-Komponente definieren, die die tatsächliche Umlauf Drehung berücksichtigt.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` ist eine Kurzschreibweise "using"-Notation zum Definieren von expressionfunctions.
>
> `using EF = Microsoft.Toolkit.Uwp.UI.Animations.Expressions.ExpressionFunctions;`

Kombinieren Sie schließlich diese Komponenten zusammen, und verweisen Sie auf die Position der roten Kugel, um die mathematische Beziehung zu definieren.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

Wenn Sie in einer hypothetischen Situation denselben Ausdruck verwenden möchten, aber mit zwei anderen visuellen Elementen, bedeutet das, dass zwei Sätze von umkreisenden Kreisen. Mit compositionanimationen können Sie die Animation wieder verwenden und mehrere compositionobjects als Ziel verwenden. Das einzige, was Sie ändern müssen, wenn Sie diesen Ausdruck für den zusätzlichen Umlauf Fall verwenden, ist der Verweis auf das visuelle Element. Diese Vorlage wird als "Vorlage" bezeichnet.

In diesem Fall ändern Sie den Ausdruck, den Sie zuvor erstellt haben. Anstatt einen Verweis auf das compositionobject zu erhalten, erstellen Sie einen Verweis mit einem Namen und weisen dann unterschiedliche Werte zu:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Dies ist der Code, wenn Sie Ihren Ausdruck mit Zeichen folgen über die öffentliche API definiert haben.

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
