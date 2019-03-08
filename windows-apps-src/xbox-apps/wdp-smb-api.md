---
title: Device Portal - Referenz zu SMB-APIs
description: Erfahren Sie, wie Sie programmgesteuert auf die SMB-APIs zugreifen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: e248a6ff666efe7dca262daa81a21ab44a4dc5aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617705"
---
# <a name="developer-folder-api-reference"></a>Referenz zur API für den Entwicklerordner   
Für den Zugriff auf Dateien auf Ihrer Xbox One, die sich auf die Entwicklung beziehen, können Sie einen standardmäßigen Datei-Explorer verwenden. Dadurch können Sie problemlos Dateien von Ihrem PC anzeigen und auf der Konsole ersetzen.

**Anforderung**

Sie können mithilfe der folgenden Anforderung auf den Entwicklerordner zugreifen. Die Anforderung gibt Folgendes zurück:    
* Den Speicherort der Dateifreigabe. Dieser Speicherort kann in die Adressleiste eines Datei-Explorers eingegeben werden.
* Den Benutzernamen für den Zugriff auf die Dateifreigabe.
* Das Kennwort für den Zugriff auf die Dateifreigabe.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**   
Path: Der Pfad zur Dateifreigabe mit den Entwicklerdateien.   
Username: Der Benutzername für den Zugriff auf die Freigabe mit den Entwicklerdateien.   
Password: Das Kennwort für den Zugriff auf die Freigabe mit den Entwicklerdateien.   

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung für den Zugriff auf die Anmeldeinformationen für die Dateifreigabe wurde gewährt.
4XX | Fehlercodes
5XX | Fehlercodes
<br />
**Gerätefamilien verfügbar**

* Windows Xbox
