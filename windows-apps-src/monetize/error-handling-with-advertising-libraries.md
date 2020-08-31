---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Hier erfahren Sie, wie Sie mit Fehlern umgehen, die in den Microsoft Advertising-Bibliotheken von der AdControl-Klasse generiert werden.
title: Behandeln von Fehlern bei Anzeigen
ms.date: 02/18/2020
ms.topic: article
keywords: 'Windows 10, UWP, Werbung, Werbung, Fehlerbehandlung, JavaScript, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 2a1a82c9977bfbe61712d39e4d23fdd68cd598ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171554"
---
# <a name="handle-ad-errors"></a>Behandeln von Fehlern bei Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Die Klassen [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol),  [interstitialad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)und [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) verfügen jeweils über ein **erroreingetreten** -Ereignis, das ausgelöst wird, wenn ein Ad-bezogener Fehler auftritt. Ihr app-Code kann dieses Ereignis behandeln und die Eigenschaften [errorCode](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) und [ErrorMessage](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) des ereignisargs-Objekts überprüfen, um die Ursache des Fehlers zu ermitteln.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML-Apps

So behandeln Sie AD-bezogene Fehler in einer XAML-App:

1. Weisen Sie das Ereignis **erroreingetreten** Ihres **adcontrol**-, **interstitialad**-oder **NativeAdsManagerV2** -Objekts dem Namen eines Ereignishandlerdelegaten zu.

2. Codieren Sie den Ereignishandlerdelegaten so, dass er zwei Parameter verwendet: ein **Objekt** für den Absender und ein [AdErrorEventArgs](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs)-Objekt.

Es folgt ein Beispiel, das dem Ereignis **erroreingetreten** einen Delegaten namens **onaderror** für ein **adcontrol** -Objekt mit dem Namen *mybanneradcontrol*zuweist.

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

So behandeln **Sie** Fehler Fehler in einer JavaScript-App:

1.  Weisen Sie das **OnErrorOccurred**-Ereignis einem Ereignishandler zu.

2.  Codieren Sie den Ereignishandler.

Es folgt ein Beispiel, das dem Ereignis **erroreingetreten** eines **adcontrol** -Objekts einen Ereignishandler mit dem Namen **errorlogger** zuweist.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

Die Fehlerbehandlungsfunktion ist deklarativ und muss in die Funktion [MarkSupportedForProcessing](/previous-versions/windows/apps/hh967819(v=win.10)) eingeschlossen werden.

Der Fehlerhandler fängt das JavaScript-Fehler Objekt ab, wenn ein Fehler auftritt. Das Fehlerobjekt liefert dem Fehlerhandler zwei Argumente. Weitere Informationen finden Sie unter [Eigenschaften spezieller Fehler von asynchronen Methoden von Windows-Runtime](/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods).

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