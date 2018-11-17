---
author: Xansky
Description: Whether your app is free or not, you can sell content, other apps, or new app functionality (such as unlocking the next level of a game) from right within the app. Here we show you how to enable these products in your app.
title: Unterstützen des Kaufs von In-App-Produkten
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, Add-Ons, In-App-Käufe, IAPs Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 89e9fff8f041c4beb2a897c7be75b2f6e009f809
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "7151339"
---
# <a name="enable-in-app-product-purchases"></a>Unterstützen von In-App-Produktkäufen

Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.

> [!IMPORTANT]
> Dieser Artikel beschreibt, wie Sie Mitglieder des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace verwenden, um In-App-Produktkäufe zu ermöglichen. Dieser Namespace wird nicht mehr mit neuen Funktionen aktualisiert, daher wird empfohlen, dass Sie stattdessen den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace verwenden. Der **Windows.Services.Store** -Namespace unterstützt die neuesten Add-on-Typen, z. B. Store verwalteten Endverbraucher-Add-Ons und Abonnements, und wurde entwickelt, um die Kompatibilität mit künftigen Arten von Produkten und Features von Partner Center und dem Store unterstützt werden. Der **Windows.Services.Store**-Namespace wurde in Windows10, Version 1607, eingeführt und kann nur in Projekten für die **Windows10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Weitere Informationen zum Aktivieren von in-app-Produktkäufe mit dem **Windows.Services.Store** -Namespace finden Sie [in diesem Artikel](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> In-App-Produkte können nicht in einer Testversion einer App angeboten werden. Kunden, die eine Testversion Ihrer App verwenden, können nur dann In-App-Produkte kaufen, wenn sie eine Vollversion der App kaufen.

## <a name="prerequisites"></a>Voraussetzungen

-   Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.
-   Wenn Sie zum ersten Mal Code für neue In-App-Produkte schreiben und testen, müssen Sie anstelle des [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Dazu müssen Sie die Datei mit dem Namen „WindowsStoreProxy.xml“ in %userprofile%\\AppData\\local\\packages\\&lt;Paketname&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für Universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Schritt 1: Initialisieren der Lizenzinfos für Ihre App

Rufen Sie bei der Initialisierung Ihrer App das [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157)-Objekt für Ihre App ab, indem Sie [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) oder [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) initialisieren, um Käufe von In-App-Produkten zu aktivieren.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Schritt 2: Hinzufügen von In-App-Produktangeboten zu Ihrer App

Erstellen Sie für jedes Feature, das über ein In-App-Produkt zur Verfügung stehen soll, ein Angebot in der App, und fügen Sie es Ihrer App hinzu.

> [!IMPORTANT]
> Sie müssen Ihrer App alle In-App-Produkte hinzufügen, die Sie Ihren Kunden zur Verfügung stellen möchten, bevor Sie sie an den Store übermitteln. Wenn Sie zu einem späteren Zeitpunkt neue In-App-Produkte hinzufügen möchten, müssen Sie die App aktualisieren und eine neue Version übermitteln.

1.  **Erstellen Sie ein Token für In-App-Angebote**

    Sie identifizieren die einzelnen In-App-Produkte Ihrer App durch Token. Bei diesem Token handelt es sich um eine Zeichenfolge, die Sie festlegen und in Ihrer App und im Store verwenden, um ein bestimmtes In-App-Produkt zu identifizieren. Geben Sie ihm einen (für Ihre App) eindeutigen und aussagekräftigen Namen, sodass Sie beim Schreiben des Codes schnell das richtige Feature ermitteln können, für das es steht. Im Folgenden finden Sie einige Beispiele für Namen:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * „RainbowThemePack“

  > [!NOTE]
  > Das in-app-angebotstoken, das Sie in Ihrem Code verwenden muss entsprechen den [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) -Wert, die Sie, wenn angeben Sie [das entsprechende Add-on für Ihre app im Partner Center zu definieren](../publish/add-on-submissions.md).

2.  **Schreiben Sie den Code für das Feature in einem Bedingungsblock.**

    Sie müssen den Code für jedes Feature, das mit einem In-App-Produkt verknüpft ist, in einen Bedingungsblock aufnehmen. Dieser Bedingungsblock überprüft, ob ein Kunde eine Lizenz für die Verwendung dieses Features besitzt.

    Im folgenden Beispiel wird gezeigt, wie Sie Code für das Produkt-Feature **featureName** in einem lizenzspezifischen Bedingungsblock kodieren. Die Zeichenfolge **featureName** ist das Token, das dieses Produkt eindeutig in der App und im Store identifiziert.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Fügen Sie die Kauf-UI für dieses Feature hinzu.**

    Ihre App muss den Kunden außerdem die Möglichkeit bieten, das über das In-App-Produkt angebotene Produkt oder Feature zu kaufen. Das Feature oder Produkt kann nicht auf die gleiche Weise wie die gesamte App im Store erworben werden.

    Hier finden Sie ein Beispiel dafür, wie Sie testen, ob der Kunde bereits ein In-App-Produkt besitzt. Es veranschaulicht außerdem, wie das Kaufdialogfeld angezeigt wird, sodass der Kunde es ggf. erwerben kann. Ersetzen Sie den Kommentar „show the purchase dialog“ durch den benutzerdefinierten Code für das Kaufdialogfeld (z.B. ein Fenster mit der Schaltfläche „Diese App kaufen“) .

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Schritt 3: Ändern Sie den Testcode für die endgültigen Aufrufe.

Dies ist ein einfacher Schritt: Ändern Sie im Code Ihrer App alle Verweise auf [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) in [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Sie müssen die Datei „WindowsStoreProxy.xml“ nicht mehr bereitstellen. Entfernen Sie diese daher aus dem Pfad Ihrer App. Sie können sie jedoch zu späteren Referenzzwecken speichern, wenn Sie im nächsten Schritt das Angebot in der App konfigurieren.

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Schritt 4: Konfigurieren des In-App-Produktangebots im Store

Navigieren Sie im Partner Center zu Ihrer app und [ein Add-on zu erstellen](../publish/add-on-submissions.md) , die Ihrem in-app-Produktangebot entspricht. Definieren Sie Produkt-ID, Typ, Preis und andere Eigenschaften für das Add-On. Die Konfiguration muss genau mit der Konfiguration in der Datei WindowsStoreProxy.xml übereinstimmen, die Sie beim Testen festlegen.

  > [!NOTE]
  > Das in-app-angebotstoken, das Sie in Ihrem Code verwenden, muss die [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) -Wert übereinstimmen, die, den Sie für das entsprechende Add-on im Partner Center angeben.

## <a name="remarks"></a>Hinweise

Wenn Sie Ihren Kunden konsumierbare In-App-Produktoptionen (Elemente, die gekauft, verwendet und erneut gekauft werden können, wenn gewünscht) bereitstellen möchten, wechseln Sie zum Thema [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md).

Wenn Sie anhand von Belegen überprüfen möchten, ob ein Kunde einen In-App-Einkauf getätigt hat, lesen Sie den Artikel [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Verwandte Themen


* [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md)
* [Verwalten eines großen Katalogs von In-App-Produkten](manage-a-large-catalog-of-in-app-products.md)
* [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Einkäufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
