---
title: Umfassende Präsenz
description: Erfahren Sie, wie Xbox Live umfassender vorhanden Ihre Titel höher stufen.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, umfassender vorhanden
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604115"
---
# <a name="rich-presence"></a>Umfassende Präsenz

Mit umfassender vorhanden, kann Ihr Spiel ankündigen ein Spieler jetzt Wozu dient. Z. B. Ihr Spiel können umfangreiche Anwesenheit Zeichenfolgen aller Spieler den Status des Spiels Spieler, z. B. zeigen *Away*. Umfangreiche Anwesenheitsinformationen ist für die es Spielern ermöglichen, die Verbindung zu Xbox Live sichtbar. Im Idealfall erfahren eine umfassende Anwesenheitsfunktionen Zeichenfolge andere Spieler Xbox Live, wozu dient ein Spieler und, in dem Ihr Spiel der Spieler ihn ausführen. Das Konzept der Anwesenheit von Rich-Zeichenfolgen entspricht dem auf der Xbox One auf der Xbox 360 war, aber die neue Implementierung folgt die Unterhaltung als eine Dienst-Initiative. In die Themen in diesem Abschnitt wird beschrieben, wie Ihre Anwesenheit von Rich-Zeichenfolgen zu konfigurieren und wie Sie die Zeichenfolge für einen Benutzer, die Wiedergabe des Titels festlegen.


## <a name="definitions"></a>Definitionen

**Enumerationen**  
Eine Enumeration ist eine Liste mit einer in-Game-Dimension. Beispiele für diese Dimensionen im Spiel sind Waffen, Zeichenklassen in regulären Ausdrücken, Zuordnungen und So weiter. Wir möchten eine Liste der möglichen Waffen in Ihrem Spiel, eine Liste aller möglichen Zeichenklassen in regulären Ausdrücken oder Zuordnungen angezeigt, und so weiter.

**Gebietsschema-Paar**  
Jede mögliche umfassender vorhanden Zeichenfolge müssen ein Gebietsschema aus, um anzugeben, in welchen Regionen die Zeichenfolge verwendet werden kann/sollte zugeordnet. Jeder Enumeration müssen auch einen Satz von gebietsschemazeichenfolge Paaren ebenfalls.

**String-set**  
Eine Zeichenfolge-Gruppe besteht aus einer Gruppe von gebietsschemazeichenfolge Paaren. Dieser Satz definiert die möglichen Werte einer Zeichenfolge umfassender vorhanden, für alle möglichen Gebietsschemas oder die möglichen Werte für eine Enumeration für alle möglichen Gebietsschemas.

**Anzeigenamen**  
Es gibt zwei Arten von Anzeigenamen:

**Vorhandensein von Rich-Zeichenfolge**  
Der Anzeigename für einen Zeichenfolge-Satz ist ein eindeutiger Bezeichner in Form einer Zeichenfolge verwendet, um einen Zeichenfolge-Satz zu verweisen.

**Enumeration**  
Diese Anzeigenamen werden verwendet, um eine bestimmte Enumeration wie die Enumeration Waffen oder die Zeichen Klasse Enumeration eindeutig zu identifizieren.


## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[Vorhandensein von Rich-Konfiguration](rich-presence-strings-configuration.md)  
Vorgehensweise: Konfigurieren von umfassender vorhanden für die Verwendung in Ihrem Titel.

[Umfassender vorhanden, die Zeichenfolgen aktualisieren](rich-presence-strings-updating-strings.md)  
Wie Sie umfangreiche Anwesenheit Zeichenfolgen aus dem Titel des zu aktualisieren.

[Bewährte Methoden für Rich-Präsenz](rich-presence-strings-best-practices.md)  
Bewährte Methoden für die umfassende Anwesenheit in Titel des verwenden.

[Vorhandensein von Rich-Richtlinien und Einschränkungen](rich-presence-strings-policies-and-limitations.md)  
Die Richtlinien zur Verwendung von Rich-Präsenz in den Titel.

[Vorhandensein von Rich-Anhang](rich-presence-strings-appendix.md)  
Weitere Beispiele und Informationen über die Datenplattform, die relevant für umfangreiche vorhanden.

[Programmierung Xbox Live umfassender vorhanden](programming-rich-presence.md)  
Veranschaulicht, wie umfangreiche Anwesenheit mit Xbox Live.
