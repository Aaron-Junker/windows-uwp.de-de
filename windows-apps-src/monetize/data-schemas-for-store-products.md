---
author: Xansky
description: Beschreibt das erweiterte JSON-Datenschema für Store-Produkte im Windows.Services.Store-Namespace.
title: Datenschemata für Store-Produkte
ms.author: mhopkins
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, ExtendedJsonData, Store-Produkte, Schema
ms.localizationpriority: medium
ms.openlocfilehash: 980fde1a222b5fb7ba2d4524469a9b6673cbabd3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "5971476"
---
# <a name="data-schemas-for-store-products"></a>Datenschemata für Store-Produkte

Wenn Sie ein Produkt wie z.B. eine App oder ein Add-On im Store einreichen, verwaltet der Informationsspeicher einen umfassenden Satz von Daten für das Produkt und dessen Lizenzen. In Ihrem App-Code können Sie programmgesteuerten Zugriff auf einige dieser Daten mithilfe der Eigenschaften im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace nutzen. Zum Beispiel können Sie die Beschreibung und den Preis für die aktuelle App oder ein Add-On für die aktuelle App mithilfe der [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) und [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price) Eigenschaften abrufen.

Viele Daten für Produkte im Store sind jedoch ist nicht über vordefinierte Eigenschaften im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace verfügbar. Um auf die vollständigen Daten für ein Produkt in Ihrem Code zuzugreifen, können Sie die folgenden allgemeinen Eigenschaften verwenden:

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

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) zurückgegeben wird. Die [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData), und [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) Eigenschaften geben nur die Teile des Schemas zurück, die unter den Pfaden ```DisplaySkuAvailabilities\Sku```, ```DisplaySkuAvailabilities\Availabilities``` und ```DisplaySkuAvailabilities\Sku\CollectionData``` definiert sind.

Ein Beispiel für eine JSON-formatierte Zeichenfolge von [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) finden Sie [in diesem Abschnitt](#product-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge von der [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) Eigenschaft für die App.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schema für StoreAppLicense und StoreLicense

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) zurückgegeben wird. Die [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) Eigenschaft gibt nur die Teile des Schemas zurück, die unter dem ```productAddOns``` Pfad definiert sind.

Ein Beispiel für eine JSON-formatierte Zeichenfolge von [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) finden Sie [in diesem Abschnitt](#license-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge von der [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) Eigenschaft für die App.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Schema für StorePurchaseProperties

Das folgende Schema beschreibt die JSON-formatierte Zeichenfolge die von [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData) zurückgegeben wird.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Käufen konsumierbarer Add-Ons](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion Ihrer App](implement-a-trial-version-of-your-app.md)
