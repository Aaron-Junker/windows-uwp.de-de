---
description: Vorlageneinstellungsklassen
title: Vorlageneinstellungsklassen
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b0d68b31a1574d23dd66de37e2f708d8d77fc052
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371123"
---
# <a name="template-settings-classes"></a>Vorlageneinstellungsklassen


## <a name="prerequisites"></a>Vorraussetzungen

Es wird davon ausgegangen, dass Sie Ihrer Benutzeroberfläche Steuerelemente hinzufügen, deren Eigenschaften festlegen und Ereignishandler anfügen können. Anweisungen zum Hinzufügen von Steuerelementen zu Ihrer App finden Sie unter [Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro). Wir gehen auch davon aus, dass Sie mit der grundlegenden Definition einer benutzerdefinierten Vorlage für Steuerelemente durch Erstellen und Bearbeiten einer Kopie der Standardvorlage vertraut sind. Weitere Informationen dazu finden Sie unter [Schnellstart: Steuerelementvorlagen](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

## <a name="the-scenario-for-templatesettings-classes"></a>Das Szenario für **TemplateSettings**-Klassen

**TemplateSettings**-Klassen bieten eine Reihe von Eigenschaften, die bei der Definition einer neuen Steuerelementvorlage für ein Steuerelement verwendet werden. Die Eigenschaften haben Werte wie Pixelmaße für die Größe bestimmter Teile des UI-Elements. Bei diesen Werten handelt es sich manchmal um berechnete Werte, die aus der Steuerelementlogik stammen; sie zu überschreiben oder darauf zuzugreifen, ist in der Regel nicht einfach. Einige der Eigenschaften dienen als **From**- und **To**-Werte, die Übergänge und Animationen von Teilen steuern; daher treten die relevanten **TemplateSettings**-Eigenschaften paarweise auf.

Es gibt mehrere **TemplateSettings**-Klassen. Alle sind im [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives)-Namespace enthalten. Im Anschluss finden Sie eine Liste der Klassen und einen Link zur **TemplateSettings**-Eigenschaft des entsprechenden Steuerelements. Mit dieser **TemplateSettings**-Eigenschaft greifen Sie auf die **TemplateSettings**-Werte für das Steuerelement zu und können Vorlagenbindungen an seine Eigenschaften festlegen:

-   [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): Wert der [ **ComboBox.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): Wert der [ **GridViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): Wert der [ **ListViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): Wert der [ **ProgressBar.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): Wert der [ **ProgressRing.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): Wert der [ **SettingsFlyout.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): Wert der [ **ToggleSwitch.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): Wert der [ **ToolTip.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

**TemplateSettings**-Eigenschaften sind zur Verwendung in XAML und nicht im Code bestimmt. Es handelt sich dabei um schreibgeschützte Untereigenschaften einer schreibgeschützten **TemplateSettings**-Eigenschaft eines übergeordneten Steuerelements. Für ein Szenario mit erweiterten benutzerdefinierten Steuerelementen, in dem Sie eine neue [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)-basierte Klasse erstellen und daher die Steuerelementlogik beeinflussen können, empfiehlt es sich, eine benutzerdefinierte **TemplateSettings**-Eigenschaft für das Steuerelement zu definieren, um hilfreiche Informationen für das Ändern der Vorlage eines Steuerelements zu kommunizieren. Als schreibgeschützten Wert dieser Eigenschaft definieren Sie eine neue **TemplateSettings**-Klasse im Zusammenhang mit Ihrem Steuerelement, das schreibgeschützte Eigenschaften für alle Informationselemente aufweist, die für Vorlagenmaße, Animationspositionierung usw. relevant sind, und geben Aufrufern die Runtime-Instanz dieser Klasse, die mit Ihrer Steuerelementlogik initialisiert wird. **TemplateSettings**-Klassen werden von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet, sodass die Eigenschaften das Abhängigkeitseigenschaftensystem für Rückrufe für Eigenschaftsänderungen verwenden können. Die Bezeichner von Abhängigkeitseigenschaften für die Eigenschaften sind jedoch nicht als öffentliche API verfügbar, da die **TemplateSettings**-Eigenschaften für Aufrufer schreibgeschützt sein sollen.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Verwendung von **TemplateSettings** in einer Steuerelementvorlage

Hier sehen Sie ein Beispiel aus den ersten XAML-Standardvorlagen für Steuerelemente. Dieses Beispiel stammt aus der Standardvorlage von [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing):

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Der vollständige XAML-Code für die [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)-Vorlage umfasst mehrere hundert Zeilen, hierbei handelt es sich also nur um einen sehr kleinen Auszug. Dieser XAML-Code definiert einen Teil des Steuerelements, das als eines von sechs [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Elementen die sich drehende Animation für einen unbestimmten Fortschritt darstellt. Ihnen als Entwickler gefallen die Kreise möglicherweise nicht, und Sie möchten einen anderen Grafikgrundtyp oder eine andere grundlegende Form für den Animationsverlauf verwenden. Sie können stattdessen z. B. ein **ProgressRing**-Element erstellen, das eine Reihe von in einem Quadrat angeordneten [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Elementen enthält. Dabei kann jede einzelne **Rectangle**-Komponente Ihrer neuen Vorlage wie folgt aussehen:

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Die **TemplateSettings**-Eigenschaften eignen sich hier gut, da es sich dabei um berechnete Werte aus der grundlegenden Steuerlogik von [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) handelt. Die Berechnung unterteilt das gesamte [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)- und [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)-Element des **ProgressRing**-Elements und weist eine berechnete Messung für jedes Bewegungselemente in den Vorlagen zu, sodass die Vorlagenteile die Größe an den Inhalt anpassen können.

Hier sehen Sie ein weiteres Beispiel für die Verwendung von standardmäßigen XAML-Steuerelementvorlagen, diesmal mit einem der Eigenschaftensätze, die die **From**- und **To**-Elemente einer Animation darstellen. Dieses Beispiel stammt aus der [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)-Standardvorlage:

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

Da auch diese Vorlage viel XAML-Code enthält, zeigen wir wieder nur einen Auszug. Dies ist nur ein Beispiel für die Zustände und Designanimationen, die dieselben [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings)-Eigenschaften verwenden. Für [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) erzwingt die Verwendung der **ComboBoxTemplateSettings**-Werte durch Bindungen, dass verwandte Animationen in der Vorlage beendet werden und an Positionen beginnen, die auf gemeinsamen Werten basieren. Dadurch ist der Übergang nahtlos.

**Beachten Sie**    bei Verwendung **TemplateSettings** Werte als Teil Ihrer Steuerelementvorlage, stellen Sie sicher, Eigenschaften, die den Typ des Werts übereinstimmen einstellen. Andernfalls müssen Sie u. U. einen Wertkonverter für die Bindung erstellen, damit der Zieltyp der Bindung von einem anderen Quelltyp des **TemplateSettings**-Werts konvertiert werden kann. Weitere Informationen finden Sie unter [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

## <a name="related-topics"></a>Verwandte Themen

* [Schnellstart: Steuerelementvorlagen](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))

