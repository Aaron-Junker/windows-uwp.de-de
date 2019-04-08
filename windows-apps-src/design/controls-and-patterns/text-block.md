---
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
Description: Der Textblock ist das wichtigste Steuerelement zum Anzeigen von schreibgeschütztem Text in Apps.
title: Textblock
label: Text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 692b8837f3bd74dfc5f74bee02786213c9a898f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599385"
---
# <a name="text-block"></a>Textblock

 

 Der Textblock ist das wichtigste Steuerelement zum Anzeigen von schreibgeschütztem Text in Apps. Sie können es zum Anzeigen von einzeiligem oder mehrzeiligem Text, Inlinelinks und Text mit Formatierung, z. B. fett, kursiv oder unterstrichen, verwenden.
 
 > **Wichtige APIs:** [TextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [Texteigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx), [Inlines-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Ein Textblock ist in der Regel einfacher zu verwenden und bietet eine bessere Leistung beim Rendern von Text als ein Rich-Text-Block. Daher wird er in der Regel für App-UI-Text bevorzugt. Sie können über einen Textblock in Ihrer App ganz einfach auf den Text zugreifen und ihn verwenden, indem Sie den Wert der [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)-Eigenschaft abrufen. Er enthält außerdem viele der gleichen Formatierungsoptionen zum Anpassen des Renderns von Text.

Sie können zwar Zeilenumbrüche in den Text einfügen, jedoch ist der Textblock zum Anzeigen eines einzelnen Absatzes vorgesehen und unterstützt keinen Texteinzug. Verwenden Sie **RichTextBlock**, wenn Sie Unterstützung für mehrere Absätze, mehrspaltigen Text, andere komplexe Textlayouts oder Inline-UI-Elemente, z. B. Bilder, benötigen.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TextBlock">die App zu öffnen und TextBlock in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-text-block"></a>Erstellen eines Textblocks

Im Folgenden wird veranschaulicht, wie Sie ein einfaches TextBlock-Steuerelement definieren und seine Text-Eigenschaft auf eine Zeichenfolge festlegen.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### <a name="content-model"></a>Inhaltsmodell

Es gibt zwei Eigenschaften, die Sie verwenden können, um Inhalt an einen TextBlock hinzuzufügen: [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) und [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

Das häufigste Verfahren zum Anzeigen von Text ist das Festlegen der Text-Eigenschaft auf einen Zeichenfolgenwert, wie im vorherigen Beispiel gezeigt.

Sie können auch Inhalte hinzufügen, indem Sie wie hier gezeigt Elemente mit fließendem Inlineinhalt in die TextBox.Inlines-Eigenschaft einfügen.
```xaml
<TextBlock>Text can be <Bold>bold</Bold>, <Underline>underlined</Underline>, 
    <Italic>italic</Italic>, or a <Bold><Italic>combination</Italic></Bold>.</TextBlock>
```

Von der Inline-Klasse abgeleitete Elemente, z. B. Bold, Italic, Run, Span und LineBreak, ermöglichen unterschiedliche Formatierungen für unterschiedliche Teile des Texts. Weitere Informationen finden Sie im Abschnitt [Formatieren von Text](#formatting-text). Mit dem Inline-Hyperlink-Element können Sie dem Text einen Link hinzufügen. Durch die Verwendung von Inlines wird jedoch auch das Rendern von Text im schnellen Pfad deaktiviert, wie im nächsten Abschnitt erläutert.


## <a name="performance-considerations"></a>Leistungsaspekte

XAML verwendet, wenn möglich, einen effizienteren Codepfad für Layouttext. Dieser schnelle Pfad verringert die gesamte Arbeitsspeicherauslastung und reduziert erheblich die CPU-Zeit für die Abmessung und Anordnung von Text. Dieser schnelle Pfad gilt nur für TextBlock und sollte deshalb nach Möglichkeit RichtTextBlock gegenüber bevorzugt werden.

Bestimmte Bedingungen erfordern TextBlock, um auf einen prozessorintensiven Codepfad mit zahlreichen Funktionen zum Rendern von Text zurückzugreifen. Damit das Rendern von Text im schnellen Pfad weiterhin ausgeführt wird, beachten Sie beim Festlegen der Eigenschaften unbedingt die im Folgenden aufgeführten Richtlinien.
- [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): Die wichtigste Bedingung ist, dass der schnelle Pfad verwendet wird, nur, wenn Sie Text festlegen, durch die Text-Eigenschaft explizit festlegen, entweder in XAML oder in Code (wie in den vorherigen Beispielen gezeigt wird). Beim Festlegen des Textes über die Inlines-Sammlung von TextBlock (z. B. `<TextBlock>Inline text</TextBlock>` wird der schnelle Pfad aufgrund der potenziellen Komplexität mehrerer Formate deaktiviert.
- [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): Nur der Standardwert 0 ist schnellen Pfad.
- [TextTrimming](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): Nur die **keine**, **CharacterEllipsis**, und **WordEllipsis** Werte schnellen Pfad. Der **Clip**-Wert deaktiviert den schnellen Pfad.

> **Hinweis**&nbsp;&nbsp;Vor Windows 10, Version 1607 wird der schnelle Pfad durch weitere Eigenschaften beeinflusst. Wenn Ihre App unter einer früheren Windows-Version ausgeführt wird, wird Text unter diesen Bedingungen auf dem langsamen Pfad gerendert. Weitere Informationen zu Versionen finden Sie unter „Versionsadaptiver Code“.
- [Typografie](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): Nur die Standardwerte für die verschiedenen Typografie-Eigenschaften sind schnelle Pfad.
- [Von LineStackingStrategy](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): Wenn [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) ist nicht 0 ist, die **BaselineToBaseline** und **MaxHeight** Werte deaktivieren Sie den schnellen Pfad.
- [IsTextSelectionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): Nur **"false"** schnellen Pfad ist. Wenn diese Eigenschaft auf **true** festgelegt wird, wird der schnelle Pfad deaktiviert.

Sie können die [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx)-Eigenschaft während des Debuggens auf **true** festlegen, um festzustellen, ob das Rendern von Text im schnellen Pfad erfolgt. Wenn diese Eigenschaft auf „true“ festgelegt ist, wird der Text im schnellen Pfad in Hellgrün angezeigt.

>**Tipp**&nbsp;&nbsp;dieses Feature wird ausführlich erläutert, in dieser Sitzung von Build 2015 - [XAML-Leistung: Techniken zum Maximieren der universellen Windows-App-Umgebungen, die mit XAML erstellten](https://channel9.msdn.com/Events/Build/2015/3-698).



Debugeinstellungen legen Sie in der Regel wie folgt in der [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx)-Methodenüberschreibung in der CodeBehind-Seite für „App.xaml” fest.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

In diesem Beispiel wird die erste TextBlock-Klasse unter Verwendung des schnellen Pfads gerendert, während dies bei der zweiten nicht der Fall ist.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Wenn Sie diesen XAML-Code im Debugmodus mit der auf „true“ festgelegten IsTextPerformanceVisualizationEnabled-Eigenschaft ausführen, sieht das Ergebnis wie folgt aus.

![Im Debugmodus gerenderter Text](images/text-block-rendering-performance.png)

>**Achtung**&nbsp;&nbsp;Die Farbe von Text, der nicht im schnellen Pfad enthalten ist, wird nicht geändert. Wenn die Farbe von Text in der App als Hellgrün angegeben ist, wird der Text auch noch in Hellgrün angezeigt, wenn er sich im langsameren Renderingpfad befindet. Verwechseln Sie Text, der in der App auf Grün festgelegt ist, nicht mit Text im schnellen Pfad, der aufgrund der Debugeinstellungen grün ist.

## <a name="formatting-text"></a>Formatieren von Text

Obwohl die Text-Eigenschaft Nur-Text speichert, können Sie verschiedene Formatierungsoptionen auf das TextBlock-Steuerelement anwenden, um das Rendern des Texts in der App anzupassen. Sie können Standard-Steuerelementeigenschaften, z. B. FontFamily, FontSize, FontStyle, Foreground und CharacterSpacing festlegen, um das Erscheinungsbild des Texts zu ändern. Sie können den Text auch mit Inlinetextelementen und angefügten Typografie-Eigenschaften formatieren. Diese Optionen beeinflussen nur die lokale Anzeige des Texts im TextBlock. Wenn Sie den Text kopieren und z. B. in ein Rich-Text-Steuerelement einfügen, wird daher keine Formatierung angewendet.

>**Hinweis**&nbsp;&nbsp;Beachten Sie, dass Inlinetextelemente und nicht standardmäßige Typografiewerte nicht im schnellen Pfad gerendert werden (dies wurde im vorherigen Abschnitt erläutert).


### <a name="inline-elements"></a>Inline-Elemente

Der [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx)-Namespace bietet eine Vielzahl von Inlinetextelementen, mit denen Sie Text formatieren können, z. B. Bold, Italic, Run, Span und LineBreak.

Sie können in einem TextBlock auch mehrere Zeichenfolgen anzeigen und jede Zeichenfolge anders formatieren. Dazu verwenden Sie ein Run-Element für die Anzeige der einzelnen Zeichenfolgen mit der zugehörigen Formatierung. Trennen Sie die einzelnen Run-Elemente dann mit einem LineBreak-Element.

So definieren Sie unterschiedlich formatierte Textzeichenfolgen in einem TextBlock mit Run-Objekten, die durch einen LineBreak voneinander getrennt sind.
```xaml
<TextBlock FontFamily="Segoe UI" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Segoe UI Light" FontSize="24">
        Segoe UI Light 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Georgia" FontSize="18" FontStyle="Italic">
        Georgia Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="Black" FontFamily="Arial" FontSize="14" FontWeight="Bold">
        Arial Bold 14
    </Run>
</TextBlock>
```

Dies ist das Ergebnis.

![Mit Run-Elementen formatierter Text](images/text-block-run-examples.png)

### <a name="typography"></a>Typografie

Die angefügten Eigenschaften der [Typografie](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)-Klasse ermöglichen den Zugriff auf eine Reihe von Microsoft OpenType-Typografieeigenschaften. Sie können diese angefügten Eigenschaften entweder für das TextBlock-Element oder für einzelne Inlinetextelemente festlegen. In diesen Beispielen wird beides gezeigt.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Richtlinien für die Rechtschreibprüfung](text-controls.md)
- [Hinzufügen von Suchfunktionen](search.md)
- [Richtlinien für die Texteingabe](text-controls.md)
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length-Eigenschaft](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
