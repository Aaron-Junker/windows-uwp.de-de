---
title: Referenz zur Medienerfassungs-API
description: Erfahren Sie, wie Sie programmgesteuert auf die Medienerfassungs-API zugreifen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 7a27d13f7ceedd14a84d5b4b4aa1233445037a1f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640765"
---
# <a name="media-capture-api-reference"></a>Referenz zur Medienerfassungs-API #

**Anforderung**

Mithilfe des folgenden Anforderungsformats können Sie eine PNG-Darstellung des aktuellen Bildschirms erfassen.

| Methode        | Anforderungs-URI     | 
| ------------- |-----------------|
| GET           | /ext/screenshot |
<br>

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:


| URI-Parameter      | Beschreibung     | 
| ------------------ |-----------------|
| download (optional)| Ein boolescher Wert, der angibt, ob HTTP-Antwortheader festgelegt werden sollen, die angeben, dass der Hostbrowser den Screenshot als Anhang herunterladen soll, anstatt ihn im Browser zu rendern.  |
<br>

**Anforderungsheader**

* Keine

**Anforderungstext**

* Keine

###<a name="response"></a>Antwort ###

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode   | Beschreibung     | 
| ------------------ |-----------------|
| 200                | Die Screenshotanforderung war erfolgreich, und der erfasste Inhalt wurde zurückgegeben. |
| 5XX                | Fehlercodes für unerwartete Fehler |
<br>

**Gerätefamilien verfügbar**

* Windows Xbox

