---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Hier erfahren Sie, wie Sie mit Fehlern umgehen, die in den Microsoft Advertising-Bibliotheken von der AdControl-Klasse generiert werden.
title: Behandeln von Fehlern bei Anzeigen
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeige, Werbung, Fehlerbehandlung, Javascript, XAML, C#
ms.localizationpriority: medium
ms.openlocfilehash: 1804bc6b44069dccdd92d0a33fcfd48567363a33
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463842"
---
# <a name="handle-ad-errors"></a>Behandeln von Fehlern bei Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://aka.ms/ad-monetization-shutdown)

Die einzelnen Klassen [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) und [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) verfügen über ein **ErrorOccurred**-Ereignis, das ausgelöst wird, wenn ein Fehler im Zusammenhang mit Anzeigen auftritt. Der App-Code kann dieses Ereignis behandeln und die Eigenschaften von [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) und [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) des Ereignisarguments untersuchen, um die Ursache des Fehlers zu bestimmen.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML-Apps

So behandeln Sie Anzeigen-bezogene Fehler in einer XAML-App:

1. Weisen Sie das **ErrorOccurred**-Ereignis des Objekts **AdControl**, **InterstitialAd** oder **NativeAdsManagerV2** dem Namen eines Ereignishandlerdelegaten zu.

2. Codieren Sie den Ereignishandlerdelegaten so, dass er zwei Parameter verwendet: ein **Objekt** für den Absender und ein [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs)-Objekt.

Hier ist ein Beispiel, das einen Delegaten mit dem Namen **OnAdError**dem Ereignis **ErrorOccurred** des **AdControl**-Objekts mit Namen *myBannerAdControl* zuweist.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Hier ist eine Beispieldefinition des **OnAdError**-Delegaten, die Fehlerinformationen in das Ausgabefenster in Visual Studio schreibt.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

Unter [Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#](error-handling-in-xamlc-walkthrough.md) finden Sie eine exemplarische Vorgehensweise zur Veranschaulichung der **AdControl**-Fehlerbehandlung in XAML und C#.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>JavaScript/HTML-Apps

So behandeln Sie **ErrorOccur** Fehler in einer JavaScript-App:

1.  Weisen Sie das **OnErrorOccurred**-Ereignis einem Ereignishandler zu.

2.  Codieren Sie den Ereignishandler.

Hier ist ein Beispiel, das einen Ereignishandler mit dem Namen **errorLogger** dem Ereignis **ErrorOccurred** des **AdControl**-Objekts zuweist.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

Die Fehlerbehandlungsfunktion ist deklarativ und muss in die Funktion [MarkSupportedForProcessing](https://docs.microsoft.com/previous-versions/windows/apps/hh967819(v=win.10)) eingeschlossen werden.

Der Fehlerhandler fängt das JavaScript-Fehlerobjekt auf, wenn ein Fehler auftritt. Das Fehlerobjekt liefert dem Fehlerhandler zwei Argumente. Weitere Informationen finden Sie unter [Eigenschaften spezieller Fehler von asynchronen Methoden von Windows-Runtime](https://docs.microsoft.com/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods).

Hier ist ein Beispiel für eine Fehlerbehandlungsfunktion mit dem Namen **ErrorLogger**, die das Ereignis **OnErrorOccurred** behandelt.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

Unter [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md) finden Sie eine exemplarische Vorgehensweise zur Veranschaulichung der **AdControl**-Fehlerbehandlung in JavaScript.
