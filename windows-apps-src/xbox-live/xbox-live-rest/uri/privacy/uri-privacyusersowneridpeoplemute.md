---
title: /users/{ownerId}/people/mute
assetID: efb929d8-79a7-83f0-c348-c92ced42bc05
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemute.html
description: " /users/{ownerId}/people/mute"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 86010d11ff0a7ebe188766f51bc3160a4f2befa4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641545"
---
# <a name="usersowneridpeoplemute"></a>/users/{ownerId}/people/mute
Greift auf die stumm schalten Liste für einen Benutzer.

  * [URI-Parameter](#ID4EQ)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| ownerId| string| Erforderlich. Der Bezeichner des Benutzers, dessen Ressource zugegriffen wird. Die möglichen Werte sind "me", <code>xuid({xuid})</code>, oder gt({gamertag}). Der authentifizierte Benutzer muss sein werden. Beispielwerte: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Maximale Größe: keine. |

<a id="ID4ETB"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[GET (/ Users / {"ownerid"} / People/stummschaltung)](uri-privacyusersowneridpeoplemuteget.md)

&nbsp;&nbsp;Ruft die stumm schalten Liste für einen Benutzer ab.

<a id="ID4E4B"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E6B"></a>


##### <a name="parent"></a>Parent

[Datenschutz-URIs](atoc-reference-privacyv2.md)
