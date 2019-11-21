---
title: Erstellen einer Kundendatenbankanwendung
description: Erstellen Sie eine Kundendaten Bank Anwendung, und erfahren Sie, wie Sie grundlegende Funktionen für Unternehmensanwendungen implementieren.
keywords: Enterprise, Tutorial, Kunde, Daten, Erstellung von Lese Updates löschen, Rest, Authentifizierung
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258617"
---
# <a name="tutorial-create-a-customer-database-application"></a>Lernprogramm: Erstellen einer Kundendatenbankanwendung

In diesem Tutorial wird eine einfache APP zum Verwalten einer Kundenliste erstellt. Dadurch wird eine Auswahl von grundlegenden Konzepten für Unternehmens-apps in UWP eingeführt. Sie erfahren Folgendes:

* Implementieren Sie Erstellungs-, Lese-, Aktualisierungs-und Löschvorgänge für eine lokale SQL-Datenbank.
* Fügen Sie ein Datenraster hinzu, um Kundendaten in der Benutzeroberfläche anzuzeigen und zu bearbeiten.
* Anordnen von UI-Elementen in einem grundlegenden Formularlayout.

Der Ausgangspunkt für dieses Tutorial ist eine einseitige App mit minimaler Benutzeroberfläche und Funktionalität, die auf einer vereinfachten Version der Beispiel- [App der Customer Orders-Datenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database)basiert. Es ist in C# und XAML geschrieben, und wir erwarten, dass Sie eine grundlegende Vertrautheit mit beiden Sprachen haben.

![Die Hauptseite der funktionierenden App](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Voraussetzungen

* [Stellen Sie sicher, dass Sie über die neueste Version von Visual Studio und das Windows 10 SDK verfügen.](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Beispiel zum Klonen oder Herunterladen des Kundendatenbank-Tutorials](https://github.com/microsoft/windows-tutorials-customer-database)

Nachdem Sie das Repository geklont/heruntergeladen haben, können Sie das Projekt bearbeiten, indem Sie **customerdatabasetutorial. sln** in Visual Studio öffnen.

> [!NOTE]
> Sehen Sie sich das [Beispiel der vollständigen Customer Orders-Datenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) an, um die APP anzuzeigen, auf der dieses Tutorial basiert.

## <a name="part-1-code-of-interest"></a>Teil 1: relevanter Code

Wenn Sie die APP sofort nach dem Öffnen Ausführen, werden einige Schaltflächen oben auf einem leeren Bildschirm angezeigt. Obwohl Sie für Sie nicht sichtbar ist, enthält die APP bereits eine lokale SQLite-Datenbank, die mit einigen Testkunden bereitgestellt wird. Von hier aus müssen Sie zunächst ein UI-Steuerelement implementieren, um diese Kunden anzuzeigen, und dann mit dem Hinzufügen von Vorgängen für die Datenbank fortfahren. Bevor Sie beginnen, arbeiten Sie hier.

### <a name="views"></a>Ansichten

**Customerlistpage. XAML** ist die Ansicht der APP, die die Benutzeroberfläche für die einzelne Seite in diesem Tutorial definiert. Jedes Mal, wenn Sie ein visuelles Element in der Benutzeroberfläche hinzufügen oder ändern müssen, führen Sie es in dieser Datei aus. Dieses Tutorial führt Sie durch das Hinzufügen dieser Elemente:

* Ein **raddatagrid** zum Anzeigen und Bearbeiten Ihrer Kunden. 
* Ein **StackPanel** , um die Anfangswerte für einen neuen Kunden festzulegen.

### <a name="viewmodels"></a>ViewModels

" **Viewmodels\customerlistpageviewmodel.cs** " ist der Ort, an dem sich die grundlegende Logik der APP befindet. Jede Benutzeraktion, die in der Ansicht ausgeführt wird, wird zur Verarbeitung an diese Datei übermittelt. In diesem Tutorial fügen Sie neuen Code hinzu und implementieren die folgenden Methoden:

* " **Kreatenewcustomerasync**": Initialisiert ein neues CustomerViewModel-Objekt.
* **Deletenewcustomerasync**, wodurch ein neuer Kunde entfernt wird, bevor er in der Benutzeroberfläche angezeigt wird.
* **Deleteandupdateasync**, der die Logik der Lösch Schaltfläche behandelt.
* **Getcustomerlistasync**: Hiermit wird eine Liste von Kunden aus der Datenbank abgerufen.
* **Saveinitialchangesasync**: Hiermit werden der Datenbank die Informationen eines neuen Kunden hinzugefügt.
* **Updatecustomersasync**, das die Benutzeroberfläche aktualisiert, um alle hinzugefügten oder gelöschten Kunden widerzuspiegeln.

**CustomerViewModel** ist ein Wrapper für die Informationen eines Kunden, der nachverfolgt, ob er vor kurzem geändert wurde. Sie müssen dieser Klasse nichts hinzufügen, aber ein Teil des Codes, den Sie an anderer Stelle hinzufügen, wird darauf verweisen.

Weitere Informationen zur Erstellung des Beispiels finden Sie in der Übersicht über die [App-Struktur](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Teil 2: Hinzufügen des DataGrid

Bevor Sie mit der Arbeit mit Kundendaten beginnen, müssen Sie ein UI-Steuerelement hinzufügen, um diese Kunden anzuzeigen. Zu diesem Zweck verwenden wir ein vorgefertigte **raddatagrid** -Steuerelement von Drittanbietern. Das nuget-Paket **Telerik. UI. for. universalwindowsplatform** ist bereits in diesem Projekt enthalten. Fügen wir das Raster zum Projekt hinzu.

1. Öffnen Sie **views\customerlistpage.XAML** aus der Projektmappen-Explorer. Fügen Sie die folgende Codezeile innerhalb des **Seiten** Tags hinzu, um eine Zuordnung zum Telerik-Namespace zu deklarieren, der das Datenraster enthält.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Fügen Sie unterhalb der **Befehlsleiste** im **hauptrelativepanel** der Ansicht ein **raddatagrid-** Steuerelement mit einigen grundlegenden Konfigurationsoptionen hinzu:

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

3. Sie haben das Datenraster hinzugefügt, es müssen jedoch Daten angezeigt werden. Fügen Sie die folgenden Codezeilen hinzu:

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Nachdem Sie nun eine Datenquelle definiert haben, die angezeigt werden soll, verarbeitet **raddatagrid** den größten Teil der Benutzeroberflächen-Logik. Wenn Sie jedoch das Projekt ausführen, werden Ihnen keine Daten auf dem Bildschirm angezeigt. Das liegt daran, dass das ViewModel es noch nicht lädt.

![Leere APP, ohne Kunden](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Teil 3: Kunden lesen

Wenn es initialisiert ist, ruft **viewmodels\customerlistpageviewmodel.cs** die **getcustomerlistasync** -Methode auf. Diese Methode muss die Testkunden Daten aus der SQLite-Datenbank abrufen, die im Tutorial enthalten ist.

1. Aktualisieren Sie in **viewmodels\customerlistpageviewmodel.cs**die **getcustomerlistasync** -Methode mit dem folgenden Code:

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
    Die **getcustomerlistasync** -Methode wird aufgerufen, wenn das ViewModel geladen wird, aber vor diesem Schritt hat Sie nichts unternommen. Hier haben wir einen Aufrufen der **getasync** -Methode im **Repository/sqlcustomerrepository**hinzugefügt. Dadurch kann eine Verbindung mit dem Repository hergestellt werden, um eine Aufzähl Bare Auflistung von Kunden Objekten abzurufen. Anschließend werden Sie in einzelne Objekte analysiert, bevor Sie zu ihrer internen **ObservableCollection** hinzugefügt werden, sodass Sie angezeigt und bearbeitet werden können.

2. Ausführen der APP: Sie sehen jetzt das Datenraster, in dem die Kundenliste angezeigt wird.

![Anfängliche Kundenliste](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Teil 4: Bearbeiten von Kunden

Sie können die Einträge im Datenraster bearbeiten, indem Sie darauf doppelklicken. Sie müssen jedoch sicherstellen, dass alle Änderungen, die Sie in der Benutzeroberfläche vornehmen, auch an ihrer Kunden Auflistung im Code-Behind vorgenommen werden. Dies bedeutet, dass Sie eine bidirektionale Datenbindung implementieren müssen. Weitere Informationen hierzu finden Sie [in unserer Einführung in die Datenbindung](../get-started/display-customers-in-list-learning-track.md).

1. Deklarieren Sie zunächst, dass **viewmodels\customerlistpageviewmodel.cs** die **INotifyPropertyChanged** -Schnittstelle implementiert:

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Fügen Sie dann im Hauptteil der-Klasse das folgende Ereignis und die folgende Methode hinzu:

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    Mit der **OnPropertyChanged** -Methode können Ihre Setter problemlos das **PropertyChanged** -Ereignis aufrufen, das für die bidirektionale Datenbindung erforderlich ist.

3. Aktualisieren Sie den Setter für **selectedcustomer** mit diesem Funktions aufzurufen:

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

4. Fügen Sie in **views\customerlistpage.XAML**dem Datenraster die **selectedcustomer** -Eigenschaft hinzu.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Dadurch wird die Auswahl des Benutzers im Datenraster mit dem entsprechenden Customer-Objekt im Code Behind verknüpft. Der TwoWay-Bindungs Modus ermöglicht das Wiedergeben der Änderungen, die auf der Benutzeroberfläche vorgenommen werden, für dieses Objekt.

5. Führen Sie die APP aus. Sie können jetzt die Kunden anzeigen, die im Raster angezeigt werden, und die zugrunde liegenden Daten über die Benutzeroberfläche ändern.

![Bearbeiten eines Kunden im Datenraster](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Teil 5: Aktualisieren von Kunden

Nachdem Sie Ihre Kunden nun sehen und bearbeiten können, müssen Sie in der Lage sein, Ihre Änderungen per Push an die Datenbank zu übernehmen und alle von anderen vorgenommenen Updates abzurufen.

1. Kehren Sie zu " **viewmodels\customerlistpageviewmodel.cs**" zurück, und navigieren Sie zur **updatecustomersasync** -Methode. Aktualisieren Sie ihn mit diesem Code, um Änderungen an die Datenbank zu übersetzen und neue Informationen abzurufen:

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
    In diesem Code wird die **IsModified** -Eigenschaft von **viewmodels\customerviewmodel.cs**verwendet. diese wird automatisch aktualisiert, wenn der Kunde geändert wird. Auf diese Weise können Sie unnötige Aufrufe vermeiden und Änderungen von aktualisierten Kunden nur per Push an die Datenbank Übertragung.

## <a name="part-6-create-a-new-customer"></a>Teil 6: Erstellen eines neuen Kunden

Das Hinzufügen eines neuen Kunden stellt eine Herausforderung dar, da der Kunde als leere Zeile angezeigt wird, wenn Sie ihn vor der Angabe von Werten für die Eigenschaften der Benutzeroberfläche hinzufügen. Das ist kein Problem, aber hier vereinfachen wir das Festlegen der Anfangswerte eines Kunden. In diesem Tutorial fügen wir einen einfachen redusible Panel hinzu, aber wenn Sie weitere Informationen zum Hinzufügen hätten, können Sie für diesen Zweck eine separate Seite erstellen.

### <a name="update-the-code-behind"></a>Code Behind aktualisieren

1. Fügen Sie **viewmodels\customerlistpageviewmodel.cs**ein neues privates Feld und eine öffentliche Eigenschaft hinzu. Hiermit wird gesteuert, ob der Panel sichtbar ist.

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

2. Fügen Sie dem ViewModel eine neue öffentliche Eigenschaft hinzu, eine Umkehrung des Werts von **addingnewcustomer**. Diese Option wird verwendet, um die regulären Befehls leisten Schaltflächen zu deaktivieren, wenn der Bereich sichtbar ist.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Sie benötigen nun eine Möglichkeit, den redusible Panel anzuzeigen und einen Kunden zu erstellen, der darin bearbeitet werden kann. 

3. Fügen Sie dem ViewModel eine neue private und öffentliche Eigenschaft hinzu, um den neu erstellten Kunden zu speichern.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Aktualisieren Sie die Methode " **kreatenewcustomerasync** ", um einen neuen Kunden zu erstellen, ihn dem Repository hinzuzufügen und ihn als ausgewählten Kunden festzulegen:

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Aktualisieren Sie die **saveinitialchangesasync** -Methode, um einen neu erstellten Kunden zum Repository hinzuzufügen, aktualisieren Sie die Benutzeroberfläche, und schließen Sie den Bereich.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Fügen Sie die folgende Codezeile als letzte Zeile im Setter für **addingnewcustomer**hinzu:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Dadurch wird sichergestellt, dass **enablecommandbar** immer dann automatisch aktualisiert wird, wenn **addingnewcustomer** geändert wird.

### <a name="update-the-ui"></a>Aktualisieren der Benutzeroberfläche

1. Navigieren Sie zurück zu " **views\customerlistpage.XAML**", und fügen Sie ein **StackPanel** -Element mit den folgenden Eigenschaften zwischen der **Befehlsleiste** und dem Datenraster hinzu:

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    Das Attribut **x:Load** stellt sicher, dass dieser Bereich nur angezeigt wird, wenn Sie einen neuen Kunden hinzufügen.

2. Nehmen Sie die folgende Änderung an der Position des Datenrasters vor, um sicherzustellen, dass es nach unten verschoben wird, wenn der neue Bereich angezeigt wird:

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Aktualisieren Sie das Stapel Panel mit vier **TextBox** -Steuerelementen. Sie werden an die einzelnen Eigenschaften des neuen Kunden gebunden und ermöglichen es Ihnen, die Werte zu bearbeiten, bevor Sie Sie dem Datenraster hinzufügen.

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

4. Fügen Sie dem neuen Stapel Panel eine einfache Schaltfläche hinzu, um den neu erstellten Kunden zu speichern:

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Aktualisieren Sie die **Befehlsleiste**, sodass die regulären Schaltflächen "erstellen", "Löschen" und "Aktualisieren" deaktiviert werden, wenn der Stapel Panel sichtbar ist:

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Führen Sie die APP aus. Sie können jetzt einen Kunden erstellen und die zugehörigen Daten im Stapel Panel eingeben.

![Erstellen eines neuen Kunden](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Teil 7: Löschen eines Kunden

Das Löschen eines Kunden ist der abschließende grundlegende Vorgang, den Sie implementieren müssen. Wenn Sie einen Kunden löschen, den Sie im Datenraster ausgewählt haben, sollten Sie sofort **updatecustomersasync** aufrufen, um die Benutzeroberfläche zu aktualisieren. Sie müssen diese Methode jedoch nicht anrufen, wenn Sie einen Kunden löschen, den Sie gerade erstellt haben.

1. Navigieren Sie zu " **viewmodels\customerlistpageviewmodel.cs**", und aktualisieren Sie die **deleteandupdateasync** -Methode:

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

2. Aktualisieren Sie in **views\customerlistpage.XAML**den Stapelbereich zum Hinzufügen eines neuen Kunden, sodass er eine zweite Schaltfläche enthält:

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

3. Aktualisieren Sie in **viewmodels\customerlistpageviewmodel.cs**die **deletenewcustomerasync** -Methode, um den neuen Kunden zu löschen:

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

4. Führen Sie die APP aus. Sie können jetzt Kunden löschen, entweder innerhalb des Datenrasters oder im Stapel Panel.

![Neuen Kunden löschen](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Abschluss

Gratulation! In diesem Fall verfügt Ihre APP nun über eine vollständige Palette an lokalen Daten Bank Vorgängen. Sie können Kunden innerhalb Ihrer Benutzeroberfläche erstellen, lesen, aktualisieren und löschen, und diese Änderungen werden in der Datenbank gespeichert und werden über verschiedene Starts der APP hinweg beibehalten.

Nachdem Sie nun fertig sind, sollten Sie Folgendes beachten:
* Wenn Sie dies noch nicht getan haben, finden Sie in der Übersicht über die [App-Struktur](../enterprise/customer-database-app-structure.md) Weitere Informationen dazu, warum die APP wie Sie erstellt wird.
* Erkunden Sie das [vollständige Beispiel der Customer Orders-Datenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database) , um die APP anzuzeigen, auf der dieses Tutorial basiert.

Oder wenn Sie eine Herausforderung haben, können Sie fortfahren...

## <a name="going-further-connect-to-a-remote-database"></a>Weitere Informationen: Herstellen einer Verbindung mit einer Remote Datenbank

Wir haben eine schrittweise exemplarische Vorgehensweise zur Implementierung dieser Aufrufe für eine lokale SQLite-Datenbank bereitgestellt. Aber was geschieht, wenn Sie stattdessen eine Remote Datenbank verwenden möchten?

Wenn Sie dies ausprobieren möchten, benötigen Sie ein eigenes Azure Active Directory-Konto (AAD) und die Möglichkeit, ihre eigene Datenquelle zu hosten.

Sie müssen Authentifizierung, Funktionen zum Verarbeiten von Rest-aufrufen und dann eine Remote Datenbank für die Interaktion mit erstellen. Das vollständige [Customer Orders-Daten Bank Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) enthält Code, auf den Sie für jeden erforderlichen Vorgang verweisen können.

### <a name="settings-and-configuration"></a>Einstellungen und Konfiguration

Die erforderlichen Schritte zum Herstellen einer Verbindung mit ihrer eigenen Remote Datenbank sind in der Info Datei des Beispiels [beschrieben.](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md) Sie müssen die folgenden Schritte ausführen:

* Geben Sie die Client-ID Ihres Azure-Kontos für [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)an.
* Geben Sie die URL der Remote Datenbank für [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)an.
* Geben Sie die Verbindungs Zeichenfolge für die [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)-Datenbank an.
* Verknüpfen Sie die APP mit dem Microsoft Store.
* Kopieren Sie das [Dienstprojekt](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) in Ihre APP, und stellen Sie es in Azure bereit.

### <a name="authentication"></a>Authentication

Sie müssen eine Schaltfläche erstellen, um eine Authentifizierungs Sequenz zu starten, sowie ein Popup oder eine separate Seite, um die Informationen eines Benutzers zu erfassen. Nachdem Sie das erstellt haben, müssen Sie Code bereitstellen, der die Informationen eines Benutzers anfordert und ihn zum Abrufen eines Zugriffs Tokens verwendet. Das Customer Orders Database-Beispiel umschließt Microsoft Graph Aufrufe mit der **webaccountmanager** -Bibliothek, um ein Token abzurufen und die Authentifizierung für ein Aad-Konto zu verarbeiten.

* Die Authentifizierungs Logik ist in [**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)implementiert.
* Der Authentifizierungsprozess wird im benutzerdefinierten [**authenticationcontrol. XAML**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) -Steuerelement angezeigt.

### <a name="rest-calls"></a>Rest-Aufrufe

Sie müssen keinen der in diesem Tutorial hinzugefügten Codes ändern, um Rest-Aufrufe zu implementieren. Stattdessen müssen Sie die folgenden Schritte ausführen:

* Erstellen Sie neue Implementierungen der **icustomerrepository** -Schnittstelle und der **itutorialrepository** -Schnittstelle, und implementieren Sie dabei den gleichen Funktions Satz über Rest anstelle von SQLite. Sie müssen JSON Serialisieren und deserialisieren, und Sie können Ihre Rest-Aufrufe bei Bedarf in einer separaten **httphelper** -Klasse einschließen. Einzelheiten finden Sie [im vollständigen Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) .
* Erstellen Sie in **app.XAML.cs**eine neue Funktion, um das Rest-Repository zu initialisieren, und rufen Sie es anstelle von **SQLiteDatabase** auf, wenn die APP initialisiert wird. Weitere Informationen finden Sie [im vollständigen Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Nachdem alle drei Schritte ausgeführt wurden, sollten Sie sich über Ihre APP bei Ihrem Aad-Konto authentifizieren können. Rest-Aufrufe an die Remote Datenbank ersetzen die lokalen SQLite-Aufrufe, aber die Benutzer Darstellung sollte identisch sein. Wenn Sie noch ehrgeiziger sind, können Sie eine Seite mit Einstellungen hinzufügen, die es dem Benutzer ermöglicht, zwischen beiden dynamisch zu wechseln.
