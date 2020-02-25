---
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: Erfahren Sie mehr über Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in JavaScript/HTML-Apps.
title: Handbuch zur Problembehandlung für HTML und JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, AdControl, Problembehandlung, HTML, Javascript
ms.localizationpriority: medium
ms.openlocfilehash: e9427218bc2ee6e7de7d5cb367da21d7b4bbfe30
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507094"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>Handbuch zur Problembehandlung für HTML und JavaScript

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieses Thema enthält Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in JavaScript/HTML-Apps.

* [HTML](#html)
  * [Adcontrol wird nicht angezeigt.](#html-notappearing)
  * [Schwarze Felder blinkt und verschwinden](#html-blackboxblinksdisappears)
  * [Werbung wird nicht aktualisiert](#html-adsnotrefreshing)

* [Ja](#js)
  * [Adcontrol wird nicht angezeigt.](#js-adcontrolnotappearing)
  * [Schwarze Felder blinkt und verschwinden](#js-blackboxblinksdisappears)
  * [Werbung wird nicht aktualisiert](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)** -Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass der JavaScript-Verweis vorhanden ist. Ohne den Verweis ad.js im Abschnitt &lt;head&gt; (nach dem Verweis default.js) kann **AdControl** nicht angezeigt werden, und während der Erstellung tritt ein Fehler auf.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und der Ad Unit ID identisch sein, die Sie im Partner Center abgerufen haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  Überprüfen Sie die Eigenschaften **height** und **width**. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  Überprüfen Sie die Elementpositionierung. [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) muss sich im sichtbaren Bereich befinden.

6.  Überprüfen Sie die Eigenschaft **visibility**. Diese Eigenschaft darf nicht auf reduziert oder ausgeblendet festgelegt sein. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  Überprüfen Sie die Eigenschaft **position**. Die Eigenschaft „position“ muss auf einen geeigneten Wert abhängig von den anderen Eigenschaften des Elements festgelegt werden (beispielsweise Rändern im übergeordneten Element und z-index). Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  Überprüfen Sie die Eigenschaft **z-index**. Die Eigenschaft **z-index** muss hoch genug festgelegt werden, sodass **AdControl** stets oberhalb anderer Elemente angezeigt wird. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  Überprüfen Sie die externen Stylesheets. Wenn im Element **AdControl** Eigenschaften über ein externes Stylesheet festgelegt werden, müssen alle oben genannten Eigenschaften ordnungsgemäß festgelegt worden sein.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. Überprüfen Sie das übergeordnete Element von **AdControl**. Wenn sich **AdControl** in einem übergeordneten Element befindet, muss das übergeordnete Element aktiv und sichtbar sein.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. Stellen Sie sicher, dass **AdControl** im Viewport nicht ausgeblendet ist. **AdControl** muss sichtbar sein, damit Anzeigen ordnungsgemäß dargestellt werden.

12. Echte Werte für [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) und [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie [test values](set-up-ad-units-in-your-app.md#test-ad-units) sowohl für **ApplicationId** und **AdUnitId**.

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im vorherigen Abschnitt [AdControl wird nicht angezeigt](#html-notappearing).

2.  Behandeln Sie das Ereignis **onErrorOccurred**, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie in [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. **AdControl** wird standardmäßig reduziert, wenn keine Anzeige dargestellt werden kann. Wenn andere Elemente demselben übergeordneten Element untergeordnet sind, können sie verschoben werden, um den freien Platz des reduzierten **AdControl**-Elements zu füllen, und bei der nächsten Anforderung erweitert werden.

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die Eigenschaft **isAutoRefreshEnabled**. Diese optionale Eigenschaft ist standardmäßig auf true festgelegt. Wenn sie auf false festgelegt ist, muss die **refresh**-Methode verwendet werden, um eine weitere Anzeige abzurufen.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  Überprüfen Sie Aufrufe der Methode **refresh**. Bei Verwendung der automatischen Aktualisierung kann **refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    In diesem Beispiel wird die Verwendung der Methode **refresh** gezeigt. Der folgende HTML-Code zeigt ein Beispiel für die Instanziierung von **AdControl**, wenn **isAutoRefreshEnabled** auf false festgelegt ist.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    Dieses Beispiel zeigt die Verwendung der Funktion **refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)** -Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass **AdControl** instanziiert ist. Wenn **AdControl** nicht instanziiert ist, ist es nicht verfügbar.

    Die folgenden Codeausschnitte zeigen ein Beispiel für die Instanziierung von **AdControl**. Dieser HTML-Code zeigt ein Beispiel für das Einrichten der Benutzeroberfläche für **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    Der folgende JavaScript-Code zeigt ein Beispiel für die Instanziierung von **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  Überprüfen Sie das übergeordnete Element. Das übergeordnete Element **&lt;div&gt;** muss richtig zugewiesen, aktiv und sichtbar sein.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen mit der Anwendungs-ID und der Ad Unit ID identisch sein, die Sie im Partner Center abgerufen haben. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  Überprüfen Sie das übergeordnete Element von **AdControl**. Das übergeordnete Element muss aktiv und sichtbar sein.

6.  Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie [test values](set-up-ad-units-in-your-app.md#test-ad-units) sowohl für **ApplicationId** und **AdUnitId**.

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im Abschnitt [AdControl wird nicht angezeigt](#js-adcontrolnotappearing).

2.  Behandeln Sie das Ereignis **onErrorOccurred**, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie in [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md).

    In diesem Beispiel wird die Implementierung eines Fehlerhandlers gezeigt, der Fehlermeldungen meldet. Der folgende HTML-Codeausschnitt enthält ein Beispiel für das Einrichten der Benutzeroberfläche zum Anzeigen von Fehlermeldungen.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    In diesem Beispiel wird die Instanziierung von **AdControl** gezeigt. Diese Funktion würde in der Datei app.onactivated eingefügt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    In diesem Beispiel wird die Meldung von Fehlern gezeigt. Diese Funktion würde unterhalb der selbstausführenden Funktion in der Datei default.js eingefügt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie, ob in der [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx)-Eigenschaft **AdControl** auf „false“ festgelegt ist. Diese optionale Eigenschaft ist standardmäßig auf **true** festgelegt. Wenn sie auf **false** festgelegt ist, muss die Methode **Refresh** verwendet werden, um eine weitere Anzeige abzurufen.

2.  Überprüfen Sie Aufrufe der [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx)-Methode. Bei Verwendung der automatischen Aktualisierung ((**IsAutoRefreshEnabled** ist **true**)) kann **Refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung (**IsAutoRefreshEnabled** ist **false**) sollte **Refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    Dieses Beispiel zeigt das Erstellen von **div** für **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    In diesem Beispiel wird die Verwendung der **Refresh**-Funktion gezeigt.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

 

 
