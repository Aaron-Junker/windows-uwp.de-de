---
Description: Gestalten Sie Ihre App so, dass sie global einsetzbar ist, indem Sie Datum, Uhrzeit, Telefonnummern und Währungen entsprechend formatieren. Dadurch können Sie Ihre App später zur weltweiten Vermarktung für weitere Kulturkreise, Regionen und Sprachen anpassen.
title: Globalisieren von Datum, Uhrzeit und Zahlenformaten
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: d641bcff48b830c56a1d03ee861ec2a4c5f433b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634135"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globalisieren von Datum, Uhrzeit und Zahlenformaten

Gestalten Sie Ihre App so, dass sie global einsetzbar ist, indem Sie Datum, Uhrzeit, Telefonnummern und Währungen entsprechend formatieren. Dadurch können Sie Ihre App später zur weltweiten Vermarktung für weitere Kulturkreise, Regionen und Sprachen anpassen.

## <a name="introduction"></a>Einführung

Wenn Sie bei der Erstellung Ihrer App nicht nur Ihre Sprache und Kultur berücksichtigen, werden nur wenige (oder keine) Probleme auftreten, sobald Sie für Ihre App neue Märkte erschließen. Datumsangaben, Uhrzeiten, Zahlen, Kalender, Währungen, Telefonnummern, Maßeinheiten und Papierformate sind Beispiele für Elemente, die in den verschiedenen Kulturen oder Sprachen anders angezeigt werden können.

Verschiedene Regionen und Kulturen verwenden unterschiedliche Formate für Datum und Uhrzeit. Dazu gehören unterschiedliche Konventionen für die Reihenfolge von Tag und Monat im Datum, für die Trennung von Stunden und Minuten und sogar für das zu verwendende Trennzeichen. Zudem kann das Datum in verschiedenen langen Formaten (Mittwoch, 28. März 2012) oder kurzen Formaten (28.03.12) angezeigt werden, die je nach Kultur variieren können. Und natürlich sind die Namen und Kurzformen für die Wochentage und Monate in jeder Sprache unterschiedlich.

Sie können eine Vorschau der für verschiedene Sprachen verwendeten Formate anzeigen. Wählen Sie **Einstellungen** > **Zeit und Sprache** > **Region und Sprache**, und klicken Sie auf **Zusätzliche Datums-, Uhrzeit- und Ländereinstellungen** > **Datums-, Uhrzeit- oder Zahlenformat ändern**. Auf der Registerkarte **Formate** wählen Sie eine Sprache aus der Dropdownliste **Format** und erhalten eine Vorschau der Formate in **Beispiele**.

In diesem Thema werden die Begriffe „Benutzerprofil-Sprachenliste”, „App-Manifest-Sprachenliste” und „App-Laufzeit-Sprachenliste” verwendet. Einzelheiten dazu, was genau diese Begriffe bedeuten und wie sie auf ihre Werte zugreifen können, finden Sie unter [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Formatieren von Datumsangaben und Uhrzeiten für die App-Laufzeit-Sprachenliste

Wenn Sie Benutzern erlauben möchten, ein Datum oder eine Uhrzeit auszuwählen, sollten Sie die Standardsteuerelemente für [Kalender, Datum und Uhrzeit](../controls-and-patterns/date-and-time.md) verwenden. Diese verwenden automatisch das beste Datums- und Zeitformat für die App-Laufzeit-Sprachenliste.

Zur Anzeige von Datumsangaben oder Uhrzeiten können Sie die Klasse [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) verwenden. Standardmäßig verwendet **DateTimeFormatter** automatisch das beste Datums- und Zeitformat für die App-Laufzeit-Sprachenliste. Der folgende Code formatiert also einen gegebenen **DateTime**-Wert optimal für diese Liste. Nehmen wir zum Beispiel an, dass die App-Manifest-Sprachenliste Englisch (USA) als Standardsprache und zudem Deutsch (Deutschland) enthält. Wenn das aktuelle Datum „Nov 6 2017” ist und die Benutzerprofil-Sprachenliste Deutsch (Deutschland) als erste Sprache enthält, gibt der Formatierer „06.11.2017” zurück. Wenn die Benutzerprofil-Sprachenliste zuerst Englisch (USA) enthält (oder wenn sie weder Englisch noch Deutsch enthält), gibt der Formatierer „11/6/2017” zurück (da „en-US” übereinstimmt oder als Standard verwendet wird) ).

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

Sie können den Code oben auf Ihrem eigenen PC wie folgt testen.

- Stellen Sie sicher, dass die Ressourcendateien in Ihrem Projekt sowohl für „en-US”" als auch für „de-DE” qualifiziert sind (siehe [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und andere Eigenschaften](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Ändern Sie die Benutzerprofil-Sprachenliste unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Sprachen**. Fügen Sie Deutsch (Deutschland) als Standardsprache hinzu, und führen Sie den Code erneut aus.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Formatieren von Datumsangaben und Uhrzeiten für die Benutzerprofil-Sprachenliste

Denken Sie daran, dass **DateTimeFormatter** standardmäßig die App-Runtime-Sprachenliste übereinstimmt. Daher wird bei der Anzeige einer Zeichenfolge wie „Das Datum ist der &lt;date&gt;” die Sprache dem Datumsformat entsprechen.

Wenn Sie Daten und/oder Uhrzeiten aus irgendeinem Grund nur gemäß der Benutzerprofil-Sprachenliste formatieren möchten, können Sie dies mithilfe von Code wie im folgenden Beispiel tun. Aber in diesem Fall können Sie nicht ausschließen, dass der Benutzer eine Sprache wählt, für die Ihre App keine Zeichenfolgen übersetzt hat. Wenn Ihre App beispielsweise nicht in Deutsch (Deutschland) lokalisiert ist, aber der Benutzer diese Sprache als bevorzugte Sprache auswählt, kann dies zur Anzeige einer Zeichenfolgen wie „Das Datum ist der 06.11.2017” führen.

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Formatieren Sie Zahlen und Währungen entsprechend

In verschiedenen Kulturen werden Zahlen unterschiedlich formatiert. Formatunterschiede können sich auf Folgendes beziehen: wie viele Dezimalziffern angezeigt werden, welche Zeichen als Dezimaltrennzeichen verwendet werden und welches Währungssymbol verwendet wird. Verwenden Sie Klassen aus dem Namespace [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live), um Dezimal-, Prozent- oder Promillezahlen und Währungen anzuzeigen. In den meisten Fällen wird es Ihre Absicht sein, dass diese Formatierungsklassen das beste Format für das Benutzerprofil verwenden. Sie können die Formatierer aber auch verwenden, um eine Währung für eine beliebige Region oder ein beliebiges Format anzuzeigen.

Dieses Beispiel zeigt, wie Währungen sowohl für das Benutzerprofil als auch für ein bestimmtes Währungssystem angezeigt werden.

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

Sie können den nachstehenden Code auf Ihrem PC testen. Ändern Sie dazu Land oder Region unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Land oder Region**. Wählen Sie ein Land oder eine Region (z. B. Island), und führen Sie den Code erneut aus.

## <a name="use-a-culturally-appropriate-calendar"></a>Verwenden Sie einen kulturspezifischen Kalender

Der Kalender ist für verschiedene Regionen und Sprachen unterschiedlich. Der gregorianische Kalender ist nicht der Standardkalender für alle Regionen. In einigen Regionen wählen Benutzer möglicherweise alternative Kalender aus, z. B. den japanischen Era-Kalender oder den arabischen Mondkalender. In Datumsangaben und Uhrzeiten im Kalender werden auch verschiedene Zeitzonen und Sommerzeiten berücksichtigt.

Um sicherzustellen, dass das bevorzugte Kalenderformat verwendet wird, können Sie die standardmäßigen [Steuerelemente für Kalender, Datum und Uhrzeit](../controls-and-patterns/date-and-time.md) verwenden. Für komplexere Szenarien, in denen direkt mit Vorgängen in Bezug auf das Kalenderdatum gearbeitet werden muss, stellt **Windows.Globalization** eine [**Calendar**](/uwp/api/windows.globalization.calendar?branch=live)-Klasse bereit, die eine passende Kalenderdarstellung für die jeweilige Kultur, Region und den Kalendertyp ermöglicht.

## <a name="format-phone-numbers-appropriately"></a>Formatieren Sie Telefonnummern entsprechend

Telefonnummern werden in verschiedenen Regionen unterschiedlich formatiert. Die Anzahl der Stellen, die Gruppierung der Ziffern und die Bedeutung bestimmter Teile der Telefonnummer variieren zwischen verschiedenen Ländern. Ab Windows 10, Version 1607, können Sie Klassen aus dem Namespace [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) verwenden, um Telefonnummern gemäß der aktuellen Region zu formatieren.

[**PhoneNumberInfo** ](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analysiert eine Zeichenfolge der Ziffern und ermöglicht Ihnen,: bestimmen, ob die Ziffern einer gültigen Telefonnummer in der aktuellen Region sind; Vergleichen zweier Zahlen Gleichheit und zum Extrahieren von den funktionalen Bestandteilen des Telefons Zahl, z. B. Ländercode oder geografischen Bereich-Code.

[**PhoneNumberFormatter** ](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) Formate, die eine Folge von Ziffern oder **PhoneNumberInfo** für die Anzeige, selbst wenn die Zeichenfolge der Ziffern eine partielle Telefonnummer darstellt. Sie können diese partielle Nummernformatierung verwenden, um eine Zahl zu formatieren, die gerade von einem Benutzer eingegeben wird.

Das folgende Beispiel veranschaulicht die Verwendung von **PhoneNumberFormatter** zum Formatieren einer Telefonnummer, die gerade eingegeben wird. Sobald sich der Text in einer **TextBox** mit dem Namen „phoneNumberInputTextBox“ ändert, wird der Inhalt des Textfelds gemäß der aktuellen Standardregion formatiert und in einem **TextBlock** mit dem Namen „phoneNumberOutputTextBlock“ angezeigt. Zu Demonstrationszwecken wird die Zeichenfolge auch für die Region Neuseeland formatiert und in einem TextBlock mit dem Namen phoneNumberOutputTextBlockNZ angezeigt.
  
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

Sie können den nachstehenden Code auf Ihrem PC testen. Ändern Sie dazu Land oder Region unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Land oder Region**. Wählen Sie ein Land oder eine Region aus (z. B. Neuseeland, um zu prüfen, ob die Formate stimmen), und führen Sie den Code erneut aus. Für Testdaten können Sie eine Websuche nach der Telefonnummer eines Unternehmens in Neuseeland durchführen.

## <a name="the-users-language-and-cultural-preferences"></a>Sprach- und Kultureinstellungen des Benutzers

Für Szenarien, in denen Sie basierend auf den Sprach-, Regions- und Kultureinstellungen des Benutzers unterschiedliche Funktionen bereitstellen möchten, bietet Windows die Möglichkeit, über [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live) auf diese Einstellungen zuzugreifen. Verwenden Sie bei Bedarf die **GlobalizationPreferences**-Klasse, um den Wert der aktuellen Region des Benutzers sowie die bevorzugten Sprachen, Währungen usw. zu ermitteln. Aber vergessen Sie nicht: Wenn die Zeichenfolgen/Bilder Ihrer App nicht für die bevorzugte Sprache des Benutzers lokalisiert sind, werden Datum, Uhrzeit und andere Daten, die für diese bevorzugte Sprache formatiert sind, nicht mit den Zeichenfolgen übereinstimmen, die Sie anzeigen.

## <a name="important-apis"></a>Wichtige APIs

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Kalender](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Verwandte Themen

* [Kalender, Datums- und Zeitauswahl](../controls-and-patterns/date-and-time.md)
* [Grundlegendes zu Benutzersprachen-Profil und app-manifest-Sprachen](manage-language-and-region.md)
* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Beispiele

* [Kalenderdetails "und" Math-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=231636)
* [Datums- und Zeitformat-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=231618)
* [Beispiel für eine globalisierte Voreinstellungen](https://go.microsoft.com/fwlink/p/?linkid=231608)
* [Formatierung und Analyse von Beispiel-Nummer](https://go.microsoft.com/fwlink/p/?linkid=231620)
