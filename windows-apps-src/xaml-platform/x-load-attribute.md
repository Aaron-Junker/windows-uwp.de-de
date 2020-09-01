---
title: xload-Attribut
description: xload ermöglicht die dynamische Erstellung und Zerstörung eines Elements und seiner untergeordneten Elemente, wodurch die Startzeit und die Speicherauslastung gesenkt werden.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1b110091c1a1f208ad06ea52f5c84dc7daad56c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161684"
---
# <a name="xload-attribute"></a>x:Load-Attribut

Sie können **x:Load** verwenden, um den Start, die visuelle Struktur Erstellung und die Speicherauslastung ihrer XAML-APP zu optimieren. Die Verwendung von **x:Load** hat einen ähnlichen visuellen Effekt wie **Sichtbarkeit**, außer wenn das Element nicht geladen wird, wird sein Arbeitsspeicher freigegeben, und intern wird ein kleiner Platzhalter verwendet, um die Position in der visuellen Struktur zu markieren.

Das Benutzeroberflächen Element, das mit "x:Load" attributiert wurde, kann über Code geladen und entladen werden oder mithilfe eines [x:Bind](x-bind-markup-extension.md) -Ausdrucks. Dies ist hilfreich, um die Kosten für Elemente zu reduzieren, die selten oder bedingt angezeigt werden. Wenn Sie x:Load für einen Container wie Grid oder StackPanel verwenden, werden der Container und alle untergeordneten Elemente als Gruppe geladen oder entladen.

Die Nachverfolgung von verzögerten Elementen durch das XAML-Framework fügt der Speicherauslastung für jedes mit x:Load zugeordneten Element ungefähr 600 Bytes hinzu, um den Platzhalter zu berücksichtigen. Daher ist es möglich, dieses Attribut in dem Ausmaß zu überschreiten, in dem sich die Leistung tatsächlich verringert. Es wird empfohlen, dass Sie Sie nur für Elemente verwenden, die ausgeblendet werden müssen. Wenn Sie x:Load für einen Container verwenden, wird der Aufwand nur für das-Element mit dem x:Load-Attribut bezahlt.

> [!IMPORTANT]
> Das Attribut x:Load ist ab Windows 10, Version 1703 (Creators Update) verfügbar. Die minimale Version Ihres Visual Studio-Projekts muss *Windows 10 Creators Update (10,0, Build 15063)* sein, damit x:loadverwendet werden kann.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Elemente werden geladen.

Es gibt verschiedene Möglichkeiten, die Elemente zu laden:

- Verwenden Sie einen [x:Bind](x-bind-markup-extension.md) -Ausdruck zum Angeben des Ladezustands. Der Ausdruck sollte **true** zurückgeben, um geladen zu werden, und **false** zum Entladen des Elements.
- Ruft [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) mit dem Namen auf, den Sie für das Element definiert haben.
- Aufrufen von [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) mit dem Namen, den Sie für das Element definiert haben.
- Verwenden Sie in einem [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)eine [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) -oder **Storyboard** -Animation, die das x:Load-Element als Ziel hat.
- Das entladene Element in einem beliebigen **Storyboard**als Ziel.

> HINWEIS: Nach Start der Instanziierung eines Elements wird es im Benutzeroberflächen-Thread erstellt. Dies kann ggf. bewirken, dass die Oberfläche ruckelt, wenn zu viele auf einmal erstellt werden.

Nachdem ein verzögertes Element in einer der zuvor aufgeführten Methoden erstellt wurde, geschieht Folgendes:

- Das [**geladene**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) -Ereignis für das-Element wird ausgelöst.
- Das-Feld für "x:Name" wird festgelegt.
- Alle x:Bind-Bindungen für das Element werden ausgewertet.
- Wenn Sie sich für das Empfangen von Benachrichtigungen über Eigenschafts Änderungen für die Eigenschaft registriert haben, die das verzögerte Element (en) enthält, wird die Benachrichtigung ausgelöst.

## <a name="unloading-elements"></a>Entladen von Elementen

So entladen Sie ein Element:

- Verwenden Sie einen x:Bind-Ausdruck zum Angeben des Ladezustands. Der Ausdruck sollte **true** zurückgeben, um geladen zu werden, und **false** zum Entladen des Elements.
- In einer Seite oder einem Benutzer Steuerelement wird **unloadobject** aufgerufen und der Objekt Verweis übergeben.
- Ruft **Windows. UI. XAML. Markup. xamlmarkuphelper. unloadobject** auf und übergibt den Objekt Verweis.

Wenn ein Objekt entladen wird, wird es in der Struktur durch einen Platzhalter ersetzt. Die Objektinstanz verbleibt im Arbeitsspeicher, bis alle Verweise freigegeben wurden. Die unloadobject-API auf einer Seite/einem UserControl-Steuerelement wurde entwickelt, um die von CodeGen für x:Name und x:bindbaren Verweise freizugeben. Wenn Sie zusätzliche Verweise im App-Code enthalten, müssen Sie auch veröffentlicht werden.

Wenn ein Element entladen wird, werden alle Zustände, die dem Element zugeordnet sind, verworfen. Wenn Sie x:Load als optimierte Version der Sichtbarkeit verwenden, stellen Sie sicher, dass der gesamte Zustand über Bindungen angewendet wird, oder wird durch Code erneut angewendet, wenn das geladene Ereignis ausgelöst wird.

## <a name="restrictions"></a>Beschränkungen

Die folgenden Einschränkungen gelten für die Verwendung von **x:Load** :

- Sie müssen einen [x:Name](x-name-attribute.md)   für das-Element definieren, da es eine Möglichkeit gibt, das-Element zu einem späteren Zeitpunkt zu finden.
- Sie können x:Load nur für Typen verwenden, die von " [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) " oder " [**flyoutbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase)" abgeleitet werden.
- Sie können x:Load nicht für Stamm Elemente in einer [**Seite**](/uwp/api/windows.ui.xaml.controls.page), einem [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)oder einem [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate)verwenden.
- X:Load kann nicht für Elemente in einem [**ResourceDictionary verwendet werden**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Sie können x:Load nicht für lose XAML verwenden, die mit [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)geladen wurde.
- Durch das Verschieben eines übergeordneten Elements werden alle Elemente gelöscht, die nicht geladen wurden.

## <a name="remarks"></a>Bemerkungen

Sie können x:Load für in der Liste eingefügte Elemente verwenden, Sie müssen jedoch über das äußerste Element in realisiert werden. Wenn Sie versuchen, ein untergeordnetes Element zu erkennen, bevor das übergeordnete Element erkannt wurde, wird eine Ausnahme ausgelöst.

In der Regel wird empfohlen, Elemente zurückzustellen, die im ersten Frame nicht angezeigt werden können.Bei der Suche nach zu verzögernden Kandidaten empfiehlt es sich, nach Elementen zu suchen, die mit reduzierter [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) erstellt werden. Außerdem ist die Benutzeroberfläche, die durch Benutzerinteraktion ausgelöst wird, ein guter Ausgangspunkt, um nach Elementen zu suchen, die Sie verzögern können.

Seien Sie vorsichtig, wenn Sie Elemente in einer [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)verzögern, da dadurch die Startzeit verringert wird, Sie können jedoch auch die Schwenk Leistung verringern, je nachdem, was Sie erstellen. Wenn Sie die Schwenk Leistung erhöhen möchten, finden Sie weitere Informationen in der Dokumentation zu [{x:Bind} Markup Erweiterung](x-bind-markup-extension.md) und [x:Phase-Attribut](x-phase-attribute.md) .

Wenn das [x:Phase-Attribut](x-phase-attribute.md) in Verbindung mit **x:Load** verwendet wird, und wenn ein Element oder eine Elementstruktur realisiert wird, werden die Bindungen bis einschließlich der aktuellen Phase übernommen. Die für **x:Phase** angegebene Phase wirkt sich auf den Ladezustand des Elements aus oder steuert diesen. Wenn ein Listenelement im Rahmen der schwenken wieder verwendet wird, Verhalten sich erkannte Elemente auf die gleiche Weise wie andere aktive Elemente, und kompilierte Bindungen (**{x:Bind}** -Bindungen) werden mit denselben Regeln, einschließlich der Phasen Verarbeitung, verarbeitet.

Eine allgemeine Richtlinie besteht darin, die Leistung Ihrer APP vor und nach zu messen, um sicherzustellen, dass Sie die gewünschte Leistung erhalten.

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