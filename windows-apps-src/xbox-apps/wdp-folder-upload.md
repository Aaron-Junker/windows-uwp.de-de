---
title: Device Portal - Referenz zu den APIs zum Hochladen von Ordnern
description: Erfahren Sie, wie Sie mithilfe der Xbox-Geräte Portal-Rest-API einen Ordner in das Entwicklungs Verzeichnis hochladen können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: b71f60350bf5c8318adb2a4741bb1a275a4b0276
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157754"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Hochladen eines Ordners in das Entwicklungsverzeichnis

**Anforderung**

Sie können einen vollständigen Ordner unter der ID für bekannte Ordner in den Ordner „DevelopmentFiles“ (oder in einen Unterordner innerhalb dieses Ordners) auf einmal hochladen.

Methode      | Anforderungs-URI
:------     | :------
POST | /api/app/packagemanager/upload 

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

URI-Parameter      | BESCHREIBUNG
:------     | :-----
destinationFolder (erforderlich) | Der Name des Zielordners für den Ordner, der hochgeladen werden soll. Dieser Ordner wird unter „d:\developmentfiles\LooseApps“ auf der Konsole gespeichert. Der Ordnername muss base64-codiert sein, da er Pfadtrennzeichen enthalten kann, wenn der Ordner ein Unterordner unter „LooseApps“ ist.


**Anforderungsheader**

- Keine

**Anforderungstext**

- Multipart-konformer HTTP-Text des Verzeichnisinhalts.

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Erfolg
4XX | Fehlercodes
5XX | Fehlercodes

**Verfügbare Gerätefamilien**

* Windows Xbox

