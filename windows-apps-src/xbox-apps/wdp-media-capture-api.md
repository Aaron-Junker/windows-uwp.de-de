---
title: Referenz zur Medienerfassungs-API
description: Erfahren Sie, wie Sie eine PNG-Darstellung des aktuellen Bildschirms mithilfe der Xbox-Geräte Portal-Rest-API erfassen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 5d5cd87b95f923fc0303d5cd4ed7ecbff70b8209
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254195"
---
# <a name="media-capture-api-reference"></a>Referenz zur Medienerfassungs-API

## <a name="request"></a>Anforderung

Mithilfe des folgenden Anforderungsformats können Sie eine PNG-Darstellung des aktuellen Bildschirms erfassen.

| Methode        | Anforderungs-URI     |
| ------------- |-----------------|
| GET           | /ext/screenshot |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter       | Beschreibung     |
| ------------------- |-----------------|
| download (optional) | Ein boolescher Wert, der angibt, ob HTTP-Antwortheader festgelegt werden sollen, die angeben, dass der Hostbrowser den Screenshot als Anhang herunterladen soll, anstatt ihn im Browser zu rendern. |

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

**Verfügbare Gerätefamilien**

* Windows Xbox
