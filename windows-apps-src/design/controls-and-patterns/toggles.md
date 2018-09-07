---
author: QuinnRadich
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: Richtlinien für Umschaltersteuerelemente
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: daffbc5ff74adc234ac6c2b414a7e1b85763849d
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "3421208"
---
# <a name="toggle-switches"></a>Umschalter

Der Umschalter stellt einen physischen Schalter dar, mit dem Benutzer Dinge ein- oder ausschalten können, wie ein Lichtschalter. Mit Umschalter-Steuerelementen können Sie Benutzern zwei Optionen anbieten, die sich gegenseitig ausschließen (wie Ein/Aus), wobei eine Option mit der Auswahl unmittelbare Ergebnisse liefert.

Um ein Umschalter-Steuerelement zu erstellen, verwenden Sie die  [ToggleSwitch-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch).

> **Wichtige APIs**: [ToggleSwitch-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch), [IsOn-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [Toggled-Ereignis](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie einen Umschalter für binäre Vorgänge, die wirksam werden, sobald der Benutzer den Umschalter umstellt.

![WLAN-Umschalter ein- und ausgeschaltet](images/toggleswitches01.png)

Stellen Sie sich den Umschalter als physischen Netzschalter für ein Gerät vor: Sie schalten ihn ein oder aus, wenn Sie die vom Gerät ausgeführte Aktion aktivieren oder deaktivieren möchten.

Um den Umschalter leicht verständlich zu machen, kennzeichnen Sie ihn mit einem oder zwei Wörtern, vorzugsweise Substantiven, welche die von ihm gesteuerten Funktionen beschreiben, beispielsweise „WLAN“ oder „Küchenlicht“. 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> oder <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>Wählen zwischen Umschalter und Kontrollkästchen

Für einige Aktionen kann entweder ein Umschalter oder ein Kontrollkästchen verwendet werden. Berücksichtigen Sie bei der Entscheidung zwischen den beiden Steuerelementen folgende Überlegungen:

- Verwenden Sie einen Umschalter für binäre Einstellungen, wenn Änderungen direkt wirksam werden.

    ![Umschalter im Vergleich zum Kontrollkästchen](images/toggleswitches02.png)

    In diesem Beispiel ist durch den Umschalter klar, dass das „Küchenlicht“ aktiviert ist. Im Falle eines Kontrollkästchens muss der Benutzer erst überlegen, ob das Licht derzeit aktiviert ist oder ob er das Kontrollkästchen aktivieren muss, um das Licht zu aktivieren.

- Verwenden Sie Kontrollkästchen für optionale („nützliche“) Elemente.
- Verwenden Sie ein Kontrollkästchen, wenn der Benutzer zusätzliche Schritte ausführen muss, damit die Änderungen wirksam werden. Verwenden Sie beispielsweise ein Kontrollkästchen, wenn der Benutzer auf die Schaltfläche „Übermitteln“ oder „Weiter“ klicken muss, damit Änderungen übernommen werden.
- Verwenden Sie Kontrollkästchen, wenn der Benutzer mehrere Elemente auswählen kann, die sich auf eine einzelne Einstellung oder ein einzelnes Feature beziehen.

## <a name="toggle-switches-in-the-the-windows-ui"></a>Umschalter in der Windows-Benutzeroberfläche

Diese Bilder zeigen, wie Umschalter in der Windows-Benutzeroberfläche verwendet werden. Hier sehen Sie, wie Umschalter auf der Seite für intelligente Speichereinstellungen verwendet werden:

![Umschalter in intelligentem Speicher](images/SmartStorageToggle.png)

Dieses Beispiel stammt aus der Seite für Nachtlichteinstellungen:

![Umschalter in den Einstellungen für das Menü „Start” in Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Erstellen von Umschaltern

Hier sehen Sie, wie Sie einen einfachen Umschalter erstellen. Mit diesem XAML-Code wird der oben gezeigte Umschalter erstellt.

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

An dieser Stelle wird beschrieben, wie derselbe Umschalter im Code erstellt wird.

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

Der Schalter kann entweder ein- oder ausgeschaltet sein. Verwenden Sie die [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison)-Eigenschaft, um den Zustand des Schalters zu ermitteln. Wenn der Schalter zur Steuerung des Zustands einer anderen binären Eigenschaft verwendet wird, können Sie wie hier gezeigt eine Bindung verwenden.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

In anderen Fällen können Sie das [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)-Ereignis zur Reaktion auf Zustandsänderungen behandeln.

In diesem Beispiel wird veranschaulicht, wie in XAML und im Code ein Toggled-Ereignis hinzugefügt wird. Das Toggled-Ereignis wird behandelt, um einen Statusring ein- oder auszuschalten oder seine Sichtbarkeit zu ändern.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

An dieser Stelle wird beschrieben, wie derselbe Umschalter im Code erstellt wird.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Hier sehen Sie den Handler für das Toggled-Ereignis.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>Beschriftungen „Ein”/„Aus”

Standardmäßig beinhaltet der Umschalter literalen Beschriftungen „Ein” und „Aus”, die automatisch lokalisiert werden. Sie können diese Beschriftungen durch Festlegen der Eigenschaften [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) und [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent) ersetzen.

In diesem Beispiel werden die Beschriftungen „Ein”/„Aus” durch die Beschriftungen „Einblenden”/„Ausblenden” ersetzt.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

Sie können auch komplexeren Inhalt verwenden, indem Sie die Eigenschaften [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) und [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate) verwenden.

## <a name="recommendations"></a>Empfehlungen

- Verwenden Sie möglichst die Standardeinstellung „Ein“ und „Aus“; ersetzen Sie diese, wenn dies erforderlich ist, damit der Umschalter Sinn ergibt. Wenn Sie sie ersetzen, verwenden Sie ein einzelnes Wort, das die Umschaltfläche genauer beschreibt. In der Regel gilt, dass Sie möglicherweise ein anderes Steuerelement benötigen, wenn die Wörter „Ein“ und „Aus“ die mit einem Umschalter verknüpfte Aktion nicht beschreiben.
- Die Beschriftungen „Ein“ und „Aus“ sollten nur ersetzt werden, falls unbedingt nötig. Behalten Sie die Standardbeschriftungen bei, sofern keine benutzerdefinierten Beschriftungen erforderlich sind.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementkatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [ToggleSwitch-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [Optionsfelder](radio-button.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)
