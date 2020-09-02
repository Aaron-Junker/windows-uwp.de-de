---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um Lizenzinformationen für die aktuelle App und ihre Add-Ons abzurufen.
title: Abrufen von Lizenzinformationen für Ihre Apps und Add-Ons
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, Lizenzen, apps, Add-ons, in-App-Käufe, IAPS, Windows. Services. Store
ms.localizationpriority: medium
ms.openlocfilehash: 29f2e5ceee6d7d9c779c7fb544d507e31456b3fd
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363163"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Abrufen von Lizenzinformationen zu Apps und Add-Ons

In diesem Artikel wird veranschaulicht, wie die Methoden der [storecontext](/uwp/api/windows.services.store.storecontext) -Klasse im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwendet werden, um Lizenzinformationen für die aktuelle APP und deren Add-ons zu erhalten. Mit diesen Informationen können Sie ermitteln, ob die Lizenzen für die App oder deren Add-Ons aktiv sind, oder ob es sich um Testversionen handelt.

> [!NOTE]
> Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine universelle Windows-Plattform-app (UWP), die auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version.
* Sie haben [eine APP-Übermittlung](../publish/app-submissions.md) im Partner Center erstellt, und diese APP wird im Store veröffentlicht. Optional können Sie die APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist, während Sie Sie testen. Weitere Informationen finden Sie in unserer [Test Anleitung](in-app-purchases-and-trials.md#testing).
* Wenn Sie Lizenzinformationen für ein Add-on für die APP erhalten möchten, müssen Sie [das Add-on auch im Partner Center erstellen](../publish/add-on-submissions.md).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](/uwp/api/windows.ui.xaml.controls.page), die einen [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) mit dem Namen ```workingProgressRing``` und einen [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktop Anwendung verfügen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)verwendet, müssen Sie möglicherweise zusätzlichen Code hinzufügen, der in diesem Beispiel nicht gezeigt wird, um das [storecontext](/uwp/api/windows.services.store.storecontext) -Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Codebeispiel

Verwenden Sie zum Abrufen von Lizenzinformationen für die aktuelle App die [GetAppLicenseAsync](/uwp/api/windows.services.store.storecontext.getapplicenseasync)-Methode. Dabei handelt es sich um eine asynchrone Methode, die ein [storeapplicense](/uwp/api/windows.services.store.storeapplicense) -Objekt zurückgibt, das Lizenzinformationen für die APP bereitstellt, einschließlich Eigenschaften, die angeben, ob der Benutzer derzeit über eine gültige Lizenz für die Verwendung der APP ([IsActive](/uwp/api/windows.services.store.storeapplicense.isactive)) verfügt und ob die Lizenz für eine Testversion ([istrial](/uwp/api/windows.services.store.storeapplicense.istrial)) gilt.

Um auf die Lizenzen für permanente Add-ons der aktuellen App zuzugreifen, für die der Benutzer über die Berechtigung verfügt, zu verwenden, verwenden Sie die [addonlicenses](/uwp/api/windows.services.store.storeapplicense.addonlicenses) -Eigenschaft des [storeapplicense](/uwp/api/windows.services.store.storeapplicense) -Objekts. Diese Eigenschaft gibt eine Auflistung von [storelicense](/uwp/api/windows.services.store.storelicense) -Objekten zurück, die die Add-on-Lizenzen darstellen.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs" id="GetLicenseInfo":::

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Implementieren einer Testversion der App](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
