---
title: xDeferLoadStrategy-Attribut
description: „xDeferLoadStrategy“ verzögert die Erstellung eines Elements und seiner untergeordneten Elemente, verkürzt die Startzeit, erhöht aber leicht die Arbeitsspeicherauslastung.Jedes betroffene Element erhöht die Arbeitsspeicherauslastung um ca. 600 Bytes.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f37c04af132e742a54df8e88e5c875a32fa94beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173774"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy-Attribut

> [!IMPORTANT]
> Ab Windows 10, Version 1703 (Creators Update), wird **x:deferloadstrategy** durch das [**x:Load-Attribut**](x-load-attribute.md)abgelöst. Die Verwendung `x:Load="False"` von entspricht `x:DeferLoadStrategy="Lazy"` , bietet jedoch die Möglichkeit, die Benutzeroberfläche bei Bedarf zu entladen. Weitere Informationen finden Sie unter dem [x:Load-Attribut](x-load-attribute.md) .

Sie können **x:deferloadstrategy = "Lazy"** verwenden, um die Leistung der Start-oder Baum Erstellung Ihrer XAML-APP zu optimieren. Wenn Sie **x:deferloadstrategy = "Lazy"** verwenden, wird die Erstellung eines Elements und seiner untergeordneten Elemente verzögert, wodurch die Startzeit und die Arbeitsspeicher Kosten reduziert werden. Dies ist hilfreich, um die Kosten für Elemente zu reduzieren, die selten oder bedingt angezeigt werden. Das-Element wird erkannt, wenn auf das Element von Code oder visualstatus Manager verwiesen wird.

Bei der Nachverfolgung von verzögerten Elementen durch das XAML-Framework werden allerdings ungefähr 600 Byte der Speicherauslastung für jedes betroffene Element hinzugefügt. Je größer die verzögerte Elementstruktur ist, desto mehr Startzeit sparen Sie – aber auf Kosten eines größeren Arbeitsspeicherbedarfs. Daher ist es möglich, dieses Attribut in dem Ausmaß zu überschreiten, in dem sich die Leistung verringert.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Bemerkungen

Die Einschränkungen für die Verwendung von **x:DeferLoadStrategy** sind:

- Sie müssen einen [x:Name](x-name-attribute.md)   für das-Element definieren, da es eine Möglichkeit gibt, das-Element zu einem späteren Zeitpunkt zu finden.
- Sie können nur Typen verzögern, die von " [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) " oder " [**flyoutbase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase)" abgeleitet werden.
- Stamm Elemente können nicht auf einer [**Seite**](/uwp/api/windows.ui.xaml.controls.page), einem [**UserControl-Steuer**](/uwp/api/windows.ui.xaml.controls.usercontrol)Element oder auf einem [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate)-Element verschoben werden.
- Elemente in einem [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)können nicht verschoben werden.
- Mit " [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)" geladene lose XAML kann nicht verschoben werden.
- Durch das Verschieben eines übergeordneten Elements werden alle Elemente gelöscht, die nicht erkannt wurden.

Es gibt mehrere Möglichkeiten, verzögerte Elementen zu erkennen:

- Ruft [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) mit dem Namen auf, den Sie für das Element definiert haben.
- Aufrufen von [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) mit dem Namen, den Sie für das Element definiert haben.
- Verwenden Sie in einem [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState)eine [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) -oder **Storyboard** -Animation, die das verzögerte Element als Ziel hat.
- Legen Sie das verzögerte Elemente in allen **Storyboards** als Ziel fest.
- Verwenden Sie eine Bindung, die das verzögerte Element als Ziel hat.

> HINWEIS: Nach Start der Instanziierung eines Elements wird es im Benutzeroberflächen-Thread erstellt. Dies kann ggf. bewirken, dass die Oberfläche ruckelt, wenn zu viele auf einmal erstellt werden.

Nachdem ein verzögertes Element in einer der zuvor aufgeführten Methoden erstellt wurde, geschieht Folgendes:

- Das [**geladene**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) -Ereignis für das-Element wird ausgelöst.
- Alle Bindungen für das Element werden ausgewertet.
- Wenn Sie sich für das Empfangen von Benachrichtigungen über Eigenschafts Änderungen für die Eigenschaft registriert haben, die das verzögerte Element (en) enthält, wird die Benachrichtigung ausgelöst.

Sie können verzögerte Elemente verschachteln, jedoch müssen Sie vom äußersten Element aus nach innen erkannt werden. Wenn Sie versuchen, ein untergeordnetes Element zu erkennen, bevor das übergeordnete Element erkannt wurde, wird eine Ausnahme ausgelöst.

In der Regel wird empfohlen, Elemente zurückzustellen, die im ersten Frame nicht angezeigt werden können.Bei der Suche nach zu verzögernden Kandidaten empfiehlt es sich, nach Elementen zu suchen, die mit reduzierter [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) erstellt werden. Außerdem ist die Benutzeroberfläche, die durch Benutzerinteraktion ausgelöst wird, ein guter Ausgangspunkt, um nach Elementen zu suchen, die Sie verzögern können.

Seien Sie vorsichtig, wenn Sie Elemente in einer [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)verzögern, da dadurch die Startzeit verringert wird, Sie können jedoch auch die Schwenk Leistung verringern, je nachdem, was Sie erstellen. Wenn Sie die Schwenk Leistung erhöhen möchten, finden Sie weitere Informationen in der Dokumentation zu [{x:Bind} Markup Erweiterung](x-bind-markup-extension.md) und [x:Phase-Attribut](x-phase-attribute.md) .

Wenn das [x:Phase-Attribut](x-phase-attribute.md) in Verbindung mit **x:deferloadstrategy** verwendet wird, werden die Bindungen bis einschließlich der aktuellen Phase angewendet, wenn ein Element oder eine Elementstruktur realisiert wird. Die für **x:Phase** angegebene Phase wirkt sich nicht auf die Verzögerung des Elements aus oder steuert diese nicht. Wenn ein Listenelement im Rahmen der schwenken wieder verwendet wird, Verhalten sich erkannte Elemente auf dieselbe Weise wie andere aktive Elemente, und kompilierte Bindungen (**{x:Bind}** -Bindungen) werden mit denselben Regeln, einschließlich der Phasen Verarbeitung, verarbeitet.

Eine allgemeine Richtlinie besteht darin, die Leistung Ihrer APP vor und nach zu messen, um sicherzustellen, dass Sie die gewünschte Leistung erhalten.

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