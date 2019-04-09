---
Description: Verwenden Sie verfolgbare Kachelbenachrichtigungen, um herauszufinden, was Ihre App auf der Live-Kachel anzeigt, wenn der Benutzer darauf geklickt hat.
title: Verfolgbare Kachelbenachrichtigungen
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, Uwp, verfolgbare Kacheln, Live-Kacheln, verfolgbare Kachelbenachrichtigungen
ms.localizationpriority: medium
ms.openlocfilehash: 6e27dec0e7256cfc035ecc3150bd976f69743fe3
ms.sourcegitcommit: f15cf141c299bde9cb19965d8be5198d7f85adf8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2019
ms.locfileid: "58358615"
---
# <a name="chaseable-tile-notifications"></a>Verfolgbare Kachelbenachrichtigungen

Mit den verfolgbaren Kachelbenachrichtigungen können Sie festlegen, welche Kachelbenachrichtigungen die Live-Kachel Ihrer App anzeigen sollte, wenn der Benutzer auf die Kachel geklickt hat.  
Beispielsweise könnte eine Nachrichtenanwendung diese Funktion verwenden, um herauszufinden, welche Nachrichtenstory ihre Live-Kachel angezeigt hat. Sie könnte sicherstellen, dass die Story prominent angezeigt wird, damit der Benutzer sie finden kann. 

> [!IMPORTANT]
> **Erfordert Anniversary Update**: Mit chaseable Tile-Benachrichtigungen mit C#, C++ und VB-basierte UWP-apps, Sie müssen als Ziel SDK 14393 und Build 14393 oder höher ausgeführt werden. Für JavaScript-basierte UWP-Apps müssen Sie SDK 17134 als Zielobjekt auswählen und Build 17134 oder höher verwenden. 


> **Wichtige APIs:** [LaunchActivatedEventArgs.TileActivatedInfo Eigenschaft](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [TileActivatedInfo-Klasse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Funktionsweise

Um verfolgbare Kachelbenachrichtigungen zu aktivieren, verwenden Sie die **Argumente**-Eigenschaft auf den Kachelbenachrichtigungs-Payload, ähnlich der Launch-Eigenschaft auf den Toast-Benachrichtigungs-Payload, um Informationen über den Inhalt in die Kachelbenachrichtigung einzubetten.

Wenn Ihre App über die Live-Kachel gestartet wird, gibt das System eine Liste der Argumente aus den aktuell/zuletzt angezeigten Kachelbenachrichtigungen zurück.


## <a name="when-to-use-chaseable-tile-notifications"></a>Wann werden Benachrichtigungen über verfolgbare Kacheln verwendet?

Verfolgbare Kachelbenachrichtigungen werden normalerweise verwendet, wenn Sie die Benachrichtigungswarteschlange für Ihrer Live-Kachel verwenden (was bedeutet, dass Sie bis zu 5 verschiedene Benachrichtigungen durchlaufen). Sie sind auch dann von Vorteil, wenn die Inhalte in Live-Kacheln möglicherweise nicht mit den neuesten Inhalten in der App synchronisiert sind. Zum Beispiel aktualisiert die Nachrichtenanwendung ihre Live-Kachel alle 30 Minuten. Aber wenn die Anwendung gestartet wird, lädt sie die neuesten Nachrichten (die möglicherweise etwas nicht enthalten, das sich auf der Kachel aus dem letzten Polling-Intervall befand). Wenn das passiert, könnte der Benutzer frustriert darüber sein, dass er die Story, die er auf der Live-Kachel gesehen hat, nicht finden kann. Hier helfen Ihnen verfolgbare Kachelbenachrichtigungen, indem Sie sicherstellen können, dass das, was der Benutzer auf der Kachel gesehen hat, leicht erkennbar ist.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Was tun mit einer verfolgbaren Kachelbenachrichtigung?

Der wichtigste Aspekt ist, dass Sie in den meisten Szenarien **NICHT direkt zu der spezifischen Benachrichtigung navigieren sollten**, die sich auf der Kachel befand, als der Benutzer darauf geklickt hat. Ihr Live-Kachel wird als Einstiegspunkt in Ihre Anwendung verwendet. Es kann zwei Szenarien vorhanden sein, wenn ein Benutzer auf Ihre Live-Kachel klickt: (1) sie verwenden wollten, Ihre app normal zu starten, oder (2) sie weitere Informationen zu einer bestimmten Benachrichtigung sehen, die auf der Live-Kachel war wollten. Da es für den Benutzer keine Möglichkeit gibt, explizit anzugeben, welches Verhalten er wünscht, ist es das ideale Erlebnis, **Ihre Anwendung normal zu starten und gleichzeitig sicherzustellen, dass die Benachrichtigung, die der Benutzer gesehen hat, leicht erkennbar ist**.

Wenn Sie beispielsweise auf die Live-Kachel der MSN News-App klicken, wird die Anwendung normal gestartet: Sie zeigt die Startseite oder den Artikel an, den der Benutzer zuvor gelesen hat. Auf der Startseite sorgt die App jedoch dafür, dass die Story aus der Live-Kachel leicht auffindbar ist. Auf diese Weise werden beide Szenarien unterstützt: das Szenario, in dem Sie einfach nur die Anwendung starten/fortsetzen möchten, und das Szenario, in dem Sie die spezifische Story anzeigen möchten.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Einbeziehen der Argumente-Eigenschaft in Ihren Kachelbenachrichtigungs-Payload

Bei einem Benachrichtigungs-Payload ermöglicht die Eigenschaft „Arguments“ Ihrer App, Daten zur Verfügung zu stellen, mit denen Sie die Benachrichtigung später identifizieren können. Ihre Argumente könnten z. B. die ID der Story enthalten, so dass Sie beim Start die Story abrufen und anzeigen können. Die Eigenschaft akzeptiert eine Zeichenfolge, die nach Belieben serialisiert werden kann (Abfrage, JSON etc.), aber wir empfehlen in der Regel eine Abfrage, da diese klein und von XML codierbar ist.

Die Eigenschaft kann sowohl auf die Elemente **TileVisual** als auch **TileBinding** festgelegt werden und wird nach unten hin kaskadiert. Wenn Sie die gleichen Argumente für jede Kachelgröße möchten, setzen Sie einfach die Argumente auf **TileVisual**. Wenn Sie spezifische Argumente für bestimmte Kachelgrößen benötigen, können Sie die Argumente auf einzelne **TileBinding**-Elemente setzen.

In diesem Beispiel wird ein Payload für Benachrichtigungen erstellt, de die Arguments-Eigenschaft verwendet, damit Benachrichtigungen später identifiziert werden können. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>So überprüfen Sie die Arguments-Eigenschaft, wenn Ihre Anwendung startet

Die meisten Apps haben eine Datei App.xaml.cs, die eine Überschreibung der [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_)-Methode enthält. Wie der Name schon sagt, ruft Ihre App diese Methode beim Start auf. Sie benötigt ein einziges Argument, ein [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs)-Objekt.

Das LaunchActivatedEventArgs-Objekt hat eine Eigenschaft, die verfolgbare Benachrichtigungen ermöglicht: die [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)-Eigenschaft, die den Zugriff auf ein [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)-Objekt ermöglicht. Wenn der Benutzer Ihre Anwendung startet, wird diese Eigenschaft initialisiert, wenn Sie Ihre Anwendung aus ihrer Kachel starten (anstatt aus der App-Liste, der Suche oder einem anderen Eingabepunkt).

Das [TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)-Objekt enthält eine Eigenschaft namens [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), die eine Liste von Benachrichtigungen enthält, die innerhalb der letzten 15 Minuten auf der Kachel angezeigt wurden. Das erste Element in der Liste stellt die Benachrichtigung dar, die gerade auf de Kachel angezeigt wird, und die folgenden Elemente stellen die Benachrichtigungen dar, die der Benutzer vor dem aktuellen Element gesehen hat. Wenn Ihre Kachel gelöscht wurde, ist diese Liste leer.

Jede ShownTileNotification verfügt über eine Arguments-Eigenschaft. Arguments-Eigenschaft wird mit der Argumentzeichenfolge "von Ihrem Nutzlast der Tile-Benachrichtigung oder Null initialisiert werden, wenn Ihre Nutzlast die Argumentzeichenfolge nicht enthalten waren.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


### <a name="accessing-onlaunched-from-desktop-applications"></a>Zugreifen auf OnLaunched von desktop-Anwendungen

Desktop-apps (wie Win32, WPF, usw.) mithilfe der [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop), chaseable Kacheln können zu verwenden! Der einzige Unterschied ist die OnLaunched-Argumente zugreifen. Beachten Sie, dass Sie zuerst müssen [Verpacken der app mit Desktop Bridge](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root).

> [!IMPORTANT]
> **Erfordert Update für Oktober 2018**: Verwenden der `AppInstance.GetActivatedEventArgs()` -API, Sie müssen als Ziel SDK 17763 und Build 17763 oder höher ausgeführt werden.

Bei desktop-Anwendungen für den Zugriff auf den Argumenten starten, gehen Sie...

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>Beispiel mit unformatiertem XML

Wenn Sie mit unformatiertes XML anstatt der Notifications-Bibliothek verwenden, finden Sie hier den XML-Code.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Verwandte Artikel

- [LaunchActivatedEventArgs.TileActivatedInfo property](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo-Klasse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)
