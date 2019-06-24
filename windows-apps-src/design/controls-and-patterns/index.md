---
description: Hier finden Sie einen Designleitfaden und Codierungsanweisungen zum Hinzufügen von Steuerelementen und Mustern zu Ihrer UWP-App. Sie finden mehr als 45 leistungsstarke Steuerelemente für die Verwendung mit Ihrer App.
title: UWP-Steuerelemente und -Muster – Entwicklung von Windows-Apps
keywords: UWP-Steuerelemente, Benutzeroberfläche, App-Steuerelemente
label: Controls & patterns
template: detail.hbs
ms.date: 11/16/2017
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 2ecf82294614114e711483dfdc58cfad36591369
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319559"
---
# <a name="controls-for-uwp-apps"></a>Steuerelemente für UWP-Apps 

![Steuerelemente](../images/controls-2x.png)

In der UWP-App-Entwicklung ist ein <i>Steuerelement</i> ein UI-Element, das Inhalte anzeigt oder Interaktionen ermöglicht. Steuerelemente sind die Bausteine der Benutzeroberfläche. Ein <i>Muster</i> ist eine Anleitung zum Kombinieren verschiedener Steuerelemente, um etwas Neues zu erstellen.

Wir stellen Ihnen mehr als 45 Steuerelemente bereit, angefangen bei einfachen Schaltflächen bis hin zu leistungsstarken Datensteuerelementen wie der Rasteransicht.  Diese Steuerelemente sind Teil des Fluent Design-Systems und können Ihnen bei der Erstellung einer ansprechenden, skalierbaren UI helfen, die auf allen Geräten und Bildschirmgrößen großartig aussieht. 

Die Artikel in diesem Abschnitt enthalten Designrichtlinien und Codierungsanweisungen für das Hinzufügen von Steuerelementen und Mustern zu Ihrer UWP-App. 

## <a name="intro"></a>Einführung

Allgemeine Anweisungen und Codebeispiele für das Hinzufügen und Formatieren von Steuerelementen in XAML und C#.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a></b> <br/>
Es gibt drei wichtige Schritte zum Hinzufügen von Steuerelementen zur App: Hinzufügen eines Steuerelements zu Ihrer App-UI, Festlegen der Eigenschaften für das Steuerelement und Hinzufügen von Code zu den Ereignishandlern des Steuerelements, sodass dieses eine Aktion ausführt.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">Formatieren von Steuerelementen</a></b> <br/>
Das XAML-Framework bietet zahlreiche Anpassungsmöglichkeiten für die App-Darstellung. Sie können mit Stilen die Steuerelementeigenschaften festlegen und diese Einstellungen dann für andere Steuerelemente übernehmen, um so für ein einheitliches Erscheinungsbild zu sorgen.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Abrufen der Windows-UI-Bibliothek
Einige Steuerelemente sind nur in der Windows-UI-Bibliothek verfügbar. Informationen dazu, wie Sie diese abrufen, finden Sie im Artikel mit der [Übersicht über die Windows-UI-Bibliothek und Installationsanweisungen](/uwp/toolkits/winui/).

## <a name="alphabetical-index"></a>Alphabetischer Index 

Detaillierte Informationen zu bestimmten Steuerelementen und Mustern. (Eine nach Funktionen sortierte Liste finden Sie unter <a href="controls-by-function.md">Index der Steuerelemente nach Funktion</a>.)

<div style="column-count: 2; column-gap: 40px; margin-top: 40px;" >
<ul style="margin-top: 0px; padding-top: 0px; list-style-type: none;">
<li style="list-style-type: none;"><a href="auto-suggest-box.md">Feld mit automatischen Vorschlägen</a></li>

<li style="list-style-type: none;"><a href="app-bars.md">Balken</a></li>

<li style="list-style-type: none;"><a href="buttons.md">Schaltflächen</a></li>

<li style="list-style-type: none;"><a href="checkbox.md">Kontrollkästchen </a></li>

<li style="list-style-type: none;"><a href="color-picker.md">Farbauswahl</a></li>

<li style="list-style-type: none;"><a href="contact-card.md">Visitenkarte</a></li>

<li style="list-style-type: none;"><a href="date-and-time.md">Datums- und Uhrzeitsteuerelemente</a></li>

<li style="list-style-type: none;"><a href="dialogs-and-flyouts/index.md">Dialogfelder und Flyouts</a></li>

<li style="list-style-type: none;"><a href="flipview.md">Flip-Ansicht</a></li>

<li style="list-style-type: none;"><a href="forms.md">Formulare</a></li>

<li style="list-style-type: none;"><a href="hyperlinks.md">Hyperlinks</a></li>

<li style="list-style-type: none;"><a href="images-imagebrushes.md">Bilder und Bildpinsel</a></li>

<li style="list-style-type: none;"><a href="inking-controls.md">Steuerelemente für Freihandeingaben</a></li>

<li style="list-style-type: none;"><a href="lists.md">Listen</a></li>

<li style="list-style-type: none;"><a href="../../maps-and-location/controls-map.md">Kartensteuerelement</a></li>

<li style="list-style-type: none;"><a href="master-details.md">Master/Details</a></li>

<li style="list-style-type: none;"><a href="media-playback.md">Medienwiedergabe</a></li>

<li style="list-style-type: none;"><a href="menus.md">Menüs und Kontextmenüs</a></li>

<li style="list-style-type: none;"><a href="navigationview.md">Navigationsansicht</a></li>

<li style="list-style-type: none;"><a href="person-picture.md">Bild einer Person</a></li>

<li style="list-style-type: none;"><a href="pivot.md">Pivot</a></li>

<li style="list-style-type: none;"><a href="progress-controls.md">Statussteuerelemente</a></li>

<li style="list-style-type: none;"><a href="radio-button.md">Optionsfeld</a></li>

<li style="list-style-type: none;"><a href="rating.md">Bewertungssteuerelement</a></li>

<li style="list-style-type: none;"><a href="scroll-controls.md">Steuerelemente für Bildlauf und Schwenken</a></li>

<li style="list-style-type: none;"><a href="search.md">Suche</a></li>

<li style="list-style-type: none;"><a href="semantic-zoom.md">Semantischer Zoom</a></li>

<li style="list-style-type: none;"><a href="shapes.md">Formen</a></li>

<li style="list-style-type: none;"><a href="slider.md">Schieberegler</a></li>

<li style="list-style-type: none;"><a href="split-view.md">Geteilte Ansicht</a></li>

<li style="list-style-type: none;"><a href="text-controls.md">Textsteuerelemente</a></li>


<li style="list-style-type: none;"><a href="toggles.md">Ein-/Ausschalten</a></li>
<li style="list-style-type: none;"><a href="tooltips.md">QuickInfos</a></li>

<li style="list-style-type: none;"><a href="tree-view.md">Strukturansicht</a></li>

<li style="list-style-type: none;"><a href="web-view.md">Webansicht</a></li>
</ul>
</div>

## <a name="xaml-controls-gallery"></a>XAML-Steuerelementekatalog

Rufen Sie die _XAML-Steuerelementekatalog_-App aus dem Microsoft Store ab, um diese Steuerelemente und das Fluent Design-System in Aktion zu sehen. Die App ist eine interaktive Ergänzung zu dieser Website. Wenn Sie sie installiert haben, können Sie Links auf einzelnen Steuerungsseiten verwenden, um die App zu starten und das Steuerelement in Aktion zu sehen.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>Zusätzliche Steuerelemente

Zusätzliche Steuerelemente für die UWP-Entwicklung werden von Unternehmen wie <a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> und <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a> bereitgestellt. Diese Steuerelemente bieten zusätzliche Unterstützung für Unternehmen und .NET-Entwickler, indem sie die Steuerelemente des Standardsystems um benutzerdefinierte Steuerelemente und Dienste erweitern.  

Wenn Sie mehr über diese Steuerelemente erfahren möchten, sehen Sie sich auf GitHub das Beispiel <a href="https://github.com/Microsoft/Windows-appsample-customers-orders-database">Customer Orders Database</a> (Datenbank für Kundenaufträge) an. In diesem Beispiel werden das Rastersteuerelement und die Validierung der Dateneingabe von Telerik verwendet, die Teil der UI für die UWP-Suite sind. Die UI für die UWP-Suite besteht aus einer Sammlung von über 20 Steuerelementen, die als Open-Source-Projekt über die .NET Foundation verfügbar sind.
