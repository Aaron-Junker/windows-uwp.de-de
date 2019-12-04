---
description: Im Hintergrund Ihrer UI befinden sich Ihre Geschäfts- und Datenebenen.
title: Portieren von WPSL-Geschäfts-und Daten Ebenen auf UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9548026f0cae4ac414da15ad4ad2aa86f6226cbc
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734915"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Portieren Windows Phone Silverlight-Geschäfts-und Daten Ebenen auf UWP


Das vorherige Thema war [Portieren: E/A, Gerät und App-Modell](wpsl-to-uwp-input-and-sensors.md).

Im Hintergrund Ihrer UI befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für eine UWP (Universelle Windows-Plattform)-App verfügbar](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)). Sie können also davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt.

## <a name="asynchronous-methods"></a>Asynchrone Methoden

Einer der Hauptvorteile der universellen Windows-Plattform (UWP) besteht darin, das sie Ihnen die Erstellung von Apps ermöglicht, die wirklich und durchgehend reaktionsfähig sind. Animationen sind immer reibungslos, und Touchinteraktionen wie Verschieben und Wischen erfolgen sofort und ohne Verzögerung, wodurch der Eindruck entsteht, dass die UI quasi an Ihrem Finger „klebt“. Um dies zu erreichen, wurden alle UWP-APIs, deren Ausführung innerhalb von 50 Millisekunden nicht garantiert werden kann, als asynchron konzipiert und mit dem Namenssuffix **Async** versehen. Die Rückgabe Ihres UI-Threads nach dem Aufruf einer **Async**-Methode erfolgt sofort, und die Arbeit wird in einem anderen Thread ausgeführt. Die Nutzung einer **Async**-Methode ist in Bezug auf die Syntax mit dem C#-Operator **await**, JavaScript-Promise-Objekten und C++-Fortsetzungen sehr einfach. Weitere Informationen finden Sie unter [Asynchrone Programmierung](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).

## <a name="background-processing"></a>Hintergrundverarbeitung

Eine Windows Phone Silverlight-App kann ein verwaltetes **scheduledtaskagent** -Objekt verwenden, um eine Aufgabe auszuführen, während sich die APP nicht im Vordergrund befindet. UWP-Apps verwenden auf ähnliche Weise die [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)-Klasse zum Erstellen und Registrieren von Hintergrundaufgaben. Sie definieren eine Klasse, die die Arbeit Ihrer Hintergrundaufgabe implementiert. Das System führt die Hintergrundaufgabe regelmäßig aus, indem die [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode der Klasse zum Ausführen der Arbeit aufgerufen wird. Denken Sie bei einer UWP-App daran, die Deklaration **Hintergrundaufgaben** im App-Paketmanifest festzulegen. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Um große Datendateien im Hintergrund zu übertragen, verwendet eine Windows Phone Silverlight-APP die **backgroundtransferservice** -Klasse. Eine UWP-App verwendet zu diesem Zweck APIs im [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer)-Namespace. Die Features initiieren Übertragungen anhand eines ähnlichen Musters, die neue API verfügt aber über verbesserte Funktionen und eine höhere Leistung. Weitere Informationen finden Sie unter [Übertragen von Daten im Hintergrund](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

Eine Windows Phone Silverlight-App verwendet die verwalteten Klassen im **Microsoft. Phone. backgroundaudionamespace** , um Audioinhalte wiederzugeben, während sich die APP nicht im Vordergrund befindet. Die UWP verwendet das Windows Phone Store-App-Modell. Weitere Informationen finden Sie unter [Hintergrundaudio](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) und im [Hintergrundaudio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio)-Beispiel.

## <a name="cloud-services-networking-and-databases"></a>Clouddienste, Netzwerk und Datenbanken

Das Hosten von Daten und App-Diensten in der Cloud ist mit Azure möglich. Weitere Informationen finden Sie unter [Erste Schritte mit Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/). Für Lösungen, die sowohl Online- als auch Offlinedaten erfordern: [Verwendung der Offlinedatensynchronisierung in Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

Die UWP verfügt über eine Teilunterstützung für die **System.Net.HttpWebRequest**-Klasse, aber die **System.Net.WebClient**-Klasse wird nicht unterstützt. Die empfohlene modernere Alternative ist die [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)), wenn Ihr Code auf andere Plattformen mit .NET-Unterstützung portierbar sein soll). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen.

UWP-Apps bieten zurzeit keine integrierte Unterstützung für datenintensive Szenarien wie beispielsweise Branchenszenarien. Sie können jedoch SQLite für lokale Transaktionsdatenbankdienste verwenden. Weitere Informationen finden Sie unter [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Übergeben Sie absolute URIs, keine relativen URIs, an Windows-Runtime-Typen. Weitere Informationen finden Sie unter [Übergeben eines URI an die Windows-Runtime](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Launcher und Chooser

Mit Launcher und Auswahlmöglichkeiten (im **Microsoft. Phone. Tasks** -Namespace) kann eine Windows Phone Silverlight-App mit dem Betriebssystem interagieren, um allgemeine Vorgänge auszuführen, z. b. das Verfassen einer e-Mail, das Auswählen eines Fotos oder das Freigeben bestimmter Arten von Daten mit einer anderen APP. Suchen Sie im Thema [Windows Phone Silverlight zu Windows 10-Namespace und Klassen](wpsl-to-uwp-namespace-and-class-mappings.md) Zuordnungen nach **Microsoft. Phone. Tasks** , um den entsprechenden UWP-Typ zu suchen. Diese reichen von ähnlichen Mechanismen (so genannten Launchern und Pickern) bis zur Implementierung eines Vertrags zum Teilen von Daten zwischen Apps.

Eine Windows Phone Silverlight-App kann in einen Ruhezustand versetzt werden oder sogar in einem tombstoning, wenn z. b. die Fotoauswahl Aufgabe verwendet wird. Eine UWP-App bleibt aktiv und wird weiter ausgeführt, wenn die [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Klasse verwendet wird.

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monetisierung (Testmodus und In-App-Einkäufe)

Eine Windows Phone Silverlight-App kann die UWP [**currentapp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse für den größten Teil des Test Modus und in-App-Kauf Funktionalität verwenden, sodass der Code nicht portiert werden muss. Aber eine Windows Phone Silverlight-App Ruft " **marketplacedetailtask. Show** " auf, um die APP für den Kauf bereitzustellen:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Portieren Sie diesen Code zum Aufrufen der UWP [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)-Methode:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Falls Sie über Code verfügen, der Ihre Features für App-Kauf und In-App-Einkäufe zu Testzwecken simuliert, können Sie ihn portieren, um stattdessen die [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)-Klasse zu verwenden.

## <a name="notifications-for-tile-or-toast-updates"></a>Benachrichtigungen für Kachel- oder Popupaktualisierungen

Benachrichtigungen sind eine Erweiterung des Push-Benachrichtigungs Modells für Windows Phone Silverlight-apps. Wenn Sie eine Benachrichtigung vom Windows-Pushbenachrichtigungsdienst (WNS) empfangen, können Sie die Informationen mit einer Kachelaktualisierung oder einem Popup in der Benutzeroberfläche anzeigen. Informationen zum Portieren des Benutzeroberflächenteils Ihrer Benachrichtigungsfeatures finden Sie unter [Kacheln und Popups](w8x-to-uwp-porting-xaml-and-ui.md).

Ausführlichere Informationen zur Verwendung von Benachrichtigungen in UWP-Apps finden Sie unter [Senden von Popupbenachrichtigungen](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Informationen und Lernprogramme zur Verwendung von Kacheln, Popups, Signalen, Bannern und Benachrichtigungen in einer Windows-Runtime-App mit C++, C# oder Visual Basic finden Sie unter [Verwenden von Kacheln, Signalen und Popupbenachrichtigungen](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Speicher (Dateizugriff)

Windows Phone Silverlight-Code, der App-Einstellungen als Schlüssel-Wert-Paare in isoliertem Speicher speichert, kann problemlos portiert werden. Im folgenden finden Sie ein Beispiel vor und nach dem Windows Phone Silverlight-Version:

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

Und hier die UWP-Entsprechung:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Obwohl eine Teilmenge des **Windows. Storage** -Namespace für Sie verfügbar ist, führen viele Windows Phone Silverlight-apps Datei-e/a-Vorgänge mit der **IsolatedStorageFile** -Klasse aus, da Sie länger unterstützt wird. Vorausgesetzt, dass " **IsolatedStorageFile** " verwendet wird, ist hier ein Beispiel für das Schreiben und Lesen einer Datei, zuerst die Windows Phone Silverlight-Version:

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

Und hier die gleiche Funktionalität mit der UWP:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Eine Windows Phone Silverlight-APP hat schreibgeschützten Zugriff auf die optionale SD-Karte. Eine UWP-App hat Lese-/Schreibzugriff auf die SD-Karte. Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Informationen zum Zugriff auf Fotos, Musik und Videodateien in einer UWP-App finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Weitere Informationen finden Sie unter [Dateien, Ordner und Bibliotheken](https://docs.microsoft.com/windows/uwp/files/index).

Das nächste Thema ist [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Verwandte Themen

* [Namespace-und Klassen Zuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md)
 

