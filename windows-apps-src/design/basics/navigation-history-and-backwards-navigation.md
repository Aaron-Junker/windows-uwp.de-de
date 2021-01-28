---
description: In diesem Artikel erfahren Sie, wie Sie die Rückwärtsnavigation zum Durchlaufen des Navigationsverlaufs eines Benutzers in einer Windows-App implementieren.
title: Navigationsverlauf und Rückwärtsnavigation
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 269d3b9f256016cb0d10441dc394eb4783ef6ec9
ms.sourcegitcommit: 9bd23e0e08ed834accebde4db96fc87f921d983d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98949126"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Navigationsverlauf und Rückwärtsnavigation für Windows-Apps

> **Wichtige APIs:** [BackRequested-Ereignis](/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [SystemNavigationManager-Klasse](/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)

Die Windows-App enthält ein einheitliches System zur Rückwärtsnavigation, mit dem der Navigationsverlauf des Benutzers innerhalb einer App und je nach Gerät von App zu App durchlaufen werden kann.

Um die Rückwärtsnavigation in Ihrer App zu implementieren, platzieren Sie einen Zurück-Button in der oberen linken Ecke der Benutzeroberfläche Ihrer App. Der Benutzer erwartet, dass durch Drücken der Zurück-Schaltfläche die vorherige Seite im Navigationsverlauf der App aufgerufen wird. Sie können entscheiden, welche Navigationsaktionen dem Navigationsverlauf hinzugefügt werden sollen und wie auf das Drücken der Zurück-Schaltfläche reagiert werden soll.

Für die meisten Apps mit mehreren Seiten empfehlen wir, das Steuerelement [NavigationView](../controls-and-patterns/navigationview.md) zu verwenden, um das Navigationsframework für Ihre App bereitzustellen. Es ermöglicht die Anpassung an verschiedene Bildschirmgrößen und unterstützt sowohl den Navigationsstil _oben_ als auch _links_. Wenn Ihre App das `NavigationView`-Steuerelement verwendet, können Sie die integrierte [Zurück-Schaltfläche von NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation) verwenden.

> [!NOTE]
> Die Richtlinien und Beispiele in diesem Artikel sollten verwendet werden, wenn Sie die Navigation ohne Verwendung des `NavigationView`-Steuerelements implementieren. Bei Verwendung von `NavigationView` liefern diese Informationen nützliches Hintergrundwissen, aber Sie sollten sich an die spezifischen Anleitungen und Beispiele im Artikel [NavigationView](../controls-and-patterns/navigationview.md) halten.

## <a name="back-button"></a>Zurück-Schaltfläche

Um eine Zurück-Schaltfläche zu erstellen, verwenden Sie das [Button](../controls-and-patterns/buttons.md)-Steuerelement mit dem `NavigationBackButtonNormalStyle`-Stil, und platzieren Sie die Schaltfläche in der oberen linken Ecke der Benutzeroberfläche Ihrer App (Einzelheiten hierzu finden Sie unten in den XAML-Beispielen).

![Zurück-Schaltfläche oben links in der App-Oberfläche](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button x:Name="BackButton"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>

    </Grid>
</Page>
```

Wenn Ihre App eine obere [CommandBar](../controls-and-patterns/app-bars.md) hat, wird das 44 Pixel hohe Button-Steuerelement nicht sehr gut neben 48 Pixel hohe AppBarButtons passen. Um jedoch Inkonsistenzen zu vermeiden, richten Sie den oberen Teil der Schaltfläche innerhalb der 48-Pixel-Grenzen aus.

![Zurück-Schaltfläche in der oberen Befehlsleiste](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button x:Name="BackButton"
                        Style="{StaticResource NavigationBackButtonNormalStyle}"
                        IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                        ToolTipService.ToolTip="Back" 
                        VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

Um UI-Elemente zu minimieren, die sich in Ihrer App bewegen, zeigen Sie eine deaktivierte Zurück-Schaltfläche an, wenn sich nichts im Backstack befindet (`IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}"`). Wenn Sie jedoch davon ausgehen, dass in Ihrer App nie ein Backstack auftritt, muss die Zurück-Schaltfläche überhaupt nicht angezeigt werden.

![Zustände der Zurück-Schaltfläche](images/back-nav/BackDisabled.png)

## <a name="optimize-for-different-devices-and-inputs"></a>Optimierung für verschiedene Geräte und Eingaben

Diese Anleitung für den Entwurf der Rückwärtsnavigation ist für alle Geräte anwendbar, aber Ihre Benutzer können davon profitieren, wenn Sie sie für verschiedene Formfaktoren und Eingabemethoden optimieren.

So optimieren Sie die Benutzeroberfläche

- **Desktop/Hub**: Stellen Sie den In-App-Button in der linken oberen Ecke der Benutzeroberfläche Ihrer App dar.
- **[Tablet-Modus](https://support.microsoft.com/windows/use-your-pc-like-a-tablet-4fbfcca5-f058-814a-4f80-a12e703d7c34)** : Eine Hardware- oder Software-Zurück-Schaltfläche kann auf Tablets vorhanden sein, aber wir empfehlen, eine In-App-Zurück-Schaltfläche zu entwerfen, um die Übersichtlichkeit zu erhöhen.
- **Xbox/TV**: Nutzen Sie keine Zurück-Schaltfläche. Sie macht die Benutzeroberfläche unnötigerweise unübersichtlich. Verlassen Sie sich stattdessen auf die Gamepad-Taste B, um rückwärts zu navigieren.

Wenn Ihre App auf Xbox läuft, [erstellen Sie einen benutzerdefinierten visuellen Trigger für die Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox), um die Sichtbarkeit der Schaltfläche umzuschalten. Wenn Sie das Steuerelement [NavigationView](../controls-and-patterns/navigationview.md) verwenden, schaltet es die Sichtbarkeit der Zurück-Schaltfläche automatisch um, wenn Ihre App auf der Xbox ausgeführt wird.

Es wird empfohlen, die folgenden Ereignisse (zusätzlich zum Klick auf die Schaltfläche „Zurück“) zu verarbeiten, um die häufigsten Eingaben für die Rückwärtsnavigation zu unterstützen.

| Ereignis | Eingabe |
| --- | --- |
| [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) | ALT+NACH-LINKS,<br/>VirtualKey.GoBack |
| [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) | B-TASTE auf dem Gamepad,<br/>Tablet-Modus Zurück-Schaltfläche,<br/>Hardware-Zurück-Taste |
| [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) | VirtualKey.XButton1<br/>(z. B. die Zurück-Schaltfläche auf einigen Mäusen.) |

## <a name="code-examples"></a>Codebeispiele

In diesem Abschnitt wird gezeigt, wie die Rückwärtsnavigation mit verschiedenen Eingaben verarbeitet wird.

### <a name="back-button-and-back-navigation"></a>Zurück-Schaltfläche und Rückwärtsnavigation

Sie müssen mindestens das Ereignis `Click` für die Zurück-Schaltfläche verarbeiten und den Code zur Durchführung der Rückwärtsnavigation bereitstellen. Sie sollten auch die Zurück-Taste deaktivieren, wenn der Backstack leer ist.

Dieser Beispielcode verdeutlicht, wie Sie das Rückwärtsnavigationsverhalten mit einer Zurück-Schaltfläche implementieren. Der Code reagiert auf das „Button [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click)“-Ereignis, um zu navigieren. Die Zurück-Schaltfläche wird in der Methode [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) aktiviert oder deaktiviert, die beim Navigieren zu einer neuen Seite aufgerufen wird.

Hier wird der Code für `MainPage` gezeigt, aber Sie fügen diesen Code zu jeder Seite hinzu, die die Rückwärtsnavigation unterstützt. Um eine Duplizierung zu vermeiden, können Sie den auf die Navigation bezogenen Code in der `App`-Klasse in der `App.xaml`-Code-Behind-Seite unterbringen.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
        <Button x:Name="BackButton" Click="BackButton_Click"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>
...
<Page/>
```

CodeBehind:

```csharp
// MainPage.xaml.cs
private void BackButton_Click(object sender, RoutedEventArgs e)
{
    ((App)Application.Current).TryGoBack();
}

// App.xaml.cs
//
// Add this method to the App class.
public bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// MainPage.h
namespace winrt::AppName::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();
 
        void MainPage::BackButton_Click(IInspectable const&, RoutedEventArgs const&)
        {
            m_navigationHelper->TryGoBack();
        }
    };
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
};
```

### <a name="support-access-keys"></a>Unterstützung von Zugriffsschlüsseln

Die Tastaturunterstützung ist ein wesentlicher Bestandteil, um sicherzustellen, dass Ihre Anwendungen optimal für Benutzer mit unterschiedlichen Kenntnissen, Fähigkeiten und Erwartungen funktionieren. Wir empfehlen, dass Sie Zugriffstasten sowohl für die Vorwärts- als auch für die Rückwärtsnavigation unterstützen, da Benutzer, die sich auf diese verlassen, beides erwarten. Weitere Informationen finden Sie unter [Tastaturinteraktionen](..\input\keyboard-interactions.md) und [Tastaturzugriffstaste ](..\input\keyboard-accelerators.md).

Die üblichen Zugriffstasten für die Vorwärts- und Rückwärtsnavigation sind ALT+NACH-RECHTS (vorwärts) und ALT+NACH-LINKS (rückwärts). Um diese Tasten für die Navigation zu unterstützen, verarbeiten Sie das Ereignis [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated). Sie verarbeiten ein Ereignis, das sich direkt im Fenster befindet (im Gegensatz zu einem Element auf der Seite), so, dass die App auf die Zugriffstasten reagiert, unabhängig davon, welches Element den Fokus hält.

Fügen Sie den Code zur Klasse `App` hinzu, um Zugriffstasten und Vorwärtsnavigation zu unterstützen, wie hier gezeigt. (Dies setzt voraus, dass der vorherige Code zur Unterstützung der Zurück-Schaltfläche bereits hinzugefügt wurde.) Sie finden den gesamten `App`-Code am Ende des Abschnitts mit den Codebeispielen.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // ...

    }
}

// ...

// Add this code after the TryGoBack method added previously.
// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...
    // Add this code after the TryGoBack method added previously.

private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
 
 
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }
};
```

### <a name="handle-system-back-requests"></a>Verarbeiten von Zurück-Anforderungen des Systems

Windows-Geräte bieten verschiedene Möglichkeiten, wie das System eine Anforderung zur Rückwärtsnavigation an Ihre App übergeben kann. Einige gängige Möglichkeiten sind die B-Taste auf einem Gamepad, die Tastenkombination WINDOWS-TASTE+RÜCKTASTE oder die Systemtaste „Zurück“ im Tablet-Modus; die genauen verfügbaren Optionen hängen vom Gerät ab.

Sie können vom System bereitgestellte Zurück-Anforderungen über Hardware- und Softwaresystem-Zurück-Tasten unterstützen, indem Sie einen Listener für das Ereignis [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) registrieren.

Hier ist der Code, der der Klasse `App` hinzugefügt wurde, um vom System bereitgestellte Zurück-Anforderungen zu unterstützen. (Dies setzt voraus, dass der vorherige Code zur Unterstützung der Zurück-Schaltfläche bereits hinzugefügt wurde.) Sie finden den gesamten `App`-Code am Ende des Abschnitts mit den Codebeispielen.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // ...

    }
}

// ...
// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }
};
```

#### <a name="system-back-behavior-for-backward-compatibility"></a>Zurück-Verhalten des Systems für Abwärtskompatibilität

Bisher verwendeten UWP-Apps [SystemNavigationManager.AppViewBackButtonVisibility](/uwp/api/windows.ui.core.systemnavigationmanager.appviewbackbuttonvisibility), um eine Zurück-Schaltfläche des Systems für die Rückwärtsnavigation ein- oder auszublenden. (Diese Schaltfläche löst eine [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested)-Ereignis aus.) Die API wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt, es wird jedoch davon abgeraten, die von `AppViewBackButtonVisibility` bereitgestellte Zurück-Schaltfläche zu verwenden. Stattdessen sollten Sie eine eigene In-App-Zurück-Schaltfläche bereitstellen, wie in diesem Artikel beschrieben.

Wenn Sie [AppViewBackButtonVisibility](/uwp/api/windows.ui.core.appviewbackbuttonvisibility) weiterhin verwendet, rendert die System-UI die Zurück-Schaltfläche des Systems in der Titelleiste. (Darstellung und Benutzerinteraktionen der Zurück-Schaltfläche bleiben gegenüber früheren Builds unverändert.)

![Zurück-Schaltfläche in der Titelleiste](images/nav-back-pc.png)

### <a name="handle-mouse-navigation-buttons"></a>Verarbeiten von Mausnavigationstasten

Einige Mäuse bieten Hardwarenavigationstasten für die Vorwärts- und Rückwärtsnavigation. Sie können diese Maustasten unterstützen, indem Sie das Ereignis [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) verarbeiten und prüfen, ob [IsXButton1Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton1pressed) (zurück) oder [IsXButton2Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton2pressed) (vorwärts) vorhanden ist.

Hier ist der Code, der der Klasse `App` hinzugefügt wurde, um die Navigation mit der Maustaste zu unterstützen. (Dies setzt voraus, dass der vorherige Code zur Unterstützung der Zurück-Schaltfläche bereits hinzugefügt wurde.) Sie finden den gesamten `App`-Code am Ende des Abschnitts mit den Codebeispielen.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

### <a name="all-code-added-to-app-class"></a>Der gesamte der App-Klasse hinzugefügte Code
 
```csharp
// App.xaml.cs
//
// (Add event handlers in OnLaunched override.)
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// (Add these methods to the App class.)
public bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}

// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}

// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}


```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
  
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Richtlinien für das benutzerdefinierte Verhalten der Rückwärtsnavigation

Wenn Sie Ihre eigene Zurück-Stapelnavigation bereitstellen möchten, sollte die Benutzeroberfläche mit anderen Apps konsistent sein. Wir empfehlen, die folgenden Muster für Navigationsaktionen zu verwenden:

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Navigationsaktion</th>
<th align="left">Zum Navigationsverlauf hinzufügen?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Seite zu Seite, verschiedene Peer-Gruppen</strong></td>
<td style="vertical-align:top;"><strong>Ja</strong>
<p>In der folgenden Abbildung navigiert der Benutzer von Ebene 1 der App zu Ebene 2. Dabei werden Peer-Gruppen durchlaufen, sodass die Navigation dem Navigationsverlauf hinzugefügt wird.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two and the back to group one." /></p>
<p>In der nächsten Abbildung navigiert der Benutzer zwischen zwei Peer-Gruppen derselben Ebene. Er durchläuft erneut Peer-Gruppen, sodass die Navigation dem Navigationsverlauf hinzugefügt wird.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two then on to group three and back to group two." /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Seite zu Seite, gleiche Peer-Gruppe, kein Bildschirmnavigationselement</strong>
<p>Der Benutzer navigiert von einer Seite zu einer anderen mit derselben Peer-Gruppe. Es ist kein Bildschirmnavigationselement (wie <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>) vorhanden, das die direkte Navigation zu beiden Seiten ermöglicht.</p></td>
<td style="vertical-align:top;"><strong>Ja</strong>
<p>In der folgenden Abbildung navigiert der Benutzer zwischen zwei Seiten in derselben Peer-Gruppe, und die Navigation sollte dem Navigationsverlauf hinzugefügt werden.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Seite zu Seite, gleiche Peer-Gruppe, mit einem Bildschirmnavigationselement</strong>
<p>Der Benutzer navigiert von einer Seite zu einer anderen in derselben Peer-Gruppe. Beide Seiten werden im gleichen Navigationselement angezeigt, z. B. in <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Das kommt darauf an</strong>
<p>Ja, fügen Sie es dem Navigationsverlauf hinzu, jedoch mit zwei Ausnahmen. Wenn Sie davon ausgehen, dass Benutzer Ihrer App häufig zwischen Seiten in der Peer-Gruppe wechseln, oder wenn Sie die Navigationshierarchie beibehalten möchten, dann fügen Sie es dem Navigationsverlauf nicht hinzu. Wenn der Benutzer in diesem Fall die Zurück-Schaltfläche drückt, wird wieder die letzte Seite aufgerufen, die geöffnet war, bevor der Benutzer zur aktuellen Peer-Gruppe navigierte. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Anzeigen einer vorübergehenden Benutzeroberfläche</strong>
<p>Die App zeigt ein Popupfenster oder ein untergeordnetes Fenster an, z. B. ein Dialogfeld, einen Begrüßungsbildschirm oder eine Bildschirmtastatur. Anderenfalls wechselt die App in einen speziellen Modus, z. B. den Mehrfachauswahlmodus.</p></td>
<td style="vertical-align:top;"><strong>Nein</strong>
<p>Wenn der Benutzer die Zurück-Schaltfläche drückt, sollte die vorübergehende Benutzeroberfläche geschlossen werden (durch Ausblenden der Bildschirmtastatur, Abbrechen des Dialogfelds usw.) und zur Seite zurückgekehrt werden, die die vorübergehende Benutzeroberfläche aufgerufen hat.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Aufzählen von Elementen</strong>
<p>Die App zeigt die Inhalte für ein Bildschirmelement an, z. B. die Details für das ausgewählte Element in der Master/Details-Liste.</p></td>
<td style="vertical-align:top;"><strong>Nein</strong>
<p>Das Aufzählen von Elementen ist mit der Navigation innerhalb einer Peer-Gruppe vergleichbar. Wenn der Benutzer die Zurück-Schaltfläche drückt, sollte zu der Seite navigiert werden, die vor der aktuellen Seite angezeigt wurde, die die Elementenaufzählung enthält.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Fortsetzen

Wenn der Benutzer zu einer anderen App wechselt und zu Ihrer App zurückkehrt, wird empfohlen, zur letzten Seite im Navigationsverlauf zurückzukehren.

## <a name="related-articles"></a>Verwandte Artikel

- [Navigationsgrundlagen](navigation-basics.md)
