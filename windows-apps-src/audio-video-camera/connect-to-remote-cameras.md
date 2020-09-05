---
ms.assetid: ''
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit Remote Kameras herstellen und eine mediaframesourcegroup abrufen, um Frames von den einzelnen Kameras abzurufen.
title: Herstellen einer Verbindung mit Remotekameras
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1fae76aea28ffb63f6cb0ad5af8c5eb6e3fac6e4
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494366"
---
# <a name="connect-to-remote-cameras"></a>Herstellen einer Verbindung mit Remotekameras

In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem oder mehreren Remote Kameras herstellen und ein [**mediaframesourcegroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) -Objekt erhalten, mit dem Sie Frames von jeder Kamera lesen können. Weitere Informationen zum Lesen von Frames aus einer Medienquelle finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md). Weitere Informationen zur Kopplung mit Geräten finden Sie unter [Pair Devices](../devices-sensors/pair-devices.md).

> [!NOTE] 
> Die in diesem Artikel erläuterten Features sind ab Windows 10, Version 1903, verfügbar.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Erstellen einer devicewatcher-Klasse zum Überwachen von verfügbaren Remote Kameras

Die [**devicewatcher**](/uwp/api/windows.devices.enumeration.devicewatcher) -Klasse überwacht die für Ihre APP verfügbaren Geräte und benachrichtigt Ihre APP, wenn Geräte hinzugefügt oder entfernt werden. Rufen Sie eine Instanz von **devicewatcher** durch Aufrufen von [**DeviceInformation. createwatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)ab, und übergeben Sie eine AQS-Zeichenfolge (Advanced Query Syntax), die den Typ der zu überwachenden Geräte identifiziert. Die AQS-Zeichenfolge, die Netzwerkkamera Geräte angibt, lautet wie folgt:

```syntax
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Die Hilfsmethode " [**mediaframesourcegroup. getdeviceselector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) " gibt eine AQS-Zeichenfolge zurück, mit der lokal verbundene und Remote Netzwerkkameras überwacht werden. Wenn Sie nur Netzwerkkameras überwachen möchten, sollten Sie die oben gezeigte AQS-Zeichenfolge verwenden.

Wenn Sie den zurückgegebenen **devicewatcher** starten, indem Sie die [**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) -Methode aufrufen, wird das [**hinzugefügte**](/uwp/api/windows.devices.enumeration.devicewatcher.added) Ereignis für jede derzeit verfügbare Netzwerkkamera erhoben. Bis Sie den Watcher durch Aufrufen von [**Beenden**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)beenden, wird das **hinzugefügte** Ereignis ausgelöst, wenn neue Netzwerkkamera Geräte verfügbar werden und das [**entfernte**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) Ereignis ausgelöst wird, wenn ein Kamera Gerät nicht mehr verfügbar ist.

Die Ereignis Argumente, die an die **hinzugefügten** und **entfernten** Ereignishandler übergeben werden, sind ein Objekt vom Typ " [**toviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) " bzw. " [**toviceinformationupdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) ". Jedes dieser Objekte verfügt über eine **ID** -Eigenschaft, bei der es sich um den Bezeichner der Netzwerkkamera handelt, für die das Ereignis ausgelöst wurde. Übergeben Sie diese ID an die [**mediaframesourcegroup. fromittel Async**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Methode, um ein [**mediaframesourcegroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) -Objekt abzurufen, das Sie zum Abrufen von Frames von der Kamera verwenden können.

## <a name="remote-camera-pairing-helper-class"></a>Hilfsklasse für Remote Kamera Kopplung

Das folgende Beispiel zeigt eine Hilfsklasse, die einen **devicewatcher** verwendet, um eine **ObservableCollection** von **mediaframesourcegroup** -Objekten zu erstellen und zu aktualisieren, um die Datenbindung an die Liste der Kameras zu unterstützen. Typische apps würden die **mediaframesourcegroup** in eine benutzerdefinierte Modell Klasse einschließen. Beachten Sie, dass die Hilfsklasse einen Verweis auf den [**coredispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) der APP beibehält und die Sammlung von Kameras innerhalb von Aufrufen von [**runasync**](/uwp/api/windows.ui.core.coredispatcher.runasync) aktualisiert, um sicherzustellen, dass die an die Auflistung gebundene Benutzeroberfläche im UI-Thread aktualisiert wird.

Außerdem wird in diesem Beispiel das [**devicewatcher. aktualisierte**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) -Ereignis zusätzlich zu den **hinzugefügten** und **entfernten** Ereignissen behandelt. Im **aktualisierten** Handler wird das zugehörige Remote Kamera Gerät aus entfernt und dann wieder der Sammlung hinzugefügt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::

```cppwinrt
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Media.Capture.Frames.h>
#include <winrt/Windows.UI.Core.h>
using namespace winrt;
using namespace winrt::Windows::Devices::Enumeration;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::Media::Capture::Frames;
using namespace winrt::Windows::UI::Core;

struct RemoteCameraPairingHelper
{
    RemoteCameraPairingHelper(CoreDispatcher uiDispatcher) :
        m_dispatcher(uiDispatcher)
    {
        m_remoteCameraCollection = winrt::single_threaded_observable_vector<MediaFrameSourceGroup>();
        auto remoteCameraAqs =
            LR"(System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"")"
            LR"(AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True)";
        m_watcher = DeviceInformation::CreateWatcher(remoteCameraAqs);
        m_watcherAddedAutoRevoker = m_watcher.Added(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Added });
        m_watcherRemovedAutoRevoker = m_watcher.Removed(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Removed });
        m_watcherUpdatedAutoRevoker = m_watcher.Updated(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Updated });
        m_watcher.Start();
    }
    ~RemoteCameraPairingHelper()
    {
        m_watcher.Stop();
    }
    IObservableVector<MediaFrameSourceGroup> FrameSourceGroups()
    {
        return m_remoteCameraCollection;
    }
    winrt::fire_and_forget Watcher_Added(DeviceWatcher /* sender */, DeviceInformation args)
    {
        co_await AddDeviceAsync(args.Id());
    }
    winrt::fire_and_forget Watcher_Removed(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
    }
    winrt::fire_and_forget Watcher_Updated(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
        co_await AddDeviceAsync(args.Id());
    }
    Windows::Foundation::IAsyncAction AddDeviceAsync(winrt::hstring id)
    {
        auto group = co_await MediaFrameSourceGroup::FromIdAsync(id);
        if (group)
        {
            co_await m_dispatcher;
            m_remoteCameraCollection.Append(group);
        }
    }
    Windows::Foundation::IAsyncAction RemoveDevice(winrt::hstring id)
    {
        co_await m_dispatcher;

        uint32_t ix{ 0 };
        for (auto const&& item : m_remoteCameraCollection)
        {
            if (item.Id() == id)
            {
                m_remoteCameraCollection.RemoveAt(ix);
                break;
            }
            ++ix;
        }
    }

private:
    CoreDispatcher m_dispatcher{ nullptr };
    DeviceWatcher m_watcher{ nullptr };
    IObservableVector<MediaFrameSourceGroup> m_remoteCameraCollection;
    DeviceWatcher::Added_revoker m_watcherAddedAutoRevoker;
    DeviceWatcher::Removed_revoker m_watcherRemovedAutoRevoker;
    DeviceWatcher::Updated_revoker m_watcherUpdatedAutoRevoker;
};
```

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Beispiel für Kameraframes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Verarbeiten von Medienframes mit „MediaFrameReader“](process-media-frames-with-mediaframereader.md)
