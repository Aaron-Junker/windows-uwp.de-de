---
description: Mit dem Microsoft-Anzeigenvermittlungsdienst können Sie Ihren Anzeigenumsatz und Funktionalitäten zur App-Bewerbung durch die Darstellung von Anzeigen aus mehreren Anzeigennetzwerken verbessern.
title: Microsoft-Anzeigenvermittlungsdienst
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Anzeigenvermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 9197dd4b62df26ff63e469063573789da948c767
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592255"
---
# <a name="microsoft-ad-mediation-service"></a>Microsoft-Anzeigenvermittlungsdienst

Wenn Sie das [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp) verwenden, um [Anzeigen in Ihren Apps anzuzeigen](display-ads-in-your-app.md), können Sie optional den Microsoft-Anzeigenvermittlungsdienst nutzen, um Ihren Anzeigenumsatz zu maximieren. In diesem Artikel wird eine Übersicht über den Anzeigenvermittlungsdienst und seine Ziele bereitgestellt.

Der Anzeigenvermittlungsdienst ist ein Teil der [Monetarisierungsplattform mit Anzeigen bei Microsoft](https://developer.microsoft.com/windows/ad-monetization-platform). Die Plattform besteht aus folgenden Teilen.

![addreferences](images/ad-mediation-service.png)

Der Anzeigenvermittlungsdienst soll den Anzeigenumsatz für Ihre Apps durch Nutzung der folgenden Funktionen maximieren.

## <a name="diversity-of-demand-by-market-and-format"></a>Vielfalt der Nachfrage nach Markt und Format

Der Anzeigenvermittlungsdienst wird in eine Vielzahl von Anzeigennetzwerken über die verschiedenen Anzeigeformate (Banner, Interstitialanzeigenbanner, Interstitialanzeigenvideos und systemeigen) hinweg integriert. Der Anzeigenvermittlungsdienst arbeitet mit jedem Anzeigennetzwerk zusammen, um sicherzustellen, dass Anzeigen mit dem größten Potenzial, das Interesse des Endbenutzers zu wecken, zur Verfügung gestellt werden. Wir werden weiterhin verschiedene Anzeigenpartner hinzufügen, um die Vielfalt und Qualität der Nachfrage zu erweitern.

## <a name="manage-complexity-of-ad-network-relationships"></a>Verwalten der Komplexität von Anzeigennetzwerkbeziehungen  

Der Anzeigenvermittlungsdienst ist in eine Vielzahl von Anzeigennetzwerken integriert, sodass Sie diese Arbeit nicht übernehmen müssen. Nachdem Sie das Microsoft Advertising SDK verwenden, anzeigen in Ihrer app angezeigt, können Sie ändern, dass Ihre Einstellungen für die Vermittlung [im Partner Center](../publish/in-app-ads.md#mediation-settings) anzeigen über mehrere Ad-Netzwerke dargestellt werden. Sie profitieren davon, Anzeigen aus neuen Anzeigennetzwerken abrufen zu können, ohne Änderungen an Ihrem Code vorzunehmen.

Wir verwalten die End-to-End-Beziehung mit den Anzeigennetzwerken in Ihrem Auftrag. Wir kümmern uns um alles von der Anzeigennetzwerkintegration bis zur Bereitstellung von Anzeigen, der Berichterstellung und den Auszahlungen ohne zusätzlichen Aufwand von Ihrer Seite.

## <a name="machine-learning-based-yield-optimization-algorithms"></a>Machine Learning-basierte Algorithmen für die Optimierung der Rentabilität

Der Anzeigenvermittlungsdienst ist darauf ausgelegt, die höchste Rentabilität für Entwickler zu generieren. Dafür wendet er Machine Learning-basierte Algorithmen für die Optimierung der Rentabilität an. Der Algorithmus ermittelt für jeden Anzeigenaufruf die beste *Wasserfallreihenfolge*, in der die Anzeigennetzwerke aufgerufen werden müssen, um Ihren Umsatz zu maximieren. Dies erfolgt durch Berücksichtigung zahlreicher Faktoren, darunter:

* Der Benutzer, der die Anforderung stellt
* Die Anwendung, für die die Anforderung erfolgt
* Frühere Leistung des Anzeigennetzwerks
* Das Anzeigenformat und der Markt, aus dem die Anforderung stammt
* Der Zeitpunkt des Anzeigenaufrufs
* Die Kategorie der App-Inhalte
* Das Benutzersegment
* Der Standort des Benutzers

Neue Anzeigennetzwerke werden automatisch eingefügt und über ein lernendes Budget auf ihre Leistung bewertet. Innerhalb kurzer Zeit finden sie Ihren Platz im Wasserfall. Damit werden die Anzeigennetzwerke wettbewerbsfähiger, und es trägt außerdem dazu bei, dass der Entwickler die Monetarisierung durch Apps optimal nutzt.

Es wird dringend empfohlen, unsere [empfohlenen Vermittlungseinstellungen](../publish/in-app-ads.md#mediation-settings) zu verwenden, um den Umsatz durch Anzeigen in Ihren Apps zu maximieren. Dadurch können unsere Algorithmen,die beste Rentabilität für Ihre App ermöglichen. Allerdings müssen Sie auch die Freiheit gibt, wählen Ihre eigenen Einstellungen für die Vermittlung im Partner Center, um mehr Kontrolle über die Ad-Netzwerke zu erhalten, die dienen, anzeigen und die Reihenfolge, in der dies der Fall.

## <a name="rich-data-and-signals"></a>Umfangreiche Daten und Signale

Der Anzeigenvermittlungsdienst arbeitet mit verschiedenen Anzeigennetzwerken zusammen, um die Zielgruppenadressierung zu verbessern und so relevantere Anzeigen für jeden Benutzer bereitzustellen. Dies erfolgt durch das Senden umfangreicherer Signale über den Benutzer und die App an das Anzeigennetzwerk, während gleichzeitig Datenschutzanforderungen berücksichtigt werden.

## <a name="related-topics"></a>Verwandte Themen

* [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp)
* [Einstellungen für die Vermittlung](../publish/in-app-ads.md#mediation-settings)
* [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md)
