---
Description: ''
title: Ereignisse für Sprachausgabe und Hardware Schaltfläche
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: Windows 10, UWP, Barrierefreiheit, Sprachausgabe, Bildschirmleser
ms.localizationpriority: medium
ms.openlocfilehash: 41c6ed531f21a922b407ff3ba5aae93afb8d917e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234933"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>Schaltflächen für Bildschirm Sprachausgaben und Hardwaresystem

Bildschirme (z. b. die Sprachausgabe) müssen in der Lage [sein, Hardware](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)System-Schaltflächen Ereignisse zu erkennen und zu behandeln und ihren Zustand Benutzern mitzuteilen. In einigen Fällen muss die Bildschirm Sprachausgabe diese Hardware-Schaltflächen Ereignisse möglicherweise exklusiv behandeln und Sie nicht an andere Handler weitergeben.

Ab Windows 10, Version 2004, können UWP-Anwendungen die Ereignisse der **FN** -Hardwaresystem Schaltfläche auf die gleiche Weise wie andere Hardware Schaltflächen lauschen und behandeln. Zuvor war diese Schaltfläche des Systems nur als Modifizierer für die Art und Weise aufgetreten, wie andere Hardware Schaltflächen Ihre Ereignisse und den Zustand

> [!NOTE]
> Die Unterstützung für FN-Schaltflächen ist OEM-spezifisch und kann Features wie die Möglichkeit zum Umschalten/Sperren von ein-oder ausschalten (vs. eine Press-und-halten-Tastenkombination) sowie eine entsprechende Sperr Indikator Beleuchtung enthalten, die für blinde oder Sehgeschädigte Benutzer möglicherweise nicht hilfreich ist.

Ereignisse der FN-Schaltfläche werden über eine neue [systembuttoneventcontroller-Klasse](/uwp/api/windows.ui.input.systembuttoneventcontroller) im [Windows. UI. Input](/uwp/api/windows.ui.input) -Namespace verfügbar gemacht. Das systembuttoneventcontroller-Objekt unterstützt die folgenden Ereignisse:

- [Systemfunctionbuttonpressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [Systemfunctionbuttonreleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [Systemfunctionlockchanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [Systemfunctionlockder orchanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> Der systembuttoneventcontroller kann diese Ereignisse nicht empfangen, wenn Sie bereits von einem Handler höherer Priorität behandelt wurden.

## <a name="examples"></a>Beispiele

In den folgenden Beispielen wird veranschaulicht, wie ein [systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) basierend auf einer DispatcherQueue erstellt und die vier Ereignisse behandelt werden, die von diesem-Objekt unterstützt werden.

Es kommt häufig vor, dass mehr als eines der unterstützten Ereignisse ausgelöst wird, wenn die FN-Schaltfläche gedrückt wird. Wenn Sie z. b. die FN-Schaltfläche auf einer Oberflächen Tastatur drücken, werden systemfunctionbuttonpressed, systemfunctionlockchanged und systemfunctionlockder orchanged gleichzeitig ausgelöst.

1. In diesem ersten Code Ausschnitt fügen wir einfach die erforderlichen Namespaces ein und geben einige globale Objekte an, einschließlich der [DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) -und [dispatcherqueuecontroller](/uwp/api/windows.system.dispatcherqueuecontroller) -Objekte zum Verwalten des [systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) -Threads.

   Anschließend geben wir die [Ereignis Token](/uwp/cpp-ref-for-winrt/event-token) an, die beim Registrieren der [systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) -Ereignishandlerdelegaten zurückgegeben werden.

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. Außerdem geben wir ein Ereignis Token für das [systemfunctionlockboundorchanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) -Ereignis zusammen mit einem booleschen Wert an, um anzugeben, ob sich die Anwendung im "Lernmodus" befindet (wobei der Benutzer einfach versucht, die Tastatur zu durchsuchen, ohne Funktionen auszuführen).

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. Dieser dritte Ausschnitt enthält die entsprechenden Ereignishandlerdelegaten für jedes Ereignis, das vom [systembuttoneventcontroller](/uwp/api/windows.ui.input.systembuttoneventcontroller) -Objekt unterstützt wird.

   Jeder Ereignishandler kündigt das Ereignis an, das aufgetreten ist. Außerdem steuert der functionlockndes orchanged-Handler auch, ob sich die APP im "Learning"-Modus befindet ( `_isLearningMode` = true). Dadurch wird verhindert, dass das Ereignis an andere Handler gesendet wird, und der Benutzer kann die Tastatur Features durchsuchen, ohne die Aktion tatsächlich auszuführen.

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>Siehe auch

[Systembuttoneventcontroller-Klasse](/uwp/api/windows.ui.input.systembuttoneventcontroller)
