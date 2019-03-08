---
title: Spieletechnologien für UWP-Apps
description: Dieses Handbuch enthält Informationen über verfügbare Technologien für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP).
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Technologie, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 53616e572be6051d096c095f9587c498b769a6af
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623305"
---
# <a name="game-technologies-for-uwp-apps"></a>Spieletechnologien für UWP-Apps



Dieses Handbuch enthält Informationen über verfügbare Technologien für die Entwicklung von Spielen für die Universelle Windows-Plattform (UWP).

##  <a name="benefits-of-windows10-for-game-development"></a>Vorteile von Windows 10 für die Entwicklung von Spielen


Mit der Einführung von UWP, Windows 10 werden alle von der Microsoft-Plattformen umfassen können Ihre Windows 10-Titel. Bei kostenlosen Migration von früheren Versionen von Windows besteht eine stetig steigende Zahl von Windows 10-Clients zur Verfügung. Die Kombination dieser zwei Dinge bedeutet, dass Ihre Windows 10-Titel eine große Anzahl von Kunden über den Microsoft Store zu erreichen.

Windows 10 bietet außerdem viele neue Features, die besonders nützlich für Spiele sind:

-   Reduzierte Speicherauslagerung und reduzierte Gesamtgröße des Speichersystems
-   Mit der verbesserten Grafikspeicherverwaltung wird aktiv mehr Speicher für das Spiel im Vordergrund zugeordnet und reserviert.

## <a name="uwp-games-with-c-and-directx"></a>UWP-Spiele mit C++ und DirectX


Echtzeitspiele, die hohe Leistung erfordern, sollten DirectX-APIs verwenden. DirectX ist eine Sammlung systemeigener APIs zum Erstellen von Spielen und Multimedia-Anwendungen, die hohe Leistung erfordern, z. B. 3D-Spiele.

## <a name="development-environment"></a>Entwicklungsumgebung


Um Spiele für die UWP zu erstellen, müssen Sie Ihre Entwicklungsumgebung einrichten, indem Sie die Installation von Visual Studio 2015 oder höher. Es wird empfohlen, dass Sie die neueste Version von Visual Studio, installieren Sie Zugriff auf die neuesten Updates für Entwicklung und Sicherheit zu erhalten. Visual Studio ermöglicht Ihnen die Erstellung von UWP-apps und stellt Tools für die Entwicklung von Spielen bereit:

-   Visual Studio-Tools für die DX-Spielprogrammierung – Visual Studio stellt Tools zum Erstellen, Bearbeiten, Anzeigen einer Vorschau und Exportieren von Bild-, Modell- und Shaderressourcen bereit. Außerdem sind Tools verfügbar, mit denen Sie Ressourcen zur Erstellungszeit konvertieren und DirectX-Grafikcode debuggen können. Weitere Informationen finden Sie unter [Visual Studio-Tools für die Spieleprogrammierung](set-up-visual-studio-for-game-development.md).
-   Visual Studio-Grafikdiagnosefeatures – Grafikdiagnosetools stehen nun als optionales Feature in Windows zur Verfügung. Mit den Diagnosetools können Sie Grafiken debuggen, Grafikframeanalysen ausführen und die GPU-Nutzung in Echtzeit überwachen. Weitere Informationen finden Sie unter [Tools für die Grafikdiagnose](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md).

Weitere Informationen finden Sie unter „Vorbereiten der Universellen Windows-Plattform und [DirectX-Programmierung](directx-programming.md).

## <a name="getting-started-with-directx-game-project-templates"></a>Erste Schritte mit DirectX-Spielprojektvorlagen


Nach Einrichtung der Entwicklungsumgebung können Sie eine verwandte DirectX-Projektvorlage zum Erstellen Ihres DirectX-Spiels für UWP verwenden. Visual Studio 2015 enthält drei Vorlagen für das Erstellen neuer UWP-DirectX-Projekte, **DirectX 11-App (Universelles Windows)**, **DirectX 12-App (Universelles Windows)** und **DirectX 11- und XAML-App (Universelles Windows)**. Weitere Informationen finden Sie unter [DirectX-Spielprojektvorlagen](user-interface.md).

## <a name="windows-10-apis"></a>Windows 10-APIs


Windows 10 bietet eine umfangreiche Sammlung von APIs, die für die Spieleentwicklung hilfreich sind. Es gibt APIs für praktisch alle Aspekte von Spielen: 3D- und 2D-Grafiken, Audio, Eingabe, Textressourcen, Benutzeroberfläche und Netzwerk.

Es gibt viele verwandte APIs für die Spielentwicklung, nicht für alle Spiele müssen jedoch alle APIs verwendet werden. Einige Spiele werden beispielsweise nur 3D-Grafiken und Direct3D verwenden; andere Spielen werden nur 2D-Grafiken und Direct2D verwenden; und einige Spiele werden möglicherweise beides verwenden. Im folgenden Diagramm werden die mit der Entwicklung von Spielen zusammenhängenden APIs nach Funktionalitätstyp aufgeführt.

![Technologien für die Spielplattform](images/gameplatformtechnologies.png)

-   3D-Grafiken – Windows 10 unterstützt zwei Sätze von 3D-Grafik-APIs, Direct3D 11 und [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121). Mit diesen beiden APIs können 3D- und 2D-Grafiken erstellt werden. Direct3D 11 und Direct3D 12 werden nicht zusammen verwendet, können jedoch zusammen mit APIs in der 2D-Grafik- und Benutzeroberflächengruppe verwendet werden. Weitere Informationen zum Verwenden der Grafik-APIs in Ihrem Spiel finden Sie unter [Grundlegendes zu 3D-Grafiken für DirectX-Spiele](an-introduction-to-3d-graphics-with-directx.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12 stellt die nächste Version von Direct3D vor, der 3D-Grafik-API im Herzen von DirectX. Diese Version von Direct3D ist schneller und effizienter als frühere Versionen von Direct3D. Der Kompromiss für die höhere Geschwindigkeit von Direct3D 12 besteht darin, dass die Ebene niedrigerer ist. Sie müssen Ihre Grafikressourcen selbst verwalten und über eine umfassendere Grafikprogrammiererfahrung verfügen, um die höhere Geschwindigkeit zu realisieren.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie Direct3D 12, wenn Sie die Leistung Ihres Spiels optimieren müssen und das Spiel CPU-gebunden ist.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/dn899121">Direct3D 12</a>-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11 ist die vorherige Version von Direct3D, mit der Sie 3D-Grafiken unter Verwendung einer höheren Ebene der Hardwareabstraktion als bei D3D 12 erstellen können.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie Direct3D 11, wenn Sie über vorhandenen Direct3D 11-Code verfügen, Ihr Spiel nicht CPU-gebunden ist, oder Sie davon profitieren möchten, dass Ihre Ressourcen verwaltet werden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11</a>-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   2D-Grafiken und Benutzeroberfläche – APIs für 2D-Grafiken z. B. Text und Benutzeroberflächen. Alle 2D-Grafiken und Benutzeroberflächen-APIs sind optional.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D ist eine hardwarebeschleunigte 2D-Grafik-API mit unmittelbarem Modus, die das Rendern mit hoher Leistung und in hoher Qualität für 2D-Geometrie, Bitmaps und Text bereitstellt. Die Direct2D-API baut auf Direct3D auf und ist für die Verwendung mit GDI, GDI+ und Direct3D konzipiert.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Direct2D kann anstelle von Direct3D zum Bereitstellen von Grafiken für reine 2D-Spiele verwendet werden, beispielsweise Side-Scroller oder Brettspiele. Es kann auch mit Direct3D für die vereinfachte Erstellung von 2D-Grafiken in einem 3D-Spiel verwendet werden, beispielsweise einer Benutzeroberfläche oder einer Heads-Up-Anzeige.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/dd370990">Direct2D</a>-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite stellt zusätzliche Funktionen für das Arbeiten mit Text bereit und kann mit Direct3D oder Direct2D zum Bereitstellen der Textausgabe für Benutzeroberflächen oder andere Bereiche verwendet werden, in denen Text erforderlich ist. DirectWrite unterstützt Messung, Zeichnung und Treffererkennung von Text in mehreren Formaten. DirectWrite behandelt Text in allen unterstützten Sprachen für globale und lokalisierte Anwendungen. DirectWrite stellt außerdem eine API für Glyphenrendering auf niedriger Ebene für Entwickler zur Ausführung von eigenem Layout und eigener Unicode-zu-Glyphen-Verarbeitung bereit.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p></p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/dd368038">DirectWrite</a>-Dokumentation.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition ist eine Windows-Komponente, die eine Hochleistungs-Bitmapanordnung mit Übergängen, Effekten und Animationen ermöglicht. Anwendungsentwickler können die DirectComposition-API zum Erstellen von visuell ansprechenden Benutzeroberflächen verwenden, die umfassende und fließende animierte Übergänge von einem visuellen Objekt zum nächsten ermöglichen.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>DirectComposition wurde für die vereinfachte Anordnung von visuellen Objekten und Erstellen von animierten Übergängen entwickelt. Wenn Ihr Spiel eine komplexe Benutzeroberfläche erfordert, können Sie DirectComposition zum vereinfachten Erstellen und Verwalten der Benutzeroberfläche verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371">DirectComposition</a>-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Audio – APIs für die Audiowiedergabe und die Anwendung von Audioeffekten. Informationen zum Verwenden von Audio-APIs in Ihrem Spiel finden Sie unter [Audio für Spiele](working-with-audio-in-your-directx-game.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 ist eine Low-Level-Audio-API, die eine grundlegende Signalverarbeitung und -abmischung bereitstellt. XAudio wurde für eine hohe Reaktionsfähigkeit in Bezug auf Audiomodule von Spielen entwickelt. Gleichzeitig können weiterhin benutzerdefinierte Audioeffekte und komplexe Ketten von Audioeffekten und -filtern erstellt werden.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie XAudio2, wenn Ihr Spiel Sounds mit minimalem Aufwand und minimaler Verzögerung wiedergeben muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/hh405049">XAudio2</a>-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Media Foundation</td>
    <td align="left"><p>Microsoft Media Foundation wurde für die Wiedergabe von Mediendateien und Streams (Audio und Video) entwickelt, kann jedoch auch in Spielen verwendet werden, wenn eine Funktionalität auf höherer Ebene als XAudio2 erforderlich ist und der zusätzliche Aufwand akzeptabel ist.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Media Foundation ist bei Filmszenen oder nichtinteraktiven Komponenten des Spiels besonders nützlich. Media Foundation ist auch für die Decodierung von Audiodateien für die Wiedergabe mit XAudio2 nützlich.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter <a href="https://msdn.microsoft.com/library/windows/desktop/ms694197">Microsoft Media Foundation</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Eingabe – APIs für die Eingabe über Tastatur, Maus, Gamepad und andere Benutzereingabequellen.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>Mit der XInput-Gamecontroller-API können Anwendungen Eingaben von Gamecontrollern erhalten.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Wenn Ihr Spiel die Gamepadeingabe unterstützen muss und Sie über vorhandenen XInput-Code verfügen, können Sie weiterhin XInput verwenden. XInput wurde durch Windows.Gaming.Input für UWP ersetzt, und Sie sollten beim Schreiben von neuem Eingabecode Windows.Gaming.Input anstelle von XInput verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/hh405053">XInput</a>-Dokumentation.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>Die Windows.Gaming.Input-API ersetzt XInput und bietet die gleiche Funktionalität mit den folgenden Vorteilen gegenüber Xinput:</p>
    <ul>
    <li>Geringere Ressourcenverwendung</li>
    <li>Geringere Latenz des API-Aufrufs zum Abrufen von Eingaben</li>
    <li>Die Möglichkeit, mit mehr als 4 Gamepads gleichzeitig zu arbeiten</li>
    <li>Die Möglichkeit zum Zugreifen auf zusätzliche Xbox One-Gamepadfeatures, z. B. Triggervibrationsmotoren.</li>
    <li>Die Möglichkeit, beim Herstellen oder Trennen einer Verbindung durch ein Ereignis anstelle von Abruf benachrichtigt zu werden.</li>
    <li>Die Möglichkeit zum Zuweisen der Eingabe an einen bestimmten Benutzer (Windows.System.User)</li>
    </ul>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Wenn Ihr Spiel Eingaben über ein Gamepad unterstützen muss und einen vorhandenen XInput-Code nicht verwendet, oder wenn Sie einen der oben genannten Vorteile nutzen möchten, sollten Sie Windows.Gaming.Input verwenden.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der Dokumentation für <a href="https://msdn.microsoft.com/library/windows/apps/dn707817">Windows.Gaming.Input</a>.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>Mit der Windows.UI.Core.CoreWindow-Klasse können Sie nachverfolgen, wann der Zeiger gedrückt oder verschoben wird oder wann eine Taste gedrückt wird.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie die Windows.UI.Core.CoreWindows-Ereignisse, wenn Sie nachverfolgen möchten, wann die Maus oder eine Taste in Ihrem Spiel gedrückt wird.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen zum Verwenden der Maus oder Tastatur in Ihrem Spiel finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">Bewegungs-/Blicksteuerungen für Spiele</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Math-APIs – APIs zum Vereinfachen häufig verwendeter mathematischer Vorgänge.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>Die DirectXMath-API bietet SIMD-freundliche C++-Typen und Funktionen für allgemeine lineare Algebra- und mathematische Grafikvorgänge für Spiele.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Die Verwendung von DirectXMath ist optional und vereinfacht allgemeine mathematische Vorgänge.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie in der <a href="https://msdn.microsoft.com/library/windows/desktop/hh437833">DirectXMath</a>-Dokumentation.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Netzwerke – APIs für die Kommunikation mit anderen Computern und Geräten entweder über das Internet oder private Netzwerke.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Der Windows.Networking.Sockets-Namespace stellt TCP- und UDP-Sockets bereit, die eine zuverlässige bzw. nicht zuverlässige Netzwerkkommunikation ermöglichen.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie Windows.Networking.Sockets, wenn Ihr Spiel mit anderen Computern und Geräten über das Netzwerk kommunizieren muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Netzwerk für Spiele</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>Der Windows.Web.HTTP-Namespace stellt eine zuverlässige Verbindung mit HTTP-Servern bereit, die für den Zugriff auf eine Website verwendet werden kann.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie Windows.Web.HTTP, wenn Ihr Spiel zum Abrufen oder Speichern von Informationen auf eine Website zugreifen muss.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">Netzwerk für Spiele</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Support-Dienstprogramme - Bibliotheken, die auf Grundlage von Windows 10-APIs entwickelt wurden.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Library</th>
    <th align="left">Beschreibung</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectX-Toolkit</td>
    <td align="left"><p>Das DirectX-Toolkit (DirectXTK) ist eine Sammlung von Hilfsklassen für DirectX 11.x-Code in C++.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie das DirectX-Toolkit, wenn Sie C++-Entwickler und auf der Suche nach einem modernen Ersatz für älteren D3DX-Hilfsprogrammcode sind, oder ein XNA Game Studio-Entwickler sind, der zu systemeigenen C++ wechselt.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter der DirectX-Toolkit-Projektseite <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a>.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D ist eine einfach zu verwendende Windows-Runtime-API für 2D-Grafikrendering im unmittelbaren Modus.</p>
    <p><strong>Verwendungsszenarien</strong></p>
    <p>Verwenden Sie Win2D, wenn Sie C++-Entwickler sind und einen einfacher zu verwendenden WinRT-Wrapper für Direct2D und DirectWrite benötigen, oder C#-Entwickler sind und Direct2D und DirectWrite verwenden möchten.</p>
    <p><strong>Weitere Informationen</strong></p>
    <p>Weitere Informationen finden Sie unter der Win2D-Projektseite <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

## <a name="xbox-live-services"></a>Xbox Live-Dienste

Die [Xbox Live Creators-Programm](https://developer.microsoft.com/games/xbox/xboxlive/creator) kann jeder Entwickler zum Integrieren von Xbox Live in ihre UWP-Spiel, und Veröffentlichen von Xbox One und Windows 10. Integrieren Sie soziale Xbox Live-Funktionen wie Anmelden, Präsenzinformationen, Ranglisten und mehr mit minimaler Entwicklungszeit in Ihre Titel. Die sozialen Xbox Live-Features wurden entwickelt, damit Sie Ihre Zielgruppe organisch auf bis zu 55 Millionen aktive Spieler erweitern können.

Treten Sie dem [ID@Xbox](https://www.xbox.com/developers/id) bei, wenn Sie Zugriff auf weitere Xbox Live-Funktionen wünschen, dedizierte Marketing- und Entwicklungsunterstützung benötigen oder im allgemeinen Xbox One-Store vertreten sein möchten. Um festzustellen, welche Features für das Xbox Live Creators-Programm und das ID@Xbox-Programm verfügbar sind, wechseln Sie auf die [Tabelle der Features](../xbox-live/developer-program-overview.md#feature-table).

Weitere Informationen finden Sie unter [Hinzufügen von Xbox Live zu Ihrem Spiel](e2e.md#adding-xbox-live-to-your-game).

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>Alternativen zum Schreiben von Spielen mit DirectX und UWP


### <a name="uwp-games-without-directx"></a>UWP-Spiele ohne DirectX

Einfachere Spiele mit minimalem Leistungsanforderungen, z. B. Kartenspiele oder Brettspiele, können ohne DirectX und müssen nicht unbedingt in C++ geschrieben werden. Bei dieser Art von Spielen können alle von UWP unterstützten Sprachen verwendet werden, z. B. C#, Visual Basic, C++ und HTML/JavaScript. Wenn Ihr Spiel weder eine hohe Leistung noch rechenintensive Grafiken erfordert, sollten Sie sich das [Beispiel für ein JavaScript- und HTML5-Fingereingabespiel](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031) ansehen.

### <a name="game-engines"></a>Spielengines

Eine Alternative zum Schreiben eigener Spielemodule mithilfe der Windows-APIs für die Entwicklung von Spielen stellen die zahlreichen qualitativ hochwertigen Spielemodule dar, die auf den Windows-APIs für die Entwicklung von Spielen basieren und für die Entwicklung von Spielen auf Windows-Plattformen zur Verfügung stehen. Wenn Sie eine Spielengine oder Bibliothek in Erwägung ziehen, können Sie zwischen den folgenden Möglichkeiten wählen:

-   Vollständige Spielengine – Eine vollständige Spielengine umfasst die meisten bzw. alle Windows 10-APIs, die Sie zum Schreiben einer eigenen Spielengine verwenden würden, z. B. Grafik, Eingabe und Netzwerk. Vollständige Spielengines können auch Spiellogikfunktionalität bereitstellen, z. B. künstliche Intelligenz und Pfadsuche.
-   Grafikengine – Grafikengines umfassen Windows 10-Grafik-APIs, verwalten Grafikressourcen und unterstützen zahlreiche Modell- und Weltformate.
-   Audioengine – Audioengines umfassen Windows 10-Audio-APIs, verwalten Audioressourcen und stellen erweiterte Audiowiedergabe und -Effekte bereit.
-   Netzwerkengine – Netzwerkengines umfassen Windows 10-Netzwerk-APIs zum Hinzufügen von Peer-zu-Peer- oder serverbasierter Multiplayerunterstützung zu Ihrem Spiel und enthalten ggf. erweiterte Netzwerkfunktionalität zur Unterstützung einer großen Anzahl von Spielern.
-   Engine für künstliche Intelligenz und Pfadsuche – Engines für künstliche Intelligenz und Pfadsuche stellen ein Framework zur Steuerung des Agentverhaltens in einem Spiel bereit.
-   Engines für besondere Zwecke – Es stehen unterschiedliche weitere Engines zur Bewältigung von nahezu allen möglichen Spielentwicklungsaufgaben zur Verfügung, z. B. Erstellen von Inventarsystemen und Dialogstrukturen.

## <a name="submitting-a-game-to-the-store"></a>Übermitteln eines Spiels an den Store


Wenn Ihr Spiel zur Veröffentlichung bereit steht, müssen Sie ein Entwicklerkonto erstellen und Ihr Spiel an den Microsoft Store übermitteln.

Informationen zum Übermitteln Ihres Spiels an den Microsoft Store finden Sie unter [Übermitteln und Veröffentlichen Ihres Spiels](e2e.md#submitting-and-publishing-your-game).

 

 




