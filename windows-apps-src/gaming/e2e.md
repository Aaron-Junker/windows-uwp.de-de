---
author: joannaleecy
title: Handbuch zur Entwicklung von Spielen unter Windows10
description: Ein umfassender Leitfaden mit Ressourcen und Informationen zur Entwicklung von Spielen für die universelle Windows-Plattform (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.author: joanlee
ms.date: 04/16/2018
ms.topic: article
keywords: Windows10, Uwp, Spiele, Entwickeln von Spielen
ms.localizationpriority: medium
ms.openlocfilehash: d29e647b2932e1d89247da5b91d8f836d11260d6
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "5938490"
---
# <a name="windows-10-game-development-guide"></a>Handbuch zur Entwicklung von Spielen unter Windows10


Willkommen beim Windows10-Handbuch für die Entwicklung von Spielen.

Dieses Handbuch enthält eine umfassende Sammlung von Ressourcen und Informationen, die Sie für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP) benötigen. Eine englische Version (USA) dieses Handbuchs steht im [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf)-Format zur Verfügung.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Einführung in die Spieleentwicklung für die universelle Windows-Plattform (UWP)


Mit einem Windows10-Spiel können Sie Millionen Smartphone-, PC- und XboxOne-Benutzer auf der ganzen Welt erreichen. Dank Xbox auf Windows, Xbox Live, geräteübergreifendem Multiplayer, einer tollen Spiele-Community und leistungsstarken neuen Features wie die Universelle Windows-Plattform (UWP) und DirectX12 begeistern Windows10-Spiele Spieler aller Altersklassen und Genres. Die neue universelle Windows-Plattform (UWP) gewährleistet durch eine gemeinsame API für Smartphone, PC und Xbox One die geräteübergreifende Kompatibilität Ihres Spiels unter Windows10 und stellt zudem Tools und Optionen bereit, mit denen Sie Ihr Spiel optimal an die jeweilige Geräteumgebung anpassen können.

Dieses Handbuch enthält eine umfassende Sammlung von hilfreichen Informationen und Ressourcen für die Spieleentwicklung. Die Abschnittsstruktur orientiert sich an den Entwicklungsphasen und vereinfacht die Informationssuche.

Wenn Sie das Entwickeln von Spielen für Windows oder Xbox noch nicht kennen, sollten Sie möglicherweise mit dem Handbuch [Erste Schritte](getting-started.md) beginnen. Der Einführungsabschnitt [Ressourcen für die Spieleentwicklung](#game-development-resources) bietet ebenfalls einen allgemeinen Überblick über die Dokumentation, Programme und andere hilfreiche Ressourcen für die Spieleerstellung. Wenn Sie mit dem Einblick auf UWP-Code beginnen möchten, lesen Sie [Spielbeispiele](#game-samples).

Dieses Handbuch wird bei Bedarf mit weiteren Ressourcen für die Entwicklung von Windows10-Spielen aktualisiert.

## <a name="game-development-resources"></a>Ressourcen für die Spieleentwicklung

Von der Dokumentation bis hin zu Entwicklerprogrammen, Foren, Blogs und Beispielen steht Ihnen bei der Spieleentwicklung eine Vielzahl hilfreicher Ressourcen zur Verfügung. Hier finden Sie eine Zusammenfassung wichtiger Ressourcen für den Einstieg in die Entwicklung Ihres Windows 10-Spiels.

> [!Note]
> Einige Features werden über verschiedene Programme verwaltet. Dieses Handbuch behandelt eine breite Palette von Ressourcen. Je nach Programmteilnahme oder spezifischer Entwicklungsrolle stehen Ihnen bestimmte Ressourcen unter Umständen nicht zur Verfügung. Beispiele wären etwa Links, die zu „developer.xboxlive.com“, „forums.xboxlive.com“, „xdi.xboxlive.com“ oder zum Netzwerk für Spieleentwickler (Game Developer Network, GDN) aufgelöst werden. Informationen zur Partnerschaft mit Microsoft finden Sie unter [Entwicklerprogramme](#developer-programs).


### <a name="game-development-documentation"></a>Dokumentation für die Spieleentwicklung

In diesem Handbuch finden Sie immer wieder direkte Links zu relevanten Dokumentationen – strukturiert nach Aufgabe, Technologie und Entwicklungsphase. Hier sehen Sie eine Übersicht über die wichtigsten verfügbaren Dokumentationsportale für die Entwicklung von Windows10-Spielen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Hauptportal für Windows Dev Center</td>
        <td><a href="https://dev.windows.com">Windows Dev Center</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows-Apps</td>
        <td><a href="https://dev.windows.com/develop">Entwickeln von Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Entwicklung von UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Anleitungen für Windows 10-Apps</a></td>
    </tr>
    <tr>
        <td>Anleitungen für UWP-Spiele</td>
        <td><a href="index.md">Spiele und DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX-Referenz und -Übersichten</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Azure für Gaming</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Erstellen und Skalieren von Spielen mit Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Vollständige Back-End-Lösung für Live-Spiele</a></td>
    </tr>
    <tr>
        <td>UWP auf Xbox One</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">Erstellen von UWP-Apps auf Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP auf HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Erstellen von UWP-Apps auf HoloLens</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Dokumentation</td>
        <td><a href="../xbox-live/index.md">Xbox Live – Entwicklerhandbuch</a></td>
    </tr>
    <tr>
        <td>Xbox One – Entwicklerdokumentation (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One – Entwicklung</a></td>
    </tr>
    <tr>
        <td>Xbox One – Whitepapers für Entwickler (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Whitepapers</a></td>
    </tr>
    <tr>
        <td>Interaktive Mixer-Dokumentation</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Fügen Sie Interaktivität zu Ihrem Spiel hinzu</a></td>
    </tr>        
</table>

### <a name="windows-dev-center"></a>Windows Dev Center

Der Prozess für die Veröffentlichung Ihres Windows-Spiels beginnt mit der Registrierung eines Entwicklerkontos bei Windows Dev Center. Mit einem Entwicklerkonto können Sie den Namen Ihres Spiels reservieren und kostenlose oder kostenpflichtige Spiele für alle Windows-Geräte an den Microsoft Store übermitteln. Sie können Ihr Spiel und Ihre spielinternen Produkte verwalten, ausführliche Analysen abrufen und Dienste aktivieren, die Spieler auf der ganzen Welt begeistern. 

Microsoft bietet ebenfalls mehrere Entwicklerprogramme an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen. Wir empfehlen, vor dem Registrieren für ein Dev Center-Konto zu überprüfen, ob diese für Sie geeignet sind. Weitere Informationen finden Sie unter [Entwicklerprogramme](#developer-programs)

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entwicklerkonto registrieren</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/bg124287">Bereit für die Registrierung?</a></td>
    </tr> 
</table>

### <a name="developer-programs"></a>Entwicklerprogramme

Microsoft bietet mehrere Entwicklerprogramme an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen. Erwägen Sie, an einem Entwicklerprogramm teilzunehmen, wenn Sie Spiele für Xbox One entwickeln möchten und Xbox Live-Features in Ihrem Spiel integrieren möchten. Wenn Sie ein Spiel im Microsoft Store veröffentlichen möchten, benötigen Sie ebenfalls ein Entwicklerkonto in Windows Dev Center.

#### <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Mit dem Xbox Live Creators-Programm kann jeder Xbox Live in seine Titel integrieren und sie auf Xbox One und Windows 10 veröffentlichen. Wir haben einen vereinfachten Zertifizierungsprozess ohne Konzeptgenehmigung außerhalb der standardmäßigen [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx).

Sie können Ihr Spiel im Creators-Programm ohne einen dedizierten Entwicklerkit bereitstellen, entwerfen und veröffentlichen, indem Sie nur Einzelhandels-Hardware verwenden. Laden Sie zunächst die [DEVMODE-Aktivierungs-App](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) auf Ihre Xbox One.

Treten Sie dem [ID@Xbox](http://www.xbox.com/Developers/id) bei, wenn Sie Zugriff auf weitere Xbox Live-Funktionen wünschen, dedizierte Marketing- und Entwicklungsunterstützung benötigen oder im allgemeinen Xbox One-Store vertreten sein möchten.

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

Das ID@Xbox-Programm unterstützt qualifizierte Spieleentwickler bei der eigenständigen Veröffentlichung für Windows und Xbox One. Wenn Sie für Xbox One entwickeln oder Ihr Windows10-Spiel mit XboxLive-Features wie Gamerscore, Erfolgen und Ranglisten versehen möchten, registrieren Sie sich bei ID@Xbox. Als ID@Xbox-Entwickler erhalten Sie Zugriff auf Tools und Supportleistungen, mit denen Sie Ihrer Kreativität freien Lauf lassen und Ihren Erfolg maximieren können. Es wird empfohlen, die Sie sich zuerst an ID@Xbox wenden, bevor Sie ein Entwicklerkonto im Windows Dev Center registrieren.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox Entwicklerprogramm</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">Unabhängiges Entwicklerprogramm für Xbox One</a></td>
    </tr>
    <tr>
        <td>ID@Xbox Verbraucherwebsite</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
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

Für Windows 10-Spiele und -Apps stehen zahlreiche Beispiele zur Verfügung, die einen Eindruck von den Features von Windows 10-Spielen vermitteln und den Einstieg in die Spieleentwicklung erleichtern. Es werden regelmäßig weitere Beispiele entwickelt und veröffentlicht. Schauen Sie daher immer mal wieder bei den Beispielportalen vorbei. Darüber hinaus können Sie GitHub-Repositorys [überwachen](https://help.github.com/articles/watching-repositories/), um über Änderungen und Ergänzungen informiert zu werden.

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
        <td>Grafikbeispiele für Direct3D12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Grafikbeispiele</a></td>
    </tr>
    <tr>
        <td>Grafikbeispiele für Direct3D11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein First-Person-Spiel mit Direct3D11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>Beispiel für benutzerdefinierte Direct2D-Bildeffekte</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Direct2D-Farbverlaufsgitter</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Beispiel für eine Direct2D-Fotoanpassung</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Xbox Advanced Technology Group – öffentliche Beispiele</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Beispiele</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox-live-samples</a></td>
    </tr>
    <tr>
        <td>Beispiele für XboxOne-Spiele (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Beispiele</a></td>
    </tr>
    <tr>
        <td>Beispiele für Windows-Spiele (MSDN Code Gallery)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Beispiele für MicrosoftStore-Spiele</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Spiel mit JavaScript und 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Erstellen eines UWP-Spiels in JavaScript</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein Spiel mit JavaScript und 3D</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Erstellen eines 3D-JavaScript-Spiels mit three.js</a></td>
    </tr>
    <tr>
        <td>Beispiel für ein MonoGame 2D UWP-Spiel</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Erstellen eines UWP-Spiels in MonoGame-2D</a></td>
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
        <td>Windows-Apps-Entwicklerforen</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">Foren für Windows Store und Apps</a></td>
    </tr>
    <tr>
        <td>Entwicklerforum für UWP-Apps</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">Entwicklung von UWP-Apps (Apps für die Universelle Windows-Plattform)</a></td>
    </tr>
    <tr>
        <td>Entwicklerforen für Desktopanwendungen</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Foren für Windows-Desktopanwendungen</a></td>
    </tr>
    <tr>
        <td>Microsoft Store-Spiele mit DirectX (archivierte Forenbeiträge)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">Erstellen von MicrosoftStore-Spielen mit DirectX (archiviert)</a></td>
    </tr>
    <tr>
        <td>Windows10-Entwicklerforen für verwaltete Partner</td>
        <td><a href="http://aka.ms/win10devforums">Xbox-Entwicklerforen: Windows10</a></td>
    </tr>
    <tr>
        <td>DirectX-Foren</td>
        <td><a href="http://forums.directxtech.com/index.php">DirectX12-Forum</a></td>
    </tr>
    <tr>
        <td>Azure-Plattform-Foren</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Azure-Forum</a></td>
    </tr>
    <tr>
        <td>Xbox Live-Forum</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Xbox Live-Entwicklerforum</a></td>
    </tr>
    <tr>
        <td>PlayFab-Foren</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab-Foren</a></td>
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
        <td>Blog „Building Apps for Windows“</td>
        <td><a href="http://blogs.windows.com/buildingapps/">Building Apps for Windows</a></td>
    </tr>
    <tr>
        <td>Windows10 (Blogbeiträge)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Beiträge in Windows10</a></td>
    </tr>
    <tr>
        <td>Blog des VisualStudio-Entwicklerteams</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">The Visual Studio Blog</a></td>
    </tr>
    <tr>
        <td>Blogs zu VisualStudio-Entwicklertools</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">Developer Tools Blogs</a></td>
    </tr>
    <tr>
        <td>Somasegars-Blog zu Entwicklertools</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Somasegar’s blog</a></td>
    </tr>
    <tr>
        <td>DirectX-Entwicklerblog</td>
        <td><a href="http://blogs.msdn.com/b/directx">DirectX Developer Blog</a></td>
    </tr>
    <tr>
        <td>Einführung in DirectX12 (Blogbeitrag)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX12</a></td>
    </tr>
    <tr>
        <td>Teamblog zu VisualC++-Tools</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Teamblog zu Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog des PIX-Teams</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">Leistungsoptimierung und -Debuggen von DirectX12-Spielen für Windows und Xbox</a></td>
    </tr>
    <tr>
        <td>Universelle Windows-App – Blog des Bereitstellungsteams</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Erstellen und Bereitstellen von UWP-Apps – Teamblog</a></td>
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
 

Diese drei GDC2015-Videos vermitteln einen guten Überblick über die Entwicklung von Windows10-Spielen und das Spielerlebnis unter Windows10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Entwicklung von Windows10-Spielen (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Entwickeln von Spielen für Windows10</a></td>
    </tr>
    <tr>
        <td>Spielerlebnis unter Windows10 (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Spielerfahrung von Endbenutzern unter Windows10</a></td>
    </tr>
    <tr>
        <td>Übergreifendes Spielen im gesamten Microsoft-Ökosystem (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">Die Zukunft des Spielens im gesamten Microsoft-Ökosystem</a></td>
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
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">Barrierefreiheit von Spielen</a></td>
    </tr>
    <tr>
        <td>Erstellen von Spielen mit der Cloud</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">Cloud für Spiele</a></td>
    </tr>
    <tr>
        <td>Monetisierung eines Spiels</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">Monetarisierung von Spielen</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Auswählen der Grafiktechnologie und Programmiersprache

Für Windows10-Spiele stehen verschiedene Programmiersprachen und Grafiktechnologien zur Verfügung. Der jeweilige Ansatz richtet sich nach der Art des Spiels, das Sie entwickeln, der Erfahrung und den Vorlieben Ihres Entwicklungsstudios und den bestimmten Funktionsanforderungen Ihres Spiels. Möchten Sie C#, C++ oder JavaScript verwenden? DirectX, XAML oder HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX ist die richtige Wahl für 2D/3D-Grafik und Multimediaelemente.

DirectX 12 ist schneller und effizienter als alle früheren Versionen von Direct3D. Direct3D12 ermöglicht detaillierte Umgebungen, mehr Objekte, komplexere Effekte und eine optimale Nutzung moderner GPU-Hardware auf Windows 10 PCs und Xbox One.

Sie können weiterhin die vertraute Grafikpipeline von Direct3D11 verwenden und gleichzeitig von den neuen Rendering- und Optimierungsfeatures profitieren, die in Direct3D11.3 hinzugekommen sind. Und wenn Sie ein richtiger Windows-API-Entwickler für den Desktop mit Win32-Erfahrung sind, steht Ihnen unter Windows10 auch diese Option zur Verfügung.

Dank umfangreicher Features und einer umfassenden Plattformintegration lassen sich mit DirectX selbst anspruchsvollste Spiele realisieren.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX für die UWP-Entwicklung</td>
        <td><a href="directx-programming.md">DirectX-Programmierung</a></td>
    </tr>
    <tr>
        <td>Lernprogramm: Erstellen eines UWP-DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D12-Programmieranleitung und -referenz</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D12-Grafiken</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX 12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Informationen zu Microsoft DirectX12 und Grafiken</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">XAML-Plattform</a></td>
    </tr>
    <tr>
        <td>XAML-UI und -Steuerelemente</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">Steuerelemente, Layouts und Text</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML5

Die HyperText Markup Language (HTML) ist eine häufig verwendete Markup-Sprache für Benutzeroberflächen, die bei Webseiten, Apps und Rich-Clients zum Einsatz kommt. Für Windows-Spiele kann HTML5 als Darstellungsschicht mit vollem Funktionsumfang genutzt werden. Dabei stehen die vertrauten Features von HTML, Zugriff auf die universelle Windows-Plattform und Unterstützung für moderne Webfeatures wie AppCache, Web-Worker, Canvas, Drag&Drop, asynchrone Programmierung und SVG zur Verfügung. Im Hintergrund wird für das HTML-Rendering die leistungsstarke DirectX-Hardwarebeschleunigung genutzt, sodass Sie weiterhin in den Genuss der Leistungsvorteile von DirectX kommen, ohne zusätzlichen Code schreiben zu müssen. HTML5 ist eine gute Wahl, wenn Sie sich mit der Webentwicklung auskennen, ein Webspiel portieren oder Sprach- und Grafikebenen nutzen möchten, die unter Umständen leichter zugänglich als andere Optionen sind. HTML5 wird zusammen mit JavaScript verwendet, kann aber auch mit Komponenten verknüpft werden, die mit C# oder C++/CX erstellt wurden.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informationen zu HTML5 und zum Dokumentobjektmodell</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">HTML- und DOM-Referenz</a></td>
    </tr>
    <tr>
        <td>Die HTML5-Empfehlung des W3C</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilität von DirectX und XAML</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX ist eine effiziente Hochleistungssprache und bietet eine erstklassige Mischung aus Geschwindigkeit, Kompatibilität und Plattformzugriff. C++/CX erleichtert Ihnen die Nutzung aller nützlichen Gaming-Features unter Windows10, z.B. DirectX und Xbox Live. Außerdem können Sie vorhandenen C++-Code und die dazugehörigen Bibliotheken verwenden. Mit C++/CX wird schneller, systemeigener Code erstellt, bei dem kein Aufwand für die Garbage Collection anfällt. So kann Ihr Spiel mit einer hohen Leistung und einem geringen Stromverbrauch aufwarten und somit auch eine längere Akkulaufzeit ermöglichen. Verwenden Sie C++/CX mit DirectX oder XAML, oder erstellen Sie ein Spiel mit einer Kombination aus beidem.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referenz und Übersichten für C++/CX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Visual C++-Programmiersprachenreferenz (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++-Programmieranleitung und -Referenz</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ in Visual Studio 2017</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">C#-Programmiersprachenreferenz</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">JavaScript-Referenz</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Kombinieren von Programmiersprachen mithilfe von Windows-Runtime-Komponenten

Mit der universellen Windows-Plattform lassen sich problemlos Komponenten in unterschiedlichen Programmiersprachen kombinieren. Erstellen Sie Komponenten für Windows-Runtime in C++, C# oder Visual Basic, und nutzen Sie diese dann per JavaScript, C#, C++ oder Visual Basic. Dies ist eine hervorragende Möglichkeit, wenn Sie Teile des Spiels in der Sprache Ihrer Wahl programmieren möchten. Über Komponenten können Sie außerdem externe Bibliotheken nutzen, die nur in einer bestimmten Programmiersprache verfügbar sind, oder auch älteren Code, den Sie bereits geschrieben haben.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>So wird's gemacht: Erstellen von Komponenten für die Windows-Runtime</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Erstellen von Windows-Runtime-Komponenten</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>Welche DirectX-Version sollte Ihr Spiel verwenden?

Wenn Sie DirectX für Ihr Spiel auswählen, müssen Sie entscheiden, welche Version Sie verwenden: Microsoft Direct3D12 oder Microsoft Direct3D11.

DirectX 12 ist schneller und effizienter als alle früheren Versionen von Direct3D. Direct3D12 ermöglicht detaillierte Umgebungen, mehr Objekte, komplexere Effekte und eine optimale Nutzung moderner GPU-Hardware auf Windows 10 PCs und Xbox One. Da Direct3D12 auf einer sehr niedrigen Ebene ausgeführt wird, erhält ein erfahrenes Grafikentwicklungs- oder DirectX11-Entwicklungsteam alle notwendigen Steuerungsmöglichkeiten für die Maximierung der Grafikoptimierung.

Direct3D11.3 ist eine Grafik-API auf einem niedrigen Niveau, die das vertraute Direct3D-Programmiermodell verwendet und Ihnen einen größeren Teil der Komplexität abnimmt, die mit dem GPU-Rendering verbunden ist. Sie wird auch von Windows10 und Xbox One unterstützt. Wenn Sie über ein vorhandenes Modul verfügen, das in Direct3D 11 geschrieben wurde, und noch nicht bereit sind, zu Direct3D12 zu wechseln, können Sie Direct3D11 auf 12 verwenden, um einige Leistungsverbesserungen zu erzielen. Die Versionen ab 11.3 enthalten die neuen Rendering- und Optimierungsfeatures, die auch in Direct3D12 zur Verfügung stehen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Auswählen von Direct3D12 oder Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">Was ist Direct3D12?</a></td>
    </tr>
    <tr>
        <td>Übersicht über Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D11-Grafik</a></td>
    </tr>
    <tr>
        <td>Übersicht über „Direct3D 11 on 12“</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">Direct3D 11 on 12</a></td>
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
        <td><a href="https://dev.windows.com/bridges/">Portieren Ihres Codes für Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für iOS</td>
        <td><a href="https://dev.windows.com/bridges/ios">Portieren Ihrer iOS-Apps für Windows</a></td>
    </tr>
    <tr>
        <td>Windows-Brücke für Desktop-Anwendungen (.NET und Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Konvertieren Ihrer Desktopanwendung in eine UWP-App</a></td>
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
        <td><a href="https://playfab.com/">Übersicht über Tools und Dienste</a></td>
    </tr>
    <tr>
        <td>Erste Schritte</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Generelles Handbuch über erste Schritte</a></td>
    </tr>
    <tr>
        <td>Video-Lernprogrammserie</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Serie von Demovideos über Kernsysteme von PlayFab</a></td>
    </tr>
    <tr>
        <td>Rezepte</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Beliebte Spielmechanismen und Designmusterbeispiele</a></td>
    </tr>
    <tr>
        <td>Plattformen</td>
        <td><a href="https://api.playfab.com/platforms">Spezielle Dokumentation für verschiedene Plattformen und Spielengines</a></td>
    </tr>
    <tr>
        <td>GitHub-Repository</td>
        <td><a href="https://github.com/PlayFab">Hier erhalten Sie Skripts und SDKs für verschiedene Plattformen, einschließlich Android, iOS, Windows, Unity und Unreal.</a></td>
    </tr>
    <tr>
        <td>API-Dokumentation</td>
        <td><a href="https://api.playfab.com/documentation/">PlayFab-Dienst direkt über REST-ähnlichen Web-APIs</a></td>
    </tr>
    <tr>
        <td>Foren</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab-Foren</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity bietet eine Plattform zum Erstellen ansprechender 2D, 3D, VR, und AR-Spiele und Apps. Es ermöglicht Ihnen, Ihre kreative Vision schnell umzusetzen und Ihre Inhalte auf nahezu alle Medien oder Geräten anzubieten.

Unity unterstützt ab Unity5.4 die Direct3D12-Entwicklung.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Die Unity-Spielengine</td>
        <td><a href="http://unity3d.com/">Unity – Spielengine</a></td>
    </tr>
    <tr>
        <td>Unity herunterladen</td>
        <td><a href="http://unity3d.com/get-unity">Unity herunterladen</a></td>
    </tr>
    <tr>
        <td>Unity-Dokumentation für Windows</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Unity-Handbuch/Windows</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Erste Schritte – führen Sie Ihren ersten PlayFab-API-Aufruf aus Ihrem Unity-Spiel durch</a></td>
    </tr>
    <tr>
        <td>Wie fügen Sie Ihrem Spiel mit dem interaktiven Mixer Interaktivität hinzu</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Leitfaden für erste Schritte</a></td>
    </tr>
    <tr>
        <td>Mixer SDK für Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Mixer Unity-Plug-In</a></td>
    </tr>
    <tr>
        <td>Vollständige Referenzdokumentation für Mixer SDK für Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">API-Referenz für das Mixer Unity-Plug-In</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen eines Unity-Spiels im Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Portierungsleitfaden</a></td>
    </tr>
    <tr>
        <td>Problembehandlung bei fehlenden Assemblyverweisen im Zusammenhang mit .NET APIs</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Fehlende .NET-APIs in Unity und UWP</a></td>
    </tr>
    <tr>
        <td>Veröffentlichen eines Unity-Spiels als UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">So veröffentlichen Sie Ihr Unity-Spiel als UWP-App</a></td>
    </tr>
    <tr>
        <td>Verwenden von Unity zum Erstellen von Windows-Spielen und -Apps (Video)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Erstellen von Windows-Spielen und -Apps mit Unity</a></td>
    </tr>
    <tr>
        <td>Unity-Spielentwicklung mit Visual Studio (Videoserie)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">Verwendung von Unity mit Visual Studio 2015</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Mit den Tools und Technologien aus der modular aufgebauten Suite von Havok erreichen Spieleentwickler eine noch nie dagewesene Interaktivität und Immersion. Havok bietet eine äußerst realistische Physik, interaktive Simulationen und beeindruckende Effekte. Version 2015.1 und höher unterstützen UWP in Visual Studio2015 auf x86-, 64-Bit- und ARM-Plattformen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok-Website</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Havok-Toolsuite</td>
        <td><a href="http://www.havok.com/products/">Havok-Produktübersicht</a></td>
    </tr>
    <tr>
        <td>Havok-Supportforen</td>
        <td><a href="http://support.havok.com">Havok</a></td>
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
        <td><a href="http://www.monogame.net">MonoGame-Website</a></td>
    </tr>
    <tr>
        <td>MonoGame-Dokumentation</td>
        <td><a href="http://www.monogame.net/documentation/">MonoGame-Dokumentation (aktuell)</a></td>
    </tr>
    <tr>
        <td>MonoGame-Downloads</td>
        <td><a href="http://www.monogame.net/downloads/">Laden Sie Versionen, Entwicklungsbuilds und Quellcode</a> von der MonoGame-Website herunter, oder <a href="https://www.nuget.org/profiles/MonoGame">rufen Sie die neueste Version über NuGet ab</a>.
    </tr>
    <tr>
        <td>Beispiel für ein MonoGame 2D UWP-Spiel</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Erstellen eines UWP-Spiels in MonoGame-2D</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-X ist eine plattformübergreifende Suite mit Open-Source-Spieleentwicklungsengine und Tools, die die Erstellung von UWP-Spielen unterstützt. Ab Version3 werden auch 3D-Features hinzugefügt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-X</td>
        <td><a href="http://www.cocos2d-x.org/">Was ist Cocos2d-X?</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X-Programmieranleitung</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Cocos2d-X-Programmieranleitung</a></td>
    </tr>
    <tr>
        <td>Cocos2d-X unter Windows10 (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Ausführen von Cocos2d-X unter Windows10</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Erste Schritte – führen Sie Ihren ersten PlayFab-API-Aufruf aus Ihrem Cocos2d-Spiel durch</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine4 ist eine umfassende, für alle Arten von Spielen und Entwicklern geeignete Suite mit Tools für die Spieleentwicklung. Die Unreal Engine wird von Spieleentwicklern auf der ganzen Welt für besonders anspruchsvolle Konsolen- und PC-Spiele eingesetzt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Übersicht über die Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine4</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Erste Schritte – führen Sie Ihren ersten PlayFab-API-Aufruf aus Ihrem Unreal-Spiel durch</a></td>
    </tr>
    <tr>
        <td>Fügen Sie mit PlayFab LiveOps hinzu - Entwürfe</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Erste Schritte – führen Sie Ihren ersten PlayFab-API-Aufruf aus Ihrem Unreal-Spiel durch</a></td>
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
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
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
        <td>Portieren einer Windows8-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">Wechsel von Windows-Runtime 8.x zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Windows 8-App zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Portieren von Windows 8.1-Apps zu Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren einer iOS-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">Wechsel von iOS zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren einer Silverlight-App zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">Wechsel von Windows Phone Silverlight zu UWP</a></td>
    </tr>
    <tr>
        <td>Portieren von XAML oder Silverlight zu einer UWP-App (Universelle Windows-Plattform) (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Portieren einer App von XAML oder Silverlight zu Windows 10</a></td>
    </tr>
    <tr>
        <td>Portieren eines Xbox-Spiels zu einer UWP-App (Universelle Windows-Plattform)</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Portieren von Xbox One zu Windows 10 (UWP)</a></td>
    </tr>
    <tr>
        <td>Portieren von DirectX 9 zu DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Portieren von DirectX9 zur universellen Windows-Plattform (UWP)</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D11 zu Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">Portieren von Direct3D11 zu Direct3D12</a></td>
    </tr>
    <tr>
        <td>Portieren von OpenGLES zu Direct3D11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Portieren von OpenGLES2.0 zu Direct3D11</a></td>
    </tr>
    <tr>
        <td>OpenGLES2.0 zu Direct3D11 mit ANGLE</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>Entsprechungen für die klassische Windows-API in der UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">Alternativen zu Windows-APIs in Apps für die Universelle Windows-Plattform (UWP)</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Prototyp und Design


Nachdem Sie sich entschieden haben, welche Art von Spiel Sie entwickeln und welche Tools und Grafiktechnologie Sie dabei verwenden möchten, können Sie sich der Gestaltung zuwenden und einen Prototyp entwickeln. Da es sich bei Ihrem Spiel im Grunde um eine UWP-App (Universelle Windows-Plattform) handelt, beginnen Sie dort.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Einführung in die universelle Windows-Plattform (UWP)

Windows10 führt die universelle Windows-Plattform (UWP) ein. Diese stellt eine gemeinsame, übergreifende API-Plattform für Windows10-Geräte bereit. Bei der UWP handelt es sich um eine Weiterentwicklung und Erweiterung des Windows-Runtime-Modells zu einem geschlossenen, einheitlichen Kern. Für die UWP entwickelte Spiele können WinRT-APIs aufrufen, die bei allen Geräten vorhanden sind. Da die UWP eine garantierte API-Ebene bereitstellt, können Sie ein einzelnes App-Paket erstellen, das dann auf allen Windows10-Geräten installiert werden kann. Bei Bedarf kann Ihr Spiel natürlich auch weiterhin spezifische APIs der Geräte aufrufen, auf denen das Spiel ausgeführt wird – etwa einige klassische Windows-APIs von Win32 und .NET.

Im Anschluss finden Sie praktische Handbücher, die sich ausführlich mit UWP-Apps (Universelle Windows-Plattform) auseinandersetzen und hilfreiche Erkenntnisse zur Plattform liefern.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einführung in UWP-Apps (Universelle Windows-Plattform)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">Was ist eine UWP-App (Universelle Windows-Plattform)?</a></td>
    </tr>
    <tr>
        <td>Übersicht über die UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">Anleitung für UWP-Apps</a></td>
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
        <td><a href="https://dev.windows.com/getstarted">Erste Schritte mit Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Vorbereitung auf die UWP-Entwicklung</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">Vorbereitung</a></td>
    </tr>
</table>

Wenn Sie noch keine Erfahrungen mit der UWP-Programmierung haben und die Verwendung von XAML in Ihrem Spiel in Betracht ziehen (siehe [Auswählen von Grafiktechnologie und Programmiersprache](#choosing-your-graphics-technology-and-programming-language)), ist die Videoserie [Windows10-Entwicklung für Neueinsteiger](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) ein guter Ausgangspunkt.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einsteigerhandbuch für die Windows10-Entwicklung mit XAML (Videoserie)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Windows10-Entwicklung für Neueinsteiger</a></td>
    </tr>
    <tr>
        <td>Ankündigung der Windows10-Neueinsteigerserie mit XAML (Blogbeitrag)</td>
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Windows 10-Entwicklung für Neueinsteiger</a></td>
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
        <td><a href="https://dev.windows.com/develop">Entwickeln von Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Übersicht über die Netzwerkprogrammierung der UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">Netzwerk und Webdienste</a></td>
    </tr>
    <tr>
        <td>Verwenden von „Windows.Web.HTTP“ und „Windows.Networking.Sockets“ in Spielen</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Netzwerkfunktionen für Spiele</a></td>
    </tr>
    <tr>
        <td>Asynchrone Programmierkonzepte der UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">Asynchrone Programmierung</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows Desktop APIsto UWP

Hier finden Sie einige Links, die Sie beim Wechsel von Windows-Desktop-Spielen zu UWP-Spielen unterstützen.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Verwenden Sie vorhandenen C++-Code für die UWP-Spieleentwicklung</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Vorgehensweise: Verwenden von vorhandenem C++-Code in einer UWP-App</a></td>
    </tr>
    <tr>
        <td>UWP-APIs für Win32- und COM-APIs</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">Win32- und COM-APIs für UWP-Apps</a></td>
    </tr>
    <tr>
        <td>Nicht unterstützte CRT-Funktionen in UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">Nicht unterstützte CRT-Funktionen in Apps für die universelle Windows-Plattform</a></td>
    </tr>
    <tr>
        <td>Alternativen zu Windows-APIs</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">Alternativen zu Windows-APIs in Apps für die universelle Windows-Plattform (UWP)</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">App-Lebenszyklus</a></td>
    </tr>
    <tr>
        <td>Auslösen von App-Übergängen mithilfe von Microsoft Visual Studio</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">So wird's gemacht: Auslösen von Anhalte-, Fortsetzungs- und Hintergrundereignissen für UWP-Apps in Visual Studio</a></td>
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
        <td><a href="https://dev.windows.com/design">Gestalten von UWP-Apps</a></td>
    </tr>
    <tr>
        <td>Gestalten für App-Lebenszykluszustände</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">UX-Richtlinien für das Starten, Anhalten und Fortsetzen von Apps</a></td>
    </tr>
    <tr>
        <td>Entwerfen Sie Ihre UWP-App für Xbox One und Fernsehgeräte</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Entwerfen für Xbox und Fernsehgeräte</a></td>
    </tr>
    <tr>
        <td>Ausrichten auf verschiedene Geräteformfaktoren (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Entwerfen von Spielen für eine WindowsCore-Welt</a></td>
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
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">Bewährte Methoden: Typografie</a></td>
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
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">Bewährte Methoden: UI-Zuordnung</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">Programmieranleitung und API-Referenz für XAudio2</a></td>
    </tr>
    <tr>
        <td>Plattformübergreifende Audioverarbeitungsobjekte erstellen</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">XAPO: Übersicht</a></td>
    </tr>
    <tr>
        <td>Einführung in Audio-Konzepte</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio für Spiele</a></td>
    </tr>
    <tr>
        <td>Übersicht über Windows Sonic</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">Raumklang</a></td>
    </tr>
    <tr>
        <td>Windows-Sonic Raumklang - Beispiele</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Xbox Advanced Technology Group – Audiobeispiele</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie Windows Sonic in Ihre Spiele (Video) integrieren</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Einführung in die räumliche Audiowiedergabe Funktionen für Xbox oder Windows</a></td>
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
        <td><a href="directx-programming.md">DirectX-Programmierung</a></td>
    </tr>
    <tr>
        <td>Lernprogramm: Erstellen eines UWP-DirectX-Spiels</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Erstellen eines einfachen UWP-Spiels mit DirectX</a></td>
    </tr>
    <tr>
        <td>DirectX-Interaktionen mit dem UWP-App-Modell</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Das App-Objekt und DirectX</a></td>
    </tr>
    <tr>
        <td>Videos zu Grafiken und zur DirectX12-Entwicklung (YouTube-Kanal)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Informationen zu Microsoft DirectX12 und Grafiken</a></td>
    </tr>
    <tr>
        <td>Übersichten und Referenzen zu DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX-Grafiken und -Spiele</a></td>
    </tr>
    <tr>
        <td>Direct3D12-Programmieranleitung und -referenz</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D12-Grafiken</a></td>
    </tr>
    <tr>
        <td>DirectX12-Grundlagen (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Bessere Leistung und Performance: Ihr Spiel unter DirectX12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Erlernen von Direct3D2

Erfahren Sie mehr über die Änderungen in Direct3D12 und wie Sie mit der Programmierung in Direct3D12 beginnen können. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Einrichten der Programmierumgebung</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Einrichtung der Direct3D12 Programmierungsumgebung</a></td>
    </tr>
    <tr>
        <td>Erstellen einer Grundkomponente</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">Erstellen einer einfachen Direct3D12-Komponente</a></td>
    </tr>
    <tr>
        <td>Änderungen in Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">Wichtige Änderungen bei der Migration von Direct3D11 zu Direct3D12</a></td>
    </tr>
    <tr>
        <td>Portieren von Direct3D11 zu Direct3D12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">Portieren von Direct3D11 zu Direct3D12</a></td>
    </tr>
    <tr>
        <td>Konzepte für die Ressourcenbindung (deckt Deskriptor, Deskriptortabelle, Deskriptorheap und Stammsignatur ab) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Ressourcenbindung in Direct3D12</a></td>
    </tr>
    <tr>
        <td>Verwalten des Arbeitsspeichers</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Arbeitsspeicherverwaltung in Direct3D12</a></td>
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
        <td>DirectX-Toolkit für DirectX11 herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>DirectX-Toolkit für DirectX12 herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK12</a></td>
    </tr>
    <tr>
        <td>DirectX-Texturverarbeitungsbibliothek herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>DirectXMesh-Geometrieverarbeitungsbibliothek herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>UVAtlas zum Erstellen und Verpacken des isoChart-Texturatlas herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>DirectXMath-Bibliothek herunterladen</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Direct3D12-Unterstützung im DirectXTK (Blogbeitrag)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Unterstützung für DirectX12</a></td>
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
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX12 auf Nvidia-GPUs</a></td>
    </tr>
    <tr>
        <td>Intel: Effizientes Rendering mit DirectX12</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">DirectX12-Rendering auf Intel-Grafikkarten</a></td>
    </tr>
    <tr>
        <td>Intel: Mehrfachadapterunterstützung in DirectX12</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Implementieren einer expliziten Mehrfachadapteranwendung mit DirectX12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX12-Lernprogramm</td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">Kacheln, Badges und Benachrichtigungen</a></td>
    </tr>
    <tr>
        <td>Beispiel zur Veranschaulichung von Live-Kacheln und Benachrichtigungen</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Benachrichtigungsbeispiel</a></td>
    </tr>
    <tr>
        <td>Vorlagen für adaptive Kacheln (Blogbeitrag)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">Vorlagen für adaptive Kacheln – Schema und Dokumentation</a></td>
    </tr>
    <tr>
        <td>Gestalten von Kacheln und Signalen</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">Richtlinien für Kacheln und Signale</a></td>
    </tr>
    <tr>
        <td>Windows10-App für die interaktive Entwicklung von Vorlagen für Live-Kacheln</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">Tool zum Erstellen aller erforderlichen Kacheln über ein einziges Image</a></td>
    </tr>
    <tr>
        <td>UWP-Erweiterung für die Generierung von Kacheln für Visual Studio (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Tipps zum Verwenden des UWP-Tools für die Generierung von Kacheln</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-iap-purchases"></a>Unterstützen von In-App-Produktkäufen (IAP-Käufen)

Bei einem IAP (In-App-Produkt) handelt es sich um einen zusätzlichen Artikel, den Spieler innerhalb des Spiels erwerben können. Beispiele für IAPs sind Add-Ons, Spielelevels, Gegenstände und alles andere, was für die Spieler interessant sein könnte. Bei richtiger Anwendung können IAPs Umsätze generieren und gleichzeitig das Spielerlebnis verbessern. Die IAPs Ihres Spiels werden über das WindowsDevCenter-Dashboard definiert und veröffentlicht. Die Aktivierung von In-App-Käufen erfolgt über den Code Ihres Spiels.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Langlebige In-App-Produkte</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">Unterstützen von Käufen von In-App-Produkten</a></td>
    </tr>
    <tr>
        <td>In-App-Verbrauchsprodukte</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">Käufe von konsumierbaren In-App-Produkten aktivieren</a></td>
    </tr>
    <tr>
        <td>Details zu und Einreichung von In-App-Produkten</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">IAP-Übermittlungen</a></td>
    </tr>
    <tr>
        <td>Überwachen von IAP-Verkauf und Demografie für Ihr Spiel</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">Bericht zu IAP-Käufen</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>Debuggen, Leistungsoptimierung und Überwachung

Zur Optimierung der Leistung, nutzen Sie den Spielmodus in Windows10, um Ihren Spielern ein optimales Erlebnis durch die vollständige Nutzung der Kapazität ihrer aktuellen Hardware zu gewährleisten.

Das Windows Performance Toolkit (WPT) besteht aus Leistungsüberwachungstools, die detaillierte Leistungsprofile von Windows-Betriebssystemen und -Anwendungen erstellen. Dies ist besonders hilfreich für die Überwachung der Speicherverwendung und zum Verbessern der Leistung eines Spiels. Das Windows Performance Toolkit ist im SDK für Windows 10 und im Windows ADK enthalten. Dieses Toolkit besteht aus zwei unabhängigen Tools: Windows Performance Recorder (WPR) und Windows Performance Analyzer (WPA). ProcDump ist Teil von [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default) und ist ein Befehlszeilenprogramm, das die Leistungsspitzen der CPU überwacht und Speicherabbilddateien während der Spielabstürze generiert. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Leistungstest für Ihren Code</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">Cloud-basierte Auslastungstests</a></td>
    </tr>
    <tr>
        <td>Erhalten Sie Xbox-Konsolentypen mithilfe der Geräteinformationen für Spiele</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">Gaming-Geräteinformationen</a></td>
    </tr>
    <tr>
        <td>Verbessern Sie die Leistung durch einen exklusiven oder prioritären Zugriff auf Hardwareressourcen mithilfe der Spielmodus-APIs</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">Spielmodus</a></td>
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
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Analyse des kritischen Pfads in WPA</a></td>
    </tr>
    <tr>
        <td>Diagnostizieren von Speicherverwendung und Arbeitsspeicherverlusten mit Windows Performance Recorder (Video)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Speicherbedarf und Arbeitsspeicherverluste</a></td>
    </tr>
    <tr>
        <td>Abrufen von ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
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
        <td>PIX für Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">Leistungsoptimierungs- und -Debugg-Tools von DirectX12 für Windows</a></td>
    </tr>
    <tr>
        <td>Überprüfungs- und Debugg-Tools für die Entwicklung von D3D12 (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12-Leistung Leistungsoptimierung und-Debuggen mit PIX und GPUValidation</a></td>
    </tr>
    <tr>
        <td>Grafik- und Leistungsoptimierung (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Erweiterte DirectX12-Grafiken und -Leistung</a></td>
    </tr>
    <tr>
        <td>Debuggen von DirectX-Grafiken (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Lösen Sie komplexe Grafikprobleme in Ihrem Spiel mithilfe von DirectX-Tools.</a></td>
    </tr>
    <tr>
        <td>Visual Studio2015-Tools zum Debuggen von DirectX12 (Video)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">DirectX-Tools für Windows10 in Visual Studio2015</a></td>
    </tr>
    <tr>
        <td>Direct3D12-Programmieranleitung</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D12-Programmieranleitung</a></td>
    </tr>
    <tr>
        <td>Kombinieren von DirectX und XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilität von DirectX und XAML</a></td>
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
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Verbessern von HDR und erweiterte Farbe in DirectX</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie HDR-Inhalt darstellen, und ermitteln Sie, ob die aktuelle Anzeige dies unterstützt</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR-Beispiel</a></td>
    </tr>
    <tr>
        <td>Erstellen Sie und konfigurieren Sie eine erweiterte Farbe mit DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Beispiel der erweiterten Farbe in Direct2D für das Rendering von Bildern</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">Richtlinien für die Entwicklung für eine weltweite Zielgruppe</a></td>
    </tr>
    <tr>
        <td>Überwinden sprachlicher, kultureller und technologischer Barrieren</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">Onlineressource für Sprachkonventionen und Microsoft-Standardterminologie</a></td>
    </tr>
</table>

### <a name="security"></a>Sicherheit

Erstellen einer Umgebung, in der Spieler spielen und fair konkurrieren. Ein Spiel, das mit TruePlay registriert wird, wird in einem geschützten Prozess ausgeführt, das eine Klasse von häufigen Angriffen verringert. Das Überwachungssystem der Spiele hilft Ihnen, allgemeine Täuschungsszenarien zu ermitteln. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Tools zur Bekämpfung von Täuschungen innerhalb von PC-Spielen</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Übermitteln und Veröffentlichen Ihres Spiels

Die folgenden Handbücher und Informationen sorgen für eine möglichst reibungslose Veröffentlichung und Übermittlung.

### <a name="publishing"></a>Publishing

Spielpakete werden über das neue einheitliche Windows Dev Center-Dashboard veröffentlicht und verwaltet.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>App-Veröffentlichung mit Windows Dev Center</td>
        <td><a href="https://dev.windows.com/publish">Veröffentlichen von Windows-Apps</a></td>
    </tr>
    <tr>
        <td>Erweiterte Veröffentlichung (GDN) über das Windows Dev Center</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">Handbuch zur erweiterten Veröffentlichung über das Windows Dev Center-Dashboard</a></td>
    </tr>
    <tr>
        <td>Sie können mit Azure Active Directory (AAD) Ihrem Dev Center-Konto Benutzer hinzufügen.</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">Verwalten von Kontobenutzern</a></td>
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
        <td>Informationen zur Verwendung der Streaming-Installation und optionale Pakete (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Nextgen UWP-app-Verteilung: Erstellen erweiterbare, Stream-fähig Componentizedapps</a></td>
    </tr>
    <tr>
        <td>Unterteilen und Gruppieren zum Aktivieren von Inhalten für die Streaming-Installation</td>
        <td><a href="../packaging/streaming-install.md">UWP-App-Streaming installieren</a></td>
    </tr>
    <tr>
        <td>Optionale Pakete wie DLC-Spielinhalte erstellen</td>
        <td><a href="../packaging/optional-packages.md">Optionale Pakete und die Erstellung zugehöriger Sets</a></td>
    </tr>
    <tr>
        <td>Packen Ihres UWP-Spiels</td>
        <td><a href="../packaging/index.md">Verpacken von Apps</a></td>
    </tr>
    <tr>
        <td>Packen Ihres UWP-DirectX-Spiels</td>
        <td><a href="package-your-windows-store-directx-game.md">Packen Ihres UWP-DirectX-Spiels</a></td>
    </tr>
    <tr>
        <td>Packen Ihres Spiels als Drittentwickler (Blogbeitrag)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Erstellen von hochladbaren Paketen ohne Zugriff auf das Store-Konto des Veröffentlichers</a></td>
    </tr>
    <tr>
        <td>Erstellen von App-Paketen und App-Paketbündeln mit MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">Erstellen von Paketen mit dem App-Verpackungsprogramm MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Digitales Signieren Ihrer Dateien mithilfe von SignTool</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">Signieren von Dateien und Überprüfen von Signaturen in Dateien mithilfe von SignTool</a></td>
    </tr>    
    <tr>
        <td>Hochladen und Verwalten der Versionen Ihres Spiels</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">Hochladen von App-Paketen</a></td>
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
        <td>Vereinbarung für Entwickler von MicrosoftStore-Apps</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">Vereinbarung für App-Entwickler</a></td>
    </tr>
    <tr>
        <td>Richtlinien für die Veröffentlichung von Apps im MicrosoftStore</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Microsoft Store-Richtlinien</a></td>
    </tr>
    <tr>
        <td>So wird's gemacht: Vermeiden allgemeiner Probleme bei der App-Zertifizierung</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">Vermeiden allgemeiner Zertifizierungsfehler</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">StoreManifest-Schema (Windows10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Spiellebenszyklusverwaltung


Wer glaubt, sich nach dem Abschluss der Entwicklung und der Auslieferung eines Spiels entspannt zurücklehnen zu können, irrt: Die Entwicklung von Version1 mag zwar abgeschlossen sein, die Marktphase Ihres Spiels hat jedoch gerade erst begonnen. Sie sollten Verwendung und Fehlerberichte überwachen, auf Benutzerfeedback reagieren und Updates für Ihr Spiel veröffentlichen.

### <a name="windows-dev-center-analytics-and-promotion"></a>Windows Dev Center-Analysen und Werbung

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DevCenter-App</td>
        <td><a href="https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws">Dev Center-App unter Windows10 zum Anzeigen Ihrer veröffentlichten Apps</a></td>
    </tr>  
    <tr>
        <td>Windows Dev Center-Analysen</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">Analysieren der App-Leistung</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Ihre Kunden mit der Xbox-Features in Ihrem Spiel interagieren</td>
        <td><a href="../publish/xbox-analytics-report.md">Bericht der Xbox Analyse</a></td>
    </tr>
    <tr>
        <td>Reagieren auf Kundenrezensionen</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">Reagieren auf Kundenrezensionen</a></td>
    </tr>
    <tr>
        <td>Werbemöglichkeiten für Ihr Spiel</td>
        <td><a href="https://dev.windows.com/store-promotion">Bewerben Ihrer Apps</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights bietet Leistungs-, Telemetrie- und Verwendungsanalysen für Ihr veröffentlichtes Spiel. Application Insights unterstützt Sie nach der Veröffentlichung Ihres Spiels beim Erkennen und Beheben von Problemen sowie bei der kontinuierlichen Überwachung und Optimierung der Verwendung und beim Nachvollziehen der weiteren Spielerinteraktionen mit Ihrem Spiel. Application Insights fügt Ihrer App ein SDK hinzu, das Telemetriedaten an das [Azure-Portal](http://portal.azure.com/) übermittelt.

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
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Application Insights für Windows Phone- und Windows Store-Apps</a></td>
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
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Verbinden Sie Ihr UWP-Spiel mit Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Holen Sie sich Windows SDK für Google Analytics</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie Windows SDK für Google Analytics (Video) verwenden</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Erste Schritte mit Windows SDK für Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Verwenden der Facebook-App zum Installieren von Anzeigen, bietet Werbemöglichkeiten für Ihr Spiel für Facebook-Benutzer an</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Holen Sie sich Windows SDK für Facebook</a></td>
    </tr>
    <tr>
        <td>Hier erfahren Sie, wie Sie die Facebook-App zum Installieren von Anzeigen verwenden (Video)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Erste Schritte mit Windows SDK für Facebook</a></td>
    </tr>
    <tr>
        <td>Verwenden von Vungle, um Ihren Spielen Videoanzeigen hinzuzufügen</td>
        <td><a href="https://v.vungle.com/sdk">WindowsSDK für Vungle herunterladen</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Paketversionsnummern</a></td>
    </tr>
    <tr>
        <td>Leitfaden für die Spielpaketverwaltung</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Leitfaden für die App-Paketverwaltung</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Hinzufügen von Xbox Live zu Ihrem Spiel

Xbox Live ist ein erstklassiges Gaming-Netzwerk, das Millionen von Spielern weltweit verbindet. Entwickler haben Zugriff auf Xbox Live-Features, die die Spielerzielgruppe steigern, einschließlich Xbox Live, Bestenlisten, Cloudspeicherungen, Spielehubs, Clubs, Party-Chat, Game DVR und mehr.

> [!Note]
> Wenn Sie Xbox Live aktivierte Titel entwickeln möchten, stehen Ihnen verschiedene Optionen zur Verfügung. Informationen zu den verschiedenen Programmen finden Sie unter [Übersicht über das Entwickler-Programm](../xbox-live/developer-program-overview.md).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live – Übersicht</td>
        <td><a href="../xbox-live/index.md">Xbox Live – Entwicklerhandbuch</a></td>
    </tr>
    <tr>
        <td>Verstehen, welche Features je nach Programm verfügbar sind</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">Übersicht über das Entwickler-Programm: Tabelle der Features</a></td>
    </tr>
    <tr>
        <td>Links zu nützlichen Ressourcen für die Entwicklung von Xbox Live-Spielen</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Xbox Live-Ressourcen</a></td>
    </tr>
    <tr>
        <td>Erfahren Sie, wie Sie Informationen über Xbox Live-Dienste abrufen</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Einführung in Xbox Live APIs</a></td>
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
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Erste Schritte – Xbox Live Creators-Programm</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Schrittweise Anleitung zur Integration mit dem Xbox Live Creators-Programm</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Erste Schritte bei der Entwicklung eines Titels im Xbox Live Creators-Programm mit der Unity-Spielengine</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs-Sandbox</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Einführung in die Xbox Live-Sandbox</a></td>
    </tr>
    <tr>
        <td>Einrichten von Konten zum Testen</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Xbox Live-Konten in Ihrer Testumgebung autorisieren</a></td>
    </tr>
    <tr>
        <td>Beispiele für das Xbox Live Creators-Programm</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Codebeispiele für Creators Programm-Entwickler</a></td>
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
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Erste Schritte mit Xbox Live als verwalteter Partner oder ID-Entwickler</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem Spiel</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Schrittweise Anleitung zum Integrieren von Xbox Live für verwaltete Partner und ID Mitglieder</a></td>
    </tr>
    <tr>
        <td>Hinzufügen von Xbox Live zu Ihrem UWP-Spiel, das mit Unity erstellt wurde</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Hinzufügen von Xbox Live-Unterstützung zu Unity für UWP mit IL2CPP Scripting-Back-End für ID- und verwaltete Partner</a></td>
    </tr>
    <tr>
        <td>Einrichten der Entwicklungs-Sandbox</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Erweiterte Xbox Live-Sandbox</a></td>
    </tr>
    <tr>
        <td>Anforderungen für Spiele mit Xbox Live (GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Xbox-Anforderungen für Xbox Live unter Windows 10</a></td>
    </tr>
    <tr>
        <td>Beispiele</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Codebeispiele für ID@Xbox-Entwickler</a></td>
    </tr>  
    <tr>
        <td>Übersicht über die Entwicklung von Spielen mit Xbox Live (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Entwickeln mit Xbox Live für Windows10</a></td>
    </tr>
    <tr>
        <td>Plattformübergreifende Spielersuche (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live Multiplayer: Einführung in die Dienste für plattformübergreifende Spielersuche und Spiele</a></td>
    </tr>
    <tr>
        <td>Geräteübergreifendes Spielen in Fable Legends (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: Geräteübergreifendes Spielen mit Xbox Live</a></td>
    </tr>
    <tr>
        <td>Xbox Live und Erfolge (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Bewährte Methoden für die Nutzung von cloudbasierten Benutzerstatistiken und Erfolgen in Xbox Live</a></td>
    </tr>
</table>


## <a name="additional-resources"></a>Weitere Ressourcen

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entwicklung von Spielen (Video)</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Videos von wichtigen Konferenzen wie GDC und //Build</a></td>
    </tr>
    <tr>
        <td>Entwicklung von Indie-Spielen (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">Neue Möglichkeiten für unabhängige Entwickler</a></td>
    </tr>
    <tr>
        <td>Überlegungen für mobile Multi-Core-Geräte (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Kontinuierlich hohe Spieleleistung auf mobilen Multi-Core-Geräten</a></td>
    </tr>
    <tr>
        <td>Entwickeln von Windows10-Desktopspielen (Video)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">PC-Spiele für Windows10</a></td>
    </tr>
</table>



 

 

 
