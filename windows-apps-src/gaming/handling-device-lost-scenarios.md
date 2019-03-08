---
title: Behandeln von Szenarien mit entfernten Geräten in Direct3D 11
description: In diesem Thema wird erläutert, wie Sie die Geräteschnittstellenkette für Direct3D und DXGI neu erstellen, wenn die Grafikkarte entfernt oder neu initialisiert wird.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX 11, Gerät verloren gegangen
ms.localizationpriority: medium
ms.openlocfilehash: c11bbf7657644fbf616590f50d75d93f62ed993e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646605"
---
# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Verarbeiten von Gerät entfernt-Szenarien in Direct3D 11



In diesem Thema wird erläutert, wie Sie die Geräteschnittstellenkette für Direct3D und DXGI neu erstellen, wenn die Grafikkarte entfernt oder neu initialisiert wird.

In DirectX 9 kann für Anwendungen die Bedingung "[Gerät ist nicht mehr auffindbar](https://msdn.microsoft.com/library/windows/desktop/bb174714)" entstehen, bei der das D3D-Gerät nicht mehr betriebsbereit ist. Wenn eine Direct3D 9-Vollbildanwendung den Fokus verliert, ist das Direct3D-Gerät „nicht mehr auffindbar“. Alle Versuche, mit einem nicht auffindbaren Gerät zu zeichnen, sind nicht erfolgreich (ohne Warnung). Für Direct3D 11 werden virtuelle Grafikgerätschnittstellen verwendet, damit mehrere Programme dasselbe physische Grafikgerät nutzen können und keine Bedingungen entstehen, in denen Apps die Kontrolle über das Direct3D-Gerät verlieren. Es ist jedoch weiterhin möglich, dass sich die Verfügbarkeit der Grafikkarte ändert. Zum Beispiel:

-   Der Grafiktreiber wird aktualisiert.
-   Das System führt eine Umstellung von einer energiesparenden Grafikkarte auf eine Grafikkarte mit hoher Leistung durch.
-   Das Grafikgerät reagiert nicht mehr und wird zurückgesetzt.
-   Eine Grafikkarte wird physisch angeschlossen oder entfernt.

Unter diesen Umständen wird von DXGI ein Fehlercode zurückgegeben, der angibt, dass das Direct3D-Gerät neu initialisiert werden muss und die Geräteressourcen neu erstellt werden müssen. In dieser exemplarischen Vorgehensweise wird erläutert, wie Direct3D 11-Apps und -Spiele Fälle, in denen die Grafikkarte zurückgesetzt, entfernt oder geändert wird, erkennen und darauf reagieren kann. Codebeispiele werden über die DirectX 11-App (Universelles Windows)-Vorlage, die mit Microsoft Visual Studio 2015 bereitgestellten bereitgestellt.

## <a name="instructions"></a>Anweisungen

### <a name="spanspanstep-1"></a><span></span>Schritt 1:

Fügen Sie eine Überprüfung auf den Fehler „Gerät entfernt“ in die Renderschleife ein. Stellen Sie den Frame dar, indem Sie [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) (bzw. [**Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) usw.) aufrufen. Klicken Sie dann überprüfen, ob sie zurückgegeben [ **DXGI\_Fehler\_Gerät\_entfernt** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) oder **DXGI\_Fehler\_Gerät \_Zurücksetzen**.

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

Fügen Sie auch eine Überprüfung auf den Fehler „Gerät entfernt“ ein, wenn Sie auf Änderungen der Fenstergröße reagieren. Dies ist ein guter Ausgangspunkt zu prüfen, [ **DXGI\_Fehler\_Gerät\_entfernt** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) oder **DXGI\_Fehler\_Gerät\_ Zurücksetzen** verschiedene Ursachen haben:

-   Das Ändern der Größe der Swapchain erfordert das Aufrufen des zugrunde liegenden DXGI-Adapters, von dem der Fehler „Gerät entfernt“ zurückgegeben werden kann.
-   Möglicherweise wurde die App auf einen Monitor verschoben, der an ein anderes Grafikgerät angeschlossen ist.
-   Wenn ein Grafikgerät entfernt oder zurückgesetzt wird, ändert sich häufig die Desktopauflösung. Dies führt zu einer Änderung der Fenstergröße.

Die Vorlage überprüft den HRESULT-Wert, der von [**ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577) zurückgegeben wird:

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

Jedes Mal, die Ihre app empfängt die [ **DXGI\_Fehler\_Gerät\_entfernt** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) Fehler muss alle geräteabhängigen neu zu erstellen und initialisieren Sie das Direct3D-Gerät erneut Ressourcen zu. Geben Sie alle Verweise auf Ressourcen des Grafikgeräts frei, die mit dem vorherigen Direct3D-Gerät erstellt wurden. Diese Ressourcen sind jetzt ungültig, und alle Verweise auf die Swapchain müssen freigegeben werden, bevor eine neue erstellt werden kann.

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

## <a name="remarks"></a>Hinweise


### <a name="investigating-the-cause-of-device-removed-errors"></a>Untersuchen der Ursache für Fehler vom Typ „Gerät entfernt“

Wiederkehrende Probleme mit dem DXGI-Fehler „Gerät entfernt“ können darauf hinweisen, dass von Ihrem Grafikcode während einer Zeichenroutine ungültige Bedingungen erstellt werden. Außerdem können sie auf einen Hardwarefehler oder einen Fehler in der Grafikkarte hinweisen. Rufen Sie zum Untersuchen der Fehler vom Typ „Gerät entfernt“ [**ID3D11Device::GetDeviceRemovedReason**](https://msdn.microsoft.com/library/windows/desktop/ff476526) auf, bevor Sie das Direct3D-Gerät freigeben. Diese Methode gibt einen von sechs möglichen DXGI-Fehlercodes zurück, um die Ursache des Fehlers „Gerät entfernt“ anzugeben:

-   **DXGI\_FEHLER\_GERÄT\_ABGESTÜRZTER**: Der Grafiktreiber reagiert aufgrund eine ungültige Kombination von Grafikbefehle, die von der app gesendet. Wenn Sie diesen Fehler häufiger erhalten, ist dies ein Hinweis darauf, dass Ihre App zum Hängen des Geräts geführt hat und ein Debugvorgang durchgeführt werden muss.
-   **DXGI\_FEHLER\_GERÄT\_ENTFERNT**: Das Grafikgerät physisch entfernt wurde, deaktiviert ist, oder ein treiberupgrade ist aufgetreten. Dies geschieht von Zeit zu Zeit und ist normal. Die App bzw. das Spiel sollte die Geräteressourcen wie in diesem Thema beschrieben neu erstellen.
-   **DXGI\_FEHLER\_GERÄT\_ZURÜCKSETZEN**: Das Grafikgerät Fehler aufgrund ungültiger-Befehls. Wenn Sie diesen Fehler häufig erhalten, kann dies bedeuten, dass vom Code ungültige Zeichenbefehle gesendet werden.
-   **DXGI\_FEHLER\_TREIBER\_INTERN\_FEHLER**: Der Grafiktreiber ist ein Fehler aufgetreten, und das Gerät zurückgesetzt.
-   **DXGI\_FEHLER\_UNGÜLTIGE\_AUFRUFEN**: Die Anwendung bereitgestellte Daten für ungültige Parameter. Auch wenn Sie diesen Fehler nur einmal erhalten, bedeutet dies, dass Ihr Code die Bedingung „Gerät entfernt“ verursacht hat und ein Debugvorgang durchgeführt werden muss.
-   **S\_OK**: Zurückgegeben, wenn ein Grafikgerät wurde aktiviert, deaktiviert oder zurücksetzen, ohne die aktuellen-Grafikgerät für ungültig zu erklären. Dieser Fehler kann beispielsweise zurückgegeben werden, wenn eine App die [Windows Advanced Rasterization Platform (WARP)](https://msdn.microsoft.com/library/windows/desktop/gg615082) verwendet und ein Hardwareadapter verfügbar wird.

Der folgende Code Ruft die [ **DXGI\_Fehler\_Gerät\_entfernt** ](https://msdn.microsoft.com/library/windows/desktop/bb509553) Fehler code, und klicken Sie in der Debugging-Konsole ausgeben. Fügen Sie diesen Code am Anfang der HandleDeviceLost-Methode hinzu:

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Weitere Informationen finden Sie unter [ **GetDeviceRemovedReason** ](https://msdn.microsoft.com/library/windows/desktop/ff476526) und [ **DXGI\_Fehler**](https://msdn.microsoft.com/library/windows/desktop/bb509553).

### <a name="testing-device-removed-handling"></a>Testgerät hat Behandlung entfernt

Die Developer-Eingabeaufforderung für Visual Studio unterstützt das Befehlszeilenprogramm „dxcap“ für die Direct3D-Ereigniserfassung und -Wiedergabe im Zusammenhang mit der Visual Studio-Grafikdiagnose. Können Sie die Befehlszeilenoption folgender Option "-Forcetdr" während Ihrer app die zwingt, ein GPU-Timeouterkennungs- und Wiederherstellungsfunktion-Ereignis ausgeführt wird, wodurch auslösen DXGI\_Fehler\_Gerät\_entfernt und dadurch können Sie den Fehler zu testen. Verarbeitung von Code.

> **Hinweis** DXCap und die Unterstützungs-DLLs werden unter „system32/syswow64“ als Teil der Grafiktools für Windows 10 installiert, die nicht mehr über das Windows SDK verteilt werden. Stattdessen werden sie über das bei Bedarf verfügbare Feature „Grafiktools“ bereitgestellt. Dies ist eine optionale Betriebssystemkomponente, die installiert sein muss, um die Grafiktools unter Windows 10 zu aktivieren und zu verwenden. Weitere Informationen zum Installieren der Grafiktools für Windows 10 finden Sie hier: <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>
