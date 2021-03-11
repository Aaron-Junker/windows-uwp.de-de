---
title: Vergleichen Sie Plattformfunktionen zwischen iOS, Android und Windows 10.
description: Sehen Sie sich einen ausführlichen Vergleich der Entwicklungskonzepte und Platt Form Features zwischen IOS, Android und der universelle Windows-Plattform (UWP) unter Windows 10 an.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: d15f0b1dccbdb165cb2bbcf39628e16859a42ed5
ms.sourcegitcommit: c5fdcc0779d4b657669948a4eda32ca3ccc7889b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102784731"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Windows-Apps-Konzeptzuordnung für Android- und iOS-Entwickler

Wenn Sie Entwickler mit Android- oder iOS-Kenntnissen sind und/oder über den entsprechenden Code verfügen und zu Windows 10 und zur universellen Windows-Plattform (UWP) wechseln möchten, bietet Ihnen dieser Artikel alle Informationen, die Sie für die Zuordnung der Plattformfeatures – und Ihrer Kenntnisse – zwischen den drei Plattformen benötigen.

Siehe auch die Portierungsinformationen in [Wechsel von iOS zu UWP](ios-to-uwp-root.md). Dieses Dokument kann [heruntergeladen](https://www.microsoft.com/download/details.aspx?id=52041) werden.

## <a name="user-interface-ui"></a>Benutzeroberfläche


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Entwurfssprache.</strong><br><br>Eine Reihe von Konventionen für das Aussehen und Verhalten von Apps auf der Plattform.</td>
<td align="left">Die Richtlinien von <strong>Material Design für Android</strong> bieten eine visuelle Sprache für Android-Designer und -Entwickler.</td>
<td align="left">Die <strong>Richtlinien für Eingabegeräte</strong> bieten Empfehlungen für iOS-Designer und -Entwickler.</td>
<td align="left"><a href="https://developer.microsoft.com/windows/apps/design"><strong>UWP-Windows-apps-Entwurf</strong></a> zeigt Ihnen, wie Sie eine APP erstellen, die auf allen Windows 10-Geräten fantastisch aussieht. Dort finden Sie Grundlagen zum Benutzeroberflächenentwurf, Techniken für reaktionsfähiges Design und eine umfassende Liste ausführlicher Richtlinien.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Markup Sprache der Benutzeroberfläche.</strong> <br><br>Eine Markupsprache, mit der die Benutzeroberfläche und ihre Komponenten gerendert und beschrieben werden. Jede Plattform bietet einen Editor für die visuelle Bearbeitung und Markupbearbeitung.<br/></td>
<td align="left"><strong>XML-Layouts</strong>, mit <strong>Android Studio</strong> oder <strong>Eclipse</strong> bearbeitet.</td>
<td align="left"><strong>XIB</strong> und <strong>Storyboards</strong>, mit <strong>Interface Builder</strong> in Xcode bearbeitet.</td>
<td align="left"><strong><a href="/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>, mit <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> und <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend für Visual Studio</a></strong> bearbeitet.<br/><br/><a href="/windows/uwp/xaml-platform/index">XAML-Plattform</a><br/><br/><a href="/windows/uwp/design/basics/xaml-basics-ui">Erstellen einer Benutzeroberfläche mit XAML</a><br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definieren von Layouts mit XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Integrierte Steuerelemente der Benutzeroberfläche.</strong> <br><br>Von der Plattform bereitgestellte wiederverwendbare UI-Elemente, z. B. Schaltflächen, Listensteuerelemente und Textsteuerelemente.</td>
<td align="left">Vordefinierte <strong>Ansichtsklassen</strong> und <strong>Ansichtsgruppenklassen</strong>, die als Widgets, Layouts, Textfelder, Container, Datums- und Uhrzeitsteuerelemente und Expertensteuerelemente bezeichnet werden.</td>
<td align="left"><strong>Ansichten</strong> und <strong>Steuerelemente</strong> in der Xcode-Objektbibliothek und im UIKit-Benutzeroberflächenkatalog. Ansichten umfassen Bildansichten, Auswahlansichten und Bildlaufansichten. Steuerelemente umfassen Schaltflächen, Datumsauswahl und Textfelder.</td>
<td align="left">Die XAML-Plattform bietet Ihnen einen reichhaltigen Satz <strong>integrierter Steuerelemente</strong>, z. B. Schaltflächen, Listensteuerelemente, Panels, Textsteuerelemente, Befehlsleisten, Auswahl, Medien und Freihandeingabe.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Steuern der Ereignis Behandlung.</strong> <br><br>Definieren der Logik, die ausgeführt wird, wenn in UI-Steuerelementen Ereignisse ausgelöst werden.</td>
<td align="left"><strong>Ereignishandler</strong> und <strong>Ereignislistener</strong> werden in XML oder programmgesteuert hinzugefügt.</td>
<td align="left">Steuerelemente senden <strong>Action</strong>-Meldungen an <strong>Ziele</strong>.</td>
<td align="left">Sie können Methoden zum Behandeln der Ereignisse eines XAML-Steuerelements in einer <strong>CodeBehind-Datei</strong>, die der XAML-Seite zugeordnet ist, definieren. <strong>Ereignishandler</strong> werden immer im Code geschrieben. Sie können jedoch diese Handler im XAML-Markup oder im Code mit Ereignissen verbinden.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a><br/><br/><a href="/windows/uwp/xaml-platform/events-and-routed-events-overview">Übersicht über Ereignisse und Routingereignisse</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Datenbindung.</strong> <br><br>Ein Softwaredesignmuster, das der App-UI das Rendern von Daten und optional das Synchronisieren mit diesen Daten ermöglicht.</td>
<td align="left">Es wird eine <strong>Datenbindungsbibliothek</strong> bereitgestellt, bislang jedoch nur als Betaversion.</td>
<td align="left">In iOS ist kein integriertes Bindungssystem vorhanden. Die <strong>Schlüssel-Wert-Beobachtung</strong> kann auf erstellt werden, um die Datenbindung durchzuführen, entweder mit der Verwendung einer Drittanbieter Bibliothek oder durch das Schreiben von zusätzlichem Code. Steuerelemente verwenden zum Abrufen von Daten Delegate/Rückrufe.</td>
<td align="left">Die <strong>Datenbindung</strong> erfolgt durch die UWP-Plattform. Mit der <strong><a href="/windows/uwp/xaml-platform/x-bind-markup-extension">{x:Bind}</a></strong>-Markuperweiterung nutzen Sie Bindung mit hoher Leistung, während Ihnen die <strong><a href="/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong>-Markuperweiterung eine größere Anzahl von Features bietet. Sie können die Bindung so konfigurieren, dass auf der Benutzeroberfläche mithilfe von <strong>unidirektionaler Bindung</strong> Werte aus einer Datenquelle angezeigt werden oder dass mithilfe von <strong>bidirektionaler Bindung</strong> diese Werte außerdem beobachtet werden und die Benutzeroberfläche aktualisiert wird, wenn sie sich ändern.<br/><br/><a href="/windows/uwp/data-binding/index">Datenbindung</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Automatisierung der Benutzeroberfläche.</strong> <br><br>Der programmgesteuerte Zugriff auf UI-Elemente ermöglicht den Zugriff auf Apps durch Hilfstechnologieprodukte und die Interaktion von automatisierten Testskripts mit der Benutzeroberfläche.</td>
<td align="left"><strong>Beschriftungen</strong>, <strong>contentDescription</strong>- und <strong>hint</strong>-Werte erleichtern die automatisierte Suche von UI-Elementen. Android Studio ermöglicht Ihnen mit den Testumgebungen <strong>UI Automator</strong> und <strong>Espresso</strong> das Schreiben von UI-Tests.</td>
<td align="left">Mit dem <strong>Automation-Instrument</strong> können Sie automatisierte UI-Testskripts schreiben, die mithilfe der <strong>Accessibility</strong>-Einstellungen Elemente identifizieren oder die die Position des Elements in der <strong>Elementhierarchie</strong> ermitteln.</td>
<td align="left">Die <strong><a href="/windows/desktop/WinAuto/uiauto-uiautomationoverview">Benutzeroberflächenautomatisierung</a></strong> bietet Ihnen unmittelbaren programmgesteuerten Zugriff auf integrierte UI-Elemente in UWP.<br/><strong><a href="/windows/uwp/accessibility/custom-automation-peers">Benutzerdefinierte Automatisierungspeers</a></strong> ermöglichen Ihnen die Bereitstellung von Automatisierungsunterstützung für eigene benutzerdefinierte UI-Klassen. Mit dem <strong><a href="/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">Coded UI-Testprojekt</a></strong> in Visual Studio können Sie Ihre gesamte Anwendung automatisch über die Benutzeroberfläche testen oder die Benutzeroberfläche isoliert testen.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Ändern der Darstellung eines Steuer Elements.</strong> <br><br>Bearbeiten von Größe, Farbe und anderen Attributen.</td>
<td align="left">Steuerelemente verfügen über <strong>Eigenschaften</strong>, die mithilfe des Entwicklungstools, im XML-Markup oder programmgesteuert bearbeitet werden können.</td>
<td align="left">Steuerelemente verfügen über <strong>Attribute</strong>, die Sie mit dem <strong>Attributes Inspector</strong> in Interface Builder oder programmgesteuert bearbeiten können.</td>
<td align="left">Sie können mit Visual Studio und Blend für Visual Studio die <strong>Eigenschaften</strong> von Steuerelementen im XAML-Markup oder programmgesteuert bearbeiten.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Hinzufügen von Steuerelementen und Verarbeiten von Ereignissen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Wiederverwendbare visuelle Stile.</strong> <br><br>Anwenden visueller Änderungen in einem wiederverwendbaren Format auf eine Reihe von Steuerelementen.</td>
<td align="left"><strong>XML Styles</strong> sind Gruppen von Eigenschaften, die auf ein oder mehrere Steuerelemente angewendet werden.</td>
<td align="left">In iOS wird die Wiederverwendung visueller Stile nicht standardmäßig unterstützt, jedoch ermöglicht das UIAppearance-Protokoll die gemeinsame Nutzung allgemeiner Attribute durch mehrere Steuerelemente.</td>
<td align="left">Sie können wiederverwendbare <strong><a href="/uwp/api/Windows.UI.Xaml.Style">Stile</a></strong> erstellen, die auf mehrere Steuerelemente angewendet und für die einfache Wiederverwendung in einem <strong><a href="/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> gespeichert werden.<br/><br/><a href="/previous-versions/windows/apps/hh465381(v=win.10)">Schnellstart: Entwerfen von Steuerelementen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bearbeiten der visuellen Struktur von Steuerelementen.</strong> <br><br>Passen Sie die visuelle Struktur eines Steuer Elements über das Ändern von Eigenschaften oder Attributen hinaus an, indem Sie beispielsweise den CheckBox-Text unterhalb des Kontrollkästchens verschieben.</td>
<td align="left">In Android gibt es keine einfache Methode zum Bearbeiten der visuellen Struktur von Steuerelementen.</td>
<td align="left">In iOS gibt es keine einfache Methode zum Bearbeiten der visuellen Struktur von Steuerelementen.</td>
<td align="left">Um die visuelle Struktur eines Steuerelements anzupassen, können Sie seine <strong><a href="/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">Steuerelementvorlage</a></strong> im XAML-Markup kopieren und bearbeiten.<br/><br/><a href="/previous-versions/windows/apps/hh465374(v=win.10)">Schnellstart: Steuerelementvorlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Integrierte Touchgesten.</strong> <br><br>Bereitstellen von angepasster Touchunterstützung durch die Behandlung allgemeiner Gestenereignisse, z. B. Tippen und Doppeltippen in Ansichten und Steuerelementen.</td>
<td align="left"><strong>GestureDetectors</strong> erkennen allgemeine Touchgesten, einschließlich Bildlauf, langes Drücken, Tippen, Doppeltippen und schnelles Wischen.</td>
<td align="left">Das UIKit-Framework bietet integrierte <strong>Gestenerkennungen</strong>, die Touchgesten, einschließlich Tippen, Zusammendrücken, Schwenken, Wischen, Drehen und langes Drücken, erkennen.</td>
<td align="left">Mit <strong>UI-Elementen</strong> können Sie <strong>statische Gestenereignisse</strong>, z. B. Tippen, Doppeltippen, Rechtstippen und Halten, sowie <strong>Manipulationsgestenereignisse</strong>, z. B. Ziehen, Wischen, Drehen, Zusammendrücken und Aufziehen, behandeln. Gestenereignisse sind <strong>Routingereignisse</strong> und können von übergeordneten Objekten, die das untergeordnete UIElement enthalten, behandelt werden.<br/><br/><a href="/windows/uwp/input-and-devices/touch-interactions">Interaktionen per Toucheingabe</a><br/><br/><a href="/windows/uwp/design/layout/index">Benutzerdefinierte Benutzerinteraktionen – Gesten, Manipulationen und Interaktionen</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navigations- und App-Struktur</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Layouts.</strong> <br><br>Das Layout definiert die Struktur der Benutzeroberfläche.</td>
<td align="left">Das Layout besteht aus <strong>Ansichtsgruppen</strong>, z. B. <strong>LinearLayout</strong> und <strong>RelativeLayout</strong>, in denen andere Ansichtsgruppen oder Ansichten geschachtelt werden können.</td>
<td align="left">Das Layout besteht aus einem <strong>UIViewController</strong>, der <strong>UIViews</strong> enthält, die geschachtelt werden können.</td>
<td align="left">XAML, das ein flexibles Layoutsystem bereitstellt, das aus <strong>LayoutPanel-Klassen</strong> wie <strong><a href="/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.relativepanel">relativepanel</a></strong> und <strong><a href="/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> für statische und reaktionsfähige Layouts besteht. <strong><a href="/visualstudio/ide/reference/properties-window?view=vs-2015">Eigenschaften</a></strong> dienen zum Steuern der Größe und Position der Elemente.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definieren von Layouts mit XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Peer Navigation.</strong> <br><br>Bereitstellen von Methoden für den Benutzer, um zwischen Seiten auf der gleichen Hierarchieebene zu navigieren.</td>
<td align="left"><strong>Registerkarten</strong>, <strong>wischen-Ansichten</strong> und <strong>Navigations Schub</strong> leisten bieten eine <strong>seitliche Navigation</strong>.</td>
<td align="left"><strong>Register</strong>Karten leisten Controller, <strong>geteilte Ansichts</strong> Controller und <strong>Seiten Ansichts Controller</strong> ermöglichen die Navigation zwischen Sichten der gleichen Hierarchie.</td>
<td align="left">Sie können eine persistente Liste von Links/Registerkarten über dem Inhalt mithilfe von <strong><a href="/windows/uwp/controls-and-patterns/navigationview">navigationview</a></strong>anzeigen. Mit <strong><a href="/windows/uwp/controls-and-patterns/split-view">Navigationsbereich/geteilter Ansicht</a></strong> können Sie neben dem Inhalt eine Liste von Links anzeigen.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Navigieren zwischen zwei Seiten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Hierarchische Navigation.</strong> <br><br>Navigieren zwischen über- und untergeordneten Seiten einer Hierarchie.</td>
<td align="left"><strong>Listen</strong>, <strong>Raster Listen</strong>, <strong>Schalt</strong> Flächen und andere Steuerelemente bieten eine untergeordnete <strong>Navigation</strong> , wenn Sie mit <strong>Intents</strong> verwendet werden, um andere <strong>Aktivitäten</strong>zu laden.</td>
<td align="left"><strong>Navigations Controller</strong> ermöglichen Benutzern das Navigieren zwischen den Ebenen einer Hierarchie.</td>
<td align="left"><strong>							Mit <a href="/windows/uwp/controls-and-patterns/hub">Hubs</a></strong> kann für den Benutzer eine Vorschau von Inhalten angezeigt werden, die ausgewählt werden können, um zwischen untergeordneten Seiten zu navigieren. <strong>							Mit <a href="/windows/uwp/controls-and-patterns/master-details">Master/Details</a></strong> kann der Benutzer aus einer Liste von Elementübersichten auswählen, die neben dem entsprechenden Detailabschnitt angezeigt werden.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Navigieren zwischen zwei Seiten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigations Taste zurück.</strong> <br><br>Rückwärtsnavigation in einer Anwendung.</td>
<td align="left">Die <strong>Zurück</strong>- und <strong>Nach-oben</strong>-Schaltfläche auf der Aktionsleiste bieten <strong>aufsteigende</strong> und <strong>temporale</strong> Navigation mithilfe des <strong>Zurück-Stapels</strong>.</td>
<td align="left">Dem <strong>Navigation Controller</strong> kann eine Zurück-Schaltfläche hinzugefügt werden.<br/></td>
<td align="left">Mit der <strong><a href="/uwp/api/windows.ui.xaml.controls.frame.backstack">BackStack-Eigenschaft</a></strong>, die dem Benutzer das Durchlaufen des <strong>Navigationsverlaufs</strong> ermöglicht, lässt sich das Betätigen der Zurück-Schaltfläche bzw. Zurück-Taste einfach behandeln.<br/><br/><a href="/windows/uwp/layout/navigation-history-and-backwards-navigation">Navigation per Schaltfläche „Zurück“</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Begrüßungsbildschirm.</strong> <br><br>Das Anzeigen eines Bilds beim Starten der App, hauptsächlich für Branding verwendet.</td>
<td align="left">Begrüßungsbildschirme werden nicht standardmäßig bereitgestellt und werden durch Bearbeiten des ersten <strong>Designhintergrunds</strong> für Aktivitäten implementiert.</td>
<td align="left">Apps müssen über ein <strong>statisches Startbild</strong> oder eine <strong>XIB-/Storyboard-Startdatei</strong> verfügen.</td>
<td align="left">Sie erstellen einen Begrüßungsbildschirm mithilfe eines <strong>Bilds</strong> und eines farbigen Hintergrunds. <a href="/windows/uwp/launch-resume/create-a-customized-splash-screen">Die Anzeige eines Begrüßungsbildschirms kann verlängert werden</a>.<br/><br/><a href="/windows/uwp/launch-resume/add-a-splash-screen">Hinzufügen eines Begrüßungsbildschirms</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Benutzerdefinierte Eingaben</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Anschließen.</strong> <br><br>Spracherkennung für die Spracheingabe und zusätzliche Sprachfunktionen.</td>
<td align="left">Die Spracheingabe kann durch jede App bereitgestellt werden, die einen <strong>RecognizerIntent</strong> implementiert, z. B. die <strong>Google-Sprachsuche</strong>. Die <strong>SpeechRecognizer</strong>-Klasse ermöglicht Apps die Verwendung der Spracherkennungs-API von Google.</td>
<td align="left">Apps können die <strong>sfspeech</strong> -Erkennungs Klasse verwenden, um Spracheingaben und sprach Erkennungsmöglichkeiten zu implementieren.</td>
<td align="left">Sie können für die Interaktion mit der App im Vordergrund die <strong><a href="/windows/uwp/input-and-devices/speech-recognition">Spracherkennungs</a></strong>-API verwenden. Sie können sprachbasierte <strong><a href="/windows/uwp/input-and-devices/cortana-interactions">Cortana-Interaktionen</a></strong> verwenden, um apps im Vorder-oder Hintergrund zu starten und um mit Hintergrund-apps zu interagieren.<br/><br/><a href="/windows/uwp/input-and-devices/speech-interactions">Sprachinteraktionen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Benutzerdefinierte Benutzereingaben.</strong> <br><br>Behandeln von Eingaben per Tastatur, Maus, Stift und anderen Eingaben.</td>
<td align="left">Die Unterstützung von Interaktionen umfasst <strong>Berührung</strong>, <strong>Touchpad</strong>, <strong>Eingabestift</strong>, <strong>Maus</strong> und <strong>Tastatur</strong>. Bewegungen und Eingaben werden auf die gleiche Weise wie Berührung gemeldet, es lassen sich jedoch weitere Informationen über das <strong>Eingabegerät</strong> ermitteln.</td>
<td align="left">Es wird Unterstützung für <strong>Berührung</strong>, <strong>Apple Pencil</strong> und <strong>Hardwaretastaturen</strong> bereitgestellt.</td>
<td align="left">Sie finden Unterstützung für eine Vielzahl von Interaktionen, einschließlich <strong><a href="/windows/uwp/input-and-devices/touch-interactions">Berührung</a></strong>, <strong><a href="/windows/uwp/input-and-devices/touchpad-interactions">Touchpad</a></strong>, <strong><a href="/windows/uwp/input-and-devices/pen-and-stylus-interactions">Zeichen-/Eingabestift</a></strong> mit Freihandschrift, <strong><a href="/windows/uwp/input-and-devices/mouse-interactions">Maus</a></strong> und <strong><a href="/windows/uwp/input-and-devices/keyboard-interactions">Tastatur</a></strong>. Ihre Apps können die Daten behandeln, ohne Informationen über das verwendete Eingabegerät zu benötigen, und bei Bedarf kann auf Daten von Rohdateneingabegeräten zugegriffen werden.<br/><br/><a href="/windows/uwp/input-and-devices/handle-pointer-input">Behandeln von Zeigereingaben</a><br/><br/><a href="/windows/uwp/design/layout/index">Benutzerdefinierte Benutzerinteraktionen</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Daten</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Lokale App-Daten.</strong> <br><br>Lokales Speichern von Einstellungen und Dateien für die App.</td>
<td align="left">Lokale Dateien können mit <strong>openFileOutput</strong> und <strong>openFileInput</strong> gespeichert werden. Auf die Einstellungen in einer <strong>SharedPreferences-Datei</strong> kann mithilfe von <strong>getSharedPreferences</strong> zugegriffen werden.</td>
<td align="left">Lokale Dateien können im Verzeichnis <strong>Application Support</strong> gespeichert werden, auf das über die <strong>NSFileManager</strong>-Klasse zugegriffen wird. Der Zugriff auf die Einstellungen in <strong>Preference</strong>-Dateien erfolgt über die <strong>NSUserDefaults</strong>-Klasse.</td>
<td align="left">Die <strong><a href="/uwp/api/Windows.Storage">Windows. Storage</a></strong> -Klassen verarbeiten den lokalen Datenspeicher auf einheitliche Weise. Sie speichern Einstellungen als <strong><a href="/uwp/api/Windows.Storage.ApplicationDataContainer">applicationdatacontainer</a></strong> -Objekt, auf das über die <strong><a href="/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData. LocalSettings</a></strong> -Eigenschaft zugegriffen wird. Dateien in einem <strong><a href="/uwp/api/windows.storage.storagefolder">storagefolder</a></strong> -Objekt, auf das über die <strong><a href="/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData. localfolder</a></strong> -Eigenschaft zugegriffen wird, werden gespeichert.<br/><br/><a href="/windows/uwp/app-settings/store-and-retrieve-app-data">Speichern und Abrufen von Einstellungen und anderen App-Daten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Lokaler Daten Bank Speicher.</strong> <br><br>Speichern von App-Daten in einer relationalen Datenbank, ggf. mit objektrelationalem Mapping (ORM).</td>
<td align="left">Die <strong>SQLite</strong>-Datenbank wird bereitgestellt. ORM ist nicht integriert. SQL-Abfragen werden mit der <strong>SQLiteDatabase</strong>-Klasse ausgeführt.</td>
<td align="left">Die <strong>SQLite</strong>-Datenbank wird bereitgestellt. <strong>CoreData</strong> ist das integrierte Objekt Graph-Framework, das mit SQLite verwendet werden kann und Funktionen bereitstellt, die mit einem ORM vergleichbar sind.</td>
<td align="left">Sie können Daten mithilfe <strong>SQLite</strong> speichern. <strong><a href="/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> ist ein integriertes ORM, mit dem die Notwendigkeit entfällt, umfangreichen Datenzugriffscode zu schreiben, und mit dem Sie die Datenbank einfach abfragen können, ohne SQL zu schreiben. Sie können SQL-Abfragen direkt mit der <a href="/windows/uwp/data-access/sqlite-databases">SQLite-Bibliothek</a> ausführen.<br/><br/><a href="/windows/uwp/data-access/index">Datenzugriff</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>HTTP-Bibliotheken für den Rest-Zugriff.</strong> <br><br>Integrierte Bibliotheken, die Ihnen die Kommunikation mit Webdiensten und Webservern per HTTP(S) ermöglichen.<br/></td>
<td align="left">HTTP-Bibliotheken <strong>HttpURLConnection</strong> und <strong>Volley</strong>.</td>
<td align="left"><strong>NSURLSession</strong>, <strong>NSURLConnection</strong> und <strong>NSURLDownload</strong>.</td>
<td align="left">Die integrierte <strong><a href="/uwp/api/Windows.Web.Http.HttpClient">HttpClient</a></strong>-API bietet Ihnen Zugriff auf allgemeine HTTP-Funktionen, z. B. GET, DELETE, PUT, POST, allgemeine Authentifizierungsmuster, SSL, Cookies und Statusinformationen.</td>
</tr>
<tr class="even">
<td align="left"><strong>Cloud-Sicherungsdienste.</strong> <br><br>Von der Plattform bereitgestellte Sicherungsdienste für App-Daten.</td>
<td align="left">Mit dem <strong>Backup Manager</strong> von Android werden Anwendungsdaten im <strong>Android Sicherungsdienst</strong> von Google gesichert.</td>
<td align="left">Das <strong>iCloud-Backup</strong> kann vom Benutzer zum Durchführen der Sicherungen, einschließlich der App-Daten, konfiguriert werden. Apps, die mit iCloud kompatible <strong>Core Data</strong>, den <strong>iCloud-Schlüssel-Wert-Speicher</strong> und die <strong>Dokumentenspeicherung in iCloud</strong> verwenden.</td>
<td align="left">Alle App-Daten, die Sie mithilfe der Roaming- <strong><a href="/uwp/api/windows.storage.applicationdata">ApplicationData-APIs</a></strong> (einschließlich <strong><a href="/uwp/api/windows.storage.applicationdata.roamingfolder">roamingfolder</a></strong> und <a href="/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>roamingsettings</strong></a>) speichern, werden automatisch mit der Cloud und den anderen Geräten des Benutzers synchronisiert. Die Synchronisierung erfolgt über das Microsoft-Konto des Benutzers.<br/><br/><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">Richtlinien für das Roaming von App-Daten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Herunterladen von http-Dateien.</strong> <br><br>Herunterladen von kleinen und großen Dateien per HTTP.</td>
<td align="left">Für den Download per HTTP und FTP werden <strong>URLConnection</strong> und <strong>HTTPURLConnection</strong> verwendet. Für den Download im Hintergrund kann auch der <strong>Download-Manager</strong> des Systems verwendet werden.</td>
<td align="left">Für den Download von Dateien per HTTP und FTP können <strong>NSURLSession</strong> und <strong>NSURLConnection</strong> verwendet werden.</td>
<td align="left">Mit der <strong><a href="/uwp/api/windows.networking.backgroundtransfer">Hintergrundübertragungs-API</a></strong> können Sie Dateien zuverlässig per HTTP(S) und FTP übertragen und dabei das Anhalten der App sowie Verbindungsunterbrechungen berücksichtigen und Anpassungen entsprechend Konnektivität und Akkulaufzeit vornehmen. Sie können auch <strong><a href="/uwp/api/windows.web.http.httpclient">HttpClient</a></strong> verwenden, der sich ideal für kleinere Dateien eignet.<br/><br/><a href="/windows/uwp/networking/which-networking-technology">Welche Netzwerktechnologie?</a><br/><br/><a href="/windows/uwp/networking/background-transfers">Hintergrundübertragungen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Silbernen.</strong> <br><br>Erstellen von UDP-Datagrammsockets und TCP-Sockets als Low-Level-Schnittstelle für die Kommunikation mit anderen Geräten unter Verwendung des eigenen Protokolls.</td>
<td align="left">Die <strong>Socket</strong>-Klasse stellt TCP-Sockets bereit, und die <strong>DatagramSocket</strong>-Klasse stellt einen UDP-Socket bereit.</td>
<td align="left"><strong>Nsstream</strong> und <strong>CFStream</strong> bieten TCP-Sockets, <strong>cfsocket</strong> stellt UDP-Sockets bereit.</td>
<td align="left">Sie können die <strong><a href="/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> -Klasse verwenden, um mithilfe eines UDP-Datagramm-Sockets und der <strong><a href="/uwp/api/Windows.Networking.Sockets.StreamSocket">Streamsocket</a></strong> -Klasse über TCP oder Bluetooth RFCOMM zu kommunizieren.<br/><br/><a href="/windows/uwp/networking/networking-basics">Netzwerkgrundlagen</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">Welche Netzwerktechnologie?</a><br/><br/><a href="/windows/uwp/networking/sockets">Übersicht über Sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Bieten bidirektionale Kommunikation zwischen einem Client und einem Server und ermöglichen die Datenübertragung in Echtzeit.</td>
<td align="left">In Android sind keine WebSockets-Bibliotheken integriert.</td>
<td align="left">In iOS sind keine WebSockets-Bibliotheken integriert.</td>
<td align="left">Sichere Verbindungen mit Servern, die websockets unterstützen, können mit der <strong><a href="/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> -Klasse für kleinere Nachrichten mit Empfangs Benachrichtigungen und <strong><a href="/uwp/api/windows.networking.sockets.streamwebsocket">streamwebsocket</a></strong> für größere Binärdatei Übertragungen hergestellt werden, die in Abschnitten gelesen werden können.<br/><br/><a href="/windows/uwp/networking/networking-basics">Netzwerkgrundlagen</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">Welche Netzwerktechnologie?</a><br/><br/><a href="/windows/uwp/networking/websockets">Übersicht über WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>OAuth-Bibliotheken.</strong> <br><br>OAuth-Bibliotheken ermöglichen den Zugriff auf OAuth-Drittanbieter und jede in die Plattform integrierte Kontoverwaltung.</td>
<td align="left">Es wird keine generische OAuth-Bibliothek bereitgestellt. Für die OAuth-Authentifizierung bei Google Play-Diensten wird die <strong>GoogleAuthUtil</strong>-Klasse bereitgestellt.<br/></td>
<td align="left">Es wird keine generische OAuth-Bibliothek bereitgestellt. Das <strong>Accounts Framework</strong> bietet Zugriff auf Benutzerkonten, die bereits auf dem Gerät gespeichert sind, z. B. Facebook und Twitter.</td>
<td align="left">Mit dem <strong><a href="/windows/uwp/security/web-authentication-broker">Webauthentifizierungsbroker</a></strong> aus der generischen OAuth-Bibliothek können Sie eine Verbindung mit Identitätsanbieterdiensten von Drittanbietern herstellen. Die Benutzer können im <strong><a href="/windows/uwp/security/credential-locker">Schließfach für Anmeldeinformationen</a></strong> ihre Anmeldeinformationen speichern und sie auf mehreren Geräten verwenden. Mit dem <strong><a href="/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft. Live</a></strong> -Namespace können Sie problemlos auf Live SDK OAuth zugreifen, um auf Microsoft-Dienste zuzugreifen.<br/><br/><a href="/windows/uwp/security/authentication-and-user-identity">Authentifizierung und Benutzeridentität</a><br/><br/><a href="/uwp/api/windows.security.authentication.web">Windows.Security.Authentication.Web-API-Dokumentation</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">WebAuthenticationBroker-Codebeispiel</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Tools</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Das zum Erstellen der App verwendete Toolset.</td>
<td align="left"><strong>Android Studio</strong> und <strong>Eclipse</strong>mit Google Push-Entwicklern zur Verwendung von Android Studio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> und <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend für Visual Studio</a></strong> verfügen über alle Tools, die Sie zum Codieren, Entwerfen, Verbinden, Debuggen, Analysieren, Optimieren und Testen von UWP-Apps benötigen. Visual Studio bietet Ihnen außerdem <strong><a href="/windows/uwp/debug-test-perf/test-with-the-emulator">Emulatoren</a></strong> für Windows 10-Geräte, sodass Sie Ihre App auf unterschiedlichen emulierten Geräten testen können.<br/><br/><a href="https://developer.microsoft.com/windows/downloads">Downloads und Tools für UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Code Organisation.</strong> <br><br>Die grundlegende Ordnerstruktur einer App, häufig anhand einer anfänglichen Vorlage erstellt.</td>
<td align="left"><strong>AndroidManifest</strong>-Datei, <strong>java</strong>-Ordner mit Quelldateien, <strong>res</strong>-Ordner mit Ressourcen, einschließlich Layouts und Werten, <strong>Gradle</strong>-Buildskripts in Android Studio und <strong>Ant</strong>-Buildskripts in Eclipse.</td>
<td align="left">Quelldateien und <strong>Supporting Files</strong>, Datei <strong>Info.plist</strong>, <strong>Main.storyboard</strong> und <strong>LaunchScreen.storyboard</strong>. In <strong>Objektbibliotheken</strong> gespeicherte Images.</td>
<td align="left">Die UWP-App enthält XAML- und Codedateien für die App mit den Namen „Example.xaml“ und „Example.xaml.cs“, verschiedene Images im Ordner <strong>Assets</strong>, eine Startseite, z. B. <strong>MainPage.xaml</strong> oder <strong>MainPage.xaml.cs</strong>, und ein Manifest.<br/><br/><a href="/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Erstellen einer "Hello World"-App</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">App-Lebenszyklus</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>App-Lebenszyklus.</strong> <br><br>Behandeln von Ereignissen beim Starten, Anhalten, Fortsetzen und Schließen der App, mit der Möglichkeit, den Anwendungszustand zu speichern/wiederherzustellen und weitere Aufgaben auszuführen.</td>
<td align="left">Jede Aktivität verfügt über einen eigenen <strong>Aktivitätslebenszyklus</strong> mit Zuständen, z. B. <strong>resumed</strong>. <strong>Lebenszyklus Rückrufe</strong> , wie z. b. <strong>onresume</strong> , werden in Ihren <strong>Aktivitäts Klassen</strong>implementiert.</td>
<td align="left">Der <strong>Anwendungslebenszyklus</strong> weist Zustände, z. B. <strong>suspended</strong>, auf. Methoden, z. B. <strong>applicationDidEnterBackground:</strong>, werden im <strong>appDelegate</strong>-Objekt implementiert, um bei Zustandsänderungen Code auszuführen.</td>
<td align="left">Die Anwendung weist die <strong>App-Ausführungszustände</strong> „NotRunning“, „Activated“, „Runningׅ“, „Suspending“, „Suspended“ und „Resuming“ auf.<br/><br/>Sie können die Methode „OnLaunched“, „OnActivated“, „Suspending“ oder „Resuming“ der <strong><a href="/uwp/api/windows.ui.xaml.application">Application-Klasse</a></strong> implementieren, um Code auszuführen, wenn sich der Zustand ändert.<br/><br/><a href="/windows/uwp/launch-resume/app-lifecycle">App-Lebenszyklus</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Hintergrundaufgaben.</strong> <br><br>Aufgaben, die Vorgänge im Hintergrund ausführen und weiterhin ausgeführt werden, wenn die App nicht mehr im Vordergrund ausgeführt wird.</td>
<td align="left">Apps können <strong>Dienste</strong> starten, die Hintergrundvorgänge ausführen, wenn die App nicht mehr im Vordergrund ausgeführt wird. Dienste besitzen einen eigenen <strong>Lebenszyklus</strong> und werden im Manifest registriert.</td>
<td align="left">Die <strong>Hintergrund Ausführung</strong> ist nur für bestimmte Aufgaben Typen zulässig.<br/><br/>Apps deklarieren <strong>unterstützte Hintergrundaufgaben</strong> in der Datei „Info.plist“ mithilfe der <strong>UIBackgroundModes</strong>.<br/><br/>Das System steuert, wann und wie lange Hintergrundaufgaben ausgeführt werden.</td>
<td align="left">Sie können einen Hintergrund Task erstellen, indem Sie die <strong><a href="/uwp/api/windows.applicationmodel.background.ibackgroundtask">ibackgroundtask</a></strong> -Schnittstelle implementieren und die Aufgabe im Anwendungs Manifest registrieren. Sie können festlegen, dass eine Aufgabe mit einem <a href="/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>Timer</strong></a>, einem <a href="/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>System Trigger</strong></a>und einem <a href="/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>Wartungs Trigger</strong></a>ausgelöst wird.<br/><br/><a href="/windows/uwp/launch-resume/support-your-app-with-background-tasks">Unterstützen Ihrer App mit Hintergrundaufgaben</a><br/><br/><a href="/windows/uwp/launch-resume/create-and-register-a-background-task">Erstellen und Registrieren einer Hintergrundaufgabe</a><br/><br/><a href="/windows/uwp/launch-resume/guidelines-for-background-tasks">Richtlinien für Hintergrundaufgaben</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Leistung</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bewährte Methoden für die Leistung.</strong> <br><br>Richtlinien für das Erstellen von schnellen, dynamischen und akkuschonenden Apps mit kurzer Startzeit.</td>
<td align="left">Android bietet das Schulungshandbuch <strong>Best Practices for Performance</strong> (Bewährte Methoden zur Leistungssteigerung).</td>
<td align="left">iOS bietet das Dokument <strong>Performance Overview</strong> (Übersicht über die Leistung).</td>
<td align="left">Ausführliche Informationen zu Themen, wie z. b., finden Sie im ausführlichen <strong><a href="/windows/uwp/debug-test-perf/performance-and-xaml-ui">Leistungs Handbuch</a></strong> . Festlegen von Leistungszielen, Messen der Leistung, Speicherverwaltung, Smooth Animation, effizientes zugreifen auf das Dateisystem und verfügbare Tools für die Profilerstellung und Leistung.</td>
</tr>
<tr class="even">
<td align="left"><strong>Anzeigen der Optimierung für eine reaktionsfähige Benutzeroberfläche.</strong> <br><br>Verbessern der Leistung durch Optimieren der Ansichten.</td>
<td align="left">Das Optimieren der <strong>Layouthierarchien</strong> mit dem Tool Hierarchy Viewer, das <strong>Wiederverwenden von Layouts</strong> und das Laden von <strong>Ansichten bei Bedarf</strong> sind Verfahren, mit denen Sie die Reaktionsfähigkeit des UI-Threads erhalten und verhindern, dass &quot;ANR&quot;-Meldungen (<strong>Application Not Responding</strong>) ausgegeben werden.<br/></td>
<td align="left">Beheben von UI-Problemen mit <strong>Offscreen-Rendering</strong>, <strong>Mischebenen</strong> und <strong>Rasterung</strong> mithilfe des Tools <strong>Core Animation</strong>, um die Reaktionsfähigkeit des UI-Threads zu erhalten.</td>
<td align="left">Sie können mit einigen einfachen Schritten das XAML-<strong>Markup</strong> und die XAML-<strong>Layouts</strong><strong>optimieren</strong>. Dazu gehören das Reduzieren der Layoutstruktur, das Minimieren der Elementanzahl und das Minimieren der Überzeichnung. <br/><br/><a href="/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">Aufrechterhalten der Reaktionsfähigkeit des UI-Threads</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-xaml-loading">Optimieren Ihres XAML-Markups</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-your-xaml-layout">Optimieren des XAML-Layouts</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threading.</strong> <br><br>Verwenden von Threading zum Aufrechterhalten der <strong>Reaktionsfähigkeit der Benutzeroberfläche</strong> und <strong>parallelen Ausführen mehrerer Aufgaben</strong>.</td>
<td align="left">Threading erfolgt mithilfe der Klassen <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong> und der abstrakten Klasse <strong>AsyncTask</strong>.</td>
<td align="left">Threading erfolgt mithilfe von <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> und der abstrakten Klasse <strong>NSOperation</strong>.</td>
<td align="left">Sie können mit Threads arbeiten, indem Sie <strong>Arbeitsaufgaben</strong> mit <strong><a href="/uwp/api/windows.system.threading.threadpool.runasync">runasync</a></strong>an den <strong>Thread Pool</strong> senden. Sie können einen Timer verwenden, um eine Arbeitsaufgabe mit " <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createtimer">kreatetimer</a></strong> " zu übermitteln und eine sich wiederholende Arbeitsaufgabe mit " <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">kreateperiodictimer</a></strong>" zu erstellen.<br/><br/><a href="/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">Senden ein Arbeitselement an den Threadpool</a><br/><br/><a href="/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">Timergesteuertes Übermitteln einer Arbeitsaufgabe</a><br/><br/><a href="/windows/uwp/threading-async/create-a-periodic-work-item">Erstellen ein regelmäßiges Arbeitselement</a><br/><br/><a href="/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">Bewährte Methoden zum Verwenden des Threadpools</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Asynchrone Programmierung.</strong> <br><br>Vermeiden Sie komplexes Threading, indem Sie mit Mustern der asynchronen Programmierung die Reaktionsfähigkeit des UI-Threads aufrechterhalten.</td>
<td align="left">Zum Erstellen eigener asynchroner Klassen ist <strong>die Verwendung von Threading erforderlich</strong>. Einige integrierte Klassen sind asynchron.</td>
<td align="left">Zum Erstellen eigener asynchroner Klassen ist <strong>die Verwendung von Threading erforderlich</strong>. Einige integrierte Klassen sind asynchron.</td>
<td align="left">Sie können asynchrone Muster verwenden, um zu vermeiden, dass der Haupt Thread blockiert wird, wenn Sie Ihre eigenen APIs erstellen, z <strong></strong> . b. asynchrone und in c# zu <strong>erwartende und Visual Basic</strong> . Sie können die integrierten asynchronen APIs verwenden, die mit dem Wort <strong>Async</strong> enden.<br/><br/><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Asynchrone Programmierung</a><br/><br/><a href="/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">Aufrufen asynchroner APIs in C# oder Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Listen Ansichts Optimierung.</strong> <br><br>Integrierte Muster zum Optimieren von Datenlisten, die häufig eine geringe Leistung aufweisen, wenn große Mengen von Daten angezeigt werden müssen.</td>
<td align="left">Das <strong>ViewHolder</strong>-Entwurfsmuster wird verwendet, um mehrfache Ansichtssuchvorgänge zu vermeiden, sodass Sie wiederverwendbare UI-Elemente verwenden können.</td>
<td align="left">Die Leistung von <strong>UITableView</strong> kann mit einer Reihe von Optimierungen verbessert werden, es ist jedoch keine Optimierung integriert.</td>
<td align="left">Sie können das <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a>- und das <a href="/uwp/api/windows.ui.xaml.controls.gridview">GridView</a>-Steuerelement verwenden. Diese bieten bereits <strong>UI-Virtualisierung</strong> für gleichmäßiges Schwenken und gleichmäßigen Bildlauf sowie schnelleres Starten. Sie können auch <a href="/dotnet/api/system.collections.ilist">IList</a> und <a href="/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> in der Datenquelle implementieren, um <strong>Datenvirtualisierung</strong> bereitzustellen und die Leistung zusätzlich zu verbessern.<br/><br/><a href="/windows/uwp/debug-test-perf/optimize-gridview-and-listview">Optimieren der ListView- und GridView-Benutzeroberfläche</a><br/><br/><a href="/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">Virtualisierung von ListView- und GridView-Daten</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monetisierung</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>In-App-Käufe.</strong> <br><br>Plattformfeatures ermöglichen es den Benutzern, in Ihren Apps Käufe durchzuführen.</td>
<td align="left">Google-Dienste bieten <strong>In-App-Abrechnung</strong>. Produkte werden der <strong>Google Play Developer Console</strong> hinzugefügt. In-App-Käufe werden mit der <strong>Google Play Billing Library</strong> implementiert.</td>
<td align="left">Produkte werden <strong>iTunes Connect</strong> hinzugefügt. In-App-Käufe werden mit dem <strong>StoreKit</strong>-Framework implementiert.<br/><br/>Produkte werden mithilfe von <strong>SKMutablePayment</strong> und <strong>SKPaymentQueue</strong> gekauft.</td>
<td align="left">Sie erstellen In-App-Produktkäufe für Ihre App, indem Sie <a href="/windows/uwp/publish/iap-submissions">sie Ihrer App hinzufügen und an den Store übermitteln</a>. <br/><br/>Sie definieren In-App-Käufe mit der <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp">CurrentApp-Klasse</a></strong>. <br/><br/>Mit <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp.RequestProductPurchaseAsync</a></strong> zeigen Sie die Benutzeroberfläche an, auf der Kunden das Produkt kaufen können.<br/><br/><a href="/windows/uwp/monetize/enable-in-app-product-purchases">Unterstützen des Kaufs von In-App-Produkten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Nutzbare in-App-Käufe.</strong> <br><br>In-App-Produkte, die gekauft, verwendet und dann erneut gekauft werden können.</td>
<td align="left">Käufe von Verbrauchsartikeln werden aktiviert, indem ein regulärer Kauf getätigt wird und das Produkt dann mit <strong>consumePurchase</strong> verbraucht wird, sodass es gekauft, verwendet und dann erneut gekauft werden kann.</td>
<td align="left">Verbrauchsartikel sind in iTunes Connect <strong>als Verbrauchsartikel definiert</strong>.</td>
<td align="left">Sie unterstützen Verbrauchsartikel, indem Sie <a href="/windows/uwp/publish/enter-iap-properties">ihren Produkttyp als „Verbrauchsartikel“ definieren, wenn Sie sie an den Store übermitteln</a>. Sie können dann <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">currentapp. reportconsumableerfüllllmentasync</a></strong> aufrufen, nachdem ein verbrauchbarer Kauf erfolgt ist, damit der Kunde darauf zugreifen kann.<br/><br/><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Unterstützen von Endverbraucher-In-App-Käufen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Testen von in-App-Käufen.</strong> <br><br>Ermöglichen es Ihnen, den Code für In-App-Käufe zu testen, ohne Ihre App an den Store zu übermitteln.</td>
<td align="left">Für Tests wird die <strong>In-app Billing Sandbox</strong> verwendet.</td>
<td align="left">Für Tests werden <strong>Sandbox-Testerkonten</strong> verwendet.</td>
<td align="left">Sie können in-App-Käufe testen, indem Sie einfach die <strong><a href="/uwp/api/windows.applicationmodel.store.currentappsimulator">currentappsimulator</a></strong> -Klasse anstelle von currentapp verwenden.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Zeit.</strong> <br><br>Ermöglichen es Ihnen, für die Testversion einer App einfach den Inhalt zu beschränken oder Werbung zu entfernen.</td>
<td align="left"><strong>App-Testversionen werden von Google Play nicht offiziell unterstützt</strong>. Für Testversionen oder das Entfernen von Werbung wird ein In-App-Kauf erstellt und der entsprechende Codepfad verwendet, wenn der Kauf erfolgreich bestätigt wurde.</td>
<td align="left"><strong>App-Testversionen werden vom App Store nicht offiziell unterstützt</strong>. Für Testversionen oder das Entfernen von Werbung wird ein In-App-Kauf erstellt und der entsprechende Codepfad verwendet, wenn der Kauf erfolgreich bestätigt wurde.</td>
<td align="left">Sie können eine kostenlose Testversion Ihrer App anbieten, indem Sie beim Übermitteln der App an den Store die <strong><a href="/windows/uwp/publish/set-app-pricing-and-availability">Option „Kostenlose Testversion“</a></strong> verwenden. Anschließend können Sie mit <strong><a href="/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation.IsTrial</a></strong> den Teststatus der App überprüfen und entsprechend andere Codepfade angeben. Sie können sich für das <a href="/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">LicenseChanged-Ereignis</a> registrieren, um benachrichtigt zu werden, wenn der Benutzer während der Ausführung der App den Teststatus ändert.<br/><br/><a href="/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">Ausschließen oder Einschränken von Features in einer Testversion</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Anpassen an mehrere Plattformen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Adaptive Benutzeroberfläche: flexible Layouts.</strong> <br><br>Unterstützen unterschiedlicher Bildschirmgrößen mit flexibler Höhe und Breite.</td>
<td align="left">Flexible Layouts lassen sich mit den Werten <strong>wrap_content</strong> und <strong>match_parent</strong> in LinearLayout-Objekten oder durch Verwendung von RelativeLayout-Objekten für die Ausrichtung erstellen.</td>
<td align="left">Flexible Layouts können unter Verwendung des <strong>adaptiven Modells</strong> mit universellen Storyboards erstellt werden, wobei <strong>Autolayout</strong> mit <strong>Constraints</strong> und <strong>Traits</strong>, z. B. horizontalSizeClass und displayScale, die auf Ansichtscontroller angewendet werden, genutzt wird.</td>
<td align="left">Mit <strong>Layouteigenschaften</strong> und <strong>Panels</strong> sowie einer Kombination von festen und dynamischen Größen erstellen Sie ein dynamisches Layout.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definieren von Layouts mit XAML – Layouteigenschaften und -panels</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Reaktionsfähiges Design für Apps für die universelle Windows Platform (UWP) – Grundlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Adaptive Benutzeroberfläche: angepasste Layouts.</strong> <br><br>Unterstützung unterschiedlicher Bildschirmgrößen mit eigenen Ziellayouts.</td>
<td align="left">Durch die Bereitstellung alternativer Layoutdateien für unterschiedliche Bildschirmkonfigurationen im Verzeichnis „resources“ mit <strong>Konfigurationsqualifizierern</strong>, z. B. <strong>small</strong>, <strong>large</strong>, <strong>ldpi</strong> und <strong>hdpi</strong>, können Sie benutzerdefinierte Layouts auf Bildschirme unterschiedlicher Größe und Pixeldichte anwenden.</td>
<td align="left">Definieren Sie ein <strong>jeweils eigenes iPhone- und iPad-Storyboard</strong>, um Layouts in einer universellen App an unterschiedliche Gerätefamilien anzupassen.</td>
<td align="left">Sie können ein maßgeschneidertes Layout erstellen, indem Sie <strong>unterschiedliche XAML-Markupdateien</strong> pro Gerätefamilie definieren.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definieren von Layouts mit XAML – Maßgeschneiderte Layouts</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Adaptive Benutzeroberfläche: reaktionsfähige Layouts.</strong> <br><br>Reaktion auf Änderungen der Bildschirmgröße, z. B. Drehung, oder eine Änderung der Fenstergröße.</td>
<td align="left">Dynamische Layouts werden durch die Verwendung flexibler Layouts mit <strong>LinearLayout</strong> und <strong>RelativeLayout</strong> oder das Bereitstellen alternativer Layoutdateien für unterschiedliche Ausrichtungen ermöglicht.</td>
<td align="left">Wenn sich die <strong>Größe</strong> oder <strong>Traits</strong> einer Ansicht ändern, werden die in Storyboards angegebenen <strong>Constraints</strong> angewendet.</td>
<td align="left">Mit <strong><a href="/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> und <strong><a href="/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong> können Sie ganz einfach als Reaktion auf Änderungen der Fenstergröße zur Laufzeit Abschnitte der Benutzeroberfläche dynamisch umbrechen, neu positionieren, anzeigen, ersetzen oder ihre Größe ändern.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Definieren von Layouts mit XAML – Visuelle Zustände und Zustandsauslöser</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Reaktionsfähiges Design für Apps für die universelle Windows Platform (UWP) – Grundlagen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Unterstützung verschiedener Gerätefunktionen.</strong> <br><br>Nutzen Sie erweiterte Hardwarefeatures, während Geräte, die diese nicht aufweisen, trotzdem unterstützt werden.</td>
<td align="left">Durch das Testen von Gerätefeatures zur Laufzeit mit <strong>PackageManager.hasSystemFeature</strong> können Sie bestimmen, ob hardwarespezifischer Code ausgeführt werden kann.</td>
<td align="left">Es gibt <strong>keine einzelne Prüfung</strong>, mit der Sie zur Laufzeit Gerätefeatures testen können. Stattdessen müssen Sie für jedes Feature spezielle Tests ausführen, um zu bestimmen, ob hardwarespezifischer Code ausgeführt werden kann.</td>
<td align="left">Sie können für die Berücksichtigung zusätzlicher Funktionen in unterschiedlichen Gerätefamilien, einschließlich Smartphone, Desktop und IoT, dem Paket <strong>Plattformerweiterungs-SDKs</strong> hinzufügen. Mit der <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">ApiInformation-API</a></strong> testen Sie zur Laufzeit, ob Typen und Member vorhanden sind, und rufen diese nur auf, wenn sie vorhanden sind.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Unterstützung verschiedener Gerätefunktionen.</strong> <br><br>Nutzen Sie erweiterte Hardwarefeatures, während Geräte, die diese nicht aufweisen, trotzdem unterstützt werden.</td>
<td align="left">Die <strong>Android Support Library</strong> kann Ihrer App hinzugefügt werden, um neuere APIs für Geräte verfügbar zu machen, auf denen ältere Versionen von Android ausgeführt werden. Das Testen der API-Ebene zur Laufzeit kann mit <strong>Build.Version.SDK_INT</strong> erfolgen.</td>
<td align="left">Mit Standardlaufzeitprüfungen lässt sich ermitteln, ob APIs verfügbar sind. Zum Beispiel kann mit der <strong>class</strong>-Methode überprüft werden, ob eine Klasse vorhanden ist, und mit <strong>respondsToSelector:</strong> lässt sich überprüfen, ob Methoden in Klassen vorhanden sind.</td>
<td align="left">Mithilfe von " <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">apiinformation. isapikontratpresent</a></strong> " können Sie ermitteln, ob ein API-Vertrag mit einer angegebenen Haupt-und neben Zahl vorhanden ist. Mit der <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">ApiInformation-API</a></strong> testen Sie außerdem zur Laufzeit, ob Typen und Member vorhanden sind, und rufen diese nur auf, wenn sie vorhanden sind.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Benachrichtigungen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Kacheln und Zeichen.</strong> <br><br>Präsentation von Updates für Benutzer auf dem Startbildschirm.</td>
<td align="left"><strong>App-Widgets</strong> sind Ansichten in Ihrer Anwendung, die auf den Startbildschirm eingebettet werden können und regelmäßige Aktualisierungen empfangen können. Unter Android ist <strong>kein Badge-System</strong> vorhanden. Es ist kein mit Kacheln identisches System vorhanden.</td>
  <td align="left"><strong>Widgets</strong> für IOS mapbirnen im Benachrichtigungs Center und werden als <strong>App-Erweiterungen</strong>implementiert. Sie können dem Symbol auch einen <strong>Badge</strong> mit einer Zahl hinzufügen, die sich in Reaktion auf lokale Benachrichtigungen und Remote Benachrichtigungen ändern kann. Es ist kein Kacheln System vorhanden.</td>
<td align="left">Ihre App verfügt über eine <strong>Kachel</strong>, die an den Startbildschirm angeheftet werden kann und in der Text und Bilder Ihrer Wahl sowie ein <strong>Signal</strong> mit Glyphen und Zahlen angezeigt werden. Sie können den Inhalt der Kacheln über die App aktualisieren, entweder durch Pushbenachrichtigungen oder zu vordefinierten Zeitpunkten. Kacheln können adaptiv sein und sich abhängig davon ändern, wo sie angezeigt werden.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Erstellen von Kacheln</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">Erstellen adaptiver Kacheln</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Auswählen einer Methode für die Übermittlung von Benachrichtigungen</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Richtlinien für Kacheln und Signale</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Anzeigen von Benachrichtigungen</strong> <br><br>Typen von Benachrichtigungen, die angezeigt werden können.</td>
<td align="left">Benachrichtigungen können im <strong>Benachrichtigungsbereich</strong> und in der <strong>Benachrichtigungsschublade</strong> angezeigt werden. Bei <strong>Heads-Up-Benachrichtigungen</strong> handelt es sich um Benachrichtigungen in einem kleinen unverankerten Fenster. Den Benachrichtigungen können durch Definieren eines <strong>PendingIntent</strong> Aktionen hinzugefügt werden.</td>
<td align="left">Popupbenachrichtigungen werden als <strong>Banner</strong> oder <strong>Warnungen</strong> angezeigt. Sie können <strong>interaktiven Benachrichtigungen</strong>, die mit <strong>UIMutableUserNotificationAction</strong> definiert werden, benutzerdefinierte Aktionsschaltflächen hinzufügen.</td>
<td align="left">Sie können adaptive <strong>Popupbenachrichtigungen</strong> hinzufügen. Sie können in XML Popups mit visuellem Inhalt, mit <strong>Aktionen</strong>, bei denen es sich um Schaltflächen handeln kann, oder mit Eingaben und Audioinhalten definieren.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Adaptive und interaktive Popupbenachrichtigungen</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Auswählen einer Methode für die Übermittlung von Benachrichtigungen</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">Richtlinien für Popupbenachrichtigungen</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Lokale Benachrichtigungen werden geplant.</strong> <br><br>Lokale Benachrichtigungen, die von der App zu einem geplanten Zeitpunkt gesendet werden.</td>
<td align="left">Benachrichtigungen und Aktionen werden mit einem <strong>NotificationCompat.Builder</strong> definiert. Sie können mit <strong>AlarmManager</strong> und <strong>BroadcastReceiver</strong> in der App geplant und behandelt werden.</td>
<td align="left">Lokale Benachrichtigungen werden mithilfe von <strong>uilocalnotification</strong>erstellt und können mit " <b> uilocalnotification. schedulelocalnotification:. |" geplant werden<strong>. Sie können eine Popup Benachrichtigung mithilfe </strong>von <a href="/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">scheduledtoastnotification</a>planen<strong>. Sie können eine Kachel Benachrichtigung über die </strong> <a href="/uwp/api/Windows.UI.Notifications.TileNotification">tilenotificlass</a>von Ihrer APP senden <strong> oder eine Kachel Benachrichtigung mit <a href="/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">scheduledtilenotifiplanen</a>.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Adaptive und interaktive Popupbenachrichtigungen</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">Benachrichtigung über lokale Kachel senden</a> | | </strong>Senden von Pushbenachrichtigungen.</b> Von einem Pushbenachrichtigungsserver wird eine Benachrichtigung gesendet, die optional in der App behandelt wird.</td>
<td align="left"><strong>Google Cloud Messaging</strong> unterstützt Pushbenachrichtigungen für Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Medienaufzeichnung und -rendering</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Erfassungs Medien.</strong> <br><br>Aufzeichnen von Audio- und visuellen Inhalten.</td>
<td align="left">Mithilfe eines <strong>Intents</strong>, z. B. MediaStore.ACTION_VIDEO_CAPTURE, können Medien mit einer vorhandenen Kamera-App aufgezeichnet werden. Die Bibliothek <strong>android.hardware.camera2</strong> oder <strong>camera</strong> ermöglicht die Implementierung einer benutzerdefinierten Kameraschnittstelle. Für die Audioaufzeichnung können <strong>MediaRecorder</strong>-APIs verwendet werden.</td>
<td align="left">Der <strong>UIImagePickerController</strong> ermöglicht das Aufzeichnen von Videos und Fotos mit der Benutzeroberfläche des Systems. Die <strong>AVFoundation</strong>-Klassen, z. B. <strong>AVCaptureSession</strong>, aktivieren den direkten Zugriff auf die Kamera. <br/>Die <strong>AVAudioRecorder</strong>-Klasse ermöglicht die Audioaufzeichnung.</td>
<td align="left">Sie können Fotos und Videos aufzeichnen, während Sie die integrierte Kamera-Benutzeroberfläche mit der <strong><a href="/uwp/api/Windows.Media.Capture.CameraCaptureUI">cameracaptureui-Klasse</a></strong>verwenden. Sie können auf einer niedrigen Ebene mit der Kamera interagieren und Klassen in <strong><a href="/uwp/api/Windows.Media.Capture">Windows.Media.Capture</a></strong>, z. B. die <strong><a href="/uwp/api/Windows.Media.Capture.MediaCapture">MediaCapture-API</a></strong>, für die Audioaufzeichnung verwenden. <br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">Aufnehmen von Fotos und Videos mit CameraCaptureUI</a><br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">Aufnehmen von Fotos und Videos mit MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Medienwiedergabe.</strong> <br><br>Wiedergeben von Audio- und Videodateien.</td>
<td align="left">Zum Wiedergeben von Audio- und Videodateien werden die <strong>MediaPlayer</strong>-Klasse und die <strong>AudioManager</strong>-Klasse verwendet.</td>
<td align="left">Zum Wiedergeben von Audio- und Videodateien werden das <strong>AVKit-Framework</strong>, <strong>AVAudioPlayer</strong> und das <strong>Media Player-Framework</strong> verwendet.</td>
<td align="left">Sie können mit den Klassen <strong><a href="/uwp/api/Windows.Media.Core.MediaSource">MediaSource</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong> und <strong><a href="/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> Audio- und Videodateien aus Quellen, z. B. lokalen und Remotedateien, wiedergeben.<br/><br/><a href="/windows/uwp/audio-video-camera/media-playback-with-mediasource">Medienwiedergabe mit „MediaSource“</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Medien werden bearbeitet.</strong> <br><br>Erstellen neuer Mediendateien aus vorhandenen Aufzeichnungen und Anwenden von Spezialeffekten.</td>
<td align="left">Zum Bearbeiten von Inhalten können Klassen niedriger Ebene, z. B. <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> und <strong>android.media.effect</strong>, verwendet werden.</td>
<td align="left">Zum Bearbeiten von Inhalten können Klassen im <strong>AV Foundation</strong>-Framework, z. B. <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> und <strong>AVMutableAudioMix</strong>, verwendet werden.</td>
<td align="left">Sie können die <strong><a href="/uwp/api/windows.media.editing">Windows. Media. Editing</a></strong> -APIs, wie z. b. <strong><a href="/uwp/api/windows.media.editing.mediacomposition">mediacomposition</a></strong> und <strong><a href="/uwp/api/windows.media.editing.mediaclip">Mediaclip</a></strong> , verwenden, um Medien Kompositionen aus Audiodateien und Videodateien zu erstellen. Sie können Video- und Bildüberlagerungen hinzufügen, Videoclips kombinieren, Hintergrundaudio hinzufügen und Audio- und Videoeffekte anwenden.<br/><br/><a href="/windows/uwp/audio-video-camera/media-compositions-and-editing">Medienkompositionen und -bearbeitung</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Sensoren</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sensoren.</strong> <br><br>Erkennen der Bewegung, Position und Umwelteigenschaften des Geräts.</td>
<td align="left">Das <strong>Sensor-Framework</strong> wird für den Zugriff auf Hardware- und Softwaresensoren mit Klassen, z. B. <strong>SensorManager</strong> und <strong>SensorEvent</strong>, verwendet.</td>
<td align="left">Das <strong>Core Motion Framework</strong> wird für den Zugriff auf Sensorrohdaten und verarbeitete Sensordaten verwendet.</td>
<td align="left">Sie können Klassen in <strong><a href="/uwp/api/windows.devices.sensors">Windows.Devices.Sensors</a></strong> für den Zugriff auf Sensorwerte und Ereignisse verwenden, die ausgelöst werden, wenn der Sensor neue Messungsdaten empfängt.<br/><br/><a href="/windows/uwp/devices-sensors/sensors">Sensoren</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Position und Kartenfunktionen</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Hotels.</strong> <br><br>Suchen der <strong>aktuellen</strong> Position des Geräts und Nachverfolgen von <strong>Änderungen</strong>.</td>
<td align="left">Die Standort-APIs der Google Play-Dienste bieten mit dem <strong>Fused Location Provider</strong> unter Verwendung der <strong>getLastLocation</strong>-Methode und der <strong>requestLocationUpdates</strong>-Methode High-Level-Zugriff auf die <strong>letzte bekannte Position</strong>. Low-Level-Zugriff wird in den Android-Bibliotheken mit dem <strong>LocationManager</strong> bereitgestellt.</td>
<td align="left">Die <strong>Core Location</strong> <strong>CLLocationManager</strong>-Klasse wird zum Überwachen der Position eines Geräts verwendet, mit <strong>startUpdatingLocation</strong> für den Standardstandortdienst und <strong>startMonitoringSignificantLocationChanges</strong> für den <strong>significant-change</strong>-Standortdienst.</td>
<td align="left">Sie können die Geräteposition mit Klassen in <strong><a href="/uwp/api/windows.devices.geolocation">Windows.Devices.Geolocation</a></strong> nachverfolgen. Verwenden Sie <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">GeoLocator. getgeopositionasync</a></strong> für einen einmaligen Lesevorgang. Verwenden Sie <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.positionchanged">Geolocator.PositionChanged</a></strong>, um die Position mithilfe eines Timers regelmäßig abzurufen oder um informiert zu werden, wenn sich die Position ändert.<br/><br/><a href="/windows/uwp/maps-and-location/get-location">Abrufen der Position eines Benutzers</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Anzeigen von Zuordnungen.</strong> <br><br>Anzeigen einer <strong>interaktiven integrierten Karte</strong> und Hinzufügen von <strong>interessanten Orten</strong>.</td>
<td align="left">Die Klassen <strong>GoogleMap</strong>, <strong>MapFragment</strong> und <strong>MapView</strong> in der <strong>Google Maps Android API</strong> ermöglichen das Einbetten von Karten in Apps. Interessante Orte können mit <strong>Markern</strong> und der anpassbaren <strong>Marker</strong>-Klasse angezeigt werden.</td>
<td align="left">Karten werden mit der <strong>MKMapView</strong>-Klasse im <strong>MapKit-Framework</strong> in iOS-Apps eingebettet. Mit Objektklassen, z. B. <strong>MKPointAnnotation</strong>, und Ansichtsklassen, z. B. <strong>MKPinAnnotationView</strong>, können den Apps <strong>Anmerkungen</strong> hinzugefügt werden, um interessante Orte anzuzeigen.</td>
<td align="left">Sie können Karten in Ihre apps einbetten, indem Sie das integrierte <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">mapcontrol</a></strong> -XAML-Steuerelement verwenden, das 2D-, 3D-und Streetside-Ansichten bereitstellt. Mithilfe von Klassen wie <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapicon">mapicon</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolygon">MapPolygon</a></strong> und <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolyline">mappolyline</a></strong>können Sie interessante Punkte mit einem Pushpin, einem Bild oder einer Form hinzufügen.<br/><br/><a href="/windows/uwp/maps-and-location/display-maps">Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten</a><br/><br/><a href="/windows/uwp/maps-and-location/display-poi">Anzeigen von interessanten Orten (POI) auf einer Karte</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Geofencing.</strong> <br><br>Überwachen des Betretens und Verlassens einer bestimmten geografischen Region.</td>
<td align="left">Geofences werden mit den <strong>Location Services</strong> im Google Play Services SDK überwacht.</td>
<td align="left">Regionen werden mit der <strong>CLCircularRegion</strong>-Klasse überwacht und mit der <strong>CLLocationManager.startMonitoringForRegion:</strong>-Methode registriert.</td>
<td align="left">Sie können einen Geofence befindet mit der <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofence">Geofence befindet</a></strong> -Klasse erstellen und ihre über <strong>wachten Zustände</strong> definieren, wie z. b. das eingeben oder belassen einer Region. Behandeln Sie Geofence befindet-Ereignisse im Vordergrund mit der <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">geofencemonitor-Klasse</a></strong>und im Hintergrund mit der <strong><a href="/uwp/api/windows.applicationmodel.background.locationtrigger">locationtriggerhintergrundklasse</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/set-up-a-geofence">Einrichten eines Geofence</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Geocodierung und umgekehrte Geocodierung.</strong> <br><br>Das Umwandeln von Adressen in geografische Standorte (Geocodierung) und das Umwandeln geografischer Standorte in Adressen (umgekehrte Geocodierung).<br/></td>
<td align="left">Für Geocodierung und umgekehrte Geocodierung wird die <strong>Geocoder</strong>-Klasse verwendet.</td>
<td align="left">Für Geocodierung wird die <strong>CLGeocoder</strong>-Klasse verwendet.</td>
<td align="left">Sie können die Geocodierung mithilfe der <strong><a href="/uwp/api/windows.services.maps.maplocationfinder">maplocationfinder-Klasse</a></strong> in <strong><a href="/uwp/api/windows.services.maps">Windows. Services. Maps</a></strong>durchführen. Sie verwenden <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> für Geocodierung und <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> für umgekehrte Geocodierung.<br/><br/><a href="/windows/uwp/maps-and-location/geocoding">Durchführen der Geocodierung und umgekehrten Geocodierung</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Routen und Anweisungen.</strong> <br><br>Bereitstellen von Routen und Entfernungen zwischen zwei geografische Standorten und der entsprechenden Wegbeschreibungen.</td>
<td align="left">Google bietet den Webdienst <strong>Google Maps Directions API</strong>, der in Android verwendet werden kann, obwohl kein SDK verfügbar ist.</td>
<td align="left">MapKit stellt die <strong>MKDirections</strong>-API bereit, mit der Informationen zu einer Route und Wegbeschreibungen abgerufen werden können.</td>
<td align="left">Sie können mit der <strong><a href="/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong>-Klasse in <strong><a href="/uwp/api/windows.services.maps">Windows.Services.Maps</a></strong> eine Fußgänger -oder Autoroute anfordern. Routen werden als <strong><a href="/uwp/api/windows.services.maps.maproute">MapRoute</a></strong>-Instanz zurückgegeben, die einfach in einem MapControl angezeigt werden kann. Wegbeschreibungen werden im <strong><a href="/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong>-Objekt zurückgegeben.<br/><br/><a href="/windows/uwp/maps-and-location/routes-and-directions">Anzeigen von Routen und Wegbeschreibungen auf einer Karte</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">App-zu-App-Kommunikation</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Aufrufen einer anderen APP.</strong> <br><br>Starten einer anderen App und optionales Freigeben von Daten, z. B. Links, Text, Fotos, Videos und Dateien.</td>
<td align="left">Zum Starten einer anderen App wird ein <strong>impliziter Intent</strong> verwendet. Dabei werden eine <strong>Action</strong> und optionale Daten in einem <strong>Intent</strong> definiert und mit <strong>startActivityForResult</strong> aufgerufen.<br/></td>
<td align="left">Mit <strong>App-Erweiterungen</strong> kann der Zugriff auf App-Daten für eine andere App bereitgestellt werden. <strong>URL-Schemas</strong> ermöglichen die Übergabe einer URL an eine andere app.</td>
<td align="left">Sie können eine andere app starten, die für einen URI mit " <strong><a href="/uwp/api/windows.system.launcher.launchuriasync">Launcher. launchuriasync</a></strong>" oder " <strong><a href="/uwp/api/windows.system.launcher.launchuriforresultsasync">Launcher. launchuriforresultasync</a></strong> " registriert ist, um die Ergebnisse zu starten und die Daten aus der gestarteten app zurück zu erhalten. Sie können mit <strong><a href="/uwp/api/windows.system.launcher.launchfileasync">Launcher.LaunchFileAsync</a></strong> eine Datei an eine andere App übergeben, damit sie von dieser behandelt wird.<br/><br/>Zum Freigeben von Daten zwischen Apps können Sie einfach einen <strong>Freigabe-Vertrag</strong> verwenden.<br/><br/><a href="/windows/uwp/launch-resume/launch-default-app">Starten der Standard-App für einen URI</a><br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Starten einer App für Ergebnisse</a><br/><br/><a href="/windows/uwp/launch-resume/launch-the-default-app-for-a-file">Starten der Standard-App für eine Datei</a><br/><br/><a href="/windows/uwp/app-to-app/share-data">Freigeben von Daten</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Zulassen, dass Ihre APP aufgerufen wird.</strong> <br><br>Zulassen, dass Ihre App auf eine Anforderung von einer anderen App antwortet.</td>
<td align="left">Apps registrieren eine <strong>Intent-Behandlungsaktivität</strong> bei einem <strong>Intent-Filter</strong>, um auf einen impliziten Intent von einer anderen App zu reagieren.</td>
<td align="left">Durch das Verpacken einer <strong>App-Erweiterung</strong> können Daten für andere Apps freigegeben werden. Apps können mit dem Schlüssel <strong>CFBundleURLTypes</strong> in „Info.plist“ ein <strong>benutzerdefiniertes URL-Schema</strong> registrieren.</td>
<td align="left">Sie können Ihre App als Standardhandler für einen <strong>URI-Schemanamen</strong> registrieren, indem Sie ein <strong><a href="/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">Protokoll</a></strong> im Paketmanifest registrieren und den <strong><a href="/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong>-Ereignishandler aktualisieren. Optional werden Ergebnisse zurückgegeben. Auf die gleiche Weise können Sie Ihre App als Standardhandler für bestimmte Dateitypen registrieren, indem Sie im Paketmanifest eine Deklaration hinzufügen und das <strong><a href="/uwp/api/windows.ui.xaml.application.onfileactivated">Application.OnFileActivated</a></strong>-Ereignis behandeln.<br/><br/>Sie können Freigabe Vertragsanforderungen verarbeiten, indem Sie Ihre APP als Freigabe Ziel im Manifest registrieren und das <strong><a href="/uwp/api/windows.ui.xaml.application.onsharetargetactivated">Application. onsharetargetaktivierte</a></strong> -Ereignis behandeln.<br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Starten einer App für Ergebnisse</a><br/><br/><a href="/windows/uwp/launch-resume/handle-file-activation">Behandeln der Dateiaktivierung</a><br/><br/><a href="/windows/uwp/app-to-app/receive-data">Empfangen von Daten</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Kopieren und einfügen.</strong> <br><br>Kopieren und Einfügen von Text und anderen Inhalten zwischen Apps.</td>
<td align="left">Das <strong>Clipboard Framework</strong> kann verwendet werden, um Kopieren und Einfügen mit der <strong>ClipboardManager</strong>-Klasse und der <strong>ClipData</strong>-Klasse zu implementieren.</td>
<td align="left">Zum Implementieren von Kopieren und Einfügen können <strong>UIPasteboard</strong>, <strong>UIMenuController</strong> und <strong>UIResponderStandardEditActions</strong> verwendet werden.</td>
<td align="left">Viele Standard-XAML-Steuerelemente unterstützen bereits Kopieren und Einfügen. Sie können mithilfe der <strong><a href="/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong>-Klasse und der <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong>-Klasse in <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer">Windows.ApplicationModel.DataTransfer</a></strong> Kopieren und Einfügen selbst implementieren.<br/><br/><a href="/windows/uwp/app-to-app/copy-and-paste">Kopieren und Einfügen</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Drag & Drop.</strong> <br><br>Drag & Drop von Inhalten zwischen Apps.</td>
<td align="left">Drag & Drop kann mit dem <strong>Android Drag/Drop Framework</strong> in einer einzelnen Anwendung implementiert werden.</td>
<td align="left">Von iOS werden keine allgemeinen Drag & Drop-APIs bereitgestellt.</td>
<td align="left">Sie können in der App Drag & Drop implementieren, um Drag & Drop-Funktionen von App zu App, von Desktop zu App und von App zu Desktop zu ermöglichen. Sie können in der UIElement-Klasse mit der <strong><a href="/uwp/api/windows.ui.xaml.uielement.allowdrop">AllowDrop</a></strong>-Eigenschaft und der <strong><a href="/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong>-Eigenschaft sowie mit dem <strong><a href="/uwp/api/windows.ui.xaml.uielement.dragover">DragOver</a></strong>-Ereignis und dem <strong><a href="/uwp/api/windows.ui.xaml.uielement.drop">Drop</a></strong>-Ereignis Unterstützung für Drag & Drop implementieren.<br/><br/><a href="/windows/uwp/app-to-app/drag-and-drop">Drag & Drop</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Softwaredesign</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Allgemeines Konzept</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>Windows 10-UWP</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Software Entwurfsmuster.</strong> <br><br>Empfohlene oder perfekt verwendete Muster für die Plattform.</td>
<td align="left">Für die Android-Entwicklung wurde kein formales Musters empfohlen oder bereitgestellt, jedoch ermöglicht eventuell die Betaversion des Data Binding Frameworks die häufigere Verwendung des <strong>Model-View-ViewModel (MVVM)</strong>-Musters. In einer Reihe von Drittanbieterartikeln und -Frameworks werden die Ansätze <strong>Model-View-Presenter (MVP)</strong> und <strong>MVVM</strong> empfohlen.</td>
<td align="left"><strong>Model-View-Controller (MVC)</strong> ist ein häufig verwendetes Muster für iOS und in die Plattform integriert.</td>
<td align="left">Sie sind beim Softwaredesign für UWP nicht auf ein bestimmtes Muster beschränkt.<br/><br/>Sie können das integrierte <a href="/windows/uwp/data-binding/index">Datenbindungsmuster</a> verwenden, um eine deutliche Trennung von Datenaspekten und UI-Aspekten sicherzustellen und zu vermeiden, dass Sie UI-Ereignishandler programmieren müssen, die Eigenschaftswerte aktualisieren.<br/><br/>Sie können die Datenbindung auf die Befolgung des <strong>Model-View-ViewModel (MVVM)</strong>-Musters erweitern, indem Sie MVVM-Bibliotheken von Drittanbietern, z. B. das <a href="https://archive.codeplex.com/?p=mvvmlight">MVVM Light Toolkit</a>, nutzen, oder eine eigene Bibliothek verwenden und Logik von der CodeBehind-Datei fernhalten.<br/><br/><a href="/previous-versions/msp-n-p/hh848246(v=pandp.10)">Das MVVM-Muster</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Visual Studio-Template 10-Projektvorlagen</a></td>
</tr>
</tbody>
</table>