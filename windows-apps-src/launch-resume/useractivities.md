---
title: Benutzeraktivitäten geräteübergreifend fortsetzen
description: In diesem Thema wird beschrieben, wie Benutzer fortsetzen können, was sie in ihrer App erledigt haben, auch über mehrere Geräte hinweg.
keywords: benutzeraktivität, benutzeraktivitäten, zeitachse, cortana aufgaben weiterführen, cortana begonnene aufgaben bearbeiten, project rome
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7ddceda3457ef5251cb2b1e384dbb880725103fa
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282309"
---
# <a name="continue-user-activity-even-across-devices"></a>Benutzeraktivitäten geräteübergreifend fortsetzen

In diesem Thema wird beschrieben, wie Benutzer fortsetzen können, was sie in ihrer App, auf ihrem PC und anderen Geräten erledigt haben.

## <a name="user-activities-and-timeline"></a>Benutzeraktivitäten und die Zeitachse

Jeden Tag verbringen wir viel Zeit an unterschiedlichen Geräten. Im Bus benutzen wir unser Telefon, tagsüber im Büro den PC und abends surfen wir auf dem Smartphone oder dem Tablet. Wenn Sie in Windows 10, Build 1803 oder höher, eine [Benutzeraktivität](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) erstellen, wird diese Aktivität in der Windows-Zeitachse und in der Funktion „Dort fortfahren, wo Sie aufgehört haben” in Cortana angezeigt. Die Zeitachse bietet eine umfassende Aufgabenansicht, die Ihnen über Benutzeraktivitäten in einer chronologischen Ansicht anzeigt, woran Sie bisher gearbeitet haben. Hier kann auch aufgezeichnet werden, woran Sie auf unterschiedlichen Geräten gearbeitet haben.

![Bild der Windows-Zeitachse](images/timeline.png)

Wenn Sie Ihr Smartphone mit Ihrem Windows-PC verbinden, können Sie an den Aufgaben weiterarbeiten, die Sie auf Ihrem iOS- oder Android-Gerät begonnen haben.

Bei einer **UserActivity** handelt es sich um eine bestimmte Aufgabe, die der Nutzer in Ihrer App bearbeitet hat. Ein Beispiel für eine **UserActivity** könnte ein Feed Ihres RSS-Readers sein, den Sie kürzlich gelesen haben. Wenn Sie ein Spiel spielen, ist die **UserActivity** möglicherweise das Level, das Sie erreicht haben. Ein anderes Beispiel für eine **UserActivity** könnte eine Playlist in Ihrer Musik-App sein. Wenn Sie an einem Dokument arbeiten, stellt die **UserActivity** unter Umständen die Stelle dar, an der Sie aufgehört haben zu arbeiten.  Kurz gesagt: Bei einer **UserActivity** handelt es sich um ein Ziel in Ihrer App, das ein Nutzer erreichen und an dem er später weiter die Arbeit wieder aufnehmen kann.

Wenn Sie mit einer **UserActivity** interagieren, indem Sie [UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession) aufrufen, erstellt das System einen Verlaufsdatensatz, der die Start- und Endzeit der **UserActivity** angibt. Wie Sie mit der **UserActivity** im Laufe der Zeit mehrmals interagieren, werden mehrere Verlaufsdatensätze für sie aufgezeichnet.

## <a name="add-user-activities-to-your-app"></a>Benutzeraktivitäten zur App hinzufügen

Über die [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) wird die Benutzerbindung in Windows gemessen. Sie besteht aus drei Teilen: einem URI zur Aktivierung der App, zu der die Aktivität gehört, visuelle Elemente sowie Metadaten, die die Aktivität beschreiben.

1. Der [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) wird verwendet, um die Anwendung in einem bestimmten Kontext fortzusetzen. Dieser Link übernimmt in der Regel die Form des Protokoll Handlers für ein Schema (z. b. "My-App://Page2? Action = Edit") oder einen appurihandler (z. b. http://constoso.com/page2?action=edit).
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) stellt eine Klasse bereit, die es dem Benutzer ermöglicht, eine Aktivität visuell mit einem Titel, einer Beschreibung oder Adaptive Karten-Elementen zu identifizieren.
3. In [Content](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) können Sie Metadaten für die Aktivität speichern, die zum Gruppieren und Abrufen von Aktivitäten in einem bestimmten Kontext verwendet werden können. Häufig handelt es sich hierbei um [https://schema.org](https://schema.org)-Daten.

So fügen Sie Ihrer App eine **UserActivity** hinzu:

1. Wenn der Kontext des Benutzers innerhalb der App geändert wird (z. B. Seitennavigation, neues Spiellevel etc.), erstellen Sie **UserActivity**-Objekte.
2. Füllen Sie **useractivity** -Objekte mit dem minimalen Satz erforderlicher Felder auf: " [ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId)", " [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)" und " [useractivity. visualelements. DisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText)".
3. Fügen Sie Ihrer App einen benutzerdefinierten Schema-Handler hinzu, damit er erneut durch eine **UserActivity** aktiviert werden kann.

Eine **UserActivity** kann mit nur wenigen Codezeilen in eine App integriert werden. Angenommen, dieser Code befindet sich in „MainPage.xaml.cs” in der MainPage-Klasse (Hinweis: es wird von `using Windows.ApplicationModel.UserActivities;` ausgegangen):

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

Die erste Zeile in der oben genannten `GenerateActivityAsync()`-Methode ruft [UserActivityChannel ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel) eines Benutzers auf. Hierbei handelt es sich um den Feed, in dem die Aktivitäten dieser App veröffentlicht werden. Die nächste Zeile fragt den Kanal einer Aktivität namens `MainPage` an.

* Ihre App sollte Aktivitäten so benennen, dass jedes Mal dieselbe ID generiert wird, wenn der Benutzer sich an einer bestimmten Stelle in der App befindet. Wenn die App beispielsweise seitenbasiert ist, verwenden Sie einen Bezeichner für die Seite. Wenn sie dokumentbasiert ist, verwenden Sie den Dokumentnamen (oder ein Hash des Namens).
* Wenn im Feed eine vorhandene Aktivität mit derselben ID vorhanden ist, wird die Aktivität vom Kanal so zurückgegeben, dass `UserActivity.State`den Status [Veröffentlicht](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate)) hat. Wenn keine Aktivität mit diesem Namen vorhanden ist, wird neue Aktivität mit dem `UserActivity.State`**Neu**  zurückgegeben.
* Aktivitäten sind auf Ihre App beschränkt. Es gibt keine Konflikte zwischen Ihrer Aktivitäts-ID und Aktivitäts-IDs in anderen Apps.

Nach dem Abrufen oder Erstellen der **UserActivity**, geben Sie die beiden anderen erforderlichen Felder an: `UserActivity.VisualElements.DisplayText`und `UserActivity.ActivationUri`.

Speichern Sie anschließend die **UserActivity**-Metadaten durch Aufrufen von [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync) und schließlich [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), die eine [UserActivitySession ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession) zurückgibt. Mit dem Objekt **UserActivitySession** können wir verwalten, wann der Benutzer tatsächlich mit der **UserActivity**  beschäftigt ist. Wir sollten z. B. `Dispose()` in der **UserActivitySession** aufrufen, wenn der Benutzer die Seite verlässt. Im obigen Beispiel rufen wir auch `Dispose()` in `_currentActivity` auf, bevor wir `CreateSession()` aufrufen. Dies ist der Fall, weil wir `_currentActivity` zu einem Mitgliedsfeld unserer Seite gemacht haben und alle vorhandenen Aktivität beenden möchten, bevor wir die neue starten (Hinweis: `?`ist der [Operator mit NULL-Bedingung](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-), der auf Null testet, bevor der Member-Zugriff durchgeführt wird).

Da es sich in diesem Fall beim `ActivationUri` um ein benutzerdefiniertes Schema handelt, müssen wir außerdem das Protokoll im Anwendungsmanifest registrieren. Dies erfolgt in der Package.appmanifest XML-Datei oder mithilfe des Designers.

Wenn Sie die Änderung mit dem Designer durchführen möchten, doppelklicken Sie auf die Datei „Package.appmanifest” in Ihrem Projekt, um den Designer zu starten. Wählen Sie dann die Registerkarte **Deklarationen** und fügen Sie eine **Protokoll**-Definition hinzu. Die einzige Eigenschaft, die ausgefüllt werden muss, ist **Name**. Sie sollte mit dem URI übereinstimmen, den wir oben angegeben haben – `my-app`.

Nun müssen wir etwas Code schreiben, um der App mitzuteilen, was passiert, wenn sie von einem Protokoll aktiviert wird. Wir überschreiben die `OnActivated`-Methode in „App.xaml.cs” wie folgt, sodass der URI an die Hauptseite übergeben wird:

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

Der Code bewirkt, dass erkannt wird, ob die App über ein Protokoll aktiviert wurde. Wenn dem so ist, wird ermittelt, wie die App den Vorgang fortsetzen kann, für den sie aktiviert wurde. Die einzige Aktivität, die von dieser APP fortgesetzt wird, ist eine einfache APP, die Sie beim Eintreffen der APP auf der sekundären Seite vornimmt.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Adaptive Karten verwenden, um die Zeitachse zu verbessern

Aktivitäten des Benutzers werden in Cortana und der Zeitachse angezeigt. Wenn Aktivitäten in der Zeitachse angezeigt werden, geschieht dies über das [Adaptive Karten](https://adaptivecards.io/)-Framework. Wenn Sie nicht für jede Aktivität eine adaptive Karte angeben, erstellt die Zeitachse automatisch eine einfache Aktivitätskarte basierend auf dem App-Namen und -Symbol, dem Titelfeld und optional auf der Beschreibung. Es folgt ein Beispiel für die Nutzlast einer adaptiven Karte sowie die Karte, die produziert wird.

![Eine adaptive Karte](images/adaptivecard.png)]

Beispiel für die Nutzlast einer adaptiven Karte mit JSON-Zeichenfolge:

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

Fügen Sie die Nutzlast einer adaptiven Karte mit JSON-Zeichenfolge wie folgt zur **UserActivity** hinzu:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Plattformübergreifende Integration und Integration zwischen Diensten

Wenn Ihre App plattformübergreifend ausgeführt wird (z. B. auf Android und iOS) oder der Benutzerzustand in der Cloud verwaltet wird, können Sie UserActivities über [Microsoft Graph](https://developer.microsoft.com/graph) veröffentlichen.
Nachdem Ihre App oder Ihr Dienst mit einem Microsoft-Konto authentifiziert wurde, sind lediglich zwei einfache REST-Aufrufe erforderlich, um anhand der gleichen Daten wie weiter oben beschrieben [Aktivitäts](https://docs.microsoft.com/graph/api/resources/projectrome-activity)- und [Verlaufs](https://docs.microsoft.com/graph/api/resources/projectrome-historyitem)-Objekte zu generieren.

## <a name="summary"></a>Zusammenfassung

Sie können die [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)-API verwenden, um Ihre App in der Zeitachse und Cortana anzeigen zu lassen.
* Weitere Informationen zur [ **useractivity** -API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)
* Sehen Sie sich den [Beispielcode](https://github.com/Microsoft/project-rome) an.
* Hier finden Sie [weitere ausgeklügelte adaptive Karten](https://adaptivecards.io/).
* Veröffentlichen Sie eine **UserActivity** von iOS, Android oder Ihrem Webdienst über [Microsoft Graph](https://developer.microsoft.com/graph).
* Erfahren Sie mehr zu [Project Rome auf GitHub](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>Wichtige APIs

* [Useractivities-Namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Verwandte Themen

* [Benutzeraktivitäten (Project Rom-Dokumentation)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Adaptive Karten](https://docs.microsoft.com/adaptive-cards/)
* [Bildschirm Abbildung von adaptiven Karten, Beispiele](https://adaptivecards.io/)
* [Behandeln der URI-Aktivierung](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Verwenden der Microsoft Graph, des Aktivitäts Feeds und adaptiver Karten für Ihre Kunden auf beliebigen Plattformen](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)
