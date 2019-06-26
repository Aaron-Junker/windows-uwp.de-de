---
title: WNS-Benachrichtigung-Prioritäten
description: Beschreibung der verschiedenen Prioritäten, die Sie für eine Benachrichtigung festlegen können
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, Uwp, WinRT-API, WNS
localizationpriority: medium
ms.openlocfilehash: f5c4b9f1db58a091dc4f9389888ad3739c4439e5
ms.sourcegitcommit: b0edd3c09f931b9b62f9c2d17037fb58d826174f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2019
ms.locfileid: "67349868"
---
# <a name="wns-notification-priorities"></a>WNS-Benachrichtigung-Prioritäten
Wenn eine Benachrichtigung, die Priorität mit einem einfachen-Header auf WNS beitragsnachrichten festlegen, können Sie steuern, wie Benachrichtigungen in Akku vertrauliche Situationen übermittelt werden.

## <a name="power-on-windows"></a>Schalten Sie Windows
Mehr Benutzer nur auf Akkubetrieb Geräten arbeiten, ist nach der Stromverbrauch minimiert ist eine standard-Voraussetzung für alle apps geworden. Wenn apps mehr Energie als der Wert, die sie bereitstellen verwendet, können Benutzer die apps deinstallieren. Während des Windows-Betriebssystems Energieverbrauch auf den Akku möglichst reduziert, ist es der Zuständigkeit der app an effizient arbeiten. 

WNS-Prioritäten ist eine Möglichkeit zum Verschieben der Akku nicht kritische Arbeit. Die Prioritäten WNS Teilen Sie das System die Benachrichtigungen sofort gesendet werden soll, und die darauf warten, bis das Gerät an eine Stromquelle angeschlossen ist. Mit diesen Hinweisen kann das System die genaue Zeit, die sie die Benutzer und die app die wertvollsten sind Benachrichtigungen bereitstellen. 

## <a name="power-modes-on-the-device"></a>Energiestatus auf dem Gerät
Jedes Windows-Gerät ausgeführt wird, über eine Vielzahl von Energiestatus (Akku, Stromsparmodus und Kosten), und Benutzer erwarten, dass unterschiedliche Verhaltensweisen von apps in anderen Modi. Wenn das Gerät aktiviert ist, sollten alle Benachrichtigungen übermittelt werden. In Stromsparmodus sollten nur die wichtigsten Benachrichtigungen übermittelt werden. Während das Gerät im Netzbetrieb befindet, können Sync oder nicht-Time-kritische Vorgänge ausgeführt werden.

Windows weiß nicht, welche Benachrichtigungen für alle Benutzer oder eine app, wichtig sind, damit das System vollständig apps zum Festlegen der richtigen Priorität für die Benachrichtigungen verwendet. 

## <a name="priorities"></a>Prioritäten
Es gibt vier Prioritäten für eine app verwendet beim Senden von Pushbenachrichtigungen zur Verfügung. Die Priorität für einzelne Benachrichtigungen, können Sie auswählen, welche Benachrichtigungen sofort übermittelt werden müssen (z. B. eine IM-Nachricht), und warten können, welche festgelegt ist (z. B. Wenden Sie sich an Foto-Updates).

Die Prioritäten liegen auf: 

|    Priority    |    Außerkraftsetzung durch Benutzer    |    Beschreibung    |    Beispiel    |
|----------------|---------------------|-------------------|---------------|
|    Hoch    |    Ja – Benutzer kann alle Benachrichtigungen von einer app blockiert, oder kann verhindern, dass eine app in Stromsparmodus gedrosselt.    |    Die wichtigsten Benachrichtigungen, die in jedem Fall sofort zugestellt werden müssen, wenn das Gerät Benachrichtigungen empfangen kann. Dinge wie VoIP-Anrufe oder kritische Warnungen, die das Gerät zu reaktivieren soll fallen in diese Kategorie.    |    VoIP-Anrufe, Zeit: kritische Warnungen    |
|    Mittel    |    Ja – Benutzer kann alle Benachrichtigungen von einer app blockiert, oder kann verhindern, dass eine app in Stromsparmodus gedrosselt.    |    Hierbei handelt es sich um Dinge, die nicht als wichtige, Punkte, die nicht sofort erfolgen müssen, aber Benutzer würde nicht mögen werden, wenn sie nicht im Hintergrund ausgeführt werden.    |    Sekundären e-Mail-Konto-Synchronisierung aktualisiert live-Kachel.    |
|    Niedrig    |    Ja – Benutzer kann alle Benachrichtigungen von einer app blockiert, oder kann verhindern, dass eine app in Stromsparmodus gedrosselt.    |    Benachrichtigungen, die nur sinnvoll, wenn der Benutzer das Gerät verwendet oder Hintergrundaktivitäten sinnvoll ist. Zwischengespeichert Sie werden und nicht verarbeitet, bis der Benutzer anmeldet oder das Plug-in in ihr Gerät.    |    Wenden Sie sich an Status (online/offline)    |
|    Sehr niedrig     |    Nein – nicht es verhindern, dass Benachrichtigungen mit sehr niedriger Priorität in Stromsparmodus gedrosselt.    |    Dies ist fast identisch, da es sich bei die Akku Bildschirmschoner-Richtlinie mit niedriger Priorität, mit Ausnahme von Benutzern nicht übersteuert werden kann. Diese Benachrichtigungen werden nie in Stromsparmodus übermittelt werden.    |    Synchronisieren von Dateien für einen Synchronisierungsdienst.    |

Beachten Sie, dass viele apps Benachrichtigungen mit unterschiedlichen Prioritäten während des gesamten Lebenszyklus. Da die Priorität auf einer pro-Notification-Basis festgelegt ist, ist dies kein Problem. Eine VoIP-app kann eine hohe Priorität-Benachrichtigung für einen eingehenden Aufruf senden und anschließend die entsprechende es mit niedriger Priorität eine Wenn ein Kontakt online geschaltet wird. 

## <a name="setting-the-priority"></a>Festlegen der Prioritäten

Festlegen der Prioritäten für die benachrichtigungsanforderung erfolgt über einen zusätzlichen Header auf die POST-Anforderung `X-WNS-PRIORITY`. Dies ist eine ganze Zahl zwischen 1 und 4 der eine Priorität zugeordnet: 

| Prioritätenname | X-WNS-PRIORITY-Wert | Standardwert für: |
|---------------|----------------------|------------------|
| Hoch | 1 | Popups |
| Meduim | 2 | Kacheln und Signale |
| Niedrig | 3 | Raw |
| Sehr niedrig | 4 |  |

Um Abwärtskompatibilität zu sein ist kompatibel, das eine Priorität nicht erforderlich. Für den Fall, dass eine app nicht die Priorität der ihre Benachrichtigungen festgelegt, wird das System eine Standardpriorität bereitstellen. Die Standardwerte werden im Diagramm oben angezeigt und entspricht das Verhalten der vorhandenen Versionen von Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Ausführliche Liste der desktop-Verhalten 

Wenn Sie Ihre app über viele verschiedene SKUs von Windows liefern, empfiehlt es sich normalerweise auf das Diagramm im vorherigen Abschnitt folgen. 

Spezifischere empfohlene Verhalten für die einzelnen Prioritäten sind unten aufgeführt. Dies ist keine Garantie dafür, die jedes Gerät genau entsprechend dem Diagramm funktionieren. OEMs können das Verhalten anders konfigurieren, aber die meisten sind in der Nähe dieses Diagramm. 

| Gerätestatus    | PRIORITÄT: Hoch    |    PRIORITÄT: Mittel        | PRIORITÄT: Niedrig    |    PRIORITÄT: Sehr niedrig    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Bildschirm für oder im Netzbetrieb befindet    |    Übermitteln    |    Übermitteln    |    Übermitteln    |    Übermitteln    |
|    Bildschirm ausschalten und Akkubetrieb    |    Übermitteln    |    Wenn Benutzer ausgenommen: übermitteln Else: Batch     |    Wenn Benutzer ausgenommen: übermitteln Else: Cache *    |    Cache    |
|    Stromsparmodus aktiviert    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Cache     |
|    Akku + Stromsparmodus aktiviert und Bildschirm deaktiviert    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Wenn Benutzer ausgenommen: übermitteln Else: Cache    |    Cache    |

Beachten Sie, die mit niedriger Priorität Benachrichtigungen für den Bildschirm standardmäßig off bereitgestellt werden und Akku nur für Windows Phone-basierten Geräten. Dies ist zum Maintian der Kompatibilität mit vorhandenen MPNS-Richtlinie. Beachten Sie außerdem, dass die vierten und fünften Zeilen identisch, nur verschiedene Szenarien aufgerufen werden.

Um eine app in Stromsparmodus ausschließen möchten, müssen die Benutzer finden Sie unter der "Akku Nutzung von App" in den Einstellungen und wählen Sie "Zulassen" der app"zum Ausführen von Aufgaben im Hintergrund". Diese Benutzerauswahl nimmt die app aus dem Stromsparmodus für hoch, Mittel und niedriger Priorität Benachrichtigungen aus. Sie können auch aufrufen [BackgroundExecutionManager API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) programmgesteuert für die Berechtigung des Benutzers zu bitten.  

## <a name="related-topics"></a>Verwandte Themen
- [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md)
- [Berechtigung zum Ausführen im Hintergrund ausgeführt werden](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
