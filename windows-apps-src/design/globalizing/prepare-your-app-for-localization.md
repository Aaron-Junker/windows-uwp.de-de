---
Description: Eine lokalisierte APP ist eine, die auf anderen Märkten, Sprachen oder Regionen lokalisiert werden kann, ohne funktionale Mängel in der APP zu decken. Die wichtigste Eigenschaft einer lokalisierbaren App besteht darin, dass der ausführbare Code ordnungsgemäß von den lokalisierbaren Ressourcen getrennt wurde.
title: App lokalisierbar machen
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 8f07e7901bf89ed73087833c92b7a3ba29165fec
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493445"
---
# <a name="make-your-app-localizable"></a>App lokalisierbar machen

Eine lokalisierte APP ist eine, die für andere Märkte, Sprachen oder Regionen lokalisiert werden kann, ohne dass funktionale Mängel in der APP nicht abgedeckt werden. Die wichtigste Eigenschaft einer lokalisierbaren App besteht darin, dass der ausführbare Code ordnungsgemäß von den lokalisierbaren Ressourcen getrennt wurde. Sie sollten also festlegen, welche Ressourcen der APP lokalisiert werden müssen. Fragen Sie sich, was geändert werden muss, wenn Ihre APP für andere Märkte lokalisiert werden muss.

Außerdem wird empfohlen, sich mit den [Richtlinien für die Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md)vertraut zu machen.

## <a name="put-your-strings-into-resources-files-resw"></a>Einfügen von Zeichen folgen in Ressourcen Dateien (. resw)

Zeichen folgen Literale in Ihrem imperativen Code, XAML-Markup oder im App-Paket Manifest dürfen nicht hart codiert werden. Platzieren Sie Ihre Zeichen folgen stattdessen in Ressourcen Dateien (. resw), damit Sie unabhängig von den erstellten Binärdateien Ihrer APP an verschiedene lokale Märkte angepasst werden können. Weitere Informationen finden Sie unter Lokalisieren von Zeichen folgen [in der Benutzeroberfläche und im App-Paket Manifest](../../app-resources/localize-strings-ui-manifest.md).

In diesem Thema wird außerdem gezeigt, wie Sie Ihrer Standard Ressourcen Datei (. resw) Kommentare hinzufügen. Wenn Sie z. b. eine informelle Stimme oder einen Farbton annehmen, stellen Sie sicher, dass Sie dies in Kommentaren erläutern. Um die Kosten zu minimieren, stellen Sie außerdem sicher, dass nur die Zeichen folgen, die übersetzt werden müssen

Legen Sie die Standardsprache für Ihre APP entsprechend in der Quelldatei für das App-Paket Manifest (die `Package.appxmanifest` Datei) fest. Die Standardsprache bestimmt die Sprache, die verwendet wird, wenn die bevorzugten Sprachen des Benutzers keiner der unterstützten Sprachen Ihrer App entsprechen. Markieren Sie alle Ihre Ressourcen mit ihrer Sprache (z. b. auch die in Ihrer Standardsprache `\Assets\en-us\Logo.png` ), damit das System feststellen kann, in welcher Sprache sich die Ressource befindet und wie Sie in bestimmten Situationen verwendet wird.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Anpassen von Images und anderen Datei Ressourcen für die Sprache

Im Idealfall können Sie Ihre Bilder globalisieren &mdash; , d. h., Sie werden Kultur unabhängig machen. Für alle Images und andere Datei Ressourcen, bei denen dies nicht möglich ist, erstellen Sie beliebig viele verschiedene Varianten, die Sie benötigen, und fügen Sie die entsprechenden sprach Qualifizierer in Ihre Datei-oder Ordnernamen ein. Weitere Informationen finden Sie unter [Anpassen von Ressourcen für Sprache, Skalierung, hohen Kontrast und andere Qualifizierer](../../app-resources/tailor-resources-lang-scale-contrast.md).

Um die Lokalisierungskosten zu minimieren, sollten Sie keine Text-und Kultur abhängigen Materialien in Bilder einfügen, damit Sie beginnen können. Ein Image, das in ihrer eigenen Kultur geeignet ist, kann in anderen Kulturen anstößig oder falsch interpretiert werden. Vermeiden Sie die Verwendung kulturspezifischer Bilder, wie z. b. Postfächer, die auf der ganzen Welt nicht häufig vorkommen. Vermeiden Sie religiöse Symbole, Tiere, politische oder geschlechtsspezifische Bilder. Die Anzeige von "Fleisch", "Textteil" oder "Handgesten" kann auch ein sensitiv Thema sein. Wenn Sie nicht alle diese vermeiden können, müssen Ihre Bilder durchdacht lokalisiert werden. Wenn die lokalisierte Sprache in eine andere Leserichtung als die ursprüngliche Sprache verläuft, unterstützen symmetrische Bilder und Effekte die Spiegelung besser.

Vermeiden Sie außerdem die Verwendung von Text in Bildern und Sprache in Audiodateien und Videodateien.

## <a name="the-use-of-color-in-your-app"></a>Die Verwendung der Farbe in Ihrer APP

Beachten Sie beim Verwenden von Color. Die Verwendung von Farbkombinationen, die mit nationalen Flags oder politischen Bewegungen verknüpft sind, kann problematisch sein. Die Farbauswahl muss möglicherweise von Kulturexperten überprüft werden. Es gibt auch Probleme mit der Barrierefreiheit bei der Verwendung von Color. Wenn Sie Farbe verwenden, um Bedeutungen zu vermitteln, sollten Sie dieselben Informationen auch auf andere Weise übermitteln, z. b. Größe, Form oder Bezeichnung.

## <a name="consider-factoring-your-strings-into-sentences"></a>Berücksichtigen Sie die Umgestaltung der Zeichen folgen in Sätze.

Verwenden Sie entsprechend groß Zeichenfolgen. Kurze Zeichen folgen sind einfacher zu übersetzen, und Sie ermöglichen die Übersetzung der Übersetzung (wodurch Kosten gespart werden, da dieselbe Zeichenfolge mehrmals nicht an den Lokalisierer gesendet wird). Außerdem werden extrem lange Zeichen folgen möglicherweise nicht von Lokalisierungstools unterstützt.

Aber im Spannungsverhältnis zu dieser Richtlinie besteht das Risiko, eine Zeichenfolge in verschiedenen Kontexten wiederzuverwenden. Auch einfache Wörter wie &quot; on &quot; und &quot; Off &quot; können je nach Kontext anders übersetzt werden. Im Englischen können sich „on“ und „off“ auf das Ein- und Ausschalten von Flugzeugmodus, Bluetooth oder Geräten beziehen. Im Italienischen hängt die Übersetzung jedoch davon ab, was ein- und ausgeschaltet wird. Sie müssten für jeden Kontext ein Zeichenfolgenpaar erstellen. Sie können Zeichenfolgen im gleichen Kontext mehrmals verwenden. Sie können z. B. die Zeichenfolge „Volume“ sowohl für Soundeffekte als auch für die Lautstärke von Musik verwenden, da sich beides auf die Intensität von Tönen bezieht. Verwenden Sie dieselbe Zeichenfolge jedoch nicht für Festplattenvolumes, da sich hier Kontext und Bedeutung unterscheiden und das Wort anders übersetzt wird.

Zudem können Zeichenfolgen wie „text“ oder „fas“ in der englischen Sprache als Verb und als Substantiv verwendet werden, was die Übersetzung erschweren kann. Erstellen Sie deshalb jeweils eine getrennte Zeichenfolge für das Verb und das Substantiv. Falls unklar ist, ob es sich um den gleichen Kontext handelt, gehen Sie auf Nummer sicher, und verwenden Sie getrennte Zeichenfolgen.

Kurz gesagt: gestalten Sie Ihre Zeichen folgen in Teile, die in allen Kontexten funktionieren. Es gibt Fälle, in denen eine Zeichenfolge einen vollständigen Satz aufweisen muss.

Beachten Sie die folgende Zeichenfolge: "die {0} konnte nicht synchronisiert werden."

Eine Vielzahl von Wörtern könnte ersetzen {0} , z. b. "Termin", "Aufgabe" oder "Dokument". Obwohl dieses Beispiel für die englische Sprache funktioniert, funktioniert es nicht in allen Fällen für den entsprechenden Satz in, z. b. Deutsch. Sie sehen, dass in den folgenden deutschen Sätzen einige der Wörter in der Vorlagenzeichenfolge („Der“, „Die“, „Das“) zum parametrisierten Wort passen müssen:

| Englisch                                    | Deutsch                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

Ein weiteres Beispiel ist der Satz "Erinnerung an mich in {0} Minute (n)". Die Verwendung von "Minute (n)" funktioniert in englischer Sprache, aber in anderen Sprachen können andere Begriffe verwendet werden. Auf Polnisch heißt es je nach Kontext „minuta“, „minuty“ oder „minut“.

Lokalisieren Sie den gesamten Satz, statt nur ein einzelnes Wort, um das Problem zu beheben. Auf den ersten Blick wirkt diese Lösung vielleicht nicht ganz so elegant und sieht nach überflüssiger Arbeit aus, die Gründe sprechen jedoch für sich:

- Für alle Sprachen wird eine entsprechende Meldung angezeigt.
- Der Konvertierer muss nicht gefragt werden, durch welche Zeichen folgen die Zeichen folgen ersetzt werden.
- Sie müssen keine kostspielige Codefehlerbehebung implementieren, wenn ein solches Problem in der fertigen App auftaucht.

## <a name="other-considerations-for-strings"></a>Weitere Überlegungen zu Zeichen folgen

Vermeiden Sie in den Zeichen folgen, die Sie in Ihrer Standardsprache erstellen, Umgangs-und-Metaphern. Die Sprache für eine demografische Gruppe, z. b. Kultur und Alter, kann schwer zu verstehen oder zu übersetzen sein, da nur Personen in dieser demografischen Gruppe diese Sprache verwenden. Ebenso ergeben Metaphern nicht für alle Personen Sinn. Beispielsweise hat &quot;Bache&quot; (weibliches Wildschwein) für Jäger eine besondere, für andere Personen jedoch keine Bedeutung.

Verwenden Sie keinen technischen Jargon, Abkürzungen oder Akronyme. Fachsprache wird von Fachfremden kaum verstanden und ist schwer zu übersetzen. Im alltäglichen Sprachgebrauch ist diese Terminologie unüblich. Die technische Sprache wird häufig in Fehlermeldungen angezeigt, um Hardware-und Software Probleme zu identifizieren. Sie sollten jedoch nur dann technische Zeichen folgen haben, *Wenn der Benutzer diese Informationsebene benötigt und entweder eine Aktion durchgeführt oder eine beliebige Person finden kann*.

Die Verwendung einer informellen Stimme oder eines Klangs in ihren Zeichen folgen ist eine gültige Wahl. Sie können Kommentare in der Standard Ressourcen Datei (. resw) verwenden, um diese Absicht anzugeben.

## <a name="pseudo-localization"></a>Pseudo Lokalisierung

Pseudo Lokalisierung Ihrer APP, um Lokalisier barkeits Probleme zu erkennen. Bei der Pseudo Lokalisierung handelt es sich um eine Art von Lokalisierung und einen Offenlegungs Test. Sie können einen Satz von Ressourcen entwickeln, die nicht wirklich übersetzt werden. Sie sehen nur auf diese Weise. Die Zeichen folgen sind ungefähr 40% länger als in der Standardsprache, und Sie verfügen über Trennzeichen, damit Sie auf einen Blick sehen können, ob Sie auf der Benutzeroberfläche abgeschnitten wurden.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Wenn Sie eine App installieren, die lokalisierte Sprach Daten enthält, werden Sie möglicherweise feststellen, dass nur die Standardsprache für die app verfügbar ist, auch wenn Sie anfänglich Ressourcen für mehrere Sprachen eingefügt haben. Dies liegt daran, dass der Installationsvorgang so optimiert ist, dass nur Sprachressourcen installiert werden, die der aktuellen Sprache und Kultur des Geräts entsprechen. Wenn Ihr Gerät für en-US konfiguriert ist, werden daher nur die Sprachressourcen von en-US mit Ihrer APP installiert.

> [!NOTE]
> Es ist nicht möglich, nach der Erstinstallation zusätzliche Sprachunterstützung für Ihre APP zu installieren. Wenn Sie nach der Installation einer APP die Standardsprache ändern, verwendet die APP weiterhin nur die ursprünglichen Sprachressourcen.

Wenn Sie sicherstellen möchten, dass nach der Installation alle Sprachressourcen verfügbar sind, erstellen Sie eine Konfigurationsdatei für das App-Paket, mit der angegeben wird, dass während der Installation bestimmte Ressourcen erforderlich sind (einschließlich der Sprachressourcen). Diese optimierte Installationsfunktion wird automatisch aktiviert, wenn die appxbundle-Datei Ihrer Anwendung während der Paket Erstellung generiert wird. Weitere Informationen finden Sie unter [sicherstellen, dass Ressourcen auf einem Gerät installiert sind, unabhängig davon, ob Sie von einem Gerät benötigt werden](https://docs.microsoft.com/previous-versions/dn482043(v=vs.140)).

Optional können Sie die. appxbundle-Generierung deaktivieren, wenn Sie Ihre APP packen, um sicherzustellen, dass alle Ressourcen installiert sind (nicht nur eine Teilmenge). Dies wird jedoch nicht empfohlen, da dadurch die Installationszeit der APP erhöht werden kann.

Deaktivieren Sie die automatische Generierung der appxbundle-Datei, indem Sie das Attribut "App-Bündel generieren" auf "nie" festlegen:

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Projektnamen.
2. Wählen Sie **Store**  ->  **App-Pakete erstellen... aus.**
3. Wählen Sie im Dialogfeld **Pakete erstellen** die Option **Ich möchte Pakete zum Hochladen in die Microsoft Store mit einem neuen APP-Namen erstellen** aus, und klicken Sie dann auf **weiter**.
4. Wählen Sie im Dialogfeld **APP-Namen auswählen** /erstellen einen APP-Namen für das Paket aus.
5. Legen Sie im Dialogfeld **Pakete auswählen und konfigurieren** die Option **App Bundle generieren** auf **nie**fest.

## <a name="geopolitical-awareness"></a>Geopolitisches Bewusstsein

Vermeiden Sie politische Beleidigungen in Karten oder bei Verweisen auf Regionen. Zuordnungen können umstrittene regionale oder nationale Grenzen umfassen und sind eine häufige Quelle für politische Beleidigung. Achten Sie darauf, dass die Benutzeroberfläche für die Auswahl von Nationen die Formulierung &quot;Land/Region&quot; verwendet. Wenn Sie ein umstrittenes Gebiet in einer Liste mit der Bezeichnung &quot; Länder &quot; &mdash; wie z. b. in einem Adressformular auflisten, &mdash; kann dies einige Benutzer beleidigen

## <a name="language--and-region-changed-events"></a>Sprach-und Regions geänderte Ereignisse

Abonnieren von Ereignissen, die ausgelöst werden, wenn sich die Sprach-und Regions Einstellungen des Systems ändern. Führen Sie diese Schritte aus, damit Sie ggf. Ressourcen erneut laden können. Weitere Informationen finden Sie unter Aktualisieren von Zeichen folgen als [Reaktion auf Änderungs Ereignisse für Qualifizierer](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) und [Aktualisieren von Bildern als Reaktion auf Änderungs Ereignisse von Qualifiziererwerten](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Beim Formatieren von Zeichen folgen die richtige Parameterreihenfolge

Gehen Sie nicht davon aus, dass alle Sprachen Parameter in derselben Reihenfolge Ausdrücken. Nehmen Sie beispielsweise an, dass dieses Format vorliegt.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

Die Format Zeichenfolge in diesem Beispiel funktioniert in englischer Sprache (USA). Dies ist beispielsweise für Deutsch (Deutschland) nicht geeignet, wenn der Tag und der Monat in umgekehrter Reihenfolge angezeigt werden. Stellen Sie sicher, dass der Übersetzer die Absicht der einzelnen Parameter kennt, damit Sie die Reihenfolge der Format Elemente in der Format Zeichenfolge (z. b {1} {0} . "") entsprechend der Zielsprache umkehren können.

## <a name="dont-over-localize"></a>Nicht über lokalisieren

Nur in natürlicher Sprache an Übersetzer senden weder Programmiersprache noch Markup. Ein `<link>` Tag ist keine natürliche Sprache. Beachten Sie diese Beispiele.

| Nicht lokalisieren                   | Lokalisieren |
|:--------------------------------------- |:-------------------------- |
| &lt;Verknüpfen &gt; der Nutzungsbedingungen &lt; /Link&gt;   | Nutzungsbedingungen               |
| &lt;Verknüpfen der &gt; Datenschutzrichtlinie &lt; /Link&gt; | Datenschutzrichtlinie             |

`<link>`Wenn Sie das-Tag in ihre Ressourcen Datei (. resw) einschließen, wird es wahrscheinlich auch übersetzt. Dies würde das Tag als ungültig erweisen. Wenn Sie über lange Zeichen folgen verfügen, die Markup einschließen müssen, um den Kontext beizubehalten und die Reihenfolge zu gewährleisten, müssen Sie in den Kommentaren klar werden, was nicht übersetzt werden soll.

## <a name="choose-an-appropriate-translation-approach"></a>Auswählen eines geeigneten Übersetzungs Ansatzes

Nachdem die Zeichenfolgen in Ressourcendateien untergliedert wurden, können sie übersetzt werden. Der ideale Zeitpunkt für die Übersetzung ist nach Fertigstellung der Zeichenfolgen im Projekt, was in der Regel gegen Projektende der Fall ist. Der Übersetzungsprozess kann auf unterschiedliche Weise gehandhabt werden. Die Vorgehensweise hängt vom Umfang der zu übersetzenden Zeichenfolgen ab, von der Anzahl der zu übersetzenden Sprachen und wie die Übersetzung erfolgt (intern oder über externe Auftragnehmer).

Berücksichtigen Sie diese Optionen.

- **Die Ressourcendateien können zum Übersetzen direkt im Projekt geöffnet werden.** Diese Vorgehensweise eignet sich gut für ein Projekt mit einer kleinen Anzahl von Zeichen folgen, die in zwei oder drei Sprachen übersetzt werden müssen. Er könnte sich für Szenarien eignen, in denen ein Entwickler mehrere Sprachen spricht und die Übersetzung übernehmen kann. Diese Vorgehensweise hat den Vorteil, dass Sie schnell ist, keine Tools erfordert und das Risiko von mierungen minimiert. Es ist jedoch nicht skalierbar. Insbesondere kann es passieren, das die Ressourcen in verschiedenen Sprachen nicht mehr synchron sind, was eine mangelnde Benutzerfreundlichkeit und Verwaltungsprobleme zur Folge haben kann.
- **Die Zeichen folgen-Ressourcen Dateien befinden sich im XML-oder resjson-Textformat und können daher mithilfe eines beliebigen Text-Editors für die Übersetzung übergeben werden. Die übersetzten Dateien werden dann wieder in das Projekt kopiert.** Bei diesem Ansatz besteht die Gefahr, dass Übersetzer versehentlich die XML-Tags bearbeiten. Andererseits besteht die Möglichkeit, Übersetzungen außerhalb des Microsoft Visual Studio-Projekts durchzuführen. Dieser Ansatz eignet sich gut für Projekte, die nur in wenige Sprachen übersetzt werden. Das XLIFF-Format ist ein XML-Format, das speziell für die Lokalisierung vorgesehen ist. Es sollte zudem von einigen Lokalisierungsanbietern oder -tools unterstützt werden. Sie können mit dem [Multilingual App Toolkit](https://docs.microsoft.com/previous-versions/windows/apps/jj572370(v=win.10)) XLIFF-Dateien aus anderen Ressourcendateien wie „.resw“ oder „.resjson“ generieren.

> [!NOTE]
> Möglicherweise ist auch eine Lokalisierung für andere Ressourcen erforderlich, einschließlich Bildern und Audiodateien.

Beachten Sie auch Folgendes:

- **Lokalisierungstools** Es stehen eine Reihe von Lokalisierungstools zum Auswerten von Ressourcen Dateien zur Verfügung, sodass nur die übersetzbaren Zeichen folgen von Konvertierungsprogrammen bearbeitet werden können. Das Risiko, dass ein Übersetzer versehentlich XML-Tags bearbeitet, ist somit geringer. Der Nachteil ist aber, dass neue Tools und Prozesse in den Lokalisierungsprozess eingebunden werden müssen. Ein Lokalisierungs Tool eignet sich gut für Projekte mit einer großen Anzahl von Zeichen folgen, jedoch mit einer kleinen Anzahl von Sprachen. Weitere Informationen finden Sie unter [Verwenden des Multilingual App Toolkit](https://docs.microsoft.com/previous-versions/windows/apps/jj572370(v=win.10)).
- **Lokalisierungsanbieter** Ziehen Sie die Verwendung eines Lokalisierungs Anbieters in Erwägung, wenn Ihre Anwendung umfangreiche Zeichen folgen enthält, die in eine große Anzahl von Sprachen übersetzt werden müssen. Ein Lokalisierungsanbieter kann Sie hinsichtlich der Tools und Prozesse beraten sowie die Ressourcendateien übersetzen. Diese Lösung ist ideal, stellt allerdings auch die teuerste Option dar und kann die Bearbeitungszeit für den übersetzten Inhalt verlängern.

## <a name="keep-access-keys-and-labels-consistent"></a>Zugriffsschlüssel und Bezeichnungen konsistent halten

Die „Synchronisierung“ der für die Barrierefreiheit verwendeten Zugriffstasten mit der Anzeige der lokalisierten Zugriffstasten stellt eine besondere Herausforderung dar, da beide Zeichenfolgenressourcen in getrennten Abschnitten kategorisiert sind. Stellen Sie für die Bezeichnungszeichenfolge unbedingt Kommentare bereit wie: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Unterstützung von Furigana für japanische Zeichen folgen, die sortiert werden können

Japanische Kanji-Zeichen verfügen über mehrere Lesevorgänge (Aussprache), je nachdem, in welchem Wort Sie verwendet werden. Bei der Sortierung japanisch bezeichneter Objekte (wie Anwendungsnamen, Dateien, Lieder usw.) führt das zu Problemen. Japanische Kanji haben in der Vergangenheit normalerweise in einer Computer verständlichen Reihenfolge mit dem Namen XJIS sortiert. Da es sich hierbei um keine phonetische Sortierung handelt, ist sie für menschliche Nutzer leider nicht hilfreich.

*Furigana* umgeht dieses Problem, indem es dem Benutzer oder dem Ersteller ermöglicht, die Phonetik für die von Ihnen verwendeten Zeichen anzugeben. Wenn Sie das folgende Verfahren zum Hinzufügen von Furigana zum APP-Namen verwenden, können Sie sicherstellen, dass es an der richtigen Stelle in der APP-Liste sortiert ist. Wenn Ihr App-Name Kanji-Zeichen enthält und Furigana nicht bereitgestellt wird, wenn die Benutzeroberflächen Sprache des Benutzers oder die Sortierreihenfolge auf Japanisch festgelegt ist, macht Windows den besten Aufwand, die entsprechende Aussprache zu generieren. Es ist jedoch möglich, App-Namen mit seltener oder spezieller Schreibweise stattdessen nach einer gebräuchlicheren Schreibweise zu sortieren. Daher besteht die bewährte Vorgehensweise für japanische Anwendungen (insbesondere solche, die Kanji-Zeichen in ihren Namen enthalten) darin, eine Furigana-Version Ihres App-namens als Teil des japanischen Lokalisierungsprozesses bereitzustellen.

1. Fügen Sie „ms-resource:Appname“ als Paketanzeigenamen und Anwendungsanzeigenamen hinzu.
2. Erstellen Sie unter „strings“ den Ordner „ja-JP“, und fügen Sie zwei Ressourcendateien wie folgt hinzu:

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3. Unter „Resources.resw“ für „ja-JP“ allgemein: Fügen Sie eine Zeichenfolgenressource für den App-Namen „希蒼“ hinzu.
4. In Resources. altform-MSFT-Phonetic. resw für japanische Furigana-Ressourcen: Hinzufügen eines Furigana-Werts für appname "のあ"

Der Benutzer kann mithilfe des Furigana-Werts "のあ" (Noa) und des phonetischen Werts (mithilfe der **GetPhonetic** -Funktion aus dem Eingabemethoden-Editor (IME)) "まれあお" (Stute-AO) nach dem APP-Namen "希蒼" suchen.

Die Sortierung folgt dem **regionalen Format der Systemsteuerung**:

- Unter einem japanischen Benutzer Gebiets Schema
  - Wenn Furigana aktiviert ist, wird "希蒼" unter "の" sortiert.
  - Wenn Furigana fehlt, wird "希蒼" unter "ま" sortiert.
- Unter einem nicht japanischen Benutzer Gebiets Schema
  - Wenn Furigana aktiviert ist, wird "希蒼" unter "の" sortiert.
  - Wenn Furigana fehlt, wird "希蒼" unter "漢字" sortiert.

## <a name="related-topics"></a>Verwandte Themen

- [Richtlinien für Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md)
- [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
- [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)
- [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md)
- [Aktualisieren von Bildern als Reaktion auf Änderungs Ereignisse für qualifiziererwert](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Beispiele

- [Anwendungsressourcen und Lokalisierung – Beispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208))
