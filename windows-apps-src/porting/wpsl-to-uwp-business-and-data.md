---
description: Im Hintergrund Ihrer Benutzeroberfläche befinden sich Ihre Geschäfts- und Datenebenen.
title: Windows Phone Silverlight Business und Datenebenen für die UWP Portieren
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3e5b97c236f71c95cdff9c56ccc205d3b0fbde5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661685"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Windows Phone Silverlight Business und Datenebenen für die UWP Portieren


Das vorherige Thema war [Portieren: E/A, Gerät und App-Modell](wpsl-to-uwp-input-and-sensors.md).

Im Hintergrund Ihrer Benutzeroberfläche befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für eine UWP (Universelle Windows-Plattform)-App verfügbar](https://msdn.microsoft.com/library/windows/apps/br211369). Sie können also davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt.

## <a name="asynchronous-methods"></a>Asynchrone Methoden

Einer der Hauptvorteile der universellen Windows-Plattform (UWP) besteht darin, das sie Ihnen die Erstellung von Apps ermöglicht, die wirklich und durchgehend reaktionsfähig sind. Animationen sind immer reibungslos, und Touchinteraktionen wie Verschieben und Wischen erfolgen sofort und ohne Verzögerung, wodurch der Eindruck entsteht, dass die UI quasi an Ihrem Finger „klebt“. Um dies zu erreichen, wurden alle UWP-APIs, deren Ausführung innerhalb von 50 Millisekunden nicht garantiert werden kann, als asynchron konzipiert und mit dem Namenssuffix **Async** versehen. Die Rückgabe Ihres UI-Threads nach dem Aufruf einer **Async**-Methode erfolgt sofort, und die Arbeit wird in einem anderen Thread ausgeführt. Die Nutzung einer **Async**-Methode ist in Bezug auf die Syntax mit dem C#-Operator **await**, JavaScript-Promise-Objekten und C++-Fortsetzungen sehr einfach. Weitere Informationen finden Sie unter [Asynchrone Programmierung](https://msdn.microsoft.com/library/windows/apps/mt187335).

## <a name="background-processing"></a>Hintergrundverarbeitung

Eine Windows Phone Silverlight-app können ein verwaltetes **ScheduledTaskAgent** Objekt um eine Aufgabe auszuführen, während die app nicht im Vordergrund ist. UWP-Apps verwenden auf ähnliche Weise die [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)-Klasse zum Erstellen und Registrieren von Hintergrundaufgaben. Sie definieren eine Klasse, die die Arbeit Ihrer Hintergrundaufgabe implementiert. Das System führt die Hintergrundaufgabe regelmäßig aus, indem die [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode der Klasse zum Ausführen der Arbeit aufgerufen wird. Denken Sie bei einer UWP-App daran, die Deklaration **Hintergrundaufgaben** im App-Paketmanifest festzulegen. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/mt299103).

Zur Übertragung von großen Dateien im Hintergrund eine Windows Phone Silverlight-app verwendet die **BackgroundTransferService** Klasse. Eine UWP-App verwendet zu diesem Zweck APIs im [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)-Namespace. Die Features initiieren Übertragungen anhand eines ähnlichen Musters, die neue API verfügt aber über verbesserte Funktionen und eine höhere Leistung. Weitere Informationen finden Sie unter [Übertragen von Daten im Hintergrund](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975).

Eine Windows Phone Silverlight-app verwendet die verwalteten Klassen in der **Microsoft.Phone.BackgroundAudio** Namespace zum Abspielen von Audiodateien, während die app befindet sich nicht im Vordergrund. Die UWP verwendet das Windows Phone Store-App-Modell. Weitere Informationen finden Sie unter [Hintergrundaudio](https://msdn.microsoft.com/library/windows/apps/mt282140) und im [Hintergrundaudio](https://go.microsoft.com/fwlink/p/?linkid=619997)-Beispiel.

## <a name="cloud-services-networking-and-databases"></a>Clouddienste, Netzwerk und Datenbanken

Das Hosten von Daten und App-Diensten in der Cloud ist mit Azure möglich. Weitere Informationen finden Sie unter [Erste Schritte mit Mobile Services](https://go.microsoft.com/fwlink/p/?LinkID=403138). Lösungen, die online und offline-Daten erfordern finden Sie unter: [Verwenden der Synchronisierung von Offlinedaten in Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

Die UWP verfügt über eine Teilunterstützung für die **System.Net.HttpWebRequest**-Klasse, aber die **System.Net.WebClient**-Klasse wird nicht unterstützt. Die empfohlene modernere Alternative ist die [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)-Klasse (oder [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx), wenn Ihr Code auf andere Plattformen mit .NET-Unterstützung portierbar sein soll). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) verwendet, um eine HTTP-Anforderung darzustellen.

UWP-Apps bieten zurzeit keine integrierte Unterstützung für datenintensive Szenarien wie beispielsweise Branchenszenarien. Sie können jedoch SQLite für lokale Transaktionsdatenbankdienste verwenden. Weitere Informationen finden Sie unter [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Übergeben Sie absolute URIs, keine relativen URIs, an Windows-Runtime-Typen. Weitere Informationen finden Sie unter [Übergeben eines URI an die Windows-Runtime](https://msdn.microsoft.com/library/hh763341.aspx).

## <a name="launchers-and-choosers"></a>Launcher und Chooser

Mit Auswahl- und Startprogramme (finden Sie in der **Microsoft.Phone.Tasks** Namespace), kann eine Windows Phone Silverlight-app mit dem Betriebssystem allgemeiner Vorgänge wie das Verfassen einer e-Mails, und wählen ein Foto interagieren oder bestimmte Arten von Daten und eine andere app freigeben. Suchen Sie nach **Microsoft.Phone.Tasks** im Thema [Windows Phone Silverlight zu Zuordnungen für Windows 10 Namespace- und Klassennamen](wpsl-to-uwp-namespace-and-class-mappings.md) den entsprechenden UWP-Typ gefunden. Diese reichen von ähnlichen Mechanismen (so genannten Launchern und Pickern) bis zur Implementierung eines Vertrags zum Teilen von Daten zwischen Apps.

Eine Windows Phone Silverlight-app kann in einen Ruhezustand versetzt oder sogar ein Tombstoning abgelegt werden, Verwendung, z. B. die Foto-Auswahl-Aufgabe. Eine UWP-App bleibt aktiv und wird weiter ausgeführt, wenn die [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse verwendet wird.

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monetisierung (Testmodus und In-App-Einkäufe)

Eine Windows Phone Silverlight-app können Sie die UWP [**CurrentApp** ](https://msdn.microsoft.com/library/windows/apps/hh779765) Klasse Sie für die meisten seiner Testmodus und in-app-Funktionalität, erwerben, sodass Code nicht portiert werden muss. Eine Windows Phone Silverlight-app ruft jedoch **MarketplaceDetailTask.Show** die app zum Kauf anbieten:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Portieren Sie diesen Code zum Aufrufen der UWP [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)-Methode:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Falls Sie über Code verfügen, der Ihre Features für App-Kauf und In-App-Einkäufe zu Testzwecken simuliert, können Sie ihn portieren, um stattdessen die [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)-Klasse zu verwenden.

## <a name="notifications-for-tile-or-toast-updates"></a>Benachrichtigungen für Kachel- oder Popupaktualisierungen

Benachrichtigungen sind eine Erweiterung der Benachrichtigung Pushmodell für Windows Phone Silverlight-apps. Wenn Sie eine Benachrichtigung vom Windows-Pushbenachrichtigungsdienst (WNS) empfangen, können Sie die Informationen mit einer Kachelaktualisierung oder einem Popup in der Benutzeroberfläche anzeigen. Informationen zum Portieren des Benutzeroberflächenteils Ihrer Benachrichtigungsfeatures finden Sie unter [Kacheln und Popups](w8x-to-uwp-porting-xaml-and-ui.md).

Ausführlichere Informationen zur Verwendung von Benachrichtigungen in UWP-Apps finden Sie unter [Senden von Popupbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266).

Informationen und Lernprogramme zur Verwendung von Kacheln, Popups, Signalen, Bannern und Benachrichtigungen in einer Windows-Runtime-App mit C++, C# oder Visual Basic finden Sie unter [Verwenden von Kacheln, Signalen und Popupbenachrichtigungen](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## <a name="storage-file-access"></a>Speicher (Dateizugriff)

Windows Phone Silverlight-Code, in der app-Einstellungen als Schlüssel-Wert-Paaren im isolierten Speicher gespeichert werden einfache Weise portiert werden. Hier ist ein Beispiel vorher-nachher-zunächst die Windows Phone Silverlight-Version:

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

Obwohl es eine Teilmenge von der **Windows.Storage** -Namespace ist ihnen zur Verfügung, viele Windows Phone Silverlight-apps führen Datei-e/as mit der **IsolatedStorageFile** Klasse, da es für unterstützt wird Je länger. Vorausgesetzt, dass **IsolatedStorageFile** wird verwendet, hier ist ein vorher-nachher-Beispiel schreiben und Lesen einer Datei, zuerst die Windows Phone Silverlight-Version:

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

Eine Windows Phone Silverlight-app verfügt über schreibgeschützten Zugriff auf die optionale SD-Karte. Eine UWP-App hat Lese-/Schreibzugriff auf die SD-Karte. Weitere Informationen finden Sie unter [Zugreifen auf die SD-Karte](https://msdn.microsoft.com/library/windows/apps/mt188699).

Informationen zum Zugriff auf Fotos, Musik und Videodateien in einer UWP-App finden Sie unter [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](https://msdn.microsoft.com/library/windows/apps/mt188703).

Weitere Informationen finden Sie unter [Dateien, Ordner und Bibliotheken](https://msdn.microsoft.com/library/windows/apps/mt185399).

Das nächste Thema ist [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Verwandte Themen

* [Namespace und Klasse-Zuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md)
 

