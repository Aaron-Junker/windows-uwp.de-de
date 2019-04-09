---
Description: Wenn Ihre app anzeigen, die mit dem Microsoft Advertising SDK angezeigt wird, verwenden Sie der In-app-Werbung-Seite des Partner Center, um die Verwendung der Ads zu verwalten.
title: In-App-Anzeigen
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4b8fc913818d5abe3317582cc18d5cb676a7a2fe
ms.sourcegitcommit: 9f23bc9265c13f7ccedbc826261ceed2aff5ab83
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417056"
---
# <a name="in-app-ads"></a>In-App-Anzeigen

Verwenden der **Monetize** &gt; **In-app-Werbung** auf der Seite [Partner Center](https://partner.microsoft.com/dashboard) zum Erstellen und Verwalten von Ad-Einheiten für:

* Apps für die universelle Windows-Plattform (UWP), die [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp) verwenden.
* Zuvor veröffentlichten Windows 8.x und Windows Phone 8.x-apps, mit denen die [Microsoft Advertising SDK für Windows und Windows Phone 8.x](https://aka.ms/store-8-sdk).

> [!IMPORTANT]
> Ab 31. Oktober 2018 keine Produkte neu erstellten Pakete mit dem Ziel Windows 8.x/Windows enthalten Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Weitere Informationen dazu, wie Sie diese SDKs in Ihren Apps zu Werbezwecken integrieren, finden Sie unter [Anzeigen von Werbung in Ihrer App mit dem Microsoft Advertising-SDK](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Erstellen von Anzeigeneinheiten

So erstellen Sie eine Anzeigeeinheit für eine [Banneranzeige](../monetize/banner-ads.md), [Interstitialwerbung](../monetize/interstitial-ads.md) oder [native Anzeige](../monetize/native-ads.md) in Ihrer App:

1.  Wechseln Sie zu der **Monetize** &gt; **In-app-Werbung** Seite im Partner Center, und klicken Sie auf **erstellen Werbeeinheit**.
2.  Wählen Sie in der Dropdownliste **App-Name** die App aus, in der die Anzeigeneinheit verwendet werden soll.
3.  Geben Sie im Feld **Name der Anzeigeneinheit** einen Namen für die Anzeigeneinheit ein. Dies kann eine beliebige beschreibende Zeichenfolge sein, die Sie verwenden, um die Anzeigeneinheit zu Berichterstellungszwecken zu identifizieren.
4.  Wählen Sie in der Dropdownliste **Art der Anzeigeneinheit** den Anzeigentyp aus.

    * Wenn Sie ein Werbebanner in Ihrer app angezeigt werden, wählen Sie **Banner**.
    * Wenn Sie eine interstitial videowerbung oder interstitial Werbebanner in Ihrer app angezeigt werden, wählen Sie **Video interstitial** oder **Banner interstitial** (Achten Sie darauf, wählen Sie die entsprechende Option für den Typ des Interstitial Ad, die Sie anzeigen möchten).
    * Wenn Sie eine native Ad in Ihrer app angezeigt werden, wählen Sie **Native**.

5. Wählen Sie in der Dropdownliste **Gerätefamilie** die Gerätefamilie aus, auf die Ihre App ausgerichtet ist, in der die Anzeigeneinheit verwendet werden. Die verfügbaren Optionen lauten: **UWP (Windows 10)**, **PC/Tablet (Windows 8.1)**, oder **Mobile (Windows Phone 8.x)**.

6. Konfigurieren Sie die folgenden zusätzlichen Einstellungen wie gewünscht:

    * Wenn Sie eine **UWP (Windows 10)**-Gerätefamilie für die Anzeigeneinheit auswählen, können Sie optional die [Vermittlungseinstellungen](#mediation) für die Anzeigeneinheit konfigurieren.
    * Wenn Sie die **PC/Tablet (Windows 8.1)** oder **Mobile (Windows Phone 8.x)**-Gerätefamilie für eine Banneranzeigeneinheit auswählen, können Sie optional **Community-Anzeigen in Ihrer App anzeigen** auswählen, um sich bei [Community-Anzeigen](about-community-ads.md) anzumelden.

7.  Wenn Sie die COPPA-Compliance für die ausgewählte App noch nicht eingerichtet haben, wählen Sie eine Option im Abschnitt [COPPA-Compliance](#coppa) aus.
8.  Klicken Sie auf **Anzeigeneinheit erstellen**.

Nach der Erstellung der neuen Ad-Einheit in der Tabelle der verfügbaren Ad Einheiten in werden anscheinend die **Monetize** &gt; **In-app-Werbung** Seite.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Überprüfen und Bearbeiten von Anzeigeneinheiten

Nach der Erstellung von Ad-Einheiten für eine oder mehrere apps in Ihrem Konto diesen Ad-Einheiten werden angezeigt, in einer Tabelle am unteren Rand der **Monetize** &gt; **In-app-Werbung** Seite. Diese Tabelle zeigt die **Anwendungs-ID** und **Anzeigeneinheits-ID** für jede Anzeigeneinheit zusammen mit anderen Informationen an. Zum Einblenden von Anzeigen in Ihrer App müssen Sie diese Werte in Ihrem Code verwenden. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](../monetize/set-up-ad-units-in-your-app.md).

* Wenn Ihre App [Banneranzeigen](../monetize/banner-ads.md) anzeigt, weisen Sie diese Werte den Eigenschaften [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) und [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) Ihres [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)-Objekts hinzu.
* Wenn Ihre App [Interstitialwerbung](../monetize/interstitial-ads.md) anzeigt, übergeben Sie diese Werte an die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Method Ihres [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Objekts.
* Wenn Ihre app zeigt [native Ads](../monetize/native-ads.md), übergeben Sie diese Werte, die **NativeAdsManagerV2** Konstruktor.
  > [!IMPORTANT]
  > Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

  > [!NOTE]
  > Können mehrere Steuerelemente für Banner-, Interstitialwerbungen und native Anzeigen in einer einzelnen App verwenden. In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

Um die [Vermittlungseinstellungen](#mediation) für eine UWP-Anzeigeneinheit oder die [COPPA-Compliance](#coppa) für die App, in denen die Anzeigeneinheit verwendet wird, zu bearbeiten, klicken Sie auf den Namen der Anzeigeneinheit.

> [!NOTE]
> Verfügt eine Werbeeinheit keine Aktivität für die letzten sechs Monate, werden wir bezeichnen sie als **inaktiv**, und schließlich vom Partner Center zu entfernen. Sie können Filter verwenden, um nur **aktive** oder **inaktive** Anzeigeneinheiten anzuzeigen. Wenn Sie Anzeigeneinheiten sehen, für die Sie der Meinung sind, sie seien ungenau als **inaktiv** markiert, [wenden Sie sich an den Support](https://aka.ms/storesupport).

<span id="mediation" />

## <a name="mediation-settings"></a>Einstellungen der Anzeigenvermittlung

Wenn Sie [erstellen Sie eine neue UWP-Ad-Einheit](#create-ad-unit) oder [bearbeiten eine vorhandene UWP Werbeeinheit](#available-ad-units), verwenden Sie die Optionen in diesem Abschnitt konfigurieren [Ad Vermittlung](../monetize/ad-mediation-service.md) für die Ad-Einheit. Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken und Anzeigen ohne Umsatzgenerierung zu Werbekampagnen für Microsoft-Apps. Wir kümmern uns um die Vermittlung von Banneranzeigenanforderungen von den gewählten Anzeigennetzwerken. Wenn Sie eine UWP-Anzeigeneinheit haben, die bereits mit einer Banner-, Interstitial oder nativen Anzeige in Ihrer App verbunden ist, erfordert das Aktivieren der Anzeigenvermittlung keine Codeänderungen in Ihrer App.

> [!NOTE]
> Wenn Sie die Anzeigenvermittlung für eine UWP-Anzeigeneinheit aktivieren, müssen Sie keine Anzeigeneinheit von Drittanbieter-Anzeigennetzwerken erhalten. Unser Anzeigenvermittlungsdienst erstellt automatisch alle erforderlichen Drittanbieter-Anzeigeneinheiten.

So konfigurieren Sie die Anzeigenvermittlung für eine UWP-Anzeigeneinheit in Ihrer App:

1. [Eine Anzeigeneinheit erstellen](#create-ad-unit) oder [Eine vorhandene Anzeigeneinheit auswählen](#available-ad-units).
2. Auf der **In-app-Werbung** wechseln Sie zur Seite der **Vermittlung Einstellungen** Abschnitt und die Konfiguration der Einstellungen.

    * In der Standardeinstellung die **Teilen Sie Microsoft optimieren die Einstellungen meines** das Kontrollkästchen aktiviert ist. Es wird empfohlen, diese Option zu verwenden. Diese Option verwendet Machine Learning-Algorithmen, um automatisch die Anzeigenvermittlungseinstellungen für Ihre App auszuwählen, um Ihnen beim Optimieren der Anzeigenumsätze in den verschiedenen Märkten zu helfen, die Ihre App unterstützt. Wenn Sie diese Option verwenden, können Sie auch die Ad-Netzwerke auswählen, die Sie in der Konfiguration verwenden möchten. Deaktivieren Sie die Ad-Netzwerke, die nicht Teil der Konfiguration werden möchten, und unsere Algorithmus wird sichergestellt, dass Ihre app nur Anzeige von den ausgewählten Ad-Netzwerken erhält.
    * Wenn Sie Ihre eigenen Ad Einstellungen für die Vermittlung auswählen möchten, wählen Sie **Standardeinstellungen ändern**.

    > [!NOTE]
    > Die restlichen Schritte in diesem Abschnitt gelten nur bei Auswahl **Standardeinstellungen ändern**.

3. Wählen Sie in der Dropdownliste **Ziel** die Option **Basisplan**, um die Standardkonfiguration für Ihre Anzeigenvermittlungseinstellungen zu konfigurieren. Diese Standardkonfiguration wird auf alle Märkte angewendet, mit Ausnahme von Märkten, für die Sie marktspezifische Konfigurationen definieren.
4. Geben Sie dann das Verhältnis der Anzeigen an, die Sie auf dem Steuerelement von kostenpflichtigen Netzwerken (die Sie für Aufrufe bezahlen) und anderen Anzeigennetzwerken (die Sie nicht für Aufrufe bezahlen) anzeigen möchten. Geben Sie hierzu einen Wert zwischen 0 und 100 im Feld **Gewichtung** für **Paid ad networks** und **Weitere Anzeigennetzwerke** ein.  
5. Aktivieren Sie im Abschnitt **Paid ad networks** das Kontrollkästchen in der Spalte **Aktiv** für jedes [kostenpflichtige Netzwerk](#paid-networks), das Sie verwenden möchten, und sortieren Sie die Netzwerke dann mithilfe der Pfeile in der Spalte **Rang** nach Rang. (Dies gibt an,wie oft jedes Netzwerk von Ihrem Steuerelement verwendet werden soll.)
6. Wenn Sie eine **Banner** oder **Interstitialwerbung**-Anzeigeneinheit ausgewählt haben, sehen Sie außerdem einen Abschnitt namens **Weitere Anzeigennetzwerke** . Mit den Netzwerken in diesem Abschnitt erzielen Sie keine Einnahmen für Anzeigenaufrufe. Stattdessen zeigen diese Netzwerke Werbung aus Quellen wie Werbekampagnen für Apps an.

    Aktivieren Sie im Abschnitt **Weitere Anzeigennetzwerke** das Kontrollkästchen in der Spalte **Aktiv** für jedes [weitere Netzwerk](#other-networks), das Sie verwenden möchten, und sortieren Sie die Netzwerke dann mithilfe der Pfeile in der Spalte **Rang** nach Rang. (Dies gibt an,wie oft jedes Netzwerk von Ihrem Steuerelement verwendet werden soll.) Die folgenden weiteren Netzwerke werden derzeit unterstützt:

7. Für jeden Markt, in dem Sie die Standardvermittlungskonfiguration außer Kraft setzen möchten, wählen Sie den Markt in der Dropdownliste **Ziel**, und aktualisieren Sie die Anzeigennetzwerkauswahl und den Rang.
8. Klicken Sie auf **Anzeigeneinheit erstellen** (wenn Sie eine neue Anzeigeneinheit erstellen) oder **Speichern** (wenn Sie eine vorhandene Anzeigeneinheit bearbeiten).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Unterstützte Anzeigennetzwerke

Die folgende Tabelle enthält die kostenpflichtigen Netzwerke, die wir derzeit für jeden Anzeigentyp unterstützen. Beachten Sie, dass einige dieser Netzwerke [nicht in allen Märkten verfügbar](#network-markets) sind.

|  Anzeigennetzwerk  |  Beschreibung  |  Unterstützte Anzeigentypen  |
|--------------|---------------|---------------------|
| Oath- und AppNexus |  Dies ist eine von Microsoft verwaltet Ad-Netzwerk, das werbeeinblendungen über unser Partner Netzwerke Oath- und AppNexus erfüllt.<p/>**Hinweis**: Oath- und AppNexus wird immer zuerst Rangfolge der **bezahlt anzeigennetzwerke** Liste Werbeeinheiten Banner, und es kann nicht geändert werden, um eine niedrigere Rangfolge für diese Typen von anzeigen. | Banneranzeigen, Video-Interstitialanzeigen |
| AppNexus (direkt) | Wählen Sie diese Option, um die Werbung von dienen [AppNexus](https://www.appnexus.com). | Video-Interstitialanzeigen, native Anzeigen  |
| Microsoft-Anzeigen für die App-Installation | Wählen Sie diese Option, um Anzeigen für die App-Installation oder das Wiedereinschalten von Anzeigen in Apps anzuzeigen, die von anderen Entwicklern im Windows-Ökosystem erstellt wurden, die [Werbeanzeigenkampagnen für ihre Apps erstellen](create-an-ad-campaign-for-your-app.md).  |  Banneranzeigen, Banner-Interstitialwerbung, native Anzeigen  |
| MSN-Content-Empfehlungen |  Wählen Sie diese Option, um das Anzeigen von Inhalt MSN-Empfehlungen dienen. |  Banneranzeigen, Banner-Interstitialwerbung  |
| Outbrain |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Outbrain](https://www.outbrain.com/). |  Banneranzeigen, Banner-Interstitialwerbung  |
| Revcontent |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Revcontent](https://www.revcontent.com/). |  Banner, nativ  |
| Smaato |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Smaato](https://www.smaato.com/). |  Banner  |
| Smartclip |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Smartclip](http://www.smartclip.com/). |  Video-Interstitialanzeigen  |
| SpotX |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [SpotX](https://www.spotx.tv/). |  Video-Interstitialanzeigen  |
| Taboola |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Taboola](https://www.taboola.com/). |  Banner  |
| Undertone | Wählen Sie diese Option, um die Werbung von dienen [Undertone](https://www.undertone.com/). | Banner interstitial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Weitere Anzeigennetzwerke

Die folgende Tabelle enthält die anderen Netzwerke, die wir derzeit für jeden Anzeigentyp unterstützen.

|  Anzeigennetzwerk  |  Beschreibung  |  Unterstützte Anzeigentypen  |
|--------------|---------------|---------------------|
| Microsoft Community-Anzeigen |  Wenn Sie [eine Anzeigenkampagne für eine Ihrer Apps erstellen](create-an-ad-campaign-for-your-app.md) und diese Kampagne als [Kampagne für Community-Anzeigen](about-community-ads.md) konfigurieren, wählen Sie diese Optionen, um Anzeigen aus dieser Kampagne anzuzeigen. | Banneranzeigen, Banner-Interstitialwerbung |
| Microsoft-Eigenwerbung | Wenn Sie [eine Anzeigenkampagne für eine Ihrer Apps erstellen](create-an-ad-campaign-for-your-app.md) und diese Kampagne als [Kampagne für Eigenwerbung](about-house-ads.md) konfigurieren, wählen Sie diese Optionen, um Anzeigen aus dieser Kampagne anzuzeigen. | Banneranzeigen, Banner-Interstitialwerbung  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Unterstützte Märkte für Anzeigennetzwerke

Die verfügbaren Anzeigennetzwerke schalten Anzeigen in allen [unterstützten Märkten](define-market-selection.md#microsoft-store-consumer-markets) mit den folgenden Ausnahmen.

|  Anzeigennetzwerk  |  Unterstützte Märkte  |
|--------------|---------------------|
| Revcontent | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Vereinigtes Königreich, USA  |
| Smaato | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Vereinigtes Königreich, USA |
| Smartclip | Österreich, Belgien, Dänemark, Finnland, Deutschland, Italien, Niederlande, Norwegen, Schweden, Schweiz  |
| Undertone | USA |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA-Compliance

Wenn Sie [erstellen Sie eine Werbeeinheit](#create-ad-unit) oder [wählen Sie eine vorhandene Ad-Einheit](#available-ad-units), **COPPA Compliance** Abschnitt wird am unteren Rand der Seite angezeigt, die ausgewählte app für die Ad-Einheit verfügt über mindestens eine Übergabe, die erreicht die [in den Store](../publish/the-app-certification-process.md#in-the-store) Schritt Zertifizierungsprozess der app.

Im Rahmen des Children's Online Privacy Protection Act ("COPPA"), wählen Sie **Diese Anwendung richtet sich an Kinder unter 13 Jahren** in diesem Abschnitt aus, wenn Ihre App an Kinder unter 13 Jahren gerichtet ist. Wenn Sie diese Option auswählen, wird Microsoft Maßnahmen ergreifen, um die verhaltensorientierten Werbedienste bei der Übermittlung von Werbung in Ihre App zu deaktivieren.

Die **COPPA-Compliance**-Einstellung, die Sie auswählen, wird automatisch auf allen Anzeigeeinheiten für die ausgewählte App angewendet.

> [!IMPORTANT]
> Wenn Ihre App an Kinder unter 13 Jahren gerichtet ist, ergeben sich aus COPPA bestimmte Verpflichtungen für Sie. Weitere Informationen über Ihre Verpflichtungen finden Sie [auf dieser Seite](https://go.microsoft.com/fwlink/p/?linkid=536558).
