---
title: Lernpfad – Anzeigen von Kunden in einer Liste
description: Hier erfahren Sie, was Sie tun müssen, um eine Sammlung von Kundenobjekten in einer Liste anzuzeigen.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Datenbindung, Liste
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd4a1f6747ea68623039b7eac22ac08aaa15d9ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651375"
---
# <a name="display-customers-in-a-list"></a>Anzeigen von Kunden in einer Liste

Das Anzeigen und Bearbeiten von echten Daten auf der Benutzeroberfläche ist für die Funktionalität vieler Apps von entscheidender Bedeutung. In diesem Artikel erfahren Sie, was Sie wissen müssen, um eine Sammlung von Kundenobjekten in einer Liste anzuzeigen.

Dies ist kein Lernprogramm. Wenn Sie ein Lernprogramm suchen, sehen Sie sich unser [Datenbindungs-Tutorial](../data-binding/xaml-basics-data-binding.md) an, in dem Sie schrittweise Anleitungen finden.

Wir beginnen mit einer kurzen Erläuterung der Datenbindung. Im Anschluss daran fügen wir eine **ListView** (Listenansicht) zur Benutzeroberfläche hinzu, fügen eine Datenbindung hinzu und passen die Datenbindung mit weiteren Features an. 

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Das Binden von Daten ist eine Möglichkeit, die Daten einer App auf ihrer Benutzeroberfläche anzuzeigen. Dies ermöglicht eine Art *Aufgabentrennung* in der App, um die Benutzeroberfläche vom anderen Code zu separieren. Dadurch entsteht ein übersichtlicheres konzeptionelles Modell, das leichter zu lesen und besser zu pflegen ist.

Jede Bindung besteht aus zwei Teilen:

* Einer Quelle, die die zu bindenden Daten bereitstellt
* Einem Ziel auf der Benutzeroberfläche, wo die Daten angezeigt werden

Um eine Datenbindung zu implementieren, müssen Sie der Quelle, die die Daten für die Bindung bereitstellt, Code hinzufügen. Sie müssen auch eine der beiden Markuperweiterungen zu Ihrem XAML-Code hinzufügen, um die Eigenschaften der Datenquelle anzugeben. Dies ist der wichtigste Unterschied zwischen beiden Erweiterungen:

* [**X: Bind** ](../xaml-platform/x-bind-markup-extension.md) ist stark typisiert, und generiert Code zum Zeitpunkt der Kompilierung für eine bessere Leistung. Standardmäßig wird für x:Bind eine einmalige Bindung verwendet, die die schnelle Anzeige von schreibgeschützten Daten optimiert, die sich nicht ändern.
* [**Binden von** ](../xaml-platform/binding-markup-extension.md) ist SQLite schwach typisiert und zur Laufzeit zusammengestellt. Dies führt zu einer schlechteren Leistung als bei x:Bind. In fast allen Fällen sollten Sie x:Bind anstelle von Bindung verwenden. Allerdings ist Binding wahrscheinlich noch in älterem Code zu finden. Bei Binding wird standardmäßig eine unidirektionale Datenübertragung verwendet, die für schreibgeschützte Daten optimiert ist, die sich an der Quelle ändern können.

Wir empfehlen, immer möglichst **x:Bind** zu verwenden. Dies wird in diesem Artikel anhand von Codeausschnitten veranschaulicht. Weitere Informationen zu den Unterschieden finden Sie unter [Vergleich der Features von {x:Bind} und {Binding}](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison).

## <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Zunächst benötigen Sie eine Klasse zur Darstellung Ihrer Kundendaten. Um Ihnen einen Referenzpunkt zu geben, zeigen wir den Prozess anhand dieses einfachen Beispiels:

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>Erstellen einer Liste

Bevor Sie Kunden anzeigen können, müssen Sie eine Liste dieser Kunden erstellen, in der sie gespeichert werden. Die [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) ist ein einfaches XAML-Steuerelement, das sich für diese Aufgabe perfekt eignet. Ihre Listenansicht erfordert eine Position auf der Seite und gleich auch einen Wert für ihre **ItemSource**-Eigenschaft.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

Nachdem Sie Daten an Ihre ListView gebunden haben, empfehlen wir Ihnen, zur Dokumentation zurückzukehren und mit der Anpassung der Darstellung und des Layouts zu experimentieren.

## <a name="bind-data-to-your-list"></a>Binden von Daten an Ihre Liste

Nun, da Sie eine grundlegende Oberfläche zum Speichern der Bindungen erstellt haben, müssen Sie Ihre Quelle konfigurieren, um sie bereitzustellen. Dies ist ein Beispiel hierfür:

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

In der [Übersicht über Datenbindung](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items) werden Sie durch ein ähnliches Problem geführt (siehe Abschnitt zum Binden an eine Sammlung von Elementen). Unser Beispiel zeigt die folgenden wesentlichen Schritte:

* Erstellen Sie im Code hinter Ihrer Benutzeroberfläche eine Eigenschaft vom Typ **ObservableCollection<T>**, um Ihre Kundenobjekte abzulegen.
* Binden Sie die **ItemSource** Ihrer Listenansicht an diese Eigenschaft.
* Stellen Sie ein einfaches **ItemTemplate** für die Listenansicht bereit. Diese Vorlage legt fest, wie jedes Element in der Liste angezeigt wird.

Schauen Sie sich gern noch einmal die Dokumente zur [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) an, wenn Sie das Layout anpassen, eine Auswahl von Elementen hinzufügen oder das soeben erstellte **DataTemplate** anpassen möchten. Aber was geschieht, wenn Sie Ihre Kunden bearbeiten möchten?

## <a name="edit-your-customers-through-the-ui"></a>Bearbeiten der Kunden über die Benutzeroberfläche

Sie haben Kunden in einer Liste angezeigt, aber die Datenbindung bietet noch mehr Möglichkeiten. Was wäre, wenn Sie Ihre Daten direkt über die Benutzeroberfläche bearbeiten könnten? Zu diesem Zweck müssen wir zunächst über die drei Modi der Datenbindung sprechen:

* *Einmalige*: Dieser Datenbindung wird nur einmal aktiviert, und nicht auf Änderungen reagieren.
* *Unidirektionale*: Dieser Datenbindung wird die Benutzeroberfläche mit Änderungen an der Datenquelle aktualisiert werden.
* *Bidirektionale*: Dieser Datenbindung wird die Benutzeroberfläche mit Änderungen an der Datenquelle aktualisiert, und aktualisieren Sie die Daten auch mit den Änderungen, die auf der Benutzeroberfläche vorgenommen.

Wenn Sie die Codeausschnitte von vorher befolgt haben, verwendet Ihre Bindung x:Bind und gibt keinen Modus an; folglich handelt es sich um eine einmalige Bindung. Wenn Sie Ihre Kunden direkt über die Benutzeroberfläche bearbeiten möchten, müssen Sie die Bindung in eine bidirektionale Bindung ändern, damit die Änderungen von den Daten zurück an die Kundenobjekte übergeben werden. Weitere Informationen finden Sie unter [Datenbindung im Detail](../data-binding/data-binding-in-depth.md).

Die bidirektionale Bindung aktualisiert ebenfalls die Benutzeroberfläche, wenn die Datenquelle geändert wird. Damit dies funktioniert, müssen Sie [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) an der Quelle implementieren und sicherstellen, dass durch die Eigenschaftseinstellungen das Ereignis **PropertyChanged** ausgelöst wird. Üblicherweise rufen diese eine Hilfsmethode wie **OnPropertyChanged** auf, wie unten dargestellt:

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
Machen Sie dann den Text in der Listenansicht bearbeitbar, indem Sie ein **TextBox** anstelle eines **TextBlock** verwenden. Stellen Sie sicher, dass Sie den **Modus** für Ihre Datenbindung auf **TwoWay** (bidirektional) festlegen.

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Sie können schnell sicherzustellen, dass dies funktioniert, indem Sie eine zweite Listenansicht mit TextBox-Steuerelementen und unidirektionalen (OneWay) Bindungen hinzufügen. Die Werte in der zweiten Liste werden automatisch geändert, wenn die erste bearbeitet wird.

> [!NOTE]
> Das direkte Bearbeiten innerhalb einer Listenansicht ist eine einfache Möglichkeit, die bidirektionale Bindung in Aktion zu zeigen. Dies kann jedoch zu Komplikationen bei der Verwendbarkeit führen. Wenn Sie Ihre App weiter vertiefen möchten, erwägen Sie, [andere XAML-Steuerelemente](../design/controls-and-patterns/controls-and-events-intro.md) zum Bearbeiten Ihrer Daten zu verwenden und Ihre Listansicht schreibgeschützt zu lassen.

## <a name="going-further"></a>Vertiefung

Nun, da Sie eine Liste von Kunden mit einer bidirektionalen Bindung erstellt haben, können Sie noch einmal die Dokumente durcharbeiten, für die wir Links zur Verfügung gestellt haben, und etwas experimentieren. Sie können auch unser [Datenbindungs-Tutorial](../data-binding/xaml-basics-data-binding.md) abrufen, wenn Sie eine schrittweise Anleitung für die grundlegenden und erweiterten Bindungen erhalten oder sich näher mit Steuerelementen wie die [Master/Detail-Muster](../design/controls-and-patterns/master-details.md) beschäftigen möchten, um eine stabilere Benutzeroberfläche zu erreichen.

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung zu den APIs und weiterer nützlicher Dokumentation, die Sie bei Ihren ersten Schritten rund um die Datenbindung unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [Datenvorlage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | Beschreibt die visuelle Struktur eines Datenobjekts, was die Anzeige bestimmter Elementen auf der Benutzeroberfläche ermöglicht. |
| [X: Bind](../xaml-platform/x-bind-markup-extension.md) | Dokumentation zur empfohlenen x:Bind-Markuperweiterung. |
| [Bindung](../xaml-platform/binding-markup-extension.md) | Dokumentation zur älteren Binding-Markuperweiterung. |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | Stellt ein UI-Steuerelement dar, das Datenelemente in einem vertikalen Stapel anzeigt. |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Stellt ein einfaches Text-Steuerelement zum Anzeigen bearbeitbarer Textdaten auf der Benutzeroberfläche dar. |
| ["INotifyPropertyChanged"](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | Schnittstelle, um Daten feststellbar („observable“) zu machen und für eine Datenbindung bereitzustellen. |
| [ItemsControl-Element](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | Die **ItemsSource**-Eigenschaft dieser Klasse ermöglicht die Bindung einer Listenansicht an eine Datenquelle. |

### <a name="useful-docs"></a>Nützliche Dokumente

| Thema | Beschreibung |
|-------|----------------|
| [Datenbindung im Detail](../data-binding/data-binding-in-depth.md) | Eine grundlegende Übersicht über die Prinzipien der Datenbindung |
| [Übersicht über die Datenbindung](../data-binding/data-binding-quickstart.md) | Ausführliche konzeptionelle Informationen zur Datenbindung. |
| [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) | Informationen zum Erstellen und Konfigurieren einer Listenansicht, einschließlich der Implementierung einer Datenvorlage (**DataTemplate**) |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Data Binding-Lernprogramm](../data-binding/xaml-basics-data-binding.md) | Eine schrittweise Anleitung durch die Grundlagen der Datenbindung. |
| [ListView und GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | Erkunden Sie ausführlichere Listenansichten mit Datenbindung. |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | Hier sehen Sie die Datenbindung in Aktion, einschließlich der **BindableBase**-Klasse (im Ordner „Common“) für eine Standardimplementierung von **INotifyPropertyChanged**. |
