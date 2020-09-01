---
title: Behandeln von Szenarien mit entfernten Geräten in Direct3D 11
description: In diesem Thema wird erläutert, wie Sie die Geräteschnittstellenkette für Direct3D und DXGI neu erstellen, wenn die Grafikkarte entfernt oder neu initialisiert wird.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX 11, Gerät verloren
ms.localizationpriority: medium
ms.openlocfilehash: 49356b910879f96d607a8bfeb2586b8da0ea3c69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173154"
---
# <a name="span-iddev_gaminghandling_device-lost_scenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Behandeln von Szenarien mit entfernten Geräten in Direct3D 11



In diesem Thema wird erläutert, wie Sie die Geräteschnittstellenkette für Direct3D und DXGI neu erstellen, wenn die Grafikkarte entfernt oder neu initialisiert wird.

In DirectX 9 kann für Anwendungen die Bedingung "[Gerät ist nicht mehr auffindbar](/windows/desktop/direct3d9/lost-devices)" entstehen, bei der das D3D-Gerät nicht mehr betriebsbereit ist. Wenn eine Direct3D 9-Vollbildanwendung den Fokus verliert, ist das Direct3D-Gerät „nicht mehr auffindbar“. Alle Versuche, mit einem nicht auffindbaren Gerät zu zeichnen, sind nicht erfolgreich (ohne Warnung). Für Direct3D 11 werden virtuelle Grafikgerätschnittstellen verwendet, damit mehrere Programme dasselbe physische Grafikgerät nutzen können und keine Bedingungen entstehen, in denen Apps die Kontrolle über das Direct3D-Gerät verlieren. Es ist jedoch weiterhin möglich, dass sich die Verfügbarkeit der Grafikkarte ändert. Beispiel:

-   Der Grafiktreiber wird aktualisiert.
-   Das System führt eine Umstellung von einer energiesparenden Grafikkarte auf eine Grafikkarte mit hoher Leistung durch.
-   Das Grafikgerät reagiert nicht mehr und wird zurückgesetzt.
-   Eine Grafikkarte wird physisch angeschlossen oder entfernt.

Unter diesen Umständen wird von DXGI ein Fehlercode zurückgegeben, der angibt, dass das Direct3D-Gerät neu initialisiert werden muss und die Geräteressourcen neu erstellt werden müssen. In dieser exemplarischen Vorgehensweise wird erläutert, wie Direct3D 11-Apps und -Spiele Fälle, in denen die Grafikkarte zurückgesetzt, entfernt oder geändert wird, erkennen und darauf reagieren kann. Codebeispiele sind in der DirectX 11-Vorlage für Apps (Universelle Windows-Plattform) in Microsoft Visual Studio 2015 enthalten.

## <a name="instructions"></a>Instructions

### <a name="spanspanstep-1"></a><span></span>Schritt 1:

Fügen Sie eine Überprüfung auf den Fehler „Gerät entfernt“ in die Renderschleife ein. Stellen Sie den Frame dar, indem Sie [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) (bzw. [**Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) usw.) aufrufen. Überprüfen Sie dann, ob das [**DXGI- \_ Fehler \_ Gerät \_ entfernt**](/windows/desktop/direct3ddxgi/dxgi-error) wurde oder ob der **DXGI- \_ Fehler zurück \_ \_ gesetzt**wurde.

Zuerst wird in der Vorlage der HRESULT-Wert gespeichert, der von der DXGI-Swapchain zurückgegeben wird:

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

Nachdem Sie alle anderen Schritte zum Darstellen des Frames ausgeführt haben, führt die Vorlage eine Überprüfung auf den Fehler aufgrund eines entfernten Geräts durch. Bei Bedarf wird eine Methode zum Behandeln der Bedingung „Gerät entfernt“ aufgerufen:

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>Schritt 2:

Fügen Sie auch eine Überprüfung auf den Fehler „Gerät entfernt“ ein, wenn Sie auf Änderungen der Fenstergröße reagieren. Dies ist ein guter Ausgangspunkt, um zu überprüfen, ob das [**DXGI- \_ Fehler \_ Gerät \_ entfernt**](/windows/desktop/direct3ddxgi/dxgi-error) wurde, oder ob die **DXGI- \_ Fehler \_ Geräte \_ ** Zurücksetzung

-   Das Ändern der Größe der Swapchain erfordert das Aufrufen des zugrunde liegenden DXGI-Adapters, von dem der Fehler „Gerät entfernt“ zurückgegeben werden kann.
-   Möglicherweise wurde die App auf einen Monitor verschoben, der an ein anderes Grafikgerät angeschlossen ist.
-   Wenn ein Grafikgerät entfernt oder zurückgesetzt wird, ändert sich häufig die Desktopauflösung. Dies führt zu einer Änderung der Fenstergröße.

Die Vorlage überprüft den HRESULT-Wert, der von [**ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) zurückgegeben wird:

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>Schritt 3:

Jedes Mal, wenn Ihre APP den Fehler "Fehler beim [** \_ \_ \_ Entfernen des DXGI-Fehlers**](/windows/desktop/direct3ddxgi/dxgi-error) " erhält, muss Sie das Direct3D-Gerät erneut initialisieren und alle geräteabhängigen Ressourcen neu erstellen. Geben Sie alle Verweise auf Ressourcen des Grafikgeräts frei, die mit dem vorherigen Direct3D-Gerät erstellt wurden. Diese Ressourcen sind jetzt ungültig, und alle Verweise auf die Swapchain müssen freigegeben werden, bevor eine neue erstellt werden kann.

Die HandleDeviceLost-Methode gibt die Swapchain frei und weist die App-Komponenten an, die Geräteressourcen freizugeben:

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

Anschließend erstellt sie eine neue Swapchain und initialisiert die geräteabhängigen Ressourcen neu, die von der Geräteverwaltungsklasse gesteuert werden:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

Nachdem das Gerät und die Swapchain neu eingerichtet wurden, werden die App-Komponenten angewiesen, geräteabhängige Ressourcen neu zu initialisieren:

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

Wenn die HandleDeviceLost-Methode beendet wird, geht die Steuerung wieder auf die Renderschleife über, die dann mit dem Zeichnen des nächsten Frames fortfährt.

## <a name="remarks"></a>Bemerkungen


### <a name="investigating-the-cause-of-device-removed-errors"></a>Untersuchen der Ursache für Fehler vom Typ „Gerät entfernt“

Wiederkehrende Probleme mit dem DXGI-Fehler „Gerät entfernt“ können darauf hinweisen, dass von Ihrem Grafikcode während einer Zeichenroutine ungültige Bedingungen erstellt werden. Außerdem können sie auf einen Hardwarefehler oder einen Fehler in der Grafikkarte hinweisen. Rufen Sie zum Untersuchen der Fehler vom Typ „Gerät entfernt“ [**ID3D11Device::GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) auf, bevor Sie das Direct3D-Gerät freigeben. Diese Methode gibt einen von sechs möglichen DXGI-Fehlercodes zurück, um die Ursache des Fehlers „Gerät entfernt“ anzugeben:

-   **DXGI \_ Fehler \_ Gerät \_ **nicht reagiert: der Grafiktreiber hat aufgrund einer ungültigen Kombination der von der APP gesendeten Grafikbefehle nicht mehr reagiert. Wenn Sie diesen Fehler häufiger erhalten, ist dies ein Hinweis darauf, dass Ihre App zum Hängen des Geräts geführt hat und ein Debugvorgang durchgeführt werden muss.
-   **DXGI \_ Fehler \_ Gerät \_ entfernt**: das Grafikgerät wurde physisch entfernt, ausgeschaltet, oder es ist ein Treiber Upgrade aufgetreten. Dies geschieht von Zeit zu Zeit und ist normal. Die App bzw. das Spiel sollte die Geräteressourcen wie in diesem Thema beschrieben neu erstellen.
-   **DXGI \_ Fehler \_ beim \_ Zurücksetzen des Geräts**: das Grafikgerät ist aufgrund eines falsch formatierten Befehls fehlgeschlagen. Wenn Sie diesen Fehler häufig erhalten, kann dies bedeuten, dass vom Code ungültige Zeichenbefehle gesendet werden.
-   **DXGI \_ \_Interner Fehler \_ Treiber \_ Fehler**: der Grafiktreiber hat einen Fehler ermittelt und das Gerät zurückgesetzt.
-   **DXGI \_ \_Ungültiger \_ Rückruf**: die Anwendung hat ungültige Parameterdaten bereitgestellt. Auch wenn Sie diesen Fehler nur einmal erhalten, bedeutet dies, dass Ihr Code die Bedingung „Gerät entfernt“ verursacht hat und ein Debugvorgang durchgeführt werden muss.
-   **S \_ OK**: wird zurückgegeben, wenn ein Grafikgerät aktiviert, deaktiviert oder zurückgesetzt wurde, ohne das aktuelle Grafikgerät ungültig zu machen. Dieser Fehler kann beispielsweise zurückgegeben werden, wenn eine App die [Windows Advanced Rasterization Platform (WARP)](/windows/desktop/direct3darticles/directx-warp) verwendet und ein Hardwareadapter verfügbar wird.

Mit dem folgenden Code wird der Fehlercode des [**DXGI- \_ Fehlers " \_ Gerät \_ entfernt**](/windows/desktop/direct3ddxgi/dxgi-error) " abgerufen und in der Debug-Konsole gedruckt. Fügen Sie diesen Code am Anfang der HandleDeviceLost-Methode hinzu:

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Weitere Informationen finden Sie unter Fehler von [**getdeviceremovedreason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) und [**DXGI \_ **](/windows/desktop/direct3ddxgi/dxgi-error).

### <a name="testing-device-removed-handling"></a>Testgerät hat Behandlung entfernt

Die Developer-Eingabeaufforderung für Visual Studio unterstützt das Befehlszeilenprogramm „dxcap“ für die Direct3D-Ereigniserfassung und -Wiedergabe im Zusammenhang mit der Visual Studio-Grafikdiagnose. Sie können die Befehlszeilenoption "-forcetdr" verwenden, während Ihre APP ausgeführt wird. Dadurch wird ein GPU-Timeout Erkennungs-und Wiederherstellungs Ereignis erzwungen. Dadurch wird das DXGI- \_ Fehler \_ Gerät \_ entfernt, und Sie können den Fehler Behandlungs Code testen.

> **Hinweis** Dxcap und seine Unterstützungs-DLLs werden im Rahmen der Grafik Tools für Windows 10 in System32/syswow64 installiert, die nicht mehr über die Windows SDK verteilt werden. Stattdessen werden sie über das bei Bedarf verfügbare Feature „Grafiktools“ bereitgestellt. Dies ist eine optionale Betriebssystemkomponente, die installiert sein muss, um die Grafiktools unter Windows 10 zu aktivieren und zu verwenden. Weitere Informationen zum Installieren der Grafiktools für Windows 10 finden Sie hier: <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>