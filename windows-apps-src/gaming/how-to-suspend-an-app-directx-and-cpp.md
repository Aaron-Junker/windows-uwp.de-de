---
title: "So wird's gemacht: Anhalten einer App (DirectX und C++)"
description: In diesem Thema wird gezeigt, wie wichtige Systemzustände und App-Daten gespeichert werden, wenn das System Ihre DirectX-App für die universelle Windows-Plattform (UWP) anhält.
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, anhalten, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 0b588d6bf6e7cbf43651d94a7fd46e9a767c6f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656035"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>So wird's gemacht: Anhalten einer App (DirectX und C++)



In diesem Thema wird gezeigt, wie wichtige Systemzustände und App-Daten gespeichert werden, wenn das System Ihre DirectX-App für die universelle Windows-Plattform (UWP) anhält.

## <a name="register-the-suspending-event-handler"></a>Registrieren des Suspending-Ereignishandlers


Registrieren Sie zuerst die Behandlung des [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignisses, das ausgelöst wird, wenn die App durch eine Aktion des Benutzers oder des Systems in den angehaltenen Zustand versetzt wird.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)-Methode Ihres Ansichtsanbieters folgenden Code hinzu:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>Speichern von App-Daten vor dem Anhalten


Wenn die App das [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignis behandelt, kann sie die wichtigen App-Daten in der Handlerfunktion speichern. Die App sollte die [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)-Speicher-API verwenden, um einfache Anwendungsdaten synchron zu speichern. Wenn sie ein Spiel entwickeln, speichern Sie alle wichtigen Informationen zum Spielstand. Vergessen Sie nicht, die Audioverarbeitung anzuhalten!

Implementieren Sie nun den Rückruf. Speichern Sie die Daten der App in dieser Methode.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

Dieser Rückruf muss nach 5 Sekunden beendet werden. Während des Rückrufs müssen Sie durch den Aufruf von [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690), der einen Countdown einleitet, eine Verzögerung anfordern. Rufen Sie nach Abschluss des Speichervorgangs [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) auf, um dem System mitzuteilen, dass die App nun zum Anhalten bereit ist. Wenn Sie keine Verzögerung anfordern oder wenn die App mehr als fünf Sekunden zum Speichern der Daten benötigt, wird die App automatisch angehalten.

Dieser Rückruf tritt als Ereignismeldung auf, die vom [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Element des [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Elements der App verarbeitet wird. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) nicht aufrufen.

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="call-trim"></a>Aufrufen von „Trim()“


Ab Windows 8.1, alle DirectX-UWP-apps müssen Aufrufen [ **IDXGIDevice3::Trim** ](https://msdn.microsoft.com/library/windows/desktop/dn280346) beim Anhalten. Dieser Aufruf weist den Grafiktreiber an, alle für die App zugeordneten temporären Puffer freizugeben. Dadurch wird die Wahrscheinlichkeit verringert, dass die angehaltene App beendet wird, um Arbeitsspeicherressourcen freizugeben. Dies ist eine zertifizierungsanforderung für Windows 8.1.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>Freigeben exklusiver Ressourcen und Dateihandles


Wenn Ihre App das [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignis behandelt, hat sie auch die Möglichkeit, exklusive Ressourcen und Dateihandles freizugeben. Durch die explizite Freigabe von Ressourcen und Dateihandles kann sichergestellt werden, dass andere Apps auf die Ressourcen und Dateihandles zugreifen können, wenn Ihre App sie nicht verwendet. Wenn die App nach der Beendigung aktiviert ist, sollte sie ihre exklusiven Ressourcen und Dateihandles öffnen.

## <a name="remarks"></a>Hinweise


Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird diese vom System fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden.

Das System versucht, die App und die zugehörigen Daten im Arbeitsspeicher beizubehalten, während sie angehalten ist. Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher zu behalten, beendet es die App. Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sendet das System ein [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis und sollte die App-Daten im Handler für das **CoreApplicationView::Activated**-Ereignis wiederherstellen.

Das System benachrichtigt eine App nicht, wenn sie beendet wird. Wenn Ihre App angehalten wird, muss sie daher die App-Daten speichern und die exklusiven Ressourcen und Dateihandles freigeben, damit diese beim erneuten Aktivieren der App nach der Beendigung wiederhergestellt werden können.

## <a name="related-topics"></a>Verwandte Themen

* [Fortsetzen eine app (DirectX und C++)](how-to-resume-an-app-directx-and-cpp.md)
* [Gewusst wie: Aktivieren Sie eine app (DirectX und C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




