---
Description: Eine lokalisierte App kann in anderen Märkten, Sprachen oder Regionen lokalisiert werden, ohne dass funktionale Mängel in der App auftreten. Die wichtigste Eigenschaft einer lokalisierbaren App ist, dass ihr ausführbarer Code sauber von den lokalisierbaren Ressourcen der App getrennt ist.
title: App lokalisierbar machen
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 341d46879895da221e3a17ba88f28fd22e7c5e27
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258097"
---
# <a name="make-your-app-localizable"></a>App lokalisierbar machen

Eine lokalisierte App kann in anderen Märkten, Sprachen oder Regionen verwendet werden, ohne dass funktionale Mängel in der App auftreten. Die wichtigste Eigenschaft einer lokalisierbaren App ist, dass ihr ausführbarer Code sauber von den lokalisierbaren Ressourcen der App getrennt ist. Daher sollten Sie ermitteln, welche Ressourcen Ihrer App lokalisiert werden müssen. Überlegen Sie, was bei einer Lokalisierung für andere Märkte geändert werden muss.

Wir empfehlen außerdem, dass Sie sich mit den [Richtlinien für Globalisierung](guidelines-and-checklist-for-globalizing-your-app.md) vertraut machen.

## <a name="put-your-strings-into-resources-files-resw"></a>Zeichenfolgen nur in Ressourcendateien (.resw)

Don't hard-code string literals in your imperative code, XAML markup, nor in your app package manifest. Speichern Sie Ihre Zeichenfolgen stattdessen in Ressourcendateien (.resw), damit sie unabhängig von den Binärdateien Ihrer App-Builds an unterschiedliche lokale Märkte angepasst werden können. Details dazu finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im Paketmanifest der App](../../app-resources/localize-strings-ui-manifest.md).

In diesem Thema erfahren Sie außerdem, wie Sie Kommentare zu Ihrer Standard-Ressourcendatei (.resw) hinzufügen. Zum Beispiel können Sie informelle Anmerkungen in Kommentaren angeben. Um die Kosten zu minimieren, sollten Sie außerdem sicherstellen, dass nur die Zeichenfolgen, die übersetzt werden müssen, an die Übersetzer weitergegeben werden.

Legen Sie die Standardsprache für Ihre App in der Quelldatei des App-Paketmanifests (die `Package.appxmanifest`-Datei) entsprechend fest. Die Standardsprache wird verwendet, wenn keine der bevorzugten Sprachen des Nutzers eine der von Ihrer App unterstützten Sprache ist. Markieren Sie alle Ihre Ressourcen mit ihrer Sprache (auch die in Ihrer Standardsprache, zum Beispiel `\Assets\en-us\Logo.png`), damit das System feststellen kann, in welcher Sprache die Ressource vorliegt und wie sie in bestimmten Situationen zu verwenden ist.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Anpassen von Bild- und anderen Dateiressourcen an eine Sprache

Im Idealfall es ist möglich, Bilder zu globalisieren, sodass sie kulturunabhängig sind. Erstellen Sie für alle Bild- und andere Dateiressourcen, bei denen dies nicht möglich ist, so viele verschiedene Varianten, wie Sie benötigen, und fügen Sie die entsprechenden Sprachqualifizierer in ihre Datei- oder Ordnernamen ein. Weitere Informationen über Ressourcenqualifizierer finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und andere Eigenschaften](../../app-resources/tailor-resources-lang-scale-contrast.md)).

Um die Lokalisierungskosten zu minimieren, sollten Sie zunächst keine Texte oder kulturabhängiges Material in Bilder einfügen. Bilder, die in Ihrer Kultur angemessen sind, könnten in anderen Kulturen als anstößig empfunden werden. Vermeiden Sie kulturspezifische Bilder wie Briefkästen, die nicht weltweit verwendet werden. Vermeiden Sie religiöse Symbole, Tiere sowie politische oder geschlechtsspezifische Bilder. Die Anzeige von Haut, Körperteilen oder Gesten kann auch ein sensibles Thema sein. Wenn sich all dies nicht vermeiden lässt, müssen Ihre Bilder durchdacht lokalisiert werden. Wenn die lokalisierte Sprache in eine andere Leserichtung als die ursprüngliche Sprache verläuft, unterstützen symmetrische Bilder und Effekte die Spiegelung besser.

Vermeiden Sie auch die Verwendung von Text in Bildern sowie Sprache in Audio- und Videodateien.

## <a name="the-use-of-color-in-your-app"></a>Verwendung von Farben in einer App

Seien Sie vorsichtig bei der Verwendung von Farben. Die Verwendung von Farbkombinationen, die mit Nationalflaggen oder politischen Bewegungen verbunden sind, kann problematisch sein. Die Farbauswahl muss möglicherweise von Experten der jeweiligen Kultur überprüft werden. Bei der Verwendung von Farbe können auch Probleme mit der Zugänglichkeit auftreten. Wenn Sie Farbe verwenden, um eine Bedeutung zu vermitteln, sollten Sie diese Informationen auch auf andere Weise darstellen, z. B. durch Größe, Form oder Beschriftung.

## <a name="consider-factoring-your-strings-into-sentences"></a>Lange Zeichenfolgen aufteilen

Verwenden Sie Zeichenfolgen geeigneter Länge. Kurze Zeichenfolgen sind leichter zu übersetzen, und sie ermöglichen die Wiederverwendung von Übersetzungen (was Kosten spart, da dieselbe Zeichenfolge nur einmal an den Übersetzer gesendet wird). Zudem werden sehr lange Zeichenfolgen möglicherweise von einigen Lokalisierungstools nicht unterstützt.

Das Risiko dieser Vorgehensweise besteht darin, dass eine Zeichenfolge in verschiedenen Kontexten wiederverwendet wird. Selbst einfache Wörter wie z. B. &quot;auf&quot; und &quot;aus&quot; werden möglicherweise je nach Kontext unterschiedlich übersetzt. Im Englischen können sich „ein“ und „aus“ auf das Ein- und Ausschalten von Flugzeugmodus, Bluetooth oder Geräten beziehen. Im Italienischen hängt die Übersetzung jedoch davon ab, was ein- und ausgeschaltet wird. Sie müssten für jeden Kontext ein Zeichenfolgenpaar erstellen. Sie können Zeichenfolgen im gleichen Kontext mehrmals verwenden. Sie können z. B. die Zeichenfolge „Volume“ sowohl für Soundeffekte als auch für die Lautstärke von Musik verwenden, da sich beides auf die Intensität von Tönen bezieht. Verwenden Sie dieselbe Zeichenfolge jedoch nicht für Festplattenvolumes, da sich hier Kontext und Bedeutung unterscheiden und das Wort anders übersetzt wird.

Zudem können Zeichenfolgen wie „text“ oder „fas“ in der englischen Sprache als Verb und als Substantiv verwendet werden, was die Übersetzung erschweren kann. Erstellen Sie deshalb jeweils eine getrennte Zeichenfolge für das Verb und das Substantiv. Falls unklar ist, ob es sich um den gleichen Kontext handelt, gehen Sie auf Nummer sicher, und verwenden Sie getrennte Zeichenfolgen.

Kurz gesagt, gliedern Sie die Zeichenfolgen in Teile, die in allen Kontexten verwendbar sind. Es wird Fälle geben, in denen eine Zeichenfolge ein vollständiger Satz sein muss.

Consider the following string: "The {0} could not be synchronized."

A variety of words could replace {0}, such as "appointment", "task", or "document". Dieses Beispiel funktioniert zwar in der englischen Sprache, aber nicht in jedem Fall im entsprechenden deutschen Satz (beispielsweise). Sie sehen, dass in den folgenden deutschen Sätzen einige der Wörter in der Vorlagenzeichenfolge („Der“, „Die“, „Das“) zum parametrisierten Wort passen müssen:

| Englisch                                    | Deutsch                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

As another example, consider the sentence "Remind me in {0} minute(s)." Während „minute(s)" im Englischen passt, werden in anderen Sprachen möglicherweise unterschiedliche Begriffe verwendet. Auf Polnisch heißt es je nach Kontext „minuta“, „minuty“ oder „minut“.

Lokalisieren Sie den gesamten Satz, statt nur ein einzelnes Wort, um das Problem zu beheben. Auf den ersten Blick wirkt diese Lösung vielleicht nicht ganz so elegant und sieht nach überflüssiger Arbeit aus, die Gründe sprechen jedoch für sich:

- Für alle Sprachen wird eine grammatikalisch korrekte Meldung angezeigt.
- Übersetzer müssen nicht nachfragen, womit die Zeichenfolge ersetzt werden soll.
- Sie müssen keine kostspielige Codefehlerbehebung implementieren, wenn ein solches Problem in der fertigen App auftaucht.

## <a name="other-considerations-for-strings"></a>Andere Überlegungen für Zeichenfolgen

Vermeiden Sie umgangssprachliche Ausdrücke und Metaphern in Zeichenfolgen, die Sie in der Standardsprache zu erstellen. Eine für eine demografische Gruppe (z. B. Kultur oder Alter) spezifische Sprache ist schwer zu übersetzen, da nur Personen aus dieser demografischen Gruppe die Sprache verwenden. Ebenso ergeben Metaphern nicht für alle Personen Sinn. Beispielsweise hat &quot;Bache&quot; (weibliches Wildschwein) für Jäger eine besondere, für andere Personen jedoch keine Bedeutung.

Verwenden Sie keinen technischen Jargon, Abkürzungen oder Akronyme. Fachsprache wird von Fachfremden kaum verstanden und ist schwer zu übersetzen. Im alltäglichen Sprachgebrauch ist diese Terminologie unüblich. Technische Ausdrücke stehen oft in Fehlermeldungen, um Hardware- und Softwareprobleme zu identifizieren, jedoch sollten Sie technische Zeichenfolgen *nur verwenden, wenn der Benutzer sie in dieser Form benötigt, darauf reagieren oder einen Fachmann zur Rate ziehen kann*.

Gegen informelle Angaben in Zeichenfolgen ist nichts einzuwenden. Sie können entsprechende Kommentare in Ihrer Standard-Ressourcendatei (.resw) verwenden.

## <a name="pseudo-localization"></a>Pseudolokalisierung

Nehmen Sie ein Pseudolokalisierung Ihrer App vor, um Lokalisierungsprobleme zu erkennen. Pseudolokalisierung ist eine Art von Lokalisierungstestlauf oder Offenlegungstest. Sie erstellen eine Reihe von Ressourcen, die nicht wirklich übersetzt werden, sondern nur übersetzt aussehen. Ihre Zeichenfolgen sind beispielsweise etwa 40 % länger als in der Standardsprache, und sie enthalten Trennzeichen, sodass Sie auf einen Blick erkennen können, ob sie in der Benutzeroberfläche abgeschnitten wurden.

## <a name="deployment-considerations"></a>Deployment Considerations

When you install an app that contains localized language data, you might find that only the default language is available for the app even though you initially included resources for multiple languages. This is because the installation process is optimized to only install language resources that match the current language and culture of the device. Therefore, if your device is configured for en-US, only the en-US language resources are installed with your app.

> [!NOTE]
> It is not possible to install additional language support for your app after the initial installation. If you change the default language after installing an app, the app continues to use only the original language resources.

If you want to ensure all language resources are available after installation, create a configuration file for the app package that specifies that certain resources are required during installation (including language resources). This optimized installation feature is automatically enabled when your application's .appxbundle is generated during packaging. For more information, see [Ensure that resources are installed on a device regardless of whether a device requires them](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)).

Optionally, to ensure all resources are installed (not just a subset), you can disable .appxbundle generation when you package your app. This is not recommended however as it can increase the installation time of your app.

Disable automatic generation of the .appxbundle by setting the "Generate App Bundle" attribute to “never”:

1. In Visual Studio, right-click the project name
2. Select **Store** -> **Create app packages...**
3. In the **Create Your Packages** dialog, select **I want to create packages to upload to the Microsoft Store using a new app name** and then click **Next**.
4. In the **Select an app name** dialog, select/create an app name for your package.
5. In the **Select and Configure Packages** dialog, set **Generate app bundle** to **Never**.

## <a name="geopolitical-awareness"></a>Geopolitische Rücksichtnahme

Vermeiden Sie heikle politische Angaben in Karten oder bei Verweisen auf Regionen. Karten können umstrittene regionale oder nationale Grenzen enthalten und sind eine häufige Quelle von politischer Beleidigungen. Achten Sie darauf, dass die Benutzeroberfläche für die Auswahl von Nationen die Formulierung &quot;Land/Region&quot; verwendet. Wenn ein umstrittenes Gebiet in einer Liste mit der Bezeichnung &quot;Länder&quot; (z. B. in einem Adressformular) aufgeführt wird, kann dies zu Problemen führen.

## <a name="language--and-region-changed-events"></a>Ereignisse bei Änderung von Sprache und Region

Abonnieren Sie Ereignisse, die ausgelöst werden, wenn sich die Standardeinstellungen des Systems für Sprache und Region ändern. Dann können Sie Ressourcen bei Bedarf erneut laden. Weitere Informationen finden Sie unter [Aktualisieren von Zeichenfolgen als Reaktion auf Ereignisse, die durch die Änderung von Qualifiziererwerten ausgelöst werden](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) und [Aktualisieren von Bilder als Reaktion auf Ereignisse, die durch die Änderung von Qualifiziererwerten ausgelöst werden](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Sicherstellen der richtigen Parameterreihenfolge beim Formatieren von Zeichenfolgen

Gehen Sie nicht davon aus, dass Parameter in allen Sprachen in der gleichen Reihenfolge auftreten. Betrachten Sie z. B. dieses Format.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

Die Formatzeichenfolge in diesem Beispiel ist für Englisch (USA) richtig. Sie ist jedoch nicht für Deutsch (Deutschland) geeignet, da in dieser Sprache Tag und Monat in umgekehrter Reihenfolge angezeigt werden. Ensure that the translator knows the intent of each of the parameters so that they can reverse the order of the format items in the format string (for example, "{1} {0}") as appropriate for the target language.

## <a name="dont-over-localize"></a>Vermeiden Sie eine zu starke Lokalisierung.

Übermitteln Sie nur natürliche Sprache an Übersetzer; keine Programmiersprache oder Markup. Ein `<link>`-Tag ist keine natürlicher Sprache. Betrachten Sie die folgenden Beispiel.

| Dies nicht lokalisieren                   | Dies lokalisieren |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;Nutzungsbedingungen&lt;/link&gt;   | Nutzungsbedingungen               |
| &lt;link&gt;Datenschutzrichtlinie&lt;/link&gt; | Datenschutzrichtlinie             |

Wenn Sie das Tag `<link>` in Ihre Ressourcendatei (.resw) einfügen, wird es wahrscheinlich übersetzt. Dadurch würde das Tag ungültig. Wenn Sie lange Zeichenfolgen haben, die Markup enthalten müssen, um den Kontext zu erhalten und die Reihenfolge zu gewährleisten, machen Sie in Kommentaren deutlich, was nicht zu übersetzen ist.

## <a name="choose-an-appropriate-translation-approach"></a>Auswahl des richtigen Übersetzungsansatzes

Nachdem die Zeichenfolgen in Ressourcendateien untergliedert wurden, können sie übersetzt werden. Der ideale Zeitpunkt für die Übersetzung ist nach Fertigstellung der Zeichenfolgen im Projekt, was in der Regel gegen Projektende der Fall ist. Der Übersetzungsprozess kann auf unterschiedliche Weise gehandhabt werden. Die Vorgehensweise hängt vom Umfang der zu übersetzenden Zeichenfolgen ab, von der Anzahl der zu übersetzenden Sprachen und wie die Übersetzung erfolgt (intern oder über externe Auftragnehmer).

Berücksichtigen Sie diese Optionen.

- **The resource files can be translated by opening them directly in the project.** Dieser Ansatz funktioniert gut für ein Projekt mit wenigen Zeichenfolgen, die in zwei oder drei Sprachen übersetzt werden müssen. Er könnte sich für Szenarien eignen, in denen ein Entwickler mehrere Sprachen spricht und die Übersetzung übernehmen kann. Dieser Ansatz ist schnell, erfordert keine Tools und minimiert das Risiko von falschen Übersetzungen. Es ist jedoch nicht skalierbar. Insbesondere kann es passieren, das die Ressourcen in verschiedenen Sprachen nicht mehr synchron sind, was eine mangelnde Benutzerfreundlichkeit und Verwaltungsprobleme zur Folge haben kann.
- **The string resource files are in XML or ResJSON text format, so could be handed off for translation using any text editor. The translated files would then be copied back into the project.** Bei diesem Ansatz besteht die Gefahr, dass Übersetzer versehentlich die XML-Tags bearbeiten. Andererseits besteht die Möglichkeit, Übersetzungen außerhalb des Microsoft Visual Studio-Projekts durchzuführen. Dieser Ansatz eignet sich gut für Projekte, die nur in wenige Sprachen übersetzt werden. Das XLIFF-Format ist ein XML-Format, das speziell für die Lokalisierung vorgesehen ist. Es sollte zudem von einigen Lokalisierungsanbietern oder -tools unterstützt werden. Sie können mit dem [Multilingual App Toolkit](https://docs.microsoft.com/previous-versions/windows/apps/jj572370(v=win.10)) XLIFF-Dateien aus anderen Ressourcendateien wie „.resw“ oder „.resjson“ generieren.

> [!NOTE]
> Localization might also be necessary for other assets, including images and audio files.

You should also consider the following:

- **Localization tools** A number of localization tools are available for parsing resource files and allowing only the translatable strings to be edited by translators. Das Risiko, dass ein Übersetzer versehentlich XML-Tags bearbeitet, ist somit geringer. Der Nachteil ist aber, dass neue Tools und Prozesse in den Lokalisierungsprozess eingebunden werden müssen. Ein Lokalisierungstool eignet sich für Projekte mit vielen Zeichenfolgen für wenige Sprachen. Weitere Informationen finden Sie unter [Verwenden des Multilingual App Toolkit](https://docs.microsoft.com/previous-versions/windows/apps/jj572370(v=win.10)).
- **Localization vendors** Consider using a localization vendor if your application contains extensive strings that need to be translated into a large number of languages. Ein Lokalisierungsanbieter kann Sie hinsichtlich der Tools und Prozesse beraten sowie die Ressourcendateien übersetzen. Diese Lösung ist ideal, stellt allerdings auch die teuerste Option dar und kann die Bearbeitungszeit für den übersetzten Inhalt verlängern.

## <a name="keep-access-keys-and-labels-consistent"></a>Zugriffstasten und Beschreibungen konsistent halten

Die „Synchronisierung“ der für die Barrierefreiheit verwendeten Zugriffstasten mit der Anzeige der lokalisierten Zugriffstasten stellt eine besondere Herausforderung dar, da beide Zeichenfolgenressourcen in getrennten Abschnitten kategorisiert sind. Stellen Sie für die Bezeichnungszeichenfolge unbedingt Kommentare bereit wie: `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Furigana für sortierbare japanische Zeichenfolgen unterstützen

Japanische Kanji-Zeichen weisen die Besonderheit auf, dass sie je nach Wort und Kontext ihrer Verwendung anders ausgesprochen werden. Bei der Sortierung japanisch bezeichneter Objekte (wie Anwendungsnamen, Dateien, Lieder usw.) führt das zu Problemen. In der Vergangenheit wurde japanisches Kanji normalerweise in einer maschinenlesbaren Anordnung, XJIS genannt, sortiert. Da es sich hierbei um keine phonetische Sortierung handelt, ist sie für menschliche Nutzer leider nicht hilfreich.

*Furigana* umgeht dieses Problem, da der Benutzer oder Ersteller die Phonetik für die verwendeten Zeichen angeben kann. Wenn Sie das folgende Verfahren verwenden, um Furigana Ihrem App-Namen hinzuzufügen, können Sie sicherstellen, dass es in der App-Liste an der richtigen Stelle einsortiert ist. Wenn Ihr App-Name Kanji-Zeichen enthält und Furigana nicht bereitgestellt wird, wenn die Benutzeroberflächensprache oder die Sortierreihenfolge auf Japanisch festgelegt ist, versucht Windows, die entsprechende Aussprache zu erstellen. Es ist jedoch möglich, App-Namen mit seltener oder spezieller Schreibweise stattdessen nach einer gebräuchlicheren Schreibweise zu sortieren. Deshalb empfiehlt es sich, bei einer japanischen Anwendung (insbesondere wenn sie Kanji-Zeichen im Namen enthält) im Rahmen der japanischen Lokalisierung eine Furigana-Version des App-Namens bereitzustellen.

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
4. Unter „Resources.altform-msft-phonetic.resw“ für japanische Furigana-Ressourcen: Fügen Sie einen Furigana-Wert für den App-Namen „のあ“ hinzu.

Der Benutzer kann nach dem App-Namen „希 蒼” sowohl mit dem Furigana-Wert "の あ" (noa) als auch mit dem phonetischen Wert „ま れ あ お” (mare-ao) suchen (mit der Funktion **GetPhonetic** aus dem Eingabemethoden-Editor (IME)).

Die Sortierung folgt dem **regionalen Format der Systemsteuerung**:

- Wenn unter einem japanischen Benutzergebietsschema
  - Furigana aktiviert ist, wird „希蒼“ unter „の“ einsortiert.
  - Wenn Furigana fehlt, wird „希蒼“ unter „ま“ sortiert.
- Wenn unter einem nicht japanischem Benutzergebietsschema
  - Furigana aktiviert ist, wird „希蒼“ unter „の“ einsortiert.
  - Wenn Furigana fehlt, wird „希蒼“ unter „漢字“ sortiert.

## <a name="related-topics"></a>Verwandte Themen

- [Guidelines for globalization](guidelines-and-checklist-for-globalizing-your-app.md)
- [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
- [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)
- [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md)
- [Updating images in response to qualifier value change events](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Beispiele

- [Application resources and localization sample](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
