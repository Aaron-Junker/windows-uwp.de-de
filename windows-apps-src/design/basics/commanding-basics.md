---
Description: Bei den Befehlselementen in einer Universellen Windows-Plattform (UWP)-App handelt es sich um die interaktiven Benutzeroberflächenelemente, mit denen der Benutzer Aktionen durchführen kann, um beispielsweise eine E-Mail zu senden, ein Element zu löschen oder ein Formular zu übermitteln.
title: Befehlsdesigngrundlagen für Apps der universellen Windows-Plattform (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796427"
---
# <a name="command-design-basics-for-uwp-apps"></a>Befehlsdesigngrundlagen für UWP-Apps

Bei den *Befehlselementen* in einer Universellen Windows-Plattform (UWP)-App handelt es sich um die interaktiven Benutzeroberflächenelemente, mit denen Benutzer Aktionen durchführen können, um beispielsweise eine E-Mail zu senden, ein Element zu löschen oder ein Formular zu übermitteln. *Befehlsschnittstellen* setzen sich aus gängigen Befehlselementen, den Befehlsoberflächen, von denen sie gehostet werden, den von ihnen unterstützten Interaktionen sowie den gebotenen Erfahrungen zusammen.

## <a name="provide-the-best-command-experience"></a>Bieten Sie die beste Benutzerfreundlichkeit für Befehle

Der wichtigste Aspekt einer Befehlsschnittstelle besteht darin, was der Benutzer nach Ihrer Absicht erreichen soll. Bedenken Sie beim Planen der Funktionalität Ihrer App die Schritte, die zum Realisieren der jeweiligen Aufgaben erforderlich sind, sowie die Benutzererfahrung, die sie damit verbinden möchten. Wenn Sie einen ersten Entwurf dieser Erfahrungen skizziert haben, können Sie Entscheidungen zu den Tools und den Interaktionen zu deren Umsetzung treffen.

Einige gängige Benutzererfahrungen im Zusammenhang mit Befehlen:

- Senden oder Übermitteln von Informationen
- Auswählen von Einstellungen und Auswahlmöglichkeiten
- Suchen und Filtern von Inhalten
- Öffnen, Speichern und Löschen von Dateien
- Bearbeiten oder Erstellen von Inhalten

Seien Sie kreativ beim Entwerfen der Benutzererfahrung für Befehle. Wählen Sie aus, welche Eingabegeräte von der App unterstützt werden und wie die App auf jedes Gerät reagiert. Durch Unterstützung der breitesten Palette von Funktionen und Einstellungen machen Sie Ihre Apps so nützlich, portabel und zugänglich wie möglich (weitere Einzelheiten finden Sie unter [Befehlsdesign für Universelle Windows-Plattform (UWP)-Apps](../controls-and-patterns/commanding.md)).



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Auswählen der richtigen Befehlselemente

Die Verwendung der richtigen Elemente in einer Befehlsschnittstelle kann den Unterschied zwischen einer intuitiven, benutzerfreundlichen App und einer schwierigen, verwirrenden App bewirken. In der Universellen Windows-Plattform (UWP) ist eine breite Palette von Befehlselementen verfügbar. Dies ist eine Liste mit einigen der gängigsten UWP-Befehlselemente.

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

Eine vollständige Liste finden Sie unter [Steuerelemente und UI-Elemente](../controls-and-patterns/index.md).

## <a name="place-commands-on-the-right-surface"></a>Platzieren von Befehlen auf der passenden Oberfläche

Sie können Befehlselemente auf einer Reihe von Oberflächen in Ihrer App platzieren, einschließlich der App-Canvas oder spezieller Befehlscontainer, wie Befehlsleisten, Befehlsleisten-Flyouts, Menüleisten und Dialoge.

Lassen Sie Benutzer nach Möglichkeit den Inhalt direkt bearbeiten und nicht über Befehle, die auf den Inhalt einwirken, z. B. Neuanordnen von Listen durch Ziehen und Ablegen anstatt Nach-oben- und Nach-unten-Befehlsschaltflächen. 

Bei bestimmten Eingabegeräten ist dies u. U. nicht möglich, oder wenn bestimmte Benutzerfunktionen und -einstellungen integriert werden. Stellen Sie in diesen Fällen so viele Befehlsoptionen wie möglich bereit, und platzieren Sie diese Befehlselemente auf einer Befehlsoberfläche in der App.

Hier eine Liste mit einigen der gängigsten Befehlsoberflächen.

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Feedback zu Befehlen 

Durch Feedback zu Befehlen wird Benutzern mitgeteilt, dass eine Interaktion oder ein Befehl erkannt wurde, wie der Befehl interpretiert und verarbeitet wurde und ob der Befehl erfolgreich abgeschlossen wurde. Dadurch können Benutzer verstehen, welche Aktion sie ausgeführt haben und welche sie als Nächstes ausführen können. Im Idealfall sollte das Feedback natürlich in Ihre Benutzeroberfläche integriert werden, so dass Benutzer nicht unterbrochen werden oder zusätzliche Maßnahmen ergreifen müssen, es sei denn, dies ist absolut notwendig.

> [!NOTE]
> Geben Sie nur dann Feedback, wenn dies notwendig ist und kein Feedback an anderer Stelle zur Verfügung steht. Halten Sie die Benutzeroberfläche der Anwendung einfach und übersichtlich, es sei denn, sie fügen hilfreiche Funktionen hinzu.

Hier sind einige Möglichkeiten, um Feedback in Ihrer App bereitzustellen.

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Flyouts</a> sind einfache, kontextbezogene Popups, die durch Antippen oder Klicken außerhalb des Flyouts verworfen werden können.
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> Verwenden Sie nicht zu viele Bestätigungsdialogfelder. Diese können zwar sehr hilfreich sein, wenn dem Benutzer ein Fehler unterläuft, bei bewusst durchgeführten Aktionen sind sie jedoch eher hinderlich.

### <a name="when-to-confirm-or-undo-actions"></a>Bestätigen oder Rückgängigmachen von Aktionen

Unabhängig davon, wie durchdacht das Design der UI Ihrer App ist – jeder Benutzer kann einmal eine unabsichtlich eine unerwünschte Aktion ausführen. In einer solchen Situation kann Ihre App hilfreich sein, indem sie eine Bestätigung der Aktion fordert oder eine Möglichkeit zum Rückgängigmachen der kürzlich durchgeführten Aktionen anbietet.

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Optimieren für bestimmte Eingabearten

Ausführliche Informationen zum Optimieren der Benutzerfreundlichkeit bei einem bestimmten Eingabetyp oder -gerät finden Sie unter [Einführung in die Interaktion](../input/index.md).

