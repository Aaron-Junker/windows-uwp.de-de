---
title: API-Referenz für Netzwerk Anmelde Informationen des Geräte Portals
description: Erfahren Sie, wie Sie Programm gesteuert Netzwerk Anmelde Informationen hinzufügen, entfernen oder aktualisieren.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7ba9e25031590da02881276ff9a10a9f952c78ec
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254215"
---
# <a name="network-credentials-api-reference"></a>API-Referenz für Netzwerk Anmelde Informationen

Sie können gespeicherte Netzwerk Anmelde Informationen in Ihrem devkit mithilfe dieser Rest-API hinzufügen, entfernen oder aktualisieren.

## <a name="get-existing-credentials"></a>Vorhandene Anmelde Informationen erhalten

**Anforderung**

Sie können eine Liste der gespeicherten Freigaben zusammen mit dem Benutzernamen des Benutzers, der über Anmelde Informationen für diese Netzwerkfreigabe verfügt, erhalten.

| Methode | Anforderungs-URI |
|--------|-------------|
| GET | /ext/networkcredential |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

JSON-Array im folgenden Format:

* Anmeldeinformationen
  * NetworkPath: der Pfad zur Netzwerkfreigabe.
  * Username: der Benutzername mit gespeicherten Anmelde Informationen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 200 | Erfolg |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

## <a name="add-or-update-stored-credentials-for-a-user"></a>Gespeicherte Anmelde Informationen für einen Benutzer hinzufügen oder aktualisieren

**Anforderung**

| Methode | Anforderungs-URI |
|--------|-------------|
| POST | /ext/networkcredential |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | Beschreibung     |
| ------------------ |-----------------|
| NetworkPath        | Der Netzwerkpfad der Freigabe, für die Sie Anmelde Informationen hinzufügen. |

**Anforderungsheader**

- Keine

**Anforderungstext**

Die folgenden JSON-Elemente:
* NetworkPath: der Pfad zur Netzwerkfreigabe.
* Username: der Benutzername, unter dem die Anmelde Informationen gespeichert werden sollen.
* Password: das neue oder aktualisierte Kennwort für diesen Benutzer.

**Antwort**   

- Keine  

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 204 | Erfolg |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

## <a name="remove-stored-credentials-for-a-share"></a>Entfernen Sie die gespeicherten Anmelde Informationen für eine Freigabe.

**Anforderung**

| Methode | Anforderungs-URI |
|--------|-------------|
| DELETE | /ext/networkcredential |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | Beschreibung     |
| ------------------ |-----------------|
| NetworkPath        | Der Netzwerkpfad der Freigabe, aus der Sie gespeicherte Anmelde Informationen entfernen. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | BESCHREIBUNG |
|------------------|-------------|
| 204 | Die Anforderung an die Anmelde Informationen war erfolgreich. |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Xbox