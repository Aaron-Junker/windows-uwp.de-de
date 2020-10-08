---
description: Entwerfen Sie Ihre APP für die Bereitstellung von bidirektionaler Textunterstützung, damit Sie Skripts von links nach rechts (LTR) und von rechts nach links (RTL)-Schreibsystemen kombinieren können, die in der Regel unterschiedliche Arten von Alphabets enthalten.
title: Entwerfen einer App für bidirektionalen Text
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung, RTL, Ltr
ms.localizationpriority: medium
ms.openlocfilehash: f09aa1ac2b56c83b502e54ce631e46d2f4054943
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829607"
---
# <a name="design-your-app-for-bidirectional-text"></a>Entwerfen einer App für bidirektionalen Text

Entwerfen Sie Ihre APP für die Bereitstellung von bidirektionaler Textunterstützung, sodass Sie Skripts von rechts nach links (RTL) und von links nach rechts (LTR)-Schreibsystemen kombinieren können, die in der Regel unterschiedliche Arten von Alphabets enthalten.

Schreibsysteme von rechts nach links, z. b. solche, die in den Regionen Naher Osten, zentral und Asien und in Afrika verwendet werden, haben besondere Entwurfs Anforderungen. Diese Schreibsysteme erfordern bidirektionale Textunterstützung. Die Bidi-Unterstützung ist die Möglichkeit, Text Layout in der Reihenfolge von rechts nach links (RTL) oder von links nach rechts (LTR) einzugeben und anzuzeigen.

In Windows sind insgesamt neun Bidi-Sprachen enthalten.
- Zwei vollständig lokalisierte Sprachen. Arabisch und Hebräisch.
- Sieben Sprachschnittstellen Pakete für neue Märkte. Persian, Urdu, Dari, zentral Kurdisch, Sindhi, Punjabi (Pakistan) und Uyghur.

Dieses Thema enthält die Windows-Bidi-Entwurfs Philosophie und Fallstudien, die Überlegungen zum Bidi-Entwurf zeigen.

## <a name="bidi-design-elements"></a>Bidi-Entwurfs Elemente

Vier Elemente beeinflussen Bidi-Entwurfsentscheidungen in Windows.

- **Benutzeroberflächen Spiegelung**. Der Benutzeroberflächen Fluss ermöglicht das darstellen von Inhalten von rechts nach links im systemeigenen Layout. Der Benutzeroberflächen Entwurf ist für einen lokalen Bidi-Markt konzipiert.
- **Konsistenz in der Benutzer**Leistung. Der Entwurf ist naturgemäß von rechts nach links. Benutzeroberflächen Elemente haben eine konsistente Layoutrichtung und werden angezeigt, wenn der Benutzer Sie erwartet.
- **Berührungs Optimierung**. Ähnlich wie bei der Touchscreen-Benutzeroberfläche in einer nicht gespiegelten Benutzeroberfläche können Elemente problemlos erreicht werden, und Sie können die Interaktion natürlich berühren.
- **Unterstützung für gemischtes Text**. Die Unterstützung von Text directionalität ermöglicht eine hervorragend gemischte Textpräsentation (englischer Text in Bidi-Builds und umgekehrt).

## <a name="feature-design-overview"></a>Übersicht über den Funktions Entwurf

Windows unterstützt die vier Bidi-Entwurfs Elemente. Sehen wir uns einige der wichtigsten wichtigen Features von Windows an, und geben Sie einen Kontext für Ihre APP an.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Navigieren Sie in der Richtung, die sich in natürlicher Richtung

Windows passt die Richtung des typografischen Rasters so an, dass Sie von rechts nach links verläuft. das bedeutet, dass die erste Kachel im Raster in der oberen rechten Ecke und die letzte Kachel unten links platziert wird. Dies entspricht dem RTL-Muster von gedruckten Veröffentlichungen, wie z. b. Büchern und Zeitschriften, bei denen das Lese Muster immer in der oberen rechten Ecke beginnt und nach links verläuft.

![Bidi-Startmenü ](images/56283_BIDI_01_startscreen_resized.png)
 ![ Bidi-Startmenü mit Charms](images/56283_BIDI_02_startscreen_charm_resized.png)

Um einen konsistenten Benutzeroberflächen Fluss beizubehalten, behalten Inhalte auf Kacheln ein Layout von rechts nach links. Dies bedeutet, dass der Name und das Logo der APP unabhängig von der Benutzeroberflächen Sprache der app in der unteren rechten Ecke der Kachel positioniert werden.

#### <a name="bidi-tile"></a>Bidi-Kachel

![Bidi-Kachel](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Englische Kachel

![Englische Kachel](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Erhalten von ordnungsgemäß gelesenen Kachel Benachrichtigungen

Kacheln verfügen über gemischte Textunterstützung. Der Benachrichtigungsbereich verfügt über integrierte Flexibilität, um die Textausrichtung basierend auf der Benachrichtigungs Sprache anzupassen.  Wenn eine APP Arabisch, Hebräisch oder andere Bidi-sprach Benachrichtigungen sendet, wird der Text rechtsbündig ausgerichtet. Wenn eine englische (oder andere LTR-) Benachrichtigung eingeht, wird Sie linksbündig ausgerichtet.

![Kachel Benachrichtigungen](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Eine konsistente, benutzerfreundliche RTL-Benutzer Darstellung

Jedes Element in der Windows-Benutzeroberfläche ist in die RTL-Ausrichtung integriert. Charms und Flyouts wurden auf der linken Seite des Bildschirms positioniert, sodass Sie sich nicht überlappen, um Suchergebnisse zu überlappen oder die Berührungs Optimierung zu beeinträchtigen. Sie können problemlos mit den Daumen erreicht werden.

![Screenshot von Bidi mit dem Screenshot mit der Größe der suchflyout-Darstellung ](images/56286_BIDI_05_search_flyout_resized.png)
 ![ von Bidi mit der Größenänderung des druckflyout](images/56286_BIDI_06_print_flyout_resized.png)

![Screenshot von Bidi mit dem Bildschirm Abbildung "Settings Flyout geändert" ](images/56286_BIDI_07_settings_flyout_resized.png)
 ![ von Bidi mit der Größenänderung der APP-leisten](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Text Eingabe in beliebiger Richtung

Windows bietet eine auf dem Bildschirm eingelegte touchtastatur, die sauber und Cluster frei ist. Für Bidi-Sprachen gibt es einen Textrichtung-Steuerelement Schlüssel, sodass die Texteingabe Richtung nach Bedarf gewechselt werden kann.

![Touchscreen-Tastatur für Bidi-Sprache](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Beliebige app in beliebiger Sprache verwenden

Installieren und verwenden Sie Ihre bevorzugten apps in einer beliebigen Sprache. Die apps werden angezeigt und funktionieren wie bei nicht-Bidi-Versionen von Windows. Elemente innerhalb von apps werden immer in einer konsistenten und vorhersagbaren Position platziert.

![Englische App mit Bidi-Inhalt](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Klammern korrekt anzeigen

Mit der Einführung des Bidi-Klammern Algorithmus (BPA) werden gekoppelte Klammern immer ordnungsgemäß angezeigt, unabhängig von den Eigenschaften der Sprache oder Textausrichtung.

#### <a name="incorrect-parentheses"></a>Falsche Klammern

![Bidi-App mit falschen Klammern](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Richtige Klammern

![Bidi-App mit korrekten Klammern](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Typografie

Windows verwendet die Segoe UI Schriftart für alle Bidi-Sprachen. Diese Schriftart ist für die Windows-Benutzeroberfläche strukturiert und skaliert.

![Ein Screenshot, der die Segoe UI Schriftart auf dem Bildschirm "Startbildschirm" anzeigt, ](images/56290_BIDI_13_start_screen_segoe.png)
 ![ die die Schriftart "Bild Arabisch" auf der Startseite anzeigt](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Fallstudie #1: eine Bidi-Musik-app

### <a name="overview"></a>Übersicht

Multimedia-apps stellen eine sehr interessante Entwurfs Herausforderung dar, da in der Regel ein Layout von links nach rechts erwartet wird, das der nicht-Bidi-Sprache ähnelt.

![Mediensteuer Elemente von links nach rechts](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Mediensteuer Elemente von rechts nach links](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Einrichten der Benutzeroberfläche

Die Beibehaltung des von rechts nach links verwendeten UI-Flusses ist wichtig für konsistentes entwerfen für Bidi-Märkte. Das Hinzufügen von Elementen, die in diesem Kontext von links nach rechts fließen, ist schwierig, da einige Navigationselemente, wie z. b. die Schaltfläche zurück, der direktionalen Ausrichtung der Schaltfläche zurück in den audiosteuerelementen widersprechen können.

![Musik-app-Titelseite](images/56292_BIDI_16_app_layout_callouts_resized.png)

Diese Musik-app behält ein Raster, das von rechts nach linksorientiert ist. Dadurch wird die APP für Benutzer, die in dieser Richtung bereits auf der Windows-Benutzeroberfläche navigieren, ein sehr natürliches Gefühl. Der Flow wird beibehalten, indem sichergestellt wird, dass die Hauptelemente nicht nur von rechts nach links geordnet sind, sondern auch in den Abschnitts Headern ordnungsgemäß ausgerichtet werden, um den Benutzeroberflächen Fluss aufrechtzuerhalten.

![Musik-app-Albumseite](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Text Verarbeitung

Die Künstlerin Bio im obigen Screenshot wird linksbündig ausgerichtet, während andere Textelemente, wie z. b. "Album" und "Track Names", die Rechte Ausrichtung beibehalten. Das Feld "Bio" ist ein recht großes Textelement, das beim Ausrichten an der rechten Seite schlecht liest, wenn es schwierig ist, zwischen den Zeilen zu verfolgen, während ein größerer TextBlock gelesen wird. Im Allgemeinen sollten alle Textelemente mit mehr als zwei oder drei Zeilen, die fünf oder mehr Wörter enthalten, für ähnliche Ausrichtungs Ausnahmen in Erwägung gezogen werden, bei denen die Text Block Ausrichtung dem gesamten App-direktionalen Layout entspricht.

Das Ändern der Ausrichtung in der APP kann einfach aussehen, aber häufig werden einige Grenzen und Einschränkungen der Rendering-Engines in Bezug auf die neutrale Zeichen Platzierung in Bidi-Zeichen folgen angezeigt. Beispielsweise kann die folgende Zeichenfolge je nach Ausrichtung unterschiedlich angezeigt werden.

| | Englische Zeichenfolge (LTR) | Hebräisch-Zeichenfolge (RTL) |
| -------------- | ------------------- | ------------------- |
| **Linke Ausrichtung** | Hello, World! | בוקר טוב! |
| **Rechte Ausrichtung** | ! Hallo Welt | !בוקר טוב |

Um sicherzustellen, dass die Informationen der Künstlerin in der Musik-app ordnungsgemäß angezeigt werden, trennt das Entwicklungsteam die Text Layout-Eigenschaften von der Ausrichtung. Anders ausgedrückt: die Informationen zu den Künstlern werden möglicherweise in vielen Fällen als rechtsbündig angezeigt, aber die Anpassung des Zeichen folgen Layouts wird basierend auf der angepassten Hintergrundverarbeitung festgelegt. Die Hintergrundverarbeitung bestimmt basierend auf dem Inhalt der Zeichenfolge die am besten direktionale Layouteinstellung.

![Musik-app-Künstlerseite](images/56292_BIDI_18_app_layout_callouts_resized.png)

Wenn z. b. ohne benutzerdefinierte zeichenfolgenlayoutverarbeitung, wird der Name des Künstlers "The The" The "The würde als angezeigt werden. Der ".

### <a name="specialized-string-direction-preprocessing"></a>Spezialisierte Richtung der Zeichen folgen Richtung

Wenn die APP den Server für Medien Metadaten kontaktiert, verarbeitet Sie jede Zeichenfolge vor der Anzeige für den Benutzer. Während dieser Vorverarbeitung führt die APP auch eine Transformation durch, um die Textrichtung konsistent zu machen. Zu diesem Zweck wird überprüft, ob an den Enden der Zeichenfolge neutrale Zeichen vorhanden sind. Wenn die Textrichtung der Zeichenfolge nicht mit der durch die Windows-Spracheinstellungen festgelegten App-Richtung zu tun hat, fügt Sie auch Unicode-Richtungs Marker (und manchmal auch vorangestellt) an. Die Transformations Funktion sieht wie folgt aus.

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

Die hinzugefügten Unicode-Zeichen weisen eine Breite von NULL auf, sodass Sie sich nicht auf den Abstand der Zeichen folgen auswirken. Dieser Code führt zu einer potenziellen Leistungs Einbuße, da das Erkennen der Zeichen folgen Richtung die Zeichenfolge bis zu einem nicht neutralen Zeichen durchlaufen muss. Jedes Zeichen, das auf die Neutralität überprüft wird, wird zunächst mit mehreren Unicode-Bereichen verglichen, sodass es keine triviale Prüfung ist.

## <a name="case-study-2-a-bidi-mail-app"></a>Fallstudie #2: eine Bidi-e-Mail-App

### <a name="overview"></a>Übersicht

Hinsichtlich der Anforderungen an die Benutzeroberflächen Layout ist ein e-Mail-Client relativ einfach zu entwerfen. Die Mail-app in Windows wird standardmäßig gespiegelt. Aus Sicht der Textverarbeitung ist es erforderlich, dass die Mail-App robustere Textanzeige-und Kompositions Funktionen hat, um gemischte Text Szenarios zu ermöglichen.

### <a name="establishing-ui-directionality"></a>Einrichten der Benutzeroberfläche

Das UI-Layout für die Mail-APP wird gespiegelt. Die drei Bereiche wurden neu ausgerichtet, sodass sich der Ordner Bereich auf der rechten Seite des Bildschirms befindet, gefolgt von der Liste der e-Mail-Element Listen auf der linken Seite und dem e-Mail-Kompositions Bereich.

![Gespiegelte Mail-App](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Zusätzliche Elemente wurden neu ausgerichtet, um dem allgemeinen Benutzeroberflächen Fluss und der touchoptimierung zu entsprechen. Hierzu gehören die APP-Leiste und die Symbole "Verfassen", "Antworten" und "Löschen".

![Gespiegelte Mail-App mit App-Leiste](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Text Verarbeitung

#### <a name="ui"></a>Benutzeroberfläche

Die Text Ausrichtung über die Benutzeroberfläche wird normalerweise rechtsbündig ausgerichtet. Dies schließt den Ordner Bereich und den Bereich Elemente ein. Der Element Bereich ist auf zwei Textzeilen ("Address" und "Title") beschränkt. Dies ist wichtig, wenn die Ausrichtung von rechts nach Links beibehalten werden soll, ohne einen TextBlock einzuführen, der schwer lesbar ist, wenn sich die Richtung des Inhalts dem Fluss der Benutzeroberflächen Richtung gegen teiliert.

#### <a name="text-editing"></a>Text Bearbeitung

Die Text Bearbeitung erfordert, dass Sie sowohl von rechts nach links als auch von links nach rechts verfassen können. Außerdem muss das Kompositions Layout in einem Format beibehalten werden, &mdash; wie z. b. Rich-Text mit &mdash; der Möglichkeit, direktionale Informationen zu speichern.

![Mail-APP von links nach rechts](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Mail-APP von rechts nach links](images/56294_BIDI_22_email_orientation_RtL_resized.png)
