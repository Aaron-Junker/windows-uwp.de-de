---
title: Nicht unterstützte Schablonen Formate mit Streamingressourcen
description: Formate, die Schablonen enthalten, werden von Streamingressourcen nicht unterstützt.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Von den Streamingressourcen nicht unterstützte Schablonenformate
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db6476afe265ea4a2556a5f787a14daa2e212e61
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735135"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Von den Streamingressourcen nicht unterstützte Schablonenformate


Formate, die Schablonen enthalten, werden von Streamingressourcen nicht unterstützt.

Zu den Formaten, die die Schablone enthalten, gehören DXGI\_Format\_D24\_unorm\_S8\_uint (und Verwandte Formate in der R24G8-Familie) und DXGI\_Format\_d32\_float\_S8X24\_uint (und Verwandte Formate in der R32G8X24-Familie).

Einige Implementierungen speichern Tiefen- und Schabloneninformationen in separaten Zuordnungen, andere speichern sie zusammen. Die Kachelverwaltung für die beiden Schemas müsste unterschiedlich sein, und keine API kann die Unterschiede abstrahieren oder rationalisieren. Wir empfehlen für zukünftige Hardware die Unterstützung unabhängiger Tiefen- und Schablonenoberflächen, die voneinander unabhängige Kacheln verwenden.

Die 32-Bit-Tiefe müsste 128 x 128 Kacheln und die 8-Bit Schablone müsste 256 x 256 Kacheln haben. Daher würde es Anwendungen mit einer Diskrepanz in der Kachelform zwischen Tiefe und Schablone geben. Aber das gleiche Problem besteht bereits bei anderen Formaten von Renderzieloberflächen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Prozessübergreifende und Geräte Freigabe für Streamingressourcen](streaming-resource-cross-process-and-device-sharing.md)

 

 




