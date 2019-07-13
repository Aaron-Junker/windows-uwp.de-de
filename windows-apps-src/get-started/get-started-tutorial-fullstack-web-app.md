---
title: Erstellen einer Single-Page-Web-App mit REST API-Back-End
description: Verwende beliebte Webtechnologien zum Erstellen einer gehosteten Web-App für den Microsoft Store
keywords: Gehostete Web-App, HWA, REST-API, einseitige App, SPA
ms.date: 05/10/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b1b837d6585507311dc2246d42f3094ce8b07421
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321176"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>Erstellen einer Single-Page-Web-App mit REST API-Back-End

**Erstellen einer gehosteten Web-App für den Microsoft Store mit beliebten Fullstack-Webtechnologien**

![Einfaches Gedächtnisspiel als Single-Page-Web-App](images/fullstack.png)

Dieses zweiteilige Tutorial bietet einen kurzen Überblick über die moderne Fullstack-Webentwicklung, indem ein einfaches Gedächtnisspiel erstellt wird, das im Browser und auch als gehostete Web-App für den Microsoft Store funktioniert. In Teil I erstellst du einen einfachen REST-API-Dienst für das Back-End des Spiels. Durch das Hosten der Spiellogik in der Cloud als API-Dienst wird der Spielzustand beibehalten, damit deine Benutzer ihre Spielinstanzen auf verschiedenen Geräten weiterspielen können. In Teil II erstellst du die Front-End-Benutzeroberfläche als eine Single-Page-Web-App mit dynamischem Layout.

Wir verwenden einige der beliebtesten Webtechnologien, einschließlich der [Node.js](https://nodejs.org/en/)-Runtime und [Express](https://expressjs.com/) für die serverseitige Entwicklung, das [Bootstrap](https://getbootstrap.com/)-UI-Framework, das [Pug](https://www.npmjs.com/package/pug)-Vorlagenmodul und [Swagger](https://swagger.io/tools/) zum Erstellen von RESTful-APIs. Außerdem lernst du das [Azure-Portal](https://ms.portal.azure.com/) für das Cloudhosting kennen und arbeitest mit dem [Visual Studio Code](https://code.visualstudio.com/)-Editor.

## <a name="prerequisites"></a>Voraussetzungen

Wenn die folgenden Ressourcen nicht bereits auf deinem Computer installiert sind, kannst du sie unter den folgenden Links herunterladen:

 - [Node.js](https://nodejs.org/en/download/): Wähle hier die Option zum Hinzufügen von Node zu deinem Pfad (PATH) aus.

 - [Express-Generator](https://expressjs.com/en/starter/generator.html): Installiere Express nach der Installation von Node, indem du `npm install express-generator -g` ausführst.

 - [Visual Studio Code](https://code.visualstudio.com/)

Wenn du die letzten Schritte zum Hosten deines API-Diensts und der Gedächtnisspiel-App auf Microsoft Azure abschließen möchtest, musst du [ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/en-us/free/), sofern du dies noch nicht getan hast.

Wenn du den Azure-Teil auslassen (oder verschieben) möchtest, kannst du einfach die letzten Abschnitte der Teile I und II auslassen, die das Hosten in Azure und das Verpacken deiner App für den Microsoft Store behandeln. Der API-Dienst und die von dir erstellte Web-App werden weiterhin lokal auf deinem Computer ausgeführt (über `http://localhost:8000` bzw. `http://localhost:3000`).

## <a name="part-i-build-a-rest-api-backend"></a>Teil I: Erstellen eines REST-API-Back-Ends

Zunächst erstellen wir eine einfache Gedächtnisspiel-API für unsere Gedächtnisspiel-Web-App. Wir verwenden [Swagger](https://swagger.io/), um unsere API zu definieren und ein Codegerüst sowie eine Webbenutzeroberfläche für das manuelle Testen zu erstellen.

Wenn du diesen Teil überspringen und direkt mit [Teil II: Erstellen einer Single-Page-Web-App](#part-ii-build-a-single-page-web-application) fortfahren möchtest, findest du hier den [fertigen Code für Teil I](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend). Führe die Anweisungen in der *Infodatei* aus, um den Code lokal auszuführen. Alternativ findest du unter *5. Hosten deines API-Diensts in Azure und Aktivieren von CORS* Informationen zur Ausführung in Azure.

### <a name="game-overview"></a>Spielübersicht

*Gedächtnisspiele* (auch bekannt als [*Pairs*](https://en.wikipedia.org/wiki/Concentration_(game)) oder [*Memory*](https://en.wikipedia.org/wiki/Pelmanism_(system))) ist ein einfaches Spiel, das aus Kartenpaaren besteht. Die Karten werden umgedreht auf einen Tisch gelegt, und ein Spieler sieht sich zwei Karten an und prüft, ob sie übereinstimmen. Anschließend legt er die Karten wieder umgedreht auf den Tisch. Wenn ein Spieler ein übereinstimmendes Paar findet, werden die beiden Karten aus dem Spiel entfernt. Das Ziel des Spiels besteht darin, so schnell wie möglich alle Kartenpaare zu finden.

Die Struktur, die wir für unser Spiel verwenden, ist ganz einfach gehalten: Es ist ein einzelnes Spiel mit nur einem Spieler. Die Spiellogik ist jedoch serverseitig (und nicht clientseitig), sodass du dasselbe Spiel auf verschiedenen Geräten weiterspielen kannst und der Spielzustand gespeichert wird.

Die Datenstruktur für ein Gedächtnisspiel besteht einfach aus einem Array von JavaScript-Objekten, die jeweils eine einzelne Karte darstellen. Die Indizes im Array fungieren als Karten-IDs. Auf dem Server hat jedes Kartenobjekt einen Wert und ist mit dem Flag **cleared** gekennzeichnet. Ein Spiel mit zwei Übereinstimmungen (vier Karten) kann beispielsweise nach dem Zufallsprinzip generiert und wie folgt serialisiert werden:

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

Wenn das Spiel-Array an den Client übergeben wurde, werden die Wertschlüssel aus dem Array entfernt, um Täuschungen zu verhindern (z. B. Untersuchen des HTTP-Antworttexts mithilfe von F12-Browsertools). Und so sieht dieses neue Spiel für einen Client aus, der den **GET /game**-REST-Endpunkt aufruft:

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

Zum Thema Endpunkte gilt Folgendes: Der Gedächtnisspieldienst besteht aus drei REST-APIs.

#### <a name="post-new"></a>POST /new
Initialisiert ein neues Spiel mit der angegebenen Größe (Anzahl der Übereinstimmungen).

| Parameter | Beschreibung |
|-----------|-------------|
| int *size* |Die Anzahl der übereinstimmenden Paare im Spiel. Beispiel: `http://memorygameapisample/new?size=2`|

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Das neue Gedächtnisspiel in der angeforderten Größe ist bereit.|
| 400 BAD REQUEST| Die angeforderte Größe befindet sich außerhalb des zulässigen Bereichs.|


#### <a name="get-game"></a>GET /game
Ruft den aktuellen Spielzustand des Gedächtnisspiels ab.

*Keine Parameter*

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Gibt ein JSON-Array mit Kartenobjekten zurück. Jede Karte verfügt über eine **cleared**-Eigenschaft, die angibt, ob die übereinstimmende Karte bereits gefunden wurde. Übereinstimmende Karten geben auch ihren **Wert** wieder. Beispiel: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
Gibt eine Karte an, die aufgedeckt werden soll, und prüft, ob sie mit der zuvor aufgedeckten Karte übereinstimmt.

| Parameter | Beschreibung |
|-----------|-------------|
| int *card* | Karten-ID (Index im Spielearray) der aufzudeckenden Karte. Jeder Spielzug besteht aus zwei aufgedeckten Karten (z. B. zwei Aufrufe von **/guess** mit gültigen und eindeutigen *card*-Werten). Beispiel: `http://memorygameapisample/guess?card=0`|

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Gibt JSON-Code mit der **ID** und dem **Wert** der angegebenen Karte zurück. Beispiel: `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  Fehler bei der angegebenen Karte. Weitere Details findest du im HTTP-Antworttext.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. API-Spezifikationen und Generieren von Rumpfcode

Wir verwenden [Swagger](https://swagger.io/), um das Design der Gedächtnisspiel-APIs in funktionierenden Node.js-Servercode umzuwandeln. Hier erfährst du, wie du unsere [Gedächtnisspiel-APIs als Swagger-Metadaten](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json) definieren kannst. Damit erstellen wir unseren Serverrumpfcode.

1. Erstelle einen neuen Ordner (z. B. in deinem lokalen *GitHub*-Verzeichnis), und lade die Datei [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) herunter, die unsere Gedächtnisspiel-API-Definitionen enthält. Der Ordnername darf keine Leerzeichen enthalten.

2. Öffnen deine bevorzugte Shell ([oder verwende das integrierte Terminal von Visual Studio Code](https://code.visualstudio.com/docs/editor/integrated-terminal)) in dem Ordner, und führe den folgenden Node Package Manager (NPM)-Befehl aus, um das [Yeoman](https://yeoman.io/)-Codegerüsttool (yo) und den Swagger-Generator für deine globale ( **-g**) Node-Umgebung zu installieren:

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. Jetzt können wir das Servercodegerüst mithilfe von Swagger generieren:

    ```
    yo swaggerize
    ```

4. Der Befehl **swaggerize** wird dir einige Fragen stellen.
    - Pfad (oder URL) zum Swagger-Dokument: **api.json**
    - Framework: **express**
    - Name des Projekts (YourFolderNameHere): **[eingeben]**

    Beantworte die anderen Fragen nach Belieben. Die Informationen sind hauptsächlich dazu gedacht, dass die Datei *package.json* deine Kontaktinformationen erhält, damit du deinen Code als NPM-Paket verteilen kannst.

5. Installiere zum Schluss alle Abhängigkeiten (wie in *package.json* aufgelistet) für dein neues Projekt und die Unterstützung für die [Swagger-UI](https://swagger.io/swagger-ui/).

    ```
    npm install
    npm install swaggerize-ui
    ```

    Starte nun VS Code, wähle **Datei** > **Ordner öffnen...** , und wechsle zum Verzeichnis „MemoryGameAPI“. Dies ist der Node.js-API-Server, den du gerade erstellt hast. Er verwendet das beliebte [ExpressJS](https://expressjs.com/en/4x/api.html)-Webanwendungsframework zum Strukturieren und Ausführen deines Projekts.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. Anpassen des Servercodes und Einrichten von Debugging

Die Datei *server.js* in deinem Projektstammverzeichnis fungiert als Hauptfunktion des Servers. Öffne sie in VS Code, und füge Folgendes ein. Die vom generierten Code geänderten Zeilen sind mit weiteren Erläuterungen versehen.

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

Nun ist es an der Zeit, den Server auszuführen. Bei der Gelegenheit können wir auch Visual Studio Code für das Debuggen von Node einrichten. Wähle das Bereichssymbol **Debuggen** (STRG+UMSCHALT+D) und anschließend das Zahnradsymbol („launch.json“ öffnen) aus, und ändere „configurations” wie folgt:

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

Drücke nun F5, und öffne [https://localhost:8000](https://localhost:8000) in deinem Browser. Die Seite sollte die Swagger-UI unserer Gedächtnisspiel-API öffnen. Dort kannst du die Details und die Eingabefelder der einzelnen Methoden erweitern. Du kannst auch versuchen, die APIs aufzurufen. Deren Antworten enthalten jedoch nur Pseudodaten (bereitgestellt durch das [Swagmock](https://www.npmjs.com/package/swagmock)-Modul). Nun können wir unsere Spiellogik für diese APIs hinzuzufügen.

### <a name="3-set-up-your-route-handlers"></a>3. Einrichten der Routenhandler

Die Swagger-Datei (config\swagger.json) weist unseren Server an, wie verschiedene HTTP-Clientanforderungen behandelt werden, indem jeder von ihm festgelegte URL-Pfad einer Handler-Datei (in „\handlers“) und jede für den Pfad definierte Methode (z. B. **GET**, **POST**) einer `operationId` (Funktion) in der Handlerdatei zugeordnet wird.

In dieser Ebene unseres Programms fügen wir Eingabeüberprüfungen hinzu, bevor wir die verschiedenen Clientanforderungen an unser Datenmodell übergeben. Lade Folgendes herunter (oder kopiere es, und füge es anschließend ein):

 - [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D)-Code in deine **handlers\game.js**-Datei
 - [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D)-Code in deine **handlers\guess.js**-Datei
 - [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D)-Code in deine **handlers\new.js**-Datei

 In den Kommentaren in diesen Dateien findest du ggf. weitere Details zu den Änderungen, aber im Wesentlichen prüfen sie auf grundlegende Eingabefehler (z. B. der Client fordert ein neues Spiel mit weniger als einer Übereinstimmung an) und senden bei Bedarf aussagekräftige Fehlermeldungen. Die Handler leiten außerdem gültige Clientanforderungen an die entsprechenden Datendateien (in „\data“) zur weiteren Verarbeitung weiter. Dies sehen wir uns als Nächstes an.

### <a name="4-set-up-your-data-model"></a>4. Einrichten des Datenmodells

Nun können wir den Dienst mit den Pseudodaten, die als Platzhalter fungiert haben, durch ein richtiges Datenmodell unseres Gedächtnisspiels ersetzen.

Diese Ebene unseres Programms stellt die Spielkarten dar und stellt den Großteil der Spiellogik bereit. Dazu zählt auch das Mischen der Karten für ein neues Spiel, das Erkennen von übereinstimmenden Paaren und das Nachverfolgen des Spielzustands. Kopiere Folgendes, und füge es ein:

 - [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D)-Code in deine **data\game.js**-Datei
 - [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D)-Code in deine **data\guess.js**-Datei
 - [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D)-Code in deine **data\new.js**-Datei

Der Einfachheit halber speichern wir unser Spiel in einer globalen Variablen (`global.board`) auf dem Node-Server. In der praktischen Anwendung würdest du jedoch einen Cloudspeicher (z. B. Google [Cloud Datastore](https://cloud.google.com/datastore/) oder Azure [DocumentDB](https://azure.microsoft.com/services/cosmos-db/)) verwenden, um einen funktionierenden Gedächtnisspiel-API-Dienst zu erhalten, der mehrere Spiele und Spieler unterstützt.

Speichere alle Änderungen in VS Code. Starte den Server erneut (F5 in VS Code oder `npm start` in der Shell, und navigiere dann zu [https://localhost:8000](https://localhost:8000)), um die Spiele-API zu testen.

Jedes Mal, wenn du die Schaltfläche **Try it out!** (Jetzt testen) für einen der Vorgänge **/game**, **/guess** oder **/new** auswählst, überprüfst du den resultierenden **Antworttext** und den **Antwortcode** (s. u.), um sicherzustellen, dass alles wie erwartet funktioniert.

Teste Folgendes: 

1. Erstelle ein neues `size=2`-Spiel.

    ![Starten eines neuen Gedächtnisspiels über die Swagger-Benutzeroberfläche](images/swagger_new.png)

2. Errate ein paar Werte.

    ![Erraten einer Karte über die Swagger-Benutzeroberfläche](images/swagger_guess.png)

3. Sieh dir das Spiel im Verlauf an.

    ![Überprüfen des Spielzustands über die Swagger-Benutzeroberfläche](images/swagger_game.png)

Wenn alles gut aussieht, kann dein API-Dienst in Azure gehostet werden. Wenn Probleme auftreten, versuche, die folgenden Zeilen in „\data\game.js“ auszukommentieren.

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

Mit dieser Änderung gibt die Methode **GET /game** alle Kartenwerte (einschließlich derjenigen, die noch nicht deaktiviert wurden) zurück. Dies ist ein hilfreicher Trick zum Debuggen während der Entwicklung des Front-Ends für das Gedächtnisspiel.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (Optional) Hosten deines API-Diensts in Azure und Aktivieren von CORS

In der Azure-Dokumentation ist Folgendes erläutert:

 - [Registrieren einer neuen *API-App* mit dem Azure-Portal](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [Einrichten einer Git-Bereitstellung für deine API-App](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)
 - [Bereitstellen deines API-App-Codes in Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

Versuche bei der Registrierung, deinen *App-Namen* abzugrenzen (um Benennungskonflikte mit anderen Benutzern zu vermeiden, die Varianten unter der URL *http://memorygameapi.azurewebsites.net* anfordern).

Wenn Azure jetzt deine Swagger-Benutzeroberfläche anzeigt, ist nur noch ein letzter Schritt für das Gedächtnisspiel-Back-End erforderlich. Wähle im [Azure Portal](https://portal.azure.com) deinen neu erstellten *App-Dienst* aus. Wähle anschließend die Option **CORS** (Cross-Origin Resource Sharing) aus, oder suche nach dieser Option. Füge unter **Zulässige Ursprünge** ein Sternchen (`*`) hinzu, und klicke auf **Speichern**. Auf diese Weise kannst du während der Entwicklung auf dem lokalen Computer ursprungsübergreifende Aufrufe deines API-Diensts über das Gedächtnisspiel-Front-End ausführen. Sobald du das Gedächtnisspiel-Front-End fertig gestellt und in Azure bereitgestellt hast, kannst du diesen Eintrag durch die spezifische URL deiner Web-App ersetzen.

### <a name="going-further"></a>Vertiefung

Um die Gedächtnisspiel-API zu einem funktionierenden Back-End-Dienst für eine Produktions-App zu machen, solltest du den Code erweitern, um mehrere Spieler und Spiele zu unterstützen. Dazu musst du wahrscheinlich [Authentifizierung](https://swagger.io/docs/specification/authentication/) (zur Verwaltung von Spieleridentitäten), eine [NoSQL-Datenbank](https://azure.microsoft.com/blog/dear-documentdb-customers-welcome-to-azure-cosmos-db/) (zur Nachverfolgung von Spielen und Spielern) und einige grundlegende [Komponententests](https://apigee.com/about/blog/api-technology/swagger-test-templates-test-your-apis) für deine API hinzufügen.

Hier sind einige nützliche Ressourcen für weiterführende Schritte:

 - [Erweitertes Node.js-Debugging mit Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Dokumente für Azure Web + Mobil](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Azure DocumentDB-Dokumente](https://azure.microsoft.com/blog/dear-documentdb-customers-welcome-to-azure-cosmos-db/)

## <a name="part-ii-build-a-single-page-web-application"></a>Teil II: Erstellen einer Single-Page-Web-App

Nachdem du das [REST-API-Back-End](#part-i-build-a-rest-api-backend) aus Teil I erstellt (oder [heruntergeladen](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) hast, kannst du nun das Einzelseiten-Gedächtnisspiel-Front-End mit [Node](https://nodejs.org/en/), [Express](https://expressjs.com/) und [Bootstrap](https://getbootstrap.com/) erstellen.

In Teil II dieses Tutorials machst du dich mit Folgendem vertraut: 

* [Node.js](https://nodejs.org/en/): zum Erstellen des Servers, der dein Spiel hostet
* [jQuery](https://jquery.com/): eine JavaScript-Bibliothek
* [Express](https://expressjs.com/): für das Webanwendungsframework
* [Pug](https://pugjs.org/): (ehemals Jade) für das Vorlagenmodul
* [Bootstrap](https://getbootstrap.com/): für das dynamische Layout
* [Visual Studio Code](https://code.visualstudio.com/): zum Schreiben von Code, Anzeigen des Markdowns und Debuggen

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Erstellen einer Node.js-Anwendung mit Express

Beginnen wir mit der Erstellung des Node.js-Projekts mithilfe von Express.

1. Öffne ein Eingabeaufforderungsfenster, und navigiere zu dem Verzeichnis, in dem du dein Spiel speichern möchtest. 
2. Verwende den Express-Generator, um mithilfe des Vorlagenmoduls *Pug* eine neue Anwendung mit dem Namen *memory* zu erstellen.

    ```
    express --view=pug memory
    ```

3. Installiere im Verzeichnis „memory“ die in der Datei „package.json“ aufgeführten Abhängigkeiten. Die Datei „package.json“ wird im Stamm des Projekts erstellt. Diese Datei enthält die für deine Node.js-App erforderlichen Module.  

    ```
    cd memory
    npm install
    ```

    Nach dem Ausführen dieses Befehls sollte dir ein Ordner namens „node_modules“ angezeigt werden, der alle für diese App benötigten Module enthält. 

4. Führe nun deine Anwendung aus.

    ```
    npm start
    ```

5. Rufe [https://localhost:3000/](https://localhost:3000/) auf, um deine Anwendung anzuzeigen.

    ![Screenshot von http://localhost:3000/](./images/express.png)

6. Ändere den Standardtitel deiner Web-App, indem du „index.js“ im Verzeichnis „memory\routes“ bearbeitest. Ändere `Express` in der unten stehenden Zeile in `Memory Game` (oder einen anderen Titel deiner Wahl).

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. Wenn du die App zum Anzeigen des neuen Titels aktualisieren möchtest, beende die App, indem du **STRG+C**, **Y** in der Eingabeaufforderung drückst. Starte sie anschließend mit `npm start` neu.

### <a name="2-add-client-side-game-logic-code"></a>2. Hinzufügen von clientseitigem Spiellogikcode
Die für diesen Teil des Tutorials erforderlichen Dateien findest du im Ordner [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start). Der fertige Code ist bei Bedarf im Ordner [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final) verfügbar. 

1. Kopiere die Datei „scripts.js“ im Ordner [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start), und füge sie in „memory\public\javascripts“ ein. Diese Datei enthält die gesamte Spiellogik, die zum Ausführen des Spiels erforderlich ist. Hierzu zählen u. a. folgende Schritte:

    * Erstellen/Starten eines neuen Spiels
    * Wiederherstellen eines auf dem Server gespeicherten Spiels
    * Zeichnen des Spiels und der Karten basierend auf der Benutzerauswahl
    * Aufdecken der Karten

2. Ändern wir in „script.js“ zunächst die Funktion `newGame()`. Mit dieser Funktion werden folgende Aktionen ausgeführt:

    * Verarbeiten der Größe der Spielauswahl des Benutzers
    * Abrufen des [gameboard-Arrays](#part-i-build-a-rest-api-backend) vom Server
    * Aufrufen der Funktion `drawGameBoard()`, um das Spiel auf dem Bildschirm zu platzieren

    Füge den folgenden Code innerhalb von `newGame()` nach dem Kommentar `// Add code from Part 2.2 here` ein.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    Dieser Code ruft den Wert aus dem Dropdownmenü mit `id="selectGameSize"` (wird später erstellt) ab und speichert ihn in einer Variablen (`size`).  Wir verwenden die Funktion [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) zum Analysieren des Zeichenfolgenwerts aus der Dropdownliste, um eine ganze Zahl zurückzugeben, damit wir das `size`-Element des angeforderten Spiels an den Server übermitteln können. 

    Wir verwenden die in Teil I dieses Tutorials erstellte Methode [`/new`](#part-i-build-a-rest-api-backend), um die ausgewählte Größe des Spiels an den Server zu übermitteln. Die Methode gibt ein einzelnes JSON-Array von Karten sowie `true/false`-Werte zurück, die angeben, ob die Karten zugeordnet wurden. 

3. Fülle als Nächstes die Funktion `restoreGame()` aus, die das zuletzt gespielte Spiel wiederherstellt. Der Einfachheit halber lädt die App immer das zuletzt gespielte Spiel. Wenn kein Spiel auf dem Server gespeichert ist, starte mithilfe des Dropdownmenüs ein neues Spiel. 

    Kopiere diesen Code, und füge ihn in `restoreGame()` ein.

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    Das Spiel ruft nun den Spielzustand vom Server ab. Weitere Informationen zu der in diesem Schritt verwendeten Methode [`/game`](#part-i-build-a-rest-api-backend) findest du in Teil I dieses Tutorials. Ersetze bei Verwendung von Azure (oder eines anderen Diensts) zum Hosten der Back-End-API die *localhost*-Adresse oben durch deine Produktions-URL.

4. Jetzt erstellen wir die Funktion `drawGameBoard()`.  Mit dieser Funktion werden folgende Aktionen ausgeführt:

    * Ermitteln der Größe des Spiels und Anwenden bestimmter CSS-Formatvorlagen
    * Generieren der Karten in HTML
    * Hinzufügen der Karten zur Seite

    Kopiere diesen Code, und füge ihn in die `drawGameBoard()`-Funktion in *scripts.js* ein:

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. Als Nächstes müssen wir die `flipCard()`-Funktion fertig stellen.  Diese Funktion verarbeitet den Großteil der Spiellogik. Unter anderem ruft sie mithilfe der in Teil I des Tutorials entwickelten Methode [`/guess`](#part-i-build-a-rest-api-backend) die Werte der ausgewählten Karten vom Server ab. Vergiss nicht, die *localhost*-Adresse durch deine Produktions-URL zu ersetzen, wenn du das-REST-API-Back-End in der Cloud hostest.

    Heb in der Funktion `flipCard()` die Auskommentierung des folgenden Codes auf:

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> Wähle bei Verwendung von Visual Studio Code alle Codezeilen aus, deren Auskommentierung du aufheben möchtest, und drücke STRG+K, U.

Hier verwenden wir [`jQuery.ajax()`](https://api.jquery.com/jQuery.ajax/) und die **PUT**-Methode [`/guess`](#part-i-build-a-rest-api-backend), die wir in Teil I erstellt haben. 

Dieser Code wird in der folgenden Reihenfolge ausgeführt:

* Das `id`-Element der ersten vom Benutzer ausgewählten Karte wird als erster Wert zum Array „selectedCards[]“ hinzugefügt: `selectedCards[0]`. 
* Der Wert (`id`) in `selectedCards[0]` wird mithilfe der Methode [`/guess`](#part-i-build-a-rest-api-backend) an den Server gesendet.
* Der Server antwortet mit dem `value`-Element dieser Karte (eine ganze Zahl).
* Ein [Bootstrap-Glyphicon](https://getbootstrap.com/components/) wird der Rückseite der Karte hinzugefügt, deren `id`-Element `selectedCards[0]` lautet.
* Das `value`-Element der ersten Karte (vom Server) ist im Array `selectedCardsValues[]` gespeichert: `selectedCardsValues[0]`. 

Der zweite Versuch des Benutzers folgt derselben Logik. Wenn die vom Benutzer ausgewählten Karten die gleichen IDs besitzen (z. B. `selectedCards[0] == selectedCards[1]`), stimmen die Karten überein. Die CSS-Klasse `.matched` wird den übereinstimmenden Karten hinzugefügt (wodurch sie grün werden), und die Karten bleiben aufgedeckt.

Nun müssen wir Logik hinzufügen, um zu überprüfen, ob der Benutzer alle übereinstimmenden Karten gefunden und das Spiel gewonnen hat. Füge in der Funktion `flipCard()` die folgenden Codezeilen unter dem Kommentar `//check if the user won the game` hinzu. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

Wenn die Anzahl der aufgedeckten Karten der Größe des Spiels entspricht (z. B. `cardsFlipped == gameBoardSize`), gibt es keine weiteren aufzudeckenden Karten, und der Benutzer hat das Spiel gewonnen. Wir fügen `div` einige einfache HTML-Codezeilen mit `id="game-board"` hinzu, damit der Benutzer weiß, dass er gewonnen hat und erneut spielen kann.  

### <a name="3-create-the-user-interface"></a>3. Erstellen der Benutzeroberfläche 
Jetzt sehen wir uns diesen Code in Aktion an, indem wir die Benutzeroberfläche erstellen. In diesem Tutorial verwenden wir das Vorlagenmodul [Pug](https://pugjs.org/) (ehemals Jade).  *Pug* ist eine saubere Syntax zum Schreiben von HTML-Code mit Berücksichtigung von Leerzeichen. Hier sehen Sie ein Beispiel. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

wird zu

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Ersetze die Datei „layout.pug“ in „memory\views“ durch die im Ordner „Start“ bereitgestellte Datei „layout.pug“. „layout.pug“ enthält Links zu folgenden Komponenten:

    * Bootstrap
    * jQuery
    * eine benutzerdefinierte CSS-Datei
    * die JavaScript-Datei, die wir gerade geändert haben

2. Öffne die Datei mit dem Namen „index.pug“ im Verzeichnis „memory\views“.
Diese Datei erweitert die Datei „layout.pug“ und wird unser Spiel rendern. Füge in „layout.pug“ die folgenden Codezeilen ein:

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> Beachte Folgendes: Pug berücksichtigt Leerzeichen. Stelle sicher, dass alle Einzüge korrekt sind.

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. Verwenden des Bootstrap-Rastersystems zum Erstellen eines dynamischen Layouts
Das [Rastersystem](https://getbootstrap.com/css/#grid) von Bootstrap ist ein dynamisches Rastersystem, das ein Raster bei der Änderung eines Geräte-Viewports skaliert. Die Karten in diesem Spiel verwenden u. a. folgende vordefinierte Bootstrap-Rastersystemklassen für das Layout:
* `.container-fluid`: Gibt den dynamischen Container für das Raster an.
* `.row-fluid`: Gibt die dynamischen Zeilen an.
* `.col-xs-3`: Gibt die Anzahl von Spalten an.

Mit dem Rastersystem von Bootstrap kann ein Rastersystem auf eine vertikale Spalte reduziert werden, wie es bei einem Navigationsmenü eines mobilen Geräts der Fall ist.  Da unser Spiel jedoch immer Spalten haben soll, verwenden wir die vordefinierte Klasse `.col-xs-3`, mit der das Raster jederzeit horizontal ausgerichtet bleibt. 

Das Rastersystem kann bis zu zwölf Spalten enthalten. Da wir für unser Spiel nur vier Spalten benötigen, verwenden wir die Klasse `.col-xs-3`. Diese Klasse gibt an, dass sich unsere Spalten jeweils über drei der zwölf zuvor erwähnten verfügbaren Spalten erstrecken müssen. Die folgende Abbildung zeigt ein Raster mit zwölf Spalten und ein Raster mit vier Spalten, wie es in diesem Spiel verwendet wird.

![Bootstrap-Raster mit zwölf Spalten und vier Spalten](./images/grid.png)

1. Öffne „scripts.js“, und suche die Funktion `drawGameBoard()`.  Findest du im Codeblock, in dem die HTML für die einzelnen Karten generiert wird, das `div`-Element mit `class="col-xs-3"`? 

2. Füge in „index.pug“ die zuvor erwähnten vordefinierten Bootstrap-Klassen hinzu, um das dynamische Layout zu erstellen.  Ändere „index.pug“ wie folgt:

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. Hinzufügen einer Animation zum Aufdecken von Karten mit CSS-Transformationen
Ersetze die Datei „style.css“ in „memory\public\stylesheets“ durch die Datei „style.css“ im Ordner „Start“.

Durch das Hinzufügen einer Aufdeckbewegung mit [CSS-Transformationen](https://developer.mozilla.org/docs/Web/CSS/CSS_Transforms) wird das Aufdecken deiner Karten realistisch in 3D dargestellt. Die Karten im Spiel werden unter Verwendung der folgenden HTML-Struktur erstellt und dem Spiel programmgesteuert hinzugefügt (in der zuvor gezeigten Funktion `drawGameBoard()`).

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. Lege für den übergeordneten Container der Animation (`.flipContainer`) zunächst [Perspektive](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) fest.  Dadurch entsteht für seine untergeordneten Elemente die Illusion von Tiefe: je höher der Wert, desto weiter weg erscheint dem Benutzer das Element. Nun fügen wir in „style.css“ der `.flipContainer`-Klasse die folgende Perspektive hinzu.

    ``` css
    perspective: 1000px; 
    ```

2. Füge jetzt der `.cards`-Klasse in „style.css“ die folgenden Eigenschaften hinzu. `.cards` `div` ist das Element, das die Aufdeckanimation tatsächlich ausführt. Dabei wird entweder die Vorder- oder die Rückseite der Karte angezeigt. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    Die Eigenschaft [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) erstellt einen 3D-Rendering-Kontext, und die untergeordneten Elemente der Klasse `.cards` (`.front` und `.back`) sind Mitglieder des 3D-Raums. Durch Hinzufügen der Eigenschaft [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) wird die Anzahl der Sekunden für die Ausführung der Animation angegeben. 

3.  Mit der Eigenschaft [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) Eigenschaft können wir die Karte um die y-Achse drehen.  Füge `cards.flip` das folgende CSS hinzu:

    ``` css
    transform: rotateY(180deg);
    ```

    Der in `cards.flip` definierte Stil wird in der `flipCard`-Funktion mit [`.toggleClass()`](https://api.jquery.com/toggleClass/) ein- und ausgeschaltet. 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    Wenn ein Benutzer nun auf eine Karte klickt, wird die Karte um 180 Grad gedreht.

### <a name="6-test-and-play"></a>6. Testen und Spielen
Gratulation! Du hast die Web-App erfolgreich erstellt. Testen wir sie nun. 

1. Öffne eine Eingabeaufforderung im Arbeitsspeicherverzeichnis, und gib den folgenden Befehl ein: `npm start`.

2. Rufe in deinem Browser [https://localhost:3000/](https://localhost:3000/) auf, und spiele ein Spiel.

3. Wenn Fehler auftreten, kannst du die Node.js-Debugtools von Visual Studio Code verwenden. Drücke dazu auf der Tastatur F5, und gib `Node.js` ein. Weitere Informationen zum Debuggen in Visual Studio Code findest du in [diesem Artikel](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations). 

    Du kannst deinen Code auch mit dem im Ordner „Final“ zur Verfügung gestellten Code vergleichen.

4. Drücke zum Beenden des Spiels an der Eingabeaufforderung die folgende Tastenkombination: **STRG+C**, **Y**. 

### <a name="going-further"></a>Vertiefung

Du kannst deine App jetzt in Azure (oder in einem anderen Cloudhostingdienst) zum Testen auf verschiedenen Geräteformfaktoren (etwa Mobilgeräten, Tablets und Desktops) bereitstellen. (Vergiss nicht, sie auch in verschiedenen Browsern zu testen!) Sobald deine App für die Produktion bereit ist, kannst du sie ganz einfach als *gehostete Web-App* (Hosted Web App, HWA) für die *universelle Windows-Plattform* (UWP) verpacken und über den Microsoft Store verteilen.

Hier siehst du die grundlegenden Schritte für die Veröffentlichung im Microsoft Store:

 1. Erstellen eines [Windows-Entwicklerkontos](https://developer.microsoft.com/en-us/store/register)
 2. Verwenden der [Prüfliste](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions) für die App-Übermittlung
 3. Übermitteln deiner App für die [Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process)

Hier sind einige nützliche Ressourcen für weiterführende Schritte:

 - [Bereitstellen deines Anwendungsentwicklungsprojekts für Azure-Websites](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [Konvertieren deiner Webanwendung in eine App für die universelle Windows-Plattform (UWP)](https://docs.microsoft.com/microsoft-edge/progressive-web-apps)

 - [Veröffentlichen von Windows-Apps](https://docs.microsoft.com/windows/uwp/publish/)
