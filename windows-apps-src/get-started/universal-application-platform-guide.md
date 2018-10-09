---
author: TylerMSFT
title: Was ist eine Universelle Windows-Plattform (UWP)-App?
description: Erfahren Sie mehr über Apps für die Universelle Windows-Plattform (UWP), die auf einer Vielzahl von Geräten unter Windows 10 ausgeführt werden können.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.author: twhitney
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, universal
ms.localizationpriority: medium
ms.openlocfilehash: 7f0168f0a1baef5e68bccdf0a33c3ac7eb7683a7
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "4427379"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Was ist eine Universelle Windows-Plattform (UWP)-App?

![Apps für die Universelle Windows-Plattform können auf einer Vielzahl von Geräten ausgeführt werden, unterstützen adaptive Benutzeroberflächen, natürliche Benutzereingaben, einen Store, ein Dev Center und Clouddienste.](images/universalapps-overview.png)

Eine UWP-App ist:

- Sicher: UWP-Apps deklarieren, auf welche Geräteressourcen und Daten sie zugreifen. Der Benutzer muss den Zugriff autorisieren.
- Fähig, eine allgemeine API auf allen Geräten zu verwenden, auf denen Windows10 ausgeführt wird.
- Fähig, bestimmte Gerätefunktionen zu verwenden und die UI auf verschiedene Geräte-Bildschirmgrößen, Auflösungen und DPI-Werte anzupassen.
- Verfügbar aus dem Microsoft Store für alle Geräte (oder nur solche, die Sie angeben), auf denen Windows10 ausgeführt wird. Das Microsoft Store bietet mehrere Möglichkeiten, mit Ihrer App Geld zu verdienen.
- Fähig, ohne Risiko für den Computer oder Maschinenschaden installiert und deinstalliert zu werden.
- Ansprechend: Verwenden Sie Live-Kacheln, Pushbenachrichtigungen und Benutzeraktivitäten, um mit der Windows-Zeitachse und Cortana „Begonnene Aufgaben” zu interagieren und Benutzer zu erreichen.
- Programmierbar in C#, C++, Visual Basic und Javascript. Verwenden Sie für die Benutzeroberfläche XAML, HTML oder DirectX.

Betrachten wir dies im Detail.

## <a name="secure"></a>Secure

UWP-Apps deklarieren im Manifest die Gerätefunktionen, die sie benötigen wie z.B. den Zugriff auf das Mikrofon, Standort, Webcam, USB-Geräte, Dateien usw. Der Benutzer muss den Zugriff bestätigen und autorisieren, bevor die App die Funktion nutzen kann.

## <a name="a-common-api-surface-across-all-devices"></a>Es gibt eine gemeinsame API-Oberfläche für alle Geräte

Windows10 führt die Universelle Windows-Plattform (UWP) ein, die eine gemeinsame App-Plattform auf jedem Gerät bereitstellt, auf dem Windows10 ausgeführt wird. Die wichtigsten UWP-APIs sind auf allen Windows-Geräten identisch. Wenn Ihre App nur die zentralen APIs verwendet, wird sie auf jedem Windows10-Gerät ausgeführt, unabhängig davon, ob Sie sie für einen Desktop-PC, eine Xbox oder einen Mixed Reality-Headset entwickeln usw.

Eine UWP-App die in C++ /WinRT oder C++ /CX geschrieben wurde, hat Zugriff auf die Win32 APIs, die Teil der universellen Windows-Plattform (UWP) sind. Diese Win32-APIs werden von allen Windows10-Geräten implementiert.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Erweiterungs-SDKs bieten die einzigartigen Funktionen der jeweiligen Gerätetypen

Wenn Sie die universellen APIs verwenden, kann Ihre App auf allen Geräten ausgeführt werden, die Windows10 ausführen. Wenn Sie Ihre UWP-App für gerätespezifische APIs nutzen möchten, ist dies möglich.

Mit Erweiterungs-SDKs können Sie spezielle APIs für verschiedene Geräte aufrufen. Wenn Ihre UWP-App ein IoT-Gerät als Ziel hat, können Sie z.B. das IoT-Erweiterungs-SDK auf das Projekt hinzufügen, um spezifische Funktionen anzuvisieren, die speziell für IoT-Geräten gelten. Weitere Informationen zum Hinzufügen von Erweiterungs-SDKs finden Sie unter im Abschnitt **Erweiterungs-SDKs** unter [Übersicht über die Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Sie können Ihre App so entwerfen, dass sie nur auf einem bestimmten Gerätetyp ausgeführt werden kann, und den Verteilungspunkt aus dem Microsoft Store nur auf einen Gerätetyp beschränken. Oder Sie können bedingt testen, ob eine API zur Laufzeit vorhanden ist und das Verhalten Ihrer App entsprechend anpassen. Weitere Informationen finden Sie im Abschnitt **Schreiben von Code** unter [Übersicht über die Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code).<br>

Das folgende Video bietet einen kurzen Überblick über die Gerätefamilien und adaptiven Code:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Adaptive Steuerelemente und Eingaben

UI-Elemente reagieren auf die Größe und DPI-Werte des Fensters der App, indem Sie das Layout und die Skalierung anpassen. UWP-Apps funktionieren außerdem mit mehreren Arten von Eingaben, z.B. Tastatur, Maus, Touch, Stift und Xbox One-Controller. Wenn Sie die Benutzeroberfläche auf eine bestimmte Bildschirmgröße oder ein bestimmtes Gerät anpassen müssen, helfen Ihnen neue Layoutbereiche und Tools dabei, die Benutzeroberfläche auf den Geräten so zu erstellen, dass sie auf die verschiedenen Geräte und Formfaktoren angepasst werden kann, auf denen Ihre App möglicherweise ausgeführt wird.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

Windows unterstützt Sie mit folgenden Features, Ihre UI auf mehrere Geräte auszurichten:

- Universelle Steuerelemente und Layoutbereiche können Sie bei der Optimierung Ihrer UI für die Bildschirmauflösung des Geräts unterstützen Steuerelemente wie Schaltflächen und Schieberegler werden automatisch nach Bildschirmgröße und DPI-Dichte angepasst. Layoutpanels können das Layout von Inhalten basierend auf der Größe des Bildschirms anpassen. Die adaptive Skalierung passt die Auflösung und DPI-Unterschiede auf allen Geräten an.
- Mit der allgemeinen Eingabebehandlung können Sie Benutzereingaben per Toucheingabe, Stift, Maus, Tastatur oder Controller (Microsoft Xbox-Controller oder Ähnliches) erhalten.
- Tools können Sie bei dem Entwerfen der UI unterstützen, sodass sie sich an verschiedene Bildschirmauflösungen anpasst.

Einige Aspekte der App-UI Ihrer App werden automatisch auf allen Geräten angepasst. Das Design der Benutzererfahrung Ihrer App muss jedoch möglicherweise angepasst werden, je nachdem, auf welchem Gerät die App ausgeführt wird. Eine Foto-App sollte die UI beispielsweise anpassen, wenn sie auf einem kleinen, tragbaren Gerät ausgeführt wird, um sicherzustellen, dass sie optimal mit einer Hand bedient werden kann. Wenn die Foto-App auf einem Desktopcomputer ausgeführt wird, sollte sich die Benutzeroberfläche anpassen, um die zusätzliche Bildschirmfläche zu nutzen.

## <a name="theres-one-store-for-all-devices"></a>Es gibt einen Store für alle Geräte

Ein einheitlicher App-Store stellt Ihre App auf Windows10-Geräten wie PC, Tablet, Xbox, HoloLens, Surface Hub, und Internet der Dinge (IoT) zur Verfügung. Sie können Ihre App an den Store übermitteln und auf für Geräte aller Art oder nur für bestimmte Geräte zur Verfügung stellen. Sie übermitteln und verwalten alle Ihre Apps für Windows-Geräte an einem zentralen Ort. Sie haben eine mit C++ entwickelte Desktop-App, die Sie mit UWP-Features modernisieren und im Microsoft Store verkaufen möchten? Das ist auch möglich.

UWP-Apps werden mit [Application Insights](http://azure.microsoft.com/services/application-insights/) für detaillierte Telemetrie und Analyse integriert. Dies ist ein besonders wichtiges Tool zum Verständnis der Anwender und zur Verbesserung Ihrer Apps.

### <a name="monetize-your-app"></a>Monetisieren Ihrer App

Sie können auswählen, wie Sie Ihre App vermarkten. Es gibt eine Reihe von Möglichkeiten, mit Ihren Apps Geld zu verdienen. Sie müssen nur die für Sie am besten geeignete Methode auswählen, zum Beispiel:

- Ein bezahlter Download ist die einfachste Möglichkeit. Geben Sie einfach nur Ihren Preis an.
- Dank Testversionen können Benutzern Ihre App vor dem Kauf testen. Im Vergleich zu den herkömmlicheren „Freemium“-Optionen kann sie leichter gefunden werden, und die Zahl der Konversionen ist höher.
- Verkaufspreise als Anreiz für Benutzer.
- In-App-Käufe und -Anzeigen sind ebenfalls verfügbar.

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Apps aus dem Microsoft Store bieten eine nahtlose Installation, Deinstallation und Upgrade des Erlebnisses

Alle UWP-Apps werden mithilfe eines Packaging-Systems verteilt, das den Benutzer, das Gerät und das System schützt. Benutzer müssen es niemals bedauern, eine App zu installieren, da UWP-Apps deinstalliert werden können, ohne etwas zu verlieren mit Ausnahme der Dokumente, die mit der App erstellt wurden.

Apps können problemlos bereitgestellt und aktualisiert werden. App-Pakete können modularisiert werden, damit Sie Inhalt und Erweiterungen nach Bedarf herunterladen können.

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Stellen Sie relevante Infos in Echtzeit für Benutzer bereit, damit sie immer wieder auf die App zugreifen.

Es gibt eine Vielzahl von Möglichkeiten, um Benutzer für Ihre UWP-App zu engagieren:

- Auf Live-Kacheln und dem Sperrbildschirm werden kontextbezogene, relevante und aktuelle Kachelinformationen auf einen Blick in der App sichtbar angezeigt.
- Pushbenachrichtigungen informieren Benutzer in Echtzeit über wichtige Ereignisse.
- Benutzeraktivitäten ermöglichen Benutzern geräteübergreifend dort weiterzumachen, wo sie in Ihrer App aufgehört haben.
- Das Info-Center organisiert Benachrichtigungen von Ihrer App.
- Hintergrundausführung und Auslöser erwecken Ihre App zum Leben, wenn der Benutzer sie benötigt.
- Ihre App kann Sprache und Bluetooth LE-Geräte verwenden, um Benutzer bei der Interaktion mit der Welt um sie herum zu unterstützen.
- Integrieren Sie Cortana für Befehl der Sprachfunktionalität von Ihrer App.

##  <a name="use-a-language-you-already-know"></a>Verwenden einer vertrauten Sprache

UWP-Apps verwenden die Windows-Runtime. Dies ist eine native, vom Betriebssystem integrierte API. Diese API ist in C++ implementiert und wird in C#, Visual Basic, C++ und JavaScript unterstützt. Einige Optionen für das Schreiben von Apps in UWP sind:

- XAML-UI und C#, VB oder C++
- DirectX UI und C++
- JavaScript und HTML

## <a name="links-to-help-you-get-going"></a>Links für die ersten Schritte

### <a name="get-set-up"></a>Vorbereiten

Navigieren Sie zu [Vorbereiten](get-set-up.md), um die Tools herunterzuladen, die Sie benötigen, um mit dem Erstellen von Apps zu beginnen, und [schreiben Sie Ihre erste App](your-first-app.md)!

### <a name="design-your-app"></a>Entwerfen Sie Ihre App

Das Microsoft-Entwurfssystem heißt Fluent. Das Fluent Design System stellt eine Reihe von UWP-Funktionen in Kombination mit bewährten Verfahrensweisen für die Erstellung von Apps dar, die auf allen Arten von Windows-basierten Geräten hervorragend funktionieren. Fluent-Umgebungen sind anpassungsfähig und fühlen sich auf Geräten wie Tablets, Laptops, PCs, Fernsehern und Virtual-Reality-Geräten ganz natürlich an. Weitere Informationen über Fluent Design finden Sie unter [Das Fluent Design System für UWP-Apps](https://docs.microsoft.com/windows/uwp/design/fluent-design-system).

Zu einem guten [Design](http://go.microsoft.com/fwlink/?LinkId=258848) gehören nicht nur das gute Aussehen und die Funktionen einer App, sondern auch die Entscheidung darüber, wie Benutzer mit der App interagieren. Die Benutzerfreundlichkeit spielt eine große Rolle bei der Beurteilung, wie gerne Benutzer Ihre App verwenden. Sparen Sie daher nicht an diesem Schritt. [Designgrundlagen](https://dev.windows.com/design) bieten eine Einführung in das Design von Apps für die Universelle Windows-App. Unter [Einführung in universelle Windows-Plattform-Apps (UWP) für Designer](https://msdn.microsoft.com/library/windows/apps/dn958439) finden Sie Informationen zum Entwerfen von UWP-Apps, die Benutzer begeistern. Bevor Sie mit dem Schreiben von Code beginnen, lesen Sie die Informationen unter [Einführung der Geräte](../design/devices/index.md). Diese helfen Ihnen dabei, die Interaktionsmöglichkeiten Ihrer App für alle in Frage kommenden Formfaktoren zu durchdenken.

Zusätzlich zur Interaktion auf verschiedenen Geräten sollten Sie [Ihre App planen](https://msdn.microsoft.com/library/windows/apps/hh465427), um die Vorteile verschiedener Geräte optimal zu nutzen. Zum Beispiel:

- Entwerfen Sie Ihren Workflow anhand von [Navigationsdesigngrundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn958438), damit die App auf dem Bildschirm eines mobilen Geräts genauso gut funktioniert wie auf Geräten mit mittelgroßem und großem Bildschirm. [Erstellen Sie das Layout der Benutzeroberfläche](https://msdn.microsoft.com/library/windows/apps/dn958435), um unterschiedliche Bildschirmgrößen und Auflösungen zu berücksichtigen.

- Achten Sie darauf, wie Sie verschiedene Eingabearten umsetzen können. In den [Richtlinien für Interaktionen](https://msdn.microsoft.com/library/windows/apps/dn611861) finden Sie Informationen darüber, wie Benutzer anhand von Features wie [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), [Sprache](https://msdn.microsoft.com/library/windows/apps/dn596121), [Toucheingaben](https://msdn.microsoft.com/library/windows/apps/hh465370), der [Touch-Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/hh972345) und vielem mehr mit Ihrer App interagieren.  In den [Richtlinien für Text und Texteingabe](https://msdn.microsoft.com/library/windows/apps/dn611864) finden Sie Informationen für eine herkömmlichere Benutzerumgebung.

### <a name="add-services"></a>Dienste hinzufügen

- Verwenden Sie [Clouddienste](http://go.microsoft.com/fwlink/?LinkId=526377) für die Synchronisierung auf allen Geräten.
- Erfahren Sie, wie Sie eine [Verbindung mit Webdiensten](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504) zur Unterstützung der App-Benutzerumgebung herstellen.
- Finden Sie heraus, wie Sie [Cortana zu Ihrer App hinzufügen können](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382), damit sie auf Sprachbefehle reagiert.
- Beziehen Sie [Push-Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/mt187203) und [In-App-Einkäufe](https://msdn.microsoft.com/library/windows/apps/mt219684) in Ihre Planung mit ein. Diese Features sollten auf allen Geräten funktionieren.

### <a name="submit-your-app-to-the-store"></a>Übermitteln Ihrer App an den Store.

Im neuen einheitlichen WindowsDevCenter-Dashboard können Sie all Ihre Apps für Windows-Geräte zentral verwalten und einreichen. Unter [Verwenden des einheitlichen Windows Dev Center-Dashboards](../publish/using-the-windows-dev-center-dashboard.md) erfahren Sie, wie Sie Ihre Apps zur Veröffentlichung im MicrosoftStore übermitteln.

Neue Features vereinfachen Prozesse und geben Ihnen mehr Kontrolle. Sie finden dort auch detaillierte [Analyseberichte](https://msdn.microsoft.com/library/windows/apps/mt148522) in Kombination mit [Auszahlungsdetails](https://msdn.microsoft.com/library/windows/apps/dn986925), Möglichkeiten, [Ihre App zu bewerben und Kunden zu erreichen](https://msdn.microsoft.com/library/windows/apps/mt148526) und vieles mehr.

Weitere einführende Informationen finden Sie unter [Einführung in das Entwickeln von Windows-Apps für Windows 10-Geräte](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>Fortgeschrittenere Themen

- Hier erfahren Sie, wie Sie [Benutzeraktivitäten](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) verwenden, damit Benutzeraktivitäten in Ihrer App auf der Windows-Zeitachse und Cortana „Begonnene Aufgaben” angezeigt werden.
- Verwenden von [Kacheln, Signale und Benachrichtigungen für UWP-Apps](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Eine vollständige Liste von Win32-APIs für UWP-Apps finden Sie unter [API-Sätze für UWP-Apps](https://msdn.microsoft.com/library/windows/desktop/mt186421) und [DLLs für UWP-Apps](https://msdn.microsoft.com/library/windows/desktop/mt186422).
- Eine Übersicht über das Schreiben von .NET UWP-Apps finden Sie unter [Universelle Windows-Apps in .NET](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net).
- Eine Liste der .NET-Typen, die Sie in einer UWP-App verwenden können, finden Sie unter [.NET für UWP-Apps](https://msdn.microsoft.com/library/mt185501.aspx)
- [.NET Native - Bedeutung für UWP-Entwickler (Universelle Windows-Plattform)](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
- Erfahren Sie, wie Sie moderne Erfahrungen für Benutzer von Windows 10 für Ihre vorhandene Desktop-App hinzufügen können und diese im Microsoft Store mit [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) veröffentlichen.

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Anwendbarkeit von universellen Windows-Plattform zu Windows-Runtime-APIs
Wenn Sie eine universelle Windows-Plattform (UWP)-app erstellen, können Sie einen Großteil Kilometer und Komfort aus die Begriffe "universelle Windows-Plattform (UWP)" und "Windows-Runtime (WinRT)" als mehr oder weniger Synonyme behandelt abrufen. Aber es *ist* möglich, hinter den Kulissen der Technologie suchen und zu ermitteln, was ist der Unterschied zwischen diesen Ideen. Wenn Sie wissen möchten, sind, ist dieser letzten Abschnitt für Sie.

Die Windows-Runtime und WinRT-APIs sind eine Weiterentwicklung des Windows-APIs. Windows wurde ursprünglich über flach, C-Stil Win32-APIs programmiert. Zu den wurden COM-APIs ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) wird ein Markantes Beispiel) hinzugefügt. Windows Forms, WPF, .NET und verwalteten Sprachen zusätzlich zu ihren eigenen Möglichkeit zur Erstellung von Windows-apps und ihre eigenen Typ des API-Technologie verwendet. Windows-Runtime ist, im Hintergrund die nächste Phase des COM Der Ebene tatsächlichen Application binary Interface (ABI) werden die Stämme in COM sichtbar. Aber die Windows-Runtime wurde entwickelt, um über eine große Palette von verschiedenen Programmiersprachen aufgerufen werden. Und in einer Weise, die sehr natürlich an die einzelnen Sprachen werden kann. Zu diesem Zweck ist Zugriff auf die Windows-Runtime verfügbar gemacht über was als sprachprojektionen bezeichnet werden. Es ist eine Windows-Runtime-sprachprojektion in c#, in Visual Basic, in C++-Standardbibliothek, in JavaScript und So weiter. Darüber hinaus einmal entsprechend verpackt (siehe [Desktop-Brücke](/windows/uwp/porting/desktop-to-uwp-root)), können Sie WinRT-APIs aufrufen, aus einer app in eine hervorragende Bereich von Anwendungsmodellen erstellt: Win32, .NET, WinForms und WPF.

Und natürlich können Sie WinRT-APIs aufrufen, von Ihrer UWP-app. UWP ist eine Anwendungsmodell basiert auf der Windows-Runtime. Technisch gesehen Modell der UWP-Anwendung [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), basiert auf auch dieses Detail aus, je nach Ihrer Wahl Programmiersprache ausgeblendet werden kann. Wie in diesem Thema, aus einem Wert nutzen Sicht beschrieben wurde, die UWP eignet sich für für Schreiben einer einzelnen Binärdatei, die können Sie auswählen sollen, an den Microsoft Store veröffentlicht und für eine beliebige eine große Palette von Geräte-Formfaktoren ausgeführt werden. Die Gerät Reichweite Ihrer UWP-App verwendet wird, hängt die Teilmenge von UWP-APIs, Ihre app mit einem Aufruf zu beschränken, oder dass Sie bedingt aufrufen.

Wir hoffen, dass wurde in diesem Abschnitt beschreiben die Differenz zwischen der zugrunde liegenden Windows-Runtime-APIs, und der Mechanismus und Geschäftswert der universellen Windows-Plattform-Technologie erfolgreich.
