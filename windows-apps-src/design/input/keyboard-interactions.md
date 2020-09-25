---
Description: Erfahren Sie, wie Sie Ihre Windows-apps entwerfen und optimieren, sodass Sie die bestmögliche Benutzerfreundlichkeit für Benutzer von Tastatur und Anwendungen mit Behinderungen und anderen Barrierefreiheits Anforderungen bereitstellen.
title: Tastaturinteraktionen
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: Tastatur, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen, Gamepad, Remote
ms.date: 09/24/2020
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: ae3d4826c4468cabea318ed230da0cfbb4d5f24b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219113"
---
# <a name="keyboard-interactions"></a>Tastaturinteraktionen

![Tastaturfavoritenbild](images/keyboard/keyboard-hero.jpg)

Erfahren Sie, wie Sie Ihre Windows-apps entwerfen und optimieren, sodass Sie die bestmögliche Benutzerfreundlichkeit für Benutzer von Tastatur und Anwendungen mit Behinderungen und anderen Barrierefreiheits Anforderungen bereitstellen.

Geräte übergreifende Tastatureingaben sind ein wichtiger Bestandteil der gesamten Windows-App-Interaktion. Eine gut entworfene Tastatur Umgebung ermöglicht Benutzern das effiziente Navigieren in der Benutzeroberfläche Ihrer APP und den Zugriff auf die vollständige Funktionalität, ohne die Hände von der Tastatur zu überarbeiten.

![Bild Tastatur und Gamepad](images/keyboard/keyboard-gamepad.jpg)

***Allgemeine Interaktionsmuster werden von Tastatur und Gamepad gemeinsam genutzt.***

In diesem Thema konzentrieren wir uns speziell auf das Windows-App-Design für Tastatureingaben auf PCs. Eine gut entworfene Tastatur ist jedoch wichtig für die Unterstützung von Tools für die Barrierefreiheit, wie z. b. die Windows-Sprachausgabe, für die Verwendung von [Software-Tastaturen](#software-keyboard) wie der touchtastatur und der Bildschirmtastatur (OSK) sowie für die Handhabung anderer Eingabegeräte Typen, wie z. b. Xbox Gamepad und Remote Steuerung.

Viele der hier erläuterten Richtlinien und Empfehlungen, einschließlich [Fokus Visuals](#focus-visuals), [Zugriffstasten](#access-keys)und [UI-Navigation](#navigation), sind auch für diese anderen Szenarien anwendbar.

**Hinweis**  Obwohl sowohl Hardware-als auch Software-Tastaturen für Texteingaben verwendet werden, liegt der Schwerpunkt dieses Themas auf Navigation und Interaktion.

## <a name="built-in-support"></a>Integrierte Unterstützung

Zusammen mit der Maus ist die Tastatur die am häufigsten verwendete Peripherie auf PCs und daher ein grundlegender Bestandteil des PC-Erlebnisses. PC-Benutzer erwarten ein umfassendes und konsistentes Verhalten sowohl aus dem System als auch aus den einzelnen Apps als Reaktion auf Tastatureingaben.

Alle UWP-Steuerelemente bieten integrierte Unterstützung für umfangreiche Tastaturfunktionen und Benutzerinteraktionen, während die Plattform selbst eine umfangreiche Grundlage für die Erstellung von Tastatur Erlebnissen bietet, die Sie sich am besten für Ihre benutzerdefinierten Steuerelemente und apps eignen.

![Tastatur mit Telefon Bild](images/keyboard/keyboard-phone.jpg)

***UWP unterstützt Tastatur mit jedem Gerät.***

## <a name="basic-experiences"></a>Grundlegende Erfahrungen
![Fokus basierte Geräte](images/keyboard/focus-based-devices.jpg)

Wie bereits erwähnt, haben Eingabegeräte wie z. b. das Xbox Gamepad und die Remote Steuerung sowie Barrierefreiheits Tools wie die Sprachausgabe einen Großteil der Tastatureingabe-Oberfläche für die Navigation und die Befehlszeile. Diese allgemeine Darstellung von Eingabetypen und Tools minimiert zusätzliche Arbeit von Ihnen und trägt dazu bei, das Ziel "Build Once, Run Anywhere" des universelle Windows-Plattform zu haben.

Bei Bedarf identifizieren wir wichtige Unterschiede, die Sie kennen sollten, und beschreiben alle Maßnahmen, die Sie berücksichtigen sollten.

Hier finden Sie die Geräte und Tools, die in diesem Thema erläutert werden:

| Gerät/Tool                       | BESCHREIBUNG     |
|-----------------------------------|-----------------|
|Tastatur (Hardware und Software)   |Neben der Standard Hardware Tastatur werden von Windows-Anwendungen zwei Software-Tastaturen unterstützt: die [touchtastatur (oder die Software Tastatur)](#software-keyboard) und die [Bildschirmtastatur](#on-screen-keyboard).|
|Gamepad und Remotesteuerung         |Das Xbox Gamepad und die Remote Steuerung sind grundlegende Eingabegeräte in der [10-Fuß-](../devices/designing-for-tv.md)Darstellung. Spezifische Informationen zur Windows-Unterstützung für Gamepad und Remote Steuerung finden Sie unter [Gamepad-und Remote Steuerungs Interaktionen](gamepad-and-remote-interactions.md).|
|Sprachausgaben (Sprachausgabe)          |Die Sprachausgabe ist eine integrierte Bildschirmausgabe für Windows, die einzigartige Interaktions Funktionen und-Funktionen bereitstellt, aber weiterhin grundlegende Tastaturnavigation und-Eingabe erfordert. Details zur Sprachausgabe finden Sie unter [Getting Started with](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)Sprachausgabe.|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Benutzerdefinierte Benutzeroberflächen und effiziente keyboardingfunktionen
Wie bereits erwähnt, ist die Tastatur Unterstützung von wesentlicher Bedeutung, um sicherzustellen, dass Ihre Anwendungen hervorragend für Benutzer mit unterschiedlichen Fähigkeiten, Fähigkeiten und Erwartungen funktionieren. Es wird empfohlen, dass Sie Folgendes priorisieren.
- Unterstützung von Tastaturnavigation und Interaktion
    - Stellen Sie sicher, dass ausführbare Elemente als Tabstopps identifiziert werden (und nicht Handlungs relevante Elemente sind nicht) und die Navigations Reihenfolge logisch und vorhersagbar ist (siehe [Tabstopps](#tab-stops)).
    - Legen Sie den anfänglichen Fokus auf das logischste Element fest (siehe [anfangs Fokus](#initial-focus)).
    - Bereitstellen der Pfeiltasten Navigation für "innere Navigation" (siehe [Navigation](#navigation))
- Unterstützung von Tastenkombinationen
    - Bereitstellen von Zugriffstasten für schnelle Aktionen (siehe [Accelerators](#accelerators))
    - Geben Sie Zugriffsschlüssel für die Navigation in der Benutzeroberfläche Ihrer Anwendung an (siehe [Zugriffsschlüssel](access-keys.md)).

### <a name="focus-visuals"></a>Visuelle Elemente im Fokus

Die UWP unterstützt einen einzelnen visuellen Fokus Entwurf, der für alle Eingabetypen und-Erfahrungen geeignet ist.
![Fokusanzeige](images/keyboard/focus-visual.png)

Eine visuelle Fokus Visualisierung:

- Wird angezeigt, wenn ein Benutzeroberflächen Element den Fokus von Tastatur und/oder Gamepad/Remote Steuerung erhält.
- Wird als hervorgehobene Rahmen um das UI-Element gerendert, um anzugeben, dass eine Aktion ausgeführt werden kann.
- Ermöglicht Benutzern die Navigation in einer App-Benutzeroberfläche, ohne zu verlieren.
- Kann für Ihre APP angepasst werden (siehe [visuelle Fokus Visuals mit hoher Sichtbarkeit](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**Hinweis** Die UWP-Fokus Visualisierung ist nicht mit dem Fokus Rechteck für die Sprachausgabe identisch.

### <a name="tab-stops"></a>Tabstopps

Damit ein Steuerelement (einschließlich der Navigationselemente) über die Tastatur verwendet werden kann, muss auf dem Steuerelement der Fokus liegen. Eine Möglichkeit für ein Steuerelement, den Tastaturfokus zu erhalten, besteht darin, ihn über die Registerkarten Navigation zugänglich zu machen, indem er als Tabstopp in der Aktivier Reihenfolge der Anwendung identifiziert

Damit ein Steuerelement in der Aktivier Reihenfolge enthalten ist, muss die [isaktivierte](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) Eigenschaft auf **true** festgelegt werden, und die [istabstopp](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) -Eigenschaft muss auf **true**festgelegt werden.

Wenn Sie ein Steuerelement explizit aus der Aktivier Reihenfolge ausschließen möchten, legen Sie die Eigenschaft [istabstopp](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) auf **false**fest.

Standardmäßig gibt die Aktivier Reihenfolge die Reihenfolge an, in der Benutzeroberflächen Elemente erstellt werden. Wenn z. b. ein `StackPanel` `Button` -, ein `Checkbox` -und ein-Wert enthält, ist die Aktivier `TextBox` Reihenfolge `Button` , `Checkbox` und `TextBox` .

Sie können die Standard Sortierreihenfolge überschreiben, indem Sie die [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) -Eigenschaft festlegen.

#### <a name="tab-order-should-be-logical-and-predictable"></a>Tab-Reihenfolge muss logisch und vorhersagbar sein.

Ein gut konzipiertes Tastatur Navigations Modell, bei dem eine logische und vorhersagbare Aktivier Reihenfolge verwendet wird, macht Ihre APP intuitiver und hilft Benutzern dabei, die Funktionalität effizienter und effektiver zu erforschen, zu entdecken und zu nutzen

Alle interaktiven Steuerelemente sollten Tabstopps haben (es sei denn, Sie befinden sich in einer [Gruppe](#control-group)), während nicht interaktive Steuerelemente, wie z. b. Bezeichnungen, nicht.

Vermeiden Sie eine benutzerdefinierte Aktivier Reihenfolge, mit der der Fokus in der Anwendung angezeigt wird. Eine Liste der Steuerelemente in einem Formular sollte z. b. eine Aktivier Reihenfolge aufweisen, die von oben nach unten und von links nach rechts (abhängig vom Gebiets Schema) verläuft.

Weitere Informationen zum Anpassen von Tabstopps finden Sie unter [Tastatur](../accessibility/keyboard-accessibility.md) Eingabe.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Koordinaten der Aktivier Reihenfolge und visuelle Reihenfolge koordinieren

Durch die Koordination der Aktivier Reihenfolge und der visuellen Reihenfolge (auch als Lesefolge oder Anzeigereihenfolge bezeichnet) werden Verwirrung für Benutzer verringert, wenn Sie durch die Benutzeroberfläche Ihrer Anwendung navigieren

Versuchen Sie, die wichtigsten Befehle, Steuerelemente und Inhalte zuerst in der Aktivier Reihenfolge und in der visuellen Reihenfolge zu ordnen und zu präsentieren. Die tatsächliche Anzeigeposition kann jedoch vom übergeordneten Layoutcontainer und bestimmten Eigenschaften der untergeordneten Elemente abhängen, die das Layout beeinflussen. Insbesondere Layouts, die eine Raster Metapher oder eine Tabellen Metapher verwenden, können eine visuelle Reihenfolge aufweisen, die sich von der Aktivier Reihenfolge unterscheidet.

**Hinweis** Die visuelle Reihenfolge ist auch vom Gebiets Schema und der Sprache abhängig.

### <a name="initial-focus"></a>Anfänglicher Fokus

Ursprünglicher Fokus gibt das Benutzeroberflächen Element an, das den Fokus erhält, wenn eine Anwendung oder eine Seite zum ersten Mal gestartet oder aktiviert wird. Wenn Sie eine Tastatur verwenden, ist es von diesem Element aus, dass ein Benutzer mit der Interaktion mit der Benutzeroberfläche Ihrer Anwendung beginnt.

Für UWP-apps wird der anfängliche Fokus auf das-Element mit dem höchsten [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) festgelegt, das den Fokus erhalten kann. Untergeordnete Elemente von Container Steuerelementen werden ignoriert. In einer Bindung erhält das erste Element in der visuellen Struktur den Fokus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Legen Sie den anfänglichen Fokus auf das logischste Element fest.

Legen Sie den anfänglichen Fokus auf das UI-Element für die erste oder primäre Aktion fest, die Benutzer am wahrscheinlichsten beim Starten der APP oder beim Navigieren zu einer Seite ausführen. Beispiele:
-   Eine Foto-APP, bei der der Fokus auf das erste Element in einem Katalog festgelegt ist
-   Eine Musik-app, bei der Fokus auf die Wiedergabe Schaltfläche gesetzt ist

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Legen Sie den anfänglichen Fokus nicht auf ein Element fest, das potenziell negatives oder sogar katastrophale Ergebnisse verfügbar macht.

Dieses Maß an Funktionalität sollte eine Benutzer Auswahl sein. Wenn Sie den anfänglichen Fokus auf ein Element mit einem signifikanten Ergebnis festlegen, kann dies zu unbeabsichtigtem Datenverlust oder System Zugriff führen. Legen Sie den Fokus beispielsweise beim Navigieren zu einer e-Mail nicht auf die Schaltfläche Löschen fest.

Weitere Informationen zum Überschreiben der Aktivier Reihenfolge finden Sie unter [Fokus Navigation](focus-navigation.md)

### <a name="navigation"></a>Navigation

Die Tastaturnavigation wird in der Regel über die Tab-Taste und die Pfeiltasten unterstützt.

![Tab-Taste und Pfeiltasten](images/keyboard/tab-and-arrow.png)

Standardmäßig folgen UWP-Steuerelemente diesen grundlegenden Tastatur Verhalten:
-   **Tab-Taste** Navigieren zwischen handlungsfähigen/aktiven Steuerelementen in der Aktivier Reihenfolge.
-   **UMSCHALT + TAB** Navigation Steuerelemente in umgekehrter Aktivier Reihenfolge. Wenn der Benutzer im Steuerelement mithilfe der Pfeiltaste navigiert hat, wird der Fokus auf den letzten bekannten Wert im Steuerelement festgelegt.
-   **Pfeiltasten** machen eine Steuerelement spezifische "innere Navigation" verfügbar, wenn der Benutzer in "innere Navigation" wechselt, und die Pfeiltasten werden nicht aus einem Steuerelement navigiert. Beispiele:
    -   Nach-oben/nach-unten-Taste Verschiebt den Fokus in `ListView` und `MenuFlyout`
    -   Aktuell ausgewählte Werte für `Slider` und ändern `RatingsControl`
    -   Einfügemarke in verschieben `TextBox`
    -   Elemente in erweitern/reduzieren `TreeView`

Verwenden Sie diese Standardverhalten, um die Tastaturnavigation Ihrer Anwendung zu optimieren.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Verwenden der inneren Navigation mit Sätzen verwandter Steuerelemente

Durch die Bereitstellung der Pfeiltasten Navigation in eine Gruppe verwandter Steuerelemente wird die Beziehung innerhalb der gesamten Organisation der Benutzeroberfläche Ihrer Anwendung verstärkt.

Beispielsweise bietet das `ContentDialog` hier gezeigte Steuerelement standardmäßig innere Navigation für eine horizontale Zeile von Schaltflächen (für benutzerdefinierte Steuerelemente finden Sie im Abschnitt [Steuerungsgruppe](#control-group) ).

![Dialog Beispiel](images/keyboard/dialog.png)

***Die Interaktion mit einer Auflistung verwandter Schaltflächen wird durch die Pfeiltasten Navigation vereinfacht.***

Wenn Elemente in einer einzelnen Spalte angezeigt werden, navigiert die nach-oben-oder nach-unten-Taste Elemente. Wenn Elemente in einer einzelnen Zeile angezeigt werden, navigiert die nach-links-Taste für die Elemente. Wenn es sich bei den Elementen um mehrere Spalten handelt, navigieren alle vier Pfeiltasten.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Definieren eines einzelnen Tabstopps für eine Sammlung verwandter Steuerelemente

Durch Definieren eines einzelnen Tabstopps für eine Auflistung verwandter, oder ergänzender Steuerelemente können Sie die Anzahl der allgemeinen Tabstopps in der APP minimieren.

Die folgenden Bilder zeigen z. b. zwei gestapelte Steuer `ListView` Elemente. Das Bild auf der linken Seite zeigt die Pfeiltasten Navigation, die mit einem Tabstopp zum Navigieren zwischen Steuerelementen verwendet wird `ListView` , während das Bild auf der rechten Seite anzeigt, wie die Navigation zwischen untergeordneten Elementen einfacher und effizienter gemacht werden kann, da übergeordnete Steuerelemente nicht mit einer Tabulator Taste durchlaufen werden müssen.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaktionen mit zwei gestapelten ListView-Steuerelementen können einfacher und effizienter gemacht werden, indem die Tab-Taste und die Navigation mit nur Pfeiltasten vermieden werden.***

Im Abschnitt [Steuerungsgruppe](#control-group) finden Sie Informationen zum Anwenden der Optimierungs Beispiele auf die Benutzeroberfläche Ihrer Anwendung.

### <a name="interaction-and-commanding"></a>Interaktion und Befehls Steuerung

Wenn ein Steuerelement den Fokus besitzt, kann ein Benutzer damit interagieren und jede zugehörige Funktionalität mithilfe bestimmter Tastatureingaben aufrufen.

#### <a name="text-entry"></a>Texteintrag

Für Steuerelemente, die speziell für Texteingaben wie `TextBox` und entworfen wurden `RichEditBox` , werden alle Tastatureingaben für die Eingabe oder Navigation von Text verwendet, der Vorrang vor anderen Tastaturbefehlen hat. Beispielsweise erkennt das Dropdown Menü für ein `AutoSuggestBox` Steuerelement die **Leerzeichen** Taste nicht als Auswahl Befehl.

![Text Eintrag](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Leertaste

Wenn Sie sich nicht im Texteingabe Modus befinden, ruft die **Leerzeichen** Taste die Aktion oder den Befehl auf, die dem fokussierten Steuerelement zugeordnet ist (wie z. b. Tippen mit berühren oder mit der Maus).

![Leertaste](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Eingabetaste

Die **Eingabe** Taste kann eine Reihe von allgemeinen Benutzerinteraktionen ausführen, abhängig vom Steuerelement mit dem Fokus:
-   Aktiviert Befehls Steuerelemente wie z `Button` `Hyperlink` . b. oder. Um Endbenutzer Verwirrung zu vermeiden, aktiviert die **Eingabe** Taste auch Steuerelemente, die wie z `ToggleButton` . b `AppBarToggleButton` . oder aussehen.
-   Zeigt die Auswahl Benutzeroberfläche für Steuerelemente wie `ComboBox` und an `DatePicker` . Die **Eingabe** Taste führt auch einen Commit für die Benutzeroberfläche für die Auswahl aus.
-   Aktiviert Listen Steuerelemente, wie z `ListView` `GridView` . b., und `ComboBox` .
    -   Die **Eingabe** Taste führt die Auswahl Aktion als **LEERTASTE** für Listen-und Raster Elemente aus, es sei denn, es gibt eine zusätzliche Aktion, die diesen Elementen zugeordnet ist (Öffnen eines neuen Fensters).
    -   Wenn dem Steuerelement eine zusätzliche Aktion zugeordnet ist, führt die **Eingabe** Taste die zusätzliche Aktion aus, und die **Speicherplatz** Taste führt die Auswahl Aktion aus.

**Hinweis** Die **Eingabe** Taste und der **LEERTASTE** führen nicht immer dieselbe Aktion aus, aber dies ist häufig der Fall.

![EINGABETASTE](images/keyboard/enter-key.png)

Mit der ESC-Taste kann ein Benutzer eine vorübergehende Benutzeroberfläche Abbrechen (zusammen mit allen laufenden Aktionen in dieser Benutzeroberfläche).

Beispiele für diese Vorgehensweise:
-   Der Benutzer öffnet einen `ComboBox` mit einem ausgewählten Wert und verwendet die Pfeiltasten, um die Fokus Auswahl auf einen neuen Wert zu verschieben. Durch Drücken der ESC-Taste wird das geschlossen `ComboBox` und der ausgewählte Wert auf den ursprünglichen Wert zurückgesetzt.
-   Der Benutzer ruft eine permanente Löschaktion für eine e-Mail auf und wird `ContentDialog` zur Bestätigung der Aktion aufgefordert. Der Benutzer entscheidet, dass dies nicht die beabsichtigte Aktion ist, und drückt die **ESC** -Taste, um das Dialogfeld zu schließen. Wenn die **ESC** -Taste der Schaltfläche **Abbrechen** zugeordnet ist, wird das Dialogfeld geschlossen, und die Aktion wird abgebrochen. Die **ESC** -Taste wirkt sich nur auf die vorübergehende Benutzeroberfläche aus, Sie wird nicht geschlossen, oder Sie Navigieren durch die Benutzeroberfläche der app.

![ESC-Taste](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>POS1- und Ende-Tasten

Die **Start** -und **Endschlüssel** ermöglichen einem Benutzer einen Bildlauf zum Anfang oder Ende eines UI-Bereichs.

Beispiele für diese Vorgehensweise:
-   Bei `ListView` -und-Steuer `GridView` Elementen verschiebt der- **Start** Schlüssel den Fokus auf das erste Element und führt einen Bildlauf in die Ansicht durch, während die **endtaste** den Fokus auf das letzte Element verschiebt und in die Ansicht übergeht.
-   Bei einem `ScrollView` Steuerelement führt der **Home** Key einen Bildlauf zum oberen Rand des Bereichs aus, während die **endtaste** zum unteren Rand des Bereichs führt (der Fokus wird nicht geändert).

![Start-und Endschlüssel](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Bild-auf-und Bild-ab-Taste

Mit den **Seiten** Schlüsseln können Benutzer einen Benutzeroberflächen Bereich in diskreten Inkrementen durchsuchen.

Bei `ListView` -und-Steuerelementen führt die Bild-auf-Taste z. b `GridView` . einen Bildlauf nach oben um eine "Seite" durch (in der Regel die Höhe des Viewports) und verschiebt den Fokus auf den oberen Rand der Region. **Page up** Alternativ führt die Bild- **ab** -Taste den Bereich durch eine Seite nach unten und verschiebt den Fokus auf den unteren Rand des Bereichs.

![Bild-auf-und Abbild Taste](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Tastenkombinationen

Mithilfe von Tastenkombinationen können Sie die Verwendung Ihrer APP vereinfachen, indem Sie eine verbesserte Unterstützung für Barrierefreiheit und eine verbesserte Effizienz für Tastatur Benutzer bereitstellen.

Neben der Unterstützung der Tastaturnavigation und-Aktivierung in Ihrer APP empfiehlt es sich auch, Verknüpfungen für die Funktionalität Ihrer Anwendung bereitzustellen. Die Registerkarten Navigation bietet eine gute, grundlegende Ebene der Tastatur Unterstützung, aber mit einer komplexeren Benutzeroberfläche können Sie auch Unterstützung für Tastenkombinationen hinzufügen. 

Tastenkombinationen sind eine effiziente Methode für den Zugriff auf App-Funktionen und verbessern daher die Produktivität der Benutzer. Es gibt zwei Arten von Tastenkombinationen:
-   [Accelerators](#accelerators) sind Verknüpfungen, die einen app-Befehl aufrufen. Ihre APP kann eine bestimmte Benutzeroberfläche bereitstellen, die dem Befehl entspricht. Zugriffstasten bestehen normalerweise aus der STRG-Taste und einem Buchstaben.
-   [Zugriffsschlüssel](#access-keys) sind Verknüpfungen, die den Fokus auf bestimmte Benutzeroberflächen in der Anwendung festlegen. Zugriffsschlüssel typicaly bestehen aus der Alt-Taste und einem Buchstaben Schlüssel.

Durch die Bereitstellung konsistentes Tastenkombinationen, die ähnliche Aufgaben über Anwendungen hinweg unterstützen, sind diese viel nützlicher und leistungsfähiger.

#### <a name="accelerators"></a>Zugriffstasten

Accelerators helfen Benutzern, gängige Aktionen in einer Anwendung wesentlich schneller und effizienter auszuführen. 

Beispiele für Accelerators:
-   Durch Drücken der Tastenkombination STRG + N in der **Mail** -APP wird ein neues e-Mail-Element gestartet.
-   Durch Drücken von STRG + E-Taste in Microsoft Edge (und viele Microsoft Store Anwendungen) wird die Suche gestartet.

Beschleuniger verfügen über die folgenden Eigenschaften:
-   Sie verwenden hauptsächlich STRG-und Funktions schlüsselsequenzen (Windows-Systemtasten Kombinationen verwenden auch alt + nicht-alphanumerische Tasten und die Windows-Taste).
-   Sie werden nur den am häufigsten verwendeten Befehlen zugewiesen.
-   Ihre Speicherung ist vorgesehen, und sie werden nur in Menüs, QuickInfos und in der Hilfe dokumentiert.
-   Sie sind in der gesamten Anwendung wirksam, wenn Sie unterstützt werden.
-   Sie sollten konsistent zugewiesen werden, wenn Sie gespeichert und nicht direkt dokumentiert werden.

#### <a name="access-keys"></a>Zugriffsschlüssel

Weitere ausführliche Informationen zur Unterstützung von Zugriffs Schlüsseln mit UWP finden Sie auf der Seite " [Zugriffsschlüssel](access-keys.md) ".

Zugriffsschlüssel unterstützen Benutzer mit Motor Funktions Behinderungen dabei, eine Taste zu einem bestimmten Zeitpunkt auf ein bestimmtes Element in der Benutzeroberfläche zu drücken. Darüber hinaus können Zugriffsschlüssel verwendet werden, um zusätzliche Tastenkombinationen zu übermitteln, damit fortgeschrittene Benutzeraktionen schnell ausführen können.

Zugriffstasten weisen die folgenden Merkmale auf:
-   Sie verwenden ALT und eine alphanumerische Taste.
-   Sie dienen in erster Linie der Barrierefreiheit.
-   Sie werden direkt in der Benutzeroberfläche, neben dem Steuerelement, über [wichtige Tipps](access-keys.md)dokumentiert.
-   Sie wirken sich nur auf das aktuelle Fenster aus und navigieren zum entsprechenden Menüelement oder Steuerelement.
-   Zugriffsschlüssel sollten nach Möglichkeit konsistent den häufig verwendeten Befehlen (insbesondere "Commit"-Schaltflächen) zugewiesen werden.
-   Sie sind lokalisiert.

#### <a name="common-keyboard-shortcuts"></a>Allgemeine Tastenkombinationen

In der folgenden Tabelle finden Sie eine kleine Stichprobe häufig verwendeter Tastenkombinationen. 

| Aktion                               | Tastaturbefehl                                      |
|--------------------------------------|--------------------------------------------------|
| Alle auswählen                           | STRG+A                                           |
| Fortlaufend auswählen                  | UMSCHALT+Pfeiltaste                                  |
| Speichern                                 | STRG+S                                           |
| Suchen                                 | STRG+F                                           |
| Drucken                                | STRG+P                                           |
| Kopieren                                 | STRG+C                                           |
| Ausschneiden                                  | STRG+X                                           |
| Einfügen                                | STRG+V                                           |
| Rückgängig machen                                 | STRG+Z                                           |
| Nächste Registerkarte                             | STRG+TAB                                         |
| Registerkarte schließen                            | STRG + F4 oder STRG + W                                |
| Semantischer Zoom                        | STRG+PLUS-TASTE oder STRG+MINUS-TASTE                                 |

Eine umfassende Liste der Windows-System Verknüpfungen finden Sie unter [Tastenkombinationen für Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). Allgemeine Anwendungs Verknüpfungen finden Sie unter [Tastenkombinationen für Microsoft-Anwendungen](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Erweiterte Erfahrungen

In diesem Abschnitt werden einige der komplexeren Tastatur Interaktionen erläutert, die von UWP-Apps unterstützt werden, sowie einige der Verhaltensweisen, die Sie kennen sollten, wenn Ihre APP auf unterschiedlichen Geräten und mit unterschiedlichen Tools verwendet wird.

### <a name="control-group"></a>Steuerelement Gruppe

Sie können eine Gruppe verwandter oder ergänzender Steuerelemente in einer "Steuerelement Gruppe" (oder einem direktionalen Bereich) gruppieren, die die "innere Navigation" mithilfe der Pfeiltasten ermöglicht. Die Kontrollgruppe kann ein einzelner Tabstopp sein, oder Sie können mehrere Tabstopps innerhalb der Steuerelement Gruppe angeben.

#### <a name="arrow-key-navigation"></a>Pfeiltasten Navigation

Benutzer erwarten Unterstützung für die Pfeiltasten Navigation, wenn eine Gruppe ähnlicher, verwandter Steuerelemente in einer Benutzeroberflächen Region vorhanden ist:
-   `AppBarButtons` in einer `CommandBar`
-   `ListItems` oder `GridItems` innerhalb von `ListView` oder `GridView`
-   `Buttons` innen `ContentDialog`

UWP-Steuerelemente unterstützen standardmäßig die Navigation per Pfeiltaste. Verwenden Sie für benutzerdefinierte Layouts und Steuerelement Gruppen, `XYFocusKeyboardNavigation="Enabled"` um ein ähnliches Verhalten bereitzustellen.

Fügen Sie Unterstützung für die Pfeiltasten Navigation in Erwägung, wenn Sie die folgenden Steuerelemente verwenden

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Dialog Felder</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>Appbarbuttons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems und GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Tabstopps

Abhängig von der Funktionalität und dem Layout Ihrer Anwendung kann die beste Navigations Option für eine Steuerelement Gruppe ein einzelner Tabstopp mit Pfeil Navigation zu untergeordneten Elementen, mehreren Tabstopps oder einer Kombination sein.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Verwenden mehrerer Tabstopps und Pfeiltasten für Schaltflächen

Barrierefreiheits Benutzer verlassen sich auf gut festgelegte Tastatur Navigationsregeln, die in der Regel keine Pfeiltasten verwenden, um eine Auflistung von Schaltflächen zu navigieren. Allerdings kann es sein, dass Benutzer ohne visuelle Beeinträchtigungen das Verhalten der Natur haben.

Ein Beispiel für das UWP-Standardverhalten ist in diesem Fall der `ContentDialog` . Während Pfeiltasten verwendet werden können, um zwischen Schaltflächen zu navigieren, ist jede Schaltfläche auch ein Tabstopp.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Zuweisen einzelner Tabstopps zu vertrauten UI-Mustern

Wenn Ihr Layout auf ein bekanntes UI-Muster für Steuerelement Gruppen folgt, kann das Zuweisen eines einzelnen Tabstopps zur Gruppe die Navigations Effizienz für Benutzer verbessern.

Beispiele:
-   `RadioButtons`
-   Mehrere `ListViews` und Verhalten sich wie ein einzelnes `ListView`
-   Alle Benutzeroberflächen, die so aussehen und Verhalten wie Raster von Kacheln (z. b. die Kacheln im Startmenü)

#### <a name="specifying-control-group-behavior"></a>Festlegen des Verhaltens von Steuerelement Gruppen

Verwenden Sie die folgenden APIs, um das Verhalten von benutzerdefinierten Steuerungsgruppen zu unterstützen (alle werden weiter unten in diesem Thema ausführlicher erläutert):

-   [Xyfocekeyboardnavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) ermöglicht die Navigation per Pfeiltaste zwischen Steuerelementen.
-   [Tabfocus Navigation](focus-navigation.md#tab-navigation) gibt an, ob mehrere Tabstopps oder einzelne Tabstopps vorhanden sind.
-   [Findfirstfocusableelement und findlastfocusableelement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) legt den Fokus auf das erste Element mit dem **Home** -Schlüssel und dem letzten Element mit dem **Endschlüssel** fest.

Die folgende Abbildung zeigt ein intuitives Verhalten der Tastaturnavigation für eine Steuerelement Gruppe zugeordneter Options Felder. In diesem Fall empfiehlt es sich, ein einzelnes Tabstopp für die Steuerelement Gruppe, die innere Navigation zwischen den Options Feldern mit den Pfeiltasten, die an das erste Optionsfeld gebundene **Start** Taste und die **endtaste** , die an das letzte Optionsfeld gebunden ist, zu verwenden.

![alles vereint](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Tastatur und Sprachausgabe

Die Sprachausgabe ist ein Tool zur Benutzeroberflächen Barrierefreiheit, das auf Tastatur Benutzer ausgerichtet ist (andere Eingabetypen werden ebenfalls unterstützt). Die Funktionen der Sprachausgabe werden jedoch über die von UWP-apps unterstützten Tastatur Interaktionen hinausgehen, und beim Entwerfen der UWP-App für die Sprachausgabe sind zusätzliche Sorgfalt erforderlich. (Die [Seite mit den Grundlagen](https://support.microsoft.com/help/22808/windows-10-narrator-basics) der Sprachausgabe führt Sie durch die benutzerfreundliche Sprachausgabe.)

Zu den Unterschieden zwischen UWP-Tastatur Verhalten und den von der Sprachausgabe unterstützten Unterschieden gehören:
-   Zusätzliche Tastenkombinationen für die Navigation zu Benutzeroberflächen Elementen, die nicht über die Standardtastatur Navigation verfügbar gemacht werden, wie z. b. die Feststell Taste + Pfeiltasten für Lese Steuerelement Bezeichnungen
-   Navigation zu deaktivierten Elementen. Deaktivierte Elemente werden standardmäßig nicht über die Standardtastatur Navigation verfügbar gemacht.
-   Steuern Sie "Ansichten" für eine schnellere Navigation basierend auf der Granularität der Benutzeroberfläche. Benutzer können zu Elementen, Zeichen, Wörtern, Zeilen, Absätzen, Links, Überschriften, Tabellen, Kenn Wörtern und Vorschlägen navigieren. Die Standard Tastaturnavigation macht diese Objekte als flache Liste verfügbar, die die Navigation möglicherweise mühsam macht, es sei denn, Sie stellen Tastenkombinationen bereit.

#### <a name="case-study--autosuggestbox-control"></a>Fallstudie – autovorschlags Box-Steuerelement

Die Such Schaltfläche für `AutoSuggestBox` ist nicht für die Standardtastatur Navigation über Tab-und Pfeiltasten zugänglich, da der Benutzer die **Eingabe** Taste drücken kann, um die Suchabfrage zu übermitteln. Es ist jedoch über die Sprachausgabe zugänglich, wenn der Benutzer die Taste gedrückt + eine Pfeiltaste drückt.

![Tastaturfokus automatisch vorschlagen](images/keyboard/auto-suggest-keyboard.png)

*Mit Tastatur drücken Benutzer die* ***Eingabe*** *Taste, um eine Suchabfrage zu übermitteln* .

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>Mit der Sprachausgabe drücken Benutzer die <strong>Eingabe</strong> Taste, um eine Suchabfrage zu übermitteln.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>Mit der Sprachausgabe können Benutzer auch auf die Such Schaltfläche zugreifen, indem Sie die Taste " <strong>Caps Lock + right Pfeil</strong>" drücken und dann die <strong>LEERTASTE</strong> drücken.</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Tastatur und Xbox Gamepad und Remote Steuerung

Xbox Gamepads und Remote Steuerelemente unterstützen viele UWP-Tastatur Verhalten und-Erfahrungen. Aufgrund fehlender verschiedener wichtiger Optionen, die auf einer Tastatur verfügbar sind, fehlen Gamepad und Remote Steuerung jedoch viele Tastatur Optimierungen (die Remote Steuerung ist noch stärker eingeschränkt als Gamepad).

Weitere Details zur UWP-Unterstützung für Gamepad und Remote Steuerung finden Sie unter [Gamepad-und Remote Steuerungs Interaktionen](gamepad-and-remote-interactions.md) .

Das folgende Beispiel zeigt einige Schlüssel Zuordnungen zwischen Tastatur, Gamepad und Remote Steuerung.

| **Tastatur**  | **Gamepad**                         | **Remotesteuerung**  |
|---------------|-------------------------------------|---------------------|
| LeerZchn         | A-Taste                            | Schaltfläche auswählen       |
| EINGABETASTE         | A-Taste                            | Schaltfläche auswählen       |
| Escape        | B-Taste                            | Zurück-Schaltfläche         |
| Start/Ende      | –                                 | –                 |
| Seite nach oben/unten  | Schaltfläche "auslöst" für vertikalen Bildlauf, Schaltfläche "Stoß Leiste"   | –                 |

Einige wichtige Unterschiede, die Sie beim Entwerfen der UWP-App für die Verwendung mit Gamepad und der Verwendung der Remote Steuerung berücksichtigen sollten, sind:
-   Der Text Eintrag erfordert, dass der Benutzer ein-Steuerelement zum Aktivieren eines Text Steuer Elements drückt.
-   Die Fokus Navigation ist nicht auf Steuerungsgruppen beschränkt, Benutzer können in der APP frei zu einem beliebigen Fokus baren UI-Element navigieren.

    **Hinweis** Der Fokus kann auf ein beliebiges Benutzeroberflächen Element in der Tastenkombination verschoben werden, es sei denn, es befindet sich in einer über Lagerungs Benutzeroberfläche, oder es wird eine [Fokus Bindung](gamepad-and-remote-interactions.md#focus-engagement) festgelegt. Dadurch wird verhindert, dass ein Bereich in den Fokus wechselt bzw. beendet wird. Weitere Informationen finden Sie im Abschnitt " [direktionale Navigation](#directional-navigation) ".
-   Die Schaltflächen "D-Pad" und "Linker Stick" werden verwendet, um den Fokus zwischen Steuerelementen und für die innere Navigation

    **Hinweis** Gamepad und Remote Steuerung Navigieren nur zu Elementen, die sich in derselben visuellen Reihenfolge wie der angedrückte direktionale Schlüssel befinden. Die Navigation ist in dieser Richtung deaktiviert, wenn kein nachfolgendes Element vorhanden ist, das den Fokus erhalten kann. Abhängig von der Situation haben Tastatur Benutzer nicht immer diese Einschränkung. Weitere Informationen finden Sie im Abschnitt [integrierte Tastatur Optimierung](#built-in-keyboard-optimization) .

#### <a name="directional-navigation"></a>Direktionale Navigation

Die direktionale Navigation wird durch eine UWP [Focus Manager](/uwp/api/Windows.UI.Xaml.Input.FocusManager) -Hilfsklasse verwaltet, die den gerichteten Tastatur Schlüssel (Pfeiltaste, D-Pad) übernimmt und versucht, den Fokus in der entsprechenden visuellen Richtung zu verschieben.

Anders als bei der Tastatur wird eine direktionale Navigation in der gesamten Anwendung für Gamepad und Remote Steuerung angewendet, wenn eine APP aus dem [Maus Modus wechselt](gamepad-and-remote-interactions.md#mouse-mode). Ausführlichere Informationen zur direktionalen Navigations Optimierung finden Sie unter [Gamepad-und Remote Steuerungs Interaktionen](gamepad-and-remote-interactions.md) .

**Hinweis** Die Navigation mit der Tastatur Tab-Taste wird nicht als direktionale Navigation betrachtet. Weitere Informationen finden Sie im Abschnitt [Tabstopps](#tab-stops) .

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Unterstützte Richtungs Navigation</strong></br>Mithilfe von Richtungs Schlüsseln (Tastatur Pfeile, Gamepad und Remote Steuerung D-Pad) kann der Benutzer zwischen verschiedenen Steuerelementen navigieren.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Direktionale Navigation nicht unterstützt</strong> </br>Der Benutzer kann mit direktionalen Schlüsseln nicht zwischen verschiedenen Steuerelementen navigieren. Andere Methoden zum Navigieren zwischen Steuerelementen (Tab-Taste) sind nicht betroffen.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Integrierte Tastatur Optimierung

Abhängig vom verwendeten Layout und den verwendeten Steuerelementen können UWP-apps speziell für Tastatureingaben optimiert werden.

Das folgende Beispiel zeigt eine Gruppe von Listenelementen, Raster Elementen und Menü Elementen, die einem einzelnen Tabstopp zugewiesen wurden (siehe Abschnitt " [Tabstopps](#tab-stops) "). Wenn die Gruppe den Fokus besitzt, wird die innere Navigation mit den Richtungspfeil Schlüsseln in der entsprechenden visuellen Reihenfolge ausgeführt (siehe [Navigations](#navigation) Abschnitt).

![Navigation in einer einzelnen Spalten Pfeiltaste](images/keyboard/single-column-arrow.png)

***Navigation in einer einzelnen Spalten Pfeiltaste***

![Navigation in einer einzelnen Zeilen Pfeiltaste](images/keyboard/single-row-arrow.png)

***Navigation in einer einzelnen Zeilen Pfeiltaste***

![Navigation durch mehrere Spalten-und Zeilen Pfeiltasten](images/keyboard/multiple-column-and-row-navigation.png)

***Mehrere Spalten/Zeilen-Pfeiltasten Navigation***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Umwickeln von homogenen Listen-und Raster Ansichts Elementen

Die direktionale Navigation ist nicht immer die effizienteste Methode, um durch mehrere Zeilen und Spalten von List-und GridView-Elementen zu navigieren.

**Hinweis** Menü Elemente sind in der Regel einzelne Spalten Listen, aber in einigen Fällen können spezielle Fokus Regeln zutreffen (siehe [Popup-UI](#popup-ui)).

Listen-und Raster Objekte können mit mehreren Zeilen und Spalten erstellt werden. Diese sind in der Regel in Zeilen-Major (wobei Elemente vollständige Zeilen zuerst vor dem Ausfüllen der nächsten Zeile ausfüllen) oder Spalte-Major (wobei Elemente zuerst die gesamte Spalte ausfüllen, bevor Sie die nächste Spalte ausfüllen). Die Reihenfolge der Zeilen oder Spalten hängt von der Scrollrichtung ab, und Sie sollten sicherstellen, dass die Reihenfolge der Elemente nicht in Konflikt mit dieser Richtung

In Zeilen-Haupt Reihenfolge (in der Elemente von links nach rechts, von oben nach unten), wenn sich der Fokus auf dem letzten Element in einer Zeile befindet und die nach-rechts-Taste gedrückt wird, wird der Fokus auf das erste Element in der nächsten Zeile verschoben. Das gleiche Verhalten tritt in umgekehrter Reihenfolge auf: Wenn der Fokus auf das erste Element in einer Zeile festgelegt ist und die nach-links-Taste gedrückt wird, wird der Fokus zum letzten Element in der vorherigen Zeile verschoben.

In der Spalte-Haupt Reihenfolge (in der Elemente von oben nach unten, von links nach rechts), wenn sich der Fokus auf dem letzten Element in einer Spalte befindet und der Benutzer die nach-unten-Taste drückt, wird der Fokus auf das erste Element in der nächsten Spalte verschoben. Das gleiche Verhalten tritt in umgekehrter Reihenfolge auf: Wenn der Fokus auf das erste Element in einer Spalte festgelegt ist und die nach-oben-Taste gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Spalte verschoben.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Zeilen Haupttastatur Navigation</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Spalten Haupttastatur Navigation</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Popup-Benutzeroberfläche

Wie bereits erwähnt, sollten Sie sicherstellen, dass die direktionale Navigation der visuellen Reihenfolge der Steuerelemente in der Benutzeroberfläche Ihrer Anwendung entspricht.

Einige Steuerelemente (z. b. das Kontextmenü, das Befehls leisten-Überlauf Menü und das Menü "AutoVorschlagen") zeigen in Bezug auf das primäre Steuerelement und den verfügbaren Bildschirmbereich ein Menü-Popup in einer Position und Richtung an (standardmäßig abwärts). Beachten Sie, dass die öffnende Richtung von einer Vielzahl von Faktoren zur Laufzeit betroffen sein kann.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

Wenn für diese Steuerelemente das Menü zum ersten Mal geöffnet wird (und kein Element vom Benutzer ausgewählt wurde), legt die nach-unten-Taste den Fokus immer auf das erste Element fest, während die nach-oben-Taste den Fokus immer auf das letzte Element im Menü festlegt. 

Wenn das letzte Element den Fokus besitzt und die nach-unten-Taste gedrückt wird, wechselt der Fokus zum ersten Element im Menü. Wenn das erste Element den Fokus besitzt und die nach-oben-Taste gedrückt wird, wechselt der Fokus zum letzten Element im Menü. Dieses Verhalten wird als " *Radfahren* " bezeichnet und eignet sich zum Navigieren in Popup Menüs, die in unvorhersehbaren Richtungen geöffnet werden können.

> [!NOTE]
> Das Radfahren sollte in nicht-Popup-UIs vermieden werden, in denen Benutzer sich in einer Endlosschleife befinden könnten. 

Es wird empfohlen, dass Sie diese Verhaltensweisen in Ihren benutzerdefinierten Steuerelementen emulieren. Ein Code Beispiel zur Implementierung dieses Verhaltens finden Sie in der [programmatischen Fokus-Navigations](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) Dokumentation.


## <a name="test-your-app"></a>Testen Ihrer App

Testen Sie Ihre APP mit allen unterstützten Eingabegeräten, um sicherzustellen, dass Benutzeroberflächen Elemente auf eine kohärente und intuitive Weise navigiert werden können und dass keine unerwarteten Elemente die gewünschte Aktivier Reihenfolge beeinträchtigen.

## <a name="related-articles"></a>Verwandte Artikel
* [Tastaturereignisse](keyboard-events.md)
* [Identifizieren von Eingabegeräten](identify-input-devices.md)
* [Reagieren auf das Vorhandensein der Bildschirmtastatur](respond-to-the-presence-of-the-touch-keyboard.md)
* [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
* [Storyboarding-Besonderheiten für navigationview-Steuerelement](../controls-and-patterns/navigationview.md#hierarchical-navigation) 

## <a name="appendix"></a>Anhang

### <a name="software-keyboard"></a>Software Tastatur

Software Tastatur ist eine Tastatur, die auf dem Bildschirm angezeigt wird, den der Benutzer anstelle der physischen Tastatur zum eingeben und eingeben von Daten mithilfe von Finger Eingaben, Maus Eingaben, Stift/Tablettstift oder anderen Zeige Geräten verwenden kann (ein Touchscreen ist nicht erforderlich). Auf dem Touchscreen können diese Tastaturen direkt zum Eingeben von Text berührt werden. Auf Xbox One-Geräten müssen einzelne Schlüssel ausgewählt werden, indem das Fokus visuelle Element verschoben oder Tastenkombinationen mithilfe von Gamepad oder der Remote Steuerung verwendet werden.

![Windows 10-Fingereingabe Tastatur](images/keyboard/default.png)

***Windows 10-Fingereingabe Tastatur***

![Xbox One-Bildschirmtastatur](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One-Bildschirmtastatur***

Abhängig vom Gerät wird die Software Tastatur angezeigt, wenn ein Textfeld oder ein anderes bearbeitbares Text Steuerelement den Fokus erhält oder wenn der Benutzer es manuell über das **Benachrichtigungs Center**aktiviert:

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

***Die Touchscreen-Tastatur im standardlayoutmodus***

![Touch-Bildschirmtastatur mit erweitertem Layout](images/keyboard/extendedview.png)

***Die Touchscreen-Tastatur im erweiterten Layoutmodus***

Erfolgreiche Tastaturinteraktionen ermöglichen es Benutzern, einfache App-Szenarien nur über die Tastatur auszuführen; Benutzer können demnach über die Tastatur alle interaktiven Elemente erreichen und Standardfunktionen aktivieren. Eine Reihe von Faktoren kann den Erfolg beeinflussen, z. B. Tastaturnavigation, Tastenkombinationen für die Barrierefreiheit sowie Tastenkombinationen für erfahrene Benutzer.

**Hinweis**    Die Fingereingabe Tastatur unterstützt die UMSCHALTTASTE und die meisten Systembefehle nicht.

#### <a name="on-screen-keyboard"></a>Bildschirmtastatur
Wie bei der Software Tastatur ist die Bildschirmtastatur eine visuelle Software Tastatur, die Sie anstelle der physischen Tastatur verwenden können, um Daten mithilfe von Toucheingabe, Maus, Stift/Tablettstift oder einem anderen Zeigegerät einzugeben und einzugeben (ein Touchscreen ist nicht erforderlich). Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Die Bildschirmtastatur kann auf der Seite „Tastatur“ unter „Einstellungen &gt; Erleichterte Bedienung“ aktiviert werden.

**Hinweis** Die Bildschirmtastatur hat Vorrang vor der touchtastatur, die nicht angezeigt wird, wenn die Bildschirmtastatur vorhanden ist.

![Bildschirmtastatur](images/keyboard/osk.png)

***Bildschirmtastatur***

Weitere Informationen zur Bildschirmtastatur finden Sie [auf der Bildschirmtastatur](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) .

## <a name="related-articles"></a>Verwandte Artikel

- [Tastatureingabe](../accessibility/keyboard-accessibility.md)
