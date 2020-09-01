---
title: Eingaben für Spiele
description: In diesem Abschnitt wird die Arbeit mit Gamepads und anderen Eingabegeräten für Spiele für die universelle Windows-Plattform (UWP) veranschaulicht.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Input
ms.localizationpriority: medium
ms.openlocfilehash: 0c9565f02356b776738bb325eb9a29e84582f4ad
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163094"
---
# <a name="input-for-games"></a>Eingaben für Spiele

In diesem Abschnitt werden die verschiedenen Arten von Eingabegeräten beschrieben, die in Spielen für die universelle Windows-Plattform (UWP) unter Windows 10 und auf Xbox One verwendet werden können. Darüber hinaus wird ihre grundlegende Nutzung veranschaulicht, und es werden Muster und Verfahren für die effektive Eingabeprogrammierung in Spielen empfohlen.

> **Hinweis**    Es gibt weitere Arten von Eingabegeräten für den Einsatz in UWP-Spielen, z. B. benutzerdefinierte Eingabegeräte, die genre- oder spielspezifisch sein können. Diese Geräte und deren Programmierung werden in diesem Abschnitt nicht behandelt. Informationen zu den Schnittstellen, die die Bereitstellung benutzerdefinierter Eingabegeräte ermöglichen, finden Sie unter dem [Windows.Gaming.Input.Custom](/uwp/api/windows.gaming.input.custom)-Namespace.

## <a name="gaming-input-devices"></a>Eingabegeräte für Spiele

Eingabegeräte für Spiele werden in UWP-Spielen und -Apps für Windows 10 und Xbox One durch den [Windows.Gaming.Input](/uwp/api/windows.gaming.input)-Namespace unterstützt.

### <a name="gamepads"></a>Gamepads

Gamepads sind die Standardeingabegeräte für Xbox One und werden häufig auch von Windows-Spielern als Alternative zur Tastatur und Maus genutzt. Sie bieten eine Vielzahl digitaler und analoger Steuerungen, eignen sich für nahezu alle Arten von Spielen und geben durch integrierte Vibrationsmotoren auch taktiles Feedback.

Weitere Informationen zur Verwendung von Gamepads in Ihrem UWP-Spiel finden Sie unter [Gamepad und Vibration](gamepad-and-vibration.md).

### <a name="arcade-sticks"></a>Arcade-Joysticks

Arcade-Joysticks sind reine digitale Eingabegeräte, die den Eindruck klassischer Arcade-Automaten vermitteln sollen und das ideale Eingabegerät für Kampfspiele und andere Spiele im Arcade-Stil darstellen.

Weitere Informationen zur Verwendung von Arcade-Joysticks in Ihrem UWP-Spiel finden Sie unter [Arcade-Joystick](arcade-stick.md).

### <a name="racing-wheels"></a>Rennlenkräder

Rennlenkräder sind Eingabegeräte, die Benutzern das Gefühl vermitteln, in einem echten Rennwagencockpit zu sitzen. Sie sind das ideale Eingabegerät für Rennsportspiele mit Rennwagen oder Trucks. Viele Rennlenkräder sind mit einer echten Kraftrückmeldung und nicht einfach nur Vibrationsmotoren ausgestattet, d. h., sie können echte Kräfte auf eine Steuerungsachse wie das Lenkrad ausüben.

Informationen zur Verwendung von Rennlenkrädern in Ihrem UWP-Spiel finden Sie unter [Rennlenkräder und Kraftrückmeldung](racing-wheel-and-force-feedback.md).

### <a name="flight-sticks"></a>Flugsticks

Bei Flight Sticks handelt es sich um Gaming-Eingabegeräte, die das Gefühl der flugsticks reproduzieren, die im Cockpit einer Ebene oder des Netz Schiffs gefunden werden. Sie sind das perfekte Eingabegerät für die schnelle und exakte Kontrolle des Flugs.

Weitere Informationen zur Verwendung von flugsticks in Ihrem UWP-Spiel finden Sie unter [Flight Stick](flight-stick.md).

### <a name="raw-game-controllers"></a>Rohspiel Controller

Ein unformatierte Spielcontroller ist eine generische Darstellung eines Spiel Controllers, wobei Eingaben auf vielen unterschiedlichen Arten von gängigen Spiel Controllern gefunden werden. Diese Eingaben werden als einfache Arrays unbenannter Schaltflächen, Switches und Achsen verfügbar gemacht. Mithilfe eines unformatierten Spiel Controllers können Sie es Kunden ermöglichen, benutzerdefinierte Eingabe Zuordnungen zu erstellen, unabhängig davon, welche Art von Controller Sie verwenden.

Weitere Informationen zur Verwendung von rohspiel Controllern in Ihrem UWP-Spiel finden Sie unter [RAW Game Controller](raw-game-controller.md).

### <a name="ui-navigation-controllers"></a>UI-Navigations Controller

Benutzeroberflächen-Navigationscontroller sind logische Eingabegeräte, die einen allgemeingültigen Befehlskontext für die Benutzeroberflächennavigation bieten. So wird bei unterschiedlichen Spielen und physischen Eingabegeräten eine einheitliche Benutzererfahrung gewährleistet. Für die Benutzeroberfläche eines Spiels sollten anstelle gerätespezifischer Schnittstellen die UINavigationController-Schnittstellen verwendet werden.

Weitere Informationen zur Verwendung der Benutzeroberflächen-Navigationscontroller in UWP-Spielen finden Sie unter [Benutzeroberflächen-Navigationscontroller](ui-navigation-controller.md).

### <a name="headsets"></a>Lenkköpfe

Headsets sind Geräte für die Audioaufnahme und -wiedergabe, die einem bestimmten Benutzer zugeordnet werden, wenn er über sein Eingabegerät verbunden ist. Sie werden häufig in Onlinespielen für Sprach-Chats verwendet, können jedoch auch die Immersion von Spielern verbessern oder Gameplayfeatures in Online- und Offlinespielen bereitstellen.

Weitere Informationen zur Verwendung von Headsets in UWP-Spielen finden Sie unter [Headset](headset.md).

### <a name="users"></a>Benutzer

Jedes Eingabegerät und das angeschlossene Headset können einem bestimmten Benutzer zugeordnet werden, um seine Identität mit seinem Spiel zu verknüpfen. Die Identität des Benutzers dient auch dazu, die Eingaben über ein physisches Eingabegerät mit Eingaben über einen logischen Benutzeroberflächen-Navigationscontroller zu korrelieren.

Weitere Informationen zur Verwaltung von Benutzern und ihren Eingabegeräten finden Sie unter [Nachverfolgen von Benutzern und ihren Geräten](input-practices-for-games.md#tracking-users-and-their-devices).

## <a name="see-also"></a>Weitere Informationen

* [Eingabemethoden für Spiele](input-practices-for-games.md)
* [Windows. Gaming. Input-Namespace](/uwp/api/windows.gaming.input)
* [Windows. Gaming. Input. Custom-Namespace](/uwp/api/windows.gaming.input.custom)