---
Description: Wenn Ihre App anzeigen mithilfe des Microsoft Advertising SDK anzeigt, verwenden Sie die Seite in-App-ADS von Partner Center, um ihre Verwendung von anzeigen zu verwalten.
title: In-App-Anzeigen
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 96994566d19e03f1d85b751242331f04fef098ad
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852845"
---
# <a name="in-app-ads"></a>In-App-Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Verwenden Sie **die Seite** &gt; **in-App-Werbeeinblendungen** in [Partner Center](https://partner.microsoft.com/dashboard) , um Ad-Einheiten für Folgendes zu erstellen und zu verwalten:

* Apps für die universelle Windows-Plattform (UWP), die [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) verwenden.
* Zuvor veröffentlichte Windows 8. x-und Windows Phone 8. x-apps, die das [Microsoft Advertising SDK für Windows und Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)verwenden.

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 können neu erstellte Produkte keine Pakete enthalten, die auf Windows 8. x/Windows Phone 8. x oder früher abzielen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Weitere Informationen dazu, wie Sie diese SDKs in Ihren Apps zu Werbezwecken integrieren, finden Sie unter [Anzeigen von Werbung in Ihrer App mit dem Microsoft Advertising-SDK](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Erstellen von Anzeigeneinheiten

So erstellen Sie eine Anzeigeeinheit für eine [Banneranzeige](../monetize/banner-ads.md), [Interstitialwerbung](../monetize/interstitial-ads.md) oder [native Anzeige](../monetize/native-ads.md) in Ihrer App:

1.  Wechseln Sie in Partner Center zur Seite **Monetize** &gt; **in-App ADS** , und klicken Sie auf **Ad Unit erstellen**.
2.  Wählen Sie in der Dropdownliste **App-Name** die App aus, in der die Anzeigeneinheit verwendet werden soll.
3.  Geben Sie im Feld **Name der Anzeigeneinheit** einen Namen für die Anzeigeneinheit ein. Dies kann eine beliebige beschreibende Zeichenfolge sein, die Sie verwenden, um die Anzeigeneinheit zu Berichterstellungszwecken zu identifizieren.
4.  Wählen Sie in der Dropdownliste **Art der Anzeigeneinheit** den Anzeigentyp aus.

    * Wenn Sie in Ihrer APP eine Banner Anzeige anzeigen, wählen Sie **Banner**aus.
    * Wenn Sie in Ihrer APP eine Video-oder Interstitial-Banner Anzeige mit Interstitial anzeigen, wählen Sie die Option **Video Interstitial** oder **Banner Interstitial** aus. (achten Sie darauf, dass Sie die entsprechende Option für den Typ der zu Anzeige enden Schnittstelle auswählen.)
    * Wenn Sie in Ihrer APP eine native Werbung anzeigen, wählen Sie **native**aus.

5. Wählen Sie in der Dropdownliste **Gerätefamilie** die Gerätefamilie aus, auf die Ihre App ausgerichtet ist, in der die Anzeigeneinheit verwendet werden. Folgende Optionen sind verfügbar: **UWP (Windows 10)** , **PC/Tablet (Windows 8.1)** oder **Mobile (Windows Phone 8.x)** .

6. Konfigurieren Sie die folgenden zusätzlichen Einstellungen wie gewünscht:

    * Wenn Sie eine **UWP (Windows 10)** -Gerätefamilie für die Anzeigeneinheit auswählen, können Sie optional die [Vermittlungseinstellungen](#mediation) für die Anzeigeneinheit konfigurieren.
    * Wenn Sie die **PC/Tablet (Windows 8.1)** oder **Mobile (Windows Phone 8.x)** -Gerätefamilie für eine Banneranzeigeneinheit auswählen, können Sie optional **Community-Anzeigen in Ihrer App anzeigen** auswählen, um sich bei [Community-Anzeigen](about-community-ads.md) anzumelden.

7.  Wenn Sie die COPPA-Compliance für die ausgewählte App noch nicht eingerichtet haben, wählen Sie eine Option im Abschnitt [COPPA-Compliance](#coppa) aus.
8.  Klicken Sie auf **Anzeigeneinheit erstellen**.

Nachdem Sie die neue Ad-Einheit erstellt haben, wird Sie in der Tabelle Verfügbare Ad-Einheiten auf der Seite **Monetize** &gt; **in-App-Werbung** angezeigt.

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Überprüfen und Bearbeiten von Anzeigeneinheiten

Nachdem Sie Ad-Einheiten für eine oder mehrere apps in Ihrem Konto erstellt haben, werden diese Ad-Einheiten in einer Tabelle am unteren Rand der Seite **Monetize** &gt; **in-App-Werbung** angezeigt. Diese Tabelle zeigt die **Anwendungs-ID** und **Anzeigeneinheits-ID** für jede Anzeigeneinheit zusammen mit anderen Informationen an. Zum Einblenden von Anzeigen in Ihrer App müssen Sie diese Werte in Ihrem Code verwenden. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](../monetize/set-up-ad-units-in-your-app.md).

* Wenn Ihre App [Banneranzeigen](../monetize/banner-ads.md) anzeigt, weisen Sie diese Werte den Eigenschaften [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) und [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) Ihres [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)-Objekts hinzu.
* Wenn Ihre App [Interstitialwerbung](../monetize/interstitial-ads.md) anzeigt, übergeben Sie diese Werte an die [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Method Ihres [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad)-Objekts.
* Wenn Ihre APP [native Werbung](../monetize/native-ads.md)anzeigt, übergeben Sie diese Werte an den **NativeAdsManagerV2** -Konstruktor.
  > [!IMPORTANT]
  > Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

  > [!NOTE]
  > Können mehrere Steuerelemente für Banner-, Interstitialwerbungen und native Anzeigen in einer einzelnen App verwenden. In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

Um die [Vermittlungseinstellungen](#mediation) für eine UWP-Anzeigeneinheit oder die [COPPA-Compliance](#coppa) für die App, in denen die Anzeigeneinheit verwendet wird, zu bearbeiten, klicken Sie auf den Namen der Anzeigeneinheit.

> [!NOTE]
> Wenn eine Ad-Einheit in den letzten sechs Monaten keine Aktivität aufweist, bezeichnen wir diese als **inaktiv**und entfernen Sie schließlich aus Partner Center. Sie können Filter verwenden, um nur **aktive** oder **inaktive** Anzeigeneinheiten anzuzeigen. Wenn Sie Anzeigeneinheiten sehen, für die Sie der Meinung sind, sie seien ungenau als **inaktiv** markiert, [wenden Sie sich an den Support](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Einstellungen der Anzeigenvermittlung

Wenn Sie [eine neue UWP-Ad-Einheit erstellen](#create-ad-unit) oder [eine vorhandene UWP-Ad-Einheit bearbeiten](#available-ad-units), verwenden Sie die Optionen in diesem Abschnitt, um die [AD-Vermittlung](../monetize/ad-mediation-service.md) für die Ad-Einheit zu konfigurieren. Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken und Anzeigen ohne Umsatzgenerierung zu Werbekampagnen für Microsoft-Apps. Wir kümmern uns um die Vermittlung von Banneranzeigenanforderungen von den gewählten Anzeigennetzwerken. Wenn Sie eine UWP-Anzeigeneinheit haben, die bereits mit einer Banner-, Interstitial oder nativen Anzeige in Ihrer App verbunden ist, erfordert das Aktivieren der Anzeigenvermittlung keine Codeänderungen in Ihrer App.

> [!NOTE]
> Wenn Sie die Anzeigenvermittlung für eine UWP-Anzeigeneinheit aktivieren, müssen Sie keine Anzeigeneinheit von Drittanbieter-Anzeigennetzwerken erhalten. Unser Anzeigenvermittlungsdienst erstellt automatisch alle erforderlichen Drittanbieter-Anzeigeneinheiten.

So konfigurieren Sie die Anzeigenvermittlung für eine UWP-Anzeigeneinheit in Ihrer App:

1. [Eine Anzeigeneinheit erstellen](#create-ad-unit) oder [Eine vorhandene Anzeigeneinheit auswählen](#available-ad-units).
2. Wechseln Sie auf der Seite " **in-App-Werbung** " zum Abschnitt " **Vermittlungs Einstellungen** ", und konfigurieren Sie die Einstellungen.

    * Standardmäßig ist das Kontrollkästchen **Microsoft meine Einstellungen optimieren** aktiviert. Es wird empfohlen, diese Option zu verwenden. Diese Option verwendet Machine Learning-Algorithmen, um automatisch die Anzeigenvermittlungseinstellungen für Ihre App auszuwählen, um Ihnen beim Optimieren der Anzeigenumsätze in den verschiedenen Märkten zu helfen, die Ihre App unterstützt. Wenn Sie diese Option verwenden, können Sie auch die Ad-Netzwerke auswählen, die Sie in der Konfiguration verwenden möchten. Deaktivieren Sie die Ad-Netzwerke, die nicht Teil der Konfiguration sein sollen, und unser Algorithmus stellt sicher, dass Ihre APP nur anzeigen aus den ausgewählten Ad-Netzwerken empfängt.
    * Wenn Sie Ihre eigenen Ad-Vermittlungs Einstellungen auswählen möchten, wählen Sie **Standardeinstellungen ändern**aus.

    > [!NOTE]
    > Die restlichen Schritte in diesem Abschnitt gelten nur, wenn Sie die Option **Standardeinstellungen ändern**auswählen.

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
| Oath und appnexus |  Dies ist ein von Microsoft verwaltetes Ad-Netzwerk, das Werbeeinblendungen über unsere Partnernetzwerke, Oath und appnexus, bietet.<p/>**Hinweis**: Oath und appnexus werden immer zuerst in der Liste der **kostenpflichtigen Ad-Netzwerke** für Banner-Werbe Einheiten sortiert und können für diese Art von Werbung nicht in eine niedrigere Rangfolge geändert werden. | Banneranzeigen, Video-Interstitialanzeigen |
| AppNexus (direkt) | Wählen Sie diese Option aus, um Werbung von [appnexus](https://www.appnexus.com)zu verarbeiten. | Video-Interstitialanzeigen, native Anzeigen  |
| Microsoft-Anzeigen für die App-Installation | Wählen Sie diese Option, um Anzeigen für die App-Installation oder das Wiedereinschalten von Anzeigen in Apps anzuzeigen, die von anderen Entwicklern im Windows-Ökosystem erstellt wurden, die [Werbeanzeigenkampagnen für ihre Apps erstellen](create-an-ad-campaign-for-your-app.md).  |  Banneranzeigen, Banner-Interstitialwerbung, native Anzeigen  |
| MSN Content-Empfehlungen |  Wählen Sie diese Option aus, um Anzeigen von MSN Content-Empfehlungen zu verarbeiten. |  Banneranzeigen, Banner-Interstitialwerbung  |
| Outbrain |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Outbrain](https://www.outbrain.com/). |  Banneranzeigen, Banner-Interstitialwerbung  |
| Revcontent |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Revcontent](https://www.revcontent.com/). |  Banner, nativ  |
| Smaato |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Smaato](https://www.smaato.com/). |  Banner  |
| Smartclip |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Smartclip](http://www.smartclip.com/). |  Video-Interstitialanzeigen  |
| SpotX |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [SpotX](https://www.spotx.tv/). |  Video-Interstitialanzeigen  |
| Taboola |  Wählen Sie diese Option zum Bereitstellen von Anzeigen von [Taboola](https://www.taboola.com/). |  Banner  |
| Vungle | Wählen Sie diese [Option aus,](https://vungle.com/) um Werbeeinblendungen zu verarbeiten. | Video-Interstitialanzeigen |
| Unterton | Wählen Sie diese [Option aus,](https://www.undertone.com/)um Werbeeinblendungen bereitzustellen. | Banner Interstitial |


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
| Revcontent | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Großbritannien, USA  |
| Smaato | Brasilien, Kanada, Frankreich, Deutschland, Italien, Japan, Spanien, Großbritannien, USA |
| Smartclip | Österreich, Belgien, Dänemark, Finnland, Deutschland, Italien, Niederlande, Norwegen, Schweden, Schweiz  |
| Unterton | USA |

<span id="coppa" />

## <a name="coppa-compliance"></a>COPPA-Compliance

Wenn Sie [eine Ad-Einheit erstellen](#create-ad-unit) oder [eine vorhandene Ad-Einheit auswählen](#available-ad-units), wird der Abschnitt **Coppa-Konformität** unten auf der Seite angezeigt, wenn für die ausgewählte App für die Ad-Einheit mindestens eine Übermittlung vorhanden ist, die im Schritt [Store](../publish/the-app-certification-process.md#in-the-store) im App-Zertifizierungsprozess erreicht wurde.

Im Rahmen des Children's Online Privacy Protection Act ("COPPA"), wählen Sie **Diese Anwendung richtet sich an Kinder unter 13 Jahren** in diesem Abschnitt aus, wenn Ihre App an Kinder unter 13 Jahren gerichtet ist. Wenn Sie diese Option auswählen, wird Microsoft Maßnahmen ergreifen, um die verhaltensorientierten Werbedienste bei der Übermittlung von Werbung in Ihre App zu deaktivieren.

Die **COPPA-Compliance**-Einstellung, die Sie auswählen, wird automatisch auf allen Anzeigeeinheiten für die ausgewählte App angewendet.

> [!IMPORTANT]
> Wenn Ihre App an Kinder unter 13 Jahren gerichtet ist, ergeben sich aus COPPA bestimmte Verpflichtungen für Sie. Weitere Informationen über Ihre Verpflichtungen finden Sie [auf dieser Seite](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
