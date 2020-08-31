---
description: Erfahren Sie, wie Sie die x:NULL-Markup Erweiterung in XAML-Markup verwenden können, um einen NULL-Wert für eine Eigenschaft anzugeben.
title: xNull-Markuperweiterung
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 46256a5456583f9e434f76a2d06bc552c2cb0d3f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169034"
---
# <a name="xnull-markup-extension"></a>{x:Null}-Markuperweiterung


Gibt im XAML-Markup einen **NULL**-Wert für eine Eigenschaft an.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Bemerkungen

**null** ist das Nullverweis-Schlüsselwort für C# und C++. Das Microsoft Visual Basic-Schlüsselwort für einen Nullverweis lautet **Nothing**.

Der anfängliche Standardwert kann zwischen Abhängigkeitseigenschaften variieren und ist nicht unbedingt **null**. Viele Abhängigkeitseigenschaften akzeptieren **null** außerdem aufgrund ihrer internen Implementierung nicht als Wert (weder per Markup noch per Code). In einem solchen Fall tritt unter Umständen eine Analyseausnahme auf, wenn ein XAML-Attributwert mit **{x:Null}** festgelegt wird.

Einige Windows-Runtime-Typen akzeptieren NULL-Werte. Sollte bei einem Typ, der NULL-Werte akzeptiert, **null** nicht bereits als Standardwert festgelegt sein, können Sie **{x:Null}** in XAML verwenden, um den **NULL**-Wert festzulegen. Bei Verwendung von Visual C++-Komponentenerweiterungen (C++/CX) werden Typen, die NULL-Werte akzeptieren, als [**Platform::IBox<T>**](/cpp/cppcx/platform-ibox-interface) dargestellt. In Microsoft .NET-Sprachen werden Typen, die NULL-Werte akzeptieren, mit [**Nullable<T>**](/dotnet/api/system.nullable-1) angegeben.

## <a name="related-topics"></a>Zugehörige Themen

* [**Werte zulässt<T>**](/dotnet/api/system.nullable-1)
* [**IReference<T>**](/uwp/api/Windows.Foundation.IReference_T_)
 