---
description: Ändert das XAML-Kompilierungsverhalten, sodass Felder für Verweise auf benannte Objekte mit öffentlichem Zugriff definiert werden und nicht das private Standardverhalten aufweisen.
title: xFieldModifier-Attribut
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660915"
---
# <a name="xfieldmodifier-attribute"></a>x:FieldModifier-Attribut


Ändert das XAML-Kompilierungsverhalten, sodass Felder für Verweise auf benannte Objekte mit **öffentlichem** Zugriff definiert werden und nicht das **private** Standardverhalten aufweisen.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Abhängigkeiten

[x:Name-Attribut](x-name-attribute.md) muss in demselben Element ebenfalls bereitgestellt werden.

## <a name="remarks"></a>Hinweise

Der Wert für das **x:FieldModifier**-Attribut variiert je nach Programmiersprache. Gültige Werte sind **private**, **public**, **protected**, **internal** oder **friend**. Für C#, Microsoft Visual Basic oder Visual C++-komponentenerweiterungen (C++ / CX), erhalten Sie die Zeichenfolge-Wert "public" oder "Public"; der Parser keine Groß-und Kleinschreibung bei den Wert dieses Attributs zu erzwingen.

**Private**-Zugriff ist die Standardeinstellung.

**x:FieldModifier** ist nur für Elemente mit einem [x:Name-Attribut](x-name-attribute.md) relevant, weil unter diesem Namen auf das Feld verwiesen wird, nachdem dieses den Zustand „public“ erreicht hat.

**Beachten Sie**  XAML für Windows-Runtime unterstützt keine **X: ClassModifier-** oder **X: Subclass-**.

