---
Description: Verwenden Sie Benachrichtigungen über eine Kachel, um herauszufinden, was Ihre APP auf der Live-Kachel angezeigt hat, als der Benutzer darauf geklickt hat.
title: Verfolgbare Kachelbenachrichtigungen
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, verwertbare Kacheln, Live-Kacheln, Benachrichtigungen zu beschreibbaren Kacheln
ms.localizationpriority: medium
ms.openlocfilehash: 951dc891fb34ae4be7551c08ff47eabc19ae9eb6
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100278"
---
# <a name="chaseable-tile-notifications"></a>Verfolgbare Kachelbenachrichtigungen

Mit einer beschreibbaren Kachel Benachrichtigung können Sie feststellen, welche Kachel Benachrichtigungen von der Live-Kachel ihrer App angezeigt wurden, als der Benutzer auf die Kachel geklickt hat.  
Beispielsweise könnte eine News-App dieses Feature verwenden, um zu bestimmen, welche Nachrichten Story die Live-Kachel beim Starten des Benutzers anzeigt hat. Dadurch kann sichergestellt werden, dass die Story hervorgehoben wird, damit der Benutzer Sie finden kann. 

> [!IMPORTANT]
> Das **Anniversary Update ist erforderlich**: zur Verwendung von Verlaufs fähigen Kachel Benachrichtigungen mit c#-, C++-oder VB-basierten UWP-apps müssen Sie das SDK 14393 als Ziel verwenden und Build 14393 oder höher ausführen. Bei JavaScript-basierten UWP-apps müssen Sie auf SDK 17134 abzielen und Build 17134 oder höher ausführen. 


> **Wichtige APIs**: [launchactivatedeventargs. tileactivatedinfo-Eigenschaft](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [tileactivatedinfo-Klasse](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Funktionsweise

Um chaseable-Kachel Benachrichtigungen zu aktivieren, verwenden Sie die **Arguments** -Eigenschaft auf der Kachel Benachrichtigungs Nutzlast, ähnlich der Launch-Eigenschaft für die Toast Benachrichtigungs Nutzlast, um Informationen über den Inhalt in die Kachel Benachrichtigung einzubetten.

Wenn Ihre APP über die Live-Kachel gestartet wird, gibt das System eine Liste von Argumenten aus den aktuellen/zuletzt angezeigten Kachel Benachrichtigungen zurück.


## <a name="when-to-use-chaseable-tile-notifications"></a>Verwendung von Benachrichtigungen über die beschreibbaren Kacheln

Die Benachrichtigungen zu beschreibbaren Kacheln werden in der Regel verwendet, wenn Sie die Benachrichtigungs Warteschlange auf Ihrer Live-Kachel verwenden (was bedeutet, dass Sie bis zu 5 verschiedene Benachrichtigungen durchlaufen). Sie sind auch nützlich, wenn der Inhalt auf Ihrer Live-Kachel möglicherweise nicht mehr mit den neuesten Inhalten in der APP synchron ist. Beispielsweise wird die Live-Kachel von der News-App alle 30 Minuten aktualisiert, aber wenn die APP gestartet wird, werden die neuesten Nachrichten geladen (was möglicherweise keine etwas enthält, das sich auf der Kachel aus dem letzten Abruf Intervall befunden hat). Wenn dies der Fall ist, wird der Benutzer möglicherweise nicht mehr in der Lage sein, die Story zu finden, die er in der Live-Kachel gesehen hat. An dieser Stelle können beschreibbare Kachel Benachrichtigungen helfen, indem Sie sicherstellen können, dass das, was der Benutzer auf der Kachel gesehen hat, leicht erkennbar ist.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Vorgehensweisen bei einer beschreibbaren Kachel Benachrichtigung

Wichtig zu beachten ist, dass Sie in den meisten Szenarien **nicht direkt zu der spezifischen Benachrichtigung navigieren sollten** , die auf der Kachel war, als der Benutzer darauf geklickt hat. Ihre Live-Kachel wird als Einstiegspunkt für Ihre Anwendung verwendet. Es gibt zwei Szenarios, in denen ein Benutzer auf die Live-Kachel klickt: (1) die APP sollte normal gestartet werden, oder (2) Sie möchten weitere Informationen zu einer bestimmten Benachrichtigung anzeigen, die auf der Live-Kachel war. Da es nicht möglich ist, dass der Benutzer explizit das gewünschte Verhalten anweist, empfiehlt es sich, die **App normal zu starten und gleichzeitig sicherzustellen, dass die vom Benutzer erkannte Benachrichtigung leicht erkennbar ist**.

Wenn Sie beispielsweise auf die Live-Kachel der MSN News-App klicken, wird die APP normal gestartet: Sie zeigt die Startseite oder den Artikel an, den der Benutzer zuvor gelesen hat. Auf der Startseite stellt die APP jedoch sicher, dass die Story von der Live-Kachel leicht erkennbar ist. Auf diese Weise werden beide Szenarien unterstützt: das Szenario, in dem Sie die APP einfach starten/fortsetzen möchten, und das Szenario, in dem Sie die jeweilige Story anzeigen möchten.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Einschließen der Arguments-Eigenschaft in Ihre Kachel Benachrichtigungs Nutzlast

In einer Benachrichtigungs Nutzlast ermöglicht die Arguments-Eigenschaft ihrer APP, Daten bereitzustellen, die Sie verwenden können, um die Benachrichtigung zu einem späteren Zeitpunkt zu identifizieren. Ihre Argumente können z. b. die Story-ID enthalten, sodass Sie beim Start die Story abrufen und anzeigen können. Die-Eigenschaft akzeptiert eine Zeichenfolge, die Sie beliebig serialisieren können (Abfrage Zeichenfolge, JSON usw.), aber es wird empfohlen, das Format der Abfrage Zeichenfolge zu empfehlen, da es sich um eine fein-und XML-Codierung handelt.

Die-Eigenschaft kann sowohl für die **tilevisual** -als auch für die **tilebinding** -Elemente festgelegt werden und wird nach unten weitergegeben. Wenn Sie für jede Kachel Größe die gleichen Argumente wünschen, legen Sie einfach die Argumente für das **tilevisual**fest. Wenn Sie bestimmte Argumente für bestimmte Kachel Größen benötigen, können Sie die Argumente für einzelne **tilebinding** -Elemente festlegen.

In diesem Beispiel wird eine Benachrichtigungs Nutzlast erstellt, bei der die Arguments-Eigenschaft verwendet wird, damit die Benachrichtigung später identifiziert werden kann. 

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


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Überprüfen der Arguments-Eigenschaft beim Starten der APP

Die meisten apps verfügen über eine APP.XAML.cs-Datei, die eine außer Kraft setzung für die [ongestartete](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) -Methode enthält. Wie der Name bereits vermuten lässt, ruft Ihre APP diese Methode auf, wenn Sie gestartet wird. Es wird ein einzelnes Argument, ein [launchactivatedeventargs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) -Objekt, benötigt.

Das launchactivatedeventargs-Objekt verfügt über eine-Eigenschaft, die Lösch Bare Benachrichtigungen ermöglicht: die [tileactivatedinfo-Eigenschaft](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), die den Zugriff auf ein [tileactivatedinfo-Objekt](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)ermöglicht. Wenn der Benutzer Ihre APP über seine Kachel (anstatt über die APP-Liste, die Suche oder einen anderen Einstiegspunkt) gestartet, initialisiert Ihre APP diese Eigenschaft.

Das [tileactivatedinfo-Objekt](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) enthält eine Eigenschaft namens [recentlyshownbenachrichtigungen](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), die eine Liste der Benachrichtigungen enthält, die in den letzten 15 Minuten auf der Kachel angezeigt wurden. Das erste Element in der Liste stellt die Benachrichtigung dar, die derzeit auf der Kachel enthalten ist, und die nachfolgenden Elemente stellen die Benachrichtigungen dar, die der Benutzer vor dem aktuellen Element gesehen hat. Wenn die Kachel gelöscht wurde, ist diese Liste leer.

Jede showntilenotifiweist eine Arguments-Eigenschaft auf. Die Arguments-Eigenschaft wird mit der Argument Zeichenfolge aus ihrer Kachel Benachrichtigungs Nutzlast initialisiert, oder NULL, wenn Ihre Nutzlast nicht die Argument Zeichenfolge enthielt.

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


### <a name="accessing-onlaunched-from-desktop-applications"></a>Zugreifen auf ongestartete über Desktop Anwendungen

Desktop-Apps (z. b. WPF usw.) können mithilfe der [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)auch beschreibbare Kacheln verwenden. Der einzige Unterschied besteht darin, auf die ongestartete Argumente zuzugreifen. Beachten Sie, dass Sie [Ihre APP zunächst mit der Desktop Bridge Verpacken](/windows/msix/desktop/source-code-overview)müssen.

> [!IMPORTANT]
> **Erfordert das Update vom Oktober 2018**: um die API zu verwenden `AppInstance.GetActivatedEventArgs()` , müssen Sie das SDK 17763 als Ziel verwenden und Build 17763 oder höher ausführen.

Führen Sie für Desktop Anwendungen die folgenden Schritte aus, um auf die Start Argumente zuzugreifen...

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


## <a name="raw-xml-example"></a>XML-Beispiel Beispiel

Wenn Sie anstelle der Benachrichtigungs Bibliothek unformatierte XML-Daten verwenden, hier ist die XML-Datei.

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

- [Launchactivatedeventargs. tileactivatedinfo (Eigenschaft)](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Tileactivatedinfo-Klasse](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)