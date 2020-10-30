---
description: Der Microsoft Store Services SDK bietet Bibliotheken und Tools zum Hinzufügen von Features zu Ihren Apps, mit denen Sie mehr Geld verdienen und Kunden gewinnen können.
title: Kundengewinnung mit Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 8356367b47242f7bda01da753cc8599aff9edd79
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030533"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Kundengewinnung mit Microsoft Store Services SDK

Das Microsoft Store Services SDK bietet Features, mit denen Sie sich mit Kunden in ihren universelle Windows-Plattform-Apps (UWP) in Verbindung treten können, z. b. das Senden von Benachrichtigungen an Ihre apps und das Ausführen von A/B-Experimenten in ihren apps. Dieses SDK ist eine Erweiterung für Visual Studio 2015 und neuere Versionen von Visual Studio.

> [!NOTE]
> Verwenden Sie zum Anzeigen von anzeigen in ihren UWP-apps das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) anstelle des Microsoft Store Services SDK. Die Werbe Bibliotheken wurden aus dem Microsoft Store Services SDK in das Microsoft Advertising SDK verschoben. Weitere Informationen finden Sie unter [Anzeigen von Werbung in Ihrer App](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Vom Microsoft Store Services SDK Unterstützte Szenarien

Das Microsoft Store Services SDK unterstützt derzeit die folgenden Szenarien für UWP-apps. Eine API-Referenz Dokumentation finden Sie unter [Microsoft Store Services SDK-API-Referenz](/uwp/api/overview/engagement).

|  Szenario  |  BESCHREIBUNG   |
|------------|----------------|
|  [Durchführen von Experimenten mit A/B-Tests in Ihrer UWP-App](run-app-experiments-with-a-b-testing.md)    |  Führen Sie A/B-Tests in Ihrer App für die universelle Windows-Plattform (UWP) aus, um die Effektivität der Features für einige Kunden zu messen, bevor Sie die Features für alle Benutzer freigeben. Nachdem Sie in Partner Center ein Experiment definiert haben, verwenden Sie die Klasse [storeservicesexperimentellen Variation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) , um Variationen für Ihr Experiment in Ihrer APP zu erhalten. verwenden Sie diese Daten, um das Verhalten der zu testenden Funktion zu ändern, und verwenden Sie dann die [logforvariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) -Methode, um Ansichts Ereignis-und Konvertierungs Ereignisse an Partner Center zu senden. Verwenden Sie abschließend Partner Center, um die Ergebnisse anzuzeigen und das Experiment zu verwalten.  |
|  [Starten des Feedback-Hubs über Ihre UWP-App](launch-feedback-hub-from-your-app.md)    |  Verwenden Sie die [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher)-Klasse in Ihrer UWP-App, um Ihre Windows 10-Kunden auf den Feedback-Hub zu verweisen. Dort können Kunden ihre Probleme und Vorschläge übermitteln und das Feedback anderer Benutzer lesen und bewerten. Verwalten Sie anschließend dieses Feedback im [Feedbackbericht](../publish/feedback-report.md) in Partner Center. |
|  [Konfigurieren der UWP-App für den Empfang von Partner Center-Pushbenachrichtigungen](configure-your-app-to-receive-dev-center-notifications.md)    |  Verwenden Sie die [storeservicesengagementmanager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) -Klasse in ihrer UWP-APP, um Ihre APP zu registrieren, um gezielte Pushbenachrichtigungen zu erhalten, die Sie mithilfe von Partner Center an Ihre Kunden senden.  |
|   [Protokollieren von benutzerdefinierten Ereignissen in der UWP-App für den Verwendungs Bericht im Partner Center](log-custom-events-for-dev-center.md)   |  Verwenden Sie die [storeservicescustomeventlogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) -Klasse in ihrer UWP-APP, um benutzerdefinierte Ereignisse zu protokollieren, die mit Ihrer APP im Partner Center verknüpft sind. Überprüfen Sie dann die Gesamtzahl der Vorkommen für Ihre benutzerdefinierten Ereignisse im Abschnitt **benutzerdefinierte Ereignisse** des [Verwendungs Berichts](../publish/usage-report.md) im Partner Center.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Voraussetzungen

Das Microsoft Store Services SDK erfordert:

* Visual Studio 2015 oder eine neuere Version.
* Visual Studio-Tools für universelle Windows-Apps, installiert mit Ihrer Version von Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Installieren des SDKs

Es gibt zwei Optionen für die Installation des Microsoft Store Services SDK auf dem Entwicklungs Computer:

* **MSI-Installer** &nbsp; &nbsp; Sie können das SDK über das [hier](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)verfügbare MSI-Installationsprogramm installieren.
* **Nuget-Paket** &nbsp; &nbsp; Sie können das SDK als nuget-Paket installieren.

Microsoft veröffentlicht in regelmäßigen Abständen neue Versionen des Microsoft Store Services SDKs, die Leistungsverbesserungen und neue Features umfassen. Wenn Sie über vorhandene Projekte verfügen, die das Microsoft Store Services SDK verwenden, und die neueste Version verwenden möchten, laden Sie einfach die neueste Version des SDKs auf Ihren Entwicklungscomputer herunter.

<span id="install-msi" />

### <a name="install-via-msi"></a>Installation über MSI

So installieren Sie das Microsoft Store-Services-SDK über das MSI-Installationsprogramm:

1.  Schließen Sie alle Instanzen von Visual Studio.

2. Wenn Sie zuvor das Microsoft Store Engagement-und monetisierungssdk, das Universal AD Client SDK oder die AD-Vermittler Erweiterung installiert haben, deinstallieren Sie diese SDKs jetzt. Öffnen Sie optional ein **Eingabe** Aufforderungs Fenster, und führen Sie diese Befehle aus, um ältere SDK-Versionen zu bereinigen, die möglicherweise mit Visual Studio installiert wurden, aber in der Liste der installierten Programme auf dem Computer nicht angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Laden Sie das [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) herunter, und installieren Sie es. Die Installation kann einige Minuten dauern. Warten Sie unbedingt, bis der Vorgang abgeschlossen ist.

4.  Starten Sie Visual Studio neu.

5.  Wenn Sie über ein vorhandenes Projekt verfügen, das auf Bibliotheken aus einer früheren Version des Microsoft Store Services SDK verweist, Microsoft Advertising SDK, Universal AD Client SDK oder Microsoft Store Engagement-und monetarisierungssdk wird empfohlen, das Projekt in Visual Studio zu öffnen und das Projekt zu bereinigen und neu zu erstellen (Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projekt Knoten, und wählen Sie dann **Bereinigen** aus, und **Klicken Sie dann** erneut mit der rechten Maustaste auf den Projekt Knoten.

  Wenn Sie das SDK zum ersten Mal in Ihrem Projekt verwenden, können Sie nun [den Assemblyverweis zu Ihrem Projekt hinzufügen](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Installieren über NuGet

So installieren Sie die Microsoft Store Services SDK-Bibliotheken über nuget:

1.  Schließen Sie alle Instanzen von Visual Studio.

2. Wenn Sie zuvor das Microsoft Store Engagement-und monetisierungssdk, das Universal AD Client SDK oder die AD-Vermittler Erweiterung installiert haben, deinstallieren Sie diese SDKs jetzt. Öffnen Sie optional ein **Eingabe** Aufforderungs Fenster, und führen Sie diese Befehle aus, um ältere SDK-Versionen zu bereinigen, die möglicherweise mit Visual Studio installiert wurden, aber in der Liste der installierten Programme auf dem Computer nicht angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Starten Sie Visual Studio, und öffnen Sie das Projekt, in dem Sie das Microsoft Store Services SDK verwenden möchten.
    > [!NOTE]
    > Wenn das Projekt bereits Bibliotheks Verweise aus einer früheren MSI-Installation des SDK enthält, entfernen Sie diese Verweise aus dem Projekt. Diese Verweise sind mit Warnsymbolen versehen, da die Bibliotheken, auf die sie verweisen, in den vorherigen Schritten entfernt wurden.

4. Klicken Sie in Visual Studio auf **Projekt** und **NuGet-Pakete verwalten** .

5. Geben Sie im Suchfeld " **Microsoft. Services. Store. Engagement** " ein, und installieren Sie das Paket "Microsoft. Services. Store. Engagement". Wenn die Installation des Pakets abgeschlossen ist, speichern Sie die Projekt Mappe.
    > [!NOTE]
    > Wenn das **Ausgabe** Fenster einen *Installationspaket* Fehler meldet, der angibt, dass der angegebene Pfad zu lang ist, müssen Sie möglicherweise nuget so konfigurieren, dass Pakete an einen alternativen Speicherort mit einem kürzeren Pfad als der Standard Speicherort extrahiert werden. Fügen Sie hierzu den `repositoryPath`-Wert einer nuget.config-Datei auf Ihrem Computer hinzu, und weisen Sie ihn einem kurzen Ordnerpfad zu, unter dem die NuGet-Pakete extrahiert werden können. Weitere Informationen finden Sie in [diesem Artikel](/nuget/consume-packages/configuring-nuget-behavior) in der NuGet-Dokumentation. Sie können auch versuchen, das Visual Studio-Projekt in einen anderen Ordner mit einem kürzeren Pfad zu verschieben. Das Problem kann auch darauf zurückzuführen sein, dass Ihr globaler Paketpfad zu lang ist. Fügen Sie in diesem Fall den `globalPackagesFolder` Wert ihrer nuget.config Datei hinzu.

6. Schließen Sie die Visual Studio-Projekt Mappe, die das Projekt enthält, und öffnen Sie die Projekt Mappe erneut.

7.  Wenn Ihr Projekt bereits auf Bibliotheken aus einer früheren Version des Microsoft Store Services SDKs verweist, die über NuGet installiert wurde, und Sie Ihr Projekt auf eine neuere Version des SDKs aktualisiert haben, empfehlen wir, das Projekt zu bereinigen und neu zu erstellen (klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Bereinigen** aus, klicken Sie dann mit der rechten Maustaste erneut auf den Projektknoten, und wählen Sie **Neu erstellen** ).

  Wenn Sie das SDK zum ersten Mal in Ihrem Projekt verwenden, können Sie nun [den Assemblyverweis zu Ihrem Projekt hinzufügen](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Hinzufügen des Assemblyverweises zu Ihrem Projekt

Nachdem Sie das Microsoft Store Services SDK über das MSI-Installationsprogramm oder nuget installiert haben, befolgen Sie diese Anweisungen, um auf die SDK-Assembly im UWP-Projekt zu verweisen.

1. Öffnen Sie Ihr Projekt in Visual Studio.
    > [!NOTE]
    > Wenn Ihr Projekt eine JavaScript-APP ist, die auf eine **beliebige CPU** ausgerichtet ist, aktualisieren Sie das Projekt so, dass eine architekturspezifische Buildausgabe (z. b. **x86** ) verwendet wird.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise** , und wählen Sie **Verweis hinzufügen...** aus.

3. Erweitern Sie im **Verweis-Manager** den Knoten **universelle Fenster** , klicken Sie auf **Erweiterungen** , und aktivieren Sie dann das Kontrollkästchen neben **Microsoft Engagement Framework** . Dies ermöglicht es Ihnen, die APIs im [Microsoft. Services. Store. Engagement](/uwp/api/microsoft.services.store.engagement) -Namespace zu verwenden.

3. Klicken Sie auf **OK** .

> [!NOTE]
> Wenn Sie die SDK-Bibliotheken über nuget installiert haben, enthält das Projekt eine **Microsoft. Services. Store. Engagement** -Referenz. Die **Microsoft. Services. Store. Engagement** -Referenz stellt das nuget-Paket dar (und nicht die darin dargestellten Bibliotheken), und Sie können es ignorieren.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Die Frameworkpakete in dem SDK

Die Microsoft.Services.Store.Engagement.dll-Bibliothek im Microsoft Store Services SDK ist als *frameworkpaket* konfiguriert. Diese Bibliothek enthält die APIs im [Microsoft.Services.Store.Engagement](/uwp/api/microsoft.services.store.engagement)-Namespace.

Da diese Bibliothek ein frameworkpaket ist, bedeutet dies, dass diese Bibliothek nach dem Installieren einer Version der APP, die diese Bibliothek verwendet, automatisch über Windows Update aktualisiert wird, wenn wir eine neue Version der Bibliothek mit Korrekturen und Leistungsverbesserungen veröffentlichen. Dadurch wird sichergestellt, dass Ihre Kunden immer die neueste verfügbare Version der Bibliothek auf Ihren Geräten installiert haben.

Wenn wir eine neue Version des SDK veröffentlichen, mit der neue APIs oder Features in diese Bibliothek eingeführt werden, müssen Sie die neueste Version des SDK installieren, um diese Features zu verwenden. In diesem Szenario müssen Sie auch die aktualisierte App im Store veröffentlichen.

## <a name="related-topics"></a>Verwandte Themen

* [Microsoft Store Services SDK-API-Referenz](/uwp/api/overview/engagement)
* [Ausführen von Experimenten mit A/B-Tests](run-app-experiments-with-a-b-testing.md)
* [Starten des Feedback-Hubs über Ihre App](launch-feedback-hub-from-your-app.md)
* [Konfigurieren Ihrer App zum Empfangen von Partner Center-Pushbenachrichtigungen](configure-your-app-to-receive-dev-center-notifications.md)
* [Protokollieren benutzerdefinierter Ereignisse im Partner Center](log-custom-events-for-dev-center.md)
