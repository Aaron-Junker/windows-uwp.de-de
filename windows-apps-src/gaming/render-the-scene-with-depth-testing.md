---
title: Rendern der Szene mit Tiefentest
description: Erstellen Sie einen Schatteneffekt, indem Sie dem Vertex-Shader (bzw. Geometry-Shader) und dem Pixel-Shader einen Tiefentest hinzufügen.
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Rendern, Szene, Tiefentest, Direct3D, Schatten
ms.localizationpriority: medium
ms.openlocfilehash: d1c2c4e5d45b28c318085f4ce257b587f23f1426
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368099"
---
# <a name="render-the-scene-with-depth-testing"></a>Rendern der Szene mit Tiefentest




Erstellen Sie einen Schatteneffekt, indem Sie dem Vertex-Shader (bzw. Geometry-Shader) und dem Pixel-Shader einen Tiefentest hinzufügen. Teil 3 von [Exemplarische Vorgehensweise: Implementieren Sie mithilfe von Tiefenpuffern in Direct3D 11 Schattenvolumen](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="include-transformation-for-light-frustum"></a>Einfügen der Transformation für Licht-Frustum


Ihr Vertex-Shader muss für jeden Scheitelpunkt (Vertex) die transformierte Position im Lichtraum berechnen. Geben Sie die Modell-, Ansichts- und Projektionsmatrizen für den Lichtraum mithilfe eines Konstantenpuffers an. Sie können diesen Konstantenpuffer auch verwenden, um die Lichtposition und Normale für Lichtberechnungen bereitzustellen. Die transformierte Position im Lichtraum wird während des Tiefentests verwendet.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

Als Nächstes wird vom Pixelshader die vom Vertex-Shader bereitgestellte interpolierte Lichtraumposition verwendet, um zu testen, ob das Pixel im Schatten liegt.

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>Testen, ob sich die Position im Licht-Frustum befindet


Prüfen Sie zuerst, ob sich das Pixel im Ansichts-Frustum des Lichts befindet, indem Sie die X- und Y-Koordinaten normalisieren. Wenn beide innerhalb des Bereichs \[0, 1\] ist es möglich, dass das Pixel im Schatten. Andernfalls können Sie den Tiefentest überspringen. Mit einem Shader kann dies ohne viel Zeitaufwand getestet werden, indem [Saturate](https://docs.microsoft.com/windows/desktop/direct3dhlsl/saturate) aufgerufen und das Ergebnis mit dem Originalwert verglichen wird.

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>Tiefentest unter Verwendung der Schattenmap


Verwenden Sie eine Samplevergleichsfunktion (entweder [SampleCmp](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmp) oder [SampleCmpLevelZero](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)), um die Tiefe des Pixels im Lichtraum unter Verwendung der Tiefenmap zu testen. Berechnen Sie den normalisierten Lichtraum-Tiefenwert, für den `z / w` gilt, und übergeben Sie den Wert an die Vergleichsfunktion. Da wir für den Sampler einen LessOrEqual-Vergleichstest verwenden, wird von der systeminternen Funktion null zurückgegeben, wenn der Vergleichstest bestanden wurde; dies gibt an, dass das Pixel im Schatten liegt.

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>Berechnen der Beleuchtung innerhalb und außerhalb des Schattens


Wenn das Pixel nicht im Schatten liegt, kann der Pixelshader die direkte Beleuchtung berechnen und dem Pixelwert hinzufügen.

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

Andernfalls sollte der Pixelshader den Pixelwert mithilfe des Umgebungslichts berechnen.

```cpp
return float4(input.color * ambient, 1.f);
```

Der nächste Teil dieser exemplarischen Vorgehensweise beschäftigt sich mit dem [Unterstützen von Schattenmaps für unterschiedliche Hardware](target-a-range-of-hardware.md).

 

 




