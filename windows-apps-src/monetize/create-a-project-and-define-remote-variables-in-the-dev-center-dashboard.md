---
Description: Vor dem Ausführen eines Experiments in Ihrer app (Universelle Windows Plattform) mit A / B-Tests, müssen Sie ein Projekt erstellen und definieren Ihre remote-Variablen im Partner Center.
title: Erstellen eines Experimentprojekts im Partner Center
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: acfd654f02cb7fb727d35271175e59966e2abdc4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650795"
---
# <a name="create-an-experiment-project-in-partner-center"></a>Erstellen eines Experimentprojekts im Partner Center

Erstellen Sie zum Einstieg in experimentieren Experimente [Projekt](run-app-experiments-with-a-b-testing.md#terms) für Ihre app im Partner Center und definieren Sie die remote-Variablen, die Ihre app zugreifen können.

Die folgenden Anweisungen beschreiben die wichtigsten Schritte für die Erstellung eines Projekts. Eine ausführliche Erläuterung, die den gesamten Erstellungs- und Ausführungsprozess für ein Projekt und die Durchführung eines Experiments veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Anweisungen

1. Melden Sie sich im [Partner Center](https://partner.microsoft.com/dashboard) an.
2. Wählen Sie unter **Ihre Apps** die App aus, für die Sie ein Experiment erstellen möchten.
3. Wählen Sie im Navigationsbereich **Dienste** und dann **Experimentation** aus.
4. Klicken Sie auf der Seite **Experimente** im Abschnitt **Projekte** auf die Schaltfläche **Neues Projekt**. Wenn Sie bereits ein oder mehrere Projekte erstellt haben, werden diese Projekte im Abschnitt **Projekte** aufgeführt.
5. Geben Sie auf der Seite **Neues Projekt** einen Namen für das neue Projekt ein.
6. Fügen Sie im Abschnitt **Remotevariablen** die [Variablen](run-app-experiments-with-a-b-testing.md#terms) hinzu, die für alle Experimente in diesem Projekt verfügbar sein sollen, und definieren Sie die Standardwerte für jede Variable. Die hier festgelegten Standardwerte werden für die Steuerelementgruppe der Experimente verwendet, sowie für alle Benutzer, die an dem Experiment nicht teilnehmen.
  1. Falls der Abschnitt **Remotevariablen** reduziert ist, klicken Sie in der Abschnittsüberschrift auf **Anzeigen**.
  2. Klicken Sie auf **Variable hinzufügen**, um jede neue Variable zu erstellen, die für jedes Experiment in diesem Projekt verfügbar sein soll, und geben Sie den Variablennamen und den Standardwert der Variablen ein.
  3. Wenn Sie das Hinzufügen von Variablen beendet haben, klicken Sie auf **Speichern**.
3. Notieren Sie im Abschnitt **SDK-Integration** den Wert der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms). Wenn Sie [code Ihrer app experimentieren](code-your-experiment-in-your-app.md), Sie müssen dieses Projekt-ID in Ihrem Code verweisen, Sie Variante Daten empfangen und Ereignisse anzeigen und die Konvertierung zum Partner Center melden können.

> [!NOTE]
> Sie können keine Remotevariablen bearbeiten, hinzufügen oder entfernen, während ein Experiment im Projekt aktiv ist. Diese Einschränkung hilft, die Integrität der Daten der Steuerelementgruppe für das aktive Experiment zu schützen.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein Projekt erstellt haben, können Sie mit dem [Codieren der App für das Experiment](code-your-experiment-in-your-app.md) beginnen, indem Sie Werte von Remotevariablen in Ihrer App abrufen, und Sie können [ein Experiment im Projekt erstellen](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Verwandte Themen

* [Codieren Sie Ihre app für Experimente](code-your-experiment-in-your-app.md)
* [Definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten Sie Ihr Experiment in Partner Center](manage-your-experiment.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A / B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md)
