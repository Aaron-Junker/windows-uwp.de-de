---
title: Gerätetypen
description: Direct3D-Gerätetypen sind HAL-Gerät (Hardwareabstraktionsschicht) und den Referenz-Rasterizer.
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- Gerätetypen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e51b7ce49d9acba80e3ed5cb9e75a02662b5dad8
ms.sourcegitcommit: e58e27a12026454270ad9db2cf5746bcede7075d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77128089"
---
# <a name="device-types"></a>Gerätetypen


Direct3D-Gerätetypen sind HAL-Gerät (Hardwareabstraktionsschicht) und den Referenz-Rasterizer.

## <a name="span-idhal_devicespanspan-idhal_devicespanspan-idhal_devicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>Hal-Gerät


Der primäre Gerätetyp ist das HAL-Gerät. Dieses unterstützt die hardwarebeschleunigt Rasterung und die Hardware- und Software-Vertex-Verarbeitung. Wenn der Computer, auf dem die Anwendung ausgeführt wird, mit einer Grafikkarte mit Direct3D-Unterstützung ausgestattet ist, muss die Anwendung Direct3D-Vorgänge verwenden. Direct3D-Hal Geräte implementieren alle oder Teile der Transformations-, Beleuchtungs- und Rasterungsvorgänge über die Hardware.

Anwendungen greifen nicht direkt auf Grafikadapter zu. Sie rufen Direct3D-Funktionen und -Methoden auf. Direct3D greift über die HAL auf die Hardware zu. Wenn der Computer, auf dem die Anwendung ausgeführt wird, die Hal unterstützt, erhalten Sie durch die Nutzung des HAL-Gerätes eine optimale Leistung.

## <a name="span-idreference_devicespanspan-idreference_devicespanspan-idreference_devicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>Referenzgerät


Direct3D unterstützt ein zusätzliches Gerät – das Referenzgerät bzw. den Referen-Rasterizer. Im Gegensatz zu einem Softwaregerät unterstützt der Referenz-Rasterizer alle Direct3D-Funktionen. Dieses Gerät ist zum Debuggen gedacht und daher nur auf Computern verfügbar, auf denen das DirectX SDK installiert wurde. Da diese Funktionen mit dem Ziel einer möglichst hohen Genauigkeit statt für eine hohe Geschwindigkeit implementiert wurde, sind die Ergebnisse nicht besonders schnell. Der Referen-Rasterizer nutzt wenn möglich spezielle CPU-Anweisungen. Er ist jedoch nicht für den Einsatz in fertigen Anwendungen gedacht. Verwenden Sie die Referenz-Rasterizer nur für Featuretests oder zu Demonstrationszwecken.

## <a name="span-idhal_vs_refspanspan-idhal_vs_refspanspan-idhal_vs_refspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>HAL-und ref-Geräte


HAL-Geräte (Hardware Abstraction Layer, Hardwareabstraktionsschicht) und REF-Geräte (REFerence Rasterizer, Referenz-Rasterizer) stellen die beiden wichtigsten Typen von Direct3D-Geräten dar. HAL-Geräte arbeiten über Hardware und sind sehr schnell. Sie unterstützen jedoch möglicherweise nicht alle Elemente. REF-Geräte nutzen keine Hardwarebeschleunigung. Daher sind sie sehr langsam. Bei REF-Geräten ist jedoch sichergestellt, dass alle Direct3D-Funktionen korrekt unterstützt werden. In der Regel benötigen Sie nur HAL Geräte. Wenn Sie jedoch erweiterte Funktionen nutzen, die die Grafikkarte nicht unterstützt, dann müssen Sie unter Umständen auf ein REF-Gerät zurückgreifen.

Ein REF-Gerät können Sie auch dann verwenden, wenn HAL-Gerät ungewöhnliche Ergebnissen liefert. So können Sie prüfen, ob Ihr Code überhaupt in Ordnung ist. Das REF-Gerät verhält sich immer ordnungsgemäß. Sie können Ihre Anwendung daher über das REF-Gerät testen und dabei überprüfen, ob das ungewöhnliche Verhalten weiterhin auftritt. Wenn dies nicht der Fall ist, nutzt die Anwendung (a) eine nicht von der Grafikkarte unterstützte Funktion, oder es handelt sich (b) um einen Treiberfehler. Wenn der Code auch mit dem REF-Gerät nicht funktioniert, handelt es sich um einen Anwendungsfehler.

## <a name="span-idhardware_vs_softwarespanspan-idhardware_vs_softwarespanspan-idhardware_vs_softwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>Hardware-und Software Scheitelpunkt Verarbeitung


Der Unterschied zwischen der Hardware- und Software-Vertex-Verarbeitung betrifft nur HAL-Geräte. Wenn Sie Vertizes in die Pipeline schieben, müssen diese transformiert (über die Welt-, View- und Projektionsmatrizen) und beleuchtet (über die integrierte Beleuchtung von D3D) werden. Dieser Verarbeitungsschritt heiß T&L (Transformation & Lighting). Die Verarbeitung von Hardware Scheitel Punkten bedeutet, dass dies auf Hardware erfolgt, wenn Sie von der Hardware unterstützt wird. ERGO die Verarbeitung von Software Scheitel Punkten erfolgt in Software. Normalerweise wird zuerst ein Hardware-T&L-Gerät erstellt. Falls dieses fehlschlägt wird ein Mixed-Gerät verwendet. Schlägt auch dies fehl, wird ein Software-Gerät verwendet. (Fall das Software-Gerät fehlschlägt wird der Vorgang mit einem Fehler beeendet.)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 




