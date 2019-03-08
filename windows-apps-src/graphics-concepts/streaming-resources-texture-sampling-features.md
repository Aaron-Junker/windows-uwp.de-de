---
title: Textursampling-Features für Streamingressourcen
description: 'Textursampling-Features für Streamingressourcen enthalten: Abrufen von Feedback zum Shaderstatus zugeordneter Bereiche, Überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden, Klammerung, damit Shader Bereiche in Mipmap-Streamingressourcen vermeiden, die nicht zugeordnet wurden, und Ermitteln der minimalen Detailtiefe (Level-of-Detail, LOD), die für den gesamten Speicherbedarf einer Texturfilterung vollständig zugeordnet ist.'
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Textursampling-Features für Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b6290fba9d4194df78c39902b8d96e952134682
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607415"
---
# <a name="streaming-resources-texture-sampling-features"></a>Textursampling-Features für Streamingressourcen


Textursampling-Features für Streamingressourcen enthalten: Abrufen von Feedback zum Shaderstatus zugeordneter Bereiche, Überprüfen, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden, Klammerung, damit Shader Bereiche in Mipmap-Streamingressourcen vermeiden, die nicht zugeordnet wurden, und Ermitteln der minimalen Detailtiefe (Level-of-Detail, LOD), die für den gesamten Speicherbedarf einer Texturfilterung vollständig zugeordnet ist.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Anforderungen für das streaming von Ressourcen texture-Sampling-Funktionen


Die hier beschriebenen Textursampling-Features benötigen die [Ebene 2](tier-2.md) der Streamingressourcenunterstützung.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Shader-Status-Feedback zur zugeordneten Bereiche


Alle Shaderanweisungen, die eine Streamingressource lesen oder in diese schreiben, führen dazu, dass Statusinformationen aufgezeichnet werden. Dieser Status wird als zusätzlicher optionaler Rückgabewert für jede Anweisung zum Zugreifen auf eine Ressource verfügbar gemacht, die in ein temporäres 32-Bit-Register übernommen wird. Der Inhalt des Rückgabewerts ist nicht transparent. Das heißt, ein direktes Lesen durch das Shaderprogramm ist nicht zulässig. Aber Sie können die Statusinformationen mit der Funktion [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) extrahieren.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Vollständig zugeordneten überprüfen


Die Funktion [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interpretiert den von einem Speicherzugriff zurückgegebenen Status und gibt an, ob alle Daten, auf die zugegriffen wird, in der Ressource zugeordnet wurden. **CheckAccessFullyMapped** gibt „true” (0xFFFFFFFF) zurück, wenn Daten zugeordnet wurden, oder „false” (0x00000000), wenn Daten nicht zugeordnet wurden.

Bei Filterungsvorgängen ist die Gewichtung eines bestimmten Texels manchmal 0,0. Ein Beispiel ist ein Beispiel für lineare mit Texturkoordinaten, die direkt auf ein Texel Center fallen: 3 andere Texel (welche werden je nach Hardware variieren können) beitragen, den Filter, aber mit Gewichtung von 0. Diese Texel mit der Gewichtung 0 tragen nicht zum Filterungsergebnis bei. Wenn sie also auf **NULL**-Kacheln fallen, werden sie nicht als nicht zugeordneter Zugriff gezählt. Die gleiche Garantie gilt für Texturfilter, die mehrere Mip-Ebenen enthalten. Wenn die Texel für eines der Mipmaps nicht zugeordnet sind, aber die Gewichtung für diese Texel 0 ist, zählen diese Texel nicht als nicht zugeordneter Zugriff.

Wenn für den Stichproben aus einem Format, das weniger als 4 Komponenten verfügt (z. B. DXGI\_FORMAT\_R8\_UNORM), alle texeln, die auf fallen **NULL** Kacheln zu der eine **NULL** zugeordnet Zugriff wird gemeldet, unabhängig davon, welche Komponenten des Shaders tatsächlich in das Ergebnis prüft. Also z.B. zum Lesen von R8\_UNORM und maskiert das Lese Ergebnis in der Shader mit.gba/.yzw wäre nicht angezeigt werden, müssen Sie die Textur zu lesen. Wenn die Adresse des Texels aber eine **NULL**-zugeordnete Kachel ist, wird der Vorgang trotzdem als **NULL**-Zuordnungszugriff gewertet.

Der Shader kann den Status überprüfen und bei einem Fehler alle gewünschten Vorgehensweise fortsetzen. Zum Beispiel kann eine Vorgehensweise die Protokollierung von „Fehlern” (z. B. über UAV-Schreibvorgänge) und/oder Ausführen eines weiteren Lesevorgangs mit der Klammerung an eine undifferenziertere zugeordnete LOD sein. Eine Anwendung könnte erfolgreiche Zugriffe auch nachverfolgen, um zu ermitteln, auf welchen Teil der zugeordneten Gruppe von Kacheln zugegriffen wurde.

Ein Problem für die Protokollierung besteht darin, dass kein Mechanismus vorhanden ist, um die genaue Gruppe von Kacheln zu erfassen, auf die zugegriffen worden wäre. Die Anwendung kann vorsichtige Schätzungen auf Basis der Kenntnis der Koordinaten vornehmen, die für den Zugriff verwendet wurden, sowie die LOD-Anweisung verwenden. Zum Beispiel gibt [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)) die Hardware-LOD-Berechnung zurück.

Ein weiteres Problem besteht darin, dass viele Zugriffe auf dieselben Kacheln erfolgen, sodass viele redundante Protokolleinträge und möglicherweise Konflikte im Arbeitsspeicher auftreten. Es wäre praktisch, wenn die Hardware über eine Option verfügen würde, Kachelzugriffe nicht zu erfassen, wenn diese schon vorher an anderer Stelle erfasst wurden. Vielleicht könnte der Status einer derartigen Verfolgung von der API (wahrscheinlich an Framegrenzen) zurückgesetzt werden.

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Pro-Sample MinLOD clamp


Damit Shader Bereiche in Mipmap-Streamingressourcen vermeiden, die nicht zugeordnet wurden, verfügen die meisten Shaderanweisungen, bei denen ein Sampler (Filterung) verwendet wird, über einen Modus, in dem der Shader einen zusätzlichen float32-Parameter für die MinLOD-Klammerung an das Texturbeispiel übergeben kann. Dieser Wert befindet sich im Mipmap-Nummernraum der Ansicht, im Gegensatz zu der zugrunde liegenden Ressource.

Die Hardware führt` max(fShaderMinLODClamp,fComputedLOD) `an derselben Position in der LOD-Berechnung aus, an der die MinLOD-Klammerung pro Ressource auftritt, was auch ein [**max**](https://msdn.microsoft.com/library/windows/desktop/bb509624)() darstellt.

Wenn das Ergebnis der Anwendung, die pro-Sample LOD Clamp und alle anderen LOD Spannpratzen Farbmuster definiert eine leere Menge ist, ist das Ergebnis der Out-of Grenzen Zugriff Ergebnis wie die einzelnen Ressourcen MinLOD Clamp: 0 für Komponenten in die Oberfläche Format und die Standardwerte für fehlende Komponenten.

Die LOD-Anweisung (z. B. [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)), die der hier beschriebenen MinLOD-Klammerung pro Sampling zeitlich vorausgeht, gibt eine LOD mit und ohne Klammerung zurück. Die von dieser LOD-Anweisung zurückgegebene geklammerte LOD gibt die gesamte in der Klammerung pro Ressource enthaltene Klammerung zurück, aber keine Klammerung pro Sampling. Die Klammerung pro Sampling wird vom Shader gesteuert und ist ihm bekannt, daher kann der Autor des Shaders diese Klammerung bei Bedarf manuell auf den Rückgabewert der LOD-Anweisung anwenden.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Minimale/maximale Reduzierung filtern


In Anwendungen können eigene Datenstrukturen verwaltet werden, die Informationen über die Zuordnungen für eine Streamingressource liefern. Ein Beispiel wäre eine Oberfläche, die ein Texel zum Speichern von Informationen für jede Kachel in einer Streamingressource enthält. Man könnte die erste LOD speichern, die einer bestimmten Kachelposition zugeordnet ist. Durch sorgfältiges Sampling dieser Datenstruktur auf ähnliche Weise, wie die Streamingressource gesampelt werden soll, kann die minimale LOD, die für den gesamten Speicherbedarf einer Texturfilterung vollständig zugeordnet ist, ermittelt werden. Um diesen Prozess zu vereinfachen, führt Direct3D 11.2 einen neuen allgemeinen Samplermodus, die Min/Max-Filterung, ein.

Die Min/Max-Filterung für die LOD-Verfolgung kann auch für andere Zwecke nützlich sein, wie z. B. für das Filtern von Tiefenoberflächen.

Die Min/Max-Reduzierungsfilterung ist ein Modus für Sampler, der die gleiche Gruppe von Texeln abruft, die ein normaler Texturfilter abrufen würde. Aber anstatt die Werte zu mischen, um eine Antwort zu erstellen, gibt dieser Modus die abgerufenen min()- oder max()-Texel pro Komponente zurück (z. B. das Minimum aller R-Werte, getrennt vom Minimum aller G-Werte usw.).

Für Min/Max-Operationen gelten die arithmetischen Genauigkeitsregeln von Direct3D. Die Reihenfolge der Vergleiche spielt keine Rolle.

Bei Filterungsvorgängen, die keine Min/Max-Operationen sind, ist die Gewichtung eines bestimmten Texels manchmal 0,0. Ein Beispiel ist ein lineares Muster mit Texturkoordinaten, die direkt auf die Mitte eines Texels fallen: 3 andere Texel (welche das sind, variiert je nach Hardware) tragen zur Filterung bei, aber mit einer Gewichtung von 0. Für alle diese Texel, deren Gewichtung für eine andere als die Min/Max-Filterung 0 wäre, würden diese Texel, auch wenn eine Min/Max-Filterung vorhanden wäre, trotzdem nicht zum Ergebnis beitragen (und die Gewichtungen wirken sich nicht anderweitig auf die Min/Max-Filterungsoperation aus).

Für die Unterstützung dieses Features wird die [Ebene 2](tier-2.md) der Streamingressourcenunterstützung benötigt.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Pipeline-Zugriff auf Ressourcen streaming](pipeline-access-to-streaming-resources.md)

 

 




