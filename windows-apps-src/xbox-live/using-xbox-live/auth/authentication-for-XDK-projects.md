---
title: Authentifizierung für xdk-Version-Projekte
description: Erfahren Sie, wie Xbox Live-Benutzer in einen Titel ein Xbox Development Kit (xdk-Version) anmelden.
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 597b3becfa2083955d8bd4e0adc91e4ae9b827a1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613495"
---
# <a name="authentication-for-xdk-projects"></a>Authentifizierung für xdk-Version-Projekte

Um Xbox Live-Funktionen in Spielen nutzen zu können, muss ein Benutzer ein Xbox Live-Profil selbst identifizieren, in der Xbox Live-Community erstellen.  Xbox Live Services mitverfolgen Spiel bezogene Aktivitäten, die mithilfe dieses Profils Xbox Live, z. B. Gamertag des Benutzers und der Spieler Bild, die die Freunde des Benutzers Spiele sind, was den Benutzer Spiele gespielt hat, welche Erfolge, die der Benutzer entsperrt hat, in denen der Benutzer auf steht die die Bestenliste für ein bestimmtes Spiels, usw.

Im Allgemeinen verwenden Sie die Xbox Live-APIs mit folgenden Schritten:
1. Identifizieren Sie den interagierenden Benutzer
2. Erstellen Sie einen Xbox Live-Kontext basierend auf dem Benutzer
3. Verwenden Sie den Xbox Live-Kontext, den Zugriff auf Xbox Live-Dienste
4. Wenn das Spiel beendet wird oder der Benutzer meldet-Ausgang, das XboxLiveContext-Objekt frei, auf null festgelegt wird

### <a name="creating-an-xboxliveuser-object"></a>Erstellen eines Objekts XboxLiveUser
Die meisten Aktivitäten Xbox Live beziehen sich auf der Xbox Live-Benutzer.  Als Entwickler von Spielen müssen Sie zuerst ein XboxLiveUser-Objekt zur Darstellung des lokalen Benutzers zu erstellen.

C++:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT:
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
