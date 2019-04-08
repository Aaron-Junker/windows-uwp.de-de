---
description: In diesem Thema werden die XAML-Syntaxregeln und die Terminologie erläutert, mit der die in der XAML-Syntax geltenden Einschränkungen bzw. die verfügbaren Optionen beschrieben werden.
title: Anleitung zur XAML-Syntax
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e988582877a6aa4ca3cf88ba0a5d98aceb56939e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595055"
---
# <a name="xaml-syntax-guide"></a>Anleitung zur XAML-Syntax


In diesem Thema werden die XAML-Syntaxregeln und die Terminologie erläutert, mit der die in der XAML-Syntax geltenden Einschränkungen bzw. die verfügbaren Optionen beschrieben werden. Dieses Thema ist nützlich, wenn Sie die XAML-Programmiersprache noch nie verwendet haben, Ihre Kenntnisse in Bezug auf die Terminologie bzw. bestimmte Syntaxregeln auffrischen möchten oder an der Funktionsweise der XAML-Sprache interessiert sind und Hintergrundinformationen und Kontext benötigen.

## <a name="xaml-is-xml"></a>XAML ist XML

Extensible Application Markup Language (XAML) verfügt über eine einfache auf XML basierende Syntax, und definitionsgemäß muss gültiges XAML auch gültiges XML sein. Zudem verfügt XAML aber auch über eigene Syntaxkonzepte, die XML erweitern. Eine XML-Entität kann in reinem XML gültig sein, aber als XAML eine andere und komplexere Bedeutung haben. In diesem Thema werden diese XAML-Syntaxkonzepte erläutert.

## <a name="xaml-vocabularies"></a>XAML-Vokabular

Ein Punkt, in dem sich XAML von den meisten XML-Verwendungen unterscheidet, ist, dass XAML normalerweise nicht mit einem Schema durchgesetzt wird, z. B. einer XSD-Datei. Der Grund dafür ist, dass XAML erweiterbar sein soll, wie das „X“ in der Bezeichnung XAML anzeigt. Nach dem Analysieren von XAML wird erwartet, dass die Elemente und Attribute, auf die Sie im XAML-Code verweisen, in einer Unterstützungscodedarstellung vorhanden sind. Dies können entweder die Kerntypen sein, die von der Windows-Runtime definiert werden, oder Typen, mit denen die Windows-Runtime erweitert wird bzw. die darauf basieren. In der SDK-Dokumentation wird an einigen Stellen auf die Typen Bezug genommen, die bereits in die Windows-Runtime integriert sind und im XAML-Code als *XAML-Vokabular* für die Windows-Runtime verwendet werden können. Microsoft Visual Studio unterstützt Sie beim Erstellen von Markup, das innerhalb dieses XAML-Vokabulars gültig ist. Visual Studio kann auch Ihre benutzerdefinierten Typen für die XAML-Verwendung einbeziehen, solange im Projekt korrekt auf die Quelle dieser Typen verwiesen wird. Weitere Informationen zu XAML und benutzerdefinierten Typen finden Sie unter [XAML-Namespaces und Namespacezuordnung](xaml-namespaces-and-namespace-mapping.md).

##  <a name="declaring-objects"></a>Deklarieren von Objekten

Programmierer denken oft in Objekten und Membern, während eine Markupsprache aus Elementen und Attributen konzipiert ist. Ganz allgemein gesprochen, wird ein Element, das Sie im XAML-Markup deklarieren, zu einem Objekt in einer Sicherungslaufzeit-Objektdarstellung. Um ein Laufzeitobjekt für Ihre App zu erstellen, deklarieren Sie ein XAML-Element im XAML-Markup. Das Objekt wird erstellt, wenn die Windows-Runtime Ihren XAML-Code lädt.

Eine XAML-Datei enthält immer genau ein Stammelement. Dieses Stammelement deklariert ein Objekt, das als konzeptioneller Stamm einer Programmierstruktur (z. B. einer Seite) oder als Objektdiagramm der gesamten Laufzeitdefinition einer Anwendung fungiert.

Für die XAML-Syntax gibt es drei Möglichkeiten zum Deklarieren von Objekten in XAML:

-   **Verwenden direkt Objektelement-Syntax ein:** Dabei wird die öffnenden und schließenden Tags zum Instanziieren eines Objekts als XML-Format Element verwendet. Mit dieser Syntax können Sie Stammobjekte deklarieren oder geschachtelte Objekte erstellen, die Eigenschaftswerte festlegen.
-   **Indirekt mithilfe der Attributsyntax:** Dabei wird einen Inline-String-Wert, die Anweisungen zum Erstellen eines Objekts verwendet. Der XAML-Parser legt mithilfe dieser Zeichenfolge den Wert einer Eigenschaft auf einen neu erstellten Referenzwert fest. Diese Technik wird nur für bestimmte allgemeine Objekte und Eigenschaften unterstützt.
-   Mithilfe einer Markuperweiterung.

Dies bedeutet nicht, dass Sie zur Objekterstellung immer jede beliebige Syntax in einem XAML-Vokabular verwenden können. Einige Objekte können nur mithilfe der Objektelementsyntax erstellt werden. Andere Objekte können nur erstellt werden, indem Sie sie zu Beginn in einem Attribut festlegen. Tatsächlich sind Objekte, die mit der Objektelement- oder Attributsyntax erstellt werden können, im XAML-Vokabular vergleichsweise selten. Selbst wenn beide Syntaxformen möglich sind, ist eine von ihnen aufgrund des Stils allgemeiner.
Es gibt auch Techniken, mit denen Sie in XAML auf vorhandene Objekte verweisen können, anstatt neue Werte zu erstellen. Die vorhandenen Objekte können in anderen Bereichen von XAML definiert oder implizit durch ein Verhalten der Plattform und ihrer Anwendungs- oder Programmiermodelle vorhanden sein.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>Deklarieren eines Objekts mithilfe einer Objektelementsyntax

Wenn Sie ein Objekt mit einer Objektelementsyntax deklarieren möchten, schreiben Sie Tags wie die folgenden: `<objectName>  </objectName>`. Dabei ist *objectName* der Typname für das zu instanziierende Objekt. Mit der folgenden Objektelementsyntax wird ein [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267)-Objekt deklariert:

```xml
<Canvas>
</Canvas>
```

Falls das Objekt keine anderen Objekte enthält, können Sie das Objektelement mit einem selbstschließenden Tag deklarieren (anstelle eines öffnenden/schließenden Tagpaars): `<Canvas />`

### <a name="containers"></a>Container

Viele als UI-Elemente verwendete Objekte, wie [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), können andere Objekte enthalten. Solche Objekte werden auch als Container bezeichnet. Das folgende Beispiel zeigt einen **Canvas**-Container, der ein anderes Element enthält, ein [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Element.

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>Deklarieren eines Objekts mithilfe einer Attributsyntax

Da dieses Verhalten an die Festlegung von Eigenschaften gebunden ist, werden wir uns an späterer Stelle ausführlicher damit befassen.

### <a name="initialization-text"></a>Initialisierungstext

Für einige Objekte können Sie mithilfe von innerem Text, der als Initialisierungswert für die Erstellung verwendet wird, neue Werte deklarieren. Bei XAML wird diese Technik und Syntax als *Initialisierungstext* bezeichnet. Vom Konzept her ist Initialisierungstext vergleichbar mit dem Aufruf eines Konstruktors mit Parametern. Initialisierungstext eignet sich zum Festlegen der Anfangswerte bestimmter Strukturen.

Wenn ein Strukturwert einen **x:Key** enthalten soll, damit er in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) eingesetzt werden kann, wird häufig eine Objektelementsyntax mit Initialisierungstext verwendet. Dies kann der Fall sein, wenn Sie den Strukturwert für mehrere Zieleigenschaften freigeben. Bei einigen Strukturen können Sie keine Attributsyntax zum Festlegen der Strukturwerte verwenden: Initialisierungstext ist dann die einzige Möglichkeit, um eine verwendbare und freigabefähige [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343)-, [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864)-, [**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754)- oder [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723)-Ressource zu erstellen.

Im folgenden gekürzten Beispiel wird Initialisierungstext verwendet, um Werte für [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) anzugeben. In diesem Fall werden Werte angegeben, mit denen **Left** und **Right** auf 20 sowie **Top** und **Bottom** auf 10 festgelegt werden. In diesem Beispiel wird **Thickness** als Schlüsselressource und dann der Verweis auf diese Ressource dargestellt. Weitere Informationen zu Initialisierungstext für [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864) finden Sie unter [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864).

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**Beachten Sie**  einige Strukturen können nicht als Objektelement deklariert werden. Initialisierungstext wird nicht unterstützt, und die Strukturen können nicht als Ressourcen verwendet werden. Sie müssen eine Attributsyntax verwenden, um in XAML Eigenschaften auf diese Werte festzulegen. Diese Typen sind: [**Dauer**](https://msdn.microsoft.com/library/windows/apps/br242377), [ **RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411), [ **Punkt**](https://msdn.microsoft.com/library/windows/apps/br225870), [ **Rect**  ](https://msdn.microsoft.com/library/windows/apps/br225994) und [ **Größe**](https://msdn.microsoft.com/library/windows/apps/br225995).

## <a name="setting-properties"></a>Festlegen von Eigenschaften

Sie können Eigenschaften für von Ihnen deklarierte Objekte mithilfe einer Objektelementsyntax festlegen. Es gibt verschiedenen Möglichkeiten, um Eigenschaften in XAML festzulegen.

-   Mithilfe der Attributsyntax.
-   Mithilfe der Eigenschaftselementsyntax.
-   Mithilfe der Elementsyntax, bei der der Inhalt (enthaltener Text oder untergeordnetes Element) die XAML-Inhaltseigenschaft eines Objekts festlegt.
-   Mithilfe einer Sammlungssyntax (bei der es sich in der Regel um die implizite Sammlungssyntax handelt).

Wie bei der Objektdeklaration gilt auch hier, dass nicht alle Eigenschaften mit jeder dieser Techniken festgelegt werden können. Einige Eigenschaften unterstützen nur eine der Techniken.
Manche Eigenschaften unterstützen mehrere Techniken. Es gibt z. B. Eigenschaften, für die die Eigenschaftselementsyntax oder die Attributsyntax verwendet werden kann. Was möglich ist, hängt von der Eigenschaft und dem von ihr verwendeten Objekttyp ab. In der Windows-Runtime-API-Referenz sind die möglichen XAML-Verwendungen im Abschnitt **Syntax** aufgeführt. In einigen Fällen ist eine alternative Verwendungsmöglichkeit verfügbar, die jedoch umfangreicher ist. Diese umfangreichen Verwendungen werden nicht immer angezeigt, weil wir Ihnen die bewährten Methoden bzw. die realen Szenarien zum Einsetzen der Eigenschaft im XAML-Code zeigen möchten. Anleitungen zur XAML-Syntax finden Sie in den Abschnitten zur **XAML-Verwendung** der Referenzseiten für Eigenschaften, die in XAML festgelegt werden können.

Einige Eigenschaften für Objekte können in XAML gar nicht festgelegt werden, sondern nur mithilfe von Code. In der Regel sind dies Eigenschaften, die sich besser für CodeBehind eignen als für XAML.

Schreibgeschützte Eigenschaften können in XAML nicht festgelegt werden. Selbst im Code müsste der besitzende Typ eine andere Festlegungsmethode unterstützen, z. B. eine Konstruktorüberladung, eine Hilfsmethode oder eine berechnete Eigenschaft. Eine berechnete Eigenschaft basiert auf den Werten anderer festlegbarer Eigenschaften und in machen Fällen auf einem Ereignis mit integrierter Behandlung. Diese Features sind im Abhängigkeitseigenschaftensystem verfügbar. Weitere Informationen zur Unterstützung berechneter Eigenschaften mithilfe von Abhängigkeitseigenschaften finden Sie unter [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md).

Die Sammlungssyntax in XAML vermittelt den Eindruck, dass Sie eine schreibgeschützte Eigenschaft festlegen, was jedoch nicht der Fall ist. Informationen dazu finden Sie weiter unten in diesem Thema unter "Festlegen einer Eigenschaft mithilfe einer Sammlungssyntax".

### <a name="setting-a-property-by-using-attribute-syntax"></a>Festlegen einer Eigenschaft mithilfe einer Attributsyntax

Das Festlegen eines Attributwerts ist eine typische Möglichkeit, einen Eigenschaftenwert in einer Markupsprache festzulegen, beispielsweise in XML oder HTML. Das Festlegen von XAML-Attributen ähnelt dem Festlegen von Attributwerten in XML. Der Attributname wird an einer beliebigen Stelle innerhalb der Tags nach dem Elementnamen angegeben und durch mindestens ein Leerzeichen vom Elementnamen getrennt. Auf den Attributnamen folgt ein Gleichheitszeichen. Der Attributwert steht in einem Anführungszeichenpaar. Dies können doppelte oder einfache Anführungszeichen sein, solange sie übereinstimmen und den Wert umschließen. Der Attributwert selbst muss als Zeichenfolge ausgedrückt werden. Die Zeichenfolge enthält oft Zahlen. Bis der XAML-Parser ins Spiel kommt und eine einfache Wertkonvertierung durchführt, sind in XAML jedoch alle Attributwerte Zeichenfolgenwerte.

In diesem Beispiel wird eine Attributsyntax für vier Attribute verwendet, um die Eigenschaften [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735), [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Objekts festzulegen.

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>Festlegen einer Eigenschaft mithilfe einer Eigenschaftselementsyntax

Viele Eigenschaften eines Objekts können mithilfe einer Eigenschaftselementsyntax festgelegt werden. Ein Eigenschaftselement sieht wie folgt aus: `<`*object*`.`*property*`>`.

Wenn Sie eine Eigenschaftselementsyntax verwenden möchten, erstellen Sie XAML-Eigenschaftselemente für die Eigenschaft, die Sie festlegen möchten. Bei Standard-XML würde dieses Element einfach nur als Element mit einem Punkt in seinem Namen interpretiert werden. Bei XAML identifiziert der Punkt im Elementnamen das Element jedoch als Eigenschaftselement, wobei erwartet wird, dass *property* ein Member von *object* in einer Sicherungsobjektmodell-Implementierung ist. Wenn Sie eine Eigenschaftselementsyntax verwenden möchten, muss ein Objektelement angegeben werden können, um die Eigenschaftselementtags zu "füllen". Ein Eigenschaftselement hat immer einen Inhalt (ein Element, mehrere Elemente oder inneren Text). Ein selbstschließendes Eigenschaftselement macht daher keinen Sinn.

In der folgenden Grammatik ist *property* der Name der Eigenschaft, die Sie festlegen möchten, und *propertyValueAsObjectElement* ist ein einzelnes Objektelement, das die Anforderungen bezüglich des Werttyps der Eigenschaft erfüllt.

`<`*Objekt*`>`

`<`*Objekt*`.`*Eigenschaft*`>`

*propertyValueAsObjectElement*

`</`*Objekt*`.`*Eigenschaft*`>`

`</`*Objekt*`>`

Im folgenden Beispiel wird eine Eigenschaftselementsyntax verwendet, um [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) von [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) mit einem [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)-Objektelement festzulegen. (In der **SolidColorBrush**, [ **Farbe** ](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) als Attribut festgelegt ist.) Das analysierte Ergebnis dieses XAML ist identisch mit dem vorherigen XAML-Beispiel, das festgelegt **füllen** mithilfe der Attributsyntax.

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>XAML-Vokabular und objektorientierte Programmierung

Als XAML-Member eines Windows-Runtime-XAML-Typs verwendete Eigenschaften und Ereignisse werden oft von Basistypen geerbt. Betrachten Sie das folgende Beispiel: `<Button Background="Blue" .../>`. Die [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395)-Eigenschaft ist keine direkt deklarierte Eigenschaft der [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265)-Klasse. Stattdessen wird **Background** von der [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390)-Basisklasse geerbt. In der Tat, wenn Sie im Referenzthema für betrachten **Schaltfläche** sehen Sie, dass der Member enthält mindestens einen geerbten Member aus jeder von einer Kette von aufeinander folgenden Basis-Klassen enthalten: [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736), [ **Steuerelement**](https://msdn.microsoft.com/library/windows/apps/br209390), [ **"FrameworkElement"**](https://msdn.microsoft.com/library/windows/apps/br208706), [  **"UIElement"**](https://msdn.microsoft.com/library/windows/apps/br208911), [ **DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Im Sinne des XAML-Vokabulars sind alle Lese-/Schreibeigenschaften und Sammlungseigenschaften in der Liste **Eigenschaften** geerbt. Ereignisse (wie z. B. die verschiedenen [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Ereignisse) sind ebenfalls geerbt.

Wenn Sie die Windows-Runtime-Referenz als XAML-Leitfaden verwenden, gilt der in einer Syntax oder sogar im Beispielcode gezeigte Elementname mitunter für den Typ, der die Eigenschaft ursprünglich definiert. Dies liegt daran, dass das Referenzthema alle möglichen Typen abdeckt, die von einer Basisklasse erben. Wenn Sie das IntelliSense-Feature für XAML von Visual Studio im XML-Editor verwenden, wird die Vererbung – nachdem Sie mit einem Objektelement für eine Klasseninstanz begonnen haben – in IntelliSense und den zugehörigen Dropdownlisten zusammengeführt, sodass Ihnen eine genaue Liste der festlegbaren Attribute zur Verfügung steht.

### <a name="xaml-content-properties"></a>XAML-Inhaltseigenschaften

Einige Typen definieren eine ihrer Eigenschaften so, dass die Eigenschaft eine XAML-Inhaltssyntax ermöglicht. Bei der XAML-Inhaltseigenschaft eines Typs können Sie das Eigenschaftselement für diese Eigenschaft weglassen, wenn Sie sie in XAML angegeben. Sie können die Eigenschaft auch auf einen inneren Textwert festlegen, indem Sie diesen inneren Text direkt in den Objektelementtags des besitzenden Typs angeben. XAML-Inhaltseigenschaften unterstützen eine einfache Markupsyntax für diese Eigenschaft und verbessern durch die geringere Schachtelung die Lesbarkeit des XAML-Codes.

Wenn eine XAML-Inhaltssyntax verfügbar ist, ist sie in der Windows-Runtime-Referenzdokumentation unter **Syntax** in den XAML-Abschnitten für die jeweilige Eigenschaft angegeben. Auf der [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258)-Eigenschaftenseite für [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) wird z. B. anstelle einer Eigenschaftselementsyntax eine XAML-Inhaltssyntax verwendet, um den **Border.Child**-Einzelobjektwert von **Border** wie folgt festzulegen:

```xml
<Border>
  <Button .../>
</Border>
```

Wenn es sich bei der als XAML-Inhaltseigenschaft deklarierten Eigenschaft um den **Object**- oder **String**-Typ handelt, unterstützt die XAML-Inhaltssyntax das, was im XML-Dokumentmodell im Grunde der innere Text ist: eine Zeichenfolge zwischen den öffnenden und schließenden Objekttags. Die [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676)-Eigenschaftenseite für [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) zeigt z. B. eine XAML-Inhaltssyntax mit einem inneren Textwert zum Festlegen von **Text**, die Zeichenfolge „Text“ erscheint aber niemals im Markup. Beispiel zur Verwendung:

```xml
<TextBlock>Hello!</TextBlock>
```

Wenn eine XAML-Inhaltseigenschaft für eine Klasse vorhanden ist, ist dies im Referenzthema der Klasse im Abschnitt "Attribute" angegeben. Suchen Sie nach dem Wert des [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011). Dieses Attribut verwendet ein benanntes Feld „Name“. Der Wert von „Name“ ist der Name der Eigenschaft dieser Klasse, d. h. die XAML-Inhaltseigenschaft. Z. B. auf die [ **Rahmen** ](https://msdn.microsoft.com/library/windows/apps/br209250) Referenzseite, zeige ich dies: ContentProperty("Name=Child").

Eine erwähnenswerte XAML-Syntaxregel ist, dass die XAML-Inhaltseigenschaft nicht mit anderen Eigenschaftselementen vermischt werden kann, die Sie für das Element festlegen. Die XAML-Inhaltseigenschaft muss entweder komplett vor oder komplett nach allen Eigenschaftselementen festgelegt werden. Folgende XAML-Syntax ist z. B. ungültig:

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>Sammlungsyntax

Mit allen bisher dargestellten Syntaxen werden Eigenschaften für einzelne Objekte festgelegt. Aber viele Benutzeroberflächenszenarios erfordern jedoch, dass ein angegebenes übergeordnetes Element mehrere untergeordnete Elemente aufweisen kann. Eine Benutzeroberfläche für ein Eingabeformular erfordert beispielsweise verschiedene Textfeldelemente, einige Beschriftungen und möglicherweise die Schaltfläche „Übermitteln“. Wenn Sie für den Zugriff auf diese verschiedenen Elemente ein Programmierobjektmodell verwenden würden, wären diese in der Regel Elemente in einer einzelnen Sammlungseigenschaft und nicht jedes Element der Wert verschiedener Eigenschaften. XAML unterstützt mehrere untergeordnete Elemente sowie ein typisches Sicherungssammlungsmodell, indem Eigenschaften, die einen Sammlungstyp verwenden, als implizit behandelt werden und eine spezielle Verarbeitung für die untergeordneten Elemente eines Sammlungstyps ausgeführt wird.

Viele Sammlungseigenschaften werden auch als XAML-Inhaltseigenschaft für die Klasse identifiziert. Die Kombination aus impliziter Sammlungsverarbeitung und XAML-Inhaltssyntax ist oft bei Typen zu finden, die zum Zusammensetzen von Steuerelementen wie Panels, Ansichten oder ItemsControls verwendet werden. Die folgenden Beispiele zeigen die einfachste XAML-Syntax zum Zusammensetzen von zwei Peer-UI-Elementen in einem [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>Der Mechanismus einer XAML-Sammlungssyntax

Zunächst scheint es möglicherweise so, dass XAML einen "Satz" der schreibgeschützten Sammlungseigenschaft ermöglicht. Tatsächlich ermöglicht XAML hier jedoch das Hinzufügen von Elementen zu einer vorhandenen Sammlung. Die XAML-Sprache und XAML-Prozessoren, die die XAML-Unterstützung implementieren, sind von einer Konvention in Sicherungssammlungstypen abhängig, um diese Syntax zu ermöglichen. In der Regel gibt es eine Sicherungseigenschaft wie einen Indexer oder eine **Items**-Eigenschaft, der bzw. die auf bestimmte Elemente der Sammlung verweist. Im Allgemeinen ist diese Eigenschaft in XAML-Syntax nicht explizit. Bei Sammlungen ist der zugrunde liegende Mechanismus für die XAML-Analyse keine Eigenschaft, sondern eine Methode: in den meisten Fällen die **Add**-Methode. Wenn der XAML-Prozessor ein oder mehrere Objektelemente in einer XAML-Sammlungssyntax ermittelt, wird jedes dieser Objekte zunächst aus einem Element erstellt, und dann wird jedes neue Objekt durch Aufrufen der **Add**-Methode nacheinander der enthaltenden Sammlung hinzugefügt.

Wenn ein XAML-Parser einer Sammlung Elemente hinzufügt, bestimmt die Logik der **Add**-Methode, ob ein angegebenes XAML-Element ein zulässiges untergeordnetes Element des Sammlungsobjekts ist. Viele Sammlungstypen sind stark durch die Sicherungsimplementierung typisiert. Das bedeutet, dass der Eingabeparameter von **Add** erwartet, dass der Typ des übergebenen Elements mit dem **Add**-Parametertyp übereinstimmt.

Besondere Vorsicht ist geboten, wenn Sie eine Sammlungseigenschaft explizit als Objektelement angeben. Ein XAML-Parser erstellt jedes Mal ein neues Objekt, wenn er auf ein Objektelement trifft. Ist die gewünschte Sammlungseigenschaft schreibgeschützt, kann dies zu einer XAML-Analyseausnahme führen. Diese Ausnahme lässt sich vermeiden, indem Sie die Sammlungssyntax verwenden.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>Wann sollte eine Attributsyntax oder eine Eigenschaftselementsyntax verwendet werden

Alle Eigenschaften, die in XAML festgelegt werden können, unterstützen die Attributsyntax oder die Eigenschaftselementsyntax zum direkten Festlegen von Werten, aber möglicherweise nicht beide Syntaxen. Einige Eigenschaften unterstützen eine der beiden Syntaxen, während andere zusätzliche Syntaxoptionen wie die XAML-Inhaltseigenschaft unterstützen. Der von einer Eigenschaft unterstützte XAML-Syntaxtyp hängt vom Objekttyp ab, der von der Eigenschaft als Eigenschaftstyp verwendet wird. Wenn es sich beim Eigenschaftstyp um einen primitiven Typ wie "Double" ("Float" oder "Decimal"), "Integer", "Boolean" oder "String" handelt, unterstützt die Eigenschaft immer eine Attributsyntax.

Sie können eine Eigenschaft auch mit einer Attributsyntax festlegen, wenn der zum Festlegen der Eigenschaft verwendete Objekttyp durch Verarbeiten einer Zeichenfolge erstellt werden kann. Bei primitiven Typen ist die Typkonvertierung immer in den Parser integriert. Bestimmte andere Objekttypen können aber auch mithilfe einer als Attributwert angegebenen Zeichenfolge erstellt werden (anstelle eines Objektelements innerhalb eines Eigenschaftselements). Damit dies möglich ist, muss eine zugrunde liegende Typkonvertierung vorhanden sein, die entweder von der jeweiligen Eigenschaft oder allgemein für alle Werte mit diesem Eigenschaftstyp unterstützt wird. Anhand des Zeichenfolgenwerts des Attributs werden Eigenschaften festgelegt, die für die Initialisierung des neuen Objekts wichtig sind. Unter Umständen kann ein spezieller Typkonverter auch verschiedene Unterklassen eines allgemeinen Eigenschaftstyps erstellen. Dies hängt allerdings davon ab, wie er die Informationen in der Zeichenfolge einzeln verarbeitet. Für Objekttypen, die dieses Verhalten unterstützen, ist eine spezielle Grammatik im Syntaxabschnitt der Referenzdokumentation aufgeführt. Die XAML-Syntax für [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) zeigt z. B., wie mit einer Attributsyntax ein neuer [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962)-Wert für eine Eigenschaft vom Typ **Brush** erstellt werden kann (und es gibt viele **Brush**-Eigenschaften in der Windows-Runtime-XAML).

## <a name="xaml-parsing-logic-and-rules"></a>XAML-Analyselogik und -Regeln

Mitunter ist es aufschlussreich, die XAML-Syntax so zu lesen, wie sie von einem XAML-Parser gelesen werden muss: als ein Satz linear geordneter Zeichenfolgentokens. Ein XAML-Parser muss diese Tokens anhand eines Regelsatzes interpretieren, der Teil der XAML-Definition ist.

Das Festlegen eines Attributwerts ist eine typische Möglichkeit, einen Eigenschaftenwert in einer Markupsprache festzulegen, beispielsweise in XML oder HTML. In der folgenden Syntax stellt *objectName* das Objekt, das Sie instanziieren möchten, *propertyName* den Namen der Eigenschaft, die Sie für das Objekt festlegen möchten, und *propertyValue* den festzulegenden Wert dar.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

Mit jeder Syntax können Sie ein Objekt deklarieren und eine Eigenschaft für dieses Objekt festlegen. Auch wenn es sich beim ersten Beispiel um ein einzelnes Element in einem Markup handelt, gibt es hier tatsächlich unterschiedliche Schritte in Bezug darauf, wie ein XAML-Prozessor dieses Markup analysiert.

Zunächst gibt das Vorhandensein des Objektelements an, dass ein neues *objectName*-Objekt instanziiert werden muss. Erst nachdem eine solche Instanz vorhanden ist, kann die Instanzeigenschaft *propertyName* dafür festgelegt werden.

Eine weitere XAML-Regel ist, dass es möglich sein muss, Attribute eines Elements in beliebiger Reihenfolge festzulegen. Zwischen `<Rectangle Height="50" Width="100" />` und `<Rectangle Width="100"  Height="50" />` besteht z. B. kein Unterschied. Welche Reihenfolge Sie verwenden, ist eine Frage des Programmierstils.

**Beachten Sie**  XAML-Designer sortieren Konventionen häufig höher stufen, wenn Sie Entwurfsoberflächen als XML-Editor verwenden, aber Sie frei, XAML später bearbeiten können, um die Attribute neu anordnen oder neue einführen.

## <a name="attached-properties"></a>Angefügte Eigenschaften

XAML erweitert XML durch ein als *angefügte Eigenschaft* bezeichnetes Syntaxelement. Ähnlich wie die Eigenschaftselementsyntax enthält die Syntax der angefügten Eigenschaft einen Punkt, und der Punkt hat eine spezielle Bedeutung für die XAML-Analyse. Der Punkt trennt insbesondere den Besitzeranbieter der angefügten Eigenschaft und den Eigenschaftennamen.

In XAML legen Sie angefügte Eigenschaften mithilfe der Syntax *AttachedPropertyProvider*.*PropertyName* fest. Hier ist ein Beispiel, wie Sie die angefügte Eigenschaft [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) in XAML festlegen können:

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Sie können die angefügte Eigenschaft für Elemente festlegen, die im Unterstützungstyp keine Eigenschaft mit diesem Namen aufweisen – insofern funktioniert eine angefügte Eigenschaft ähnlich wie eine globale Eigenschaft oder ein durch einen anderen XML-Namespace festgelegtes Attribut (z. B. das **xml:space**-Attribut).

In Windows-Runtime-XAML sind angefügte Eigenschaften verfügbar, die folgende Szenarien unterstützen:

-   Untergeordnete Elemente können übergeordneten Container Bereichen informieren, wie sie im Layout Verhalten soll: [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [ **Raster**](https://msdn.microsoft.com/library/windows/apps/br242704), [ **VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651).
-   Steuerelement Verwendungen können es sich um eine wichtige Steuerelementteil Verhalten beeinflussen, die von der Steuerelementvorlage stammt: [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), [ **"VirtualizingStackPanel"**](https://msdn.microsoft.com/library/windows/apps/br227689).
-   Verwenden einen Dienst, der in einer entsprechenden Klasse verfügbar ist, nicht den Dienst und die Klasse, die sie verwendet Vererbung weiterleiten: [**Typografie**](https://msdn.microsoft.com/library/windows/apps/hh702143), [ **VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021), [ **AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081), [ **ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609).
-   Als Animationsziel: [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490).

Weitere Informationen finden Sie unter [Übersicht über angefügte Eigenschaften](attached-properties-overview.md).

## <a name="literal--values"></a>"{"-Literalwerte

Da die öffnende geschweifte Klammer Symbol \{ ist das Öffnen der Markuperweiterungssequenz, Sie verwenden eine Escape-Sequenz an einen literalen Zeichenfolgenwert, der mit "\{". Die-Escapesequenz ist "\{\}". Z. B. um einen Zeichenfolgenwert anzugeben, die eine einzelne Klammer ist, geben Sie den Wert des Attributs als "\{\}\{". Können Sie auch die alternativen Anführungszeichen (z. B. eine **"** in einem Attribut gesetzte **" "**) zu einer"\{"Werts als Zeichenfolge.

**Beachten Sie**  "\\}" funktioniert auch, wenn es in ein Attribut in Anführungszeichen ist.
 
## <a name="enumeration-values"></a>Enumerationswerte

Viele Eigenschaften in der Windows-Runtime-API verwenden als Werte Enumerationen. Wenn es sich beim Member um eine Lese-/Schreibeigenschaft handelt, können Sie die Eigenschaft durch Angeben eines Attributwerts festlegen. Den als Eigenschaftswert zu verwendenden Enumerationswert geben Sie anhand des nicht qualifizierten Namens des Konstantennamens an. Folgendes Beispiel zeigt, wie Sie [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) in XAML festlegen: `<Button Visibility="Visible"/>`. Hier ist das „Visible“-Element als Zeichenfolge direkt einer benannten Konstante der [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006)-Enumeration zugeordnet, **Visible**.

-   Verwenden Sie keine qualifizierte Form, weil dies nicht funktioniert. Folgende XAML-Syntax ist z. B. ungültig: `<Button Visibility="Visibility.Visible"/>`.
-   Verwenden Sie nicht den Wert der Konstante. Anders ausgedrückt: Verlassen Sie sich nicht auf den ganzzahligen Wert der Enumeration, die explizit oder implizit von der Definitionsweise der Enumeration abhängt. Auch wenn dies scheinbar funktioniert, ist in XAML bzw. im Code davon abzuraten, weil Sie sich auf ein potenziell kurzlebiges Implementierungsdetail verlassen. Verwenden Sie beispielsweise nicht Folgendes: `<Button Visibility="1"/>`.

**Beachten Sie**  Referenzthemen für APIs, die XAML verwenden, und Verwenden von Enumerationen, klicken Sie auf den Link in den Enumerationstyp, in der **Eigenschaftswert** Abschnitt **Syntax**. So gelangen Sie auf die Enumerationsseite, auf der Sie die benannten Konstanten für diese Enumeration ermitteln können.

Enumerationen können flagspezifisch sein. In diesem Fall wird ihnen **FlagsAttribute** zugeordnet. Wenn Sie eine Kombination von Werten für eine flagspezifische Enumeration als XAML-Attributwert angeben müssen, verwenden Sie die Namen der einzelnen Enumerationskonstanten getrennt durch Kommas (,) und ohne Leerzeichen. Flagspezifische Attribute kommen im Windows-Runtime-XAML-Vokabular nicht häufig vor, aber [**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934) ist ein Beispiel für einen Fall, in dem das Festlegen eines flagspezifischen Enumerationswerts im XAML-Code unterstützt wird.

## <a name="interfaces-in-xaml"></a>Schnittstellen in XAML

In seltenen Fällen liegt eine XAML-Syntax vor, in der der Typ einer Eigenschaft eine Schnittstelle darstellt. Im XAML-Typsystem ist ein Typ, der diese Schnittstelle implementiert, bei der Analyse als Wert zulässig. Eine erstellte Instanz eines solchen Typs muss verfügbar sein, die als Wert fungieren kann. Eine als ein Typ in der XAML-Syntax verwendete Schnittstelle für die Eigenschaften [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) und [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) von [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) wird angezeigt. Diese Eigenschaften unterstützen Model-View-ViewModel (MVVM)-Entwurfsmuster, in denen die **ICommand**-Schnittstelle den Vertrag für die Interaktionsweise der Ansichten und Modelle darstellt.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>XAML-Platzhalterkonventionen in der Windows-Runtime-Referenz

Wenn Sie den Abschnitt **Syntax** der Referenzthemen für Windows-Runtime-APIs überprüft haben, die XAML verwenden können, ist Ihnen wahrscheinlich aufgefallen, dass die Syntax mehrere Platzhalter enthält. XAML-Syntax unterscheidet sich von der C#, Microsoft Visual Basic oder Visual C++-komponentenerweiterungen (C++ / CX) Syntax, da die XAML-Syntax eine Syntax für die Verwendung ist. Sie gibt einen Hinweis auf die spätere Verwendung in Ihren XAML-Dateien, gibt jedoch nicht zu genau vor, welche Werte verwendet werden können. Die Verwendung beschreibt also in der Regel einen Grammatiktyp, bei dem Literale und Platzhalter gemischt werden, und definiert einige der Platzhalter im Abschnitt **XAML-Werte**.

Wenn in einer XAML-Syntax für eine Eigenschaft Typ-/Elementnamen angezeigt werden, steht der angezeigte Name für den Typ, der die Eigenschaft ursprünglich definiert. Windows-Runtime-XAML unterstützt jedoch ein Klassenvererbungsmodell für die [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-basierten Klassen. Sie können also häufig ein Attribut für eine Klasse verwenden, bei der es sich nicht direkt um die definierende Klasse handelt, sondern die von einer Klasse abgeleitet ist, mit der die Eigenschaft bzw. das Attribut zuerst definiert wurde. Sie können beispielsweise unter Verwendung einer tiefen Vererbung [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) als Attribut für eine beliebige abgeleitete [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)-Kasse festlegen. Beispiel: `<Button Visibility="Visible" />`. Nehmen Sie den in einer XAML-Verwendungssyntax angezeigten Elementnamen also nicht zu wörtlich. Die Syntax ist ggf. für Elemente, die die Klasse darstellen, sowie für Elemente geeignet, die eine abgeleitete Klasse darstellen. Wenn der als das definierende Element angezeigte Typ selten oder gar nicht in einer realen Verwendung eingesetzt werden kann, wird dieser Typname in der Syntax absichtlich in Kleinbuchstaben angegeben. Die für **UIElement.Visibility** angezeigte Syntax lautet beispielsweise folgendermaßen:

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

Viele XAML-Syntaxabschnitte enthalten Platzhalter unter "Verwendung", die dann im Abschnitt **XAML-Werte** definiert werden (direkt unterhalb des Abschnitts **Syntax**).

In den XAML-Verwendungsabschnitten werden ebenfalls verschiedene generalisierte Platzhalter verwendet. Diese Platzhalter werden nicht jedes Mal unter **XAML-Werte** neu definiert, da Sie irgendwann wissen, wofür sie stehen. Die meisten Leser bevorzugen es unserer Meinung nach, wenn sie nicht immer wieder unter **XAML-Werte** aufgeführt werden. Daher haben wir sie nicht in die Definitionen aufgenommen. Zu Referenzzwecken finden Sie im Folgenden eine Liste mit einigen dieser Platzhalter und Informationen zu ihrer allgemeinen Bedeutung:

-   *object*: Dies ist theoretisch ein beliebiger Objektwert, der jedoch häufig auf bestimmte Objekttypen, z. B. die Wahl zwischen einer Zeichenfolge und einem Objekt, beschränkt ist. Weitere Informationen finden Sie in den Anmerkungen auf der Referenzseite.
-   *Objekt* *Eigenschaft*: *Objekt* *Eigenschaft* in Kombination dient für Situationen, in denen die Syntax wird die Syntax für einen Typ, der als verwendet werden kann ein der Attributwert für viele Eigenschaften. Z. B. die **Xaml-Attributverwendung** für angezeigte [ **Pinsel** ](/uwp/api/Windows.UI.Xaml.Media.Brush) enthält: <*Objekt* *Eigenschaft*= "*PredefinedColorName*" / >
-   *EventHandler*: Dies wird als der Attributwert für alle XAML-Syntax für ein Ereignisattribut angezeigt. In diesem Fall geben Sie den Funktionsnamen für eine Ereignishandlerfunktion an. Diese Funktion muss im CodeBehind für die XAML-Seite definiert werden. Auf Programmierungsebene muss die Funktion der Delegatsignatur des behandelten Ereignisses entsprechen. Andernfalls kann der App-Code nicht kompiliert werden. Dies muss jedoch nur bei der Programmierung und nicht im XAML berücksichtigt werden, daher enthält die XAML-Syntax keine Hinweise zum Delegattyp. Wenn Sie wissen möchten, welcher Delegat für ein Ereignis implementiert werden sollte, finden Sie Informationen hierzu im Abschnitt **Ereignisinformationen** im Referenzthema für das Ereignis (in einer Tabellenspalte mit der Bezeichnung **Delegat**).
-   *enumMemberName*: wird in der Attributsyntax für alle Enumerationen angezeigt. Es gibt einen ähnlichen Platzhalter für Eigenschaften, die einen Enumerationswert verwenden. In der Regel wird dem Platzhalter jedoch ein Präfix mit einem Hinweis auf den Namen der Enumeration vorangestellt. So lautet beispielsweise die Syntax für [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) is <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>. Klicken Sie auf einer der Eigenschaftenreferenzseiten auf den Link zum Enumerationstyp, der im Abschnitt **Eigenschaftswert** neben **Typ:** angezeigt wird. Für den Attributwert einer Eigenschaft, die diese Enumeration verwendet, können Sie eine der Zeichenfolgen verwenden, die in der Liste **Member** in der Spalte **Member** aufgeführt sind.
-   *doppelte*, *Int*, *Zeichenfolge*, *"bool"*: Dies sind die primitiven Typen bekannt, dass die XAML-Sprache. Bei der Programmierung mit C# oder Visual Basic werden diese in entsprechende Typen für Microsoft .NET umgewandelt, z. B. [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx), [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) und [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx). Sie können alle Member für diese .NET-Typen verwenden, wenn Sie mit Ihren XAML-definierten Werten in .NET-CodeBehind arbeiten. Bei der Programmierung mit C++/CX verwenden Sie die primitiven C++-Typen. Diese können jedoch auch als äquivalent zu durch den [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx)-Namespace definierten Typen betrachtet werden, z. B. [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx). Für bestimmte Eigenschaften liegen manchmal zusätzliche Werteinschränkungen vor. Diese werden aber in der Regel in dem Abschnitt **Eigenschaftswert** oder "Anmerkungen" und nicht in einem XAML-Abschnitt erwähnt, da solche Einschränkungen sowohl für Code- als auch für XAML-Verwendungen gelten.

## <a name="tips-and-tricks-notes-on-style"></a>Tipps, Tricks und Hinweise zum Stil

-   Eine allgemeine Beschreibung von Markuperweiterungen finden Sie in der [Übersicht über XAML](xaml-overview.md). Die für die Anleitungen in diesem Thema wichtigste Markuperweiterung ist aber die [StaticResource](staticresource-markup-extension.md)-Markuperweiterung (und die zugehörige [ThemeResource](themeresource-markup-extension.md)-Markuperweiterung). Die StaticResource-Markuperweiterung soll es Ihnen ermöglichen, Ihren XAML-Code in wiederverwendbare Ressourcen aus einem XAML-[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) zu zerlegen. In den allermeisten Fällen definieren Sie Steuerelementvorlagen und zugehörige Stile in einem **ResourceDictionary**. Die kleineren Teile einer Steuerelement-Vorlagendefinition oder eines app-spezifischen Stils definieren Sie oft auch in einem **ResourceDictionary**, z. B. einen [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) für eine Farbe, die in Ihrer App mehrmals für verschiedene Teile der UI verwendet wird. Durch die Verwendung einer StaticResource kann jede Eigenschaft, für die andernfalls eine Eigenschaftselementsyntax erforderlich wäre, jetzt in einer Attributsyntax festgelegt werden. Die vereinfachte Syntax auf Seitenebene ist aber nur einer der Vorteile, die die Zerlegung des XAML-Codes zur Wiederverwendung bietet. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenreferenzen](https://msdn.microsoft.com/library/windows/apps/mt187273).
-   Sie werden verschiedene Konventionen für die Verwendung von Leerzeichen und Zeilenvorschüben in XAML-Beispielen kennen lernen. Insbesondere gelten unterschiedliche Konventionen für die Unterteilung von Objektelementen, für die viele verschiedene Attribute festgelegt sind. Dies ist nur eine Frage des Stils. Der XML-Editor in Visual Studio wendet einige standardmäßige Stilregeln an, wenn Sie XAML-Code bearbeiten, Sie können diese Regeln aber in den Einstellungen ändern. Es gibt einige wenige Fälle, in denen die Leerzeichen in einer XAML-Datei von Bedeutung sind. Weitere Informationen finden Sie unter [XAML und Leerzeichen](xaml-and-whitespace.md).

## <a name="related-topics"></a>Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [XAML-Namespaces und Namespacezuordnung](xaml-namespaces-and-namespace-mapping.md)
* [ResourceDictionary und XAML Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

