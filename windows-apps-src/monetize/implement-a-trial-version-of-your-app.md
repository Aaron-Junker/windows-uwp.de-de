---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um eine Testversion Ihrer App zu implementieren.
title: Implementieren einer Testversion der App
keywords: Windows 10, UWP, Testversion, in-App-Käufe, Windows. Services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8cac33f36e66c1a5f22fc246daab192298e9f876
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167554"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implementieren einer Testversion der App

Wenn Sie [Ihre APP als kostenlose Testversion in Partner Center konfigurieren](../publish/set-app-pricing-and-availability.md#free-trial) , damit Kunden Ihre APP während eines Testzeitraums kostenlos nutzen können, können Sie Ihre Kunden für ein Upgrade auf die Vollversion Ihrer APP verwenden, indem Sie einige Features während des Testzeitraums ausschließen oder einschränken. Bestimmen Sie die einzuschränkenden Features, bevor Sie mit dem Codieren beginnen, und stellen Sie dann sicher, dass diese nur beim Erwerb einer Lizenz für die Vollversion der App verfügbar sind. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

In diesem Artikel wird gezeigt, wie Sie mithilfe der Member der [storecontext](/uwp/api/windows.services.store.storecontext) -Klasse im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace ermitteln, ob der Benutzer über eine Testlizenz für Ihre APP verfügt und benachrichtigt werden, wenn sich der Status der Lizenz ändert, während Ihre APP ausgeführt wird. 

> [!NOTE]
> Der **Windows. Services. Store** -Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten verwendet werden, die auf **Windows 10 Anniversary Edition (10,0;) abzielen. Build 14393)** oder eine spätere Version in Visual Studio. Wenn Ihre App für eine frühere Version von Windows 10 geeignet ist, müssen Sie den [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store)-Namespace anstelle des **Windows.Services.Store**-Namespace verwenden. Weitere Informationen finden Sie in [diesem Artikel](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Richtlinien für die Implementierung einer Testversion

Der aktuelle Lizenzstatus Ihrer App wird als Eigenschaften der [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense)-Klasse gespeichert. In der Regel nehmen Sie die vom Lizenzstatus abhängigen Funktionen in einen Bedingungsblock auf wie im nächsten Schritt beschrieben. Stellen Sie beim Auswählen dieser Features sicher, dass sie auf eine Weise implementiert werden können, dass sie in jedem Lizenzstatus funktionieren.

Überlegen Sie auch, wie Änderungen an der App-Lizenz während der Ausführung der App verarbeitet werden sollen. Ihre Test-App kann alle Features, jedoch zusätzlich In-App-Anzeigenbanner enthalten, die die kostenpflichtige Version nicht enthält. Oder in der Test-App sind bestimmte Features deaktiviert, oder es werden regelmäßig Aufforderungen zum Kauf angezeigt.

Überlegen Sie, welche Art App Sie erstellen und welche Test- oder Ablaufstrategie sich gut für sie eignet. Bei einer Testversion für ein Spiel hat es sich bewährt, den Umfang des Spielinhalts, den ein Anwender nutzen kann, zu beschränken. Bei der Testversion eines Hilfsprogramms möchten Sie vielleicht ein Ablaufdatum festlegen oder die Features einschränken, die ein potenzieller Kunde verwenden kann.

Bei den meisten Apps, die keine Spiele sind, ist das Festlegen eines Ablaufdatums eine gute Methode, da Benutzer ein gutes Verständnis für die vollständige App entwickeln. Im Folgenden sind häufige Ablaufszenarien und Ihre Optionen für ihre Handhabung aufgeführt.

-   **Ablauf der Testlizenz während der App-Ausführung**

    Falls die Testversion abläuft, während die App ausgeführt wird, kann Ihre App:

    -   Sie unternehmen nichts.
    -   dem Kunden eine Meldung anzeigen.
    -   Fast richtig.
    -   den Kunden auffordern, die App zu kaufen.

    Die beste Vorgehensweise besteht darin, eine Meldung mit der Aufforderung zum Kauf der App anzuzeigen und nach dem Kauf die App mit allen aktivierten Features fortzusetzen. Wenn der Kunde die App nicht kauft, wird die App geschlossen, oder der Kunde wird in regelmäßigen Abständen daran erinnert, die App zu kaufen.

-   **Ablauf der Testlizenz vor dem Starten der App**

    Falls die Testversion abläuft, bevor der Benutzer die App startet, wird Ihre App nicht gestartet. Stattdessen sehen Benutzer ein Dialogfeld, das ihnen die Möglichkeit bietet, die App im Windows Store zu kaufen.

-   **Kauf der App durch den Kunden während der App-Ausführung**

    Wenn der Kunde Ihre App während der Ausführung erwirbt, kann die App:

    -   keine Aktion ausführen und die Fortsetzung im Testmodus bis zum Neustart der App zulassen.
    -   dem Kunden für den Kauf danken oder eine andere Meldung anzeigen.
    -   die Features aktivieren, die mit einer Volllizenz verfügbar sind (oder die Meldungen, dass es sich um eine Testversion handelt, deaktivieren).

Beschreiben Sie, wie sich Ihre App während und nach dem kostenlosen Testzeitraum verhält, sodass Ihre Kunden vom Verhalten Ihrer App nicht überrascht werden. Weitere Informationen zum Beschreiben Ihrer App finden Sie unter [Erstellen von App-Beschreibungen](../publish/create-app-store-listings.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Beispiel gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine universelle Windows-Plattform-app (UWP), die auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version.
* Sie haben eine app in Partner Center erstellt, die als [Kostenlose Testversion](../publish/set-app-pricing-and-availability.md) ohne Zeit Limit konfiguriert ist und diese APP im Store veröffentlicht wird. Optional können Sie die APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist, während Sie Sie testen. Weitere Informationen finden Sie in unserer [Test Anleitung](in-app-purchases-and-trials.md#testing).

Der Code in diesem Beispiel geht von folgenden Voraussetzungen aus:
* Die Ausführung des Codes erfolgt im Kontext einer [Seite](/uwp/api/windows.ui.xaml.controls.page), die einen [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) mit dem Namen ```workingProgressRing``` und einen [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) mit dem Namen ```textBlock``` enthält. Diese Objekte werden verwendet, um anzugeben, dass ein asynchroner Vorgang ausgeführt wird, bzw. um Ausgabemeldungen anzuzeigen.
* Die Codedatei enthält eine **using**-Anweisung für den Namespace **Windows.Services.Store**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktop Anwendung verfügen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)verwendet, müssen Sie möglicherweise zusätzlichen Code hinzufügen, der in diesem Beispiel nicht gezeigt wird, um das [storecontext](/uwp/api/windows.services.store.storecontext) -Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Codebeispiel

Ruft während des Initialisierens der App das [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense)-Objekt für Ihre App ab und behandelt das [OfflineLicensesChanged](/uwp/api/windows.services.store.storecontext.offlinelicenseschanged)-Ereignis, um Benachrichtigungen zu empfangen, wenn die Lizenz während der Ausführung der App geändert wird. Die App-Lizenz kann zum Beispiel geändert werden, wenn der Testzeitraum abläuft oder der Kunde die App in einem Store kauft. Ruft die neue Lizenz ab, wenn die Lizenz geändert wird, und aktiviert oder deaktiviert dementsprechend ein Feature Ihrer App.

Wenn ein Benutzer die App gekauft hat, wird empfohlen, den Benutzer zu diesem Zeitpunkt über den geänderten Lizenzstatus zu informieren. Gegebenenfalls müssen Sie den Benutzer zum Neustarten der App auffordern, falls Ihre Programmierung dies erfordert. Versuchen Sie jedoch, diesen Übergang so nahtlos und problemlos wie möglich zu gestalten.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

Eine vollständige Beispielanwendung finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Zugehörige Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen zu Apps und Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von In-App-Käufen von Apps und Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Unterstützen von Endverbraucher-Add-On-Käufen](enable-consumable-add-on-purchases.md)
* [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)