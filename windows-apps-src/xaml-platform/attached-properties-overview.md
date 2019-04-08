---
description: Erläutert das Konzept einer angefügten Eigenschaft in XAML und bietet einige Beispiele.
title: Übersicht über angefügte Eigenschaften
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 265a99e7abbb9b2f4c3341f90aecc3661ce3ffbf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614855"
---
# <a name="attached-properties-overview"></a>Übersicht über angefügte Eigenschaften

Eine *angefügte Eigenschaft* ist ein XAML-Konzept. Mit angefügten Eigenschaften können zusätzliche Eigenschaft-Wert-Paare für ein Objekt festgelegt werden, aber die Eigenschaften sind nicht Teil der ursprünglichen Objektdefinition. Angefügte Eigenschaften werden in der Regel als eine spezialisierte Form der Abhängigkeitseigenschaft definiert, die keinen herkömmlichen Eigenschaftenwrapper im Objektmodell des Besitzertyps enthält.

## <a name="prerequisites"></a>Voraussetzungen

Es wird davon ausgegangen, dass Sie das Grundkonzept von Abhängigkeitseigenschaften verstehen und die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) gelesen haben.

## <a name="attached-properties-in-xaml"></a>Angefügte Eigenschaften in XAML

In XAML legen Sie angefügte Eigenschaften mithilfe der Syntax _AttachedPropertyProvider.PropertyName_ fest. Hier ist ein Beispiel dafür angegeben, wie Sie [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) in XAML festlegen können.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> Wir verwenden einfach [ **Canvas.Left** ](https://msdn.microsoft.com/library/windows/apps/hh759771) als Beispiel die Eigenschaft ohne vollständig erläutert, warum Sie es nutzen würden angefügt. Weitere Informationen darüber, wozu **Canvas.Left** dient und wie [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) untergeordnete Layoutelemente handhabt, finden Sie im Referenzthema zu [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) oder unter [Definieren von Layouts mit XAML](https://msdn.microsoft.com/library/windows/apps/mt228350).

## <a name="why-use-attached-properties"></a>Gründe für die Verwendung von angefügten Eigenschaften

Mit angefügten Eigenschaften können Sie die Codierungskonventionen umgehen, die zur Laufzeit die Weitergabe von Informationen zwischen verschiedenen Objekten in einer Beziehung verhindern können. Es ist natürlich möglich, Eigenschaften in eine allgemeine Basisklasse einzufügen, sodass jedes Objekt diese Eigenschaft abrufen und festlegen kann. Die reine Anzahl an Szenarien, in denen Sie diese Möglichkeit vielleicht nutzen möchten, bläht jedoch die Basisklassen mit freigabefähigen Eigenschaften auf. Es können sogar Fälle auftreten, in denen nur zwei von Hunderten von Nachfolgerelementen versuchen, eine Eigenschaft zu verwenden. Das ist kein optimaler Klassenentwurf. Daher ist es durch das Konzept der angefügten Eigenschaften möglich, dass ein Objekt einer Eigenschaft einen Wert zuweist, den die eigene Klassenstruktur nicht definiert. Die definierende Klasse kann diesen Wert zur Laufzeit aus untergeordneten Objekten lesen, nachdem die verschiedenen Objekte in einer Objektstruktur erstellt wurden.

Untergeordnete Elemente können beispielsweise angefügte Eigenschaften verwenden, um ihre übergeordneten Elemente darüber zu informieren, wie sie auf der Benutzeroberfläche dargestellt werden müssen. Dies ist bei der angefügten Eigenschaft [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) der Fall. **Canvas.Left** wird als angefügte Eigenschaft erstellt, da sie für Elemente, die in einem [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Element enthalten sind, und nicht für das **Canvas**-Element selbst festgelegt wird. Jedes mögliche untergeordnete Element verwendet dann **Canvas.Left** und [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772), um sein Layoutoffset innerhalb des übergeordneten **Canvas**-Layoutcontainerelements anzugeben. Durch angefügte Eigenschaften kann dieses Szenario verwendet werden, ohne dass das Basiselementobjekt mit zahlreichen Eigenschaften gefüllt wird, die jeweils nur für einen der vielen möglichen Layoutcontainer gelten. Stattdessen implementieren viele der Layoutcontainer ihren eigenen Satz an angefügten Eigenschaften.

Zum Implementieren der angefügten Eigenschaft definiert die [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Klasse ein statisches [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Feld mit dem Namen [**Canvas.LeftProperty**](https://msdn.microsoft.com/library/windows/apps/br209272). Dann stellt **Canvas** die Methoden [**SetLeft**](https://msdn.microsoft.com/library/windows/apps/br209273) und [**GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) als öffentliche Accessoren für die angefügte Eigenschaft bereit, um den Zugriff auf die XAML-Einstellung und den Laufzeitwert zu ermöglichen. Für XAML und für das Abhängigkeitssystem erfüllt dieser API-Satz ein Muster, das eine spezielle XAML-Syntax für angefügte Eigenschaften ermöglicht und den Wert im Abhängigkeitseigenschaftenspeicher speichert.

## <a name="how-the-owning-type-uses-attached-properties"></a>Verwenden angefügter Eigenschaften durch den besitzenden Typ

Obwohl angefügte Eigenschaften für ein beliebiges XAML-Element (oder zugrunde liegendes [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)) festgelegt werden können, bedeutet dies nicht automatisch, dass das Festlegen der Eigenschaft zu einem greifbaren Ergebnis führt oder jemals auf den Wert zugegriffen werden kann. Der Typ, der die angefügte Eigenschaft definiert, folgt in der Regel einem dieser Szenarien:

- Der Typ, der die angefügte Eigenschaft definiert, ist das übergeordnete Element in einer Beziehung von anderen Objekten. Die untergeordneten Objekte legen Werte für die angefügte Eigenschaft fest. Der Besitzertyp der angefügten Eigenschaft weist ein eigenes Verhalten auf. Er durchläuft die untergeordneten Elemente, ruft die Werte ab und verarbeitet diese Werte irgendwann in der Objektlebensdauer (Layoutaktion, [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) usw.)
- Der Typ, der die angefügte Eigenschaft definiert, wird für eine Vielzahl möglicher übergeordneter Elemente und Inhaltsmodelle als untergeordnetes Element verwendet. Bei den Informationen handelt es sich aber nicht unbedingt um Layoutinformationen
- Die angefügte Eigenschaft meldet Informationen an einen Dienst, nicht an ein anderes UI-Element.

Weitere Informationen zu diesen Szenarien und besitzenden Typen finden Sie im Abschnitt „Weitere Infos zu 'Canvas.Lef“ von [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md).

## <a name="attached-properties-in-code"></a>Angefügte Eigenschaften in Code

Angefügte Eigenschaften verfügen nicht über die typischen Eigenschaftenwrapper für einfachen Zugriff zum Abrufen und Festlegen wie andere Abhängigkeitseigenschaften. Das liegt daran, dass die angefügte Eigenschaft nicht unbedingt Teil des codezentrierten Objektmodells für Instanzen ist, in denen die Eigenschaft festgelegt wurde. (Es ist zulässig, aber ungewöhnlich, eine Eigenschaft zu definieren, bei der es sich sowohl um eine angefügte Eigenschaft handelt, die andere Typen für sich selbst festlegen kann, und die auch eine herkömmliche Eigenschaftennutzung für den besitzenden Typ aufweist.)

Es gibt zwei Möglichkeiten, eine angefügte Eigenschaft in Code festzulegen: Verwenden Sie das Eigenschaftensystem, oder verwenden Sie die XAML-Musteraccessoren. In Bezug auf das Endergebnis sind diese Techniken etwa gleich. Welche Sie verwenden, ist daher meist vom Codierungsstil abhängig.

### <a name="using-the-property-system"></a>Verwenden des Eigenschaftensystems

Angefügte Eigenschaften für Windows-Runtime werden als Abhängigkeitseigenschaften implementiert, damit die Werte vom Eigenschaftensystem im freigegebenen Abhängigkeitseigenschaftenspeicher gespeichert werden können. Daher stellen angefügte Eigenschaften eine Abhängigkeitseigenschafts-ID für die besitzende Klasse zur Verfügung.

Wenn Sie eine angefügte Eigenschaft im Code festlegen möchten, rufen Sie die [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)-Methode auf und übergeben das [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Feld, das als ID für diese angefügte Eigenschaft dient. (Sie übergeben auch den festzulegenden Wert.)

Wenn Sie den Wert einer angefügten Eigenschaft im Code abrufen möchten, rufen Sie die [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)-Methode auf und übergeben erneut das [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)-Feld, das als ID dient.

### <a name="using-the-xaml-accessor-pattern"></a>Verwenden des XAML-Accessormusters

Ein XAML-Prozessor muss Werte von angefügten Eigenschaften festlegen können, wenn XAML in einer Objektstruktur analysiert wird. Der Besitzertyp der angefügten Eigenschaft muss dedizierte Accessormethoden bereit, die mit dem Namen in das Formular implementieren **erhalten *** PropertyName* und **festgelegt *** PropertyName*. Diese dedizierten Accessormethoden stellen auch eine Möglichkeit dar, die angefügte Eigenschaft in Code abzurufen oder festzulegen. Im Hinblick auf den Code ähnelt eine angefügte Eigenschaft einem Sicherungsfeld, das anstelle von Eigenschaftenaccessoren über Methodenaccessoren verfügt, und dieses Sicherungsfeld kann für jedes Objekt vorhanden sein und muss nicht speziell definiert werden.

Im folgenden Beispiel ist dargestellt, wie Sie über die XAML-Accessor-API eine angefügte Eigenschaft in Code festlegen können. In diesem Beispiel ist `myCheckBox` eine Instanz der [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316)-Klasse. Die letzte Zeile ist der Code, der den Wert tatsächlich festlegt. Mit den Zeilen davor werden nur die Instanzen und ihre Beziehungen der untergeordneten Elemente eingerichtet. Die unkommentierte letzte Zeile ist die Syntax, wenn Sie das Eigenschaftensystem verwenden. Die kommentierte letzte Zeile ist die Syntax, wenn Sie das XAML-Accessormuster verwenden.

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>Benutzerdefinierte angefügte Eigenschaften

Codebeispiele zum Definieren benutzerdefinierter angefügter Eigenschaften und weitere Informationen zu den Szenarien für die Verwendung einer angefügten Eigenschaften finden Sie unter [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md).

## <a name="special-syntax-for-attached-property-references"></a>Spezielle Syntax für Verweise auf angefügte Eigenschaften

Der Punkt im Namen einer angefügten Eigenschaft ist ein wichtiger Teil des Identifikationsmusters. Es gibt manchmal Mehrdeutigkeiten, wenn eine Syntax oder Situation den Punkt mit einer anderen Bedeutung interpretiert. Ein Punkt wird beispielsweise als Objektmodellausnahme für einen Bindungspfad behandelt. In vielen Fällen solcher Mehrdeutigkeiten gibt es eine spezielle Syntax für eine angefügte Eigenschaft, womit der innere Punkt dennoch als das _owner_**.**_property_-Trennzeichen einer angefügten Eigenschaft analysiert wird.

- Wenn Sie eine angefügte Eigenschaft als Teil des Zielpfads für eine Animation angeben möchten, schließen Sie den Namen der angefügten Eigenschaft in Klammern („()“) ein, beispielsweise „(Canvas.Left)“. Weitere Informationen finden Sie unter [Property-path-Syntax](property-path-syntax.md).

> [!WARNING]
> Eine vorhandene Einschränkung der Windows-Runtime-XAML-Implementierung ist, dass Sie eine benutzerdefinierte, angefügte Eigenschaft animieren nicht möglich.

- So geben Sie eine angefügte Eigenschaft als die Zieleigenschaft für einen Ressourcenverweis aus einer Ressourcendatei, an **X: Uid-**, verwenden eine spezielle Syntax, die eine Code-Format, vollständig qualifizierter fügt **verwenden:** Deklaration innerhalb eckige Klammern ("\[\]"), um eine Unterbrechung absichtliche Bereich zu erstellen. Angenommen, es ist ein Element vorhanden `<TextBlock x:Uid="Title" />`, den Ressourcenschlüssel in der Ressourcendatei, dessen Ziel die **Canvas.Top** Wert für diese Instanz ist "Title.\[ Using:Windows.UI.Xaml.Controls\]Canvas.Top ". Weitere Informationen zu Ressourcendateien und XAML finden Sie unter [Schnellstart: Übersetzen von UI-Ressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329).

## <a name="related-topics"></a>Verwandte Themen

- [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md)
- [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md)
- [Definieren von Layouts mit XAML](https://msdn.microsoft.com/library/windows/apps/mt228350)
- [Schnellstart: Übersetzen von UI-Ressourcen](https://msdn.microsoft.com/library/windows/apps/hh943060)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)
- [**"GetValue"**](https://msdn.microsoft.com/library/windows/apps/br242359)
