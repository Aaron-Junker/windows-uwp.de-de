---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Hier erfahren Sie, wie Sie die AdControl-Klasse nutzen können, um Werbebanner in einer JavaScript/HTML-App für Windows 10 (UWP) anzuzeigen.
title: AdControl in HTML 5 und JavaScript
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, UWP, Anzeigen, Werbung, AdControl, Anzeigen-Steuerelement, HTML, Javascript
ms.localizationpriority: medium
ms.openlocfilehash: 08b834343aafb91fee1e75f9df7ed2a752992fa2
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "8825469"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>„AdControl“ in HTML 5 und JavaScript

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)-Klasse nutzen können, um Werbebanner in einer Universellen Windows-Plattform-, JavaScript-/HTML-App für Windows 10 anzuzeigen.

Ein vollständiges Beispiel-Projekt mit einer Veranschaulichung, wie Sie Werbebanner einer JavaScript/HTML-App hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising-SDK](http://aka.ms/ads-sdk-uwp) mit Visual Studio2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Wenn Sie die Windows 10 SDK Version 10.0.14393 (Anniversary Update) oder eine höhere Version des Windows SDKS installiert haben, müssen Sie auch die [WinJS](https://github.com/winjs/winjs) -Bibliothek installieren. Diese Bibliothek war in den früheren Versionen von Windows SDK für Windows 10 enthalten, aber ab Windows 10 Anniversary SDK Version 10.0.14393 (Anniversary Update) muss diese Bibliothek separat installiert werden. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Banneranzeige in Ihrer App integrieren

1. Öffnen Sie in Visual Studio Ihr Projekt oder erstellen Sie ein neues Projekt.

    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package.appxmanifest" in Ihrem Projekt, und stellen sicher, dass die **Internet (Client)**-Funktion aktiviert ist. Ihre App benötigt diese Funktion, um Testanzeigen und Live-Werbung zu erhalten.

2. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z.B. **x86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising-SDK für JavaScript** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

6.  Öffnen Sie die Datei „index.html“ (oder je nach Projekt eine andere HTML-Datei).

7.  Fügen Sie im Abschnitt **&lt;head&gt;** nach den JavaScript-Verweisen auf default.css und main.js des Projekts den Verweis auf ad.js ein.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Diese Zeile muss im Abschnitt **&lt;Head&gt;** nach der Datei „main.js“ platziert werden. Andernfalls tritt bei der Erstellung des Projekts ein Fehler auf.

8.  Ändern Sie den Bereich **&lt;body&gt;** in der Datei „default.HTML“ (oder je nach Projekt einer anderen html-Datei), sodass sie das **DIV**-Element für die **AdControl** enthält. Weisen Sie den Eigenschaften **applicationId** und **adUnitId** Eigenschaften dem **AdControl** unter [Testanzeigen-Einheitenwerte](set-up-ad-units-in-your-app.md#test-ad-units) bereitgestellten Testwerte zu. Passen Sie zudem **Höhe** und **Breite** des Steuerelements an, damit es einer der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) entspricht.

    > [!NOTE]
    > Jedes **AdControl** verfügt über ein entsprechendes *Anzeigeneinheit*, die von unseren Diensten verwendet wird, um Werbung auf das Steuerelement zu übertragen, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schrittenweisen Sie eine Test Anzeigeneinheits-ID und Anwendungs-ID für Ihr Steuerelement zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre app im Store veröffentlichen, müssen Sie [Ersetzen Testwerte mit den live-Werten](#release) aus dem Partner Center.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Kompilieren Sie die App, und führen Sie sie aus, um sie mit einer Anzeige zu sehen.

Das folgende Beispiel zeigt die vollständige index.html für eine einfache App.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Erstellen eines programmgesteuerten AdControl-Elements in JavaScript

Die vorherigen Schritte zeigen das Deklarieren einer **AdControl** in Ihrem HTML-Markup. Alternativ können Sie programmgesteuert ein **AdControl** (Anzeigensteuerelement) mit JavaScript erstellen. In diesem Beispiel wird davon ausgegangen, dass Sie in Ihrem HTML-Code ein vorhandenes **div**-Element mit der ID **MyAd** verwenden.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

In diesem Beispiel wird davon ausgegangen, dass Sie die Ereignishandlermethoden **myAdError**, **myAdRefreshed** und **myAdEngagedChanged** bereits deklariert haben.

Wenn Sie diesen Code verwenden und keine Anzeigen angezeigt werden, können Sie versuchen, ein **position:relativ**-Attribut im **div**-Element einzufügen, das das **AdControl** enthält. Dadurch wird die Standardeinstellung von **IFrame** überschrieben. Anzeigen werden ordnungsgemäß angezeigt, sofern sie nicht aufgrund des Werts dieses Attributs nicht angezeigt werden. Beachten Sie, dass neue Anzeigeeinheiten unter Umständen bis zu 30 Minuten nicht verfügbar sind.

> [!NOTE]
> Die in diesem Beispiel angezeigten Werte *applicationId* und *adUnitId* sind [Testmoduswerte](set-up-ad-units-in-your-app.md#test-ad-units). [Ersetzen Sie diese Werte mit livewerten](set-up-ad-units-in-your-app.md#live-ad-units) aus dem Partner Center müssen vor dem Übermitteln Ihrer app für die Übermittlung.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Ihrer App mit Live-Anzeigen

1. Stellen Sie sicher, dass die Verwendung von Werbebannern in Ihrer App unseren [Richtlinien für das Anzeigen von Werbebannern](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads) entspricht.

1.  Im Partner Center wechseln Sie zu der Seite [In-app-anzeigen](../publish/in-app-ads.md) und [Erstellen Sie eine anzeigeneinheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Typ für die Anzeigeneinheit **Banner** an. Notieren Sie die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-IDs für Test-Anzeigeneinheiten und Live-UWP-Anzeigeneinheiten besitzen unterschiedliche Formate. Testanwendungs-ID sind GUIDs. Wenn Sie eine live-UWP-anzeigeneinheit im Partner Center erstellen, entspricht die Anwendungs-ID-Wert für die anzeigeneinheit immer die Store-ID für Ihre app (der ein Beispiel für Store-ID-Wert ist 9nblggh4r315).).

2. Sie können optional die Anzeigenvermittlung für **AdControl** durch Konfigurieren der [Vermittlungseinstellungen](../publish/in-app-ads.md#mediation) auf der Seite [In-App-Anzeigen](../publish/in-app-ads.md) aktivieren. Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken wie Taboola und Smaato sowie Anzeigen zu Werbekampagnen für Microsoft-Apps.

3.  Ersetzen Sie in Ihrem Code die Testwerte der anzeigeneinheit (**ApplicationId** und **AdUnitId**) mit den live-Werten, die Sie im Partner Center generiert.

4.  [Übermitteln Ihrer app](../publish/app-submissions.md) mithilfe der Partner Center an den Store.

5.  Überprüfen Sie Ihre [Berichte zur anzeigenleistung](../publish/advertising-performance-report.md) im Partner Center.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Anzeigeeinheiten für mehrere Steuerelemente von Anzeigen in Ihrer App

Sie können mehrere **AdControl** Objekte in einer einzelnen App nutzen (z.B. jede Seite in Ihrer App hostet ein anderes **AdControl** Objekt). In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

> [!IMPORTANT]
> Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Banneranzeigen](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Anzeigenbeispiele auf GitHub](http://aka.ms/githubads)
* [Einrichten von Anzeigeneinheiten für die App](set-up-ad-units-in-your-app.md)
* [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md)
