---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: In diesem Artikel werden das Lesen und Schreiben von Eigenschaften von Bildmetadaten und das Hinzufügen von Geomarkierungen zu Dateien mithilfe der GeotagHelper-Hilfsklasse erläutert.
title: Bildmetadaten
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ca2a5abe5c0a7f60246322dd81fad9af8f0def77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157494"
---
# <a name="image-metadata"></a>Bildmetadaten



In diesem Artikel werden das Lesen und Schreiben von Eigenschaften von Bildmetadaten und das Hinzufügen von Geomarkierungen zu Dateien mithilfe der [**GeotagHelper**](/uwp/api/Windows.Storage.FileProperties.GeotagHelper)-Hilfsklasse erläutert.

## <a name="image-properties"></a>Image-Eigenschaften

Die [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties)-Eigenschaft gibt ein [**StorageItemContentProperties**](/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties)-Objekt zurück, das Zugriff auf inhaltsbezogene Informationen zur Datei bietet. Rufen Sie die bildspezifischen Eigenschaften durch Aufruf von [**GetImagePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync) ab. Das zurückgegebene [**ImageProperties**](/uwp/api/Windows.Storage.FileProperties.ImageProperties)-Objekt macht Member verfügbar, die Felder mit grundlegenden Bildmetadaten enthalten, z. B. den Titel des Bilds und das Aufnahmedatum.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Verwenden Sie für den Zugriff auf eine größere Menge von Dateimetadaten das Windows-Eigenschaftensystem, eine Reihe von Eigenschaften von Dateimetadaten, die mit einem eindeutigen Zeichenfolgenbezeichner abgerufen werden können. Erstellen Sie eine Liste mit Zeichenfolgen, und fügen Sie den Bezeichner für jede Eigenschaft hinzu, die Sie abrufen möchten. Die [**ImageProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync)-Methode verwendet diese Liste mit Zeichenfolgen und gibt ein Wörterbuch von Schlüssel-Wert-Paaren zurück, bei denen der Schlüssel der Eigenschaftsbezeichner und der Wert der Eigenschaftswert ist.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Eine umfassende Liste der Windows-Eigenschaften, einschließlich der Bezeichner und des Typs für jede Eigenschaft, finden Sie unter [Windows-Eigenschaften](/windows/desktop/properties/props).

-   Einige Eigenschaften werden nur für bestimmte Dateicontainer und Bildcodecs unterstützt. Eine Auflistung der Bild Metadaten, die für jeden Bildtyp unterstützt werden, finden Sie unter [Photo Metadata Policies](/windows/desktop/wic/photo-metadata-policies).

-   Da nicht unterstützte Eigenschaften beim Abrufen möglicherweise einen NULL-Wert zurückgeben, prüfen Sie immer auf NULL, bevor Sie einen zurückgegebenen Metadatenwert verwenden.

## <a name="geotag-helper"></a>GeotagHelper

„GeotagHelper“ ist eine Hilfsklasse, die das problemlose und direkte Markieren von Bildern mit geografischen Daten unter Verwendung der [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation)-APIs ermöglicht, ohne dass das Metadatenformat manuell analysiert oder konstruiert werden muss.

Wenn Sie bereits über ein [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)-Objekt verfügen, das die Position darstellt, die Sie im Bild markieren möchten (entweder aus einer früheren Verwendung der Geolocation-APIs oder aus einer anderen Quelle), können Sie die Geomarkierungsdaten festlegen, indem Sie [**GeotagHelper.SetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) aufrufen und eine [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) und einen **Geopoint** übergeben.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Um die Geomarkierungsdaten mithilfe der aktuellen Position des Geräts festzulegen, erstellen Sie ein neues [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Objekt, und rufen Sie [**GeotagHelper.SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) auf. Übergeben Sie dabei die **Geolocator**-Klasse und die zu markierende Datei.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Sie müssen die **location**-Gerätefunktion in das App-Manifest einschließen, um die [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync)-API verwenden zu können.

-   Sie müssen [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) vor [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) aufrufen, um sicherzustellen, dass der Benutzer Ihrer App die Berechtigung zur Verwendung seiner Position gewährt hat.

-   Weitere Informationen zu den Geolocation-APIs finden Sie unter [Karten und Position](../maps-and-location/index.md).

Um einen GeoPoint abzurufen, der die geomarkierte Position einer Bilddatei darstellt, rufen Sie [**GetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync) auf.

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Decodieren und Codieren von Bildmetadaten

Die fortschrittlichste Methode zum Arbeiten mit Bilddaten ist das Lesen und Schreiben der Eigenschaften auf Datenstromebene mithilfe von [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) oder [BitmapEncoder](bitmapencoder-options-reference.md). Für diese Vorgänge können Sie Windows-Eigenschaften verwenden, um die gelesenen oder geschriebenen Daten anzugeben. Sie können aber auch die von der Windows-Bilderstellungskomponente (WIC) bereitgestellte Metadatenabfragesprache verwenden, um den Pfad zu einer angeforderten Eigenschaft anzugeben.

Um Bildmetadaten mit diesem Verfahren lesen zu können, benötigen Sie einen [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), der mit dem Dateidatenstrom des Quellbilds erstellt wurde. Informationen zur Vorgehensweise finden Sie unter [Bildverarbeitung](imaging.md).

Nachdem Sie über den Decoder verfügen, erstellen Sie eine Liste mit Zeichenfolgen, und fügen Sie für jede abzurufende Metadateneigenschaft einen neuen Eintrag hinzu; verwenden Sie dabei entweder die ID-Zeichenfolge der Windows-Eigenschaft oder eine WIC-Metadatenabfrage. Rufen Sie die [**BitmapPropertiesView.GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync)-Methode für den [**BitmapProperties**](/uwp/api/Windows.Graphics.Imaging.BitmapProperties)-Member des Decoders auf, um die angegebenen Eigenschaften anzufordern. Die Eigenschaften werden in einem Wörterbuch mit Schlüssel-Wert-Paaren zurückgegeben, die den Eigenschaftsnamen oder den Pfad und den Eigenschaftswert enthalten.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Weitere Informationen zur WIC-Metadatenabfragesprache und den unterstützten Eigenschaften finden Sie unter [WIC Image Format Native Metadata Queries](/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   Viele Metadateneigenschaften werden nur von einer Teilmenge aller Bildtypen unterstützt. [              Bei Ausführung von **GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) tritt ein Fehler mit Fehlercode 0x88982F41 auf, wenn eine der angeforderten Eigenschaften von dem dem Decoder zugeordneten Bild nicht unterstützt wird, und mit Fehlercode 0x88982F81, wenn das Bild überhaupt keine Metadaten unterstützt. Die diesen Fehlercodes zugeordneten Konstanten lauten wincodec \_ Err \_ propertynotsupported und wincodec \_ Err \_ unsupportedoperation und sind in der Header Datei Winerror. h definiert.
-   Da ein Bild nicht unbedingt einen Wert für eine bestimmte Eigenschaft enthält, überprüfen Sie mithilfe von **IDictionary.ContainsKey**, ob eine Eigenschaft in den Ergebnissen vorhanden ist, bevor Sie versuchen, darauf zuzugreifen.

Um Bildmetadaten in den Datenstrom zu schreiben, muss der Bildausgabedatei ein **BitmapEncoder** zugeordnet sein.

Erstellen Sie ein [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet)-Objekt als Container für die festzulegenden Eigenschaftswerte. Erstellen Sie ein [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue)-Objekt für den Eigenschaftswert. Dieses Objekt verwendet **object** als Wert und Member der [**PropertyType**](/uwp/api/Windows.Foundation.PropertyType)-Enumeration, die den Typ des Werts definiert. Fügen Sie den **BitmapTypedValue** dem **BitmapPropertySet** hinzu, und rufen Sie dann [**BitmapProperties.SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) auf, damit der Encoder die Eigenschaften in den Datenstrom schreibt.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Ausführliche Informationen dazu, welche Eigenschaften für welche Bilddateitypen unterstützt werden, finden Sie unter [Windows-Eigenschaften](/windows/desktop/properties/props), [Richtlinien zu Fotometadaten](/windows/desktop/wic/photo-metadata-policies) und [WIC-Metadatenabfragen für systemeigene Bildformate](/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   [Für **SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) tritt ein Fehler mit dem Fehlercode 0x88982F41 auf, wenn das dem Encoder zugeordnete Bild eine der angeforderten Eigenschaften nicht unterstützt.

## <a name="related-topics"></a>Zugehörige Themen

* [Bildverarbeitung](imaging.md)
 

 