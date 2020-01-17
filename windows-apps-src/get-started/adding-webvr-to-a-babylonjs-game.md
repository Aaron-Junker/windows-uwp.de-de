---
title: Hinzufügen von WebVR-Support zu einem Babylon.js-3D-Spiel
description: Es wird beschrieben, wie du einem vorhandenen Babylon.js-3D-Spiel WebVR-Unterstützung hinzufügst.
ms.date: 11/29/2017
ms.topic: article
keywords: WebVR, Edge, Webentwicklung, Babylon, Babylonjs, Babylon.js, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: ff350f8ce08f566b8c95c3c46faad330923e4b2e
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685202"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Hinzufügen von WebVR-Support zu einem Babylon.js-3D-Spiel

Wenn du mithilfe von Babylon.js ein 3D-Spiel erstellt hast und der Meinung bist, dass es gut für virtuelle Realität (VR) geeignet ist, kannst du die einfachen Schritte dieses Tutorials ausführen, um deine Idee umzusetzen.

In diesem Tutorial werden die wenigen Schritte beschrieben, die erforderlich sind, um ein 3D-Spiel für WebVR einzurichten. Wir verwenden ein [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality)-Headset, mit dem die zusätzliche WebVR-Unterstützung in Microsoft Edge genutzt werden kann. Nachdem wir diese Änderungen auf das Spiel angewendet haben, kannst du davon ausgehen, dass es auch für andere Browser/Headset-Kombinationen funktioniert, für die WebVR unterstützt wird.



## <a name="prerequisites"></a>Voraussetzungen

- Ein Text-Editor (z. B. [Visual Studio Code](https://code.visualstudio.com/download))
- Ein Xbox-Controller, der an den Computer angeschlossen ist
- Windows 10 Creators Update
- Ein Computer mit den [Mindestanforderungen für die Ausführung von Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality/immersive_headset_setup)
- Ein Windows Mixed Reality-Gerät (optional) 



## <a name="getting-started"></a>Erste Schritte

Am einfachsten kannst du beginnen, indem du zum [GitHub-Repository „Windows-tutorials-web“](https://github.com/Microsoft/Windows-tutorials-web) navigierst, auf die grüne Schaltfläche **Clone or download** klickst und die Option **Open in Visual Studio** wählst.

![Schaltfläche „Clone or download“](images/3dclone.png)

Wenn du das Projekt nicht klonen möchtest, kannst du es als ZIP-Datei herunterladen.
Dir stehen dann zwei Ordner zur Verfügung: [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) und [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). Der Ordner „before“ entspricht unserem Spiel vor dem Hinzufügen von VR-Features, und der Ordner „after“ enthält das fertige Spiel mit VR-Unterstützung.

Die Ordner „before“ und „after“ enthalten diese Dateien:
-   **textures/** : Ein Ordner, der die im Spiel verwendeten Bilder enthält.
-   **css/** : Ein Ordner mit den CSS-Daten für das Spiel.
-   **js/** : Ein Ordner mit den JavaScript-Dateien. Die Datei „main.js“ ist unser Spiel, und die anderen Dateien enthalten die Daten für die verwendeten Bibliotheken.
-   **models/** : Ein Ordner mit den 3D-Modellen. Für dieses Spiel gibt es nur ein Modell (für den Dinosaurier).
-   **index.html**: Die Webseite zum Hosten des Renderers für das Spiel. Das Spiel wird gestartet, wenn du diese Seite in Microsoft Edge öffnest.

Du kannst beide Versionen des Spiels testen, indem du die jeweiligen „index.html“-Dateien in Microsoft Edge öffnest.



## <a name="the-mixed-reality-portal"></a>Mixed Reality-Portal

Falls du mit Windows Mixed Reality noch nicht vertraut bist und Windows 10 Creators Update auf einem Computer mit einer kompatiblen Grafikkarte installiert hast, kannst du versuchen, die App **Mixed Reality-Portal** über das Startmenü von Windows 10 zu öffnen.

![Suche im Mixed Reality-Portal](images/mixed-reality-portal.png)

Wenn du alle Anforderungen erfüllt hast, kannst du die Entwicklerfeatures aktivieren und ein an den Computer angeschlossenes Windows Mixed Reality-Headset simulieren. Falls du ein Headset zur Hand hast, kannst du es anschließen und das Setup ausführen.

> [!IMPORTANT]
> Das Mixed Reality-Portal muss während dieses Tutorials immer geöffnet sein.

Jetzt ist alles für WebVR mit Microsoft Edge bereit.

## <a name="2d-ui-in-a-virtual-world"></a>2D-Benutzeroberfläche in einer virtuellen Welt

>[!NOTE]
> Klicke auf den Ordner [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before), um das Startbeispiel zu erhalten.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) ist eine für VR geeignete Bibliothek, mit der du einfache interaktive Benutzeroberflächen erstellen kannst, die für VR und andere Displays gut funktionieren.
Eine Erweiterung von Babylon.js, die `GUI`-Bibliothek, wird im gesamten Beispiel verwendet, um 2D-Elemente zu erstellen.


Das 2D-Textelement `GUI` kann mit nur wenigen Zeilen erstellt werden. Der Umfang hängt jeweils davon ab, wie viele Attribute du optimieren möchtest.
Der folgende Codeausschnitt ist im [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)-Beispiel bereits enthalten, aber hier gehen wir noch einmal Schritt für Schritt durch, was passiert.
Zuerst erstellen wir ein [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)-Objekt, um festzulegen, welcher Bereich von der GUI abgedeckt wird. Im Beispiel wird dies auf `CreateFullScreenUI()` festgelegt, um anzugeben, dass unsere Benutzeroberfläche den gesamten Bildschirm abdeckt. Nachdem `AdvancedDynamicTexture` erstellt wurde, erstellen wir mit `GUI.Rectanlge()` und `GUI.TextBlock()` ein 2D-Textfeld, das beim Starten des Spiels angezeigt wird.


Dieser Code wird in [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168) hinzugefügt.
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


Diese Benutzeroberfläche ist nach ihrer Erstellung sichtbar, aber sie kann mit `isVisible` je nach Spielsituation ein- oder ausgeblendet werden.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Erkennen von Headsets

Es ist üblich, dass VR-Anwendungen über zwei Arten von Kameras verfügen, damit mehrere Szenarien unterstützt werden können. Für dieses Spiel soll eine Kamera unterstützt werden, für die ein funktionierendes Headset angeschlossen werden muss, und eine weitere, für die kein Headset verwendet wird. Um ermitteln zu können, welche Kamera für das Spiel verwendet werden soll, müssen wir zuerst überprüfen, ob ein Headset erkannt wurde. Hierfür verwenden wir [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Füge diesen Code oberhalb von `window.addEventListener('DOMContentLoaded')` in **main.js** ein.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Mit den in der `headset`-Variable gespeicherten Informationen können wir jetzt die Kamera auswählen, die für den Benutzer geeignet ist.


## <a name="creating-and-selecting-the-initial-camera"></a>Erstellen und Auswählen der ersten Kamera

Mit Babylon.js kann WebVR schnell hinzugefügt werden, indem [`WebVRFreeCamera`](https://doc.babylonjs.com/api/classes/babylon.webvrfreecamera) verwendet wird. Diese Kamera kann Tastatureingaben erfassen und ermöglicht es dir, ein VR-Headset zum Steuern der Kopfdrehung zu nutzen.


### <a name="step-1-checking-for-headsets"></a>Schritt 1: Durchführen einer Überprüfung auf Headsets

Als Reservekamera verwenden wir [`UniversalCamera`](https://doc.babylonjs.com/api/classes/babylon.universalcamera), die derzeit im ursprünglichen Spiel genutzt wird.

Wir überprüfen unsere `headset`-Variable, um zu ermitteln, ob wir die `WebVRFreeCamera`-Kamera verwenden können.

Ersetze `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` durch den folgenden Code.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>Schritt 2: Aktivieren der WebVRFreeCamera
Zum Aktivieren dieser Kamera muss der Benutzer in den meisten Browsern einige Schritte ausführen, um die virtuelle Oberfläche anzufordern.
Wir verknüpfen diese Funktion mit einem Mausklick.


Füge den Code in der Funktion `createScene()` nach `camera.applyGravity = true;` ein.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Mit einem Klick wird im Spiel nun eine Eingabeaufforderung der folgenden Art erstellt oder das Spiel sofort im Headset angezeigt, falls der Benutzer die Aufforderung bereits bestätigt hat.

![Immersive Eingabeaufforderung](images/immersiveview.png)

Wir können auch Code hinzufügen, mit dem die Ansicht `UniversalCamera` angezeigt wird, bevor wir zu `WebVRFreeCamera` wechseln. So kann der Benutzer das Spiel verfolgen und muss nicht auf ein blaues Fenster schauen. 

Füge nach `engine.runRenderLoop(function () {` Folgendes hinzu:
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>Schritt 3: Hinzufügen von Gamepad-Unterstützung

Da für `WebVRFreeCamera` anfänglich keine Gamepads unterstützt werden, ordnen wir die Gamepad-Tasten den Pfeiltasten auf der Tastatur zu. Hierfür nutzen wir die `inputs`-Eigenschaft der Kamera. Durch das Zuordnen der Codes für „Linker analoger Joystick nach oben, unten, links und rechts“ zu den entsprechenden Pfeiltasten ist unser Gamepad wieder einsatzbereit.


Füge diesen Code unter dem `scene.onPointerDown = function() {...}`-Aufruf hinzu.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Schritt 4: Ausprobieren!

Wenn wir **index.html** bei angeschlossenem Headset und Gamecontroller öffnen, können wir mit einem Linksklick auf das blaue Spielfenster in den VR-Modus wechseln! Setze das Headset auf, um die Ergebnisse zu überprüfen. 



## <a name="conclusion"></a>Schlussbemerkung

Herzlichen Glückwunsch! Du verfügst jetzt über ein vollständiges Babylon.js-Spiel mit WebVR-Unterstützung. Das Gelernte kannst du nun nutzen, um das Spiel zu erweitern oder ein noch besseres Spiel zu erstellen.
