---
title: PermissionId-Enumeration
assetID: 3e7ee317-4946-1105-ecd7-1e26346deccb
permalink: en-us/docs/xboxlive/rest/privacy-enum-permissionid.html
description: " PermissionId-Enumeration"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: aff463e2268af972536984a00e29348bf0a6859e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594685"
---
# <a name="permissionid-enumeration"></a>PermissionId-Enumeration
Erläutert die PermissionId-Enumeration.
Berechtigung-IDs können mit der Berechtigung Überprüfung URLs verwendet werden:

   * [GET (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidateget.md)
   * [POST (/users/{requestorId}/permission/validate)](../uri/privacy/uri-privacyusersrequestoridpermissionvalidatepost.md)

Diese IDs enthalten direkte Überprüfungen im Hinblick auf eine bestimmte Einstellung für einen Benutzer, z. B. eine Einstellung für die einzelnen Datenschutz ein Ziel oder eine einzelne Berechtigung Actor Überprüfung. Darüber hinaus stehen Berechtigung-IDs, die verwendet werden können, mit der API-Berechtigung und Überprüfungen im Hinblick auf mehrere Einstellungen für bestimmte Benutzeraktionen zu integrieren.

<a id="ID4EIB"></a>


## <a name="permissions"></a>Berechtigungen

Dies sind Werte, die ein Aufrufer verwenden können, um zu überprüfen, ob eine bestimmte Aktion ausgeführt werden kann. Im Gegensatz zu den oben genannten Einstellungen diese kapseln Richtlinien, die vom Dienst definiert und können nicht direkt von Benutzern geändert werden, obwohl in den meisten Fällen die Richtlinien auf eine oder mehrere Einstellungen aufbauen, deren Werte von Benutzern definiert sind. Hierbei handelt es sich um in der Regel zusammengesetzten Überprüfungen im Hinblick auf mehr als eine Einstellung, die oben definierten. Beispiel: Die <b>ViewProfile</b> Berechtigung ist eine Überprüfung des Zielbereichs <b>ShareProfile</b> Einstellung für Datenschutz und der anfordernden Person <b>AllowProfileViewing</b> Berechtigungen.

Im Allgemeinen empfiehlt es sich, dass Aufrufer eine Berechtigung-ID für Aktionen, die anfordern statt direkt datenschutzeinstellungen und Berechtigungen überprüft werden soll, müssen. Dadurch wird die Datenschutzrichtlinien konsistent über den Dienst geändert werden, wie neue Überprüfungen integriert werden.

| Name der Berechtigung| Beschreibung|
| --- | --- |
| CommunicateUsingText| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer eine Nachricht mit Textinhalt an der Zielbenutzer senden können|
| CommunicateUsingVideo| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer der Zielbenutzer mit Video kommunizieren können|
| CommunicateUsingVoice| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer kommunizieren kann, verwenden die Stimme mit dem Zielbenutzer|
| ViewTargetProfile| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer das Profil des Zielbenutzers anzeigen kann|
| ViewTargetGameHistory| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer den Spielen Verlauf des Zielbenutzers anzeigen kann|
| ViewTargetVideoHistory| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer die ausführlichen video eines Verlaufs der Zielbenutzer anzeigen kann|
| ViewTargetMusicHistory| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer den Verlauf der ausführliche Musik lauschenden des Zielbenutzers anzeigen kann|
| ViewTargetExerciseInfo| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer die Übung Informationen des Zielbenutzers anzeigen kann|
| ViewTargetPresence| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer den Onlinestatus des Zielbenutzers anzeigen kann|
| ViewTargetVideoStatus| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer die Details zu den Zielen-video-Status (Erweiterte Onlinepräsenz) anzeigen kann|
| ViewTargetMusicStatus| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer die Details zu den Zielen Musik-Status (Erweiterte Onlinepräsenz) anzeigen kann|
| ViewTargetUserCreatedContent| Überprüfen Sie, und zwar unabhängig davon, ob der Benutzer den Inhalt benutzererstellte anderer Benutzer anzeigen kann|
