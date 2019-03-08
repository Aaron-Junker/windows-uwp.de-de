---
Description: Passen Sie die integrierte Handschrifterkennungs-Ansicht für Freihandeingaben auf die Texteingabe, die von UWP Textsteuerelemente wie z. B. im Textfeld für die RichEditBox (und Steuerelemente wie die AutoSuggestBox, die ein ähnliches Verhalten wie Text input) unterstützt wird.
title: Texteingabe, mit der Handschrift-Ansicht
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634915"
---
# <a name="text-input-with-the-handwriting-view"></a>Texteingabe, mit der Handschrift-Ansicht

![Textfeld wird erweitert, wenn mit einem Stift darauf getippt wird](images/handwritingview/handwritingview2.gif)

Passen Sie die integrierte Handschrifterkennungs-Ansicht für Freihandeingaben auf die Texteingabe von UWP-Text-Steuerelemente wie z. B. unterstützt die [Textfeld](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), und abgeleitete Steuerelemente wie z. B. aus diesen die [ AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Übersicht

XAML-Text-Eingabefelder feature eingebetteten Unterstützung für Stifteingabe mit [Windows Ink](../input/pen-and-stylus-interactions.md). Wenn ein Benutzer in ein Texteingabefeld, mit einem Windows-Stift tippt, transformiert im Textfeld, in eine Oberfläche für die handschrifterkennung, anstatt eine separate Eingabebereich zu öffnen.

Text wird erkannt, wenn der Benutzer an einer beliebigen Stelle in das Textfeld ein, und ein Kandidat schreibt, dass im Fenster die Erkennungsergebnisse angezeigt wird. Der Benutzer kann auf ein Ergebnis tippen, um es auszuwählen, oder er kann den Schreibvorgang fortsetzen, um das vorgeschlagene Wort zu akzeptieren. Die literalen Erkennungsergebnisse (buchstabenweise) sind im Kandidatenfenster enthalten, somit ist die Erkennung nicht auf Wörter in einem Wörterbuch beschränkt. Während der Benutzer schreibt, wird die akzeptierte Texteingabe in ein Schriftzeichen konvertiert, das das Verhalten der natürlichen Schreibweise beibehält.

> [!NOTE]
> Die Handschrift-Ansicht ist standardmäßig aktiviert, aber Sie können auf einer Basis pro Steuerelement deaktivieren und stattdessen eine Wiederherstellung auf der Eingabebereich Text.

![Textfeld mit Freihand- und Vorschläge](images/handwritingview/handwritingview-inksuggestion1.gif)

Ein Benutzer kann seinen Text mithilfe der folgenden Standardgesten und -aktionen bearbeiten:

- _Durchstreichen_ oder _Ausstreichen_ – Ermöglicht das Durchziehen eines Strichs, um ein Wort oder einen Teil eines Wortes zu löschen.
- _Verknüpfen_ – Zeichnet einen Bogen zwischen Wörtern, um den Abstand zwischen ihnen zu löschen.
- _Einfügen_ – Ermöglicht das Zeichnen eines Caret-Symbols, um ein Leerzeichen einzufügen.
- _Überschreiben_ – Überschreibt vorhandenen Text, um ihn zu ersetzen.

![Textfeld mit der Korrektur von Freihandeingaben](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Deaktivieren Sie die Handschrift-Ansicht

Die integrierte Handschrifterkennungs-Ansicht ist standardmäßig aktiviert.

Sie möchten die Handschrift-Ansicht zu deaktivieren, wenn Sie bereits entsprechende Freihand-zu-Text-Funktionalität in Ihrer Anwendung bereitstellen oder Ihren Text input Erfahrungen stützt sich auf eine Art von Formatierung oder Sonderzeichen Zeichen (z. B. eine Registerkarte ") nicht verfügbar ist, über das Handschrifterkennungs.

In diesem Beispiel wir die handschrifterkennung Ansicht deaktivieren, indem die [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) Eigenschaft der [Textfeld](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) Steuerelement auf "false". Alle Textsteuerelemente, die Unterstützung der handschrifterkennung Ansicht unterstützen eine ähnliche Eigenschaft.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Geben Sie die Ausrichtung der Handschrift-Ansicht

Die handschrifterkennung Ansicht befindet sich über das zugrunde liegende Textsteuerelement und angepasst, um die Einstellungen des Benutzers Handschrift aufzunehmen (finden Sie unter **Einstellungen -> Geräte Pen -> & Windows Ink-Handschrift > -> der Schriftgrad, wenn direkt in das Schreiben von Textfeld**). Die Ansicht wird relativ zum Text-Steuerelement und seine Position in der app auch automatisch ausgerichtet werden.

Die Benutzeroberfläche die Anwendung nicht erfolgt ein Umbruch in größeren Steuerelements aufzunehmen, damit das System die Ansicht, um wichtige UI verdeckt verursachen kann.

Hier, wir zeigen, wie die [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) Eigenschaft eine [Textfeld](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) angeben, welche Anker auf das zugrunde liegende Textsteuerelement verwendet wird, der Anpassung an die Handschrifterkennungs-Ansicht.

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

## <a name="disable-auto-completion-candidates"></a>Deaktivieren Sie Kandidaten für automatische Vervollständigung

Das Text-Popup Vorschlag ist standardmäßig aktiviert, um eine Liste von oberen Freihandeingaben Anerkennung Kandidaten bereitzustellen in denen der Benutzer auswählen kann, für den Fall, dass die fünf hauptkandidaten bei den falsch ist.

Wenn die Anwendung bereits die Erkennung von robusten, benutzerdefinierte Funktionen bereitstellt, können Sie die [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) Eigenschaft, um die integrierte Vorschläge zu deaktivieren, wie im folgenden Beispiel gezeigt.

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

## <a name="use-handwriting-font-preferences"></a>Erfasste, handschriftliche schrifteinstellungen verwenden

Ein Benutzer kann aus einer vordefinierten Sammlung von Handschrift-Schriftarten verwendet wird, wenn Rendern von Text basierend auf handschriftenerkennung auswählen (finden Sie unter **Einstellungen -> Geräte Pen -> & Windows Ink -> Handschrift-Font > bei Verwendung von handschrifterkennung**).

> [!NOTE]
> Benutzer können sogar eine Schriftart, die basierend auf ihren eigenen Handschrift erstellen.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Ihre app Zugriff auf diese Einstellung und verwenden Sie die ausgewählte Schriftart für den erkannten Text im Textsteuerelement.

In diesem Beispiel wir warten die [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) Ereignis eine [Textfeld](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) und des Benutzers ausgewählten Schriftart anwenden, wenn die textänderungen der HandwritingView (oder die eine Standardschriftart, ist dies nicht der) stammt.

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Zugriff auf die HandwritingView in zusammengesetzten Steuerelementen

Zusammengesetzte Steuerelemente, verwenden die [Textfeld](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) oder [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) Steuerelemente, z. B. [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) unterstützen auch eine [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Für den Zugriff auf die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) verwenden Sie in einem zusammengesetzten Steuerelement, das [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

Der folgende XAML-Ausschnitt zeigt ein [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) Steuerelement.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

In der entsprechenden Code-Behind, erfahren, wie So deaktivieren Sie die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) auf die [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Zuerst behandeln wir der Anwendung geladenen Ereignisses wird aufgerufen, in denen eine FindInnerTextBox-Funktion, um die Traversierung der visuellen Struktur zu starten.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Klicken Sie dann beginnen wir damit, die visuelle Struktur (beginnend bei einem AutoSuggestBox) in der FindInnerTextBox-Funktion mit einem Aufruf von FindVisualChildByName durchlaufen.

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

3. Diese Funktion durchläuft schließlich die visuelle Struktur fort, bis das Textfeld abgerufen wird.

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

## <a name="reposition-the-handwritingview"></a>Verschieben Sie die HandwritingView

In einigen Fällen müssen Sie möglicherweise sicherstellen, dass die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) Benutzeroberflächenelemente, die sie andernfalls nicht möglicherweise behandelt.

Hier erstellen wir ein TextBox-Element, das Diktieren (implementiert durch die Platzierung von einem Textfeld und eine Schaltfläche Diktat in einem StackPanel) unterstützt.

![TextBox mit Diktat](images/handwritingview/textbox-with-dictation.png)

Wie das StackPanel jetzt größer als im Textfeld für die ist die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) möglicherweise nicht alle der zusammengesetzten Cotnrol verdeckt.

![TextBox mit Diktat](images/handwritingview/textbox-with-dictation-handwritingview.png)

Um dieses Problem zu beheben, legen Sie die Eigenschaft "PlacementTarget" die [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) an den UI-Element, das mit der sie ausgerichtet werden soll.

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

Sie können auch die Größe der Festlegen der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), die hilfreich sein können, wenn Sie die Ansicht sicherstellen müssen, nicht wichtig UI verdeckt.

Wie im vorherigen Beispiel erstellen wir ein TextBox-Element, das Diktieren (implementiert durch die Platzierung von einem Textfeld und eine Schaltfläche Diktat in einem StackPanel) unterstützt.

![TextBox mit Diktat](images/handwritingview/textbox-with-dictation.png)

In diesem Fall möchten wir sicherstellen, dass die Schaltfläche mit den Diktatmodus immer sichtbar ist.

![TextBox mit Diktat](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Zu diesem Zweck wird eine die MaxWidth-Eigenschaft der Bindung der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) auf die Breite des UI-Elements, das es verdeckt werden soll.

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

## <a name="reposition-custom-ui"></a>Verschieben der benutzerdefinierten Benutzeroberfläche

Wenn Sie benutzerdefinierte Benutzeroberfläche verfügen, die als Reaktion auf die Texteingabe, z. B. ein nur zu Informationszwecken Popupfenster angezeigt wird, müssen Sie die Benutzeroberfläche neu zu positionieren, damit es nicht die handschrifterkennung Ansicht verdeckt.

![TextBox mit benutzerdefinierten Benutzeroberfläche](images/handwritingview/textbox-with-customui.png)

Das folgende Beispiel zeigt, wie zum Lauschen auf die [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [geschlossen](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
), und [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) Ereignisse der [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) Festlegen der Positionieren von einem [Popup](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Retemplate das HandwritingView-Steuerelement

Wie alle Steuerelemente in einer XAML-Framework wird die visuelle Struktur und das visuelle Verhalten eines anpassbaren eine [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) Ihren speziellen Anforderungen entsprechend.

Zum Erstellen einer benutzerdefinierten Vorlage sehen Sie sich ein vollständiges Beispiel finden Sie unter den [benutzerdefinierten Transport-Steuerelemente erstellen,](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) Gewusst-wie- oder die [benutzerdefinierte bearbeiten Beispielsteuerelement](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








