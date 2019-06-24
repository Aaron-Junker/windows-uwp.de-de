---
title: Vergleichen der OpenGL ES 2.0-Shaderpipeline mit Direct3D
description: Vom Konzept her ist die Direct3D 11-Shaderpipeline der in OpenGL ES 2.0 sehr ähnlich.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, OpenGL, Direct3D, Shaderpipeline
ms.localizationpriority: medium
ms.openlocfilehash: fc5e1eb9c261a4397d83c833591f2497521aa1c6
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321382"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Vergleichen der OpenGL ES 2.0-Shaderpipeline mit Direct3D




**Wichtige APIs**

-   [Eingabe-Assembler-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [Vertex-Shader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))
-   [Pixel-Shader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))

Vom Konzept her ist die Direct3D 11-Shaderpipeline der in OpenGL ES 2.0 sehr ähnlich. Hinsichtlich des API-Entwurfs sind die Hauptkomponenten für die Erstellung und Verwaltung der Shaderstufen jedoch Teile der zwei primären Schnittstellen [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) und [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). In diesem Thema versuchen wir, allgemeine Muster der OpenGL ES 2.0-Shaderpipeline-API den Direct3D 11-Entsprechungen in diesen Schnittstellen zuzuordnen.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Die Direct3D 11-Shaderpipeline


Die Shaderobjekte werden mit Methoden der [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)-Schnittstelle wie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) erstellt.

Die Direct3D 11-Grafikpipeline wird von Instanzen der [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)-Schnittstelle verwaltet und umfasst die folgenden Stufen:

-   [Eingabe-Assembler-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). Die Eingabe-Assembler-Stufe stellt Daten (Dreiecke, Linien und Punkte) für die Pipeline bereit. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) Methoden, die dieser Phase unterstützen "IA" vorangestellt.
-   [Vertex-Shader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Die Vertex-Shader-Stufe verarbeitet Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung aus. Ein Vertex-Shader verarbeitet immer einen einzigen Eingabescheitelpunkt und erzeugt daraus einen einzigen Ausgabescheitelpunkt. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) Methoden, die dieser Phase unterstützen "Im Vergleich" vorangestellt.
-   [Datenstrom-Ausgabe-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage). Die Datenstrom-Ausgabe-Stufe streamt Grundtypdaten auf dem Weg zum Rasterizer aus der Pipeline in den Arbeitsspeicher. Daten können "ausgestreamt" und/oder in den Rasterizer übergeben werden. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten wieder der Pipeline zugeführt oder von der CPU eingelesen werden. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) Methoden, die dieser Phase unterstützen "Usw." vorangestellt.
-   [Rasterizer-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage). Der Rasterizer beschneidet Grundtypen, bereitet Grundtypen für den Pixelshader vor und bestimmt, wie Pixelshader aufgerufen werden. Sie können die Rasterung Deaktivieren von wird, dass der Pipeline gibt es kein PixelShader (Legen Sie die Pixel-Shader-Stufe, auf NULL mit [ **ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)), und Deaktivieren von Tiefe und Schablone (testen Legen Sie DepthEnable und StencilEnable auf "false" in [ **D3D11\_Tiefe\_SCHABLONE\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)). Wenn die Rasterung deaktiviert ist, werden damit zusammenhängende Pipelinezähler nicht aktualisiert.
-   [Pixelshader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Die Pixelshader-Stufe empfängt interpolierte Daten für einen Grundtyp und generiert Pro-Pixel-Daten (z. B. die Farbe). [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) Methoden, die Unterstützung dieser Phase haben das Präfix "PS".
-   [Ausgabezusammenführungs-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). Die Ausgabezusammenführungs-Stufe kombiniert verschiedene Ausgabedaten (Pixelshaderwerte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren. [**ID3D11DeviceContext1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) Methoden, die dieser Phase unterstützen "OM" vorangestellt.

(Es gibt auch Stufen für Geometry-Shader, Hull-Shader, Tesselator und Domain-Shader. Diese haben aber keine Äquivalente in OpenGL ES 2.0 und werden hier daher nicht behandelt.) Eine vollständige Liste der Methoden für diese Stufen finden Sie auf den Referenzseiten zu [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) und [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). **ID3D11DeviceContext1** erweitert **ID3D11DeviceContext** für Direct3D 11.

## <a name="creating-a-shader"></a>Erstellen eines Shaders


In Direct3D werden Shaderressourcen nicht vor dem Kompilieren und Laden erstellt. Stattdessen wird die Ressource beim Laden der HLSL-Datei erstellt. Daher besteht keine direkte Analog Funktion zur GlCreateShader, die ein initialisiertes Shaderressource eines bestimmten Typs erstellt (z. B. GL\_VERTEX\_Shader- oder GL\_FRAGMENT\_SHADER). Shader werden stattdessen nach dem Laden der HLSL-Datei mit spezifischen Funktionen wie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) erstellt, für die der Typ und die kompilierte HLSL-Datei als Parameter verwendet werden.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Nachdem das kompilierte Shaderobjekt (Compiled Shader Object, CSO) geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) auf. Übergeben Sie die CSO-Datei dabei als Puffer. |

 

## <a name="compiling-a-shader"></a>Kompilieren eines Shaders


Direct3D-Shader müssen in UWP-Apps (Universelle Windows-Plattform) als CSO-Dateien vorkompiliert und mit einer der Windows-Runtime-Datei-APIs geladen werden. (Desktop-apps können die Shader aus Textdateien oder Zeichenfolge zur Laufzeit Kompilieren). Die CSO-Dateien werden aus .hlsl Dateien erstellt, die Teil des Microsoft Visual Studio-Projekts und behalten die gleichen Namen, nur mit der Erweiterung .cso. Stellen Sie sicher, dass Sie in Ihrem gelieferten Paket enthalten sind!

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | Nicht zutreffend. Kompilieren Sie die Shader in Visual Studio in CSO-Dateien, und fügen Sie sie Ihrem Paket hinzu.                                                                                     |
| Verwenden von "glGetShaderiv" für den Kompilierungsstatus | Nicht zutreffend. Überprüfen Sie, ob die Kompilierungsausgabe des FX-Compilers (FXC) von Visual Studio Fehler enthält. Bei erfolgreicher Kompilierung wird eine entsprechende CSO-Datei erstellt. |

 

## <a name="loading-a-shader"></a>Laden eines Shaders


Wie im Abschnitt zum Erstellen eines Shaders erwähnt, erstellt Direct3D 11 den Shader, wenn die entsprechende CSO-Datei in einen Puffer geladen und an eine der Methoden in der folgenden Tabelle übergeben wird.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Nachdem das kompilierte Shaderobjekt geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) auf. |

 

## <a name="setting-up-the-pipeline"></a>Einrichten der Pipeline


In OpenGL ES 2.0 wird das "Shaderprogrammobjekt" verwendet, das mehrere Shader für die Ausführung enthält. Einzelne Shader werden an das Shaderprogrammobjekt angefügt. In Direct3D 11 arbeiten Sie jedoch direkt mit dem Renderkontext ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) und erstellen Shader für den Kontext.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glLinkProgram   | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glUseProgram    | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glGetProgramiv  | Verwenden Sie den erstellten Verweis auf [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). |

 

Erstellen Sie mit der statischen [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice)-Methode eine Instanz von [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) und [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2).

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>Festlegen der Viewports


Das Festlegen eines Viewports funktioniert in Direct3D 11 und OpenGL ES 2.0 ähnlich. Rufen Sie in Direct3D 11, [ **ID3D11DeviceContext::RSSetViewports** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) mit einem konfigurierten [ **CD3D11\_VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)).

Direct3D 11: Wenn einen Viewport an.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11\_VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [**ID3D11DeviceContext::RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>Konfigurieren der Vertex-Shader


Die Konfiguration eines Vertex-Shaders erfolgt in Direct3D 11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1) übergeben.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>Konfigurieren der Pixelshader


Die Konfiguration eines Pixelshaders erfolgt in Direct3D 11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::PSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1) übergeben.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>Generieren der endgültigen Ergebnisse


Wenn die Pipeline abgeschlossen ist, zeichnen Sie die Ergebnisse der Shaderstufen in den Hintergrundpuffer. In Direct3D 11 wird dazu wie in OpenGL ES 2.0 ein Zeichnen-Befehl aufgerufen, um die Ergebnisse als Farbzuordnung in den Hintergrundpuffer auszugeben. Dieser Hintergrundpuffer wird anschließend an die Anzeige gesendet.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [ **ID3D11DeviceContext1::DrawIndexed** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (oder andere Draw\* Methoden [  **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Portieren von GLSL zu HLSL


GLSL und HLSL unterscheiden sich abgesehen von der Unterstützung komplexer Typen und einiger allgemeiner Syntaxteile nur wenig. Für viele Entwickler besteht die einfachste Methode für die Portierung zwischen den beiden Programmiersprachen darin, Aliase für allgemeine OpenGL ES 2.0-Anweisungen und -Definitionen zu erstellen und sie ihrem HLSL-Äquivalent zuzuordnen. Beachten Sie, dass Direct3D die Shadermodellversion verwendet, um die von einer Grafikschnittstelle unterstützte HLSL-Funktionsgruppe darzustellen. In OpenGL wird eine andere Versionsspezifikation für HLSL verwendet. Die folgende Tabelle soll Ihnen – in Bezug auf die Version – eine ungefähre Vorstellung von den für Direct3D 11 und OpenGL ES 2.0 definierten Funktionsgruppen der Shaderprogrammiersprachen geben.

| Shaderprogrammiersprache           | GLSL-Funktionsversion                                                                                                                                                                                                      | Direct3D-Shadermodell |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES für OpenGL ES 2.0 | 1.40. Ältere Implementierungen von GLSL ES für OpenGL ES 2.0 verwenden möglicherweise die Versionen 1.10 bis 1.30. Überprüfen Sie den ursprünglichen Code mit GlGetString (GL\_SCHATTIERUNG\_Sprache\_VERSION) oder GlGetString (SCHATTIERUNG\_Sprache\_VERSION) fest. | ~SM 2.0               |

 

Weitere Informationen zu den Unterschieden zwischen den beiden Shaderprogrammiersprachen und allgemeine Syntaxzuordnungen finden Sie in der [GLSL-zu-HLSL-Referenz](glsl-to-hlsl-reference.md).

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Portieren der systeminternen OpenGL-Funktionen zu HLSL-Semantik


Bei der Direct3D 11-HLSL-Semantik handelt es sich um Zeichenfolgen, die wie eine Uniform-Variable oder ein Attributname zum Identifizieren eines zwischen der App und einem Shaderprogramm übergebenen Werts verwendet werden. Obwohl viele verschiedene Zeichenfolgen möglich sind, empfiehlt es sich, eine Zeichenfolge wie POSITION oder COLOR zu verwenden, aus der der Verwendungszweck hervorgeht. Sie weisen diese Semantik zu, wenn Sie einen Konstantenpuffer oder ein Puffereingabelayout erstellen. Sie können auch eine Zahl zwischen 0 und 7 an die Semantik anfügen, wenn Sie separate Register für ähnliche Werte verwenden möchten. Zum Beispiel: FARBE0, COLOR1, COLOR2...

Semantik, die mit dem Präfix "SV\_" Wertsemantik System sind, die in der von Ihrem shaderprogramm geschrieben werden; der app selbst (die auf der CPU ausgeführt wird) nicht geändert werden können. In der Regel enthält diese Semantik Werte, die Ein- oder Ausgaben einer anderen Shaderstufe in der Grafikpipeline sind oder komplett von der GPU generiert werden.

Darüber hinaus SV\_ Semantik haben unterschiedliche Verhaltensweisen auf, wenn sie verwendet werden, geben Sie eine Eingabe, oder die Ausgabe einer Shader-Stufe. Z. B. SV\_POSITION (Ausgabe) enthält die Vertexdaten, die während der Vertexshader-Stufe und SV transformiert\_POSITION (Eingabe) enthält die Position Pixelwerte während der Rasterung interpoliert.

Die folgende Tabelle enthält einige Zuordnungen für allgemeine systeminterne OpenGL ES 2.0-Shaderfunktionen:

| OpenGL-Systemwert | Verwenden Sie diese HLSL-Semantik:                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_Position        | POSITION(n) für Scheitelpunkt-Pufferdaten. SV\_POSITION bietet eine Pixelposition, an dem PixelShader und kann nicht geschrieben werden, von Ihrer app.                                        |
| gl\_Normal          | NORMAL(n) für vom Scheitelpunktpuffer bereitgestellte Normalendaten.                                                                                                                 |
| gl\_TexCoord\[n\]   | TEXCOORD(n) für Textur-UV-Koordinatendaten (ST in manchen OpenGL-Dokumentationen), die an einen Shader übergeben werden.                                                                       |
| gl\_FragColor       | COLOR(n) für RGBA-Farbdaten, die an einen Shader übergeben werden. Diese Daten werden wie Koordinatendaten behandelt. Durch die Semantik können Sie nur leichter erkennen, dass es sich um Farbdaten handelt. |
| gl\_FragData\[n\]   | SV\_Ziel\[n\] zum Schreiben von einem Pixel-Shader auf eine Textur oder andere pixelpuffer.                                                                               |

 

Die Methode zum Codieren der Semantik unterscheidet sich von der Verwendung systeminterner Funktionen in OpenGL ES 2.0. In OpenGL können Sie auf viele der systeminternen Funktionen direkt ohne Konfiguration oder Deklaration zugreifen. In Direct3D müssen Sie ein Feld in einem bestimmten Konstantenpuffer deklarieren, um eine bestimmte Semantik verwenden zu können, oder es als Rückgabewert für die **main()** -Methode eines Shaders deklarieren.

Das folgende Beispiel zeigt die Verwendung einer Semantik in einer Konstantenpufferdefinition:

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Der Code definiert ein Paar einfacher Konstantenpuffer.

Das folgende Beispiel zeigt die Verwendung einer Semantik zum Definieren des Rückgabewerts eines Fragment-Shaders:

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

In diesem Fall SV\_Ziel ist der Speicherort des Renderziels, die in die Pixelfarbe (definiert als die eines Vektors mit vier Gleitkommawerten) geschrieben werden, wenn der Shader Ausführung abgeschlossen ist.

Ausführliche Informationen zur Verwendung der Semantik mit Direct3D finden Sie unter [HLSL-Semantik](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

 

 




