---
title: Eckradius
description: Erfahren Sie mehr zu den Grundsätzen abgerundeter Ecken, Entwurfsansätzen und Anpassungsoptionen.
ms.date: 10/08/2019
ms.topic: article
keywords: Windows 10, UWP, Eckradius, gerundet
ms.openlocfilehash: 134a49ac57678eea0da718e93a14e3d0cf8896d5
ms.sourcegitcommit: 7112e4ec3f19d46a1fc4d81d1c29fd9c01522610
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "81001477"
---
# <a name="corner-radius"></a>Eckradius

Ab Version 2.2 der [Windows-UI-Bibliothek](/uwp/toolkits/winui/) (WinUI) wurde das Standardformat für viele Steuerelemente dahin gehend aktualisiert, dass abgerundete Ecken verwendet werden. Mit diesen neuen Formatvorlagen sollen Wärme und Vertrauen gefördert und die visuelle Verarbeitung der Benutzeroberfläche erleichtert werden.

Hier sehen Sie zwei Schaltflächen-Steuerelemente abgebildet, das eine ohne abgerundete Ecken und das zweite mit der neuen Formatvorlage mit abgerundeten Ecken.

![Schaltflächen ohne und mit abgerundeten Ecken](images/rounded-corner/my-button.png)

Wenn Sie das NuGet-Paket für WinUI 2.2 oder höher installieren, werden sowohl für WinUI-Steuerelemente als auch für Plattformsteuerelemente neue Standardformatvorlagen installiert. Diese Formatvorlagen werden automatisch verwendet, wenn Sie WinUI 2.2 in Ihrer App verwenden. Sie brauchen keine weiteren Maßnahmen zu ergreifen, um die neuen Formatvorlagen zu verwenden. Weiter unten in diesem Artikel erläutern wir jedoch die Anpassung der abgerundeten Ecken, sollte dies bei Ihnen erforderlich sein.

> [!IMPORTANT]
> Einige Steuerelemente stehen sowohl für die Plattform ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) als auch für WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2)) zur Verfügung; beispielsweise **TreeView** oder **ColorPicker**. Wenn Sie WinUI in Ihrer App verwenden, sollten Sie die WinUI-Version des Steuerelements verwenden. Das Abrunden der Ecken wird bei Verwendung mit WinUI in der Plattformversion möglicherweise inkonsistent übernommen.

> **Wichtige APIs:** [Control.CornerRadius-Eigenschaft](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>Standarddesigns für Steuerelemente

Die Formatvorlagen mit abgerundeten Ecken werden in drei Bereichen der Steuerelemente verwendet: bei rechteckigen Elementen, Flyoutelementen und Balkenelementen.

### <a name="corners-of-rectangle-ui-elements"></a>Ecken von rechteckigen Benutzeroberflächenelementen

- Zu diesen Benutzeroberflächenelementen gehören einfache Steuerelemente, wie Schaltflächen, die Benutzer zu jeder Zeit auf dem Bildschirm sehen.
- Der Standardwert des Radius, den wir für diese Benutzeroberflächenelemente verwenden, ist **2 px**.

![Hervorgehobene Schaltfläche mit abgerundeten Ecken](images/rounded-corner/button.png)

**Steuerelemente**

- AutoSuggestBox
- Schaltfläche
  - ContentDialog-Schaltflächen
- CalendarDatePicker
- CheckBox
  - TreeView-Kontrollkästchen für Mehrfachauswahl
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>Ecken der Flyout- und Overlay-Elemente der Benutzeroberfläche

- Hierbei kann es sich um vorübergehende Benutzeroberflächenelemente handeln, die nur zeitweilig auf dem Bildschirm angezeigt werden, oder um Elemente, die andere UI-Elemente überlagern, wie TabView-Registerkarten.
- Der Standardwert des Radius, den wir für diese Benutzeroberflächenelemente verwenden, ist **4 px**.

![Flyoutbeispiel](images/rounded-corner/flyout.png)

**Steuerelemente**

- CommandBarFlyout
- ContentDialog
- Flyout
- MenuFlyout
- TabView-Registerkarten
- TeachingTip
- ToolTip
- Flyoutteil (in offenem Zustand)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>Balkenelemente

- Diese Elemente der Benutzeroberfläche sind wie Balken oder Linien geformt. Beispiel: ProgressBar.
- Der Standardwert des Radius, den wir hier verwenden, ist **2 px**.

![Statusleistenbeispiel](images/rounded-corner/bars.png)

**Steuerelemente**

- NavigationView-Auswahlindikator
- Pivot-Auswahlindikator
- ProgressBar
- ScrollBar (wenn `IndicatorMode=TouchIndicator`)
- Slider
  - ColorPicker-Farbschieberegler
  - Schieberegler der MediaTransportControls-Suchleiste

## <a name="customization-options"></a>Anpassungsoptionen

Die von uns angegebenen Standardwerte für den Eckradius sind nicht in Stein gemeißelt, und es gibt eine Reihe von Wegen, auf denen Sie den Betrag an Rundung an den Ecken komfortabel ändern können. Dies kann mithilfe zweier globaler Ressourcen oder über die [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius)-Eigenschaft direkt im Steuerelement erfolgen, abhängig vom gewünschten Detailgrad der Anpassung.

### <a name="when-not-to-round"></a>Wann Rundung vermieden werden soll

Es gibt Fälle, in denen die Ecken eines Steuerelements nicht gerundet werden sollten, und in diesen Fällen runden wir auch nicht.

- Wenn sich mehrere Benutzeroberflächenelemente berühren, die innerhalb eines Containers bereitgestellt werden, wie etwa die zwei Teile eines SplitButton-Elements. An der Berührungsfläche darf kein Leerraum vorhanden sein.

![SplitButton](images/rounded-corner/split-button-2.png)

- Wenn sich ein Steuerelement innerhalb eines anderen Containers befindet, wie die Leiste und die Schaltflächen eines ScrollBar-Elements, die Teil des ScrollBar-Containers sind, der seinerseits Bestandteil von ScrollViewer ist.

![ScrollBar](images/rounded-corner/scrollbar.png)

- Wenn ein Flyout-Benutzeroberflächenelement mit einer Benutzeroberfläche verbunden ist, die das Flyout auf einer Seite aufruft.

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>Rechteck und Schatten für den Tastaturfokus

In unserem Standarddesign sind keine besonderen Maßnahmen zum Abrunden der Ecken des Rechtecks für den Tastaturfokus oder seines Schattens vorgesehen. Die Verwendung eines größeren Werts für den Eckradius bewirkt keine Störung ihrer Funktion; es ist jedoch sinnvoll, sich dieses Details bewusst zu sein, um visuelle Fehler zu vermeiden, die mit einem größeren Wert einhergehen könnten.

Hier sehen Sie ein Beispiel dafür, wie ein größerer Eckradius ein unerwünschtes Aussehen des Schattens bewirken kann:

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>Abgerundete Ecken und Leistung

Das Rendern von abgerundeten Ecken nimmt naturgemäß mehr Grafikleistung als das Rendern von rechtwinkligen Ecken in Anspruch. Bei der Auswahl der Standardwerte für den Eckradius haben wir nicht nur unsere Designrichtlinien in Betracht gezogen, sondern uns auch bemüht, sicherzustellen, dass unsere Standardsteuerelemente in Ihren Apps eine gute Leistung zeigen.

Wenn Sie sich in diesem Kontext mit der Leistung von Apps befassen, sollten Sie in erster Linie die Ladezeit von Seiten und die Startzeit von Apps im Blick haben. Bedenken Sie, dass abgerundete Ecken bei größerer Fläche der Benutzeroberfläche eine größere Auswirkung auf die Leistung haben. Vermeiden Sie die Darstellung abgerundeter Ecken auf der Benutzeroberfläche einer Vollbild-App. Dieses Problem ist weniger bedeutsam, wenn die Benutzeroberfläche kurz und nach dem Laden der Seite angezeigt wird, wie bei ContentDialog.

### <a name="page-or-app-wide-cornerradius-changes"></a>Änderung von CornerRadius für eine gesamte Seite oder gesamte App

Es gibt zwei Ressourcen, die die Eckradien aller Steuerelemente steuern:

- `ControlCornerRadius`: der Standardwert ist 2 px.
- `OverlayCornerRadius`: der Standardwert ist 4 px.

Wenn Sie den Wert dieser Ressourcen in einem beliebigen Gültigkeitsbereich überschreiben, wirkt sich dies auf alle Steuerelemente in diesem Bereich aus.

Das bedeutet, wenn Sie die Rundung aller Steuerelemente ändern möchten, auf die Rundung angewendet werden kann, können Sie beide Ressourcen auf App-Ebene mit neuen Werten für CornerRadius definieren, wie hier dargestellt:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">0</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">0</CornerRadius>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Wenn Sie die Rundung aller Steuerelemente in einem bestimmten Bereich ändern möchten, etwa auf Seiten- oder Containerebene, können Sie einem ähnlichen Muster folgen:

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> Die `OverlayCornerRadius`-Ressource muss auf App-Ebene definiert sein, um wirksam zu sein.
>
>Dies hat den Grund, dass Popups und Flyouts dynamisch sind und im Stammelement der visuellen Struktur erstellt werden, sodass alle Ressourcen, die sie verwenden, ebenfalls hier definiert sein müssen. Andernfalls liegen sie außerhalb des Bereichs.

### <a name="per-control-cornerradius-changes"></a>Änderungen von CornerRadius auf Steuerelementebene

Sie können die [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius)-Eigenschaften von Steuerelementen direkt ändern, wenn Sie die Rundung nur für eine Anzahl ausgewählter Steuerelemente ändern möchten.

|Standardwert | Geänderte Eigenschaft |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

Nicht alle Steuerelemente reagieren mit geänderter Darstellung der Ecken auf die Änderung der `CornerRadius`-Eigenschaft. Um sicherzustellen, dass das Steuerelement, dessen Ecken Sie abrunden möchten, in der erwarteten Weise auf die geänderte `CornerRadius`-Eigenschaft reagiert, überprüfen Sie zuerst, ob sich die globalen Ressourcen `ControlCornerRadius` oder `OverlayCornerRadius` auf das fragliche Steuerelement auswirken. Ist dies nicht der Fall, überprüfen Sie, ob das Element, das Sie runden möchten, überhaupt Ecken aufweist. Viele unserer Steuerelemente rendern tatsächlich keine Kanten und können die `CornerRadius`-Eigenschaften daher nicht ordnungsgemäß nutzen.

### <a name="basing-custom-styles-on-winui"></a>Benutzerdefinierte Formatvorlagen basierend auf WinUI

Du kannst deine benutzerdefinierten Formatvorlagen basierend auf den WinUI-Formatvorlagen für abgerundete Ecken erstellen, indem du das richtige `BasedOn`-Attribut in deiner Formatvorlage angibst. Um beispielsweise eine benutzerdefinierte Formatvorlage basierend auf dem WinUI-Schaltflächenstil zu erstellen, gehst du folgendermaßen vor:

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

Im Allgemeinen folgen die WinUI-Steuerelementstile einer konsistenten Namenskonvention: „DefaultXYZStyle“, wobei „XYZ“ für den Namen des Steuerelements steht. Für eine vollständige Referenz kannst du die XAML-Dateien im WinUI-Repository durchsuchen.
