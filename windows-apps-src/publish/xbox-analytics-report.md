---
author: jnHs
Description: The Xbox analytics report in Partner Center shows you statistics about how your customers are engaging with the Xbox features in your product.
title: Bericht der Xbox Analyse
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, UWP, Xbox Analyse, Xbox Live-Analyse, Xbox-Statistiken
ms.localizationpriority: medium
ms.openlocfilehash: c2c1f54a402fc4ae7184f1d588cc255525f762c2
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "7102435"
---
# <a name="xbox-analytics-report"></a>Xbox Analysebericht

Der **Xbox** Analysebericht im [Partner Center](https://partner.microsoft.com/dashboard) zeigt Ihnen Statistiken darüber wie Ihre Kunden die Xbox-Features in Ihrem Spiel interagieren. Es enthält auch Informationen zur Dienstintegrität, um Clientfehler zu beheben.

> [!IMPORTANT]
> Dieser Bericht wird nur angezeigt, wenn Sie ein Spiel für Xbox oder ein Spiel veröffentlichen, das Xbox Live-Dienste verwendet. Zu diesem Zweck Sie [konzeptgenehmigungsprozess](../gaming/concept-approval.md), einschließlich Spiele, die von [Microsoft-Partnern](../xbox-live/developer-program-overview.md#microsoft-partners) veröffentlicht und Spiele über übermittelt durchlaufen müssen die [ ID@Xbox Programm](../xbox-live/developer-program-overview.md#id). Spiele, die über das [Xbox Live Creators-Programm](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) veröffentlicht sind derzeit nicht sichtbar in diesem Bericht.

Sie können den **Xbox** Analysebericht im linken Navigationsmenü für Ihr Spiel anzeigen, indem erweitern **Analysieren** und Auswählen von **Xbox-Analyse**.  Sie können diese Daten im Partner Center oder [den Bericht herunterladen](download-analytic-reports.md) offline anzeigen anzeigen.


## <a name="overview-tab"></a>Registerkarte Übersicht

Die Abschnitte der Registerkarte **Übersicht** zeigen Informationen über die Spieler und deren Interaktion mit Xbox Live-Features an.

Für viele dieser Statistiken zeigen wir auch den **Xbox Durchschnitt** an, damit Sie leicht erkennen können, wie Ihre Kunden im Vergleich zu den durchschnittlichen Xbox-Kunden mit Xbox interagieren.

> [!NOTE]
> Diese Statistiken enthalten Kunden, die mit Xbox Live verbunden sind und nicht alle Xbox-Kunden.


### <a name="concurrent-usage"></a>Gleichzeitige Nutzung

In diesem Abschnitt zeigt die Nutzungsdaten nahezu in Echtzeit an (5-15 Minuten Latenz) gegenüber der Anzahl Kunden, die Ihr Spiel stündlich oder pro Minute spielen. Sie können den Zeitraum auswählen (von **Letzte Stunde** bis **Letzten 7 Tage**), indem Sie das Filtersymbol in der oberen rechten Ecke in diesem Abschnitt auswählen.


### <a name="gamerscore-distribution"></a>Gamerscore-Verteilung

In diesem Abschnitt finden Sie Informationen zum Gamerscore Ihrer Kunden. Sie können **alle Spiele** auswählen, um die Verteilung aller Gamerscores Ihren Kunden anzuzeigen oder **dieses Spiel** auswählen, um die Verteilung von Gamerscores anzuzeigen, die nur durch Ihr Spiel erhalten wurden.


### <a name="achievement-unlocks"></a>Freigeschaltete Erfolge

In diesem Abschnitt wird die Gesamtanzahl der Kunden angezeigt, die im angegebenen Zeitraum den Erfolg freigeschaltet haben. Sie können den Zeitraum auswählen (**Letzter Tag**, **Letzen 30 Tage** oder **Lebensdauer**), indem Sie das Filtersymbol in der oberen rechten Ecke in diesem Abschnitt auswählen.


### <a name="game-statistics"></a>Spielstatistiken

Dieser Abschnitt enthält Registerkarten, die Sie auswählen können, um unterschiedliche Daten über die Kunden Ihres Spiels anzuzeigen. Beachten Sie, dass die Statistiken in diesem Abschnitt auf die allgemeine Featureverwendung hinweisen und nicht auf Ihr bestimmtes Produkt.

- Die Registerkarte **Social Usage** zeigt Daten im Zusammenhang über die sozialen Interaktionen Ihrer Kunden an.
   - **Spieleinladungen** zeigt den Prozentsatz Ihrer Kunden an, denen Sie Einladungen (für ein Spiel) gesendet haben.
   - **Party-Chat** zeigt den Prozentsatz Ihrer Kunden an, die Party-Chat verwenden (für alle Spiele).
   - **SMS** zeigt den Prozentsatz Ihrer Kunden an, die Nachrichten über die Xbox-Shell senden (für ein Spiel).
- Die Registerkarte **Streaming Usage** zeigt die Prozentsätze der Kunden Ihrer Spiele an, die Spiele auf Twitch und YouTube ansehen oder streamen (für alle Spiele).
- Die Registerkarte **Game DVR usage** zeigt Daten darüber an, wie Ihre Kunden Spiele aufzeichnen und anzeigen. Sie können die Prozentsätze der Kunden sehen, die Spielclips und Bildschirmfotos des Spiels angezeigt und hochgeladen haben (für alle Spiele).


### <a name="friends-and-followers"></a>Freunde und Follower

In diesem Abschnitt wird die **durchschnittliche Anzahl der Freunde** und **die durchschnittliche Anzahl der Follower** für Kunden angezeigt, die Ihr Spiel spielen.


### <a name="accessory-usage"></a>Nutzung des Zubehörs

Dieses Diagramm zeigt die Prozentsätze der Kunden Ihres Spiels an, die externe Festplatten und Xbox Elite Wireless Controller verwenden (auf der Xbox).

Diese Daten bedeutet nicht, das diese Kunden Ihr Produkt auf externen Festplatten installiert haben oder Elite-Controller während der Wiedergabe verwenden. Er bezieht sich darauf, wie viele Kunden Ihres Produkts diese Features in der Regel verwenden.


### <a name="connection-type"></a>Verbindungstyp

Dieses Diagramm zeigt die Prozentsätze für die Benutzer Ihres Produkts an, die **verkabelte** Internetverbindungen im Vergleich zu **WLAN** nutzen (auf der Xbox).


## <a name="xbox-live-service-health-tab"></a>Xbox Live Registerkarte „Dienstintegrität”

In den Abschnitten auf der **Xbox Live Registerkarte „Dienstintegrität”** können Sie die Auswirkung der Xbox Live Clientfehler sehen, einschließlich der Begrenzung der Übertragungsrate. Außerdem können Sie einen Drilldown nach Endpunkt und Statuscode durchführen, um Informationen abzurufen, die bei der Behebung der Probleme helfen. Sie können dort ebenfalls die Verfügbarkeit von Xbox Live-Diensten für die Aufrufe Ihres Produkts einsehen.

> [!NOTE]
> Wenn Sie diese Informationen einsehen und Probleme behandeln, empfehlen wir ein Priorisieren der Begrenzung der Übertragungsrate, da diese Fehler in der Regel die größte Auswirkung auf Ihre Kunden haben.


### <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Registerkarte können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **30D** (30Tage), aber Sie können Daten für **7D** (7Tage) oder einen benutzerdefinierten Datumsbereich auswählen, den Sie festlegen (nicht mehr als 30Tage). Beachten Sie für einen benutzerdefinierten Datumsbereich, dass alle Diagramme den Diagrammbereich auf den ersten und letzten Tag der Daten innerhalb des Bereichs kürzen, den Sie eingegeben haben.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite nach Paketversion, Gerätetyp und/oder Sandbox zu filtern.
- **Paketversion**: Der Standardfilter ist **Alle Versionen**, aber Sie können die Dienstintegritätsdaten auf eine bestimmte Paketversion beschränken.
- **Gerätetyp**: Die Standardeinstellung ist **Alle Gerät**, Sie können jedoch die Dienstintegritätsdaten auf einen bestimmten Gerätetyp beschränken.
- **Sandbox**: Die Standardeinstellung ist **RETAIL**, Sie können jedoch die Dienstintegritätsdaten auf eine bestimmte Sandbox beschränken.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den ausgewählten Zeitraum und alle von Ihnen ausgewählten Filter. In einigen Abschnitten können Sie zusätzliche Filter anwenden.


### <a name="client-errors-by-service"></a>Clientfehler pro Dienst

Das Diagramm **Clientfehler pro Dienst** zeigt die Anzahl der täglichen Clientfehler (4xx) auf jedem Xbox Live-Dienst über einen ausgewählten Zeitraum an.

Sie können auch nur Ratenbegrenzungsfehler anzeigen, indem Sie **Begrenzung der Übertragungsrate** auswählen. Zeigt die täglichen Fehler bei der Begrenzung der Übertragungsrate (429) und der Ratenbegrenzungsausnahmen (429E) bei Xbox Live-Diensten über den ausgewählten Zeitraum an.

> [!NOTE]
> Ein Statuscode 429E wurde tatsächlich erfolgreich als Statuscode 200 zurückgegeben, wäre jedoch in der Rate begrenzt, wenn der Dienst ein hohes Volumen zu dem Zeitpunkt hatte, daher wird empfohlen, dass Sie es den Fehler genauso behandeln, als ob er erzwungen (429) wurde.

Dieses Diagramm zeigt standardmäßig die sechs Top-Dienste nach Fehleranzahl an. Sie können das Filtersymbol in der oberen rechten Ecke dieses Abschnitts auswählen, um unterschiedliche Dienste auszuwählen. Sie können Fehler für maximal sechs Dienste auf einmal anzeigen.

> [!NOTE]
> Die Legende zeigt nur die unterschiedlichen Präfixe für die einzelnen Dienste an (z.B. **presence** anstelle von **presence.xboxlive.com**). Sie finden die vollständige Service-Adresse in der Tabelle **Clientfehler nach Endpunkt** weiter unten in der Registerkarte **Xbox Live-Dienstintegrität** .


### <a name="service-availability"></a>Dienstverfügbarkeit

Das Diagramm **Dienstverfügbarkeit** zeigt die tägliche Verfügbarkeit aller Xbox Live-Dienste über den ausgewählten Zeitraum an. Dies wird als *1-(Serverfehler insgesamt (5xx) /Antworten insgesamt)* berechnet. Dies ist produktspezifisch und nicht Xbox Live-spezifisch.

Dieses Diagramm zeigt standardmäßig die sechs Dienste mit der niedrigsten Verfügbarkeit an. Sie können das Filtersymbol in der oberen rechten Ecke dieses Abschnitts auswählen, um unterschiedliche Dienste auszuwählen. Sie können die Verfügbarkeit für maximal sechs Dienste auf einmal anzeigen.

> [!NOTE]
> Die Legende zeigt nur die unterschiedlichen Präfixe für die einzelnen Dienste an (z.B. **presence** anstelle von **presence.xboxlive.com**). Sie finden die vollständige Service-Adresse in der Tabelle **Clientfehler nach Endpunkt** weiter unten in der Registerkarte **Xbox Live-Dienstintegrität** .


### <a name="client-errors-by-endpoint"></a>Clientfehler nach Endpunkt

Die Tabelle **Clientfehler nach Endpunkt** zeigt die Anzahl der täglichen Clientfehler (4xx) pro Xbox Live-Dienst, Endpunkt und Statuscode über einen ausgewählten Zeitraum an. Standardmäßig wird diese Tabelle nach der Gesamtzahl der Service-Antworten in absteigender Reihenfolge sortiert, Sie können die Sortierreihenfolge allerdings ändern, indem Sie auf die Spaltenüberschriften klicken.

Sie können auch nur Ratenbegrenzungsfehler anzeigen, indem Sie **Begrenzung der Übertragungsrate** auswählen. Zeigt die täglichen Fehler bei der Begrenzung der Übertragungsrate (429) und der Ratenbegrenzungsausnahmen (429E) bei Xbox Live-Diensten, Endpunkten und Statuscodes über den ausgewählten Zeitraum an.

> [!NOTE]
> Ein Statuscode 429E wurde tatsächlich erfolgreich als Statuscode 200 zurückgegeben, wäre jedoch in der Rate begrenzt, wenn der Dienst ein hohes Volumen zu dem Zeitpunkt hatte, daher wird empfohlen, dass Sie es den Fehler genauso behandeln, als ob er erzwungen (429) wurde.










 

 
