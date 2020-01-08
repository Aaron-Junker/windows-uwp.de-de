---
title: Voice-aktivierte Shell (VES) auf Xbox
description: Erfahren Sie, wie Sie Ihre UWP-apps auf Xbox Unterstützung für sprach Steuerelemente hinzufügen.
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox, Speech, Voice-aktivierte Shell
ms.openlocfilehash: f51ec2c93a904893dc337545f634d04affde10fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685180"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Verwenden von Sprache zum Aufrufen von UI-Elementen

Voice-aktivierte Shell (VES) ist eine Erweiterung der Windows-Sprachplattform, die eine erstklassige Sprachdarstellung innerhalb von apps ermöglicht, sodass Benutzersprache zum Aufrufen von Bildschirm Steuerelementen und zum Einfügen von Text per Diktat verwenden können. VES ist bestrebt, eine gängige End-to-End-See-it-IT-Darstellung für alle Windows-Shells und-Geräte zu bieten, wobei der minimale Aufwand für App-Entwickler erforderlich ist.  Um dies zu erreichen, nutzt es die Microsoft Speech Platform und das UI Automation-Framework (UIA).

## <a name="user-experience-walkthrough"></a>Exemplarische Vorgehensweise zur Benutzer Darstellung ##
Im folgenden finden Sie eine Übersicht über die Möglichkeiten eines Benutzers bei der Verwendung von VES auf Xbox. Außerdem sollte er den Kontext vor dem Einstieg in die Details zur Funktionsweise von ven unterstützen.

- Der Benutzer schaltet die Xbox-Konsole ein und möchte seine apps durchsuchen, um etwas Interessantes zu finden:

        User: "Hey Cortana, open My Games and Apps"

- Der Benutzer befindet sich im aktiven Lesemodus (ALM), was bedeutet, dass die Konsole jetzt den Benutzer abhört, ein Steuerelement aufzurufen, das auf dem Bildschirm sichtbar ist, ohne dass Sie jedes Mal "Hey Cortana" sagen müssen.  Der Benutzer kann jetzt zum Anzeigen von apps wechseln und in der APP-Liste einen Bildlauf durchführen

        User: "applications"

- Zum Scrollen der Ansicht kann der Benutzer einfach Folgendes sagen:

        User: "scroll down"

- Der Benutzer sieht die Box-Art für die APP, an der er interessiert ist, aber den Namen vergessen hat.  Der Benutzer fordert die Anzeige von sprach Tipp Bezeichnungen an:

        User: "show labels"

- Nun ist klar, was Sie sagen können: die APP kann gestartet werden:

        User: "movies and TV"

- Um den Modus für aktive Überwachung zu beenden, teilt der Benutzer der Xbox mit, das lauschen anzuhalten

        User: "stop listening"

- Später kann eine neue aktive Überwachungs Sitzung mit folgenden Aktionen gestartet werden:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI-Automatisierungs Abhängigkeit ##
VES ist ein Benutzeroberflächenautomatisierungs-Client und basiert auf Informationen, die von der APP über seine Benutzeroberflächenautomatisierungs-Anbieter verfügbar gemacht Dies ist die gleiche Infrastruktur, die bereits von der Funktion "Sprachausgabe" auf Windows-Plattformen verwendet wird.  Die Benutzeroberflächen Automatisierung ermöglicht den programmgesteuerten Zugriff auf Benutzeroberflächen Elemente, einschließlich des Namens des Steuer Elements, seines Typs und der von ihm implementierten Steuerelement Muster.  Wenn sich die Benutzeroberfläche in der app ändert, reagiert VES auf UIA-Aktualisierungs Ereignisse und analysiert die aktualisierte Benutzeroberflächenautomatisierungs-Struktur erneut, um alle umsetzbaren Elemente zu finden. diese Informationen werden verwendet, um eine sprach Erkennungs Grammatik zu erstellen. 

Alle UWP-apps haben Zugriff auf das Benutzeroberflächenautomatisierungs-Framework und können Informationen über die Benutzeroberfläche unabhängig von dem Grafik Framework verfügbar machen, auf dem Sie basieren (XAML, DirectX/Direct3D, xamarin usw.).  In einigen Fällen, wie z. b. XAML, erfolgt der größte Teil der Arbeit durch das Framework

Weitere Informationen zur Benutzeroberflächen Automatisierung finden Sie unter [Grundlagen der Benutzeroberflächen Automatisierung](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Grundlagen der Benutzeroberflächenautomatisierung").

## <a name="control-invocation-name"></a>Name des Steuerungs aufnamens ##
Ves verwendet die folgende heuristische, um zu bestimmen, welcher Ausdruck mit der Spracherkennung als Name des Steuer Elements registriert werden soll (d. h., was der Benutzer zum Aufrufen des Steuer Elements sprechen muss).  Dies ist auch der Ausdruck, der in der Sprech Tip Bezeichnung angezeigt wird.

Quelle des Namens in der Prioritäts Reihenfolge:

1. Wenn das Element über eine `LabeledBy` angefügte Eigenschaft verfügt, verwendet Ves die `AutomationProperties.Name` dieser Text Bezeichnung.
2. `AutomationProperties.Name` des Elements.  In XAML wird der Text Inhalt des Steuer Elements als Standardwert für `AutomationProperties.Name`verwendet.
3. Wenn das Steuerelement ein ListItem oder eine Schaltfläche ist, sucht Ves nach dem ersten untergeordneten Element mit einem gültigen `AutomationProperties.Name`.

## <a name="actionable-controls"></a>Handlungsfähige Steuerelemente ##
Ves betrachtet, dass ein Steuerelement handlungsfähig ist, wenn eines der folgenden Automatisierungs Steuerelement Muster implementiert wird:

- **InvokePattern** (z. b. Button): stellt Steuerelemente dar, die eine einzelne, eindeutige Aktion initiieren oder ausführen und den Zustand bei der Aktivierung nicht beibehalten.

- **TogglePattern** (z. b. Kontrollkästchen): stellt ein Steuerelement dar, das eine Reihe von Zuständen durchlaufen und einen Zustand beibehalten kann, sobald er festgelegt ist.

- **SelectionItemPattern** (z. b. Kombinations Feld): stellt ein Steuerelement dar, das als Container für eine Auflistung von auswählbaren untergeordneten Elementen fungiert.

- **Expandredusepattern** (z. b. Kombinations Feld): stellt Steuerelemente dar, die visuell erweitert werden, um Inhalt anzuzeigen und zu reduzieren, um Inhalt auszublenden

- **ScrollPattern** (z. b. List): stellt Steuerelemente dar, die als scrollbare Container für eine Auflistung von untergeordneten Elementen fungieren.

## <a name="scrollable-containers"></a>Scrollbare Container ##
Für Bild lauffähige Container, die das ScrollPattern unterstützen, lauscht VES auf Sprachbefehle wie "Scroll Left", "Scroll right" usw. und führt einen Bildlauf mit den entsprechenden Parametern aus, wenn der Benutzer einen dieser Befehle auslöst.  Scrollbefehle werden basierend auf dem Wert der Eigenschaften `HorizontalScrollPercent` und `VerticalScrollPercent` eingefügt.  Wenn `HorizontalScrollPercent` beispielsweise größer als 0 ist, wird "Scroll Left" hinzugefügt, wenn es kleiner als 100 ist, "Scroll right" hinzugefügt wird usw.

## <a name="narrator-overlap"></a>Sprachausgabe Überlappung ##
Die Sprachausgabe Anwendung ist auch ein Benutzeroberflächenautomatisierungs-Client und verwendet die `AutomationProperties.Name`-Eigenschaft als eine der Quellen für den Text, den Sie für das aktuell ausgewählte UI-Element liest.  Um eine bessere Barrierefreiheits Funktion bereitzustellen, haben viele App-Entwickler die `Name`-Eigenschaft mit langem beschreibenden Text überladen, der beim Lesen durch die Sprachausgabe Weitere Informationen und Kontext bereitstellt.  Dies verursacht jedoch einen Konflikt zwischen den beiden Features: Ves benötigt kurze Ausdrücke, die mit dem sichtbaren Text des Steuer Elements übereinstimmen oder genau übereinstimmen, während die Sprachausgabe von längeren, aussagekräftigeren Ausdrücken profitiert, um einen besseren Kontext zu bieten.

Um dieses Problem zu beheben, wurde mit Windows 10 Creators Update die Sprachausgabe aktualisiert, um auch die `AutomationProperties.HelpText`-Eigenschaft zu überprüfen.  Wenn diese Eigenschaft nicht leer ist, spricht der Sprachausgabe Inhalt zusätzlich zu `AutomationProperties.Name`.  Wenn `HelpText` leer ist, liest die Sprachausgabe nur den Inhalt des Namens.  Dies ermöglicht es, dass bei Bedarf längere beschreibende Zeichen folgen verwendet werden, behält aber einen kürzeren Ausdrucks Erkennungs Ausdruck in der `Name`-Eigenschaft bei.

![](images/ves_narrator.jpg)

Weitere Informationen finden Sie [unter Automatisierungs Eigenschaften für die Barrierefreiheits Unterstützung in der Benutzeroberfläche](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Automatisierungs Eigenschaften für die Barrierefreiheits Unterstützung in der Benutzeroberfläche").

## <a name="active-listening-mode-alm"></a>Aktiver Überwachungsmodus (ALM) ##
### <a name="entering-alm"></a>Wechseln in Alm ###
Auf der Xbox lauscht VES nicht ständig auf Spracheingaben.  Der Benutzer muss den aktiven Lesemodus explizit eingeben, indem er Folgendes sagt:

- "Hallo Cortana, Select" oder
- "Hallo Cortana, treffen Sie eine Auswahl"

Es gibt mehrere andere Cortana-Befehle, die den Benutzer auch aktiv überwachen, wenn der Abschluss abgeschlossen ist, z. b. "Hey Cortana, Sign in" oder "Hey Cortana, Go Home". 

Die Eingabe von Alm hat die folgenden Auswirkungen:

- Die Cortana-Überlagerung wird in der oberen rechten Ecke angezeigt, und der Benutzer wird darüber informiert, was Sie sehen.  Während der Benutzer spricht, werden auch Ausdrucks Fragmente, die von der Spracherkennung erkannt werden, an dieser Stelle angezeigt.
- Ves analysiert die UIA-Struktur, sucht alle handlungsfähigen Steuerelemente, registriert Ihren Text in der sprach Erkennungs Grammatik und startet eine fortlaufende Überwachungs Sitzung.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Alm wird beendet ###
Das System bleibt in Alm, während der Benutzer mit der Benutzeroberfläche über die Stimme interagiert.  Es gibt zwei Möglichkeiten, Alm zu beenden:

- Der Benutzer hat explizit Folgendes gesagt: "lauschen Abbrechen", oder
- Ein Timeout tritt auf, wenn innerhalb von 17 Sekunden nach der Aktivierung von Alm oder seit der letzten positiven Erkennung keine positive Kennung vorhanden ist.

## <a name="invoking-controls"></a>Aufrufen von Steuerelementen ##
In Alm kann der Benutzer mithilfe von Voice mit der Benutzeroberfläche interagieren.  Wenn die Benutzeroberfläche ordnungsgemäß konfiguriert ist (mit namens Eigenschaften, die mit dem sichtbaren Text übereinstimmen), sollte die Verwendung von Voice zum Ausführen von Aktionen eine nahtlose, natürliche Oberfläche sein  Der Benutzer sollte in der Lage sein, nur zu sagen, was er auf dem Bildschirm angezeigt wird.

## <a name="overlay-ui-on-xbox"></a>Overlay-Benutzeroberfläche auf Xbox ##
Der Name des-Steuer Elements, das für ein Steuerelement abgeleitet ist, kann sich von dem tatsächlich sichtbaren Text in der Benutzeroberfläche  Dies kann darauf zurückzuführen sein, dass die `Name`-Eigenschaft des Steuer Elements oder das angefügte `LabeledBy` Element explizit auf eine andere Zeichenfolge festgelegt wird.  Oder das Steuerelement verfügt über keinen GUI-Text, sondern nur über ein Symbol oder Bildelement.

In diesen Fällen benötigen Benutzer eine Möglichkeit, um zu sehen, was zu sagen ist, um ein solches Steuerelement aufzurufen.  Aus diesem Grund können Sie nach dem aktiven lauschen die sprach Tipps anzeigen, indem Sie "Bezeichnungen anzeigen" sagen.  Dies bewirkt, dass Sprech Tip Bezeichnungen oberhalb jedes umsetzbaren Steuer Elements angezeigt werden.

Es gibt ein Limit von 100 Bezeichnungen. wenn die Benutzeroberfläche der APP mehr handlungsfähige Steuerelemente als 100 hat, gibt es einige, für die keine sprach Tipp Bezeichnungen angezeigt werden.  Die Bezeichnungen, die in diesem Fall ausgewählt werden, sind nicht deterministisch, da Sie von der Struktur und der Komposition der aktuellen Benutzeroberfläche als erste in der UIA-Struktur aufgezählt sind.

Wenn die Bezeichnungen der Sprech Tip angezeigt werden, gibt es keinen Befehl, Sie auszublenden. Sie bleiben sichtbar, bis eines der folgenden Ereignisse eintritt:

- Benutzer ruft ein Steuerelement auf.
- der Benutzer navigiert von der aktuellen Szene.
- der Benutzer sagt: "lauschen nicht mehr"
- Timeout für aktiven Lesemodus

## <a name="location-of-voice-tip-labels"></a>Speicherort der Sprech Tip Bezeichnungen ##
Sprach Tipp Bezeichnungen werden horizontal und vertikal innerhalb des boundingrechteck des Steuer Elements zentriert.  Wenn die Steuerelemente klein und eng gruppiert sind, können sich die Bezeichnungen von anderen überlappen bzw. durch andere verdeckt werden, und Ves versucht, diese Bezeichnungen zu trennen, um Sie zu trennen und sicherzustellen, dass Sie sichtbar sind  Es ist jedoch nicht garantiert, dass 100% der Fälle funktionieren.  Wenn eine stark überfüllte Benutzeroberfläche vorliegt, führt dies wahrscheinlich dazu, dass einige Bezeichnungen von anderen Personen verdeckt werden. Überprüfen Sie Ihre Benutzeroberfläche mit "Bezeichnungen anzeigen", um sicherzustellen, dass ausreichend Platz für die Sichtbarkeit von sprach Tipps vorliegt.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Kombinations Felder ##
Wenn ein Kombinations Feld erweitert wird, erhält jedes einzelne Element im Kombinations Feld eine eigene voicetip-Bezeichnung, und häufig werden diese auf der Grundlage vorhandener Steuerelemente hinter der Dropdown Liste angezeigt.  Um zu vermeiden, dass eine Übersichtlichkeit und eine verwirrende Muddle von Bezeichnungen angezeigt werden (wobei Kombinations Feld-Element Bezeichnungen mit den Bezeichnungen der Steuerelemente hinter dem Kombinations Feld gemischt werden), werden nur die Bezeichnungen für die untergeordneten Elemente angezeigt, wenn ein Kombinations Feld erweitert wird.  alle anderen sprach Tipp Bezeichnungen werden ausgeblendet.  Der Benutzer kann dann entweder eines der Dropdown Elemente auswählen oder das Kombinations Feld "Schließen".

- Bezeichnungen auf reduzierten Kombinations Feldern:

    ![](images/ves_combo_closed.png)

- Beschriftungen im erweiterten Kombinations Feld:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Scrollbare Steuerelemente ##
Bei Bild lauffähigen Steuerelementen werden die sprach Tipps für die scrollbefehle auf die einzelnen Ränder des Steuer Elements zentriert.  Sprach Tipps werden nur für die ausführbaren Bild Laufrichtungen angezeigt, z. b. Wenn ein vertikaler Bildlauf nicht verfügbar ist, wird der Bildlauf nach oben und der Bildlauf nach unten nicht angezeigt.  Wenn mehrere Bild lauffähige Bereiche vorhanden sind, werden durch ordinale für die Unterscheidung zwischen Ihnen (z. b. "Scroll Right 1", "Scroll Right 2" usw.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Mehrdeutigkeitsvermeidung ##
Wenn mehrere Benutzeroberflächen Elemente denselben Namen aufweisen oder die Spracherkennung mit mehreren Kandidaten übereinstimmt, wechselt Ves in den disambialisierungsmodus.  In diesem Modus werden voicetip-Bezeichnungen für die beteiligten Elemente angezeigt, sodass der Benutzer das richtige Element auswählen kann. Der Benutzer kann den disambitätsmodus abbrechen, indem er "Abbrechen" sagt.

Zum Beispiel:

- Im aktiven Empfangsmodus, vor der Eindeutigkeit; der Benutzer sagt: "ist ich mehrdeutig":

    ![](images/ves_disambig1.png) 

- Beide Schaltflächen stimmen überein. Eindeutigkeit gestartet:

    ![](images/ves_disambig2.png) 

- Die Click-Aktion wird angezeigt, wenn "Select 2" ausgewählt wurde:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Beispielbenutzeroberfläche ##
Im folgenden finden Sie ein Beispiel für eine XAML-basierte Benutzeroberfläche, in der die AutomationProperties.Name auf verschiedene Weise festgelegt wird:

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


Das obige Beispiel zeigt, wie die Benutzeroberfläche mit und ohne sprach Tipp Bezeichnungen aussieht.
 
- Im aktiven Empfangsmodus ohne angezeigte Bezeichnungen:

    ![](images/ves_alm_nolabels.png) 

- Im aktiven Lesemodus, nachdem der Benutzer "Bezeichnungen anzeigen" angezeigt hat:

    ![](images/ves_alm_labels.png) 

Im Fall von `button1`füllt XAML die `AutomationProperties.Name`-Eigenschaft automatisch mit Text aus dem sichtbaren Text Inhalt des Steuer Elements auf.  Aus diesem Grund gibt es auch dann eine Sprech Tip Bezeichnung, wenn kein expliziter `AutomationProperties.Name` festgelegt ist.

Mit `button2`legen wir die `AutomationProperties.Name` explizit auf einen anderen Wert als den Text des Steuer Elements fest.

Mit `comboBox`haben wir die Eigenschaft `LabeledBy` verwendet, um als Quelle der Automatisierungs `Name`auf `label1` zu verweisen. in `label1` wir die `AutomationProperties.Name` auf einen natürlicheren Ausdruck festgelegt, der nicht auf dem Bildschirm gerendert wird ("Wochentag" statt "Tag der Woche auswählen").

Zum Schluss greift mit `button3`die `Name` vom ersten untergeordneten Element ab, da für `button3` selbst kein `AutomationProperties.Name` festgelegt ist.

## <a name="see-also"></a>Weitere Informationen:
- [Grundlagen der Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Grundlagen der Benutzeroberflächenautomatisierung")
- [Automatisierungs Eigenschaften für die Barrierefreiheits Unterstützung in der Benutzeroberfläche](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Automatisierungs Eigenschaften für die Barrierefreiheits Unterstützung in der Benutzeroberfläche")
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf Xbox One](index.md)
