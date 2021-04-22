---
title: Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)
description: In diesem Thema wird beschrieben, wie Sie Benutzern dabei helfen können, ihre App auch auf mehreren Geräten wieder aufzunehmen.
keywords: Benutzeraktivität, Benutzeraktivitäten, Zeitachse, Cortana-Abholung an der Stelle, an der Sie aufgelassen haben, Cortana pick up where i left off, Project Rome
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 45a1d1d319f67dfac5e7c44a4c65c3bf2e3c0335
ms.sourcegitcommit: 73ec979ce6b9701e7135fd0541bf932b0847908e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107881693"
---
# <a name="continue-user-activity-even-across-devices"></a>Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)

In diesem Thema wird beschrieben, wie Sie Benutzern dabei helfen können, ihre App auf ihrem PC und geräteübergreifend wieder aufzunehmen.

> [!NOTE]
> Ab Juni 2021 haben Benutzer, deren Aktivitätsverlauf auf ihren Windows-Geräten über ihr Microsoft-Konto (MSA) synchronisiert wurde, nicht mehr die Möglichkeit, neue Aktivitäten auf der Zeitachse hochzuladen. Sie können weiterhin die Zeitachse verwenden und ihren Aktivitätsverlauf (Informationen zu aktuellen Apps, Websites und Dateien) auf ihrem lokalen PC anzeigen. Mit AAD verbundene Konten sind nicht wirkt sich aus. 

## <a name="user-activities-and-timeline"></a>Benutzeraktivitäten und Zeitachse

Unsere Zeit wird jeden Tag auf mehrere Geräte verteilt. Wir können unser Telefon im Bus, einen PC am Tag und dann ein Telefon oder Tablet am Abend verwenden. Ab Windows 10 Build 1803 oder höher wird [](/uwp/api/windows.applicationmodel.useractivities.useractivity) diese Aktivität beim Erstellen einer Benutzeraktivität auf der Windows-Zeitachse und in der Cortana-Funktion Pick up where I left off angezeigt. Die Zeitachse ist eine umfangreiche Aufgabenansicht, die Benutzeraktivitäten nutzt, um eine chronologische Ansicht ihrer Arbeit anzuzeigen. Sie kann auch das enthalten, was Sie geräteübergreifend verwendet haben.

![Windows-Zeitachsenbild](images/timeline.png)

Ebenso können Sie mit dem Verknüpfen Ihres Smartphones mit Ihrem Windows-PC fortfahren, was Sie zuvor auf Ihrem iOS- oder Android-Gerät ausgeführt haben.

Stellen Sie sich **eine UserActivity** als etwas bestimmtes vor, an dem der Benutzer in Ihrer App arbeitet. Wenn Sie beispielsweise einen RSS-Reader verwenden, kann **userActivity** der Feed sein, den Sie lesen. Wenn Sie ein Spiel spielen, kann **die UserActivity** die Ebene sein, die Sie spielen. Wenn Sie auf eine Musik-App lauschen, kann **die UserActivity** die Wiedergabeliste sein, auf die Sie lauschen. Wenn Sie an einem Dokument arbeiten, können Sie **die UserActivity** dort befinden, wo Sie die Arbeit daran aufgelassen haben, und so weiter.  Kurz gesagt stellt eine **UserActivity** ein Ziel innerhalb Ihrer App dar, sodass der Benutzer seine Arbeit fortsetzen kann.

Wenn Sie eine **UserActivity** durch Aufrufen von [UserActivity.CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)aufrufen, erstellt das System einen Verlaufsdatensatz, der die Start- und Endzeit für diese **UserActivity** angibt. Wenn Sie sich im Laufe der Zeit erneut mit **dieser UserActivity** befassen, werden mehrere Verlaufsdatensätze dafür aufgezeichnet.

## <a name="add-user-activities-to-your-app"></a>Hinzufügen von Benutzeraktivitäten zu Ihrer App

Eine [UserActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity) ist die Einheit der Benutzerbindung in Windows. Sie umfasst drei Teile: einen URI, der zum Aktivieren der App verwendet wird, zu der die Aktivität gehört, visuelle Elemente und Metadaten, die die Aktivität beschreiben.

1. [Der ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) wird verwendet, um die Anwendung mit einem bestimmten Kontext fortzusetzen. Dieser Link hat in der Regel die Form eines Protokollhandlers für ein Schema (z.B. "my-app://page2?action=edit") oder eines AppUriHandlers (z. http://constoso.com/page2?action=edit) B. .
2. [VisualElements](/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) macht eine Klasse verfügbar, die es dem Benutzer ermöglicht, eine Aktivität mit einem Titel, einer Beschreibung oder adaptiven Kartenelementen visuell zu identifizieren.
3. Schließlich können Sie mit [Inhalt](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) Metadaten für die Aktivität speichern, die zum Gruppieren und Abrufen von Aktivitäten in einem bestimmten Kontext verwendet werden können. Häufig hat dies die Form von [https://schema.org](https://schema.org) Daten.

So fügen Sie Ihrer App eine **UserActivity** hinzu:

1. Generieren von **UserActivity-Objekten,** wenn sich der Kontext Ihres Benutzers innerhalb der App ändert (z. B. Seitennavigation, neue Spielebene usw.)
2. Füllen Sie **UserActivity-Objekte** mit den mindestens erforderlichen Feldern auf: [ActivityId](/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)und [UserActivity.VisualElements.DisplayText](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Fügen Sie Ihrer App einen benutzerdefinierten Schemahandler hinzu, damit sie von einer **UserActivity** erneut aktiviert werden kann.

Eine **UserActivity** kann mit nur wenigen Codezeilen in eine App integriert werden. Stellen Sie sich beispielsweise diesen Code in MainPage.xaml.cs in der MainPage-Klasse vor (Hinweis: geht von `using Windows.ApplicationModel.UserActivities;` aus):

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

Die erste Zeile in der obigen Methode ruft den `GenerateActivityAsync()` [UserActivityChannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)eines Benutzers ab. Dies ist der Feed, in dem die Aktivitäten dieser App veröffentlicht werden. In der nächsten Zeile wird der Kanal einer Aktivität namens `MainPage` abgefragt.

* Ihre App sollte Aktivitäten so benennen, dass dieselbe ID jedes Mal generiert wird, wenn sich der Benutzer an einem bestimmten Ort in der App befindet. Wenn Ihre App z. B. seiten basiert, verwenden Sie einen Bezeichner für die Seite. Wenn das Dokument auf dem Dokument basiert, verwenden Sie den Namen des Dokuments (oder einen Hash des Namens).
* Wenn eine vorhandene Aktivität im Feed mit der gleichen ID vorhanden ist, wird diese Aktivität vom Kanal zurückgegeben, und auf `UserActivity.State` [Veröffentlicht festgelegt.](/uwp/api/windows.applicationmodel.useractivities.useractivitystate) Wenn es keine Aktivität mit diesem Namen gibt und neue Aktivität zurückgegeben wird, bei der `UserActivity.State` auf New **festgelegt ist.**
* Aktivitäten gelten für Ihre App. Sie müssen sich keine Gedanken darüber machen, ob Ihre Aktivitäts-ID mit IDs in anderen Apps in Zusammenhang stößt.

Geben Sie nach dem Abrufen oder **Erstellen der UserActivity** die beiden anderen erforderlichen Felder an:  `UserActivity.VisualElements.DisplayText` und `UserActivity.ActivationUri` .

Speichern Sie als Nächstes die **UserActivity-Metadaten,** indem [Sie SaveAsync](/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)und schließlich [CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)aufrufen, die eine [UserActivitySession zurückgibt.](/uwp/api/windows.applicationmodel.useractivities.useractivitysession) Die **UserActivitySession** ist das Objekt, das wir verwenden können, um zu verwalten, wann der Benutzer tatsächlich mit der **UserActivity beschäftigt ist.** Beispielsweise sollten wir für die `Dispose()` **UserActivitySession** aufrufen, wenn der Benutzer die Seite verlässt. Im obigen Beispiel rufen wir auch auf, `Dispose()` bevor `_currentActivity` wir `CreateSession()` aufrufen. Dies liegt daran, dass wir ein Memberfeld unserer Seite erstellt haben und alle vorhandenen Aktivitäten beenden möchten, bevor wir die neue aktivität starten (Hinweis: ist der `_currentActivity` NULL-bedingte Operator, der vor dem Ausführen des Memberzugriffs auf NULL `?` testet). [](/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)

Da in diesem Fall ein benutzerdefiniertes Schema ist, müssen wir auch das Protokoll `ActivationUri` im Anwendungsmanifest registrieren. Dies erfolgt in der XML-Datei Package.appmanifest oder mithilfe des Designers.

Doppelklicken Sie zum Ändern mit dem Designer auf die Datei Package.appmanifest in  Ihrem Projekt, um den Designer zu starten, wählen Sie die Registerkarte Deklarationen aus, und fügen Sie eine **Protokolldefinition** hinzu. Die einzige Eigenschaft, die ausgefüllt werden muss, ist vorerst **Name**. Er sollte mit dem oben angegebenen URI übereinstimmen: `my-app` .

Nun müssen wir Code schreiben, um der App mitzuteilen, was sie tun soll, wenn sie von einem Protokoll aktiviert wird. Wir überschreiben die `OnActivated` -Methode in App.xaml.cs, um den URI wie folgt an die Hauptseite zu übergeben:

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

Dieser Code erkennt, ob die App über ein Protokoll aktiviert wurde. Wenn dies der Wartevorgang war, sieht es aus, was die App tun sollte, um die Aufgabe fortzusetzen, für die sie aktiviert wird. Da es sich um eine einfache App handelt, wird diese App nur fortgesetzt, wenn Sie auf der sekundären Seite angezeigt werden, wenn die App angezeigt wird.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Verwenden von Adaptive Karten zum Verbessern der Zeitachsenerfahrung

Benutzeraktivitäten werden in Cortana und der Zeitachse angezeigt. Wenn Aktivitäten auf der Zeitachse angezeigt werden, werden sie mithilfe des [Adaptive Card-Frameworks](https://adaptivecards.io/) angezeigt. Wenn Sie keine adaptive Karte für jede Aktivität bereitstellen, erstellt die Zeitachse automatisch eine einfache Aktivitätskarte basierend auf Ihrem Anwendungsnamen und Symbol, dem Titelfeld und optional dem Beschreibungsfeld. Im Folgenden finden Sie ein Beispiel für die Nutzlast der adaptiven Karte und die von ihr erzeugte Karte.

![Eine adaptive Karte](images/adaptivecard.png)]

Beispiel für json-Zeichenfolge für die Nutzlast adaptiver Karten:

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

Fügen Sie der **UserActivity** die Adaptive Karten Nutzlast wie folgt als JSON-Zeichenfolge hinzu:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Plattformübergreifende und Dienst-zu-Dienst-Integration

Wenn Ihre App plattformübergreifend ausgeführt wird (z.B. unter Android und iOS) oder den Benutzerzustand in der Cloud beibehält, können Sie UserActivities über [Microsoft Graph](https://developer.microsoft.com/graph)veröffentlichen.
Nachdem Ihre Anwendung oder Ihr Dienst mit einem Microsoft-Konto authentifiziert wurde, werden nur noch zwei einfache REST-Aufrufe benötigt, um [Aktivitäts-](/graph/api/resources/projectrome-activity) und [Verlaufsobjekte](/graph/api/resources/projectrome-historyitem) zu generieren, wobei die gleichen Daten wie oben beschrieben verwendet werden.

## <a name="summary"></a>Zusammenfassung

Sie können die [UserActivity-API](/uwp/api/windows.applicationmodel.useractivities) verwenden, damit Ihre App auf der Zeitachse und in Cortana angezeigt wird.
* Weitere Informationen zur [ **UserActivity-API**](/uwp/api/windows.applicationmodel.useractivities)
* Sehen Sie sich den [Beispielcode an.](https://github.com/Microsoft/project-rome)
* Erfahren [Sie mehr über Adaptive Karten](https://adaptivecards.io/).
* Veröffentlichen Sie **eine UserActivity** über iOS, Android oder Ihren Webdienst [über Microsoft Graph](https://developer.microsoft.com/graph).
* Weitere Informationen zu [Project Rome finden Sie auf GitHub.](https://github.com/Microsoft/project-rome)

## <a name="key-apis"></a>Schlüssel-APIs

* [UserActivities-Namespace](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Verwandte Themen

* [Benutzeraktivitäten (Project Rome-Dokumentation)](/windows/project-rome/user-activities/)
* [Adaptive Karten](/adaptive-cards/)
* [Schnellansicht für adaptive Karten, Beispiele](https://adaptivecards.io/)
* [Behandeln der URI-Aktivierung](./handle-uri-activation.md)
* [Zusammenarbeit mit Ihren Kunden auf jeder Plattform mithilfe der Microsoft Graph, des Aktivitätsfeeds und Adaptive Karten](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)
