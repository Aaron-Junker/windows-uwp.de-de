---
Description: Problembehandlung bei Ereignissen und Fehlern der Microsoft-Prüfung mithilfe der Ereignisanzeige.
title: Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: Windows 10, Uwp, Bildung
ms.localizationpriority: medium
ms.openlocfilehash: 2f4bdcf45c7dd37dd540a666d99b5fa2fd2d49f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598475"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige.

Sie können die Ereignisanzeige nutzen um sich Ereignisse und Fehler der Prüfung anzeigen zu lassen. Prüfung protokolliert Ereignisse, wenn eine Sperrmodus-Anforderung empfangen wurde, wenn eine Geräteregistrierung erfolgreich war, die Sperrmodusrichtlinien erfolgreich angewendet wurden und vieles mehr.

So aktivieren Sie Ereignisse in der Ereignisanzeige
1. Öffnen der `Event Viewer`
2. Navigieren Sie zu `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. Mit der rechten Maustaste `Operational` , und wählen Sie `Enable Log`

So speichern Sie die Ereignisprotokolle
1. Mit der rechten Maustaste `Operational`
2. Klicken Sie auf `Save All Events As…`
