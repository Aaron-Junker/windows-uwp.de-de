---
Description: Wenn Sie Fertigstellen Ihrer app senden, und klicken Sie auf die an den Store übermitteln, gibt die Übermittlung der zertifizierungsschritt an.
title: Der App-Zertifizierungsprozess
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, veröffentlichen, vorverarbeitung, Zertifizierung, freigeben, Ausstehend, übermitteln, zu veröffentlichen, Status, Zeit
ms.localizationpriority: medium
ms.openlocfilehash: d88d8deeb467f186f120fb8c1e579d5c9222aaf1
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826228"
---
# <a name="the-app-certification-process"></a>Der App-Zertifizierungsprozess

Nachdem Sie die App-Einreichung fertig gestellt haben und auf **An Store übermitteln** klicken, tritt die Übermittlung in die Zertifizierungsphase ein. Dieser Vorgang ist in der Regel innerhalb weniger Stunden abgeschlossen, kann in Einzelfällen aber bis zu drei Arbeitstage dauern. Nachdem Ihre Einsendung Zertifizierung erfolgreich ist, dauert es bis zu 24 Stunden für Kunden, die app Liste für eine neue Eingabe oder für eine aktualisierte Übermittlung mit Änderungen an der Pakete angezeigt. Wenn das Update nur Store, die Details von ändert, wird der Veröffentlichungsprozess in weniger als einer Stunde abgeschlossen werden.  Sie werden benachrichtigt, sobald die Übermittlung wird veröffentlicht und den app Status im Dashboard werden **In der Store**.

## <a name="preprocessing"></a>Vorverarbeitung

Nach dem erfolgreichen Hochladen der App-Pakete und dem Übermitteln der App zur Zertifizierung werden die Pakete zum Testen in die Warteschlange eingereiht. Es wird eine Meldung angezeigt, wenn während der Vorverarbeitung Fehler erkannt werden. Weitere Informationen zu möglichen Fehlern finden Sie unter [Fehler bei der Einreichung](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Während dieser Phase werden mehrere Tests durchgeführt:

-   **Sicherheitstests:** Dieser erste Test überprüft Ihrer app-Pakete für den Viren und Schadsoftware. Besteht Ihre App diesen Test nicht, müssen Sie Ihr Entwicklungssystem überprüfen, indem Sie die aktuelle Antivirensoftware ausführen und Ihr App-Paket anschließend in einem bereinigten System neu erstellen.
-   **Technische Einhaltung der Standards Tests:** Technische Einhaltung der Standards wird von der Windows-Zertifizierungskit für Apps getestet. (Achten Sie immer darauf, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](../debug-test-perf/windows-app-certification-kit.md), bevor Sie sie im Store einreichen.)
-   **Inhaltliche Einhaltung der Standards:** Die Zeitspanne, während dieses Vorgangs variiert je nach Komplexität Ihrer app ist, wie viele visuellen Inhalt hat und wie viele apps vor kurzem übermittelt wurden. Achten Sie darauf, auf der Seite [Hinweise für Zertifizierung](notes-for-certification.md) alle für Tester erforderlichen Informationen bereitzustellen.

Nach Abschluss des Zertifizierungsprozesses erhalten Sie einen Zertifizierungsbericht, der Aufschluss darüber gibt, ob Ihre App die Zertifizierung bestanden hat. War die Zertifizierung nicht erfolgreich, ist im Bericht angegeben, welcher Test nicht bestanden bzw. welche [Richtlinie](store-policies.md) nicht erfüllt wurde. Nachdem Sie das Problem behoben haben, können Sie eine neue Einreichung für Ihre App erstellen, um den Zertifizierungsprozess erneut einzuleiten.

## <a name="release"></a>Version

Wenn Ihre app, Zertifizierung durchläuft, es ist bereit für die **Publishing** Prozess.

- Wenn Sie angegeben haben, dass es sich bei Ihrer Übermittlung so bald wie möglich (die Standardoption) veröffentlicht werden sollen, werden sofort der Veröffentlichung zu beginnen.
- Wenn das erste Mal haben Sie die app veröffentlicht, und Sie angegeben ein **Veröffentlichungsdatum** in die [Zeitplan](configure-precise-release-scheduling.md#release) Abschnitt die app zur Verfügung stehen nach Ihrer **Veröffentlichungsdatum**Auswahl.
- Wenn Sie verwendet haben [Veröffentlichung enthalten Optionen](manage-submission-options.md#publishing-hold-options) festlegen, dass es bis zu einem bestimmten Datum nicht freigegeben werden sollen, wir warten bis zum Zeitpunkt, um den Veröffentlichungsprozess zu beginnen, oder wählen Sie **Änderung Veröffentlichungsdatum**.
- Wenn Sie verwendet haben [Veröffentlichung enthalten Optionen](manage-submission-options.md#publishing-hold-options) festlegen, dass die Übermittlung manuell veröffentlichen möchten, es wird nicht erst gestartet, den Veröffentlichungsprozess Auswahl **jetzt veröffentlichen** (oder wählen Sie **ändern Veröffentlichungsdatum** , und wählen Sie ein bestimmtes Datum).


## <a name="publishing"></a>Publishing

Die Pakete Ihrer App werden digital signiert, damit sie nach ihrer Veröffentlichung nicht manipuliert werden können. Nach Beginn dieser Phase ist ein Abbruch der Einreichung oder eine Änderung des Veröffentlichungsdatums nicht mehr möglich.

Für neue apps und Updates, bei die Änderungen an der app-Pakete umfassen, wird der Veröffentlichungsprozess innerhalb von 24 Stunden abgeschlossen werden. Für Updates, die nur Optionen wie z. B. Store Auflisten von Details, aber nicht ändern der app-Pakete, dauert der Veröffentlichungsprozess weniger als einer Stunde.

Während Ihre app in der Phase beim Veröffentlichen, ist die **Details anzeigen** -link in der Spalte Status für die Übermittlung von Ihrer app können Sie die, wenn die neuen Pakete und die Store Auflisten von Informationen für Kunden, die für jedes von Ihr unterstützten Betriebssystem verfügbar sind Versionen. Schritte, die noch nicht abgeschlossenen wurden, werden als **Ausstehend** angezeigt. Ihre app bleibt in der Phase beim Veröffentlichen, bis der Vorgang abgeschlossen wurde, was bedeutet, dass die neuen Pakete und/oder Auflisten von Details für alle potenziellen Kunden Ihrer app zur Verfügung.

## <a name="in-the-store"></a>Im Store 

Nach erfolgreicher Absolvierung der obigen Schritte ändert sich der Status der Übermittlung von **Veröffentlichung** in **Im Store**. Ihre Übermittlung steht den Kunden nun im Microsoft Store zum Download zur Verfügung (sofern Sie unter [Erkennbarkeit](choose-visibility-options.md#discoverability) keine andere Option ausgewählt haben). 

> [!NOTE]
> Außerdem führen wir Stichprobenkontrollen für bereits veröffentlichte Apps durch, um potenzielle Probleme zu ermitteln und sicherzustellen, dass Ihre App alle [Microsoft Store-Richtlinien](store-policies.md) erfüllt. Falls Probleme gefunden werden, werden Sie benachrichtigt, dass ein Problem aufgetreten ist und wie Sie es ggf. beheben können, oder dass die App aus dem Store entfernt wurde.

 

 

 




