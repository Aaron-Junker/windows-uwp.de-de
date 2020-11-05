---
description: Der Umschalter stellt einen physischen Schalter dar, mit dem Benutzer Dinge ein- oder ausschalten können.
title: Richtlinien für Umschaltersteuerelemente
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 03714fef129ccb51bd84a73317bfda702317f313
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034713"
---
# <a name="toggle-switches"></a>Umschalter

Der Umschalter stellt einen physischen Schalter dar, mit dem Benutzer Dinge ein- oder ausschalten können, wie ein Lichtschalter. Mit Umschaltersteuerelementen kannst du Benutzern zwei Optionen anbieten, die sich gegenseitig ausschließen (wie Ein/Aus), wobei die Auswahl einer Option unmittelbare Ergebnisse liefert.

Zum Erstellen eines Umschaltersteuerelements verwendest du die [ToggleSwitch-Klasse](/uwp/api/windows.ui.xaml.controls.toggleswitch).

> **Plattform-APIs:** [ToggleSwitch-Klasse](/uwp/api/windows.ui.xaml.controls.toggleswitch), [IsOn-Eigenschaft](/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [Toggled-Ereignis](/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie einen Umschalter für binäre Vorgänge, die wirksam werden, sobald der Benutzer den Umschalter umstellt.

![WLAN-Umschalter ein- und ausgeschaltet](images/toggleswitches01.png)

Stell dir den Umschalter als physischen Netzschalter für ein Gerät vor: Du schaltest ihn ein oder aus, wenn du die vom Gerät ausgeführte Aktion aktivieren oder deaktivieren möchtest.

Damit Benutzer die Funktion des Umschalters leicht verstehen, kennzeichne ihn mit einem oder zwei Wörtern (vorzugsweise Substantiven), die die von ihm gesteuerten Funktionen beschreiben, beispielsweise „WLAN“ oder „Küchenlicht“. 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn du die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicke hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> oder <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>Wählen zwischen Umschalter und Kontrollkästchen

Für einige Aktionen kann entweder ein Umschalter oder ein Kontrollkästchen verwendet werden. Berücksichtigen Sie bei der Entscheidung zwischen den beiden Steuerelementen folgende Überlegungen:

- Verwenden Sie einen Umschalter für binäre Einstellungen, wenn Änderungen direkt wirksam werden.

    ![Umschalter im Vergleich zum Kontrollkästchen](images/toggleswitches02.png)

    In diesem Beispiel ist durch den Umschalter klar, dass das Küchenlicht eingeschaltet ist. Im Falle eines Kontrollkästchens muss der Benutzer erst überlegen, ob das Licht derzeit eingeschaltet ist oder ob er das Kontrollkästchen aktivieren muss, um das Licht zu einzuschalten.

- Verwende Kontrollkästchen für optionale („nützliche“) Elemente.
- Verwenden Sie ein Kontrollkästchen, wenn der Benutzer zusätzliche Schritte ausführen muss, damit die Änderungen wirksam werden. Verwenden Sie beispielsweise ein Kontrollkästchen, wenn der Benutzer auf die Schaltfläche „Übermitteln“ oder „Weiter“ klicken muss, damit Änderungen übernommen werden.
- Verwende Kontrollkästchen, wenn der Benutzer mehrere Elemente auswählen kann, die sich auf eine einzelne Einstellung oder ein einzelnes Feature beziehen.

## <a name="toggle-switches-in-the-windows-ui"></a>Umschalter auf der Windows-Benutzeroberfläche

Auf diesen Bildern ist gezeigt, wie Umschalter auf der Windows-Benutzeroberfläche verwendet werden. Hier siehst du, wie Umschalter auf dem Bildschirm für intelligente Speichereinstellungen verwendet werden:

![Umschalter in intelligentem Speicher](images/SmartStorageToggle.png)

Dieses Beispiel stammt von der Seite mit den Einstellungen für den Nachtmodus:

![Umschalter in den Einstellungen für das Menü „Start” in Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Erstellen von Umschaltern

Hier sehen Sie, wie Sie einen einfachen Umschalter erstellen. Mit diesem XAML-Code wird der zuvor gezeigte Umschalter erstellt.

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

Der Schalter kann entweder ein- oder ausgeschaltet sein. Mit der Eigenschaft [IsOn](/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) kannst du den Zustand des Schalters ermitteln. Wenn der Schalter zur Steuerung des Zustands einer anderen binären Eigenschaft verwendet wird, können Sie wie hier gezeigt eine Bindung verwenden.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

In anderen Fällen kannst du das [Toggled](/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)-Ereignis verarbeiten, um auf Zustandsänderungen zu reagieren.

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

Standardmäßig beinhaltet der Umschalter literalen Beschriftungen „Ein” und „Aus”, die automatisch lokalisiert werden. Du kannst diese Beschriftungen durch Festlegen der Eigenschaften [OnContent](/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) und [OffContent](/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent) ersetzen.

In diesem Beispiel werden die Beschriftungen „Ein”/„Aus” durch die Beschriftungen „Einblenden”/„Ausblenden” ersetzt.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

Du kannst auch komplexeren Inhalt verwenden, indem du die Eigenschaften [OnContentTemplate](/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) und [OffContentTemplate](/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate) festlegst.

## <a name="recommendations"></a>Empfehlungen

- Verwende möglichst die Standardbeschriftungen „Ein“ und „Aus“. Ersetze diese nur, wenn dies erforderlich ist, damit der Umschalter Sinn ergibt. Wenn du sie ersetzt, verwende ein einzelnes Wort, das die Umschaltfläche genauer beschreibt. In der Regel gilt Folgendes: Wenn die Wörter „Ein“ und „Aus“ die mit einem Umschalter verknüpfte Aktion nicht beschreiben, benötigst du möglicherweise ein anderes Steuerelement.
- Die Beschriftungen „Ein“ und „Aus“ sollten nur ersetzt werden, falls unbedingt nötig. Behalten Sie die Standardbeschriftungen bei, sofern keine benutzerdefinierten Beschriftungen erforderlich sind.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [ToggleSwitch-Klasse](/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [Optionsfelder](radio-button.md)
- [Umschaltfläche](toggles.md)
- [Kontrollkästchen](checkbox.md)
