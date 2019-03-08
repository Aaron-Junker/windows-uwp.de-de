---
title: xDeferLoadStrategy-Attribut
description: „xDeferLoadStrategy“ verzögert die Erstellung eines Elements und seiner untergeordneten Elemente, verkürzt die Startzeit, erhöht aber leicht die Arbeitsspeicherauslastung. Jedes betroffene Element erhöht die Arbeitsspeicherauslastung um ca. 600 Bytes.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4432362db74f830774a2c4f74401c472c128a120
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659985"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy-Attribut

> [!IMPORTANT]
> Ab Windows 10, Version 1703 (Creators Update) wird **x:DeferLoadStrategy** durch das [**x:Load-Attribut**](x-load-attribute.md) ersetzt. Die Verwendung von `x:Load="False"` ist der von `x:DeferLoadStrategy="Lazy"` gleichzustellen, jedoch ermöglicht x:Load bei Bedarf das Entladen der Benutzeroberfläche. Weitere Informationen finden Sie unter [x:Load-Attribut](x-load-attribute.md).

Sie können **x:DeferLoadStrategy = "Lazy"** zur Optimierung des Starts oder der Strukturerstellungsleistung Ihrer XAML-App. Bei der Verwendung von **x:DeferLoadStrategy = "Lazy"** wird die Erstellung eines Elements und seiner untergeordneten Elemente verzögert, was zu einer verringerten Startzeit und Arbeitsspeicherkosten führt. Dies ist zur Reduzierung der Kosten für Elemente nützlich, die selten oder nur bedingt angezeigt werden. Das Element wird verwirklicht, wenn Code oder VisualStateManager darauf verweist.

Durch die Verfolgung von verzögerten Elementen durch das XAML-Framework werden ca. 600 Bytes zur Speicherverwendung für jedes betroffene Element hinzugefügt. Je größer die verzögerte Elementstruktur ist, desto mehr Startzeit sparen Sie – aber auf Kosten eines größeren Arbeitsspeicherbedarfs. Daher ist es möglich, dass durch eine übermäßige Verwendung dieses Attributs die Leistung beeinträchtigt wird.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Hinweise

Die Einschränkungen für die Verwendung von **x:DeferLoadStrategy** sind:

- Definieren Sie eine [X: Name](x-name-attribute.md) für das Element, wie es eine Möglichkeit, das Element später zu finden sein muss.
- Sie können nur Typen ableiten, die von einem [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) oder [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) abgeleitet werden.
- Die können keine Stammelemente in einer [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) oder [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) zurückstellen.
- Sie können keine Elemente in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) zurückstellen.
- Sie können keine losen XAML zurückstellen, die mit [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) geladen wurden.
- Durch das Verschieben eines übergeordneten Elements werden alle Elemente gelöscht, die nicht erkannt wurden.

Es gibt mehrere Möglichkeiten, verzögerte Elementen zu erkennen:

- Rufen Sie [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) mit dem Namen auf, der für das Element definiert wurde.
- Rufen Sie [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) mit dem Namen auf, der für das Element definiert wurde.
- Verwenden Sie in [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) eine [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)- oder **Storyboard**-Animation, die auf das verzögerte Element ausgerichtet ist.
- Legen Sie das verzögerte Elemente in allen **Storyboards** als Ziel fest.
- Verwenden Sie eine Bindung, die auf das verzögerte Element ausgerichtet ist.

> HINWEIS: Nach dem Start der Instanziierung eines Elements wird es der UI-Thread erstellt, damit es verursachen die Benutzeroberfläche auf, wenn zu Stocken, dass viel auf einmal erstellt wird.

Wenn ein verzögertes Element mit einer der oben aufgeführten Methoden erstellt wurde, passiert Folgendes:

- Das [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723)-Ereignis für das Element wird ausgelöst.
- Alle Bindungen für das Element werden ausgewertet.
- Wenn Sie sich für den Empfang von Benachrichtigungen über Änderungen an der Eigenschaft, die die verzögerten Elemente enthält, registriert haben, wird die Benachrichtigung ausgelöst.

Sie können verzögerte Elemente verschachteln, jedoch müssen Sie vom äußersten Element aus nach innen erkannt werden.  Wenn Sie versuchen, ein untergeordnetes Element zu erkennen, bevor das übergeordnete Element erkannt wurde, wird eine Ausnahme ausgelöst.

In der Regel wird empfohlen, dass Sie Elemente zurückstellen, die nicht im ersten Frame angezeigt werden. Bei der Suche nach zu verzögernden Kandidaten empfiehlt es sich, nach Elementen zu suchen, die mit reduzierter [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) erstellt werden. Eine Benutzeroberfläche, die durch eine Benutzerinteraktion ausgelöst wird, ist außerdem ein guter Ort zum Suchen nach Elementen, die zurückgestellt werden können.

Seien Sie vorsichtig beim Verzögern von Elementen in einem [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), da so die Startzeit verringert, aber auch die Leistung beim Verschieben reduziert werden kann – je nachdem, was Sie erstellen. Wenn Sie die Leistung beim Verschieben erhöhen möchten, finden Sie in den Dokumentationen [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md) und [x:Phase-Attribut](x-phase-attribute.md) weitere Informationen.

Wenn das [x:Phase-Attribut](x-phase-attribute.md) zusammen mit **x:DeferLoadStrategy** verwendet wird und ein Element oder eine Elementstruktur realisiert wird, werden die Bindungen bis einschließlich zur aktuellen Phase angewendet. Die für **x:Phase** angegebene Phase wirkt sich nicht auf die Verzögerung des Elements aus und diese lässt sich darüber nicht steuern. Wenn ein Listenelement beim Schwenken wiederverwendet wird, verhalten sich realisierte Elemente wie andere Elemente, und kompilierte Bindungen (**{x:Bind}**-Bindungen) werden unter Verwendung der gleichen Regeln einschließlich Phasing verarbeitet.

Eine allgemeine Richtlinie besteht darin, die Leistung Ihrer App vorher und nachher zu messen, um sicherzustellen, das Sie die gewünschte Leistung erhalten.

## <a name="example"></a>Beispiel

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
