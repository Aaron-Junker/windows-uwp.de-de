---
title: Bildschirmaufnahme
description: Der Windows.Graphics.Capture-Namespace enthält APIs zum Abrufen von Frames aus einer Anzeige oder einem Anwendungsfenster, um Videostreams zu erstellen oder gemeinsame und interaktive Benutzeroberflächen zu erstellen.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 06/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP, Bildschirmaufnahme
ms.localizationpriority: medium
ms.openlocfilehash: fce0dbad0e36fe2470d8e07944afa80054cfb3d7
ms.sourcegitcommit: a5031e95b90ee72babace8e80370551f3fa88593
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88722025"
---
# <a name="screen-capture"></a>Bildschirmaufnahme

Ab Windows 10, Version 1803, stellt der [Windows. Graphics. Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) -Namespace APIs zum Abrufen von Frames aus einem Anzeige-oder Anwendungsfenster bereit, um Videostreams oder Momentaufnahmen zu erstellen, um kollaborative und interaktive Benutzeroberflächen zu erstellen.

Mit der Bildschirmaufnahme rufen Entwickler eine sichere Systembenutzer Oberfläche für Endbenutzer auf, um die zu erfassende Anzeige oder das Anwendungsfenster auszuwählen, und ein gelber Benachrichtigungs Rahmen wird vom System um das aktiv erfasste Element gezeichnet. Bei mehreren gleichzeitigen Erfassungs Sitzungen wird ein gelber Rahmen um jedes erfasste Element gezeichnet.

> [!NOTE]
> Die Bildschirmaufnahme-APIs werden nur auf Desktop-und Windows Mixed Reality-, immersiven Headsets unterstützt.

## <a name="add-the-screen-capture-capability"></a>Hinzufügen der Bildschirmaufnahme Funktion

Die im **Windows. Graphics. Capture** -Namespace gefundenen APIs erfordern eine allgemeine Funktion, die im Manifest der Anwendung deklariert werden muss:

1. Öffnen Sie " **Package. appxmanifest** " im **Projektmappen-Explorer**.
2. Wählen Sie die Registerkarte **Funktionen** aus.
3. Überprüfen Sie die **Grafik Erfassung**.

![Grafik Erfassung](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Starten der Systembenutzer Oberfläche zum Starten der Bildschirmaufzeichnung

Vor dem Starten der Systembenutzer Oberfläche können Sie überprüfen, ob Ihre Anwendung derzeit Bildschirmaufzeichnungen aufnehmen kann. Es gibt mehrere Gründe, warum Ihre Anwendung die Bildschirmaufnahme möglicherweise nicht verwenden kann. Dies umfasst u. a., ob das Gerät die Hardwareanforderungen nicht erfüllt oder ob die Zielanwendung für die Erfassung von Bildschirmen verwendet wird. Verwenden Sie die **IsSupported** -Methode in der [graphicscapturesession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) -Klasse, um zu bestimmen, ob die UWP-Bildschirmaufnahme unterstützt wird:

```csharp
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported())
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed;
    }
}
```

```vb
Public Sub OnInitialization()
    If Not GraphicsCaptureSession.IsSupported Then
        CaptureButton.Visibility = Visibility.Collapsed
    End If
End Sub
```

Nachdem Sie überprüft haben, dass die Bildschirmaufnahme unterstützt wird, verwenden Sie die [graphicscapturepicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) -Klasse, um die Benutzeroberfläche der Systemauswahl aufzurufen. Der Endbenutzer verwendet diese Benutzeroberfläche, um das Anzeige-oder Anwendungsfenster auszuwählen, dessen Bildschirmaufzeichnungen übernommen werden sollen. Die Auswahl gibt ein [graphicscaptureitem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem) -Element zurück, das verwendet wird, um eine **graphicscapturesession**zu erstellen:

```csharp
public async Task StartCaptureAsync()
{
    // The GraphicsCapturePicker follows the same pattern the
    // file pickers do.
    var picker = new GraphicsCapturePicker();
    GraphicsCaptureItem item = await picker.PickSingleItemAsync();

    // The item may be null if the user dismissed the
    // control without making a selection or hit Cancel.
    if (item != null)
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item);
    }
}
```

```vb
Public Async Function StartCaptureAsync() As Task
    ' The GraphicsCapturePicker follows the same pattern the
    ' file pickers do.
    Dim picker As New GraphicsCapturePicker
    Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

    ' The item may be null if the user dismissed the
    ' control without making a selection or hit Cancel.
    If item IsNot Nothing Then
        StartCaptureInternal(item)
    End If
End Function
```

Da es sich um einen UI-Code handelt, muss er im UI-Thread aufgerufen werden. Wenn Sie den Code aus dem Code-Behind für eine Seite Ihrer Anwendung aufrufen (wie z. b. **MainPage.XAML.cs**), erfolgt dies automatisch, wenn dies nicht der Fall ist, können Sie die Ausführung im UI-Thread mit folgendem Code erzwingen:

```csharp
CoreWindow window = CoreApplication.MainView.CoreWindow;

await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

```vb
Dim window As CoreWindow = CoreApplication.MainView.CoreWindow
Await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,
                                 Async Sub() Await StartCaptureAsync())
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Erstellen eines Aufzeichnungs Frame Pools und einer Erfassungs Sitzung

Mithilfe von **graphicscaptureitem**erstellen Sie ein [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) -Element mit Ihrem D3D-Gerät, unter unterstütztes Pixel Format (**DXGI- \_ Format \_ B8G8R8A8 \_ unorm**), Anzahl der gewünschten Frames (beliebige ganzzahlige Zahlen) und Frame Größe. Die **contentsize** -Eigenschaft der **graphicscaptureitem** -Klasse kann als Größe Ihres Frames verwendet werden:

```csharp
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item)
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, // D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
        2, // Number of frames
        _item.Size); // Size of the buffers
}
```

```vb
WithEvents CaptureItem As GraphicsCaptureItem
WithEvents FramePool As Direct3D11CaptureFramePool
Private _canvasDevice As CanvasDevice
Private _session As GraphicsCaptureSession

Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
    CaptureItem = item

    FramePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, ' D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
        2, '  Number of frames
        CaptureItem.Size) ' Size of the buffers
End Sub
```

Anschließend erhalten Sie eine Instanz der **graphicscapturesession** -Klasse für Ihre **Direct3D11CaptureFramePool** , indem Sie das **graphicscaptureitem** -Element an die Methode " **fiatecapturesession** " übergeben:

```csharp
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

Sobald der Benutzer explizit eine Zustimmung zum Erfassen eines Anwendungsfensters oder zum Anzeigen in der Benutzeroberfläche des Systems gegeben hat, kann das **graphicscaptureitem** mehreren **capturesession** -Objekten zugeordnet werden. Auf diese Weise kann Ihre Anwendung auswählen, dass das gleiche Element für verschiedene Umgebungen erfasst werden soll.

Um mehrere Elemente gleichzeitig zu erfassen, muss Ihre Anwendung eine Aufzeichnungs Sitzung für jedes zu erfassende Element erstellen. Dies erfordert, dass für jedes Element, das aufgezeichnet werden soll, die Auswahl Benutzeroberfläche aufgerufen wird.

## <a name="acquire-capture-frames"></a>Erfassungs Frames abrufen

Wenn Sie Ihren Frame-Pool und ihre Erfassungs Sitzung erstellt haben, können Sie die **startcapture** -Methode für Ihre **graphicscapturesession** -Instanz aufrufen, um das System zu benachrichtigen, damit Sie mit dem Senden von Aufzeichnungs Frames

```csharp
_session.StartCapture();
```

```vb
_session.StartCapture()
```

Zum Abrufen dieser Erfassungs Frames, die [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe) -Objekte sind, können Sie das **Direct3D11CaptureFramePool. framekam** -Ereignis verwenden:

```csharp
_framePool.FrameArrived += (s, a) =>
{
    // The FrameArrived event fires for every frame on the thread that
    // created the Direct3D11CaptureFramePool. This means we don't have to
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame())
    {
        // We'll define this method later in the document.
        ProcessFrame(frame);
    }  
};
```

```vb
Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
    ' The FrameArrived event is raised for every frame on the thread
    ' that created the Direct3D11CaptureFramePool. This means we
    ' don't have to do a null-check here, as we know we're the only
    ' one dequeueing frames in our application.  

    ' NOTE Disposing the frame retires it And returns  
    ' the buffer to the pool.

    Using frame = FramePool.TryGetNextFrame()
        ProcessFrame(frame)
    End Using
End Sub
```

Es wird empfohlen, den UI-Thread möglichst **nicht zu verwenden, da**dieses Ereignis jedes Mal ausgelöst wird, wenn ein neuer Frame verfügbar ist. Dies wird häufig vorkommen. Wenn Sie sich für die Überwachung von **frame't** im UI-Thread entscheiden, achten Sie darauf, wie viel Arbeit Sie bei jedem Auslösen des Ereignisses tun.

Alternativ können Sie Frames manuell mit der **Direct3D11CaptureFramePool. trygetnextframe** -Methode abrufen, bis Sie alle benötigten Frames erhalten.

Das **Direct3D11CaptureFrame** -Objekt enthält die Eigenschaften " **contentsize**", " **Surface**" und " **systemrelativetime**". **Systemrelativetime** ist QPC ([QueryPerformanceCounter](https://docs.microsoft.com/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)), der zum Synchronisieren anderer Medienelemente verwendet werden kann.

## <a name="process-capture-frames"></a>Prozess Erfassungs Frames

Jeder Frame aus der **Direct3D11CaptureFramePool** wird ausgecheckt, wenn **trygetnextframe**aufgerufen und entsprechend der Lebensdauer des **Direct3D11CaptureFrame** -Objekts wieder eingecheckt wird. Bei nativen Anwendungen reicht das Freigeben des **Direct3D11CaptureFrame** -Objekts aus, um den Frame wieder in den Frame Pool zu überprüfen. Bei verwalteten Anwendungen empfiehlt es sich, die **Direct3D11CaptureFrame.** verwerfen (**Close** in C++)-Methode zu verwenden. **Direct3D11CaptureFrame** implementiert die [iclosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable) -Schnittstelle, die als " [iverwerf"](https://docs.microsoft.com/dotnet/api/system.idisposable) für c#-Aufrufer projiziert wird.

Anwendungen sollten keine Verweise auf **Direct3D11CaptureFrame** -Objekte speichern und keine Verweise auf die zugrunde liegende Direct3D-Oberfläche speichern, nachdem der Frame wieder eingeglichen wurde.

Bei der Verarbeitung eines Frames empfiehlt es sich, dass Anwendungen die [ID3D11Multithread](https://docs.microsoft.com/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread) -Sperre auf demselben Gerät verwenden, das dem **Direct3D11CaptureFramePool** -Objekt zugeordnet ist.

Die zugrunde liegende Direct3D-Oberfläche ist immer die Größe, die beim Erstellen (oder Neuerstellen) des **Direct3D11CaptureFramePool**angegeben wird. Wenn der Inhalt größer als der Frame ist, wird der Inhalt auf die Größe des Frames zugeschnitten. Wenn der Inhalt kleiner als der Rahmen ist, enthält der Rest des Frames nicht definierte Daten. Es wird empfohlen, dass Anwendungen ein subrect mithilfe der **contentsize** -Eigenschaft für dieses **Direct3D11CaptureFrame** kopieren, damit nicht definierte Inhalte angezeigt werden.

## <a name="take-a-screenshot"></a>Screenshot erstellen

In unserem Beispiel konvertieren wir jede **Direct3D11CaptureFrame** in eine [canvasbitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm), die Teil der [Win2D-APIs](https://microsoft.github.io/Win2D/html/Introduction.htm)ist.

```csharp
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

Sobald wir über die **canvasbitmap**verfügen, können wir Sie als Bilddatei speichern. Im folgenden Beispiel speichern wir ihn als PNG-Datei im Ordner " **gespeicherte Bilder** " des Benutzers.

```csharp
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Reagieren auf das Ändern der Größe von Aufzeichnungs Elementen oder das Verlust des Geräts

Während des Erfassungs Vorgangs möchten Anwendungen möglicherweise Aspekte Ihrer **Direct3D11CaptureFramePool**ändern. Dies umfasst das Bereitstellen eines neuen Direct3D-Geräts, das Ändern der Größe der Frame Puffer oder sogar das Ändern der Anzahl der Puffer im Pool. In jedem dieser Szenarien ist die Methode zum erneuten **Erstellen** des **Direct3D11CaptureFramePool** -Objekts das empfohlene Tool.

Wenn "neu **Erstellen** " aufgerufen wird, werden alle vorhandenen Frames verworfen. Dadurch wird verhindert, dass Frames, deren zugrunde liegende Direct3D-Oberflächen zu einem Gerät gehören, auf das die Anwendung möglicherweise keinen Zugriff mehr hat. Aus diesem Grund kann es ratsam sein, alle ausstehenden Frames vor dem Aufrufen von neu **Erstellen**zu verarbeiten.

## <a name="putting-it-all-together"></a>Zusammenfügen des Gesamtbilds

Der folgende Code Ausschnitt ist ein End-to-End-Beispiel für das Implementieren der Bildschirmaufzeichnung in einer UWP-Anwendung. In diesem Beispiel haben wir zwei Schaltflächen im Front-End: eine ruft **Button_ClickAsync**auf, und die andere ruft **ScreenshotButton_ClickAsync**auf.

> [!NOTE]
> In diesem Code Ausschnitt wird [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)verwendet, eine Bibliothek für das 2D-Grafik Rendering. Informationen dazu, wie Sie für Ihr Projekt eingerichtet werden, finden Sie in der zugehörigen Dokumentation.

```csharp
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.Storage;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace ScreenCaptureTest
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;
        private CanvasBitmap _currentFrame;
        private string _screenshotFilename = "test.png";

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();

            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(
                Window.Current.Compositor,
                _canvasDevice);

            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with
                                                    // the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the
            // file pickers do.
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the
            // control without making a selection or hit Cancel.
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
               2, // Number of frames
               _item.Size); // Size of the buffers

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we
                // don't have to do a null-check here, as we know we're the only
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids
            // throwing in the catch block below (device creation could always
            // fail) along with ensuring that resize completes successfully and
            // isn’t vulnerable to device-lost.
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                _currentFrame = canvasBitmap;

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }

        private async void ScreenshotButton_ClickAsync(object sender, RoutedEventArgs e)
        {
            await SaveImageAsync(_screenshotFilename, _currentFrame);
        }

        private async Task SaveImageAsync(string filename, CanvasBitmap frame)
        {
            StorageFolder pictureFolder = KnownFolders.SavedPictures;

            StorageFile file = await pictureFolder.CreateFileAsync(
                filename,
                CreationCollisionOption.ReplaceExisting);

            using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
            {
                await frame.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
            }
        }
    }
}
```

```vb
Imports System.Numerics
Imports Microsoft.Graphics.Canvas
Imports Microsoft.Graphics.Canvas.UI.Composition
Imports Windows.Graphics
Imports Windows.Graphics.Capture
Imports Windows.Graphics.DirectX
Imports Windows.UI
Imports Windows.UI.Composition
Imports Windows.UI.Xaml.Hosting

Partial Public NotInheritable Class MainPage
    Inherits Page

    ' Capture API objects.
    WithEvents CaptureItem As GraphicsCaptureItem
    WithEvents FramePool As Direct3D11CaptureFramePool

    Private _lastSize As SizeInt32
    Private _session As GraphicsCaptureSession

    ' Non-API related members.
    Private _canvasDevice As CanvasDevice
    Private _compositionGraphicsDevice As CompositionGraphicsDevice
    Private _compositor As Compositor
    Private _surface As CompositionDrawingSurface

    Sub New()
        InitializeComponent()
        Setup()
    End Sub

    Private Sub Setup()
        _canvasDevice = New CanvasDevice()
        _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice)
        _compositor = Window.Current.Compositor
        _surface = _compositionGraphicsDevice.CreateDrawingSurface(
            New Size(400, 400), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied)
        Dim visual = _compositor.CreateSpriteVisual()
        visual.RelativeSizeAdjustment = Vector2.One
        Dim brush = _compositor.CreateSurfaceBrush(_surface)
        brush.HorizontalAlignmentRatio = 0.5F
        brush.VerticalAlignmentRatio = 0.5F
        brush.Stretch = CompositionStretch.Uniform
        visual.Brush = brush
        ElementCompositionPreview.SetElementChildVisual(Me, visual)
    End Sub

    Public Async Function StartCaptureAsync() As Task
        ' The GraphicsCapturePicker follows the same pattern the
        ' file pickers do.
        Dim picker As New GraphicsCapturePicker
        Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

        ' The item may be null if the user dismissed the
        ' control without making a selection or hit Cancel.
        If item IsNot Nothing Then
            StartCaptureInternal(item)
        End If
    End Function

    Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
        ' Stop the previous capture if we had one.
        StopCapture()

        CaptureItem = item
        _lastSize = CaptureItem.Size

        FramePool = Direct3D11CaptureFramePool.Create(
            _canvasDevice, ' D3D device
            DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
            2, '  Number of frames
            CaptureItem.Size) ' Size of the buffers

        _session = FramePool.CreateCaptureSession(CaptureItem)
        _session.StartCapture()
    End Sub

    Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
        ' The FrameArrived event is raised for every frame on the thread
        ' that created the Direct3D11CaptureFramePool. This means we
        ' don't have to do a null-check here, as we know we're the only
        ' one dequeueing frames in our application.  

        ' NOTE Disposing the frame retires it And returns  
        ' the buffer to the pool.

        Using frame = FramePool.TryGetNextFrame()
            ProcessFrame(frame)
        End Using
    End Sub

    Private Sub CaptureItem_Closed(sender As GraphicsCaptureItem, args As Object) Handles CaptureItem.Closed
        StopCapture()
    End Sub

    Public Sub StopCapture()
        _session?.Dispose()
        FramePool?.Dispose()
        CaptureItem = Nothing
        _session = Nothing
        FramePool = Nothing
    End Sub

    Private Sub ProcessFrame(frame As Direct3D11CaptureFrame)
        ' Resize and device-lost leverage the same function on the
        ' Direct3D11CaptureFramePool. Refactoring it this way avoids
        ' throwing in the catch block below (device creation could always
        ' fail) along with ensuring that resize completes successfully And
        ' isn't vulnerable to device-lost.

        Dim needsReset As Boolean = False
        Dim recreateDevice As Boolean = False

        If (frame.ContentSize.Width <> _lastSize.Width) OrElse
            (frame.ContentSize.Height <> _lastSize.Height) Then
            needsReset = True
            _lastSize = frame.ContentSize
        End If

        Try
            ' Take the D3D11 surface and draw it into a  
            ' Composition surface.

            ' Convert our D3D11 surface into a Win2D object.
            Dim bitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                _canvasDevice,
                frame.Surface)

            ' Helper that handles the drawing for us.
            FillSurfaceWithBitmap(bitmap)
            ' This is the device-lost convention for Win2D.
        Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
            ' We lost our graphics device. Recreate it and reset
            ' our Direct3D11CaptureFramePool.  
            needsReset = True
            recreateDevice = True
        End Try

        If needsReset Then
            ResetFramePool(frame.ContentSize, recreateDevice)
        End If
    End Sub

    Private Sub FillSurfaceWithBitmap(canvasBitmap As CanvasBitmap)
        CanvasComposition.Resize(_surface, canvasBitmap.Size)

        Using session = CanvasComposition.CreateDrawingSession(_surface)
            session.Clear(Colors.Transparent)
            session.DrawImage(canvasBitmap)
        End Using
    End Sub

    Private Sub ResetFramePool(size As SizeInt32, recreateDevice As Boolean)
        Do
            Try
                If recreateDevice Then
                    _canvasDevice = New CanvasDevice()
                End If
                FramePool.Recreate(_canvasDevice, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2, size)
                ' This is the device-lost convention for Win2D.
            Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
                _canvasDevice = Nothing
                recreateDevice = True
            End Try
        Loop While _canvasDevice Is Nothing
    End Sub

    Private Async Sub Button_ClickAsync(sender As Object, e As RoutedEventArgs) Handles CaptureButton.Click
        Await StartCaptureAsync()
    End Sub

End Class
```

## <a name="record-a-video"></a>Aufzeichnen eines Videos

Wenn Sie ein Video der Anwendung aufzeichnen möchten, können Sie dies mit dem [Windows. Media. apprecording-Namespace](https://docs.microsoft.com/uwp/api/windows.media.apprecording)vereinfachen. Diese Funktion ist Teil des Desktop Erweiterungs-SDKs, funktioniert daher nur auf dem Desktop und erfordert, dass Sie einen Verweis darauf aus dem Projekt hinzufügen. Weitere Informationen finden Sie unter [Übersicht über Gerätefamilien](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) .

## <a name="see-also"></a>Siehe auch

* [Windows. Graphics. Capture-Namespace](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
