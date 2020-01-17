---
title: Erstellen eines UWP-Spiels in JavaScript
description: Ein einfaches UWP-Spiel für den Microsoft Store, geschrieben in JavaScript und CreateJS
ms.date: 02/09/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: b2b60354acb2c3d97ced3dce0b3fb7f6d97ac35d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684775"
---
# <a name="create-a-uwp-game-in-javascript"></a>Erstellen eines UWP-Spiels in JavaScript

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Ein einfaches 2D-UWP-Spiel für den Microsoft Store, geschrieben in JavaScript und CreateJS


![Sprite-Blatt für laufenden Dino](images/JS2D_1.png)


## <a name="introduction"></a>Einführung


Durch das Veröffentlichen einer App im Microsoft Store kannst du sie für Millionen von Menschen auf vielen verschiedenen Geräten freigeben (oder verkaufen!).  

Um die App im Microsoft Store zu veröffentlichen, muss sie als UWP-App (Universelle Windows-Plattform) geschrieben werden. UWP ist aber extrem flexibel und unterstützt eine Vielzahl von Sprachen und Frameworks. Hier ist der Beweis: Dieses Beispiel ist ein einfaches Spiel, das in JavaScript geschrieben ist und mehrere CreateJS-Bibliotheken nutzt. Es veranschaulicht, wie Sprites gezeichnet werden, eine Spielschleife erstellt wird, Tastatur und Maus unterstützt werden und die Anpassung an verschiedene Bildschirmgrößen erfolgt.

Dieses Projekt wurde mit JavaScript unter Verwendung von Visual Studio erstellt. Mit einigen geringfügigen Änderungen kann es auch auf einer Website gehostet oder an andere Plattformen angepasst werden. 

**Hinweis:** Dies ist natürlich noch kein vollständiges (bzw. gutes) Spiel, sondern es soll lediglich zeigen, wie du mit JavaScript und einer Drittanbieterbibliothek eine App erstellen kannst, die für die Veröffentlichung im Microsoft Store bereit ist.


## <a name="requirements"></a>Anforderungen

Um dieses Projekt ausprobieren zu können, benötigst du Folgendes:

* Einen Windows-Computer (bzw. einen virtuellen Computer) mit der aktuellen Version von Windows 10.
* Eine Kopie von Visual Studio. Die kostenlose Visual Studio Community Edition kann von der [Visual Studio-Homepage](https://visualstudio.com) heruntergeladen werden.

Für dieses Projekt wird das CreateJS-JavaScript-Framework verwendet. Bei CreateJS handelt es sich um einen kostenlosen Satz von Tools mit MIT-Lizenz, mit denen die Erstellung von Sprite-basierten Spielen erleichtert werden soll. Die CreateJS-Bibliotheken sind bereits im Projekt vorhanden (suche in der Projektmappen-Explorer-Ansicht nach *js/easeljs-0.8.2.min.js* und *js/preloadjs-0.6.2.min.js*). Weitere Informationen zu CreateJS findest du auf der [CreateJS-Homepage](https://www.createjs.com).


## <a name="getting-started"></a>Erste Schritte

Der vollständige Quellcode für die App befindet sich auf [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d).

Die einfachste Möglichkeit für den Einstieg besteht darin, auf der GitHub-Seite auf die grüne Schaltfläche **Clone or download** zu klicken und die Option **Open in Visual Studio** zu wählen. 

![Klonen des Repositorys](images/JS2D_2.png)

Du kannst das Projekt auch als ZIP-Datei herunterladen oder andere Standardverfahren zum Arbeiten mit [GitHub-Projekten](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples) nutzen.

Nachdem die Projektmappe in Visual Studio geladen wurde, werden verschiedene Dateien angezeigt, z. B.:

* Images/: Ein Ordner mit den verschiedenen Symbolen, die von UWP-Apps benötigt werden, sowie das SpriteSheet des Spiels und einige andere Bitmaps.
* js/: Ein Ordner mit den JavaScript-Dateien. Die Datei „main.js“ ist unser Spiel, und die anderen Dateien sind EaselJS und PreloadJS.
* index.html: Die Webseite, die das Canvas-Objekt zum Hosten der Grafiken des Spiels enthält.

Jetzt kannst du das Spiel ausführen!

Drücke **F5**, um die App auszuführen. Ein Fenster sollte geöffnet werden, und wir sehen unseren vertrauten Dinosaurier in einer idyllischen (wenn auch kargen) Landschaft. Wir werden jetzt die App untersuchen, einige wichtige Teile erklären und währenddessen den Rest der Features entsperren.

![Ein ganz normaler Dinosaurier mit einer Ninja-Katze auf dem Rücken](images/JS2D_3.png)

**Hinweis:** Hat etwas nicht geklappt? Vergewissere dich, dass du Visual Studio mit Webunterstützung installiert hast. Erstelle hierzu ein neues Projekt. Sollte keine JavaScript-Unterstützung vorhanden sein, musst du Visual Studio neu installieren und das Kontrollkästchen *Microsoft Web Developer Tools* aktivieren.

## <a name="walkthough"></a>Exemplarische Vorgehensweise

Wenn du das Spiel mit F5 gestartet hast, fragst du dich vielleicht, was gerade passiert. Die Antwort lautet „nicht viel“, da ein Großteil des Codes derzeit auskommentiert ist. Alles, was du siehst, ist der Dinosaurier und eine unwirksame Aufforderung, die Leertaste zu drücken. 

### <a name="1-setting-the-stage"></a>1. Festlegen der Bühne

Wenn du **index.html** öffnest und untersuchst, siehst du, dass die Datei fast leer ist. Diese Datei ist die Standardwebseite, die unsere App enthält, und hat lediglich zwei wichtige Aufgaben. Sie enthält erstens den JavaScript-Quellcode für die CreateJS-Bibliotheken **EaselJS** und **PreloadJS** sowie die Datei **main.js** (unsere eigene Quellcodedatei).
Zweitens definiert sie ein &lt;Canvas&gt;-Tag, in dem unsere gesamten Grafiken angezeigt werden. Eine &lt;Canvas&gt; ist eine Standardkomponente eines HTML5-Dokuments. Wir geben ihr einen Namen (gameCanvas), damit unser Code in **main.js** darauf verweisen kann. Wenn du dein eigenes JavaScript-Spiel von Grund auf neu schreiben möchten, musst du die Dateien **EaselJS** und **PreloadJS** in deine Projektmappe kopieren und dann ein Canvas-Objekt erstellen.

Von EaselJS erhalten wir ein neues Objekt, das als „Bühne“ (*stage*) bezeichnet wird. Die Bühne ist mit der Canvas verknüpft und dient zum Anzeigen von Bildern und Text. Alle Objekte, die auf der Bühne angezeigt werden sollen, müssen zunächst wie folgt der Bühne als untergeordnetes Element hinzugefügt werden:

```
    stage.addChild(myObject);
```

Du siehst, dass diese Codezeile in **main.js** mehrmals enthalten ist.

Jetzt ist ein guter Zeitpunkt zum Öffnen von **main.js**.

### <a name="2-loading-the-bitmaps"></a>2. Laden der Bitmaps

EaselJS umfasst verschiedene Arten von Grafikobjekten. Wir können einfache Formen (z. B. das blaue Rechteck für den Himmel) oder Bitmaps (z. B. die Wolken, die wir hinzufügen möchten), Textobjekte und Sprites erstellen. Sprites verwenden ein (SpriteSheet)[https://createjs.com/docs/easeljs/classes/SpriteSheet.html ]: Die ist eine einzelne Bitmap, die mehrere Bilder enthält. Beispielsweise verwenden wir dieses SpriteSheet zum Speichern des anderen Frames der Dinosaurier-Animation:

![Sprite-Blatt für laufenden Dinosaurier](images/JS2D_4.png)

Wir bringen den Dinosaurier zum Laufen, indem wir die verschiedenen Frames definieren und festlegen, wie schnell sie in diesem Code animiert werden sollen:

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

Jetzt fügen wir der Bühne einige kleine Wolken hinzu. Wenn das Spiel ausgeführt wird, sollen sie über den Bildschirm schweben. Das Bild für die Cloud befindet sich bereits in der Projektmappe im Ordner *images*.

Suche in der Datei **main.js** nach der Funktion **init()** . Diese wird aufgerufen, wenn das Spiel gestartet wird, und dort beginnen wir mit der Einrichtung unserer Grafikobjekte.

Suche nach dem folgenden Code, und entferne die Kommentare (\\) aus der Zeile, die auf das Wolkenbild verweist.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript benötigt etwas Hilfe, wenn es darum geht, Ressourcen (z. B. Bilder) zu laden. Deshalb verwenden wir ein Feature der CreateJS-Bibliothek, das Bilder vorab laden kann und den Namen [LoadQueue](https://www.createjs.com/docs/preloadjs/classes/LoadQueue.html) hat. Wir wissen nicht, wie lange das Laden der Bilder dauert. Deshalb verwenden wir LoadQueue, um dies zu steuern. Sobald die Bilder verfügbar sind, werden wir von der Warteschlange darüber informiert, dass sie bereit sind. Zu diesem Zweck erstellen wir zuerst ein neues Objekt, das unsere gesamten Bilder enthält, und dann ein LoadQueue-Objekt. Im Code unten ist zu sehen, wie eine Funktion namens **loadingComplete()** aufgerufen wird, wenn alles bereit ist.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

Wenn die Funktion **loadingComplete()** aufgerufen wird, sind die Bilder geladen und können verwendet werden. Du siehst einen auskommentierten Abschnitt, mit dem die Wolken erstellt werden, sobald die Bitmap verfügbar ist. Entferne die Kommentare, damit der Abschnitt wie folgt aussieht:

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
Dieser Code erstellt drei Cloudobjekte, die jeweils das vorab geladene Bild verwenden, definiert deren Position und fügt diese anschließend der Bühne hinzu.

Wenn du die App erneut ausführst (durch Drücken von F5), siehst du, dass unsere Wolken angezeigt werden.

### <a name="3-moving-the-clouds"></a>3. Bewegen der Wolken

Jetzt sorgen wir dafür, dass sich die Wolken bewegen. Das Geheimnis für die Bewegung der Wolken – und aller anderen Elemente – ist die Einrichtung einer [Ticker](https://www.createjs.com/docs/easeljs/classes/Ticker.html)-Funktion, die wiederholt mehrmals pro Sekunde aufgerufen wird. Bei jedem Aufruf dieser Funktion wird die Grafik an einem etwas anderen Ort neu gezeichnet.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">Sieh dir den Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - Animating clouds</a> (CreateJS – Animieren von Wolken) von Microsoft Edge Docs (<a href="https://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) auf <a href="https://codepen.io">CodePen</a> an.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
  Der Code dazu ist bereits in der Datei **main.js** vorhanden, die von der CreateJS-Bibliothek „EaselJS“ bereitgestellt wird. Er sieht ungefähr so aus:

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

Dieser Code ruft eine Funktion namens **gameLoop()** mit 30 bis 60 Frames pro Sekunde auf. Die genaue Geschwindigkeit hängt von der Geschwindigkeit des Computers ab.

Suche nach der Funktion **gameLoop()** . Am unteren Ende siehst du eine Funktion namens **animateClouds()** . Bearbeite sie so, dass sie nicht mehr auskommentiert ist.

```
    // Move clouds
    animateClouds();
```

Wenn du die Definition dieser Funktion betrachtest, siehst du, wie für jede Wolke jeweils die X-Koordinate geändert wird. Wenn die X-Koordinate außerhalb der Bildschirmseite liegt, wird sie an den rechten Rand zurückgesetzt. Jede Wolke bewegt sich also mit einer etwas unterschiedlichen Geschwindigkeit.

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

Beim nächsten Ausführen der App siehst du, dass sich die Wolken bewegen. Jetzt haben wir also Bewegung!

### <a name="4-adding-keyboard-and-mouse-input"></a>4. Hinzufügen von Tastatur- und Mauseingaben

Ein Spiel, mit dem du nicht interagieren kannst, ist kein Spiel. Wir müssen dem Spieler also ermöglichen, die Tastatur oder Maus zu verwenden, um eine Aktion durchzuführen. In der Funktion **loadingComplete()** siehst du Folgendes. Entferne die Kommentare.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

Wir haben jetzt zwei Funktionen, die aufgerufen werden, wenn der Spieler eine Taste drückt oder mit der Maus klickt. Für beide Ereignisse wird **userDidSomething()** aufgerufen. Mit dieser Funktion wird die Gamestate-Variable untersucht, um zu entscheiden, was das Spiel derzeit ausführt und was daher als Nächstes passieren muss.

Gamestate ist ein Entwurfsmuster, das häufig in Spielen verwendet wird. Alle Vorgänge werden in der Funktion **gameLoop()** durchgeführt, die vom Ticker-Timer aufgerufen wird. Die Funktion „gameLoop()“ verfolgt mithilfe einer Variablen, ob das Spiel gespielt wird, gerade vorbei ist, jetzt begonnen werden kann oder sich in einem anderen vom Autor definierten Zustand befindet. Diese Zustandsvariable wird in einer Switch-Anweisung getestet, und sie definiert, welche anderen Funktionen aufgerufen werden. Wenn der Zustand auf „Wiedergabe“ festgelegt ist, werden also die Funktionen aufgerufen, die das Springen des Dinosauriers und die Bewegung der Fässer bewirken. Wenn der Dinosaurier getötet wird, wird die Gamestate-Variable auf den Zustand „Spiel beendet“ gesetzt und die Meldung „Game over!“ angezeigt. Wenn du an Spielentwurfsmustern interessiert bist, ist das Buch [Game Programming Patterns](https://gameprogrammingpatterns.com/) sehr hilfreich.

Führe die App erneut aus. Du kannst jetzt endlich mit dem Spielen beginnen. Drücke die Leertaste (oder klicke mit der Maus oder tippe auf den Bildschirm), damit etwas passiert. 

Du siehst, dass ein Fass auf dich zurollt: Drücke genau zum richtigen Zeitpunkt erneut die Leertaste oder klicke mit der Maus, damit der Dinosaurier springt. Wenn du nicht den richtigen Zeitpunkt erwischst, ist das Spiel vorüber.

Das Fass ist genauso animiert wie die Wolken (aber es wird jedes Mal schneller). Wir überprüfen die Position des Dinosauriers und des Fasses, um sicherzustellen, dass sie nicht kollidiert sind:

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

Wenn der Dinosaurier nicht springt und das Fass in der Nähe ist, ändert der Code die Zustandsvariable in den Zustand, den wir *GameOver* genannt haben. Wie du wahrscheinlich schon vermutest, wird das Spiel mit *GameOver* beendet.

Die wichtigsten Mechanismen unseres Spiels sind hiermit also abgeschlossen.

### <a name="5-resizing-support"></a>5. Unterstützen der Größenanpassung

Wir sind jetzt fast fertig! Zuvor gibt es noch ein lästiges Problem, das wir zuerst lösen müssen. Versuche, die Größe des Fensters anzupassen, während das Spiel ausgeführt wird. Du siehst, dass das Spiel schnell durcheinandergerät und Objekte sich nicht mehr am gewünschten Ort befinden. Wir können dies beheben, indem wir einen Handler für das Ereignis zum Ändern der Fenstergröße erstellen. Dieses Ereignis wird generiert, wenn der Spieler die Fenstergröße ändert oder das Gerät vom Quer- ins Hochformat gedreht wird.

Der Code dazu ist bereits vorhanden. (Wir rufen ihn beim Starten des Spiels auf, um sicherzustellen, dass die Standardfenstergröße funktioniert. Wenn eine UWP-App gestartet wird, ist nämlich nicht klar, welche Größe das Fenster haben wird.)

Kommentiere diese Zeile einfach aus, um die Funktion aufzurufen, wenn das Ereignis für die Bildschirmgröße ausgelöst wird:

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

Beim erneuten Ausführen der App solltest du jetzt die Größe des Fensters anpassen können und bessere Ergebnisse erhalten.

## <a name="publishing-to-the-microsoft-store"></a>Veröffentlichen im Microsoft Store

Nachdem du nun über eine UWP-App verfügst, kannst du sie im Microsoft Store veröffentlichen (vorausgesetzt, du hast sie vorher verbessert). 

Dieser Prozess umfasst mehrere Schritte.

1. Du musst als Windows-Entwickler [registriert](https://developer.microsoft.com/store/register) sein.
2. Du musst die [Prüfliste für die App-Übermittlung](https://docs.microsoft.com/windows/uwp/publish/app-submissions) verwenden.
3. Die App muss zur [Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process) eingereicht werden.

Weitere Informationen findest du unter [Veröffentlichen von Windows-Apps und -Spielen](https://docs.microsoft.com/windows/uwp/publish/).

## <a name="suggestions-for-other-features"></a>Vorschläge für andere Features.

Was tue ich jetzt? Hier sind einige Vorschläge für Features angegeben, die du deiner (hoffentlich bald) preisgekrönten App hinzufügen kannst.

1. Soundeffekte: Die CreateJS-Bibliothek ermöglicht die Audiounterstützung mit einer Bibliothek namens [SoundJS](https://www.createjs.com/soundjs).
2. Gamepad-Unterstützung: Es ist eine [API verfügbar](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345).
3. Erstelle ein deutlich verbessertes Spiel! Diese Aufgabe bleibt dir überlassen, aber es sind zahlreiche nützliche Onlineressourcen verfügbar. 

## <a name="other-links"></a>Weitere Links

* [Erstellen eines einfachen Windows-Spiels mit JavaScript](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [Auswählen einer HTML/JS-Spielengine](https://html5gameengine.com/)
* [Verwenden von CreateJS in deinem JS-basierten Spiel](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [Spielentwicklungskurse auf LinkedIn Learning](https://www.linkedin.com/learning/topics/game-development)
