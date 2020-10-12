---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Das Microsoft Advertising SDK bietet Ihnen mehrere Möglichkeiten, Ihre APP mit Werbung zu verdienen.
title: Anzeigen von Werbung mithilfe des Microsoft Advertising-SDK in der App
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, Banner, Ad Control, Interstitial
ms.localizationpriority: medium
ms.openlocfilehash: c12d79b97010826b05bf42a9de46780dd2f93756
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933121"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Anzeigen von Werbung mithilfe des Microsoft Advertising-SDK in der App

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Erhöhen Sie Ihre Verkaufschancen, indem Sie mit dem Microsoft Advertising SDK in ihrer universelle Windows-Plattform-app (UWP) für Windows 10 anzeigen. Unsere AD-Monetarisierungsplattform bietet eine Vielzahl von AD-Formaten, die nahtlos in ihre Apps integriert werden können und die Vermittlung mit vielen gängigen Ad-Netzwerken unterstützt. Unsere Plattform ist mit den Standards von OpenRTB, VAST 2.x, MRAID 2 und VPAID 3, sowie mit MOAT und IAS kompatibel. 

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
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Installieren des Microsoft Advertising SDK</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Leitfaden für Entwickler</b><br/><br/>
    <a href="banner-ads.md">Banner Anzeigen</a>
    <br/>
    <a href="interstitial-ads.md">Interstitial-anzeigen</a>
    <br/>
    <a href="native-ads.md">Native Werbung</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Weitere Ressourcen</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Einrichten von Ad-Einheiten in Ihrer APP</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Bewährte Methoden</a>
    <br/>
    <a href="/uwp/api/overview/advertising">API-Referenz</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Schritt 1: Installieren des Microsoft Advertising SDK

Installieren Sie zunächst das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) auf dem Entwicklungs Computer, den Sie zum Erstellen Ihrer APP verwenden. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Schritt 2: Implementieren von anzeigen in Ihrer APP

Das Microsoft Advertising SDK bietet verschiedene Typen von AD-Steuerelementen, die Sie in Ihrer APP verwenden können. Wählen Sie aus, welche Arten von Werbeeinblendungen für Ihr Szenario am besten geeignet sind, und fügen Sie dann der app Code hinzu In diesem Schritt verwenden Sie eine Test-Ad-Einheit, damit Sie sehen können, wie Ihre APP während des Tests anzeigen rendert.

### <a name="banner-ads"></a>Banneranzeigen

Dabei handelt es sich um statische Anzeige anzeigen, die einen rechteckigen Teil einer Seite in Ihrer APP verwenden, um Aktions Inhalte anzuzeigen. Diese anzeigen können in regelmäßigen Abständen automatisch aktualisiert werden. Dies ist ein guter Ausgangspunkt, wenn Sie mit der Werbung in Ihrer APP noch nicht vertraut sind.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](adcontrol-in-xaml-and--net.md).

![Ein Bild, das eine Banner Ankündigung auf einem Tablet darstellt.](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Video-und Interstitial-Banner Anzeigen mit Interstitial

Dabei handelt es sich um Vollbild-Werbeanzeigen, bei denen der Benutzer in der Regel ein Video ansehen oder durch diese klicken kann, um die APP oder das Spiel fortzusetzen. Wir unterstützen zwei Arten von Interstitial-Werbeeinblendungen: Video und Banner.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](interstitial-ads.md).

![Ein Bild, das eine Interstitial-Ankündigung in einem Spiel darstellt, das auf einem Tablet abgespielt wird.](images/interstitial-ad.png)

### <a name="native-ads"></a>Native Anzeigen

Dabei handelt es sich um komponentenbasierte Werbung. Die einzelnen Teile der kreativen Werbeaktion (z. b. Titel, Bild, Beschreibung und Aktions Text) werden an Ihre APP als einzelnes Element übermittelt, das Sie in Ihre APP integrieren können, indem Sie Ihre eigenen Schriftarten, Farben und andere UI-Komponenten verwenden.

Anweisungen und Codebeispiele finden Sie in [diesem Artikel](native-ads.md).

![Ein Bild, das eine systemeigene Ankündigung darstellt, die auf verschiedenen Geräten angezeigt werden kann.](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Schritt 3: Erstellen einer Ad-Einheit und Konfigurieren der Vermittlung

Nachdem Sie das Testen der APP abgeschlossen haben und Sie bereit sind, Sie an den Store zu übermitteln, erstellen Sie eine Ad-Einheit auf der Seite " [in-App-Werbung](../publish/in-app-ads.md) " im Partner Center. Aktualisieren Sie dann Ihren app-Code so, dass diese Ad-Einheit verwendet wird, damit Ihre APP Live Werbung empfängt. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

Standardmäßig zeigt Ihre APP Anzeigen von Werbeeinblendungen aus dem Microsoft-Netzwerk an. Um Ihren AD-Umsatz zu maximieren, können Sie [AD-Vermittlung](ad-mediation-service.md) für Ihre Ad-Einheit aktivieren, um Werbung von zusätzlichen kostenpflichtigen Ad-Netzwerken wie Taboola und smaato anzuzeigen. Sie können Ihre APP-herauf Stufungs Funktionen auch erhöhen, indem Sie Werbung aus Microsoft App Promotion-Kampagnen anordnen.

Um mit der Verwendung von AD-Mediation in ihrer UWP-APP zu beginnen, [Konfigurieren Sie AD-Vermittlungs Einstellungen](../publish/in-app-ads.md#mediation-settings) für Ihre Ad-Einheit Standardmäßig werden die Vermittlungs Einstellungen mithilfe von Machine Learning-Algorithmen automatisch konfiguriert, damit Sie Ihre Werbeeinnahmen in den von Ihrer APP unterstützten Märkten maximieren können. Sie haben jedoch auch die Möglichkeit, die Netzwerke, die Sie verwenden möchten, manuell auszuwählen. In beiden Fällen werden die Vermittlungs Einstellungen auf unseren Servern vollständig konfiguriert. Sie müssen keine Codeänderungen in Ihrer APP vornehmen.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Schritt 4: übermitteln der APP und Überprüfen der Leistung

Nachdem Sie die Entwicklung Ihrer APP mit anzeigen abgeschlossen haben, können Sie [Ihre aktualisierte APP](../publish/app-submissions.md) im Partner Center einreichen, um Sie im Store verfügbar zu machen. Apps, die ADS anzeigen, müssen die zusätzlichen Anforderungen erfüllen, die in [Abschnitt 10,10 der Microsoft Store Richtlinien](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) und der [App-Entwickler Vereinbarung](/legal/windows/agreements/app-developer-agreement)angegeben sind.

Nachdem Ihre App veröffentlicht und im Store verfügbar ist, können Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) für Ankündigungen im Partner Center überprüfen und weiterhin Änderungen an den Vermittlungs Einstellungen vornehmen, um die Leistung Ihrer Werbeeinblendungen zu optimieren. Ihr Werbe Umsatz ist in Ihrer [Auszahlungs Zusammenfassung](../publish/payout-summary.md)enthalten.

<span id="additional-help" />

## <a name="additional-help"></a>Zusätzliche Hilfe

Verwenden Sie die folgenden Ressourcen, um zusätzliche Hilfe bei der Verwendung des Microsoft Advertising SDK zu erhalten.

|  Aufgabe    | Resource |               
|----------|-------|
| Melden eines Fehlers und Supportunterstützung für Werbung     | Besuchen Sie die [Supportseite](https://developer.microsoft.com/windows/support) , und wählen Sie **ADS-in-Apps aus**.        |
| Community-Support erhalten     | Besuchen des [Forums](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)       |
| Herunterladen von Beispielprojekten, die veranschaulichen, wie Sie Banner- und Interstitialwerbung zu Apps hinzufügen.     | Siehe [Anzeigenbeispiele bei GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Weitere Informationen zu den neuesten Umsatzchancen für Windows-Apps     | Besuchen Sie Seite [Monetarisierung Ihrer Apps](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 und Windows Phone 8. x-apps

Für Windows 8.1-und Windows Phone 8. x-apps stellen wir das [Microsoft Advertising SDK für Windows und Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)bereit. Weitere Informationen zur Verwendung dieses SDK zum Anzeigen von anzeigen in Windows 8.1 und Windows Phone 8. x-apps finden Sie in [diesem Artikel](/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Zugehörige Themen

* [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md)
* [Programm für Herausgeber von Windows Premium-Anzeigen](windows-premium-ads-publishers-program.md)