---
title: Hinzufügen von WebVR-Support zu einem 3D-Babylon.js-Spiel
description: Hier erfahren Sie, wie Sie einem vorhandenen Babylon.js-3D-Spiel WebVR-Unterstützung hinzufügen.
ms.date: 11/29/2017
ms.topic: article
keywords: WebVR, Edge, Webentwicklung, Babylon, Babylonjs, babylon.js, Javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638555"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Hinzufügen von WebVR-Support zu einem 3D-Babylon.js-Spiel

Wenn Sie mithilfe von Babylon.js ein 3D-Spiel erstellt haben und vermuten, dass es in virtueller Realität (VR) großartig aussehen könnte, befolgen Sie die einfachen Schritte in diesem Lernprogramm, um Ihre Idee umzusetzen.

Dem hier gezeigten Spiel wird von uns WebVR-Unterstützung hinzugefügt. Schließen Sie nun einen Xbox-Controller an, um es auszuprobieren!


<iframe height='300' scrolling='no' title='Babylon.js Dino-Spiel mithilfe von Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Finden Sie unter dem Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js Dino-Spiel mithilfe von Babylon.GUI</a> von Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Dies ist ein 3D-Spiel, das gut auf dem Flachbildschirm funktioniert, ist das auch so in VR?
In diesem Lernprogramm behandeln wir die wenigen Schritte, die erforderlich sind, um das Spiel mit WebVR einzurichten und freizugeben. Wir verwenden ein [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality)-Headset, das im Rahmen des für WebVR in Microsoft Edge hinzugefügten Supports eingesetzt werden kann. Wenn wir diese Änderungen auf das Spiel angewendet haben, können Sie davon ausgehen, dass es auch unter anderen Browser/Headset-Kombinationen funktioniert, die WebVR unterstützen.



## <a name="prerequisites"></a>Voraussetzungen

- Ein Text-Editor (z. B. [Visual Studio Code](https://code.visualstudio.com/download))
- Ein Xbox-Controller, der an den Computer angeschlossen ist
- Windows 10 Creators Update
- Ein Computer mit den [Mindestanforderungen, um Windows Mixed Reality auszuführen](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Ein Windows Mixed Reality-Gerät (Optional) 



## <a name="getting-started"></a>Erste Schritte

Die einfachste Möglichkeit ist es, die [Windows-Tutorials-Web-Repository GitHub](https://github.com/Microsoft/Windows-tutorials-web) zu besuchen, auf die grüne Schaltfläche **Clone or download** zu klicken und **In Visual Studio öffnen** auszuwählen.

![Schaltfläche „clone or download“](images/3dclone.png)

Wenn Sie das Projekt nicht klonen möchten, können Sie es als Zip-Datei herunterladen.
Ihnen stehen dann zwei Ordner zur Verfügung; [vor](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) und [nach](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). Der Ordner „vor” ist unser Spiel, bevor VR-Features hinzugefügt werden, und der Ordner „nach” ist das fertige Spiel mit VR-Unterstützung.

Die Ordner „vor” und „nach” enthalten diese Dateien:
-   **Texturen /** : ein Ordner, die im Spiel verwendete Bilder enthält.
-   **CSS /** : ein Ordner mit CSS für das Spiel.
-   **Js /** : ein Ordner mit der JavaScript-Dateien. Die main.js-Datei ist unser Spiel, während die anderen Dateien die verwendeten Bibliotheken sind.
-   **Modelle /** : ein Ordner mit dem 3D-Modelle. Für dieses Spiel gibt es nur ein Modell; das für den Dinosaurier.
-   **"Index.HTML"** -Webseite, die das Spiel Renderer hostet. Das Spiel wird gestartet, wenn Sie diese Seite in Microsoft Edge öffnen.

Sie können beide Versionen des Spiels testen, öffnen Sie die entsprechende Datei "Index.HTML"-Dateien in Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Das Mixed-Reality-Portal

Wenn Sie mit Windows Mixed Reality nicht vertraut sind und das Windows 10 Creators Update auf einem Computer mit einer kompatiblen Grafikkarte installiert haben, versuchen Sie, die **Mixed-Reality-Portal**-App aus dem Startmenü in Windows 10 zu öffnen.

![Mixed-Reality-Portal-Suche](images/mixed-reality-portal.png)

Wenn Sie alle Anforderungen erfüllt haben, können Sie die Entwicklerfeatures aktivieren und einen an dem Computer angeschlossenen Windows Mixed Reality-Kopfhörer simulieren. Wenn Sie tatsächlich einen Kopfhörer bei sich in der Nähe haben, schließen Sie ihn an, und führen Sie das Setup aus.

> [!IMPORTANT]
> Das Mixed-Reality-Portal muss während dieses Lernprogramms stets geöffnet sein.

Sie nun können WebVR mit Microsoft Edge auftreten.

## <a name="2d-ui-in-a-virtual-world"></a>2D-UI in einer virtuellen Welt

>[!NOTE]
> Ziehen Sie die [ **vor** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) Ordner des Starter-Beispiels zu erhalten.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) ist eine geeignete VR-Bibliothek, sodass Sie zum einfachen Erstellen interaktiver Benutzeroberflächen, eignen sich gut für VR und nicht-VR zeigt.
Eine Erweiterung von Babylon.js begonnen die `GUI` -Bibliothek ist verwendeten Throuhout im Beispiel zum Erstellen von 2D-Elementen.


Eine Direct2D Text `GUI` Element erstellt werden kann, mit wenigen Zeilen je nachdem wie viele Attribute, die Sie optimieren möchten.
Der folgende Codeausschnitt befindet sich bereits in unserem [ **vor** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) Beispiel, aber wir Exemplarische Vorgehensweise, was geschieht.
Wir stellen Sie zunächst eine [ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) Objekts herstellen, was die grafische Benutzeroberfläche behandelt. Das Beispiel setzt diesen Wert auf `CreateFullScreenUI()`, d. h. unserer Benutzeroberfläche behandelt den gesamten Bildschirm. Mit `AdvancedDynamicTexture` erstellt haben, wir nehmen Sie dann ein 2D Textfeld, das angezeigt wird, nach dem Start der Spiele mit `GUI.Rectanlge()` und `GUI.TextBlock()`.


Dieser Code wird hinzugefügt, in [ **"Main.js"**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Diese Benutzeroberfläche wird angezeigt, nachdem Sie erstellt, jedoch können ein- und ausgeschaltet werden, mit `isVisible` je nachdem, was in's Spiel passiert.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Erkennen von Headsets

Es ist empfehlenswert, für die VR-Anwendungen auf zwei Arten von Kameras haben, sodass mehrere Szenarien unterstützt werden können. Für dieses Spiel werden wir eine Kamera unterstützen, für die ein funktionierender und angeschlossener Kopfhörer erforderlich ist, und eine weitere, die keinen Kopfhörer verwendet. Um zu ermitteln, welches das Spiel verwenden wird, müssen wir zuerst überprüfen, ob ein Kopfhörer erkannt wurde. Zu diesem Zweck verwenden wir [ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Fügen Sie folgenden Code, der oben genannten `window.addEventListener('DOMContentLoaded')` in **"Main.js"**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Mit den in der `headset`-Variable gespeicherten Informationen können wir jetzt die für den Benutzer geeignete Kamera auswählen.


## <a name="creating-and-selecting-the-initial-camera"></a>Erstellen und Auswählen der anfänglichen Kamera

Mit Babylon.js, WebVR kann hinzugefügt werden schnell mithilfe der [ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera). Diese Kamera kann Tastatureingaben erfassen und ermöglicht es Ihnen, anhand eines VR-Kopfhörers die Drehung Ihres „Kopfs” zu steuern.


### <a name="step-1-checking-for-headsets"></a>Schritt 1: Headsets werden gesucht

Für unsere fallback Kamera, wir verwenden die [ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera) , die derzeit in der ursprünglichen Spiel verwendet wird.

Wir überprüfen, unsere `headset` Variable zu bestimmen, ob wir verwenden können, die `WebVRFreeCamera` Kamera.

Ersetzen Sie `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` mit dem folgenden Code.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>Schritt 2: Aktivieren die "webvrfreecamera"
Um diese Kamera in den meisten Browsern zu aktivieren, muss der Benutzer einige Interaktionen ausführen, die die virtuelle Oberfläche erfordern.
Wir binden diese Funktion bis zu einem Mausklick.


Fügen Sie den Code in der `createScene()`-Funktion nach `camera.applyGravity = true;` ein.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Ein Klick im Spiel jetzt eine Eingabeaufforderung wie folgt erstellt oder zeigt das Spiel in den Kopfhörer sofort, wenn der Benutzer mit die Aufforderung vor akzeptiert hat.

![Immersive Aufforderung](images/immersiveview.png)

Wir können auch hinzufügen, einen Codeabschnitt, der angezeigt wird der `UniversalCamera` anzeigen, bevor wir zu wechseln unsere `WebVRFreeCamera`, damit der Benutzer das Spiel, anstatt ein blaues Fenster anzuzeigen. 

Fügen Sie die folgenden nach `engine.runRenderLoop(function () {`.
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

Da die `WebVRFreeCamera` unterstützt keine anfänglich Gamepads, ordnen wir unsere Gamepad-Schaltflächen auf den Pfeiltasten der Tastatur. Wir erreichen dies, indem zuwenden der `inputs` Eigenschaft der Kamera. Durch das Zuordnen der Codes für den linken Analogstick nach oben, unten, links und rechts zu den entsprechenden Pfeiltasten ist unser Gamepad wieder einsatzbereit.


Fügen Sie diesen Code unter der `scene.onPointerDown = function() {...}` aufrufen.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Schritt 4: Probieren sie es!

Wenn wir öffnen **"Index.HTML"** unsere Kopfhörer und game Controller angeschlossen sind, ein Linker Klick auf die blaue Spielfenster angezeigt unser Spiel VR-Modus wechselt. Legen Sie Ihre Kopfhörer auf, um die Ergebnisse zu überprüfen. 


<iframe height='300' scrolling='no' title='Babylon.js Dino-Spiel mithilfe von Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Finden Sie unter dem Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js Dino-Spiel mithilfe von Babylon.GUI - WebVR</a> von Microsoft Edge-Dokumentation (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Abschluss

Gratulation! Sie haben nun ein vollständiges Babylon.js-Spiel mit WebVR-Unterstützung. Von hier aus können Sie das Gelernte nutzen, um ein noch besseres Spiel zu erstellen.
