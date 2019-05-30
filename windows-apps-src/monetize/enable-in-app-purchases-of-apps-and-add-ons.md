---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um eine App oder ein Add-on zu erwerben.
title: Aktivieren von In-App-Käufen von Apps und Add-Ons
keywords: windows 10, uwp, add-ons, in-app-käufe, IAPs, Windows.Services.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0825732698da415f976ce1429aaa5af41b681bff
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372571"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Aktivieren von In-App-Käufen von Apps und Add-Ons

In diesem Artikel wird veranschaulicht, wie Sie Mitglieder im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace verwenden, um für den Benutzer den Kauf der aktuellen App oder eines ihrer Add-Ons anzufordern. Wenn der Benutzer beispielsweise aktuell über eine Testversion der App verfügt, können Sie diesen Vorgang verwenden, um für den Benutzer eine Volllizenz zu erwerben. Alternativ können Sie diesen Prozess auch verwenden, um für den Benutzer ein Add-On wie z. B. ein neues Gamelevel zu erwerben.

Um den Kauf einer App oder eines Add-Ons anzufordern, bietet der [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace verschiedene Methoden:
* Wenn Sie die [Store-ID](in-app-purchases-and-trials.md#store_ids) der App bzw. des Add-Ons kennen, verwenden Sie die [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync)-Methode der [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse.
* Wenn Sie bereits über ein [**StoreProduct**](in-app-purchases-and-trials.md#products-skus)-, **StoreSku**- oder **StoreAvailability**-Objekt verfügen, das die App bzw. das Add-On darstellt, können Sie die **RequestPurchaseAsync**-Methoden dieser Objekte verwenden. Beispiele für die verschiedenen Methoden zum Abrufen einer **StoreProduct** in Ihrem Code finden Sie unter [Abrufen von Produktinformationen für Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md).

Jede Methode zeigt dem Benutzer eine Standardbenutzeroberfläche für den Einkauf an und führt den Vorgang nach Abschluss der Transaktion asynchron aus. Die Methode gibt ein Objekt zurück, das angibt, ob die Transaktion erfolgreich war.

> [!NOTE]
> Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Vorraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher, geeignet ist.
* Sie haben [erstellt eine app-Einsendung](https://docs.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center und diese app wird in den Store veröffentlicht. Optional können Sie die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar ist. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Wenn Sie in-app-Käufe für ein Add-on für die app aktivieren möchten, müssen Sie auch [erstellen Sie das Add-on im Partner Center](../publish/add-on-submissions.md).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), die einen [ProgressRing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktopanwendung verfügen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwendet, müssen Sie möglicherweise zusätzlichen, in diesem Beispiel nicht aufgeführten Code hinzufügen, um das [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Codebeispiel

In diesem Beispiel wird die Verwendung der [RequestPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestpurchaseasync)-Methode der [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse veranschaulicht, um eine App oder ein Add-On mit bekannter [Store-ID](in-app-purchases-and-trials.md#store-ids) zu erwerben. Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Video

Sehen Sie sich das folgende Video mit einer Übersicht über In-App-Käufe in Ihrer App an.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen für apps und -Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren Sie nutzbar Add-on-Käufe](enable-consumable-add-on-purchases.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
