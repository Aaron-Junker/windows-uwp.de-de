---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: Anpassen der Benutzeroberfläche für die Druckvorschau
description: In diesem Abschnitt wird beschrieben, wie die Druckoptionen und -einstellungen in der Benutzeroberfläche für die Druckvorschau angepasst werden.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, printing
ms.localizationpriority: medium
ms.openlocfilehash: 000985d5f9dac5363a1ea2fb002c2be40e2777dd
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258656"
---
# <a name="customize-the-print-preview-ui"></a>Anpassen der Benutzeroberfläche für die Druckvorschau



**Wichtige APIs**

-   [**Windows.Graphics.Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager)

In diesem Abschnitt wird beschrieben, wie die Druckoptionen und -einstellungen in der Benutzeroberfläche für die Druckvorschau angepasst werden. Weitere Informationen zur Druckfunktion finden Sie unter [Drucken in Apps](print-from-your-app.md).

**Tip**  Most of the examples in this topic are based on the print sample. Laden Sie das [Druckbeispiel für die universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing) aus dem Repository [Beispiele für Universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples) auf GitHub herunter, um den vollständigen Code anzuzeigen.

 

## <a name="customize-print-options"></a>Anpassen der Druckoptionen

Standardmäßig werden in der Benutzeroberfläche für die Druckvorschau die Druckoptionen [**ColorMode**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode), [**Copies**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies) und [**Orientation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation) angezeigt. Neben diesen Optionen sind weitere allgemeine Druckeroptionen verfügbar, die Sie der Benutzeroberfläche für die Druckvorschau hinzufügen können:

-   [**Binding**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**Collation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**Duplex**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**MediaType**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**NUp**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**PrintQuality**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**Staple**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

Diese Optionen werden in der [**StandardPrintTaskOptions**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions)-Klasse definiert. Sie können in der Optionsliste, die in der Druckvorschau-Benutzeroberfläche angezeigt wird, Optionen hinzufügen oder entfernen. Sie können auch die Reihenfolge, in der die Optionen angezeigt werden, und die für den Benutzer angezeigten Standardeinstellungen ändern.

Die Änderungen, die Sie auf diese Weise vornehmen, betreffen allerdings nur die Druckvorschau-Benutzeroberfläche. Der Benutzer kann stets auf alle vom Drucker unterstützten Optionen zugreifen, indem er in der Druckvorschau-Benutzeroberfläche auf **Weitere Einstellungen** tippt.

**Note**  Although your app can specify any print options to be displayed, only those that are supported by the selected printer are shown in the print preview UI. In der Druckbenutzeroberfläche werden keine Optionen angezeigt, die der ausgewählte Drucker nicht unterstützt.

 

### <a name="define-the-options-to-display"></a>Definieren der anzuzeigenden Optionen

Wenn der Bildschirm der App geladen wird, wird die App für den Vertrag für „Drucken“ registriert. Im Rahmen dieser Registrierung wird der [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress)-Ereignishandler definiert. Der Code zum Anpassen der in der Druckvorschau-Benutzeroberfläche angezeigten Optionen wird dem **PrintTaskRequested**-Ereignishandler hinzugefügt.

Ändern Sie den [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress)-Ereignishandler, um die [**printTask.options**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.options)-Anweisungen einzubeziehen, mit denen die Druckeinstellungen konfiguriert werden, die Sie auf der Benutzeroberfläche für die Druckvorschau anzeigen möchten. Überschreiben Sie für den Bildschirm Ihrer App, in dem Sie eine benutzerdefinierte Liste von Druckoptionen anzeigen möchten, den **PrintTaskRequested**-Ereignishandler in der Basisklasse, um Code hinzuzufügen, der die Optionen angibt, die bei der Ausgabe des Bildschirms angezeigt werden sollen.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**Important**  Calling [**displayedOptions.clear**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions)() removes all of the print options from the print preview UI, including the **More settings** link. Fügen Sie alle Optionen an, die in der Druckvorschau-Benutzeroberfläche angezeigt werden sollen.

### <a name="specify-default-options"></a>Festlegen der Standardoptionen

Sie können auch die Standardwerte der Optionen in der Druckvorschau-Benutzeroberfläche festlegen. Die folgende Codezeile aus dem letzten Beispiel legt den Standardwert der Option [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize) fest.

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>Hinzufügen neuer Druckoptionen

In diesem Abschnitt werden die Erstellung einer neuen Druckoption, die Definition einer Liste von Werten, die von dieser Option unterstützt werden, und das Hinzufügen der Option zur Druckvorschau gezeigt. Fügen Sie die Druckoption wie im vorherigen Abschnitt gezeigt im [**PrintTaskRequested**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress)-Ereignishandler hinzu.

Rufen Sie zunächst ein [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails)-Objekt ab. Dies wird verwendet, um die neue Druckoption zur Benutzeroberfläche für die Druckvorschau hinzuzufügen. Löschen Sie dann die Liste der Optionen, die in der Druckvorschau-Benutzeroberfläche angezeigt werden, und fügen Sie die Optionen hinzu, die angezeigt werden sollen, wenn der Benutzer in der App druckt. Anschließend erstellen Sie die neue Druckoption und initialisieren die Liste der Optionswerte. Abschließend fügen Sie die neue Option hinzu und weisen dem **OptionChanged**-Ereignis einen Handler zu.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

Die Optionen werden in der Druckvorschau-Benutzeroberfläche in der Reihenfolge angezeigt, in der sie hinzugefügt werden, wobei die erste Option oben im Fenster erscheint. In diesem Beispiel wird die benutzerdefinierte Option als Letztes hinzugefügt, sodass sie am Ende der Optionsliste erscheint. Sie könnten sie jedoch an einer beliebigen Stelle der Liste platzieren. Benutzerdefinierte Druckoptionen müssen nicht zuletzt hinzugefügt werden.

Wenn der Benutzer die ausgewählte Option in Ihrer benutzerdefinierten Druckoption ändert, aktualisieren Sie das Druckvorschaubild. Rufen Sie die [**InvalidatePreview**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview)-Methode auf, um das Bild in der Druckvorschau-Benutzeroberfläche neu zu zeichnen, wie unten gezeigt.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Design guidelines for printing](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [//Build 2015 video: Developing apps that print in Windows 10](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP print sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)
