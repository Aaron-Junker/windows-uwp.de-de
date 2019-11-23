---
description: Gibt im XAML-Markup einen NULL-Wert für eine Eigenschaft an.
title: xNull-Markuperweiterung
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d6ee1f4cb1fa0a97c8d1b8a447ccc15cd0b51776
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340454"
---
# <a name="xnull-markup-extension"></a>{x:Null}-Markuperweiterung


Gibt im XAML-Markup einen **NULL**-Wert für eine Eigenschaft an.

## <a name="xaml-attribute-usage"></a>XAML-Attributverwendung

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Hinweise

**null** ist das Nullverweis-Schlüsselwort für C# und C++. Das Microsoft Visual Basic-Schlüsselwort für einen Nullverweis lautet **Nothing**.

Der anfängliche Standardwert kann zwischen Abhängigkeitseigenschaften variieren und ist nicht unbedingt **null**. Viele Abhängigkeitseigenschaften akzeptieren **null** außerdem aufgrund ihrer internen Implementierung nicht als Wert (weder per Markup noch per Code). In einem solchen Fall tritt unter Umständen eine Analyseausnahme auf, wenn ein XAML-Attributwert mit **{x:Null}** festgelegt wird.

Einige Windows-Runtime-Typen akzeptieren NULL-Werte. Sollte bei einem Typ, der NULL-Werte akzeptiert, **null** nicht bereits als Standardwert festgelegt sein, können Sie **{x:Null}** in XAML verwenden, um den **NULL**-Wert festzulegen. Wenn Sie Visual C++ Component Extensions (C++/CX) verwenden, werden Werte zulässt-Typen als [**Platform:: iBox-<T>** ](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface)dargestellt. In Microsoft .NET-Sprachen werden Typen, die NULL-Werte akzeptieren, mit [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1) angegeben.

## <a name="related-topics"></a>Verwandte Themen

* [**NULL-Werte zulassen<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1)
* [**IReference-<T>** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

