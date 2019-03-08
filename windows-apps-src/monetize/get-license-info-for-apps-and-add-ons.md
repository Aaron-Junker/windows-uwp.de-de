---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um Lizenzinformationen für die aktuelle App und ihre Add-Ons abzurufen.
title: Abrufen von Lizenzinformationen für Ihre Apps und Add-Ons
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, Lizenzen, Apps, Add-Ons, In-App-Einkäufe, IAPs, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 4d7c832907af17436d588f0fac6c5039d4affa82
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641915"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Abrufen von Lizenzinformationen zu Apps und Add-Ons

Dieser Artikel veranschaulicht die Verwendung von Methoden der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace, um Lizenzinformationen für die aktuelle App und deren Add-Ons abzurufen. Mit diesen Informationen können Sie ermitteln, ob die Lizenzen für die App oder deren Add-Ons aktiv sind, oder ob es sich um Testversionen handelt.

> [!NOTE]
> Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher, geeignet ist.
* Sie haben [erstellt eine app-Einsendung](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center und diese app wird in den Store veröffentlicht. Optional können Sie die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar ist. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Wenn Sie die Lizenzinformationen für ein Add-on für die app abrufen möchten, müssen Sie auch [erstellen Sie das Add-on im Partner Center](../publish/add-on-submissions.md).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktopanwendung verfügen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwendet, müssen Sie möglicherweise zusätzlichen, in diesem Beispiel nicht aufgeführten Code hinzufügen, um das [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Codebeispiel

Verwenden Sie zum Abrufen von Lizenzinformationen für die aktuelle App die [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync)-Methode. Hierbei handelt es sich um eine asynchrone Methode, die ein [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx)-Objekt mit Lizenzinformationen für die App zurückgibt, darunter die Eigenschaften, die angeben, ob der Benutzer zurzeit über eine gültige Lizenz für die App verfügt ([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)), oder ob die Lizenz für eine Testversion gilt ([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)).

Um auf die Lizenzen für dauerhafte Add-Ons der aktuellen App zuzugreifen, für die der Benutzer eine Nutzungsberechtigung hat, verwenden Sie die [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses)-Eigenschaft des [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx)-Objekts. Diese Eigenschaft gibt eine Sammlung von [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx)-Objekten zurück, die den Add-On-Lizenzen für die App entsprechen.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Verwandte Themen

* [In-app-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Aktivieren von in-app-Käufe von apps und -Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Aktivieren Sie nutzbar Add-on-Käufe](enable-consumable-add-on-purchases.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
