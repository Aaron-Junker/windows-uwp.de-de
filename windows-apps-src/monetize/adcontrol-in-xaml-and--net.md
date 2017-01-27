---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "Hier erfahren Sie, wie Sie die AdControl-Klasse nutzen können, um Werbebanner in einer XAML-App für Windows 10 (UWP), Windows 8.1 oder Windows Phone 8.1 anzuzeigen."
title: "„AdControl“ in XAML und .NET"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: f68171524fc535c5c4b855305898d56b1731182d

---

# <a name="adcontrol-in-xaml-and-net"></a>„AdControl“ in XAML und .NET


In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)-Klasse nutzen können, um Werbebanner in einer XAML-App für Windows 10 (UWP), Windows 8.1 oder Windows Phone 8.1 anzuzeigen. In dieser exemplarischen Vorgehensweise wird die **AdMediatorControl** oder die Anzeigenvermittlung nicht verwendet.

Ein vollständiges Beispiel-Projekt, das veranschaulicht, wie Sie einer XAML-App mithilfe von C# und C++ Werbebanner hinzufügen, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Voraussetzungen

* Für UWP-Apps: Installieren Sie das [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) mit Visual Studio 2015.
* Für Windows 8.1- oder Windows Phone 8.1-Apps: Installieren Sie das [Microsoft Advertising SDK für Windows und Windows Phone 8.x](http://aka.ms/store-8-sdk) mit Visual Studio 2015 oder Visual Studio 2013.

## <a name="code-development"></a>Codeentwicklung

1. Öffnen Sie in Visual Studio Ihr Projekt oder erstellen Sie ein neues Projekt.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.

2.  Wählen Sie im **Verweis-Manager** je nach Projekttyp einen der folgenden Verweise aus:

    -   Für ein Projekt für die Universelle Windows-Plattform (UWP): Erweitern Sie **Universelles Windows**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).

    -   Für ein Windows 8.1-Projekt: Erweitern Sie **Windows 8.1**, klicken Sie auf **Erweiterungen**, und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

    -   Für ein Windows Phone 8.1-Projekt: Erweitern Sie **Windows Phone 8.1**, klicken Sie auf **Erweiterungen** und aktivieren Sie anschließend das Kontrollkästchen neben **Ad Mediator SDK für Windows Phone 8.1 XAML**. Mit dieser Option werden die Microsoft Advertising- und Ad Mediator-Bibliotheken dem Projekt hinzugefügt. Die Ad Mediator-Bibliotheken können Sie jedoch außer Acht lassen.

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **Hinweis**&nbsp;&nbsp;Diese Abbildung bezieht sich auf die Erstellung eines UWP-Projekts für Windows 10 mit Visual Studio 2015. Wenn Sie eine Windows 8.1- oder Windows Phone 8.1-App erstellen oder Visual Studio 2013 verwenden, sieht Ihr Bildschirm anders aus.

3.  Klicken Sie im **Verweis-Manager** auf OK.
4.  Ändern Sie das XAML für die Seite, in die Sie Werbung einbetten möchten, um den **Microsoft.Advertising.WinRT.UI**-Namespace miteinzubeziehen. Beispiel: Bei der Standard-Beispiel-App, die von Visual Studio generiert wird (in dieser App mit MyAdFundedWindows10AppXAML bezeichnet), heißt die XAML-Seite **"MainPage.xaml"**.

  Der Bereich **Seite** der von Visual Studio generierten Datei „MainPage.xaml“ hat den folgenden Code.

  > [!div class="tabbedCodeSnippets"]
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

  > [!div class="tabbedCodeSnippets"]
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

5. Fügen Sie im **Raster**-Tag den Code für die **AdControl** ein. Weisen Sie den Eigenschaften [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) und [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) Eigenschaften auf der **Seite** die unter [Testmoduswerte](test-mode-values.md) bereitgestellten Testwerte zu. Passen Sie zudem Höhe und Breite des Steuerelements an, damit es einer der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) entspricht.

  > **Hinweis**&nbsp;&nbsp;Sie ersetzen die Testwerte ID der Anwendung und ID der Anzeigeneinheit durch Livewerte, bevor Sie Ihre App übermitteln.

  Das vollständige **Raster**-Tag sieht aus wie dieser Code.

  > [!div class="tabbedCodeSnippets"]
  ``` xml
  <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
      <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="10865270"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
  </Grid>
  ```

  Der vollständige Code für die Datei „MainPage.xaml“ sollte wie folgt aussehen.

  > [!div class="tabbedCodeSnippets"]
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
                  AdUnitId="10865270"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
        </Grid>
    </Page>
    ```

6.  Kompilieren und Ausführen der App, um sie mit einer Anzeige zu sehen

## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Veröffentlichen der App mit Live-Werbung mithilfe von Windows Dev Center

1.  Rufen Sie für Ihre App im Dev Center-Dashboard die Seite **Monetarisierung** &gt; **Gewinnbringende Nutzung mit Anzeigen** auf, und [erstellen Sie eine eigenständige Microsoft Advertising-Einheit](../publish/monetize-with-ads.md). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeeinheits-ID und die Anwendungs-ID.

2.  Ersetzen Sie in Ihrem Code die Testwerte der Anzeigenheit (**ApplicationId** und **AdUnitId**) mit den Live-Werten, die Sie in Dev Center generiert haben.

3.  [Übermitteln Sie Ihre App](../publish/app-submissions.md) mithilfe des Dev Center-Dashboards an den Store.

4.  Überprüfen Sie die [Anzeigenvermittlungsberichte](../publish/advertising-performance-report.md) auf dem Dev Center-Dashboard.

## <a name="notes"></a>Hinweise

* C#: Unter [Beispiel für XAML-Eigenschaften](xaml-properties-example.md) finden Sie ein Beispiel dafür, wie Sie **AdControl**-Ereignissen Ereignishandler zuweisen. Sehen Sie sich daraufhin [AdControl-Ereignisse in C#](adcontrol-events-in-c.md) an. Hier finden Sie Beispielcode, der in C# geschriebene Ereignishandler zeigt.

* C++: Die aktuelle Version der Microsoft Advertising-Bibliotheken unterstützt C++. Die **AdControl**-Klasse wird in systemeigenem C++ implementiert und bewirkt nicht, dass die .NET CLR geladen wird. Codebeispiele, die die Verwendung von **AdControl** in C++ veranschaulichen, finden Sie unter den [Anzeigenbeispielen auf GitHub](http://aka.ms/githubads).

* Visual Basic: Unter [Beispiel für XAML-Eigenschaften](xaml-properties-example.md) finden Sie ein Beispiel dafür, wie Sie **AdControl**-Ereignissen Ereignishandler zuweisen.

* Fehlerbehandlung: Weitere Informationen zum Behandeln von Fehlern erhalten Sie unter [AdControl-Fehlerbehandlung](adcontrol-error-handling.md).

## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


