---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: Datenzugriff
description: In diesem Abschnitt wird das Speichern von Daten auf dem Gerät in einer privaten Datenbank und die Verwendung der objektrelationalen Zuordnung in UWP-Apps (Universelle Windows-Plattform) erläutert.
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, Daten, Datenbank, relational, Tabellen, SQLite
ms.localizationpriority: medium
ms.openlocfilehash: d324c5b17e167b841761b92507eec2134e4b7009
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173644"
---
# <a name="data-access"></a>Datenzugriff

Mithilfe einer SQLite-Datenbank können Sie Daten auf dem Gerät des Benutzers speichern. Sie können Ihre App auch direkt mit einer SQL Server-Datenbank verbinden, ohne eine Dienstebene verwenden zu müssen.

| Thema | BESCHREIBUNG|
|-------|------------|
| [Verwenden einer SQLite-Datenbank in einer UWP-App](sqlite-databases.md) | Hier wird gezeigt, wie Sie SQLite verwenden, um Daten in einer einfachen Datenbank auf dem Gerät des Benutzers zu speichern und abzurufen. SQLite ist ein eingebettetes Datenbankmodul ohne Server. |
| [Verwenden einer SQL Server-Datenbank in einer UWP-App](sql-server-databases.md) | Hier wird erläutert, wie Sie eine direkte Verbindung mit einer SQL Server-Datenbank herstellen und dann Daten über Klassen im Namespace [System.Data.SqlClient](/dotnet/api/system.data.sqlclient) speichern und abrufen. Es ist keine Dienstebene erforderlich. |

## <a name="related-topics"></a>Zugehörige Themen

* [Beispieldatenbank für Kundenbestellung](https://github.com/Microsoft/Windows-appsample-customers-orders-database)