---
title: Mithilfe der visuellen Ebene mit Win32
description: Verwenden Sie der visuellen Ebene, um die Benutzeroberfläche der Win32-desktop-app zu verbessern.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, rendering, composition, win32
ms.localizationpriority: medium
ms.openlocfilehash: 0cf4e45ac6214e714e2c73a73006654584f50799
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985240"
---
# <a name="using-the-visual-layer-with-win32"></a>Mithilfe der visuellen Ebene mit Win32

Sie können Windows-Runtime-Kompositions-APIs verwenden (so genannte der [visueller Ebene](/windows/uwp/composition/visual-layer)) in Ihre Win32-apps auf moderne Funktionen für Windows 10-Benutzern das einfache erstellen.

Der vollständige Code für dieses Tutorial ist auf GitHub verfügbar: [Win32-HelloComposition Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Universelle Windows-Anwendungen, die genaue Kontrolle über ihre UI-Zusammenstellung benötigen, haben Zugriff auf die [Windows.UI.Composition](/uwp/api/windows.ui.composition) Namespace, auszuüben eine differenziertere Kontrolle darüber, wie ihre Benutzeroberfläche erstellt und gerendert wird. Diese Kompositions-API ist jedoch nicht auf die UWP-apps beschränkt. Win32-desktopanwendungen können die moderne Komposition-Systeme in UWP und Windows 10 nutzen.

## <a name="prerequisites"></a>Vorraussetzungen

Die UWP hosting-API verfügt über diese erforderlichen Komponenten.

- Es wird davon ausgegangen, dass Sie eine gewisse Vertrautheit mit der Entwicklung von Apps mithilfe von Win32- und UWP verfügen. Weitere Informationen:
  - [Erste Schritte mit Win32 undC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/)
  - [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10, Version 1803 oder höher
- Windows 10 SDK 17134 oder höher

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Wie Sie mit der Kompositions-APIs aus einer Win32-desktop-Anwendung

In diesem Tutorial erstellen Sie eine einfache Win32 C++ app, und fügen Sie UWP-Komposition Elemente hinzu. Der Schwerpunkt liegt auf ordnungsgemäß Konfigurieren des Projekts, erstellen die interop-Code und etwa mithilfe von Windows-Kompositions-APIs. Die fertige app sieht wie folgt aus.

![Die ausgeführte app-Benutzeroberfläche](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Erstellen Sie eine C++ Win32-Projekt in Visual Studio

Der erste Schritt ist das Win32-app-Projekt in Visual Studio zu erstellen.

Erstellen Sie ein neues Projekt für die Win32-Anwendung in C++ mit dem Namen _HelloComposition_:

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **neu** > **Projekt**.

    Die **neues Projekt** Dialogfeld wird geöffnet.
1. Unter den **installiert** (Kategorie), erweitern Sie die **Visual C++**  Knoten, und wählen Sie dann **Windows Desktop**.
1. Wählen Sie die **Windows-Desktopanwendung** Vorlage.
1. Geben Sie den Namen _HelloComposition_, klicken Sie dann auf **OK**.

    Visual Studio erstellt das Projekt und öffnet den-Editor für die Haupt-app-Datei.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts zur Verwendung von Windows-Runtime-APIs

Um Windows-Runtime (WinRT) APIs in der Win32-app verwenden zu können, verwenden wir C++"/ WinRT". Sie müssen zum Konfigurieren des Visual Studio-Projekts hinzufügen C++"/ WinRT" unterstützt.

(Weitere Informationen finden Sie unter [erste Schritte mit C++"/ WinRT" - ändern Sie ein Windows-Desktop-Anwendungsprojekt hinzufügen C++"/ WinRT" Unterstützung](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Von der **Projekt** Menü öffnen Sie die Projekteigenschaften (_HelloComposition Eigenschaften_) und stellen Sie sicher, die folgenden Einstellungen werden auf die angegebenen Werte festgelegt:

    - Für **Konfiguration**Option _alle Konfigurationen_. Für **Plattform**Option _alle Plattformen_.
    - **Konfigurationseigenschaften** > **allgemeine** > **Windows SDK-Version** = _10.0.17763.0_ oder höher

    ![SDK-Zeichensatz-version](images/visual-layer-interop/sdk-version.png)

    - **C /C++** > **Sprache**  >   **C++ Standard der Sprache** = _ISO C++ 17-Standard (/ Stf:c ++ 17)_

    ![Set-Sprachstandard](images/visual-layer-interop/language-standard.png)

    - **Linkertoolfehler** > **Eingabe** > **zusätzliche Abhängigkeiten** muss enthalten "_windowsapp.lib_". Wenn sie nicht in der Liste enthalten ist, fügen Sie ihn aus.

    ![Linker-Abhängigkeit hinzufügen](images/visual-layer-interop/linker-dependencies.png)

1. Aktualisieren des vorkompilierten Headers

    - Benennen Sie `stdafx.h` und `stdafx.cpp` zu `pch.h` und `pch.cpp`bzw.
    - Festlegen von Projekteigenschaften **C/C++-** > **vorkompilierte Header** > **vorkompilierte Headerdatei** zu *"PCH.h"*.
    - Suchen und Ersetzen `#include "stdafx.h"` mit `#include "pch.h"` in allen Dateien.

        (**Bearbeiten** > **suchen und Ersetzen** > **in Dateien suchen**)

        ![Suchen und Ersetzen Sie "stdafx.h"](images/visual-layer-interop/replace-stdafx.png)

    - In `pch.h`, umfassen `winrt/base.h` und `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Es ist eine gute Idee, erstellen Sie das Projekt an diesem Punkt, um sicherzustellen, dass keine Fehler vorliegen, bevor Sie fortfahren.

## <a name="create-a-class-to-host-composition-elements"></a>Erstellen Sie eine Klasse zum Hosten von Komposition-Elementen

Auf Content-Host können Sie auch mit der visuellen Ebene erstellen, erstellen Sie eine Klasse (_CompositionHost_) zu verwaltende Interop Komposition Elemente erstellen. Dies ist in dem sich der Großteil der Konfiguration für das Hosten der Kompositions-APIs, einschließlich sollen:

- Abrufen einer [Compositor](/uwp/api/windows.ui.composition.compositor), die erstellt und verwaltet Sie Objekte in der [Windows.UI.Composition](/uwp/api/windows.ui.composition) Namespace.
- Erstellen einer [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) zur Verwaltung von Aufgaben für die WinRT-APIs.
- Erstellen einer [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) und Kompositionscontainer, um die kompositionsobjekte anzuzeigen.

Wir legen Sie dieses in ein Singleton, um zu vermeiden, Threadingprobleme-Klasse. Beispielsweise können Sie nur erstellen, ein Dispatcher-Warteschlange pro Thread, damit instanziieren eine zweite Instanz von CompositionHost auf dem gleichen Thread auf einen Fehler verursachen würden.

> [!TIP]
> Wenn Sie möchten, überprüfen Sie den vollständigen Code am Ende des Tutorials stellen Sie sicher, dass der gesamte Code in den richtigen stellen ist, wie Sie das Lernprogramm durchzuarbeiten.

1. Fügen Sie Ihrem Projekt eine neue Klassendatei hinzu.
    - In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
    - Wählen Sie im Kontextmenü des **hinzufügen** > **Klasse...** .
    - In der **Klasse hinzufügen** Dialogfeld, nennen Sie die Klasse _CompositionHost.cs_, klicken Sie dann auf **hinzufügen**.

1. Header einschließen und _Using-Direktiven_ für Komposition Interop erforderlich sind.
    - Fügen Sie in CompositionHost.h, diese _enthält_ am Anfang der Datei.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - Fügen Sie in CompositionHost.cpp, diese _Using-Direktiven_ am Anfang der Datei nach jedem _enthält_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Bearbeiten Sie die Klasse, um das Singletonmuster verwenden.
    - In CompositionHost.h dass der Konstruktor privat.
    - Deklarieren Sie eine öffentliche statische _GetInstance_ Methode.

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

    - In CompositionHost.cpp, fügen Sie die Definition der _GetInstance_ Methode.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. Deklarieren Sie in CompositionHost.h Variablen für die der Compositor DispatcherQueueController und DesktopWindowTarget privaten Member.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Fügen Sie eine öffentliche Methode zum Initialisieren der Kompositions-interop-Objekte.
    > [!NOTE]
    > In _initialisieren_, rufen Sie die _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_, und _CreateCompositionRoot_ Methoden. Sie erstellen diese Methoden in den nächsten Schritten.

    - In CompositionHost.h, deklarieren Sie eine öffentliche Methode mit dem Namen _initialisieren_ , das HWND als ein Argument akzeptiert.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - In CompositionHost.cpp, fügen Sie die Definition der _initialisieren_ Methode.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Erstellen Sie eine Dispatcher-Warteschlange, auf den Thread, der Windows-Komposition verwendet wird.

    Ein Compositor muss in einem Thread erstellt werden, die eine Dispatcher-Warteschlange verfügt, damit diese Methode zuerst während der Initialisierung aufgerufen wird.

    - In CompositionHost.h, deklarieren Sie eine private Methode mit dem Namen _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - In CompositionHost.cpp, fügen Sie die Definition der _EnsureDispatcherQueue_ Methode.

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

1. Registrieren Sie Ihrer app-Fenster als ein kompositionsziel an.
    - In CompositionHost.h, deklarieren Sie eine private Methode mit dem Namen _CreateDesktopWindowTarget_ , das HWND als ein Argument akzeptiert.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - In CompositionHost.cpp, fügen Sie die Definition der _CreateDesktopWindowTarget_ Methode.

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

1. Erstellen Sie einen visuellen Root-Container zum Speichern von visuellen Objekten.
    - In CompositionHost.h, deklarieren Sie eine private Methode mit dem Namen _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - In CompositionHost.cpp, fügen Sie die Definition der _CreateCompositionRoot_ Methode.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 24, 24, 0 });
        m_target.Root(root);
    }
    ```

Erstellen Sie jetzt das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

Diese Methoden richten Sie die Komponenten, die für die Interoperabilität zwischen der visuellen Ebene UWP und Win32-APIs benötigt. Jetzt können Sie Inhalte zu Ihrer app hinzufügen.

### <a name="add-composition-elements"></a>Komposition Elemente hinzufügen

Mit der Infrastruktur vorhanden ist können Sie nun den Inhalt der Zusammenstellung generieren, die, den Sie anzeigen möchten.

In diesem Beispiel fügen Sie Code, der ein einfaches Quadrat erstellt [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Fügen Sie eine Kompositionselement.
    - In CompositionHost.h, deklarieren Sie eine öffentliche Methode mit dem Namen _AddElement_ akzeptiert 3 **"float"** Werte als Argumente.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - In CompositionHost.cpp, fügen Sie die Definition der _AddElement_ Methode.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
            visual.Size({ size, size });
            visual.Offset({ x, y, 0.0f, });

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Erstellen und die Fenster anzeigen

Jetzt können Sie den Inhalt der UWP-Komposition der Win32-Benutzeroberfläche hinzufügen. Verwenden von Ihrer Anwendungsverzeichnis _InitInstance_ Methode zum Initialisieren der Windows-Komposition und den Inhalt generieren.

1. HelloComposition.cpp, einschließt _CompositionHost.h_ am Anfang der Datei.

    ```cppwinrt
    #include "CompositionHost.h"
    ```

1. In der `InitInstance` -Methode, fügen Sie Code zum Initialisieren und verwenden Sie die CompositionHost-Klasse hinzu.

    Fügen Sie folgenden Code ein, nach der Erstellung des HWND und vor dem Aufruf von `ShowWindow`.

    ```cppwinrt
    CompositionHost* compHost = CompositionHost::GetInstance();
    compHost->Initialize(hWnd);
    compHost->AddElement(150, 10, 10);
    ```

Sie können jetzt erstellen und Ausführen der app. Wenn Sie möchten, überprüfen Sie den vollständigen Code am Ende des Tutorials stellen Sie sicher, dass der gesamte Code in den richtigen Stellen stattfinden.

Wenn Sie die app ausführen, sollte ein blaues Quadrat aus hinzugefügt, die an der Benutzeroberfläche angezeigt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Win32-HelloComposition-Beispiel (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Erste Schritte mit Win32 undC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/) (UWP)
- [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Hier ist der vollständige Code für die CompositionHost-Klasse und die InitInstance-Methode.

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
    root.Offset({ 24, 24, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
        visual.Size({ size, size });
        visual.Offset({ x, y, 0.0f, });

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp---initinstance"></a>HelloComposition.cpp - InitInstance

```cppwinrt
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   CompositionHost* compHost = CompositionHost::GetInstance();
   compHost->Initialize(hWnd);
   compHost->AddElement(150, 10, 10);

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}
```