---
title: URIs für Gamerpic
assetID: 811ab696-c433-aa54-90d8-66614ad09901
permalink: en-us/docs/xboxlive/rest/atoc-reference-gamerpic.html
description: " URIs für Gamerpic"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1a8ba79784ed73ae62e7fe8d65c626c3ebc6003a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618385"
---
# <a name="gamerpic-uris"></a>URIs für Gamerpic
 
Dieser Abschnitt enthält Details zu den Gamerpic Universal Resource Identifier (URI)-Adressen und zugehörigen Hypertext Transport Protocol (HTTP)-Methoden von Xbox Live Services für *Gamerpics*.
 
Die Domäne für diese URIs ist `gamerpics.xboxlive.com`.
 
Die Gamerpic Service wurde entwickelt, Benutzern mehr Optionen zur Personalisierung, gewähren, indem Sie Ihr einem Titel gewähren, die Möglichkeit, die der Benutzer eine Gamerpic ihre Spiele Zeichens generieren kann (ein Spiel Zeichen in diesem Szenario bezieht sich auf ein in-Game-Nutznießer ausgerichtete nachhaltige; es könnte eine Person sein ein Auto, ein Raumschiff oder einer anderen Entität, die die Benutzersteuerelemente im Titel).
 
Die grundlegende Vorgehensweise zum Generieren von einem Titel Gamerpic lautet wie folgt aus:
 
   * Der Titel bietet den Benutzer die Möglichkeit zum Erstellen eines Images von ihrer in-Game-Zeichen. 
     * Wenn dies nicht der Fall ist, der Titel kann dann Nachrichten den Benutzer, dass sie nicht über die entsprechende Berechtigung verfügen.
     * Wenn der Benutzer die Berechtigung verfügt, kann der Benutzer weiterhin ihre Gamerpic Zeichen zu erstellen.
  
   * Der Benutzer erstellt, das Bild und der Titel der 1080 x 1080 PNG-Datei an den Gamerpic-Dienst sendet.
   * Der Dienst speichert das Bild, und legt das Bild als neue Gamerpic des Benutzers.
   * Alle Funktionen, die für den Benutzer Gamerpic aufrufen, erhalten das aktualisierte Image.
  
Die Möglichkeit, einen Titel Gamerpic festzulegen, wird durch eine Erzwingung nur Berechtigungen (211) gesteuert. Wenn die Erzwingung der Berechtigungen widerruft, der Benutzer wird verhindert, speichern einen Titel Gamerpic und der Dienst gibt 403 zurück. Titel sollte CheckPrivilege, um sicherzustellen, dass Benutzer zulässig sind, Freigeben von Inhalten (Priv 211) aufrufen.
 
Um diesen Dienst nutzen zu können, muss dem Titel des gegenwärtig in der Whitelist enthalten sein. Um eine Genehmigung anfordern, e-Mail- `slsgamerpics@microsoft.com`.
 
<a id="ID4EGC"></a>

 
## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[/users/me/gamerpic](uri-usersmegamerpic.md)

&nbsp;&nbsp;Greift auf eine Gamerpic 1080 x 1080.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Universal Resource Identifier (URI)-Referenz](../atoc-xboxlivews-reference-uris.md)

   