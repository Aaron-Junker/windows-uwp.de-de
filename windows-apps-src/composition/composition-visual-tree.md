---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Visuelle Kompositionselemente
description: Visuelle Kompositionselemente bilden die visuelle Struktur, die die Grundlage für alle anderen Features der Composition-API bildet und von diesen verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a488126de73fccfd8a783ddde98b4245b46ced39
ms.sourcegitcommit: 651a6b9769fad1736ab16e2a4e423258889b248e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91366866"
---
# <a name="composition-visual"></a>Visuelle Kompositionselemente

Visuelle Kompositionselemente bilden die visuelle Struktur, die die Grundlage für alle anderen Features der Composition-API bildet und von diesen verwendet wird. Die API ermöglicht es Entwicklern, visuelle Objekte zu definieren und zu erstellen, die jeweils für einen einzelnen Knoten in einer visuellen Struktur stehen.

## <a name="visuals"></a>Visuals

Es gibt mehrere visuelle Typen, aus denen die visuelle Struktur besteht, sowie eine Basis Pinsel Klasse mit mehreren Unterklassen, die sich auf den Inhalt eines visuellen Elements auswirken:

- [**Visual**](/uwp/api/Windows.UI.Composition.Visual) – Basisobjekt; umfasst den Großteil der Eigenschaften. Diese werden von anderen visuellen Objekten geerbt.
- [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual): abgeleitet von [**Visual**](/uwp/api/Windows.UI.Composition.Visual). Fügt die Fähigkeit zum Erstellen von untergeordneten Elementen hinzu.
  - [**Spritevisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – wird von [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)abgeleitet. Bietet die Möglichkeit, einen Pinsel zuzuordnen, sodass die Visualisierung Pixel, einschließlich Bildern, Effekten oder einer voll Tonfarbe, Renderern kann.
  - [**Layervisual**](/uwp/api/Windows.UI.Composition.LayerVisual) – wird von [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)abgeleitet. Untergeordnete Elemente des visuellen Elements werden zu einer einzelnen Ebene vereinfacht.<br/>(_Eingeführt in Windows 10, Version 1607, SDK 14393._)
  - [**Shapevisual**](/uwp/api/Windows.UI.Composition.ShapeVisual) – wird von [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)abgeleitet. Ein visueller Struktur Knoten, der der Stamm einer compositionshape ist.<br/>(_Eingeführt in Windows 10, Version 1803, SDK 17134._)
  - [**Redirectvisual**](/uwp/api/Windows.UI.Composition.RedirectVisual) – wird von [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)abgeleitet. Der Inhalt wird von der Visualisierung von einem anderen visuellen Element abgerufen.<br/>(_Eingeführt in Windows 10, Version 1809, SDK 17763._)
  - [**Scenevisual**](/uwp/api/windows.ui.composition.scenes.scenevisual) – wird von [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)abgeleitet. Eine Container Visualisierung für die Knoten einer 3D-Szene.<br/>(_Eingeführt in Windows 10, Version 1903, SDK 18362._)

Sie können mit dem [**compositionbrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) und seinen Unterklassen, einschließlich [**compositioncolorbrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**compositionsurfacebrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) und [**compositioneffectbrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush), Inhalte und Effekte auf spritevisuals anwenden. Weitere Informationen zu Pinsel finden Sie unter [**Übersicht über compositionbrush**](./composition-brushes.md).

## <a name="the-compositionvisual-sample"></a>Das CompositionVisual-Beispiel

Hier sehen Sie einen Beispielcode, der die drei verschiedenen visuellen Typen veranschaulicht, die zuvor aufgelistet wurden. Obwohl in diesem Beispiel keine Konzepte wie Animationen oder komplexere Effekte behandelt werden, sind die Bausteine enthalten, die von all diesen Systemen verwendet werden. (Der vollständige Beispielcode ist am Ende dieses Artikels aufgeführt.)

Im Beispiel gibt es eine Reihe von voll Tonfarbe-Quadraten, die auf den Bildschirm geklickt und gezogen werden können. Durch Klicken auf ein Quadrat gelangt dieses in den Vordergrund, dreht sich um 45 Grad und wird während der Bewegung undurchsichtig.

Es zeigt einige grundlegenden Konzepte für die Arbeit mit der API, einschließlich:

- Erstellen eines Kompositors
- Erstellen eines spritevisual mit einem compositioncolorbrush
- Clipping des visuellen Elements
- Drehen des visuellen Elements
- Festlegen der Deckkraft
- Ändern der Position des visuellen Elements in der Auflistung.

## <a name="creating-a-compositor"></a>Erstellen eines Kompositors

Das Erstellen eines [**Compositors**](/uwp/api/Windows.UI.Composition.Compositor) und das Speichern in einer Variablen für die Verwendung als Factory ist eine einfache Aufgabe. Der folgende Codeausschnitt veranschaulicht das Erstellen eines neuen **Compositor**-Objekts:

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Erstellen eines „SpriteVisual“- und eines „ColorBrush“-Elements

Mit dem [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor)-Objekt können Sie auf einfache Weise Objekte erstellen, wenn Sie sie benötigen, wie etwa ein [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual)- und ein [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush)-Element:

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Obwohl dies nur ein paar Codezeilen ist, wird ein leistungsfähiges Konzept veranschaulicht: [**spritevisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) -Objekte sind das Herzstück des Effects-Systems. Das **SpriteVisual**-Element ermöglicht hohe Flexibilität und Interaktion bei der Farb-, Bild- und Effektgestaltung. **Spritevisual** ist ein einzelner visueller Typ, der ein 2D-Rechteck mit einem Pinsel ausfüllen kann, in diesem Fall eine voll Tonfarbe.

## <a name="clipping-a-visual"></a>Beschneiden eines visuellen Elements

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) kann auch zum Beschneiden eines [**Visual**](/uwp/api/Windows.UI.Composition.Visual)-Objekts verwendet werden. Im folgenden Beispiel werden mit [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) die Seiten des visuellen Elements gekürzt:

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

Wie bei anderen Objekten in der API kann " [**insetclip**](/uwp/api/Windows.UI.Composition.InsetClip) " Animationen auf seine Eigenschaften anwenden.

## <a name="span-idrotating_a_clipspanspan-idrotating_a_clipspanspan-idrotating_a_clipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Drehen von Clips

Ein [**Visual**](/uwp/api/Windows.UI.Composition.Visual)-Objekt kann mit einer Drehung transformiert werden. Beachten Sie, dass [**RotationAngle**](/uwp/api/windows.ui.composition.visual.rotationangle) Radianten und Grad unterstützt. Standardmäßig wird das Bogenmaß verwendet, aber es ist leicht, Grad festzulegen, wie im folgenden Code Ausschnitt gezeigt:

```cs
child.RotationAngleInDegrees = 45.0f;
```

Drehung ist nur ein Beispiel für eine Reihe von Transformationskomponenten, die von der API bereitgestellt werden, um diese Aufgaben zu vereinfachen. Zu den anderen gehören Offset, Scale, Orientation, RotationAxis und 4X4 transformMatrix.

## <a name="setting-opacity"></a>Festlegen der Deckkraft

Das Festlegen der Deckkraft eines visuellen Elements ist mit einem Float-Wert unproblematisch. In diesem Beispiel haben alle Quadrate anfangs eine Deckkraft von 0,8:

```cs
visual.Opacity = 0.8f;
```

Wie die Drehung kann auch die [**Opacity**](/uwp/api/windows.ui.composition.visual.opacity)-Eigenschaft animiert werden.

## <a name="changing-the-visuals-position-in-the-collection"></a>Ändern der Position des visuellen Elements in der Auflistung

Die Kompositions-API ermöglicht das Ändern der Position eines visuellen Elements in einer [**VisualCollection**](/uwp/api/windows.ui.composition.visualcollection) auf verschiedene Weise. Es kann über einem anderen visuellen Element mit [**insertabove**](/uwp/api/windows.ui.composition.visualcollection.insertabove)platziert werden, das nach unten mit [**insertattop**](/uwp/api/windows.ui.composition.visualcollection.insertbelow)platziert wird [**InsertAtTop**](/uwp/api/windows.ui.composition.visualcollection.insertattop), oder das Ende mit [**insertatbottom**](/uwp/api/windows.ui.composition.visualcollection.insertatbottom).

Im Beispiel wird eine [**Visualisierung**](/uwp/api/Windows.UI.Composition.Visual) , auf die geklickt wurde, nach oben sortiert:

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Vollständiges Beispiel

Im vollständigen Beispiel werden alle oben beschriebenen Konzepte zusammen verwendet, um eine einfache Struktur von [**Visual**](/uwp/api/Windows.UI.Composition.Visual)-Objekten zu erstellen und vorzuführen. Die Deckkraft wird ohne Verwendung von XAML, WWA oder DirectX geändert. Dieses Beispiel zeigt, wie untergeordnete **Visual**-Objekte erstellt und hinzugefügt und Eigenschaften geändert werden.

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
