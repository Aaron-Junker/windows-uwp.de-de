---
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Herunterladen und Installieren von Paketupdates aus dem Store
description: Erfahren Sie, wie im Partner Center-Pakete als obligatorisch kennzeichnen und Schreiben von Code in Ihre app herunterladen und installieren die Paket-Updates.
ms.date: 04/04/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e73452cdcb02798d4ebd225b48272ab77c40fef9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604445"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Herunterladen und Installieren von Paketupdates aus dem Store

Ab Windows 10 (Version 1607) können Sie Methoden der [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace verwenden, um programmgesteuert nach Paketupdates für die aktuelle App im Microsoft Store zu suchen und diese herunterzuladen und zu installieren. Sie können auch für Pakete Abfragen wurden im Partner Center als obligatorisch gekennzeichnet und Funktionen in Ihrer app zu deaktivieren, bis das obligatorische Update installiert wird.

Mithilfe von zusätzlichen [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Methoden, die in Version 1803 von Windows 10 eingeführt wurden, können Sie Paketupdates im Hintergrund herunterladen und installieren (ohne dem Benutzer eine Benachrichtigung anzuzeigen), ein [optionales Paket](optional-packages.md) deinstallieren und Informationen zu Paketen im Download und zur Installationswarteschlange für Ihre App erhalten.

Mithilfe dieser Features können Sie Ihre Benutzerbasis automatisch mit der neuesten Version Ihrer App, optionalen Paketen und verwandten Diensten im Store auf dem neuesten Stand halten.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>Herunterladen und Installieren von Paketupdates mit Zustimmung des Benutzers

In diesem Codebeispiel wird veranschaulicht, wie Sie die [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync)-Methode verwenden, um alle verfügbaren Updates im Store zu ermitteln und dann die [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync)-Methode aufzurufen, um die Updates herunterzuladen und zu installieren. Bei Verwendung dieser Methode zum Herunterladen und Installieren von Updates zeigt das Betriebssystem ein Dialogfeld an, in dem die Berechtigung des Benutzers vor dem Herunterladen der Updates angefragt wird.

> [!NOTE]
> Diese Methoden unterstützen erforderliche [optionale Paketen](optional-packages.md) für Ihre App. Optionale Pakete sind nützlich für herunterladbare Inhalte (DLC)-Add-Ons, da große Apps so im Hinblick auf Größenbeschränkungen geteilt werden, oder auch, um zusätzliche Inhalte getrennt von der Haupt-App zu liefern. Informationen zum Erhalt einer Berechtigung, eine App in den Store zu übermitteln, die optionale Pakete (einschließlich DLC-Add-Ons) verwendet, finden Sie unter [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support).

In diesem Codebeispiel wird von Folgendem ausgegangen:

* Der Code wird im Kontext einer [Seite](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) ausgeführt.
* Die **Seite** enthält eine [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) mit dem Namen ```downloadProgressBar```, um einen Status für den Downloadvorgang bereitzustellen.
* Die Codedatei enthält eine **using**-Anweisung für die Namespaces **Windows.Services.Store**, **Windows.Threading.Tasks** und **Windows.UI.Popups**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Verwenden Sie für eine [Multi-User-App](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications) die [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User)-Methode, um ein **StoreContext**-Objekt anstelle der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault)-Methode abzurufen.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

> [!NOTE]
> Verwenden Sie die [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync)-Methode, um die verfügbare Paketupdates nur herunterzuladen (jedoch nicht zu installieren).

### <a name="display-download-and-install-progress-info"></a>Anzeigen von Statusinformationen für das Herunterladen und Installieren

Wenn Sie die [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync)-Methode oder [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync)-Methode aufrufen, können Sie einen [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress)-Handler hinzufügen, der einmal für jeden Schritt im Downloadprozess (oder Download und Installation) für die einzelnen Pakete in dieser Anforderung aufgerufen wird. Der Handler empfängt ein [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus)-Objekt, das Informationen zum Updatepaket enthält, das die Statusbenachrichtigung ausgelöst hat. Im vorherigen Beispiel wird das Feld **PackageDownloadProgress** des **StorePackageUpdateStatus**-Objekts verwendet, um den Fortschritt des Download- und Installationsprozesses anzuzeigen.

Beachten Sie, dass sich beim Aufruf von **RequestDownloadAndInstallStorePackageUpdatesAsync** zum Herunterladen und Installieren der Paketupdates in einem einzigen Vorgang der Wert des **PackageDownloadProgress**-Felds während des Downloadvorgangs für ein Paket von 0,0 auf 0,8 und dann während der Installation von 0,8 auf 1,0 erhöht. Wenn Sie den auf Ihrer benutzerdefinierten Statusanzeige angezeigten Prozentsatz direkt dem Wert des **PackageDownloadProgress**-Felds zuordnen, zeigt die Anzeige daher 80 % an, wenn das Paket fertig herunterladen ist, und das Betriebssystem zeigt das Installationsdialogfeld an. Wenn Ihre benutzerdefinierte Statusanzeige 100 % anzeigen soll, wenn das Paket heruntergeladen und bereit zum Installieren ist, können Sie den Code so ändern, dass der Statusanzeige 100 % zugewiesen werden, wenn das **PackageDownloadProgress**-Feld 0,8 erreicht.

## <a name="download-and-install-package-updates-silently"></a>Herunterladen und Installieren aller Paketupdates im Hintergrund

Ab Windows 10, Version 1803, können Sie die Methoden [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) und [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) verwenden, Paketupdates im Hintergrund herunterzuladen und zu installieren, ohne dem Benutzer eine Benachrichtigung anzuzeigen. Dieser Vorgang wird nur erfolgreich ausgeführt, wenn der Benutzer die Einstellung **Apps automatisch aktualisieren** im Store aktiviert hat und der Benutzer nicht in einem getakteten Netzwerk ist. Bevor Sie diese Methoden aufrufen, können Sie zunächst die [CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates)-Eigenschaft überprüfen, um festzustellen, ob diese Bedingungen derzeit erfüllt sind.

In diesem Codebeispiel wird veranschaulicht, wie Sie die [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync)-Methode verwenden, um alle verfügbaren Paketupdates zu ermitteln und dann die Methoden [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) und [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) zum Herunterladen und Installieren im Hintergrund zu verwenden.

In diesem Codebeispiel wird von Folgendem ausgegangen:
* Die Codedatei enthält eine **using**-Anweisung für die Namespaces **Windows.Services.Store** und **System.Threading.Tasks**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Verwenden Sie für eine [Multi-User-App](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications) die [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User)-Methode, um ein **StoreContext**-Objekt anstelle der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault)-Methode abzurufen.

> [!NOTE]
> Die Methoden **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater** und **RetryInstallLater**, die in diesem Code aufgerufen werden, sind Platzhaltermethoden, die gemäß den Anforderungen Ihres eigenen App-Designs implementiert werden können.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>Obligatorische Paketupdates

Wenn Sie eine Paket-Eingabe im Partner Center für eine app erstellen, die auf Windows 10, Version 1607 oder höher abzielt können Sie [das Paket als obligatorisch kennzeichnen](../publish/upload-app-packages.md#mandatory-update) sowie Datum und Zeit für die es erforderlich wird. Wenn diese Eigenschaft festgelegt wurde und Ihre App erkennt, dass das Paketupdate verfügbar ist, kann die App ermitteln, ob das Updatepaket obligatorisch ist, und ihr Verhalten ändern, bis das Update installiert ist (z. B. kann Ihre App Features deaktivieren).

> [!NOTE]
> Der obligatorische Status eines Pakets wird von Microsoft nicht erzwungen, und das Betriebssystem verfügt über keine Benutzeroberfläche, die den Benutzer darauf hinweist, dass ein erforderliches App-Update installiert werden muss. Entwickler sollten die Einstellung „Obligatorisch” verwenden, um erforderliche App-Updates in ihrem eigenen Code zu erzwingen.  

So kennzeichnen Sie eine Paketübermittlung als obligatorisch:

1. Melden Sie sich beim [Partner Center](https://partner.microsoft.com/dashboard) und navigieren Sie zu der Seite "Übersicht" Ihrer app.
2. Klicken Sie auf den Namen der Übermittlung, die das Paketupdate enthält, das Sie erforderlich machen möchten.
3. Navigieren Sie zu der **Pakete**-Seite für die Übermittlung. Wählen Sie im unteren Bereich der Seite **Dieses Update als obligatorisch kennzeichnen** aus, und wählen Sie dann den Tag und die Uhrzeit aus, wann das Paketupdate obligatorisch wird. Diese Option gilt für alle UWP-Pakete in der Übermittlung.

Weitere Informationen finden Sie unter [Hochladen von App-Paketen](../publish/upload-app-packages.md).

> [!NOTE]
> Wenn Sie ein [Flight-Paket](../publish/package-flights.md) erstellen, können Sie die Pakete mit einer ähnlichen Benutzeroberfläche auf der **Pakete**-Seite für das Test-Flight als obligatorisch kennzeichnen. In diesem Fall gilt das obligatorische Paketupdate nur für die Kunden, die Teil der Flight-Gruppe sind.

### <a name="code-example-for-mandatory-packages"></a>Codebeispiel für obligatorische Pakete

Im folgenden Codebeispiel wird veranschaulicht, wie Sie feststellen, ob Updatepakete verpflichtend sind. In der Regel sollten Sie Ihre App-Funktionen kontrolliert für den Benutzer herabstufen, wenn ein obligatorisches Paketupdate nicht erfolgreich heruntergeladen oder installiert werden kann.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

## <a name="uninstall-optional-packages"></a>Deinstallieren optionaler Pakete

Ab Windows 10, Version 1803, können Sie die Methoden [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) oder [RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) verwenden, um ein [Optionales Paket](optional-packages.md) (einschließlich eines DLC-Pakets) für die aktuelle App zu deinstallieren. Wenn Sie beispielsweise eine App mit Inhalten haben, die über optionale Pakete installiert wurden, sollten Sie z. B. eine Benutzeroberfläche bereitstellen, die Benutzern ermöglicht, die optionalen Pakete zu deinstallieren, um Speicherplatz freizugeben.

Das folgende Codebeispiel veranschaulicht, wie Sie die [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync)-Methode aufrufen. In diesem Beispiel wird von Folgendem ausgegangen:
* Die Codedatei enthält eine **using**-Anweisung für die Namespaces **Windows.Services.Store** und **System.Threading.Tasks**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Verwenden Sie für eine [Multi-User-App](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications) die [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User)-Methode, um ein **StoreContext**-Objekt anstelle der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault)-Methode abzurufen.

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>Abrufen von Informationen für Downloadwarteschlangen

Ab Windows 10, Version 1803, können Sie die Methoden [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) und [GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) zum Abrufen von Informationen zu den Paketen verwenden, die sich in der aktuellen Download- und Installationswarteschlange des Store befinden. Diese Methoden sind hilfreich, wenn die App oder das Spiel große optionale Pakete (einschließlich DLCs) unterstützt, deren Download und Installation Stunden oder Tage dauern kann, und Sie möchten problemlos auf den Fall reagieren, wenn ein Kunde Ihre App oder Ihr Spiel vor dem Abschluss des Downloads und der Installation schließt. Wenn der Kunde die App oder das Spiel erneut startet, kann Ihr Code diese Methoden zum Abrufen von Informationen über den Zustand der Pakete verwenden, die noch in der Download- und Installationswarteschlange sind, damit Sie dem Kunden den Status der einzelnen Pakete anzeigen können.

Im folgenden Codebeispiel wird veranschaulicht, wie [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) zum Abrufen der Liste der aktiven Paketupdates für die aktuelle App sowie der Statusinformationen für die einzelnen Pakete verwendet wird. In diesem Beispiel wird von Folgendem ausgegangen:
* Die Codedatei enthält eine **using**-Anweisung für die Namespaces **Windows.Services.Store** und **System.Threading.Tasks**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Verwenden Sie für eine [Multi-User-App](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications) die [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User)-Methode, um ein **StoreContext**-Objekt anstelle der [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault)-Methode abzurufen.

> [!NOTE]
> Die Methoden **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI** und **MarkInstallPausedInUI**, die vom Code in diesem Beispiel aufgerufen werden, sind Platzhaltermethoden, die bei Bedarf entsprechend in Ihren eigenen App-Entwurf implementiert werden können.

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Optionale Pakete und die zugehörigen erstellen](optional-packages.md)
