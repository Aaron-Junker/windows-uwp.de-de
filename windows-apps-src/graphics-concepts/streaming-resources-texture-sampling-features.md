---
title: Textursampling-Features für Streamingressourcen
description: Zu den Funktionen für die Textur Stichproben für Streaming-Ressourcen gehört das Abrufen des shaderstatus-Feedbacks zu zugeordneten Bereichen, das überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurde, das Anspannen an hilfshader, um Bereiche in falsch zugeordneten Streamingressourcen zu vermeiden, die bekanntermaßen nicht zugeordnet sind
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Textursampling-Features für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 823b7b6ba7835f62277e15fa41fb968c6f4a167f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173044"
---
# <a name="streaming-resources-texture-sampling-features"></a>Textursampling-Features für Streamingressourcen


Zu den Funktionen für die Textur Stichproben für Streaming-Ressourcen gehört das Abrufen des shaderstatus-Feedbacks zu zugeordneten Bereichen, das überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurde, das Anspannen an hilfshader, um Bereiche in falsch zugeordneten Streamingressourcen zu vermeiden, die bekanntermaßen nicht zugeordnet sind

## <a name="span-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Anforderungen an Streaming-Ressourcen Textur-Samplings


Die hier beschriebenen Funktionen für Textur Stichproben erfordern eine Ebene der Ebene [2](tier-2.md) für Streaming-Ressourcen.

## <a name="span-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Shader-Status Feedback zu zugeordneten Bereichen


Jede shaderanweisung zum Lesen und/oder schreiben in eine streamingressource bewirkt, dass Statusinformationen aufgezeichnet werden. Dieser Status wird als optionaler zusätzlicher Rückgabewert für jede Ressourcen Zugriffs Anweisung verfügbar gemacht, die in ein Temp-32-Bit-Register übergeht. Der Inhalt des Rückgabewerts ist nicht transparent. Das heißt, das direkte Lesen durch das Shader-Programm ist nicht zulässig. Sie können jedoch die [**checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) -Funktion verwenden, um die Statusinformationen zu extrahieren.

## <a name="span-idfully_mapped_checkspanspan-idfully_mapped_checkspanspan-idfully_mapped_checkspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Vollständige Zuordnung überprüfen


Die [**checkaccessfullymapping**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) -Funktion interpretiert den von einem Speicherzugriff zurückgegebenen Status und gibt an, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden. **Checkaccessfullymapping** gibt true (0xFFFFFFFF) zurück, wenn Daten zugeordnet wurden, oder false (0x00000000), wenn die Daten nicht zugeordnet wurden.

Bei Filter Vorgängen ist die Gewichtung eines bestimmten Texttyps manchmal 0,0. Ein Beispiel hierfür ist ein lineares Beispiel mit Texturkoordinaten, die sich direkt in einem texturcenter befinden: 3 andere texeln (die von der Hardware abweichen können) tragen zum Filter bei, aber mit 0 Gewichtung. Diese 0-Gewichtungs texeln tragen überhaupt nicht zum Filterergebnis bei. Wenn Sie also auf **null** -Kacheln fallen, werden Sie nicht als nicht zugeordneter Zugriff gezählt. Beachten Sie, dass die gleiche Garantie für Textur Filter gilt, die mehrere MIP-Ebenen einschließen. Wenn die texeln in einer der Mipmaps nicht zugeordnet sind, aber die Gewichtung für diese texeln gleich 0 ist, werden diese texeln nicht als nicht zugeordneter Zugriff gezählt.

Wenn die Stichprobenentnahme von einem Format mit weniger als 4 Komponenten (z. b. DXGI- \_ Format \_ R8 \_ unorm) erfolgt, führen alle texeln, die auf **null** -Kacheln fallen, dazu, dass ein **null** zugeordneter Zugriff gemeldet wird, unabhängig davon, welche Komponenten der Shader tatsächlich im Ergebnis untersucht. Beispielsweise scheint das Lesen aus dem R8 \_ -unorm und das Maskieren des Lese Ergebnisses im Shader mit. GBA/. yzw die Textur überhaupt nicht zu lesen. Wenn die textrece jedoch eine **null** zugeordnete Kachel ist, wird der Vorgang weiterhin als **null** -Zuordnungs Zugriff gezählt.

Der Shader kann den Status überprüfen und jede gewünschte Vorgehensweise bei einem Fehler verfolgen. Eine Vorgehensweise kann z. b. "Fehler" protokollieren (z. b. über den UAV-Schreibvorgang) und/oder einen anderen Lesevorgang an eine gröbere-Lod ausgeben, die bekanntermaßen zugeordnet ist. Eine Anwendung kann auch erfolgreiche Zugriffe nachverfolgen, um einen Eindruck davon zu erhalten, auf welchen Teil des zugeordneten Satzes von Kacheln zugegriffen wurde.

Eine Komplikation für die Protokollierung besteht darin, dass kein Mechanismus vorhanden ist, mit dem der genaue Satz von Kacheln gemeldet werden kann Die Anwendung kann konservative Schätzwerte treffen, wenn Sie die für den Zugriff verwendeten Koordinaten kennen und die Lod-Anweisung verwenden. Beispielsweise gibt [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)) die Berechnung der Hardware-Lod zurück.

Eine weitere Komplikation ist, dass viele Zugriffe auf die gleichen Kacheln erfolgen, sodass eine Menge redundanter Protokollierung und möglicherweise Konflikte im Speicher stattfindet. Es kann praktisch sein, wenn der Hardware die Möglichkeit eingeräumt wird, sich nicht auf den Zugriff auf Berichts Kacheln zu kümmern, wenn Sie an anderer Stelle als gemeldet wurden. Möglicherweise könnte der Status einer solchen Nachverfolgung von der API zurückgesetzt werden (wahrscheinlich an Frame Grenzen).

## <a name="span-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Minlod-Klammer pro Stichprobe


Um Shader bei der Vermeidung von Bereichen in falsch zugeordneten Streamingressourcen zu unterstützen, die bekanntermaßen nicht zugeordnet sind, haben die meisten shaderanweisungen mit einem Sampler (Filterung) einen Modus, der es dem Shader ermöglicht, einen zusätzlichen float32 minlod-Klammer Parameter an das Textur Beispiel zu übergeben. Dieser Wert befindet sich im Gegensatz zur zugrunde liegenden Ressource im MipMap-Nummernbereich der Ansicht.

Die Hardware ` max(fShaderMinLODClamp,fComputedLOD) ` wird an derselben Stelle in der Lod-Berechnung ausgeführt, bei der die minlod-Klammer pro Ressource auftritt. Dies ist auch ein [**Maximum**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)().

Wenn das Ergebnis der Anwendung der Lod-Klammer pro Stichprobe und sämtlicher anderer im Sampler definierter Lod-Klammern eine leere Menge ist, ist das Ergebnis das gleiche out-of-Limit-Zugriffs Ergebnis wie die pro-Ressource-minlod-Klammer: 0 für Komponenten im Oberflächen Format und Standardwerte für fehlende Komponenten.

Die Lod-Anweisung (z. b. [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)), in der die hier beschriebene minlod-Klammer für Stichproben (z. b Die von dieser Lod-Anweisung zurückgegebene gebundene Lod spiegelt alle Klammern wider, einschließlich der pro-Ressource-Klammer, aber keine pro-Stichproben-Klammer. Die pro-Stichproben-Klammer wird gesteuert und wird vom Shader trotzdem bekannt, sodass der Shader-Autor diese Klammer bei Bedarf manuell auf den Rückgabewert der Lod-Anweisung anwenden kann.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Minimale/maximale Reduzierungs Filterung


Anwendungen können Ihre eigenen Datenstrukturen verwalten, die Sie darüber informieren, wie die Zuordnungen für eine streamingressource aussehen. Ein Beispiel wäre eine Oberfläche, die ein Texel enthält, das Informationen für jede Kachel in einer streamingressource enthalten soll. Eine kann die erste Lod speichern, die an einem bestimmten Kachel Speicherort zugeordnet ist. Durch die sorgfältige Stichprobenentnahme dieser Datenstruktur auf die gleiche Weise, wie die streamingressource als Stichprobe verwendet werden soll, könnte man ermitteln, welche der minimalen Lod, die für einen vollständigen Textur Filter vollständig zugeordnet ist, ist. Um diesen Prozess zu vereinfachen, bietet Direct3D 11,2 einen neuen allgemeinen Samplingmodus, Min/Max-Filterung.

Das Hilfsprogramm der Min/Max-Filterung für die Lod-Nachverfolgung kann für andere Zwecke nützlich sein, z. b. das Filtern von tiefen Oberflächen.

Die minimale/maximale Reduzierungs Filterung ist ein Modus für Samplern, der denselben textursatz abruft, der von einem normalen Textur Filter abgerufen wird. Anstatt jedoch die Werte zu mischen, um eine Antwort zu erhalten, gibt Sie die minimale () oder die maximale Anzahl () der texeln zurück, die pro Komponente abgerufen werden (z. b. die minimale Anzahl aller R-Werte, getrennt von der Summe aller G-Werte usw.).

Die Min/Max-Vorgänge folgen den Regeln für die arithmetische Genauigkeit Direct3D. Die Reihenfolge der Vergleiche spielt keine Rolle.

Bei Filter Vorgängen, bei denen es sich nicht um min/max handelt, ist die Gewichtung eines bestimmten Texttyps manchmal 0,0. Ein Beispiel hierfür ist ein lineares Beispiel mit Texturkoordinaten, die direkt in ein texturcenter fallen. in diesem Fall sind drei andere texeln (die je nach Hardware variieren können) zum Filter beitragen, aber mit 0 Gewichtung. Bei allen diesen texeln, bei denen es sich um 0 (null) bei einem nicht-Min/Max-Filter handeln würde, tragen diese textteln nach wie vor nicht zum Ergebnis bei, und die Gewichtungen wirken sich nicht auf den Min/Max-Filter Vorgang aus.

Die Unterstützung für dieses Feature hängt von der Unterstützung der [Ebene 2](tier-2.md) für Streamingressourcen ab

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipelinezugriff auf Streamingressourcen](pipeline-access-to-streaming-resources.md)

 

 