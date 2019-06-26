---
title: Neuigkeiten in der Windows-Dokumentation im Juli 2017 – Entwicklung von UWP-Apps
description: Neue Features, Beispiele und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im Juli 2017
keywords: Neuigkeiten, Update, Features, Anleitungen für Entwickler, Windows 10
ms.date: 07/05/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 03bc41ca76f6c6b20f06c98b0e71e2bc5a361992
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "65984205"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2017"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im Juli 2017

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Codebeispiele wurden erst kürzlich bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/your-first-app.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="fluent-design"></a>Fluent Design

Diese neuen, jetzt für [Windows-Insider](https://insider.windows.com/) in SDK Preview Builds verfügbaren Effekte verwenden Tiefe, Perspektive und Bewegung, sodass die Benutzer sich auf wichtige Elemente der Benutzeroberfläche konzentrieren können.

[Acrylmaterial](../design/style/acrylic.md) ist eine Art von Pinsel, der eine transparente Texturen erzeugt. 

![Acryl in hellem Design](../design/style/images/Acrylic_DarkTheme_Base.png)

Der [Parallaxeneffekt](../design/motion/parallax.md) fügt Ihrer App dreidimensionale Tiefe und Perspektive hinzu.

![Beispiel für Parallaxe mit einer Liste und einem Hintergrundbild](../design/style/images/_Parallax_v2.gif)

[Einblendung](../design/style/reveal.md) hebt wichtige Elemente Ihrer App hervor. 

![Visuelle Einblendung](../design/style/images/Nav_Reveal_Animation.gif)

### <a name="ui-controls"></a>UI-Steuerelemente

Diese neuen Steuerelemente für [Windows-Insider](https://insider.windows.com/) in SDK Preview-Builds erleichtern die Erstellung einer gut gestalteten Benutzeroberfläche.

Das [Farbwähler-Steuerelement](../design/controls-and-patterns/color-picker.md) ermöglicht es Benutzern, Farben zu durchsuchen und auszuwählen.  

![Ein Standard-Farbwähler](../design/controls-and-patterns/images/color-picker-default.png)

Mit dem [Navigationsansichts-Steuerelement](../design/controls-and-patterns/navigationview.md) lassen sich ganz einfach Navigationselemente auf oberster Ebene zu Ihrer App hinzufügen.

![Abschnitte von NavigationView](../design/controls-and-patterns/images/navview_sections.png)

Das [Steuerelement für Bilder von Personen](../design/controls-and-patterns/person-picture.md) zeigt das Avatarbild für eine Person an.

![Das Personenbild-Steuerelement](../design/controls-and-patterns/images/person-picture/person-picture_hero.png)

Das [Bewertungssteuerelement](../design/controls-and-patterns/rating.md) ermöglicht es Benutzern, auf einfache Weise Bewertungen anzuzeigen und festzulegen, die den Grad der Zufriedenheit mit Inhalten und Diensten widerspiegeln.

![Beispiel mit einem Bewertungssteuerelement](../design/controls-and-patterns/images/rating_rs2_doc_ratings_intro.png)

### <a name="design-toolkits"></a>Design-Toolkits

Die [Design-Toolkits und Ressourcen für UWP-Apps](../design/downloads/index.md) wurden um die Sketch- and Adobe XD-Toolkits erweitert. Zudem wurden die bisherigen Toolkits aktualisiert und überarbeitet. Sie enthalten nun stabilere Steuerelemente und Layoutvorlagen für Ihre UWP-Apps.

### <a name="dashboard-monetization-and-store-services"></a>Dashboard, Monetarisierung und Store-Dienste

Die folgenden neuen Features sind nun verfügbar:

* Das Microsoft Advertising-SDK ermöglicht nun die Darstellung [nativer Anzeigen](../monetize/native-ads.md) in Ihren Apps. Eine native Anzeige ist ein komponentenbasiertes Anzeigenformat, in denen jedes Element der Anzeige (wie Titel, Bild, Beschreibung und Handlungsaufforderungstext) als einzelnes Element übermittelt wird, das Sie in Ihre App integrieren können. Native Anzeigen sind zurzeit nur für Entwickler in einem Pilotprogramm verfügbar, aber wir beabsichtigen, dieses Feature bald allen Entwicklern zur Verfügung zu stellen.

* Die [Microsoft Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md) verfügt nun über eine Methode, um die [CAB-Datei für einen App-Fehler herunterzuladen](../monetize/download-the-cab-file-for-an-error-in-your-app.md).

* Mit [gezielten Angeboten](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md) können Sie bestimmte Kundensegmente mit attraktivem, personalisiertem Inhalt ansprechen, um Kundenbindung und Monetarisierung zu verbessern. 

* Der Store-Eintrag für Ihre App kann nun [Videotrailer](../publish/app-screenshots-and-images.md#trailers) enthalten.

* Mit neuen Optionen für Preisgestaltung und Verfügbarkeit können Sie [Preisänderungen planen](../publish/set-and-schedule-app-pricing.md) und [genaue Freigabetermine festlegen](..//publish/configure-precise-release-scheduling.md).

* Sie können einen [Store-Eintrag importieren und exportieren](../publish/import-and-export-store-listings.md) und damit Updates beschleunigen, insbesondere dann, wenn Sie über Einträge in vielen Sprachen verfügen.

### <a name="my-people"></a>Meine Kontakte

Das neue Feature „Meine Kontakte” ist für [Windows-Insider](https://insider.windows.com/) in SDK Preview-Builds verfügbar. Damit können Benutzer Kontakte aus einer Anwendung direkt an die Taskleiste anheften. [Erfahren Sie, wie Sie „Meine Kontakte” in Ihre Anwendung einfügen.](../contacts-and-calendar/my-people-support.md)

![„Meine Kontakte”-Kontaktliste](images/my-people.png)

Mit [Freigeben für „Meine Kontakte”](../contacts-and-calendar/my-people-sharing.md) können Benutzer Dateien mithilfe Ihrer Anwendung direkt über die Taskleiste freigeben.

![Freigeben von „Meine Kontakte”](images/my-people-sharing.png)

[Benachrichtigungen für „Meine Kontakte“](../contacts-and-calendar/my-people-support.md) sind eine neue Art von Popupbenachrichtigungen, die Benutzer an ihre angehefteten Kontakte senden können.

![Benachrichtigungen für „Meine Kontakte“](images/my-people-notification.png)

### <a name="pin-to-taskbar"></a>An Taskleiste anheften

Die neue TaskbarManager-Klasse ist für [Windows-Insider](https://insider.windows.com/) in SDK Preview-Builds verfügbar. Sie können Benutzer damit auffordern, [Ihre App an die Taskleiste anzuheften](../design/shell/pin-to-taskbar.md).

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="media-playback"></a>Medienwiedergabe

Dem grundlegenden Artikel [Wiedergeben von Audio- und Videoinhalten mit MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) wurden neue Abschnitte hinzugefügt. Im Abschnitt [Wiedergeben von sphärischen Videos mit MediaPlayer](../audio-video-camera/play-audio-and-video-with-mediaplayer.md) wird gezeigt, wie Sie sphärisch codiertes Videomaterial wiedergeben und Sichtfeld sowie Ausrichtung für unterstützte Videoformate anpassen. Der Abschnitt [Verwenden von MediaPlayer im Frame-Server-Modus](../audio-video-camera/play-audio-and-video-with-mediaplayer.md#use-mediaplayer-in-frame-server-mode) erläutert, wie Sie Frames aus einer Medienwiedergabe mit [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) auf eine Direct3D-Oberfläche kopieren. Dies ermöglicht Szenarien wie etwa das Anwenden von Echtzeiteffekten mit Pixel-Shadern. Der Beispielcode zeigt die schnelle Implementierung eines Weichzeichnereffekts für die Wiedergabe von Videos mithilfe von Win2D.

### <a name="media-capture"></a>Medienerfassung

Der Artikel [Verarbeiten von Medienframes mit MediaFrameReader](../audio-video-camera/process-media-frames-with-mediaframereader.md) wurde aktualisiert, um die Verwendung der neuen Klasse [MultiSourceMediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) zu erläutern, mit deren Hilfe Sie zeitkorrelierte Frames aus mehreren Medienquellen abrufen können. Dies ist hilfreich, wenn Sie Frames aus anderen Quellen wie einer Tiefenkamera oder Farbkamera verarbeiten und dabei sicherstellen müssen, dass die Frames aus jeder Quelle zeitnah zueinander erfasst wurden. Weitere Informationen finden Sie unter [Verwenden von MultiSourceMediaFrameReader zum Abrufen zeitkorrelierter Frames aus mehreren Quellen](../audio-video-camera/process-media-frames-with-mediaframereader.md#use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources).

### <a name="scoped-search"></a>Begrenzter Suchbereich

Der [konzeptionellen UWP-Dokumentation](../get-started/universal-application-platform-guide.md) und der [API-Referenz](https://docs.microsoft.com/en-us/uwp/api/) auf „docs.microsoft.com“ wurde der Bereich „UWP” hinzugefügt. Sofern dieser Bereich nicht deaktiviert ist, geben Suchvorgänge in diesen Umgebungen nur UWP-Dokumente zurück.

![Begrenzter Suchbereich](images/scoped-search.png)

### <a name="test-your-windows-app-for-windows-10-s"></a>Testen Ihrer Windows-App für Windows 10 S

Testen Sie Ihre Windows-App, um sicherzustellen, dass sie korrekt auf Geräten unter Windows S ausgeführt wird. Anleitungen dazu finden Sie in [diesem neuen Handbuch](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-test-windows-s).

## <a name="samples"></a>Beispiele

### <a name="annotated-audio-app-sample"></a>Beispiel für kommentierte Audio-App

[Ein Beispiel für eine Mini-App mit Audio-, Freihand- und OneDrive-Datenroamingszenarien.](https://github.com/Microsoft/Windows-appsample-annotated-audio) Dieses Beispiel zeichnet Audio auf und erlaubt gleichzeitig die synchronisierte Erfassung von Freihandanmerkungen, sodass Sie später nachvollziehen können, was besprochen wurde, als Sie die Notiz erstellt haben.

![Screenshot des kommentierten Audioaufbeispiels](images/Playback.png)  

### <a name="shopping-app-sample"></a>Beispiel für eine Shopping-App

[Ein Mini-App für einen einfachen Einkaufsvorgang, bei dem ein Benutzer Emojis erwerben kann](https://github.com/Microsoft/Windows-appsample-shopping). Diese App veranschaulicht, wie die Kaufabwicklung mit den [Payment Request-APIs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) zu implementieren ist.

![Screenshot des Shopping-App-Beispiels](images/shoppingcart.png)  

## <a name="videos"></a>Videos

### <a name="accessibility"></a>Bedienungshilfen

Durch Bedienungshilfen in Ihren Apps erreichen Sie viel größere Zielgruppen. [Sehen Sie sich das Video an](https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility), und erfahren Sie dann mehr über das [Entwickeln von Apps mit Barrierefreiheit](https://developer.microsoft.com/en-us/windows/accessible-apps).

### <a name="payments-request-api"></a>Payments Request-API

Die Payment Request-API ermöglicht Kunden und Verkäufern eine nahtlose, verbesserte Onlineverkaufsabwicklung. [Sehen Sie sich das Video an](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API), und lesen Sie anschließend die [Payment Request-Dokumentation](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-the-Payments-Request-API).

### <a name="windows-10-iot-core"></a>Windows 10 IoT Core

Windows 10 IoT Core und die Universelle Windows-Plattform ermöglichen, schnell Prototypen und Projekte mit unterschiedlichen Komponenten zu erstellen. Ein Beispiel ist diese Tür, die Ihr Haustier erkennt. [Sehen Sie sich das Video an](https://channel9.msdn.com/Blogs/One-Dev-Minute/Building-a-Pet-Recognition-Door-Using-Windows-10-IoT-Core), und erfahren Sie dann mehr über [erste Schritte mit Windows 10 IoT Core](https://developer.microsoft.com/en-us/windows/iot).