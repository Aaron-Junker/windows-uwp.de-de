---
title: Vorhandensein von Rich-Richtlinien und Einschränkungen
description: Informationen Sie zu Richtlinien und Einschränkungen des Systems Xbox Live umfassender vorhanden.
ms.assetid: 0ad21a75-0524-45a8-8d8a-0dec0f7d6d6f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, umfassender vorhanden, Richtlinien
ms.localizationpriority: medium
ms.openlocfilehash: f85974c0ccb38f3c33fb214ddaf24b98dd8dcdd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643595"
---
# <a name="rich-presence-policies-and-limitations"></a>Vorhandensein von Rich-Richtlinien und Einschränkungen

Wenn Sie Rich-Anwesenheit für den Titel implementieren, müssen Sie die folgenden Richtlinien und Einschränkungen erfüllen.

-   Jeder Titel müssen mindestens 1 Zeichenfolge-Set, aber es gibt keine Obergrenze für wie viele Zeichenfolgen-Mengen, die Sie verwenden können.
-   Definieren Sie eine Standardzeichenfolge als auch für neutrale Kultur-Zeichenfolgen für jede Enumeration und für jede Zeichenfolge umfassender vorhanden.
-   Sie können numerische oder Zeichenfolge Statistiken, um die Parameter in Ihrer Zeichenfolgen zu füllen. Sie können keine Statistiken für "DateTime".
-   Wenn Sie Statistiken in Ihre Anwesenheit von Rich-Zeichenfolgen verwenden, müssen diese Statistiken, (z. B. alle Enumerationen für Statistiken) in der gleichen SCID & Sandbox verfügbar sein.
-   Sie haben es sich um 1 Zeile 44 Zeichen insgesamt (einschließlich der Werte der Parameter). Dies ähnelt Grenzwerte für Xbox 360 umfassender vorhanden. Wir arbeiten mit dem Client, um festzustellen, ob die Länge der Zeichenfolge annehmen kann. Es wird eine Ankündigung vorhanden sein, wenn die Zeichenfolge länger sein kann.
    -   Unicode-Zeichen sind erforderlich und müssen in der Lage, arbeiten mit UTF-8-Codierung für die Anzeige.
-   Die Anzeigenamen müssen die folgenden Regeln einhalten:
    -   Die zulässigen Zeichen sind "A" bis "Z", "a" bis "Z", einem Unterstrich ("\_"), und "0" bis "9".

        Es unterliegt keiner zeichenbeschränkung.

-   Die Zeichenfolgen erfolgt keine Überprüfung der Zeichenfolge; Sie müssen die Zeichenfolge Überprüfung, wie z. B. Rechtschreibprüfung und sicherstellen, dass die Zeichenfolge ordnungsgemäß lokalisiert wurde.
    -   Wenn wir den Eindruck, dass eine Zeichenfolge-Set (z. B. als beleidigend oder anstößige Sprache) nicht geeignet ist haben, wird verhindert, dass Microsoft-Publikationen von mit umfassender vorhanden, bis Zeichenfolgen zu unseren Zufriedenheit aktualisiert wurden.
-   Wenn Ihre Titel in die Data-Plattform integrieren ist nicht, gibt es keine Optionen für die Verwendung von Statistiken in Zeichenfolgen als Parameter.
    -   Alle Zeichenfolgen muss in diesem Fall vollständig vordefinierte (keine Token sind zulässig).
-   Namen der Enumeration in allen Enumerationen eindeutig sein und müssen eindeutig sein, um Namen von Statistiken.
-   Wenn eine Zeile überschreitet die Anzahl der Zeichen, die angezeigt werden kann und ein Zeilenumbruch vorhanden ist, wird die Zeile automatisch abgeschnitten.
