---
title: Planen der Portierung von OpenGL ES 2.0 zu Direct3D
description: Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, OpenGL, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: dddbf3a1009ccbaeb7fd0695d995cd17ba6182d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165344"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>Planen der Portierung von OpenGL ES 2.0 zu Direct3D




**Wichtige APIs**

-   [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](/previous-versions/60k1461a(v=vs.140))

Wenn Sie ein Spiel von der iOS- oder Android-Plattform portieren, haben Sie vermutlich erheblich in OpenGL ES 2.0 investiert. Beim Vorbereiten der Portierung Ihrer Grafikpipeline-Codebasis zu Direct3D 11 und der Windows-Runtime sind im Vorfeld einige Dinge zu beachten.

Meist beinhaltet das Portieren eine anfängliche Überprüfung der Codebasis und die Zuordnung allgemeiner APIs und Muster zwischen den beiden Modellen. Sie können diesen Vorgang vereinfachen, indem Sie sich die Zeit für die Lektüre dieses Themas nehmen.

Folgende Punkte sind beim Portieren von Grafiken von OpenGL ES 2.0 zu Direct3D 11 zu beachten.

## <a name="notes-on-specific-opengl-es-20-providers"></a>Hinweise zu bestimmten OpenGL ES 2.0-Anbietern


Die Themen zur Portierung in diesem Abschnitt beziehen sich auf die Windows-Implementierung der von der Khronos Group erstellten OpenGL ES 2.0-Spezifikation. Alle OpenGL ES 2.0-Codebeispiele wurden mit Visual Studio 2012 und einfacher Windows C-Syntax entwickelt. Bedenken Sie im Fall einer Objective-C (iOS)- oder Java (Android)-Codebasis, dass in den gezeigten OpenGL ES 2.0-Codebeispielen möglicherweise keine ähnliche API-Aufrufsyntax oder ähnlichen Parameter verwendet werden. Wir haben versucht, diesen Leitfaden so plattformagnostisch wie möglich zu halten.

In dieser Dokumentation werden nur die APIs der 2.0-Spezifikation für den OpenGL ES-Code und die zugehörige Referenz verwendet. Falls Sie von OpenGL ES 1.1 oder 3.0 portieren, kann diese Dokumentation trotzdem hilfreich sein, auch wenn Ihnen einige der OpenGL ES 2.0-Codebeispiele und der Kontext vielleicht nicht vertraut sind.

In den Direct3D 11-Beispielen in diesem Thema wird Microsoft Windows C++ mit Komponentenerweiterungen (CX) verwendet. Weitere Informationen zu dieser Version der C++-Syntax finden Sie unter [Visual C++](/previous-versions/60k1461a(v=vs.140)), [Komponenten Erweiterungen für laufzeitplattformen](/cpp/windows/component-extensions-for-runtime-platforms)und [Kurzübersicht (C++ \\ CX)](/cpp/cppcx/quick-reference-c-cx).

## <a name="understand-your-hardware-requirements-and-resources"></a>Verständnis der Hardwareanforderungen und Ressourcen


Die von OpenGL ES 2.0 unterstützten Grafikverarbeitungsfunktionen entsprechen in etwa den in Direct3D 9.1 verfügbaren Funktionen. Wenn Sie die Vorteile der umfassenderen Funktionen von Direct3D 11 nutzen möchten, lesen Sie beim Planen der Portierung die Dokumentation zu [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) oder im Anschluss an die Planung die Themen zum [Portieren von DirectX 9 zur Universellen Windows-Plattform (UWP)](porting-your-directx-9-game-to-windows-store.md).

Um Ihre erste Portierung möglichst einfach zu machen, sollten Sie mit einer Visual Studio Direct3D-Vorlage beginnen. Sie enthält einen einfachen bereits konfigurierten Renderer und unterstützt Features von UWP-Apps wie das Neuerstellen von Ressourcen bei Fensteränderungen und Direct3D-Funktionsebenen.

## <a name="understand-direct3d-feature-levels"></a>Verständnis der Direct3D-Funktionsebenen


Direct3D 11 bietet Unterstützung für Hardware-Funktionsebenen von 9 \_ 1 (Direct3D 9,1) für 11 \_ 1. Diese Funktionsebenen geben die Verfügbarkeit bestimmter Grafikfunktionen und -ressourcen an. In der Regel unterstützen die meisten OpenGL es 2,0-Plattformen einen Direct3D 9,1 (Feature Level 9 \_ 1)-Satz von Features.

## <a name="review-directx-graphics-features-and-apis"></a>Überprüfen der DirectX-Grafikfunktionen und -APIs


| API-Familie                                                | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | Die DirectX Graphics Infrastructure (DXGI) stellt eine Schnittstelle zwischen der Grafikhardware und Direct3D bereit. Sie legt die Geräteadapter- und Hardwarekonfiguration mit den COM-Schnittstellen [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) und [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) fest. Verwenden Sie sie zum Erstellen und Konfigurieren Ihrer Puffer und anderen Fensterressourcen. Beachten Sie insbesondere, dass das [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)-Factorymuster zum Erfassen der Grafikressourcen verwendet wird, einschließlich der Swapchain (eine Gruppe von Framepuffern). Da DXGI der Besitzer der Swapchain ist, wird die [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)-Schnittstelle verwendet, um Frames auf dem Bildschirm darzustellen. |
| [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D ist der Satz von APIs, die eine virtuelle Darstellung der Grafikschnittstelle bereitstellen und es Ihnen ermöglichen, damit Grafiken zu zeichnen. Version 11 ist von den Funktionen her in etwa mit OpenGL 4.3 vergleichbar. (OpenGL ES .0 ähnelt hinsichtlich der Funktionen dagegen DirectX9 und OpenGL 2.0, verwendet aber die einheitliche Shaderpipeline von OpenGL 3.0.) Der Großteil der Arbeit wird mit den ID3D11Device1- und ID3D11DeviceContext1-Schnittstellen erledigt, die Zugriff auf einzelne Ressourcen und Unterressourcen bzw. den Renderkontext bieten.                                                                                                                                          |
| [Direct2D](/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D stellt einen Satz von APIs für GPU-beschleunigtes 2D-Rendering bereit. Hinsichtlich des Verwendungszwecks ist es mit OpenVG vergleichbar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite stellt einen Satz von APIs für GPU-beschleunigtes Schriftartrendering in hoher Qualität bereit.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath stellt einen Satz von APIs und Makros für die Behandlung allgemeiner linearer Algebra sowie trigonometrischer Typen, Werte und Funktionen bereit. Diese Typen und Funktionen sind so konzipiert, dass sie gut mit Direct3D und den zugehörigen Shadervorgängen funktionieren.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Die von Direct3D-Shadern verwendete aktuelle HLSL-Syntax. Sie implementiert das Direct3D-Shadermodell 5.0.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Überprüfen der Windows-Runtime-APIs und -Vorlagenbibliothek


Die Windows-Runtime-APIs stellen die allgemeine Infrastruktur für UWP-Apps bereit. Weitere Informationen finden [hier](/uwp/api/).

Folgende wichtige Windows-Runtime-APIs werden beim Portieren Ihrer Grafikpipeline verwendet:

-   [**Windows:: UI:: Core:: corewindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows:: UI:: Core:: coredispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

Die C++-Vorlagenbibliothek für Windows-Runtime (WRL) ist eine Vorlagenbibliothek, die eine einfache Methode zum Erstellen und Verwenden von Komponenten für Windows-Runtime bietet. Die Direct3D 11-APIs für UWP-Apps lassen sich am besten in Verbindung mit den Schnittstellen und Typen in dieser Bibliothek wie etwa intelligenten Zeigern ([ComPtr](/cpp/windows/comptr-class)) verwenden. Weitere Informationen zur WRL finden Sie unter [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl).

## <a name="change-your-coordinate-system"></a>Ändern des Koordinatensystems


Ein Unterschied, der bei der Portierung anfänglich für Verwirrung sorgen kann, ist die Umstellung vom traditionellen rechtshändigen Koordinatensystem von OpenGL auf das standardmäßige linkshändige Koordinatensystem von Direct3D. Diese Änderung der Koordinatenmodellierung wirkt sich auf viele Teile des Spiels aus – angefangen bei der Einrichtung und Konfiguration der Scheitelpunktpuffer bis hin zu vielen der mathematischen Matrixfunktionen. Die folgenden zwei Änderungen sind am wichtigsten:

-   Spiegeln Sie die Reihenfolge von Dreieckscheitelpunkten, sodass sie von Direct3D von vorne im Uhrzeigersinn durchlaufen werden. Wenn die Scheitelpunkte in der OpenGL-Pipeline beispielsweise als 0, 1 und 2 indiziert wurden, übergeben Sie sie stattdessen als 0, 2, 1 an Direct3D.
-   Verwenden Sie die Ansichtsmatrix, um den Raum um -1.0f in der z-Richtung zu skalieren, sodass die z-Achsenkoordinaten umgekehrt werden. Kehren Sie dazu das Vorzeichen der Werte an den Positionen M31, M32 und M33 in der Ansichtsmatrix um (beim Portieren in den [**Matrix**](/windows/desktop/direct3d9/matrix4x4)-Typ). Falls M34 nicht 0 ist, kehren Sie das Vorzeichen dieser Position ebenfalls um.

Direct3D kann jedoch ein rechtshändiges Koordinatensystem unterstützen. DirectXMath bietet eine Reihe von Funktionen, die sowohl für links- als auch für rechtshändige Koordinatensysteme funktionieren. Mithilfe dieser Funktionen können Sie einen Teil Ihrer ursprünglichen Gitterdaten und der Matrixverarbeitung beibehalten. Dazu gehören:

| DirectXMath-Matrixfunktion                                                   | BESCHREIBUNG                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.       |
| [**XMMatrixLookAtRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einem Fokus.      |
| [**XMMatrixLookToLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | Erstellt eine Ansichtsmatrix für ein linkshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung.  |
| [**XMMatrixLookToRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | Erstellt eine Ansichtsmatrix für ein rechtshändiges Koordinatensystem mit einer Kameraposition, einer Aufwärtsrichtung und einer Kamerarichtung. |
| [**XMMatrixOrthographicLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | Erstellt eine orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein linkshändiges Koordinatensystem.                                           |
| [**XMMatrixOrthographicOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | Erstellt eine benutzerdefinierte orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                          |
| [**XMMatrixOrthographicRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | Erstellt eine orthogonale Projektionsmatrix für ein rechtshändiges Koordinatensystem.                                                |
| [**XMMatrixPerspectiveFovLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | Erstellt eine linkshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                                |
| [**XMMatrixPerspectiveFovRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | Erstellt eine rechtshändige perspektivische Projektionsmatrix auf der Grundlage eines Sichtfelds.                                               |
| [**XMMatrixPerspectiveLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | Erstellt eine linkshändige perspektivische Projektionsmatrix.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | Erstellt eine benutzerdefinierte Version einer linkshändigen perspektivischen Projektionsmatrix.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | Erstellt eine benutzerdefinierte Version einer rechtshändigen perspektivischen Projektionsmatrix.                                                    |
| [**XMMatrixPerspectiveRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | Erstellt eine rechtshändige perspektivische Projektionsmatrix.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>Häufig gestellte Fragen zur Portierung von OpenGL ES 2.0 zu Direct3D 11


-   Frage: Kann ich generell nach bestimmten Zeichenfolgen oder Mustern in meinem OpenGL-Code suchen und sie durch die Direct3D-Entsprechungen ersetzen?
-   Antwort: Nein. OpenGL ES 2.0 und Direct3D 11 stammen aus verschiedenen Generationen der Grafikpipelinemodellierung. Obwohl es Ähnlichkeiten zwischen den Konzepten und APIs gibt (z. B. der Renderkontext und die Instanziierung von Shadern), sollten Sie sowohl diesen Leitfaden als auch die Direct3D 11-Referenz lesen, um beim Neuerstellen Ihrer Pipeline die beste Wahl treffen zu können. Eine 1:1-Zuordnung ist nicht zu empfehlen. Wenn Sie aber von GLSL zu HLSL portieren, kann das Erstellen eines allgemeinen Satzes von Aliasen für Variablen, systeminterne Elemente und Funktionen von GLSL nicht nur die Portierung vereinfachen, sondern es bietet Ihnen auch die Möglichkeit, mit einem einzigen Satz von Shadercodedateien zu arbeiten.

 

 