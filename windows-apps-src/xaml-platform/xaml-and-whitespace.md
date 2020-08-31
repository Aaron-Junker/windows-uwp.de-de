---
description: Erfahren Sie mehr über die Regeln, die von XAML für die Verarbeitung des Leerraums, der Zeilenvorschub und der Registerkarte verwendet werden.
title: XAML und Leerzeichen
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 16d5b0b3faa4d356ced2eb352192bd1a91882475
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094627"
---
# <a name="xaml-and-whitespace"></a>XAML und Leerzeichen


Informationen zu von XAML verwendeten Regeln für die Leerzeichenverarbeitung.

## <a name="whitespace-processing"></a>Leerzeichenverarbeitung

In Übereinstimmung mit XML sind Leerzeichen in XAML Leerzeichen, Zeilenvorschub und Tabulator. Diese entsprechen den Unicode-Werten 0020, 000A bzw. 0009. Standardmäßig wird diese Leerzeichennormalisierung angewendet, wenn ein XAML-Verarbeiter auf internen Text zwischen Elementen in einer XAML-Datei stößt:

-   Zeilenvorschubzeichen zwischen ostasiatischen Zeichen werden entfernt.
-   Alle Leerraumzeichen (Leerzeichen, Zeilenvorschub, Tabulator) werden in Leerzeichen konvertiert.
-   Alle aufeinander folgenden Leerzeichen werden gelöscht und durch ein Leerzeichen ersetzt.
-   Ein Leerzeichen unmittelbar nach dem Starttag wird gelöscht.
-   Ein Leerzeichen unmittelbar vor dem Endtag wird gelöscht.
-   Mit *ostasiatischen Zeichen* sind die Unicode-Zeichenbereiche „U+20000 bis U+2FFFD“ und „U+30000 bis U+3FFFD“ gemeint. Diese Teilmenge wird manchmal auch als *cjk-ideograph*bezeichnet. Weitere Informationen finden Sie unter http://www.unicode.org.

„Standard“ entspricht dem Zustand, der durch den Standardwert des **xml:space** -Attribut bezeichnet wird.

### <a name="whitespace-in-inner-text-and-string-primitives"></a>Leerzeichen in internem Text und Zeichenfolgen-Grundtypen

Die oben angegebenen Normalisierungsregeln gelten für internen Text innerhalb von XAML-Elementen. Nach der Normalisierung konvertiert ein XAML-Prozessor sämtlichen internen Text gemäß den folgenden Regeln in einen passenden Typ:

-   Wenn es sich beim Typ der Eigenschaft nicht um eine Sammlung, aber auch nicht direkt um einen **Object**-Typ handelt, versucht der XAML-Prozessor, mithilfe des eigenen Typkonverters eine Konvertierung in diesen Typ auszuführen. Ist diese Konvertierung nicht erfolgreich, tritt ein XAML-Analysefehler auf.
-   Wenn es sich beim Typ der Eigenschaft um eine Sammlung handelt und der interne Text zusammenhängend ist (also keine zusätzlichen Elementtags enthält), wird der interne Text als einzelner **String** analysiert. Wenn der Sammlungstyp keinen **String** akzeptiert, tritt ebenfalls ein XAML-Analysefehler auf.
-   Wenn es sich um eine Eigenschaft vom Typ **Object** handelt, wird der interne Text als einzelner **String** analysiert. Sind zusätzliche Elementtags enthalten, tritt ein XAML-Analysefehler auf, da der Typ **Object** für ein einzelnes Objekt steht (**String** oder ein anderes Objekt).
-   Wenn es sich beim Typ der Eigenschaft um eine Sammlung handelt und der interne Text nicht zusammenhängend ist, wird die erste Teilzeichenfolge in einen **String** konvertiert und als Sammlungselement hinzugefügt. Das Zwischenelement wird ebenfalls als Sammlungselement hinzugefügt, und schließlich wird der Sammlung die nachgestellte Teilzeichenfolge (sofern vorhanden) als drittes **String**-Element hinzugefügt.

### <a name="whitespace-and-text-content-models"></a>Leerzeichen und Textinhaltsmodelle

In der Praxis ist der Erhalt von Leerzeichen nur bei einem Teil der möglichen Inhaltsmodelle von Bedeutung. Zu diesem Teil gehören Inhaltsmodelle, die ein Singleton vom Typ **String** in beliebiger Form, eine dedizierte Sammlung mit Elementen vom Typ **String** oder eine Mischung aus **String** und anderen Typen in Listen, Sammlungen oder Wörterbüchern akzeptieren.

Sogar für Inhaltsmodelle, die Zeichenfolgen unterstützen, besteht das Standardverhalten innerhalb dieser Inhaltsmodelle darin, dass alle verbleibenden Leerräume als nicht signifikant behandelt werden.

### <a name="preserving-whitespace"></a>Beibehalten von Leerzeichen

Leerzeichen im Quell-XAML-Code können für die spätere Darstellung auf unterschiedliche Arten beibehalten werden. Diese sind von der XAML-Leerzeichennormalisierung des XAML-Verarbeiters nicht betroffen.

`xml:space="preserve"`: Geben Sie dieses Attribut auf der Ebene des Elements an, für das die Leerzeichen erhalten bleiben sollen. Beachten Sie, dass dadurch alle Leerzeichen erhalten bleiben, einschließlich derer, die möglicherweise von Codeeditoren oder Entwurfsoberflächen hinzugefügt werden, um Markupelemente als visuell intuitive Schachtelung auszurichten. Ob diese Leerzeichen gerendert werden ist ebenfalls wieder eine Frage des Inhaltsmodells für das Containerelement. Wir raten davon ab, `xml:space="preserve"` auf Stammebene anzugeben, da die Mehrzahl an Objektmodellen Leerzeichen in der Regel nicht als signifikant ansehen. Es ist besser, das Attribut nur auf der Ebene von Elementen spezifisch festzulegen, die Leerräume in Zeichenfolgen rendern oder leeraumsignifikate Auflistungen sind.

Entitäten und geschützte Leerzeichen: In XAML kann eine beliebige Unicodeentität innerhalb eines Textobjektmodells angegeben werden. Sie können dedizierte Entitäten wie geschützte Leerzeichen (in UTF-8-Codierung) verwenden. Sie können auch Rich-Text-Steuerelemente verwenden, die geschützte Leerzeichen unterstützen. Gehen Sie mit Bedacht vor, wenn Sie Layoutmerkmale wie Einzüge mithilfe von Entitäten simulieren, da die Laufzeitausgabe der Entitäten von einer größeren Anzahl von Faktoren abhängt als bei den allgemeinen Layoutkomponenten, beispielsweise der ordnungsgemäßen Verwendung von Bereichen und Rändern.

