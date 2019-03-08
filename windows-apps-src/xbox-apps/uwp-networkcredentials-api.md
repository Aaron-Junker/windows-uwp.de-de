---
title: Referenz zur API für Netzwerkanmeldeinformationen des Geräteportals
description: Erfahren Sie, wie Sie Netzwerkanmeldeinformationen programmgesteuert hinzufügen, entfernen oder aktualisieren.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: ac30d8db830c51ee40653feb49b443ed44502617
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659215"
---
# <a name="network-credentials-api-reference"></a>Referenz zur API für Netzwerkanmeldeinformationen
Sie können gespeicherte Netzwerkanmeldeinformationen auf Ihrem Dev Kit mit dieser REST-API hinzufügen, entfernen oder aktualisieren.

## <a name="get-existing-credentials"></a>Abrufen vorhandener Anmeldeinformationen

**Anforderung**

Sie können eine Liste der gespeicherten Freigaben sowie den Benutzernamen des Benutzers abrufen, die über Anmeldeinformationen für diese Netzwerkfreigabe verfügt.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/networkcredential
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Ein JSON-Array im folgenden Format:
* Anmeldeinformationen
  * NetworkPath: Der Pfad zur Netzwerkfreigabe.
  * Benutzername: Der Benutzername mit gespeicherten Anmeldeinformationen.

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Möglich
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="add-or-update-stored-credentials-for-a-user"></a>Hinzufügen oder Aktualisieren von gespeicherten Anmeldeinformationen für einen Benutzer

**Anforderung**

Methode      | Anforderungs-URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | Beschreibung     | 
| ------------------ |-----------------|
| NetworkPath        | Der Netzwerkpfad zur Freigabe, zu der Sie Anmeldeinformationen für den Zugriff hinzufügen. |
<br>

**Anforderungsheader**

- Keine

**Anforderungstext**

- Die folgenden JSON-Elemente:
* NetworkPath: Der Pfad zur Netzwerkfreigabe.
* Benutzername: der Benutzername, unter dem die Anmeldeinformationen gespeichert werden sollen.
* Passwort: Das neue oder aktualisierte Kennwort für diesen Benutzer.

**Antwort**   

- Keine  

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Möglich
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="remove-stored-credentials-for-a-share"></a>Entfernen Sie gespeicherte Anmeldeinformationen für eine Freigabe.

**Anforderung**

Methode      | Anforderungs-URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | Beschreibung     | 
| ------------------ |-----------------|
| NetworkPath        | Der Netzwerkpfad zur Freigabe, aus der Sie die gespeicherten Anmeldeinformationen entfernen. |
<br>

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Keine 

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Die Anforderung der Anmeldeinformationen war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Gerätefamilien verfügbar**

* Windows Xbox


