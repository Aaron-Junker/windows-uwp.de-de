---
Description: Verwenden Sie Cortana-Sprachbefehle, Spracherkennung und Sprachsynthese zum Integrieren von Sprache in Ihre Apps.
title: Surface Dial-Interaktionen
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, Windows-Radgeräte, RadialController, Radial-Controller, Benutzerinteraktion, Eingabe
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: bcdb8ca6843d126bc245e48f0b50209890740819
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639615"
---
# <a name="surface-dial-interactions"></a>Surface Dial-Interaktionen

![Abbildung des Surface Dial mit Surface Studio](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial mit Surface Studio und Stift* (im [Microsoft Store](https://aka.ms/purchasesurfacedial) käuflich erhältlich).

## <a name="overview"></a>Übersicht

Windows Wheel-Geräte wie das Surface Dial bilden eine neue Kategorie von Eingabegeräten, die eine Vielzahl attraktiver, innovativer Benutzerinteraktionen für Windows und Windows-Apps unterstützen. 

> [!IMPORTANT]
> In diesem Thema gehen wir speziell auf Interaktionen mit dem Surface Dial ein, die Informationen treffen jedoch auf alle Windows Wheel-Geräte zu. 

| Videos |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Surface Dial-app-Partner* | *Surface Dial für Entwickler* |

Der Formfaktor des Surface Dial entspricht einer *Dreh*-Aktion (oder -Geste). Das Surface Dial soll als sekundäres, multimodales Eingabegerät genutzt werden, das Eingaben über ein primäres Gerät ergänzt. In den meisten Fällen wird das Gerät von einem Benutzer mit der nicht dominanten Hand bedient, während er mit seiner dominanten Hand eine Aufgabe ausführt (z. B. Freihandzeichnen mit einem Stift). Es wurde nicht für präzise Zeigereingaben konzipiert (wie Touch-, Stift- oder Mauseingaben). 

Das Surface Dial unterstützt außerdem eine *Drücken-und-Halten*-Aktion und eine *Klick*-Aktion. „Drücken und Halten“ hat nur eine Funktion: die Anzeige eines Befehlsmenüs. Wenn das Menü aktiv ist, wird die Dreh- und Klickeingabe vom Menü verarbeitet. Andernfalls wird die Eingabe zur Verarbeitung an Ihre App übergeben. 

**Wie bei allen Windows-Eingabegeräte, können Sie anpassen und die Surface Dial Interaktion geboten, die Funktionalität in Ihren apps entsprechend anpassen.**

> [!TIP]
> Im Zusammenspiel mit dem neuen Surface Studio bietet das Surface Dial Benutzern ein noch innovativeres Bedienerlebnis.  
>
>Das Surface Dial unterstützt nicht nur die beschriebene Standardmenüaktion „Drücken und Halten“, sondern kann zusätzlich direkt auf dem Bildschirm des Surface Studio platziert werden. Dadurch wird ein spezielles Onscreen-Menü aktiviert. 
>
>Das System erkennt sowohl die Auflageposition als auch die Begrenzungen des Surface Dial, kann anhand dieser Informationen auf die durch das Gerät verursachte Okklusion reagieren und eine größere Menüversion darstellen, die außen um die Drehsteuerung herum angezeigt wird. Auch die App kann diese Informationen nutzen, um die Benutzeroberfläche an das Vorhandensein des Geräts und dessen beabsichtigte Nutzung anzupassen, z. B. daran, wie der Benutzer seine Hand und seinen Arm platziert.

| Offscreen-Menü des Surface Dial | | Onscreen-Menü des Surface Dial |
| --- | --- | --- |
| ![Offscreen-Menü des Surface Dial](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Onscreen-Menü des Surface Dial](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>Systemintegration

Das Surface Dial ist eng in Windows integriert und unterstützt eine Reihe integrierter Menütools, wie Systemlautstärke, Scrollen, Vergrößern/Verkleinern und Rückgängigmachen/Wiederholen.

Diese integrierten Tools passen sich an den aktuellen Systemkontext an und umfassen:
- Ein Tool für die Systemhelligkeit, wenn der Benutzer mit dem Windows-Desktop arbeitet
- Ein Tool für den vorherigen/nächsten Titel bei der Medienwiedergabe

Das Surface Dial bietet nicht nur allgemeine Plattformunterstützung, sondern ist auch nahtlos in die Windows Ink-Plattformsteuerelemente ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) und [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)) integriert.

![Surface Dial mit Surface-Stift](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Dial mit Surface-Stift*

Bei Verwendung mit dem Surface Dial bieten diese Steuerelemente zusätzliche Funktionen, mit denen Freihandattribute geändert und die Linealschablone der Freihandsymbolleiste gesteuert werden können.

Wenn Sie das Surface Dial-Menü in einer Freihandanwendung öffnen, die die Freihandsymbolleiste verwendet, enthält das Menü jetzt Tools zum Steuern des Stifttyps und der Pinselstärke. Wenn das Lineal aktiviert ist, wird das Menü durch ein entsprechendes Tool ergänzt, durch das das Gerät die Position und den Winkel des Lineals steuern kann.

![Surface Dial-Menüs mit Tools "Stift Auswahl" für die Windows Ink-Symbolleiste](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Surface Dial-Menüs mit Tools "Stift Auswahl" für die Windows Ink-Symbolleiste*

![Surface Dial-Menü mit Stroke Größe Tool für die Windows Ink-Symbolleiste](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Surface Dial-Menü mit Stroke Größe Tool für die Windows Ink-Symbolleiste*

![Surface Dial-Menü, mit dem Lineal-Tool für die Windows Ink-Symbolleiste](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Surface Dial-Menü, mit dem Lineal-Tool für die Windows Ink-Symbolleiste*

## <a name="user-customization"></a>Benutzeranpassung

Benutzer können einige Aspekte der Surface Dial-Bedienung auf der Seite **Windows-Einstellungen -> Geräte -> Rad** anpassen, einschließlich Standardtools, Vibration (oder haptisches Feedback) und schreibende (oder dominante) Hand. 

Beim Anpassen der Benutzererfahrung mit dem Surface Dial sollten Sie immer sicherstellen, dass eine bestimmte Funktion oder ein bestimmtes Verhalten verfügbar ist und vom Benutzer aktiviert wird.

## <a name="custom-tools"></a>Benutzerdefinierte Tools

Dieses Thema enthält sowohl Erläuterungen zur Benutzeroberfläche als auch Erläuterungen für Entwickler, die sich auf die Anpassung der Tools im Surface Dial-Menü beziehen.

### <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

**Achten Sie darauf, dass Ihre Tools dem aktuellen Kontext entsprechen.** Wenn die Funktionsweise eines Tools und die Interaktionsmöglichkeiten mit dem Surface Dial schnell und intuitiv vom Benutzer erfasst werden können, helfen Sie ihm, sich auf seine Arbeit zu konzentrieren.

**Minimieren Sie die Anzahl von app-Tools so weit wie möglich**  
Das Surface Dial-Menü bietet Platz für sieben Elemente. Bei acht oder mehr Elementen muss der Benutzer die Drehsteuerung bedienen, um die verfügbaren Tools in einem Erweiterungsflyout anzuzeigen. Das Navigieren im Menü wird schwieriger, und die verfügbaren Tools sind nicht mehr einfach zu erkennen und auszuwählen.

Wir empfehlen, ein einzelnes benutzerdefiniertes Tool für Ihre App oder Ihren App-Kontext bereitzustellen. Dadurch können Sie das Tool auf Grundlage der aktuellen Benutzeraktion festlegen, ohne dass der Benutzer das Surface Dial-Menü aktivieren und ein Tool auswählen muss. 

**Die Sammlung von Tools dynamisch zu aktualisieren**  
Da Elemente im deaktivierten Zustand im Surface Dial-Menü nicht unterstützt werden, sollten Sie Tools (einschließlich integrierter Tools und Standardtools) basierend auf dem Benutzerkontext (aktuelle Ansicht oder Fenster im Fokus) dynamisch hinzufügen und entfernen. Wenn ein Tool für die aktuelle Aktivität nicht relevant oder redundant ist, entfernen Sie es.

> [!IMPORTANT]
> Wenn Sie dem Menü ein Element hinzufügen, vergewissern Sie sich, dass es nicht bereits vorhanden ist.

**Entfernen Sie nicht das integrierte System-Tool für die Volume-Einstellung**  
Normalerweise benötigt der Benutzer immer eine Lautstärkeregelung. Da er bei Verwendung Ihrer App u. U. Musik hört, sollten Tools zum Regeln der Lautstärke und zur Auswahl des nächsten Titels immer im Surface Dial-Menü verfügbar sein. (Wenn Medien wiedergegeben werden, wird dem Menü automatisch das Tool zur Auswahl des nächsten Titels hinzugefügt.)

**Mit Menü Organisation konsistent sein**  
Dadurch erkennen Benutzer bei Verwendung Ihrer App leichter, welche Tools verfügbar sind, und können effizienter zwischen Tools wechseln.

**Geben Sie qualitativ hochwertige-Symbole, die konsistent mit der integrierten Symbole**  
Symbole können Benutzern Professionalität, Kompetenz und Vertrauenswürdigkeit vermitteln.
- Stellen Sie ein hochwertiges PNG-Bild von 64 x 64 Pixeln bereit (44 x 44 ist die kleinste unterstützte Größe)
- Stellen Sie sicher, dass der Hintergrund transparent ist.
- Das Symbol sollte den größten Teil des Bilds ausfüllen.
- Ein weißes Symbol sollte über eine schwarze Kontur verfügen, damit es im Modus mit hohem Kontrast sichtbar ist.

|   |   |   |
| --- | --- | --- |
| ![Symbol mit Alphahintergrund](images/windows-wheel/surface-dial-menu-icon1.png) | ![Bei Verwendung des Standarddesigns im Radmenü angezeigtes Symbol](images/windows-wheel/surface-dial-menu-icon2.png) | ![Onscreen-Menü des Surface Dial](images/windows-wheel/surface-dial-menu-icon3.png) |
| *Symbol mit alpha-Hintergrund* | *Symbol, das Rad-Menü mit Standarddesign angezeigt* | *Symbol für Rad-Menü, mit dem Design "hoher Kontrast (weiß)" angezeigt* |

**Verwenden Sie präzise und beschreibende Namen**  
Der Name des Tools wird im Toolmenü zusammen mit dem Toolsymbol angezeigt und auch von Sprachausgaben verwendet. 
- Namen sollten kurz sein und in den mittleren Kreis des Radmenüs passen.
- Namen sollten die primäre Aktion eindeutig identifizieren (eine ergänzende Aktion kann impliziert werden):
  - „Scrollen“ gibt an, dass beide Drehrichtungen unterstützt werden.
  - „Rückgängigmachen“ gibt eine primäre Aktion an, „Wiederholen“ (die ergänzenden Aktion) kann vom Benutzer jedoch problemlos abgeleitet und identifiziert werden.

### <a name="developer-guidance"></a>Erläuterungen für Entwickler

Sie können die Funktionsweise des Surface Dial mit einer umfassenden Reihe von [Windows-Runtime-APIs](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) anpassen, um die Funktionalität Ihrer Apps zu erweitern. 

Wie erwähnt, verfügt das Standardmenü des Surface Dial bereits über mehrere integrierte Tools, die zahlreiche grundlegende Systemfeatures abdecken (Systemlautstärke, Systemhelligkeit, Scrollen, Zoomen, Rückgängigmachen sowie Mediensteuerung, wenn das System erkennt, dass Audio- oder Videomedien wiedergegeben werden). Möglicherweise stellen diese Standardtools aber nicht die für Ihre App erforderlichen Funktionen bereit. 

In den folgenden Abschnitten wird beschrieben, wie Sie dem Surface Dial-Menü ein benutzerdefiniertes Tool hinzufügen und angeben, welche integrierten Tools verfügbar gemacht werden.

**Fügen Sie ein benutzerdefiniertes tool**

In diesem Beispiel wird ein einfaches benutzerdefiniertes Tool hinzugefügt, das die Eingabedaten sowohl von Drehereignissen als auch von Klickereignissen an einige XAML-UI-Steuerelemente übergibt.

1. Zunächst deklarieren wir unsere Benutzeroberfläche (lediglich ein Schieberegler und eine Umschaltfläche) in XAML.

   ![Abbildung der Benutzeroberfläche der Beispielapp](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *Die Benutzeroberfläche der Beispielapp*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. Anschließend fügen wir in der CodeBehind-Datei dem Surface Dial-Menü ein benutzerdefiniertes Tool hinzu und deklarieren die [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)-Eingabehandler. 

   Wir rufen [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView) auf, um für das Surface Dial (myController) einen Verweis auf das [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)-Objekt zu erhalten.

   Anschließend erstellen wir eine [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem)-Instanz (myItem), indem wir [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/mt759255) aufrufen. 

   Als Nächstes fügen wir das Element an die Sammlung der Menüelemente an.

   Wir deklarieren die Eingabeereignishandler ([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) und [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) für das [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)-Objekt.

   Zuletzt definieren wir die Ereignishandler.

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

Wenn wird die App ausführen, verwenden wir das Surface Dial, um mit ihr zu interagieren. Zuerst drücken und halten wird das Gerät, um das Menü zu öffnen und unser benutzerdefiniertes Tool auszuwählen. Sobald das benutzerdefinierte Tool aktiviert ist, kann das Schieberegler-Steuerelement durch Drehen des Surface Dial angepasst werden, und die Umschaltfläche kann durch Klicken mit dem Surface Dial umgeschaltet werden.

![Bild von der Beispiel-app-Benutzeroberfläche aktiviert, mit dem benutzerdefinierten Surface Dial-tool](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*Die Beispiel-app-Benutzeroberfläche aktiviert, mit dem benutzerdefinierten Surface Dial-tool*

**Geben Sie die integrierten tools**

Mit der [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration)-Klasse können Sie die Sammlung integrierter Menüelemente für Ihre App anpassen.

Wenn Ihre App beispielsweise keine Scroll- oder Zoombereiche aufweist und keine Funktionen zum Rückgängigmachen/Wiederholen benötigt, können diese Tools aus dem Menü entfernt werden. Dadurch bietet das Menü mehr Platz für benutzerdefinierte Tools in Ihrer App. 

> [!IMPORTANT] 
> Das Surface Dial-Menü muss mindestens über ein Menüelement verfügen. Wenn alle Standardtools entfernt werden, bevor Sie ein benutzerdefiniertes Tools hinzufügen, werden die Standardtools wiederhergestellt, und Ihr Tool wird an die Standardsammlung angefügt.

Laut Entwurfsrichtlinien wird davon abgeraten, Tools für die Mediensteuerung (Lautstärkeregelung und Wechsel zum nächsten bzw. vorherigen Titel) zu entfernen, da Benutzer häufig Musik im Hintergrund hören, während sie andere Aufgaben erledigen.

Hier wird veranschaulicht, wie Sie das Surface Dial-Menü so konfigurieren, dass es nur die Mediensteuerungen für die Lautstärke und den Wechsel zum nächsten bzw. vorherigen Titel enthält.

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>Benutzerdefinierte Interaktionen

Wie bereits erwähnt, unterstützt das Surface Dial drei Bewegungen (Drücken und Halten, Drehen, Klicken) mit den zugehörigen Standardinteraktionen. 

Stellen Sie sicher, dass benutzerdefinierte Interaktionen, die auf diesen Bewegungen basieren, für die ausgewählte Aktion oder das ausgewählte Tool sinnvoll sind. 

> [!NOTE]
> Die Verarbeitung der Interaktionen richtet sich nach dem Zustand des Surface Dial-Menüs. Falls aktiv, wird die Eingabe vom Menü verarbeitet, andernfalls von Ihrer App.

### <a name="press-and-hold"></a>Drücken und Halten

Durch diese Bewegung wird das Surface Dial-Menü aktiviert und angezeigt. Dieser Bewegung ist keine App-Funktion zugeordnet. 

Das Menü wird standardmäßig mittig auf dem Bildschirms des Benutzers angezeigt. Der Benutzer kann es jedoch an eine beliebige andere Stelle verschieben.

> [!NOTE]
> Wenn das Surface Dial auf dem Bildschirm des Surface Studio platziert wird, wird das Menü an der Onscreen-Position des Surface Dial zentriert.

### <a name="rotate"></a>Drehen

Die vom Surface Dial unterstützten Drehungen eignen sich hauptsächlich für Interaktionen, mit denen analoge Werte oder Steuerungen stufenlos inkrementiert werden.

Das Gerät kann im Uhrzeigersinn und gegen den Uhrzeigersinn gedreht werden und mittels haptischem Feedback diskrete Entfernungen angeben.

> [!NOTE]
> Haptisches Feedback kann vom Benutzer auf der Seite **Windows-Einstellungen -> Geräte -> Rad** deaktiviert werden.

#### <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

**Tools mit rotierenden Vertraulichkeit von kontinuierlichen oder hohe sollte Übermitteln von haptischem Feedback deaktiviert werden.**

Das haptische Feedback entspricht der Drehempfindlichkeit des aktiven Tools. Es wird empfohlen, haptisches Feedback für Tools mit kontinuierlicher oder hoher Drehempfindlichkeit zu deaktivieren, da andernfalls die Benutzerfreundlichkeit beeinträchtigt werden kann. 

**Vorherrschende Hand sollten Drehung-basierte Interaktionen nicht beeinträchtigen.**

Das Surface Dial erkennt nicht, welche Hand verwendet wird. Der Benutzer kann die schreibende (oder dominante) Hand jedoch unter **Windows-Einstellungen -> Gerät -> Stift & Windows Ink** festlegen.

**Gebietsschema berücksichtigen Sie für alle Interaktionen der Drehung**

Erhöhen Sie die Kundenzufriedenheit, indem Sie Interaktionen an das jeweilige Gebietsschema und Rechts-nach-links-Layouts anpassen.

Folgende Richtlinien für drehbasierte Aktionen gelten für die integrierten Tools und Befehle im Surface Dial-Menü:

|   |   |   |
| --- | --- | --- |
| Nach links<br/>Nach oben<br/>Heraus | ![Abbildung: Surface Dial](images/windows-wheel/surface-dial-rotate.png) | Nach rechts<br/>Nach unten<br/>Hinein |
|   |   |   |

| Konzeptionelle Richtung | Zuordnung des Surface Dial | Drehung im Uhrzeigersinn | Drehung gegen den Uhrzeigersinn |
| --- | --- | --- | --- |
| Horizontal | Links/Rechts-Zuordnung ausgehend von der Oberseite des Surface Dial | Nach rechts | Nach links |
| Vertikal | Oben/Unten-Zuordnung ausgehend von der linken Seite des Surface Dial | Nach unten | Nach oben |
| Z-Achse | Hinein (oder näher) wird Oben/Rechts zugeordnet<br/>Heraus (oder weiter) wird Unten/Links zugeordnet | Hinein | Heraus |

#### <a name="developer-guidance"></a>Erläuterungen für Entwickler

Während der Benutzer das Gerät dreht, werden [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)-Ereignisse auf Grundlage eines Deltas([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees)) relativ zur Drehrichtung ausgelöst. Die Empfindlichkeit (oder Auflösung) der Daten kann mit der [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees)-Eigenschaft festgelegt werden.

> [!NOTE]
> Ein Eingabeereignis für Drehungen wird standardmäßig nur an ein [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)-Objekt übergeben, wenn das Gerät mindestens um 10 Grad gedreht wird. Das Gerät vibriert bei jedem Eingabeereignis.

Im Allgemeinen wird empfohlen, haptisches Feedback zu deaktivieren, wenn die Drehauflösung auf weniger als 5 Grad festgelegt ist. Dadurch können kontinuierliche Interaktionen reibungsloser ausgeführt werden. 

Sie können haptisches Feedback für benutzerdefinierte Tools aktivieren und deaktivieren, indem Sie die [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback)-Eigenschaft festlegen.

> [!NOTE]
> Für Systemtools wie die Lautstärkeregelung kann das haptische Verhalten nicht überschrieben werden. Für diese Tools kann das haptische Feedback nur durch den Benutzer über die Einstellungsseite „Rad“ deaktiviert werden.

Das folgende Beispiel veranschaulicht, wie die Auflösung der Drehdaten angepasst und haptisches Feedback aktiviert oder deaktiviert werden.

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>Klicken Sie

Das Klicken mit dem Surface Dial ähnelt dem Klicken mit der linken Maustaste (der Drehzustand des Geräts hat keinen Einfluss auf diese Aktion).

#### <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

**Lassen nicht zuordnen einer Aktion oder einen Befehl dieser Bewegung, wenn der Benutzer aus dem Ergebnis nicht leicht erholen kann**

Jede Aktion, die in der App ausgeführt wird, weil der Benutzer mit dem Surface Dial klickt, muss umkehrbar sein. Der Benutzer sollte den Back-Stapel der App immer problemlos durchlaufen und einen früheren App-Zustand wiederherstellen können.

Bei binären Vorgängen wie „Stummschalten/Stummschaltung aufheben“ oder „Ein-/Ausblenden“ funktioniert die Klickbewegung optimal.

**Modale Tools nicht aktiviert oder deaktiviert werden, indem Sie auf dem Surface Dial**

Einige App-/Toolmodi können Konflikte mit Interaktionen verursachen, die auf Drehungen basieren, oder diese deaktivieren. Tools wie das Lineal in der Windows Ink-Symbolleiste sollten durch andere UI-Angebote ein- oder ausgeblendet werden (die Freihandsymbolleiste bietet ein integriertes [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton)-Steuerelement).

Ordnen Sie bei modalen Tools das aktive Surface Dial-Menüelement dem Zieltool oder dem zuvor ausgewählten Menüelement zu.

#### <a name="developer-guidance"></a>Erläuterungen für Entwickler

Beim Klicken mit dem Surface Dial wird ein [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked)-Ereignis ausgelöst. [  **RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) umfassen eine [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)-Eigenschaft, die die Position und den Begrenzungsbereich der Surface Dial-Auflagefläche auf dem Surface Studio-Bildschirm enthält. Wenn das Surface Dial keinen Kontakt mit dem Bildschirm hat, ist diese Eigenschaft NULL. 

### <a name="on-screen"></a>Onscreen-Modus

Wie oben beschrieben, kann das Surface Dial in Verbindung mit dem Surface Studio verwendet werden, um das Surface Dial-Menü in einem speziellen Onscreen-Modus anzuzeigen. 

In diesem Modus können die Interaktionsfunktionen zwischen Surface Dial und Ihren Apps noch weiter integriert und angepasst werden. Beispiele für einzigartige Funktionen, die nur durch das Surface Dial in Kombination mit dem Surface Studio ermöglicht werden:
- Anzeigen kontextbezogener Tools (z. B. einer Farbpalette) abhängig von der Position des Surface Dial, wodurch die Tools leichter zu finden und zu verwenden sind
- Festlegen des aktiven Tools auf Grundlage der Benutzeroberfläche, auf der das Surface Dial platziert wird
- Vergrößern eines Bildschirmbereichs basierend auf der Position des Surface Dial
- Einzigartige Spielinteraktionen basierend auf der Position auf dem Bildschirm

#### <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

**Apps sollten reagieren, wenn auf dem Bildschirm das Surface Dial erkannt wird**

Visuelles Feedback weist Benutzer darauf hin, dass das Gerät auf dem Bildschirm des Surface Studio von Ihrer App erkannt wurde.

**Passen Sie Surface Dial-bezogene-Benutzeroberfläche, die basierend auf den Gerätestandort an**

Je nachdem, wo das Gerät platziert wird, können wichtige UI-Elemente durch das Gerät selbst (oder durch den Benutzer) verdeckt werden.

**Passen Sie die Surface Dial-bezogene Benutzeroberfläche, die je nach Benutzerinteraktion**

Bei Verwendung des Geräts können Teile des Bildschirms nicht nur durch die Hardware, sondern auch durch die Hand und den Arm des Benutzers verdeckt werden. 

Welcher Bereich verdeckt wird, hängt davon ab, mit welcher Hand das Gerät bedient wird. Da das Gerät hauptsächlich mit der nicht dominanten Hand bedient werden soll, müssen Surface Dial-bezogene UI-Elemente für die vom Benutzer angegebene entgegengesetzte Hand angepasst werden (Einstellung unter **Windows-Einstellungen > Geräte > Stift & Windows Ink > Schreibhand auswählen**).

**Interaktionen sollte auf Surface Dial Position statt Verschiebung reagieren.**

Der Gerätefuß ist so konzipiert, dass er auf dem Bildschirm haftet anstatt zu gleiten, da es sich nicht um ein Präzisionszeigegerät handelt. Aus diesem Grund erwarten wir, dass Benutzer das Surface Dial eher anheben und an einer anderen Stelle platzieren, anstatt es über den Bildschirm zu ziehen.

**Verwenden Sie Bildschirmposition, um zu bestimmen, der Benutzerabsicht**

Wenn Sie das aktive Tool in Abhängigkeit vom UI-Kontext festlegen, z. B. von der Nähe zu einem Steuerelement, Zeichenbereich oder Fenster, können Sie die Benutzerfreundlichkeit verbessern, da sich die zum Ausführen einer Aufgabe erforderlichen Schritte verringern.

#### <a name="developer-guidance"></a>Erläuterungen für Entwickler

Wenn das Surface Dial auf der Digitalisiereroberfläche des Surface Studio platziert wird, wird ein [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted)-Ereignis ausgelöst, und die Informationen zum Bildschirmkontakt([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact)) werden an die App übermittelt.

Auch, wenn mit dem Surface Dial geklickt wird, während es sich auf der Digitalisiereroberfläche des Surface Studio befindet, wird ein [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked)-Ereignis ausgelöst, und die Informationen zum Bildschirmkontakt ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)) werden an die App übermittelt. 

Die Informationen zum Bildschirmkontakt ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) enthalten die X/Y-Koordinate für die Mitte des Surface Dial im Koordinatenbereich der App ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position)) und das umgebende Rechteck ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds)) in geräteunabhängigen Pixeln (DIPs). Diese Informationen sind sehr hilfreich, wenn es darum geht, Kontext für das aktive Tool bereitzustellen und dem Benutzer gerätebezogenes visuelles Feedback zu geben.

Im folgenden Beispiel haben wir eine grundlegende App mit vier verschiedenen Abschnitten erstellt. Jeder Abschnitt enthält einen Schieberegler und eine Umschaltfläche. Anschließend bestimmen wir ausgehend von der Bildschirmposition des Surface Dial, welche Gruppe von Schieberegler und Umschaltfläche durch das Surface Dial gesteuert wird.

1. Zunächst deklarieren wir die Benutzeroberfläche (vier Abschnitte, die jeweils einen Schieberegler und eine Umschaltfläche enthalten) in XAML.

   ![Abbildung der Benutzeroberfläche der Beispielapp](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *Die Benutzeroberfläche der Beispielapp*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. Hier der CodeBehind mit definierten Handlern für die Bildschirmposition des Surface Dial.

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

Wenn wird die App ausführen, verwenden wir das Surface Dial, um mit ihr zu interagieren. Zunächst platzieren wir das Gerät auf dem Surface Studio-Bildschirm. Es wird von der App erkannt und dem unteren rechten Abschnitt zugeordnet (siehe Abbildung). Anschließend drücken und halten wir das Surface Dial, um das Menü zu öffnen und das benutzerdefinierte Tool auszuwählen. Sobald das benutzerdefinierte Tool aktiviert ist, kann das Schieberegler-Steuerelement durch Drehen des Surface Dial angepasst werden, und die Umschaltfläche kann durch Klicken mit dem Surface Dial umgeschaltet werden.

![Bild von der Beispiel-app-Benutzeroberfläche aktiviert, mit dem benutzerdefinierten Surface Dial-tool](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*Die Beispiel-app-Benutzeroberfläche aktiviert, mit dem benutzerdefinierten Surface Dial-tool*

## <a name="summary"></a>Zusammenfassung

Dieses Thema bietet eine Übersicht über das Eingabegerät Surface Dial, enthält Erläuterungen zur Benutzeroberfläche und für Entwickler und veranschaulicht die Anpassung der Benutzerumgebung sowohl für Offscreen-Szenarien als auch für Onscreen-Szenarien bei Verwendung mit dem Surface Studio.

## <a name="feedback"></a>Feedback senden

Bitte senden Sie Fragen, Vorschläge und Feedback an [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com).

## <a name="related-articles"></a>Verwandte Artikel

### <a name="api-reference"></a>API-Referenz

- [**RadialController** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** Klasse](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** class](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon** Enumeration](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind** enum](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Beispiele

[Beispiele für universelle Windows-Plattform (C# und C++)](https://go.microsoft.com/fwlink/?linkid=832713)

[Windows-classic-desktop-Beispiel](https://aka.ms/radialcontrollerclassicsample)