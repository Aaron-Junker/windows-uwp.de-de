---
title: Voice aktiviert Shell (VES) auf der Xbox
description: Erfahren Sie, wie Ihre UWP-apps auf der Xbox Voice Unterstützung hinzugefügt.
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, Uwp, Xbox, Spracherkennung und Sprache aktiviert-shell
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596055"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Verwendung der Spracherkennung zum Aufrufen des UI-Elemente

Voice aktiviert Shell (VES) ist eine Erweiterung für Windows Sprachplattform, mit dem Sie eine erstklassige Sprache-Umgebung in apps, können Benutzer zum Sprache aufrufen auf dem Bildschirm Steuerelemente und zum Einfügen von Text über Diktat. VES zielt darauf ab, um eine gemeinsame End-to-End finden Sie unter It-Say-It-Erfahrung auf allen Windows-Shells und Geräten, mit minimalem Aufwand, die vom app-Entwickler bereitzustellen.  Um dies zu erreichen, nutzt es Microsoft Speech-Plattform und der Benutzeroberflächenautomatisierung (UIA)-Framework.

## <a name="user-experience-walkthrough"></a>Exemplarische Vorgehensweise für Benutzer ##
Im folgenden einen Überblick darüber, was ein Benutzer bei der Verwendung von VES auf der Xbox auftreten würden, und sie sollten zum legen Sie den Kontext vor dem Einstieg in die Details der Funktionsweise von VES.

- Benutzer auf der Xbox-Konsole aktiviert und möchte, durchsuchen ihre apps auch etwas von Interesse sind:

        User: "Hey Cortana, open My Games and Apps"

- Benutzer bleibt in aktiv überwacht Modus (ALM), was bedeutet, dass die Konsole jetzt eine Überwachung für den Benutzer ein Steuerelement aufzurufen, die auf dem Bildschirm sichtbar ist, ohne zu sagen, "Hey Cortana" jedes Mal.  Benutzer kann jetzt zum Anzeigen von apps und scrollen Sie durch die app-Liste wechseln:

        User: "applications"

- Um die Ansicht zu scrollen, kann Benutzer einfach sagen:

        User: "scroll down"

- Benutzer wird die werbegrafik, für die app, die sie interessiert sind, aber vergessen den Namen an.  Benutzer fragt nach Voice Tipp Bezeichnungen angezeigt werden:

        User: "show labels"

- Nun, da sie was Sie sagen ist, kann die app gestartet werden:

        User: "movies and TV"

- Zum Beenden der aktiven Empfangsmodus mitteilt Benutzer, Xbox, deren Überwachung beendet:

        User: "stop listening"

- Eine neue active lauschende Sitzung kann später mit gestartet werden:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI-Automatisierung-Abhängigkeit ##
VES ist ein Benutzeroberflächenautomatisierungs-Client und basiert auf Informationen, die von der app über die Benutzeroberflächenautomatisierungs-Anbietern verfügbar gemacht werden. Dies ist die gleiche Infrastruktur, die bereits von der Sprachausgabe-Funktion auf Windows-Plattformen verwendet wird.  Automatisierung der Benutzeroberfläche ermöglicht den programmgesteuerten Zugriff auf Elemente der Benutzeroberfläche, einschließlich des Namens des Steuerelements, dessen Typ und welche implementiert Steuerelementmuster es.  Wenn die Benutzeroberfläche in der app geändert wird, wird VES UIA-Update-Ereignisse zu reagieren und erneut analysieren die aktualisierte Benutzeroberflächenautomatisierungs-Struktur, um alle verwertbare Elemente zu suchen, verwenden diese Informationen, um eine spracherkennungsgrammatik zu erstellen. 

Alle UWP-apps haben Zugriff auf die UI-Automatisierungsframework und verfügbar machen Informationen über die Benutzeroberfläche, die unabhängig von den Grafik-Framework, die sie bei (XAML, DirectX/Direct3D, Xamarin, usw.) erstellt werden.  In einigen Fällen, wie XAML ein Großteil der Schwerstarbeit erfolgt durch das Framework verringert erheblich den Arbeitsaufwand beim Microsoft-Sprachausgabe und VES zu unterstützen.

Weitere Informationen zur Automatisierung der Benutzeroberfläche finden Sie unter [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "Grundlagen der Benutzeroberflächenautomatisierung").

## <a name="control-invocation-name"></a>Aufrufs Steuerelementname ##
VES setzt die folgende Heuristik, um zu bestimmen, was einen Ausdruck, um bei der Spracherkennung wie der Name des Steuerelements zu registrieren (d.h. was der Benutzer muss zum Aufrufen des Steuerelements sprechen).  Dies ist auch der Ausdruck, der in der Voice-QuickInfo-Bezeichnung angezeigt wird.

Die Quelle des Namens in der Reihenfolge ihrer Priorität:

1. Wenn das Element verfügt über eine `LabeledBy` angefügte Eigenschaft VES verwenden die `AutomationProperties.Name` mit dieser textbezeichnung.
2. `AutomationProperties.Name` des-Elements.  In XAML, die der Textinhalt des Steuerelements verwendet werden, als der Standardwert für `AutomationProperties.Name`.
3. Wenn das Steuerelement ein ListItem oder eine Schaltfläche, VES sucht das erste untergeordnete Element mit einer gültigen `AutomationProperties.Name`.

## <a name="actionable-controls"></a>Aktionen erfordernde Steuerelemente ##
VES berücksichtigt ein Steuerelements Handlungsbedarf, wenn sie eine der folgenden Steuerelementmuster für Benutzeroberflächenautomatisierung implementiert:

- **"InvokePattern"** (z. b. Schaltfläche ") – Stellt Steuerelemente, die initiieren oder eine einzelne, eindeutige Aktion und Zustand nach der Aktivierung nicht beibehalten.

- **TogglePattern** (z. b. Das Kontrollkästchen) – stellt ein Steuerelement, das eine Reihe von Zuständen durchlaufen und verwaltet einen einmal festgelegten Zustand werden kann.

- **SelectionItemPattern** (eg. Kombinationsfeld) – stellt ein Steuerelement, das als Container für eine Auflistung von untergeordneten, auswählbaren Elementen dient.

- **ExpandCollapsePattern** (z. b. Kombinationsfeld) - Steuerelemente, die zum Anzeigen von Inhalt erweitert bzw. reduziert werden, um Inhalte auszublenden darstellt.

- **ScrollPattern** (eg. Die Liste) – Stellt Steuerelemente, die als scrollbare Container für eine Auflistung von untergeordneten Elementen dienen.

## <a name="scrollable-containers"></a>Scrollbare Container ##
Für bildlauffähigen Container, die den überwacht das ScrollPattern, VES für Voice unterstützen Befehle wie "einen Bildlauf nach links", "Bildlauf nach rechts" usw., und scrollen wird mit den entsprechenden Parametern aufgerufen werden, wenn der Benutzer einen der folgenden Befehle auslöst.  Scroll-Befehle werden anhand des Werts des eingefügt, die `HorizontalScrollPercent` und `VerticalScrollPercent` Eigenschaften.  Z. B. wenn `HorizontalScrollPercent` ist größer als 0, "einen Bildlauf nach links" hinzugefügt wird, wenn dieser Wert ist kleiner als 100 "Bildlauf nach rechts" hinzugefügt werden, und so weiter.

## <a name="narrator-overlap"></a>Überlappung der Sprachausgabe ##
Die Sprachausgabe-Anwendung ist auch ein Benutzeroberflächenautomatisierungs-Client und verwendet die `AutomationProperties.Name` Eigenschaft als eine der Quellen für den Text für das aktuell ausgewählte Benutzeroberflächenelement gelesen.  Um eine verbesserte Barrierefreiheit viele app benutzererfahrung haben Entwickler zum Überladen von neu sortiert die `Name` Eigenschaft mit dem lange beschreibenden Text mit dem Ziel der Bereitstellung Weitere Informationen und Kontext, wenn Sie von der Sprachausgabe gelesen.  Dies führt jedoch einen Konflikt zwischen den zwei Funktionen: VES benötigt kurze Ausdrücke, die übereinstimmen oder weitestgehend entsprechen den sichtbaren Text des Steuerelements bei Vorteile der Microsoft-Sprachausgabe aus länger, aussagekräftigeren Ausdrücken, um eine bessere Kontext anzugeben.

Um dies zu beheben, beginnend mit Windows 10 Creators Update, Microsoft-Sprachausgabe wurde aktualisiert außerdem den `AutomationProperties.HelpText` Eigenschaft.  Wenn diese Eigenschaft nicht leer ist, wird die Sprachausgabe seinen Inhalt zusätzlich zu sprechen `AutomationProperties.Name`.  Wenn `HelpText` ist leer, Sprachausgabe liest nur den Inhalt des Namens.  Dies ermöglicht mehr beschreibende Zeichenfolgen, bei Bedarf verwendet werden soll, jedoch behält einen kürzeren, Speech Recognition Anzeigenamen-Ausdruck in der `Name` Eigenschaft.

![](images/ves_narrator.jpg)

Weitere Informationen finden Sie unter [-Eigenschaften für die Unterstützung von Eingabehilfen in der Benutzeroberfläche](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "-Eigenschaften für die Unterstützung von Eingabehilfen in der Benutzeroberfläche").

## <a name="active-listening-mode-alm"></a>Aktiven Empfangsmodus (ALM) ##
### <a name="entering-alm"></a>Eingeben von ALM ###
Auf der Xbox lauscht VES nicht ständig die Spracheingabe.  Der Benutzer muss überwacht Aktivmodus explizit mit den Worten eingeben:

- "Hey Cortana", "Select", oder
- "Hey Cortana", "Erstellen eines Auswahl"

Es gibt mehrere andere Cortana-Befehle, die auch den Benutzer in aktiven Lauschen nach Abschluss wird z. B. "Hey Cortana, melden Sie sich" oder "Hey Cortana, wechseln Sie Home" zu verlassen. 

ALM eingeben, müssen die folgende Auswirkungen:

- In der oberen rechten Ecke, die dem Benutzer mitteilt, dass sie sagen können, was sie sehen, wird das Cortana-Overlay angezeigt.  Während der Benutzer erweitern, werden auch an diesem Speicherort Ausdruck-Fragmente, die von der Spracherkennung erkannt werden angezeigt.
- VES analysiert die UIA-Struktur, sucht nach allen umsetzbare-Steuerelementen, den Text in die spracherkennungsgrammatik registriert und startet eine kontinuierliche Lausch-Sitzung.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Beenden von ALM ###
Das System bleibt in ALM, die während der Benutzer mit der Benutzeroberfläche, über Sprachbefehle interagiert.  Es gibt zwei Möglichkeiten, um ALM zu beenden:

- Benutzer ausdrücklich darauf hinweist, "stop überwacht", oder
- Ein Timeout tritt auf, wenn es gibt keine positive allgemein anerkannt innerhalb von 17 Sekunden ALM eingeben oder seit der letzten positiven Erkennung

## <a name="invoking-controls"></a>Aufrufen von Steuerelementen ##
Im ALM, die der Benutzer interagieren kann, mit der Benutzeroberfläche, über Sprachbefehle.  Wenn die Benutzeroberfläche (mit Name-Eigenschaften, die mit dem Text angezeigt) ordnungsgemäß konfiguriert ist, sollte die Stimme für Aktionen mit, dass eine nahtlose und natürliche-Erlebnis.  Der Benutzer sollte sein nur sagen, was sie auf dem Bildschirm sehen können.

## <a name="overlay-ui-on-xbox"></a>Überlagern Sie die UI für Xbox ##
Der Name VES leitet für ein Steuerelement möglicherweise anders als die tatsächliche sichtbaren Text auf der Benutzeroberfläche.  Dies liegt möglicherweise aufgrund der `Name` Eigenschaft des Steuerelements oder der angefügte `LabeledBy` Element explizit in eine andere Zeichenfolge festgelegt.  Oder das Steuerelement keine GUI-Text, jedoch nur ein Symbol oder Bild-Element.

In diesen Fällen benötigen Benutzer eine Möglichkeit festzustellen, was muss gesagt werden, um z. B. ein Steuerelement aufzurufen.  Aus diesem Grund können einmal im aktiv überwacht, Voice-Tipps angezeigt werden davon die Rede "Beschriftungen anzeigen".  Dadurch Voice QuickInfo-Bezeichnungen auf jede umsetzbare Steuerelement angezeigt werden.

Es gibt ein Limit von 100 Bezeichnungen, so verfügt die Benutzeroberfläche der app besser umsetzbare Steuerelemente als 100 gibt es einige, die keine Stimme Tipp Bezeichnungen angezeigt werden.  Die Bezeichnungen ausgewählt werden in diesem Fall ist nicht deterministisch, wie sie die Struktur und die Zusammensetzung des die aktuelle UI abhängt erster in der UIA-Struktur aufgelistet.

Nach der Voice-QuickInfo-Bezeichnungen angezeigt werden, dass es gibt keinen Befehl zum Ausblenden, werden sie sichtbar bleiben, bis einer der folgenden Ereignisse auftreten:

- der Benutzer ruft ein Steuerelement
- Benutzer navigiert, Weg von die aktuelle Szene
- Benutzer angezeigt wird, "Lauschen" stop "
- Timeout des aktiven Empfangsmodus

## <a name="location-of-voice-tip-labels"></a>Position von Bezeichnungen für Voice-Tipp ##
Voice-QuickInfo-Bezeichnungen werden innerhalb des Steuerelements BoundingRectangle horizontal und vertikal zentriert werden.  Wenn Steuerelemente klein und eng gruppierten sind, können die Bezeichnungen überschneiden sich/werden von anderen Benutzern verborgen und VES versucht, mithilfe von Push übertragen diese Bezeichnungen voneinander entfernt sind, trennen Sie diese, und stellen Sie sicher, sie werden angezeigt.  Dies ist jedoch nicht garantiert, 100 % der Zeit funktioniert.  Bei eine sehr hart umkämpften Benutzeroberfläche wird es vermutlich führen einige Bezeichnungen, die von anderen verdeckt wird. Überprüfen Sie Ihre Benutzeroberfläche mit "Beschriftungen anzeigen" um sicherzustellen, dass ausreichend Speicherplatz für die Sichtbarkeit der Voice-Tipp vorhanden ist.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Kombinationsfelder ##
Wenn das Kombinationsfeld für jedes einzelne Element im Kombinationsfeld erweitert wird, ruft eine eigene Stimme QuickInfo-Bezeichnung, und diese werden häufig dazu auf vorhandene Steuerelemente hinter der Dropdown-Liste.  Vermeiden von einem überladen und verwirrend Muddle Bezeichnungen (, in denen werden elementbezeichnungen für Kombinationsfeld-Feld mit den Bezeichnungen der hinter das Kombinationsfeld-Steuerelemente und optional) Wenn das Kombinationsfeld nur die Bezeichnungen erweitert wird, für dessen untergeordnete Elemente angezeigt werden;  Alle anderen Voice QuickInfo-Bezeichnungen werden ausgeblendet.  Der Benutzer kann dann wählen Sie eine der Dropdown-Elemente oder "Schließen" im Kombinationsfeld.

- Bezeichnungen in reduzierten Kombinationsfelder angezeigt:

    ![](images/ves_combo_closed.png)

- Bezeichnungen in erweiterten Kombinationsfeld:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Bildlauffähigen Steuerelementen ##
Für bildlauffähigen Steuerelementen werden die Sprach-Tipps für die Bildlauf-Befehle für jeden der Ränder des Steuerelements zentriert werden soll.  Voice-Tipps werden nur angezeigt werden, für den Bildlauf-Anweisungen, die eine Aktion, z. B. wenn ein vertikaler Bildlauf nicht verfügbar ist, "einen Bildlauf nach oben" und "einen Bildlauf nach unten" wird nicht angezeigt wird.  Wenn mehrere der bildlauffähigen Bereiche vorhanden sind verwendet VES Ordnungszahlen zur Unterscheidung zwischen ihnen (z. b. "Führen Sie einen Bildlauf rechten 1", "Scrollen rechten 2" usw.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Mehrdeutigkeitsvermeidung ##
Wenn mehrere UI-Elemente den gleichen Namen aufweisen, oder die Spracherkennung, mehrere Kandidaten abgeglichen, wird die VES zur mehrdeutigkeitsvermeidung Modus versetzt.  In diesem Modus Voice-Tipp werden Bezeichnungen angezeigt werden für die Elemente beteiligt, damit der Benutzer das richtige Abonnement auswählen kann. Der Benutzer kann aus dem zur mehrdeutigkeitsvermeidung Modus abbrechen, indem Sie sagen, "Abbrechen".

Zum Beispiel:

- Im aktiven überwacht Modus, bevor Sie zur mehrdeutigkeitsvermeidung; Benutzer sagt, "Bin ich nicht eindeutig":

    ![](images/ves_disambig1.png) 

- Beide Schaltflächen übereinstimmen; zur Klärung Schritte:

    ![](images/ves_disambig2.png) 

- Mit Aktion klicken Sie auf, wenn "2 auswählen" ausgewählt wurde:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Beispiel-UI ##
Hier ist ein Beispiel für eine XAML-basierte Benutzeroberfläche, AutomationProperties.Name auf verschiedene Weise festlegen:

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


Verwenden das obige Beispiel hier ist wie die Benutzeroberfläche mit und ohne Voice Tipp Bezeichnungen dargestellt wird.
 
- Im aktiven überwacht Modus ohne Bezeichnung angezeigt:

    ![](images/ves_alm_nolabels.png) 

- Im aktiven überwacht Modus wird nach dem Benutzer, "Bezeichnungen anzeigen", sagt:

    ![](images/ves_alm_labels.png) 

Im Fall von `button1`, füllt XAML automatisch die `AutomationProperties.Name` Eigenschaft mithilfe von Text aus dem sichtbaren Text-Inhalt des Steuerelements.  Daher ist es eine Stimme QuickInfo-Bezeichnung, obwohl es keine explizite `AutomationProperties.Name` festgelegt.

Mit `button2`, legen wir explizit den `AutomationProperties.Name` auf einen anderen als den Text des Steuerelements.

Mit `comboBox`, verwendet der `LabeledBy` -Eigenschaft zum Verweisen auf `label1` als Quelle für die Automatisierung `Name`, und klicken Sie in `label1` legen wir die `AutomationProperties.Name` auf eine natürlichere Ausdruck als was auf dem Bildschirm gerendert wird ("Day of Week" eher als "Select Tag der Woche").

Schließlich `button3`, VES holt die `Name` aus dem ersten untergeordneten Element seit `button3` selbst verfügt nicht über eine `AutomationProperties.Name` festgelegt.

## <a name="see-also"></a>Siehe auch
- [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "Grundlagen der Benutzeroberflächenautomatisierung")
- [Eigenschaften für die Unterstützung von Eingabehilfen in der Benutzeroberfläche](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "-Eigenschaften für die Unterstützung von Eingabehilfen in der Benutzeroberfläche")
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf Xbox One](index.md)
