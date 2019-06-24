---
title: Konfigurieren eines Strichcodescanners
description: Erfahren Sie, wie Sie einen Barcode-Scanner für die betroffene Anwendung zu konfigurieren.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321586"
---
# <a name="configure-a-barcode-scanner"></a>Konfigurieren eines Strichcodescanners

Strichcodescanner können in mehreren verschiedenen Modi konfiguriert werden.  Es ist wichtig für Ihren Strichcodescanner, dass er für die Anwendung ordnungsgemäß konfiguriert ist.

Viele Strichcodescanner können im **Tastatur Wedge**-Modus konfiguriert werden. Dabei wird der Strichcodescanner als eine Tastatur auf Windows angezeigt.  Dadurch können Sie Strichcodes in Anwendungen scannen, die keine Strichcodescanner unterstützen, wie Notepad.  Wenn Sie einen Strichcode in diesem Modus scannen, werden die decodierten Daten aus dem Strichcodescanner an der Einfügemarke eingefügt, als ob Sie die Daten über die Tastatur eingegeben.  Wenn Sie mehr Kontrolle über Ihren Strichcodescanner von der UWP-App haben möchten, müssen Sie diesen in einem Modus ohne Tastatur-Wedge konfigurieren.

## <a name="usb-barcode-scanner"></a>USB-Strichcodescanner
Ein USB-verbundener Strichcodescanner muss im **HID POS-Scanner**-Modus mit den Strichcodescannertreiber konfiguriert werden, der in Windows enthalten ist. Dieser Treiber ist eine Implementierung von der **HID Punkt von Sale Nutzung Tabellen** Spezifikation veröffentlicht [USB HID](https://www.usb.org/hid).  Weitere Informationen finden Sie in der Dokumentation der Strichcodescanner oder wenden Sie sich an den Hersteller des Scanners für weitere Anweisungen zum Aktivieren des **POS-HID-Scanner**-Modus.  Wenn der Strichcodescanner als **POS-HID-Scanner** konfiguriert ist, erscheint er im Geräte-Manager unter dem **POS-Strichcodescanner**-Knoten als **POS HID Strichcodescanner**.

Der Hersteller des Strichcodescanners hat möglicherweise einen anbieterspezifischen Treiber, der die UWP Strichcodescanner-APIs in einem anderen Modus als **POS-HID-Scanner** unterstützt.  Wenn Sie einen Hersteller bereitgestellte Treiber, die kompatibel mit UWP-Barcode-Scanner-APIs bereits installiert haben, können Sie sehen, dass eine anbieterspezifische Geräte unter **POS Barcode-Scanner** im Geräte-Manager.

## <a name="bluetooth-barcode-scanner"></a>Bluetooth-Strichcodescanner
Ein mit Bluetooth verbundener Scanner muss im **Seriellen Port-Protokoll – einfache serielle Schnittstelle (SPP-SSI)** -Modus für die Arbeit mit UWP Strichcodescanner-APIs konfiguriert sein.  Weitere Informationen finden Sie in der Dokumentation der Strichcodescanner oder wenden Sie sich an den Hersteller des Scanners für weitere Anweisungen zum Aktivieren des **SSI-SPP-Modus**.

Vor der Verwendung des Bluetooth-Barcode-Scanners müssen Sie es mit koppeln **Einstellungen > Geräte > Bluetooth & andere Geräte > Bluetooth hinzufügen oder ein anderes Gerät**.

Können Sie zu initiieren und steuern, die ereignispaarbildung Prozess mithilfe der [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) Namespace.  Finden Sie unter [Paar Geräte](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) für Weitere Informationen.

[!INCLUDE [feedback](./includes/pos-feedback.md)]