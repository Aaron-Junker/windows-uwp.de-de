---
Description: Im vorherigen Thema (Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt) wird die Zuordnung von Qualifizierern im Allgemeinen behandelt. Dieses Thema konzentriert sich ausführlicher auf den Vergleich von Sprachtags.
title: Wie das Ressourcenverwaltungssystem Sprachtags zuordnet
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 2db4c7d98924ffb25800b1c615e929495b279fdc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254661"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Wie das Ressourcenverwaltungssystem Sprachtags zuordnet

Im vorherigen Thema ([Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md)) wird die Zuordnung von Qualifizierern im Allgemeinen behandelt. Dieses Thema konzentriert sich ausführlicher auf den Vergleich von Sprachtags.

## <a name="introduction"></a>Einführung

Ressourcen mit Sprachtagqualifizierern werden basierend auf der Sprachenliste für die App-Laufzeit verglichen und bewertet. Definitionen der verschiedenen Sprachlisten finden Sie unter [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](../design/globalizing/manage-language-and-region.md). Zuerst wird die erste Sprache in einer Liste abgeglichen und dann die zweite Sprache in der Liste (auch bei anderen regionalen Varianten). Eine Ressource für „en-GB“ wird z. B. vor einer „fr-CA“-Ressource ausgewählt, wenn „en-US“ die Sprache der App-Laufzeit ist. Nur dann, wenn keine Ressourcen für eine Form von „en“ vorhanden sind, wird eine Ressource für „fr-CA“ gewählt. (Beachten Sie, dass in diesem Fall die Standardsprache der App nicht auf eine beliebige Form von „en“ festgelegt werden kann).

Der Bewertungsmechanismus verwendet Daten aus dem [BCP-47](https://tools.ietf.org/html/bcp47)-Register und aus andere Datenquellen. Dies ermöglicht einen Bewertungsgradienten mit unterschiedlichen Übereinstimmungsqualitäten. Sind mehrere Kandidaten verfügbar, wird der Kandidat mit der besten Übereinstimmungsbewertung ausgewählt.

Dadurch können Sie Sprachinhalten allgemeine Tags hinzufügen, bei Bedarf aber dennoch bestimmte Inhalte angeben. Beispielsweise könnte Ihre App über viele englische Zeichenfolgen verfügen, die sowohl in den USA als auch in Großbritannien und anderen Regionen üblich sind. Werden diese Zeichenfolgen mit dem Tag „en” (Englisch) versehen, kann dadurch Platz gespart und der Lokalisierungsaufwand reduziert werden. Wenn eine Unterscheidung erforderlich ist, z. B. in einer Zeichenfolge mit dem Wort „color” oder „colour”, können für die US-amerikanische und die englische Version jeweils die Tags „en-US” und „en-GB” verwendet werden.

## <a name="language-tags"></a>Sprachtags

Sprachen werden über normalisierte, wohlgeformte BCP-47-Sprachtags identifiziert. Komponenten für untergeordnete Tags werden in der BCP-47-Registrierung für untergeordnete Tags definiert. Eine normale Struktur für ein BCP-47-Sprachtag besteht aus mindestens einem dieser Subtag-Elemente.

- Sprachen-Subtag (erforderlich)
- Untergeordnetes Skripttag (kann mithilfe des Standardelements in der Registrierung für untergeordnete Tags abgeleitet werden)
- Untergeordnetes Regionstag (optional)
- Varianten-Subtag (optional)

Zusätzliche Subtag-Elemente können vorhanden sein, wirken sich jedoch nur unerheblich auf den Sprachvergleich aus. Mit dem Platzhalter „ *” werden keine Sprachbereiche (z. B. „en-* ”) definiert.

## <a name="matching-two-languages"></a>Vergleichen zweier Sprachen

Wenn Windows zwei Sprachen vergleicht, geschieht dies in der Regel im Kontext eines umfangreicheren Prozesses. Möglicherweise wird der Vorgang im Kontext der Bewertung mehrerer Sprachen durchgeführt, z. B. wenn Windows die Anwendungssprachenliste generiert (siehe [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](../design/globalizing/manage-language-and-region.md)). Windows gleicht dazu mehrere Sprachen aus den Benutzereinstellungen mit den im App-Manifest angegebenen Sprachen ab. Der Vergleich kann auch im Rahmen der Bewertung von Sprachen und anderen Qualifizierern für eine bestimmte Ressource erfolgen. Ein Beispiel hierfür ist, wenn unter Windows eine bestimmte Dateiressource in einen bestimmten Ressourcenkontext aufgelöst wird und der Wohnort des Benutzers oder die aktuelle Skalierung des Geräts oder der DPI-Wert neben der Sprache als weitere Faktoren (neben der Sprache) verwendet werden, die in die Ressourcenauswahl eingehen.

Beim Vergleich von zwei Sprachtags wird dem Vergleich basierend auf der Höhe der Übereinstimmung eine Bewertung zugewiesen.

| Übereinstimmung | Wert | Beispiel |
| ----- | ----- | ------- |
| Genaue Übereinstimmung | Höchster | en-AU : en-AU |
| Übereinstimmung der Variante (Sprache, Skript, Region, Variante) |  | en-AU-variant1 : en-AU-variant1-t-ja |
| Übereinstimmung der Region (Sprache, Skript, Region) |  | en-AU : en-AU-variant1 |
| Teilweise Übereinstimmung (Sprache, Skript) |  |  |
| – Übereinstimmung der Makroregion |  | en-AU : en-053 |
| – Regionsneutrale Übereinstimmung |  | en-AU : en |
| – Übereinstimmung der orthografischen Affinität (eingeschränkte Unterstützung) |  | en-AU : en-GB |
| – Übereinstimmung der bevorzugten Region |  | en-AU : en-US |
| – Übereinstimmung irgendeiner Region |  | en-AU : en-CA |
| Nicht festgelegte Sprache (Übereinstimmung irgendeiner Sprache) |  | en-AU : und |
| Keine Übereinstimmung (Nichtübereinstimmung des Skripts oder des Tags für die primäre Sprache) | Niedrigster | en-AU : fr-FR |

### <a name="exact-match"></a>Genaue Übereinstimmung

Die Tags sind genau gleich (alle Subtag-Elemente stimmen überein). Ein Vergleich wird von einer Übereinstimmung der Variante oder Region möglicherweise auf diesen Übereinstimmungstyp heraufgestuft. Zum Beispiel stimmt „en-US“ mit „en-US“ überein.

### <a name="variant-match"></a>Übereinstimmung der Variante

Die Tags stimmen in Bezug auf die Subtags für Sprachen, Skripte, Regionen und Varianten überein, unterscheiden sich jedoch in anderer Hinsicht.

### <a name="region-match"></a>Übereinstimmung der Region

Die Tags stimmen in Bezug auf die Subtags für Sprachen, Skripte und Regionen überein, unterscheiden sich jedoch in anderer Hinsicht. Zum Beispiel stimmt „de-DE-1996“ mit „de-DE“ und „en-US-x-Pirate“ mit „en-US“ überein.

### <a name="partial-matches"></a>Teilweise Übereinstimmungen

Die Tags stimmen in Bezug auf die Subtags für Sprachen und Skripte überein, unterscheiden sich jedoch in der Region oder einigen anderen Subtags. Beispielsweise entspricht "en-US" en, oder "en-US" entspricht "en-\*".

#### <a name="macro-region-match"></a>Vergleichen von Makroregion

Die Tags stimmen in Bezug auf die Subtags für Sprachen- und Skripte überein. Beide Tags enthalten Regionen-Subtags, von denen eines eine Makroregion angibt, in der die andere Region enthalten ist. Die untergeordneten Tags für die Makroregion sind immer numerischer Art und werden von den Regionscodes der Statistikabteilung der Vereinten Nationen (M49) abgeleitet. Ausführliche Informationen zu umfassenden Beziehungen finden Sie unter [Composition of macro geographic (continental) regions, geographical sub-regions, and selected economic and other groupings](https://unstats.un.org/unsd/methods/m49/m49regin.htm) (Zusammensetzung makrogeografischer (Kontinental-)Regionen, geografischer Unterregionen und ausgewählter ökonomischer und anderer Gruppierungen).

**Hinweis** UN-Codes für „ökonomische Gruppierungen” oder „andere Gruppierungen” werden in BCP-47 nicht unterstützt.
 
**Hinweis** Ein Tag mit dem Makroregionen-Subtag „001” wird als gleichwertig mit einem regionsneutralen Tag angesehen. Beispielsweise werden „es-001” und „es” als Synonyme behandelt.

#### <a name="region-neutral-match"></a>Regionsneutrale Übereinstimmung

Die Tags stimmen in den Sprachen- und Skripte-Subtags überein, und nur ein Tag besitzt ein Regionstag. Eine Übereinstimmung gleichrangiger Elemente wird partiellen Übereinstimmungen vorgezogen.

#### <a name="orthographic-affinity-match"></a>Übereinstimmung der ortografischen Affinität

Die Tags stimmen in den Sprachen- und Skripte-Subtags überein, und die Regionen-Substags weisen ortografische Affinität auf. Die Affinität basiert auf in Windows vorgehaltenen Daten, die sprachspezifisch ähnliche Regionen definieren, z. B. „en-IE” und „en-GB”.

#### <a name="preferred-region-match"></a>Übereinstimmung der bevorzugten Region

Die Tags stimmen in den Sprachen- und Skripte-Subtags überein, und eines der Regionen-Subtags ist das standardmäßige Regionen-Subtag für die Sprache. Beispiel: „fr-FR” ist die Standardregion für das Subtag „fr”. „fr-FR“ passt z. B. besser zu „fr-BE“ als zu „fr-CA“. Dies basiert auf in Windows vorgehaltenen Daten, die eine Standardregion für jede Sprache definieren, in die Windows lokalisiert wurde.

#### <a name="sibling-match"></a>Übereinstimmung gleichgeordneter Elemente

Die Tags stimmen in den Sprachen- und Skripte-Subtags überein und besitzen beide Regionen-Subtags. Es ist jedoch keine andere Beziehung zwischen ihnen definiert. Bei der Übereinstimmung mehrerer gleichgeordneter Elemente erhält das zuletzt aufgeführte gleichgeordnete Element eine höhere Bewertung, wenn keine höhere Übereinstimmung vorliegt.

### <a name="undetermined-language"></a>Undefiniert Sprache

Eine Ressource kann mit dem Tag „und” versehen sein. Damit wird angegeben, dass sie mit jeder Sprache übereinstimmt. Dieses Tag kann auch mit einem Skripttag zum Filtern der Übereinstimmungen auf Basis eines Skripts verwendet werden. Beispiel: „und-Latn” stimmt mit allen Sprachtags überein, die lateinische Schrift verwenden. Weitere Informationen finden Sie weiter unten.

### <a name="script-mismatch"></a>Unterschiedlicher Skripttag

Wenn die Tags nur im Tag für die primäre Sprache, aber nicht im Skripttag übereinstimmen, wird das Paar als nicht übereinstimmend betrachtet. Es erhält eine Bewertung für eine Stufe unterhalb der gültigen Übereinstimmungen.

### <a name="no-match"></a>Keine Übereinstimmung

Nicht übereinstimmende untergeordnete Tags für die primäre Sprache erhalten einen Wert für eine Stufe unterhalb einer gültigen Übereinstimmung. Zum Beispiel stimmt „zh-Hant“ nicht mit „zh-Hans“ überein.

## <a name="examples"></a>Beispiele

Die Benutzersprache „zh-Hans-CN” (vereinfachtes Chinesisch (China)) stimmt mit den folgenden Ressourcen in der angegebenen Prioritätsreihenfolge überein. Ein X steht für eine Nichtübereinstimmung.

![Übereinstimmung für vereinfachtes Chinesisch (China)](images/language_matching_1.png)

1. Genaue Übereinstimmung; 2. und 3: Übereinstimmung der Region; 4. Übereinstimmung des übergeordneten Elements; 5. Übereinstimmung der gleichgeordneten Elemente

Wenn für ein untergeordnetes Sprachtag in der BCP-47-Registrierung für untergeordnete Tags ein Wert zum Unterdrücken von Skripts definiert ist, wird ein entsprechender Abgleich ausgeführt. Dabei wird der Wert des unterdrückten Skriptcodes übernommen. „en-Latn-US“ passt z. B. zu „en-US“. Im folgenden Beispiel ist die Benutzersprache „en-AU” (Englisch (Australien)).

![Übereinstimmung für Englisch (Australien)](images/language_matching_2.png)

1. Genaue Übereinstimmung; 2. Übereinstimmung der Makroregion; 3: Regionsneutrale Übereinstimmung; 4: Übereinstimmung der orthographischen Affinität; 5. Übereinstimmung der bevorzugten Region; 6. Übereinstimmung der gleichgeordneten Elemente

## <a name="matching-a-language-to-a-language-list"></a>Abgleichen einer Sprache mit einer Sprachenliste

Manchmal wird der Abgleich im Rahmen des umfangreicheren Prozesses zum Abgleichen einer einzelnen Sprache mit einer Liste von Sprachen ausgeführt. Ein Beispiel hierfür ist eine Übereinstimmung einer einzelnen sprachbasierten Ressource mit einer Sprachenliste einer App. Der Wert für die Höhe der Übereinstimmung wird nach der Position der ersten übereinstimmenden Sprache in der Liste gewichtet. Je weiter unten in der Liste sich die Sprache befindet, desto niedriger ist der Wert.

Wenn die Sprachenliste zwei oder mehr regionale Varianten mit denselben untergeordneten Sprach- und Skripttags enthält, werden Vergleiche für das erste Sprachtag nur für genaue Übereinstimmungen und Übereinstimmungen der Variante und Region bewertet. Die Vergabe von Werten für partielle Übereinstimmungen wird bis zur letzten Regionsvariante verschoben. Dadurch können Benutzer das Abgleichverhalten für ihre Sprachenliste genau steuern. Es ist z. B. möglich, dass eine genaue Übereinstimmung für ein sekundäres Element in der Liste einer partiellen Übereinstimmung für das erste Element in der Liste vorgezogen wird, wenn ein drittes Element vorliegt, das mit der Sprache und dem Skript des ersten Elements übereinstimmt. Hier sehen Sie ein Beispiel.

- Sprachenliste (in dieser Reihenfolge): „pt-PT” (Portugiesisch (Portugal)), „en-US” (Englisch (USA)), „pt-BR” (Portugiesisch (Brasilien)).
- Ressourcen: „en-US”, „pt-BR”.
- Ressource mit der höheren Bewertung: „en-US”.
- Beschreibung: Der Vergleich beginnt mit „pt-PT”, es wird jedoch keine genaue Übereinstimmung gefunden. Da „pt-BR” in der Sprachenliste des Benutzers vorhanden ist, wird der teilweise Vergleich auf den Vergleich mit „pt-BR” verschoben. Der nächste Sprachvergleich ist „en-US”, für den eine genaue Übereinstimmung vorliegt. Die vorrangige Ressource ist somit „en-US”.

ODER

- Sprachenliste (in dieser Reihenfolge): „es-MX” (Spanisch (Mexiko)), „es-HO ”(Spanisch (Honduras)).
- Ressourcen: „en-ES”, „es-HO”.
- Ressource mit der höheren Bewertung: „es-HO”.

## <a name="undetermined-language-und"></a>Undefinierte Sprache („und”)

Das Sprachtag „und” kann zum Angeben einer Ressource verwendet werden, die mit allen Sprachen übereinstimmt, wenn keine bessere Übereinstimmung vorliegt. Sie ähnelt dem BCP-47-Sprachbereich „ *” oder „* -&lt;script&gt;”. Hier sehen Sie ein Beispiel.

- Sprachenliste: „en-US”, „zh-Hans-CN”.
- Ressourcen: „zh-Hans-CN”, „und.”
- Ressource mit dem höherem Wert: „und”.
- Beschreibung: Der Vergleich beginnt mit „en-US”, aber es wird keine Übereinstimmung basierend auf „en” gefunden (weder eine partielle noch eine höhere Übereinstimmung). Da eine Ressource mit dem Tag „und” vorhanden ist, wird diese vom Vergleichsalgorithmus verwendet.

Das Tag „und” ermöglicht, dass mehrere Sprachen eine einzelne Ressource gemeinsam verwenden und dass einzelne Sprachen als Ausnahmen behandelt werden. Beispiel:

- Sprachenliste: „zh-Hans-CN”, „en-US”.
- Ressourcen: „zh-Hans-CN”, „und.”
- Ressource mit dem höherem Wert: „zh-Hans-CN”.
- Beschreibung: Der Vergleich findet eine genaue Übereinstimmung für das erste Element und sucht daher nicht nach der mit „und” bezeichneten Ressource.

„und” kann mit einem Skripttag zum Filtern von Ressourcen nach Skript verwendet werden. Beispiel:

- Sprachenliste: „ru”.
- Ressourcen: „und-Latn”, „und-Cyrl”, „und-Arab”.
- Ressource mit dem höherem Wert: „und-Cyrl”.
- Beschreibung: Der Vergleich findet keine Übereinstimmung für „ru” (weder eine partielle noch höhere), daher wird das Sprachtag „und” zugeordnet. Der dem Sprachtag „ru” zugeordnete Wert für die Skriptunterdrückung „Cyrl ”stimmt mit der Ressource „und-Cyrl” überein.

## <a name="orthographic-regional-affinity"></a>Ortografische Regionsaffinität

Wenn zwei Sprachtags mit unterschiedlichen untergeordneten Regionstags abgeglichen werden, besitzen bestimmte Regionspaare vielleicht eine höhere Affinität zueinander als andere. Nur die ähnlichen Gruppen für Englisch („en”) werden unterstützt. Die Regionen-Subtags „PH” (Philippinen) und „LR” (Liberia) besitzen eine orthografische Affinität mit dem Regionen-Subtag „US”. Alle anderen Regionen-Subtags werden dem Regionen-Subtag „GB” (Vereinigtes Königreich) zugeordnet. Wenn sowohl die Ressource „en-US” als auch die Ressource „en-GB” vorhanden sind, erhält eine Sprachenliste mit „en-HK” (Englisch (Hongkong SAR)) eine höhere Bewertung für „en-GB”-Ressourcen als für „en-US”-Ressourcen.

## <a name="handling-languages-with-many-regional-variants"></a>Behandeln von Sprachen mit vielen regionalen Varianten

Bestimmte Sprachen verfügen über viele Muttersprachler in unterschiedlichen Regionen, die verschiedene Varianten der Sprache sprechen. Dies gilt z. B. für Sprachen wie Englisch, Französisch und Spanisch, die auch zu den Sprachen gehören, die in mehrsprachigen Apps am häufigsten unterstützt werden. Zu den regionalen Unterschieden können Abweichungen bei der Schreibweise (z. B. „color” und „colour” im Englischen) oder beim Dialekt gehören, also beispielsweise bei der Wortwahl (z. B. „truck” und „lorry” im Englischen).

Für diese Sprachen mit erheblichen regionalen Varianten stellt sich für eine weltweit einsetzbare App folgende Frage: „Wie viele unterschiedliche regionale Varianten sollen unterstützt werden?” „Welche Varianten?” „Welche Methode zum Verwalten dieser regionalen Ressourcenvarianten ist für meine App die kostengünstigste?” Die Beantwortung dieser Fragen würde den Rahmen dieses Themas sprengen. Die in Windows enthaltenen Mechanismen für den Sprachabgleich umfassen jedoch Funktionen, die Sie beim Behandeln regionaler Varianten unterstützen.

Von Apps wird häufig nur eine Variante einer Sprache unterstützt. Angenommen, eine App verfügt über Ressourcen für nur eine Variante des Englischen, die von englischen Muttersprachlern unabhängig von der Region verwendet werden soll, aus der sie stammen. In diesem Fall würde das Tag „en” ohne Regionen-Subtag auf diese Zielsetzung hinweisen. Für Apps werden jedoch oft Tags wie „en-US” verwendet, die ein Regionen-Subtag enthalten. In diesem Fall funktioniert dies auch: Von der App wird nur eine Variante des Englischen verwendet. Windows führt den Abgleich einer Ressource, die mit einem Tag für eine regionale Variante versehen ist, mit einer vom Benutzer bevorzugten Sprache entsprechend durch.

Wenn zwei oder mehr regionale Varianten unterstützt werden sollen, kann ein Unterschied zwischen „en” und „en-US” jedoch eine erhebliche Auswirkung auf die Benutzererfahrung haben. Dabei ist es dann wichtig zu berücksichtigen, welche Regionen-Subtags verwendet werden.

Angenommen, Sie möchten für das in Kanada und in Europa gesprochene Französisch separate Lokalisierungen bereitstellen. Für Französisch (Kanada) können Sie „fr-CA” verwenden. Für Muttersprachler in Europa wird für die Lokalisierung Französisch (Frankreich) verwendet, sodass sich „fr-FR” eignet. Was gilt dann aber Benutzer aus Belgien, deren sprachliche Vorliebe „fr-BE” ist? Die Region „BE” unterscheidet sich sowohl von „FR” als auch von „CA”, sodass für beide die Übereinstimmung „beliebige Region” geeignet wäre. Für Französisch ist jedoch Frankreich die bevorzugte Region, sodass in diesem Fall „fr-FR” als beste Übereinstimmung angesehen wird.

Angenommen, Sie haben die App zuerst nur für eine Variante des Französischen lokalisiert, also die Zeichenfolgen für Französisch (Frankreich) mit der generischen Qualifizierung „fr” verwendet, und möchten dann Unterstützung für Frankreich (Kanada) hinzufügen. Es ist sehr wahrscheinlich, dass für Französisch (Kanada) nur bestimmte Ressourcen neu übersetzt werden müssen. Sie können weiterhin alle ursprünglichen Ressourcen verwenden, die Qualifizierung als „fr” beibehalten und nur die kleine Gruppe neuer Ressourcen mit dem Tag „fr-CA” hinzufügen. Wenn „fr-CA” die bevorzugte Benutzersprache ist, verfügt die Ressource „fr-CA” über eine höhere Übereinstimmungsbewertung als die Ressource „fr”. Falls aber die bevorzugte Benutzersprache eine andere Französisch-Variante ist, ergibt sich für die regionsneutrale Ressource „fr” eine höhere Übereinstimmung als für „fr-CA”.

Nehmen wir als weiteres Beispiel an, Sie möchten für Muttersprachler aus Spanien und Lateinamerika separate Lokalisierungen für Spanisch bereitstellen. Angenommen, die Übersetzungen für Lateinamerika werden von einem Anbieter in Mexiko geliefert. Sollten Sie für diese beiden Ressourcengruppen dann „es-ES” (Spanien) und „es-MX” (Mexiko) verwenden? Wenn Sie so vorgehen, kann dies zu Problemen für Muttersprachler aus anderen Regionen Lateinamerikas führen, z. B. Argentinien oder Kolumbien, da für diese die Ressourcen von „es-ES” verwendet werden würden. In diesem Fall gibt es eine bessere Alternative: Sie können mit dem Makroregionen-Subtag (es-419) angeben, dass die Ressourcen für Muttersprachler aus allen Teilen Lateinamerikas und der Karibik verwendet werden sollen.

Regionsneutrale Sprachtags und untergeordnete Tags für die Makroregion können sehr effektiv sein, wenn Sie mehrere regionale Varianten unterstützen möchten. Wenn Sie die Anzahl von erforderlichen separaten Ressourcen reduzieren möchten, können Sie eine Ressource so qualifizieren, dass die jeweils zutreffende größtmögliche Abdeckung widergespiegelt wird. Anschließend können Sie eine Ressource mit breiter Abdeckung bei Bedarf um eine speziellere Variante erweitern. Eine Ressource mit einem regionsneutralen Sprachqualifizierer wird für Benutzer jeder regionalen Variante verwendet, es sei denn, es ist eine andere Ressource mit einem regionsspezifischeren Qualifizierer vorhanden, die für den jeweiligen Benutzer gilt. Die Ressource „en” ergibt beispielsweise eine Übereinstimmung für einen Benutzer von „Englisch (Australien)”. Eine Ressource mit „en-053” (Englisch für Australien oder Neuseeland) ergibt für den Benutzer jedoch eine bessere Übereinstimmung, und die bestmögliche Übereinstimmung wird mit „en-AU” erzielt.

Englisch erfordert hierbei besondere Beachtung. Wenn für eine App die Lokalisierung für zwei Varianten des Englischen hinzugefügt wird, sind dies meist Englisch (UK) und Englisch (Großbritannien), was auch als internationales Englisch bezeichnet wird. Wie oben bereits erwähnt, richten sich auch bestimmte Regionen außerhalb der USA nach der Schreibweise der Vereinigten Staaten. Dies wird beim Windows-Sprachabgleich entsprechend berücksichtigt. Für dieses Szenario wird davon abgeraten, für eine der Varianten das regionsneutrale Tag „en” zu verwenden. Nutzen Sie stattdessen „en-GB” und „en-US”. („en” kann jedoch verwendet werden, wenn eine gegebene Ressource keine separaten Varianten benötigt.) Wenn Sie entweder „en-GB” oder „en-US” durch „en” ersetzen, wird dies die von Windows bereitgestellte orthographische regionale Affinität beeinträchtigen. Falls eine dritte Lokalisierung für Englisch hinzugefügt wird, sollten Sie für die zusätzlichen Varianten je nach Bedarf ein spezifisches Regionen-Subtag oder ein Makroregionen-Subtag angeben (z. B. „en-CA”, „en-AU” oder „en-053”), dabei aber weiterhin „en-GB” und „en-US” verwenden.

## <a name="related-topics"></a>Verwandte Themen

* [Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md)
* [Bcp-47](https://tools.ietf.org/html/bcp47)
* [Grundlegendes zu Benutzerprofil Sprachen und App-Manifest-Sprachen](../design/globalizing/manage-language-and-region.md)
* [Komposition von Makro geografischen (kontinentalen) Regionen, geografischen Teilregionen und ausgewählten wirtschaftlichen und anderen Gruppierungen](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
