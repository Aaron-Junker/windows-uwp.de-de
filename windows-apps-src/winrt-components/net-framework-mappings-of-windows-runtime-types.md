---
author: msatranjr
title: .NET Framework-Zuordnungen von Windows-Runtime-Typen
description: Die folgende Tabelle enthält die Zuordnungen, die das .NET Framework zwischen UWP-Typen (Universelle Windows-Plattform) und .NET Framework-Typen vornimmt.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f1869038ad98b8b4103b9706534a2d456f17e734
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "6197850"
---
# <a name="net-framework-mappings-of-windows-runtime-types"></a>.NET Framework-Zuordnungen von Windows-Runtime-Typen



Die folgende Tabelle enthält die Zuordnungen, die das .NET Framework zwischen UWP-Typen (Universelle Windows-Plattform) und .NET Framework-Typen vornimmt. In einer universellen Windows-App, die in verwaltetem Code geschrieben wurde, zeigt IntelliSense den .NET Framework-Typ anstelle des UWP-Typs an. Wenn beispielsweise eine Windows-Runtime Methode einen Parameter vom Typ IVector&lt;string&gt; übernimmt, zeigt IntelliSense einen Parameter vom Typ IList&lt;string&gt; an. Entsprechend verwenden Sie in einer Komponente für Windows-Runtime, die in verwaltetem Code geschrieben wurde, den .NET Framework-Typ in Membersignaturen. Wenn das [Windows-Runtime-Metadatenexport-Tool (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) die Komponente für Windows-Runtime generiert, wird der .NET Framework-Typ zum entsprechenden UWP-Typ.

## <a name="mapping-tables"></a>Zuordnungstabellen


Die meisten Typen mit demselben Namespace- und Typnamen in der UWP und dem .NET Framework sind Strukturen (oder Typen, die Strukturen zugeordnet sind, z.B. Aufzählungen bzw. Enumerationen). Strukturen verfügen in der UWP nur über Felder und benötigen Hilfstypen, die das .NET Framework ausblendet. Die .NET Framework-Versionen dieser Strukturen haben Eigenschaften und Methoden, die die Funktionalität der ausgeblendeten Hilfstypen bereitstellen.

Tabelle1: UWP-Typen, die .NET Framework-Typen mit einem anderen Namen und/oder Namespace zugeordnet sind.

| UWP-Typ/Namespace                                            | .NET Framework-Typ/Namespace                                          | .NET Framework-Assembly                           |
|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------|
| AttributeUsageAttribute (Windows.Foundation.Metadata)         | AttributeUsageAttribute (System)                                       | System.Runtime.dll                                |
| AttributeTargets (Windows.Foundation.Metadata)                | AttributeTargets (System)                                              | System.Runtime.dll                                |
| DateTime (Windows.Foundation)                                 | DateTimeOffset (System)                                                | System.Runtime.dll                                |
| EventHandler&lt;T&gt; (Windows.Foundation)                    | EventHandler&lt;T&gt; (System)                                         | System.Runtime.dll                                |
| EventRegistrationToken (Windows.Foundation)                   | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) | System.Runtime.InteropServices.WindowsRuntime.dll |
| HResult (Windows.Foundation)                                  | Exception (System)                                                     | System.Runtime.dll                                |
| IReference&lt;T&gt; (Windows.Foundation)                      | Nullable&lt;T&gt; (System)                                             | System.Runtime.dll                                |
| TimeSpan (Windows.Foundation)                                 | TimeSpan (System)                                                      | System.Runtime.dll                                |
| Uri (Windows.Foundation)                                      | Uri (System)                                                           | System.Runtime.dll                                |
| IClosable (Windows.Foundation)                                | IDisposable (System)                                                   | System.Runtime.dll                                |
| IIterable&lt;T&gt; (Windows.Foundation.Collections)           | IEnumerable&lt;T&gt; (System.Collections.Generic)                      | System.Runtime.dll                                |
| IVector&lt;T&gt; (Windows.Foundation.Collections)             | IList&lt;T&gt; (System.Collections.Generic)                            | System.Runtime.dll                                |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections)         | IReadOnlyList&lt;T&gt; (System.Collections.Generic)                    | System.Runtime.dll                                |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections)              | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)            | System.Runtime.dll                                |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections)          | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)    | System.Runtime.dll                                |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections)     | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic)           | System.Runtime.dll                                |
| IBindableIterable (Windows.UI.Xaml.Interop)                   | IEnumerable (System.Collections)                                       | System.Runtime.dll                                |
| IBindableVector (Windows.UI.Xaml.Interop)                     | IList (System.Collections)                                             | System.Runtime.dll                                |
| INotifyCollectionChanged (Windows.UI.Xaml.Interop)            | INotifyCollectionChanged (System.Collections.Specialized)              | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized)   | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop)    | NotifyCollectionChangedEventArgs (System.Collections.Specialized)      | System.ObjectModel.dll                            |
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop)       | NotifyCollectionChangedAction (System.Collections.Specialized)         | System.ObjectModel.dll                            |
| INotifyPropertyChanged (Windows.UI.Xaml.Data)                 | INotifyPropertyChanged (System.ComponentModel)                         | System.ObjectModel.dll                            |
| PropertyChangedEventHandler (Windows.UI.Xaml.Data)            | PropertyChangedEventHandler (System.ComponentModel)                    | System.ObjectModel.dll                            |
| PropertyChangedEventArgs (Windows.UI.Xaml.Data)               | PropertyChangedEventArgs (System.ComponentModel)                       | System.ObjectModel.dll                            |
| TypeName (Windows.UI.Xaml.Interop)                            | Type (System)                                                          | System.Runtime.dll                                |

 

Tabelle2: UWP-Typen, die .NET Framework-Typen mit demselben Namen und Namespace zugeordnet sind.

| Namespace                           | Typ               | .NET Framework-Assembly                   |
|-------------------------------------|--------------------|-------------------------------------------|
| Windows.UI                          | Color              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Point              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Rect               | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Size               | System.Runtime.WindowsRuntime.dll         |
| Windows.UI.Xaml.Input               | ICommand           | System.ObjectModel.dll                    |
| Windows.UI.Xaml                     | CornerRadius       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Duration           | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | DurationTyp       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridLength         | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridUnitType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Thickness          | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition  | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media               | Matrix             | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | KeyTime            | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehavior     | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehaviorTyp | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Media3D       | Matrix3D           | System.Runtime.WindowsRuntime.UI.Xaml.dll |

 

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
