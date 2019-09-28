---
description: Listet Unterstützung auf Sprachebene in XAML für die Windows-Runtime für bestimmte Datentypen in der Common Language Runtime (CLR) und in anderen Programmiersprachen wie C++ auf.
title: Systeminterne XAML-Datentypen
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19f297e731225d28f92f63ad9359bba70f0f0b32
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340444"
---
# <a name="xaml-intrinsic-data-types"></a>Systeminterne XAML-Datentypen


XAML für die Windows-Runtime stellt auf Sprachebene Unterstützung für mehrere Datentypen bereit, die häufig verwendete Grundtypen in der Common Language Runtime (CLR) und in anderen Programmiersprachen wie C++ sind.

Am häufigsten finden Sie die Verwendung systeminterner XAML-Datentypen, wenn Ressourcen in einem XAML-Ressourcenwörterbuch definiert werden. Sie können in diesem Wörterbuch Konstanten definieren, z. B. Zahlen, die Sie für mehrere Werte verwenden. Sie können auch eine Storyboardanimation verwenden, um eine Animation mithilfe eines Zeichenfolgen- oder booleschen Werts zu erreichen. Sie benötigen dann ein XAML-Objektelement, das die Zeichenfolge oder den booleschen Wert darstellt, um den Keyframe der [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Definition aufzufüllen. Die Windows-Runtime-XAML-Standardvorlagen verwenden beide Techniken.

XAML für die Windows-Runtime bietet Unterstützung auf Sprachebene für diese Typen:

| XAML-Grundtyp | Beschreibung |
|-------|-------------|
| **x:Boolean**  | Für die CLR-Unterstützung: [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean). XAML analysiert Werte für **x:Boolean** ohne Berücksichtigung der Groß-/Kleinschreibung. „x:Bool“ ist keine zulässige Alternative. |
| **x:String**   | Für die CLR-Unterstützung: [**String**](https://docs.microsoft.com/dotnet/api/system.string). Die Codierung für die Zeichenfolge wird standardmäßig auf die umgebende XML-Codierung festgelegt. |
| **x:Double**   | Für die CLR-Unterstützung: [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Zusätzlich zu den numerischen Werten lässt die Textsyntax für **x:Double** das Token „NaN“ zu. So kann „Auto“ für das Layoutverhalten als Ressourcenwert gespeichert werden. Bei den Token wird die Groß-/Kleinschreibung berücksichtigt. Sie können die wissenschaftliche Schreibweise verwenden (also beispielsweise „1+E06“ für `1,000,000`). |
| **x:Int32**    | Für die CLR-Unterstützung: [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32). **x:Int32** wird als Zahl mit Vorzeichen behandelt. Sie können das Minuszeichen („-“) für eine negative ganze Zahl verwenden. In XAML wird das Fehlen eines Vorzeichens in der Textsyntax als positiver Wert mit Vorzeichen interpretiert. |

Diese Grundtypen der Programmiersprache XAML sind im Allgemeinen die einzigen Fälle, in denen Sie ein Objektelement definieren, das das Präfix **x:** in XAML verwendet. Alle anderen XAML-Sprachfeatures werden in der Regel in Attributform oder als Markuperweiterung verwendet.

**Beachten Sie**  BY-Konvention, die sprach primitiven für XAML und alle anderen XAML-Sprachelemente werden mit dem Präfix "x:" angezeigt. So werden XAML-Sprachelemente in der Regel im echten Markup verwendet. Diese Konvention wird in der Dokumentation für XAML und auch in der XAML-Spezifikation befolgt.

## <a name="other-xaml-primitives"></a>Andere XAML-Grundtypen

In der XAML 2009-Spezifikation werden weitere XAML-Grundtypen auf Sprachebene aufgeführt, etwa **x:Uri** und **x:Single**. Falls diese nicht in der Tabelle in diesem Artikel aufgeführt sind, werden andere XAML-Grundtypen auf Sprachebene, die in einem anderen XAML-Vokabular oder in der XAML-Spezifikation von 2009 definiert sind, derzeit von XAML für die Windows-Runtime nicht unterstützt.

**Beachten Sie**   Datumsangaben und Uhrzeiten (Eigenschaften, die [**DateTime**](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime) oder [**DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset), [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) oder [**System. TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan)verwenden) nicht mit einem XAML-primitiven festgelegt werden können. Diese Eigenschaften können in XAML grundsätzlich nicht festgelegt werden, da im XAML-Parser der Windows-Runtime kein Standardverhalten für die Konvertierung der Ausgangszeichenfolge für Datums- und Uhrzeitangaben vorhanden ist. Für Initialisierungswerte von Datums- und Uhrzeiteigenschaften müssen Sie CodeBehind verwenden, der ausgeführt wird, wenn eine Seite oder ein Element geladen wird.

## <a name="related-topics"></a>Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Leitfaden zur XAML-Syntax](xaml-syntax-guide.md)
* [Storyboarding-Animationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

