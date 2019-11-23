---
title: Vergleichen der OpenGL ES 2.0-Shaderpipeline mit Direct3D
description: Vom Konzept her ist die Direct3D 11-Shaderpipeline der in OpenGL ES 2.0 sehr ähnlich.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, OpenGL, Direct3D, Shaderpipeline
ms.localizationpriority: medium
ms.openlocfilehash: 7a35102fed9993ca37afa1d1f47850427235ed49
ms.sourcegitcommit: cbd900f350569a3901086a44b2d5007bb6fb7bed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72276292"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Vergleichen der OpenGL ES 2.0-Shaderpipeline mit Direct3D




**Wichtige APIs**

-   [Eingabe-Assembler-Phase](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [Vertex-Shader-Phase](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))
-   [Pixel-Shader-Phase](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))

Vom Konzept her ist die Direct3D 11-Shaderpipeline der in OpenGL ES 2.0 sehr ähnlich. Hinsichtlich des API-Entwurfs sind die Hauptkomponenten für die Erstellung und Verwaltung der Shaderstufen jedoch Teile der zwei primären Schnittstellen [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) und [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). In diesem Thema versuchen wir, allgemeine Muster der OpenGL ES 2.0-Shaderpipeline-API den Direct3D 11-Entsprechungen in diesen Schnittstellen zuzuordnen.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Die Direct3D 11-Shaderpipeline


Die Shaderobjekte werden mit Methoden der [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)-Schnittstelle wie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) erstellt.

Die Direct3D 11-Grafikpipeline wird von Instanzen der [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)-Schnittstelle verwaltet und umfasst die folgenden Stufen:

-   [Eingabe-Assembler-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). Die Eingabe-Assembler-Stufe stellt Daten (Dreiecke, Linien und Punkte) für die Pipeline bereit. [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) -Methoden, die diese Phase unterstützen, haben das Präfix "ia".
-   [Vertex-Shader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Die Vertex-Shader-Stufe verarbeitet Scheitelpunkte und führt dabei in der Regel Vorgänge wie Transformationen, das Anwenden von Skins und Beleuchtung aus. Ein Vertex-Shader verarbeitet immer einen einzigen Eingabescheitelpunkt und erzeugt daraus einen einzigen Ausgabescheitelpunkt. [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) -Methoden, die diese Phase unterstützen, haben das Präfix "vs".
-   [Datenstrom-Ausgabe-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage). Die Datenstrom-Ausgabe-Stufe streamt Grundtypdaten auf dem Weg zum Rasterizer aus der Pipeline in den Arbeitsspeicher. Daten können "ausgestreamt" und/oder in den Rasterizer übergeben werden. In den Arbeitsspeicher gestreamte Daten können als Eingabedaten wieder der Pipeline zugeführt oder von der CPU eingelesen werden. [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) -Methoden, die diese Phase unterstützen, haben das Präfix "so".
-   [Rasterizer-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage). Der Rasterizer beschneidet Grundtypen, bereitet Grundtypen für den Pixelshader vor und bestimmt, wie Pixelshader aufgerufen werden. Sie können die rasterisierung deaktivieren, indem Sie der Pipeline mitteilen, dass kein Pixelshader vorhanden ist (legen Sie die Pixel-Shader-Phase mit [**Verknüpfung id3d11devicecontext aus::P ssetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)auf NULL fest), und deaktivieren Sie Tiefe und Schablonen Tests (legen Sie depthenable und stencilenable in [**D3D11\_Tiefe\_Schablone\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)) auf "false" fest. Wenn die Rasterung deaktiviert ist, werden damit zusammenhängende Pipelinezähler nicht aktualisiert.
-   [Pixelshader-Stufe](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Die Pixelshader-Stufe empfängt interpolierte Daten für einen Grundtyp und generiert Pro-Pixel-Daten (z. B. die Farbe). [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) -Methoden, die diese Phase unterstützen, haben das Präfix "PS".
-   [Ausgabezusammenführungs-Stufe](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). Die Ausgabezusammenführungs-Stufe kombiniert verschiedene Ausgabedaten (Pixelshaderwerte, Tiefen- und Schabloneninformationen) mit dem Inhalt des Renderziels und Tiefen-/Schablonenpuffern, um das endgültige Pipelineergebnis zu generieren. [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) -Methoden, die diese Phase unterstützen, haben das Präfix "OM".

(Es gibt auch Stufen für Geometrie-Shader, Hüllen-Shader, Tesselator und Domain-Shader, aber da es dafür in OpenGL ES 2.0 keine Äquivalente gibt, gehen wir hier nicht weiter darauf ein.) Eine vollständige Liste der Methoden für diese Stufen finden Sie auf den Referenzseiten [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) und [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). **ID3D11DeviceContext1** erweitert **ID3D11DeviceContext** für Direct3D 11.

## <a name="creating-a-shader"></a>Erstellen eines Shaders


In Direct3D werden Shaderressourcen nicht vor dem Kompilieren und Laden erstellt. Stattdessen wird die Ressource beim Laden der HLSL-Datei erstellt. Daher gibt es keine direkt analoge Funktion zu gldeeshader, die eine initialisierte Shaderressource eines bestimmten Typs erstellt (z. b. gl\_Vertex\_Shader oder GL\_Fragment\_Shader). Shader werden stattdessen nach dem Laden der HLSL-Datei mit spezifischen Funktionen wie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) erstellt, für die der Typ und die kompilierte HLSL-Datei als Parameter verwendet werden.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Nachdem das kompilierte Shaderobjekt (Compiled Shader Object, CSO) geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) auf. Übergeben Sie die CSO-Datei dabei als Puffer. |

 

## <a name="compiling-a-shader"></a>Kompilieren eines Shaders


Direct3D-Shader müssen als kompilierte Shader-Objektdateien (. CSO) in universelle Windows-Plattform-Apps (UWP) vorkompiliert und mithilfe einer der Windows-Runtime Datei-APIs geladen werden. (Desktop-Apps können die Shader zur Laufzeit aus Textdateien oder Zeichenfolgen kompilieren.) Die CSO-Dateien werden aus beliebigen HLSL-Dateien des Microsoft Visual Studio-Projekts erstellt und behalten ihre Namen (jedoch mit der Dateierweiterung „.cso“). Stellen Sie sicher, dass Sie in Ihrem gelieferten Paket enthalten sind!

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | Nicht zutreffend. Kompilieren Sie die Shader in Visual Studio in CSO-Dateien, und fügen Sie sie Ihrem Paket hinzu.                                                                                     |
| Verwenden von "glGetShaderiv" für den Kompilierungsstatus | Nicht zutreffend. Überprüfen Sie, ob die Kompilierungsausgabe des FX-Compilers (FXC) von Visual Studio Fehler enthält. Bei erfolgreicher Kompilierung wird eine entsprechende CSO-Datei erstellt. |

 

## <a name="loading-a-shader"></a>Laden eines Shaders


Wie im Abschnitt zum Erstellen eines Shaders erwähnt, erstellt Direct3D 11 den Shader, wenn die entsprechende CSO-Datei in einen Puffer geladen und an eine der Methoden in der folgenden Tabelle übergeben wird.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Nachdem das kompilierte Shaderobjekt geladen wurde, rufen Sie [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) und [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) auf. |

 

## <a name="setting-up-the-pipeline"></a>Einrichten der Pipeline


In OpenGL ES 2.0 wird das "Shaderprogrammobjekt" verwendet, das mehrere Shader für die Ausführung enthält. Einzelne Shader werden an das Shaderprogrammobjekt angefügt. In Direct3D 11 arbeiten Sie jedoch direkt mit dem Renderkontext ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) und erstellen Shader für den Kontext.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glLinkProgram   | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glUseProgram    | Nicht zutreffend. In Direct3D 11 wird die Shaderprogrammobjekt-Abstraktion nicht verwendet.                          |
| glGetProgramiv  | Verwenden Sie den erstellten Verweis auf [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). |

 

Erstellen Sie mit der statischen [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)-Methode eine Instanz von [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) und [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

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


Das Festlegen eines Viewports funktioniert in Direct3D 11 und OpenGL ES 2.0 ähnlich. Wenden Sie in Direct3D 11 [**Verknüpfung id3d11devicecontext aus:: rssetviewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) mit einem konfigurierten [**CD3D11\_Viewport**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))an.

Direct3D 11: Festlegen eines Viewports.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11\_Viewport**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [ **Verknüpfung id3d11devicecontext aus:: rssetviewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>Konfigurieren der Vertex-Shader


Die Konfiguration eines Vertex-Shaders erfolgt in Direct3D 11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1) übergeben.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1:: kreatevertexshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1:: vsgetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1:: VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>Konfigurieren der Pixelshader


Die Konfiguration eines Pixelshaders erfolgt in Direct3D 11 beim Laden des Shaders. Uniform-Variablen werden als Konstantenpuffer mit [**ID3D11DeviceContext1::PSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1) übergeben.

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1:: kreatepixelshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::P sgetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::P sgetconstantbuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>Generieren der endgültigen Ergebnisse


Wenn die Pipeline abgeschlossen ist, zeichnen Sie die Ergebnisse der Shaderstufen in den Hintergrundpuffer. In Direct3D 11 wird dazu wie in OpenGL ES 2.0 ein Zeichnen-Befehl aufgerufen, um die Ergebnisse als Farbzuordnung in den Hintergrundpuffer auszugeben. Dieser Hintergrundpuffer wird anschließend an die Anzeige gesendet.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::D RAW**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [**ID3D11DeviceContext1::D rawindexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (oder andere Draw\*-Methoden auf [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1::P resent1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Portieren von GLSL zu HLSL


GLSL und HLSL unterscheiden sich abgesehen von der Unterstützung komplexer Typen und einiger allgemeiner Syntaxteile nur wenig. Für viele Entwickler besteht die einfachste Methode für die Portierung zwischen den beiden Programmiersprachen darin, Aliase für allgemeine OpenGL ES 2.0-Anweisungen und -Definitionen zu erstellen und sie ihrem HLSL-Äquivalent zuzuordnen. Beachten Sie, dass Direct3D die Shadermodellversion verwendet, um die von einer Grafikschnittstelle unterstützte HLSL-Funktionsgruppe darzustellen. In OpenGL wird eine andere Versionsspezifikation für HLSL verwendet. Die folgende Tabelle soll Ihnen – in Bezug auf die Version – eine ungefähre Vorstellung von den für Direct3D 11 und OpenGL ES 2.0 definierten Funktionsgruppen der Shaderprogrammiersprachen geben.

| Shaderprogrammiersprache           | GLSL-Funktionsversion                                                                                                                                                                                                      | Direct3D-Shadermodell |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Direct3D 11 HLSL          | ~4.30                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES für OpenGL ES 2.0 | 1.40. Ältere Implementierungen von GLSL ES für OpenGL ES 2.0 verwenden möglicherweise die Versionen 1.10 bis 1.30. Überprüfen Sie den ursprünglichen Code mit glgetstring (GL\_Schattierung\_Language\_Version) oder glgetstring (Schattierung\_Sprache\_Version), um den Code zu ermitteln. | ~SM 2.0               |

 

Weitere Informationen zu den Unterschieden zwischen den beiden Shaderprogrammiersprachen und allgemeine Syntaxzuordnungen finden Sie in der [GLSL-zu-HLSL-Referenz](glsl-to-hlsl-reference.md).

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Portieren der systeminternen OpenGL-Funktionen zu HLSL-Semantik


Bei der Direct3D 11-HLSL-Semantik handelt es sich um Zeichenfolgen, die wie eine Uniform-Variable oder ein Attributname zum Identifizieren eines zwischen der App und einem Shaderprogramm übergebenen Werts verwendet werden. Obwohl viele verschiedene Zeichenfolgen möglich sind, empfiehlt es sich, eine Zeichenfolge wie POSITION oder COLOR zu verwenden, aus der der Verwendungszweck hervorgeht. Sie weisen diese Semantik zu, wenn Sie einen Konstantenpuffer oder ein Puffereingabelayout erstellen. Sie können auch eine Zahl zwischen 0 und 7 an die Semantik anfügen, wenn Sie separate Register für ähnliche Werte verwenden möchten. Beispiel: COLOR0, COLOR1, COLOR2...

Bei der Semantik mit dem Präfix "SV\_" handelt es sich um eine System Wert Semantik, die von Ihrem Shader-Programm geschrieben wird. Ihre APP selbst (die auf der CPU ausgeführt wird) kann Sie nicht ändern. In der Regel enthält diese Semantik Werte, die Ein- oder Ausgaben einer anderen Shaderstufe in der Grafikpipeline sind oder komplett von der GPU generiert werden.

Außerdem hat die SV-\_ Semantik verschiedene Verhalten, wenn Sie verwendet werden, um Eingaben für eine Shader-Stufe anzugeben oder diese auszugeben. Beispielsweise enthält SV\_Position (Ausgabe) die Vertex-Daten, die während der Vertex-Shader-Stufe transformiert werden, und SV\_Position (Eingabe) enthält die Pixel Positionswerte, die während der rasterisierung interpoliert wurden.

Die folgende Tabelle enthält einige Zuordnungen für allgemeine systeminterne OpenGL ES 2.0-Shaderfunktionen:

| OpenGL-Systemwert | Verwenden Sie diese HLSL-Semantik:                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GL\_Position        | POSITION(n) für Scheitelpunkt-Pufferdaten. SV\_Position gibt eine Pixel Position für den Pixelshader an und kann nicht von Ihrer APP geschrieben werden.                                        |
| GL\_normal          | NORMAL(n) für vom Scheitelpunktpuffer bereitgestellte Normalendaten.                                                                                                                 |
| GL\_texcoord\[n\]   | TEXCOORD(n) für Textur-UV-Koordinatendaten (ST in manchen OpenGL-Dokumentationen), die an einen Shader übergeben werden.                                                                       |
| GL\_fragcolor       | COLOR(n) für RGBA-Farbdaten, die an einen Shader übergeben werden. Diese Daten werden wie Koordinatendaten behandelt. Durch die Semantik können Sie nur leichter erkennen, dass es sich um Farbdaten handelt. |
| GL\_fragdata\[n\]   | SV\_Ziel\[n\] zum Schreiben von einem Pixelshader in eine Ziel Textur oder einen anderen Pixel Puffer.                                                                               |

 

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

In diesem Fall ist das Ziel SV\_Ziel der Speicherort des Renderziels, in das die Pixelfarbe (als Vektor mit vier Gleit Komma Werten definiert ist) geschrieben wird, wenn der Shader die Ausführung abschließt.

Ausführliche Informationen zur Verwendung der Semantik mit Direct3D finden Sie unter [HLSL-Semantik](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

 

 




