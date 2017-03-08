---
author: Jwmsft
Description: "Das Hub-Steuerelement verwendet ein hierarchisches Navigationsmuster, um Apps zu unterstützen, die eine relationale Informationsarchitektur erfordern."
title: Hub-Steuerelemente
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 32a558866457c4a9e9a359f12d43af317fc0f84a
ms.lasthandoff: 02/07/2017

---
# <a name="hub-controlpattern"></a>Hub-Steuerelement/-Muster

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Mit einem Hub-Steuerelement können Sie App-Inhalte in unterschiedlichen, jedoch zusammenhängenden Abschnitten oder Kategorien organisieren. Abschnitte in einem Hub sind für das Durchlaufen in einer bevorzugten Reihenfolge vorgesehen und können als Ausgangspunkt für den Zugriff auf detaillierte Inhalte dienen.

![Beispiel für einen Hub](images/hub_example_tablet.png)

Inhalte in einem Hub können in einer Panoramaansicht dargestellt werden, die Benutzern einen schnellen Überblick über Neuigkeiten sowie verfügbare und wichtige Inhalte bietet. Hubs verfügen in der Regel über eine Seitenüberschrift. Die einzelnen Inhaltsabschnitte erhalten jeweils eine Abschnittsüberschrift.


<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li>[**Hub-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn251843)</li>
<li>[**HubSection-Klasse**](https://msdn.microsoft.com/library/windows/apps/dn251845)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Das Hub-Steuerelement eignet sich gut zum Anzeigen großer Mengen von Inhalten, die in einer Hierarchie angeordnet sind. Hubs wenden beim Durchsuchen und Entdecken neuer Inhalte eine Priorität an und sind daher nützlich zum Anzeigen von Elementen in einem Store oder einer Mediensammlung.

Das Hub-Steuerelement verfügt über verschiedene Funktionen, durch die es gut für die Erstellung eines Inhaltsnavigationsmustern geeignet ist.

-   **Visuelle Navigation**

    Ein Hub ermöglicht das Anzeigen von Inhalten in einem vielseitigen, kurzen, übersichtlichen Array.

-   **Kategorisierung**

    Jeder Hub-Abschnitt ermöglicht die Anordnung seiner Inhalte in einer logischen Reihenfolge.

-   **Gemischte Inhaltstypen**

    Bei gemischten Inhaltstypen sind variable Ressourcengrößen und -verhältnisse üblich. Ein Hub ermöglicht das individuelle und feinsäuberliche Anordnen jedes Inhaltstyps in den einzelnen Hub-Abschnitten.

-   **Variable Breite für Seiten und Inhalte**

    Da es sich um ein Panoramamodell handelt, ermöglicht der Hub variable Abschnittsbreiten. Dies ist ideal für Inhalte mit unterschiedlichen Tiefen oder Mengen.

-   **Flexible Architektur**

    Wenn Sie eine flache App-Architektur beibehalten möchten, können Sie alle Kanalinhalte in eine Hub-Abschnittszusammenfassung einfügen.

Ein Hub ist nur eines von mehreren Navigationselementen, die Ihnen zur Verfügung stehen. Weitere Informationen zu Navigationsmustern und anderen Navigationselementen finden Sie unter [Navigationsdesigngrundlagen für UWP-Apps (Universelle Windows-Plattform)](../layout/navigation-basics.md).

## <a name="hub-architecture"></a>Hub-Architektur

Das Hub-Steuerelement verfügt über ein hierarchisches Navigationsmuster zur Unterstützung von Apps, die eine relationale Informationsarchitektur erfordern. Ein Hub besteht aus verschiedenen Inhaltskategorien, die den einzelnen Bereichsseiten der App zugeordnet sind. Bereichsseiten können in jeder beliebigen Form dargestellt werden, die dem jeweiligen Szenario und den Inhalten des Bereichs am besten entspricht.

![Drahtmodell der hierarchischen App "Essen mit Freunden"](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>Layouts und Schwenken/Bildlauf

Es gibt eine Reihe von Möglichkeiten zum Erstellens eines Layouts und zum Navigieren von Inhalten in einem Hub, stellen Sie nur sicher, dass Inhaltslisten in einem Hub immer senkrecht zur Richtung schwenken, in die der Bildlauf des Hubs erfolgt.

**Horizontale Verschiebung**

![Beispiel für einen horizontal verschobenen Hub](images/controls_hub_horizontal_pan.png)
**Vertikale Verschiebung**

![Beispiel für einen vertikal verschobenen Hub](images/controls_hub_vertical_pan.png)
**Horizontale Verschiebung mit vertikalem Bildlauf der Liste/des Rasters**

![Beispiel für einen horizontal verschobenen Hub mit einer Liste mit vertikalem Bildlauf](images/controls_hub_horizontal_vertical_scroll.png)
**Vertikale Verschiebung mit horizontalem Bildlauf der Liste/des Rasters**

![Beispiel für einen horizontal verschobenen Hub](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>Beispiele

Der Hub bietet eine hohe Flexibilität beim Entwerfen. Dadurch können Sie Apps mit einer großen Auswahl an attraktiven und visuell komplexen UI-Elementen entwerfen. Sie können für die erste Gruppe ein Favoritenbild oder einen Inhaltsabschnitt verwenden. Ein großes Favoritenbild kann sowohl vertikal als auch horizontal zugeschnitten werden, ohne dass der Interessenschwerpunkt verloren geht. Hier sehen Sie ein Beispiel für ein einzelnes Favoritenbild und für seinen Zuschnitt in das Quer- und Hochformat und auf eine schmale Breite.

![zugeschnittenes Favoritenbild für unterschiedliche Fenstergrößen](images/hub_hero_cropped2.png)

Auf mobilen Geräten ist jeweils ein Hub-Abschnitt sichtbar.

![Beispiel für ein Hubmuster auf einem kleinen Bildschirm](images/phone_hub_example.png)

## <a name="recommendations"></a>Empfehlungen

-   Wir empfehlen, den Inhalt zu beschneiden, sodass ein bestimmter Teil davon verkürzt eingeblendet wird, um Benutzern mitzuteilen, dass weitere Inhalte in einem Hub-Abschnitt vorhanden sind.
-   Je nach den Anforderungen Ihrer App können Sie dem Hub-Steuerelement mehrere Hub-Abschnitte hinzufügen, wobei jedes Steuerelement eine bestimmte Funktion erfüllt. Ein Abschnitt kann beispielsweise eine Reihe von Links und Steuerelementen enthalten, während ein anderer Abschnitt als Repository für Miniaturansichten dient. Benutzer können zwischen diesen Abschnitten mit der Geste wechseln, die von dem Hub-Steuerelement unterstützt wird.
-   Das dynamische Umbrechen von Inhalt ist die beste Methode, um verschiedene Fenstergrößen einzuschließen.
-   Wenn Sie viele Hub-Abschnitte haben, sollten Sie das Hinzufügen des semantischen Zooms in Erwägung ziehen. Dadurch können die Abschnitte auch leichter gefunden werden, wenn die App-Größe in eine schmale Breite geändert wird.
-   Es wird empfohlen, dass ein Element in einem Hub-Abschnitt nicht zu einem anderen Hub führt. Stattdessen können Sie interaktive Überschriften für die Navigation zu einem anderen Hub-Abschnitt oder einer anderen Seite verwenden.
-   Der Hub dient als Ausgangspunkt und sollte an die Anforderungen Ihrer App angepasst werden. Sie können die folgenden Aspekte eines Hubs ändern:
    -   Anzahl Abschnitte
    -   Inhaltstyp in den einzelnen Abschnitten
    -   Positionierung und Reihenfolge von Abschnitten
    -   Größe von Abschnitten
    -   Abstand zwischen Abschnitten
    -   Abstand zwischen einem Abschnitt und dem oberen oder unteren Rand des Hubs
    -   Textstil und -größe in Überschriften und Inhalt
    -   Farbe des Hintergrunds, der Abschnitte, Abschnittsüberschriften und Abschnittsinhalte


## <a name="get-the-sample-code"></a>Beispielcode herunterladen
* [Beispiel für XAML-UI-Grundlagen](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-articles"></a>Verwandte Artikel

- [Hub-Klasse](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [Navigationsgrundlagen](../layout/navigation-basics.md)
- [Verwenden von Hubs](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [Beispiel für ein XAML-Hub-Steuerelement](http://go.microsoft.com/fwlink/p/?LinkID=310072)

