---
description: Beschreibt das erweiterte JSON-Datenschema für Store-Produkte im Windows.Services.Store-Namespace.
title: Datenschemata für Store-Produkte
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, ExtendedJsonData, Store-Produkte, Schema
ms.localizationpriority: medium
ms.openlocfilehash: 77f63ce409a576b3c873d95df0d2e8d0f0933808
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372536"
---
# <a name="data-schemas-for-store-products"></a>Datenschemata für Store-Produkte

Wenn Sie ein Produkt wie z. B. eine App oder ein Add-On im Store einreichen, verwaltet der Informationsspeicher einen umfassenden Satz von Daten für das Produkt und dessen Lizenzen. In Ihrem App-Code können Sie programmgesteuerten Zugriff auf einige dieser Daten mithilfe der Eigenschaften im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) Namespace nutzen. Zum Beispiel können Sie die Beschreibung und den Preis für die aktuelle App oder ein Add-On für die aktuelle App mithilfe der [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) und [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price) Eigenschaften abrufen.

Viele Daten für Produkte im Store sind jedoch ist nicht über vordefinierte Eigenschaften im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) Namespace verfügbar. Um auf die vollständigen Daten für ein Produkt in Ihrem Code zuzugreifen, können Sie die folgenden allgemeinen Eigenschaften verwenden:

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Diese Eigenschaften geben die vollständigen Daten für das entsprechende Objekt als JSON-formatierte Zeichenfolge zurück. Dieser Artikel enthält das vollständige Schema für die Daten von diesen Eigenschaften zurückgegeben werden.

> [!NOTE]
> Produkte im Store sind in einer Hierarchie von [Produkt](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [SKU](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) und [Verfügbarkeit](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability) Objekten organisiert. Weitere Informationen finden Sie unter [Produkte, SKUs und Verfügbarkeiten](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Schema für StoreProduct, StoreSku, StoreAvailability und StoreCollectionData

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) zurückgegeben wird. Die [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData), und [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) Eigenschaften geben nur die Teile des Schemas zurück, die unter den Pfaden `DisplaySkuAvailabilities\Sku`, `DisplaySkuAvailabilities\Availabilities` und `DisplaySkuAvailabilities\Sku\CollectionData` definiert sind.

Ein Beispiel für eine JSON-formatierte Zeichenfolge von [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) finden Sie [in diesem Abschnitt](#product-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge von der [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) Eigenschaft für die App.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schema für StoreAppLicense und StoreLicense

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) zurückgegeben wird. Die [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) Eigenschaft gibt nur die Teile des Schemas zurück, die unter dem `productAddOns` Pfad definiert sind.

Ein Beispiel für eine JSON-formatierte Zeichenfolge von [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) finden Sie [in diesem Abschnitt](#license-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge von der [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) Eigenschaft für die App.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Schema für StorePurchaseProperties

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData) zurückgegeben wird.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen für apps und -Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von in-app-Käufe von apps und -Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Aktivieren Sie nutzbar Add-on-Käufe](enable-consumable-add-on-purchases.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
