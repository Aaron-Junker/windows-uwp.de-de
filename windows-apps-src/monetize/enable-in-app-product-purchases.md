---
Description: Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.
title: Unterstützen des Kaufs von In-App-Produkten
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, Add-ons, in-App-Käufe, IAPS, Windows. applicationmodel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac6fc8a6ac39c106e3d5d593a36595097c4bde45
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364113"
---
# <a name="enable-in-app-product-purchases"></a>Unterstützen des Kaufs von In-App-Produkten

Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.

> [!IMPORTANT]
> In diesem Artikel wird die Verwendung von Membern des [Windows. applicationmodel. Store](/uwp/api/windows.applicationmodel.store) -Namespace zum Aktivieren von in-App-Produkt Käufen veranschaulicht. Dieser Namespace wird nicht mehr mit neuen Features aktualisiert, und es wird empfohlen, stattdessen den [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace zu verwenden. Der **Windows. Services. Store** -Namespace unterstützt die neuesten Add-on-Typen, z. b. von Filialen verwaltete, nutzbare Add-ons und Abonnements und ist für die Kompatibilität mit zukünftigen Typen von Produkten und Features konzipiert, die von Partner Center und dem Store unterstützt werden. Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Weitere Informationen zum Aktivieren von in-App-Produkt Käufen mit dem **Windows. Services. Store** -Namespace finden Sie in [diesem Artikel](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> In-App-Produkte können nicht in einer Testversion einer APP angeboten werden. Kunden, die eine Testversion Ihrer App verwenden, können nur dann In-App-Produkte kaufen, wenn sie eine Vollversion der App kaufen.

## <a name="prerequisites"></a>Voraussetzungen

-   Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.
-   Wenn Sie Code für neue In-App-Produkte erstmalig schreiben und testen, müssen Sie anstelle des [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)-Objekts das [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Zu diesem Zweck müssen Sie die Datei mit dem Namen WindowsStoreProxy.xml in% User Profile% \\ AppData \\ local \\ Packages \\ &lt; Package Name &gt; \\ localstate \\ Microsoft \\ Windows Store \\ apidata anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Schritt 1: Initialisieren der Lizenzinfos für Ihre App

Rufen Sie bei der Initialisierung Ihrer App das [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation)-Objekt für Ihre App ab, indem Sie [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) oder [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) initialisieren, um Einkäufe von In-App-Produkten zu aktivieren.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="InitializeLicenseTest":::

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Schritt 2: Hinzufügen von In-App-Produktangeboten zu Ihrer App

Erstellen Sie für jedes Feature, das über ein In-App-Produkt zur Verfügung stehen soll, ein Angebot in der App, und fügen Sie es Ihrer App hinzu.

> [!IMPORTANT]
> Sie müssen Ihrer APP alle in-App-Produkte hinzufügen, die Sie Ihren Kunden präsentieren möchten, bevor Sie Sie an den Store übermitteln. Wenn Sie zu einem späteren Zeitpunkt neue In-App-Produkte hinzufügen möchten, müssen Sie die App aktualisieren und eine neue Version übermitteln.

1.  **Erstellen Sie ein Token für In-App-Angebote**

    Sie identifizieren die einzelnen In-App-Produkte Ihrer App durch Token. Bei diesem Token handelt es sich um eine Zeichenfolge, die Sie festlegen und in Ihrer App und im Store verwenden, um ein bestimmtes In-App-Produkt zu identifizieren. Geben Sie ihm einen (für Ihre App) eindeutigen und aussagekräftigen Namen, sodass Sie beim Schreiben des Codes schnell das richtige Feature ermitteln können, für das es steht. Im Folgenden finden Sie einige Beispiele für Namen:

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > Das in-App-Angebots Token, das Sie in Ihrem Code verwenden, muss mit dem [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) -Wert übereinstimmen, den Sie angeben, wenn Sie [das entsprechende Add-on für Ihre APP im Partner Center definieren](../publish/add-on-submissions.md).

2.  **Schreiben Sie den Code für das Feature in einem Bedingungsblock.**

    Sie müssen den Code für jedes Feature, das mit einem In-App-Produkt verknüpft ist, in einen Bedingungsblock aufnehmen. Dieser Bedingungsblock überprüft, ob ein Kunde eine Lizenz für die Verwendung dieses Features besitzt.

    Im folgenden Beispiel wird gezeigt, wie Sie Code für das Produkt-Feature **featureName** in einem lizenzspezifischen Bedingungsblock kodieren. Die Zeichenfolge **FeatureName**ist das Token, das dieses Produkt innerhalb der APP eindeutig identifiziert und auch zur Identifizierung im Speicher verwendet wird.

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="CodeFeature":::

3.  **Fügen Sie die Kauf-UI für dieses Feature hinzu.**

    Ihre App muss den Kunden außerdem die Möglichkeit bieten, das über das In-App-Produkt angebotene Produkt oder Feature zu kaufen. Das Feature oder Produkt kann nicht auf die gleiche Weise wie die gesamte App im Store erworben werden.

    Hier finden Sie ein Beispiel dafür, wie Sie testen, ob der Kunde bereits ein In-App-Produkt besitzt. Es veranschaulicht außerdem, wie das Kaufdialogfeld angezeigt wird, sodass der Kunde es ggf. erwerben kann. Ersetzen Sie den Kommentar „show the purchase dialog“ durch den benutzerdefinierten Code für das Kaufdialogfeld (z. B. ein Fenster mit der Schaltfläche „Diese App kaufen“) .

    > [!div class="tabbedCodeSnippets"]
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs" id="BuyFeature":::

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Schritt 3: Ändern Sie den Testcode für die endgültigen Aufrufe.

Dies ist ein einfacher Schritt: Ändern Sie im Code Ihrer App alle Verweise auf [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) in [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Sie müssen die Datei „WindowsStoreProxy.xml“ nicht mehr bereitstellen. Entfernen Sie diese daher aus dem Pfad Ihrer App. Sie können sie jedoch zu späteren Referenzzwecken speichern, wenn Sie im nächsten Schritt das Angebot in der App konfigurieren.

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Schritt 4: Konfigurieren des In-App-Produktangebots im Store

Navigieren Sie in Partner Center zu Ihrer APP, und [Erstellen Sie ein Add-on](../publish/add-on-submissions.md) , das Ihrem in-App-Produktangebot entspricht. Definieren Sie die Produkt-ID, den Typ, den Preis und andere Eigenschaften für das Add-on. Die Konfiguration muss genau mit der Konfiguration in der Datei WindowsStoreProxy.xml übereinstimmen, die Sie beim Testen festlegen.

  > [!NOTE]
  > Das in-App-Angebots Token, das Sie in Ihrem Code verwenden, muss mit dem [Produkt-ID](../publish/set-your-add-on-product-id.md#product-id) -Wert übereinstimmen, den Sie für das entsprechende Add-on im Partner Center angeben.

## <a name="remarks"></a>Bemerkungen

Wenn Sie Ihren Kunden konsumierbare In-App-Produktoptionen (Elemente, die gekauft, verwendet und erneut gekauft werden können, wenn gewünscht) bereitstellen möchten, wechseln Sie zum Thema [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md).

Wenn Sie anhand von Belegen überprüfen möchten, ob ein Kunde einen In-App-Einkauf getätigt hat, lesen Sie den Artikel [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Verwandte Themen


* [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md)
* [Verwalten eines großen Katalogs von In-App-Produkten](manage-a-large-catalog-of-in-app-products.md)
* [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
