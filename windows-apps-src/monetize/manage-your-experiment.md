---
Description: Nachdem Sie das Experiment im Partner Center zu definieren und code Ihres Experiments in Ihrer app, Sie sind bereit für die aktive Ihres Experiments und Partner Center zu verwenden, um die Ergebnisse Ihres Experiments zu überprüfen.
title: Verwalten eines Experiments im Partner Center
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594925"
---
# <a name="manage-your-experiment-in-partner-center"></a>Verwalten eines Experiments im Partner Center

Nachdem Sie [definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md) und [code Ihrer app experimentieren](code-your-experiment-in-your-app.md), Sie können das Experiment zu aktivieren und Partner Center zu verwenden, um die Ergebnisse Ihres Experiments zu überprüfen. Nach Abrufen aller benötigten Daten können Sie das Experiment beenden und festlegen, ob die Variablenwerte in der Steuerungsvariation für alle Apps weiter verwendet werden sollen oder die Variablenwerte einer anderen Variation verwendet werden sollen.

> [!NOTE]
> Wenn Sie ein Experiment aktivieren, beginnt Partner Center sofort Sammeln von Daten aus allen apps, die instrumentiert werden, um Daten für das Experiment zu protokollieren. Allerdings dauert es mehrere Stunden Experiment Daten im Partner Center angezeigt werden.

Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Aktivieren Ihres Experiments

Wenn Sie mit den Parametern des Experiments im Partner Center zufrieden sind, und Sie haben Ihren app-Code aktualisiert, können Sie das Experiment zu aktivieren, damit Sie das Experiment Datensammlung über Ihre app nutzen können. Wenn das Experiment aktiv ist, Ihre app Variation Werte abrufen und anzeigen und Konvertierung-Ereignisse zum Partner Center melden.

1. Melden Sie sich im [Partner Center](https://partner.microsoft.com/dashboard) an.
2. Wählen Sie unter **Ihre Apps** die App mit dem Experiment, das Sie aktivieren möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimentation** aus.
4. Erweitern Sie in der Tabelle der Projekte im Abschnitt **Projekte** das Projekt, das Ihr Experiment enthält, und führen Sie dann eine der folgenden Aufgaben aus:
  * Klicken Sie auf den Link **Aktivieren** für Ihr Experiment. Ihr Experiment wird dem Abschnitt **Aktive Experimente** im oberen Bereich der Seite hinzugefügt.
  * Klicken Sie auf den Namen des Experiments, führen Sie auf der Seite für Experimente einen Bildlauf nach unten aus, und klicken Sie auf **Aktivieren**.

> [!IMPORTANT]
> Nach dem Aktivieren eines Experiments können Sie die Experimentparameter nicht mehr ändern, wenn Sie beim Erstellen des Experiments nicht auf das Kontrollkästchen **Editable experiment** geklickt haben. Es wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

## <a name="review-the-results-of-your-experiment"></a>Prüfen der Experimentergebnisse

1. Im Partner Center zurück, zu der **experimentieren** für Ihre app.
2. Klicken Sie im Abschnitt **Aktive Experimente** auf den Namen des aktiven Experiments, um zur Experimentseite zu wechseln.
3. Bei aktiven oder beendeten Experimenten enthalten die ersten zwei Abschnitte auf dieser Seite die Ergebnisse Ihres Experiments:
  * Der Abschnitt **Ergebniszusammenfassung** enthält die Experimentziele und die Umwandlungsquote für jede Variation.
  * Der Abschnitt **Ergebnisdetails** enthält ausführlichere Informationen zu den einzelnen Varianten all der Ziele im Experiment, einschließlich Ansichten, Konvertierungen, eindeutiger Benutzer, Umwandlungsquote, Delta in %, Konfidenz und Signifikanz. Die *Konfidenz* ist ein statistisches Maß zum Angeben der Zuverlässigkeit einer Schätzung, mit dem die Fehlerspanne ermittelt wird. Die *Signifikanz* ist ein statistisches Maß, mit dem basierend auf einer Stichprobe die Wahrscheinlichkeit dafür bestimmt wird, dass ein Ergebnis nicht zufällig ist, sondern auf eine bestimmte Ursache zurückzuführen ist.

> [!NOTE]
> Partner Center meldet nur das erste Ereignis Konvertierung für jeden Benutzer in einem 24-Stunden-Zeitraum. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.


## <a name="complete-your-experiment"></a>Beenden Ihres Experiments

1. Zurück zu Ihrer Seite "Experiment" im Partner Center. Einzelheiten dazu finden Sie im vorherigen Abschnitt.
2. Führen Sie im Abschnitt **Ergebniszusammenfassung** eine der folgenden Aktionen durch:
  * Klicken Sie zum Beenden des Experiments und zum weiteren Verwenden der Variablenwerte der Steuerungsvariation für Ihre App auf **Beibehalten**.
  * Klicken Sie zum Beenden des Experiments und zum Verwenden der Variablenwerte einer anderen Variation für Ihre App unter der Variation, zu der Sie wechseln möchten, auf **Wechseln**.
3. Klicken Sie auf **OK**, um zu bestätigen, dass Sie das Experiment beenden möchten.


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie ein Projekt, und definieren Sie die remote-Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codieren Sie Ihre app für Experimente](code-your-experiment-in-your-app.md)
* [Definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A / B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md)
