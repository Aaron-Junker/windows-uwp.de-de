---
Description: Entwerfen Sie Ihre app Unterstützung für bidirektionalen Text (BiDi) bereitstellen, damit Sie kombinieren können Skripts von links nach rechts (LNR) und rechts-nach-links (RTL) Schreibsysteme, die in der Regel verschiedene Arten von Buchstaben enthalten.
title: Entwerfen einer App für bidirektionalen Text
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 66a158a96fcab5391030f4517b6420ba4585bf04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641125"
---
# <a name="design-your-app-for-bidirectional-text"></a>Entwerfen einer App für bidirektionalen Text

Entwerfen Sie Ihre App so, dass bidirektionale Textunterstützung (BiDi) bereitgestellt wird, sodass Sie Texte von rechts nach links (RTL) und von links nach rechts (LTR) kombinieren können, die im Allgemeinen unterschiedliche Alphabete enthalten.

Rechts-nach-links-Schriftsysteme, wie sie im Nahen Osten, in Zentral- und Südasien und in Afrika verwendet werden, haben besondere Designanforderungen. Diese Schreibsysteme erfordern Unterstützung von bidirektionalem Text (BiDi). Bei BiDi-Unterstützung handelt es sich um die Möglichkeit, Text sowohl von rechts nach links (RTL) als auch von links nach rechts (LTR) einzugeben und anzuzeigen.

In Windows werden insgesamt neun BiDi-Sprachen unterstützt.
- Zwei vollständig lokalisierte Sprachen. Arabisch und Hebräisch.
- Sieben Benutzeroberflächen-Sprachpakete für aufstrebende Märkte. Persisch, Urdu, Dari, Zentralkurdisch, Sindhi, Punjabi (Pakistan) und Uigurisch.

Dieses Thema enthält die Design-Philosophie von Windows BiDi und Fallstudien mit Überlegungen zum BiDi-Design.

## <a name="bidi-design-elements"></a>Bidirektionale Gestaltungselemente

Die BiDi-Gestaltung in Windows wird von 4 Prinzipien bestimmt.

- **Spiegeln der Benutzeroberfläche (UI)**. Der Benutzeroberflächenfluss ermöglicht die Darstellung von Inhalten mit Leserichtung von rechts nach links in einem einheitlichen Layout. Die Benutzeroberfläche für Länder mit BiDi-Sprachen orientiert sich an den örtlichen Konventionen.
- **Einheitliche Benutzererfahrung**. Ein Design wird in der Ausrichtung von rechts nach links als natürlich empfunden. Die Elemente der Benutzeroberfläche weisen eine einheitliche Layoutrichtung auf und werden so dargestellt, wie der Benutzer dies erwartet.
- **Optimierung der Fingereingabe**. Wie in nicht gespiegelten Benutzeroberflächen sind alle Elemente leicht zugänglich und ermöglichen eine natürliche Interaktion per Toucheingabe.
- **Unterstützung für gemischten Text**. Die Unterstützung verschiedener Textausrichtungen erlaubt eine weitgehende Mischung von Textrichtungen (deutscher Text in BiDi-Builds und umgekehrt).

## <a name="feature-design-overview"></a>Überblick über die Gestaltungselemente

Windows unterstützt vier BiDi-Gestaltungselemente. Wir betrachten nun einige der wichtigsten Features in Windows und erläutern, wie sie sich auf Ihre App auswirken.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Navigieren in die gewohnte Richtung

Windows passt die Richtung des typografischen Rasters so an, dass es von rechts nach links verläuft, d. h. die erste Kachel im Raster befindet sich in der oberen rechten Ecke und die letzte Kachel unten links. Dies entspricht dem RTL-Muster von gedruckten Publikationen wie Büchern und Zeitschriften. Darin wird auch von rechts oben nach links unten gelesen.

![Menü "start" BiDi](images/56283_BIDI_01_startscreen_resized.png)
![BiDi Menü "start" mit Charms](images/56283_BIDI_02_startscreen_charm_resized.png)

Um einen einheitlichen Benutzeroberflächenfluss sicherzustellen, behalten die Kachelinhalte im Raster immer ihre Ausrichtung von rechts nach links bei. Der Name Ihrer App und das Logo werden daher unabhängig von der Sprache der Benutzeroberfläche Ihrer App immer rechts unten auf der Kachel angezeigt.

#### <a name="bidi-tile"></a>BiDi-Kachel

![BiDi-Kachel](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Englische Kachel

![Englische Kachel](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Ordnungsgemäß zu lesende Kachelbenachrichtigungen

Kacheln unterstützen gemischte Texte. Der Benachrichtigungsbereich weist eine integrierte Flexibilität auf, sodass die Textausrichtung an die jeweils verwendete Sprache angepasst werden kann.  Wenn eine App Benachrichtigungen in einer BiDi-Sprache wie Arabisch oder Hebräisch sendet, wird der Text rechts ausgerichtet. Und wenn eine Benachrichtigung in Englisch (oder einer anderen LTR-Sprache) eingeht, wird sie am linken Rand ausgerichtet.

![Kachelbenachrichtigungen](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Eine konsistente, per Toucheingabe einfach zu bedienende RTL-Benutzererfahrung

Alle Elemente der Benutzeroberfläche von Windows sind mit der RTL-Ausrichtung kompatibel. Charms und Flyouts wurden auf der linken Seite des Bildschirms platziert, um das Überlappen von Suchergebnissen oder eine Beeinträchtigung der Toucheingabe zu vermeiden. Sie sind leicht mit dem Daumen erreichbar.

![Bildschirmabbildung von BiDi](images/56286_BIDI_05_search_flyout_resized.png)
![BiDi-Screenshot](images/56286_BIDI_06_print_flyout_resized.png)

![Bildschirmabbildung von BiDi](images/56286_BIDI_07_settings_flyout_resized.png)
![BiDi-Screenshot](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Texteingabe in beliebiger Richtung

Windows enthält eine kompakte und übersichtliche Bildschirmtastatur. Für BiDi-Sprachen kann die Texteingaberichtung über eine entsprechende Steuerungstaste nach Bedarf umgeschaltet werden.

![Bildschirmtastatur für BiDi-Sprachen](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Jede App in jeder Sprache verwenden

Installieren und verwenden Sie Ihre bevorzugten Apps in jeder beliebigen Sprache. Die Apps funktionieren genau so wie in Nicht-BiDi-Versionen von Windows und werden auch entsprechend dargestellt Elemente in Apps werden stets einheitlich an vorhersehbaren Positionen angezeigt.

![Englische App mit BiDi-Inhalt](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Ordnungsgemäße Darstellung von Klammern

Dank der Einführung des BiDi-Klammeralgorithmus (BiDi Parenthesis Algorithm, BPA) werden Klammerpaare unabhängig von den Sprach- oder Textausrichtungseigenschaften stets korrekt dargestellt.

#### <a name="incorrect-parentheses"></a>Falsche Klammern

![BiDi-App mit falschen Klammern](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Korrekte Klammern

![BiDi-App mit korrekten Klammern](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Typografie

Windows verwendet die Schrift „Segoe UI” für alle BiDi-Sprachen. Diese Schrift wird für die Windows-Benutzeroberfläche angepasst und skaliert.

![Segoe UI-Schriftart für BiDi-Sprachen](images/56290_BIDI_13_start_screen_segoe.png)
![Segoe UI-Schriftart für BiDi-Sprachen](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>#1-Fallstudie: Eine BiDi-Musik-app

### <a name="overview"></a>Übersicht

Multimediale Apps stellen eine interessante Herausforderung hinsichtlich der Gestaltung dar. Von den Steuerelementen für entsprechende Medien wird i. d. R. ein Layout mit ähnlicher Ausrichtung (links-nach-rechts) wie bei Nicht-BiDi-Sprachen erwartet.

![Mediensteuerelemente (links nach rechts)](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Mediensteuerelemente (rechts nach links)](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Festlegen der Ausrichtung von Elementen der Benutzeroberfläche

Die Beibehaltung der Rechts-nach-links-Ausrichtung auf der Benutzeroberfläche ist im Hinblick auf die BiDi-Märkte für eine einheitliche Gestaltung von großer Bedeutung. Das Hinzufügen von Elementen mit Links-nach-rechts-Fluss in diesem Kontext ist schwierig, da einige Navigationselemente wie die Zurück-Schaltfläche unter Umständen der direktionalen Ausrichtung der Zurück-Schaltfläche in den Audiosteuerelementen widerspricht.

![Titelseite der Musik-App](images/56292_BIDI_16_app_layout_callouts_resized.png)

Die Musik-App von Microsoft übernimmt die Rechts-nach-links-Ausrichtung des Rasters. Dadurch wirkt die App für Benutzer, die diese Navigationsrichtung bereits auf der Windows-Benutzeroberfläche verwenden, sehr natürlich. Bei der Beibehaltung des Flusses wird nicht nur dafür gesorgt, dass die Hauptelemente von links nach rechts angeordnet werden, sondern es wird auch auf eine ordnungsgemäße Ausrichtung in den Abschnittsüberschriften geachtet, um eine fließende Verwendung der Benutzeroberfläche zu ermöglichen.

![Musik-App: Albumseite](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Textbehandlung

Die Biografie des Künstlers im Screenshot oben wird linksbündig dargestellt. Andere auf den Künstler bezogene Textelemente wie der Name des Albums und die Namen der Einzeltitel werden weiterhin rechtsbündig dargestellt. Das Feld mit der Biografie ist ein relativ großes Textelement, das schlecht lesbar ist, wenn es nach rechts ausgerichtet ist, weil es beim Lesen eines breiteren Textblocks schwierig ist, die Orientierung zu behalten Allgemein gilt: Jedes Textelement mit mehr als zwei oder drei Zeilen und mindestens fünf Wörtern pro Zeile kann ähnlichen Ausnahmen bezüglich der Ausrichtung unterworfen sein, sodass die Ausrichtung der Textblöcke nicht der allgemeinen Layoutausrichtung der App entspricht.

Die Änderung der Ausrichtung innerhalb der App mag recht unkompliziert wirken, offenbart aber häufig einige Einschränkungen und Schwachstellen der verschiedenen Renderingmodule bei der neutralen Zeichenplatzierung in BiDi-Zeichenfolgen. So wird möglicherweise die folgende Zeichenfolge je nach Ausrichtung unterschiedlich dargestellt.

| | Englische Zeichenfolge (LTR) | Hebräische Zeichenfolge (RTL) |
| -------------- | ------------------- | ------------------- |
| **Left-alignment** | Hello, World! | בוקר טוב! |
| **Right-alignment** | !Hello, World | !בוקר טוב |

Um sicherzustellen, dass die Informationen zum jeweiligen Künstler in der Musik-App fehlerfrei angezeigt werden, wurden die Eigenschaften für das Textlayout und die Ausrichtung vom Entwicklungsteam getrennt behandelt. Mit anderen Worten: Die Informationen über den Künstler werden zwar möglicherweise in vielen Fällen rechtsbündig dargestellt, die Layoutausrichtung wird jedoch auf der Grundlage einer angepassten Hintergrundverarbeitung festgelegt. Die Hintergrundverarbeitung bestimmt die auf Basis des Zeichenfolgeninhalts das am besten geeignete Ausrichtungslayout.

![Musik-App: Künstlerseite](images/56292_BIDI_18_app_layout_callouts_resized.png)

Beispiel: Ohne benutzerdefinierte Verarbeitung des Zeichenfolgenlayouts würde der Interpretenname „The Contoso Band.” dargestellt als „.The Contoso Band”.

### <a name="specialized-string-direction-preprocessing"></a>Spezielle Verarbeitung der Zeichenfolgenausrichtung

Wenn die App Medienmetadaten vom Server abruft, wird jede einzelne Zeichenfolge zunächst verarbeitet, bevor der Benutzer sie sehen kann. Bei dieser Vorverarbeitung findet auch eine Umwandlung statt, um für eine einheitliche Textrichtung zu sorgen. Hierzu prüft die App, ob am Ende der Zeichenfolge neutrale Zeichen stehen. Und wenn die Textausrichtung der Zeichenfolge nicht der durch die Windows-Spracheinstellungen festgelegten Richtung für die App entspricht, werden Unicode-Richtungsmarker an die Zeichenfolge angefügt (und manchmal auch vorangestellt). Die Transformationsfunktion sieht wie folgt aus:

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

Die hinzugefügten Unicode-Zeichen sind Nullbreitenzeichen und wirken sich daher nicht auf die Zeichenfolgenabstände aus. Dieser Code beeinträchtigt unter Umständen die Leistung, da die Zeichenfolge zum Erkennen der Richtung durchlaufen werden muss, bis ein nicht neutrales Zeichen gefunden wird. Da jedes Zeichen, dessen Neutralität überprüft wird, zunächst mit mehreren Unicode-Bereichen abgeglichen wird, handelt es sich um eine umfangreiche Prüfung.

## <a name="case-study-2-a-bidi-mail-app"></a>#2-Fallstudie: Eine BiDi-Mail-app

### <a name="overview"></a>Übersicht

Im Hinblick auf die Anforderungen an das Layout der Benutzeroberfläche gestaltet sich der Entwurf eines Mail-Clients relativ einfach. Die Mail-App in Windows wird standardmäßig gespiegelt. Im Hinblick auf die Textbehandlung erfordert die Mail-App eine leistungsfähigere Textanzeige und leistungsfähigere Erstellungsfunktionen, um Szenarien mit gemischter Textausrichtung behandeln zu können.

### <a name="establishing-ui-directionality"></a>Festlegen der Ausrichtung von Elementen der Benutzeroberfläche

Das Layout der Benutzeroberfläche für die Mail-App wird gespiegelt. Die Ausrichtung der drei Bereiche wurde so angepasst, dass der Ordnerbereich rechts im Bildschirm dargestellt wird. Die Liste der E-Mail-Elemente wird links davon angezeigt, gefolgt vom Bereich zum Verfassen von E-Mails.

![Gespiegelte Mail-App](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Auch die Ausrichtung zusätzlicher Elemente wurde an den allgemeinen Benutzeroberflächenfluss und die Optimierung für die Toucheingabe angepasst. Dazu gehören die App-Leiste und die Symbole zum Erstellen, Beantworten und Löschen.

![Gespiegelte Mail-App mit App-Leiste](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Textverarbeitung

#### <a name="ui"></a>UI

Texte auf der Benutzeroberfläche sind im Allgemeinen rechtsbündig ausgerichtet. Dies gilt auch für den Ordner- und Elementbereich. Der Elementbereich ist auf zwei Textzeilen (Adresse und Titel) begrenzt. Dies ist wichtig, um die Rechts-nach-links-Ausrichtung beizubehalten, ohne dass ein Textblock entsteht, der nur schwer zu lesen ist, wenn die Inhaltsrichtung nicht der Flussrichtung der Benutzeroberfläche entspricht.

#### <a name="text-editing"></a>Textbearbeitung

Bei der Textbearbeitung muss es möglich sein, Text sowohl von rechts nach links als auch von links nach rechts zu erstellen. Darüber hinaus muss das Layout zum Erstellen von Nachrichten mithilfe eines Formats wie&mdash;Rich-Text&mdash;beibehalten werden, das Ausrichtungsinformationen speichern kann.

![Mail-App (links-nach-rechts)](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Mail-App (rechts-nach-links)](images/56294_BIDI_22_email_orientation_RtL_resized.png)
