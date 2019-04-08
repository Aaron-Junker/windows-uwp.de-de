---
title: Erstellen einer Kundendatenbankanwendung
description: Erstellen Sie eine datenbankanwendung Kunden, und erfahren Sie, wie grundlegende Enterprise-app-Funktionen zu implementieren.
keywords: Tutorial, Enterprise, Kunde, die Daten, erstellen, lesen, aktualisieren-löschen, REST,-Authentifizierung
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 9c09e0fb73e42fd8a3d0c70bbb5396be32624387
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623245"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutorial: Erstellen einer Kundendatenbankanwendung

In diesem Tutorial wird eine einfache app zum Verwalten einer Liste von Kunden erstellt. In diesem Fall führt es eine Auswahl von grundlegenden Konzepte für Unternehmens-apps in UWP. Sie erfahren Folgendes:

* Implementieren von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen für eine lokale SQL-Datenbank.
* Fügen Sie einem Datenraster, zum Anzeigen und Bearbeiten von Kundendaten in der Benutzeroberfläche hinzu.
* Anordnen von Elementen der Benutzeroberfläche in ein einfaches Formularlayout zusammen.

Der Ausgangspunkt für dieses Tutorial ist eine Einzelseiten-app mit minimalen Benutzeroberfläche und Funktionen, basierend auf einer vereinfachten Version des der [Auftragsdatenbank für die Customer-Beispiel-app](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Es hat C# und XAML, und wir erwarten, dass Sie über grundlegende Kenntnisse von diesen beiden Sprachen verfügen.

![Die Hauptseite der app arbeiten](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Voraussetzungen

* [Stellen Sie sicher, dass Sie die neueste Version von Visual Studio und das Windows 10 SDK verfügen](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Klonen Sie oder Laden Sie das Lernprogramm für die Customer-Beispiel](https://aka.ms/customer-database-tutorial)

Nachdem Sie geklont/Repository heruntergeladen haben, können Sie das Projekt bearbeiten, indem Sie öffnen **CustomerDatabaseTutorial.sln** mit Visual Studio.

> [!NOTE]
> Sehen Sie sich die [vollständigen Customer Orders-Beispieldatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) um die app anzuzeigen, in diesem Tutorial wurde basierend auf.

## <a name="part-1-code-of-interest"></a>Teil 1: Relevanten Code

Wenn Sie Ihre app unmittelbar nach dem öffnen es ausführen, sehen Sie einige Schaltflächen am oberen Rand einen leeren Bildschirm. Obwohl es nicht für Sie sichtbar ist, enthält die app bereits eine lokale SQLite-Datenbank, die mit wenigen Tests Kunden bereitgestellt. Von hier aus Sie zunächst ein UI-Steuerelement, sodass diese Kunden angezeigt, und klicken Sie dann auf Hinzufügen, die in Vorgänge in der Datenbank verschieben. Bevor Sie beginnen, ist hier, in dem Sie arbeiten werden.

### <a name="views"></a>Ansichten

**CustomerListPage.xaml** der Ansicht der app, die die Benutzeroberfläche für eine einzelne Seite in diesem Tutorial definiert ist. Jedes Mal, wenn müssen Sie zum Hinzufügen oder ändern ein visuelles Element auf der Benutzeroberfläche werden Sie es in dieser Datei ausführen. Dieses Tutorial führt Sie durch das Hinzufügen dieser Elemente:

* Ein **RadDataGrid** zum Anzeigen und bearbeiten Ihre Kunden. 
* Ein **StackPanel** die Anfangswerte für einen neuen Kunden festgelegt.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs** ist, in denen die grundlegende Logik der app befindet. Jede Benutzeraktion, die in der Ansicht ausgeführt werden in diese Datei für die Verarbeitung übergeben. In diesem Tutorial haben Sie fügen Sie neuen Code hinzu, und implementieren Sie die folgenden Methoden:

* **CreateNewCustomerAsync**, die ein neues CustomerViewModel Objekt initialisiert.
* **DeleteNewCustomerAsync**, das einen neuen Kunden entfernt, bevor sie in der Benutzeroberfläche angezeigt wird.
* **DeleteAndUpdateAsync**, die Schaltfläche zum Löschen der Logik behandelt.
* **GetCustomerListAsync**, die eine Liste von Kunden aus der Datenbank abruft.
* **SaveInitialChangesAsync**, die der Datenbank einen neuen Kunden Informationen hinzugefügt.
* **UpdateCustomersAsync**, die die Benutzeroberfläche hinzufügen oder Löschen von Kunden entsprechend aktualisiert.

**CustomerViewModel** ist ein Wrapper für die Informationen eines Kunden, die nachverfolgt, und zwar unabhängig davon, ob er zuletzt geändert wurde. Sie müssen nicht alle Elemente dieser Klasse hinzugefügt, jedoch einen Teil des Codes, die Sie an anderer Stelle hinzufügen, wird darauf verweisen.

Weitere Informationen über den Aufbau des Beispiels sehen Sie sich die [Übersicht über app-Struktur](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Teil 2: Fügen Sie das DataGrid-Steuerelement hinzu

Bevor Sie beginnen mit Kundendaten arbeiten, müssen Sie ein UI-Steuerelement zum Anzeigen von Kunden hinzufügen. Zu diesem Zweck, wir verwenden einen vorgefertigtes Drittanbieter- **RadDataGrid** Steuerelement. Die **Telerik.UI.for.UniversalWindowsPlatform** NuGet-Paket ist bereits in diesem Projekt enthalten. Fügen Sie im Raster auf das Projekt ein.

1. Open **Views\CustomerListPage.xaml** Projektmappen-Explorer. Fügen Sie die folgende Codezeile in der **Seite** Tag, um eine Zuordnung zu der Telerik-Namespace, enthält das Datenraster zu deklarieren.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Unterhalb der **CommandBar** im des Haupt- **"relativepanel"** fügen Sie der Ansicht eine **RadDataGrid** -Steuerelement, mit einigen grundlegenden Konfigurationsoptionen:

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. Sie haben das Datenraster hinzugefügt, aber die anzuzeigenden Daten benötigt. Fügen Sie darin die folgenden Codezeilen hinzu:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Nun, da Sie eine Datenquelle angezeigt werden, definiert haben **RadDataGrid** behandelt den größten Teil der UI-Logik für Sie. Aber wenn Sie das Projekt ausführen, wird nicht weiterhin keine Daten auf Anzeige angezeigt. Dies liegt daran "ViewModel" noch beim Laden nicht ist.

![Leere app, die keine Kunden](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Teil 3: Kunden lesen

Wenn es initialisiert wird, **ViewModels\CustomerListPageViewModel.cs** Aufrufe der **GetCustomerListAsync** Methode. In diesem Tutorial ist, dass die Anforderungen der Methode zum Abrufen von des Tests von Kundendaten aus der SQLite-Datenbank enthalten.

1. In **ViewModels\CustomerListPageViewModel.cs**, aktualisieren Sie Ihre **GetCustomerListAsync** Methode durch den folgenden Code:

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    Die **GetCustomerListAsync** Methode wird aufgerufen, wenn das "ViewModel" wird geladen, aber vor diesem Schritt haben sie nicht sonderlich. Wir haben hier hinzugefügt, einen Aufruf der **"getasync"** -Methode in der **Repository/SqlCustomerRepository**. Dadurch kann er, wenden Sie sich an das Repository um eine aufzählbare Auflistung von Customer-Objekten abzurufen. Es analysiert dann diese einzelne Objekte, bevor sie Sie der internen hinzufügen **ObservableCollection** , damit sie angezeigt und bearbeitet werden können.

2. Führen Sie die app - wird jetzt das Datenraster mit einer Liste der Kunden angezeigt.

![Ursprüngliche Liste von Kunden](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Teil 4: Bearbeiten von Kunden

Sie können die Einträge im Raster bearbeiten, indem Sie darauf doppelklicken, aber Sie müssen sicherstellen, dass alle Änderungen, die Sie in der Benutzeroberfläche treffen auch auf die Auflistung von Kunden in der CodeBehind-vorgenommen werden. Dies bedeutet, dass müssen Sie die bidirektionale Datenbindung zu implementieren. Wenn Sie weitere Informationen zu diesem möchten, sehen Sie sich unsere [Einführung in die Datenbindung](../get-started/display-customers-in-list-learning-track.md).

1. Zuerst deklarieren, die **ViewModels\CustomerListPageViewModel.cs** implementiert die **"INotifyPropertyChanged"** Schnittstelle:

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Fügen Sie dann in den Hauptteil der-Klasse, die folgenden Methoden und -Ereignis:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    Die **OnPropertyChanged** Methode erleichtert es Ihrem Setter zum Auslösen der **PropertyChanged** -Ereignis, das für die bidirektionale Datenbindung erforderlich ist.

3. Aktualisieren Sie den Setter für **SelectedCustomer** mit dieser Funktionsaufruf:

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. In **Views\CustomerListPage.xaml**, Hinzufügen der **SelectedCustomer** Eigenschaft, um Ihre Datenraster.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Dadurch wird die Auswahl des Benutzers im Datenraster mit der entsprechenden Customer-Objekt im Code-Behind verknüpft. Die TwoWay-Bindung-Modus können die Änderungen, die in der Benutzeroberfläche für das Objekt berücksichtigt werden.

5. Führen Sie Ihre app ein. Sie können jetzt finden Sie unter der Kunden, die im Raster angezeigt, und nehmen Sie Änderungen an der zugrunde liegenden Daten über die Benutzeroberfläche.

![Ein Kunde im Raster bearbeiten](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Teil 5: Aktualisieren von Kunden

Nun können Sie finden Sie unter und Ihre Kunden bearbeiten, müssen Sie in der Lage, Ihre Änderungen in der Datenbank mithilfe von Push übertragen, und um Updates abzurufen, die von anderen Benutzern vorgenommen wurden.

1. Wechseln Sie zurück zur **ViewModels\CustomerListPageViewModel.cs**, und navigieren Sie zu der **UpdateCustomersAsync** Methode. Mit diesem Code Änderungen in der Datenbank zu übertragen und Abrufen von neuen Informationen aktualisieren:

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    Dieser Code nutzt die **IsModified** Eigenschaft **ViewModels\CustomerViewModel.cs**, die wird automatisch aktualisiert, wenn der Kunde geändert wird. Dadurch können Sie unnötige Aufrufe zu vermeiden und nur die Änderungen von aktualisierten Kunden an die Datenbank übertragen.

## <a name="part-6-create-a-new-customer"></a>Teil 6: Erstellen eines neuen Kunden

Hinzufügen eines neuen Kunden stellt eine Herausforderung dar, wie der Kunde als eine leere Zeile angezeigt wird, wenn Sie es an der Benutzeroberfläche hinzufügen, vor dem Bereitstellen von Werten für die zugehörigen Eigenschaften. Das ist es nicht um ein Problem, aber hier wir müssen machen es einfacher, legen Sie die ursprünglichen Werte eines Kunden. In diesem Tutorial fügen wir einen einfache reduzierbaren Bedienfelder, aber wenn Sie weitere Informationen hinzugefügt haben könnten Sie eine separate Seite für diesen Zweck erstellen.

### <a name="update-the-code-behind"></a>Das Code-Behind zu aktualisieren

1. Hinzufügen eines neues private-Feld und eine öffentliche Eigenschaft, die **ViewModels\CustomerListPageViewModel.cs**. Dies wird verwendet, um zu steuern, ob der Bereich sichtbar ist.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. Fügen Sie eine neue öffentliche Eigenschaft, an das ViewModel eine Umkehrung der Wert des **AddingNewCustomer**. Hiermit wird die reguläre Schaltflächen auf der Befehlsleiste zu deaktivieren, wenn der Bereich sichtbar ist.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Sie benötigen jetzt eine Möglichkeit, die reduzierbare angezeigt, und klicken Sie zum Erstellen eines Kunden aus, um darin zu bearbeiten. 

3. Fügen Sie eine neue private Fiend und eine öffentliche Eigenschaft, an das ViewModel, die den neu erstellten Kunden enthalten soll.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if {_newCustomer != value}
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Aktualisieren Ihrer **CreateNewCustomerAsync** Methode, um einen neuen Kunden erstellen, fügen ihn in das Repository und legen Sie es als den ausgewählten Kunden:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Aktualisieren der **SaveInitialChangesAsync** Methode, um einen neu erstellten Kunden an das Repository hinzuzufügen. die Benutzeroberfläche aktualisiert, und schließen Sie den Zugriffsbereich.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Fügen Sie die folgende Zeile des Codes als die letzte Zeile in der Setter **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Dies stellt sicher, dass **EnableCommandBar** wird automatisch aktualisiert, wenn **AddingNewCustomer** geändert wird.

### <a name="update-the-ui"></a>Die Benutzeroberfläche aktualisiert

1. Navigieren Sie zurück zur **Views\CustomerListPage.xaml**, und fügen eine **StackPanel** mit den folgenden Eigenschaften zwischen Ihrem **CommandBar** und Ihre Daten:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    Die **X: Load** Attribut wird sichergestellt, dass in diesem Bereich wird nur angezeigt, wenn Sie einen neuen Kunden hinzufügen.

2. Nehmen Sie folgende Änderung an der Position des Ihre Datenraster, um sicherzustellen, dass sie nach unten verschoben wird, wenn der neue Bereich angezeigt wird:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Aktualisieren Sie Ihre StackPanel-Element mit vier **Textfeld** Steuerelemente. Sie werden an die einzelnen Eigenschaften der neuen Kundenabonnements binden und ermöglichen es Ihnen, ihre Werte zu bearbeiten, bevor Sie ihn in das Datenraster hinzufügen.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. Fügen Sie einer einfachen Schaltfläche, auf Ihre neuen StackPanel-Element, um den neu erstellten Kunden zu speichern:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Aktualisieren der **CommandBar**, sodass die regulären erstellen, DELETE- und Update-Schaltflächen deaktiviert sind, wenn das StackPanel-Element angezeigt wird:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Führen Sie Ihre app ein. Sie können jetzt einen Kunden zu erstellen und geben Sie die Daten auf den StackPanel-Element.

![Erstellen ein neues Kunden](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Teil 7: Löschen Sie ein Kunden

Löschen eines Kunden ist die endgültige grundlegende Funktionsweise, die Sie implementieren müssen. Wenn Sie keinen Kunden Sie im Datenraster ausgewählt haben löschen, sollten Sie sofort aufrufen **UpdateCustomersAsync** um die Benutzeroberfläche zu aktualisieren. Allerdings müssen Sie diese Methode aufrufen, wenn Sie einen Kunden löschen, die, den Sie soeben erstellt haben.

1. Navigieren Sie zu **ViewModels\CustomerListPageViewModel.cs**, und aktualisieren Sie die **DeleteAndUpdateAsync** Methode:

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. In **Views\CustomerListPage.xaml**, aktualisieren Sie das StackPanel-Element für einen neuen Kunden hinzufügen, sodass sie eine zweite Schaltfläche enthält:

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. In **ViewModels\CustomerListPageViewModel.cs**, aktualisieren Sie die **DeleteNewCustomerAsync** Methode, um den neuen Kunden zu löschen:

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. Führen Sie Ihre app ein. Sie können jetzt Kunden, die entweder im Datenraster oder im StackPanel-Element löschen.

![Löschen Sie einen neuen Kunden](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Abschluss

Gratulation! Mit all dies erfolgt hat Ihre app jetzt eine Vielzahl von Vorgängen der lokalen Datenbank. Sie erstellen, lesen, aktualisieren und Löschen von Kunden innerhalb Ihrer Benutzeroberfläche, und diese Änderungen werden in der Datenbank gespeichert und bleiben für verschiedene Start Ihrer App.

Nachdem Sie fertig sind, können Sie Folgendes:
* Wenn Sie nicht bereits getan haben, sehen Sie sich die [Übersicht über app-Struktur](../enterprise/customer-database-app-structure.md) für Weitere Informationen darüber, warum ist die app erstellt, es ist.
* Untersuchen der [vollständigen Customer Orders-Beispieldatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) um die app anzuzeigen, in diesem Tutorial wurde basierend auf.

Oder wenn Sie für eine Herausforderung darstellen möchten, können Sie weiter, oder höher...

## <a name="going-further-connect-to-a-remote-database"></a>Weiterführende Themen: Verbinden Sie mit einer Remotedatenbank

Wir haben eine schrittweise Anleitung zum Implementieren dieser Aufrufe mit einer lokalen SQLite-Datenbank bereitgestellt. Aber was geschieht, wenn Sie stattdessen eine Remotedatenbank, möchten?

Wenn Sie dies auszuprobieren möchten, benötigen Sie Ihr eigenes Konto für Azure Active Directory (AAD) und die Möglichkeit, eine eigene Datenquelle hosten.

Sie müssen zum Hinzufügen von Funktionen für die REST-Aufrufe zu behandeln, und erstellen Sie dann eine Remotedatenbank für die Interaktion mit Authentifizierung. Code vorhanden ist, in der vollständigen [Customer Orders-Datenbankbeispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) , die Sie für jeden erforderlichen Vorgang verweisen können.

### <a name="settings-and-configuration"></a>Einstellungen und Konfigurationen

Die erforderlichen Schritte für die Verbindung mit Ihren eigenen Remotedatenbank geschrieben ist der [-Infodatei des Beispiels](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Sie müssen die folgenden Schritte ausführen:

* Geben Sie Ihre Client-Id des Azure-Konto zu ["Constants.cs"](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Geben Sie die Url der Remotedatenbank ["Constants.cs"](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Geben Sie die Verbindungszeichenfolge für die Datenbank ["Constants.cs"](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Verknüpfen Sie Ihre app mit dem Microsoft Store.
* Kopieren Sie die [Dienstprojekt](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) zu Ihrer app, und in Azure bereitstellen.

### <a name="authentication"></a>Authentication

Sie müssen zum Erstellen einer Schaltfläche, um eine Authentifizierungssequenz ist, und ein Popupfenster oder eine separate Seite zum Sammeln von Benutzerinformationen zu starten. Nachdem Sie die, die erstellt haben, müssen Sie Code bereitstellen, die Anforderungen der Informationen eines Benutzers und wird verwendet, um ein Zugriffstoken abzurufen. Der Kunde Bestellungen-Datenbankbeispiel dient als Wrapper für Microsoft Graph-Aufrufe mit dem **Web Account Manager** Bibliothek, um ein Token abrufen und verarbeiten die Authentifizierung mit einem AAD-Konto.

* Ist die Authentifizierungslogik in implementiert [ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* Der Authentifizierungsprozess wird angezeigt, in der benutzerdefinierten [ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) Steuerelement.

### <a name="rest-calls"></a>REST-Aufrufe

Sie müssen nicht ändern des Codes wir hinzugefügt, in diesem Tutorial um REST-Aufrufe zu implementieren. Stattdessen müssen Sie die folgenden Schritte ausführen:

* Erstellen von neuen Implementierungen von der **ICustomerRepository** und **ITutorialRepository** Schnittstellen, die den gleichen Satz von Funktionen über REST anstelle von SQLite zu implementieren. Sie müssen zum Serialisieren und Deserialisieren von JSON und können Ihre REST-Aufrufe in einer separaten umschließen **HttpHelper** Klasse, wenn Sie möchten. Finden Sie unter [das vollständige Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) Einzelheiten.
* In **"App.Xaml.cs"**, erstellen Sie eine neue Funktion, um die REST-Repository zu initialisieren und rufen sie anstelle von **SqliteDatabase** beim Initialisieren der app. In diesem Fall finden Sie unter [das vollständige Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Nachdem Sie alle drei Schritte abgeschlossen haben, sollten Sie Ihre AAD-Kontos über Ihre app authentifizieren können. REST-Aufrufe an die Remotedatenbank ersetzt die lokalen SQLite-Aufrufe, aber die Benutzeroberfläche sollte identisch sein. Wenn Sie besonders ambitioniert fühlen, können Sie eine Seite "Einstellungen", damit der Benutzer dynamisch zwischen den beiden wechseln kann hinzufügen.