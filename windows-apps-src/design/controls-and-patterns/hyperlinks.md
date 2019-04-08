---
Description: Über Hyperlinks können Benutzer zu einem anderen Teil der App oder zu einer anderen App navigieren oder mit einer separaten Browser-App einen bestimmten URI (Uniform Resource Identifier) starten.
title: Hyperlinks
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9aa5053d83518c61335807874cd0d31943a60695
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606475"
---
# <a name="hyperlinks"></a>Hyperlinks

 

Über Hyperlinks können Benutzer zu einem anderen Teil der App oder zu einer anderen App navigieren oder mit einer separaten Browser-App einen bestimmten URI (Uniform Resource Identifier) starten. Sie haben zwei Möglichkeiten, einer XAML-App einen Link hinzuzufügen: über das Textelement **Link** oder das Steuerelement **HyperlinkButton**.

> **Wichtige APIs:** [Text des Linkelements](https://msdn.microsoft.com/library/windows/apps/dn279356), [HyperlinkButton-Steuerung](https://msdn.microsoft.com/library/windows/apps/br242739)

![Eine Linkschaltfläche](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie einen Link, wenn Text erforderlich ist, der bei Auswahl reagiert und der weitere Informationen zum ausgewählten Text aufruft.

Wählen Sie den richtigen Linktyp basierend auf Ihren Anforderungen:

-   Verwenden Sie innerhalb eines Textsteuerelements ein Inline-**Link**textelement. Ein Linkelement wird mit anderen Textelementen umgebrochen, und Sie können es in einem beliebigen InlineCollection-Element verwenden. Verwenden Sie einen Textlink, wenn Sie automatischen Textumbruch nutzen möchten und nicht unbedingt ein großes Tippziel benötigen. Der Linktext kann klein sein und sich nur schwer auswählen lassen, insbesondere bei der Toucheingabe.
-   Verwenden Sie ein **HyperlinkButton**-Element für eigenständige Links. Ein HyperlinkButton-Element ist ein spezielles Schaltflächen-Steuerelement, das Sie überall dort verwenden können, wo Sie eine Schaltfläche verwenden würden.
-   Verwenden Sie ein **HyperlinkButton**-Element mit einem [Bild](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx)als Inhalt, um ein klickbares Bild zu erstellen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/HyperlinkButton">die App zu öffnen und HyperlinkButton in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>Erstellen eines Linktextelements

In diesem Beispiel wird veranschaulicht, wie Sie ein Linktextelement in einem [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) verwenden.

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
Der Link wird inline angezeigt und mit dem umgebenden Text umbrochen:

![Beispiel für einen Link als Textelement](images/controls_hyperlink-element.png) 

> **Tipp:**&nbsp;&nbsp;Wenn Sie einen Link in einem Textsteuerelement mit anderen Textelementen in XAML verwenden, platzieren Sie den Inhalt in einem [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx)-Container und wenden das Attribut `xml:space="preserve"` auf den Span-Container an, um die Leerstelle zwischen dem Link und anderen Elementen beizubehalten.

## <a name="create-a-hyperlinkbutton"></a>Erstellen eines HyperlinkButton-Elements

Hier sehen Sie, wie Sie ein HyperlinkButton-Element sowohl mit Text als auch mit Bild verwenden.

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

Die Linkschaltflächen mit Textinhalt werden als markierter Text angezeigt. Das Contoso-Logobild ist ebenfalls ein klickbarer Link:

![Beispiel für einen Link als Schaltflächensteuerelement](images/controls_hyperlink-button-image.png)

Dieses Beispiel zeigt, wie Sie ein einfaches HyperlinkButton-Element in Code erstellen.

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>Handhaben der Navigation

Die Navigation wird bei beiden Linktypen gleich gehandhabt. Sie können die Eigenschaft **NavigateUri** festlegen oder das **Click**-Ereignis behandeln.

**Navigieren Sie zu einem URI**

Wenn Sie mit dem Link zu einem URI navigieren möchten, legen Sie die NavigateUri-Eigenschaft fest. Wenn ein Benutzer auf den Link klickt oder tippt, wird der angegebene URI im Standardbrowser geöffnet. Der Standardbrowser wird in einem separaten Prozess von Ihrer App ausgeführt.

> [!NOTE]
> Ein URI wird durch die [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri)-Klasse dargestellt. Bei der Programmierung mit .NET wird diese Klasse ausgeblendet und sollte die [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri)-Klasse verwendet werden. Weitere Informationen finden Sie auf den Referenzseiten für diese Klassen.

Sie müssen keine Schemas wie **http:** oder **https:** verwenden. Sie können Schemas wie **ms-appx:**, **ms-appdata:** oder **ms-resources:** verwenden, falls dort Ressourceninhalte vorhanden sind, die in einem Browser geladen werden können. Das Schema **file:** ist ausdrücklich blockiert. Weitere Informationen finden Sie unter [URI-Schemas](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx).

Wenn ein Benutzer auf den Link klickt, wird der Wert der NavigateUri-Eigenschaft an einen Systemhandler für URI-Typen und -Schemas übergeben. Das System startet dann die App, die für das Schema des URIs registriert ist, der für „NavigateUri“ angegeben wird.

Wenn der Link keine Inhalte in einem Standardwebbrowser laden soll (und kein Browser angezeigt werden soll), legen Sie keinen Wert für „NavigateUri“ fest. Behandeln Sie stattdessen das Click-Ereignis, und schreiben Sie Code, der die gewünschte Aktion ausführt.


**Behandelt das Click-Ereignis**

Verwenden Sie das Click-Ereignis für alle Aktionen außer für das Starten eines URIs in einem Browser (also beispielsweise für die Navigation innerhalb der App). Wenn Sie beispielsweise keinen Broswer öffnen, sondern eine neue App-Seite laden möchten, rufen Sie eine [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx)-Methode in Ihrem Click-Ereignishandler auf, um zur neuen App-Seite zu navigieren. Wenn ein externer, absoluter URI in einem [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx)-Steuerelement geladen werden soll, das auch in Ihrer App vorhanden ist, rufen Sie [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx) als Teil Ihrer Klickhandlerlogik auf.

In der Regel behandeln Sie nicht das Click-Ereignis und legen gleichzeitig einen NavigateUri-Wert fest, da dies zwei verschiedene Methoden zur Verwendung des Linkelements darstellen. Wenn Sie den URI im Standardbrowser öffnen möchten und einen Wert für „NavigateUri“ angegeben haben, behandeln Sie das Click-Ereignis nicht. Im umgekehrten Fall geben Sie kein NavigateUri-Element an, wenn Sie das Click-Ereignis behandeln.

Sie können im Click-Ereignishandler nicht verhindern, dass der Standardbrowser ein für „NavigateUri“ angegebenes gültiges Ziel lädt. Die Aktion wird automatisch (asynchron) ausgeführt, wenn der Link aktiviert wird und kann nicht im Click-Ereignishandler abgebrochen werden. 

## <a name="hyperlink-underlines"></a>Unterstreichung von Links
Links sind standardmäßig unterstrichen. Diese Unterstreichung ist wichtig, da dadurch Anforderungen für Barrierefreiheit erfüllt werden. Farbenblinde Benutzer können anhand der Unterstreichung zwischen Links und anderem Text unterscheiden. Wenn Sie die Unterstreichung deaktivieren, sollten Sie eine andere Art der Formatierung in Betracht ziehen (z. B. „FontWeight“ oder „FontStyle“), um Links von anderem Text abzuheben.

**Hyperlink-Text-Elemente**

Sie können die [UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx)-Eigenschaft festlegen, um die Unterstreichung zu deaktivieren. Ziehen Sie in diesem Fall die Verwendung von [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) oder [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx) in Betracht, um Ihren Linktext zu differenzieren.

**HyperlinkButton** 

„HyperlinkButton“ wird standardmäßig als unterstrichener Text angezeigt, wenn Sie eine Zeichenfolge als Wert für die [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx)-Eigenschaft festlegen.

Der Text wird in den folgenden Fällen nicht unterstrichen angezeigt:
- Sie legen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) als Wert für die Content-Eigenschaft und die [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx)-Eigenschaft für „TextBlock“ fest.
- Sie passen die Vorlage für „HyperlinkButton“ an und ändern den Namen des [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx)-Vorlagenteils.

Wenn Sie eine Schaltfläche benötigen, die als nicht unterstrichener Text angezeigt wird, können Sie ein Standard-Schaltflächen-Steuerelement verwenden und die integrierte Systemressource `TextBlockButtonStyle` auf die Style-Eigenschaft anwenden.

## <a name="notes-for-hyperlink-text-element"></a>Hinweise zum Linktextelement

Dieser Abschnitt gilt nur für das Linktextelement, nicht das HyperlinkButton-Steuerelement.

**Eingabeereignisse**

Da es sich bei einem Link nicht um ein [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) handelt, enthält er nicht den Satz von Eingabeereignissen für UI-Elemente wie „Tapped“, „PointerPressed“ usw. Stattdessen enthält ein Link ein eigenes Click-Ereignis sowie das implizite Verhalten des Systems, das jeden als „NavigateUri“ angegebenen URI lädt. Das System verarbeitet alle Eingabeaktionen, die die Linkaktionen aufrufen sollten, und löst als Reaktion das Click-Ereignis aus.

**Inhalt**

Für den Link liegen Einschränkungen in Bezug auf den Inhalt vor, der in der [Inlines](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx)-Sammlung enthalten sein darf. Genauer gesagt: Ein Link lässt nur [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx)- und andere [Span](/uwp/api/windows.ui.xaml.documents.span)-Typen zu, die keinen anderen Link darstellen. [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx) darf nicht in der Inlines-Sammlung eines Links enthalten sein. Beim Versuch, eingeschränkte Inhalte hinzuzufügen, wird eine Ausnahme für ein ungültiges Argument oder eine XAML-Analyseausnahme ausgelöst.

**Verhalten von links und Design-Stil**

Links erben nicht von [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx). Daher enthalten sie keine [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx)- oder [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx)-Eigenschaft. Sie können die von [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx) geerbten Eigenschaften wie „Foreground“ oder „FontFamily“ bearbeiten, um das Erscheinungsbild eines Links zu ändern. Sie können jedoch keinen allgemeinen Stil bzw. keine allgemeine Vorlage zum Anwenden von Änderungen verwenden. Verwenden Sie anstelle einer Vorlage allgemeine Ressourcen für Werte der Linkeigenschaften, um die Konsistenz zu gewährleisten. Einige Linkeigenschaften verwenden Standardwerte aus einem vom System bereitgestellten {ThemeResource}-Markuperweiterungswert. Dadurch kann die Linkdarstellung entsprechend angepasst werden, wenn der Benutzer das Systemdesign während der Laufzeit ändert.

Die Standardfarbe des Links ist die Akzentfarbe des Systems. Dieses Verhalten können Sie mit der [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx)-Eigenschaft außer Kraft setzen.

## <a name="recommendations"></a>Empfehlungen

-   Verwenden Sie Links nur für die Navigation. Verwenden Sie sie nicht für andere Aktionen.
-   Verwenden Sie den Textstil aus dem Typenverlauf für textbasierte Links. Informieren Sie sich über [Schriftarten und den Windows 10-Typenverlauf](../style/typography.md).
-   Separate Links sollten weit genug voneinander platziert werden, damit der Benutzer zwischen ihnen unterscheiden kann und sie mühelos einzeln auswählen kann.
-   Fügen Sie Hyperlinks QuickInfos hinzu, die dem Benutzer anzeigen, wohin er umgeleitet wird. Wenn der Benutzer zu einer externen Website weitergeleitet werden soll, schließen Sie den Namen der Domäne der obersten Ebene in die QuickInfo ein und formatieren den Text mit einer zweiten Schriftfarbe.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [Richtlinien für QuickInfos](tooltips.md)

**Für Entwickler (XAML)**
- [Windows.UI.Xaml.Documents.Hyperlink-Klasse](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Windows.UI.Xaml.Controls.HyperlinkButton-Klasse](https://msdn.microsoft.com/library/windows/apps/br242739)
