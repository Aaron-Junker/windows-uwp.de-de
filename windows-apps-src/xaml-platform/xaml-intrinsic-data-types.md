---
description: Listet Unterstützung auf Sprachebene in XAML für die Windows-Runtime für bestimmte Datentypen in der Common Language Runtime (CLR) und in anderen Programmiersprachen wie C++ auf.
title: Systeminterne XAML-Datentypen
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 683c41cba0c002f1482851b3cd3d00a2df352721
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173764"
---
# <a name="xaml-intrinsic-data-types"></a>Systeminterne XAML-Datentypen


XAML für die Windows-Runtime stellt auf Sprachebene Unterstützung für mehrere Datentypen bereit, die häufig verwendete Grundtypen in der Common Language Runtime (CLR) und in anderen Programmiersprachen wie C++ sind.

Am häufigsten finden Sie die Verwendung systeminterner XAML-Datentypen, wenn Ressourcen in einem XAML-Ressourcenwörterbuch definiert werden. Sie können in diesem Wörterbuch Konstanten definieren, z. B. Zahlen, die Sie für mehrere Werte verwenden. Sie können auch eine Storyboardanimation verwenden, um eine Animation mithilfe eines Zeichenfolgen- oder booleschen Werts zu erreichen. Sie benötigen dann ein XAML-Objektelement, das die Zeichenfolge oder den booleschen Wert darstellt, um den Keyframe der [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Definition aufzufüllen. Die Windows-Runtime-XAML-Standardvorlagen verwenden beide Techniken.

XAML für die Windows-Runtime bietet Unterstützung auf Sprachebene für diese Typen:

| XAML-Grundtyp | BESCHREIBUNG |
|-------|-------------|
| **x:Boolean**  | Für die CLR-Unterstützung: [**Boolean**](/dotnet/api/system.boolean). XAML analysiert Werte für **x:Boolean** ohne Berücksichtigung der Groß-/Kleinschreibung. „x:Bool“ ist keine zulässige Alternative. |
| **x:String**   | Für die CLR-Unterstützung: [**String**](/dotnet/api/system.string). Die Codierung für die Zeichenfolge wird standardmäßig auf die umgebende XML-Codierung festgelegt. |
| **x:Double**   | Für die CLR-Unterstützung: [**Double**](/dotnet/api/system.double). Zusätzlich zu den numerischen Werten lässt die Textsyntax für **x:Double** das Token „NaN“ zu. So kann „Auto“ für das Layoutverhalten als Ressourcenwert gespeichert werden. Bei den Token wird die Groß-/Kleinschreibung berücksichtigt. Sie können die wissenschaftliche Schreibweise verwenden (also beispielsweise „1+E06“ für `1,000,000`). |
| **x:Int32**    | Für die CLR-Unterstützung: [**Int32**](/dotnet/api/system.int32). **x:Int32** wird als Zahl mit Vorzeichen behandelt. Sie können das Minuszeichen („-“) für eine negative ganze Zahl verwenden. In XAML wird das Fehlen eines Vorzeichens in der Textsyntax als positiver Wert mit Vorzeichen interpretiert. |

Diese Grundtypen der Programmiersprache XAML sind im Allgemeinen die einzigen Fälle, in denen Sie ein Objektelement definieren, das das Präfix **x:** in XAML verwendet. Alle anderen XAML-Sprachfeatures werden in der Regel in Attributform oder als Markuperweiterung verwendet.

**Hinweis**    Gemäß der Konvention werden die sprach primitiven für XAML und alle anderen XAML-Sprachelemente mit dem Präfix "x:" angezeigt. So werden XAML-Sprachelemente in der Regel in realem Markup verwendet. Diese Konvention wird in der Dokumentation für XAML und auch in der XAML-Spezifikation befolgt.

## <a name="other-xaml-primitives"></a>Andere XAML-Grundtypen

In der XAML 2009-Spezifikation werden weitere XAML-Grundtypen auf Sprachebene aufgeführt, etwa **x:Uri** und **x:Single**. Falls diese nicht in der Tabelle in diesem Artikel aufgeführt sind, werden andere XAML-Grundtypen auf Sprachebene, die in einem anderen XAML-Vokabular oder in der XAML-Spezifikation von 2009 definiert sind, derzeit von XAML für die Windows-Runtime nicht unterstützt.

**Hinweis**    Datums-und Uhrzeitangaben (Eigenschaften, die [**DateTime**](/uwp/api/Windows.Foundation.DateTime) oder [**DateTimeOffset**](/dotnet/api/system.datetimeoffset), [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan) oder [**System. TimeSpan**](/dotnet/api/system.timespan)verwenden) können nicht mit einem XAML-primitiv festgelegt werden. Diese Eigenschaften können in XAML grundsätzlich nicht festgelegt werden, da im XAML-Parser der Windows-Runtime kein Standardverhalten für die Konvertierung der Ausgangszeichenfolge für Datums- und Uhrzeitangaben vorhanden ist. Für Initialisierungswerte von Datums- und Uhrzeiteigenschaften müssen Sie CodeBehind verwenden, der ausgeführt wird, wenn eine Seite oder ein Element geladen wird.

## <a name="related-topics"></a>Zugehörige Themen

* [Übersicht über XAML](xaml-overview.md)
* [Anleitung zur XAML-Syntax](xaml-syntax-guide.md)
* [Storyboardanimationen](../design/motion/storyboarded-animations.md)
 