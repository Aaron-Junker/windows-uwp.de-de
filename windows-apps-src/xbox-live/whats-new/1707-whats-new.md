---
title: What's new in Xbox Live-APIs – Juli 2017
description: What's new in Xbox Live-APIs – Juli 2017
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, einen neuen Funktionen, Juli 2017
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618825"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>Neuerungen für die Xbox Live-APIs – Juli 2017

Finden Sie unter den [– Juni 2017 Neuigkeiten](1706-whats-new.md) Artikel Neuheiten in der Version Juni 2017.

Sie können auch überprüfen, die [Xbox Live-API-GitHub-Commit-Verlauf](https://github.com/Microsoft/xbox-live-api/commits/master) alle den codeänderungen den Xbox Live-APIs anzeigen.

## <a name="xbox-live-features"></a>Xbox Live-Funktionen

### <a name="multiplayer-updates"></a>Multiplayer-updates

Abfragen von Aktivität verarbeitet und bei der Suche jetzt enthält die benutzerdefinierte Eigenschaften in der Antwort.

### <a name="tournaments"></a>Turniere

Neue APIs wurden hinzugefügt, um Turniere zu unterstützen. Sie können jetzt die Xbox::services::tournaments::tournament_service-Klasse verwenden, Zugriff auf die Turniere aus dem Titel des.
Diese neue Turnier APIs ermöglichen die folgenden Szenarien:
* Fragen Sie den Dienst, um alle vorhandenen Turniere für den aktuellen Titel finden.
* Abrufen von Details zu ein Turnier aus dem Dienst.
* Fragen Sie den Dienst, um eine Liste der Teams für ein Turnier abzurufen.
* Abrufen von Details zu den Teams für ein Turnier aus dem Dienst.
* Nachverfolgen von Änderungen mithilfe von Echtzeit-Aktivität (RTA) Abonnements Turniere und Teams.
