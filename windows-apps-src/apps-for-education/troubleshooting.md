---
description: Problembehandlung bei Ereignissen und Fehlern der Microsoft-Prüfung mithilfe der Ereignisanzeige.
title: Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: Windows 10, UWP, Bildung
ms.localizationpriority: medium
ms.openlocfilehash: cd30d54f1bff5fd43fbeb6e286e327fed9f8a585
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031483"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige.

Sie können die Ereignisanzeige nutzen um sich Ereignisse und Fehler der Prüfung anzeigen zu lassen. Prüfung protokolliert Ereignisse, wenn eine Sperrmodus-Anforderung empfangen wurde, wenn eine Geräteregistrierung erfolgreich war, die Sperrmodusrichtlinien erfolgreich angewendet wurden und vieles mehr.

So aktivieren Sie Ereignisse in der Ereignisanzeige
1. Öffnen Sie das `Event Viewer`
2. Navigieren Sie zu `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`.
3. Klicken Sie mit der rechten Maustaste auf `Operational``Enable Log`

So speichern Sie die Ereignisprotokolle
1. Mit der rechten Maustaste klicken `Operational`
2. Klicken Sie auf `Save All Events As…`.
