---
Description: In dieser exemplarischen Vorgehensweise wird Ihr erstes Experiment mit A/B-Tests erstellt, ausgeführt und verwaltet.
title: Erstellen und Ausführen des ersten Experiments
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: 8dba9095326c01029e14742c98c1c368b896dfb8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660215"
---
# <a name="create-and-run-your-first-experiment"></a>Erstellen und Ausführen des ersten Experiments

In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:
* Erstellen Sie eine experimentieren [Projekt](run-app-experiments-with-a-b-testing.md#terms) im Partner Center, das mehrere Remoteserver Variablen definiert, die Text und Farbe einer Schaltfläche für die app darstellen.
* Erstellen Sie eine app mit Code, der die remote Variablenwerte abruft, verwendet diese Daten so ändern Sie die Hintergrundfarbe einer Schaltfläche und Protokolle anzeigen und Konvertierung-Ereignis, das wieder in Partner Center an.
* Erstellen eines Experiments im Projekt, um zu testen, ob die Anzahl von Klicks auf eine App-Schaltfläche durch das Ändern der Hintergrundfarbe der Schaltfläche erfolgreich erhöht werden kann
* Ausführen der App, um Experimentdaten zu sammeln
* Überprüfen Sie die experimentergebnisse im Partner Center, wählen Sie eine Variante, die für alle Benutzer der app zu aktivieren, und führen Sie das Experiment.

Eine Übersicht über A / B-Tests mit Partner Center finden Sie [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Voraussetzungen

Um diese exemplarische Vorgehensweise befolgen zu können, benötigen Sie ein Partner Center-Konto und Ihrem Entwicklungscomputer müssen Sie konfigurieren, wie in beschrieben [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>Erstellen Sie ein Projekt mit remote-Variablen im Partner Center

1. Melden Sie sich im [Partner Center](https://partner.microsoft.com/dashboard) an.
2. Wenn Sie bereits eine app im Partner Center, die Sie verwenden verfügen, um ein Experiment erstellen möchten, wählen Sie die app im Partner Center. Wenn Sie noch keine app im Partner Center verfügen [eine neue app erstellen, indem Sie einen Namen reservieren](../publish/create-your-app-by-reserving-a-name.md) und wählen Sie dann die app im Partner Center.
3. Klicken Sie im Navigationsbereich auf **Dienste** und dann auf **Experimentation**.
4. Klicken Sie auf der nächsten Seite im Abschnitt **Projekte** auf die Schaltfläche **Neues Projekt**.
5. Geben Sie auf der Seite **Neues Projekt** den Namen **Button Click Experiments** für das neue Projekt ein.
6. Erweitern Sie den Abschnitt **Remotevariablen**, und klicken Sie viermal auf **Variable hinzufügen**. Sie sollten jetzt über vier leere Variablenzeilen verfügen.
  * Geben Sie in der ersten Zeile **buttonText** als Variablennamen und **Graue Schaltfläche** in der Spalte **Standardwert** ein.
  * Geben Sie in der zweiten Zeile **r** als Variablennamen und **128** in der Spalte **Standardwert** ein.
  * Geben Sie in der dritten Zeile **g** als Variablennamen und **128** in der Spalte **Standardwert** ein.
  * Geben Sie in der vierten Zeile **b** als Variablennamen und **128** in der Spalte **Standardwert** ein.
7. Klicken Sie auf **Speichern**, und notieren Sie den Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) im Abschnitt **SDK-Integration**. Im nächsten Abschnitt aktualisieren Sie den App-Code und verweisen im Code auf diesen Wert.

## <a name="code-the-experiment-in-your-app"></a>Codieren des Experiments in der App

1. Erstellen Sie in Visual Studio ein neues UWP-Projekt (Universelle Windows-Plattform) mit Visual C#. Geben Sie dem Projekt den Namen **SampleExperiment**.
2. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Engagement Framework**, und klicken Sie anschließend auf **OK**.
5. Doppelklicken Sie im **Projektmappen-Explorer** auf „MainPage.xaml“, um den Designer für die Hauptseite in der App zu öffnen.
6. Ziehen Sie eine **Schaltfläche** aus der **Toolbox** auf die Seite.
7. Doppelklicken Sie im Designer auf die Schaltfläche, um die Codedatei zu öffnen, und fügen Sie einen Ereignishandler für das **Click**-Ereignis hinzu.  
8. Ersetzen Sie den gesamten Inhalt der Codedatei mit folgendem Code. Weisen Sie die ```projectId``` -Variable auf die [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) -Wert, der Sie vom Partner Center im vorherigen Abschnitt abgerufen haben.
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. Speichern Sie die Codedatei, und erstellen Sie das Projekt.

## <a name="create-the-experiment-in-partner-center"></a>Erstellen Sie das Experiment im Partner Center

1. Wechseln Sie zurück zur der **Schaltfläche klicken Sie auf der Experimente** im Partner Center-Projektseite.
2. Klicken Sie im Abschnitt **Experimente** auf die Schaltfläche **Neues Experiment**.
3. Geben Sie im Abschnitt **Experiment details** den Namen **Optimize Button Clicks** im Feld **Experiment name** ein.
4. Geben Sie im Abschnitt **View event** den Namen **userViewedButton** im Feld **Anzeige Ereignisname** ein. Dieser Name entspricht der Anzeigeereigniszeichenfolge, die Sie im Code protokolliert haben, der im vorherigen Abschnitt hinzugefügt wurde.
5. Geben Sie im Abschnitt **Goals and conversion events** die folgenden Werte ein:
  * Geben Sie im Feld **Goal name****Increase Button Clicks** ein.
  * Geben Sie im Feld **Umwandlungsereignisname** den Namen **userClickedButton** ein. Dieser Name entspricht der Umwandlungsereigniszeichenfolge, die Sie im Code protokolliert haben, der im vorherigen Abschnitt hinzugefügt wurde.
  * Wählen Sie im Feld **Ziel** die Option **Maximieren**.
6. Stellen Sie im Abschnitt **Remote variables and variations** sicher, dass das Kontrollkästchen **Gleichmäßig verteilen** aktiviert ist, damit die Varianten gleichmäßig über Ihre App verteilt werden.
7. Fügen Sie dem Experiment Variablen hinzu:
    1. Klicken Sie auf das Dropdownsteuerelement, wählen Sie **buttonText** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **Graue Schaltfläche** sollte automatisch in der Spalte **Variation A** angezeigt werden (dieser Wert wird von den Projekteinstellungen abgeleitet). Geben Sie in der Spalte **Variation B** den Text **Blaue Schaltfläche** ein.
    2. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **r** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **1** ein.
    3. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **g** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **1** ein.  
    4. Klicken Sie erneut auf das Dropdownsteuerelement, wählen Sie **b** aus, und klicken Sie auf **Variable hinzufügen**. Die Zeichenfolge **128** sollte automatisch in der Spalte **Variation A** angezeigt werden. Geben Sie in der Spalte **Variation B** die Zahl **255** ein.  
8. Klicken Sie auf **Speichern** und dann auf **Aktivieren**.

> [!IMPORTANT]
> Nach dem Aktivieren eines Experiments können Sie die Experimentparameter nicht mehr ändern, wenn Sie beim Erstellen des Experiments nicht auf das Kontrollkästchen **Editable experiment** geklickt haben. In der Regel wird empfohlen, das Experiment vor der Aktivierung in der App zu codieren.

## <a name="run-the-app-to-gather-experiment-data"></a>Ausführen der App, um Experimentdaten zu sammeln

1. Führen Sie die App **SampleExperiment** aus, die Sie zuvor erstellt haben.
2. Stellen Sie sicher, dass entweder eine graue oder eine blaue Schaltfläche angezeigt wird. Klicken Sie auf die Schaltfläche, und schließen Sie dann die App.
3. Wiederholen Sie die obigen Schritte auf demselben Computer mehrmals, um zu bestätigen, dass in Ihrer App die gleiche Schaltflächenfarbe angezeigt wird.

## <a name="review-the-results-and-complete-the-experiment"></a>Überprüfen der Ergebnisse Abschließen des Experiments

Warten Sie nach Abschluss des vorherigen Abschnitts mindestens ein paar Stunden, und führen Sie dann diese Schritte aus, um die Ergebnisse Ihres Experiments zu überprüfen und das Experiment abzuschließen.

> [!NOTE]
> Sobald Sie ein Experiment zu aktivieren, Partner Center wird sofort gestartet Sammeln von Daten aus allen apps, die instrumentiert werden, um Daten für das Experiment zu protokollieren. Allerdings dauert es mehrere Stunden Experiment Daten im Partner Center angezeigt werden.

1. Im Partner Center zurück, zu der **experimentieren** für Ihre app.
2. Klicken Sie im Abschnitt **Active experiments** auf **Optimize Button Clicks**, um zur Seite für dieses Experiment zu wechseln.
3. Vergewissern Sie sich, dass die Ergebnisse in den Abschnitten **Results summary** und **Results details** Ihren Erwartungen entsprechen. Weitere Informationen zu diesen Abschnitten, finden Sie unter [verwalten Sie Ihr Experiment in Partner Center](manage-your-experiment.md#review-the-results-of-your-experiment).
    > [!NOTE]
    > Partner Center meldet nur das erste Ereignis Konvertierung für jeden Benutzer in einem 24-Stunden-Zeitraum. Wenn ein Benutzer innerhalb von 24 Stunden mehrere Umwandlungsereignisse in Ihrer App auslöst, wird nur das erste Umwandlungsereignis gemeldet. So soll verhindert werden, dass die Experimentergebnisse für eine Stichprobengruppe von Benutzern durch einen einzelnen Benutzer mit mehreren Umwandlungsereignissen verfälscht wird.

4. Jetzt sind Sie bereit, das Experiment zu beenden. Klicken Sie im Abschnitt **Results summary** in der Spalte **Variation B** auf **Wechseln**. Dadurch wird für alle Benutzer Ihrer App zur blauen Schaltfläche gewechselt.
5. Klicken Sie auf **OK**, um zu bestätigen, dass Sie das Experiment beenden möchten.
6. Führen Sie die App **SampleExperiment** aus, die Sie im vorhergehenden Abschnitt erstellt haben.
7. Vergewissern Sie sich, dass eine blaue Schaltfläche angezeigt wird. Beachten Sie, dass es bis zu zwei Minuten dauern kann, bis Ihre App eine aktualisierte Variantenzuweisung erhält.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie ein Projekt, und definieren Sie die remote-Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codieren Sie Ihre app für Experimente](code-your-experiment-in-your-app.md)
* [Definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten Sie Ihr Experiment in Partner Center](manage-your-experiment.md)
* [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md)
