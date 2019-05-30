---
title: Lernprogramm für die ersten Schritte – ein 3D-UWP-Spiel in JavaScript
description: Eine UWP-Spiel für den Microsoft Store, in three.js in JavaScript geschrieben wurde
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: 2e1eba68d1046892a67d779d353fd9b2e6bf224e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366944"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>Erstellen eines 3D-JavaScript-Spiels mit three.js

## <a name="introduction"></a>Einführung

Für Webentwickler oder JavaScript-Tüftler ist die Entwicklung von UWP-Apps mit JavaScript eine einfache Möglichkeit, ihre Apps auf der ganzen Welt zu vertreiben. Sie müssen sich dadurch nicht um das Lernen einer Sprache wie C# oder C++ kümmern.

In diesem Beispiel werden wir von den Vorteilen der **three.js**-Bibliothek profitieren. Diese Bibliothek baut auf WebGL auf, einer API, die zum Rendern von 2D- und 3D-Grafiken für Webbrowser verwendet wird. **three.js** verwendet diese komplizierte API und vereinfacht sie, wodurch die 3D-Entwicklung viel einfacher wird. 


Möchten Sie einen Blick auf die App werfen, die wir erstellen werden, bevor Sie weiterlesen? Probieren Sie sie auf CodePen aus!

<iframe height='300' scrolling='no' title='Letzte Dino-Spiel' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Finales Dino-Spiel</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE] 
> Dies ist kein vollständiges Kartenspiel; Es soll veranschaulicht die Verwendung von JavaScript und Drittanbieter Bibliothek, um eine app auf dem Microsoft Store veröffentlichen.


## <a name="requirements"></a>Anforderungen

Um dieses Projekt auszuprobieren, benötigen Sie Folgendes:
-   Einen Windows-Computer (oder einen virtuellen Computer) mit der aktuellen Version von Windows 10.
-   Eine Kopie von Visual Studio. Die kostenlose Visual Studio Community Edition kann von der [Visual Studio-Homepage](https://visualstudio.com/) heruntergeladen werden.
Dieses Projekt verwendet die **three.js**-JavaScript-Bibliothek. **three.js** ist unter der MIT-Lizenz veröffentlicht. Diese Bibliothek ist bereits im Projekt vorhanden (suchen Sie nach `js/libs` in der Projektmappen-Explorer-Ansicht). Weitere Informationen zu dieser Bibliothek finden Sie auf der [**three.js**](https://threejs.org/)-Startseite.

## <a name="getting-started"></a>Erste Schritte

Der vollständige Quellcode für die App befindet sich in [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d).

Die einfachste Möglichkeit ist es, GitHub zu besuchen, auf die grüne Schaltfläche „Clone or download“ zu klicken und „In Visual Studio öffnen“ auszuwählen. 

![Schaltfläche „clone or download“](images/3dclone.png)

Wenn Sie das Projekt nicht klonen möchten, können Sie es als Zip-Datei herunterladen.
Nachdem die Projektmappe in Visual Studio geladen wurde, sehen Sie mehrere Dateien, einschließlich:
-   Images/ – ein Ordner, der die verschiedenen Symbole enthält, die für UWP-Apps erforderlich sind.
- css / – ein Ordner, der die CSS enthält, die verwendet werden.
-   js / – ein Ordner mit den JavaScript-Dateien. Die main.js-Datei ist unser Spiel, während die anderen Dateien Drittanbieterbibliotheken sind.
-   models/ – ein Ordner mit den 3D-Modellen. Für dieses Spiel gibt es nur ein Modell für den Dinosaurier.
-   index.html – die Webseite, die den Spiel-Renderer hostet.

Jetzt können Sie das Spiel ausführen!

Drücken Sie F5, um die App zu starten. Sie sollten ein Fenster sehen, das Sie auffordert, auf den Bildschirm zu klicken. Sie sehen außerdem einen Dinosaurier, der sich im Hintergrund bewegt. Schließen Sie das Spiel, damit wir jetzt die App und ihre wichtigsten Komponenten untersuchen können.

> [!NOTE] 
> Hat etwas nicht geklappt? Stellen Sie sicher, dass Sie Visual Studio mit Web-Unterstützung installiert haben. Sie können dies überprüfen, indem Sie ein neues Projekt erstellen. Wenn es keine Unterstützung für JavaScript gibt, müssen Sie Visual Studio neu installieren und das Feld „Microsoft Web Developer Tools“ aktivieren.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Wenn Sie das Spiel starten, werden Sie dazu aufgefordert, auf den Bildschirm zu klicken. Die [Pointer-Lock-API](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) wird verwendet, damit Sie die Maus verwenden können. Das Bewegen erfolgt durch Drücken der Pfeiltasten W, A, S, D.
Das Ziel dieses Spiels ist es, sich vom Dinosaurier fernzuhalten. Sobald der Dinosaurier nah genug bei Ihnen ist, beginnt er, Ihnen nachzujagen, bis Sie entweder außerhalb seines Bereichs gelangen oder das Spiel verlieren.

### <a name="1-setting-up-your-initial-html-file"></a>1. Das Einrichten Ihrer ersten HTML-Datei

In **index.html** müssen Sie zuerst etwas HTML-Code hinzufügen. Diese Datei ist die Standardwebseite, die unsere App enthält.

Jetzt werden wir diese mit den Bibliotheken einrichten, die wir verwenden werden, und der `div` (mit dem Namen `container`), die wir verwenden, um unsere Grafiken zu rendern. Außerdem legen wir fest, dass diese zu unserer **main.js** (unserem Spielcode) zeigt.


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


Nachdem unsere Starter-HTML nun einsatzbereit ist, können wir uns **main.js** zuwenden und einige Grafiken erstellen!

### <a name="2-creating-your-scene"></a>2. Erstellen Ihre Szene

Im Abschnitt zu exemplarischen Vorgehensweisen werden wir die Grundlage für das Spiel hinzufügen.

Beginnen wir damit, eine `scene` auszuarbeiten. Eine `scene` in **three.js** ist der Ort, in dem Ihre Kamera, Objekte und Lichter hinzugefügt werden. Sie benötigen auch einen Renderer, der aufnimmt, was die Kamera in der Szene sieht, und dies angezeigt.

In **main.js** erstellen wir eine Funktion, die all dies erledigt und `init()` heißt. Sie ruft einige zusätzliche Funktionen auf:

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

Die anderen Funktionen, die wir erstellen müssen, sind:
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- Einheitskonvertierungsfunktionen

#### <a name="createmazecubes"></a>createMazeCubes()

Die `createMazeCubes()`-Funktion wird einen einfachen Würfel zu unserer Szene hinzufügen. Später verwenden wir die Funktion, um viele Würfel hinzuzufügen und unser Labyrinth zu erstellen.

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

Die `addLights()`-Funktion ist eine einfache Funktion, welche die Erstellung unserer Lichter gruppiert und diese zur Szene hinzufügt.

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

Die `onWindowResize`-Funktion wird aufgerufen, wenn der Ereignis-Listener hört, dass ein `resize`-Ereignis ausgelöst wurde. Dies geschieht, wenn der Benutzer die Größe des Fensters anpasst. In diesem Fall möchten wir sicherstellen, dass das Bild proportional bleibt und im gesamten Fenster angezeigt werden kann.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

Zuletzt benötigen wir unsere `animate()`-Funktion, die auch die `render()`-Funktion aufrufen wird. Die [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame)-Funktion wird verwendet, um unseren Renderer kontinuierlich zu aktualisieren. Später verwenden wir diese Funktionen, um unseren Renderer mit Animationen wie dem Bewegen im Labyrinth zu aktualisieren.

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

#### <a name="unit-conversion-functions"></a>Einheitskonvertierungsfunktionen

In **three.js** werden Drehungen im Bogenmaß gemessen. Um dies für uns zu vereinfachen, fügen wir einige Funktionen hinzu, damit wir einfach zwischen Grad und Bogenmaß/Radianten konvertieren können. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

Wer würde sich merken, dass 30 Grad 523 Radianten sind? Es ist sehr viel einfacher, `degreesToRadians(30)` durchzuführen, um unsere Drehung zu erhalten, was in unserer `createMazeCubes()`-Funktion verwendet wird.

___

Das war relativ viel Code, aber nun haben wir einen wunderschönen Würfel, der zu unserem `container` gerendert wird! Sehen Sie sich die Ergebnisse in CodePen an.

Sie können den gesamten JavaScript-Inhalt kopieren und in diesen in CodePen einfügen, wenn Probleme aufgetreten sind, oder diesen bearbeiten, um die Lichter anzupassen und einige Farben zu ändern. 

<iframe height='300' scrolling='no' title='Lichter "," Kamera "," Cube "!' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Finden Sie unter dem Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>Beleuchtung, Kamera, Cube!</a> von Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="3-making-the-maze"></a>3. Machen den Irrgarten

Auch wenn das Ansehen eines Würfels bereits atemberaubend ist, ist ein ganzes Labyrinth aus Würfeln noch beeindruckender! Es ist ein ziemlich bekanntes Geheimnis in der Spiele-Community, dass eine der schnellsten Möglichkeiten für das Erstellen eines Labyrinths das Platzieren von Würfeln mit einem 2D-Array ist.
 
![Labyrinth, das mit einem 2D-Array erstellt wurde](images/dinomap.png)

Das Platzieren von 1ern bei Würfeln und von 0ern bei Leerräumen bietet eine manuelle und einfache Methode zum Erstellen bzw. Optimieren des Labyrinths.

Wir erreichen dies, indem wir unsere alte `createMazeCubes()`-Funktion mit einer Funktion mit geschachtelter Schleife ersetzen, um mehrere Würfel zu erstellen und zu platzieren. Anschließend erstellen wir einen Arraynamen `collidableObjects` und fügen die Würfel für die Kollisionserkennung später in diesem Lernprogramm hinzu:

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

Da wir nun wissen, wie viele Würfel verwendet werden (und wie groß sie sind), können wir jetzt die berechnete `mapSize`-Variable verwenden, um die Dimensionen der Grundebene festzulegen:

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

Der letzte Teil des Labyrinths, den wir hinzufügen, sind die Umfassungswände, die alles beherbergen. Wir werden eine Schleife verwenden, um zwei Ebenen (unsere Wände) gleichzeitig zu erstellen, und verwenden die `mapSize`-Variable, die wir in `createGround()` berechnet haben, um zu bestimmen, wie breit diese sein sollen. Die neuen Wände werden auch zu unserem `collidableObjects`-Array für die zukünftige Kollisionserkennung hinzugefügt:

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


Vergessen Sie nicht, einen Aufruf zu `createGround()` und `createPerimWalls` nach `createMazeCubes()` in Ihrer `init()` -Funktion hinzuzufügen, damit diese kompiliert werden!
___

Wir haben nun ein ansprechendes Labyrinth, das wir betrachten können, können jedoch nicht wirklich einen Eindruck erhalten, wie toll es wirklich ist, da die Kamera nur einen bestimmten Ort anzeigt. Es ist an der Zeit, einen Schritt weiter zu gehen und einige Kamerasteuerelemente hinzuzufügen.

Testen Sie in CodePen Dinge wie das Ändern von Farben der Würfel oder das Entfernen des Bodens, indem Sie `createGround()` in der `init()`-Funktion auskommentieren.


<iframe height='300' scrolling='no' title='Erstellen des Labyrinths' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>Labyrintherstellung</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. Sodass der Player sich mit der Registerkarte

Jetzt ist es an der Zeit, in das Labyrinth zu gehen und uns umzusehen. Dazu verwenden wir die **PointerLockControls.js**-Bibliothek und die Kamera.

Die **PoinerLockControls.js**-Bibliothek verwendet die Maus, um die Kamera in die Richtung zu drehen, in der der Mauszeiger bewegt wird, sodass sich der Spieler umsehen kann. 

Zuerst fügen wir einige neue Elemente zu unserer **index.html**-Datei hinzu:

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

Sie benötigen auch alle CSS in CodePen am Ende dieses Abschnitts. Diese sollten in Ihre **stylesheet.css**-Datei eingefügt werden.

Wechseln Sie zurück zu **main.js**, fügen Sie ein paar neue globale Variablen hinzu; `controls` zum Speichern unseres Controllers, `controlsEnabled`, um den Zustand des Controllers nachzuverfolgen, und `blocker`, um das `blocker`-Element in **index.html** abzurufen:

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


In unserer `init()`-Funktion können wir jetzt ein neues `PoinerLockControls`-Objekt erstellen, dieses unserer `camera` übergeben, und dann die `camera` hinzufügen (Zugriff mit `controls.getObject()`).

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

Die Kamera ist jetzt verbunden, jedoch müssen wir nun Maus und Controller interagieren lassen, damit wir uns umsehen können. 

In dieser Situation kommt die [Pointer-Lock-API](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) ins Spiel, mit der wir die Mausbewegungen mit unserer Kamera verbinden. Die Pointer-Lock-API sorgt dafür, dass die Maus ausgeblendet wird, um ein intensiveres Erlebnis zu erhalten. Durch Drücken der ESC-Taste beenden wir die Maus-Kamera-Verbindung und sorgen dafür, dass die Maus wieder angezeigt wird. Das Hinzufügen der `getPointerLock()`- und `lockChange()`-Funktionen hilft uns, genau das zu tun.

Die `getPointerLock()`-Funktion wartet auf einen Mausklick. Nach dem Klick versucht unser gerendertes Spiel (im `container`-Element), die Kontrolle über die Maus zu erhalten. Wir fügen auch einen Ereignislistener hinzu, um zu erkennen, wenn der Spieler die Sperre aktiviert oder deaktiviert, was wiederum `lockChange()` aufruft. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

Unsere `lockChange()`-Funktion muss die Steuerelemente und das `blocker`-Element deaktivieren oder aktivieren. Wir können den Status der Zeiger-Sperre ermitteln, indem wir überprüfen, ob das Ziel der [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement)-Eigenschaft für Mausereignisse zu unserem `container` festgelegt ist.

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

Jetzt können wir einen Aufruf zu `getPointerLock()` unmittelbar vor unserer `init()`-Funktion hinzufügen.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

Zu diesem Zeitpunkt haben wir nun die Möglichkeit, **uns umzusehen**, aber der echte „Wow“-Faktor ist die Möglichkeit, **sich zu bewegen**. Nun geht es mit Vektoren etwas mathematischer zu, aber was wären 3D-Grafiken ohne etwas Mathematik?

<iframe height='300' scrolling='no' title='Suchen Sie' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>Sich umsehen</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="5-adding-player-movement"></a>5. Hinzufügen von Player-Bewegung

Damit sich unser Spieler bewegen kann, müssen wir an unsere Berechnungen zurückdenken. Wir möchten Geschwindigkeit (Bewegung) auf die `camera` entlang eines bestimmten Vektors (Richtung) anwenden.

Jetzt fügen wir einige globale Variablen zum Nachverfolgen der Richtung hinzu, in die sich der Spieler bewegt, und legen einen anfänglichen Geschwindigkeitsvektor fest:

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

Legen Sie zu Beginn der `init()`-Funktion für `clock` ein neues `Clock`-Objekt fest. Wir verwenden dies, um die Zeitdifferenz (Delta) nachzuverfolgen, die benötigt wird, um neue Frames zu rendern. Sie müssen auch einen Aufruf zu `listenForPlayerMovement()` hinzufügen, was die Benutzereingaben erfasst. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

Unsere `listenForPlayerMovement()`-Funktion ist das, was unsere Richtungszustände ändern wird. Am Ende der Funktion haben wir zwei Ereignislistener, die darauf warten, dass Tasten gedrückt und losgelassen werden. Nachdem eines dieser Ereignisse ausgelöst wurde, werden wir anschließend überprüfen, ob es sich um eine Taste handelt, mit der wir eine Bewegung beenden oder auslösen möchten.

Für dieses Spiel haben wir festgelegt, dass sich der Spieler mit den Tasten W, A, S und D oder den Pfeiltasten bewegen kann.

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

Da wir nun bestimmen können, welche Richtung der Benutzer einschlagen möchte (das wird jetzt als `true` in einem der globalen Richtungsflags gespeichert), ist es Zeit für eine Aktion. Diese Aktion wird in Form der `animatePlayer()`-Funktion ausgeführt.

Diese Funktion wird innerhalb von `animate()` aufgerufen und an `delta` übergeben, um die Zeitdifferenz zwischen Frames zu ändern, damit die Bewegung bei Änderungen der Framerate nicht asynchron erscheint:

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

Jetzt ist es Zeit für den unterhaltsamen Teil! Unser Bewegungsvektor (`playerVeloctiy`) verfügt über drei Parameter `(x, y, z)`, wobei `y` die vertikale Bewegung ist. Da wir in diesem Spiel nicht springen, arbeiten wir nur mit den Parametern `x` und `z`. Dieser Vektor ist anfänglich auf (0, 0, 0) festgelegt.

Wie im folgenden Code dargestellt, erfolgen einige Checks, um zu sehen, welche Richtungsflags auf `true` gedreht sind. Wenn wir die Richtung haben, addieren wir zu `x` und `y` oder subtrahieren davon, um Bewegung in die entsprechende Richtung anzuwenden. Wenn keine Bewegungstasten gedrückt werden, wird der Vektor zurück zu `(0, 0, 0)` gesetzt.


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

Am Ende werden die aktualisierten `x`- und `y`-Werte als Übersetzungen auf die Kamera angewendet, damit sich der Spieler tatsächlich bewegen kann.

---

Herzlichen Glückwunsch! Sie haben nun eine vom Spieler gesteuerte bewegbare Kamera, mit der sich der Spieler umsehen kann. Wir können immer noch direkt durch die Wände laufen, darum kümmern wir uns jedoch etwas später. Als Nächstes fügen wir unseren Dinosaurier hinzu.

<iframe height='300' scrolling='no' title='Verschieben' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Finden Sie unter dem Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>verschieben</a> von Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE]
> Wenn Sie diese Steuerelemente in Ihrer UWP-App verwenden, bemerken Sie möglicherweise verzögerte Bewegungen und nicht registrierte `keyUp`-Ereignisse. Wir untersuchen dies gerade und hoffen, diesen Teil des Beispiels bald beheben zu können!

### <a name="6-load-that-dino"></a>6. Laden Sie diese Dino!

Wenn Sie das Repository dieses Projekts geklont oder heruntergeladen haben, sehen Sie einen `models`-Ordner, der `dino.json` enthält. Diese JSON-Datei ist ein 3D-Dinosaurier-Modell, das von Blender erstellt und exportiert wurde.


Wir müssen weitere globale Variablen hinzufügen, damit dieser Dino verwendet werden kann:

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

Nachdem wir unseren `JSONLoader` erstellt haben, geben wir den Pfad an unsere **dino.json**-Datei weiter, sowie einen Rückruf mit der Geometrie und dem Material, das aus der Datei abgerufen wurde.
Das Laden des Dinos ist eine asynchrone Aufgabe, was bedeutet, dass nichts gerendert wird, bis der Dino vollständig geladen ist. In unserer Datei **index.html** haben wir die Zeichenfolge im `instructions`-Element in `"Loading..."` geändert, damit der Spieler weiß, dass etwas ausgeführt wird.

Aktualisieren Sie nach dem Laden des Dinos das `instructions`-Element mit den tatsächlichen Spielanweisungen, und verschieben Sie die `animate()`-Funktion vom Ende von `init()` an das Ende des Funktionsrückrufs (unten angezeigt):

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

Wir haben nun unser Dinomodell geladen. Probieren Sie es aus!

<iframe height='300' scrolling='no' title='Hinzufügen der dino' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>Hinzufügen des Dinos</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="7-move-that-dino"></a>7. Verschieben Sie diese Dino!

Künstliche Intelligenz für ein Spiel zu erstellen, kann äußerst komplex werden, weshalb wir für dieses Beispiel ein einfaches Bewegungsverhalten für diesen Dino festlegen. Unser Dino wird geradeaus laufen, an Wänden entlang laufen und in der Ferne im Nebel verschwinden.

Zu diesem Zweck fügen Sie zunächst die globale Variable `dinoVelocity` hinzu.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 Als Nächstes rufen Sie die `animateDino()`-Funktion aus der `animation()`-Funktion auf und fügen den folgenden Code hinzu:

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

Zu beobachten, wie der Dino verschwindet, ist nicht sehr unterhaltsam, aber wenn wir Kollisionserkennung hinzufügen, wird es interessanter.

<iframe height='300' scrolling='no' title='Verschieben Sie die Dino - kein Konflikt' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>Bewegen des Dinos – kein Kollision</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. Kollisionserkennung für den player

Jetzt können sich der Spieler und der Dino bewegen, jedoch gibt es immer noch das lästige Problem, dass jeder durch Wände gehen kann. Als wir unsere Würfel und Wände weiter oben in diesem Lernprogramm hinzugefügt haben, übertrugen wir sie in das `collidableObjects`-Array. Dieses Array verwenden wir, um festzustellen, ob ein Spieler zu nahe an etwas ist, wo er nicht durchlaufen kann.

Wir werden Raycasters verwenden, um zu bestimmen, wann eine Überschneidung stattfinden wird. Sie können sich einen Raycaster als Laserstrahl vorstellen, der aus der Kamera in eine angegebene Richtung geht und zurückmeldet, wenn er ein Objekt antrifft, und angibt, wie weit es entfernt ist.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

Wir erstellen eine neue Funktion namens `detectPlayerCollision()`, die `true` zurückgibt, wenn der Spieler zu nahe an einem kollisionsfähigen Objekt ist.
Für den Spieler werden wir ein Raycaster verwenden, das ändert, auf welche Richtung es zeigt, je nachdem, in welche Richtung der Spieler geht.

Zu diesem Zweck erstellen wir `rotationMatrix`, eine nicht definierte Matrix. Wenn wir überprüfen, in welche Richtung wir gehen, erhalten wir entweder eine definierte `rotationMatrix` oder eine nicht definierte, wenn Sie weitergehen.
Wenn sie definiert ist, wird `rotationMatrix` auf die Richtung der Steuerelemente angewendet. 

Dann wird ein Raycaster erstellt, das bei der Kamera beginnt und in die `cameraDirection`-Richtung reicht.


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

Unsere `detectPlayerCollision()`-Funktion basiert auf der `rayIntersect()`-Hilfsfunktion.
Diese nimmt einen Raycaster und einen Wert, der darstellt, wie nah wir zu einem Objekt im `collidableObjects`-Array gehen können, bevor eine Kollision auftritt.

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

Nachdem wir nun ermitteln können, wann eine Kollision auftreten wird, können wir unsere `animatePlayer()`-Funktion optimieren:

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

Wir haben jetzt die Kollisionserkennung für den Spieler und versuchen jetzt, gegen einige Wänden zu rennen!

<iframe height='300' scrolling='no' title='Bewegen des Players - Konflikt' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>Bewegen des Spielers – kein Kollision</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. Kollisionserkennung und die Animation für Dino Esposito

Jetzt müssen wir verhindern, dass unser Dino durch Wände läuft, und stattdessen festlegen, dass er in eine zufällige Richtung wechselt, sobald er einem kollisionsfähigen Objekt zu nahe kommt.

Lassen Sie uns zuerst herausfinden, wann unser Dino kollidiert. 

Wir müssen eine andere globale Variable für die Kollisionserkennung festlegen:

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

Nachdem wir angegeben haben, bei welchem Abstand unser Dino kollidieren soll, fügen wir eine ähnliche Funktion wie `detectPlayerCollision()` hinzu, aber etwas einfacher.
Die `detectDinoCollision`-Funktion ist einfach, da wir immer ein Raycaster haben, das direkt aus der Vorderseite des Dinos kommt. Es ist keine Drehung erforderlich wie bei der Spielerkollision.

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

Werfen wir einen Blick darauf, wie unsere endgültige `animateDino()`-Funktion aussehen wird, wenn sie Kollisionserkennung enthält:


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

Wir möchten, dass sich unser Dino um -90, 90 oder um 180 Grad dreht. Dazu haben wir das `directionMultiples`-Array erstellt, das diese Zahlen erzeugt, wenn es mit 90 multipliziert wird.
Für eine zufällige Auswahl der Drehungsgradzahl haben wir die `getRandomInt()`-Hilfsfunktion hinzugefügt, die einen Wert von 0, 1 oder 2 nimmt, welcher einen zufälligen Index des Arrays darstellt.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

Nachdem dieser Vorgang abgeschlossen ist, multiplizieren wir den zufälligen Index des Arrays mit 90, um die Gradanzahl (in Radianten konvertiert) der Drehung zu erhalten.
Durch Hinzufügen dieses Werts zur `y`-Drehung des Dinos mit `dino.rotation.y += randomDirection;` macht der Dino jetzt bei Kollisionen zufällige Drehungen.


---

Wir haben es geschafft! Wir haben jetzt einen Dino mit künstlicher Intelligenz, der sich in unserem Labyrinth bewegen kann!

<iframe height='300' scrolling='no' title='Verschieben Sie die Dino - Konflikt' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Finden Sie unter dem Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>Verschieben der Dino - Konflikt</a> von Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="10-starting-the-chase"></a>10. Starten Sie den chase

Sobald der Dino innerhalb einer bestimmten Entfernung zum Spieler ist, möchten wir ihm nachjagen. Da dies nur ein Beispiel ist, gibt es keine erweiterten Algorithmen für den Dino, der vom Spieler verfolgt wird. Stattdessen wird der Dino den Spieler ansehen und zu ihm laufen. In einem offenen Teil des Labyrinths funktioniert dies gut, aber der Dino bleibt stecken, wenn eine Wand im Weg ist.

In unserer `animate()`-Funktion fügen wir eine boolesche Variable hinzu, die durch das festgelegt ist, was von `triggerChase()` zurückgegeben wird:

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

Unsere `triggerChase`-Funktion überprüft, ob der Spieler im Jagdbereich des Dinos ist, und sorgt dann dafür, dass der Dino immer den Spieler ansieht, wodurch er in Richtung des Spielers gehen kann. 

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

In der zweiten Hälfte von `triggerChase` geht es um das Anzeigen von Text, der dem Spieler mitteilt, wie weit der Dino entfernt ist. Wir sehen uns auch `CATCHOFFSET` an, womit wir angeben, wie weit `0` entfernt sein sollte. Wenn wir keinen Ausgleich hätten, wäre `0` direkt über dem Spieler, was nicht sehr passend für das Ende wäre.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

An dieser Stelle haben wir einen wilden Dinosaurier, der beginnt, den Spieler zu verfolgen, sobald dieser ihm zu nahe kommt, und nicht anhält, bis sich seine Position über dem Spieler befindet.
Der letzte Schritt ist es, einige Bedingungen für das Spielende hinzuzufügen, sobald der Dino `CATCHOFFSET` Einheiten entfernt ist.

<iframe height='300' scrolling='no' title='Die chase' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie im Pen zu <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>Die Jagd</a> von Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="11-ending-the-game"></a>11. Ende des Spiels


Wir haben einen langen Weg von einem einfachen Würfel aus bestritten, und jetzt ist es Zeit, zum Ende zu kommen.

Zuerst legen wir eine Variable fest, um nachzuverfolgen, ob das Spiel vorbei ist oder nicht:

```javascript
var gameOver = false;
```

Jetzt müssen wir unsere `animate()`-Funktion ein letztes Mal aktualisieren, um zu überprüfen, ob der Dino zu nahe am Spieler ist.
Ist der Dino zu nah, beginnen wir eine neue Funktion namens `caught()` und beenden die Bewegung des Spielers und Dinos, falls nicht, geht es wie gewohnt weiter, und Spieler und Dino bewegen sich weiter.

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

Wenn der Dino den Spieler fängt, zeigt `caught()` unser `blocker`-Element an und aktualisiert den Text, um anzugeben, dass das Spiel verloren wurde.
Die `gameOver`-Variable ist ebenfalls auf `true` festgelegt, wodurch wir erfahren, dass das Spiel vorbei ist.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


Da wir nun wissen, ob das Spiel vorbei ist oder nicht, können wir eine diesbezügliche Überprüfung zu unserer `lockChange()`-Funktion hinzufügen.
Wenn der Benutzer ESC drückt, nachdem das Spiel vorbei ist, können wir `location.reload` hinzufügen, um das Spiel erneut zu starten.

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

Das ist alles! Es war ein langer Weg, aber nun haben wir ein Spiel mit **three.js** erstellt.

Gehen Sie zurück an den Anfang der Seite, wo Sie den [finalen CodePen](#introduction) finden!


## <a name="publishing-to-the-microsoft-store"></a>Veröffentlichung in den Microsoft Store
Nachdem Sie eine UWP-app verfügen, ist es möglich, die in den Microsoft Store (vorausgesetzt, dass Sie es zunächst verbessert haben!) veröffentlichen Dazu müssen Sie einige Schritte durchführen.

1.  Sie müssen als Windows-Entwickler [registriert](https://developer.microsoft.com/store/register) sein.
2.  Verwenden Sie die [Prüfliste für App-Übermittlung](https://docs.microsoft.com/windows/uwp/publish/app-submissions).
3.  Die App muss zur [Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process) eingereicht werden.
Weitere Informationen finden Sie unter [Veröffentlichen Ihrer UWP-app](https://developer.microsoft.com/store/publish-apps).

