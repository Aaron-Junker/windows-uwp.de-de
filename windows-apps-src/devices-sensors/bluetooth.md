---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: Dieser Abschnitt enthält Artikel zur Bluetooth-Integration in UWP-Apps (Universelle Windows-Plattform). Dies umfasst u. a. die Verwendung von RFCOMM-, GATT- und LE (Low Energy)-Ankündigungen.
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469565"
---
# <a name="bluetooth"></a>Bluetooth
Dieser Abschnitt enthält Artikel zum Integrieren von Bluetooth in universelle Windows-Plattform-Apps (UWP). Es gibt zwei verschiedene Bluetooth-Technologien, die Sie in Ihrer APP implementieren können.

> [!Important]
> Sie müssen die Funktion "Bluetooth" in " *Package. appxmanifest*" deklarieren.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>Klassisches Bluetooth (RFCOMM)
Vor Bluetooth Le verwendeten Geräte häufig dieses Protokoll für die Kommunikation mit Bluetooth. Dieses Protokoll ist einfach und nützlich für die Kommunikation zwischen Geräten, ohne dass Energie gespart werden muss. Weitere Informationen zu diesem Protokoll, einschließlich Codebeispielen, finden Sie im Thema [Bluetooth RFCOMM](send-or-receive-files-with-rfcomm.md) .

## <a name="bluetooth-low-energy-le"></a>Bluetooth Low-Energy (Le)
Bluetooth Low Energy (Le) ist eine Spezifikation, die Protokolle für die Ermittlung und Kommunikation zwischen Geräten definiert, die eine effiziente Stromverbrauchs Anforderung haben. Weitere Informationen, einschließlich Codebeispielen, finden Sie im Thema [Bluetooth Low Energy (Bluetooth Low Energy](bluetooth-low-energy-overview.md) ).

## <a name="see-also"></a>Weitere Informationen
- [Bluetooth-Entwickler – Häufig gestellte Fragen](bluetooth-dev-faq.md)