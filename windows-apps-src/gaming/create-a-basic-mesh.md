---
title: Erstellen und Anzeigen einfacher Gitter
description: In 3D-Spielen für die universelle Windows-Plattform (UWP) werden Spielobjekte und Oberflächen in der Regel durch Polygone dargestellt.
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Gitter, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 9b5aa00b5beb7c80a903fbf17d432f73f16561a2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368985"
---
# <a name="create-and-display-a-basic-mesh"></a>Erstellen und Anzeigen einfacher Gitter



In 3D-Spielen für die universelle Windows-Plattform (UWP) werden Spielobjekte und Oberflächen in der Regel durch Polygone dargestellt. Die Liste der Vertizes, die die Struktur dieser polygonalen Objekte und Oberflächen darstellen, werden als Gitter bezeichnet. Hier erstellen wir ein einfaches Gitter für ein Würfelobjekt und stellen es zum Rendern und Anzeigen für die Shader-Pipeline bereit.

> **Wichtige**    Beispiel enthaltenen Codes hier verwendet werden, Typen (z. B. DirectX::XMFLOAT3 und DirectX::XMFLOAT4X4) und Inline-Methoden, die in DirectXMath.h deklariert. Wenn Sie Ausschneiden und Einfügen dieses Codes, \#enthalten &lt;DirectXMath.h&gt; in Ihrem Projekt.

 

## <a name="what-you-need-to-know"></a>Wissenswertes


### <a name="technologies"></a>Technologien

-   [Direct3D](https://docs.microsoft.com/windows/desktop/getting-started-with-direct3d)

### <a name="prerequisites"></a>Vorraussetzungen

-   Grundkenntnisse in linearer Algebra und 3D-Koordinatensystemen
-   Eine Visual Studio 2015 oder höher Direct3D-Vorlage

## <a name="instructions"></a>Anweisungen

Diese Schritte verdeutlichen, wie Sie einen einfachen Gitterwürfel erstellen. 


Wenn Sie eine gesprochene Erklärung dieser Konzepte bevorzugen, sehen Sie sich dieses Video an.
</br>
</br>
<iframe src="https://channel9.msdn.com/Series/Introduction-to-C-and-DirectX-Game-Development/03/player#time=7m39s:paused" width="600" height="338" allowFullScreen frameBorder="0"></iframe>


### <a name="step-1-construct-the-mesh-for-the-model"></a>Schritt 1: Erstellen Sie das Netz für das Modell

In den meisten Spielen wird das Gitter für ein Spielobjekt aus einer Datei geladen, die die spezifischen Vertexdaten enthält. Die Reihenfolge dieser Vertizes ist App-abhängig, in der Regel werden sie jedoch ketten- oder fächerförmig serialisiert. Vertexdaten können aus einer beliebigen Softwarequelle stammen oder manuell erstellt werden. Die Daten müssen vom Spiel so interpretiert werden, dass sie vom Vertex-Shader effektiv verarbeitet werden können.

In unserem Beispiel verwenden wir ein einfaches Gitter für einen Würfel. Der Würfel wird wie alle Objektgitter in diesem Abschnitt der Pipeline mithilfe eines eigenen Koordinatensystems dargestellt. Der Vertex-Shader verwendet die Koordinaten und gibt durch Anwendung der bereitgestellten Transformationsmatrizen die endgültige 2D-Anzeigeprojektion in einem homogenen Koordinatensystem zurück.

Definieren Sie das Gitter für einen Würfel. (Laden Sie es alternativ aus einer Datei. Das bleibt Ihnen überlassen.)

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

Das Koordinatensystem des Würfels platziert die Mitte des Würfels am Ursprung. Dabei verläuft die Y-Achse bei Verwendung eines linkshändigen Koordinatensystems von oben nach unten. Die Koordinatenwerte werden als 32-Bit-Gleitkommawerte zwischen -1 und 1 ausgedrückt.

Bei jedem in Klammern gesetzten Paar gibt die zweite DirectX::XMFLOAT3-Wertgruppe die dem Vertex zugeordnete Farbe als RGB-Wert an. Beispiel: Der erste Vertex bei (-0,5, 0,5, -0,5) hat eine deckende grüne Farbe (für den Wert "G" ist 1,0 festgelegt, für die Werte "R" und "B" ist 0 festgelegt).

Daraus ergeben sich acht Vertizes mit jeweils einer bestimmten Farbe. Jedes Paar aus Vertex und Farbe stellt die vollständigen Daten für einen Vertex in unserem Beispiel dar. Wenn Sie den Vertexpuffer angeben, müssen Sie dabei dieses spezielle Layout berücksichtigen. Dieses Eingabelayout wird dem Vertex-Shader zur Verfügung gestellt, damit er die Vertexdaten interpretieren kann.

### <a name="step-2-set-up-the-input-layout"></a>Schritt 2: Richten Sie das eingabelayout

Nun befinden sich die Vertizes im Speicher. Ihr Grafikgerät besitzt jedoch einen eigenen Speicher, auf den Sie mithilfe von Direct3D zugreifen. Um die Vertexdaten zur Verarbeitung an das Grafikgerät zu übergeben, müssen Sie den Weg dazu ebnen. Das bedeutet: Sie müssen deklarieren, wie die Vertexdaten angeordnet sind, sodass sie vom Grafikgerät interpretiert werden können, wenn sie vom Spiel an das Gerät übergeben werden. Verwenden Sie dazu [**ID3D11InputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout).

Deklarieren Sie das Eingabelayout für den Vertexpuffer, und legen Sie es fest.

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

In diesem Code geben Sie ein Layout für die Vertizes an – genauer gesagt: welche Daten die einzelnen Elemente in der Vertexliste enthalten. In **basicVertexLayoutDesc** geben Sie zwei Datenkomponenten an:

-   **POSITION**: Hierbei handelt es sich um einen HLSL für Positionsdaten bereitgestellt, um einen Shader. In diesem Code ist dies ein DirectX::XMFLOAT3-Wert, oder genauer ausgedrückt, eine Struktur mit drei 32-Bit-Gleitkommazahlen, die einem 3D-Koordinatensystem (X, Y, Z) entsprechen. Sie können auch eine float4 verwenden, wenn Sie die homogene "w" Koordinate bereitgestellten, und in diesem Fall DXGI\_FORMAT\_R32G32B32A32\_"float". Ob Sie einen DirectX::XMFLOAT3- oder einen float4-Wert verwenden, ist von den speziellen Anforderungen Ihres Spiels abhängig. Stellen Sie lediglich sicher, dass die Vertexdaten für Ihr Gitter dem verwendeten Format entsprechen.

    Jeder Koordinatenwert wird als Gleitkommawert zwischen -1 und 1 im Koordinatenbereich des Objekts ausgedrückt. Nach Abschluss des Vertex-Shaders befindet sich der transformierte Vertex im homogenen (korrigierte Perspektive) Anzeigeprojektionsbereich.

    „Aber der Enumerationswert gibt RGB und nicht XYZ an!“, wie Sie völlig zu Recht bemerkt haben. Gut aufgepasst! Sowohl für Farbdaten als auch für Koordinatendaten verwenden Sie in der Regel drei oder vier Komponentenwerte. Warum kann also nicht dasselbe Formate für beide verwendet werden? Nicht der Formatname, sondern die HLSL-Semantik gibt an, wie der Shader die Daten behandelt.

-   **FARBE**: Dies ist eine Semantik zum Farbdaten HLSL. Genau wie **POSITION** besteht auch sie aus drei 32-Bit-Gleitkommawerten (DirectX::XMFLOAT3). Jeder Wert enthält eine Farbkomponente: rot (r), blau (b) oder grün (g). Diese werden als Gleitkommazahl zwischen 0 und 1 ausgedrückt.

    **COLOR**-Werte werden in der Regel als RGBA-Wert aus vier Komponenten am Ende der Shader-Pipeline zurückgegeben. In diesem Beispiel legen Sie in der Shader-Pipeline für alle Pixel den Alpha-Wert „A“ auf „1,0“ (maximale Deckkraft) fest.

Eine vollständige Liste der Formate, finden Sie unter [ **DXGI\_FORMAT**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format). Die vollständige HLSL-Semantikliste finden Sie unter [Semantik](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

Rufen Sie [**ID3D11Device::CreateInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) auf, und erstellen Sie das Eingabelayout auf dem Direct3D-Gerät. Nun müssen Sie einen Puffer erstellen, der die Daten auch enthalten kann.

### <a name="step-3-populate-the-vertex-buffers"></a>Schritt 3: Füllen Sie die Vertexpuffer

Vertexpuffer enthalten die Liste der Vertizes für alle Dreiecke im Gitter. Alle Vertizes müssen in dieser Liste eindeutig sein. In unserem Beispiel sind acht Vertizes für den Würfel vorhanden. Der Vertex-Shader wird auf dem Grafikgerät ausgeführt. Er liest aus dem Vertexpuffer und interpretiert die Daten basierend auf dem im vorherigen Schritt angegebenen Eingabelayout.

Im nächsten Beispiel geben Sie eine Beschreibung und eine Unterressource für den Puffer an. Diese beiden Elemente teilen Direct3D verschiedene Informationen zur physischen Zuordnung der Vertexdaten und zu ihrer Behandlung im Speicher des Grafikgeräts mit. Das ist erforderlich, weil Sie einen generischen [**ID3D11Buffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11buffer) verwenden, der alles Mögliche enthalten kann. Die [ **D3D11\_Puffer\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) und [ **D3D11\_SUBRESOURCE\_Daten** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)Strukturen werden bereitgestellt, um sicherzustellen, dass die Direct3D das Layout der physikalischen Speicher des Puffers, u. a. die Größe der einzelnen Vertex-Elemente in den Puffer sowie die maximale Größe der Liste Vertex versteht. Darüber hinaus können Sie hier den Zugriff auf den Pufferspeicher und seinen Durchlauf steuern. Dies ist jedoch nicht Teil dieses Lernprogramms.

Nach dem Konfigurieren des Puffers rufen Sie [**ID3D11Device::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) auf, um ihn zu erstellen. Bei mehreren Objekten müssen Sie natürlich Puffer für jedes einzelne Modell erstellen.

Deklarieren Sie den Vertexpuffer, und erstellen Sie ihn.

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
vertexBufferDesc.CPUAccessFlags = 0;
vertexBufferDesc.MiscFlags = 0;
vertexBufferDesc.StructureByteStride = 0;

D3D11_SUBRESOURCE_DATA vertexBufferData;
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;

ComPtr<ID3D11Buffer> vertexBuffer;
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

Die Vertizes werden geladen. Aber in welcher Reihenfolge werden diese Vertizes verarbeitet? Dies wird festgelegt, wenn Sie für die Vertizes eine Liste mit den Indizes angeben. Die Reihenfolge dieser Indizes ist die Reihenfolge, in der sie vom Vertex-Shader verarbeitet werden.

### <a name="step-4-populate-the-index-buffers"></a>Schritt 4: Füllen Sie die Indexpuffer

Nun geben Sie eine Liste mit den Indizes für die einzelnen Vertizes an. Diese Indizes entsprechen der Position des Vertex im Vertexpuffer und beginnen mit 0. Stellen Sie sich zur besseren Veranschaulichung einfach vor, dass jedem eindeutigen Vertex in Ihrem Gitter eine eindeutige Zahl (wie eine ID) zugeordnet ist. Diese ID ist die ganzzahlige Position des Vertex im Vertexpuffer.

![Ein Würfel mit acht nummerierten Vertizes](images/cube-mesh-1.png)

Unser Beispielwürfel besitzt acht Vertizes, die wiederum sechs Vierecke für die Seiten bilden. Wenn Sie die Vierecke in Dreiecke teilen, entstehen aus den acht Vertizes also insgesamt zwölf Dreiecke. Bei drei Vertizes pro Dreieck enthält der Indexpuffer 36 Einträge. In unserem Beispiel wird dieses indexmuster als Dreiecksliste bezeichnet, und Sie zu Direct3D als angeben einer **D3D11\_PRIMITIVEN\_Topologie\_TRIANGLELIST** beim Festlegen der primitiven Topologie.

Diese Art der Indexauflistung ist besonders ineffizient, da Dreiecke gemeinsame Punkte und Seiten besitzen und es so zu Redundanzen kommt. Teilt sich ein Dreieck also beispielsweise eine Seite mit einer Raute, werden für die vier Vertizes sechs Indizes aufgelistet:

![Reihenfolge der Indizes beim Konstruieren einer Raute](images/rhombus-surface-1.png)

-   Dreieck 1: \[0, 1, 2\]
-   Dreieck 2: \[0, 2, 3\]

In einer Topologie Offsettyp der Leiste oder Lüfter sortieren Sie Vertices in einer Weise, die beseitigt viele redundante Seiten während des Durchlaufs (z. B. die Seite von Index 0 in den Index 2 in der Abbildung.) Für große Gitter versehe, wird erheblich die Anzahl der Vertex-Shader ausgeführt wird, und verbessert die Leistung erheblich reduziert. Wir halten es hier jedoch einfach und verwenden die Dreiecksliste.

Deklarieren Sie die Indizes für den Vertexpuffer als eine einfache Dreieckslistentopologie.

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

Bei 36 Indexelemente im Puffer für lediglich acht Vertizes ergibt sich ein hohes Maß an Redundanz. Wenn Sie auswählen, einige der Redundanzen zu entfernen, und verwenden Sie einen anderen Vertex Liste, z. B. ein Streifen oder ein Lüfter, müssen Sie diesen Typ angeben, wenn Sie angeben, dass eine bestimmte [ **D3D11\_PRIMITIVEN\_Topologie** ](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) Wert der [ **ID3D11DeviceContext::IASetPrimitiveTopology** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) Methode.

Weitere Informationen zu verschiedenen Indexlistentechniken finden Sie unter [Primitive Topologien](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-primitive-topologies).

### <a name="step-5-create-a-constant-buffer-for-your-transformation-matrices"></a>Schritt 5: Erstellen Sie einen Konstanten Puffer für die Transformation für Matrizen

Bevor Sie mit der Verarbeitung von Vertizes beginnen können, müssen Sie die Transformationsmatrizen angeben, die bei der Ausführung auf alle Vertizes angewendet (multipliziert) werden. Die meisten 3D-Spielen enthalten drei Matrizen:

-   4 x 4-Matrix, die eine Transformation vom Koordinatensystem des Objekts (Modell) zum allgemeinen Weltkoordinatensystem ausführt
-   4 x 4-Matrix, die eine Transformation vom Weltkoordinatensystem zum Kamerakoordinatensystem (Ansicht) ausführt
-   4 x 4-Matrix, die eine Transformation vom Kamerakoordinatensystem zum Koordinatensystem der 2D-Anzeigeprojektion ausführt

Diese Matrizen werden in einem *konstanten Puffer* an den Shader übergeben. Bei einem Konstantenpuffer handelt es sich um einen Bereich des Speichers, der während des nächsten Schritts der Shader-Pipeline konstant bleibt und auf den von den Shadern aus dem HLSL-Code direkt zugegriffen werden kann. Sie definieren die einzelnen konstanten Puffer zweimal: zuerst im C++-Code Ihres Spiels und (mindestens) einmal in der C-ähnlichen HLSL-Syntax für den Shadercode. Die beiden Deklarationen müssen in Bezug auf Typ und Datenausrichtung direkt übereinstimmen. Schwer auffindbare Fehler entstehen schnell, wenn der Shader die HLSL-Deklaration zum Interpretieren von in C++ deklarierten Daten verwendet und die Typen nicht übereinstimmen oder die Ausrichtung der Daten deaktiviert ist.

Konstante Puffer werden von der HLSL-Syntax nicht geändert. Sie können sie ändern, wenn vom Spiel bestimmte Daten aktualisiert werden. Häufig erstellen Spieleentwickler vier Klassen konstanter Puffer: einen Typ für Updates pro Frame, einen Typ für Updates pro Modell/Objekt, einen Typ für Updates pro Aktualisierung des Spielzustands und einen Typ für Daten, die sich während der Lebensdauer des Spiels nie ändern.

Dieses Beispiel enthält nur eine Art von Daten, die sich nie ändern: die DirectX::XMFLOAT4X4-Daten für die drei Matrizen.

> **Beachten Sie**    der hier dargestellte Beispielcode wird spaltengerichteten Matrizen verwendet. Sie können stattdessen zeilenmatrizen mithilfe der **Zeile\_wichtigen** -Schlüsselwort in "HLSL", und sicherstellen, dass die Daten Ihrer Quelle Matrix ist ebenfalls zeilengerichteter. DirectXMath zeilenmatrizen verwendet und kann verwendet werden, direkt mit dem HLSL-Matrizen mit definiert die **Zeile\_wichtigen** Schlüsselwort.

 

Deklarieren und erstellen Sie einen Konstantenpuffer für die drei Matrizen, die Sie zum Transformieren der einzelnen Vertizes verwenden.

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **Beachten Sie**  Sie in der Regel deklarieren, die Projektionsmatrix beim Einrichten von Ressourcen für bestimmte Geräte, da die Ergebnisse der Multiplikation mit ihm die aktuellen Parameter der 2-D Viewport-Größe entsprechen müssen (die häufig mit der Pixel entsprechen Höhe und Breite der Anzeige). Ändern sich diese, müssen Sie die Werte für die X- und die Y-Koordinate entsprechend skalieren.

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

Wenn Sie schon einmal dabei sind, legen Sie die Vertex- und Indexpuffer für [ID3D11DeviceContext](https://docs.microsoft.com/windows/desktop/direct3d11/d3d11-graphics-reference-10level9-context) sowie die verwendete Topologie fest.

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

Gut. Das Eingabeassembly ist fertig. Nun ist alles bereit für das Rendern. Führen wir also den Vertex-Shader aus.

### <a name="step-6-process-the-mesh-with-the-vertex-shader"></a>Schritt 6: Mit den Vertex-Shader verarbeiten

Sie haben nun einen Vertexpuffer mit den Vertizes, die Ihr Gitter definieren, und den Indexpuffer, der die Reihenfolge für die Verarbeitung der Vertizes festlegt. Senden Sie die beiden Puffer jetzt an den Vertex-Shader. Der Vertex-Shader-Code (ausdrückt als kompilierte High-Level Shader Language (HLSL)) wird ein Mal für jeden Vertex im Vertexpuffer ausgeführt. Dadurch wird die Transformation der einzelnen Vertizes ermöglicht. Das Endergebnis ist in der Regel eine 2D-Projektion.

(Haben Sie den Vertex-Shader geladen? Falls nicht, lesen Sie [So wird's gemacht: Laden von Ressourcen im DirectX-Spiel](load-a-game-asset.md).)

Hier erstellen Sie den Vertex-Shader...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

...und legen die Konstantenpuffer fest.

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

Hier sehen Sie den Vertex-Shader-Code, mit dem die Transformation von Objektkoordinaten zu Weltkoordinaten und schließlich zum Koordinatensystem der 2D-Anzeigeprojektion ausgeführt wird. Darüber hinaus wenden Sie eine einfache Beleuchtung für einzelne Vertizes an, um das Ganze zu verschönern. Dies wird in der HLSL-Datei des Vertex-Shaders gespeichert (in diesem Beispiel in „SimplerVertexShader.hlsl“).

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

Sehen Sie **cbuffer** ganz oben? Dieser HLSL-Code entspricht dem Konstantenpuffer, den wir zuvor in unserem C++-Code deklariert haben. Und wie sieht es mit **VertexShaderInputstruct** aus? Das sieht doch ganz nach der Deklaration Ihres Eingabelayouts und der Vertexdaten aus. Die Deklarationen des Konstantenpuffers und der Vertexdaten im C++-Code müssen mit den Deklarationen im HLSL-Code übereinstimmen – einschließlich Vorzeichen, Typen und Datenausrichtung.

**PixelShaderInput** gibt das Layout der Daten an, die von der Hauptfunktion des Vertex-Shaders zurückgegeben werden. Nach der Verarbeitung eines Vertex geben Sie eine Vertexposition im 2D-Projektionsbereich und eine für die Beleuchtung der einzelnen Vertizes verwendete Farbe zurück. Die Grafikkarte verwendet die Datenausgabe des Shaders zum Berechnen der Fragmente (mögliche Pixel), die koloriert werden müssen, wenn der Pixel-Shader im nächsten Abschnitt der Pipeline ausgeführt wird.

### <a name="step-7-passing-the-mesh-through-the-pixel-shader"></a>Schritt 7: Übergibt Mesh den PixelShader

In der Regel führen Sie in diesem Abschnitt der Grafikpipeline Aktionen für einzelne Pixel auf den sichtbaren projizierten Oberflächen der Objekte aus. (Wie z. B. Texturen Personen.) Für das Beispiel übergeben jedoch Sie einfach ihr in dieser Phase.

Erstellen wir zunächst eine Instanz des Pixel-Shaders. Der Pixel-Shader wird für jedes Pixel in der 2D-Projektion der Szene ausgeführt und weist dabei dem jeweiligen Pixel eine Farbe zu. In diesem Fall wird die vom Vertex-Shader für das Pixel zurückgegebene Farbe direkt weitergeleitet.

Legen Sie den Pixel-Shader fest.

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

Definieren Sie einen Passthrough-Pixel-Shader in HLSL.

``` syntax
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

Fügen Sie diesen Code getrennt vom Vertex-Shader-HLSL-Code in eine HLSL-Datei (beispielsweise „SimplePixelShader.hlsl“) ein. Dieser Code wird einmal für jedes sichtbare Pixel im Viewport (eine speicherinterne Darstellung des Bildschirmbereichs, in dem Sie zeichnen) ausgeführt. In diesem Fall entspricht er dem gesamten Bildschirm. Nun ist Ihre Grafikpipeline vollständig definiert.

### <a name="step-8-rasterizing-and-displaying-the-mesh"></a>Schritt 8: Rastern und die Anzeige des Netzes

Führen Sie die Pipeline aus. Rufen Sie dazu einfach [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-drawindexed) auf.

Zeichnen Sie den Würfel.

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

Von der Grafikkarte wird jeder Vertex in der im Indexpuffer angegebenen Reihenfolge verarbeitet. Nachdem der Vertex-Shader von Ihrem Code ausgeführt und die 2D-Fragmente definiert wurden, wird der Pixel-Shader aufgerufen, und die Dreiecke werden koloriert.

Platzieren Sie nun den Würfel auf dem Bildschirm.

Stellen Sie den Framepuffer für die Anzeige bereit.

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

Nun sind Sie fertig. Verwenden Sie für eine mit Modellen gefüllte Szene mehrere Vertex- und Indexpuffer. Möglicherweise haben Sie sogar verschiedene Shader für unterschiedliche Modelltypen. Bedenken Sie, dass jedes Modell ein eigenes Koordinatensystem besitzt. Sie müssen die Systeme mithilfe der im konstanten Puffer definierten Matrizen in das gemeinsame Weltkoordinatensystem umwandeln.

## <a name="remarks"></a>Hinweise

In diesem Thema wird das Erstellen und Anzeigen einfacher, selbst erstellter Geometrie behandelt. Weitere Informationen zum Laden komplexerer Geometrie aus einer Datei und Konvertieren in das beispielspezifische Vertexpufferobjekt-Format (VBO) finden Sie unter [So wird's gemacht: Laden von Ressourcen im DirectX-Spiel](load-a-game-asset.md).  

 

## <a name="related-topics"></a>Verwandte Themen


* [Gewusst wie: Laden von Ressourcen in Ihrer DirectX-Spielen](load-a-game-asset.md)

 

 




