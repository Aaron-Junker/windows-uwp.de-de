---
title: Verwenden einer SQL Server-Datenbank in einer UWP-App
description: Verwenden Sie eine SQL Server-Datenbank in einer UWP-App.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, SQL Server, datenbank
ms.localizationpriority: medium
ms.openlocfilehash: 4fe215a593293ff91afb7f71a830512ac365093f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621155"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>Verwenden einer SQL Server-Datenbank in einer UWP-App
Ihre App kann sich direkt mit einer SQL Server-Datenbank verbinden und dann Daten über Klassen im Namespace [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) speichern und abrufen.

In diesem Leitfaden zeigen wir Ihnen, wie Sie das tun können. Wenn Sie die [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases)-Beispieldatenbank auf Ihrer SQL Server-Instanz installieren und dann diese Snippets verwenden, erhalten Sie eine grundlegende Benutzeroberfläche, die Produkte aus der Northwind-Beispieldatenbank anzeigt.

![Northwind-Produkte](images/products-northwind.png)

Die in diesem Leitfaden gezeigten Snippets basieren auf diesem [vollständigeren Beispiel](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo).

## <a name="first-set-up-your-solution"></a>Richten Sie zunächst Ihre Lösung ein

Um Ihre App direkt mit einer SQL Server-Datenbank zu verbinden, stellen Sie sicher, dass die Mindestversion Ihres Projekts auf das Fall Creators Update zielt.  Sie finden diese Informationen auf der Eigenschaftsseite Ihres UWP-Projekts.

![Mindestversion des Windows SDKs](images/min-version-fall-creators.png)

Öffnen Sie die Datei **Package.appxmanifest** Ihres UWP-Projekts im Manifest-Designer.

In der **Funktionen** Registerkarte die **Unternehmensauthentifizierung** Kontrollkästchen, wenn Sie Windows-Authentifizierung zum Authentifizieren Ihrer SQL Server verwenden.

![Funktion Unternehmensauthentifizierung](images/enterprise-authentication.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>Hinzufügen und Abrufen von Daten in einer SQL Server-Datenbank

In diesem Abschnitt werden wir diese Dinge tun:

: ein: Fügen Sie eine Verbindungszeichenfolge hinzu.

: zwei: Erstellen Sie eine Klasse zum Speichern von Produktdaten.

: drei: Abrufen von Produkten aus der SQL Server-Datenbank.

: vier: Fügen Sie eine einfache Benutzeroberfläche hinzu.

: fünf: Füllen Sie die Benutzeroberfläche mit Produkten.

>[!NOTE]
> Dieser Abschnitt zeigt eine Möglichkeit, Ihren Datenzugriffscode zu organisieren. Es ist nur als Beispiel gedacht, wie Sie [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) zum Speichern und Abrufen von Daten aus einer SQL Server-Datenbank verwenden können. Sie können Ihren Code auf jede Art und Weise organisieren, die für das Design Ihrer App am sinnvollsten ist.

### <a name="add-a-connection-string"></a>Hinzufügen einer Verbindungszeichenfolge

Fügen Sie in der Datei **App.xaml.cs** eine Eigenschaft zur Klasse ``App`` hinzu, die anderen Klassen in Ihrer Lösung Zugriff auf die Verbindungszeichenfolge gibt.

Unsere Verbindungszeichenfolge zeigt auf die Northwind-Datenbank in einer SQL Server Express-Instanz.

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

### <a name="create-a-class-to-hold-product-data"></a>Legen Sie eine Klasse für Produktdaten an

Wir erstellen eine Klasse, die das Ereignis [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx) implementiert, so dass wir Attribute in unserer XAML-Benutzeroberfläche an die Eigenschaften in dieser Klasse binden können.

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
        if (PropertyChanged != null)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
        }
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>Produkte aus der SQL Server-Datenbank abrufen

Erstellen Sie eine Methode, die Produkte aus der Northwind-Beispieldatenbank abruft und diese dann als [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx)-Collection von ``Product``-Instanzen zurückgibt.

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

 Fügen Sie den folgenden XAML-Code zur Datei **MainPage.xaml** des UWP-Projekts hinzu.

 Dieser XAML-Code erstellt ein [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), um jedes Produkt anzuzeigen, das Sie im vorherigen Snippet zurückgegeben haben. Es bindet die Attribute jeder Zeile im [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) an die Eigenschaften, die wir in der Klasse ``Product`` definiert haben.

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

### <a name="show-products-in-the-listview"></a>Produkte im ListView anzeigen

Öffnen Sie die Datei **MainPage.xaml.cs** und fügen Sie Code zum Konstruktor der ``MainPage``-Klasse hinzu, die die **ItemSource**-Eigenschaft des [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) auf die [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) der ``Product``-Instanzen setzt.

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

Starten Sie das Projekt und sehen Sie sich die Produkte aus der Northwind-Beispieldatenbank in der Benutzeroberfläche an.

![Northwind-Produkte](images/products-northwind.png)

Erkunden Sie den [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)-Namespace, um zu sehen, was Sie mit Daten in Ihrer SQL Server-Datenbank noch alles machen können.

## <a name="trouble-connecting-to-your-database"></a>Probleme beim Herstellen einer Verbindung mit der Datenbank?

In den meisten Fällen muss ein Aspekt der SQL Server-Konfiguration geändert werden. Wenn Sie mit einem anderen Typ von Desktopanwendung wie einer Windows Forms- oder WPF-Anwendung eine Verbindung mit der Datenbank herstellen können, stellen Sie sicher, dass Sie TCP/IP für SQL Server aktiviert haben. Dies können über die Konsole **Computerverwaltung** durchführen.

![Computerverwaltung](images/computer-management.png)

Stellen Sie dann sicher, dass Ihr SQL Server-Browser-Dienst ausgeführt wird.

![SQL Server-Browser-Dienst](images/sql-browser-service.png)

## <a name="next-steps"></a>Nächste Schritte

**Verwenden Sie eine einfache Datenbank zum Speichern von Daten auf dem Gerät des Benutzers**

Weitere Infos finden Sie unter [Verwenden Sie eine SQLite-Datenbank in einer UWP-App](sqlite-databases.md).

**Freigeben von Code zwischen den verschiedenen apps auf verschiedenen Plattformen**

Weitere Informationen finden Sie unter [Teilen von Code zwischen einer Desktop-App und einer UWP-App](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)

**Hinzufügen von master-Detail-Seiten mit Azure SQL-Back-ends**

Mehr unter [Beispiel einer Kundenauftragsdatenbank.](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
