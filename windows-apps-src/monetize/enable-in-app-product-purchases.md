---
Description: Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.
title: Unterstützen des Kaufs von In-App-Produkten
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, Add-Ons, In-App-Käufe, IAPs Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a203ef79fc6ebb45107cd9ac9d79cadf330f7a5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604365"
---
# <a name="enable-in-app-product-purchases"></a>Unterstützen des Kaufs von In-App-Produkten

Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.

> [!IMPORTANT]
> Dieser Artikel beschreibt, wie Sie Mitglieder des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace verwenden, um In-App-Produktkäufe zu ermöglichen. Dieser Namespace wird nicht mehr mit neuen Funktionen aktualisiert, daher wird empfohlen, dass Sie stattdessen den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace verwenden. Die **Windows.Services.Store** Namespace unterstützt die neuesten-Add-On-Typen, z. B. nutzbar-Add-Ons Store verwaltete und Abonnements, und mit zukünftige Arten von Produkten und Funktionen, die vom Partner unterstützten kompatibel sein soll Mittelpunkt und dem Store. Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Weitere Informationen zu In-App-Produkten mithilfe des **Windows.Services.Store**-Namespace finden Sie in [diesem Artikel](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> In-App-Produkte können nicht in einer Testversion einer App angeboten werden. Kunden, die eine Testversion Ihrer App verwenden, können nur dann In-App-Produkte kaufen, wenn sie eine Vollversion der App kaufen.

## <a name="prerequisites"></a>Voraussetzungen

-   Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.
-   Wenn Sie zum ersten Mal Code für neue In-App-Produkte schreiben und testen, müssen Sie anstelle des [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Zu diesem Zweck müssen Sie die Datei mit dem Namen WindowsStoreProxy.xml in % USERPROFILE% anpassen\\AppData\\lokalen\\Pakete\\&lt;Paketname&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Schritt 1: Die Lizenzinformationen für Ihre app initialisieren

Rufen Sie bei der Initialisierung Ihrer App das [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157)-Objekt für Ihre App ab, indem Sie [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) oder [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) initialisieren, um Käufe von In-App-Produkten zu aktivieren.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Schritt 2: Hinzufügen von in-app-Angebote zu Ihrer app

Erstellen Sie für jedes Feature, das über ein In-App-Produkt zur Verfügung stehen soll, ein Angebot in der App, und fügen Sie es Ihrer App hinzu.

> [!IMPORTANT]
> Sie müssen Ihrer App alle In-App-Produkte hinzufügen, die Sie Ihren Kunden zur Verfügung stellen möchten, bevor Sie sie an den Store übermitteln. Wenn Sie zu einem späteren Zeitpunkt neue In-App-Produkte hinzufügen möchten, müssen Sie die App aktualisieren und eine neue Version übermitteln.

1.  **Erstellen Sie ein Token im app-Angebot**

    Sie identifizieren die einzelnen In-App-Produkte Ihrer App durch Token. Bei diesem Token handelt es sich um eine Zeichenfolge, die Sie festlegen und in Ihrer App und im Store verwenden, um ein bestimmtes In-App-Produkt zu identifizieren. Geben Sie ihm einen (für Ihre App) eindeutigen und aussagekräftigen Namen, sodass Sie beim Schreiben des Codes schnell das richtige Feature ermitteln können, für das es steht. Im Folgenden finden Sie einige Beispiele für Namen:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > Das Angebot von in-app-Token, die Sie in Ihrem Code verwenden muss übereinstimmen. die [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) Wert, die Sie, wann angeben Sie [definieren Sie die entsprechenden-Add-On für Ihre app im Partner Center](../publish/add-on-submissions.md).

2.  **Die Funktion in einen bedingten Block-Code**

    Sie müssen den Code für jedes Feature, das mit einem In-App-Produkt verknüpft ist, in einen Bedingungsblock aufnehmen. Dieser Bedingungsblock überprüft, ob ein Kunde eine Lizenz für die Verwendung dieses Features besitzt.

    Im folgenden Beispiel wird gezeigt, wie Sie Code für das Produkt-Feature **featureName** in einem lizenzspezifischen Bedingungsblock kodieren. Die Zeichenfolge **featureName** ist das Token, das dieses Produkt eindeutig in der App und im Store identifiziert.

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Fügen Sie den Einkauf Benutzeroberfläche für dieses Feature hinzu.**

    Ihre App muss den Kunden außerdem die Möglichkeit bieten, das über das In-App-Produkt angebotene Produkt oder Feature zu kaufen. Das Feature oder Produkt kann nicht auf die gleiche Weise wie die gesamte App im Store erworben werden.

    Hier finden Sie ein Beispiel dafür, wie Sie testen, ob der Kunde bereits ein In-App-Produkt besitzt. Es veranschaulicht außerdem, wie das Kaufdialogfeld angezeigt wird, sodass der Kunde es ggf. erwerben kann. Ersetzen Sie den Kommentar „show the purchase dialog“ durch den benutzerdefinierten Code für das Kaufdialogfeld (z. B. ein Fenster mit der Schaltfläche „Diese App kaufen“) .

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Schritt 3: Ändern Sie den Testcode für die endgültige Aufrufe

Dies ist ein einfacher Schritt: Ändern Sie im Code Ihrer App alle Verweise auf [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) in [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Sie müssen die Datei „WindowsStoreProxy.xml“ nicht mehr bereitstellen. Entfernen Sie diese daher aus dem Pfad Ihrer App. Sie können sie jedoch zu späteren Referenzzwecken speichern, wenn Sie im nächsten Schritt das Angebot in der App konfigurieren.

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Schritt 4: Konfigurieren Sie das von in-app-Produktangebot in den Store

Navigieren Sie im Partner Center zu Ihrer app und [ein Add-on erstellen](../publish/add-on-submissions.md) , die mit Ihrer in-app-Produktangebot übereinstimmt. Definieren Sie Produkt-ID, Typ, Preis und andere Eigenschaften für das Add-On. Die Konfiguration muss genau mit der Konfiguration in der Datei WindowsStoreProxy.xml übereinstimmen, die Sie beim Testen festlegen.

  > [!NOTE]
  > Das Angebot von in-app-Token, die Sie in Ihrem Code verwenden muss übereinstimmen. die [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) Wert, die Sie, für das entsprechende Add-on im Partner Center angeben.

## <a name="remarks"></a>Hinweise

Wenn Sie Ihren Kunden konsumierbare In-App-Produktoptionen (Elemente, die gekauft, verwendet und erneut gekauft werden können, wenn gewünscht) bereitstellen möchten, wechseln Sie zum Thema [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md).

Wenn Sie anhand von Belegen überprüfen möchten, ob ein Kunde einen In-App-Einkauf getätigt hat, lesen Sie den Artikel [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Verwandte Themen


* [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md)
* [Verwalten Sie einen große in-app-Produktkatalog](manage-a-large-catalog-of-in-app-products.md)
* [Verwenden von Empfangsbestätigungen Produktkäufe überprüfen](use-receipts-to-verify-product-purchases.md)
* [Store-Beispiel (veranschaulicht das Testversionen und in-app-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
