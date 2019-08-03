---
title: Neues in der Windows-Dokumentation im Mai 2018 – Entwickeln von UWP-Apps
description: Der Windows 10-Entwicklerdokumentation für Mai 2018 und die Microsoft Build-Konferenz wurden neue Features, Videos und Entwicklerleitfäden hinzugefügt.
keywords: Neues, Neuigkeiten, Update, Features, Entwicklerleitfaden, Windows 10, Mai, Build
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d8b72501c298f3814092ec4567a5fb608c4bb88f
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682763"
---
# <a name="whats-new-in-the-windows-developer-docs-in-may-2018"></a>Neues in der Windows-Entwicklerdokumentation im Mai 2018

Die Entwicklerdokumentation für die Windows-Plattform wird kontinuierlich mit Informationen zu neuen Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden, Videos und Beispiele wurden im Mai parallel zur Entwicklerkonferenz [Microsoft Build 2018](https://www.microsoft.com/build/) bereitgestellt.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="motion-in-fluent-design"></a>Bewegung in Fluent Design

Die Verwendung von Bewegung im Fluent Design-System entwickelt sich weiter, und zwar auf der Grundlage von Timing, Geschwindigkeitsverlauf, Direktionalität und Schwerkraft. Die Anwendung dieser Grundlagen erleichtert dem Benutzer die Navigation durch deine App und verbessert das digitale Erlebnis durch die Abbildung der realen Welt. Weitere Informationen dazu findest du in den folgenden Artikeln:

* [Die Bewegungsübersicht](../design/motion/index.md) wurde mit diesen Grundlagen aktualisiert.
* [Bewegung in der Praxis](../design/motion/motion-in-practice.md) enthält Beispiele für die Anwendung dieser Grundlagen in einer App.
* [Direktionalität und Schwerkraft](../design/motion/directionality-and-gravity.md) stabilisieren das mentale Modell, das der Benutzer von deiner App hat.
* [Timing und Geschwindigkeitsverlauf](../design/motion/timing-and-easing.md) lassen die Bewegungen in deiner App natürlich erscheinen.

![Bewegung in Aktion](../design/motion/images/contextual.gif)

### <a name="fluent-design-updates"></a>Fluent Design-Updates

Visuelle Updates und kleinere Änderungen wurden an den folgenden Fluent Design-Seiten vorgenommen:

* [Ausrichtung, Rand, Abstand](../design/layout/alignment-margin-padding.md)
* [Farbe](../design/style/color.md)
* [Befehlsgrundlagen](../design/basics/commanding-basics.md)
* [Fluent Design für Windows-Apps](../design/fluent-design-system/index.md)
* [Einführung in das App-Design](../design/basics/design-and-ui-intro.md)
* [Navigationsgrundlagen](../design/basics/navigation-basics.md)
* [Reaktionsfähige Designtechniken](../design/layout/responsive-design.md)
* [Bildschirmgrößen und Haltepunkte](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Stil für UWP-Apps](../design/style/index.md)
* [Schreibstil](../design/style/writing-style.md)

Darüber hinaus wurden die folgenden Seiten mit völlig neuen Informationen überarbeitet:

* [Symbole](../design/style/icons.md) bietet jetzt nützliche Empfehlungen dazu, wie Symbole genutzt und klickbar gemacht werden können.
* In [Typografie](../design/style/typography.md) werden Informationen aus thematisch ähnlichen Artikeln zusammengefasst, sodass alles an einem zentralen Ort mit aktuellen Anleitungen und Abbildungen zur Verfügung steht.

![Abbildung: Farbpalette](../design/style/images/color/accent-color-palette.svg)

### <a name="app-installer-files-in-visual-studio"></a>App-Installer-Dateien in Visual Studio

App-Installer-Dateien können nun mit Visual Studio 2017, Update 15.7 und höheren Versionen erstellt werden. Unter [Erstellen einer App-Installer-Datei mit Visual Studio](../packaging/create-appinstallerfile-vs.md) erfährst du, wie du mithilfe von Visual Studio eine App-Installer-Datei erstellst und automatische Updates für deine Apps aktivierst. Sollten Probleme auftreten, findest du unter [Problembehandlung bei der Installation der App-Installer-Datei](../packaging/troubleshoot-appinstaller-issues.md) Informationen zu häufigen Problemen sowie entsprechende Lösungen.

### <a name="edge-webview-control-for-windows-forms-and-wpf-applications"></a>Edge-Steuerelement „WebView“ für Windows Forms- und WPF-Anwendungen

Mit dem WebView-Steuerelement, das bislang nur für UWP-Anwendungen verfügbar war, kannst du nun Webinhalte in deiner Desktopanwendung anzeigen. Dieses Steuerelement verwendet die Renderingengine von Microsoft Edge, um eine Ansicht einzubetten, die HTML-Inhalte mit umfassender Formatierung von einem Remotewebserver, dynamisch generierten Code oder Inhaltsdateien rendert. Das WebView-Steuerelement ist im aktuellen [Windows-Community-Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) enthalten.

In zukünftigen Releases des Windows-Community-Toolkits werden weitere ähnliche Steuerelemente wie „WebView“ bereitgestellt. Weitere Informationen findest du unter [Hosten von UWP-Steuerelementen in WPF- und Windows Forms-Anwendungen](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

### <a name="gaze-input-and-interactions"></a>Blickeingabe und -interaktionen

[Verfolge den Blick, die Aufmerksamkeit und die Präsenz eines Benutzers anhand der Position und Bewegung seiner Augen.](../design/input/gaze-interactions.md) Diese leistungsstarke neue Methode zur Verwendung und Interaktion mit deinen UWP-Apps ist besonders nützlich als Hilfstechnologie. Die Blickeingabe bietet zudem interessante Möglichkeiten für Spiele (einschließlich Zielerfassung und -verfolgung) und andere interaktive Szenarien, in denen herkömmliche Eingabegeräte (Tastatur, Maus, Touch) nicht zur Verfügung stehen.

### <a name="msix-packaging-format"></a>MSIX-Paketformat

MSIX wurde auf der Konferenz Microsoft Build 2018 angekündigt und ist ein neues Paketformat für die Containerisierung, das für alle Windows-Anwendungen geeignet ist – einschließlich Win32, Windows Forms, WPF und UWP. Dieses neue Format erbt praktische Features von UWP:

* Stabile Installation und Aktualisierung 
* Verwaltetes Sicherheitsmodell mit flexiblem Funktionssystem
* Unterstützung von Microsoft Store, Unternehmensverwaltung und zahlreichen benutzerdefinierten Distributionsmodellen

Tools zum Erstellen dieser Pakete werden in einem späteren Release von Visual Studio und Windows SDK bereitgestellt.

Das MSIX-Paketformat ist ein Open-Source-Format, wodurch unsere Partner das MSIX-Ökosystem einfacher mit ihren Tools und Lösungen unterstützen können. Weitere Informationen zum MSIX-Paketformat findest du unter [MSIX SDK](https://github.com/Microsoft/msix-packaging). 

![Abbildung: MSIX-Paket](images/msix.png)

### <a name="optional-packages-with-executable-code"></a>Optionale Pakete mit ausführbarem Code

Optionale Pakete in deiner App können nun ausführbaren C#-Code enthalten. Informationen dazu, wie du mithilfe von Visual Studio optionale Add-On-Pakete für die Unterstützung deines Haupt-App-Pakets konfigurierst, findest du [hier](/windows/msix/package/optional-packages).

### <a name="page-transitions"></a>Seitenübergänge

[Seitenübergänge](../design/motion/page-transitions.md) lassen Benutzer zwischen den Seiten einer App navigieren. Sie ermöglichen Benutzern zu verstehen, wo sie sich in der Navigationshierarchie befinden, und bieten Feedback zur Beziehung zwischen den Seiten.

### <a name="project-rome"></a>Project Rome

Das Team von Project Rome hat seine iOS- und Android-SDKs überarbeitet, neue Features wie etwa Benutzeraktivitäten hinzugefügt und einen Großteil des Codes umgestaltet, um über verschiedene SDKs hinweg eine konsistente Programmierumgebung zu bieten. [Alle neuen API-bezogenen Referenz- und Anleitungsdokumente](https://docs.microsoft.com/windows/project-rome/) werden während der Entwicklerkonferenz Build 2018 veröffentlicht.

### <a name="sets"></a>Gruppen

Das Gruppenfeature steht in Windows-Insider-Vorabversionen zur Verfügung. Bei Verwendung des Gruppenfeatures wird deine App in ein Fenster gezogen, das sie sich mit anderen Apps teilen kann. Dabei erhält jede App ihre eigene Registerkarte auf der Titelleiste. 

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="get-started"></a>Beginnen

Unsere ersten Schritte wurden mit neuen Lernpfaden aktualisiert. Diese neuen Themen enthalten Informationen zu einigen allgemeinen Aufgaben für neue Windows 10-Entwickler. Es handelt sich dabei weder um Tutorials noch um geführte exemplarische Vorgehensweisen. Vielmehr erfahren Entwickler hier, wo sie Dokumentationsmaterial finden und wie sie es verwenden. Sieh dir die überarbeitete Seite [Start coding](../get-started/create-uwp-apps.md) (Einstieg in die Programmierung) an, oder erkunde die einzelnen Lernpfade:

* [Erstellen eines Formulars](../get-started/construct-form-learning-track.md)
* [Anzeigen von Kunden in einer Liste](../get-started/display-customers-in-list-learning-track.md)
* [Speichern und Laden von Einstellungen](../get-started/settings-learning-track.md)
* [Arbeiten mit Dateien](../get-started/fileio-learning-track.md)

![Abbildung: Erste Schritte](../get-started/images/build-your-app.png)

### <a name="advertising-performance-report"></a>Bericht zur Anzeigenleistung

Der [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md) in Partner Center enthält nun Sichtbarkeitsmetriken. Außerdem haben wir den Artikel [Optimieren der Sichtbarkeit von Anzeigeneinheiten](../monetize/optimize-ad-unit-viewability.md) mit Empfehlungen zur Optimierung der Sichtbarkeit deiner Anzeigen hinzugefügt.

### <a name="targeted-push-notifications"></a>Zielgerichtete Pushbenachrichtigungen

Auf der Partner Center-Seite [Benachrichtigungen](../publish/send-push-notifications-to-your-apps-customers.md) stehen nun zusätzliche Analysedaten für alle deine Benachrichtigungen als Grafik und in Form einer Weltkarte zur Verfügung.

## <a name="videos"></a>Videos

### <a name="cwinrt"></a>C++/WinRT

C++/WinRT ist eine neue Möglichkeit zur Erstellung und Nutzung von Windows-Runtime-APIs. Sie wird ausschließlich in Headerdateien implementiert und bietet komfortablen Zugriff auf moderne App-Features. Wie das funktioniert, erfährst du in [diesem Video](https://www.youtube.com/watch?v=TLSul1XxppA&feature=youtu.be). Weitere Informationen erhältst du dann in der [Entwicklerdokumentation](../cpp-and-winrt-apis/index.md).

### <a name="multi-instance-uwp-apps"></a>UWP-Apps mit mehreren Instanzen

Unter Windows kannst du jetzt mehrere Instanzen deiner UWP-App ausführen – jede in einem eigenen, separaten Prozess. In [diesem Video](https://www.youtube.com/watch?v=clnnf4cigd0&feature=youtu.be) erfährst du, wie du eine neue App erstellst, die dieses Feature unterstützt. Weitere Informationen zur Verwendung dieses Features findest du dann in der [Entwicklerdokumentation](../launch-resume/multi-instance-uwp.md).

## <a name="samples"></a>Beispiele

### <a name="customer-database-tutorial"></a>Tutorial „Kundendatenbank“

In diesem Tutorial wird eine einfache UWP-App zur Verwaltung einer Kundenliste erstellt. Dabei werden praktische Konzepte und Methoden für die Entwicklung in Unternehmen vorgestellt. Du erfährst Schritt für Schritt, wie Elemente der Benutzeroberfläche implementiert und Vorgänge für eine lokale SQLite-Datenbank hinzugefügt werden. Außerdem findest du hier bei Interesse eine allgemeine Anleitung zur Verbindungsherstellung mit einer REST-Remotedatenbank. [Zum Tutorial](../enterprise/customer-database-tutorial.md)