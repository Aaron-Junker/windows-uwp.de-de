---
title: Den Dienst für die Aktivitätsüberwachung in Echtzeit-Programmierung
description: Erfahren Sie mehr über das Programmieren von Xbox Live Real-Time-Aktivität-Dienst mit der C++-APIs.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Real-Time-Aktivität
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629995"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>Programmierung der Real-Time-Aktivitätsdienst mithilfe C++-APIs

Dieser Artikel enthält die folgenden Abschnitte

* Herstellen einer Verbindung mit dem Dienst für die Aktivitätsüberwachung in Echtzeit von Xbox Live
* Die Verbindung getrennt aus dem Dienst für die Aktivitätsüberwachung in Echtzeit
* Erstellen eine Statistik
* Abonnieren eine Statistik der Real-Time-Aktivität
* Eine Statistik aus dem Dienst Real-Time-Aktivität aufheben
* Beispiel

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Herstellen einer Verbindung mit dem Dienst für die Aktivitätsüberwachung in Echtzeit von Xbox Live

Anwendungen müssen mit der Real-Time-Aktivität (RTA)-Dienst zum Abrufen von Informationen von Xbox Live verbinden. In diesem Thema zeigt, wie eine solche Verbindung hergestellt wird.

> [!NOTE]
> In diesem Thema verwendeten Beispiele geben Sie an Methodenaufrufen für einen Benutzer. Titel des muss jedoch diese Aufrufe für alle Benutzer eine Verbindung herstellen und trennen den Real-Time-Aktivität (RTA)-Dienst vornehmen.

### <a name="connecting-to-the-real-time-activity-service"></a>Verbindung mit dem Dienst Real-Time-Aktivität

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>Erstellen eine Statistik

Statistiken auf XDP zu erstellen, wenn Sie ein Entwickler von xdk-Version oder der Arbeit sind auf einen Cross-Play-Titel.  Erstellen Sie Statistiken im Partner Center, wenn Sie eine reine UWP, die unter Windows 10 vornehmen.

#### <a name="xdk-developers"></a>Entwickler von xdk-Version

Informationen zum Status auf XDP zu erstellen, finden Sie unter den [XDP Dokumentation](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events).  Nachdem Sie Ihre Stat erstellt und definiert die Ereignisse haben, müssen Sie zum Ausführen der [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) um einen Header, die von Ihrer Anwendung verwendeten zu generieren.  Dieser Header enthält Funktionen, die Sie aufrufen können, zum Senden von Ereignissen, die Statistiken zu ändern.

#### <a name="uwp-developers"></a>UWP-Entwickler

Wenn Sie eine UWP unter Windows 10, die nicht auf einen Cross-Play-Titel ist entwickeln, definieren Sie Ihre Statistiken in [Partner Center](https://partner.microsoft.com/dashboard). Lesen der [Partner Center-Statistiken Konfiguration Artikel](../leaderboards-and-stats-2017/player-stats-configure-2017.md) zu erfahren, wie Sie Statistiken auf Partner Center zu konfigurieren.

> [!NOTE]
> Statistiken 2013-Entwickler benötigen, wenden Sie sich an ihren DAM für Informationen über [Stats-2013-Konfigurationstool](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) in [Partner Center](https://partner.microsoft.com/dashboard).

### <a name="disconnecting-from-the-real-time-activity-service"></a>Trennen den Dienst in Echtzeit-Aktivität

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>Abonnieren eine Statistik der Real-Time-Aktivität

Anwendungen Abonnieren einer Echtzeit Aktivität (RTA) um Updates zu erhalten, wenn die Statistiken, die im Xbox-Entwickler-Portal (XDP) oder über das Partner Center konfiguriert ändern.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>Abonnieren eine Statistik aus dem Dienst Real-Time-Aktivität

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>Eine Statistik aus dem Dienst Real-Time-Aktivität aufheben

Anwendungen abonnieren Sie eine Statistik von der Real-Time-Aktivität (RTA)-Dienst, um Updates zu erhalten, wenn die Statistik geändert wird. Wenn diese Updates nicht mehr benötigt werden, kann das Abonnement beendet werden, und der Code in diesem Thema wird gezeigt, wie das geht.

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>Eine Statistik für die Echtzeit-Diensten aufheben

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> Der Real-Time-Aktivität, die Dienst nach der Nutzung von zwei Stunden getrennt wird, muss Ihr Code Lage sein zu erkennen und eine Verbindung mit der Real-Time-Aktivität Dienst wiederherzustellen, falls er noch benötigt wird. Dies erfolgt in erster Linie, um sicherzustellen, dass das Authentifizierungstoken nach Ablauf aktualisiert werden.
> 
> Wenn ein Client RTA für Multiplayer-Sitzungen verwendet, und für 30 Sekunden getrennt wird, erkennt der Multiplayer-Sitzung Directory(MPSD) an, dass die RTA-Sitzung wird geschlossen, und den Benutzer von der Sitzung startet. Es liegt an den Client RTA erkennen, wenn die Verbindung geschlossen wird und das Initiieren einer erneut hergestellten Verbindung und erneut abonnieren, bevor MPSD den Sitzungszustandsanbieter beendet.