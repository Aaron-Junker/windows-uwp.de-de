---
description: Hier finden Sie einen Designleitfaden und Codierungsanweisungen zum Hinzufügen von Steuerelementen und Mustern zu Ihrer Windows-App. Sie finden mehr als 45 leistungsstarke Steuerelemente für die Verwendung mit Ihrer App.
title: Windows-Steuerelemente und -Muster – Entwicklung von Windows-Apps
keywords: UWP-Steuerelemente, Benutzeroberfläche, App-Steuerelemente, Windows-Steuerelemente
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 1e568dfb35f273b4efe045a6a16c944007f1ee7f
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824474"
---
# <a name="controls-for-windows-apps"></a>Steuerelemente für Windows-Apps

![Steuerelemente](../images/controls-2x.png)

In der Windows-App-Entwicklung ist ein <i>Steuerelement</i> ein UI-Element, das Inhalte anzeigt oder Interaktionen ermöglicht. Steuerelemente sind die Bausteine der Benutzeroberfläche. Ein <i>Muster</i> ist eine Anleitung zum Kombinieren verschiedener Steuerelemente, um etwas Neues zu erstellen.

Wir stellen Ihnen mehr als 45 Steuerelemente bereit, angefangen bei einfachen Schaltflächen bis hin zu leistungsstarken Datensteuerelementen wie der Rasteransicht.  Diese Steuerelemente sind Teil des Fluent Design-Systems und können Ihnen bei der Erstellung einer ansprechenden, skalierbaren UI helfen, die auf allen Geräten und Bildschirmgrößen großartig aussieht.

Die Artikel in diesem Abschnitt enthalten Designrichtlinien und Codierungsanweisungen für das Hinzufügen von Steuerelementen und Mustern zu Ihrer Windows-App.

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

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Einige Steuerelemente sind nur in der Windows-UI-Bibliothek (WinUI) verfügbar. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures enthält. Informationen dazu, wie Sie diese abrufen, finden Sie im Artikel mit der [Übersicht über die Windows-UI-Bibliothek und Installationsanweisungen](/uwp/toolkits/winui/).<br/>Ab WinUI 2.2 wurde das Standardformat für viele Steuerelemente dahin gehend aktualisiert, dass abgerundete Ecken verwendet werden. Weitere Informationen finden Sie unter [Eckradius](../style/rounded-corner.md).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

## <a name="alphabetical-index"></a>Alphabetischer Index

Detaillierte Informationen zu bestimmten Steuerelementen und Mustern. 

:::row:::
    :::column:::

- Animierter visueller Player (siehe [Lottie](/windows/communitytoolkit/animations/lottie)) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Feld mit automatischen Vorschlägen](auto-suggest-box.md)
- [Schaltfläche](buttons.md)
- [Kalenderdatumsauswahl](calendar-date-picker.md)
- [Kalenderansicht](calendar-view.md)
- [Kontrollkästchen](checkbox.md)
- [Farbauswahl](color-picker.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Kombinationsfeld](combo-box.md)
- [Befehlsleiste](app-bars.md)
- [Befehlsleisten-Flyout](command-bar-flyout.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Visitenkarte](contact-card.md)
- [Inhaltsdialogfeld](dialogs-and-flyouts/dialogs.md)
- [Inhaltslink](content-links.md)
- [Kontextmenü](menus.md)
- [Datumsauswahl](date-picker.md)
- [Dialogfelder und Flyouts](dialogs-and-flyouts/index.md)
- [Dropdown-Schaltfläche](buttons.md#create-a-drop-down-button) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Flip-Ansicht](flipview.md)
- [Flyout](dialogs-and-flyouts/flyouts.md)
- [Formular](forms.md) (Muster)
- [Rasteransicht](listview-and-gridview.md)
- [Link](hyperlinks.md)
- [Linkschaltfläche](hyperlinks.md#create-a-hyperlinkbutton)
- [Bilder und Bildpinsel](images-imagebrushes.md)
- [Steuerelemente für Freihandeingaben](inking-controls.md)
- [Listenansicht](listview-and-gridview.md)
- [Kartensteuerelement](../../maps-and-location/display-maps.md)
- [Master/Details](master-details.md) (Muster)
- [Medienwiedergabe](media-playback.md)
- [Menüleiste](menus.md#create-a-menu-bar) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Menüflyout](menus.md)
- [Navigationsansicht](navigationview.md) :::image type="icon" source="images/winui-logo-16x16.png":::

    :::column-end:::
    :::column:::

- [Zahlenfeld](number-box.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Parallax-Ansicht](..\motion\parallax.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Kennwortfeld](password-box.md)
- [Bild einer Person](person-picture.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Pivotieren](pivot.md)
- [Statusleiste](progress-controls.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Statusring](progress-controls.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Optionsfeld](radio-button.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Bewertungssteuerelement](rating.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Wiederholungsschaltfläche](buttons.md#create-a-repeat-button)
- [Rich-Edit-Feld](rich-edit-box.md)
- [Rich-Text-Block](rich-text-block.md)
- [Bildlaufanzeige](scroll-controls.md)
- [Suchen](search.md) (Muster)
- [Semantischer Zoom](semantic-zoom.md)
- [Formen](shapes.md)
- [Schieberegler](slider.md)
- [Unterteilte Schaltfläche](buttons.md#create-a-split-button) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Geteilte Ansicht](split-view.md)
- [Wischen-Steuerelement](swipe.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Registerkartenansicht](tab-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Unterrichtstipp](dialogs-and-flyouts/teaching-tip.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Textblock](text-block.md)
- [Textfeld](text-box.md)
- [Uhrzeitauswahl](time-picker.md)
- [Umschalter](toggles.md)
- [Umschalter](buttons.md)
- [Unterteilte Umschaltfläche](buttons.md#create-a-toggle-split-button)
- [QuickInfos](tooltips.md)
- [Strukturansicht](tree-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Ansicht mit zwei Bereichen](two-pane-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Webansicht](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML-Steuerelementekatalog

Rufen Sie die _XAML-Steuerelementekatalog_-App aus dem Microsoft Store ab, um diese Steuerelemente und das Fluent Design-System in Aktion zu sehen. Die App ist eine interaktive Ergänzung zu dieser Website. Wenn Sie sie installiert haben, können Sie Links auf einzelnen Steuerungsseiten verwenden, um die App zu starten und das Steuerelement in Aktion zu sehen.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>Zusätzliche Steuerelemente

Zusätzliche Steuerelemente für die Windows-Entwicklung werden von Unternehmen wie <a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> und <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a> bereitgestellt. Diese Steuerelemente bieten zusätzliche Unterstützung für Unternehmen und .NET-Entwickler, indem sie die Steuerelemente des Standardsystems um benutzerdefinierte Steuerelemente und Dienste erweitern.
