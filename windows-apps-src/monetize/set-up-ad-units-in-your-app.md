---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Erfahren Sie, wie Sie die Werte von Anwendungs-ID und Ad Unit ID aus Partner Center zu Ihrer APP hinzufügen, bevor Sie Ihre APP an den Store übermitteln.
title: Einrichten von Anzeigeneinheiten in der App
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, Ad-Einheiten, testen
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253628"
---
# <a name="set-up-ad-units-in-your-app"></a>Einrichten von Anzeigeneinheiten in der App

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Jedes AD-Steuerelement in ihrer universelle Windows-Plattform-app (UWP) verfügt über eine entsprechende *Ad-Einheit* , die von unseren Diensten verwendet wird, um Werbung für das Steuerelement bereitzustellen. Jede Ad-Einheit besteht aus einer *Ad-Einheiten-ID* und einer Anwendungs- *ID* , die Sie dem Code in der APP zuweisen müssen.

Wir stellen [Test-Ad-Einheiten Werte](#test-ad-units) bereit, die Sie während des Tests verwenden können, um zu bestätigen, dass Ihre APP Test anzeigen anzeigt. Diese Testwerte können nur in einer Testversion der APP verwendet werden. Wenn Sie Testwerte in einer veröffentlichten App verwenden, wird die App keine Livewerbung empfangen.

Nachdem Sie das Testen der UWP-App abgeschlossen haben und Sie bereit sind, Sie an Partner Center zu übermitteln, müssen Sie auf der Seite [in-App-Werbung](../publish/in-app-ads.md) im Partner Center [eine aktive Ad-Einheit erstellen](#live-ad-units) und Ihren app-Code aktualisieren, um die Werte für Anwendungs-ID und Ad Unit ID für diese Ad-Einheit zu verwenden.

Weitere Informationen zum Zuweisen von Anwendungs-ID und Ad Unit ID-Werten im Code Ihrer App finden Sie in den folgenden Artikeln:
* [„AdControl“ in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol in HTML 5 und JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Interstitialwerbung](../monetize/interstitial-ads.md)
* [Native Anzeigen](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Ad-Einheiten testen

Wenn Sie Ihre APP entwickeln, verwenden Sie die Werte für testanwendungs-ID und Ad Unit ID aus diesem Abschnitt, um zu erfahren, wie Ihre APP während des Tests anzeigen rendert.

### <a name="banner-ads-using-the-adcontrol-class"></a>Banner Anzeigen (mithilfe der adcontrol-Klasse)

* Ad-Einheiten-ID: ```test```
* Anwendungs-ID:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Bei einem **adcontrol**wird die Größe einer Live-Werbe Einwirkung durch die Eigenschaften **Width** und **height** definiert. Um optimale Ergebnisse zu erzielen, stellen Sie sicher, dass die Eigenschaften **Width** und **Height** zu den [unterstützten Eigenschaften für Banneranzeigen](supported-ad-sizes-for-banner-ads.md) gehören. Die Eigenschaften **Width** und **Height** ändern sich nicht entsprechend der Größe einer Liveanzeige.

### <a name="interstitial-ads-and-native-ads"></a>Interstitial-anzeigen und Native Werbung

* Ad-Einheiten-ID: ```test```
* Anwendungs-ID:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Live-Ad-Einheiten

So erhalten Sie eine Live-Ad-Einheit von Partner Center und verwenden Sie in Ihrer APP:

1.  [Erstellen Sie eine Ad-Einheit](../publish/in-app-ads.md#create-ad-unit) auf der Seite " **in-App-Werbung** " im Partner Center. Stellen Sie sicher, dass Sie den richtigen Typ der Ad-Einheit für das AD-Steuerelement angeben, das Sie in Ihrer APP verwenden.
    > [!NOTE]
    > Optional können Sie die AD-Vermittlung für Ihre Ad-Einheit aktivieren, indem Sie die Einstellungen im Abschnitt " [Vermittlungs Einstellungen](../publish/in-app-ads.md#mediation) " konfigurieren. Die AD-Vermittlung ermöglicht es Ihnen, ihre Werbe-und App-herauf Stufungs Funktionen zu maximieren, indem Sie Werbeeinblendungen aus mehreren Ad-Netzwerken anzeigen, einschließlich Werbung aus anderen kostenpflichtigen Ad-Netzwerken Standardmäßig wählen wir automatisch die AD-Vermittlungs Einstellungen für Ihre App mithilfe von Machine Learning-Algorithmen aus, um Ihnen zu helfen, ihre Werbeeinnahmen über die von Ihrer APP unterstützten Märkte hinweg zu maximieren, aber Sie können die Vermittlungs Einstellungen optional auch manuell konfigurieren.

2.  Nachdem Sie die neue Ad-Einheit erstellt haben, rufen Sie die **Anwendungs-ID** und die Ad-Einheiten- **ID** für die Ad-Einheit in der Tabelle der verfügbaren Ad-Einheiten auf der Seite " **Monetize** &gt; **in-App-anzeigen** " auf.
    > [!NOTE]
    > Die Anwendungs-ID-Werte für Test-und Live-UWP-Ad-Einheiten haben unterschiedliche Formate. Testanwendungs-ID-Werte sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

3.  Weisen Sie die Werte für Anwendungs-ID und Ad Unit ID dem App-Code zu. Weitere Informationen finden Sie in den folgenden Artikeln:
    * [„AdControl“ in XAML und .NET](adcontrol-in-xaml-and--net.md)
    * [Adcontrol in HTML 5 und JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Interstitialwerbung](../monetize/interstitial-ads.md)
    * [Native Anzeigen](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Ad-Einheiten für mehrere AD-Steuerelemente in Ihrer APP

Sie können mehrere Banner, Interstitial und Native AD-Steuerelemente in einer einzelnen App verwenden. In diesem Szenario wird empfohlen, jedem Steuerelement eine andere Ad-Einheit zuzuweisen. Wenn Sie verschiedene Ad-Einheiten für jedes Steuerelement verwenden, können Sie [die Vermittlungs Einstellungen separat konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md) für jedes Steuerelement erhalten. Dadurch können unsere Dienste auch die Werbeeinblendungen, die wir für Ihre APP bereitstellen, besser optimieren.

> [!IMPORTANT]
> Sie können jede Ad-Einheit nur in einer App verwenden. Wenn Sie eine Ad-Einheit in mehr als einer App verwenden, werden ADS nicht für diese Ad-Einheit bereitgestellt.

## <a name="related-topics"></a>Zugehörige Themen

* [„AdControl“ in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol in HTML 5 und JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Interstitialwerbung](interstitial-ads.md)
* [Native Anzeigen](native-ads.md)


 

 
