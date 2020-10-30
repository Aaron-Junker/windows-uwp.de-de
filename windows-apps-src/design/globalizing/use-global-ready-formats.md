---
description: Entwerfen Sie Ihre APP so, dass Sie Global bereit ist, indem Sie Datumsangaben, Uhrzeiten, Zahlen, Telefonnummern und Währungen entsprechend formatieren. Anschließend können Sie Ihre APP später für weitere Kulturen, Regionen und Sprachen auf dem globalen Markt anpassen.
title: Globalisieren von Datums-, Uhrzeit- und Zahlenformaten
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 8c3bacbfbbe944cddfe014fcd34038ca9a56c36e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034343"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globalisieren von Datums-, Uhrzeit- und Zahlenformaten

Entwerfen Sie Ihre APP so, dass Sie Global bereit ist, indem Sie Datumsangaben, Uhrzeiten, Zahlen, Telefonnummern und Währungen entsprechend formatieren. Anschließend können Sie Ihre APP später für weitere Kulturen, Regionen und Sprachen auf dem globalen Markt anpassen.

## <a name="introduction"></a>Einführung

Wenn Sie bei der Erstellung Ihrer APP in großem Umfang als eine Sprache und eine Kultur betrachten, haben Sie weniger (wenn überhaupt) unerwartete Probleme, wenn Ihre APP in neue Märkte wächst. Datumsangaben, Uhrzeiten, Zahlen, Kalender, Währungen, Telefonnummern, Maßeinheiten und Papierformate sind Beispiele für Elemente, die in den verschiedenen Kulturen oder Sprachen anders angezeigt werden können.

Für unterschiedliche Regionen und Kulturen werden verschiedene Datums-und Uhrzeit Formate verwendet. Hierzu gehören Konventionen für die Reihenfolge von Tag und Monat im Datum, für die Trennung von Stunden und Minuten in der Zeit und sogar für das, was als Trennzeichen verwendet wird. Außerdem können Datumsangaben in verschiedenen langen Formaten ("Mittwoch, am 28. März 2012") oder in kurzen Formaten ("3/28/12") angezeigt werden, die sich zwischen Kulturen unterscheiden. Und natürlich unterscheiden sich die Namen und Abkürzungen für die Wochentage und Monate des Jahres zwischen den Sprachen.

Sie können eine Vorschau der Formate verwenden, die für verschiedene Sprachen verwendet werden. Wechseln Sie zu **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprache** , und klicken Sie auf **zusätzliche Datums-, Uhrzeit-und & regionale Einstellungen**  >  **Ändern von Datums-, Uhrzeit-oder Zahlenformaten** . Wählen Sie auf der Registerkarte **Formate** eine Sprache aus der Dropdown-Dropdown- **Datei** aus, und zeigen Sie die Formate in den **Beispielen** an

In diesem Thema werden die Begriffe "Benutzerprofil-Sprachliste", "App-Manifest-Sprachliste" und "App-Lauf Zeit Sprachliste" verwendet. Ausführliche Informationen dazu, was diese Begriffe bedeuten und wie Sie auf ihre Werte zugreifen, finden Sie Untergrund Legendes zu [Benutzerprofil Sprachen und App-Manifest-Sprachen](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Formatieren von Datums-und Uhrzeitangaben für die Anwendungs-Runtime

Wenn Sie zulassen möchten, dass Benutzer ein Datum auswählen oder eine Uhrzeit auswählen, verwenden Sie die Standard Steuer [Elemente für Kalender, Datum und Uhrzeit](../controls-and-patterns/date-and-time.md). Diese verwenden automatisch das beste Datums-und Uhrzeit Format für die Sprachliste der APP-Laufzeit.

Wenn Sie Datumsangaben oder Uhrzeiten Selbstanzeigen müssen, können Sie die [**datetimeformatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) -Klasse verwenden. Standardmäßig verwendet **datetimeformatter** automatisch das beste Datums-und Uhrzeit Format für die Sprachliste der APP-Laufzeit. Der folgende Code formatiert einen gegebenen **DateTime** -Wert in der besten Weise für diese Liste. Nehmen Sie beispielsweise an, dass Ihre APP-Manifest-Sprachliste Englisch (USA) enthält. Dies ist auch die Standardeinstellung und Deutsch (Deutschland). Wenn das aktuelle Datum 6 2017 ist und die Benutzerprofil-Sprachliste zuerst Deutsch (Deutschland) enthält, gibt das Formatierer "06.11.2017" an. Wenn die Benutzerprofil-Sprachliste zuerst Englisch (USA) enthält (oder wenn Sie weder Englisch noch Deutsch enthält), gibt der Formatierer "11/6/2017" (da "en-US" übereinstimmt oder als Standard verwendet wird).

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

Sie können den obigen Code auf Ihrem eigenen PC wie folgt testen.

- Stellen Sie sicher, dass Sie Ressourcen Dateien in Ihrem Projekt für "en-US" und "de-de" qualifiziert haben (Weitere Informationen finden Sie unter [Anpassen von Ressourcen für Sprache, Skalierung, hoher Kontrast und andere Qualifizierer](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Ändern Sie die Liste der Benutzerprofil Sprachen in **Einstellungen**  >  **Zeit & sprach**  >  **Region &** Sprachen  >  **Sprachen** . Fügen Sie Deutsch (Deutschland) hinzu, machen Sie die Standardeinstellung, und führen Sie den Code erneut aus.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Formatieren von Datums-und Uhrzeitwerten für die Benutzerprofil Sprache

Beachten Sie, dass **datetimeformatter** standardmäßig mit der Sprache der APP-Lauf Zeit Sprache übereinstimmt. Wenn Sie z. b. Zeichen folgen wie "date is &lt; Date &gt; " anzeigen, entspricht die Sprache dem Datumsformat.

Wenn Sie aus irgendeinem Grund Datums-und/oder Uhrzeiten nur entsprechend der Benutzerprofil-Sprachliste formatieren möchten, können Sie dies mithilfe von Code wie dem folgenden Beispiel tun. Wenn Sie dies jedoch tun, wissen Sie, dass der Benutzer eine Sprache auswählen kann, für die Ihre APP nicht übersetzte Zeichen folgen verfügt. Wenn Ihre APP z. b. nicht in Deutsch (Deutschland) lokalisiert ist, aber der Benutzer diese als bevorzugte Sprache auswählt, kann dies dazu führen, dass ganz ungerade aussehende Zeichen folgen wie "das Datum ist 06.11.2017" angezeigt werden.

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Formatieren Sie Zahlen und Währungen entsprechend

In verschiedenen Kulturen werden Zahlen unterschiedlich formatiert. Formatunterschiede können sich auf Folgendes beziehen: wie viele Dezimalziffern angezeigt werden, welche Zeichen als Dezimaltrennzeichen verwendet werden und welches Währungssymbol verwendet wird. Verwenden Sie Klassen im Namespace " [**nummeriformatierung**](/uwp/api/windows.globalization.numberformatting?branch=live) ", um Dezimal-, Prozent-oder perpromille Zahlen sowie Währungen anzuzeigen. In den meisten Fällen möchten Sie, dass diese Formatierungs Klassen das beste Format für das Benutzerprofil verwenden. Sie können jedoch die Formatierer verwenden, um eine Währung für eine beliebige Region oder ein beliebiges Format anzuzeigen.

Dieses Beispiel zeigt, wie Währungen sowohl für das Benutzerprofil als auch für ein bestimmtes bestimmtes Währungssystem angezeigt werden.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

Sie können den obigen Code auf Ihrem eigenen PC testen, indem Sie das Land oder die Region in der **Einstellungs**  >  **Zeit & sprach**  >  **Region & Sprachen**  >  **Land oder der Region** ändern. Wählen Sie ein Land oder eine Region (vielleicht Island) aus, und führen Sie den Code erneut aus.

## <a name="use-a-culturally-appropriate-calendar"></a>Verwenden Sie einen kulturspezifischen Kalender

Der Kalender ist für verschiedene Regionen und Sprachen unterschiedlich. Der gregorianische Kalender ist nicht der Standardkalender für alle Regionen. Benutzer in einigen Regionen können alternative Kalender auswählen, z. b. den Kalender des japanischen Zeitraums oder arabische Mondkalender. Datums-und Uhrzeitangaben im Kalender sind auch für unterschiedliche Zeitzonen und Sommerzeit empfindlich.

Um sicherzustellen, dass das bevorzugte Kalender Format verwendet wird, können Sie die Standard Steuer [Elemente für Kalender, Datum und Uhrzeit](../controls-and-patterns/date-and-time.md)verwenden. Bei komplexeren Szenarien, in denen die direkte Verwendung von Vorgängen für Kalenderdaten erforderlich ist, stellt **Windows. Globalization** eine [**Kalender**](/uwp/api/windows.globalization.calendar?branch=live) Klasse bereit, die eine entsprechende Kalenderdarstellung für die angegebene Kultur, Region und den angegebenen Kalendertyp bereitstellt.

## <a name="format-phone-numbers-appropriately"></a>Formatieren Sie Telefonnummern entsprechend

Telefonnummern werden in verschiedenen Regionen unterschiedlich formatiert. Die Anzahl der Ziffern, die Art und Weise, wie die Ziffern gruppiert werden, und die Bedeutung bestimmter Teile der Telefonnummer variieren zwischen den Ländern. Ab Windows 10, Version 1607, können Sie Klassen im [**phonenumberformatierung**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) -Namespace verwenden, um die Telefonnummern für den aktuellen Bereich entsprechend zu formatieren.

[**Phonenumberinfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analysiert eine Zeichenfolge mit Ziffern und ermöglicht Ihnen Folgendes: bestimmen Sie, ob die Ziffern eine gültige Telefonnummer in der aktuellen Region sind. Vergleicht zwei Zahlen auf Gleichheit. und, um die verschiedenen funktionalen Teile der Telefonnummer zu extrahieren, z. b. Ländercode oder geografischer flächencode.

[**Phonenumberformatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) formatiert eine Zeichenfolge oder eine **phonenumberinfo** für die Anzeige, auch wenn die Zeichenfolge eine partielle Telefonnummer darstellt. Mit dieser partiellen Zahlen Formatierung können Sie eine Zahl formatieren, wenn ein Benutzer die Zahl eingibt.

Das folgende Beispiel zeigt, wie Sie **phonenumberformatter** verwenden, um eine Telefonnummer zu formatieren, während Sie eingegeben wird. Jedes Mal, wenn Text in einem **Textfeld** mit dem Namen "phonenumberinputtextbox" geändert wird, wird der Inhalt des Textfelds mit dem aktuellen Standardbereich formatiert und in einem **TextBlock** mit dem Namen "phonenumberoutputtextblock" angezeigt. Zu Demonstrationszwecken wird die Zeichenfolge auch mit der Region für Neuseeland formatiert und in einem TextBlock mit dem Namen "phonenumberoutputtextblocknz" angezeigt.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

Sie können den obigen Code auf Ihrem eigenen PC testen, indem Sie das Land oder die Region in der **Einstellungs**  >  **Zeit & sprach**  >  **Region & Sprachen**  >  **Land oder der Region** ändern. Wählen Sie ein Land oder eine Region (vielleicht Neuseeland, um zu bestätigen, dass die Formate entsprechen), und führen Sie den Code erneut aus. Für Testdaten können Sie eine Websuche nach der Telefonnummer eines Unternehmens in Neuseeland durchführen.

## <a name="the-users-language-and-cultural-preferences"></a>Benutzersprache und Kultur Einstellungen

Für Szenarien, in denen Sie unterschiedliche Funktionen bereitstellen möchten, die ausschließlich auf der Sprache, der Region oder den kulturellen Einstellungen des Benutzers basieren, bietet Windows Ihnen eine Möglichkeit, überWindows.System auf diese Einstellungen zuzugreifen [**. User Profile. globalizationpreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). Verwenden Sie bei Bedarf die **GlobalizationPreferences** -Klasse, um den Wert der aktuellen Region des Benutzers sowie die bevorzugten Sprachen, Währungen usw. zu ermitteln. Beachten Sie jedoch Folgendes: Wenn die Zeichen folgen/Images Ihrer APP nicht für die bevorzugte Sprache des Benutzers lokalisiert sind, entsprechen Datums-und Uhrzeitangaben und andere Daten, die für diese bevorzugte Sprache formatiert sind, nicht den angezeigten Zeichen folgen.

## <a name="important-apis"></a>Wichtige APIs

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Kalender](/uwp/api/windows.globalization.calendar?branch=live)
* [Phonenumberformatierung](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [Globalizationpreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Verwandte Themen

* [Kalender-, Datums- und Uhrzeitsteuerelemente](../controls-and-patterns/date-and-time.md)
* [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md)
* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Beispiele

* [Kalenderdetails und Mathematikbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Calendar%20details%20and%20math%20sample%20(Windows%208))
* [Beispiel für Datums- und Uhrzeitformatierung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
* [Beispiel für Globalisierungseinstellungen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
* [Beispiel für Zahlenformatierung und Analyse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Number%20formatting%20and%20parsing%20sample%20(Windows%208))
