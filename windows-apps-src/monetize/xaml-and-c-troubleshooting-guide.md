---
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: Hier finden Sie Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in XAML-Apps.
title: XAML- und C#-Handbuch zur Problembehandlung
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, UWP, Werbung, Advertising, AdControl, Problembehandlung, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 94af97e1e678c6ee92aaa731057c5f67185e25c5
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334658"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>XAML- und C#-Handbuch zur Problembehandlung

Dieses Thema enthält Lösungen für allgemeine Entwicklungsprobleme mit den Microsoft Advertising-Bibliotheken in XAML-Apps.

* [XAML](#xaml)
  * [Adcontrol-Element nicht angezeigt](#xaml-notappearing)
  * [Blackbox blinkt und ausgeblendet wird](#xaml-blackboxblinksdisappears)
  * [Anzeigen, die nicht aktualisiert](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [Adcontrol-Element nicht angezeigt](#csharp-adcontrolnotappearing)
  * [Blackbox blinkt und ausgeblendet wird](#csharp-blackboxblinksdisappears)
  * [Anzeigen, die nicht aktualisiert](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen übereinstimmen, die Anwendungs-ID und die Ad-Einheit-ID, die Sie im Partner Center zu erhalten. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Überprüfen Sie die **Height**-Eigenschaft und **Width**-Eigenschaft. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Überprüfen Sie die Elementposition. [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) muss sich im sichtbaren Bereich befinden.

5.  Überprüfen Sie die **Visibility**-Eigenschaft. Die optionale **Visibility**-Eigenschaft darf nicht auf „collapsed“ oder „hidden“ festgelegt werden. Diese Eigenschaft kann als Inlineeigenschaft (wie unten dargestellt) oder in einem externen Stylesheet festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Überprüfen Sie das übergeordnete Element von **AdControl**. Wenn sich das **AdControl**-Element in einem übergeordneten Element befindet, muss das übergeordnete Element aktiv und sichtbar sein.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  Stellen Sie sicher, dass **AdControl** im Viewport nicht ausgeblendet ist. **AdControl** muss sichtbar sein, damit Anzeigen ordnungsgemäß dargestellt werden.

8.  Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie [test values](set-up-ad-units-in-your-app.md#test-ad-units) sowohl für **ApplicationId** und **AdUnitId**.

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im vorherigen Abschnitt [AdControl wird nicht angezeigt](#xaml-notappearing).

2.  Behandeln Sie das **ErrorOccurred**-Ereignis, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie unter [Fehlerbehandlung in XAML/Exemplarische Vorgehensweise für C#](error-handling-in-xamlc-walkthrough.md).

    Dieses Beispiel veranschaulicht einen **ErrorOccurred**-Ereignishandler. Der erste Ausschnitt ist das XAML-UI-Markup.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Dieses Beispiel veranschaulicht den entsprechenden C#-Code.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) verhält sich normal.

    **AdControl** wird standardmäßig reduziert, wenn keine Anzeige dargestellt werden kann. Wenn andere Elemente demselben übergeordneten Element untergeordnet sind, können sie verschoben werden, um den freien Platz des reduzierten **AdControl**-Elements zu füllen, und bei der nächsten Anforderung erweitert werden.

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie die [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled)-Eigenschaft. Diese optionale Eigenschaft ist standardmäßig auf **True** festgelegt. Beim Wert **False** muss die [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh)-Methode verwendet werden, um eine weitere Anzeige abzurufen.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Überprüfen Sie Aufrufe der **Refresh**-Methode. Bei Verwendung der automatischen Aktualisierung kann **Refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung sollte **Refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    In den folgenden Codeausschnitten wird die Verwendung der **Refresh**-Methode veranschaulicht. Der erste Ausschnitt ist das XAML-UI-Markup.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    Dieser Codeausschnitt zeigt ein Beispiel für den C#-CodeBehind-Code des UI-Markups.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl wird nicht angezeigt

1.  Stellen Sie sicher, dass die **Internet (Client)**-Funktion in „Package.appxmanifest“ ausgewählt ist.

2.  Stellen Sie sicher, dass **AdControl** instanziiert ist. Wenn **AdControl** nicht instanziiert wird, ist es nicht verfügbar.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  Überprüfen Sie die ID der Anwendung und der Anzeigeneinheit. Diese IDs müssen übereinstimmen, die Anwendungs-ID und die Ad-Einheit-ID, die Sie im Partner Center zu erhalten. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Überprüfen Sie den **Height**-Parameter und **Width**-Parameter. Diese müssen auf eine der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) festgelegt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Stellen Sie sicher, dass **AdControl** einem übergeordneten Element hinzugefügt wurde. Damit **AdControl** angezeigt wird, muss es einem übergeordneten Steuerelement (z. B. **StackPanel** oder **Grid**) als untergeordnetes Element hinzugefügt werden.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  Überprüfen Sie den **Margin**-Parameter. **AdControl** muss sich im sichtbaren Bereich befinden.

7.  Überprüfen Sie die **Visibility**-Eigenschaft. Die optionale **Visibility**-Eigenschaft muss auf **Visible** festgelegt sein.

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Überprüfen Sie das übergeordnete Element von **AdControl**. Das übergeordnete Element muss aktiv und sichtbar sein.

9. Echte Werte für **ApplicationId** und **AdUnitId** sollten nicht im Emulator getestet werden. Um sicherzustellen, dass **AdControl** erwartungsgemäß funktioniert, verwenden Sie [test values](set-up-ad-units-in-your-app.md#test-ad-units) sowohl für **ApplicationId** und **AdUnitId**.

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Blackbox blinkt und wird ausgeblendet

1.  Überprüfen Sie noch einmal alle Schritte im Abschnitt oben [AdControl wird nicht angezeigt](#csharp-adcontrolnotappearing).

2.  Behandeln Sie das **ErrorOccurred**-Ereignis, und bestimmen Sie anhand der an den Ereignishandler übergebenen Meldung, ob ein Fehler aufgetreten ist und welche Art von Fehler ausgelöst wurde. Weitere Informationen finden Sie unter [Fehlerbehandlung in XAML/Exemplarische Vorgehensweise für C#](error-handling-in-xamlc-walkthrough.md).

    Die folgenden Beispiele veranschaulichen den grundlegenden Code zum Implementieren eines Fehleraufrufs. Durch diesen XAML-Code wird ein **TextBlock**-Element definiert, mit dem die Fehlermeldung angezeigt wird.

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    Durch diesen C#-Code wird die Fehlermeldung abgerufen und in **TextBlock** angezeigt.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    Eine Blackbox wird am häufigsten dadurch verursacht, dass keine Anzeige verfügbar ist. Dieser Fehler bedeutet, dass durch die Anforderung keine Anzeige zurückgegeben werden kann.

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Anzeigen werden nicht aktualisiert

1.  Überprüfen Sie, ob in der [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx)-Eigenschaft **AdControl** auf „false“ festgelegt ist. Diese optionale Eigenschaft ist standardmäßig auf **true** festgelegt. Wenn sie auf **false** festgelegt ist, muss die Methode **Refresh** verwendet werden, um eine weitere Anzeige abzurufen.

2.  Überprüfen Sie Aufrufe der [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx)-Methode. Bei Verwendung der automatischen Aktualisierung ((**IsAutoRefreshEnabled** ist **true**)) kann **Refresh** nicht verwendet werden, um eine weitere Anzeige abzurufen. Bei Verwendung der manuellen Aktualisierung (**IsAutoRefreshEnabled** ist **false**) sollte **Refresh** abhängig von der aktuellen Datenverbindung des Geräts erst nach mindestens 30 bis 60 Sekunden aufgerufen werden.

    Das folgende Beispiel veranschaulicht, wie die **Refresh**-Methode aufgerufen wird.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **AdControl** verhält sich normal. In einigen Fällen wird dieselbe Anzeige mehrmals in Folge angezeigt, wodurch der Eindruck entsteht, dass Anzeigen nicht aktualisiert werden.

 

 
