---
description: Erfahren Sie, wie Sie Native Werbung verwenden, ein komponentenbasiertes Ad-Format, bei dem die einzelnen Teile der Werbeeinblendungen als einzelnes Element an Ihre APP übermittelt werden.
title: Native Anzeigen
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, Ad Control, Native AD
ms.localizationpriority: medium
ms.openlocfilehash: 417560c9099937324b39a8cdfafb7d62ec7e64e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364083"
---
# <a name="native-ads"></a>Native Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Eine native Anzeige ist ein komponentenbasiertes Anzeigenformat, in denen jedes Element der Anzeige (wie Titel, Bild, Beschreibung und Handlungsaufforderungstext) als einzelnes Element übermittelt wird, das Sie in Ihre App integrieren können. Sie können diese Elemente in Ihre APP integrieren, indem Sie Ihre eigenen Schriftarten, Farben, Animationen und andere UI-Komponenten verwenden, um eine unaufdringliche Benutzererfahrung zu kombinieren, die dem Aussehen und Verhalten Ihrer APP entspricht, während gleichzeitig eine hohe Rendite aus den Werbe Einblicken erzielt wird.

Bei-Werbeeinblendungen bieten native Werbeeinblendungen eine hohe Leistung, da die Werbeeinblendungen eng in die APP integriert sind und die Benutzer daher tendenziell mehr mit diesen Arten von anzeigen interagieren.

> [!NOTE]
> Native Werbung werden derzeit nur für XAML-basierte UWP-Apps für Windows 10 unterstützt. Die Unterstützung für UWP-apps, die mit HTML und JavaScript geschrieben wurden, ist für eine zukünftige Version des Microsoft Advertising SDK geplant.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Integrieren einer nativen AD in Ihre APP

Befolgen Sie diese Anweisungen, um eine native Werbung in Ihre APP zu integrieren und sicherzustellen, dass Ihre Native AD-Implementierung eine Test-Werbe einblabzeige

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.
    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package. appxmanifest" in Ihrem Projekt, und stellen Sie sicher, dass die Funktion " **Internet (Client)** " ausgewählt ist. Ihre APP benötigt diese Funktion zum Empfangen von Test-und Live-Werbeeinblendungen.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Wenn Ihr Projekt auf **eine beliebige CPU**ausgerichtet ist, können Sie in den folgenden Schritten keinen Verweis auf das Microsoft Advertising SDK hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Fügen Sie einen Verweis auf das Microsoft Advertising SDK in Ihrem Projekt hinzu:

    1. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen... aus.**
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

4. Fügen Sie in der entsprechenden Codedatei in der APP (z. b. in MainPage.XAML.cs oder einer Codedatei für eine andere Seite) die folgenden Namespace Verweise hinzu.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Namespaces":::

5.  Deklarieren Sie an einem geeigneten Speicherort in Ihrer APP (z. b. in ```MainPage``` oder einer anderen Seite) ein [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) -Objekt und mehrere Zeichen folgen Felder, die die Anwendungs-ID und die AD-Einheits-ID für Ihre Native AD darstellen. Im folgenden Codebeispiel werden die `myAppId` `myAdUnitId` Felder und den [Testwerten](set-up-ad-units-in-your-app.md#test-ad-units) für Native ADS zugewiesen.
    > [!NOTE]
    > Jede **NativeAdsManagerV2** verfügt über eine entsprechende *Ad-Einheit* , die von unseren Diensten zum Bereitstellen von Werbeeinblendungen für das systemeigene AD-Steuerelement verwendet wird, und jede Ad-Einheit besteht aus einer Ad-Einheiten- *ID* *application ID* In den folgenden Schritten weisen Sie dem Steuerelement Test Ad Unit ID-und Anwendungs-ID-Werte zu. Diese Testwerte können nur in einer Testversion der APP verwendet werden. Bevor Sie Ihre APP im Store veröffentlichen, müssen Sie [diese Testwerte durch livewerte](#release) aus Partner Center ersetzen.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Variables":::

6.  Instanziieren Sie in Code, der beim Start ausgeführt wird (z. b. im Konstruktor für die Seite), das **NativeAdsManagerV2** -Objekt, und verknüpfen Sie Ereignishandler für die Ereignisse " **adready** " und " **erroreingetreten** " des Objekts.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ConfigureNativeAd":::

7.  Wenn Sie bereit sind, eine native Werbung anzuzeigen, rufen Sie die **requestad** -Methode auf, um eine Werbung abzurufen.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="RequestAd":::

8.  Wenn eine native Werbung für Ihre APP bereit ist, wird Ihr [adready](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) -Ereignishandler aufgerufen, und ein [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) -Objekt, das den nativen AD darstellt, wird an den *e* -Parameter übergeben. Verwenden Sie die **NativeAdV2** -Eigenschaften, um jedes Element des nativen AD zu erhalten, und zeigen Sie diese Elemente auf der Seite an. Stellen Sie sicher, dass Sie auch die **registeradcontainer** -Methode aufrufen, um das Benutzeroberflächen Element zu registrieren, das als Container für das Native AD fungiert. Dies ist zum ordnungsgemäßen Nachverfolgen von werbeeindrücken und Klicks erforderlich.
    > [!NOTE]
    > Einige Elemente des nativen AD sind erforderlich und müssen immer in der App angezeigt werden. Weitere Informationen finden Sie in unseren [Richtlinien für Native Werbung](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Nehmen Sie beispielsweise an, dass Ihre APP eine ```MainPage``` (oder eine andere Seite) mit dem folgenden **StackPanel**enthält. Dieses **StackPanel** enthält eine Reihe von Steuerelementen, die verschiedene Elemente einer nativen Werbung anzeigen, einschließlich Titel, Beschreibung, Bilder, *von Text gesponsert* , und eine Schaltfläche, mit der der Aktions Text *aufgerufen* wird.

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

    Im folgenden Codebeispiel wird ein **adready** -Ereignishandler veranschaulicht, der jedes Element der nativen Anzeige in den Steuerelementen im **StackPanel** anzeigt und dann die **registeradcontainer** -Methode aufruft, um das **StackPanel**zu registrieren. Bei diesem Code wird davon ausgegangen, dass er aus der Code Behind-Datei für die Seite ausgeführt wird, die das **StackPanel**-Element enthält.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="AdReady":::

9.  Definieren eines Ereignis Handlers für das Ereignis **erroreingetreten** zum Behandeln von Fehlern im Zusammenhang mit dem systemeigenen AD. Im folgenden Beispiel werden Fehlerinformationen während des Tests in das Visual Studio- **Ausgabe** Fenster geschrieben.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ErrorOccurred":::

10.  Kompilieren Sie die APP, und führen Sie Sie aus.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Sie Ihre APP mit Live Werbung

Nachdem Sie sich vergewissert haben, dass Ihre Native AD-Implementierung erfolgreich eine Test-Werbeeinblendungen anzeigt, befolgen Sie diese Anweisungen, um Ihre APP so zu konfigurieren, dass Sie echte Werbung anzeigt und die aktualisierte APP

1.  Stellen Sie sicher, dass Ihre Native AD-Implementierung unsere [Richtlinien für Native Werbung](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)befolgt.

2.  Wechseln Sie in Partner Center zur Seite [in-App-ADS](../publish/in-app-ads.md) , und [Erstellen Sie eine Ad-Einheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Ad Unit Type den Typ **native**an. Notieren Sie sich die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-ID-Werte für Test-und Live-UWP-Ad-Einheiten haben unterschiedliche Formate. Testanwendungs-ID-Werte sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

3. Optional können Sie die AD-Vermittlung für das Native AD aktivieren, indem Sie die Einstellungen im Abschnitt " [Vermittlungs Einstellungen](../publish/in-app-ads.md#mediation) " auf der Seite " [in-App-Werbung](../publish/in-app-ads.md) " konfigurieren. Die AD-Vermittlung ermöglicht es Ihnen, ihre Werbe-und App-herauf Stufungs Funktionen zu maximieren, indem Sie Werbeeinblendungen

4.  Ersetzen Sie in Ihrem Code die Test-Ad-Einheiten Werte (d. h. die Parameter *ApplicationId* und *adunitid* des [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) -Konstruktors) durch die im Partner Center generierten livewerte.

5.  Über [Mitteln Sie Ihre APP](../publish/app-submissions.md) mithilfe von Partner Center an den Store.

6.  Überprüfen Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) in Partner Center.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Verwalten von Ad-Einheiten für mehrere Native ADS in Ihrer APP

Sie können mehrere Native AD-Platzierungen in einer einzelnen App verwenden. In diesem Szenario wird empfohlen, jeder nativen Ad-Platzierung eine andere Ad-Einheit zuzuweisen. Wenn Sie verschiedene Ad-Einheiten für das Native AD verwenden, können Sie [die Vermittlungs Einstellungen separat konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md) für jedes Steuerelement erhalten. Dadurch können unsere Dienste auch die Werbeeinblendungen, die wir für Ihre APP bereitstellen, besser optimieren.

> [!IMPORTANT]
> Sie können jede Ad-Einheit nur in einer App verwenden. Wenn Sie eine Ad-Einheit in mehr als einer App verwenden, werden ADS nicht für diese Ad-Einheit bereitgestellt.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Native Werbung](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [In-App-Anzeigen](../publish/in-app-ads.md)
* [Einrichten von Ad-Einheiten für Ihre APP](set-up-ad-units-in-your-app.md)
