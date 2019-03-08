---
title: Xbox Live Creators-Dienstkonfiguration
description: Informationen Sie zu Xbox Live-Dienstkonfiguration für das Creators-Programm.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662235"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>Xbox Live-Dienstkonfiguration für das Creators-Programm

## <a name="what-is-service-configuration"></a>Was ist die Dienstkonfiguration?

Kennen Sie möglicherweise mit einigen der Xbox Live-Features wie z. B. [Bestenlisten](../leaderboards-and-stats-2017/leaderboards.md) und [verbundenen Speicher](../storage-platform/connected-storage/connected-storage-technical-overview.md).

Falls Sie nicht sind, erläutern wir kurz Leaderboards als Beispiel. Leaderboards können Spieler, um ein Wert, der eine Neuerung, im Vergleich mit anderen Spielern darstellt, finden Sie unter. Z. B. hohe Bewertungen in einem arcadetitel, Rundenzeiten in einem autorennspiel oder Headshots in eine Ego-Shooter. Im Gegensatz zu einem Arcade-Computer nur die besten Ergebnisse aus der Player angezeigt, auf dem physischen Computer gespielt haben, mit der Xbox Live es kann jedoch zum Anzeigen der Bestenliste aus, auf der ganzen Welt.

Aber um dies zu ermöglichen, müssen Sie eine einmalige Konfiguration ausführen, damit Ihre Bestenliste kennt Xbox Live. Zum Beispiel gibt an, ob die Werte in aufsteigender oder absteigender Wert sortiert werden sollen und welche Datenelement es sortieren sollte.

Diese Konfiguration erfolgt in [Partner Center](https://partner.microsoft.com/dashboard) für Xbox Live Creators-Programm, und Sie lesen können [erste Schritte mit Xbox Live](get-started-with-xbox-live-creators.md) erfahren, wie die Einrichtung.

## <a name="get-your-ids"></a>Ihre IDs erhalten

Um Xbox Live-Dienste zu aktivieren, müssen Sie mehrere IDs, die zum Konfigurieren Ihrer Entwicklungsumgebung und Titel abzurufen. Diese können durch Aktualisieren der Xbox Live-Dienstkonfiguration abgerufen werden.

Wenn Sie nicht gerade einen Titel im Partner Center verfügen, finden Sie unter [erstellen und Testen eines neuen Titels für den Ersteller](create-and-test-a-new-creators-title.md) Anleitungen.

### <a name="critical-ids"></a>Kritische IDs

Es gibt drei IDs sind wichtig für die Entwicklung von Titeln und Anwendungen für Xbox One: Sandkasten-ID, die Title-ID und die Dienstkonfiguration ID (SCID).

Ist es notwendig, um einen Sandkasten-ID zum Einrichten Ihrer Entwicklungsumgebung zu erhalten, die Title-ID und SCID sind nicht erforderlich, für die anfängliche Entwicklung sind jedoch erforderlich für die Verwendung Ihrer Xbox Live-Dienste. Aus diesem Grund wird empfohlen, dass Sie alle drei gleichzeitig abrufen. Sie können alle IDs auf der "Xbox Live" Root Konfigurationsseite wie folgt anzeigen:

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>Sandbox-IDs

Die Sandbox bietet inhaltsisolation für Ihre Umgebung während der Entwicklung, sicherzustellen, dass er zum Entwickeln und testen den Titel bereinigt ist. Der Sandkasten-ID identifiziert Ihre Sandbox. Eine Konsole kann eine Sandbox zu jedem Zeitpunkt nur zugreifen, jedoch eine Sandbox durch mehrere Konsolen zugegriffen werden kann.

Sandbox-IDs wird Groß-/Kleinschreibung beachtet.

#### <a name="service-configuration-id-scid"></a>Dienst-Konfigurations-ID (SCID)

Im Rahmen der Entwicklung erstellen Sie Statistiken, Leaderboards und eine Vielzahl von anderen online-Features. Diese sind alle Teil der Dienstkonfiguration und die SCID für den Zugriff erforderlich. SCIDs wird die Groß-/Kleinschreibung beachtet.

#### <a name="title-id"></a>Titel-ID

Titel-ID werden Ihre Titel, Xbox Live Services eindeutig identifiziert. Es wird in den Diensten zum Aktivieren Ihrer Benutzer auf Titel des Live-Inhalten, deren Statistiken zur, Erfolge und So weiter und Live Multiplayer-Funktionalität zu aktivieren.

Titel-IDs können werden Groß-/Kleinschreibung beachtet, je nachdem, wie und wo sie verwendet werden.

## <a name="publish-your-xbox-live-service-configuration"></a>Veröffentlichen Sie Ihre Xbox Live-Dienstkonfiguration

Wenn Sie Änderungen an der Xbox Live-Konfiguration für Ihr Spiel vornehmen, müssen Sie die Änderungen zu veröffentlichen, bevor sie vom Rest des Xbox Live übernommen und werden, indem Sie Ihr Spiel angezeigt können. Wenn Sie weiterhin auf Ihr Spiel funktioniert, veröffentlichen Sie in Ihre Entwicklung Sandbox ein. Die Sandbox für die Entwicklung können Sie Änderungen an Ihrem Spiel in einer isolierten Umgebung arbeiten. Wenn Ihr Spiel öffentlich freigegeben wird, wird die Xbox Live-Konfiguration automatisch mit der Sandbox RETAIL veröffentlicht werden.
Sind standardmäßig ein Xbox-Konsolen und Windows 10-PCs in der Sandbox Einzelhandel.

Klicken Sie auf der Xbox Live-Konfigurationsseite, auf die **Test** Schaltfläche, um die aktuelle Konfiguration von Xbox Live in Ihre Entwicklung Sandbox zu veröffentlichen.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)