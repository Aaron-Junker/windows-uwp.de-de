---
title: Verwenden einer SQLite-Datenbank in einer UWP-App
description: Hier erfahren Sie, wie Sie eine SQLite-Datenbank in einer UWP-App verwenden, um Daten in einer einfachen Datenbank auf dem Gerät des Benutzers zu speichern und abzurufen.
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, UWP, SQLite, Datenbank
ms.localizationpriority: medium
ms.openlocfilehash: ba2bcf104bd1fee9657e83f7a20334522fa0450c
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860203"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>Verwenden einer SQLite-Datenbank in einer UWP-App
Sie können SQLite verwenden, um Daten in einer einfachen Datenbank auf dem Gerät des Benutzers zu speichern und abzurufen. Dieser Leitfaden zeigt Ihnen wie.

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>Einige Vorteile der Verwendung von SQLite für die lokale Speicherung

:heavy_check_mark: SQLite ist einfach und eigenständig. Es ist eine Code-Bibliothek ohne weitere Abhängigkeiten. Es gibt nichts zu konfigurieren.

:heavy_check_mark: Es gibt keinen Datenbankserver. Client und der Server laufen im selben Prozess.

:heavy_check_mark: SQLite ist öffentlich zugänglich, so dass Sie es mit Ihrer App frei verwenden und verteilen können.

:heavy_check_mark: SQLite arbeitet plattform- und architekturübergreifend.

Mehr über SQLite erfahren Sie [hier](https://sqlite.org/about.html).

## <a name="choose-an-abstraction-layer"></a>Auswählen einer Abstraktionsschicht

Wir empfehlen, dass Sie entweder Entity Framework Core oder die Open-Source-[SQLite-Bibliothek](https://github.com/aspnet/Microsoft.Data.Sqlite/) von Microsoft verwenden.

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) ist eine objektrelationale Zuordnung, die Ihnen über domänenspezifische Objekte die Verwendung relationaler Daten ermöglicht. Wenn Sie dieses Framework bereits für die Arbeit mit Daten in anderen .NET-Apps verwendet haben, können Sie diesen Code in eine UWP-App migrieren. Es funktioniert mit entsprechenden Änderungen an der Verbindungszeichenfolge.

Weitere Informationen finden Sie unter [Erste Schritte mit EF Core auf der Universellen Windows-Plattform (UWP) mit einer neuen Datenbank](/ef/core/get-started/uwp/getting-started).

### <a name="sqlite-library"></a>SQLite-Bibliothek

Die [Microsoft.Data.Sqlite](/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0&preserve-view=true)-Bibliothek implementiert die Schnittstellen im [System.Data.Common](/dotnet/api/system.data.common)-Namespace. Microsoft pflegt diese Implementierungen aktiv und bietet einen intuitiven Wrapper für die native SQLite-API auf niedriger Ebene.

Der Rest dieses Leitfadens hilft Ihnen bei der Verwendung dieser Bibliothek.

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>Einrichten Ihrer Lösung für die Verwendung der Microsoft.Data.SQlite Bibliothek

Wir beginnen mit einem einfachen UWP-Projekt, fügen eine Klassenbibliothek hinzu und installieren dann die entsprechenden Nuget-Pakete.

Die Art der Klassenbibliothek, die Sie zu Ihrer Lösung hinzufügen, und die spezifischen Pakete, die Sie installieren, hängen von der Mindestversion des Windows SDKs ab, auf das Ihre Anwendung abzielt. Sie finden diese Informationen auf der Eigenschaftenseite des UWP-Projekts.

![Mindestversion des Windows SDK](images/min-version.png)

Verwenden Sie einen der folgenden Abschnitte, abhängig von der Mindestversion des Windows SDKs, auf die Ihr UWP-Projekt abzielt.

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>Die Mindestversion Ihres Projekts zielt nicht auf das Fall Creators Update ab.

Wenn Sie Visual Studio 2015 verwenden, klicken Sie auf **Hilfe**->**Info über Microsoft Visual Studio**. Stellen Sie dann in der Liste der installierten Programme sicher, dass Sie über den NuGet Package Manager Version **3.5** oder höher verfügen. Wenn Ihre Versionsnummer niedriger ist, installieren Sie [hier](https://www.nuget.org/downloads) eine spätere Version von NuGet. Auf dieser Seite finden Sie unter der Überschrift **Visual Studio 2015** alle Versionen von Nuget.

Als Nächstes fügen Sie Ihrer Lösung eine Klassenbibliothek hinzu. Sie benötigen keine Klassenbibliothek, die Ihren Datenzugriffscode enthält, in unserem Beispiel verwenden wir jedoch eine. Wir nennen die Bibliothek **DataAccessLibrary** und die Klasse in der Bibliothek **DataAccess**.

![Screenshot des Dialogfelds „Neues Projekt hinzufügen“ mit ausgewähltem „Installiert > Visual C Sharp > Windows Universal“ und der hervorgehobenen Option „Klassenbibliothek“.](images/class-library.png)

Klicken Sie mit der rechten Maustaste auf die Lösung, und klicken Sie dann auf **NuGet-Pakete für Lösung verwalten**.

![Screenshot des Projektmappen-Explorer-Bereichs mit dem Projekt, auf das mit der rechten Maustaste geklickt wurde, und der hervorgehobenen Option „NuGet-Pakete für Projektmappe verwalten“.](images/manage-nuget.png)

Wenn Sie Visual Studio 2015 verwenden, wählen Sie die Registerkarte **Installiert** aus, und stellen Sie sicher, dass die Versionsnummer des **Microsoft.NETCore.UniversalWindowsPlatform**-Pakets **5.2.2** oder höher ist.

![Version von .NETCore](images/package-version.png)

Ist dies nicht der Fall, aktualisieren Sie das Paket auf eine neuere Version.

Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach dem Paket **Microsoft.Data.SQLite**. Installieren Sie Version **1.1.1** (oder niedriger) dieses Pakets.

![Screenshot des Dialogfelds „Microsoft Data SQLite“ mit angezeigtem Versionstextfeld.](images/sqlite-package.png)

Gehen Sie zum Abschnitt [Hinzufügen und Abrufen von Daten in einer SQLite-Datenbank](#add-and-retrieve-data-in-a-sqlite-database) in diesem Leitfaden.

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>Die Mindestversion Ihres Projekts zielt auf das Fall Creators Update ab.

Das Erhöhen der Mindestversion Ihres UWP-Projekts auf das Fall Creators Update birgt eine Reihe von Vorteilen.

Zunächst einmal können Sie .NET Standard 2.0-Bibliotheken anstelle von regulären Klassenbibliotheken verwenden. Das bedeutet, dass Sie Ihren Datenzugriffscode mit jeder anderen .NET-basierten Anwendung wie WPF, Windows Forms, Android, iOS oder ASP.NET teilen können.

Zweitens müssen in Ihrer App keine SQLite-Bibliotheken gepackt werden. Stattdessen kann Ihre App die Version von SQLite verwenden, die mit Windows installiert wird. Das hilft Ihnen in mehrfacher Hinsicht.

:heavy_check_mark: Reduziert die Größe Ihrer App, da Sie die SQLite-Binärdatei nicht herunterladen und dann als Teil Ihrer App verpacken müssen.

:heavy_check_mark: Verhindert, dass Sie eine neue Version Ihrer App an Benutzer weitergeben müssen, falls SQLite wichtige Fixes für Fehler und Sicherheitsschwachstellen in SQLite veröffentlicht. Die Windows-Version von SQLite wird von Microsoft in Abstimmung mit SQLite.org gepflegt.

:heavy_check_mark: Die Ladezeit der App kann kürzer sein, da die SDK-Version von SQLite wahrscheinlich bereits in den Speicher geladen wird.

Beginnen wir mit dem Hinzufügen einer .NET Standard 2.0-Klassenbibliothek zu Ihrer Lösung. Es ist nicht notwendig, dass Sie eine Klassenbibliothek verwenden, die Ihren Datenzugriffscode enthält, aber wir verwenden eine in unserem Beispiel. Wir nennen die Bibliothek **DataAccessLibrary** und die Klasse in der Bibliothek **DataAccess**.

![Screenshot des Dialogfelds „Neues Projekt hinzufügen“ mit ausgewähltem „Installiert > Visual C Sharp > dot NET Standard“ und der hervorgehobenen Option „Klassenbibliothek“.](images/dot-net-standard.png)

Klicken Sie mit der rechten Maustaste auf die Lösung, und klicken Sie dann auf **NuGet-Pakete für Lösung verwalten**.

![Ein weiterer Screenshot des Projektmappen-Explorer-Bereichs mit dem Projekt, auf das mit der rechten Maustaste geklickt wurde, und der hervorgehobenen Option „NuGet-Pakete verwalten“.](images/manage-nuget-2.png)

> [!NOTE]
> Wenn Sie möchten, dass Ihre .NET Standard-Klassenbibliothek auf App-Ordner und Bildobjekte Ihrer UWP-App zugreifen kann, müssen Sie sie in ihren **Eigenschaften** als **EmbeddedResource** und **CopyAlways** markieren.

An diesem Punkt haben Sie die Wahl. Sie können die Version von SQLite verwenden, die in Windows enthalten ist, oder wenn Sie einen Grund haben, eine bestimmte Version von SQLite zu verwenden, können Sie die SQLite-Bibliothek in Ihr Paket aufnehmen.

Beginnen wir damit, wie Sie die Version von SQLite verwenden, die in Windows enthalten ist.

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>So verwenden Sie die unter Windows installierte Version von SQLite

Wählen Sie die Registerkarte **Durchsuchen**, suchen Sie nach dem **Microsoft.Data.SQLite.core**-Paket, und installieren Sie es.

![SQLite Core-Paket](images/sqlite-core-package.png)

Suchen Sie nach dem Paket **SQLitePCLRaw.bundle_winsqlite3**, und installieren Sie es nur im UWP-Projekt Ihrer Lösung.

![SQLite PCL Raw-Paket](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>So binden Sie SQLite in Ihre App ein

Sie müssen dies nicht tun. Wenn Sie jedoch einen Grund haben, eine bestimmte Version von SQLite in Ihre App aufzunehmen, wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach dem Paket **Microsoft.Data.SQLite**. Installieren Sie Version **2.0** (oder niedriger) dieses Pakets.

![Screenshot des Dialogfelds „Microsoft Data SQLite“ mit ausgewählter „Letzter stabiler Version 2.0.0“. Mit angezeigtem Versionstextfeld.](images/sqlite-package-v2.png)

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>Hinzufügen und Abrufen von Daten in einer SQLite-Datenbank

Wir führen die folgenden Vorgänge aus:

:one: Vorbereiten der Datenzugriffsklasse

:two: Initialisieren der SQLite-Datenbank

:three: Einfügen von Daten in die SQLite-Datenbank

:four: Abrufen von Daten aus der SQLite-Datenbank

:five: Hinzufügen einer einfachen Benutzeroberfläche

### <a name="prepare-the-data-access-class"></a>Vorbereiten der Datenzugriffsklasse

Fügen Sie aus Ihrem UWP-Projekt einen Verweis auf das **DataAccessLibrary**-Projekt in Ihrer Lösung hinzu.

![Datenzugriffs-Klassenbibliothek](images/ref-class-library.png)

Fügen Sie die folgende ``using``-Anweisung den Dateien **App.xaml.cs** und **MainPage.xaml.cs** in Ihrem UWP-Projekt hinzu.

```csharp
using DataAccessLibrary;
```

Öffnen Sie die **DataAccess**-Klasse in Ihrer **DataAccessLibrary**-Lösung, und deklarieren Sie diese Klasse als statisch.

>[!NOTE]
>In unserem Beispiel wird der Datenzugriffscode in einer statischen Klasse platziert. Dies ist jedoch nur eine Designentscheidung und völlig optional.

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

Fügen Sie die folgenden using-Anweisungen am Anfang dieser Datei hinzu.

```csharp
using Microsoft.Data.Sqlite;
using System.Collections.Generic;
```


### <a name="initialize-the-sqlite-database"></a>Initialisieren der SQLite-Datenbank

Fügen Sie der **DataAccess**-Klasse eine Methode hinzu, die die SQLite-Datenbank initialisiert.

```csharp
public async static void InitializeDatabase()
{ 
     await ApplicationData.Current.LocalFolder.CreateFileAsync("sqliteSample.db", CreationCollisionOption.OpenIfExists);
     string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
     using (SqliteConnection db =
        new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

Dieser Code erstellt die SQLite-Datenbank und speichert sie im lokalen Datenspeicher der Anwendung.

In diesem Beispiel benennen wir die Datenbank als ``sqlliteSample.db``. Sie können aber jeden beliebigen Namen verwenden, solange Sie diesen Namen in allen instanziierten [SqliteConnection](/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0&preserve-view=true)-Objekten verwenden.

Rufen Sie im Konstruktor der Datei **App.xaml.cs** Ihres UWP-Projekts die ``InitializeDatabase``-Methode der **DataAccess**-Klasse auf.

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```


### <a name="insert-data-into-the-sqlite-database"></a>Einfügen von Daten in die SQLite-Datenbank

Fügen Sie der **DataAccess**-Klasse eine Methode hinzu, die Daten in die SQLite-Datenbank einfügt. Dieser Code verwendet Parameter in der Abfrage, um Angriffe durch Einschleusung von SQL-Befehlen zu verhindern.

```csharp
public static void AddData(string inputText)
{
    string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
    using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```


### <a name="retrieve-data-from-the-sqlite-database"></a>Abrufen von Daten aus der SQLite-Datenbank

Fügen Sie eine Methode hinzu, die Datenzeilen aus einer SQLite-Datenbank abruft.

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

   string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
   using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

Die [Read](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_Read)-Methode durchläuft die Zeilen der zurückgegebenen Daten. Sie gibt **true** zurück, wenn noch Zeilen übrig sind, andernfalls **false**.

Die [GetString](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_)-Methode gibt den Wert der angegebenen Spalte als Zeichenfolge zurück. Sie akzeptiert einen Ganzzahlwert, der die nullbasierte Spalten-Ordinalzahl der gewünschten Daten darstellt. Sie können Methoden wie [GetDataTime](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) und [GetBoolean](/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0&preserve-view=true#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_) verwenden. Wählen Sie eine Methode für den Datentyp der Spalte aus.

Der Ordinalparameter ist in diesem Beispiel nicht so wichtig, da wir alle Einträge in einer einzigen Spalte auswählen. Wenn jedoch mehrere Spalten Teil Ihrer Abfrage sind, verwenden Sie den Ordinalwert, um die Spalte zu erhalten, aus der Sie Daten abrufen möchten.

## <a name="add-a-basic-user-interface"></a>Hinzufügen einer einfachen Benutzeroberfläche

Fügen Sie in der Datei **MainPage.xaml** des UWP-Projekts den folgenden XAML-Code hinzu.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

Diese grundlegende Benutzeroberfläche bietet dem Benutzer eine ``TextBox``, über die er eine Zeichenfolge eingeben kann, die wir der SQLite-Datenbank hinzufügen. Wir verbinden den ``Button`` in dieser Benutzeroberfläche mit einem Ereignishandler, der Daten aus der SQLite-Datenbank abruft und diese Daten dann in der ``ListView`` anzeigt.

Fügen Sie in der Datei **MainPage.xaml.cs** den folgenden Handler hinzu. Dies ist die Methode, die wir dem ``Click``-Ereignis des ``Button`` in der UI zugeordnet haben.

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

Das war's. Erkunden Sie [Microsoft.Data.Sqlite](/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0&preserve-view=true), um zu erfahren, welche weiteren Vorgänge Sie mit Ihrer SQLite-Datenbank ausführen können. Besuchen Sie die Links unten, um mehr über andere Möglichkeiten der Datenverwendung in Ihrer UWP-App zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

**Direktes Verbinden Ihrer App mit einer SQL Server-Datenbank**

Informationen hierzu finden Sie unter [Verwenden einer SQL Server-Datenbank in einer UWP-App](sql-server-databases.md).

**Nutzen des gleichen Codes für verschiedene Apps auf verschiedenen Plattformen**

Weitere Informationen finden Sie unter [Migrieren von einer Desktopanwendung zu UWP](../porting/desktop-to-uwp-migrate.md).

**Hinzufügen von Master/Detail-Seiten mit Azure SQL-Back-Ends**

Weitere Informationen finden Sie unter [Customers Orders Database sample](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (Beispiel für eine Kundenauftragsdatenbank).
