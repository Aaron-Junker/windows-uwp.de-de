---
title: Was ist .net traceprocessing-.net-Ablauf Verfolgungs Verarbeitung
description: In dieser Übersicht erfahren Sie, was .net traceprocessing ist.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bc2ec3035e097a8cab15b556d1b1793385b8aeb9
ms.sourcegitcommit: 7f1b64f62bc3a82ebcd3807c809363df46919195
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77705755"
---
# <a name="process-etw-traces-in-net"></a>Verarbeiten von etw-Ablauf Verfolgungen in .net

Die [Ereignis Ablauf Verfolgung für Windows (Event Tracing for Windows, etw)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) ist ein leistungsfähiges System für die Ablauf Verfolgungs Sammlung, das im Windows-Betriebssystem Windows verfügt über eine umfassende Integration in etw, einschließlich Daten zum Systemverhalten für Ereignisse wie Kontextwechsel, Speicher Belegung, Prozesserstellung und-Ende sowie viele weitere. Die systemweiten Daten, die über etw verfügbar sind, eignen sich gut für die End-to-End-Leistungsanalyse oder andere Fragen, die die Interaktion zwischen vielen Komponenten im gesamten System erfordern.

Im Gegensatz zur Text Protokollierung stellt ETW strukturierte Ereignisse bereit, die für die automatisierte Datenverarbeitung entwickelt wurden. Microsoft hat auf diesen strukturierten Ereignissen leistungsstarke Tools erstellt, darunter [Windows Performance Analyzer (WPA)](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer), das eine grafische Benutzeroberfläche für die Visualisierung und Untersuchung der in einer etw-Ablauf Verfolgungs Datei (ETL) aufgezeichneten Ablauf Verfolgungs Daten bereitstellt.

Innerhalb von Microsoft verwenden wir häufig etw-Ablauf Verfolgungen, um die Leistung neuer Builds von Windows zu messen. Aufgrund der Menge der Daten, die das Windows-Engineering-System erzeugt hat, ist die automatisierte Analyse unverzichtbar. Für unsere automatisierte Ablauf Verfolgungs Analyse verwenden C# wir häufig und .net. Daher haben wir die [.net traceprocessing-API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) für den Zugriff auf viele Arten von etw-Ablauf Verfolgungs Daten erstellt. Diese Technologie wird in Windows Performance Analyzer auch verwendet, um mehrere Tabellen zu unterhalten.

Mit den nuget-Paketen für .net traceprocessing können Sie Ihre eigenen Anwendungen und Systeme mit denselben Tools analysieren, die Microsoft zur Analyse von Windows verwendet.

## <a name="next-steps"></a>Nächste Schritte

In dieser Übersicht haben Sie erfahren, was .net traceprocessing ist.

Der nächste Schritt ist die [Verarbeitung der ersten Ablauf Verfolgung](quickstart.md).

## <a name="related-topics"></a>Verwandte Themen

* [Zugreifen auf Ablauf Verfolgungs Daten](tutorial.md)
* [Traceprocessor erweitern](extensibility.md)
* [Symbole laden](symbols.md)
* [Streaming verwenden](streaming.md)
* [Beispiele](https://github.com/microsoft/eventtracing-processing-samples)
* [API-Referenz](reference.md)
