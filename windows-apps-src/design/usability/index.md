---
description: Erfahren Sie, wie Sie Ihre App inklusiv entwickeln und für Personen auf der ganzen Welt zugänglich machen.
keywords: Eingabehilfen für UWP-Apps, Globalisierung, Apps mit inklusivem Design, Anforderungen für App-Eingabehilfen
title: Benutzerfreundlichkeit in Windows-Apps – Entwicklung von Windows-Apps
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: 2fac74f92ec814a84ecc9eb9d2ae64986af7e572
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970575"
---
# <a name="usability-for-windows-apps"></a>Benutzerfreundlichkeit für Windows-Apps

Es sind die Kleinigkeiten und Details, die eine gute Benutzerumgebung zu einer wirklich inklusiven Benutzerumgebung machen, die die Anforderungen von Benutzern auf der ganzen Welt erfüllt.

Mit den Design- und Codierungsanweisungen in diesem Abschnitt können Sie Ihre Windows-App inklusiver gestalten, indem Sie Features für Barrierefreiheit hinzufügen, Globalisierung und Lokalisierung ermöglichen, das Anpassen der Benutzeroberfläche durch die Benutzer zulassen und im Bedarfsfall Hilfe für Benutzer bereitstellen.

## <a name="accessibility"></a>Eingabehilfen

Bei der Barrierefreiheit geht es darum, die App so zu gestalten, dass sie auch von Menschen verwendet werden kann, bei denen bestimmte Beeinträchtigungen die Nutzung herkömmlicher Benutzeroberflächen verhindern oder erschweren. Für bestimmte Situationen gelten gesetzlich vorgeschriebene Vorgaben im Hinblick auf Barrierefreiheit. Es wird jedoch empfohlen, die Barrierefreiheit unabhängig von gesetzlichen Anforderungen zu berücksichtigen, um sicherzustellen, dass Ihre Apps die größtmögliche Benutzergruppe erreichen.

[Portal für Barrierefreiheit](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for Windows apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Windows app apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible Windows apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your Windows app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your Windows app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your Windows app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your Windows app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a Windows app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible Windows app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>App-Einstellungen

Mithilfe von App-Einstellungen kann der Benutzer Ihre App anpassen und entsprechend den eigenen Anforderungen und Wünschen optimieren. Die Bereitstellung der richtigen Einstellungen und ihr ordnungsgemäßes Speichern können eine großartige Benutzerumgebung sogar noch weiter verbessern.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/guidelines-for-app-settings.md">Richtlinien</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Bewährte Methoden für das Erstellen und Anzeigen von App-Einstellungen.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/store-and-retrieve-app-data.md">Speichern und Abrufen von App-Daten</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Hier erfahren Sie, wie Sie lokale, Roaming- und temporäre App-Daten speichern und abrufen können.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">Guidelines</a></b><br/>Best practices for creating and displaying app settings.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">Store and retrieve app data</a></b><br/>How to store and retrieve local, roaming, and temporary app data.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul> -->

## <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung

Windows wird auf der ganzen Welt von Anwendern unterschiedlicher Sprachen, Kulturen und Herkunft verwendet. Ihre Benutzer sprechen eine Vielzahl verschiedener Sprachen in vielen verschiedenen Ländern und Regionen. Einige Benutzer sprechen mehrere Sprachen. Ihre App wird also auf Konfigurationen ausgeführt, die viele Systemeinstellungen für Sprache, Region und Kultur beinhalten. Sie können das Marktpotenzial Ihrer App steigern, indem Sie mithilfe von *Globalisierung* und *Lokalisierung* eine variable App entwickeln.

<a href="../globalizing/globalizing-portal.md">Globalisierungs- und Lokalisierungsportal</a>

## <a name="in-app-help"></a>In-App-Hilfe
Unabhängig davon, wie optimal Sie Ihre App gestaltet haben, werden einige Benutzer etwas zusätzliche Hilfe benötigen.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/guidelines-for-app-help.md">Anleitungen für die App-Hilfe</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Anwendungen können sehr komplex sein, und Sie können die Erfahrung für die Benutzer durch Bereitstellen einer effektiven Hilfe erheblich verbessern.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/instructional-ui.md">Selbsterklärende UI</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Manchmal kann es hilfreich sein, Anleitungen bereitzustellen, um Benutzern die Funktionen in Ihrer App zu demonstrieren, die möglicherweise nicht offensichtlich sind, z. B. bestimmte Touchinteraktionen. In diesen Fällen müssen Sie Anleitungen auf der Benutzeroberfläche für die Benutzer bereitstellen, damit sie diese Funktionen, die sie möglicherweise übersehen haben, finden und nutzen können.</p>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/in-app-help.md">In-App-Hilfe</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">In den meisten Fällen empfiehlt es sich, dass die Hilfe innerhalb der App angezeigt wird, wenn der Benutzer sie anzeigen möchte. Beachten Sie beim Erstellen der In-App-Hilfe die folgenden Anleitungen.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/external-help.md">Externe Hilfe</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">In den meisten Fällen empfiehlt es sich, dass die Hilfe innerhalb der App angezeigt wird, wenn der Benutzer sie anzeigen möchte. Beachten Sie beim Erstellen der In-App-Hilfe die folgenden Anleitungen.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">Guidelines for app help</a></b><br/>Applications can be complex, and providing effective help for your users can greatly improve their experience.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">Instructional UI</a></b><br/>Sometimes it can be helpful to teach the user about functions in your app that might not be obvious to them, such as specific touch interactions. In these cases, you need to present instructions to the user through the UI so they can discover and use features they might have missed.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">In-app help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">External help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul> -->

