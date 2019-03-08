---
title: Von den Streamingressourcen nicht unterstützte Schablonenformate
description: Formate, die Schablonen enthalten, werden von Streamingressourcen nicht unterstützt.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Von den Streamingressourcen nicht unterstützte Schablonenformate
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35813a6242abd555e87329c25a413285d1d948
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660985"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Von den Streamingressourcen nicht unterstützte Schablonenformate


Formate, die Schablonen enthalten, werden von Streamingressourcen nicht unterstützt.

Formate, die Schablone enthalten sind, DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (und verwandte Formate in der Familie R24G8) und DXGI\_FORMAT\_D32\_"Float"\_S8X24\_UINT (und verwandte Formate der R32G8X24-Familie).

Einige Implementierungen speichern Tiefen- und Schabloneninformationen in separaten Zuordnungen, andere speichern sie zusammen. Die Kachelverwaltung für die beiden Schemas müsste unterschiedlich sein, und keine API kann die Unterschiede abstrahieren oder rationalisieren. Wir empfehlen für zukünftige Hardware die Unterstützung unabhängiger Tiefen- und Schablonenoberflächen, die voneinander unabhängige Kacheln verwenden.

Die 32-Bit-Tiefe müsste 128 x 128 Kacheln und die 8-Bit Schablone müsste 256 x 256 Kacheln haben. Daher würde es Anwendungen mit einer Diskrepanz in der Kachelform zwischen Tiefe und Schablone geben. Aber das gleiche Problem besteht bereits bei anderen Formaten von Renderzieloberflächen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Streamen von Ressource Prozess- und gemeinsame Nutzung von Geräten](streaming-resource-cross-process-and-device-sharing.md)

 

 




