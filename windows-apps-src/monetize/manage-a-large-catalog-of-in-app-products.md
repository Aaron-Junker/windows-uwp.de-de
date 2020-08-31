---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen.
title: Verwalten eines großen Katalogs von In-App-Produkten
ms.date: 08/25/2017
ms.topic: article
keywords: Windows 10, UWP, in-App-Käufe, IAPS, Add-ons, Katalog, Windows. applicationmodel. Store
ms.localizationpriority: medium
ms.openlocfilehash: a6bd4d95365e33ee30df87247b3aec72f70fc5b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158424"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Verwalten eines großen Katalogs von In-App-Produkten

Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen. In Versionen vor Windows 10 galt eine Store-Einschränkung von 200 Produkteinträgen pro Entwicklerkonto. Das in diesem Thema beschriebene Verfahren kann zur Umgehung dieser Einschränkung verwendet werden. Ab Windows 10 gibt es keine Einschränkung der Anzahl von Produkteinträgen pro Entwicklerkonto im Store. Das in diesem Artikel beschriebene Verfahren ist nicht mehr erforderlich.

> [!IMPORTANT]
> In diesem Artikel wird veranschaulicht, wie Member des [Windows. applicationmodel. Store](/uwp/api/windows.applicationmodel.store) -Namespace verwendet werden. Dieser Namespace wird nicht mehr mit neuen Features aktualisiert, und es wird empfohlen, stattdessen den [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace zu verwenden. Der **Windows. Services. Store** -Namespace unterstützt die neuesten Add-on-Typen, z. b. von Filialen verwaltete, nutzbare Add-ons und Abonnements und ist für die Kompatibilität mit zukünftigen Typen von Produkten und Features konzipiert, die von Partner Center und dem Store unterstützt werden. Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

Um diese Funktionalität zu aktivieren, erstellen Sie einige wenige Produkteinträge für bestimmte Preisniveaus. Jeder kann Hunderte von Produkten innerhalb eines Katalogs repräsentieren. Sie verwenden die [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Methodenüberladung, die ein App-definiertes Angebot angibt, das mit einem im Store aufgelisteten In-App-Produkt verknüpft ist. Neben der Angabe einer Zuordnung eines Angebots zu einem Produkt während des Aufrufs sollte Ihre App auch ein [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)-Objekt übergeben, das die Angebotsdetails des großen Katalogs enthält. Wenn diese Details nicht angegeben sind, werden stattdessen die Details für das gelistete Produkt verwendet.

Im Store wird nur die *offerId* aus der Kaufanforderung des entsprechenden [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) verwendet. Mit diesem Verfahren werden die Informationen, die ursprünglich bei der [Eintragung des In-App-Produkts im Store](../publish/add-on-submissions.md) bereitgestellt wurden, nicht direkt geändert.

## <a name="prerequisites"></a>Voraussetzungen

-   In diesem Thema wird die Store-Unterstützung für die Darstellung mehrerer In-App-Angebote mithilfe eines einzelnen im Store aufgeführten In-App-Produkts erläutert. Wenn Sie mit In-App-Käufen noch nicht vertraut sind, lesen Sie [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md). Dort finden Sie Lizenzinformationen und eine Anleitung zur richtigen Eintragung Ihres In-App-Kaufs im Store.
-   Wenn Sie Code für neue In-App-Angebote erstmalig schreiben und testen, müssen Sie anstelle des [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)-Objekts das [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Zu diesem Zweck müssen Sie die Datei mit dem Namen WindowsStoreProxy.xml in% User Profile% \\ AppData \\ local \\ Packages \\ &lt; Package Name &gt; \\ localstate \\ Microsoft \\ Windows Store \\ apidata anpassen. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Durchführen der Kaufanforderung für das In-App-Produkt

Die Kaufanforderung für ein bestimmtes Produkt in einem umfangreichen Katalog wird ähnlich gehandhabt wie andere In-App-Kaufanforderungen. Wenn Ihre App die neue [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Methodenüberladung aufruft, stellt die App sowohl ein *OfferId* - als auch ein [ProductPurchaseDisplayProperties](/uwp/api/windows.applicationmodel.store.productpurchasedisplayproperties)-Objekt mit dem Namen des In-App-Produkts bereit.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Melden der Erfüllung des In-App-Angebots

Die App muss die Produkterfüllung an den Store melden, sobald die lokale Erfüllung für das Angebot abgeschlossen ist. Wenn die App die Erfüllung des Angebots bei Verwendung eines großen Katalogs nicht meldet, kann der Benutzer keine In-App-Angebote erwerben, für die der gleiche Store-Produkteintrag genutzt wird.

Wie bereits erwähnt verwendet der Store nur bereitgestellte Angebotsinformationen zum Auffüllen des [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)-Elements und erstellt keine beständige Zuordnung zwischen einem Angebot aus einem umfangreichen Katalog und einem Produkteintrag im Store. Daher müssen Sie die Benutzerberechtigungen für Produkte aus umfangreichen Katalogen nachverfolgen und dem Benutzer produktspezifischen Kontext (wie z. B. den Namen des angebotenen Artikels oder Details dazu) außerhalb des [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Vorgangs zur Verfügung stellen.

Der folgende Code veranschaulicht den Erfüllungsaufruf sowie ein Muster für Meldungen auf der Benutzeroberfläche, in das die angebotsspezifischen Informationen eingefügt werden. Da keine produktspezifischen Informationen vorhanden sind, werden im Beispiel Informationen aus dem [ListingInformation](/uwp/api/Windows.ApplicationModel.Store.ListingInformation)-Element des Produkts verwendet.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Zugehörige Themen

* [Unterstützen des Kaufs von In-App-Produkten](enable-in-app-product-purchases.md)
* [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md)
* [Store-Beispiel (zeigt Testversionen und In-App-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)
* [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)