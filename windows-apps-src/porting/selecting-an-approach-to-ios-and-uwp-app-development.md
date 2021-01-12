---
description: Erfahren Sie mehr über Tools und Techniken, die Ihnen beim Schreiben von Apps helfen können, die Windows, IOS und Android unterstützen.
title: Auswählen eines Ansatzes für die Entwicklung von iOS- und UWP-Apps
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b7a8920b13b7e28947138a000fd46165abcb58b7
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104681"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>Auswählen eines Ansatzes für die Entwicklung von iOS- und UWP-Apps


Welche Optionen gibt es beim Entwickeln von plattformübergreifenden Apps?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>Wie werden iOS und Windows am besten unterstützt?

Windows und iOS sind anscheinend sehr unterschiedlich, es stehen jedoch immer mehr Tools und Methoden zur Verfügung, mit denen Sie Apps entwickeln können, die beide Plattformen (sowie Android) unterstützen. Die beste Lösung hängt vom Typ der App ab, die Sie entwickeln. Ausschlaggebend ist zudem auch, ob Sie die App von Grund auf neu erstellen oder ein vorhandenes Projekt portieren.

## <a name="writing-a-new-app"></a>Schreiben einer neuen App

Wenn Sie eine neue App erstellen, stehen zahlreiche Optionen zur Verfügung, u. a.:

-   [Xamarin](https://xamarin.com/)

    Mit Xamarin können Sie Ihre App in C# schreiben, sie unter Windows ausführen und zudem systemeigene iOS-Apps entwickeln. Unterstützung für Xamarin ist in Visual Studio integriert. Wählen Sie einfach den richtigen Projekttyp aus.

-   [Apache Cordova](https://www.microsoft.com/?ref=go)

    Falls Sie Javascript und HTML bevorzugen, unterstützt Sie Apache Cordova (auch als PhoneGap bezeichnet) beim Entwickeln plattformübergreifender Apps für iOS, Windows und Android. Dieser Projekttyp ist ebenfalls in Visual Studio integriert.

-   Spielengines

    Mit Tools wie [Unity3D](https://www.unity3d.com/) und [Unreal Engine](https://www.unrealengine.com/en-US/) können Sie Spiele in AAA-Qualität für Windows und zahlreiche andere Plattformen, einschließlich iOS, programmieren. Unity unterstützt C#-Skripting, Unreal verwendet C++.

-   [MonoGame](http://www.monogame.net/)

    Der geistige Nachfolger von XNA. Nun handelt es sich dabei um ein plattformübergreifendes Open Source-Framework. Das bedeutet, Sie können Apps in C# für zahlreiche Plattformen mit Unterstützung für Physik-Engines sowie 2D- und 3D-Grafiken schreiben.

## <a name="adapting-an-existing-app"></a>Anpassen einer vorhandenen App

Bei einer vorhandenen iOS-App stehen weniger Optionen zur Verfügung. Es ist jedoch nicht alles verloren.

-   [Windows-Brücke für iOS](https://github.com/Microsoft/WinObjC)

    Dies ist ein noch Entwicklungs Tool, mit dem Xcode-Projekte direkt in Visual Studio importiert werden können. Die Erstellung und das Debugging von Objective-C-Code kann in Visual Studio ausgeführt werden. Falls Ihr Projekt Bibliotheken verwendet, beispielsweise Cocos für Grafiken, ist dies möglicherweise eine praktische Möglichkeit zum schnellen Portieren Ihrer App.

-   Sie können Ihren C++-Code wiederverwenden.

    Wenn die Kerngeschäftslogik nicht in Objective-C oder Swift, sondern in C++ geschrieben ist, können Sie diesen Code häufig mit geringfügigen Änderungen in Ihrem Projekt verwenden. Anschließend können Sie wie bei anderen Windows-Apps mithilfe von XAML die Benutzeroberfläche definieren und den C++-Code bei Bedarf nutzen.

-   [Verwenden von ANGLE zum Ausführen von OpenGL ES unter Windows](https://github.com/microsoft/angle/wiki)

    Ein Zwischenschritt zum Portieren Ihres OpenGL ES 2.0-Projekts ist die Verwendung von ANGLE. Mit ANGLE können Sie OpenGL ES-Inhalte unter Windows ausführen, indem Sie OpenGL ES-API-Aufrufe in DirectX 11-API-Aufrufe übersetzen.

## <a name="other-cross-platform-authoring-tools"></a>Andere plattformübergreifende Erstellungstools

-   [GameSalad](https://gamesalad.com/)

    Eine Spielerstellungsumgebung

-   [Construct 2]( https://www.scirra.com/)

    Eine Spielerstellungsumgebung

-   [Titanium Studio](https://www.appcelerator.com/platform/titanium-studio/)

    Eine plattformübergreifende Erstellungsumgebung

-   [Cocos2D-x](https://www.cocos2d-x.org/)

    Eine plattformübergreifende Codebibliothek zur Spritebehandlung und Physikmodellierung

-   [Impact.js](https://impactjs.com/)

    Eine HTML-basierte Spielbibliothek.

-   [Marmalade](http://madewithmarmalade.com/)

    Ein plattformübergreifendes SDK

-   [OpenFL](https://www.openfl.org/)

    Ein plattformübergreifendes Entwicklungstool

-   [GameMaker](https://www.yoyogames.com/gamemaker/studio)

    Eine spezielle Entwicklungsumgebung für Spiele

-   [PlayCanvas](https://playcanvas.com/)

    Ein HTML-basiertes Tool für die Spielentwicklung

