---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um mit Endverbraucher-Add-Ons zu arbeiten.
title: Unterstützen von Endverbraucher-Add-On-Käufen
keywords: windows 10, uwp, verbrauchbar, add-ons, in-app-käufe, IAPs, Windows.Services.Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0446269fcbde87dfa25b7bff25f7160335950fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636725"
---
# <a name="enable-consumable-add-on-purchases"></a>Unterstützen von Endverbraucher-Add-On-Käufen

Dieser Artikel veranschaulicht die Verwendung von Methoden der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse im [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)-Namespace, um die Erfüllung des Benutzers von Endverbraucher-Add-Ons in Ihren UWP-Apps zu verwalten. Verwenden Sie Endverbraucher-Add-ons für Artikel, die gekauft, verwendet und erneut gekauft werden können. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann zum Erwerben bestimmter Power-Ups verwendet werden können.

> [!NOTE]
> Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Übersicht über Endverbraucher-Add-ons

Apps können zwei Arten von Endverbraucher-Add-Ons anbieten, die sich auf die Art und Weise unterscheiden, wie Erfüllungen verwaltet werden:

* **Von Entwicklern verwaltetes Endverbraucher-Add-On**. Bei dieser Art von Verbrauchsartikel sind Sie dafür verantwortlich, das Guthaben des Benutzers an Elementen zu verfolgen, die das Add-On darstellt, und den Kauf des Add-Ons dem Store als erfüllt zu melden, nachdem der Benutzer alle Elemente genutzt hat. Der Benutzer kann das Add-On erst dann erneut kaufen, nachdem Ihre App den vorherigen Kauf des Add-Ons als erfüllt gemeldet hat.

  Wenn beispielsweise das Add-On 100 Münzen in einem Spiel darstellt und der Benutzer 10 Münzen nutzt, muss die App oder der Dienst den neuen Restbetrag von 90 Münzen für den Benutzer verwalten. Nachdem der Benutzer alle 100 Münzen genutzt hat, muss die App das Add-On als erfüllt melden, und danach kann der Benutzer das Add-On für 100 Münzen erneut kaufen.

* **Vom Store verwalteter Verbrauchsartikel**. Bei dieser Art von Verbrauchsartikel verfolgt der Store das Guthaben des Benutzers an Elementen, die das Add-On darstellt. Wenn der Benutzer Elemente nutzt, sind Sie verantwortlich dafür, diese Elemente dem Store als erfüllt zu melden, und der Store aktualisiert das Guthaben des Benutzers. Der Benutzer kann das Add-On beliebig oft kaufen (er muss die Elemente nicht zuerst verwenden). Ihre App kann das aktuelle Guthaben für den Benutzer jederzeit im Store abfragen.

  Wenn das Add-On beispielsweise eine anfängliche Menge von 100 Münzen in einem Spiel darstellt und der Benutzer 50 Münzen nutzt, meldet die App dem Store, dass 50 Einheiten des Add-Ons verwendet wurden, und der Store aktualisiert den Restbetrag. Wenn der Benutzer dann Ihr Add-on erneut kauft, um 100 weitere Münzen zu erhalten, hat er jetzt 150 Münzen insgesamt.
    > [!NOTE]
    > Vom Store verwaltete Endverbraucher-Add-Ons sind in Windows 10, Version 1607 verfügbar.

Um einem Benutzer ein Endverbraucher-Add-on anzubieten, befolgen Sie dieses allgemeine Verfahren:

1. Ermöglichen Sie Benutzern den [Erwerb des Add-ons](enable-in-app-purchases-of-apps-and-add-ons.md) in Ihrer App.
3. Wenn der Benutzer das Add-on nutzt (z. B. indem er Münzen in einem Spiel ausgibt), [melden Sie das Add-on als erfüllt](enable-consumable-add-on-purchases.md#report_fulfilled).

Auch das [Abrufen des Restbetrags](enable-consumable-add-on-purchases.md#get_balance) für einen vom Store verwalteten Verbrauchsartikel ist jederzeit möglich.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher, geeignet ist.
* Sie haben [erstellt eine app-Einsendung](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center und diese app wird in den Store veröffentlicht. Optional können Sie die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar ist. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Sie haben [erstellt einen verwendbaren-Add-On für die app](../publish/add-on-submissions.md) im Partner Center.

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx), die einen [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) mit dem Namen ```workingProgressRing``` und einen [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Wenn Sie über eine Desktopanwendung verfügen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwendet, müssen Sie möglicherweise zusätzlichen, in diesen Beispielen nicht aufgeführten Code hinzufügen, um das [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Melden eines Endverbraucher-Add-Ons als erfüllt

Nachdem der Benutzer den [Kauf des Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md) über Ihre App tätigt und das Add-On nutzt, muss Ihre App durch Aufrufen der [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync)-Methode der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse das Add-On als erfüllt melden. Sie müssen die folgenden Informationen an diese Methode übergeben:

* Die [Store ID](in-app-purchases-and-trials.md#store-ids) des Add-Ons, das Sie als erfüllt melden möchten.
* Die Einheiten des Add-Ons, das Sie als erfüllt melden möchten.
  * Geben Sie für einen von Entwicklern verwalteten Verbrauchsartikel als Parameter für die *Menge* 1 an. Dadurch wird der Store benachrichtigt, dass der Verbrauchsartikel erfüllt wurde, und der Kunde kann dann den Verbrauchsartikel erneut kaufen. Der Benutzer kann den Verbrauchsartikel erst dann erneut kaufen, wenn Ihre App den Store benachrichtigt hat, dass er erfüllt wurde.
  * Geben Sie für einen vom Store verwalteten Verbrauchsartikel die tatsächliche Anzahl der Einheiten an, die verbraucht wurden. Der Store aktualisiert den Restbetrag für den Verbrauchsartikel.
* Die Tracking-ID für die Erfüllung. Dies ist eine vom Entwickler angegebene GUID, welche die spezielle Transaktion, mit der der Erfüllungsvorgang verknüpft ist, für Nachverfolgungszwecke identifiziert. Weitere Informationen finden Sie in den Hinweisen in [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

In diesem Beispiel wird gezeigt, wie ein vom Store verwalteter Verbrauchsartikel als erfüllt gemeldet wird.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Abrufen des Restbetrags für einen vom Store verwalteten Verbrauchsartikel

Dieses Beispiel zeigt, wie die [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync)-Methode der [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx)-Klasse verwendet wird, um den Restbetrag für ein vom Store verwaltetes Endverbraucher-Add-On abzurufen.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen für apps und -Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von in-app-Käufe von apps und -Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
