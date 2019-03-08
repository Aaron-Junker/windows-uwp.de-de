---
title: Erstellen eines UWP-Spiels in JavaScript
description: Eine einfache UWP-Spiel für den Microsoft Store, geschrieben in JavaScript und CreateJS
ms.date: 02/09/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: 4d10dbf52f0ed01d46f9e5cba83cd14d48bfc88d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639175"
---
# <a name="create-a-uwp-game-in-javascript"></a>Erstellen eines UWP-Spiels in JavaScript

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Ein einfaches 2D-UWP-Spiel für den Microsoft Store, geschrieben in JavaScript und CreateJS


![Sprite-Blatt für laufenden Dino](images/JS2D_1.png)


## <a name="introduction"></a>Einführung


Veröffentlichen einer app auf dem Microsoft Store-Sie können freigegeben werden (oder verkaufen Sie es!) mit Millionen von Menschen auf vielen verschiedenen Geräten.  

Um Ihre Anwendung für den Microsoft Store veröffentlichen müssen sie als UWP (universelle Windows-Plattform)-app geschrieben werden. UWP ist jedoch extrem flexibel und unterstützt eine Vielzahl von Sprachen und Frameworks. Dieses Beispiel ist ein einfaches Spiel, das in JavaScript geschrieben ist und mehrere CreateJS-Bibliotheken nutzt. Es veranschaulicht, wie Sprites gezeichnet werden, eine Spielschleife erstellt wird, Tastatur und Maus unterstützt werden und die Anpassung an verschiedene Bildschirmgrößen erfolgt.

Dieses Projekt wurde mit JavaScript unter Verwendung von Visual Studio erstellt. Mit einigen geringfügigen Änderungen kann es auch auf einer Website gehostet oder an andere Plattformen angepasst werden. 

**Hinweis:** Dies ist keines Spiels (oder bei einem guten!); Es soll veranschaulichen die Verwendung der Bibliothek für JavaScript und eine dritte Partei um eine app auf dem Microsoft Store veröffentlichen.


## <a name="requirements"></a>Anforderungen

Um dieses Projekt auszuprobieren, benötigen Sie Folgendes:

* Einen Windows-Computer (oder einen virtuellen Computer) mit der aktuellen Version von Windows 10.
* Eine Kopie von Visual Studio. Die kostenlose Visual Studio Community Edition kann von der [Visual Studio-Homepage](https://visualstudio.com) heruntergeladen werden.

Dieses Projekt verwendet das CreateJS-JavaScript-Framework. Bei CreateJS handelt es sich um einen kostenlosen Satz von Tools mit MIT-Lizenz, welche die Erstellung Sprite-basierter Spiele erleichtern sollen. Die CreateJS-Bibliotheken sind bereits im Projekt vorhanden (suchen Sie nach *js/easeljs-0.8.2.min.js* und *js/preloadjs-0.6.2.min.js* in der Projektmappen-Explorer-Ansicht). Weitere Informationen zu CreateJS finden Sie unter der [CreateJS-Startseite](https://www.createjs.com).


## <a name="getting-started"></a>Erste Schritte

Der vollständige Quellcode für die App befindet sich in [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d).

Die einfachste Möglichkeit ist es, GitHub zu besuchen, auf die grüne Schaltfläche **Clone or download** zu klicken und **In Visual Studio öffnen** auszuwählen. 

![Klonen des Repositorys](images/JS2D_2.png)

Sie können das Projekt auch als Zip-Datei herunterladen oder andere Standardverfahren zum Arbeiten mit [GitHub-Projekten](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples) nutzen.

Nachdem die Projektmappe in Visual Studio geladen wurde, sehen Sie mehrere Dateien, einschließlich:

* Images/ – ein Ordner mit den verschiedenen Symbolen, die von UWP-Apps benötigt werden, sowie das SpriteSheet des Spiels und einige andere Bitmaps.
* js / – ein Ordner mit den JavaScript-Dateien. Die main.js-Datei ist unser Spiel, die anderen Dateien sind EaselJS und PreloadJS.
* index.html – die Webseite, die das Canvas-Objekt enthält, welches die Grafiken des Spiels hostet.

Jetzt können Sie das Spiel ausführen!

Drücken Sie **F5** zum Ausführen der App. Daraufhin sollte ein Fenster geöffnet, und unsere vertraut Godzilla Position in einer Landschaft (bei mit geringer Dichte). Wir werden jetzt die App untersuchen, einige wichtige Teile erklären und währenddessen den Rest der Features entsperren.

![Ein ganz normaler Dinosaurier mit einer Ninja-Katze auf seinem Rücken](images/JS2D_3.png)

**Hinweis:** Hat etwas nicht geklappt? Stellen Sie sicher, dass Sie Visual Studio mit Web-Unterstützung installiert haben. Sie können dies überprüfen, indem Sie ein neues Projekt erstellen. Wenn es keine Unterstützung für JavaScript gibt, müssen Sie Visual Studio neu installieren und das Feld *Microsoft Web Developer Tools* aktivieren.

## <a name="walkthough"></a>Exemplarische Vorgehensweise

Wenn Sie das Spiel mit F5 gestartet haben, wundern Sie sich vielleicht, was gerade passiert. Und die Antwort ist „nicht viel“, da ein Großteil des Codes derzeit auskommentiert ist. Alles, was Sie sehen, ist der Dinosaurier und eine unwirksame Aufforderung, die Leertaste zu drücken. 

### <a name="1-setting-the-stage"></a>1. Schaffen optimaler Voraussetzungen

Wenn Sie **index.html** öffnen und untersuchen, sehen Sie, dass die Datei fast leer ist. Diese Datei ist die Standardwebseite, die unsere App enthält, und erledigt nur zwei wichtige Dinge. Sie enthält zuerst den JavaScript-Quellcode für die CreateJS-Bibliotheken **EaselJS** und **PreloadJS**, und zudem **main.js** (unsere eigene Quellcodedatei).
Zweitens definiert sie ein &lt;Canvas&gt;-Tag, in dem all unsere Grafiken angezeigt werden. Ein &lt;Canvas&gt; ist eine Standardkomponente eines HTML5-Dokuments. Wir geben ihm einen Namen (gameCanvas), damit unser Code **main.js** darauf verweisen kann. Wenn Sie Ihr eigenes JavaScript-Spiel von Grund auf neu schreiben möchten, müssen Sie die Dateien **EaselJS** und **PreloadJS** in Ihre Projektmappe kopieren und dann ein Canvas-Objekt erstellen.

Von EaselJS erhalten wir ein neues Objekt, das *Phase* genannt wird. Die Phase ist mit dem Canvas verknüpft und dient zum Anzeigen von Bildern und Text. Alle Objekte, die auf der Bühne angezeigt werden sollen, müssen zunächst als untergeordnetes Element der Bühne wie folgt hinzugefügt werden:

```
    stage.addChild(myObject);
```

Sie sehen, dass diese Codezeile mehrmals in **main.js** angezeigt wird

Jetzt ist ein guter Zeitpunkt zum Öffnen von **main.js**.

### <a name="2-loading-the-bitmaps"></a>2. Laden die bitmaps

EaselJS bietet verschiedene Arten von Grafikobjekten. Wir können einfache Formen (z. B. das blaue Rechteck für den Himmel) oder Bitmaps (z. B. die Wolken, die wir hinzufügen möchten), Textobjekte und Sprites erstellen. Sprites einsetzen einer (SpriteSheet) [https://createjs.com/docs/easeljs/classes/SpriteSheet.html]: eine einzelne Bitmap, die mehrere Abbilder enthält. Beispielsweise verwenden wir dieses SpriteSheet zum Speichern des anderen Frames der Dinosaurier-Animation:

![Sprite-Blatt für laufenden Dino](images/JS2D_4.png)

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

Jetzt werden wir einige kleine Wolken zur Bühne hinzufügen. Wenn das Spiel ausgeführt wird, sollen sie über den Bildschirm schweben. Das Bild für die Cloud befindet sich bereits in der Projektmappe im Ordner *images*.

Sehen Sie sich **main.js** an, bis Sie die Funktion **init()** finden. Diese wird aufgerufen, wenn das Spiel gestartet wird, und dort beginnen wir mit der Einrichtung unserer Grafikobjekte.

Suchen Sie den folgenden Code, und entfernen Sie die Kommentare (\\) über die Befehlszeile, die auf das Cloud-Image verweist.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript benötigt etwas Hilfe, wenn es darum geht, Ressourcen wie Bilder zu laden, weshalb wir ein Feature der CreateJS-Bibliothek verwenden, das Bilder vorab laden kann und [LoadQueue](https://www.createjs.com/docs/preloadjs/classes/LoadQueue.html) heißt. Wir wissen nicht, wie lange es dauert, die Bilder zu laden, weshalb wir LoadQueue verwenden, um dies auszuprobieren. Sobald die Bilder verfügbar sind, wird die Warteschlange uns mitteilen, dass sie bereit sind. Zu diesem Zweck erstellen wir zuerst ein neues Objekt, das all unsere Bilder enthält, und erstellen dann ein LoadQueue-Objekt. Sie sehen im Code unten, wie dieses eingerichtet ist, um eine Funktion namens **loadingComplete()** aufzurufen, wenn alles bereit ist.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

Wenn die Funktion **loadingComplete()** aufgerufen wird, sind die Bilder geladen und können verwendet werden. Sie sehen einen auskommentierten Abschnitt, der die Wolken erstellt, sobald die Bitmap verfügbar ist. Entfernen Sie die Kommentare, damit der Abschnitt wie folgt aussieht:

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
Dieser Code erstellt drei Cloudobjekte, die jeweils das vorab geladene Bild verwenden, definiert deren Position und fügt diese anschließend zur Bühne hinzu.

Führen Sie die App erneut aus (durch Drücken von F5), und Sie werden sehen, dass unsere Wolken angezeigt werden.

### <a name="3-moving-the-clouds"></a>3. Verschieben Sie die clouds

Jetzt werden wir die Wolken bewegen. Das Geheimnis für das Bewegen der Wolken – und aller anderen Elemente – ist, eine [Ticker](https://www.createjs.com/docs/easeljs/classes/Ticker.html)-Funktion einzurichten, die wiederholt mehrmals pro Sekunde aufgerufen wird. Jedes Mal, wenn diese Funktion aufgerufen wird, wird die Grafik an einem etwas anderen Ort neu gezeichnet.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">Sehen Sie sich den Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS – Animieren von Wolken</a> von Microsoft Edge Docs (<a href="https://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) auf <a href="https://codepen.io">CodePen</a> an.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
  Der Code hierfür befindet sich bereits in der **"Main.js"** Datei, die von der Bibliothek CreateJS, EaselJS bereitgestellt. Es sieht ungefähr so aus:

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

Dieser Code ruft eine Funktion namens **gameLoop()** mit zwischen 30 und 60 Frames pro Sekunde auf. Die genaue Geschwindigkeit hängt von der Geschwindigkeit Ihres Computers ab.

Suchen Sie nach der **gameLoop()**-Funktion. Unten zum Ende hin sehen Sie eine Funktion namens **animateClouds()**. Bearbeiten Sie sie, damit sie nicht mehr auskommentiert ist.

```
    // Move clouds
    animateClouds();
```

Wenn Sie die Definition dieser Funktion betrachten, sehen Sie, wie diese jede Wolke nacheinander nimmt und deren X-Koordinate ändert. Wenn die X-Koordinate außerhalb der Bildschirmseite liegt, wird sie nach ganz rechts zurückgesetzt. Jede Wolke bewegt sich also mit einer etwas unterschiedlichen Geschwindigkeit.

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

Wenn Sie die App jetzt ausführen, sehen Sie, dass sich die Wolken bewegen. Jetzt haben wir also Bewegung!

### <a name="4-adding-keyboard-and-mouse-input"></a>4. Hinzufügen von Tastatur- und Mauseingaben

Ein Spiel, mit dem Sie nicht interagieren können, ist kein Spiel. Wir müssen dem Spieler also ermöglichen, die Tastatur oder Maus zu verwenden, um eine Aktion auszuführen. In der **loadingComplete()**-Funktion sehen Sie Folgendes. Entfernen Sie die Kommentare.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

Wir haben jetzt zwei Funktionen, die aufgerufen werden, wenn der Spieler eine Taste drückt oder mit der Maus klickt. Beide Ereignisse rufen **userDidSomething()** auf, eine Funktion, welche die Gamestate-Variable untersucht, um zu entscheiden, was das Spiel derzeit ausführt und was daher als Nächstes passieren muss.

Gamestate ist ein Entwurfsmuster, das häufig in Spielen verwendet wird. Alles, was passiert, erfolgt in der **gameLoop()**-Funktion, die vom Ticker-Timer aufgerufen wird. Die gameLoop()-Funktion verfolgt, ob das Spiel gespielt wird, das Spiel gerade vorbei ist, das Spiel jetzt begonnen werden kann oder einen anderen vom Autor definierten Zustand hat, und verwendet eine Variable. Diese Zustandsvariable wird in einer Switch-Anweisung getestet und definiert, welche anderen Funktionen aufgerufen werden. Wenn der Zustand auf „Wiedergabe“ festgelegt ist, werden die Funktionen, welche den Dinosaurier springen lassen und die Fässer verschieben, aufgerufen. Wenn der Dinosaurier getötet wird, wird die Gamestate-Variable auf den Zustand „Spiel beendet“ gesetzt und die Meldung „Game over!“ wird angezeigt. Wenn Sie an Spielentwurfsmustern interessiert sind, ist das Buch [Game Programming Patterns](https://gameprogrammingpatterns.com/) sehr hilfreich.

Führen Sie die App erneut aus, anschließend können Sie mit dem Spiel beginnen. Drücken Sie die Leertaste (oder klicken mit der Maus oder tippen auf den Bildschirm), damit etwas passiert. 

Sie sehen ein Fass, das auf Sie zurollt: Drücken Sie genau zum richtigen Zeitpunkt erneut die Leertaste oder klicken Sie, sodass der Dinosaurier wegspringt. Wenn Sie nicht den richtigen Zeitpunkt erwischen, ist das Spiel vorüber.

Das Fass ist genauso animiert wie die Wolken (auch wenn es jedes Mal schneller wird), und wir überprüfen die Position des Dinosauriers und des Fasses, um sicherzustellen, dass sie nicht kollidieren:

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

Wenn der Dinosaurier nicht springt und das Fass in der Nähe ist, ändert der Code die Zustandsvariable in den Zustand, den wir *GameOver* genannt haben. Wie Sie sich denken können, beendet *GameOver* das Spiel.

So sind die wichtigsten Mechanismen unseres Spiels abgeschlossen.

### <a name="5-resizing-support"></a>5. Ändern der Größe von support

Wir sind jetzt fast fertig! Zuvor gibt es noch ein lästiges Problem, das wir zuerst lösen müssen. Versuchen Sie, die Größe des Fensters anzupassen, wenn das Spiel ausgeführt wird. Sie sehen, dass das Spiel schnell durcheinandergerät und Objekte nicht länger dort sind, wo sie sein sollen. Wir können dies beheben, indem wir einen Handler für das Fenstergrößeänderungsereignis erstellen, das generiert wird, wenn der Spieler die Fenstergröße ändert oder das Gerät vom Quer- ins Hochformat gedreht wird.

Der Code dazu ist bereits vorhanden (tatsächlich rufen wir ihn auf, wenn das Spiel erstmalig gestartet wird, um sicherzustellen, dass die Standardfenstergröße funktioniert. Wenn eine UWP-App gestartet wird, wissen Sie nicht sicher, welche Größe das Fenster haben wird).

Kommentieren Sie diese Zeile einfach aus, um die Funktion aufrufen, wenn das Bildschirmgrößenereignis ausgelöst wird:

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

Wenn Sie die App erneut ausführen, sollten Sie jetzt die Größe des Fensters anpassen können und bessere Ergebnisse erhalten.

## <a name="publishing-to-the-microsoft-store"></a>Veröffentlichung in den Microsoft Store

Nachdem Sie eine UWP-app verfügen, ist es möglich, die in den Microsoft Store (vorausgesetzt, dass Sie es zunächst verbessert haben!) veröffentlichen 

Dazu müssen Sie einige Schritte durchführen.

1. Sie müssen als Windows-Entwickler [registriert](https://developer.microsoft.com/en-us/store/register) sein.
2. Verwenden Sie die [Prüfliste für App-Übermittlung](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).
3. Die App muss zur [Zertifizierung](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process) eingereicht werden.

Weitere Informationen finden Sie unter [Veröffentlichen Ihrer UWP-app](https://developer.microsoft.com/en-us/store/publish-apps).

## <a name="suggestions-for-other-features"></a>Vorschläge für andere Features.

Was tue ich jetzt? Hier sind einige Vorschläge für Features, die Sie zu Ihrer (hoffentlich bald) preisgekrönten App hinzufügen können.

1. Soundeffekte Die CreateJS-Bibliothek bietet Audiounterstützung mit einer Bibliothek namens [SoundJS](https://www.createjs.com/soundjs).
2. Gamepad-Unterstützung. Es gibt eine [verfügbare API](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345).
3. Erstellen Sie ein viel, viel besseres Spiel! Dieser Teil ist Ihre Aufgabe, aber es gibt zahlreiche Ressourcen online. 

## <a name="other-links"></a>Weitere Links

* [Stellen Sie ein einfaches Windows-Spiel mit JavaScript](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [Wählen eine HTML/JS-Spiele-engine](https://html5gameengine.com/)
* [Verwendung von CreateJS in Ihre JS-basierten Spiel](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [Kursen zur Spielentwicklung auf LinkedIn Learning](https://www.linkedin.com/learning/topics/game-development)
