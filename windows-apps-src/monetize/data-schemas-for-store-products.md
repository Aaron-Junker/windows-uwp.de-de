---
description: Beschreibt das erweiterte JSON-Datenschema für Store-Produkte im Windows. Services. Store-Namespace.
title: Datenschemas für Store-Produkte
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, extendedjsondata, Store-Produkte, Schema
ms.localizationpriority: medium
ms.openlocfilehash: e09e02d12afc436f5d22d11fad85fb0bad3507f6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363183"
---
# <a name="data-schemas-for-store-products"></a>Datenschemas für Store-Produkte

Wenn Sie ein Produkt, z. b. eine APP oder ein Add-on, an den Store übermitteln, verwaltet der Store einen umfassenden Satz von Daten für das Produkt und seine Lizenzen. Im Code Ihrer APP können Sie Programm gesteuert auf einige dieser Daten zugreifen, indem Sie die Eigenschaften im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwenden. Beispielsweise können Sie die Beschreibung und den Preis der aktuellen APP oder ein Add-on für die aktuelle App mithilfe der Eigenschaften [storeproduct. Description](/uwp/api/windows.services.store.storeproduct.Description) und [storeproduct. Price](/uwp/api/windows.services.store.storeproduct.Price) abrufen.

Ein Großteil der Daten für Produkte im Speicher wird jedoch nicht durch vordefinierte Eigenschaften im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verfügbar gemacht. Für den Zugriff auf die vollständigen Daten für ein Produkt in Ihrem Code können Sie stattdessen die folgenden allgemeinen Eigenschaften verwenden:

* [Storeproduct. extendedjsondata](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [Storesku. extendedjsondata](/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [Storeavailability. extendedjsondata](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [Storecollectiondata. extendedjsondata](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [Storeapplicense. extendedjsondata](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [Storelicense. extendedjsondata](/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [Storepurchaseproperties. extendedjsondata](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Diese Eigenschaften geben die gesamten Daten für das entsprechende Objekt als JSON-formatierte Zeichenfolge zurück. Dieser Artikel enthält das komplette Schema für die Daten, die von diesen Eigenschaften zurückgegeben werden.

> [!NOTE]
> Produkte im Geschäft sind in einer Hierarchie von [Produkt](/uwp/api/windows.services.store.storeproduct)-, [SKU](/uwp/api/windows.services.store.storesku)-und [Verfügbarkeits](/uwp/api/windows.services.store.storeavailability) Objekten organisiert. Weitere Informationen finden Sie unter [Produkte, SKUs und Verfügbarkeit](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Schema für storeproduct, storesku, storeavailability und storecollectiondata

Im folgenden Schema wird die JSON-formatierte Zeichenfolge beschrieben, die von [storeproduct. extendedjsondata](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)zurückgegeben wurde. Die Eigenschaften [storesku. extendedjsondata](/uwp/api/windows.services.store.storesku.ExtendedJsonData), [storeavailability. extendedjsondata](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)und [storecollectiondata. extendedjsondata](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) geben nur die Teile des Schemas zurück, die jeweils unter den `DisplaySkuAvailabilities\Sku` Pfaden, und definiert sind `DisplaySkuAvailabilities\Availabilities` `DisplaySkuAvailabilities\Sku\CollectionData` .

Ein Beispiel für eine JSON-formatierte Zeichenfolge, die von [storeproduct. extendedjsondata](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)zurückgegeben wurde, finden Sie in [diesem Abschnitt](#product-example).

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json" range="1-729":::

<span id="product-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge, die von der [storeproduct. extendedjsondata](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) -Eigenschaft für die-APP zurückgegeben wird.

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json" range="1-268":::

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schema für storeapplicense und storelicense

Im folgenden Schema wird die JSON-formatierte Zeichenfolge beschrieben, die von [storeapplicense. extendedjsondata](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)zurückgegeben wurde. Die [storelicense. extendedjsondata](/uwp/api/windows.services.store.storelicense.ExtendedJsonData) -Eigenschaft gibt nur die Teile des Schemas zurück, die unter dem Pfad definiert sind `productAddOns` .

Ein Beispiel für eine JSON-formatierte Zeichenfolge, die von [storeapplicense. extendedjsondata](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)zurückgegeben wurde, finden Sie in [diesem Abschnitt](#license-example).

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json" range="1-80":::

<span id="license-example" />

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine JSON-formatierte Zeichenfolge, die von der [storeapplicense. extendedjsondata](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) -Eigenschaft für die-APP zurückgegeben wird.

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json" range="1-28":::

## <a name="schema-for-storepurchaseproperties"></a>Schema für storepurchaseproperties

Im folgenden Schema wird die JSON-formatierte Zeichenfolge beschrieben, die von [storepurchaseproperties. extendedjsondata](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)zurückgegeben wurde.

:::code language="json" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json" range="1-12":::

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
