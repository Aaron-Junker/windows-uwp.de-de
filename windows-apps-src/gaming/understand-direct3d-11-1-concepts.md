---
title: Wichtige Änderungen beim Wechsel von Direct3D 9 zu Direct3D 11
description: In diesem Thema werden allgemeine Unterschiede zwischen DirectX 9 und DirectX 11 erläutert.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Direct3D 9, Direct3D 11, Änderungen
ms.localizationpriority: medium
ms.openlocfilehash: ec70f77bbb124e5bce33f98e9f93b4bfd04e4a35
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168204"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Wichtige Änderungen beim Wechsel von Direct3D 9 zu Direct3D 11



**Zusammenfassung**

-   [Planen der DirectX-Portierung](plan-your-directx-port.md)
-   Wichtige Änderungen beim Wechsel von Direct3D 9 zu Direct3D 11
-   [Featurezuordnung](feature-mapping.md)


In diesem Thema werden allgemeine Unterschiede zwischen DirectX 9 und DirectX 11 erläutert.

Bei Direct3D 11 handelt es sich im Wesentlichen um die gleiche Art von API wie bei Direct3D 9: eine virtualisierte Low-Level-Schnittstelle für Grafikhardware. Sie können damit für viele unterschiedliche Hardwareimplementierungen weiterhin Vorgänge zum Zeichnen von Grafiken durchführen. Das Layout der Grafik-API hat sich seit Direct3D 9 geändert. Das Konzept des Gerätekontexts wurde erweitert, und es wurde eine API speziell für die Grafikinfrastruktur hinzugefügt. Für auf dem Direct3D-Gerät gespeicherte Ressourcen ist ein neuer Mechanismus für Datenpolymorphie (die so genannte Ressourcenansicht) verfügbar.

## <a name="core-api-functions"></a>Wichtige API-Funktionen


In Direct3D 9 mussten Sie eine Schnittstelle für die Direct3D-API erstellen, um sie verwenden zu können. In Direct3D 11-Spielen für die universelle Windows-Plattform (UWP) rufen Sie die statische Funktion [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) auf, um das Gerät und den Gerätekontext zu erstellen.

## <a name="devices-and-device-context"></a>Geräte und Gerätekontext


Ein Direct3D 11-Gerät stellt einen virtualisierten Grafikadapter dar. Dieser wird zum Erstellen von Ressourcen im Videospeicher verwendet, z. B. zum Hochladen von Texturen in die GPU, Erstellen von Ansichten für Texturressourcen und Swapchains und Erstellen von Textursamplern. Eine vollständige Liste der Verwendungsmöglichkeiten für eine Direct3D 11-Geräteschnittstelle finden Sie unter [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) und [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).

Ein Direct3D 11- Gerätekontext wird verwendet, um den Pipelinestatus festzulegen und Renderbefehle zu erzeugen. Von einer Direct3D 11-Renderkette wird beispielsweise ein Gerätekontext verwendet, um die Renderkette einzurichten und die Szene zu zeichnen (siehe unten). Über den Gerätekontext wird auf den Videospeicher zugegriffen (Map), der von den Ressourcen des Direct3D-Geräts verwendet wird. Außerdem werden damit Unterressourcendaten aktualisiert, z. B. Konstantenpufferdaten. Eine vollständige Liste der Verwendungsmöglichkeiten für einen Direct3D 11-Gerätekontext finden Sie unter [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) und [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Beachten Sie, dass für die meisten Beispiele ein unmittelbarer Kontext für das direkte Rendern auf dem Gerät genutzt wird. Von Direct3D 11 werden jedoch auch zurückgestellte Gerätekontexte unterstützt, die hauptsächlich für das Multithreading eingesetzt werden.

In Direct3D 11 werden das Handle für das Gerät und das Handle für den Gerätekontext jeweils per Aufruf von [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) abgerufen. Mit dieser Methode fordern Sie auch einen bestimmten Satz von Hardwarefeatures an und rufen Informationen zu Direct3D-Featureebenen ab, die vom Grafikadapter unterstützt werden. Weitere Informationen zu Geräten, Gerätekontexten und Threadaspekten finden Sie unter [Einführung in ein Gerät in Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro).

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Geräteinfrastruktur, Framepuffer und Renderzielansichten


In Direct3D 11 werden der Geräteadapter und die Hardwarekonfiguration mithilfe der DirectX Graphics Infrastructure-API (DXGI) unter Verwendung der COM-Schnittstellen [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) und [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) festgelegt. Puffer und andere Fensterressourcen (sichtbar oder außerhalb des Bildschirms) werden unter Verwendung spezieller DXGI-Schnittstellen erstellt und konfiguriert. Von der Implementierung des [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)-Factorymusters werden DXGI-Ressourcen (etwa der Framepuffer) abgerufen. Da DXGI für die Swapchain zuständig ist, wird eine DXGI-Schnittstelle zum Darstellen von Frames auf dem Bildschirm verwendet. Weitere Informationen finden Sie unter [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).

Verwenden Sie [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) zum Erstellen einer mit Ihrem Spiel kompatiblen Swapchain. Sie müssen eine Swapchain für ein CoreWindow-Objekt oder für die Komposition (XAML-Interoperabilität) erstellen, anstatt eine Swapchain für ein HWND-Element.

## <a name="device-resources-and-resource-views"></a>Geräteressourcen und Ressourcenansichten


Von Direct3D 11 wird eine zusätzliche Ebene der Polymorphie auf Videospeicherressourcen unterstützt, die als Ansichten bezeichnet werden. Wo vorher ein einzelnes Direct3D 9-Objekt für eine Textur vorhanden war, werden nun zwei Objekte genutzt: die Texturressource, in der die Daten enthalten sind, und die Ressourcenansicht, mit der angegeben wird, wie die Ansicht zum Rendern verwendet wird. Mithilfe einer auf einer Ressource basierenden Ansicht kann diese Ressource für einen bestimmten Zweck verwendet werden. Eine 2D-Texturressource wird als [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)-Element erstellt. Anschließend wird dafür eine Shaderressourcenansicht ([**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)) erstellt, sodass die Verwendung als Textur in einem Shader möglich ist. Außerdem kann eine Renderzielansicht ([**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)) für die gleiche 2D-Texturressource erstellt werden, sodass diese als Zeichenoberfläche verwendet werden kann. In einem anderen Beispiel werden dieselben Pixeldaten in zwei unterschiedlichen Pixelformaten dargestellt, indem zwei separate Ansichten für eine Texturressource genutzt werden.

Die zugrunde liegende Ressource muss mit Eigenschaften erstellt werden, die mit dem Typ der Ansichten kompatibel ist, die auf deren Grundlage erstellt werden. Wenn z. b. ein [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) auf eine Oberfläche angewendet wird, wird diese Oberfläche mit dem [**D3D11 Bind- \_ \_ \_ renderzielflag**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_bind_flag) erstellt. Die Oberfläche muss auch ein mit Rendering kompatibles DXGI-Oberflächen Format aufweisen (siehe [**DXGI- \_ Format**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Die meisten Ressourcen, die Sie zum Rendern verwenden, erben von der [**ID3D11Resource**](/windows/desktop/api/d3d11/nn-d3d11-id3d11resource)-Schnittstelle, die wiederum von [**ID3D11DeviceChild**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild) erbt. Vertexpuffer, Indexpuffer, Konstantenpuffer und Shader sind Direct3D 11-Ressourcen. Eingabelayouts und Samplerzustände erben direkt von [**ID3D11DeviceChild**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild).

In Ressourcen Sichten wird ein Enumerationswert für DXGI- \_ Format verwendet, um das Pixel Format anzugeben. Nicht jede D3DFMT wird als DXGI-Format unterstützt \_ . Beispielsweise gibt es in DXGI kein 24bpp-RGB-Format, das D3DFMT \_ R8G8B8 entspricht. Es gibt auch keine BGR-Entsprechungen für jedes RGB-Format (DXGI \_ \_ -Format R10G10B10A2 \_ unorm entspricht D3DFMT \_ A2B10G10R10, aber es gibt keine direkte Entsprechung zu D3DFMT \_ A2R10G10B10). Planen Sie ein, alle Inhalte in diesen Legacyformaten zur Buildzeit in unterstützte Formate zu konvertieren. Eine umfassende Liste der DXGI-Formate finden Sie in der [**DXGI- \_ formatenumeration**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) .

Direct3D-Geräteressourcen (und Ressourcenansichten) werden erstellt, bevor die Szene gerendert wird. Gerätekontexte werden, wie unten beschrieben, zum Einrichten der Renderkette verwendet.

## <a name="device-context-and-the-rendering-chain"></a>Gerätekontext und die Renderkette


In Direct3D 9 und Direct3D 10.x war ein einzelnes Direct3D-Geräteobjekt vorhanden, mit dem die Erstellung, der Status und das Zeichnen verwaltet wurde. In Direct3D 11 wird die Direct3D-Geräteschnittstelle weiterhin zum Verwalten der Ressourcenerstellung verwendet, aber alle Status- und Zeichenvorgänge werden über einen Direct3D-Gerätekontext abgewickelt. Im Anschluss finden Sie ein Beispiel für die Verwendung des Gerätekontexts ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)-Schnittstelle) zum Einrichten der Renderkette:

-   Festlegen und Löschen von Renderzielansichten (und der Tiefenschablonen-Ansicht)
-   Festlegen des Vertexpuffers, Indexpuffers und Eingabelayouts für die Eingabeassemblerphase (IA-Phase)
-   Binden von Vertex- und Pixelshadern an die Pipeline
-   Binden von Konstantenpuffern an Shader
-   Binden von Texturansichten und Samplern an den Pixelshader
-   Zeichnen der Szene

Wenn eine der [**ID3D11DeviceContext::Draw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw)-Methoden aufgerufen wird, wird die Szene in der Renderzielansicht gezeichnet. Nach Abschluss des Zeichenvorgangs wird der DXGI-Adapter zum Darstellen des abgeschlossenen Frames verwendet, indem [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) aufgerufen wird.

## <a name="state-management"></a>Zustandsverwaltung


In Direct3D 9 wurden Zustandseinstellungen mithilfe einer großen Gruppe individueller Umschaltfunktionen verwaltet, die über die Methoden „SetRenderState“, „SetSamplerState“ und „SetTextureStageState“ festgelegt wurden. Da Direct3D 11 nicht über Unterstützung für die ältere Pipeline mit festen Funktionen verfügt, wird SetTextureStageState durch das Schreiben von Pixelshadern (PS) ersetzt. Es ist kein Äquivalent zu einem Direct3D 9-Statusblock vorhanden. Bei Direct3D 11 wird der Zustand stattdessen mit vier Arten von Zustandsobjekten verwaltet, die ein optimiertes Verfahren zum Gruppieren des Renderzustands ermöglichen.

Anstatt setrenderstate mit D3DRS \_ zenable zu verwenden, erstellen Sie z. b. ein depthstencilstate-Objekt mit diesem und anderen verwandten Zustands Einstellungen und verwenden es, um den Zustand beim Rendern zu ändern.

Beachten Sie beim Portieren von Direct3D 9-Anwendungen zu Statusobjekten, dass die verschiedenen Statuskombinationen als Objekte mit unveränderlichem Status dargestellt werden. Sie sollten einmal erstellt und dann so lange wiederverwendet werden, wie sie gültig sind.

## <a name="direct3d-feature-levels"></a>Direct3D-Featureebenen


Direct3D verfügt über einen neuen Mechanismus zum Bestimmen der Hardwareunterstützung, der die Bezeichnung "Featureebenen" trägt. Featureebenen erleichtern die Ermittlung der Funktionen des Grafikadapters, da Sie einen sorgfältig definierten Satz von GPU-Funktionen anfordern können. Die Funktions \_ Ebene 9 1 implementiert beispielsweise die Funktionen, die von Direct3D 9-Grafikadaptern bereitgestellt werden, einschließlich Shader Model 2. x. Da 9 \_ 1 die geringste Funktionsebene ist, können Sie davon ausgehen, dass alle Geräte einen Vertex-Shader und einen PixelShader unterstützen, die dieselben Stufen wie das programmierbare Shader-Modell Direct3D 9 unterstützen.

Im Spiel wird [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) zum Erstellen des Direct3D-Geräts und -Gerätekontexts verwendet. Wenn Sie diese Funktion aufrufen, geben Sie eine Liste mit Featureebenen an, die vom Spiel unterstützt werden können. Die höchste unterstützte Featureebene der Liste wird zurückgegeben. Wenn Ihr Spiel z. b. BC4/BC5-Texturen (ein Feature von DirectX 10-Hardware) verwenden kann, würden Sie mindestens 9 \_ 1 und 10 \_ 0 in die Liste der unterstützten Funktionsebenen einschließen. Wenn das Spiel auf DirectX 9-Hardware ausgeführt wird und BC4/BC5-Texturen nicht verwendet werden können, gibt [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 9 \_ 1 zurück. Im Spiel kann dann auf ein anderes Texturformat (und kleinere Texturen) zurückgegriffen werden.

Wenn Sie sich dafür entscheiden, für das Direct3D 9-Spiel eine Erweiterung auf die Unterstützung höherer Direct3D-Featureebenen durchzuführen, ist es besser, zuerst das Portieren des vorhandenen Direct3D 9-Grafikcodes abzuschließen. Nachdem das Spiel unter Direct3D 11 funktioniert, können zusätzliche Renderpfade mit erweiterten Grafiken einfacher hinzugefügt werden.

Eine ausführliche Erklärung der Unterstützung von Featureebenen finden Sie unter [Direct3D-Featureebenen](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro). Eine vollständige Liste mit Direct3D 11-Features finden Sie unter [Features von Direct3D 11](/windows/desktop/direct3d11/direct3d-11-features) und [Features von Direct3D 11.1](/windows/desktop/direct3d11/direct3d-11-1-features).

## <a name="feature-levels-and-the-programmable-pipeline"></a>Featureebenen und die programmierbare Pipeline


Seit Direct3D 9 hat sich die Hardware weiterentwickelt, und der programmierbaren Grafikpipeline wurden mehrere neue optionale Phasen hinzugefügt. Die für die Grafikpipeline verfügbaren Optionen variieren je nach Direct3D-Featureebene. Die Featureebene 10.0 enthält die Geometrie-Shaderphase mit optionalem Streamout für das Rendern mit mehreren Durchläufen über die GPU. Funktionsebene 11 \_ 0 umfasst den Hull-Shader und den Domänen-Shader für die Verwendung mit dem Hardware-Mosaik. Die Featureebene 11 \_ 0 umfasst auch die vollständige Unterstützung für DirectCompute-Shader, während die featurestufen 10. x nur die Unterstützung für eine begrenzte Form von DirectCompute einschließen.

Alle Shader sind unter Verwendung von HLSL-Code mit einem Shaderprofil geschrieben, das einer Direct3D-Featureebene entspricht. Shaderprofile sind aufwärts kompatibel, sodass ein HLSL-Shader, der mit vs \_ 4 \_ 0 \_ Level \_ 9 \_ 1 oder PS \_ 4 \_ 0 \_ Level \_ 9 \_ 1 kompiliert wird, auf allen Geräten funktioniert. Shaderprofile sind nicht abwärts kompatibel, daher funktioniert ein mit vs 4 1 kompilierter Shader \_ \_ nur auf Featureebene von 10 \_ 1, 11 \_ 0 oder 11 \_ 1 Geräte.

Unter Direct3D 9 wurden Konstanten für Shader mithilfe eines freigegebenen Arrays mit „SetVertexShaderConstant“ und „SetPixelShaderConstant“ verwaltet. Unter Direct3D 11 werden Konstantenpuffer verwendet, bei denen es sich um Ressourcen handelt, z. B. ein Vertexpuffer oder ein Indexpuffer. Konstantenpuffer sind für eine effiziente Aktualisierung ausgelegt. Anstelle der Anordnung aller Shaderkonstanten in einem einzelnen globalen Array, ordnen Sie die Konstanten in logischen Gruppierungen an und verwalten sie mithilfe eines oder mehrerer Konstantenpuffer. Wenn Sie Ihr Direct3D 9-Spiel zu Direct3D 11 portieren, sollten Sie die Organisation der Konstantenpuffer so planen, dass diese entsprechend aktualisiert werden können. Gruppieren Sie Shaderkonstanten, die nicht für jeden Frame aktualisiert werden, in einem separaten Konstantenpuffer, damit diese Daten nicht ständig zusammen mit den dynamischeren Shaderkonstanten an den Grafikadapter hochgeladen werden müssen.

> **Hinweis**    Die meisten Direct3D 9-Anwendungen haben eine umfangreiche Verwendung von Shadern vorgenommen, aber gelegentlich mit dem Verhalten der Legacy Funktion mit fester Funktionsweise gemischt. Beachten Sie, dass von Direct3D 11 nur ein programmierbares Shadermodell verwendet wird. Die Legacyfeatures von Direct3D 9 mit festen Funktionen werden als veraltet angesehen.

 

 

 