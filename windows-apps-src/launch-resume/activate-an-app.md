---
title: Behandeln der App-Aktivierung
description: Erfahren Sie, wie Sie die App-Aktivierung durch Überschreiben der OnLaunched-Methode behandeln.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: a75136f26aa6cfa330e4118e6709b0b4d4be4054
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626595"
---
# <a name="handle-app-activation"></a>Behandeln der App-Aktivierung

Erfahren Sie, wie zum Verarbeiten der app-Aktivierung durch Überschreiben der [ **Application.OnLaunched** ](/uwp/api/windows.ui.xaml.application.onlaunched) Methode.

## <a name="override-the-launch-handler"></a>Überschreiben des Starthandlers

Beim Aktivieren einer Anwendung aus irgendeinem Grund das System sendet das [ **CoreApplicationView.Activated** ](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) Ereignis. Eine Liste der Aktivierungstypen finden Sie in der [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)-Enumeration.

Die [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)-Klasse definiert Methoden, die außer Kraft gesetzt werden können, um die verschiedenen Aktivierungstypen zu behandeln. Verschiedene Aktivierungstypen verfügen über eine spezifische Methode, die außer Kraft gesetzt werden kann. Setzen Sie für die übrigen Aktivierungstypen die [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)-Methode außer Kraft.

Definieren Sie die Klasse für Ihre Anwendung.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Überschreiben Sie die [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)-Methode. Diese Methode wird immer dann aufgerufen, wenn der Benutzer die App startet. Der [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731)-Parameter enthält den vorherigen Status der App sowie die Aktivierungsargumente.

> [!NOTE]
> Auf Windows nicht das Starten von Start-Kachel oder eines app-Liste einer angehaltenen app diese Methode aufrufen.

```csharp
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }

      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }

         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```

```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
```

```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>Wiederherstellen von App-Daten, wenn die App angehalten und dann beendet wurde

Wenn der Benutzer zur beendeten App wechselt, sendet das System das [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)-Ereignis, wobei [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) auf **Launch** und [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) auf **Terminated** oder **ClosedByUser** festgelegt ist. Von der App werden die gespeicherten Anwendungsdaten geladen, und der angezeigte Inhalt wird aktualisiert.

```csharp
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

Wenn der Wert von [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) gleich **NotRunning** ist, konnten die Anwendungsdaten von der App nicht erfolgreich gespeichert werden. Die App muss in diesem Fall neu gestartet werden, als ob sie erstmalig gestartet wird.

## <a name="remarks"></a>Hinweise

> [!NOTE]
> Apps können die Initialisierung überspringen, wenn für das aktuelle Fenster bereits Inhalte festgelegt wurden. Sehen Sie sich die [ **LaunchActivatedEventArgs.TileId** ](https://msdn.microsoft.com/library/windows/apps/br224736) Eigenschaft, um zu bestimmen, ob die app wurde von einer primären als auch eine sekundäre Kachel gestartet und, auf diesen Informationen basierend entscheiden Sie, ob sollten Sie Stellen Sie eine neue oder fortsetzen Sie der app-Erfahrung.

## <a name="important-apis"></a>Wichtige APIs
* [Windows.ApplicationModel.Activation](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Windows.UI.Xaml.Application](https://msdn.microsoft.com/library/windows/apps/br242324)

## <a name="related-topics"></a>Verwandte Themen
* [Handle-app anhalten](suspend-an-app.md)
* [Behandeln von app-fortsetzen](resume-an-app.md)
* [Richtlinien für die app anhalten und fortsetzen](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [App-Lebenszyklus](app-lifecycle.md)
