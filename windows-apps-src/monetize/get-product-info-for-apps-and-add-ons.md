---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um Store-bezogene Produktinformationen für die aktuelle App oder eines ihrer Add-Ons abzurufen.
title: Abrufen von Produktinformationen zu Apps und Add-Ons
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP, in-App-Käufe, IAPS, Add-ons, Windows. Services. Store
ms.localizationpriority: medium
ms.openlocfilehash: a46d9b0049e0dc9456a36c726a611cbaf00e9616
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164604"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Abrufen von Produktinformationen zu Apps und Add-Ons

In diesem Artikel wird veranschaulicht, wie die Methoden der [storecontext](/uwp/api/windows.services.store.storecontext) -Klasse im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwendet werden, um auf speicherbezogene Informationen für die aktuelle APP oder eines ihrer Add-ons zuzugreifen.

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine universelle Windows-Plattform-app (UWP), die auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version.
* Sie haben [eine APP-Übermittlung](../publish/app-submissions.md) im Partner Center erstellt, und diese APP wird im Store veröffentlicht. Optional können Sie die APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist, während Sie Sie testen. Weitere Informationen finden Sie in unserer [Test Anleitung](in-app-purchases-and-trials.md#testing).
* Wenn Sie Produktinformationen für ein Add-on für die APP erhalten möchten, müssen Sie [das Add-on auch im Partner Center erstellen](../publish/add-on-submissions.md).

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](/uwp/api/windows.ui.xaml.controls.page), die einen [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) mit dem Namen ```workingProgressRing``` und einen [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktop Anwendung verfügen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)verwendet, müssen Sie möglicherweise zusätzlichen Code hinzufügen, der in diesen Beispielen nicht angezeigt wird, um das [storecontext](/uwp/api/windows.services.store.storecontext) -Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Abrufen von Informationen für die aktuelle App

Verwenden Sie zum Abrufen von Store-Produktinformationen zur aktuellen App die [GetStoreProductForCurrentAppAsync](/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync)-Methode. Dies ist eine asynchrone Methode, die ein [StoreProduct](/uwp/api/windows.services.store.storeproduct)-Objekt zurückgibt, das Sie verwenden können, um Informationen wie den Preis abzurufen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Erhalten Sie Informationen zu Add-ons mit bekannten Store-IDs, die der aktuellen App zugeordnet sind.

Verwenden Sie die [getstoreproductionasync](/uwp/api/windows.services.store.storecontext.getstoreproductsasync) -Methode, um Speicherprodukt Informationen für Add-ons abzurufen, die der aktuellen App zugeordnet sind und für die Sie bereits die [Store-IDs](in-app-purchases-and-trials.md#store_ids)kennen. Dabei handelt es sich um eine asynchrone Methode, die eine Auflistung von [storeproduct](/uwp/api/windows.services.store.storeproduct) -Objekten zurückgibt, die die einzelnen Add-ons darstellen. Zusätzlich zu den Store-IDs müssen Sie eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen der Add-Ons identifizieren. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind)-Eigenschaft.

> [!NOTE]
> Die **getstoreproductionasync** -Methode gibt Produktinformationen für die angegebenen Add-ons zurück, die der APP zugeordnet sind, unabhängig davon, ob die Add-ons zurzeit für den Kauf verfügbar sind. Zum Abrufen von Informationen für alle Add-ons für die aktuelle APP, die derzeit erworben werden können, verwenden Sie stattdessen die **getassociatedstoreproduczasync** -Methode, wie im [folgenden Abschnitt](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app) beschrieben.

In diesem Beispiel werden Informationen für permanente Add-ons mit den angegebenen Speicher-IDs abgerufen, die der aktuellen App zugeordnet sind.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Hier erhalten Sie Informationen zu Add-ons, die über die aktuelle App erworben werden können.

Verwenden Sie die [getassociatedstoreproductionasync](/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync) -Methode, um Informationen zum Speicherprodukt für die Add-ons zu erhalten, die zurzeit für den Einkauf in der aktuellen app verfügbar sind. Dabei handelt es sich um eine asynchrone Methode, die eine Auflistung von [storeproduct](/uwp/api/windows.services.store.storeproduct) -Objekten zurückgibt, die die einzelnen verfügbaren Add-ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-Ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind)-Eigenschaft.

> [!NOTE]
> Wenn die APP über viele Add-ons verfügt, die für den Kauf verfügbar sind, können Sie alternativ die [getassociatedstoreproductwithpagingasync](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) -Methode verwenden, um das Add-on-Ergebnis mithilfe von Paging zurückzugeben.

Im folgenden Beispiel werden Informationen für alle permanenten Add-ons, von Filialen verwaltete, nutzbare Add-ons und von Entwicklern verwaltete, nutzbare Add-ons abgerufen, die über die aktuelle App erworben werden können.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Hier erhalten Sie Informationen zu Add-ons für die aktuelle APP, die der Benutzer gekauft hat.

Verwenden Sie die [getusercollectionasync](/uwp/api/windows.services.store.storecontext.getusercollectionasync) -Methode, um Informationen zum Speicherprodukt für Add-ons, die der aktuelle Benutzer gekauft hat, zu erhalten. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](/uwp/api/windows.services.store.storeproduct)-Objekten zurück gibt, die alle Add-Ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-Ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind)-Eigenschaft.

> [!NOTE]
> Wenn die APP über viele Add-ons verfügt, können Sie alternativ die [getusercollectionwithpagingasync](/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) -Methode verwenden, um das Add-on-Ergebnis mithilfe von Paging zurückzugeben.

Im folgenden Beispiel werden Informationen für permanente Add-ons mit den angegebenen [Speicher-IDs](in-app-purchases-and-trials.md#store_ids)abgerufen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Zugehörige Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)