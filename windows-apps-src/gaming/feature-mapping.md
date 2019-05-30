---
title: Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs
description: Erfahren Sie, wie die Features Ihres Direct3D 9-Spiels zu Direct3D 11 und zur Universellen Windows-Plattform (UWP) zugeordnet werden.
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX 9, DirectX 11, Portierung
ms.localizationpriority: medium
ms.openlocfilehash: 51bc293a779a96db75ce83da68cb3beea54b9618
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368727"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Zuordnung von DirectX 9-Funktionen zu DirectX 11-APIs



**Zusammenfassung**

-   [Planen Sie Ihren DirectX-port](plan-your-directx-port.md)
-   [Wichtige Änderungen von Direct3D 9 zu Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Featurezuordnung


Erfahren Sie, wie die Features Ihres Direct3D 9-Spiels zu Direct3D 11 und zur Universellen Windows-Plattform (UWP) zugeordnet werden.

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Zuordnen von Direct3D 9-Features zu DirectX 11-APIs


[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) ist nach wie vor die Grundlage von DirectX-Grafiken; die API wurde seit DirectX 9 jedoch geändert:

-   Zum Einrichten von Grafikadaptern wird die Microsoft DirectX Graphics Infrastructure (DXGI) verwendet. Verwenden Sie [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi) zum Auswählen von Pufferformaten, Erstellen von Swapchains, Darstellen von Frames und Erstellen freigegebener Ressourcen. Siehe [Übersicht über DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/d3d10-graphics-programming-guide-dxgi).
-   Ein Direct3D-Gerätekontext wird zum Festlegen des Pipelinestatus und Generieren von Renderbefehlen verwendet. In den meisten unserer Beispiele wird ein unmittelbarer Kontext verwendet, um direkt auf dem Gerät zu rendern. Direct3D 11 unterstützt auch das Multithread-Rendering, wobei dann verzögerte Kontexte verwendet werden. Siehe [Einführung in ein Gerät in Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro).
-   Einige Funktionen sind veraltet und nicht mehr verfügbar. Zu beachten ist insbesondere, dass es die Pipeline mit fester Funktion nicht mehr gibt. Siehe [Veraltete Funktionen](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

Eine vollständige Liste der Direct3D 11-Funktionen finden Sie unter [Features von Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features) und [Features von Direct3D 11.1](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features).

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Umstellung von Direct2D 9 auf Direct2D 11


[Direct2D (Windows)](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) ist weiterhin ein wichtiger Bestandteil von DirectX-Grafiken und Windows. Sie können mit Direct2D weiterhin 2D-Spiele und Direct3D-basierte Überlagerungen (HUDs) zeichnen.

Direct2D baut auf Direct3D auf. 2D-Spiele können mit beiden APIs implementiert werden. Ein mit Direct3D implementiertes 2D-Spiel kann z. B. die orthografische Projektion verwenden, Z-Werte zum Steuern der Zeichnungsreihenfolge von Grundtypen festlegen und mit Pixelshadern Spezialeffekte hinzufügen.

Da Direct2D auf Direct3D basiert, verwendet es ebenfalls DXGI und Gerätekontexte. Siehe [Übersicht über die Direct2D-API](https://docs.microsoft.com/windows/desktop/Direct2D/the-direct2d-api).

Die [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal)-API bietet Unterstützung für formatierten Text mit Direct2D. Siehe [Einführung in DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/introducing-directwrite).

## <a name="replace-deprecated-helper-libraries"></a>Ersetzen veralteter Hilfsbibliotheken


D3DX und DXUT sind veraltet und können nicht in UWP-Spielen verwendet werden. Von diesen Hilfsbibliotheken wurden Ressourcen für Aufgaben wie das Laden von Texturen und Gittern bereitgestellt.

-   In der exemplarischen Vorgehensweise [Einfache Portierung von Direct3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) wird gezeigt, wie ein Fenster eingerichtet, Direct3D initialisiert und einfaches 3D-Rendering durchgeführt wird.
-   In der exemplarischen Vorgehensweise [Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-uwp-directx-game.md) werden allgemeine Aufgaben der Spielprogrammierung erläutert, u. a. Grafiken, Laden von Dateien, UI, Steuerelemente und Sound.
-   Das Communityprojekt [DirectX-Toolkit](https://go.microsoft.com/fwlink/p/?LinkID=248929) bietet Hilfsklassen zur Verwendung mit Direct3D 11 und UWP-Apps.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>Umstellung von Shaderprogrammen von FX auf HLSL


Die D3DX-Hilfsprogrammbibliothek (D3DX 9, D3DX 10 und D3DX 11), einschließlich Effects, ist veraltet und für UWP nicht mehr verfügbar. Alle DirectX-Spiele für UWP steuern die Grafikpipeline mit [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl) ohne Effects.

Visual Studio verwendet FXC weiterhin im Hintergrund zum Kompilieren von Shaderobjekten. UWP-Spielshader werden vorab kompiliert. Der Bytecode wird zur Laufzeit geladen. Anschließend wird jede Shaderressource während des entsprechenden Renderingdurchgangs an die Grafikpipeline gebunden. Shader sollten in eigene separate HLSL-Dateien verschoben werden, und Renderingtechniken sollten im C++-Code implementiert werden.

Einen kurzen Überblick über das Laden von Shaderressourcen finden Sie unter [Einfache Portierung von Direct3D 9 zu UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

Direct3D 11 eingeführte Shader Model 5, die Direct3D-Funktionsebene 11 erfordert\_0 (oder höher). Siehe [Funktionen von HLSL-Shadermodell 5 für Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3dhlsl/overviews-direct3d-11-hlsl).

## <a name="replace-xnamath-and-d3dxmath"></a>Ersetzen von XNAMath und D3DXMath


Code mit XNAMath (oder D3DXMath) sollte zu [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) migriert werden. DirectXMath enthält Typen, die zwischen x86, x64 und ARM portierbar sind. Siehe [Codemigration von der XNAMath-Bibliothek](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration).

Beachten Sie, dass sich Float-Typen von DirectXMath zur Verwendung mit Shadern eignen. Mit [**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) und [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) können z. B. auf einfache Weise Daten für Konstantenpuffer ausgerichtet werden.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>Ersetzen von DirectSound durch XAudio2 (und Hintergrundaudio)


DirectSound wird für UWP nicht unterstützt:

-   Verwenden Sie [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal), um Ihrem Spiel Soundeffekte hinzuzufügen.

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>Ersetzen von DirectInput durch XInput und UWP-APIs


DirectInput wird für UWP nicht unterstützt:

-   Verwenden Sie [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)-Eingabeereignisrückrufe für Maus-, Tastatur- und Toucheingabe.
-   Verwenden Sie [XInput](https://docs.microsoft.com/windows/desktop/xinput/getting-started-with-xinput) 1.4 zur Unterstützung von Gamecontrollern (und Gamecontroller-Headsets). Wenn Sie eine freigegebene Codebasis für Desktop und UWP verwenden, finden Sie unter [XInput-Versionen](https://docs.microsoft.com/windows/desktop/xinput/xinput-versions) Informationen zur Abwärtskompatibilität.
-   Registrieren Sie [**EdgeGesture**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.EdgeGesture)-Ereignisse, wenn die App-Leiste in Ihrem Spiel verwendet werden muss.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>Verwenden von Microsoft Media Foundation anstelle von DirectShow


DirectShow ist nicht mehr in der DirectX-API (oder der Windows-API) enthalten. [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) stellt Videoinhalte mithilfe von freigegebenen Oberflächen für Direct3D bereit. Siehe [Direct3D 11-Video-APIs](https://docs.microsoft.com/windows/desktop/medfound/direct3d-11-video-apis).

## <a name="replace-directplay-with-networking-code"></a>Ersetzen von DirectPlay durch Netzwerkcode


Microsoft DirectPlay ist veraltet und nicht mehr verfügbar. Falls Ihr Spiel Netzwerkdienste verwendet, müssen Sie Netzwerkcode bereitstellen, der den UWP-Anforderungen entspricht. Verwenden Sie die folgenden APIs:

-   [Win32 und COM für UWP-apps (Netzwerk) (Windows)](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)
-   [**Windows.Networking-Namespace: (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking)
-   [**Windows.Networking.Sockets-Namespace (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)
-   [**Windows.Networking.Connectivity-Namespace (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity)
-   [**Windows.ApplicationModel.Background-Namespace (Windows)** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

In den folgenden Artikeln finden Sie Informationen zum Hinzufügen von Netzwerkfunktionen und Deklarieren der Netzwerkunterstützung im Paketmanifest Ihrer App.

-   [Herstellen einer Verbindung mit Sockets (UWP-apps mit C#/VB-/C++ und XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
-   [Herstellen einer Verbindung mit WebSockets (UWP-apps mit C#/VB-/C++ und XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh994396(v=win.10))
-   [Verbinden mit Webdiensten (UWP-apps mit C#/VB-/C++ und XAML) (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
-   [Grundlagen zum Netzwerk](https://docs.microsoft.com/windows/uwp/networking/networking-basics)

Beachten Sie, dass alle UWP-Apps (einschließlich Spiele) bestimmte Hintergrundaufgaben verwenden, um die Verbindung aufrechtzuerhalten, während die App angehalten ist. Wenn Ihr Spiel den Verbindungsstatus aufrechterhalten muss, während es angehalten ist, gehen Sie wie unter [Grundlagen zum Netzwerk](https://docs.microsoft.com/windows/uwp/networking/networking-basics) beschrieben vor.

## <a name="function-mapping"></a>Funktionszuordnung


Ziehen Sie beim Konvertieren von Code von Direct3D 9 in Direct3D 11 die folgende Tabelle zurate. Diese Informationen können Ihnen auch die Unterscheidung zwischen Gerät und Gerätekontext erleichtern.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Direct3D 11-Entsprechung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3ddevice9">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2">ID3D11Device2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11devicecontext2">ID3D11DeviceContext2</a></p>
<p>Die Grafikpipelinestufen werden unter <a href="https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline">Grafikpipeline</a> erläutert.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2">IDXGIFactory2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2">IDXGIAdapter2</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-present">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-testcooperativelevel">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>Rufen Sie <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1:: Present1</a> mit der festgelegten DXGI_PRESENT_TEST-Kennung auf.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dbasetexture9">IDirect3DBaseTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dtexture9">IDirect3DTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dcubetexture9">IDirect3DCubeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvolumetexture9">IDirect3DVolumeTexture9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dindexbuffer9">IDirect3DIndexBuffer9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexbuffer9">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer">ID3D11Buffer</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture1d">ID3D11Texture1D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d">ID3D11Texture2D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture3d">ID3D11Texture3D</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview">ID3D11ShaderResourceView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview">ID3D11RenderTargetView</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexshader9">IDirect3DVertexShader9</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dpixelshader9">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader">ID3D11VertexShader</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3dvertexdeclaration9">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setrenderstate">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setsamplerstate">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11blendstate1">ID3D11BlendState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilstate">ID3D11DepthStencilState</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11rasterizerstate1">ID3D11RasterizerState1</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11samplerstate">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitive">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitive">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexedinstanced">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawinstanced">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetprimitivetopology">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawauto">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-beginscene">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-endscene">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawprimitiveup">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawindexedprimitiveup">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>Keine direkte Entsprechung</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-showcursor">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorposition">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setcursorproperties">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>Verwenden Sie standardmäßige Cursor-APIs.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-reset">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>"LOST device" und POOL_MANAGED sind nicht mehr verfügbar. <a href="https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1">IDXGISwapChain1::Present1</a> kann mit einem <a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error">DXGI_ERROR_DEVICE_REMOVED</a> Rückgabewert fehlschlagen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawrectpatch">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-drawtripatch">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-lightenable">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-multiplytransform">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/direct3d9/id3dxeffectstatemanager--setlight">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setmaterial">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-setnpatchmode">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settransform">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-setfvf">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9helper/nf-d3d9helper-idirect3ddevice9-settexturestagestate">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>Die Pipeline mit fester Funktion ist veraltet und nicht mehr verfügbar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdepthstencilmatch">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-checkdeviceformat">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-getdevicecaps">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3ddevice9-validatedevice">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>Funktionsbits werden durch Funktionsebenen ersetzt. Nur einige wenige Format- und Funktionsverwendungsfälle sind für alle Funktionsebenen optional. Diese können mit <a href="https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport">ID3D11Device::CheckFeatureSupport</a> und <a href="https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-checkformatsupport">ID3D11Device::CheckFormatSupport</a> überprüft werden.</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>Oberflächenformatzuordnung


Ziehen Sie beim Konvertieren von Direct3D 9-Formaten in DXGI-Formate die folgende Tabelle heran.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D 9-Format</th>
<th align="left">Direct3D 11-Format</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    verwenden r Swizzle im Shader duplizieren Rot an andere Komponenten, um Direct3D 9-Verhalten zu erzielen.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    Swizzle .rrrg im Shader verwenden, Duplizieren von Rot, und verschieben Grün für die alpha-Komponenten, die Direct3D 9-Verhalten zu erzielen.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    In Direct3D 9 die Daten wurde vertikal skalieren, indem 255.0f, aber dies können Sie auch in den Shader verarbeitet werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    In Direct3D 9 die Daten wurde vertikal skalieren, indem 255.0f, aber dies können Sie auch in den Shader verarbeitet werden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>Beachten Sie</strong>    DXT1 und für DXT2 sind aus Sicht der API/Hardware identisch. Der einzige Unterschied besteht darin, ob prämultipliziertes Alpha verwendet wird, was von einer App nachverfolgt werden kann und kein separates Format erfordert.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>Beachten Sie</strong>    für DXT3 und für DXT4 sind aus Sicht der API/Hardware identisch. Der einzige Unterschied besteht darin, ob prämultipliziertes Alpha verwendet wird, was von einer App nachverfolgt werden kann und kein separates Format erfordert.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM & DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 & D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    verwenden r Swizzle im Shader duplizieren Rot an andere Komponenten um D3D9 Verhalten zu erzielen.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>Beachten Sie</strong>    des Shaders ruft UINT-Werten, aber float-Elemente sind erforderlich, wenn Direct3D 9 ganzzahligen formatieren (0, 0F, 1. 0f... 255.f), "uint" nur in den float32 im Shader konvertiert werden kann.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Beachten Sie</strong>    des Shaders ruft St.-Werte ab, aber wenn Direct3D 9 Stil ganzzahligen Gleitkommawerte erforderlich sind, SINT kann nur konvertiert werden, float32 im Shader.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Beachten Sie</strong>    des Shaders ruft St.-Werte ab, aber wenn Direct3D 9 Stil ganzzahligen Gleitkommawerte erforderlich sind, SINT kann nur konvertiert werden, float32 im Shader.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>Nicht verfügbar</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    erfordert Funktionsebene 10.0 oder höher
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Beachten Sie</strong>    erfordert Funktionsebene 10.0 oder höher
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




