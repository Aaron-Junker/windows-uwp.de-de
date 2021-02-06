---
title: Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen | Cortana UWP-Entwurf und-Entwicklung
description: Erweitern Sie Cortana mit Features von Ihrer APP (als Hintergrundaufgabe) mithilfe von Sprachbefehlen.
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: Cortana
ms.openlocfilehash: f1ed51107f41318cecf2d8fea73484713b4b837c
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606065"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>Aktivieren einer Hintergrund-app in Cortana mithilfe von Sprachbefehlen  

>[!WARNING]
> Diese Funktion wird ab dem Windows 10-Update vom Mai 2020 (Version 2004, Codename "20h1") nicht mehr unterstützt.
>
> Informationen dazu, wie Cortana moderne Produktivitäts Umgebungen transformiert, finden Sie [unter Cortana in Microsoft 365](/microsoft-365/admin/misc/cortana-integration) .

Zusätzlich zur Verwendung von Sprachbefehlen in **Cortana** für den Zugriff auf System Features können Sie **Cortana** auch mit Features und Funktionen von Ihrer APP (als Hintergrundaufgabe) erweitern, indem Sie Sprachbefehle verwenden, die eine Aktion oder einen auszuführenden Befehl angeben. Wenn eine App einen Sprachbefehl im Hintergrund verarbeitet, steht sie nicht im Fokus. Stattdessen werden das gesamte Feedback und alle Ergebnisse über den **Cortana**-Canvas und die **Cortana**-Stimme zurückgegeben.  

> [!NOTE]
> **Wichtige APIs**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Apps können im Vordergrund aktiviert werden (die APP hat den Fokus) oder im Hintergrund aktiviert (**Cortana** behält den Fokus), abhängig von der Komplexität der Interaktion. Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern (z. b. das Senden einer Nachricht an einen bestimmten Kontakt), werden am besten in einer Vordergrund-App behandelt, während grundlegende Befehle (z. b. das Auflisten der bevorstehenden Fahrten) in **Cortana** über eine Hintergrund-App verarbeitet werden können.  

Wenn Sie eine App im Vordergrund mit Sprachbefehlen aktivieren möchten, informieren Sie sich unter [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](cortana-launch-a-foreground-app-with-voice-commands.md).  

> [!NOTE]
> Ein Sprachbefehl ist eine einzelne Äußerung mit einer bestimmten Absicht, die in einer sprach Befehls Definitionsdatei (VCD) definiert ist, die an eine installierte App über **Cortana** gerichtet ist.
>
> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.
>
> Sprach Befehlsdefinitionen können von der Komplexität abweichen. Sie können alles von einer einzelnen, eingeschränkten Äußerung zu einer Sammlung flexibler, natürlicher Spracheingaben unterstützen, die alle dieselbe Absicht bezeichnen.

Wir verwenden eine in die **Cortana**-UI integrierte Reiseplanungs- und Verwaltungs-App mit dem Namen **Adventure Works**, um viele der hier vorgestellten Konzepte und Features zu veranschaulichen. Weitere Informationen finden Sie im [Cortana Voice-Befehlsbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899).

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Screenshot der Cortana-Start-Vordergrund-App":::

Zum Anzeigen einer **Adventure Works**-Reise ohne **Cortana** startet ein Benutzer die App und navigiert auf die Seite **Bevorstehende Reisen**.  

Durch die Verwendung von Sprachbefehlen über **Cortana** , um die APP im Hintergrund zu starten, kann der Benutzer stattdessen einfach Folgendes aufrufen: `Adventure Works, when is my trip to Las Vegas?` . Ihre App verarbeitet den Befehl, und **Cortana** zeigt die Ergebnisse zusammen mit Ihrem App-Symbol und weiteren App-Informationen an, falls diese bereitgestellt wurden.  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Screenshot von Cortana mit einer einfachen Abfrage und einem Ergebnis Bildschirm mithilfe der AdventureWorks-App im Hintergrund":::

Die folgenden grundlegenden Schritte fügen sprach Befehlsfunktionen hinzu und erweitern **Cortana** mit Hintergrund Funktionalität von ihrer App mithilfe von Sprache oder Tastatureingaben.

1. Erstellen Sie einen App-Dienst (siehe [**Windows.ApplicationModel.AppService**](/uwp/api/Windows.ApplicationModel.AppService)), der von **Cortana** im Hintergrund aufgerufen wird.  
2. Erstellen einer VCD-Datei Die Datei "VCD" ist ein XML-Dokument, in dem alle gesprochenen Befehle definiert werden, die der Benutzer zum Initiieren von Aktionen oder Aufrufen von Befehlen beim Aktivieren Ihrer APP anweist. Informationen finden Sie unter [**VCD elements and attributes v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2).  
3. Registrieren Sie die Befehlssätze in der VCD-Datei, wenn die App gestartet wird.  
4. Behandeln Sie die Hintergrund Aktivierung des App Service und die Ausführung des sprach Befehls.  
5. Zeigen Sie das entsprechende Feedback für den Sprachbefehl in **Cortana** an, und führen Sie die Sprachausgabe durch.  

> [!TIP]
> **Voraussetzungen**
>
> Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.
>
> - [Erste App erstellen](/windows/uwp/get-started/your-first-app)
> - Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Richtlinien zur Benutzer Leistung**
>
> Unter [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)  finden Sie Informationen dazu, wie Sie Ihre APP mit **Cortana** und [sprach Interaktionen](speech-interactions.md) integrieren, um hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden sprachfähigen APP zu erhalten.

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>Erstellen einer neuen Projekt Mappe mit einem primären Projekt in Visual Studio  

1. Starten Sie Microsoft Visual Studio 2015.  
    Der Visual Studio 2015-Startbildschirm wird angezeigt.  

2. Wählen Sie im Menü **Datei** die Option **Neu** > **Projekt**.  
    Das Dialogfeld **Neues Projekt** wird geöffnet. Im linken Bereich des Dialogfelds können Sie die Art der anzuzeigenden Vorlagen auswählen.  

3. Erweitern Sie im linken Bereich **installierte > Vorlagen > Visual C \# > Windows**, und wählen Sie dann die Gruppe **universelle** Vorlage aus. Im mittleren Bereich des Dialog Felds wird eine Liste der Projektvorlagen für universelle Windows-Plattform-Apps (UWP) angezeigt.  
4. Wählen Sie im mittleren Bereich die Projektvorlage **Leere App (universelle Windows-App)** aus.  
    Die Vorlage **leere App** erstellt eine minimale UWP-APP, die kompiliert und ausgeführt wird. Die Vorlage " **leere App** " enthält keine Steuerelemente oder Daten für die Benutzeroberfläche. Sie können der APP Steuerelemente hinzufügen, indem Sie diese Seite als Leitfaden verwenden.  

5. Geben Sie in das Textfeld **Name** Ihren Projektnamen ein. Beispiel: Verwenden Sie `AdventureWorks` .  
6. Klicken Sie auf die Schaltfläche **OK** , um das Projekt zu erstellen.  
    Microsoft Visual Studio erstellt Ihr Projekt und zeigt es im **Projektmappen-Explorer** an.  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>Fügen Sie dem primären Projekt Bild Ressourcen hinzu, und geben Sie diese im App-Manifest an.  

UWP-apps sollten automatisch die am besten geeigneten Images auswählen. Die Auswahl basiert auf bestimmten Einstellungen und Gerätefunktionen (hoher Kontrast, effektive Pixel, Gebiets Schema usw.). Sie müssen die Abbilder bereitstellen und sicherstellen, dass Sie die entsprechende Benennungs Konvention und Ordner Organisation innerhalb Ihres App-Projekts für die verschiedenen Ressourcen Versionen verwenden.  
Wenn Sie die empfohlenen Ressourcen Versionen nicht bereitstellen, kann die Benutzer Leistung auf folgende Weise beeinträchtigt werden.

- Barrierefreiheit
- Lokalisierung  
- Bildqualität  
Die Ressourcen Versionen werden verwendet, um die folgenden Änderungen an der Benutzer Darstellung anzupassen.  
- Benutzereinstellungen  
- G  
- Gerätetyp  
- Standort  

Weitere Informationen zu Bild Ressourcen für hohe Kontraste und Skalierungsfaktoren finden Sie auf der Seite Richtlinien für Kachel-und Symbol Ressourcen unter [MSDN.Microsoft.com/Windows/UWP/Controls-and-Patterns/tiles-and-Notifications-App-Assets](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast).  

Sie müssen Ressourcen mithilfe von Qualifizierern benennen. Ressourcenqualifizierer sind Ordner- und Dateinamenmodifikatoren, die den Kontext angeben, in dem eine bestimmte Version einer Ressource verwendet werden soll.  

Standardmäßig wird die folgende Benennungskonvention verwendet: `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`.  
Beispiel: `images/logo.scale-100_contrast-white.png` . Dies kann auf Code verweisen, indem nur der Stamm Ordner und der Dateiname verwendet werden: `images/logo.png` .  
Weitere Informationen finden Sie auf der Seite Benennen von Ressourcen mithilfe von Qualifizierern unter [MSDN.Microsoft.com/Library/Windows/apps/XAML/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10)).  

Microsoft empfiehlt, dass Sie die Standardsprache für Zeichen folgen Ressourcen Dateien (z. b. `en-US\resources.resw` ) und den Standard Skalierungsfaktor für Bilder (z `logo.scale-100.png` . b.) markieren, auch wenn Sie derzeit nicht planen, lokalisierte oder mehrere Auflösungs Ressourcen bereitzustellen. Microsoft empfiehlt jedoch, Ressourcen für die Skalierungsfaktoren 100, 200 und 400 bereitzustellen.  

>[!IMPORTANT]
> Das App-Symbol, das im Titelbereich der **Cortana** -Canvas verwendet wird, ist das in der Datei angegebene Square44x44Logo-Symbol `Package.appxmanifest` .  
> Sie können auch ein Symbol für jeden Eintrag im Inhalts Bereich der **Cortana** -Canvas angeben. Gültige Bildgrößen für die Ergebnissymbole sind:
>
> - 68 (B) x 68 (H)
> - 68 (B) x 92 (H)  
> - 280 (B) x 140 (H)  

Die Inhalts Kachel wird erst überprüft, wenn ein [**voicecommandresponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) -Objekt an die [**voicecommandserviceconnetction**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) -Klasse übergeben wird. Wenn Sie ein [**voicecommandresponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) -Objekt an **Cortana** übergeben, das eine Inhalts Kachel mit einem Bild enthält, das nicht diesen Größenverhältnissen entspricht, kann eine Ausnahme auftreten.  

Beispiel: die **Adventure Works** -app ( `VoiceCommandService\\AdventureWorksVoiceCommandService.cs` ) gibt mithilfe der TitleWith68x68IconAndText-Kachel Vorlage ein einfaches, graues Quadrat ( `GreyTile.png` ) für [](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) die [**voicecommandcontenttile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) -Klasse an. Die Logo Varianten befinden sich in `VoiceCommandService\\Images` und werden mithilfe der [**getfilefromapplicationuriasync**](/uwp/api/Windows.Storage.StorageFile) -Methode abgerufen.

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>Erstellen eines App Service Projekts

1. Klicken Sie mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **Neu > Projekt**  
2. Wählen Sie unter **installierte > Vorlagen > Visual C \# > Windows > Universal** aus, und wählen Sie **Windows-Runtime Komponente** aus. Die **Windows-Runtime Komponente** ist die Komponente, die den App Service ([**Windows. applicationmodel. Appservice**](/uwp/api/Windows.ApplicationModel.AppService)) implementiert.  
3. Geben Sie einen Namen für das Projekt ein, und klicken Sie auf die Schaltfläche **OK** .  
    Beispiel: `VoiceCommandService`.  
4. Wählen Sie in **Projektmappen-Explorer** das `VoiceCommandService` Projekt aus, und benennen Sie die `Class1.cs` von Visual Studio generierte Datei um.
    Beispiel: **Adventure Works** verwendet `AdventureWorksVoiceCommandService.cs` .  
5. Klicken Sie auf die Schaltfläche **Ja** . Wenn Sie gefragt werden, ob Sie alle Vorkommen von umbenennen möchten `Class1.cs` .  
6. In der Datei `AdventureWorksVoiceCommandService.cs`:
    1. Fügen Sie Folgendes mithilfe einer Direktive hinzu.  
        `using Windows.ApplicationModel.Background;`  
    2. Wenn Sie ein neues Projekt erstellen, wird der Projektname als standardmäßiger Stamm-Namespace in allen Dateien verwendet. Benennen Sie den Namespace um, um den App-Dienstcode unter dem primären Projekt zu schachteln.
        Beispiel: `namespace AdventureWorks.VoiceCommands`.  
    3. Klicken Sie mit der rechten Maustaste auf den Namen des App Service-Projekts in Projektmappen-Explorer und wählen Sie **Eigenschaften** aus.  
    4. Aktualisieren Sie auf der Registerkarte **Bibliothek** das Feld **Standard Namespace** mit dem gleichen Wert.  
        Beispiel: `AdventureWorks.VoiceCommands`).  
    5. Erstellen Sie eine neue Klasse zum Implementieren der Schnittstelle [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask). Diese Klasse erfordert eine [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)-Methode, die der Einstiegspunkt ist, wenn Cortana den Sprachbefehl erkennt.  

    Beispiel: eine grundlegende Hintergrundaufgaben Klasse aus der **Adventure Works** -app.  

    >[!NOTE]
    > Die Hintergrundaufgaben Klasse selbst und alle Klassen im Hintergrundaufgaben Projekt müssen versiegelte öffentliche Klassen sein.  

    ```csharp
    namespace AdventureWorks.VoiceCommands
    {
        ...
        
        /// <summary>
        /// The VoiceCommandService implements the entry point for all voice commands.
        /// The individual commands supported are described in the VCD xml file. 
        /// The service entry point is defined in the appxmanifest.
        /// </summary>
        public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
        {
            ...

            /// <summary>
            /// The background task entrypoint. 
            /// 
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
            /// </summary>
            /// <param name="taskInstance">Connection to the hosting background service process.</param>
            public void Run(IBackgroundTaskInstance taskInstance)
            {
              //
              // TODO: Insert code 
              //
              //
        }
      }
    }
    ```  

7. Deklarieren Sie Ihre Hintergrundaufgabe im App-Manifest als **AppService**.  
    1. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf die `Package.appxmanifest` Datei, und wählen Sie **Code anzeigen** aus.  
    2. Suchen Sie das- [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) Element.  
    3. Fügen Sie [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) dem-Element ein-Element hinzu [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) .  
    4. Fügen Sie [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) dem-Element ein-Element hinzu [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) .  
    5. Fügen Sie `Category` dem-Element ein-Attribut hinzu, `uap:Extension` und legen Sie den Wert des- `Category` Attributs auf fest `windows.appService` .  
    6. Fügen Sie `EntryPoint` dem-Element ein-Attribut hinzu, `uap: Extension` und legen Sie den Wert des- `EntryPoint` Attributs auf den Namen der Klasse fest, die implementiert [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) .  
        Beispiel: `AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`.  
    7. Fügen Sie [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) dem-Element ein-Element hinzu `uap:Extension` .  
    8. Fügen Sie `Name` dem [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) -Element ein-Attribut hinzu, und legen Sie den Wert des- `Name` Attributs auf einen Namen für den App Service fest, in diesem Fall `AdventureWorksVoiceCommandService` .  
    9. Fügen Sie dem-Element ein zweites- [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) Element hinzu [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) .  
    10. Fügen Sie `Category` diesem Element ein-Attribut hinzu, [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) und legen Sie den Wert des- `Category` Attributs auf fest `windows.personalAssistantLaunch` .  

    Beispiel: ein Manifest aus der Adventure Works-app.

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. Fügen Sie dieses App-Dienst-Projekt als Verweis im primären Projekt hinzu.  
    1. Klicken Sie mit der rechten Maustaste auf die **Verweise**.  
    2. Wählen Sie **Verweis hinzufügen...** aus.  
    3. Erweitern Sie im Dialogfeld **Verweis-Manager****Projekte**, und wählen Sie das App-Dienst-Projekt aus.  
    4. Klicken Sie auf die Schaltfläche **OK** .  

## <a name="create-a-vcd-file"></a>Erstellen einer VCD-Datei

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Namen Ihres primären Projekts, und wählen Sie **> neues Element hinzufügen** aus. Fügen Sie eine **XML-Datei** hinzu.  
2. Geben Sie einen Namen für die [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) -Datei ein.  
    Beispiel: `AdventureWorksCommands.xml`.
3. Klicken Sie auf die Schaltfläche **Hinzufügen** .  
4. Wählen Sie im **Projektmappen-Explorer** die [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Datei aus.  
5. Legen Sie im Fenster **Eigenschaften** die Option **Buildvorgang** auf **Inhalt** fest, und legen Sie dann **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.  

## <a name="edit-the-vcd-file"></a>Bearbeiten der VCD-Datei  

1. Fügen Sie ein- `VoiceCommands` Element mit einem `xmlns` Attribut hinzu, das auf verweist `https://schemas.microsoft.com/voicecommands/1.2` .  
2. Erstellen Sie für jede Sprache, die von ihrer App unterstützt wird, ein- [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) Element, das die von Ihrer APP unterstützten Sprachbefehle enthält.  
    Sie können mehrere Elemente deklarieren [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) , von denen jedes über ein anderes Attribut verfügt, [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) sodass Ihre APP in verschiedenen Märkten verwendet werden kann. Beispielsweise kann eine APP für den USA eine [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) für Englisch und eine [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) für Spanisch haben.  

    >[!IMPORTANT]
    > Um eine APP zu aktivieren und eine Aktion mithilfe eines Voice-Befehls zu initiieren, muss die APP eine VCD-Datei registrieren, die ein- [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) Element mit einer Sprache enthält, die der Sprache des Benutzers entspricht. Die Sprache Sprache befindet sich unter **Einstellungen > System > Sprache Sprache > Sprache Sprache**.  

3. Fügen Sie ein- `Command` Element für jeden Befehl hinzu, den Sie unterstützen möchten.  
    Jede, die `Command` in einer [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) -Datei deklariert ist, muss folgende Informationen enthalten:  
    - Ein- `Name` Attribut, das Ihre Anwendung verwendet, um den Voice-Befehl zur Laufzeit zu identifizieren.  
    - Ein `Example` Element, das einen Ausdruck enthält, der beschreibt, wie ein Benutzer den Befehl aufruft. **Cortana** zeigt das Beispiel an, wenn der Benutzer anzeigt `What can I say?` , `Help` oder tippt.  
    - Ein- `ListenFor` Element mit den Wörtern oder Ausdrücken, die von der App als Befehl erkannt werden. Jedes- `ListenFor` Element kann Verweise auf ein oder mehrere- `PhraseList` Elemente enthalten, die bestimmte, für den Befehl relevante Wörter enthalten.  

       >[!NOTE]
       > `ListenFor` Elemente dürfen nicht Programm gesteuert geändert werden. Elemente, die `PhraseList` Elementen zugeordnet sind, `ListenFor` können jedoch Programm gesteuert geändert werden. Anwendungen sollten den Inhalt des- `PhraseList` Elements zur Laufzeit basierend auf dem Dataset ändern, das generiert wird, wenn der Benutzer die APP verwendet.
       >
       > Weitere Informationen finden Sie unter [dynamisches Ändern von Cortana VCD-Ausdrucks Listen](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md).  

    - Ein- `Feedback` Element, das den Text enthält, den **Cortana** anzeigen und beim Starten der Anwendung sprechen soll.  

Ein- `Navigate` Element gibt an, dass der Voice-Befehl die APP im Vordergrund aktiviert. In diesem Beispiel ist der Befehl ```showTripToDestination``` eine Vordergrundaufgabe.  

Ein- `VoiceCommandService` Element gibt an, dass der Voice-Befehl die APP im Hintergrund aktiviert. Der Wert des- `Target` Attributs dieses Elements sollte mit dem Wert des- `Name` Attributs des- [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) Elements in der Datei "Package. appxmanifest" verglichen werden. In diesem Beispiel sind die `whenIsTripToDestination` `cancelTripToDestination` Befehle und Hintergrundaufgaben, die den Namen des App Service als angeben `AdventureWorksVoiceCommandService` .  

Weitere Informationen finden Sie in der [**VCD-Elemente und -Attribute v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)-Referenz.  

Beispiel: ein Teil der [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) -Datei, der die `en-us` Sprachbefehle für die **Adventure Works** -App definiert.  

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

>[!NOTE]
> Sprachbefehlsdaten werden nicht übergreifend über App-Installationen beibehalten. Um sicherzustellen, dass die Sprachbefehlsdaten für Ihre App intakt bleiben, ziehen Sie die Initialisierung Ihrer VCD-Datei in Betracht, wenn Ihre App gestartet oder aktiviert wird, oder verwenden Sie eine Einstellung, die angibt, ob die VCD aktuell installiert ist.  

In der Datei `app.xaml.cs`:  

1. Fügen Sie Folgendes mithilfe einer Direktive hinzu:

   ```csharp
   using Windows.Storage;
   ```

2. Markieren Sie die `OnLaunched` Methode mit dem Async-Modifizierer.  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. Wenden Sie die- [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) Methode im- [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) Handler an, um die Sprachbefehle zu registrieren, die erkannt werden sollen.  
    Beispiel: die Adventure Works-App definiert ein- [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) Objekt.  
    Beispiel: Ruft die- [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) Methode auf, um das [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) Objekt mit der Datei zu initialisieren `AdventureWorksCommands.xml` .  
    Das- [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) Objekt wird dann an die-Methode weitergegeben [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) .  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>Handle-Aktivierung  

Geben Sie an, wie Ihre APP auf nachfolgende sprach Befehls Aktivierungen antwortet.

>[!NOTE]
> Sie müssen die APP mindestens einmal starten, nachdem die sprach Befehls Sätze installiert wurden.  

1. Bestätigen Sie, dass die App über einen Sprachbefehl aktiviert wurde.  

    Überschreiben [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) Sie das Ereignis, und überprüfen Sie, ob [**iactivatedeventargs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs).[**Art**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) : [**Voicecommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)  

2. Bestimmen Sie den Namen des Befehls und die Sprachausgabe.  

    Sie erhalten einen Verweis auf ein [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) -Objekt von [**iactivatedeventargs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) und Fragen die- [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) Eigenschaft für ein- [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) Objekt ab.  

    Überprüfen Sie den Wert von [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) oder die semantischen Eigenschaften des erkannten Ausdrucks im Wörterbuch, um zu bestimmen, was der Benutzer gesagt hat [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) .  

3. Führen Sie in Ihrer App die entsprechende Aktion aus; navigieren Sie beispielsweise zur gewünschten Seite.  

    >[!NOTE]
    > Wenn Sie auf Ihren VCD verweisen müssen, besuchen Sie den Abschnitt [Bearbeiten der VCD-Datei](#edit-the-vcd-file) .

    Nachdem Sie das sprach Erkennungs Ergebnis für den Voice-Befehl erhalten haben, erhalten Sie den Befehlsnamen aus dem ersten Wert im [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) Array. Da die VCD-Datei mehr als einen möglichen Sprachbefehl definiert, müssen Sie überprüfen, ob der Wert mit den Befehlsnamen in der VCD übereinstimmt, und die entsprechende Aktion durchführen.  

    Die häufigste Aktion für eine Anwendung besteht darin, zu einer Seite mit Inhalt zu navigieren, der für den Kontext des sprach Befehls relevant ist.  
    Beispiel: Öffnen Sie **die Seite "** selektiert", und übergeben Sie den Wert des sprach Befehls, die Art der Eingabe des Befehls und den erkannten Ziel Ausdruck (falls zutreffend). Alternativ kann die APP einen Navigations Parameter an das [**sprach Erkennungs Ergebnis**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) senden, wenn Sie **zur Seite "** selektiert" navigieren.  

    Sie können herausfinden, ob der Sprachbefehl, mit dem Ihre APP gestartet wurde, tatsächlich gesprochen wurde oder ob er als Text eingegeben wurde, und zwar [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) mithilfe des **commandmode** -Schlüssels aus dem Wörterbuch. Der Wert dieses Schlüssels ist entweder `voice` oder `text` . Wenn der Wert des Schlüssels ist `voice` , sollten Sie die Sprachsynthese ([**Windows. Media. Speech Synthese**](/uwp/api/Windows.Media.SpeechSynthesis)) in Ihrer APP verwenden, um dem Benutzer das gesprochene Feedback zu bieten.  

    Verwenden Sie die Eigenschaft " [**sprecherkentionsemanticinterpretation. Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) ", um den in der- `PhraseList` oder- `PhraseTopic` Einschränkung eines-Elements gesprochenen Inhalt zu ermitteln `ListenFor` . Der Wörterbuch Schlüssel ist der Wert des- `Label` Attributs des- `PhraseList` Elements oder des- `PhraseTopic` Elements.
    Beispiel: der folgende Code für den Zugriff auf den Wert des **{Destination}** -Ausdrucks.  

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
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
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
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>Behandeln Sie den Voice-Befehl in der APP Service  

Verarbeiten Sie den Sprachbefehl im App-Dienst.  

1. Fügen Sie die folgenden using-Direktiven der sprach Befehls Dienst-Datei hinzu.  
    Beispiel: `AdventureWorksVoiceCommandService.cs`.  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. Richten Sie eine Dienstverzögerung ein, damit Ihre App beim Verarbeiten des Sprachbefehls nicht beendet wird.  
3. Stellen Sie sicher, dass Ihre Hintergrundaufgabe als App-Dienst ausgeführt wird, der per Sprachbefehl aktiviert wird.  
    1. Wandeln Sie [**IBackgroundTaskInstance.TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) in [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails) um.  
    2. Überprüfen Sie, ob [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) der Name des App-Dienstanbieter in der `Package.appxmanifest` Datei ist.  
4. Verwenden Sie [**IBackgroundTaskInstance.TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance), um ein [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)-Element für **Cortana** zum Abrufen des Sprachbefehls zu erstellen.
5. Registrieren Sie einen Ereignishandler für [**voicecommandserviceconnetction**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection).  [**Voicecommandabgeschlossen**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) zum Empfangen einer Benachrichtigung, wenn der APP Service aufgrund eines Benutzer Abbruchs geschlossen wird.  
6. Registrieren Sie einen Ereignishandler für das Element [**IBackgroundTaskInstance.Canceled**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance), um eine Benachrichtigung empfangen zu können, wenn der App-Dienst aufgrund eines unerwarteten Fehlers geschlossen wird.  
7. Bestimmen Sie den Namen des Befehls und die Sprachausgabe.  
    1. Verwenden Sie die Eigenschaft [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand).[**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand), um den Namen des Sprachbefehls zu ermitteln.  
    2. Überprüfen Sie den Wert von [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) oder die semantischen Eigenschaften des erkannten Ausdrucks im Wörterbuch, um zu bestimmen, was der Benutzer gesagt hat [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) .  
8. Führen Sie in Ihrem App-Dienst die entsprechende Aktion aus.  
9. Zeigen Sie mithilfe von **Cortana** das Feedback an, und melden Sie es mit dem Sprachbefehl an  
    1. Bestimmen Sie die Zeichen folgen, die **Cortana** anzeigen und als Antwort auf den Voice-Befehl für den Benutzer sprechen soll, und erstellen Sie ein- [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) Objekt. Eine Anleitung, wie Sie die von **Cortana** angezeigten und gesprochenen Feedback-Zeichenfolgen auswählen, finden Sie unter [Cortana-Entwurfsrichtlinien](cortana-design-guidelines.md).  
    2. Verwenden Sie die [**voicecommandserviceconnetction**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) -Instanz, um den Fortschritt oder den Abschluss an **Cortana** zu melden, indem Sie [**reportprogressasync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) oder [**reportsuccess Async**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) mit dem- `VoiceCommandServiceConnection` Objekt aufrufen.  

    >[!NOTE]
    > Wenn Sie auf Ihren VCD verweisen müssen, besuchen Sie den Abschnitt [Bearbeiten der VCD-Datei](#edit-the-vcd-file) .  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
                        case "whenIsTripToDestination":
                            {
                                var destination = 
                                voiceCommand.Properties["destination"][0];
                                SendCompletionMessageForDestination(destination);
                                break;
                            }
                            
                            // As a last resort, launch the app in the foreground.
                        default:
                            LaunchAppInForeground();
                            break;
                    }
                }
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
            var userMessage = new VoiceCommandUserMessage();
            userMessage.SpokenMessage = "Launching Adventure Works";
            
            var response = VoiceCommandResponse.CreateResponse(userMessage);
            
            // When launching the app in the foreground, pass an app 
            // specific launch parameter to indicate what page to show.
            response.AppLaunchArgument = "showAllTrips=true";
            
            await voiceServiceConnection.RequestAppLaunchAsync(response);
        }
    }
    ```  

Nach der Aktivierung verfügt der APP Service über 0,5 Sekunden, um [**reporterfolgreiches Async**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)aufzurufen. **Cortana** zeigt eine Feedback Zeichenfolge an und zeigt diese an.  

>[!NOTE]
> Sie können in der Datei "VCD" eine **Feedback** Zeichenfolge deklarieren. Die Zeichenfolge wirkt sich nicht auf den auf dem Cortana-Zeichenbereich angezeigten Benutzeroberflächen Text aus, sondern wirkt sich nur auf den Text aus, der von **Cortana**  

Wenn die APP mehr als 0,5 Sekunden benötigt, um den-Befehl durchführen zu können, fügt **Cortana** einen Hand seitigen Bildschirm ein, wie hier gezeigt. **Cortana** zeigt den Übergabebildschirm an, bis die Anwendung **ReportSuccessAsync** aufruft, oder für maximal 5 Sekunden. Wenn der APP-Dienst **reporterfolgreiches Async** oder eine der Methoden, die [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) **Cortana** Informationen bereitstellen, nicht aufruft, empfängt der Benutzer eine Fehlermeldung, und der APP Service wird abgebrochen.  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Screenshot von Cortana und einer grundlegenden Abfrage mit Fortschritts-und Ergebnis Bildschirmen, die die AdventureWorks-App im Hintergrund verwenden":::

## <a name="related-articles"></a>Verwandte Artikel

- [Cortana-Interaktionen in Windows-apps](cortana-interactions.md)
- [Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)
- [VCD elements and attributes v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana-Entwurfs Richtlinien](cortana-design-guidelines.md)
- [Cortana-Sprachbefehlbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=619899)
