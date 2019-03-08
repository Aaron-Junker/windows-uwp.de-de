---
title: Konfigurieren von Kontextsuche
description: Erfahren Sie, wie so konfigurieren Sie die Kontextsuche um Spiele Clips und Broadcasts zu markieren.
ms.assetid: 6cb2cb10-811a-4b20-9b9b-a3fc59a033c2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Dienstkonfiguration, Kontextsuche, Spiele Clip übertragen
ms.localizationpriority: medium
ms.openlocfilehash: c8c78f115a160c82a28881a3c551a958cdf4b68c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607925"
---
# <a name="configuring-contextual-search"></a>Konfigurieren von Kontextsuche

## <a name="configuration-info"></a>Konfigurationsinformationen

### <a name="designing-your-data"></a>Entwerfen Ihre Daten
Wenn Sie den Titel für die Kontextsuche konfigurieren zu können, ist es wichtig, um die Dinge zu denken, die den Titel eindeutig zu machen.  Wenn ein Benutzer Videoinhalte sucht, wäre was im Vordergrund zu suchende?  Wie genau werden die Daten muss?

Kontextsuche unterstützt die Möglichkeit, Inhalte filtern und zu sortieren, damit Ihre Metadaten, diese suchen-Konstrukte hinweisen, um sicherzustellen, dass gute Abdeckung.  Eine gute Faustregeln besteht darin, Ihr Spiel auf die Daten in zwei Pivots vorstellen:
1. Status - ex spielen.  Zeichen, sorgfältig, Map, Rolle, Checkpoint/Story Point, DLC-Ebene
2. Fähigkeit der Benutzer - Beispiel. K/D-Verhältnis, namhaften, Rang, Geschwindigkeit zur Laufzeit

Die erste PowerPivot eignet sich hervorragend zum Filtern der Inhalte, die zweite eignet sich hervorragend für die Sortierung.  Darüber hinaus sind je nach Art des Inhalts wird eingeblendet, Vertrauensebenen Stat Granularität wichtig.  Für Broadcasts, vorausgesetzt, es gibt eine begrenzte Anzahl von aktiven Broadcasts zu jedem Zeitpunkt sicherzustellen, dass niedrigere Granularität Daten werden eine höhere Wahrscheinlichkeit für einen erfolgreichen Abschluss der Suche.  Für Spiele Clips erhöht die Anzahl kontinuierlich, damit eine feinere Granularität genauere Ergebnisse sicherzustellen, dass Daten werden.

Es ist recht wahrscheinlich, dass Sie bereits über diese Daten, die für Xbox Live-Features wie Rich-Anwesenheit erstellt oder Hero-Statistiken, ist eine gute Gelegenheit, nutzen bereits Arbeit abgeschlossene.

Wenn Sie Statistiken zum Filtern der Inhalte verwenden, achten Sie darauf, dass die Statistiken für alle Kontextsuche Abfragen zu einem beliebigen Zeitpunkt verfügbar sind.  Sie sollten Ihre Ereignisse entsprechend entwerfen, um dies zu unterstützen.

Z. B. Wenn Sie Statistiken wie SinglePlayerMap und MultiplayerMap, Inhalte filtern verwenden, wird der Spieler nur in einer davon werden.  Allerdings werden beide Werte zum Abfragen des Diensts zu einem beliebigen Zeitpunkt verfügbar sein.  Es ist wichtig, wie Sie eine festlegen, die Sie den anderen deaktivieren.  Eine leere Zeichenfolge eignet sich hervorragend für Zeichenfolge-Statistiken (Stellen Sie sicher, dass nicht zu, die in der UI-Konfiguration als Option enthalten).

### <a name="configuring-a-stat-for-contextual-search"></a>Konfigurieren eine Statistik für die Kontextsuche
Konfigurieren den Titel für die Kontextsuche ist einfach, nachdem Sie die Ereignisse und Statistiken diese Leistung das kennzeichnen eingerichtet haben.  Finden Sie in anderen vorhandenen XDP oder über das Partner Center-Dokumentation zum Einrichten von Kontextsuche, wenn Sie noch nicht vertraut sind.

![](../images/contextual_search/config02.png)

In der Abbildung oben ist ein Beispiel der Hauptseite auf, nachdem Sie die Kontextsuche-Kachel in der Service Configuration hauptsächlich XDP klicken.

Auch wenn dies der erste Mal Sie diese Seite besucht haben ist, ist es möglich, dass die Statistiken, die bereits konfiguriert, für die Kontextsuche angezeigt werden.  Dies ist, da alle Ihre Hero-Statistiken für die Kontextsuche automatisch eingerichtet werden. Sie sind nicht erforderlich, diese Hero-Statistiken in Ihrer Konfiguration beibehalten, aber sie sind im Allgemeinen hervorragend für die Sortierung (und für die Sie bereits fertig).

Derzeit ist die maximale Anzahl von Stat-Instanzen, die Sie für die Kontextsuche konfigurieren können 10.

Auf dieser Seite wird jeweils von den Statistiken wie folgt dargestellt: Priorität: Anzeigename (Stat Instanzname)

Jedes davon werden im nächsten Abschnitt ausführlicher erläutert.

![](../images/contextual_search/config01.png)

Die Abbildung oben wird der Bildschirm, der angezeigt wird, wenn Sie eine neue Kontextsuche Stat erstellen oder bearbeiten möchten und bereits vorhandene.

Um erfolgreich eine Statistik für die Kontextsuche konfigurieren, führen Sie die folgenden Schritte aus:
1. Wählen Sie Ihre Stat-Instanz.

  ![](../images/contextual_search/config03.png)

  Bitte beachten Sie, nur Stat-Instanzen unterstützt werden – Stat-Vorlagen werden nicht akzeptiert.  Sie sollten auch die Sichtbarkeit kennen, die Sie für die Stat-Instanz (in den Statistiken Teil XDP oder über das Partner Center konfiguriert) festgelegt haben.  Nur Statistiken, die als gekennzeichnete **öffnen** in Drittanbieter-Benutzeroberfläche angezeigt wird.

2. Wählen Sie die Stat-Priorität. Dies ist eine Möglichkeit, um die Bedeutung dieser Stat relativ zu anderen für Erfahrungen/Suchalgorithmen voneinander abzugrenzen.  Die zulässigen Werte sind 1 bis 10 (1 ist die höchste).  Lassen Sie Sie als 0 oder leer, um diese Option, um die ignoriert werden.
3. Fügen Sie Ihren Anzeigenamen hinzu.  Dies ist eine lokalisierbare Zeichenfolge, die für den Endbenutzer verfügbar gemacht wird.
4. Überprüfen Sie, ob Sie diese Stat zum Filtern oder Sortieren der Ergebnisse verwenden möchten.  Sehen Sie beide in einigen Fällen (wenn der Wert, der den Status einer Zahl - vorzugsweise in einem Bereich definiert ist).
5. Wählen Sie, wie Ihre Stat Wert dargestellt wird.  Im Fall müssen Sie 3 Optionen.
   * Rohwert - die Stat wird dargestellt, und stellt keine Anforderungen Bereich an.  Dies wird am besten für die Sortierung verwendet.
   * Set - werden die Werte einzelner lokalisierbaren Zeichenfolgen zugeordnet.  Dies wird am besten für die Filterung verwendet werden.
   * Bereich von - liegen die Werte in einer Minute und maximalen Bereich.  Dieser Typ ist sehr praktisch, wenn Sie filtern möchten, und Sortieren von Daten.

#### <a name="explaining-sets"></a>Erläutert, Sätze
Wenn Sie sich nicht mit vertraut sind, sind sie eine Möglichkeit, potenzielle Stat Werte lokalisierbaren Zeichenfolgen zugeordnet, die für Endbenutzer verfügbar gemacht werden kann.  Sie sind sehr wichtig, die Kontextsuche-Statistiken, die, denen Sie mit filtern möchten.

{% unformatierten %} Als Beispiele nehmen wir an, dass ich eine ganze Zahl namens Stat SinglePlayerMap verfügen.  Zeigt eine Reihe von Zahlen für Endbenutzer ergibt keinen Sinn.  Stattdessen ich die Erstellung mehrerer Zuordnungen wie ```{{0, “Blood Gulch”}, {1, “Lockout”}, {2, “Zanzibar”}}```.  Der Benutzer sieht die Zeichenfolge allerdings verwenden wir die Anzahl als Teil der Kontextsuche-Abfrage.  Sie können ganze Zahlen oder Zeichenfolgen mit lokalisierten Zeichenfolgen in Gruppen zuordnen.
{% Endraw %}

### <a name="updating-your-contextual-search-document"></a>Aktualisieren das Dokument Kontextsuche
Derzeit können Sie leicht die Statistiken für Ihre Kontextsuche Dokument ändern, da wir nur Broadcasting im Moment unterstützt (die flüchtige Daten) werden.  Sobald wir beginnen, um Spiele Clip tagging (September 2015), wenn Sie sich entschieden, hinzufügen oder Entfernen von Statistiken zu unterstützen, werden Sie Clips beeinträchtigt, die bereits indiziert wurden.  Wenn Sie eine neue Stat hinzufügen, nur game-Clips und Broadcasts erstellt, nachdem die Änderung in Kraft tritt gekennzeichnet und in Abfragen mit der neuen Stat eingeblendet. Alte Spiel Clips Abfrage-kann werden nicht von diesem Status. Wenn Sie einen Status aus dem Dokument Kontextsuche entfernen, wird die alte Stat des Spiels Clips angefügt bedeutungslos und werden nicht zurückgegeben.
