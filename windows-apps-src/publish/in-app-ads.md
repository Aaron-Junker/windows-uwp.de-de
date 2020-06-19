---
Description: Wenn Ihre App anzeigen mithilfe des Microsoft Advertising SDK anzeigt, verwenden Sie die Seite in-App-ADS von Partner Center, um ihre Verwendung von anzeigen zu verwalten.
title: In-App-Anzeigen
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 11d992baf42f320856134f0e8fba845c5ad61393
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945928"
---
# <a name="in-app-ads"></a>In-App-Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Verwenden Sie **die** &gt; Seite " **in-App-Werbeeinblendungen** " im [Partner Center](https://partner.microsoft.com/dashboard) zum Erstellen und Verwalten von Ad-Einheiten für:

* Universelle Windows-Plattform-Apps (UWP), die das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)verwenden.
* Zuvor veröffentlichte Windows 8. x-und Windows Phone 8. x-apps, die das [Microsoft Advertising SDK für Windows und Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)verwenden.

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Weitere Informationen zur Integration dieser SDKs in Ihre apps zum Anzeigen von Werbeeinblendungen finden Sie unter Anzeigen von [anzeigen in Ihrer APP mit dem Microsoft Advertising SDK](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Erstellen von Ad-Einheiten

So erstellen Sie eine Ad-Einheit für eine [Banner Anzeige](../monetize/banner-ads.md), eine [Interaktion mit Interstitial](../monetize/interstitial-ads.md)oder eine [native Werbung](../monetize/native-ads.md) in Ihrer APP:

1.  **Wechseln** &gt; Sie in Partner Center zur Seite **in-App-Werbeeinblendungen** , und klicken Sie auf **Ad-Einheit erstellen**.
2.  Wählen Sie in der Dropdown-Dropdown-Dropdown-Dropdown- **App** die APP aus, in der die Ad-Einheit verwendet
3.  Geben Sie im Feld **Ad Unit Name (Ad Unit Name** ) einen Namen für die Ad-Einheit ein. Dabei kann es sich um eine beschreibende Zeichenfolge handeln, die Sie verwenden möchten, um die Ad-Einheit für Berichts Zwecke zu identifizieren.
4.  Wählen Sie in der Dropdown-Dropdown **Gruppe Ad Unit Type** den Typ AD aus.

    * Wenn Sie in Ihrer APP eine Banner Anzeige anzeigen, wählen Sie **Banner**aus.
    * Wenn Sie in Ihrer APP eine Video-oder Interstitial-Banner Anzeige mit Interstitial anzeigen, wählen Sie die Option **Video Interstitial** oder **Banner Interstitial** aus. (achten Sie darauf, dass Sie die entsprechende Option für den Typ der zu Anzeige enden Schnittstelle auswählen.)
    * Wenn Sie in Ihrer APP eine native Werbung anzeigen, wählen Sie **native**aus.

5. Wählen Sie in der Dropdown-Dropdown-Dropdown Gruppe **Gerätefamilie** die Gerätefamilie aus, auf die sich die APP spezialisiert hat. Folgende Optionen stehen zur Verfügung: **UWP (Windows 10)**, **PC/Tablet (Windows 8.1)** oder **Mobil (Windows Phone 8. x)**.

6. Konfigurieren Sie die folgenden zusätzlichen Einstellungen wie gewünscht:

    * Wenn Sie die UWP-Gerätefamilie **(Windows 10)** für die Ad-Einheit auswählen, können Sie optional die [Vermittlungs Einstellungen](#mediation) für die Ad-Einheit konfigurieren.
    * Wenn Sie die Gerätefamilie **PC/Tablet (Windows 8.1)** oder **Mobile (Windows Phone 8. x)** für eine Banner-Ad-Einheit auswählen, können Sie optional die Option **Community-anzeigen in ihrer App anzeigen** auswählen, um communitywerbung zu abonnieren. [community ads](about-community-ads.md)

7.  Wenn Sie die Coppa-Konformität für die ausgewählte APP noch nicht festgelegt haben, wählen Sie im Abschnitt [Coppa-Konformität](#coppa) eine Option aus.
8.  Klicken Sie auf **Anzeigeneinheit erstellen**.

Nachdem Sie die neue Ad-Einheit erstellt haben, wird Sie in der Tabelle mit den verfügbaren **Monetize** Ad-Einheiten auf der Seite " &gt; **in-App-Werbung** monetarisieren" angezeigt.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Ad-Einheiten überprüfen und bearbeiten

Nachdem Sie Ad-Einheiten für eine oder mehrere apps in Ihrem Konto erstellt haben, werden diese Ad-Einheiten in einer Tabelle am unteren **Monetize** Rand der Seite " &gt; **in-App-Werbung** monetarisieren" angezeigt. In dieser Tabelle werden die **Anwendungs-ID** und die Ad-Einheiten- **ID** für jede Ad-Einheit zusammen mit anderen Informationen angezeigt. Zum Anzeigen von Werbeeinblendungen in der APP müssen Sie diese Werte in Ihrem Code verwenden. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](../monetize/set-up-ad-units-in-your-app.md).

* Wenn Ihre APP [Banner Anzeigen](../monetize/banner-ads.md)anzeigt, weisen Sie diese Werte den [Eigenschaften ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) und [adunitid](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) Ihres [adcontrol](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) -Objekts zu.
* Wenn Ihre APP [Interstitial ADS](../monetize/interstitial-ads.md)anzeigt, übergeben Sie diese Werte an die [requestad](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) -Methode Ihres [interstitialad](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) -Objekts.
* Wenn Ihre APP [native Werbung](../monetize/native-ads.md)anzeigt, übergeben Sie diese Werte an den **NativeAdsManagerV2** -Konstruktor.
  > [!IMPORTANT]
  > Sie können jede Ad-Einheit nur in einer App verwenden. Wenn Sie eine Ad-Einheit in mehr als einer App verwenden, werden ADS nicht für diese Ad-Einheit bereitgestellt.

  > [!NOTE]
  > Sie können mehrere Banner, Interstitial und Native AD-Steuerelemente in einer einzelnen App verwenden. In diesem Szenario wird empfohlen, jedem Steuerelement eine andere Ad-Einheit zuzuweisen. Wenn Sie verschiedene Ad-Einheiten für jedes Steuerelement verwenden, können Sie [die Vermittlungs Einstellungen separat konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md) für jedes Steuerelement erhalten. Dadurch können unsere Dienste auch die Werbeeinblendungen, die wir für Ihre APP bereitstellen, besser optimieren.

Um die [Vermittlungs Einstellungen](#mediation) für eine UWP-Ad-Einheit oder die [Coppa-Konformität](#coppa) für die APP zu bearbeiten, in der die Ad-Einheit verwendet wird, klicken Sie auf den Namen der Ad-Einheit.

> [!NOTE]
> Wenn eine Ad-Einheit in den letzten sechs Monaten keine Aktivität aufweist, bezeichnen wir diese als **inaktiv**und entfernen Sie schließlich aus Partner Center. Sie können Filter verwenden, um nur **aktive** oder **inaktive** Ad-Einheiten anzuzeigen. Wenn Ad-Einheiten angezeigt werden, die Sie als **inaktiv**markiert haben, wenden Sie sich [an den Support](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Vermittlungs Einstellungen

Wenn Sie [eine neue UWP-Ad-Einheit erstellen](#create-ad-unit) oder [eine vorhandene UWP-Ad-Einheit bearbeiten](#available-ad-units), verwenden Sie die Optionen in diesem Abschnitt, um die [AD-Vermittlung](../monetize/ad-mediation-service.md) für die Ad-Einheit zu konfigurieren. Die AD-Vermittlung ermöglicht es Ihnen, ihre Werbe-und App-herauf Stufungs Funktionen zu maximieren, indem Sie Werbeeinblendungen aus mehreren Ad-Netzwerken anzeigen, einschließlich Werbeeinblendungen aus anderen kostenpflichtigen Ad-Netzwerken und Wir kümmern uns um die Vermittlung von Banner Werbeanforderungen aus den von Ihnen gewählten Ad-Netzwerken. Wenn Sie über eine UWP-Ad-Einheit verfügen, die bereits mit einem Banner, Interstitial oder systemeigenen AD in Ihrer APP verknüpft ist, erfordert die Aktivierung der AD-Vermittlung keine Codeänderungen in der app.

> [!NOTE]
> Wenn Sie die AD-Vermittlung für eine UWP-Ad-Einheit aktivieren, müssen Sie keine Ad-Einheit aus Ad-Netzwerken von Drittanbietern abrufen. Der AD-Vermittlungsdienst erstellt automatisch alle notwendigen Ad-Einheiten von Drittanbietern.

So konfigurieren Sie die AD-Vermittlungs Einstellungen für eine UWP-Ad-Einheit in Ihrer APP:

1. [Erstellen Sie eine Ad-Einheit](#create-ad-unit) oder [Wählen Sie eine vorhandene Ad-Einheit](#available-ad-units)aus.
2. Wechseln Sie auf der Seite " **in-App-Werbung** " zum Abschnitt " **Vermittlungs Einstellungen** ", und konfigurieren Sie die Einstellungen.

    * Standardmäßig ist das Kontrollkästchen **Microsoft meine Einstellungen optimieren** aktiviert. Es wird empfohlen, diese Option zu verwenden. Diese Option verwendet Machine Learning-Algorithmen, um automatisch die AD-Vermittlungs Einstellungen für Ihre APP auszuwählen, damit Sie Ihre Werbeeinnahmen über die von Ihrer APP unterstützten Märkte hinweg maximieren können. Wenn Sie diese Option verwenden, können Sie auch die Ad-Netzwerke auswählen, die Sie in der Konfiguration verwenden möchten. Deaktivieren Sie die Ad-Netzwerke, die nicht Teil der Konfiguration sein sollen, und unser Algorithmus stellt sicher, dass Ihre APP nur anzeigen aus den ausgewählten Ad-Netzwerken empfängt.
    * Wenn Sie Ihre eigenen Ad-Vermittlungs Einstellungen auswählen möchten, wählen Sie **Standardeinstellungen ändern**aus.

    > [!NOTE]
    > Die restlichen Schritte in diesem Abschnitt gelten nur, wenn Sie die Option **Standardeinstellungen ändern**auswählen.

3. Wählen Sie in der Dropdown Liste **Ziel** die Option **Baseline** aus, um die Standardkonfiguration für Ihre AD-Vermittlungs Einstellungen zu konfigurieren. Diese Standardkonfiguration wird auf alle Märkte angewendet, mit Ausnahme von Märkten, in denen Sie marktspezifische Konfigurationen definieren.
4. Geben Sie als nächstes das Verhältnis von Werbeeinblendungen, die Sie in Ihrem Steuerelement anzeigen möchten, aus kostenpflichtigen Netzwerken (für die Sie Umsätze bei den Eindrücken bezahlen) und anderen Ad-Netzwerken an Geben Sie hierzu einen Wert zwischen 0 und 100 in den **Gewichtungs** Feldern für **kostenpflichtige Ad-Netzwerke** und **andere Ad-Netzwerke**ein.  
5. Aktivieren Sie im Abschnitt **kostenpflichtige Ad-Netzwerke** das Kontrollkästchen in der Spalte **aktiv** für jedes [kostenpflichtige Netzwerk](#paid-networks) , das Sie verwenden möchten, und verwenden Sie dann die Pfeile in der Spalte **Rang** , um die Netzwerke nach Rang zu sortieren (Dies gibt an, wie oft jedes Netzwerk von Ihrem Steuerelement verwendet werden soll).
6. Wenn Sie ein **Banner** oder eine **Banner-Interstitial** -Ad-Einheit ausgewählt haben, wird auch ein Abschnitt mit dem Namen **andere Ad-Netzwerke**angezeigt. Die Netzwerke in diesem Abschnitt verdienen Ihnen keinen Umsatz für werbeeindrücke. Stattdessen werden in diesen Netzwerken anzeigen von Quellen wie App-Promotionkampagnen angezeigt.

    Aktivieren Sie im Abschnitt **andere Ad-Netzwerke** das Kontrollkästchen in der **Spalte aktiv** für jedes [andere Netzwerk](#other-networks) , das Sie verwenden möchten, und ordnen Sie dann mithilfe der Pfeile in der Spalte **Rang** die Netzwerke nach Rang an (Dies gibt an, wie oft jedes Netzwerk von Ihrem Steuerelement verwendet werden soll). Die folgenden anderen Netzwerke werden derzeit unterstützt:

7. Wählen Sie für jeden Markt, für den Sie die standardmäßige Vermittlungs Konfiguration außer Kraft setzen möchten, den Markt in der Dropdown Liste **Ziel** aus, und aktualisieren Sie die Auswahl und Rangfolge des AD-Netzwerks.
8. Klicken Sie auf **Ad Unit erstellen** (wenn Sie eine neue Ad-Einheit erstellen) oder **Speichern** (wenn Sie eine vorhandene Ad-Einheit bearbeiten).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Unterstützte kostenpflichtige Ad-Netzwerke

In der folgenden Tabelle werden die kostenpflichtigen Netzwerke aufgelistet, die derzeit für jeden AD-Typ unterstützt werden. Beachten Sie, dass einige dieser Netzwerke [nicht in allen Märkten zur Verfügung](#network-markets)stehen.

|  Anzeigennetzwerk  |  Beschreibung  |  Unterstützte Ad-Typen  |
|--------------|---------------|---------------------|
| Oath und appnexus |  Dies ist ein von Microsoft verwaltetes Ad-Netzwerk, das Werbeeinblendungen über unsere Partnernetzwerke, Oath und appnexus, bietet.<p/>**Hinweis**: Oath und appnexus werden immer zuerst in der Liste der **kostenpflichtigen Ad-Netzwerke** für Banner-Werbe Einheiten sortiert und können für diese Art von Werbung nicht in eine niedrigere Rangfolge geändert werden. | Banner, Video Interstitial |
| Appnexus (Direct) | Wählen Sie diese Option aus, um Werbung von [appnexus](https://www.appnexus.com)zu verarbeiten. | Video Interstitial, Native  |
| Microsoft-App-Installations anzeigen | Wählen Sie diese Option aus, um App-Installations anzeigen oder App-reengagement-Werbeeinblendungen bereitzustellen [, die von](create-an-ad-campaign-for-your-app.md)anderen Entwicklern im Windows-Ökosystem erstellt wurden  |  Banner, Banner Interstitial, Native  |
| MSN Content-Empfehlungen |  Wählen Sie diese Option aus, um Anzeigen von MSN Content-Empfehlungen zu verarbeiten. |  Banner, Banner Interstitial  |
| Outbrain |  Wählen Sie diese Option aus, um Werbung von [outbrain](https://www.outbrain.com/)zu verarbeiten. |  Banner, Banner Interstitial  |
| Revcontent |  Wählen Sie diese Option aus, um Werbung von [revcontent](https://www.revcontent.com/)zu versorgen. |  Banner, System eigen  |
| Smaato |  Wählen Sie diese Option aus, um ADS von [smaato](https://www.smaato.com/)zu verarbeiten. |  Banner  |
| Smartclip |  Wählen Sie diese Option aus, um Werbung von [Smartclip](http://www.smartclip.com/)zu verarbeiten. |  Video Interstitial  |
| Spotx |  Wählen Sie diese [Option aus,](https://www.spotx.tv/)um Werbeeinblendungen bereitzustellen. |  Video Interstitial  |
| Taboola |  Wählen Sie diese Option aus, um ADS aus [Taboola](https://www.taboola.com/)bereitzustellen. |  Banner  |
| Vungle | Wählen Sie diese [Option aus,](https://vungle.com/) um Werbeeinblendungen zu verarbeiten. | Video Interstitial |
| Unterton | Wählen Sie diese [Option aus,](https://www.undertone.com/)um Werbeeinblendungen bereitzustellen. | Banner Interstitial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Weitere Anzeigennetzwerke

In der folgenden Tabelle sind die anderen Netzwerke aufgelistet, die zurzeit für jeden AD-Typ unterstützt werden.

|  Anzeigennetzwerk  |  Beschreibung  |  Unterstützte Ad-Typen  |
|--------------|---------------|---------------------|
| Microsoft Community ADS |  Wenn Sie [eine Werbe-Werbekampagne für eine Ihrer Apps erstellen](create-an-ad-campaign-for-your-app.md) und diese Kampagne als Community- [Werbekampagne](about-community-ads.md)konfigurieren, wählen Sie diese Optionen aus, um Werbeeinblendungen anzuzeigen. | Banner, Banner Interstitial |
| Microsoft-Haus Werbung | Wenn Sie [eine Werbe-Werbekampagne für eine Ihrer Apps erstellen](create-an-ad-campaign-for-your-app.md) und diese Kampagne als Haus- [Werbekampagne](about-house-ads.md)konfigurieren, wählen Sie diese Optionen aus, um Werbeeinblendungen anzuzeigen. | Banner, Banner Interstitial  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Unterstützte Märkte für Ad-Netzwerke

Die verfügbaren Ad-Netzwerke dienen in allen [unterstützten Märkten](define-market-selection.md#microsoft-store-consumer-markets)mit den folgenden Ausnahmen.

|  Anzeigennetzwerk  |  Unterstützte Märkte  |
|--------------|---------------------|
| Revcontent | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Vereinigtes Königreich, USA  |
| Smaato | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Vereinigtes Königreich, USA |
| Smartclip | Österreich, Belgien, Dänemark, Finnland, Deutschland, Italien, Niederlande, Norwegen, Schweden, Schweiz  |
| Unterton | USA |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA-Compliance

Wenn Sie [eine Ad-Einheit erstellen](#create-ad-unit) oder [eine vorhandene Ad-Einheit auswählen](#available-ad-units), wird der Abschnitt **Coppa-Konformität** unten auf der Seite angezeigt, wenn für die ausgewählte App für die Ad-Einheit mindestens eine Übermittlung vorhanden ist, die im Schritt [Store](../publish/the-app-certification-process.md#in-the-store) im App-Zertifizierungsprozess erreicht wurde.

Zum Zwecke des Online-Datenschutz Gesetzes ("COPPA") des Children-Themas müssen Sie **diese Anwendung in diesem Abschnitt unter dem Alter von 13 unter** geordneten Elementen auswählen, wenn Ihre APP an untergeordnete Elemente im Alter von 13 gerichtet ist. Wenn Sie diese Option auswählen, nimmt Microsoft beim Bereitstellen von Werbung in Ihrer APP Maßnahmen zum Deaktivieren der Dienstleistungen für die verhaltenswerbung vor.

Die von Ihnen gewählte **Coppa** -Kompatibilitäts Einstellung wird automatisch auf alle Ad-Einheiten für die ausgewählte App angewendet.

> [!IMPORTANT]
> Wenn Ihre App an Kinder unter 13 Jahren gerichtet ist, ergeben sich aus COPPA bestimmte Verpflichtungen für Sie. Weitere Informationen zu ihren Verpflichtungen finden Sie auf [dieser Seite](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
