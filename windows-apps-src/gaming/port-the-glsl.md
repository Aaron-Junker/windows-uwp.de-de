---
title: Portieren der GLSL
description: Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, GLSL, Portieren
ms.localizationpriority: medium
ms.openlocfilehash: 809440f9e77af19c01f4a050eee3b6f8d1c709b7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621375"
---
# <a name="port-the-glsl"></a>Portieren der GLSL




**Wichtige APIs**

-   [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [Shaderkonstanten (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

Nachdem Sie sich um den Code gekümmert haben, mit dem die Puffer und Shaderobjekte erstellt und konfiguriert werden, muss der in diesen Shadern enthaltene Code von der GL Shader Language (GLSL) von OpenGL ES 2.0 in die High-Level Shader Language (HLSL) von Direct3D 11 portiert werden.

In OpenGL ES 2.0-Shader, Daten nach der Ausführung, die Verwendung systeminterner Funktionen wie z. B. zurückgeben **Gl\_Position**, **Gl\_FragColor**, oder **Gl\_FragData \[n\]**  (wobei n für der Index für eine bestimmte Renderziel ist). In Direct3D sind keine speziellen systeminternen Funktionen vorhanden. Von den Shadern werden Daten als Rückgabetyp ihrer jeweiligen main()-Funktionen zurückgegeben.

Daten, die zwischen Shaderphasen interpoliert werden sollen, z. B. Vertexposition oder Normale, werden mithilfe der **varying**-Deklaration behandelt. Direct3D verfügt jedoch nicht über diese Deklaration. Alle Daten, die zwischen Shaderphasen übertragen werden sollen, müssen daher per [HLSL-Semantik](https://msdn.microsoft.com/library/windows/desktop/bb205574) markiert werden. Mit der jeweils gewählten Semantik wird der Zweck für die Daten angegeben. Vertexdaten, die zwischen Fragmentshadern interpoliert werden sollen, werden beispielsweise wie folgt deklariert:

`float4 vertPos : POSITION;`

oder

`float4 vertColor : COLOR;`

POSITION steht hierbei für die Semantik, die zum Angeben der Vertexpositionsdaten verwendet wird. POSITION stellt außerdem einen Sonderfall dar, da vom Pixelshader nach der Interpolation darauf nicht zugegriffen werden kann. Aus diesem Grund müssen Sie mit PA Eingabe für den Pixel-Shader angeben\_POSITION und die interpolierte Vertexdaten werden in der Variablen platziert.

`float4 position : SV_POSITION;`

Die Semantik kann für die body-Methoden (main) von Shadern deklariert werden. Für die Pixel-Shader, SV\_Ziel\[n\], womit ein Renderziel ist erforderlich, für die Body-Methode. (SV\_ABZIELEN, ohne ein numerisches Suffix wird zum Rendern der Ziel-Index 0 standardmäßig.)

Beachten Sie, dass die Vertex-Shader erforderlich sind, um die Ausgabe der SV\_Wert die POSITION des semantischen. Mit dieser Semantik werden die Vertexpositionsdaten zu Koordinatenwerten aufgelöst, wobei "x" zwischen -1 und 1 und "y" zwischen -1 und 1 liegt. "z" wird durch den ursprünglichen homogenen Koordinatenwert "w" dividiert (z/w), und "w" ist 1 dividiert durch den Originalwert "w" (1/w). Pixel-Shader verwenden, die PA\_POSITION System abzurufenden die Pixelposition auf dem Bildschirm, wobei x zwischen 0 und die Breite der Render-Ziel und y ist semantische Wert zwischen 0 und die Render-Ziel-Höhe (jede Offset von 0,5). Die Funktionsebene 9\_x Pixel-Shader nicht werden aus der SV gelesen können\_Positionswert.

Konstantenpuffer müssen mit **cbuffer** deklariert und mit einem speziellen Startregister für die Suche versehen werden.

Direct3D 11: Eine Deklaration der HLSL-Konstante Puffer

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Hier wird vom Konstantenpuffer das Register „b0“ für den gepackten Puffer verwendet. Alle Register werden in der Form b bezeichnet\#. Weitere Informationen zur HLSL-Implementierung von Konstantenpuffern, Registern und Datenpaketen finden Sie unter [Shaderkonstanten (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581).

<a name="instructions"></a>Anweisungen
------------
### <a name="step-1-port-the-vertex-shader"></a>Schritt 1: Port des Vertex-Shaders

In diesem einfachen OpenGL ES 2.0-Beispiel verfügt der Vertex-Shader über drei Eingaben: eine konstante Modell-Ansicht-Projektion-4x4-Matrix und zwei Vektoren mit vier Koordinaten. Diese beiden Vektoren enthalten die Vertexposition und ihre Farbe. Der Shader transformiert den Vektor Position in Koordinaten der Perspektive, und weist diese der Gl\_systeminterne zum Rasterization Position. Außerdem wird die Vertexfarbe zur Interpolation während der Rasterung in eine abweichende Variable kopiert.

OpenGL ES 2.0: Vertex-Shader für das Cubeobjekt (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

Jetzt in Direct3D, die Konstante-Modell-Ansicht-Projektionsmatrix ist Bestandteil eines Konstantenpuffers, der an die Register b0 gepackt, und Vertexposition und Farbe der werden ausdrücklich mit der entsprechenden Semantik der jeweiligen HLSL markiert: POSITION und Farbe. Da das Eingabelayout eine bestimmte Anordnung dieser beiden Vertexwerte vorgibt, erstellen Sie dafür eine Struktur und deklarieren diese als Typ für den Eingabeparameter der body-Shaderfunktion (main). (Sie können diese auch als zwei einzelne Parameter angeben, jedoch konnten, das umständlich abgerufen.) Außerdem geben Sie einen Ausgabetyp für diese Phase, die die interpolierten Position und Farbe enthält, und deklarieren Sie sie als Rückgabewert für die Text-Funktion des Vertex-Shader.

Direct3D 11: Vertex-Shader für das Cubeobjekt (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

Der Ausgabedatentyp „PixelShaderInput“ wird während der Rasterung aufgefüllt und für den Fragmentshader (Pixelshader) bereitgestellt.

### <a name="step-2-port-the-fragment-shader"></a>Schritt 2: Port der fragmentshader

Unsere Beispiel-fragmentshader in GLSL ist sehr einfach: Geben Sie die Gl\_FragColor systeminternen Funktionen mit der interpolierten Farbwert. Von OpenGL ES 2.0 wird er in das standardmäßige Renderziel geschrieben.

OpenGL ES 2.0: Für das Cubeobjekt (GLSL)-fragmentshader

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Für Direct3D ist der Aufbau nahezu genauso einfach. Der einzige zu beachtende Unterschied besteht darin, dass die body-Funktion des Pixelshaders einen Wert zurückgeben muss. Da die Farbe einen 4-Koordinate (RGBA) Float-Wert ist, Sie float4 als Rückgabetyp anzugeben, und geben Sie dann das Standard-Renderziel als PA\_semantische System-Zielwert.

Direct3D 11: Pixel-Shader für das Cubeobjekt (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

Die Farbe für das Pixel an der Position wird in das Renderziel geschrieben. Das Anzeigen der Inhalte dieses Renderziels als nächster Schritt wird unter [Zeichnen auf den Bildschirm](draw-to-the-screen.md) beschrieben.

## <a name="previous-step"></a>Vorheriger Schritt


[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md) Nächster Schritt
---------
[Zeichnen auf den Bildschirm](draw-to-the-screen.md) Anmerkungen
-------
Wenn Sie mit der HLSL-Semantik und dem Packen von Konstantenpuffern vertraut sind, können Sie einigen Debugaufwand vermeiden und Möglichkeiten zur Optimierung schaffen. Wenn Sie die Möglichkeit erhalten, lesen [Variable Syntax (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Einführung in die Speicherpuffer in Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898), und [Vorgehensweise: Erstellen ein Konstantenpuffers](https://msdn.microsoft.com/library/windows/desktop/ff476896). Hier sind als Anfang schon einmal einige Tipps aufgeführt, die in Verbindung mit der Semantik und Konstantenpuffern zu beachten sind:

-   Überprüfen Sie stets den Direct3D-Konfigurationscode des Renderers, um sicherzustellen, dass die Strukturen für die Konstantenpuffer mit den cbuffer-Strukturdeklarationen der HLSL übereinstimmen und dass die Komponentenskalartypen für beide Deklarationen übereinstimmen.
-   Verwenden Sie im C++-Code des Renderers [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833)-Typen in den Konstantenpufferdeklarationen, um für die richtige Verpackung der Daten zu sorgen.
-   Die beste Möglichkeit zur effizienten Nutzung von Konstantenpuffern ist die Organisation von Shadervariablen in Konstantenpuffern anhand der Updatehäufigkeit. Wenn Sie beispielsweise über uniform-Daten verfügen, die einmal pro Frame aktualisiert werden, sowie über andere uniform-Daten, die nur bei Kamerabewegungen aktualisiert werden, empfiehlt sich das Aufteilen der Daten auf zwei separate Konstantenpuffer.
-   Semantik, deren Anwendung Sie vergessen oder die Sie auf fehlerhafte Weise angewendet haben, ist die wichtigste Ursache für Fehler der Shaderkompilierung (FXC). Deshalb sollten Sie die Semantik auf jeden Fall noch einmal überprüfen. Die Dokumente können etwas verwirrend erscheinen, da viele ältere Seiten und Beispiele noch auf andere Versionen der HLSL-Semantik des Stands vor Direct3D 11 verweisen.
-   Stellen Sie sicher, dass Sie wissen, auf welche Direct3D-Featureebene Sie für einen Shader jeweils abzielen. Die Semantik für Feature-Ebene 9\_ \* unterscheiden sich von denen für 11\_1.
-   PA\_POSITION semantische löst der zugeordneten nach der Interpolation Position-Daten, um Werte zu koordinieren, wobei x zwischen 0 und die Breite der Render-Ziel, y ist zwischen 0 und dem Rendern ist abzielen, Höhe, Z wird durch die ursprünglichen homogene-Koordinate w geteilt Wert (Z/w) und w, 1 dividiert durch den ursprünglichen w-Wert (1/w) ist.

## <a name="related-topics"></a>Verwandte Themen


[Gewusst wie: einen Renderer mit einfachen OpenGL ES 2.0 zu Direct3D 11-port](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Portieren der Shaderobjekte](port-the-shader-config.md)

[Portieren der Vertexpuffer und -daten](port-the-vertex-buffers-and-data-config.md)

[Zeichnen auf den Bildschirm](draw-to-the-screen.md)

 

 




