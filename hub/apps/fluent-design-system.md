---
description: Lernen Sie das Fluent Design und dessen Integration in Ihre Apps kennen.
title: Fluent Design-System für Windows
keywords: Layout von UWP-Apps, Universelle Windows-Plattform, App-Design, Benutzeroberfläche, Fluent Design-System
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: f3a2575b17cc4228d7c4db273845478aecf65f29
ms.sourcegitcommit: f0936ce8e88d78b1af99998794a8765094f6a487
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72915108"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent Design-System für Ersteller von Windows-Apps

![Fluent Design-Header](images/fluent/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Einführung

Das Fluent Design-System ist unser System zum Erstellen adaptiver, einfühlsamer und schöner Benutzeroberflächen.

## <a name="principles"></a>Prinzipien

**Adaptive: fließende Erlebnisse wirken sich auf jedem Gerät natürlich aus.**

Fluent-Benutzeroberflächen passen sich an die Umgebung an. Eine Fluent-Benutzeroberfläche fühlt sich auf einem Tablet, einem Desktop-PC und einer Xbox gut an. Sie funktioniert sogar mit einem Mixed Reality-Headset hervorragend. Und wenn Sie mehr Hardware hinzufügen, z. B. einen zusätzlichen Monitor für Ihren PC, wird sie für ein Fluent-Erlebnis genutzt.

**Einfühlsam: fließende Oberflächen sind intuitiv und leistungsstark.**

Fluent-Benutzeroberflächen passen sich dem Verhalten und der Absicht an – sie verstehen und antizipieren, was nötig ist. Sie vereinen Menschen und Ideen, egal ob diese sich auf gegenüberliegenden Seiten der Erde befinden oder direkt nebeneinander stehen.

**Schön: fließende Erfahrung ist ansprechend und immersiv**

Durch das Einbeziehen von Elementen der physischen Welt erschließt sich eine Fluent-Benutzeroberfläche etwas Grundsätzliches. Sie nutzt Licht, Schatten, Bewegung, Tiefe und Textur, um Informationen intuitiv und instinktiv zu organisieren.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Umsetzung von Fluent Design für Ihre App mit UWP

![Fluent Design-Logo](images/fluent/fluentdesign_header.png)

In unseren Entwurfsrichtlinien wird beschrieben, wie Sie Fluent Design-Prinzipien auf Apps anwenden. Welche Art von Apps? Viele unserer Richtlinien können zwar auf jede beliebige Plattform angewendet werden, aber wir haben die Universelle Windows-Plattform (UWP) zur Unterstützung von Fluent Design erstellt.

Fluent Design-Features sind in UWP integriert. Einige dieser Funktionen – z. B. effektive Pixel und das universelle Eingabesystem – arbeiten automatisch. Sie müssen keinen zusätzlichen Code schreiben, um sie zu nutzen. Andere Funktionen, z. B. Acryl, sind optional. Sie nehmen sie in Ihre Anwendung auf, indem Sie Code schreiben.

> Wir bringen UWP-Steuerelemente auf den Desktop, damit Sie das Aussehen, Verhalten und die Funktionalität Ihrer vorhandenen WPF- oder Windows-Anwendungen mit Fluent Design-Features verbessern können. Weitere Informationen finden Sie unter [Hosten von UWP-Steuerelementen in WPF- und Windows Forms-Anwendungen](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Zusätzlich zu einer Entwurfsanleitung wird in unseren Fluent Design-Artikeln auch beschrieben, wie Sie den Code für Ihre Entwürfe schreiben. Für UWP wird XAML verwendet. Dies ist eine auf Markup basierende Sprache, mit der die Erstellung von Benutzeroberflächen vereinfacht wird. Beispiel:

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![](images/fluent/xaml-example.png)


> Falls die UWP-Entwicklung neu für Sie ist, helfen Ihnen die Informationen auf unserer [Seite mit den ersten Schritte für UWP](/windows/uwp/get-started) weiter.

## <a name="find-a-natural-fit"></a>Finden einer natürlichen Form

Wie gestaltet man eine App auf einer Vielzahl von Geräten natürlich? Indem sie sich so anfühlt, als wäre sie für jedes einzelne Gerät entwickelt worden. Ein UI-Layout, das sich an verschiedene Bildschirmgrößen anpasst (ohne vergeudeten Platz oder Überladung), schafft ein natürliches Erlebnis – als ob es für dieses Gerät entwickelt wurde.

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**Entwurf für die richtigen Breakpoints**

Anstatt das Design für jede einzelne Bildschirmgröße anzupassen, kann die Konzentration auf einige wenige aber wichtige Größen („Breakpoints”) Ihre Designs und Ihren Code stark vereinfachen und Ihre App gleichzeitig auf kleinen und großen Bildschirmen großartig darstellen.

[Weitere Informationen zu Bildschirmgrößen und Breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**Erstellen eines reaktionsfähigen Layouts**

Damit eine APP naturgemäß ist, sollte Sie das Layout an verschiedene Bildschirmgrößen und Geräte anpassen. Sie können die automatische Größenanpassung, Layoutpanels, visuelle Zustände und sogar separate Benutzeroberflächen Definitionen in XAML verwenden, um eine reaktionsfähige Benutzeroberfläche zu erstellen.

[Weitere Informationen zum reaktionsfähigen Design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**Entwurf für ein Spektrum von Geräten**

UWP-Apps können auf einer Vielzahl von Windows-basierten Geräten ausgeführt werden. Es ist hilfreich zu wissen, welche Geräte verfügbar sind, wofür sie gemacht sind und wie Benutzer mit ihnen interagieren.

[Weitere Informationen zu UWP-Geräten](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**Für die Rechte Eingabe optimieren**

UWP-Apps unterstützen automatisch gängige Maus-, Tastatur-, Stift- und Touch-Interaktionen. Sie müssen sich nicht extra darum kümmern. Aber Sie können Ihre Apps um einer optimierte Unterstützung für bestimmte Eingabemöglichkeiten (z. B. Stift und Surface Dial) erweitern.

[Weitere Informationen zu Eingaben und Interaktionen](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Sicherstellen der Intuitivität

Eine Benutzeroberfläche fühlt sich intuitiv an, wenn sie sich so verhält, wie der Benutzer es erwartet. Durch die Verwendung etablierter Steuerelemente und Muster und die Nutzung der Plattformunterstützung für die Barrierefreiheit und Globalisierung schaffen Sie eine reibungslose Benutzeroberfläche, mit der Benutzer produktiver sein können.

Beim Einfühlungsvermögen geht es darum, das Richtige zur richtigen Zeit zu tun.

Für Fluent-Benutzeroberflächen werden Steuerelemente und Muster konsistent eingesetzt, damit sie sich so verhalten, wie es der Benutzer erwartet. Fluent-Benutzeroberflächen sind für Menschen mit einem breiten Spektrum an körperlichen Fähigkeiten zugänglich und umfassen Globalisierungsfunktionen für Menschen auf der ganzen Welt.

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**Bereitstellen der richtigen Navigation**

Erstellen Sie mit der richtigen App-Struktur und den entsprechenden Navigations Komponenten eine mühelose Darstellung.

[Weitere Informationen zur Navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**Interaktiv sein**

Mithilfe von Schaltflächen, Befehls leisten, Tastenkombinationen und Kontextmenüs können Benutzer mit Ihrer APP interagieren. Dabei handelt es sich um die Tools, die eine statische Darstellung in etwas dynamisches Verhalten ändern.

[Weitere Informationen zu Befehle](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**Verwenden des richtigen Steuer Elements für den Auftrag**

Steuerelemente sind die Bausteine der Benutzeroberfläche. Mit dem richtigen Steuerelement können Sie eine Benutzeroberfläche erstellen, die sich so verhält, wie es die Benutzer erwarten. UWP bietet mehr als 45 Steuerelemente – von einfachen Schaltflüchen bis hin zu leistungsstarken Daten-Steuerelementen.

[Weitere Informationen zu UWP-Steuerelementen](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![Inklusives Image](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**Inklusiv** Eine gut entworfene APP ist für Personen mit Behinderungen zugänglich. Mit zusätzlichem Code können Sie Ihre Apps mit anderen Menschen auf der ganzen Welt teilen.

[Weitere Informationen zur Benutzerfreundlichkeit](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Gestalten auf ansprechende und immersive Weise

Bei Fluent Design geht es nicht um auffällige Effekte. Es werden physikalische Effekte genutzt, die das Benutzererlebnis wirklich verbessern. Hierdurch werden Benutzeroberflächen emuliert, die unser Gehirn effizient verarbeiten kann.

## <a name="use-light"></a>Verwenden von Licht

Licht kann unsere Aufmerksamkeit auf sich ziehen. Es schafft Atmosphäre und Raumgefühl und ist ein praktisches Werkzeug, um Informationen zu beleuchten.

Bringen Sie Licht in Ihre UWP-App:

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**Reveal Highlight**

[Reveal-Highlight](/windows/uwp/design/style/reveal) setzt Licht ein, um interaktive Elemente hervorzuheben. Licht beleuchtet das Element, mit dem der Benutzer interagieren kann und zeigt verborgene Grenzen auf. Reveal ist bei einigen Steuerelementen, wie z. B. der List-Ansicht und der Grid-Ansicht, automatisch aktiviert. Sie können es für andere Steuerelementen aktivieren, indem Sie unsere vordefinierten Reveal-Highlight-Stile verwenden.
:::row-end:::

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**Reveal Focus**

[Reveal-Focus](/windows/uwp/design/style/reveal-focus) verwendet Licht, um die Aufmerksamkeit auf das Element zu lenken, das aktuell den Eingabefokus hat.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Schaffen Sie ein Gefühl von Tiefe

Wir leben in einer dreidimensionalen Welt. Durch die gezielte Integration von Tiefe in die Benutzeroberfläche verwandeln wir eine flache, 2D-Schnittstelle in eine Oberfläche, auf der Informationen und Konzepte durch eine visuelle Hierarchie effizient präsentiert werden. Es wird neu erfunden, wie sich die Dinge in einer mehrschichtigen, physikalischen Umgebung zueinander verhalten.

Bringen Sie Tiefe in Ihre UWP-App:

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**Parallaxeneffekte**

[Parallax](/windows/uwp/design/motion/parallax) Erzeugt die Illusion der Tiefe, indem es Gegenstände im Vordergrund schneller als Gegenstände im Hintergrund erscheinen lässt.
:::row-end:::

## <a name="incorporate-motion"></a>Integrieren von Bewegung

Stellen Sie sich Motion-Design wie einen Film vor. Nahtlose Übergänge halten Sie auf die Geschichte konzentriert und erwecken Erlebnisse zum Leben. Wir können dieses Gefühl in unsere Entwürfe einbringen und Menschen quasi filmisch leicht von einer Aufgabe zur nächsten führen.

Bringen Sie Bewegung in Ihre UWP-App:

:::row:::
    :::column:::
        ![Kontinuitäts GIF](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**Verbundene Animationen**

[Connected-Animations](/windows/uwp/design/motion/connected-animation) helfen dem Benutzer, den Kontext zu behalten, indem sie einen nahtlosen Übergang zwischen den Szenen herstellen.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Verwenden Sie das richtige Material

Die Dinge, die uns in der realen Welt umgeben, sind sinnlich und belebend. Sie können sich biegen, strecken, prallen, zerspringen und gleiten. Diese Materialqualitäten übersetzen sich in digitale Umgebungen, die bewirken, dass Menschen unsere Entwürfe berühren möchten.

Bringen Sie Material in Ihre UWP-App:

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrylic**

[Acrylic](/windows/uwp/design/style/acrylic) Ein transparentes Material, das dem Benutzer Ebenen von Inhalten anzeigt und eine Hierarchie von Oberflächenelementen aufbaut.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Design-Toolkits und Codebeispiele

Sie möchten Ihre eigenen Apps mit Fluent Design erstellen? Unsere Toolkits für Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer und Sketch dienen Ihnen als Hilfe bei Ihrer Entwurfsarbeit, und mit unseren Beispielen können Sie schneller entwickeln.

:::row:::
    :::column:::
        ![Grafik zum Bild](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**Entwerfen von Toolkits und Beispielen (Seite)**

Schauen Sie sich unsere [Seite zu Design-Toolkits und Beispielen](/windows/uwp/design/downloads/) an.
:::row-end:::









