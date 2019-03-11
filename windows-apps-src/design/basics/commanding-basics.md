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
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654055"
---
# <a name="command-design-basics-for-uwp-apps"></a>Befehlsdesigngrundlagen für UWP-Apps

In einer app (Universelle Windows Plattform) *Befehl Elemente* sind interaktive Elemente der Benutzeroberfläche, mit denen Benutzer Aktionen wie z. B. eine e-Mail zu senden, das Löschen eines Elements oder Senden eines Formulars durchführen zu können. *Befehl Schnittstellen* bestehen aus common Command-Elemente, die die Befehls-Oberflächen, die sie hosten, die Interaktionen, die sie unterstützen und die Erfahrungen, die sie bereitstellen.

## <a name="provide-the-best-command-experience"></a>Geben Sie den Befehl am besten

Der wichtigste Aspekt einer Befehl-Schnittstelle ist, was Sie versuchen, einen Benutzer ausführen können. Wie Sie die Funktionalität Ihrer Anwendung planen, sollten Sie die erforderlichen Schritte zum Ausführen dieser Vorgänge und die Benutzeroberflächen, die Sie aktivieren möchten. Wenn Sie einen ersten Entwurf diese Benutzeroberflächen abgeschlossen haben, können Sie Entscheidungen auf die Tools und Interaktionen zur Implementierung vornehmen.

Hier sind einige allgemeine Funktionen der Befehl ein:

- Senden oder Übermitteln von Informationen
- Auswählen von Einstellungen und Auswahlmöglichkeiten
- Suchen und Filtern von Inhalten
- Öffnen, Speichern und Löschen von Dateien
- Inhalte bearbeiten oder erstellen

Seien Sie kreativ mit dem Entwurf Befehl beizutragen. Wählen Sie, welche Eingabegeräte Ihrer app unterstützt, und wie Ihre app für jedes Gerät reagiert. Unterstützt die breiteste Palette von Funktionen und Einstellungen stellen Sie Ihre app als verwendet werden kann, portabel und wie möglich zugegriffen werden kann (finden Sie unter [Befehle Design für universelle Windows-Plattform (UWP) apps](../controls-and-patterns/commanding.md) Einzelheiten).



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Wählen Sie die Elemente für die right-Befehl

Verwenden die richtigen Elemente in eine Befehlsschnittstelle möglich, dass den Unterschied zwischen eine intuitive, leicht zu bedienende-app und eine schwierig und verwirrend. Ein umfassender Satz von Command-Elemente sind in die universelle Windows-Plattform (UWP) verfügbar. Hier ist eine Liste mit einigen der am häufigsten verwendeten Elemente der UWP-Befehl.

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

## <a name="place-commands-on-the-right-surface"></a> Platzieren von Befehlen auf der passenden Oberfläche

Sie können die Befehlselemente auf eine Anzahl von Surfaces in Ihrer app, einschließlich der app-Canvas oder spezielle Befehl-Containern, z. B. eine Befehlsleiste, Befehlsleiste Flyout, Menü oder Dialogfeld platzieren.

Immer versuchen, die Benutzer, die Inhalte direkt bearbeiten können, anstatt durch Befehle, die sich auf dem Inhalt, z. B. das Ziehen und ablegen, um die Listenelemente neu anordnen und nicht nach oben oder unten Befehlsschaltflächen Verhalten. 

Allerdings kann dies nicht möglich, mit bestimmten Eingabegeräte oder, wenn bestimmter Benutzer Funktionen und Einstellungen, sodass. Klicken Sie in diesen Fällen bereitzustellen Sie so viele Eingabeereignisse visueller Hinweise wie möglich, und platzieren Sie diese Befehlselemente auf einer Oberfläche Befehl in Ihrer app.

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

## <a name="provide-command-feedback"></a>Geben Sie Feedback zu Befehlen 

Feedback zu Befehlen kommuniziert für Benutzer, dass eine Interaktion oder der Befehl wurde erkannt, wie der Befehl interpretiert und verarbeitet wurde und angibt, ob der Befehl erfolgreich oder nicht war. Dadurch können Benutzer zu verstehen, was sie gemacht haben und was sie als Nächstes tun können. Im Idealfall sollte das Feedback natürlich in Ihre Benutzeroberfläche integriert werden, so dass Benutzer nicht unterbrochen werden müssen oder zusätzliche Maßnahmen ergreifen müssen, es sei denn, dies ist absolut notwendig.

> [!NOTE]
> Geben Sie Feedback, und nur bei Bedarf nur dann, wenn sie an anderer Stelle nicht verfügbar ist. Behalten Sie die Benutzeroberfläche Ihrer Anwendung und übersichtlich, es sei denn, Sie Wert hinzufügen.

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

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Flyouts</a> sind einfache kontextbezogene Popups, die durch Tippen oder klicken an einer beliebigen Stelle außerhalb des Flyouts geschlossen werden können.
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

Unabhängig davon, wie gut entworfenen ist der Benutzeroberfläche Ihrer Anwendung, alle Benutzer eine Aktion, die sie möchten, dass sie noch nicht ausführen. Ihre app können in diesen Situationen müssen die Bestätigung für eine Aktion oder durch die Möglichkeit, letzte Aktionen rückgängig zu machen.

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

##  <a name="optimize-for-specific-input-types"></a> Optimieren für bestimmte Eingabearten

Ausführliche Informationen zum Optimieren der Benutzerfreundlichkeit bei einem bestimmten Eingabetyp oder -gerät finden Sie unter [Einführung in die Interaktion](../input/index.md).

