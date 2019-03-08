---
description: Stellt eine Methode bereit, um die Quelle einer Bindung als relative Beziehung im Laufzeitobjektdiagramm anzugeben.
title: RelativeSource-Markuperweiterung
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 26cde97f82e6962d530721f1e0230138e5917016
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617925"
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

| Begriff | Beschreibung |
|------|-------------|
| {RelativeSource Self} | Erzeugt den [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915)-Wert <strong>Self</strong>. Das Zielelement sollte als Quelle für diese Bindung verwendet werden. Dies ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. |
| {RelativeSource TemplatedParent} | Erzeugt eine [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391), die als Quelle für diese Bindung angewendet wird. Dies ist nützlich, wenn Laufzeitinformationen in Bindungen auf Vorlagenebene angewendet werden sollen. | 

## <a name="remarks"></a>Hinweise

Mit einer [**Bindung**](https://msdn.microsoft.com/library/windows/apps/br209820) kann [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) entweder als Attribut für ein **Binding**-Objektelement oder als Komponente in einer [{Binding}-Markuperweiterung](binding-markup-extension.md) festgelegt werden. Aus diesem Grund werden zwei verschiedene Varianten der XAML-Syntax dargestellt.

**RelativeSource** ähnelt der [{Binding}-Markuperweiterung](binding-markup-extension.md).  Hierbei handelt es sich um eine Markuperweiterung, die Instanzen selbstständig zurückgeben kann und die eine zeichenfolgenbasierte Konstruktion unterstützt, die im Wesentlichen ein Argument an den Konstruktor übergibt. In diesem Fall ist das Argument, das übergeben wird, der [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915)-Wert.

Der **Self**-Modus ist nützlich, wenn eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element gebunden werden soll. Dies stellt außerdem eine Variation der [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)-Bindung dar, erfordert aber keine Benennung und keinen anschließenden Selbstverweis des Elements. Wenn Sie eine Eigenschaft eines Elements an eine andere Eigenschaft im gleichen Element binden, müssen die Eigenschaften den gleichen Eigenschaftentyp ausweisen. Andernfalls müssen Sie auch einen [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) für die Bindung verwenden, um die Werte zu konvertieren. Sie können beispielsweise [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) als Quelle für [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) ohne Konvertierung verwenden. Sie benötigen jedoch einen Konverter, um [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) als Quelle für [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006) zu nutzen.

Hier sehen Sie ein Beispiel. Dieses [**Rechteck**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) verwendet eine [{Binding}-Markuperweiterung](binding-markup-extension.md), damit [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) und [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) immer gleich sind und eine Darstellung als Quadrat erfolgt. Nur die Höhe wird als fester Wert festgelegt. Der standardmäßige [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) dieses **Rechtecks** ist **null** anstelle von **this**. Daher verwenden wir das `RelativeSource={RelativeSource Self}`-Argument in der Syntax der {Binding}-Markuperweiterung, um die Datenkontextquelle auf das eigentliche Objekt festzulegen (und die Bindung an ihre anderen Eigenschaften zu ermöglichen).

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Darüber hinaus kann `RelativeSource={RelativeSource Self}` auch verwendet werden, um den [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) eines Objekts auf sich selbst festzulegen.  Z. B. möglicherweise diese Technik in einigen der SDK-Beispiele angezeigt, in denen die [ **Seite** ](https://msdn.microsoft.com/library/windows/apps/br227503) -Klasse wurde erweitert, mit einer benutzerdefinierten Eigenschaft, die bereits ein bereit-zu-Go-Ansichtsmodell für eigene Datenbindung bereitstellt z. B.: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Beachten Sie**  der XAML-Verwendung für **RelativeSource** zeigt nur die Verwendung, die für die es bestimmt ist: Festlegen eines Werts für [ **Binding.RelativeSource** ](https://msdn.microsoft.com/library/windows/apps/br209831)in XAML als Teil von einem Bindungsausdruck. Theoretisch sind andere Verwendungen möglich, wenn Sie eine Eigenschaft mit einem [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)-Wert festlegen.

## <a name="related-topics"></a>Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Die Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [{Binding}-Markuperweiterung](binding-markup-extension.md)
* [**Bindung**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)

