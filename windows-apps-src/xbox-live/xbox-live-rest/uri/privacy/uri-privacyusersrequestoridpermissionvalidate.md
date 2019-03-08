---
title: /users/{requestorId}/permission/validate
assetID: 400a9721-bf43-76df-4cd1-9f2ae6ca5035
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidate.html
description: " /users/{requestorId}/permission/validate"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 4a062fd417bae37fd66c944e0e534ef7a50de5fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599365"
---
# <a name="usersrequestoridpermissionvalidate"></a>/users/{requestorId}/permission/validate
 
  * [URI-Parameter](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="uri-parameters"></a>URI-Parameter
 
| Parameter| Typ| Beschreibung| 
| --- | --- | --- | 
| requestorId| string| Erforderlich. Der Bezeichner des Benutzers, die Aktion ausführt. Die möglichen Werte sind <code>xuid({xuid})</code> und <code>me</code>. Dies muss ein Benutzer angemeldet sein. Beispielwert: <code>xuid(0987654321)</code>.| 
  
<a id="ID4ETB"></a>

 
## <a name="valid-methods"></a>Gültigen Methoden

[GET (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidateget.md)

&nbsp;&nbsp;Ruft ab eine Ja / Nein Antwort zu gibt an, ob der Benutzer die angegebene Aktion mit einem Zielbenutzer ausführen darf.

[POST (/users/{requestorId}/permission/validate)](uri-privacyusersrequestoridpermissionvalidatepost.md)

&nbsp;&nbsp;Ruft einen Satz von Ja oder Nein Antworten zu gibt an, ob der Benutzer bestimmte Aktionen mit einem Satz von Zielbenutzern ausführen darf.
 
<a id="ID4EAC"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ECC"></a>

   [Datenschutz-URIs](atoc-reference-privacyv2.md)

 [PermissionId-Enumeration](../../enums/privacy-enum-permissionid.md)

   