---
title: Was ist eine App der universellen Windows-Plattform (UWP)?
description: Erfahren Sie mehr über Apps für die universelle Windows-Plattform (UWP), die auf einer Vielzahl von Geräten unter Windows 10 ausgeführt werden können.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperfq1
keywords: Windows 10, UWP, universell
ms.localizationpriority: medium
ms.openlocfilehash: 3c6fd1f2ebee2e4e3a7d3ceb028f5def19e932c6
ms.sourcegitcommit: 733d422fa1e3433cde842e2f200d3ef202767cc1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94333285"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Was ist eine App der universellen Windows-Plattform (UWP)?

UWP ist eine von vielen Methoden zum Erstellen von Clientanwendungen für Windows. UWP-Apps verwenden WinRT-APIs, um leistungsstarke Benutzeroberflächen und erweiterte asynchrone Features bereitzustellen, die für mit dem Internet verbundene Geräte ideal sind.

Informationen zum Herunterladen der Tools, die Sie benötigen, um mit der Erstellung von UWP-Apps zu beginnen, finden Sie unter [Vorbereiten](/windows/apps/get-set-up.md). Anschließend können Sie [Ihre erste App schreiben](your-first-app.md).


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>Wie passt UWP in die Microsoft-Entwicklungsgeschichte?

UWP ist eine Möglichkeit zum Erstellen von Apps, die auf Windows 10-Geräten ausgeführt werden können, und lässt die Kombination mit anderen Plattformen zu. UWP-Apps können Win32-APIs und .NET-Klassen verwenden (weitere Informationen finden Sie unter [API-Sammlungen für UWP-Apps](/previous-versions/mt186421(v=vs.85)), [DLLs für UWP-Apps](/previous-versions/mt186422(v=vs.85)) und [.NET für UWP-Apps](/dotnet/api/index?view=dotnet-uwp-10.0)).

Die Microsoft-Entwicklungsgeschichte entwickelt sich ständig weiter, und zusammen mit Initiativen wie [WinUI](/windows/apps/winui/), [MSIX](/windows/msix/) und [Project Reunion](https://github.com/microsoft/ProjectReunion) ist UWP ein leistungsstarkes Tool für die Erstellung von Clientanwendungen.


## <a name="features-of-a-uwp-app"></a>Funktionen einer UWP-App

Eine UWP-App:

- Ist sicher – UWP-Apps deklarieren, auf welche Geräteressourcen und Daten sie zugreifen. Der Benutzer muss den Zugriff autorisieren.
- Kann eine gemeinsame API auf allen Geräten nutzen, auf denen Windows 10 ausgeführt wird.
- Kann gerätespezifische Funktionen verwenden und die Benutzeroberfläche an verschiedene, gerätespezifische Bildschirmgrößen, Auflösungen und DPI-Werte anpassen.
- Ist im Microsoft Store für alle (oder nur für die von Ihnen angegebenen) Geräte verfügbar, auf denen Windows 10 ausgeführt wird. Der Microsoft Store bietet mehrere Möglichkeiten, Ihre App gewinnbringend anzubieten.
- Kann ohne Risiko für den Computer oder eine schleichende Beeinträchtigung des Systems installiert und deinstalliert werden.
- Ist ansprechend – Verwenden Sie Live-Kacheln, Pushbenachrichtigungen und Benutzeraktivitäten, um mit der Windows-Zeitachse und der Funktion „Weitermachen, wo Sie aufgehört haben” von Cortana zu interagieren und Benutzer zu erreichen.
- Kann in C#, C++, Visual Basic und JavaScript programmiert werden. Verwenden Sie für die Benutzeroberfläche WinUI, XAML, HTML oder DirectX.

Betrachten wir diese Eigenschaften im Detail.

### <a name="secure"></a>Sicher

UWP-Apps deklarieren im Manifest die benötigten Gerätefunktionen, z. B. den Zugriff auf das Mikrofon, den Standort, die Webcam, USB-Geräte, Dateien usw. Der Benutzer muss den Zugriff bestätigen und autorisieren, bevor die App die Funktion nutzen kann.

### <a name="a-common-api-surface-across-all-devices"></a>Eine gemeinsame API-Oberfläche für alle Geräte

In Windows 10 wird die universelle Windows-Plattform (UWP) eingeführt, die eine gemeinsame App-Plattform auf jedem Gerät bereitstellt, auf dem Windows 10 ausgeführt wird. Die zentralen UWP-APIs sind auf allen Windows-Geräten identisch. Wenn Ihre App nur die zentralen APIs verwendet, wird sie auf jedem Windows 10-Gerät ausgeführt, unabhängig davon, ob sie für einen Desktop-PC, die Xbox oder ein Mixed Reality-Headset usw. entwickelt wurde.

Eine UWP-App, die in C++/WinRT oder C++/CX geschrieben wurde, hat Zugriff auf die Win32-APIs, die Teil der universellen Windows-Plattform (UWP) sind. Diese Win32-APIs werden von allen Windows 10-Geräten implementiert.

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Erweiterungs-SDKs machen die einzigartigen Funktionen spezifischer Gerätetypen verfügbar

Wenn Sie eine App für die universellen APIs entwickeln, kann sie auf allen Geräten mit Windows 10 ausgeführt werden. Wenn Ihre UWP-App jedoch die Vorteile gerätespezifischer APIs nutzen soll, ist dies ebenfalls möglich.

Mit Erweiterungs-SDKs können Sie spezielle APIs für unterschiedliche Geräte aufrufen. Wenn Sie Ihre UWP-App beispielsweise für ein IoT-Gerät entwickeln, können Sie Ihrem Projekt das IoT-Erweiterungs-SDK hinzufügen, um spezifische Funktionen für IoT-Geräte zu unterstützen. Weitere Informationen zum Hinzufügen von Erweiterungs-SDKs finden Sie unter [Programmieren mit Erweiterungs-SDKs](/uwp/extension-sdks/device-families-overvieww#extension-sdks) im Abschnitt zu **Erweiterungs-SDKs**.

Sie können Ihre App so entwerfen, dass sie nur auf einem bestimmten Gerätetyp ausgeführt werden kann, und den Vertrieb über den Microsoft Store genau auf diesen Gerätetyp beschränken. Alternativ können Sie zur Laufzeit testen, ob eine bestimmte API vorhanden ist, und das Verhalten der App entsprechend anpassen. Weitere Informationen finden Sie unter [Programmieren mit Erweiterungs-SDKs](/uwp/extension-sdks/device-families-overview#writing-code) im Abschnitt **Schreiben von Code**.<br>

Das folgende Video bietet einen kurzen Überblick über die Gerätefamilien und adaptiven Code:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>Adaptive Steuerelemente und Eingaben

Benutzeroberflächenelemente reagieren auf die Größe und DPI-Auflösung des Bildschirms, in dem die App ausgeführt wird, indem sie das Layout und die Skalierung anpassen. UWP-Apps unterstützen mehrere Eingabetypen, z. B. Tastatur, Maus, Toucheingabe, Stift und Xbox One-Controller. Wenn Sie die Benutzeroberfläche weiter an eine bestimmte Bildschirmgröße oder ein bestimmtes Gerät anpassen müssen, helfen Ihnen neue Layoutpanels und Tools dabei, die Benutzeroberfläche an die verschiedenen Geräte und Formfaktoren anzupassen, unter denen Ihre App ausgeführt werden kann.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

Windows hilft Ihnen mit folgenden Features, Ihre UI auf mehrere Geräte auszurichten:

- Universelle Steuerelemente und Layoutpanels unterstützen Sie bei der Optimierung Ihrer Benutzeroberfläche für die Bildschirmauflösung des Geräts. Beispielsweise werden Steuerelemente wie Schaltflächen und Schieberegler automatisch an die Bildschirmgröße und DPI-Dichte des Geräts angepasst. Layoutpanels helfen, das Layout von Inhalten basierend auf der Bildschirmgröße anzupassen. Die adaptive Skalierung sorgt dafür, dass die Auflösung und DPI-Unterschiede geräteübergreifend berücksichtigt werden.
- Dank der allgemeinen Verarbeitung von Eingaben können Eingaben per Touchgeste, Stift, Maus, Tastatur oder Controller erfolgen (z. B. Microsoft Xbox-Controller).
- Tools unterstützen Sie bei der Entwicklung von Benutzeroberflächen, die sich an verschiedene Bildschirmauflösungen anpassen.

Einige Aspekte der UI Ihrer App werden automatisch auf allen Geräten angepasst. Das Design der Benutzererfahrung Ihrer App muss jedoch möglicherweise angepasst werden, je nachdem, auf welchem Gerät die App ausgeführt wird. Eine Foto-App könnte die Benutzeroberfläche beispielsweise anpassen, wenn sie auf einem kleinen Handheld-Gerät ausgeführt wird, um sicherzustellen, dass sie optimal mit einer Hand bedient werden kann. Wenn eine Foto-App auf einem Desktopcomputer ausgeführt wird, sollte sich die Benutzeroberfläche anpassen, um die zusätzliche Bildschirmfläche zu nutzen.

### <a name="theres-one-store-for-all-devices"></a>Ein Store für alle Geräte

Ihre App wird in einem Store für allgemeine Apps für Windows 10-Geräte wie PC, Tablet, Xbox, HoloLens, Surface Hub und IoT-Geräte (Internet der Dinge) zur Verfügung gestellt. Sie können Ihre App an den Store übermitteln und für alle oder nur für ausgewählte Gerätetypen zur Verfügung stellen. Sie übermitteln und verwalten alle Ihre Apps für Windows-Geräte an einem zentralen Ort. Sie verfügen über eine C++-Desktop-App, die Sie mit UWP-Features aufwerten und im Microsoft Store vertreiben möchten? Auch das ist möglich.

Da UWP-Apps und [Application Insights](https://azure.microsoft.com/services/application-insights/) integriert sind, stehen detaillierte Telemetrie- und Analysedaten zur Verfügung. Das Tool hilft, das Benutzerverhalten zu analysieren und Ihre Apps zu optimieren.

UWP-Apps können mit [MSIX](/windows/msix/) gepackt und über den Microsoft Store oder auf andere Weise verteilt werden. MSIX ermöglicht die Aktualisierung von Apps, unabhängig davon, wie sie verteilt werden. Informationen hierzu finden Sie unter [Aktualisieren von nicht im Store veröffentlichten App-Paketen aus Ihrem Code](/windows/msix/non-store-developer-updates).

### <a name="monetize-your-app"></a>Monetarisieren Ihrer App

Sie können wählen, wie Sie Ihre App gewinnbringend nutzen. Es gibt eine Reihe von Möglichkeiten, mit Ihren Apps Geld zu verdienen. Sie müssen nur die für Sie am besten geeignete Methode auswählen, beispielsweise:

- Ein kostenpflichtiger Download ist die einfachste Möglichkeit. Geben Sie einfach nur den Preis an.
- Mit Testversionen können Benutzer Ihre App vor dem Kauf ausprobieren. Dies verbessert gegenüber den herkömmlicheren „Freemium“-Optionen die Auffindbarkeit und erhöht die Anzahl der Abschlüsse.
- Angebotspreise bieten Anreize für Benutzer.
- In-App-Käufe.


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Relevante Echtzeitinformationen lassen Benutzer immer wieder auf die App zugreifen

Es gibt zahlreiche Möglichkeiten, Benutzer für Ihre UWP-App zu begeistern:

- Auf Live-Kacheln und auf dem Sperrbildschirm sind kontextbezogene, zeitnahe Informationen zur App auf einen Blick zu sehen.
- Pushbenachrichtigungen informieren Benutzer in Echtzeit über wichtige Ereignisse.
- Dank Benutzeraktivitäten können Benutzer an der Stelle weiterarbeiten, an der sie aufgehört haben – sogar geräteübergreifend.
- Im Info-Center werden Benachrichtigungen von Ihrer App organisiert.
- Die Ausführung im Hintergrund und Trigger aktivieren die App, wenn der Benutzer sie benötigt.
- Per Sprache und mit Bluetooth LE-Geräten kann Ihre App Benutzern die Interaktion mit der Welt um sie herum ermöglichen.
- Integrieren Sie Cortana, damit Ihre App Sprachbefehle unterstützt.

##  <a name="use-a-language-you-already-know"></a>Verwenden einer vertrauten Sprache

UWP-Apps verwenden die Windows-Runtime, die native, vom Betriebssystem bereitgestellte API. Diese API ist in C++ implementiert und wird in C#, Visual Basic, C++ und JavaScript unterstützt. Einige Optionen für das Schreiben von UWP-Apps:

- XAML-UI und C#, VB oder C++
- DirectX-UI und C++
- JavaScript und HTML
- WinUI

## <a name="links-to-help-you-get-going"></a>Links für die ersten Schritte

### <a name="get-set-up"></a>Vorbereiten

Navigieren Sie zu [Vorbereiten](/windows/apps/get-started/get-set-up.md), um die erforderlichen Tools herunterzuladen und mit dem Erstellen von Apps zu beginnen, und [schreiben Sie Ihre erste App](your-first-app.md).

### <a name="design-your-app"></a>Entwerfen der App

Das Entwurfssystem von Microsoft heißt Fluent. Das Fluent Design System stellt eine Reihe von UWP-Funktionen in Kombination mit bewährten Methoden für die Erstellung von Apps bereit, die auf allen Windows-basierten Gerätetypen eine optimale Leistung bieten. Fluent-Umgebungen sind anpassungsfähig und überzeugen auf Geräten wie Tablets, Laptops, PCs, TV- und Virtual-Reality-Geräten durch eine intuitive Bedienung. Weitere Informationen zum Fluent Design System finden Sie unter [Das Fluent Design System für UWP-Apps](/windows/uwp/design/fluent-design-system).

Zu einem guten [Design](http://design.windows.com/) gehören nicht nur das Erscheinungsbild und die Funktionalität einer App, sondern auch die Entscheidung darüber, wie Benutzer mit der App interagieren. Die Benutzerfreundlichkeit spielt eine große Rolle bei der Beurteilung, wie gerne Benutzer Ihre App verwenden. Sparen Sie daher nicht an diesem Schritt. [Designgrundlagen](https://developer.microsoft.com/windows/apps/design) bieten eine Einführung in den Entwurf von UWP-Apps (Universelle Windows-Plattform). Unter [Einführung in universelle Windows-Plattform-Apps (UWP) für Designer](../design/basics/design-and-ui-intro.md) finden Sie Informationen zum Entwerfen von UWP-Apps, die Benutzer begeistern. Bevor Sie mit dem Schreiben von Code beginnen, lesen Sie die Informationen unter [Einführung der Geräte](../design/devices/index.md). Diese helfen Ihnen dabei, die Interaktionsmöglichkeiten Ihrer App für alle in Frage kommenden Formfaktoren zu durchdenken.

Zusätzlich zur Interaktion auf verschiedenen Geräten sollten Sie [Ihre App planen](./plan-your-app.md), um die Vorteile verschiedener Geräte optimal zu nutzen. Beispiel:

- Entwerfen Sie Ihren Workflow anhand von [Navigationsdesigngrundlagen für UWP-Apps](../design/basics/navigation-basics.md), damit die App auf dem Bildschirm eines mobilen Geräts genauso gut funktioniert wie auf Geräten mit mittelgroßem und großem Bildschirm. [Erstellen Sie das Layout der Benutzeroberfläche](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), um unterschiedliche Bildschirmgrößen und Auflösungen zu berücksichtigen.

- Achten Sie darauf, wie Sie verschiedene Eingabearten umsetzen können. In den [Richtlinien für Interaktionen](../design/layout/index.md) finden Sie Informationen darüber, wie Benutzer anhand von Features wie [Cortana](/cortana/skills/), [Sprache](../design/input/speech-interactions.md), [Toucheingaben](../design/input/touch-interactions.md), der [Touch-Bildschirmtastatur](../design/input/keyboard-interactions.md) und vielem mehr mit Ihrer App interagieren.  In den [Richtlinien für Text und Texteingabe](../design/controls-and-patterns/text-controls.md) finden Sie Informationen für eine herkömmlichere Benutzerumgebung.

### <a name="add-services"></a>Hinzufügen von Diensten

- Verwenden Sie [Clouddienste](https://azure.microsoft.com/documentation/services/cloud-services) für die Synchronisierung auf allen Geräten.
- Erfahren Sie, wie Sie eine [Verbindung mit Webdiensten](/previous-versions/windows/apps/hh761504(v=win.10)) zur Unterstützung der App-Benutzerumgebung herstellen.
- Beziehen Sie [Push-Benachrichtigungen](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md) und [In-App-Käufe](../monetize/enable-in-app-product-purchases.md) in Ihre Planung ein. Diese Features sollten auf allen Geräten funktionieren.

### <a name="submit-your-app-to-the-store"></a>Übermitteln Ihrer App an den Store

Im [Partner Center](https://partner.microsoft.com/dashboard) können Sie all Ihre Apps für Windows-Geräte zentral verwalten und übermitteln. Informieren Sie sich unter [Veröffentlichen von Windows-Apps und -Spielen](../publish/index.md), wie Sie Ihre Apps für die Veröffentlichung im Microsoft Store übermitteln.

Neue Features vereinfachen Prozesse und geben Ihnen mehr Kontrolle. Sie finden dort auch detaillierte [Analyseberichte](../publish/analytics.md) in Kombination mit [Auszahlungsdetails](../publish/payout-summary.md), Möglichkeiten, [Ihre App zu bewerben und Kunden zu erreichen](../publish/attract-customers-and-promote-your-apps.md) und vieles mehr.

Weitere einführende Informationen finden Sie unter [Einführung in das Entwickeln von Windows-Apps für Windows 10-Geräte](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices).

### <a name="more-advanced-topics"></a>Fortgeschrittenere Themen

- Hier erfahren Sie, wie Sie [Benutzeraktivitäten](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) verwenden, damit Benutzeraktivitäten in Ihrer App auf der Windows-Zeitachse und in der Funktion „Weitermachen, wo Sie aufgehört haben” von Cortana angezeigt werden.
- Erfahren Sie, wie Sie [Kacheln, Signale und Benachrichtigungen für UWP-Apps](../design/shell/tiles-and-notifications/index.md) verwenden.
- Eine vollständige Liste von Win32-APIs für UWP-Apps finden Sie unter [API-Sätze für UWP-Apps](/previous-versions/mt186421(v=vs.85)) und [DLLs für UWP-Apps](/previous-versions/mt186422(v=vs.85)).
- Eine Übersicht über das Schreiben von .NET UWP-Apps finden Sie unter [Universelle Windows-Apps in .NET](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/).
- Eine Liste der .NET-Typen, die Sie in einer UWP-App verwenden können, finden Sie unter [.NET für UWP-Apps](/dotnet/api/index?view=dotnet-uwp-10.0).
- [Kompilieren von Apps mit .NET Native](/dotnet/framework/net-native/)
- Erfahren Sie, wie Sie Ihre bestehende Desktop-App durch moderne Funktionen für Windows 10-Benutzer erweitern und diese mithilfe der [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) im Microsoft Store veröffentlichen.

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Beziehung zwischen der universellen Windows-Plattform und Windows-Runtime-APIs
Wenn Sie eine App für die universelle Windows-Plattform (UWP) erstellen, verfügen Sie bereits über einen beträchtlichen Wissensvorsprung, wenn Sie die Begriffe „universelle Windows-Plattform (UWP)“ und „Windows-Runtime (WinRT)“ mehr oder weniger gleichbedeutend behandeln. Sie können auch *hinter* die Kulissen der Technologie blicken, um die Unterschiede zwischen beiden Ansätzen zu ergründen. Wenn Sie sich für die Unterschiede interessieren, richtet sich dieser letzte Abschnitt an Sie.

Die Windows-Runtime und WinRT-APIs sind eine Weiterentwicklung der Windows-APIs. Windows wurde ursprünglich über flache Win32-C-APIs programmiert. Diese wurden durch COM-APIs ergänzt ([DirectX](/windows/desktop/directx) ist ein bekanntes Beispiel). Mit Windows Forms, WPF, .NET und verwalteten Sprachen kamen nicht nur weitere individuelle Programmierweisen für Windows-Apps hinzu, sondern auch eigene API-Technologien. Die Windows-Runtime stellt genauer betrachtet die nächste Stufe von COM dar. Auf der tatsächlichen ABI (Application Binary Interface)-Ebene wird der Ursprung in COM sichtbar. Bei der Konzeptionierung der Windows-Runtime wurde jedoch darauf geachtet, dass sie Aufrufe von vielen verschiedenen Programmiersprachen unterstützt. Zudem sollten die Aufrufe für jede dieser Sprachen möglichst intuitiv zu schreiben sein. Zu diesem Zweck wird der Zugriff auf die Windows-Runtime über so genannte Sprachprojektionen ermöglicht. Es gibt eine Windows-Runtime-Sprachprojektion in C#, in Visual Basic in Standard C++, in JavaScript usw. Darüber hinaus können WinRT-APIs – nachdem sie ordnungsgemäß gepackt wurden (siehe [Desktop-Brücke](/windows/msix/desktop/source-code-overview)) – von Apps aufgerufen werden, die auf vielen verschiedenen Anwendungsmodellen basieren: Win32, .NET, WinForms und WPF

Natürlich können Sie WinRT-APIs auch von Ihrer UWP-App aufrufen. UWP ist ein Anwendungsmodell, das auf der Windows-Runtime aufbaut. Technisch gesehen basiert das UWP-Anwendungsmodell auf [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), obwohl Ihnen dieses Detail je nach verwendeter Programmiersprache möglicherweise verborgen bleibt. Wie in diesem Thema erörtert, eignet sich die UWP im Hinblick auf ihren Wertbeitrag zum Schreiben einzelner Binärdateien, die (falls gewünscht) im Microsoft Store veröffentlicht und auf einer breiten Palette von Geräten unterschiedlicher Formfaktoren ausgeführt werden können. Welche Geräte Ihre UWP-App unterstützt, richtet sich danach, auf welche Teilmenge von Windows-Runtime-APIs Sie die App-Aufrufe beschränken oder welche APIs Sie bedingt aufrufen.

Wir hoffen, dass wir in diesem Abschnitt die Unterschiede zwischen der Technologie, die Windows-Runtime-APIs zugrunde liegt, und dem Mechanismus und geschäftlichen Nutzen verdeutlichen konnten, den die universelle Windows-Plattform bieten kann.
