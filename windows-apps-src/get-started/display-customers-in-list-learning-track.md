---
title: Lernpfad – Anzeigen von Kunden in einer Liste
description: Hier erfahren Sie, wie Sie eine Sammlung von Kundenobjekten in einer Liste anzeigen.
ms.date: 05/07/2018
ms.topic: article
keywords: Erste Schritte, UWP, Windows 10, Lernpfad, Datenbindung, Liste
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a949479a021d4f8de592d1991773dd2e31e9769c
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64564478"
---
# <a name="display-customers-in-a-list"></a>Anzeigen von Kunden in einer Liste

Das Anzeigen und Bearbeiten von echten Daten in der Benutzeroberfläche ist für die Funktionalität vieler Apps von entscheidender Bedeutung. In diesem Artikel erfahren Sie, wie Sie eine Sammlung von Kundenobjekten in einer Liste anzeigen.

Dies ist kein Tutorial. Wenn Sie ein Tutorial suchen, sehen Sie sich unser [Datenbindungs-Tutorial](../data-binding/xaml-basics-data-binding.md) an, in dem Sie schrittweise Anleitungen finden.

Wir beginnen mit einer kurzen Erläuterung der Datenbindung. Im Anschluss fügen wir der Benutzeroberfläche eine **ListView** (Listenansicht) hinzu, fügen eine Datenbindung hinzu und passen die Datenbindung mit weiteren Features an.

## <a name="what-do-you-need-to-know"></a>Wissenswertes

Das Binden von Daten ist eine Möglichkeit, die Daten einer App in ihrer Benutzeroberfläche anzuzeigen. Dies ermöglicht eine Art *Aufgabentrennung* in der App, um die Benutzeroberfläche vom anderen Code zu separieren. Dadurch entsteht ein übersichtlicheres konzeptionelles Modell, das leichter zu lesen und besser zu pflegen ist.

Jede Datenbindung besteht aus zwei Teilen:

* Einer Quelle, die die zu bindenden Daten bereitstellt
* Einem Ziel in der Benutzeroberfläche, wo die Daten angezeigt werden

Um eine Datenbindung zu implementieren, müssen Sie der Quelle, die die Daten für die Bindung bereitstellt, Code hinzufügen. Sie müssen auch eine der beiden Markuperweiterungen zu Ihrem XAML-Code hinzufügen, um die Eigenschaften der Datenquelle anzugeben. Dies ist der wichtigste Unterschied zwischen beiden Erweiterungen:

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) ist stark typisiert und generiert Code zum Zeitpunkt der Kompilierung, um die Leistung zu verbessern. Standardmäßig wird für x:Bind eine einmalige Bindung verwendet, die die schnelle Anzeige von schreibgeschützten, unveränderlichen Daten optimiert.
* [**Binding**](../xaml-platform/binding-markup-extension.md) ist schwach typisiert und wird zur Laufzeit assembliert. Dies führt zu einer schlechteren Leistung als bei x:Bind. In fast allen Fällen sollten Sie x:Bind anstelle von Binding verwenden. Allerdings ist Binding wahrscheinlich noch in älterem Code zu finden. Bei Binding wird standardmäßig eine unidirektionale Datenübertragung verwendet, die für schreibgeschützte Daten optimiert ist, die sich in der Quelle ändern können.

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

Bevor Sie Kunden anzeigen können, müssen Sie eine Liste erstellen, in der sie gespeichert werden. Die [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) ist ein einfaches XAML-Steuerelement, das perfekt für diese Aufgabe geeignet ist. Das ListView-Steuerelement benötigt momentan eine Position auf der Seite und in Kürze auch einen Wert für die **ItemSource**-Eigenschaft.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

Nachdem Sie Daten an ListView gebunden haben, empfehlen wir, mithilfe der Dokumentation die Darstellung und das Layout der Liste an Ihre Bedürfnisse anzupassen.

## <a name="bind-data-to-your-list"></a>Binden von Daten an die Liste

Nun, da Sie eine grundlegende Oberfläche zum Speichern der Bindungen erstellt haben, müssen Sie Ihre Quelle konfigurieren, um sie bereitzustellen. Hier ein Beispiel zur Vorgehensweise:

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

In der [Übersicht über Datenbindung](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items) wird ein ähnliches Problem behandelt (siehe Abschnitt zum Binden an eine Sammlung von Elementen). Unser Beispiel zeigt die folgenden wesentlichen Schritte:

* Erstellen Sie im CodeBehind der Benutzeroberfläche eine Eigenschaft vom Typ **ObservableCollection<T>** für die Kundenobjekte.
* Binden Sie die **ItemSource** von ListView an diese Eigenschaft.
* Stellen Sie eine einfache **ItemTemplate** für ListView bereit. Mit dieser Vorlage wird konfiguriert, wie jedes Element in der Liste angezeigt wird.

Schauen Sie sich gern noch einmal die Dokumente zur [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) an, wenn Sie das Layout anpassen, eine Elementauswahl hinzufügen oder die soeben erstellte **DataTemplate** verbessern möchten. Aber was geschieht, wenn Sie Ihre Kunden bearbeiten möchten?

## <a name="edit-your-customers-through-the-ui"></a>Bearbeiten der Kunden über die Benutzeroberfläche

Sie haben Kunden in einer Liste angezeigt, aber die Datenbindung bietet noch mehr Möglichkeiten. Was wäre, wenn Sie Ihre Daten direkt über die Benutzeroberfläche bearbeiten könnten? Zu diesem Zweck müssen wir zunächst über die drei Modi der Datenbindung sprechen:

* *Einmalig*: Diese Datenbindung wird nur einmal aktiviert. Sie reagiert nicht auf Änderungen.
* *Unidirektional*: Diese Datenbindung aktualisiert die Benutzeroberfläche mit allen Änderungen, die an der Datenquelle vorgenommen werden.
* *Bidirektional*: Diese Datenbindung aktualisiert die Benutzeroberfläche mit allen Änderungen, die an der Datenquelle vorgenommen wurden, und aktualisiert die Daten außerdem mit allen Änderungen, die in der Benutzeroberfläche vorgenommen wurden.

Wenn Sie die Codeausschnitte von vorher befolgt haben, verwendet Ihre Bindung x:Bind und gibt keinen Modus an; folglich handelt es sich um eine einmalige Bindung. Wenn Sie Kunden direkt über die Benutzeroberfläche bearbeiten möchten, müssen Sie die Bindung in eine bidirektionale Bindung ändern, damit die Änderungen an den Daten zurück an die Kundenobjekte übergeben werden. Weitere Informationen finden Sie unter [Datenbindung im Detail](../data-binding/data-binding-in-depth.md).

Die bidirektionale Bindung aktualisiert ebenfalls die Benutzeroberfläche, wenn die Datenquelle geändert wird. Damit dies funktioniert, müssen Sie [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) an der Quelle implementieren und sicherstellen, dass durch die Eigenschaftensetter das **PropertyChanged**-Ereignis ausgelöst wird. Üblicherweise rufen diese eine Hilfsmethode wie **OnPropertyChanged** auf, wie unten dargestellt:

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
Konfigurieren Sie dann den Text in ListView als bearbeitbar, indem Sie **TextBox** anstelle von **TextBlock** verwenden. Stellen Sie sicher, dass Sie **Mode** für die Datenbindungen auf **TwoWay** festlegen.

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

Sie können schnell sicherstellen, dass dies funktioniert, indem Sie eine zweite ListView mit TextBox-Steuerelementen und OneWay-Bindungen hinzufügen. Die Werte in der zweiten Liste werden automatisch geändert, wenn die erste bearbeitet wird.

> [!NOTE]
> Das direkte Bearbeiten innerhalb von ListView ist eine einfache Möglichkeit, die bidirektionale Bindung in Aktion zu zeigen. Dies kann jedoch zu Komplikationen bei der Verwendung führen. Wenn Sie Ihre App weiterentwickeln möchten, können Sie [weitere XAML-Steuerelemente](../design/controls-and-patterns/controls-and-events-intro.md) zum Bearbeiten Ihrer Daten verwenden und ListView schreibgeschützt lassen.

## <a name="going-further"></a>Vertiefung

Nun, da Sie eine Liste von Kunden mit einer bidirektionalen Bindung erstellt haben, können Sie noch einmal die Dokumente durcharbeiten, für die wir Links zur Verfügung gestellt haben, und etwas experimentieren. Sie können auch unser [Datenbindungs-Tutorial](../data-binding/xaml-basics-data-binding.md) durcharbeiten, wenn Sie eine schrittweise Anleitung für die grundlegenden und erweiterten Bindungen erhalten oder sich näher mit Steuerelementen wie dem [Master/Details-Muster](../design/controls-and-patterns/master-details.md) beschäftigen möchten, um eine stabilere Benutzeroberfläche zu erzielen.

## <a name="useful-apis-and-docs"></a>Nützliche APIs und Dokumente

Nachfolgend finden Sie eine kurze Zusammenfassung zu den APIs und weitere nützliche Dokumente, die Sie bei Ihren ersten Schritten rund um die Datenbindung unterstützen.

### <a name="useful-apis"></a>Nützliche APIs

| API | Beschreibung |
|------|---------------|
| [Datenvorlage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | Beschreibt die visuelle Struktur eines Datenobjekts, was die Anzeige bestimmter Elemente in der Benutzeroberfläche ermöglicht. |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | Dokumentation zur empfohlenen x:Bind-Markuperweiterung |
| [Binding](../xaml-platform/binding-markup-extension.md) | Dokumentation zur älteren Binding-Markuperweiterung |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | Ein UI-Steuerelement, das Datenelemente in einem vertikalen Stapel anzeigt |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Ein einfaches Textsteuerelement zum Anzeigen bearbeitbarer Textdaten in der Benutzeroberfläche |
| [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | Schnittstelle, um Daten feststellbar („observable“) zu machen und für eine Datenbindung bereitzustellen |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | Die **ItemsSource**-Eigenschaft dieser Klasse ermöglicht das Binden von ListView an eine Datenquelle. |

### <a name="useful-docs"></a>Nützliche Dokumentation

| Thema | Beschreibung |
|-------|----------------|
| [Datenbindung im Detail](../data-binding/data-binding-in-depth.md) | Eine grundlegende Übersicht über die Prinzipien der Datenbindung |
| [Übersicht über Datenbindung](../data-binding/data-binding-quickstart.md) | Ausführliche konzeptionelle Informationen zur Datenbindung. |
| [Listenansicht](../design/controls-and-patterns/listview-and-gridview.md) | Informationen zum Erstellen und Konfigurieren eines ListView-Steuerelements, einschließlich der Implementierung einer **DataTemplate** |

## <a name="useful-code-samples"></a>Nützliche Codebeispiele

| Codebeispiel | Beschreibung |
|-----------------|---------------|
| [Datenbindungs-Tutorial](../data-binding/xaml-basics-data-binding.md) | Eine schrittweise Anleitung durch die Grundlagen der Datenbindung. |
| [ListView und GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | Informationen zu fortgeschritteneren ListView-Steuerelementen mit Datenbindung |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | Hier sehen Sie die Datenbindung in Aktion, einschließlich der **BindableBase**-Klasse (im Ordner „Common“) für eine Standardimplementierung von **INotifyPropertyChanged**. |
