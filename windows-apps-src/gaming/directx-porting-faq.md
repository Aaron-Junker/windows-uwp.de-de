---
title: DirectX 11 – Häufig gestellte Fragen zur Portierung
description: Antworten auf häufig gestellte Fragen zur Portierung von Spielen zur Universellen Windows-Plattform (UWP)
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: bc6e1c2053b6ab3afe6eb42ed8b5223feb8ffb65
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192990"
---
# <a name="directx-11-porting-faq"></a>DirectX 11 – Häufig gestellte Fragen zur Portierung




Antworten auf häufig gestellte Fragen zur Portierung von Spielen zur Universellen Windows-Plattform (UWP)

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>Kann ich mein Spiel durch Suchen und Ersetzen von API-Methoden portieren, oder ist eine sorgfältigere Planung des Portierungsprozesses erforderlich?


Direct3D 11 wurde gegenüber Direct3D 9 deutlich aktualisiert. Es wurden mehrere Paradigmenwechsel vorgenommen, darunter separate APIs für den virtualisierten Grafikadapter und seinen Kontext sowie eine neue Polymorphieebene für Geräteressourcen. Im Wesentlichen kann Ihr Spiel Grafikhardware weiter wie zuvor verwenden. Sie müssen sich allerdings mit der neuen API-Architektur von Direct3D 11 vertraut machen und alle Teile Ihres Grafikcodes zur Verwendung der richtigen API-Komponenten aktualisieren. Siehe [Portierungskonzepte und Überlegungen zur Portierung](porting-considerations.md).

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>Wofür dient der neue Gerätekontext? Muss ich mein Direct3D 9-Gerät durch das Direct3D 11-Gerät ersetzen, den Gerätekontext oder beides?


Das Direct3D-Gerät wird jetzt zum Erstellen von Ressourcen im Videospeicher verwendet. Der Gerätekontext dient dagegen zum Festlegen des Pipelinestatus und Generieren von Renderbefehlen. Weitere Informationen finden Sie unter [Was sind die wichtigsten Änderungen seit Direct3D 9?](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>Muss ich meinen Spieltimer für UWP aktualisieren?


[**QueryPerformanceCounter**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter) in Verbindung mit [**QueryPerformanceFrequency**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancefrequency) ist nach wie vor die beste Methode, um einen Spieltimer für UWP-Apps zu implementieren.

Eine Kleinigkeit sollten Sie im Hinblick auf Timer und den UWP-App-Lebenszyklus beachten. Das Anhalten und Fortsetzen ist nicht das Gleiche wie das erneute Starten eines Desktopspiels durch den Spieler, da das Spiel in diesem Fall eine Momentaufnahme ab dem Zeitpunkt fortsetzt, zu dem das Spiel zuletzt gespielt wurde. Ist viel Zeit vergangen – z. B. einige Wochen – können bei einigen Timerimplementierungen Fehler auftreten. Sie können App-Lebenszyklusereignisse verwenden, um Ihren Timer beim Fortsetzen des Spiels zurückzusetzen.

Spiele, die noch immer die RDTSC-Anweisung verwenden, müssen aktualisiert werden. Siehe [Zeitliche Steuerung von Spielen und Multi-Core-Prozessoren](/windows/desktop/DxTechArts/game-timing-and-multicore-processors).

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>Mein Spielcode basiert auf D3DX und DXUT. Gibt es irgendetwas, das ich zum Migrieren des Codes verwenden kann?


Das Communityprojekt [DirectX-Toolkit (DirectXTK)](https://github.com/Microsoft/DirectXTK) bietet Hilfsklassen zur Verwendung mit Direct3D 11.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>Gewusst wie die Codepfade für den Desktop und die Microsoft Store beibehalten?


Die Artikel Reihe von Chuck walbourn mit der Bezeichnung [Dual-Use-Codierungstechniken für Spiele](https://blogs.msdn.com/b/chuckw/archive/2012/09/17/dual-use-coding-techniques-for-games.aspx) bietet Anleitungen zum Freigeben von Code zwischen dem Desktop und den Microsoft Store Codepfade.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>Wie lade ich Bildressourcen in meine DirectX-UWP-App?


Zum Laden von Bildern sind zwei API-Pfade verfügbar:

-   Die Inhaltspipeline konvertiert Bilder in DDS-Dateien, die als Direct3D-Texturressourcen verwendet werden. Siehe [Verwenden von 3D-Ressourcen in Spielen oder Apps](/visualstudio/designers/using-3-d-assets-in-your-game-or-app).
-   Mit der [Windows-Bilderstellungskomponente](/windows/desktop/wic/-wic-lh) können Bilder in vielen verschiedenen Formaten geladen werden. Sie kann sowohl für Direct2D-Bitmaps als auch für Direct3D-Texturressourcen verwendet werden.

Sie können auch den DDSTextureLoader und den WICTextureLoader aus [DirectXTK](https://github.com/Microsoft/DirectXTK) oder [DirectXTex](https://github.com/Microsoft/DirectXTex) verwenden.

## <a name="where-is-the-directx-sdk"></a>Wo finde ich das DirectX SDK?


Das DirectX SDK ist im Windows SDK enthalten. Das neueste DirectX SDK, das vom Windows SDK getrennt war, wurde im Juni 2010 veröffentlicht. Direct3D-Beispiele sind wie die anderen Windows-App-Beispiele in der Codegalerie enthalten.

## <a name="what-about-directx-redistributables"></a>Was ist mit verteilbaren DirectX-Komponenten?


Der Großteil der Komponenten im Windows SDK ist bereits in unterstützten Versionen des Betriebssystems enthalten oder verfügt nicht über eine DLL-Komponente (z. B. DirectXMath). Alle Direct3D-API-Komponenten, die von UWP-Apps verwendet werden können, werden bereits für Ihr Spiel verfügbar sein. Sie müssen sie nicht neu verteilen.

Win32-Desktopanwendungen verwenden weiterhin DirectSetup. Wenn Sie die Desktopversion Ihres Spiels aktualisieren, gehen Sie daher wie unter [Direct3D 11-Bereitstellung für Spielentwickler](/windows/desktop/direct3darticles/direct3d11-deployment) beschrieben vor.

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>Kann ich meinen Desktopcode vor der Umstellung von Effects auf DirectX 11 aktualisieren?


Siehe [Effects für Direct3D 11-Update](https://github.com/Microsoft/FX11). Mit Effects 11 können Abhängigkeiten von älteren DirectX-SDK-Headern entfernt werden. Es ist als Portierungshilfsmittel vorgesehen und kann nur für Desktop-Apps verwendet werden.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>Gibt es einen Pfad für die Portierung meines DirectX 8-Spiels zu UWP?


Yes:

-   Lesen Sie [Konvertieren in Direct3D 9](/windows/desktop/direct3d9/converting-to-directx-9).
-   Stellen Sie sicher, dass Ihr Spiel keine Überreste der festen Pipeline enthält (siehe [Veraltete Funktionen](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)).
-   Verwenden Sie anschließend den DirectX 9-Portierungspfad: [Portieren von D3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>Kann ich mein DirectX 10- oder 11-Spiel zu UWP portieren?


DirectX 10.x- und 11-Desktopspiele können ohne großen Aufwand zu UWP portiert werden. Siehe [Migrieren zu Direct3D 11](/windows/desktop/direct3d11/d3d11-programming-guide-migrating).

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>Wie wähle ich bei einem Multi-Monitor-System das richtige Anzeigegerät aus?


Der Benutzer wählt aus, auf welchem Monitor Ihre App angezeigt wird. Rufen Sie [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) auf, und legen Sie den ersten Parameter auf **nullptr** fest, damit Windows den korrekten Adapter auswählt. Rufen Sie anschließend die [**IDXGIDevice interface**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice) des Geräts ab, rufen Sie [**GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) auf, und erstellen Sie die Swapchain mit dem DXGI-Adapter.

## <a name="how-do-i-turn-on-antialiasing"></a>Wie aktiviere ich Antialiasing?


Antialiasing (Multisampling) wird beim Erstellen des Direct3D-Geräts aktiviert. Auflisten der Unterstützung für mehrere Stichproben durch Aufrufen von [**checkmultisamplequalitylevels**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels)und anschließendes Festlegen von Multisampling-Optionen in der [**DXGI- \_ \_ beispieldesc-Struktur**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) beim Aufrufen von [**CreateSurface**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-createsurface).

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>Mein Spiel rendert mit Multithreading und/oder verzögertem Rendering. Was muss ich in diesem Zusammenhang bei Direct3D 11 beachten?


Lesen Sie [Einführung in das Multithreading in Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro). Dort werden die ersten Schritte erläutert. Eine Liste der wichtigsten Unterschiede finden Sie unter [Unterschiede beim Threading zwischen Direct3D-Versionen](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-differences). Beachten Sie, dass beim verzögerten Rendering ein *verzögerter Gerätekontext* anstelle eines *unmittelbaren Kontextes* verwendet wird.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Wo finde ich weitere Informationen zur programmierbaren Pipeline seit Direct3D 9?


Lesen Sie die folgenden Themen:

-   [Programmieranleitung für HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-pguide)
-   [Häufig gestellte Fragen zu Direct3D 10](/windows/desktop/DxTechArts/direct3d10-frequently-asked-questions)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>Was sollte ich anstelle des X-Dateiformats für meine Modelle verwenden?


Obwohl wir keinen offiziellen Ersatz für das x-Dateiformat haben, verwenden viele der Beispiele das sdkmesh-Format. Visual Studio bietet außerdem eine [Inhaltspipeline](/visualstudio/designers/using-3-d-assets-in-your-game-or-app), die verschiedene gängige Formate in CMO-Dateien kompiliert, die mit Code aus dem Visual Studio 3D-Starter Kit oder mit dem [DirectXTK](https://github.com/Microsoft/DirectXTK) geladen werden können.

## <a name="how-do-i-debug-my-shaders"></a>Wie debugge ich meine Shader?


Microsoft Visual Studio enthält Diagnosetools für DirectX-Grafiken. Siehe [Debuggen von DirectX-Grafiken](/visualstudio/debugger/visual-studio-graphics-diagnostics).

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>Was ist das Direct3D 11-Äquivalent für die *x*-Funktion?


Informationen hierzu finden Sie in der [Funktionszuordnung](feature-mapping.md#function-mapping) in „Zuordnung von DirectX 9-Features zu DirectX 11-APIs“.

##  <a name="what-is-the-dxgi_format-equivalent-of-y-surface-format"></a>Welches DXGI- \_ Format entspricht dem *y* -Oberflächen Format?


Informationen hierzu finden Sie in der [Oberflächenformatzuordnung](feature-mapping.md#surface-format-mapping) in „Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs“.

 

 