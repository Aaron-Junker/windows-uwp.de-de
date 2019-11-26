---
Description: In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Anforderungen für barrierefreien Text
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8b566b1960435a856b82a7be605ef5e1f7ba86e2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257793"
---
# <a name="accessible-text-requirements"></a>Anforderungen für barrierefreien Text  




In diesem Thema werden die bewährten Methoden für barrierefreien Text in Apps beschrieben. Damit stellen Sie sicher, dass der Kontrast zwischen Farben und Hintergründen ausreichend hoch ist. Außerdem werden in diesem Thema die Rollen der Microsoft-Benutzeroberflächenautomatisierung, die für Textelemente in einer App der universellen Windows-Plattform (UWP) verwendet werden können, sowie bewährte Methoden für Text in Grafiken erläutert.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Kontrastverhältnisse  
Obwohl Benutzer immer die Möglichkeit haben, die Anzeige auf einen Modus mit hohem Kontrast umzuschalten, sollten Sie diese Option in Ihrem App-Design für Text möglichst nicht verwenden. Stellen Sie stattdessen sicher, dass der App-Text bestimmte Richtlinien für die Kontraststufe zwischen Text und seinem Hintergrund erfüllt. Die Bewertung der Kontraststufe basiert auf deterministischen Techniken, bei denen der Farbton nicht berücksichtigt wird. Wenn Sie z. B. roten Text auf grünem Hintergrund haben, ist dieser Text für einen farbenblinden Benutzer möglicherweise nicht lesbar. Durch eine Überprüfung und Korrektur des Kontrastverhältnisses können Sie solche Probleme bezüglich der Barrierefreiheit vermeiden.

Die hier dokumentierten Empfehlungen für den Textkontrast basieren auf einem Standard für Barrierefreiheit im Internet, [G18: Sicherstellen eines Kontrastverhältnisses von mindestens 4,5:1 zwischen Text (und Textbildern) und dem Texthintergrund](https://www.w3.org/TR/WCAG20-TECHS/G18.html). Dieser Leitfaden ist in der *W3C-Spezifikation für die Techniken für WCAG 2.0* enthalten.

Damit sichtbarer Text als barrierefreier Text gilt, muss er ein Leuchtdichte-Kontrastverhältnis von mindestens 4,5:1 bezogen auf den Hintergrund aufweisen. Ausnahmen sind Logos und nebensächlicher Text (z. B. Text, der Teil einer inaktiven Benutzeroberflächenkomponente ist).

Für dekorativen Text, der keine Informationen liefert, gilt diese Anforderung nicht. Wenn beispielsweise zufällige Wörter zum Erstellen eines Hintergrunds verwendet werden und die Wörter ohne Bedeutungsänderung neu angeordnet oder ersetzt werden können, gelten die Wörter als dekorative Elemente und müssen dieses Kriterium nicht erfüllen.

Überprüfen Sie mit den Farbkontrasttools, ob das Kontrastverhältnis von sichtbarem Text in Ordnung ist. Tools zum Testen des Kontrastverhältnisses finden Sie unter [Techniken für WCAG 2.0 G18 (Abschnitt Ressourcen)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Einige der unter den Techniken für WCAG 2.0 G18 aufgeführten Tools können bei einer UWP-App nicht interaktiv verwendet werden. Sie müssen möglicherweise die Werte für Vordergrund- und Hintergrundfarben manuell in das Tool eingeben oder Bildschirmaufnahmen der App-UI erstellen und anschließend das Kontrastverhältnistool für das aufgenommene Bild ausführen.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Textelementrollen  
In einer UWP-App können die folgenden Standardelemente (meist als *Textelemente* oder *textedit-Steuerelemente* bezeichnet) verwendet werden:

* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock): Rolle ist [ **Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox): Rolle ist " [ **Edit** "](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**Richtextblock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (und Überlauf Klasse [**richtextblockoverflow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)): Rolle ist [**Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**Richeditbox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox): Rolle ist [ **Edit**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

Wenn ein Steuerelement meldet, dass ihm die Rolle [**Edit**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) zugewiesen ist, gehen Hilfstechnologien davon aus, dass Benutzer die Werte ändern können. Falls Sie also statischen Text in einem [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) platzieren, wird die Rolle falsch gemeldet, wodurch auch die Struktur der App falsch für einen Benutzer gemeldet wird, der Barrierefreiheitsfeatures verwendet.

In den Textmodellen für XAML sind zwei Elemente enthalten, die hauptsächlich für statischen Text, [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elemente und [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)-Elemente verwendet werden. Bei keinem dieser Elemente handelt es sich um eine [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)-Unterklasse, sodass sie auch nicht den Tastaturfokus erhalten oder in der Aktivierreihenfolge erscheinen können. Dies bedeutet jedoch nicht, dass sie von Hilfstechnologien nicht gelesen werden bzw. gelesen werden können. Sprachausgaben sind in der Regel für die Unterstützung mehrerer Modi zum Ausgeben der Inhalte einer App ausgelegt und verfügen beispielsweise über einen dedizierten Lesemodus oder Navigationsmuster, die über den Fokus und die Aktivierreihenfolge hinausgehen, z. B. einen „virtuellen Cursor“. Fügen Sie Ihren statischen Text also nicht nur deswegen in Container ein, die den Fokus erhalten können, damit Benutzer über die Aktivierreihenfolge darauf zugreifen können. Benutzer von Hilfstechnologien erwarten, dass über die Aktivierreihenfolge verfügbare Inhalte interaktiv sind. Wenn sie darin auf statischen Text treffen, ist dies eher verwirrend als hilfreich. Testen Sie dies selbst mit der Sprachausgabe, um einen Eindruck der Benutzeroberfläche Ihrer App zu gewinnen, wenn eine Sprachausgabe zum Untersuchen des statischen Texts der App verwendet wird.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Barrierefreiheitsfunktion „Automatischer Vorschlag“  
Wenn ein Benutzer etwas in ein Eingabefeld tippt und eine Liste möglicher Vorschläge eingeblendet wird, wird dieses Szenario als „Automatischer Vorschlag“ bezeichnet. Sie finden diese Funktion häufig in der Zeile **An:** eines E-Mail-Felds, im Suchfeld von Cortana in Windows, im URL-Eingabefeld in Microsoft Edge, im Feld zur Eingabe des Standorts in der Wetter-App usw. Bei Verwendung von [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) in XAML oder von systeminternen HTML-Steuerelementen ist diese Funktionalität bereits standardmäßig integriert. Um die Funktion zur Verfügung zu stellen, müssen das Eingabefeld und die Liste verknüpft werden. Im Abschnitt [Implementieren von „Automatischer Vorschlag“](#implementing_auto-suggest) finden Sie eine Erläuterung.

Die Sprachausgabe wurde aktualisiert und stellt diese Funktion nun mit einem speziellen Vorschlagsmodus bereit. Wenn Bearbeitungsfeld und Liste ordnungsgemäß verknüpft sind, haben Endbenutzer im Allgemeinen folgende Möglichkeiten:

* Sie wissen, dass die Liste vorhanden ist und wann sie geschlossen wird.
* Sie wissen, wie viele Vorschläge verfügbar sind.
* Sie kennen das ausgewählte Element, falls vorhanden.
* Sie können den Fokus der Sprachausgabe in die Liste verschieben.
* Sie können mit allen anderen Lesemodi durch einen Vorschlag navigieren.

![Vorschlagsliste](images/autosuggest-list.png)<br/>
_Beispiel für eine Vorschlagsliste_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implementieren von „Automatischer Vorschlag“  
Für den Zugriff auf diese Funktion müssen das Eingabefeld und die Liste in der UIA-Struktur einander zugeordnet werden. Die Zuordnung wird über die [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017)-Eigenschaft in Desktop-Apps oder über die [ControlledPeers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)-Eigenschaft in UWP-Apps hergestellt.

„Automatischer Vorschlag“ kann im Allgemeinen auf zwei Weisen genutzt werden.

**Standardauswahl**  
Wenn in der Liste eine Standardauswahl getroffen wird, sucht die Sprachausgabe ein [**UIA_SelectionItem_ElementSelectedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids)-Ereignis in einer Desktop-App oder das [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents)-Ereignis, um es in einer UWP-App auszulösen. Jedes Mal, wenn sich die Auswahl ändert, wenn der Benutzer einen weiteren Buchstaben eingibt und die Vorschläge aktualisiert wurden oder wenn ein Benutzer durch die Liste navigiert, sollte das **ElementSelected**-Ereignis ausgelöst werden.

![Liste mit einer Standardauswahl](images/autosuggest-default-selection.png)<br/>
_Beispiel für eine Standardauswahl_

**Keine Standardauswahl**  
Wenn keine Standardauswahl vorhanden ist, beispielsweise im Standortfeld der Wetter-App, sucht die Sprachausgabe das [**UIA_LayoutInvalidatedEventId**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-event-ids)-Desktopereignis oder das [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents)-UWP-Ereignis, das bei jeder Listenaktualisierung für die Liste ausgelöst werden muss.

![Liste ohne Standardauswahl](images/autosuggest-no-default-selection.png)<br/>
_Beispiel, bei dem keine Standardauswahl vorhanden ist_

### <a name="xaml-implementation"></a>XAML-Implementierung  
Wenn Sie die standardmäßige [**AutosuggestBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)-Klasse in XAML verwenden, ist diese Funktionalität bereits integriert. Wenn Sie mithilfe von [**TextBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox) und einer Liste eine eigene Funktion für automatische Vorschläge erstellen, müssen Sie die Liste für [TextBox**auf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)AutomationProperties.ControlledPeers festlegen. Sie müssen das **AutomationPropertyChanged**-Ereignis jedes Mal für die [**ControlledPeers**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers)-Eigenschaft auslösen, wenn Sie diese Eigenschaft hinzufügen oder entfernen, und abhängig vom verwendeten Szenario auch Ihre eigenen [**SelectionItemPatternOnElementSelected**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents)-Ereignisse oder [**LayoutInvalidated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationevents)-Ereignisse auslösen, wie zuvor in diesem Artikel erläutert.

### <a name="html-implementation"></a>HTML-Implementierung  
Wenn Sie die systemeigenen Steuerelemente in HTML verwenden, wurde die UIA-Implementierung bereits für Sie zugeordnet. Im Folgenden ein Beispiel für eine Implementierung, die bereits für Sie integriert wurde:

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Wenn Sie eigene Steuerelemente erstellen, müssen Sie eigene ARIA-Steuerelemente einrichten, die in den W3C-Standards beschrieben sind.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Text in Grafiken

Verwenden Sie nach Möglichkeit keinen Text in Grafiken. Text, den Sie der in der App als [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)-Element angezeigten Bildquelldatei hinzufügen, ist z. B. nicht automatisch barrierefrei oder für Hilfstechnologien lesbar. Falls Sie Text in Grafiken verwenden müssen, müssen Sie sicherstellen, dass der [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name)-Wert, den Sie als Entsprechung für „alternativen Text“ angeben, diesen Text oder eine Zusammenfassung seiner Bedeutung enthält. Ähnliches gilt auch, wenn Sie Textzeichen anhand von Vektoren als Teil einer [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path)-Klasse oder mit [**Glyphs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Glyphs) erstellen.

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>Text Schriftgröße und-Skala

Benutzer können Schwierigkeiten beim Lesen von Text in einer APP haben, wenn die Schriftarten einfach zu klein sind. Stellen Sie daher sicher, dass jeder Text in Ihrer Anwendung eine sinnvolle Größe ist.

Nachdem Sie den offensichtlichen Vorgang durchgeführt haben, enthält Windows verschiedene Tools und Einstellungen für die Barrierefreiheit, die Benutzer nutzen und ihren eigenen Anforderungen und Vorlieben zum Lesen von Text anpassen können. Dazu zählen:

* Das Bildschirm Vergrößerungs Tool, das einen ausgewählten Bereich der Benutzeroberfläche vergrößert. Sie sollten sicherstellen, dass das Layout des Texts in der APP die Verwendung der Bildschirmlupe nicht erschwert.
* Globale Skalierungs-und Auflösungseinstellungen in **Einstellungen-> System > Anzeige > Skalierung und Layout**. Welche Größen Anpassungsoptionen verfügbar sind, kann variieren, je nachdem, welche Möglichkeiten das Anzeigegerät hat.
* Einstellungen für die Textgröße in den **Einstellungen-> einfache Zugriffs > Anzeige**. Passen Sie die Einstellung **Text vergrößern** an, um nur die Textgröße in unterstützenden Steuerelementen für alle Anwendungen und Bildschirme anzugeben (alle UWP-Text Steuerelemente unterstützen die Text Skalierung ohne Anpassung oder Vorlagen). 
> [!NOTE]
> Mit der Einstellung **alles erstellen** kann ein Benutzer seine bevorzugte Größe für Text und apps im Allgemeinen nur auf dem primären Bildschirm angeben.

Verschiedene Textelemente und Steuerelemente verfügen über eine [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled)-Eigenschaft. Diese Eigenschaft weist standardmäßig den Wert **true** auf. Wenn **true**, kann die Textgröße in diesem Element skaliert werden. Die Skalierung wirkt sich auf Text aus, der einen kleinen **FontSize** -Wert aufweist, als er sich auf Text auswirkt, der über eine große **FontSize**verfügt. Sie können die automatische Größe ändern, indem Sie die Eigenschaft **istextscalefactoriable** eines Elements auf **false**festlegen. 

Weitere Informationen finden Sie unter [Text Skalierung](https://docs.microsoft.com/windows/uwp/design/input/text-scaling) .

Fügen Sie der APP das folgende Markup hinzu, und führen Sie es aus. Passen Sie die Einstellung für die **Textgröße** an, und sehen Sie, was mit den einzelnen **TextBlock**geschieht.

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Es wird nicht empfohlen, die Text Skalierung zu deaktivieren, da der Benutzeroberflächen Text in allen apps universell skaliert werden kann.

Sie können auch das [**TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)-Ereignis und die [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor)-Eigenschaft verwenden, um Informationen zu Änderungen der Einstellung **Textgröße** auf dem Smartphone zu erhalten. Gehen Sie wie folgt vor:

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

Der Wert von **textscalefactor** ist ein Double im Bereich \[1, 2,25\]. Der kleinste Text wird um diesen Wert vergrößert. Unter Umständen können Sie den Wert verwenden, um beispielsweise Grafiken passend zum Text zu skalieren. Denken Sie jedoch daran, dass nicht der gesamte Text um denselben Faktor skaliert wird. Allgemein gilt: Je größer der Text, desto geringer sind die Auswirkungen der Skalierung.

Diese Typen verfügen über eine **IsTextScaleFactorEnabled**-Eigenschaft:  
* [**ContentPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**Steuer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) Element und abgeleitete Klassen
* [**Fonticon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**Textelements**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.TextElement) und abgeleitete Klassen

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen  

* [Text Skalierung](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [Bedienungshilfen](accessibility.md)
* [Grundlegende Informationen zur Barrierefreiheit](basic-accessibility-information.md)
* [Beispiel für XAML-Textanzeige](https://code.msdn.microsoft.com/windowsapps/XAML-text-display-sample-2593ba0a)
* [Beispiel für XAML-Textbearbeitung](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
* [XAML-Barrierefreiheits Beispiel](https://code.msdn.microsoft.com/windowsapps/XAML-accessibility-sample-d63e820d) 
