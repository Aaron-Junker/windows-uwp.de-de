---
title: .Net-Zuordnungen von Windows-Runtime Typen
description: In der folgenden Tabelle sind die Zuordnungen aufgeführt, die von .net zwischen den universelle Windows-Plattform Typen (UWP) und .NET-Typen erstellt werden.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393660"
---
# <a name="net-mappings-of-windows-runtime-types"></a>.Net-Zuordnungen von Windows-Runtime Typen

In der folgenden Tabelle sind die Zuordnungen aufgeführt, die von .net zwischen den universelle Windows-Plattform Typen (UWP) und .NET-Typen erstellt werden. In einer universellen Windows-APP, die mit verwaltetem Code geschrieben wurde, zeigt Visual Studio IntelliSense den .NET-Typ anstelle des UWP-Typs an. Wenn eine Windows-Runtime Methode z. b. einen Parameter des Typs IVector&lt;String&gt;annimmt, zeigt IntelliSense einen Parameter vom Typ IList&lt;String&gt;an. Analog dazu verwenden Sie in einer Windows-Runtime-Komponente, die mit verwaltetem Code geschrieben wurde, den .NET-Typ in Element Signaturen. Wenn das [Windows-Runtime Metadata-Export Tool (winmdexp. exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) Ihre Windows-Runtime Komponente generiert, wird der .NET-Typ in den entsprechenden UWP-Typ übersetzt.

Die meisten der Typen, die in UWP und .net denselben Namespace Namen und Typnamen aufweisen, sind Strukturen (oder Typen, die Strukturen zugeordnet sind, z. b. Enumerationen). In UWP haben Strukturen keine anderen Member als Felder und benötigen Hilfstypen, die von .net ausgeblendet werden. Die .NET-Versionen dieser Strukturen verfügen über Eigenschaften und Methoden, die die Funktionalität der ausgeblendeten Hilfstypen bereitstellen.

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>UWP-Typen, die .NET-Typen mit demselben Namen und Namespace zugeordnet sind

### <a name="in-net-assembly-systemobjectmodeldll"></a>In der .NET-Assembly System. ObjectModel. dll

| Namespace | Typ |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>In der .NET-Assembly System. Runtime. windowsruntime. dll

| Namespace | Typ |
|-|-|
| Windows.Foundation | Punkt |
| Windows.Foundation | Rect |
| Windows.Foundation | Größe |
| Windows.UI | Farbe |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>In der .NET-Assembly System. Runtime. windowsruntime. UI. XAML. dll

| Namespace | Typ |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Dauer |
| Windows.UI.Xaml | DurationTyp |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Stärke |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | Matrix |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorTyp |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>UWP-Typen, die .NET-Typen mit einem anderen Namen und/oder einem anderen Namespace zugeordnet sind

### <a name="in-net-assembly-systemobjectmodeldll"></a>In der .NET-Assembly System. ObjectModel. dll

| UWP-Typ/Namespace | .NET-Typ/-Namespace |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>In der .NET-Assembly System. Runtime. dll

| UWP-Typ/Namespace | .NET-Typ/-Namespace |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>In der .NET-Assembly System. Runtime. InteropServices. windowsruntime. dll

| UWP-Typ/Namespace | .NET-Typ/-Namespace |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>Verwandte Themen

* [Windows-Runtime Komponenten mit C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)