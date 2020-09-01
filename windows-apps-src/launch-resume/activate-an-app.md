---
title: Behandeln der App-Aktivierung
description: Erfahren Sie, wie Sie die App-Aktivierung durch Überschreiben der OnLaunched-Methode behandeln.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 0c3aa86fd8eee3724e092799eea6d34f0b9d453b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164994"
---
# <a name="handle-app-activation"></a>Behandeln der App-Aktivierung

Erfahren Sie, wie Sie die APP-Aktivierung durch Überschreiben der [**Application. ongestartete**](/uwp/api/windows.ui.xaml.application.onlaunched) -Methode behandeln.

## <a name="override-the-launch-handler"></a>Überschreiben des Starthandlers

Wenn eine App aktiviert ist, sendet das System aus irgendeinem Grund das [**coreapplicationview. aktivierte**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) Ereignis. Eine Liste der Aktivierungstypen finden Sie in der [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)-Enumeration.

Die [**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application)-Klasse definiert Methoden, die außer Kraft gesetzt werden können, um die verschiedenen Aktivierungstypen zu behandeln. Verschiedene Aktivierungstypen verfügen über eine spezifische Methode, die außer Kraft gesetzt werden kann. Setzen Sie für die übrigen Aktivierungstypen die [**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated)-Methode außer Kraft.

Definieren Sie die Klasse für Ihre Anwendung.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Überschreiben Sie die [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)-Methode. Diese Methode wird immer dann aufgerufen, wenn der Benutzer die App startet. Der [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)-Parameter enthält den vorherigen Status der App sowie die Aktivierungsargumente.

> [!NOTE]
> Unter Windows wird durch das Starten einer angehaltenen App aus der Start Kachel oder der APP-Liste Diese Methode nicht aufgerufen.

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

Wenn der Benutzer zur beendeten App wechselt, sendet das System das [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)-Ereignis, wobei [**Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) auf **Launch** und [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) auf **Terminated** oder **ClosedByUser** festgelegt ist. Von der App werden die gespeicherten Anwendungsdaten geladen, und der angezeigte Inhalt wird aktualisiert.

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

Wenn der Wert von [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) gleich **NotRunning** ist, konnten die Anwendungsdaten von der App nicht erfolgreich gespeichert werden. Die App muss in diesem Fall neu gestartet werden, als ob sie erstmalig gestartet wird.

## <a name="remarks"></a>Bemerkungen

> [!NOTE]
> Apps können die Initialisierung überspringen, wenn für das aktuelle Fenster bereits Inhalte festgelegt wurden. Sie können die [**launchactivatedeventargs. tileid**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) -Eigenschaft überprüfen, um zu bestimmen, ob die APP von einer primären oder sekundären Kachel gestartet wurde, und entscheiden Sie, ob Sie auf der Grundlage dieser Informationen entscheiden, ob Sie eine neue APP-Oberfläche präsentieren möchten.

## <a name="important-apis"></a>Wichtige APIs
* [Windows. applicationmodel. Activation](/uwp/api/Windows.ApplicationModel.Activation)
* [Windows.UI.Xaml.Application](/uwp/api/Windows.UI.Xaml.Application)

## <a name="related-topics"></a>Zugehörige Themen
* [Behandeln des Anhaltens von Apps](suspend-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)
* [Richtlinien für das Anhalten und Fortsetzen von Apps](./index.md)
* [App-Lebenszyklus](app-lifecycle.md)