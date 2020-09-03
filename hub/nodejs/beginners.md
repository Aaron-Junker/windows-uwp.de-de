---
title: Erste Schritte bei der Verwendung von Node.js unter Windows für Anfänger
description: Eine Anleitung für Anfänger mit den ersten Schritten der Node.js-Entwicklung unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, Erlernen von NodeJS, Node unter Windows, Node unter Windows für Anfänger, Entwickeln mit Node unter Windows, Entwickler mit Node.js unter Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 5737316ae2de0520e5443f69cefaec25679a228f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166634"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Erste Schritte bei der Verwendung von Node.js unter Windows für Anfänger

Wenn du noch nicht mit der Verwendung von Node.js vertraut bist, findest du in dieser Anleitung einige Grundlagen.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass du bereits die Schritte zum [Einrichten der Node.js-Entwicklungsumgebung nativ unter Windows](./setup-on-windows.md) abgeschlossen hast. Dazu gehören:

- Installieren eines Node.js-Versions-Managers
- Installieren von Visual Studio Code

Die direkte Installation von Node.js unter Windows stellt die einfachste Möglichkeit dar, mit minimalen Einrichtungsaufwand mit der Ausführung grundlegender Node.js-Vorgänge zu beginnen.

Wenn du bereit bist, Node.js zum Entwickeln von Anwendungen für die Produktion zu verwenden (in der Regel mit Bereitstellung auf einem Linux-Server), empfiehlt es sich, eine [Node.js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md) einzurichten. Obwohl es möglich ist, Web-Apps auf Windows-Servern bereitzustellen, wird viel häufiger ein [Linux-Server zum Hosten von Node.js-Apps](https://azure.microsoft.com/develop/nodejs/) verwendet.

## <a name="types-of-nodejs-applications"></a>Typen von Node.js-Anwendungen

Node.js ist eine JavaScript-Runtime, die primär zum Erstellen von Webanwendungen verwendet wird. Anders ausgedrückt: Es handelt sich um eine serverseitige Implementierung von JavaScript, mit der das Back-End einer Anwendung geschrieben wird. (Viele Node.js-Frameworks können jedoch auch das Front-End bereitstellen.) Im Folgenden findest du einige Beispiele, was du mit Node.js erstellen kannst.

- **Single-Page-Apps (SPA):** Dies sind Web-Apps, die in einem Browser funktionieren. Du musst die Seite dann nicht jedes Mal neu laden, wenn du neue Daten abrufen möchtest. Einige Beispiele für SPAs sind Apps sozialer Netzwerke, E-Mail- oder Karten-Apps, Onlinetext- oder -zeichnungstools usw.
- **Echtzeit-Apps (RTAs):** Dies sind Web-Apps, die es Benutzern ermöglichen, Informationen zu erhalten, sobald sie von einem Autor veröffentlicht werden. Der Benutzer (oder die Software) muss die Quelle somit nicht regelmäßig auf Aktualisierungen überprüfen. Einige Beispiele für RTAs sind Instant Messaging-Apps oder Chaträume, Online-Multiplayerspiele, die im Browser gespielt werden, Dokumente zur Onlinezusammenarbeit, Communityspeicher, Videokonferenz-Apps usw.
- **Datenstreaming-Apps:** Dies sind Apps (oder Dienste), die Daten/Inhalte beim Eintreffen (oder Erstellen) senden, während die Verbindung geöffnet bleibt, um weiterhin Daten, Inhalte oder Komponenten nach Bedarf herunterzuladen. Einige Beispiele hierfür sind Video- und Audiostreaming-Apps.
- **REST-APIs:** Dies sind Schnittstellen, die Daten für die Web-App einer anderen Person bereitstellen und mit ihr interagieren. Ein Kalender-API-Dienst könnte beispielsweise Datums- und Uhrzeitwerte für einen Konzertveranstaltungsort bereitstellen, der von der lokalen Veranstaltungswebsite einer anderen Person verwendet werden kann.
- **Serverseitig gerenderte Apps (SSRs):** Diese Web-Apps können sowohl auf dem Client (im Browser/am Front-End) als auch auf dem Server (am Back-End) ausgeführt werden. Dadurch können dynamische Seiten (für die HTML generiert wird) beliebige bekannte Inhalte anzeigen und noch nicht bekannte Inhalte sofort anzeigen, wenn sie verfügbar werden. Diese Apps werden häufig als „isomorphe“ oder „universelle“ Anwendungen bezeichnet. SSRs verwenden SPA-Methoden, da sie nicht jedes Mal neu geladen werden müssen, wenn du sie verwendest. SSRs bieten jedoch einige Vorteile, die für dich wichtig sein könnten, z. B. das Verfügbarmachen von Inhalten auf deiner Website in Google-Suchergebnissen und das Bereitstellen eines Vorschaubilds, wenn Links zu deiner App in sozialen Medien wie Twitter oder Facebook geteilt werden. Ein potenzieller Nachteil ist, dass ständig ein Node.js-Server ausgeführt werden muss. Beispiele sind eine App für ein soziales Netzwerk, die Ereignisse unterstützt, die Benutzern in den Suchergebnissen angezeigt werden sollen. Soziale Medien profitieren eher von SSRs, während für eine E-Mail-App auch eine SPA ausreichend ist. Du kannst auch auf dem Server gerenderte Apps ausführen, die keine SPAs sind, z. B. einen WordPress-Blog. Wie du siehst, kann es schnell kompliziert werden, sodass du herausfinden musst, was für dich wichtig ist.
- **Befehlszeilentools:** Diese ermöglichen dir, sich wiederholende Aufgaben zu automatisieren und dein Tool dann über das riesige Node.js-Ökosystem zu verteilen. Ein Beispiel für ein Befehlszeilentool ist cURL – eine Abkürzung für Client-URL –, das zum Herunterladen von Inhalten von einer Internet-URL verwendet wird. cURL wird häufig verwendet, um z. B. Node.js oder in unserem Fall einen Node.js-Versions-Manager zu installieren.
- **Hardwareprogrammierung:** Node.js ist zwar nicht so beliebt wie Web-Apps, aber die Beliebtheit von Node.js steigt gerade bei IoT-Anwendungsfällen, wie z. B. dem Sammeln von Daten von Sensoren, Signalen, Sendern, Motoren und allem, was große Datenmengen generiert. Node.js ermöglicht das Erfassen von Daten, das Analysieren dieser Daten, die Kommunikation zwischen einem Gerät und einem Server und das Ausführen von Maßnahmen auf der Grundlage der Analyse. npm enthält mehr als 80 Pakete für Arduino-Controller, Raspberry Pi, Intel IoT Edison, verschiedene Sensoren und Bluetooth-Geräte.

## <a name="try-using-nodejs-in-vs-code"></a>Testen von Node.js in VS Code

1. Öffne die Befehlszeile (Eingabeaufforderung, PowerShell oder was du bevorzugst), erstelle ein neues Verzeichnis mit `mkdir HelloNode`, und wechsele dann in das Verzeichnis (`cd HelloNode`).

2. Erstelle eine JavaScript-Datei mit dem Namen „app.js“ und der enthaltenen Variable „msg“: `echo var msg > app.js`

3. Öffne das Verzeichnis und die Datei „app.js“ in VS Code: `code .`

4. Füge eine einfache Zeichenfolgenvariable („Hello World“) hinzu, und sende dann den Inhalt der Zeichenfolge an die Konsole, indem du Folgendes in die Datei „app.js“ eingibst:

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Führe folgende Schritte aus, um die Datei „app.js“ mit Node.js auszuführen. Öffne das Terminal in VS Code durch Auswählen von **Anzeigen** > **Terminal** (oder Drücken von STRG+` (Graviszeichen)). Wenn du das Standardterminal ändern möchtest, wähle das Dropdownmenü und dann **Standardshell auswählen** aus.

6. Gib Folgendes im Terminal ein: `node app.js`. Die folgende Ausgabe sollte angezeigt werden: „Hello World“.

> [!NOTE]
> Beachte, dass VS Code beim Eingeben von `console` in der Datei „app.js“ unterstützte Optionen für das [`console`](https://developer.mozilla.org/docs/Web/API/Console)-Objekt anzeigt, die du mithilfe von IntelliSense auswählen kannst. Experimentiere mit IntelliSense anhand von anderen [JavaScript-Objekten](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Probiere auch das neue [Windows-Terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) aus, wenn du mehrere Befehlszeilen (Ubuntu, PowerShell, Windows-Eingabeaufforderung usw.) verwenden oder [das Terminal anpassen](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) möchtest (einschließlich Text, Hintergrundfarben, Tastenkombinationen, mehrere Fensterbereiche usw.).

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Einrichten eines einfachen Web-App-Frameworks mit Express

Express ist ein minimales, flexibles und optimiertes Node.js-Framework, das die Entwicklung einer Web-App vereinfacht, die mehrere Typen von Anforderungen wie GET, PUT, POST und DELETE verarbeiten kann. Express enthält einen Anwendungsgenerator, der automatisch eine Dateiarchitektur für Ihre App erstellt.

So erstellst du ein Projekt mit Express.js

1. Öffne eine Befehlszeile (Eingabeaufforderung, PowerShell oder was du bevorzugst).
2. Erstelle mit `mkdir ExpressProjects` einen neuen Projektordner, und wechsele mit `cd ExpressProjects` in das Verzeichnis.
3. Erstelle mit Express eine HelloWorld-Projektvorlage: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Wir verwenden hier den Befehl `npx`, um das Node-Paket von Express.js auszuführen, ohne es tatsächlich zu installieren (bzw. durch temporäres Installieren, je nach Betrachtungsweise). Wenn du versuchst, den Befehl `express` zu verwenden oder die installierte Version von Express mit `express --version` zu überprüfen, erhältst du als Antwort, dass Express nicht gefunden werden kann. Wenn du Express global installieren möchtest, um es immer wieder verwenden zu können, verwendest du `npm install -g express-generator`. Mithilfe von `npm list` kannst du eine Liste der Pakete anzeigen, die von npm installiert wurden. Sie werden nach der Tiefe (Anzahl der geschachtelten Verzeichnisebenen) aufgelistet. Pakete, die Sie installiert haben, liegen auf Ebene 0. Die Abhängigkeiten dieses Pakets liegen auf Ebene 1, weitere Abhängigkeiten auf Ebene 2 usw. Weitere Informationen findest du unter [Difference between npx and npm?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) (Unterschiede zwischen npx und npm) auf Stack Overflow.

4. Untersuche die in Express enthaltenen Dateien und Ordner, indem du das Projekt in VS Code öffnest: `code .`

   Die von Express generierten Dateien erstellen eine Web-App mit einer Architektur, die zunächst etwas überwältigend wirken kann. Du kannst im Fenster des VS Code-**Explorers** (angezeigt mit STRG+UMSCHALT+E) erkennen, dass die folgenden Dateien und Ordner erstellt wurden:

   - `bin`. Enthält die ausführbare Datei, die Ihre App startet. Startet einen Server (an Port 3000, wenn keine Alternative angegeben wird) und richtet eine grundlegende Fehlerbehandlung ein. 
   - `public`. Enthält alle öffentlich zugänglichen Dateien einschließlich JavaScript-Dateien, CSS-Stylesheets, Schriftartdateien, Bildern und allen anderen Ressourcen, die Benutzer benötigen, wenn sie eine Verbindung mit Ihrer Website herstellen.
   - `routes`. Enthält alle Routenhandler für die Anwendung. Zwei Dateien, `index.js` und `users.js`, werden in diesem Ordner automatisch generiert, um als Beispiele zum Aussondern der Routenkonfiguration Ihrer Anwendung zu dienen.
   - `views`. Enthält die Dateien, die von Ihrer Vorlagen-Engine verwendet werden. Express ist dazu konfiguriert, hier nach einer übereinstimmenden Ansicht zu suchen, wenn die Rendermethode aufgerufen wird. Die Standardvorlagen-Engine ist Jade, aber sie wurde von Pug abgelöst, daher haben wir das `--view`-Flag verwendet, um die Anzeige-Engine (Vorlagen-Engine) zu ändern. Sie können die `--view`-Flagoptionen und andere mittels `express --help` anzeigen.
   - `app.js`. Der Startpunkt der App. Sie lädt alles und beginnt, Benutzeranforderungen zu verarbeiten. Im Grunde hält sie alle verschiedenen Teile wie ein Klebstoff zusammen.
   - `package.json`. Sie enthält Projektbeschreibung, Skripts-Manager und App-Manifest. Ihr Hauptzweck ist, die Abhängigkeiten Ihrer App und ihre jeweiligen Versionen nachzuverfolgen.

5. Du musst nun die von Express verwendeten Abhängigkeiten installieren, um deine Express-App „HelloWorld“ zu erstellen und auszuführen (die Pakete, die für Aufgaben wie das Ausführen des Servers verwendet werden, wie in der Datei `package.json` definiert). Öffne in VS Code ein Terminal, indem du **Anzeigen** > **Terminal** auswählst (oder drücke STRG+` (Graviszeichen)). Achte darauf, dass du dich weiterhin im Projektverzeichnis „HelloWorld“ befindest. Installiere die Express-Paketabhängigkeiten mit:

```bash
npm install
```

6. An diesem Punkt haben Sie das Framework für eine mehrseitige Web-App mit Zugriff auf eine Vielzahl von APIs und nützliche HTTP-Methoden und Middleware eingerichtet, was das Erstellen einer stabilen API erleichtert. Starte die Express-App auf einem virtuellen Server, indem du Folgendes eingibst:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> Mit dem Teil `DEBUG=myapp:*` im obigen Befehl teilst du Node.js mit, dass du die Protokollierung zu Debuggingzwecken aktivieren möchtest. Denke daran, „myapp“ durch den Namen deiner App zu ersetzen. Du findest den App-Namen in der Datei `package.json` unter der name-Eigenschaft. Mit `npx cross-env` wird die Umgebungsvariable `DEBUG` in einem beliebigen Terminal festgelegt, du kannst sie auch über die jeweilige Vorgehensweise deines Terminals festlegen. Der Befehl `npm start` teilt npm mit, die Skripts in deiner Datei `package.json` auszuführen.

7. Du kannst jetzt die ausgeführte App anzeigen, indem du einen Webbrowser öffnest und zu **localhost:3000** navigierst.

   ![Screenshot der in einem Browser ausgeführten Express-App](../images/express-app.png)

8. Nun, da deine Express-App „HelloWorld“ lokal in einem Browser ausgeführt wird, solltest du versuchen, eine Änderung vorzunehmen. Öffne dazu den Ordner „views“ im Projektverzeichnis, und wähle die Datei „index.pug“ aus. Ändere nach dem Öffnen `h1= title` in `h1= "Hello World!"`, und wähle dann **Speichern** (STRG+S) aus. Zeige die Änderung an, indem du die URL **localhost:3000** in deinem Webbrowser aktualisierst.

9. Gib in deinem Terminal Folgendes ein, um die Ausführung der Express-App zu beenden: **STRG+C**

## <a name="try-using-a-nodejs-module"></a>Ausprobieren eines Node.js-Moduls

Node.js verfügt über viele Tools zur Entwicklung serverseitiger Web-Apps – einige sind integriert und viele weitere über npm verfügbar. Diese Module können bei zahlreichen Aufgaben hilfreich sein:

|Tool               |Verwendung                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Bildbearbeitung, einschließlich Bearbeitung, Größenänderung, Komprimierung usw., direkt in Ihrem JavaScript-Code |
|PDFKit             |PDF-Generierung                                                                                            |
|validator.js       |Zeichenfolgenvalidierung                                                                                         |
|imagemin, UglifyJS2|Minimierung                                                                                              |
|spritesmith        |Sprite-Sheet-Generierung                                                                                   |
|winston            |Protokollierung                                                                                                  |
|commander.js       |Erstellung von Befehlszeilenanwendungen                                                                       |

Wir verwenden das integrierte BS-Modul, um einige Informationen zum Betriebssystem Ihres Computers abzurufen:

1) Öffne an der Befehlszeile die Node.js-Befehlszeilenschnittstelle. Nach der Eingabe von `node` wird die Eingabeaufforderung `>` angezeigt. Sie gibt an, dass du Node.js verwendest.

2) Gib Folgendes ein, um das Betriebssystem zu identifizieren, das du zurzeit verwendest (als Antwort sollte zurückgegeben werden, dass du unter Windows arbeitest): `os.platform()`

3) Gib Folgendes ein, um die CPU-Architektur zu überprüfen: `os.arch()`

4) Gib Folgendes ein, um die auf deinem System verfügbaren CPUs anzuzeigen: `os.cpus()`

5) Verlasse die Node.js-Befehlszeilenschnittstelle, indem du `.exit` eingibst oder zweimal STRG+C drückst.

   > [!TIP]
   > Du kannst das Betriebssystemmodul von Node.js z. B. verwenden, um die Plattform zu überprüfen und eine plattformspezifische Variable zurückzugeben: „Win32/.bat“ für die Windows-Entwicklung, „darwin/.sh“ unter Mac/UNIX, Linux, SunOS usw. (z. B. `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung hast du einige grundlegende Informationen zu den Möglichkeiten von Node.js kennengelernt. Du hast die Verwendung der Node.js-Befehlszeile in VS Code ausprobiert, mit Express.js eine einfache Web-App erstellt und diese lokal in deinem Webbrowser ausgeführt und dann einige der integrierten Node.js-Module ausprobiert. Weitere Informationen zum Installieren und Verwenden einiger gängiger Node.js-Webframeworks findest du in der nächsten Anleitung zu Next.js (ein auf dem Server gerendertes Webframework auf der Grundlage von React), Nuxt.js (ein auf dem Server gerendertes Webframework auf der Grundlage von Vue) und Gatsby (ein statisch gerendertes Webframework auf der Grundlage von React). Du kannst auch direkt mit der Verwendung von MongoDB- oder PostgreSQL-Datenbanken oder Docker-Containern fortfahren.

- [Erste Schritte mit Node.js-Webframeworks unter Windows](./web-frameworks.md)
- [Erste Schritte beim Verbinden von Node.js-Apps mit einer Datenbank](/windows/wsl/tutorials/wsl-database)
- [Erste Schritte bei der Verwendung von Docker-Containern mit Node.js](./containers.md)