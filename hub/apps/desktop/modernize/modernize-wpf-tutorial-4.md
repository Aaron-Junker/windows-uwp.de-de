---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: Hinzufügen von Windows 10-Benutzeraktivitäten und Benachrichtigungen
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420120"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und Benachrichtigungen

Dies ist der vierte Teil eines Tutorials, die zeigt, wie Sie eine Beispiel WPF-desktop-app mit dem Namen Contoso-Ausgaben zu modernisieren. Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-app, finden Sie unter [Lernprogramm: Modernisieren von WPF-app](modernize-wpf-tutorial.md). In diesem Artikel wird vorausgesetzt, Sie haben bereits abgeschlossen [Teil 3](modernize-wpf-tutorial-3.md).

In den vorherigen Teilen dieses Tutorials haben Sie, dass die app mithilfe von XAML-Inseln UWP XAML-Steuerelemente hinzugefügt. Als eine nach Produkt dieser, aktiviert Sie auch die app alle WinRT-APIs aufrufen. Dies eröffnet die Möglichkeit für die app viele andere Features von Windows 10, nicht nur UWP XAML-Steuerelemente verwenden.

Das Contoso-Entwicklungsteam hat beschlossen im fiktiven Szenario in diesem Tutorial zwei neue Features der app hinzu: Aktivitäten und Benachrichtigungen. Dieser Teil des Tutorials zeigt, wie diese Funktionen implementiert wird.

## <a name="add-a-user-activity"></a>Fügen Sie eine Benutzeraktivität

In Windows 10 können apps vom Benutzer z. B. das Öffnen einer Datei oder das Anzeigen einer bestimmten Seite ausgeführten Aktivitäten nachverfolgen. Diese Aktivitäten werden dann über ein Feature in Windows 10, Version 1803, der Zeitachse zur Verfügung gestellt Dadurch kann den Benutzer schnell wechseln zurück zu der Vergangenheit und Fortsetzen einer Aktivität, die sie vorher gestartet.

![Abbildung der Zeitachse für Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

Benutzeraktivitäten werden nachverfolgt mithilfe [Microsoft Graph](https://developer.microsoft.com/graph/). Wenn Sie eine Windows 10-app erstellen, müssen Sie jedoch nicht direkt mit der REST-Endpunkte, die von Microsoft Graph bereitgestellte interagieren. Stattdessen können Sie einen geeigneten Satz von WinRT-APIs verwenden. Wir werden diese WinRT-APIs in der Contoso-Ausgaben-app verwenden, zum Nachverfolgen jedes Mal, wenn der Benutzer eine Ausgabe in der app öffnet und mit Adaptive Cards zu verwenden, um Benutzern die Erstellung die Aktivität zu aktivieren.

### <a name="introduction-to-adaptive-cards"></a>Einführung in mit Adaptive Cards

Dieser Abschnitt enthält eine kurze Übersicht über [mit Adaptive Cards](https://docs.microsoft.com/adaptive-cards/). Wenn Sie diese Informationen nicht benötigen, können Sie diesen Schritt überspringen und direkt auf die [hinzufügen eine Adaptive Card](#add-an-adaptive-card) Anweisungen.

Mit Adaptive Cards können Entwickler Inhalt in ein gebräuchlicher und konsistenter Weise auszutauschen. Eine Adaptive Card wird durch eine JSON-Nutzlast beschrieben, die den Inhalt, definiert, der z. Text, Bilder, Aktionen und vieles mehr b.

Eine Adaptive Card definiert nur den Inhalt und nicht die visuelle Darstellung des Inhalts. Die Plattform, der die Adaptive Card empfangen wird, kann den Inhalt mit den am besten geeigneten Stil zu rendern. Stellt die Möglichkeit dar, die mit Adaptive Cards dienen [einen Renderer](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), in der Lage, zu der JSON-Nutzlast und in nativen Benutzeroberfläche konvertieren ist. Beispielsweise kann die Benutzeroberfläche XAML für eine WPF oder UWP-app AXML für eine Android-app oder HTML für eine Website oder einem Bot Chat sein.

Hier ist ein Beispiel für eine einfache Adaptive Card-Nutzlast.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

Die folgende Abbildung zeigt, wie dieses JSON auf verschiedene Arten von ta Teams-Kanal, Cortana und eine Windows-Benachrichtigung gerendert wird.

![Adaptive Card renderingbilds](images/wpf-modernize-tutorial/AdaptiveCards.png)

Mit Adaptive Cards spielen eine wichtige Rolle in der Zeitachse, da es die Möglichkeit besteht, die Windows Aktivitäten rendert. Jede Miniaturansicht in der Zeitachse angezeigt wird, tatsächlich eine Adaptive Card. Wenn Sie vorhaben, um die Aktivität eines Benutzers in Ihrer app zu erstellen, werden Sie aufgefordert, geben Sie eine Adaptive Card aus, um ihn zu rendern.

> [!NOTE]
> Eine hervorragende Möglichkeit, den Entwurf eine Adaptive Card Brainstorming verwendet [online Designer](https://adaptivecards.io/designer/). Sie haben die Möglichkeit, entwerfen die Karte mit Bausteinen (Bildern, Texten, Spalten usw.) und den entsprechenden JSON-Code zu erhalten. Nachdem Sie eine Vorstellung von den endgültigen Entwurf haben, können Sie eine Bibliothek namens [mit Adaptive Cards](https://www.nuget.org/packages/AdaptiveCards/) zu zum Erstellen Ihrer Adaptive Card mit vereinfachen C# Klassen anstelle reiner JSON-Code, die möglicherweise schwer zu debuggen und zu erstellen.

### <a name="add-an-adaptive-card"></a>Fügen Sie eine Adaptive Card

1. Klicken Sie mit der rechten Maustaste auf die **ContosoExpenses.Core** Projekt im Projektmappen-Explorer, und wählen **NuGet-Pakete verwalten**.

2. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Suchen Sie nach der `Newtonsoft.Json` Packen und installieren Sie die neueste verfügbare Version. Dies ist eine beliebte JSON-Bearbeitung-Bibliothek, die Sie verwenden, um Mainipulate helfen, die JSON-Zeichenfolgen mit Adaptive Cards erforderlich.

    ![NewtonSoft.Json NuGet-Paket](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Bei der Installation nicht die `Newtonsoft.Json` separat Verpacken, die mit Adaptive Cards-Bibliothek verweist eine ältere Version von der `Newtonsoft.Json` Paket, das .NET Core 3.0 nicht unterstützt.

2. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Suchen Sie nach der `AdaptiveCards` Packen und installieren Sie die neueste verfügbare Version.

    ![Adaptive Cards NuGet-Paket](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoExpenses.Core** Projekts **hinzufügen ->-Klasse**. Nennen Sie die Klasse **TimelineService.cs** , und klicken Sie auf **OK**.

4. In der **TimelineService.cs** Datei, fügen Sie die folgenden Anweisungen am Anfang der Datei.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Ändern der Namespace deklariert wird, in der Datei aus `ContosoExpenses.Core` zu `ContosoExpenses`.

5. Fügen Sie die folgende Methode der `TimelineService` Klasse.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>Zum code

Diese Methode empfängt ein **Expense** Objekt mit der alle Informationen zu den Aufwand zum Rendern und erstellt ein neues **AdaptiveCard** Objekt. Die Methode hinzugefügt die Karte Folgendes:

- Ein Titel, der die Beschreibung der Ausgaben verwendet.
- Ein Bild, das Contoso-Logo handelt.
- Die Menge der Ausgaben.
- Das Datum der Ausgaben.

Die letzten 3 Elemente werden in zwei verschiedenen Spalten geteilt, damit das Contoso-Logo und die Details zu den Aufwand nebeneinander platziert werden können. Nachdem das Objekt erstellt wurde, gibt die Methode die entsprechende JSON-Zeichenfolge mithilfe von der **ToJson** Methode.

### <a name="define-the-user-activity"></a>Definieren Sie die Aktivitäten von Benutzern

Nun, da Sie die Adaptive Card definiert haben, können Sie eine darauf basierende Benutzeraktivität erstellen.

1. Fügen Sie die folgenden Anweisungen am Anfang **TimelineService.cs** Datei:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Hierbei handelt es sich um UWP-Namespaces. Diese aufgelöst werden, da die `Microsoft.Toolkit.Wpf.UI.Controls` NuGet-Paket, das Sie in Schritt 2 installiert, enthält einen Verweis auf die `Microsoft.Windows.SDK.Contracts` Verpacken, wodurch die **ContosoExpenses.Core** Projekt zu verweisen, WinRT-APIs, obwohl es sich um eine .NET ist 3-Core-Projekt.

2. Fügen Sie die folgenden Felddeklarationen, die `TimelineService` Klasse.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Fügen Sie die folgende Methode der `TimelineService` Klasse.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. Speichern Sie die Änderungen an **TimelineService.cs**.

#### <a name="about-the-code"></a>Zum code

Die `AddToTimeline` Methode ruft zunächst ab einem **UserActivityChannel** -Objekt, das zum Speichern der Benutzeraktivitäten erforderlich ist. Es erstellt einen neuen Benutzer Aktivität mithilfe der **GetOrCreateUserActivityAsync** Methode, die einen eindeutigen Bezeichner benötigt. Auf diese Weise, wenn eine Aktivität vorhanden ist, kann die app es aktualisieren; Andernfalls wird eine neue erstellt. Der Bezeichner, der übergeben hängt davon ab, von der Art der Anwendung, die Sie erstellen:

* Sollten Sie immer die gleiche Aktivität zu aktualisieren, sodass Zeitachse nur der aktuellste Auftrag angezeigt wird, können Sie einen festen Bezeichner (z. B. **Ausgaben**).
* Wenn Sie jede Aktivität als eine andere nachverfolgen möchten, damit Zeitachse, die alle von ihnen angezeigt werden, können Sie einen dynamischen Bezeichner.

In diesem Szenario die app wird überwacht jede geöffnete Kosten als ein anderer Benutzer-Aktivität, damit der Code jede ID erstellt, mit dem Schlüsselwort **Expense -** gefolgt von der eindeutigen Expense-ID.

Nachdem die Methode erstellt eine **UserActivity** Objekt, das Objekt mit den folgenden Informationen aufgefüllt:

* Ein **ActivationUri** , die aufgerufen wird, klickt der Benutzer auf die Aktivität in der Zeitachse. Der Code verwendet ein benutzerdefiniertes Protokoll namens **Contosoexpenses** , die die app wird später behandelt.
* Die **VisualElements** -Objekt, das einen Satz von Eigenschaften enthält, die die visuelle Darstellung der Aktivität zu definieren. Dieser Code legt die **DisplayText** (Dies ist der auf den Eintrag in der Zeitachse angezeigte Titel) und die **Content**. 

Dies ist, in denen die Adaptive Karte, die Sie zuvor definiert eine Rolle spielt. Die app übergibt die Adaptive Card Sie entworfen, dass an die Methode oben als Inhalt. Windows 10 verwendet jedoch ein anderes Objekt zur Darstellung einer Karte im Vergleich zu dem die `AdaptiveCards` NuGet-Paket. Aus diesem Grund erstellt die Methode die Karte mithilfe der **CreateAdaptiveCardFromJson** Methode verfügbar gemacht werden, indem die **AdaptiveCardBuilder** Klasse. Nachdem der Benutzeraktivität die Methode erstellt wurde, speichert die Aktivität und erstellt eine neue Sitzung.

Klickt ein Benutzer auf eine Aktivität in der Zeitachse der **Contosoexpenses: / /** Protokoll aktiviert und die URL enthält die Informationen, die die app den ausgewählten Aufwand abrufen muss. Als eine optionale Aufgabe könnten Sie die protokollaktivierung implementieren, damit, dass die Anwendung richtig reagiert, wenn der Benutzer die Zeitachse verwendet.

### <a name="integrate-the-application-with-timeline"></a>Integrieren Sie die Anwendung in der Zeitachse

Nun, dass Sie mit der Zeitachse eine Klasse, die interagiert erstellt haben, können wir starten, verwenden, um die Benutzeroberfläche der Anwendung zu verbessern. Der beste Ort zum Verwenden der **AddToTimeline** Methode verfügbar gemacht werden, indem die **TimelineService** Klasse ist, wenn der Benutzer auf die Detailseite des Aufwand öffnet.

1. In der **ContosoExpenses.Core** projizieren, erweitern Sie die **ViewModels** Ordner, und Öffnen der **ExpenseDetailViewModel.cs** Datei. Dies ist das "ViewModel", die die Ausgaben Detailfenster unterstützt.

2. Suchen Sie den öffentlichen Konstruktor, der die **ExpenseDetailViewModel** Klasse und fügen Sie den folgenden Code am Ende des Konstruktors. Wenn der Expense-Fenster geöffnet wird, ruft die Methode die **AddToTimeline** Methode und übergeben Sie die aktuelle Kosten. Die **TimelineService** Klasse verwendet diese Informationen, um eine Benutzeraktivität, die mit den Ausgabeinformationen zu erstellen.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Wenn Sie fertig sind, sollte der Konstruktor wie folgt aussehen.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. Drücken Sie F5, um die app im Debugger ausführen. Wählen Sie einen Mitarbeiter aus der Liste aus, und wählen Sie dann eine Ausgabe. Beachten Sie auf der Detailseite die Beschreibung des den Aufwand, der das Datum und der Menge.

4. Drücken Sie **Start + Registerkarte** Zeitachse zu öffnen.

5. Bildlauf nach unten in der Liste der aktuell geöffneten Anwendungen, bis Sie sehen, dass im Abschnitt **heute bereits**. In diesem Abschnitt werden einige der neuesten Benutzeraktivitäten. Klicken Sie auf die **finden Sie alle Aktivitäten** link die **heute bereits** Überschrift.

6. Überprüfen Sie, ob eine neue Karte mit den Informationen über die Kosten, die Sie gerade in der Anwendung ausgewählt haben.

    ![Zeitachse der Contoso-Ausgaben](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Wenn Sie jetzt andere Ausgaben öffnen, sehen Sie eine neue Karten, die als Benutzeraktivitäten hinzugefügt wird. Denken Sie daran, dass der Code verwendet einen anderen Bezeichner für jede Aktivität, damit sie eine Karte für jede Ausgabe erstellt, öffnen Sie in der app.

8. Schließen Sie die App.

## <a name="add-a-notification"></a>Fügen Sie eine Benachrichtigung hinzu

Das zweite Feature, das Contoso-Entwicklungsteam hinzufügen möchte, wird eine Benachrichtigung, die dem Benutzer angezeigt wird, wenn eine neue Ausgabe in der Datenbank gespeichert ist. Zu diesem Zweck können Sie das System integrierte Benachrichtigungen in Windows 10 nutzen, die für Entwickler über WinRT-APIs verfügbar gemacht wird. Diese Benachrichtigungssystem hat viele Vorteile:

- Benachrichtigungen sind konsistent mit dem Rest des Betriebssystems.
- Aktionen erfordernde sind.
- Sie erhalten im Info-Center gespeichert, damit sie später überprüft werden können.

So fügen Sie eine Benachrichtigung an die app hinzu:

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoExpenses.Core** Projekts **hinzufügen ->-Klasse**. Nennen Sie die Klasse **NotificationService.cs** , und klicken Sie auf **OK**.

2. In der **NotificationService.cs** Datei, fügen Sie die folgenden Anweisungen am Anfang der Datei.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Ändern der Namespace deklariert wird, in der Datei aus `ContosoExpenses.Core` zu `ContosoExpenses`.

4. Fügen Sie die folgende Methode der `NotificationService` Klasse.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    Popupbenachrichtigungen werden durch eine XML-Nutzlast dargestellt, die z. Text, Bilder, Aktionen und vieles mehr b. Sie finden alle unterstützten Elemente [hier](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Dieser Code verwendet ein sehr einfaches Schema mit zwei Textzeilen: der Titel und Text. Nachdem der Code die XML-Nutzlast definiert und sie in lädt einer **XmlDocument** Objekt, es dient als Wrapper für die XML-Code in eine **ToastNotification** Objekt aus, und es wird veranschaulicht, indem eine der **ToastNotificationManager** Klasse.

5. In der **ContosoExpenses.Core** projizieren, erweitern Sie die **ViewModels** Ordner, und Öffnen der **AddNewExpenseViewModel.cs** Datei. 

6. Suchen Sie die `SaveExpenseCommand` -Methode, die ausgelöst wird, wenn der Benutzer auf die Schaltfläche drückt, um eine neue Ausgaben zu speichern. Fügen Sie den folgenden Code an diese Methode einfach nach dem Aufruf der `SaveExpense` Methode.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Wenn Sie fertig sind, die `SaveExpenseCommand` Methode sollte wie folgt aussehen.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. Drücken Sie F5, um die app im Debugger ausführen. Wählen Sie einen Mitarbeiter aus der Liste aus, und klicken Sie dann auf die **Hinzufügen von neuen Ausgaben** Schaltfläche. Füllen Sie alle Felder in der Form, und drücken Sie **speichern**.

8. Sie erhalten die folgende Ausnahme aus.

    ![Toast-Benachrichtigungsfehler](images/wpf-modernize-tutorial/ToastNotificationError.png)

Diese Ausnahme wird durch die Tatsache verursacht, die Contoso-Ausgaben-app noch Paketidentität keine. Einige WinRT-APIs, die Benachrichtigungen-API, einschließlich erfordern Paketidentität, bevor sie in einer app verwendet werden können. UWP-apps erhalten Paketidentität standardmäßig, da sie nur über MSIX-Pakete verteilt werden können. Andere Arten von Windows-apps, einschließlich der WPF-apps können auch über MSIX Pakete zum Abrufen der Paketidentität bereitgestellt werden. Der nächste Teil des in diesem Tutorial wird hierzu untersuchen.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt in diesem Tutorial wurde erfolgreich hinzugefügt haben eine Benutzeraktivität der app, die in Windows-Zeitachse integriert ist, und Sie haben auch eine Benachrichtigung an die app, die ausgelöst wird, wenn Benutzer eine neue Ausgabe erstellen hinzugefügt. Die Benachrichtigung funktioniert jedoch noch nicht, da der app Paketidentität verwenden Sie die API-Benachrichtigungen erforderlich sind. Weitere Informationen zum Erstellen eines MSIX-Pakets für die app Paketidentität zu erhalten und andere Bereitstellung profitieren, finden Sie unter [Teil 5: Packen und Bereitstellen mit MSIX](modernize-wpf-tutorial-5.md).
