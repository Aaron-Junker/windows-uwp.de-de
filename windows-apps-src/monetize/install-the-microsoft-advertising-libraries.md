---
author: mcleanbyron
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: "Erfahren Sie mehr über die Installation der Microsoft Advertising-Bibliotheken."
title: Installieren der Microsoft Advertising-Bibliotheken
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Anzeigen, Werbung, Installieren, SDK, Bibliotheken
ms.openlocfilehash: 3304efd659a32176a44c33d9df4e8062b3bc7700
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="install-the-microsoft-advertising-libraries"></a>Installieren der Microsoft Advertising-Bibliotheken




Für Apps für die universelle Windows-Plattform (UWP) für Windows10 sind die Microsoft Advertising-Bibliotheken im [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) enthalten. Dieses SDK ist eine Erweiterung von Visual Studio2015 und späteren Versionen. Weitere Informationen zur Installation dieses SDKs finden Sie in [diesem Artikel](microsoft-store-services-sdk.md).

> **Hinweis**&nbsp;&nbsp;Wenn Sie Windows10 SDK (14393) oder höher installiert haben, müssen Sie auch die WinJS-Bibliothek installieren, wenn Sie einer JavaScript/HTML-UWP-App Anzeigen hinzufügen möchten. Diese Bibliothek war in den früheren Versionen des Windows10 SDK enthalten. Ab Windows10 SDK (14393) muss diese Bibliothek jedoch separat installiert werden. Informationen zur Installation von WinJS finden Sie unter [WinJS herunterladen](http://try.buildwinjs.com/download/GetWinJS/).

Für XAML- und JavaScript/HTML-Apps für Windows8.1 und Windows Phone8.x sind die Microsoft Advertising-Bibliotheken im [Microsoft Advertising SDK für Windows und Windows Phone8.x](http://aka.ms/store-8-sdk) enthalten. Dieser SDK ist eine Erweiterung von Visual Studio2015 und Visual Studio2013.

Für Windows Phone Silverlight8.x-Apps sind die Microsoft Advertising-Bibliotheken in einem NuGet-Paket verfügbar, das Sie herunterladen und für Ihr Projekt installieren können. Weitere Informationen finden Sie unter [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## <a name="library-names-for-advertising"></a>Bibliotheksnamen für Werbung


Es gibt mehrere unterschiedliche Advertising-Bibliotheken im Microsoft Store-Services-SDK und Microsoft Advertising SDK for Windows and Windows Phone8.x:

* Der Microsoft Store Services SDK enthält die Microsoft Advertising-Bibliotheken (die die Klassen [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) und [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) für XAML- und JavaScript/HTML-Apps bereitstellen).

* Der Microsoft Advertising SDK for Windows and Windows Phone8.x enthält zwei Sätze von Anzeigenbibliotheken: die Microsoft Advertising-Bibliotheken (die die Klassen [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) und [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) für XAML- und JavaScript/HTML-Apps bereitstellen) und die Bibliotheken für die Anzeigenvermittlung (die die Klasse **AdMediatorControl** bereitstellen).

In dieser Dokumentation wird die Verwendung der Klassen **AdControl** und **InterstitialAd** in den Microsoft Advertising-Bibliotheken zur Anzeige von Banner- oder Interstitialanzeigen beschrieben. Informationen zur Verwendung der Anzeigenvermittlung für Windows8.1- und Windows Phone8.x-Apps finden Sie unter [Maximieren des Umsatzes durch Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Hinweis**&nbsp;&nbsp;Die Anzeigenvermittlung mithilfe der **AdMediatorControl**-Klasse wird derzeit für UWP-Apps unter Windows10 nicht unterstützt. Die serverseitige Vermittlung wird in Kürze für UWP-Apps erhältlich sein. Dabei werden für Banneranzeigen (**AdControl**) und Interstitialanzeigen (**InterstitialAd**) dieselben APIs verwendet.

Bevor Sie eines der Anzeigensteuerelemente in Ihrem App-Code verwenden können, müssen Sie auf die entsprechende Bibliothek in Ihrem Projekt verweisen. In den folgenden Tabellen werden die Namen der einzelnen Bibliotheken aufgelistet, wie sie im Dialogfeld **Verweis-Manager** in Visual Studio angezeigt werden.


<table>
    <thead>
        <tr><th>Name des Steuerelements</th><th>Projekttyp</th><th>Name der Bibliothek im Verweis-Manager</th><th>Versionsnummer</th></tr>
    </thead>
    <tbody>
    <tr>
            <td rowspan="3">**AdControl** und **InterstitialAd** (XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK für XAML</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK für Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Ad Mediator SDK für Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    <tr>
            <td rowspan="3">**AdControl** und **InterstitialAd** (JavaScript/HTML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising SDK für JavaScript</td>
            <td>10.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Microsoft Advertising SDK für Windows8.1 Native (JS)</td>
            <td>8.5</td>
        </tr>
        <tr>
            <td>WindowsPhone8.1</td>
            <td>Microsoft Advertising SDK für Windows Phone8.1 Native (JS)</td>
            <td>8.5</td>
        </tr>
    <tr>
            <td rowspan="3">**AdMediatorControl** (nur XAML)</td>
            <td>UWP</td>
            <td>Microsoft Advertising Universal SDK</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows 8.1</td>
            <td>Ad Mediator SDK für Windows 8.1 XAML</td>
            <td>1.0</td>
        </tr>
        <tr>
            <td>Windows Phone 8.1</td>
            <td>Ad Mediator SDK für Windows Phone 8.1 XAML</td>
            <td>1.0</td>
        </tr>
    </tbody>
</table>

 

 

 
