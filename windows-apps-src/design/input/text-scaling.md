---
description: Erstellen von Windows-apps und benutzerdefinierten/Vorlagen basierten Steuerelementen, die Platt Form Text Skalierung unterstützen
title: Textskalierung
label: Text scaling
template: detail.hbs
keywords: UWP, Text, Skalierung, Barrierefreiheit, "erleichterter Zugriff", Anzeige, "Text vergrößern", Benutzerinteraktion, Eingabe
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d47523ca69f8088d5e13ab944c5dd2be2d1d8ba
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860169"
---
# <a name="text-scaling"></a>Textskalierung

![Hero Image zeigt ein Beispiel für die Text Skalierung zwischen 100% und 225% an.](images/coretext/text-scaling-news-hero-small.png)  
*Beispiel für die Text Skalierung in Windows 10 (100% bis 225%)*

## <a name="overview"></a>Übersicht

Das Lesen von Text auf einem Computerbildschirm (vom mobilen Gerät zum Laptop zum Desktop Monitor auf den riesigen Bildschirm einer Surface Hub) kann für viele Personen eine Herausforderung darstellen. Im Gegensatz dazu finden einige Benutzer die Schriftart Größen, die in apps und Websites verwendet werden, um größer als notwendig zu sein.

Um sicherzustellen, dass der Text für die breiteste Palette von Benutzern so lesbar ist wie möglich, bietet Windows die Möglichkeit, die relative Schriftgröße sowohl im Betriebssystem als auch in den einzelnen Anwendungen zu ändern. Statt eine Bildschirmlupen-App zu verwenden(die normalerweise einfach alles in einem Bereich des Bildschirms vergrößert und eigene Nutzbarkeitsprobleme mit sich bringt), die Bildschirmauflösung zu ändern oder sich auf die DPI-Skalierung zu verlassen (bei der die Größen aller Elemente anhand der Anzeige und des typischen Betrachtungsabstands geändert werden), kann ein Benutzer schnell auf eine Einstellung zugreifen, um nur die Textgröße im Bereich von 100 % (Standardgröße) bis zu 225 % zu ändern.

## <a name="support"></a>Support

Universelle Windows-Anwendungen (Standard und PWA) unterstützen standardmäßig die Text Skalierung.

Wenn Ihre Windows-Anwendung benutzerdefinierte Steuerelemente, benutzerdefinierte Text Oberflächen, hart codierte Steuerelement Höhen, ältere Frameworks oder Drittanbieter-Frameworks umfasst, müssen Sie wahrscheinlich einige Updates vornehmen, um eine konsistente und nützliche Umgebung für Ihre Benutzer zu gewährleisten.  

DirectWrite, GDI und XAML-Austausch Vorgänge unterstützen die Text Skalierung nicht nativ, während die Win32-Unterstützung auf Menüs, Symbole und Symbolleisten beschränkt ist.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Benutzererfahrung

Benutzer können die textskala mit dem Schieberegler größeren Text vergrößern auf der Seite Einstellungen > Easy of Access-> Vision/Anzeige Bildschirm anpassen.

![Screenshot der Seite mit dem Bildschirm für die Benutzerfreundlichkeit/Anzeigeeinstellungen mit dem Schieberegler größeren Text erstellen](images/coretext/text-scaling-settings-100-small.png)  
*Textskalierungs Einstellung von Einstellungen-> Easy of Access-> Vision/Anzeige Bildschirm*

## <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

Wenn die Größe der Text Größe geändert wird, müssen Steuerelemente und Container auch die Größe und den Ablauf ändern, um den Text und das neue Layout zu unterstützen. Wie bereits erwähnt, werden in Abhängigkeit von der APP, dem Framework und der Plattform viele dieser Aufgaben für Sie erledigt. Die folgende UX-Anleitung behandelt die Fälle, in denen dies nicht der Fall ist.

### <a name="use-the-platform-controls"></a>Verwenden der Platt Form Steuerelemente

Haben wir das bereits gesagt? Es lohnt sich, sich zu wiederholen: Wenn möglich, verwenden Sie immer die integrierten Steuerelemente, die mit den verschiedenen Windows-App-Frameworks bereitgestellt werden, um für den geringsten Aufwand eine möglichst umfassende Benutzerumgebung zu erhalten.

Beispielsweise unterstützen alle UWP-Text Steuerelemente die vollständige Text Skalierung ohne Anpassung oder Vorlagen.

Im folgenden finden Sie einen Ausschnitt aus einer grundlegenden UWP-APP, die einige Standardtext Steuerelemente umfasst:

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Animation von Text Skalierung 100% bis 225%.](images/coretext/text-scaling.gif)  
*Animierte Text Skalierung*

### <a name="use-auto-sizing"></a>Verwenden der automatischen Größenanpassung

Geben Sie keine absoluten Größen für Ihre Steuerelemente an. Lassen Sie die Größe der Plattform nach Möglichkeit automatisch basierend auf den Benutzer-und Geräteeinstellungen ändern.  

In diesem Code Ausschnitt aus dem vorherigen Beispiel verwenden wir die `Auto` Width- `*` Werte und für einen Satz von Raster Spalten und ermöglichen der Plattform das Anpassen des App-Layouts basierend auf der Größe der im Raster enthaltenen Elemente.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Verwenden von Text Wrapping

Um sicherzustellen, dass das Layout Ihrer APP so flexibel und anpassungsfähig wie möglich ist, aktivieren Sie das Umwickeln von Text in jedem Steuerelement, das Text enthält (viele Steuerelemente unterstützen standardmäßig keine Text Umbrüchen).

Wenn Sie Text umschließen nicht angeben, verwendet die Plattform andere Methoden, um das Layout anzupassen, einschließlich Clipping (siehe vorheriges Beispiel).

Hier verwenden wir die `AcceptsReturn` `TextWrapping` Text Feldeigenschaften und, um sicherzustellen, dass unser Layout so flexibel wie möglich ist.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Animation von Text Skalierung von 100% bis 225% mit Text Umbrüchen.](images/coretext/text-scaling-textwrap.gif)  
*Animierte Text Skalierung mit Text Wrapping*

### <a name="specify-text-trimming-behavior"></a>Textkürzungs Verhalten angeben

Wenn Text Wrapping nicht das bevorzugte Verhalten ist, können Sie mit den meisten Text Steuerelementen entweder Ihren Text ausschneiden oder Ellipsen für das Text Kürzungs Verhalten angeben. Das Clipping wird als Auslassungs Zeichen bevorzugt, da Ellipsen selbst Platz belegen.

> [!NOTE]
> Wenn Sie den Text Abschneiden müssen, schneiden Sie das Ende der Zeichenfolge ab, nicht den Anfang.

In diesem Beispiel zeigen wir, wie Sie Text in einem TextBlock mithilfe der [texttrimmen](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) -Eigenschaft abschneiden.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Screenshot von Text Skalierung von 100% bis 225% mit Text Clipping.](images/coretext/text-scaling-clipping-small.png)  
*Text Skalierung mit Text Clipping*

### <a name="use-a-tooltip"></a>Verwenden einer QuickInfo

Wenn Sie Text abschneiden, verwenden Sie eine QuickInfo, um Ihren Benutzern den vollständigen Text bereitzustellen.

Hier fügen wir eine QuickInfo zu einem TextBlock hinzu, der keine Text Umbrüchen unterstützt:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Schriftart basierte Symbole oder Symbole nicht skalieren

Wenn Sie Schriftart basierte Symbole für die Betonung oder Ergänzung verwenden, deaktivieren Sie die Skalierung für diese Zeichen.

Legen Sie für die meisten XAML-Steuerelemente die Eigenschaft [istextscalefactor aktiviert](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) auf fest `false` .

### <a name="support-text-scaling-natively"></a>Unterstützung von nativer Text Skalierung

Behandeln Sie das " [textscalefactorchanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) uisettings"-System Ereignis in Ihrem benutzerdefinierten Framework und in den Steuerelementen. Dieses Ereignis wird jedes Mal ausgelöst, wenn der Benutzer den Faktor für die Text Skalierung auf dem System festlegt.

## <a name="summary"></a>Zusammenfassung

Dieses Thema enthält eine Übersicht über die Unterstützung von Text Skalierungen in Windows und enthält Anleitungen und Anleitungen für Entwickler zur Anpassung der Benutzerfreundlichkeit.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="api-reference"></a>API-Referenz

- [Istextscalefactor aktiviert](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [Textscalefactor Changed](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
