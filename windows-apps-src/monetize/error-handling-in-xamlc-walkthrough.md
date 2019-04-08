---
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Hier erfahren Sie, wie AdControl-Fehler in Ihrer App aufgefangen werden.
title: Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeige, Werbung, Fehlerbehandlung, XAML, C#
ms.localizationpriority: medium
ms.openlocfilehash: fe01a01dc97b67c339d015a260288176a5ba57da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642145"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Anzeigen-bezogene Fehler in Ihrer App erfasst werden können. In dieser exemplarischen Vorgehensweise wird ein [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) verwendet, um eine Banneranzeige anzuzeigen, die allgemeinen Konzepte gelten jedoch auch für Interstitialwerbung und native Anzeigen.

In diesen Beispielen wird davon ausgegangen, dass Sie eine XAML/C#-App haben, die ein **AdControl** enthält. Schritt-für-Schritt-Anleitungen, die zeigen, wie ein **AdControl** zu Ihrer App hinzugefügt wird, finden Sie unter [AdControl in XAML und .NET](adcontrol-in-xaml-and--net.md). 

1.  Suchen Sie in der Datei "MainPage.xaml" nach der Definition für das **AdControl**. Dieser Code sieht folgendermaßen aus.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   Weisen Sie nach der Eigenschaft **Width**, jedoch vor dem Endtag, dem Ereignis [ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) den Namen eines Fehlerereignishandlers zu. In dieser exemplarischen Vorgehensweise ist der Name des Fehlerereignishandlers **OnAdError**.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  Um einen Fehler zur Laufzeit zu generieren, erstellen Sie ein zweites **AdControl**-Element mit einer anderen Anwendungs-ID. Da alle **AdControl**-Objekte in einer Anwendung die gleiche Anwendungs-ID verwenden müssen, wird durch das Erstellen eines zusätzlichen **AdControl** mit einer anderen Anwendungs-ID ein Fehler ausgelöst.

    Definieren Sie ein zweites **AdControl** in der Datei "MainPage.xaml" direkt nach dem ersten **AdControl**, und legen Sie für die Eigenschaft [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) den Wert "0" fest.
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  Fügen Sie in der Datei "MainPage.xaml.cs" den folgenden Ereignishandler **OnAdError** der **MainPage**-Klasse hinzu. Dieser Ereignishandler schreibt Informationen in das Visual Studio **Ausgabe**-Fenster.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Erstellen Sie das Projekt, und führen Sie es aus. Nach Ausführen der Anwendung wird eine Meldung ähnlich der Meldung unten im **Ausgabe**-Fenster von Visual Studio angezeigt.
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele auf GitHub](https://aka.ms/githubads)
