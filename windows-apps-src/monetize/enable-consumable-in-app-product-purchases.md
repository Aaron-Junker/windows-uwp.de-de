---
description: Bieten Sie nutzbare in-App-Produkte&\# 8212; Artikel, die erworben, verwendet und erneut erworben werden können,&\# 8212; über die Store Commerce-Plattform, um Ihren Kunden eine zuverlässige und zuverlässige Kauf Funktion bereitzustellen.
title: Käufe von konsumierbaren In-App-Produkten aktivieren
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: UWP, consudbar, Add-ons, in-App-Käufe, IAPS, Windows. applicationmodel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdd09fdb0d80f7811014dc910772175e152505d0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033542"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Käufe von konsumierbaren In-App-Produkten aktivieren

Sie können In-App-Käufe von Endverbraucherprodukten – Artikel, die gekauft, verwendet und wieder gekauft werden können – über die Store-Handelsplattform anbieten, um Kunden beim Kauf Stabilität und Zuverlässigkeit zu bieten. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann zum Erwerben bestimmter Power-Ups verwendet werden können.

> [!IMPORTANT]
> In diesem Artikel wird veranschaulicht, wie Member des [Windows. applicationmodel. Store](/uwp/api/windows.applicationmodel.store) -Namespace verwendet werden, um nutzbare in-App-Produkt Einkäufe zu ermöglichen. Dieser Namespace wird nicht mehr mit neuen Features aktualisiert, und es wird empfohlen, stattdessen den [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace zu verwenden. Der **Windows. Services. Store** -Namespace unterstützt die neuesten Add-on-Typen, z. b. von Filialen verwaltete, nutzbare Add-ons und Abonnements und ist für die Kompatibilität mit zukünftigen Typen von Produkten und Features konzipiert, die von Partner Center und dem Store unterstützt werden. Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Weitere Informationen zum Aktivieren von nutzbaren in-App-Produkt Käufen mit dem **Windows. Services. Store** -Namespace finden Sie in [diesem Artikel](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Voraussetzungen

-   In diesem Thema erfahren Sie, wie In-App-Produktkäufe von Consumables erfüllt und gemeldet werden. Wenn Sie mit In-App-Produkten noch nicht vertraut sind, lesen Sie [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md). Dort finden Sie Lizenzinformationen und eine Anleitung zur richtigen Eintragung Ihrer In-App-Produkte im Store.
-   Wenn Sie Code für neue In-App-Produkte erstmalig schreiben und testen, müssen Sie anstelle des [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)-Objekts das [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Zu diesem Zweck müssen Sie die Datei mit dem Namen WindowsStoreProxy.xml in% User Profile% \\ AppData \\ local \\ Packages \\ &lt; Package Name &gt; \\ localstate \\ Microsoft \\ Windows Store \\ apidata anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="step-1-making-the-purchase-request"></a>Schritt 1: Durchführen der Kaufanforderung

Die erste Kaufanforderung wird wie bei jedem Kauf über den Store mit [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) durchgeführt. Der Unterschied bei konsumierbaren In-App-Produkten besteht darin, dass ein Kunde nach einem erfolgreichen Kauf das gleiche Produkt erst dann erneut kaufen kann, wenn die App den Store benachrichtigt hat, dass der vorige Kauf des Produkts erfolgreich erfüllt wurde. Ihre App ist dafür verantwortlich, gekaufte Consumables zu erfüllen und den Store über die Erfüllung in Kenntnis zu setzen.

Im nächsten Beispiel wird eine In-App-Kaufanforderung für ein Consumable gezeigt. Sie sehen Codekommentare, die angeben, wann Ihre App den Kauf des konsumierbaren In-App-Produkts lokal erfüllen sollte, und zwar für zwei verschiedene Szenarien – eine erfolgreiche Kaufanforderung und eine nicht erfolgreiche Kaufanforderung aufgrund eines nicht erfüllten Kaufs desselben Produkts.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="MakePurchaseRequest":::

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Schritt 2: Verfolgen der lokalen Kauferfüllung des Verbrauchsartikels

Wenn Sie dem Kunden Zugriff auf das konsumierbare In-App-Produkt gewähren, müssen Sie dokumentieren, für welches Produkt der Kauf erfüllt wird ( *productId* ) und mit welcher Transaktion diese Erfüllung verbunden ist ( *transactionId* ).

> [!IMPORTANT]
> Ihre APP ist dafür verantwortlich, dass die Erfüllung genau an den Store gemeldet wird. Dieser Schritt ist die Grundlage dafür, dass der Kauf von den Kunden als fair und zuverlässig wahrgenommen wird.

Im folgenden Beispiel wird die Verwendung von [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)-Eigenschaften aus dem [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Aufruf im vorangehenden Schritt zum Identifizieren des Produkts für die Kauferfüllung gezeigt. Es wird eine Sammlung verwendet, um die Produktinformationen an einem Ort zu speichern, auf den später verwiesen werden kann, um zu bestätigen, dass die lokale Erfüllung erfolgreich war.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GrantFeatureLocally":::

Im folgenden Beispiel wird die Verwendung des Arrays aus dem vorhergehenden Beispiel gezeigt, um auf Produkt-ID/Transaktions-ID-Paare zuzugreifen, die später beim Melden der Erfüllung an den Store verwendet werden.

> [!IMPORTANT]
> Je nachdem, welche Methodik Ihre APP zum Nachverfolgen und bestätigen der Erfüllung verwendet, muss Ihre APP eine Due-Sorgfalt vorweisen, um sicherzustellen, dass ihren Kunden keine Gebühren für nicht erhaltene Elemente berechnet werden.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="IsLocallyFulfilled":::

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Schritt 3: Melden der Produkterfüllung an den Store

Nachdem die lokale Erfüllung abgeschlossen wurde, muss Ihre App einen [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync)-Aufruf ausführen, der die *productId* und die Transaktion des Produktkaufs enthält.

> [!IMPORTANT]
> Wenn Sie nicht mit dem Bericht erfüllte nutzbare in-App-Produkte im Geschäft melden, kann der Benutzer dieses Produkt erst wieder kaufen, wenn die Ausführung des vorherigen Kaufs gemeldet wird.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="ReportFulfillment":::

## <a name="step-4-identifying-unfulfilled-purchases"></a>Schritt 4: Identifizieren nicht erfüllter Käufe

Ihre App kann jederzeit mithilfe der [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync)-Methode eine Überprüfung auf nicht erfüllte Käufe konsumierbarer In-App-Produkte durchführen. Diese Methode sollte regelmäßig aufgerufen werden, um nicht erfüllte Käufe konsumierbarer Produkte zu ermitteln, die aufgrund unerwarteter Ereignisse in der App aufgetreten sind, beispielsweise Unterbrechungen der Netzwerkverbindung oder Beenden der App.

Im folgenden Beispiel wird gezeigt, auf welche Weise [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) verwendet werden kann, um nicht erfüllte Käufe konsumierbarer Produkte aufzuzählen, und wie Ihre App diese Liste iterieren kann, um die lokale Erfüllung abzuschließen.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GetUnfulfilledConsumables":::

## <a name="related-topics"></a>Verwandte Themen

* [Unterstützen des Kaufs von In-App-Produkten](enable-in-app-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 
