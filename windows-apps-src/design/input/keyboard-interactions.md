---
Description: Erfahren Sie, wie Sie Ihre UWP-Apps entwerfen und optimieren, sodass sie die bestmögliche Umgebung für erfahrene Tastaturbenutzer und Personen mit Einschränkungen und anderen Anforderungen an Barrierefreiheit bieten.
title: Tastaturinteraktionen
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: Tastatur, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen, Gamepad, Fernbedienung
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 449f0c81bdd54d99ef0977ca1c9b6ba10ba5eae7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258356"
---
# <a name="keyboard-interactions"></a>Tastaturinteraktionen

![Tastaturfavoritenbild](images/keyboard/keyboard-hero.jpg)

Erfahren Sie, wie Sie Ihre UWP-Apps entwerfen und optimieren, sodass sie die bestmögliche Umgebung für erfahrene Tastaturbenutzer und Personen mit Einschränkungen und anderen Anforderungen an Barrierefreiheit bieten.

Auf verschiedenen Geräten ist die Tastatureingabe ein wichtiger Teil der Gesamtinteraktion mit der Universellen Windows-Plattform (UWP). Eine gut durchdachte Tastatur ermöglicht Benutzern die effiziente UI-Navigation in Ihrer App und Zugriff auf die gesamte Funktionalität, ohne die Hände von der Tastatur zu nehmen.

![Bild von Tastatur und Gamepad](images/keyboard/keyboard-gamepad.jpg)

***Common interaction patterns are shared between keyboard and gamepad***

In diesem Thema konzentrieren wir uns speziell auf das UWP-App-Design für die Tastatureingabe auf PCs. However, a well-designed keyboard experience is important for supporting accessibility tools such as Windows Narrator, using [software keyboards](#software-keyboard) such as the touch keyboard and the On-Screen Keyboard (OSK), and for handling other input device types, such as the Xbox gamepad and remote control.

Viele der hier erwähnten Richtlinien und Empfehlungen, einschließlich [visueller Fokuselemente](#focus-visuals), [Zugriffstasten](#access-keys), und [UI-Navigation](#navigation), gelten auch für diese anderen Szenarien.

**HINWEIS** Während Hardware- und Softwaretastaturen für die Texteingabe verwendet werden, liegt der Fokus dieses Themas auf Navigation und Interaktion.

## <a name="built-in-support"></a>Integrierte Unterstützung

Zusammen mit der Maus ist die Tastatur das am häufigsten verwendete Peripheriegerät für PCs und daher ein wesentlicher Bestandteil des PC-Erlebnisses. PC-Benutzer erwarten ein umfassendes und konsistentes Erlebnis bei Tastatureingaben in System-Apps und individuellen Apps.

Alle UWP-Steuerelemente enthalten integrierte Unterstützung für umfangreiche Tastaturfunktionen und Benutzerinteraktionen, während die Plattform selbst eine umfassende Grundlage für das Erstellen von Tastaturfunktionen bietet, die Ihrer Meinung nach am besten für Ihre benutzerdefinierten Steuerelemente und Apps geeignet sind.

![Bild von Tastatur mit Telefon](images/keyboard/keyboard-phone.jpg)

***UWP supports keyboard with any device***

## <a name="basic-experiences"></a>Grundlegende Funktionen
![Fokusbasierte Geräte](images/keyboard/focus-based-devices.jpg)

Wie bereits erwähnt, ist die Tastatureingabe für Navigation und Befehle von Eingabegeräten wie Xbox-Gamepad und -Fernbedienung und Eingabehilfen wie der Sprachausgabe zum Großteil gleich. Die Einheitlichkeit in Bezug auf Eingabetypen und Tools minimiert zusätzliche Aufgaben und trägt zur Erreichung des Ziels der Universellen Windows-Plattform bei: einmal erstellen, überall ausführen.

Where necessary, we'll identify key differences you should be aware of and describe any mitigations you should consider.

Hier sind die Geräte und die Tools, die in diesem Thema erläutert werden:

| Gerät/Tool                       | Beschreibung     |
|-----------------------------------|-----------------|
|Tastatur (Hardware und Software)   |In addition to the standard hardware keyboard, UWP applications support two software keyboards: the [touch (or software) keyboard](#software-keyboard) and the [On-Screen Keyboard](#on-screen-keyboard).|
|Gamepad und Fernbedienung         |Xbox-Gamepad und -Fernbedienung sind grundlegende Eingabegeräte im [10-Fuß-Bereich](../devices/designing-for-tv.md). Ausführliche Informationen zur UWP-Unterstützung für Gamepad und Fernbedienung finden Sie unter [Interaktionen mit Gamepad und Fernbedienung](gamepad-and-remote-interactions.md).|
|Bildschirmleseprogramme (Sprachausgabe)          |Die Sprachausgabe ist ein integriertes Bildschirmleseprogramm für Windows, das eindeutige Interaktion und Funktionalität bietet, aber dennoch auf der einfachen Tastaturnavigation und -eingabe basiert. Weitere Informationen zur Sprachausgabe finden Sie unter [Erste Schritte mit Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Benutzerdefinierte Erfahrungen und effiziente Tastaturfunktionen
Wie bereits erwähnt, ist Die Tastaturunterstützung ist ein wesentlicher Bestandteil dafür, dass Ihre Anwendungen optimal für Benutzer mit unterschiedlichen Kenntnissen, Fähigkeiten und Erwartungen funktionieren. Wir empfehlen Ihnen, folgendes zu priorisieren.
- Unterstützung der Tastaturnavigation und Interaktion
    - Stellen Sie sicher, dass ausführbare Elemente als Tabstopps gekennzeichnet sind (und nicht bedienbare Elemente andersartig gekennzeichnet sind), und die Reihenfolge für die Seitennavigation logisch und vorhersehbar ist (weiter Informationen finden Sie unter [Tabstopps](#tab-stops))
    - Legen Sie den anfänglichen Fokus auf das logischste Element fest (weitere Informationen finden Sie unter [Anfänglicher Fokus](#initial-focus))
    - Bieten Sie eine Navigation mittels Pfeiltasten für "innere Navigationsvorgänge" an (siehe [Navigation](#navigation))
- Unterstützen Sie Tastenkombinationen
    - Stellen Sie Zugriffstasten für schnelle Aktionen bereit (siehe [Schnellinfos](#accelerators))
    - Provide access keys to navigate your application's UI (see [Access keys](access-keys.md))

### <a name="focus-visuals"></a>Focus visuals

Die UWP unterstützt ein einzelnes Design für visuelle Fokuselemente, das gut für alle Eingabearten und -funktionen funktioniert.
![Focus visual](images/keyboard/focus-visual.png)

Ein visuelles Fokuselement:

- wird angezeigt, wenn ein UI-Element über eine Tastatur und/oder ein Gamepad/eine Fernbedienung den Fokus erhält
- wird als hervorgehobener Rahmen um das UI-Element wiedergegeben, um anzugeben, dass eine Aktion ausgeführt werden kann
- unterstützt einen Benutzer dabei, in einer App-UI zu navigieren, ohne die Orientierung zu verlieren
- kann für Ihre App angepasst werden (siehe [Visuelle Fokuselemente mit hoher Sichtbarkeit](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**HINWEIS** Das visuelle UWP-Fokuselement entspricht nicht dem Fokusrechteck der Sprachausgabe.

### <a name="tab-stops"></a>Tabstopps

Damit ein Steuerelement (einschließlich der Navigationselemente) über die Tastatur verwendet werden kann, muss auf dem Steuerelement der Fokus liegen. One way for a control to receive keyboard focus is to make it accessible through tab navigation by identifying it as a tab stop in your application's tab order.

For a control to be included in the tab order, the [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) property must be set to **true** and the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property must be set to **true**.

To specifically exclude a control from the tab order, set the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property to **false**.

Standardmäßig entspricht die Aktivierreihenfolge der Reihenfolge, in der UI-Elemente erstellt werden. Wenn beispielsweise ein `StackPanel` die Elemente `Button`, `Checkbox` und `TextBox` enthält, ist die Aktivierreihenfolge `Button`, `Checkbox` und`TextBox`.

You can override the default tab order by setting the [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) property.

#### <a name="tab-order-should-be-logical-and-predictable"></a>Aktivierreihenfolge sollte logisch und vorhersehbar sein

Ein gut durchdachtes Tastatur-Navigationsmodell, das eine logische und vorhersehbare Aktivierreihenfolge verwendet, macht Ihre App intuitiver und unterstützt Benutzer dabei, Funktionen effizienter und effektiver zu erkunden, zu identifizieren und auf sie zuzugreifen.

All interactive controls should have tab stops (unless they are in a [group](#control-group)), while non-interactive controls, such as labels, should not.

Vermeiden Sie eine benutzerdefinierte Aktivierreihenfolge, durch die der Fokus in Ihrer Anwendung wechselt. Beispielsweise sollte eine Liste mit Steuerelementen in einem Formular eine Aktivierreihenfolge aufweisen, die von oben nach unten und von links nach rechts (je nach Gebietsschema) verläuft.

See [Keyboard accessibility](../accessibility/keyboard-accessibility.md) for more details about customizing tab stops.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Koordinieren von Aktivierreihenfolge und visueller Reihenfolge

Coordinating tab order and visual order (also referred to as reading order or display order) helps reduce confusion for users as they navigate through your application's UI.

Versuchen Sie, die wichtigsten Befehle, Steuerelemente und Inhalte zuerst in der Aktivierreihenfolge und der visuellen Reihenfolge einzustufen und darzustellen. Die tatsächliche Anzeigeposition kann jedoch vom übergeordneten Layoutcontainer und bestimmten Eigenschaften der untergeordneten Elemente abhängen, die das Layout beeinflussen. Insbesondere kann sich die visuelle Reihenfolge von Layouts mit einer Raster- oder Tabellenmetapher erheblich von der Aktivierreihenfolge unterscheiden.

**HINWEIS** Die visuelle Reihenfolge ist auch von Gebietsschema und Sprache abhängig.

### <a name="initial-focus"></a>Anfänglicher Fokus

Der anfängliche Fokus gibt das UI-Element an, das den Fokus erhält, wenn eine Anwendung oder eine Seite zum ersten Mal gestartet oder aktiviert wird. When using a keyboard, it is from this element that a user starts interacting with your application's UI.

For UWP apps, initial focus is set to the element with the highest [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) that can receive focus. Untergeordnete Elemente von Container-Steuerelementen werden ignoriert. Bei einer Verknüpfung erhält das erste Element in der visuellen Struktur den Fokus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Festlegen des anfänglichen Fokus auf das logischste Element

Legen Sie den anfänglichen Fokus auf das UI-Element für die erste, oder primäre, Aktion, die Benutzer mit der größten Wahrscheinlichkeit durchführen, wenn Sie Ihre App starten oder zu einer Seite navigieren. Beispiele:
-   Eine Foto-App, wobei der Fokus auf dem ersten Element in einer Galerie liegt
-   Eine Musik-App, wobei der Fokus auf der Wiedergabe-Taste liegt

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Don't set initial focus on an element that exposes a potentially negative, or even disastrous, outcome

This level of functionality should be a user's choice. Das Festlegen des anfänglichen Fokus auf ein Element mit einem signifikanten Ergebnis resultiert möglicherweise in unerwünschtem Datenverlust oder Systemzugriff. For example, don't set focus to the delete button when navigating to an e-mail.

See [Focus navigation](focus-navigation.md) for more details about overriding tab order.

### <a name="navigation"></a>Navigation

Die Tastaturnavigation wird in der Regel durch die TAB-Tasten und die Pfeiltasten unterstützt.

![TAB-Tasten und Pfeiltasten](images/keyboard/tab-and-arrow.png)

Standardmäßig weisen UWP-Steuerelemente dieses grundlegende Tastaturverhalten auf:
-   **TAB-Tasten** dienen der Navigation zwischen ausführbaren/aktiven Steuerelementen in der Aktivierreihenfolge.
-   **UMSCHALT + TAB** dient der Navigation zwischen Steuerelementen in umgekehrter Aktivierreihenfolge. Wenn der Benutzer mit der Pfeiltaste in das Steuerelement navigiert ist, liegt der Fokus auf dem letzten bekannten Wert im Steuerelement.
-   **Pfeiltasten** ermöglichen die steuerelementspezifische „interne Navigation“, wenn der Benutzer zur „internen Navigation“ wechselt. Pfeiltasten dienen nicht der Navigation außerhalb des Steuerelements. Beispiele:
    -   Up/Down arrow key moves focus inside `ListView` and `MenuFlyout`
    -   Modify currently selected values for `Slider` and `RatingsControl`
    -   Move caret inside `TextBox`
    -   Expand/collapse items inside `TreeView`

Use these default behaviors to optimize your application's keyboard navigation.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Verwenden Sie die „interne Navigation“ mit Gruppen verwandter Steuerelemente.

Providing arrow key navigation into a set of related controls reinforces their relationship within the overall organization of your application's UI.

Das hier gezeigte Steuerelement `ContentDialog` ermöglicht beispielsweise standardmäßig die interne Navigation für eine horizontale Reihe von Tasten. (Informationen zu benutzerdefinierten Steuerelementen finden Sie im Abschnitt [Steuerelementgruppe](#control-group)).

![Dialogfeldbeispiel](images/keyboard/dialog.png)

***Interaction with a collection of related buttons is made easier with arrow key navigation***

Wenn Elemente in einer einzelnen Spalte angezeigt werden, erfolgt die Elementnavigation mit der Nach-oben-/Nach-unten-Pfeiltaste. Wenn Elemente in einer einzelnen Zeile angezeigt werden, erfolgt die Elementnavigation mit der Rechts-/Links-Pfeiltaste. Wenn sich die Elemente in mehreren Spalten befinden, erfolgt die Navigation über alle 4 Pfeiltasten.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Define a single tab stop for a collection of related controls

By defining a single tab stop for a collection of related, or complementary, controls, you can minimize the number of overall tab stops in your app.

Die folgenden Abbildungen zeigen beispielsweise zwei gestapelte `ListView`-Steuerelemente. Die Abbildung auf der linken Seite zeigt die Pfeiltastennavigation zwischen `ListView`-Steuerelementen für einen Tabstopp. Die Abbildung auf der rechten Seite zeigt, wie die Navigation zwischen untergeordneten Elementen einfacher und effizienter gestaltet werden kann, da die Navigation in übergeordneten Steuerelementen nicht mit einer TAB-Taste erfolgt.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaction with two stacked ListView controls can be made easier and more efficient by eliminating the tab stop and navigating with just arrow keys.***

Im Abschnitt [Steuerelementgruppe](#control-group) erfahren Sie, wie Sie die Optimierungsbeispiele auf Ihre Anwendungs-UI anwenden.

### <a name="interaction-and-commanding"></a>Interaktion und Befehle

Wenn ein Steuerelement den Fokus hat, kann ein Benutzer mit ihm interagieren und zugehörige Funktionalität über spezielle Tastatureingaben aufrufen.

#### <a name="text-entry"></a>Texteingabe

Für Steuerelemente, die speziell der Texteingabe dienen (wie `TextBox` und `RichEditBox`), werden alle Tastatureingaben zur Eingabe von Text oder Navigation in Text verwendet, was Priorität gegenüber anderen Tastaturbefehlen hat. Beispielsweise erkennt das Dropdownmenü für ein `AutoSuggestBox`-Steuerelement nicht die **Leertaste** als Auswahlbefehl.

![Texteingabe](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Leertaste

Wenn Sie sich nicht im Texteingabemodus befinden, ruft die **Leertaste** die Aktion oder den Befehl auf, der mit dem fokussierten Steuerelement verknüpft ist (vergleichbar mit dem Tippen oder dem Klicken mit einer Maus).

![LEERTASTE](images/keyboard/space-key.png)

#### <a name="enter-key"></a>EINGABETASTE

Die **EINGABETASTE** ermöglicht eine Vielzahl an gängigen Benutzerinteraktionen, abhängig vom fokussierten Steuerelement:
-   Aktiviert Befehlssteuerelemente wie `Button` oder `Hyperlink`. Für mehr Benutzerfreundlichkeit aktiviert die **EINGABETASTE** auch Steuerelemente, die Befehlssteuerelementen ähneln, wie `ToggleButton` oder `AppBarToggleButton`.
-   Zeigt die Auswahl-UI für Steuerelemente an, z. B. `ComboBox` und `DatePicker`. Die **EINGABETASTE** bestätigt und schließt auch die Auswahl-UI.
-   Aktiviert Listensteuerelemente wie `ListView`, `GridView` und `ComboBox`.
    -   Die **EINGABETASTE** führt die Auswahlaktion als **LEERTASTE** für Listen- und Rasterelemente durch, es sei denn, es ist eine zusätzliche Aktion für diese Elemente vorhanden (wodurch ein neues Fenster geöffnet wird).
    -   Wenn eine weitere Aktion mit dem Steuerelement verknüpft ist, führt die **EINGABETASTE** die zusätzliche Aktion und die **LEERTASTE** die Auswahlaktion durch.

**HINWEIS** Die **EINGABETASTE** und die **LEERTASTE** führen nicht immer dieselbe Aktion aus, aber oft.

![EINGABETASTE](images/keyboard/enter-key.png)

Die ESC-TASTE ermöglicht Benutzern das Abbrechen einer Übergangs-UI (sowie aller laufenden Aktionen in dieser UI).

Beispiele umfassen:
-   Benutzer öffnet eine `ComboBox` mit einem ausgewählten Wert und verwendet die Pfeiltasten zum Verschieben der Fokusauswahl auf einen neuen Wert. Durch Drücken der ESC-TASTE wird die `ComboBox` geschlossen und der ausgewählte Wert auf den ursprünglichen Wert zurückgesetzt.
-   Der Benutzer ruft eine Aktion zum dauerhaften Löschen für eine E-Mail auf und wird mit einem `ContentDialog` aufgefordert, die Aktion zu bestätigen. Der Benutzer entscheidet, dass dies nicht die gewünschte Aktion ist, und drückt die **ESC-TASTE**, um das Dialogfeld zu schließen. Da die **ESC-TASTE** mit der Schaltfläche **Abbrechen** verknüpft ist, wird das Dialogfeld geschlossen und die Aktion abgebrochen. Die **ESC-TASTE** wirkt sich nur auf die Übergangs-UI aus. Die App-UI wird nicht geschlossen, und es wird auch nicht durch sie zurücknavigiert.

![ESC-TASTE](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>POS1-TASTE und ENDE-TASTE

Die Tasten **POS1** und **ENDE** ermöglichen Benutzern, einen Bildlauf zum Anfang oder Ende eines UI-Bereichs durchzuführen.

Beispiele umfassen:
-   Für die Steuerelemente `ListView` und `GridView` verlagert die Taste **POS1** den Fokus auf das erste Element und führt einen Bildlauf in die Ansicht durch. Die Taste **ENDE** verlagert den Fokus auf das letzte Element und führt einen Bildlauf in die Ansicht durch.
-   Für das Steuerelement `ScrollView` führt die Taste **POS1** einen Bildlauf zum Anfang des Bereichs durch. Die Taste **ENDE** führt einen Bildlauf zum Ende des Bereichs durch (Fokus wird nicht geändert).

![POS1-TASTE und ENDE-TASTE](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>BILD-AUF-TASTE und BILD-AB-TASTE

Die **BILD-TASTEN** ermöglichen Benutzern das Durchführen eines Bildlaufs in einem UI-Bereich in einzelnen Inkrementen.

Für die Steuerelemente `ListView` und `GridView` führt die **BILD-AUF-TASTE** einen Bildlauf nach oben im Bereich um ein „Bild“ (in der Regel die Viewporthöhe) durch und verlagert den Fokus nach oben im Bereich. Alternativ führt die **BILD-AB-TASTE** einen Bildlauf nach unten im Bereich um ein Bild durch und verlagert den Fokus nach unten im Bereich.

![BILD-AUF-TASTE und BILD-AB-TASTE](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Tastenkombinationen

Keyboard shortcuts can make your app easier to use by providing both enhanced support for accessibility and improved efficiency for keyboard users.

In addition to supporting keyboard navigation and activation in your app, it is also good practice to provide shortcuts for your application's functionality. Tab navigation provides a good, basic level of keyboard support, but with more complex UI you might want to add support for shortcut keys as well. 

Tastenkombinationen sind eine effiziente Methode für den Zugriff auf App-Funktionen und verbessern daher die Produktivität der Benutzer. Es gibt zwei Arten von Tastenkombinationen:
-   [Accelerators](#accelerators) are shortcuts that invoke an app command. Your app may or may not provide specific UI that corresponds to the command. Accelerators typically consist of the Ctrl key plus a letter key.
-   [Access keys](#access-keys) are shortcuts that set focus to specific UI in your application. Access keys typicaly consist of the Alt key plus a letter key.

Providing consistent keyboard shortcuts that support similar tasks across applications makes them much more useful and powerful and helps users remember them.

#### <a name="accelerators"></a>Schnellinfos

Accelerators help users perform common actions in an application much more quickly and efficiently. 

Beispiele für Tastenkombinationen mit der STRG-Taste:
-   Pressing Ctrl + N key anywhere in the **Mail** app launches a new mail item.
-   Pressing Ctrl + E key anywhere in Microsoft Edge (and many Microsoft Store applications) launches search.

Tastenkombinationen mit der STRG-Taste weisen die folgenden Merkmale auf:
-   They primarily use Ctrl and Function key sequences (Windows system shortcut keys also use Alt + non-alphanumeric keys and the Windows logo key).
-   Sie werden nur den am häufigsten verwendeten Befehlen zugewiesen.
-   Ihre Speicherung ist vorgesehen, und sie werden nur in Menüs, QuickInfos und in der Hilfe dokumentiert.
-   They have effect throughout the entire application, when supported.
-   They should be assigned consistently as they are memorized and not directly documented.

#### <a name="access-keys"></a>Zugriffstasten

Auf der Seite [Tastenkombinationen mit der STRG-Taste](access-keys.md) finden Sie umfassendere Informationen zur Unterstützung von Tastenkombinationen mit der STRG-Taste mit UWP.

Tastenkombinationen mit der STRG-Taste ermöglichen es Benutzern mit motorischen Einschränkungen, jeweils eine Taste zu drücken, um eine Aktion für ein bestimmtes Element in der UI durchzuführen. Darüber hinaus können Tastenkombinationen mit der STRG-Taste verwendet werden, um zusätzliche Tastenkombinationen zu kommunizieren, damit erfahrene Benutzer schnell Aktionen durchführen können.

Tastenkombinationen mit der STRG-Taste weisen die folgenden Merkmale auf:
-   Sie verwenden ALT und eine alphanumerische Taste.
-   Sie dienen in erster Linie der Barrierefreiheit.
-   They are documented directly in the UI, adjacent to the control, through [Key Tips](access-keys.md).
-   Sie wirken sich nur auf das aktuelle Fenster aus und navigieren zum entsprechenden Menü- oder Steuerelement.
-   Access keys should be assigned consistently to commonly used commands (especially commit buttons), whenever possible.
-   Sie sind lokalisiert.

#### <a name="common-keyboard-shortcuts"></a>Häufig verwendete Tastenkombinationen

The following table is a small sample of frequently used keyboard shortcuts. 

| Aktion                               | Tastenkombination                                      |
|--------------------------------------|--------------------------------------------------|
| Alle auswählen                           | STRG+A                                           |
| Fortlaufend auswählen                  | UMSCHALT+Pfeiltaste                                  |
| Speichern                                 | STRG+S                                           |
| Suchen Sie                                 | STRG+F                                           |
| Drucken                                | STRG+P                                           |
| „Kopieren“                                 | STRG+C                                           |
| Ausschneiden                                  | STRG+X                                           |
| Einfügen                                | STRG+V                                           |
| Rückgängig machen                                 | STRG+Z                                           |
| Nächste Registerkarte                             | STRG+TAB                                         |
| Registerkarte schließen                            | STRG+F4 oder STRG+W                                |
| Semantischer Zoom                        | STRG+PLUS-TASTE oder STRG+MINUS-TASTE                                 |

For a comprehensive list of Windows system shortcuts, see [keyboard shortcuts for Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). For common application shortcuts, see [keyboard shortcuts for Microsoft applications](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Erweiterte Funktionen

In diesem Abschnitt erläutern wir einige der komplexeren Tastaturinteraktionen, die durch UWP-Apps unterstützt werden, sowie einige der Verhaltensweisen, die Sie bei Verwendung Ihrer App auf verschiedenen Geräten und mit verschiedenen Tools berücksichtigen sollten.

### <a name="control-group"></a>Control group

Sie können eine Gruppe verwandter oder komplementärer Steuerelemente in einer „Steuerelementgruppe“ (oder einem direktionalen Bereich) gruppieren, was die „interne Navigation“ mit den Pfeiltasten aktiviert. Die Steuerelementgruppe kann ein einzelner Tabstopp sein, oder Sie können mehrere Tabstopps innerhalb der Steuerelementgruppe angeben.

#### <a name="arrow-key-navigation"></a>Pfeiltastennavigation

Benutzer erwarten Unterstützung für die Pfeiltastennavigation, wenn eine Gruppe ähnlicher, verwandter Steuerelemente in einem UI-Bereich vorhanden ist:
-   `AppBarButtons` in a `CommandBar`
-   `ListItems` or `GridItems` inside `ListView` or `GridView`
-   `Buttons` inside `ContentDialog`

UWP-Steuerelemente unterstützen die Pfeiltastennavigation standardmäßig. Verwenden Sie für benutzerdefinierte Layouts und Steuerelementgruppen `XYFocusKeyboardNavigation="Enabled"`, um ein ähnliches Verhalten zu erzeugen.

Consider adding support for arrow key navigation when using the following controls:

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Dialog buttons</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems and GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Tabstopps

Depending on your application's functionality and layout, the best navigation option for a control group might be a single tab stop with arrow navigation to child elements, multiple tab stops, or some combination.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Verwenden mehrerer Tabstopps und Pfeiltasten für Schaltflächen

Benutzer, für die Barrierefreiheit wichtig ist, benötigen gut etablierte Tastaturnavigationsregeln, bei denen nicht typischerweise Pfeiltasten zum Navigieren in verschiedenen Schaltflächen verwendet werden. Allerdings empfinden Benutzer ohne Sehschwäche das Verhalten evtl. als natürlich.

Ein Beispiel des UWP-Standardverhaltens ist in diesem Fall das `ContentDialog`. Während Pfeiltasten zum Navigieren zwischen Schaltflächen verwendet werden können, ist jede Schaltfläche auch ein Tabstopp.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Zuweisen einzelner Tabstopps zu vertrauten UI-Mustern

In Fällen, in denen das Layout einem bekannten UI-Muster für Steuerelementgruppen folgt, kann das Zuweisen eines einzelnen Tabstopps zur Gruppe die Navigationseffizienz für Benutzer verbessern.

Dazu gehören:
-   `RadioButtons`
-   Multiple `ListViews` that look like and behave like a single `ListView`
-   Jede UI, die wie ein Kachelraster aussehen und sich entsprechend verhalten soll (z. B. Kacheln im Startmenü)

#### <a name="specifying-control-group-behavior"></a>Angeben von Steuerelementgruppenverhalten

Verwenden Sie die folgenden APIs, um benutzerdefiniertes Steuerelementgruppenverhalten zu unterstützen (alle werden später in diesem Thema ausführlich erläutert):

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) ermöglicht die Pfeiltastennavigation zwischen Steuerelementen
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) gibt an, ob mehrere Tabstopps vorhanden sind oder ein einzelner Tabstopp vorhanden ist
-   [FindFirstFocusableElement und FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) legen den Fokus auf das erste Element mit der Taste **POS1** und das letzte Element mit der Taste **ENDE**

Die folgende Abbildung zeigt ein intuitives Tastaturnavigationsverhalten für eine Steuerelementgruppe mit zugeordneten Optionsfeldern. In diesem Fall wird Folgendes empfohlen: ein einzelner Tabstopp für die Steuerelementgruppe, interne Navigation zwischen den Optionsfeldern mit den Pfeiltasten, Taste **POS1** verknüpft mit dem ersten Optionsfeld und Taste **ENDE** verknüpft mit dem letzten Optionsfeld.

![Kombination](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Tastatur und Sprachausgabe

Die Sprachausgabe ist eine UI-Eingabehilfe für Tastaturbenutzer. (Weitere Eingabetypen werden ebenfalls unterstützt.) Allerdings geht die Sprachausgabefunktionalität über die Tastaturinteraktionen hinaus, die von den UWP-Apps unterstützt werden, und beim Entwerfen der UWP-App für die Sprachausgabe ist besondere Sorgfalt erforderlich. (Die [Seite mit den Grundlagen der Sprachausgabe](https://support.microsoft.com/help/22808/windows-10-narrator-basics) leitet Sie durch die Sprachausgabe.)

Einige der Unterschiede zwischen dem UWP-Tastaturverhalten und der Sprachausgabe umfassen:
-   Zusätzliche Tastenkombinationen für die Navigation zu UI-Elementen, die nicht über die Standardtastaturnavigation verfügbar sind, z. B. FESTSTELLTASTE + Pfeiltasten zum Lesen von Steuerelementbeschriftungen.
-   Navigation zu deaktivierten Elementen. Standardmäßig werden deaktivierte Elemente nicht über die Standardtastaturnavigation verfügbar gemacht.
-   Steuerelementansichten für schnellere Navigation basierend auf UI-Granularität. Benutzer können zu Elementen, Zeichen, Wörtern, Zeilen, Absätzen, Links, Überschriften, Tabellen, Landmarks und Vorschlägen navigieren. Die Standardtastaturnavigation macht diese Objekte als einfache Liste verfügbar, was die Navigation unter Umständen verkompliziert, es sein denn, Sie stellen Tastenkombinationen bereit.

#### <a name="case-study--autosuggestbox-control"></a>Fallstudie – AutoSuggestBox-Steuerelement

Die Suchschaltfläche für `AutoSuggestBox` ist nicht bei der standardmäßigen Tastaturnavigation mit der TAB-Taste und den Pfeiltasten zugänglich, da der Benutzer die **EINGABETASTE** drücken kann, um die Suchabfrage zu senden. Sie ist jedoch über die Sprachausgabe zugänglich, wenn der Benutzer FESTSTELLTASTE + eine Pfeiltaste drückt.

![Automatische Vorschläge für Tastaturfokus](images/keyboard/auto-suggest-keyboard.png)

*With keyboard, users press the* ***Enter*** *key to submit search query*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>With Narrator, users press the <strong>Enter</strong> key to submit search query</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>With Narrator, users are also able to access the search button using the <strong>Caps Lock + Right arrow key</strong>, then pressing <strong>Space</strong> key</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Tastatur und Xbox-Gamepad und -Fernbedienung

Xbox-Gamepads und -Fernbedienungen unterstützen das Verhalten und die Funktionen der UWP-Tastatur größtenteils. Da jedoch verschiedene Tastenoptionen einer Tastatur fehlen, stehen bei Gamepad und Fernbedienung viele Tastaturoptimierungen nicht zur Verfügung (Fernbedienung noch eingeschränkter als Gamepad).

See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on UWP support for gamepad and remote control input.

Im Folgenden finden Sie einige wichtige Zuordnungen zwischen Tastatur, Gamepad und Fernbedienung.

| **Tastatur**  | **Gamepad**                         | **Remote control**  |
|---------------|-------------------------------------|---------------------|
| LEERTASTE         | A-TASTE                            | Auswahltaste       |
| Eingabetaste         | A-TASTE                            | Auswahltaste       |
| ESCAPE-TASTE        | B-TASTE                            | Zurück-Schaltfläche         |
| POS1/ENDE      | n. v.                                 | n. v.                 |
| Bild AUF/BILD AB  | Auslösertaste für vertikalen Bildlauf, Bumper-Taste für horizontalen Bildlauf   | n. v.                 |

Einige der wichtigsten Unterschiede, die Sie beim Entwerfen Ihrer UWP-App für die Verwendung mit Gamepad und Fernbedienung beachten sollten, sind:
-   Die Texteingabe erfordert, dass der Benutzer A drückt, um ein Textsteuerelement zu aktivieren.
-   Die Fokusnavigation ist nicht auf Steuerelementgruppen beschränkt, Benutzer können frei zu beliebigen fokussierbaren UI-Elementen in der App navigieren.

    **HINWEIS** Der Fokus kann auf jedes beliebige fokussierbare UI-Element in Tastendruckrichtung verlagert werden, es sei denn, es handelt sich um eine Overlay-UI, oder [Fokusaktivierung](gamepad-and-remote-interactions.md#focus-engagement) ist angegeben. Dies verhindert, dass der Fokus in einen/aus einem Bereich wechselt, bis er durch die A-Taste belegt/nicht belegt ist. Weitere Informationen finden Sie im Abschnitt zur [direktionalen Navigation](#directional-navigation).
-   Das Steuerkreuz und der linke Stick werden zum Verlagern des Fokus zwischen Steuerelementen und für die interne Navigation verwendet.

    **HINWEIS** Gamepad und Fernbedienung navigieren nur zu Elementen in derselben visuellen Reihenfolge wie die gedrückte Richtungstaste. Die Navigation in diese Richtung wird deaktiviert, wenn es kein nachfolgendes Element gibt, das den Fokus erhalten kann. Abhängig von der Situation gilt diese Einschränkung für Tastaturbenutzer nicht immer. Im Abschnitt [Integrierte Tastaturoptimierung](#built-in-keyboard-optimization) erhalten Sie weitere Infos.

#### <a name="directional-navigation"></a>Directional navigation

Die direktionale Navigation wird über die UWP-Hilfsklasse [Focus Manager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) verwaltet, die die gedrückte Richtungstaste (Pfeiltaste, Steuerkreuz) verwendet und versucht, den Fokus in die entsprechende visuelle Richtung zu verlagern.

Unlike the keyboard, when an app opts out of [Mouse Mode](gamepad-and-remote-interactions.md#mouse-mode), directional navigation is applied across the entire application for gamepad and remote control. See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on directional navigation optimization.

**HINWEIS** Die Navigation mithilfe der TAB-TASTE der Tastatur wird nicht als direktionale Navigation betrachtet. Weitere Informationen finden Sie im Abschnitt [Tabstopps](#tab-stops).

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Directional navigation supported</strong></br>Using directional keys (keyboard arrows, gamepad and remote control D-pad), user can navigate between different controls.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Directional navigation not supported</strong> </br>Der Benutzer kann nicht zwischen verschiedenen Steuerelementen mit Richtungstasten navigieren. Other methods of navigating between controls (tab key) are not impacted.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Built in keyboard optimization

Je nach Layout und Steuerelementen können UWP-Apps speziell für die Tastatureingabe optimiert werden.

Das folgende Beispiel zeigt eine Gruppe von Listenelementen, Rasterelementen und Menüelementen, die zu einem einzelnen Tabstopp zugewiesen wurden (siehe Abschnitt [Tabstopps](#tab-stops)). Wenn die Gruppe im Fokus steht, wird die interne Navigation mit den Richtungspfeiltasten in der entsprechenden visuellen Reihenfolge ausgeführt (siehe Abschnitt [Navigation](#navigation)).

![Pfeiltastennavigation in einzelner Spalte](images/keyboard/single-column-arrow.png)

***Single Column Arrow Key Navigation***

![Pfeiltastennavigation in einzelner Zeile](images/keyboard/single-row-arrow.png)

***Single Row Arrow Key Navigation***

![Pfeiltastennavigation in mehreren Spalten und Zeilen](images/keyboard/multiple-column-and-row-navigation.png)

***Multiple Column/Row Arrow Key Navigation***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Umschließen homogener Listen- und Rasteransichtselemente

Die direktionale Navigation ist nicht immer die effizienteste Möglichkeit, durch mehrere Zeilen und Spalten der Listen- und Rasteransichtselemente zu navigieren.

**HINWEIS** Menüelemente sind in der Regel Listen mit einer Spalte, aber in einigen Fällen können spezielle Fokusregeln gelten (siehe [Popup-UI](#popup-ui)).

Listen- und Rasterobjekte können mit mehreren Zeilen und Spalten erstellt werden. Diese befinden sich in der Regel in einer zeilenweise absteigenden (Ausfüllen einer kompletten Zeile vor dem Ausfüllen der nächsten Zeile) oder spaltenweise absteigenden (Ausfüllen einer kompletten Spalte vor dem Ausfüllen der nächsten Spalte) Reihenfolge. Die zeilenweise oder spaltenweise absteigende Reihenfolge hängt von der Bildlaufrichtung ab, und Sie sollten sicherstellen, dass es keinen Konflikt mit der Elementreihenfolge gibt.

Wenn bei zeilenweise absteigender Reihenfolge (Eingabe der Elemente von links nach rechts und von oben nach unten) der Fokus auf dem letzten Element in einer Zeile liegt und die NACH-RECHTS-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Zeile verlagert. Dieses Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Zeile liegt und die NACH-LINKS-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Zeile verlagert.

Wenn bei spaltenweise absteigender Reihenfolge (Eingabe der Elemente von oben nach unten und von links nach rechts) der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Spalte verlagert. Dieses Verhalten tritt auch umgekehrt auf: Wenn der Fokus auf dem ersten Element in einer Spalte liegt und die NACH-OBEN-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Spalte verlagert.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Row major keyboard navigation</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Column major keyboard navigation</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Popup UI

As mentioned, you should try to ensure directional navigation corresponds to the visual order of the controls in your application's UI.

Some controls (such as the context menu, CommandBar overflow menu, and AutoSuggest menu) display a menu popup in a location and direction (downwards by default) relative to the primary control and available screen space. Note that the opening direction can be affected by a variety of factors at run time.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

For these controls, when the menu is first opened (and no item has been selected by the user), the Down arrow key always sets focus to the first item while the Up arrow key always sets focus to the last item on the menu. 

If the last item has focus and the Down arrow key is pressed, focus moves to the first item on the menu. Similarly, if the first item has focus and the Up arrow key is pressed, focus moves to the last item on the menu. This behavior is referred to as *cycling* and is useful for navigating popup menus that can open in unpredictable directions.

> [!NOTE]
> Cycling should be avoided in non-popup UIs where users might come to feel trapped in an endless loop. 

We recommend that you emulate these same behaviors in your custom controls. Code sample on how to implement this behavior can be found in [Programmatic focus navigation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) documentation.

## <a name="test-your-app"></a>Testen der App

Testen Sie Ihre App mit allen unterstützten Eingabegeräten, um sicherzustellen, dass eine einheitliche und intuitive Navigation zu UI-Elementen möglich ist und keine unerwarteten Elemente die gewünschte Aktivierreihenfolge beeinträchtigen.

## <a name="related-articles"></a>Verwandte Artikel
* [Keyboard events](keyboard-events.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
* [Respond to the presence of the touch keyboard](respond-to-the-presence-of-the-touch-keyboard.md)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

## <a name="appendix"></a>Anhang

### <a name="software-keyboard"></a>Software keyboard

Bei der Softwaretastatur handelt es sich um eine Tastatur auf dem Bildschirm, die der Benutzer anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, Zeichen-/Eingabestift oder mit einem anderen Zeigegerät verwenden kann. Ein Touchscreen ist nicht erforderlich. Auf einem Touchscreen kann diese Tastatur auch direkt zum Eingeben von Text verwendet werden. Auf Xbox One-Geräten müssen einzelne Tasten ausgewählt werden, indem Sie visuelle Fokuselemente verschieben oder Tastenkombinationen mit dem Gamepad oder der Fernbedienung verwenden.

![Windows 10-Bildschirmtastatur](images/keyboard/default.png)

***Windows 10 Touch Keyboard***

![Xbox one-Bildschirmtastatur](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One Onscreen Keyboard***

Abhängig vom Gerät wird die Softwaretastatur angezeigt, wenn ein Textfeld oder ein anderes bearbeitbares Textsteuerelement im Fokus steht, oder wenn der Benutzer sie über das **Benachrichtigungs-Center**manuell aktiviert.

![Symbol der Bildschirmtastatur im Benachrichtigungs-Center](images/keyboard/touch-keyboard-notificationcenter.png)

Wenn Ihre App den Fokus programmgesteuert auf ein Texteingabesteuerelement festlegt, wird die Bildschirmtastatur nicht aufgerufen. Dadurch wird unerwartetes, nicht direkt vom Benutzer ausgelöstes Verhalten verhindert. Allerdings wird die Tastatur automatisch ausgeblendet, wenn der Fokus programmgesteuert auf ein nicht textuelles Eingabesteuerelement bewegt wird.

Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann je nach den anderen Steuerelementtypen innerhalb des Formulars variieren.

Im Folgenden finden Sie eine Liste der nicht bearbeitbaren Steuerelemente, die in einer Texteingabesitzung mit der Bildschirmtastatur den Fokus erhalten können, ohne dass die Tastatur ausgeblendet wird. Statt die Benutzeroberfläche unnötigerweise zu ändern und den Benutzer möglicherweise zu verwirren, bleibt die Bildschirmtastatur angezeigt, da der Benutzer wahrscheinlich zwischen diesen Steuerelementen und der Texteingabe über die Bildschirmtastatur hin und her wechselt.

-   Kontrollkästchen
-   Kombinationsfeld
-   Optionsschaltfläche
-   Bildlaufleiste
-   Struktur
-   Strukturelement
-   Menü
-   Menüleiste
-   Menüelement
-   Symbolleiste
-   Liste
-   Listenelement

Hier finden Sie einige Beispiele für verschiedene Modi der Touch-Bildschirmtastatur. Das erste Bild zeigt das Standardlayout, das zweite das Daumenlayout. (Letzteres ist unter Umständen nicht in allen Sprachen verfügbar.)

![Bildschirmtastatur mit Standardlayout](images/keyboard/default.png)

***The touch keyboard in default layout mode***

![Bildschirmtastatur mit erweitertem Layout](images/keyboard/extendedview.png)

***The touch keyboard in expanded layout mode***

Erfolgreiche Tastaturinteraktionen ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen; Benutzer können demnach über die Tastatur alle interaktiven Elemente erreichen und Standardfunktionen aktivieren. Eine Reihe von Faktoren kann den Erfolg beeinflussen, z. B. Tastaturnavigation, Tastenkombinationen für die Barrierefreiheit sowie Tastenkombinationen für erfahrene Benutzer.

**NOTE**  The touch keyboard does not support toggle and most system commands.

#### <a name="on-screen-keyboard"></a>Bildschirmtastatur
Wie bei der Softwaretastatur handelt es sich bei der Bildschirmtastatur um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, Zeichen-/Eingabestift oder mit einem anderen Zeigegerät verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Die Bildschirmtastatur kann auf der Seite „Tastatur“ unter „Einstellungen &gt; Erleichterte Bedienung“ aktiviert werden.

**HINWEIS** Die Bildschirmtastatur hat Priorität gegenüber der Touch-Tastatur. Diese wird nicht angezeigt, wenn die Bildschirmtastatur angezeigt wird.

![Bildschirmtastatur](images/keyboard/osk.png)

***Bildschirmtastatur***

Gehen Sie zur [Seite „Bildschirmtastatur“](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard), um weitere Informationen zur Bildschirmtastatur zu erhalten.

## <a name="related-articles"></a>Verwandte Artikel

- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)
