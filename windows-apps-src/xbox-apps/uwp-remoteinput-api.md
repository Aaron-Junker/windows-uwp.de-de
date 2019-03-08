---
title: Geräteportal-Remoteeingabe – API-Referenz
description: Hier erfahren Sie, wie Sie remote Controller-, Tastatur- und Mauseingaben auf einer Xbox senden.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 882e84c5126e4f67e246dd479008133c979c06b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595955"
---
# <a name="remote-input-api-reference"></a>API-Referenz für die Remoteeingabe   
Über diese API können Sie in Echtzeit per Fernzugriff Controller-, Tastatur- und Mauseingaben senden.

**Anforderung**

Methode      | Anforderungs-URI
:------     | :-----
Websocket | /ext/remoteinput
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderung**

Der Websocket, eine Reihe von Byte-Array-Nachrichten. Für jede Nachricht ist das Format wie folgt.

Das erste Byte gibt den Eingabetyp an. Die folgenden Eingabetypen werden unterstützt:

| Eingabetyp        | Byte-Wert |
|------------|-------------|
Tastencodes der Tastatur | 0x01
ScanCodes der Tastatur | 0x02
Maus | 0x03
Auswahl aufheben | 0x04

Für KeyboardKeyCodes und KeyboardScanCodes ist das zweite Byte der Wert des Tastencodes oder Scancodes und das dritte Byte ist 0x01 für das Drücken der Taste und 0x00 für das Loslassen der Taste.

Für eine Mausmeldung ist der nächste Wert ein UINT16 in der Netzwerkreihenfolge (2 Byte), der den Typ des Mausereignisses angibt:

| Aktionstyp        | UINT16-Wert |
|------------|-------------|
Verschieben | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

Nach diesem Byte folgen zwei UINT32-Werte in der Netzwerkreihenfolge und ein optionaler dritter UINT32 für Radaktionen. Die ersten beiden Werte sind die X- und Y-Koordinaten und der dritte ist der Deltawert für das Mausrad. Die X- und Y-Koordinaten sollten erwartungsgemäß einen Wert zwischen 0 und 65535 haben, der jeweils die relative Position der Maus auf der horizontalen und vertikalen Ebene angibt.

ClearAll gibt an, dass alle derzeit gedrückten Tasten losgelassen werden sollten. Es werden keine anderen Bytes erwartet.

Um Gamepad-Eingaben zu senden, können die Tastencodewerte, die Gamepad-Tastendrücke darstellen, mit KeyboardKeyCodes verwendet werden. Diese Werte sind:

| Gamepad-Typ        | Byte-Wert |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**Antwort**   

- Keine

**Statuscode:**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Gerätefamilien verfügbar**

* Windows Xbox