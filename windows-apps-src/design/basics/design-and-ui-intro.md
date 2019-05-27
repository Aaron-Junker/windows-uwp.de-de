---
Description: Die universelle Entwurfsfunktionen, die in der Hilfe zu jedem UWP-app enthalten, erstellen Sie apps, die über eine Vielzahl von Geräten hinweg reibungslose skalieren.
title: Einführung in das UWP-App-Design (Universelle Windows-Plattform) (Windows-Apps)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 25dc7118fcb7d9c6e997fae458ef7846777b1262
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983918"
---
# <a name="introduction-to-uwp-app-design"></a>Einführung in das UWP-App-Design

![Beispiel für eine Beleuchtungs-App](images/introUWP-header.jpg)

Der Designleitfaden für die Universelle Windows-Plattform (UWP) ist eine Ressource, die Sie beim Entwerfen und Erstellen ansprechender und optimierter Apps unterstützen kann.

Es ist keine Liste von Vorschriften, sondern ein lebendiges Dokument, das entwickelt wurde, um sich entsprechend unseres [Fluent Design-Systems](/windows/apps/fluent-design-system) sowie der Bedürfnisse unserer App-Entwicklungs-Community zu entwickeln.

Diese Einführung bietet einen Überblick über die universellen Designfunktionen, die in jeder UWP-Apps enthalten sind. Es hilft Ihnen, Benutzeroberflächen (UIs) zu erstellen, die sich über eine Vielzahl von Geräten wunderbar skalieren lassen.

## <a name="effective-pixels-and-scaling"></a>Effektive Pixel und Skalierung

Führen Sie die UWP-apps auf allen [Windows 10-Geräte](../devices/index.md), von Ihr Fernsehgerät praktisch auf Ihrem Tablet oder PC. Wie entwerfen Sie eine Benutzeroberfläche, die auf einer Vielzahl von Geräten und Bildschirmgrößen in Ordnung ist?

![Dieselbe App auf verschiedenen Geräten](images/universal-image-1.jpg)

UWP kann, indem Sie die Elemente der Benutzeroberfläche automatisch anpassen, sodass sie lesbar und auf allen Geräten und Bildschirmgrößen mit Interaktion einfach sind.

Wenn Ihre App auf einem Gerät ausgeführt wird, verwendet das System einen Algorithmus, um die Art der Anzeige der UI-Elemente auf dem Bildschirm zu normalisieren. Dieser Skalierungsalgorithmus berücksichtigt den Abstand zum Bildschirm und die Bildschirmdichte (Pixel pro Zoll), um die wahrgenommene Größe (anstelle der physischen Größe) zu optimieren. Mit dem Skalierungsalgorithmus wird sichergestellt, dass der Schriftgrad 24 Pixel auf einem 3 Meter entfernten Surface Hub genauso für den Benutzer lesbar ist wie der Schriftgrad 24 Pixel auf einem 5-Zoll-Smartphone, das nur einige Zentimeter entfernt ist.

![Sichtabstände für verschiedene Geräte](images/scaling-chart.png)

Aufgrund der Funktionsweise des Skalierungssystems beim Entwerfen Ihrer UWP-App, verwenden Sie effektive Pixeln, und nicht die tatsächlichen physischen Pixel. Effektive Pixel (epx) sind eine virtuelle Maßeinheit und werden verwendet, um Layoutabmessungen und -abstände unabhängig von der Pixeldichte auszudrücken. (In unseren Richtlinien werden epx, ep und px austauschbar verwendet.)

Sie können die Pixeldichte und die tatsächliche Bildschirmauflösung beim Entwerfen ignorieren. Entwerfen Sie stattdessen für die effektive Auflösung (die Auflösung in effektiven Pixeln) für eine Größenklasse (Details finden Sie im Artikel [Bildschirmgrößen und Haltepunkte](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Legen Sie beim Erstellen von Bildschirmmodellen in Bildbearbeitungsprogrammen die DPI auf 72 und die Bildgröße auf effektive Auflösung für die Zielgrößenklasse fest. Eine Liste der Größenklassen und effektiven Auflösungen finden Sie in dem Artikel [Bildschirmgrößen und Breakpoints](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Vielfache von vier

:::row:::
    :::column span:::
        The sizes, margins, and positions of UI elements should always be in **multiples of 4 epx** in your UWP apps.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Layout

Da UWP-Apps automatisch für alle Geräte skaliert werden, folgt das Entwerfen einer UWP-App für jedes Gerät derselben Struktur. Beginnen wir mit den Grundlagen der Benutzeroberfläche einer UWP-App.

### <a name="windows-frames-and-pages"></a>Fenster, Rahmen und Seiten

:::row:::
    :::column:::
        When a UWP app is launched on any Windows 10 device, it launches in a [Window](/uwp/api/windows.ui.xaml.window) with a [Frame](/uwp/api/windows.ui.xaml.controls.frame), which can navigate between [Page](/uwp/api/windows.ui.xaml.controls.page) instances.
    :::column-end:::
    :::column:::
        ![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        You can think of your app's UI as a collection of pages. It's up to you to decide what should go on each page, and the relationships between pages.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Seitenlayout

Wie sollten diese Seiten aussehen? Meistens besitzen die Seiten eine gemeinsame Struktur, um Konsistenz bereitzustellen, sodass Benutzer problemlos zwischen und auf App-Seiten navigieren können. Seiten enthalten in der Regel drei Arten von UI-Elementen:

- [Navigationselemente](navigation-basics.md) ermöglichen dem Benutzer, die Inhalte auszuwählen, die er anzeigen möchten.
- [Befehlselemente](commanding-basics.md) initiieren Aktionen wie etwa das Bearbeiten, Speichern oder Freigeben von Inhalten.
- [Inhaltselemente](content-basics.md) zeigen die Inhalte der App an.

![Ein allgemeines Layoutmuster](../layout/images/page-components.svg)

Weitere Informationen zum Implementieren allgemeiner UWP-App Muster finden Sie im [Seitenlayout](../layout/page-layout.md).

Sie können auch das [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) in Visual Studio verwenden, um mit einem Layout für Ihre App zu beginnen.

## <a name="controls"></a>Steuerelemente

Die UWP-Designplattform bietet eine Reihe von allgemeinen Steuerelementen, die auf allen Windows-Geräten funktionieren und den Prinzipien für unser [Fluent Design-System](/windows/apps/fluent-design-system) entsprechen. Diese Steuerelemente reichen von einfachen Steuerelementen wie Schaltflächen und Textelementen bis hin zu ausgeklügelten Steuerelementen, die Listen aus einem Datensatz und einer Vorlage erzeugen können.

![UWP-Steuerelemente](../style/images/color/windows-controls.svg)

Eine vollständige Liste der UWP-Steuerelemente und der -Muster, die Sie aus ihnen erstellen können, finden Sie im Abschnitt [Steuerelemente und Muster](../controls-and-patterns/index.md).

## <a name="style"></a>Format

Die allgemeinen Steuerelemente übernehmen automatisch das Systemdesign und die Akzentfarbe, arbeiten mit allen Eingabetypen und skalieren auf allen Geräten. Auf diese Weise spiegeln sie das Fluent Design System wider – sie sind anpassungsfähig, einfühlsam und schön. Allgemeine Steuerelemente verwenden Licht, Bewegung und Tiefe in ihren Standarddesigns. Durch die Verwendung dieser Steuerelemente integrieren Sie also unser Fluent Design-System in Ihre App.

Allgemeine Steuerelemente sind zudem sehr anpassbar. Sie können die Vordergrundfarbe eines Steuerelements ändern oder sein Aussehen komplett anpassen. Um die standardmäßigen Stile in Steuerelementen zu überschreiben, verwenden Sie [einfache Formatierung](../controls-and-patterns/xaml-styles.md#lightweight-styling) oder erstellen [benutzerdefinierte Steuerelemente](../controls-and-patterns/control-templates.md) in XAML.

![Akzentfarben-Gif](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
        Your UWP app will interact with the broader Windows experience with tiles and notifications in the Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Eingaben

:::row:::
    :::column:::
        UWP apps rely on smart interactions. You can design around a click interaction without having to know or define whether the click comes from a mouse, a stylus, or a tap of a finger. However, you can also design your apps for [specific input modes](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![inputs](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Geräte

![Geräte](../layout/images/size-classes.svg)

Obwohl UWP Ihre App automatisch auf verschiedene Geräte skaliert, können Sie Ihre [UWP-App trotzdem für bestimmte Geräte optimieren](../devices/index.md).

## <a name="usability"></a>Nutzbarkeit

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Bei der Benutzerfreundlichkeit geht es darum, die App für alle Benutzer zugänglich zu machen. Jeder kann von einer wirklich umfassenden Benutzererfahrung profitieren. In [Benutzerfreundlichkeit von UWP-Apps](../usability/index.md) erfahren Sie, wie Sie Ihre App für jedermann benutzerfreundlich gestalten können.

Wenn Sie für ein internationales Publikum entwerfen, sollten Sie sich mit [Globalisierung und Lokalisierung](../globalizing/globalizing-portal.md) vertraut machen.

Und Sie sollten [Features für Bedienungshilfen](../accessibility/accessibility-overview.md) für Benutzer mit eingeschränkter Seh-, Hör- und Bewegungsfreiheit in Betracht ziehen. Wenn die Barrierefreiheit von Anfang an in Ihr Design integriert ist, sollte die [Umsetzung der Barrierefeiheit](../accessibility/accessibility-in-the-store.md) Ihrer App nur sehr wenig Zeit und Mühe in Anspruch nehmen.

## <a name="tools-and-design-toolkits"></a>Tools und Design-Toolkits

Da Sie nun über die grundlegenden Designfunktionen Bescheid wissen, sollten Sie die ersten Schritte beim Design Ihrer UWP-App unternehmen.

Wir bieten eine Vielzahl von Werkzeugen zur Unterstützung Ihres Designprozesses:

- Weitere Informationen für XD, Illustrator, Photoshop, Framer und Sketch-Toolkits sowie zusätzliche Entwicklungstools und Downloads für Schriftarten finden Sie auf der [Design-Toolkits-Seite](../downloads/index.md).

- Um Ihren Computer so einzurichten, dass er das Codieren von UWP-Apps ermöglicht, lesen Sie den Artikel [Erste Schritte &gt;Vorbereiten](../../get-started/get-set-up.md).

- Für Anregungen zur Implementierung von Benutzeroberflächen für die UWP werfen Sie einen Blick auf unsere umfassenden [UWP-Beispiel-Apps](https://developer.microsoft.com/windows/samples).

## <a name="video-summary"></a>Video-Zusammenfassung

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>nächster: Fluent Design-System

Wenn Sie weitere Informationen zu der Prinzipien hinter Fluent Design (das Design-System von Microsoft) und weitere Features für Ihre UWP-App kennenlernen möchten, fahren Sie mit dem Artikel [Fluent Design-System](/windows/apps/fluent-design-system) fort.

## <a name="related-articles"></a>Verwandte Artikel

- [Was ist eine UWP-App?](../../get-started/universal-application-platform-guide.md)
- [Fluent Design-System](/windows/apps/fluent-design-system)
- [Übersicht über die XAML-Plattform](../../xaml-platform/index.md)
