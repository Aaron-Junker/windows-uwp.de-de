---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Hier erfahren Sie, wie Sie die AdScheduler-Klasse verwenden, um Anzeigen in Videoinhalten anzuzeigen.
title: Anzeigen von Werbung in Videoinhalten
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Video, Planer, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 69fef2bc5deb21be8685badb0cf18f38769170cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603845"
---
# <a name="show-ads-in-video-content"></a>Anzeigen von Werbung in Videoinhalten

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die **AdScheduler**-Klasse zum Anzeigen von Videoinhalten in einer App für die universelle Windows-Plattform (UWP) verwenden, die mit JavaScript und HTML geschrieben wurde.

> [!NOTE]
> Diese Funktion wird derzeit nur für UWP-Apps unterstützt, die mit JavaScript und HTML geschrieben werden.

**AdScheduler** funktioniert mit progressiven Medien und Streamingmedien und nutzt Video Ad Serving Template (VAST) 2.0/3.0 nach IAB-Standard und VMAP-Nutzlastformate. Durch die Verwendung von Standards ist **AdScheduler** agnostisch gegenüber dem Anzeigendienst, mit dem die Interaktion erfolgt.

Werbung für Videoinhalte variiert in Abhängigkeit davon, ob das Programm kürzer als zehn Minuten (kurzes Format) oder länger als zehn Minuten (langes Format) ist. Die Einrichtung des letzteren Formats für den Dienst ist zwar komplizierter, aber es besteht eigentlich kein Unterschied darin, wie der clientseitige Code geschrieben wird. Wenn **AdScheduler** eine VAST-Nutzlast mit einer einzelnen Werbeanzeige anstelle eines Manifests empfängt, wird dies so behandelt, als ob das Manifest eine einzelne Pre-Roll-Anzeige (eine Unterbrechung bei 00:00) aufgerufen hat.

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie das [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp) mit Visual Studio 2015 oder einer neueren Version.

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

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
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
    > Diese Zeile muss im Abschnitt `<head>` nach der Datei „main.js“ platziert werden. Andernfalls tritt bei der Erstellung des Projekts ein Fehler auf.

6.  Fügen Sie der Datei „main.js“ des Projekts Code hinzu, mit dem ein neues **AdScheduler**-Objekt erstellt wird. Übergeben Sie den **MediaPlayer**, mit dem Ihre Videoinhalte gehostet werden. Der Code muss so angeordnet werden, dass er nach [WinJS.UI.processAll](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh440975) ausgeführt wird.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Verwenden Sie die Methode **requestSchedule** oder **requestScheduleByUrl** des **AdScheduler**-Objekts, um einen Anzeigenzeitplan vom Server anzufordern und in die **MediaPlayer**-Zeitachse einzufügen. Dann können Sie die Videomedien wiedergeben.

    * Wenn Sie Microsoft-Partner sind und eine Berechtigung zum Anfordern eines Anzeigenzeitplans vom Microsoft-Anzeigenserver erhalten haben, können Sie **requestSchedule** verwenden und die IDs für die Anwendung und die Anzeigeneinheit angeben, die von Ihrem Microsoft-Vertreter bereitgestellt wurden.

        Diese Methode hat die Form einer [Zusage](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript). Das ist ein asynchrones Konstrukt, in dem zwei Funktionszeiger übergeben werden: Ein Zeiger, um die **OnComplete**-Funktion aufzurufen, wenn die Zusage erfolgreich abgeschlossen wurde, und ein Zeiger zum Aufrufen der **OnError**-Funktion aufrufen, wenn ein Fehler aufgetreten ist. In der **OnComplete**-Funktion starten Sie die Wiedergabe Ihres Videos. Die Anzeige wird zum geplanten Zeitpunkt abgespielt. In Ihrer **OnError**-Funktion behandeln Sie den Fehler und starten dann die Wiedergabe des Videos. Ihr Video wird ohne eine Anzeige wiedergegeben. Das Argument der **OnError**-Funktion ist ein Objekt, das die folgenden Member enthält.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Verwenden Sie **requestScheduleByUrl**zum Anfordern eines Anzeigenzeitplans von einem anderen Server als dem Microsoft-Anzeigenserver, und übergeben Sie die Server-URI. Diese Methode hat auch die Form einer **Zusage**, die Zeiger für die Funktionen **OnComplete** und **OnError** übernimmt. Die Anzeigennutzlast, die vom Server zurückgegeben wird, muss dem Nutzlastformat Video Ad Serving Template (VAST) oder Video Multiple Ad Playlist (VMAP) entsprechen.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Sie sollten warten, bis die Ausführung von **requestSchedule** oder **requestScheduleByUrl** beendet ist, bevor Sie den primären Videoinhalt im **MediaPlayer** starten. Die Medienwiedergabe vor Beendigung von **requestSchedule** würde im Fall einer Pre-Roll-Anzeige dazu führen, dass die Pre-Roll den primären Videoinhalt unterbricht. Sie müssen **play** auch dann aufrufen, wenn die Funktion mit einem Fehler endet, weil **AdScheduler** den **MediaPlayer** anweist, die Anzeige(n) zu überspringen und sofort mit dem Inhalt fortzufahren. Unter Umständen besteht bei Ihnen auch eine andere geschäftliche Anforderung, z. B. das Einfügen einer integrierten Anzeige, falls eine Anzeige nicht erfolgreich von einem Remotestandort abgerufen werden kann.

8.  Während der Wiedergabe können Sie zusätzliche Ereignisse verarbeiten, mit denen Ihre App den Status bzw. Fehler nachverfolgen kann, die nach dem ersten Anzeigenabgleich auftreten. Der folgende Code zeigt einige dieser Ereignisse, einschließlich **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** und **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>AdScheduler-Member

Dieser Abschnitt enthält einige Details zu den Membern des **AdScheduler**-Objekts. Weitere Informationen zu diesen Membern finden Sie in den Kommentaren und Definitionen in der Datei „AdScheduler.js” in Ihrem Projekt.

### <a name="requestschedule"></a>requestSchedule

Diese Methode fordert einen Anzeigenzeitplan vom Microsoft-Anzeigenserver an und fügt ihn in die **MediaPlayer**-Zeitleiste ein, der an den **AdScheduler**-Konstruktor übergeben wurde.

Der optionale dritte Parameter (*adTags*) ist eine JSON-Sammlung von Name/Wert-Paaren, die für Apps mit erweitertem Targeting verwendet werden kann. Beispielsweise kann eine App, die eine Vielzahl von automatisch verknüpften Videos abspielt, die Anzeigeneinheit-ID um die Marke und das Modell des angezeigten Autos ergänzen. Dieser Parameter ist nur für Partner gedacht, die von Microsoft die Genehmigung zur Verwendung von Anzeigentags erhalten.

Die folgenden Punkte sollten in Bezug auf *adTags* beachtet werden:

* Dieser Parameter ist eine sehr selten verwendete Option. Der Herausgeber muss vor der Verwendung von adTags eng mit Microsoft zusammenarbeiten.
* Sowohl die Namen als auch die Werte müssen im Anzeigenservice vorher festgelegt werden. Anzeigentags sind keine abgeschlossenen Suchbegriffe oder Schlüsselwörter.
* Es werden maximal 10 Tags unterstützt.
* Tagnamen sind auf 16 Zeichen beschränkt.
* Tagwerte dürfen maximal 128 Zeichen umfassen.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Diese Methode fordert einen Anzeigenzeitplan vom einem Anzeigenserver an (der in der URI angegeben ist und nicht Microsoft gehört) und fügt ihn in die **MediaPlayer**-Zeitleiste ein, der an den **AdScheduler**-Konstruktor übergeben wurde. Die Anzeigennutzlast, die vom Anzeigenserver zurückgegeben wird, muss dem Nutzlastformat Video Ad Serving Template (VAST) oder Video Multiple Ad Playlist (VMAP) entsprechen.

### <a name="mediatimeout"></a>mediaTimeout

Mit dieser Eigenschaft wird die Anzahl der Millisekunden abgerufen oder festgelegt, die das Medium abspielbar sein muss. Ein Wert 0 informiert das System darüber, dass es keine Zeitüberschreitung gibt. Der Standardwert ist 30000 ms (30 Sekunden).

### <a name="playskippedmedia"></a>playSkippedMedia

Mit dieser Eigenschaft wird ein **Boolean**-Wert abgerufen oder festgelegt, der angibt, ob geplante Medien wiedergegeben werden, wenn der Benutzer zu einem Zeitpunkt nach einer geplanten Startzeit vorspringt.

Der Anzeigenclient und der Mediaplayer erzwingen Regeln darüber, was mit den Anzeigen während des schnellen Vor- und Rücklaufs des primären Videoinhalts geschieht. In den meisten Fällen erlauben App-Entwickler nicht, dass Anzeigen vollständig übersprungen werden, sondern sie sollen dem Nutzer eine angemessene Erfahrung bieten. Die beiden folgenden Optionen entsprechen den Anforderungen der meisten Entwickler:

1. Endbenutzer dürfen Anzeigenblöcke nach Belieben überspringen.
2. Benutzern dürfen Anzeigenblöcke überspringen, aber es wird der aktuellste Block angezeigt, wenn die Wiedergabe fortgesetzt wird.

Für die Eigenschaft **playSkippedMedia** gelten folgende Bedingungen:

* Anzeigen können nicht übersprungen oder vorgespult, nachdem sie begonnen haben.
* Alle Anzeigen in einem Anzeigenblock werden wiedergegeben, nachdem der Block gestartet wurde.
* Nach der Wiedergabe wird ein Anzeige während des primären Inhalts (Film, Folge usw.) nicht erneut abgespielt. Anzeigenmarkierungen erhalten die Werte „wiedergegeben” oder „entfernt”.
* Pre-Rollout-Anzeigen können nicht übersprungen werden.

Wenn Sie die Wiedergabe von Inhalten fortsetzen, die Werbung enthalten, legen Sie **playSkippedMedia** auf **false** fest, um Pre-Rolls zu überspringen und die Wiedergabe der letzten Anzeigenunterbrechung zu verhindern. Nach dem Start des Inhalts legen Sie **playSkippedMedia** auf **true** fest, um sicherzustellen, dass Benutzer nachfolgende Anzeigen nicht vorspulen können.

> [!NOTE]
> Ein Block (Pod) ist eine Gruppe von Anzeigen, die als Sequenz wiedergegeben werden, z. B. während einer Werbepause. Weitere Informationen finden Sie in der IAB Digital Video Ad Serving Template (VAST)-Spezifikation.

### <a name="requesttimeout"></a>requestTimeout

Diese Eigenschaft ruft ab oder legt die Anzahl der Millisekunden, die für eine Ad-Anforderung-Antwort, bevor ein Timeout gewartet. Ein Wert 0 informiert das System darüber, dass es keine Zeitüberschreitung gibt. Der Standardwert ist 30000 ms (30 Sekunden).

### <a name="schedule"></a>schedule

Diese Eigenschaft ruft die Zeitplandaten vom Anzeigenserver ab. Dieses Objekt enthält die vollständige Datenhierarchie, die der Struktur der Video Ad Serving-Vorlagen (VAST)-Nutzlast oder der Video-Multiple-Ad-Playlists (VMAP)-Nutzlast entspricht.

### <a name="onadprogress"></a>onAdProgress  

Dieses Ereignis wird ausgelöst, wenn die Anzeigenwiedergabe Prüfpunktquartile erreicht. Der zweite Parameter für den Ereignishandler (*eventInfo*) ist ein JSON-Objekt mit folgenden Elementen:

* **Fortschritt**: Der Status der Ad-Wiedergabe (eines der **MediaProgress** Enum-Werte, die in AdScheduler.js definiert).
* **clip**: Der Videoclip wiedergegeben wird. Dieses Objekt ist nicht dafür vorgesehen, im Code verwendet zu werden.
* **adPackage**: Ein Objekt, das den Teil der Ad-Nutzlast, die entspricht dem AD darstellt, der wiedergegeben wird. Dieses Objekt ist nicht dafür vorgesehen, im Code verwendet zu werden.

### <a name="onallcomplete"></a>onAllComplete  

Dieses Ereignis wird ausgelöst, wenn der Hauptinhalt das Ende erreicht und alle geplanten Post-Roll-Anzeigen ebenfalls beendet werden.

### <a name="onerroroccurred"></a>onErrorOccurred  

Dieses Ereignis wird ausgelöst, wenn im **AdScheduler** ein Fehler auftritt. Weitere Informationen zu den Fehlercodewerten finden Sie unter [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Dieses Ereignis wird ausgelöst, wenn eine Anzeige abgespielt wird, und gibt an, wie viel Zeit im aktuellen Pod verbleibt. Der zweite Parameter für den Ereignishandler (*eventData*) ist ein JSON-Objekt mit folgenden Elementen:

* **remainingAdTime**: Die Anzahl der Sekunden, die Links für die aktuelle Anzeige.
* **remainingPodTime**: Die Anzahl der Sekunden, die Links für den aktuellen Pod.

> [!NOTE]
> Ein Block (Pod) ist eine Gruppe von Anzeigen, die als Sequenz wiedergegeben werden, z. B. während einer Werbepause. Weitere Informationen finden Sie in der IAB Digital Video Ad Serving Template (VAST)-Spezifikation.

### <a name="onpodend"></a>onPodEnd  

Dieses Ereignis wird ausgelöst, wenn ein Anzeigenblock endet. Der zweite Parameter für den Ereignishandler (*eventData*) ist ein JSON-Objekt mit folgenden Elementen:

* **startTime**: Die Pod-Startzeit, in Sekunden.
* **pod**: Ein Objekt, das den Pod darstellt. Dieses Objekt ist nicht dafür vorgesehen, im Code verwendet zu werden.

### <a name="onpodstart"></a>onPodStart

Dieses Ereignis wird ausgelöst, wenn ein Anzeigenblock startet. Der zweite Parameter für den Ereignishandler (*eventData*) ist ein JSON-Objekt mit folgenden Elementen:

* **startTime**: Die Pod-Startzeit, in Sekunden.
* **pod**: Ein Objekt, das den Pod darstellt. Dieses Objekt ist nicht dafür vorgesehen, im Code verwendet zu werden.
