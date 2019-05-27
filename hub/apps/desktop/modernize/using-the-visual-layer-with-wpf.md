---
title: Mithilfe der visuellen Ebene mit WPF
description: Erfahren Sie, Verfahren für die Verwendung der visuellen Ebene-API in Kombination mit vorhandenen WPF-Inhalt zum Erstellen erweiterter Animation und Effekte.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215155"
---
# <a name="using-the-visual-layer-with-wpf"></a>Mithilfe der visuellen Ebene mit WPF

Sie können Windows-Runtime-Kompositions-APIs verwenden (so genannte der [visueller Ebene](/windows/uwp/composition/visual-layer)) in Ihren Windows Presentation Foundation (WPF)-apps auf moderne Funktionen für Windows 10-Benutzern das einfache erstellen.

Der vollständige Code für dieses Tutorial ist auf GitHub verfügbar: [Beispiel für WPF-HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Vorraussetzungen

Die UWP XAML hosting-API verfügt über diese erforderlichen Komponenten.

- Es wird davon ausgegangen, dass Sie eine gewisse Vertrautheit mit der Entwicklung von Apps mit WPF und UWP verfügen. Weitere Informationen:
  - [Erste Schritte (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/)
  - [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 oder höher
- Windows 10, Version 1803 oder höher
- Windows 10 SDK 17134 oder höher

## <a name="how-to-use-composition-apis-in-wpf"></a>Wie Sie mit der Kompositions-APIs in WPF

In diesem Tutorial erstellen eine einfache WPF-app-Benutzeroberfläche und animierten Komposition Elemente hinzufügen. Sowohl die Komposition der WPF-Komponenten sind einfach gehalten, aber der Interop-Code, der angezeigt wird, unabhängig von der Komplexität der Komponenten. Die fertige app sieht wie folgt aus.

![Die ausgeführte app-Benutzeroberfläche](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Erstellen Sie ein WPF-Projekt

Der erste Schritt ist das WPF-Anwendungsprojekt zu erstellen, das Definition einer Anwendung und die XAML-Seite für die Benutzeroberfläche enthält.

Erstellen Sie ein neues WPF-Anwendungsprojekt in Visual C# mit dem Namen _HelloComposition_:

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **neu** > **Projekt**.

    Die **neues Projekt** Dialogfeld wird geöffnet.
1. Unter den **installiert** (Kategorie), erweitern Sie die **Visual C#**  Knoten, und wählen Sie dann **Windows Desktop**.
1. Wählen Sie die **WPF-App ((.NET Framework)** Vorlage.
1. Geben Sie den Namen _HelloComposition_, wählen Sie Framework **.NET Framework 4.7.2**, klicken Sie dann auf **OK**.

    Visual Studio erstellt das Projekt und öffnet den Designer für das standardanwendungsfenster namens "MainWindow.xaml".

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts zur Verwendung von Windows-Runtime-APIs

Um Windows-Runtime (WinRT)-APIs in Ihrer WPF-Anwendung verwenden zu können, müssen Sie Visual Studio-Projekt für den Zugriff auf die Windows-Runtime zu konfigurieren. Darüber hinaus werden Vektoren umfassend von der Kompositions-APIs verwendet, daher Sie die Verwendung von Vektoren erforderlichen Verweise hinzufügen müssen.

NuGet-Pakete sind für die beiden dieser Anforderungen verfügbar. Installieren Sie die neuesten Versionen dieser Pakete, die erforderlichen Verweise zu Ihrem Projekt hinzuzufügen.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (erfordert Package Management Format Standardsatz für PackageReference.)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Während es wird empfohlen, die NuGet-Pakete zum Konfigurieren des Projekts verwenden, können Sie die erforderlichen Verweise manuell hinzufügen. Weitere Informationen finden Sie unter [verbessern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). Die folgende Tabelle zeigt die Dateien, denen Sie benötigen, um Verweise hinzuzufügen.

|Datei|Speicherort|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\<*Sdk-Version*> \Windows.Foundation.UniversalApiContract\<*Version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\References\<*Sdk-Version*> \Windows.Foundation.FoundationContract\<*Version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Programme\Microsoft Dateien (x86) \Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Konfigurieren Sie das Projekt werden monitorspezifische DPI-bewusst

Der visuellen Ebeneninhalt, die, den Sie Ihrer app hinzufügen, wird nicht automatisch skaliert, übereinstimmend mit die DPI-Einstellungen des Bildschirms, die auf dem es angezeigt wird. Sie müssen monitorspezifische dpi-DPI-Unterstützung für Ihre app, und klicken Sie dann stellen Sie sicher, dass der Code, den Sie verwenden, um Ihre Inhalte der visuellen Ebene zu erstellen. die aktuelle DPI-Skalierung beim Ausführen der app berücksichtigt. Hier konfigurieren wir das Projekt, um die DPI-bewusst sein. In späteren Abschnitten erläutert, wie Sie die DPI-Informationen verwenden, um den Inhalt der visuellen Ebene zu skalieren.

WPF-apps werden System-DPI-fähige standardmäßig, aber Sie müssen selbst werden monitorspezifische DPI-bewusst in einer Datei "App.manifest" zu deklarieren. Um Windows-Ebene monitorspezifische DPI-Unterstützung in der app-Manifestdatei zu aktivieren:

1. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
1. Wählen Sie im Kontextmenü des **hinzufügen** > **neues Element...** .
1. In der **neues Element hinzufügen** Dialogfeld Wählen Sie "Application Manifest File", und klicken Sie dann **hinzufügen**. (Sie können den Standardnamen beibehalten.)
1. In der Datei "App.manifest" zu finden, diese XML- und der Kommentar es:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Fügen Sie diese Einstellung nach dem öffnenden `<windowsSettings>` Tag:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. Sie müssen außerdem festlegen der **DoNotScaleForDpiChanges** in der Datei "App.config" festlegen.

    Öffnen Sie die Datei "App.config", und fügen Sie diese XML-Code in die `<configuration>` Element:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** kann nur einmal festgelegt werden. Wenn die Anwendung bereits ein festgelegt ist, müssen Sie durch Semikolons trennen Sie diese Option in der Value-Attribut.

(Weitere Informationen finden Sie in der [pro Monitor DPI-Entwicklerhandbuch und Beispiele](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) auf GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Erstellen Sie eine HwndHost abgeleitete Klasse auf Host Komposition Elemente

Host Inhalte, die Sie mit der visuellen Ebene erstellen, müssen Sie eine Klasse erstellen, die von abgeleitet [HwndHost](/dotnet/api/system.windows.interop.hwndhost). Dies ist in dem sich die meisten der Konfiguration für die Kompositions-APIs für das Hosten sollen. In dieser Klasse, verwenden Sie [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) und [COM-Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) Kompositions-APIs in Ihrer WPF-Anwendung eingefügt werden sollen. Weitere Informationen über PInvoke und COM-Interop, finden Sie unter [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/index).

> [!TIP]
> Wenn Sie möchten, überprüfen Sie den vollständigen Code am Ende des Tutorials stellen Sie sicher, dass der gesamte Code in den richtigen stellen ist, wie Sie das Lernprogramm durchzuarbeiten.

1. Fügen Sie eine neue Klassendatei hinzu, die von abgeleitet [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
    - Wählen Sie im Kontextmenü des **hinzufügen** > **Klasse...** .
    - In der **neues Element hinzufügen** Dialogfeld, nennen Sie die Klasse _CompositionHost.cs_, klicken Sie dann auf **hinzufügen**.
1. In CompositionHost.cs, bearbeiten Sie die Klassendefinition für die Ableitung **HwndHost**.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. Fügen Sie den folgenden Code und den Konstruktor der Klasse hinzu.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. Überschreiben der **BuildWindowCore** und **DestroyWindowCore** Methoden.
    > [!NOTE]
    > BuildWindowCore, Sie Aufrufen der _InitializeCoreDispatcher_ und _InitComposition_ Methoden. Sie erstellen diese Methoden in den nächsten Schritten.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) und [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) erfordern eine PInvoke-Deklaration. Platzieren Sie diese Deklaration am Ende des Codes für die Klasse an.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. Initialisieren Sie einen Thread mit einem ["coredispatcher"](/uwp/api/windows.ui.core.coredispatcher). Der Kern-Verteiler ist verantwortlich für die Verarbeitung von Windows-Meldungen, und Senden von Ereignissen für WinRT-APIs. Neue Instanzen von **"coredispatcher"** muss auf einen Thread, der eine "coredispatcher" wurde erstellt werden.
    - Erstellen Sie eine Methode mit dem Namen _InitializeCoreDispatcher_ und fügen Sie Code, um die Dispatcher-Warteschlange einzurichten.

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - Die Dispatcher-Warteschlange ist auch eine PInvoke-Deklaration erforderlich. Platzieren Sie diese Deklaration innerhalb der _PInvoke-Deklarationen_ Region, die Sie im vorherigen Schritt erstellt haben.

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    Klicken Sie jetzt haben die Dispatcher-Warteschlange bereit und können beginnen, initialisieren und Komposition erstellen.

1. Initialisieren der [Compositor](/uwp/api/windows.ui.composition.compositor). Der Compositor ist eine Factory, erstellt eine Vielzahl von Typen in, der [Windows.UI.Composition](/uwp/api/windows.ui.composition) Namespace umfassen Visualisierungen, das Effektsystem und das Animationssystem. Die Compositor-Klasse verwaltet auch die Lebensdauer der Objekte, die von der Factory erstellt.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** und **ICompositionTarget** erfordern COM importiert. Platzieren Sie diesen Code nach der _CompositionHost_ -Klasse, aber innerhalb der Namespacedeklaration.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Erstellen Sie zum Hinzufügen von Ihren Inhalts an der visuellen Struktur des WPF-UserControl

Der letzte Schritt zum Einrichten der Infrastruktur erforderlich, um den Host-Komposition, dass der Inhalt wird die visuelle Struktur von WPF der HwndHost hinzugefügt.

### <a name="create-a-usercontrol"></a>Erstellen Sie ein UserControl

Ein Steuerelement ist eine praktische Möglichkeit, Packen Sie Ihren Code, der erstellt und verwaltet die Komposition Inhalte, und fügen Sie den Inhalt einfach, Ihre XAML.

1. Fügen Sie ein neues Benutzersteuerelement-Datei zu Ihrem Projekt hinzu.
    - In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
    - Wählen Sie im Kontextmenü des **hinzufügen** > **Benutzersteuerelement...** .
    - In der **neues Element hinzufügen** Dialogfeld Namen das Benutzersteuerelement _CompositionHostControl.xaml_, klicken Sie dann auf **hinzufügen**.

    Die CompositionHostControl.xaml und CompositionHostControl.xaml.cs-Dateien erstellt und dem Projekt hinzugefügt.
1. CompositionHostControl.xaml, ersetzen die `<Grid> </Grid>` Tags mit diesem **Rahmen** -Element, das der XAML-Container ist, das Ihre HwndHost in gespeichert wird.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

In den Code für das Benutzersteuerelement, Sie erstellen eine Instanz der CompositionHost-Klasse, die Sie im vorherigen Schritt erstellt haben, und fügen Sie es als untergeordnetes Element des _CompositionHostElement_, den Rahmen, die Sie in der XAML-Seite erstellt haben.

1. Fügen Sie in CompositionHostControl.xaml.cs private Variablen für die Objekte, die Sie in Ihrem Code Komposition verwenden. Fügen Sie diese nach der Definition der Klasse hinzu.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Hinzufügen eines ereignishandlers für des Benutzersteuerelements **Loaded** Ereignis. Dies ist, in dem Sie Ihre CompositionHost-Instanz einrichten.

    - Im Konstruktor Einbinden der Ereignishandler wie folgt (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Fügen Sie die Ereignishandlermethode mit dem Namen *CompositionHostControl_Loaded*.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    Bei dieser Methode richten Sie die Objekte, die Sie in Ihrem Code Komposition verwenden. Hier ist ein kurzen Blick auf, was passiert.

    - Stellen Sie zunächst sicher, dass einrichten anhand nur einmal durchgeführt wird, ob eine Instanz von CompositionHost bereits vorhanden ist.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Der aktuelle DPI-Wert zu erhalten. Hiermit wird die Zusammensetzung Elemente richtig skalieren.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Erstellen Sie eine Instanz des CompositionHost und weisen Sie es als untergeordnetes Element des Rahmens, _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Der Compositor aus dem CompositionHost abrufen.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Verwenden Sie zum Erstellen eines Containers in visual im Compositor. Dies ist der Kompositionscontainer, dem Sie zu Ihrer Zusammensetzung Elemente hinzufügen.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Komposition Elemente hinzufügen

Mit der Infrastruktur vorhanden ist können Sie nun den Inhalt der Zusammenstellung generieren, die, den Sie anzeigen möchten.

In diesem Beispiel, die Sie hinzufügen Code, der erstellt, und erstellt eine Animation einfaches Quadrat [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Fügen Sie eine Kompositionselement. Fügen Sie diese Methoden werden in CompositionHostControl.xaml.cs hinzu auf die CompositionHostControl-Klasse.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>Behandeln von DPI-Änderungen

Der Code zum Hinzufügen und animieren ein Element berücksichtigt die aktuelle DPI-Skalierung bei Elemente werden erstellt, aber Sie müssen auch DPI-Änderungen zu berücksichtigen, während die app ausgeführt wird. Sie können behandeln die [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) Ereignis, um über Änderungen benachrichtigt werden, und passen Ihre Berechnungen auf Grundlage der neuen DPI-Wert.

1. Fügen Sie in der CompositionHostControl_Loaded-Methode nach der letzten Zeile, diese Option, um den DpiChanged-Ereignishandler einbinden.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Fügen Sie die Ereignishandlermethode mit dem Namen _CompositionHostDpiChanged_. Dieser Code wird angepasst, die Skalierung und Offsets der einzelnen Elemente und neu berechnet werden Animationen, die nicht abgeschlossen sind.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>Fügen Sie das Benutzersteuerelement auf Ihre XAML-Seite

Jetzt können Sie das Benutzersteuerelement zur XAML-UI hinzufügen.

1. In "MainWindow.xaml" wird die Fensterhöhe auf 600 und die Breite auf 840 fest.
1. Fügen Sie der XAML für die Benutzeroberfläche hinzu. Fügen Sie diese XAML zwischen den Stamm in "MainWindow.xaml" `<Grid> </Grid>` Tags.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. Zum Erstellen von neuer Elementen auf das Schaltflächen-Klickereignis zu behandeln. (Das Click-Ereignis ist bereits in der XAML eingebunden.)

    Fügen Sie in "MainWindow.Xaml.cs" dieses *Button_Click* Ereignishandlermethode. Dieser Code ruft _CompositionHost.AddElement_ um ein neues Element mit einer zufällig generierten Größe und einem Offset zu erstellen.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Sie können jetzt erstellen und Ausführen der WPF-app. Wenn Sie möchten, überprüfen Sie den vollständigen Code am Ende des Tutorials stellen Sie sicher, dass der gesamte Code in den richtigen Stellen stattfinden.

Wenn Sie die app ausführen, und klicken Sie auf die Schaltfläche, sehen Sie animierte Quadrate, die an der Benutzeroberfläche hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Ein vollständigeres Beispiel, das auf die gleiche Infrastruktur aufbaut, finden Sie unter den [Visual WPF-Layer-integrationsbeispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) auf GitHub.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erste Schritte (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/) (.NET)
- [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/) (UWP)
- [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Hier ist der vollständige Code für dieses Tutorial ein.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```