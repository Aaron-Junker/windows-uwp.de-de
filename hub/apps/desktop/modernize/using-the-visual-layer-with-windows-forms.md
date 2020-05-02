---
title: Verwenden der visuellen Ebene mit Windows Forms
description: Lernen Sie Methoden für die Verwendung der APIs der visuellen Ebene in Kombination mit vorhandenen Windows Forms-Inhalten kennen, um Animationen und hochwertige Effekte zu erzeugen.
ms.date: 03/18/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "69999638"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Verwenden der visuellen Ebene mit Windows Forms

Sie können in Ihren Windows Forms-Apps die Composition-APIs von Windows-Runtime (die auch als [visuelle Ebene](/windows/uwp/composition/visual-layer) bezeichnet werden) verwenden, um moderne Umgebungen für Windows 10-Benutzer zu erstellen.

Den gesamten Code für dieses Tutorial finden Sie auf GitHub: [Windows Forms-HelloComposition-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Voraussetzungen

Für die UWP hostende API gelten diese Voraussetzungen.

- Es wird davon ausgegangen, dass Sie mit der App-Entwicklung mit Win32 und UWP vertraut sind. Weitere Informationen:
  - [Erste Schritte mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/)
  - [Verbessern Ihrer Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 oder höher
- Windows 10 Version 1803 oder höher
- Windows 10 SDK ab 17134

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Verwenden der Composition-APIs in Windows Forms

In diesem Tutorial erstellen Sie eine einfache Benutzeroberfläche für eine Windows Forms-App und fügen ihr animierte Composition-Elemente hinzu. Sowohl die Windows Forms- als auch die Composition-Komponenten sind einfach gehalten, aber der gezeigte Interoperabilitätscode ist unabhängig von der Komplexität der Komponenten identisch. Die fertige App sieht wie folgt aus.

![Benutzeroberfläche der laufenden App](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Erstellen eines Windows Forms-Projekts

Der erste Schritt besteht darin, das Projekt für die Windows Forms-App zu erstellen. Dazu gehören eine Anwendungsdefinition und das Hauptformular für die Benutzeroberfläche.

So erstellen Sie ein neues Windows Forms-Anwendungsprojekt in Visual C# namens _HelloComposition_:

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **Neu** > **Projekt** aus.<br/>Das Dialogfeld **Neues Projekt** wird geöffnet.
1. Erweitern Sie unter der Kategorie **Installiert** den Knoten **Visual C#** , und wählen Sie dann **Windows-Desktop** aus.
1. Wählen Sie die Vorlage **Windows Forms-App (.NET Framework)** aus.
1. Geben Sie den Namen _HelloComposition_ ein, wählen Sie als Framework **.NET Framework 4.7.2** aus, und klicken Sie dann auf **OK**.

Visual Studio erstellt das Projekt und öffnet den Designer für das Standardanwendungsfenster „Form1.cs“.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts für die Verwendung der Windows-Runtime-APIs

Wenn Sie in Ihrer Windows Forms-App die Windows-Runtime-APIs (WinRT) verwenden möchten, müssen Sie das Visual Studio-Projekt für den Zugriff auf die Windows-Runtime konfigurieren. Außerdem werden Vektoren von den Composition-APIs ausgiebig verwendet, sodass Sie die für die Verwendung von Vektoren erforderlichen Verweise hinzufügen müssen.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Erstellen eines benutzerdefinierten Steuerelements zum Verwalten der Interoperabilität

Zum Hosten von Inhalten, die Sie mit der visuellen Ebene erstellen, müssen Sie ein benutzerdefiniertes Steuerelement erstellen, das von [Control](/dotnet/api/system.windows.forms.control) abgeleitet ist. Dieses Steuerelement gibt Ihnen Zugriff auf ein [Fensterhandle](/dotnet/api/system.windows.forms.control.handle), das Sie benötigen, um den Container für die Inhalte der visuellen Ebene zu erstellen.

In dieser führen Sie den größten Teil der Konfiguration für das Hosting von Composition-APIs aus. Sie verwenden in diesem Steuerelement [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) und [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute), um Composition-APIs in Ihre Windows Forms-App einzubinden. Weitere Informationen zu PInvoke und COM-Interop finden Sie unter [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/index).

> [!TIP]
> Überprüfen Sie bei Bedarf am Ende des Tutorials den vollständigen Code, um sicherzustellen, dass sich der gesamte Code während des Durcharbeitens an den richtigen Stellen befindet.

1. Fügen Sie Ihrem Projekt eine neue benutzerdefinierte Steuerelementdatei hinzu, die von [Control](/dotnet/api/system.windows.forms.control) abgeleitet ist.
    - Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
    - Wählen Sie im Kontextmenü **Hinzufügen** > **Neues Element...** aus.
    - Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Benutzerdefiniertes Steuerelement** aus.
    - Benennen Sie das Steuerelement _CompositionHost.cs_, und klicken Sie dann auf **Hinzufügen**. CompositionHost.cs wird in der Entwurfsansicht geöffnet.

1. Wechseln Sie zur Codeansicht für CompositionHost.cs, und fügen Sie der-Klasse den folgenden Code hinzu.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. Fügen Sie dem Konstruktor Code hinzu.

    Im Konstruktor rufen Sie die Methoden _InitializeCoreDispatcher_ und _InitComposition_ auf. Sie erstellen diese Methoden in den nächsten Schritten.

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }
    ```

1. Initialisieren Sie einen Thread mit einem [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). Der CoreDispatcher ist für die Verarbeitung von Fenstermeldungen und das Verteilen von Ereignissen für WinRT-APIs zuständig. Neue Instanzen von **Compositor** müssen in einem Thread erstellt werden, der über einen CoreDispatcher verfügt.
    - Erstellen Sie eine Methode mit dem Namen _InitializeCoreDispatcher_, und fügen Sie Code zum Einrichten der Verteilerwarteschlange hinzu.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

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

    - Die Verteilerwarteschlange erfordert eine PInvoke-Deklaration. Fügen Sie diese Deklaration am Ende des Codes für die Klasse ein. (Wir platzieren diesen Code innerhalb eines Bereichs, um den Klassencode ordentlich zu halten.)

    ```csharp
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

    #endregion PInvoke declarations
    ```

    Damit ist die Verteilerwarteschlange vorbereitet, und Sie können damit beginnen, Composition-Inhalte zu initialisieren und zu erstellen.

1. Initialisieren Sie den [Compositor](/uwp/api/windows.ui.composition.compositor). Der Compositor ist eine Factory, die eine Vielzahl von Typen im Namespace [Windows.UI.Composition](/uwp/api/windows.ui.composition) erstellt. Dazu gehören z. B. die visuelle Ebene, das Effektsystem und das Animationssystem. Die Compositor-Klasse verwaltet auch die Lebensdauer von Objekten, die von der Factory erstellt wurden.

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop** und **ICompositionTarget** erfordern COM-Importe. Platzieren Sie diesen Code nach der _CompositionHost_-Klasse, aber noch innerhalb der Namespacedeklaration.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Erstellen eines benutzerdefinierten Steuerelements zum Hosten von Kompositionselementen

Es empfiehlt sich, den Code, der die Kompositionselemente generiert und verwaltet, in einem separaten Steuerelement zu platzieren, das von CompositionHost abgeleitet ist. Dadurch bleibt der Interop-Code, den Sie in der CompositionHost-Klasse erstellt haben, wiederverwendbar.

Hier erstellen Sie ein benutzerdefiniertes Steuerelement, das von CompositionHost abgeleitet ist. Dieses Steuerelement wird der Visual Studio-Toolbox hinzugefügt, sodass Sie es dem Formular hinzufügen können.

1. Fügen Sie Ihrem Projekt eine neue benutzerdefinierte Steuerelementdatei hinzu, die von CompositionHost abgeleitet ist.
    - Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt _HelloComposition_.
    - Wählen Sie im Kontextmenü **Hinzufügen** > **Neues Element...** aus.
    - Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Option **Benutzerdefiniertes Steuerelement** aus.
    - Benennen Sie das Steuerelement _CompositionHostControl.cs_, und klicken Sie dann auf **Hinzufügen**. CompositionHostControl.cs wird in der Entwurfsansicht geöffnet.

1. Legen Sie im Eigenschaftsbereich der Entwurfsansicht von CompositionHostControl.cs die Eigenschaft **BackColor** auf **ControlLight** fest.

    Das Festlegen der Hintergrundfarbe ist optional. Wir nehmen es an dieser Stelle vor, damit Sie Ihr benutzerdefiniertes Steuerelement vor dem Formularhintergrund sehen können.

1. Wechseln Sie zur Codeansicht für CompositionHostControl.cs, und aktualisieren Sie die Klassendeklaration, sodass sie von CompositionHost abgeleitet wird.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Aktualisieren Sie den Konstruktor, sodass er den Basiskonstruktor aufruft.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Hinzufügen von Composition-Elementen

Nachdem Sie die Infrastruktur eingerichtet haben, können Sie der Benutzeroberfläche der App jetzt Composition-Inhalte hinzufügen.

In diesem Beispiel fügen Sie Code zur CompositionHostControl-Klasse hinzu, der ein einfaches [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) (Quadrat) erstellt und animiert.

1. Fügen Sie ein Composition-Element hinzu.

    Fügen Sie in „CompositionHostControl.cs“ der CompositionHostControl-Klasse diese Methoden hinzu.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
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

## <a name="add-the-control-to-your-form"></a>Hinzufügen des Steuerelements zum Formular

Da Sie jetzt über ein benutzerdefiniertes Steuerelement zum Hosten von Composition-Inhalten verfügen, können Sie es der Benutzeroberfläche der App hinzufügen. Hier fügen Sie eine Instanz des CompositionHostControl hinzu, das Sie im vorherigen Schritt erstellt haben. CompositionHostControl wird der Visual Studio-Toolbox automatisch unter dem **_Projektnamen_ Components** hinzugefügt.

1. Fügen Sie der Benutzeroberfläche in der Entwurfsansicht von Form1.CS eine Schaltfläche hinzu.

    - Ziehen Sie eine Schaltfläche aus der Toolbox auf Form1. Platzieren Sie sie in der oberen linken Ecke des Formulars. (Sehen Sie sich die Abbildung am Anfang des Tutorials an, um die Platzierung der Steuerelemente zu überprüfen.)
    - Ändern Sie im Bereich „Eigenschaften“ die **Text**-Eigenschaft von _button1_ in _Composition-Element hinzufügen_.
    - Ändern Sie die Größe der Schaltfläche, sodass der gesamte Text angezeigt wird.

    (Weitere Informationen finden Sie unter [Gewusst wie: Hinzufügen von Steuerelementen zu Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Fügen Sie der Benutzeroberfläche ein CompositionHostControl-Steuerelement hinzu.

    - Ziehen Sie ein CompositionHostControl-Steuerelement aus der Toolbox auf Form1. Platzieren Sie es rechts neben der Schaltfläche.
    - Ändern Sie die Größe des CompositionHost, sodass er den Rest des Formulars ausfüllt.

1. Behandeln Sie das Schaltflächen-Klickereignis.

   - Klicken Sie im Eigenschaftsbereich auf den Gewitterblitz, um zur Ereignisansicht zu wechseln.
   - Wählen Sie in der Ereignisliste das **Click**-Ereignis aus, geben Sie *Button_Click* ein, und drücken Sie die EINGABETASTE.
   - Dieser Code wird in Form1.cs hinzugefügt:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Fügen Sie dem Schaltflächen-Klick-Handler Code hinzu, um neue Elemente zu erstellen.

    - Fügen Sie in Form1.cs dem *Button_Click*-Ereignishandler, den Sie zuvor erstellt haben, Code hinzu. Mit diesem Code wird _CompositionHostControl1.AddElement_ aufgerufen, um ein neues Element mit einer zufällig generierten Größe und einem Zufallsoffset zu erstellen. (Die Instanz von CompositionHostControl wurde automatisch mit dem Namen _CompositionHostControl1_ benannt, als Sie sie auf das Formular gezogen haben.)

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Jetzt können Sie die Windows Forms-App kompilieren und ausführen. Wenn Sie auf die Schaltfläche klicken, sollten der Benutzeroberfläche die animierten Quadrate hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

Ein ausführlicheres Beispiel, das auf der gleichen Infrastruktur aufbaut, finden Sie unter [Windows Forms Visual layer integration sample](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) (Beispiel für die Integration der visuellen Windows Forms-Ebene) auf GitHub.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erste Schritte mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/) (.NET)
- [Erste Schritte mit Windows 10-Apps](/windows/uwp/get-started/) (UWP)
- [Verbessern Ihrer Desktopanwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition-Namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Dies ist der vollständige Code für dieses Tutorial.

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
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
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
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

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
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