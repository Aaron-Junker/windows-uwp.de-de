---
description: Lernen Sie das Fluent Design und dessen Integration in Ihre Apps kennen.
title: Fluent Design-System für Windows
keywords: Layout von UWP-Apps, Universelle Windows-Plattform, App-Design, Benutzeroberfläche, Fluent Design-System
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 850f721d69eafc0f730abd85093f0cb028f62254
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984940"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Fluent Design-System für Ersteller von Windows-Apps

![Fluent Design-Header](images/fluent/fluentdesign-app-header.jpg)

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

Zusätzlich zu einer Entwurfsanleitung wird in unseren Fluent Design-Artikeln auch beschrieben, wie Sie den Code für Ihre Entwürfe schreiben. Für UWP wird XAML verwendet. Dies ist eine auf Markup basierende Sprache, mit der die Erstellung von Benutzeroberflächen vereinfacht wird. Im Folgenden ein Beispiel:

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
        ![fpo image](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Sicherstellen der Intuitivität

Eine Benutzeroberfläche fühlt sich intuitiv an, wenn sie sich so verhält, wie der Benutzer es erwartet. Durch die Verwendung etablierter Steuerelemente und Muster und die Nutzung der Plattformunterstützung für die Barrierefreiheit und Globalisierung schaffen Sie eine reibungslose Benutzeroberfläche, mit der Benutzer produktiver sein können.

Beim Einfühlungsvermögen geht es darum, das Richtige zur richtigen Zeit zu tun.

Für Fluent-Benutzeroberflächen werden Steuerelemente und Muster konsistent eingesetzt, damit sie sich so verhalten, wie es der Benutzer erwartet. Fluent-Benutzeroberflächen sind für Menschen mit einem breiten Spektrum an körperlichen Fähigkeiten zugänglich und umfassen Globalisierungsfunktionen für Menschen auf der ganzen Welt.

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Gestalten auf ansprechende und immersive Weise

Bei Fluent Design geht es nicht um auffällige Effekte. Es werden physikalische Effekte genutzt, die das Benutzererlebnis wirklich verbessern. Hierdurch werden Benutzeroberflächen emuliert, die unser Gehirn effizient verarbeiten kann.

## <a name="use-light"></a>Verwenden von Licht

Licht kann unsere Aufmerksamkeit auf sich ziehen. Es schafft Atmosphäre und Raumgefühl und ist ein praktisches Werkzeug, um Informationen zu beleuchten.

Bringen Sie Licht in Ihre UWP-App:

:::row:::
    :::column:::
        ![fpo image](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](/windows/uwp/design/style/reveal) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](/windows/uwp/design/style/reveal-focus) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Schaffen Sie ein Gefühl von Tiefe

Wir leben in einer dreidimensionalen Welt. Durch die gezielte Integration von Tiefe in die Benutzeroberfläche verwandeln wir eine flache, 2D-Schnittstelle in eine Oberfläche, auf der Informationen und Konzepte durch eine visuelle Hierarchie effizient präsentiert werden. Es wird neu erfunden, wie sich die Dinge in einer mehrschichtigen, physikalischen Umgebung zueinander verhalten.

Bringen Sie Tiefe in Ihre UWP-App:

:::row:::
    :::column:::
        ![fpo image](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](/windows/uwp/design/motion/parallax) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>Integrieren von Bewegung

Stellen Sie sich Motion-Design wie einen Film vor. Nahtlose Übergänge halten Sie auf die Geschichte konzentriert und erwecken Erlebnisse zum Leben. Wir können dieses Gefühl in unsere Entwürfe einbringen und Menschen quasi filmisch leicht von einer Aufgabe zur nächsten führen.

Bringen Sie Bewegung in Ihre UWP-App:

:::row:::
    :::column:::
        ![continuity gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](/windows/uwp/design/motion/connected-animation) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Verwenden Sie das richtige Material

Die Dinge, die uns in der realen Welt umgeben, sind sinnlich und belebend. Sie können sich biegen, strecken, prallen, zerspringen und gleiten. Diese Materialqualitäten übersetzen sich in digitale Umgebungen, die bewirken, dass Menschen unsere Entwürfe berühren möchten.

Bringen Sie Material in Ihre UWP-App:

:::row:::
    :::column:::
        ![fpo image](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](/windows/uwp/design/style/acrylic) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Design-Toolkits und Codebeispiele

Sie möchten Ihre eigenen Apps mit Fluent Design erstellen? Unsere Toolkits für Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer und Sketch dienen Ihnen als Hilfe bei Ihrer Entwurfsarbeit, und mit unseren Beispielen können Sie schneller entwickeln.

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









