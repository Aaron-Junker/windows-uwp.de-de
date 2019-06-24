---
description: In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird.
title: Freigeben von Daten
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 08dbe9ed7aaa732172d488712aa47d6d3631508a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317704"
---
# <a name="share-data"></a>Freigeben von Daten


In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern.

## <a name="set-up-an-event-handler"></a>Einrichten eines Ereignishandlers

Fügen Sie einen [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)-Ereignishandler hinzu, der aufgerufen werden soll, wenn der Benutzer das Teilen-Feature verwendet. Dies kann durch Tippen auf ein Steuerelement in Ihrer App (etwa eine Schaltfläche oder ein App-Leistenbefehl) oder automatisch in einem bestimmten Szenario geschehen (etwa wenn der Benutzer einen Level mit einer hohen Punktzahl abschließt).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

Wenn ein [**DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)-Ereignis eintritt, empfängt Ihre App ein [**DataRequest**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest)-Objekt. Dieses enthält ein [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), mit dem Sie den Inhalt bereitstellen können, den der Benutzer freigeben möchte. Sie müssen einen Titel und die freizugebenden Daten angeben. Eine Beschreibung ist optional, wird aber empfohlen.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>Wählen von Daten

Sie können verschiedene Arten von Daten freigeben, einschließlich:

-   Nur-Text
-   URIs (Uniform Resource Identifiers)
-   HTML
-   Formatierter Text
-   Bitmaps
-   Dateien
-   Vom Entwickler definierte Daten

Das [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt kann mehrere dieser Formate in beliebiger Kombination enthalten. Im folgenden Beispiel wird das Freigeben von Text veranschaulicht.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>Festlegen von Eigenschaften

Beim Verpacken von Daten für die Freigabe können Sie eine Vielzahl von Eigenschaften angeben, die weitere Informationen zum freigegebenen Inhalt enthalten. Mit diesen Eigenschaften kann die Benutzererfahrung für Ziel-Apps verbessert werden. Die Angabe einer Beschreibung kann beispielsweise nützlich sein, wenn der Benutzer Inhalte für mehrere Apps freigibt. Eine Miniaturansicht beim Freigeben eines Bilds oder eines Links für eine Webseite stellt eine visuelle Referenz für den Benutzer dar. Weitere Informationen finden Sie unter [**DataPackagePropertySet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Alle Eigenschaften mit Ausnahme des Titels sind optional. Die title-Eigenschaft ist erforderlich und muss festgelegt werden.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>Starten der Benutzeroberfläche für das Freigeben

Eine Benutzeroberfläche für das Freigeben wird vom System bereitgestellt. Zum Starten rufen Sie die [**ShowShareUI**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)-Methode auf.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>Behandeln von Fehlern

In den meisten Fällen ist das Freigeben von Inhalten ein einfacher Prozess. Es besteht jedoch immer die Möglichkeit, dass unerwartet ein Problem auftritt. So kann in der App etwa vorgesehen sein, dass der Benutzer etwas zum Teilen auswählt, der Benutzer hat aber nichts ausgewählt. Um diese Situationen zu behandeln, verwenden Sie die [**FailWithDisplayText**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_)-Methode, die dem Benutzer eine Meldung anzeigt, wenn ein Fehler auftritt.

## <a name="delay-share-with-delegates"></a>Verzögern der Freigabe mit Delegaten

Manchmal ist es nicht sinnvoll, die Daten, die der Benutzer freigeben möchte, direkt vorzubereiten. Wenn Ihre App zum Beispiel das Senden von großen Bilddateien in verschiedenen Formaten unterstützt, ist es ineffizient, alle diese Bilder zu erstellen, bevor der Benutzer seine Auswahl trifft.

Zum Lösen dieses Problems kann ein [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) einen Delegaten enthalten. Dabei handelt es sich um eine Funktion, die aufgerufen wird, wenn die empfangende App Daten anfordert. Es wird empfohlen, einen Delegaten immer dann zu verwenden, wenn die von einem Benutzer freigegebenen Daten ressourcenintensiv sind.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>Siehe auch 

* [App-zu-App-Kommunikation](index.md)
* [Empfangen von Daten](receive-data.md)
* [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [DataRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 

