---
Description: Die universellen Entwurfsfeatures von UWP-Apps ermöglichen die Erstellung von Apps, die sich problemlos auf verschiedensten Geräten skalieren lassen.
title: Einführung in das UWP-App-Design (Universelle Windows-Plattform) (Windows-Apps)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 25dc7118fcb7d9c6e997fae458ef7846777b1262
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "65983918"
---
# <a name="introduction-to-uwp-app-design"></a>Einführung in das UWP-App-Design

![Exemplarische Beleuchtungs-App](images/introUWP-header.jpg)

Der Designleitfaden für die universelle Windows-Plattform (UWP) ist eine Ressource, die Sie beim Entwerfen und Erstellen ansprechender und optimierter Apps unterstützen kann.

Es handelt sich dabei nicht um eine Liste von Vorschriften, sondern um ein lebendiges Dokument, das auf unser dynamisches [Fluent Design-System](/windows/apps/fluent-design-system) sowie auf die Bedürfnisse unserer App-Entwicklercommunity abgestimmt wird.

Diese Einführung bietet eine Übersicht über die universellen Entwurfsfeatures, die in jeder UWP-App enthalten sind und dich bei der Erstellung von Benutzeroberflächen (User Interfaces, UIs) unterstützen, die sich problemlos auf verschiedensten Geräten skalieren lassen.

## <a name="effective-pixels-and-scaling"></a>Effektive Pixel und Skalierung

UWP-Apps können auf beliebigen [Windows 10-Geräten](../devices/index.md) ausgeführt werden – von Fernsehern über Tablets bis hin zu PCs. Aber wie lässt sich eine ansprechende UI für unterschiedlichste Geräte und Bildschirmgrößen entwerfen?

![Gleiche App auf verschiedenen Geräten](images/universal-image-1.jpg)

UWP passt UI-Elemente automatisch so an, dass sie auf allen Geräten sowie auf Bildschirmen mit beliebiger Größe gut lesbar und problemlos verwendbar sind.

Wenn Ihre App auf einem Gerät ausgeführt wird, verwendet das System einen Algorithmus, um die Art der Anzeige der UI-Elemente auf dem Bildschirm zu normalisieren. Dieser Skalierungsalgorithmus berücksichtigt den Abstand zum Bildschirm und die Bildschirmdichte (Pixel pro Zoll), um die wahrgenommene Größe (anstelle der physischen Größe) zu optimieren. Der Skalierungsalgorithmus sorgt dafür, dass Schrift mit einem Schriftgrad von 24 Pixeln auf einem drei Meter entfernten Surface Hub ebenso gut lesbar ist wie auf einem 5-Zoll-Smartphone, das nur wenige Zentimeter entfernt ist.

![Sichtabstände für verschiedene Geräte](images/scaling-chart.png)

Aufgrund der Funktionsweise des Skalierungssystems arbeitest du beim Entwerfen deiner UWP-App nicht mit tatsächlichen physischen Pixeln, sondern mit effektiven Pixeln. Effektive Pixel (epx) sind eine virtuelle Maßeinheit und dienen dazu, Layoutdimensionen und Abstände unabhängig von der Pixeldichte auszudrücken. (In unseren Richtlinien werden epx, ep und px synonym verwendet.)

Sie können die Pixeldichte und die tatsächliche Bildschirmauflösung beim Entwerfen ignorieren. Entwerfen Sie stattdessen für die effektive Auflösung (die Auflösung in effektiven Pixeln) für eine Größenklasse (Details finden Sie im Artikel [Bildschirmgrößen und Haltepunkte](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Legen Sie beim Erstellen von Bildschirmmodellen in Bildbearbeitungsprogrammen die DPI auf 72 und die Bildgröße auf effektive Auflösung für die Zielgrößenklasse fest. Eine Liste mit Größenklassen und effektiven Auflösungen findest du im Artikel [Bildschirmgrößen und Haltepunkte](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

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

Da UWP-Apps automatisch für alle Geräte skaliert werden, wird beim Entwerfen einer UWP-App für ein beliebiges Gerät immer die gleiche Struktur verwendet. Beginnen wir mit den Grundlagen der Benutzeroberfläche einer UWP-App.

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

Wie sollten diese Seiten aussehen? Aus Konsistenzgründen haben die meisten Seiten eine gemeinsame Struktur, sodass Benutzer mühelos zwischen und auf App-Seiten navigieren können. Seiten enthalten in der Regel drei Arten von UI-Elementen:

- [Navigationselemente](navigation-basics.md) ermöglichen es Benutzern, die Inhalte auszuwählen, die sie anzeigen möchten.
- [Befehlselemente](commanding-basics.md) initiieren Aktionen wie etwa das Bearbeiten, Speichern oder Freigeben von Inhalten.
- [Inhaltselemente](content-basics.md) zeigen die Inhalte der App an.

![Allgemeines Layoutmuster](../layout/images/page-components.svg)

Weitere Informationen zur Implementierung allgemeiner UWP-App-Muster findest du im [Artikel zum Seitenlayout](../layout/page-layout.md).

Du kannst auch [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) in Visual Studio verwenden, um mit einem Layout für deine App zu beginnen.

## <a name="controls"></a>Steuerelemente

Die UWP-Entwurfsplattform bietet eine Reihe allgemeiner Steuerelemente, die auf allen Windows-Geräten funktionieren und den Prinzipien unseres [Fluent Design-Systems](/windows/apps/fluent-design-system) entsprechen. Diese reichen von einfachen Steuerelementen wie Schaltflächen und Textelementen bis hin zu komplexen Steuerelementen zur Generierung von Listen auf der Grundlage eines Datensatzes und einer Vorlage.

![UWP-Steuerelemente](../style/images/color/windows-controls.svg)

Eine vollständige Liste der UWP-Steuerelemente und der möglichen Muster findest du im [Abschnitt zu Steuerelementen und Mustern](../controls-and-patterns/index.md).

## <a name="style"></a>Format

Allgemeine Steuerelemente übernehmen automatisch das Systemdesign und die Akzentfarbe, funktionieren mit allen Eingabetypen und werden auf allen Geräten ordnungsgemäß skaliert. Sie sind also adaptiv, intuitiv und ansprechend und spiegeln somit das Fluent Design-System wider. Allgemeine Steuerelemente nutzen Licht, Bewegung und Tiefe in ihren Standardstilen. Durch die Verwendung dieser Steuerelemente integrierst du also unser Fluent Design-System in deine App.

Allgemeine Steuerelemente sind zudem hochgradig anpassbar. So kannst du beispielsweise die Vordergrundfarbe eines Steuerelements ändern oder sein Aussehen umfassend verändern. Verwende zum Überschreiben der Standardstile von Steuerelementen entweder die [einfache Formatierung](../controls-and-patterns/xaml-styles.md#lightweight-styling), oder erstelle [benutzerdefinierte Steuerelemente](../controls-and-patterns/control-templates.md) in XAML.

![GIF-Bild zu Akzentfarben](images/intro-style.gif)

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

UWP skaliert deine App zwar automatisch für verschiedene Geräte, du kannst aber auch [deine UWP-App für bestimmte Geräte optimieren](../devices/index.md).

## <a name="usability"></a>Nutzbarkeit

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Bei der Benutzerfreundlichkeit geht es darum, die App für alle Benutzer zugänglich zu machen. Eine wirklich inklusive Benutzerumgebung kommt letztendlich allen zugute. Unter [Benutzerfreundlichkeit in UWP-Apps](../usability/index.md) erfährst du, wie du eine rundum benutzerfreundliche App gestaltest.

Wenn du Apps für ein internationales Publikum entwickelst, solltest du dich ggf. auch mit den Informationen unter [Globalisierung und Lokalisierung](../globalizing/globalizing-portal.md) vertraut machen.

Darüber hinaus solltest du auch [Barrierefreiheitsfeatures](../accessibility/accessibility-overview.md) für Benutzer mit eingeschränktem Seh- oder Hörvermögen oder mit eingeschränkter Mobilität in Betracht ziehen. Wenn du die Barrierefreiheit gleich von Anfang an in deinem Entwurf berücksichtigst, ist die [Umsetzung der Barrierefreiheit in deiner App](../accessibility/accessibility-in-the-store.md) in der Regel keine große Sache.

## <a name="tools-and-design-toolkits"></a>Tools und Design-Toolkits

Nachdem du nun mit den grundlegenden Entwurfsfeatures vertraut bist, kannst du damit beginnen, deine UWP-App zu entwerfen.

Wir stellen verschiedene Tools bereit, um dich dabei zu unterstützen.

- Auf der Seite [Design-Toolkits und Beispiele für UWP-Apps](../downloads/index.md) findest du Toolkits für XD, Illustrator, Photoshop, Framer und Sketch sowie zusätzliche Design-Tools und Downloads für Schriftarten.

- Wie du deinen Computer für die Programmierung von UWP-Apps einrichtest, erfährst du unter [Beginnen &gt; Einrichten](../../get-started/get-set-up.md).

- Anregungen zur UI-Implementierung für UWP findest du in unseren umfassenden [UWP-Beispiel-Apps](https://developer.microsoft.com/windows/samples).

## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>Nächstes Thema: Fluent Design-System

Informationen zu den Prinzipien von Fluent Design (das Entwurfssystem von Microsoft) sowie zu weiteren Features für deine UWP-App findest du im Artikel [Fluent Design-System für Ersteller von Windows-Apps](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Verwandte Artikel

- [Was ist eine UWP-App?](../../get-started/universal-application-platform-guide.md)
- [Fluent Design-System](/windows/apps/fluent-design-system)
- [XAML-Plattform](../../xaml-platform/index.md)
