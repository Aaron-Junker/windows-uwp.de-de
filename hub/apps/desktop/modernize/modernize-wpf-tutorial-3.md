---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP XAML-Benutzeroberflächen hinzufügen, MSIX-Pakete erstellen und weitere moderne Komponenten in Ihre WPF-App integrieren.
title: Hinzufügen eines UWP-CalendarView-Steuerelements mithilfe von XAML Islands
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "69643376"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Teil 3: Hinzufügen eines UWP-CalendarView-Steuerelements mithilfe von XAML Islands

Dies ist der dritte Teil eines Tutorials, in dem das Modernisieren der WPF-Desktop-Beispiel-App Contoso Expenses veranschaulicht wird. Eine Übersicht über das Tutorial, die Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden Sie unter [Tutorial: Modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass Sie [Teil 2](modernize-wpf-tutorial-2.md) bereits abgeschlossen haben.

Im fiktiven Szenario dieses Tutorials möchte das Contoso-Entwicklungsteam die Auswahl des Datums für eine Spesenabrechnung auf einem mobilen Gerät vereinfachen. In diesem Teil des Tutorials fügen Sie der App ein UWP-[CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)-Steuerelement hinzu. Dabei handelt es sich um dasselbe Steuerelement, das für die Windows 10-Funktionen für Datum und Uhrzeit auf der Taskleiste verwendet wird.

![CalendarViewControl-Abbildung](images/wpf-modernize-tutorial/CalendarViewControl.png)

Im Unterschied zum **InkCanvas**-Steuerelement, das Sie in [Teil 2](modernize-wpf-tutorial-2.md) hinzugefügt haben, bietet das Windows-Community-Toolkit keine umschlossene Version von UWP **CalendarView**, die in WPF-Apps eingesetzt werden kann. Als Alternative hosten Sie ein **InkCanvas** im generischen [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelement. Sie können dieses Steuerelement verwenden, um ein beliebiges UWP-Originalsteuerelement aus dem Windows SDK oder der WinUI-Bibliothek oder sogar ein beliebiges benutzerdefiniertes UWP-Steuerelement eines Drittanbieters zu hosten. Das **WindowsXamlHost**-Steuerelement steht im NuGet-Paket `Microsoft.Toolkit.Wpf.UI.XamlHost` zur Verfügung. Dieses Paket ist im NuGet-Paket `Microsoft.Toolkit.Wpf.UI.Controls` enthalten, das Sie in [Teil 2](modernize-wpf-tutorial-2.md) installiert haben.

> [!NOTE]
> In diesem Tutorial wird nur veranschaulicht, wie [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) zum Hosten des ersten **CalendarView**-Steuerelements aus dem Windows SDK verwendet wird. Eine exemplarische Vorgehensweise, aus der Sie ersehen können, wie ein benutzerdefiniertes Steuerelement gehostet wird, finden Sie unter [Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App unter Verwendung von XAML Islands](host-custom-control-with-xaml-islands.md).

Zum Verwenden des **WindowsXamlHost**-Steuerelements müssen Sie WinRT-APIs aus dem Code in der WPF-App direkt aufrufen. Das NuGet-Paket `Microsoft.Windows.SDK.Contracts` enthält die erforderlichen Verweise, mit denen Sie WinRT-APIs aus der App aufrufen können. Dieses Paket ist ebenfalls im NuGet-Paket `Microsoft.Toolkit.Wpf.UI.Controls` enthalten, das Sie in [Teil 2](modernize-wpf-tutorial-2.md) installiert haben.

## <a name="add-the-windowsxamlhost-control"></a>Hinzufügen des WindowsXamlHost-Steuerelements

1. Erweitern Sie im **Projektmappen-Explorer** den Ordner **Ansichten** im Projekt **ContosoExpenses.Core**, und doppelklicken Sie auf die Datei **AddNewExpense.xaml**. Dies ist das Formular, mit dem der Liste neue Spesen hinzugefügt werden. Hier sehen Sie, wie es in der aktuellen Version der App angezeigt wird.

    ![Neue Spesen hinzufügen](images/wpf-modernize-tutorial/AddNewExpense.png)

    Das in WPF enthaltene Datumsauswahl-Steuerelement ist für herkömmliche Computer mit Maus und Tastatur gedacht. Die Auswahl eines Datums mithilfe eines Touchscreens ist aufgrund der geringen Größe des Steuerelements und des begrenzten Platzes zwischen den einzelnen Tagen im Kalender kaum möglich.

2. Fügen Sie oben in der Datei **AddNewExpense.xaml** dem **Window**-Element das folgende Attribut hinzu.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Window**-Element in etwa so aussehen.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. Ändern Sie das **Height**-Attribut des **Window**-Elements von 450 in 800. Dies ist erforderlich, da das UWP-**CalendarView**-Steuerelement mehr Platz als das WPF-Datumsauswahl-Steuerelement benötigt.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. Suchen Sie das `DatePicker`-Element am Ende der Datei, und ersetzen Sie dieses Element durch den folgenden XAML-Code.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Dieser XAML-Code für das **WindowsXamlHost**-Steuerelement hinzu. Die **InitialTypeName**-Eigenschaft gibt den vollständigen Namen des UWP-Steuerelements an, das Sie hosten möchten (in diesem Fall **Windows.UI.Xaml.Controls.CalendarView**).

5. Drücken Sie F5, um die App zu kompilieren und im Debugger auszuführen. Wählen Sie einen Mitarbeiter in der Liste aus, und drücken Sie dann die Schaltfläche **Add new expense** (Neue Spesen hinzufügen). Vergewissern Sie sich, dass die folgende Seite die neue UWP-**CalendarView**-Steuerelement hostet.

    ![CalendarView-Wrapper](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Schließen Sie die App.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagieren mit dem WindowsXamlHost-Steuerelement

Im nächsten Schritt aktualisieren Sie die App, damit sie das ausgewählte Datum verarbeitet, es auf dem Bildschirm anzeigt und das **Expense**-Objekt auffüllt, um es in der Datenbank zu speichern.

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) enthält zwei Elemente, die für dieses Szenario relevant sind:

- Die **SelectedDates**-Eigenschaft enthält das vom Benutzer ausgewählte Datum.
- Das **SelectedDatesChanged**-Ereignis wird ausgelöst, wenn der Benutzer ein Datum auswählt.

Bei dem **WindowsXamlHost**-Steuerelement handelt es sich jedoch um ein generisches Hoststeuerelement für *jede* Art von UWP-Steuerelement. Als solches macht es keine Eigenschaft mit dem Namen **SelectedDates** oder ein Ereignis mit dem Namen **SelectedDatesChanged** verfügbar, da beide für das **CalendarView**-Steuerelement spezifisch sind. Um auf diese Elemente zuzugreifen, müssen Sie Code schreiben, der den Typ **WindowsXamlHost** in den Typ **CalendarView** konvertiert. Der beste Ort dafür ist in der Antwort auf das **ChildChanged**-Ereignis des **WindowsXamlHost**-Steuerelements, das ausgelöst wird, wenn das gehostete Steuerelement gerendert wurde.

1. Fügen Sie in der **AddNewExpense.xaml**-Datei einen Ereignishandler für das **ChildChanged**-Ereignis des **WindowsXamlHost**-Steuerelements hinzu, das Sie zuvor hinzugefügt haben. Wenn Sie den Vorgang abgeschlossen haben, sollte das **WindowsXamlHost**-Element wie folgt aussehen.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Suchen Sie in der gleichen Datei das **Grid.RowDefinitions**-Element des **Grid**-Hauptrasters. Fügen Sie am Ende der Liste der untergeordneten Elemente ein weiteres **RowDefinition**-Element mit auf **Auto** festgelegter Eigenschaft **Height** hinzu. Wenn Sie den Vorgang abgeschlossen haben, sollte das **Grid.RowDefinitions**-Element wie folgt aussehen (es sollten nun 9 **RowDefinition**-Elemente vorhanden sein).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. Fügen Sie den folgenden XAML-Code nach dem **WindowsXamlHost**-Element und vor dem **Button**-Element kurz vorm Ende der Datei hinzu.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Suchen Sie das **Button**-Element in der Nähe des Dateiendes, und ändern Sie die **Grid.Row**-Eigenschaft von **7** in **8**. Dadurch wird die Schaltfläche um eine Zeile im Raster nach unten verschoben, da Sie eine neue Zeile hinzugefügt haben.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Öffnen Sie die Codedatei **AddNewExpense.xaml.cs**.

7. Fügen Sie am Anfang der Datei die folgende Anweisung hinzu.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Fügen Sie der Klasse `AddNewExpense` den folgenden Ereignishandler hinzu. Dieser Code implementiert das **ChildChanged**-Ereignis des **WindowsXamlHost**-Steuerelements, das Sie früher in der XAML-Datei deklariert hatten.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
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
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    Dieser Code verwendet die **Child**-Eigenschaft des **WindowsXamlHost**-Steuerelements, um auf das UWP-**CalendarView**-Steuerelement zuzugreifen. Anschließend abonniert der Code das **SelectedDatesChanged**-Ereignis, das ausgelöst wird, wenn der Benutzer ein Datum im Kalender auswählt. Dieser Ereignishandler übergibt das ausgewählte Datum an das ViewModel, in dem das neue **Expense**-Objekt erstellt und in der Datenbank gespeichert wird. Zu diesem Zweck verwendet der Code die Messaging-Infrastruktur, die vom MVVM Light NuGet-Paket bereitgestellt wird. Der Code sendet eine Nachricht mit der Bezeichnung **SelectedDateMessage** an das ViewModel, die sie empfängt und die **Date**-Eigenschaft auf den ausgewählten Wert festlegt. In diesem Szenario ist das **CalendarView**-Steuerelement für den Einzelauswahlmodus konfiguriert, sodass die Sammlung nur ein Element enthält.

    An diesem Punkt lässt sich das Projekt immer noch nicht kompilieren, da ihm die Implementierung der **SelectedDateMessage**-Klasse fehlt. In den folgenden Schritten wird diese Klasse implementiert.

9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Messages** (Nachrichten), und wählen Sie **Hinzufügen > Klasse** aus. Benennen Sie die neue Klasse **SelectedDateMessage**, und klicken Sie auf **Hinzufügen**.

10. Ersetzen Sie den Inhalt der **SelectedDateMessage.cs**-Codedatei durch den folgenden Code. Dieser Code fügt eine using-Anweisung für den `GalaSoft.MvvmLight.Messaging`-Namespace (aus dem MVVM Light NuGet-Paket) hinzu, erbt aus der **MessageBase**-Klasse, und fügt eine **DateTime**-Eigenschaft hinzu, die mithilfe des öffentlichen Konstruktors initialisiert wird. Wenn Sie fertig sind, sollte die Datei so aussehen:

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. Als nächstes aktualisieren Sie das ViewModel, um die Nachricht zu empfangen und die **Date**-Eigenschaft des ViewModel aufzufüllen. Erweitern Sie im **Projektmappen-Explorer** den Ordner **ViewModels**, und öffnen Sie die **AddNewExpenseViewModel.cs**-Datei.

12. Suchen Sie den öffentlichen Konstruktor für die Klasse `AddNewExpenseViewModel`, und fügen Sie am Ende des Konstruktors den folgenden Code hinzu. Dieser Code registriert sich für den Empfang der **SelectedDateMessage**, extrahiert mithilfe der Eigenschaft **SelectedDate** das ausgewählte Datum aus ihr, und wir verwenden ihn, um die vom ViewModel verfügbar gemachte **Date**-Eigenschaft festzulegen. Da diese Eigenschaft an das **TextBlock**-Steuerelement gebunden ist, das Sie zuvor hinzugefügt haben, können Sie das vom Benutzer ausgewählte Datum sehen.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Wenn Sie fertig sind, sollte der `AddNewExpenseViewModel`-Konstruktor so aussehen. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > Durch die `SaveExpenseCommand`-Methode in derselben Codedatei werden die Spesen in der Datenbank gespeichert. Diese Methode verwendet die **Date**-Eigenschaft, um das neue **Expense**-Objekt zu erstellen. Diese Eigenschaft wird nun mithilfe der soeben von Ihnen erstellten `SelectedDateMessage`-Nachricht vom **CalendarView**-Steuerelement aufgefüllt.

13. Drücken Sie F5, um die App zu kompilieren und im Debugger auszuführen. Wählen Sie einen der verfügbaren Mitarbeiter aus, und klicken Sie dann auf die Schaltfläche **Add new expense** (Neue Spesen hinzufügen). Füllen Sie alle Felder im Formular aus, und wählen Sie im neuen **CalendarView**-Steuerelement ein Datum aus. Vergewissern Sie sich, dass das ausgewählte Datum unterhalb des Steuerelements als Zeichenfolge angezeigt wird.

14. Drücken Sie die Schaltfläche **Speichern**. Das Formular wird geschlossen, und die neue Spesenausgabe wird der Liste der Spesen am Ende hinzugefügt. Die erste Spalte mit dem Spesendatum sollte das Datum aufweisen, das Sie im **CalendarView**-Steuerelement ausgewählt haben.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt im Tutorial haben Sie ein WPF-Datum/Uhrzeit-Steuerelement erfolgreich durch das UWP-**CalendarView**-Steuerelement ersetzt, das über die Maus- und Tastatureingabe hinaus auch Toucheingabe und digitale Stifte unterstützt. Obwohl das Windows-Community-Toolkit keine umschlossene Version des UWP-**CalendarView**-Steuerelements bereitstellt, das direkt in einer WPF-App verwendet werden kann, konnten Sie das Steuerelement mithilfe des generischen [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements hosten.

Sie sind jetzt bereit für [Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und -Benachrichtigungen](modernize-wpf-tutorial-4.md).
