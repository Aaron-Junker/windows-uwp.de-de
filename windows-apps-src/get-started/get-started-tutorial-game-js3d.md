---
title: Tutorial für die ersten Schritte – 3D-UWP-Spiel in JavaScript
description: Ein UWP-Spiel für den Microsoft Store, geschrieben in JavaScript mit „three.js“
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: e3f46e0d1837f391ffc7cc6ca361a2c92212565b
ms.sourcegitcommit: 9940ed6431aadbd8d4e54ca23d8ae44d3a2d048d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2020
ms.locfileid: "91403925"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>Erstellen eines 3D-JavaScript-Spiels mit „three.js“

## <a name="introduction"></a>Einführung

Für Webentwickler oder JavaScript-Tüftler ist die Entwicklung von UWP-Apps mit JavaScript eine einfache Möglichkeit, ihre Apps auf der ganzen Welt zu vertreiben. Der Vorteil: Sie müssen keine Sprache wie C# oder C++ lernen.

In diesem Beispiel nutzen wir die Bibliothek **three.js**. Diese Bibliothek baut auf WebGL auf – einer API zum Rendern von 2D- und 3D-Grafiken für Webbrowser. **three.js** vereinfacht diese komplizierte API und damit auch die 3D-Entwicklung. 


Möchtest du schon einmal einen Blick auf die fertige App werfen, bevor du weiterliest? Du findest sie bei CodePen.

<iframe height='300' scrolling='no' title='Dino game final' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Dino game final</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

> [!NOTE] 
> Das ist natürlich noch kein vollständiges Spiel, sondern soll lediglich zeigen, wie du mit JavaScript und einer Drittanbieterbibliothek eine App erstellen kannst, die für die Veröffentlichung im Microsoft Store bereit ist.


## <a name="requirements"></a>Anforderungen

Um dieses Projekt ausprobieren zu können, benötigst du Folgendes:
-   Einen Windows-Computer (bzw. einen virtuellen Computer) mit der aktuellen Version von Windows 10.
-   Eine Kopie von Visual Studio. Die kostenlose Visual Studio Community Edition kann von der [Visual Studio-Homepage](https://visualstudio.com/) heruntergeladen werden.
In diesem Projekt wird die JavaScript-Bibliothek **three.js** verwendet. **three.js** wird unter der MIT-Lizenz veröffentlicht. Diese Bibliothek ist bereits im Projekt vorhanden. (Suche im Projektmappen-Explorer nach `js/libs`.) Weitere Informationen zu dieser Bibliothek findest du auf der Homepage von [**three.js**](https://threejs.org/).

## <a name="getting-started"></a>Erste Schritte

Der vollständige Quellcode für die App befindet sich auf [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d).

Am einfachsten ist es, GitHub zu besuchen, auf die grüne Schaltfläche „Clone or download“ (Klonen oder herunterladen) zu klicken und „In Visual Studio öffnen“ auszuwählen. 

![Schaltfläche zum Klonen oder Herunterladen](images/3dclone.png)

Wenn du das Projekt nicht klonen möchtest, kannst du es als ZIP-Datei herunterladen.
Nachdem die Projektmappe in Visual Studio geladen wurde, werden mehrere Dateien angezeigt. Hierzu zählen unter anderem:
-   „Images/“: Ordner mit den verschiedenen Symbolen, die für UWP-Apps erforderlich sind.
- „css/“: Ordner mit den zu verwendenden CSS.
-   „js/“: Ordner mit den JavaScript-Dateien. Die Datei „main.js“ ist unser Spiel. Bei den anderen Dateien handelt es sich dagegen um Drittanbieterbibliotheken.
-   „models/“: Ordner mit den 3D-Modellen. Bei diesem Spiel haben wir nur ein einzelnes Modell für den Dinosaurier.
-   „index.html“: Webseite zum Hosten des Renderers für das Spiel.

Jetzt kannst du das Spiel ausführen!

Drücke F5, um die App zu starten. Daraufhin sollte ein Fenster mit der Aufforderung geöffnet werden, auf den Bildschirm zu klicken. Im Hintergrund bewegt sich außerdem ein Dinosaurier. Schließe das Spiel wieder, damit wir uns die App und ihre wichtigsten Komponenten genauer ansehen können.

> [!NOTE] 
> Hat etwas nicht geklappt? Vergewissere dich, dass du Visual Studio mit Webunterstützung installiert hast. Erstelle hierzu ein neues Projekt. Sollte keine JavaScript-Unterstützung vorhanden sein, musst du Visual Studio neu installieren und das Kontrollkästchen „Microsoft Web Developer Tools“ aktivieren.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wenn du dieses Spiel startest, wirst du aufgefordert, auf den Bildschirm zu klicken. Die [Pointer Lock-API](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) sorgt dafür, dass du dich mit der Maus umsehen kannst. Zur Bewegungssteuerung können die Tasten W, A, S, D bzw. die Pfeiltasten verwendet werden.
Das Ziel dieses Spiels besteht darin, sich von dem Dinosaurier fernzuhalten. Ab einer gewissen Entfernung zu dir beginnt der Dinosaurier, dich zu jagen, bis du entweder wieder außer Reichweite bist oder er dir zu nahe kommt und du das Spiel verlierst.

### <a name="1-setting-up-your-initial-html-file"></a>1. Einrichten der ersten HTML-Datei

In **index.html** musst du zunächst etwas HTML-Code hinzufügen. Diese Datei ist die Standardwebseite, die unsere App enthält.

Dafür richten wir nun die zu verwendenden Bibliotheken sowie das Element vom Typ `div` (namens `container`) ein, das als Renderziel für unsere Grafik fungiert. Außerdem legen wir einen Verweis auf **main.js** (unseren Spielcode) fest.


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


Damit ist unser anfänglicher HTML-Code einsatzbereit. Als Nächstes kümmern wir uns in **main.js** um die Grafik.

### <a name="2-creating-your-scene"></a>2. Erstellen der Szene

Im Abschnitt mit der exemplarischen Vorgehensweise fügen wir die Grundlage des Spiels hinzu.

Dazu arbeiten wir zunächst eine Szene (`scene`) aus. Bei einer Szene (`scene`) in **three.js** handelt es sich um den Ort, dem du deine Kamera, Objekte und Lichter hinzufügst. Du benötigst auch einen Renderer, der die Perspektive deiner Kamera in der Szene erfasst und anzeigt.

In **main.js** erstellen wir eine Funktion namens `init()`, die all das erledigt und einige zusätzliche Funktionen aufruft:

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

Zu den anderen Funktionen, die wir erstellen müssen, zählen folgende:
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- Funktionen zur Konvertierung von Einheiten

#### <a name="createmazecubes"></a>createMazeCubes()

Die Funktion `createMazeCubes()` fügt unserer Szene einen einfachen Würfel hinzu. Später verwenden wir die Funktion, um viele Würfel hinzuzufügen und unser Labyrinth zu erstellen.

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

Die Funktion `addLights()` ist eine einfache Funktion, die die Erstellung unserer Lichter zusammenfasst und sie der Szene hinzufügt.

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

Die Funktion `onWindowResize` wird aufgerufen, wenn der Ereignislistener feststellt, dass ein Ereignis vom Typ `resize` ausgelöst wurde. Das passiert, wenn der Benutzer die Größe des Fensters anpasst. In diesem Fall möchten wir sicherstellen, dass das Bild proportional bleibt und im gesamten Fenster sichtbar ist.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

Als Letztes benötigen wir noch die Funktion `animate()`, die auch die Funktion `render()` aufruft. Die Funktion [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) dient zur kontinuierlichen Aktualisierung unseres Renderers. Später verwenden wir diese Funktionen, um unseren Renderer mit Animationen wie etwa der Bewegung durch das Labyrinth zu aktualisieren.

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>Funktionen zur Konvertierung von Einheiten

Drehungen werden in **three.js** im Bogenmaß gemessen. Zur Vereinfachung fügen wir einige Funktionen hinzu, um Werte mühelos zwischen Grad und Bogenmaß konvertieren zu können. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

Wer weiß schon, dass 30 Grad einem Bogenmaß von 0,523 entspricht? Stattdessen verwenden wir lieber `degreesToRadians(30)`, um unsere Drehung zu erhalten, die dann in der Funktion `createMazeCubes()` verwendet wird.

___

Das war zwar eine ganze Menge Code, dafür haben wir nun aber einen hübschen Würfel, der in unserem Container (`container`) gerendert wird. Schau dir die Ergebnisse in der CodePen-Instanz an.

Du kannst den gesamten JavaScript-Code kopieren und in diese CodePen-Instanz einfügen, falls bei dir Probleme aufgetreten sind. Außerdem kannst du den Code bearbeiten, um einige Lichter anzupassen und einige Farben zu ändern. 

<iframe height='300' scrolling='no' title='Lights, camera, cube!' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>Lights, camera, cube!</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) bei <a href='https://codepen.io'>CodePen</a> an.
</iframe>


### <a name="3-making-the-maze"></a>3. Erstellen des Labyrinths

Ein einzelner Würfel ist ja gut und schön, noch beeindruckender ist jedoch ein ganzes Labyrinth aus Würfeln. Es kein Geheimnis, dass eine der schnellsten Methoden zur Erstellung eines Levels in der Platzierung von Würfeln mit einem 2D-Array besteht.
 
![Labyrinth, das mit einem 2D-Array erstellt wurde](images/dinomap.png)

Durch die Platzierung von „1“ (für Stellen, an denen sich Würfel befinden) und „0“ (für Stellen, an denen sich keine Würfel befinden) kannst du das Labyrinth ganz einfach manuell erstellen und optimieren.

Hierzu ersetzen wir unsere alte Funktion `createMazeCubes()` durch eine Funktion mit einer geschachtelten Schleife, um mehrere Würfel zu erstellen und zu platzieren. Außerdem erstellen wir ein Array namens `collidableObjects` und fügen ihm die Würfel hinzu, um sie später in diesem Tutorial für die Kollisionserkennung verwenden zu können:

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

Da wir nun wissen, wie viele Würfel verwendet werden (und wie groß sie sind), können wir als Nächstes mithilfe der berechneten Variablen `mapSize` die Dimensionen der Grundebene festlegen:

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

Zum Schluss fügen wir dem Labyrinth noch Außenwände hinzu, um alles einzufassen. Wir verwenden eine Schleife, um gleichzeitig zwei Ebenen (unsere Wände) zu erstellen. Zur Bestimmung der Breite nutzen wir dabei die Variable `mapSize`, die wir in `createGround()` berechnet haben. Die neuen Wände werden zur späteren Kollisionserkennung ebenfalls dem Array `collidableObjects` hinzugefügt:

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


Vergiss nicht, in deiner Funktion `init()` nach `createMazeCubes()` einen Aufruf für `createGround()` und `createPerimWalls` hinzuzufügen, damit sie kompiliert werden.
___

Nun haben wir zwar ein hübsches Labyrinth, können uns aber noch kein so rechtes Bild davon machen, wie toll es ist, da wir unsere Kamera nicht bewegen können. Daher fügen wir als Nächstes einige Kamerasteuerelemente hinzu.

In der CodePen-Instanz kannst du problemlos mit einigen Dingen experimentieren und beispielsweise die Farben der Würfel ändern oder den Boden entfernen, indem du `createGround()` in der Funktion `init()` auskommentierst.


<iframe height='300' scrolling='no' title='Maze building' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>Maze building</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. Ermöglichen, dass der Spieler sich umsehen kann

Nun ist es an der Zeit, sich in das Labyrinth zu begeben und sich ein wenig umzusehen. Dazu verwenden wir die Bibliothek **PointerLockControls.js** und unsere Kamera.

Die Bibliothek **PointerLockControls.js** verwendet die Maus, um die Kamera in die Richtung zu drehen, in der die Maus bewegt wird, sodass der Spieler sich umsehen kann. 

Als Erstes fügen wir der Datei **index.html** einige neue Elemente hinzu:

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

Außerdem benötigen wir alle CSS in der CodePen-Instanz am Ende dieses Abschnitts. Diese müssen in die Datei **stylesheet.css** eingefügt werden.

Wechsle nun wieder zurück zu **main.js**, und füge einige neue globale Variablen hinzu (`controls` zum Speichern unseres Controllers, `controlsEnabled` zum Nachverfolgen des Controllerzustands und `blocker`, um das Element `blocker` in **index.html** abzurufen):

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


Nun können wir in der Funktion `init()` ein neues Objekt vom Typ `PointerLockControls` erstellen, ihm unsere Kamera (`camera`) übergeben und die Kamera (`camera`) anschließend hinzufügen. (Der Zugriff erfolgt mithilfe von `controls.getObject()`.)

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

Die Kamera ist jetzt zwar verbunden, wir müssen jedoch noch die Interaktion mit der Maus und dem Controller ermöglichen, damit wir uns umsehen können. 

Hier kommt die [Pointer Lock-API](/microsoft-edge/dev-guide/dom/pointer-lock) ins Spiel: Mit ihr lassen sich Mausbewegungen mit unserer Kamera verknüpfen. Die Pointer Lock-API sorgt auch dafür, dass der Mauszeiger ausgeblendet wird, um die Immersion zu verbessern. Nach dem Drücken von ESC wird die Verbindung zwischen Maus und Kamera getrennt, und der Mauszeiger wird wieder angezeigt. Dazu fügen wir die Funktionen `getPointerLock()` und `lockChange()` hinzu.

Die Funktion `getPointerLock()` lauscht auf einen Mausklick. Nach dem Klick versucht unser gerendertes Spiel (im Element `container`), die Kontrolle über die Maus zu erlangen. Wir fügen auch einen Ereignislistener hinzu, um zu erkennen, wenn der Spieler die Sperre aktiviert oder deaktiviert, wodurch wiederum `lockChange()` aufgerufen wird. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

Die Funktion `lockChange()` muss die Steuerelemente und das Element `blocker` entweder deaktivieren oder aktivieren. Wir können den Zustand der Zeigersperre ermitteln, indem wir überprüfen, ob das Ziel der Eigenschaft [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) für Mausereignisse auf unseren Container (`container`) festgelegt ist.

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

Als Nächstes können wir unmittelbar vor der Funktion `init()` einen Aufruf von `getPointerLock()` hinzufügen.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

Wir können uns nun zwar **umsehen**, so richtig beeindruckend wird es aber erst, wenn wir uns auch **bewegen** können. Im nächsten Abschnitt geht es mit Vektoren etwas mathematischer zu, aber was wären 3D-Grafiken ohne etwas Mathematik?

<iframe height='300' scrolling='no' title='Look around' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>Look around</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>


### <a name="5-adding-player-movement"></a>5. Hinzufügen der Spielerbewegung

Damit wir es dem Spieler ermöglichen können, sich zu bewegen, müssen wir uns zunächst an unseren Mathematikunterricht zurückerinnern. Auf die Kamera (`camera`) muss eine Geschwindigkeit (Bewegung) entlang eines bestimmten Vektors (Richtung) angewendet werden.

Dazu fügen wir weitere globale Variablen hinzu, um die Richtung nachzuverfolgen, in die sich der Spieler bewegt, und legen einen anfänglichen Geschwindigkeitsvektor fest:

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

Lege `clock` am Anfang der Funktion `init()` auf ein neues Objekt vom Typ `Clock` fest. Dies dient zum Nachverfolgen der Zeitdifferenz (Delta) beim Rendern neuer Frames. Darüber hinaus muss ein Aufruf von `listenForPlayerMovement()` hinzugefügt werden, um die Benutzereingabe zu erfassen. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

Die Funktion `listenForPlayerMovement()` dient zum Ändern unserer Richtungszustände. Am Ende der Funktion befinden sich zwei Ereignislistener, die darauf warten, dass Tasten gedrückt und losgelassen werden. Nachdem eines dieser Ereignisse ausgelöst wurde, überprüfen wir, ob es sich um eine Taste handelt, die eine Bewegung auslösen oder beenden soll.

Dieses Spiel haben wir so eingerichtet, dass sich der Spieler mit den Tasten W, A, S und D oder mit den Pfeiltasten bewegen kann.

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

Da wir nun bestimmen können, in welche Richtung sich der Benutzer bewegen möchte (wird als `true` in einem der globalen Richtungsflags gespeichert), ist es Zeit für etwas Action. Dazu verwenden wir die Funktion `animatePlayer()`.

Diese Funktion wird innerhalb von `animate()` aufgerufen und übergibt `delta`, um die Zeitdifferenz zwischen Frames abzurufen, damit unsere Bewegungen nicht asynchron wirken, wenn sich die Framerate verändert:

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

Kommen wir nun zum unterhaltsamen Teil. Unser Bewegungsvektor (`playerVeloctiy`) besitzt drei Parameter (`(x, y, z)`), wobei `y` die vertikale Bewegung darstellt. Da wir in diesem Spiel nicht springen, arbeiten wir nur mit den Parametern `x` und `z`. Dieser Vektor ist anfangs auf (0, 0, 0) festgelegt.

Wie im folgenden Code zu sehen, werden einige Überprüfungen ausgeführt, um zu ermitteln, welches Richtungsflag in `true` geändert wird. Sobald wir über die Richtung verfügen, wenden wir mittels Addition oder Subtraktion für `x` und `y` eine Bewegung in die entsprechende Richtung an. Wenn keine Bewegungstasten gedrückt werden, wird der Vektor wieder auf `(0, 0, 0)` zurückgesetzt.


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

Am Ende wenden wir die aktualisierten Werte für `x` und `y` als Übersetzungen auf die Kamera an, damit sich der Spieler tatsächlich bewegt.

---

Herzlichen Glückwunsch! Du hast nun eine vom Spieler steuerbare Kamera, mit der sich der Spieler bewegen und umsehen kann. Wir können zwar immer noch durch Wände gehen, darum kümmern wir uns aber später. Als Nächstes fügen wir unseren Dinosaurier hinzu.

<iframe height='300' scrolling='no' title='Move around' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>Move around</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

> [!NOTE]
> Wenn du diese Steuerelemente in deiner UWP-App verwendest, kommt es möglicherweise zu Eingabeverzögerungen sowie zu nicht registrierten Ereignissen vom Typ `keyUp`. Diese Probleme sind bekannt und werden voraussichtlich bald behoben.

### <a name="6-load-that-dino"></a>6. Laden des Dinosauriers

Wenn du das Repository dieses Projekts geklont oder heruntergeladen hast, siehst du einen Ordner namens `models`, der `dino.json` enthält. Bei dieser JSON-Datei handelt es sich um das 3D-Modell eines Dinosauriers, das in Blender erstellt und exportiert wurde.


Um diesen Dino laden zu können, müssen wir weitere globale Variablen hinzufügen:

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

Nach der Erstellung unseres JSON-Loaders (`JSONLoader`) übergeben wir den Pfad unserer Datei **dino.json** sowie einen Rückruf mit der Geometrie und dem Material aus der Datei.
Das Laden des Dinos ist eine asynchrone Aufgabe. Es wird also nichts gerendert, solange der Dino nicht vollständig geladen ist. In unserer Datei **index.html** haben wir die Zeichenfolge im Element `instructions` in `"Loading..."` geändert, damit der Spieler weiß, dass sich etwas tut.

Aktualisiere nach dem Laden des Dinos das Element `instructions` mit der tatsächlichen Spielanleitung, und verschiebe die Funktion `animate()` vom Ende von `init()` an das Ende des Funktionsrückrufs, wie hier zu sehen:

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

Unser Dinomodell ist nun geladen. Sieh es dir an!

<iframe height='300' scrolling='no' title='Adding the dino' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>Adding the dino</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

### <a name="7-move-that-dino"></a>7. Bewegen des Dinos

Da künstliche Intelligenz in Spielen eine äußerst komplexe Angelegenheit sein kann, statten wir den Dino in diesem Beispiel nur mit einem ganz einfachen Bewegungsverhalten aus. Unser Dino bewegt sich geradeaus sowie durch Wände und verschwindet in der Ferne im Nebel.

Füge hierzu zunächst die globale Variable `dinoVelocity` hinzu.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 Rufe als Nächstes über die Funktion `animation()` die Funktion `animateDino()` auf, und füge den folgenden Code hinzu:

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

Es ist nicht besonders befriedigend, dabei zuzusehen, wie der Dino verschwindet. Mit der später hinzugefügten Kollisionserkennung wird es aber interessanter.

<iframe height='300' scrolling='no' title='Moving the dino - no collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>Moving the dino - no collision</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. Kollisionserkennung für den Spieler

Spieler und Dino können sich nun bewegen. Wir haben allerdings das Problem, dass sie immer noch durch Wände gehen können. Beim Hinzufügen unserer Würfel und Wände weiter oben in diesem Tutorial haben wir sie auch dem Array `collidableObjects` hinzugefügt. Dieses Array verwenden wir nun, um zu ermitteln, ob sich ein Spieler zu nah an einem Objekt befindet, das für ihn nicht passierbar ist.

Eine bevorstehende Überschneidung wird hier mithilfe von Raycastern bestimmt. Einen Raycaster kannst du dir wie einen Laserstrahl vorstellen, der die Kamera in eine bestimmte Richtung verlässt und meldet, ob er auf ein Objekt getroffen ist und wie weit dieses Objekt genau entfernt ist.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

Wir erstellen eine neue Funktion namens `detectPlayerCollision()`, die `true` zurückgibt, wenn sich der Spieler zu nah an einem Kollisionsobjekt befindet.
Für den Spieler wenden wir einen einzelnen Raycaster an, dessen Richtung an die Bewegungsrichtung des Spielers gekoppelt ist.

Hierzu erstellen wir `rotationMatrix` – eine undefinierte Matrix. Die Überprüfung der Bewegungsrichtung liefert eine definierte Matrix vom Typ `rotationMatrix` (oder eine undefinierte, wenn du dich geradeaus bewegst).
Ist sie definiert, wird `rotationMatrix` auf die Richtung der Steuerelemente angewendet. 

Anschließend wird ein Raycaster erstellt, der bei der Kamera beginnt und in die Richtung `cameraDirection` ausgesendet wird.


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

Die Funktion `detectPlayerCollision()` basiert auf der Hilfsfunktion `rayIntersect()`.
Diese akzeptiert einen Raycaster und einen Wert, der angibt, wie nah wir einem Objekt aus dem Array `collidableObjects` kommen dürfen, bevor eine Kollision festgestellt wird.

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

Nachdem wir nun ermitteln können, wann eine Kollision bevorsteht, können wir die Funktion `animatePlayer()` optimieren:

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

Wir haben jetzt eine Kollisionserkennung für den Spieler. Probiere es am besten gleich mal aus!

<iframe height='300' scrolling='no' title='Moving the player - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>Moving the player - collision</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. Kollisionserkennung und Animation für den Dino

Als Nächstes müssen wir verhindern, dass unser Dino durch Wände läuft, und dafür sorgen, dass er nach dem Zufallsprinzip die Richtung ändert, wenn er einem Kollisionsobjekt zu nahe kommt.

Fangen wir mit der Kollisionserkennung für unseren Dino an. 

Für die Kollisionserkennung benötigen wir eine weitere globale Variable:

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

Nachdem wir angegeben haben, bei welchem Abstand unser Dino kollidieren soll, fügen wir eine ähnliche (aber etwas einfachere) Funktion wie `detectPlayerCollision()` hinzu.
Die Funktion `detectDinoCollision` ist einfach, weil wir immer nur einen einzelnen Raycaster benötigen, der direkt von der Vorderseite des Dinos ausgeht. Es ist keine Drehung erforderlich wie bei der Spielerkollision.

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

Sehen wir uns doch einmal kurz die fertige Funktion `animateDino()` mit implementierter Kollisionserkennung an:


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

Unser Dino soll sich immer um -90, 90 oder 180 Grad drehen. Der Einfachheit halber haben wir weiter oben das Array `directionMultiples` erstellt, das diese Zahlen erzeugt, wenn es mit 90 multipliziert wird.
Für die Wahl eines zufälligen Drehungsgradwerts haben wir die Hilfsfunktion `getRandomInt()` hinzugefügt, die den Wert „0“, „1“ oder „2“ verwendet, um einen willkürlichen Index des Arrays darzustellen.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

Anschließend multiplizieren wir den willkürlichen Index des Arrays mit 90, um den Gradwert (im Bogenmaß) für die Drehung zu erhalten.
Wenn wir diesen Wert nun der `y`-Drehung des Dinos mit `dino.rotation.y += randomDirection;` hinzufügen, ändert der Dino bei einer Kollision seine Richtung nach dem Zufallsprinzip.


---

Geschafft! Wir haben jetzt einen Dino mit künstlicher Intelligenz, der sich in unserem Labyrinth bewegen kann!

<iframe height='300' scrolling='no' title='Moving the dino - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>Moving the dino - collision</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>

### <a name="10-starting-the-chase"></a>10. Starten der Jagd

Ab einer bestimmten Entfernung zwischen Dino und Spieler soll der Dino den Spieler jagen. Da es sich hierbei nur um ein Beispiel handelt, werden für den Dino keine komplexen Algorithmen angewendet, um den Spieler zu verfolgen. Stattdessen blickt der Dino einfach in Richtung des Spielers und bewegt sich auf ihn zu. In einem offenen Teil des Labyrinths funktioniert das sehr gut. Ist allerdings eine Wand im Weg, bleibt der Dino stecken.

In der Funktion `animate()` fügen wir eine boolesche Variable hinzu, deren Wert auf der Rückgabe von `triggerChase()` basiert:

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

Die Funktion `triggerChase` überprüft, ob sich der Spieler in Jagdreichweite des Dinos befindet, und sorgt dafür, dass der Dino immer in Richtung des Spielers blickt, sodass er sich auf ihn zu bewegen kann. 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

Die zweite Hälfte von `triggerChase` dient zum Anzeigen von Text, um dem Spieler mitzuteilen, wie weit der Dino entfernt ist. Außerdem geben wir mithilfe von `CATCHOFFSET` die gewünschte Entfernung von `0` an. Ohne Angabe eines Offsets befindet sich `0` direkt auf dem Spieler, was nicht gerade ein filmreifes Ende abgeben würde.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

Wir haben nun also einen wilden Dinosaurier, der beginnt, den Spieler zu jagen, sobald dieser ihm zu nahe kommt, und nicht lockerlässt, bis sich seine Position mit der Position des Spielers deckt.
Zum Schluss müssen noch ein paar Bedingungen für das Spielende hinzugefügt werden, sobald der Dino `CATCHOFFSET` Einheiten entfernt ist.

<iframe height='300' scrolling='no' title='The chase' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sieh dir bei <a href='https://codepen.io'>CodePen</a> den Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>The chase</a> der Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) an.
</iframe>


### <a name="11-ending-the-game"></a>11. Spielende


Zu Beginn hatten wir nur einen einfachen Würfel. Inzwischen haben wir eine Menge erreicht. Daher ist es nun Zeit, sich um das Ende zu kümmern.

Dazu legen wir zunächst eine Variable fest, um nachzuverfolgen, ob das Spiel beendet ist:

```javascript
var gameOver = false;
```

Als Nächstes müssen wir die Funktion `animate()` ein letztes Mal aktualisieren, um zu überprüfen, ob der Dino dem Spieler zu nahe gekommen ist.
Ist der Dino zu nah, starten wir eine neue Funktion namens `caught()` und beenden die Bewegung von Spieler und Dino. Andernfalls bleibt alles beim Alten, und Spieler und Dino bewegen sich weiter.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

Wenn der Dino den Spieler erwischt, zeigt `caught()` das Element `blocker` an und aktualisiert den Text, um anzugeben, dass der Spieler verloren hat.
Außerdem wird die Variable `gameOver` auf `true` festgelegt, um anzugeben, dass das Spiel vorbei ist.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


Nachdem wir nun feststellen können, ob das Spiel vorbei ist, können wir der Funktion `lockChange()` eine entsprechende Überprüfung hinzufügen.
Wenn der Benutzer nach dem Ende des Spiels ESC drückt, können wir `location.reload` hinzufügen, um das Spiel neu zu starten.

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

Das war's. Es war ein langer Weg, aber nun haben wir Spiel, das unter Verwendung von **three.js** erstellt wurde.

Navigiere wieder zum Anfang der Seite, und sieh dir den [finalen CodePen](#introduction) an.


## <a name="publishing-to-the-microsoft-store"></a>Veröffentlichen im Microsoft Store
Nachdem du nun über eine UWP-App verfügst, kannst du sie im Microsoft Store veröffentlichen (vorausgesetzt, du hast sie vorher verbessert). Dieser Prozess umfasst mehrere Schritte.

1.  Du musst als Windows-Entwickler [registriert](https://developer.microsoft.com/store/register) sein.
2.  Du musst die [Prüfliste für die App-Übermittlung](../publish/app-submissions.md) verwenden.
3.  Die App muss zur [Zertifizierung](../publish/the-app-certification-process.md) eingereicht werden.
Weitere Informationen findest du unter [Veröffentlichen von Windows-Apps und -Spielen](../publish/index.md).