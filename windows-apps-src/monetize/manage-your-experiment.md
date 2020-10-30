---
description: Nachdem Sie Ihr Experiment in Partner Center definiert und ihr Experiment in ihrer App codiert haben, können Sie Ihr Experiment aktivieren und mithilfe von Partner Center die Ergebnisse des Experiments überprüfen.
title: Verwalten eines Experiments im Partner Center
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030553"
---
# <a name="manage-your-experiment-in-partner-center"></a>Verwalten eines Experiments im Partner Center

Nachdem Sie [Ihr Experiment in Partner Center definiert](define-your-experiment-in-the-dev-center-dashboard.md) und [Ihre APP für Experimente codiert](code-your-experiment-in-your-app.md)haben, können Sie Ihr Experiment aktivieren und mithilfe von Partner Center die Ergebnisse des Experiments überprüfen. Nach Abrufen aller benötigten Daten können Sie das Experiment beenden und festlegen, ob die Variablenwerte in der Steuerungsvariation für alle Apps weiter verwendet werden sollen oder die Variablenwerte einer anderen Variation verwendet werden sollen.

> [!NOTE]
> Wenn Sie ein Experiment aktivieren, beginnt Partner Center sofort mit der Erfassung von Daten aus allen apps, die zum Protokollieren von Daten für Ihr Experiment instrumentiert werden. Allerdings kann es mehrere Stunden dauern, bis Experimentdaten im Partner Center angezeigt werden.

Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Aktivieren Ihres Experiments

Wenn Sie mit den Parametern Ihres Experiments in Partner Center zufrieden sind und Ihren app-Code aktualisiert haben, können Sie das Experiment aktivieren, damit Sie mit der Erfassung von Experimentdaten aus Ihrer APP beginnen können. Wenn das Experiment aktiv ist, kann Ihre APP Variations Werte und Berichtsansicht-und Konvertierungs Ereignisse an Partner Center abrufen.

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard) an.
2. Wählen Sie unter **Ihre Apps** die App mit dem Experiment, das Sie aktivieren möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimentation** aus.
4. Erweitern Sie in der Tabelle der Projekte im Abschnitt **Projekte** das Projekt, das Ihr Experiment enthält, und führen Sie dann eine der folgenden Aufgaben aus:
  * Klicken Sie auf den Link **Aktivieren** für Ihr Experiment. Ihr Experiment wird dem Abschnitt **Aktive Experimente** im oberen Bereich der Seite hinzugefügt.
  * Klicken Sie auf den Namen des Experiments, führen Sie auf der Seite für Experimente einen Bildlauf nach unten aus, und klicken Sie auf **Aktivieren** .

> [!IMPORTANT]
> Nachdem Sie ein Experiment aktiviert haben, können Sie die Experiment Parameter nicht mehr ändern, es sei denn, Sie haben beim Erstellen des Experiments auf das Kontrollkästchen " **bearbeitbarer Versuch** " geklickt. Es wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

## <a name="review-the-results-of-your-experiment"></a>Prüfen der Experimentergebnisse

1. Kehren Sie in Partner Center zur Seite **experimentieren** für Ihre APP zurück.
2. Klicken Sie im Abschnitt **Aktive Experimente** auf den Namen des aktiven Experiments, um zur Experimentseite zu wechseln.
3. Bei aktiven oder beendeten Experimenten enthalten die ersten zwei Abschnitte auf dieser Seite die Ergebnisse Ihres Experiments:
  * Der Abschnitt **Ergebniszusammenfassung** enthält die Experimentziele und die Umwandlungsquote für jede Variation.
  * Der Abschnitt **Ergebnisdetails** enthält ausführlichere Informationen zu den einzelnen Varianten all der Ziele im Experiment, einschließlich Ansichten, Konvertierungen, eindeutiger Benutzer, Umwandlungsquote, Delta in %, Konfidenz und Signifikanz. Die *Konfidenz* ist ein statistisches Maß zum Angeben der Zuverlässigkeit einer Schätzung, mit dem die Fehlerspanne ermittelt wird. Die *Signifikanz* ist ein statistisches Maß, mit dem basierend auf einer Stichprobe die Wahrscheinlichkeit dafür bestimmt wird, dass ein Ergebnis nicht zufällig ist, sondern auf eine bestimmte Ursache zurückzuführen ist.

> [!NOTE]
> Partner Center meldet nur das erste Konvertierungs Ereignis für jeden Benutzer innerhalb eines Zeitraums von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.


## <a name="complete-your-experiment"></a>Beenden Ihres Experiments

1. Kehren Sie in Partner Center zu ihrer Experiment Seite zurück. Einzelheiten dazu finden Sie im vorherigen Abschnitt.
2. Führen Sie im Abschnitt **Ergebniszusammenfassung** eine der folgenden Aktionen durch:
  * Klicken Sie zum Beenden des Experiments und zum weiteren Verwenden der Variablenwerte der Steuerungsvariation für Ihre App auf **Beibehalten** .
  * Klicken Sie zum Beenden des Experiments und zum Verwenden der Variablenwerte einer anderen Variation für Ihre App unter der Variation, zu der Sie wechseln möchten, auf **Wechseln** .
3. Klicken Sie auf **OK** , um zu bestätigen, dass Sie das Experiment beenden möchten.


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen eines Projekts und Definieren von Remote Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md)
* [Definieren eines Experiments im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)
