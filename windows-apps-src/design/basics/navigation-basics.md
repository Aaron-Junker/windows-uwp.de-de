---
description: Die Navigation in Windows-Apps basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene.
title: Navigationsgrundlagen für Windows-Apps
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ae88d30988ff2c3ccb4e7b32e1fefbf4d8bb9fde
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031123"
---
# <a name="navigation-design-basics-for-windows-apps"></a>Grundlagen des Navigationsdesigns für Windows-Apps

![Header für Navigationsgrundlagen](images/nav/navigation-basics-header.jpg)

Wenn du dir eine App als eine Sammlung von Seiten vorstellst, beschreibt der Begriff *Navigation* den Wechsel zwischen Seiten sowie Bewegungen auf einer Seite. Die Navigation ist die Grundlage der Benutzererfahrung und definiert, wie Benutzer zu den für sie relevanten Inhalten und Features gelangen. Eine gute Navigation ist immens wichtig, aber manchmal nicht ganz einfach umzusetzen.

Bei der Navigation müssen zahlreiche Entscheidungen getroffen werden. Wir haben unter anderem folgende Möglichkeiten:

:::row:::
    :::column:::
        ![Navigationsbeispiel 1](images/nav/nav-1.svg)

Wir können festlegen, dass Benutzer eine Reihe von Seiten in der vorgegebenen Reihenfolge durchlaufen müssen.
    :::column-end:::
    :::column:::
        ![Navigationsbeispiel 2](images/nav/nav-2.svg)

Wie können ein Menü bereitstellen, über das Benutzer direkt zu einer beliebigen Seite gelangen.
    :::column-end:::
    :::column:::
        ![Navigationsbeispiel 3](images/nav/nav-3.svg)

Wir können alles auf einer einzelnen Seite platzieren Filtermechanismen für die Inhalte bereitstellen.
    :::column-end:::
:::row-end:::

Es gibt zwar kein universelles Navigationsdesign, das für jede App geeignet ist, es gibt jedoch Prinzipien und Richtlinien, die dazu beitragen, das passende Design für deine App zu finden.

## <a name="principles-of-good-navigation"></a>Merkmale einer guten Navigation

Beginnen wir mit den Grundprinzipien eines guten Navigationsdesigns:

- **Konsistenz:** Erfülle die Erwartungen des Benutzers.
- **Einfachheit:** Beschränke dich auf das Wesentliche.
- **Klarheit:** Stelle klare Pfade und Optionen bereit.

### <a name="consistency"></a>Konsistenz

Die Navigation muss die Erwartungen des Benutzers erfüllen. Vertraute [Standardsteuerelemente](#use-the-right-controls) und die Einhaltung von Standardkonventionen für Symbole, Positionierung und Stil machen die Navigation für Benutzer berechenbar und intuitiv.

![Abbildung: Seitenkomponenten](images/nav/page-components.svg)

> Benutzer erwarten, dass sich bestimmte UI-Elemente an bestimmten Standardpositionen befinden.

### <a name="simplicity"></a>Einfachheit

Weniger Navigationselemente vereinfachen den Entscheidungsprozess von Benutzern. Müheloser Zugriff auf wichtige Orte sowie das Ausblenden weniger wichtiger Elemente tragen dazu bei, dass Benutzer schneller ans gewünschte Ziel gelangen.

:::row:::
    :::column:::
        ![Erster Screenshot eines grünen Balkens mit einem grünen Häkchen und dem darin enthaltenen Wort „Do“ (machen).](images/nav/do.svg)

        ![Navigationsansicht gut](images/nav/navview-good.svg)

Darstellung von Navigationselementen in einem vertrauten Navigationsmenü
    :::column-end:::
    :::column:::
        ![Negatives Beispiel](images/nav/dont.svg)

        ![Navigationsansicht schlecht](images/nav/navview-bad.svg)

Überforderung der Benutzer durch zahlreiche Navigationsoptionen
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Klarheit

Klare Pfade ermöglichen eine logische Benutzernavigation. Offensichtliche Navigationsoptionen sowie die Verdeutlichung von Zusammenhängen zwischen Seiten sorgen dafür, dass Benutzer nicht die Orientierung verlieren.

![Screenshot einer Modellnachbildung einer Anwendung, in der klare Navigationspfade für einen Benutzer angezeigt werden.](images/nav/clarity-image.svg)

> Ziele sind klar gekennzeichnet, damit die Benutzer wissen, wo sie sich befinden.

## <a name="general-recommendations"></a>Allgemeine Empfehlungen

In diesem Abschnitt entwickeln wir auf der Grundlage der Gestaltungsprinzipien „Konsistenz“, „Einfachheit“ und „Klarheit“ einige allgemeine Empfehlungen.

1. Führe dir deine Benutzer vor Augen. Skizziere typische Pfade durch deine App, und überlege dir für jede Seite, warum der Benutzer dort ist und wohin er wahrscheinlich möchte.

2. Vermeide tiefe Navigationshierarchien. Bei Verwendung von mehr als drei Navigationsebenen besteht die Gefahr, dass der Benutzer die Orientierung verliert.

3. Vermeide sogenanntes „Pogo Sticking”. Pogo Sticking tritt auf, wenn der Benutzer für die Navigation zu verwandten Inhalten eine Ebene nach oben und dann wieder eine Ebene nach unten navigieren muss.

## <a name="use-the-right-structure"></a>Verwenden der richtigen Struktur

Nachdem du nun mit allgemeinen Navigationsprinzipien vertraut bist, beschäftigen wir uns als Nächstes mit der Strukturierung deiner App. Du hast die Wahl zwischen zwei allgemeinen Strukturen: flach und hierarchisch.

:::row:::
    :::column:::
        ![In einer flachen Struktur angeordnete Seiten](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="flatlateral"></a>Flach/lateral

In einer flachen/lateralen Struktur sind die Seiten nebeneinander angeordnet. Sie können in beliebiger Reihenfolge von einer Seite zur nächsten wechseln.

Die Verwendung einer flachen Struktur empfiehlt sich in folgenden Fällen:

- Die Seiten können in beliebiger Reihenfolge angezeigt werden.
- Die Seiten sind deutlich voneinander abgegrenzt und verfügen nicht über eine offensichtliche Beziehung zwischen über- und untergeordneten Elementen.
- Die Gruppe enthält weniger als acht Seiten. <br>
(Sind mehr Seiten vorhanden, können Benutzer möglicherweise nicht ohne Weiteres nachvollziehen, inwiefern sich die Seiten unterscheiden oder wo sie sich innerhalb der Gruppe befinden. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehe andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![In einer Hierarchie angeordnete Seiten](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="hierarchical"></a>Hierarchisch

In einer hierarchischen Struktur werden Seiten in einer baumartigen Struktur angeordnet. Jede untergeordnete Seite hat genau ein übergeordnetes Element. Ein übergeordnetes Element kann jedoch eine oder mehrere untergeordnete Seiten haben. Um eine untergeordnete Seite aufzurufen, durchlaufen Sie das übergeordnete Element.

Hierarchische Strukturen sind gut geeignet, um komplexe Inhalte, die sich über viele Seiten erstrecken, zu strukturieren. Der Nachteil ist ein gewisser Mehraufwand bei der Navigation: je tiefer die Struktur, desto mehr Klicks sind erforderlich, um zwischen den Seiten zu wechseln.

Eine hierarchische Struktur empfiehlt sich in folgenden Fällen:
        
- Die Seiten sollen in einer bestimmten Reihenfolge durchlaufen werden.
- Zwischen den Seiten besteht eine klare hierarchische Beziehung.
- In der Gruppe gibt es mehr als 7 Seiten.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![App mit einer Hybridstruktur](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="combining-structures"></a>Kombinieren von Strukturen

Sie müssen sich nicht zwischen den beiden Strukturen entscheiden: In vielen gut gestalteten Apps wird eine Kombination aus beiden Strukturen verwendet. Eine App kann flache Strukturen für übergeordnete Seiten verwenden, die in beliebiger Reihenfolge angezeigt werden können, und hierarchische Strukturen für Seiten mit komplexeren Beziehungen.

Wenn die Navigationsstruktur über mehrere Ebenen verfügt, empfehlen wir, dass Peer-to-Peer-Navigationselemente nur mit den Peers innerhalb der aktuellen Unterstruktur verknüpft sind. Die Abbildung zeigt eine Navigationsstruktur mit drei Ebenen:

- Auf der ersten Ebene ermöglicht das Peer-to-Peer-Navigationselement den Zugriff auf die Seiten A, B, C und D.
- Auf Ebene 2 sollten die Peer-to-Peer Navigationselemente für die A2-Seiten nur mit den anderen A2-Seiten verknüpft werden. Sie sollten nicht mit Seiten auf Ebene 2 in der C-Unterstruktur verknüpft sein.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Verwenden der richtigen Steuerelemente

Nachdem du dich für eine Seitenstruktur entschieden hast, musst du dir überlegen, wie die Benutzer durch die entsprechenden Seiten navigieren sollen. Die UWP bietet eine Vielzahl von Navigationssteuerelementen, um in deiner App ein konsistentes und zuverlässiges Navigationserlebnis zu gewährleisten.

:::row:::
    :::column:::
        ![Abbildung: Frame](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame)

Für Apps mit mehreren Seiten wird fast immer ein Frame verwendet. Eine App verfügt in der Regel über eine Hauptseite, die den Frame und ein primäres Navigationselement (beispielsweise ein NavigationView-Steuerelement) enthält. Wenn der Benutzer eine Seite auswählt, wird sie durch den Frame geladen und angezeigt.
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung: Registerkarten und Pivotbereiche](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Obere Navigation**](../controls-and-patterns/navigationview.md)

Zeigt eine horizontale Liste mit Links zu Seiten auf der gleichen Ebene an. Das Steuerelement [NavigationView](../controls-and-patterns/navigationview.md) implementiert die Muster für die obere Navigation.
        
Die obere Navigation kann in folgenden Fällen verwendet werden:

- Wenn du alle Navigationsoptionen auf dem Bildschirm anzeigen möchtest
- Wenn du mehr Platz für den Inhalt deiner App benötigst
- Wenn sich deine Navigationskategorien nicht eindeutig durch Symbole darstellen lassen

:::row-end:::

:::row:::
    :::column:::
        ![Abbildung: Registerkarten und Pivotbereiche](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Registerkarten**](../controls-and-patterns/tab-view.md)

Zeigt einen horizontalen Satz von Registerkarten und deren jeweiligen Inhalt an. Die Registerkarte [TabView](../controls-and-patterns/tab-view.md) ist nützlich, um mehrere Seiten (oder Dokumente) anzuzeigen und dem Benutzer die Möglichkeit zu geben, Registerkarten neu anzuordnen, zu öffnen oder zu schließen.
    
Registerkarten können in folgenden Fällen verwendet werden:

- Benutzer sollen Registerkarten dynamisch öffnen, schließen oder neu anordnen können.
- Sie erwarten, dass möglicherweise eine große Anzahl von Registerkarten gleichzeitig geöffnet sind.
- Benutzer sollen Registerkarten auf einfache Weise zwischen Fenstern in Ihrer Anwendung, die mit Registerkarten verwendet, verschieben können, ähnlich wie bei Webbrowsern wie Microsoft Edge.

:::row-end:::

:::row:::
    :::column:::
         ![Abbildung: Registerkarten und Pivotbereiche](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
Ähnlich wie [NavigationView](../controls-and-patterns/navigationview.md), aber mit zusätzlicher Unterstützung von Toucheingaben und etwas anderem Navigationsverhalten.
    
Pivot kann in folgenden Fällen verwendet werden:
- Wenn du in deiner App das Wechseln zwischen Kategorien per Wischgeste ermöglichen möchtest
- Wenn Navigationsoptionen in Form eines Karussells dargestellt werden sollen
- Wenn du keine umfangreiche Kontrolle über das Navigationsverhalten zwischen Kategorien benötigst

:::row-end:::

:::row:::
    :::column:::
        ![Abbildung: Navigationsansicht](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Linke Navigation**](../controls-and-patterns/navigationview.md)

Zeigt eine vertikale Liste mit Links zu übergeordneten Seiten an. Zu verwenden in folgenden Fällen:
        
- Die Seiten befinden sich auf der obersten Ebene.
- Es sind mehr als fünf Navigationselemente vorhanden.
- Sie erwarten nicht, dass Benutzer häufig zwischen Seiten wechseln werden.

:::row-end:::
        
:::row:::
    :::column:::
        ![Abbildung: Master/Details](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/Details**](../controls-and-patterns/master-details.md)

Zeigt eine Liste (Masteransicht) mit Elementen an. Durch Auswählen eines Elements wird die entsprechende Seite im Detailbereich angezeigt. Zu verwenden in folgenden Fällen:
        
- Sie erwarten, dass Benutzer häufig zwischen untergeordneten Elementen wechseln werden.
- Sie möchten es dem Benutzer ermöglichen, Vorgänge auf hoher Ebene, z. B. Löschen oder Sortieren, für einzelne Elemente oder Gruppen von Elementen durchzuführen, und Sie möchten es dem Benutzer ermöglichen, Details für jedes Element anzuzeigen oder zu aktualisieren.

Das Master/Details-Muster eignet sich sehr gut für E-Mail-Postfächer, Kontaktlisten und die Dateneingabe.
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung: Hyperlinks und Schaltflächen](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

Eingebettete Navigationselemente können im Inhalt einer Seite angezeigt werden. Im Gegensatz zu anderen Navigationselementen, die für alle Seiten konsistent sein sollten, sind im Inhalt eingebettete Navigationselemente auf jeder Seite einzigartig.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Nächster Schritt: Hinzufügen von Navigationscode zu deiner App

Im nächsten Artikel ([Implementieren einer grundlegenden Navigation](navigate-between-two-pages.md)) wird der Code behandelt, der erforderlich ist, um unter Verwendung eines Frame-Steuerelements eine grundlegende Navigation zwischen zwei Seiten in deiner App zu ermöglichen.
