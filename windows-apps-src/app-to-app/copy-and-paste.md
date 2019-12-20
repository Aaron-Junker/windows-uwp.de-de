---
description: In diesem Artikel wird erläutert, wie in Apps für die universelle Windows-Plattform (UWP) das Kopieren und Einfügen über die Zwischenablage unterstützt wird.
title: Kopieren und Einfügen
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dfa5c15c2bd4d82588e0b197dc265c4b529e64c9
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302684"
---
# <a name="copy-and-paste"></a>Kopieren und Einfügen

In diesem Artikel wird erläutert, wie in Apps für die universelle Windows-Plattform (UWP) das Kopieren und Einfügen über die Zwischenablage unterstützt wird. Kopieren und Einfügen ist die klassische Methode zum Austausch von Daten zwischen Apps oder in einer App, und nahezu jede App kann Zwischenablageaktionen bis zu einem gewissen Grad unterstützen. Umfassende Codebeispiele, in denen verschiedene Kopier-und Einfügen-Szenarien veranschaulicht werden, finden Sie im Beispiel für die [Zwischenablage](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard).

## <a name="check-for-built-in-clipboard-support"></a>Überprüfen der integrierten Unterstützung für die Zwischenablage

In vielen Fällen müssen Sie keinen Code für die Unterstützung von Zwischenablageaktionen schreiben. Viele der Standard-XAML-Steuerelemente, die Sie beim Erstellen Ihrer Apps verwenden können, unterstützen bereits Zwischenablageaktionen. 

## <a name="get-set-up"></a>Vorbereitung

Schließen Sie zunächst den [**Windows.ApplicationModel.DataTransfer**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer)-Namespace in Ihre App ein. Fügen Sie dann eine Instanz des [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekts hinzu. Dieses Objekt enthält sowohl die vom Benutzer kopierten Daten als auch alle Eigenschaften (z. B. eine Beschreibung), die Sie einfügen möchten.

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Kopieren und Ausschneiden

Kopieren und Ausschneiden (auch als *Verschieben* bezeichnet) funktionieren nahezu identisch. Wählen Sie mit der [**RequestedOperation**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation)-Eigenschaft den gewünschten Vorgang aus.

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>Den kopierten Inhalt festlegen

Anschließend können Sie die vom Benutzer ausgewählten Daten in das [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt einfügen. Wenn diese Daten von der **DataPackage** -Klasse unterstützt werden, können Sie eine der entsprechenden [Methoden](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods) des **DataPackage** -Objekts verwenden. Im folgenden wird erläutert, wie Sie Text mithilfe der [**SetText**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) -Methode hinzufügen:

```cs
dataPackage.SetText("Hello World!");
```

Im letzten Schritt wird das [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt durch Aufrufen der statischen [**SetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)-Methode zur Zwischenablage hinzugefügt.

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>Einfügen

Rufen Sie zum Abrufen des Inhalts der Zwischenablage die statische [**GetContent**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)-Methode auf. Die Methode gibt eine [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) mit dem Inhalt zurück. Dieses Objekt ist mit dem [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt nahezu identisch, der Inhalt ist allerdings schreibgeschützt. Anhand dieses Objekts können Sie dann entweder mit der [**AvailableFormats**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)-Methode oder mit der [**Contains**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)-Methode ermitteln, welche Formate verfügbar sind. Rufen Sie anschließend die entsprechende [**DataPackageView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView)-Methode auf, um die Daten zu erhalten.

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

Zusätzlich zu den Befehlen zum Kopieren und Einfügen empfiehlt es sich unter Umständen, auch Änderungen an der Zwischenablage nachzuverfolgen. Behandeln Sie dafür das [**ContentChanged**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)-Ereignis der Zwischenablage.

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

## <a name="see-also"></a>Weitere Informationen:

* [Beispiel für Zwischenablage](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [App-zu-App-Kommunikation](index.md)
* [DataTransfer](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [Datapackageview](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [Datapackagepropertyset]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [Datarequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [Failwithdisplaytext](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [Showshareui](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [Controlslist](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)
* [SetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [Availableformats](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Enthält](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [Contentchanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)

