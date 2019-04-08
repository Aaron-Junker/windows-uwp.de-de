---
Description: Zum Ausführen eines Experiments in Ihrer universellen Windows-Plattform (UWP)-App mit einem A/B-Test müssen Sie das Experiment in der App codieren.
title: Programmieren Ihrer App für Experimente
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, A/B-Tests, Experimente
ms.localizationpriority: medium
ms.openlocfilehash: edd0fbcf841dc9d8fa43873da95dc08b276a5418
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636745"
---
# <a name="code-your-app-for-experimentation"></a>Programmieren Ihrer App für Experimente

Nachdem Sie [ein Projekt erstellen und definieren Sie die remote-Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), Sie können den Code in Ihrer app (Universelle Windows Plattform) zum Aktualisieren:
* Erhalten Sie remote Variablenwerte von Partner Center an.
* Remotevariablen zum Konfigurieren von App-Funktionen für die Benutzer zu verwenden
* Protokollieren von Ereignissen in Partner Center, die angeben, wenn Benutzer Ihr Experiment angezeigt und eine gewünschte Aktion ausgeführt (so genannte eine *Konvertierung*).

Um der App dieses Verhalten hinzuzufügen, verwenden Sie die vom Microsoft Store Services SDK bereitgestellten APIs.

Den folgenden Abschnitten werden den allgemeinen Prozess der erste Variationen des Experiments und die Protokollierung von Ereignissen zum Partner Center. Nachdem Sie Ihre app für Experimente programmieren, können Sie [definieren Sie ein Experiment in Partner Center](define-your-experiment-in-the-dev-center-dashboard.md). Eine exemplarische Vorgehensweise, die den gesamten Erstellungs- und Ausführungsprozess für ein Experiment veranschaulicht, finden Sie unter [Erstellen und Durchführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Einige der Experimente-APIs in der Microsoft Store Services SDK verwendet die [asynchrone Muster](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) zum Abrufen von Daten über Partner Center. Dies bedeutet, dass ein Teil der Methodenausführung nach dem Aufrufen der Methoden stattfinden kann. Auf diese Weise bleibt die Benutzeroberfläche Ihrer App weiter reaktionsfähig, während die Vorgänge abgeschlossen werden. Für das asynchrone Muster muss die App beim Aufrufen der APIs das **async**-Schlüsselwort und den **await**-Operator verwenden, wie aus den Codebeispielen in diesem Artikel ersichtlich. Asynchrone Methoden enden üblicherweise mit **Async**.

## <a name="configure-your-project"></a>Konfigurieren des Projekts

Installieren Sie zunächst das Microsoft Store Services SDK auf dem Entwicklungscomputer, und fügen Sie dem Projekt die erforderlichen Verweise hinzu.

1. [Installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Öffnen Sie das Projekt in Visual Studio.
3. Erweitern Sie im Projektmappen-Explorer den Projektknoten, klicken Sie mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Verweis hinzufügen**.
3. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
4. Aktivieren Sie in der Liste mit den SDKs das Kontrollkästchen neben **Microsoft Engagement Framework**, und klicken Sie anschließend auf **OK**.

> [!NOTE]
> Bei den Codebeispielen in diesem Artikel wird davon ausgegangen, dass Ihre Codedatei **using**-Anweisungen für **System.Threading.Tasks** und **Microsoft.Services.Store.Engagement**-Namespaces enthält.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Abrufen von Abweichungsdaten und protokollieren des Anzeigeereignisses für Ihr Experiment

Suchen Sie im Projekt nach dem Code für das Feature, das Sie in Ihrem Experiment ändern möchten. Fügen Sie Code, der Daten für eine Variante abruft, diese Daten verwenden, um das Verhalten der Funktion zu ändern, Sie testen, und melden Sie sich dann das sichtereignis für das Experiment mit dem A / B-Tests Dienst im Partner Center.

Der jeweils benötigte Code richtet sich nach Ihrer App, im folgenden Beispiel wird jedoch die grundlegende Vorgehensweise veranschaulicht. Ein vollständiges Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

In den folgenden Schritten werden die wichtigen Schritte dieses Verfahrens ausführlich beschrieben.

1. Deklarieren Sie eine [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) Objekt, das die aktuelle Variation-Zuweisung darstellt und einen [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) -Objekt, das Sie zu Protokollansicht und Konvertierung verwenden Ereignisse zum Partner Center.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Deklarieren Sie eine Zeichenfolgenvariable, die der [Projekt-ID](run-app-experiments-with-a-b-testing.md#terms) für das Experiment zugewiesen wird, das Sie abrufen möchten.
    > [!NOTE]
    > Sie erhalten ein Projekt-ID, wenn Sie [erstellen Sie ein Projekt im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Der hier gezeigte Projekt-ID dient nur als Beispiel.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Rufen Sie die statische [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync)-Methode auf, um die aktuelle zwischengespeicherte Abweichungszuweisung für Ihr Experiment abzurufen, und übergeben Sie die Projekt-ID für das Experiment an die Methode. Diese Methode gibt ein [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult)-Objekt zurück, das den Zugriff auf die Abweichungszuweisung (über die [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation)-Eigenschaft) ermöglicht.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Überprüfen Sie anhand der [IsStale](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale)-Eigenschaft, ob die zwischengespeicherte Abweichungszuweisung mit einer Remoteabweichungszuweisung vom Server aktualisiert werden muss. Wenn ja, rufen Sie die statische [GetRefreshedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync)-Methode auf, um auf dem Server nach einer aktualisierten Abweichungszuweisung zu suchen und die lokale zwischengespeicherte Abweichung zu aktualisieren.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Verwenden Sie die [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean)-, [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble)-, [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) oder [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring)-Methode des [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)-Objekts, um die Werte für die Abweichungszuweisung abzurufen. In jeder Methode der erste Parameter ist der Name des der Variante, die Sie abrufen möchten (Dies ist das denselben Namen wie eine Variante, die Sie im Partner Center eingeben). Der zweite Parameter ist der Standardwert, den die Methode zurückgeben soll, ist dies nicht den angegebenen Wert von Partner Center abrufen, (z. B., wenn keine Netzwerkverbindung vorhanden ist), und eine zwischengespeicherte Version des der Variante ist nicht verfügbar.

    Im folgenden Beispiel werden mithilfe von [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) eine Variable namens *buttonText* abgerufen und der Standardvariablenwert **Grey Button** angegeben.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. Verwenden Sie die Variablenwerte im Code, um das Verhalten des getesteten Features zu ändern. Der folgende Code weist beispielsweise den *buttonText*-Wert dem Inhalt einer Schaltfläche in Ihrer App zu. In diesem Beispiel wird davon ausgegangen, dass Sie diese Schaltfläche bereits an einer anderen Stelle in Ihrem Projekt definiert haben.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Melden Sie sich schließlich die [sichtereignis](run-app-experiments-with-a-b-testing.md#terms) für das Experiment mit dem A / B-Tests Dienst im Partner Center. Initialisieren Sie das ```logger```-Feld in ein [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger)-Objekt, und rufen Sie die [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)-Methode auf. Übergeben Sie die [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) Objekt, das die aktuelle Variation-Zuweisung (dieses Objekt stellt Kontext zum Ereignis zum Partner Center) und den Namen des Ereignisses anzeigen, für das Experiment darstellt. Dieser muss den Ansicht-Ereignisnamen übereinstimmen, den Sie für das Experiment im Partner Center eingeben. Vom Code sollte das Anzeigeereignis protokolliert werden, wenn der Benutzer mit dem Anzeigen einer Abweichung beginnt, die Teil des Experiments ist.

    Das folgende Beispiel veranschaulicht, wie ein Anzeigeereignis namens **userViewedButton** protokolliert wird. Das Ziel des Experiments in diesem Beispiel besteht darin, dass der Benutzer auf eine Schaltfläche in der App klickt, damit das Anzeigeereignis protokolliert wird, nachdem die App die Abweichungsdaten (in diesem Fall den Schaltflächentext) abgerufen und dem Inhalt der Schaltfläche zugewiesen hat.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>Protokollieren von Konvertierung-Ereignissen in Partner Center

Fügen Sie Code, der protokolliert [Konvertierung Ereignisse](run-app-experiments-with-a-b-testing.md#terms) mit dem A / B-Tests Dienst im Partner Center. Vom Code sollte ein Umwandlungsereignis protokolliert werden, wenn der Benutzer ein für Ihr Experiment gesetztes Ziel erreicht. Welchen Code Sie genau benötigen, hängt von Ihrer App ab. Hier werden daher nur die allgemeinen Schritte angegeben. Ein vollständiges Codebeispiel finden Sie unter [Erstellen und Ausführen eines ersten Experiments mit A/B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Rufen Sie in dem Code, der ausgeführt wird, wenn der Benutzer eines der Ziele für das Experiment erreicht, erneut die [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation)-Methode auf, und übergeben Sie das [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation)-Objekt und den Namen eines Umwandlungsereignisses für Ihr Experiment. Dies muss mit einem von der Konvertierung Ereignisnamen übereinstimmen, die Sie für das Experiment im Partner Center eingeben.

    Im folgenden Beispiel wird ein Umwandlungsereignis namens **userClickedButton** aus dem **Click**-Ereignishandler für eine Schaltfläche protokolliert. In diesem Beispiel besteht das Ziel des Experiments darin, dass der Benutzer auf die Schaltfläche klickt.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Experiment in Ihrer App programmiert haben, sind Sie bereit für die folgenden Schritte:
1. [Definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md). Erstellen Sie ein Experiment, das die Anzeigeereignisse, Umwandlungsereignisse und eindeutigen Abweichungen für den A/B-Test festlegt.
2. [Ausführen und verwalten Sie Ihr Experiment in Partner Center](manage-your-experiment.md).


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie ein Projekt, und definieren Sie die remote-Variablen im Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definieren Sie das Experiment im Partner Center](define-your-experiment-in-the-dev-center-dashboard.md)
* [Verwalten Sie Ihr Experiment in Partner Center](manage-your-experiment.md)
* [Erstellen und Ausführen Ihres ersten Experiments mit A / B-Tests](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ausführen von app-Experimenten mit A / B-Tests](run-app-experiments-with-a-b-testing.md)
