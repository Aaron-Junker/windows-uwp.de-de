---
author: mcleanbyron
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Hier erfahren Sie, wie AdControl-Fehler in Ihrer App aufgefangen werden.
title: Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: ad177c964f2a01640d33fc09f1789fc857801792


---

# <a name="error-handling-in-javascript-walkthrough"></a>Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript




In diesem Thema wird veranschaulicht, wie [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Fehler in Ihrer App erfasst werden können.

In diesen Beispielen wird davon ausgegangen, dass Sie eine JavaScript/HTML-App haben, die ein **AdControl** enthält. Schritt-für-Schritt-Anleitungen, die zeigen, wie ein **AdControl** zu Ihrer App hinzugefügt wird, finden Sie unter [AdControl in HTML 5 und Javascript](adcontrol-in-html-5-and-javascript.md). Ein vollständiges Beispiel-Projekt, das veranschaulicht, wie Sie mithilfe von C# und C++ Werbebanner zu einer JavaScript/HTML-App hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

1.  Fügen Sie in der Datei "default.html" einen Wert für das Ereignis **OnErrorOccurred** hinzu, wo Sie die **data-win-options** in **div** für das **AdControl** definieren. Suchen Sie den folgenden Code in der Datei „default.html“.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
  </div>
  ```

  Fügen Sie nach dem **adUnitId**-Attribut den Wert für das Ereignis **OnErrorOccurred** hinzu.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Erstellen Sie ein **div**-Element, das Text anzeigt, damit Sie die generierte Nachrichten sehen können. Fügen Sie dazu den folgenden Code nach **div** für **MyAd** hinzu.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
      <b>Ad Events</b><br />
      <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
  </div>
  ```

3.  Erstellen Sie ein **AdControl**, das ein Fehlerereignis auslöst. Es kann nur eine Anwendungs-ID für alle **AdControl**-Objekte in einer App vorhanden sein. Das Erstellen eines zusätzlichen AdControl-Objekts mit einer anderen Anwendungs-ID löst einen Fehler zur Laufzeit aus. Fügen Sie dazu nach den vorherigen **div**-Abschnitten, die Sie hinzugefügt haben, den folgenden Code im "body"-Abschnitt der Seite "default.html" hinzu.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
  <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: '10865270', onErrorOccurred: errorLogger }" >
  </div>
  ```

4.  In der Datei "default.js" des Projekts fügen nach der Standardinitialisierungsfunktion den Ereignishandler für **ErrorLogger** hinzu. Führen Sie einen Bildlauf bis zum Ende der Datei durch. Nach dem letzten Semikolon fügen Sie den folgenden Code ein.

  > [!div class="tabbedCodeSnippets"]
  ``` javascript
  WinJS.Utilities.markSupportedForProcessing(
  window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
  });
  ```

5.  Erstellen Sie die Datei, und führen Sie diese aus. Sie sehen die ursprüngliche Anzeige aus der Beispiel-App, die Sie zuvor erstellt haben, und der Text unter dieser Anzeige, die den Fehler beschreibt. Sie sehen die Anzeige nicht mit der ID **liveAd**.

## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


