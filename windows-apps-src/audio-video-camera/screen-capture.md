---
title: Bildschirmaufnahme
description: Der Windows.Graphics.Capture-Namespace enthält APIs zum Abrufen von Frames aus einer Anzeige oder einem Anwendungsfenster, um Videostreams zu erstellen oder gemeinsame und interaktive Benutzeroberflächen zu erstellen.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 6/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP, Bildschirmaufnahme
ms.localizationpriority: medium
ms.openlocfilehash: df9f1b536494d2ae7c765eb34a3867f3b2a9e269
ms.sourcegitcommit: 36e692ca19bebfb07ba611bb459db22b5bde650f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153266"
---
# <a name="screen-capture"></a>Bildschirmaufnahme

Ab Windows 10,Version 1803, enthält der [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) APIs zum Abrufen von Frames aus einer Anzeige oder einem Anwendungsfenster, um Videostreams zu erstellen oder gemeinsame und interaktive Benutzeroberflächen zu erstellen.

Mit der Bildschirmaufnahme können Entwickler sichere System-UIs für Endbenutzer aufrufen, um das Fenster für die Anzeige oder die Anwendung für die Aufzeichnung auszuwählen. Es wird ein gelber Rahmen mit einer Benachrichtigung vom System um das aktiv erfasste Element herum gezeichnet. Im Fall von mehreren gleichzeitigen Aufnahmesitzungen wird ein gelber Rahmen um jedes erfasste Element gezeichnet.

> [!NOTE]
> In der Bildschirmaufnahme APIs werden nur unterstützt, auf dem Desktop und Windows Mixed Reality immersive Headsets.

## <a name="add-the-screen-capture-capability"></a>Hinzufügen der Bildschirmaufnahmefunktion

Die APIs finden Sie in der **Windows.Graphics.Capture** Namespace erfordern eine allgemeine Funktion in Ihrem Anwendungsmanifest deklariert werden:

1. Open **"Package.appxmanifest"** in die **Projektmappen-Explorer**.
2. Wählen Sie die Registerkarte **Funktionen** aus.
3. Überprüfen Sie **Grafikerfassung**.

![Grafik erfassen](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Starten Sie die System-Benutzeroberfläche, um die Bildschirmaufnahme zu starten

Vor dem Starten der Systembenutzeroberfläche sollten Sie überprüfen, ob Ihre Anwendung momentan Bildschirmaufnahmen erstellen kann. Es gibt verschiedene Gründe, warum die Anwendung möglicherweise keine Bildschirmaufnahme erstellen kann, z. B. wenn das Gerät die Hardwareanforderungen nicht erfüllt, oder wenn die Anwendung für die Aufnahme Bildschirmaufnahmen blockiert. Verwenden Sie die **IsSupported**-Methode in der [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession)-Klasse, um zu bestimmen, ob UWP.Bildschirmaufnahmen unterstützt werden:

```cs
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

Nachdem Sie überprüft haben, dass Bildschirmaufnahmen unterstützt werden, verwenden Sie die [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker)-Klasse verwenden, um die System-Auswahl-UI aufzurufen. Der Benutzer verwendet die Benutzeroberfläche, um das Anzeige- oder Anwendungsfenster für die auszuführende Bildschirmaufnahmen auszuwählen. Die Auswahl gibt [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)zurück, das verwendet wird, um eine **GraphicsCaptureSession** zu erstellen:

```cs
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

Da dies die UI-Code ist, muss sie für den UI-Thread aufgerufen werden. Wenn Sie es im Code-Behind für eine Seite Ihrer Anwendung aufrufen (wie **"MainPage.Xaml.cs"** ) Dies ist automatisch für Sie ausgeführt, aber falls nicht, Sie können erzwingen, dass es für die Ausführung im UI-Thread durch den folgenden Code:

```cs
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

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Erstellen Sie ein Frameerfassungspool und eine Sammlungssitzung

Mithilfe der **GraphicsCaptureItem**, erstellen Sie eine [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) mit dem D3D-Gerät unterstützt Pixelformat (**DXGI\_FORMAT\_ B8G8R8A8\_UNORM**), die Anzahl der gewünschten Frames (die eine beliebige ganze Zahl sein können) und Rahmen Größe. Die **ContentSize**-Eigenschaft der **GraphicsCaptureItem**-Klasse kann ebenfalls als Anzahl der Frames verwendet werden:

```cs
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

Als Nächstes rufen Sie eine Instanz der **GraphicsCaptureSession**-Klasse für Ihren **Direct3D11CaptureFramePool** durch Übergabe der **GraphicsCaptureItem** auf der **CreateCaptureSession**-Methode auf:

```cs
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

Nachdem der Benutzer die Zustimmung für die Aufnahme eines Anwendungsfensters oder einer Anzeige in der System-Benutzeroberfläche erteilt hat, kann die **GraphicsCaptureItem** auf mehrere **CaptureSession**-Objekte angewendet werden. Auf diese Weise kann die Anwendung das gleiche Element für verschiedene Umgebungen erfassen.

Um mehrere Elemente gleichzeitig zu erfassen, muss die Anwendung eine Sammlungssitzung für jedes aufzuzeichnende Element erstellen, was das Aufrufen der Dateiauswahl-UI für jedes Element erfordert, das erfasst werden muss.

## <a name="acquire-capture-frames"></a>Erwerben von Aufnahmeframes

Rufen Sie nach der Erstellung des Frame-Pool und der Aufnahmesitzung die **StartCapture**-Methode für Ihre **GraphicsCaptureSession**-Instanz auf, die das System benachrichtigt, die Aufnahmeframes an Ihre App zu senden:

```cs
_session.StartCapture();
```

```vb
_session.StartCapture()
```

Um diese Aufnahmeframes zu erwerben, die [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe)-Objekte sind, können Sie das **Direct3D11CaptureFramePool.FrameArrived**-Ereignis verwenden:

```cs
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

Es wird empfohlen, den UI-Thread für **FrameArrived** nach Möglichkeit zu vermeiden, da dieses Ereignis jedes Mal ausgelöst wird, wenn ein neuer Frame verfügbar ist, was häufig vorkommt. Wenn Sie sich entscheiden, **FrameArrived** im UI-Thread zu übernehmen, überlegen Sie, wie viel Arbeit Sie jedes Mal machen, wenn das Ereignis ausgelöst wird.

Alternativ können Sie manuell Frames mit der **Direct3D11CaptureFramePool.TryGetNextFrame**-Methode ziehen, bis Sie alle Frames haben, die Sie benötigen.

Das **Direct3D11CaptureFrame**-Objekt enthält die Eigenschaften **ContentSize**, **Surface** und **SystemRelativeTime**. Die **SystemRelativeTime** ist die QPC ([QueryPerformanceCounter](https://docs.microsoft.com/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter))-Zeit, die zum Synchronisieren von anderen Medienelemente verwendet werden kann.

## <a name="process-capture-frames"></a>Frames für Prozess erfassen

Frames aus **Direct3D11CaptureFramePool** werden beim Aufrufen von **TryGetNextFrame** ausgecheckt, und gemäß der Lebensdauer des **Direct3D11CaptureFrame**-Objekts wieder eingecheckt. Für systemeigene Anwendungen reicht das Veröffentlichen des **Direct3D11CaptureFrame**-Objekts aus, um die Frame zurück in den Frame-Pool einzuchecken. Für verwaltete Anwendungen wird empfohlen, die **Direct3D11CaptureFrame.Dispose** (**Schließen** in C++)-Methode zu verwenden. **Direct3D11CaptureFrame** implementiert die [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable)-Schnittstelle, die als [IDisposable](https://docs.microsoft.com/dotnet/api/system.idisposable?redirectedfrom=MSDN) für C#-Aufrufer projiziert wird.

Anwendungen sollten weder Verweise auf **Direct3D11CaptureFrame**-Objekte speichern, noch Verweise auf die zugrunde liegende Direct3D-Oberfläche speichern, nachdem der Frame erneut eingecheckt wurde.

Während der Verarbeitung eines Frames, empfiehlt es sich, dass Anwendungen die [ID3D11Multithread](https://docs.microsoft.com/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread)-Sperre auf dem gleichen Gerät wie das **Direct3D11CaptureFramePool**-Objekt verwenden.

Die zugrunde liegende Direct3D-Oberfläche gibt immer die Größe an, die beim Erstellen (oder neuerstellen) von **Direct3D11CaptureFramePool** angegeben wird. Wenn der Inhalt größer als der Frame ist, werden die Inhalte auf die Größe des Frame zugeschnitten. Wenn der Inhalt kleiner als der Frames ist, enthält der Rest des Frames nicht definierte Daten. Es wird empfohlen, dass Anwendungen eine Sub-Rect mit der **ContentSize**-Eigenschaft für den **Direct3D11CaptureFrame** kopieren, um zu verhindern, dass der Inhalt nicht definiert ist.

## <a name="take-a-screenshot"></a>Erstellen Sie einen screenshot

In unserem Beispiel konvertieren wir jede **Direct3D11CaptureFrame** in einem [CanvasBitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm), diese ist Teil der [Win2D-APIs](https://microsoft.github.io/Win2D/html/Introduction.htm).

```cs
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

Sobald wir haben die **CanvasBitmap**, können wir ihn als Image-Datei speichern. Im folgenden Beispiel wir speichern es als PNG-Datei des Benutzers **gespeicherte Bilder** Ordner.

```cs
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Auf das Ändern der Größe von Elementen oder verlorene Geräte reagieren

Während der Aufnahme können Anwendungen Aspekte der ihrer **Direct3D11CaptureFramePool** ändern. Dies umfasst die Bereitstellung eines neuen Direct3D-Geräts, das Ändern der Größe der Framepuffer oder das Ändern der Anzahl der Puffer innerhalb des Pools. In jedem dieser Szenarien ist die **Recreate**-Methode für das **Direct3D11CaptureFramePool**-Objekt das empfohlene Tool.

Wenn **Recreate** aufgerufen wird, werden alle vorhandenen Frames verworfen. Dadurch wird die Ausgabe von Frames verhindert, deren zugrunde liegende Direct3D-Oberfläche zu einem Gerät gehören, auf das die Anwendung nicht mehr zugreifen kann. Aus diesem Grund sollten alle ausstehenden Frames vor dem Aufruf von **Recreate** verarbeitet werden.

## <a name="putting-it-all-together"></a>Zusammenfassung

Der folgende Codeausschnitt ist ein End-to-End-Beispiel zum Erfassen von Bildschirminhalten in einer UWP-Anwendung zu implementieren. In diesem Beispiel haben wir zwei Schaltflächen im Front-End: eine Aufrufe **Button_ClickAsync**, und die andere **ScreenshotButton_ClickAsync**.

> [!NOTE]
> Dieser Codeausschnitt verwendet [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm), eine Bibliothek für das Rendering von 2D-Grafiken. Informationen finden Sie ihre Dokumentation dazu, wie Sie es für Ihr Projekt einzurichten.

```cs
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

Wenn Sie ein Video zu Ihrer Anwendung aufzeichnen möchten, erreichen Sie so leichter mit dem [Windows.Media.AppRecording Namespace](https://docs.microsoft.com/uwp/api/windows.media.apprecording). Dies ist Teil des Desktop Erweiterungs-SDK, damit er nur auf Desktop funktioniert und erfordert, dass Sie einen Verweis darauf aus Ihrem Projekt hinzufügen. Finden Sie unter [Familien Geräteübersicht](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) für Weitere Informationen.

## <a name="see-also"></a>Siehe auch

* [Windows.Graphics.Capture-Namespace](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
