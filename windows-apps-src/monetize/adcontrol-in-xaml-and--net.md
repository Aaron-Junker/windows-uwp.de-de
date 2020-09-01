---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Erfahren Sie, wie Sie die adcontrol-Klasse verwenden, um Banner Anzeigen in einer XAML-App für Windows 10 (UWP) anzuzeigen.
title: „AdControl“ in XAML und .NET
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, adcontrol, Ad Control, XAML, .net, Exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 8d1d1e75754bc9f3bed0be862a38ede43d55ad52
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155664"
---
# <a name="adcontrol-in-xaml-and-net"></a>„AdControl“ in XAML und .NET

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie mithilfe der [adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) -Klasse Banner Anzeigen in einer universelle Windows-Plattform (UWP)-XAML-App für Windows 10 anzeigen, die mithilfe von c# implementiert wird.

> [!NOTE]
> Das Microsoft Advertising SDK unterstützt auch XAML-apps, die mithilfe von C++ implementiert werden. Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Integrieren einer Banner Anzeige in Ihre APP

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.

    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package. appxmanifest" in Ihrem Projekt, und stellen Sie sicher, dass die Funktion " **Internet (Client)** " ausgewählt ist. Ihre APP benötigt diese Funktion zum Empfangen von Test-und Live-Werbeeinblendungen.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Fügen Sie einen Verweis auf das Microsoft Advertising SDK in Ihrem Projekt hinzu:

    1. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen... aus.**
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

4.  Ändern Sie das XAML für die Seite, in die Sie Werbung einbetten möchten, um den **Microsoft.Advertising.WinRT.UI**-Namespace miteinzubeziehen. Beispiel: Bei der Standard-Beispiel-App, die von Visual Studio generiert wird (in dieser App mit MyAdFundedWindows10AppXAML bezeichnet), heißt die XAML-Seite **"MainPage.xaml"**.

    Der Bereich **Seite** der von Visual Studio generierten Datei „MainPage.xaml“ hat den folgenden Code.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Fügen Sie den Namespace-Verweis **Microsoft.Advertising.WinRT.UI** ein, sodass der Abschnitt **Seite** der Datei „MainPage.xaml“ den folgenden Code hat.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. Fügen Sie im **Raster**-Tag den Code für die **AdControl** ein. Weisen Sie die Eigenschaften "  [adunitid](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) " und " [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) " den [Test-Ad-Einheiten Werten](set-up-ad-units-in-your-app.md#test-ad-units)zu. Passen Sie außerdem die **Höhe** und **Breite** des Steuer Elements an, sodass es eine der [unterstützten Werbe Größen für Bannerwerbung](supported-ad-sizes-for-banner-ads.md)ist.

    > [!NOTE]
    > Jede **adcontrol** verfügt über eine entsprechende *Ad-Einheit* , die von unseren Diensten verwendet wird, um Werbung für das Steuerelement bereitzustellen, und jede Ad-Einheit besteht aus einer Ad-Einheiten- *ID* und einer Anwendungs- *ID*. In den folgenden Schritten weisen Sie dem Steuerelement Test Ad Unit ID-und Anwendungs-ID-Werte zu. Diese Testwerte können nur in einer Testversion der APP verwendet werden. Bevor Sie Ihre APP im Store veröffentlichen, müssen Sie [diese Testwerte durch livewerte](#release) aus Partner Center ersetzen.

    Das vollständige **Raster**-Tag sieht aus wie dieser Code.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    Der vollständige Code für die Datei „MainPage.xaml“ sollte wie folgt aussehen.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Kompilieren und Ausführen der App, um sie mit einer Anzeige zu sehen

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Sie Ihre APP mit Live Werbung

1. Stellen Sie sicher, dass ihre Verwendung von Banner Anzeigen in Ihrer APP unseren [Richtlinien für Banner Anzeigen](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)folgt.

2.  Wechseln Sie in Partner Center zur Seite [in-App-ADS](../publish/in-app-ads.md) , und [Erstellen Sie eine Ad-Einheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-ID-Werte für Test-und Live-UWP-Ad-Einheiten haben unterschiedliche Formate. Testanwendungs-ID-Werte sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

3. Sie können optional die AD-Vermittlung für das **adcontrol** aktivieren, indem Sie die Einstellungen im Abschnitt " [Vermittlungs Einstellungen](../publish/in-app-ads.md#mediation) " auf der Seite " [in-App-Werbung](../publish/in-app-ads.md) " konfigurieren. Die AD-Vermittlung ermöglicht es Ihnen, ihre Werbe-und App-herauf Stufungs Funktionen zu maximieren, indem Sie Werbeeinblendungen aus mehreren Ad-Netzwerken anzeigen, darunter auch anzeigen aus anderen kostenpflichtigen Ad-Netzwerken wie Taboola und smaato und Werbe

4.  Ersetzen Sie in Ihrem Code die Ad-Einheiten Werte (**ApplicationId** und **adunitid**) der Test durch die im Partner Center generierten livewerte.

5.  Über [Mitteln Sie Ihre APP](../publish/app-submissions.md) mithilfe von Partner Center an den Store.

6.  Überprüfen Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) in Partner Center.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Ad-Einheiten für mehrere AD-Steuerelemente in Ihrer APP

Sie können mehrere **adcontrol** -Objekte in einer einzelnen App verwenden (beispielsweise könnte jede Seite in der APP ein anderes **adcontrol** -Objekt hosten). In diesem Szenario wird empfohlen, jedem Steuerelement eine andere Ad-Einheit zuzuweisen. Wenn Sie verschiedene Ad-Einheiten für jedes Steuerelement verwenden, können Sie [die Vermittlungs Einstellungen separat konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md) für jedes Steuerelement erhalten. Dadurch können unsere Dienste auch die Werbeeinblendungen, die wir für Ihre APP bereitstellen, besser optimieren.

> [!IMPORTANT]
> Sie können jede Ad-Einheit nur in einer App verwenden. Wenn Sie eine Ad-Einheit in mehr als einer App verwenden, werden ADS nicht für diese Ad-Einheit bereitgestellt.

## <a name="related-topics"></a>Zugehörige Themen

* [Richtlinien für Banneranzeigen](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Fehlerbehandlung in XAML/c#](error-handling-in-xamlc-walkthrough.md)Exemplarische Vorgehensweise.
* [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Einrichten von Ad-Einheiten für Ihre APP](set-up-ad-units-in-your-app.md)