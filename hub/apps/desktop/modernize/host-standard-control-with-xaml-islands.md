---
description: Dieser Artikel veranschaulicht, wie du ein standardmäßiges WinRT-XAML-Steuerelement in einer WPF-App mithilfe von XAML Islands hostest.
title: Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App unter Verwendung von XAML Islands
ms.date: 10/02/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands, umschlossene Steuerelemente, Standardsteuerelemente, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: a8f5b141e5726d19651aeafeb9b6d432e20c2f47
ms.sourcegitcommit: b8d0e2c6186ab28fe07eddeec372fb2814bd4a55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91671529"
---
# <a name="host-a-standard-winrt-xaml-control-in-a-wpf-app-using-xaml-islands"></a>Hosten eines WinRT-XAML-Standardsteuerelements in einer WPF-App unter Verwendung von XAML Islands

In diesem Artikel werden zwei Möglichkeiten zum Hosten eines WinRT-XAML-Standardsteuerelements (d. h. eines vom Windows SDK bereitgestellten WinRT-XAML-Erstanbietersteuerelements) in einer WPF-App mithilfe von [XAML Islands](xaml-islands.md) veranschaulicht:

* Es wird gezeigt, wie du die UWP-Steuerelemente [InkCanvas](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) und [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar) unter Verwendung von [umschlossenen Steuerelementen](xaml-islands.md#wrapped-controls) im Windows-Community-Toolkit hostest. Diese Steuerelemente kapseln Schnittstelle und Funktionalität einer kleinen Teilmenge nützlicher WinRT-XAML-Steuerelemente. Du kannst sie direkt zur Entwurfsoberfläche deines WPF- oder Windows Forms-Projekts hinzufügen und sie dann wie jedes andere WPF- oder Windows Forms-Steuerelement im Designer verwenden.

* Außerdem wird gezeigt, wie du ein [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)-UWP-Steuerelement durch Verwendung des [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements im Windows-Community-Toolkit hosten kannst. Da nur eine kleine Teilmenge der WinRT-XAML-Steuerelemente in Form von umschlossenen Steuerelementen verfügbar ist, kannst du [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) verwenden, um jedes andere WinRT-XAML-Standardsteuerelement zu hosten.

In diesem Artikel wird gezeigt, wie du WinRT-XAML-Steuerelemente in einer WPF-App hosten kannst, aber das Vorgehen für eine Windows Forms-App ist ähnlich.

## <a name="required-components"></a>Erforderliche Komponenten

Um ein WinRT-XAML-Steuerelement in einer WPF-App (oder einer Windows Forms-App) zu hosten, benötigen Sie die folgenden Komponenten in Ihrer Projektmappe. Dieser Artikel stellt Anweisungen zum Erstellen jeder dieser Komponenten bereit.

* **Projekt- und Quellcode für deine App**. Die Verwendung des [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements zum Hosten benutzerdefinierter WinRT-XAML-Steuerelemente wird nur in Apps für die Zielplattform .NET Core 3.x unterstützt.

* **UWP-App-Projekt mit Definition einer Stammanwendungsklasse, die von XamlApplication abgeleitet ist**. Dein WPF- oder Windows Forms-Projekt muss Zugriff auf eine Instanz der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse haben, die durch das Windows-Community-Toolkit bereitgestellt wird, sodass benutzerdefinierte UWP XAML-Steuerelemente ermittelt und geladen werden können. Die empfohlene Vorgehensweise hierfür besteht darin, dieses Objekt in einem separaten UWP-App-Projekt zu definieren, das nicht Bestandteil der Projektmappe für Ihre WPF- oder Windows Forms-App ist. 

    > [!NOTE]
    > Wenngleich das `XamlApplication`-Objekt nicht zum Hosten eines WinRT-XAML-Erstanbietersteuerelements erforderlich ist, benötigt deine App dieses Objekt, um die ganze Bandbreite an XAML Islands-Szenarien zu unterstützen, einschließlich des Hostings benutzerdefinierter WinRT-XAML-Steuerelemente. Deshalb wird empfohlen, dass du immer ein `XamlApplication`-Objekt in einer Projektmappe definierst, in der du XAML Islands verwendest.

    > [!NOTE]
    > Deine Projektmappe kann nur ein Projekt enthalten, das ein `XamlApplication`-Objekt definiert. Das Projekt, in dem das `XamlApplication`-Objekt definiert ist, muss Verweise auf alle weiteren Bibliotheken und -Projekte enthalten, die zum Hosten von WinRT-XAML-Steuerelementen in XAML Islands verwendet werden.

## <a name="create-a-wpf-project"></a>Erstellen eines WPF-Projekts

Befolgen Sie vor dem Einstieg diese Anweisungen, um ein WPF-Projekt zu erstellen und es zum Hosting in XAML Islands zu konfigurieren. Wenn du über ein vorhandenes WPF-Projekt verfügst, kannst du diese Schritte und Codebeispiele für dein Projekt anpassen.

1. Erstellen Sie in Visual Studio 2019 ein neues **WPF-App (.NET Core)** -Projekt. Zuerst müssen Sie die neueste Version von [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/current) installieren, wenn dies noch nicht geschehen ist.

2. Vergewissern Sie sich, dass [Paketverweise](/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Stellen Sie sicher, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

3. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr WPF-Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

4. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls), und installieren Sie die neueste stabile Version. Dieses Paket enthält alle benötigten Elemente zur Verwendung der umschlossenen WinRT-XAML-Steuerelemente für WPF (einschließlich der Steuerelemente [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas), [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) und [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)).
    > [!NOTE]
    > Windows Forms-Apps müssen das Paket [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) verwenden.

5. Konfiguriere deine Projektmappe für eine Zielplattform wie beispielsweise x86 oder x64. Die meisten XAML Islands-Szenarien werden nicht für Projekte unterstützt, die als Ziel **Beliebige CPU** verwenden.

    1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wähle **Eigenschaften** -> **Konfigurationseigenschaften** -> **Konfigurations-Manager** aus. 
    2. Klicken Sie unter **Aktive Projektmappenplattform** auf **Neu**. 
    3. Wählen Sie im Dialogfeld **Neue Projektmappenplattform** die Option **x64** oder **x86** aus, und klicken Sie auf **OK**. 
    4. Schließen Sie die geöffneten Dialogfelder.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definieren einer XamlApplication-Klasse in einem UWP-App-Projekt

Als Nächstes fügen Sie Ihrer Projektmappe ein UWP-App-Projekt hinzu und überarbeiten die `App`-Standardklasse in diesem Projekt, um sie von der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse abzuleiten, die im Windows-Community-Toolkit bereitgestellt wird. Diese Klasse unterstützt die [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider)-Schnittstelle, mit der deine App Metadaten für benutzerdefinierte UWP-XAML-Steuerelemente in Assemblys im aktuellen Verzeichnis der Anwendung zur Laufzeit ermitteln und laden kann. Diese Klasse initialisiert außerdem das UWP-XAML-Framework für den aktuellen Thread.

> [!NOTE]
> Wenngleich dieser Schritt nicht zum Hosten eines WinRT-XAML-Erstanbietersteuerelements erforderlich ist, benötigt Ihre App das `XamlApplication`-Objekt, um die ganze Bandbreite an XAML Islands-Szenarien zu unterstützen, einschließlich des Hostings benutzerdefinierter WinRT-XAML-Steuerelemente. Deshalb wird empfohlen, dass du immer ein `XamlApplication`-Objekt in einer Projektmappe definierst, in der du XAML Islands verwendest.

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wähle **Hinzufügen** -> **Neues Projekt** aus.
2. Fügen Sie Ihrer Projektmappe ein Projekt vom Typ **Leere App (Universal Windows)** hinzu. Vergewissern Sie sich, dass sowohl die Zielversion als auch die mindestens erforderliche Version auf **Windows 10, Version 1903 (Build 18362)** oder höher festgelegt ist.
3. Installieren Sie im UWP-App-Projekt das NuGet-Paket [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (neueste stabile Version).
4. Öffnen Sie die Datei **App.xaml**, und ersetzen Sie deren Inhalte durch den nachstehenden XAML-Ausschnitt. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Öffnen Sie die Datei **App.xaml.cs**, und ersetzen Sie die Inhalte dieser Datei durch den nachstehenden Code. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Lösche die Datei **MainPage.xaml** aus dem UWP-App-Projekt.
7. Kompiliere das UWP-App-Projekt.

## <a name="add-a-reference-to-the-uwp-project-in-your-wpf-project"></a>Fügen Sie in Ihrem WPF-Projekt einen Verweis auf das UWP-Projekt hinzu.

1. Geben Sie die kompatible Frameworkversion in der WPF-Projektdatei an. 

    1. Doppelklicken Sie im **Projektmappen-Explorer** auf den WPF-Projektknoten, um die Projektdatei im Editor zu öffnen.
    2. Fügen Sie im ersten **PropertyGroup**-Element das folgende untergeordnete Element hinzu. Ändern Sie den `19041`-Teil des Werts nach Bedarf auf die Werte des Ziel- und Mindestbetriebssystembuilds des UWP-Projekts.

        ```xml
        <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        ```

        Wenn Sie fertig sind, sollte das **PropertyGroup**-Element ungefähr wie dieses aussehen.

        ```xml
        <PropertyGroup>
            <OutputType>WinExe</OutputType>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UseWPF>true</UseWPF>
            <Platforms>AnyCPU;x64</Platforms>
            <AssetTargetFallback>uap10.0.19041</AssetTargetFallback>
        </PropertyGroup>
        ```

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Knoten **Abhängigkeiten** unter dem WPF-Projekt, und fügen Sie einen Verweis auf Ihr UWP-App-Projekt hinzu.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Instanziieren des XamlApplication-Objekts im Einstiegspunkt Ihrer WPF-App

Als Nächstes fügst du Code zum Einstiegspunkt für deine WPF-App hinzu, um eine Instanz der `App`-Klasse zu erstellen, die du soeben im UWP-Projekt definiert hast (dies ist die Klasse, die jetzt von `XamlApplication` abgeleitet wird).

1. Klicke in deinem WPF-Projekt mit der rechten Maustaste auf den Projektknoten, wähle **Hinzufügen** -> **Neues Element** und dann **Klasse** aus. Geben Sie der Klasse den Namen **Program**, und klicken Sie auf **Hinzufügen**.

2. Ersetzen Sie die generierte `Program`-Klasse durch den folgenden Code, und speichern Sie dann die Datei. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts und `MyWPFApp` durch den Namespace Ihres WPF-App-Projekts.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Eigenschaften** aus.

4. Klicken Sie auf der Registerkarte **Anwendung** der Eigenschaften auf die Dropdownliste **Startobjekt**, und wählen Sie den vollqualifizierten Namen der `Program`-Klasse aus, die Sie im vorherigen Schritt hinzugefügt haben. 
    > [!NOTE]
    > Standardmäßig definieren WPF-Projekte eine `Main`-Einstiegspunktfunktion in einer generierten Codedatei, die nicht zur Bearbeitung vorgesehen ist. Durch diesen Schritt wird der Einstiegspunkt für Ihr Projekt in die `Main`-Methode der neuen `Program`-Klasse geändert, wodurch Sie Code hinzufügen können, der so früh wie möglich in der Startphase der App ausgeführt wird. 

5. Speichere deine Änderungen an den Projekteigenschaften.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hosten von InkCanvas und InkToolbar mithilfe umschlossener Steuerelemente

Nachdem du dein Projekt zur Verwendung von UWP XAML Islands konfiguriert hast, kannst du jetzt die umschlossenen WinRT-XAML-Steuerelemente [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) zur App hinzufügen.

1. Öffnen Sie in Ihrem WPF-Projekt die Datei **MainWindow.xaml**.

2. Füge dem **Window**-Element im oberen Bereich der XAML-Datei das folgende Attribut hinzu. So wird auf den XAML-Namespace für die umschlossenen WinRT-XAML-Steuerelemente [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) verwiesen.

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Window**-Element in etwa wie folgt aussehen.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. Ersetze in der Datei **MainWindow.xaml** das vorhandene `<Grid>`-Element durch den folgenden XAML-Ausschnitt. Hiermit wird ein [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)- und ein [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)-Steuerelement (mit dem **Controls**-Schlüsselwort als Präfix, das zuvor als Namespace definiert wurde) zu `<Grid>` hinzugefügt.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > Du kannst diese und weitere umschlossene Steuerelemente zum Fenster hinzufügen, indem du sie vom Abschnitt **Windows-Community-Toolkit** in der **Toolbox** auf den Designer ziehst.

4. Speichere die Datei **MainWindow.xaml**.

    Wenn du über ein Gerät mit Unterstützung eines digitalen Stifts verfügst – wie beispielsweise Surface – und du dieses Lab auf einem physischen Computer ausführst, könntest du jetzt die App kompilieren und ausführen und mit dem digitalen Stift auf dem Bildschirm zeichnen. Wenn du jedoch über kein solches Gerät verfügst und versuchst, mit deiner Maus zu zeichnen, geschieht nichts. Dies liegt daran, dass das **InkCanvas**-Steuerelement standardmäßig nur für die Eingabe mit einem digitalen Stift aktiviert ist. Dieses Verhalten kannst du jedoch ändern.

5. Öffne die Datei **MainWindow.xaml.cs**.

6. Füge am Anfang der Datei die folgende Namespacedeklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Suche nach dem `MainWindow()`-Konstruktor. Füge nach der `InitializeComponent()`-Methode die folgende Codezeile ein, und speichere die Codedatei.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Du kannst das Standardverhalten für die Freihandeingabe mit dem **InkPresenter**-Objekt anpassen. Dieser Code verwendet die **InputDeviceTypes**-Eigenschaft, um sowohl die Maus- als auch die Stifteingabe zu aktivieren.

8. Drücke erneut F5, um die App neu zu kompilieren und im Debugger auszuführen. Wenn du einen Computer mit Maus verwendest, vergewissere dich, dass du im Zeichenbereich mit der Maus zeichnen kannst.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hosten einer CalendarView durch Verwendung des Hoststeuerelements

Nachdem du die umschlossenen WinRT-XAML-Steuerelemente [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) zur App hinzugefügt hast, kannst du jetzt das Steuerelement [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) verwenden, um der App eine [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) hinzuzufügen.

> [!NOTE]
> Das Steuerelement [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) wird vom Paket [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) bereitgestellt. Dieses Paket ist im [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)-Paket enthalten, das du in einem vorherigen Schritt installiert hast.

1. Öffne im **Projektmappen-Explorer** die Datei **MainWindow.xaml**.

2. Füge dem **Window**-Element im oberen Bereich der XAML-Datei das folgende Attribut hinzu. Dieses Attribut verweist auf den XAML-Namespace für das [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelement.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Window**-Element in etwa wie folgt aussehen.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. Ersetze in der Datei **MainWindow.xaml** das vorhandene `<Grid>`-Element durch den folgenden XAML-Ausschnitt. Hierdurch wird dem Raster eine Zeile hinzugefügt und das [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Objekt in die letzte Zeile eingefügt. Zum Hosten eines UWP-Steuerelements [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) legt dieser XAML-Ausschnitt die `InitialTypeName`-Eigenschaft auf den vollqualifizierten Namen des Steuerelements fest. Darüber hinaus wird auf diese Weise ein Ereignishandler für das `ChildChanged`-Ereignis definiert, das ausgelöst wird, wenn das gehostete Steuerelement gerendert wurde.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. Speichere die Datei **MainWindow.xaml**, und öffne die Datei **MainWindow.xaml.cs**.

7. Füge am Anfang der Datei die folgende Namespacedeklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Füge der `MainWindow`-Klasse die folgende `ChildChanged`-Ereignishandlermethode hinzu, und speichere die Codedatei. Wenn das Hoststeuerelement gerendert wurde, wird dieser Ereignishandler ausgeführt und erstellt einen einfachen Ereignishandler für das `SelectedDatesChanged`-Ereignis des Kalendersteuerelements.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. Drücke erneut F5, um die App neu zu kompilieren und im Debugger auszuführen. Vergewissere dich, dass das Kalendersteuerelement jetzt im unteren Fensterbereich angezeigt wird.

## <a name="package-the-app"></a>Packen der App

Sie können die WPF-App optional in einem [MSIX-Paket](/windows/msix) für die Bereitstellung verpacken. MSIX ist eine moderne App-Pakettechnologie für Windows, die auf einer Kombination aus MSI-, APPX-, App-V- und ClickOnce-Installationstechnologien basiert.

Die folgenden Anweisungen veranschaulichen, wie Sie alle Komponenten in der Projektmappe in ein MSIX-Paket einschließen können, indem Sie die Option [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) in Visual Studio 2019 verwenden. Diese Schritte sind nur erforderlich, wenn du die WPF-App in einem MSIX-Paket packen möchtest.

> [!NOTE]
> Wenn du deine Anwendung nicht in einem [MSIX-Paket](/windows/msix) für die Bereitstellung packst, muss auf Computern zur Ausführung deiner App die [Visual C++-Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

1. Fügen Sie Ihrer Projektmappe ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts **Windows 10, Version 1903 (10.0; Build 18362)** sowohl für **Zielversion** als auch für **Mindestversion** aus.

2. Klicken Sie im Paketprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und wählen Sie **Verweis hinzufügen** aus. Wähle in der Liste der Projekte das WPF-Projekt in deiner Projektmappe aus, und klicke auf **OK**.

3. Konfiguriere deine Projektmappe für eine Zielplattform wie beispielsweise x86 oder x64. Dies ist erforderlich, um die WPF-App über „Paketerstellungsprojekt für Windows-Anwendungen“ in einem MSIX-Paket zu packen.

    1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wähle **Eigenschaften** -> **Konfigurationseigenschaften** -> **Konfigurations-Manager** aus.
    2. Wähle unter **Aktive Projektmappenplattform** die Option **x64** oder **x86** aus.
    3. Wähle in der Zeile für dein WPF-Projekt in der Spalte **Plattform** die Option **Neu** aus.
    4. Wähle im Dialogfeld **Neue Projektmappenplattform** die Option **x64** oder **x86** aus (dieselbe Plattform, die du für **Aktive Projektmappenplattform** ausgewählt hast), und klicke auf **OK**.
    5. Schließe die geöffneten Dialogfelder.

5. Kompiliere das Paketerstellungsprojekt, und führe es aus. Vergewissern Sie sich, dass die WPF-App ausgeführt und das benutzerdefinierte UWP-Steuerelement wie erwartet angezeigt wird.

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)