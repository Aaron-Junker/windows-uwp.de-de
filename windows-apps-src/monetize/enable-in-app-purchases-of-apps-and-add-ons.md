---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um eine App oder ein Add-on zu erwerben.
title: Aktivieren von In-App-Käufen von Apps und Add-Ons
keywords: Windows 10, UWP, Add-ons, in-App-Käufe, IAPS, Windows. Services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 36140b18c2358bf0b7ed497138ca7f9059900736
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171564"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Aktivieren von In-App-Käufen von Apps und Add-Ons

In diesem Artikel wird veranschaulicht, wie Member im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwendet werden, um den Kauf der aktuellen APP oder eines der Add-ons für den Benutzer anzufordern. Wenn der Benutzer beispielsweise aktuell über eine Testversion der App verfügt, können Sie diesen Vorgang verwenden, um für den Benutzer eine Volllizenz zu erwerben. Alternativ können Sie diesen Prozess auch verwenden, um für den Benutzer ein Add-On wie z. B. ein neues Gamelevel zu erwerben.

Der [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace stellt mehrere verschiedene Methoden bereit, um den Kauf einer APP oder eines Add-ins anzufordern.
* Wenn Sie die [Store-ID](in-app-purchases-and-trials.md#store_ids) der App bzw. des Add-Ons kennen, verwenden Sie die [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync)-Methode der [StoreContext](/uwp/api/windows.services.store.storecontext)-Klasse.
* Wenn Sie bereits über ein [ **storeproduct**-, **storesku**-oder **storeavailability** -Objekt](in-app-purchases-and-trials.md#products-skus) verfügen, das die APP oder das Add-on darstellt, können Sie die **requestpurchaseasync** -Methoden dieser Objekte verwenden. Beispiele für verschiedene Möglichkeiten zum Abrufen eines **storeproduct** in Ihren Code finden Sie unter Abrufen von [Produktinformationen für apps und Add-ons](get-product-info-for-apps-and-add-ons.md).

Jede Methode zeigt dem Benutzer eine Standardbenutzeroberfläche für den Einkauf an und führt den Vorgang nach Abschluss der Transaktion asynchron aus. Die Methode gibt ein Objekt zurück, das angibt, ob die Transaktion erfolgreich war.

> [!NOTE]
> Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine universelle Windows-Plattform-app (UWP), die auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version.
* Sie haben [eine APP-Übermittlung](../publish/app-submissions.md) im Partner Center erstellt, und diese APP wird im Store veröffentlicht. Optional können Sie die APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist, während Sie Sie testen. Weitere Informationen finden Sie in unserer [Test Anleitung](in-app-purchases-and-trials.md#testing).
* Wenn Sie in-App-Käufe für ein Add-on für die APP aktivieren möchten, müssen Sie [das Add-on auch im Partner Center erstellen](../publish/add-on-submissions.md).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](/uwp/api/windows.ui.xaml.controls.page), die einen [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) mit dem Namen ```workingProgressRing``` und einen [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktop Anwendung verfügen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)verwendet, müssen Sie möglicherweise zusätzlichen Code hinzufügen, der in diesem Beispiel nicht gezeigt wird, um das [storecontext](/uwp/api/windows.services.store.storecontext) -Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Codebeispiel

In diesem Beispiel wird die Verwendung der [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync)-Methode der [StoreContext](/uwp/api/windows.services.store.storecontext)-Klasse veranschaulicht, um eine App oder ein Add-On mit bekannter [Store-ID](in-app-purchases-and-trials.md#store-ids) zu erwerben. Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnablePurchases](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs#PurchaseAddOn)]

## <a name="video"></a>Video

Im folgenden Video sehen Sie eine Übersicht über die Implementierung von in-App-Käufen in Ihrer APP.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Zugehörige Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)