---
author: Xansky
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Hier erfahren Sie, wie Sie Interstitialwerbung in UWP-Apps für Windows10 mithilfe des Microsoft Advertising-SDK einfügen.
title: Interstitialwerbung
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, UWP, Anzeigen, Werbung, Anzeigensteuerelement, Interstitial
ms.localizationpriority: medium
ms.openlocfilehash: f25b607b382b179ecf82d277ca2ac7e06d596a06
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765026"
---
# <a name="interstitial-ads"></a>Interstitialwerbung

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Interstititalwerbung in Universelle Windows-Plattform (UWP)-Apps und -Spiele für Windows10. Vollständige Beispielprojekte, die das Hinzufügen von Interstitialwerbung zu JavaScript-/HTML- und XAML-Apps unter Verwendung von C# und C++ zeigen, finden Sie in den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>Was ist Interstitialwerbung?

Im Gegensatz zu Standardbanneranzeigen, die auf einen Teil der Benutzeroberfläche in einer App oder einem Spiel eingeschränkt sind, wird Interstitialwerbung auf dem gesamten Bildschirm angezeigt. In Spielen werden häufig zwei Basisformen verwendet.

* Bei *Paywall*-Anzeigen muss der Benutzer eine Anzeige in regelmäßigen Intervallen ansehen. Dies findet beispielsweise zwischen Spiellevels statt:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Bei *prämienbasierten* Anzeigen möchte der Benutzer ausdrücklich einen Vorteil nutzen, z.B. einen Hinweis oder zusätzliche Zeit zum Abschließen eines Levels, und initialisiert daher die Anzeige über die Benutzeroberfläche der App.

Wir bieten zwei Arten von Interstitialwerbung in Ihren Apps und Spielen an: **Videointerstitialwerbung** und **Bannerintersititalwerbungen**.

> [!NOTE]
> Die API für Interstitialwerbung behandelt die Benutzeroberfläche nicht, mit Ausnahme des Zeitpunkts der Videowiedergabe. Informieren Sie sich in den [bewährten Methoden für Interstitialwerbung](ui-and-user-experience-guidelines.md#interstitialbestpractices10) über Richtlinien für das, was Sie tun und was Sie vermeiden sollten, wenn Sie sich überlegen, wie Sie Interstitialwerbung in Ihre App integrieren können.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising-SDK](http://aka.ms/ads-sdk-uwp) mit Visual Studio2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Interstitialwerbung in Ihrer App integrieren

Befolgen Sie zum Anzeigen von Interstitialwerbung in Ihrer App die Anweisungen für den jeweiligen Projekttyp:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

Dieser Abschnitt enthält Beispiele für C#. Visual Basic und C++ werden jedoch ebenfalls für XAML-/.NET-Projekte unterstützt. Ein vollständiges C#-Codebeispiel finden Sie unter [Beispielcode für Interstitialwerbung in C#](interstitial-ad-sample-code-in-c.md).

1. Öffnen Sie das Projekt in Visual Studio.
    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package.appxmanifest" in Ihrem Projekt, und stellen sicher, dass die **Internet (Client)**-Funktion aktiviert ist. Ihre App benötigt diese Funktion, um Testanzeigen und Live-Werbung zu erhalten.

2. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z.B. **x86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising-SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

3.  Fügen Sie in der geeigneten Codedatei in Ihrer App (z.B. in „MainPage.xaml.cs“ oder einer Codedatei für eine andere Seite) den folgenden Namespaceverweis hinzu.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  Deklarieren Sie an einer geeigneten Stelle in Ihrer App (z.B. in ```MainPage``` oder einer anderen Seite) ein [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Objekt und mehrere Zeichenfolgenfelder, die die Anwendungs-ID und Anzeigeneinheits-ID für die Interstitialwerbung darstellen. Im folgenden Codebeispiel werden die Felder `myAppId` und `myAdUnitId` den [Testwerten](set-up-ad-units-in-your-app.md#test-ad-units) für Interstitialwerbung zugewiesen.

    > [!NOTE]
    > Jedes **InterstitialAd** verfügt über ein entsprechendes *Anzeigeneinheit*, die von unseren Diensten verwendet wird, um Werbung auf das Steuerelement zu übertragen, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schrittenweisen Sie eine Test Anzeigeneinheits-ID und Anwendungs-ID für Ihr Steuerelement zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre App im Store veröffentliche, müssen Sie die [Testwerte mit den Livewerten aus dem Windows Dev Center ersetzen](#release).

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  Instanziieren Sie in Code, der beim Start ausgeführt wird (z.B. im Konstruktor der Seite) das **InterstitialAd**-Objekt, und verbinden Sie Ereignishandler für die Ereignisse des Objekts.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Wenn Sie *Video-Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 30 bis 60Sekunden, bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **AdType.Video** für den Anzeigentyp festzulegen.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    Wenn Sie *Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 5 bis 8Sekunden bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **AdType.Display** für den Anzeigentyp festzulegen.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  An dem Punkt des Codes, an dem Sie die Video- oder Banner-Interstitialwerbung anzeigen möchten, bestätigen Sie, dass **InterstitialAd** bereit zur Anzeige ist, und zeigen sie anschließend mittels der [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)-Methode an.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Definieren Sie die Ereignishandler für das **InterstitialAd**-Objekt.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

Bei den folgenden Anweisungen wird davon ausgegangen, dass Sie ein universelles Windows-Projekt für JavaScript in Visual Studio erstellt haben und dieses auf eine bestimmte CPU ausgerichtet ist. Ein vollständiges Codebeispiel finden Sie unter [Beispielcode für Interstitialwerbung in JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Öffnen Sie das Projekt in Visual Studio.

2. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z.B. **x86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising-SDK für JavaScript** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

3.  Fügen Sie im Abschnitt **&lt;head&gt;** der HTML-Datei im Projekt nach den JavaScript-Projektverweisen von „default.css“ und „default.js“ den Verweis auf „ad.js“ ein.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Deklarieren Sie in einer JS-Datei in Ihrem Projekt ein [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Objekt und mehrere Felder, die die Anwendungs-ID und Anzeigeneinheits-ID für die Interstitialwerbung enthalten. Im folgenden Codebeispiel werden die Felder `applicationId` und `adUnitId` den [Testwerten](set-up-ad-units-in-your-app.md#test-ad-units) für Interstitialwerbung zugewiesen.

    > [!NOTE]
    > Jedes **InterstitialAd** verfügt über ein entsprechendes *Anzeigeneinheit*, die von unseren Diensten verwendet wird, um Werbung auf das Steuerelement zu übertragen, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schrittenweisen Sie eine Test Anzeigeneinheits-ID und Anwendungs-ID für Ihr Steuerelement zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre App im Store veröffentlichen, müssen Sie [die Testwerte mit den Live-Werten](#release) aus dem Windows Dev Center ersetzen.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  Instanziieren Sie in Code, der beim Start ausgeführt wird (z.B. im Konstruktor der Seite) das **InterstitialAd**-Objekt, und verbinden Sie Ereignishandler für das Objekt.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Wenn Sie *Video-Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 30 bis 60Sekunden, bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **InterstitialAdType.video** für den Anzeigentyp festzulegen.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    Wenn Sie *Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 5 bis 8Sekunden bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **InterstitialAdType.display** für den Anzeigentyp festzulegen.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  Bestätigen Sie an der Stelle im Code, an der die Anzeige angezeigt werden soll, dass **InterstitialAd** bereit zum Einblenden ist, und zeigen Sie sie dann mit der [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)-Methode an.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Definieren Sie die Ereignishandler für das **InterstitialAd**-Objekt.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (DirectX Interop)

In diesem Beispiel wird angenommen, dass Sie ein C++-Projekt **DirectX- und XAML-App (Universelle Windows-App)** in Visual Studio erstellt haben, das auf eine bestimmte CPU-Architektur ausgerichtet ist.
 
1. Öffnen Sie das Projekt in Visual Studio.

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising-SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

2.  Deklarieren Sie in einer geeigneten Headerdatei für Ihre App (z.B. DirectXPage.xaml.h) ein [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Objekt und verwandte Ereignishandlermethoden.  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  Deklarieren Sie in derselben Headerdatei mehrere Zeichenfolgenfelder, die die Anwendungs-ID und Anzeigeneinheits-ID für die Interstitialwerbung darstellen. Im folgenden Codebeispiel werden die Felder `myAppId` und `myAdUnitId` den [Testwerten](set-up-ad-units-in-your-app.md#test-ad-units) für Interstitialwerbung zugewiesen.

    > [!NOTE]
    > Jedes **InterstitialAd** verfügt über ein entsprechendes *Anzeigeneinheit*, die von unseren Diensten verwendet wird, um Werbung auf das Steuerelement zu übertragen, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schrittenweisen Sie eine Test Anzeigeneinheits-ID und Anwendungs-ID für Ihr Steuerelement zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre App im Store veröffentlichen, müssen Sie [die Testwerte mit den Live-Werten](#release) aus dem Windows Dev Center ersetzen.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  Fügen Sie in der CPP-Datei, an der Stelle, an der Sie Code zum Anzeigen einer Interstitialwerbung hinzufügen möchten, den folgenden Namespaceverweis hinzu. In den folgenden Beispielen wird davon ausgegangen, dass Sie den Code der Datei DirectXPage.xaml.cpp in Ihrer App hinzufügen.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  Instanziieren Sie in Code, der beim Start ausgeführt wird (z.B. im Konstruktor der Seite) das **InterstitialAd**-Objekt, und verbinden Sie Ereignishandler für die Ereignisse des Objekts. Im folgenden Beispiel entspricht ```InterstitialAdSamplesCpp``` dem Namespace für Ihr Projekt. Ändern Sie diesen Namen nach Bedarf für Ihren Code.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Wenn Sie *Video-Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 30 bis 60Sekunden, bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **AdType::Video** für den Anzeigentyp festzulegen.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    Wenn Sie *Interstitialwerbung* verwenden möchten: Verwenden Sie ungefähr 5 bis 8Sekunden bevor Sie die Interstitialwerbung benötigen, die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode, um die Anzeige vorab abzurufen. Dadurch bleibt genügend Zeit, um die Anzeige anzufordern und vorzubereiten, bevor sie angezeigt werden soll. Achten Sie darauf, **AdType::Display** für den Anzeigentyp festzulegen.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  Bestätigen Sie an der Stelle im Code, an der die Anzeige angezeigt werden soll, dass **InterstitialAd** bereit zum Einblenden ist, und zeigen Sie sie dann mit der [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)-Methode an.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Definieren Sie die Ereignishandler für das **InterstitialAd**-Objekt.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Erstellen und testen Sie Ihre App, um zu überprüfen, ob sie Testanzeigen anzeigt.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Ihrer App mit Live-Anzeigen

1. Stellen Sie sicher, dass die Verwendung von Interstitialwerbung in Ihrer App unseren [Richtlinien für Interstitialwerbung](ui-and-user-experience-guidelines.md#interstitialbestpractices10) entspricht.

2.  Wechseln Sie im Dev Center-Dashboard auf die Seite [In-App-Anzeigen](../publish/in-app-ads.md) und [erstellen Sie eine Anzeigeneinheit](set-up-ad-units-in-your-app.md#live-ad-units). Wählen Sie für den Anzeigeneinheitstyp **Video-Interstitialwerbung** oder **Banner-Interstitialwerbung** aus, je nachdem, welche Art von Interstitialwerbung Sie anzeigen. Notieren Sie die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-IDs für Test-Anzeigeneinheiten und Live-UWP-Anzeigeneinheiten besitzen unterschiedliche Formate. Testanwendungs-ID sind GUIDs. Wenn Sie eine Live-UWP-Anzeigeneinheit im Dashboard erstellen, entspricht die Anwendungs-ID für die Anzeigeneinheit immer der Store-ID Ihrer App (ein Beispiel für einen Store-ID-Wert ist 9NBLGGH4R315).

3. Sie können optional die Anzeigenvermittlung für die **Interstitialwerbung** durch das Konfigurieren der Einstellungen in der [Vermittlungsverwaltung](../publish/in-app-ads.md#mediation) auf der Seite [in-app ads](../publish/in-app-ads.md) aktivieren. Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken wie Taboola und Smaato sowie Anzeigen zu Werbekampagnen für Microsoft-Apps.

4.  Ersetzen Sie in Ihrem Code die Testanzeigen-Einheitenwerte mit den Livewerten, die Sie in Dev Center generiert haben.

5.  [Übermitteln Sie Ihre App](../publish/app-submissions.md) mithilfe des Windows Dev Center-Dashboards an den Store.

6.  Überprüfen Sie die [Anzeigenvermittlungsberichte](../publish/advertising-performance-report.md) im Dev Center-Dashboard.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Verwalten von Anzeigeeinheiten für mehrere Steuerelemente von Interstitialwerbung in Ihrer App

Können mehrere Steuerelemente für **InterstitialAd** in einer einzelnen App verwenden. In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

> [!IMPORTANT]
> Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Interstitialanzeigen](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Beispielcode für Interstitialwerbung in C#](interstitial-ad-sample-code-in-c.md)
* [Beispielcode für Interstitialwerbung in JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Anzeigenbeispiele auf GitHub](http://aka.ms/githubads)
* [Einrichten von Anzeigeneinheiten für die App](set-up-ad-units-in-your-app.md)
