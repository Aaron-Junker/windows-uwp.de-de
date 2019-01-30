---
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0ab83e74dc59b407c15e1a8213540c8954fcd16e
ms.sourcegitcommit: a60ab85e9f2f9690e0141050ec3aa51f18ec61ec
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "9037045"
---
# <a name="rich-text-block"></a>Rich-Text-Block

 

Rich-Text-Blöcke bieten verschiedene Features für erweitertes Textlayout, die Sie verwenden können, wenn Sie Unterstützung für Absätze, Inline-UI-Elemente oder komplexe Textlayouts benötigen.

> **Wichtige APIs**: [RichTextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [RichTextBlockOverflow-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx), [Paragraph-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx), [Typography-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie **RichTextBlock**, wenn Sie Unterstützung für mehrere Absätze, mehrspaltige oder andere komplexe Textlayouts oder Inline-UI-Elemente wie Bilder benötigen.

Mit **TextBlock** können Sie die meisten schreibgeschützten Texte in Ihrer App anzeigen. Sie können es zum Anzeigen von einzeiligem oder mehrzeiligem Text, Inlinelinks und Text mit Formatierung, z. B. fett, kursiv oder unterstrichen, verwenden. TextBlock stellt ein einfacheres Inhaltsmodell bereit. Daher ist er in der Regel einfacher zu verwenden und bietet eine bessere Leistung beim Rendern von Text als RichTextBlock. Er wird für den meisten UI-Text in Apps bevorzugt. Sie können zwar Zeilenumbrüche in den Text einfügen, jedoch ist TextBlock zum Anzeigen eines einzelnen Absatzes vorgesehen und unterstützt keinen Texteinzug.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel über [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RichTextBlock">die App zu öffnen und RichTextBlock in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Erstellen eines Rich-Text-Blocks

Die Inhaltseigenschaft von RichTextBlock ist die [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx)-Eigenschaft, die über das [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)-Element absatzbasierten Text unterstützt. Es gibt keine **Text**-Eigenschaft, die Sie für einen bequemen Zugriff auf den Textinhalt des Steuerelements in Ihrer App verwenden können. RichTextBlock bietet jedoch verschiedene einzigartige Features, die TextBlock nicht bereitstellt. 

Von RichTextBlock unterstützte Features:
- Mehrere Absätze. Legen Sie den Einzug für Absätze mit der [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx)-Eigenschaft fest.
- Inline-UI-Elemente. Verwenden Sie einen [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx), um UI-Elemente, z. B. Bilder, inline im Text anzuzeigen.
- Überlaufcontainer. Verwenden Sie [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)-Elemente, um mehrspaltige Textlayouts zu erstellen.

### <a name="paragraphs"></a>Absätze

Sie definieren mit [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)-Elementen die Textblöcke, die in einem RichTextBlock-Steuerelement angezeigt werden sollen. Jeder RichTextBlock sollte mindestens einen Paragraph enthalten. 

Mit der [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx)-Eigenschaft können Sie die Größe des Einzugs für alle Absätze in einem RichTextBlock festlegen. Sie können diese Einstellung für bestimmte Absätze in einem RichTextBlock überschreiben, indem Sie die [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx)-Eigenschaft auf einen anderen Wert festlegen.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Inline-UI-Elemente

Mit der [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx)-Klasse können Sie jedes UIElement inline im Text einbetten. Ein gängiges Szenario ist das Einfügen eines Inline-Elements vom Typ „Image“ in den Text. Sie können aber natürlich auch interaktive Elemente wie „Button“ oder „CheckBox“ verwenden.

Wenn Sie mehrere Elemente inline an der gleichen Position einbetten möchten, können Sie ein Panel als einzelnes untergeordnetes InlineUIContainer-Element verwenden und dann mehrere Elemente in diesem Panel platzieren.

In diesem Beispiel wird veranschaulicht, wie mithilfe eines InlineUIContainer ein Bild in einen RichTextBlock eingefügt wird. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>Überlaufcontainer

Sie können einen RichTextBlock mit [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)-Elementen verwenden, um mehrspaltige Seitenlayouts oder andere erweiterte Seitenlayouts zu erstellen. Der Inhalt für ein RichTextBlockOverflow-Element stammt immer aus einem RichTextBlock-Element. Sie verknüpfen RichTextBlockOverflow-Elemente, indem Sie sie als OverflowContentTarget eines RichTextBlock-Elements oder eines anderen RichTextBlockOverflow-Elements festlegen.

Hier ist ein einfaches Beispiel, in dem ein zweispaltiges Layout erstellt wird. Ein komplexeres Beispiel finden Sie im Abschnitt „Beispiele“.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>Formatieren von Text

Obwohl der RichTextBlock Nur-Text speichert, können Sie verschiedene Formatierungsoptionen anwenden, um das Rendern des Texts in der App anzupassen. Sie können Standard-Steuerelementeigenschaften, z. B. FontFamily, FontSize, FontStyle, Foreground und CharacterSpacing, festlegen, um das Erscheinungsbild des Texts zu ändern. Sie können den Text auch mit Inlinetextelementen und angefügten Typography-Eigenschaften formatieren. Diese Optionen beeinflussen nur die lokale Anzeige des Texts im RichTextBlock. Wenn Sie den Text kopieren und z. B. in ein Rich-Text-Steuerelement einfügen, wird daher keine Formatierung angewendet.

### <a name="inline-elements"></a>Inline-Elemente

Der [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx)-Namespace bietet eine Vielzahl von Inlinetextelementen, mit denen Sie Text formatieren können, z. B. Bold, Italic, Run, Span und LineBreak. Ein typisches Verfahren zum Formatieren von Textabschnitten ist das Einfügen des Texts in ein Run-Element oder Span-Element und das anschließende Festlegen der Eigenschaften für das Element.

Dies ist ein Paragraph, in dem der erste Ausdruck als fett formatierter blauer Text mit dem Schriftgrad 16 pt angezeigt wird.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Typografie

Die angefügten Eigenschaften der [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)-Klasse ermöglichen den Zugriff auf eine Reihe von Microsoft OpenType-Typografieeigenschaften. Sie können diese angefügten Eigenschaften entweder für den RichTextBlock oder für einzelne Inlinetextelemente festlegen, wie hier gezeigt.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>Empfehlungen

Siehe „Typografie“ und „Richtlinien für Schriftarten“.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementkatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

[Textsteuerelemente](text-controls.md)

**Für Designer**
- [Richtlinien für die Rechtschreibprüfung](text-controls.md)
- [Hinzufügen von Suchfunktionen](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Richtlinien für die Texteingabe](text-controls.md)

**Für Entwickler (XAML)**
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)


**Für Entwickler (Sonstige)**
- [StringLength-Eigenschaft](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
