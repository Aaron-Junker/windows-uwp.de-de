---
Description: Erstellen von UWP-apps und benutzerdefinierte/tabellenlose Steuerelemente, die die Plattform Text skalieren zu unterstützen.
title: Textskalierung
label: Text scaling
template: detail.hbs
keywords: UWP, Text, Skalierung, Barrierefreiheit, "erleichterte Bedienung", "Stellen Text größer", eine Benutzerinteraktion, Eingabe anzeigen
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600815"
---
# <a name="text-scaling"></a>Textskalierung

![Beispiel für Text, die Skalierung von 100 bis 225 %](images/coretext/text-scaling-news-hero-small.png)  
*Beispiel für Text, die Skalierung in Windows 10 (100 % bis 225 %)*

## <a name="overview"></a>Übersicht

Lesen von Text auf einem Computerbildschirm (vom mobilen Gerät Laptop, desktop-Monitor zum riesigen Bildschirm von einem Surface Hub) kann für viele Benutzer schwierig sein. Im Gegensatz dazu finden Sie einige Benutzer die Schriftgrade Ihren Bedürfnissen entsprechend in apps und Websites verwendet, um die stärker als notwendig werden.

Um sicherzustellen, dass Text wie möglich für die breiteste Palette an Benutzer genauso lesbar ist, bietet Windows die Möglichkeit, Benutzern das Ändern der relativen Schriftgröße für das Betriebssystem und für einzelne Anwendungen. Statt mithilfe einer Bildschirmlupe-app (die in der Regel einfach alles, was in einem Bereich des Bildschirms vergrößert und führt eine eigene Probleme hinsichtlich der Verwendbarkeit), ändern die Bildschirmauflösung oder der vertrauenden Seite, auf die DPI-Skalierung (das wird alles, was basierend auf der Anzeige und typische anzeigen (Abstand), ein Benutzer zugreifen kann schnell eine Einstellung, um die Größe von nur-Text, im Bereich von 100 % (die Standardgröße) bis zu 225 %.

## <a name="support"></a>Support

Universelle Windows-Anwendungen (sowohl für standardmäßige und PWA), Text in der Standardeinstellung skalieren zu unterstützen.

Wenn Ihrer UWP-Anwendung benutzerdefinierte Steuerelemente, benutzerdefinierten Text Oberflächen, hartcodierte Steuerelement Höhe, älteren Frameworks oder 3rd Party-Frameworks enthält, müssen Sie wahrscheinlich einige Updates, um sicherzustellen, dass eine konsistente und nützliche Erfahrungen Ihrer Benutzer zu machen.  

DirectWrite, GDI und XAML-SwapChainPanels unterstützen systemintern Text zu skalieren, keine zwar Unterstützung für Win32-Symbole, Menüs und Symbolleisten auf.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Benutzererfahrung

Benutzer können Text Skalierung anpassen, mit dem größeren Schieberegler auf den Einstellungen -> Stellen-Text-erleichterte Bedienung Vision/Bildschirmanzeige >.

![Beispiel für Text, die Skalierung von 100 bis 225 %](images/coretext/text-scaling-settings-100-small.png)  
*Text-größeneinstellung aus den Einstellungen -> erleichterte Bedienung Vision/Bildschirmanzeige ->*

## <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

Wie der Text geändert wird, Steuerelemente und Container müssen auch die Größe und dynamisch umgebrochen, um den Text und das neue Layout zu bieten. Wie bereits erwähnt, abhängig von der app, Framework und Plattform, ist der Großteil der Arbeit für Sie erledigt. Die folgende Anleitung für die UX behandelt Fälle, in denen es nicht ist.

### <a name="use-the-platform-controls"></a>Verwenden Sie die Plattformsteuerelemente

Sagen wir dies bereits? Es lohnt sich wiederholen: Verwenden Sie nach Möglichkeit immer die integrierten Steuerelemente, die mit den verschiedenen Windows-app-Frameworks bereitgestellt auf, um die umfassende benutzerfreundlichkeit für die geringstmöglichen Aufwand zu erzielen.

Alle UWP-Text-Steuerelemente unterstützen z. B. den vollständigen Text Skalierung ohne Anpassung oder Vorlagen.

Hier ist ein Ausschnitt aus einer einfachen UWP-app, die eine Reihe von standard-Text-Steuerelemente enthält:

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

![Skalieren von 100 bis 225 % Textanimation](images/coretext/text-scaling.gif)  
*Animierte Text skalieren*

### <a name="use-auto-sizing"></a>Verwenden Sie die automatische größenanpassung

Geben Sie die absolute Größen für die Steuerelemente nicht. Wann immer möglich, können Sie die Plattform, die Größe Ihrer Steuerelemente automatisch basierend auf Benutzer-und geräteeinstellungen ändern.  

In diesem Codeausschnitt aus dem vorherigen Beispiel verwenden wir die `Auto` und `*` passen Sie Werte für eine Gruppe von Spalten des Rasters und lassen die Plattform für die Breite des app-Layouts, die basierend auf der Größe der Elemente innerhalb des Rasters.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Verwenden Sie den Textumbruch

Um sicherzustellen, dass das Layout der app so flexibel und anpassungsbereit wie möglich ist, aktivieren Sie das Umbrechen von Text in jedem Steuerelement, das Text enthält (viele Steuerelemente nicht Umbrechen von Text in der Standardeinstellung unterstützen).

Wenn Sie den Textumbruch nicht angeben, die Plattform die andere Methoden zum Anpassen des Layouts, einschließlich Clipping verwendet (siehe vorherigen Beispiel).

Hier verwenden wir die `AcceptsReturn` und `TextWrapping` Textfeldeigenschaften, um sicherzustellen, dass unsere Layout ist so flexibel wie möglich.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Animierte Text Skalieren von 100 bis 225 % mit Umbrechen von text](images/coretext/text-scaling-textwrap.gif)  
*Skalieren von Daten in den Textumbruch Textanimation*

### <a name="specify-text-trimming-behavior"></a>Textzuschnittverhalten angeben

Ist Umbrechen von Text nicht empfohlen, können die meisten Textsteuerelemente entweder Beschneiden von Text oder Auslassungspunkte für das textzuschnittverhalten angeben. Clipping wird bevorzugt, Ellipsen, da Ellipsen Speicherplatz selbst in Anspruch nehmen.

> [!NOTE]
> Wenn Sie für das Abschneiden von Text müssen, schneiden Sie das Ende der Zeichenfolge, die nicht am Anfang.

In diesem Beispiel zeigen wir, wie für das Abschneiden von Text in einem TextBlock-Element mithilfe der [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) Eigenschaft.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Text, die Skalierung von 100 bis 225 % mit Ausschneiden von text](images/coretext/text-scaling-clipping-small.png)  
*Skalieren mit Text Clipping Text*

### <a name="use-a-tooltip"></a>Verwenden Sie eine QuickInfo

Wenn Sie Text beschneiden, verwenden Sie eine QuickInfo, um den vollständigen Text für Ihre Benutzer bereitzustellen.

Hier können wir eine QuickInfo hinzuzufügen, zu einem TextBlock, der den Textumbruch nicht unterstützt:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Nicht die Skalierung schriftartbasierte Symbole oder Symbole

Wenn schriftartbasierte Symbole für Nachdruck oder Decoration verwenden möchten, deaktivieren Sie die Skalierung für diese Zeichen.

Legen Sie die [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) Eigenschaft `false` bei den meisten XAML gesteuert.

### <a name="support-text-scaling-natively"></a>Unterstützung für Text, die Skalierung systemintern

Behandeln der [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings Systemereignis in Ihren benutzerdefinierten Framework und Steuerelemente. Dieses Ereignis wird jedes Mal ausgelöst, die der Benutzer den Text-Skalierungsfaktor auf ihrem System festlegt.

## <a name="summary"></a>Zusammenfassung

Dieses Thema bietet einen Überblick über die Text-Unterstützung in Windows für die Skalierung und UX und Entwickler Anleitungen zum Anpassen der Benutzeroberfläche enthält.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="api-reference"></a>API-Referenz

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
