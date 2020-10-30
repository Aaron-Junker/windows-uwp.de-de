---
description: Im vorherigen Thema (Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt) wird die Zuordnung von Qualifizierern im Allgemeinen behandelt. Dieses Thema konzentriert sich ausführlicher auf den Vergleich von Sprachtags.
title: Wie das Ressourcenverwaltungssystem Sprachtags zuordnet
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 3c5b2d595585cabdfb9f2983f2ad87f05b7fa5a4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031883"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Wie das Ressourcenverwaltungssystem Sprachtags zuordnet

Im vorherigen Thema ([Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md)) wird die Zuordnung von Qualifizierern im Allgemeinen behandelt. Dieses Thema konzentriert sich ausführlicher auf den Vergleich von Sprachtags.

## <a name="introduction"></a>Einführung

Ressourcen mit Sprachtag-Qualifizierern werden basierend auf der APP-Lauf Zeit Sprachliste verglichen und bewertet. Definitionen der verschiedenen sprach Listen finden Sie Untergrund Legendes zu [Benutzerprofil Sprachen und App-Manifest-Sprachen](../design/globalizing/manage-language-and-region.md). Zuerst wird die erste Sprache in einer Liste abgeglichen und dann die zweite Sprache in der Liste (auch bei anderen regionalen Varianten). Beispielsweise wird eine Ressource für "en-GB" für eine "fr-ca"-Ressource ausgewählt, wenn die APP-Lauf Zeit Sprache "en-US" ist. Nur wenn keine Ressourcen für eine Art von en vorhanden sind, wird eine Ressource für "fr-ca" ausgewählt (Beachten Sie, dass die Standardsprache der app in diesem Fall nicht auf eine Form von "en" festgelegt werden kann).

Der Bewertungsmechanismus verwendet Daten, die in der [bcp-47-](https://tools.ietf.org/html/bcp47) unter tagregistrierung und anderen Datenquellen enthalten sind. Er ermöglicht einen Bewertungs Verlauf mit unterschiedlichen Qualitäten der Übereinstimmung, und wenn mehrere Kandidaten verfügbar sind, wählt er den Kandidaten mit der besten Treffergenauigkeit aus.

Daher können Sie Sprachinhalte in generischen Begriffen markieren, aber Sie können bei Bedarf trotzdem auch bestimmte Inhalte angeben. Ihre APP kann z. b. viele englische Zeichen folgen haben, die sowohl für die USA, für Großbritannien als auch für andere Regionen üblich sind. Das Markieren dieser Zeichen folgen als "en" (Englisch) spart Speicherplatz und Lokalisierungs Aufwand. Wenn Unterschiede vorgenommen werden müssen, z. b. in einer Zeichenfolge, die das Wort "Color/Color" enthält, können die USA-und die britische Version separat mithilfe von Sprach-und Regions unter Tags gekennzeichnet werden, wie z. b. "en-US" bzw. "en-GB".

## <a name="language-tags"></a>Sprachkennzeichen

Sprachen werden mithilfe normalisierter, wohlgeformter bcp-47-sprach Tags identifiziert. Tags-Komponenten werden in der bcp-47-Tags-Registrierung definiert. Die normale Struktur eines bcp-47-sprach Tags besteht aus einem oder mehreren der folgenden Tags-Elemente.

- Sprach Untertag (erforderlich).
- Script Tags (kann mithilfe der in der unter tagregistrierung angegebenen Standardeinstellung abgeleitet werden).
- Regions Untertag (optional).
- Variant-Untertag (optional).

Möglicherweise sind zusätzliche Tags-Elemente vorhanden, Sie haben jedoch eine vernachlässigende Auswirkung auf den sprach Abgleich. Es sind keine Sprachbereiche mit der Platzhalter (" *") definiert, z. b. "en-* ".

## <a name="matching-two-languages"></a>Übereinstimmung mit zwei Sprachen

Jedes Mal, wenn Windows zwei Sprachen vergleicht, erfolgt dies in der Regel im Kontext eines größeren Prozesses. Es kann sich im Zusammenhang mit der Bewertung mehrerer Sprachen befinden, z. b. wenn Windows die Anwendungs Sprachliste generiert (Siehe Grundlegendes zu [Benutzerprofil Sprachen und App-Manifest-Sprachen](../design/globalizing/manage-language-and-region.md)). Dies geschieht, indem mehrere Sprachen von den Benutzereinstellungen mit den im Manifest der APP angegebenen Sprachen abgeglichen werden. Der Vergleich kann auch im Kontext der Bewertung von Sprache zusammen mit anderen Qualifizierern für eine bestimmte Ressource erfolgen. Ein Beispiel hierfür ist, wenn Windows eine bestimmte Datei Ressource in einen bestimmten Ressourcen Kontext auflöst. mit dem Start Standort des Benutzers oder der aktuellen Skala oder dem Gerät des Geräts als andere Faktoren (außer der Sprache), die in die Ressourcen Auswahl einbezogen werden.

Wenn zwei sprach Tags verglichen werden, wird dem Vergleich basierend auf der Nähe der Entsprechung eine Bewertung zugewiesen.

| Match | Ergebnis | Beispiel |
| ----- | ----- | ------- |
| Genaue Übereinstimmung | Maximal | de-au: en-au |
| Variant-Übereinstimmung (Sprache, Skript, Region, Variante) |  | de-au-variant1: en-au-variant1-t-ja |
| Regions Übereinstimmung (Sprache, Skript, Region) |  | de-au: en-au-variant1 |
| Teil Übereinstimmung (Sprache, Skript) |  |  |
| -Makro Regions Übereinstimmung |  | de-au: en-053 |
| -Regions neutrale Übereinstimmung |  | de-au: en |
| -Orthografische Affinitäts Übereinstimmung (eingeschränkte Unterstützung) |  | de-au: en-GB |
| -Bevorzugte Regions Übereinstimmung |  | de-au: en-US |
| -Beliebige Regions Übereinstimmung |  | de-au: en-ca |
| Nicht festgelegte Sprache (beliebige sprach Übereinstimmung) |  | de-au: und |
| Keine Übereinstimmung (Skript Konflikt oder nicht übereinstimmende primäre Sprach Tags) | Niedrigste | de-au: fr-FR |

### <a name="exact-match"></a>Genaue Übereinstimmung

Die Tags sind genau gleich (alle Tags-Elemente stimmen überein). Ein Vergleich kann aus einer Variant-oder einer Regions Übereinstimmung zu diesem Übereinstimmungs Typ herauf gestuft werden. Beispielsweise entspricht "en-US" en-US.

### <a name="variant-match"></a>Variant-Entsprechung

Die Tags entsprechen den unter Tags Sprache, Skript, Region und Variant, Sie unterscheiden sich jedoch in anderer Hinsicht.

### <a name="region-match"></a>Regions Übereinstimmung

Die Tags entsprechen den unter Tags Sprache, Skript und Region, Sie unterscheiden sich jedoch in anderer Hinsicht. Beispielsweise entspricht "de-de-1996" de-de, und "en-US-x-Pirate" entspricht "en-US".

### <a name="partial-matches"></a>Teilweise Übereinstimmungen

Die Tags entsprechen den unter Tags der Sprache und des Skripts, aber Sie unterscheiden sich in der Region oder einem anderen Untertag. Beispielsweise entspricht "en-US" en, oder "en-US" entspricht "en-" \* .

#### <a name="macro-region-match"></a>Makro Bereichs Übereinstimmung

Die Tags entsprechen den unter Tags der Sprache und des Skripts. Beide Tags verfügen über Regions unter Tags, von denen eine einen Makrobereich bezeichnet, der die andere Region umfasst. Die Subtags der Makroregion sind immer numerisch und werden aus den Bundesstaaten-und Bundes flächencodes der US-Statistik Division M. 49 abgeleitet. Ausführliche Informationen zum Einbeziehen von Beziehungen finden Sie unter [Komposition von Makro geografischen Regionen (kontinental), geografischen Teilregionen und ausgewählten wirtschaftlichen und anderen Gruppierungen](https://unstats.un.org/unsd/methods/m49/m49regin.htm).

**Hinweis** Nicht-Codes für "wirtschaftliche Gruppierungen" oder "andere Gruppierungen" werden in BCP-47 nicht unterstützt.
 
**Hinweis** Ein Tag mit dem unter Tag "001" der Makroregion wird als äquivalent zu einem Regions neutralen Tag betrachtet. Beispielsweise werden "es-001" und "es" als Synonym behandelt.

#### <a name="region-neutral-match"></a>Regions neutrale Übereinstimmung

Die Tags entsprechen den untergeordneten Tags für Sprache und Skript, und nur ein Tag weist ein Regions-Tag auf. Eine übergeordnete Übereinstimmung wird gegenüber anderen partiellen Übereinstimmungen bevorzugt.

#### <a name="orthographic-affinity-match"></a>Orthografische Affinitäts Übereinstimmung

Die Tags entsprechen den unter Tags für Sprache und Skript, und die Subtags der Region haben eine orthografische Affinität. Die Affinität basiert auf Daten, die in Windows verwaltet werden, das sprachspezifische Bereiche definiert, z. b. "en-IE" und "en-GB".

#### <a name="preferred-region-match"></a>Abgleich der bevorzugten Region

Die Tags entsprechen den unter Tags "Sprache" und "Skript", und eines der Regions Subtags ist das standardmäßige Regions Untertag für die Sprache. "Fr-FR" ist z. b. die Standard Region für das untergeordnete Tag "fr". Daher ist "fr-FR" für "fr-be" besser geeignet als "fr-ca". Dies basiert auf Daten, die in Windows verwaltet werden und eine Standard Region für jede Sprache definieren, in der Windows lokalisiert wird.

#### <a name="sibling-match"></a>Gleich geordnete Übereinstimmung

Die Tags entsprechen den unter Tags der Sprache und des Skripts, und beide verfügen über Regions unter Tags, aber es ist keine andere Beziehung zwischen Ihnen definiert. Im Fall von mehreren gleich geordneten Übereinstimmungen ist das letzte aufgelistete gleich geordnete Element der Gewinner, wenn eine höhere Übereinstimmung fehlt.

### <a name="undetermined-language"></a>Nicht festgelegte Sprache

Eine Ressource kann als "und" gekennzeichnet werden, um anzugeben, dass Sie mit einer beliebigen Sprache übereinstimmt. Dieses Tag kann auch mit einem Skripttag verwendet werden, um Übereinstimmungen auf der Grundlage des Skripts zu filtern. Beispielsweise entspricht "und-Latn" jedem sprach Kennzeichen, das das lateinische Skript verwendet. Weitere Informationen finden Sie unten.

### <a name="script-mismatch"></a>Skript Konflikt

Wenn die Tags nur für das primäre Sprachtag, jedoch nicht für das Skript gefunden werden, wird das Paar als nicht zu vergleichen angesehen und wird unterhalb der Ebene einer gültigen Entsprechung bewertet.

### <a name="no-match"></a>Keine Übereinstimmung

Nicht übereinstimmende primäre Sprach untertags werden unterhalb der Ebene einer gültigen Übereinstimmung bewertet. Beispielsweise entspricht zh-Hant nicht zh-Hans.

## <a name="examples"></a>Beispiele

Die Benutzersprache "zh-Hans-CN" (Chinesisch (vereinfacht) (China)) entspricht den folgenden Ressourcen in der angezeigten Prioritäts Reihenfolge. Ein X weist auf keine Entsprechung hin.

![Vergleich für Chinesisch (vereinfacht) (China)](images/language_matching_1.png)

1. Exakte Entsprechung; 2. & 3. Regions Übereinstimmung 4. Übergeordnete Entsprechung; 5. Gleich geordnete Übereinstimmung.

Wenn ein sprach teiltag einen Suppress-Script Wert enthält, der in der bcp-47-Tags-Registrierung definiert ist, erfolgt die entsprechende Übereinstimmung, wobei der Wert des unterdrückten Skriptcodes übernommen wird. "en-Latn-US" passt z. B. zu "en-US". Im nächsten Beispiel ist die Benutzersprache "en-au" (Englisch (Australien)).

![Abgleich für Englisch (Australien)](images/language_matching_2.png)

1. Exakte Entsprechung; 2. Makrobereich entspricht. 3. Regions neutrale Entsprechung; 4. Orthografische Affinitäts Übereinstimmung; 5. Die bevorzugte Region entspricht. 6. Gleich geordnete Übereinstimmung.

## <a name="matching-a-language-to-a-language-list"></a>Abgleichen einer Sprache mit einer Sprachliste

Manchmal erfolgt der Abgleich im Rahmen eines größeren Prozesses, bei dem eine einzelne Sprache einer Liste von Sprachen entspricht. Beispielsweise kann es sein, dass eine einzelne sprachbasierte Ressource einer Anwendungsliste der APP entspricht. Das Ergebnis der Übereinstimmung wird durch die Position der ersten übereinstimmenden Sprache in der Liste gewichtet. Je niedriger die Sprache in der Liste ist, desto niedriger ist das Ergebnis.

Wenn die Sprachliste zwei oder mehr Regions Varianten enthält, die die gleichen sprach-und Skript untertags aufweisen, werden Vergleiche für das erste sprach Kennzeichen nur für exakte, Variante und Regions Übereinstimmungen bewertet. Die Bewertung von Teil Übereinstimmungen wird in die letzte Regions Variante verschoben. Dies ermöglicht es Benutzern, das abgleichsverhalten für Ihre Sprachliste zu steuern. Das abgleichsverhalten kann einschließen, dass eine exakte Übereinstimmung für ein sekundäres Element in der Liste für eine partielle Übereinstimmung für das erste Element in der Liste bevorzugt wird, wenn ein drittes Element vorhanden ist, das mit der Sprache und dem Skript der ersten übereinstimmt. Hier sehen Sie ein Beispiel.

- Sprachliste (in der richtigen Reihenfolge): "pt-pt" (Portugiesisch (Portugal)), "en-US" (Englisch (USA)), "pt-br" (Portugiesisch (Brasilien)).
- Ressourcen: "en-US", "pt-br".
- Ressource mit der höheren Bewertung: "en-US".
- Beschreibung: der Vergleich beginnt mit "pt-pt", findet jedoch keine genaue Entsprechung. Da "pt-br" in der Sprachliste des Benutzers vorhanden ist, wird die Teil Übereinstimmung in den Vergleich mit "pt-br" verschoben. Der nächste sprach Vergleich ist "en-US", der über eine genaue Entsprechung verfügt. Die gewinnende Ressource ist also "en-US".

oder

- Sprachliste (in der richtigen Reihenfolge): "es-MX" (Spanisch (Mexiko)), "es-Ho" (Spanisch (Honduras)).
- Ressourcen: "en-es", "es-Ho".
- Ressource mit der höheren Bewertung: "es-Ho".

## <a name="undetermined-language-und"></a>Nicht festgelegte Sprache ("und")

Das Sprachtag "und" kann verwendet werden, um eine Ressource anzugeben, die mit einer beliebigen Sprache identisch ist, wenn eine bessere Entsprechung fehlt. Sie kann als vergleichbar mit dem bcp-47-Sprachbereich " *" oder "* - &lt; Skript &gt; " betrachtet werden. Hier sehen Sie ein Beispiel.

- Sprachliste: "en-US", "zh-Hans-CN".
- Ressourcen: "zh-Hans-CN", "und".
- Ressource mit dem höheren Ergebnis: "und".
- Beschreibung: der Vergleich beginnt mit "en-US", findet jedoch keine Treffer auf der Grundlage von "en" (teilweise oder besser). Da eine Ressource mit "und" gekennzeichnet ist, verwendet der übereinstimmende Algorithmus diese.

Das Tag "und" ermöglicht es mehreren Sprachen, eine einzelne Ressource freizugeben, und ermöglicht es, dass einzelne Sprachen als Ausnahmen behandelt werden. Beispiel:

- Sprachliste: "zh-Hans-CN", "en-US".
- Ressourcen: "zh-Hans-CN", "und".
- Ressource mit der höheren Bewertung: "zh-Hans-CN".
- Beschreibung: beim Vergleich wird eine genaue Entsprechung für das erste Element gefunden, und es wird keine Überprüfung der Ressource mit der Bezeichnung "und" durchführt.

Sie können "und" mit einem Skripttag verwenden, um Ressourcen nach Skript zu filtern. Beispiel:

- Sprachliste: "ru".
- Ressourcen: "und-Latn", "und-Cyrl", "und-Arabisch".
- Ressource mit dem höheren Ergebnis: "und-Cyrl".
- Beschreibung: der Vergleich findet keine Übereinstimmung mit "ru" (partiell oder besser) und entspricht somit dem sprach Kennzeichen "und". Der mit dem Sprachtag "ru" verknüpfte "SUP-Script"-Wert "Cyrl" stimmt mit der Ressource "und-Cyrl" überein.

## <a name="orthographic-regional-affinity"></a>Orthografische regionale Affinität

Wenn zwei Sprachen Tags mit unterschiedlichen Regions unter Tags übereinstimmen, haben bestimmte Regions Paare möglicherweise eine höhere Affinität als andere. Die einzigen unterstützten affiniegruppen sind für Englisch ("en"). Die Regions unter Tags "pH" (Philippinen) und "LR" (Liberia) haben eine orthografische Affinität mit dem Subtag "US" Region. Alle anderen Subtags der Region werden dem Subtag "GB" (Vereinigtes Königreich) zugeordnet. Wenn also die Ressourcen "en-US" und "en-GB" verfügbar sind, erhält die Sprachliste "en-HK" (Englisch (Hongkong SAR)) eine höhere Bewertung mit "en-GB"-Ressourcen als mit den Ressourcen "en-US".

## <a name="handling-languages-with-many-regional-variants"></a>Behandeln von Sprachen mit vielen regionalen Varianten

Bestimmte Sprachen haben große Sprecher in unterschiedlichen Regionen, die unterschiedliche Arten von Sprachen verwenden, &mdash; wie z. b. Englisch, Französisch und Spanisch, die häufig in mehrsprachigen Apps unterstützt werden. Regionale Unterschiede können Unterschiede in der Orthografie (z. b. "Color" im Vergleich zu "Color") oder Unterschiede im Dialekt (z. b. "LKW" und "LKW") enthalten.

Diese Sprachen mit erheblichen regionalen Varianten stellen bestimmte Herausforderungen dar, wenn Sie eine weltweit einsetzbare app erstellen: "wie viele verschiedene regionale Varianten sollten unterstützt werden?" "Welche?" "Was ist die kostengünstigste Methode, diese regionalen Varianten Ressourcen für meine APP zu verwalten?" Es geht über den Rahmen dieses Themas hinaus, alle Fragen zu beantworten. Die Übereinstimmungs Mechanismen der Sprache in Windows bieten jedoch Funktionen, die Ihnen bei der Handhabung regionaler Varianten helfen können.

-Apps unterstützen häufig nur eine einzige Vielzahl von Sprachen. Angenommen, eine APP verfügt über Ressourcen für nur eine Vielzahl von Englisch, die von englischen Referenten unabhängig von der Region verwendet werden sollen, von der Sie stammen. In diesem Fall würde das Tag "en" ohne ein Regions Untertag diese Erwartung widerspiegeln. Doch apps haben möglicherweise in der Vergangenheit ein Tag wie "en-US" verwendet, das ein Regions Untertag enthält. In diesem Fall funktioniert dies auch: in der APP wird nur eine Vielzahl von Englisch verwendet, und Windows verarbeitet eine Ressource, die für eine regionale Variante mit einer Benutzer Spracheinstellung für eine andere Regions Variante gekennzeichnet ist.

Wenn zwei oder mehr regionale Sorten unterstützt werden, kann ein Unterschied wie z. b. "en" im Vergleich zu "en-US" eine erhebliche Auswirkung auf die Benutzerumgebung haben. Außerdem ist es wichtig zu verstehen, welche Regions teiltags verwendet werden sollen.

Angenommen, Sie möchten separate französische Lokalisierungs Informationen für Französisch bereitstellen, die in Kanada im Vergleich zum Europäischen Französisch verwendet werden. Für Französisch (Kanada) kann "fr-ca" verwendet werden. Für Sprecher aus Europa verwendet die Lokalisierung Französisch (Frankreich), sodass "fr-FR" dafür verwendet werden kann. Was aber, wenn ein bestimmter Benutzer aus Belgien besteht und die Spracheinstellung "fr-be" lautet; welche werden Sie erhalten? Die Region "be" unterscheidet sich von "fr" und "ca" und deutet auf eine beliebige Region hin. Frankreich ist jedoch die bevorzugte Region für Französisch, sodass "fr-FR" in diesem Fall die beste Entsprechung ist.

Angenommen, Sie haben Ihre APP zunächst nur für eine Vielzahl von Französisch lokalisiert, indem Sie französische (Frankreich) Zeichen folgen verwenden, Sie aber generisch als "fr" qualifizieren und dann die Unterstützung für Französisch (Kanada) hinzufügen möchten. Wahrscheinlich müssen nur bestimmte Ressourcen für Französisch (Kanada) neu übersetzt werden. Sie können weiterhin alle ursprünglichen Assets verwenden, die Sie als "fr" qualifizieren, und einfach den kleinen Satz neuer Assets mithilfe von "fr-ca" hinzufügen. Wenn die Benutzer Spracheinstellung "fr-ca" lautet, hat das Objekt "fr-ca" eine höhere Treffergenauigkeit als das "fr"-Asset. Wenn die Benutzersprache jedoch für eine andere Vielfalt von Französisch gilt, ist das Regions neutrale Asset "fr" besser geeignet als das Asset "fr-ca".

Ein weiteres Beispiel: angenommen, Sie möchten separate spanische Lokalisierungs Informationen für Referenten aus Spanien und Sprecher aus Lateinamerika bereitstellen. Nehmen Sie weiter an, dass die Übersetzungen für Lateinamerika von einem Anbieter in Mexiko bereitgestellt wurden. Sollten Sie "es-es" (Spanien) und "es-MX" (Mexiko) für zwei Sätze von Ressourcen verwenden? Wenn Sie dies getan haben, könnte dies für Redner aus anderen lateinischen Regionen wie Argentinien oder Kolumbien Probleme verursachen, da Sie die Ressourcen "es-es" erhalten. In diesem Fall gibt es eine bessere Alternative: Sie können ein Makro Regions Untertag "es-419" verwenden, um widerzuspiegeln, dass Sie beabsichtigen, die Assets für Referenten aus beliebigen Teilen Lateinamerika oder der Karibik zu verwenden.

Regions neutrale Sprachen Tags und Makro Regions-unter Tags können sehr effektiv sein, wenn Sie verschiedene regionale Variationen unterstützen möchten. Um die Anzahl der separaten Ressourcen zu minimieren, die Sie benötigen, können Sie ein bestimmtes Medienobjekt so qualifizieren, dass es die breiteste Abdeckung angibt, auf die es anwendbar ist. Ergänzen Sie anschließend ein allgemein zutreffendes Asset mit einer spezifischeren Variante nach Bedarf. Ein Medienobjekt mit einem Regions neutralen sprach Qualifizierer wird für Benutzer jeder regionalen Vielfalt verwendet, es sei denn, es gibt ein anderes Asset mit einem stärker regionalspezifischen Qualifizierer, der für diesen Benutzer gilt. Beispielsweise entspricht ein "en"-Asset einem australischen englischen Benutzer, aber ein Asset mit "en-053" (Englisch, das in Australien oder Neuseeland verwendet wird) ist für diesen Benutzer besser geeignet, während ein Asset mit "en-au" die bestmögliche Entsprechung ist.

Englisch erfordert besondere Überlegungen. Wenn eine APP eine Lokalisierung für zwei englische Sorten hinzufügt, sind diese wahrscheinlich für US-Englisch und für Großbritannien oder "International", Englisch. Wie bereits erwähnt, folgen bestimmte Regionen außerhalb der USA USA Rechtschreibkonventionen, und die Windows-sprach Übereinstimmung berücksichtigt dies. In diesem Szenario wird die Verwendung des Regions neutralen Tags "en" für eine der Varianten nicht empfohlen. Verwenden Sie stattdessen "en-GB" und "en-US". (Wenn für eine bestimmte Ressource keine separaten Varianten erforderlich sind, kann jedoch "en" verwendet werden.) Wenn "en-GB" oder "en-US" durch "en" ersetzt wird, beeinträchtigt dies die von Windows bereitgestellte orthografische regionale Affinität. Wenn eine dritte englische Lokalisierung hinzugefügt wird, verwenden Sie bei Bedarf ein bestimmtes Tags oder ein Makro Regions-Tags für die zusätzlichen Varianten (z. b. "en-ca", "en-au" oder "en-053"), verwenden Sie jedoch weiterhin "en-GB" und "en-US".

## <a name="related-topics"></a>Verwandte Themen

* [Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](../design/globalizing/manage-language-and-region.md)
* [Komposition von Makro geografischen (kontinentalen) Regionen, geografischen Teilregionen und ausgewählten wirtschaftlichen und anderen Gruppierungen](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
