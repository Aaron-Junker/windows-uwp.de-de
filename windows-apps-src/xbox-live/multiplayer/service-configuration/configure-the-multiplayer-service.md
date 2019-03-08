---
title: Multiplayer-Dienstkonfiguration
description: Erfahren Sie, wie Xbox Live Multiplayer-Dienst zu konfigurieren.
ms.assetid: d042d4d5-1c75-4257-8a6f-07eddd39ca7e
ms.date: 07/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Multiplayer-Spiele, Dienstkonfiguration, sitzungsvorlage, benutzerdefinierte einladungs-Zeichenfolge, Smartmatch hopper
ms.localizationpriority: medium
ms.openlocfilehash: bf829069824443cdc1c8c0658fcfdfcbe72d0b93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655875"
---
# <a name="multiplayer-service-configuration"></a>Multiplayer-Dienstkonfiguration
In der Reihenfolge für den Titel, um die Dienste nutzen, die Xbox Live bereitstellt, müssen Sie zuerst die Dienstkonfiguration definieren. Diese Dienstkonfiguration in der Xbox Live-Cloud-Dienst vorhanden ist, und definiert, wie die Xbox Live-Dienst mit Geräten interagiert, die Ihr Titel/Spiel ausgeführt werden.

Für multiplayer-Diensten gibt es drei Aspekte der Multiplayer-Spiele, die Sie konfigurieren können:
* Vorlagen für ereignissitzungen
* SmartMatch Hoppers
* Benutzerdefinierte einladungs-Zeichenfolgen

## <a name="session-templates"></a>Vorlagen für ereignissitzungen
Der Xbox-multiplayer-Dienst ermöglicht es Spielern ermöglichen, erstellen und Binden von Sitzungen, die zum Austauschen von sitzungsnachrichten mit anderen Spielern in der gleichen Sitzung und die Ergebnisse der die Wiedergabe der Sitzung zu senden. (Veröffentlichen der Ergebnisse der Sitzung bereinigt und aktualisiert außerdem den Bestenlisten für alle Spieler in der Sitzung.)

Z. B. möglicherweise eine Multiplayer-Sitzung ein einzelnes Spiel Schach zwischen zwei Spielern auszutauschen. Alternativ kann es werden einer Sitzung fortsetzen einer Aktion und adventure Titel, die durch eine viel größere Anzahl von Spielern gespielt.

Bei einem Spiel eine neue Sitzung erstellt wird, erstellt die Sitzung, die basierend auf einer sitzungsvorlage für vordefinierte. Diese Vorlage ist im Wesentlichen eine jsonobjekt, das Attribute enthält, die die Sitzung zu beschreiben.

Wenn Sie eine Vorlage für die neue Sitzung erstellen, müssen Sie Folgendes definieren:

| Feld | Beschreibung |
| --- | --- |
| Sitzungsname | Geben Sie einen Namen, die kennzeichnet, dass der Vorlage Multiplayer-Sitzung, und, die Sie leicht erkennen, und denken Sie daran. Der Name muss eine Zeichenfolge, mit maximal 100 Zeichen sein. |
| Contract-Version | Dieser Wert wird vom System automatisch aufgefüllt und gibt die aktuelle Systemversion des JSON-Vertrags. Bearbeiten Sie es nicht. |
| Sitzungsvorlage (JSON-Text) | Geben Sie die JSON-Daten, die beschreibt, die verschiedene Attribute einer Multiplayer-Sitzung zugeordnet. |

Weitere Informationen zu Multiplayer-Sitzung-Vorlagen, einschließlich der vielen vordefinierte Vorlagen, mit denen Sie als Grundlage für die JSON-Text, finden Sie unter [Multiplayer-Sitzung Vorlagen](session-templates.md).

> **Wichtig:** Nachdem Sie ein Titel endgültigen Zertifizierung erfolgreich ist, können vorhandene Multiplayer-Sitzungen im Titel nicht mehr geändert oder gelöscht werden.

## <a name="smartmatch-hoppers"></a>SmartMatch hoppers

Eine optionale Hinzufügung an den Xbox-multiplayer-Dienst ist die Xbox serverbasierte Vermittlungsdienst, das eine Methode der Gruppierung Spieler zusammen, die basierend auf Informationen bereitgestellt, die durch den Titel in Statistiken gespeichert oder basierend auf den Einstellungen des Benutzers bereitstellt, oder basierend auf Dienstqualität.

Da Xbox One Vermittlung Server-basiert ist, Benutzer können eine Anforderung an den Dienst bereitstellen, und Sie werden dann benachrichtigt werden später noch Mal, wenn eine Übereinstimmung gefunden wird. Das heißt: der Benutzer in Ihrem Titel warten, bis der Vermittlung-Prozess tritt nicht erzwungen – sie sind frei, um die Wiedergabe des einzelnen-Player-Teils des Titels oder sogar zum Wiedergeben von anderen Titeln und sich noch Kandidaten für die Vermittlung. Hierdurch entfällt die Notwendigkeit eine "kritische Masse" der Spieler erreichen, bevor Übereinstimmungen gefunden werden können.

Ein datenbankgesteuertes Hopper muss auf eine zuvor definierte sitzungsvorlage basieren.

Wenn Sie eine neue Vermittlung Hopper erstellen, müssen Sie Folgendes definieren:

| Feld | Beschreibung |
|---|---|
|Name| Geben Sie einen Namen, die die Vermittlung Hopper kennzeichnet, und, die Sie leicht erkennen, und denken Sie daran. Der Name muss eine Zeichenfolge, mit jeweils höchstens 140 Zeichen sein. |
| Min-Gruppengröße | Geben Sie die zulässige Mindestanzahl der Spieler. Mindestwert ist 1. |
| Max. Größe | Geben Sie die maximale zulässige Anzahl der Spieler. Maximalwert ist 256. |
| Erweiterung Zyklen sollten Regel werden. | Der Standardwert ist 3. Standardmäßig ist der Wert sollte nicht für normale Player Auffüllungen nicht geändert werden müssen. |
| Rang hopper | Ist eine Hopper als eine Hopper geordnet markiert ist kann Spieler in diese Hopper zusammen abgeglichen wird, auch wenn sie gegenseitig blockiert haben. Dies hilft beim verhindern, dass Personen versuchen, Playern mit größere Fähigkeiten zu vermeiden, indem Sie blockiert. |
| Die automatische Aktualisierung von Sitzung | Wenn dieses Feld aktiviert ist, Änderungen an der Sitzung Memberliste oder Elementen benutzerdefinierte Eigenschaften werden automatisch an ein zuvor übermittelten Ticket weitergegeben. |

> **Wichtig:** Nachdem Sie ein Titel endgültigen Zertifizierung erfolgreich ist, können vorhandene Vermittlung Hoppers im Titel nicht mehr geändert oder gelöscht werden.

## <a name="custom-invite-strings"></a>Benutzerdefinierte einladungs-Zeichenfolgen
Wenn Ihrem Titel, eine Einladung an einen Player zum Verknüpfen eines Multiplayer-Spiels sendet, können Sie auswählen, um eine Textzeichenfolge benutzerdefinierte einladen, statt die Standardzeichenfolge für die INVITE-Nachricht anzuzeigen.

Wenn Sie eine neue benutzerdefinierte Einladung Zeichenfolge erstellen, müssen Sie Folgendes definieren:

| Feld | Beschreibung |
|---|---|
| ID | Die ID des benutzerdefinierten einladen, Zeichenfolge, die zum Identifizieren der Zeichenfolge verwendet wird. "Custominvitestrings_" wird automatisch am Anfang Ihrer ID angefügt werden Max. 100 Zeichen |
| Wert | Der Text der benutzerdefinierten einladen, Zeichenfolge, die in Ihrer benutzerdefinierten Einladung Toast angezeigt wird. Max. 100 Zeichen |

## <a name="additional-information"></a>Weitere Informationen

Weitere Informationen zum Konfigurieren der multiplayer-Diensts finden Sie unter den folgenden Artikeln:

**Artikel** | **Beschreibung**
--- | ---
[Konfigurieren von Ihrem AppXManifest für Multiplayer-Spiele](configure-your-appxmanifest-for-multiplayer.md) | Beschreibt, wie so konfigurieren Sie eine UWP-AppXManifest-Datei mit dem Xbox Live Multiplayer-Dienst funktioniert.
[Vorlagen für mehrere Spieler ereignissitzungen](session-templates.md) | Bietet eine kurze Übersicht über Vorlagen von Multiplayer-Sitzung, und nennt einige Beispiele für Vorlagen, die Sie kopieren und ändern Sie für Ihre multiplayer-Sitzungen können.
[Sitzung Vorlage-Konstanten](session-template-constants.md) | Beschreibt die vordefinierte Elemente einer Vorlage Multiplayer-Sitzung.
[Große Sitzungen](large-sessions.md) | Beschreibt, wann und wie Sie große Sitzungen verwenden.
