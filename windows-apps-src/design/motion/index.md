---
Description: 'Gut gestaltete Bewegungen lassen Apps lebhaft und realistisch erscheinen. Helfen Sie Benutzern dabei, Kontextänderungen zu verstehen, und verbinden Sie Interaktionen mit visuellen Übergängen.'
title: Bewegung und Animation in UWP-Apps
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: 'Windows 10, UWP'
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
---
# <a name="motion-for-uwp-apps"></a>Bewegung für UWP-Apps

![Herobild](images/header-motion2.svg)

In einer App sind fließende Bewegungen wichtig. Sie geben intelligentes Feedback basierend auf dem Verhalten des Benutzers, lassen die Benutzeroberfläche lebendig wirken und unterstützen den Benutzer bei der Navigation in der App. Durch fließende Bewegungen entsteht eine gewisse emotionale Verbindung zwischen einem Benutzer und der digitalen Benutzeroberfläche. Wir gehen von grundlegenden natürlichen Bewegungen aus, die der Benutzer bereits aus der physischen Welt kennt, und erweitern unser System darauf aufbauend.

## <a name="fluent-motion-principles"></a>Prinzipien fließender Bewegungen

### <a name="physical"></a>Physikalisch

Bewegte Objekte verhalten sich wie Objekte in der realen Welt. Fließende, dynamische Bewegungen ermöglichen eine natürliche Verwendung der Benutzeroberfläche und verleihen ihr eine persönliche Note, durch die eine gewisse emotionale Verbindung entsteht.

![UI-Beispiel für physische Bewegung](images/Physical.gif)
> Wenn Sie per Toucheingabe mit der UI interagieren, entspricht die Bewegung auf der Benutzeroberfläche der Geschwindigkeit der Interaktion. Die Toucheingabe ist eine direkte Eingabe. Daher beeinflusst das Objekt, mit dem Sie interagieren, die umgebenden Objekte.

### <a name="functional"></a>Funktionell

Bewegung dient einem Zweck und muss überzeugend sein. Sie unterstützt den Benutzer in komplexen Umgebungen und hilft beim Festlegen der Hierarchie. Bewegung vermittelt den Eindruck einer besseren Leistung und optimiert das Benutzererlebnis, da keine Latenz wahrgenommen wird.

![UI-Beispiel für funktionelle Bewegung](images/functional.gif)
> Seitenübergänge sind zweckorientiert gestaltet. Sie geben Hinweise darauf, wie Seiten zusammenhängen. Sie erfolgen auf eine Weise, die selbst dann als schnell empfunden wird, wenn die Leistung nicht optimal ist.

### <a name="continuous"></a>Kontinuierlich

Durch fließende Bewegungen wird die Aufmerksamkeit des Benutzers geschickt auf bestimmte Punkte gelenkt. Dadurch werden die einzelnen Schritte einer Aufgabe elegant miteinander verknüpft, um den Benutzer bei der Ausführung zu unterstützen.

![UI-Beispiel für kontinuierliche Bewegung](images/continuous3.gif)
> Objekte können sich von Szene zu Szene bewegen oder in einer Szene die Gestalt ändern, um Kontinuität zu schaffen und den Kontext für Benutzer aufrechtzuerhalten.

### <a name="contextual"></a>Kontextbezogen

Dank intelligenter Bewegung erhält der Benutzer eine Rückmeldung, die sich an seiner Interaktion mit der Benutzeroberfläche orientiert. Die Interaktion ist auf den Benutzer ausgerichtet. Die Bewegung muss dem Formfaktor angemessen und dem Szenario entsprechend gestaltet sein. Sie sollte jedem Benutzer vertraut sein.

![UI-Beispiel für kontextuelle Bewegung](images/Contextual.gif)
> Eine Animation sollte mit der Benutzerinteraktion verknüpft sein. Ein Kontextmenü wird dort bereitgestellt, wo es vom Benutzer aktiviert wurde. 

## <a name="motion-articles"></a>Artikel zu Bewegungen

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
