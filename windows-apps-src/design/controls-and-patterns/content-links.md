---
Description: Verwenden Sie Links zu Inhalten, um umfangreiche Daten in Ihre Textsteuerelemente einzubetten.
title: Links zu Inhalten in Textsteuerelementen
label: Content links
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: b72d7fced63a4c3ef4b1b233afcbb9f9874d9a3f
ms.sourcegitcommit: 81e29e66f0cb1e13c3daa3a239e4dcc4dd674a66
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68796714"
---
# <a name="content-links-in-text-controls"></a>Links zu Inhalten in Textsteuerelementen

Links zu Inhalten bieten eine Möglichkeit, umfangreiche Daten in Textsteuerelemente einzubetten, wodurch Benutzer weitere Informationen zu einer Person oder einem Ort finden und verwenden können, ohne den Kontext Ihrer App verlassen zu müssen.

Wenn der Benutzer einem Eintrag in einer RichEditBox das kaufmännisches Und-Zeichen (@) als Präfix hinzufügt, wird eine Liste der Personen und/oder Ortsvorschläge angezeigt, die mit dem Eintrag übereinstimmen. Wenn der Benutzer dann beispielsweise einen Ort auswählt, wird ein ContentLink für diesen Ort in den Text eingefügt. Wenn der Benutzer den Link zu dem Inhalt aus der RichEditBox aufruft, werden ein Flyout mit einer Karte und zusätzliche Informationen zu dem Ort angezeigt.

> **Wichtige APIs:** [ContentLink-Klasse](/uwp/api/windows.ui.xaml.documents.contentlink), [ContentLinkInfo-Klasse](/uwp/api/windows.ui.text.contentlinkinfo), [RichEditTextRange-Klasse](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> Die APIs für Links zu Inhalten erstrecken sich über die folgenden Namespaces: Windows.UI.Xaml.Controls, Windows.UI.Xaml.Documents und Windows.UI.Text.



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>Links zu Inhalten in Rich-Edit- oder Textblock-Steuerelementen

Es gibt zwei unterschiedliche Methoden, Links zu Inhalten zu verwenden:

1. In einem [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) kann der Benutzer eine Auswahl zum Hinzufügen eines Links öffnen, indem dem Text ein @-Symbol als Präfix angefügt wird. Der Link zu Inhalten wird als Teil des Rich-Text-Inhalts gespeichert.
1. In einem [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) oder [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock) ist der Link zu Inhalten ein Textelement, dessen Verwendung und Verhalten weitestgehend einem [Hyperlink](/uwp/api/windows.ui.xaml.documents.hyperlink) ähnelt.

So sehen Links zu Inhalten standardmäßig in einem RichEditBox und einem Textblock aus.

![Link zu Inhalten im Rich-Edit-Feld](images/content-link-default-richedit.png)
![Link zu Inhalten im Textblock](images/content-link-default-textblock.png)

Unterschiede bei der Verwendung, beim Rendering und beim Verhalten werden in den folgenden Abschnitten ausführlich behandelt. Diese Tabelle bietet einen kurzen Vergleich der Hauptunterschiede zwischen einem Link zu Inhalten in einem RichEditBox und einem Textblock.

| Feature   | RichEditBox | Textblock |
| --------- | ----------- | ---------- |
| Verwendungszweck | ContentLinkInfo-Instanz | ContentLink-Textelement |
| Cursor | Wird durch den Typ des Links zu Inhalten bestimmt, kann nicht geändert werden | Wird durch die Cursor-Eigenschaft bestimmt, standardmäßig **null** |
| ToolTip | Nicht gerendert | Zeigt sekundären Text an |

## <a name="enable-content-links-in-a-richeditbox"></a>Aktiviert Links zu Inhalten in einem RichEditBox

Ein Link zu Inhalten wird am häufigsten verwendet, um einem Benutzer das schnelle Hinzufügen von Informationen zu ermöglichen, indem dem Namen einer Person oder eines Ortes ein kaufmännisches Und-Zeichen (@) im entsprechenden Text vorangestellt wird. Bei Aktivierung in einem [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) wird eine Auswahl geöffnet und der Benutzer kann je nach aktivierter Option eine Person aus seiner Kontaktliste oder einen nahegelegenen Ort hinzufügen.

Der Link zu Inhalten kann mit dem Rich-Text-Inhalt gespeichert werden, und Sie können ihn zur Verwendung in anderen Bereichen Ihrer App extrahieren. In einer E-Mail-App können Sie beispielsweise die persönlichen Informationen extrahieren und verwenden, um das Feld „An“ mit einer E-Mail-Adresse zu füllen.

> [!NOTE]
> Die Auswahl von Links zu Inhalten ist eine App, die Teil von Windows ist und daher in einem separaten Prozess über Ihre App ausgeführt wird.

### <a name="content-link-providers"></a>Anbieter von Links zu Inhalten

Links zu Inhalten werden in einem RichEditBox aktiviert, indem mindestens ein Anbieter für Links zu Inhalten der Sammlung [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders) hinzugefügt wird. Es gibt zwei Anbieter für Links zu Inhalten, die in das XAML-Framework integriert sind.

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider) – Sucht mithilfe der **Personen**-App nach Kontakten.
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider) – Sucht mithilfe der **Karten**-App nach Inhalten.

> [!IMPORTANT]
> Der Standardwert für die RichEditBox.ContentLinkProviders-Eigenschaft ist **null**, nicht etwa eine leere Sammlung. Sie müssen ausdrücklich die [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection) erstellen, bevor Sie Anbieter für Links zu Inhalten hinzufügen.

Hier erfahren Sie, wie Sie die Anbieter für Links zu Inhalten in XAML hinzufügen.

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

Sie können Anbieter für Links zu Inhalten auch in einem Stil hinzufügen und ihn auf mehrere RichEditBoxes wie dieses anwenden.

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

Hier erfahren Sie, wie Sie die Anbieter für Links zu Inhalten in Code hinzufügen.

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>Farben für Links zu Inhalten

Das Aussehen eines Links zu Inhalten wird durch dessen Vordergrund, Hintergrund und Symbol bestimmt. In einer RichEditBox können Sie die Eigenschaften [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) und [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) so festlegen, dass die Standardfarben geändert werden. 

Der Cursor kann nicht festgelegt werden. Der Cursor wird durch das RichEditbox basierend auf dem Typ des Links zu Inhalten gerendert – ein [Person](/uwp/api/windows.ui.core.corecursortype)-Cursor für einen Link zu einer Person oder ein [Pin](/uwp/api/windows.ui.core.corecursortype)-Cursor für einen Link zu einem Ort.

### <a name="the-contentlinkinfo-object"></a>Das ContentLinkInfo-Objekt

Wenn der Benutzer eine Auswahl über die Personen- oder Ortsauswahl vornimmt, erstellt das System ein [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo)-Objekt und fügt es der **ContentLinkInfo**-Eigenschaft des aktuellen [RichEditTextRange ](/uwp/api/windows.ui.text.richedittextrange) hinzu.

Das ContentLinkInfo-Objekt enthält die Informationen, die zum Anzeigen, Aufrufen und Verwalten des Links zu Inhalten verwendet werden.

- **DisplayText** – Dies ist die Zeichenfolge, die beim Rendern des Links zu Inhalten angezeigt wird. In einem RichEditBox kann der Benutzer den Text eines Links zu Inhalten nach dem Erstellen bearbeiten, wodurch der Wert dieser Eigenschaft geändert wird.
- **SecondaryText** – Diese Zeichenfolge wird in der QuickInfo eines gerenderten Links zu Inhalten angezeigt.
  - In einem Link zu Inhalten für Orte, der über die Auswahl erstellt wurde, enthält sie die Adresse des Ortes, falls verfügbar.
- **Uri** – Der Link zu weiteren Informationen zum Betreff des Links zu Inhalten. Mit diesem URI kann eine installierte App oder eine Website geöffnet werden.
- **Id** – Dies ist ein schreibgeschützter Zähler für das jeweilige Steuerelement, der durch das RichEditBox-Steuerelement erstellt wird. Er dient zum Nachverfolgen dieser ContentLinkInfo, wenn beispielsweise Lösch- oder Bearbeitungsaktionen durchgeführt werden. Wenn das ContentLinkInfo-Objekt ausgeschnitten und wieder in das Steuerelement eingefügt wird, erhält es eine neue ID. ID-Werte sind inkrementell.
- **LinkContentKind** – Eine Zeichenfolge, die den Typ des Links zu Inhalten beschreibt. Die integrierten Inhaltstypen sind _Orte_ und _Kontakte_. Beim Wert wird die Groß-/Kleinschreibung berücksichtigt.

#### <a name="link-content-kind"></a>Art des Links zu Inhalten

Es gibt verschiedene Situationen, in denen die LinkContentKind wichtig ist.

- Wenn ein Benutzer einen Link zu Inhalten aus einem RichEditBox kopiert und ihn in ein anderes RichEditBox einfügt, müssen beide Steuerelemente über einen ContentLinkProvider für diesen Inhaltstyp verfügen. Wenn dies nicht der Fall ist, wird der Link als Text eingefügt.
- Sie können LinkContentKind in einem [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged)-Ereignishandler verwenden, um zu bestimmen, welche Aktion mit einem Content Link ausgeführt werden soll, wenn Sie ihn in anderen Bereichen Ihrer App verwenden. Weitere Informationen finden Sie im Abschnitt „Beispiel“.
- Die LinkContentKind wirkt sich darauf aus, wie das System den URI öffnet, wenn der Link aufgerufen wird. Weitere Informationen hierzu erhalten Sie im folgenden Abschnitt zum Starten von URIs.

#### <a name="uri-launching"></a>Starten des URI

Die Uri-Eigenschaft funktioniert ähnlich wie die NavigateUri-Eigenschaft eines Hyperlinks. Wenn ein Benutzer darauf klickt, wird der URI im Standardbrowser oder in der App gestartet, die für das spezielle im URI-Wert angegebene Protokoll registriert ist.

Hier wird das spezifische Verhalten für die beiden integrierten Arten von Link-Inhalten beschrieben.

##### <a name="places"></a>Orte

Die Ortsauswahl erstellt ContentLinkInfo mit dem URI-Stamm https://maps.windows.com/. Es gibt drei Möglichkeiten, diesen Link zu öffnen:

- Wenn LinkContentKind „Orte“ entspricht, wird eine Infokarte in einem Flyout geöffnet. Das Flyout ist vergleichbar mit dem Auswahl-Flyout für Links zu Inhalten. Es ist Bestandteil von Windows und wird in einem separaten Prozess von Ihrer App ausgeführt.
- Wenn LinkContentKind nicht „Orte“ entspricht, wird versucht, die **Karten**-App am angegebenen Speicherort zu öffnen. Dies kann beispielsweise der Fall sein, wenn Sie die LinkContentKind im ContentLinkChanged-Ereignishandler geändert haben.
- Wenn der URI nicht in der Karten-App geöffnet werden kann, wird die Karte im Standardbrowser geöffnet. Dies ist in der Regel dann der Fall, wenn die _Apps für Websites_-Einstellungen des Benutzers das Öffnen der URI mit der **Karten**-App nicht zulassen.

##### <a name="people"></a>People

Die Personenauswahl erstellt eine ContentLinkInfo mit einem Uri, der das **ms-People**-Protokoll verwendet.

- Wenn LinkContentKind „Personen“ entspricht, wird eine Infokarte in einem Flyout geöffnet. Das Flyout ist vergleichbar mit dem Auswahl-Flyout für Links zu Inhalten. Es ist Bestandteil von Windows und wird in einem separaten Prozess von Ihrer App ausgeführt.
- Wenn LinkContentKind nicht „Personen“ entspricht, wird die **Personen**-App geöffnet. Dies kann beispielsweise der Fall sein, wenn Sie die LinkContentKind im ContentLinkChanged-Ereignishandler geändert haben.

> [!TIP]
> Weitere Informationen zum Öffnen anderer Apps und Websites über Ihre App finden Sie unter [Starten einer App mit einem URI](/windows/uwp/launch-resume/launch-app-with-uri).

#### <a name="invoked"></a>Aufgerufen

Wenn der Benutzer einen Link zu Inhalten aufruft, wird das [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked)-Ereignis vor dem Standardverhalten zum Starten des URI ausgelöst. Sie können dieses Ereignis behandeln, um das Standardverhalten außer Kraft zu setzen oder abzubrechen.

In diesem Beispiel wird veranschaulicht, wie Sie das standardmäßige Aufrufverhalten für einen Link zu Inhalten vom Typ „Ort“ außer Kraft setzen können. Anstatt die Karte in einer Ort-Informationskarte, in der Karten-App oder im Standardwebbrowser zu öffnen, kennzeichnen Sie das Ereignis als „Handled“ und öffnen die Karte in einem [WebView](/uwp/api/windows.ui.xaml.controls.webview)-In-App-Steuerelement.

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

Sie können das [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) -Ereignis verwenden, um benachrichtigt zu werden, wenn ein Link zu Inhalten hinzugefügt, geändert oder entfernt wird. Auf diese Weise können Sie den Link zu Inhalten aus dem Text extrahieren und ihn in anderen Bereichen Ihrer App verwenden. Dies wird später im Abschnitt „Beispiele“ veranschaulicht.

Die Eigenschaften der [ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) lauten wie folgt:

- **ChangedKind** – Diese ContentLinkChangeKind-Enumeration ist die Aktion innerhalb des ContentLink. Wenn der ContentLink beispielsweise eingefügt, entfernt oder bearbeitet wird.
- **Info** – Die ContentLinkInfo, die das Ziel der Aktion war.

Dieses Ereignis wird für jede ContentLinkInfo-Aktion ausgelöst. Wenn der Benutzer beispielsweise mehrere Links zu Inhalten gleichzeitig kopiert und in das RichEditBox einfügt, wird dieses Ereignis für jedes hinzugefügte Element ausgelöst. Oder wenn der Benutzer mehrere Links zu Inhalten gleichzeitig auswählt und löscht, wird dieses Ereignis für jedes gelöschte Element ausgelöst.

Dieses Ereignis wird nach dem **TextChanging**-Ereignis und vor dem **TextChanged**-Ereignis für das RichEditBox ausgelöst.

#### <a name="enumerate-content-links-in-a-richeditbox"></a>Aufzählen von Links zu Inhalten in einem RichEditBox

Sie können die RichEditTextRange.ContentLink-Eigenschaft verwenden, um einen Link zu Inhalten aus einem Rich-Edit-Dokument abzurufen. Die TextRangeUnit-Enumeration hat den Wert ContentLink, um den Link zu Inhalten als eine Einheit festzulegen, die beim Navigieren in einem Textbereich verwendet werden soll.

Dieses Beispiel zeigt, wie Sie alle Links zu Inhalten in einem RichEditBox aufzählen und die Personen in einer Liste extrahieren können.

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>ContentLink-Textelement

Um einen Link zu Inhalten in einem TextBlock- oder RichTextBlock-Steuerelement verwenden zu können, verwenden Sie das [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink)-Textelement (aus dem Windows.UI.Xaml.Documents-Namespace).

Nachfolgend finden Sie typische Quellen für einen ContentLink in einem Textblock:

- Ein von einer Auswahl erstellter Link zu Inhalten, den Sie aus einem RichTextBlock-Steuerelement extrahiert haben.
- Ein Link zu Inhalten für einen Ort, den Sie in Ihrem Code erstellen.

In anderen Situationen ist ein Hyperlink-Textelement in der Regel geeignet.

### <a name="contentlink-appearance"></a>ContentLink-Aussehen

Das Aussehen eines Links zu Inhalten wird durch dessen Vordergrund, Hintergrund und Cursor bestimmt. In einem Textblock können Sie die Vordergrund-Eigenschaft (von TextElement) und [Hintergrund](/uwp/api/windows.ui.xaml.documents.contentlink.background)-Eigenschaft so festlegen, dass die Standardfarben geändert werden.

Der [Hand](/uwp/api/windows.ui.core.corecursortype)-Cursor wird standardmäßig angezeigt, wenn der Benutzer mit der Maus auf den Link zu Inhalten zeigt. Im Gegensatz zum RichEditBlock ändern TextBlock-Steuerelemente den Cursor nicht automatisch entsprechend dem Linktyp. Sie können die [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor)-Eigenschaft so festlegen, dass der Cursor entsprechend dem Linktyp oder anderen Faktoren geändert wird.

Im Folgenden finden Sie ein Beispiel für einen ContentLink, der in einem TextBlock verwendet wird. Die ContentLinkInfo wird im Code erstellt und der Info-Eigenschaft des ContentLink-Textelements zugewiesen, das in XAML erstellt wird.

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> Wenn Sie einen ContentLink in einem Textsteuerelement mit anderen Textelementen in XAML verwenden, platzieren Sie den Inhalt in einem [Span](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.span)-Container und wenden das Attribut `xml:space="preserve"` auf den Span-Container an, um die Leerstelle zwischen dem ContentLink und anderen Elementen beizubehalten.

## <a name="examples"></a>Beispiele

In diesem Beispiel kann ein Benutzer einen Link zu Inhalten für eine Person oder einen Ort eingeben oder in einem RickTextBlock platzieren. Sie verarbeiten das ContentLinkChanged-Ereignis, um die Links zu Inhalten zu extrahieren und sie in einer Personen- oder Ortsliste auf dem neuesten Stand zu halten.

In den Elementvorlagen für die Listen können Sie einen TextBlock mit einem ContentLink-Textelement verwenden, um die Informationen zum Link zu Inhalten anzuzeigen. Die ListView stellt ihren eigenen Hintergrund für jedes Element bereit, sodass der ContentLink-Hintergrund auf „Transparent“ festgelegt wird.

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```
