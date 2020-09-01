---
Description: Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: Tastatur, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: e6e140a1967ca3ffe7775f427ccae7a7e07c5ca6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165814"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.

### <a name="important-apis"></a>Wichtige APIs
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


Die Bildschirmtastatur kann für die Texteingabe verwendet werden, wenn Ihre App auf einem Gerät mit Touchscreen ausgeführt wird. Die Fingereingabe Tastatur wird aufgerufen, wenn der Benutzer auf ein bearbeitbares Eingabefeld (z. b. ein **[Textfeld](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** oder **[richeditbox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)**) tippt. Sie können es Benutzern viel schneller und einfacher machen, Daten in Ihre APP einzugeben, indem Sie den *Eingabebereich* des Text-Steuer Elements so festlegen, dass es mit der Art der Daten, die Sie für den Benutzer erwarten, entspricht. Der Eingabeumfang bietet dem System einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen.

Wenn z. b. ein Textfeld nur für die Eingabe einer vierstelligen PIN verwendet wird, legen Sie die [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) -Eigenschaft auf **Number**fest. Dadurch wird das System angewiesen, die Zehnertastatur anzuzeigen, was dem Benutzer die Eingabe der PIN erleichtert.

> [!IMPORTANT]
> - Diese Informationen gelten nur für das SIP. Sie gelten nicht für Hardwaretastaturen oder die Bildschirmtastatur, die in den Windows-Optionen für erleichterte Bedienung verfügbar ist.
> - Durch diesen Eingabeumfang wird keine Eingabeüberprüfung durchgeführt, und der Benutzer kann Eingaben über eine Hardwaretastatur oder ein anderes Eingabegerät vornehmen. Die Benutzereingabe muss je nach Bedarf trotzdem in Ihrem Code überprüft werden.

## <a name="changing-the-input-scope-of-a-text-control"></a>Ändern des Eingabeumfangs eines Textsteuerelements

Die in einer App verfügbaren Eingabeumfangoptionen sind Member der **[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)**-Enumeration. Sie können die **InputScope**-Eigenschaft eines **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)**- oder **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)**-Elements auf einen dieser Werte festlegen.

> [!IMPORTANT]
> Die **[InputScope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** -Eigenschaft in **[PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** unterstützt nur die **Kennwort** -und **numericpin** -Werte. Andere Werte werden ignoriert.

In diesem Verfahren ändern Sie den Eingabeumfang von mehreren Textfeldern, um ihn auf die erwarteten Daten für jedes Textfeld abzustimmen.

**Ändern des Eingabeumfangs in XAML**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten.
2.  Fügen Sie dem Tag das [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)-Attribut hinzu, und geben Sie den [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)-Wert für die erwartete Eingabe an.

    Im Anschluss sehen Sie einige Textfelder, wie sie etwa in einem allgemeinen Kundenkontaktformular enthalten sein können. Durch Festlegen von [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) wird für jedes Textfeld eine Bildschirmtastatur mit geeigneten Layout für die jeweiligen Daten angezeigt.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Ändern des Eingabeumfangs im Code**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten. Ist es nicht festgelegt, legen Sie das [x:Name-Attribut](../../xaml-platform/x-name-attribute.md) fest, um im Code auf das Steuerelement verweisen zu können.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instanziieren Sie ein neues [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekt.

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Instanziieren Sie ein neues [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekt.
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Legen Sie die [**NameValue**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue)-Eigenschaft des [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekts auf den Wert der [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)-Enumeration fest.

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Fügen Sie das [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekt zur [**Names**](/uwp/api/windows.ui.xaml.input.inputscope.names)-Sammlung des [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekts hinzu.

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Legen Sie das [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekt als Wert für die [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)-Eigenschaft des Textsteuerelements fest.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Hier sehen Sie den vollständigen Code:

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

Die gleichen Schritte können zu folgendem Code zusammengefasst werden:

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>Textvorhersage, Rechtschreibprüfung und Autokorrektur

Die Steuerelemente [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) und [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) verfügen über mehrere Eigenschaften, die das Verhalten des SIP beeinflussen. Sie müssen wissen, wie sich diese Eigenschaften auf die touchbasierte Texteingabe auswirken, um Ihren Benutzer die bestmögliche Benutzererfahrung bieten zu können.

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled): Wenn die Rechtschreibprüfung für ein Textsteuerelement aktiviert ist, interagiert das Steuerelement mit der Rechtschreibprüfung des Systems, um nicht erkannte Wörter zu markieren. Durch Tippen auf ein Wort können Sie eine Liste mit Korrekturvorschlägen anzeigen. Die Rechtschreibprüfung ist standardmäßig aktiviert.

    Bei Verwendung des Eingabeumfangs **Default** ermöglicht diese Eigenschaft auch die automatische Großschreibung des ersten Worts in einem Satz sowie eine Autokorrektur während der Eingabe. Diese Features für die automatische Korrektur sind in anderen Eingabeumfängen möglicherweise deaktiviert. Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled): Wenn die Textvorhersage für ein Textsteuerelement aktiviert ist, zeigt das System eine Liste mit Wörtern an, die Sie vermutlich eingeben möchten. Sie können aus der Liste auswählen, sodass Sie nicht das gesamte Wort eingeben müssen. Die Textvorhersage ist standardmäßig aktiviert.

    Die Textvorhersage ist möglicherweise deaktiviert, wenn der Eingabeumfang nicht auf **Default** festgelegt ist (auch wenn die [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)-Eigenschaft auf **true** festgelegt ist). Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus): Ist diese Eigenschaft auf **true** festgelegt, verhindert sie die Anzeige des SIP, wenn der Fokus programmgesteuert auf ein Textsteuerelement festgelegt wird. Stattdessen wird die Tastatur nur angezeigt, wenn der Benutzer mit dem Steuerelement interagiert.

## <a name="touch-keyboard-index-for-windows"></a>Touchscreen-Tastatur Index für Windows

In diesen Tabellen werden die Windows-SIP-Layouts (Soft Input Panel) für allgemeine Eingabe Bereichs Werte angezeigt. Die Auswirkungen des Eingabeumfangs auf die durch die Eigenschaften **IsSpellCheckEnabled** und **IsTextPredictionEnabled** aktivierten Features werden für jeden Eingabeumfang aufgeführt. Dies ist keine vollständige Liste der verfügbaren Eingabeumfangoptionen.

> [!Tip] 
> Sie können die meisten Berührungs Tastatur zwischen einem alphabetischen Layout und einem Zahlen-und Symbol Layout umschalten, indem Sie die **&123** -Taste drücken, um das Layout für Zahlen und Symbole zu ändern, und die **ABCD** -Taste drücken, um zum alphabetischen Layout zu wechseln.

### <a name="default"></a>Standard

`<TextBox InputScope="Default"/>`

Die standardmäßige Windows-Touchscreen-Tastatur.

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Rechtschreibprüfung: aktiviert, wenn **isspellcheckaktivierte**  =  **true**ist, deaktiviert, wenn **isspellcheckaktivierte**  =  **false** ist.
- Automatische Korrektur: aktiviert, wenn **isspellcheckaktivierte**  =  **true**ist, deaktiviert, wenn **isspellcheckaktivierte**  =  **false** ist.
- Automatische groß Schreibung: aktiviert, wenn **isrechtschreib checkaktiviert**  =  **true**ist, deaktiviert, wenn **isspellcheckaktivierte**  =  **false** ist.
- Text Vorhersage: aktiviert, wenn **istextvorhertionaktivierte**  =  **true**ist, deaktiviert, wenn **istextvorhertionaktivierte**  =  **false**

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

Das standardmäßige Tastaturlayout für Ziffern und Sonderzeichen.

![Windows-Bildschirmtastatur für Währungen](images/input-scopes/currencyamountandsymbol.png)

- Schließt die linke und Rechte Taste der Seite ein, um weitere Symbole anzuzeigen.
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden
 
### <a name="url"></a>url

`<TextBox InputScope="Url"/>`

![Windows-Bildschirmtastatur für URLs](images/input-scopes/url.png)

- Enthält die Tasten **.com** und ![Los-Taste](images/input-scopes/kbdgokey.png) (Los). Halten Sie die com-Taste gedrückt, um zusätzliche Optionen anzuzeigen (**. org**, **.net**und regionsspezifische Suffixe) **.**
- Enthält die **Schlüssel:**, **-** und **/** .
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden
- Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden
- Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Windows-Bildschirmtastatur für E-Mail-Adressen](images/input-scopes/emailsmtpaddress.png)
- Schließt die **@** Schlüssel und **. com** ein. Halten Sie die com-Taste gedrückt, um zusätzliche Optionen anzuzeigen (**. org**, **.net**und regionsspezifische Suffixe) **.**
- Enthält die **_** -und- **-** Schlüssel.
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden
- Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden
- Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![Windows-Bildschirmtastatur für Ziffern](images/input-scopes/number.png)
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Windows-Bildschirmtastatur für Telefonnummern](images/input-scopes/telephonenumber.png)
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="search"></a>Suchen,

`<TextBox InputScope="Search"/>`

![Windows-Bildschirmtastatur für die Suche](images/input-scopes/search.png)
- Enthält den **Such** Schlüssel anstelle der **Eingabe** Taste.
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Windows-Tastenkombination für inkrementelle Suche](images/input-scopes/searchincremental.png)
- Gleiches Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Immer deaktiviert
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Immer deaktiviert

### <a name="formula"></a>Formel

`<TextBox InputScope="Formula"/>`

![Windows-Tastenkombination für Formel](images/input-scopes/formula.png)
- Schließt den **=** Schlüssel ein.
- Umfasst auch die **%** **$** Schlüssel, und **+** .
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Gleiches Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Standardmäßig aktiviert, kann deaktiviert werden
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Gleiches Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden
- Automatische groß Schreibung: standardmäßig deaktiviert, kann aktiviert werden (der erste Buchstabe jedes Worts ist groß geschrieben)
- Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden