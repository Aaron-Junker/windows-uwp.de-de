---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Erfahren Sie, wie Sie eine Interstitial-Werbe einblads in einer universelle Windows-Plattform-app (UWP) mit c# und XAML anzeigen und starten können.
title: Beispielcode für Interstitialwerbung in C#
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, Interstitial, c#, Beispielcode
ms.localizationpriority: medium
ms.openlocfilehash: 3d80e53fd8d122e01588ffcbd16f487e1526c08d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363683"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Beispielcode für die Interaktion mit Interstitial in C\# #  

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieses Thema enthält den vollständigen Beispielcode für eine grundlegende c#-und XAML-universelle Windows-Plattform-app (UWP), die eine Interstitial-Videoanzeige zeigt. Eine schrittweise Anleitung dazu, wie Sie Ihr Projekt zur Verwendung dieses Codes konfigurieren, finden Sie unter [Interstitialwerbung](interstitial-ads.md). Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Codebeispiel

In diesem Abschnitt wird der Inhalt der Dateien „MainPage.xaml“ und „MainPage.xaml.cs“ in einer einfachen App veranschaulicht, in der eine Interstitialwerbung angezeigt wird. Um diese Beispiele zu verwenden, kopieren Sie den Code in ein Visual c#-Projekt für **leere Apps (Universal Windows)** in Visual Studio.

Diese Beispiel-App verwendet zwei Schaltflächen, um eine Interstitialwerbung anzufordern und dann zu starten. Ersetzen Sie die Werte der ```myAppId``` ```myAdUnitId``` Felder und durch livewerte aus Partner Center, bevor Sie Ihre APP an den Store übermitteln. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Um dieses Beispiel so zu ändern, dass eine Interstitial-Banner Anzeige anstelle einer Interstitial-Videoanzeige angezeigt wird, übergeben Sie den Wert **adtype. Display** an den ersten Parameter der [requestad](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) -Methode anstelle von **adtype. Video**. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
:::code language="xml" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml" range="1-13":::

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="CompleteSample":::

 
## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
