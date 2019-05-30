---
Description: Testverfahren, mit denen Sie sicherstellen können, dass Ihre App für die universelle Windows-Plattform (UWP) barrierefrei ist.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Barrierefreiheitstests
label: Accessibility testing
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8af03b32453bcdacb3da95678cf23a988c375f1b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359641"
---
# <a name="accessibility-testing"></a>Barrierefreiheitstests  

Testverfahren, mit denen Sie sicherstellen können, dass Ihre App für die universelle Windows-Plattform (UWP) barrierefrei ist.

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>

## <a name="run-accessibility-testing-tools"></a>Ausführen der Tools zum Testen der Barrierefreiheit  
Das Windows Software Development Kit (SDK) enthält verschiedene Tools zum Testen der Barrierefreiheit, z. B. [**EH-Viewer**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope), [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) und [**UI-Barrierefreiheitsprüfung**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker). Mit diesen Tools können Sie die Barrierefreiheit Ihrer App überprüfen. Achten Sie darauf, dass Sie sämtliche App-Szenarien und UI-Elemente testen.

Sie können die Tools zum Testen der Barrierefreiheit entweder über die Eingabeaufforderung von Microsoft Visual Studio oder über den Tools-Ordner des Windows SDK öffnen (bin-Unterverzeichnis des Ordners, in dem das Windows SDK auf dem Entwicklungscomputer installiert ist).
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>

### <a name="accscope"></a>**EH-Viewer (AccScope)**  

Mit dem Tool [**EH-Viewer**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) können Entwickler und Tester die Barrierefreiheit der App während ihrer Entwicklungs- und Entwurfsphase auswerten. Dies wird meist in frühen Prototypphasen und nicht in den letzten Testphasen des Entwicklungszyklus einer App durchgeführt. Das Hauptziel besteht im Testen von Barrierefreiheitsszenarien in Verbindung mit der Sprachausgabe für die App.

<span id="inspect"/>
<span id="INSPECT"/>

### <a name="inspect"></a>**Prüfen**  

[**Überprüfen Sie** ](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) ermöglicht es Ihnen, wählen ein UI-Element, und zeigen die barrierefreiheitsdaten. Sie können Eigenschaften und Steuerelementmuster der Microsoft-Benutzeroberflächenautomatisierung anzeigen und die Navigationsstruktur der Automatisierungselemente im Benutzeroberflächenautomatisierungs-Baum testen. Verwenden Sie beim Entwickeln der Benutzeroberfläche **Inspect**, um zu überprüfen, wie die Barrierefreiheitsattribute in der Benutzeroberflächenautomatisierung verfügbar gemacht werden. In einigen Fällen stammen die Attribute aus der Unterstützung der Benutzeroberflächenautomatisierung, die für Standard-XAML-Steuerelemente bereits implementiert wurde. In anderen Fällen stammen die Attribute aus bestimmten Werten, die Sie im XAML-Markup als an [**AutomationProperties**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties) angefügte Eigenschaften festgelegt haben.

Die folgende Abbildung zeigt das [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects)-Tool, mit dem die Benutzeroberflächenautomatisierungseigenschaften des Menübefehls **Bearbeiten** in Editor abgefragt werden.

![Screenshot des Inspect-Tools.](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>

### <a name="ui-accessibility-checker"></a>**UI Accessibility Checker**  
Mithilfe der **UI-Barrierefreiheitsprüfung (AccChecker)** können Sie Probleme hinsichtlich der Barrierefreiheit zur Laufzeit erkennen. Wenn die Benutzeroberfläche vollständig und funktionsfähig ist, können Sie **AccChecker** verwenden, um verschiedene Szenarien zu testen, die Richtigkeit von Laufzeit-Barrierefreiheitsinformationen zu überprüfen und Laufzeitprobleme zu ermitteln. Sie können **AccChecker** im Benutzeroberflächen- oder Befehlszeilenmodus ausführen. Öffnen Sie zum Ausführen des Tools für den Benutzeroberflächenmodus das Verzeichnis **AccChecker** im bin-Verzeichnis des Windows SDK, führen Sie „acccheckui.exe“ aus, und klicken Sie auf das Menü **Hilfe**.

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>

### <a name="ui-automation-verify"></a>**Benutzeroberflächenautomatisierungs-Überprüfung**  
Die **Benutzeroberflächenautomatisierungs-Überprüfung (UIA Verify)** ist ein automatisiertes Test- und Überprüfungsframework für Implementierungen der Benutzeroberflächenautomatisierung. **UIA Verify** kann in den Testcode integriert werden. Somit können regelmäßige automatisierte Tests oder Stichprobenkontrollen der Benutzeroberflächenautomatisierungs-Szenarien ausgeführt werden. Um **UIA Verify** auszuführen, führen Sie „VisualUIAVerifyNative.exe“ aus dem Unterverzeichnis „UIAVerify“ aus.

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>

### <a name="accessible-event-watcher"></a>**Überwachung für barrierefreie Ereignisse**  
[**Zugänglich-Ereignisüberwachung (AccEvent)** ](https://docs.microsoft.com/windows/desktop/WinAuto/accessible-event-watcher) testet, ob die Elemente der Benutzeroberfläche der app ausgelöst, entsprechenden-Benutzeroberflächenautomatisierung und Microsoft Active Accessibility-Ereignisse beim Auftreten von Änderungen an der Benutzeroberfläche werden. Änderungen an der Benutzeroberfläche können auftreten, wenn sich der Fokus ändert, ein Benutzeroberflächenelement aufgerufen oder ausgewählt wird oder sich sein Zustand oder seine Eigenschaft ändert.

> [!NOTE]
> Die meisten in dieser Dokumentation erwähnten Tools zum Testen der Barrierefreiheit können auf einem PC, aber nicht auf einem Mobiltelefon ausgeführt werden. Sie können einige dieser Tools ausführen, während Sie entwickeln und einen Emulator verwenden, aber die meisten dieser Tools können den Benutzeroberflächenautomatisierungs-Baum im Emulator nicht bereitstellen.

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>

## <a name="test-keyboard-accessibility"></a>Testen der Barrierefreiheit der Tastaturnavigation  
Die beste Methode, um die Barrierefreiheit der Tastaturnavigation zu testen, besteht darin, die Maus auszustöpseln oder im Fall eines Tabletgeräts die Bildschirmtastatur zu verwenden. Testen Sie die Barrierefreiheit der Tastaturnavigation mithilfe der _TAB_-TASTE. Sie sollten alle interaktiven UI-Elemente mit der _TAB_-TASTE erreichen können. Überprüfen Sie bei zusammengesetzten UI-Elementen, ob Sie mit den Pfeiltasten zwischen den einzelnen Elementteilen navigieren können. Sie sollten beispielsweise mithilfe der Tasten auf der Tastatur in Listen mit Elementen navigieren können. Stellen Sie zum Schluss sicher, dass Sie alle interaktiven UI-Elemente mithilfe der Tastatur aufrufen können, wenn der Fokus auf diesen Elementen liegt (in der Regel mit der EINGABETASTE oder LEERTASTE).

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>

## <a name="verify-the-contrast-ratio-of-visible-text"></a>Überprüfen des Kontrastverhältnisses von sichtbarem Text  
Überprüfen Sie mit den Farbkontrasttools, ob das Kontrastverhältnis von sichtbarem Text in Ordnung ist. Zu den Ausnahmen zählen inaktive UI-Elemente und Logos oder dekorativer Text, der keine Informationen enthält und neu angeordnet werden kann, ohne dadurch seine Bedeutung zu verändern. Weitere Informationen zum Kontrastverhältnis sowie zu Ausnahmen finden Sie unter [Anforderungen für barrierefreien Text](accessible-text-requirements.md). Tools zum Testen des Kontrastverhältnisses finden Sie unter [Techniken für WCAG 2.0 G18 (Abschnitt Ressourcen)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Einige der unter den Techniken für WCAG 2.0 G18 aufgeführten Tools können bei einer UWP-App nicht interaktiv verwendet werden. Möglicherweise müssen Sie die Farbwerte für den Vorder- und Hintergrund im Tool manuell eingeben, Bildschirmaufnahmen der App-Benutzeroberfläche anfertigen und dann das Kontrastverhältnistool für die Bildschirmaufnahme ausführen. Es kann auch erforderlich sein, dass Sie das Tool beim Öffnen der Quellbitmapdateien in einem Bildbearbeitungsprogramm ausführen, und nicht während des Ladens eines Bilds durch die App.

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>

## <a name="verify-your-app-in-high-contrast"></a>Überprüfen der App mit hohem Kontrast  
Verwenden Sie die App mit einem Design mit hohem Kontrast, um sicherzustellen, dass alle UI-Elemente korrekt angezeigt werden. Der gesamte Text muss lesbar sein, und alle Bilder müssen deutlich zu erkennen sein. Passen Sie die XAML-Designverzeichnisressourcen oder Steuerelementvorlagen an, um durch Steuerelemente verursachte Designprobleme zu beheben. Wenn schwerwiegende Probleme mit hohem Kontrast nicht durch Designs oder Steuerelemente (z. B. durch Bilddateien) verursacht werden, stellen Sie separate Versionen bereit, die verwendet werden, wenn ein Design mit hohem Kontrast aktiv ist.

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>

## <a name="verify-your-app-with-display-settings"></a>Überprüfen der App mit Anzeigeeinstellungen  

Verwenden Sie die Systemanzeigeoptionen, die den DPI-Wert der Anzeige anpassen, und stellen Sie sicher, dass Ihre App-UI bei einer Änderung des DPI-Werts richtig skaliert wird. (Einige Benutzer ändern die dpi-Werte als Zugriffsoption auf diese Klasse steht in **erleichterte Bedienung** auch als Eigenschaften anzuzeigen.) Wenn Sie auf Probleme stoßen, führen Sie die [Richtlinien zum Skalieren Layouts](https://developer.microsoft.com/windows/design) und geben Sie zusätzliche Ressourcen für andere Skalierungsfaktoren.

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>

## <a name="verify-main-app-scenarios-by-using-narrator"></a>Überprüfen der wichtigsten App-Szenarien mit der Sprachausgabe  
Verwenden Sie die Sprachausgabe, um die Qualität der Bildschirmtexterkennung für Ihre App einzuschätzen.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**Verwenden Sie diese Schritte zum Testen Ihrer app, einer Tastatur und Maus mit Sprachausgabe:**
1.  Starten Sie die Sprachausgabe, indem Sie _Windows-Logo-Taste+STRG+EINGABETASTE_ drücken. In Versionen vor der Version 1607 von Windows 10 verwenden Sie _Windows-Logo-Taste + EINGABETASTE_ zum Starten der Sprachausgabe.
2.  Bedienen Sie Ihre App über die Tastatur mit der _TAB_-TASTE, den PFEILTASTEN und _FESTSTELLTASTE+PFEILTASTEN_.
3.  Hören Sie sich an, wie die Sprachausgabe Elemente der Benutzeroberfläche vorliest, während Sie die App bedienen, und achten Sie auf Folgendes:
    * Achten Sie bei allen Steuerelementen darauf, dass die Sprachausgabe korrekt für alle sichtbaren Inhalte erfolgt. Stellen Sie zudem sicher, dass die Sprachausgabe jeweils den Namen, alle relevanten Zustände (aktiviert, ausgewählt usw.) und den Typ des Steuerelements (Schaltfläche, Kontrollkästchen, Listenelement usw.) vorliest.
    * Überprüfen Sie bei einem interaktiven Element, ob Sie dessen Aktion per Sprachausgabe aufrufen können, indem Sie _FESTSTELL+EINGABE_ drücken.
    * Achten Sie bei Tabellen darauf, dass die Sprachausgabe den Tabellennamen, die Tabellenbeschreibung (falls verfügbar) und die Zeilen- und Spaltenüberschriften richtig vorliest.

4.  Drücken Sie _FESTSTELL+UMSCHALT+EINGABE_, um Ihre App zu durchsuchen und zu überprüfen, ob alle Steuerelemente in der Suchliste erscheinen und ob die Namen der Steuerelemente lokalisiert und lesbar sind.
5.  Schalten Sie den Monitor aus und versuchen Sie, die Hauptszenarien für Ihre App durchzuspielen, indem Sie nur Tastatur und Maus verwenden. Eine umfassende Liste aller Befehle und Tastenkombinationen erhalten Sie, indem Sie _FESTSTELL+F1_ drücken.

Ab Windows 10, Version 1607, wurde in der Sprachausgabe ein neuer Entwicklermodus eingeführt. Aktivieren Sie den Entwicklermodus, wenn die Sprachausgabe bereits ausgeführt wird, indem Sie _FESTSTELL+UMSCHALT+F12_ drücken. Bei aktiviertem Entwicklermodus wird der Bildschirm maskiert, und es werden nur die zugänglichen Objekte und der dazugehörige Text hervorgehoben, der für die Sprachausgabe programmgesteuert verfügbar gemacht wird. So erhalten Sie eine gute visuelle Darstellung der Informationen, die für die Sprachausgabe verfügbar sind.

**Verwenden Sie diese Schritte, um Ihrer app mithilfe des Microsoft-Sprachausgabe fingereingabemodus zu testen:**

> [!NOTE]
> Die Sprachausgabe wechselt auf Geräten, die mehr als 4 Kontakte unterstützen, automatisch in den Toucheingabemodus. Die Sprachausgabe unterstützt keine Szenarien mit mehreren Monitoren oder Digitalisierungsgeräte mit Multitouch auf dem Hauptbildschirm.

1.  Machen Sie sich mit der Benutzeroberfläche vertraut und nehmen Sie das Layout in Augenschein.

    * **Navigieren Sie über die Benutzeroberfläche mit einem Finger wischbewegungen.** Wischen Sie nach links oder rechts, um zwischen Elementen hin und her zu wechseln, und nach oben und unten, um die Kategorie der zu durchsuchenden Elemente zu ändern. Zu den Kategorien gehören alle Elemente, Links, Tabellen, Überschriften usw. Das Navigieren per Wischen mit einem Finger gestaltet sich so ähnlich wie das Navigieren mithilfe von _FESTSTELL+PFEILTASTE_.
    * **Verwenden Sie Registerkarte zum Navigieren durch die Elemente der Fokus erhalten kann.** Wenn Sie mit drei Fingern nach rechts oder links wischen, hat dies denselben Effekt wie das Drücken von _TAB_ und _UMSCHALT+TAB_ auf einer Tastatur.
    * **Untersuchen Sie die Benutzeroberfläche mit einem einzelnen Finger räumlich.** Führen Sie einen Finger nach oben und unten oder links und rechts, um sich von der Sprachausgabe die Elemente unter Ihrem Finger vorlesen zu lassen. Alternativ können Sie auch eine Maus dazu nutzen, da bei ihr dieselbe Zugriffstestlogik zum Einsatz kommt wie beim Ziehen Ihres Fingers.
    * **Lassen Sie sich das ganze Fenster und alle Inhalte vorlesen, indem Sie mit drei Fingern eine Wischbewegung nach oben durchführen**. Diese Bewegung entspricht der Tastenkombination _FESTSTELL+W_.

    Sollten wichtige Elemente der UI nicht erreichbar sein, liegt vielleicht ein Fehler bei der Barrierefreiheit vor.

2.  Interagieren Sie mit Steuerelementen, um deren primäre und sekundäre Aktionen und Bildlaufverhalten zu testen.

    Primäre Aktionen sind zum Beispiel die Aktivierung einer Schaltfläche, das Einfügen eines Caretzeichens und das Legen des Fokus auf das Steuerelement. Unter sekundäre Aktionen fallen z. B. Aktionen wie die Auswahl eines Listenelements oder das Erweitern einer Schaltfläche, die mehrere Optionen anbietet.

    * So testen Sie eine primäre Aktion: Double Tippen Sie, oder drücken Sie die mit einem Finger, und tippen Sie auf, mit einem anderen.
    * So testen Sie eine sekundäre Aktion: Tripleresolutionimage-tippen oder mit einem Finger und Doppeltippen mit einem anderen drücken.
    * So testen Sie das Scrollverhalten: Verwenden Sie zwei-Finger-Kundenkarte, um in die gewünschte Richtung zu scrollen.

    Einige Steuerelemente bieten zusätzliche Aktionen. Rufen Sie eine vollständige Liste auf, indem Sie mit vier Fingern einmal tippen.

    Sollte ein Steuerelement auf Maus- und Tastatureingaben, jedoch nicht auf eine primäre oder sekundäre Berührungsinteraktion reagieren, müssen für das Steuerelement unter Umständen zusätzliche Steuerungsmuster zur [Benutzeroberflächenautomatisierung](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiauto-win32) implementiert werden.

Erwägen Sie auch die Nutzung des [**EH-Viewer**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope)-Tools (EH-Viewer) zum Testen von Barrierefreiheitsszenarien in Verbindung mit der Sprachausgabe für die App. Im [**Thema zum EH-Viewer-Tool**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) wird erläutert, wie **EH-Viewer** für das Testen von Szenarien in Verbindung mit der Sprachausgabe konfiguriert wird.

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>

## <a name="examine-the-ui-automation-representation-for-your-app"></a>Untersuchen der Darstellung der Benutzeroberflächenautomatisierung für Ihre App  
Einige der bereits erwähnten Tools zum Testen der Benutzeroberflächenautomatisierung ermöglichen eine Anzeige Ihrer App, bei der absichtlich nicht berücksichtigt wird, wie die App aussieht. Stattdessen wird die App als Struktur aus Benutzeroberflächenautomatisierungs-Elementen dargestellt. Mit diesem Verfahren interagieren Benutzeroberflächenautomatisierungs-Clients, vor allem Hilfstechnologien, mit der App in Barrierefreiheitsszenarien.

Das Tool [**EH-Viewer**](https://docs.microsoft.com/windows/desktop/WinAuto/accscope) ermöglicht eine besonders interessante Anzeige der App, da Sie die Benutzeroberflächenautomatisierungs-Elemente entweder als visuelle Darstellung oder als Liste anzeigen können. Wenn Sie die Visualisierung verwenden, können Sie einen Drilldown zu den einzelnen Bestandteilen so durchführen, dass eine Korrelation mit der visuellen Darstellung der App-UI möglich ist. Sie können sogar die Barrierefreiheit Ihrer frühesten UI-Prototypen testen, bevor Sie der UI die gesamte Logik zugewiesen haben. So stellen Sie sicher, dass sich die visuelle Interaktion und die Navigation für Barrierefreiheitsszenarien der App im Gleichgewicht befinden.

Außerdem können Sie testen, ob in der Elementansicht der Benutzeroberflächenautomatisierung Elemente angezeigt werden, die dort nicht erscheinen sollen. Falls Sie Elemente finden, die aus der Ansicht entfernt werden sollen, oder falls Elemente fehlen, können Sie mithilfe der angefügten XAML-Eigenschaft [**AutomationProperties.AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) anpassen, wie XAML-Steuerelemente in Barrierefreiheitsansichten erscheinen. Nachdem Sie sich die grundlegenden Barrierefreiheitsansichten angesehen haben, ist dies ein guter Zeitpunkt für die erneute Überprüfung der Registerkartensequenzen oder der räumlichen Navigation mithilfe von Pfeiltasten. So können Sie sich vergewissern, dass Benutzer Zugang zu allen Teilbereichen haben, die interaktiv sind und in der Steuerungsansicht verfügbar gemacht werden.

<span id="related_topics"/>

## <a name="related-topics"></a>Verwandte Themen  
* [Bedienungshilfen](accessibility.md)
* [Nicht empfehlenswerte Methoden](practices-to-avoid.md)
* [Benutzeroberflächenautomatisierung](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiauto-win32)
* [Barrierefreiheit in Windows](https://go.microsoft.com/fwlink/p/?LinkId=320802)
* [Erste Schritte mit der Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
