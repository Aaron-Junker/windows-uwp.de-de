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
ms.openlocfilehash: e3fcf6b792990fad9cb0071aece878cac31f5420
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662915"
---
# <a name="keyboard-interactions"></a>Tastaturinteraktionen

![Tastaturfavoritenbild](images/keyboard/keyboard-hero.jpg)

Erfahren Sie, wie Sie Ihre UWP-Apps entwerfen und optimieren, sodass sie die bestmögliche Umgebung für erfahrene Tastaturbenutzer und Personen mit Einschränkungen und anderen Anforderungen an Barrierefreiheit bieten.

Auf verschiedenen Geräten ist die Tastatureingabe ein wichtiger Teil der Gesamtinteraktion mit der Universellen Windows-Plattform (UWP). Eine gut durchdachte Tastatur ermöglicht Benutzern die effiziente UI-Navigation in Ihrer App und Zugriff auf die gesamte Funktionalität, ohne die Hände von der Tastatur zu nehmen.

![Bild von Tastatur und Gamepad](images/keyboard/keyboard-gamepad.jpg)

***Allgemeine Interaktionsmuster werden von Tastatur und Gamepad gemeinsam genutzt.***

In diesem Thema konzentrieren wir uns speziell auf das UWP-App-Design für die Tastatureingabe auf PCs. Eine gut entworfene Tastatur-Benutzeroberfläche ist jedoch wichtig für die Unterstützung von Eingabehilfen-Tools wie Windows-Sprachausgabe, mithilfe von [Software Tastaturen](#software-keyboard) wie z. B. der Bildschirmtastatur und das On-Screen-Tastatur (Bildschirmtastatur), und klicken Sie für die Behandlung von anderen Geben Sie ein Gerät und Typen, z. B. die Xbox-Gamepad und Remotesteuerung.

Viele der hier erwähnten Richtlinien und Empfehlungen, einschließlich [visueller Fokuselemente](#focus-visuals), [Zugriffstasten](#access-keys), und [UI-Navigation](#navigation), gelten auch für diese anderen Szenarien.

**HINWEIS** Während Hardware- und Softwaretastaturen für die Texteingabe verwendet werden, liegt der Fokus dieses Themas auf Navigation und Interaktion.

## <a name="built-in-support"></a>Integrierte Unterstützung

Zusammen mit der Maus ist die Tastatur das am häufigsten verwendete Peripheriegerät für PCs und daher ein wesentlicher Bestandteil des PC-Erlebnisses. PC-Benutzer erwarten ein umfassendes und konsistentes Erlebnis bei Tastatureingaben in System-Apps und individuellen Apps.

Alle UWP-Steuerelemente enthalten integrierte Unterstützung für umfangreiche Tastaturfunktionen und Benutzerinteraktionen, während die Plattform selbst eine umfassende Grundlage für das Erstellen von Tastaturfunktionen bietet, die Ihrer Meinung nach am besten für Ihre benutzerdefinierten Steuerelemente und Apps geeignet sind.

![Bild von Tastatur mit Telefon](images/keyboard/keyboard-phone.jpg)

***UWP unterstützt Tastatur mit jedem Gerät***

## <a name="basic-experiences"></a>Grundlegende Funktionen
![Fokusbasierte Geräte](images/keyboard/focus-based-devices.jpg)

Wie bereits erwähnt, ist die Tastatureingabe für Navigation und Befehle von Eingabegeräten wie Xbox-Gamepad und -Fernbedienung und Eingabehilfen wie der Sprachausgabe zum Großteil gleich. Die Einheitlichkeit in Bezug auf Eingabetypen und Tools minimiert zusätzliche Aufgaben und trägt zur Erreichung des Ziels der Universellen Windows-Plattform bei: einmal erstellen, überall ausführen.

Bei Bedarf werden wir wichtige Unterschiede identifizieren, die sollten Sie berücksichtigen und beschreiben alle vorbeugenden Maßnahmen, die Sie berücksichtigen sollten.

Hier sind die Geräte und die Tools, die in diesem Thema erläutert werden:

| Gerät/Tool                       | Beschreibung     |
|-----------------------------------|-----------------|
|Tastatur (Hardware und Software)   |Zusätzlich zu den standard-Hardware-Tastatur, unterstützt die UWP-Anwendungen zwei Software Tastaturen: die [Touch (oder Software) Tastatur](#software-keyboard) und [Bildschirmtastatur](#on-screen-keyboard).|
|Gamepad und Remotesteuerung         |Xbox-Gamepad und -Fernbedienung sind grundlegende Eingabegeräte im [10-Fuß-Bereich](../devices/designing-for-tv.md). Ausführliche Informationen zur UWP-Unterstützung für Gamepad und Fernbedienung finden Sie unter [Interaktionen mit Gamepad und Fernbedienung](gamepad-and-remote-interactions.md).|
|Bildschirmleseprogramme (Sprachausgabe)          |Die Sprachausgabe ist ein integriertes Bildschirmleseprogramm für Windows, das eindeutige Interaktion und Funktionalität bietet, aber dennoch auf der einfachen Tastaturnavigation und -eingabe basiert. Weitere Informationen zur Sprachausgabe finden Sie unter [Erste Schritte mit Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-narrator-get-started).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Benutzerdefinierte Erfahrungen und effiziente Tastaturfunktionen
Wie bereits erwähnt, ist Die Tastaturunterstützung ist ein wesentlicher Bestandteil dafür, dass Ihre Anwendungen optimal für Benutzer mit unterschiedlichen Kenntnissen, Fähigkeiten und Erwartungen funktionieren. Wir empfehlen Ihnen, folgendes zu priorisieren.
- Unterstützung der Tastaturnavigation und Interaktion
    - Stellen Sie sicher, dass ausführbare Elemente als Tabstopps gekennzeichnet sind (und nicht bedienbare Elemente andersartig gekennzeichnet sind), und die Reihenfolge für die Seitennavigation logisch und vorhersehbar ist (weiter Informationen finden Sie unter [Tabstopps](#tab-stops))
    - Legen Sie den anfänglichen Fokus auf das logischste Element fest (weitere Informationen finden Sie unter [Anfänglicher Fokus](#initial-focus))
    - Bieten Sie eine Navigation mittels Pfeiltasten für "innere Navigationsvorgänge" an (siehe [Navigation](#navigation))
- Unterstützen Sie Tastenkombinationen
    - Stellen Sie Zugriffstasten für schnelle Aktionen bereit (siehe [Schnellinfos](#accelerators))
    - Geben Sie Zugriffsschlüssel, um Benutzeroberfläches Ihrer Anwendung zu navigieren (siehe [Zugriffsschlüssel](access-keys.md))

### <a name="focus-visuals"></a>Visuelle Fokuselemente

Die UWP unterstützt ein einzelnes Design für visuelle Fokuselemente, das gut für alle Eingabearten und -funktionen funktioniert.
![Visuelle Fokuselement](images/keyboard/focus-visual.png)

Ein visuelles Fokuselement:

- wird angezeigt, wenn ein UI-Element über eine Tastatur und/oder ein Gamepad/eine Fernbedienung den Fokus erhält
- wird als hervorgehobener Rahmen um das UI-Element wiedergegeben, um anzugeben, dass eine Aktion ausgeführt werden kann
- unterstützt einen Benutzer dabei, in einer App-UI zu navigieren, ohne die Orientierung zu verlieren
- kann für Ihre App angepasst werden (siehe [Visuelle Fokuselemente mit hoher Sichtbarkeit](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**HINWEIS** Das visuelle UWP-Fokuselement entspricht nicht dem Fokusrechteck der Sprachausgabe.

### <a name="tab-stops"></a>Tabstopps

Damit ein Steuerelement (einschließlich der Navigationselemente) über die Tastatur verwendet werden kann, muss auf dem Steuerelement der Fokus liegen. Eine Möglichkeit für ein Steuerelement den Tastaturfokus empfangen ist darauf zugegriffen werden kann durch die Registerkartennavigation durch das Identifizieren sie als Registerkarte in der Reihenfolge Ihrer Anwendung beenden.

Für ein Steuerelement in der Aktivierreihenfolge einbezogen werden die [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) Eigenschaft muss festgelegt werden, um **"true"** und [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) Eigenschaft muss festgelegt werden, um **"true"**.

Um explizit ein Steuerelement aus der Aktivierreihenfolge auszuschließen, legen Sie die [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) Eigenschaft **"false"**.

Standardmäßig entspricht die Aktivierreihenfolge der Reihenfolge, in der UI-Elemente erstellt werden. Wenn beispielsweise ein `StackPanel` die Elemente `Button`, `Checkbox` und `TextBox` enthält, ist die Aktivierreihenfolge `Button`, `Checkbox` und`TextBox`.

Sie können die Standardaktivierreihenfolge überschreiben, indem Sie die Einstellung der [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) Eigenschaft.

#### <a name="tab-order-should-be-logical-and-predictable"></a>Aktivierreihenfolge sollte logisch und vorhersehbar sein

Ein gut durchdachtes Tastatur-Navigationsmodell, das eine logische und vorhersehbare Aktivierreihenfolge verwendet, macht Ihre App intuitiver und unterstützt Benutzer dabei, Funktionen effizienter und effektiver zu erkunden, zu identifizieren und auf sie zuzugreifen.

Alle interaktiven Steuerelemente müssen Tabstopps (es sei denn, sie werden eine [Gruppe](#control-group)), sollten Sie bei der nicht interaktive Steuerelemente, z. B. Bezeichnungen nicht.

Vermeiden Sie eine benutzerdefinierte Aktivierreihenfolge, durch die der Fokus in Ihrer Anwendung wechselt. Beispielsweise sollte eine Liste mit Steuerelementen in einem Formular eine Aktivierreihenfolge aufweisen, die von oben nach unten und von links nach rechts (je nach Gebietsschema) verläuft.

Finden Sie unter [Tastatur Barrierefreiheit](../accessibility/keyboard-accessibility.md) ausführliche Informationen zum Anpassen von Tabstopps.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Koordinieren von Aktivierreihenfolge und visueller Reihenfolge

Koordinieren Aktivierreihenfolge und visuellen Reihenfolge (auch als lesen, Reihenfolge oder Anzeigereihenfolge bezeichnet) können eine Verwirrung von Benutzern zu reduzieren, während sie durch die Benutzeroberfläche Ihrer Anwendung navigieren.

Versuchen Sie, die wichtigsten Befehle, Steuerelemente und Inhalte zuerst in der Aktivierreihenfolge und der visuellen Reihenfolge einzustufen und darzustellen. Die tatsächliche Anzeigeposition kann jedoch vom übergeordneten Layoutcontainer und bestimmten Eigenschaften der untergeordneten Elemente abhängen, die das Layout beeinflussen. Insbesondere kann sich die visuelle Reihenfolge von Layouts mit einer Raster- oder Tabellenmetapher erheblich von der Aktivierreihenfolge unterscheiden.

**HINWEIS** Die visuelle Reihenfolge ist auch von Gebietsschema und Sprache abhängig.

### <a name="initial-focus"></a>Anfänglicher Fokus

Der anfängliche Fokus gibt das UI-Element an, das den Fokus erhält, wenn eine Anwendung oder eine Seite zum ersten Mal gestartet oder aktiviert wird. Wenn Sie eine Tastatur verwenden, ist es von diesem Element, das ein Benutzer mit der Benutzeroberfläche Ihrer Anwendung startet interagieren.

Für UWP-apps Anfangsfokus festgelegt ist, auf das Element mit dem höchsten [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) den Fokus erhalten kann. Untergeordnete Elemente von Container-Steuerelementen werden ignoriert. Bei einer Verknüpfung erhält das erste Element in der visuellen Struktur den Fokus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Festlegen des anfänglichen Fokus auf das logischste Element

Legen Sie den anfänglichen Fokus auf das UI-Element für die erste, oder primäre, Aktion, die Benutzer mit der größten Wahrscheinlichkeit durchführen, wenn Sie Ihre App starten oder zu einer Seite navigieren. Beispiele:
-   Eine Foto-App, wobei der Fokus auf dem ersten Element in einer Galerie liegt
-   Eine Musik-App, wobei der Fokus auf der Wiedergabe-Taste liegt

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Legen Sie Anfangsfokus nicht auf ein Element, das ein Ergebnis möglicherweise negativ oder sogar katastrophaler Vorfall verfügbar macht.

Dieses Maß an Funktionalität muss die Auswahl des Benutzers. Das Festlegen des anfänglichen Fokus auf ein Element mit einem signifikanten Ergebnis resultiert möglicherweise in unerwünschtem Datenverlust oder Systemzugriff. Z. B. Legen Sie nicht den Fokus auf die Schaltfläche "löschen" beim Navigieren zu einer e-Mail-Nachricht.

Finden Sie unter [konzentrieren Navigation](focus-navigation.md) für Weitere Informationen zum Überschreiben der Aktivierreihenfolge.

### <a name="navigation"></a>Navigation

Die Tastaturnavigation wird in der Regel durch die TAB-Tasten und die Pfeiltasten unterstützt.

![TAB-Tasten und Pfeiltasten](images/keyboard/tab-and-arrow.png)

Standardmäßig weisen UWP-Steuerelemente dieses grundlegende Tastaturverhalten auf:
-   **TAB-Tasten** dienen der Navigation zwischen ausführbaren/aktiven Steuerelementen in der Aktivierreihenfolge.
-   **UMSCHALT + TAB** dient der Navigation zwischen Steuerelementen in umgekehrter Aktivierreihenfolge. Wenn der Benutzer mit der Pfeiltaste in das Steuerelement navigiert ist, liegt der Fokus auf dem letzten bekannten Wert im Steuerelement.
-   **Pfeiltasten** ermöglichen die steuerelementspezifische „interne Navigation“, wenn der Benutzer zur „internen Navigation“ wechselt. Pfeiltasten dienen nicht der Navigation außerhalb des Steuerelements. Beispiele:
    -   Nach oben/nach-unten-Taste wird Fokus innerhalb verschoben `ListView` und `MenuFlyout`
    -   Ändern Sie die ausgewählten Werte für `Slider` und `RatingsControl`
    -   Verschiebt die Einfügemarke innerhalb `TextBox`
    -   Elemente in aufklappen/Zuklappen `TreeView`

Verwenden Sie dieses Standardverhalten, um Ihrer Anwendung die Tastaturnavigation zu optimieren.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Verwenden Sie die „interne Navigation“ mit Gruppen verwandter Steuerelemente.

Pfeil Key Navigation in einen Satz von verwandten Steuerelementen bereitstellen verstärkt ihre Beziehung innerhalb der gesamten Organisation von der Benutzeroberfläche Ihrer Anwendung.

Das hier gezeigte Steuerelement `ContentDialog` ermöglicht beispielsweise standardmäßig die interne Navigation für eine horizontale Reihe von Tasten. (Informationen zu benutzerdefinierten Steuerelementen finden Sie im Abschnitt [Steuerelementgruppe](#control-group)).

![Dialogfeldbeispiel](images/keyboard/dialog.png)

***Interaktion mit einer Auflistung von zugehörigen Schaltflächen wird mit Pfeil Key Navigation erleichtert.***

Wenn Elemente in einer einzelnen Spalte angezeigt werden, erfolgt die Elementnavigation mit der Nach-oben-/Nach-unten-Pfeiltaste. Wenn Elemente in einer einzelnen Zeile angezeigt werden, erfolgt die Elementnavigation mit der Rechts-/Links-Pfeiltaste. Wenn sich die Elemente in mehreren Spalten befinden, erfolgt die Navigation über alle 4 Pfeiltasten.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Definieren Sie einen einzelnen Tabstopp für eine Sammlung von verwandten Steuerelementen

Definieren Sie einen einzelnen Tabstopp für eine Auflistung von zugehörigen oder ergänzen, Steuerelemente, können Sie die Anzahl der insgesamt Tabstopps in Ihrer app minimieren.

Die folgenden Abbildungen zeigen beispielsweise zwei gestapelte `ListView`-Steuerelemente. Die Abbildung auf der linken Seite zeigt die Pfeiltastennavigation zwischen `ListView`-Steuerelementen für einen Tabstopp. Die Abbildung auf der rechten Seite zeigt, wie die Navigation zwischen untergeordneten Elementen einfacher und effizienter gestaltet werden kann, da die Navigation in übergeordneten Steuerelementen nicht mit einer TAB-Taste erfolgt.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaktion mit zwei gestapelte ListView-Steuerelemente kann einfacher und effizienter durch Beseitigen des Tabstopps, und navigieren Sie mit nur Pfeiltasten vorgenommen werden.***

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

Tastenkombinationen in Visual Studio erleichtern, Ihre app zu verwenden, indem Sie sowohl erweiterte Unterstützung für den Zugriff auf und verbessert die Effizienz für Benutzer über die Tastatur.

Zusätzlich zur Unterstützung von Navigation per Tastatur und der Aktivierung in Ihrer app, ist es auch empfiehlt sich, die Tastenkombinationen für die Funktionalität Ihrer Anwendung bereitstellen. Registerkartennavigation bietet eine gute, einfache Bildschirmtastatur-Unterstützung, jedoch mit komplexere Benutzeroberfläche, die Sie Unterstützung für Tastenkombinationen auch hinzufügen möchten. 

Tastenkombinationen sind eine effiziente Methode für den Zugriff auf App-Funktionen und verbessern daher die Produktivität der Benutzer. Es gibt zwei Arten von Tastenkombinationen:
-   [Accelerators](#accelerators) sind Verknüpfungen, die einen app-Befehl aufrufen. Ihre app auch kann bereitgestellt oder nicht Benutzeroberfläche, die den Befehl entspricht. Accelerators bestehen in der Regel die STRG-Taste und einer Buchstabentaste.
-   [Zugriffsschlüssel](#access-keys) sind Verknüpfungen, die Fokus auf der Benutzeroberfläche in Ihrer Anwendung festgelegt. Access Keys Typicaly bestehen aus Alt-Taste und einer Buchstabentaste.

Bereitstellung konsistenten Tastenkombinationen, die ähnliche Aufgaben in Anwendungen zu unterstützen sind sie viel nützlicher und leistungsfähiger und hilft Benutzern, die sie denken Sie daran.

#### <a name="accelerators"></a>Schnellinfos

Accelerators helfen Benutzern, die häufig verwendete Aktionen in einer Anwendung wesentlich schneller und effizienter ausführen. 

Beispiele für Tastenkombinationen mit der STRG-Taste:
-   Drücken von STRG + N eine beliebige Stelle im Schlüssel der **-e-Mails** Anwendung gestartet wird, ein neue e-Mail-Element.
-   Drücken von STRG + E an einer beliebigen Stelle in Microsoft Edge (und viele Microsoft Store-Anwendungen), wird die Suche gestartet.

Tastenkombinationen mit der STRG-Taste weisen die folgenden Merkmale auf:
-   Sie verwenden in erster Linie STRG und die Funktion Schlüssel Sequenzen (Tastenkombinationen für Windows-System auch verwenden Alt + alphanumerische Schlüssel und die Windows-Logo-Taste).
-   Sie werden nur den am häufigsten verwendeten Befehlen zugewiesen.
-   Ihre Speicherung ist vorgesehen, und sie werden nur in Menüs, QuickInfos und in der Hilfe dokumentiert.
-   Sie wirken sich in der gesamten Anwendung, wenn unterstützt.
-   Sie sollten konsistent zugewiesen werden, wie sie merken können und nicht direkt dokumentiert sind.

#### <a name="access-keys"></a>Zugriffstasten

Auf der Seite [Tastenkombinationen mit der STRG-Taste](access-keys.md) finden Sie umfassendere Informationen zur Unterstützung von Tastenkombinationen mit der STRG-Taste mit UWP.

Tastenkombinationen mit der STRG-Taste ermöglichen es Benutzern mit motorischen Einschränkungen, jeweils eine Taste zu drücken, um eine Aktion für ein bestimmtes Element in der UI durchzuführen. Darüber hinaus können Tastenkombinationen mit der STRG-Taste verwendet werden, um zusätzliche Tastenkombinationen zu kommunizieren, damit erfahrene Benutzer schnell Aktionen durchführen können.

Zugriffstasten weisen die folgenden Merkmale auf:
-   Sie verwenden ALT und eine alphanumerische Taste.
-   Sie dienen in erster Linie der Barrierefreiheit.
-   Sind dokumentiert direkt auf der Benutzeroberfläche, die neben dem Steuerelement, über [Zugriffstasteninfos](access-keys.md).
-   Sie wirken sich nur auf das aktuelle Fenster aus und navigieren zum entsprechenden Menüelement oder Steuerelement.
-   Zugriffsschlüssel sollte konsistent zugewiesen werden, um häufig verwendete Befehle (insbesondere Commit Schaltflächen), wann immer möglich.
-   Sie sind lokalisiert.

#### <a name="common-keyboard-shortcuts"></a>Häufig verwendete Tastenkombinationen

In der folgende Tabelle ist ein kleiner Teil häufig verwendeten Tastenkombinationen. 

| Aktion                               | Tastenkombination                                      |
|--------------------------------------|--------------------------------------------------|
| Alle auswählen                           | STRG+A                                           |
| Fortlaufend auswählen                  | UMSCHALT+Pfeiltaste                                  |
| Speichern                                 | STRG+S                                           |
| Suchen                                 | STRG+F                                           |
| Print                                | STRG+P                                           |
| Kopieren                                 | STRG+C                                           |
| Ausschneiden                                  | STRG+X                                           |
| Einfügen                                | STRG+V                                           |
| Rückgängig machen                                 | STRG+Z                                           |
| Nächste Registerkarte                             | STRG+TAB                                         |
| Registerkarte schließen                            | STRG+F4 oder STRG+W                                |
| Semantischer Zoom                        | STRG+PLUS-TASTE oder STRG+MINUS-TASTE                                 |

Eine umfassende Liste der Tastenkombinationen für Windows-System, finden Sie unter [Tastenkombinationen für Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). Allgemeine anwendungsverknüpfungen, finden Sie unter [Tastenkombinationen für Microsoft-Anwendungen](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Erweiterte Funktionen

In diesem Abschnitt erläutern wir einige der komplexeren Tastaturinteraktionen, die durch UWP-Apps unterstützt werden, sowie einige der Verhaltensweisen, die Sie bei Verwendung Ihrer App auf verschiedenen Geräten und mit verschiedenen Tools berücksichtigen sollten.

### <a name="control-group"></a>Steuerelementgruppe

Sie können eine Gruppe verwandter oder komplementärer Steuerelemente in einer „Steuerelementgruppe“ (oder einem direktionalen Bereich) gruppieren, was die „interne Navigation“ mit den Pfeiltasten aktiviert. Die Steuerelementgruppe kann ein einzelner Tabstopp sein, oder Sie können mehrere Tabstopps innerhalb der Steuerelementgruppe angeben.

#### <a name="arrow-key-navigation"></a>Pfeiltastennavigation

Benutzer erwarten Unterstützung für die Pfeiltastennavigation, wenn eine Gruppe ähnlicher, verwandter Steuerelemente in einem UI-Bereich vorhanden ist:
-   `AppBarButtons` in einem `CommandBar`
-   `ListItems` oder `GridItems` in `ListView` oder `GridView`
-   `Buttons` innerhalb `ContentDialog`

UWP-Steuerelemente unterstützen die Pfeiltastennavigation standardmäßig. Verwenden Sie für benutzerdefinierte Layouts und Steuerelementgruppen `XYFocusKeyboardNavigation="Enabled"`, um ein ähnliches Verhalten zu erzeugen.

Erwägen Sie Unterstützung für die Pfeil-Key-Navigation, wenn Sie die folgenden Steuerelemente verwenden:

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Dialogfelder</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems und GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Tabstopps

Abhängig von Ihrer Anwendungsverzeichnis Funktionen und Layout möglicherweise am besten bei der Navigation für eine Steuerelementgruppe einen einzelnen Tabstopp mit Pfeil-Navigation zu untergeordneten Elementen, mehrere Tabstopps oder eine Kombination aus.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Verwenden mehrerer Tabstopps und Pfeiltasten für Schaltflächen

Benutzer, für die Barrierefreiheit wichtig ist, benötigen gut etablierte Tastaturnavigationsregeln, bei denen nicht typischerweise Pfeiltasten zum Navigieren in verschiedenen Schaltflächen verwendet werden. Allerdings empfinden Benutzer ohne Sehschwäche das Verhalten evtl. als natürlich.

Ein Beispiel des UWP-Standardverhaltens ist in diesem Fall das `ContentDialog`. Während Pfeiltasten zum Navigieren zwischen Schaltflächen verwendet werden können, ist jede Schaltfläche auch ein Tabstopp.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Zuweisen einzelner Tabstopps zu vertrauten UI-Mustern

In Fällen, in denen das Layout einem bekannten UI-Muster für Steuerelementgruppen folgt, kann das Zuweisen eines einzelnen Tabstopps zur Gruppe die Navigationseffizienz für Benutzer verbessern.

Dazu gehören:
-   `RadioButtons`
-   Mehrere `ListViews` , die wie folgt aussehen und Verhalten sich wie ein einzelnes `ListView`
-   Jede UI, die wie ein Kachelraster aussehen und sich entsprechend verhalten soll (z. B. Kacheln im Startmenü)

#### <a name="specifying-control-group-behavior"></a>Angeben von Steuerelementgruppenverhalten

Verwenden Sie die folgenden APIs, um benutzerdefiniertes Steuerelementgruppenverhalten zu unterstützen (alle werden später in diesem Thema ausführlich erläutert):

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) ermöglicht die Pfeiltastennavigation zwischen Steuerelementen
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) gibt an, ob mehrere Tabstopps vorhanden sind oder ein einzelner Tabstopp vorhanden ist
-   [FindFirstFocusableElement und FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) legen den Fokus auf das erste Element mit der Taste **POS1** und das letzte Element mit der Taste **ENDE**

Die folgende Abbildung zeigt ein intuitives Tastaturnavigationsverhalten für eine Steuerelementgruppe mit zugeordneten Optionsfeldern. In diesem Fall wird Folgendes empfohlen: ein einzelner Tabstopp für die Steuerelementgruppe, interne Navigation zwischen den Optionsfeldern mit den Pfeiltasten, Taste **POS1** verknüpft mit dem ersten Optionsfeld und Taste **ENDE** verknüpft mit dem letzten Optionsfeld.

![Kombination](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Tastatur und Sprachausgabe

Die Sprachausgabe ist eine UI-Eingabehilfe für Tastaturbenutzer. (Weitere Eingabetypen werden ebenfalls unterstützt.) Allerdings geht die Sprachausgabefunktionalität über die Tastaturinteraktionen hinaus, die von den UWP-Apps unterstützt werden, und beim Entwerfen der UWP-App für die Sprachausgabe ist besondere Sorgfalt erforderlich. (Die [Seite mit den Grundlagen der Sprachausgabe](https://support.microsoft.com/help/22808/windows-10-narrator-learning-basics) leitet Sie durch die Sprachausgabe.)

Einige der Unterschiede zwischen dem UWP-Tastaturverhalten und der Sprachausgabe umfassen:
-   Zusätzliche Tastenkombinationen für die Navigation zu UI-Elementen, die nicht über die Standardtastaturnavigation verfügbar sind, z. B. FESTSTELLTASTE + Pfeiltasten zum Lesen von Steuerelementbeschriftungen.
-   Navigation zu deaktivierten Elementen. Standardmäßig werden deaktivierte Elemente nicht über die Standardtastaturnavigation verfügbar gemacht.
    -   Steuerelementansichten für schnellere Navigation basierend auf UI-Granularität. Benutzer können zu Elementen, Zeichen, Wörtern, Zeilen, Absätzen, Links, Überschriften, Tabellen, Landmarks und Vorschlägen navigieren. Die Standardtastaturnavigation macht diese Objekte als einfache Liste verfügbar, was die Navigation unter Umständen verkompliziert, es sein denn, Sie stellen Tastenkombinationen bereit.

#### <a name="case-study--autosuggestbox-control"></a>Fallstudie – AutoSuggestBox-Steuerelement

Die Suchschaltfläche für `AutoSuggestBox` ist nicht bei der standardmäßigen Tastaturnavigation mit der TAB-Taste und den Pfeiltasten zugänglich, da der Benutzer die **EINGABETASTE** drücken kann, um die Suchabfrage zu senden. Sie ist jedoch über die Sprachausgabe zugänglich, wenn der Benutzer FESTSTELLTASTE + eine Pfeiltaste drückt.

![Automatische Vorschläge für Tastaturfokus](images/keyboard/auto-suggest-keyboard.png)

*Mit der Tastatur, Benutzer durch Drücken der* ***EINGABETASTE*** *-Taste, um die Abfrage zu übermitteln.*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>Mit der Sprachausgabe, Benutzer durch Drücken der <strong>EINGABETASTE</strong> -Taste, um die Abfrage zu übermitteln.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>Mit der Sprachausgabe, Benutzer können auch die Durchsuchen-Schaltfläche mit den Zugriff auf die <strong>Feststelltaste + Pfeil nach rechts-Taste</strong>, drücken Sie dann <strong>Speicherplatz</strong> Schlüssel</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Tastatur und Xbox-Gamepad und -Fernbedienung

Xbox-Gamepads und -Fernbedienungen unterstützen das Verhalten und die Funktionen der UWP-Tastatur größtenteils. Da jedoch verschiedene Tastenoptionen einer Tastatur fehlen, stehen bei Gamepad und Fernbedienung viele Tastaturoptimierungen nicht zur Verfügung (Fernbedienung noch eingeschränkter als Gamepad).

Finden Sie unter [Gamepad und Remotesteuerung Interaktionen](gamepad-and-remote-interactions.md) Ausführlichere Informationen zur UWP-Unterstützung für Gamepad und Remotesteuerung Eingabe.

Im Folgenden finden Sie einige wichtige Zuordnungen zwischen Tastatur, Gamepad und Fernbedienung.

| **Tastatur**  | **Gamepad**                         | **Remotesteuerung**  |
|---------------|-------------------------------------|---------------------|
| LEERTASTE         | A-Taste                            | Auswahltaste       |
| EINGABETASTE         | A-Taste                            | Auswahltaste       |
| Beenden        | B-Taste                            | Zurück-Schaltfläche         |
| POS1/ENDE      | n. a.                                 | n. a.                 |
| Bild AUF/BILD AB  | Auslösertaste für vertikalen Bildlauf, Bumper-Taste für horizontalen Bildlauf   | n. a.                 |

Einige der wichtigsten Unterschiede, die Sie beim Entwerfen Ihrer UWP-App für die Verwendung mit Gamepad und Fernbedienung beachten sollten, sind:
-   Die Texteingabe erfordert, dass der Benutzer A drückt, um ein Textsteuerelement zu aktivieren.
-   Die Fokusnavigation ist nicht auf Steuerelementgruppen beschränkt, Benutzer können frei zu beliebigen fokussierbaren UI-Elementen in der App navigieren.

    **HINWEIS** Der Fokus kann auf jedes beliebige fokussierbare UI-Element in Tastendruckrichtung verlagert werden, es sei denn, es handelt sich um eine Overlay-UI, oder [Fokusaktivierung](gamepad-and-remote-interactions.md#focus-engagement) ist angegeben. Dies verhindert, dass der Fokus in einen/aus einem Bereich wechselt, bis er durch die A-Taste belegt/nicht belegt ist. Weitere Informationen finden Sie im Abschnitt zur [direktionalen Navigation](#directional-navigation).
-   Das Steuerkreuz und der linke Stick werden zum Verlagern des Fokus zwischen Steuerelementen und für die interne Navigation verwendet.

    **HINWEIS** Gamepad und Fernbedienung navigieren nur zu Elementen in derselben visuellen Reihenfolge wie die gedrückte Richtungstaste. Die Navigation in diese Richtung wird deaktiviert, wenn es kein nachfolgendes Element gibt, das den Fokus erhalten kann. Abhängig von der Situation gilt diese Einschränkung für Tastaturbenutzer nicht immer. Im Abschnitt [Integrierte Tastaturoptimierung](#built-in-keyboard-optimization) erhalten Sie weitere Infos.

#### <a name="directional-navigation"></a>Richtungsnavigation

Die direktionale Navigation wird über die UWP-Hilfsklasse [Focus Manager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) verwaltet, die die gedrückte Richtungstaste (Pfeiltaste, Steuerkreuz) verwendet und versucht, den Fokus in die entsprechende visuelle Richtung zu verlagern.

Im Gegensatz zu der Tastatur, wenn eine app von "OPTS" [Mausmodus](gamepad-and-remote-interactions.md#mouse-mode), für die gesamte Anwendung Gamepad und Remotesteuerung Richtungsnavigation angewendet wird. Finden Sie unter [Gamepad und Remotesteuerung Interaktionen](gamepad-and-remote-interactions.md) Ausführlichere Informationen zur Optimierung der Richtungsnavigation.

**HINWEIS** Die Navigation mithilfe der TAB-TASTE der Tastatur wird nicht als direktionale Navigation betrachtet. Weitere Informationen finden Sie im Abschnitt [Tabstopps](#tab-stops).

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Richtungsnavigation unterstützt</strong></br>Mithilfe von direktionalen Keys (Taste der Tastatur, Gamepad und Remotesteuerung Steuerkreuz) kann Benutzer zwischen verschiedenen Steuerelemente navigieren.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Richtungsnavigation nicht unterstützt</strong> </br>Der Benutzer kann nicht zwischen verschiedenen Steuerelementen mit Richtungstasten navigieren. Andere Methoden zum Navigieren zwischen Steuerelementen (Tab-Taste) sind nicht betroffen.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Erstellt bei der Optimierung der Tastatur

Je nach Layout und Steuerelementen können UWP-Apps speziell für die Tastatureingabe optimiert werden.

Das folgende Beispiel zeigt eine Gruppe von Listenelementen, Rasterelementen und Menüelementen, die zu einem einzelnen Tabstopp zugewiesen wurden (siehe Abschnitt [Tabstopps](#tab-stops)). Wenn die Gruppe im Fokus steht, wird die interne Navigation mit den Richtungspfeiltasten in der entsprechenden visuellen Reihenfolge ausgeführt (siehe Abschnitt [Navigation](#navigation)).

![Pfeiltastennavigation in einzelner Spalte](images/keyboard/single-column-arrow.png)

***Einzelne Spalte Pfeil-Key-Navigation***

![Pfeiltastennavigation in einzelner Zeile](images/keyboard/single-row-arrow.png)

***Einzelne Zeile Pfeil-Key-Navigation***

![Pfeiltastennavigation in mehreren Spalten und Zeilen](images/keyboard/multiple-column-and-row-navigation.png)

***Mehrere Schlüssel Spalte bzw. Zeile Pfeil-Navigation***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Umschließen homogener Listen- und Rasteransichtselemente

Die direktionale Navigation ist nicht immer die effizienteste Möglichkeit, durch mehrere Zeilen und Spalten der Listen- und Rasteransichtselemente zu navigieren.

**HINWEIS** Menüelemente sind in der Regel Listen mit einer Spalte, aber in einigen Fällen können spezielle Fokusregeln gelten (siehe [Popup-UI](#popup-ui)).

Listen- und Rasterobjekte können mit mehreren Zeilen und Spalten erstellt werden. Diese befinden sich in der Regel in einer zeilenweise absteigenden (Ausfüllen einer kompletten Zeile vor dem Ausfüllen der nächsten Zeile) oder spaltenweise absteigenden (Ausfüllen einer kompletten Spalte vor dem Ausfüllen der nächsten Spalte) Reihenfolge. Die zeilenweise oder spaltenweise absteigende Reihenfolge hängt von der Bildlaufrichtung ab, und Sie sollten sicherstellen, dass es keinen Konflikt mit der Elementreihenfolge gibt.

Wenn bei zeilenweise absteigender Reihenfolge (Eingabe der Elemente von links nach rechts und von oben nach unten) der Fokus auf dem letzten Element in einer Zeile liegt und die NACH-RECHTS-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Zeile verlagert. Dieses Verhalten tritt auf, in umgekehrter Reihenfolge: Wenn den Fokus auf das erste Element in einer Zeile festgelegt ist, und die nach-links-Taste gedrückt wird, wird den Fokus auf das letzte Element in der vorherigen Zeile verschoben.

Wenn bei spaltenweise absteigender Reihenfolge (Eingabe der Elemente von oben nach unten und von links nach rechts) der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus zum ersten Element in der nächsten Spalte verlagert. Dieses Verhalten tritt auf, in umgekehrter Reihenfolge: Bei den Fokus auf das erste Element in einer Spalte festgelegt ist, und die nach-oben-Taste gedrückt wird, wird den Fokus auf das letzte Element in der vorherigen Spalte verschoben werden.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Zeile wichtigen Tastaturnavigation</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Wichtige Tastaturnavigation Spalte</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Popup-Benutzeroberfläche

Wie bereits erwähnt, sollten Sie versuchen, um sicherzustellen, dass der visuellen Reihenfolge der Steuerelemente in der Benutzeroberfläche Ihrer Anwendung Richtungsnavigation entspricht.

Einige Steuerelemente, z. B. ContextMenu, AppBarOverflowMenu und Vorschlagssuche, enthalten ein Menü-Popupfenster, das einen Speicherort und die Richtung relativ zum das primäre Steuerelement (basierend auf verfügbaren Bildschirmplatz) angezeigt wird. Wenn beispielsweise nicht genügend Platz vorhanden ist, um das Menü nach unten (Standardrichtung) zu öffnen, wird es nach oben geöffnet. Es kann nicht garantiert werden, dass das Menü jedes Mal in dieselbe Richtung geöffnet wird.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

Wenn das Menü zum ersten Mal geöffnet wird (und vom Benutzer kein Element ausgewählt wurde), legt die NACH-UNTEN-TASTE den Fokus für diese Steuerelemente immer auf das erste Element im Menü. Die NACH-OBEN-TASTE legt den Fokus für diese Steuerelemente immer auf das letzte Element im Menü. Wenn das letzte Element ausgewählt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus gleichermaßen auf das erste Element im Menü verlagert. Wenn das erste Element ausgewählt und die NACH-OBEN-TASTE gedrückt wird, wird der Fokus gleichermaßen auf das letzte Element im Menü verlagert

Sie sollten versuchen, diese Verhaltensweisen für Ihre benutzerdefinierten Steuerelemente zu emulieren. Beispiel zur Implementierung dieses Verhaltens finden Sie im [programmgesteuerte Fokusnavigation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) Dokumentation.

## <a name="test-your-app"></a>Testen der App

Testen Sie Ihre App mit allen unterstützten Eingabegeräten, um sicherzustellen, dass eine einheitliche und intuitive Navigation zu UI-Elementen möglich ist und keine unerwarteten Elemente die gewünschte Aktivierreihenfolge beeinträchtigen.

## <a name="related-articles"></a>Verwandte Artikel
* [Tastaturereignisse](keyboard-events.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
* [Reagieren Sie auf das Vorhandensein der Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>Anhang

### <a name="software-keyboard"></a>Software-Tastatur

Bei der Softwaretastatur handelt es sich um eine Tastatur auf dem Bildschirm, die der Benutzer anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, Zeichen-/Eingabestift oder mit einem anderen Zeigegerät verwenden kann. Ein Touchscreen ist nicht erforderlich. Auf einem Touchscreen kann diese Tastatur auch direkt zum Eingeben von Text verwendet werden. Auf Xbox One-Geräten müssen einzelne Tasten ausgewählt werden, indem Sie visuelle Fokuselemente verschieben oder Tastenkombinationen mit dem Gamepad oder der Fernbedienung verwenden.

![Windows 10-Bildschirmtastatur](images/keyboard/default.png)

***Windows 10-Bildschirmtastatur***

![Xbox one-Bildschirmtastatur](images/keyboard/xbox-onscreen-keyboard.png)

***Bildschirmtastatur für Xbox One***

Abhängig vom Gerät wird die Softwaretastatur angezeigt, wenn ein Textfeld oder ein anderes bearbeitbares Textsteuerelement im Fokus steht, oder wenn der Benutzer sie über das **Benachrichtigungs-Center**manuell aktiviert.

![Symbol der Touch-Bildschirmtastatur im Benachrichtigungs-Center](images/keyboard/touch-keyboard-notificationcenter.png)

Wenn Ihre App den Fokus programmgesteuert auf ein Texteingabesteuerelement festlegt, wird die Touch-Bildschirmtastatur nicht aufgerufen. Dadurch wird unerwartetes, nicht direkt vom Benutzer ausgelöstes Verhalten verhindert. Allerdings wird die Tastatur automatisch ausgeblendet, wenn der Fokus programmgesteuert auf ein nicht textuelles Eingabesteuerelement bewegt wird.

Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann je nach den anderen Steuerelementtypen innerhalb des Formulars variieren.

Im Folgenden finden Sie eine Liste der nicht bearbeitbaren Steuerelemente, die in einer Texteingabesitzung mit der Bildschirmtastatur den Fokus erhalten können, ohne dass die Tastatur ausgeblendet wird. Statt die Benutzeroberfläche unnötigerweise zu ändern und den Benutzer möglicherweise zu verwirren, bleibt die Bildschirmtastatur angezeigt, da der Benutzer wahrscheinlich zwischen diesen Steuerelementen und der Texteingabe über die Bildschirmtastatur hin und her wechselt.

-   Kontrollkästchen
-   Kombinationsfeld
-   Optionsfeld
-   Bildlaufleiste
-   Struktur
-   Strukturelement
-   Menü
-   Menüleiste
-   Menüelement
-   Symbolleiste
-   List
-   Listenelement

Hier finden Sie einige Beispiele für verschiedene Modi der Touch-Bildschirmtastatur. Das erste Bild zeigt das Standardlayout, das zweite das Daumenlayout. (Letzteres ist unter Umständen nicht in allen Sprachen verfügbar.)

![Touch-Bildschirmtastatur mit Standardlayout](images/keyboard/default.png)

***Die Fingereingabe der Tastatur im Layoutmodus ".***

![Touch-Bildschirmtastatur mit erweitertem Layout](images/keyboard/extendedview.png)

***Der Bildschirmtastatur im erweiterten Layoutmodus "***

Erfolgreiche Tastaturinteraktionen ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen; Benutzer können demnach über die Tastatur alle interaktiven Elemente erreichen und Standardfunktionen aktivieren. Eine Reihe von Faktoren kann den Erfolg beeinflussen, z. B. Tastaturnavigation, Tastenkombinationen für die Barrierefreiheit sowie Tastenkombinationen für erfahrene Benutzer.

**Beachten Sie**  die Bildschirmtastatur ein-/ausschalten und die meisten Systembefehle nicht unterstützt.

#### <a name="on-screen-keyboard"></a>Bildschirmtastatur
Wie bei der Softwaretastatur handelt es sich bei der Bildschirmtastatur um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, Zeichen-/Eingabestift oder mit einem anderen Zeigegerät verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Die Bildschirmtastatur kann auf der Seite „Tastatur“ unter „Einstellungen &gt; Erleichterte Bedienung“ aktiviert werden.

**HINWEIS** Die Bildschirmtastatur hat Priorität gegenüber der Touch-Tastatur. Diese wird nicht angezeigt, wenn die Bildschirmtastatur angezeigt wird.

![Bildschirmtastatur](images/keyboard/osk.png)

***Bildschirmtastatur***

Gehen Sie zur [Seite „Bildschirmtastatur“](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard), um weitere Informationen zur Bildschirmtastatur zu erhalten.

## <a name="related-articles"></a>Verwandte Artikel

- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)
