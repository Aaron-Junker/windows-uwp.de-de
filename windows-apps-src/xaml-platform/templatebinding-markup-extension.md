---
description: Verknüpft den Wert einer Eigenschaft in einer Steuerelementvorlage mit dem Wert einer anderen Eigenschaft, die im Steuerelement mit Vorlagen verfügbar gemacht wird. TemplateBinding kann nur in einer ControlTemplate-Definition in XAML verwendet werden.
title: TemplateBinding-Markuperweiterung
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154924"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding}-Markuperweiterung

Verknüpft den Wert einer Eigenschaft in einer Steuerelementvorlage mit dem Wert einer anderen Eigenschaft, die im Steuerelement mit Vorlagen verfügbar gemacht wird. **TemplateBinding** kann nur in einer [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) -Definition in XAML verwendet werden.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML-Attributsyntax (für die Setter-Eigenschaft in einer Vorlage oder Formatvorlage)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | BESCHREIBUNG |
|------|-------------|
| propertyName | Der Name der Eigenschaft, die in der Setter-Syntax festgelegt wird. Diese Eigenschaft muss eine Abhängigkeitseigenschaft sein. |
| sourceProperty | Der Name einer anderen Abhängigkeitseigenschaft, die für den als Vorlage festgelegten Typ vorhanden ist. |

## <a name="remarks"></a>Bemerkungen

Die Verwendung von **TemplateBinding** ist ein grundlegender Punkt beim Definieren einer Steuerelementvorlage, wenn Sie entweder ein Autor von benutzerdefinierten Steuerelementen sind oder eine Steuerelementvorlage für vorhandene Steuerelemente ersetzen. Weitere Informationen finden Sie unter [Schnellstart: Steuerelementvorlagen](/previous-versions/windows/apps/hh465374(v=win.10)).

Für *propertyName* und *targetProperty* wird häufig derselbe Eigenschaftsname verwendet. In diesem Fall kann ein Steuerelement eine Eigenschaft für sich selbst definieren und diese an eine vorhandene, intuitiv benannte Eigenschaft eines seiner Komponententeile weiterleiten. Beispielsweise kann ein Steuerelement, das einen [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) in seine Zusammensetzung einschließt, das zum Anzeigen der eigenen **Text** -Eigenschaft des Steuer Elements verwendet wird, dieses XAML als Teil in die Steuerelement Vorlage einschließen: `<TextBlock Text="{TemplateBinding Text}" .... />`

Die Typen, die als Wert für die Quelleigenschaft und die Zieleigenschaft verwendet werden, müssen übereinstimmen. Bei Verwendung von **TemplateBinding** gibt es keine Möglichkeit, einen Konverter zu nutzen. Falls die Werte nicht übereinstimmen, tritt beim Analysieren des XAML-Codes ein Fehler auf. Wenn Sie einen Konverter benötigen, können Sie die ausführliche Syntax für eine Vorlagenbindung verwenden, z. B.: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Wenn Sie versuchen, eine **TemplateBinding** außerhalb einer [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Definition in XAML zu verwenden, führt dies zu einem Parserfehler.

Sie können **TemplateBinding** für Fälle verwenden, in denen der übergeordnete Wert mit Vorlagen auch als weitere Bindung zurückgestellt wird. Die Auswertung für **TemplateBinding** kann warten, bis etwaige erforderliche Laufzeitbindungen über Werte verfügen.

Ein **TemplateBinding**-Element ist stets eine unidirektionale Bindung. Beide betroffenen Eigenschaften müssen Abhängigkeitseigenschaften sein.

**TemplateBinding** ist eine Markuperweiterung. Markuperweiterungen werden in der Regel implementiert, wenn Attributwerte mit Escapezeichen versehen werden müssen, damit diese nicht als literale Werte oder als Handlernamen betrachtet werden, und diese Anforderung eher global und nicht nur durch den Einsatz von Typkonvertern für bestimmte Typen oder Eigenschaften erfüllt werden soll. Alle Markuperweiterungen in XAML verwenden die Zeichen „{” und „}” in ihrer Attributsyntax. Anhand dieser Konvention erkennt ein XAML-Prozessor, dass eine Markuperweiterung das Attribut verarbeiten muss.

**Hinweis**    In der Windows-Runtime XAML-Prozessor Implementierung gibt es keine unterstützende Klassen Darstellung für **TemplateBinding**. **TemplateBinding** ist ausschließlich für die Verwendung im XAML-Markup vorgesehen. Es gibt keine einfache Methode zum Reproduzieren des Verhaltens im Code.

### <a name="xbind-in-controltemplate"></a>x:Bind in ControlTemplate

> [!NOTE]
> Die Verwendung von x:Bind in einer ControlTemplate erfordert Windows 10, Version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) oder höher. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](../debug-test-perf/version-adaptive-code.md).

Ab Windows 10, Version 1809, können Sie die **x:Bind** -Markup Erweiterung überall dort verwenden, wo Sie **TemplateBinding** in einer [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)verwenden. 

Die [TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) -Eigenschaft ist für " [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) " erforderlich (nicht optional), wenn " **x:Bind**" verwendet wird.

Mit der Unterstützung von **x:Bind** können Sie sowohl [Funktions Bindungen](../data-binding/function-bindings.md) als auch bidirektionale Bindungen in einer [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)verwenden.

In diesem Beispiel wird die **TextBlock. Text** -Eigenschaft zu **Button. Content. Inhalts**Objekt ausgewertet. Der TargetType in der ControlTemplate fungiert als Datenquelle und führt dasselbe Ergebnis wie eine TemplateBinding an das übergeordnete Element aus.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Zugehörige Themen

* [Schnellstart: Steuerelement Vorlagen](/previous-versions/windows/apps/hh465374(v=win.10))
* [Datenbindung im Detail](../data-binding/data-binding-in-depth.md)
* [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [Übersicht über XAML](xaml-overview.md)
* [Übersicht über Abhängigkeits Eigenschaften](dependency-properties-overview.md)
 