---
Description: This topic defines the terms user profile language list, app manifest language list, and app runtime language list. Wir verwenden diese Begriffe in diesem Thema und in anderen Themen in diesem Featurebereich. Daher ist es wichtig zu wissen, was sie bedeuten.
title: Benutzerprofilsprachen und App-Manifest-Sprachen verstehen
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 79edf30733f7bca443c5fd12103fbd5d93909732
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258069"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Benutzerprofilsprachen und App-Manifest-Sprachen verstehen
Ein Windows-Benutzer kann mit **Einstellungen** > **Zeit und Sprache** > **und Region und Sprache** eine geordnete Liste der bevorzugten Anzeigesprachen oder nur eine bevorzugte Anzeigesprache konfigurieren. Zu einer Sprache kann es eine regionale Variante geben. Zum Beispiel können Sie u. a. ein Spanisch wählen, das entweder in Spanien, in Mexiko oder in den USA gesprochen wird.

Ebenfalls unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache**kann der Benutzer unabhängig von der Sprache seinen Standort (Region genannt) in der Welt angeben. Beachten Sie, dass die Einstellung für die Anzeigesprache (und die regionale Variante) nicht die Einstellung der Region bestimmt und umgekehrt. Zum Beispiel könnte ein Benutzer derzeit in Frankreich leben, aber als bevorzugte Windows-Anzeigesprache Spanisch (Mexiko) wählen.

Für UWP-Apps wird eine Sprache durch ein [BCP-47-Sprachtag](https://tools.ietf.org/html/bcp47) dargestellt. Das Sprachtag BCP-47 „en-US” entspricht beispielsweise Englisch (Vereinigte Staaten) in **Einstellungen**. Entsprechende UWP-APIs akzeptieren Zeichenfolgendarstellungen von BCP-47-Sprachtags und geben diese zurück.

Einzelheiten zu Sprachtags finden Sie auch unter [IANA Language Subtag Registry](https://www.iana.org/assignments/language-subtag-registry).

In den folgenden drei Abschnitten werden die Begriffe „Benutzerprofil-Sprachenliste”, „App-Manifest-Sprachenliste” und „App-Laufzeit-Sprachenliste” definiert. Wir verwenden diese Begriffe in diesem Thema und in anderen Themen in diesem Featurebereich. Daher ist es wichtig zu wissen, was sie bedeuten.

## <a name="user-profile-language-list"></a>Sprachliste für Benutzerprofile
Die Benutzerprofil-Sprachenliste ist der Name der Liste, die vom Benutzer unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Sprachen** konfiguriert wird. Im Code können Sie die Eigenschaft [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) verwenden, um auf die Benutzerprofilsprachenliste als schreibgeschützte Liste von Zeichenfolgen zuzugreifen, wobei jede Zeichenfolge ein einzelnes [BCP-47-Sprachentag](https://tools.ietf.org/html/bcp47) wie „en-US” oder „ja-JP” ist.

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Sprachenliste für App-Manifest
Die App-Manifest-Sprachenliste ist die Liste der Sprachen, deren Unterstützung in der App deklariert wird. Diese Liste wächst, wenn Sie Ihre App im Entwicklungslebenszyklus bis hin zur Lokalisierung weiterentwickeln.

Die Liste wird zur Kompilierungszeit festgelegt. Sie haben zwei Möglichkeiten, um genau zu steuern, wie dies geschehen soll. Eine Möglichkeit besteht darin, Visual Studio die Liste aus den Dateien Ihres Projekts ermitteln zu lassen. Definieren Sie dazu zunächst die **Standardsprache** für Ihre App auf der Registerkarte **Anwendung** in der Quelldatei des App-Paketmanifests (`Package.appxmanifest`). Bestätigen Sie dann, dass die gleiche Datei diese Konfiguration enthält (was standardmäßig der Fall ist).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Jedes Mal, wenn Visual Studio die Manifestdatei für das erstellte Anwendungspaket (`AppxManifest.xml`) erstellt, nimmt es dieses einzelne `Resource`-Element in der Quelldatei in eine Sammlung aller Sprachqualifizierer auf, die es in Ihrem Projekt findet (siehe [Ressourcen an Sprache, Skalierung, hohen Kontrast und andere Qualifizierer anpassen](../../app-resources/tailor-resources-lang-scale-contrast.md)). Wenn Sie beispielsweise mit der Lokalisierung begonnen haben und über Zeichenfolgen-, Bild- und/oder Dateiressourcen verfügen, deren Ordner- oder Dateinamen „en-US”, „ja-JP” und „fr-FR” enthalten, wird Folgendes in Ihrer `AppxManifest.xml`-Datei für den Build stehen (der erste Eintrag in der Liste ist die von Ihnen festgelegte Standardsprache).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

Die andere Möglichkeit ist, das einzelne „x-generate” `<Resource>`-Element in der Quelldatei Ihres App-Paketmanifests (`Package.appxmanifest`) durch die erweiterte Liste von `<Resource>`-Elementen zu ersetzen (wobei zuerst die Standardsprache aufgeführt wird). Diese Möglichkeit bedeutet mehr Wartungsarbeiten für Sie, ist jedoch möglicherweise eine geeignete Option, wenn Sie ein benutzerdefiniertes Buildsystem verwenden.

Dabei wird die Sprachenliste für Ihr App-Manifest zunächst nur eine Sprache enthalten. Beispielsweise en-US. Aber möglicherweise wird diese Liste wachsen, wenn Sie Ihr Manifest manuell konfigurieren oder übersetzte Ressourcen zu Ihrem Projekt hinzufügen.

Wenn sich Ihre App im Microsoft Store befindet, werden die Sprachen aus der Sprachenkiste des App-Manifests den Kunden angezeigt. Eine Liste der vom Microsoft Store unterstützten BCP-47-Sprachtags finden Sie unter [Unterstützte Sprachen](../../publish/supported-languages.md).

Im Code können Sie die Eigenschaft [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) verwenden, um auf die App-Manifest-Sprachenliste als schreibgeschützte Liste von Zeichenfolgen zuzugreifen, wobei jede Zeichenfolge ein einzelnes BCP-47-Sprachentag ist.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>App-Laufzeit-Sprachenliste
Die dritte Sprachenliste von Interesse ist eine Überschneidung der beiden Listen, die wir gerade beschrieben haben. Zur Laufzeit wird die Liste der Sprachen, für die Ihre App Unterstützung deklariert hat (die App-Manifest-Sprachenliste), mit der Liste der Sprachen verglichen, für die der Benutzer eine Präferenz deklariert hat (Benutzerprofil-Sprachenliste). Die Liste der App-Laufzeitsprachen wird auf die entsprechende Schnittmenge festgelegt (wenn diese nicht leer ist) oder nur auf die Standardsprache der App (wenn die Schnittmenge leer ist).

Genauer gesagt besteht die Sprachenliste für die App-Laufzeit aus diesen Elementen.

1.  **Überschreibung der primären Sprache (optional)** . [  **PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) ist eine einfache Überschreibungseinstellung für Apps, in denen die Benutzer eine eigene unabhängige Sprachauswahl treffen können, oder für Apps, bei denen die Standardsprachauswahl aus irgend einem Grund überschrieben werden sollte. Weitere Informationen dazu finden Sie unter [Anwendungsressourcen und Lokalisierung – Beispiel](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa).
2.  **Die von der App unterstützten Sprachen des Benutzers**. Dies ist die Benutzerprofil-Sprachenliste, gefiltert nach der App-Manifest-Sprachenliste. Das Filtern der Benutzersprachen anhand der von der App unterstützten Sprachen sorgt für dauerhafte Konsistenz zwischen den Software Development Kits (SDKs), Klassenbibliotheken, abhängigen Frameworkpaketen und der App.
3.  **Wenn 1 und 2 leer sind, die Standardsprache oder die erste von der App unterstützte Sprache.** Wenn die Sprachenliste des Benutzerprofils keine von der App unterstützten Sprachen enthält, ist die App-Laufzeitsprache die erste von der App unterstützte Sprache.

Im Code können Sie die Eigenschaft [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) verwenden, um auf die App-Laufzeit-Sprachenliste in Form einer Zeichenfolge zuzugreifen, die eine durch Semikolons getrennte Liste von BCP-47-Sprachvariablen enthält.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

Sie können auf die Sprachenliste auch in Form einer schreibgeschützte Liste von Zeichenfolgen zugreifen, die jeweils ein einzelnes BCP-47-Sprachtag enthalten. Dazu verwenden Sie die Eigenschaft [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) oder [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages).

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

Die Liste der App-Laufzeitsprachen bestimmt die Ressourcen, die Windows für Ihre App lädt, sowie die Sprachen, die zum Formatieren von Datum, Uhrzeit, Zahlen und anderen Komponenten verwendet werden. Siehe [Globalisieren von Datum, Uhrzeit und Zahlenformaten](use-global-ready-formats.md).

**Hinweis:** Sind die Benutzerprofilsprache und die App-Manifestsprache regionale Varianten voneinander, wird die regionale Variante des Benutzers als App-Laufzeitsprache verwendet. Wenn der Benutzer beispielsweise „en-GB” bevorzugt, die App jedoch „en-US” unterstützt, ist „en-GB ”die App-Laufzeitsprache. Dadurch wird sichergestellt, dass Datumsangaben, Uhrzeiten und Zahlen den Erwartungen des Benutzers entsprechend formatiert werden („en-GB“), die lokalisierten Ressourcen aber dank des Sprachabgleichs trotzdem in der von der App unterstützten Sprache („en-US“) geladen werden.

## <a name="qualify-resource-files-with-their-language"></a>Ressourcendateien mit Sprachqualifizierern benennen
Benennen Sie die Ressourcendateien oder ihre Ordner mit Qualifizierern für Sprachressourcen. Weitere Informationen über Ressourcenqualifizierer finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und andere Eigenschaften](../../app-resources/tailor-resources-lang-scale-contrast.md)). A resource file can be an image (or other asset), or it can be a resource container file, such as a *.resw* that contains text strings.

**Note** Even resources in your app's default language must specify the language qualifier. For example, if your app's default language is English (United States), then qualify your assets as `\Assets\Images\en-US\logo.png`.

- Windows performs complex matching, including across regional variants such as en-US and en-GB. So include the region sub-tag as appropriate. Siehe [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md).
- Specify a language script sub-tag in the qualifier when there is no Suppress-Script value defined for the language. For example, instead of zh-CN or zh-TW, use zh-Hant, zh-Hant-TW, or zh-Hans (for more detail, see the [IANA language subtag registry](https://www.iana.org/assignments/language-subtag-registry)).
- For languages that have a single standard dialect, there is no need to include the region qualifier. For example, use ja instead of ja-JP.
- Für einige Tools und Komponenten wie Übersetzungsprogramme können spezielle Sprachtags, beispielsweise Informationen zu regionalen Dialekten, für das Verständnis der Daten hilfreich sein.

### <a name="not-all-resources-need-to-be-localized"></a>Not all resources need to be localized

Localization might not be required for all resources.

- At a minimum, ensure all resources exist in the default language.
- A subset of some resources might suffice for a closely related language (partial localization). Beispielsweise muss nicht die gesamte Benutzeroberfläche einer App ins Katalanische lokalisiert werden, wenn die App über einen vollständigen Satz von Ressourcen auf Spanisch verfügt. For users who speak Catalan and then Spanish, the resources that are not available in Catalan appear in Spanish.
- Some resources might require exceptions for specific languages, while the majority of other resources map to a common resource. In this case, mark the resource intended to be used for all languages with the undetermined language tag 'und'. Windows interprets the 'und' language tag as a wildcard (similar to '\*') in that it matches the top app language after any other specific match. Wenn beispielsweise einige Ressourcen für Finnisch verschieden sind, der Rest der Ressourcen aber für alle Sprachen übereinstimmt, sollte die Ressource für Finnisch mit dem Sprachtag für Finnisch und die übrigen mit „und” markiert werden.
- For resources that are based on a language script, such as a font or height of text, use the undetermined language tag with a specified script: 'und-&lt;script&gt;'. Verwenden Sie beispielsweise `und-Latn\\fonts.css` für lateinische Schriften und `und-Cryl\\fonts.css` für kyrillische Schriften.

## <a name="set-the-http-accept-language-request-header"></a>Festlegen des HTTP-Headers „Accept-Language“.
Bedenken Sie, ob die von Ihnen aufgerufenen Webdienste im selben Umfang lokalisiert sind wie Ihre App. HTTP-Anforderungen in UWP- und Desktop-Apps sowie XMLHttpRequest (XHR) verwenden den HTTP-Standardheader „Accept-Language“. Dieser HTTP-Header wird der Liste der Benutzersprofilsprachen standardmäßig hinzugefügt. Jede Sprache in der Liste wird zudem um die regionsneutralen Varianten der Sprache sowie um eine Gewichtung (q) erweitert. Beispielsweise ergibt eine Benutzersprachenliste, die aus „fr-FR“ und „en-US“ besteht, einen„Accept-Language“-Anforderungsheader (HTTP) mit „fr-FR“, „fr“, „en-US“, „en“ („fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3“). Aber wenn Ihre App eine Benutzeroberfläche in Französisch (Frankreich) anzeigt, die erste Sprache des Benutzers in dessen Präferenzenliste jedoch Deutsch ist, müssen Sie Französisch (Frankreich) explizit vom Dienst anfordern, um in der App konsistent zu bleiben.

## <a name="apis-in-the-windowsglobalization-namespace"></a>APIs im Namespace Windows.Globalization
In der Regel verwenden die APIs im Namespace [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) die Liste der App-Laufzeitsprachen zur Bestimmung der Sprache. Hat keine der Sprachen ein übereinstimmendes Format, wird das Benutzergebietsschema verwendet. Hierbei handelt es sich um das von der Systemuhr verwendete Gebietsschema. Das Benutzergebietsschema ist unter **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Zusätzliche Datums-, Uhrzeit- und Ländereinstellungen** > **Region: Datums-, Uhrzeit- oder Zahlenformat ändern** verfügbar. Die **Windows.Globalization**-APIs akzeptieren auch eine Überschreibung, wenn Sie eine Liste mit Sprachen angeben möchten, die anstelle der App-Laufzeit-Sprachenliste verwendet werden soll.

Mithilfe der Klasse [**Language**](/uwp/api/windows.globalization.language?branch=live) können Sie die Details einer bestimmten Sprache überprüfen, wie das Skript der Sprache, den Anzeigenamen und den systemeigenen Namen.

## <a name="use-geographic-region-when-appropriate"></a>Verwenden einer geografischen Region, falls erforderlich.
In **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Land oder Region** kann der Benutzer seinen Standort in der Welt angeben. Sie können diese Einstellungen anstelle der Sprache verwenden, um auszuwählen, welcher Inhalt dem Benutzer angezeigt werden soll. Beispielsweise kann eine Nachrichten-App standardmäßig Inhalte aus dieser Region anzeigen.

Im Code kann mit der Eigenschaft [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) auf diese Einstellung zugegriffen werden.

Mithilfe der Klasse [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) können Sie Details zu einer bestimmten Region (wie Anzeigename, systemeigener Name und verwendete Währungen) überprüfen.

## <a name="examples"></a>Beispiele
Die folgende Tabelle enthält Beispiele für die Elemente, die dem Benutzer unter den verschiedenen Sprach- und Regionseinstellungen auf der Oberfläche Ihrer App anzeigt werden.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Sprachenliste für App-Manifest</th>
<th align="left">Sprachliste für Benutzerprofile</th>
<th align="left">Überschreibung der primären Sprache (optional)</th>
<th align="left">App-Laufzeit-Sprachenliste</th>
<th align="left">Anzeige in der App</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Englisch (GB) (Standard); Deutsch (Deutschland)</td>
<td align="left">Englisch (GB)</td>
<td align="left">keine</td>
<td align="left">Englisch (GB)</td>
<td align="left">UI: Englisch (GB)<br>Datum/Uhrzeit/Zahlen: Englisch (GB)</td>
</tr>
<tr>
<td align="left">Deutsch (Deutschland) (Standard); Französisch (Frankreich); Italienisch (Italien)</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">keine</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">UI: Französisch (Frankreich) (Fallback aus Französisch (Österreich))<br>Datum/Uhrzeit/Zahlen: Französisch (Österreich)</td>
</tr>
<tr>
<td align="left">Englisch (US) (Standard); Französisch (Frankreich); Englisch (GB)</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">keine</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">UI: Englisch (USA) (Fallback aus Englisch (Kanada))<br>Datum/Uhrzeit/Zahlen: Englisch (Kanada)</td>
</tr>
<tr>
<td align="left">Spanisch (Spanien) (Standard); Spanisch (Mexiko); Spanisch (Lateinamerika); Portugiesisch (Brasilien)</td>
<td align="left">Englisch (USA)</td>
<td align="left">keine</td>
<td align="left">Spanisch (Spanien)</td>
<td align="left">UI: Spanisch (Spanien) (verwendet „Standard“ da kein Fallback für Englisch verfügbar ist)<br>Datum/Uhrzeit/Zahlen Spanisch (Spanien)</td>
</tr>
<tr>
<td align="left">Katalanisch (Standard); Spanisch (Spanien); Französisch (Frankreich)</td>
<td align="left">Katalanisch; Französisch (Frankreich)</td>
<td align="left">keine</td>
<td align="left">Katalanisch; Französisch (Frankreich)</td>
<td align="left">UI: Vorwiegend Katalanisch, etwas Französisch (Frankreich), da nicht alle Zeichenfolgen in Katalanisch vorhanden sind<br>Datum/Uhrzeit/Zahlen: Katalanisch</td>
</tr>
<tr>
<td align="left">Englisch (GB) (Standard); Französisch (Frankreich); Deutsch (Deutschland)</td>
<td align="left">Deutsch (Deutschland); Englisch (GB)</td>
<td align="left">Englisch (GB) (vom Benutzer in der App-UI ausgewählt)</td>
<td align="left">Englisch (GB); Deutsch (Deutschland)</td>
<td align="left">UI: Englisch (GB) (Sprachüberschreibung)<br>Datum/Uhrzeit/Zahlen Englisch (GB)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> For a list of standard country/region codes used by Microsoft, see the [Official Country/Region List](https://globalready.azurewebsites.net/marketreadiness/OfficialCountryregion).

## <a name="important-apis"></a>Wichtige APIs
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Sprache](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [BCP-47 language tag](https://tools.ietf.org/html/bcp47)
* [IANA language subtag registry](https://www.iana.org/assignments/language-subtag-registry)
* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Supported languages](../../publish/supported-languages.md)
* [Globalize your date/time/number formats](use-global-ready-formats.md)
* [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Beispiele
* [Application resources and localization sample](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
