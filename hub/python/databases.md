---
title: Erste Schritte bei der Verwendung von Python unter Windows mit einer Datenbank
description: Eine Anleitung für den Einstieg in die Verwendung von PostgreSQL oder MongoDB mit Python unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, Python unter Windows, Installieren von PostgreSQL unter Windows, Installieren von MongoDB unter Windows, Verwenden von PostgreSQL mit Python, Verwenden von MongoDB mit Python, PostgreSQL im WSL, MongoDB im WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517776"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Erste Schritte bei der Verwendung von PostgreSQL oder MongoDB mit Python unter Windows

Diese ausführliche Anleitung hilft Ihnen bei den ersten Schritten beim Verbinden Ihrer Python-App mit einer Datenbank. Dabei werden insbesondere zwei beliebte Optionen behandelt: PostgreSQL und MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Unterschiede zwischen MongoDB und PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> Möglicherweise möchten Sie die Integration des Frameworks und der Tools, die Sie verwenden, mit einem bestimmten Datenbanksystem berücksichtigen. Das [Django-Webframework](./web-frameworks.md#hello-world-tutorial-for-django) scheint besser mit PostgreSQL integriert zu sein. (Weitere Informationen finden Sie in der [Django-Dokumentation](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) und unter [psycopg2](https://github.com/psycopg/psycopg2).) Das [Flask-Webframework](./web-frameworks.md#hello-world-tutorial-for-flask) scheint besser mit MongoDB integriert zu sein. (Weitere Informationen finden Sie unter [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) und [PyMongo](https://github.com/dcrosta/flask-pymongo).)

## <a name="install-postgresql"></a>Installieren von PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code-Unterstützung für PostgreSQL

VS Code unterstützt die Arbeit mit PostgreSQL-Datenbanken über die [PostgreSQL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql). Sie können PostgreSQL-Datenbanken in VS Code erstellen, verbinden, verwalten und abfragen.

## <a name="install-mongodb"></a>Installieren von MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>VS Code-Unterstützung für MongoDB

VS Code unterstützt die Arbeit mit MongoDB-Datenbanken über die [Azure Cosmos DB-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb). Sie können MongoDB-Datenbanken in VS Code erstellen, verbinden, verwalten und abfragen.

Weitere Informationen finden Sie in der Dokumentation zu VS Code: [Arbeiten mit MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Einrichten von Profilaliasen

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
