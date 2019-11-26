---
description: Stellt durch Auswerten eines Verweises auf eine bereits definierte Quelle einen Wert für ein beliebiges XAML-Attribut bereit. Ressourcen sind in einem ResourceDictionary definiert, und mit der Verwendung einer StaticResource wird auf den Schlüssel dieser Ressource im ResourceDictionary verwiesen.
title: StaticResource-Markuperweiterung
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07d8e6c180f332e75852c6a6627004f0306e26d4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259843"
---
# <a name="staticresource-markup-extension"></a>{StaticResource}-Markuperweiterung


Stellt durch Auswerten eines Verweises auf eine bereits definierte Quelle einen Wert für ein beliebiges XAML-Attribut bereit. Ressourcen sind in einem [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) definiert, und mit der Verwendung einer **StaticResource** wird auf den Schlüssel dieser Ressource im **ResourceDictionary** verwiesen.

## <a name="xaml-attribute-usage"></a>XAML-Attributverwendung

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| Schlüssel | Der Schlüssel für die angeforderte Ressource. Dieser Schlüssel wird anfänglich durch das [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) zugewiesen. Ein Ressourcenschlüssel kann eine beliebige in der XamlName-Grammatik definierte Zeichenfolge sein. |

## <a name="remarks"></a>Hinweise

**StaticResource** ist eine Methode zum Abrufen von Werten für ein XAML-Attribut, die an anderer Stelle in einem XAML-Ressourcenwörterbuch definiert sind. Werte können in einem Ressourcenwörterbuch definiert werden, da sie von mehreren Eigenschaftswerten gemeinsam verwendet werden sollen oder ein XAML-Ressourcenwörterbuch als Teil einer XAML-Verpackungs- oder Gestaltungsmethode verwendet wird. Ein Beispiel für eine XAML-Verpackungsmethode ist das Designwörterbuch für ein Steuerelement. Ein weiteres Beispiel ist die Verwendung von zusammengeführten Ressourcenwörterbüchern für das Ressourcenfallback.

**StaticResource** akzeptiert ein Argument, das den Schlüssel für die angeforderte Ressource angibt. Ein Ressourcenschlüssel ist immer eine Zeichenfolge in der Windows-Runtime-XAML. Weitere Informationen zum anfänglichen Festlegen des Ressourcenschlüssels finden Sie unter [x:Key-Attribut](x-key-attribute.md).

Die Regeln, nach denen die Auflösung einer **StaticResource** zu einem Element in einem Ressourcenwörterbuch erfolgt, wird in diesem Thema nicht beschrieben. Dies hängt davon ab, ob sowohl der Verweis als auch die Ressource in einer Vorlage vorhanden sind, ob zusammengeführte Ressourcenwörterbücher verwendet werden usw. Weitere Informationen dazu, wie Sie Ressourcen und Eigenschaften mithilfe eines [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) definieren, und zusätzlichen Beispielcode finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

**Wichtig**   eine **statikresource** darf nicht versuchen, einen vorwärts Verweis auf eine Ressource zu erstellen, die lexikalisch weiter oben in der XAML-Datei definiert ist. Dieser Versuch wird nicht unterstützt. Auch wenn der weitergeleitete Verweis keinen Fehler verursacht, wird durch den Versuch die Leistung beeinträchtigt. Um optimale Ergebnisse zu erzielen, sollten Sie die Ressourcenwörterbücher so erstellen, dass Vorwärtsverweise vermieden werden können.

Wenn Sie versuchen, eine **StaticResource** für einen Schlüssel anzugeben, die nicht aufgelöst werden kann, führt dies zu einer XAML-Analyseausnahme zur Laufzeit. Entwicklungstools geben unter Umständen auch Warnungen oder Fehler aus.

Die XAML-Prozessorimplementierung der Windows-Runtime enthält keine Sicherungsklassendarstellung für **StaticResource**-Funktionen. **StaticResource** ist ausschließlich für die Verwendung in XAML vorgesehen. Die weitestgehende Entsprechung im Code ist die Verwendung der Auflistungs-API eines [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), z. B. der Aufruf von [**Contains**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.contains) oder [**TryGetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue).

Die [{ThemeResource}-Markuperweiterung](themeresource-markup-extension.md) ist eine ähnliche Markuperweiterung, die auf benannte Ressourcen an einer andere Position verweist. Der Unterschied besteht darin, dass die {ThemeResource}-Markuperweiterung je nach aktivem Systemdesign verschiedene Ressourcen zurückgeben kann. Weitere Informationen finden Sie unter [{ThemeResource}-Markuperweiterung](themeresource-markup-extension.md).

**StaticResource** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn für Attributwerte Escapezeichen verwendet werden müssen, damit sie keine Literalwerte oder Handlernamen darstellen, und es nicht ausreicht, Typkonverter für bestimmte Typen oder Eigenschaften zu verwenden. Alle Markup Erweiterungen in XAML verwenden die Zeichen "\{" und "\}" in der Attribut Syntax. dabei handelt es sich um die Konvention, mit der ein XAML-Prozessor erkennt, dass das Attribut von einer Markup Erweiterung verarbeitet werden muss.

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

In diesem bestimmten Beispiel wird ein Objekt erstellt, hinter dem eine benutzerdefinierte Klasse steht. Dieses Objekt wird als Ressource in einer [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) erstellt. Dieses `local:S2Formatter`-Element muss auch einen **x:Key**-Attributwert haben, um als gültige Ressource zu gelten. Der Wert des Attributs wird auf "GradeConverter" festgelegt.

Die Ressource wird dann etwas weiter hinten im XAML angefordert, wo `{StaticResource GradeConverter}` angezeigt wird.

Die Syntax der {StaticResource}-Markuperweiterung legt eine Eigenschaft einer anderen Markuperweiterung ([{Binding}-Markuperweiterung](binding-markup-extension.md)) fest, sodass zwei geschachtelte Markuperweiterungssyntaxen vorhanden sind. Die innere wird zuerst ausgewertet, damit die Ressource zuerst abgerufen wird und als Wert verwendet werden kann. Dieses Beispiel wird auch in der {Binding}-Markuperweiterung gezeigt.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>Unterstützung von Entwurfszeittools für die **{StaticResource}** -Markuperweiterung

Microsoft Visual Studio 2013 kann mögliche Schlüsselwerte in die Microsoft IntelliSense-Dropdown Liste einschließen, wenn Sie die **{statikresource}** -Markup Erweiterung auf einer XAML-Seite verwenden. Sobald Sie z. B. "{StaticResource" eingeben, wird ein beliebiger Ressourcenschlüssel aus den Designressourcen angezeigt. Neben den typischen Ressourcen auf Seitenebene ([**FrameworkElement.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources)) und App-Ebene ([**Application.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources)) sehen Sie auch [XAML-Designressourcen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources) und Ressourcen aus den von Ihrem Projekt verwendeten Erweiterungen.

Sobald ein Ressourcenschlüssel als Teil einer **{StaticResource}** -Verwendung vorhanden ist, kann das Feature **Gehe zu Definition** (F12) diese Ressource auflösen und Ihnen das Wörterbuch anzeigen, in dem sie definiert ist. Bei Designressourcen wird dies an generic.xaml für die Entwurfszeit weitergeleitet.

## <a name="related-topics"></a>Verwandte Themen

* [ResourceDictionary- und XAML-Ressourcenreferenzen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
* [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [x:Key-Attribut](x-key-attribute.md)
* [{Themeresource}-Markup Erweiterung](themeresource-markup-extension.md)

