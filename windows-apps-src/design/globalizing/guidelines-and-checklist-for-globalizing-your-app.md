---
Description: Entwerfen und entwickeln Sie Ihre APP so, dass Sie auf Systemen mit unterschiedlichen sprach-und Kultur Konfigurationen ordnungsgemäß funktioniert.
Search.Refinement.TopicID: 180
title: Richtlinien für Globalisierung
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: d71cf2289654860b47aef18c117ac9d6d36fab0a
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493245"
---
# <a name="guidelines-for-globalization"></a>Richtlinien für Globalisierung

Entwerfen und entwickeln Sie Ihre APP so, dass Sie auf Systemen mit unterschiedlichen sprach-und Kultur Konfigurationen ordnungsgemäß funktioniert. Verwenden von [**Globalisierungs**](/uwp/api/Windows.Globalization?branch=live) -APIs zum Formatieren von Daten und vermeiden Sie Annahmen in Ihrem Code über Sprache, Region, Zeichen Klassifizierung, Schreiben von System, Datums-/Uhrzeitformatierung, Zahlen, Währungen, Gewichtungen und Sortierregeln.

| Empfehlung | BESCHREIBUNG |
| ------------- | ----------- |
| Berücksichtigen Sie beim Bearbeiten und Vergleichen von Zeichen folgen die Kultur. | Ändern Sie beispielsweise die Groß-/Kleinschreibung von Zeichen folgen nicht, bevor Sie Sie vergleichen Siehe [Empfehlungen zur Zeichen folgen Verwendung](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Wenn Sie Zeichen folgen und andere Daten sortieren (Sortieren), gehen Sie nicht davon aus, dass Sie immer alphabetisch ausgeführt werden. | Für Sprachen, die kein lateinischen Skript verwenden, basiert die Sortierung auf Faktoren wie der Aussprache oder der Anzahl der Stift Striche. Auch Sprachen, die das lateinische Skript verwenden, verwenden nicht immer die alphabetische Sortierung. In einigen Kulturen enthält ein Telefonbuch vielleicht keine alphabetische Sortierung. Windows kann die Sortierung für Sie behandeln, aber wenn Sie einen eigenen Sortieralgorithmus erstellen, sollten Sie die in den Zielmärkten verwendeten Sortiermethoden berücksichtigen. |
| Angemessene Formatierung von Zahlen, Datumsangaben, Uhrzeiten, Adressen und Telefonnummern. | Diese Formate unterscheiden sich zwischen Kulturen, Regionen, Sprachen und Märkten. Wenn Sie diese Daten anzeigen, verwenden Sie [**Globalisierungs**](/uwp/api/Windows.Globalization?branch=live) -APIs, um das für eine bestimmte Zielgruppe geeignete Format zu erhalten. Siehe [Globalisieren von Datums-/Uhrzeit-/nummerformaten](use-global-ready-formats.md). Die Reihenfolge, in der die Familie und die angegebenen Namen angezeigt werden, und das Format der Adressen können ebenfalls abweichen. Verwenden Sie die standardmäßigen Datums-, Uhrzeit-und Zahl anzeigen. Verwenden Sie Standard Steuerelemente für Datums-und Zeitauswahl. Standard Adressinformationen verwenden. |
| Unterstützen Sie internationale Maßeinheiten und Währungen. | In den verschiedenen Ländern werden unterschiedlichen Einheiten und Skalen verwendet, wobei das metrische und das imperiale System am verbreitetsten sind. Stellen Sie sicher, dass Sie die richtige System Messung unterstützen, wenn Sie Messungen wie Länge, Temperatur und Bereich durchsetzen. Verwenden Sie die Eigenschaft " [**geocregion. Currency cesinuse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) ", um den Satz von Währungen zu erhalten, die in einer Region verwendet werden. |
| Verwenden Sie Unicode zur Zeichencodierung. | Standardmäßig verwendet Microsoft Visual Studio Unicode-Zeichencodierung für alle Dokumente. Stellen Sie bei Verwendung eines anderen Editors sicher, dass Sie die Quelldateien in der richtigen Unicode-Zeichencodierung speichern. Alle Windows-Runtime-APIs geben UTF-16-codierte Zeichenfolgen zurück. |
| Unterstützen Sie internationale Papierformate. | Die gängigsten Papiergrößen unterscheiden sich zwischen den Ländern. Wenn Sie also Features einschließen, die von der Papiergröße abhängen, z. b. Drucken, stellen Sie sicher, dass Sie allgemeine internationale Größen unterstützen und testen. |
| Notieren Sie die Sprache der Tastatur oder der IME. | Wenn die APP den Benutzer zur Eingabe von Texteingaben auffordert, notieren Sie das Sprachtag für das aktuell aktivierte Tastaturlayout oder den Eingabemethoden-Editor (IME). Dadurch stellen Sie sicher, dass die Eingabe dem Benutzer später in der richtigen Formatierung angezeigt wird. Verwenden Sie die Eigenschaft [**Language. currentinputmethodlanguagetag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) , um die aktuelle Eingabe Sprache abzurufen. |
| Verwenden Sie die Sprache nicht, um die Region eines Benutzers anzunehmen. Verwenden Sie die Region nicht, um die Sprache eines Benutzers anzunehmen. | Sprache und Region sind separate Konzepte. Ein Benutzer kann eine bestimmte regionale Variante einer Sprache sprechen, wie z. b. "en-GB" für Englisch, wie im Vereinigten Königreich gesprochen, aber der Benutzer befindet sich möglicherweise in einem völlig anderen Land oder einer Region. Stellen Sie sich vor, ob Ihre APP Kenntnisse über die Sprache des Benutzers (z. b. für den Benutzeroberflächen Text) oder die Region (z. b. zur Lizenzierung) erfordert. Weitere Informationen finden Sie Untergrund Legendes zu [Benutzerprofil Sprachen und App-Manifest-Sprachen](manage-language-and-region.md). |
| Die Regeln zum Vergleichen von sprach Tags sind nicht trivial. | [Bcp-47-sprach Tags](https://tools.ietf.org/html/bcp47) sind komplex. Beim Vergleichen von Sprachtags muss einiges berücksichtigt werden, wie z. B. der Abgleich von Skriptinformationen, Legacytags und mehrfache regionale Varianten. Das Ressourcen Verwaltungs System in Windows kümmert sich um die Übereinstimmung. Sie können eine Ressourcengruppe in beliebigen Sprachen angeben, und das System wählt die geeignete Gruppe für den Benutzer und die App aus. Weitere Informationen finden Sie unter [App-Ressourcen und das Ressourcen Verwaltungssystem](../../app-resources/index.md) und [Funktionsweise des Resource Management-Systems mit sprach Tags](../../app-resources/how-rms-matches-lang-tags.md). |
| Entwerfen Sie die Benutzeroberfläche, um unterschiedliche Textlängen und Schriftgrößen für Bezeichnungen und Texteingabe-Steuerelemente zu verarbeiten. | Zeichen folgen, die in verschiedene Sprachen übersetzt werden, können stark variieren, sodass Sie Ihre UI-Steuerelemente für eine dynamische Größe Ihrer Inhalte benötigen. Gängige Zeichen in anderen Sprachen enthalten Markierungen oberhalb oder unterhalb der in englischer Sprache verwendeten Zeichen (z. b. "Å" oder "Ņ"). Verwenden Sie die Standardschrift Größen und Linien Höhen, um angemessenen vertikalen Raum bereitzustellen. Beachten Sie, dass Schriftarten für andere Sprachen größere mindestschriftgrade erfordern, damit Sie lesbar sind. Weitere Informationen finden Sie in den Klassen im [Windows. Globalization. Fonts](/uwp/api/windows.globalization.fonts?branch=live) -Namespace. |
| Unterstützung der Spiegelung von Lesereihenfolge. | Text Ausrichtung und Lesefolge können von links nach rechts (z. b. in englischer Sprache) oder von rechts nach links (RTL) (z. b. Arabisch oder Hebräisch) sein. Wenn Sie Ihr Produkt in Sprachen lokalisieren, die eine andere Lesereihenfolge als ihre eigenen verwenden, stellen Sie sicher, dass das Layout der Benutzeroberflächen Elemente die Spiegelung unterstützt. Sogar Elemente, wie z. b. Back-Schaltflächen, UI-Übergangseffekte und Bilder, müssen möglicherweise gespiegelt werden. Weitere Informationen finden Sie [unter Anpassen von Layout und Schriftarten und Unterstützung von RTL](adjust-layout-and-fonts--and-support-rtl.md). |
| Zeigen Sie Text und Schriftarten richtig an. | Die ideale Schriftart, Schriftgrad und Textrichtung hängt vom jeweiligen Markt ab. Weitere Informationen finden Sie [**unter Anpassen von Layout und Schriftarten und unterstützen von RTL**](adjust-layout-and-fonts--and-support-rtl.md) und [internationalen Schriftarten](loc-international-fonts.md). |

## <a name="important-apis"></a>Wichtige APIs
 
* [Globalisierung](/uwp/api/Windows.Globalization?branch=live)
* [Geografische Region. Currency cregion](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language. currentinputmethodlanguagetag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Verwandte Themen

* [Empfehlungen für die Zeichen folgen Verwendung](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globalisieren von Datums-, Uhrzeit- und Zahlenformaten](use-global-ready-formats.md)
* [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md)
* [Bcp-47-sprach Tags](https://tools.ietf.org/html/bcp47)
* [App-Ressourcen und das Ressourcenverwaltungssystem](../../app-resources/index.md)
* [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md)
* [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md)
* [Internationale Schriftarten](loc-international-fonts.md)
* [App lokalisierbar machen](prepare-your-app-for-localization.md)

## <a name="samples"></a>Beispiele

* [Beispiel für Globalisierungseinstellungen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
