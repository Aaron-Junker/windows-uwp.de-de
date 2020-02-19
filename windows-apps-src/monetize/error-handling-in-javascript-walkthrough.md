---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Hier erfahren Sie, wie AdControl-Fehler in Ihrer App aufgefangen werden.
title: Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeige, Werbung, Fehlerbehandlung, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 01b9949a17d5653bf121018dc40058b99af21719
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463852"
---
# <a name="error-handling-in-javascript-walkthrough"></a>Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://aka.ms/ad-monetization-shutdown)

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Anzeigen-bezogene Fehler in Ihrer JavaScript-App erfasst werden können. In dieser exemplarischen Vorgehensweise wird ein [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) verwendet, um eine Banneranzeige anzuzeigen, die allgemeinen Konzepte gelten jedoch auch für Interstitialwerbung und native Anzeigen.

In diesen Beispielen wird davon ausgegangen, dass Sie eine JavaScript-App haben, die ein **AdControl** enthält. Schritt-für-Schritt-Anleitungen, die zeigen, wie ein **AdControl** zu Ihrer App hinzugefügt wird, finden Sie unter [AdControl in HTML 5 und Javascript](adcontrol-in-html-5-and-javascript.md). Ein vollständiges Beispiel-Projekt mit einer Veranschaulichung, wie Sie mithilfe von C# und C++ Werbebanner einer JavaScript/HTML-App hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

1.  Fügen Sie in der Datei "default.html" einen Wert für das Ereignis **OnErrorOccurred** hinzu, wo Sie die **data-win-options** in **div** für das **AdControl** definieren. Suchen Sie den folgenden Code in der Datei „default.html“.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Fügen Sie nach dem **adUnitId**-Attribut den Wert für das Ereignis **OnErrorOccurred** hinzu.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Erstellen Sie ein **div**-Element, das Text anzeigt, damit Sie die generierte Nachrichten sehen können. Fügen Sie dazu den folgenden Code nach **div** für **MyAd** hinzu.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Erstellen Sie ein **AdControl**, das ein Fehlerereignis auslöst. Es kann nur eine Anwendungs-ID für alle **AdControl**-Objekte in einer App vorhanden sein. Das Erstellen eines zusätzlichen AdControl-Objekts mit einer anderen Anwendungs-ID löst einen Fehler zur Laufzeit aus. Fügen Sie dazu nach den vorherigen **div**-Abschnitten, die Sie hinzugefügt haben, den folgenden Code im "body"-Abschnitt der Seite "default.html" hinzu.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  In der Datei "default.js" des Projekts fügen nach der Standardinitialisierungsfunktion den Ereignishandler für **ErrorLogger** hinzu. Führen Sie einen Bildlauf bis zum Ende der Datei durch. Nach dem letzten Semikolon fügen Sie den folgenden Code ein.
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

* [Anzeigenbeispiele auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
