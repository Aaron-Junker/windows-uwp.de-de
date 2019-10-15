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
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314854"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>Einstieg in die Verwendung von Datenbanken mit Python unter Windows

Python-Anwendungen müssen häufig Daten persistent speichern, was durch Dateien, lokalen Speicher, Clouddienste oder Datenbanken erfolgen kann. Diese Schritt-für-Schritt-Anleitung hilft Ihnen beim Einstieg in die Verbindung Ihrer python-App mit einer Datenbank. Wir haben uns entschieden, sich auf zwei beliebte Optionen zu konzentrieren: PostgreSQL und MongoDB.

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

Weitere Informationen finden Sie in den vs Code docs: [Arbeiten mit MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Einrichten von Profil Aliasen

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
