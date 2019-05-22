---
Description: Freigeben von Code für eine desktop-Anwendung und einer UWP-app
title: Freigeben von Code für eine desktop-Anwendung und einer UWP-app
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 16b75226d6b79b19978ddf7e37231b15ac7a4e3e
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984169"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>Verschieben von einer Desktopanwendung für UWP

Wenn Sie eine vorhandene desktop-Anwendung verfügen, die mithilfe von .NET Framework (einschließlich WPF und Windows Forms) erstellt wurde oder C++ Win32-APIs, gibt es mehrere Möglichkeiten zum Verschieben in die universelle Windows-Plattform (UWP) und Windows 10.

## <a name="package-your-desktop-application-in-an-msix-package"></a>Packen Sie die desktop-Anwendung in einem Paket MSIX

Sie können die desktop-Anwendung in einem Paket MSIX für den Zugriff auf viele weitere Features von Windows 10 verpacken. MSIX ist ein moderner Windows app-Paket-Format, die eine universelle Packaging-Ergebnis für alle Windows-apps, einschließlich UWP, WPF, Windows Forms und Win32-apps bereitstellt. Packen Ihre Windows-desktop-apps in MSIX Paketen erhalten Sie Zugriff auf eine stabile Installation und aktualisieren die benutzerfreundlichkeit, eine verwaltete Sicherheitsmodell mit eine flexible Funktion System, das Unterstützung für den Microsoft Store, unternehmensverwaltung und viele benutzerdefinierte Distribution-Modelle. Sie können die Anwendung, ob Sie den Quellcode haben oder wenn nur eine vorhandene Installationsdatei (z. B. eine MSI- oder App-V-Installer) verpacken. Nachdem Sie Ihre Anwendung gepackt haben, können Sie die UWP-Features wie z. B. Paket Extensions und andere UWP-Komponenten integrieren.

Weitere Informationen finden Sie unter [Packen von desktop-Anwendungen (Desktop-Brücke)](/windows/msix/desktop/desktop-to-uwp-root) und [Features, die Paketidentität erfordern](/windows/apps/desktop/modernize/modernize-packaged-apps).

## <a name="use-uwp-apis"></a>Verwenden von UWP-APIs

Sie können viele UWP-APIs aufrufen, direkt in Ihrer Windows Forms, WPF oder C++ Win32-desktop-app auf moderne Funktionen für Windows 10-Benutzern das einfache integrieren. Beispielsweise können Sie die UWP-APIs zum Hinzufügen von Popupbenachrichtigungen zu Ihrer desktop-app aufrufen.

Weitere Informationen finden Sie unter [nutzen UWP-APIs in desktop-apps](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>Migrieren von .NET Framework-app in einer UWP-app

Wenn Ihre Anwendung auf .NET Framework ausgeführt wird, können Sie es zu einer UWP-app migrieren, durch die Nutzung von .NET Standard 2.0. Verschieben Sie soviel Code wie möglich in .NET Standard 2.0-Klassenbibliotheken, und klicken Sie dann erstellen Sie eine UWP-app, die von .NET Standard 2.0-Bibliotheken verweist. 

### <a name="share-code-in-a-net-standard-20-library"></a>Teilen von Code in Bibliotheken für .NET Standard 2.0

Wenn Ihre Anwendung auf .NET Framework ausgeführt wird, platzieren Sie möglichst viel Code, wie Sie Bibliotheken in .NET Standard 2.0 Klasse können. Solange Ihr Code-APIs verwendet, die standardmäßig definiert sind, können Sie diesen in einer UWP-App wiederverwenden. Es ist einfacher denn je, Code in den Bibliotheken für .NET Standard zu teilen, da viele andere APIs in .NET Standard 2.0 enthalten sind.

Hier ist ein Video, das Sie mehr darüber informiert.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>Hinzufügen von Bibliotheken für .NET Standard

Fügen Sie der Lösung zunächst eine oder mehrere Klassenbibliotheken für .NET Standard 2.0 hinzu.  

![Hinzufügen eines Dotnet-Standardprojekts](images/desktop-to-uwp/dot-net-standard-project-template.png)

Die Anzahl der Bibliotheken, die Sie der Lösung hinzufügen, hängt davon ab, wie Sie Ihren Code organisieren möchten.

Stellen Sie sicher, dass jede Klassenbibliothek auf **.NET Standard 2.0** abzielt.

![Festlegen von .NET Standard 2.0 als Ziel](images/desktop-to-uwp/target-standard-20.png)

Sie finden diese Einstellung auf den Eigenschaftenseiten des Klassenbibliotheksprojekts.

Fügen Sie dem Desktopanwendungsprojekt einfach einen Verweis auf das Klassenbibliotheksprojekt hinzu.

![Verweis auf die Klassenbibliothek](images/desktop-to-uwp/class-library-reference.png)

Sie können anschließend mit den Tools bestimmen, welcher Teil des Codes dem Standard entspricht. Auf diese Weise können Sie vor dem Verschieben des Code in die Bibliothek entscheiden, welche Teile Sie wiederverwenden können, welche Teile minimal geändert werden müssen und welche Teile anwendungsspezifisch bleiben.

#### <a name="check-library-and-code-compatibility"></a>Überprüfen der Kompatibilität für die Bibliothek und den Code

Wir beginnen mit NuGet-Paketen und anderen DLL-Dateien, die Sie von einem Drittanbieter erhalten haben.

Wenn Ihre Anwendung diese verwendet, bestimmen Sie, ob sie mit .NET Standard 2.0 kompatibel sind. Sie können dazu eine Visual Studio-Erweiterung oder ein Befehlszeilendienstprogramm verwenden.

Verwenden Sie die gleichen Tools, um Ihren Code zu analysieren. Laden Sie die Tools hier herunter ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases)). Schauen Sie sich dieses Video an, um zu erfahren, wie Sie diese verwenden können.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Wenn Ihr Code nicht mit dem Standard kompatibel ist, ziehen Sie weitere Möglichkeiten in Betracht, um diesen Code zu implementieren. Öffnen Sie dazu den [.NET API-Browser](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0). Sie können diesen Browsers verwenden, um die APIs zu überprüfen, die in .NET Standard 2.0 verfügbar sind. Beschränken Sie die Liste unbedingt auf den .NET Standard 2.0.

![Dot-Net-Option](images/desktop-to-uwp/dot-net-option.png)

Ein Teil Ihres Codes ist plattformspezifisch und muss in Ihrem Desktopanwendungsprojekt verbleiben.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Beispiel: Migrieren von Datenzugriffscode in einer .NET Standard 2.0-Bibliothek

Nehmen wir an, dass wir eine sehr einfache Windows Forms-Anwendung verfügen, die Kunden unsere Northwind-Beispieldatenbank anzeigt.

![Windows Forms-App](images/desktop-to-uwp/win-forms-app.png)

Das Projekt enthält eine Klassenbibliothek für .NET Standard 2.0 mit einer statischen Klasse namens **Northwind**. Wenn wir diesen Code in die **Northwind**-Klasse verschieben, wird er nicht kompiliert, da er die Klassen ``SQLConnection``, ``SqlCommand`` und ``SqlDataReader`` verwendet und diese Klassen in .NET Standard 2.0 nicht verfügbar sind.

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
Wir können jedoch den [.NET API-Browser](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0) verwenden, um eine Alternative zu finden. Die Klassen ``DbConnection``, ``DbCommand``, und ``DbDataReader`` sind alle in .NET Standard 2.0 verfügbar, sodass wir diese stattdessen verwenden können.  

Die überarbeitete Version verwendet diese Klassen, um eine Liste der Kunden zu erhalten. Zum Erstellen einer ``DbConnection``-Klasse müssen wir allerdings ein Factoryobjekt übergeben, das wir in der Clientanwendung erstellen.

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

In der CodeBehind-Seite von Windows Form können wir einfach eine Factoryinstanz erstellen und Sie diese an unsere Methode übergeben.

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

### <a name="create-a-uwp-app"></a>Erstellen einer UWP-app

Jetzt können Sie der Lösung eine UWP-App hinzuzufügen.

![Abbildung der Desktop-zu-UWP-Brücke](images/desktop-to-uwp/adaptive-ui.png)

Sie müssen allerdings noch die Seiten der Benutzeroberfläche in XAML erstellen und den für jedes Gerät oder jede Plattform spezifischen Code schreiben. Wenn Sie damit fertig sind, können Sie den gesamten Umfang der Windows 10-Geräte erreichen und Ihre App-Seiten werden modern dargestellt und eignen sich so gut für unterschiedliche Bildschirmgrößen und -Auflösungen.

Ihre App reagiert auf andere Eingabemechanismen als nur Tastatur und Maus, und die Funktionen und Einstellungen sind auf allen Geräten intuitiv. Dies bedeutet, dass Benutzer lernen, Aufgaben nur einmal durchzuführen. Dies funktioniert in vertrauter Art und Weise und ist vom Gerät unabhängig.

Dies sind nur einige Beispiele für die interessanten Funktionen, die in der UWP enthalten sind. Weitere Informationen finden Sie unter [Erstellen Sie mit Windows herausragende Umgebungen](https://developer.microsoft.com/windows/why-build-for-uwp).

#### <a name="add-a-uwp-project"></a>Hinzufügen eines UWP-Projekts

Fügen Sie Ihrer Lösung zunächst ein UWP-Projekt hinzu.

![UWP-Projekt](images/desktop-to-uwp/new-uwp-app.png)

Fügen Sie dann vom UWP-Projekt einen Verweis auf das Projekt der Bibliothek für .NET Standard 2.0 hinzu.

![Verweis auf die Klassenbibliothek](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>Erstellen der Seiten

Fügen Sie XAML-Seiten hinzu, und rufen Sie den Code in der Bibliothek für .NET Standard 2.0 auf.

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

Erste Schritte mit UWP finden Sie unter [Was ist eine UWP-App](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

### <a name="reach-ios-and-android-devices"></a>Erreichen von iOS- und Android-Geräten

Durch das Hinzufügen von Xamarin-Projekten können Sie Android und iOS-Geräten erreichen.  

![Xamarin-Apps](images/desktop-to-uwp/xamarin-apps.png)

Diese Projekte ermöglichen die Verwendung von C# zur Entwicklung von Android- und iOS-Apps mit Vollzugriff auf plattformspezifische und gerätespezifische APIs. Diese Apps nutzen die plattformspezifische Hardwarebeschleunigung und wurden für die native Leistung kompiliert.

Sie haben Zugriff auf das gesamte Spektrum der Funktionen, die von der zugrunde liegenden Plattform und dem Gerät verfügbar gemacht wurden, einschließlich der Plattformfunktionen wie iBeacons und Android Fragments. Sie verwenden standardmäßig native Steuerelemente der Benutzeroberfläche, um Benutzeroberflächen zu erstellen, die dem gewünschten Erscheinungsbild der Benutzer entsprechen.

Genau wie bei UWPs sind die Kosten für das Hinzufügen einer Android- oder iOS-App niedriger, da Sie die Geschäftslogik in einer Klassenbibliothek für .NET Standard 2.0 wiederverwenden können. Sie müssen allerdings noch die Seiten der Benutzeroberfläche in XAML erstellen und den für jedes Gerät oder jede Plattform spezifischen Code schreiben.

#### <a name="add-a-xamarin-project"></a>Hinzufügen eines Xamarin-Projekts

Fügen Sie zuerst der Lösung ein **Android**-, **iOS**- oder **plattformübergreifendes** hinzu.

Sie finden diese Vorlagen im Dialogfeld **Neues Projekt hinzufügen** in der **Visual C#-**-Gruppe.

![Xamarin-Apps](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Plattformübergreifende Projekte sind ideal für Apps mit wenigen plattformspezifischen Funktionen. Sie können diese zur Entwicklung einer nativen XAML-basierten Benutzeroberfläche verwenden, die unter iOS, Android und Windows ausgeführt wird. Weitere Informationen finden Sie [hier](https://www.xamarin.com/forms).

Fügen Sie anschließen von Ihrem Android-, iOS- oder plattformübergreifenden Projekt einen Verweis auf das Klassenbibliotheksprojekt hinzu.

![Verweis auf die Klassenbibliothek](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>Erstellen der Seiten

Unser Beispiel zeigt eine Liste der Kunden in einer Android-App.

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

Informationen zu den ersten Schritten mit Android-, iOS- und plattformübergreifenden Projekten finden Sie im [Xamarin-Entwicklerportal](https://developer.xamarin.com/).

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
