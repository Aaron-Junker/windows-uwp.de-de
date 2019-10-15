---
title: Einstieg in die Verwendung von nodejs unter Windows für Einsteiger
description: Eine Anleitung für Einsteiger bei den ersten Schritten mit der Node. js-Entwicklung unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, Node on Windows, Node on Windows für Einsteiger, entwickeln mit Node on Windows, Entwickler mit NodeJS unter Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315134"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Einstieg in die Verwendung von Node. js unter Windows für Einsteiger

Wenn Sie mit der Verwendung von Node. js noch nicht vertraut sind, erhalten Sie in diesem Handbuch einen Einstieg in einige Grundlagen.

## <a name="prerequisites"></a>Erforderliche Komponenten

In dieser Anleitung wird davon ausgegangen, dass Sie bereits die Schritte zum [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md)abgeschlossen haben, einschließlich:

- Installieren Sie Windows 10 Insider Preview Build 18932 oder höher.
- Aktivieren Sie das WSL 2-Feature unter Windows.
- Installieren Sie eine Linux-Distribution (Ubuntu 18,04 für unsere Beispiele). Sie können Folgendes überprüfen: `wsl lsb_release -a`
- Stellen Sie sicher, dass die Ubuntu 18,04-Distribution im WSL 2-Modus ausgeführt wird. (WSL kann Verteilungen im v1-oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`
- Legen Sie Ubuntu 18,04 als Standardverteilung mithilfe von PowerShell mit: `wsl -s ubuntu 18.04` fest.

> [!NOTE]
> Es gibt zwar einige zusätzliche Einrichtungsschritte für die Verwendung des Windows-Subsystems für Linux, die Verwendung von WSL 2, zusammen mit vs Code und der Remote-WSL-Erweiterung, bietet jedoch den Entwicklungs Workflow "größte Glättung angibt Node. js" sowie eine Anpassung an die Mehrzahl der Tools, Gewusst wie Hier finden Sie Artikel, Tutorials und [Bereitstellungs Umgebungen](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01) . Wenn Sie sich jedoch für die Verwendung von Node. js unter Windows verpflichtet haben, finden Sie in unserem Handbuch weitere Informationen zum [Einrichten der Node. js-Entwicklungsumgebung direkt unter Windows](./setup-on-windows.md). Wenn Sie sich in der (etwas seltenen) Situation befinden, in der eine Node. js-App auf einem Windows-Server gehostet werden muss, scheint das häufigste Szenario [ein Reverseproxy zu verwenden](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Hierfür gibt es zwei Möglichkeiten: 1) [verwenden Sie iisnode](https://harveywilliams.net/blog/installing-iisnode) oder [direkt](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Wir behalten diese Ressourcen nicht bei und empfehlen die [Verwendung von Linux-Servern zum Hosten Ihrer Node. js-apps](https://azure.microsoft.com/en-us/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Typen von Node. js-Anwendungen

Node. js ist eine JavaScript-Laufzeit, die primär zum Erstellen von Webanwendungen verwendet wird. Anders ausgedrückt: Es handelt sich um eine serverseitige Implementierung von JavaScript, mit der das Back-End einer Anwendung geschrieben wird. (Obwohl viele Node. js-Frameworks auch das Front-End verarbeiten können.) Im folgenden finden Sie einige Beispiele für das, was Sie mit Node. js erstellen können.

- **Single-Page-Apps (Spas)** : Dies sind Web-Apps, die in einem Browser funktionieren und eine Seite nicht jedes Mal erneut laden müssen, wenn Sie Sie zum erhalten neuer Daten verwenden. Einige Beispiele für Spas sind soziale Netzwerk-apps, e-Mail-oder Karten-apps, Online Text-oder Zeichnungs Tools usw.
- **Echt Zeit-apps (RTAS)** : Dies sind Web-Apps, die es Benutzern ermöglichen, Informationen zu erhalten, sobald Sie von einem Autor veröffentlicht werden, anstatt zu verlangen, dass der Benutzer (oder die Software) eine Quelle regelmäßig auf Aktualisierungen überprüft. Einige Beispiele für RTAS sind Instant Messaging-Apps oder Chaträume, Online-Multiplayerspiele, die im Browser, online Kollaborations Dokumente, communityspeicher, Videokonferenz-apps usw. abgespielt werden können.
- **Datenstreaminganwendungen**: Dabei handelt es sich um apps (oder Dienste), die Daten/Inhalt beim Eintreffen (oder beim Erstellen) senden, während die Verbindung geöffnet bleibt, um weiterhin weitere Daten, Inhalte oder Komponenten nach Bedarf herunterzuladen. Einige Beispiele hierfür sind Video-und Audiostreaming-apps.
- **Rest-APIs**: Dies sind Schnittstellen, die Daten für die Web-App einer anderen Person bereitstellen, mit der Sie interagieren. Ein Kalender-API-Dienst könnte z. b. Datums-und Uhrzeitwerte für einen Konzert Veranstaltungsort bereitstellen, der von der lokalen Ereignis Website einer anderen Person verwendet werden kann.
- **Server seitige gerenderte Apps (SSRS)** : Diese Web-Apps können sowohl auf dem Client (in Ihrem Browser/am Front-End) als auch auf dem Server (dem Back-End) ausgeführt werden. Dadurch können Seiten, die dynamisch angezeigt werden (HTML für HTML-Code generieren), Inhalte, die bekannt sind, angezeigt werden. Diese werden häufig als "isomorphe" oder "universelle" Anwendungen bezeichnet. SSRS verwendet Spa-Methoden, da Sie nicht jedes Mal neu geladen werden müssen, wenn Sie Sie verwenden. SRAs bieten jedoch einige Vorteile, die für Sie von Bedeutung sein können, z. b. das Erstellen von Inhalten auf Ihrer Website in Google-Suchergebnissen und das Bereitstellen eines Vorschau Images, wenn Verknüpfungen zu Ihrer APP auf sozialen Medien wie Twitter oder Facebook freigegeben werden. Der potenzielle Nachteil ist, dass ein Node. js-Server ständig ausgeführt wird. In den Beispielen kann eine Social Network-APP, die Ereignisse unterstützt, die Benutzer in den Suchergebnissen und sozialen Medien anzeigen möchten, von der SSR profitieren, während eine e-Mail-app in Ordnung sein kann. Sie können auch auf dem Server gerenderte No-Spa-apps ausführen. Dies ist ein Beispiel für einen WordPress-Blog. Wie Sie sehen können, können Dinge kompliziert werden, Sie müssen lediglich entscheiden, was wichtig ist.
- **Befehlszeilen Tools**: Diese ermöglichen es Ihnen, sich wiederholende Aufgaben zu automatisieren und dann das Tool über das riesige Node. js-Ökosystem zu verteilen. Ein Beispiel für ein Befehlszeilen Tool ist curl, das für die Client-URL steht und zum Herunterladen von Inhalten aus einer Internet-URL verwendet wird. cURL wird häufig verwendet, um z. b. Node. js oder, in unserem Fall einen Node. js-Versions-Manager, zu installieren.
- **Hardware Programmierung**: Node. js ist zwar nicht so beliebt wie Web-Apps, aber Node. js wächst bei IOT-Verwendungsmöglichkeiten, wie z. b. das Sammeln von Daten von Sensoren, Beacons, Sendern, Motoren oder alles, was große Datenmengen generiert. Mithilfe von Node. js können Sie die Datensammlung aktivieren, diese Daten analysieren, zwischen einem Gerät und einem Server hin-und herkommunizieren und auf der Grundlage der Analyse Maßnahmen ergreifen. NPM enthält mehr als 80 Pakete für Arduino-Controller, Raspberry Pi, Intel IOT Edison, verschiedene Sensoren und Bluetooth-Geräte.

## <a name="try-using-nodejs-in-vs-code"></a>Verwenden Sie Node. js in vs Code

1. Öffnen Sie das Ubuntu-Terminal, und erstellen Sie ein neues Verzeichnis: `mkdir HelloNode`, und geben Sie dann das Verzeichnis ein: `cd HelloNode`

2. Erstellen Sie eine leere JavaScript-Datei mit dem Namen "App. js": `touch app.js`

3. Öffnen Sie das Verzeichnis und die leere Datei in vs Code:: `code .`

4. Erstellen Sie eine einfache Zeichen folgen Variable in "App. js", und senden Sie den Inhalt der Zeichenfolge an die Konsole, indem Sie diese in der Datei "App. js" eingeben:

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. So führen Sie die Datei "App. js" mit "Node. js" aus. Öffnen Sie das Ubuntu-Terminal direkt innerhalb vs Code, indem Sie  > -**Terminal** **anzeigen**auswählen (oder drücken Sie STRG + ', wobei Sie das Graviszeichen-Zeichen verwenden). Wenn Sie das Standard Terminal ändern müssen, klicken Sie auf das Dropdown Menü, und wählen Sie **Standardshell auswählen**aus. Wählen Sie dann **WSL** als standardterminalshell aus, die mit vs Code verwendet werden soll.

6. Geben Sie im Terminal Folgendes ein: `node app.js`. Die Ausgabe sollte angezeigt werden: "Hallo Welt".

> [!NOTE]
> Beachten Sie Folgendes: da Sie die WSL-Remote Erweiterung bereits installiert haben, wird Ihr Verzeichnis in einer Remote Umgebung geöffnet, die auf dem Ubuntu Linux System ausgeführt wird, wie in der grünen Registerkarte unten links im vs Code Fenster gezeigt. Beachten Sie außerdem, dass beim Eingeben von "`console`" in der Datei "App. js" vs Code unterstützte Optionen im Zusammenhang mit dem [`console`-](https://developer.mozilla.org/docs/Web/API/Console) Objekt angezeigt wird, das Sie mithilfe von IntelliSense auswählen können. Experimentieren Sie mit IntelliSense, und verwenden Sie andere [JavaScript-Objekte](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Sie sollten das neue [Windows-Terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) ausprobieren, wenn Sie beabsichtigen, mehrere Befehlszeilen (Ubuntu, PowerShell, Windows-Eingabeaufforderung usw.) zu verwenden, oder wenn Sie [Ihr Terminal anpassen](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)möchten, einschließlich Text, Hintergrundfarben, Tastenbindungen usw.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Einrichten eines grundlegenden Web-App-Frameworks mithilfe von Express

Express ist ein minimales, flexibles und optimiertes Node. js-Framework, mit dem Sie eine Web-App, die mehrere Arten von Anforderungen verarbeiten kann, wie Get, Put, Post und DELETE, einfacher entwickeln können. Express wird mit einem Anwendungs Generator verwendet, der automatisch eine Datei Architektur für Ihre APP erstellt.

So erstellen Sie ein Projekt mit "Express. js":

1. Öffnen Sie das WSL-Terminal (Ubuntu 18,04).
2. Erstellen Sie einen neuen Projektordner: `mkdir ExpressProjects`, und geben Sie das Verzeichnis ein: `cd ExpressProjects`.
3. Verwenden Sie Express, um eine HelloWorld-Projektvorlage zu erstellen: `npx express-generator HelloWorld --view=pug`.

>[!NOTE]
> Wir verwenden den Befehl "`npx`" hier, um das Express. js-Knoten Paket auszuführen, ohne es tatsächlich zu installieren (oder durch temporäres installieren, je nachdem, wie Sie es sich vorstellen möchten). Wenn Sie versuchen, den Befehl "`express`" zu verwenden oder die mit: `express --version` installierte Version von Express zu überprüfen, erhalten Sie eine Antwort, dass Express nicht gefunden werden kann. Wenn Sie Express Global installieren möchten, damit es immer wieder verwendet werden kann, verwenden Sie Folgendes: `npm install -g express-generator`. Sie können eine Liste der Pakete anzeigen, die von NPM mithilfe von `npm list` installiert wurden. Sie werden nach Tiefe aufgelistet (die Anzahl der verschachtelten Verzeichnisse Tiefe). Pakete, die Sie installiert haben, werden in der Tiefe 0 angezeigt. Die Abhängigkeiten des Pakets liegen in Tiefe 1, weiteren Abhängigkeiten in Tiefe 2 usw. Weitere Informationen finden Sie [unter Differenz zwischen NPX und NPM?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) auf StackOverflow.

4. Überprüfen Sie die in Express enthaltenen Dateien und Ordner, indem Sie das Projekt in vs Code öffnen, mit: `code .`

   Die Dateien, die von Express generiert werden, erstellen eine Web-App, die eine Architektur verwendet, die zuerst etwas überwältigend erscheinen kann. Im Fenster "vs Code- **Explorer** " (STRG + UMSCHALT + E zum anzeigen) sehen Sie, dass die folgenden Dateien und Ordner generiert wurden:

   - `bin`. installiert haben. Enthält die ausführbare Datei, mit der die APP gestartet wird. Es wird ein Server (auf Port 3000, wenn keine Alternative bereitgestellt wird) ausgelöst und die grundlegende Fehlerbehandlung eingerichtet. 
   - `public`. installiert haben. Enthält alle Dateien, auf die öffentlich zugegriffen wird, einschließlich JavaScript-Dateien, CSS-Stylesheets, Schriftart Dateien, Bilder und alle anderen Ressourcen, die Benutzer benötigen, wenn Sie eine Verbindung mit der Website herstellen.
   - `routes`. installiert haben. Enthält alle Routen Handler für die Anwendung. Zwei Dateien, `index.js` und `users.js`, werden automatisch in diesem Ordner generiert und dienen als Beispiele für die Trennung der Routen Konfiguration Ihrer Anwendung.
   - `views`. installiert haben. Enthält die Dateien, die von Ihrer Vorlagen-Engine verwendet werden. Express wird so konfiguriert, dass nach einer übereinstimmenden Ansicht gesucht wird, wenn die Rendering-Methode aufgerufen wird. Die Standardvorlagen-Engine ist Jade, aber Jade ist zugunsten von pug veraltet. Daher haben wir das Flag `--view` verwendet, um die Ansichts-Engine (Template) zu ändern. Sie können die `--view`-Flag-Optionen und andere mithilfe `express --help` anzeigen.
   - `app.js`. installiert haben. Der Ausgangspunkt ihrer app. Es lädt alles und beginnt damit, Benutzer Anforderungen zu erfüllen. Es ist im Grunde der Kleber, der alle Teile vereint.
   - `package.json`. installiert haben. Enthält die Projektbeschreibung, den Skript-Manager und das App-Manifest. Der Hauptzweck besteht darin, die Abhängigkeiten der APP und ihre jeweiligen Versionen zu verfolgen.

5. Sie müssen nun die von Express verwendeten Abhängigkeiten installieren, um Ihre HelloWorld Express-App zu erstellen und auszuführen (die Pakete, die für Aufgaben wie das Ausführen des Servers verwendet werden, wie in der `package.json`-Datei definiert). Öffnen Sie in vs Code das WSL-Terminal, indem Sie  > -**Terminal** **anzeigen**auswählen (oder drücken Sie STRG + ', wobei Sie das Graviszeichen-Zeichen verwenden), und stellen Sie sicher, dass Sie sich noch im Projektverzeichnis ' HelloWorld ' befinden. Installieren Sie die Express-Paketabhängigkeiten mit:

```bash
npm install
```

6. An diesem Punkt haben Sie das Framework für eine Web-App mit mehreren Seiten eingerichtet, die Zugriff auf eine große Bandbreite an APIs und http-Hilfsprogrammmethoden und Middleware hat, sodass es einfacher ist, eine robuste API zu erstellen. Starten Sie die Express-App auf einem virtuellen Server, indem Sie Folgendes eingeben:

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> Der `DEBUG=myapp:*`-Teil des obigen Befehls bedeutet, dass Sie Node. js mitteilen, dass Sie die Protokollierung zu Debuggingzwecken aktivieren möchten. Denken Sie daran, "MyApp" durch ihren APP-Namen zu ersetzen. Sie finden ihren APP-Namen in der Datei "Package. JSON" unter der Eigenschaft "Name". Der Befehl "`npm start`" teilt NPM mit, die Skripts in der Datei "Package. JSON" auszuführen.

7. Sie können jetzt die laufende App anzeigen, indem Sie einen Webbrowser öffnen und zu " **localhost: 3000** " navigieren.

   ![Screenshot der Express-App, die in einem Browser ausgeführt wird](../images/express-app.png)

8. Nun, da ihre HelloWorld-Express-App lokal in Ihrem Browser ausgeführt wird, versuchen Sie, eine Änderung vorzunehmen, indem Sie den Ordner "Views" im Projektverzeichnis öffnen und die Datei "index. pug" auswählen. Ändern Sie nach dem Öffnen `h1= title` in `h1= "Hello World!"`, und wählen Sie **Speichern** (STRG + S) aus. Zeigen Sie Ihre Änderung an, indem Sie die URL " **localhost: 3000** " in Ihrem Webbrowser aktualisieren.

9. Geben Sie in Ihrem Terminal Folgendes ein, um die Ausführung ihrer Express-App zu unterbinden: **STRG + C**

## <a name="try-using-a-nodejs-module"></a>Versuchen Sie, ein Node. js-Modul zu verwenden.

Node. js verfügt über Tools, die Sie beim Entwickeln von serverseitigen Web-Apps unterstützen, einige integriert sind und viele weitere über NPM verfügbar sind. Diese Module können viele Aufgaben unterstützen:

|Tool               |Verwendung                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|GM, Sharp          |Bildbearbeitung, einschließlich Bearbeitung, Änderung der Größe, Komprimierung usw., direkt in Ihrem JavaScript-Code |
|PDFKit             |PDF-Generierung                                                                                            |
|validator. js       |Zeichen folgen Validierung                                                                                         |
|imagemin, UglifyJS2|Minimierung                                                                                              |
|spritesmith        |Sprite-Blatt Generierung                                                                                   |
|Winston            |Protokollierung                                                                                                  |
|"Commander. js"       |Erstellen von Befehlszeilen Anwendungen                                                                       |

Verwenden Sie das integrierte Betriebssystem Modul, um einige Informationen über das Betriebssystem Ihres Computers zu erhalten:

1) Öffnen Sie in Ihrem WSL-Terminal (Ubuntu 18,04) die Node. js-CLI. Die `>`-Eingabeaufforderung wird angezeigt, dass Sie "Node. js" nach Eingabe von "`node`" verwenden.

2) Geben Sie Folgendes ein, um das Betriebssystem zu identifizieren, das Sie zurzeit verwenden (damit eine Antwort zurückgegeben werden kann, in der Sie wissen, dass Sie Windows verwenden): `os.platform()`

3) Geben Sie Folgendes ein, um die Architektur Ihrer CPU zu überprüfen: `os.arch()`

4) Geben Sie Folgendes ein, um die auf Ihrem System verfügbaren CPUs anzuzeigen: `os.cpus()`

5) Belassen Sie die Node. js-CLI, indem Sie `.exit` eingeben oder STRG + C zweimal auswählen.

   > [!TIP]
   > Sie können das Modul Node. js OS zum Überprüfen der Plattform und zum Zurückgeben einer plattformspezifischen Variablen verwenden: Win32/. bat für Windows-Entwicklung, Darwin/. sh für Mac/UNIX, Linux, SunOS usw. (z. b. `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie einige grundlegende Informationen zu den Möglichkeiten von Node. js kennengelernt, die Verwendung der Node. js-Befehlszeile in vs Code ausprobiert, eine einfache Web-App mit Express. js erstellt und diese lokal in Ihrem Webbrowser ausgeführt und dann einige der integrierten Node. js-Module ausprobiert. Weitere Informationen zum Installieren und verwenden einiger gängiger Node. js-Webframe Works finden Sie in der nächsten Anleitung, in der "Next. js" (ein auf dem Server gerendertes Webframework basierend auf der Reaktion), "nuxt. js" (ein auf Vue basierendes Server gerendertes Webframework) und "Gatsby (a statisch gerendertes Webframework basierend auf der Reaktion). Sie können auch überspringen, um zu erfahren, wie Sie mit MongoDb-oder PostgreSQL-Datenbanken oder docker-Containern arbeiten.

- [Einstieg in Node. js-Webframe Works unter Windows](./web-frameworks.md)
- [Einstieg in das Verbinden von Node. js-apps mit einer Datenbank](./databases.md)
- [Einstieg in die Verwendung von Docker-Containern mit Node. js](./containers.md)
