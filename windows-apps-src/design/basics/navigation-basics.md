---
Description: Die Navigation in UWP-Apps (Apps für die universelle Windows-Plattform) basiert auf einem flexiblen Modell aus Navigationsstrukturen, Navigationselementen und Funktionen auf Systemebene.
title: Navigationsgrundlagen für UWP-Apps
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1c764eeb57ec8046a93e7fb58e156fa68daea8df
ms.sourcegitcommit: 13fe5d04bdb43c75d0fc4de18c2c3d4ae58ff982
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "64564521"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Navigationsdesigngrundlagen für UWP-Apps

![Navigationsgrundlagen-Header](images/nav/navigation-basics-header.jpg)

Wenn Sie sich eine App als eine Sammlung von Seiten vorstellen, beschreibt der Begriff *Navigation* den Wechselvorgang zwischen Seiten und innerhalb einer Seite. Die Navigation ist der Ausgangspunkt für die Benutzererfahrung und definiert die Art und Weise, in der Nutzer die Inhalte und Funktionen finden, an denen sie interessiert sind. Sie ist sehr wichtig, und es kann schwierig sein, sie richtig zu implementieren.

Es gibt eine große Anzahl von Navigationsmöglichkeiten. Wir könnten:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

Müssen Sie Benutzer eine Reihe von Seiten in der Reihenfolge zu durchlaufen.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

Geben Sie ein Menü, das Benutzer direkt zu einer beliebigen Seite wechseln kann.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

Platzieren Sie alles auf einer Seite, und geben Sie die Filtermechanismen zur Anzeige von Inhalt.
    :::column-end:::
:::row-end:::

Es gibt zwar kein einheitliches Navigationsdesign, das für jede App funktioniert, aber es gibt Prinzipien und Richtlinien, die Ihnen helfen, das richtige Design für Ihre App zu finden.

## <a name="principles-of-good-navigation"></a>Grundsätze guter Navigation

Beginnen wir mit den Grundprinzipien eines guten Navigationsdesigns:

- **Konsistenz:** Erfüllt die Erwartungen der Benutzer.
- **Einfachheit:** Nicht mehr als erforderlich.
- **Klarheit:** Geben Sie eindeutigen Pfaden und Optionen.

### <a name="consistency"></a>Konsistenz

Die Navigation sollte den Erwartungen der Benutzer entsprechen. Mithilfe von [Standardsteuerelemente](#use-the-right-controls) , dass Benutzer verstehen und mit der folgenden Standardkonventionen für Symbole, Speicherort, und formatieren Navigation vorhersagbarste und intuitivste für Benutzer veranlasst.

![Bild mit Seitenkomponenten](images/nav/page-components.svg)

> Benutzer erwarten einige UI-Elemente an Standardpositionen.

### <a name="simplicity"></a>Einfachheit

Weniger Navigationselemente erleichtern den Anwendern die Entscheidungsfindung. Der einfache Zugriff auf wichtige Ziele und das Ausblenden weniger wichtiger Objekte hilft den Benutzern, schneller dorthin zu gelangen, wohin sie wollen.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

Präsentieren Sie Navigationselemente in einer vertrauten Navigationsmenü.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

Überlasten von Benutzern mit vielen Navigationsoptionen.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Klarheit

Klare Pfade ermöglichen eine logische Navigation für den Benutzer. Navigationsmöglichkeiten sichtbar zu machen und Zusammenhänge zwischen den Seiten zu klären, soll verhindern, dass sich die Benutzer „verirren”.

![Beispiel für „Richtig”](images/nav/clarity-image.svg)

> Die Ziele sind klar gekennzeichnet, so dass die Benutzer wissen, wo sie sich befinden.

## <a name="general-recommendations"></a>Allgemeine Empfehlungen

Nehmen wir nun unsere Gestaltungsprinzipien – Konsistenz, Einfachheit und Klarheit – und verwenden wir sie, um einige allgemeine Empfehlungen zu formulieren.

1. Denken Sie an Ihre Benutzer. Verfolgen Sie typische Pfade, die sie durch Ihre App nehmen könnten, und überlegen Sie für jede Seite, warum der Benutzer dort ist und wohin er gehen möchte.

2. Vermeiden Sie umfassende Navigationshierarchien. Wenn Sie über drei Navigationsebenen hinausgehen, riskieren Sie, Ihren Benutzer in einer tiefen Hierarchie zu verlieren, die er nur schwer verlassen kann.

3. Vermeiden Sie „Pogo Sticking”. Pogo Sticking tritt auf, wenn der Benutzer für die Navigation zu zugehörigen Inhalten eine Ebene nach oben und erneut eine nach unten navigieren muss.

## <a name="use-the-right-structure"></a>Verwenden Sie die richtige Navigationsstruktur

Nun, da Sie mit den allgemeinen Navigationsprinzipien vertraut sind, überlegen wir uns die Strukturierung Ihrer App. Es gibt zwei allgemeine Strukturen: Flache und hierarchische.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

In einer flachen/lateralen Struktur stehen die Seiten parallel zueinander. Sie können in beliebiger Reihenfolge von einer Seite zur nächsten wechseln.

Wir empfehlen die Verwendung einer flachen Struktur bei:

- Die Seiten können in beliebiger Reihenfolge angezeigt werden.
- Die Seiten sind deutlich voneinander abgegrenzt und verfügen nicht über eine offensichtliche Beziehung zwischen über- und untergeordneten Elementen.
- Es gibt weniger als 8 Seiten in der Gruppe ein. <br>
(Wenn eine Gruppe mehr Seiten enthält, wird es für Benutzer möglicherweise schwierig, zu verstehen, inwiefern sich die Seiten unterscheiden oder welche Position sie zurzeit in der Gruppe haben. Wenn Sie davon ausgehen, dass dies kein Problem für Ihre App ist, machen Sie aus den Seiten Peers. Ziehen Sie andernfalls eine hierarchische Struktur in Betracht, um die Seiten in zwei oder mehr kleinere Gruppen zu unterteilen.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

In einer hierarchischen Struktur werden Seiten in einer Baumstruktur organisiert. Jede untergeordnete Seite hat ein übergeordnetes Element. Ein übergeordnetes Element kann jedoch eine oder mehrere untergeordnete Seiten haben. Um eine untergeordnete Seite aufzurufen, durchlaufen Sie das übergeordnete Element.

Hierarchische Strukturen sind gut geeignet, um komplexe Inhalte, die sich über viele Seiten erstrecken, zu organisieren. Der Nachteil besteht darin, dass hierarchische Seiten Navigationsmehraufwand verursachen: je tiefer die Struktur, desto mehr Klicks sind für das Wechseln der Seiten erforderlich.

Wir empfehlen eine hierarchische Struktur, wenn:
        
- Seiten, die in einer bestimmten Reihenfolge durchlaufen werden sollen.
- Es gibt eine klare Beziehung zwischen übergeordneten und untergeordneten Seiten.
- In der Gruppe gibt es mehr als 7 Seiten.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

Sie haben wählen Sie nicht auf eine Struktur oder das andere; Viele gut-Design-apps verwenden. Eine App verwendet flache Strukturen für Seiten auf obersten Ebenen, die in beliebiger Reihenfolge angezeigt werden können, und hierarchische Strukturen für Seiten, die komplexere Beziehungen haben.

Wenn die Navigationsstruktur über mehrere Ebenen verfügt, empfehlen wir, dass Peer-to-Peer-Navigationselemente nur mit den Peers innerhalb der aktuellen Unterstruktur verknüpft sind. Betrachten Sie eine Navigationsstruktur angezeigt, die zwei Ebenen die angrenzende Abbildung aus:

- Auf Ebene 1 sollte das Peer-to-Peer-Navigationselement Zugriff auf die Seiten A, B, C und D ermöglichen.
- Auf Ebene 2 sollten die Peer-to-Peer Navigationselemente für die A2-Seiten nur mit den anderen A2-Seiten verknüpft werden. Sie sollten nicht mit Seiten auf Ebene 2 in der C-Unterstruktur verknüpft sein.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Verwenden der richtigen Steuerelemente

Sobald Sie sich für eine Seitenstruktur entschieden haben, müssen Sie entscheiden, wie der Benutzer durch die Seiten navigieren soll. UWP bietet eine Vielzahl von Navigationssteuerelementen, um ein konsistentes und zuverlässiges Navigationserlebnis in Ihrer App zu gewährleisten.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

Mit wenigen Ausnahmen verwendet jede App mit mehrere Seiten den Frame. In einem typischen Szenario hat die App eine Hauptseite, die den Frame und ein primäres Navigationselement beinhaltet, z. B. ein Navigationssteuerelement. Wählt der Benutzer eine Seite, wird sie durch den Frame geladen und angezeigt.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

Zeigt eine horizontale Liste mit Links zu Seiten auf derselben Ebene an. Die [NavigationView](../controls-and-patterns/navigationview.md) Steuerelement implementiert der oberen Navigationsleiste und Registerkarten von Mustern.
        
Verwenden Sie oben im Navigationsbereich bei:

- Möchten Sie alle Navigationsoptionen für die auf dem Bildschirm anzuzeigen.
- Sie verlangen mehr Speicherplatz für den Inhalt Ihrer app.
- Symbole können nicht Ihre Navigationskategorien klar beschreiben.
        
Mit Registerkarten, wenn:

- Navigationszustand Seite und der Verlauf beibehalten werden sollen.
- Sie erwarten, dass Benutzer häufig Registerkarten wechseln.

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
Ähnlich wie [Navigationsansicht](../controls-and-patterns/navigationview.md), jedoch mit zusätzlicher Unterstützung für Touch- und etwas anders verhalten.
    
Verwenden von Pivot bei:
- Ihre App um Touch Streifen zwischen Kategorien zu ermöglichen.
- Sie möchten Navigationsoptionen, Karussell infintely
- Eine umfangreiche Steuerung der Navigation verhaltensänderungen bei den verschiedenen Kategorien ist nicht erforderlich.

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

Zeigt eine vertikale Liste mit Links zu den übergeordneten Seiten an. Zu verwenden in folgenden Fällen:
        
- Die Seiten befinden sich auf der obersten Ebene.
- Es gibt viele Navigationselementen (mehr als 5)
- Sie erwarten nicht, dass Benutzer häufig zwischen Seiten wechseln werden.

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

Zeigt eine Liste (Masteransicht) der Elemente an. Durch Auswahl eines Elements wird die entsprechende Seite im Detailbereich angezeigt. Zu verwenden in folgenden Fällen:
        
- Sie erwarten, dass Benutzer häufig zwischen untergeordneten Elementen wechseln werden.
- Sie möchten es dem Benutzer ermöglichen, Vorgänge auf hoher Ebene, z. B. Löschen oder Sortieren, für einzelne Elemente oder Gruppen von Elementen durchzuführen, und Sie möchten es dem Benutzer ermöglichen, Details für jedes Element anzuzeigen oder zu aktualisieren.

Master/Details eignet sich gut für E-Mail-Posteingänge, Kontaktlisten und die Dateneingabe.
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

Eingebettete Navigationselemente können im Inhalt einer Seite erscheinen. Im Gegensatz zu anderen Navigationselementen, die für alle Seiten konsistent sein sollten, sind im Inhalt eingebettete Navigationselemente auf jeder Seite einzigartig.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>nächster: Hinzufügen von Navigations-Code zu Ihrer app

Im nächsten Artikel, [Implementierung grundlegender Navigation,](navigate-between-two-pages.md), lernen Sie den Code kennen, die für die Verwendung von Frame-Steuerelementen zur Bereitstellung einer grundlegenden Navigation zwischen zwei Seiten in Ihrer App erforderlich ist.
