---
Description: Über Hyperlinks können Benutzer zu einem anderen Teil der App oder zu einer anderen App navigieren oder mit einer separaten Browser-App einen bestimmten URI (Uniform Resource Identifier) starten.
title: Hyperlinks
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 471e17199d6a1858625ce418ea756d79cf8600f9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160134"
---
# <a name="hyperlinks"></a>Hyperlinks

Über Hyperlinks können Benutzer zu einem anderen Teil der App oder zu einer anderen App navigieren oder mit einer separaten Browser-App einen bestimmten URI (Uniform Resource Identifier) starten. Sie haben zwei Möglichkeiten, einer XAML-App einen Link hinzuzufügen: über das Textelement **Link** oder das Steuerelement **HyperlinkButton**.

> **Plattform-APIs:** [Textelement „Hyperlink“](/uwp/api/Windows.UI.Xaml.Documents.Hyperlink), [Steuerelement „HyperlinkButton“](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)

![Eine Linkschaltfläche](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie einen Link, wenn Text erforderlich ist, der bei Auswahl reagiert und der weitere Informationen zum ausgewählten Text aufruft.

Wählen Sie den richtigen Linktyp basierend auf Ihren Anforderungen:

-   Verwenden Sie innerhalb eines Textsteuerelements ein Inline-**Link**textelement. Ein Linkelement wird mit anderen Textelementen umgebrochen, und Sie können es in einem beliebigen InlineCollection-Element verwenden. Verwenden Sie einen Textlink, wenn Sie automatischen Textumbruch nutzen möchten und nicht unbedingt ein großes Tippziel benötigen. Der Linktext kann klein sein und sich nur schwer auswählen lassen, insbesondere bei der Toucheingabe.
-   Verwenden Sie ein **HyperlinkButton**-Element für eigenständige Links. Ein HyperlinkButton-Element ist ein spezielles Schaltflächen-Steuerelement, das Sie überall dort verwenden können, wo Sie eine Schaltfläche verwenden würden.
-   Verwenden Sie ein **HyperlinkButton**-Element mit einem [Bild](/uwp/api/windows.ui.xaml.controls.image)als Inhalt, um ein klickbares Bild zu erstellen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Falls die App <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> installiert ist, klicke <a href="xamlcontrolsgallery:/item/HyperlinkButton">hier</a>, um die App zu öffnen und „HyperlinkButton“ in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>Erstellen eines Linktextelements

In diesem Beispiel wird veranschaulicht, wie Sie ein Linktextelement in einem [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) verwenden.

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

> **Tipp:** &nbsp;&nbsp;Wenn du einen Link in einem Textsteuerelement mit anderen Textelementen in XAML verwendest, platziere den Inhalt in einem [Span](/uwp/api/windows.ui.xaml.documents.span)-Container, and wende das Attribut `xml:space="preserve"` darauf an, damit die Leerstelle zwischen dem Link und anderen Elementen erhalten bleibt.

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

Dieses Beispiel zeigt die Erstellung eines HyperlinkButton-Elements im Code.

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>Handhaben der Navigation

Die Navigation wird bei beiden Linktypen gleich gehandhabt. Sie können die Eigenschaft **NavigateUri** festlegen oder das **Click**-Ereignis behandeln.

**Navigieren zu einem URI**

Wenn Sie mit dem Link zu einem URI navigieren möchten, legen Sie die NavigateUri-Eigenschaft fest. Wenn ein Benutzer auf den Link klickt oder tippt, wird der angegebene URI im Standardbrowser geöffnet. Der Standardbrowser wird in einem separaten Prozess von Ihrer App ausgeführt.

> [!NOTE]
> Ein URI wird durch die Klasse [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) dargestellt. Da diese Klasse bei der Programmierung mit .NET verborgen ist, sollte die Klasse [System.Uri](/dotnet/api/system.uri) verwendet werden. Weitere Informationen findest du auf den Referenzseiten für diese Klassen.

Du musst nicht das Schema **http:** oder **https:** verwenden. Stattdessen kannst du Schemas wie **ms-appx:** , **ms-appdata:** oder **ms-resources:** verwenden, falls dort Ressourceninhalte vorhanden sind, die in einem Browser geladen werden können. Das Schema **file:** ist allerdings ausdrücklich blockiert. Weitere Informationen finden Sie unter [URI-Schemas](/previous-versions/windows/apps/jj655406(v=win.10)).

Wenn ein Benutzer auf den Link klickt, wird der Wert der NavigateUri-Eigenschaft an einen Systemhandler für URI-Typen und -Schemas übergeben. Das System startet dann die App, die für das Schema des URIs registriert ist, der für „NavigateUri“ angegeben wird.

Wenn der Link keine Inhalte in einem Standardwebbrowser laden soll (und kein Browser angezeigt werden soll), legen Sie keinen Wert für „NavigateUri“ fest. Behandeln Sie stattdessen das Click-Ereignis, und schreiben Sie Code, der die gewünschte Aktion ausführt.


**Behandeln des Click-Ereignisses**

Verwenden Sie das Click-Ereignis für alle Aktionen außer für das Starten eines URIs in einem Browser (also beispielsweise für die Navigation innerhalb der App). Wenn Sie beispielsweise keinen Broswer öffnen, sondern eine neue App-Seite laden möchten, rufen Sie eine [Frame.Navigate](/uwp/api/windows.ui.xaml.controls.frame.navigate)-Methode in Ihrem Click-Ereignishandler auf, um zur neuen App-Seite zu navigieren. Wenn ein externer, absoluter URI in einem [WebView](/uwp/api/windows.ui.xaml.controls.webview)-Steuerelement geladen werden soll, das auch in Ihrer App vorhanden ist, rufen Sie [WebView.Navigate](/uwp/api/windows.ui.xaml.controls.webview.navigate) als Teil Ihrer Klickhandlerlogik auf.

In der Regel behandeln Sie nicht das Click-Ereignis und legen gleichzeitig einen NavigateUri-Wert fest, da dies zwei verschiedene Methoden zur Verwendung des Linkelements darstellen. Wenn Sie den URI im Standardbrowser öffnen möchten und einen Wert für „NavigateUri“ angegeben haben, behandeln Sie das Click-Ereignis nicht. Im umgekehrten Fall geben Sie kein NavigateUri-Element an, wenn Sie das Click-Ereignis behandeln.

Sie können im Click-Ereignishandler nicht verhindern, dass der Standardbrowser ein für „NavigateUri“ angegebenes gültiges Ziel lädt. Die Aktion wird automatisch (asynchron) ausgeführt, wenn der Link aktiviert wird und kann nicht im Click-Ereignishandler abgebrochen werden. 

## <a name="hyperlink-underlines"></a>Unterstreichung von Links
Links sind standardmäßig unterstrichen. Diese Unterstreichung ist wichtig, da dadurch Anforderungen für Barrierefreiheit erfüllt werden. Farbenblinde Benutzer können anhand der Unterstreichung zwischen Links und anderem Text unterscheiden. Wenn Sie die Unterstreichung deaktivieren, sollten Sie eine andere Art der Formatierung in Betracht ziehen (z. B. „FontWeight“ oder „FontStyle“), um Links von anderem Text abzuheben.

**Linktextelemente**

Sie können die [UnderlineStyle](/uwp/api/windows.ui.xaml.documents.hyperlink.underlinestyle)-Eigenschaft festlegen, um die Unterstreichung zu deaktivieren. Ziehen Sie in diesem Fall die Verwendung von [FontWeight](/uwp/api/windows.ui.xaml.documents.textelement.fontweight) oder [FontStyle](/uwp/api/windows.ui.xaml.documents.textelement.fontstyle) in Betracht, um Ihren Linktext zu differenzieren.

**HyperlinkButton** 

„HyperlinkButton“ wird standardmäßig als unterstrichener Text angezeigt, wenn Sie eine Zeichenfolge als Wert für die [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)-Eigenschaft festlegen.

Der Text wird in den folgenden Fällen nicht unterstrichen angezeigt:
- Sie legen [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) als Wert für die Content-Eigenschaft und die [Text](/uwp/api/windows.ui.xaml.controls.textblock.text)-Eigenschaft für „TextBlock“ fest.
- Sie passen die Vorlage für „HyperlinkButton“ an und ändern den Namen des [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)-Vorlagenteils.

Wenn Sie eine Schaltfläche benötigen, die als nicht unterstrichener Text angezeigt wird, können Sie ein Standard-Schaltflächen-Steuerelement verwenden und die integrierte Systemressource `TextBlockButtonStyle` auf die Style-Eigenschaft anwenden.

## <a name="notes-for-hyperlink-text-element"></a>Hinweise zum Linktextelement

Dieser Abschnitt gilt nur für das Linktextelement, nicht das HyperlinkButton-Steuerelement.

**Eingabeereignisse**

Da es sich bei einem Link nicht um ein [UIElement](/uwp/api/windows.ui.xaml.uielement) handelt, enthält er nicht den Satz von Eingabeereignissen für UI-Elemente wie „Tapped“, „PointerPressed“ usw. Stattdessen enthält ein Link ein eigenes Click-Ereignis sowie das implizite Verhalten des Systems, das jeden als „NavigateUri“ angegebenen URI lädt. Das System verarbeitet alle Eingabeaktionen, die die Linkaktionen aufrufen sollten, und löst als Reaktion das Click-Ereignis aus.

**Inhalt**

Für den Link liegen Einschränkungen in Bezug auf den Inhalt vor, der in der [Inlines](/uwp/api/windows.ui.xaml.documents.span.inlines)-Sammlung enthalten sein darf. Genauer gesagt: Ein Link lässt nur [Run](/uwp/api/windows.ui.xaml.documents.run)- und andere [Span](/uwp/api/windows.ui.xaml.documents.span)-Typen zu, die keinen anderen Link darstellen. [InlineUIContainer](/uwp/api/windows.ui.xaml.documents.inlineuicontainer) darf nicht in der Inlines-Sammlung eines Links enthalten sein. Beim Versuch, eingeschränkte Inhalte hinzuzufügen, wird eine Ausnahme für ein ungültiges Argument oder eine XAML-Analyseausnahme ausgelöst.

**Links und Design-/Stilverhalten**

Links erben nicht von [Control](/uwp/api/windows.ui.xaml.controls.control). Daher enthalten sie keine [Style](/uwp/api/windows.ui.xaml.frameworkelement.style)- oder [Template](/uwp/api/windows.ui.xaml.controls.control.template)-Eigenschaft. Sie können die von [TextElement](/uwp/api/windows.ui.xaml.documents.textelement) geerbten Eigenschaften wie „Foreground“ oder „FontFamily“ bearbeiten, um das Erscheinungsbild eines Links zu ändern. Sie können jedoch keinen allgemeinen Stil bzw. keine allgemeine Vorlage zum Anwenden von Änderungen verwenden. Verwenden Sie anstelle einer Vorlage allgemeine Ressourcen für Werte der Linkeigenschaften, um die Konsistenz zu gewährleisten. Einige Linkeigenschaften verwenden Standardwerte aus einem vom System bereitgestellten {ThemeResource}-Markuperweiterungswert. Dadurch kann die Linkdarstellung entsprechend angepasst werden, wenn der Benutzer das Systemdesign während der Laufzeit ändert.

Die Standardfarbe des Links ist die Akzentfarbe des Systems. Dieses Verhalten können Sie mit der [Foreground](/uwp/api/windows.ui.xaml.documents.textelement.foreground)-Eigenschaft außer Kraft setzen.

## <a name="recommendations"></a>Empfehlungen

-   Verwenden Sie Links nur für die Navigation. Verwenden Sie sie nicht für andere Aktionen.
-   Verwenden Sie den Textstil aus dem Typenverlauf für textbasierte Links. Informationen zu Schriftarten und zum Windows 10-Typenverlauf findest du [hier](../style/typography.md).
-   Separate Links sollten weit genug voneinander platziert werden, damit der Benutzer zwischen ihnen unterscheiden kann und sie mühelos einzeln auswählen kann.
-   Fügen Sie Hyperlinks QuickInfos hinzu, die dem Benutzer anzeigen, wohin er umgeleitet wird. Wenn der Benutzer zu einer externen Website weitergeleitet werden soll, schließen Sie den Namen der Domäne der obersten Ebene in die QuickInfo ein und formatieren den Text mit einer zweiten Schriftfarbe.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Textsteuerelemente](text-controls.md)
- [QuickInfos](tooltips.md)

**Für Entwickler (XAML)**
- [Klasse „Windows.UI.Xaml.Documents.Hyperlink“](/uwp/api/Windows.UI.Xaml.Documents.Hyperlink)
- [Klasse „Windows.UI.Xaml.Controls.HyperlinkButton“](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)