---
title: Interagieren mit einer Hintergrund-App in Cortana – Cortana UWP-Entwurf und -Entwicklung
description: Ermöglichen Sie Benutzerinteraktion mit einer Hintergrund-App über Sprach- und Texteingabe im Cortana-Canvas, während ein Sprachbefehl ausgeführt wird.
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 6e63d86d8d3764f8ca95dce4c1b8b7de437c95ec
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824494"
---
# <a name="interact-with-a-background-app-in-cortana"></a>Interagieren mit einer Hintergrund-App in Cortana

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.
>
> Informationen dazu, wie Cortana moderne Produktivitäts Umgebungen transformiert, finden Sie [unter Cortana in Microsoft 365](/microsoft-365/admin/misc/cortana-integration) .

Ermöglichen Sie Benutzerinteraktion mit einer Hintergrund-App über Sprach- und Texteingabe im **Cortana**-Canvas, während ein Sprachbefehl ausgeführt wird.

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana unterstützt einen vollständigen Sprachnavigationsworkflow mit Ihrer App. Dieser Workflow wird von der App definiert und kann die folgenden Funktionen unterstützen: 

- Erfolgreicher Abschluss
- Übergabe
- Fortschritt
- Bestätigung
- Mehrdeutigkeitsvermeidung
- Fehler

## <a name="composing-feedback-strings"></a>Verfassen von Feedbacks

> [!TIP]
> **Voraussetzungen**
>
> Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.
>
> - [Erste App erstellen](../../get-started/your-first-app.md)
> - Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](../../xaml-platform/events-and-routed-events-overview.md).
>
> **Richtlinien zur Benutzer Leistung**
>
> Unter [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)  finden Sie Informationen dazu, wie Sie Ihre APP mit **Cortana** und [sprach Interaktionen](speech-interactions.md) integrieren, um hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden sprachfähigen APP zu erhalten.

Erstellen Sie Feedback-Zeichenfolgen, die sowohl auf dem Bildschirm angezeigt als auch von **Cortana** gesprochen werden.

Die [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md) bieten Empfehlungen zum Verfassen von Zeichen folgen für **Cortana**.

Inhaltskarten können zusätzlichen Kontext für den Benutzer bereitstellen und dazu beitragen, die Feedback-Zeichenfolgen möglichst kurz zu halten.

**Cortana** unterstützt die folgenden Inhaltskartenvorlagen. (Für den Abschlussbildschirm kann nur eine einzige Vorlage verwendet werden.)

- Nur Titel
- Titel mit bis zu drei Textzeilen
- Titel mit Bild
- Titel mit Bild und bis zu drei Textzeilen

Das Image kann Folgendes sein:

- 68 (B) x 68 (H)
- 68 (B) x 92 (H)
- 280 (B) x 140 (H)

Sie können es dem Benutzer auch ermöglichen, Ihre App durch Klicken auf eine Karte oder auf den Textlink zu Ihrer App im Vordergrund zu starten.

## <a name="completion-screen"></a>Abschlussbildschirm

Ein Abschlussbildschirm informiert den Benutzer über die abgeschlossene Sprachbefehlsaufgabe.

Aufgaben, auf die Ihre App innerhalb von weniger als 500 Millisekunden reagieren muss und keine weiteren Informationen vom Benutzer erfordern, werden ohne weitere Interaktion mit  **Cortana** abgeschlossen. Cortana zeigt den Abschlussbildschirm an.

Im Folgenden verwenden wir die **Adventure Works**-App, um den Abschlussbildschirm für eine Sprachbefehlsanforderung zum Anzeigen anstehender Reisen nach London darzustellen.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="Screenshot der Cortana-Hintergrund-App-Vervollständigung für eine bevorstehende Fahrt":::

Der Sprachbefehl ist in der Datei „AdventureWorksCommands.xml“ definiert:

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs enthält die Methode für die Abschlussmeldung:

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
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

## <a name="hand-off-screen"></a>Übergabebildschirm

Sobald ein Sprachbefehl erkannt wird, muss **Cortana** reporterfolgreiches Async aufrufen und innerhalb von ungefähr 500.000 Feedback abgeben, wenn der APP Service die durch den Voice-Befehl angegebene Aktion nicht innerhalb von 500 ms ausführen kann. **Cortana** zeigt einen Hand seitigen Bildschirm an, der angezeigt wird, bis die APP reportsuccess Async aufruft, oder bis zu 5 Sekunden.

Falls der App-Dienst ReportSuccessAsync oder eine andere VoiceCommandServiceConnection-Methode nicht aufruft, erhält der Benutzer eine Fehlermeldung, und der Aufruf des App-Diensts wird abgebrochen.

Hier sehen Sie einen Übergabebildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer **Cortana** nach anstehenden Reisen befragt. Der Übergabebildschirm enthält eine Meldung mit dem Namen des App-Diensts, einem Symbol und einer **Feedback**-Zeichenfolge. 

[!NOTE] Sie eine **Feedback**-Zeichenfolge in der VCD-Datei deklarieren. Diese Zeichenfolge hat keine Auswirkungen auf den auf der Cortana-Canvas angezeigten UI-Text, sie wirkt sich nur auf den von **Cortana** gesprochenen Text aus.

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Screenshot des Bildschirms für die Hintergrund-App der Cortana-App":::

## <a name="progress-screen"></a>Statusbildschirm

Wenn der App-Dienst mehr als 500 ms benötigt, um ReportSuccessAsync aufzurufen, zeigt **Cortana** dem Benutzer einen Statusbildschirm an. Das App-Symbol wird angezeigt, und Sie müssen sowohl eine GUI- als auch eine TTS-Statuszeichenfolge bereitstellen, die deutlich macht, dass die Aufgabe aktiv bearbeitet wird.

**Cortana** zeigt einen Statusbildschirm maximal fünf Sekunden lang an. Nach fünf Sekunden zeigt **Cortana** eine Fehlermeldung an, und der App-Dienst wird geschlossen. Sollte die Durchführung der Aktion länger als fünf Sekunden dauern, kann der App-Dienst **Cortana** mit weiteren Statusbildschirmen aktualisieren.

Hier sehen Sie einen Statusbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer eine Reise nach Las Vegas storniert. Der Statusbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="Screenshot von Cortana mit dem Bildschirm Status der Hintergrund-App":::

AdventureWorksVoiceCommandService.cs enthält die folgende Statusmeldungsmethode, die [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) zum Anzeigen des Statusbildschirms in **Cortana** aufruft.

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>Bestätigungsbildschirm

Wenn eine per Sprachbefehl angegebene Aktion nicht rückgängig gemacht werden kann, erhebliche Auswirkungen hat oder die Treffgenauigkeit gering ist, kann der App-Dienst eine Bestätigung anfordern.

Hier sehen Sie einen Bestätigungsbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Der App-Dienst hat für **Cortana** einen Bestätigungsbildschirm bereitgestellt, in dem der Benutzer die Reisestornierung bestätigen oder ablehnen kann.

Wenn der Benutzer nicht mit „Ja“ oder „Nein“ antwortet, kann **Cortana** die Antwort auf die Frage nicht ermitteln. In diesem Fall stellt **Cortana** dem Benutzer eine ähnliche, vom App-Dienst bereitgestellte Frage.

Falls der Benutzer auch die zweite Frage nicht mit „Ja“ oder „Nein“ beantwortet, fragt **Cortana** ein drittes Mal nach. Dabei wird nochmals die gleiche Frage verwendet und eine Entschuldigung vorangestellt. Antwortet der Benutzer wieder nicht mit „Ja“ oder „Nein“, lauscht **Cortana** nicht weiter auf Spracheingaben und fordert den Benutzer stattdessen auf, auf eine der Schaltflächen zu tippen.

Der Bestätigungsbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="Screenshot von Cortana mit dem Bestätigungsbildschirm der Hintergrund-App":::

AdventureWorksVoiceCommandService.cs enthält die folgende Methode für die Stornierung der Reise, die [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) zum Anzeigen eines Bestätigungsbildschirms in **Cortana** aufruft.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>Bildschirm zur Vermeidung von Mehrdeutigkeiten

Wenn für eine per Sprachbefehl angegebene Aktion mehrere Ergebnisse möglich sind, kann der App-Dienst vom Benutzer weitere Informationen anfordern.

Hier sehen Sie einen Mehrdeutigkeitsvermeidungsbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Für den Benutzer sind allerdings zwei Las Vegas-Reisen mit unterschiedlichen Reisedaten vorhanden, und der App-Dienst kann die Aktion nur abschließen, wenn der Benutzer die gewünschte Reise auswählt.

Der App-Dienst stellt für **Cortana** einen Mehrdeutigkeitsvermeidungsbildschirm bereit, über den der Benutzer vor der Stornierung aufgefordert wird, die gewünschte Reise in einer Liste mit passenden Reisen auszuwählen.

In diesem Fall stellt **Cortana** dem Benutzer eine ähnliche, vom App-Dienst bereitgestellte Frage.

Wenn der Benutzer auch bei der zweiten Aufforderung nichts sagt, wovon sich die gewünschte Auswahl ableiten lässt, fragt **Cortana** ein drittes Mal nach. Dabei wird nochmals die gleiche Frage verwendet und eine Entschuldigung vorangestellt. Sagt der Benutzer wieder nichts, wovon sich die gewünschte Auswahl ableiten lässt, lauscht **Cortana** nicht weiter auf Spracheingaben und fordert den Benutzer stattdessen auf, auf eine der Schaltflächen zu tippen.

Der Mehrdeutigkeitsvermeidungsbildschirm enthält eine aktionsspezifische Meldung, ein Symbol und eine Inhaltskachel mit Informationen zur stornierten Reise.

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Screenshot von Cortana mit Bildschirm für die Unterscheidung von Hintergrund-apps":::

AdventureWorksVoiceCommandService.cs enthält die folgende Methode für die Stornierung der Reise, die [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) zum Anzeigen eines Mehrdeutigkeitsvermeidungsbildschirms in **Cortana** aufruft.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>Fehlerbildschirm

Wenn eine per Sprachbefehl angegebene Aktion nicht abgeschlossen werden kann, kann ein App-Dienst einen Fehlerbildschirm bereitstellen.

Hier sehen Sie einen Fehlerbildschirm für die App **Adventure Works**. In diesem Beispiel hat ein Benutzer den App-Dienst über **Cortana** angewiesen, eine Reise nach Las Vegas zu stornieren. Für den Benutzer sind jedoch keine Reisen nach Las Vegas geplant.

Der App-Dienst stellt für **Cortana** einen Fehlerbildschirm mit einer aktionsspezifischen Meldung, einem Symbol und der entsprechenden Fehlermeldung bereit.

Rufen Sie [**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) auf, um den Fehlerbildschirm in **Cortana** anzuzeigen.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-Apps](cortana-interactions.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)