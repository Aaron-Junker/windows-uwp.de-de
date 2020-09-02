---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Erfahren Sie, wie Sie eine Interstitial-Werbeanzeige mithilfe einer in JavaScript und HTML geschriebenen universelle Windows-Plattform (UWP)-app starten.
title: Beispielcode für Interstitialwerbung in JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, Interstitial, JavaScript, Beispielcode
ms.localizationpriority: medium
ms.openlocfilehash: 07a61bfde1fc6a77f87617ee553408c71f79cbc3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363633"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Beispielcode für Interstitialwerbung in JavaScript

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieses Thema enthält den vollständigen Beispielcode für eine grundlegende JavaScript- und HTML-App für die universelle Windows-Plattform (UWP), in der Interstitialwerbung angezeigt wird. Eine schrittweise Anleitung dazu, wie Sie Ihr Projekt zur Verwendung dieses Codes konfigurieren, finden Sie unter [Interstitialwerbung](interstitial-ads.md). Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Codebeispiel

In diesem Abschnitt wird der Inhalt der HTML- und JavaScript-Dateien in einer einfachen App veranschaulicht, in der eine Interstitialwerbung angezeigt wird. Um diese Beispiele zu verwenden, kopieren Sie diesen Code in ein JavaScript- **winjs-App-Projekt (Universal Windows)** in Visual Studio.

Diese Beispiel-App verwendet zwei Schaltflächen, um eine Interstitialwerbung anzufordern und dann zu starten. Die von Visual Studio generierten Dateien „main.js“ und „index.html“ wurden geändert und sind unten zu sehen. Die unten dargestellte Datei „script.js“ enthält den größten Teil des Beispielcodes. Diese Datei muss dem Ordner **js** in Ihrem Projekt hinzugefügt werden.

Ersetzen Sie die Werte der ```applicationId``` ```adUnitId``` Variablen und durch livewerte aus Partner Center, bevor Sie Ihre APP an den Store übermitteln. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Um dieses Beispiel so zu ändern, dass eine Interstitial-Banner Anzeige anstelle einer Interstitial-Videoanzeige angezeigt wird, übergeben Sie den Wert **interstitialadtype. Display** anstelle von **interstitialadtype. Video**an den ersten Parameter der [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) -Methode. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
:::code language="html" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/index.html" range="1-21":::

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="script":::

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/main.js" id="main":::

## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
