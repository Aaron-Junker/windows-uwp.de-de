---
title: Verwenden der visuellen Ebene mit Windows Forms
description: Erfahren Sie, Verfahren für die Verwendung der visuellen Ebene APIs in Kombination mit vorhandenen Windows Forms-Inhalte zum Erstellen erweiterter Animation und Effekte.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0a3aff0bee68b971accd96f895601343726024d0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985030"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Verwenden der visuellen Ebene mit Windows Forms

Sie können Windows-Runtime-Kompositions-APIs verwenden (so genannte der [visueller Ebene](/windows/uwp/composition/visual-layer)) in Ihre Windows Forms-apps auf moderne Funktionen für Windows 10-Benutzern das einfache erstellen.

Der vollständige Code für dieses Tutorial ist auf GitHub verfügbar: [Beispiel für Windows Forms HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Vorraussetzungen

Die UWP hosting-API verfügt über diese erforderlichen Komponenten.

- Es wird davon ausgegangen, dass Sie eine gewisse Vertrautheit mit der Entwicklung von Apps mithilfe von Windows Forms und UWP verfügen. Weitere Informationen:
  - [Erste Schritte mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/)
  - [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 oder höher
- Windows 10, Version 1803 oder höher
- Windows 10 SDK 17134 oder höher

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Gewusst wie: Verwenden Sie die Kompositions-APIs in Windows Forms

In diesem Tutorial erstellen eine einfache Windows Forms-Benutzeroberfläche und animierten Komposition Elemente hinzufügen. Sowohl die Komposition der Windows Forms-Komponenten sind einfach gehalten, aber der Interop-Code, der angezeigt wird, unabhängig von der Komplexität der Komponenten. Die fertige app sieht wie folgt aus.

![Die ausgeführte app-Benutzeroberfläche](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Erstellen Sie ein Windows Forms-Projekt

Der erste Schritt ist das Windows Forms-app-Projekt zu erstellen, das Definition einer Anwendung und das Hauptformular für die Benutzeroberfläche enthält.

Erstellen Sie ein neues Windows Forms-Anwendungsprojekt in Visual C# mit dem Namen _HelloComposition_:

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **neu** > **Projekt**.<br/>Die **neues Projekt** Dialogfeld wird geöffnet.
1. Unter den **installiert** (Kategorie), erweitern Sie die **Visual C#**  Knoten, und wählen Sie dann **Windows Desktop**.
1. Wählen Sie die **Windows Forms-App ((.NET Framework)** Vorlage.
1. Geben Sie den Namen _HelloComposition,_ wählen Sie Framework **.NET Framework 4.7.2**, klicken Sie dann auf **OK**.

Visual Studio erstellt das Projekt und öffnet den Designer für das standardanwendungsfenster namens "Form1.cs".

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts zur Verwendung von Windows-Runtime-APIs

Um Windows-Runtime (WinRT) APIs in Ihrer Windows Forms-app verwenden zu können, müssen Sie Visual Studio-Projekt für den Zugriff auf die Windows-Runtime zu konfigurieren. Darüber hinaus werden Vektoren umfassend von der Kompositions-APIs verwendet, daher Sie die Verwendung von Vektoren erforderlichen Verweise hinzufügen müssen.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Erstellen eines benutzerdefinierten Steuerelements zum Verwalten von interop

Für das hosten, die Inhalte, die Sie mit der visuellen Ebene erstellen, erstellen Sie ein benutzerdefiniertes Steuerelement abgeleitet [Steuerelement](/dotnet/api/system.windows.forms.control). Dieses Steuerelement ermöglicht den Zugriff auf ein Fenster [behandeln](/dotnet/api/system.windows.forms.control.handle), die Sie benötigen, um den Container für Ihre Inhalte der visuellen Ebene zu erstellen.

Dies ist in dem sich die meisten der Konfiguration für die Kompositions-APIs für das Hosten sollen. In diesem Steuerelement verwenden Sie [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) und [COM-Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) , Kompositions-APIs in Ihrer Windows Forms-app zu öffnen. Weitere Informationen über PInvoke und COM-Interop, finden Sie unter [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/index).

> [!TIP]
> Wenn Sie möchten, überprüfen Sie den vollständigen Code am Ende des Tutorials stellen Sie sicher, dass der gesamte Code in den richtigen stellen ist, wie Sie das Lernprogramm durchzuarbeiten.

1. Fügen Sie eine neue benutzerdefinierte Steuerelement-Datei zu Ihrem Projekt, die von abgeleitet [Steuerelement](/dotnet/api/system.windows.forms.control).
    - In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
    - Wählen Sie im Kontextmenü des **hinzufügen** > **neues Element...** .
    - In der **neues Element hinzufügen** wählen Sie im Dialogfeld **benutzerdefiniertes Steuerelement**.
    - Benennen Sie das Steuerelement _CompositionHost.cs_, klicken Sie dann auf **hinzufügen**. CompositionHost.cs wird in der Entwurfsansicht geöffnet.

1. Wechseln Sie zur Codeansicht für CompositionHost.cs, und fügen Sie den folgenden Code der Klasse.

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

1. Fügen Sie Code hinzu, an den Konstruktor.

    Im Konstruktor rufen Sie die _InitializeCoreDispatcher_ und _InitComposition_ Methoden. Sie erstellen diese Methoden in den nächsten Schritten.

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

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

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

    - Die Dispatcher-Warteschlange ist eine PInvoke-Deklaration erforderlich. Platzieren Sie diese Deklaration am Ende des Codes für die Klasse an. (Wir setzen diese Code innerhalb einer Region, um den Klassencode übersichtlicher zu machen.)

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

    Klicken Sie jetzt haben die Dispatcher-Warteschlange bereit und können beginnen, initialisieren und Komposition erstellen.

1. Initialisieren der [Compositor](/uwp/api/windows.ui.composition.compositor). Der Compositor ist eine Factory, erstellt eine Vielzahl von Typen in, der [Windows.UI.Composition](/uwp/api/windows.ui.composition) Aufteilung der visuellen Ebene Effektsystem und Animationssystem Namespace. Die Compositor-Klasse verwaltet auch die Lebensdauer der Objekte, die von der Factory erstellt.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Erstellen Sie ein benutzerdefiniertes Steuerelement zum Hosten von Komposition-Elementen

Es ist eine gute Idee, den Code einfügen, der generiert und verwaltet die Komposition-Elemente in einem separaten Steuerelement, das von CompositionHost abgeleitet wird. Auf diese Weise die Interop-Code, den Sie in der Klasse CompositionHost, wiederverwendbaren erstellten.

Hier erstellen Sie ein benutzerdefiniertes Steuerelement CompositionHost abgeleitet. Dieses Steuerelement wird der Visual Studio-Toolbox hinzugefügt, sodass Sie es dem Formular hinzufügen können.

1. Fügen Sie eine neue benutzerdefinierte Steuerelement-Datei zu Ihrem Projekt, das von CompositionHost abgeleitet wird.
    - In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die _HelloComposition_ Projekt.
    - Wählen Sie im Kontextmenü des **hinzufügen** > **neues Element...** .
    - In der **neues Element hinzufügen** wählen Sie im Dialogfeld **benutzerdefiniertes Steuerelement**.
    - Benennen Sie das Steuerelement _CompositionHostControl.cs_, klicken Sie dann auf **hinzufügen**. CompositionHostControl.cs wird in der Entwurfsansicht geöffnet.

1. Legen Sie im Eigenschaftenbereich für CompositionHostControl.cs Entwurfsansicht der **BackColor** Eigenschaft **ControlLight**.

    Festlegen der Hintergrundfarbe ist optional. Wir übernehmen das hier damit Sie Ihr benutzerdefinierte Steuerelement mit dem Formularhintergrund sehen können.

1. Wechseln Sie zur Codeansicht für CompositionHostControl.cs, und aktualisieren Sie die Klassendeklaration in CompositionHost abgeleitet.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Aktualisieren Sie den Konstruktor, um den Basiskonstruktor aufrufen.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Komposition Elemente hinzufügen

Mit der Infrastruktur vorhanden ist können Sie jetzt Kompositionsinhalt auf der Benutzeroberfläche der app hinzufügen.

In diesem Beispiel fügen Sie Code, der CompositionHostControl-Klasse, die erstellt und eine einfache animiert [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Fügen Sie eine Kompositionselement.

    Fügen Sie diese Methoden werden in CompositionHostControl.cs hinzu auf die CompositionHostControl-Klasse.

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

## <a name="add-the-control-to-your-form"></a>Fügen Sie das Steuerelement zum Formular

Nun, da Sie ein benutzerdefiniertes Steuerelement zum Hosten der Komposition Inhalte verfügen, können Sie es auf der Benutzeroberfläche der app hinzufügen. Hier fügen Sie eine Instanz von der CompositionHostControl, die Sie im vorherigen Schritt erstellt haben. CompositionHostControl wird automatisch hinzugefügt, um die Toolbox von Visual Studio unter  **_Projektname_ Komponenten**.

1. Fügen Sie eine Schaltfläche an der Benutzeroberfläche, in der Entwurfsansicht "Form1.cs".

    - Ziehen Sie eine Schaltfläche aus der Toolbox auf Form1. Platzieren Sie es in der oberen linken Ecke des Formulars. (Siehe Abbildung am Anfang des Tutorials wird die Platzierung der Steuerelemente zu überprüfen.)
    - Ändern Sie im Bereich Eigenschaften die **Text** Eigenschaft _"Button1"_ zu _hinzufügen Kompositionselement_.
    - Größe anzupassen Sie der Schaltfläche, sodass der gesamte Text angezeigt wird.

    (Weitere Informationen finden Sie unter [Vorgehensweise: Hinzufügen von Steuerelementen zu Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Fügen Sie eine CompositionHostControl an der Benutzeroberfläche hinzu.

    - Ziehen Sie eine CompositionHostControl aus der Toolbox auf Form1. Legen Sie es rechts neben der Schaltfläche.
    - Ändern Sie die Größe der CompositionHost, so dass es sich um den Rest des Formulars ausfüllt.

1. Handle die Schaltfläche mit den click-Ereignis.

   - Klicken Sie auf das Blitzsymbol, in die Ereignisansicht zu wechseln, klicken Sie im Bereich "Eigenschaften".
   - Wählen Sie in der Liste der Ereignisse, die **klicken Sie auf** -Ereignis *Button_Click*, und drücken Sie EINGABETASTE.
   - Dieser Code wird in der Datei Form1.cs hinzugefügt:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Fügen Sie Code hinzu, auf die Schaltfläche mit den click-Ereignishandler zum Erstellen von neuer Elementen.

    - Fügen Sie Code in der Datei Form1.cs die *Button_Click* Ereignishandler, die Sie zuvor erstellt haben. Dieser Code ruft _CompositionHostControl1.AddElement_ um ein neues Element mit einer zufällig generierten Größe und einem Offset zu erstellen. (Die Instanz von CompositionHostControl wurde automatisch mit dem Namen _compositionHostControl1_ Wenn Sie es auf das Formular gezogen.)

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

Sie können jetzt erstellen und Ausführen der Windows Forms-app. Wenn Sie die Schaltfläche klicken, sehen Sie animierte Quadrate, die an der Benutzeroberfläche hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

Ein vollständigeres Beispiel, das auf die gleiche Infrastruktur aufbaut, finden Sie unter den [Windows Forms Visual Layer-integrationsbeispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) auf GitHub.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Erste Schritte mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/) (.NET)
- [Erste Schritte mit Windows 10-apps](/windows/uwp/get-started/) (UWP)
- [Erweitern Sie Ihre desktop-Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Vollständiger Code

Hier ist der vollständige Code für dieses Tutorial ein.

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