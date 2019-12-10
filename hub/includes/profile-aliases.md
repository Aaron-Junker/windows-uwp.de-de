---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314904"
---
Das Eingeben von `sudo service mongodb start` oder `sudo service postgres start` und `sudo -u postgrest psql` kann mühsam werden.  Sie können jedoch in Betracht ziehen, Aliase in Ihrer Datei `.profile` unter WSL einzurichten, um diese Befehle einfacher verwenden zu können und sie sich besser zu merken. 

So richten Sie einen eigenen benutzerdefinierten Alias oder eine Verknüpfung für die Ausführung dieser Befehle ein

1. Öffnen Sie das WSL-Terminal, und geben Sie `cd ~` ein, um sicherzustellen, dass Sie sich im Stammverzeichnis befinden.
2. Öffnen Sie die Datei `.profile`, in der die Einstellungen für das Terminal gesteuert werden, mit dem Terminal-Text-Editor nano: `sudo nano .profile`.
3. Fügen Sie am Ende der Datei Folgendes hinzu (ohne die `# set PATH`-Einstellungen zu ändern):

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Dadurch können Sie `start-pg` eingeben, um den PostgreSQL-Dienst zu starten, und `run-pg`, um die psql-Shell zu öffnen. Sie können `start-pg` und `run-pg` in beliebige Namen ändern. Achten Sie jedoch darauf, keinen Befehl zu überschreiben, der von Postgres bereits verwendet wird.

4. Nachdem Sie die neuen Aliase hinzugefügt haben, beenden Sie den Text-Editor nano, indem Sie **STRG+X** drücken, `Y` (Ja) auswählen, wenn Sie gefragt werden, ob Sie speichern möchten, und die EINGABETASTE drücken (wobei Sie den Dateinamen `.profile`beibehalten).
5. Schließen Sie das WSL-Terminal, öffnen Sie es erneut, und probieren Sie dann die neuen Aliasbefehle aus.
