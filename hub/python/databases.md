---
title: Einstieg in die Verwendung von Python unter Windows mit einer Datenbank
description: Eine Anleitung für den Einstieg in die Verwendung von PostgreSQL oder MongoDB mit Python unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, python unter Windows, Installieren von PostgreSQL unter Windows, Installieren von MongoDB unter Windows, verwenden von PostgreSQL mit Python, verwenden von MongoDB mit Python, PostgreSQL auf WSL, MongoDB auf WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517776"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Einstieg in die Verwendung von PostgreSQL oder MongoDB mit Python unter Windows

Diese Schritt-für-Schritt-Anleitung hilft Ihnen beim Einstieg in die Verbindung Ihrer python-App mit einer Datenbank. Wir haben uns entschieden, sich auf zwei beliebte Optionen zu konzentrieren: PostgreSQL und MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Unterschiede zwischen MongoDB und PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> Möglicherweise möchten Sie auch die Integration des Frameworks und der Tools, die Sie verwenden, mit einem bestimmten Datenbanksystem in Erwägung gezogen. Das [Django-Webframework](./web-frameworks.md#hello-world-tutorial-for-django) scheint in PostgreSQL besser integriert zu sein (siehe [Django Docs](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) and [psycopg2](https://github.com/psycopg/psycopg2)). Das [Flask-Webframework](./web-frameworks.md#hello-world-tutorial-for-flask) scheint in MongoDB besser integriert zu sein (siehe [mongoengine](https://github.com/MongoEngine/flask-mongoengine) und [pymongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Installieren von PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code-Unterstützung für PostgreSQL

VS Code die Arbeit mit PostgreSQL-Datenbanken über die [PostgreSQL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)unterstützt, können Sie PostgreSQL-Datenbanken in vs Code erstellen, verbinden, verwalten und Abfragen.

## <a name="install-mongodb"></a>Installieren von MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code-Unterstützung für MongoDB

VS Code die Arbeit mit MongoDB-Datenbanken über die [Azure Cosmos DB-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)unterstützt, können Sie MongoDB-Datenbanken in vs Code erstellen, verbinden, verwalten und Abfragen.

Weitere Informationen finden Sie in der vs Code docs: [Arbeiten mit MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Einrichten von Profil Aliasen

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
