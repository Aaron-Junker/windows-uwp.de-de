---
title: Einstieg in das Verbinden von Node. js-apps mit einer Datenbank
description: Beginnen Sie mit dem Verbinden von Node. js-apps mit einer Datenbank unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, Node on Windows, Node on WSL, Node on Linux on Windows, install Node on Windows, NodeJS with vs Code, Develop with Node on Windows, Develop with NodeJS on Windows, install Node on WSL, NodeJS on Windows Subsystem für Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315114"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>Einstieg in das Verbinden von Node. js-apps mit einer Datenbank

Node. js-Anwendungen müssen häufig Daten persistent speichern, was durch Dateien, lokalen Speicher, Clouddienste oder Datenbanken erfolgen kann. Diese Schritt-für-Schritt-Anleitung hilft Ihnen beim Einstieg in die Verbindung Ihrer Node. js-App mit einer-Datenbank. Wir haben uns entschieden, sich auf zwei beliebte Optionen zu konzentrieren: MongoDB und PostgreSQL.

## <a name="prerequisites"></a>Erforderliche Komponenten

In dieser Anleitung wird davon ausgegangen, dass Sie bereits die Schritte zum [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md)abgeschlossen haben, einschließlich:

- Installieren Sie Windows 10 Insider Preview Build 18932 oder höher.
- Aktivieren Sie das WSL 2-Feature unter Windows.
- Installieren Sie eine Linux-Distribution (Ubuntu 18,04 für unsere Beispiele). Sie können Folgendes überprüfen: `wsl lsb_release -a`
- Stellen Sie sicher, dass die Ubuntu 18,04-Distribution im WSL 2-Modus ausgeführt wird. (WSL kann Verteilungen im v1-oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`
- Legen Sie mithilfe von PowerShell Ubuntu 18,04 als Standardverteilung fest, mit: `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>Unterschiede zwischen MongoDB und PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Installieren von MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code-Unterstützung für MongoDB

VS Code die Arbeit mit MongoDB-Datenbanken über die [Azure Cosmos DB-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)unterstützt, können Sie MongoDB-Datenbanken in vs Code erstellen, verwalten und Abfragen.

Weitere Informationen finden Sie in den vs Code docs: [Arbeiten mit MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Weitere Informationen finden Sie in der MongoDB-Dokumentation:

- [Einführung in die Verwendung von MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Benutzer erstellen](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Herstellen einer Verbindung mit einer MongoDB-Instanz auf einem Remote Host](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD: Create, Read, Update, DELETE @ no__t-0
- [Referenzdokumente](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Installieren von PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code-Unterstützung für PostgreSQL

VS Code die Arbeit mit PostgreSQL-Datenbanken über die [PostgreSQL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)unterstützt, können Sie PostgreSQL-Datenbanken in vs Code erstellen, verbinden, verwalten und Abfragen.

## <a name="set-up-profile-aliases"></a>Einrichten von Profil Aliasen

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
