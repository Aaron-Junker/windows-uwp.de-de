---
title: Erste Schritte beim Verbinden von Node.js-Apps mit einer Datenbank
description: Erste Schritte beim Verbinden von Node.js-Apps mit einer Datenbank unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, Erlernen von Node.js, Node unter Windows, Node im WSL, Node unter Linux unter Windows, Installieren von Node unter Windows, NodeJS mit VS Code, Entwickeln mit Node unter Windows, Entwickeln mit NodeJS unter Windows, Installieren von Node im WSL, NodeJS im Windows-Subsystem für Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517816"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Erste Schritte bei der Verwendung von MongoDB oder PostgreSQL mit Node.js unter Windows

Node.js-Anwendungen müssen Daten oftmals dauerhaft speichern, was mithilfe von Dateien, lokalem Speicher, Clouddiensten oder Datenbanken erfolgen kann. Diese ausführliche Anleitung hilft Ihnen bei den ersten Schritten beim Verbinden Ihrer Node.js-App mit einer Datenbank. Dabei werden insbesondere zwei beliebte Optionen behandelt: MongoDB und PostgreSQL.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass du die Schritte zum [Einrichten der Node.js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md) bereits abgeschlossen hast. Dazu gehören:

- Installieren von Windows 10 Insider Preview Build 18932 oder höher
- Aktivieren des WSL 2-Features unter Windows
- Installieren einer Linux-Distribution (Ubuntu 18.04 für unsere Beispiele) Dies können Sie mit `wsl lsb_release -a` überprüfen.
- Achten Sie darauf, dass die Distribution Ubuntu 18.04 im WSL 2-Modus ausgeführt wird. (WSL kann Distributionen im V1- oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`.
- Legen Sie in PowerShell mit `wsl -s ubuntu 18.04` Ubuntu 18.04 als Standarddistribution fest.

## <a name="differences-between-mongodb-and-postgresql"></a>Unterschiede zwischen MongoDB und PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Installieren von MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code-Unterstützung für MongoDB

VS Code unterstützt die Arbeit mit MongoDB-Datenbanken über die [Azure Cosmos DB-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb). Sie können MongoDB-Datenbanken in VS Code erstellen, verwalten und abfragen.

Weitere Informationen finden Sie in der Dokumentation zu VS Code: [Arbeiten mit MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Weitere Informationen finden Sie in der MongoDB-Dokumentation:

- [Einführung in die Verwendung von MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Erstellen von Benutzern](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Herstellen einer Verbindung mit einer MongoDB-Instanz auf einem Remotehost](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: Erstellen, lesen, aktualisieren und löschen](https://docs.mongodb.com/manual/crud/)
- [Referenzdokumentation](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Installieren von PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code-Unterstützung für PostgreSQL

VS Code unterstützt die Arbeit mit PostgreSQL-Datenbanken über die [PostgreSQL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql). Sie können PostgreSQL-Datenbanken in VS Code erstellen, verbinden, verwalten und abfragen.

## <a name="set-up-profile-aliases"></a>Einrichten von Profilaliasen

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
