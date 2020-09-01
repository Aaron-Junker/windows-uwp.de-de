---
ms.assetid: ''
title: Unterstützen von Surface Dial (und anderen Radgeräten) in Ihrer Windows-App
description: Ein Schritt-für-Schritt-Tutorial zum Hinzufügen von Unterstützung für die Oberfläche (und andere radgeräte) zu Ihrer Windows-app.
keywords: wählen, radiales, Tutorial
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8edd7a9345f93d3cf0abe76f68c321a977ee2e50
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173374"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>Tutorial: unterstützen der Oberflächen Wahl (und anderer radgeräte) in der Windows-App

![Abbildung: Surface Dial mit Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
Oberfläche für die Oberfläche *mit Surface Studio und Surface Pen* (für den Kauf am [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)verfügbar).

In diesem Tutorial wird erläutert, wie Sie die Benutzerinteraktionen anpassen, die von radgeräten unterstützt werden, z. b. die Oberfläche. Wir verwenden Ausschnitte aus einer Beispiel-APP, die Sie von GitHub herunterladen können (siehe [Beispielcode](#sample-code)), um die verschiedenen Features und zugeordneten [**radialcontroller**](/uwp/api/windows.ui.input.radialcontroller) -APIs zu veranschaulichen, die in den einzelnen Schritten erläutert werden.

Wir konzentrieren uns auf Folgendes:
* Angeben der integrierten Tools, die im [**radialcontroller**](/uwp/api/windows.ui.input.radialcontroller) -Menü angezeigt werden
* Hinzufügen eines benutzerdefinierten Tools zum Menü
* Steuern von haptischem Feedback
* Anpassen von Click-Interaktionen
* Anpassen von Rotations Interaktionen

Weitere Informationen zur Implementierung dieser und anderer Funktionen finden Sie unter [Oberflächen wählinteraktionen in Windows-apps](windows-wheel-interactions.md).

## <a name="introduction"></a>Einführung

Bei der Oberflächen Auswahl handelt es sich um ein sekundäres Eingabegerät, mit dem Benutzer produktiver arbeiten können, wenn Sie mit einem primären Eingabegerät wie pen, Touchscreen oder Maus verwendet werden. Als sekundäres Eingabegerät wird die Wahl üblicherweise mit der nicht dominanten Hand verwendet, um den Zugriff auf Systembefehle und andere, kontextabhängige Tools und Funktionen bereitzustellen. 

Der wählwert unterstützt drei grundlegende Gesten: 
- Halten Sie die Taste gedrückt, um das integrierte Menü der Befehle anzuzeigen.
- Drehen Sie, um ein Menü Element hervorzuheben (wenn das Menü aktiv ist), oder um die aktuelle Aktion in der APP zu ändern (wenn das Menü nicht aktiv ist).
- Klicken Sie hier, um das markierte Menü Element (wenn das Menü aktiv ist) auszuwählen oder einen Befehl in der APP aufzurufen (wenn das Menü nicht aktiv ist).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Computer (oder ein virtueller Computer), auf dem Windows 10 Creators Update oder höher ausgeführt wird
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Ein radgerät (zu diesem Zeitpunkt nur die [Oberflächen Wahl](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116) )
* Wenn Sie mit der Entwicklung von Windows-apps mit Visual Studio noch nicht vertraut sind, sehen Sie sich diese Themen an, bevor Sie mit diesem Tutorial beginnen:  
    * [Vorbereiten](../../get-started/get-set-up.md)
    * [Erstellen der App „Hello, world“ (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)

## <a name="set-up-your-devices"></a>Einrichten Ihrer Geräte

1. Stellen Sie sicher, dass Ihr Windows-Gerät eingeschaltet ist.
2. Wechseln Sie zu **Start**, wählen Sie **Einstellungen**  >  **Geräte**  >  **Bluetooth & andere Geräte**aus, und aktivieren Sie dann **Bluetooth** .
3. Entfernen Sie den unteren Rand der Oberfläche, um das Akku Depot zu öffnen, und stellen Sie sicher, dass in zwei AAA-Akkus vorhanden sind.
4. Wenn die Registerkarte Akku auf der Unterseite des wähles vorhanden ist, entfernen Sie Sie.
5. Halten Sie die kleine, eingelegte Schaltfläche neben den Akkus gedrückt, bis das Bluetooth-Licht blinkt.
6. Wechseln Sie zurück zu Ihrem Windows-Gerät, und wählen Sie **Bluetooth oder anderes Gerät hinzufügen**aus.
7. Wählen Sie im Dialogfeld **Gerät hinzufügen** die Option **Bluetooth**-  >  **Oberfläche**auswählen aus. Ihre Oberfläche sollte nun eine Verbindung herstellen und der Liste der Geräte unter **Maus, Tastatur, & Stift** auf der Seite **Bluetooth & andere Geräte** Einstellungen hinzugefügt werden.
8. Testen Sie die Auswahl, indem Sie Sie für einige Sekunden drücken und halten, um das integrierte Menü anzuzeigen.
9. Wenn das Menü nicht auf dem Bildschirm angezeigt wird (die Auswahl muss ebenfalls vibrieren), kehren Sie zu den Bluetooth-Einstellungen zurück, entfernen Sie das Gerät, und versuchen Sie erneut, das Gerät zu verbinden.

> [!NOTE]
> Radgeräte können über die **Radeinstellungen** konfiguriert werden:
> 1. Wählen Sie im **Startmenü** die Option **Einstellungen**aus.
> 2. Wählen Sie **Geräte**  >  **Rad**aus.    
> ![Bildschirm mit Radeinstellungen](images/radialcontroller/wheel-settings.png)

Jetzt sind Sie bereit, dieses Tutorial zu starten. 

## <a name="sample-code"></a>Beispielcode
In diesem Tutorial verwenden wir eine Beispiel-APP, um die beschriebenen Konzepte und Funktionen zu veranschaulichen.

Laden Sie dieses Visual Studio-Beispiel und den Quellcode von [GitHub](https://github.com/) unter [Windows-appsample-Get-Started-radialcontroller Sample](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController)herunter:

1. Wählen Sie die Schaltfläche grünes **Klonen oder herunterladen** aus.  
![Klonen des Repositorys](images/radialcontroller/wheel-clone.png)
2. Wenn Sie über ein GitHub-Konto verfügen, können Sie das Repository auf Ihrem lokalen Computer Klonen, indem Sie **in Visual Studio öffnen**auswählen. 
3. Wenn Sie nicht über ein GitHub-Konto verfügen oder nur eine lokale Kopie des Projekts wünschen, wählen Sie **ZIP herunterladen** aus (Sie müssen regelmäßig zurückkehren, um die neuesten Updates herunterzuladen).

> [!IMPORTANT]
> Der größte Teil des Codes im Beispiel ist auskommentiert. Beim Durchlaufen der einzelnen Schritte in diesem Thema werden Sie aufgefordert, die Auskommentierung verschiedener Code Abschnitte zu entfernen. Markieren Sie in Visual Studio einfach die Codezeilen, und drücken Sie STRG + K und dann STRG + U.

## <a name="components-that-support-wheel-functionality"></a>Komponenten, die Wheel-Funktionalität unterstützen

Diese Objekte stellen den größten Teil des radgeräts für Windows-apps bereit.

| Komponente | BESCHREIBUNG |
| --- | --- |
| [ **Radialcontroller** -Klasse](/uwp/api/Windows.UI.Input.RadialController) und Verwandte | Stellt ein radeingabegerät oder ein Zubehör, z. b. die Oberfläche |
| [**Iradialcontrollerconfigurationinterop**](/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop)  /  [ **Iradialcontrollerinterop**](/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>Diese Funktion wird hier nicht behandelt. Weitere Informationen finden Sie im Beispiel für ein [klassisches Windows-Desktop](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController). | Ermöglicht die Interoperabilität mit einer Windows-app. |

## <a name="step-1-run-the-sample"></a>Schritt 1: Ausführen des Beispiels

Nachdem Sie die radialcontroller-Beispiel-App heruntergeladen haben, vergewissern Sie sich, dass Sie ausgeführt wird:
1. Öffnen Sie das Beispiel Projekt in Visual Studio.
2. Legen Sie **Solution Platforms** die Dropdown Liste Projektmappenplattformen auf eine nicht-arm-Auswahl fest.
3. Drücken Sie F5, um zu kompilieren, bereitzustellen und auszuführen. 

> [!NOTE]
> Alternativ können Sie **Debug**  >  das Menü Element**Debuggen Debuggen starten** auswählen, oder wählen Sie die Schaltfläche **lokaler Computer** ausführen aus, die hier angezeigt wird: ![ Schaltfläche "Projekt](images/radialcontroller/wheel-vsrun.png)

Das App-Fenster wird geöffnet, und nachdem ein Begrüßungsbildschirm für einige Sekunden angezeigt wird, sehen Sie diesen ersten Bildschirm.

![Leere App](images/radialcontroller/wheel-app-step1-empty.png)

Wir verfügen jetzt über die grundlegende Windows-APP, die wir im weiteren Verlauf dieses Tutorials verwenden werden. In den folgenden Schritten fügen wir die **radialcontroller** -Funktionalität hinzu.

## <a name="step-2-basic-radialcontroller-functionality"></a>Schritt 2: grundlegende radialcontroller-Funktionalität

Wenn die app ausgeführt wird und Sie sich im Vordergrund befinden, halten Sie die Oberfläche gedrückt, um das Menü **radialcontroller** anzuzeigen.

Wir haben noch keine Anpassung für unsere App durchgeführt, sodass das Menü einen Standardsatz kontextbezogener Tools enthält. 

Diese Bilder zeigen zwei Variationen des Standardmenüs. (Es gibt viele andere, einschließlich grundlegender System Tools, wenn der Windows-Desktop aktiv ist und keine apps im Vordergrund stehen, zusätzliche Tools zum einbinden, wenn eine inktoolbar vorhanden ist, und Zuordnungs Tools, wenn Sie die Maps-APP verwenden.

| Radialcontroller-Menü (Standard)  | Radialcontroller-Menü (Standard bei Wiedergabe von Medien)  |
|---|---|
| ![Standardmäßiges radialcontroller-Menü](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Standardmäßiges radialcontroller-Menü mit Musik](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Nun beginnen wir mit einer grundlegenden Anpassung.

## <a name="step-3-add-controls-for-wheel-input"></a>Schritt 3: Hinzufügen von Steuerelementen für die Radeingabe

Fügen Sie zunächst die Benutzeroberfläche für die APP hinzu:

1. Öffnen Sie die Datei MainPage_Basic. XAML.
2. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 3: Add controls for wheel input --> ").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
An diesem Punkt sind nur die Schaltflächen " **Beispiel initialisieren** ", "Schieberegler" und "umschalten" aktiviert. Die anderen Schaltflächen werden in späteren Schritten zum Hinzufügen und Entfernen von **radialcontroller** -Menü Elementen verwendet, die den Zugriff auf den Schieberegler und den umschaltschalter ermöglichen.

![Einfache Beispiel-App-Benutzeroberfläche](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Schritt 4: Anpassen des grundlegenden radialcontroller-Menüs

Nun fügen wir den Code hinzu, der zum Aktivieren des **radialcontroller** -Zugriffs auf unsere Steuerelemente erforderlich ist.

1. Öffnen Sie die Datei MainPage_Basic. XAML. cs.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 4: einfache radialcontroller-Menü Anpassung").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen:
    - Die Typverweise " [Windows. UI. Input](/uwp/api/windows.ui.input) " und " [Windows. Storage. Streams](/uwp/api/windows.storage.streams) " werden in nachfolgenden Schritten für die Funktionalität verwendet:  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Diese globalen Objekte ([radialcontroller](/uwp/api/windows.ui.input.radialcontroller), [radialcontrollerconfiguration](/uwp/api/windows.ui.input.radialcontrollerconfiguration), [radialcontrollermenuitem](/uwp/api/windows.ui.input.radialcontrollermenuitem)) werden in der gesamten App verwendet.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Hier geben wir den **Click** -Handler für die Schaltfläche an, die die Steuerelemente aktiviert und das benutzerdefinierte Menü Element " **radialcontroller** " initialisiert.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - Als nächstes initialisieren wir unser [radialcontroller](/uwp/api/windows.ui.input.radialcontroller) -Objekt und richten Handler für die Ereignisse " [rotationchanged](/uwp/api/windows.ui.input.radialcontroller.RotationChanged) " und " [buttongeklickt](/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) " ein.

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - Hier initialisieren wir das benutzerdefinierte radialcontroller-Menü Element. Wir verwenden " [forateforcurrentview](/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) ", um einen Verweis auf das [radialcontroller](/uwp/api/windows.ui.input.radialcontroller) -Objekt zu erhalten. Wir legen die Rotations Sensitivität mithilfe der Eigenschaft " [rotationresolutionindegrees](/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) " auf "1" fest. Anschließend erstellen wir unser [radialcontrollermenuitem-Element](/uwp/api/windows.ui.input.radialcontrollermenuitem) mithilfe von " [anatefromfontglyph](/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph)". das Menü Element wird der **radialcontroller** -Menü Element Auflistung hinzugefügt. schließlich verwenden wir " [setdefaultmenuitems](/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) ", um die Standardmenü Elemente zu löschen und nur unser benutzerdefiniertes Tool zu belassen. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. Führen Sie die App nun erneut aus.
5. Wählen Sie die Schaltfläche **radialen Controller initialisieren** aus.  
6. Wenn die APP im Vordergrund angezeigt wird, halten Sie die Oberfläche gedrückt, um das Menü anzuzeigen. Beachten Sie, dass alle Standard Tools entfernt wurden (mit der **radialcontrollerconfiguration. setdefaultmenuitems** -Methode), sodass nur das benutzerdefinierte Tool verwendet wird. Hier ist das Menü mit dem benutzerdefinierten Tool. 

| Radialcontroller (Menü) (Benutzer definiert)  | 
|---|
| ![Benutzerdefiniertes radialcontroller-Menü](images/radialcontroller/wheel-app-step3-custom.png) |

7. Wählen Sie das benutzerdefinierte Tool aus, und testen Sie die Interaktionen, die jetzt über die Oberfläche unterstützt werden
    * Eine Aktion zum Drehen verschiebt den Schieberegler. 
    * Mit einem Klick wird der Schalter auf ein oder aus festgelegt.

OK, lassen Sie uns diese Schaltflächen anschließen.

## <a name="step-5-configure-menu-at-runtime"></a>Schritt 5: Konfigurieren des Menüs zur Laufzeit

In diesem Schritt verbinden wir die Menü Schaltflächen " **Element hinzufügen/entfernen** " und " **radialcontroller zurücksetzen** ", um zu zeigen, wie Sie das Menü dynamisch anpassen können.

1. Öffnen Sie die Datei MainPage_Basic. XAML. cs.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 5: Menü zur Laufzeit konfigurieren").
3. Heben Sie die Auskommentierung des Codes in den folgenden Methoden auf, und führen Sie die APP erneut aus, aber wählen Sie keine Schaltflächen (Speichern Sie diese für den nächsten Schritt).  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. Wählen Sie die Schaltfläche **Element entfernen** aus, und halten Sie die Auswahl gedrückt, um das Menü erneut anzuzeigen.

    Beachten Sie, dass das Menü nun die Standard Auflistung von Tools enthält. Denken Sie daran, dass wir in Schritt 3 beim Einrichten des benutzerdefinierten Menüs alle Standard Tools entfernt und nur unser benutzerdefiniertes Tool hinzugefügt haben. Wir haben auch festgestellt, dass die Standardelemente für den aktuellen Kontext wieder hergestellt werden, wenn das Menü auf eine leere Auflistung festgelegt ist. (Wir haben unser benutzerdefiniertes Tool vor dem Entfernen der Standard Tools hinzugefügt.)

5. Wählen Sie die Schaltfläche **Element hinzufügen** aus, und halten Sie die Auswahl gedrückt.

    Beachten Sie, dass das Menü nun sowohl die Standard Auflistung von Tools als auch das benutzerdefinierte Tool enthält.

6. Wählen Sie die Menü Schaltfläche " **radialcontroller zurücksetzen** " aus, und halten Sie die Auswahl gedrückt.

    Beachten Sie, dass das Menü wieder in seinen ursprünglichen Zustand wechselt.

## <a name="step-6-customize-the-device-haptics"></a>Schritt 6: Anpassen der Geräte Haptik
Die Oberfläche und andere radgeräte können Benutzern ein haptisches Feedback bereitstellen, das der aktuellen Interaktion entspricht (abhängig von Klick oder Drehung).

In diesem Schritt zeigen wir, wie Sie haptisches Feedback anpassen können, indem wir die Schieberegler-und UMSCHALT Steuerelemente zuordnen und diese verwenden, um das Verhalten von haptischem Feedback dynamisch anzugeben. In diesem Beispiel muss der umschaltschalter auf ON festgelegt werden, damit das Feedback aktiviert wird, während der Schieberegler-Wert angibt, wie oft das Click-Feedback wiederholt wird. 

> [!NOTE]
> Haptisches Feedback kann vom Benutzer auf der Seite mit den **Einstellungen**für Geräte deaktiviert werden  >   **Devices**  >  **Wheel** .

1. Öffnen Sie die Datei App.xaml.cs.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("Schritt 6: Anpassen der Geräte Haptik").
3. Kommentieren Sie die erste und dritte Zeile ("MainPage_Basic" und "MainPage") aus, und heben Sie die Auskommentierung der zweiten Zeile ("MainPage_Haptics") auf.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Öffnen Sie die Datei MainPage_Haptics. XAML.
5. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 6: Customize the device haptics --> ").
6. Entfernen Sie die Auskommentierung der folgenden Zeilen. (Dieser UI-Code gibt einfach an, welche Haptik Features vom aktuellen Gerät unterstützt werden.)    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. Öffnen Sie die Datei MainPage_Haptics. XAML. cs.
8. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("Schritt 6: willkürliche Anpassung").
9. Entfernen Sie die Auskommentierung der folgenden Zeilen:  

    - Die Typreferenz " [Windows. Devices. Haptics](/uwp/api/windows.devices.haptics) " wird in nachfolgenden Schritten für die Funktionalität verwendet.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Hier geben wir den Handler für das Ereignis " [controlerworde](/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) " an, das ausgelöst wird, wenn unser benutzerdefiniertes **radialcontroller** -Menü Element ausgewählt wird.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - Als nächstes definieren wir den [controlerworten](/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) Handler, in dem wir das standardmäßige haptische Feedback deaktivieren und die Benutzeroberfläche der Haptik initialisieren.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - In den Ereignis Handlern [rotationchanged](/uwp/api/windows.ui.input.radialcontroller.RotationChanged) und [buttongeklickt](/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) verbinden wir den entsprechenden Schieberegler und die UMSCHALT Fläche-Steuerelemente mit unserer benutzerdefinierten Haptik. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - Schließlich erhalten wir das angeforderte **[Wellen Formular](/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** (falls unterstützt) für das haptische Feedback. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

Führen Sie die App nun erneut aus, um die benutzerdefinierte Haptik zu testen, indem Sie den Schieberegler-Wert und den UMSCHALT Zustand umschalten.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Schritt 7: Definieren von Bildschirm Interaktionen für Surface Studio und ähnliche Geräte
In Kombination mit der Surface Studio-Oberfläche kann die Oberfläche auch eine noch präziseere Benutzeroberfläche bereitstellen. 

Das Surface Dial unterstützt nicht nur die beschriebene Standardmenüaktion „Drücken und Halten“, sondern kann zusätzlich direkt auf dem Bildschirm des Surface Studio platziert werden. Dadurch wird ein spezielles Onscreen-Menü aktiviert. 

Wenn Sie sowohl den Kontakt Standort als auch die Begrenzungen der Oberfläche ermitteln, verarbeitet das System die Okklusion vom Gerät und zeigt eine größere Version des Menüs an, die die Außenseite des wähles umschließt. Auch die App kann diese Informationen nutzen, um die Benutzeroberfläche an das Vorhandensein des Geräts und dessen beabsichtigte Nutzung anzupassen, z. B. daran, wie der Benutzer seine Hand und seinen Arm platziert. 

Das Beispiel für dieses Tutorial enthält ein etwas komplexeres Beispiel, das einige dieser Funktionen veranschaulicht.

Um dies in Aktion zu sehen (Sie benötigen ein Surface Studio):

1. Herunterladen des Beispiels auf einem Surface Studio-Gerät (mit installiertem Visual Studio)
2. Öffnen Sie das Beispiel in Visual Studio
3. Öffnen Sie die Datei app.XAML.cs.
4. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("Schritt 7: Definieren von Interaktionen auf dem Bildschirm für Surface Studio und ähnliche Geräte").
5. Kommentieren Sie die erste und zweite Zeile ("MainPage_Basic" und "MainPage_Haptics") aus, und heben Sie die Auskommentierung des dritten ("MainPage") auf.  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Führen Sie die APP aus, und platzieren Sie die Oberfläche in jeder der beiden Steuerelement Bereiche, die zwischen ihnen wechseln.    
![Auf dem Bildschirm radialcontroller](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Im folgenden finden Sie ein Video zu diesem Beispiel in Aktion:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben das Tutorial für die ersten Schritte abgeschlossen *: Unterstützung der Oberfläche (und anderer radgeräte) in Ihrer Windows-App*! Wir haben Ihnen den grundlegenden Code gezeigt, der für die Unterstützung eines radgeräts in Ihren Windows-apps erforderlich ist, und die Bereitstellung einiger der umfassenderen Benutzeroberflächen, die von den **radialcontroller** -APIs unterstützt werden

## <a name="related-articles"></a>Verwandte Artikel

[Surface Dial-Interaktionen](windows-wheel-interactions.md)

### <a name="api-reference"></a>API-Referenz

- [**Radialcontroller** -Klasse](/uwp/api/Windows.UI.Input.RadialController)
- [**Radialcontrollerbuttonclickedeventargs** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**Radialcontrollerconfiguration** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**Radialcontrollercontrolacquiredeventargs** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**Radialcontrollermenu** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**Radialcontrollermenuitem** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**Radialcontrollerrotationchangedeventargs** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**Radialcontrollerscreencontact** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**Radialcontrollerscreencontactcontinuedeventargs** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**Radialcontrollerscreencontactstartedeventargs** -Klasse](/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**Radialcontrollermenuknownicon** -Aufzählung](/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**Radialcontrollersystemmenuitemkind** -Enumeration](/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Beispiele

#### <a name="topic-samples"></a>Themenbeispiele

[Radialcontroller-Anpassung](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>Andere Beispiele
[Beispiel für ein Farb Buch](https://github.com/Microsoft/Windows-appsample-coloringbook)

[Universelle Windows-Plattform – Beispiele (C# und C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Klassischer Windows-Desktop – Beispiel](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)