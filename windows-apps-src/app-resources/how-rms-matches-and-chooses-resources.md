---
description: Wenn eine Ressource angefordert wird, kann es mehrere Kandidaten geben, für die sich in einem gewissen Maße eine Übereinstimmung mit dem aktuellen Ressourcenkontext ergibt. Das Ressourcenverwaltungssystem analysiert alle Kandidaten und ermittelt den besten Kandidaten für die Rückgabe. In diesem Thema wird dieser Prozess ausführlich und anhand von Beispielen beschrieben.
title: Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: d430aae696b0f021e2412a73f137ea6db826937b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031853"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt
Wenn eine Ressource angefordert wird, kann es mehrere Kandidaten geben, für die sich in einem gewissen Maße eine Übereinstimmung mit dem aktuellen Ressourcenkontext ergibt. Das Ressourcenverwaltungssystem analysiert alle Kandidaten und ermittelt den besten Kandidaten für die Rückgabe. Dies geschieht, indem alle Qualifizierer in Erwägung gezogen werden, alle Kandidaten zu ordnen.

In dieser Rangfolge werden den verschiedenen Qualifizierern unterschiedliche Prioritäten zugewiesen: die Sprache hat die größten Auswirkungen auf die Gesamt Rangfolge, gefolgt von Kontrast, Skalierung usw. Für jeden Qualifizierer werden Kandidaten Qualifizierer mit dem Kontext qualifiziererwert verglichen, um die Qualität der Übereinstimmung zu ermitteln Wie der Vergleich erfolgt, hängt vom Qualifizierer ab.

Ausführliche Informationen zur Vorgehensweise beim Abgleichen von sprach Tags finden Sie unter [Funktionsweise des Ressourcen Verwaltungssystems mit sprach Tags](how-rms-matches-lang-tags.md).

Bei manchen Qualifizierern, wie z. b. Skalierung und Kontrast, gibt es immer ein Minimum an Übereinstimmungen. Beispielsweise entspricht ein Kandidat, der für "Scale-100" qualifiziert ist, einem Kontext von "Scale-400" zu einem kleinen Grad, aber nicht als auch als Kandidat, der für "Scale-200" oder (für eine perfekte Übereinstimmung) "Scale-400" qualifiziert ist.

Für andere Qualifizierer, wie z. b. Sprache oder Heim Region, ist es jedoch möglich, dass ein Vergleich nicht übereinstimmender Vergleiche (und Grad an Übereinstimmungen) erfolgt. Beispielsweise ist ein Kandidat, der für die Sprache "en-US" qualifiziert ist, eine Teil Übereinstimmung für den Kontext "en-GB", aber ein Kandidat, der als "fr" qualifiziert ist, ist keine Entsprechung. Entsprechend stimmt ein Kandidat, der für die Heimatregion als "155" (Westeuropa) qualifiziert ist, mit einem Kontext für einen Benutzer mit einer Heim Regions Einstellung von "fr" gut überein, aber ein Kandidat, der als "US" qualifiziert ist, stimmt nicht überein.

Wenn ein Kandidat ausgewertet wird und für einen Qualifizierer kein vergleichsvergleich vorhanden ist, erhält dieser Kandidat eine Gesamt Rangfolge, die keine Entsprechung ergibt und nicht ausgewählt wird. Auf diese Weise können die Qualifizierer mit höherer Priorität bei der Auswahl der besten Übereinstimmung das größte Gewicht haben, aber auch ein Qualifizierer mit niedriger Priorität kann einen Kandidaten aufgrund einer nicht Übereinstimmung ausschließen.

Ein Kandidat ist in Bezug auf einen Qualifizierer neutral, wenn er nicht für diesen Qualifizierer gekennzeichnet ist. Bei einem beliebigen Qualifizierer ist ein neutraler Kandidat immer eine Übereinstimmung für den Kontext qualifiziererwert, jedoch nur mit einer niedrigeren Qualität der Übereinstimmung als alle Kandidaten, die für diesen Qualifizierer markiert wurden und einen gewissen Grad an Übereinstimmung (genau oder teilweise) aufweist. Wenn wir z. b. Kandidaten für "en-US", "en", "fr" und auch einen sprach neutralen Kandidaten haben, wird für einen Kontext mit dem sprach Qualifizierungswert "en-GB" der Kandidaten in der folgenden Reihenfolge sortiert: "en", "en-US", neutral und "fr". In diesem Fall stimmt "fr" nicht überein, während die anderen Kandidaten mit einem gewissen Grad übereinstimmen.

Der allgemeine Rang folgen Prozess beginnt mit dem Auswerten von Kandidaten in Bezug auf den Qualifizierer mit der höchsten Priorität (Sprache). Nicht-Übereinstimmungen werden entfernt. Die verbleibenden Kandidaten werden hinsichtlich ihrer Qualität der Übereinstimmung für die Sprache sortiert. Wenn Verknüpfungen vorhanden sind, wird der Qualifizierer mit der höchsten Priorität mit der höchsten Priorität berücksichtigt, wobei die Qualität der Übereinstimmung verwendet wird, um zwischen den verknüpften Kandidaten zu unterscheiden. Im Gegensatz dazu wird der Skalierungs Qualifizierer verwendet, um die verbleibenden Verknüpfungen zu unterscheiden, usw. bis zu den zahlreichen Qualifizierern, die für eine ordnungsgemäß geordnete Rangfolge erforderlich sind

Wenn alle Kandidaten aufgrund von Qualifizierern, die nicht dem Kontext entsprechen, nicht mehr berücksichtigt werden, durchläuft das Ressourcen Lade Modul einen zweiten Durchlauf, der nach einem Standard Kandidaten für die Anzeige sucht. Standard Kandidaten werden während der Erstellung der PRI-Datei festgelegt und sind erforderlich, um sicherzustellen, dass immer ein Kandidat für einen beliebigen Lauf Zeit Kontext ausgewählt wird. Wenn ein Kandidat über Qualifizierer verfügt, die nicht mit dem Standardwert identisch sind, wird dieser Ressourcen Kandidat endgültig außer bedacht.

Für alle bereits in Erwägung kommenden Ressourcen Kandidaten prüft der Ressourcen Lader den Kontext qualifiziererwert mit der höchsten Priorität und wählt den Wert aus, der die beste oder beste Standardbewertung aufweist. Jede tatsächliche Übereinstimmung ist besser als eine Standardbewertung.

Wenn eine Verknüpfung vorliegt, wird der Kontext qualifiziererwert mit der höchsten Priorität überprüft, und der Prozess wird fortgesetzt, bis eine am besten geeignete Entsprechung gefunden wird.

## <a name="example-of-choosing-a-resource-candidate"></a>Beispiel für die Auswahl eines Ressourcen Kandidaten
Beachten Sie diese Dateien.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Und nehmen Sie an, dass es sich hierbei um die Einstellungen im aktuellen Kontext handelt.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

Das Ressourcen Verwaltungs System entfernt drei Dateien, da der hohe Kontrast und die deutschsprachige Sprache nicht mit dem durch die Einstellungen definierten Kontext identisch sind. Die diese Kandidaten verlassen.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

Für diese verbleibenden Kandidaten verwendet das Ressourcen Verwaltungs System den Kontext Qualifizierer mit der höchsten Priorität, bei dem es sich um eine Sprache handelt. Die englischen Ressourcen sind einander näher als die französischen Ressourcen, weil Englisch in den Einstellungen vor Französisch aufgeführt ist.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

Im nächsten Schritt verwendet das Ressourcen Verwaltungs System den Kontext Qualifizierer mit der höchsten Priorität, Skalierung. Das ist also die zurückgegebene Ressource.

```console
en/images/logo.scale-400.jpg
```

Sie können die Advanced [**namedresource. ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) -Methode verwenden, um alle Kandidaten in der Reihenfolge abzurufen, in der Sie den Kontext Einstellungen entsprechen. Im soeben durch gelaufene Beispiel gibt **ResolveAll** Kandidaten in dieser Reihenfolge zurück.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Beispiel für das Erstellen einer Fall Back Option
Beachten Sie diese Dateien.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Und nehmen Sie an, dass es sich hierbei um die Einstellungen im aktuellen Kontext handelt.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

Alle Dateien werden gelöscht, da Sie nicht mit dem Kontext verglichen werden. Daher wird ein Standard Durchlauf eingegeben, bei dem der Standardwert (siehe [Kompilierung von Ressourcen manuell mit MakePri.exe](compile-resources-manually-with-makepri.md)) während der Erstellung der PRI-Datei war.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Dadurch bleiben alle Ressourcen, die entweder dem aktuellen Benutzer oder dem Standard entsprechen.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Das Ressourcen Verwaltungs System verwendet den Kontext Qualifizierer mit höchster Priorität, die Sprache, um die benannte Ressource mit der höchsten Bewertung zurückzugeben.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>Wichtige APIs
* [Namedresource. ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
