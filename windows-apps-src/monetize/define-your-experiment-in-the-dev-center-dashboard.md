---
description: Bevor Sie ein Experiment in ihrer universelle Windows-Plattform-app (UWP) mit A/B-Tests ausführen können, müssen Sie Ihr Experiment im Partner Center definieren.
title: Definieren eines Experiments im Partner Center
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: 909fa6b36f0b6dc51a3bb6eac117abae3b8b73ae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033213"
---
# <a name="define-your-experiment-in-partner-center"></a>Definieren eines Experiments im Partner Center

Nachdem Sie [ein Projekt erstellt und Remote Variablen in Partner Center definiert](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) und [Ihre APP für Experimente codiert](code-your-experiment-in-your-app.md)haben, können Sie im Projekt ein Experiment erstellen. Beim Erstellen des Experiments definieren Sie die Ziele und Abweichungen, die Ihre Benutzer erhalten.

Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>Erstellen Ihres Experiments

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard) an.
2. Wählen Sie unter **Ihre Apps** die App aus, für die Sie ein Experiment erstellen möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimentation** aus.
4. Geben Sie auf der Seite **Experimentation** in der Projekttabelle das Projekt an, dem Sie ein Experiment hinzufügen möchten, und klicken Sie für dieses Projekt auf **Add Experiment** .
5. Geben Sie im Feld **Name des Experiments** einen Namen ein, mit dem Sie das Experiment leicht ermitteln können. Nach dem Erstellen eines Experiments wird dieser Name auf der Seite **Experimentation** Ihrer App in der Liste der vorhandenen Experimente sowie auf der Seite des Projekts angezeigt.
6. Wenn Sie ein aktives Experiment bearbeiten möchten, aktivieren Sie das Kontrollkästchen **Editable experiment** . Aktivieren Sie dieses Kontrollkästchen nur, wenn Sie ein Experiment zum Überprüfen aller Abweichungen im Rahmen interner Tests erstellen. Weitere Informationen finden Sie unter [Erstellen eines Experiments für interne Tests](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments).
    > [!NOTE]
    > Aktivieren Sie dieses Kontrollkästchen nicht, wenn Sie ein Experiment erstellen, das Sie für Kunden freigeben möchten (d. h. ein Experiment, das einer Projekt-ID zugeordnet ist, die in einer Version der APP verwendet wird, die für Kunden verfügbar ist). Wenn Sie ein aktives Experiment bearbeiten, werden die Ergebnisse des Experiments ungültig.

7. Das aktuelle Projekt wird im Dropdown-Menü **Projektname** automatisch ausgewählt. Wenn Sie das neue Experiment einem anderen Projekt hinzufügen möchten, können Sie das Projekt hier auswählen. Nehmen Sie andernfalls keine Auswahl vor.
8.   Notieren Sie sich den Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms). Wenn Sie [Ihre APP für Experimente Program](code-your-experiment-in-your-app.md)mieren, müssen Sie diese ID in Ihrem Code referenzieren, damit Sie Variations Daten und Berichtsansicht-und Konvertierungs Ereignisse an Partner Center empfangen können.
9. Geben Sie im Abschnitt **Anzeigeereignis** den Namen des [Anzeigeereignisses](run-app-experiments-with-a-b-testing.md#terms) für das Experiment im Feld **Ereignisnamen anzeigen** ein.
10. Definieren Sie im Abschnitt **Ziele und Umwandlungsereignisse** mindestens ein Ziel für Ihr Experiment:
  * Geben Sie im Feld **Name des Ziels** einen beschreibenden Namen für Ihr Ziel ein. Nach dem Ausführen eines Experiments erscheint dieser Name in der Ergebniszusammenfassung des Experiments.
  * Geben Sie im Feld **Ereignisnamenumwandlung** den Namen des [Umwandlungsereignisses](run-app-experiments-with-a-b-testing.md#terms) für dieses Ziel ein.
  * Wählen Sie im Feld **Ziel****Maximieren** oder **Minimieren** aus, je nachdem, ob Sie das Vorkommen des Umwandlungsereignisses maximieren oder minimieren möchten. Diese Informationen werden in der Ergebniszusammenfassung des Experiments verwendet.

> [!NOTE]
> Partner Center meldet nur das erste Konvertierungs Ereignis für jede Benutzeransicht in einem Zeitraum von 24 Stunden. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer verfälscht werden, wenn das Ziel darin besteht, die Anzahl der Benutzer zu maximieren, die eine Umwandlung durchführen.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>Definieren der Remotevariablen und Abweichungen für das Experiment

Definieren Sie anschließend die [Remotevariablen](run-app-experiments-with-a-b-testing.md#terms) und [Abweichungen](run-app-experiments-with-a-b-testing.md#terms) für das Experiment.

1. Im Abschnitt **Remotevariablen und Abweichungen** sollten zwei Standardabweichungen angezeigt werden: **Variante A (Steuerelement)** und **Variante B** . Wenn Sie mehrere Abweichungen möchten, klicken Sie auf **Variante hinzufügen** . Optional können Sie jede Variante umbenennen.
2. Standardmäßig werden Variationen gleichmäßig an Ihre App-Benutzer verteilt. Wenn Sie einen bestimmten Verteilungsprozentsatz auswählen möchten, deaktivieren Sie das Kontrollkästchen **Gleichmäßig verteilen** , und geben Sie die Prozentsätze in die Zeile **Verteilung (%)** ein.
3. Fügen Sie Ihren Abweichungen Remotevariablen hinzu. Wählen Sie im Dropdown-Steuerelement am Ende dieses Abschnitts die einzelnen hinzuzufügenden Variablen aus, und klicken Sie auf **Variable hinzufügen** .
    > [!NOTE]
    > Die in diesem Steuerelement aufgeführten Variablen werden vom Projekt für das Experiment geerbt. Der Standardwert für die Variable (wie im Projekt definiert) wird der Steuerelementabweichung automatisch zugewiesen. Wenn Sie neue, hier nicht aufgeführte Variablen erstellen möchten, wechseln Sie zur entsprechenden Projektseite, und fügen Sie die Variablen dort hinzu.

4. Bearbeiten Sie die Variablenwerte für jede eindeutige Abweichung des Experiments (d. h. alle Abweichungen mit Ausnahme der Steuerelementabweichung).

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>Speichern und Aktivieren des Experiments

Wenn Sie die Eingabe in die erforderlichen Felder für Ihr Experiment abgeschlossen haben, klicken Sie auf **Speichern** , um Ihr Experiment zu speichern.

Wenn Sie mit den Parametern für Ihr Experiment zufrieden sind und Sie bereit sind, es zu aktivieren, damit Sie mit der Datenerfassung von Ihrer App beginnen können, klicken Sie auf **Aktivieren** . Wenn das Experiment aktiv ist, kann Ihre APP Variations Variablen und Berichtsansicht-und Konvertierungs Ereignisse an Partner Center abrufen. Weitere Informationen finden Sie unter [ausführen und Verwalten Ihres Experiments in Partner Center](manage-your-experiment.md).

> [!IMPORTANT]
> Ein Projekt kann jeweils nur ein aktives Experiment enthalten. Nach dem Aktivieren eines Experiments können Sie die Experimentparameter nicht mehr ändern, wenn beim Erstellen des Experiments das Kontrollkästchen **Editable experiment** nicht aktiviert wurde. Es wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>Erstellen eines Experiments für interne Tests

Sie können Ihr Experiment vor der Kundenaktivierung im Rahmen einer gesteuerten Zielgruppe (z. B. einer Reihe interner Prüfer) testen und sicherstellen, dass alle Abweichungen wie erwartet vorliegen. Dies ist möglich, wenn beim Erstellen des Experiments die Option **Editable experiment** ausgewählt wurde.

Führen Sie diese Schritte durch, um Ihr Experiment vor der Freigabe für die Kunden zu testen:

1. Erstellen Sie zwei Projekte: jeweils eins für den öffentlichen und privaten Build Ihrer App. Letzteres ist nur für Ihre Testzielgruppe verfügbar. Die folgenden Anweisungen beziehen sich auf diese Projekte, also jeweils auf das öffentliche und das Testprojekt.
2. Verweisen Sie beim [Codieren der App für das Experiment](code-your-experiment-in-your-app.md) über Ihr öffentliches Projekt im öffentlichen Build Ihrer App auf die Projekt-ID. Verweisen Sie im privaten Build der App über Ihr Testprojekt auf die Projekt-ID.
3. Erstellen Sie im Testprojekt ein Experiment, und wählen Sie die Option **Editable experiment** aus.
4. Aktivieren Sie das Experiment im Testprojekt. Weisen Sie einer Abweichung 100 % Verteilung zu, und stellen Sie sicher, dass diese Abweichung für Ihre Tester erwartungsgemäß auftritt. Wiederholen Sie den Vorgang für andere Abweichungen.
5. Vergewissern Sie sich, dass die Abweichungen wie erwartet auftreten, und nehmen Sie anschließend endgültige Änderungen am Experiment des Testprojekts vor. Wenn Sie das Experiment für die Kunden freigeben möchten, klonen Sie das Experiment im öffentlichen Projekt. Wählen Sie für dieses Experiment nicht die Option **Editable experiment** aus.
4. Stellen Sie sicher, dass die Verteilung der Zielabweichung im geklonten Experiment richtig ist.
5. Aktivieren Sie das geklonte Experiment, um es für die Kunden freizugeben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihr Experiment in Partner Center definiert und das Experiment in der App codiert haben, können Sie das [Experiment im Partner Center ausführen und verwalten](manage-your-experiment.md).

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen eines Projekts und Definieren von Remote Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programmieren Ihrer App für Experimente](code-your-experiment-in-your-app.md)
* [Verwalten eines Experiments im Partner Center](manage-your-experiment.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)
