---
title: WNS-Benachrichtigungs Prioritäten
description: Beschreibung der verschiedenen Prioritäten, die Sie für eine Benachrichtigung festlegen können
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, WinRT-API, WNS
localizationpriority: medium
ms.openlocfilehash: 3310b34b2748bd684e46e04775c973680f8e03a9
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282239"
---
# <a name="wns-notification-priorities"></a>WNS-Benachrichtigungs Prioritäten
Durch Festlegen der Priorität einer Benachrichtigung mit einem einfachen Header auf WNS-Post-Nachrichten können Sie steuern, wie Benachrichtigungen in Akku sensiblen Situationen zugestellt werden.

## <a name="power-on-windows"></a>Einschalten von Fenstern
Da mehr Benutzer nur auf Akku Geräten arbeiten, ist das Minimieren des Energieverbrauchs zu einer Standard Anforderung für alle apps geworden. Wenn apps mehr Energie beanspruchen als der von Ihnen bereitgestellte Wert, können Benutzer die apps deinstallieren. Wenngleich das Windows-Betriebssystem die Stromversorgung für den Akku nach Möglichkeit reduziert, ist es für die APP verantwortlich, effizient zu arbeiten. 

WNS-Prioritäten sind eine Möglichkeit zum Verschieben nicht kritischer Arbeit aus dem Akku. Die WNS-Prioritäten teilen dem System mit, welche Benachrichtigungen umgehend zugestellt werden sollten und wie gewartet werden kann, bis das Gerät an eine Stromquelle angeschlossen ist. Mit diesen hinweisen kann das System die Benachrichtigungen genau so bereitstellen, wie Sie für den Benutzer und die APP am nützlichsten sind. 

## <a name="power-modes-on-the-device"></a>Energie Modi auf dem Gerät
Jedes Windows-Gerät wird über eine Vielzahl von Energie Modi betrieben (Akku, Akku Schoner und Belastung), und Benutzer erwarten verschiedene Verhaltensweisen von apps in unterschiedlichen Energie Modi. Wenn das Gerät eingeschaltet ist, sollten alle Benachrichtigungen übermittelt werden. Im Akku Spar Modus sollten nur die wichtigsten Benachrichtigungen zugestellt werden. Während das Gerät angeschlossen ist, können Synchronisierungs-oder nicht zeitkritische Vorgänge abgeschlossen werden.

Windows weiß nicht, welche Benachrichtigungen für einen Benutzer oder eine APP wichtig sind, sodass das System vollständig von apps abhängig ist, um die richtige Priorität für Ihre Benachrichtigungen festzulegen. 

## <a name="priorities"></a>Prioritäten
Es stehen vier Prioritäten für eine App zur Verfügung, die beim Senden von Pushbenachrichtigungen verwendet werden können. Die Priorität wird für einzelne Benachrichtigungen festgelegt, sodass Sie auswählen können, welche Benachrichtigungen umgehend zugestellt werden müssen (z. b. eine im-Nachricht) und welche Wartezeiten Sie warten können (z. b. mit Foto Aktualisierungen).

Die Prioritäten lauten: 

|    Priority    |    Benutzer Außerkraftsetzung    |    Beschreibung    |    Beispiel    |
|----------------|---------------------|-------------------|---------------|
|    Hoch    |    Ja – der Benutzer kann alle Benachrichtigungen von einer APP blockieren oder verhindern, dass eine APP im Akku Spar Modus gedrosselt wird.    |    Die wichtigsten Benachrichtigungen, die sofort übermittelt werden müssen, wenn das Gerät Benachrichtigungen empfangen kann. Dinge wie VoIP-Anrufe oder kritische Warnungen, die das Gerät reaktivieren sollten, fallen in diese Kategorie.    |    VoIP-Anrufe, zeitkritische Warnungen    |
|    Mittel    |    Ja – der Benutzer kann alle Benachrichtigungen von einer APP blockieren oder verhindern, dass eine APP im Akku Spar Modus gedrosselt wird.    |    Dabei handelt es sich um Dinge, die nicht so wichtig sind, aber Dinge, die nicht sofort ausgeführt werden müssen. die Benutzer sind jedoch verärgert, wenn Sie nicht im Hintergrund ausgeführt werden.    |    Sekundäre e-Mail-Konto Synchronisierung, Live-Kachel Aktualisierungen    |
|    Niedrig    |    Ja – der Benutzer kann alle Benachrichtigungen von einer APP blockieren oder verhindern, dass eine APP im Akku Spar Modus gedrosselt wird.    |    Benachrichtigungen, die nur sinnvoll sind, wenn der Benutzer das Gerät verwendet oder wenn Hintergrund Aktivität sinnvoll ist. Diese werden zwischengespeichert und erst verarbeitet, wenn sich der Benutzer anmeldet oder in seinem Gerät einfügt.    |    Kontakt Status (Online/Offline)    |
|    Sehr niedrig     |    Nein – es kann nicht verhindern, dass Benachrichtigungen mit sehr niedriger Priorität im Akku Spar Modus gedrosselt werden.    |    Dies ist fast identisch mit niedriger Priorität, außer dass Benutzer die Akku-sparrichtlinie nicht außer Kraft setzen können. Diese Benachrichtigungen werden nie in Akku Schoner zugestellt.    |    Synchronisieren von Dateien für einen Synchronisierungs Dienst.    |

Beachten Sie, dass viele apps über den gesamten Lebenszyklus hinweg Benachrichtigungen mit unterschiedlichen Prioritäten erhalten. Da die Priorität pro Benachrichtigung festgelegt wird, ist dies kein Problem. Eine VoIP-App kann eine Benachrichtigung mit hoher Priorität für einen eingehenden Rückruf senden und Sie dann mit einer niedrigen Priorität nachverfolgen, wenn ein Kontakt online geschaltet wird. 

## <a name="setting-the-priority"></a>Festlegen der Priorität

Das Festlegen der Priorität für die Benachrichtigungs Anforderung erfolgt über einen zusätzlichen Header in der Post-Anforderung, `X-WNS-PRIORITY`. Dies ist ein ganzzahliger Wert zwischen 1 und 4, der einer Priorität zugeordnet ist: 

| Prioritäts Name | X-WNS-Priority-Wert | Standardwert für: |
|---------------|----------------------|------------------|
| Hoch | 1 | Popups |
| Mittlerer | 2 | Kacheln und Abzeichen |
| Niedrig | 3 | Raw |
| Sehr niedrig | 4 |  |

Das Festlegen einer Priorität ist nicht erforderlich, um abwärts kompatibel zu sein. Für den Fall, dass eine APP die Priorität Ihrer Benachrichtigungen nicht festgelegt, wird vom System eine Standardpriorität bereitgestellt. Die Standardwerte werden im obigen Diagramm angezeigt und entsprechen dem Verhalten vorhandener Versionen von Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Ausführliche Auflistung von Desktop Verhalten 

Wenn Sie Ihre APP über viele verschiedene SKUs von Windows versenden, empfiehlt es sich in der Regel, das Diagramm im obigen Abschnitt zu befolgen. 

Spezifischere Empfohlene Verhaltensweisen für jede Priorität sind unten aufgeführt. Dies ist keine Garantie dafür, dass jedes Gerät genau entsprechend dem Diagramm funktioniert. OEMs können das Verhalten anders konfigurieren, aber die meisten sind in der Nähe dieses Diagramms. 

| Gerätestatus    | HABEN Hoch    |    HABEN Mittel        | HABEN Niedrig    |    HABEN Sehr niedrig    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Bildschirm ein-oder angeschlossen    |    Umsetzen    |    Umsetzen    |    Umsetzen    |    Umsetzen    |
|    Bildschirm aus und Akku    |    Umsetzen    |    Wenn Benutzer ausgenommen: Übermitteln von else: Batch     |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache *    |    Cache    |
|    Akku Schoner aktiviert    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Cache     |
|    Auf Akku + Akku Schoner aktiviert + Bildschirm aus    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Wenn Benutzer ausgenommen: Übermitteln von else: Cache    |    Cache    |

Beachten Sie, dass Benachrichtigungen mit niedriger Priorität standardmäßig für den Abbild Modus und Batterie nur für Windows Phone basierte Geräte übermittelt werden. Dies ist die Kompatibilität mit einer bereits vorhandenen mpns-Richtlinie. Beachten Sie auch, dass die vierte und fünfte Zeile identisch sind, indem Sie lediglich verschiedene Szenarien aufrufen.

Um eine APP im Akku Spar Modus auszumachen, müssen die Benutzer in den Einstellungen zur "Akku Nutzung nach app" wechseln und "zulassen, dass die APP Hintergrundaufgaben ausführen" auswählen. Bei Auswahl dieser Option wird die APP vom Akku Schoner für Benachrichtigungen mit hoher, mittlerer und niedriger Priorität ausgenommen. Sie können auch die [backgroundexecutionmanager-API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) anrufen, um die Berechtigung des Benutzers Programm gesteuert anzufordern.  

## <a name="related-topics"></a>Verwandte Themen
- [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md)
- [Anfordern der Berechtigung zum Ausführen im Hintergrund](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)
