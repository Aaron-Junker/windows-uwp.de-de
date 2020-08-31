---
description: Stellt eine Methode bereit, um die Quelle einer Bindung als relative Beziehung im Laufzeitobjektdiagramm anzugeben.
title: RelativeSource-Markuperweiterung
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 383e7b0576b5462879f903b684c332b35ee7d756
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155024"
---
# <a name="relativesource-markup-extension"></a>{RelativeSource}-Markuperweiterung


Stellt eine Methode bereit, um die Quelle einer Bindung als relative Beziehung im Laufzeitobjektdiagramm anzugeben.

## <a name="xaml-attribute-usage-self-mode"></a>Verwendung von XAML-Attributen (Self-Modus)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>Verwendung von XAML-Attributen (TemplatedParent-Modus)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | BESCHREIBUNG |
|------|-------------|
| {RelativeSource Self} | Erzeugt den [<strong>Mode</strong>](/uwp/api/windows.ui.xaml.data.relativesource.mode)-Wert <strong>Self</strong>. Das Zielelement sollte als Quelle für diese Bindung verwendet werden. Dies ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. |
| {RelativeSource TemplatedParent} | Erzeugt eine [<strong>ControlTemplate</strong>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate), die als Quelle für diese Bindung angewendet wird. Dies ist nützlich, wenn Laufzeitinformationen in Bindungen auf Vorlagenebene angewendet werden sollen. | 

## <a name="remarks"></a>Bemerkungen

Mit einer [**Bindung**](/uwp/api/Windows.UI.Xaml.Data.Binding) kann [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) entweder als Attribut für ein **Binding**-Objektelement oder als Komponente in einer [{Binding}-Markuperweiterung](binding-markup-extension.md) festgelegt werden. Aus diesem Grund werden zwei verschiedene Varianten der XAML-Syntax dargestellt.

**RelativeSource** ähnelt der [{Binding}-Markuperweiterung](binding-markup-extension.md).  Hierbei handelt es sich um eine Markuperweiterung, die Instanzen selbstständig zurückgeben kann und die eine zeichenfolgenbasierte Konstruktion unterstützt, die im Wesentlichen ein Argument an den Konstruktor übergibt. In diesem Fall ist das Argument, das übergeben wird, der [**Mode**](/uwp/api/windows.ui.xaml.data.relativesource.mode)-Wert.

Der **Self**-Modus ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. Dies stellt außerdem eine Variation der [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname)-Bindung dar, erfordert aber keine Benennung und keinen anschließenden Selbstverweis des Elements. Wenn Sie eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element binden, müssen die Eigenschaften den gleichen Eigenschaftentyp ausweisen. Andernfalls müssen Sie auch einen [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) für die Bindung verwenden, um die Werte zu konvertieren. Sie können beispielsweise [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) als Quelle für [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) ohne Konvertierung verwenden. Sie benötigen jedoch einen Konverter, um [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) als Quelle für [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility) zu nutzen.

Hier sehen Sie ein Beispiel. Dieses [**Rechteck**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) verwendet eine [{Binding}-Markuperweiterung](binding-markup-extension.md), damit [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) immer gleich sind und eine Darstellung als Quadrat erfolgt. Nur die Höhe wird als fester Wert festgelegt. Der standardmäßige [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) dieses **Rechtecks** ist **null** anstelle von **this**. Daher verwenden wir das `RelativeSource={RelativeSource Self}`-Argument in der Syntax der {Binding}-Markuperweiterung, um die Datenkontextquelle auf das eigentliche Objekt festzulegen (und die Bindung an ihre anderen Eigenschaften zu ermöglichen).

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Darüber hinaus kann `RelativeSource={RelativeSource Self}` auch verwendet werden, um den [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) eines Objekts auf sich selbst festzulegen.  Beispielsweise können Sie dieses Verfahren in einigen der SDK-Beispiele sehen, in denen die [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) -Klasse mit einer benutzerdefinierten Eigenschaft erweitert wurde, die bereits ein fertiges Ansichts Modell für die eigene Datenbindung bereitstellt, z. b.: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Hinweis**    Die XAML-Verwendung für **RelativeSource** zeigt nur die Verwendung an, für die Sie vorgesehen ist: das Festlegen eines Werts für " [**Binding. RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) " in XAML als Teil eines Bindungs Ausdrucks. Theoretisch sind andere Verwendungen möglich, wenn Sie eine Eigenschaft mit einem [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)-Wert festlegen.

## <a name="related-topics"></a>Zugehörige Themen

* [Übersicht über XAML](xaml-overview.md)
* [Datenbindung im Detail](../data-binding/data-binding-in-depth.md)
* [{Binding}-Markuperweiterung](binding-markup-extension.md)
* [**Bindung**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)