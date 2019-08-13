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
ms.openlocfilehash: 8b33fe420e93c9ce78c625ad365ec8dc10e343ad
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867455"
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
![Abbildung einer Schaltfläche](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>Schaltflächen</b>

<a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Schaltflächen</a> lösen eine sofortige Aktion aus. Beispiele hierfür sind das Senden einer E-Mail oder von Formulardaten oder das Bestätigen einer Aktion in einem Dialogfeld.
:::row-end:::

:::row:::
    :::column:::
![Abbildung einer Liste](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>Listen</b>

<a href="../controls-and-patterns/lists.md" style="text-decoration:none">Listen</a> stellen Elemente in einer interaktiven Liste oder in einem Raster dar. Listen werden normalerweise für viele Optionen oder Anzeigeelemente verwendet. Beispiele sind u.a. Dropdownliste, Listenfeld sowie Listen- und Rasteransicht.
:::row-end:::

:::row:::
    :::column:::
![Abbildung eines Auswahlsteuerelements](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>Auswahlsteuerelemente</b>

Ermöglichen die Auswahl einiger Optionen, z.B. beim Ausfüllen einer Umfrage oder beim Konfigurieren von App-Einstellungen. Beispiele hierfür sind <a href="../controls-and-patterns/checkbox.md">Kontrollkästchen</a>, <a href="../controls-and-patterns/radio-button.md">Optionsfeld</a> und <a href="../controls-and-patterns/toggles.md">Umschalter</a>.
:::row-end:::

:::row:::
    :::column:::
![Abbildung eines Kalenders](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>Auswahlsteuerelemente für Kalender, Datum und Uhrzeit</b>

<a href="../controls-and-patterns/date-and-time.md">Auswahlsteuerelemente für Kalender, Datum und Uhrzeit</a> ermöglichen die Anzeige und Änderung von Datum und Uhrzeit, z.B. beim Erstellen eines Ereignisses oder Festlegen eines Alarms. Beispiele sind Kalenderdatumsauswahl, Kalenderansicht, Datumsauswahl und Uhrzeitauswahl.
:::row-end:::

:::row:::
    :::column:::
![Abbildung einer Textvorhersage](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>Textvorhersage</b>

Bietet Vorschläge, während Benutzer z.B. Daten eingeben oder Abfragen ausführen. Ein Beispiel hierfür ist das <a href="../controls-and-patterns/auto-suggest-box.md">Feld mit automatischen Vorschlägen</a>.<br>
:::row-end:::

Eine vollständige Liste finden Sie unter [Steuerelemente und UI-Elemente](../controls-and-patterns/index.md).

## <a name="place-commands-on-the-right-surface"></a>Platzieren von Befehlen auf der passenden Oberfläche

Sie können Befehlselemente auf einer Reihe von Oberflächen in Ihrer App platzieren, einschließlich der App-Canvas oder spezieller Befehlscontainer, wie Befehlsleisten, Befehlsleisten-Flyouts, Menüleisten und Dialoge.

Lassen Sie Benutzer nach Möglichkeit den Inhalt direkt bearbeiten und nicht über Befehle, die auf den Inhalt einwirken, z. B. Neuanordnen von Listen durch Ziehen und Ablegen anstatt Nach-oben- und Nach-unten-Befehlsschaltflächen. 

Bei bestimmten Eingabegeräten ist dies u. U. nicht möglich, oder wenn bestimmte Benutzerfunktionen und -einstellungen integriert werden. Stellen Sie in diesen Fällen so viele Befehlsoptionen wie möglich bereit, und platzieren Sie diese Befehlselemente auf einer Befehlsoberfläche in der App.

Hier eine Liste mit einigen der gängigsten Befehlsoberflächen.

:::row:::
    :::column:::
![Abbildung einer App-Canvas](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>App-Canvas (Inhaltsbereich)</b>

Wenn ein Befehl ständig für Kernszenarien benötigt wird, legen Sie ihn auf der Canvas ab. Da Sie Befehle in der Nähe von (oder auf) Objekten platzieren können, die durch den Befehl beeinflusst werden, sind auf der Canvas platzierte Befehle besonders komfortabel und intuitiv. Wählen Sie die Befehle für die Canvas aber mit Bedacht. Wenn Sie zu viele Befehle auf der App-Canvas positionieren, gehen dem Benutzer wertvoller Platz und die Übersichtlichkeit verloren. Wenn der Befehl nicht häufig verwendet wird, sollten Sie ihn in eine andere Befehlsoberfläche einfügen.
:::row-end:::

:::row:::
    :::column:::
![Abbildung einer Befehlsleiste](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>Befehls- und Menüleisten</b>

<a href="../controls-and-patterns/app-bars.md">Befehlsleisten</a> helfen beim Organisieren von Befehlen und machen sie leicht zugänglich. Befehlsleisten können oben oder unten auf dem Bildschirm bzw. oben und unten auf dem Bildschirm platziert werden (eine <a href="../controls-and-patterns/menus.md#create-a-menu-bar">Menüleiste</a> kann auch verwendet werden, wenn die Funktionalität in Ihrer Anwendung zu komplex für eine Befehlsleiste ist).
:::row-end:::

:::row:::
    :::column:::
![Abbildung eines Kontextmenüs](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>Menüs und Kontextmenüs</b>

<p>Menüs und Kontextmenüs sparen Platz, indem Befehle angeordnet und ausgeblendet werden, bis der Benutzer sie benötigt. Benutzer greifen in der Regel auf ein Menü oder Kontextmenü zu, indem Sie auf eine Schaltfläche klicken oder mit der rechten Maustaste auf ein Steuerelement klicken.</p> 

<p>Das <a href="../controls-and-patterns/command-bar-flyout.md">Flyout der Befehlsleiste</a> ist ein Kontextmenü, in dem die Vorteile einer Befehlsleiste und eines Kontextmenüs zu einem einzelnen Steuerelement kombiniert werden. Es eignet sich für Verknüpfungen mit häufig verwendeten Aktionen und ermöglicht den Zugriff auf sekundäre Befehle, die nur in bestimmten Kontexten relevant sind (z.B. Zwischenablage oder benutzerdefinierte Befehle).</p>

<p>Die UWP bietet auch eine Reihe herkömmlicher Menüs und Kontextmenüs. Weitere Informationen finden Sie in der <a href="../controls-and-patterns/menus.md">Übersicht über Menüs und Kontextmenüs</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Feedback zu Befehlen 

Durch Feedback zu Befehlen wird Benutzern mitgeteilt, dass eine Interaktion oder ein Befehl erkannt wurde, wie der Befehl interpretiert und verarbeitet wurde und ob der Befehl erfolgreich abgeschlossen wurde. Dadurch können Benutzer verstehen, welche Aktion sie ausgeführt haben und welche sie als Nächstes ausführen können. Im Idealfall sollte das Feedback natürlich in Ihre Benutzeroberfläche integriert werden, so dass Benutzer nicht unterbrochen werden oder zusätzliche Maßnahmen ergreifen müssen, es sei denn, dies ist absolut notwendig.

> [!NOTE]
> Geben Sie nur dann Feedback, wenn dies notwendig ist und kein Feedback an anderer Stelle zur Verfügung steht. Halten Sie die Benutzeroberfläche der Anwendung einfach und übersichtlich, es sei denn, sie fügen hilfreiche Funktionen hinzu.

Hier sind einige Möglichkeiten, um Feedback in Ihrer App bereitzustellen.

:::row:::
    :::column:::
![Abbildung des Inhaltsbereichs einer Befehlsleiste](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>Befehlsleiste</b>

Der Inhaltsbereich der <a href="../controls-and-patterns/app-bars.md">Befehlsleiste</a> ist ein intuitiver Ort, um Benutzern einen Status mitzuteilen, wenn sie Feedback erhalten möchten.
:::row-end:::

:::row:::
    :::column:::
![Abbildung eines Flyouts](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Flyouts</a> sind einfache, kontextbezogene Popups, die durch Antippen oder Klicken außerhalb des Flyouts verworfen werden können.
:::row-end:::

:::row:::
    :::column:::
![Abbildung eines Dialogs](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>Dialogsteuerelemente</b>

<a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialogsteuerelemente</a> sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Die meisten Dialogfelder blockieren die Interaktion mit dem App-Fenster, bis sie explizit geschlossen werden, und erfordern häufig eine Aktion des Benutzers. Dialogfelder können als störend empfunden werden und sollten daher nur in bestimmten Situationen zum Einsatz kommen. Weitere Informationen finden Sie unter [Bestätigen oder Rückgängigmachen von Aktionen](#when-to-confirm-or-undo-actions)
    :::column-end:::
:::row-end:::

> [!TIP]
> Verwenden Sie nicht zu viele Bestätigungsdialogfelder. Diese können zwar sehr hilfreich sein, wenn dem Benutzer ein Fehler unterläuft, bei bewusst durchgeführten Aktionen sind sie jedoch eher hinderlich.

### <a name="when-to-confirm-or-undo-actions"></a>Bestätigen oder Rückgängigmachen von Aktionen

Unabhängig davon, wie durchdacht das Design der UI Ihrer App ist – jeder Benutzer kann einmal eine unabsichtlich eine unerwünschte Aktion ausführen. In einer solchen Situation kann Ihre App hilfreich sein, indem sie eine Bestätigung der Aktion fordert oder eine Möglichkeit zum Rückgängigmachen der kürzlich durchgeführten Aktionen anbietet.

:::row:::
    :::column:::
![Abbildung von „Richtig“](images/do.svg)

Für Aktionen, die nicht rückgängig gemacht werden können und schwerwiegende Auswirkungen haben, empfehlen wir die Verwendung eines Bestätigungsdialogfelds. Beispiele für solche Aktionen:
-   Überschreiben einer Datei
-   Schließen einer Datei ohne Speichern
-   Bestätigen der endgültigen Löschung einer Datei oder von Daten
-   Einkaufen (es sei denn, der Benutzer lehnt eine Bestätigungsanforderung ab)
-   Senden eines Formulars, z. B. bei Registrierungen
    :::column-end:::
    :::column:::
![Abbildung von „Richtig“](images/do.svg)

Für Aktionen, die rückgängig gemacht werden können, ist ein einfacher „Rückgängig“-Befehl in der Regel ausreichend. Beispiele für solche Aktionen:
-   Löschen einer Datei
-   Löschen einer E-Mail (nicht endgültig)
-   Ändern des Inhalts oder Bearbeiten von Text
-   Umbenennen einer Datei
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Optimieren für bestimmte Eingabearten

Ausführliche Informationen zum Optimieren der Benutzerfreundlichkeit bei einem bestimmten Eingabetyp oder -gerät finden Sie unter [Einführung in die Interaktion](../input/index.md).

