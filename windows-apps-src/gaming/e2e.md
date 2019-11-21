---
title: Handbuch zur Entwicklung von Spielen unter Windows 10
description: Ein umfassender Leitfaden mit Ressourcen und Informationen zur Entwicklung von Spielen für die universelle Windows-Plattform (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, Uwp, Spiele, Entwickeln von Spielen
ms.localizationpriority: medium
ms.openlocfilehash: c05a973dc9a954569531be6e0fea212135532b84
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258508"
---
# <a name="windows-10-game-development-guide"></a>Handbuch zur Entwicklung von Spielen unter Windows 10


Willkommen beim Windows 10-Handbuch für die Entwicklung von Spielen.

Dieses Handbuch enthält eine umfassende Sammlung von Ressourcen und Informationen, die Sie für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP) benötigen. Eine englische Version (USA) dieses Handbuchs steht im [PDF](https://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf)-Format zur Verfügung.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Einführung in die Spieleentwicklung für die Universelle Windows-Plattform (UWP)


Mit einem Windows 10-Spiel können Sie Millionen Smartphone-, PC- und Xbox One-Benutzer auf der ganzen Welt erreichen. Dank Xbox auf Windows, Xbox Live, geräteübergreifendem Multiplayer, einer tollen Spiele-Community und leistungsstarken neuen Features wie die Universelle Windows-Plattform (UWP) und DirectX 12 begeistern Windows 10-Spiele Spieler aller Altersklassen und Genres. Die neue universelle Windows-Plattform (UWP) gewährleistet durch eine gemeinsame API für Smartphone, PC und Xbox One die geräteübergreifende Kompatibilität Ihres Spiels unter Windows 10 und stellt zudem Tools und Optionen bereit, mit denen Sie Ihr Spiel optimal an die jeweilige Geräteumgebung anpassen können.

Dieses Handbuch enthält eine umfassende Sammlung von hilfreichen Informationen und Ressourcen für die Spieleentwicklung. Die Abschnittsstruktur orientiert sich an den Entwicklungsphasen und vereinfacht die Informationssuche.

Wenn Sie das Entwickeln von Spielen für Windows oder Xbox noch nicht kennen, sollten Sie möglicherweise mit dem Handbuch [Erste Schritte](getting-started.md) beginnen. Der Einführungsabschnitt [Ressourcen für die Spieleentwicklung](#game-development-resources) bietet ebenfalls einen allgemeinen Überblick über die Dokumentation, Programme und andere hilfreiche Ressourcen für die Spieleerstellung. Wenn Sie mit dem Einblick auf UWP-Code beginnen möchten, lesen Sie [Spielbeispiele](#game-samples).

Dieses Handbuch wird bei Bedarf mit weiteren Ressourcen für die Entwicklung von Windows 10-Spielen aktualisiert.

## <a name="game-development-resources"></a>Ressourcen für die Spieleentwicklung

Von der Dokumentation bis hin zu Entwicklerprogrammen, Foren, Blogs und Beispielen steht Ihnen bei der Spieleentwicklung eine Vielzahl hilfreicher Ressourcen zur Verfügung. Hier finden Sie eine Zusammenfassung wichtiger Ressourcen für den Einstieg in die Entwicklung Ihres Windows 10-Spiels.

> [!Note]
> Einige Features werden über verschiedene Programme verwaltet. Dieses Handbuch behandelt eine breite Palette von Ressourcen. Je nach Programmteilnahme oder spezifischer Entwicklungsrolle stehen Ihnen bestimmte Ressourcen unter Umständen nicht zur Verfügung. Beispiele wären etwa Links, die zu „developer.xboxlive.com“, „forums.xboxlive.com“, „xdi.xboxlive.com“ oder zum Netzwerk für Spieleentwickler (Game Developer Network, GDN) aufgelöst werden. Informationen zur Partnerschaft mit Microsoft finden Sie unter [Entwicklerprogramme](#developer-programs).


### <a name="game-development-documentation"></a>Dokumentation für die Spieleentwicklung

In diesem Handbuch finden Sie immer wieder direkte Links zu relevanten Dokumentationen – strukturiert nach Aufgabe, Technologie und Entwicklungsphase. Hier sehen Sie eine Übersicht über die wichtigsten verfügbaren Dokumentationsportale für die Entwicklung von Windows 10-Spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Hauptportal für Windows Dev Center</td>
        <td><a href="https://developer.microsoft.com/windows">Windows Dev Center</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows-Apps</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Develop Windows apps</a></td>
    </tr>
    <tr>
        <td>Entwicklung von UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.microsoft.com/windows/apps">How-to guides for Windows 10 apps</a></td>
    </tr>
    <tr>
        <td>Anleitungen für UWP-Spiele</td>
        <td><a href="index.md">Games and DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX-Referenz und -Übersichten</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Azure für Gaming</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Build and scale your games using Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Complete backend solution for live games</a></td>
    </tr>
    <tr>
        <td>UWP auf Xbox One</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xbox-apps/index">Building UWP apps on Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP auf HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Building UWP apps on HoloLens</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Dokumentation</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/">Xbox Live developer guide</a></td>
    </tr>
    <tr>
        <td>Xbox One – Entwicklerdokumentation (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One Development</a></td>
    </tr>
    <tr>
        <td>Xbox One – Whitepapers für Entwickler (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">White Papers</a></td>
    </tr>
    <tr>
        <td>Interaktive Mixer-Dokumentation</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Add interactivity to your game</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>Partner Center

[Registering a developer account in Partner Center](https://developer.microsoft.com/store/register) is the first step towards publishing your Windows game. Mit einem Entwicklerkonto können Sie den Namen Ihres Spiels reservieren und kostenlose oder kostenpflichtige Spiele für alle Windows-Geräte an den Microsoft Store übermitteln. Sie können Ihr Spiel und Ihre spielinternen Produkte verwalten, ausführliche Analysen abrufen und Dienste aktivieren, die Spieler auf der ganzen Welt begeistern. 

Microsoft bietet ebenfalls mehrere Entwicklerprogramme an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen. We recommend seeing if any are right for you before registering for a Partner Center account. Weitere Informationen finden Sie unter [Entwicklerprogramme](#developer-programs)


### <a name="developer-programs"></a>Entwicklerprogramme

Microsoft bietet mehrere Entwicklerprogramme an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen. Erwägen Sie, an einem Entwicklerprogramm teilzunehmen, wenn Sie Spiele für Xbox One entwickeln möchten und Xbox Live-Features in Ihrem Spiel integrieren möchten. To publish a game in the Microsoft Store, you'll also need to create a developer account in [Partner Center](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Xbox Live Creators Program

Mit dem Xbox Live Creators-Programm kann jeder Xbox Live in seine Titel integrieren und sie auf Xbox One und Windows 10 veröffentlichen. Wir haben einen vereinfachten Zertifizierungsprozess ohne Konzeptgenehmigung außerhalb der standardmäßigen [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies).

Sie können Ihr Spiel im Creators-Programm ohne einen dedizierten Entwicklerkit bereitstellen, entwerfen und veröffentlichen, indem Sie nur Einzelhandels-Hardware verwenden. Laden Sie zunächst die [DEVMODE-Aktivierungs-App](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) auf Ihre Xbox One.

Treten Sie dem [ID@Xbox](https://www.xbox.com/Developers/id) bei, wenn Sie Zugriff auf weitere Xbox Live-Funktionen wünschen, dedizierte Marketing- und Entwicklungsunterstützung benötigen oder im allgemeinen Xbox One-Store vertreten sein möchten.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live Creators Program</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Erfahren Sie mehr über das Xbox Live Creators-Programm</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

Das ID@Xbox-Programm unterstützt qualifizierte Spieleentwickler bei der eigenständigen Veröffentlichung für Windows und Xbox One. Wenn Sie für Xbox One entwickeln oder Ihr Windows 10-Spiel mit Xbox Live-Features wie Gamerscore, Erfolgen und Ranglisten versehen möchten, registrieren Sie sich bei ID@Xbox. Als ID@Xbox-Entwickler erhalten Sie Zugriff auf Tools und Supportleistungen, mit denen Sie Ihrer Kreativität freien Lauf lassen und Ihren Erfolg maximieren können. We recommend that you apply to ID@Xbox first before registering for a developer account in Partner Center.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox Entwicklerprogramm</td>
        <td><a href="https://www.xbox.com/Developers/id">Independent Developer Program for Xbox One</a></td>
    </tr>
    <tr>
        <td>ID@Xbox Verbraucherwebsite</td>
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
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
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
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox-live-samples</a></td>
    </tr>
    <tr>
        <td>Beispiele für Xbox One-Spiele (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Beispiele</a></td>
    </tr>
    <tr>
        <td>Beispiele für Windows-Spiele (MSDN Code Gallery)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Microsoft Store game samples</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Spiel mit JavaScript und 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Create a UWP game in JavaScript</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Spiel mit JavaScript und 3D</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Creating a 3D JavaScript game using three.js</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein MonoGame 2D UWP-Spiel</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Create a UWP game in MonoGame 2D</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>Entwicklerforen

In Entwicklerforen können Entwickler Fragen zur Spieleentwicklung stellen und beantworten und sich mit anderen Spieleentwicklern austauschen. Darüber hinaus halten Foren häufig Lösungen für komplizierte Probleme bereit, die Entwickler bereits bewältigt haben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publishing apps and games developer forums</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">Publishing and ads-in-apps</a></td>
    </tr>
    <tr>
        <td>Entwicklerforum für UWP-Apps</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">Developing Universal Windows Platform apps</a></td>
    </tr>
    <tr>
        <td>Entwicklerforen für Desktopanwendungen</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Windows desktop applications forums</a></td>
    </tr>
    <tr>
        <td>Microsoft Store-Spiele mit DirectX (archivierte Forenbeiträge)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">Building Microsoft Store games with DirectX (archived)</a></td>
    </tr>
    <tr>
        <td>Windows 10-Entwicklerforen für verwaltete Partner</td>
        <td><a href="https://forums.xboxlive.com/users/login.html">XBOX Developer Forums: Windows 10</a></td>
    </tr>
    <tr>
        <td>DirectX-Foren</td>
        <td><a href="https://forums.directxtech.com/index.php">DirectX 12 forum</a></td>
    </tr>
    <tr>
        <td>Azure-Plattform-Foren</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Azure forum</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Forum</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Xbox Live development forum</a></td>
    </tr>
    <tr>
        <td>PlayFab-Foren</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab forums</a></td>
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
        <td>Windows 10 (Blogbeiträge)</td>
        <td><a href="https://blogs.windows.com/blog/tag/windows-10/">Posts in Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog des Visual Studio-Entwicklerteams</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">The Visual Studio Blog</a></td>
    </tr>
    <tr>
        <td>Blogs zu Visual Studio-Entwicklertools</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Developer Tools Blogs</a></td>
    </tr>
    <tr>
        <td>Somasegars-Blog zu Entwicklertools</td>
        <td><a href="https://devblogs.microsoft.com/somasegar/">Somasegar’s blog</a></td>
    </tr>
    <tr>
        <td>DirectX-Entwicklerblog</td>
        <td><a href="https://devblogs.microsoft.com/directx/">DirectX Developer blog</a></td>
    </tr>
    <tr>
        <td>Einführung in DirectX 12 (Blogbeitrag)</td>
        <td><a href="https://devblogs.microsoft.com/directx/directx-12/">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Teamblog zu Visual C++-Tools</td>
        <td><a href="https://devblogs.microsoft.com/cppblog/">Visual C++ team blog</a></td>
    </tr>
    <tr>
        <td>Blog des PIX-Teams</td>
        <td><a href="https://devblogs.microsoft.com/pix/">Performance tuning and debugging for DirectX 12 games on Windows and Xbox</a></td>
    </tr>
    <tr>
        <td>Universelle Windows-App – Blog des Bereitstellungsteams</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Build and deploy UWP apps team blog</a></td>
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
        <td><a href="game-development-platform-guide.md">Game technologies for UWP apps</a></td>
    </tr>
</table>
 

Diese drei GDC 2015-Videos vermitteln einen guten Überblick über die Entwicklung von Windows 10-Spielen und das Spielerlebnis unter Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Entwicklung von Windows 10-Spielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Developing Games for Windows 10</a></td>
    </tr>
    <tr>
        <td>Spielerlebnis unter Windows 10 (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Gaming Consumer Experience on Windows 10</a></td>
    </tr>
    <tr>
        <td>Übergreifendes Spielen im gesamten Microsoft-Ökosystem (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">The Future of Gaming Across the Microsoft Ecosystem</a></td>
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
        <td>Erstellen barrierefreier Spiele</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/accessibility-for-games">Accessibility for games</a></td>
    </tr>
    <tr>
        <td>Erstellen von Spielen mit der Cloud</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/cloud-for-games">Cloud for games</a></td>
    </tr>
    <tr>
        <td>Monetisierung eines Spiels</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/monetization-for-games">Monetization for games</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Auswählen der Grafiktechnologie und Programmiersprache

Für Windows 10-Spiele stehen verschiedene Programmiersprachen und Grafiktechnologien zur Verfügung. Der jeweilige Ansatz richtet sich nach der Art des Spiels, das Sie entwickeln, der Erfahrung und den Vorlieben Ihres Entwicklungsstudios und den bestimmten Funktionsanforderungen Ihres Spiels. Möchten Sie C#, C++ oder JavaScript verwenden? DirectX, XAML oder HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX ist die richtige Wahl für 2D/3D-Grafik und Multimediaelemente.

DirectX 12 ist schneller und effizienter als alle früheren Versionen von Direct3D. Direct3D 12 ermöglicht detaillierte Umgebungen, mehr Objekte, komplexere Effekte und eine optimale Nutzung moderner GPU-Hardware auf Windows 10 PCs und Xbox One.

Sie können weiterhin die vertraute Grafikpipeline von Direct3D 11 verwenden und gleichzeitig von den neuen Rendering- und Optimierungsfeatures profitieren, die in Direct3D 11.3 hinzugekommen sind. Und wenn Sie ein richtiger Windows-API-Entwickler für den Desktop mit Win32-Erfahrung sind, steht Ihnen unter Windows 10 auch diese Option zur Verfügung.

Dank umfangreicher Features und einer umfassenden Plattformintegration lassen sich mit DirectX selbst anspruchsvollste Spiele realisieren.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX für die UWP-Entwicklung</td>
        <td><a href="directx-programming.md">DirectX programming</a></td>
    </tr>
    <tr>
        <td>Lernprogramm: Erstellen eines UWP-DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung und -referenz</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 Graphics</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX 12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 and Graphics Education</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML ist eine benutzerfreundliche deklarative UI-Sprache mit nützlichen Features wie Animationen, Storyboards, Datenbindung, skalierbaren vektorbasierten Grafiken, dynamischer Größenänderung und Szenendiagrammen. XAML eignet sich gut für Benutzeroberflächen, Menüs, Sprites und 2D-Grafiken von Spielen. Zur Vereinfachung der UI-Layouterstellung ist XAML mit Entwurfs- und Entwicklungstools wie Expression Blend und Microsoft Visual Studio kompatibel. XAML wird häufig mit C# kombiniert, aber auch C++ ist eine gute Wahl, falls dies Ihre bevorzugte Programmiersprache ist oder Ihr Spiel hohe Ansprüche an die CPU stellt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML-Plattformübersicht</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">XAML-Plattform</a></td>
    </tr>
    <tr>
        <td>XAML-UI und -Steuerelemente</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/basics/">Controls, layouts, and text</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML5

Die HyperText Markup Language (HTML) ist eine häufig verwendete Markup-Sprache für Benutzeroberflächen, die bei Webseiten, Apps und Rich-Clients zum Einsatz kommt. Für Windows-Spiele kann HTML5 als Darstellungsschicht mit vollem Funktionsumfang genutzt werden. Dabei stehen die vertrauten Features von HTML, Zugriff auf die universelle Windows-Plattform und Unterstützung für moderne Webfeatures wie AppCache, Web-Worker, Canvas, Drag & Drop, asynchrone Programmierung und SVG zur Verfügung. Im Hintergrund wird für das HTML-Rendering die leistungsstarke DirectX-Hardwarebeschleunigung genutzt, sodass Sie weiterhin in den Genuss der Leistungsvorteile von DirectX kommen, ohne zusätzlichen Code schreiben zu müssen. HTML5 ist eine gute Wahl, wenn Sie sich mit der Webentwicklung auskennen, ein Webspiel portieren oder Sprach- und Grafikebenen nutzen möchten, die unter Umständen leichter zugänglich als andere Optionen sind. HTML5 wird zusammen mit JavaScript verwendet, kann aber auch mit Komponenten verknüpft werden, die mit C# oder C++/CX erstellt wurden.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informationen zu HTML5 und zum Dokumentobjektmodell</td>
        <td><a href="https://developer.mozilla.org/en-US/docs/Web">HTML and DOM reference</a></td>
    </tr>
    <tr>
        <td>Die HTML5-Empfehlung des W3C</td>
        <td><a href="https://www.w3.org/TR/html5/">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>Kombinieren von Darstellungstechnologien

Die Microsoft DirectX Graphic Infrastructure (DXGI) bietet Interoperabilität und Kompatibilität mit mehreren Arten von Grafiktechnologien. Für Hochleistungsgrafiken können Sie XAML und DirectX kombinieren, indem Sie XAML für Menüs und andere einfache UI-Elemente und DirectX für das Rendern von komplexen 2D- und 3D-Szenen nutzen. DXGI sorgt auch für Kompatibilität zwischen Direct2D, Direct3D, DirectWrite, DirectCompute und der Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programmieranleitung und Referenz für die DirectX Graphic Infrastructure</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi">DXGI</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">DirectX and XAML interop</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX ist eine effiziente Hochleistungssprache und bietet eine erstklassige Mischung aus Geschwindigkeit, Kompatibilität und Plattformzugriff. C++/CX erleichtert Ihnen die Nutzung aller nützlichen Gaming-Features unter Windows 10, z. B. DirectX und Xbox Live. Außerdem können Sie vorhandenen C++-Code und die dazugehörigen Bibliotheken verwenden. Mit C++/CX wird schneller, systemeigener Code erstellt, bei dem kein Aufwand für die Garbage Collection anfällt. So kann Ihr Spiel mit einer hohen Leistung und einem geringen Stromverbrauch aufwarten und somit auch eine längere Akkulaufzeit ermöglichen. Verwenden Sie C++/CX mit DirectX oder XAML, oder erstellen Sie ein Spiel mit einer Kombination aus beidem.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referenz und Übersichten für C++/CX</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx">Visual C++ Language Reference (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++-Programmieranleitung und -Referenz</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ in Visual Studio 2019</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C# (sprich: „C sharp“) ist eine moderne, innovative Programmiersprache, die einfach, leistungsstark, typsicher und objektorientiert ist. C# ermöglicht eine schnelle Entwicklung, während gleichzeitig die Vertrautheit und Ausdruckskraft von Sprachen im C-Stil gewahrt bleibt. Obwohl C# einfach zu verwenden ist, verfügt die Sprache über viele moderne Sprachfeatures wie Polymorphie, Delegate, Lambda-Elemente, Abschlüsse, Iteratormethoden, Kovarianz und LINQ-Ausdrücke (Language-Integrated Query). C# ist eine ausgezeichnete Wahl, wenn Sie XAML verwenden möchten, schnell mit der Entwicklung Ihres Spiels beginnen möchten oder bereits über C#-Erfahrung verfügen. C# wird vorrangig mit XAML genutzt. Wenn Sie also DirectX verwenden möchten, entscheiden Sie sich besser für C++, oder schreiben Sie einen Teil des Spiels als C++-Komponente, die mit DirectX interagieren kann. Eine weitere Alternative wäre [Win2D](https://github.com/Microsoft/Win2D) – eine Direct2D-Grafikbibliothek im unmittelbaren Modus für C# und C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C#-Programmieranleitung und -Referenz</td>
        <td><a href="https://docs.microsoft.com/dotnet/articles/csharp/csharp">C#-Programmiersprachenreferenz</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript ist eine dynamische Skriptsprache, die gerne für moderne Webanwendungen und Rich-Client-Anwendungen eingesetzt wird.

Bei Windows-JavaScript-Apps kann auf einfache und intuitive Weise auf die leistungsfähigen Features der universellen Windows-Plattform zugegriffen werden – in Form von Methoden und Eigenschaften objektorientierter JavaScript-Klassen. JavaScript ist für Ihr Spiel eine gute Wahl, wenn Sie aus dem Bereich der Webentwicklung kommen, sich mit JavaScript bereits auskennen oder HTML5-, CSS-, WinJS- oder JavaScript-Bibliotheken verwenden möchten. Wenn Sie Ihre Entwicklung auf DirectX oder XAML ausrichten möchten, ist C# oder C++/CX die bessere Wahl.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referenz zu JavaScript und Windows-Runtime</td>
        <td><a href="https://docs.microsoft.com/scripting/javascript/javascript-language-reference">JavaScript reference</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Use Windows Runtime components to combine languages

Mit der universellen Windows-Plattform lassen sich problemlos Komponenten in unterschiedlichen Programmiersprachen kombinieren. Create Windows Runtime components in C++, C#, or Visual Basic, and then call into them from JavaScript, C#, C++, or Visual Basic. Dies ist eine hervorragende Möglichkeit, wenn Sie Teile des Spiels in der Sprache Ihrer Wahl programmieren möchten. Über Komponenten können Sie außerdem externe Bibliotheken nutzen, die nur in einer bestimmten Programmiersprache verfügbar sind, oder auch älteren Code, den Sie bereits geschrieben haben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>How to create Windows Runtime components</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Komponenten für Windows-Runtime mit C++/CX</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>Welche DirectX-Version sollte Ihr Spiel verwenden?

If you are choosing DirectX for your game, you'll need to decide which version to use: Microsoft Direct3D 12 or Microsoft Direct3D 11.

DirectX 12 ist schneller und effizienter als alle früheren Versionen von Direct3D. Direct3D 12 ermöglicht detaillierte Umgebungen, mehr Objekte, komplexere Effekte und eine optimale Nutzung moderner GPU-Hardware auf Windows 10 PCs und Xbox One. Da Direct3D 12 auf einer sehr niedrigen Ebene ausgeführt wird, erhält ein erfahrenes Grafikentwicklungs- oder DirectX 11-Entwicklungsteam alle notwendigen Steuerungsmöglichkeiten für die Maximierung der Grafikoptimierung.

Direct3D 11.3 ist eine Grafik-API auf einem niedrigen Niveau, die das vertraute Direct3D-Programmiermodell verwendet und Ihnen einen größeren Teil der Komplexität abnimmt, die mit dem GPU-Rendering verbunden ist. Sie wird auch von Windows 10 und Xbox One unterstützt. Wenn Sie über ein vorhandenes Modul verfügen, das in Direct3D 11 geschrieben wurde, und noch nicht bereit sind, zu Direct3D 12 zu wechseln, können Sie Direct3D 11 auf 12 verwenden, um einige Leistungsverbesserungen zu erzielen. Die Versionen ab 11.3 enthalten die neuen Rendering- und Optimierungsfeatures, die auch in Direct3D 12 zur Verfügung stehen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Choosing Direct3D 12 or Direct3D 11</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/what-is-directx-12-">What is Direct3D 12?</a></td>
    </tr>
    <tr>
        <td>Overview of Direct3D 11</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11">Direct3D 11 Graphics</a></td>
    </tr>
    <tr>
        <td>Übersicht über „Direct3D 11 on 12“</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-11-on-12">Direct3D 11 on 12</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>Brücken, Spielengines und Middleware

Mithilfe von Brücken, Spielengines und Middleware können Sie je nach Spiel unter Umständen die Entwicklung und das Testing beschleunigen und den damit verbundenen Ressourcenaufwand verringern. Hier ist eine Übersicht und Ressourcen für Brücken, Spielengines und Middleware.

#### <a name="universal-windows-platform-bridges"></a>Brücken für die universelle Windows-Plattform

Bei Brücken für die universelle Windows-Plattform handelt es sich um Technologien für die UWP-Portierung Ihrer vorhandenen Apps oder Spiele. Brücken eignen sich sehr gut für den schnellen Einstieg in die Entwicklung von UWP-Spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP Brücken</td>
        <td><a href="https://developer.microsoft.com/windows/bridges">Bring your code to Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für iOS</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/ios">Bring your iOS apps to Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für Desktop-Anwendungen (.NET und Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Convert your desktop application to a UWP app</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

PlayFab ist jetzt Teil von Microsoft Family und eine vollständige Back-End-Plattform für Live-Spiele und ein leistungsstarkes Mittel für die ersten Schritte unabhängiger Studios. Steigern Sie Umsatz, Engagement und Bindung – bei gleichzeitigen Kosteneinsparungen – mit Spieldiensten, Echtzeitanalysen und LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Overview of tools and services</a></td>
    </tr>
    <tr>
        <td>Erste Schritte</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">General getting started guide</a></td>
    </tr>
    <tr>
        <td>Video-Lernprogrammserie</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Series of demo videos about PlayFab's core systems</a></td>
    </tr>
    <tr>
        <td>Rezepte</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Popular game mechanics and design pattern samples</a></td>
    </tr>
    <tr>
        <td>Plattformen</td>
        <td><a href="https://api.playfab.com/platforms">Specific documentation for various platforms and game engines</a></td>
    </tr>
    <tr>
        <td>GitHub-Repository</td>
        <td><a href="https://github.com/PlayFab">Get scripts and SDKs for various platforms including Android, iOS, Windows, Unity, and Unreal.</a></td>
    </tr>
    <tr>
        <td>API-Dokumentation</td>
        <td><a href="https://api.playfab.com/documentation/">Access PlayFab service directly via REST-like Web APIs</a></td>
    </tr>
    <tr>
        <td>Foren</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab forums</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity bietet eine Plattform zum Erstellen ansprechender 2D, 3D, VR, und AR-Spiele und Apps. Es ermöglicht Ihnen, Ihre kreative Vision schnell umzusetzen und Ihre Inhalte auf nahezu alle Medien oder Geräten anzubieten.

Unity unterstützt ab Unity 5.4 die Direct3D 12-Entwicklung.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Die Unity-Spielengine</td>
        <td><a href="https://unity.com/">Unity - Game Engine</a></td>
    </tr>
    <tr>
        <td>Unity herunterladen</td>
        <td><a href="https://store.unity.com/">Unity herunterladen</a></td>
    </tr>
    <tr>
        <td>Unity-Dokumentation für Windows</td>
        <td><a href="https://docs.unity3d.com/Manual/Windows.html">Unity Manual / Windows</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Getting started - Make your first PlayFab API call from your Unity game</a></td>
    </tr>
    <tr>
        <td>Wie fügen Sie Ihrem Spiel mit dem interaktiven Mixer Interaktivität hinzu</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Getting started guide</a></td>
    </tr>
    <tr>
        <td>Mixer SDK für Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Mixer Unity plugin</a></td>
    </tr>
    <tr>
        <td>Vollständige Referenzdokumentation für Mixer SDK für Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">API reference for Mixer Unity plugin</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen eines Unity-Spiels im Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Porting guide</a></td>
    </tr>
    <tr>
        <td>Problembehandlung bei fehlenden Assemblyverweisen im Zusammenhang mit .NET APIs</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Missing .NET APIs in Unity and UWP</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen eines Unity-Spiels als UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">How to publish your Unity game as a UWP app</a></td>
    </tr>
    <tr>
        <td>Verwenden von Unity zum Erstellen von Windows-Spielen und -Apps (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Making Windows games and apps with Unity</a></td>
    </tr>
    <tr>
        <td>Unity-Spielentwicklung mit Visual Studio (Videoserie)</td>
        <td><a href="https://www.youtube.com/playlist?list=PLReL099Y5nRfseAg0k1SJOlpqdcsDs8Em">Using Unity with Visual Studio 2015</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Mit den Tools und Technologien aus der modular aufgebauten Suite von Havok erreichen Spieleentwickler eine noch nie dagewesene Interaktivität und Immersion. Havok bietet eine äußerst realistische Physik, interaktive Simulationen und beeindruckende Effekte. Version 2015.1 und höher unterstützen UWP in Visual Studio 2015 auf x86-, 64-Bit- und ARM-Plattformen.

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
        <td><a href="https://www.havok.com/products/">Havok Product Overview</a></td>
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
        <td><a href="https://www.monogame.net">MonoGame website</a></td>
    </tr>
    <tr>
        <td>MonoGame-Dokumentation</td>
        <td><a href="https://www.monogame.net/documentation/">MonoGame Documentation (latest)</a></td>
    </tr>
    <tr>
        <td>MonoGame-Downloads</td>
        <td><a href="https://www.monogame.net/downloads/">Laden Sie Versionen, Entwicklungsbuilds und Quellcode</a> von der MonoGame-Website herunter, oder <a href="https://www.nuget.org/profiles/MonoGame">rufen Sie die neueste Version über NuGet ab</a>.
    </tr>
    <tr>
        <td>Beispiel für ein MonoGame 2D UWP-Spiel</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Create a UWP game in MonoGame 2D</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-X ist eine plattformübergreifende Suite mit Open-Source-Spieleentwicklungsengine und Tools, die die Erstellung von UWP-Spielen unterstützt. Ab Version 3 werden auch 3D-Features hinzugefügt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-X</td>
        <td><a href="https://www.cocos2d-x.org/">What is Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X-Programmieranleitung</td>
        <td><a href="https://www.cocos2d-x.org/programmersguide/">Cocos2d-x Programmers Guide</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X unter Windows 10 (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Running Cocos2d-x on Windows 10</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Getting started - Make your first PlayFab API call from your Cocos2d game</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 ist eine umfassende, für alle Arten von Spielen und Entwicklern geeignete Suite mit Tools für die Spieleentwicklung. Die Unreal Engine wird von Spieleentwicklern auf der ganzen Welt für besonders anspruchsvolle Konsolen- und PC-Spiele eingesetzt.

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
        <td>Fügen Sie mit PlayFab LiveOps hinzu - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Getting started - Make your first PlayFab API call from your Unreal game</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu - Entwürfe</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Getting started - Make your first PlayFab API call from your Unreal game</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS ist ein vollständiges JavaScript-Framework für die Erstellung von 3D-Spielen mit HTML5, WebGL, WebVR und Web-Audio.

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
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Learning WebGL 3D and BabylonJS</a></td>
    </tr>
    <tr>
        <td>Erstellen eines plattformübergreifenden WebGL-Spiels mit BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Use BabylonJS to develop a cross-platform game</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/w8x-to-uwp-root">Wechsel von Windows-Runtime 8.x zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Windows 8-App zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Porting 8.1 Apps to Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren einer iOS-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/ios-to-uwp-root">Wechsel von iOS zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Silverlight-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/wpsl-to-uwp-root">Wechsel von Windows Phone Silverlight zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren von XAML oder Silverlight zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Porting an App from XAML or Silverlight to Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren eines Xbox-Spiels zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Porting from Xbox One to Windows 10 UWP</a></td>
    </tr>
    <tr>
        <td>Portieren von DirectX 9 zu DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Port from DirectX 9 to Universal Windows Platform (UWP)</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D 11 zu Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Porting from Direct3D 11 to Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portieren von OpenGL ES zu Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Port from OpenGL ES 2.0 to Direct3D 11</a></td>
    </tr>
    <tr>
        <td>OpenGL ES 2.0 zu Direct3D 11 mit ANGLE</td>
        <td><a href="https://github.com/microsoft/angle/wiki">ANGLE</a></td>
    </tr>
    <tr>
        <td>Entsprechungen für die klassische Windows-API in der UWP</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">Alternatives to Windows APIs in Universal Windows Platform (UWP) apps</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Prototyp und Design


Nachdem Sie sich entschieden haben, welche Art von Spiel Sie entwickeln und welche Tools und Grafiktechnologie Sie dabei verwenden möchten, können Sie sich der Gestaltung zuwenden und einen Prototyp entwickeln. Da es sich bei Ihrem Spiel im Grunde um eine UWP-App (Universelle Windows-Plattform) handelt, beginnen Sie dort.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Einführung in die universelle Windows-Plattform (UWP)

Windows 10 führt die universelle Windows-Plattform (UWP) ein. Diese stellt eine gemeinsame, übergreifende API-Plattform für Windows 10-Geräte bereit. Bei der UWP handelt es sich um eine Weiterentwicklung und Erweiterung des Windows-Runtime-Modells zu einem geschlossenen, einheitlichen Kern. Für die UWP entwickelte Spiele können WinRT-APIs aufrufen, die bei allen Geräten vorhanden sind. Da die UWP eine garantierte API-Ebene bereitstellt, können Sie ein einzelnes App-Paket erstellen, das dann auf allen Windows 10-Geräten installiert werden kann. Bei Bedarf kann Ihr Spiel natürlich auch weiterhin spezifische APIs der Geräte aufrufen, auf denen das Spiel ausgeführt wird – etwa einige klassische Windows-APIs von Win32 und .NET.

Im Anschluss finden Sie praktische Handbücher, die sich ausführlich mit UWP-Apps (Universelle Windows-Plattform) auseinandersetzen und hilfreiche Erkenntnisse zur Plattform liefern.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einführung in UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp">What's a Universal Windows Platform app?</a></td>
    </tr>
    <tr>
        <td>Übersicht über die UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide">Anleitung für UWP-Apps</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/get-set-up">Vorbereiten</a></td>
    </tr>
</table>

Wenn Sie noch keine Erfahrungen mit der UWP-Programmierung haben und die Verwendung von XAML in Ihrem Spiel in Betracht ziehen (siehe [Auswählen von Grafiktechnologie und Programmiersprache](#choosing-your-graphics-technology-and-programming-language)), ist die Videoserie [Windows 10-Entwicklung für Neueinsteiger](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) ein guter Ausgangspunkt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einsteigerhandbuch für die Windows 10-Entwicklung mit XAML (Videoserie)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Windows 10 development for absolute beginners</a></td>
    </tr>
    <tr>
        <td>Ankündigung der Windows 10-Neueinsteigerserie mit XAML (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Windows 10 development for absolute beginners</a></td>
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
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Develop Windows apps</a></td>
    </tr>
    <tr>
        <td>Übersicht über die Netzwerkprogrammierung der UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/networking/index">Netzwerk und Webdienste</a></td>
    </tr>
    <tr>
        <td>Verwenden von „Windows.Web.HTTP“ und „Windows.Networking.Sockets“ in Spielen</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Networking for games</a></td>
    </tr>
    <tr>
        <td>Asynchrone Programmierkonzepte der UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Asynchrone Programmierung</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows Desktop APIs to UWP

Hier finden Sie einige Links, die Sie beim Wechsel von Windows-Desktop-Spielen zu UWP-Spielen unterstützen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Verwenden Sie vorhandenen C++-Code für die UWP-Spieleentwicklung</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">How to: Use existing C++ code in a UWP app</a></td>
    </tr>
    <tr>
        <td>UWP-APIs für Win32- und COM-APIs</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">Win32 and COM APIs for UWP apps</a></td>
    </tr>
    <tr>
        <td>Nicht unterstützte CRT-Funktionen in UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/crt-functions-not-supported-in-universal-windows-platform-apps">CRT functions not supported in Universal Windows Platform apps</a></td>
    </tr>
    <tr>
        <td>Alternativen zu Windows-APIs</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/alternatives-to-windows-apis-uwp">Alternatives to Windows APIs in Universal Windows Platform (UWP) apps</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">App-Lebenszyklus</a></td>
    </tr>
    <tr>
        <td>Auslösen von App-Übergängen mithilfe von Microsoft Visual Studio</td>
        <td><a href="https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015">How to trigger suspend, resume, and background events for UWP apps in Visual Studio</a></td>
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
        <td><a href="https://developer.microsoft.com/en-us/windows/apps/design">Gestalten von UWP-Apps</a></td>
    </tr>
    <tr>
        <td>Gestalten für App-Lebenszykluszustände</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/index">UX guidelines for launch, suspend, and resume</a></td>
    </tr>
    <tr>
        <td>Entwerfen Sie Ihre UWP-App für Xbox One und Fernsehgeräte</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Entwerfen für Xbox und Fernsehgeräte</a></td>
    </tr>
    <tr>
        <td>Ausrichten auf verschiedene Geräteformfaktoren (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Designing Games for a Windows Core World</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>Farbrichtlinie und -palette

Eine einheitliche Farbrichtlinie verbessert die Spielästhetik, vereinfacht die Navigation und ist ein wirksames Mittel, um Spieler über Menü- und HUD-Funktionen zu informieren. Eine einheitliche Farbgestaltung von Spielelementen wie Warnungen, Schäden, Erfahrungspunkten und Erfolgen kann die Übersichtlichkeit der Benutzeroberfläche verbessern und explizite Beschriftungen überflüssig machen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Farbhandbuch</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Best Practices: Color</a></td>
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
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_Typography.pdf">Best Practices: Typography</a></td>
    </tr>
</table>
 

#### <a name="ui-map"></a>UI-Zuordnung

Bei einer UI-Zuordnung handelt es sich um eine Layoutübersicht über die Navigation und Menüs eines Spiels in Form eines Flussdiagramms. Die UI-Zuordnung verbessert die Nachvollziehbarkeit der Oberfläche und der Navigationspfade eines Spiels für alle Beteiligten und trägt zur frühzeitigen Erkennung potenzieller Probleme und Sackgassen bei.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Anleitung für die UI-Zuordnung</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_UI_Map.pdf">Best Practices: UI Map</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Audio in Spielen

Anleitungen und Referenzen für die Implementierung von Audio in Spielen mit XAudio2, XAPO und Windows Sonic. XAudio2 ist eine Low-Level-Audio-API, die eine grundlegende Signalverarbeitung und -abmischung zum Entwickeln von Audiomodulen mit hoher Leistung für Spiele bereitstellt. XAPO-API ermöglicht die Erstellung von plattformübergreifenden Audioverarbeitungsobjekten (XAPO) zur Verwendung in XAudio2 für Windows und Xbox. Mit der Windows Sonic Audiounterstützung können Sie Ihrem Spiel oder der Streaming-Media-Anwendung Dolby Atmos for Home Theater, Dolby Atmos for Headphones und Windows kopfbezogene Übertragungsfunktionen hinzufügen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAudio2-APIs</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal">Programming guide and API reference for XAudio2</a></td>
    </tr>
    <tr>
        <td>Plattformübergreifende Audioverarbeitungsobjekte erstellen</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview">XAPO Overview</a></td>
    </tr>
    <tr>
        <td>Einführung in Audio-Konzepte</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio for games</a></td>
    </tr>
    <tr>
        <td>Übersicht über Windows Sonic</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound">Raumklang</a></td>
    </tr>
    <tr>
        <td>Windows-Sonic Raumklang - Beispiele</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Xbox Advanced Technology Group audio samples</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie Windows Sonic in Ihre Spiele (Video) integrieren</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Introducing Spatial Audio Capabilities for Xbox and Windows</a></td>
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
        <td>DirectX für die UWP-Entwicklung</td>
        <td><a href="directx-programming.md">DirectX programming</a></td>
    </tr>
    <tr>
        <td>Lernprogramm: Erstellen eines UWP-DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>DirectX-Interaktionen mit dem UWP-App-Modell</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">The app object and DirectX</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX 12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 and Graphics Education</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung und -referenz</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 Graphics</a></td>
    </tr>
    <tr>
        <td>DirectX 12-Grundlagen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Better Power, Better Performance: Your Game on DirectX 12</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/directx-12-programming-environment-set-up">Direct3D 12 programming environment setup</a></td>
    </tr>
    <tr>
        <td>Erstellen einer Grundkomponente</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Creating a basic Direct3D 12 component</a></td>
    </tr>
    <tr>
        <td>Änderungen in Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/important-changes-from-directx-11-to-directx-12">Important changes migrating from Direct3D 11 to Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D 11 zu Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Porting from Direct3D 11 to Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Konzepte für die Ressourcenbindung (deckt Deskriptor, Deskriptortabelle, Deskriptorheap und Stammsignatur ab) </td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/resource-binding">Resource binding in Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Verwalten des Arbeitsspeichers</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/memory-management">Memory management in Direct3D 12</a></td>
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
        <td><a href="https://github.com/Microsoft/DirectXTK12">DirectXTK 12</a></td>
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
        <td>Direct3D 12 support in the DirectXTK (blog post)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Support for DirectX 12</a></td>
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
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX 12 on Nvidia GPUs</a></td>
    </tr>
    <tr>
        <td>Intel: Effizientes Rendering mit DirectX 12</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">DirectX 12 rendering on Intel Graphics</a></td>
    </tr>
    <tr>
        <td>Intel: Mehrfachadapterunterstützung in DirectX 12</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">How to implement an explicit multi-adapter application using DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12-Lernprogramm</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Collaborative white paper by Intel, Suzhou Snail and Microsoft</a></td>
    </tr>
</table>


## <a name="production"></a>Production um


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
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">Kacheln, Signale und Benachrichtigungen</a></td>
    </tr>
    <tr>
        <td>Beispiel zur Veranschaulichung von Live-Kacheln und Benachrichtigungen</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Notifications sample</a></td>
    </tr>
    <tr>
        <td>Vorlagen für adaptive Kacheln (Blogbeitrag)</td>
        <td><a href="https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/06/30/adaptive-tile-templates-schema-and-documentation/">Adaptive Tile Templates - Schema and Documentation</a></td>
    </tr>
    <tr>
        <td>Gestalten von Kacheln und Signalen</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Richtlinien für Kacheln und Signale</a></td>
    </tr>
    <tr>
        <td>Windows 10-App für die interaktive Entwicklung von Vorlagen für Live-Kacheln</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio</td>
        <td><a href="https://marketplace.visualstudio.com/items?itemName=shenchauhan.UWPTileGenerator">Tool for creating all required tiles using single image</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Tips on using the UWP Tile Generator tool</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>Enable in-app product (add-on) purchases

An add-on (in-app product) is a supplementary item that players can purchase in-game. Add-ons can be game levels, items, or anything else that your players might enjoy. Used appropriately, add-ons can provide revenue while improving the game experience. You define and publish your game's add-ons through Partner Center, and enable in-app purchases in your game's code.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Durable add-ons</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">Aktivieren von In-App-Produktkäufen</a></td>
    </tr>
    <tr>
        <td>Consumable add-ons</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Unterstützen von Käufen konsumierbarer In-App-Produkte</a></td>
    </tr>
    <tr>
        <td>Add-on details and submission</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">Add-On-Übermittlungen</a></td>
    </tr>
    <tr>
        <td>Monitor add-on sales and demographics for your game</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-acquisitions-report">Bericht zu Add-On-Käufen</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>Debuggen, Leistungsoptimierung und Überwachung

Zur Optimierung der Leistung, nutzen Sie den Spielmodus in Windows 10, um Ihren Spielern ein optimales Erlebnis durch die vollständige Nutzung der Kapazität ihrer aktuellen Hardware zu gewährleisten.

Das Windows Performance Toolkit (WPT) besteht aus Leistungsüberwachungstools, die detaillierte Leistungsprofile von Windows-Betriebssystemen und -Anwendungen erstellen. Dies ist besonders hilfreich für die Überwachung der Speicherverwendung und zum Verbessern der Leistung eines Spiels. Das Windows Performance Toolkit ist im SDK für Windows 10 und im Windows ADK enthalten. Dieses Toolkit besteht aus zwei unabhängigen Tools: Windows Performance Recorder (WPR) und Windows Performance Analyzer (WPA). ProcDump ist Teil von [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default) und ist ein Befehlszeilenprogramm, das die Leistungsspitzen der CPU überwacht und Speicherabbilddateien während der Spielabstürze generiert. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Leistungstest für Ihren Code</td>
        <td><a href="https://azure.microsoft.com/services/devops/test-plans/">Cloud based load testing</a></td>
    </tr>
    <tr>
        <td>Erhalten Sie Xbox-Konsolentypen mithilfe der Geräteinformationen für Spiele</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamingdvcinfo/gaming-device-information-portal">Gaming Device Information</a></td>
    </tr>
    <tr>
        <td>Verbessern Sie die Leistung durch einen exklusiven oder prioritären Zugriff auf Hardwareressourcen mithilfe der Spielmodus-APIs</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal">Spielmodus</a></td>
    </tr>
    <tr>
        <td>Abrufen von Windows Performance Toolkit (WPT) aus dem Windows 10 SDK</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Windows 10 SDK</a></td>
    </tr>
    <tr>
        <td>Abrufen von Windows Performance Toolkit (WPT) aus dem Windows ADK</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Problembehandlung bei nicht reagierender Benutzeroberfläche mit Windows Performance Analyzer (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Critical path analysis with WPA</a></td>
    </tr>
    <tr>
        <td>Diagnostizieren von Speicherverwendung und Arbeitsspeicherverlusten mit Windows Performance Recorder (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Memory footprint and leaks</a></td>
    </tr>
    <tr>
        <td>Abrufen von ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>Informationen zur Verwendung von ProcDump (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Configure ProcDump to create dump files</a></td>
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
        <td>PIX für Windows</td>
        <td><a href="https://devblogs.microsoft.com/pix/introducing-pix-on-windows-beta/">Performance tuning and debugging tool for DirectX 12 on Windows</a></td>
    </tr>
    <tr>
        <td>Überprüfungs- und Debugg-Tools für die Entwicklung von D3D12 (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 Performance Tuning and Debugging with PIX and GPU Validation</a></td>
    </tr>
    <tr>
        <td>Grafik- und Leistungsoptimierung (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Advanced DirectX 12 Graphics and Performance</a></td>
    </tr>
    <tr>
        <td>Debuggen von DirectX-Grafiken (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Solve the tough graphics problems with your game using DirectX Tools</a></td>
    </tr>
    <tr>
        <td>Visual Studio 2015-Tools zum Debuggen von DirectX 12 (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">DirectX tools for Windows 10 in Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Direct3D 12-Programmieranleitung</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 Programming Guide</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">DirectX and XAML interop</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Entwicklung von HDR (HDR)-Inhalt

Erstellen Sie Spieleinhalt, der die vollständigen Farbfunktionen von HDR verwendet.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einführung in die Konzepte für HDR und Farbe (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Lighting up HDR and advanced color in DirectX</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie HDR-Inhalt darstellen, und ermitteln Sie, ob die aktuelle Anzeige dies unterstützt</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR sample</a></td>
    </tr>
    <tr>
        <td>Erstellen Sie und konfigurieren Sie eine erweiterte Farbe mit DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Direct2D advanced color image rendering sample</a></td>
    </tr>   
</table>


### <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung

Entwickeln Sie Windows-Spiele für den weltweiten Markt, und erfahren Sie mehr über die internationalen Features, die in die führenden Produkte von Microsoft integriert sind.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vorbereiten Ihres Spiels für den globalen Markt</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal">Guidelines when developing for a global audience</a></td>
    </tr>
    <tr>
        <td>Überwinden sprachlicher, kultureller und technologischer Barrieren</td>
        <td><a href="https://www.microsoft.com/Language/Default.aspx">Online resource for language conventions and standard Microsoft terminology</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Übermitteln und Veröffentlichen Ihres Spiels

Die folgenden Handbücher und Informationen sorgen für eine möglichst reibungslose Veröffentlichung und Übermittlung.

### <a name="publishing"></a>Publishing

You'll use [Partner Center](https://partner.microsoft.com/dashboard) to publish and manage your game packages.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Partner Center app publishing</td>
        <td><a href="https://developer.microsoft.com/store/publish-apps">Veröffentlichen von Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Partner Center advanced publishing (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">Partner Center advanced publishing guide</a></td>
    </tr>
    <tr>
        <td>Use Azure Active Directory (AAD) to add users to your Partner Center account</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">Manage account users</a></td>
    </tr>   
    <tr>
        <td>Bewertung des Spiels (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Single workflow to assign age ratings using IARC system</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Packen und Hochladen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informationen zur Verwendung der Streaming-Installation und optionale Pakete (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Nextgen UWP app distribution: Building extensible, stream-able, componentized apps</a></td>
    </tr>
    <tr>
        <td>Unterteilen und Gruppieren zum Aktivieren von Inhalten für die Streaming-Installation</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/streaming-install">UWP App Streaming install</a></td>
    </tr>
    <tr>
        <td>Optionale Pakete wie DLC-Spielinhalte erstellen</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/optional-packages">Optionale Pakete und die Erstellung zugehöriger Sets</a></td>
    </tr>
    <tr>
        <td>Packen Ihres UWP-Spiels</td>
        <td><a href="../packaging/index.md">Verpacken von Apps</a></td>
    </tr>
    <tr>
        <td>Packen Ihres UWP-DirectX-Spiels</td>
        <td><a href="package-your-windows-store-directx-game.md">Package your UWP DirectX game</a></td>
    </tr>
    <tr>
        <td>Packen Ihres Spiels als Drittentwickler (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Create uploadable packages without publisher's store account access</a></td>
    </tr>
    <tr>
        <td>Erstellen von App-Paketen und App-Paketbündeln mit MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool">Create packages using app packager tool MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Digitales Signieren Ihrer Dateien mithilfe von SignTool</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/SecCrypto/signtool">Sign files and verify signatures in files using SignTool</a></td>
    </tr>    
    <tr>
        <td>Hochladen und Verwalten der Versionen Ihres Spiels</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/upload-app-packages">Upload app packages</a></td>
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
        <td>Vereinbarung für Entwickler von Microsoft Store-Apps</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement">Vereinbarung für App-Entwickler</a></td>
    </tr>
    <tr>
        <td>Richtlinien für die Veröffentlichung von Apps im Microsoft Store</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/store-policies">Microsoft Store-Richtlinien</a></td>
    </tr>
    <tr>
        <td>So wird's gemacht: Vermeiden allgemeiner Probleme bei der App-Zertifizierung</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/avoid-common-certification-failures">Avoid common certification failures</a></td>
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
        <td><a href="https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root">StoreManifest schema (Windows 10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Spiellebenszyklusverwaltung


Wer glaubt, sich nach dem Abschluss der Entwicklung und der Auslieferung eines Spiels entspannt zurücklehnen zu können, irrt: Die Entwicklung von Version 1 mag zwar abgeschlossen sein, die Marktphase Ihres Spiels hat jedoch gerade erst begonnen. Sie sollten Verwendung und Fehlerberichte überwachen, auf Benutzerfeedback reagieren und Updates für Ihr Spiel veröffentlichen.

### <a name="partner-center-analytics-and-promotion"></a>Partner Center analytics and promotion

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Partner Center analytics</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/analytics">Analyze app performance</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Ihre Kunden mit der Xbox-Features in Ihrem Spiel interagieren</td>
        <td><a href="../publish/xbox-analytics-report.md">Xbox analytics report</a></td>
    </tr>
    <tr>
        <td>Reagieren auf Kundenrezensionen</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/respond-to-customer-reviews">Reagieren auf Kundenrezensionen</a></td>
    </tr>
    <tr>
        <td>Werbemöglichkeiten für Ihr Spiel</td>
        <td><a href="https://developer.microsoft.com/store/promote-your-apps">Bewerben Sie Ihre Apps</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights bietet Leistungs-, Telemetrie- und Verwendungsanalysen für Ihr veröffentlichtes Spiel. Application Insights unterstützt Sie nach der Veröffentlichung Ihres Spiels beim Erkennen und Beheben von Problemen sowie bei der kontinuierlichen Überwachung und Optimierung der Verwendung und beim Nachvollziehen der weiteren Spielerinteraktionen mit Ihrem Spiel. Application Insights fügt Ihrer App ein SDK hinzu, das Telemetriedaten an das [Azure-Portal](https://portal.azure.com/) übermittelt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analyse von Anwendungsleistung und -verwendung</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>Aktivieren von Application Insights in Windows-Apps</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Application Insights for Windows Phone and Store apps</a></td>
    </tr>
</table>


### <a name="third-party-solutions-for-analytics-and-promotion"></a>Lösungen von Drittanbietern für Analysen und Werbung

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Verstehen der Spielerverhalten mit GameAnalytics</td>
        <td><a href="https://gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Verbinden Sie Ihr UWP-Spiel mit Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Get Windows SDK for Google Analytics</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie Windows SDK für Google Analytics (Video) verwenden</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Getting started with Windows SDK for Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Verwenden der Facebook-App zum Installieren von Anzeigen, bietet Werbemöglichkeiten für Ihr Spiel für Facebook-Benutzer an</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Get Windows SDK for Facebook</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie die Facebook-App zum Installieren von Anzeigen verwenden (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Getting started with Windows SDK for Facebook</a></td>
    </tr>
    <tr>
        <td>Verwenden von Vungle, um Ihren Spielen Videoanzeigen hinzuzufügen</td>
        <td><a href="https://publisher.vungle.com/sdk/">Get Windows SDK for Vungle</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">Package version numbering</a></td>
    </tr>
    <tr>
        <td>Leitfaden für die Spielpaketverwaltung</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">Guidance for app package management</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Hinzufügen von Xbox Live zu Ihrem Spiel

Xbox Live ist ein erstklassiges Gamingnetzwerk, das Millionen von Spielern weltweit verbindet. Entwickler haben Zugriff auf Xbox Live-Features, die die Spielerzielgruppe steigern, einschließlich Xbox Live, Bestenlisten, Cloudspeicherungen, Spielehubs, Clubs, Party-Chat, Game DVR und mehr.

> [!Note]
> Wenn Sie Xbox Live aktivierte Titel entwickeln möchten, stehen Ihnen verschiedene Optionen zur Verfügung. Informationen zu den verschiedenen Programmen finden Sie unter [Übersicht über das Entwickler-Programm](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live – Übersicht</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/index.md">Xbox Live developer guide</a></td>
    </tr>
    <tr>
        <td>Verstehen, welche Features je nach Programm verfügbar sind</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#feature-table">Developer program overview: Feature table</a></td>
    </tr>
    <tr>
        <td>Links zu nützlichen Ressourcen für die Entwicklung von Xbox Live-Spielen</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/xbox-live-resources.md">Xbox Live resources</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Informationen über Xbox Live-Dienste abrufen</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/introduction-to-xbox-live-apis.md">Introduction to Xbox Live APIs</a></td>
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
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Get started with the Xbox Live Creators Program</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Step by step guide to integrate Xbox Live Creators Program</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Get started developing an Xbox Live Creators Program title with the Unity game engine</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs-Sandbox</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Xbox Live sandboxes introduction</a></td>
    </tr>
    <tr>
        <td>Einrichten von Konten zum Testen</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Authorize Xbox Live accounts in your test environment</a></td>
    </tr>
    <tr>
        <td>Beispiele für das Xbox Live Creators-Programm</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Code samples for Creators Program developers</a></td>
    </tr>
    <tr>
        <td>Enthält Informationen zum Integrieren von plattformübergreifenden Xbox Live-Umgebungen in UWP-Spielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Xbox Live Creators-Programm</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Für verwaltete Partner und Entwickler im ID@Xbox-Programm

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Get started with Xbox Live as a managed partner or an ID developer</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Step by step guide to integrate Xbox Live for managed partners and ID members</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Add Xbox Live support to Unity for UWP with IL2CPP scripting backend for ID and managed partners</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs-Sandbox</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Advanced Xbox Live sandboxes</a></td>
    </tr>
    <tr>
        <td>Anforderungen für Spiele mit Xbox Live (GDN)</td>
        <td><a href="https://edadfs.partners.extranet.microsoft.com/adfs/ls/?wa=wsignin1.0&wtrealm=https%3a%2f%2fdeveloper.xboxlive.com&wctx=rm%3d0%26id%3dpassive%26ru%3d%252fen-us%252flive%252fcertification%252frequirements%252fPages%252fTCR.aspx&wct=2019-11-20T19%3a55%3a26Z">Xbox Requirements for Xbox Live on Windows 10</a></td>
    </tr>
    <tr>
        <td>Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Code samples for ID@Xbox developers</a></td>
    </tr>  
    <tr>
        <td>Übersicht über die Entwicklung von Spielen mit Xbox Live (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Developing with Xbox Live for Windows 10</a></td>
    </tr>
    <tr>
        <td>Plattformübergreifende Spielersuche (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live Multiplayer: Introducing services for cross-platform matchmaking and gameplay</a></td>
    </tr>
    <tr>
        <td>Geräteübergreifendes Spielen in Fable Legends (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: Cross-device Gameplay with Xbox Live</a></td>
    </tr>
    <tr>
        <td>Xbox Live und Erfolge (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Best Practices for Leveraging Cloud-Based User Stats and Achievements in Xbox Live</a></td>
    </tr>
</table>


## <a name="additional-resources"></a>Weitere Ressourcen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Videos zur Entwicklung von Spielen</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Videos from major conferences like GDC and //build</a></td>
    </tr>
    <tr>
        <td>Entwicklung von Indie-Spielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">New Opportunities for Independent Developers</a></td>
    </tr>
    <tr>
        <td>Überlegungen für mobile Multi-Core-Geräte (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Sustained Gaming Performance in multi-core mobile devices</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows 10-Desktopspielen (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">PC Games for Windows 10</a></td>
    </tr>
</table>



 

 

 
