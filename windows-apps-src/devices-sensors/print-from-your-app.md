---
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: Drucken in Apps
description: Hier erfahren Sie, wie Sie Dokumente in einer Universellen Windows-App drucken. In diesem Thema wird zudem gezeigt, wie bestimmte Seiten gedruckt werden.
ms.date: 01/29/2018
ms.topic: article
keywords: Windows 10, UWP, Drucken
ms.localizationpriority: medium
ms.openlocfilehash: d14a037a84fe64fd9fd3ccca171e3ecfdaaae472
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258634"
---
# <a name="print-from-your-app"></a>Drucken in Apps



**Wichtige APIs**

-   [**Windows. Graphics. Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows. UI. XAML. Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)

Hier erfahren Sie, wie Sie Dokumente in einer Universellen Windows-App drucken. In diesem Thema wird zudem gezeigt, wie bestimmte Seiten gedruckt werden. Informationen zu erweiterten Änderungen an der Benutzeroberfläche für die Druckvorschau finden Sie unter [Anpassen der Benutzeroberfläche für die Druckvorschau](customize-the-print-preview-ui.md).

> [!TIP]
> die meisten der Beispiele in diesem Thema basieren auf dem [universelle Windows-Plattform (UWP) Print Sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing), das Teil des [universelle Windows-Plattform (UWP) app Samples](https://github.com/Microsoft/Windows-universal-samples) -Repository auf GitHub ist.

## <a name="register-for-printing"></a>Registrieren für Druckfunktionen

Um Ihrer App Druckfunktionen hinzuzufügen, müssen Sie sie als Erstes für den Vertrag für "Drucken" registrieren. Die Registrierung ist für jeden Bildschirm erforderlich, auf dem der Benutzer drucken können soll. Es kann jeweils nur der Bildschirm, der gerade angezeigt wird, für das Drucken registriert werden. Wenn ein Bildschirm Ihrer App für das Drucken registriert wurde, muss die Registrierung beim Schließen des Bildschirms aufgehoben werden. Wird an seiner Stelle ein anderer Bildschirm angezeigt, muss dieser nächste Bildschirm beim Öffnen für einen neuen Vertrag für "Drucken" registriert werden.

> [!TIP]
> Wenn Sie das Drucken von mehr als einer Seite in der APP unterstützen müssen, können Sie diesen Druck Code in einer gemeinsamen Hilfsklasse platzieren und die APP-Seiten wieder verwenden. Ein entsprechendes Beispiel finden Sie in der `PrintHelper`-Klasse im [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing).

Geben Sie zuerst die Klassen [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) und [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument) an. Der **PrintManager**-Typ und Typen zur Unterstützung anderer Windows-Druckfunktionen befinden sich im [**Windows.Graphics.Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)-Namespace. Der **PrintDocument**-Typ und andere Typen zur Unterstützung der Vorbereitung von XAML-Inhalten für das Drucken sind im [**Windows.UI.Xaml.Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)-Namespace enthalten. Sie können das Schreiben eines eigenen Druckcodes vereinfachen, indem Sie der Seite die folgenden **using**- oder **Imports**-Anweisungen hinzufügen.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

Die [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)-Klasse wird verwendet, um den Großteil der Interaktion zwischen der App und dem [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) zu behandeln. Sie macht jedoch auch verschiedene eigene Rückrufe verfügbar. Erstellen Sie während der Registrierung Instanzen der Klassen **PrintManager** und **PrintDocument**, und registrieren Sie Handler für deren Druckereignisse.

Im [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) wird die Registrierung von der `RegisterForPrinting`-Methode durchgeführt.

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

Wenn der Benutzer zu einer Seite wechselt, die das Drucken unterstützt, wird die Registrierung innerhalb der `OnNavigatedTo`-Methode initiiert.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

Im Beispiel wird die Registrierung der Ereignishandler in der `UnregisterForPrinting`-Methode aufgehoben.

```csharp
public virtual void UnregisterForPrinting()
{
    if (printDocument == null)
    {
        return;
    }

    printDocument.Paginate -= CreatePrintPreviewPages;
    printDocument.GetPreviewPage -= GetPrintPreviewPage;
    printDocument.AddPages -= AddPrintPages;

    PrintManager printMan = PrintManager.GetForCurrentView();
    printMan.PrintTaskRequested -= PrintTaskRequested;
}
```

Wenn der Benutzer eine Seite verlässt, die das Drucken unterstützt, wird die Registrierung der Ereignishandler innerhalb der `OnNavigatedFrom` Methode aufgehoben. 

> [!NOTE]
> Wenn Sie über eine APP mit mehreren Seiten verfügen und den Druck nicht trennen, wird eine Ausnahme ausgelöst, wenn der Benutzer die Seite verlässt und dann wieder zurückgibt.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```

## <a name="create-a-print-button"></a>Erstellen einer Druckschaltfläche

Fügen Sie eine Druckschaltfläche an der gewünschten Stelle auf dem App-Bildschirm hinzu. Stellen Sie sicher, dass sie den zu druckenden Inhalt nicht verdeckt oder anderweitig stört.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

Als Nächstes fügen Sie einen Ereignishandler zum Behandeln des Click-Ereignisses zum Code Ihrer App hinzu. Verwenden Sie die [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printmanager.showprintuiasync)-Methode, um das Drucken über Ihre App zu starten. **ShowPrintUIAsync** ist eine asynchrone Methode, die das entsprechende Druckfenster anzeigt. Wir empfehlen zunächst den Aufruf der [**IsSupported**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printmanager.issupported)-Methode, um zu prüfen, ob die App auf einem gerät ausgeführt wird, das den Druck unterstützt (und den Fall behandelt, wenn dies nicht der Fall ist). Wenn das Drucken zu diesem Zeitpunkt aus einem anderen Grund nicht ausgeführt werden kann, gibt**ShowPrintUIAsync** eine Ausnahme aus. Es wird empfohlen, die Ausnahmen abzufangen und dem Benutzer mitzuteilen, wenn der Druckvorgang nicht fortgesetzt werden kann.

In diesem Beispiel wird ein Druckfenster im Ereignishandler für das Klicken auf eine Schaltfläche angezeigt. Wenn die Methode eine Ausnahme auslöst (da das Drucken zu diesem Zeitpunkt nicht möglich ist), informiert ein [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)-Steuerelement den Benutzer über die Situation.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

## <a name="format-your-apps-content"></a>Formatieren der App-Inhalte

Wenn **ShowPrintUIAsync** aufgerufen wird, wird das [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress)-Ereignis ausgelöst. Der in diesem Schritt gezeigte **PrintTaskRequested**-Ereignishandler erstellt eine [**PrintTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintTask)-Klasse, indem er die [**PrintTaskRequest.CreatePrintTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskrequest.createprinttask)-Methode aufruft und den Titel für die zu druckende Seite sowie den Namen eines [**PrintTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.source) -Delegaten übergibt. Beachten Sie, dass **PrintTaskSourceRequestedHandler** in diesem Beispiel inline definiert wird. **PrintTaskSourceRequestedHandler** stellt den formatierten Inhalt für das Drucken bereit und wird an späterer Stelle beschrieben.

In diesem Beispiel wurde außerdem ein Abschlusshandler definiert, um Fehler aufzufangen. Es empfiehlt sich, Abschlussereignisse zu behandeln, da die App den Benutzer dann über aufgetretene Fehler und mögliche Lösungen informieren kann. Die App kann das Abschlussereignis auch verwenden, um nachfolgende Schritte anzugeben, die der Benutzer nach einem erfolgreichen Druckauftrag ausführen kann.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

Nachdem die Druckaufgabe erstellt wurde, löst der [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) das [**Paginate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.paginate)-Ereignis aus, um eine Sammlung zu druckender Seiten anzufordern, die auf der Druckvorschau-Benutzeroberfläche angezeigt werden. Dies entspricht der **Paginate**-Methode der **IPrintPreviewPageCollection**-Schnittstelle. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

> [!IMPORTANT]
> wenn der Benutzer die Druckeinstellungen ändert, wird der paginieren-Ereignishandler erneut aufgerufen, damit Sie den Inhalt erneut ausführen können. Für die bestmögliche Benutzerfreundlichkeit wird empfohlen, die Einstellungen zu überprüfen, bevor Sie den Inhalt umbrechen und die erneute Initialisierung der auf Seiten aufgeteilten Inhalte vermeiden, wenn dies nicht erforderlich ist.

Im [**Paginate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.paginate)-Ereignishandler (der `CreatePrintPreviewPages`-Methode im [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)) erstellen Sie die Seiten, die auf der Druckvorschau-Benutzeroberfläche angezeigt und an den Drucker gesendet werden. Der Code zum Vorbereiten der App-Inhalte für den Druck muss speziell an Ihre App und die gedruckten Inhalte angepasst werden. Im Quellcode für das [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) können Sie sehen, wie der Inhalt für das Drucken formatiert wird.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

Wenn eine bestimmte Seite in der Druckvorschau angezeigt werden soll, löst [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) das [**GetPreviewPage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage)-Ereignis aus. Dies entspricht der **MakePage**-Methode der **IPrintPreviewPageCollection**-Schnittstelle. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

Legen Sie im [**GetPreviewPage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.getpreviewpage)-Ereignishandler (der `GetPrintPreviewPage`-Methode im [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)) die entsprechende Seite des Druckdokuments fest.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

Nachdem der Benutzer auf die Druckschaltfläche geklickt hat, fordert [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager) die abschließende Sammlung der an den Drucker zu sendenden Seiten an, indem die **MakeDocument**-Methode der **IDocumentPageSource**-Schnittstelle aufgerufen wird. In XAML löst dies das [**AddPages**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.addpages)-Ereignis aus. Der Ereignishandler, den Sie bei der Registrierung erstellt haben, wird zu diesem Zeitpunkt aufgerufen.

Im [**AddPages**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.addpages)-Ereignishandler (der `AddPrintPages`-Methode im [UWP-Druckbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)) fügen Sie dem [**PrintDocument**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing.PrintDocument)-Objekt, das an den Drucker gesendet werden soll, Seiten aus der Seitensammlung hinzu. Wenn ein Benutzer bestimmte Seiten oder einen Seitenbereich zum Drucken angibt, verwenden Sie diese Informationen, um nur die Seiten hinzuzufügen, die tatsächlich an den Drucker gesendet werden.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>Vorbereiten von Druckoptionen

Als Nächstes bereiten Sie Druckoptionen vor. Dieser Abschnitt beschreibt als Beispiel, wie die Seitenbereichsoption festgelegt wird, um das Drucken bestimmter Seiten zu gestatten. Weitere Optionen finden Sie unter [Anpassen der Benutzeroberfläche für die Druckvorschau](customize-the-print-preview-ui.md).

In diesem Schritt wird eine neue Druckoption erstellt und eine Liste von Werten definiert, die die Option unterstützt. Dann wird die Option der Druckvorschau-UI hinzugefügt. Die Seitenbereichsoption hat folgende Einstellungen:

| Optionsname          | Aktion |
|----------------------|--------|
| **Alle drucken**        | Druckt alle Seiten im Dokument.|
| **Auswahl drucken**  | Druckt nur den vom Benutzer ausgewählten Inhalt.|
| **Druckbereich**      | Zeigt ein Bearbeitungssteuerelement an, in das der Benutzer die zu druckenden Seiten eingeben kann.|

Ändern Sie zunächst den [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress)-Ereignishandler, um den Code zum Abrufen eines [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails)-Objekts hinzuzufügen.

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

Löschen Sie die Liste der Optionen, die in der Druckvorschau-Benutzeroberfläche angezeigt werden, und fügen Sie die Optionen hinzu, die angezeigt werden sollen, wenn der Benutzer in der App druckt.

> [!NOTE]
> die Optionen in der Benutzeroberfläche der Seitenansicht in derselben Reihenfolge angezeigt, in der Sie angefügt werden, und die erste Option wird oben im Fenster angezeigt.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

Erstellen Sie die neue Druckoption, und initialisieren Sie die Liste der Optionswerte.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

Fügen Sie Ihre benutzerdefinierte Druckoption hinzu, und weisen Sie den Ereignishandler zu. Die benutzerdefinierte Option wird als Letztes hinzugefügt, sodass sie am Ende der Optionsliste erscheint. Sie können sie aber an einer beliebigen Stelle der Liste platzieren. Benutzerdefinierte Druckoptionen müssen nicht zuletzt hinzugefügt werden.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

Die [**CreateTextOption**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption)-Methode erstellt das Textfeld **Bereich**. Hier gibt der Benutzer die zu druckenden Seiten ein, wenn die Option **Druckbereich** ausgewählt wurde.

## <a name="handle-print-option-changes"></a>Behandeln von Druckoptionsänderungen

Der **OptionChanged**-Ereignishandler ist im Wesentlichen für zwei Dinge zuständig. Erstens blendet er das Texteingabefeld für den Seitenbereich je nach der vom Benutzer ausgewählten Seitenbereichsoption ein und aus. Zweitens testet er den Text, den der Benutzer in das Seitenbereich-Textfeld eingibt, um sicherzustellen, dass es sich um einen gültigen Seitenbereich für das Dokument handelt.

Dieses Beispiel zeigt, wie die Änderungs Ereignisse von Druckoptionen im [Beispiel für den UWP-Druck](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)behandelt werden.

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> die `GetPagesInRange`-Methode im [UWP-Druck Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) anzuzeigen, finden Sie ausführliche Informationen zum Analysieren des Seitenbereichs, den der Benutzer in das Textfeld "Bereich" eingibt.

## <a name="preview-selected-pages"></a>Vorschau auf ausgewählte Seiten

Die Formatierung des Inhalts Ihrer App zum Drucken hängt von der Art der App und deren Inhalt ab. Eine druckhilfsklasse, die im [UWP-Druck Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) verwendet wird, um den Inhalt für den Druck zu formatieren.

Beim Drucken einer Teilmenge von Seiten gibt es mehrere Möglichkeiten, den Inhalt in der Seitenansicht anzuzeigen. Unabhängig davon, welche Methode Sie zum Anzeigen des Seitenbereichs in der Druckvorschau ausgewählt haben, darf der Ausdruck nur die ausgewählten Seiten enthalten.

-   Alle Seiten in der Druckvorschau anzeigen, unabhängig davon, ob ein Seitenbereich festgelegt wurde. Der Benutzer muss selbst wissen, welche Seiten ausgedruckt werden.
-   Nur den vom Benutzer ausgewählten Seitenbereich in der Druckvorschau anzeigen und die Anzeige aktualisieren, wenn der Benutzer den Seitenbereich ändert.
-   Alle Seiten in der Druckvorschau anzeigen, dabei aber alle Seiten ausgrauen, die nicht in dem vom Benutzer ausgewählten Seitenbereich liegen.

## <a name="related-topics"></a>Verwandte Themen

* [Entwurfs Richtlinien für das Drucken](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [Build 2015-Video: Entwickeln von apps, die in Windows 10 gedruckt werden](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Beispiel für UWP-Druck](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)
