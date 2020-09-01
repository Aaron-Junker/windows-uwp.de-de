---
title: Referenz zur Medienerfassungs-API
description: Erfahren Sie, wie Sie eine PNG-Darstellung des aktuellen Bildschirms mithilfe der Xbox-Geräte Portal-Rest-API erfassen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: ee1ccba3fe2a3f83a95c3538cb267730f7770c4c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172734"
---
# <a name="media-capture-api-reference"></a>Referenz zur Medienerfassungs-API #

## <a name="request"></a>Anforderung

Mithilfe des folgenden Anforderungsformats können Sie eine PNG-Darstellung des aktuellen Bildschirms erfassen.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:


| URI-Parameter      | BESCHREIBUNG     | 
| ------------------ |-----------------|
| download (optional)| Ein boolescher Wert, der angibt, ob HTTP-Antwortheader festgelegt werden sollen, die angeben, dass der Hostbrowser den Screenshot als Anhang herunterladen soll, anstatt ihn im Browser zu rendern.  |

**Anforderungsheader**

* Keine

**Anforderungstext**

* Keine

## <a name="response"></a>Antwort

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | BESCHREIBUNG     | 
| ------------------ |-----------------|
| 200                | Die Screenshotanforderung war erfolgreich, und der erfasste Inhalt wurde zurückgegeben. |
| 5XX                | Fehlercodes für unerwartete Fehler |
<br>

**Verfügbare Gerätefamilien**

* Windows Xbox

