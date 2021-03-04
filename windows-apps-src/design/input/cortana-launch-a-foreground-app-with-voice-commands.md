---
title: Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana – Cortana UWP-Entwurf und -Entwicklung
description: Verwenden Sie Sprachbefehle, um Ihre APP im Vordergrund zu aktivieren und eine Aktion oder einen Befehl in der APP auszuführen.
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 4b17a93d950d48209637cedf89bb49759ba0cf1f
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824324"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.
>
> Informationen dazu, wie Cortana moderne Produktivitäts Umgebungen transformiert, finden Sie [unter Cortana in Microsoft 365](/microsoft-365/admin/misc/cortana-integration) .

Zusätzlich zur Verwendung von Sprachbefehlen in **Cortana** für den Zugriff auf Systemfeatures können Sie **Cortana** auch mit Features und Funktionalität von Ihrer App erweitern. Durch die Verwendung von Sprachbefehlen kann Ihre App im Vordergrund aktiviert und eine Aktion oder ein Befehl in der App ausgeführt werden.

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Wenn eine App im Vordergrund einen Sprachbefehl verarbeitet, steht sie im Fokus, und Cortana wird geschlossen. Falls gewünscht, können Sie Ihre App aktivieren und einen Befehl als Hintergrundaufgabe ausführen. In diesem Fall steht Cortana im Fokus, und Ihre App gibt das gesamte Feedback und alle Ergebnisse über die **Cortana**-Canvas und die **Cortana**-Stimme zurück.

Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern (z. B. das Senden einer Nachricht an einen bestimmten Kontakt), lassen sich am besten in einer Vordergrund-App verarbeiten, während einfache Befehle in **Cortana** durch eine Hintergrund-App verarbeitet werden können.

Wenn Sie eine APP im Hintergrund mithilfe von Sprachbefehlen aktivieren möchten, finden Sie weitere Informationen unter [Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen](cortana-launch-a-background-app-with-voice-commands.md).

> [!NOTE]
> Ein Sprachbefehl ist eine einzelne Äußerung mit einer bestimmten Absicht, die in einer sprach Befehls Definitionsdatei (VCD) definiert ist, die an eine installierte App über **Cortana** gerichtet ist.
>
> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.
>
> Sprach Befehlsdefinitionen können von der Komplexität abweichen. Sie können alles von einer einzelnen, eingeschränkten Äußerung zu einer Sammlung flexibler, natürlicher Spracheingaben unterstützen, die alle dieselbe Absicht bezeichnen.

Zum Veranschaulichen der Features der Vordergrund-App verwenden wir eine Reise-und Verwaltungs-App namens **Adventure Works** aus dem [Cortana Voice-Befehlsbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899).

Zum Erstellen einer neuen Reise in **Adventure Works** ohne **Cortana** startet ein Benutzer die App und navigiert zur Seite **Neue Reise**. Zum Anzeigen einer vorhandenen Reise startet ein Benutzer die App, navigiert zur Seite **Bevorstehende Reisen** und wählt die Reise aus.

Mit den Sprachbefehlen über **Cortana** kann der Benutzer stattdessen einfach sagen „Adventure Works – Reise hinzufügen“ oder „Reise in Adventure Works hinzufügen“, um die App zu starten und zur Seite **Neue Reise** zu navigieren. Sagt der Benutzer dagegen „Adventure Works, zeige meine Reise nach London an“, wird die App mit der hier dargestellten Seite mit den Details zur **Reise** geöffnet.

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Screenshot der Cortana-Start-Vordergrund-App":::

Die grundlegenden Schritte zum Hinzufügen der Sprachbefehlserkennung und zum Integrieren von Cortana in Ihre App mithilfe der Sprach- oder Tastatureingabe lauten wie folgt:

1. Erstellen einer VCD-Datei Hierbei handelt es sich um ein XML-Dokument, das alle Sprachbefehle definiert, mit denen der Benutzer Aktionen initiieren oder Befehle aufrufen kann, wenn er Ihre App aktiviert. Informationen finden Sie unter [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2).
2. Registrieren Sie die Befehlssätze in der VCD-Datei, wenn die App gestartet wird.
3. Verarbeiten Sie die Aktivierung per Sprachbefehl, die Navigation innerhalb der App und die Befehlsausführung.

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

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>Erstellen einer neuen Lösung mit einem Projekt in Visual Studio

1. Starten Sie Microsoft Visual Studio 2015.

    Der Visual Studio 2015-Startbildschirm wird angezeigt.

2. Wählen Sie im Menü **Datei** die Option **Neu** > **Projekt**.

    Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie die Art der anzuzeigenden Vorlagen auswählen.

3. Erweitern Sie im linken Bereich **installierte > Vorlagen > Visual C \# > Windows**, und wählen Sie dann die Gruppe **universelle** Vorlage aus. Im mittleren Bereich des Dialogfelds sehen Sie eine Liste mit Projektvorlagen für Apps der universellen Windows-Plattform (UWP).
4. Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.

    Die Vorlage **Leere App** stellt eine minimale UWP-App bereit, die kompiliert und ausgeführt wird, aber keine Steuerelemente oder Daten für die Benutzeroberfläche enthält. Die App wird später in diesem Lernprogramm noch mit Steuerelementen versehen.

5. Geben Sie in das Textfeld **Name** Ihren Projektnamen ein. In diesem Beispiel verwenden wir „AdventureWorks“.
6. Klicken Sie auf **OK**, um das Projekt zu erstellen.

    Microsoft Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer** an.

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>Hinzufügen von Bildressourcen zum Projekt und Angeben im App-Manifest

UWP-Apps können automatisch die am besten geeigneten Bilder basierend auf bestimmten Einstellungen und Gerätefunktionen (hoher Kontrast, effektive Pixel, Gebietsschema und so weiter) auswählen. Sie müssen lediglich die Bilder bereitstellen und sicherstellen, dass Sie die entsprechende Benennungskonvention und Ordnerstruktur innerhalb des App-Projekts für die verschiedenen Ressourcenversionen verwenden. Wenn Sie nicht die empfohlenen Ressourcenversionen bereitstellen können, werden unter Umständen je nach Voreinstellungen, Fähigkeiten, Gerätetyp und Standort des Benutzers die Eingabehilfen, Lokalisierung und Bildqualität beeinträchtigt.

Weitere Informationen zu Bild Ressourcen für hohe Kontraste und Skalierungsfaktoren finden Sie unter [Richtlinien für Kachel-und Symbol Objekte](../../app-resources/images-tailored-for-scale-theme-contrast.md).

Sie benennen Ressourcen mithilfe von Qualifizierern. Ressourcenqualifizierer sind Ordner- und Dateinamenmodifikatoren, die den Kontext angeben, in dem eine bestimmte Version einer Ressource verwendet werden soll.

Standardmäßig wird die folgende Benennungskonvention verwendet: `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Beispiel: Auf `images/logo.scale-100_contrast-white.png` wird im Code einfach durch Angabe des Stammordners und des Dateinamens verwiesen: `images/logo.png`. Weitere Informationen finden Sie unter [Benennen von Ressourcen mithilfe von Qualifizierern](/previous-versions/windows/apps/hh965324(v=win.10)).

Wir empfehlen, bei Zeichenfolgen-Ressourcendateien die Standardsprache (Beispiel: `en-US\resources.resw`) und bei Bildern den Standardskalierungsfaktor (Beispiel: `logo.scale-100.png`) selbst dann zu markieren, wenn diese Dateien derzeit nicht lokalisiert bzw. keine Ressourcen in unterschiedlichen Auflösungen bereitgestellt werden sollen. Es wird jedoch empfohlen, dass Sie mindestens Ressourcen für die Skalierungsfaktoren 100, 200 und 400 bereitstellen.

> [!IMPORTANT]
> Das App-Symbol, das im Titelbereich der **Cortana** -Canvas verwendet wird, ist das in der Datei "Package. appxmanifest" angegebene Square44x44Logo-Symbol.

## <a name="create-a-vcd-file"></a>Erstellen einer VCD-Datei

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihren primären Projektnamen, und wählen Sie **Hinzufügen > Neues Element**. Fügen Sie eine **XML-Datei** hinzu.
2. Geben Sie einen Namen für die [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Datei ein (z. B. „AdventureWorksCommands.xml“), und klicken Sie auf „Hinzufügen“.
3. Wählen Sie im **Projektmappen-Explorer** die [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Datei aus.
4. Legen Sie im Fenster **Eigenschaften** die Option **Buildvorgang** auf **Inhalt** fest, und legen Sie dann **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.

## <a name="edit-the-vcd-file"></a>Bearbeiten der VCD-Datei

Fügen Sie ein **VoiceCommands**-Element mit einem **xmlns**-Attribut hinzu, das auf `https://schemas.microsoft.com/voicecommands/1.2` zeigt.

1. Erstellen Sie für jede von Ihrer App unterstützte Sprache ein [**CommandSet**](/previous-versions/windows/dn722331(v=win.10))-Element, das die von Ihrer App unterstützten Sprachbefehle enthält.

   Sie können mehrere [**CommandSet**](/previous-versions/windows/dn722331(v=win.10))-Elemente deklarieren, jedes mit einem anderen [**xml:lang**](/previous-versions/windows/dn722331(v=win.10))-Attribut, sodass Ihre App in verschiedenen Märkten verwendet werden kann. So kann eine App für die Vereinigten Staaten beispielsweise einen [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) für Englisch und einen [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) für Spanisch umfassen.

   > [!CAUTION]
   > Für die Aktivierung einer App und die Initiierung einer Aktion per Sprachbefehl muss die App eine VCD-Datei registrieren. Die VCD-Datei muss einen [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) mit einer Sprache enthalten, die der für das Gerät ausgewählten Sprache für die Spracherkennung entspricht. Die Sprache Sprache befindet sich unter **Einstellungen > System > Sprache Sprache > Sprache Sprache**.

2. Fügen Sie ein **Command**-Element für jeden Befehl hinzu, den Sie unterstützen möchten. Jeder in einer [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) -Datei deklarierte **Befehl** muss die folgenden Informationen enthalten:

   - Ein **appname** -Attribut, das Ihre Anwendung verwendet, um den Voice-Befehl zur Laufzeit zu identifizieren.
   - Element **Example**, das eine Beschreibung enthält, wie ein Benutzer den Befehl aufrufen kann. **Cortana** zeigt dieses Beispiel, wenn der Benutzer sagt: "Was kann ich sagen?", "Help", oder Sie tippen auf " **mehr** anzeigen".
   - Element **ListenFor**, das die Wörter oder Ausdrücke enthält, die Ihre App als Befehl erkennt. Jedes **ListenFor**-Element kann Referenzen zu einem oder mehreren **PhraseList**-Elementen enthalten, die spezielle für den Befehl relevante Wörter enthalten.
  
> [!NOTE]
> **ListenFor**-Elemente können nicht programmgesteuert geändert werden. **PhraseList-Elemente**, die **ListenFor**-Elementen zugeordnet sind, können dagegen programmgesteuert geändert werden. Anwendungen sollten den Inhalt der **PhraseList** zur Laufzeit basierend auf dem Datensatz ändern, der bei der Verwendung der App durch den Benutzer generiert wird. Siehe [dynamisches Ändern von Cortana VCD-Ausdrucks Listen](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md).

Element **Feedback**, das den Text für die Anzeige und Sprachausgabe von **Cortana** beim Starten der Anwendung enthält.

Ein **Navigate**-Element gibt an, dass der Sprachbefehl die App im Vordergrund aktiviert. In diesem Beispiel ist der Befehl `showTripToDestination` eine Vordergrundaufgabe.

Ein **VoiceCommandService**-Element gibt an, dass der Sprachbefehl die App im Hintergrund aktiviert. Der Wert des Attributs **Target** dieses Elements sollte dem Wert des Attributs **Name** des Elements [**Uap:AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) in der Datei „package.appxmanifest“ entsprechen. In diesem Beispiel sind die Befehle `whenIsTripToDestination` und `cancelTripToDestination` Hintergrundaufgaben, die den Namen des App-Diensts als „AdventureWorksVoiceCommandService“ angeben.

Weitere Informationen finden Sie in der [**VCD-Elemente und -Attribute v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Referenz.

Im Folgenden finden Sie einen Teil der [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Datei, der die en-us-Sprachbefehle für die App **Adventure Works** definiert.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>Installieren der VCD-Befehle

Ihre App muss einmal ausgeführt werden, um die VCD-Datei zu installieren.

> [!NOTE]
> Sprachbefehlsdaten werden nicht übergreifend über App-Installationen beibehalten. Um sicherzustellen, dass die Sprachbefehlsdaten für Ihre App intakt bleiben, ziehen Sie die Initialisierung Ihrer VCD-Datei in Betracht, wenn Ihre App gestartet oder aktiviert wird, oder verwenden Sie eine Einstellung, die angibt, ob die VCD aktuell installiert ist.

In der Datei „app.xaml.cs“:

1. Fügen Sie Folgendes mithilfe einer Direktive hinzu:

    ```csharp
    using Windows.Storage;
    ```

1. Markieren Sie die Methode „OnLaunched“ mit dem asynchronen Modifizierer.  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. Rufen Sie [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) im Handler [**OnLaunched**](/uwp/api/Windows.UI.Xaml.Application) auf, um die Sprachbefehle zu registrieren, die das System erkennen soll.

  Im Adventure Works-Beispiel definieren wir zunächst ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekt.

  Anschließend rufen wir [**GetFileAsync**](/uwp/api/Windows.Storage.StorageFolder) auf, um es mit unserer Datei „AdventureWorksCommands.xml“ zu initialisieren.

  Dieses [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekt wird anschließend an [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) übergeben.

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>Behandeln der Aktivierung und Ausführen von Sprachbefehlen

Geben Sie an, wie Ihre App auf nachfolgende Sprachbefehlaktivierungen reagiert (nachdem sie mindestens einmal gestartet wurde und die Sätze mit den Sprachbefehlen installiert wurden).

1. Bestätigen Sie, dass die App über einen Sprachbefehl aktiviert wurde.

    Überschreiben Sie das [**Application.OnActivated**](/uwp/api/Windows.UI.Xaml.Application)-Ereignis, und überprüfen Sie, ob [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs).[**Kind**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) ist.

2. Bestimmen Sie den Namen des Befehls und die Sprachausgabe.

    Rufen Sie einen Verweis auf ein [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)-Objekt aus [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) ab, und fragen Sie die [**Result**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)-Eigenschaft nach einem [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)-Objekt ab.

    Um zu ermitteln, was der Benutzer gesagt hat, überprüfen Sie den Wert von [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) oder die semantischen Eigenschaften des erkannten Begriffs im Wörterbuch [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation).

3. Führen Sie in Ihrer App die entsprechende Aktion aus; navigieren Sie beispielsweise zur gewünschten Seite.

In diesem Beispiel beziehen wir uns wieder auf die VCD-Datei aus Schritt 3: Bearbeiten der VCD-Datei.

Sobald das Spracherkennungsergebnis für den Sprachbefehl vorliegt, können wir den Befehlsnamen aus dem ersten Wert im [**RulePath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)-Array ableiten. Da in der VCD-Datei mehrere mögliche Sprachbefehle definiert wurden, müssen wir den Wert mit den Befehlsnamen in der VCD-Datei vergleichen und die entsprechende Aktion ausführen.

Die gängigste Aktion in einer Anwendung ist das Navigieren zu einer Seite mit Inhalt, der für den Kontext des Sprachbefehls relevant ist. In diesem Beispiel navigieren wir zur Seite **TripPage** und übergeben den Wert des Sprachbefehls, wie der Befehl eingegeben wurde, und den erkannten Ziel-Begriff (falls zutreffend). Alternativ kann die App beim Navigieren zu der Seite einen Navigationsparameter an [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) senden.

Sie können herausfinden, ob der Sprachbefehl, mit dem Ihre App gestartet wurde, tatsächlich gesprochen wurde oder ob er als Text eingegeben wurde. Dazu verwenden Sie das [**SpeechRecognitionSemanticInterpretation.Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)-Wörterbuch und den **commandMode**-Schlüssel. Der Wert des Schlüssels lautet „voice“ oder „text“. Ist der Wert des Schlüssels „voice“, erwägen Sie die Verwendung von Sprachsynthese ([**Windows.Media.SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) in der App, um für den Benutzer gesprochenes Feedback bereitzustellen.

Ermitteln Sie mithilfe von [**SpeechRecognitionSemanticInterpretation.Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) die gesprochenen Inhalte in den Einschränkungen **PhraseList** oder **PhraseTopic** eines **ListenFor**-Elements. Der Wörterbuchschlüssel ist der Wert des **Label**-Attributs des **PhraseList**-Elements oder des **PhraseTopic**-Elements. Hier zeigen wir, wie auf den Wert des **{destination}**-Begriffs zugegriffen wird.

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-Apps](cortana-interactions.md)
- [Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen](cortana-launch-a-background-app-with-voice-commands.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)