---
title: So wird's gemacht - Reaktivieren einer App (DirectX und C++)
description: In diesem Thema erfahren Sie, wie wichtige Anwendungsdaten wiederhergestellt werden, wenn das System Ihre DirectX-App für die Universelle Windows-Plattform (UWP) fortsetzt.
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, resuming, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 37bceafae39c314966a95f06a282fe5c91814738
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175284"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>So wird's gemacht - Reaktivieren einer App (DirectX und C++)



In diesem Thema erfahren Sie, wie wichtige Anwendungsdaten wiederhergestellt werden, wenn das System Ihre DirectX-App für die Universelle Windows-Plattform (UWP) fortsetzt.

## <a name="register-the-resuming-event-handler"></a>Registrieren des „resuming“-Ereignishandlers


Registrieren Sie die Behandlung des [**CoreApplication::Resuming**](/uwp/api/windows.applicationmodel.core.coreapplication.resuming)-Ereignisses, mit dem angegeben wird, dass der Benutzer aus der App und wieder zurück gewechselt hat.

Fügen Sie Ihrer Implementierung der [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)-Methode Ihres Ansichtsanbieters folgenden Code hinzu:

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>Aktualisieren der angezeigten Inhalte nach einer Unterbrechung


Wenn die App das Resuming-Ereignis behandelt, kann der angezeigte Inhalt aktualisiert werden. Stellen Sie alle mit dem Handler für [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) gespeicherten Apps wieder her, und setzen Sie die Verarbeitung fort. Für Spieleentwickler: Wenn Sie das Audiomodul angehalten haben, muss es jetzt neu gestartet werden.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

Dieser Rückruf tritt als Ereignismeldung auf, die vom [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Element des [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Elements der App verarbeitet wird. Dieser Rückruf wird nicht aufgerufen, wenn Sie in der Hauptschleife der App (in der [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode des Ansichtsanbieters implementiert) [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) nicht aufrufen.

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

## <a name="remarks"></a>Bemerkungen


Das System hält Ihre App an, wenn der Benutzer zu einer anderen App oder zum Desktop wechselt. Wenn der Benutzer wieder zu Ihrer App wechselt, wird diese vom System fortgesetzt. Beim Fortsetzen der App haben die Variablen und Datenstrukturen den gleichen Inhalt wie vor der Unterbrechung. Das System stellt die App exakt so wieder her, wie sie unterbrochen wurde. Dadurch entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden. Da die App jedoch unter Umständen längere Zeit angehalten war, müssen sämtliche angezeigten Inhalte, die sich möglicherweise in der Zwischenzeit geändert haben, aktualisiert werden, und alle Rendering- und Audioverarbeitungs-Threads müssen neu gestartet werden. Wenn Sie während eines vorherigen Anhalteereignisses Spielstände gespeichert haben, müssen Sie diese nun wiederherstellen.

## <a name="related-topics"></a>Zugehörige Themen

* [So wird's gemacht: Anhalten einer App (DirectX und C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [So wird's gemacht - Aktivieren einer App (DirectX und C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 