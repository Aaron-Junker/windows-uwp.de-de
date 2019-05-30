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
ms.openlocfilehash: c522e21c45a3edd08a14b081cc227a83f19a3ea0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365355"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.

### <a name="important-apis"></a>Wichtige APIs
- [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


Die Bildschirmtastatur kann für die Texteingabe verwendet werden, wenn Ihre App auf einem Gerät mit Touchscreen ausgeführt wird. Die Bildschirmtastatur wird aufgerufen, wenn der Benutzer auf ein bearbeitbares Eingabefeld tippt (etwa auf ein **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** - oder **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** -Element). Benutzer können Daten in Ihrer App schneller und komfortabler eingeben, wenn Sie den *Eingabeumfang* des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet dem System einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen.

Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die Eigenschaft [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) auf **Number** fest. Dadurch wird das System angewiesen, die Zehnertastatur anzuzeigen, was dem Benutzer die Eingabe der PIN erleichtert.

> [!IMPORTANT]
> - Diese Informationen gelten nur für das SIP. Sie gelten nicht für Hardwaretastaturen oder die Bildschirmtastatur, die in den Windows-Optionen für erleichterte Bedienung verfügbar ist.
> - Durch diesen Eingabeumfang wird keine Eingabeüberprüfung durchgeführt, und der Benutzer kann Eingaben über eine Hardwaretastatur oder ein anderes Eingabegerät vornehmen. Die Benutzereingabe muss je nach Bedarf trotzdem in Ihrem Code überprüft werden.

## <a name="changing-the-input-scope-of-a-text-control"></a>Ändern des Eingabeumfangs eines Textsteuerelements

Die in einer App verfügbaren Eingabeumfangoptionen sind Member der **[InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** -Enumeration. Sie können die **InputScope**-Eigenschaft eines **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** - oder **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** -Elements auf einen dieser Werte festlegen.

> [!IMPORTANT]
> Die **[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** Eigenschaft **[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** unterstützt nur die **Kennwort** und  **NumericPin** Werte. Andere Werte werden ignoriert.

In diesem Verfahren ändern Sie den Eingabeumfang von mehreren Textfeldern, um ihn auf die erwarteten Daten für jedes Textfeld abzustimmen.

**So ändern Sie den Eingabebereich in XAML**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten.
2.  Fügen Sie dem Tag das [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)-Attribut hinzu, und geben Sie den [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)-Wert für die erwartete Eingabe an.

    Im Anschluss sehen Sie einige Textfelder, wie sie etwa in einem allgemeinen Kundenkontaktformular enthalten sein können. Durch Festlegen von [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) wird für jedes Textfeld eine Bildschirmtastatur mit geeigneten Layout für die jeweiligen Daten angezeigt.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**So ändern Sie den Eingabebereich in code**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten. Ist es nicht festgelegt, legen Sie das [x:Name-Attribut](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) fest, um im Code auf das Steuerelement verweisen zu können.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instanziieren Sie ein neues [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekt.

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Instanziieren Sie ein neues [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekt.
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Legen Sie die [**NameValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscopename.namevalue)-Eigenschaft des [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekts auf den Wert der [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)-Enumeration fest.

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Fügen Sie das [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName)-Objekt zur [**Names**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope.names)-Sammlung des [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekts hinzu.

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Legen Sie das [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope)-Objekt als Wert für die [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)-Eigenschaft des Textsteuerelements fest.

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

Die Steuerelemente [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) und [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) verfügen über mehrere Eigenschaften, die das Verhalten des SIP beeinflussen. Sie müssen wissen, wie sich diese Eigenschaften auf die touchbasierte Texteingabe auswirken, um Ihren Benutzer die bestmögliche Benutzererfahrung bieten zu können.

-   [**IsSpellCheckEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)– Wenn die Rechtschreibprüfung für ein Textfeld-Steuerelement aktiviert ist, wird das Steuerelement interagiert, mit dem System-Rechtschreibprüfung-Engine gekennzeichnet werden sollen, die nicht erkannt werden. Durch Tippen auf ein Wort können Sie eine Liste mit Korrekturvorschlägen anzeigen. Die Rechtschreibprüfung ist standardmäßig aktiviert.

    Bei Verwendung des Eingabeumfangs **Default** ermöglicht diese Eigenschaft auch die automatische Großschreibung des ersten Worts in einem Satz sowie eine Autokorrektur während der Eingabe. Diese Features für die automatische Korrektur sind in anderen Eingabeumfängen möglicherweise deaktiviert. Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

-   [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)– Wenn Textvorhersage für ein Textfeld-Steuerelement aktiviert ist, zeigt das System eine Liste von Wörtern, die Sie in den Typ ab werden können. Sie können aus der Liste auswählen, sodass Sie nicht das gesamte Wort eingeben müssen. Die Textvorhersage ist standardmäßig aktiviert.

    Die Textvorhersage ist möglicherweise deaktiviert, wenn der Eingabeumfang nicht auf **Default** festgelegt ist (auch wenn die [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)-Eigenschaft auf **true** festgelegt ist). Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus)– Wenn diese Eigenschaft ist **"true"** , es wird verhindert, dass das System das SIP angezeigt, wenn der Fokus in einem Textsteuerelement programmgesteuert festgelegt wird. Stattdessen wird die Tastatur nur angezeigt, wenn der Benutzer mit dem Steuerelement interagiert.

## <a name="touch-keyboard-index-for-windows"></a>Tastatur-Index für die Windows Touch

Die folgende Tabelle enthält die Windows Soft Input Panel (SIP)-Layouts für häufig verwendete Eingabebereich-Werte. Die Auswirkungen des Eingabeumfangs auf die durch die Eigenschaften **IsSpellCheckEnabled** und **IsTextPredictionEnabled** aktivierten Features werden für jeden Eingabeumfang aufgeführt. Dies ist keine vollständige Liste der verfügbaren Eingabeumfangoptionen.

> [!Tip] 
> Sie können den meisten Touch-Tastaturen, zwischen einer alphabetischen Layout und ein Layout für Zahlen und Symbole wechseln, drücken Sie die **& 123** Schlüssel so ändern Sie das Layout von Zahlen und Symbole ein, und drücken Sie die **Abcd** Schlüssel Ändern Sie in der alphabetischen Layout.

### <a name="default"></a>Default

`<TextBox InputScope="Default"/>`

Die Standard-Windows-Bildschirmtastatur.

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Rechtschreibprüfung: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
- Autokorrektur: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
- Automatische Großschreibung: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
- Textvorhersage: Aktiviert, wenn **IsTextPredictionEnabled** = **true**. Deaktiviert, wenn **IsTextPredictionEnabled** = **false**.

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

Das standardmäßige Tastaturlayout für Ziffern und Sonderzeichen.

![Windows-Bildschirmtastatur für Währungen](images/input-scopes/currencyamountandsymbol.png)

- Enthält die Seite nach links/rechts-Tasten, um die weitere Symbole anzeigen
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Windows-Bildschirmtastatur für URLs](images/input-scopes/url.png)

- Enthält die Tasten **.com** und ![Los-Taste](images/input-scopes/kbdgokey.png) (Los). Halten Sie die **.com** , um zusätzliche Optionen anzuzeigen ( **.org**, **.net**, und regionsspezifische Suffixe)
- Enthält die **:** , **-** , und **/** Schlüssel
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden
- Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden
- Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Windows-Bildschirmtastatur für E-Mail-Adressen](images/input-scopes/emailsmtpaddress.png)
- Enthält die Tasten **@** und **.com**. Halten Sie die **.com** , um zusätzliche Optionen anzuzeigen ( **.org**, **.net**, und regionsspezifische Suffixe)
- Enthält die **_** und **-** Schlüssel
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

### <a name="search"></a>Suche

`<TextBox InputScope="Search"/>`

![Windows-Bildschirmtastatur für die Suche](images/input-scopes/search.png)
- Enthält die **Suche** Schlüssel anstelle von der **EINGABETASTE** Schlüssel
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Windows Touch-Tastatur für inkrementelle Suche](images/input-scopes/searchincremental.png)
- Dasselbe Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Immer deaktiviert
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Immer deaktiviert

### <a name="formula"></a>Formula

`<TextBox InputScope="Formula"/>`

![Windows die Bildschirmtastatur-Formel](images/input-scopes/formula.png)
- Enthält die **=** Schlüssel
- Enthält auch die **%** , **$** , und **+** Schlüssel
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Immer deaktiviert
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Dasselbe Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden
- Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden
- Automatische Großschreibung: Standardmäßig aktiviert, kann deaktiviert werden
- Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/default.png)
- Dasselbe Layout wie **Standard**
- Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden
- Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden
- Automatische Großschreibung: off in der Standardeinstellung kann aktiviert werden (die ersten Buchstaben jedes Worts ist groß geschrieben)
- Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden
