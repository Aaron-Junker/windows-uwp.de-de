---
description: Erfahren Sie mehr über das Fluent Design-System in der universellen Windows-Plattform (UWP) und darüber, wie Sie es in Ihre Apps integrieren.
title: Fluent Design-System für Windows
keywords: Layout von UWP-Apps, Universelle Windows-Plattform, App-Design, Benutzeroberfläche, Fluent Design-System
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 55dfda48feb87538abc9ae206f93cd577dc63eb8
ms.sourcegitcommit: 4f82bc9e2096d0212f10d633899f4bc19b7fe75d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94973301"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent Design-System für Ersteller von Windows-Apps

![Fluent Design-Header](images/fluent/fluentdesign-app-header.png)

## <a name="introduction"></a>Einführung

Das Fluent Design-System ist unser System zum Erstellen adaptiver, einfühlsamer und schöner Benutzeroberflächen.

## <a name="principles"></a>Prinzipien

**Adaptiv: Fluent-Benutzeroberflächen fühlen sich auf jedem Gerät natürlich an.**

Fluent-Benutzeroberflächen passen sich an die Umgebung an. Eine Fluent-Benutzeroberfläche fühlt sich auf einem Tablet, einem Desktop-PC und einer Xbox gut an. Sie funktioniert sogar mit einem Mixed Reality-Headset hervorragend. Und wenn Sie mehr Hardware hinzufügen, z. B. einen zusätzlichen Monitor für Ihren PC, wird sie für ein Fluent-Erlebnis genutzt.

**Einfühlsam: Fluent-Benutzeroberflächen sind intuitiv und kraftvoll.**

Fluent-Benutzeroberflächen passen sich dem Verhalten und der Absicht an – sie verstehen und antizipieren, was nötig ist. Sie vereinen Menschen und Ideen, egal ob diese sich auf gegenüberliegenden Seiten der Erde befinden oder direkt nebeneinander stehen.

**Wunderschön: Fluent-Benutzeroberflächen sind einnehmend und immersiv.**

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

![XAML-Beispiel](images/fluent/xaml-example.png)


> Falls die UWP-Entwicklung neu für Sie ist, helfen Ihnen die Informationen auf unserer [Seite mit den ersten Schritte für UWP](/windows/uwp/get-started) weiter.

## <a name="find-a-natural-fit"></a>Finden einer natürlichen Form

Wie gestaltet man eine App auf einer Vielzahl von Geräten natürlich? Indem sie sich so anfühlt, als wäre sie für jedes einzelne Gerät entwickelt worden. Ein UI-Layout, das sich an verschiedene Bildschirmgrößen anpasst (ohne vergeudeten Platz oder Überladung), schafft ein natürliches Erlebnis – als ob es für dieses Gerät entwickelt wurde.

:::row:::
    :::column:::
        ![Abbildung des Designs der richtigen Breakpoints.](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**Design der richtigen Breakpoints**

Anstatt das Design für jede einzelne Bildschirmgröße anzupassen, kann die Konzentration auf einige wenige aber wichtige Größen („Breakpoints”) Ihre Designs und Ihren Code stark vereinfachen und Ihre App gleichzeitig auf kleinen und großen Bildschirmen großartig darstellen.

[Mehr über Bildschirmgrößen und Breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Kurzes Video über ein dynamisches Layout.](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**Erstellen eines dynamischen Layouts**

Damit eine App natürlich wirkt, sollte sich das Layout verschiedenen Bildschirmgrößen und Geräten anpassen. In XAML können Sie automatische Größenanpassung, Layoutpanel, visuelle Zustände und sogar getrennte UI-Definitionen verwenden, um eine reaktionsfähige Benutzeroberfläche zu erstellen.

[Weitere Informationen zu dynamischem Design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung des Designs für ein Gerätespektrum.](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**Design für ein Gerätespektrum**

UWP-Apps können auf einer Vielzahl von Windows-basierten Geräten ausgeführt werden. Es ist hilfreich zu wissen, welche Geräte verfügbar sind, wofür sie gemacht sind und wie Benutzer mit ihnen interagieren.

[Mehr über UWP-Geräte](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung, die darstellt, wie Sie die Optimierung für die passende Ausgabe vornehmen.](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**Optimierung für die passende Eingabe**

UWP-Apps unterstützen automatisch gängige Maus-, Tastatur-, Stift- und Touch-Interaktionen. Sie brauchen sich nicht eigens darum zu kümmern. Aber wenn Sie möchten, können Sie Ihre Apps um optimierte Unterstützung für bestimmte Eingabemöglichkeiten erweitern (z. B. Stift und Surface Dial).

[Mehr über Eingaben und Interaktionen](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Sicherstellen der Intuitivität

Eine Benutzeroberfläche fühlt sich intuitiv an, wenn sie sich so verhält, wie der Benutzer es erwartet. Durch die Verwendung etablierter Steuerelemente und Muster und die Nutzung der Plattformunterstützung für die Barrierefreiheit und Globalisierung schaffen Sie eine reibungslose Benutzeroberfläche, mit der Benutzer produktiver sein können.

Beim Einfühlungsvermögen geht es darum, das Richtige zur richtigen Zeit zu tun.

Für Fluent-Benutzeroberflächen werden Steuerelemente und Muster konsistent eingesetzt, damit sie sich so verhalten, wie es der Benutzer erwartet. Fluent-Benutzeroberflächen sind für Menschen mit einem breiten Spektrum an körperlichen Fähigkeiten zugänglich und umfassen Globalisierungsfunktionen für Menschen auf der ganzen Welt.

:::row:::
    :::column:::
        ![Abbildung, die darstellt, wie Sie die richtige Navigation bereitstellen.](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**Bereitstellen der richtigen Navigation**

Sorgen Sie mit der richtigen App-Struktur und den entsprechenden Navigationskomponenten für eine angenehme Benutzererfahrung.

[Weitere Informationen zur Navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung, die darstellt, wie Sie interaktiv sind.](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**Interaktiv sein**

Mithilfe von Schaltflächen, Befehlsleisten, Tastenkombinationen und Kontextmenüs können Benutzer mit Ihrer App interagieren. Dabei handelt es sich um die Tools, die einer statischen Benutzeroberfläche Dynamik verleihen.

[Weitere Informationen zu Befehlen](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![Abbildung, die darstellt, wie Sie das passende Steuerelement für eine Aufgabe verwenden.](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**Das passende Steuerelement für eine Aufgabe**

Steuerelemente sind die Bausteine der Benutzeroberfläche. Mit dem richtigen Steuerelement können Sie eine Benutzeroberfläche erstellen, die sich so verhält, wie es die Benutzer erwarten. UWP bietet mehr als 45 Steuerelemente – von einfachen Schaltflächen bis hin zu leistungsstarken Datensteuerelementen.

[Mehr über UWP-Steuerelemente](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![Inklusiv-Bild](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**Inklusiv sein** Eine gut gestaltete App ist für Menschen mit Behinderungen zugänglich. Mit zusätzlichem Code können Sie Ihre Apps mit anderen Menschen auf der ganzen Welt teilen.

[Mehr über Benutzerfreundlichkeit](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Gestalten auf ansprechende und immersive Weise

Bei Fluent Design geht es nicht um auffällige Effekte. Es werden physikalische Effekte genutzt, die das Benutzererlebnis wirklich verbessern. Hierdurch werden Benutzeroberflächen emuliert, die unser Gehirn effizient verarbeiten kann.

## <a name="use-light"></a>Verwenden von Licht

Licht kann unsere Aufmerksamkeit auf sich ziehen. Es schafft Atmosphäre und Raumgefühl und ist ein praktisches Werkzeug, um Informationen zu beleuchten.

Bringen Sie Licht in Ihre UWP-App:

:::row:::
    :::column:::
        ![Kurzes Video über Reveal Highlight.](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**Reveal Highlight**

[Reveal highlight](/windows/uwp/design/style/reveal) hebt interaktive Elemente mit Lichteffekten hervor. Mithilfe von Lichteffekten werden die Elemente hervorgehoben, mit denen der Benutzer interagieren kann, und dabei verborgene Ränder angezeigt. „Reveal“ ist bei einigen Steuerelementen, wie z. B. der Listen- und der Rasteransicht, automatisch aktiviert. Zur Aktivierung für andere Steuerelemente können Sie unsere vordefinierten Reveal-Highlight-Stile verwenden.
:::row-end:::

:::row:::
    :::column:::
        ![Kurze Video über Reveal Focus.](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**Reveal Focus**

[Reveal Focus](/windows/uwp/design/style/reveal-focus) lenkt mit Lichteffekten die Aufmerksamkeit auf das Element, das aktuell den Eingabefokus hat.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Schaffen Sie ein Gefühl von Tiefe

Wir leben in einer dreidimensionalen Welt. Durch die gezielte Integration von Tiefe in die Benutzeroberfläche verwandeln wir eine flache, 2D-Schnittstelle in eine Oberfläche, auf der Informationen und Konzepte durch eine visuelle Hierarchie effizient präsentiert werden. Es wird neu erfunden, wie sich die Dinge in einer mehrschichtigen, physikalischen Umgebung zueinander verhalten.

Bringen Sie Tiefe in Ihre UWP-App:

:::row:::
    :::column:::
        ![Kurzes Video über Parallax-Scrolling.](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**Parallax**

[Parallax](/windows/uwp/design/motion/parallax) erzeugt eine Tiefenillusion, indem die Bewegung von Gegenständen im Vordergrund schneller als die Bewegung von Gegenständen im Hintergrund erscheint.
:::row-end:::

## <a name="incorporate-motion"></a>Integrieren von Bewegung

Stellen Sie sich Motion-Design wie einen Film vor. Nahtlose Übergänge halten Sie auf die Geschichte konzentriert und erwecken Erlebnisse zum Leben. Wir können dieses Gefühl in unsere Entwürfe einbringen und Menschen quasi filmisch leicht von einer Aufgabe zur nächsten führen.

Bringen Sie Bewegung in Ihre UWP-App:

:::row:::
    :::column:::
        ![Kontinuitäts-GIF](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**Verbundene Animationen**

[Verbundene Animationen](/windows/uwp/design/motion/connected-animation) helfen dem Benutzer, den Kontext beizubehalten, indem ein nahtloser Übergang zwischen den Szenen hergestellt wird.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Verwenden Sie das richtige Material

Die Dinge, die uns in der realen Welt umgeben, sind sinnlich und belebend. Sie können sich biegen, strecken, prallen, zerspringen und gleiten. Diese Materialqualitäten übersetzen sich in digitale Umgebungen, die bewirken, dass Menschen unsere Entwürfe berühren möchten.

Bringen Sie Material in Ihre UWP-App:

:::row:::
    :::column:::
        ![Abbildung einer Acrylic-Ebene.](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrylic**

[Acrylic](/windows/uwp/design/style/acrylic) ist ein transparentes Material, das dem Benutzer Ebenen von Inhalten anzeigt und eine Hierarchie von Oberflächenelementen aufbaut.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Design-Toolkits und Codebeispiele

Sie möchten Ihre eigenen Apps mit Fluent Design erstellen? Unsere Toolkits für Figma, Adobe XD und Sketch dienen Ihnen als Hilfe bei Ihrer Entwurfsarbeit, und mit unseren Beispielen können Sie schneller entwickeln.

:::row:::
    :::column:::
        ![Screenshot der Seite mit den Design-Toolkits und Beispielen.](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**Seite zu Design-Toolkits und Beispielen**

Schauen Sie sich unsere [Seite zu Design-Toolkits und Beispielen](/windows/uwp/design/downloads/) an.
:::row-end:::









