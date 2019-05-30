---
title: Erstellen einer Web-App mit einer einzelnen Seite mit REST API-Backend
description: Verwenden Sie beliebte Webtechnologien zum Erstellen einer gehosteten Web-App für den Microsoft Store
keywords: Gehostete Web-App, HWA, REST-API, Einzelseiten-App, SPA
ms.date: 05/10/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c38a7182cd27abcfb0de66c721f0e06b95b695d5
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366984"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>Erstellen einer Web-App mit einer einzelnen Seite mit REST API-Backend

**Gehostete Web-App für den Microsoft Store mit beliebten Fullstack webtechnologien erstellen.**

![Einfaches Arbeitsspeicherspiel als Einzelseiten-Web-App](images/fullstack.png)

Dieses zweiteilige Lernprogramm bietet einen kurzen Überblick über die moderne Fullstack-Webentwicklung, indem Sie ein einfaches Arbeitsspeicherspiel erstellen, das sowohl im Browser als auch als eine gehostete Web-App für den Microsoft Store funktioniert. In Teil I erstellen Sie einen einfachen REST-API-Dienst für das Spiel-Back-End. Durch das Hosten der Spiellogik in der Cloud als ein API-Dienst wird der Spielstand beibehalten, damit Ihre Benutzer ihre Spielinstanzen auf verschiedenen Geräten weiterspielen können. In Teil II werden Sie die Front-End-Benutzeroberfläche als eine Einzelseiten-Web-App mit dynamischem Layout erstellen.

Wir werden einige der beliebtesten Webtechnologien verwenden, einschließlich des [Node.js](https://nodejs.org/en/) Runtime und [Express](https://expressjs.com/) für serverseitige Entwicklung, das [Bootstrap](https://getbootstrap.com/)-UI-Framework, das [Pug](https://www.npmjs.com/package/pug)-Vorlagenmodul und [Swagger](https://swagger.io/tools/) zum Erstellen von RESTful-APIs. Sie werden auch Erfahrung mit dem [Azure-Portal](https://ms.portal.azure.com/) für das Cloud-Hosting sammeln, und mit dem [Visual Studio Code](https://code.visualstudio.com/)-Editor arbeiten.

## <a name="prerequisites"></a>Vorraussetzungen

Wenn Sie diese Ressourcen nicht bereits auf dem Computer besitzen, folgen Sie den folgenden Downloadlinks:

 - [Node.js](https://nodejs.org/en/download/) – Stellen Sie sicher, dass Sie die Option für das Hinzufügen eines Knotens zu Ihrem PATH ausgewählt haben.

 - [Express Generator](https://expressjs.com/en/starter/generator.html)– nach der Installation von Knoten, Installieren von Express mit `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

Wenn Sie die endgültigen Schritte zum Hosten Ihres API-Diensts und Ihrer Arbeitsspeicherspiele-App auf Microsoft Azure abschließen möchten, müssen [Sie ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/en-us/free/), wenn Sie dies noch nicht getan haben.

Wenn Sie den Azure-Teil beenden (oder verschieben) möchten, überspringen Sie einfach die endgültigen Abschnitte der Teile I und II, die das Azure-Hosten und Verpacken Ihrer App für den Microsoft Store abdecken. Der API-Dienst und Web-App, die Sie erstellen, wird weiterhin lokal (entsprechend aus `http://localhost:8000` und `http://localhost:3000`) auf Ihrem Computer ausgeführt.

## <a name="part-i-build-a-rest-api-backend"></a>Teil I: Erstellen eines REST-API-Back-Ends

Zunächst erstellen wir eine einfache Speicherspiel-API, um unsere Arbeitsspeicherspiele-Web-App anzutreiben. Wir verwenden [Swagger](https://swagger.io/), um unsere API zu definieren und Gerüst-Code und eine Web-Benutzeroberfläche für das manuelle Testen zu generieren.

Wenn Sie möchten, überspringen Sie diesen Teil, und gehen direkt in [Teil II: Erstellen einer Single-Page-Webanwendung](#part-ii-build-a-single-page-web-application), hier ist die [Code Teil I beendet](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend). Führen Sie die *Infodatei* Anweisungen zum Abrufen der Programmieren und lokal ausgeführt oder finden Sie unter *5. Hosten Sie Ihren API-Dienst in Azure, und aktivieren Sie CORS* für die Ausführung von Azure.

### <a name="game-overview"></a>Spielübersicht

*Arbeitsspeicher* (auch bekannt als [*Konzentration*](https://en.wikipedia.org/wiki/Concentration_(game)) und [*Pelmanism*](https://en.wikipedia.org/wiki/Pelmanism_(system))), ist ein einfaches Spiel aus einem Kartenspiel mit Kartenpaaren. Die Karten werden verdeckt auf den Tisch platziert. Der Spieler prüft die Kartenwerte, zwei Karten gleichzeitig, und sucht nach Übereinstimmungen. Nach jeder Runde werden die Karten erneut verdeckt auf den Tisch gelegt, sofern kein übereinstimmendes Paar gefunden wurde. Wird ein Paar gefunden, werden die zwei Karten aus dem Spiel entfernt. Das Ziel des Spiels ist es, alle Kartenpaare in so wenig Runden wie möglich zu finden.

Die von uns verwendete Spielstruktur ist zur Veranschaulichung sehr einfach: ein einziges Spiel mit einem Spieler. Die Spiellogik erfolgt jedoch zur Bewahrung des Spielstands serverseitig (und nicht über den Client), sodass Sie dasselbe Spiel auf verschiedenen Geräten weiterspielen können.

Die Datenstruktur für ein Arbeitsspeicherspiel besteht einfach aus einem Array von JavaScript-Objekten, die jeweils eine einzelne Karte darstellen. Die Indizes im Array fungieren als Karten-IDs. Auf dem Server hat jedes Kartenobjekt einen Wert und ein **deaktiviert**-Kennzeichen. Ein Brett mit 2 Übereinstimmungen (4 Karten) können nach dem Zufallsprinzip generiert und wie folgt serialisiert werden.

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

Wenn das Brett-Array an den Client übergeben wurde, werden die Werteschlüssel aus dem Array entfernt, um Täuschungen zu verhindern (z. B. Untersuchen des HTTP-Antworttexts mit Browser-Entwicklertools (F12)). So würde das gleiche neue Spiel für einen Client aussehen, der den **GET /game**-REST-Endpunkt aufruft:

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

Apropos Endpunkte, der Arbeitsspeicherspiel-Dienst wird aus drei REST-APIs bestehen.

#### <a name="post-new"></a>POST /new
Initialisiert eine neues Spielbrett in der angegebenen Größe (# der Übereinstimmungen).

| Parameter | Beschreibung |
|-----------|-------------|
| Int *Größe* |Die Anzahl der übereinstimmenden Paare, die in das „Spielbrett” gemischt wird. Beispiel: `http://memorygameapisample/new?size=2`|

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Neues Speicherspiel in der angeforderten Größe ist bereit.|
| 400 BAD REQUEST| Angeforderte Größe befindet sich außerhalb des zulässigen Bereichs.|


#### <a name="get-game"></a>GET /game
Ruft den aktuellen Zustand des Arbeitsspeicherspiels ab.

*Ohne Parameter*

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Gibt JSON-Array mit Kartenobjekten zurück. Jede Karte hat eine **deaktiviert**-Eigenschaft, die angibt, ob die Übereinstimmung bereits gefunden wurde. Übereinstimmende Karten melden auch ihren **Wert**. Beispiel: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
Gibt eine Karte an, die aufgedeckt werden soll, und prüft nach einer Übereinstimmung mit der zuvor aufgedeckten Karte.

| Parameter | Beschreibung |
|-----------|-------------|
| Int *Karte* | Karten-ID (Index im Array des Spielbretts) der Karte zum Aufdecken. Jede abgeschlossene „Vermutung” besteht aus zwei angegebenen Karten (z. B. zwei Aufrufe für **/erraten** mit gültigen und eindeutigen *Karten*-Werten). Beispiel: `http://memorygameapisample/guess?card=0`|

| Antwort | Beschreibung |
|----------|-------------|
| 200 OK | Gibt JSON mit gibt der **ID** und **Wert** der angegebenen Karte zurück. Beispiel: `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  Fehler bei der angegebenen Karte. Weitere Details finden Sie im HTTP-Antworttext.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. Den API-Spezifikation und Codestubs generieren

Wir verwenden [Swagger](https://swagger.io/), um den Entwurf unseres Speicherspiel-APIs in funktionieRendern Node.js-Server-Code umzuwandeln. Hier erfahren Sie, wie Sie unsere [Speicherspiel-APIs als Swagger-Metadaten](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json) definieren können. Wir werden dies benutzen, um Server-Code-Stubs zu generieren.

1. Erstellen Sie (z. B. in Ihrem lokalen *GitHub*-Verzeichnis) einen neuen Ordner, und laden die Datei [**API.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) herunter, die unsere Speicherspiel-API-Definitionen enthält. Stellen Sie sicher, dass der Ordnername keine Leerzeichen enthält.

2. Öffnen Sie Ihre Lieblings-Shell ([, oder verwenden Sie das integrierte Terminal von Visual Studio Code!](https://code.visualstudio.com/docs/editor/integrated-terminal)) in dem Ordner, und führen Sie folgenden Knoten-Paket-Manager-Befehl (NPM-Befehl) aus, um das [Yeoman](https://yeoman.io/)-Code-Gerüst-Tool (yo) und Swagger-Generator für die globale ( **-g**) Knotenumgebung zu installieren:

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. Jetzt können wir den Gerüst-Servercode mit Swagger generieren:

    ```
    yo swaggerize
    ```

4. Der **swaggerize**-Befehl wird Sie einige Fragen stellen.
    - Pfad (oder URL) zum Swagger-Dokument: **API.json**
    - Framework: **express**
    - Wie möchten Sie dieses Projekt (YourFolderNameHere) nennen: **[EINGABETASTE]**

    Beantworten Sie alles andere, wie Sie möchten; die Informationen sind hauptsächlich zum Angeben Ihrer Kontaktinformationen in der *package.json*-Datei, damit Sie Ihren Code als NPM-Paket verteilen können.

5. Zum Schluss installieren Sie alle Abhängigkeiten (gemäß der Liste in *package.json*) für Ihr neues Projekt und [Swagger UI](https://swagger.io/swagger-ui/)-Unterstützung.

    ```
    npm install
    npm install swaggerize-ui
    ```

    Starten Sie nun den VS-Code und **Datei** > **Ordner öffnen...** , und wechseln Sie zum Verzeichnis MemoryGameAPI. Dies ist der Node.js-API-Server, den Sie gerade erstellt haben! Er verwendet das beliebte [ExpressJS](https://expressjs.com/en/4x/api.html)-Web-Anwendungsframework zum Strukturieren und Ausführung Ihres Projekts.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. Anpassen der Servercode und das Setup zu debuggen

Die *Server.js*-Datei in Ihrem Projektstammverzeichnis fungiert als die „Hauptfunktion” des Servers. Öffnen Sie sie im VS-Code, und fügen Sie Folgendes ein. Die vom generierten Code geänderten Zeilen sind mit weiteren Erläuterung versehen.

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

Damit ist es Zeit, den Server auszuführen! Richten wir nun Visual Studio Code für das Debuggen von Knoten ein. Wählen Sie auf dem **Debug** das Steuersymbol (STRG + UMSCHALT+D) und dann das Zahnradsymbol (Open launch.json) und ändern Sie „Konfigurationen” auf Folgendes:

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

Drücken Sie nun F5, und öffnen Sie Ihren Browser zu [https://localhost:8000](https://localhost:8000). Die Seite sollte zur Swagger-Benutzeroberfläche unserer Speicherspiel-API geöffnet werden, und von dort aus können Sie die Details und die Eingabefelder für die einzelnen Methoden erweitern. Sie können sogar versuchen, die APIs aufzurufen. Deren Antworten werden jedoch nur Pseudo-Daten enthalten (bereitgestellt durch das [Swagmock](https://www.npmjs.com/package/swagmock)-Modul). Es ist Zeit, unsere Spiellogik hinzuzufügen, um diese APIs real zu machen.

### <a name="3-set-up-your-route-handlers"></a>3. Richten Sie die Routenhandler

Die Swagger-Datei (config\swagger.json) weist unseren Server an, wie verschiedene HTTP-Clientanforderungen durch die Zuordnung der einzelnen von ihm festgelegten URL-Pfade zu einer Handler-Datei (in \handlers) behandelt werden, und jede für den Pfad definierte Methode (z. B. **abrufen**, **POST**) zu einer `operationId` (Funktion) in der Handler-Datei.

In dieser Ebene unsere Programms werden wir Eingabeüberprüfungen hinzufügen, bevor wir die verschiedenen Clientanforderungen an unser Datenmodell übergeben. Herunterladen (oder kopieren und einfügen):

 - Dieser [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D)-Code für Ihre **handlers\game.js**-Datei
 - Dieser [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D)-Code für Ihre **handlers\guess.js**-Datei
 - Dieser [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D)-Code für Ihre **handlers\new.js**-Datei

 Sie können die Kommentare in diesen Dateien für weitere Details zu den Änderungen überfliegen, aber im Wesentlichen prüfen sie nach grundlegenden Eingabefehlern (z. B. der Client fordert ein neues Spiel mit weniger als eine Übereinstimmung an) und senden bei Bedarf aussagekräftige Fehlermeldungen. Die Handler leiten außerdem gültige Clientanforderungen an die entsprechenden Datendateien (in \data) zur weiteren Verarbeitung weiter. Lassen Sie uns nun mit diesen Dateien weiterarbeiten.

### <a name="4-set-up-your-data-model"></a>4. Richten Sie Ihr Datenmodell

Es ist Zeit, den Platzhalter für den imitieRendern Dienst mit einem echten Datenmodell für unser Speicherspielbrett auszutauschen.

Diese Ebene unseres Programms stellt die Speicherkarten selbst dar und bietet den Großteil der Spiellogik, einschließlich das Mischen des Kartenstapel für ein neues Spiel, Erkennen von übereinstimmenden Paaren und das Verwalten des Spielzustands. Kopieren und Einfügen:

 - Dieser [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D)-Code für Ihre **data\game.js**-Datei
 - Dieser [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D)-Code für Ihre **data\guess.js**-Datei
 - Dieser [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D)-Code für Ihre **data\new.js**-Datei

Der Einfachheit halber speichern wir unser Spielbrett in einer globalen Variable (`global.board`) auf dem Serverknoten. Aber realistisch gesehen würden Sie Cloud-Speicher (z. B. Google [Cloud-Datenspeicher](https://cloud.google.com/datastore/) oder Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/)) verwenden, um die Funktion in eine geeignete Speicherspiel-API-Dienst zu verwandeln, der gleichzeitig mehrere Spiele und Spieler unterstützt.

Stellen Sie sicher, dass Sie alle Änderungen in VS-Code gespeichert haben. Führen Sie den Server erneut aus (F5 in VS-Code oder `npm start` von Shell, und navigieren Sie dann zu [https://localhost:8000](https://localhost:8000)), um die Spiele-APIs zu testen.

Jedes Mal beim Drücken der **Probieren Sie es aus!** Schaltfläche auf einem der **/Spiele**-, **/erraten**-, oder **/ neue** Vorgänge, überprüfen Sie den resultieRendern **Antworttext** und **Antwortcode** unten, um sicherzustellen, dass alles wie erwartet funktioniert.

Versuchen Sie: 

1. Ein neues `size=2` Spiel zu erstellen.

    ![Ein neues Speicherspiel von der Swagger-Benutzeroberfläche zu starten.](images/swagger_new.png)

2. Ein paar Werte zu erraten.

    ![Eine Karte auf der Swagger-Benutzeroberfläche zu erraten.](images/swagger_guess.png)

3. Das Spielbrett während des Spiels zu überprüfen.

    ![Aus der Swagger-Benutzeroberfläche den Spielzustand zu überprüfen.](images/swagger_game.png)

Wenn alles gut aussieht, kann Ihr API-Dienst auf Azure gehostet werden! Wenn Probleme auftreten, versuchen Sie, mit den folgenden Zeilen in \data\game.js zu kommentieren.

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

Mit dieser Änderung wird die **GET /game**-Methode alle Kartenwerte (einschließlich diejenigen, die noch nicht gelöscht wurden) zurückgeben. Hierbei handelt es sich um einen hilfreichen Debug-Hack, den Sie beim Erstellen des Front-Ends Ihres Spiels aufbewahren können.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (Optional) Hosten Sie Ihren API-Dienst in Azure, und Aktivieren von CORS

Mit der Azure-Dokumentation begleitet Sie beim:

 - [Registrieren eines neuen *API-App* mit Azure-Portal](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [Einrichten von Git-Bereitstellung für Ihre API-App](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git), und
 - [Bereitstellen des API-app-Codes in Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

Versuchen Sie bei der Registrierung Ihrer App Ihren *App-Namen* zu differenzieren (zum Vermeiden von Überschneidungen mit anderen, die Varianten unter *http://memorygameapi.azurewebsites.net* URL anfordern).

Wenn sie es bis hier hin geschafft haben und Azure jetzt die Swagger-Benutzeroberfläche anzeigt, gibt nur noch einen letzten Schritt für das Arbeitsspeicherspiel-Back-End. Von [Azure-Portal](https://portal.azure.com), wählen Sie den neu erstellten *App-Dienst* und dann wählen oder suchen Sie nach der **CORS**-Option (Cross-Origin Resource Sharing). Fügen Sie unter **Zulässige Ursprünge** ein Sternchen (`*`) hinzu und klicken Sie auf **Speichern**. Auf diese Weise können Sie ursprungsübergreifende Aufrufe Ihrer API-Dienste aus dem Arbeitsspeicherspiel-Front-End während der Entwicklung auf dem lokalen Computer durchführen. Nach dem Abschließen des Speicherspiel-Front-Ends und das Bereitstellen auf Azure können Sie diesen Eintrag mit der bestimmten URL Ihrer Web-App ersetzen.

### <a name="going-further"></a>Vertiefung

Um die Speicherspiel-API als einen geeigneten Back-End-Dienst für eine Produktions-App bereitzustellen, sollten Sie den Code zur Unterstützung von mehreren Spielern und Spiele erweitern. Dazu müssen Sie wahrscheinlich die [Authentifizierung](https://swagger.io/docs/specification/authentication/) (für die Verwaltung von Spieleridentitäten), eine [NoSQL-Datenbank](https://docs.microsoft.com/en-us/azure/documentdb/) (für die Verfolgung von Spielen und Spieler), und einige grundlegenden [Unittest](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis) für Ihre API gruppieren.

Hier sind einige nützliche Ressourcen für die weiterfühRendern Schritte:

 - [Erweiterte Node.js Debuggen mit Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Azure-Web + Mobile-Dokumentation](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Azure DocumentDB-Dokumentation](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>Teil II: Erstellen einer Single-Page-Webanwendung

Nun, da Sie mit dem Erstellen (oder [Herunterladen](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) des [REST-API-Back-Ends](#part-i-build-a-rest-api-backend) aus Teil I fertig sind, sind Sie bereit, um das Einzelseiten-Speicherspiel-Front-End mit [Knoten](https://nodejs.org/en/), [Express](https://expressjs.com/), und [Bootstrap](https://getbootstrap.com/) zu erstellen.

Teil II dieses Lernprogramms wird Ihnen folgende Kenntnisse übermitteln: 

* [Node.js](https://nodejs.org/en/): Für das Erstellen des Servers, der Ihr Spiel hostet
* [jQuery](https://jquery.com/): eine JavaScript-Bibliothek
* [Express](https://expressjs.com/): für das Web-Anwendungsframework
* [Pug](https://pugjs.org/): (ehemals Jade) für das Vorlagenmodul
* [Bootstrap](https://getbootstrap.com/): für das dynamische Layout
* [Visual Studio Code](https://code.visualstudio.com/): Für das Schreiben von Code, Anzeigen des Markdowns und das Debugging

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Erstellen Sie eine Node.js-Anwendung mithilfe von Express

Beginnen wir mit der Erstellung des Node.js-Projekts mithilfe von Express.

1. Öffnen Sie ein Eingabeaufforderungsfenster und navigieren Sie zu dem Verzeichnis, in dem Sie Ihr Spiel speichern möchten. 
2. Verwenden Sie den Express-Generator Zum Erstellen einer neuen Anwendung namens *Speicher* mithilfe des Vorlagenmoduls *Pug*.

    ```
    express --view=pug memory
    ```

3. Installieren Sie im Arbeitsspeicherverzeichnis die in der Datei package.json aufgeführten Abhängigkeiten. Die package.json-Datei wird im Stamm des Projekts erstellt. Diese Datei enthält die Module, die für Ihre Node.js-App erforderlich sind.  

    ```
    cd memory
    npm install
    ```

    Nach dem Ausführen dieses Befehls sollte Ihnen ein Ordner namens Node_modules angezeigt werden, der alle für diese App benötigten Module enthält. 

4. Führen Sie nun Ihre Anwendung aus.

    ```
    npm start
    ```

5. Zeigen Sie Ihre Anwendung an, indem Sie zu [https://localhost:3000/](https://localhost:3000/) wechseln.

    ![Screenshot von http://localhost:3000/-Ansichten](./images/express.png)

6. Ändern Sie den Standardtitel Ihrer Web-App durch Ändern von Index.js im Verzeichnis Memory\routes. Ändern Sie `Express` in der unten stehenden Zeile zu `Memory Game` (oder einen anderen Titel Ihrer Wahl).

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. Beenden Sie Ihre App zum Aktualisieren der App und zum Anzeigen Ihres neues Titels durch Drücken von **STRG+C**, **Y** in der Befehlszeile, und starten Sie sie mit `npm start` neu.

### <a name="2-add-client-side-game-logic-code"></a>2. Fügen Sie Code für die clientseitige Spiellogik
Sie finden die Dateien, die Sie für diese Hälfte des Lernprogramms benötigen, im [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start)-Ordner. Wenn Sie sich verirren, steht der fertige Code im [endgültigen](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final) Ordner zur Verfügung. 

1. Kopieren Sie die Datei scripts.js in den [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start)-Ordner und fügen Sie ihn in memory\public\javascripts ein. Diese Datei enthält die gesamte Spiellogik, die zum Ausführen des Spiels erforderlich ist, einschließlich:

    * Erstellen/Starten eines neuen Spiels.
    * Wiederherstellen eines Spiels, das auf dem Server gespeichert ist.
    * Zeichnen des Spielbretts und der Karten basierend auf der Benutzerauswahl.
    * Umdrehen der Karten.

2. Beginnen wir in script.js mit dem Ändern der `newGame()`-Funktion. Diese Funktion:

    * Bearbeitet die Größe der Spielauswahl des Benutzers.
    * Ruft die [Gameboard-Array](#part-i-build-a-rest-api-backend) vom Server auf.
    * Ruft die `drawGameBoard()` -Funktion auf, um das Spielbrett auf dem Bildschirm anzuzeigen.

    Fügen Sie folgenden Code in `newGame()` nach dem Kommentar `// Add code from Part 2.2 here` ein.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    Dieser Code ruft den Wert aus dem Dropdownmenü mit `id="selectGameSize"` (was wir später noch erstellen) ab und speichert ihn in einer variablen (`size`).  Wir verwenden die [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)-Funktion zum Analysieren des String-Werts aus der Dropdownliste, um eine ganze Zahl zurückzugeben, damit wir die `size` des angeforderten Spiels an den Server übermitteln können. 

    Wir verwenden die [`/new`](#part-i-build-a-rest-api-backend)-Methode, die in Teil I dieses Lernprogramms erstellt wurde, um die ausgewählte Größe des Spiels an den Server zu übermitteln. Die Methode gibt ein einzelnes JSON-Array von Karten `true/false`-Werte zurück, die angeben, ob die Karten zugeordnet wurden. 

3. Füllen Sie als Nächstes die `restoreGame()`-Funktion aus, die das zuletzt gespielte Spiel wiederherstellt. Der Einfachheit halber lädt die App immer das zuletzt gespielte Spiel. Wenn kein Spiel auf dem Server gespeichert ist, verwenden Sie das Dropdown-Menü, um ein neues Spiel zu starten. 

    Kopieren und fügen Sie diesen Code in `restoreGame()` ein.

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

    Das Spiel wird nun den Spielezustand vom Server abrufen. Weitere Informationen zur [`/game`](#part-i-build-a-rest-api-backend)-Methode für diesen Schritt finden Sie in Teil I des Lernprogramms. Bei Verwendung von Azure (oder einem anderen Dienst) zum Hosten der Back-End-API, ersetzen Sie die *Localhost*-Adresse oben mit der Produktions-URL-Adresse.

4. Jetzt erstellen wir die `drawGameBoard()`-Funktion.  Diese Funktion:

    * Erfasst die Größe des Spiels und wendet bestimmte CSS-Formatvorlagen an.
    * Generiert die Karten im HTML-Code.
    * Fügt die Karten und der Seite hinzu.

    Kopieren und fügen Sie diesen Code in die `drawGameBoard()`-Funktion in *scripts.js* ein:

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

5. Als Nächstes müssen wir die `flipCard()`-Funktion abschließen.  Diese Funktion bearbeitet den Großteil der Spiellogik, einschließlich der Werte der ausgewählten Karten aus dem Server, anhand der [`/guess`](#part-i-build-a-rest-api-backend) -Methode, die in Teil I des Lernprogramms entwickelt wurde. Vergessen Sie nicht, die *Localhost*-Adresse mit Ihrer Produktions-URL-Adresse zu ersetzen, wenn Sie das-REST-API-Back-End über die Cloud hosten.

    Entfernen Sie in der `flipCard()`-Funktion das Kommentar zu diesem Code:

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
> Wenn Sie Visual Studio Code verwenden, wählen Sie alle Codezeilen aus, bei denen Sie die Kommentare löschen möchten, und drücken Sie STRG+K, U.

Hier verwenden wir [ `jQuery.ajax()` ](https://api.jquery.com/jQuery.ajax/) und **PUT** [ `/guess` ](#part-i-build-a-rest-api-backend) in Teil I erstellten Methode. 

Dieser Code wird in der folgenden Reihenfolge ausgeführt.

* Die `id` der ersten Karte vom Benutzer ausgewählten als der erste Wert in das SelectedCards []-Array hinzugefügt wird: `selectedCards[0]` 
* Der Wert (`id`) in `selectedCards[0]` wird an den Server anhand der [`/guess`](#part-i-build-a-rest-api-backend)-Methode gesendet.
* Der Server antwortet mit dem `value` der Karte (eine ganze Zahl).
* Ein [Bootstrap Glyphicon](https://getbootstrap.com/components/) wurde in den Hintergrund der Karte, deren `id` ist `selectedCards[0]`
* Der `value` der ersten Karte (vom Server) ist im `selectedCardsValues[]`-Array: `selectedCardsValues[0]` gespeichert. 

Der zweite Versuch des Benutzers folgt derselben Logik. Wenn die vom Benutzer ausgewählten Karten die gleichen IDs besitzen (z. B. `selectedCards[0] == selectedCards[1]`), stimmen die Karten überein! Die CSS-Klasse `.matched` wird den übereinstimmenden Karten hinzugefügt (wodurch sie grün werden), und die Karten bleiben aufgedeckt.

Nun müssen wir Logik hinzufügen, um zu überprüfen, ob der Benutzer alle übereinstimmenden Karten gefunden und das Spiel gewonnen hat. Fügen Sie in der `flipCard()`-Funktion die folgenden Codezeilen unter dem `//check if the user won the game`-Kommentar hinzu. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

Wenn die Anzahl der aufgedeckten Karten die Größe des Spielbretts entspricht (z. B. `cardsFlipped == gameBoardSize`), gibt keine weiteren Karten, die aufgedeckt werden können, und der Benutzer hat das Spiel gewonnen. Wir fügen einige einfache HTML-Codezeilen zur `div` hinzu mit `id="game-board"`, damit der Benutzer weiß, dass er gewonnen hat und erneut spielen kann.  

### <a name="3-create-the-user-interface"></a>3. Erstellen der Benutzeroberfläche 
Jetzt sehen wir uns diesen Code in Aktion an, indem wir die Benutzeroberfläche erstellen. In diesem Lernprogramm verwenden wir das Vorlagenmodul [Pug](https://pugjs.org/) (formell Jade).  *Pug* ist eine saubere Syntax zum Schreiben von HTML-Code, die auf Leerzeichen achtet. Hier sehen Sie ein Beispiel. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

wird

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Ersetzen Sie die Datei layout.pug in memory\views mit der bereitgestellten Datei layout.pug-Datei im Ordner „Start”. In layout.pug sehen Sie Links zu:

    * Bootstrap
    * jQuery
    * Eine benutzerdefinierte CSS-Datei
    * Die JavaScript-Datei, die wir gerade geändert haben

2. Öffnen Sie die Datei mit dem Namen index.pug im Verzeichnis memory\views.
Diese Datei erweitert die Datei layout.pug und wird unser Spiel rendern. Fügen Sie in layout.pug die folgenden Codezeilen ein:

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
> merken: Pug ist ein Leerraum vertrauliche. Stellen Sie sicher, dass alle Ihre Einzüge richtig sind!

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. Verwenden von Bootstrap-Rastersystem zu ein reaktionsfähigen Layout erstellen
Das [Rastersystem](https://getbootstrap.com/css/#grid) von Bootstrap ist ein dynamisches Rastersystem, das ein Raster beim Ändern eines Geräte-Viewports skaliert. Die Karten in diesem Spiel verwenden die vordefinierten Rastersystemklassen von Bootstrap für das Layout, einschließlich:
* `.container-fluid`: Gibt den flüssigen Container für das Raster
* `.row-fluid`: Gibt an, die fließende Zeilen
* `.col-xs-3`: Gibt die Anzahl von Spalten

Mit dem Rastersystem von Bootstrap kann ein Rastersystem in eine vertikalen Spalte zusammengeklappt werden, wie bei einem Navigationsmenü eines mobilen Geräts.  Da wir jedoch bei unserem Spiel immer Spalten haben sollen, verwenden wir die vordefinierte Klasse `.col-xs-3`, die jederzeit für ein horizontales Raster sorgt. 

Das Rastersystem ermöglicht bis zu 12 Spalten. Da wir nur 4 Spalten in unserem Spiel haben möchten, verwenden wir die Klasse `.col-xs-3`. Diese Klasse gibt an, dass die gewünschten Spalten eine Breite von 3 der 12 bereits erwähnten Spalten umfassen. Diese Abbildung zeigt ein Raster mit 12 Spalten und ein Raster mit 4 Spalten, wie es in diesem Spiel verwendet wird.

![Bootstrap-Raster mit 12 Spalten und 4 Zeilen](./images/grid.png)

1. Öffnen Sie scripts.js und suchen Sie die `drawGameBoard()`-Funktion.  Können Sie im Codeblock, in dem wir den HTML-Code für jede Karte generieren, das `div`-Element mit `class="col-xs-3"` erkennen? 

2. Fügen Sie in Index.pug die zuvor erwähnten vordefinierten Bootstrap-Klassen hinzu, um unser dynamisches Layout zu erstellen.  Ändern Sie index.pug folgendermaßen.

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

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. Hinzufügen einer Karte-Flip-Animation mit CSS-Transformationen
Ersetzen Sie die Datei style.css im Verzeichnis memory\public\stylesheets mit der Datei style.css aus dem Ordner „Start”.

Das Hinzufügen einer Flip-Bewegung mit [CSS-Transformationen](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms) verleiht den Karten eine realistische 3D-Flip-Bewegung. Die Karten im Spiel werden anhand der folgenden HTML-Struktur erstellt und dem Spielbrett programmgesteuert hinzugefügt (in der zuvor gezeigten `drawGameBoard()`-Funktion).

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. Geben Sie dem übergeordneten Container der Animation zunächst [Perspektive](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) (`.flipContainer`).  Dadurch entsteht für seine untergeordneten Elemente die Illusion von Tiefe: je höher der Wert, desto weiter Weg erscheint dem Benutzer das Element. Nun fügen wir in style.css die folgende Perspektive der `.flipContainer`-Klasse hinzu.

    ``` css
    perspective: 1000px; 
    ```

2. Fügen Sie jetzt die folgenden Eigenschaften der `.cards`-Klasse in style.css hinzu. Die `.cards` `div` ist das Element, das tatsächlich die Kipp Animation, die angezeigt wird, entweder im Vordergrund oder am Ende der Karte. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    Die [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style)-Eigenschaft erstellt ein 3D-Rendering-Kontext, und die untergeordneten Elemente der `.cards`-Klasse (`.front` und `.back`) sind Mitglieder des 3D-Raums. Das Hinzufügen der [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) -Eigenschaft gibt die Anzahl der Sekunden für die Ausführung der Animation an. 

3.  Mit der [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) -Eigenschaft können wir die Karte um die Y-Achse drehen.  Fügen Sie den `cards.flip` den folgenden CSS-Code hinzu:

    ``` css
    transform: rotateY(180deg);
    ```

    Der in `cards.flip` definierte Stil kann in der `flipCard`-Funktion mit [`.toggleClass()`](https://api.jquery.com/toggleClass/) ein- und ausgeschaltet werden. 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    Wenn ein Benutzer nun auf eine Karte klickt, wird die Karte um 180 Grad gedreht.

### <a name="6-test-and-play"></a>6. Play-Test
Herzlichen Glückwunsch! Sie haben die Web-App erfolgreich erstellt! Testen wir sie. 

1. Öffnen Sie eine Eingabeaufforderung im Verzeichnis Arbeitsspeicher aus, und geben Sie den folgenden Befehl aus: `npm start`

2. Rufen Sie in Ihrem Browser [https://localhost:3000/](https://localhost:3000/) auf und spielen Sie ein Spiel!

3. Wenn Fehler auftreten, können Sie die Node.js-Debugtools von Visual Studio Code durch Drücken von F5 auf der Tastatur und Eingeben von `Node.js` verwenden. Weitere Informationen zum Debuggen in Visual Studio Code finden Sie in diesem [Artikel](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations). 

    Sie können auch Ihren Code mit dem im letzten Ordner zur Verfügung gestellten Code vergleichen.

4. So beenden Sie das Spiel, in der Eingabeaufforderung Folgendes: **Ctrl + C**, **Y**. 

### <a name="going-further"></a>Vertiefung

Sie können jetzt Sie Ihre App in Azure (oder einen anderen Cloud-Hostingdienst) zum Testen auf verschiedenen Geräteformfaktoren, z. B. Mobile, Tablet und PC, bereitstellen. (Vergessen Sie nicht, für verschiedene Browser testen!) Sobald Ihre app für die Produktion bereit ist, Sie können problemlos Verpacken als eine *gehostete Web-App* (HWA) für die *universelle Windows-Plattform* (UWP) und verteilen Sie sie aus dem Microsoft Store.

Die grundlegenden Schritte für die Veröffentlichung auf dem Microsoft Store sind:

 1. Erstellen Sie ein Konto für [Windows-Entwickler](https://developer.microsoft.com/en-us/store/register).
 2. Verwenden Sie die [Prüfliste für die App-Übermittlung](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions).
 3. Reichen Sie Ihre App für die [Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process) ein.

Hier sind einige nützliche Ressourcen für die weiterfühRendern Schritte:

 - [Bereitstellen Sie Ihres Anwendungsentwicklungsprojekts auf Azure-Websites](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [Konvertieren Sie Ihre Webanwendung in einer app für die universelle Windows-Plattform (UWP)](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [Veröffentlichen von Windows-apps](https://developer.microsoft.com/en-us/store/publish-apps)
