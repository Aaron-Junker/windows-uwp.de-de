---
title: Xbox Live-Dienstkonfiguration
description: Erfahren Sie, wie Sie die Xbox Live-Dienst für Ihr Spiel zu konfigurieren.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Dienstkonfiguration
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640905"
---
# <a name="xbox-live-service-configuration"></a>Xbox Live-Dienstkonfiguration

## <a name="what-is-service-configuration"></a>Was ist die Dienstkonfiguration?

Kennen Sie möglicherweise mit einigen der Xbox Live-Features wie z. B. [Erfolge](achievements-2017/achievements.md), [Bestenlisten](leaderboards-and-stats-2017/leaderboards.md) und [Vermittlung](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking).

Falls Sie nicht sind, erläutern wir kurz Leaderboards als Beispiel. Leaderboards können Spieler, um ein Wert, der eine Neuerung, im Vergleich mit anderen Spielern darstellt, finden Sie unter.  Z. B. hohe Bewertungen in einem arcadetitel, Rundenzeiten in einem autorennspiel oder Headshots in eine Ego-Shooter. Im Gegensatz zu einem Arcade-Computer nur die besten Ergebnisse aus der Player angezeigt, auf dem physischen Computer gespielt haben, mit der Xbox Live es kann jedoch zum Anzeigen der Bestenliste aus, auf der ganzen Welt.

Aber um dies zu ermöglichen, müssen Sie eine einmalige Konfiguration ausführen, damit Ihre Bestenliste kennt Xbox Live. Zum Beispiel gibt an, ob die Werte in aufsteigender oder absteigender Wert sortiert werden sollen und welche Datenelement es sortieren sollte.

Diese Konfiguration erfolgt auf [Partner Center](https://partner.microsoft.com/dashboard) den meisten Fällen. Bestimmte Entwickler verwenden jedoch [Xbox-Entwickler-Portal (XDP)](https://xdp.xboxlive.com).

Titel des als Teil der Xbox Live Creators-Programm entwickeln möchten, verwenden Sie [Partner Center](https://partner.microsoft.com/dashboard), und Sie können lesen [erste Schritte mit Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) erfahren, wie die Einrichtung.

Wenn Sie sind ein ID@Xbox Entwickler oder Arbeiten mit einem Verleger, die Microsoft-Partner, lesen Sie auf.

## <a name="choose-your-development-portal"></a>Wählen Sie Ihr Entwicklungs-portal

Wie bereits erwähnt, gibt es zwei verschiedene Portale, die zum Konfigurieren von Xbox Live Services verwendet werden können. Partner Center unter [ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard) und die Xbox-Entwicklungs-Portal (XDP) am [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com).

Partner Center wird empfohlen, für alle Titel in Zukunft, aber für bestimmte Features, Sie können weiterhin XDP verwenden möchten. In diesem Abschnitt können informiert Sie, wo Sie den Titel konfigurieren.

Abhängig von Ihrem ausgewählten-Portal finden Sie Informationen zu den Seiten für die Konfiguration von bestimmten Dienst:

* [Partner Center-Konfiguration](configure-xbl/windows-dev-center.md)
* [Xbox-Entwicklungs-Portal-Konfiguration](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) : auf diesen Link klicken, benötigen Sie ein Microsoft-Konto (MSA), der für vollständigen Zugriff auf Xbox Live aktiviert wurde.

Wenn Sie bereits über einen Titel konfiguriert haben, können Sie nach unten zu scrollen [rufen Ihre IDs](#get_ids) erfahren, wie die verschiedenen IDs, die zum Einrichten der Titels des abzurufenden.

### <a name="pcmobile-uwp-game-only"></a>Nur PC/Mobile-UWP-Spiel
Partner Center wird empfohlen, zum Konfigurieren und Verwalten von UWP-Spiele, die nur auf Windows 10-PCs und/oder Windows 10 mobile-Geräten ausgeführt werden.

#### <a name="using-xdp-to-configure-uwp-titles"></a>Verwenden XDP zum Konfigurieren von UWP-Titel

Sie möchten möglicherweise XDP verwenden, um UWP-Titel zu konfigurieren, wenn Sie eine der folgenden Anforderungen haben:

1. Sie verwenden die einnehmen.
2. Sie haben vorhandene Benutzer, Gruppen und Berechtigungen-Setup auf XDP, die Sie weiterhin verwenden möchten.
3. Sie verwenden Tools, die funktioniert nur auf XDP wie Turniere Tool oder Multiplayer-Sitzungsverzeichnis Sitzung Verlaufsanzeige.
4. Entwickeln Sie einen Titel, der Cross-Platform von Play zwischen einer Xbox One XDK-basierten spielen und UWP-PC/Mobile-Version von dasselbe Spiel hat.

Wenn Sie in eine dieser Kategorien fallen nicht, sollten Sie in Partner Center verwenden. Andernfalls sehen Sie unten zum XDP verwenden, um einen Titel für die UWP zu konfigurieren.

Verwenden XDP zum Konfigurieren von Xbox Live Services für UWP-Anwendungen hat einige wichtige Einschränkungen:

* **Nachdem ein Spiel Xbox Live-Dienstkonfiguration auf "Zertifikat/RETAIL" in XDP veröffentlicht wurde, passiert nicht wieder!** Die Konfiguration des Xbox Live-Dienstes für, die von Spielen muss für die Lebensdauer des Spiels Titels in XDP bleiben.
* **Es ist kein Migrationspfad von XDP zum Partner Center.** Wenn Sie Ihre Xbox Live-Konfiguration in XDP starten, müssen Sie manuell es im Partner Center neu erstellen, wenn Sie sie verschieben möchten.

Wenn diese beiden Aspekte, empfiehlt es sich mit Partner Center für PC/Mobile Spiele, es sei denn, Sie in einer der oben beschriebenen Kategorien fallen.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Cross-Play zwischen Xbox One und PC/Mobile Spiele ###
Geräteübergreifende Spiele zwischen der Xbox One und den PC, bekannt als Cross-Wiedergabe, ist es sich um eine Vorstellung unserer Windows 10-Umgebung. In diesem Szenario verwenden sowohl der Xbox One und PC-Versionen für ein Spiel die dieselbe Konfiguration für die Xbox Live.

Dieses Szenario deckt auch den Fall, in dem Sie haben ein existierendes Spiel für Xbox One XDK-und Cross-Play-Unterstützung für eine UWP-Version Ihres Spiels hinzugefügt werden soll.

Führen Sie folgende Schritte aus, um Cross-Play zu implementieren:

* **Verwenden Sie zum Konfigurieren und veröffentlichen Sie Ihr Spiel xdk-Version XDP.** Partner Center können Sie Spiele für Xbox One XDK-nicht zu diesem Zeitpunkt unterstützt.
* **Verwenden Sie eine einzelne Xbox Live-Konfiguration, die Sie für die xdk-Version und die UWP-Versionen für ein Spiel in XDP erstellt.** XDP verfügt jetzt über neue Features, mit die ein Spiel zwischen der xdk-Version und die UWP-Version eines Spiels eine einzelne Xbox Live Dienstkonfiguration freigeben können.
* **Verwenden Sie Partnercenter, zum Erfassen und veröffentlichen Sie Ihr UWP-Spiel.** Allerdings verwenden Sie nicht Partner Center so konfigurieren Sie die Xbox Live-Dienste, wie Ihr Spiel die Dienstkonfiguration verwenden, die Sie in XDP erstellt haben.
* **Xbox Live-Dienstkonfiguration zwischen XDP und Partner Center wird nicht aufgeteilt.** XDP und Partner Center sind nicht aufeinander aufmerksam gemacht, und veröffentlichen eine Dienstkonfiguration aus einer Quelle, überschreibt die Konfiguration, die von der anderen Quelle veröffentlicht. Dies kann dazu führen, dass Benutzerdaten verloren gehen (fehlende Erfolge, gelöschte Spiel gespeichert, usw.) können die benutzerfreundlichkeit zu erstellen. Aus diesem Grund **ist erforderlich, dass 100 % der Xbox Live-Dienstkonfiguration in XDP für Cross-Play xdk-Version und UWP Spiele erfolgt.**

Finden Sie weitere Informationen zu diesem Prozess, z. B. Elemente, die *nicht* Self-service in der [erste Schritte mit Cross-Play Games](get-started-with-partner/get-started-with-cross-play-games.md) Handbuch

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>Xbox One und PC/Mobile Spiele, die nicht Cross-Play-sind separate Versionen
Möglicherweise möchten Sie, dass Ihre Xbox One Version eines Spiels, von der PC/Mobile-Version des dasselbe Spiel getrennt zu halten. In diesem Fall Sie zwei separate Produkte erstellen und führen Sie die nur für Xbox One XDK-Anleitungen und PC/Mobile UWP-Spiel nur jeweils.

Sie können nicht die gleiche Dienstkonfiguration in diesem Fall für beide Versionen verwenden, und Sie müssen manuell erstellen die Dienstkonfiguration für jede separate Version Ihres Spiels, XDP oder im Partner Center entsprechend.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>Ihre IDs erhalten

Um Xbox Live-Dienste zu aktivieren, müssen Sie mehrere IDs, die zum Konfigurieren des Development Kits und den Titel zu erhalten. Diese können wie Xbox Live-Dienstkonfiguration abgerufen werden.

Wenn Sie derzeit nicht über einen Titel in XDP oder über das Partner Center verfügen, finden Sie im Abschnitt oben [Xbox Live-Dienstkonfiguration Portale](#xbox_live_portals) Anleitungen.

### <a name="critical-ids"></a>Kritische IDs

Es gibt drei IDs sind wichtig für die Entwicklung von Titeln und Anwendungen für Xbox One: Sandkasten-ID, die Title-ID und die SCID.

Ist es notwendig, damit eine Sandkasten-ID ein Development Kits verwenden, die Title-ID und SCID sind nicht erforderlich, für die anfängliche Entwicklung sind jedoch erforderlich für die Verwendung Ihrer Xbox Live-Dienste. Aus diesem Grund wird empfohlen, dass Sie alle drei gleichzeitig abrufen.

#### <a name="sandbox-ids"></a>Sandbox-IDs

Die Sandbox bietet inhaltsisolation für Ihr Development Kit während der Entwicklung, sicherzustellen, dass Sie eine saubere Umgebung zum Entwickeln und testen den Titel haben. Der Sandkasten-ID identifiziert Ihre Sandbox. Eine Konsole kann eine Sandbox zu jedem Zeitpunkt nur zugreifen, jedoch eine Sandbox durch mehrere Konsolen zugegriffen werden kann.

Sandbox-IDs wird Groß-/Kleinschreibung beachtet.

**Partner Center**

Wenn Sie den Titel im Partner Center konfigurieren, erhalten Sie die Sandkasten-ID auf der "Xbox Live" Root Konfigurationsseite wie unten dargestellt:

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

Wenn Sie den Titel auf XDP konfigurieren, erhalten Sie die Sandkasten-ID auf der Seite "Übersicht" für Ihr Produkt wie unten dargestellt:

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>Dienst-Konfigurations-ID (SCID)

Im Rahmen der Entwicklung erstellen Sie Ereignisse, Erfolge und eine Vielzahl von anderen online-Features. Diese sind alle Teil der Dienstkonfiguration und die SCID für den Zugriff erforderlich.

SCIDs wird die Groß-/Kleinschreibung beachtet.

**Partner Center**

Um Ihre SCID im Partner Center abzurufen, navigieren Sie zu den Xbox Live Services-Abschnitt, und wechseln Sie zu *Xbox Live-Setup* . Ihre SCID wird in der Tabelle unten angezeigt:

![](images/getting_started/devcenter_scid.png)

**XDP**

Klicken Sie zum Abrufen Ihrer SCID auf XDP navigieren Sie zum Abschnitt "Produkt-Setup" unter Ihrem Titel, und sehen Sie den Titel-ID und die SCID.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>Titel-ID

Titel-ID werden Ihre Titel, Xbox Live Services eindeutig identifiziert. Es wird in den Diensten zum Aktivieren Ihrer Benutzer auf Titel des Live-Inhalten, deren Statistiken zur, Erfolge und So weiter und Live Multiplayer-Funktionalität zu aktivieren.

Titel-IDs können werden Groß-/Kleinschreibung beachtet, je nachdem, wie und wo sie verwendet werden.

**Partner Center**

Ihre Titel-ID im Partner Center befindet sich in der gleichen Tabelle wie die SCID in die *Xbox Live-Setup* Seite.

**XDP**

Ihre Titel-ID auf XDP wird vom gleichen Speicherort abgerufen, wie die SCID ist.

<a name="xbox_live_portals"></a>
