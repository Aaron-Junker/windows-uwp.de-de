---
title: Unterstützung von Point of Service (POS)-Hardware
description: Dieser Artikel enthält Informationen zur Unterstützung der Hardware für jede Point of Service (POS)-Geräteklasse
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6d67dd7bc7d2f6323679dd7c69a98df841b2848c
ms.sourcegitcommit: 769ec7811aaaa79fe521e3e984a2e1a2a9671caf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70057814"
---
# <a name="supported-point-of-service-peripherals"></a>Unterstützte Point of Service-Peripheriegeräte

## <a name="barcode-scanner"></a>Strichcodescanner
| Verbindung | Unterstützung |
| -------------|-------------|
| USB          | <p>Windows enthält einen in-Box-Klassen Treiber für USB-verbundene Barcode Scanner, die auf der von [USB.org](https://www.usb.org/hid)definierten Spezifikation für die Verwendung von HID POS-Überprüfungs Tabellen (8c) basieren. Eine Liste bekannter kompatibler Geräte finden Sie in folgender Tabelle.  Sehen Sie im Handbuch Ihres Strichcodescanners nach oder wenden Sie sich an den Hersteller, um zu erfahren, wie Sie Ihren Scanner im **USB.HID.POS Scanner**-Scannermodus konfigurieren. </p><p>Windows unterstützt auch die Implementierung von herstellerspezifischen Treibern für weitere Strichcodescanner, die den Scannerstandard „USB.HID.POS“ nicht unterstützen. Erfragen Sie beim Hersteller Ihres Strichcodescanners, ob ein herstellerspezifischer Treiber verfügbar ist.</p><p>Hersteller von Strichcodescannern sollten Sie sich an das [Strichcodescannertreiber-Entwurfshandbuch](https://aka.ms/pointofservice-drv) für weitere Informationen zum Erstellen eines benutzerdefinierten Strichcodescannertreibers wenden</p> |
| Bluetooth    | <p>Windows unterstützt den auf Seriellem Port-Protokoll – einfacher serieller Schnittstelle (SPP-SSI) basierten Bluetooth-Strichcodescanner. Eine Liste bekannter kompatibler Geräte finden Sie in folgender Tabelle. Sehen Sie im Handbuch Ihres Strichcodescanners nach oder wenden Sie sich an den Hersteller, um zu erfahren, wie Sie Ihren Scanner im **SPP-SSI**-Scannermodus konfigurieren.</p> |
| Webcam       | <p>Ab Windows 10, Version 1803, können Sie Strichcodescanner über ein Standard-Kameraobjektiv von einer universellen Windows-Anwendung lesen. Es wird empfohlen, dass Sie eine Kamera verwenden, die Autofokus und eine Auflösung von mindestens 1920 x 1440 unterstützt.  Einige niedrigere Auflösungen bei Kameras können Standardstrichcodes lesen, wenn der Strichcode groß genug gedruckt wird.  Barcodes mit weniger umfangreichen Elementen benötigen möglicherweise höhere Auflösungen bei einer Kamera.</p>| 
|


| Hersteller  | Modell                          | Funktion | Verbindung    | Typ         | Modus                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Code          | Leser™ 950                    | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 1021                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 1421                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Code          | Leser™ 5000                   | 2D         | USB          | Presentation | HID POS-Scanner           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Presentation | HID POS-Scanner           |
| Honeywell     | Granit 198xi                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | Granit 191xi                   | 2D         | USB          | Gehaltenen     | HID POS-Scanner           |
| Honeywell     | N5680                          | 2D         | Intern     | Komponente    | HID POS-Scanner           |
| Honeywell     | N3680                          | 2D         | Intern     | Komponente    | HID POS-Scanner           |
| Honeywell     | Umlauf 7190g                    | 2D         | USB          | Presentation | HID POS-Scanner           |
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




## <a name="cash-drawer"></a>Kassenschublade
| Verbindung | Unterstützung |
| -------------|-------------|
| Netzwerk/Bluetooth | <p> Eine direkte Verbindung zur Kassenschublade kann über das Netzwerk oder über Bluetooth hergestellt werden, je nach den Funktionen der Kassenschublade. </p><p>APG-Bargeld:  NetPRO, BluePRO</p> |
| DK-Port | <p> Kassenschubladen ohne Netzwerk- oder Bluetooth-Funktionen können über den DK-Port auf unterstützten Belegdruckern oder das Star Micronics DK-AirCash-Zubehör verbunden werden. </p>
| OPOS    | <p> Unterstützt alle OPOS-kompatiblen Kassenschubladen über OPOS Service-Objekte, die vom Hersteller bereitgestellt werden. Installieren Sie die OPOS-Treiber für das Gerät entsprechend den Anweisungen des Herstellers. </p> |


## <a name="customer-display-linedisplay"></a>Kunden-Anzeige (LineDisplay)
Unterstützt alle OPOS-kompatiblen Zeilenanzeigen über OPOS Service-Objekte, die vom Hersteller bereitgestellt werden. Installieren Sie die OPOS-Treiber für das Gerät entsprechend den Anweisungen des Herstellers.

## <a name="magnetic-stripe-reader"></a>Magnetstreifenleser
Windows bietet Unterstützung für die folgenden Magnetstreifenleser von Magtek und IDTech, basierend auf deren Anbieter-ID und Produkt-ID (VID/PID).

| Hersteller |    Model(le) |  Teilenummer |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| | MiniMag (VID:0ACD PID:0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |  210730xx |
| | Dynamag (VID:0801 PID:0002) |   210401xx |

 Windows unterstützt die Implementierung der zusätzlichen anbieterspezifischen Treiber zur Unterstützung von Magnetstreifenlesern. Bitte prüfen Sie die Verfügbarkeit des Magnetstreifenlesers bei Ihrem Hersteller. Hersteller von Magnetstreifenlesers sollten sich an das [Magnetstreifenleser-Entwurfshandbuch](https://aka.ms/pointofservice-drv) für weitere Informationen zum Erstellen eines benutzerdefinierten Magnetstreifenlesers wenden.

## <a name="receipt-printer-posprinter"></a>Belegdrucker (POSPrinter)
| Verbindung | Unterstützung |
| -------------|-------------|
| Netzwerk und Bluetooth | <p>Windows unterstützt die Möglichkeit, mit der Epson ESC/POS-Druckersteuerungssprache auf Belegdruckern über Netzwerk und Bluetooth zu drucken.  Die unten aufgeführten Drucker werden automatisch über POSPrinter-APIs ermittelt. Zusätzliche Belegdrucker, die eine ESC/POS-Emulation bieten, funktionieren auch, müssen aber über einen [Out-Band-Kopplungs](https://aka.ms/pointofservice-oobpairing)-Prozess verknüpft werden .</p><p>Hinweis: Kassenbelegstationen und Journalstationen werden von dieser Methode nicht unterstützt.</p> |
| OPOS    | <p> Unterstützt alle OPOS-kompatiblen Belegdrucker über OPOS-Dienstobjekte. Installieren Sie die OPOS-Treiber für das Gerät entsprechend den Anweisungen des Herstellers. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Stationäre Belegdrucker (Netzwerk/ Bluetooth)
| Hersteller |    Model(le) |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Mobile Belegdrucker (Bluetooth)
| Hersteller |    Model(le) |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
