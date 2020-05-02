---
description: In diesem Tutorial wird veranschaulicht, wie du UWP-XAML-Benutzeroberflächen hinzufügst, MSIX-Pakete erstellst und weitere moderne Komponenten in deine WPF-App integrierst.
title: Hinzufügen von Windows 10-Benutzeraktivitäten und -Benachrichtigungen
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "67420120"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Teil 4: Hinzufügen von Windows 10-Benutzeraktivitäten und -Benachrichtigungen

Dies ist der vierte Teil eines Tutorials, in dem das Modernisieren der WPF-Beispiel-Desktop-App Contoso Expenses veranschaulicht wird. Eine Übersicht über das Tutorial, die Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App findest du unter [Tutorial: Modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass du [Teil 3](modernize-wpf-tutorial-3.md) bereits abgeschlossen hast.

In den vorherigen Abschnitten dieses Tutorials hast du der App mithilfe von XAML Islands UWP-XAML-Steuerelemente hinzugefügt. Als Nebeneffekt hast du der App auch ermöglicht, jede beliebige WinRT-API aufzurufen. Dies eröffnet der App die Möglichkeit, viele andere Features von Windows 10 zu nutzen, nicht nur die UWP XAML-Steuerelemente.

Im fiktiven Szenario dieses Tutorials hat das Entwicklungsteam von Contoso beschlossen, der App zwei neue Features hinzuzufügen: Aktivitäten und Benachrichtigungen. In diesem Teil des Tutorials wird gezeigt, wie diese Features implementiert werden.

## <a name="add-a-user-activity"></a>Hinzufügen einer Benutzeraktivität

Unter Windows 10 können Apps die vom Benutzer ausgeführten Aktivitäten wie das Öffnen einer Datei oder das Anzeigen einer bestimmten Seite nachverfolgen. Diese Aktivitäten werden dann über die Zeitachse zur Verfügung gestellt, ein Feature, das in der Windows 10-Version 1803 eingeführt wurde und dem Benutzer ermöglicht, schnell in die Vergangenheit zurückzugehen und eine zuvor begonnene Aktivität fortzusetzen.

![Abbildung der Windows-Zeitachse](images/wpf-modernize-tutorial/WindowsTimeline.png)

Benutzeraktivitäten werden mithilfe von [Microsoft Graph](https://developer.microsoft.com/graph/) nachverfolgt. Wenn du jedoch eine Windows 10-App entwickelst, musst du nicht direkt mit den REST-Endpunkten von Microsoft Graph interagieren. Stattdessen kannst du einen praktischen Satz von WinRT-APIs verwenden. Wir verwenden diese WinRT-APIs in der App Contoso Expenses, um jedes Mal zu verfolgen, wenn der Benutzer eine Ausgabe innerhalb der App öffnet. Außerdem nutzen wir adaptive Karten, um Benutzern das Erstellen der Aktivität zu ermöglichen.

### <a name="introduction-to-adaptive-cards"></a>Einführung in adaptive Karten

Dieser Abschnitt enthält eine kurze Einführung in [adaptive Karten](https://docs.microsoft.com/adaptive-cards/). Wenn du diese Informationen nicht benötigst, kannst du sie überspringen und direkt zu den Anweisungen zum [Hinzufügen einer adaptiven Karte](#add-an-adaptive-card) wechseln.

Mit adaptiven Karten können Entwickler Karteninhalte auf einheitliche Weise austauschen. Eine adaptive Karte wird durch eine JSON-Nutzlast beschrieben, die ihren Inhalt definiert, der Text, Bilder, Aktionen und mehr umfassen kann.

Eine adaptive Karte definiert lediglich den Inhalt und nicht die visuelle Darstellung des Inhalts. Die Plattform, auf der die adaptive Karte empfangen wird, kann den Inhalt mit dem am besten geeigneten Layout rendern. Adaptive Karten werden durch [ einen Renderer](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started) gestaltet, der in der Lage ist, die JSON-Nutzlast in eine native Benutzeroberfläche zu konvertieren. Die Benutzeroberfläche kann beispielsweise XAML für eine WPF- oder UWP-Anwendung, AXML für eine Android-Anwendung oder HTML für eine Website oder einen Botchat sein.

Hier ist ein Beispiel der Nutzlast einer einfachen adaptiven Karte.

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

Das folgende Bild zeigt, wie dieser JSON-Codeausschnitt durch einen Teams-Kanal, Cortana und eine Windows-Benachrichtigung auf unterschiedliche Weise gerendert wird.

![Abbildung des Renderings einer adaptiven Karte](images/wpf-modernize-tutorial/AdaptiveCards.png)

Adaptive Karten spielen eine wichtige Rolle für die Zeitachse, da sie bestimmen, wie Windows Aktivitäten rendert. Jede auf der Zeitachse angezeigte Miniaturansicht ist im Grunde eine adaptive Karte. Wenn du also eine Benutzeraktivität innerhalb deiner App erstellst, wirst du gebeten, eine adaptive Karte bereitzustellen, um sie zu rendern.

> [!NOTE]
> Eine gute Möglichkeit, das Design einer adaptiven Karte zu entwerfen, stellt der [Online-Designer](https://adaptivecards.io/designer/) dar. Du hast die Möglichkeit, die Karte mit Bausteinen (Bildern, Texten, Spalten usw.) zu gestalten und den entsprechenden JSON-Code abzurufen. Sobald du eine Vorstellung vom endgültigen Design hast, kannst du eine Bibliothek namens [Adaptive Karten](https://www.nuget.org/packages/AdaptiveCards/) verwenden. Damit wird es einfacher, deine adaptive Karte unter Verwendung von C#-Klassen anstatt von einfachem JSON-Code zu entwickeln, der schwer zu debuggen und zu kompilieren sein kann.

### <a name="add-an-adaptive-card"></a>Hinzufügen einer adaptiven Karte

1. Klicke im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **NuGet-Pakete verwalten** aus.

2. Klicke im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suche das Paket `Newtonsoft.Json`, und installiere die neueste verfügbare Version. Dies ist eine beliebte Bibliothek zur Bearbeitung von JSON, mit deren Hilfe du die von adaptiven Karten benötigten JSON-Zeichenfolgen bearbeiten kannst.

    ![NuGet-Paket NewtonSoft.Json](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Wenn du das Paket `Newtonsoft.Json` nicht separat installierst, verweist die Bibliothek „Adaptive Karten“ auf eine ältere Version des Pakets `Newtonsoft.Json`, das .NET Core 3.0 nicht unterstützt.

2. Klicke im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suche das Paket `AdaptiveCards`, und installiere die neueste verfügbare Version.

    ![NuGet-Paket „Adaptive Karten“](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **Hinzufügen > Klasse** aus. Nenne die Klasse **TimelineService.cs**, und klicke auf **OK**.

4. Füge am Anfang der Datei **TimelineService.cs** die folgenden Anweisungen hinzu.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Ändere den in der Datei deklarierten Namespace von `ContosoExpenses.Core` in `ContosoExpenses`.

5. Füge der `TimelineService`-Klasse die folgende Methode hinzu.

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

#### <a name="about-the-code"></a>Informationen zum Code

Diese Methode empfängt ein **Expense**-Objekt mit allen Informationen zur zu rendernden Ausgabe und erstellt ein neues **AdaptiveCard**-Objekt. Die Methode fügt der Karte Folgendes hinzu:

- Einen Titel, der die Beschreibung der Ausgabe verwendet.
- Ein Bild, bei dem es sich um das Contoso-Logo handelt.
- Den Betrag der Ausgabe.
- Das Datum der Ausgabe.

Die letzten drei Elemente sind in zwei Spalten aufgeteilt, sodass das Contoso-Logo und die Angaben zu den Ausgaben nebeneinander platziert werden können. Nachdem das Objekt erstellt wurde, gibt die Methode die entsprechende JSON-Zeichenfolge mithilfe der **ToJson**-Methode zurück.

### <a name="define-the-user-activity"></a>Definieren der Benutzeraktivität

Nachdem du die adaptive Karte definiert hast, kannst du nun eine Benutzeraktivität auf Basis dieser Karte erstellen.

1. Füge am Anfang der Datei **TimelineService.cs** die folgenden Anweisungen hinzu:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Dabei handelt es sich um UWP-Namespaces. Diese werden aufgelöst, weil das NuGet-Paket `Microsoft.Toolkit.Wpf.UI.Controls`, das du in Schritt 2 installiert hast, einen Verweis auf das Paket `Microsoft.Windows.SDK.Contracts` enthält. Dies ermöglicht dem Projekt **ContosoExpenses.Core**, auf WinRT-APIs zu verweisen, obwohl es sich um ein .NET Core 3-Projekt handelt.

2. Füge der `TimelineService`-Klasse die folgenden Felddeklarationen hinzu.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Füge der `TimelineService`-Klasse die folgende Methode hinzu.

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

4. Speichere die Änderungen an **TimelineService.cs**.

#### <a name="about-the-code"></a>Informationen zum Code

Die `AddToTimeline`-Methode ruft zunächst ein **UserActivityChannel**-Objekt ab, das zum Speichern von Benutzeraktivitäten erforderlich ist. Dann erstellt sie eine neue Benutzeraktivität mit der **GetOrCreateUserActivityAsync**-Methode, die einen eindeutigen Bezeichner erfordert. Auf diese Weise kann die App, wenn eine Aktivität bereits vorhanden ist, diese aktualisieren. Andernfalls wird eine neue Aktivität erstellt. Der zu übergebende Bezeichner hängt von der Art der Anwendung ab, die du entwickelst:

* Wenn du immer dieselbe Aktivität aktualisieren möchtest, sodass auf der Zeitachse nur die jüngste Aktivität angezeigt wird, kannst du einen festen Bezeichner verwenden (z. B. **Expenses**).
* Wenn du jede Aktivität einzeln so verfolgen möchten, dass auf der Zeitachse alle Aktivitäten angezeigt werden, kannst du einen dynamischen Bezeichner verwenden.

In diesem Szenario verfolgt die App alle geöffneten Ausgaben als unterschiedliche Benutzeraktivitäten, sodass der Code jeden Bezeichner mit dem Schlüsselwort **Expense-** , gefolgt von der eindeutigen Ausgaben-ID, erstellt.

Nachdem die Methode ein **UserActivity**-Objekt erstellt hat, füllt sie das Objekt mit den folgenden Informationen auf:

* Einem **ActivationUri**, der aufgerufen wird, wenn der Benutzer auf der Zeitachse auf die Aktivität klickt. Im Code wird ein benutzerdefiniertes Protokoll namens **contosoexpenses** verwendet, das die Anwendung später verarbeiten wird.
* Dem Objekt **VisualElements**, das eine Reihe von Eigenschaften enthält, die die visuelle Darstellung der Aktivität definieren. Dieser Code legt **DisplayText** (Anzeigetext des Titels, der über dem Eintrag auf der Zeitachse angezeigt wird) und **Content** (Inhalt) fest. 

Hier spielt die von dir zuvor definierte adaptive Karte eine Rolle. Die App gibt die von dir zuvor entworfene adaptive Karte als Inhalt an die Methode weiter. Allerdings verwendet Windows 10 zur Darstellung einer Karte ein anderes Objekt als das vom NuGet-Paket `AdaptiveCards` verwendete. Daher erstellt die Methode die Karte neu, indem sie die Methode **CreateAdaptiveCardFromJson** verwendet, die durch die **AdaptiveCardBuilder**-Klasse verfügbar gemacht wird. Nachdem die Methode die Benutzeraktivität erstellt hat, speichert sie die Aktivität und erstellt eine neue Sitzung.

Wenn ein Benutzer auf der Zeitachse auf eine Aktivität klickt, wird das Protokoll **contosoexpenses://** aktiviert. Die URL enthält die Informationen, die die App zum Abrufen der ausgewählten Ausgabe benötigt. Als optionale Aufgabe kannst du die Protokollaktivierung implementieren, sodass die Anwendung ordnungsgemäß reagiert, wenn der Benutzer die Zeitachse verwendet.

### <a name="integrate-the-application-with-timeline"></a>Integrieren der Anwendung in die Zeitachse

Nachdem du nun eine Klasse erstellt hast, die mit der Zeitachse interagiert, können wir damit beginnen, sie zum Verbessern der Benutzeroberfläche der Anwendung zu verwenden. Die beste Stelle für die Verwendung der **AddToTimeline**-Methode, die durch die **TimelineService**-Klasse verfügbar gemacht wird, ist, wenn der Benutzer die Detailseite einer Ausgabe öffnet.

1. Erweitere im Projekt **ContosoExpenses.Core** den Ordner **ViewModels**, und öffne die Datei **ExpenseDetailViewModel.cs**. Dies ist das ViewModel zur Unterstützung des Fensters mit den Details zur Ausgabe.

2. Wechsle zum öffentlichen Konstruktor der **ExpenseDetailViewModel**-Klasse, und füge am Ende des Konstruktors den folgenden Code hinzu. Immer wenn das Ausgabenfenster geöffnet wird, ruft die Methode die **AddToTimeline**-Methode auf und übergibt die aktuelle Ausgabe. Die **TimelineService**-Klasse nutzt diese Informationen, um eine Benutzeraktivität unter Verwendung der Ausgabeninformationen zu erstellen.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Wenn du fertig bis, sollte der Konstruktor so aussehen.

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

3. Drücke F5, um die App zu kompilieren und im Debugger auszuführen. Wähle einen Mitarbeiter in der Liste und dann eine Ausgabe aus. Achte auf der Detailseite auf die Beschreibung der Ausgabe, das Datum und den Betrag.

4. Drücke **START+TAB**, um die Zeitachse zu öffnen.

5. Scrolle in der Liste der derzeit geöffneten Anwendungen nach unten, bis du den Abschnitt mit dem Titel **Vor ein paar Stunden** siehst. In diesem Abschnitt werden einige der jüngsten Benutzeraktivitäten angezeigt. Klicke neben der Überschrift **Vor ein paar Stunden** auf **Alle Aktivitäten anzeigen**.

6. Bestätige, dass du eine neue Karte mit den Informationen zu den soeben in der Anwendung ausgewählten Ausgabe siehst.

    ![Zeitachse von Contoso Expenses](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Wenn du jetzt andere Ausgaben öffnest, siehst du, dass neue Karten als Benutzeraktivitäten hinzugefügt werden. Denke daran, dass der Code für jede Aktivität einen anderen Bezeichner verwendet, sodass für jede Ausgabe, die du in der App öffnest, eine Karte erstellt wird.

8. Schließen Sie die App.

## <a name="add-a-notification"></a>Hinzufügen einer Benachrichtigung

Das zweite Feature, das das Contoso-Entwicklungsteam hinzufügen möchte, ist eine Benachrichtigung, die dem Benutzer angezeigt wird, wenn eine neue Ausgabe in der Datenbank gespeichert wird. Dazu kannst du das in Windows 10 integrierte Benachrichtigungssystem nutzen, das Entwicklern über WinRT-APIs zur Verfügung steht. Dieses Benachrichtigungssystem hat zahlreiche Vorteile:

- Benachrichtigungen sind mit dem Rest des Betriebssystems konsistent.
- Sie sind umsetzbar.
- Sie werden im Info-Center gespeichert, damit sie später überprüft werden können.

So fügst du der App eine Benachrichtigung hinzu

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wähle **Hinzufügen > Klasse** aus. Nenne die Klasse **NotificationService.cs**, und klicke auf **OK**.

2. Füge am Anfang der Datei **NotificationService.cs** die folgenden Anweisungen hinzu.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Ändere den in der Datei deklarierten Namespace von `ContosoExpenses.Core` in `ContosoExpenses`.

4. Füge der `NotificationService`-Klasse die folgende Methode hinzu.

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

    Popupbenachrichtigungen werden durch eine XML-Nutzlast dargestellt, die Text, Bilder, Aktionen und vieles mehr enthalten kann. [Hier](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema) findest du alle unterstützten Elemente. Dieser Code verwendet ein sehr einfaches Schema mit zwei Textzeilen: Titel und Textkörper. Nachdem der Code die XML-Nutzlast definiert und in ein **XmlDocument**-Objekt geladen hat, umschließt er den XML-Code in einem **ToastNotification**-Objekt und zeigt es mithilfe der **ToastNotificationManager**-Klasse an.

5. Erweitere im Projekt **ContosoExpenses.Core** den Ordner **ViewModels**, und öffne die Datei **AddNewExpenseViewModel.cs**. 

6. Wechsle zur `SaveExpenseCommand`-Methode, die ausgelöst wird, wenn der Benutzer auf die Schaltfläche klickt, um eine neue Ausgabe zu speichern. Füge den folgenden Code dieser Methode direkt nach dem Aufruf der `SaveExpense`-Methode hinzu.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Wenn du fertig bis, sollte die `SaveExpenseCommand`-Methode so aussehen.

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

7. Drücke F5, um die App zu kompilieren und im Debugger auszuführen. Wähle einen Mitarbeiter in der Liste aus, und klicke dann auf die Schaltfläche **Add new expense** (Neue Ausgabe hinzufügen). Fülle alle Felder im Formular aus, und klicke auf **Speichern**.

8. Du erhältst die folgende Ausnahme.

    ![Fehler bei Popupbenachrichtigung](images/wpf-modernize-tutorial/ToastNotificationError.png)

Diese Ausnahme wird dadurch verursacht, dass die App Contoso Expenses noch keine Paketidentität hat. Einige WinRT-APIs (einschließlich der Benachrichtigungs-API) erfordern eine Paketidentität, bevor sie in einer App verwendet werden können. UWP-Apps erhalten standardmäßig eine Paketidentität, da sie nur über MSIX-Pakete verteilt werden können. Andere Typen von Windows-Apps, einschließlich WPF-Apps, können ebenfalls über MSIX-Pakete bereitgestellt werden, um eine Paketidentität zu erhalten. Im nächsten Teil dieses Tutorials erfährst du, wie das geht.

## <a name="next-steps"></a>Nächste Schritte

An dieser Stelle im Tutorial hast du erfolgreich eine Benutzeraktivität zur App hinzugefügt, die in die Windows-Zeitachse integriert ist. Außerdem hast du der App eine Benachrichtigung hinzugefügt, die ausgelöst wird, wenn Benutzer eine neue Ausgabe erstellen. Die Benachrichtigung funktioniert jedoch noch nicht, da die Anwendung zur Verwendung der Benachrichtigungs-API eine Paketidentität erfordert. Informationen dazu, wie ein MSIX-Paket für die Anwendung so erstellt wird, dass es eine Paketidentität erhält, und du in den Genuss anderer Bereitstellungsvorteile kommst, findest du in [Teil 5: Packen und Bereitstellen mit MSIX](modernize-wpf-tutorial-5.md).
