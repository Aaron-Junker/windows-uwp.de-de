---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, msix-Pakete erstellen und andere moderne Komponenten in Ihre WPF-App integrieren.
title: Hinzufügen eines UWP-CalendarView-Steuerelements mithilfe von XAML Islands
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643376"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Teil 3: Hinzufügen eines UWP-CalendarView-Steuerelements mithilfe von XAML Islands

Dies ist der dritte Teil eines Tutorials, in dem veranschaulicht wird, wie Sie eine WPF-Beispiel-Desktop-App mit dem Namen "" mit den Kosten für " Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden [Sie unter Tutorial: Modernisieren einer WPF-](modernize-wpf-tutorial.md)app. In diesem Artikel wird davon ausgegangen, dass Sie [Teil 2](modernize-wpf-tutorial-2.md)bereits abgeschlossen haben.

Im fiktiven Szenario dieses Lernprogramms möchte das Entwicklungsteam von "Configuration Manager" das Datum für einen Ausgaben Bericht auf einem Touchscreen-fähigen Gerät leichter auswählen. In diesem Teil des Tutorials fügen Sie der APP ein UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) -Steuerelement hinzu. Dabei handelt es sich um dasselbe Steuerelement, das in der Windows 10-Funktionen für Datum und Uhrzeit auf der Taskleiste verwendet wird.

![Calendarviewcontrol-Bild](images/wpf-modernize-tutorial/CalendarViewControl.png)

Im Gegensatz zum **InkCanvas** -Steuerelement, das Sie in [Teil 2](modernize-wpf-tutorial-2.md)hinzugefügt haben, bietet das Windows Community Toolkit keine umschließende Version der UWP **CalendarView** , die in WPF-Apps verwendet werden kann. Als Alternative können Sie eine **InkCanvas** im generischen [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement hosten. Sie können dieses Steuerelement zum Hosten beliebiger UWP-Steuerelemente von UWP verwenden, die von der Windows SDK oder der WinUI-Bibliothek bereitgestellt werden Das **windowsxamlhost** -Steuerelement wird vom `Microsoft.Toolkit.Wpf.UI.XamlHost` Paket nuget-Paket bereitgestellt. Dieses Paket ist in dem `Microsoft.Toolkit.Wpf.UI.Controls` nuget-Paket enthalten, das Sie in [Teil 2](modernize-wpf-tutorial-2.md)installiert haben.

> [!NOTE]
> Dieses Tutorial veranschaulicht nur die Verwendung von [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) zum Hosten des vom Windows SDK bereitgestellten **CalendarView** -Steuer Elements. Eine exemplarische Vorgehensweise, die das Hosten eines benutzerdefinierten Steuer Elements veranschaulicht, finden [Sie unter Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App mithilfe von XAML](host-custom-control-with-xaml-islands.md)

Um das **windowsxamlhost** -Steuerelement zu verwenden, müssen Sie WinRT-APIs direkt aus dem Code in der WPF-App abrufen. Das `Microsoft.Windows.SDK.Contracts` nuget-Paket enthält die Verweise, die erforderlich sind, damit Sie WinRT-APIs von der APP aufrufen können. Dieses Paket ist auch in dem `Microsoft.Toolkit.Wpf.UI.Controls` nuget-Paket enthalten, das Sie in [Teil 2](modernize-wpf-tutorial-2.md)installiert haben.

## <a name="add-the-windowsxamlhost-control"></a>Hinzufügen des Steuer Elements "windowsxamlhost"

1. Erweitern Sie in **Projektmappen-Explorer**den Ordner **views** im Projekt **contosoaufwendungen. Core** , und doppelklicken Sie auf die Datei **addnewexpse. XAML** . Dies ist das Formular, mit dem der Liste neue Ausgaben hinzugefügt werden. Hier sehen Sie, wie Sie in der aktuellen Version der App angezeigt wird.

    ![Neue Ausgaben hinzufügen](images/wpf-modernize-tutorial/AddNewExpense.png)

    Das in WPF enthaltene Steuerelement für die Datumsauswahl ist für herkömmliche Computer mit Maus und Tastatur gedacht. Die Auswahl eines Datums mit einem Touchscreen ist aufgrund der geringen Größe des Steuer Elements und des begrenzten Platzes zwischen den einzelnen Tagen im Kalender nicht wirklich möglich.

2. Fügen Sie am Anfang der Datei " **addnewexpse. XAML** " dem **Window** -Element das folgende-Attribut hinzu.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Nach dem Hinzufügen dieses Attributs sollte das **Fenster** Element nun wie folgt aussehen.

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

3. Ändern Sie das **height** -Attribut des **Window** -Elements von 450 in 800. Dies ist erforderlich, da das UWP **CalendarView** -Steuerelement mehr Platz benötigt als die WPF-Datumsauswahl.

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

4. Suchen Sie `DatePicker` das-Element am Ende der Datei, und ersetzen Sie dieses Element durch den folgenden XAML-Code.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Mit diesem XAML-Code wird das **windowsxamlhost** -Steuerelement hinzugefügt. Die **initialtypame** -Eigenschaft gibt den vollständigen Namen des UWP-Steuer Elements an, das Sie hosten möchten (in diesem Fall **Windows. UI. XAML. Controls. CalendarView**).

5. Drücken Sie F5, um die APP im Debugger zu erstellen und auszuführen. Wählen Sie einen Mitarbeiter aus der Liste aus, und klicken Sie dann auf die Schaltfläche **neue Ausgaben hinzufügen** . Vergewissern Sie sich, dass die folgende Seite das neue UWP **CalendarView** -Steuerelement hostet.

    ![CalendarView-Wrapper](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Schließen Sie die App.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagieren mit dem windowsxamlhost-Steuerelement

Als nächstes aktualisieren Sie die APP, um das ausgewählte Datum zu verarbeiten, auf dem Bildschirm anzuzeigen und das **Ausgaben** Objekt zu füllen, das in der Datenbank gespeichert werden soll.

Die UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) enthält zwei Member, die für dieses Szenario relevant sind:

- Die **SelectedDates** -Eigenschaft enthält das Datum, das vom Benutzer ausgewählt wurde.
- Das **selecteddateschangi-** Ereignis wird ausgelöst, wenn der Benutzer ein Datum auswählt.

Das **windowsxamlhost** -Steuerelement ist jedoch ein generisches Host Steuerelement für *jede* Art von UWP-Steuerelement. Daher wird eine Eigenschaft mit dem Namen **SelectedDates** oder ein Ereignis mit dem Namen **selecteddateschge**nicht verfügbar gemacht, da Sie für das **CalendarView** -Steuerelement spezifisch sind. Um auf diese Member zuzugreifen, müssen Sie Code schreiben, der den **windowsxamlhost** in den **CalendarView** -Typ umwandelt. Der beste Ort hierfür ist die Reaktion auf das **ChildChanged** -Ereignis des Steuer Elements **windowsxamlhost** , das ausgelöst wird, wenn das gehostete Steuerelement gerendert wurde.

1. Fügen Sie in der Datei **addnewexpse. XAML** einen Ereignishandler für das **ChildChanged** -Ereignis des Steuer Elements **windowsxamlhost** hinzu, das Sie zuvor hinzugefügt haben. Wenn Sie dies abgeschlossen haben, sollte das **windowsxamlhost** -Element wie folgt aussehen.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Suchen Sie in derselben Datei das **Grid. RowDefinitions** -Element des Haupt **Rasters**. Fügen Sie ein **RowDefinition** -Element mit der **Höhe** " **Auto** " am Ende der Liste mit untergeordneten Elementen hinzu. Wenn Sie den Vorgang abgeschlossen haben, sollte das **Grid. RowDefinitions** -Element wie folgt aussehen (es sollten nun 9 **RowDefinition** -Elemente vorhanden sein).

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

4. Fügen Sie den folgenden XAML-Code nach dem **windowsxamlhost** -Element und vor dem **Schalt** Flächen Element in der Nähe des Datei Endes hinzu.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Suchen Sie das **Schalt** Flächen Element am Ende der Datei, und ändern Sie die Eigenschaft **Grid. Row** von **7** in **8**. Dadurch wird die Schaltfläche um eine Zeile im Raster nach unten verschoben, da Sie eine neue Zeile hinzugefügt haben.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Öffnen Sie die **AddNewExpense.XAML.cs** -Codedatei.

7. Fügen Sie am Anfang der Datei die folgende-Anweisung hinzu.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Fügen Sie der `AddNewExpense`-Klasse den folgenden Ereignishandler hinzu. Mit diesem Code wird das **ChildChanged** -Ereignis des Steuer Elements **windowsxamlhost** implementiert, das Sie zuvor in der XAML-Datei deklariert haben.

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

    Dieser Code verwendet die **Child** -Eigenschaft des **windowsxamlhost** -Steuer Elements, um auf das UWP **CalendarView** -Steuerelement zuzugreifen. Der Code abonniert dann das **selecteddateschangi-** Ereignis, das ausgelöst wird, wenn der Benutzer ein Datum aus dem Kalender auswählt. Dieser Ereignishandler übergibt das ausgewählte Datum an das ViewModel-Objekt, in dem das neue **Ausgaben** Objekt erstellt und in der Datenbank gespeichert wird. Zu diesem Zweck verwendet der Code die Messaging-Infrastruktur, die vom MVVM Light-nuget-Paket bereitgestellt wird. Der Code sendet eine Nachricht mit dem Namen **selecteddatemess** an das ViewModel, das Sie empfängt und die **Date** -Eigenschaft mit dem ausgewählten Wert festgelegt. In diesem Szenario wird das **CalendarView** -Steuerelement für den Einzel Auswahlmodus konfiguriert, sodass die Auflistung nur ein Element enthält.

    An diesem Punkt wird das Projekt immer noch nicht kompiliert, weil die Implementierung für die **selecteddatemess** -Klasse fehlt. In den folgenden Schritten wird diese Klasse implementiert.

9. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Messages** , und wählen Sie **Add-> Class**aus. Nennen Sie die neue Klasse **selecteddatemess** , und klicken Sie auf **Hinzufügen**.

10. Ersetzen Sie den Inhalt der **SelectedDateMessage.cs** -Codedatei durch den folgenden Code. Mit diesem Code wird eine using-Anweisung `GalaSoft.MvvmLight.Messaging` für den-Namespace (aus dem MVVM Light-nuget-Paket) hinzugefügt, von der **messagebase** -Klasse geerbt und die **DateTime** -Eigenschaft hinzugefügt, die über den öffentlichen-Konstruktor initialisiert wird. Wenn Sie den Vorgang abgeschlossen haben, sollte die Datei wie folgt aussehen.

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

11. Als nächstes aktualisieren Sie das ViewModel, um diese Meldung zu empfangen und die **Date** -Eigenschaft von ViewModel aufzufüllen. Erweitern Sie in **Projektmappen-Explorer**den Ordner **ViewModels** , und öffnen Sie die Datei **AddNewExpenseViewModel.cs** .

12. Suchen Sie den öffentlichen Konstruktor für `AddNewExpenseViewModel` die Klasse, und fügen Sie am Ende des Konstruktors den folgenden Code hinzu. Dieser Code registriert sich für den Empfang von **selecteddatemess**, extrahiert das ausgewählte Datum über die **SelectedDate** -Eigenschaft, und wir verwenden dieses, um die **Date** -Eigenschaft festzulegen, die von ViewModel verfügbar gemacht wird. Da diese Eigenschaft mit dem **TextBlock** -Steuerelement gebunden ist, das Sie zuvor hinzugefügt haben, können Sie das vom Benutzer ausgewählte Datum sehen.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Wenn Sie den Vorgang abgeschlossen haben `AddNewExpenseViewModel` , sollte der Konstruktor wie folgt aussehen. 

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
    > Mit `SaveExpenseCommand` der-Methode in derselben Codedatei werden die Ausgaben in der Datenbank gespeichert. Diese Methode verwendet die **Date** -Eigenschaft, um das neue **Ausgaben** Objekt zu erstellen. Diese Eigenschaft wird jetzt durch das **CalendarView** -Steuerelement durch die `SelectedDateMessage` soeben erstellte Nachricht aufgefüllt.

13. Drücken Sie F5, um die APP im Debugger zu erstellen und auszuführen. Wählen Sie einen der verfügbaren Mitarbeiter aus, und klicken Sie dann auf die Schaltfläche **neue Ausgaben hinzufügen** . Vervollständigen Sie alle Felder im Formular, und wählen Sie ein Datum aus dem neuen **CalendarView** -Steuerelement aus. Vergewissern Sie sich, dass das ausgewählte Datum als Zeichenfolge unter dem Steuerelement angezeigt wird.

14. Klicken Sie auf die Schaltfläche **Speichern** . Das Formular wird geschlossen, und die neue Kosten werden am Ende der Liste der Ausgaben hinzugefügt. Die erste Spalte mit dem Spesen Datum sollte das Datum sein, das Sie im **CalendarView** -Steuerelement ausgewählt haben.

## <a name="next-steps"></a>Nächste Schritte

An dieser Stelle im Tutorial haben Sie ein WPF-Datum/Uhrzeit-Steuerelement erfolgreich durch das UWP **CalendarView** -Steuerelement ersetzt, das neben Maus-und Tastatureingaben auch Finger Eingaben und digitale Stifte unterstützt. Obwohl das Windows Community Toolkit keine umschließende Version des UWP **CalendarView** -Steuer Elements bereitstellt, das direkt in einer WPF-App verwendet werden kann, konnten Sie das Steuerelement mit dem generischen [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement hosten.

Sie sind jetzt bereit für [Teil 4: Fügen Sie Windows 10-Benutzeraktivitäten](modernize-wpf-tutorial-4.md)und-Benachrichtigungen hinzu.
