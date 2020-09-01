---
Description: Zum Ausführen eines Experiments in Ihrer universellen Windows-Plattform (UWP)-App mit einem A/B-Test müssen Sie das Experiment in der App codieren.
title: Programmieren Ihrer App für Experimente
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: 3a7709311539d3f9c50f600f617c211f99c7b507
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171674"
---
# <a name="code-your-app-for-experimentation"></a>Programmieren Ihrer App für Experimente

Nachdem Sie [ein Projekt erstellt und Remote Variablen im Partner Center definiert](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)haben, können Sie den Code in ihrer universelle Windows-Plattform-app (UWP) wie folgt aktualisieren:
* Empfangen von Remote Variablen Werten aus Partner Center.
* Remotevariablen zum Konfigurieren von App-Funktionen für die Benutzer zu verwenden
* Protokollieren Sie Ereignisse in Partner Center, die angeben, wann Benutzer ihr Experiment angezeigt und eine gewünschte Aktion (auch als *Konvertierung*bezeichnet) durchgeführt haben.

Um der App dieses Verhalten hinzuzufügen, verwenden Sie die vom Microsoft Store Services SDK bereitgestellten APIs.

In den folgenden Abschnitten wird der allgemeine Prozess zum erhalten von Variationen für das Experimentieren und Protokollieren von Ereignissen an Partner Center beschrieben. Nachdem Sie Ihre APP für Experimente codiert haben, können Sie [ein Experiment in Partner Center definieren](define-your-experiment-in-the-dev-center-dashboard.md). Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Einige der experimentieren-APIs im Microsoft Store Services SDK verwenden das [asynchrone Muster](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) zum Abrufen von Daten aus Partner Center. Dies bedeutet, dass ein Teil der Methodenausführung nach dem Aufrufen der Methoden stattfinden kann. Auf diese Weise bleibt die Benutzeroberfläche Ihrer App weiter reaktionsfähig, während die Vorgänge abgeschlossen werden. Für das asynchrone Muster muss die App beim Aufrufen der APIs das **async**-Schlüsselwort und den **await**-Operator verwenden, wie aus den Codebeispielen in diesem Artikel ersichtlich. Asynchrone Methoden enden üblicherweise mit **Async**.

## <a name="configure-your-project"></a>Konfigurieren des Projekts

Installieren Sie zunächst das Microsoft Store Services SDK auf dem Entwicklungscomputer, und fügen Sie dem Projekt die erforderlichen Verweise hinzu.

1. [Installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Öffnen Sie Ihr Projekt in Visual Studio.
3. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Engagement Framework**, und klicken Sie anschließend auf **OK**.

> [!NOTE]
> In den Codebeispielen in diesem Artikel wird davon ausgegangen, dass Ihre Codedatei-Anweisungen für die Namespaces " **System. Threading. Tasks** " und " **Microsoft. Services. Store. Engagement** " **verwendet** .

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Abrufen von Abweichungsdaten und protokollieren des Anzeigeereignisses für Ihr Experiment

Suchen Sie im Projekt nach dem Code für das Feature, das Sie in Ihrem Experiment ändern möchten. Fügen Sie Code hinzu, mit dem Daten für eine Variation abgerufen werden, verwenden Sie diese Daten, um das Verhalten der zu testenden Funktion zu ändern, und protokollieren Sie dann das Ansichts Ereignis für Ihr Experiment an den a/B-Test Dienst im Partner Center.

Der jeweils benötigte Code richtet sich nach Ihrer App, im folgenden Beispiel wird jedoch die grundlegende Vorgehensweise veranschaulicht. Ein vollständiges Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

In den folgenden Schritten werden die wichtigen Schritte dieses Verfahrens ausführlich beschrieben.

1. Deklarieren Sie ein [storeservicesexperimentellen Variation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) -Objekt, das die aktuelle Variations Zuweisung und ein [storeservicescustomeventlogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) -Objekt darstellt, das Sie zum Protokollieren von Ansichts-und Konvertierungs Ereignissen in Partner Center verwenden.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Deklarieren Sie eine Zeichenfolgenvariable, die der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) für das Experiment zugewiesen wird, das Sie abrufen möchten.
    > [!NOTE]
    > Sie erhalten eine Projekt-ID, wenn Sie [ein Projekt im Partner Center erstellen](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Der hier gezeigte Projekt-ID dient nur als Beispiel.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Rufen Sie die statische [GetCachedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync)-Methode auf, um die aktuelle zwischengespeicherte Abweichungszuweisung für Ihr Experiment abzurufen, und übergeben Sie die Projekt-ID für das Experiment an die Methode. Diese Methode gibt ein [StoreServicesExperimentVariationResult](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult)-Objekt zurück, das den Zugriff auf die Abweichungszuweisung (über die [ExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation)-Eigenschaft) ermöglicht.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Überprüfen Sie anhand der [IsStale](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale)-Eigenschaft, ob die zwischengespeicherte Abweichungszuweisung mit einer Remoteabweichungszuweisung vom Server aktualisiert werden muss. Wenn ja, rufen Sie die statische [GetRefreshedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync)-Methode auf, um auf dem Server nach einer aktualisierten Abweichungszuweisung zu suchen und die lokale zwischengespeicherte Abweichung zu aktualisieren.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Verwenden Sie die [GetBoolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean)-, [GetDouble](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble)-, [GetInt32](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) oder [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring)-Methode des [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)-Objekts, um die Werte für die Abweichungszuweisung abzurufen. In jeder Methode ist der erste Parameter der Name der Variation, die Sie abrufen möchten (Dies ist der gleiche Name einer Variation, die Sie im Partner Center eingeben). Der zweite Parameter ist der Standardwert, der von der-Methode zurückgegeben werden soll, wenn der angegebene Wert nicht aus Partner Center abgerufen werden kann (z. b. wenn keine Netzwerk Konnektivität vorhanden ist) und eine zwischengespeicherte Version der Variation nicht verfügbar ist.

    Im folgenden Beispiel werden mithilfe von [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) eine Variable namens *buttonText* abgerufen und der Standardvariablenwert **Grey Button** angegeben.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. Verwenden Sie die Variablenwerte im Code, um das Verhalten des getesteten Features zu ändern. Der folgende Code weist beispielsweise den *buttonText*-Wert dem Inhalt einer Schaltfläche in Ihrer App zu. In diesem Beispiel wird davon ausgegangen, dass Sie diese Schaltfläche bereits an einer anderen Stelle in Ihrem Projekt definiert haben.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Protokollieren Sie schließlich das [Ansichts Ereignis](run-app-experiments-with-a-b-testing.md#terms) für Ihr Experiment in den A/B-Test Dienst im Partner Center. Initialisieren Sie das ```logger```-Feld in ein [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger)-Objekt, und rufen Sie die [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)-Methode auf. Übergeben Sie das [storeservicesexperimentellen Variation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) -Objekt, das die aktuelle Variations Zuweisung darstellt (dieses Objekt stellt den Kontext zum Ereignis für Partner Center bereit), und den Namen des Ansichts Ereignisses für das Experiment. Dies muss mit dem Ansichts Ereignis Namen, den Sie für Ihr Experiment in Partner Center eingegeben haben, identisch sein. Vom Code sollte das Anzeigeereignis protokolliert werden, wenn der Benutzer mit dem Anzeigen einer Abweichung beginnt, die Teil des Experiments ist.

    Das folgende Beispiel veranschaulicht, wie ein Anzeigeereignis namens **userViewedButton** protokolliert wird. Das Ziel des Experiments in diesem Beispiel besteht darin, dass der Benutzer auf eine Schaltfläche in der App klickt, damit das Anzeigeereignis protokolliert wird, nachdem die App die Abweichungsdaten (in diesem Fall den Schaltflächentext) abgerufen und dem Inhalt der Schaltfläche zugewiesen hat.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>Protokollieren von Konvertierungs Ereignissen in Partner Center

Fügen Sie als nächstes Code hinzu, der [Konvertierungs Ereignisse](run-app-experiments-with-a-b-testing.md#terms) in den A/B-Test Dienst in Partner Center protokolliert. Vom Code sollte ein Umwandlungsereignis protokolliert werden, wenn der Benutzer ein für Ihr Experiment gesetztes Ziel erreicht. Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein vollständiges Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Rufen Sie in dem Code, der ausgeführt wird, wenn der Benutzer eines der Ziele für das Experiment erreicht, erneut die [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)-Methode auf, und übergeben Sie das [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)-Objekt und den Namen eines Umwandlungsereignisses für Ihr Experiment. Diese muss mit einem der Konvertierungs Ereignis Namen identisch sein, die Sie für Ihr Experiment in Partner Center eingegeben haben.

    Im folgenden Beispiel wird ein Umwandlungsereignis namens **userClickedButton** aus dem **Click**-Ereignishandler für eine Schaltfläche protokolliert. In diesem Beispiel besteht das Ziel des Experiments darin, dass der Benutzer auf die Schaltfläche klickt.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Experiment in Ihrer App programmiert haben, sind Sie bereit für die folgenden Schritte:
1. [Definieren Sie Ihr Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md). Erstellen Sie ein Experiment, das die Anzeigeereignisse, Umwandlungsereignisse und eindeutigen Abweichungen für den A/B-Test festlegt.
2. [Führen Sie das Experiment im Partner Center aus, und verwalten Sie es](manage-your-experiment.md).


## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen eines Projekts und Definieren von Remote Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definieren eines Experiments im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten eines Experiments im Partner Center](manage-your-experiment.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)