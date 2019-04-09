---
Description: Durch einen kostenlose, zeitlich begrenzte Testversion Ihrer App mit eingeschränkten Features können Sie Ihre Kunden motivieren, auf die Vollversion Ihrer App zu aktualisieren.
title: Ausschließen oder Einschränken von Features in einer Testversion
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: Windows 10, UWP, Testversion, In-App-Kauf, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38590282a95e29ab240486e9c4a3f9cb9afe229c
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335098"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>Ausschließen oder Einschränken von Features in einer Testversion

Durch einen kostenlose, zeitlich begrenzte Testversion Ihrer App mit eingeschränkten Features können Sie Ihre Kunden motivieren, auf die Vollversion Ihrer App zu aktualisieren. Bestimmen Sie die einzuschränkenden Features, bevor Sie mit dem Codieren beginnen, und stellen Sie dann sicher, dass diese nur beim Erwerb einer Lizenz für die Vollversion der App verfügbar sind. Außerdem können Sie Features wie Banner oder Wasserzeichen aktivieren, die nur in der Testversion angezeigt werden, bevor ein Kunde Ihre App kauft.

> [!IMPORTANT]
> Dieser Artikel beschreibt, wie Sie Mitglieder des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)-Namespace verwenden, um Testfunktionen zu implementieren. Dieser Namespace wird nicht mehr mit neuen Funktionen aktualisiert, daher wird empfohlen, dass Sie stattdessen den [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) Namespace verwenden. Die **Windows.Services.Store** Namespace unterstützt die neuesten-Add-On-Typen, z. B. nutzbar-Add-Ons Store verwaltete und Abonnements, und mit zukünftige Arten von Produkten und Funktionen, die vom Partner unterstützten kompatibel sein soll Mittelpunkt und dem Store. Der **Windows.Services.Store**-Namespace wurde in Windows 10, Version 1607, eingeführt und kann nur in Projekten für die **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer neueren Version in Visual Studio verwendet werden. Weitere Informationen zum Implementieren von Testfunktionen mithilfe des **Windows.Services.Store**-Namespace finden Sie in [diesem Artikel](implement-a-trial-version-of-your-app.md).

## <a name="prerequisites"></a>Vorraussetzungen

Eine Windows-App zum Hinzufügen von Features zum Kauf für Kunden.

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>Schritt 1: Wählen Sie die Funktionen, die Sie verwenden möchten, aktivieren oder deaktivieren Sie während des Testzeitraums

Der aktuelle Lizenzstatus Ihrer App wird als Eigenschaften der [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157)-Klasse gespeichert. In der Regel nehmen Sie die vom Lizenzstatus abhängigen Funktionen in einen Bedingungsblock auf wie im nächsten Schritt beschrieben. Stellen Sie beim Auswählen dieser Features sicher, dass sie auf eine Weise implementiert werden können, dass sie in jedem Lizenzstatus funktionieren.

Überlegen Sie auch, wie Änderungen an der App-Lizenz während der Ausführung der App verarbeitet werden sollen. Ihre Test-App kann alle Features, jedoch zusätzlich In-App-Anzeigenbanner enthalten, die die kostenpflichtige Version nicht enthält. Oder in der Test-App sind bestimmte Features deaktiviert, oder es werden regelmäßig Aufforderungen zum Kauf angezeigt.

Überlegen Sie, welche Art App Sie erstellen und welche Test- oder Ablaufstrategie sich gut für sie eignet. Bei einer Testversion für ein Spiel hat es sich bewährt, den Umfang des Spielinhalts, den ein Anwender nutzen kann, zu beschränken. Bei der Testversion eines Hilfsprogramms möchten Sie vielleicht ein Ablaufdatum festlegen oder die Features einschränken, die ein potenzieller Kunde verwenden kann.

Bei den meisten Apps, die keine Spiele sind, ist das Festlegen eines Ablaufdatums eine gute Methode, da Benutzer ein gutes Verständnis für die vollständige App entwickeln. Im Folgenden sind häufige Ablaufszenarien und Ihre Optionen für ihre Handhabung aufgeführt.

-   **Testlizenz abläuft, während die app ausgeführt wird**

    Falls die Testversion abläuft, während die App ausgeführt wird, kann Ihre App:

    -   keine Aktion ausführen.
    -   dem Kunden eine Meldung anzeigen.
    -   Schließen Sie.
    -   den Kunden auffordern, die App zu kaufen.

    Die beste Vorgehensweise besteht darin, eine Meldung mit der Aufforderung zum Kauf der App anzuzeigen und nach dem Kauf die App mit allen aktivierten Features fortzusetzen. Wenn der Kunde die App nicht kauft, wird die App geschlossen, oder der Kunde wird in regelmäßigen Abständen daran erinnert, die App zu kaufen.

-   **Testlizenz abläuft, bevor die app gestartet wird**

    Falls die Testversion abläuft, bevor der Benutzer die App startet, wird Ihre App nicht gestartet. Stattdessen sehen Benutzer ein Dialogfeld, das ihnen die Möglichkeit bietet, die App im Windows Store zu kaufen.

-   **Kunde kauft die app aus, während er ausgeführt wird**

    Wenn der Kunde Ihre App während der Ausführung erwirbt, kann die App:

    -   keine Aktion ausführen und die Fortsetzung im Testmodus bis zum Neustart der App zulassen.
    -   dem Kunden für den Kauf danken oder eine andere Meldung anzeigen.
    -   die Features aktivieren, die mit einer Volllizenz verfügbar sind (oder die Meldungen, dass es sich um eine Testversion handelt, deaktivieren).

Falls Sie die Lizenzänderung ermitteln und eine Aktion in der App ausführen möchten, müssen Sie einen Ereignishandler hinzufügen, wie im nächsten Schritt beschrieben.

## <a name="step-2-initialize-the-license-info"></a>Schritt 2: Initialisieren Sie die Lizenzinformationen

Rufen Sie beim Initialisieren der App das [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157)-Objekt für die App ab wie im folgenden Beispiel beschrieben. Es wird angenommen, dass **licenseInformation** eine globale Variable oder ein Feld vom Typ **LicenseInformation** ist.

Zu diesem Zeitpunkt erhalten Sie simulierte Lizenzinformationen mithilfe von [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) anstelle von [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Bevor Sie die endgültige Version Ihrer App an den **Store** übermitteln, müssen Sie alle **CurrentAppSimulator**-Verweise in Ihrem Code durch **CurrentApp** ersetzen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

Fügen Sie als Nächstes einen Ereignishandler hinzu, um Benachrichtigungen zu Lizenzänderungen während der Ausführung der App zu erhalten. Die App-Lizenz kann sich zum Beispiel ändern, wenn der Testzeitraum abläuft oder der Kunde die App in einem Store kauft.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>Schritt 3: Das Feature in bedingte Blöcke

Beim Auslösen des Lizenzänderungsereignisses muss die App über einen Aufruf der Lizenz-API ermitteln, ob sich der Teststatus geändert hat. Der Code in diesem Schritt veranschaulicht, wie der Handler für dieses Ereignis strukturiert werden muss. Wenn ein Benutzer die App gekauft hat, wird empfohlen, den Benutzer zu diesem Zeitpunkt über den geänderten Lizenzstatus zu informieren. Gegebenenfalls müssen Sie den Benutzer zum Neustarten der App auffordern, falls Ihre Programmierung dies erfordert. Versuchen Sie jedoch, diesen Übergang so nahtlos und problemlos wie möglich zu gestalten.

Dieses Beispiel zeigt, wie der Lizenzstatus einer App ermittelt wird, um ein Feature Ihrer App zu aktivieren oder zu deaktivieren.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>Schritt 4: Der app-Ablauf des Testzeitraums Datum abrufen

Fügen Sie Code ein, um das Ablaufdatum der Testversion einer App abzurufen.

Der Code in diesem Beispiel legt eine Funktion fest, mit der das Ablaufdatum der Testversion einer App abgerufen wird. Ist die Lizenz noch gültig, wird das Ablaufdatum zusammen mit den verbleibenden Tagen bis zum Ablauf des Testzeitraums angezeigt.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>Schritt 5: Testen der Funktionen, die mit der simulierte Aufrufe der API-Lizenz

Testen Sie nun Ihre App mithilfe simulierter Daten. **CurrentAppSimulator** ruft testspezifische Lizenzierung Informationen aus einer XML-Datei mit dem Namen WindowsStoreProxy.xml, im Verzeichnis % USERPROFILE%\\AppData\\lokalen\\Pakete\\ &lt; Paketname&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Sie können WindowsStoreProxy.xml bearbeiten, um die simulierten Ablaufdaten für die App und die Features zu ändern. Testen Sie alle möglichen Ablauf- und Lizenzkonfigurationen, um sicherzustellen, dass alles wie beabsichtigt funktioniert. Weitere Informationen finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).

Wenn dieser Pfad und diese Datei nicht vorhanden sind, müssen Sie diese während der Installation oder zur Laufzeit erstellen. Wenn die Datei WindowsStoreProxy.xml nicht am angegebenen Speicherort vorhanden ist, und Sie versuchen, auf die [CurrentAppSimulator.LicenseInformation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation)-Eigenschaft zuzugreifen, erhalten Sie eine Fehlermeldung.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>Schritt 6: Ersetzen Sie die simulierten Lizenz-API-Methoden, mit der tatsächlichen-API

Nachdem Sie die App mit dem simulierten Lizenzserver getestet haben, und bevor Sie die App zur Zertifizierung an einen Store übermitteln wie im nächsten Codebeispiel gezeigt, müssen Sie **CurrentAppSimulator** durch **CurrentApp** ersetzen.

> [!IMPORTANT]
> Ihre App muss das **CurrentApp**-Objekt verwenden, wenn Sie sie an einen Store übermitteln, da ansonsten ein Zertifizierungsfehler auftritt.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>Schritt 7: Beschreiben der Funktionsweise der kostenlosen Testversion für Ihre Kunden

Beschreiben Sie, wie sich Ihre App während und nach dem kostenlosen Testzeitraum verhält, sodass Ihre Kunden vom Verhalten Ihrer App nicht überrascht werden.

Weitere Informationen zum Beschreiben Ihrer App finden Sie unter [Erstellen von App-Beschreibungen](https://msdn.microsoft.com/library/windows/apps/mt148529).

## <a name="related-topics"></a>Verwandte Themen

* [Store-Beispiel (veranschaulicht das Testversionen und in-app-Käufe)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Set app – Preise und Verfügbarkeit](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 
