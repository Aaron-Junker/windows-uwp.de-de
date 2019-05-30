---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Visuelle Kompositionselemente
description: Visuelle Kompositionselemente bilden die visuelle Struktur, die die Grundlage für alle anderen Features der Composition-API bildet und von diesen verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d6d150f2f882348bffb36dd2918f0f61ea1586c7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360486"
---
# <a name="composition-visual"></a>Visuelle Kompositionselemente

Visuelle Kompositionselemente bilden die visuelle Struktur, die die Grundlage für alle anderen Features der Composition-API bildet und von diesen verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.

## <a name="visuals"></a>Visuelle Elemente

Es gibt drei Arten visueller Elemente, aus denen sich die visuelle Struktur zusammensetzt, sowie eine grundlegende Pinselklasse mit mehreren Unterklassen, die Einfluss auf den Inhalt eines visuellen Elements hat:

- [**Visual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) – Basis Objekts, die meisten Eigenschaften sind hier und von den anderen visuellen Objekten geerbt.
- [**ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) – leitet sich von [ **Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual), und fügt die Möglichkeit zum Erstellen von untergeordneten Elementen hinzu.
- [**SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) – leitet sich von [ **ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) und fügt die Fähigkeit zum Zuordnen ein Pinsels, damit das visuelle Element Pixel, die Bilder, einschließlich Rendern kann Effekte oder einen soliden hinzu. Farbe.

Sie können Inhalte und Effekte für SpriteVisuals mit [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) und deren Unterklassen, wie [**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush),[**CompositionSurfaceBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) und [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush), festlegen. Weitere Informationen zu Pinseln finden Sie unter [**CompositionBrush-Überblick**](https://docs.microsoft.com/windows/uwp/composition/composition-brushes).

## <a name="the-compositionvisual-sample"></a>Das CompositionVisual-Beispiel

Hier betrachten wir einige Codebeispiele für die zuvor aufgeführten drei Arten visueller Elemente. Dieses Beispiel veranschaulicht keine Konzepte wie Animationen oder komplexere Effekte. Es enthält die Bausteine, die alle diese Systeme verwenden. (Den vollständigen Beispielcode finden Sie am Ende dieses Artikels.)

Im Beispiel sehen Sie eine Reihe farbiger Quadrate, die Sie anklicken und über den Bildschirm ziehen können. Durch Klicken auf ein Quadrat gelangt dieses in den Vordergrund, dreht sich um 45 Grad und wird während der Bewegung undurchsichtig.

Es zeigt einige grundlegenden Konzepte für die Arbeit mit der API, einschließlich:

- Erstellen eines Kompositors
- Erstellen eines SpriteVisual-Elements mit „CompositionColorBrush“
- Beschneiden des visuellen Elements
- Drehen des visuellen Elements
- Festlegen der Deckkraft
- Ändern der Position des visuellen Elements in der Auflistung.

## <a name="creating-a-compositor"></a>Erstellen eines Kompositors

Es ist einfach, ein neues [**Compositor**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Compositor)-Objekt zu erstellen und diese für die Verwendung als Factory in einer Variable zu speichern. Der folgende Codeausschnitt veranschaulicht das Erstellen eines neuen **Compositor**-Objekts:

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Erstellen eines „SpriteVisual“- und eines „ColorBrush“-Elements

Mit dem [**Compositor**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Compositor)-Objekt können Sie auf einfache Weise Objekte erstellen, wenn Sie sie benötigen, wie etwa ein [**SpriteVisual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual)- und ein [**CompositionColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)-Element:

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Obwohl dies nur ein paar Codezeilen erforderlich ist, wird es ein leistungsstarkes Konzept veranschaulicht: [**SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) Objekte sind das Herzstück des Systems Auswirkungen. Das **SpriteVisual**-Element ermöglicht hohe Flexibilität und Interaktion bei der Farb-, Bild- und Effektgestaltung. **SpriteVisual** ist das einzige visuelle Element, das ein 2D-Rechteck mit einem Pinsel füllen kann, in diesem Fall mit einer Volltonfarbe.

## <a name="clipping-a-visual"></a>Beschneiden eines visuellen Elements

[  **Compositor**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Compositor) kann auch zum Beschneiden eines [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)-Objekts verwendet werden. Im folgenden Beispiel werden mit [**InsetClip**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.InsetClip) die Seiten des visuellen Elements gekürzt:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Auf die Eigenschaften von [**InsetClip**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.InsetClip) können wie auch auf andere Objekte in der API Animationen angewendet werden.

## <a name="span-idrotatingaclipspanspan-idrotatingaclipspanspan-idrotatingaclipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Drehen einen Clip

Ein [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)-Objekt kann mit einer Drehung transformiert werden. Beachten Sie, dass [**RotationAngle**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visual.rotationangle) Radianten und Grad unterstützt. Der Standardwert ist „Radianten“. Wie im folgenden Codeausschnitt dargestellt, ist es jedoch ganz einfach, einen Wert in Grad anzugeben:

```cs
child.RotationAngleInDegrees = 45.0f;
```

Drehung ist nur ein Beispiel für eine Reihe von Transformationskomponenten, die von der API bereitgestellt werden, um diese Aufgaben zu vereinfachen. Dazu gehören auch Offset, Skalieren, Ausrichtung, Drehachse und eine 4x4-Transformationsmatrix.

## <a name="setting-opacity"></a>Festlegen der Deckkraft

Das Festlegen der Deckkraft eines visuellen Elements ist mit einem Float-Wert unproblematisch. In diesem Beispiel haben alle Quadrate anfangs eine Deckkraft von 0,8:

```cs
visual.Opacity = 0.8f;
```

Wie die Drehung kann auch die [**Opacity**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visual.opacity)-Eigenschaft animiert werden.

## <a name="changing-the-visuals-position-in-the-collection"></a>Ändern der Position des visuellen Elements in der Auflistung

Die Composition-API ermöglicht es, dass die Position eines visuellen Elements in [**VisualCollection**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visualcollection) auf vielfältige Weise geändert werden kann. Es kann mit [**InsertAbove**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visualcollection.insertabove) über einem anderen visuellen Element und mit [**InsertBelow**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visualcollection.insertbelow) darunter platziert werden. An die oberste Position wird es mit [**InsertAtTop**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visualcollection.insertattop) und an die unterste mit [**InsertAtBottom**](https://docs.microsoft.com/uwp/api/windows.ui.composition.visualcollection.insertatbottom) verschoben.

In dem Beispiel wird ein [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)-Objekt, auf das geklickt wurde, oben platziert:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Vollständiges Beispiel

Im vollständigen Beispiel werden alle oben beschriebenen Konzepte zusammen verwendet, um eine einfache Struktur von [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual)-Objekten zu erstellen und vorzuführen. Die Deckkraft wird ohne Verwendung von XAML, WWA oder DirectX geändert. Dieses Beispiel zeigt, wie untergeordnete **Visual**-Objekte erstellt und hinzugefügt und Eigenschaften geändert werden.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```
