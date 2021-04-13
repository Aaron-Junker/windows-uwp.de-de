---
title: IWindowNative-Schnittstelle
description: WinUI-COM-Schnittstelle, die Interoperation zwischen XAML und einem nativen Fenster bereitstellt.
ms.topic: reference
ms.date: 03/09/2021
keywords: WinUI, Windows-UI-Bibliothek
ms.openlocfilehash: e641ab73a87e993fddb3f6aa8b90a0149744b99f
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730659"
---
# <a name="iwindownative-interface-microsoftuixamlwindowh"></a>IWindowNative-Schnittstelle (microsoft.ui.xaml.window.h)

Aktiviert die Interoperation zwischen XAML und einem nativen Fenster.

Diese Schnittstelle wird durch [Window](/windows/winui/api/microsoft.ui.xaml.window) implementiert, mit dem Desktop-Apps das zugrunde liegende HWND des Fensters abrufen können.

## <a name="inheritance"></a>Vererbung

Die IWindowNative-Schnittstelle erbt von der IUnknown-Schnittstelle. IWindowNative verfügt auch über die folgenden Typen von Membern:

- [Eigenschaften](#properties)

## <a name="properties"></a>Eigenschaften

Die IWindowNative-Schnittstelle verfügt über diese Eigenschaften.

| Eigenschaft | Beschreibung |
| --- | --- |
| [IWindowNative::WindowHandle](iwindownative-windowhandle.md) | Ruft das angeforderte Fenster-HWND ab. |

## <a name="applies-to"></a>Gilt für:

| „Product“ (Produkt) | Versionen |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5, 3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>Weitere Informationen

[Windows-UI-Bibliothek (WinUI)](../index.md)