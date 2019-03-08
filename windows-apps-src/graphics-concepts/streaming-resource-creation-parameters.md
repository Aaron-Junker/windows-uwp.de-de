---
title: Parameter für das Erstellen von Streamingressourcen
description: Es gibt einige Einschränkungen für den Direct3D-Ressourcentyp, den Sie als Streamingressource erstellen können.
ms.assetid: 6FC5AD93-6F47-479E-947C-895C99B427BC
keywords:
- Parameter für das Erstellen von Streamingressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1ddb150e570e25af7162a50309b9b0fc30cedf60
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617335"
---
# <a name="streaming-resource-creation-parameters"></a>Parameter für das Erstellen von Streamingressourcen


Es gibt einige Einschränkungen für den Direct3D-Ressourcentyp, den Sie als Streamingressource erstellen können.

<span id="Supported-Resource-Type"></span><span id="supported-resource-type"></span><span id="SUPPORTED-RESOURCE-TYPE"></span>**Unterstützter Ressourcentyp.**  
Texture2D\[Array\] (einschließlich TextureCube\[Array\], dies ist eine Variante des Texture2D\[Array\]) oder einen Puffer.

**NICHT unterstützt:  **Texture1D\[Array\] .

<span id="Supported-Resource-Usage"></span><span id="supported-resource-usage"></span><span id="SUPPORTED-RESOURCE-USAGE"></span>**Unterstützte Ressourcenverwendung**  
Standardverwendung.

**NICHT unterstützt:  **dynamische, staging oder unveränderlich.

<span id="Supported-Resource-Misc-Flags"></span><span id="supported-resource-misc-flags"></span><span id="SUPPORTED-RESOURCE-MISC-FLAGS"></span>**Unterstützte Ressourcen Verschiedenes Flags**  
Unterteilt; d. h. Streaming (gemäß Definition), Texturwürfel, indirekte Argumente zeichnen, Puffer lässt Rohdatenansichten zu, strukturierter Puffer, Ressourcenklammerung oder Mips generieren.

**NICHT unterstützt:  **freigegeben, freigegeben schlüsselgebundene Mutex, GDI-kompatibel, NT-Handle freigegeben, eingeschränkter Inhalte, freigegebene Ressource zu beschränken, beschränken Sie die freigegebene Ressource Treiber, geschützt oder Kachel Pool.

<span id="Supported-Bind-Flags"></span><span id="supported-bind-flags"></span><span id="SUPPORTED-BIND-FLAGS"></span>**Unterstützte Bindungsflags**  
Als Shaderressource binden, Renderziel, Tiefenschablone oder unsortierter Zugriff.

**NICHT unterstützt:  **Bindung als Konstantenpuffer, Vertexpuffer (Binden einen unterteilten Puffer aus, wie Sie einen SRV/UAV/RTV unterstützt wird), Index Puffer, datenstromausgabe, video-Encoder oder Decoder.

<span id="Supported-Formats"></span><span id="supported-formats"></span><span id="SUPPORTED-FORMATS"></span>**Zu den unterstützten Formaten**  
Alle Formate, die für die gegebene Konfiguration verfügbar sind, unabhängig davon, ob sie eine geteilte Anordnung verwenden, mit einigen Ausnahmen.

<span id="Supported-Sample-Description--Multisample-count--quality-"></span><span id="supported-sample-description--multisample-count--quality-"></span><span id="SUPPORTED-SAMPLE-DESCRIPTION--MULTISAMPLE-COUNT--QUALITY-"></span>**Unterstützte Beispielbeschreibung (Anzahl der für Multisampling, Qualität)**  
Alles, was für die gegebene Konfiguration unterstützt wird, unabhängig davon, ob eine geteilte Anordnung verwendet wird, mit einigen Ausnahmen.

<span id="Supported-Width-Height-MipLevels-ArraySize"></span><span id="supported-width-height-miplevels-arraysize"></span><span id="SUPPORTED-WIDTH-HEIGHT-MIPLEVELS-ARRAYSIZE"></span>**Unterstützte Breite/Höhe/MipLevels/ArraySize**  
Im vollen Umfang von Direct3D unterstützt. Streamingressourcen unterliegen nicht der Beschränkung auf die Gesamtspeichergröße wie Nicht-Streamingressourcen. Streamingressourcen werden nur durch die Grenzen des virtuellen Gesamtadressraums beschränkt. Siehe [Zuordnen des verfügbaren Speicherplatzes für Streamingressourcen](address-space-available-for-streaming-resources.md).

Der anfängliche Inhalt des Kachelpoolspeichers ist nicht definiert.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="address-space-available-for-streaming-resources.md">Adressraum für das streaming von Ressourcen</a></p></td>
<td align="left"><p>In diesem Abschnitt wird der virtuelle Adressraum angegeben, der für Streamingressourcen verfügbar ist.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Erstellen von streaming-Ressourcen](creating-streaming-resources.md)

 

 




