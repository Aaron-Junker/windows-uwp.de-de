---
Description: Teilen von Code zwischen einer Desktop-Anwendung und einer UWP-App
title: Teilen von Code zwischen einer Desktop-Anwendung und einer UWP-App
ms.date: 10/03/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2e13c656f02531d500a72aa74b2d3c5d6cc29aa4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174934"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>Wechseln von einer Desktop Anwendung zu einer UWP

Wenn Sie über eine vorhandene Desktop Anwendung verfügen, die mit dem .NET Framework (einschließlich WPF-und Windows Forms)-oder C++ Win32-APIs erstellt wurde, haben Sie mehrere Optionen für den Umstieg auf die universelle Windows-Plattform (UWP) und Windows 10.

## <a name="package-your-desktop-application-in-an-msix-package"></a>Verpacken der Desktop Anwendung in einem msix-Paket

Sie können Ihre Desktop Anwendung in einem msix-Paket Verpacken, um Zugriff auf viele weitere Windows 10-Funktionen zu erhalten. MSIX ist ein modernes Windows-App-Paketformat, bei dem eine universelle Verpackungsoberfläche für alle Windows-Apps bereitgestellt wird, z. B. UWP-, WPF-, Windows Forms- und Win32-Apps. Durch das Verpacken Ihrer Windows-Desktop-Apps in MSIX-Paketen erhalten Sie Zugriff auf eine stabile Installations- und Aktualisierungsoberfläche, ein verwaltetes Sicherheitsmodell mit einem flexiblen Funktionssystem, Support für den Microsoft Store, Unternehmensverwaltung und viele benutzerdefinierte Distributionsmodelle. Sie können Ihre Anwendung Verpacken, unabhängig davon, ob Sie über den Quellcode verfügen oder ob Sie nur über eine vorhandene Installationsdatei verfügen (z. b. einen MSI-oder App-V-Installer). Nachdem Sie die Anwendung Paketieren, können Sie UWP-Funktionen wie Paket Erweiterungen und andere UWP-Komponenten integrieren.

Weitere Informationen finden Sie unter [Package Desktop Applications (Desktop Bridge)](/windows/msix/desktop/desktop-to-uwp-root) und [Features, die die Paket Identität erfordern](/windows/apps/desktop/modernize/modernize-packaged-apps).

## <a name="use-windows-runtime-apis"></a>Verwenden von Windows-Runtime-APIs

Sie können viele Windows-Runtime-APIs direkt in Ihrer WPF-, Windows Forms- oder C++-Win32-Desktop-App aufrufen, um moderne Benutzeroberflächen für Windows 10-Benutzer zu integrieren. Sie können beispielsweise Windows-Runtime-APIs aufrufen, um Ihrer Desktop-App Popupbenachrichtigungen hinzuzufügen.

Weitere Informationen finden Sie unter [Verwenden von Windows-Runtime-APIs in Desktop-Apps](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>Migrieren einer .NET Framework-APP zu einer UWP-App

Wenn die Anwendung auf dem .NET Framework ausgeführt wird, können Sie Sie zu einer UWP-App migrieren, indem Sie .NET Standard 2,0 nutzen. Verschieben Sie so viel Code wie möglich in .NET Standard 2,0-Klassenbibliotheken, und erstellen Sie dann eine UWP-APP, die auf Ihre .NET Standard 2,0-Bibliotheken verweist. 

### <a name="share-code-in-a-net-standard-20-library"></a>Freigeben von Code in einer .NET Standard 2,0-Bibliothek

Wenn die Anwendung auf dem .NET Framework ausgeführt wird, platzieren Sie so viel Code wie möglich in .NET Standard 2,0-Klassenbibliotheken. Solange Ihr Code APIs verwendet, die im Standard definiert sind, können Sie Sie in einer UWP-APP wieder verwenden. Es ist einfacher, Code in einer .NET Standard Bibliothek gemeinsam zu nutzen, da so viele weitere APIs in der .NET Standard 2,0 enthalten sind.

Im folgenden finden Sie ein Video, das Sie darüber informiert.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>.NET Standard Bibliotheken hinzufügen

Fügen Sie der Projekt Mappe zunächst mindestens eine .NET Standard-Klassenbibliothek hinzu.  

![DotNet-Standard Projekt hinzufügen](images/desktop-to-uwp/dot-net-standard-project-template.png)

Die Anzahl der Bibliotheken, die Sie Ihrer Projekt Mappe hinzufügen, hängt davon ab, wie Sie Ihren Code organisieren möchten.

Stellen Sie sicher, dass jede Klassenbibliothek auf den **.NET Standard 2,0**abzielt.

![Ziel .NET Standard 2,0](images/desktop-to-uwp/target-standard-20.png)

Diese Einstellung finden Sie auf den Eigenschaften Seiten des Klassen Bibliotheks Projekts.

Fügen Sie in Ihrem Desktop Anwendungsprojekt einen Verweis auf das Klassen Bibliotheksprojekt hinzu.

![Klassen Bibliotheks Referenz](images/desktop-to-uwp/class-library-reference.png)

Verwenden Sie als nächstes die Tools, um zu bestimmen, wie viel Code dem Standard entspricht. Auf diese Weise können Sie vor dem Verschieben von Code in die Bibliothek entscheiden, welche Teile Sie wieder verwenden können, welche Teile minimal geändert werden müssen und welche Teile anwendungsspezifisch bleiben.

#### <a name="check-library-and-code-compatibility"></a>Überprüfen der Bibliothek-und Code Kompatibilität

Wir beginnen mit nuget-Paketen und anderen DLL-Dateien, die Sie von einem Drittanbieter erhalten haben.

Wenn Sie von Ihrer Anwendung verwendet werden, stellen Sie fest, ob Sie mit dem .NET Standard 2,0 kompatibel sind. Hierfür können Sie eine Visual Studio-Erweiterung oder ein Befehlszeilen-Hilfsprogramm verwenden.

Verwenden Sie dieselben Tools, um Ihren Code zu analysieren. Laden Sie die Tools hier herunter ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases)), und sehen Sie sich dieses Video an, um zu erfahren, wie Sie es verwenden.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Wenn Ihr Code nicht mit dem Standard kompatibel ist, sollten Sie andere Möglichkeiten in Erwägung nehmen, den Code zu implementieren. Öffnen Sie zunächst den [.NET-API-Browser](/dotnet/api/?view=netstandard-2.0). Sie können diesen Browser zum Überprüfen der APIs verwenden, die in der .NET Standard 2,0 verfügbar sind. Stellen Sie sicher, dass Sie den Bereich der Liste auf den .NET Standard 2,0 festlegen.

![DotNet-Option](images/desktop-to-uwp/dot-net-option.png)

Ein Teil Ihres Codes ist plattformspezifisch und muss in Ihrem Desktop Anwendungsprojekt verbleiben.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Beispiel: Migrieren von Datenzugriffs Code zu einer .NET Standard 2,0-Bibliothek

Angenommen, wir verfügen über eine sehr einfache Windows Forms Anwendung, die Kunden aus der Beispieldatenbank Northwind zeigt.

![Windows Forms-App](images/desktop-to-uwp/win-forms-app.png)

Das Projekt enthält eine .NET Standard 2,0-Klassenbibliothek mit einer statischen Klasse namens **Northwind**. Wenn Sie diesen Code in die **Northwind** -Klasse verschieben, wird er nicht kompiliert, da er die ``SQLConnection`` ``SqlCommand`` Klassen, und verwendet, und die Klassen, die ``SqlDataReader`` nicht im .NET Standard 2,0 verfügbar sind.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
Mit dem .net- [API-Browser](/dotnet/api/?view=netstandard-2.0) können Sie jedoch eine Alternative suchen. Die ``DbConnection`` ``DbCommand`` Klassen, und ``DbDataReader`` sind alle im .NET Standard 2,0 verfügbar, damit wir Sie stattdessen verwenden können.  

Diese überarbeitete Version verwendet diese Klassen, um eine Kundenliste zu erhalten. zum Erstellen einer ``DbConnection`` Klasse müssen wir jedoch ein Factoryobjekt übergeben, das in der Client Anwendung erstellt wird.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Auf der Code Behind-Seite des Windows-Formulars können Sie einfach eine factoryinstanz erstellen und Sie an unsere Methode übergeben.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

### <a name="create-a-uwp-app"></a>Erstellen einer UWP-App

Nun können Sie Ihrer Projekt Mappe eine UWP-app hinzufügen.

![Bild für Desktop-zu-UWP-Bridge](images/desktop-to-uwp/adaptive-ui.png)

Sie müssen weiterhin UI-Seiten in XAML entwerfen und jeden Geräte-oder plattformspezifischen Code schreiben, aber wenn Sie dies erledigt haben, können Sie die gesamte Breite von Windows 10-Geräten erreichen, und Ihre APP-Seiten haben ein modernes Gefühl, das sich gut an verschiedene Bildschirmgrößen und Auflösungen anpasst.

Ihre APP antwortet auf andere Eingabe Mechanismen als nur auf Tastatur und Maus, und Features und Einstellungen werden Geräte übergreifend intuitiv. Dies bedeutet, dass Benutzer sich ein Mal mit der Vorgehensweise vertraut machen, und dann funktioniert Sie in einer sehr vertrauten Weise, unabhängig vom Gerät.

Dies sind nur einige der in UWP enthaltenen Leckereien. Weitere Informationen finden Sie unter [Erstellen von hervor artigen Erfahrungen mit Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

#### <a name="add-a-uwp-project"></a>Hinzufügen eines UWP-Projekts

Fügen Sie der Projekt Mappe zunächst ein UWP-Projekt hinzu.

![UWP-Projekt](images/desktop-to-uwp/new-uwp-app.png)

Fügen Sie dann im UWP-Projekt einen Verweis auf das .NET Standard 2,0-Bibliotheksprojekt hinzu.

![Klassen Bibliotheks Referenz](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>Erstellen Ihrer Seiten

Fügen Sie XAML-Seiten hinzu, und nennen Sie den Code in Ihrer .NET Standard 2,0-Bibliothek.

![UWP-App](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```

Informationen zu den ersten Schritten mit UWP finden Sie unter [Was ist eine UWP-App](../get-started/universal-application-platform-guide.md).

### <a name="reach-ios-and-android-devices"></a>Erreichen von IOS-und Android-Geräten

Sie können Android-und IOS-Geräte erreichen, indem Sie xamarin-Projekte hinzufügen.  

![Xamarin-Apps](images/desktop-to-uwp/xamarin-apps.png)

Mit diesen Projekten können Sie c# verwenden, um Android-und IOS-apps mit Vollzugriff auf plattformspezifische und gerätespezifische APIs zu erstellen. Diese Apps nutzen die plattformspezifische Hardwarebeschleunigung und werden für die native Leistung kompiliert.

Sie haben Zugriff auf das gesamte Spektrum an Funktionen, die von der zugrunde liegenden Plattform und dem Gerät verfügbar gemacht werden, einschließlich plattformspezifischer Funktionen wie ibeacons und Android-Fragmente, und Sie verwenden standardmäßige Native Benutzeroberflächen-Steuerelemente, um Benutzeroberflächen zu erstellen, die die Benutzer erwarten, die Sie erwarten.

Ebenso wie uwps sind die Kosten für das Hinzufügen einer Android-oder IOS-App geringer, da Sie die Geschäftslogik in einer .NET Standard 2,0-Klassenbibliothek wieder verwenden können. Sie müssen ihre UI-Seiten in XAML entwerfen und jeden Geräte-oder plattformspezifischen Code schreiben.

#### <a name="add-a-xamarin-project"></a>Xamarin-Projekt hinzufügen

Fügen Sie Ihrer Projekt Mappe zunächst ein **Android**-, **IOS**-oder Platt **Form übergreifendes** Projekt hinzu.

Diese Vorlagen finden Sie im Dialogfeld **Neues Projekt hinzufügen** unter der Gruppe **Visual c#** .

![Xamarin-Apps](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Plattformübergreifende Projekte eignen sich hervorragend für apps mit wenig plattformspezifischer Funktionalität. Sie können Sie verwenden, um eine native XAML-basierte Benutzeroberfläche zu erstellen, die unter IOS, Android und Windows ausgeführt wird. [Hier](/xamarin/xamarin-forms/)erhalten Sie weitere Informationen.

Fügen Sie dann in Ihrem Android-, IOS-oder plattformübergreifenden Projekt einen Verweis auf das Klassen Bibliotheksprojekt hinzu.

![Klassen Bibliotheks Referenz](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>Erstellen Ihrer Seiten

In unserem Beispiel wird eine Liste von Kunden in einer Android-App angezeigt.

![Android-App](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Informationen zu den ersten Schritten mit Android-, IOS-und plattformübergreifenden Projekten finden Sie im [xamarin-Entwickler Portal](/xamarin).

## <a name="next-steps"></a>Nächste Schritte

**Antworten auf deine Fragen**

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.

**Feedback geben oder Funktions Vorschläge machen**

Siehe [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).