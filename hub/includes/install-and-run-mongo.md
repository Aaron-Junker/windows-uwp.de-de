---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314884"
---
So installieren Sie MongoDB

1. Öffnen Sie das WSL-Terminal (d. h. Ubuntu 18.04).
2. Aktualisieren Sie Ihre Ubuntu-Pakete: `sudo apt update`.
3. Nachdem die Pakete aktualisiert wurden, installieren Sie MongoDB wie folgt: `sudo apt-get install mongodb`.
4. Bestätigen Sie die Installation, und rufen Sie die Versionsnummer ab: `mongod --version`.

Nach der Installation von MongoDB müssen Sie drei Befehle kennen:

1. `sudo service mongodb status` zum Überprüfen des Status Ihrer Datenbank.
2. `sudo service mongodb start`, um die Ausführung der Datenbank zu starten.
3. `sudo service mongodb stop`, um die Ausführung der Datenbank zu stoppen.

> [!NOTE]
> Möglicherweise wird in Tutorials oder Artikeln der Befehl `sudo systemctl status mongodb` verwendet. Damit das Paket nicht zu groß wird, ist `systemd` (ein Dienstverwaltungssystem unter Linux) in WSL nicht enthalten. Stattdessen wird SysVinit verwendet, um Dienste auf Ihrem Computer zu starten. Sie sollten keinen Unterschied bemerken, aber wenn in einem Tutorial die Verwendung von `sudo systemctl` empfohlen wird, verwenden Sie stattdessen `sudo /etc/init.d/`. Beispiel: `sudo systemctl status mongodb` entspricht in WSL `sudo /etc/inid.d/mongodb status`. Sie können jedoch auch `sudo service mongodb status` verwenden.

### <a name="run-your-mongo-database-in-a-local-server"></a>Ausführen Ihrer Mongo-Datenbank auf einem lokalen Server

1. Überprüfen des Status Ihrer Datenbank: `sudo service mongodb status` Es sollte die Antwort „Fail“ angezeigt werden, sofern Sie die Datenbank noch nicht gestartet haben.

2. Starten Ihrer Datenbank: `sudo service mongodb start` Nun sollte die Antwort „OK“ angezeigt werden.

3. Überprüfen Sie den Zustand, indem Sie eine Verbindung mit dem Datenbankserver herstellen und einen Diagnosebefehl ausführen: `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` Hierdurch werden die aktuelle Datenbankversion, die Serveradresse und der Port sowie die Ausgabe des Statusbefehls ausgegeben. Der Wert `1` für das Feld „ok“ in der Antwort gibt an, dass der Server funktioniert.

4. Wenn Sie die Ausführung des MongoDB-Diensts unterbinden möchten, geben Sie Folgendes ein: `sudo service mongodb stop`

> [!NOTE]
> MongoDB verfügt über mehrere Standardparameter, darunter die Speicherung von Daten in „/data/db“ und die Ausführung an Port 27017. Außerdem ist `mongod` der Daemon (Hostprozess für die Datenbank), und `mongo` ist die Befehlszeilenshell, die eine Verbindung mit einer bestimmten Instanz von `mongod` herstellt.
