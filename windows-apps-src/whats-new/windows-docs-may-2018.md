---
title: 'Neuerungen in Windows-Dokumentation im Mai 2018: Entwickeln von UWP-apps'
description: Neue Features, Videos und Anleitungen für Entwickler haben die Windows 10-Entwicklerdokumentation für Mai 2018 und der Microsoft Build-Konferenz hinzugefügt wurde.
keywords: Was ist neu, zu aktualisieren, Funktionen, Anleitung für Entwickler, Windows 10, Mai, Build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69df2bbe8bc91fcf4a2631c0f257fc44851c24f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598785"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Was ist neu in der Windows-Entwickler-Dokumentation im Mai 2018

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Anleitung für Entwickler, Videos und Beispiele wurden im Monat Mai an mit der [Microsoft Build 2018](https://www.microsoft.com/build) -Entwicklerkonferenz.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="motion-in-fluent-design"></a>Während der Übertragung in Fluent-Entwurf

Der Benutzer während der Übertragung in die Fluent-Entwurfssystem wird weiterentwickelt, auf die Grundlagen der zeitlichen Steuerung, Übergängen, die direktionalität und Schwerkraft basiert. Unterstützt den Benutzer über Ihre app und verbindet sie mit ihrer digitalen-Erfahrung durch das Spiegeln der natürlichen Welt diese Grundlagen anwenden. Weitere Informationen finden Sie in diesem Artikel:

* [Die Motion-Übersicht](../design/motion/index.md) wurde aktualisiert, um diese Grundlagen widerzuspiegeln.
* [In der Praxis Motion](../design/motion/motion-in-practice.md) enthält Beispiele, wie diese Grundlagen in Ihrer app angewendet.
* [Direktionalität und die Schwerkraft](../design/motion/directionality-and-gravity.md) verdichtet Begriffsverständnis des Benutzers, der Ihre app.
* [Zeitsteuerungssystem und vereinfachen der](../design/motion/timing-and-easing.md) Realismus verleihen, die während der Übertragung in Ihrer app hinzugefügt.

![Während der Übertragung in Aktion](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Designupdates

Visuelle Updates und geringen Änderungen wurden an den folgenden Seiten der Fluent-Entwurf vorgenommen:

* [Ausrichtung, zeichenauffüllung, Ränder](../design/layout/alignment-margin-padding.md)
* [Farbe](../design/style/color.md)
* [Befehlsgrundlagen](../design/basics/commanding-basics.md)
* [Fluent Design für Windows-apps](../design/fluent-design-system/index.md)
* [Einführung in die app-design](../design/basics/design-and-ui-intro.md)
* [Navigationsgrundlagen](../design/basics/navigation-basics.md)
* [Reaktionsfähige Designtechniken](../design/layout/responsive-design.md)
* [Bildschirmgrößen und Haltepunkte](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Style-Übersicht](../design/style/index.md)
* [Schreibstil](../design/style/writing-style.md)

Darüber hinaus haben wir die folgenden Seiten mit völlig neue Informationen zu ihren Bereichen umgeschrieben:

* [Symbole](../design/style/icons.md) bietet jetzt nützliche Empfehlungen mithilfe von Symbolen und somit die geklickt werden kann.
* [Typografie](../design/style/typography.md) werden Informationen über ähnliche Artikel, platzieren alles an einem Ort mit aktualisierten Anleitung und Abbildungen, konsolidiert.

![Bild der Farben-palette](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>App-Installer-Dateien in Visual Studio

App-Installer-Dateien können jetzt mit Visual Studio 2017 Update 15.7 erstellt werden. [Erfahren Sie, wie Sie Visual Studio verwenden, um eine App-Installer-Datei erstellen](../packaging/create-appinstallerfile-vs.md) und automatische Updates für Ihre apps aktivieren. Wenn Sie Probleme ausführen, finden Sie unter [Problembehandlung bei der Installation mit der App-Installer-Datei](../packaging/troubleshoot-appinstaller-issues.md) um häufig auftretende Probleme und Lösungen anzuzeigen.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge-WebView-Steuerelement für Windows Forms und WPF-Anwendungen

Anzeigen von Webinhalten in Ihrer desktop-Anwendung mithilfe der WebView-Steuerelement, das zuvor nur für UWP-Anwendungen verfügbar. Dieses Steuerelement verwendet, der rendering-Engine eine Ansicht einbetten, rendert die HTML aufwändig formatierten Inhalt von einem Remotewebserver, dynamisch generiertem Code oder Inhaltsdateien Microsoft Edge. Suchen Sie das WebView-Steuerelement in der neuesten Version von der [Community-Toolkit für Windows.](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)

Suchen Sie nach anderen Steuerelemente für die Webansicht das Windows-Community-Toolkit in zukünftigen Versionen. Weitere Informationen finden Sie unter [Host UWP-Steuerelemente in WPF und Windows Forms-Anwendungen.](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="gaze-input-and-interactions"></a>Blickverlaufseingabe und Interaktionen

[Verfolgen eines Benutzers Blicke, Aufmerksamkeit und Vorhandensein basierend auf den Speicherort und die Verschiebung von ihren Augen Wirklichkeit.](../design/input/gaze-interactions.md) Dieses leistungsfähige neue Möglichkeit zum verwenden, und interagieren mit Ihren UWP-apps ist besonders nützlich als hilfstechnologien. Blickverlaufseingabe hinaus überzeugende Verkaufschancen für Spiele (einschließlich der Ziel-Übernahme und Nachverfolgen) und anderen interaktiven Szenarios, in denen herkömmliche Eingabegeräte (Tastatur, Maus, Touch) nicht verfügbar sind.

### <a name="msix-packaging-format"></a>MSIX Verpackungsformat

Auf der Microsoft Build 2018-Konferenz angekündigt, ist die MSIX eine neue containerisierung-Paketformat, das für alle Windows-Anwendungen einschließlich Win32, Windows Forms, WPF und UWP gilt. Dieses neue Format erbt UWP hervorragende Funktionen:

* Sichere Installation und Aktualisierung. 
* Verwaltet Sicherheitsmodell, mit einem System flexible Funktion.
* Unterstützung für den Microsoft Store, unternehmensverwaltung und viele benutzerdefinierte Verteilung Modelle.

Tools zum Erstellen von diesen Paketen werden in einer zukünftigen Version von Visual Studio und Windows SDK zur Verfügung.

Das verpackungsformat MSIX ist ein open-Source-Format mit einer unserer Partner zur Unterstützung der MSIX-Ökosystem, mit deren Tools und Lösungen erleichtert. Weitere Informationen zu MSIX Paketformat finden Sie unter [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![MSIX Packaging-Bild](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Optionale Pakete mit ausführbarem Code

Optionale Pakete in Ihrer app können jetzt enthalten die ausführbare Datei C# Code. [Erfahren Sie, wie Sie mithilfe von Visual Studio so konfigurieren Sie optionale Add-On-Pakete, um Ihr Haupt-app-Paket zu unterstützen.](../packaging/optional-packages-with-executable-code.md)

### <a name="page-transitions"></a>Seitenübergänge

[Seitenübergänge](../design/motion/page-transitions.md) navigieren die Benutzer zwischen den Seiten in einer app. Sie können die Benutzer nachvollziehen, wo sie in der Navigationshierarchie und Bereitstellen von Feedback über die Beziehung zwischen den Seiten.

### <a name="project-rome"></a>Project Rome

Das Projekt "ROME"-Team hat ihren IOS- und Android SDKs, neue Features wie Benutzeraktivitäten hinzufügen und das refactoring Großteil ihres Codes, um eine konsistente Programmierfunktionen bieten, über die verschiedenen SDKs überarbeitet. [Alle neuen API-Verweis und den exemplarischen Vorgehensweisen-Dokumente](https://docs.microsoft.com/windows/project-rome/) wird online geschaltet während der Entwicklerkonferenz Build 2018.

### <a name="sets"></a>Legt

Das Sets-Feature ist in Windows-Insider Preview-builds verfügbar. Wenn die Sets-Funktion verwenden zu können, wird Ihre app in einem Fenster gezeichnet, die mit anderen apps, mit jeder app, die mit einer eigenen Registerkarte in der Titelleiste freigegeben werden könnte. 

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="get-started"></a>Beginnen

Wir haben unsere Get revitalized Inhalt mit neue Kurse für Learning gestartet. Geben Sie die neue Windows 10-Entwickler mit Informationen zu einigen allgemeinen Aufgaben, dass sie möglicherweise ausführen möchten, ist diese neuen Themen sollen. Sie sind nicht Lernprogramme und bieten keine Handheld eine exemplarische Vorgehensweise, aber Sie können stattdessen darauf hingewiesen, in dem vorhandener Dokumentation vorhanden ist und wie Sie es verwenden. Sehen Sie sich die überarbeitete [mit Programmieren beginnen](../get-started/create-uwp-apps.md) Seite, oder untersuchen Sie jeden einzelnen Learning nachverfolgen:

* [Erstellen Sie ein Formular](../get-started/construct-form-learning-track.md)
* [Anzeigen von Kunden in einer Liste](../get-started/display-customers-in-list-learning-track.md)
* [Speichern und Laden von Einstellungen](../get-started/settings-learning-track.md)
* [Arbeiten mit Dateien](../get-started/fileio-learning-track.md)

![Gestartete Image abrufen](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Bericht zur Anzeigenleistung

Die [ankündigen Leistungsbericht](../publish/advertising-performance-report.md) im Partner Center jetzt Viewability stellt Metriken bereit. Wir außerdem hinzugefügt, die [optimieren die Viewability Ihre Ad-Einheiten](../monetize/optimize-ad-unit-viewability.md) Artikel soll Ihnen Empfehlungen für die Optimierung der Viewability von Ihr anzeigen.

### <a name="targeted-push-notifications"></a>Zielgerichtete Pushbenachrichtigungen

Die [Benachrichtigungen](../publish/send-push-notifications-to-your-apps-customers.md) Seite im Partner Center bietet jetzt zusätzliche Analysedaten für alle Ihre Benachrichtigungen in Ansichten "Diagramm" und "World zuordnen.

## <a name="videos"></a>Videos

### <a name="cwinrt"></a>C++/WinRT

C++ / WinRT ist eine neue Art der Erstellung und Nutzung von Windows-Runtime-APIs. Es hat Sole in Headerdateien implementiert, und Sie mit erstklassigen Zugriff auf moderne app-Funktionen bereit. [Das Video](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be) um zu erfahren, wie sie Sie dann funktioniert [Developer Dokumentation lesen](../cpp-and-winrt-apis/index.md) für Weitere Informationen.

### <a name="multi-instance-uwp-apps"></a>UWP-Apps mit mehreren Instanzen

Windows ermöglicht jetzt mehrere Instanzen Ihrer UWP-App, mit jeweils in eigenen separaten Prozess ausgeführt. [Das Video](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) zu erfahren, wie Sie eine neue app erstellen, das dieses Feature unterstützt [Developer Dokumentation lesen](../launch-resume/multi-instance-uwp.md) für Weitere Informationen dazu, wie und warum dieses Feature zu verwenden.

## <a name="samples"></a>Beispiele

### <a name="customer-database-tutorial"></a>Kunden-Datenbank-tutorial

In diesem Tutorial eine einfache UWP-app zum Verwalten einer Liste von Kunden erstellt und werden Konzepte und Methoden, die in der Enterprise-Entwicklung nützlich. Es führt Sie durch Implementieren von UI-Elemente und Hinzufügen von Operationen auf einer lokalen SQLite-Datenbank und lose Anleitungen für die Verbindung mit einer REST-Remotedatenbank, wenn Sie fortfahren möchten. [Sehen Sie sich das Lernprogramm hier](../enterprise/customer-database-tutorial.md)