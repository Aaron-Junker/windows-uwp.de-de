---
title: Deep-Link von einer Hintergrund-app in Cortana zu einer Vordergrund-App-Cortana UWP-Entwurf und-Entwicklung
description: Stellen Sie Deep-Links von einer Hintergrund-App in **Cortana** bereit, die die App in einem bestimmten Zustand oder Kontext im Vordergrund starten.
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 5d0f841ed653adac6770089a9ec9c1be0335e346
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057836"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>Deep-Link von einer Hintergrund-app in Cortana zu einer Vordergrund-App

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.

Stellen Sie Deep-Links von einer Hintergrund-App in **Cortana** bereit, die die App in einem bestimmten Zustand oder Kontext im Vordergrund starten.

> [!NOTE]
> Sowohl **Cortana** als auch der Hintergrund-App-Dienst werden beendet, wenn die Vordergrund-App gestartet wird.

Standardmäßig wird auf dem **Cortana** Abschlussbildschirm wie hier dargestellt ein Deep-Link angezeigt („Zu AdventureWorks“), Sie können jedoch in verschiedenen anderen Bildschirmen Deep-Links anzeigen.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="Screenshot der Cortana-Hintergrund-App-Vervollständigung für eine bevorstehende Fahrt":::

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>Übersicht

Benutzer können wie folgt über **Cortana** auf Ihre App zugreifen:

- Aktivieren der App als Vordergrund-App (siehe [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)).
- Verfügbar machen spezifischer Funktionen als Hintergrund-App-Dienst (siehe [Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen](cortana-launch-a-background-app-with-voice-commands.md)).
- Erstellen von Deep-Links zu bestimmten Seiten, Inhalten, Zuständen oder Kontext.

In diesem Abschnitt wird das Erstellen von Deep-Links erläutert.

Das Erstellen von Deep-Links ist hilfreich, wenn Cortana und Ihr App-Dienst als Gateway zu Ihrer vollfunktionalen dienen (anstatt den Benutzer zum Starten der App über das Startmenü aufzufordern), oder um Zugriff auf umfangreichere Details und Funktionen in Ihrer App zu bieten, was über Cortana nicht möglich ist. Das Erstellen von Deep-Links ist eine weitere Möglichkeit zur Steigerung der Nutzbarkeit und zum Bewerben Ihrer App.

Es gibt drei Möglichkeiten zum Bereitstellen von Deep-Links:

- Ein Link „Zur &lt;App&gt;“ in verschiedenen **Cortana**-Bildschirmen.
- Ein in eine Inhaltskachel eingebetteter Link in verschiedenen **Cortana**-Bildschirmen.
- Programmgesteuertes Starten der Vordergrund-App aus dem Hintergrund-App-Dienst.

## <a name="go-to-ltappgt-deep-link"></a>Deep-Link „Gehe zu &lt;App&gt;“

**Cortana** zeigt in den meisten Bildschirmen einen Deep-Link „Zur &lt;App&gt;“ unterhalb der Inhaltskarte an.

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="Screenshot des Deep-Links &quot;Gehe zu app&quot; in der Hintergrundbild Schirm-app.":::

Sie können ein Start-Argument für diesen Link bereitstellen, mit dem Ihre App in einem ähnlichen Kontext wie der App-Dienst geöffnet wird. Wenn Sie kein Start-Argument angeben, wird der Hauptbildschirm der App gestartet.

In diesem Beispiel aus AdventureWorksVoiceCommandService.cs des Beispiels **AdventureWorks** übergeben wir das angegebene Ziel (`destination`) an die SendCompletionMessageForDestination-Methode, die alle übereinstimmenden Reisen abruft und einen Deep-Link zu der App bereitstellt.

Zuerst erstellen wir eine [**voicecommandusermessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ( ```userMessage``` ), die von **Cortana** gesprochen und im **Cortana** -Zeichenbereich angezeigt wird. Anschließend wird ein [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)-Listenobbjekt zum Anzeigen der Sammlung von Ergebniskarten auf der Canvas erstellt.

Diese beiden Objekte werden dann an die [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Methode des [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Objekts (`response`) übergeben. Anschließend legen wir den [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Eigenschaftswert des Antwortobjekts auf den Wert des Ziels (`destination`) fest, das an diese Funktion übergeben wurde. Wenn ein Benutzer in der Cortana-Canvas auf eine Inhaltskachel tippt, werden die Parameterwerte über das Antwortobjekt an die App übergeben.

Abschließend rufen wir die [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Methode von [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) auf.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="content-tile-deep-link"></a>Deep-Link-Inhaltskachel

Sie können Deep-Links zu Inhaltskarten auf verschiedenen **Cortana**-Bildschirmen hinzufügen.

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Screenshot des Cortana-Canvas für den End-to-End-Cortana-Hintergrund-App-Flow unter Verwendung von AdventureWorks bevorstehende Fahrt mit der Übergabe von":::*AdventureWorks "bevorstehende Fahrt" mit Übergabe Bildschirm*

Wie bei den Links „Gehe zu &lt;App&gt;“ können Sie ein Start-Argument angeben, um die App mit einem ähnlichen Kontext wie der App-Dienst zu öffnen. Wenn Sie kein Start-Argument angeben, wird die Inhaltskachel nicht mit Ihrer App verknüpft.

In diesem Beispiel aus AdventureWorksVoiceCommandService.cs des Beispiels **AdventureWorks** übergeben wir das angegebene Ziel an die SendCompletionMessageForDestination-Methode, die alle übereinstimmenden Reisen abruft und Inhaltskarten mit Deep-Links zu der App bereitstellt.

Zuerst erstellen wir eine  [**voicecommandusermessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ( ```userMessage``` ), die von **Cortana** gesprochen und im **Cortana** -Zeichenbereich angezeigt wird. Anschließend wird ein [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)-Listenobbjekt zum Anzeigen der Sammlung von Ergebniskarten auf der Canvas erstellt. 

Diese beiden Objekte werden dann an die [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Methode des [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Objekts (```response```) übergeben. Anschließend legen wir als Wert für die [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Eigenschaft den Wert des Ziels in dem Sprachbefehl fest.

Abschließend rufen wir die [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Methode von [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) auf.
Hier fügen wir zwei Inhaltskacheln mit unterschiedlichen [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Parameterwerten zu einer [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)-Liste hinzu, die im [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Aufruf des [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Objekts verwendet wird.

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="programmatic-deep-link"></a>Programmgesteuerter Deep-Link

Sie können Ihre App auch mit einem Start-Argument programmgesteuert starten, um die App mit einem ähnlichen Kontext wie der App-Dienst zu öffnen. Wenn Sie kein Start-Argument angeben, wird der Hauptbildschirm der App gestartet.

Hier fügen wir einen [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Parameter mit dem Wert „Las Vegas“ zu einem [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)-Objekt hinzu, das im [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Aufruf des [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Objekts verwendet wird.

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>App-Manifest

Um Deep-Links zu Ihrer App zu ermöglichen, müssen Sie die `windows.personalAssistantLaunch`-Erweiterung in der Datei „Package.appxmanifest“ Ihres App-Projekts deklarieren.

Hier deklarieren wir die `windows.personalAssistantLaunch`-Erweiterung für die **Adventure Works**-App.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>Protokollvertrag

Ihre App wird über die URI (Uniform Resource Identifier)-Aktivierung mithilfe eines [**Protocol**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)-Vertrags im Vordergrund gestartet. Die App muss das [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application)-Ereignis Ihrer App überschreiben und nach einem **ActivationKind**-Element mit dem Wert **Protocol** suchen. Weitere Informationen finden Sie unter [Behandeln der URI-Aktivierung](/windows/uwp/launch-resume/handle-uri-activation).

Hier wird der von [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) bereitgestellte URI decodiert, um auf das Start-Argument zuzugreifen. In diesem Beispiel wird für den [**Uri**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) „windows.personalassistantlaunch:? LaunchContext = Las Vegas“ festgelegt.

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-apps](cortana-interactions.md)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)
