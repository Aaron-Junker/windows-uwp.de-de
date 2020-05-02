---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314894"
---
So installieren Sie PostgreSQL:

1. Öffnen Sie das WSL-Terminal (d. h. Ubuntu 18.04).
2. Aktualisieren Sie Ihre Ubuntu-Pakete: `sudo apt update`.
3. Nachdem die Pakete aktualisiert wurden, installieren Sie PostgreSQL (und das -contrib-Paket mit einigen nützlichen Dienstprogrammen) mit: `sudo apt install postgresql postgresql-contrib`.
4. Bestätigen Sie die Installation, und rufen Sie die Versionsnummer ab: `psql --version`.

Es gibt drei Befehle, die Sie nach der Installation von PostgreSQL kennen müssen:

1. `sudo service postgresql status` zum Überprüfen des Status Ihrer Datenbank.
2. `sudo service postgresql start`, um die Ausführung der Datenbank zu starten.
3. `sudo service postgresql stop`, um die Ausführung der Datenbank zu stoppen.

### <a name="postgresql-user-setup"></a>PostgreSQL-Benutzereinrichtung

Der standardmäßige Administratorbenutzer (`postgres`) benötigt ein zugewiesenes Kennwort, um eine Verbindung mit einer Datenbank herzustellen. So legen Sie ein Kennwort fest:

1. Geben Sie den Befehl `sudo passwd postgres` ein.
2. Sie erhalten eine Aufforderung, Ihr neues Kennwort einzugeben.
3. Schließen Sie das Terminal, und öffnen Sie es erneut.

### <a name="run-postgresql-with-psql-shell"></a>Ausführen von PostgreSQL mit der psql-Shell

[psql](https://www.postgresql.org/docs/10/app-psql.html) ist ein terminalbasiertes Front-End zu PostgreSQL. Es ermöglicht Ihnen, Abfragen interaktiv einzugeben, sie in PostgreSQL auszugeben und die Abfrageergebnisse anzuzeigen. Alternativ können Eingaben aus einer Datei erfolgen. Außerdem bietet es eine Reihe von Metabefehlen und verschiedene shellähnliche Features, um das Schreiben von Skripts und das Automatisieren einer Vielzahl von Aufgaben zu vereinfachen.

So starten Sie die psql-Shell:

1. Starten Ihres Postgres-Diensts: `sudo service postgresql start`.
2. Verbinden Sie sich mit dem Postgres-Dienst, und öffnen Sie die psql-Shell: `sudo -u postgres psql`.

Nachdem Sie die psql-Shell erfolgreich eingegeben haben, sehen Sie, dass die Befehlszeile sich wie folgt ändert: `postgres=#`.

> [!NOTE]
> Alternativ dazu können Sie die psql-Shell öffnen, indem Sie mit `su - postgres` zum Postgres-Benutzer wechseln und dann den folgenden Befehl eingeben: `psql`.

Geben Sie zum Beenden von „postgres=#“ `\q` ein, oder verwenden Sie diese Tastenkombination: Strg+D

Um anzuzeigen, welche Benutzerkonten in Ihrer PostgreSQL-Installation erstellt wurden, geben Sie `psql -c "\du"` in Ihr WSL-Terminal ein, oder einfach `\du`, wenn die psql-Shell geöffnet ist. Mit diesem Befehl werden die folgenden Spalten angezeigt: Kontobenutzername, Liste der Rollenattribute und Mitglied der Rollengruppe(n). Um zur Befehlszeile zurückzukehren, geben Sie `q` ein.
