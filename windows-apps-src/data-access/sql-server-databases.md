---
title: Verwenden einer SQL Server-Datenbank in einer UWP-App
description: Hier erfährst du, wie du eine SQL Server-Datenbank in einer UWP-App verwendest.
ms.date: 03/28/2019
ms.topic: article
keywords: Windows 10, UWP, SQL Server, Datenbank
ms.localizationpriority: medium
ms.openlocfilehash: 54907dac63580794b7df42fa2e61162d16be8a1b
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302564"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>Verwenden einer SQL Server-Datenbank in einer UWP-App
Ihre App kann sich direkt mit einer SQL Server-Datenbank verbinden und dann Daten über Klassen im Namespace [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) speichern und abrufen.

In diesem Leitfaden erfährst du, wie das geht. Wenn du die [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases)-Beispieldatenbank in deiner SQL Server-Instanz installierst und diese Codeausschnitte verwendest, erhältst du eine einfache Benutzeroberfläche (User Interface, UI), die Produkte aus der Northwind-Beispieldatenbank anzeigt.

![Northwind-Produkte](images/products-northwind.png)

Die Codeausschnitte in diesem Leitfaden basieren auf [diesem umfassenderen Beispiel](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo).

## <a name="first-set-up-your-solution"></a>Einrichten der Lösung

Um deine App direkt mit einer SQL Server-Datenbank verbinden zu können, muss die Mindestversion deines Projekts auf das Fall Creators Update ausgerichtet sein.  Diese Information findest du auf der Eigenschaftenseite deines UWP-Projekts.

![Mindestversion des Windows SDK](images/min-version-fall-creators.png)

Öffne die Datei **Package.appxmanifest** deines UWP-Projekts im Manifest-Designer.

Aktiviere auf der Registerkarte **Funktionen** das Kontrollkästchen **Unternehmensauthentifizierung**, wenn du zur Authentifizierung deiner SQL Server-Instanz die Windows-Authentifizierung verwendest.

![Funktion „Unternehmensauthentifizierung“](images/enterprise-authentication.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>Hinzufügen und Abrufen von Daten in einer SQL Server-Datenbank

In diesem Abschnitt führen wir folgende Schritte aus:

:one: Hinzufügen einer Verbindungszeichenfolge

:two: Erstellen einer Klasse für Produktdaten

:three: Abrufen von Produkten aus der SQL Server-Datenbank

:four: Hinzufügen einer einfachen Benutzeroberfläche

:five: Auffüllen der Benutzeroberfläche mit Produkten

>[!NOTE]
> In diesem Abschnitt wird eine mögliche Methode zur Strukturierung deines Datenzugriffscodes veranschaulicht. Hierbei handelt es sich lediglich um ein Beispiel, um zu zeigen, wie du Daten mithilfe von [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) in einer SQL Server-Datenbank speichern und daraus abrufen kannst. Du kannst deinen Code natürlich auch anders strukturieren, wenn das für das Design deiner App sinnvoller ist.

### <a name="add-a-connection-string"></a>Hinzufügen einer Verbindungszeichenfolge

Füge in der Datei **App.xaml.cs** der ``App``-Klasse eine Eigenschaft hinzu, damit andere Klassen in deiner Lösung auf die Verbindungszeichenfolge zugreifen können.

Unsere Verbindungszeichenfolge verweist auf die Northwind-Datenbank in einer SQL Server Express-Instanz.

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>Erstellen einer Klasse für Produktdaten

Wir erstellen eine Klasse, die das [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged)-Ereignis implementiert, damit wir Attribute auf unserer XAML-Benutzeroberfläche an die Eigenschaften in dieser Klasse binden können.

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>Abrufen von Produkten aus der SQL Server-Datenbank

Erstelle eine Methode, die Produkte aus der Northwind-Beispieldatenbank abruft und sie dann als [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)-Sammlung von ``Product``-Instanzen zurückgibt.

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>Hinzufügen einer einfachen Benutzeroberfläche

 Füge der Datei **MainPage.xaml** des UWP-Projekts den folgenden XAML-Code hinzu.

 Dieser XAML-Code erstellt ein [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)-Element, um die einzelnen Produkte anzuzeigen, die im vorherigen Codeausschnitt zurückgegeben wurden, und er bindet die Attribute jeder Zeile im [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)-Element an die Eigenschaften, die wir in der ``Product``-Klasse definiert haben.

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>Anzeigen von Produkten in der Listenansicht

Öffne die Datei **MainPage.xaml.cs**, und füge dem Konstruktor der ``MainPage``-Klasse Code hinzu, um die **ItemSource**-Eigenschaft des [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)-Elements auf die [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)-Sammlung von ``Product``-Instanzen festzulegen.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

Starte das Projekt. Daraufhin werden auf der Benutzeroberfläche Produkte aus der Northwind-Beispieldatenbank angezeigt.

![Northwind-Produkte](images/products-northwind.png)

Erkunde den [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient)-Namespace, um noch weitere Verwendungsmöglichkeiten für Daten in deiner SQL Server-Datenbank kennenzulernen.

## <a name="trouble-connecting-to-your-database"></a>Probleme beim Herstellen der Verbindung mit deiner Datenbank?

In den meisten Fällen muss etwas an der SQL Server-Konfiguration geändert werden. Falls du über eine andere Art von Desktopanwendung (etwa über eine Windows Forms- oder WPF-Anwendung) eine Verbindung mit deiner Datenbank herstellen kannst, vergewissere dich, dass du TCP/IP für SQL Server aktiviert hast. Hierzu kannst du die Konsole **Computerverwaltung** verwenden.

![Computerverwaltung](images/computer-management.png)

Vergewissere dich anschließend, dass der Dienst „SQL Server-Browser“ ausgeführt wird.

![Dienst „SQL Server-Browser“](images/sql-browser-service.png)

## <a name="next-steps"></a>Nächste Schritte

**Verwenden einer einfachen Datenbank, um Daten auf dem Gerät des Benutzers zu speichern**

Weitere Informationen findest du unter [Verwenden einer SQLite-Datenbank in einer UWP-App](sqlite-databases.md).

**Nutzen des gleichen Codes für verschiedene Apps auf verschiedenen Plattformen**

Weitere Informationen finden Sie unter [Migrieren von einer Desktopanwendung zu UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

**Hinzufügen von Master/Detail-Seiten mit Azure SQL-Back-Ends**

Weitere Informationen finden Sie unter [Customers Orders Database sample](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (Beispiel für eine Kundenauftragsdatenbank).
