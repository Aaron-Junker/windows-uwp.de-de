---
title: Was ist eine App der universellen Windows-Plattform (UWP)?
description: Erfahren Sie mehr über Apps für die Universelle Windows-Plattform (UWP), die auf einer Vielzahl von Geräten unter Windows 10 ausgeführt werden können.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, Uwp, universal
ms.localizationpriority: medium
ms.openlocfilehash: 37207d4ce65551a7bdd33d57f72f3fa6a0a6185d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370698"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Was ist eine App der universellen Windows-Plattform (UWP)?

![Apps der universellen Windows-Plattform auf einer Vielzahl von Geräten ausgeführt, adaptive Benutzeroberfläche, natürlichen, Geschäft, Partnercenter zu unterstützen und cloud-Dienste](images/universalapps-overview.png)

Eine UWP-App ist:

- Sichern: UWP-apps deklarieren, welche Ressourcen und Daten, die sie zugreifen. Der Benutzer muss den Zugriff autorisieren.
- Fähig, eine allgemeine API auf allen Geräten zu verwenden, auf denen Windows 10 ausgeführt wird.
- Können bestimmte Gerätefunktionen verwenden und Anpassen der Benutzeroberflächenautomatisierungs auf anderes Gerät Bildschirmgrößen, Auflösungen und DPI-Wert.
- Verfügbar aus dem Microsoft Store für alle Geräte (oder nur solche, die Sie angeben), auf denen Windows 10 ausgeführt wird. Das Microsoft Store bietet mehrere Möglichkeiten, mit Ihrer App Geld zu verdienen.
- Fähig, ohne Risiko für den Computer oder Maschinenschaden installiert und deinstalliert zu werden.
- Ansprechend: Verwenden Sie Live-Kacheln, Pushbenachrichtigungen und Benutzeraktivitäten, um mit der Windows-Zeitachse und Cortana „Begonnene Aufgaben” zu interagieren und Benutzer zu erreichen.
- Programmierbar in C#, C++, Visual Basic und Javascript. Verwenden Sie für die Benutzeroberfläche XAML, HTML oder DirectX.

Betrachten wir dies im Detail.

## <a name="secure"></a>Secure

UWP-Apps deklarieren im Manifest die Gerätefunktionen, die sie benötigen wie z. B. den Zugriff auf das Mikrofon, Standort, Webcam, USB-Geräte, Dateien usw. Der Benutzer muss den Zugriff bestätigen und autorisieren, bevor die App die Funktion nutzen kann.

## <a name="a-common-api-surface-across-all-devices"></a>Es gibt eine gemeinsame API-Oberfläche für alle Geräte

Windows 10 stellt die universelle Windows-Plattform (UWP), der eine allgemeine app-Plattform auf jedem Gerät bereitstellt, die Windows 10 ausgeführt wird. Die wichtigsten UWP-APIs sind auf allen Windows-Geräten identisch. Wenn Ihre app nur die Kern-APIs verwendet, wird es auf Windows 10-Geräten unabhängig davon, ob das Ziel eines desktop-PCs, Xbox, Mixed Reality-Kopfhörer, werden ausgeführt und so weiter.

Eine UWP-App die in C++ /WinRT oder C++ /CX geschrieben wurde, hat Zugriff auf die Win32 APIs, die Teil der universellen Windows-Plattform (UWP) sind. Diese Win32-APIs werden von allen Windows 10-Geräten implementiert.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Erweiterungs-SDKs bieten die einzigartigen Funktionen der jeweiligen Gerätetypen

Wenn Sie die universellen APIs verwenden, kann Ihre App auf allen Geräten ausgeführt werden, die Windows 10 ausführen. Wenn Sie Ihre UWP-App für gerätespezifische APIs nutzen möchten, ist dies möglich.

Mit Erweiterungs-SDKs können Sie spezielle APIs für verschiedene Geräte aufrufen. Wenn Ihre UWP-App ein IoT-Gerät als Ziel hat, können Sie z. B. das IoT-Erweiterungs-SDK auf das Projekt hinzufügen, um spezifische Funktionen anzuvisieren, die speziell für IoT-Geräten gelten. Weitere Informationen zum Hinzufügen von Erweiterungs-SDKs finden Sie unter im Abschnitt **Erweiterungs-SDKs** unter [Übersicht über die Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Sie können Ihre App so entwerfen, dass sie nur auf einem bestimmten Gerätetyp ausgeführt werden kann, und den Verteilungspunkt aus dem Microsoft Store nur auf einen Gerätetyp beschränken. Oder Sie können bedingt testen, ob eine API zur Laufzeit vorhanden ist und das Verhalten Ihrer App entsprechend anpassen. Weitere Informationen finden Sie im Abschnitt **Schreiben von Code** unter [Übersicht über die Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code).<br>

Das folgende Video bietet einen kurzen Überblick über die Gerätefamilien und adaptiven Code:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Adaptive Steuerelemente und Eingaben

UI-Elemente reagieren auf die Größe und DPI-Werte des Fensters der App, indem Sie das Layout und die Skalierung anpassen. UWP-Apps funktionieren außerdem mit mehreren Arten von Eingaben, z. B. Tastatur, Maus, Touch, Stift und Xbox One-Controller. Wenn Sie die Benutzeroberfläche auf eine bestimmte Bildschirmgröße oder ein bestimmtes Gerät anpassen müssen, helfen Ihnen neue Layoutbereiche und Tools dabei, die Benutzeroberfläche auf den Geräten so zu erstellen, dass sie auf die verschiedenen Geräte und Formfaktoren angepasst werden kann, auf denen Ihre App möglicherweise ausgeführt wird.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

Windows unterstützt Sie mit folgenden Features, Ihre UI auf mehrere Geräte auszurichten:

- Universelle Steuerelemente und Layoutbereiche können Sie bei der Optimierung Ihrer UI für die Bildschirmauflösung des Geräts unterstützen Steuerelemente wie Schaltflächen und Schieberegler werden automatisch nach Bildschirmgröße und DPI-Dichte angepasst. Layoutpanels können das Layout von Inhalten basierend auf der Größe des Bildschirms anpassen. Die adaptive Skalierung passt die Auflösung und DPI-Unterschiede auf allen Geräten an.
- Mit der allgemeinen Eingabebehandlung können Sie Benutzereingaben per Toucheingabe, Stift, Maus, Tastatur oder Controller (Microsoft Xbox-Controller oder Ähnliches) erhalten.
- Tools können Sie bei dem Entwerfen der UI unterstützen, sodass sie sich an verschiedene Bildschirmauflösungen anpasst.

Einige Aspekte der App-UI Ihrer App werden automatisch auf allen Geräten angepasst. Das Design der Benutzererfahrung Ihrer App muss jedoch möglicherweise angepasst werden, je nachdem, auf welchem Gerät die App ausgeführt wird. Eine Foto-App sollte die UI beispielsweise anpassen, wenn sie auf einem kleinen, tragbaren Gerät ausgeführt wird, um sicherzustellen, dass sie optimal mit einer Hand bedient werden kann. Wenn die Foto-App auf einem Desktopcomputer ausgeführt wird, sollte sich die Benutzeroberfläche anpassen, um die zusätzliche Bildschirmfläche zu nutzen.

## <a name="theres-one-store-for-all-devices"></a>Es gibt einen Store für alle Geräte

Ein einheitliche app-Store stellt Ihre app für Windows 10-Geräte wie etwa PC, Tablet, Xbox, HoloLens, Surface Hub und Internet der Dinge (IoT) Geräte zur Verfügung. Sie können Ihre App an den Store übermitteln und auf für Geräte aller Art oder nur für bestimmte Geräte zur Verfügung stellen. Sie übermitteln und verwalten alle Ihre Apps für Windows-Geräte an einem zentralen Ort. Sie haben eine mit C++ entwickelte Desktop-App, die Sie mit UWP-Features modernisieren und im Microsoft Store verkaufen möchten? Das ist auch möglich.

UWP-Apps werden mit [Application Insights](https://azure.microsoft.com/services/application-insights/) für detaillierte Telemetrie und Analyse integriert. Dies ist ein besonders wichtiges Tool zum Verständnis der Anwender und zur Verbesserung Ihrer Apps.

### <a name="monetize-your-app"></a>Monetisierung Ihrer App

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
- Per Sprache und mit Bluetooth LE-Geräten kann Ihre App Benutzern die Interaktion mit der Welt um sie herum ermöglichen.
- Integrieren Sie Cortana für Befehl der Sprachfunktionalität von Ihrer App.

##  <a name="use-a-language-you-already-know"></a>Verwenden einer vertrauten Sprache

UWP-Apps verwenden die Windows-Runtime. Dies ist eine native, vom Betriebssystem integrierte API. Diese API ist in C++ implementiert und wird in C#, Visual Basic, C++ und JavaScript unterstützt. Einige Optionen für das Schreiben von Apps in UWP sind:

- XAML-UI und C#, VB oder C++
- DirectX UI und C++
- JavaScript und HTML

## <a name="links-to-help-you-get-going"></a>Links für die ersten Schritte

### <a name="get-set-up"></a>Einrichten

Navigieren Sie zu [Vorbereiten](get-set-up.md), um die Tools herunterzuladen, die Sie benötigen, um mit dem Erstellen von Apps zu beginnen, und [schreiben Sie Ihre erste App](your-first-app.md)!

### <a name="design-your-app"></a>Entwerfen Sie Ihre App

Das Microsoft-Entwurfssystem heißt Fluent. Das Fluent Design System stellt eine Reihe von UWP-Funktionen in Kombination mit bewährten Verfahrensweisen für die Erstellung von Apps dar, die auf allen Arten von Windows-basierten Geräten hervorragend funktionieren. Fluent-Umgebungen sind anpassungsfähig und fühlen sich auf Geräten wie Tablets, Laptops, PCs, Fernsehern und Virtual-Reality-Geräten ganz natürlich an. Weitere Informationen über Fluent Design finden Sie unter [Das Fluent Design System für UWP-Apps](https://docs.microsoft.com/windows/uwp/design/fluent-design-system).

Zu einem guten [Design](https://go.microsoft.com/fwlink/?LinkId=258848) gehören nicht nur das gute Aussehen und die Funktionen einer App, sondern auch die Entscheidung darüber, wie Benutzer mit der App interagieren. Die Benutzerfreundlichkeit spielt eine große Rolle bei der Beurteilung, wie gerne Benutzer Ihre App verwenden. Sparen Sie daher nicht an diesem Schritt. [Designgrundlagen](https://developer.microsoft.com/en-us/windows/apps/design) bieten eine Einführung in das Design von Apps für die Universelle Windows-App. Unter [Einführung in universelle Windows-Plattform-Apps (UWP) für Designer](https://docs.microsoft.com/windows/uwp/layout/design-and-ui-intro) finden Sie Informationen zum Entwerfen von UWP-Apps, die Benutzer begeistern. Bevor Sie mit dem Schreiben von Code beginnen, lesen Sie die Informationen unter [Einführung der Geräte](../design/devices/index.md). Diese helfen Ihnen dabei, die Interaktionsmöglichkeiten Ihrer App für alle in Frage kommenden Formfaktoren zu durchdenken.

Zusätzlich zur Interaktion auf verschiedenen Geräten sollten Sie [Ihre App planen](https://docs.microsoft.com/windows/uwp/get-started/plan-your-app), um die Vorteile verschiedener Geräte optimal zu nutzen. Zum Beispiel:

- Entwerfen Sie Ihren Workflow anhand von [Navigationsdesigngrundlagen für UWP-Apps](https://docs.microsoft.com/windows/uwp/layout/navigation-basics), damit die App auf dem Bildschirm eines mobilen Geräts genauso gut funktioniert wie auf Geräten mit mittelgroßem und großem Bildschirm. [Erstellen Sie das Layout der Benutzeroberfläche](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design), um unterschiedliche Bildschirmgrößen und Auflösungen zu berücksichtigen.

- Achten Sie darauf, wie Sie verschiedene Eingabearten umsetzen können. In den [Richtlinien für Interaktionen](https://developer.microsoft.com/windows/design/inputs-devices) finden Sie Informationen darüber, wie Benutzer anhand von Features wie [Cortana](https://docs.microsoft.com/windows/uwp/input-and-devices/cortana-design-guidelines), [Sprache](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions), [Toucheingaben](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-user-interaction), der [Touch-Bildschirmtastatur](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) und vielem mehr mit Ihrer App interagieren.  In den [Richtlinien für Text und Texteingabe](https://docs.microsoft.com/windows/uwp/controls-and-patterns/text-controls) finden Sie Informationen für eine herkömmlichere Benutzerumgebung.

### <a name="add-services"></a>Dienste hinzufügen

- Verwenden Sie [Clouddienste](https://go.microsoft.com/fwlink/?LinkId=526377) für die Synchronisierung auf allen Geräten.
- Erfahren Sie, wie Sie eine [Verbindung mit Webdiensten](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10)) zur Unterstützung der App-Benutzerumgebung herstellen.
- Finden Sie heraus, wie Sie [Cortana zu Ihrer App hinzufügen können](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382), damit sie auf Sprachbefehle reagiert.
- Beziehen Sie [Push-Benachrichtigungen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) und [In-App-Einkäufe](https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases) in Ihre Planung mit ein. Diese Features sollten auf allen Geräten funktionieren.

### <a name="submit-your-app-to-the-store"></a>Übermitteln Ihrer App an den Store

[Partner Center](https://partner.microsoft.com/dashboard) ermöglicht Ihnen das Verwalten und übermitteln all Ihre apps für Windows-Geräte an einem Ort. Finden Sie unter [veröffentlichen Windows-apps und Spiele](../publish/index.md) zu erfahren, wie Sie Ihre apps für die Veröffentlichung in den Microsoft Store übermitteln.

Neue Features vereinfachen Prozesse und geben Ihnen mehr Kontrolle. Sie finden dort auch detaillierte [Analyseberichte](https://docs.microsoft.com/windows/uwp/publish/analytics) in Kombination mit [Auszahlungsdetails](https://docs.microsoft.com/windows/uwp/publish/payout-summary), Möglichkeiten, [Ihre App zu bewerben und Kunden zu erreichen](https://docs.microsoft.com/windows/uwp/publish/app-promotion-and-customer-engagement) und vieles mehr.

Weitere einführende Informationen finden Sie unter [Einführung in das Entwickeln von Windows-Apps für Windows 10-Geräte](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>Fortgeschrittenere Themen

- Hier erfahren Sie, wie Sie [Benutzeraktivitäten](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) verwenden, damit Benutzeraktivitäten in Ihrer App auf der Windows-Zeitachse und Cortana „Begonnene Aufgaben” angezeigt werden.
- Verwenden von [Kacheln, Signale und Benachrichtigungen für UWP-Apps](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Eine vollständige Liste von Win32-APIs für UWP-Apps finden Sie unter [API-Sätze für UWP-Apps](https://docs.microsoft.com/previous-versions//mt186421(v=vs.85)) und [DLLs für UWP-Apps](https://docs.microsoft.com/previous-versions//mt186422(v=vs.85)).
- Eine Übersicht über das Schreiben von .NET UWP-Apps finden Sie unter [Universelle Windows-Apps in .NET](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/).
- Eine Liste der .NET-Typen, die Sie in einer UWP-App verwenden können, finden Sie unter [.NET für UWP-Apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
- [Kompilieren von apps mit .NET Native](https://docs.microsoft.com/dotnet/framework/net-native/)
- Erfahren Sie, wie Sie moderne Erfahrungen für Benutzer von Windows 10 für Ihre vorhandene Desktop-App hinzufügen können und diese im Microsoft Store mit [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) veröffentlichen.

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Beziehung von der universellen Windows-Plattform zu Windows-Runtime-APIs
Wenn Sie eine app für die universelle Windows-Plattform (UWP) erstellen, können Sie viele der Unterschiede und benutzerfreundlichkeit aus behandeln die Begriffe "universelle Windows-Plattform (UWP)" und "Windows-Runtime (WinRT)" als mehr oder weniger gleichbedeutend abrufen. Sondern *ist* hinter den Kulissen von der Technologie, und bestimmen nur Was ist der Unterschied zwischen diesen Ideen möglich. Wenn Sie neugierig sind, sind, ist in diesem letzten Abschnitt für Sie.

Die Windows-Runtime und WinRT-APIs sind eine Weiterentwicklung der Windows-APIs. Windows wurde ursprünglich über flachen, Win32-APIs der C-Stil so programmiert. Zu den COM-APIs hinzugefügt wurden ([DirectX](https://docs.microsoft.com/windows/desktop/directx) wird ein prominentes Beispiel). Windows Forms, WPF, .NET und verwalteten Sprachen übertragen ihre eigenen Verfahren zum Erstellen von Windows-apps und ihre eigenen Flavor-API-Technologie. Die Windows-Runtime ist, im Hintergrund die nächste Phase von COM. Auf der Ebene tatsächlich verwendeten Anwendung anwendungsbinärschnittstelle (ABI) werden seine Wurzeln in COM sichtbar. Aber die Windows-Runtime wurde entworfen, um aus einer große Palette an verschiedenen Programmiersprachen aufgerufen werden können. Und auf eine Weise, die sehr intuitiv für jede dieser Sprachen ist aufgerufen. Zu diesem Zweck wird Zugriff auf die Windows-Runtime verfügbar gemacht über sprachprojektionen sogenannte. Es ist eine Windows-Runtime-sprachprojektion in C#, in Visual Basic, in standard C++, JavaScript, und So weiter. Darüber hinaus einmal ordnungsgemäß gepackt (finden Sie unter [Desktop-Brücke](/windows/uwp/porting/desktop-to-uwp-root)), können Sie WinRT-APIs von Apps in einer von einem hervorragenden Bereich Anwendungsmodelle aufrufen: Win32, .NET, WinForms und WPF.

Und natürlich können Sie WinRT-APIs von UWP-app aufrufen. UWP ist ein Anwendungsmodell basiert auf der Windows-Runtime. Technisch gesehen das UWP-Anwendungsmodell basiert ["coreapplication"](/uwp/api/windows.applicationmodel.core.coreapplication), obwohl diese Details aus, je nach verwendeter Programmiersprache verborgen sein kann. Wie in diesem Thema, aus einer Wert-Option der Sicht beschrieben wurde, bietet die UWP sich zum Schreiben von einer einzelnen Binärdatei, die können Sie auswählen sollten, auf dem Microsoft Store veröffentlicht werden und auf einem eine große Palette von geräteausführungen ausgeführt werden. Die geräteunterstützung der UWP-app verwendet wird, hängt die Teilmenge der UWP-APIs, dass Sie Ihre app aufrufen begrenzen oder bedingt aufrufen.

Hoffentlich war in diesem Abschnitt beschreiben die Differenz zwischen der Technologie, die zugrunde liegenden Windows-Runtime-APIs, und der Mechanismus und Business-Wert, der universellen Windows-Plattform erfolgreich.
