---
title: Point of Service-Hardware Unterstützung
description: Dieser Artikel enthält Informationen zur Hardwareunterstützung für jede der Punkt-zu-Dienst-Geräteklassen.
ms.date: 06/13/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9e89011086e6c6d318d589226400789b41f8fe64
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750346"
---
# <a name="supported-point-of-service-peripherals"></a>Unterstützte Dienst Peripherie-Peripheriegeräte

## <a name="barcode-scanner"></a>Barcodescanner
| Konnektivität | Support |
| -------------|-------------|
| USB          | <p>Windows enthält einen in-Box-Klassen Treiber für USB-verbundene Barcode Scanner, die auf der von [USB.org](https://www.usb.org/hid)definierten Spezifikation für die Verwendung von HID POS-Überprüfungs Tabellen (8c) basieren. In der folgenden Tabelle finden Sie eine Liste bekannter kompatibler Geräte.  Wenden Sie sich an das manuelle für Ihren Barcode Scanner, oder wenden Sie sich an den Hersteller, um zu bestimmen, wie Sie den Scanner auf **USB HID. POS-Scanner** -Modus. </p><p>Windows unterstützt auch die Implementierung Hersteller spezifischer Treiber, um zusätzliche Barcode Scanner zu unterstützen, die den USB-Code nicht unterstützen. HID. POS-Scanner-Standard. Wenden Sie sich an den Hersteller des Barcode Scanners, um die herstellerspezifische Treiber Verfügbarkeit zu ermitteln.</p><p>Barcode Scanner-Hersteller weitere Informationen zum Erstellen eines benutzerdefinierten Barcode Scanner-Treibers finden Sie im [Entwurfs Handbuch für den Barcode Scanner](/windows-hardware/drivers/ddi/_pos/index) .</p> |
| Bluetooth    | <p>Windows unterstützt serielle Port Protokoll-Simple Serial Interface (SPP-SSI)-basierte Bluetooth-Barcode Scanner. In der folgenden Tabelle finden Sie eine Liste bekannter kompatibler Geräte. Wenden Sie sich an das manuelle für Ihren Barcode Scanner, oder wenden Sie sich an den Hersteller, um zu bestimmen, wie Sie den Scanner im **spp-SSI-** Modus</p> |
| Webcam       | <p>Ab Windows 10, Version 1803, können Sie Barcodes über einen Standardkamera-Foto aus einer universellen Windows-Anwendung lesen. Es wird empfohlen, dass Sie eine Kamera verwenden, die den automatischen Fokus und eine minimale Auflösung von 1920 x 1440 unterstützt.  Einige Kameras mit niedrigerer Auflösung können Standard-Barcodes lesen, wenn der Barcode groß genug gedruckt ist.  Barcodes mit dünneren Elementen erfordern möglicherweise eine höhere Auflösung von Kameras.</p>| 
|


| Hersteller  | Modell                          | Funktion | Verbindung    | type         | Mode                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Code          | Leser™ 950                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 1021                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 1421                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 5000                   | 2D         | USB          | Präsentation | HID POS-Scanner           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Präsentation | HID POS-Scanner           |
| Honeywell     | Granit 198xi                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Granit 191xi                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | N5680                          | 2D         | Intern     | Komponente    | HID POS-Scanner           |
| Honeywell     | N3680                          | 2D         | Intern     | Komponente    | HID POS-Scanner           |
| Honeywell     | Umlauf 7190g                    | 2D         | USB          | Präsentation | HID POS-Scanner           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | Im Zählers   | HID POS-Scanner           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Voyager 1202-BF                | 1D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | "Voyager 145xg"                  | 1D/2D<sup>1</sup>   | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Xenon 1900g                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Xenon 1902g                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Xenon 1902g-BF                 | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Xenon 19 h                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Xenon 1902h                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| HP            | Wert Barcode Scanner (HR2150) | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Intermec      | SG20                           | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Socket Mobile | CHS 7ci                        | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | CHS 7DI                        | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | CHS 7MI                        | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | CHS 7pi                        | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | CHS 8ci                        | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | DuraScan D700                  | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | DuraScan D730                  | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | DuraScan D740                  | 2D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | Socketscan S700                | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | Socketscan S730                | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | Socketscan S740                | 2D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | Socketscan S800                | 1D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Socket Mobile | Socketscan S850                | 2D         | Bluetooth    | Gehaltenen     | PP (Serial Port Profile) |
| Ze         | DS2208<sup>2</sup>                        | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Ze         | DS2278                         | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Ze         | DS8108<sup>3</sup>                        | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Ze         | DS8178<sup>4</sup>                         | 2D         | USB          | Gehaltenen     | HID POS-Scanner           | 


<sup>1</sup> aktualisierbar zur Unterstützung von 2D-Barcodes durch Honeywell <br/>
<sup>2</sup> mindestens erforderliche Firmware 009 (2018.07.09) erforderlich. Aktualisierbar mithilfe von Zebra [123scan](http://www.zebra.com/123scan).<br/>
<sup>3</sup> mindestens erforderliche Firmware (2018.01.18) erforderlich. Aktualisierbar mithilfe von Zebra [123scan](http://www.zebra.com/123scan).<br/> 
<sup>4</sup> minimale Firmware 023 (2019.03.11) erforderlich. Aktualisierbar mithilfe von Zebra [123scan](http://www.zebra.com/123scan).<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Windows-Geräte mit integrierter Barcode Scanner
| Hersteller   | Modell | Betriebssystem |
|----------------|-------|------------------|
| InnoWi         | Checout-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Windows Mobile-Geräte mit integrierter Barcode Scanner
| Hersteller   | Modell | Betriebssystem |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| Pointmobile    | PM80 | Windows Mobile   |
| Ze          | TC700j | Windows Mobile   |
| HP             | Elite-X3-Jacke | Windows Mobile   |




## <a name="cash-drawer"></a>Einschub Fach
| Konnektivität | Support |
| -------------|-------------|
| Netzwerk/Bluetooth | <p> Die direkte Verbindung mit dem Bargeld Bereich kann über das Netzwerk oder über Bluetooth erfolgen, abhängig von den Funktionen der Einheiten für die Bargeld Schublade. </p><p>APG Cash-Schublade: NetPro, bluepro</p> |
| DK-Port | <p> Bargeld Träger, die keine Netzwerk-oder Bluetooth-Funktionen aufweisen, können über den DK-Port auf einem unterstützten Empfangsdrucker oder das Star Micronics DK-aircash-Zubehör verbunden werden. </p>
| OPOS    | <p> Unterstützt alle mit OPOS kompatiblen Bargeld-und vom Hersteller bereitgestellten OPOS-Dienst Objekte. Installieren Sie die OPOS-Treiber gemäß den Installationsanweisungen für Gerätehersteller. </p> |


## <a name="customer-display-linedisplay"></a>Kundenanzeige (LineDisplay)
Unterstützt alle mit OPOS kompatiblen Zeilen anzeigen, die vom Hersteller bereitgestellt werden. Installieren Sie die OPOS-Treiber gemäß den Installationsanweisungen für Gerätehersteller.

## <a name="magnetic-stripe-reader"></a>Magnetstreifenleser
Windows bietet Unterstützung für die folgenden Magnetstreifenleser von MagTek und IDTech basierend auf ihrer Hersteller-ID und Produkt-ID (VID/PID).

| Hersteller |    Modell (e) |  Teilenummer |
|--------------|-----------|--------------|
| IDTech | Securemag (vid: 0acd PID: 2010) | Idre-3x5xxxx |
| | Minimag (vid: 0acd PID: 0500) |   Idmb-3x5xxxx |
| MagTek | Magnesafe (vid: 0801 PID: 0011) |  210730xx |
| | Dynamag (vid: 0801 PID: 0002) |   210401xx |

 Windows unterstützt die Implementierung zusätzlicher Anbieter spezifischer Treiber zur Unterstützung zusätzlicher Magnetstreifenleser. Informieren Sie sich bei Ihrem Magnet Stripe Reader-Hersteller über die Verfügbarkeit. Magnetstreifenleser-Hersteller Informationen zum Erstellen eines benutzerdefinierten Magnet Stripe-Reader-Treibers finden Sie im [Entwurfs Handbuch für den Magnet Stripe Reader-Treiber](/windows-hardware/drivers/ddi/_pos/index) .

## <a name="receipt-printer-posprinter"></a>Empfangsdrucker (posprinter)
| Konnektivität | Support |
| -------------|-------------|
| Netzwerk und Bluetooth | <p>Windows unterstützt Netzwerk-und Bluetooth-Empfangsdrucker, die die Epson ESC/POS-druckersteuerungssprache verwenden.  Die nachstehend aufgeführten Drucker werden automatisch mithilfe von posprinter-APIs erkannt. Zusätzliche Empfangsdrucker, die eine ESC/POS-Emulation bereitstellen, können auch funktionieren, müssen jedoch mithilfe eines [out-of-Band](./point-of-service.md) -Kopplung-Prozesses verknüpft werden.</p><p>Hinweis: die-Station und die Journal Stationen werden von dieser Methode nicht unterstützt.</p> |
| OPOS    | <p> Unterstützt alle mit OPOS kompatiblen empfangsdruckern über OPOS-Dienst Objekte. Installieren Sie die OPOS-Treiber gemäß den Installationsanweisungen für Gerätehersteller. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Drucker der stationären Bestätigung (Netzwerk/Bluetooth)
| Hersteller |    Modell (e) |
|--------------|-----------|
| EP |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Mobile Belegdrucker (Bluetooth)
| Hersteller |    Modell (e) |
|--------------|-----------|
| EP |   MobiLink P20 (TM-P20), MobiLink P60 (TM-P60), MobiLink P80 (TM-P80) |