---
title: API-Referenz für den Geräte Portal-Remote Eingabe
description: Erfahren Sie, wie Sie Controller, Tastatur und Maus Eingaben per Remote Zugriff auf eine Xbox senden.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 34546f5aa7a1fd5bddd9122d24fc38dd2026700d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254245"
---
# <a name="remote-input-api-reference"></a>API-Referenz für Remote Eingaben   
Sie können Controller-, Tastatur-und Maus Eingaben in Echtzeit über diese API senden.

**Anforderung**

| Methode | Anforderungs-URI |
|--------|-------------|
| WebSocket | /ext/remoteinput |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderung**

Der WebSocket eine Reihe von Byte Array Nachrichten. Für jede Nachricht lautet das Format wie folgt.

Das erste Byte gibt den Eingabetyp an. Die folgenden Eingabetypen werden unterstützt:

| Eingabetyp | Bytewert |
|------------|------------|
| Tastatur Tastaturcodes | 0x01 |
| Tastatur-Scancodes | 0x02 |
| Maus | 0x03 |
| Auswahl aufheben | 0x04 |

Bei keyboardkeycodes und keyboardscancodes ist das zweite Byte der Wert von Keycode oder Scancode, und das dritte Byte ist 0x01 für eine nach-oben-Taste und 0x00 für ein Release.

Bei einer Maus Meldung ist der nächste Wert eine UInt16 in der Netzwerk Reihenfolge (2 Bytes), die den Typ des Maus Ereignisses angibt:

| Aktionstyp | UInt16-Wert |
|-------------|--------------|
| Move | 0x0001 |
| Leftdown | 0x0002 |
| Linker | 0x0004 |
| Rechts unten | 0x0008 |
| Rechter | 0x0010 |
| Middledown | 0x0020 |
| Middleup | 0x0040 |
| X1Down | 0x0080 |
| X1Up | 0x0100 |
| X2Down | 0x0200 |
| X2Up | 0x0400 |
| Verticalwheelverschoben | 0x0800 |
| Horizontalwheelverschoben | 0x1000 |

Auf dieses Byte folgen zwei UInt32-Werte in der Netzwerk Reihenfolge und ein optionales drittes UInt32 für Wheel-Aktionen. Die ersten beiden Werte sind die X-und Y-Koordinate, das dritte ist das raddelta. Der X-und der Y-Koordinaten Wert sind ein Wert zwischen 0 und 65535, der die relative Position der Maus in den horizontalen und vertikalen Ebenen angibt.

ClearAll gibt an, dass alle zurzeit abgehaltenen Schlüssel freigegeben werden sollen. Es werden keine anderen Bytes erwartet.

Zum Senden von Gamepad-Eingaben können die keycodewerte, die Gamepad-Schaltflächen drücken, mit keyboardkeycodes verwendet werden. Diese Werte lauten wie folgt:

| Gamepad-Typ | Bytewert |
|--------------|------------|
| VK_GAMEPAD_A                       |  0xC3 |
| VK_GAMEPAD_B                       |  0xc4 |
| VK_GAMEPAD_X                       |  0xc5 |
| VK_GAMEPAD_Y                       |  0xC6 |
| VK_GAMEPAD_RIGHT_SHOULDER          |  0xc7 |
| VK_GAMEPAD_LEFT_SHOULDER           |  0xc8 |
| VK_GAMEPAD_LEFT_TRIGGER            |  0xC9 |
| VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA |
| VK_GAMEPAD_DPAD_UP                 |  0xcb |
| VK_GAMEPAD_DPAD_DOWN               |  0xcc |
| VK_GAMEPAD_DPAD_LEFT               |  0xCD gefüllt |
| VK_GAMEPAD_DPAD_RIGHT              |  0xce |
| VK_GAMEPAD_MENU                    |  0xCF |
| VK_GAMEPAD_VIEW                    |  0xd0 |
| VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2 |
| VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xd3 |
| VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xd4 |
| VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xd5 |
| VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xd6 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xd7 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xd9 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xda |

**Antwort**   

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode | BESCHREIBUNG |
|----------------|-------------|
| 200 | Die Anforderung war erfolgreich. |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Xbox
