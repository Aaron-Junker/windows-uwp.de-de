---
description: Erfahren Sie, wie Sie native Anzeigen zu Ihrer UWP-App hinzufügen.
title: Native Anzeigen
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Ad-Steuerelement, native Anzeige
ms.localizationpriority: medium
ms.openlocfilehash: 83c174dc682af27a4811a44c826c572f91ec1f06
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260218"
---
# <a name="native-ads"></a>Native Anzeigen

Eine native Anzeige ist ein komponentenbasiertes Anzeigenformat, in denen jedes Element der Anzeige (wie Titel, Bild, Beschreibung und Handlungsaufforderungstext) als einzelnes Element übermittelt wird, das Sie in Ihre App integrieren können. Sie können diese Elemente mit eigenen Schriftarten, Farben, Animationen und andere Komponenten der Benutzeroberfläche in Ihrer App integrieren und so eine Benutzerfunktionalität schaffen, die perfekt in das Erscheinungsbild Ihrer App passt und hohe Einnahmen mit Anzeigen generieren.

Für Werbekunden bieten native anzeigen perfekte Platzierungen, da Anzeigen-Umgebung eng in die App integriert ist und die Benutzer daher dazu neigen, mehr Interaktion mit diesen Arten von anzeigen durchzuführen.

> [!NOTE]
> Native Anzeigen werden derzeit nur für XAML-basierte UWP-Apps für Windows 10 unterstützt. Die Unterstützung für UWP-Apps mit HTML und JavaScript ist für eine zukünftige Version des Microsoft Advertising-SDK geplant.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Native Anzeigen in Ihrer App integrieren

Befolgen Sie diesen Anweisungen, um eine native Anzeige in Ihrer App zu integrieren und vergewissern Sie sich, dass der Implementierung der nativen Anzeige eine Test-Anzeige angezeigt.

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.
    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package.appxmanifest" in Ihrem Projekt, und stellen sicher, dass die **Internet (Client)** -Funktion aktiviert ist. Ihre App benötigt diese Funktion, um Testanzeigen und Live-Werbung zu erhalten.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **Jede CPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf das Microsoft Advertising-SDK hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

4. Fügen Sie in der geeigneten Codedatei in Ihrer App (z. B. in „MainPage.xaml.cs“ oder einer Codedatei für eine andere Seite) den folgenden Namespaceverweis hinzu.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  Deklarieren Sie an einer geeigneten Stelle in Ihrer App (z. B. in ```MainPage``` oder einer anderen Seite) ein [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2)-Objekt und mehrere Zeichenfolgenfelder, die die Anwendungs-ID und Anzeigeneinheits-ID für die native Anzeige darstellen. Im folgenden Codebeispiel werden die Felder `myAppId` und `myAdUnitId` den [Testwerten](set-up-ad-units-in-your-app.md#test-ad-units) für native Anzeigen zugewiesen.
    > [!NOTE]
    > Jedes **NativeAdsManagerV2**-Objekt verfügt über eine entsprechende *Anzeigeneinheit*, die durch unsere Dienste zum Anzeigen des nativen Anzeigensteuerelements verwendet wird, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schritten weisen Sie dem Steuerelement eine Anzeigeneinheits-ID und Anwendungs-ID zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre APP im Store veröffentlichen, müssen Sie [diese Testwerte durch livewerte](#release) aus Partner Center ersetzen.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  Instanziieren Sie in Code, der beim Start ausgeführt wird (z. B. im Konstruktor der Seite) das **NativeAdsManagerV2**-Objekt, und verbinden Sie Ereignishandler für die Ereignisse **AdReady** und **ErrorOccurred** des Objekts.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  Wenn Sie bereit sind, eine native Anzeige anzuzeigen, rufen Sie die **RequestAd**-Methode auf, um eine Anzeige abzurufen.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  Wenn eine native Anzeige für Ihre App bereit ist, wird Ihr [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready)-Ereignishandler aufgerufen und ein [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2)-Objekt für die native Anzeige wird im *e*-Parameter übergeben. Verwenden Sie die **NativeAdV2**-Eigenschaften, um jedes Element der nativen Anzeige abzurufen und diese Elemente auf der Seite anzuzeigen. Achten Sie darauf, dass Sie auch die **RegisterAdContainer**-Methode aufrufen, um die UI-Element zu registrieren, die als Container für die native Anzeige dienen. Dies ist erforderlich, um Anzeigenaufrufe und -klicks ordnungsgemäß nachzuverfolgen.
    > [!NOTE]
    > Einige Elemente der nativen Anzeige sind erforderlich und müssen immer in Ihrer App angezeigt werden. Weitere Informationen finden Sie in unseren [Richtlinien für native Anzeigen](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Nehmen wir beispielsweise an, Ihre App enthält eine ```MainPage``` (oder eine andere Seite) mit dem folgenden **StackPanel**. Dies **StackPanel** enthält eine Reihe von Steuerelementen, die verschiedenen Elemente einer nativen Anzeige darstellen (z. B. Titel, Beschreibung, Bilder, *unterstützt von*-Text und eine Schaltfläche, die den *Handlungsaufforderung*-Text darstellt).

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    Im folgenden Codebeispiel wird veranschaulicht, wie Sie einen **AdReady**-Ereignishandler nutzen, der jedes Element der nativen Anzeige in den Steuerelementen im **StackPanel** angezeigt und dann die **RegisterAdContainer**-Methode zur Registrierung des **StackPanel** aufruft. Dieser Code geht davon aus, dass er aus der CodeBehind-Datei für die Seite ausgeführt wird, die **StackPanel** enthält.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

9.  Definiert einen Ereignishandler für das **ErrorOccurred**-Ereignis zum Behandeln von Fehlern im Zusammenhang mit der nativen Anzeige. Das folgende Beispiel schreibt Fehlerinformationen in das Visual Studio **Ausgabe**-Fenster während der Tests.

    [!code-csharp[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  Kompilieren und Ausführen der App, um eine Testanzeige zu sehen.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Ihrer App mit Live-Anzeigen

Nachdem Sie bestätigt, dass der Implementierung der nativen Anzeige erfolgreich eine Test-Anzeige angezeigt, befolgen Sie diese Anweisungen, um die Konfiguration der App für echte Anzeigen durchzuführen und die aktualisierte App an den Store zu übermitteln.

1.  Stellen Sie sicher, dass die Implementierung der nativen Anzeigen den [Richtlinien für native Anzeigen](ui-and-user-experience-guidelines.md#guidelines-for-native-ads) folgt.

2.  Wechseln Sie in Partner Center zur Seite [in-App-ADS](../publish/in-app-ads.md) , und [Erstellen Sie eine Ad-Einheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Einheitentyp **Native** an. Notieren Sie sich die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-IDs für Test-Anzeigeneinheiten und Live-UWP-Anzeigeneinheiten besitzen unterschiedliche Formate. Testanwendungs-ID sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

3. Sie können optional die Anzeigenvermittlung für die native Anzeige durch Konfigurieren der Einstellungen im Abschnitt [Vermittlungseinstellungen](../publish/in-app-ads.md#mediation) auf der [In-App-Anzeigen](../publish/in-app-ads.md)-Seite aktivieren. Mit der Anzeigenvermittlung können Sie Ihren Anzeigenumsatz und Funktionalitäten zur App-Bewerbung durch die Darstellung von Anzeigen aus mehreren Anzeigennetzwerken verbessern.

4.  Ersetzen Sie in Ihrem Code die Test-Ad-Einheiten Werte (d. h. die Parameter *ApplicationId* und *adunitid* des [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) -Konstruktors) durch die im Partner Center generierten livewerte.

5.  Über [Mitteln Sie Ihre APP](../publish/app-submissions.md) mithilfe von Partner Center an den Store.

6.  Überprüfen Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) in Partner Center.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Verwalten von Anzeigeeinheiten für mehrere native Anzeigen in Ihrer App

Können mehrere native Platzierung von Werbung in einer einzelnen App verwenden. In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Platzierung von Werbung zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für native Anzeigen können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

> [!IMPORTANT]
> Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Native Werbung](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [In-App-Anzeigen](../publish/in-app-ads.md)
* [Einrichten von Ad-Einheiten für Ihre APP](set-up-ad-units-in-your-app.md)
