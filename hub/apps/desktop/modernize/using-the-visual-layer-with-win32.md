---
title: Verwenden der visuellen Ebene mit Win32
description: Erfahren Sie, wie die Composition-APIs von Windows-Runtime der universellen Windows-Plattform (UWP), auch als visuelle Ebene bezeichnet, verwenden werden, um die Benutzeroberfläche einer Win32 C++-App zu verbessern.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, Rendering, Komposition, Win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 98811d890aa496e05c38009dedea6978386d5e4a
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043472"
---
# <a name="using-the-visual-layer-with-win32"></a>Verwenden der visuellen Ebene mit Win32

Du kannst in deinen Win32-Apps die Composition-APIs von Windows-Runtime (die auch als [visuelle Ebene](/windows/uwp/composition/visual-layer) bezeichnet werden) verwenden, um moderne Umgebungen für Windows 10-Benutzer zu erstellen.

Den gesamten Code für dieses Tutorial findest du auf GitHub: [Win32-Beispiel HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Universelle Windows-Anwendungen, die eine präzise Steuerung der Komposition ihrer Benutzeroberfläche benötigen, haben Zugriff auf den Namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition), der eine detaillierte Steuerung der Komposition und Darstellung der Benutzeroberfläche ermöglicht. Diese Composition-API ist jedoch nicht auf UWP-Apps beschränkt. Win32-Desktopanwendungen können die modernen Kompositionssysteme in UWP und Windows 10 nutzen.

## <a name="prerequisites"></a>Voraussetzungen

Für die UWP hostende API gelten diese Voraussetzungen.

- Es wird davon ausgegangen, dass du mit der App-Entwicklung mit Win32 und UWP vertraut bist. Weitere Informationen:
  - [Erste Schritte mit Win32 und C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/)
  - [Verbessern deiner Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 ab Version 1803
- Windows 10 SDK ab 17134

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Verwenden von Composition-APIs in einer Win32-Desktopanwendung

In diesem Tutorial erstellst du eine einfache Benutzeroberfläche für eine Win32 C++-App und fügst ihr Composition-Elemente von UWP hinzu. Der Schwerpunkt liegt auf der ordnungsgemäßen Konfiguration des Projekts, der Erstellung des Interopcodes und dem Zeichnen von etwas Einfachem mithilfe von Composition-APIs von Windows. Die fertige App sieht wie folgt aus.

![Benutzeroberfläche der laufenden App](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Erstellen eines C++ Win32-Projekts in Visual Studio

Der erste Schritt ist das Erstellen des Win32-App-Projekts in Visual Studio.

So erstellst du ein Win32-Anwendungsprojekt in C++ namens _HelloComposition_

1. Öffne Visual Studio, und wähle **Datei** > **Neu** > **Projekt** aus.

    Das Dialogfeld **Neues Projekt** wird geöffnet.
1. Erweitere unter der Kategorie **Installiert** den Knoten **Visual C++** , und wähle dann **Windows-Desktop** aus.
1. Wähle die Vorlage **Windows-Desktopanwendung** aus.
1. Gib den Namen _HelloComposition_ ein, und klicke dann auf **OK**.

    Visual Studio erstellt das Projekt und öffnet den Editor für die Hauptdatei der App.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts für die Verwendung der Windows-Runtime-APIs

Zum Verwenden von Windows-Runtime-APIs (WinRT) in deiner Win32-APP nutzen wir C++/WinRT. Du musst dein Visual Studio-Projekt so konfigurieren, dass es C++/WinRT-Unterstützung bietet.

(Einzelheiten findest du unter [Erste Schritte mit C++/WinRT: Ändern eines Windows Desktop-Anwendungsprojekts, um C++/WinRT-Unterstützung hinzuzufügen](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Öffne im Menü **Projekt** die Projekteigenschaften (_HelloComposition-Eigenschaften_) und achte darauf, dass die folgenden Einstellungen auf die angegebenen Werte festgelegt sind:

    - Wähle für **Konfiguration** die Option _Alle Konfigurationen_ aus. Wähle für **Plattform** die Option _Alle Plattformen_ aus.
    - **Konfigurationseigenschaften** > **Allgemein** > **Windows SDK ab Version** = _10.0.17763.0_

    ![Festlegen der SDK-Version](images/visual-layer-interop/sdk-version.png)

    - **C/C++**  > **Sprache** > **C++-Sprachstandard** = _ISO C++ 17 Standard (/stf:c++17)_

    ![Festlegen des Sprachstandards](images/visual-layer-interop/language-standard.png)

    - **Linker** > **Eingabe** > **Zusätzliche Abhängigkeiten** muss _windowsapp.lib_ enthalten. Falls nicht in der Liste enthalten, füge sie hinzu.

    ![Hinzufügen von Linkerabhängigkeit](images/visual-layer-interop/linker-dependencies.png)

1. Aktualisieren des vorkompilierten Headers

    - Benenne `stdafx.h` und `stdafx.cpp` in `pch.h` und `pch.cpp` um.
    - Lege die Projekteigenschaft **C/C++**  > **Vorkompilierte Header** > **Vorkompilierter Headerdatei** auf *pch.h* fest.
    - Suche und ersetze `#include "stdafx.h"` durch `#include "pch.h"` in allen Dateien.

        (**Bearbeiten** > **Suchen und ersetzen** > **In Dateien suchen**)

        ![Suchen und Ersetzen von „stdafx.h“](images/visual-layer-interop/replace-stdafx.png)

    - Schließe `winrt/base.h` und `unknwn.h` in `pch.h` ein.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Es empfiehlt sich, das Projekt zu diesem Zeitpunkt zu kompilieren, um sicherzustellen, dass keine Fehler vorliegen, ehe fortgefahren wird.

## <a name="create-a-class-to-host-composition-elements"></a>Erstellen einer Klasse zum Hosten von Kompositionselementen

Um Inhalte zu hosten, die du mit der visuellen Ebene erstellst, erstelle eine Klasse (_CompositionHost_) zum Verwalten von Interop-Elementen und Erstellen von Kompositionselementen. In dieser Klasse führst du den größten Teil der Konfiguration für das Hosting von Composition-APIs aus, wie z. B.:

- Abrufen eines [Compositor](/uwp/api/windows.ui.composition.compositor)-Elements, das Objekte im Namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition) erstellt und verwaltet.
- Erstellen von [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue), um Aufgaben für die WinRT-APIs zu verwalten.
- Erstellen eines [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget)-Elements und Composition-Containers zum Anzeigen der Kompositionsobjekte.

Wir machen diese Klasse zu einem Singleton, um Threadingprobleme zu vermeiden. Du kannst z. B. nur eine Verteilerwarteschlange pro Thread erstellen, weshalb die Instanziierung einer zweiten Instanz von CompositionHost im selben Thread zu einem Fehler führen würde.

> [!TIP]
> Überprüfe bei Bedarf am Ende des Tutorials den vollständigen Code, um sicherzustellen, dass sich der gesamte Code während des Durcharbeitens an den richtigen Stellen befindet.

1. Fügen Sie Ihrem Projekt eine neue Klassendatei hinzu.
    - Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
    - Wähle im Kontextmenü **Hinzufügen** > **Klasse...** aus.
    - Benenne die Klasse im Dialogfeld **Klasse hinzufügen** mit _CompositionHost.cs_, und klicke dann auf **Hinzufügen**.

1. Schließe Header und _using_-Anweisungen ein, die für die Interop der Komposition erforderlich sind.
    - Füge in „CompositionHost.h“ diese _include_-Anweisungen am Anfang der Datei hinzu.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - Füge in „CompositionHost.cpp“ diese _using_-Anweisungen am Anfang der Datei hinter beliebigen _include_-Anweisungen hinzu.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Bearbeite die Klasse so, dass das Singletonmuster verwendet wird.
    - Lege den Konstruktor in „CompositionHost.h“ als privat fest.
    - Deklariere eine öffentliche statische _GetInstance_-Methode.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _GetInstance_-Methode hinzu.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. Deklariere in „CompositionHost.h“ private Membervariablen für Compositor, DispatcherQueueController und DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Füge eine öffentliche Methode hinzu, um die Interop-Objekte der Komposition zu initialisieren.
    > [!NOTE]
    > Rufe in _Initialize_ die Methoden _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_ und _CreateCompositionRoot_ auf. Du erstellst diese Methoden in den nächsten Schritten.

    - Deklariere in „CompositionHost.h“ eine öffentliche Methode namens _Initialize_, die ein HWND als Argument verwendet.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _Initialize_-Methode hinzu.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Erstelle eine Verteilerwarteschlange für den Thread, die Windows Composition verwendet.

    Ein Compositor muss für einen Thread erstellt werden, der über eine Verteilerwarteschlange verfügt. Daher wird diese Methode bei der Initialisierung zuerst aufgerufen.

    - Deklariere in „CompositionHost.h“ eine private Methode mit dem Namen _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _EnsureDispatcherQueue_-Methode hinzu.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Registriere das Fenster deiner App als Kompositionsziel.
    - Deklariere in „CompositionHost.h“ eine private Methode namens _CreateDesktopWindowTarget_, die ein HWND als Argument verwendet.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _CreateDesktopWindowTarget_-Methode hinzu.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Erstelle einen visuellen Stammcontainer für visuelle Objekte.
    - Deklariere in „CompositionHost.h“ eine private Methode mit dem Namen _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _CreateCompositionRoot_-Methode hinzu.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Erstelle jetzt das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

Diese Methoden richten die Komponenten ein, die für die Interop zwischen der visuellen UWP-Schicht und den Win32-APIs benötigt werden. Nun kannst du deiner App Inhalte hinzufügen.

### <a name="add-composition-elements"></a>Hinzufügen von Composition-Elementen

Nachdem du die Infrastruktur eingerichtet hast, kannst du nun den Composition-Inhalt generieren, den du anzeigen möchtest.

Füge für dieses Beispiel Code hinzu, der ein zufällig koloriertes quadratisches [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) mit einer Animation erzeugt, die bewirkt, dass es nach einer kurzen Verzögerung fällt.

1. Füge ein Composition-Element hinzu.
    - Deklariere in „CompositionHost.h“ eine öffentliche Methode namens _AddElement_, die drei **float**-Werte als Argumente verwendet.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - Füge in „CompositionHost.cpp“ die Definition der _AddElement_-Methode hinzu.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Erstellen und Anzeigen des Fensters

Jetzt kannst du eine Schaltfläche und den Inhalt der UWP-Komposition deiner Win32-Benutzeroberfläche hinzufügen.

1. Füge in „HelloComposition.cpp“ am Anfang der Datei _CompositionHost.h_ ein. Definiere BTN_ADD, und rufe eine Instanz von CompositionHost ab.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. Ändere in der `InitInstance`-Methode die Größe des Fensters, das erstellt wird. (Ändere in dieser Zeile `CW_USEDEFAULT, 0` in `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. Füge in der WndProc-Funktion `case WM_CREATE` dem Schalterblock _message_ hinzu. In diesem Fall initialisierst du CompositionHost und erstellst die Schaltfläche.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. Behandle außerdem in der WndProc-Funktion den Schaltflächenklick so, dass ein Composition-Element zur Benutzeroberfläche hinzugefügt wird. 

    Füge innerhalb des WM_COMMAND-Blocks `case BTN_ADD` dem Schalterblock _wmId_ hinzu.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

Jetzt kannst du deine App kompilieren und ausführen. Überprüfe ggf. den vollständigen Code am Ende des Tutorials, um sicherzustellen, dass sich der gesamte Code an den richtigen Stellen befindet.

Wenn du die App ausführst und auf die Schaltfläche klickst, sollten die animierten Quadrate der Benutzeroberfläche hinzugefügt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Win32-Beispiel HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Erste Schritte mit Win32 und C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/) (UWP)
- [Verbessern Ihrer Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition-Namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Hier ist der vollständige Code der CompositionHost-Klasse und der InitInstance-Methode.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (partiell)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```