---
title: Ressourcen
description: Eine Ressource ist ein Bereich im Speicher, auf den die Direct3D-Pipeline zugreifen kann.
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- Ressourcen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c31dcbcc3019538d769118b018c693174b17b4c7
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "8880689"
---
# <a name="resources"></a>Ressourcen


Eine Ressource ist ein Bereich im Speicher, auf den die Direct3D-Pipeline zugreifen kann. Damit die Pipeline effizient auf den Speicher zugreifen kann, müssen für die Pipeline bereitgestellte Daten (wie z.B. Input-Geometrie, Shader-Ressourcen und Texturen) in einer Ressource gespeichert werden. Es gibt zwei Arten von Ressourcen, aus denen alle Direct3D-Ressourcen abgeleitet sind: ein Puffer oder eine Textur. Bis zu 128 Ressourcen können für jede Pipeline-Phase aktiv sein.

Jede Anwendung wird in der Regel viele Ressourcen erstellen. Beispiele für Ressourcen sind unter anderem: Vertexpuffer, Indexpuffer, Konstantenpuffer, Texturen und Shader-Ressourcen. Es gibt mehrere Optionen, die bestimmen, wie Ressourcen verwendet werden können. Sie können Ressourcen erstellen, die stark typisiert sind oder weniger typisieren; sie können steuern, ob Ressourcen sowohl Lese- als auch Schreibzugriff erhalten; sie können Ressourcen nur für die CPU, nur die GPU oder beide zugänglich machen. Natürlich wird es einen Geschwindigkeit-Funktionalität-Kompromiss geben - je mehr Funktionen Sie einer Ressource gewähren, desto weniger Leistung sollten Sie erwarten.

Da eine Anwendung oft viele Texturen verwendet, hat Direct3D ferner das Konzept eines Textur-Arrays, um die Texturverwaltung zu vereinfachen. Ein Texturarray enthält eine oder mehrere Texturen (alle desselben Typs und mit denselben Dimensionen), die von einer Anwendung oder von Shadern indiziert werden können. Durch Texturarrays können Sie eine einzelne Oberfläche mit mehreren Indizes zu verwenden, um auf viele Texturen zuzugreifen. Sie können beliebig viele Texturarrays zum Verwalten unterschiedlicher Texturtypen nach Bedarf erstellen.

Nachdem Sie die Ressourcen erstellt haben, die Ihre Anwendung verwenden wird, verbinden oder binden Sie jede Ressource mit der/an die Pipelinephasen, die diese verwenden. Dies geschieht durch Aufrufen einer Bindungs-API, die einen Zeiger auf die Ressource verwendet. Da mehr als eine Pipelinephase Zugriff auf dieselbe Ressource benötigen kann, hat Direct3D das Konzept einer Ressourcenansicht. Eine Ansicht identifiziert den Teil einer Ressource, auf den zugegriffen werden kann. Sie können *m* Ansichten oder eine Ressource erstellen und diese an *n* Pipelinephasen binden, vorausgesetzt, dass Sie die Bindungsregeln für freigegebene Ressourcen einhalten (die Runtime generiert Fehler zur Kompilierzeit, falls dies nicht der Fall ist).

Eine Ressourcenansicht bietet ein allgemeines Modell für den Zugriff auf eine Ressource (z.B. Texturen oder Puffer). Da Sie der Runtime über eine Ansicht mitteilen können, auf welche Daten diese wie zugreifen kann, können Sie durch Ressourcenansichten weniger Ressourcen typisieren. Das heißt Sie können Ressourcen für eine bestimmte Größe zur Kompilierzeit erstellen und dann den Datentyp in der Ressource erklären, wenn die Ressource an die Pipeline gebunden wird. Ansichten zeichnen sich durch viele Funktionen für die Verwendung von Ressourcen aus, z. B. die Fähigkeit, Tiefe-/Schablonenoberflächen im Shader wiederzugeben, das Generieren von dynamischen Cubemaps in einem Durchgang und das gleichzeitige Berechnen und Ausgeben mehrerer Segmente eines Volumens.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>In diesem Abschnitt


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="resource-types.md">Ressourcentypen</a></p></td>
<td align="left"><p>Die verschiedenen Ressourcentypen haben unterschiedliche Layouts (bzw. unterschiedliche Speicheranforderungen). Alle von der Direct3D-Pipeline verwendeten Ressourcen sind von zwei grundlegenden Ressourcentypen abgeleitet: <a href="resource-types.md#buffer-resources">Puffer</a> und <a href="resource-types.md#texture-resources">Texturen</a>. Ein Puffer ist eine Sammlung von Rohdaten (Elemente); eine Textur ist eine Sammlung von Texeln (Textur-Elementen).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">Auswählen einer Ressource</a></p></td>
<td align="left"><p>Eine Ressource ist eine Sammlung von Daten, die von der 3D-Pipeline verwendet werden. Das Erstellen von Ressourcen und das Definieren des Verhaltens ist der erste Schritt zur Programmierung Ihrer Anwendung. Dieses Handbuch enthält grundlegende Themen für die Auswahl der Ressourcen, die Ihre Anwendung erfordert.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">Kopieren und Zugreifen auf Ressourcendaten</a></p></td>
<td align="left"><p>Nutzungsflags geben an, wie die Anwendung Ressourcendaten nutzen will, um Ressourcen nach Möglichkeit in den leistungsfähigsten Speicherbereich zu platzieren. Ressourcendaten werden über Ressourcen kopiert, so dass die CPU- oder GPU darauf zugreifen kann, ohne eine Beeinträchtigung der Leistung herbeizuführen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">Texturansichten</a></p></td>
<td align="left"><p>Auf Texturressourcen wird in Direct3D mit einer Ansicht zugegriffen, bei der es sich um einen Mechanismus für die Hardware-Interpretation einer Ressource im Speicher handelt. Eine Ansicht ermöglicht einer bestimmten Pipelinephase den Zugriff auf die erforderlichen <a href="resource-types.md">Unterressourcen</a>, in der von der Anwendung gewünschten Darstellung.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Koordinatensysteme](coordinate-systems.md)

[Lernanleitung für Direct3D-Grafiken](index.md)

[Gleitkommaregeln](floating-point-rules.md)

[Konvertieren von Datentypen](data-type-conversion.md)
