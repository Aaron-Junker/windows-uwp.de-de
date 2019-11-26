---
ms.assetid: ''
title: Unterstützen von Surface Dial (und anderen Radgeräten) in Ihrer UWP-App
description: Eine ausführliche Anleitung zum Hinzufügen der Unterstützung für das Surface Dial (und anderen Radgeräten) zu Ihrer UWP-App.
keywords: Drehsteuerung, radial, Lernprogramm
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79503f38941fd6fd54c47582811c5e28c4d89db2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258287"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-uwp-app"></a>Lernprogramm: Unterstützen von Surface Dial (und anderen Radgeräten) in Ihrer UWP-App

![Abbildung der Oberflächen wählfläche mit Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Surface Dial mit Surface Studio und Surface Pen* (im [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116) käuflich erhältlich).

In diesem Lernprogramm erfahren Sie, wie Sie die von Radgeräten wie Surface Dial unterstützte Benutzerinteraktionen anpassen. Wie verwenden Codeausschnitte aus einer Beispiel-App, die Sie von GitHub herunterladen können (Siehe [Beispielcode](#sample-code)), um die verschiedenen Features und zugehörigen [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)-APIs zu veranschaulichen, die in den einzelnen Schritten beschrieben werden.

Wir konzentrieren uns auf Folgendes:
* Angeben, welche integrierten Tools in dem [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)-Menü angezeigt werden.
* Hinzufügen eines benutzerdefinierten Tools zum Menü
* Steuern des haptischen Feedbacks
* Anpassen der Klick-Interaktionen
* Anpassen der Drehinteraktionen

Weitere Informationen zur Implementierung dieser und anderer Features finden Sie unter [Surface Dial-Interaktionen in UWP-Apps](windows-wheel-interactions.md).

## <a name="introduction"></a>Einführung

Das Surface Dial ist ein sekundäres Eingabegerät, mit dem Benutzer zusammen mit einem primären Eingabegerät wie Stift, Toucheingabe oder Maus produktiver sein können. Als sekundäres Eingabegerät wird das Dial in der Regel mit der nicht dominanten Hand verwendet, um den Zugriff auf Systembefehle und andere kontextbezogene Tools und Funktionen bereitzustellen. 

Das Dial unterstützt drei grundlegende Gesten: 
- Drücken Sie und halten Sie, um das integrierte Menü mit Befehlen anzuzeigen.
- Drehen Sie zum Markieren eines Menüelements (wenn das Menü aktiv ist) oder zum Ändern der aktuelle Aktion in der App (wenn das Menü nicht aktiv ist).
- Klicken Sie, um das hervorgehobene Menüelement auswählen (wenn das Menü aktiv ist) oder um einen Befehl in der App aufzurufen (wenn das Menü nicht aktiv ist).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Computer (oder ein virtueller Computer) mit Windows 10 Creators Update oder höher
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Ein Radgerät (aktuell nur die [Surface Dial](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116))
* Wenn Sie noch keine Erfahrung mit der App-Entwicklung in der Universellen Windows-Plattform (UWP) mit Visual Studio haben, werfen Sie einen Blick in diese Themen, bevor Sie dieses Lernprogramm starten:  
    * [Vorbereiten](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Erstellen einer "Hello, World"-app (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Einrichten Ihrer Geräte

1. Stellen Sie sicher, dass Ihr Windows-Gerät aktiviert ist.
2. Wechseln Sie zu **Start** und wählen Sie **Einstellungen** > **Geräte** > **Bluetooth & andere Geräte** und schalten Sie **Bluetooth** an.
3. Entfernen Sie die Unterseite der Surface Dial, um das Batteriefach zu öffnen, und stellen Sie sicher, dass es zwei AAA-Batterien enthält.
4. Entfernen Sie falls nötig an der Unterseite der Dial den Batteriedeckel.
5. Drücken und halten Sie den kleinen eingesetzten Knopf neben den Batterien, bis das Bluetooth-Licht aufleuchtet.
6. Wechseln Sie zurück zu Ihrem Windows-Gerät und wählen Sie **Bluetooth oder ein anderes Gerät hinzufügen**.
7. Wählen Sie im Dialogfeld **Hinzufügen eines Geräts** **Bluetooth** > **Surface Dial**. Ihre Surface Dial sollte jetzt eine Verbindung herstellen und der Liste der Geräte unter **Maus, Tastatur und Stift** auf der Einstellungsseite **Bluetooth & andere Geräte** hinzugefügt werden.
8. Testen Sie das Dial, indem Sie sie einige Sekunden lang gedrückt halten, um das integrierte Menü anzuzeigen.
9. Wenn das Menü nicht auf dem Bildschirm angezeigt wird (die Auswahl muss ebenfalls vibrieren), kehren Sie zu den Bluetooth-Einstellungen zurück, entfernen Sie das Gerät, und versuchen Sie erneut, das Gerät zu verbinden.

> [!NOTE]
> Wheel-Geräte können in den **Rad**-Einstellungen angepasst werden:
> 1. Wählen Sie im **Startmenü** **Einstellungen**.
> 2. Wählen Sie **Geräte** > **Rad**.    
> Bildschirm "![Radeinstellungen"](images/radialcontroller/wheel-settings.png)

Jetzt können Sie dieses Lernprogramm starten. 

## <a name="sample-code"></a>Beispielcode
In diesem Lernprogramm verwenden wir eine Beispiels-App, um die erläuterten Konzepte und Funktionen zu veranschaulichen.

Laden Sie dieses Visual Studio-Beispiel und den Quellecode von [GitHub](https://github.com/) unter [Windows-App-Beispiel-Erste-Schritte-Radialcontroller-Beispiel](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController) herunter:

1. Wählen Sie die grüne Schaltfläche **Klonen oder herunterladen** aus.  
![Klonen des Repository](images/radialcontroller/wheel-clone.png)
2. Wenn Sie ein GitHub-Konto haben, können Sie das Repository auf Ihrem lokalen Computer mit der Option **In Visual Studio öffnen** klonen. 
3. Wenn Sie kein GitHub-Konto haben, oder wenn Sie einfach eine lokale Kopie des Projekts möchten, wählen Sie **Herunterladen der ZIP-Datei** (Sie müssen regelmäßig auf neues Updates prüfen).

> [!IMPORTANT]
> Der Großteil des Codes im Beispiel ist als Kommentar formatiert. Bei den einzelnen Schritten in diesem Thema werden Sie aufgefordert, die Kommentare der verschiedenen Codeabschnitte zu entfernen. Markieren Sie einfach in Visual Studio die Codezeilen und drücken Sie STRG+K und anschließend STRG + U.

## <a name="components-that-support-wheel-functionality"></a>Komponenten, die Radfunktionen unterstützen

Diese Objekte bieten den Großteil der Radgerätefunktion für UWP-Apps.

| Komponente | Beschreibung |
| --- | --- |
| [ **Radialcontroller** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController) und Verwandte | Stellt einen Radeingabegerät oder -Zubehör, z. B. das Surface Dial, dar. |
| [**Iradialcontrollerconfigurationinterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop) / [ **iradialcontrollerinterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>Wir gehen hier nicht auf diese Funktionalität ein. Weitere Informationen finden Sie unter der [Klassisches Windows-Desktopbeispiel](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController). | Ermöglicht die Interoperabilität mit einer UWP-App. |

## <a name="step-1-run-the-sample"></a>Schritt 1: Ausführen des Beispiels

Nachdem Sie die RadialController-Beispiel-App heruntergeladen haben, stellen Sie sicher, dass sie ausgeführt wird:
1. Öffnen Sie das Beispielprojekt in Visual Studio.
2. Legen Sie das **Lösungsplattformen**-Dropdownmenü für eine nicht ARM-Auswahl fest.
3. Drücken Sie F5, um zu kompilieren, bereitzustellen und auszuführen. 

> [!NOTE]
> Wahlweise können Sie auch **Debuggen** > Menü Element **Debuggen starten** auswählen oder die Schaltfläche **lokaler Computer** ausführen auswählen, die hier angezeigt wird: ![Visual Studio-Schaltfläche Projekt erstellen](images/radialcontroller/wheel-vsrun.png)

Das App-Fenster wird geöffnet und nach einem Begrüßungsbildschirm wird der Startbildschirm angezeigt.

![Leere App](images/radialcontroller/wheel-app-step1-empty.png)

OK, nun haben wir die grundlegende UWP-App, die wir im verbleibenden Teil dieses Lernprogramms verwenden werden. In den folgenden Schritten fügen wir unsere **RadialController**-Funktionen hinzu.

## <a name="step-2-basic-radialcontroller-functionality"></a>Schritt 2: Grundlegende RadialController-Funktionen

Wenn die app ausgeführt wird und Sie sich im Vordergrund befinden, halten Sie die Oberfläche gedrückt, um das Menü **radialcontroller** anzuzeigen.

Wir haben unsere App noch nicht angepasst, sodass das Menü einen Standardsatz von kontextbezogenen Tools enthält. 

Diese Abbildungen zeigen zwei Varianten des Standardmenüs. (Es gibt viele andere, einschließlich grundlegende Systemprogramme, wenn der Windows-Desktop aktiv ist und keine Apps im Vordergrund ausgeführt werden, zusätzliche Freihand-Tools, wenn eine InkToolbar vorhanden ist, und Zuordnungs-Tools, wenn Sie die Karten-App verwenden.

| RadialController-Menü (Standard)  | RadialController-Menü (Standard bei der Wiedergabe von Medien)  |
|---|---|
| ![RadialController-Standardmenü](images/radialcontroller/wheel-app-step2-basic-default.png) | ![RadialController-Standardmenü mit Musik](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

Jetzt werden wir einige grundlegende Anpassungen vornehmen.

## <a name="step-3-add-controls-for-wheel-input"></a>Schritt 3: Hinzufügen von Steuerelementen für die Radeingabe

Fügen wir zunächst die Benutzeroberfläche für unsere App hinzu:

1. Öffnen Sie die Datei „MainPage_Basic.xaml“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("\<!--Schritt 3: Hinzufügen von Steuerelementen für Wheel Input-->").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen.

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
An dieser Stelle sind nur die **Initialisierungsbeispiel**-Schaltfläche, Schieberegler und Ein/Aus-Schalter aktiviert. Die anderen Schaltflächen werden in späteren Schritten verwendet, um **RadialController**-Menüelemente, die den Zugriff auf die Schieberegler und Umschalter bereitstellen, hinzuzufügen und zu entfernen.

![Grundlegendes Beispiel für die App-UI](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Schritt 4: Anpassen des grundlegenden RadialController-Menüs

Fügen wir nun den für den **RadialController**-Zugriff auf unsere Steuerelemente erforderlichen Code hinzu.

1. Öffnen Sie die Datei „MainPage_Basic.xaml.cs”.
2. Suchen Sie den Code, der mit dem Titel in dieses Schritts gekennzeichnet ist („/ / Schritt 4: grundlegende RadialController Anpassung für das Menü”).
3. Entfernen Sie die Kommentare aus den folgenden Zeilen:
    - Die [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) und [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams)-Typenverweise werden für Funktionen in den folgenden Schritten verwendet:  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Diese globalen Objekte ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) werden in der App verwendet.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Hier geben wir den **Klick**-Ereignishandler für die Schaltfläche an, die unsere Steuerelemente aktiviert und das benutzerdefinierte **RadialController**-Menüelement initialisiert.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - Als Nächstes initialisieren wir unser [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)-Objekt, und richten Ereignishandler für die [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) und [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked)-Ereignisse ein.

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

    - Hier initialisieren wir unser benutzerdefiniertes RadialController-Menüelement. Wir verwenden [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView), um einen Verweis auf unser [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)-Objekt zu erhalten, wir legen die Drehempfindlichkeit mit der [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees)-Eigenschaft auf „1” fest, anschließend erstellen wir unser [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) mit [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), wir fügen das Menüelement zu der Sammlung von **RadialController**-Menüelementen und schließlich verwenden wir [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems), um die Standard-Menüelemente zu löschen und lassen nur die benutzerdefinierten Tools bestehen. 

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
4. Führen Sie nun erneut die App aus.
5. Wählen Sie die Schaltfläche **Initialisierung eines radialen Controllers**.  
6. Halten Sie das Surface Dial gedrückt, während die App im Vordergrund ausgeführt wird, um das Menü anzuzeigen. Wie Sie sehen, wurden alle standardmäßigen Tools entfernt (unter Verwendung der **RadialControllerConfiguration.SetDefaultMenuItems**-Methode), sodass nur das benutzerdefinierte Tool übrig bleibt. Hier sehen Sie das Menü mit unserem benutzerdefinierten Tool. 

| RadialController-Menü (benutzerdefiniert)  | 
|---|
| ![Benutzerdefiniertes RadialController-Menü](images/radialcontroller/wheel-app-step3-custom.png) |

7. Wählen Sie das benutzerdefinierte Tool aus und testen Sie die Interaktionen über die nun unterstützte Surface Dial:
    * Eine Drehaktion bewegt den Schieberegler. 
    * Ein Klick legt den Umschalter auf aktiviert oder deaktiviert.

OK, verknüpfen wir nun diese Schaltflächen.

## <a name="step-5-configure-menu-at-runtime"></a>Schritt 5: Konfigurieren des Menüs zur Laufzeit

In diesem Schritt verknüpfen wir die Schaltflächen **Element hinzufügen/entfernen** und **RadialController-Menü zurücksetzen**, um zu zeigen, wie Sie das Menü dynamisch anpassen können.

1. Öffnen Sie die Datei „MainPage_Basic.xaml.cs”.
2. Suchen Sie den Code, der mit dem Titel in dieses Schritts gekennzeichnet ist („// Schritt 5: Konfigurieren von Menü zur Laufzeit”).
3. Löschen Sie die Kommentare in den folgenden Methoden und führen Sie die App erneut aus. Wählen Sie dabei jedoch keine Schaltflächen aus (sparen Sie sich das für den nächsten Schritt auf).  

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
4. Wählen Sie die Schaltfläche **Element entfernen** halten Sie das Dial gedrückt, um das Menü erneut zu öffnen.

    Wie sie sehen, enthält das Menü jetzt die Standard-Tools. Erinnern Sie sich daran, dass Sie in Schritt 3 beim Einrichten unseres benutzerdefinierten Menüs alle standardmäßigen Tools entfernt und nur unser benutzerdefiniertes Tool hinzugefügt haben. Wir haben außerdem festgestellt, dass die standardmäßigen Elemente für den aktuellen Kontext wiederhergestellt werden, wenn das Menü auf eine leere Auflistung festgelegt ist. (Wir haben unser benutzerdefiniertes Tool vor dem Entfernen der Standardtools hinzugefügt.)

5. Wählen Sie die Schaltfläche **Element hinzufügen** und halten Sie dann das Dial gedrückt.

    Wie Sie sehen, beinhaltet das Menü jetzt sowohl die Standard-Tools und unser benutzerdefiniertes Tools.

6. Wählen Sie die Schaltfläche **RadialController-Menü zurücksetzen** und halten Sie dann das Dial gedrückt.

    Wie Sie sehen, wurde das Menü auf den ursprünglichen Zustand zurückgesetzt.

## <a name="step-6-customize-the-device-haptics"></a>Schritt 6: Anpassen der Gerätehaptik
Das Surface Dial und andere Radgeräte können Benutzern haptisches Feedback für die aktuelle Interaktion (basierend auf den Klick oder die Drehung) bereitstellen.

In diesem Schritt wird gezeigt, wie Sie das haptische Feedback anpassen können, indem Sie die Steuerelemente für den Schieberegler und Umschalter zuordnen, sodass das haptische Feedbackverhalten dynamisch festgelegt wird. In diesem Beispiel muss der Umschalter auf an gestellt sein, damit das Feedback aktiviert wird, während der Schiebereglerwert angibt, wie oft das Feedback wiederholt wird. 

> [!NOTE]
> Haptisches Feedback kann vom Benutzer auf der Seite **Einstellungen** >  **Geräte** > **Rad** deaktiviert werden.

1. Öffnen Sie die Datei „App.xaml.cs“:
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist („Schritt 6: Anpassen der Gerätehaptik”).
3. Kommentieren Sie die erste und dritte Zeile („MainPage_Basic” und „MainPage”), und entfernen Sie die Kommentare aus der zweiten Zeile („MainPage_Haptics”).  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Öffnen Sie die Datei „MainPage_Haptics.xaml“.
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("\<!--Schritt 6: Anpassen der Gerät->").
6. Entfernen Sie die Kommentare aus den folgenden Zeilen. (Dieser UI-Code gibt lediglich an, welche Haptikfunktionen von dem aktuellen Gerät unterstützt werden.)    

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
7. Öffnen Sie die Datei „MainPage_Haptics.xaml.cs“.
8. Suchen Sie den Code, mit dem Titel dieses Schritts gekennzeichnet ist („Schritt 6: Haptikeinstellungen”).
9. Entfernen Sie die Kommentare aus den folgenden Zeilen:  

    - Der [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics)-Typverweis wird für die Funktionen in den folgenden Schritten verwendet.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Hier geben wir den Handler für das [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired)-Ereignis an, das ausgelöst wird, wenn das benutzerdefinierte **RadialController**-Menüelement ausgewählt ist.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - Als Nächstes definieren wir den [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired)-Handler, in dem wir das standardmäßige Haptik-Feedback deaktivieren und unserer Haptik-UI initialisieren.

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

    - In unseren [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) und [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked)-Ereignishandlern verknüpfen wir die entsprechenden Schieberegler und Ein-/Aus-Steuerelemente mit unserer benutzerdefinierten Haptik. 

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
    - Schließlich erhalten wir die angeforderte **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** (falls unterstützt) für das Haptik-Feedback. 

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

Führen Sie nun die App erneut aus, um die benutzerdefinierte Haptik auszuprobieren, indem Sie den Schiebereglerwert und den Zustand des Ein-/Aus-Schalters ändern.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Schritt 7: Onscreen-Interaktionen auf dem Bildschirm für Surface Studio und ähnliche Geräte definieren
Im Zusammenspiel mit Surface Studio bietet das Surface Dial Benutzern ein noch innovativeres Bedienerlebnis. 

Das Surface Dial unterstützt nicht nur die beschriebene Standardmenüaktion „Drücken und Halten“, sondern kann zusätzlich direkt auf dem Bildschirm des Surface Studio platziert werden. Dadurch wird ein spezielles Onscreen-Menü aktiviert. 

Das System erkennt sowohl die Auflageposition als auch die Begrenzungen des Surface Dial, kann anhand dieser Informationen auf die durch das Gerät verursachte Okklusion reagieren und eine größere Menüversion darstellen, die außen um die Drehsteuerung herum angezeigt wird. Auch die App kann diese Informationen nutzen, um die Benutzeroberfläche an das Vorhandensein des Geräts und dessen beabsichtigte Nutzung anzupassen, z. B. daran, wie der Benutzer seine Hand und seinen Arm platziert. 

Das Beispiel, das mit diesem Lernprogramm einhergeht, enthält ein etwas komplexeres Beispiel, das einige dieser Funktionen veranschaulicht.

Um dies in Aktion zu sehen (dafür ist Surface Studio erforderlich):

1. Laden Sie das Beispiel auf einem Surface Studio-Gerät (mit Visual Studio installiert) herunter
2. Öffnen Sie das Beispiel in Visual Studio.
3. Öffnen Sie die Datei „App.xaml.cs“.
4. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist („Schritt 7: Onscreen-Interaktionen für Surface Studio und ähnliche Geräte definieren”)
5. Kommentieren Sie die zweite Zeile („MainPage_Basic” und „MainPage_Haptics”), und entfernen Sie die Kommentare aus der dritten Zeile („MainPage”).  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Führen Sie die App aus und legen Sie das Surface Dial die beiden Steuerungsbereiche, wechseln Sie zwischen ihnen.    
![Onscreen RadialController](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Hier sehen Sie ein Video zu diesem Beispiel in Aktion:  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben das *Lernprogramm für die ersten Schritte: Unterstützung von Surface Dial (und anderen Radgeräten) in Ihrer UWP-App* abgeschlossen! Wir haben Ihnen den für die Unterstützung von einem Radgerät in Ihren UWP-Apps erforderlichen Code gezeigt. Außerdem haben Sie erfahren, wie Sie Ihren Benutzern umfangreichere Erfahrungen bereitstellen, die von **RadialController**-APIs unterstützt werden.

## <a name="related-articles"></a>Verwandte Artikel

[Interaktionen bei der Oberflächen Wahl](windows-wheel-interactions.md)

### <a name="api-reference"></a>API-Referenz

- [**Radialcontroller** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)
- [**Radialcontrollerbuttonclickedeventargs** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**Radialcontrollerconfiguration** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**Radialcontrollercontrolacquiredeventargs** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**Radialcontrollermenu** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**Radialcontrollermenuitem** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**Radialcontrollerrotationchangedeventargs** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**Radialcontrollerscreencontact** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**Radialcontrollerscreencontactcontinuedeventargs** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**Radialcontrollerscreencontactstartedeventargs** -Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**Radialcontrollermenuknownicon** -Aufzählung](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**Radialcontrollersystemmenuitemkind** -Enumeration](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Beispiele

#### <a name="topic-samples"></a>Themenbeispiele

[Radialcontroller-Anpassung](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>Andere Beispiele
[Beispiel für ein Farb Buch](https://github.com/Microsoft/Windows-appsample-coloringbook)

[Universelle Windows-Plattform – Beispiele (C# und C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Beispiel für klassisches Windows-Desktop](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
