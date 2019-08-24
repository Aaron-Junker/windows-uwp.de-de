---
title: Verwenden der visuellen Ebene mit Windows Forms
description: Erlernen Sie die Vorgehensweisen für die Verwendung der Visual Layer-APIs in Kombination mit vorhandenen Windows Forms Inhalt, um erweiterte Animationen und Effekte zu erstellen.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, UWP
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999638"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Verwenden der visuellen Ebene mit Windows Forms

Sie können Windows-Runtime Kompositions-APIs (auch als [visuelle Schicht](/windows/uwp/composition/visual-layer)bezeichnet) in Ihren Windows Forms-Apps verwenden, um moderne Benutzeroberflächen zu erstellen, die für Windows 10-Benutzer sorgen.

Den gesamten Code für dieses Tutorial finden Sie auf GitHub: [Windows Forms hellocomposition-Beispiel](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Erforderliche Komponenten

Die UWP-Hosting-API verfügt über diese Voraussetzungen.

- Wir gehen davon aus, dass Sie mit der APP-Entwicklung über Windows Forms und UWP vertraut sind. Weitere Informationen:
  - [Erste Schritte mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Einstieg in Windows 10-apps](/windows/uwp/get-started/)
  - [Verbessern Sie Ihre Desktop Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 oder höher
- Windows 10, Version 1803 oder höher
- Windows 10 SDK 17134 oder höher

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Verwenden von Kompositions-APIs in Windows Forms

In diesem Tutorial erstellen Sie eine einfache Windows Forms-Benutzeroberfläche und fügen ihr animierte Kompositions Elemente hinzu. Die Windows Forms-und Kompositions Komponenten sind einfach gehalten, aber der gezeigte Interop-Code ist unabhängig von der Komplexität der Komponenten identisch. Die fertige App sieht wie folgt aus.

![Die Benutzeroberfläche der laufenden app](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Erstellen eines Windows Forms Projekts

Der erste Schritt besteht darin, das Windows Forms App-Projekt zu erstellen, das eine Anwendungs Definition und das Hauptformular für die Benutzeroberfläche enthält.

So erstellen Sie ein neues Windows Forms-Anwendungsprojekt C# in Visual mit dem Namen " _hellocomposition_":

1. Öffnen Sie Visual Studio, und wählen Sie **Datei** > **neu** > **Projekt**aus.<br/>Das Dialogfeld " **Neues Projekt** " wird geöffnet.
1. Erweitern Sie unter der Kategorie **installiert** den **Knoten C# visuelle** Knoten, und wählen Sie dann **Windows-Desktop**aus.
1. Wählen Sie die Vorlage **Windows Forms app (.NET Framework)** aus.
1. Geben Sie den Namen _hellocomposition ein,_ wählen Sie Framework **.NET Framework 4.7.2**aus, und klicken Sie dann auf **OK**.

Visual Studio erstellt das Projekt und öffnet den Designer für das Standard Anwendungsfenster mit dem Namen Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Konfigurieren des Projekts für die Verwendung Windows-Runtime APIs

Wenn Sie in Ihrer Windows Forms-APP Windows-Runtime (WinRT)-APIs verwenden möchten, müssen Sie das Visual Studio-Projekt für den Zugriff auf die Windows-Runtime konfigurieren. Außerdem werden Vektoren von den Kompositions-APIs ausgiebig verwendet, sodass Sie die für die Verwendung von Vektoren erforderlichen Verweise hinzufügen müssen.

Nuget-Pakete sind verfügbar, um beide Anforderungen zu erfüllen. Installieren Sie die neuesten Versionen dieser Pakete, um dem Projekt die erforderlichen Verweise hinzuzufügen.  

- [Microsoft. Windows. SDK. Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (erfordert das Standardformat für die Paketverwaltung, das auf packagereferenziert ist.)
- [System. Numerics. Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Es wird empfohlen, die nuget-Pakete zu verwenden, um Ihr Projekt zu konfigurieren. Sie können jedoch die erforderlichen Verweise manuell hinzufügen. Weitere Informationen finden Sie unter [Erweitern der Desktop Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). In der folgenden Tabelle werden die Dateien angezeigt, denen Sie Verweise hinzufügen müssen.

|Datei|Speicherort|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Programme (x86) \Windows kits\10\references\<*SDK Version*> \Windows.Foundation.UniversalApiContract\<*Version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Programme (x86) \Windows kits\10\references\<*SDK Version*> \Windows.Foundation.FoundationContract\<*Version*>|
|System. Numerics. Vectors. dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System. Numerics. dll|C:\Programme (x86) \Reference assemblies\microsoft\framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Erstellen eines benutzerdefinierten Steuer Elements zum Verwalten von Interop

Zum Hosten von Inhalten, die Sie mit der visuellen Ebene erstellen, erstellen Sie ein benutzerdefiniertes Steuerelement, das von [Control](/dotnet/api/system.windows.forms.control)abgeleitet Dieses Steuerelement ermöglicht Ihnen den Zugriff auf ein Fenster [handle](/dotnet/api/system.windows.forms.control.handle), das Sie benötigen, um den Container für den Inhalt der visuellen Ebene zu erstellen.

Hier führen Sie den größten Teil der Konfiguration für das Hosting von Kompositions-APIs aus. In diesem Steuerelement verwenden Sie " [PInvoke" (Platform invoations Dienste)](/cpp/dotnet/calling-native-functions-from-managed-code) und [COM-Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) , um Kompositions-APIs in Ihre Windows Forms-APP zu bringen. Weitere Informationen über PInvoke und COM-Interop finden Sie unter [Interoperation mit nicht verwaltetem Code](/dotnet/framework/interop/index).

> [!TIP]
> Überprüfen Sie ggf. den vollständigen Code am Ende des Tutorials, um sicherzustellen, dass sich der gesamte Code bei der Durchführung des Tutorials an den richtigen Stellen befindet.

1. Fügen Sie dem Projekt, das von [Control](/dotnet/api/system.windows.forms.control)abgeleitet ist, eine neue benutzerdefinierte Steuerelement Datei hinzu.
    - Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt _hellocomposition_ .
    - Wählen Sie im Kontextmenü die Option**Neues Element** **Hinzufügen** > ... aus.
    - Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **benutzerdefiniertes Steuer**Element.
    - Benennen Sie das Steuerelement _CompositionHost.cs_, und klicken Sie auf **Hinzufügen**. CompositionHost.cs wird im Designansicht geöffnet.

1. Wechseln Sie zur Code Ansicht für CompositionHost.cs, und fügen Sie der-Klasse den folgenden Code hinzu.

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

    Im Konstruktor werden die Methoden _initializecoredispatcher_ und _initcomposition_ aufgerufen. Sie erstellen diese Methoden in den nächsten Schritten.

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

1. Initialisieren Sie einen Thread mit einem [coredispatcher](/uwp/api/windows.ui.core.coredispatcher). Der kerndispatcher ist für die Verarbeitung von Fenster Meldungen und das Verteilen von Ereignissen für WinRT-APIs zuständig. Neue Instanzen von **Compositor** müssen in einem Thread erstellt werden, der über einen coredispatcher verfügt.
    - Erstellen Sie eine Methode mit dem Namen _initializecoredispatcher_ , und fügen Sie Code hinzu, um die Verteiler-Warteschlange einzurichten.

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

    - Die Verteiler-Warteschlange erfordert eine PInvoke-Deklaration. Platzieren Sie diese Deklaration am Ende des Codes für die-Klasse. (Dieser Code wird in einer Region abgelegt, um den Klassen Code sauber zu halten.)

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

    Sie haben nun die Verteiler Warteschlange vorbereitet und können damit beginnen, Kompositions Inhalte zu initialisieren und zu erstellen.

1. Initialisieren Sie den [Compositor](/uwp/api/windows.ui.composition.compositor). Der Compositor ist eine Factory, die eine Vielzahl von Typen im [Windows. UI. Composition](/uwp/api/windows.ui.composition) -Namespace erstellt, der die visuelle Schicht, das Effekte System und das Animationssystem umfasst. Die Compositor-Klasse verwaltet auch die Lebensdauer von Objekten, die aus der Factory erstellt wurden.

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

    - **Icompositor desktopinterop** und **icompositiontarget** erfordern com-Importe. Platzieren Sie diesen Code nach der _compositionhost_ -Klasse, aber innerhalb der Namespace Deklaration.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Erstellen eines benutzerdefinierten Steuer Elements zum Hosten von Kompositions Elementen

Es empfiehlt sich, den Code, der die Kompositions Elemente generiert und verwaltet, in einem separaten Steuerelement zu platzieren, das von compositionhost abgeleitet ist. Dadurch bleibt der Interop-Code, den Sie in der Klasse compositionhost erstellt haben, wiederverwendbar.

Hier erstellen Sie ein benutzerdefiniertes Steuerelement, das von compositionhost abgeleitet ist. Dieses Steuerelement wird der Visual Studio-Toolbox hinzugefügt, sodass Sie es dem Formular hinzufügen können.

1. Fügen Sie dem Projekt, das von compositionhost abgeleitet wird, eine neue benutzerdefinierte Steuerelement Datei hinzu.
    - Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt _hellocomposition_ .
    - Wählen Sie im Kontextmenü die Option**Neues Element** **Hinzufügen** > ... aus.
    - Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **benutzerdefiniertes Steuer**Element.
    - Benennen Sie das Steuerelement _CompositionHostControl.cs_, und klicken Sie auf **Hinzufügen**. CompositionHostControl.cs wird im Designansicht geöffnet.

1. Legen Sie im Bereich Eigenschaften für die CompositionHostControl.cs-Entwurfs Ansicht die **BackColor** -Eigenschaft auf **ControlLight**fest.

    Das Festlegen der Hintergrundfarbe ist optional. Hier wird das benutzerdefinierte Steuerelement im Formular Hintergrund angezeigt.

1. Wechseln Sie zur Code Ansicht für CompositionHostControl.cs, und aktualisieren Sie die Klassen Deklaration, sodass Sie von compositionhost abgeleitet wird.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Aktualisieren Sie den Konstruktor, um den Basiskonstruktor aufzurufen.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Kompositions Elemente hinzufügen

Nachdem Sie die Infrastruktur eingerichtet haben, können Sie der App-Benutzeroberfläche nun Kompositions Inhalte hinzufügen.

In diesem Beispiel fügen Sie der compositionhostcontrol-Klasse Code hinzu, mit dem ein einfaches [spritevisual](/uwp/api/windows.ui.composition.spritevisual)erstellt und animiert wird.

1. Fügen Sie ein Kompositions Element hinzu.

    Fügen Sie in CompositionHostControl.cs diese Methoden der compositionhostcontrol-Klasse hinzu.

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

## <a name="add-the-control-to-your-form"></a>Fügen Sie das Steuerelement zum Formular hinzu.

Nachdem Sie nun über ein benutzerdefiniertes Steuerelement zum Hosten von Kompositions Inhalten verfügen, können Sie es der App-Benutzeroberfläche hinzufügen. Hier fügen Sie eine Instanz des compositionhostcontrol-Steuer Elements hinzu, das Sie im vorherigen Schritt erstellt haben. Compositionhostcontrol wird der Visual Studio-Toolbox unter  **_Project Name_ Components**automatisch hinzugefügt.

1. Fügen Sie der Benutzeroberfläche in der Entwurfs Ansicht Form1.cs eine Schaltfläche hinzu.

    - Ziehen Sie eine Schaltfläche aus der Toolbox auf Form1. Platzieren Sie ihn in der oberen linken Ecke des Formulars. (Sehen Sie sich das Bild am Anfang des Tutorials an, um die Platzierung der Steuerelemente zu überprüfen.)
    - Ändern Sie im Eigenschaften Bereich die **Text** -Eigenschaft von _Button1_ in _Add Composition Element_.
    - Ändern Sie die Größe der Schaltfläche, damit der gesamte Text angezeigt wird.

    (Weitere Informationen finden [Sie unter Vorgehensweise: Hinzufügen von Steuer](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)Elementen zu Windows Forms.)

1. Fügen Sie der Benutzeroberfläche ein compositionhostcontrol-Steuerelement hinzu.

    - Ziehen Sie ein compositionhostcontrol aus der Toolbox auf Form1. Platzieren Sie es rechts von der Schaltfläche.
    - Ändern Sie die Größe des compositionhost, sodass er den Rest des Formulars füllt.

1. Behandeln Sie das Click-Ereignis der Schaltfläche.

   - Klicken Sie im Bereich Eigenschaften auf den Blitz, um zur Ansicht Ereignisse zu wechseln.
   - Wählen Sie in der Liste Ereignisse das **Click** -Ereignis aus, geben Sie *Button_Click*ein, und drücken Sie die EINGABETASTE.
   - Dieser Code wird in Form1.cs hinzugefügt:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Fügen Sie Code zum Click-Handler der Schaltfläche hinzu, um neue Elemente zu erstellen.

    - Fügen Sie in Form1.cs dem *Button_Click* -Ereignishandler, den Sie zuvor erstellt haben, Code hinzu. Dieser Code ruft _CompositionHostControl1. AddElement_ auf, um ein neues Element mit einer zufällig generierten Größe und einem zufällig generierten Offset zu erstellen. (Die Instanz von compositionhostcontrol wurde automatisch mit dem Namen _compositionHostControl1_ benannt, als Sie Sie auf das Formular gezogen haben.)

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

Nun können Sie Ihre Windows Forms-app erstellen und ausführen. Wenn Sie auf die Schaltfläche klicken, sollten die animierten Quadrate der Benutzeroberfläche hinzugefügt werden.

## <a name="next-steps"></a>Nächste Schritte

Ein ausführeres Beispiel, das auf derselben Infrastruktur aufbaut, finden Sie im Beispiel [Windows Forms Visual Layer-Integration](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) auf GitHub.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Ersten Schritte [mit Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) NET
- Interoperabilität [mit nicht verwaltetem Code](/dotnet/framework/interop/) NET
- Einstieg [in Windows 10-apps](/windows/uwp/get-started/) UWP
- [Verbessern Sie Ihre Desktop Anwendung für Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) UWP
- [Windows. UI. Composition-Namespace](/uwp/api/windows.ui.composition) UWP

## <a name="complete-code"></a>Vollständiger Code

Im folgenden finden Sie den gesamten Code für dieses Tutorial.

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