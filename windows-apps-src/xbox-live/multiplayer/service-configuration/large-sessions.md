---
title: Umfangreiche Sitzungen
description: Informationen Sie zur Verwendung von großer Sitzungen mit Xbox Live-Multiplayer-Plattform.
ms.date: 07/11/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, mehrere Spieler, große "Session", letzte Spieler
ms.localizationpriority: medium
ms.openlocfilehash: dcd7b27bc0aea8b8406e7eccb420140cf32c1ed5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618545"
---
# <a name="large-sessions"></a>Umfangreiche Sitzungen

Wenn Sie eine Multiplayer-Sitzung, die mehr als 100 Mitglieder verarbeiten kann benötigen, müssen Sie verwenden, was als große Sitzung bekannt ist. Dieses Szenario ist die am häufigsten verwendete online (MMO) vollbringen und Broadcasts (wobei die meisten Member für die Zuschauer sind), aber möglicherweise Anwendungen in andere Formate sowie Spiele.

In einigen Fällen auch möchten große Sitzungen verwenden, auch beim Umgang mit kleineren Gruppen der Spieler. Wenn Sie möchten mehrere Spieler, um in der gleichen Sitzung werden, jedoch nicht notwendigerweise Sensibilisierung für einander bei miteinander in Spiel auftretenden nicht, können Sie die Eigenschaft "auf trifft", große Sitzungen.

Große Sitzungen werden derzeit nicht unterstützt von [Xbox integrierte Multiplayer-Spiele (XIM)](../xbox-integrated-multiplayer.md) oder [Multiplayer-Manager (MPM)](../multiplayer-manager.md), sodass Sie die Multiplayer-2015-APIs verwenden müssen, verwenden Sie direkte Aufrufe der Multiplayer-Spiele Dienstverzeichnis (MPSD).

Große Sitzungen werden geringfügig anders, als reguläre Sitzungen behandelt:

* Enthält weniger Informationen als reguläre Sitzungen, aber ist effizienter.
* Unterstützt bis zu 10.000 Elemente.
* Sie können nicht mit einer großen Sitzung abonnieren.
* Es gibt keine automatische Einbindung in die aktuellen Listen der Player für Mitglieder einer großen Sitzung.

## <a name="recent-players"></a>Letzte Spieler

Die Funktionen von Xbox Live ist, dass wenn Xbox Live-Player Multiplayer-Spiele für neue Personen spielen, nach dem das Spiel die Spieler auf ihrem Dashboard im sehen sie können die **letzte Spieler** Liste. Wenn ein Player eine großartige Erfahrung mit einem neuen Player in einem Spiel haben, sollten sie erneut mit ihnen spielen oder deren hinzufügen als ein Friend. Wenn sie eine schlechte benutzererfahrung mit einer Player haben, sollten sie in der Zukunft vermieden bzw. melden das fehlerhafte Verhalten, nachdem das Spiel beendet ist.

In regulären Sitzungen fügt Xbox Live automatisch Spieler in der gleichen Sitzung die Liste der zuletzt geöffneten Spieler. Wenn Sie jedoch große Sitzungen verwenden, müssen Sie einige zusätzlichen Schritte ausführen, um sicherzustellen, dass die aktuellen Listen der Player ordnungsgemäß aufgefüllt werden ausführen.

## <a name="set-up-a-large-session"></a>Eine große Sitzung einrichten

Um Sitzungen so groß einzurichten, fügen `“large”: true` zum Abschnitt mit Funktionen in der sitzungsvorlage. Mit der Sie das Festlegen der `maxMembersCount` bis zu 10.000. Wie Sie eine sitzungsvorlage der im folgenden sollten funktionieren:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>Arbeiten mit großen Sitzungen

Beim Schreiben von großer Sitzungen in MPSD, empfehlen wir, dass Sie tun, nicht um 10 Schreibvorgänge pro Sekunde zu überschreiten. Dies ist normalerweise eine 1000-Player-Sitzung mit einem Schreibschutz alle 2 Minuten durchschnittlich pro Player (z. B. beitreten/verlassen).

Andere Eigenschaften sollten nicht in den großen Sitzungen beibehalten werden.

### <a name="associating-players-from-the-same-large-session"></a>Zuordnen der Spieler in der gleichen große-Sitzung

Wenn Sie eine große Sitzung aus MPSD abrufen, die Liste der Member nicht hochfährt, mit der Antwort, und in der Tat besteht keine Möglichkeit, um die vollständige Liste zu erhalten. Stattdessen ist der Aufrufer in der Sitzung, deren memberdatensatz verkürzt werden kann nur in der Sammlung "Member" bezeichnetes als "me" (wie in der Anforderung).

Dies bedeutet, dass Clients Member werden nur in der Lage, ihre eigenen Eintrag in der Sitzung zu aktualisieren, und verlässt sich auf dem Server für diese ein allgemeiner Bezeichner bereit, mit dem Xbox Live Player zuordnen, die zusammen wiedergegeben.

Es gibt zwei Möglichkeiten, um anzugeben, dass Personen in einer Sitzung gemeinsam (zum Aktualisieren von Ruf und aktueller Status der Spieler) wiedergegeben.

#### <a name="1-persistent-groups"></a>1. Persistente Gruppen

Wenn eine Gruppe von Personen zusammen fortlaufend, möglicherweise mit Personen, die ein- und ausgehende, bleiben wird erhalten Sie der Gruppe einen Namen (z. B. eine Guid – befolgen die gleichen Benennungsregeln wie bei regulären Sitzungen).  Wie jedes Element stammt und wird aus der Gruppe, sollten sie hinzufügen oder entfernen den Gruppennamen, um ihre eigenen Groups-Eigenschaft, die ein Array von Zeichenfolgen ist:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. Kurze entdeckt

Wenn zwei Personen eine kurze einmalige auftreten können kann das Spiel stattdessen "auf trifft" Array verwenden. Geben Sie jedes auftreten, einen Namen und nach dem auftreten würden beide (oder alle) der Teilnehmer den Namen ihre eigenen "erkennt"-Eigenschaft schreiben:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

Sie können den gleichen Namen für "Groups" und "erkennt" verwenden – z. B. wenn ein einziger Player "mit eine Gruppe Klassen", die Personen in der Gruppe müssen nicht noch nicht sonderlich (sofern sie den Gruppennamen, die "Groups" vorher hinzugefügt) und die Person, die hatte den auftreten würden, u nforderungen den Gruppennamen in der Liste "erkennt". Die bewirkt, dass der Person, die alle Member der Gruppe als letzte Spieler und umgekehrt finden Sie unter.

Die Anzahl der gefundenen als Mitglied der Gruppe für 30 Sekunden. Da die gefundenen einmalige Ereignisse betrachtet werden, ist das Array "auf trifft" immer sofort verarbeitet und anschließend aus der Sitzung gelöscht.  Sie wird nie in einer Antwort angezeigt.  (Das Array "Groups" sticks rund um bis geändert oder entfernt, oder das Element wird die Sitzung.)
