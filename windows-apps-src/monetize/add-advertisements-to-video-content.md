---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Erfahren Sie, wie Sie die adscheduler-Klasse verwenden, um Anzeigen in Videoinhalten in einer universelle Windows-Plattform-app (UWP) anzuzeigen, die mithilfe von JavaScript mit HTML geschrieben wurde.
title: Anzeigen von Werbung in Videoinhalten
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, Video, Scheduler, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 6baf26b083cce08557a9b09f2ba95d5ad889f4a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175104"
---
# <a name="show-ads-in-video-content"></a>Anzeigen von Werbung in Videoinhalten

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In dieser exemplarischen Vorgehensweise wird gezeigt, wie die **adscheduler** -Klasse verwendet wird, um Anzeigen in Videoinhalten in einer universelle Windows-Plattform-app (UWP) anzuzeigen, die mithilfe von JavaScript mit HTML geschrieben wurde.

> [!NOTE]
> Diese Funktion wird zurzeit nur für UWP-Apps unterstützt, die mithilfe von JavaScript mit HTML geschrieben werden.

**AdScheduler** funktioniert mit progressiven Medien und Streamingmedien und nutzt Video Ad Serving Template (VAST) 2.0/3.0 nach IAB-Standard und VMAP-Nutzlastformate. Durch die Verwendung von Standards ist **AdScheduler** agnostisch gegenüber dem Anzeigendienst, mit dem die Interaktion erfolgt.

Werbung für Videoinhalte variiert in Abhängigkeit davon, ob das Programm kürzer als zehn Minuten (kurzes Format) oder länger als zehn Minuten (langes Format) ist. Die Einrichtung des letzteren Formats für den Dienst ist zwar komplizierter, aber es besteht eigentlich kein Unterschied darin, wie der clientseitige Code geschrieben wird. Wenn **AdScheduler** eine VAST-Nutzlast mit einer einzelnen Werbeanzeige anstelle eines Manifests empfängt, wird dies so behandelt, als ob das Manifest eine einzelne Pre-Roll-Anzeige (eine Unterbrechung bei 00:00) aufgerufen hat.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version.

* In Ihrem Projekt muss das [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)-Steuerelement verwendet werden, um die Videoinhalte bereitstellen zu können, für die die Anzeigen eingeplant werden. Dieses Steuerelement ist in der Bibliotheksammlung [TVHelpers](https://github.com/Microsoft/TVHelpers) von Microsoft auf GitHub verfügbar.

  Das folgende Beispiel zeigt, wie Sie einen [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) im HTML-Markup deklarieren. Dieser Markupcode gehört normalerweise in den Abschnitt `<body>` in der Datei „index.html“ (oder einer anderen HTML-Datei, je nach Eignung für Ihr Projekt).

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  Im folgenden Beispiel wird veranschaulicht, wie Sie einen [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) in JavaScript-Code einrichten.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Verwenden der AdScheduler-Klasse im Code

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Fügen Sie Ihrem Projekt einen Verweis auf die Bibliothek **Microsoft Advertising SDK für JavaScript** hinzu.

    1. Klicken Sie im **Projektmappen-Explorer** Fenster mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen... aus.**
    2. Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für JavaScript** (Version 10.0).
    3. Klicken Sie im **Verweis-Manager** auf „OK“.

4.  Fügen Sie dem Projekt die Datei „AdScheduler.js“ hinzu:

    1. Klicken Sie in Visual Studio auf **Projekt** und **NuGet-Pakete verwalten**.
    2. Geben Sie im Suchfeld den Suchbegriff **Microsoft.StoreServices.VideoAdScheduler** ein, und installieren Sie das Microsoft.StoreServices.VideoAdScheduler-Paket. Die Datei „AdScheduler.js“ wird dem Unterverzeichnis „../js“ Ihres Projekts hinzugefügt.

5.  Öffnen Sie die Datei „index.html“ (oder je nach Projekt eine andere HTML-Datei). Fügen Sie im Abschnitt `<head>` nach den JavaScript-Verweisen auf „default.css“ und „main.js“ des Projekts den Verweis auf „ad.js“ und „adscheduler.js“ hinzu.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Diese Zeile muss `<head>` nach dem einschließen von main.js in den Abschnitt eingefügt werden. andernfalls tritt ein Fehler auf, wenn Sie das Projekt erstellen.

6.  Fügen Sie der Datei „main.js“ des Projekts Code hinzu, mit dem ein neues **AdScheduler**-Objekt erstellt wird. Übergeben Sie den **MediaPlayer**, mit dem Ihre Videoinhalte gehostet werden. Der Code muss so angeordnet werden, dass er nach [WinJS.UI.processAll](/previous-versions/windows/apps/hh440975) ausgeführt wird.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Verwenden Sie die **requestschedule** -Methode oder die **requestschedulebyurl** -Methode des **adscheduler** -Objekts, um einen AD-Zeitplan vom Server anzufordern und in die **Media Player** -Zeitachse einzufügen und dann die Video Medien wiederzugeben.

    * Wenn Sie Microsoft-Partner sind und eine Berechtigung zum Anfordern eines Anzeigenzeitplans vom Microsoft-Anzeigenserver erhalten haben, können Sie **requestSchedule** verwenden und die IDs für die Anwendung und die Anzeigeneinheit angeben, die von Ihrem Microsoft-Vertreter bereitgestellt wurden.

        Diese Methode hat die Form einer [Zusicherung](../threading-async/asynchronous-programming-universal-windows-platform-apps.md#asynchronous-patterns-in-uwp-using-javascript), bei der es sich um ein asynchrones Konstrukt handelt, bei dem zwei Funktionszeiger übermittelt werden: ein Zeiger für die **OnComplete** -Funktion, die aufgerufen werden soll, wenn die Zusage erfolgreich abgeschlossen wird, und ein Zeiger für die **OnError** -Funktion, die bei einem Fehler aufgerufen wird Beginnen Sie in der **OnComplete** -Funktion mit der Wiedergabe von Videoinhalten. Die Werbe Wiedergabe wird zum geplanten Zeitpunkt wiedergegeben. Behandeln Sie den Fehler in der **OnError** -Funktion, und beginnen Sie dann mit der Wiedergabe Ihres Videos. Ihre Videoinhalte werden ohne Werbung wiedergegeben. Das-Argument der **OnError** -Funktion ist ein Objekt, das die folgenden Member enthält.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Verwenden Sie **requestschedulebyurl**, und übergeben Sie den Server-URI, um einen AD-Zeitplan von einem nicht-Microsoft-Ad-Server anzufordern. Diese Methode hat auch die Form einer **Zusicherung** , die Zeiger für die **OnComplete** -und **OnError** -Funktionen akzeptiert. Die vom Server zurückgegebene AD-Nutzlast muss den Nutz Last Formaten für die Video-und Videowiedergabe Vorlage (Vast) oder Video Multiple AD Wiedergabelisten (VMAP) entsprechen.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Warten Sie, bis **requestschedule** oder **requestschedulebyurl** zurückgegeben wird, bevor Sie mit der Wiedergabe der primären Videoinhalte in **Media Player**beginnen. Die Wiedergabe von Medien vor der Rückgabe von **requestschedule** (bei einer Pre-Roll-Ankündigung) führt dazu, dass der primäre Videoinhalt von der vorab Ausführung unterbrochen wird. Sie müssen auch " **Play** " aufruft, auch wenn die Funktion fehlschlägt, da der " **Media Player" den Media Player** anweist, die AD (s) zu überspringen und direkt zum Inhalt zu wechseln. **AdScheduler** Unter Umständen besteht bei Ihnen auch eine andere geschäftliche Anforderung, z. B. das Einfügen einer integrierten Anzeige, falls eine Anzeige nicht erfolgreich von einem Remotestandort abgerufen werden kann.

8.  Während der Wiedergabe können Sie zusätzliche Ereignisse verarbeiten, mit denen Ihre App den Status bzw. Fehler nachverfolgen kann, die nach dem ersten Anzeigenabgleich auftreten. Der folgende Code zeigt einige dieser Ereignisse, einschließlich **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** und **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>Adscheduler-Mitglieder

Dieser Abschnitt enthält einige Details zu den Membern des **adscheduler** -Objekts. Weitere Informationen zu diesen Elementen finden Sie in den Kommentaren und Definitionen in der AdScheduler.js-Datei in Ihrem Projekt.

### <a name="requestschedule"></a>requestschedule

Diese Methode fordert einen AD-Zeitplan von Microsoft Ad Server an und fügt ihn in die Zeitachse von **Media Player** ein, die an den **adscheduler** -Konstruktor übergeben wurde.

Der optionale dritte Parameter (*adtags*) ist eine JSON-Auflistung von Name-Wert-Paaren, die für Apps verwendet werden kann, die eine erweiterte Zielplattform haben. Beispielsweise kann eine APP, die eine Vielzahl von automatisch verknüpften Videos wieder gibt, die AD-Einheits-ID mit der Marke und dem Modell des angezeigten Fahrzeugs ergänzen. Dieser Parameter soll nur von Partnern verwendet werden, die die Genehmigung von Microsoft zur Verwendung von AD-Tags erhalten.

Beim Verweisen auf *adtags*sollten folgende Punkte beachtet werden:

* Dieser Parameter ist eine sehr selten verwendete Option. Der Herausgeber muss vor der Verwendung von adtags eng mit Microsoft zusammenarbeiten.
* Sowohl der Name als auch die Werte müssen für den AD-Dienst vorgegeben werden. AD-Tags sind keine geöffneten Suchbegriffe oder Schlüsselwörter.
* Die maximal unterstützte Anzahl von Tags beträgt 10.
* Tagnamen sind auf 16 Zeichen beschränkt.
* Tagwerte haben maximal 128 Zeichen.

### <a name="requestschedulebyuri"></a>requestschedulebyuri

Diese Methode fordert einen AD-Zeitplan von dem im URI angegebenen nicht-Microsoft-Ad-Server an und fügt ihn in die Zeitachse von **Media Player** ein, die an den **adscheduler** -Konstruktor übergeben wurde. Die AD-Nutzlast, die vom AD-Server zurückgegeben wird, muss den Nutz Last Formaten für die Video-und Videowiedergabe Vorlage (Vast) oder Video Multiple AD Playlist (VMAP) entsprechen.

### <a name="mediatimeout"></a>mediatimeout

Diese Eigenschaft ruft die Anzahl der Millisekunden ab, die das Medium für die über Wiedergabe benötigt, oder legt diese fest. Der Wert 0 teilt dem System mit, dass nie ein Timeout auftritt. Der Standardwert ist 30000 MS (30 Sekunden).

### <a name="playskippedmedia"></a>playskippedmedia

Mit dieser Eigenschaft wird ein **boolescher** Wert abgerufen oder festgelegt, der angibt, ob geplante Medien wiedergegeben werden, wenn der Benutzer einen Zeitpunkt hinter einer geplanten Startzeit überspringt.

Der AD-Client und Media Player erzwingen Regeln in Bezug auf die Aktionen, die beim schnellen weiterleiten und erneuten Laden der primären Videoinhalte durchgeführt werden. In den meisten Fällen erlauben App-Entwickler nicht, dass Ankündigungen vollständig übersprungen werden, sondern eine sinnvolle benutzerfreundliche Benutzerfunktion bereitstellen möchten. Die folgenden beiden Optionen fallen den Anforderungen der meisten Entwickler an:

1. Ermöglicht Endbenutzern das Überspringen von Ankündigungs-Pods bei der Anzeige.
2. Erlauben Sie Benutzern das Überspringen von Ankündigungs-Pods, aber spielen Sie beim Fortsetzen der Wiedergabe den neuesten Pod.

Die **playskippedmedia** -Eigenschaft hat die folgenden Bedingungen:

* Ankündigungen können nach Beginn der Ankündigung nicht mehr ausgelassen oder schnell weitergeleitet werden.
* Alle Ankündigungen in einem Ankündigungs Pod werden wiedergegeben, sobald der Pod gestartet wurde.
* Nach der Wiedergabe wird eine Ankündigung bei den primären Inhalten (Movie, Episode usw.) nicht wiedergegeben. Ankündigungs Marker werden als wiedergegeben markiert oder entfernt.
* Ankündigungen vor dem Rollout können nicht übersprungen werden.

Wenn Sie Inhalte mit Werbung fortsetzen, legen Sie **playskippedmedia** auf **false** fest, um die vorab Ausführung zu überspringen und die Wiedergabe der letzten Werbepause zu verhindern. Legen Sie **playskippedmedia** nach dem Start des Inhalts auf **true** fest, um sicherzustellen, dass die Benutzer nicht schnell durch nachfolgende anzeigen springen können.

> [!NOTE]
> Ein Pod ist eine Gruppe von Werbeeinblendungen, die in einer Sequenz wiedergegeben werden, wie z. b. eine Gruppe von Werbeeinblendungen Weitere Informationen finden Sie in der Definition des IAB Digital Video Ad-diensvorlage (Vast).

### <a name="requesttimeout"></a>requestTimeout

Diese Eigenschaft ruft die Anzahl der Millisekunden ab, die auf eine AD-Anforderungs Antwort gewartet werden soll, bevor ein Timeout auftritt Der Wert 0 teilt dem System mit, dass nie ein Timeout auftritt. Der Standardwert ist 30000 MS (30 Sekunden).

### <a name="schedule"></a>schedule

Diese Eigenschaft ruft die Zeit Plan Daten ab, die vom AD-Server abgerufen wurden. Dieses Objekt enthält die vollständige Daten Hierarchie, die der Struktur der Nutzlast Video Ad-Dienst Vorlage (Vast) oder Video Multiple AD Wiedergabeliste (VMAP) entspricht.

### <a name="onadprogress"></a>onadprogress  

Dieses Ereignis wird ausgelöst, wenn die AD-Wiedergabe Quartil-Prüfpunkte erreicht. Der zweite Parameter des Ereignis Handlers (*EventInfo*) ist ein JSON-Objekt mit den folgenden Membern:

* Status **: der**Status der AD-Wiedergabe (einer der **mediaprogress** -Enumerationswerte, der in AdScheduler.js definiert ist).
* **Clip**: der Videoclip, der wiedergegeben wird. Dieses Objekt ist nicht für die Verwendung im Code vorgesehen.
* **adpackage**: ein Objekt, das den Teil der AD-Nutzlast darstellt, der der wiedergegebenen Werbe Eingabe entspricht. Dieses Objekt ist nicht für die Verwendung im Code vorgesehen.

### <a name="onallcomplete"></a>onallcomplete  

Dieses Ereignis wird ausgelöst, wenn der Hauptinhalt das Ende erreicht und alle geplanten postrollenads ebenfalls beendet werden.

### <a name="onerroroccurred"></a>onerroreingetreten  

Dieses Ereignis wird ausgelöst, wenn der **adscheduler** auf einen Fehler stößt. Weitere Informationen zu den Fehlercode Werten finden Sie unter [errorCode](/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onpodcountdown

Dieses Ereignis wird ausgelöst, wenn eine Werbung abgespielt wird und angibt, wie viel Zeit im aktuellen Pod verbleibt. Der zweite Parameter des Ereignis Handlers (*EventData*) ist ein JSON-Objekt mit den folgenden Membern:

* **restingadtime**: die Anzahl von Sekunden, die für die aktuelle Werbeanzeige verbleiben.
* **restingpodtime**: die Anzahl von Sekunden, die für den aktuellen Pod verbleiben.

> [!NOTE]
> Ein Pod ist eine Gruppe von Werbeeinblendungen, die in einer Sequenz wiedergegeben werden, wie z. b. eine Gruppe von Werbeeinblendungen Weitere Informationen finden Sie in der Definition des IAB Digital Video Ad-diensvorlage (Vast).

### <a name="onpodend"></a>onpodend  

Dieses Ereignis wird ausgelöst, wenn ein Ad-Pod beendet wird. Der zweite Parameter des Ereignis Handlers (*EventData*) ist ein JSON-Objekt mit den folgenden Membern:

* **StartTime**: die Startzeit des Pods in Sekunden.
* **Pod**: ein Objekt, das den Pod darstellt. Dieses Objekt ist nicht für die Verwendung im Code vorgesehen.

### <a name="onpodstart"></a>onpodstart

Dieses Ereignis wird ausgelöst, wenn ein Ad-Pod gestartet wird. Der zweite Parameter des Ereignis Handlers (*EventData*) ist ein JSON-Objekt mit den folgenden Membern:

* **StartTime**: die Startzeit des Pods in Sekunden.
* **Pod**: ein Objekt, das den Pod darstellt. Dieses Objekt ist nicht für die Verwendung im Code vorgesehen.