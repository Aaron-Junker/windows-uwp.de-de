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
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657082"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Einstieg in die Verwendung von Node. js unter Windows für Einsteiger

Wenn Sie mit der Verwendung von Node. js noch nicht vertraut sind, erhalten Sie in diesem Handbuch einen Einstieg in einige Grundlagen.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass Sie die Schritte zum [Einrichten der Node. js-Entwicklungsumgebung auf](./setup-on-windows.md)systemeigenen Fenstern bereits abgeschlossen haben, einschließlich:

- Installieren Sie einen Node. js-Versions-Manager.
- Installieren Sie Visual Studio Code.

Die direkte Installation von Node. js unter Windows ist die einfachste Möglichkeit, um mit der Ausführung grundlegender Node. js-Vorgänge mit einer minimalen Menge an Einrichtung zu beginnen.

Wenn Sie bereit sind, Node. js zum Entwickeln von Anwendungen für die Produktion zu verwenden, was in der Regel die Bereitstellung auf einem Linux-Server umfasst, empfiehlt es sich, die [node. js-Entwicklungsumgebung mit WSL2 einzurichten](./setup-on-wsl2.md). Obwohl es möglich ist, Web-Apps auf Windows-Servern bereitzustellen, ist es viel häufiger, [Linux-Server zum Hosten Ihrer Node. js-apps zu verwenden](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Typen von Node.js-Anwendungen

Node. js ist eine JavaScript-Laufzeit, die primär zum Erstellen von Webanwendungen verwendet wird. Anders ausgedrückt: Es handelt sich um eine serverseitige Implementierung von JavaScript, mit der das Back-End einer Anwendung geschrieben wird. (Obwohl viele Node. js-Frameworks auch das Front-End verarbeiten können.) Im folgenden finden Sie einige Beispiele für das, was Sie mit Node. js erstellen können.

- **Single-Page-Apps (Spas)** : Hierbei handelt es sich um Web-Apps, die in einem Browser funktionieren. Sie müssen eine Seite nicht jedes Mal erneut laden, wenn Sie Sie zum erhalten neuer Daten verwenden. Einige Beispiele für Spas sind soziale Netzwerk-apps, e-Mail-oder Karten-apps, Online Text-oder Zeichnungs Tools usw.
- **Echt Zeit-apps (Real-Time apps, RTAS)** : Dies sind Web-Apps, die es Benutzern ermöglichen, Informationen zu erhalten, sobald Sie von einem Autor veröffentlicht werden, anstatt zu verlangen, dass der Benutzer (oder die Software) in regelmäßigen Abständen eine Quelle auf Aktualisierungen überprüft. Einige Beispiele für RTAS sind Instant Messaging-Apps oder Chaträume, Online-Multiplayerspiele, die im Browser, online Kollaborations Dokumente, communityspeicher, Videokonferenz-apps usw. abgespielt werden können.
- **Datenstreaminganwendungen**: Hierbei handelt es sich um apps (oder Dienste), die Daten/Inhalt beim Eintreffen (oder beim Erstellen) senden, während die Verbindung geöffnet bleibt, um weiterhin weitere Daten, Inhalte oder Komponenten nach Bedarf herunterzuladen. Einige Beispiele hierfür sind Video-und Audiostreaming-apps.
- **Rest-APIs**: Dies sind Schnittstellen, die Daten für die Web-App einer anderen Person bereitstellen, mit der Sie interagieren. Ein Kalender-API-Dienst könnte z. b. Datums-und Uhrzeitwerte für einen Konzert Veranstaltungsort bereitstellen, der von der lokalen Ereignis Website einer anderen Person verwendet werden kann.
- **Serverseitige gerenderte Apps (SSRS)** : diese Web-Apps können sowohl auf dem Client (in Ihrem Browser/am Front-End) als auch auf dem Server (dem Back-End) ausgeführt werden. Dadurch können Seiten, die dynamisch angezeigt werden sollen (HTML für HTML-Code generieren), Inhalte enthalten, die nicht bekannt sind, wenn Sie verfügbar sind. Diese werden häufig als "isomorphe" oder "universelle" Anwendungen bezeichnet. SSRS verwendet Spa-Methoden, da Sie nicht jedes Mal neu geladen werden müssen, wenn Sie Sie verwenden. SSRS bieten jedoch einige Vorteile, die für Sie von Bedeutung sein können, z. b. das Erstellen von Inhalten auf Ihrer Website in Google-Suchergebnissen und das Bereitstellen eines Vorschau Images, wenn Verknüpfungen zu Ihrer APP auf sozialen Medien wie Twitter oder Facebook freigegeben werden. Der potenzielle Nachteil ist, dass ein Node. js-Server ständig ausgeführt wird. In den Beispielen kann eine Social Network-APP, die Ereignisse unterstützt, die Benutzer in den Suchergebnissen und sozialen Medien anzeigen möchten, von der SSR profitieren, während eine e-Mail-app in Ordnung sein kann. Sie können auch auf dem Server gerenderte No-Spa-apps ausführen. Dies ist ein Beispiel für einen WordPress-Blog. Wie Sie sehen können, können Dinge kompliziert werden, Sie müssen lediglich entscheiden, was wichtig ist.
- **Befehlszeilen Tools**: Diese ermöglichen es Ihnen, sich wiederholende Aufgaben zu automatisieren und dann das Tool über das riesige Node. js-Ökosystem zu verteilen. Ein Beispiel für ein Befehlszeilen Tool ist curl, das für die Client-URL steht und zum Herunterladen von Inhalten aus einer Internet-URL verwendet wird. cURL wird häufig verwendet, um z. b. Node. js oder, in unserem Fall einen Node. js-Versions-Manager, zu installieren.
- **Hardware Programmierung**: bei Verwendung von Web-Apps ist Node. js für IOT-Anwendungen immer beliebter, wie z. b. das Sammeln von Daten von Sensoren, Beacons, Sendern, Motoren oder anderen, die große Datenmengen generieren. Mithilfe von Node. js können Sie die Datensammlung aktivieren, diese Daten analysieren, zwischen einem Gerät und einem Server hin-und herkommunizieren und auf der Grundlage der Analyse Maßnahmen ergreifen. NPM enthält mehr als 80 Pakete für Arduino-Controller, Raspberry Pi, Intel IOT Edison, verschiedene Sensoren und Bluetooth-Geräte.

## <a name="try-using-nodejs-in-vs-code"></a>Verwenden Sie Node. js in vs Code

1. Öffnen Sie die Befehlszeile (Eingabeaufforderung, PowerShell oder was Sie bevorzugen), und erstellen Sie ein neues Verzeichnis: `mkdir HelloNode`, und geben Sie dann das Verzeichnis ein: `cd HelloNode`

2. Erstellen Sie eine JavaScript-Datei mit dem Namen "App. js" und einer Variablen mit dem Namen "msg" in: `echo var msg > app.js`

3. Öffnen Sie das Verzeichnis und die Datei "App. js" in vs Code:: `code .`

4. Fügen Sie eine einfache Zeichen folgen Variable ("Hallo Welt") hinzu, und senden Sie dann den Inhalt der Zeichenfolge an die Konsole, indem Sie diese in der Datei "App. js" eingeben:

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. So führen Sie die Datei "App. js" mit "Node. js" aus. Öffnen Sie das Terminal direkt innerhalb vs Code, indem Sie > **Terminal** **anzeigen** auswählen (oder drücken Sie STRG + ', indem Sie das Graviszeichen-Zeichen verwenden). Wenn Sie das Standard Terminal ändern müssen, klicken Sie auf das Dropdown Menü, und wählen Sie **Standardshell auswählen**aus.

6. Geben Sie im Terminal Folgendes ein: `node app.js`. Die Ausgabe sollte angezeigt werden: "Hallo Welt".

> [!NOTE]
> Beachten Sie, dass beim Eingeben von `console` in der Datei "App. js" vs Code unterstützte Optionen im Zusammenhang mit dem [`console`](https://developer.mozilla.org/docs/Web/API/Console) Objekt angezeigt wird, die Sie aus der Verwendung von IntelliSense auswählen können. Experimentieren Sie mit IntelliSense, und verwenden Sie andere [JavaScript-Objekte](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Testen Sie das neue [Windows-Terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) , wenn Sie beabsichtigen, mehrere Befehlszeilen (Ubuntu, PowerShell, Windows-Eingabeaufforderung usw.) zu verwenden, oder wenn Sie [Ihr Terminal anpassen](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)möchten, einschließlich Text, Hintergrundfarben, Tastenkombinationen, mehreren Fensterbereichen usw.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Einrichten eines einfachen Web-App-Frameworks mit Express

Express ist ein minimales, flexibles und optimiertes Node.js-Framework, das die Entwicklung einer Web-App vereinfacht, die mehrere Typen von Anforderungen wie GET, PUT, POST und DELETE verarbeiten kann. Express enthält einen Anwendungsgenerator, der automatisch eine Dateiarchitektur für Ihre App erstellt.

So erstellen Sie ein Projekt mit "Express. js":

1. Öffnen Sie die Befehlszeile (Eingabeaufforderung, PowerShell oder je nachdem, was Sie bevorzugen).
2. Neuen Projektordner erstellen: `mkdir ExpressProjects` und dieses Verzeichnis eingeben: `cd ExpressProjects`
3. Verwenden Sie Express, um eine HelloWorld-Projektvorlage zu erstellen: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Wir verwenden den Befehl "`npx`" hier, um das Express. js-Knoten Paket auszuführen, ohne es tatsächlich zu installieren (oder durch temporäres installieren, je nachdem, wie Sie es sich vorstellen möchten). Wenn Sie versuchen, den `express` Befehl zu verwenden oder die Version von Express zu überprüfen, die mit: `express --version`installiert wurde, erhalten Sie eine Antwort, dass Express nicht gefunden werden kann. Wenn Sie Express Global installieren möchten, damit es immer wieder verwendet werden kann, verwenden Sie Folgendes: `npm install -g express-generator`. Mithilfe `npm list`können Sie eine Liste der Pakete anzeigen, die von NPM installiert wurden. Sie werden nach der Tiefe (Anzahl der geschachtelten Verzeichnisebenen) aufgelistet. Pakete, die Sie installiert haben, liegen auf Ebene 0. Die Abhängigkeiten dieses Pakets liegen auf Ebene 1, weitere Abhängigkeiten auf Ebene 2 usw. Weitere Informationen finden Sie [unter Differenz zwischen NPX und NPM?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) auf StackOverflow.

4. Überprüfen Sie die in Express enthaltenen Dateien und Ordner, indem Sie das Projekt in vs Code öffnen, und zwar mit: `code .`

   Die Dateien, die Express generiert, erstellen eine Web-App mithilfe einer bestimmten Architektur, die zuerst etwas überwältigend erscheinen mag. Im Fenster "vs Code- **Explorer** " (STRG + UMSCHALT + E zum anzeigen) sehen Sie, dass die folgenden Dateien und Ordner generiert wurden:

   - `bin`. Enthält die ausführbare Datei, die Ihre App startet. Startet einen Server (an Port 3000, wenn keine Alternative angegeben wird) und richtet eine grundlegende Fehlerbehandlung ein. 
   - `public`. Enthält alle öffentlich zugänglichen Dateien einschließlich JavaScript-Dateien, CSS-Stylesheets, Schriftartdateien, Bildern und allen anderen Ressourcen, die Benutzer benötigen, wenn sie eine Verbindung mit Ihrer Website herstellen.
   - `routes`. Enthält alle Routenhandler für die Anwendung. Zwei Dateien, `index.js` und `users.js`, werden in diesem Ordner automatisch generiert, um als Beispiele zum Aussondern der Routenkonfiguration Ihrer Anwendung zu dienen.
   - `views`. Enthält die Dateien, die von Ihrer Vorlagen-Engine verwendet werden. Express ist dazu konfiguriert, hier nach einer übereinstimmenden Ansicht zu suchen, wenn die Rendermethode aufgerufen wird. Die Standardvorlagen-Engine ist Jade, aber sie wurde von Pug abgelöst, daher haben wir das `--view`-Flag verwendet, um die Anzeige-Engine (Vorlagen-Engine) zu ändern. Sie können die `--view`-Flagoptionen und andere mittels `express --help` anzeigen.
   - `app.js`. Der Startpunkt der App. Sie lädt alles und beginnt, Benutzeranforderungen zu verarbeiten. Im Grunde hält sie alle verschiedenen Teile wie ein Klebstoff zusammen.
   - `package.json`. Sie enthält Projektbeschreibung, Skripts-Manager und App-Manifest. Ihr Hauptzweck ist, die Abhängigkeiten Ihrer App und ihre jeweiligen Versionen nachzuverfolgen.

5. Sie müssen nun die von Express verwendeten Abhängigkeiten installieren, um Ihre HelloWorld Express-App zu erstellen und auszuführen (die Pakete, die für Aufgaben wie das Ausführen des Servers verwendet werden, wie in der `package.json`-Datei definiert). Öffnen Sie in vs Code Ihr Terminal, indem Sie > **Terminal** **anzeigen** auswählen (oder drücken Sie Strg + ", verwenden Sie das Graviszeichen-Zeichen). Stellen Sie sicher, dass Sie sich noch im Projektverzeichnis" HelloWorld "befinden. Installieren Sie die Express-Paketabhängigkeiten mit:

```bash
npm install
```

6. An diesem Punkt haben Sie das Framework für eine mehrseitige Web-App mit Zugriff auf eine Vielzahl von APIs und nützliche HTTP-Methoden und Middleware eingerichtet, was das Erstellen einer stabilen API erleichtert. Starten Sie die Express-App auf einem virtuellen Server, indem Sie Folgendes eingeben:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> Der `DEBUG=myapp:*` Teil des obigen Befehls bedeutet, dass Sie Node. js mitteilen, dass Sie die Protokollierung zu Debuggingzwecken aktivieren möchten. Denken Sie daran, "MyApp" durch ihren APP-Namen zu ersetzen. Sie finden ihren APP-Namen in der `package.json`-Datei unter der Eigenschaft "Name". Mit `npx cross-env` wird die `DEBUG`-Umgebungsvariable in einem beliebigen Terminal festgelegt, aber Sie können Sie auch mit der Terminal spezifischen Methode festlegen. Der `npm start` Befehl teilt NPM mit, die Skripts in ihrer `package.json`-Datei auszuführen.

7. Sie können jetzt die laufende App anzeigen, indem Sie einen Webbrowser öffnen und zu " **localhost: 3000** " navigieren.

   ![Screenshot einer im Browser ausgeführten Express-App](../images/express-app.png)

8. Nun, da ihre HelloWorld-Express-App lokal in Ihrem Browser ausgeführt wird, versuchen Sie, eine Änderung vorzunehmen, indem Sie den Ordner "Views" im Projektverzeichnis öffnen und die Datei "index. pug" auswählen. Nachdem Sie geöffnet haben, ändern Sie `h1= title` in `h1= "Hello World!"` und klicken Sie auf **Speichern** (STRG + S). Zeigen Sie Ihre Änderung an, indem Sie die URL " **localhost: 3000** " in Ihrem Webbrowser aktualisieren.

9. Geben Sie in Ihrem Terminal Folgendes ein, um die Ausführung ihrer Express-App zu verhindern: **STRG + C**

## <a name="try-using-a-nodejs-module"></a>Verwenden eines Node.js-Moduls

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

1) Öffnen Sie in der Befehlszeile die Node. js-CLI. Die `>`-Eingabeaufforderung wird angezeigt, dass Sie Node. js nach der Eingabe von: `node`

2) Geben Sie Folgendes ein, um das Betriebssystem zu identifizieren, das Sie zurzeit verwenden (damit eine Antwort zurückgegeben werden kann, in der Sie wissen, dass Sie Windows verwenden): `os.platform()`

3) Geben Sie Folgendes ein, um die Architektur Ihrer CPU zu überprüfen: `os.arch()`

4) Geben Sie Folgendes ein, um die auf Ihrem System verfügbaren CPUs anzuzeigen: `os.cpus()`

5) Verlassen Sie die Node.js-Befehlszeilenschnittstelle durch Eingeben von `.exit` oder zweimaliges Auswählen von STRG+C.

   > [!TIP]
   > Sie können das Modul Node. js OS zum Überprüfen der Plattform und zum Zurückgeben einer plattformspezifischen Variablen verwenden: Win32/. bat für die Windows-Entwicklung, Darwin/. sh für Mac/UNIX, Linux, SunOS usw. (z. b. `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie einige grundlegende Informationen zu den Möglichkeiten von Node. js kennengelernt, die Verwendung der Node. js-Befehlszeile in vs Code ausprobiert, eine einfache Web-App mit Express. js erstellt und diese lokal in Ihrem Webbrowser ausgeführt und dann einige der integrierten Node. js-Module ausprobiert. Weitere Informationen zum Installieren und verwenden einiger gängiger Node. js-Webframe Works finden Sie in der nächsten Anleitung, in der "Next. js" (ein auf dem Server gerendertes Webframework basierend auf der Reaktion), "nuxt. js" (ein auf Vue basierendes Server gerendertes Webframework) und "Gatsby (a statisch gerendertes Webframework basierend auf der Reaktion). Sie können auch überspringen, um zu erfahren, wie Sie mit MongoDb-oder PostgreSQL-Datenbanken oder docker-Containern arbeiten.

- [Einstieg in Node. js-Webframe Works unter Windows](./web-frameworks.md)
- [Einstieg in das Verbinden von Node. js-apps mit einer Datenbank](./databases.md)
- [Einstieg in die Verwendung von Docker-Containern mit Node. js](./containers.md)
