---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: Hinzufügen eines UWP-CalendarView-Steuerelements, das mithilfe von XAML-Inseln
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420114"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Teil 3: Hinzufügen eines UWP-CalendarView-Steuerelements, das mithilfe von XAML-Inseln

Dies ist der dritte Teil eines Tutorials, die zeigt, wie Sie eine Beispiel WPF-desktop-app mit dem Namen Contoso-Ausgaben zu modernisieren. Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-app, finden Sie unter [Lernprogramm: Modernisieren von WPF-app](modernize-wpf-tutorial.md). In diesem Artikel wird vorausgesetzt, Sie haben bereits abgeschlossen [Teil 2](modernize-wpf-tutorial-2.md).

Das Contoso-Entwicklungsteam möchte, im fiktiven Szenario in diesem Tutorial wählen Sie das Datum für einen Ausgabenbericht auf einem Touchscreen-Gerät zu vereinfachen. In diesem Teil des Lernprogramms fügen Sie eine UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) Steuerelement an die app. Dies ist das gleiche Steuerelement, das in der Windows 10-Funktionen für Datum und Uhrzeit auf der Taskleiste verwendet wird.

![CalendarViewControl-Bild](images/wpf-modernize-tutorial/CalendarViewControl.png)

Im Gegensatz zu den **InkCanvas** steuern, den Sie in [Teil 2](modernize-wpf-tutorial-2.md), das Windows-Community-Toolkit stellt keine umschlossene Version der UWP **CalendarView** , die in WPF-apps verwendet werden kann . Als Alternative können Sie Hosten einer **InkCanvas** in der allgemeinen [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Steuerelement. Sie können dieses Steuerelement verwenden, um alle UWP-Steuerelemente bereitgestellt, die vom Hersteller Plattform oder a 3rd-Party hosten. Die **WindowsXamlHost** Steuerelement erfolgt über die `Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet-Paket. Dieses Paket ist im Lieferumfang der `Microsoft.Toolkit.Wpf.UI.Controls` NuGet-Paket, das Sie in installiert [Teil 2](modernize-wpf-tutorial-2.md).

Zum Verwenden der **WindowsXamlHost** -Steuerelement, müssen Sie die WinRT-APIs direkt aus Code in der WPF-app aufrufen. Die `Microsoft.Windows.SDK.Contracts` NuGet-Paket enthält die Verweise notwendig, um Sie zum Aufrufen von WinRT-APIs aus der app zu ermöglichen. Dieses Paket ist auch im enthalten die `Microsoft.Toolkit.Wpf.UI.Controls` NuGet-Paket, das Sie in installiert [Teil 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Fügen Sie das Steuerelement WindowsXamlHost

1. In **Projektmappen-Explorer**, erweitern Sie die **Ansichten** Ordner in der **ContosoExpenses.Core** Projekt, und doppelklicken Sie auf die **AddNewExpense.xaml**Datei. Dies ist das Formular verwendet, um eine neue Ausgabe zur Liste hinzuzufügen. Hier ist, wie sie in der aktuellen Version der app angezeigt wird.

    ![Fügen Sie neue Ausgaben hinzu.](images/wpf-modernize-tutorial/AddNewExpense.png)

    Datumsauswahl-Steuerelement enthalten, die in WPF ist für herkömmliche Computer mit Maus und Tastatur vorgesehen. Auswählen eines Datums mit einem Touchscreen ist, nicht wirklich sinnvoll, aufgrund der kleinen Größe des Steuerelements und der begrenzten Platz zwischen jeden Tag im Kalender.

2. Am oberen Rand der **AddNewExpense.xaml** hinzufügen. das folgende Attribut hinzu der **Fenster** Element.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Nach dem Hinzufügen dieses Attributs, das **Fenster** -Element sollte nun wie folgt aussehen.

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

3. Ändern der **Höhe** Attribut der **Fenster** Element von 450 auf 800. Dies ist erforderlich, da die UWP **CalendarView** mehr Speicherplatz als der WPF-Datumsauswahl die Kontrolle übernimmt.

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

4. Suchen Sie die `DatePicker` Element im unteren Bereich der Datei, und Ersetzen Sie dieses Element durch das folgende XAML.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Dieses XAML fügt die **WindowsXamlHost** Steuerelement. Die **InitialTypeName** Eigenschaft gibt an, der vollständige Name des UWP-Steuerelements, die Sie hosten möchten (in diesem Fall **Windows.UI.Xaml.Controls.CalendarView**).

5. Drücken Sie F5, um die app im Debugger ausführen. Wählen Sie einen Mitarbeiter aus der Liste aus, und drücken Sie dann die **Hinzufügen von neuen Ausgaben** Schaltfläche. Vergewissern Sie sich, dass die folgende Seite auf die neue UWP hostet **CalendarView** Steuerelement.

    ![CalendarView-wrapper](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Schließen Sie die App.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagieren Sie mit dem WindowsXamlHost-Steuerelement

Als Nächstes aktualisieren Sie die app, um das ausgewählte Datum zu verarbeiten, auf dem Bildschirm angezeigt und das Füllen der **Expense** Objekt, das in der Datenbank gespeichert.

Die UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) enthält zwei Member, die für dieses Szenario relevant sind:

- Die **SelectedDates** Eigenschaft enthält das Datum, die vom Benutzer ausgewählt wurde.
- Die **SelectedDatesChanged** Ereignis wird ausgelöst, wenn der Benutzer ein Datum auswählt.

Allerdings die **WindowsXamlHost** Steuerelement ist ein generischer Host-Steuerelement für *alle* der UWP-Steuerelement. Daher keine Eigenschaft namens offengelegt **SelectedDates** oder ein Ereignis namens **SelectedDatesChanged**, da sie von spezifisch sind die **CalendarView** Steuerelement. Um auf diese Member zuzugreifen, müssen Sie Code, in den umgewandelt schreiben die **WindowsXamlHost** auf die **CalendarView** Typ. Der beste Ort zum hierfür erfolgt als Reaktion auf die **ChildChanged** Ereignis die **WindowsXamlHost** -Steuerelement, das ausgelöst wird, wenn das gehostete Steuerelement gerendert wurde.

1. In der **AddNewExpense.xaml** -Datei, einen Ereignishandler hinzufügen für die **ChildChanged** Ereignis die **WindowsXamlHost** Kontrolle, die Sie zuvor hinzugefügt haben. Wenn Sie fertig sind, die **WindowsXamlHost** -Element sollte wie folgt aussehen.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Suchen Sie in der gleichen Datei die **Grid.RowDefinitions** Element der Haupt- **Raster**. Fügen Sie eine weitere **RowDefinition** -Element mit dem **Höhe** gleich **automatisch** am Ende der Liste der untergeordneten Elemente. Wenn Sie fertig sind, die **Grid.RowDefinitions** -Element sollte wie folgt aussehen (es sollte nun 9 werden **RowDefinition** Elemente).

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

4. Der folgende XAML nach dem Hinzufügen der **WindowsXamlHost** Element und vor der **Schaltfläche** Element am Ende der Datei.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Suchen Sie die **Schaltfläche** Element am Ende der Datei, und Ändern der **Grid.Row** Eigenschaft **7** zu **8**. Dies verlagert die Schaltfläche eine Zeile im Raster nach unten, da Sie eine neue Zeile hinzugefügt.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Öffnen der **AddNewExpense.xaml.cs** Codedatei.

7. Fügen Sie die folgende Anweisung am Anfang der Datei.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Fügen Sie den folgenden Ereignishandler, um die `AddNewExpense` Klasse. Dieser Code implementiert die **ChildChanged** Ereignis die **WindowsXamlHost** Kontrolle, die Sie zuvor in der XAML-Datei deklariert.

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

    Dieser Code verwendet die **untergeordneten** Eigenschaft der **WindowsXamlHost** Steuerelement auf der UWP **CalendarView** Steuerelement. Der Code abonniert dann den **SelectedDatesChanged** -Ereignis, das ausgelöst wird, wenn der Benutzer ein Datum aus dem Kalender auswählt. Dieser Ereignishandler übergibt das ausgewählte Datum, an dem "ViewModel", in dem die neue **Expense** Objekt erstellt und in der Datenbank gespeichert. Zu diesem Zweck werden im Code die messaging-Infrastruktur von MVVM Light-NuGet-Paket bereitgestellt. Der Code sendet eine Nachricht namens **SelectedDateMessage** an das ViewModel wird dem empfangen und Festlegen der **Datum** Eigenschaft mit dem ausgewählten Wert. In diesem Szenario die **CalendarView** Steuerelement für Einzelauswahl-Modus konfiguriert ist, damit die Auflistung nur ein Element enthalten soll.

    An diesem Punkt das Projekt dennoch wird nicht kompiliert, da die Implementierung für fehlt die **SelectedDateMessage** Klasse. Die folgenden Schritte aus dieser Klasse zu implementieren.

9. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Nachrichten** Ordner, und wählen Sie **hinzufügen ->-Klasse**. Nennen Sie die neue Klasse **SelectedDateMessage** , und klicken Sie auf **hinzufügen**.

10. Ersetzen Sie den Inhalt von der **SelectedDateMessage.cs** Codedatei mit dem folgenden Code. Dieser Code Fügt eine using-Anweisung für die `GalaSoft.MvvmLight.Messaging` Namespace (in der MVVM Light-NuGet-Paket), erbt die **MessageBase** Klasse, und fügt **"DateTime"** -Eigenschaft, die über initialisiert wird die öffentlicher Konstruktor. Wenn Sie fertig sind, sollte die Datei wie folgt aussehen.

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

11. Als Nächstes aktualisieren Sie das "ViewModel", um diese Meldung wird angezeigt, und füllen Sie die **Datum** Eigenschaft von "ViewModel". In **Projektmappen-Explorer**, erweitern Sie die **ViewModels** Ordner, und Öffnen der **AddNewExpenseViewModel.cs** Datei.

12. Suchen Sie den öffentlichen Konstruktor für die `AddNewExpenseViewModel` Klasse und fügen Sie den folgenden Code am Ende des Konstruktors. Dieser Code registriert wird, zum Empfangen der **SelectedDateMessage**, extrahieren Sie das ausgewählte Datum aus über die **"SelectedDate"** -Eigenschaft, und wir eine Verwendung zur Festlegung der **Datum** Eigenschaft durch das "ViewModel" verfügbar gemacht. Da diese Eigenschaft gebunden ist, mit der **TextBlock** Kontrolle, die Sie zuvor hinzugefügt haben, sehen Sie das Datum, die vom Benutzer ausgewählt wurde.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Wenn Sie fertig sind, die `AddNewExpenseViewModel` Konstruktor sollte wie folgt aussehen. 

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
    > Die `SaveExpenseCommand` -Methode in derselben Codedatei übernimmt das die Ausgaben in der Datenbank speichern. Diese Methode verwendet die **Datum** Eigenschaft zum Erstellen des neuen **Expense** Objekt. Diese Eigenschaft wird jetzt aufgefüllt wird, indem die **CalendarView** steuern, über die `SelectedDateMessage` Meldung, die Sie gerade erstellt.

13. Drücken Sie F5, um die app im Debugger ausführen. Wählen Sie eine der verfügbaren Mitarbeiter, und klicken Sie dann auf die **Hinzufügen von neuen Ausgaben** Schaltfläche. Füllen Sie alle Felder im Formular aus, und wählen Sie ein Datum aus dem neuen **CalendarView** Steuerelement. Vergewissern Sie sich, dass das ausgewählte Datum als Zeichenfolge unterhalb des Steuerelements angezeigt wird.

14. Drücken Sie die **speichern** Schaltfläche. Das Formular wird geschlossen, und die neue Ausgabe wird an das Ende der Liste der Ausgaben hinzugefügt. Die erste Spalte mit dem Expense-Datum muss das Datum, die Sie, in ausgewählt der **CalendarView** Steuerelement.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt in diesem Tutorial, Sie haben erfolgreich ersetzt ein WPF-Datum-Uhrzeit-Steuerelement mit der UWP **CalendarView** -Steuerelement, das Fingereingabe und digitale Stifte zusätzlich zu Maus- und Tastatureingaben unterstützt. Zwar enthält das Windows-Community-Toolkit eine umschlossene Version der UWP keine **CalendarView** -Steuerelement, das direkt in einer WPF-app verwendet werden kann zum Hosten des Steuerelements mithilfe der allgemeinen konnten [WindowsXamlHost ](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) Steuerelement.

Sie sind nun bereit zur [Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und Benachrichtigungen](modernize-wpf-tutorial-4.md).
