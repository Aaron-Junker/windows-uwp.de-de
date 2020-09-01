---
title: So deaktivieren Sie die Skalierung
description: Erfahren Sie, wie Sie den standardmäßigen Skalierungsfaktor deaktivieren und bewirken, dass Ihre Anwendung die tatsächlichen 1910 x 1080 Pixel-Geräte Dimensionen verwendet.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 404bdd9a4b25254c1941928dbfb0b548492f03a5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174704"
---
# <a name="how-to-turn-off-scaling"></a>So deaktivieren Sie die Skalierung   
Standardmäßig werden Anwendungen für XAML-Apps auf 200 Prozent und für HTML-Apps auf 150 Prozent skaliert. Der Standardskalierungsfaktor kann deaktiviert werden. Infolgedessen verwendet die Anwendung die tatsächlichen Pixelabmessungen des Geräts (1910 x 1080 Pixel).   
   
## <a name="html"></a>HTML   
Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Oder Sie können eine für das Web geeignete Methode verwenden:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Sie können den Skalierungsfaktor deaktivieren, indem Sie den folgenden Codeausschnitt verwenden:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
DirectX-/C++-Anwendungen werden nicht skaliert. Die automatische Skalierung gilt nur für HTML- und XAML-Anwendungen.  

## <a name="see-also"></a>Weitere Informationen
- [Bewährte Methoden für Xbox](tailoring-for-xbox.md)
- [UWP auf Xbox One](index.md)
