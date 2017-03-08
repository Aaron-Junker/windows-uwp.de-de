---
author: jnHs
Description: "Der Nutzungsbericht im Windows Dev Center-Dashboard gibt Aufschluss darüber, wie Kunden Ihre App verwenden."
title: Nutzungsbericht
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b3c225316b028baa9a499e81841cdb939be9588e
ms.lasthandoff: 02/07/2017

---

# <a name="usage-report"></a>Nutzungsbericht


Im Bericht **Nutzung** im Windows Dev Center-Dashboard erfahren Sie, wie Kunden mit Windows 10 Ihre App verwenden, und erhalten Informationen zu den von Ihnen definierten benutzerdefinierten Ereignissen. Sie können diese Daten in Ihrem Dashboard anzeigen oder den [Bericht herunterladen](download-analytic-reports.md), um ihn offline anzuzeigen.

> **Hinweis** Zuvor wurden vom Bericht **Nutzung** nur Daten bereitgestellt, wenn Sie das Visual Studio Application Insights SDK in Ihrer App aktiviert hatten. Beim aktualisierten Bericht **Nutzung** ist dies nicht mehr erforderlich.

## <a name="apply-filters"></a>Anwenden von Filtern


Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite nach Datumsbereich und/oder Produktgruppe (zugehörigen Betriebssystemversionen) zu filtern.

-   **Datum**: Der Standardfilter lautet **Letzte 30 Tage**, aber er kann bis auf **Letzte 3 Monate** erweitert werden.
-   **Paketversion**: Die Standardeinstellung ist **Alle**. Wenn Ihre App mehr als ein Paket enthält, können Sie hier ein bestimmtes Paket auswählen.
-   **Gerätetyp**: Die Standardeinstellung ist **Alle**, Sie können jedoch festlegen, dass nur Daten für einen bestimmten Gerätetyp angezeigt werden.

Die Informationen in allen unten angezeigten Diagrammen beziehen sich auf den unter **Filter anwenden** ausgewählten Zeitraum. Standardmäßig gehören dazu Daten für alle Paketversionen und unterstützten Gerätetypen, wenn Sie nicht im Abschnitt **Filter anwenden** nach einer einzelnen Gruppe gefiltert haben.

> **Hinweis** In diesem Bericht sind nur Nutzungsdaten von Kunden mit Windows 10 enthalten.

## <a name="total-user-sessions"></a>Gesamtzahl der Benutzersitzungen

Das Diagramm **Gesamtzahl der Benutzersitzungen** zeigt die Anzahl täglicher Benutzersitzungen für die App über den ausgewählten Zeitraum an.

Jede Benutzersitzung stellt einen bestimmten Zeitraum dar, in dem ein Kunde mit der App interagiert hat. Da davon ausgegangen wird, dass jede Benutzersitzung nach einer Zeit der Inaktivität endet, kann ein einzelner Kunde am selben Tag mehrere Benutzersitzungen führen. Beachten Sie, dass in diesem Diagramm keine einzelnen App-Benutzer nachverfolgt werden.

## <a name="active-users"></a>Aktive Benutzer

Das Diagramm **Aktive Benutzer** zeigt die Anzahl der Kunden, die Ihre App im ausgewählten Zeitraum an einem bestimmten Tag verwendet haben.

Jeder aktive Benutzer steht für einen Kunden, der Ihre App an diesem Tag verwendet hat. In diesem Diagramm werden Benutzersitzungen nicht einzeln nachverfolgt (d. h., ein Kunde wird in diesem Diagramm unabhängig davon dargestellt, ob er Ihre App an diesem Tag nur einmal oder mehrmals verwendet hat).

## <a name="custom-events"></a>Benutzerdefinierte Ereignisse

Das Diagramm **Benutzerdefinierte Ereignisse** zeigt, wie häufig die für Ihre App definierten benutzerdefinierten Ereignisse insgesamt aufgetreten sind. Dazu können mehrere Vorkommen für denselben Kunden gehören.

Benutzerdefinierte Ereignisse werden unter Verwendung der [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)-Methode im [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md) implementiert.



 

