---
title: Konfigurieren eines Strichcodescanners
description: Erfahren Sie, wie Sie einen Barcode Scanner für die beabsichtigte Anwendung konfigurieren.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168504"
---
# <a name="configure-a-barcode-scanner"></a>Konfigurieren eines Strichcodescanners

Barcode Scanner können in verschiedenen Modi konfiguriert werden.  Es ist wichtig, dass der Barcode Scanner für die vorgesehene Anwendung ordnungsgemäß konfiguriert ist.

Viele Barcode Scanner können im Tastatur- **Keil** -Modus konfiguriert werden, sodass der Barcode Scanner als Tastatur für Windows angezeigt wird.  Dies ermöglicht Ihnen das Scannen von Barcodes in Anwendungen, bei denen es sich nicht um Barcode Scanner handelt, z. b. Notepad.  Wenn Sie in diesem Modus einen Barcode Scannen, werden die decodierten Daten aus dem Barcode Scanner an der Einfügemarke eingefügt, als wenn Sie die Daten mit der Tastatur eingegeben haben.  Wenn Sie über Ihre UWP-Anwendung mehr Kontrolle über den Barcode Scanner wünschen, müssen Sie ihn in einem nicht-Tastatur-Keil-Modus konfigurieren.

## <a name="usb-barcode-scanner"></a>USB-Barcode Scanner
Ein USB-verbundener Barcode Scanner muss im **HID POS-Scanner** -Modus konfiguriert werden, damit er mit dem Barcode Scanner-Treiber funktioniert, der in Windows enthalten ist. Dieser Treiber ist eine Implementierung der " **HID Point of Sale Usage Tables** "-Spezifikation, die auf [USB-HID](https://www.usb.org/hid)veröffentlicht wird.  Anweisungen zum Aktivieren des Modus " **HID POS Scanner** " finden Sie in der Dokumentation zum Barcode Scanner.  Nach der Konfiguration des **Scanners "HID POS** " wird der Barcode Scanner in Geräte-Manager unter dem **POS-Barcode** Scanner-Knoten als POS-unterstrich- **Barcode Scanner**angezeigt.

Der Barcode Scanner kann auch über einen herstellerspezifischen Treiber verfügen, der die UWP-Barcode Scanner-APIs in einem anderen Modus als der **HID POS-Scanner**unterstützt.  Wenn Sie bereits einen vom Hersteller bereitgestellten Treiber installiert haben, der mit UWP-Barcode Scanner kompatibel ist, wird in Geräte-Manager möglicherweise ein herstellerspezifisches Gerät angezeigt, das unter **POS-Barcode Scanner** aufgeführt ist.

## <a name="bluetooth-barcode-scanner"></a>Bluetooth-Barcode Scanner
Ein Bluetooth-verbundener Scanner muss im **spp-SSI-Modus (Serial Port Protocol-Simple Serial Interface)** konfiguriert werden, um mit den UWP-Barcode Scanner-APIs arbeiten zu können.  Anweisungen zum Aktivieren des **spp-SSI-Modus**finden Sie in der Dokumentation zum Barcode Scanner, oder wenden Sie sich an Ihren Barcode Scanner.

Bevor Sie den Bluetooth-Barcode Scanner verwenden können, müssen Sie ihn mithilfe von **Einstellungen > Geräte > Bluetooth & anderen Geräten koppeln, > Bluetooth oder ein anderes Gerät hinzufügen**.

Sie können den paarungsprozess mit dem [Windows. Devices. Enumeration](/uwp/api/windows.devices.enumeration) -Namespace initiieren und steuern.  Weitere Informationen finden Sie unter [Pair Devices](./pair-devices.md) .

[!INCLUDE [feedback](./includes/pos-feedback.md)]