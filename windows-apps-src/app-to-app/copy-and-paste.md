---
description: In diesem Artikel wird erläutert, wie in Apps für die universelle Windows-Plattform (UWP) das Kopieren und Einfügen über die Zwischenablage unterstützt wird.
title: Kopieren und Einfügen
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ba498a9bc6541f45f01e74b1ea90f4f9a1bfde90
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "7701236"
---
# <a name="copy-and-paste"></a>Kopieren und Einfügen

In diesem Artikel wird erläutert, wie Kopieren und Einfügen mit der Zwischenablage in Apps der universellen Windows-Plattform unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen.

## <a name="check-for-built-in-clipboard-support"></a>Überprüfen der integrierten Unterstützung für die Zwischenablage

In vielen Fällen müssen Sie keinen Code für die Unterstützung von Zwischenablageaktionen schreiben. Viele der Standard-XAML-Steuerelemente, die Sie beim Erstellen Ihrer Apps verwenden können, unterstützen bereits Zwischenablageaktionen. 

## <a name="get-set-up"></a>Vorbereiten

Schließen Sie zunächst den [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer)-Namespace in Ihre App ein. Fügen Sie dann eine Instanz des [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekts hinzu. Dieses Objekt enthält sowohl die vom Benutzer kopierten Daten als auch alle Eigenschaften (z.B. eine Beschreibung), die Sie einfügen möchten.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Kopieren und Ausschneiden

Kopieren und Ausschneiden (auch als *Verschieben* bezeichnet) funktionieren nahezu identisch. Wählen Sie mit der [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation)-Eigenschaft den gewünschten Vorgang aus.

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
## <a name="drag-and-drop"></a>Drag & Drop

Anschließend können Sie die vom Benutzer ausgewählten Daten in das [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt einfügen. Wenn die Daten von der **DataPackage**-Klasse unterstützt werden, können Sie die entsprechenden Methoden aus dem **DataPackage**-Objekt verwenden. Gehen Sie zum Hinzufügen von Text wie folgt vor:

```cs
dataPackage.SetText("Hello World!");
```

Im letzten Schritt wird das [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt durch Aufrufen der statischen [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage))-Methode zur Zwischenablage hinzugefügt.

```cs
Clipboard.SetContent(dataPackage);
```
## <a name="paste"></a>Einfügen

Rufen Sie zum Abrufen des Inhalts der Zwischenablage die statische [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent)-Methode auf. Die Methode gibt eine [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) mit dem Inhalt zurück. Dieses Objekt ist mit dem [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt nahezu identisch, der Inhalt ist allerdings schreibgeschützt. Anhand dieses Objekts können Sie dann entweder mit der [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats)-Methode oder mit der [**Contains**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.Contains(System.String))-Methode ermitteln, welche Formate verfügbar sind. Rufen Sie anschließend die entsprechende [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView)-Methode auf, um die Daten zu erhalten.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>Nachverfolgen von Änderungen an der Zwischenablage

Zusätzlich zu den Befehlen zum Kopieren und Einfügen empfiehlt es sich unter Umständen, auch Änderungen an der Zwischenablage nachzuverfolgen. Behandeln Sie dafür das [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged)-Ereignis der Zwischenablage.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>Siehe auch

* [App-zu-App-Kommunikation](index.md)
* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [Contains](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)

