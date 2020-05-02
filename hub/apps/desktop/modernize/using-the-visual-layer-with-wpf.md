---
title: Benutzung der visuellen Ebene mit WPF
description: Lerne Methoden für die Verwendung der APIs der visuellen Ebene in Kombination mit vorhandenen WPF-Inhalten kennen, um Animationen und hochwertige Effekte zu erzeugen.
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215155"
---
# <a name="using-the-visual-layer-with-wpf"></a>Benutzung der visuellen Ebene mit WPF

Du kannst in deinen WPF-Apps (Windows Presentation Foundation) die Windows Runtime Composition-APIs (auch als [visuelle Ebene](/windows/uwp/composition/visual-layer) bezeichnet) verwenden, um moderne Umgebungen für Windows 10-Benutzer zu schaffen.

Den gesamten Code für dieses Tutorial findest du auf GitHub: [WPF-Beispiel „HelloComposition“](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Voraussetzungen

Für die UWP-XAML-Hosting-API gelten die folgenden Voraussetzungen.

- Es wird davon ausgegangen, dass du mit der App-Entwicklung mit WPF und UWP grundsätzlich vertraut bist. Weitere Informationen:
  - [Erste Schritte (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/)
  - [Verbessern Ihrer Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 oder höher
- Windows 10 Version 1803 oder höher
- Windows 10 SDK 17134 oder höher

## <a name="how-to-use-composition-apis-in-wpf"></a>Verwenden der Composition-APIs in WPF

In diesem Tutorial erstellst du eine einfache Benutzeroberfläche für eine WPF-App und fügst ihr animierte Composition-Elemente hinzu. Sowohl die WPF- als auch die Composition-Komponenten sind einfach gehalten, aber der gezeigte Interoperabilitätscode ist unabhängig von der Komplexität der Komponenten identisch. Die fertige App sieht wie folgt aus.

![Benutzeroberfläche der laufenden App](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Erstellen eines WPF-Projekts

Der erste Schritt besteht darin, das Projekt für die WPF-App zu erstellen. Dazu gehören eine Anwendungsdefinition und die XAML-Seite für die Benutzeroberfläche.

So erstellst du ein neues WPF-Anwendungsprojekt in Visual C# namens _HelloComposition_

1. Öffne Visual Studio, und wähle **Datei** > **Neu** > **Projekt** aus.

    Das Dialogfeld **Neues Projekt** wird geöffnet.
1. Erweitere unter der Kategorie **Installiert** den Knoten **Visual C#** , und wähle dann **Windows-Desktop** aus.
1. Wähle die Vorlage **WPF-App (.NET Framework)** aus.
1. Gib den Namen _HelloComposition_ ein, wähle als Framework **.NET Framework 4.7.2** aus, und klicke dann auf **OK**.

    Visual Studio erstellt das Projekt und öffnet den Designer für das Standardanwendungsfenster „MainWindow.xaml“.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts für die Verwendung der Windows-Runtime-APIs

Wenn du in deiner WPF-App die Windows-Runtime-APIs (WinRT) verwenden möchtest, musst du das Visual Studio-Projekt für den Zugriff auf die Windows-Runtime konfigurieren. Außerdem werden Vektoren von den Composition-APIs ausgiebig verwendet, sodass du die für die Verwendung von Vektoren erforderlichen Verweise hinzufügen musst.

Für diese beiden Anforderungen sind NuGet-Pakete verfügbar. Installieren Sie die neuesten Versionen dieser Pakete, um dem Projekt die erforderlichen Verweise hinzuzufügen.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (erfordert die Festlegung des Standardformats für die Paketverwaltung auf „PackageReference“)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Es wird empfohlen, die NuGet-Pakete für die Konfiguration Ihres Projekts zu verwenden. Sie können die erforderlichen Verweise jedoch auch manuell hinzufügen. Weitere Informationen finden Sie unter [Verbessern der Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). In der folgenden Tabelle werden die Dateien angezeigt, denen Sie Verweise hinzufügen müssen.

|File|Speicherort|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Programme (x86)\Windows Kits\10\References\<*SDK-Version*>\Windows.Foundation.UniversalApiContract\<*Version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Programme (x86)\Windows Kits\10\References\<*SDK-Version*>\Windows.Foundation.FoundationContract\<*Version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Programme (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Konfigurieren des Projekts für die Erkennung der DPI pro Monitor

Der Inhalt der visuellen Ebene, den du der App hinzufügst, wird nicht automatisch gemäß den DPI-Einstellungen des Bildschirms skaliert, auf dem er angezeigt wird. Du musst die DPI-Informationen pro Monitor für deine App aktivieren und dann sicherstellen, dass dein Code zum Erstellen des Inhalts der visuellen Ebene die aktuelle DPI-Skalierung bei der App-Ausführung berücksichtigt. Im Folgenden wird das Projekt für die Erkennung der DPI-Werte konfiguriert. In späteren Abschnitten wird gezeigt, wie du die DPI-Informationen zum Skalieren des Inhalts der visuellen Ebene verwendest.

WPF-Apps erkennen standardmäßig die System-DPI, müssen dies jedoch in der Datei „app.manifest“ für die Erkennung der DPI pro Monitor selbst deklarieren. So aktivierst du die DPI-Erkennung pro Monitor auf Windows-Ebene in der App-Manifestdatei

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
1. Wähle im Kontextmenü **Hinzufügen** > **Neues Element** aus.
1. Wähle im Dialogfeld **Neues Element hinzufügen** die Option „Anwendungsmanifestdatei“ aus, und klicke dann auf **Hinzufügen**. (Du kannst den Standardnamen beibehalten.)
1. Suche in der Datei „app.manifest“ nach dem folgenden XML-Code, und hebe seine Auskommentierung auf:

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Füge diese Einstellung nach dem öffnenden `<windowsSettings>`-Tag hinzu:

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. Darüber hinaus musst Du in der Datei „App.config“ die Einstellung **DoNotScaleForDpiChanges** festlegen.

    Öffne die Datei „App.config“, und füge diesen XML-Code im `<configuration>`-Element hinzu:

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** kann nur einmal festgelegt werden. Wenn diese Einstellung in deiner Anwendung bereits einmal festgelegt wurde, musst du diesen Schalter innerhalb des value-Attributs mit einem Semikolon abtrennen.

(Weitere Informationen findest du unter [Per Monitor DPI Developer Guide and samples](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) (Entwicklerhandbuch und Beispiele zur Einstellung der DPI pro Monitor) auf GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Erstellen einer abgeleiteten HwndHost-Klasse zum Hosten von Composition-Elementen

Zum Hosten von Inhalten, die du mit der visuellen Ebene erstellst, musst du eine Klasse erstellen, die von [HwndHost](/dotnet/api/system.windows.interop.hwndhost) abgeleitet ist. In dieser führst du den größten Teil der Konfiguration für das Hosting von Composition-APIs aus. Du verwendest in dieser Klasse [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) und [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute), um Composition-APIs in deine WPF-App einzubinden. Weitere Informationen zu PInvoke und COM-Interop findest du unter [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/index).

> [!TIP]
> Überprüfe ggf. den vollständigen Code am Ende des Tutorials, um sicherzustellen, dass sich sämtlicher Code aus diesem Tutorial an den richtigen Stellen befindet.

1. Füge dem Projekt eine neue Klassendatei hinzu, die von [HwndHost](/dotnet/api/system.windows.interop.hwndhost) abgeleitet ist.
    - Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
    - Wähle im Kontextmenü **Hinzufügen** > **Klasse** aus.
    - Benenne die Klasse im Dialogfeld **Neues Element hinzufügen** mit _CompositionHost.cs_, und klicke dann auf **Hinzufügen**.
1. Bearbeite in „CompositionHost.cs“ die Klassendefinition, um die Klasse von **HwndHost** abzuleiten.

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

1. Füge der Klasse den folgenden Code und Konstruktor hinzu.

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

1. Überschreibe die Methoden **BuildWindowCore** und **DestroyWindowCore**.
    > [!NOTE]
    > In BuildWindowCore werden die Methoden _InitializeCoreDispatcher_ und _InitComposition_ aufgerufen. Du erstellst diese Methoden in den nächsten Schritten.

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

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) und [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) erfordern eine PInvoke-Deklaration. Füge diese Deklaration am Ende des Codes für die Klasse ein.

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

1. Initialisiere einen Thread mit einem [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). Der CoreDispatcher ist für die Verarbeitung von Fenstermeldungen und das Verteilen von Ereignissen für WinRT-APIs zuständig. Neue Instanzen von **CoreDispatcher** müssen in einem Thread erstellt werden, der über einen CoreDispatcher verfügt.
    - Erstelle eine Methode mit dem Namen _InitializeCoreDispatcher_, und füge Code zum Einrichten der Verteilerwarteschlange hinzu.

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

    - Die Verteilerwarteschlange erfordert ebenfalls eine PInvoke-Deklaration. Platziere diese Deklaration im Abschnitt der _PInvoke-Deklarationen_, die du im vorherigen Schritt erstellt hast.

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

    Damit ist die Verteilerwarteschlange vorbereitet, und du kannst damit beginnen, Composition-Inhalte zu initialisieren und zu erstellen.

1. Initialisiere den [Compositor](/uwp/api/windows.ui.composition.compositor). Der Compositor ist eine Factory, die eine Vielzahl von Typen im Namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition) erstellt. Dazu gehören z. B. visuelle Elemente, das Effektsystem und das Animationssystem. Die Compositor-Klasse verwaltet auch die Lebensdauer von Objekten, die von der Factory erstellt wurden.

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

    - **ICompositorDesktopInterop** und **ICompositionTarget** erfordern COM-Importe. Platziere diesen Code hinter der _CompositionHost_-Klasse, aber noch innerhalb der Namespacedeklaration.

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

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Erstellen eines UserControl-Steuerelements zum Hinzufügen des Inhalts zur visuellen WPF-Struktur

Der letzte Schritt beim Einrichten der zum Hosten von Composition-Inhalten erforderlichen Infrastruktur besteht darin, den HwndHost der visuellen WPF-Struktur hinzuzufügen.

### <a name="create-a-usercontrol"></a>Erstellen eines UserControl-Steuerelements

Ein UserControl-Steuerelement stellt eine bequeme Möglichkeit dar, deinen Code, mit dem Composition-Inhalte erstellt und verwaltet werden, zu packen und den Inhalt dann ganz einfach deinem XAML-Code hinzuzufügen.

1. Füge deinem Projekt ein neues Benutzersteuerelement hinzu.
    - Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
    - Wähle im Kontextmenü **Hinzufügen** > **Benutzersteuerelement** aus.
    - Benenne das Benutzersteuerelement im Dialogfeld **Neues Element hinzufügen** mit _CompositionHostControl.xaml_, und klicke dann auf **Hinzufügen**.

    Die Dateien „CompositionHostControl.xaml“ und „CompositionHostControl.xaml.cs“ werden erstellt und deinem Projekt hinzugefügt.
1. Ersetze in „CompositionHostControl.xaml“ die `<Grid> </Grid>`-Tags durch dieses **Border**-Element. Dabei handelt es sich um den XAML-Container, in den dein HwndHost eingefügt wird.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

Erstelle im Code für das Benutzersteuerelement eine Instanz der CompositionHost-Klasse, die du im vorherigen Schritt erstellt hast, und füge sie als untergeordnetes Element von _CompositionHostElement_ (dem Rahmen, den du auf der XAML-Seite erstellt hast) hinzu.

1. Füge in „CompositionHostControl.xaml.cs“ private Variablen für die Objekte hinzu, die du in deinem Composition-Code verwendest. Füge diese nach der Klassendefinition hinzu.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Füge einen Handler für das **Loaded**-Ereignis des Benutzersteuerelements hinzu. An dieser Stelle richtest du die CompositionHost-Instanz ein.

    - Füge den Ereignishandler im Konstruktor wie hier gezeigt ein (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Füge die Ereignishandlermethode mit dem Namen *CompositionHostControl_Loaded* hinzu.
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

    In dieser Methode richtest du die Objekte ein, die du in deinem Composition-Code verwendest. Dies ist eine kurze Übersicht über die Abläufe.

    - Stelle zunächst sicher, dass die Einrichtung nur einmal durchgeführt wird, indem du überprüfst, ob bereits eine Instanz von CompositionHost vorhanden ist.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Rufe den aktuellen DPI-Wert ab. Dieser wird verwendet, um deine Composition-Elemente ordnungsgemäß zu skalieren.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Erstelle eine Instanz von CompositionHost, und weise diese als untergeordnetes Element des Rahmens _CompositionHostElement_ zu.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Rufe den Compositor vom CompositionHost ab.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Verwende den Compositor, um eine Containervisualisierung zu erstellen. Dies ist der Composition-Container, dem du deine Composition-Elemente hinzufügst.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Hinzufügen von Composition-Elementen

Nachdem du die Infrastruktur eingerichtet hast, kannst du nun den Composition-Inhalt generieren, den du anzeigen möchtest.

In diesem Beispiel fügst du Code hinzu, mit dem ein einfaches Quadrat ([SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)) erstellt und animiert wird.

1. Füge ein Composition-Element hinzu. Füge in „CompositionHostControl.xaml.cs“ der CompositionHostControl-Klasse diese Methoden hinzu.

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

Der Code zum Hinzufügen und Animieren eines Elements berücksichtigt die aktuelle DPI-Skalierung beim Erstellen von Elementen, du musst aber auch mögliche DPI-Änderungen während der App-Ausführung berücksichtigen. Du kannst das [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged)-Ereignis behandeln, um über Änderungen benachrichtigt zu werden und ihre Berechnungen basierend auf dem neuen DPI-Wert anzupassen.

1. Füge in der CompositionHostControl_Loaded-Methode nach der letzten Zeile folgenden Code hinzu, um den Ereignishandler DpiChanged einzubinden.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Füge die Ereignishandlermethode mit dem Namen _CompositionHostDpiChanged_ hinzu. Dieser Code passt die Skalierung und den Offset der einzelnen Elemente an und berechnet alle Animationen neu, die nicht vollständig sind.

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

## <a name="add-the-user-control-to-your-xaml-page"></a>Hinzufügen des Benutzersteuerelements zur XAML-Seite

Nun kannst du das Benutzersteuerelement der XAML-Benutzeroberfläche hinzufügen.

1. Lege in „MainWindow.xaml“ die Fensterhöhe auf 600 und die Breite auf 840 fest.
1. Füge den XAML-Code für die Benutzeroberfläche hinzu. Füge in „MainWindow.xaml“ diesen XAML-Code zwischen den `<Grid> </Grid>`-Stammtags hinzu.

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

1. Behandele das Klicken auf die Schaltfläche, um neue Elemente zu erstellen. (Das Click-Ereignis ist bereits im XAML-Code eingerichtet.)

    Füge in „MainWindow.xaml.cs“ diese *Button_Click*-Ereignishandlermethode hinzu. Mit diesem Code wird _CompositionHost.AddElement_ aufgerufen, um ein neues Element mit einer zufällig generierten Größe und einem Zufallsoffset zu erstellen.

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

Jetzt kannst du die WPF-App kompilieren und ausführen. Überprüfe ggf. den vollständigen Code am Ende des Tutorials, um sicherzustellen, dass sich sämtlicher Code an den richtigen Stellen befindet.

Wenn du die App ausführst und auf die Schaltfläche klickst, sollten die animierten Quadrate auf der Benutzeroberfläche hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

Ein ausführlicheres Beispiel, das auf der gleichen Infrastruktur aufbaut, findest du unter [WPF Visual layer integration sample](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) (Beispiel für die Integration der visuellen WPF-Ebene) auf GitHub.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erste Schritte (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/) (.NET)
- [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/) (UWP)
- [Verbessern Ihrer Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition-Namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Dies ist der vollständige Code für dieses Tutorial.

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