---
author: jnHs
Description: "Sie können Ihre App mit Apps von anderen Entwicklern bewerben. Dieses Feature wird Community-Anzeigen genannt."
title: Informationen zu Community-Anzeigen
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.openlocfilehash: 708f46dc0be53961d8fd26f41a933e5756720a98
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="about-community-ads"></a>Informationen zu Community-Anzeigen

Wenn Ihre App ein **AdMediatorControl**- oder ein **AdControl**-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre App mit anderen Entwicklern mit Apps im WindowsStore kostenlos bewerben. Dieses Feature wird *Community-Anzeigen* genannt.  

Das funktioniert wie folgt:

* Nachdem Sie sich für die [Teilnahme an Community-Anzeigen](#how-to-opt-in-to-community-ads) entschieden und eine [kostenlose Community-Anzeigenkampagne](create-an-ad-campaign-for-your-app.md) erstellt haben, teilt sich Ihre App den Platz für Werbeanzeigen mit anderen Entwicklern, die sich ebenfalls für die Nutzung von Community-Anzeigen entschieden haben. In Ihrer App werden Anzeigen für Apps anderer Entwickler angezeigt, die an Community-Anzeigen teilnehmen. Im Gegenzug werden in deren Apps Anzeigen für Ihre App angezeigt.
* Durch die Anzeige von Community-Anzeigen in Ihrer App erwerben Sie Guthaben für Werbefläche in anderen Apps. Das Guthaben wird wie folgt berechnet:
  * Für jedes Land bzw. jede Region, in dem bzw. in der eine App verfügbar ist, die Community-Anzeigen bereitstellt, wird der jeweils aktuelle eCPM-Wert (effektive Kosten pro tausend Anzeigenaufrufe) für das Land oder die Region mit der Anzahl von Anforderungen für Community-Anzeigen multipliziert, die von Ihrer App im betreffenden Land oder in der betreffenden Region gestellt wurden. Dieser Wert entspricht dem Guthaben, das Sie im betreffenden Land oder in der betreffenden Region für Ihre App erworben haben.
  * Ihr gesamtes erworbenes Guthaben für einen bestimmten Zeitraum entspricht der Summe sämtlicher Guthaben, die mit allen Ihren Apps, die Community-Anzeigen bereitstellen, in allen Ländern oder Regionen erwirtschaftet wurden.
* Ihr Guthaben wird gleichmäßig auf alle aktiven Community-Anzeigenkampagnen verteilt und auf der Grundlage der jeweils aktuellen eCPM-Werte der Länder, auf die Ihre Community-Anzeigenkampagnen ausgerichtet sind, in Anzeigenaufrufe für Ihre App umgewandelt.
* Informationen zum Nachverfolgen der Performance der Community-Anzeigen in Ihrer App finden Sie im [Bericht zur Anzeigen-Performance auf Kontoebene](advertising-performance-report.md#account-level-advertising-performance-report).

## <a name="how-to-opt-in-to-community-ads"></a>So melden Sie sich für Community-Anzeigen an

So melden Sie sich für Community-Anzeigen an:

1. Navigieren Sie im WindowsDevCenter-Dashboard zu **Monetarisierung** &gt; **Gewinnbringende Nutzung mit Anzeigen**.
2. Aktivieren Sie im Abschnitt **Community ads** das Kontrollkästchen **Show community ads in my app**.
   > **Hinweis**  Nach dem Aktivieren oder Deaktivieren dieses Kontrollkästchens müssen Sie Ihre App nicht neu veröffentlichen, damit die Änderungen wirksam werden.

3. [Erstellen Sie eine Anzeigenkampagne](create-an-ad-campaign-for-your-app.md) für Ihre App. Wählen Sie den Kampagnentyp **Kostenlose Community-Anzeigen** aus.


## <a name="related-topics"></a>Verwandte Themen

* [Monetisierung durch Werbeanzeigen](monetize-with-ads.md)
* [Erstellen einer Anzeigenkampagne für Ihre App](create-an-ad-campaign-for-your-app.md)
