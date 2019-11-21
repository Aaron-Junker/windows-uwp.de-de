---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Erfahren Sie, wie Sie Interstitialwerbung mithilfe von JavaScript/HTML veröffentlichen.
title: Beispielcode für Interstitialwerbung in JavaScript
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Interstitialwerbung, JavaScript, Beispielcode
ms.localizationpriority: medium
ms.openlocfilehash: 742d10f7e49557435a6249e65cf66960d338b784
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260256"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Beispielcode für Interstitialwerbung in JavaScript

Dieses Thema enthält den vollständigen Beispielcode für eine grundlegende JavaScript- und HTML-App für die universelle Windows-Plattform (UWP), in der Interstitialwerbung angezeigt wird. Eine schrittweise Anleitung dazu, wie Sie Ihr Projekt zur Verwendung dieses Codes konfigurieren, finden Sie unter [Interstitialwerbung](interstitial-ads.md). Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Codebeispiel

In diesem Abschnitt wird der Inhalt der HTML- und JavaScript-Dateien in einer einfachen App veranschaulicht, in der eine Interstitialwerbung angezeigt wird. Um die Beispiele zu verwenden, kopieren Sie diesen Code in ein JavaScript-Projekt **WinJS-App (universelles Windows)** in Visual Studio.

Diese Beispiel-App verwendet zwei Schaltflächen, um eine Interstitialwerbung anzufordern und dann zu starten. Die von Visual Studio generierten Dateien „main.js“ und „index.html“ wurden geändert und sind unten zu sehen. Die unten dargestellte Datei „script.js“ enthält den größten Teil des Beispielcodes. Diese Datei muss dem Ordner **js** in Ihrem Projekt hinzugefügt werden.

Replace the values of the ```applicationId``` and ```adUnitId``` variables with live values from Partner Center before submitting your app to the Store. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Wenn Sie dieses Beispiel ändern möchten, um eine Interstitialwerbung mit Bannern anstelle einer Videointerstitialanzeige anzuzeigen, übergeben Sie den Wert **InterstitialAdType.display** an den ersten Parameter der [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode anstelle von **InterstitialAdType.video**. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
