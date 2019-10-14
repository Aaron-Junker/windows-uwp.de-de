---
Description: Mit dem Auswahlmodus können Benutzer ein einzelnes oder mehrere Elemente auswählen und dafür Aktionen vornehmen.
title: Auswahlmodus
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 9625f8fb86ff6473ac2851e600bc02e996993660
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165097"
---
# <a name="selection-mode-overview"></a>Auswahlmodus (Übersicht)

Mit dem Auswahlmodus können Benutzer ein einzelnes oder mehrere Elemente auswählen und dafür Aktionen vornehmen. Er kann über ein Kontextmenü aufgerufen werden, indem Sie bei gedrückter STRG- oder UMSCHALTTASTE auf ein Element klicken oder in einer Fotogalerieansicht bei einem Element auf ein Ziel zeigen. Wenn der Auswahlmodus aktiviert ist, werden Kontrollkästchen neben jedem Listenelement angezeigt, und Aktionen können am oberen oder unteren Bildschirmrand angezeigt werden.

## <a name="different-selection-modes"></a>Verschiedene Auswahlmodi
Es gibt drei verschiedene Auswahlmodi:

- Single (Einzelauswahl): Dabei kann der Benutzer jeweils nur ein Element auswählen.
- Multiple (Mehrfachauswahl): Der Benutzer kann mehrere Elemente ohne Modifizierer auswählen.
- Extended (Erweiterte Auswahl): Dabei kann der Benutzer mit Zusatztasten mehrere Elemente auswählen, z.B. durch Gedrückthalten der UMSCHALTTASTE.

## <a name="selection-mode-examples"></a>Beispiele für Auswahlmodi
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>Im Folgenden findest du eine ListView mit aktiviertem Einzelauswahlmodus.
![ListView mit Einzelauswahlmodus](images/listview-selection-single.png)

Das Element „Art Venere“ ist ausgewählt, und aktuell wird mit dem Mauszeiger auf das Element „Mitsue Tollner“ gezeigt.

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>Im Folgenden findest du eine ListView mit aktiviertem Mehrfachauswahlmodus:
![ListView mit Mehrfachauswahlmodus](images/listview-selection-multiple.png)

Es sind drei Elemente ausgewählt, und es wird mit dem Mauszeiger auf das Element „Donette Foller“ gezeigt.

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>Im Folgenden findest du eine GridView mit aktiviertem Einzelauswahlmodus:
![Rasteransicht mit Einzelauswahlmodus](images/gridview-selection-single.png)

Element 7 ist ausgewählt, und aktuell wird mit dem Mauszeiger auf das Element 3 gezeigt.

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>Im Folgenden findest du eine GridView mit aktiviertem Mehrfachauswahlmodus:
![Rasteransicht mit Mehrfachauswahlmodus](images/gridview-selection-multiple.png)

Elemente 2, 4 und 5 sind ausgewählt, und aktuell wird mit dem Mauszeiger auf das Element 7 gezeigt.

## <a name="behavior-and-interaction"></a>Verhalten und Interaktion
Durch Tippen auf ein Element wird es ausgewählt. Das Tippen auf die Aktion auf der Befehlsleiste wirkt sich auf alle ausgewählten Elemente aus. Wenn kein Element ausgewählt ist, sind die Aktionen auf der Befehlsleiste mit Ausnahme von „Alle auswählen“ in der Regel inaktiv.

Im Auswahlmodus funktioniert das einfache Ausblenden nicht. Wenn Sie außerhalb des Frames tippen, in dem der Auswahlmodus aktiv ist, wird der Modus nicht beendet. Dadurch wird die unbeabsichtigte Deaktivierung des Modus verhindert. Per Klick auf die Zurück-Schaltfläche wird der Mehrfachauswahlmodus beendet.

Wenn eine Aktion ausgewählt wurde, wird eine visuelle Bestätigung angezeigt. Ziehen Sie die Anzeige eines Bestätigungsdialogfelds für bestimmte Aktionen in Erwägung, insbesondere bei destruktiven Aktionen wie Löschen.

Der Auswahlmodus ist auf die Seite beschränkt, auf der er aktiv ist. Er wirkt sich nicht auf Elemente außerhalb dieser Seite aus.

Der Einstiegspunkt für den Auswahlmodus sollte neben dem Inhalt platziert werden, auf den er sich auswirkt.

Empfehlungen für die Befehlsleiste finden Sie unter [Richtlinien für Befehlsleisten](app-bars.md).

Weitere Informationen zu Auswahlmodi und zur Behandlung von Auswahlereignissen für bestimmte Steuerelemente findest du auf der Anleitungsseite für das jeweilige Steuerelement.
