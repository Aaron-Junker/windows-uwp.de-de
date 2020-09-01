---
title: Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)
description: In diesem Thema wird beschrieben, wie Sie Benutzern dabei helfen können, ihre Aufgaben in Ihrer APP wieder aufzunehmen, sogar über mehrere Geräte hinweg.
keywords: Benutzeraktivität, Benutzeraktivitäten, Zeitachse, Cortana nehmen Sie an der Stelle an, an der Sie aufgehört haben, Cortana an der Stelle, an der ich aufgehört habe, Project
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ebaca3b831ae30637a88d01319a89d139dde1cf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162624"
---
# <a name="continue-user-activity-even-across-devices"></a>Fortsetzen von Benutzeraktivitäten (auch geräteübergreifend)

In diesem Thema wird beschrieben, wie Sie Benutzern das Fortsetzen Ihrer APP auf Ihrem PC und auf Geräten unterstützen können.

## <a name="user-activities-and-timeline"></a>Benutzeraktivitäten und Zeitachse

Unsere Zeit täglich ist auf mehrere Geräte verteilt. Wir können unser Telefon auf dem Bus, einem PC während des Tages, einem Telefon oder Tablet am Abend verwenden. Beginnend mit Windows 10 Build 1803 oder höher wird durch das Erstellen einer [Benutzeraktivität](/uwp/api/windows.applicationmodel.useractivities.useractivity) die Aktivität in der Windows-Zeitachse und in Cortana an der Stelle, an der das Feature ausgelassen wurde, angezeigt. Die Zeitachse ist eine umfangreiche Aufgaben Ansicht, die Benutzeraktivitäten nutzt, um eine chronologische Darstellung ihrer Arbeit anzuzeigen. Es kann auch sein, dass Sie über Geräte hinweg gearbeitet haben.

![Windows-Zeitachsen Bild](images/timeline.png)

Wenn Sie Ihr Telefon mit Ihrem Windows-PC verknüpfen, können Sie auch den Vorgang fortsetzen, den Sie zuvor auf Ihrem IOS-oder Android-Gerät durchgeführt haben.

Stellen Sie sich eine **useractivity** als etwas spezifisches Element vor, an dem der Benutzer in Ihrer APP gearbeitet hat. Wenn Sie z. b. einen RSS-Reader verwenden, könnte eine **useractivity** der Feed sein, den Sie lesen. Wenn Sie ein Spiel spielen, kann die **useractivity** die Ebene sein, die Sie spielen. Wenn Sie eine Musik-app lauschen, kann die **useractivity** die Wiedergabeliste sein, die Sie überwachen. Wenn Sie an einem Dokument arbeiten, kann es sein, dass die **useractivity** an der Stelle ist, an der Sie daran gearbeitet haben, usw.  Kurz gesagt, eine **useractivity** stellt ein Ziel innerhalb Ihrer APP dar, sodass der Benutzer das fortsetzen kann.

Wenn Sie eine **useractivity** durch Aufrufen von [useractivity. CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)einbinden, erstellt das System einen Verlaufs Daten Satz, der die Start-und Endzeit für diese **useractivity**angibt. Wenn Sie sich im Laufe der Zeit mit dieser **useractivity** erneut in Verbindung treten, werden mehrere Verlaufs Datensätze für Sie aufgezeichnet.

## <a name="add-user-activities-to-your-app"></a>Hinzufügen von Benutzeraktivitäten zu Ihrer APP

Eine [useractivity](/uwp/api/windows.applicationmodel.useractivities.useractivity) ist die Einheit der Benutzer Bindung in Windows. Sie besteht aus drei Teilen: einem URI, der zum Aktivieren der APP verwendet wird, zu der die Aktivität gehört, visuellen Elementen und Metadaten, die die Aktivität beschreiben.

1. Der [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) wird verwendet, um die Anwendung mit einem bestimmten Kontext wieder aufzunehmen. Dieser Link übernimmt in der Regel die Form des Protokoll Handlers für ein Schema (z. b. "My-App://Page2? Action = Edit") oder ein appurihandler (z http://constoso.com/page2?action=edit) . b.).
2. [Visualelements](/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) macht eine Klasse verfügbar, die es dem Benutzer ermöglicht, eine Aktivität mit einem Titel, einer Beschreibung oder adaptiven Karten Elementen visuell zu identifizieren.
3. Und schließlich können Sie Metadaten für die Aktivität [speichern, die](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) zum Gruppieren und Abrufen von Aktivitäten in einem bestimmten Kontext verwendet werden kann. Häufig ist dies die Form von [https://schema.org](https://schema.org) Daten.

So fügen Sie eine **useractivity** zu Ihrer APP hinzu:

1. Generieren Sie **useractivity** -Objekte, wenn sich der Kontext des Benutzers innerhalb der app ändert (z. b. Seitennavigation, neue spielsebene usw.).
2. Füllen Sie **useractivity** -Objekte mit dem minimalen Satz erforderlicher Felder auf: " [ActivityId](/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId)", " [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)" und " [useractivity. visualelements. DisplayText](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText)".
3. Fügen Sie Ihrer APP einen benutzerdefinierten Schema Handler hinzu, damit dieser durch eine **useractivity**reaktiviert werden kann.

Eine **useractivity** kann mit nur wenigen Codezeilen in eine APP integriert werden. Stellen Sie sich beispielsweise den folgenden Code in MainPage.XAML.cs innerhalb der MainPage-Klasse vor (Hinweis: geht davon aus `using Windows.ApplicationModel.UserActivities;` ):

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

In der ersten Zeile der `GenerateActivityAsync()` obigen Methode wird der [useractivitychannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)eines Benutzers abgerufen. Dies ist der Feed, in dem die Aktivitäten dieser App veröffentlicht werden. In der nächsten Zeile wird der Kanal einer Aktivität mit dem Namen abgefragt `MainPage` .

* Ihre APP sollte Aktivitäten so benennen, dass dieselbe ID generiert wird, wenn sich der Benutzer an einem bestimmten Speicherort in der APP befindet. Wenn Ihre APP z. b. Seiten basiert ist, verwenden Sie einen Bezeichner für die Seite. Wenn das Dokument basiert, verwenden Sie den Namen des Dokuments (oder einen Hash des Namens).
* Wenn eine Aktivität im Feed mit der gleichen ID vorhanden ist, wird diese Aktivität vom Kanal zurückgegeben, wobei `UserActivity.State` auf [veröffentlicht](/uwp/api/windows.applicationmodel.useractivities.useractivitystate)festgelegt ist. Wenn keine-Aktivität mit diesem Namen vorhanden ist, wird eine neue-Aktivität zurückgegeben, wobei `UserActivity.State` auf **New**festgelegt ist.
* Aktivitäten werden auf Ihre APP beschränkt. Sie müssen sich keine Gedanken über ihre Aktivitäts-ID machen, die mit IDs in anderen apps in Konflikt steht.

Nachdem Sie die **useractivity**erhalten oder erstellt haben, geben Sie die beiden anderen Pflichtfelder an:  `UserActivity.VisualElements.DisplayText` und `UserActivity.ActivationUri` .

Speichern Sie als nächstes die **useractivity** -Metadaten, indem Sie [saveasync](/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)aufrufen und schließlich [CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), die eine [useractivitysession](/uwp/api/windows.applicationmodel.useractivities.useractivitysession)zurückgibt. **Useractivitysession** ist das Objekt, mit dem Sie verwalten können, wann der Benutzer tatsächlich mit der **useractivity**beschäftigt ist. Beispielsweise sollten wir für `Dispose()` die **useractivitysession** aufrufen, wenn der Benutzer die Seite verlässt. Im obigen Beispiel wird auch aufgerufen, `Dispose()` `_currentActivity` bevor aufgerufen wird `CreateSession()` . Dies liegt daran, dass wir `_currentActivity` ein Element Feld unserer Seite erstellt haben und alle vorhandenen Aktivitäten vor dem Start der neuen Aktivität abbrechen möchten (Hinweis: `?` ist der [null bedingte Operator](/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-) , der vor der Ausführung des Element Zugriffs auf Null testet).

Da in diesem Fall `ActivationUri` ein benutzerdefiniertes Schema ist, müssen wir auch das Protokoll im Anwendungs Manifest registrieren. Dies erfolgt in der XML-Datei "Package. AppManifest" oder mithilfe des Designers.

Um die Änderung mit dem Designer vorzunehmen, doppelklicken Sie in Ihrem Projekt auf die Datei Package. AppManifest, um den Designer zu starten, wählen Sie die Registerkarte **Deklarationen** aus, und fügen Sie eine **Protokoll** Definition hinzu. Die einzige Eigenschaft, die ausgefüllt werden muss, ist vorerst " **Name**". Er sollte dem URI entsprechen, den wir oben angegeben haben `my-app` .

Nun müssen wir Code schreiben, um der APP mitzuteilen, was zu tun ist, wenn Sie durch ein Protokoll aktiviert wird. Wir überschreiben die- `OnActivated` Methode in app.XAML.cs, um den URI wie folgt an die Hauptseite zu übergeben:

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

Mit diesem Code wird erkannt, ob die APP über ein Protokoll aktiviert wurde. Wenn dies der Fall ist, wird überprüft, was die APP tun sollte, um die Aufgabe fortzusetzen, für die Sie aktiviert wird. Die einzige Aktivität, die von dieser APP fortgesetzt wird, ist eine einfache APP, die Sie beim Eintreffen der APP auf der sekundären Seite vornimmt.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Verwenden von adaptiven Karten zur Verbesserung der Zeitachsen Darstellung

Benutzeraktivitäten werden in Cortana und Timeline angezeigt. Wenn Aktivitäten in der Zeitachse angezeigt werden, werden Sie mit dem [adaptiven Karten](https://adaptivecards.io/) Framework angezeigt. Wenn Sie für jede Aktivität keine adaptive Karte angeben, erstellt die Zeitachse automatisch eine einfache Aktivitäts Karte, die auf dem Anwendungsnamen und-Symbol, dem Titelfeld und dem optionalen Beschreibungsfeld basiert. Im folgenden finden Sie ein Beispiel für eine Adaptive Karten Nutzlast und die von ihr erzeugte Karte.

![Eine Adaptive Karte](images/adaptivecard.png)]

Beispiel-JSON-Zeichenfolge für Adaptive Karten Nutzlast:

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

Fügen Sie die Nutzlast der adaptiven Karten als JSON-Zeichenfolge der **useractivity** wie folgt hinzu:

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Plattformübergreifende und Dienst-zu-Dienst-Integration

Wenn Ihre APP plattformübergreifend ausgeführt wird (z. b. unter Android und IOS) oder den Benutzer Zustand in der Cloud verwaltet, können Sie useractivities über [Microsoft Graph](https://developer.microsoft.com/graph)veröffentlichen.
Nachdem Ihre Anwendung oder Ihr Dienst mit einem Microsoft-Konto authentifiziert wurde, werden nur zwei einfache Rest-Aufrufe zum Generieren von [Aktivitäts](/graph/api/resources/projectrome-activity) [-und](/graph/api/resources/projectrome-historyitem) Verlaufs Objekten erstellt. dabei werden dieselben Daten wie oben beschrieben verwendet.

## <a name="summary"></a>Zusammenfassung

Sie können die [useractivity](/uwp/api/windows.applicationmodel.useractivities) -API verwenden, um Ihre APP in der Zeitachse und in Cortana anzuzeigen.
* Weitere Informationen zur [ **useractivity** -API](/uwp/api/windows.applicationmodel.useractivities)
* Sehen Sie sich den [Beispielcode](https://github.com/Microsoft/project-rome)an.
* [Weitere anspruchsvollere Adaptive Karten](https://adaptivecards.io/)finden Sie unter.
* Veröffentlichen Sie über [Microsoft Graph](https://developer.microsoft.com/graph)eine **useractivity** von IOS, Android oder Ihrem Webdienst.
* Weitere Informationen zu [Project Rom finden Sie auf GitHub](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>Wichtige APIs

* [UserActivities-Namespace](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Zugehörige Themen

* [Benutzeraktivitäten (Project Rom-Dokumentation)](/windows/project-rome/user-activities/)
* [Adaptive Karten](/adaptive-cards/)
* [Bildschirm Abbildung von adaptiven Karten, Beispiele](https://adaptivecards.io/)
* [Behandeln der URI-Aktivierung](./handle-uri-activation.md)
* [Verwenden der Microsoft Graph, des Aktivitäts Feeds und adaptiver Karten für Ihre Kunden auf beliebigen Plattformen](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)