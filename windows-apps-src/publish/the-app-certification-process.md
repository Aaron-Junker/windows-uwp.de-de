---
Description: Wenn Sie das Erstellen der APP-Übermittlung abgeschlossen haben und auf an den Store senden klicken, wird die Übermittlung in den Zertifizierungs Schritt eingegeben.
title: Der App-Zertifizierungsprozess
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Veröffentlichung, Vorverarbeitung, Zertifizierung, Freigabe, Ausstehend, übermitteln, veröffentlichen, Status, Zeit
ms.localizationpriority: medium
ms.openlocfilehash: d88d8deeb467f186f120fb8c1e579d5c9222aaf1
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210956"
---
# <a name="the-app-certification-process"></a>Der App-Zertifizierungsprozess

Nachdem Sie die App-Einreichung fertig gestellt haben und auf **An Store übermitteln** klicken, tritt die Übermittlung in die Zertifizierungsphase ein. Dieser Vorgang ist in der Regel innerhalb weniger Stunden abgeschlossen, kann in Einzelfällen aber bis zu drei Arbeitstage dauern. Nachdem die Übermittlung der Zertifizierung weitergeleitet wurde, kann es bis zu 24 Stunden dauern, bis Kunden das Auflisten der APP für eine neue Übermittlung oder eine aktualisierte Übermittlung mit Änderungen an den Paketen sehen. Wenn das Update nur die Details der Liste speichert, wird der Veröffentlichungsprozess in weniger als einer Stunde abgeschlossen.  Sie werden benachrichtigt, wenn ihre Übermittlung veröffentlicht wird, und der Status der APP im Dashboard befindet sich **im Store**.

## <a name="preprocessing"></a>Vorverarbeitung

Nach dem erfolgreichen Hochladen der App-Pakete und dem Übermitteln der App zur Zertifizierung werden die Pakete zum Testen in die Warteschlange eingereiht. Es wird eine Meldung angezeigt, wenn während der Vorverarbeitung Fehler erkannt werden. Weitere Informationen zu möglichen Fehlern finden Sie unter [Fehler bei der Einreichung](resolve-submission-errors.md).

## <a name="certification"></a>Zertifizierung

Während dieser Phase werden mehrere Tests durchgeführt:

-   **Sicherheitstests:** Dieser erste Test überprüft die Pakete Ihrer App auf Viren und Schadsoftware. Besteht Ihre App diesen Test nicht, müssen Sie Ihr Entwicklungssystem überprüfen, indem Sie die aktuelle Antivirensoftware ausführen und Ihr App-Paket anschließend in einem bereinigten System neu erstellen.
-   **Tests bezgl. der Erfüllung technischer Anforderungen:** Die Erfüllung technischer Anforderungen wird mithilfe des Zertifizierungskits für Windows-Apps getestet. (Achten Sie immer darauf, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](../debug-test-perf/windows-app-certification-kit.md), bevor Sie sie im Store einreichen.)
-   **Erfüllung inhaltlicher Anforderungen:** Die dafür benötigte Zeit hängt von der Komplexität Ihrer App, der Menge an visuellem Inhalt sowie der Anzahl von kürzlich übermittelten Apps ab. Achten Sie darauf, auf der Seite [Hinweise für Zertifizierung](notes-for-certification.md) alle für Tester erforderlichen Informationen bereitzustellen.

Nach Abschluss des Zertifizierungsprozesses erhalten Sie einen Zertifizierungsbericht, der Aufschluss darüber gibt, ob Ihre App die Zertifizierung bestanden hat. War die Zertifizierung nicht erfolgreich, ist im Bericht angegeben, welcher Test nicht bestanden bzw. welche [Richtlinie](store-policies.md) nicht erfüllt wurde. Nachdem Sie das Problem behoben haben, können Sie eine neue Einreichung für Ihre App erstellen, um den Zertifizierungsprozess erneut einzuleiten.

## <a name="release"></a>Version

Wenn Ihre APP die Zertifizierung übergibt, ist Sie zum **Veröffentlichungs** Prozess bereit.

- Wenn Sie angegeben haben, dass ihre Übermittlung so bald wie möglich veröffentlicht werden soll (Standardoption), beginnt der Veröffentlichungsprozess sofort.
- Wenn Sie die APP zum ersten Mal veröffentlicht haben und im Abschnitt " [Zeitplan](configure-precise-release-scheduling.md#release) " ein **Veröffentlichungsdatum** angegeben haben, wird die APP gemäß ihrer **Veröffentlichungsdatums** Auswahl verfügbar.
- Wenn Sie die [Option zum Veröffentlichen von Optionen](manage-submission-options.md#publishing-hold-options) verwendet haben, um anzugeben, dass Sie bis zu einem bestimmten Datum nicht freigegeben werden soll, warten wir bis zum Beginn des Veröffentlichungs Vorgangs, es sei denn, Sie wählen das **Veröffentlichungsdatum ändern**aus.
- Wenn Sie die Option zum Veröffentlichen von [Optionen](manage-submission-options.md#publishing-hold-options) verwendet haben, um anzugeben, dass Sie die Übermittlung manuell veröffentlichen möchten, wird der Veröffentlichungsprozess erst gestartet, wenn Sie **Jetzt veröffentlichen** auswählen (oder wählen Sie **Veröffentlichungsdatum ändern** und ein bestimmtes Datum auswählen).


## <a name="publishing"></a>Publishing

Die Pakete Ihrer App werden digital signiert, damit sie nach ihrer Veröffentlichung nicht manipuliert werden können. Nach Beginn dieser Phase ist ein Abbruch der Einreichung oder eine Änderung des Veröffentlichungsdatums nicht mehr möglich.

Bei neuen apps und Updates, die Änderungen an den Paketen der APP einschließen, wird der Veröffentlichungs Vorgang innerhalb von 24 Stunden abgeschlossen. Für Updates, die nur Optionen wie Details zum Store auflisten ändern, aber nicht die Pakete der App ändern, dauert der Veröffentlichungs Vorgang weniger als eine Stunde.

Während Ihre APP in der Veröffentlichungs Phase enthalten ist, können Sie den Link **Details anzeigen** in der Spalte Status für die Übermittlung Ihrer APP informieren, wenn Ihre neuen Pakete und Store-Listen Details für Kunden auf jeder der unterstützten Betriebssystemversionen verfügbar sind. Schritte, die noch nicht abgeschlossenen wurden, werden als **Ausstehend** angezeigt. Ihre APP bleibt in der Veröffentlichungs Phase, bis der Prozess abgeschlossen ist. das bedeutet, dass die neuen Pakete und/oder Auflistungs Details allen potenziellen Kunden Ihrer APP zur Verfügung stehen.

## <a name="in-the-store"></a>Im Store 

Nach erfolgreicher Absolvierung der obigen Schritte ändert sich der Status der Übermittlung von **Veröffentlichung** in **Im Store**. Ihre Übermittlung steht den Kunden nun im Microsoft Store zum Download zur Verfügung (sofern Sie unter [Erkennbarkeit](choose-visibility-options.md#discoverability) keine andere Option ausgewählt haben). 

> [!NOTE]
> Außerdem führen wir Stichprobenkontrollen für bereits veröffentlichte Apps durch, um potenzielle Probleme zu ermitteln und sicherzustellen, dass Ihre App alle [Microsoft Store-Richtlinien](store-policies.md) erfüllt. Falls Probleme gefunden werden, werden Sie benachrichtigt, dass ein Problem aufgetreten ist und wie Sie es ggf. beheben können, oder dass die App aus dem Store entfernt wurde.

 

 

 




