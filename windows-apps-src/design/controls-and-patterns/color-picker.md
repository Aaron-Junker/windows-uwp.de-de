---
Description: Mithilfe eines Farbwählers kann der Benutzer Farben suchen und auswählen.
title: Farbwähler
label: Color Picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f287f738c39e21ea76ff2595cc34ac715a1b52ca
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081199"
---
# <a name="color-picker"></a>Farbauswahl

Mithilfe eines Farbwählers können Benutzer Farben suchen und auswählen. Standardmäßig können sie durch die Farben eines Farbspektrums navigieren oder eine bestimmte Farbe in einem Textfeld des Typs „Rot-Grün-Blau (RGB)“, „Wert für Farbton/Sättigung“ oder „Hexadezimal“ angeben.

![Ein Standard-Farbwähler](images/color-picker-default.png)

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement **ColorPicker** ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Windows-UI-Bibliotheks-APIs:** [ColorPicker-Klasse](/uwp/api/microsoft.ui.xaml.controls.colorpicker), [Color-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.colorpicker.Color), [ColorChanged-Ereignis](/uwp/api/microsoft.ui.xaml.controls.colorpicker.ColorChanged)
>
> **Plattform-APIs:** [ColorPicker-Klasse](/uwp/api/windows.ui.xaml.controls.colorpicker), [Color-Eigenschaft](/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [ColorChanged-Ereignis](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Mithilfe eines Farbwählers können Benutzer in Ihrer App Farben auswählen. Beispielsweise können sie so Farbeinstellungen wie Schriftfarben, Hintergrundfarben oder App-Farbdesigns ändern.

Falls Ihre App zum Zeichnen oder für andere stiftbasierte Aufgaben gedacht ist, sollten Sie neben dem Farbwähler auch die Implementierung von [Steuerelementen für Freihandeingaben](inking-controls.md) in Betracht ziehen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/ColorPicker">die App zu öffnen und ColorPicker in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>Erstellen eines Farbwählers

In diesem Beispiel zeigen wir Ihnen, wie Sie einen Standardfarbwähler in XAML erstellen können.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

Der Farbwähler zeigt standardmäßig eine Vorschau der ausgewählten Farbe in dem rechteckigen Balken neben dem Farbspektrum an. Um auf die ausgewählte Farbe zuzugreifen und sie in Ihrer App zu verwenden, können Sie entweder das Ereignis [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) oder die Eigenschaft [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) verwenden. Detaillierten Code finden Sie in den nachfolgenden Beispielen.

### <a name="bind-to-the-chosen-color"></a>Binden an die ausgewählte Farbe

Soll die Farbauswahl sofort wirksam werden, können Sie sie entweder per Datenbindung an die Color-Eigenschaft binden oder über das ColorChanged-Ereignis in Ihrem Code auf die ausgewählte Farbe zugreifen.

In diesem Beispiel binden Sie die Color-Eigenschaft einer als Füllung für ein Rechteck verwendeten SolidColorBrush-Klasse direkt an die im Farbwähler ausgewählte Farbe. Jede Änderung am Farbwähler zieht eine Liveänderung an der gebundenen Eigenschaft nach sich.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

In diesem Beispiel wird ein vereinfachter Farbwähler verwendet, der nur aus einem kreisförmigen Spektrum und einem Schieberegler besteht. Dies ist eine gängige unkomplizierte Ausführung eines Farbwählers. Lässt sich die Farbänderung in Echtzeit an dem jeweils betroffenen Objekt nachvollziehen, muss keine Farbvorschauleiste angezeigt werden. Weitere Informationen finden Sie im Abschnitt *Anpassen des Farbwählers*.

### <a name="save-the-chosen-color"></a>Speichern der ausgewählten Farbe

In einigen Fällen soll die Farbänderung nicht direkt angewendet werden. Wenn Sie den Farbwähler beispielsweise in einem Flyout hosten, sollten Sie die ausgewählte Farbe erst anwenden, nachdem der Benutzer die Auswahl bestätigt oder das Flyout geschlossen hat. Der ausgewählte Farbwert lässt sich auch zur späteren Verwendung speichern.

In diesem Beispiel wird ein Farbwähler in einem Flyout mit „Bestätigen“-Schaltfläche und „Abbrechen“-Schaltfläche gehostet. Sobald der Benutzer seine Farbauswahl bestätigt, wird die ausgewählte Farbe gespeichert und kann später in der App verwendet werden.

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>Konfigurieren des Farbwählers

Zur Farbauswahl sind nicht alle Felder notwendig, daher ist der Farbwähler flexibel. Sie haben eine Vielzahl von Optionen zur Verfügung, über die Sie dieses Steuerelement an Ihre Anforderungen anpassen können.

Benötigt der Benutzer keine präzise Kontrolle, beispielsweise bei der Auswahl der Textmarkerfarbe in einer Notizen-App, können Sie eine vereinfachte Benutzeroberfläche verwenden. Sie können die Texteingabefelder ausblenden und das Farbspektrum als Kreis darstellen.

Benötigt der Benutzer präzise Kontrolle, beispielsweise in einer Grafikdesign-App, können Sie sowohl Schieberegler als auch Texteingabefelder für jeden Aspekt der Farbe anzeigen.

#### <a name="show-the-circle-spectrum"></a>Anzeigen eines kreisförmigen Spektrums

In diesem Beispiel wird demonstriert, wie Sie den Farbwähler mithilfe der Eigenschaft [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) so konfigurieren können, dass statt des standardmäßigen quadratischen Farbspektrums ein kreisförmiges Farbspektrum angezeigt wird.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Farbwähler mit kreisförmigem Spektrum](images/color-picker-ring.png)

Bei der Entscheidung zwischen einem quadratischen und einem kreisförmigen Farbspektrum ist das Hauptkriterium die Genauigkeit. In einem quadratischen Farbspektrum hat der Benutzer mehr Kontrolle über die Farbauswahl, da ein größerer Teil des Farbraums angezeigt wird. Ein kreisförmiges Spektrum ist eher für eine schnelle, unkomplizierte Farbauswahl gedacht.

#### <a name="show-the-alpha-channel"></a>Anzeigen des Alphakanals

In diesem Beispiel implementieren Sie im Farbwähler einen Schieberegler und ein Textfeld für die Deckkraft.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![Farbwähler mit „IsAlphaEnabled“ auf „true“](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>Anzeigen eines einfachen Wählers

In diesem Beispiel wird demonstriert, wie Sie den Farbwähler mit einer einfachen Benutzeroberfläche für die schnelle und unkomplizierte Verwendung konfigurieren können. Sie implementieren das kreisförmige Spektrum und blenden die standardmäßigen Texteingabefelder aus. Lässt sich die Farbänderung in Echtzeit an dem jeweils betroffenen Objekt nachvollziehen, muss keine Farbvorschauleiste angezeigt werden. Andernfalls sollte die Farbvorschau sichtbar sein.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![Ein einfacher Farbwähler](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>Anzeigen oder Ausblenden von zusätzlichen Funktionen

In der Tabelle unten sind alle Optionen aufgeführt, die Sie zur Konfiguration des Steuerelements „ColorPicker“ verwenden können.

Feature | Eigenschaften
--------|-----------
Farbspektrum | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
Farbvorschau | IsColorPreviewVisible
Farbwerte| IsColorSliderVisible, IsColorChannelTextInputVisible
Deckkraftwerte | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
Hexadezimalwerte | IsHexInputVisible

> [!NOTE]
> Das Textfeld und der Schieberegler für die Deckkraft werden nur angezeigt, wenn „AlphaEnabled“ auf **true** festgelegt ist. Dann können Sie die Sichtbarkeit der Eingabesteuerelemente mithilfe der Eigenschaften „IsAlphaTextInputVisible“ und „IsAlphaSliderVisible“ anpassen. Details hierzu finden Sie in der API-Dokumentation.

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Überlegen Sie, welche Art von Farbwähler für Ihre App am besten geeignet ist. Einige Szenarien erfordern keine präzise Farbauswahl; dann ist ein vereinfachter Wähler die bessere Entscheidung.
- Für maximale Genauigkeit bei der Farbauswahl sollten Sie das quadratische Spektrum verwenden, mit einer Mindestgröße von 256 × 256 Pixel. Alternativ können Sie die Texteingabefelder implementieren, damit der Benutzer seine Wunschfarbe präzise anpassen kann.
- Wenn Sie den Farbwähler in einem Flyout hosten, sollte die Farbauswahl nicht bereits durch ein simples Tippen in das Spektrum oder eine einfache Justierung des Schiebereglers übernommen werden. Regeln Sie die Übernahme der Auswahl wie folgt:
  - Implementieren Sie Schaltflächen zum Übernehmen und Abbrechen, über die der Benutzer seine Auswahl anwenden oder verwerfen kann. Durch Klicken oder Tippen auf die Zurück-Schaltfläche bzw. auf eine Stelle außerhalb des Flyouts wird das Flyout geschlossen, ohne dass die Auswahl des Benutzers gespeichert wird.
  - Alternativ kann die Auswahl übernommen werden, sobald der Benutzer das Flyout per Tippen oder Klicken auf eine Stelle außerhalb des Flyouts oder auf die Zurück-Schaltfläche schließt.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Zeichen- und Eingabestiftinteraktionen in UWP-Apps](../input/pen-and-stylus-interactions.md)
- [Freihandeingaben](inking-controls.md)

<!--
<div class="microsoft-internal-note">
<p>
<p>
Note: For more info, see the [color picker redlines](https://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->