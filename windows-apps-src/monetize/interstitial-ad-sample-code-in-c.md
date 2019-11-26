---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Erfahren Sie, wie Sie mithilfe von C# Interstitialwerbung veröffentlichen.
title: Beispielcode für Interstitialwerbung in C#
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Interstitial, C#, Beispielcode
ms.localizationpriority: medium
ms.openlocfilehash: 8c05681d2f59c88643e8cd224bb7de554a35de8d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259289"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Beispielcode für die Interaktion mit Interstitial in C\# #  

Dieses Thema enthält den vollständigen Beispielcode für eine grundlegende C#- und XAML-App für die universelle Windows-Plattform (UWP), in der Videointerstitialwerbung angezeigt wird. Eine schrittweise Anleitung dazu, wie Sie Ihr Projekt zur Verwendung dieses Codes konfigurieren, finden Sie unter [Interstitialwerbung](interstitial-ads.md). Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Codebeispiel

In diesem Abschnitt wird der Inhalt der Dateien „MainPage.xaml“ und „MainPage.xaml.cs“ in einer einfachen App veranschaulicht, in der eine Interstitialwerbung angezeigt wird. Um die Beispiele zu verwenden, kopieren Sie den Code in ein Visual C#-Projekt **Leere App (Universelle Windows-App)** in Visual Studio.

Diese Beispiel-App verwendet zwei Schaltflächen, um eine Interstitialwerbung anzufordern und dann zu starten. Ersetzen Sie die Werte der Felder ```myAppId``` und ```myAdUnitId``` durch livewerte aus Partner Center, bevor Sie Ihre APP an den Store übermitteln. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in Ihrer App](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Wenn Sie dieses Beispiel ändern möchten, um eine Interstitialwerbung mit Bannern anstelle einer Videointerstitialanzeige anzuzeigen, übergeben Sie den Wert **AdType.Display** an den ersten Parameter der [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode anstelle von **AdType.Video**. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
