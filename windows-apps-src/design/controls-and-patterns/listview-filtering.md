---
Description: Filtern der Elemente in Ihrer Sammlung mithilfe von Benutzereingaben.
title: Filtern von Sammlungen
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: Windows 10, UWP
pm-contact: anawish
ms.openlocfilehash: 24669b81c244339509e30a43a0da8a2b27e67eeb
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75302654"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>Filtern von Sammlungen und Listen mithilfe von Benutzereingaben
Wenn Ihre Sammlung viele Elemente aufweist oder stark an Benutzerinteraktion gebunden ist, ist Filterung eine nützliche Funktion, die Sie implementieren können. Filtern mithilfe der in diesem Artikel beschriebenen Methoden kann in den meisten Steuerelementen für Sammlungen implementiert werden, einschließlich [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) und [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2). Viele Arten von Benutzereingaben können zum Filtern einer Sammlung verwendet werden – beispielsweise Kontrollkästchen, Optionsfelder und Schieberegler –, dieser Artikel konzentriert sich aber auf die Verwendung von textbasierten Benutzereingaben zum Aktualisieren einer ListView in Echtzeit, ausgehend von der Suche des Benutzers. 

> [!NOTE]
> Dieser Artikel konzentriert sich auf das Filtern in einer Listenansicht (ListView). Bitte beachten Sie, dass diese Filtermethode auch auf andere Sammlungssteuerelemente wie GridView, ItemsRepeater oder TreeView angewendet werden kann.

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>Einrichten von Benutzeroberfläche und XAML für das Filtern
Zum Implementieren von Filterung sollte Ihre App eine ListView aufweisen, die neben einem Textfeld (TextBox) oder einem anderen Steuerelement angezeigt wird, das Benutzereingaben ermöglicht. Der Text, den der Benutzer in die TextBox eingibt, wird als Filter verwendet, d. h. nur Ergebnisse, die diese Texteingabe/Suchabfrage enthalten, werden angezeigt. Während der Benutzer Text in die TextBox eingibt, wird die ListView ständig mit gefilterten Ergebnissen aktualisiert – insbesondere durchläuft die ListView bei jeder Änderung des Texts in der TextBox, sei es auch nur ein Buchstabe, ihre Elemente und filtert anhand des aktuellen Ausdrucks.

Der Code unten zeigt eine Benutzeroberfläche mit einer einfachen ListView und ihrer DataTemplate, zusammen mit einer begleitenden TextBox. In diesem Beispiel zeigt die ListView eine Sammlung von Person-Objekten. „Person“ ist eine Klasse, die im CodeBehind definiert ist (im Codebeispiel unten nicht dargestellt), und jedes Person-Objekt besitzt die folgenden Eigenschaften: „FirstName“, „LastName“ und „Company“.

Mithilfe der TextBox können Benutzer einen Such-/Filterausdruck eingeben, um die Liste der Person-Objekte anhand des Nachnamens zu filtern. Beachten Sie, dass die TextBox an einen bestimmten Namen (`FilterByLName`) gebunden ist und über ein eigenes Ereignis „TextChanged“ verfügt (`FilteredLV_LNameChanged`). Der gebundene Name ermöglicht den Zugriff auf den Inhalt/Text der TextBox im CodeBehind, und das TextChanged-Ereignis wird bei jeder Eingabe des Benutzers in der TextBox ausgelöst, sodass wir beim Empfang der Benutzereingabe einen Filtervorgang durchführen können. 

Damit das Filtern funktioniert, muss die ListView über eine Datenquelle verfügen, die im CodeBehind bearbeitet werden kann, wie etwa eine `ObservableCollection<>`. In diesem Fall wird die ItemsSource-Eigenschaft der ListView einer `ObservableCollection<Person>` im CodeBehind zugewiesen. 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>Filtern der Daten
[Linq](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries)-Abfragen ermöglichen des Gruppieren, Sortieren und Auswählen bestimmter Elemente in einer Sammlung. Zum Filtern einer Liste erstellen wir eine Linq-Abfrage, die nur Ausdrücke auswählt, die mit der vom Benutzer in der TextBox `FilterByLName` eingegebenen Suchabfrage bzw. dem eingegebenen Filterausdruck übereinstimmen. Das Abfrageergebnis kann einem [IEnumerable<T>](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1)-Sammlungsobjekt zugewiesen werden. Sobald wir über diese Sammlung verfügen, können wir sie für den Vergleich mit der ursprünglichen Liste verwenden, um nicht passende Elemente zu entfernen und passende Elemente wieder einzufügen (falls der Benutzer die RÜCKTASTE verwendet).

> [!NOTE]
> Damit die ListView beim Hinzufügen und Entfernen von Elementen auf möglichst intuitive Weise animiert wird, ist es wichtig, Elemente aus der ItemsSource-Sammlung der ListView selbst zu entfernen bzw. sie dieser hinzuzufügen, statt eine neue Sammlung gefilterter Objekte zu erstellen und diese der ItemsSource-Eigenschaft der ListView hinzuzufügen.

Um anzufangen, müssen wir unsere ursprüngliche Datenquelle in einer separaten Sammlung initialisieren, etwa einer `List<T>` oder `ObservableCollection<T>`. In diesem Beispiel verwenden wir eine `List<Person>` mit dem Namen `People`, die alle in der ListView angezeigten Person-Objekte enthält (die Auffüllung/Initialisierung dieser Liste wird im Codeausschnitt unten nicht dargestellt). Wir benötigen außerdem eine Liste zur Aufnahme der gefilterten Daten, die sich bei jeder Anwendung eines Filters ändert. Dies ist dann eine `ObservableCollection<Person>` mit dem Namen `PeopleFiltered`, und zum Zeitpunkt der Initialisierung enthält sie den gleichen Inhalt wie `People`.
 
Der Code unten führt den Filtervorgang über die folgenden Schritte aus, wie aus dem Code zu ersehen:
 - Festlegen der ItemsSource-Eigenschaft der ListView auf `PeopledFiltered`. 
 - Definieren des TextChanged-Ereignisses `FilteredLV_LNameChanged()` für die TextBox `FilterByLName`. Innerhalb dieser Funktion werden die Daten gefiltert.
 - Zum Filtern der Daten greifen Sie über `FilterByLName.Text` auf die vom Benutzer eingegebene Suchabfrage/den Filterausdruck zu. Verwenden Sie eine Linq-Abfrage, um die Elemente in `People` abzurufen, deren Nachname den Ausdruck `FilterByLName.Text` enthält, und fügen Sie diese passenden Elemente in eine Sammlung mit dem Namen `TempFiltered` ein.
 - Vergleichen Sie die aktuelle `PeopleFiltered`-Sammlung mit den neu gefilterten Elementen in `TempFiltered`, und entfernen Sie gegebenenfalls Elemente aus `PeopleFiltered`, bzw. fügen Sie Elemente hinzu.
 - Wenn Elemente aus der Sammlung `PeopleFiltered` entfernt bzw. ihr hinzugefügt werden, wird die ListView entsprechend aktualisiert und animiert.

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

Wenn der Benutzer jetzt seine Filterausdrücke in die TextBox `FilterByLName` eingibt, wird die ListView sofort aktualisiert, um nur die Personen anzuzeigen, deren Nachname den Filterausdruck enthält.

## <a name="next-steps"></a>Nächste Schritte

### <a name="get-the-sample-code"></a>Beispielcode herunterladen
- Wenn auf Ihrem System die XAML-Steuerelementkatalog</strong>-App installiert ist, klicken Sie [hier](xamlcontrolsgallery:/item/ListView), um die App zu öffnen und ein robusteres, ausführlicheres Beispiel für das Filtern von Listen auf der Seite ListView anzuzeigen.
- Die [XAML-Steuerelementkatalog-App (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT) herunterladen.

### <a name="related-articles"></a>Verwandte Artikel
- [Listen](lists.md)
- [Listenansicht und Rasteransicht](listview-and-gridview.md)
- [Sammlungssteuerung](collection-commanding.md)