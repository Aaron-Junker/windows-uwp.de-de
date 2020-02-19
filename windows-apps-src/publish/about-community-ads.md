---
Description: Sie können Ihre App mit Apps von anderen Entwicklern bewerben. Dieses Feature wird Community-Anzeigen genannt.
title: Informationen zu Community-Anzeigen
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8da380cb49d584e56f584f3ad321d601d211faf0
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463442"
---
# <a name="about-community-ads"></a>Informationen zu Community-Anzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://aka.ms/ad-monetization-shutdown)

Wenn in Ihrer APP [Banner oder Banner mit Interstitial angezeigt](../monetize/display-ads-in-your-app.md)werden, können Sie Ihre APP mit anderen Entwicklern, die apps im Microsoft Store kostenlos nutzen, übergreifende herauf Stufen. Dieses Feature wird *Community-Anzeigen* genannt.  

Das funktioniert wie folgt:

* Nachdem Sie sich wie unten beschrieben für die Community-Werbung angemeldet haben, können Sie [eine kostenlose Community-Werbekampagne erstellen](create-an-ad-campaign-for-your-app.md). Ihre APP gibt dann Werbe Speicher für Werbeeinblendungen für andere Entwickler weiter, die auch communitywerbung abonnieren. In Ihrer App werden Anzeigen für Apps anderer Entwickler angezeigt, die an Community-Anzeigen teilnehmen. Im Gegenzug werden in deren Apps Anzeigen für Ihre App angezeigt.
* Durch die Anzeige von Community-Anzeigen in Ihrer App erwerben Sie Guthaben für Werbefläche in anderen Apps. Das Guthaben wird wie folgt berechnet:
  * Für jedes Land bzw. jede Region, in dem bzw. in der eine App verfügbar ist, die Community-Anzeigen bereitstellt, wird der jeweils aktuelle eCPM-Wert (effektive Kosten pro tausend Anzeigenaufrufe) für das Land oder die Region mit der Anzahl von Anforderungen für Community-Anzeigen multipliziert, die von Ihrer App im betreffenden Land oder in der betreffenden Region gestellt wurden. Dieser Wert entspricht dem Guthaben, das Sie im betreffenden Land oder in der betreffenden Region für Ihre App erworben haben.
  * Ihr gesamtes erworbenes Guthaben für einen bestimmten Zeitraum entspricht der Summe sämtlicher Guthaben, die mit allen Ihren Apps, die Community-Anzeigen bereitstellen, in allen Ländern oder Regionen erwirtschaftet wurden.
* Ihr Guthaben wird gleichmäßig auf alle aktiven Community-Anzeigenkampagnen verteilt und auf der Grundlage der jeweils aktuellen eCPM-Werte der Länder, auf die Ihre Community-Anzeigenkampagnen ausgerichtet sind, in Anzeigenaufrufe für Ihre App umgewandelt.
* Informationen zum Nachverfolgen der Performance der Community-Anzeigen in Ihrer App finden Sie im [Bericht zur Anzeigen-Performance](advertising-performance-report.md).

### <a name="opt-in-to-community-ads"></a>Melden Sie sich für Community-Anzeigen an

Bevor Sie eine Community AD-Kampagne für eine Ihrer Apps erstellen können, müssen Sie sich auf der Seite **Monetize** &gt; **in-App-Werbung** im [Partner Center](https://partner.microsoft.com/dashboard)anmelden.

So abonnieren Sie communitywerbung für eine UWP-App:

1. Wählen Sie eine Ad-Einheit aus, die Sie in der App verwenden, und Scrollen Sie nach unten zu den **Vermittlungs Einstellungen**.
2. Wenn **Microsoft meine Einstellungen optimieren** aktiviert ist, werden communitywerbung automatisch für Ihre Ad-Einheit aktiviert. Wählen Sie andernfalls in der Dropdown Liste **Ziel** die Baselineversion oder eine marktspezifische Konfiguration aus, und aktivieren Sie dann das Kontrollkästchen **Microsoft Community ADS** in der Liste **andere Ad-Netzwerke** .

    > [!NOTE]
    > Mithilfe der **Gewichtungs** Felder können Sie das Verhältnis von Werbeeinblendungen aus kostenpflichtigen Netzwerken und anderen Ad-Netzwerken, einschließlich der communitywerbung, angeben.

Sie müssen Ihre App nicht noch einmal veröffentlichen, nachdem Sie Ihre Auswahl getroffen haben. Wenn Sie sich angemeldet haben, werden Sie feststellen, dass Sie **Community-Anzeige (kostenlos)** als Kampagnentyp angeben können, wenn Sie eine [Anzeigenkampagne erstellen](create-an-ad-campaign-for-your-app.md).

### <a name="related-topics"></a>Verwandte Themen

* [In-App-Anzeigen](in-app-ads.md)
* [Erstellen einer Werbekampagne für Ihre APP](create-an-ad-campaign-for-your-app.md)
