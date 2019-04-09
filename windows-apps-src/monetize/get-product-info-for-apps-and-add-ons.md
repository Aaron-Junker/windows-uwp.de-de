---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um Store-bezogene Produktinformationen für die aktuelle App oder eines ihrer Add-Ons abzurufen.
title: Abrufen von Produktinformationen zu Apps und Add-Ons
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP, In-App-Einkäufe, IAPs, Add-Ons, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 8ec9c64e3cffb55a674fcc19d3152de29ef6d988
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334888"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Abrufen von Produktinformationen zu Apps und Add-Ons

Dieser Artikel veranschaulicht die Verwendung von Methoden der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace, um Storeinformationen für die aktuelle App oder eine der Add-Ons abzurufen.

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Vorraussetzungen

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher, geeignet ist.
* Sie haben [erstellt eine app-Einsendung](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center und diese app wird in den Store veröffentlicht. Optional können Sie die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar ist. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Wenn Sie die Produktinformationen für ein Add-on für die app abrufen möchten, müssen Sie auch [erstellen Sie das Add-on im Partner Center](../publish/add-on-submissions.md).

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktopanwendung verfügen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwendet, müssen Sie möglicherweise zusätzlichen, in diesen Beispielen nicht aufgeführten Code hinzufügen, um das [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Abrufen von Informationen für die aktuelle App

Verwenden Sie zum Abrufen von Store-Produktinformationen zur aktuellen App die [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync)-Methode. Dies ist eine asynchrone Methode, die ein [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekt zurückgibt, das Sie verwenden können, um Informationen wie den Preis abzurufen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Abrufen von Informationen für Add-Ons mit bekannten Store-IDs, die der aktuellen App zugeordnet sind

Verwenden Sie zum Abrufen von Store-Produktinformationen für Add-Ons, die der aktuellen App zugeordnet sind und für die Sie bereits die [Store-IDs](in-app-purchases-and-trials.md#store_ids) kennen, die [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurück gibt, die alle Add-Ons darstellen. Zusätzlich zu den Store-IDs müssen Sie eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen der Add-Ons identifizieren. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind)-Eigenschaft.

> [!NOTE]
> Die **GetStoreProductsAsync**-Methode gibt Produktinformationen für die angegebenen Add-Ons zurück, die in der App zugeordnet sind, unabhängig davon, ob die Add-Ons derzeit zum Erwerb erhältlich sind. Verwenden Sie zum Abrufen von Informationen für alle Add-Ons für die aktuelle App, die derzeit erworben werden können, stattdessen die **GetAssociatedStoreProductsAsync**-Methode, die im [folgenden Abschnitt](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app) beschrieben ist.

Im diesem Beispiel werden Informationen für dauerhafte Add-Ons mit den angegebenen Store-IDs abgerufen, die der aktuellen App zugeordnet sind.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Abrufen von Informationen für Add-Ons, die für die aktuelle App zum Erwerb verfügbar sind

Verwenden Sie zum Abrufen von Store-Produktinformationen für die Add-Ons, die für die aktuelle App derzeit zum Erwerb verfügbar sind, die [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurückgibt, die alle verfügbaren Add-Ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-Ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind)-Eigenschaft.

> [!NOTE]
> Wenn die App über viele Add-Ons verfügt, die zum Erwerb verfügbar sind, können Sie alternativ die Methode [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) verwenden, um für die Rückgabe der Add-On-Ergebnisse Paging zu verwenden.

Im folgenden Beispiel werden Informationen für alle dauerhaften Add-Ons, vom Store verwalteten Endverbraucher-Add-Ons und von Entwicklern verwalteten Endverbraucher-Add-Ons von der aktuellen App abgerufen, die zum Erwerb verfügbar sind.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Abrufen von Informationen zu Add-Ons für die aktuelle App, die der Nutzer erworben hat.

Verwenden Sie zum Abrufen von Store-Produktinformationen für Add-Ons, die der aktuelle Nutzer erworben hat, die [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die eine Sammlung von [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)-Objekten zurück gibt, die alle Add-Ons darstellen. Sie müssen eine Liste mit Zeichenfolgen an diese Methode übergeben, welche die Typen von Add-Ons identifizieren, die Sie abrufen möchten. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie in der [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx)-Eigenschaft.

> [!NOTE]
> Wenn die App über viele Add-Ons verfügt, können Sie alternativ die Methode [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) verwenden, um für die Rückgabe der Add-On-Ergebnisse Paging zu verwenden.

Im folgenden Beispiel werden Informationen für dauerhafte Add-Ons mit den angegebenen [Store-IDs](in-app-purchases-and-trials.md#store_ids) abgerufen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Lizenzinformationen für apps und -Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von in-app-Käufe von apps und -Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Aktivieren Sie nutzbar Add-on-Käufe](enable-consumable-add-on-purchases.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
