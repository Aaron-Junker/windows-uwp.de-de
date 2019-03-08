---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Erfahren Sie, wie Sie die Anwendung-ID und Ad Einheit-ID Werte aus der Partner Center zu Ihrer app hinzufügen, bevor Sie Ihre app an den Store übermitteln.
title: Einrichten von Anzeigeneinheiten in der App
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Anzeigeeinheiten, Test
ms.localizationpriority: medium
ms.openlocfilehash: b2d01434e508d4a5067ffd66bdf86b3083b43016
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651575"
---
# <a name="set-up-ad-units-in-your-app"></a>Einrichten von Anzeigeneinheiten in der App

Jedes Steuerelement für eine universelle Windows Platform (UWP) in Ihrer App verfügt über eine entsprechende *Anzeigeneinheit*. Diese wird von unseren Diensten verwendet, um Werbung auf das Steuerelement zu leiten. Jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*, die Sie Code in Ihrer App zuweisen müssen.

Wir bieten [Testanzeigen-Einheitenwerte](#test-ad-units), die Sie während der Testphase verwenden können, um sicherzustellen, dass Ihre App Testanzeigen anzeigt. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Wenn Sie Testwerte in einer veröffentlichten App verwenden, wird die App keine Livewerbung empfangen.

Nach Abschluss der Tests die UWP-app, und Sie können sie zum Partner Center zu übermitteln, müssen Sie [erstellen Sie eine live-Ad-Einheit](#live-ad-units) aus der [In-app-Werbung](../publish/in-app-ads.md) Seite im Partner Center, und aktualisieren Sie Ihren app-Code, um die Anwendung zu verwenden. ID und Einheit ID-Werte für diese Ad-Einheit.

Weitere Informationen zum Zuweisen der Anwendungs-ID- und Anzeigeneinheits-ID-Werte in Ihrem App-Code finden Sie in den folgenden Artikeln:
* [Adcontrol-Element in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol-Element in HTML 5- und Javascript](adcontrol-in-html-5-and-javascript.md)
* [Interstitialwerbung](../monetize/interstitial-ads.md)
* [Native Anzeigen](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Testen von Anzeigeeinheiten

Während Sie Ihre App entwickeln, verwenden Sie die entsprechende Testanwendungs-ID und Anzeigenblock-ID, um zu beobachten, wie die Werbung während der Testphase von Ihrer App gerendert wird.

### <a name="banner-ads-using-the-adcontrol-class"></a>Banneranzeigen (unter Verwendung der AdControl-Klasse)

* AD-Einheit-ID: ```test```
* Anwendungs-ID:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Für ein **AdControl** wird die Größe einer Liveanzeige durch die Eigenschaften **Width** und **Height** bestimmt. Um optimale Ergebnisse zu erzielen, stellen Sie sicher, dass die Eigenschaften **Width** und **Height** zu den [unterstützten Eigenschaften für Banneranzeigen](supported-ad-sizes-for-banner-ads.md) gehören. Die Eigenschaften **Width** und **Height** ändern sich nicht entsprechend der Größe einer Liveanzeige.

### <a name="interstitial-ads-and-native-ads"></a>Interstitialwerbung und native Anzeigen

* AD-Einheit-ID: ```test```
* Anwendungs-ID:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Live-Anzeigeneinheiten

Eine live-Ad-Einheit von Partner Center und in Ihrer app verwenden:

1.  [Erstellen Sie eine Werbeeinheit](../publish/in-app-ads.md#create-ad-unit) auf die **In-app-Werbung** Seite im Partner Center. Achten Sie darauf, dass Sie den richtigen Typ der Anzeigeneinheit für das Ad-Steuerelement angeben, die Sie in Ihrer App verwenden.
    > [!NOTE]
    > Sie können optional die Anzeigenvermittlung für Ihre Anzeigeneinheit durch das Konfigurieren der Einstellungen im Abschnitt [Vermittlungseinstellungen](../publish/in-app-ads.md#mediation). Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken und Anzeigen zu Werbekampagnen für Microsoft-Apps. Wir wählen standardmäßig die Einstellungen der Anzeigenvermittlung für Ihre App mithilfe von Machine Learning Algorithmen aus, um Ihnen beim Optimieren der Anzeigenumsätze in den verschiedenen Märkten zu helfen, die Ihre App unterstützt. Sie können optional die Einstellungen für die Anzeigenvermittlung auch manuell konfigurieren.

2.  Nach der Erstellung der neuen Ad-Einheit Abrufen der **Anwendungs-ID** und **Ad-Einheit-ID** für die Ad-Einheit in der Tabelle der verfügbaren Ad Einheiten in der **Monetize** &gt;  **In-app-Werbung** Seite.
    > [!NOTE]
    > Die Anwendungs-IDs für Test-Anzeigeneinheiten und Live-UWP-Anzeigeneinheiten besitzen unterschiedliche Formate. Testanwendungs-ID sind GUIDs. Bei der Erstellung einer UWP-Ad liveunittests im Partner Center entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer die Store-ID für Ihre app (ein Beispiel für Store-ID-Wert ist 9NBLGGH4R315 dargestellt).

3.  Weisen Sie die Anwendungs-ID- und Anzeigeneinheits-ID-Werte in Ihrem App Code zu. Weitere Informationen finden Sie in den folgenden Artikeln:
    * [Adcontrol-Element in XAML und .NET](adcontrol-in-xaml-and--net.md)
    * [Adcontrol-Element in HTML 5- und Javascript](adcontrol-in-html-5-and-javascript.md)
    * [Interstitialwerbung](../monetize/interstitial-ads.md)
    * [Native Anzeigen](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Anzeigeeinheiten für mehrere Steuerelemente von Anzeigen in Ihrer App

Können mehrere Steuerelemente für Banner-, Interstitialwerbungen und native Anzeigen in einer einzelnen App verwenden. In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

> [!IMPORTANT]
> Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

## <a name="related-topics"></a>Verwandte Themen

* [Adcontrol-Element in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol-Element in HTML 5- und Javascript](adcontrol-in-html-5-and-javascript.md)
* [Interstitialwerbung](interstitial-ads.md)
* [Native Anzeigen](native-ads.md)


 

 
