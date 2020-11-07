---
title: Handbuch zur Entwicklung von Spielen unter Windows 10
description: Ein umfassender Leitfaden für Ressourcen und Informationen zur Entwicklung von Spielen für die universelle Windows-Plattform (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Spiele, Spieleentwicklung
ms.localizationpriority: medium
ms.openlocfilehash: be63c7ec1e682924e2364a2c6ea8e4d2d81dee3f
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339508"
---
# <a name="windows-10-game-development-guide"></a>Handbuch zur Entwicklung von Spielen unter Windows 10

Willkommen beim Windows 10-Handbuch für die Entwicklung von Spielen!

Dieses Handbuch enthält eine umfassende Sammlung von Ressourcen und Informationen, die Sie für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP) benötigen. Eine englische Version (US) dieses Handbuchs ist im [PDF-](https://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) Format verfügbar.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Einführung in die Spieleentwicklung für die Universelle Windows-Plattform (UWP)

Wenn Sie ein Spiel für Windows 10 entwickeln, haben Sie die Möglichkeit, Millionen von Spielern weltweit über Smartphone, PC und Xbox One zu erreichen. Mit der Xbox für Windows, Xbox Live, geräteübergreifenden Multiplayer-Spielen, einer fantastischen Spielcommunity und leistungsstarken neuen Features wie der Universellen Windows-Plattform (UWP) und DirectX 12, begeistern Windows 10-Spiele Spieler unabhängig von Alter und Geschlecht. Die neue Universelle Windows-Plattform (UWP) bietet Kompatibilität für Ihr Spiel mit allen Windows 10-Geräten über eine API für Smartphones, PCs und Xbox One sowie Tools und Optionen, um das Spielerlebnis für unterschiedliche Gerätetypen anpassen zu können.

Dieses Handbuch enthält eine umfassende Sammlung von hilfreichen Informationen und Ressourcen für die Spieleentwicklung. Die Abschnittsstruktur orientiert sich an den Entwicklungsphasen und vereinfacht die Informationssuche.

Wenn Sie noch nicht [mit der Entwicklung](getting-started.md) von Spielen auf Windows oder Xbox vertraut sind, können Sie mit dem Leitfaden "erste Schritte" beginnen. Der Abschnitt [Spiele Entwicklungsressourcen](#game-development-resources) bietet auch eine allgemeine Übersicht über Dokumentation, Programme und andere Ressourcen, die bei der Erstellung eines Spiels hilfreich sind. Wenn Sie stattdessen einen UWP-Code betrachten möchten, finden Sie weitere Informationen unter [Game Samples](#game-samples).

Dieses Handbuch wird bei Bedarf mit weiteren Ressourcen für die Entwicklung von Windows 10-Spielen aktualisiert.

## <a name="game-development-resources"></a>Ressourcen für die Spieleentwicklung

Von der Dokumentation bis hin zu Entwicklerprogrammen, Foren, Blogs und Beispielen steht Ihnen bei der Spieleentwicklung eine Vielzahl hilfreicher Ressourcen zur Verfügung. Hier finden Sie eine Zusammenfassung wichtiger Ressourcen für den Einstieg in die Entwicklung Ihres Windows 10-Spiels.

> [!Note]
> Einige Features werden über verschiedene Programme verwaltet. Dieses Handbuch behandelt eine breite Palette von Ressourcen. Je nach Programmteilnahme oder spezifischer Entwicklungsrolle stehen Ihnen bestimmte Ressourcen unter Umständen nicht zur Verfügung. Beispiele wären etwa Links, die zu „developer.xboxlive.com“, „forums.xboxlive.com“, „xdi.xboxlive.com“ oder zum Netzwerk für Spieleentwickler (Game Developer Network, GDN) aufgelöst werden. Informationen zur Partnerschaft mit Microsoft finden Sie unter [Entwicklerprogramme](#developer-programs).

### <a name="game-development-documentation"></a>Dokumentation für die Spieleentwicklung

In diesem Handbuch finden Sie immer wieder direkte Links zu relevanten Dokumentationen – strukturiert nach Aufgabe, Technologie und Entwicklungsphase. Hier sehen Sie eine Übersicht über die wichtigsten verfügbaren Dokumentationsportale für die Entwicklung von Windows 10-Spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Hauptportal für Windows Dev Center</td>
        <td><a href="https://developer.microsoft.com/windows">Windows Developer Center</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows-Apps</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Windows-Apps entwickeln</a></td>
    </tr>
    <tr>
        <td>Entwicklung von UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.microsoft.com/windows/apps">Anleitungen für Windows 10-Apps</a></td>
    </tr>
    <tr>
        <td>Anleitungen für UWP-Spiele</td>
        <td><a href="index.md">Spiele und DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX-Referenz und -Übersichten</td>
        <td><a href="/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Azure für Gaming</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Erstellen und Skalieren von Spielen mit Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Complete Backend-Lösung für Live Spiele</a></td>
    </tr>
    <tr>
        <td>UWP auf Xbox One</td>
        <td><a href="/windows/uwp/xbox-apps/index">Erstellen von UWP-Apps auf Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP in hololens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Entwickeln von UWP-apps auf hololens</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Dokumentation</td>
        <td><a href="/gaming/xbox-live/">Xbox Live – Entwicklerhandbuch</a></td>
    </tr>
    <tr>
        <td>Xbox One-Entwicklungsdokumentation (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One-Entwicklung</a></td>
    </tr>
    <tr>
        <td>Whitepaper zur Xbox One-Entwicklung (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Whitepaper</a></td>
    </tr>
    <tr>
        <td>Interaktive mixerdokumentation</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Hinzufügen von Interaktivität zu Ihrem Spiel</a></td>
    </tr>
</table>

### <a name="partner-center"></a>Partner Center

Das [Registrieren eines Entwickler Kontos im Partner Center](https://developer.microsoft.com/store/register) ist der erste Schritt zum Veröffentlichen Ihres Windows-Spiels. Mit einem Entwicklerkonto können Sie den Namen Ihres Spiels reservieren und kostenlose oder kostenpflichtige Spiele an die Microsoft Store für alle Windows-Geräte senden. Sie können über Ihr Entwicklerkonto Ihr Spiel und Ihre spielinternen Produkte verwalten, ausführliche Analysen abrufen und Dienste aktivieren, die Spieler auf der ganzen Welt begeistern.

Außerdem bietet Microsoft mehrere Entwickler Programme, die Sie beim entwickeln und Veröffentlichen von Windows-spielen unterstützen. Wir empfehlen Ihnen, vor der Registrierung für ein Partner Center-Konto zu sehen, ob Sie für Sie geeignet sind. Weitere Informationen finden Sie unter [Developer-Programme](#developer-programs) .

### <a name="developer-programs"></a>Entwicklerprogramme

Microsoft bietet mehrere Entwicklerprogramme an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen. Sie sollten einem Entwicklerprogramm beitreten, wenn Sie Spiele für Xbox One entwickeln und Xbox Live-Features in Ihr Spiel integrieren möchten. Wenn Sie ein Spiel in der Microsoft Store veröffentlichen möchten, müssen Sie auch ein Entwicklerkonto im [Partner Center](https://partner.microsoft.com/dashboard) erstellen.

#### <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Das Xbox Live Creators-Programm ermöglicht allen Benutzern, Xbox Live in ihren Titel zu integrieren und auf Xbox One und Windows 10 zu veröffentlichen. Es ist ein vereinfachtes Zertifizierungsverfahren vorhanden, und es ist keine Konzept Genehmigung außerhalb der standardmäßigen [Microsoft Store-Richtlinien](/legal/windows/agreements/store-policies)erforderlich.

Sie können Ihr Spiel im Creators-Programm ohne dediziertes dev-Kit bereitstellen, entwerfen und veröffentlichen. dabei wird nur die Einzelhandels Hardware verwendet. Laden Sie zunächst die Entwickler [Modus-Aktivierungs-App](../xbox-apps/devkit-activation.md) auf Ihrer Xbox One herunter.

Wenn Sie noch mehr über die Xbox Live-Funktionen, dedizierte Marketing-und Entwicklungsunterstützung und die Gelegenheit verfügen möchten, im Hauptspeicher von Xbox One zu finden, wenden Sie sich auf das [ID@Xbox](https://www.xbox.com/Developers/id) Programm an.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live Creators-Programm</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Erfahren Sie mehr über das Xbox Live Creators-Programm</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

Das ID@Xbox Programm unterstützt qualifizierte Spieleentwickler bei der Selbstveröffentlichung in Windows und Xbox One. Wenn Sie für Xbox One entwickeln oder Xbox Live-Features wie Gamerscore, Erfolge und Bestenlisten zu Ihrem Windows 10-Spiel hinzufügen möchten, registrieren Sie sich mit ID@Xbox . Entwickeln ID@Xbox Sie Entwickler, um Tools und Unterstützung zu erhalten, die Sie benötigen, um Ihre Kreativität zu steigern und ihren Erfolg zu maximieren. Es wird empfohlen, dass Sie zuerst auf anwenden, bevor Sie sich ID@Xbox für ein Entwicklerkonto im Partner Center registrieren.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox Entwicklerprogramm</td>
        <td><a href="https://www.xbox.com/Developers/id">Unabhängiges Entwicklerprogramm für Xbox One</a></td>
    </tr>
    <tr>
        <td>ID@Xbox consumersite</td>
        <td><a href="https://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Xbox-Tools und Middleware

Im Rahmen des Programms für Xbox-Tools und Middleware werden Xbox-Entwicklungskits für professionelle Entwickler von Spieletools und Middleware lizenziert. Entwickler, die in das Programm aufgenommen werden, können ihre Xbox XDK-Technologien an andere lizenzierte Xbox-Entwickler weitergeben und vertreiben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programm für Tools und Middleware kontaktieren</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>

### <a name="game-samples"></a>Beispielspiele

Für Windows 10-Spiele und -Apps stehen zahlreiche Beispiele zur Verfügung, die einen Eindruck von den Features von Windows 10-Spielen vermitteln und den Einstieg in die Spieleentwicklung erleichtern. Es werden regelmäßig weitere Beispiele entwickelt und veröffentlicht. Schauen Sie daher immer mal wieder bei den Beispielportalen vorbei. Darüber hinaus können Sie GitHub-Repositorys [überwachen](https://help.github.com/en/articles/watching-and-unwatching-repositories), um über Änderungen und Ergänzungen informiert zu werden.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Beispiele für Universelle Windows-Plattform-Apps</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Grafikbeispiele für Direct3D 12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Grafikbeispiele</a></td>
    </tr>
    <tr>
        <td>Grafikbeispiele für Direct3D 11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein First-Person-Spiel mit Direct3D 11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>Beispiel für benutzerdefinierte Direct2D-Bildeffekte</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DCustomEffects">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Direct2D-Farbverlaufsgitter</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DGradientMesh">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Beispiel für eine Direct2D-Fotoanpassung</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DPhotoAdjustment">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Xbox Advanced Technology Group – öffentliche Beispiele</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Beispiele</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">Xbox-Live-Samples</a></td>
    </tr>
    <tr>
        <td>Xbox One-Spiel Beispiele (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Beispiele</a></td>
    </tr>
    <tr>
        <td>Windows-Spiel Beispiele (MSDN Code Gallery)</td>
        <td><a href="/samples/browse/?term=games">Microsoft Store von Spiel Beispielen</a></td>
    </tr>
    <tr>
        <td>JavaScript-Beispiel für 2D-Spiel</td>
        <td><a href="/windows/uwp/get-started/">Erstellen eines UWP-Spiels in JavaScript</a></td>
    </tr>
    <tr>
        <td>JavaScript 3D-Spielbeispiel</td>
        <td><a href="/windows/uwp/get-started/">Erstellen eines 3D-JavaScript-Spiels mit „three.js“</a></td>
    </tr>
    <tr>
        <td>Monogame 2D UWP-Spielbeispiel</td>
        <td><a href="/windows/uwp/get-started/">Erstellen eines UWP-Spiels in MonoGame-2D</a></td>
    </tr>
</table>

### <a name="developer-forums"></a>Entwicklerforen

Entwickler Foren sind ein hervorragend für die Frage und Beantwortung von Fragen zur Spielentwicklung, und Sie können sich mit der Spiel Entwicklungs Community in Verbindung setzen. Darüber hinaus halten Foren häufig Lösungen für komplizierte Probleme bereit, die Entwickler bereits bewältigt haben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Veröffentlichen von apps und spielen Entwickler Foren</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsstore%2Cwpsubmit%2Caiaads%2Caiasdk%2Caiamgr">Veröffentlichen und anzeigen in apps</a></td>
    </tr>
    <tr>
        <td>Entwicklerforum für UWP-Apps</td>
        <td><a href="/answers/topics/uwp.html">Entwicklung von UWP-Apps (Apps für die Universelle Windows-Plattform)</a></td>
    </tr>
    <tr>
        <td>Entwicklerforen für Desktopanwendungen</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsgeneraldevelopmentissues">Foren für Windows-Desktopanwendungen</a></td>
    </tr>
    <tr>
        <td>DirectX Microsoft Store Games (Archivierte Forumsbeiträge)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=wingameswithdirectx">Entwickeln von Microsoft Store spielen mit DirectX (archiviert)</a></td>
    </tr>
    <tr>
        <td>Windows 10-Entwicklerforen für verwaltete Partner</td>
        <td><a href="https://forums.xboxlive.com/users/login.html">Xbox-Entwicklerforen: Windows 10</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Forum</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=xboxlivedev">Xbox Live Development-Forum</a></td>
    </tr>
    <tr>
        <td>Playfab-Foren</td>
        <td><a href="https://community.playfab.com/index.html">Playfab-Foren</a></td>
    </tr>
</table>

### <a name="developer-blogs"></a>Entwicklerblogs

Entwicklerblogs sind eine weitere praktische Ressource für topaktuelle Informationen zur Spieleentwicklung. Hier finden Sie Beiträge zu neuen Features, Implementierungsdetails, bewährte Methoden, Hintergrundinformationen zur Architektur und vieles mehr.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog "Building Apps for Windows"</td>
        <td><a href="https://blogs.windows.com/buildingapps/">Building Apps for Windows</a></td>
    </tr>
    <tr>
        <td>Windows 10 (Blogbeiträge)</td>
        <td><a href="https://blogs.windows.com/blog/tag/windows-10/">Beiträge in Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog des Visual Studio-Entwicklerteams</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Der Visual Studio-Blog</a></td>
    </tr>
    <tr>
        <td>Blogs zu Visual Studio-Entwicklertools</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Blogs zu Entwicklertools</a></td>
    </tr>
    <tr>
        <td>Somasegars Blog zu Entwicklertools</td>
        <td><a href="https://devblogs.microsoft.com/somasegar/">Blog von Somasegar</a></td>
    </tr>
    <tr>
        <td>DirectX-Entwicklerblog</td>
        <td><a href="https://devblogs.microsoft.com/directx/">DirectX-Entwickler Blog</a></td>
    </tr>
    <tr>
        <td>Einführung in DirectX 12 (Blogbeitrag)</td>
        <td><a href="https://devblogs.microsoft.com/directx/directx-12/">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Teamblog zu Visual C++-Tools</td>
        <td><a href="https://devblogs.microsoft.com/cppblog/">Blog des Visual C++-Teams</a></td>
    </tr>
    <tr>
        <td>Pix-Teamblog</td>
        <td><a href="https://devblogs.microsoft.com/pix/">Leistungsoptimierung und-Debuggen für DirectX 12-Spiele unter Windows und Xbox</a></td>
    </tr>
    <tr>
        <td>Blog des universellen Windows-App-Bereitstellungs Teams</td>
        <td><a href="/windows/msix/">Erstellen und Bereitstellen von UWP-apps-Teamblog</a></td>
    </tr>
</table>

## <a name="concept-and-planning"></a>Konzept und Planung

In der Konzeptionierungs- und Planungsphase entscheiden Sie, welches Spiel Sie entwickeln möchten und mit welchen Tools und Technologien Sie es zum Leben erwecken.

### <a name="overview-of-game-development-technologies"></a>Übersicht über Technologien für die Spieleentwicklung

Zu Beginn der Entwicklung eines UWP-Spiels haben Sie die Wahl zwischen verschiedenen Optionen für Grafik, Eingabe, Audio, Netzwerk, Hilfsprogramme und Bibliotheken.

Vielleicht haben Sie ja bereits entschieden, welche Technologien Sie in Ihrem Spiel verwenden möchten. Andernfalls finden Sie im Handbuch [Spieletechnologien für UWP-Apps](game-development-platform-guide.md) eine hervorragende Übersicht über viele der verfügbaren Technologien. Es wird nachdrücklich empfohlen, dieses Handbuch zu lesen, um mehr über die Optionen und ihre Kombinationsmöglichkeiten zu erfahren.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Überblick über UWP-Spieletechnologien</td>
        <td><a href="game-development-platform-guide.md">Spieletechnologien für UWP-Apps</a></td>
    </tr>
</table>

Diese drei GDC 2015-Videos vermitteln einen guten Überblick über die Entwicklung von Windows 10-Spielen und das Spielerlebnis unter Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Entwicklung von Windows 10-Spielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Entwickeln von Spielen für Windows 10</a></td>
    </tr>
    <tr>
        <td>Spielerlebnis unter Windows 10 (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Spiele-Consumer-Benutzeroberflächen unter Windows 10</a></td>
    </tr>
    <tr>
        <td>Übergreifendes Spielen im gesamten Microsoft-Ökosystem (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">Die Zukunft des Spielens im gesamten Microsoft-Ökosystem</a></td>
    </tr>
</table>

### <a name="game-planning"></a>Planen von Spielen

Im Folgenden finden Sie einige Konzept- und Planungsthemen, die Ihnen einen Überblick über das geben, was Sie bei der Planung Ihres Spiels berücksichtigen sollten.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Machen Sie Ihr Spiel zugänglich</td>
        <td><a href="/windows/uwp/gaming/accessibility-for-games">Barrierefreiheit von Spielen</a></td>
    </tr>
    <tr>
        <td>Spiele mithilfe der Cloud erstellen</td>
        <td><a href="/windows/uwp/gaming/cloud-for-games">Cloud für Spiele</a></td>
    </tr>
    <tr>
        <td>Verdienen Sie Ihr Spiel</td>
        <td><a href="/windows/uwp/gaming/monetization-for-games">Monetarisierung für Spiele</a></td>
    </tr>
</table>

### <a name="choosing-your-graphics-technology-and-programming-language"></a>Auswählen von Grafiktechnologie und Programmiersprache

Für die Verwendung in Windows 10-Spielen sind verschiedene Programmiersprachen und Grafiktechnologien verfügbar. Der von Ihnen verwendete Pfad hängt von der Art des Spiels, das Sie entwickeln, von der Benutzer Funktionalität und den Einstellungen Ihres Entwicklungsstudio und von speziellen Featureanforderungen Ihres Spiels ab. Verwenden Sie C#, C++ oder JavaScript? DirectX, XAML oder HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX ist die richtige Wahl für 2D/3D-Grafiken und -Multimediaelemente.

DirectX 12 ist schneller und effizienter als jede vorherige Version. Direct3D 12 ermöglicht umfangreichere Szenen, mehr Objekte, komplexere Effekte und eine vollständige Nutzung moderner GPU-Hardware auf Windows 10-PCs und Xbox One.

Wenn Sie die vertraute Grafik Pipeline Direct3D 11 verwenden möchten, profitieren Sie weiterhin von den neuen Rendering-und Optimierungs Features, die zu Direct3D 11,3 hinzugefügt werden. Und wenn Sie ein bewährter Windows-API-Entwickler mit Stamm in Win32 sind, haben Sie diese Option in Windows 10.

Die umfassenden Features und die umfassende Plattformintegration von DirectX sorgen für eine Leistung und Performance, die auch für die anspruchsvollsten Spiele ausreicht.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX für UWP-Entwicklung</td>
        <td><a href="directx-programming.md">DirectX-Programmierung</a></td>
    </tr>
    <tr>
        <td>Tutorial: Erstellen eines UWP DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung und -referenz</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12-Grafiken</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX 12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Informationen zu Microsoft DirectX 12 und Grafiken</a></td>
    </tr>
</table>

#### <a name="xaml"></a>XAML

XAML ist eine benutzerfreundliche deklarative UI-Sprache mit nützlichen Features wie Animationen, Storyboards, Datenbindung, skalierbaren vektorbasierten Grafiken, dynamischer Größenänderung und Szenendiagrammen. XAML eignet sich gut für Benutzeroberflächen, Menüs, Sprites und 2D-Grafiken von Spielen. Zur Vereinfachung der UI-Layouterstellung ist XAML mit Entwurfs- und Entwicklungstools wie Expression Blend und Microsoft Visual Studio kompatibel. XAML wird häufig mit c# verwendet, aber C++ ist auch eine gute Wahl, wenn dies Ihre bevorzugte Sprache ist oder wenn Ihr Spiel hohe CPU-Anforderungen hat.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML-Plattformübersicht</td>
        <td><a href="/windows/uwp/xaml-platform/index">XAML-Plattform</a></td>
    </tr>
    <tr>
        <td>XAML-UI und -Steuerelemente</td>
        <td><a href="/windows/uwp/design/basics/">Steuerelemente, Layouts und Text</a></td>
    </tr>
</table>

#### <a name="html-5"></a>HTML5

Die HyperText Markup Language (HTML) ist eine häufig verwendete Markup-Sprache für Benutzeroberflächen, die für Webseiten, Apps und Rich Clients eingesetzt wird. Für Windows-Spiele kann HTML5 als Darstellungsschicht mit vollem Funktionsumfang genutzt werden. Dabei stehen die vertrauten Features von HTML, Zugriff auf die universelle Windows-Plattform und Unterstützung für moderne Webfeatures wie AppCache, Web-Worker, Canvas, Drag & Drop, asynchrone Programmierung und SVG zur Verfügung. Im Hintergrund wird für das HTML-Rendering die leistungsstarke DirectX-Hardwarebeschleunigung genutzt, sodass Sie weiterhin in den Genuss der Leistungsvorteile von DirectX kommen, ohne zusätzlichen Code schreiben zu müssen. HTML5 ist eine gute Wahl, wenn Sie sich mit der Webentwicklung auskennen, ein Webspiel portieren oder Sprach- und Grafikebenen nutzen möchten, die unter Umständen leichter zugänglich als andere Optionen sind. HTML5 wird zusammen mit JavaScript verwendet, kann aber auch mit Komponenten verknüpft werden, die mit C# oder C++/CX erstellt wurden.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informationen zu HTML5 und zum Dokumentobjektmodell</td>
        <td><a href="https://developer.mozilla.org/docs/Web">HTML- und DOM-Referenz</a></td>
    </tr>
    <tr>
        <td>Die HTML5-Empfehlung des W3C</td>
        <td><a href="https://www.w3.org/TR/html5/">HTML5</a></td>
    </tr>
</table>

#### <a name="combining-presentation-technologies"></a>Kombinieren von Darstellungstechnologien

Die Microsoft DirectX Graphic Infrastructure (DXGI) bietet Interoperabilität und Kompatibilität über mehrere Arten von Grafiktechnologie hinweg. Für Hochleistungsgrafiken können Sie XAML und DirectX kombinieren, indem Sie XAML für Menüs und andere einfache UI-Elemente und DirectX für das Rendern von komplexen 2D- und 3D-Szenen nutzen. DXGI bietet auch Kompatibilität zwischen Direct2D, Direct3D, DirectWrite, DirectCompute und der Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programmieranleitung und Referenz für die DirectX Graphic Infrastructure</td>
        <td><a href="/windows/desktop/direct3ddxgi/dx-graphics-dxgi">DXGI</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilität von DirectX und XAML</a></td>
    </tr>
</table>

#### <a name="c"></a>C++

C++/CX ist eine Sprache mit hoher Leistung und geringerem Mehraufwand, die eine starke Kombination aus Geschwindigkeit, Kompatibilität und Plattformzugriff bietet. C++/CX erleichtert Ihnen die Nutzung aller nützlichen Gaming-Features unter Windows 10, z. B. DirectX und Xbox Live. Außerdem können Sie vorhandenen C++-Code und die dazugehörigen Bibliotheken verwenden. C++/CX erstellt schnellen, systemeigenen Code, der nicht den mehr Aufwand Garbage Collection verursacht, sodass Ihr Spiel eine hohe Leistung und einen niedrigen Stromverbrauch aufweisen kann, was zu einer längeren Akku Lebensdauer führt. Verwenden Sie C++/CX zusammen mit DirectX oder XAML, oder erstellen Sie ein Spiel, in dem eine Kombination daraus genutzt wird.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referenz und Übersichten für C++/CX</td>
        <td><a href="/cpp/cppcx/visual-c-language-reference-c-cx">Visual C++-Sprachreferenz (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++-Programmieranleitung und -Referenz</td>
        <td><a href="/cpp/visual-cpp-in-visual-studio">Visual C++ in Visual Studio 2019</a></td>
    </tr>
</table>

#### <a name="c"></a>C#

C# (sprich: „C sharp“) ist eine moderne, innovative Sprache, die einfach, leistungsstark, typsicher und objektorientiert ist. C# ermöglicht eine schnelle Entwicklung, während gleichzeitig die Vertrautheit und Ausdruckskraft von Sprachen im C-Stil gewahrt bleibt. Obwohl C# einfach zu verwenden ist, verfügt die Sprache über viele moderne Sprachfeatures wie Polymorphie, Delegate, Lambda-Elemente, Abschlüsse, Iteratormethoden, Kovarianz und LINQ-Ausdrücke (Language-Integrated Query). C# ist eine ausgezeichnete Wahl, wenn Sie XAML verwenden möchten, schnell mit der Entwicklung Ihres Spiels beginnen möchten oder bereits über C#-Erfahrung verfügen. C# wird vorrangig mit XAML genutzt. Falls Sie DirectX einsetzen möchten, sollten Sie stattdessen besser C++ wählen oder einen Teil des Spiels als C++-Komponente schreiben, die mit DirectX interagieren kann. Eine weitere Alternative wäre [Win2D](https://github.com/Microsoft/Win2D) – eine Direct2D-Grafikbibliothek im unmittelbaren Modus für C# und C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C#-Programmieranleitung und -Referenz</td>
        <td><a href="/dotnet/articles/csharp/csharp">C#-Programmiersprachenreferenz</a></td>
    </tr>
</table>

#### <a name="javascript"></a>JavaScript

JavaScript ist eine dynamische Skriptsprache, die häufig für moderne Webanwendungen und Rich-Clientanwendungen eingesetzt wird.

Bei Windows-JavaScript-Apps kann auf einfache und intuitive Weise auf die leistungsfähigen Features der universellen Windows-Plattform zugegriffen werden – in Form von Methoden und Eigenschaften objektorientierter JavaScript-Klassen. JavaScript ist eine gute Wahl für Ihr Spiel, wenn Sie von einer Webentwicklungs Umgebung aus arbeiten, bereits mit JavaScript vertraut sind oder HTML5-, CSS-, winjs-oder JavaScript-Bibliotheken verwenden möchten. Wenn Sie DirectX oder XAML als Ziel verwenden, wählen Sie stattdessen c# oder C++/CX aus.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referenz zu JavaScript und Windows-Runtime</td>
        <td><a href="/scripting/javascript/javascript-language-reference">JavaScript-Referenz</a></td>
    </tr>
</table>

#### <a name="use-windows-runtime-components-to-combine-languages"></a>Verwenden von Windows-Runtime-Komponenten zum Kombinieren von Sprachen

Mit dem universelle Windows-Plattform können in verschiedenen Sprachen geschriebene Komponenten leicht kombiniert werden. Erstellen Sie Windows-Runtime Komponenten in C++, c# oder Visual Basic, und rufen Sie Sie dann über JavaScript, c#, C++ oder Visual Basic auf. Dies ist eine hervorragende Möglichkeit, wenn Sie Teile des Spiels in der Sprache Ihrer Wahl programmieren möchten. Mithilfe von Komponenten können Sie auch externe Bibliotheken nutzen, die nur in einer bestimmten Sprache verfügbar sind, und Legacy Code verwenden, den Sie bereits geschrieben haben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Erstellen von Windows-Runtime-Komponenten</td>
        <td><a href="/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Komponenten für Windows-Runtime mit C++/CX</a></td>
    </tr>
</table>

### <a name="which-version-of-directx-should-your-game-use"></a>Welche DirectX-Version sollte Ihr Spiel verwenden?

Wenn Sie ein Spiel mit DirectX entwickeln, müssen Sie sich zwischen Microsoft Direct3D 12 und Microsoft Direct3D 11 entscheiden.

DirectX 12 ist schneller und effizienter als jede vorherige Version. Direct3D 12 ermöglicht umfangreichere Szenen, mehr Objekte, komplexere Effekte und eine vollständige Nutzung moderner GPU-Hardware auf Windows 10-PCs und Xbox One. Da Direct3D 12 auf einer sehr niedrigen Ebene ausgeführt wird, erhält ein erfahrenes Grafikentwicklungs- oder DirectX 11-Entwicklungsteam alle notwendigen Steuerungsmöglichkeiten für die Maximierung der Grafikoptimierung.

Direct3D 11.3 ist eine Grafik-API auf einem niedrigen Niveau, die das vertraute Direct3D-Programmiermodell verwendet und Ihnen einen größeren Teil der Komplexität abnimmt, die mit dem GPU-Rendering verbunden ist. Sie wird auch von Windows 10 und Xbox One unterstützt. Wenn Sie über ein vorhandenes Modul verfügen, das in Direct3D 11 geschrieben wurde, und noch nicht bereit sind, zu Direct3D 12 zu wechseln, können Sie Direct3D 11 auf 12 verwenden, um einige Leistungsverbesserungen zu erzielen. Die Versionen ab 11.3 enthalten die neuen Rendering- und Optimierungsfeatures, die auch in Direct3D 12 zur Verfügung stehen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entscheidung zwischen Direct3D 12 und Direct3D 11</td>
        <td><a href="/windows/desktop/direct3d12/what-is-directx-12-">Was ist Direct3D 12?</a></td>
    </tr>
    <tr>
        <td>Übersicht über Direct3D 11</td>
        <td><a href="/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11">Direct3D 11-Grafik</a></td>
    </tr>
    <tr>
        <td>Übersicht über „Direct3D 11 on 12“</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-11-on-12">Direct3D 11 on 12</a></td>
    </tr>
</table>

### <a name="bridges-game-engines-and-middleware"></a>Brücken, Spielengines und Middleware

Mithilfe von Brücken, Spielengines und Middleware können Sie je nach Spiel unter Umständen die Entwicklung und das Testing beschleunigen und den damit verbundenen Ressourcenaufwand verringern. Hier finden Sie einige Übersicht und Ressourcen für Bridges, Spiele-Engines und Middleware.

#### <a name="universal-windows-platform-bridges"></a>Brücken für die universelle Windows-Plattform

Bei Brücken für die universelle Windows-Plattform handelt es sich um Technologien für die UWP-Portierung Ihrer vorhandenen Apps oder Spiele. Brücken eignen sich sehr gut für den schnellen Einstieg in die Entwicklung von UWP-Spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP Brücken</td>
        <td><a href="https://developer.microsoft.com/windows/bridges">Portieren Ihres Codes für Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für iOS</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/ios">Portieren Ihrer iOS-Apps für Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für Desktop-Anwendungen (.NET und Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Konvertieren Ihrer Desktopanwendung in eine UWP-App</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

PlayFab ist jetzt Bestandteil der Microsoft-Familie und eine vollständige Back-End-Plattform für Live-Spiele und ein leistungsstarkes Mittel für die ersten Schritte unabhängiger Studios. Steigern Sie Umsatz, Engagement und Bindung – bei gleichzeitigen Kosteneinsparungen – mit Spieldiensten, Echtzeitanalysen und LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Übersicht über Tools und Dienste</a></td>
    </tr>
    <tr>
        <td>Erste Schritte</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Leitfaden zu den allgemeinen ersten Schritten</a></td>
    </tr>
    <tr>
        <td>Videotutorialreihe</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Eine Reihe von Demovideos zu den wichtigsten Systemen von playfab</a></td>
    </tr>
    <tr>
        <td>Rezepte</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Beliebte Spielmechanismen und Entwurfsmuster Beispiele</a></td>
    </tr>
    <tr>
        <td>Plattformen</td>
        <td><a href="https://api.playfab.com/platforms">Spezifische Dokumentation für verschiedene Plattformen und Spiel-Engines</a></td>
    </tr>
    <tr>
        <td>GitHub-Repository</td>
        <td><a href="https://github.com/PlayFab">Hier finden Sie Skripts und sdkchen für verschiedene Plattformen, einschließlich Android, Ios, Windows, Unity und Unreal.</a></td>
    </tr>
    <tr>
        <td>API-Dokumentation</td>
        <td><a href="https://api.playfab.com/documentation/">Greifen Sie direkt über Rest-ähnliche Web-APIs auf playfab-Dienst zu</a></td>
    </tr>
    <tr>
        <td>Foren</td>
        <td><a href="https://community.playfab.com/index.html">Playfab-Foren</a></td>
    </tr>
</table>

#### <a name="unity"></a>Unity

Unity bietet eine Plattform zum Erstellen von schönen und ansprechenden 2D-, 3D-, VR-und AR-spielen und-apps. Damit können Sie Ihre kreative Vision schnell umsetzen und ihre Inhalte praktisch an Medien oder Geräte übermittelt.

Ab Unity 5,4 unterstützt Unity die Direct3D 12-Entwicklung.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Die Unity-Spielengine</td>
        <td><a href="https://unity.com/">Unity – Spielengine</a></td>
    </tr>
    <tr>
        <td>Unity herunterladen</td>
        <td><a href="https://store.unity.com/">Unity herunterladen</a></td>
    </tr>
    <tr>
        <td>Unity-Dokumentation für Windows</td>
        <td><a href="https://docs.unity3d.com/Manual/Windows.html">Unity-Handbuch/Windows</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von LiveOps mithilfe von playfab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Erste Schritte: Erstellen Ihres ersten playfab-API-Aufrufes aus Ihrem Unity-Spiel</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Interaktivität zu Ihrem Spiel mithilfe von Mixer Interactive</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Leitfaden zu den ersten Schritten</a></td>
    </tr>
    <tr>
        <td>Mixsdk für Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Unity-Plug-in</a></td>
    </tr>
    <tr>
        <td>Referenz Dokumentation für das Mixer-SDK für Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">API-Referenz für Mixer Unity-Plug-in</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen Sie Ihr Unity-Spiel für Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Portierungsleitfaden</a></td>
    </tr>
    <tr>
        <td>Problembehandlung fehlender Assemblyverweise für .NET-APIs</td>
        <td><a href="/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Fehlende .NET-APIs in Unity und UWP</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen des Unity-Spiels als UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">So veröffentlichen Sie Ihr Unity-Spiel als UWP-App</a></td>
    </tr>
    <tr>
        <td>Verwenden von Unity zum Erstellen von Windows-Spielen und -Apps (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Erstellen von Windows-Spielen und -Apps mit Unity</a></td>
    </tr>
    <tr>
        <td>Unity-Spielentwicklung mit Visual Studio (Videoserie)</td>
        <td><a href="https://www.youtube.com/playlist?list=PLReL099Y5nRfseAg0k1SJOlpqdcsDs8Em">Verwendung von Unity mit Visual Studio 2015</a></td>
    </tr>
</table>

#### <a name="havok"></a>Havok

Die modulare Suite von Tools und Technologien von Havok hilft Spiel Entwicklern dabei, neue Ebenen der Interaktivität zu erreichen und Sie zu unterstützen. Havok bietet äußerst realistische Physik, interaktive Simulationen und beeindruckende Effekte. Version 2015,1 und höher unterstützt offiziell UWP in Visual Studio 2015 auf x86, 64-Bit und Arm.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok-Website</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Havok-Toolsuite</td>
        <td><a href="https://www.havok.com/products/">Havok-Produktübersicht</a></td>
    </tr>
    <tr>
        <td>Havok-Supportforen</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
</table>

#### <a name="monogame"></a>MonoGame

MonoGame ist ein plattformübergreifendes Open-Source-Framework für die Spieleentwicklung, das ursprünglich auf XNA Framework 4.0 von Microsoft basierte. MonoGame unterstützt derzeit Windows, Windows Phone und Xbox sowie Linux, macOS, iOS, Android und verschiedene andere Plattformen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="https://www.monogame.net">MonoGame-Website</a></td>
    </tr>
    <tr>
        <td>MonoGame-Dokumentation</td>
        <td><a href="https://www.monogame.net/documentation/">MonoGame-Dokumentation (aktuell)</a></td>
    </tr>
    <tr>
        <td>MonoGame-Downloads</td>
        <td><a href="https://www.monogame.net/downloads/">Laden Sie Versionen, Entwicklungsbuilds und Quellcode</a> von der MonoGame-Website herunter, oder <a href="https://www.nuget.org/profiles/MonoGame">rufen Sie die neueste Version über NuGet ab</a>.
    </tr>
    <tr>
        <td>Monogame 2D UWP-Spielbeispiel</td>
        <td><a href="/windows/uwp/get-started/">Erstellen eines UWP-Spiels in MonoGame-2D</a></td>
    </tr>
</table>

#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x ist eine plattformübergreifende Open-Source-Spiel Entwicklungs-Engine und Tools Suite, die das Entwickeln von UWP-spielen unterstützt. Ab Version 3 werden auch 3D-Features hinzugefügt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-X</td>
        <td><a href="https://www.cocos2d-x.org/">Was ist Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X-Programmieranleitung</td>
        <td><a href="https://www.cocos2d-x.org/programmersguide/">Leitfaden zu Cocos2d-x-Programmierern</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X unter Windows 10 (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Ausführen von Cocos2d-X unter Windows 10</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von LiveOps mithilfe von playfab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Erste Schritte: Erstellen Ihres ersten playfab-API-Aufrufes aus Ihrem Cocos2d-Spiel</a></td>
    </tr>
</table>

#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 ist eine komplette Suite mit Tools für die Spieleentwicklung und für alle Arten von Spielen und Entwicklern geeignet. Die Unreal Engine wird von Spieleentwicklern auf der ganzen Welt für besonders anspruchsvolle Konsolen- und PC-Spiele eingesetzt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von LiveOps mithilfe von playfab-C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Erste Schritte: Erstellen Ihres ersten playfab-API-Aufrufes aus Ihrem Unreal Game</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von LiveOps mithilfe von playfab-Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Erste Schritte: Erstellen Ihres ersten playfab-API-Aufrufes aus Ihrem Unreal Game</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

"Babylonjs" ist ein umfassendes JavaScript-Framework zum Entwickeln von 3D-Spielen mit HTML5, WebGL, webvr und Web-Audiodaten.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="https://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL-3D mit HTML5 und BabylonJS (Videoserie)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Learning WebGL 3D- und BabylonJS</a></td>
    </tr>
    <tr>
        <td>Erstellen eines plattformübergreifenden WebGL-Spiels mit BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Verwenden von BabylonJS zur Entwicklung eines plattformübergreifenden Spiels</a></td>
    </tr>
</table>

### <a name="porting-your-game"></a>Portieren Ihres Spiels

Entwicklern, die bereits über ein Spiel verfügen, stehen zahlreiche Ressourcen und Handbücher für eine schnelle UWP-Portierung ihres Spiels zur Verfügung. Als Starthilfe bei der Portierung empfiehlt sich unter Umständen die Verwendung einer [Brücke für die universelle Windows-Plattform](#universal-windows-platform-bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portieren einer Windows 8-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="/windows/uwp/porting/w8x-to-uwp-root">Wechseln von Windows-Runtime 8. x zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Windows 8-App zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Portieren von Windows 8.1-Apps zu Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren einer iOS-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="/windows/uwp/porting/ios-to-uwp-root">Wechsel von iOS zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Silverlight-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="/windows/uwp/porting/wpsl-to-uwp-root">Wechseln von Windows Phone Silverlight zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren von XAML oder Silverlight zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Portieren einer App von XAML oder Silverlight zu Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren eines Xbox-Spiels zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Portieren von Xbox One zu Windows 10 (UWP)</a></td>
    </tr>
    <tr>
        <td>Portieren von DirectX 9 zu DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Portieren von DirectX 9 zur Universellen Windows-Plattform (UWP)</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D 11 zu Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portieren von Direct3D 11 zu Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portieren von OpenGL ES zu Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Portieren von OpenGL ES 2.0 zu Direct3D 11</a></td>
    </tr>
    <tr>
        <td>OpenGL ES 2.0 zu Direct3D 11 mit ANGLE</td>
        <td><a href="https://github.com/microsoft/angle/wiki">Ultra</a></td>
    </tr>
    <tr>
        <td>Entsprechungen für die klassische Windows-API in der UWP</td>
        <td><a href="/uwp/win32-and-com/win32-and-com-for-uwp-apps">Alternativen zu Windows-APIs in Apps für die Universelle Windows-Plattform (UWP)</a></td>
    </tr>
</table>

## <a name="prototype-and-design"></a>Prototyp und Design

Nachdem Sie sich entschieden haben, welche Art von Spiel Sie entwickeln und welche Tools und Grafiktechnologie Sie dabei verwenden möchten, können Sie sich der Gestaltung zuwenden und einen Prototyp entwickeln. Da es sich bei Ihrem Spiel im Grunde um eine UWP-App (Universelle Windows-Plattform) handelt, beginnen Sie dort.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Einführung in die universelle Windows-Plattform (UWP)

Windows 10 führt die universelle Windows-Plattform (UWP) ein. Diese stellt eine gemeinsame, übergreifende API-Plattform für Windows 10-Geräte bereit. Bei der UWP handelt es sich um eine Weiterentwicklung und Erweiterung des Windows-Runtime-Modells zu einem geschlossenen, einheitlichen Kern. Für die UWP entwickelte Spiele können WinRT-APIs aufrufen, die bei allen Geräten vorhanden sind. Da die UWP garantierte API-Ebenen bereitstellt, können Sie ein einzelnes App-Paket erstellen, das auf Windows 10-Geräten installiert wird. Bei Bedarf kann Ihr Spiel natürlich auch weiterhin spezifische APIs der Geräte aufrufen, auf denen das Spiel ausgeführt wird – etwa einige klassische Windows-APIs von Win32 und .NET.

Im Anschluss finden Sie praktische Handbücher, die sich ausführlich mit UWP-Apps (Universelle Windows-Plattform) auseinandersetzen und hilfreiche Erkenntnisse zur Plattform liefern.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einführung in UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="/windows/uwp/get-started/whats-a-uwp">Was ist eine App der universellen Windows-Plattform?</a></td>
    </tr>
    <tr>
        <td>Übersicht über die UWP</td>
        <td><a href="/windows/uwp/get-started/universal-application-platform-guide">Anleitung für UWP-Apps</a></td>
    </tr>
</table>

### <a name="getting-started-with-uwp-development"></a>Erste Schritte bei der UWP-Entwicklung

Die Vorbereitung auf die Entwicklung einer UWP-App (Universelle Windows-Plattform) ist ganz einfach und im Handumdrehen erledigt. Die erforderlichen Schritte werden in den folgenden Handbüchern erläutert:

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Erste Schritte bei der UWP-Entwicklung</td>
        <td><a href="https://developer.microsoft.com/windows/apps/getstarted">Erste Schritte mit Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Vorbereitung auf die UWP-Entwicklung</td>
        <td><a href="/windows/uwp/get-started/get-set-up">Vorbereiten</a></td>
    </tr>
</table>

Wenn Sie noch keine Erfahrungen mit der UWP-Programmierung haben und die Verwendung von XAML in Ihrem Spiel in Betracht ziehen (siehe [Auswählen von Grafiktechnologie und Programmiersprache](#choosing-your-graphics-technology-and-programming-language)), ist die Videoserie [Windows 10-Entwicklung für Neueinsteiger](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) ein guter Ausgangspunkt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einsteigerhandbuch für die Windows 10-Entwicklung mit XAML (Videoserie)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Windows 10-Entwicklung für absolute Anfänger</a></td>
    </tr>
    <tr>
        <td>Ankündigung der Windows 10-Neueinsteigerserie mit XAML (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Windows 10-Entwicklung für absolute Anfänger</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>UWP-Entwicklungskonzepte

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Entwicklung von UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Windows-Apps entwickeln</a></td>
    </tr>
    <tr>
        <td>Übersicht über die Netzwerkprogrammierung der UWP</td>
        <td><a href="/windows/uwp/networking/index">Netzwerk und Webdienste</a></td>
    </tr>
    <tr>
        <td>Verwenden von „Windows.Web.HTTP“ und „Windows.Networking.Sockets“ in Spielen</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Netzwerk für Spiele</a></td>
    </tr>
    <tr>
        <td>Asynchrone Programmierkonzepte der UWP</td>
        <td><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Asynchrone Programmierung</a></td>
    </tr>
</table>

### <a name="windows-desktop-apis-to-uwp"></a>Windows-Desktop-APIs für UWP

Dies sind einige Links, mit denen Sie Ihr Windows-Desktop Spiel auf UWP umstellen können.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Verwenden von vorhandenem C++-Code für die UWP-Spielentwicklung</td>
        <td><a href="/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Gewusst wie: Verwenden von vorhandenem C++-Code in einer UWP-App</a></td>
    </tr>
    <tr>
        <td>Windows-Runtime-APIs für Win32-und com-APIs</td>
        <td><a href="/uwp/win32-and-com/win32-and-com-for-uwp-apps">Win32- und COM-APIs für UWP-Apps</a></td>
    </tr>
    <tr>
        <td>Nicht unterstützte CRT-Funktionen in UWP</td>
        <td><a href="/cpp/cppcx/crt-functions-not-supported-in-universal-windows-platform-apps">In Apps für die universelle Windows-Plattform nicht unterstützte CRT-Funktionen</a></td>
    </tr>
    <tr>
        <td>Alternativen für Windows-APIs</td>
        <td><a href="/uwp/win32-and-com/alternatives-to-windows-apis-uwp">Alternativen zu Windows-APIs in Apps für die Universelle Windows-Plattform (UWP)</a></td>
    </tr>
</table>

### <a name="process-lifetime-management"></a>Prozesslebensdauer-Verwaltung

Prozesslebensdauer-Verwaltung (oder App-Lebenszyklus) beschreibt die verschiedenen Aktivierungszustände, die eine UWP-App (Universelle Windows-Plattform) durchlaufen kann. Ihr Spiel kann aktiviert, angehalten, fortgesetzt oder beendet werden und diese Zustände auf unterschiedliche Arten durchlaufen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Behandeln von App-Lebenszyklusübergängen</td>
        <td><a href="/windows/uwp/launch-resume/app-lifecycle">App-Lebenszyklus</a></td>
    </tr>
    <tr>
        <td>Auslösen von App-Übergängen mithilfe von Microsoft Visual Studio</td>
        <td><a href="/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio">Vorgehensweise beim Auslösung von Suspend-, Resume-und Background-Ereignissen für UWP-apps in Visual Studio</a></td>
    </tr>
</table>

### <a name="designing-game-ux"></a>Gestalten der UX von Spielen

Großartigen Spielen liegt in der Regel ein kreatives Design zugrunde.

Spiele und Apps teilen sich zwar einige Benutzeroberflächenelemente und Designprinzipien, beim Spieldesign werden jedoch häufig ein ganz besonderer Look und ein einzigartiges Spielgefühl angestrebt. Spiele sind erfolgreich, wenn in beiden Bereichen ein durchdachtes Design gewählt wird: Wann sollten Sie in Ihrem Spiel bewährte Benutzeroberflächenelemente verwenden, und wann sollten Sie davon abweichen und innovativ vorgehen? Die Darstellungstechnologie, die Sie für das Spiel auswählen – DirectX, XAML, HTML5 oder eine beliebige Kombination –, wird sich auf die Implementierungsdetails auswirken. Die von Ihnen angewendeten Entwurfsprinzipien sind aber nicht von der jeweiligen Wahl abhängig.

Zusätzlich zum UX-Design müssen Sie sich auch mit dem Gameplay-Design auseinandersetzen, was unter anderem Aspekte wie Leveldesign, Pacing und Umgebungsdesign umfasst. Da es sich hierbei um eine ganz eigene Kunstform handelt, gehen wir in diesem Dokument nicht näher darauf ein, sondern überlassen diesen Bereich ganz Ihnen und Ihrem Team.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP-Gestaltungsgrundlagen und -richtlinien</td>
        <td><a href="https://developer.microsoft.com/windows/apps/design">Gestalten von UWP-Apps</a></td>
    </tr>
    <tr>
        <td>Gestalten für App-Lebenszykluszustände</td>
        <td><a href="/windows/uwp/launch-resume/index">UX-Richtlinien für das Starten, Anhalten und Fortsetzen von Apps</a></td>
    </tr>
    <tr>
        <td>Entwerfen der UWP-App für Xbox One-und TV-Bildschirme</td>
        <td><a href="/windows/uwp/design/devices/designing-for-tv">Entwerfen für Xbox und Fernsehgeräte</a></td>
    </tr>
    <tr>
        <td>Ausrichten auf verschiedene Geräteformfaktoren (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Entwerfen von Spielen für eine Windows Core-Welt</a></td>
    </tr>
</table>

#### <a name="color-guideline-and-palette"></a>Richtlinie und Palette für Farben

Die Befolgung einer einheitlichen Farbrichtlinie für das Spiel sorgt für eine Verbesserung der Ästhetik und der Navigation und ist ein wirksames Mittel, um Spieler über Menü- und HUD-Funktionen zu informieren. Eine einheitliche Farbgestaltung von Spielelementen wie Warnungen, Schäden, Erfahrungspunkten und Erfolgen kann zu einer aufgeräumteren Benutzeroberfläche führen und explizite Bezeichnungen überflüssig machen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Farbhandbuch</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Bewährte Methoden: Farbe</a></td>
    </tr>
</table>

#### <a name="typography"></a>Typografie

Durch den angemessenen Einsatz von Typografie können Sie Ihr Spiel in vielerlei Hinsicht verbessern – etwa in Bezug auf UI-Layout, Navigation, Lesbarkeit, Atmosphäre und Spielerimmersion.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Typografiehandbuch</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_Typography.pdf">Bewährte Methoden: Typografie</a></td>
    </tr>
</table>

#### <a name="ui-map"></a>UI-Zuordnung

Eine UI-Zuordnung ist eine Layoutübersicht der Navigation und Menüs eines Spiels in Form eines Flussdiagramms. Die UI-Zuordnung hilft allen Beteiligten Projekt beteiligten dabei, die Schnittstelle und die Navigationspfade des Spiels zu verstehen, und kann potenzielle Hindernisse bereitstellen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Anleitung für die UI-Zuordnung</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_UI_Map.pdf">Bewährte Methoden: UI-Zuordnung</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Spielton

Anleitungen und Verweise zum Implementieren von Audiodaten in spielen mit XAudio2, xapo und Windows Sonic. XAudio2 ist eine audioapi auf niedriger Ebene, die Signalverarbeitung und Vermischung der Grundlage für die Entwicklung Hochleistungs-audioengines bereitstellt. Die xapo-API ermöglicht die Erstellung von plattformübergreifenden audioverarbeitungsobjekten (xapo) für die Verwendung in XAudio2 sowohl unter Windows als auch in Xbox. Mit der Windows-Unterstützung für die Unterstützung von Sonic können Sie Dolby Atmos für das Heim Theater, Dolby Atmos für Kopfhörer und die Unterstützung von Windows HRTF für Ihre Spiele-oder streamingmedienanwendung

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAudio2-APIs</td>
        <td><a href="/windows/desktop/xaudio2/xaudio2-apis-portal">Programmier Handbuch und API-Referenz für XAudio2</a></td>
    </tr>
    <tr>
        <td>Erstellen von plattformübergreifenden audioverarbeitungsobjekten</td>
        <td><a href="/windows/desktop/xaudio2/xapo-overview">Übersicht über xapo</a></td>
    </tr>
    <tr>
        <td>Einführung in audiokonzepte</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio für Spiele</a></td>
    </tr>
    <tr>
        <td>Übersicht über Windows-Sound</td>
        <td><a href="/windows/desktop/CoreAudio/spatial-sound">Raumklang</a></td>
    </tr>
    <tr>
        <td>Beispiele für räumliche Klangbeispiele in Windows</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Audiobeispiele für Xbox Advanced Technology Group</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Windows Sonic in ihre Spiele integrieren (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Einführung in räumliche Audiofunktionen für Xbox und Windows</a></td>
    </tr>
</table>

### <a name="directx-development"></a>DirectX-Entwicklung

Anleitungen und Referenzen für die Entwicklung von DirectX-Spielen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX für UWP-Entwicklung</td>
        <td><a href="directx-programming.md">DirectX-Programmierung</a></td>
    </tr>
    <tr>
        <td>Tutorial: Erstellen eines UWP DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>DirectX-Interaktionen mit dem UWP-App-Modell</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Das App-Objekt und DirectX</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX 12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Informationen zu Microsoft DirectX 12 und Grafiken</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung und -referenz</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12-Grafiken</a></td>
    </tr>
    <tr>
        <td>DirectX 12-Grundlagen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Bessere Leistung und Performance: Ihr Spiel unter DirectX 12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Erlernen von Direct3D 2

Erfahren Sie mehr über die Änderungen in Direct3D 12 und wie Sie mit der Programmierung in Direct3D 12 beginnen können.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einrichten der Programmierumgebung</td>
        <td><a href="/windows/desktop/direct3d12/directx-12-programming-environment-set-up">Einrichtung der Direct3D 12 Programmierungsumgebung</a></td>
    </tr>
    <tr>
        <td>Erstellen einer Grundkomponente</td>
        <td><a href="/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Erstellen einer einfachen Direct3D 12-Komponente</a></td>
    </tr>
    <tr>
        <td>Änderungen in Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/important-changes-from-directx-11-to-directx-12">Wichtige Änderungen bei der Migration von Direct3D 11 zu Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D 11 zu Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portieren von Direct3D 11 zu Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Konzepte für die Ressourcenbindung (deckt Deskriptor, Deskriptortabelle, Deskriptorheap und Stammsignatur ab) </td>
        <td><a href="/windows/desktop/direct3d12/resource-binding">Ressourcenbindung in Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Verwalten des Arbeitsspeichers</td>
        <td><a href="/windows/desktop/direct3d12/memory-management">Arbeitsspeicherverwaltung in Direct3D 12</a></td>
    </tr>
</table>

#### <a name="directx-tool-kit-and-libraries"></a>DirectX-Toolkit und -Bibliotheken

Das DirectX-Toolkit, die DirectX-Texturverarbeitungsbibliothek, die DirectXMesh-Geometrieverarbeitungsbibliothek, die UVAtlas-Bibliothek und die DirectXMath-Bibliothek bieten textur-, gitter- und spritebezogene sowie weitere Hilfsprogrammfunktionen und Hilfsklassen für die DirectX-Entwicklung. Diese Bibliotheken können Ihnen helfen, Entwicklungszeit und -aufwand einzusparen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX-Toolkit für DirectX 11 herunterladen</td>
        <td><a href="https://github.com/Microsoft/DirectXTK">DirectXTK</a></td>
    </tr>
    <tr>
        <td>DirectX-Toolkit für DirectX 12 herunterladen</td>
        <td><a href="https://github.com/Microsoft/DirectXTK12">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>DirectX-Texturverarbeitungsbibliothek herunterladen</td>
        <td><a href="https://github.com/Microsoft/DirectXTex">DirectXTex</a></td>
    </tr>
    <tr>
        <td>DirectXMesh-Geometrieverarbeitungsbibliothek herunterladen</td>
        <td><a href="https://github.com/Microsoft/DirectXMesh">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>UVAtlas zum Erstellen und Verpacken des isoChart-Texturatlas herunterladen</td>
        <td><a href="https://github.com/Microsoft/UVAtlas">UVAtlas</a></td>
    </tr>
    <tr>
        <td>DirectXMath-Bibliothek herunterladen</td>
        <td><a href="https://github.com/Microsoft/DirectXMath">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Unterstützung im DirectXTK (Blogbeitrag)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Unterstützung für DirectX 12</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>DirectX-Ressourcen von Partnern

Dies sind einige zusätzliche DirectX-Dokumentationen, die von externen Partnern erstellt wurden.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: Empfohlene und nicht empfohlene Vorgehensweisen für DX12 (Blogbeitrag) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX 12 auf Nvidia-GPUs</a></td>
    </tr>
    <tr>
        <td>Intel: Effizientes Rendering mit DirectX 12</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">DirectX 12-Rendering auf Intel-Grafikkarten</a></td>
    </tr>
    <tr>
        <td>Intel: Mehrfachadapterunterstützung in DirectX 12</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Implementieren einer expliziten Mehrfachadapteranwendung mit DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12-Lernprogramm</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Gemeinsames Whitepaper von Intel, Suzhou Snail und Microsoft</a></td>
    </tr>
</table>

## <a name="production"></a>Produktion

Ihr Studio ist jetzt vollständig eingebunden und beginnt mit dem Produktionszyklus, wobei die Arbeiten auf die einzelnen Teammitglieder aufgeteilt werden. Der Prototyp wird optimiert, überarbeitet und erweitert, um ein vollständiges Spiel zu erhalten.

### <a name="notifications-and-live-tiles"></a>Benachrichtigungen und Live-Kacheln

Ihr Spiel wird im Menü „Start“ durch eine Kachel dargestellt. Über Kacheln und Benachrichtigungen können Sie das Interesse von Spielern wecken, auch wenn diese das Spiel gerade gar nicht spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entwickeln von Kacheln und Signalen</td>
        <td><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">Kacheln, Signale und Benachrichtigungen</a></td>
    </tr>
    <tr>
        <td>Beispiel zur Veranschaulichung von Live-Kacheln und Benachrichtigungen</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Benachrichtigungsbeispiel</a></td>
    </tr>
    <tr>
        <td>Vorlagen für adaptive Kacheln (Blogbeitrag)</td>
        <td><a href="/archive/blogs/tiles_and_toasts/adaptive-tile-templates-schema-and-documentation">Vorlagen für adaptive Kacheln – Schema und Dokumentation</a></td>
    </tr>
    <tr>
        <td>Gestalten von Kacheln und Signalen</td>
        <td><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Richtlinien für Kacheln und Signale</a></td>
    </tr>
    <tr>
        <td>Windows 10-App für die interaktive Entwicklung von Vorlagen für Live-Kacheln</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio</td>
        <td><a href="https://marketplace.visualstudio.com/items?itemName=shenchauhan.UWPTileGenerator">Tool zum Erstellen aller erforderlichen Kacheln über ein einziges Image</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Tipps zum Verwenden des UWP-Tools für die Generierung von Kacheln</a></td>
    </tr>
</table>

### <a name="enable-in-app-product-add-on-purchases"></a>Käufe in-App-Produkten aktivieren (Add-on)

Ein Add-on (in-App-Produkt) ist ein zusätzliches Element, das Spieler im Spiel erwerben können. Add-ons können Spielebenen, Elemente oder etwas anderes sein, das Ihre Spieler möglicherweise genießen. Durch die angemessene Verwendung können Add-ons Umsätze bereitstellen und gleichzeitig das Spielverhalten verbessern. Sie definieren und veröffentlichen die Add-ons Ihres Spiels über Partner Center und ermöglichen in-App-Käufe im Code Ihres Spiels.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Dauerhafte Add-ons</td>
        <td><a href="/windows/uwp/monetize/enable-in-app-product-purchases">Unterstützen des Kaufs von In-App-Produkten</a></td>
    </tr>
    <tr>
        <td>Nutzbare Add-ons</td>
        <td><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Nutzbare in-App-Produktkäufe aktivieren</a></td>
    </tr>
    <tr>
        <td>Add-on-Details und-Übermittlung</td>
        <td><a href="/windows/uwp/publish/iap-submissions">Add-On-Übermittlungen</a></td>
    </tr>
    <tr>
        <td>Überwachen von Add-on-Verkäufen und demografiken für Ihr Spiel</td>
        <td><a href="/windows/uwp/publish/iap-acquisitions-report">Bericht zu Add-On-Käufen</a></td>
    </tr>
</table>

### <a name="debugging-performance-optimization-and-monitoring"></a>Debuggen, Leistungsoptimierung und Überwachung

Um die Leistung zu optimieren, nutzen Sie den Spielmodus in Windows 10, um Ihren Gamern das beste Spiel zu bieten, indem Sie die Kapazität ihrer aktuellen Hardware voll ausschöpfen.

Das Windows Performance Toolkit (WPT) besteht aus Leistungsüberwachungstools, die detaillierte Leistungsprofile von Windows-Betriebssystemen und -Anwendungen erstellen. Dies ist besonders hilfreich für die Überwachung der Speicherverwendung und zum Verbessern der Leistung eines Spiels. Das Windows Performance Toolkit ist im SDK für Windows 10 und im Windows ADK enthalten. Dieses Toolkit besteht aus zwei unabhängigen Tools: Windows Performance Recorder (WPR) und Windows Performance Analyzer (WPA). Procdump, das Teil von [Windows Sysinternals](/sysinternals/)ist, ist ein Befehlszeilen-Hilfsprogramm, das CPU-Spitzen überwacht und Dumpdateien bei Spiel abstürzen generiert.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Leistungstests für Ihren Code</td>
        <td><a href="https://azure.microsoft.com/services/devops/test-plans/">Cloudbasierte Auslastungs Tests</a></td>
    </tr>
    <tr>
        <td>Get Xbox Console Type using Gaming Device Information</td>
        <td><a href="/previous-versions/windows/desktop/gamingdvcinfo/gaming-device-information-portal">Gaming-Geräteinformationen</a></td>
    </tr>
    <tr>
        <td>Verbessern der Leistung durch exklusiven oder priorisierten Zugriff auf Hardware Ressourcen mithilfe von spielmodusapis</td>
        <td><a href="/previous-versions/windows/desktop/gamemode/game-mode-portal">Spielmodus</a></td>
    </tr>
    <tr>
        <td>Abrufen von Windows Performance Toolkit (WPT) aus dem Windows 10 SDK</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Windows 10 SDK</a></td>
    </tr>
    <tr>
        <td>Abrufen von Windows Performance Toolkit (WPT) aus dem Windows ADK</td>
        <td><a href="/windows-hardware/get-started/adk-install">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Problembehandlung bei nicht reagierender Benutzeroberfläche mit Windows Performance Analyzer (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Analyse des kritischen Pfads in WPA</a></td>
    </tr>
    <tr>
        <td>Diagnostizieren von Speicherverwendung und Arbeitsspeicherverlusten mit Windows Performance Recorder (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Speicherbedarf und Arbeitsspeicherverluste</a></td>
    </tr>
    <tr>
        <td>Abrufen von ProcDump</td>
        <td><a href="/sysinternals/downloads/procdump">ProcDump</a></td>
    </tr>
    <tr>
        <td>Informationen zur Verwendung von ProcDump (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Konfigurieren von ProcDump zum Erstellen von Speicherabbilddateien</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>Erweiterte DirectX-Techniken und -Konzepte

Einige Aspekte der DirectX-Entwicklung können sich als differenziert und komplex erweisen. Wenn Sie sich im Rahmen der Produktionsphase mit den Einzelheiten des DirectX-Moduls auseinandersetzen oder komplexe Leistungsprobleme debuggen müssen, können Sie auf die Ressourcen und Informationen in diesem Abschnitt zurückgreifen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PIX on Windows</td>
        <td><a href="https://devblogs.microsoft.com/pix/introducing-pix-on-windows-beta/">Tool zur Leistungsoptimierung und zum Debuggen für DirectX 12 unter Windows</a></td>
    </tr>
    <tr>
        <td>Debuggen und Validierungstools für die D3D12-Entwicklung (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 Leistungsoptimierung und-Debuggen mit Pix-und GPU-Validierung</a></td>
    </tr>
    <tr>
        <td>Grafik- und Leistungsoptimierung (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Erweiterte DirectX 12-Grafiken und -Leistung</a></td>
    </tr>
    <tr>
        <td>Debuggen von DirectX-Grafiken (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Lösen Sie komplexe Grafikprobleme in Ihrem Spiel mithilfe von DirectX-Tools.</a></td>
    </tr>
    <tr>
        <td>Visual Studio 2015-Tools zum Debuggen von DirectX 12 (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">DirectX-Tools für Windows 10 in Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12-Programmier Handbuch</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilität von DirectX und XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Entwicklung von Inhalten mit hohem dynamischen Bereich (HDR)

Build-Spielinhalt, der die vollständigen Farbfunktionen von HDR verwendet.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einführung in HDR und Farbkonzepte (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Beleuchtung von HDR und erweiterter Farbe in DirectX</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie HDR-Inhalte Rendering und ermitteln, ob die aktuelle Anzeige diese unterstützt.</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR-Beispiel</a></td>
    </tr>
    <tr>
        <td>Erstellen und Konfigurieren einer erweiterten Farbe mithilfe von DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Direct2D Advanced Color Image Rendering Sample</a></td>
    </tr>
</table>

### <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung

Entwickeln Sie weltweit einsatzbereite Spiele für die Windows-Plattform, und informieren Sie sich über die in den Top-Produkten von Microsoft integrierten internationalen Features.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vorbereiten Ihres Spiels für den globalen Markt</td>
        <td><a href="/windows/uwp/globalizing/globalizing-portal">Richtlinien für die Entwicklung für eine weltweite Zielgruppe</a></td>
    </tr>
    <tr>
        <td>Überwinden sprachlicher, kultureller und technologischer Barrieren</td>
        <td><a href="https://www.microsoft.com/Language/Default.aspx">Onlineressource für Sprachkonventionen und Microsoft-Standardterminologie</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Übermitteln und Veröffentlichen Ihres Spiels

Die folgenden Handbücher und Informationen sorgen für eine möglichst reibungslose Veröffentlichung und Übermittlung.

### <a name="publishing"></a>Veröffentlichen

Sie verwenden [Partner Center](https://partner.microsoft.com/dashboard) zum Veröffentlichen und Verwalten von Spielpaketen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Partner Center-App-Veröffentlichung</td>
        <td><a href="https://developer.microsoft.com/store/publish-apps">Veröffentlichen von Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Partner Center Advanced Publishing (GDN)</td>
        <td><a href="https://developer.xboxlive.com/windows/documentation/Pages/home.aspx">Leitfaden für die erweiterte Veröffentlichung von Partner Center</a></td>
    </tr>
    <tr>
        <td>Verwenden von Azure Active Directory (AAD) zum Hinzufügen von Benutzern zu Ihrem Partner Center-Konto</td>
        <td><a href="/windows/uwp/publish/manage-account-users">Verwalten von Kontobenutzern</a></td>
    </tr>
    <tr>
        <td>Bewertung des Spiels (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Einzelner Workflow zum Zuweisen von Altersfreigaben mit IARC-System</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Packen und Hochladen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informationen zur Verwendung der streaminginstallation und optionaler Pakete (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">NextGen UWP-App-Verteilung: aufbauen erweiterbarer, streambarer, Komponenten basierter apps</a></td>
    </tr>
    <tr>
        <td>Teilen und Gruppieren von Inhalten zum Aktivieren der streaminginstallation</td>
        <td><a href="/windows/msix/package/streaming-install">UWP-App-Streaming-Installation</a></td>
    </tr>
    <tr>
        <td>Optionale Pakete wie DLC-Spielinhalte erstellen</td>
        <td><a href="/windows/msix/package/optional-packages">Optionale Pakete und die Erstellung zugehöriger Sets</a></td>
    </tr>
    <tr>
        <td>Packen Sie Ihr UWP-Spiel</td>
        <td><a href="../packaging/index.md">Verpacken von Apps</a></td>
    </tr>
    <tr>
        <td>Packen Sie Ihr UWP DirectX-Spiel</td>
        <td><a href="package-your-windows-store-directx-game.md">Packen Sie Ihr UWP DirectX-Spiel</a></td>
    </tr>
    <tr>
        <td>Packen Ihres Spiels als Drittentwickler (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Erstellen von hochladbaren Paketen ohne Zugriff auf das Store-Konto des Veröffentlichers</a></td>
    </tr>
    <tr>
        <td>Erstellen von App-Paketen und App-Paketbündeln mit MakeAppx</td>
        <td><a href="/windows/msix/package/create-app-package-with-makeappx-tool">Erstellen von Paketen mit dem App-Verpackungsprogramm MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Digitales Signieren Ihrer Dateien mithilfe von SignTool</td>
        <td><a href="/windows/desktop/SecCrypto/signtool">Signieren von Dateien und Überprüfen von Signaturen in Dateien mithilfe von SignTool</a></td>
    </tr>
    <tr>
        <td>Hochladen und Verwalten der Versionen Ihres Spiels</td>
        <td><a href="/windows/uwp/publish/upload-app-packages">Hochladen von App-Paketen</a></td>
    </tr>
</table>

### <a name="policies-and-certification"></a>Richtlinien und Zertifizierung

Stellen Sie sicher, dass sich die Veröffentlichung Ihres Spiels nicht aufgrund von Zertifizierungsproblemen verzögert. Hier finden Sie Richtlinien und Informationen zu gängigen Zertifizierungsproblemen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Microsoft Store App-Entwickler Vereinbarung</td>
        <td><a href="/legal/windows/agreements/app-developer-agreement">Vereinbarung für App-Entwickler</a></td>
    </tr>
    <tr>
        <td>Richtlinien zum Veröffentlichen von apps im Microsoft Store</td>
        <td><a href="/legal/windows/agreements/store-policies">Microsoft Store-Richtlinien</a></td>
    </tr>
    <tr>
        <td>So wird's gemacht: Vermeiden allgemeiner Probleme bei der App-Zertifizierung</td>
        <td><a href="/windows/uwp/publish/avoid-common-certification-failures">Vermeiden allgemeiner Zertifizierungsfehler</a></td>
    </tr>
</table>

### <a name="store-manifest-storemanifestxml"></a>Store-Manifest („StoreManifest.xml“)

Das Store-Manifest („StoreManifest.xml“) ist eine optionale Konfigurationsdatei, die Sie Ihrem App-Paket hinzufügen können. Das Store-Manifest bietet zusätzliche Features, die über den Umfang der Datei „AppxManifest.xml“ hinausgehen. So können Sie mithilfe des Store-Manifests etwa die Installation Ihres Spiels blockieren, wenn ein Zielgerät nicht über die mindestens erforderliche DirectX-Featureebene verfügt oder der verfügbare Systemspeicher nicht ausreicht.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Store-Manifest-Schema</td>
        <td><a href="/uwp/schemas/storemanifest/storemanifestschema2015/schema-root">StoreManifest-Schema (Windows 10)</a></td>
    </tr>
</table>

## <a name="game-lifecycle-management"></a>Spiellebenszyklusverwaltung

Wer glaubt, sich nach dem Abschluss der Entwicklung und der Auslieferung eines Spiels entspannt zurücklehnen zu können, irrt: Die Entwicklung von Version 1 mag zwar abgeschlossen sein, die Marktphase Ihres Spiels hat jedoch gerade erst begonnen. Sie sollten Verwendung und Fehlerberichte überwachen, auf Benutzerfeedback reagieren und Updates für Ihr Spiel veröffentlichen.

### <a name="partner-center-analytics-and-promotion"></a>Partner Center-Analyse und-herauf Stufung

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Partner Center-Analyse</td>
        <td><a href="/windows/uwp/publish/analytics">Analysieren der App-Leistung</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Ihre Kunden die Xbox-Features in Ihrem Spiel einbinden.</td>
        <td><a href="../publish/xbox-analytics-report.md">Xbox-Analysebericht</a></td>
    </tr>
    <tr>
        <td>Reagieren auf Kundenrezensionen</td>
        <td><a href="/windows/uwp/publish/respond-to-customer-reviews">Reagieren auf Kundenrezensionen</a></td>
    </tr>
    <tr>
        <td>Werbemöglichkeiten für Ihr Spiel</td>
        <td><a href="https://developer.microsoft.com/store/promote-your-apps">Bewerben Ihrer Apps</a></td>
    </tr>
</table>

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights bietet Leistungs-, Telemetrie- und Verwendungsanalysen für Ihr veröffentlichtes Spiel. Application Insights unterstützt Sie nach der Veröffentlichung Ihres Spiels beim Erkennen und Beheben von Problemen sowie bei der kontinuierlichen Überwachung und Optimierung der Verwendung und beim Nachvollziehen der weiteren Spielerinteraktionen mit Ihrem Spiel. Application Insights funktioniert durch Hinzufügen eines SDK zu Ihrer App, welches Telemetriedaten an das [Azure-Portal](https://portal.azure.com/)sendet.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analyse von Anwendungsleistung und -verwendung</td>
        <td><a href="/azure/azure-monitor/app/app-insights-overview">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>Aktivieren von Application Insights in Windows-Apps</td>
        <td><a href="/azure/azure-monitor/overview">Application Insights für Windows Phone- und Windows Store-Apps</a></td>
    </tr>
</table>

### <a name="third-party-solutions-for-analytics-and-promotion"></a>Lösungen von Drittanbietern für Analyse und herauf Stufung

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Grundlegendes zum Player Verhalten mit gameanalytics</td>
        <td><a href="https://gameanalytics.com/">Gameanalytics</a></td>
    </tr>
    <tr>
        <td>Verbinden Sie Ihr UWP-Spiel mit Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Holen Sie sich Windows SDK für Google Analytics.</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Windows SDK für Google Analytics verwenden (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Ersten Schritte mit Windows SDK für Google Analytics</a></td>
    </tr>
    <tr>
        <td>Verwenden der Facebook-App installieren von Werbeeinblendungen</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Windows SDK für Facebook erhalten</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Facebook-App-Installationen (Video) verwenden.</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Ersten Schritte mit Windows SDK für Facebook</a></td>
    </tr>
    <tr>
        <td>Verwenden von vungle zum Hinzufügen von Video-Werbeeinblendungen</td>
        <td><a href="https://publisher.vungle.com/sdk/">Windows SDK für vungle erhalten</a></td>
    </tr>
</table>

### <a name="creating-and-managing-content-updates"></a>Erstellen und Verwalten von Inhaltsaktualisierungen

Zum Aktualisieren Ihres veröffentlichten Spiels übermitteln Sie ein neues App-Paket mit einer höheren Versionsnummer. Nachdem das Paket den Übermittlungs- und Zertifizierungsprozess durchlaufen hat, wird es für die Kunden automatisch als Update verfügbar.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Aktualisieren und Verwalten der Versionen Ihres Spiels</td>
        <td><a href="/windows/uwp/publish/package-version-numbering">Paketversionsnummern</a></td>
    </tr>
    <tr>
        <td>Leitfaden für die Spielpaketverwaltung</td>
        <td><a href="/windows/uwp/publish/package-version-numbering">Leitfaden für die App-Paketverwaltung</a></td>
    </tr>
</table>

## <a name="adding-xbox-live-to-your-game"></a>Hinzufügen von Xbox Live zu Ihrem Spiel

Xbox Live ist ein erstklassiges Gamingnetzwerk, das Millionen von Spielern weltweit verbindet. Entwickler erhalten Zugriff auf Xbox Live-Features, die die Zielgruppe von spielen, einschließlich Xbox Live Anwesenheits, Bestenlisten, cloudspeicherungen, Spiele-Hubs, Clubs, Party Chat, Game DVR und mehr, erweitern können.

> [!Note]
> Wenn Sie Xbox Live-fähige Titel entwickeln möchten, stehen Ihnen mehrere Optionen zur Verfügung. Informationen zu den verschiedenen Programmen finden Sie unter [Übersicht über das Entwicklerprogramm](/gaming/xbox-live/developer-program-overview).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über Xbox Live</td>
        <td><a href="/gaming/xbox-live/index.md">Xbox Live – Entwicklerhandbuch</a></td>
    </tr>
    <tr>
        <td>Verstehen, welche Features je nach Programm verfügbar sind</td>
        <td><a href="/gaming/xbox-live/developer-program-overview.md#feature-table">Übersicht über das Entwicklerprogramm: Funktions Tabelle</a></td>
    </tr>
    <tr>
        <td>Links zu nützlichen Ressourcen für die Entwicklung von Xbox Live-spielen</td>
        <td><a href="/gaming/xbox-live/xbox-live-resources.md">Xbox Live-Ressourcen</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Informationen von Xbox Live-Diensten erhalten.</td>
        <td><a href="/gaming/xbox-live/introduction-to-xbox-live-apis.md">Einführung in Xbox Live-APIs</a></td>
    </tr>
</table>

### <a name="for-developers-in-the-xbox-live-creators-program"></a>Für Entwickler im Xbox Live Creators-Programm

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Beginnen Sie mit dem Xbox Live Creators-Programm</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Schritt-für-Schritt-Anleitung zur Integration von Xbox Live Creators Program</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zum UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Einstieg in die Entwicklung eines Xbox Live Creators-Programm Titels mit der Unity-Spiel-Engine</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs Sandbox</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Einführung in Xbox Live Sandboxes</a></td>
    </tr>
    <tr>
        <td>Einrichten von Konten für Tests</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autorisieren von Xbox Live-Konten in Ihrer Testumgebung</a></td>
    </tr>
    <tr>
        <td>Beispiele für das Xbox Live Creators-Programm</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Code Beispiele für Entwicklerprogramm Entwickler</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie plattformübergreifende Xbox Live-Erlebnisse in UWP-spielen integrieren (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Xbox Live Creators-Programm</a></td>
    </tr>
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Für verwaltete Partner und Entwickler im ID@Xbox Programm

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Beginnen Sie mit Xbox Live als verwalteter Partner oder ID-Entwickler</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Schritt-für-Schritt-Anleitung für die Integration von Xbox Live für verwaltete Partner und ID-Mitglieder</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zum UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Hinzufügen von Xbox Live-Unterstützung zu Unity für UWP mit IL2CPP Scripting Backend für ID und verwaltete Partner</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs Sandbox</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Erweiterte Xbox Live-Sand Fächer</a></td>
    </tr>
    <tr>
        <td>Anforderungen für Spiele, die Xbox Live (GDN) verwenden</td>
        <td><a href="https://edadfs.partners.extranet.microsoft.com/adfs/ls/?wa=wsignin1.0&wtrealm=https%3a%2f%2fdeveloper.xboxlive.com&wctx=rm%3d0%26id%3dpassive%26ru%3d%252fen-us%252flive%252fcertification%252frequirements%252fPages%252fTCR.aspx&wct=2019-11-20T19%3a55%3a26Z">Xbox-Anforderungen für Xbox Live unter Windows 10</a></td>
    </tr>
    <tr>
        <td>Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Code Beispiele für ID@Xbox Entwickler</a></td>
    </tr>
    <tr>
        <td>Übersicht über die Entwicklung von Spielen mit Xbox Live (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Entwickeln mit Xbox Live für Windows 10</a></td>
    </tr>
    <tr>
        <td>Plattformübergreifende Spielersuche (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live Multiplayer: Einführung in die Dienste für plattformübergreifende Spielersuche und Spiele</a></td>
    </tr>
    <tr>
        <td>Geräteübergreifendes Spielen in Fable Legends (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: Geräteübergreifendes Spielen mit Xbox Live</a></td>
    </tr>
    <tr>
        <td>Xbox Live und Erfolge (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Bewährte Methoden für die Nutzung von cloudbasierten Benutzerstatistiken und Erfolgen in Xbox Live</a></td>
    </tr>
</table>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Videos zur Entwicklung von Spielen</td>
        <td><a href="/windows/uwp/gaming/game-development-videos">Videos aus wichtigen Konferenzen wie GDC und Build</a></td>
    </tr>
    <tr>
        <td>Entwicklung von Indie-Spielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">Neue Möglichkeiten für unabhängige Entwickler</a></td>
    </tr>
    <tr>
        <td>Überlegungen für mobile Multi-Core-Geräte (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Kontinuierlich hohe Spieleleistung auf mobilen Multi-Core-Geräten</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows 10-Desktopspielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">PC-Spiele für Windows 10</a></td>
    </tr>
</table>