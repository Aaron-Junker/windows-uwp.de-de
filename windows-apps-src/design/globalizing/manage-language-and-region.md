---
description: In diesem Thema werden die Begriffe Benutzerprofil-Sprachliste, App-Manifest-Sprachliste und App-Lauf Zeit Sprache definiert. Diese Begriffe werden in diesem Thema und in anderen Themen dieses featurebereichs verwendet, daher ist es wichtig, zu wissen, was Sie bedeuten.
title: Benutzerprofilsprachen und App-Manifest-Sprachen verstehen
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: ee79eae52898bda81312fedd50e117cabeec7b3a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030763"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Benutzerprofilsprachen und App-Manifest-Sprachen verstehen
Ein Windows-Benutzer kann mit den **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprache** eine geordnete Liste von bevorzugten Anzeige Sprachen oder nur eine einzige bevorzugte Anzeige Sprache konfigurieren. Eine Sprache kann über eine regionale Variante verfügen. Sie können z. b. Spanisch als gesprochen in Spanien, Spanisch, wie in Mexiko gesprochen, Spanisch (Spanisch, wie in der USA gesprochen) auswählen.

Außerdem **Settings**  >  kann der Benutzer in den Einstellungen **Zeit & sprach**  >  **Region & Sprache** , aber separat von der Sprache angeben, seinen Standort (als "Region" bezeichnet) auf der Welt. Beachten Sie, dass die Einstellung für die Anzeige Sprache (und die regionale Variante) kein bestimmende Spalten der Regions Einstellung ist und umgekehrt. Beispielsweise kann ein Benutzer derzeit in Frankreich leben, aber eine bevorzugte Windows-Anzeige Sprache von Español (México) auswählen.

Für Windows-apps wird eine Sprache als [bcp-47-Sprachtag](https://tools.ietf.org/html/bcp47)dargestellt. Beispielsweise entspricht das bcp-47-Sprachtag "en-US" in den **Einstellungen** Englisch (USA). Geeignete Windows-Runtime-APIs akzeptieren und geben Zeichen folgen Darstellungen von bcp-47-sprach Tags zurück.

Weitere Informationen finden Sie auch in der [IANA Language Tags-Registrierung](https://www.iana.org/assignments/language-subtag-registry).

In den folgenden drei Abschnitten werden die Begriffe "Benutzerprofil-Sprachliste", "App-Manifest-Sprachliste" und "App-Lauf Zeit Sprachliste" definiert. Diese Begriffe werden in diesem Thema und in anderen Themen dieses featurebereichs verwendet, daher ist es wichtig, zu wissen, was Sie bedeuten.

## <a name="user-profile-language-list"></a>Benutzerprofil-Sprachliste
Bei der Liste Benutzerprofil Sprache handelt es sich um den Namen der Liste, die vom Benutzer in der **Einstellungs**  >  **Zeit & sprach**  >  **Region & Sprachen Sprache** konfiguriert wird  >  **Languages** . Im Code können Sie die [**globalizationpreferences. Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) -Eigenschaft verwenden, um auf die Benutzerprofil-Sprachliste als schreibgeschützte Liste von Zeichen folgen zuzugreifen, wobei jede Zeichenfolge ein einzelnes [bcp-47-Sprachtag](https://tools.ietf.org/html/bcp47) wie z. b. "en-US" oder "ja-JP" ist.

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Liste der APP-Manifest-Sprachen
Die Sprachliste des App-Manifests ist die Liste der Sprachen, für die Ihre APP die Unterstützung deklariert (oder deklariert). Diese Liste wächst, wenn Sie Ihre APP über den Entwicklungs Lebenszyklus bis hin zur Lokalisierung entwickeln.

Die Liste wird zur Kompilierzeit bestimmt, aber Sie haben zwei Möglichkeiten, um genau zu steuern, wie dies geschieht. Eine Möglichkeit besteht darin, Visual Studio die Liste der Dateien in Ihrem Projekt zu bestimmen. Legen Sie zu diesem Zweck zuerst die **Standardsprache** Ihrer APP auf der Registerkarte **Anwendung** in der Quelldatei des App-Paket Manifests ( `Package.appxmanifest` ) fest. Vergewissern Sie sich dann, dass dieselbe Datei diese Konfiguration enthält (standardmäßig).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Jedes Mal, wenn Visual Studio die erstellte App-Paket Manifest-Datei ( `AppxManifest.xml` ) erstellt, wird dieses einzelne `Resource` Element in der Quelldatei in eine Union aller sprach Qualifizierer erweitert, die es in Ihrem Projekt findet (Weitere Informationen finden Sie unter [Anpassen von Ressourcen für Sprache, Skalierung, hoher Kontrast und andere Qualifizierer](../../app-resources/tailor-resources-lang-scale-contrast.md)). Wenn Sie z. b. mit der Lokalisierung begonnen haben und Sie über Zeichen folgen-, Bild-und/oder Datei Ressourcen verfügen, deren Ordner-oder Dateinamen "en-US", "ja-JP" und "fr-FR" enthalten, enthält die integrierte `AppxManifest.xml` Datei Folgendes (der erste Eintrag in der Liste ist die Standardsprache, die Sie festlegen).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

Die andere Möglichkeit besteht darin, das einzige "x-Generate"- `<Resource>` Element in der Quelldatei des App-Paket Manifests ( `Package.appxmanifest` ) durch die erweiterte Liste von Elementen zu ersetzen `<Resource>` (achten Sie darauf, zuerst die Standardsprache aufzulisten). Diese Option erfordert mehr Wartungsarbeiten, aber es kann eine geeignete Option für Sie sein, wenn Sie ein benutzerdefiniertes Buildsystem verwenden.

Zunächst enthält die Sprachliste des App-Manifests nur eine Sprache. Vielleicht ist es auch de-US. &mdash;Wenn Sie das Manifest jedoch entweder manuell konfigurieren oder wenn Sie dem Projekt übersetzte Ressourcen hinzufügen, &mdash; wird die Liste vergrößert.

Wenn sich Ihre APP im Microsoft Store befindet, werden die Sprachen in der Sprachliste der APP-Manifeste für Kunden angezeigt. Eine Liste der bcp-47-sprach Tags, die speziell vom Microsoft Store unterstützt werden, finden Sie [unter Unterstützte Sprachen](../../publish/supported-languages.md).

Im Code können Sie die [**applicationlanguages. ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) -Eigenschaft verwenden, um auf die Sprachliste der APP-Manifeste als schreibgeschützte Liste von Zeichen folgen zuzugreifen, wobei jede Zeichenfolge ein einzelnes bcp-47-Sprachtag ist.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Sprachliste der APP-Runtime
Die dritte relevante Liste ist die Schnittmenge der beiden Listen, die wir soeben beschrieben haben. Zur Laufzeit wird die Liste der Sprachen, für die Ihre APP die Unterstützung deklariert hat (die Sprachliste des App-Manifests), mit der Liste der Sprachen verglichen, für die der Benutzer eine bevorzugte deklariert hat (die Benutzerprofil-Sprachliste). Die Sprachliste der APP-Runtime ist auf diese Schnittmenge festgelegt (wenn die Schnittmenge nicht leer ist) oder nur auf die Standardsprache der APP (wenn die Schnittmenge leer ist).

Genauer gesagt besteht die Sprachliste der APP-Laufzeit aus diesen Elementen.

1.  **(Optional) außer Kraft setzung der Primärsprache** . [**Primarylanguageoverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) ist eine einfache Außerkraftsetzungs Einstellung für apps, die Benutzern eine eigene unabhängige Sprachauswahl bieten, oder apps, die einen starken Grund zum Überschreiben der Standard Sprachauswahl haben. Weitere Informationen dazu finden Sie unter [Anwendungsressourcen und Lokalisierung – Beispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208)).
2.  **Die Sprachen des Benutzers, die von der App unterstützt werden** . Dabei handelt es sich um die Liste der Benutzerprofil Sprachen, gefiltert nach der Liste App-Manifest-Sprache. Das Filtern der Benutzersprachen anhand der von der App unterstützten Sprachen sorgt für dauerhafte Konsistenz zwischen den Software Development Kits (SDKs), Klassenbibliotheken, abhängigen Frameworkpaketen und der App.
3.  **Wenn 1 und 2 leer sind, wird die Standardsprache oder die erste Sprache von der App unterstützt** . Wenn die Benutzerprofil-Sprachliste keine Sprachen enthält, die von der App unterstützt werden, ist die APP-Lauf Zeit Sprache die erste Sprache, die von der App unterstützt wird.

Im Code können Sie die [resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) -Eigenschaft verwenden, um auf die Sprache der APP-Lauf Zeit Sprache in Form einer Zeichenfolge zuzugreifen, die eine durch Semikolons getrennte Liste von bcp-47-sprach Tags enthält.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

Sie können darauf auch als schreibgeschützte Liste von Zeichen folgen zugreifen, die jeweils ein einzelnes bcp-47-Sprachtag enthalten. Hierfür können Sie die [**resourcecontext. Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) -Eigenschaft oder die [**applicationlanguages. Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) -Eigenschaft verwenden.

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

Die Sprache der APP-Lauf Zeit Sprache bestimmt die Ressourcen, die von Windows für Ihre APP geladen werden, sowie die Sprachen, die zum Formatieren von Datumsangaben, Uhrzeiten, Zahlen und anderen Komponenten verwendet werden. Siehe [Globalisieren von Datums-/Uhrzeit-/nummerformaten](use-global-ready-formats.md).

**Hinweis** Wenn die Benutzerprofil Sprache und die APP-Manifest-Sprache Regions Varianten voneinander sind, wird die regionale Variante des Benutzers als App-Lauf Zeit Sprache verwendet. Wenn der Benutzer z. b. "en-GB" bevorzugt und die app "en-US" unterstützt, ist die APP-Lauf Zeit Sprache "en-GB". Dadurch wird sichergestellt, dass Datumsangaben, Uhrzeiten und Zahlen genauer mit den Erwartungen des Benutzers (en-GB) formatiert werden, lokalisierte Ressourcen jedoch weiterhin (aufgrund von sprach Abgleich) in der unterstützten Sprache der APP (en-US) geladen werden.

## <a name="qualify-resource-files-with-their-language"></a>Qualifizieren von Ressourcen Dateien mit ihrer Sprache
Benennen Sie Ihre Ressourcen Dateien oder deren Ordner mit Sprachressourcen Qualifizierern. Weitere Informationen zu Ressourcen Qualifizierern finden Sie unter [Anpassen von Ressourcen für Sprache, Skalierung, hohen Kontrast und andere Qualifizierer](../../app-resources/tailor-resources-lang-scale-contrast.md). Eine Ressourcen Datei kann ein Image (oder ein anderes Objekt) oder eine Ressourcen Containerdatei sein, z *. b. eine resw* -Datei, die Text Zeichenfolgen enthält.

**Hinweis** Selbst Ressourcen in der Standardsprache Ihrer APP müssen den sprach Qualifizierer angeben. Wenn die Standardsprache ihrer App z. b. Englisch (USA) ist, qualifizieren Sie Ihre Assets als `\Assets\Images\en-US\logo.png` .

- Windows führt komplexe Übereinstimmungen aus, einschließlich Regions übergreifender Varianten wie z. b. en-US und en-GB. Nehmen Sie also das untergeordnete Tag Region nach Bedarf. Erfahren Sie, [wie das Resource Management-System mit sprach Tags übereinstimmt](../../app-resources/how-rms-matches-lang-tags.md).
- Geben Sie ein sprach Skript Untertag im Qualifizierer an, wenn für die Sprache kein Suppress-Script Wert definiert ist. Verwenden Sie beispielsweise anstelle von zh-CN oder zh-tw zh-Hant, zh-Hant-TW oder zh-Hans (Weitere Informationen finden Sie in der [IANA Language Tags-Registrierung](https://www.iana.org/assignments/language-subtag-registry)).
- Für Sprachen, die über einen einzelnen Standarddialekt verfügen, ist es nicht erforderlich, den Regions Qualifizierer einzubeziehen. Verwenden Sie z. b. ja anstelle von ja-JP.
- Einige Tools und andere Komponenten, z. b. maschinelle Konvertierer, finden möglicherweise bestimmte sprach Tags, z. b. regionale Dialekt Informationen, für das Verständnis der Daten.

### <a name="not-all-resources-need-to-be-localized"></a>Nicht alle Ressourcen müssen lokalisiert werden.

Für alle Ressourcen ist möglicherweise keine Lokalisierung erforderlich.

- Stellen Sie mindestens sicher, dass alle Ressourcen in der Standardsprache vorhanden sind.
- Eine Teilmenge einiger Ressourcen genügt möglicherweise für eine eng verwandte Sprache (partielle Lokalisierung). Beispielsweise können Sie nicht alle Benutzeroberflächen Ihrer APP in Katalanisch lokalisieren, wenn Ihre APP über einen vollständigen Satz von Ressourcen auf Spanisch verfügt. Für Benutzer, die Katalanisch und dann Spanisch sprechen, werden die Ressourcen, die in Katalanisch nicht verfügbar sind, in Spanisch angezeigt.
- Einige Ressourcen erfordern möglicherweise Ausnahmen für bestimmte Sprachen, während die Mehrzahl anderer Ressourcen einer gemeinsamen Ressource zugeordnet werden. Markieren Sie in diesem Fall die Ressource, die für alle Sprachen mit dem unbestimmten Sprachtag "und" verwendet werden soll. Windows interpretiert das sprach Kennzeichen "und" als Platzhalter Zeichen (ähnlich wie " \* "), da es nach jeder anderen spezifischen Übereinstimmung mit der Sprache der obersten App übereinstimmt. Wenn z. b. einige Ressourcen für Finnisch anders sind, aber die restlichen Ressourcen für alle Sprachen identisch sind, sollte die finnische Ressource mit dem finnischen Sprachtag gekennzeichnet werden, und der Rest sollte mit "und" gekennzeichnet werden.
- Verwenden Sie für Ressourcen, die auf einem sprach Skript basieren, z. b. eine Schriftart oder eine Texthöhe, das undefinierte Sprachtag mit einem angegebenen Skript: "und- &lt; Script &gt; ". Verwenden Sie z. b. für lateinische Schriftarten `und-Latn\\fonts.css` und für die Verwendung von kyrillischen Schriftarten `und-Cryl\\fonts.css` .

## <a name="set-the-http-accept-language-request-header"></a>Festlegen des http-Accept-Language-Anforderungs Headers
Stellen Sie sich vor, ob die von Ihnen aufzurufenden Webdienste denselben Umfang an Lokalisierung aufweisen wie Ihre APP. HTTP-Anforderungen, die von Windows-apps in typischen Webanforderungen und XMLHttpRequest (XHR) vorgenommen werden, verwenden den Standard-http-Accept-Language-Anforderungs Header. Standardmäßig wird der-HTTP-Header auf die Benutzerprofil-Sprachliste festgelegt. Jede Sprache in der Liste wird zudem um die regionsneutralen Varianten der Sprache sowie um eine Gewichtung (q) erweitert. Beispielsweise führt die Sprachliste eines Benutzers von "fr-FR" und "en-US" zu einem http-Accept-Language-Anforderungs Header "fr-FR", "fr", "en-US", "en" ("fr-FR, fr; q = 0.8, en-US; q = 0,5, en; q = 0.3"). Wenn Ihre Wetter-app (z. b.) eine Benutzeroberfläche in Französisch (Frankreich) anzeigt, die oberste Sprache des Benutzers in der Einstellungs Liste jedoch Deutsch ist, müssen Sie für den Dienst explizit Französisch (Frankreich) anfordern, damit Sie in Ihrer APP konsistent bleiben.

## <a name="apis-in-the-windowsglobalization-namespace"></a>APIs im Windows. Globalization-Namespace
In der Regel verwenden die APIs im [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) -Namespace die Sprachliste der APP-Runtime, um die Sprache zu bestimmen. Wenn keine der Sprachen ein übereinstimmendes Format aufweist, wird das Gebiets Schema des Benutzers verwendet. Hierbei handelt es sich um das von der Systemuhr verwendete Gebietsschema. Das Gebiets Schema des Benutzers ist über **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprache**  >  **Zusatz Datum, Uhrzeit, &**  >  **Regions Einstellungen Bereich: Ändern von Datums-, Uhrzeit-oder Zahlenformaten** verfügbar. Die **Windows. Globalization** -APIs verfügen auch über außer Kraft setzungen, um eine Liste der zu verwendenden Sprachen anstelle der Sprache der APP-Lauf Zeit Sprache anzugeben.

Mithilfe der [**Language**](/uwp/api/windows.globalization.language?branch=live) -Klasse können Sie Details zu einer bestimmten Sprache (z. b. das Skript der Sprache, den anzeigen Amen und den systemeigenen Namen) überprüfen.

## <a name="use-geographic-region-when-appropriate"></a>Geografische Region bei Bedarf verwenden
In den **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprache**  >  **Land oder Region** kann der Benutzer seinen Standort in der Welt angeben. Sie können diese Einstellungen anstelle von Sprache verwenden, um den Inhalt auszuwählen, der für den Benutzer angezeigt werden soll. Beispielsweise kann eine News-App standardmäßig den Inhalt aus dieser Region anzeigen.

Im Code können Sie auf diese Einstellung zugreifen, indem Sie die [**globalizationpreferences. homegeocregion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) -Eigenschaft verwenden.

Mithilfe der [**geocregion**](/uwp/api/windows.globalization.geographicregion?branch=live) -Klasse können Sie Details zu einer bestimmten Region überprüfen, z. b. den anzeigen Amen, den nativen Namen und die verwendeten Währungen.

## <a name="examples"></a>Beispiele
Die folgende Tabelle enthält Beispiele dafür, was der Benutzer in der Benutzeroberfläche Ihrer APP unter verschiedenen Sprach-und Regions Einstellungen sehen würde.

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
<th align="left">Liste der APP-Manifest-Sprachen</th>
<th align="left">Benutzerprofil-Sprachliste</th>
<th align="left">Überschreibung der primären Sprache (optional)</th>
<th align="left">Sprachliste der APP-Runtime</th>
<th align="left">Anzeige in der App</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Englisch (GB) (Standard); Deutsch (Deutschland)</td>
<td align="left">Englisch (Vereinigtes Königreich)</td>
<td align="left">Keine</td>
<td align="left">Englisch (Vereinigtes Königreich)</td>
<td align="left">UI: Englisch (GB)<br>Datum/Uhrzeit/Zahlen: Englisch (GB)</td>
</tr>
<tr>
<td align="left">Deutsch (Deutschland) (Standard); Französisch (Frankreich); Italienisch (Italien)</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">Keine</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">UI: Französisch (Frankreich) (Fallback aus Französisch (Österreich))<br>Datum/Uhrzeit/Zahlen: Französisch (Österreich)</td>
</tr>
<tr>
<td align="left">Englisch (US) (Standard); Französisch (Frankreich); Englisch (GB)</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">Keine</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">UI: Englisch (USA) (Fallback aus Englisch (Kanada))<br>Datum/Uhrzeit/Zahlen: Englisch (Kanada)</td>
</tr>
<tr>
<td align="left">Spanisch (Spanien) (Standard); Spanisch (Mexiko); Spanisch (Lateinamerika); Portugiesisch (Brasilien)</td>
<td align="left">Englisch (USA)</td>
<td align="left">Keine</td>
<td align="left">Spanisch (Spanien)</td>
<td align="left">UI: Spanisch (Spanien) (verwendet „Standard“ da kein Fallback für Englisch verfügbar ist)<br>Datum/Uhrzeit/Zahlen Spanisch (Spanien)</td>
</tr>
<tr>
<td align="left">Katalanisch (Standard); Spanisch (Spanien); Französisch (Frankreich)</td>
<td align="left">Katalanisch; Französisch (Frankreich)</td>
<td align="left">Keine</td>
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
> Eine Liste der von Microsoft verwendeten Standard Codes für Länder und Regionen finden Sie in der [Liste offizieller Länder/](../../publish/supported-languages.md)Regionen.

## <a name="important-apis"></a>Wichtige APIs
* [Globalizationpreferences. Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [Applicationlanguages. ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [Primarylanguageoverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [Resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [Resourcecontext. Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [Applicationlanguages. Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Sprache](/uwp/api/windows.globalization.language?branch=live)
* [Globalizationpreferences. homegeocregion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [Geografische Region](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [BCP-47-Sprachtag](https://tools.ietf.org/html/bcp47)
* [IANA-sprachsubtag-Registrierung](https://www.iana.org/assignments/language-subtag-registry)
* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Unterstützte Sprachen](../../publish/supported-languages.md)
* [Globalisieren von Datums-, Uhrzeit- und Zahlenformaten](use-global-ready-formats.md)
* [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Beispiele
* [Anwendungsressourcen und Lokalisierung – Beispiel](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
