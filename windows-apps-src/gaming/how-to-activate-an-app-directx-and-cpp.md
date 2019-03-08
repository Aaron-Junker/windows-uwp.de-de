---
title: So wird's gemacht - Aktivieren einer App (DirectX und C++)
description: In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die Universelle Windows-Plattform (UWP) definieren.
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, DirectX, Aktivierung
ms.localizationpriority: medium
ms.openlocfilehash: 51c2435c8edeac2431198b7b5f3d9b1a307b5b78
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648845"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>So wird's gemacht - Aktivieren einer App (DirectX und C++)



In diesem Thema erfahren Sie, wie Sie die Aktivierungsbenutzeroberfläche für eine DirectX-App für die Universelle Windows-Plattform (UWP) definieren.

## <a name="register-the-app-activation-event-handler"></a>Registrieren des Ereignishandlers für die Aktivierung der App


Registrieren Sie zuerst die Behandlung des [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignisses, das ausgelöst wird, wenn die App vom Betriebssystem gestartet und initialisiert wird.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)-Methode Ihres Ansichtsanbieters (in diesem Beispiel **MyViewProvider**) folgenden Code hinzu:

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>Aktivieren der CoreWindow-Instanz für die App


Beim Start der App müssen Sie einen Verweis auf die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Instanz für Ihre App abrufen. **CoreWindow** enthält den Meldungsverteiler für Fensterereignisse, mit dem die App Fensterereignisse verarbeitet. Rufen Sie diesen Verweis im Rückruf für das App-Aktivierungsereignis durch Aufrufen von [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) ab. Wenn Sie diesen Verweis abgerufen haben, aktivieren Sie das Hauptfenster der App durch Aufrufen von [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>Beginn der Verarbeitung von Ereignismeldungen für das Hauptfenster der App


Ihre Rückrufe treten als Ereignismeldungen auf, die vom [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)-Element des [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Elements der App verarbeitet werden. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) nicht aufrufen.

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

## <a name="related-topics"></a>Verwandte Themen


* [Gewusst wie: Anhalten eine app (DirectX und C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Fortsetzen eine app (DirectX und C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 




