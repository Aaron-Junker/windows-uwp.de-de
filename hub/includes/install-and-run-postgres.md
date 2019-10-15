---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314894"
---
So installieren Sie PostgreSQL:

1. Öffnen Sie das WSL-Terminal (d.h. Ubuntu 18,04).
2. Aktualisieren Sie Ihre Ubuntu-Pakete: `sudo apt update`
3. Nachdem die Pakete aktualisiert wurden, installieren Sie PostgreSQL (und das-contrib-Paket mit einigen nützlichen Dienstprogrammen) mit: `sudo apt install postgresql postgresql-contrib`
4. Bestätigen Sie die Installation, und erhalten Sie die Versionsnummer: `psql --version`

Es gibt drei Befehle, die Sie nach der Installation von PostgreSQL wissen müssen:

1. `sudo service postgresql status` zum Überprüfen des Status der Datenbank.
2. `sudo service postgresql start`, um mit der Ausführung der Datenbank zu beginnen.
3. `sudo service postgresql stop`, um die Ausführung der Datenbank zu verhindern.

### <a name="postgresql-user-setup"></a>PostgreSQL-Benutzer Einrichtung

Der Standard Administrator Benutzer (`postgres`) benötigt ein zugewiesenes Kennwort, um eine Verbindung mit einer Datenbank herzustellen. So legen Sie ein Kennwort fest:

1. Geben Sie den folgenden Befehl ein: `sudo passwd postgres`
2. Sie erhalten eine Aufforderung, Ihr neues Kennwort einzugeben.
3. Schließen Sie das Terminal und öffnen Sie es erneut.

### <a name="run-postgresql-with-psql-shell"></a>Ausführen von PostgreSQL mit psql-Shell

[psql](https://www.postgresql.org/docs/10/app-psql.html) ist ein Terminal basiertes Front-End zu PostgreSQL. Sie ermöglicht es Ihnen, Abfragen interaktiv einzugeben, Sie in PostgreSQL auszugeben und die Abfrageergebnisse anzuzeigen. Alternativ können Eingaben aus einer Datei erfolgen. Außerdem bietet es eine Reihe von metabefehlen und verschiedene shellähnliche Features, um das Schreiben von Skripts und das Automatisieren einer Vielzahl von Aufgaben zu vereinfachen.

So starten Sie die psql-Shell:

1. Starten Sie Ihren Postgres-Dienst: `sudo service postgresql start`
2. Stellen Sie eine Verbindung zum Postgres-Dienst her, und öffnen Sie die psql-Shell: `sudo -u postgres psql`

Nachdem Sie die psql-Shell erfolgreich eingegeben haben, sehen Sie, dass die Befehlszeile wie folgt aussieht: `postgres=#`

> [!NOTE]
> Alternativ dazu können Sie die psql-Shell öffnen, indem Sie zum Postgres-Benutzer mit: `su - postgres` wechseln und dann den folgenden Befehl eingeben: `psql`.

Um Postgres = # Enter: `\q` zu beenden, oder verwenden Sie die Tastenkombination: STRG + D

Um anzuzeigen, welche Benutzerkonten in Ihrer PostgreSQL-Installation erstellt wurden, verwenden Sie von Ihrem WSL-Terminal aus: `psql -c "\du"`... oder nur `\du`, wenn die psql-Shell geöffnet ist. Mit diesem Befehl werden die folgenden Spalten angezeigt: Kontobenutzer Name, Liste der Rollen Attribute und Mitglied der Rollen Gruppe (n). Um wieder zur Befehlszeile zu wechseln, geben Sie Folgendes ein: `q`.
