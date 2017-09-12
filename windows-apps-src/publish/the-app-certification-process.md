---
author: jnHs
Description: "Nachdem Sie die App-Einreichung fertig gestellt haben und auf „An Store übermitteln” klicken, tritt die App in die Zertifizierungsphase ein."
title: Der App-Zertifizierungsprozess
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 04/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: f075dc6e112a526d7aee71f0f9aa7d4da570d649
ms.sourcegitcommit: cd9b4bdc9c3a0b537a6e910a15df8541b49abf9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2017
---
# <a name="the-app-certification-process"></a>Der App-Zertifizierungsprozess

Nachdem Sie die App-Einreichung fertig gestellt haben und auf **An Store übermitteln** klicken, tritt die App in die Zertifizierungsphase ein. Dieser Vorgang ist in der Regel innerhalb weniger Stunden abgeschlossen, kann in Einzelfällen aber bis zu drei Arbeitstage dauern. Nach der Zertifizierung der Übermittlung kann es bis zu 24Stunden dauern, bis der App-Eintrag (oder ein Update für eine bereits veröffentlichte App) für Kunden im Store sichtbar wird. Eine Benachrichtigung wird angezeigt, wenn Ihre Übermittlung veröffentlicht wurde und für Kunden verfügbar ist, und der App-Status im Dashboard lautet **Im Store**.

## <a name="preprocessing"></a>Vorverarbeitung

Nach dem erfolgreichen Hochladen der App-Pakete und dem Übermitteln der App zur Zertifizierung werden die Pakete zum Testen in die Warteschlange eingereiht. Es wird eine Meldung angezeigt, wenn während der Vorverarbeitung Fehler erkannt werden. Weitere Informationen zu möglichen Fehlern finden Sie unter [Fehler bei der Einreichung](resolve-submission-errors.md).

## <a name="certification"></a>Zertifizierung

Während dieser Phase werden mehrere Tests durchgeführt:

-   **Sicherheitstests:** Dieser erste Test überprüft die Pakete Ihrer App auf Viren und Schadsoftware. Besteht Ihre App diesen Test nicht, müssen Sie Ihr Entwicklungssystem überprüfen, indem Sie die aktuelle Antivirensoftware ausführen und Ihr App-Paket anschließend in einem bereinigten System neu erstellen.
-   **Tests bezgl. der Erfüllung technischer Anforderungen:** Die Erfüllung technischer Anforderungen wird mithilfe des Zertifizierungskits für Windows-Apps getestet. (Achten Sie immer darauf, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](../debug-test-perf/windows-app-certification-kit.md), bevor Sie sie im Store einreichen.)
-   **Erfüllung inhaltlicher Anforderungen:** Die dafür benötigte Zeit hängt von der Komplexität Ihrer App, der Menge von visuellem Inhalt sowie der Anzahl von kürzlich übermittelten Apps ab. Achten Sie darauf, auf der Seite [Hinweise für Zertifizierung](notes-for-certification.md) alle für Tester erforderlichen Informationen bereitzustellen.

Nach Abschluss des Zertifizierungsprozesses erhalten Sie einen Zertifizierungsbericht, der Aufschluss darüber gibt, ob Ihre App die Zertifizierung bestanden hat. War die Zertifizierung nicht erfolgreich, ist im Bericht angegeben, welcher Test nicht bestanden bzw. welche [Richtlinie](https://msdn.microsoft.com/library/windows/apps/dn764944) nicht erfüllt wurde. Nachdem Sie das Problem behoben haben, können Sie eine neue Einreichung für Ihre App erstellen, um den Zertifizierungsprozess erneut einzuleiten.

## <a name="release"></a>Version

Nachdem Ihre App zertifiziert wurde, kann sie in den **Veröffentlichungsprozess** eintreten. Wenn Sie angegeben haben, dass Ihre Übermittlung so schnell wie möglich veröffentlicht werden soll, geschieht dies sofort. Wenn Sie angegeben haben, dass sie erst zu einem bestimmten Datum veröffentlicht werden soll, warten wir bis zu diesem Datum, es sei denn, Sie klicken auf den Link **Veröffentlichungsdatum ändern**. Wenn Sie angegeben haben, dass Sie die Übermittlung manuell veröffentlichen möchten, wird sie erst veröffentlicht, wenn Sie dies ausdrücklich angeben, indem Sie auf die Schaltfläche **Jetzt veröffentlichen** klicken, oder wenn Sie auf den Link **Veröffentlichungsdatum ändern** klicken und ein bestimmtes Datum auswählen.

## <a name="publishing"></a>Veröffentlichung

Die Pakete Ihrer App werden digital signiert, damit sie nach ihrer Veröffentlichung nicht manipuliert werden können. Nach Beginn dieser Phase ist ein Abbruch der Einreichung oder eine Änderung des Veröffentlichungsdatums nicht mehr möglich.

Während sich Ihre App in der Veröffentlichungsphase befindet, erfahren Sie über den Link **Details anzeigen** in der Statusspalte für die App-Übermittlung, wann Ihre neuen Pakete und die Details des Store-Eintrags auf jedem der unterstützten Betriebssystemversionen für Kunden zur Verfügung stehen. Ihre App bleibt in der Veröffentlichungsphase, bis die neuen Pakete und Details des Eintrags für alle potenziellen Kunden der App verfügbar sind. Dies kann bis zu 24Stunden dauern. 

## <a name="in-the-store"></a>Im Store 

Nach erfolgreicher Absolvierung der obigen Schritte ändert sich der Status der Übermittlung von **Veröffentlichung** in **Im Store**. Ihre Übermittlung steht den Kunden nun im Windows Store zum Download zur Verfügung (sofern Sie unter [Sichtbarkeit](set-app-pricing-and-availability.md#visibility) keine andere Option ausgewählt haben). 

**Hinweis**  Außerdem führen wir Stichprobenkontrollen für bereits veröffentlichte Apps durch, um potenzielle Probleme zu ermitteln und sicherzustellen, dass Ihre App alle [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944) erfüllt. Falls Probleme gefunden werden, werden Sie benachrichtigt, dass ein Problem aufgetreten ist und wie Sie es ggf. beheben können, oder dass die App aus dem Store entfernt wurde.

 

 

 




