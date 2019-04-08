---
Description: Überlegen Sie einmal, wie häufig wir jeden Tag Texte lesen – E-Mails, Büchern, Straßenschilder, Preise auf einer Speisekarte, Reifendruckangaben oder Plakate auf Werbetafeln an der Bushaltestelle.
title: Textsteuerelemente
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
ms.date: 10/01/2018
ms.topic: article
keywords: windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0812e4655b8d817f4318f6bd87f7578ee09d635e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621425"
---
# <a name="text-controls"></a>Textsteuerelemente

Textsteuerelemente bestehen aus Texteingabefeldern, Kennwortfeldern, Feldern mit automatischen Vorschlägen und Textblöcken. Das XAML-Framework stellt mehrere Steuerelemente für die Darstellung, Eingabe und Bearbeitung von Text sowie eine Reihe von Eigenschaften für die Formatierung von Text bereit.

- Für die Anzeige von schreibgeschütztem Text stehen die Steuerelemente [TextBlock](text-block.md) und [RichTextBlock](rich-text-block.md) zur Verfügung.
- Die Steuerelemente für die Texteingabe und Bearbeiten von sind: [Textfeld](text-box.md), [RichEditBox](rich-edit-box.md), [AutoSuggestBox](auto-suggest-box.md), und [PasswordBox](password-box.md).

> **Wichtige APIs:** [TextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [RichTextBlock-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx), [RichEditBox Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx), [AutoSuggestBox Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx), [PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Das zu verwendende Textsteuerelement hängt vom jeweiligen Szenario ab. Anhand der Informationen in diesem Abschnitt können Sie das richtige Textsteuerelement für Ihre App auswählen.

### <a name="render-read-only-text"></a>Rendern von schreibgeschütztem Text

Verwenden Sie **TextBlock** zur Anzeige der überwiegenden Menge an schreibgeschütztem Text in Ihrer App. Sie können es zum Anzeigen von einzeiligem oder mehrzeiligem Text, Inlinelinks und Text mit Formatierung, z. B. fett, kursiv oder unterstrichen, verwenden.

TextBlock ist in der Regel einfacher zu verwenden und bietet eine bessere Leistung beim Rendern von Text als RichTextBlock. Daher wird er in der Regel für App-UI-Text bevorzugt. Sie können über TextBlock in Ihrer App ganz einfach auf den Text zugreifen und ihn verwenden, indem Sie den Wert der [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)-Eigenschaft abrufen.

Er enthält außerdem viele der gleichen Formatierungsoptionen zum Anpassen des Renderns von Text. Sie können zwar Zeilenumbrüche in den Text einfügen, jedoch ist TextBlock zum Anzeigen eines einzelnen Absatzes vorgesehen und unterstützt keinen Texteinzug.

Verwenden Sie **RichTextBlock**, wenn Sie Unterstützung für mehrere Absätze, mehrspaltigen Text, andere komplexe Textlayouts oder Inline-UI-Elemente, z. B. Bilder, benötigen. RichTextBlock bietet mehrere Features für erweitertes Textlayout.

Die Inhaltseigenschaft von RichTextBlock ist die [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx)-Eigenschaft, die über das [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)-Element absatzbasierten Text unterstützt. Es bietet keine **Text**-Eigenschaft, die Sie zum einfachen Zugriff auf den Textinhalt des Steuerelements in Ihrer App verwenden können.  

### <a name="text-input"></a>Texteingabe

Ein **TextBox**-Steuerelement ermöglicht es Benutzern, unformatierten Text einzugeben und zu bearbeiten, z. B. in einem Formular. Mit der [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)-Eigenschaft können Sie den Text in einem TextBox abrufen und festlegen.

Sie können das TextBox-Element als schreibgeschützt festlegen, dies sollte aber nur ein temporärer, bedingter Zustand sein. Wenn der Text nie bearbeitbar sein soll, ziehen Sie stattdessen die Verwendung eines TextBlock-Elements in Erwägung.

Verwenden Sie ein **PasswordBox**-Steuerelement, um Kennwörter oder andere private Daten wie Sozialversicherungsnummern zu erfassen. Bei einem Kennwortfeld handelt es sich um ein Texteingabefeld, das die darin eingegebenen Zeichen zu Datenschutzzwecken verdeckt. Ein Kennwortfeld sieht wie ein Texteingabefeld aus. Die einzige Ausnahme besteht darin, dass anstelle des eingegebenen Texts Aufzählungszeichen erscheinen. Das Aufzählungszeichen ist anpassbar.

Verwenden Sie ein **AutoSuggestBox**-Steuerelement, um eine Liste mit Vorschlägen anzuzeigen, aus der Benutzer während der Eingabe auswählen können. Ein Feld mit automatischen Vorschlägen ist ein Texteingabefeld, das eine Liste grundlegender Suchvorschläge auslöst. Begriffsvorschläge können auf einer Kombination aus häufig verwendeten Suchbegriffen und historischen vom Benutzer eingegebenen Begriffen basieren.

Sie sollten außerdem ein AutoSuggestBox-Steuerelement verwenden, um ein Suchfeld zu implementieren.

Verwenden Sie **RichEditBox** zum Anzeigen und Bearbeiten von Textdateien. Ein RichEditBox-Element wird nicht zum Einfügen von Benutzereingaben in Ihre App verwendet, wie dies bei anderen standardmäßigen Texteingabefeldern der Fall ist. Es wird vielmehr dazu verwendet, mit Textdateien zu arbeiten, die von Ihrer App getrennt sind. Den in ein RichEditBox-Element eingegebenen Text speichern Sie üblicherweise in einer RTF-Datei.

**Gibt die Texteingabe am besten?**

Es bestehen zahlreiche Möglichkeiten, Benutzereingaben in Ihrer App zu erhalten. Diese Fragen helfen Ihnen festzustellen, ob sich eines der Standardeingabefelder oder ein anderes Steuerelement am besten eignet, um Benutzereingaben zu erhalten.

-   **Ist es ratsam, effizient aller gültigen Werte auflisten?** Wenn dies der Fall ist, ziehen Sie die Verwendung eines der Auswahlsteuerelemente in Betracht, wie [Kontrollkästchen](checkbox.md), [Dropdownliste](lists.md), Listenfeld, [Optionsfeld](radio-button.md), [Schieberegler](slider.md), [Umschalter](toggles.md), [Datumsauswahl](date-and-time.md) oder Zeitauswahl.
-   **Gibt es ein relativ kleinen Satz gültiger Werte?** Wenn dies der Fall ist, sollten Sie eine [Einblendliste](lists.md) oder ein Listenfeld in Erwägung ziehen, insbesondere, wenn die Werte viele Zeichen enthalten.
-   **Ist der gültigen Daten vollständig uneingeschränkt? Oder die gültigen Daten nur beschränkt durch Format (eingeschränkte Länge oder Typen mit Zeichen)?** Wenn ja, verwenden Sie einen Texteingabesteuerelement. Sie können die Anzahl der Zeichen, die eingegeben werden können, beschränken, und Sie können das Format in Ihrem App-Code überprüfen.
-   **Stellt der Wert ein Datentyps dar, die eine spezielle Standardsteuerelements?** Ist dies der Fall, verwenden Sie das entsprechende Steuerelement anstelle eines Texteingabesteuerelements. Verwenden Sie anstelle eines Texteingabesteuerelements zum Beispiel [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681), um eine Dateneingabe zu akzeptieren.
-   Wenn die Daten streng numerisch sind:
    -   **Ist der Wert ungefähre und/oder relativ zu einer anderen Menge auf derselben Seite wird eingegeben?** Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) verwenden.
    -   **Profitieren der Benutzer, von sofortiges Feedback zu den Auswirkungen der Einstellung ändert sich?** Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) verwenden, eventuell zusammen mit einem begleitenden Steuerelement.
    -   **Ist der wahrscheinlich eingegebene Wert angepasst werden, nachdem das Ergebnis wird festgestellt, z. B. wie bei einem Volume oder Bildschirm Helligkeit?** Wenn dies der Fall ist, sollten Sie einen [Schieberegler](slider.md) verwenden.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/category/Text">die App zu öffnen und Textsteuerelemente in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Textfeld

![Ein Textfeld](images/text-box.png)

Feld mit automatischen Vorschlägen

![Beispiel für das erweiterte Steuerelement für automatische Vorschläge](images/controls_autosuggest_expanded01.png)

Kennwortfeld

![Kennwortfeld im Fokuszustand bei Texteingabe](images/passwordbox-focus-typing.png)

## <a name="create-a-text-control"></a>Erstellen eines Textsteuerelements

Informationen und Beispiele für jedes Textsteuerelement finden Sie in den folgenden Artikeln.

-   [AutoSuggestBox](auto-suggest-box.md)
-   [PasswordBox-Element](password-box.md)
-   [RichEditBox](rich-edit-box.md)
-   [RichTextBlock](rich-text-block.md)
-   [TextBlock](text-block.md)
-   [TextBox](text-box.md)

## <a name="font-and-style-guidelines"></a>Richtlinien für Schriftart und -schnitt
Richtlinien für Schriftarten finden Sie in den folgenden Artikeln:

- [Richtlinien zur Typografie](../style/typography.md)
- [Liste der Segoe MDL2-Symbol und Richtlinien](../style/segoe-ui-symbol-font.md)

## <a name="pen-input"></a>Stifteingabe

**Gilt für:** Textfeld RichEditBox, AutoSuggestBox

Ab Windows 10, Version 1803, verfügen XAML-Texteingabefelder über eine integrierte Unterstützung für Stifteingaben mit [Windows Ink](../input/pen-and-stylus-interactions.md). Wenn ein Benutzer einen Windows-Stift für Eingaben in ein Textfeld verwendet, wird das Textfeld transformiert, sodass der Benutzer mit einem Stift direkt in das Feld schreiben kann, anstatt einen separaten Eingabebereich öffnen zu müssen.

![Textfeld wird erweitert, wenn mit einem Stift darauf getippt wird](images/handwritingview/handwritingview2.gif)

Weitere Informationen finden Sie unter [Texteingabe, mit der Ansicht Handschrift](text-handwriting-view.md).

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Auswählen der richtigen Tastatur für Ihr Textsteuerelement

**Gilt für:** Textfeld PasswordBox RichEditBox

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.

>Hinweis: Diese Informationen gelten nur für das SIP. Sie gelten nicht für Hardwaretastaturen oder die Bildschirmtastatur, die in den Windows-Optionen für erleichterte Bedienung verfügbar ist.

Die Bildschirmtastatur kann für die Texteingabe verwendet werden, wenn Ihre App auf einem Gerät mit Touchscreen ausgeführt wird. Die Bildschirmtastatur wird aufgerufen, wenn der Benutzer auf ein bearbeitbares Eingabefeld wie etwa ein TextBox oder RichEditBox tippt. Benutzer können Daten in Ihre App schneller und einfacher eingeben, wenn Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet dem System einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen.

Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx)-Eigenschaft auf **Number** fest. Dadurch wird das System angewiesen, die Zehnertastatur anzuzeigen, was dem Benutzer die Eingabe der PIN erleichtert.

>Wichtig  
>Durch diesen Eingabeumfang wird keine Eingabeüberprüfung durchgeführt, und der Benutzer kann Eingaben über eine Hardwaretastatur oder ein anderes Eingabegerät vornehmen. Die Benutzereingabe muss je nach Bedarf trotzdem in Ihrem Code überprüft werden.

Weitere Informationen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/mt280229).

## <a name="color-fonts"></a>Farbige Schriftarten

**Gilt für:** TextBlock-Element RichTextBlock, Textfeld RichEditBox

Windows bietet bei Schriftarten die Möglichkeit, für jede Glyphe mehrere farbige Schichten zu verwenden. Die Segoe UI Emoji-Schriftart definiert z. B. farbige Versionen der Emoticon- und anderer Emoji-Zeichen.

Die Standard- und Richt-Text-Steuerelemente unterstützen die Anzeige von farbigen Schriftarten. Standardmäßig ist die **IsColorFontEnabled**-Eigenschaft **true**, und Schriftarten mit diesen zusätzlichen Schichten werden in Farbe gerendert. Die standardmäßige farbige Schriftart im System ist Segoe UI Emoji, und die Steuerelemente kehren für die farbige Anzeige der Glyphen zu dieser Schriftart zurück.

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

Der gerenderte Text sieht wie folgt aus:

![Textblock mit farbiger Schriftart](images/text-block-color-fonts.png)

Weitere Informationen finden Sie unter der [IsColorFontEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx)-Eigenschaft.

## <a name="guidelines-for-line-and-paragraph-separators"></a>Richtlinien für Zeilen- und Absatztrennzeichen

**Gilt für:** TextBlock-Element RichTextBlock, mehrzeiligen Textfeld RichEditBox

Verwenden Sie das Zeilentrennzeichen (0x2028) und das Absatztrennzeichen (0x2029), um Nur-Text zu trennen. Nach jeder Zeilentrennung wird eine neue Zeile begonnen. Nach jedem Absatztrennzeichen wird ein neuer Absatz begonnen.

Es ist nicht erforderlich, die erste Zeile oder den ersten Absatz in einer Datei mit diesen Zeichen zu beginnen oder die letzte Zeile oder den letzten Absatz damit zu beenden, andernfalls entsteht der Eindruck, an dieser Stelle befinde sich eine leere Zeile oder ein leerer Absatz.

Sie können in Ihrer App das Zeilentrennzeichen verwenden, um ein unbedingtes Zeilenende anzugeben. Zeilentrennzeichen entsprechen jedoch nicht den gesonderten Wagenrücklauf- oder Zeilenvorschubzeichen oder einer Kombination dieser Zeichen. Zeilentrennzeichen müssen getrennt von Wagenrücklauf- und Zeilenvorschubzeichen verarbeitet werden.

Ihre App kann zwischen Textabsätzen ein Absatztrennzeichen einfügen. Die Verwendung dieses Trennzeichens ermöglicht das Erstellen von Nur-Text-Dateien, die mit unterschiedlichen Zeilenhöhen auf verschiedenen Betriebssystemen formatiert werden können. Das Zielsystem kann Zeilentrennzeichen ignorieren und Absätze nur bei den Absatztrennzeichen umbrechen.

## <a name="guidelines-for-spell-checking"></a>Richtlinien für die Rechtschreibprüfung

**Gilt für:** Textfeld RichEditBox

Während der Texteingabe und -bearbeitung wird der Benutzer durch die Rechtschreibprüfung darauf aufmerksam gemacht, dass ein Wort falsch geschrieben ist, indem das Wort mit einer roten Wellenlinie unterstrichen wird. Außerdem wird dem Benutzer eine Korrekturmöglichkeit angeboten.

Hier ist ein Beispiel für die integrierte Rechtschreibprüfung:

![Die integrierte Rechtschreibprüfung](images/spellchecking.png)

Verwenden Sie die Rechtschreibprüfung für Texteingabe-Steuerelemente aus zwei Gründen:

-   **Um die automatische Korrektur von Rechtschreibfehlern**

    Das Rechtschreibprüfungsmodul korrigiert falsch geschriebene Wörter automatisch, wenn die Korrektur zweifelsfrei feststeht. Beispielsweise ändert das Modul "dsa" automatisch in "das".

-   **Alternative Schreibweisen angezeigt.**

    Wenn dem Rechtschreibprüfungsmodul keine zweifelsfreien Korrekturen zur Verfügung stehen, wird unter dem falsch geschriebenen Wort eine rote Linie eingefügt, und es werden Alternativen in einem Kontextmenü angezeigt, wenn Sie auf das Wort tippen oder mit der rechten Maustaste klicken.

-   Verwenden Sie die Rechtschreibprüfung, um Benutzer bei der Eingabe von Wörtern oder Sätzen in die Texteingabe-Steuerelemente zu unterstützen. Die Rechtschreibprüfung funktioniert bei Touch-, Maus- und Tastatureingaben.
-   Verwenden Sie die Rechtschreibprüfung nicht, wenn ein Wort mit hoher Wahrscheinlichkeit nicht in einem Wörterbuch enthalten ist oder wenn Benutzer die Rechtschreibprüfung nicht als nützlich empfinden. Sie sollten sie beispielsweise nicht aktivieren, wenn das Textfeld für eine Telefonnummer oder einen Namen vorgesehen ist.
-   Deaktivieren Sie die Rechtschreibprüfung nicht nur deswegen, weil das aktuelle Rechtschreibprüfungsmodul die Sprache Ihrer App nicht unterstützt. Wenn die Rechtschreibprüfung eine Sprache nicht unterstützt, passiert nichts, sodass die Option bedenkenlos aktiviert bleiben kann. Außerdem verfügen einige Benutzer eventuell über einen Eingabemethoden-Editor (Input Method Editor, IME), um eine andere Sprache in Ihre App einzugeben, die unterstützt wird. Deaktivieren Sie, wenn Sie beispielsweise eine japanische Sprachen-App erstellen, nicht die Rechtschreibprüfung, selbst wenn das Rechtschreibprüfungsmodul diese Sprache möglicherweise aktuell nicht erkennt. Der Benutzer könnte zu einem englischsprachigen IME wechseln und englischsprachige Eingaben in der App vornehmen. Wenn die Rechtschreibprüfung aktiviert ist, werden die englischsprachigen Eingaben geprüft.

Für TextBox- und RichEditBox-Steuerelemente ist die Rechtschreibprüfung standardmäßig aktiviert. Sie können sie durch Festlegen der **IsSpellCheckEnabled**-Eigenschaft auf **false** deaktivieren.

## <a name="related-articles"></a>Verwandte Artikel

**Für Designer**
- [Richtlinien zur Typografie](../style/typography.md)
- [Liste der Segoe MDL2-Symbol und Richtlinien](../style/segoe-ui-symbol-font.md)
- [Hinzufügen von Suchfunktionen](https://msdn.microsoft.com/library/windows/apps/hh465231)

**Für Entwickler (XAML)**
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length-Eigenschaft](https://msdn.microsoft.com/library/system.string.length.aspx)
