---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Erfahren Sie, wie Sie die adcontrol-Klasse verwenden, um Banner Anzeigen in einer JavaScript/HTML-App für Windows 10 (UWP) anzuzeigen.
title: „AdControl“ in HTML 5 und JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, adcontrol, Ad Control, JavaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: d770e8a9a15835d7fab52e7383acca3a3df6c6cd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155674"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>„AdControl“ in HTML 5 und JavaScript

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie die [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) -Klasse verwenden, um Banner Anzeigen in einer universelle Windows-Plattform (UWP) JavaScript/HTML-App für Windows 10 anzuzeigen.

Ein vollständiges Beispiel-Projekt mit einer Veranschaulichung, wie Sie mithilfe von C# und C++ Werbebanner einer JavaScript/HTML-App hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Wenn Sie die Windows 10 SDK-Version 10.0.14393 (Anniversary Update) oder eine neuere Version des Windows SDK installiert haben, müssen Sie auch die [winjs](https://github.com/winjs/winjs) -Bibliothek installieren. Diese Bibliothek war in früheren Versionen der Windows SDK für Windows 10 enthalten, aber ab der Windows 10 SDK-Version 10.0.14393 (Anniversary Update) muss diese Bibliothek separat installiert werden. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrieren einer Banner Anzeige in Ihre APP

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.

    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package. appxmanifest" in Ihrem Projekt, und stellen Sie sicher, dass die Funktion " **Internet (Client)** " ausgewählt ist. Ihre APP benötigt diese Funktion zum Empfangen von Test-und Live-Werbeeinblendungen.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Fügen Sie einen Verweis auf das Microsoft Advertising SDK in Ihrem Projekt hinzu:

    1. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen... aus.**
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für JavaScript** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

6.  Öffnen Sie die Datei „index.html“ (oder je nach Projekt eine andere HTML-Datei).

7.  Fügen Sie im ** &lt; Head &gt; ** -Abschnitt nach den JavaScript-verweisen des Projekts von default. CSS und main.js den Verweis auf ad.js hinzu.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Diese Zeile muss im ** &lt; Head &gt; ** -Abschnitt nach dem einschließen von main.js platziert werden. andernfalls tritt ein Fehler auf, wenn Sie das Projekt erstellen.

8.  Ändern Sie den ** &lt; Text &gt; ** Abschnitt in der default.html-Datei (oder in einer anderen HTML-Datei entsprechend für Ihr Projekt), um das **div** -Element für das **adcontrol**-Element einzubeziehen. Weisen Sie die **ApplicationId** -Eigenschaft und die **adunitid** -Eigenschaft des **adcontrol** den [Test-AD-Einheits Werten](set-up-ad-units-in-your-app.md#test-ad-units)zu. Passen Sie außerdem die **Höhe** und **Breite** an, sodass das Steuerelement eine der [unterstützten Werbe Größen für Bannerwerbung](supported-ad-sizes-for-banner-ads.md)ist.

    > [!NOTE]
    > Jede **adcontrol** verfügt über eine entsprechende *Ad-Einheit* , die von unseren Diensten verwendet wird, um Werbung für das Steuerelement bereitzustellen, und jede Ad-Einheit besteht aus einer Ad-Einheiten- *ID* und einer Anwendungs- *ID*. In den folgenden Schritten weisen Sie dem Steuerelement Test Ad Unit ID-und Anwendungs-ID-Werte zu. Diese Testwerte können nur in einer Testversion der APP verwendet werden. Bevor Sie Ihre APP im Store veröffentlichen, müssen Sie [diese Testwerte durch livewerte](#release) aus Partner Center ersetzen.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Kompilieren und Ausführen der App, um sie mit einer Anzeige zu sehen

Das folgende Beispiel zeigt die vollständige index.html für eine einfache APP.

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

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Programm gesteuertes Erstellen eines adcontrol-Elements in JavaScript

In den vorherigen Schritten wird gezeigt, wie Sie ein **adcontrol** -Element im HTML-Markup deklarieren. Alternativ können Sie mithilfe von JavaScript Programm **gesteuert ein adcontrol** -Element erstellen. In diesem Beispiel wird davon ausgegangen, dass Sie ein vorhandenes **div** in Ihrem HTML-Code mit der ID **myad**verwenden.

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

In diesem Beispiel wird davon ausgegangen, dass Sie die Ereignishandlermethoden **myAdError**, **myAdRefreshed** und **myAdEngagedChanged** bereits deklariert haben.

Wenn Sie diesen Code verwenden und keine Anzeigen angezeigt werden, können Sie versuchen, ein **position:relativ**-Attribut im **div**-Element einzufügen, das das **AdControl** enthält. Dadurch wird die Standardeinstellung von **IFrame** überschrieben. Anzeigen werden ordnungsgemäß angezeigt, sofern sie nicht aufgrund des Werts dieses Attributs nicht angezeigt werden. Beachten Sie, dass neue Anzeigeeinheiten unter Umständen bis zu 30 Minuten nicht verfügbar sind.

> [!NOTE]
> Die in diesem Beispiel gezeigten Werte für " *ApplicationId* " und " *adunitid* " sind [testmoduswerte](set-up-ad-units-in-your-app.md#test-ad-units). Sie müssen [diese Werte durch livewerte](set-up-ad-units-in-your-app.md#live-ad-units) aus Partner Center ersetzen, bevor Sie Ihre APP für die Übermittlung einreichen.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Sie Ihre APP mit Live Werbung

1. Stellen Sie sicher, dass ihre Verwendung von Banner Anzeigen in Ihrer APP unseren [Richtlinien für Banner Anzeigen](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)folgt.

1.  Wechseln Sie in Partner Center zur Seite [in-App-ADS](../publish/in-app-ads.md) , und [Erstellen Sie eine Ad-Einheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-ID-Werte für Test-und Live-UWP-Ad-Einheiten haben unterschiedliche Formate. Testanwendungs-ID-Werte sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

2. Sie können optional die AD-Vermittlung für das **adcontrol** aktivieren, indem Sie die Einstellungen im Abschnitt " [Vermittlungs Einstellungen](../publish/in-app-ads.md#mediation) " auf der Seite " [in-App-Werbung](../publish/in-app-ads.md) " konfigurieren. Die AD-Vermittlung ermöglicht es Ihnen, ihre Werbe-und App-herauf Stufungs Funktionen zu maximieren, indem Sie Werbeeinblendungen aus mehreren Ad-Netzwerken anzeigen, darunter auch anzeigen aus anderen kostenpflichtigen Ad-Netzwerken wie Taboola und smaato und Werbe

3.  Ersetzen Sie in Ihrem Code die Ad-Einheiten Werte (**ApplicationId** und **adunitid**) der Test durch die im Partner Center generierten livewerte.

4.  Über [Mitteln Sie Ihre APP](../publish/app-submissions.md) mithilfe von Partner Center an den Store.

5.  Überprüfen Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) in Partner Center.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Ad-Einheiten für mehrere AD-Steuerelemente in Ihrer APP

Sie können mehrere **adcontrol** -Objekte in einer einzelnen App verwenden (beispielsweise könnte jede Seite in der APP ein anderes **adcontrol** -Objekt hosten). In diesem Szenario wird empfohlen, jedem Steuerelement eine andere Ad-Einheit zuzuweisen. Wenn Sie verschiedene Ad-Einheiten für jedes Steuerelement verwenden, können Sie [die Vermittlungs Einstellungen separat konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md) für jedes Steuerelement erhalten. Dadurch können unsere Dienste auch die Werbeeinblendungen, die wir für Ihre APP bereitstellen, besser optimieren.

> [!IMPORTANT]
> Sie können jede Ad-Einheit nur in einer App verwenden. Wenn Sie eine Ad-Einheit in mehr als einer App verwenden, werden ADS nicht für diese Ad-Einheit bereitgestellt.

## <a name="related-topics"></a>Zugehörige Themen

* [Richtlinien für Banneranzeigen](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Einrichten von Ad-Einheiten für Ihre APP](set-up-ad-units-in-your-app.md)
* [Exemplarische Vorgehensweise zur Fehlerbehandlung in JavaScript](error-handling-in-javascript-walkthrough.md)