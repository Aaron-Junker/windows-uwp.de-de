---
title: xLoad-Attribut
description: xLoad ermöglicht die dynamische Erstellung und Zerstörung eines Elements und seiner untergeordneten Elemente, wodurch die Startzeit und Speicherverwendung gekürzt wird.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1fa0f12779ad56d57c92f667443644851dc3d5e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629365"
---
# <a name="xload-attribute"></a>x:Load-Attribut

Sie können **X: Laden** verwende, um den Start, die Erstellung der visuellen Struktur und die Speicherauslastung Ihrer XAML-App zu optimieren. Die Verwendung von **X:Load** hat einen ähnlichen visuellen Effekt auf die **Sichtbarkeit**, mit Ausnahme der Tatsache, dass wenn das Element nicht geladen wird, sein Speicher freigegeben und intern ein kleiner Platzhalter verwendet wird, um seine Position in der visuellen Struktur zu markieren.

Das dem X:Load zugeordnete UI-Element kann über Code oder mithilfe eines [X: Bind](x-bind-markup-extension.md)-Ausdrucks geladen und entladen werden. Dies ist zur Reduzierung der Kosten für Elemente nützlich, die selten oder nur bedingt angezeigt werden. Bei der Verwendung von X:Load an einem Container wie z. B. Raster oder StackPanel, wird der Container und all seine untergeordneten Elemente als Gruppe geladen und entladen.

Die Verfolgung der verzögerten Elemente vom XAML-Framework fügt der Speicherverwendung für jedes dem x:Load zugeordnete Element ca. 600 Bytes hinzu, um den Platzhalter zu berücksichtigen. Daher ist es möglich, dass durch eine übermäßige Verwendung dieses Attributs die Leistung tatsächlich beeinträchtigt wird. Es wird empfehlen, dass Sie es nur für Elemente verwenden, die ausgeblendet werden müssen. Wenn Sie x:Load an einem Container verwenden, wird der ist der Aufwand nur für das Element mit dem Attribut x:Load bezahlt.

> [!IMPORTANT]
> Das Attribut X: Load ist verfügbar ab Windows 10, Version 1703 (Creators Update). Die Mindestversion, auf die Ihr Visual Studio-Projekt abzielt, muss *Windows 10 Creators Update (10.0, Build 15063)* sein, damit x:Load verwendet werden kann.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Laden von Elementen

Es gibt mehrere Möglichkeiten, Elementen zu laden:

- Verwenden Sie einen [x:Bind](x-bind-markup-extension.md)-Ausdruck, um den Ladezustand zu bestimmen. Der Ausdruck sollte **true** zum Laden und **false** zum Entladen des Elements zurückgeben.
- Rufen Sie [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) mit dem Namen auf, der für das Element definiert wurde.
- Rufen Sie [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) mit dem Namen auf, der für das Element definiert wurde.
- Verwenden Sie in [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) eine [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817)- oder **Storyboard**-Animation, die das x:Load-Element als Ziel hat.
- Legen Sie das entladene Element in allen **Storyboards** als Ziel fest.

> HINWEIS: Nach dem Start der Instanziierung eines Elements wird es der UI-Thread erstellt, damit es verursachen die Benutzeroberfläche auf, wenn zu Stocken, dass viel auf einmal erstellt wird.

Wenn ein verzögertes Element mit einer der oben aufgeführten Methoden erstellt wurde, passiert Folgendes:

- Das [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723)-Ereignis für das Element wird ausgelöst.
- Das Feld für x:Name wird festgelegt.
- Alle x:Bind-Bindungen für das Element werden ausgewertet.
- Wenn Sie sich für den Empfang von Benachrichtigungen über Änderungen an der Eigenschaft, die die verzögerten Elemente enthält, registriert haben, wird die Benachrichtigung ausgelöst.

## <a name="unloading-elements"></a>Entladen von Elementen

Für das Entladen eines Elements:

- Verwenden Sie einen x:Bind-Ausdruck, um den Ladezustand zu bestimmen. Der Ausdruck sollte **true** zum Laden und **false** zum Entladen des Elements zurückgeben.
- Rufen Sie auf einer Seite oder in einem Benutzersteuerelement **UnloadObject** auf und übergeben Sie die Objektreferenz.
- Rufen Sie **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** auf und übergeben Sie die Objektreferenz.

Wenn ein Objekt entladen wurde, wird es in der Struktur durch einen Platzhalter ersetzt. Die Objektinstanz verbleibt im Arbeitsspeicher, bis alle Verweise freigegeben wurden. Die UnloadObject-API auf einer Seite/einem Benutzersteuerelement soll die Verweise freigeben, die vom CodeGen für x:Name und x:Bind gespeichert werden. Wenn Sie zusätzliche Verweise im App-Code halten, müssen diese auch freigegeben werden.

Wenn ein Element entladen wird, werden alle dem Element zugeordneten Zustände verworfen. Stellen Sie daher sicher, dass alle Zustände bei Verwendung von x:Load als eine optimierte Version der Sichtbarkeit über Bindungen oder als Code erneut angewendet werden, wenn das geladene Ereignis ausgelöst wird.

## <a name="restrictions"></a>Einschränkungen

Die Einschränkungen für die Verwendung von **x:Load** sind:

- Definieren Sie eine [X: Name](x-name-attribute.md) für das Element, wie es eine Möglichkeit, das Element später zu finden sein muss.
- Sie können x:Load nur an Typen anwenden, die von [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) oder [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) ableiten.
- Sie können x:Load nicht für Stammelemente in einer [**Seite**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), einem [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol), oder einem [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) anwenden.
- Sie können x:Load nicht an Elemente in einem [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) anwenden.
- Sie können x:Load nicht für lose geladenem XAML mit [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) anwenden.
- Durch das Verschieben eines übergeordneten Elements werden alle Elemente gelöscht, die nicht geladen wurden.

## <a name="remarks"></a>Hinweise

Sie können x:Load für geschachtelte Elemente anwenden, jedoch müssen Sie vom äußersten Element aus nach innen erkannt werden.  Wenn Sie versuchen, ein untergeordnetes Element zu erkennen, bevor das übergeordnete Element erkannt wurde, wird eine Ausnahme ausgelöst.

In der Regel wird empfohlen, dass Sie Elemente zurückstellen, die nicht im ersten Frame angezeigt werden. Bei der Suche nach zu verzögernden Kandidaten empfiehlt es sich, nach Elementen zu suchen, die mit reduzierter [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) erstellt werden. Eine Benutzeroberfläche, die durch eine Benutzerinteraktion ausgelöst wird, ist außerdem ein guter Ort zum Suchen nach Elementen, die zurückgestellt werden können.

Seien Sie vorsichtig beim Verzögern von Elementen in einem [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), da so die Startzeit verringert, aber auch die Leistung beim Verschieben reduziert werden kann – je nachdem, was Sie erstellen. Wenn Sie die Leistung beim Verschieben erhöhen möchten, finden Sie in den Dokumentationen [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md) und [x:Phase-Attribut](x-phase-attribute.md) weitere Informationen.

Wenn das [x:Phase-Attribut](x-phase-attribute.md) zusammen mit **x:Load** verwendet wird und ein Element oder eine Elementstruktur realisiert wird, werden die Bindungen bis einschließlich zur aktuellen Phase angewendet. Die für **x:Phase** angegebene Phase wirkt sich auf den Ladezustand des Elements aus und diese lässt sich darüber steuern. Wenn ein Listenelement beim Schwenken wiederverwendet wird, verhalten sich realisierte Elemente wie andere Elemente, und kompilierte Bindungen (**{x:Bind}**-Bindungen) werden unter Verwendung der gleichen Regeln einschließlich Phasing verarbeitet.

Eine allgemeine Richtlinie besteht darin, die Leistung Ihrer App vorher und nachher zu messen, um sicherzustellen, das Sie die gewünschte Leistung erhalten.

## <a name="example"></a>Beispiel

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

