---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Flyout-Steuerelementen
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows10, UWP
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e68f8f48ca9ba67a29c8a52a5d59767a080f642b
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "7119430"
---
# <a name="flyouts"></a>Flyouts

Ein Flyout ist ein einfach ausblendbarer Container, der beliebige UI als Inhalt anzeigen kann. Flyouts können andere Flyouts oder Kontextmenüs enthalten, um eine geschachtelte Umgebung zu kreieren.

![Kontextmenü geschachtelt in einem Flyout](../images/flyout-nested.png)

> **Wichtige APIs**: [Flyout-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

* Verwenden Sie Flyouts nicht anstelle von [QuickInfos](../tooltips.md) oder [Kontextmenüs](../menus.md). Verwenden Sie QuickInfos, um kurze Beschreibungen anzuzeigen, die nach einer festgelegten Zeit ausgeblendet werden. Verwenden Sie ein Kontextmenü für kontextbezogene Aktionen im Zusammenhang mit UI-Elementen (beispielsweise Kopieren und Einfügen).

Empfehlungen dazu, wann Sie ein Flyout verwendet werden und wann ein Dialogfeld (ein ähnliches Steuerelement), finden Sie unter [Dialogfelder und Flyouts](index.md). 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> oder <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>So erstellen Sie ein flyout


Flyouts sind an bestimmte Steuerelemente angefügt. Sie können mit der [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement)-Eigenschaft angeben, wo das Flyout angezeigt werden soll: oben, links, unten, rechts oder als Vollbild. Wenn Sie den [vollständigen Platzierungsmodus](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) auswählen, streckt die App das Flyout und zentriert es innerhalb des App-Fensters. Einige Steuerelemente wie z.B. [Schaltflächen](/uwp/api/Windows.UI.Xaml.Controls.Button), verfügen über eine [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout)-Eigenschaft, mit der Sie ein Flyout oder [Kontextmenü](../menus.md) zuordnen können.

In diesem Beispiel wird ein einfaches Flyout erstellt, das Text anzeigt, wenn die Schaltfläche gedrückt wird.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Wenn das Steuerelement nicht über eine Flyout-Eigenschaft verfügt, können Sie stattdessen die angefügte Eigenschaft [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) verwenden. In diesem Fall müssen Sie zudem die [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_)-Methode aufrufen, um das Flyout anzuzeigen.

In diesem Beispiel wird einem Bild ein einfaches Flyout hinzugefügt. Wenn der Benutzer auf das Bild tippt, zeigt die App das Flyout an.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

In den vorherigen Beispielen wurden die Flyouts inline definiert. Sie können ein Flyout auch als statische Ressource definieren und sie dann mit mehreren Elementen verwenden. In diesem Beispiel wird ein komplizierteres Flyout erstellt, mit dem eine größere Version eines Bilds angezeigt wird, wenn auf die Miniaturansicht getippt wird.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

## <a name="style-a-flyout"></a>Gestalten eines Flyouts
Um ein Flyout zu formatieren, ändern Sie den [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). In diesem Beispiel wird ein Absatz mit Textumbruch dargestellt, was den Textblock für Bildschirmleseprogramme zugänglich gemacht.

![Zugängliches Flyout mit Textumbruch](../images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="styling-flyouts-for-10-foot-experiences"></a>Formatieren von Flyouts für 10 Fuß-Umgebungen

Einfach ausblendbare Steuerelemente wie Flyouts erhalten den Tastatur- bzw. Gamepad-Fokus, bis sie nicht mehr angezeigt werden. Um dieses Verhalten optisch zu kennzeichnen, werden diese einfach ausblendbaren Steuerelemente auf der Xbox als Überlagerung gezeichnet, wobei der Kontrast und die Helligkeit bzw. Sichtbarkeit der umgebenden Benutzeroberfläche reduziert wird. Dieses Verhalten kann mit der Eigenschaft [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) geändert werden. Standardmäßig erhalten Flyouts auf der Xbox (jedoch nicht auf anderen Gerätefamilien) die einfach ausblendbare Überlagerung. Apps können jedoch durchsetzen, dass die Überlagerung stets **An** oder stets **Aus** ist.

![Flyout mit Abblend-Überlagerung](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>Verhalten für einfaches Ausblenden
Flyouts können schnell mit einer einfachen Ausblend-Aktion geschlossen werden. Dazu zählen:
-   Tippen außerhalb des Flyouts
-   Drücken der ESC-Taste auf der Tastatur
-   Drücken der Zurück-Taste des Systems (Hardware oder Software)
-   Drücken der B-Taste auf dem Gamepad

Wird das Ausblenden durch Tippen vorgenommen, wird die Geste in der Regel absorbiert und nicht an die UI unterhalb weitergegeben. Ist beispielsweise hinter einem geöffneten Flyout eine Schaltfläche sichtbar, wird durch einfaches Antippen durch den Benutzer das Flyout ausgeblendet, ohne jedoch diese Schaltfläche zu aktivieren. Um die Schaltfläche zu drücken, ist ein weiteres Antippen nötig.

Sie können dieses Verhalten ändern, indem Sie die Schaltfläche als Pass-Through-Eingabeelement für das Flyout gestalten. Das Flyout wird wie oben beschriebenen durch die einfache Ausblend-Aktion geschlossen, gibt den Antipp-Vorgang aber gleichzeitig an das entsprechende `OverlayInputPassThroughElement` weiter. Erwägen Sie, dieses Verhalten zu übernehmen, um Interaktionen des Benutzers bei ähnlich funktionierenden Elementen zu beschleunigen. Wenn Ihre App eine Sammlung von Favoriten beinhaltet und jedes Element der Sammlung ein angefügtes Flyout enthält, kann man davon auszugehen, dass die Benutzer mehrere Flyouts in schneller Abfolge abarbeiten möchten.

[!NOTE] Achten Sie darauf, kein Überlagerungseingabeelement mit Pass-Through festzulegen, da dies in einer destruktiven Aktion resultiert. Die Benutzer sind an diskrete, einfach ausblendbare Aktionen gewöhnt, die die Primär-UI nicht aktivieren. Schließen, Löschen oder ähnlich destruktive Schaltflächen sollten nicht über einfach ausblendbare Elemente aktiviert werden, um unerwartetes und störendes Verhalten zu vermeiden.

Im folgenden Beispiel werden alle drei Schaltflächen in der Favoritenleiste durch einfaches Antippen aktiviert.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementkatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel
- [QuickInfos](../tooltips.md)
- [Menüs und Kontextmenü](../menus.md)
- [Flyout-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)