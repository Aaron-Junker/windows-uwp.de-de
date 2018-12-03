---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS mit Formatierung
description: Erfahren Sie, wie Sie die ESC/POS-Befehlssprache zum Formatieren von Text, z. B. in Fett und mit doppelter Größe, für Ihren Point of Service-Drucker verwenden.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 11467a45021da7898c2b617e3b1b01312c795c4c
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2018
ms.locfileid: "8468674"
---
# <a name="epson-escpos-with-formatting"></a>Epson ESC/POS mit Formatierung


**Wichtige APIs**

-   [**PointofService-Drucker**](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Erfahren Sie, wie Sie die ESC/POS-Befehlssprache zum Formatieren von Text, z.B. in Fett und mit doppelter Größe, für Ihren Point of Service-Drucker verwenden.

## <a name="escpos-usage"></a>ESC/POS-Verwendung

Windows Point of Service ermöglicht die Verwendung einer Vielzahl von Druckern, z.B. mehrerer Drucker der EpsonTM-Reihe. (Eine vollständige Liste der unterstützten Drucker finden Sie auf der Seite [PointofService-Drucker](https://msdn.microsoft.com/library/windows/apps/Mt426652).) Windows unterstützt das Drucken über die ESC/POS-Druckersteuerungssprache, die über effiziente und funktionale Befehle für die Kommunikation mit dem Drucker verfügt.

ESC/POS ist ein von Epson erstelltes Befehlssystem, das für viele POS-Druckersysteme verwendet wird und mit dem inkompatible Befehlssätze vermieden werden sollen, indem für eine universelle Anwendbarkeit gesorgt wird. Die meisten modernen Drucker unterstützen ESC/POS.

Alle Befehle beginnen mit dem ESC-Zeichen (ASCII27, HEX1B) oder GS (ASCII29, HEX1D), gefolgt von einem weiteren Zeichen, das für den Befehl steht. Normaler Text wird – getrennt durch Zeilenumbrüche – einfach an den Drucker gesendet.

Die [**Windows PointOfService API**](https://msdn.microsoft.com/library/windows/apps/Dn298071) stellt einen Großteil der Funktionalität für Sie über die Methoden **Print()** oder **PrintLine()** bereit. Um bestimmte Formatierungen zu erzielen oder spezielle Befehle zu senden, müssen Sie jedoch ESC/POS-Befehle senden, die wie eine Zeichenfolge aufgebaut sind und an den Drucker gesendet werden.

## <a name="example-using-bold-and-double-size-characters"></a>Beispiel für die Verwendung von fetten Zeichen und Zeichen mit doppelter Größe

Im folgenden Beispiel wird die Verwendung von ESC/POS-Befehlen zum Drucken von fetten Zeichen und Zeichen mit doppelter Größe veranschaulicht. Beachten Sie, dass jeder Befehl wie eine Zeichenfolge aufgebaut ist und dann in die printJob-Aufrufe eingefügt wird.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

Weitere Informationen zu ESC/POS, darunter zu den verfügbaren Befehlen, finden Sie unter [Epson ESC/POS – FAQ](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Ausführliche Informationen zu [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) sowie zu allen verfügbaren Funktionen finden Sie auf der MSDN-Website unter [PointofService Printer](https://msdn.microsoft.com/library/windows/apps/Mt426652).
