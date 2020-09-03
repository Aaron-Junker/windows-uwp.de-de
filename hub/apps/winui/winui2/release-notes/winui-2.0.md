---
title: WinUI 2.0 – Anmerkungen zu dieser Version
description: Versionshinweise für WinUI 2.0
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: eb1cde864427768ab1adb3b982e4acc5b58906db
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154814"
---
# <a name="windows-ui-library-20"></a>Windows-UI-Bibliothek 2.0

WinUI 2.0 ist die erste öffentliche Version der Windows-Benutzeroberflächenbibliothek.

WinUI stellt die einfachste Möglichkeit dar, hervorragende Fluent Design-Benutzererfahrungen für Windows zu schaffen.

Es enthält zwei NuGet-Pakete:

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml): Steuerelemente und Fluent Design für UWP-Apps. Dies ist das WinUI-Hauptpaket.

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct): Low-Level-APIs für die Verwendung in Middlewarekomponenten.

Du kannst WinUI-Pakete mit dem NuGet-Paket-Manager herunterladen und in deiner App verwenden. Weitere Informationen findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](/uwp/toolkits/winui/getting-started).

WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Wir freuen uns über Fehlerberichte, Featureanforderungen und Communitycodebeiträge im [Repository zur Windows-UI-Bibliothek](https://aka.ms/winui).

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

Oktober 2018

Dies ist die erste Version des [Microsoft.UI.Xaml NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml). Sie enthält offizielle native Fluent-Steuerelemente und -Features für Windows UWP-Apps.

### <a name="new-features"></a>Neue Funktionen

Zu den Steuerelementen und Mustern in dieser Version gehören:

| Feature | Beschreibung |
| --- | --- |
|[AcrylicBrush]( /uwp/api/microsoft.ui.xaml.media.acrylicbrush)| Zeichnet einen Bereich mit einem halbtransparenten Material, das mehrere Effekte verwendet, einschließlich Weichzeichner und einer Rauschtextur.|
|[BitmapIconSource]( /uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| Stellt eine Symbolquelle dar, die eine Bitmap als Inhalt verwendet.|
|[ColorPicker]( /uwp/api/microsoft.ui.xaml.controls.colorpicker)| Stellt ein Steuerelement dar, mit dem ein Benutzer eine Farbe mithilfe eines Farbspektrums, Schiebereglern und einer Texteingabe auswählen kann.|
|[CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|Stellt ein spezialisiertes Flyout dar, das ein Layout für AppBarButton und verwandte Befehlselemente bereitstellt.|
|[DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|Stellt eine Schaltfläche mit einem Chevron dar, die zum Öffnen eines Menüs vorgesehen ist.|
|[FontIconSource ](/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|Stellt eine Symbolquelle dar, die eine Glyphe aus der angegebenen Schriftart verwendet.|
|[MenuBar](/uwp/api/microsoft.ui.xaml.controls.menubar)|Stellt einen spezialisierten Container dar, der eine Reihe von Menüs in einer horizontalen Zeile darstellt, normalerweise oben im Fenster einer App.|
|[MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem)|Stellt ein Menü der obersten Ebene in einem MenuBar-Steuerelement dar.|
|[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)|Stellt einen Container dar, der die Navigation von App-Inhalten ermöglicht. Er weist einen Kopfbereich, eine Ansicht für den Hauptinhalt und einen Menübereich für Navigationsbefehle auf.|
|[ParallaxView](/uwp/api/microsoft.ui.xaml.controls.parallaxview)|Stellt einen Container dar, der die Bildlaufposition eines Vordergrundelements, z. B. einer Liste, an ein Hintergrundelement koppelt, z. B. ein Bild. Bei einem Bildlauf durch das Vordergrundelement wird das Hintergrundelement animiert, um einen Parallaxeneffekt zu erzeugen.|
|[PersonPicture](/uwp/api/microsoft.ui.xaml.controls.personpicture)|Stellt ein Steuerelement dar, das das Avatarbild für eine Person anzeigt, sofern ein solches Bild verfügbar ist. Andernfalls werden die Initialen der Person oder eine allgemeine Glyphe angezeigt.|
|[RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|Stellt ein Steuerelement dar, mit dem ein Benutzer eine Sternbewertung eingeben kann.|
|[RefreshContainer](/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|Stellt ein Containersteuerelement dar, das einen RefreshVisualizer und eine Funktion zum Aktualisieren durch Abrufen für scrollbare Inhalte bereitstellt.|
|[RefreshVisualizer](/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|Stellt ein Steuerelement dar, das animierte Statusindikatoren für die Aktualisierung von Inhalten bereitstellt.|
|[RevealBackgroundBrush](/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|Zeichnet einen Steuerelementhintergrund mit einem Freilegungseffekt mithilfe von Kompositionspinseln und Lichteffekten.|
|[RevealBorderBrush](/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|Zeichnet einen Steuerelementrahmen mit einem Freilegungseffekt mithilfe von Kompositionspinseln und Lichteffekten.|
|[RevealBrush](/uwp/api/microsoft.ui.xaml.media.revealbrush)|Basisklasse für Pinsel, die Kompositionseffekte und Licht verwenden, um die Freilegung des Visual-Entwurfs zu implementieren.|
|[SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton)|Stellt eine Schaltfläche mit zwei Teilen dar, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Standardschaltfläche und der andere ruft ein Flyout auf.|
|[SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|Stellt einen Container dar, der den Zugriff auf kontextabhängige Befehle mithilfe von Toucheingabe-Interaktionen ermöglicht.|
|[SymbolIconSource](/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|Stellt eine Symbolquelle dar, die eine Glyphe aus der Schriftart Segoe MDL2-Ressourcen als Inhalt verwendet.|
|[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|Stellt ein spezialisiertes Befehlsleisten-Flyout dar, das Befehle zur Textbearbeitung enthält.|
|[ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|Stellt eine Schaltfläche mit zwei Teilen dar, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Umschaltfläche und der andere ruft ein Flyout auf.|
|[TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview)|Stellt eine Hierarchieauflistung mit Knoten dar, die das Aus- und Einblenden von geschachtelten Elementen erlauben.|

## <a name="examples"></a>Beispiele

Die Beispiel-App des XAML-Steuerelementkatalogs enthält interaktive Demos und Beispielcode für die Verwendung von WinUI-Steuerelementen.

* XAML-Steuerelementkatalog-App aus dem [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) installieren

* Der XAML-Steuerelementkatalog ist auch ein [Open-Source-Projekt auf GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Dokumentation

Anleitungen für Steuerelemente der Windows-UI-Bibliothek sind in der [Dokumentation zu Steuerelementen der universellen Windows-Plattform](/windows/uwp/design/controls-and-patterns/) enthalten.

API-Referenzdokumente findest du hier: [Windows-UI-Bibliotheks-APIs](/uwp/api/overview/winui/)