---
title: Planen der Portierung von OpenGL ES 2.0 zu Direct3D
description: Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, OpenGL, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 44b851ef96b93974724ff4cf0b309d119dfd72d7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368963"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>Planen der Portierung von OpenGL ES 2.0 zu Direct3D




**Wichtige APIs**

-   [Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140))

Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert. Beim Vorbereiten der Portierung Ihrer Grafikpipeline-Codebasis zu Direct3D 11 und der Windows-Runtime sind im Vorfeld einige Dinge zu beachten.

Meist beinhaltet das Portieren eine anfängliche Überprüfung der Codebasis und die Zuordnung allgemeiner APIs und Muster zwischen den beiden Modellen. Sie können diesen Vorgang vereinfachen, indem Sie sich die Zeit für die Lektüre dieses Themas nehmen.

Folgende Punkte sind beim Portieren von Grafiken von OpenGL ES 2.0 zu Direct3D 11 zu beachten.

## <a name="notes-on-specific-opengl-es-20-providers"></a>Hinweise zu bestimmten OpenGL ES 2.0-Anbietern


Die Themen zur Portierung in diesem Abschnitt beziehen sich auf die Windows-Implementierung der von der Khronos Group erstellten OpenGL ES 2.0-Spezifikation. Alle OpenGL ES 2.0-Codebeispiele wurden mit Visual Studio 2012 und einfacher Windows C-Syntax entwickelt. Bedenken Sie im Fall einer Objective-C (iOS)- oder Java (Android)-Codebasis, dass in den gezeigten OpenGL ES 2.0-Codebeispielen möglicherweise keine ähnliche API-Aufrufsyntax oder ähnlichen Parameter verwendet werden. Wir haben versucht, diesen Leitfaden so plattformagnostisch wie möglich zu halten.

In dieser Dokumentation werden nur die APIs der 2.0-Spezifikation für den OpenGL ES-Code und die zugehörige Referenz verwendet. Falls Sie von OpenGL ES 1.1 oder 3.0 portieren, kann diese Dokumentation trotzdem hilfreich sein, auch wenn Ihnen einige der OpenGL ES 2.0-Codebeispiele und der Kontext vielleicht nicht vertraut sind.

In den Direct3D 11-Beispielen in diesem Thema wird Microsoft Windows C++ mit Komponentenerweiterungen (CX) verwendet. Lesen Sie weitere Informationen zu dieser Version von der C++-Syntax, [Visual C++](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140)), [Komponentenerweiterungen für Laufzeitplattformen](https://docs.microsoft.com/cpp/windows/component-extensions-for-runtime-platforms), und [Quick Reference (C++\\CX)](https://docs.microsoft.com/cpp/cppcx/quick-reference-c-cx).

## <a name="understand-your-hardware-requirements-and-resources"></a>Verständnis der Hardwareanforderungen und Ressourcen


Die von OpenGL ES 2.0 unterstützten Grafikverarbeitungsfunktionen entsprechen in etwa den in Direct3D 9.1 verfügbaren Funktionen. Wenn Sie die Vorteile der umfassenderen Funktionen von Direct3D 11 nutzen möchten, lesen Sie beim Planen der Portierung die Dokumentation zu [Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) oder im Anschluss an die Planung die Themen zum [Portieren von DirectX 9 zur Universellen Windows-Plattform (UWP)](porting-your-directx-9-game-to-windows-store.md).

Um Ihre erste Portierung möglichst einfach zu machen, sollten Sie mit einer Visual Studio Direct3D-Vorlage beginnen. Sie enthält einen einfachen bereits konfigurierten Renderer und unterstützt Features von UWP-Apps wie das Neuerstellen von Ressourcen bei Fensteränderungen und Direct3D-Funktionsebenen.

## <a name="understand-direct3d-feature-levels"></a>Verständnis der Direct3D-Funktionsebenen


Direct3D 11 bietet Unterstützung für Hardware "Funktionsebenen" aus 9\_1 (Direct3D 9.1) für 11\_1. Diese Funktionsebenen geben die Verfügbarkeit bestimmter Grafikfunktionen und -ressourcen an. Die meisten OpenGL ES 2.0-Plattformen unterstützen in der Regel eine Direct3D-9.1 (9 die Funktionsebene\_1) Gruppe von Funktionen.

## <a name="review-directx-graphics-features-and-apis"></a>Überprüfen der DirectX-Grafikfunktionen und -APIs


| API-Familie                                                | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | Die DirectX Graphics Infrastructure (DXGI) stellt eine Schnittstelle zwischen der Grafikhardware und Direct3D bereit. Sie legt die Geräteadapter- und Hardwarekonfiguration mit den COM-Schnittstellen [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) und [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) fest. Verwenden Sie sie zum Erstellen und Konfigurieren Ihrer Puffer und anderen Fensterressourcen. Beachten Sie insbesondere, dass das [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)-Factorymuster zum Erfassen der Grafikressourcen verwendet wird, einschließlich der Swapchain (eine Gruppe von Framepuffern). Da DXGI der Besitzer der Swapchain ist, wird die [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)-Schnittstelle verwendet, um Frames auf dem Bildschirm darzustellen. |
| [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D ist der Satz von APIs, die eine virtuelle Darstellung der Grafikschnittstelle bereitstellen und es Ihnen ermöglichen, damit Grafiken zu zeichnen. Version 11 ist von den Funktionen her in etwa mit OpenGL 4.3 vergleichbar. (OpenGL ES 2.0 auf der anderen Seite ähnelt DirectX9, feature-wise und OpenGL 2.0, aber die OpenGL 3.0 unified Shader-Pipeline.) Ein Großteil der Schwerstarbeit erfolgt anhand der ID3D11Device1 und ID3D11DeviceContext1 Schnittstellen, die bzw. den Zugriff auf einzelne Ressourcen und-unterressourcen und das Rendering-Kontext zu bieten.                                                                                                                                          |
| [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D stellt einen Satz von APIs für GPU-beschleunigtes 2D-Rendering bereit. Hinsichtlich des Verwendungszwecks ist es mit OpenVG vergleichbar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite stellt einen Satz von APIs für GPU-beschleunigtes Schriftartrendering in hoher Qualität bereit.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath stellt einen Satz von APIs und Makros für die Behandlung allgemeiner linearer Algebra sowie trigonometrischer Typen, Werte und Funktionen bereit. Diese Typen und Funktionen sind so konzipiert, dass sie gut mit Direct3D und den zugehörigen Shadervorgängen funktionieren.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Die von Direct3D-Shadern verwendete aktuelle HLSL-Syntax. Sie implementiert das Direct3D-Shadermodell 5.0.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Überprüfen der Windows-Runtime-APIs und -Vorlagenbibliothek


Die Windows-Runtime-APIs stellen die allgemeine Infrastruktur für UWP-Apps bereit. Weitere Informationen finden [hier](https://docs.microsoft.com/uwp/api/).

Folgende wichtige Windows-Runtime-APIs werden beim Portieren Ihrer Grafikpipeline verwendet:

-   [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows::UI::Core::CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

Die C++-Vorlagenbibliothek für Windows-Runtime (WRL) ist eine Vorlagenbibliothek, die eine einfache Methode zum Erstellen und Verwenden von Komponenten für Windows-Runtime bietet. Die Direct3D 11-APIs für UWP-Apps lassen sich am besten in Verbindung mit den Schnittstellen und Typen in dieser Bibliothek wie etwa intelligenten Zeigern ([ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class)) verwenden. Weitere Informationen zur WRL finden Sie unter [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](https://docs.microsoft.com/cpp/windows/windows-runtime-cpp-template-library-wrl).

## <a name="change-your-coordinate-system"></a>Ändern des Koordinatensystems


Ein Unterschied, der bei der Portierung anfänglich für Verwirrung sorgen kann, ist die Umstellung vom traditionellen rechtshändigen Koordinatensystem von OpenGL auf das standardmäßige linkshändige Koordinatensystem von Direct3D. Diese Änderung der Koordinatenmodellierung wirkt sich auf viele Teile des Spiels aus – angefangen bei der Einrichtung und Konfiguration der Scheitelpunktpuffer bis hin zu vielen der mathematischen Matrixfunktionen. Die folgenden zwei Änderungen sind am wichtigsten:

-   Spiegeln Sie die Reihenfolge von Dreieckscheitelpunkten, sodass sie von Direct3D von vorne im Uhrzeigersinn durchlaufen werden. Wenn die Scheitelpunkte in der OpenGL-Pipeline beispielsweise als 0, 1 und 2 indiziert wurden, übergeben Sie sie stattdessen als 0, 2, 1 an Direct3D.
-   Verwenden Sie die Ansichtsmatrix, um den Raum um -1.0f in der z-Richtung zu skalieren, sodass die z-Achsenkoordinaten umgekehrt werden. Kehren Sie dazu das Vorzeichen der Werte an den Positionen M31, M32 und M33 in der Ansichtsmatrix um (beim Portieren in den [**Matrix**](https://docs.microsoft.com/windows/desktop/direct3d9/matrix4x4)-Typ). Falls M34 nicht 0 ist, kehren Sie das Vorzeichen dieser Position ebenfalls um.

Direct3D kann jedoch ein rechtshändiges Koordinatensystem unterstützen. DirectXMath bietet eine Reihe von Funktionen, die sowohl für links- als auch für rechtshändige Koordinatensysteme funktionieren. Mithilfe dieser Funktionen können Sie einen Teil Ihrer ursprünglichen Gitterdaten und der Matrixverarbeitung beibehalten. Dazu gehören:

| DirectXMath-Matrixfunktion                                                   | Beschreibung                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.       |
| [**XMMatrixLookAtRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.      |
| [**XMMatrixLookToLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung.  |
| [**XMMatrixLookToRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung. |
| [**XMMatrixOrthographicLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | Erstellt eine orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                           |
| [**XMMatrixOrthographicOffCenterRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                          |
| [**XMMatrixOrthographicRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | Erstellt eine orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                                |
| [**XMMatrixPerspectiveFovLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | Erstellt eine linkshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                                |
| [**XMMatrixPerspectiveFovRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | Erstellt eine rechtshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                               |
| [**XMMatrixPerspectiveLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | Erstellt eine linkshändige perspektivische Projektionsmatrix.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | Erstellt eine benutzerdefinierte Version einer linkshändigen perspektivischen Projektionsmatrix.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | Erstellt eine benutzerdefinierte Version einer rechtshändigen perspektivischen Projektionsmatrix.                                                    |
| [**XMMatrixPerspectiveRH**](https://docs.microsoft.com/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | Erstellt eine rechtshändige perspektivische Projektionsmatrix.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>Häufig gestellte Fragen zur Portierung von OpenGL ES 2.0 zu Direct3D 11


-   Frage: "Im Allgemeinen kann ich suchen für bestimmte Zeichenfolgen oder Mustern in meinem OpenGL-Code und durch die Direct3D-Entsprechungen ersetzen?"
-   Antwort: Nein. OpenGL ES 2.0 und Direct3D 11 stammen aus verschiedenen Generationen der Grafikpipelinemodellierung. Obwohl es Ähnlichkeiten zwischen den Konzepten und APIs gibt (z. B. der Renderkontext und die Instanziierung von Shadern), sollten Sie sowohl diesen Leitfaden als auch die Direct3D 11-Referenz lesen, um beim Neuerstellen Ihrer Pipeline die beste Wahl treffen zu können. Eine 1:1-Zuordnung ist nicht zu empfehlen. Wenn Sie aber von GLSL zu HLSL portieren, kann das Erstellen eines allgemeinen Satzes von Aliasen für Variablen, systeminterne Elemente und Funktionen von GLSL nicht nur die Portierung vereinfachen, sondern es bietet Ihnen auch die Möglichkeit, mit einem einzigen Satz von Shadercodedateien zu arbeiten.

 

 




