---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Mit dem Microsoft Advertising-SDK haben Sie mehrere Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen.
title: Anzeigen von Werbung mithilfe der Microsoft Advertising-SDK in der App
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, banner, Anzeigensteuerelement,Interstitial
ms.localizationpriority: medium
ms.openlocfilehash: d693b8a4e07e66eded7d1580291c552e7ab9ca64
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507224"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Anzeigen von Werbung mithilfe der Microsoft Advertising-SDK in der App

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Erhöhen Sie Ihre Umsatzchancen, indem Sie mithilfe des Microsoft Advertising-SDKs Anzeigen in Ihre universelle Windows-Plattform-App für Windows 10 einfügen. Unsere AD-Monetarisierungsplattform bietet eine Vielzahl von AD-Formaten, die nahtlos in ihre Apps integriert werden können und die Vermittlung mit vielen gängigen Ad-Netzwerken unterstützt. Unsere Plattform ist mit den Standards von OpenRTB, VAST 2.x, MRAID 2 und VPAID 3, sowie mit MOAT und IAS kompatibel. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>Erste Schritte</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Installieren Sie das Microsoft Advertising SDK</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Leitfaden für Entwickler</b><br/><br/>
    <a href="banner-ads.md">Banner Anzeigen</a>
    <br/>
     von " 
    <a href="interstitial-ads.md">Interstitial ADS</a> "<br/>
    <a href="native-ads.md">Native ADS</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Weitere Ressourcen</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Einrichten von Ad-Einheiten in Ihrer APP</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Bewährte Methoden</a>
    <br/>
    <a href="https://docs.microsoft.com/uwp/api/overview/advertising">API-Referenz</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Schritt 1: Installieren des Microsoft Advertising-SDK

Installieren Sie zunächst die [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) auf dem Entwicklungscomputer, den Sie verwenden, um Ihre App zu entwickeln. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Schritt 2: Implementieren von Werbung in Ihrer App

Das Microsoft Advertising-SDK enthält verschiedene Arten von Anzeigenkontrollen, die in Ihrer App verwendet werden können. Wählen Sie, welche Arten von Anzeigen für Ihr Szenario am besten geeignet sind, und fügen Sie Code für Ihre App hinzu, um diese Anzeigen darzustellen. In diesem Schritt verwenden Sie eine Testanzeigeneinheit, um zu sehen, wie Ihre App die Anzeigen während der Testphase rendert.

### <a name="banner-ads"></a>Banneranzeigen

Es handelt sich um statische Anzeigen von Werbung, die einen rechteckigen Teil einer Seite in Ihrer App zum Anzeigen von Werbeinhalten nutzen. Diese Anzeigen können in regelmäßigen Abständen automatisch aktualisiert werden. Sie sind ein guter Ausgangspunkt, wenn Sie noch nicht mit Werbung in Ihrer App vertraut sind.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Interstitialwerbung durch Videos und Banneranzeigen

Interstitialanzeigen sind Vollbildanzeigen, durch die der Benutzer in der Regel gezwungen wird, sich ein Video anzusehen oder sich durch die Anzeige zu klicken, um mit der App oder dem Spiel fortfahren zu können. Wir unterstützen zwei Arten von Interstitialanzeigen: Videos und Banner.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Native Anzeigen

Hierbei handelt es sich um komponentenbasierte Anzeigen anzeigen. Jeder Teil des Anzeigen-Creatives (z. B. Titel, Bild, Beschreibung und Handlungsaufforderungstext) wird als einzelnes Element an Ihre App übermittelt. Sie können die Elemente mit eigenen Schriften, Farben und anderen UI-Komponenten in Ihre App integrieren.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Schritt 3: Erstellen einer Anzeigeneinheit und Konfigurieren der Anzeigenvermittlung

Nachdem Sie das Testen der APP abgeschlossen haben und Sie bereit sind, Sie an den Store zu übermitteln, erstellen Sie eine Ad-Einheit auf der Seite " [in-App-Werbung](../publish/in-app-ads.md) " im Partner Center. Aktualisieren Sie anschließend Ihren App-Code, um diese Anzeigeneinheit zu verwenden, damit Ihre App Live-Anzeigen empfängt. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

Standardmäßig zeigt Ihre App Werbung der Microsoft Netzwerke für kostenpflichtige Werbeanzeigen an. Zur Maximierung Ihres Anzeigenumsatzes können Sie für Ihre Anzeigeneinheit die [Anzeigenvermittlung](ad-mediation-service.md) aktivieren, um kostenpflichtige Anzeigen von weiteren Anzeigennetzwerken anzuzeigen (z. B. Taboola und Smaato). Sie können Ihrer App-Werbung auch steigern, indem Sie Anzeigen aus Microsoft App-Werbekampagnen darstellen.

Zum Starten der Anzeigenvermittlung in Ihrer UWP-App [Konfigurieren Sie die Anzeigenvermittlungseinstellungen](../publish/in-app-ads.md#mediation-settings) für Ihre Anzeigeneinheit. Standardmäßig werden die Einstellungen für die Anzeigenvermittlung automatisch mithilfe von Machine Learning-Algorithmen konfiguriert, die ihnen bei der Optimierung der Anzeigenumsätze in den verschiedenen Märkten helfen, die Ihre App unterstützt. Sie haben jedoch auch die Möglichkeit, die gewünschten Netzwerke manuell auszuwählen. In beiden Fällen werden die Einstellungen für die Anzeigenvermittlung vollständig auf unseren Servern konfiguriert; Sie müssen keine Codes in Ihrer App ändern.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Schritt 4: Übermitteln der App und Überprüfen der Leistung

Nachdem Sie die Entwicklung Ihrer APP mit anzeigen abgeschlossen haben, können Sie [Ihre aktualisierte APP](https://docs.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center einreichen, um Sie im Store verfügbar zu machen. Apps, die Anzeigen darstellen, müssen zusätzlich die Anforderungen erfüllen, die in [Abschnitt 10.10 der Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content)[Anlage E der Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) angegeben sind.

Nachdem Ihre App veröffentlicht und im Store verfügbar ist, können Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) für Ankündigungen im Partner Center überprüfen und weiterhin Änderungen an den Vermittlungs Einstellungen vornehmen, um die Leistung Ihrer Werbeeinblendungen zu optimieren. Der Umsatz befindet sich in der [Auszahlungszusammenfassung](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Zusätzliche Hilfe

Weitere Hilfe zum Microsoft Advertising-SDK finden Sie in den folgenden Ressourcen.

|  Aufgabe    | Ressource |               
|----------|-------|
| Melden eines Fehlers und Supportunterstützung für Werbung     | Besuchen Sie die [Supportseite](https://developer.microsoft.com/windows/support), und wählen Sie **Werbung in Apps**.        |
| Community-Support erhalten     | Besuchen des [Forums](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)       |
| Herunterladen von Beispielprojekten, die veranschaulichen, wie Sie Banner- und Interstitialwerbung zu Apps hinzufügen.     | Siehe [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Weitere Informationen zu den neuesten Umsatzchancen für Windows-Apps     | Besuchen Sie Seite [Monetarisierung Ihrer Apps](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 und Windows Phone 8.x-Apps

Für Apps für Windows 8.1 und Windows Phone 8.x bieten wir das [Microsoft Advertising-SDK for Windows and Windows Phone 8.x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Weitere Informationen zur Verwendung des SDKs für Anzeigen in einer Windows 8.1- oder Windows Phone 8.x-App finden Sie in [diesem Artikel](https://docs.microsoft.com/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Verwandte Themen

* [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md)
* [Windows Premium ADS-Verleger Programm](windows-premium-ads-publishers-program.md)
