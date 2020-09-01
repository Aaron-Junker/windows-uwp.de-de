---
description: Erfahren Sie, wie Sie das x:defaultbindmode-Attribut in XAML-Markup verwenden, um einen anderen Standardmodus für x:Bind als "OneTime" anzugeben.
title: xdefaultbindmode-Attribut
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 817b89dc8f3ab7952cdbfc53489eb1afb12024f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166874"
---
# <a name="xdefaultbindmode-attribute"></a>x:DefaultBindMode-Attribut

Gibt in XAML-Markup einen Standardmodus für x:Bind.

**x:defaultbindmode** ist ab Windows 10, Version 1607 (Anniversary Update), SDK-Version 14393 verfügbar.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Hinweise

[x:Bind](x-bind-markup-extension.md) hat den Standardmodus " **OneTime**". Dies wurde aus Leistungsgründen gewählt, da die Verwendung von **OneWay** bewirkt, dass mehr Code generiert und die Änderungs Erkennung verarbeitet wird. Sie können **x:defaultbindmode** verwenden, um den Standardmodus für x:Bind für ein bestimmtes Segment der Markup Struktur zu ändern. Der angegebene Modus gilt für alle x:Bind-Ausdrücke auf diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Modus als Teil der Bindung angeben.

## <a name="related-topics"></a>Zugehörige Themen

* [x:Bind-Markup Erweiterung](x-bind-markup-extension.md)
