---
description: Sie können die PropertyPath-Klasse und die Zeichenfolgensyntax verwenden, um einen PropertyPath-Wert entweder in XAML oder in Code zu instanziieren.
title: PropertyPath-Syntax
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5a3cac608bbc985bb8c0db0e0cece219b3c566a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155034"
---
# <a name="property-path-syntax"></a>PropertyPath-Syntax


Sie können die [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)-Klasse und die Zeichenfolgensyntax verwenden, um einen **PropertyPath**-Wert entweder in XAML oder in Code zu instanziieren. **PropertyPath**-Werte werden von Datenbindungen genutzt. Eine ähnliche Syntax kommt für die Ausrichtung von Storyboardanimationen zum Einsatz. In beiden Szenarien beschreibt ein Eigenschaftspfad eine Traversierung von einer oder mehreren Objekt-Eigenschaft-Beziehungen, die schließlich in einer einzelnen Eigenschaft aufgelöst werden.

Sie können eine Eigenschaftspfad-Zeichenfolge in XAML direkt einem Attribut zuweisen. Sie haben die Möglichkeit, mit derselben Zeichenfolgensyntax entweder eine [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)-Klasse zu konstruieren, die ein [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding)-Objekt im Code festlegt, oder mithilfe der [**SetTargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.settargetproperty)-Methode ein Animationsziel im Code zu bestimmen. Es bestehen zwei unterschiedliche Featurebereiche in der Windows-Runtime, die auf einen Eigenschaftspfad zurückgreifen: Datenbindung und Animationsausrichtung. Die Animationsausrichtung generiert keine zugrunde liegenden Property-path-Syntaxwerte in der Windows-Runtime-Implementierung. Sie behält die Informationen als Zeichenfolge bei. Die Konzepte der Objekt-Eigenschaft-Traversierung sind jedoch sehr ähnlich. Datenbindung und Animationsausrichtung werten einen Eigenschaftspfad auf leicht unterschiedliche Weise aus, daher behandeln wir die Eigenschaftspfadsyntax dafür getrennt.

## <a name="property-path-for-objects-in-data-binding"></a>Eigenschaftspfad für Objekte bei der Datenbindung

In der Windows-Runtime können Sie eine Bindung zum Zielwert einer beliebigen Abhängigkeitseigenschaft vornehmen. Der Quelleigenschaftswert für eine Datenbindung muss keine Abhängigkeitseigenschaft sein. Sie kann eine Eigenschaft auf einem Geschäftsobjekt sein (zum Beispiel eine in einer Microsoft .NET-Sprache oder in C++ geschriebene Klasse ). Das Quellobjekt für den Bindungswert kann auch ein bestehendes Abhängigkeitsobjekt sein, das von der App bereits definiert ist. Die Quelle kann entweder von einem einfachen Eigenschaftennamen oder einer Traversierung der Objekt-Eigenschaft-Beziehungen im Objektgraphen des Geschäftsobjekts referenziert werden.

Sie können eine Bindung zu einem einzelnen Eigenschaftswert oder zu einer solchen Zieleigenschaft vornehmen, die Listen oder Sammlungen enthält. Wenn es sich bei der Quelle um eine Sammlung handelt oder der Pfad eine Sammlungseigenschaft angibt, gleicht das Datenbindungsmodul die Sammelelemente der Quelle mit dem Bindungsziel ab. Dies kann zum Beispiel bewirken, dass ein [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox)-Objekt mit einer Liste von Elementen von einer Datenquellensammlung gefüllt wird, ohne dass die spezifischen Elemente in der Sammlung erwartet werden müssen.

### <a name="traversing-an-object-graph"></a>Traversieren eines Objektgraphen

Das Element der Syntax, das die Traversierung einer Objekt-Eigenschaft-Beziehung in einem Objektgraphen kennzeichnet, ist das Punktzeichen (**.**). Jeder Punkt in einer Eigenschaftspfad-Zeichenfolge gibt eine Trennung zwischen einem Objekt (auf der linken Seite des Punkts) und einer Eigenschaft dieses Objekts (auf der rechten Seite des Punkts) an. Die Zeichenfolge wird von links nach rechts ausgewertet, was ermöglicht, mehrere Objekt-Eigenschaft-Beziehungen zu durchlaufen. Betrachten wir dazu ein Beispiel:

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

Dieser Pfad wird wie folgt ausgewertet:

1.  Das Datenkontextobjekt (oder eine mit derselben [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding)-Klasse angegebene [**Source**](/uwp/api/windows.ui.xaml.data.binding.source)-Eigenschaft) wird nach einer Eigenschaft namens „Customer“ durchsucht.
2.  Das Objekt, das den Wert der „Customer“-Eigenschaft darstellt, wird nach einer Eigenschaft namens „Address“ durchsucht.
3.  Das Objekt, das den Wert der „Customer“-Eigenschaft darstellt, wird nach einer Eigenschaft namens „StreetAddress1“ durchsucht.

Bei jedem dieser Schritte wird der Wert als Objekt behandelt. Der Typ des Ergebnisses wird nur dann überprüft, wenn die Bindung auf eine bestimmte Eigenschaft angewendet wird. Dieses Beispiel würde nicht funktionieren, wenn es sich bei „Address“ nur um einen Zeichenfolgenwert handeln würde, der nicht klarstellt, bei welchem Teil der Zeichenfolge es sich um die Straßenadresse handelt. In der Regel verweist die Bindung auf bestimmte geschachtelte Eigenschaftswerte eines Geschäftsobjekts, das eine bekannte und zweckmäßige Informationsstruktur aufweist.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>Regeln für die Eigenschaften in einem Datenbindungs-Eigenschaftspfad

-   Alle Eigenschaften, die von einem Eigenschaftspfad referenziert werden, müssen im Quellgeschäftsobjekt öffentlich sein.
-   Die Endeigenschaft (die Eigenschaft, bei der es sich um die letzte ausgewiesene Eigenschaft im Pfad handelt) muss öffentlich und änderbar sein. Es ist nicht möglich, Bindungen zu statischen Werten herzustellen.
-   Die Endeigenschaft muss „read/write“ sein, wenn dieser Pfad als [**Path**](/uwp/api/windows.ui.xaml.data.binding.path)-Information in einer bidirektionalen Bindung verwendet werden soll.

### <a name="indexers"></a>Indexer

Ein Eigenschaftspfad für Datenbindungen kann Verweise auf indizierte Eigenschaften aufweisen. Das ermöglicht die Bindung an geordnete Listen/Vektoren oder Wörterbücher/Karten. Verwenden Sie eckige Klammern " \[ \] ", um eine indizierte Eigenschaft anzugeben. Diese Klammern können entweder eine ganze Zahl (für eine geordnete Liste) oder eine Zeichenfolge ohne Anführungszeichen (für Wörterbücher) enthalten. Darüber hinaus ist die Bindung an ein Wörterbuch möglich, bei der der Schlüssel eine ganze Zahl ist. Sie können verschiedene indizierte Eigenschaften im selben Pfad verwenden und die Objekteigenschaft mit einem Punkt abtrennen.

Nehmen wir zum Beispiel ein Geschäftsobjekt, bei dem es eine Liste von „Teams“ gibt (geordnete Liste), von denen jedes ein Wörterbuch von „Players“ aufweist, wobei als Schlüssel für jeden Spieler der Nachname verwendet wird. Ein Beispiel für einen Eigenschafts Pfad zu einem bestimmten Player im zweiten Team ist: "Teams \[ 1 \] . Players \[ Smith \] ". (Sie verwenden 1, um das zweite Element in „Teams“ anzugeben, da die Liste nullindiziert ist.)

**Hinweis**    Die Indizierungs Unterstützung für C++-Datenquellen ist begrenzt. Ausführliche Informationen finden Sie [unter Datenbindung](../data-binding/data-binding-in-depth.md).

### <a name="attached-properties"></a>Angefügte Eigenschaften

Eigenschaftspfade können Verweise auf angefügte Eigenschaften enthalten. Da der Bezeichnername einer angefügten Eigenschaft bereits einen Punkt enthält, muss ein Name für die angefügte Eigenschaft in runden Klammern hinzugefügt werden, damit der Punkt nicht als Objekteigenschaftsschritt interpretiert wird. Wenn Sie beispielsweise [**Canvas.ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) als Bindungspfad verwenden möchten, lautet die korrekte Zeichenfolge „(Canvas.ZIndex)“. Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Übersicht über angefügte Eigenschaften](attached-properties-overview.md).

### <a name="combining-property-path-syntax"></a>Zusammenführen der Eigenschaftspfadsyntax

Sie können verschiedene Elemente der Eigenschaftspfadsyntax in einer einzelnen Zeichenfolge zusammenführen. Beispielsweise können Sie einen Eigenschaftspfad definieren, der eine indizierte angefügte Eigenschaft referenziert, sofern Ihre Datenquelle eine solche Eigenschaft aufweist.

### <a name="debugging-a-binding-property-path"></a>Debuggen eines Bindungseigenschaftspfads

Da ein Eigenschaftspfad von einem Bindungsmodul interpretiert wird und auf Informationen basiert, die möglicherweise nur während der Laufzeit verfügbar sind, müssen Sie einen Eigenschaftspfad für Bindungen oft debuggen, ohne dass Ihnen die gewöhnliche Entwurfszeit- oder Kompilierungszeitunterstützung in den Entwicklungstools zur Verfügung steht. In vielen Fällen ist das Laufzeitergebnis bei einer fehlgeschlagenen Auflösung eines Eigenschaftspfads ein leerer Wert ohne Fehler, da es sich dabei um das Standardausweichverhalten bei der Auflösung von Bindungen handelt. Glücklicherweise bietet Microsoft Visual Studio einen Debugausgabemodus, der den Teil eines Eigenschaftspfads isoliert, der eine Bindungsquelle angibt, die nicht aufgelöst werden konnte. Weitere Informationen zur Verwendung dieses Entwicklungstoolfeatures finden Sie im Abschnitt [„Debuggen“ im Thema „Datenbindung im Detail“](../data-binding/data-binding-in-depth.md#debugging).

## <a name="property-path-for-animation-targeting"></a>Eigenschaftspfad für die Animationsausrichtung

Animationen stützen sich auf die Ausrichtung auf eine Abhängigkeitseigenschaft, wobei Storyboardwerte angewendet werden, wenn die Animation läuft. Die Animation ermittelt ein Element anhand des Namens ([x:Name-Attribut](x-name-attribute.md)), um das Objekt mit der zu animierenden Eigenschaft zu identifizieren. Oft ist es erforderlich, einen Eigenschaftspfad zu definieren, der mit dem Objekt beginnt, das als der [**Storyboard.TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname) bezeichnet wird, und mit dem speziellen Abhängigkeitseigenschaftswert endet, bei dem die Animation angewendet werden soll. Dieser Eigenschaftspfad wird als Wert für [**Storyboard.TargetProperty**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)) verwendet.

Weitere Informationen zum Definieren von Animationen in XAML finden Sie unter [Storyboardanimationen](../design/motion/storyboarded-animations.md).

## <a name="simple-targeting"></a>Einfache Ausrichtung

Wenn Sie eine Eigenschaft animieren, die auf dem Zielobjekt selbst existiert, und es möglich ist, eine Animation direkt auf den Typ dieser Eigenschaft anzuwenden (statt auf eine Untereigenschaft des Werts einer Eigenschaft), können Sie die zu animierende Eigenschaft einfach benennen. Es müssen keine weiteren Bedingungen erfüllt sein. Wenn Sie zum Beispiel eine Ausrichtung auf eine [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)-Unterklasse wie [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) vornehmen und eine animierte [**Color**](/uwp/api/Windows.UI.Color)-Struktur auf die [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)-Eigenschaft anwenden, kann der Eigenschaftspfad „Fill“ sein.

## <a name="indirect-property-targeting"></a>Indirekte Eigenschaftsausrichtung

Sie können eine Eigenschaft animieren, bei der es sich um eine Untereigenschaft des Zielobjekts handelt. Anders ausgedrückt: Wenn das Zielobjekt über eine Eigenschaft verfügt, bei der es sich ebenfalls um ein Objekt handelt, und dieses Objekt Eigenschaften aufweist, müssen Sie einen Eigenschaftspfad definieren, der klarstellt, wie diese Objekt-Eigenschaft-Beziehung durchlaufen werden soll. Wenn Sie ein Objekt angeben, bei dem Sie eine Untereigenschaft animieren möchten, fügen Sie den Eigenschaftennamen in runden Klammern an. Die Eigenschaft geben Sie im Format *Typname*.*Eigenschaftsname* ein. Um beispielsweise anzugeben, dass Sie den Objektwert einer [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform)-Eigenschaft eines Zielobjekts abrufen möchten, legen Sie „(UIElement.RenderTransform)“ als ersten Schritt im Eigenschaftspfad fest. Dabei handelt es sich noch nicht um einen vollständigen Pfad, da es keine Animationen gibt, die sich direkt auf einen [**Transform**](/uwp/api/Windows.UI.Xaml.Media.Transform)-Wert anwenden lassen. In diesem Beispiel müssen Sie den Eigenschaftspfad nun vervollständigen. Bei der Endeigenschaft handelt es sich somit um eine Eigenschaft einer **Transform**-Unterklasse, die von einem **Double**-Wert animiert werden kann: „(UIElement.RenderTransform).(CompositeTransform.TranslateX)“

## <a name="specifying-a-particular-child-in-a-collection"></a>Angeben eines bestimmten untergeordneten Elements in einer Sammlung

Sie können einen numerischen Indexer dazu verwenden, ein untergeordnetes Element in einer Sammlungseigenschaft anzugeben. Verwenden Sie eckige Klammern (" \[ \] ") um den ganzzahligen Indexwert. Sie können nur geordnete Listen aber keine Wörterbücher referenzieren. Da es sich bei einer Sammlung nicht um einen Wert handelt, der animiert werden kann, kann eine Indexerverwendung nie die Endeigenschaft in einem Eigenschaftspfad sein.

Um z. b. anzugeben, dass Sie die erste Farb Ende Farbe in einem [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) animieren möchten, der auf die [**Hintergrund**](/uwp/api/windows.ui.xaml.controls.control.background) Eigenschaft eines Steuer Elements angewendet wird, ist dies der Eigenschafts Pfad: "(Control. Background). (GradientBrush. GradientStops) \[ 0 \] . ( Gradienthalte. Color) ". Achten Sie darauf, dass der Indexer nicht der letzte Schritt im Pfad ist und dass vor allem der letzte Schritt die [**GradientStop.Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color)-Eigenschaft des Elements 0 in der Sammlung referenzieren muss, um einen animierten [**Color**](/uwp/api/Windows.UI.Color)-Wert darauf anzuwenden.

## <a name="animating-an-attached-property"></a>Animieren einer angefügten Eigenschaft

Wenn auch dies selten vorkommt, kann eine angefügte Eigenschaft animiert werden, sofern die angefügte Eigenschaft einen Eigenschaftswert aufweist, der mit einem Animationstyp übereinstimmt. Da der Bezeichnername einer angefügten Eigenschaft bereits einen Punkt enthält, muss ein Name für die angefügte Eigenschaft in runden Klammern hinzugefügt werden, damit der Punkt nicht als Objekteigenschaftsschritt interpretiert wird. Verwenden Sie beispielsweise für die Zeichenfolge, die zum Animieren der angefügten [**Grid.Row**](/dotnet/api/system.windows.controls.grid.row)-Eigenschaft auf einem Objekt angegeben werden muss, den Eigenschaftspfad „(Grid.Row)“.

**Hinweis**    In diesem Beispiel ist der Wert von [**Grid. Row**](/dotnet/api/system.windows.controls.grid.row) ein **Int32** -Eigenschaftentyp. Daher ist die Animation mit einer **Double**-Animation nicht möglich. Definieren Sie stattdessen eine [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Klasse, die über [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)-Komponenten verfügt, wobei [**ObjectKeyFrame.Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) auf eine ganze Zahl wie „0“ oder „1“ festgelegt wird.

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>Regeln für die Eigenschaften in einem Animationsausrichtungs-Eigenschaftspfad

-   Der angenommene Anfangspunkt des Eigenschaftspfads ist das Objekt, das von einem [**Storyboard.TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname) bezeichnet wird.
-   Alle über den Eigenschaftspfad hinweg referenzierten Objekte und Eigenschaften müssen öffentlich sein.
-   Die Endeigenschaft (die Eigenschaft, bei der es sich um die letzte ausgewiesene Eigenschaft im Pfad handelt) muss öffentlich, les- und schreibbar und eine Abhängigkeitseigenschaft sein.
-   Die Endeigenschaft muss zudem über einen Eigenschaftstyp verfügen, der von einer der breitgefächerten Klassen von Animationstypen animiert werden kann ([**Color**](/uwp/api/Windows.UI.Color)-Animationen, **Double**-Animationen, [**Point**](/uwp/api/Windows.Foundation.Point)-Animationen, [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)).

## <a name="the-propertypath-class"></a>Die PropertyPath-Klasse

Die [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)-Klasse ist der zugrundeliegende Eigenschaftstyp der [**Binding.Path**](/uwp/api/windows.ui.xaml.data.binding.path)-Eigenschaft für das Bindungsszenario.

Meistens ist es in XAML möglich, eine [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)-Klasse ohne jeglichen Code anzuwenden. In einigen Fällen ist es jedoch sinnvoll, ein **PropertyPath**-Objekt mithilfe von Code zu definieren und dieses zur Laufzeit einer Eigenschaft zuzuordnen.

[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) verfügt über einen [**PropertyPath(String)**](/uwp/api/windows.ui.xaml.propertypath.-ctor)-Konstruktor und hat keinen Standardkonstruktor. Die Zeichenfolge, die Sie diesem Konstruktor übergeben, wird mithilfe der zuvor beschriebenen Eigenschaftspfadsyntax definiert. Dies ist dieselbe Zeichenfolge, mit der Sie [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) als XAML-Attribut zuweisen können. Die einzige andere API der **PropertyPath**-Klasse ist die schreibgeschützte [**Path**](/uwp/api/windows.ui.xaml.propertypath.path)-Eigenschaft. Sie können diese Eigenschaft als Konstruktionszeichenfolge für eine andere **PropertyPath**-Instanz verwenden.

## <a name="related-topics"></a>Zugehörige Themen

* [Datenbindung im Detail](../data-binding/data-binding-in-depth.md)
* [Storyboardanimationen](../design/motion/storyboarded-animations.md)
* [{Binding}-Markuperweiterung](binding-markup-extension.md)
* [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)
* [**Bindung**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**Binding-Konstruktor**](/uwp/api/windows.ui.xaml.data.binding.-ctor)
* [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)