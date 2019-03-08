---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen.
title: Verwalten eines großen Katalogs von In-App-Produkten
ms.date: 08/25/2017
ms.topic: article
keywords: Windows 10, UWP, In-App-Käufe, IAPs, Add-Ons, Katalog, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 2335e09253570d09c33422d2f5ba4179697e4ea7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637355"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Verwalten eines großen Katalogs von In-App-Produkten

Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen. In Versionen vor Windows 10 galt eine Store-Einschränkung von 200 Produkteinträgen pro Entwicklerkonto. Das in diesem Thema beschriebene Verfahren kann zur Umgehung dieser Einschränkung verwendet werden. Ab Windows 10, der Store gibt es keine Beschränkung der Anzahl der Produktlisten pro Developer-Konto, und der in diesem Artikel beschriebene Prozess ist nicht mehr erforderlich.

> [!IMPORTANT]
> In diesem Artikel wird veranschaulicht, wie Mitglieder des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace verwendet werden. Dieser Namespace wird nicht mehr mit neuen Funktionen aktualisiert, daher wird empfohlen, dass Sie stattdessen den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace verwenden. Die **Windows.Services.Store** Namespace unterstützt die neuesten-Add-On-Typen, z. B. nutzbar-Add-Ons Store verwaltete und Abonnements, und mit zukünftige Arten von Produkten und Funktionen, die vom Partner unterstützten kompatibel sein soll Mittelpunkt und dem Store. Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

Um diese Funktionalität zu aktivieren, erstellen Sie einige wenige Produkteinträge für bestimmte Preisniveaus. Jeder kann Hunderte von Produkten innerhalb eines Katalogs repräsentieren. Sie verwenden die [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Methodenüberladung, die ein App-definiertes Angebot angibt, das mit einem im Store aufgelisteten In-App-Produkt verknüpft ist. Zusätzlich zur Angabe einer Verknüpfung von Angebot und Produkt während des Aufrufs sollte Ihre App auch ein [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)-Objekt übergeben, das die Angebotsdetails des großen Katalogs enthält. Wenn diese Details nicht angegeben sind, werden stattdessen die Details für das gelistete Produkt verwendet.

Der Store verwendet nur die *offerId* aus der Kaufanforderung im entsprechenden [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392). Mit diesem Verfahren werden die Informationen, die ursprünglich bei der [Eintragung des In-App-Produkts im Store](../publish/add-on-submissions.md) bereitgestellt wurden, nicht direkt geändert.

## <a name="prerequisites"></a>Voraussetzungen

-   In diesem Thema wird die Store-Unterstützung für die Darstellung mehrerer In-App-Angebote mithilfe eines einzelnen im Store aufgeführten In-App-Produkts erläutert. Wenn Sie mit In-App-Käufen noch nicht vertraut sind, lesen Sie [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md). Dort finden Sie Lizenzinformationen und eine Anleitung zur richtigen Eintragung Ihres In-App-Kaufs im Store.
-   Wenn Sie zum ersten Mal Code für neue In-App-Angebote schreiben und testen, müssen Sie anstelle des [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779766)-Objekts das [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779765)-Objekt verwenden. Auf diese Weise können Sie überprüfen, ob die Lizenzlogik simulierte Aufrufe an den Lizenzserver und nicht an den Liveserver verwendet. Zu diesem Zweck müssen Sie die Datei mit dem Namen WindowsStoreProxy.xml in % USERPROFILE% anpassen\\AppData\\lokalen\\Pakete\\&lt;Paketname&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. Diese Datei wird vom Simulator in Microsoft Visual Studio erstellt, wenn Sie Ihre App zum ersten Mal ausführen. Sie können jedoch auch eine benutzerdefinierte Version dieser Datei zur Laufzeit laden. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   In diesem Thema wird auch auf Codebeispiele verwiesen, die im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store) zu finden sind. Dieses Beispiel bietet eine hervorragende Möglichkeit, die verschiedenen Monetarisierungsoptionen zu testen, die für universelle Windows-Plattform (UWP)-Apps verfügbar sind.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Durchführen der Kaufanforderung für das In-App-Produkt

Die Kaufanforderung für ein bestimmtes Produkt in einem umfangreichen Katalog wird ähnlich gehandhabt wie andere In-App-Kaufanforderungen. Wenn Ihre App die neue [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Methodenüberladung aufruft, stellt die App sowohl ein *OfferId*- als auch ein [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390)-Objekt bereit, aufgefüllt mit dem Namen des In-App-Produkts.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Melden der Erfüllung des In-App-Angebots

Die App muss die Produkterfüllung an den Store melden, sobald die lokale Erfüllung für das Angebot abgeschlossen ist. Wenn die App die Erfüllung des Angebots bei Verwendung eines großen Katalogs nicht meldet, kann der Benutzer keine In-App-Angebote erwerben, für die der gleiche Store-Produkteintrag genutzt wird.

Wie bereits erwähnt, verwendet der Store nur bereitgestellte Angebotsinformationen zum Auffüllen von [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) und erstellt keine persistente Verknüpfung zwischen einem Angebot aus einem großen Katalog und einem Produkteintrag im Store. Daher müssen Sie die Benutzerberechtigungen für Produkte aus großen Katalogen nachverfolgen und dem Benutzer produktspezifischen Kontext (wie den Namen des gekauften Artikels oder Details zu diesem) außerhalb des [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)-Vorgangs zur Verfügung stellen.

Der folgende Code veranschaulicht den Erfüllungsaufruf sowie ein Muster für Meldungen auf der Benutzeroberfläche, in das die angebotsspezifischen Informationen eingefügt werden. Da keine produktspezifischen Informationen vorhanden sind, werden im Beispiel Informationen aus [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163) des Produkts verwendet.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Verwandte Themen

* [Aktivieren von In-App-Produktkäufen](enable-in-app-product-purchases.md)
* [Unterstützen von Käufen konsumierbarer In-App-Produkte](enable-consumable-in-app-product-purchases.md)
* [Store-Beispiel (veranschaulicht das Testversionen und in-app-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)
