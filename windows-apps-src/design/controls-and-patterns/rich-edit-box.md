---
Description: Sie können ein RichEditBox-Steuerelement verwenden, um Rich-Text-Dokumente zu bearbeiten, die formatierten Text, Hyperlinks und Bilder enthalten. Sie können den Schreibschutz für ein RichEditBox-Steuerelement aktivieren, indem Sie die IsReadOnly-Eigenschaft des Steuerelements auf „true“ festlegen.
title: RichEditBox
ms.assetid: 4AFC0DFA-3B89-434D-9F86-4309CCFF7839
label: Rich edit box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cf2eebdc97a1052dd3f4a3e00dd3a911cb80bace
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970665"
---
# <a name="rich-edit-box"></a>Rich-Edit-Feld

Sie können ein RichEditBox-Steuerelement verwenden, um Rich-Text-Dokumente zu bearbeiten, die formatierten Text, Hyperlinks und Bilder enthalten. Sie können den Schreibschutz für ein RichEditBox-Steuerelement aktivieren, indem Sie die IsReadOnly-Eigenschaft des Steuerelements auf **true** festlegen.

**Abrufen der Windows-UI-Bibliothek**

|  |  |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Die Bibliothek „Windows UI“ enthält ab Version 2.2 eine neue Vorlage für dieses Steuerelement, die abgerundete Ecken verwendet. Weitere Informationen finden Sie unter [Eckradius](/windows/uwp/design/style/rounded-corner). „WinUI“ ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Plattform-APIs:** [RichEditBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox), [Document-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.document), [IsReadOnly-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isreadonly), [IsSpellCheckEnabled-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isspellcheckenabled)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie **RichEditBox** zum Anzeigen und Bearbeiten von Textdateien. Ein RichEditBox wird nicht dazu verwendet, in der Weise Benutzereingaben in Ihrer App zu erhalten, wie es bei anderen, standardmäßigen Texteingabefeldern erfolgt. Es wird vielmehr dazu verwendet, mit Textdateien zu arbeiten, die von Ihrer App getrennt sind. Den in ein RichEditBox-Element eingegebenen Text speichern Sie üblicherweise in einer RTF-Datei.
-   Wenn der Hauptzweck des mehrzeiligen Textfelds darin besteht, schreibgeschützte Dokumente zu erstellen (z. B. Blogeinträge oder die Inhalte einer E-Mail-Nachricht) und diese Dokumente Rich-Text erfordern, verwenden Sie stattdessen ein [Rich-Text-Feld](/windows/uwp/design/controls-and-patterns/rich-text-block).
-   Wenn Texte erfasst werden, die nur genutzt und nicht für die Benutzer erneut angezeigt werden, verwenden Sie ein Nur-Text-Eingabesteuerelement.
-   Verwenden Sie in allen anderen Szenarien ein Nur-Text-Eingabesteuerelement.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RichEditBox">die App zu öffnen und RichEditBox in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

In diesem Rich-Edit-Feld ist ein Rich-Text-Dokument geöffnet. Die Formatierungs- und Dateischaltflächen sind nicht Teil des Rich-Edit-Felds, Sie sollten aber zumindest grundlegende Formatierungsschaltflächen bereitstellen und ihre Aktionen implementieren.

![Ein Rich-Text-Feld mit einem geöffneten Dokument](images/rich-edit-box.png)

## <a name="create-a-rich-edit-box"></a>Erstellen eines Rich-Edit-Felds

RichEditBox unterstützt standardmäßig die Rechtschreibprüfung. Um die Rechtschreibprüfung zu deaktivieren, legen Sie die [IsSpellCheckEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.isspellcheckenabled)-Eigenschaft auf **false** fest. Weitere Informationen finden Sie im Artikel [Richtlinien für die Rechtschreibprüfung](text-controls.md).

Mit der [Document](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.document)-Eigenschaft von RichEditBox können Sie den Inhalt des Steuerelements abrufen. Der Inhalt eines RichEditBox ist anders als das RichTextBlock-Steuerelement ein [Windows.UI.Text.ITextDocument](https://docs.microsoft.com/windows/desktop/api/tom/nn-tom-itextdocument)-Objekt, das [Windows.UI.Xaml.Documents.Block](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Block)-Objekte als Inhalte verwendet. Die ITextDocument-Schnittstelle bietet unter anderem folgende Möglichkeiten: Dokumente in einen Datenstrom laden und speichern, Textbereiche abrufen, die aktive Auswahl abrufen, Änderungen rückgängig machen und wiederholen, Standardformatierungsattribute festlegen usw.

Dieses Beispiel zeigt, wie Sie eine RTF-Datei (Rich Text Format) bearbeiten, sie in ein RichEditBox laden und speichern.

```xaml
<RelativePanel Margin="20" HorizontalAlignment="Stretch">
    <RelativePanel.Resources>
        <Style TargetType="AppBarButton">
            <Setter Property="IsCompact" Value="True"/>
        </Style>
    </RelativePanel.Resources>
    <AppBarButton x:Name="openFileButton" Icon="OpenFile"
                  Click="OpenButton_Click" ToolTipService.ToolTip="Open file"/>
    <AppBarButton Icon="Save" Click="SaveButton_Click"
                  ToolTipService.ToolTip="Save file"
                  RelativePanel.RightOf="openFileButton" Margin="8,0,0,0"/>

    <AppBarButton Icon="Bold" Click="BoldButton_Click" ToolTipService.ToolTip="Bold"
                  RelativePanel.LeftOf="italicButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="italicButton" Icon="Italic" Click="ItalicButton_Click"
                  ToolTipService.ToolTip="Italic" RelativePanel.LeftOf="underlineButton" Margin="0,0,8,0"/>
    <AppBarButton x:Name="underlineButton" Icon="Underline" Click="UnderlineButton_Click"
                  ToolTipService.ToolTip="Underline" RelativePanel.AlignRightWithPanel="True"/>

    <RichEditBox x:Name="editor" Height="200" RelativePanel.Below="openFileButton"
                 RelativePanel.AlignLeftWithPanel="True" RelativePanel.AlignRightWithPanel="True"/>
</RelativePanel>
```


```csharp
private async void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open a text file.
    Windows.Storage.Pickers.FileOpenPicker open =
        new Windows.Storage.Pickers.FileOpenPicker();
    open.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    open.FileTypeFilter.Add(".rtf");

    Windows.Storage.StorageFile file = await open.PickSingleFileAsync();

    if (file != null)
    {
        try
        {
            Windows.Storage.Streams.IRandomAccessStream randAccStream =
        await file.OpenAsync(Windows.Storage.FileAccessMode.Read);

            // Load the file into the Document property of the RichEditBox.
            editor.Document.LoadFromStream(Windows.UI.Text.TextSetOptions.FormatRtf, randAccStream);
        }
        catch (Exception)
        {
            ContentDialog errorDialog = new ContentDialog()
            {
                Title = "File open error",
                Content = "Sorry, I couldn't open the file.",
                PrimaryButtonText = "Ok"
            };

            await errorDialog.ShowAsync();
        }
    }
}

private async void SaveButton_Click(object sender, RoutedEventArgs e)
{
    Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;

    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Rich Text", new List<string>() { ".rtf" });

    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";

    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
    if (file != null)
    {
        // Prevent updates to the remote version of the file until we
        // finish making changes and call CompleteUpdatesAsync.
        Windows.Storage.CachedFileManager.DeferUpdates(file);
        // write to file
        Windows.Storage.Streams.IRandomAccessStream randAccStream =
            await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

        editor.Document.SaveToStream(Windows.UI.Text.TextGetOptions.FormatRtf, randAccStream);

        // Let Windows know that we're finished changing the file so the
        // other app can update the remote version of the file.
        Windows.Storage.Provider.FileUpdateStatus status = await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
        if (status != Windows.Storage.Provider.FileUpdateStatus.Complete)
        {
            Windows.UI.Popups.MessageDialog errorBox =
                new Windows.UI.Popups.MessageDialog("File " + file.Name + " couldn't be saved.");
            await errorBox.ShowAsync();
        }
    }
}

private void BoldButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Bold = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void ItalicButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        charFormatting.Italic = Windows.UI.Text.FormatEffect.Toggle;
        selectedText.CharacterFormat = charFormatting;
    }
}

private void UnderlineButton_Click(object sender, RoutedEventArgs e)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
        if (charFormatting.Underline == Windows.UI.Text.UnderlineType.None)
        {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.Single;
        }
        else {
            charFormatting.Underline = Windows.UI.Text.UnderlineType.None;
        }
        selectedText.CharacterFormat = charFormatting;
    }
}
```

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Auswählen der richtigen Tastatur für Ihr Textsteuerelement

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Das Standardtastaturlayout ist in der Regel für die Arbeit mit Rich-Text-Dokumenten geeignet.

Weitere Informationen zur Verwendung von Eingabeumfängen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard).

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Wenn Sie ein Rich-Text-Feld erstellen, sollten Sie Stilschaltflächen bereitstellen und die entsprechenden Aktionen implementieren.
- Verwenden Sie eine Schriftart, die mit dem Stil der App konsistent ist.
- Legen Sie die Höhe des Textsteuerelements so fest, dass genügend Platz für typische Einträge vorhanden ist.
- Die Höhe der Texteingabesteuerelemente sollte während der Benutzereingabe nicht zunehmen.
- Verwenden Sie kein mehrzeiliges Textfeld, wenn die Benutzer nur eine Zeile benötigen.
- Verwenden Sie kein Rich-Text-Steuerelement, wenn ein Nur-Text-Steuerelement ausreicht.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Richtlinien für die Rechtschreibprüfung](text-controls.md)
- [Hinzufügen von Suchfunktionen](search.md)
- [Richtlinien für die Texteingabe](text-controls.md)
- [TextBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
