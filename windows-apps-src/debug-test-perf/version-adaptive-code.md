---
title: Versionsadaptiver Code
description: Mithilfe der neuen ApiInformation-Klasse kannst du neue APIs nutzen und gleichzeitig die Kompatibilität mit früheren Versionen gewährleisten.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 3293e91e-6888-4cc3-bad3-61e5a7a7ab4e
ms.localizationpriority: medium
ms.openlocfilehash: 2c03475c0c4007508a18c17645dbe99eeb7d6cb0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75681981"
---
# <a name="version-adaptive-code"></a>Versionsadaptiver Code

Das Schreiben von adaptivem Code ist vergleichbar mit dem [Erstellen einer adaptiven UI](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml). Dabei können Sie etwa Ihre grundlegende Benutzeroberfläche für den kleinsten Bildschirm entwerfen und anschließend Elemente verschieben oder hinzufügen, wenn erkannt wird, dass die App auf einem größeren Bildschirm ausgeführt wird. Bei adaptivem Code schreiben Sie Ihren Basiscode für die niedrigste Betriebssystemversion und können anschließend ausgewählte Features hinzufügen, wenn erkannt wird, dass Ihre App unter einer höheren Version ausgeführt wird, die über das neue Feature verfügt.

Wichtige Hintergrundinformationen zu ApiInformation, API-Verträgen und dem Konfigurieren von Visual Studio findest du unter [Versionsadaptive Apps](version-adaptive-apps.md).

### <a name="runtime-api-checks"></a>API-Laufzeitprüfungen

Verwenden Sie die [Windows.Foundation.Metadata.ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation)-Klasse in einer Bedingung in Ihrem Code, um zu testen, ob die aufzurufende API vorhanden ist. Diese Bedingung wird immer ausgewertet – ganz gleich, wo Ihre App ausgeführt wird. Das Ergebnis ist aber nur dann **True**, wenn sie auf einem Gerät ausgeführt wird, auf dem die API vorhanden ist und somit aufgerufen werden kann. Auf diese Weise kannst du Apps mit versionsadaptivem Code unter Nutzung von APIs erstellen, die nur unter bestimmten Betriebssystemversionen verfügbar sind.

Hier gehen wir anhand von spezifischen Beispielen auf die Nutzung neuer Features in Windows Insider Preview ein. Eine allgemeine Übersicht über die Verwendung von **ApiInformation** findest du in der [Übersicht zu Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code) und im Blogbeitrag [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) (Dynamisches Erkennen von Features mithilfe von API-Verträgen).

> [!TIP]
> Eine hohe Anzahl von API-Laufzeitprüfungen kann die Leistung Ihrer App beeinträchtigen. Die Prüfungen werden in diesen Beispielen inline veranschaulicht. In Produktionscode empfiehlt es sich, die Prüfung nur einmal durchzuführen, das Ergebnis zwischenzuspeichern und es anschließend in der gesamten App zu verwenden. 

### <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

In den meisten Fällen können Sie die auf die SDK-Version 10240 festgelegte Mindestversion Ihrer App beibehalten und neue APIs auf der Grundlage von Laufzeitprüfungen aktivieren, wenn die App unter einer höheren Version ausgeführt wird. In bestimmten Fällen müssen Sie allerdings die Mindestversion Ihrer App erhöhen, um neue Features verwenden zu können.

Die Mindestversion der App muss erhöht werden, wenn Sie Folgendes verwenden:
- Eine neue API, die eine Funktion erfordert, die in einer früheren Version nicht verfügbar ist. Die unterstützte Mindestversion muss auf eine höhere Version festgelegt werden, die über diese Funktion verfügt. Weitere Informationen finden Sie unter [Deklaration der App-Funktionen](../packaging/app-capability-declarations.md).
- Einen neuen Ressourcenschlüssel, der zu „generic.xaml“ hinzugefügt wurde und in einer früheren Version nicht verfügbar war. Die zur Laufzeit verwendete Version von „generic.xaml“ wird durch die auf dem Gerät ausgeführte Betriebssystemversion bestimmt. Mit API-Laufzeitprüfungen lässt sich nicht ermitteln, ob XAML-Ressourcen vorhanden sind. Daher dürfen nur Ressourcenschlüssel verwendet werden, die in der von der App unterstützten Mindestversion zur Verfügung stehen. Andernfalls bringt eine [XAMLParseException](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlparseexception) Ihre App zur Laufzeit zum Absturz.

### <a name="adaptive-code-options"></a>Optionen für adaptiven Code

Adaptiver Code kann auf zwei Arten erstellt werden. In den meisten Fällen schreiben Sie den Markupcode Ihrer App für die Mindestversion und verwenden dann Ihren App-Code, um ggf. die Nutzung neuerer Betriebssystemfeatures zu ermöglichen. Wenn Sie allerdings eine Eigenschaft in einem visuellen Zustand aktualisieren müssen und sich zwischen Betriebssystemversionen lediglich ein Eigenschafts- oder Enumerationswert ändert, können Sie einen erweiterbaren Zustandsauslöser erstellen, dessen Aktivierung davon abhängt, ob eine API vorhanden ist.

Im Anschluss finden Sie eine Gegenüberstellung dieser Optionen:

**App-Code**

Verwendung
- Empfohlen für alle Szenarien mit adaptivem Code mit Ausnahme bestimmter Fälle, die weiter unten für erweiterbare Auslöser definiert sind.

Vorteile:
- Weniger Aufwand/Komplexität für Entwickler beim Einbinden von API-Unterschieden in das Markup

Nachteile:
- Keine Designerunterstützung

**Zustandsauslöser**

Verwendung
- Verwenden Sie diese Option, wenn sich zwischen Betriebssystemversionen nur eine Eigenschaft oder Enumeration geändert hat, keine Änderung der Logik erforderlich ist und eine Verbindung mit einem visuellen Zustand besteht.

Vorteile:
- Ermöglicht die Erstellung spezifischer visueller Zustände, die abhängig davon ausgelöst werden, ob eine API vorhanden ist.
- Eingeschränkte Designerunterstützung verfügbar.

Nachteile:
- Die Verwendung von benutzerdefinierten Auslösern ist auf visuelle Zustände beschränkt, weshalb sich die Option nicht für komplizierte adaptive Layouts eignet.
- Wertänderungen müssen mithilfe von Settern angegeben werden, sodass nur einfache Änderungen möglich sind.
- Die Einrichtung und Verwendung benutzerdefinierter Zustandsauslöser ist relativ aufwendig.

## <a name="adaptive-code-examples"></a>Beispiele für adaptiven Code

In diesem Abschnitt zeigen wir verschiedene Beispiele für adaptiven Code, in denen neue APIs aus der Windows 10-Version 1607 (Windows Insider Preview) verwendet werden.

### <a name="example-1-new-enum-value"></a>Beispiel 1: Neuer Enumerationswert

In Windows 10 Version 1607 wird die Enumeration [InputScopeNameValue](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscopenamevalue) um einen neuen Wert erweitert: **ChatWithoutEmoji**. Dieser neue Eingabeumfang besitzt das gleiche Eingabeverhalten wie der Eingabeumfang **Chat** (Rechtschreibprüfung, AutoVervollständigen, automatische Großschreibung), wird aber einer Bildschirmtastatur ohne Emoji-Schaltfläche zugeordnet. Dies ist hilfreich, wenn Sie Ihre eigene Emoji-Auswahl erstellen und die integrierte Emoji-Schaltfläche auf der Bildschirmtastatur deaktivieren möchten. 

Dieses Beispiel zeigt, wie Sie überprüfen, ob der **ChatWithoutEmoji**-Enumerationswert vorhanden ist, und die [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)-Eigenschaft eines **TextBox**-Elements festlegen, falls dies der Fall ist. Ist der Wert in dem System, auf dem die App ausgeführt wird, nicht vorhanden, wird **InputScope** stattdessen auf **Chat** festgelegt. Der hier gezeigte Code kann in einem Page-Konstruktor oder in einem Page.Loaded-Ereignishandler platziert werden.

> [!TIP]
> Verlassen Sie sich beim Prüfen einer API nicht auf .NET-Sprachfeatures, sondern verwenden Sie statische Zeichenfolgen. Andernfalls versucht Ihre App unter Umständen, auf einen nicht definierten Typ zuzugreifen, was einen Absturz zur Laufzeit zur Folge hat.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

Im vorherigen Beispiel wird das TextBox-Element erstellt, und alle Eigenschaften werden im Code festgelegt. Wenn Sie allerdings bereits über XAML verfügen und lediglich die InputScope-Eigenschaft für Systeme ändern möchten, auf denen der neue Wert unterstützt wird, können Sie dies wie hier zu sehen ohne XAML-Änderung implementieren. Der Standardwert wird in XAML auf **Chat** festgelegt und im Code überschrieben, falls der **ChatWithoutEmoji**-Wert vorhanden ist.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

Nachdem wir nun über ein konkretes Beispiel verfügen, sehen wir uns einmal an, wie in diesem Fall die Einstellungen für Mindest- und Zielversion angewendet werden.

In diesen Beispielen können Sie den Chat-Enumerationswert in XAML oder in Code ohne Überprüfung verwenden, da der Wert in der unterstützten Mindestversion des Betriebssystems vorhanden ist. 

Wenn Sie den ChatWithoutEmoji-Wert in XAML oder in Code ohne Überprüfung verwenden, wird er ohne Fehler kompiliert, da der Wert in der Zielversion des Betriebssystems vorhanden ist. Auch die Ausführung auf einem System mit der Zielversion des Betriebssystems ist ohne Fehler möglich. Wenn die App allerdings auf einem System mit der Mindestversion des Betriebssystems ausgeführt wird, stürzt sie zur Laufzeit ab, da der ChatWithoutEmoji-Enumerationswert nicht vorhanden ist. Daher dürfen Sie diesen Wert nur in Code verwenden und müssen ihn mit einer API-Laufzeitprüfung umschließen, sodass er nur aufgerufen wird, wenn dies auf dem aktuellen System unterstützt wird.

### <a name="example-2-new-control"></a>Beispiel 2: Neues Steuerelement

Eine neue Version von Windows verfügt üblicherweise über neue Steuerelemente für die UWP-API-Oberfläche, die den Funktionsumfang der Plattform erweitern. Das Vorhandensein eines neuen Steuerelements kann mithilfe der [ApiInformation.IsTypePresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.istypepresent)-Methode ermittelt werden.

In der Windows 10-Version 1607 wird ein neues Mediensteuerelement namens [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) eingeführt. Dieses Steuerelement basiert auf der [MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)-Klasse, stellt beispielweise Features wie die problemlose Einbindung von Hintergrundaudio bereit und profitiert von Verbesserungen bei der Architektur des Medienstapels.

Wenn die App allerdings auf einem Gerät mit einer älteren Windows 10-Version als 1607 ausgeführt wird, muss anstelle des neuen **MediaPlayerElement**-Steuerelements das [**MediaElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement)-Steuerelement verwendet werden. Sie können mithilfe der [**ApiInformation.IsTypePresent**](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.istypepresent)-Methode zur Laufzeit überprüfen, ob das MediaPlayerElement-Steuerelement vorhanden ist, und dann das geeignete Steuerelement für das System laden, auf dem die App ausgeführt wird.

Dieses Beispiel zeigt, wie Sie eine App erstellen, die abhängig davon, ob der MediaPlayerElement-Typ vorhanden ist, entweder das neue MediaPlayerElement-Steuerelement oder das alte MediaElement-Steuerelement verwendet. In diesem Code werden die Steuerelemente samt dazugehöriger Benutzeroberfläche und entsprechendem Code mithilfe der [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)-Klasse in Komponenten zerlegt, sodass sie abhängig von der Betriebssystemversion einbezogen werden können. Alternativ können Sie ein benutzerdefiniertes Steuerelement verwendet. Dieses bietet mehr Funktionen und benutzerdefiniertes Verhalten als in diesem einfachen Beispiel erforderlich.
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` kapselt ein **MediaPlayerElement** und mehrere Schaltflächen für eine framebasierte Mediennavigation. „UserControl“ ermöglicht die Behandlung dieser Steuerelemente als einzelne Entität und erleichtert den Wechsel zu „MediaElement“ bei älteren Systemen. Dieses Benutzersteuerelement sollte nur für Systeme verwendet werden, die über „MediaPlayerElement“ verfügen, sodass im Code innerhalb dieses Benutzersteuerelements keine ApiInformation-Prüfungen ausgeführt werden.

> [!NOTE]
> Die Schaltflächen für die Frameschritte werden außerhalb des Medienplayers platziert, um dieses Beispiel möglichst einfach und prägnant zu halten. Zur Verbesserung der Benutzerfreundlichkeit sollten Sie „MediaTransportControls“ mit Ihren benutzerdefinierten Schaltflächen anpassen. Weitere Informationen finden Sie unter [Benutzerdefinierte Transportsteuerelemente](https://docs.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls). 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` kapselt ein **MediaElement**-Steuerelement.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> Die Codepage für `MediaElementUserControl` enthält nur generierten Code und wird daher nicht gezeigt.

**Initialisieren eines Steuerelements auf der Grundlage von IsTypePresent**

Rufen Sie zur Laufzeit **ApiInformation.IsTypePresent** auf, um zu prüfen, ob „MediaPlayerElement“ vorhanden ist. Falls ja, wird `MediaPlayerUserControl` geladen. Andernfalls wird `MediaElementUserControl` geladen.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> Zur Erinnerung: Diese Überprüfung legt nur das `mediaControl`-Objekt auf `MediaPlayerUserControl` oder `MediaElementUserControl` fest. Diese bedingten Überprüfungen müssen überall im Code durchgeführt werden, wo Sie ermitteln müssen, ob die MediaPlayerElement- oder die MediaElement-API verwendet werden soll. Es empfiehlt sich, die Prüfung nur einmal durchzuführen, das Ergebnis zwischenzuspeichern und es anschließend in der gesamten App zu verwenden.

## <a name="state-trigger-examples"></a>Beispiele für Zustandsauslöser

Mithilfe erweiterbarer Zustandsauslöser können Sie Markup und Code zusammen verwenden, um Änderungen des visuellen Zustands auf der Grundlage einer im Code überprüften Bedingung auszulösen (in diesem Fall: das Vorhandensein einer bestimmten API). In allgemeinen Szenarien mit adaptivem Code wird die Verwendung von Zustandsauslösern aufgrund des zusätzlichen Aufwands und der Beschränkung auf visuelle Zustände nicht empfohlen. 

Verwenden Sie Zustandsauslöser nur dann für adaptiven Code, wenn lediglich geringfügige Benutzeroberflächenänderungen zwischen Betriebssystemversionen vorliegen, die sich nicht auf die restliche Benutzeroberfläche auswirken (etwa die Änderung einer Eigenschaft oder eines Enumerationswerts für ein Steuerelement).

### <a name="example-1-new-property"></a>Beispiel 1: Neue Eigenschaft

Der erste Schritt bei der Einrichtung eines erweiterbaren Zustandsauslösers besteht in der Verwendung von Unterklassen für die [StateTriggerBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.statetriggerbase)-Klasse zur Erstellung eines benutzerdefinierten Auslösers, dessen Aktivierung davon abhängt, ob eine API vorhanden ist. Dieses Beispiel zeigt einen Auslöser, der aktiviert wird, wenn das Vorhandensein der Eigenschaft der in XAML festgelegten `_isPresent`-Variablen entspricht.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

Im nächsten Schritt wird der visuelle Zustandsauslöser in XAML eingerichtet, um abhängig vom Vorhandensein der API zwei unterschiedliche visuelle Zustände zu erhalten. 

In der Windows 10-Version 1607 wird für die [FrameworkElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement)-Klasse eine neue Eigenschaft namens [AllowFocusOnInteraction](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.allowfocusoninteraction) eingeführt, die bestimmt, ob ein Steuerelement den Fokus erhält, wenn ein Benutzer damit interagiert. Dies ist hilfreich, falls ein Textfeld für die Dateneingabe den Fokus behalten (und die Bildschirmtastatur weiter angezeigt werden) soll, wenn der Benutzer auf eine Schaltfläche klickt.

Der Auslöser in diesem Beispiel überprüft, ob die Eigenschaft vorhanden ist. Ist die Eigenschaft vorhanden, wird die **AllowFocusOnInteraction**-Eigenschaft für eine Schaltfläche auf **False** festgelegt. Ist die Eigenschaft nicht vorhanden, wird der ursprüngliche Zustand der Schaltfläche beibehalten. Das TextBox-Element dient zur Veranschaulichung der Auswirkung dieser Eigenschaft, wenn Sie den Code ausführen.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="example-2-new-enum-value"></a>Beispiel 2: Neuer Enumerationswert

Dieses Beispiel zeigt, wie Sie abhängig davon, ob ein Wert vorhanden ist, unterschiedliche Enumerationswerte festlegen. Hierbei kommt ein benutzerdefinierter Zustandsauslöser zum Einsatz, um das gleiche Ergebnis zu erhalten wie im vorherigen Chatbeispiel. In diesem Beispiel wird der neue ChatWithoutEmoji-Eingabeumfang verwendet, wenn auf dem Gerät die Windows 10-Version 1607 ausgeführt wird. Andernfalls wird der Eingabeumfang **Chat** verwendet. Die visuellen Zustände, die diesen Auslöser verwenden, werden im *if-else*-Stil eingerichtet, wobei die Wahl des Eingabeumfangs davon abhängt, ob der neue Enumerationswert vorhanden ist.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

## <a name="related-articles"></a>Verwandte Artikel

- [Übersicht über Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
- [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) (Dynamisches Erkennen von Features mithilfe von API-Verträgen)
