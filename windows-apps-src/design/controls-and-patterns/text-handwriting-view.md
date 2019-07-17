---
Description: Passen Sie die integrierte Handschriftansicht für Freihandeingaben an Texteingaben an, die von UWP-Textsteuerelementen wie TextBox, RichEditBox (sowie Steuerelementen wie AutoSuggestBox, die eine ähnliche Texteingabeerfahrung bieten) unterstützt werden.
title: Texteingabe mit der Handschriftansicht
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 262e033c8393544e3b5b8394d0e9a5be410cd080
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319478"
---
# <a name="text-input-with-the-handwriting-view"></a>Texteingabe mit der Handschriftansicht

![Textfeld wird erweitert, wenn mit einem Stift darauf getippt wird](images/handwritingview/handwritingview2.gif)

Passen Sie die integrierte Handschriftansicht für Freihandeingaben an Texteingaben an, die von UWP-Textsteuerelementen wie [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) und von diesen abgeleiteten Steuerelementen (wie [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)) unterstützt werden.

## <a name="overview"></a>Übersicht

XAML-Texteingabefelder verfügen über eine integrierte Unterstützung für Stifteingaben mit [Windows Ink](../input/pen-and-stylus-interactions.md). Wenn ein Benutzer einen Windows-Stift für Eingaben in ein Textfeld verwendet, wird das Textfeld in eine Handschriftoberfläche transformiert, und es wird kein separater Schreibbereich geöffnet.

Text wird erkannt, während der Benutzer an einer beliebigen Stelle in das Textfeld schreibt, und ein Kandidatenfenster zeigt die Erkennungsergebnisse an. Der Benutzer kann auf ein Ergebnis tippen, um es auszuwählen, oder er kann den Schreibvorgang fortsetzen, um das vorgeschlagene Wort zu akzeptieren. Die literalen Erkennungsergebnisse (buchstabenweise) sind im Kandidatenfenster enthalten, somit ist die Erkennung nicht auf Wörter in einem Wörterbuch beschränkt. Während der Benutzer schreibt, wird die akzeptierte Texteingabe in ein Schriftzeichen konvertiert, das das Verhalten der natürlichen Schreibweise beibehält.

> [!NOTE]
> Die Handschriftansicht ist standardmäßig aktiviert; Sie können sie jedoch für einzelne Steuerelemente deaktivieren und stattdessen zum Texteingabebereich zurückkehren.

![Textfeld mit Freihandeingabe und Vorschlägen](images/handwritingview/handwritingview-inksuggestion1.gif)

Ein Benutzer kann seinen Text mithilfe der folgenden Standardgesten und -aktionen bearbeiten:

- _Durchstreichen_ oder _Ausstreichen_: Ermöglicht das Durchziehen eines Strichs, um ein Wort oder einen Teil eines Wortes zu löschen.
- _Verknüpfen_: Zeichnet einen Bogen zwischen Wörtern, um den Abstand zwischen ihnen zu löschen.
- _Einfügen_: Ermöglicht das Zeichnen eines Caret-Symbols, um ein Leerzeichen einzufügen.
- _Überschreiben_: Überschreibt vorhandenen Text, um ihn zu ersetzen.

![Textfeld mit Korrektur von Freihandeingabe](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Deaktivieren der Handschriftansicht

Die integrierte Handschriftansicht ist standardmäßig aktiviert.

Möglicherweise möchten Sie die Handschriftansicht deaktivieren, wenn Sie bereits eine vergleichbare Freihand-zu-Text-Funktion in der Anwendung bereitgestellt haben oder wenn sich die Texteingabeerfahrung auf bestimmte Formatierungen oder Sonderzeichen (z. B. Tabulatorzeichen) stützt, die bei Freihandeingabe nicht verfügbar sind.

In diesem Beispiel deaktivieren wir die Handschriftansicht, indem die [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled)-Eigenschaft des [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)-Steuerelements auf „false“ festgelegt wird. Alle Textsteuerelemente, die die Handschriftansicht unterstützen, unterstützen eine ähnliche Eigenschaft.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Angeben der Ausrichtung der Handschriftansicht

Die Handschriftansicht befindet sich über dem zugrunde liegenden Textsteuerelement und ist gemäß den Freihandeingabeeinstellungen des Benutzers skaliert (siehe **Einstellungen > Geräte > Stift & Windows Ink > Handschrift > Schriftgrad bei direkter Eingabe in Textfeld**). Die Ansicht wird automatisch in Bezug auf das Textsteuerelement und dessen Position innerhalb der App ausgerichtet.

Die UI der Anwendung wird nicht automatisch umbrochen, um das größere Steuerelement aufzunehmen. Daher bewirkt das System möglicherweise, dass die Ansicht wichtige UI-Elemente verdeckt.

Hier wird erläutert, wie mit der [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment)-Eigenschaft einer [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) angegeben wird, mit welchem Anker das zugrunde liegende Textsteuerelement die Handschriftansicht ausrichtet.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>Deaktivieren von Kandidaten der automatischen Vervollständigung

Das Popupfenster mit Wortvorschlägen ist standardmäßig aktiviert und liefert eine Liste von Topkandidaten der Freihanderkennung, aus welcher der Benutzer auswählen kann, wenn der oberste Kandidat falsch sein sollte.

Wenn Ihre Anwendung bereits stabile, benutzerdefinierte Erkennungsfunktionen bietet, können Sie die systemeigenen Vorschläge mit der [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled)-Eigenschaft deaktivieren; dies wird im folgenden Beispiel veranschaulicht.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>Verwenden von Schriftartvoreinstellungen für Handschrift

Benutzer können aus einer vordefinierten Sammlung von handschriftlichen Schriftarten auswählen, die beim Rendern von Text aufgrund von Freihandeingabeerkennung verwendet werden sollen (siehe **Einstellungen > Geräte > Stift & Windows Ink > Handschrift > Schriftgrad bei direkter Eingabe in Textfeld**).

> [!NOTE]
> Benutzer können sogar eine Schriftart erstellen, die auf ihrer eigenen Handschrift basiert.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Ihre App kann auf diese Einstellung zugreifen und die ausgewählte Schriftart für den erkannten Text im Textsteuerelement verwenden.

In diesem Beispiel wird auf das [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged)-Ereignis einer [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) überwacht, und die vom Benutzer ausgewählte Schriftart wird angewendet, wenn die Textänderung aus der HandwritingView stammt (andernfalls wird eine Standardschriftart angewendet).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Zugreifen auf die HandwritingView in zusammengesetzten Steuerelementen

Zusammengesetzte Steuerelemente, die die Steuerelemente [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) oder [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) verwenden, z. B. [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox), unterstützen ebenfalls eine [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Verwenden Sie zum Aufrufen der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) in einem zusammengesetzten Steuerelement die [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper)-API.

Der folgende XAML-Codeausschnitt zeigt ein [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) Steuerelement.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

Im entsprechenden CodeBehind veranschaulichen wir, wie die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) im [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)-Steuerelement deaktiviert wird.

1. Zunächst wird das Loaded-Ereignis der Anwendung verarbeitet; dort wird eine FindInnerTextBox-Funktion aufgerufen, um das Durchlaufen der visuellen Struktur zu starten.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Anschließend wird das Durchlaufen der visuellen Struktur (beginnend bei einer AutoSuggestBox) in der FindInnerTextBox-Funktion mit einem Aufruf von FindVisualChildByName gestartet.

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. Diese Funktion durchläuft schließlich die visuelle Struktur so weit, bis die TextBox abgerufen wird.

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>Ändern der Position der HandwritingView

Gelegentlich müssen Sie sicherstellen, dass die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) UI-Elemente abdeckt, die andernfalls nicht abgedeckt würden.

Hier erstellen wir eine TextBox, welche eine Diktatfunktion unterstützt (dies wird implementiert, indem eine TextBox und eine Diktat-Schaltfläche in einem StackPanel platziert werden).

![TextBox mit Diktatfunktion](images/handwritingview/textbox-with-dictation.png)

Da der StackPanel nun größer als die TextBox ist, wird das zusammengesetzte Steuerelement von der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) möglicherweise nicht komplett verdeckt.

![TextBox mit Diktatfunktion](images/handwritingview/textbox-with-dictation-handwritingview.png)

Um dieses Problem zu beheben, legen Sie die PlacementTarget-Eigenschaft der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) auf das UI-Element fest, an dem sie ausgerichtet werden soll.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>Ändern der Größe der HandwritingView

Sie können auch die Größe der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) festlegen. Dies empfiehlt sich beispielsweise, wenn Sie sicherstellen müssen, dass die Ansicht keine wichtigen UI-Elemente verdeckt.

Wie im vorherigen Beispiel erstellen wir eine TextBox, die die Diktatfunktion unterstützt (dies wird implementiert, indem eine TextBox und eine Diktat-Schaltfläche in einem StackPanel platziert werden).

![TextBox mit Diktatfunktion](images/handwritingview/textbox-with-dictation.png)

In diesem Fall möchten wir sicherstellen, dass die Diktat-Schaltfläche immer sichtbar ist.

![TextBox mit Diktatfunktion](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Dazu binden wir die MaxWidth-Eigenschaft der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) an die Breite des UI-Elements, das sie verdecken soll.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>Ändern der Position von benutzerdefinierter UI

Wenn Sie über benutzerdefinierte UI-Elemente verfügen, die als Reaktion auf Texteingaben eingeblendet werden, z. B. Info-Popupfenster, müssen Sie die betreffenden UI-Elemente so neu positionieren, dass sie die Handschriftansicht nicht verdecken.

![TextBox mit benutzerdefinierter UI](images/handwritingview/textbox-with-customui.png)

Im folgenden Beispiel wird veranschaulicht, wie auf die Ereignisse [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed) und [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) überwacht wird, um die Position eines [Popup](https://docs.microsoft.com/uwp/api/windows.ui.popups) festzulegen.

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>Umschreiben der Vorlage für das HandwritingView-Steuerelement

Wie bei allen XAML-Framework-Steuerelementen können Sie sowohl die visuelle Struktur als auch das visuelle Verhalten einer [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) an Ihre spezifischen Anforderungen anpassen.

Ein komplettes Beispiel zum Erstellen einer benutzerdefinierten Vorlage finden Sie in der Anleitung [Erstellen benutzerdefinierter Transportsteuerelemente](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) bzw. im [Beispiel für ein benutzerdefiniertes Bearbeitungssteuerelement](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








