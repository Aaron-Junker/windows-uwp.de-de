---
description: Stellt durch Auswerten eines Verweises auf eine bereits definierte Quelle einen Wert für ein beliebiges XAML-Attribut bereit. Ressourcen sind in einem ResourceDictionary definiert, und mit der Verwendung einer StaticResource wird auf den Schlüssel dieser Ressource im ResourceDictionary verwiesen.
title: StaticResource-Markuperweiterung
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 32d09d4366d5ee05fe9581c4c8076811cfa0436f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155004"
---
# <a name="staticresource-markup-extension"></a>{StaticResource}-Markuperweiterung


Stellt durch Auswerten eines Verweises auf eine bereits definierte Quelle einen Wert für ein beliebiges XAML-Attribut bereit. Ressourcen sind in einem [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) definiert, und mit der Verwendung einer **StaticResource** wird auf den Schlüssel dieser Ressource im **ResourceDictionary** verwiesen.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | BESCHREIBUNG |
|------|-------------|
| Schlüssel | Der Schlüssel für die angeforderte Ressource. Dieser Schlüssel wird anfänglich von [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)zugewiesen. Ein Ressourcenschlüssel kann eine beliebige in der XamlName-Grammatik definierte Zeichenfolge sein. |

## <a name="remarks"></a>Bemerkungen

**StaticResource** ist eine Methode zum Abrufen von Werten für ein XAML-Attribut, die an anderer Stelle in einem XAML-Ressourcenwörterbuch definiert sind. Werte können in einem Ressourcenwörterbuch definiert werden, da sie von mehreren Eigenschaftswerten gemeinsam verwendet werden sollen oder ein XAML-Ressourcenwörterbuch als Teil einer XAML-Verpackungs- oder Gestaltungsmethode verwendet wird. Ein Beispiel für eine XAML-Verpackungsmethode ist das Designwörterbuch für ein Steuerelement. Ein weiteres Beispiel ist die Verwendung von zusammengeführten Ressourcenwörterbüchern für das Ressourcenfallback.

**StaticResource** akzeptiert ein Argument, das den Schlüssel für die angeforderte Ressource angibt. Ein Ressourcenschlüssel ist immer eine Zeichenfolge in der Windows-Runtime-XAML. Weitere Informationen zum anfänglichen Festlegen des Ressourcenschlüssels finden Sie unter [x:Key-Attribut](x-key-attribute.md).

Die Regeln, nach denen die Auflösung einer **StaticResource** zu einem Element in einem Ressourcenwörterbuch erfolgt, wird in diesem Thema nicht beschrieben. Dies hängt davon ab, ob sowohl der Verweis als auch die Ressource in einer Vorlage vorhanden sind, ob zusammengeführte Ressourcenwörterbücher verwendet werden usw. Weitere Informationen dazu, wie Sie Ressourcen und Eigenschaften mithilfe eines [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) definieren, und zusätzlichen Beispielcode finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).

**Wichtig**    Eine **statikresource** darf nicht versuchen, einen vorwärts Verweis auf eine Ressource zu erstellen, die lexikalisch weiter in der XAML-Datei definiert ist. Dieser Versuch wird nicht unterstützt. Auch wenn der weitergeleitete Verweis keinen Fehler verursacht, wird durch den Versuch die Leistung beeinträchtigt. Um optimale Ergebnisse zu erzielen, sollten Sie die Ressourcenwörterbücher so erstellen, dass Vorwärtsverweise vermieden werden können.

Wenn Sie versuchen, eine **StaticResource** für einen Schlüssel anzugeben, die nicht aufgelöst werden kann, führt dies zu einer XAML-Analyseausnahme zur Laufzeit. Entwicklungstools geben unter Umständen auch Warnungen oder Fehler aus.

Die XAML-Prozessorimplementierung der Windows-Runtime enthält keine Sicherungsklassendarstellung für **StaticResource**-Funktionen. **StaticResource** ist ausschließlich für die Verwendung in XAML vorgesehen. Die weitestgehende Entsprechung im Code ist die Verwendung der Auflistungs-API eines [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), z. B. der Aufruf von [**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) oder [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue).

Die [{ThemeResource}-Markuperweiterung](themeresource-markup-extension.md) ist eine ähnliche Markuperweiterung, die auf benannte Ressourcen an einer andere Position verweisen. Der Unterschied besteht darin, dass die {ThemeResource}-Markuperweiterung je nach aktivem Systemdesign verschiedene Ressourcen zurückgeben kann. Weitere Informationen finden Sie unter [{ThemeResource}-Markuperweiterung](themeresource-markup-extension.md).

**StaticResource** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn Attributwerte mit Escapezeichen versehen werden müssen, damit diese nicht als literale Werte oder als Handlernamen betrachtet werden, und diese Anforderung eher global und nicht nur durch den Einsatz von Typkonvertern für bestimmte Typen oder Eigenschaften erfüllt werden soll. Alle Markup Erweiterungen in XAML verwenden die \{ Zeichen "" und " \} " in der Attribut Syntax. Dies ist die Konvention, mit der ein XAML-Prozessor erkennt, dass das Attribut von einer Markup Erweiterung verarbeitet werden muss.

### <a name="an-example-staticresource-usage"></a>{StaticResource}-Beispielverwendung

Der folgende XAML-Beispielcode stammt aus dem [XAML-Datenbindungsbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

In diesem bestimmten Beispiel wird ein Objekt erstellt, hinter dem eine benutzerdefinierte Klasse steht. Dieses Objekt wird als Ressource in einer [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) erstellt. Dieses `local:S2Formatter`-Element muss auch einen **x:Key**-Attributwert haben, um als gültige Ressource zu gelten. Der Wert des Attributs wird auf "GradeConverter" festgelegt.

Die Ressource wird dann etwas weiter hinten im XAML angefordert, wo `{StaticResource GradeConverter}` angezeigt wird.

Die Syntax der {StaticResource}-Markuperweiterung legt eine Eigenschaft einer anderen Markuperweiterung ([{Binding}-Markuperweiterung](binding-markup-extension.md)) fest, sodass zwei geschachtelte Markuperweiterungssyntaxen vorhanden sind. Die innere wird zuerst ausgewertet, damit die Ressource zuerst abgerufen wird und als Wert verwendet werden kann. Dieses Beispiel wird auch in der {Binding}-Markuperweiterung gezeigt.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>Unterstützung von Entwurfszeittools für die **{StaticResource}**-Markuperweiterung

Microsoft Visual Studio 2013 kann mögliche Schlüsselwerte in die Microsoft IntelliSense-Dropdownelemente einbinden, wenn Sie die **{StaticResource}**-Markuperweiterung auf einer XAML-Seite verwenden. Sobald Sie z. B. "{StaticResource" eingeben, wird ein beliebiger Ressourcenschlüssel aus den Designressourcen angezeigt. Neben den typischen Ressourcen auf Seitenebene ([**FrameworkElement.Resources**](/uwp/api/windows.ui.xaml.frameworkelement.resources)) und App-Ebene ([**Application.Resources**](/uwp/api/windows.ui.xaml.application.resources)) sehen Sie auch [XAML-Designressourcen](../design/controls-and-patterns/xaml-theme-resources.md) und Ressourcen aus den von Ihrem Projekt verwendeten Erweiterungen.

Sobald ein Ressourcenschlüssel als Teil einer **{StaticResource}**-Verwendung vorhanden ist, kann das Feature **Gehe zu Definition** (F12) diese Ressource auflösen und Ihnen das Wörterbuch anzeigen, in dem sie definiert ist. Bei Designressourcen wird dies an generic.xaml für die Entwurfszeit weitergeleitet.

## <a name="related-topics"></a>Zugehörige Themen

* [ResourceDictionary- und XAML-Ressourcenreferenzen](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [x:Key-Attribut](x-key-attribute.md)
* [{ThemeResource}-Markuperweiterung](themeresource-markup-extension.md)