---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314884"
---
So installieren Sie MongoDB:

1. Öffnen Sie das WSL-Terminal (d.h. Ubuntu 18,04).
2. Aktualisieren Sie Ihre Ubuntu-Pakete: `sudo apt update`
3. Nachdem die Pakete aktualisiert wurden, installieren Sie MongoDB mit: `sudo apt-get install mongodb`
4. Bestätigen Sie die Installation, und erhalten Sie die Versionsnummer: `mongod --version`

Es gibt drei Befehle, die Sie nach der Installation von MongoDB wissen müssen:

1. `sudo service mongodb status` zum Überprüfen des Status der Datenbank.
2. `sudo service mongodb start`, um mit der Ausführung der Datenbank zu beginnen.
3. `sudo service mongodb stop`, um die Ausführung der Datenbank zu verhindern.

> [!NOTE]
> Möglicherweise wird der Befehl "`sudo systemctl status mongodb`" in Tutorials oder Artikeln angezeigt. In WSL ist `systemd` (ein Dienst Verwaltungssystem unter Linux) nicht enthalten. Stattdessen verwendet Sie sysvinit, um Dienste auf Ihrem Computer zu starten. Sie sollten keinen Unterschied bemerken, aber wenn ein Tutorial die Verwendung von `sudo systemctl` empfiehlt, verwenden Sie stattdessen: `sudo /etc/init.d/`. Beispielsweise wäre `sudo systemctl status mongodb` für WSL `sudo /etc/inid.d/mongodb status`... oder Sie können auch `sudo service mongodb status` verwenden.

### <a name="run-your-mongo-database-in-a-local-server"></a>Ausführen der Mongo-Datenbank auf einem lokalen Server

1. Überprüfen Sie den Status der Datenbank: `sudo service mongodb status` sollte eine [FAIL]-Antwort angezeigt werden, es sei denn, Sie haben die Datenbank bereits gestartet.

2. Starten Sie die Datenbank: `sudo service mongodb start` sollte nun eine [OK]-Antwort angezeigt werden.

3. Stellen Sie sicher, dass Sie eine Verbindung mit dem Datenbankserver herstellen und einen Diagnose Befehl ausführen `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` werden die aktuelle Datenbankversion, die Server Adresse und der Port sowie die Ausgabe des Status-Befehls ausgegeben. Der Wert `1` für das Feld "OK" in der Antwort gibt an, dass der Server funktioniert.

4. Geben Sie Folgendes ein, um die Ausführung des MongoDB-Dienstanbieter anzuhalten: `sudo service mongodb stop`

> [!NOTE]
> MongoDB verfügt über mehrere Standardparameter, einschließlich der Speicherung von Daten in/Data/DB und der Ausführung auf Port 27017. Außerdem ist `mongod` der Daemon (Host Prozess für die Datenbank), und `mongo` ist die Befehlszeilenshell, die eine Verbindung mit einer bestimmten Instanz von `mongod` herstellt.
