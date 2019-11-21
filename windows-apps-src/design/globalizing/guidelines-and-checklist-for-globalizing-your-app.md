---
Description: Entwerfen und entwickeln Sie Ihre App so, dass sie auf Systemen mit unterschiedlichen Sprach- und Kulturkonfigurationen ordnungsgemäß funktioniert.
Search.Refinement.TopicID: 180
title: Richtlinien für Globalisierung
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 18c68baf991b3fd939a6e6ee681700977a5a5eb9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258087"
---
# <a name="guidelines-for-globalization"></a>Richtlinien für Globalisierung

Entwerfen und entwickeln Sie Ihre App so, dass sie auf Systemen mit unterschiedlichen Sprach- und Kulturkonfigurationen ordnungsgemäß funktioniert. Verwenden Sie die [**Globalization**](/uwp/api/Windows.Globalization?branch=live)-APIs zum Formatieren von Daten; und vermeiden Sie in Ihrem Code Annahmen über Sprache, Region, Zeichenklassifizierung, Schreibsystem, Formatierung von Datum und Uhrzeit, Zahlen, Währungen, Gewichte und Sortierregeln.

| Empfehlung | Beschreibung |
| ------------- | ----------- |
| Berücksichtigen Sie die Kultur beim Bearbeiten und Vergleichen von Zeichenfolgen. | Ändern Sie beispielsweise die Groß- bzw. Kleinschreibung von Zeichenfolgen nicht, bevor Sie sie vergleichen. Weitere Informationen finden Sie unter [Empfehlungen für die Verwendung von Zeichenfolgen](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Gehen Sie bei der Sortierung von Zeichenfolgen und andere Daten nicht davon aus, dass immer alphabetisch sortiert wird. | Bei Sprachen, die keine Lateinschrift verwenden, basiert die Sortierung auf anderen Faktoren wie der Aussprache oder der Anzahl der Schriftzeichen. Selbst bei Sprachen mit Lateinschrift wird nicht immer alphabetisch sortiert. In einigen Kulturen enthält ein Telefonbuch vielleicht keine alphabetische Sortierung. Windows kann die Sortierung für Sie übernehmen. Sollten Sie einen eigenen Sortieralgorithmus erstellen, müssen Sie die Sortiermethoden des Zielmarkts berücksichtigen. |
| Verwenden Sie die richtigen Formate für Zahlen, Datumsangaben, Uhrzeiten, Adressen und Telefonnummern. | Diese Formate unterscheiden sich zwischen Kulturen, Regionen, Sprachen und Märkten. Wenn Sie diese Daten anzeigen, verwenden Sie [**Globalization**](/uwp/api/Windows.Globalization?branch=live)-APIs, um das für die jeweilige Zielgruppe angemessene Format sicherzustellen. Siehe [Globalisieren von Datum, Uhrzeit und Zahlenformaten](use-global-ready-formats.md). Die Reihenfolge, in welche Vor- und Familienname angezeigt werden, und das Format von Adressen kann ebenfalls unterschiedlich sein. Verwenden Sie Standardanzeigen für Datum, Uhrzeit und Zahlen. Verwenden Sie Standardsteuerelemente wie die Datumsauswahl für Datum und Uhrzeit. Verwenden Sie die standardisierte Adressinformationen. |
| Unterstützen Sie internationale Maßeinheiten und Währungen. | In den verschiedenen Ländern werden unterschiedlichen Einheiten und Skalen verwendet, wobei das metrische und das imperiale System am verbreitetsten sind. Wenn Sie mit Maßen wie Länge, Temperatur oder Flächen arbeiten, stellen Sie sicher, dass Sie die richtigen Systemmaße unterstützen. Verwenden Sie die Eigenschaft [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse), um die in einer Region verwendeten Währungen zu erhalten. |
| Verwenden Sie Unicode zur Zeichencodierung. | In Microsoft Visual Studio wird standardmäßig für alle Dokumente die Unicode-Zeichencodierung verwendet. Stellen Sie bei Verwendung eines anderen Editors sicher, dass Sie die Quelldateien in der richtigen Unicode-Zeichencodierung speichern. Alle UWP-APIs geben UTF-16-codierte Zeichenfolgen zurück. |
| Unterstützen Sie internationale Papierformate. | Die gebräuchlichen Papierformate unterscheiden sich zwischen den Ländern. Unterstützen und testen Sie häufig verwendete internationale Formate, wenn Sie Features einsetzen möchten, bei denen das Papierformat eine Rolle spielt (z. B. beim Drucken). |
| Speichern Sie die Sprache von Tastatur oder IME. | Wenn Ihre App den Benutzer zur Texteingabe auffordert, speichern Sie das Sprachentag für das derzeit aktivierte Tastaturlayout oder den Eingabemethoden-Editor (IME). Dadurch stellen Sie sicher, dass die Eingabe dem Benutzer später in der richtigen Formatierung angezeigt wird. Ermitteln Sie die aktuelle Eingabesprache mit der Eigenschaft [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag). |
| Leiten Sie die Region des Benutzers nicht von der Sprache ab und umgekehrt nicht die Sprache von der Region des Benutzers. | Sprache und Region sind zwei unterschiedliche Konzepte. Ein Benutzer kann eine bestimmte regionale Sprachvariante sprechen, beispielsweise „en-GB ”für Englisch in England, kann sich jedoch in einem ganz anderen Land oder einer anderen Region befinden. Überlegen Sie, ob die Sprache des Benutzers für die App relevant ist, etwa für UI-Text, und ob die Region relevant ist (beispielsweise für Lizenzbelange). Weitere Informationen finden Sie unter [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md). |
| Die Regeln zum Vergleichen von Sprachtags sind nicht trivial. | [BCP-47-Sprachtags](https://tools.ietf.org/html/bcp47) sind komplex. Beim Vergleichen von Sprachtags muss einiges berücksichtigt werden, wie z. B. der Abgleich von Skriptinformationen, Legacytags und mehrfache regionale Varianten. Das Ressourcenverwaltungssystem in Windows übernimmt den Abgleich für Sie. Sie können eine Ressourcengruppe in beliebigen Sprachen angeben, und das System wählt die geeignete Gruppe für den Benutzer und die App aus. Weitere Informationen finden Sie unter [App-Ressourcen und das Ressourcenverwaltungssystem](../../app-resources/index.md) und [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md). |
| Entwerfen Sie Ihre Benutzeroberfläche so, dass verschiedenen Textlängen und Schriftgrößen für Beschriftungen und Texteingabesteuerelemente möglich sind. | Zeichenfolgen, die in verschiedene Sprachen übersetzt sind, können sehr unterschiedlich lang sein. Daher müssen Sie die UI-Steuerelemente dynamisch an ihren Inhalt anpassen. Gängige Zeichen in anderen Sprachen enthalten Markierungen über oder unter denen, die normalerweise in Englisch verwendet werden (wie Å oder Ņ). Verwenden Sie die standardmäßigen Schriftgrößen und Zeilenhöhen, um angemessenen vertikalen Platz bereitzustellen. Beachten Sie, dass Schriften für andere Sprachen möglicherweise größere Mindestschriftgrößen benötigen, um lesbar zu bleiben. Siehe die Klassen im Namespace [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live). |
| Unterstützen Sie die Spiegelung der Leserichtung. | Die Textausrichtung und Leserichtung kann von links nach rechts (wie bei Deutsch) oder von links nach rechts (RTL, wie bei Arabisch oder Hebräisch) verlaufen. Wenn Sie Ihr Produkt in Sprachen mit einer anderen Leserichtung lokalisieren, achten Sie darauf, dass das Layout der UI-Elemente eine Spiegelung unterstützt. Auch Elemente wie Zurück-Schaltflächen, UI-Übergangseffekte und Bilder müssen gegebenenfalls gespiegelt werden. Weitere Informationen finden Sie unter [Layout und Schriften anpassen und RTL unterstützen](adjust-layout-and-fonts--and-support-rtl.md). |
| Zeigen Sie Text und Schriften richtig an. | Die ideale Schriftart sowie Schriftgröße und Textrichtung sind vom jeweiligen Markt abhängig. Weitere Informationen finden Sie unter [**Layout und Schriften anpassen und RTL unterstützen**](adjust-layout-and-fonts--and-support-rtl.md) und [Internationale Schriften](loc-international-fonts.md). |

## <a name="important-apis"></a>Wichtige APIs
 
* [Globalization](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Verwandte Themen

* [Recommendations for String Usage](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globalize your date/time/number formats](use-global-ready-formats.md)
* [Understand user profile languages and app manifest languages](manage-language-and-region.md)
* [BCP-47 language tags](https://tools.ietf.org/html/bcp47)
* [App resources and the Resource Management System](../../app-resources/index.md)
* [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md)
* [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md)
* [International fonts](loc-international-fonts.md)
* [Make your app localizable](prepare-your-app-for-localization.md)

## <a name="samples"></a>Beispiele

* [Globalization preferences sample](https://code.msdn.microsoft.com/windowsapps/Globalization-preferences-6654eb36)
