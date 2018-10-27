---
author: Xansky
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Mit dem Microsoft Advertising-SDK haben Sie mehrere Möglichkeiten zur Monetarisierung Ihrer App mit Anzeigen.
title: Zeigt Werbung mithilfe der Microsoft Advertising-SDK in Ihrer App an
ms.author: mhopkins
ms.date: 06/20/2018
ms.topic: article
keywords: Windows10, UWP, Anzeigen, Werbung, banner, Anzeigensteuerelement,Interstitial
ms.localizationpriority: medium
ms.openlocfilehash: 738c643f3c83a4f88f5c52c7337c467366ac8fe5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "5710922"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Zeigt Werbung mithilfe der Microsoft Advertising-SDK in Ihrer App an

Erhöhen Sie Ihre Umsatzchancen, indem Sie mithilfe des Microsoft Advertising-SDKs Anzeigen in Ihre universelle Windows-Plattform-App für Windows 10 einfügen. Unsere anzeigen-monetarisierungsplattform bietet eine Vielzahl von Ad-Formate, die sich nahtlos in Ihre apps und unterstützt die anzeigenvermittlung mit unterschiedlichen beliebten Anzeigennetzwerken integriert werden kann. Unsere Plattform ist kompatibel mit der OpenRTB, große 2.x, MRAID 2 und 3 VPAID Standards und ist kompatibel mit MOAT und IAS. 

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
    <a href="http://aka.ms/ads-sdk-uwp">Installieren des Microsoft Advertising-SDK</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Entwicklerhandbuch</b><br/><br/>
    <a href="banner-ads.md">Banneranzeigen</a>
    <br/>
    <a href="interstitial-ads.md">Interstitialwerbung</a>
    <br/>
    <a href="native-ads.md">Native Anzeigen</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Weitere Ressourcen</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Einrichten von Anzeigenblöcken in der App</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Bewährte Verfahren</a>
    <br/>
    <a href="https://msdn.microsoft.com/en-us/library/windows/apps/mt691884.aspx">API-Referenz</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Schritt 1: Installieren des Microsoft Advertising-SDK

Installieren Sie zunächst die [Microsoft Advertising-SDK](http://aka.ms/ads-sdk-uwp) auf dem Entwicklungscomputer, den Sie verwenden, um Ihre App zu entwickeln. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

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

Nachdem Sie Ihre App getestet haben und Sie sie an den Store übermitteln können, erstellen Sie eine Anzeigeneinheit auf der Seite [In-App-Anzeigen](../publish/in-app-ads.md) Seite im Windows Dev Center-Dashboard. Aktualisieren Sie anschließend Ihren App-Code, um diese Anzeigeneinheit zu verwenden, damit Ihre App Live-Anzeigen empfängt. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

Standardmäßig zeigt Ihre App Werbung der Microsoft Netzwerke für kostenpflichtige Werbeanzeigen an. Zur Maximierung Ihres Anzeigenumsatzes können Sie für Ihre Anzeigeneinheit die [Anzeigenvermittlung](ad-mediation-service.md) aktivieren, um kostenpflichtige Anzeigen von weiteren Anzeigennetzwerken anzuzeigen (z. B. Taboola und Smaato). Sie können Ihrer App-Werbung auch steigern, indem Sie Anzeigen aus Microsoft App-Werbekampagnen darstellen.

Zum Starten der Anzeigenvermittlung in Ihrer UWP-App [Konfigurieren Sie die Anzeigenvermittlungseinstellungen](../publish/in-app-ads.md#mediation-settings) für Ihre Anzeigeneinheit. Standardmäßig werden die Einstellungen für die Anzeigenvermittlung automatisch mithilfe von Machine Learning-Algorithmen konfiguriert, die ihnen bei der Optimierung der Anzeigenumsätze in den verschiedenen Märkten helfen, die Ihre App unterstützt. Sie haben jedoch auch die Möglichkeit, die gewünschten Netzwerke manuell auszuwählen. In beiden Fällen werden die Einstellungen für die Anzeigenvermittlung vollständig auf unseren Servern konfiguriert; Sie müssen keine Codes in Ihrer App ändern.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Schritt 4: Übermitteln der App und Überprüfen der Leistung

Nach Abschluss der Entwicklung Ihrer App mit Werbung können Sie Ihre [aktualisierte App über das Windows Dev Center-Dashboard](https://docs.microsoft.com/windows/uwp/publish/app-submissions) senden, damit sie im Store verfügbar ist. Apps, die Anzeigen darstellen, müssen zusätzlich die Anforderungen erfüllen, die in [Abschnitt 10.10 der Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) [Anlage E der Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) angegeben sind.

Nach der Veröffentlichung Ihrer App und im Store können Sie Ihre [Werbeleistungsberichte](../publish/advertising-performance-report.md) im Dashboard überprüfen und Änderungen an den für die Anzeigenvermittlung zur Optimierung der Leistung Ihrer Anzeigen vornehmen. Der Umsatz befindet sich in der [Auszahlungszusammenfassung](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Zusätzliche Hilfe

Weitere Hilfe zum Microsoft Advertising-SDK finden Sie in den folgenden Ressourcen.

|  Aufgabe    | Ressource |               
|----------|-------|
| Melden eines Fehlers und Supportunterstützung für Werbung     | Besuchen Sie die [Supportseite](https://developer.microsoft.com/en-us/windows/support), und wählen Sie **Werbung in Apps**.        |
| Community-Support erhalten     | Besuchen Sie das [Forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Laden Sie Beispielprojekte herunter, die veranschaulichen, wie Sie Banner und Interstitialwerbung zu Apps hinzufügen.     | Weitere Informationen finden Sie unter [Anzeigenbeispiele bei GitHub](http://aka.ms/githubads).       |
| Weitere Informationen zu den neuesten Umsatzchancen für Windows-Apps     | Besuchen Sie Seite [Monetarisierung Ihrer Apps](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 und Windows Phone 8.x-Apps

Für Apps für Windows8.1 und Windows Phone8.x bieten wir das [Microsoft Advertising-SDK for Windows and Windows Phone 8.x](http://aka.ms/store-8-sdk). Weitere Informationen zur Verwendung des SDKs für Anzeigen in einer Windows8.1- oder Windows Phone8.x-App finden Sie in [diesem Artikel](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Verwandte Themen

* [MicrosoftAdvertising-SDK](http://aka.ms/ads-sdk-uwp)
* [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md)
* [Programm für Herausgeber von Windows Premium-Anzeigen](windows-premium-ads-publishers-program.md)
