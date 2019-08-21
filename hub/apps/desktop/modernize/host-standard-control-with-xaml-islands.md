---
description: In diesem Artikel wird veranschaulicht, wie Sie ein Standardmäßiges UWP-Steuerelement in einer WPF-App mithilfe von XAML-Inseln hosten.
title: Hosten eines UWP-Standard Steuer Elements in einer WPF-App mithilfe von XAML-Inseln
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln, umschließende Steuerelemente, Standard Steuerelemente, InkCanvas, inktoolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: fc1b20561f67907fa973a0e30d0dc3b804c2472c
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643754"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hosten eines UWP-Standard Steuer Elements in einer WPF-App mithilfe von XAML-Inseln

In diesem Artikel werden zwei Möglichkeiten zum Hosten eines standardmäßigen UWP-Steuer Elements (d. h. eines UWP-Steuer Elements eines ersten Anbieters durch die Windows SDK-oder WinUI-Bibliothek) in einer WPF-App mithilfe von [XAML-Inseln](xaml-islands.md)veranschaulicht:

* Es wird gezeigt, wie Sie ein UWP-Steuerelement [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) und [inktoolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) mithilfe von umschließenden Steuer [Elementen](xaml-islands.md#wrapped-controls) im Windows Community Toolkit hosten. Diese Steuerelemente wrappen die Schnittstelle und Funktionalität eines kleinen Satzes nützlicher UWP-Steuerelemente. Sie können Sie direkt der Entwurfs Oberfläche Ihres WPF-oder Windows Forms Projekts hinzufügen und diese dann wie jedes andere WPF-oder Windows Forms-Steuerelement im Designer verwenden.
* Außerdem wird gezeigt, wie Sie ein UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) -Steuerelement mit dem [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit hosten. Da nur ein kleiner Satz von UWP-Steuerelementen als umschlenelte Steuerelemente verfügbar ist, können Sie mit [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) alle anderen UWP-Standard Steuerelemente hosten.

Obwohl in diesem Artikel das Hosten dieser Steuerelemente in einer WPF-App veranschaulicht wird, ist der Prozess bei einer Windows Forms-App ähnlich.

## <a name="create-a-wpf-project"></a>Erstellen eines WPF-Projekts

Befolgen Sie vor dem Einstieg diese Anweisungen, um ein WPF-Projekt zu erstellen und zum Hosten von XAML-Inseln zu konfigurieren. Wenn Sie über ein vorhandenes WPF-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

1. Erstellen Sie in Visual Studio 2019 ein neues Projekt **WPF-App (.NET Framework)** oder **WPF-App (.net Core)** .

2. Stellen Sie sicher, dass die [Paket Verweise](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf Extras **-> nuget-Paket-Manager-> Paket-Manager-Einstellungen**.
    2. Stellen Sie sicher, dass **packagereferenzierung** für das **standardmäßige Paket Verwaltungs Format**ausgewählt ist.

3. Klicken Sie mit der rechten Maustaste auf das WPF-Projekt in **Projektmappen-Explorer** und wählen Sie **nuget-Pakete verwalten**.

4. Vergewissern Sie sich, dass im Fenster **nuget-Paket-Manager** die Option **Vorabversion einschließen** ausgewählt ist.

5. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (Version v 6.0.0-preview7 oder höher), und installieren Sie das Paket. Dieses Paket enthält alles, was Sie für die Verwendung der umschlenenen UWP-Steuerelemente für WPF benötigen (einschließlich [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) und das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement).
    > [!NOTE]
    > Windows Forms-apps müssen das Paket [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (Version v 6.0.0-preview7 oder höher) verwenden.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hosten von InkCanvas und inktoolbar mithilfe von umschließenden Steuerelementen

Nachdem Sie Ihr Projekt für die Verwendung von UWP-XAML-Inseln konfiguriert haben, können Sie nun die in der APP umschließenen UWP-Steuerelemente [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) hinzufügen.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei " **MainWindow. XAML** ".

2. Fügen Sie im **Fenster** Element in der Nähe des oberen Rands der XAML-Datei das folgende Attribut hinzu. Dies verweist auf den XAML-Namespace für das umschließende UWP-Steuerelement [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) .

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Fenster** Element nun in etwa wie folgt aussehen.

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

3. Ersetzen Sie in der Datei " **MainWindow. XAML** " `<Grid>` das vorhandene-Element durch den folgenden XAML-Code. Dieser XAML- Code fügt dem `<Grid>`ein [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) -und [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) -Steuerelement hinzu (vorangestellt das Steuerelement-Steuerelement, das Sie zuvor als Namespace definiert haben).

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
    > Sie können diese und andere umschließende Steuerelemente auch dem-Fenster hinzufügen, indem Sie Sie aus dem **Windows Community Toolkit** -Abschnitt der **Toolbox** in den Designer ziehen.

4. Speichern Sie die Datei " **MainWindow. XAML** ".

    Wenn Sie ein Gerät haben, das einen digitalen Stift wie eine Oberfläche unterstützt, und Sie dieses Lab auf einem physischen Computer ausführen, können Sie nun die APP erstellen und ausführen und auf dem Bildschirm mit dem Stift eine digitale Handschrift zeichnen. Wenn Sie jedoch nicht über ein Stift fähiges Gerät verfügen und versuchen, sich mit der Maus anzumelden, geschieht nichts. Dies geschieht, da das **InkCanvas** -Steuerelement standardmäßig nur für digitale Stifte aktiviert ist. Sie können dieses Verhalten jedoch ändern.

5. Öffnen Sie die Datei **MainWindow.XAML.cs** .

6. Fügen Sie am Anfang der Datei die folgende Namespace Deklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Suchen Sie `MainWindow()` den-Konstruktor. Fügen Sie nach der `InitializeComponent()` -Methode die folgende Codezeile hinzu, und speichern Sie die Codedatei.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Sie können das **InkPresenter** -Objekt verwenden, um die standardmäßige Einbindung zu ändern. In diesem Code wird die **inputdevicetypes** -Eigenschaft verwendet, um die Maus und die Stift Eingabe zu aktivieren.

8. Drücken Sie erneut F5, um die APP im Debugger neu zu erstellen und auszuführen. Wenn Sie einen Computer mit der Maus verwenden, vergewissern Sie sich, dass Sie im frei Handzeichen Bereich mit der Maus etwas zeichnen können.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hosten einer CalendarView mithilfe des Host Steuer Elements

Nachdem Sie der App nun die umschlenenen UWP-Steuerelemente [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) und [inktoolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) hinzugefügt haben, können Sie nun das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement verwenden, um der App eine [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) hinzuzufügen.

> [!NOTE]
> Das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement wird vom [Microsoft. Toolkit. WPF. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) -Paket bereitgestellt. Dieses Paket ist im Paket [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) enthalten, das Sie zuvor installiert haben.

1. Öffnen Sie in **Projektmappen-Explorer**die Datei " **MainWindow. XAML** ".

2. Fügen Sie im **Fenster** Element in der Nähe des oberen Rands der XAML-Datei das folgende Attribut hinzu. Dies verweist auf den XAML-Namespace für das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Fenster** Element nun in etwa wie folgt aussehen.

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

4. Ersetzen Sie in der Datei " **MainWindow. XAML** " `<Grid>` das vorhandene-Element durch den folgenden XAML-Code. Mit diesem XAML-Code wird dem Raster eine Zeile hinzugefügt, und der letzten Zeile wird ein [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Objekt hinzugefügt. Um ein UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) -Steuerelement zu hosten, legt dieser `InitialTypeName` XAML-Code die-Eigenschaft auf den voll qualifizierten Namen des-Steuer Elements fest. Dieser XAML-Code definiert auch einen Ereignishandler `ChildChanged` für das-Ereignis, das ausgelöst wird, wenn das gehostete Steuerelement gerendert wurde.

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

5. Speichern Sie die Datei " **MainWindow. XAML** ", und öffnen Sie die Datei **MainWindow.XAML.cs** .

7. Fügen Sie am Anfang der Datei die folgende Namespace Deklaration hinzu:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Fügen Sie der `ChildChanged` - `MainWindow` Klasse die folgende Ereignishandlermethode hinzu, und speichern Sie die Codedatei. Wenn das Host Steuerelement gerendert wurde, wird dieser Ereignishandler ausgeführt, und es wird ein `SelectedDatesChanged` einfacher Ereignishandler für das-Ereignis des Kalender Steuer Elements erstellt.

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

11. Drücken Sie erneut F5, um die APP im Debugger neu zu erstellen und auszuführen. Vergewissern Sie sich, dass das Kalender Steuerelement jetzt am unteren Rand des Fensters angezeigt wird.

## <a name="package-the-app"></a>Verpacken der APP

Sie können die WPF-App optional in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung verpacken. Msix ist die moderne App-Verpackungstechnologie für Windows und basiert auf einer Kombination aus MSI-, AppX-, App-V-und ClickOnce-Installationstechnologien.

Die folgenden Anweisungen zeigen, wie Sie alle Komponenten in der Projekt Mappe in einem msix-Paket Verpacken, indem Sie das [Windows-Anwendungs](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) Paket in Visual Studio 2019 verwenden. Diese Schritte sind nur erforderlich, wenn Sie die WPF-App in einem msix-Paket verpacken möchten.

1. Fügen Sie der Projekt Mappe ein neues [Windows-Anwendungspaket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts die Option **Windows 10, Version 1903 (10,0; Build 18362)** für die **Zielversion** und die **Mindestversion**.

2. Klicken Sie im Paket Projekt mit der rechten Maustaste auf den Knoten **Anwendungen** , und wählen Sie **Verweis hinzufügen**aus. Wählen Sie in der Liste der Projekte das WPF-Projekt in der Projekt Mappe aus, und klicken Sie auf **OK**.

3. Wenn Ihr WPF-Projekt auf .net Core 3 ausgerichtet ist, müssen Sie die Projektdatei für die Paket Erstellung bearbeiten. Diese Änderungen sind derzeit erforderlich, um WPF-apps zu verpacken, die auf .net Core 3 ausgerichtet sind und UWP-Steuerelemente hosten.

    1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Knoten Verpackungsprojekt, und wählen Sie dann **Projektdatei bearbeiten**aus.
    2. Suchen Sie das `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />`-Element in der Datei. Ersetzen Sie dieses Element durch den folgenden XML-Code.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject>
                </SourceProject>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Speichern Sie die Projektdatei, und schließen Sie Sie.

4. Konfigurieren Sie die Lösung so, dass Sie auf eine bestimmte Plattform wie x86 oder x64 ausgerichtet ist. Dies ist erforderlich, um die WPF-App mithilfe des paketpakets für die Windows-Anwendung in einem msix-Paket zu erstellen.

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Eigenschaften** -> **Konfigurations Eigenschaften** -> **Configuration Manager**.
    2. Wählen Sie unter **aktive**Projektmappenplattform die Option **x64** oder **x86**aus.
    3. Wählen Sie in der Zeile für Ihr WPF-Projekt in der Spalte **Plattform** die Option **neu**aus.
    4. Wählen Sie im Dialogfeld Neue Projektmappenplattform die Option **x64** oder **x86** (dieselbe Plattform, die Sie für die **aktive Lösungsplattform**ausgewählt haben) aus, und klicken Sie auf
    5. Schließen Sie die Dialogfelder öffnen.

5. Erstellen Sie das Verpackungsprojekt, und führen Sie es aus. Vergewissern Sie sich, dass WPF ausgeführt wird und das benutzerdefinierte UWP-Steuerelement erwartungsgemäß angezeigt wird.

## <a name="related-topics"></a>Verwandte Themen

* [UWP-Steuerelemente in Desktop Anwendungen](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
