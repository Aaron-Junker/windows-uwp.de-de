---
title: Hinzufügen von Domänencontroller-Unterstützung zu Xbox Live prefabs (Vorlagen)
description: Hinzufügen von Domänencontroller-Unterstützung zu Xbox Live prefabs (Vorlagen) mit der Xbox Live Unity-Plug-in
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Domänencontroller-Unterstützung
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622875"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>Hinzufügen von Domänencontroller-Unterstützung zu Xbox Live prefabs (Vorlagen)

> [!IMPORTANT]
> Die Xbox Live Unity-Plug-in unterstützt keine Erfolge oder online Multiplayer-Spiele und wird nur empfohlen, für die [Xbox Live Creators-Programm](../developer-program-overview.md) Member.

Unterstützung aller Unity-Plug-Ins Prefabs von Xbox Live Angabe Controller-Eingabe, im Inspektor.

Nehmen wir beispielsweise an einem spielobjekt bezeichnet man `UserProfile1` die basiert auf der `UserProfile` prefab. Wenn Sie möchten dieses spielobjekt Spieler 1 zu verknüpfen und diese Anmeldung der `A` auf ihre Xbox-Controller auf die Schaltfläche, die einfach schreiben `joystick 1 button 0` in die `Input Controller Button` Feld im Inspektor.

  ![Domänencontroller-Unterstützung in UserProfile-Prefab](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>Alle Prefab Controller Eingabefelder
### <a name="userprofile-prefab"></a>UserProfile-prefab
- **Eingabe-Controller-Schaltfläche:** Fügt ein, und meldet sich bei einem Xbox Live-Benutzer.

### <a name="social-prefab"></a>Soziale prefabs
- **Umschaltfläche Filter "Controller":** Schaltet den Filter, um entweder "Alle" Freunde oder "Online" Freunde zu zeigen.

### <a name="leaderboard-prefab"></a>Leaderboard-prefab
- **Erste Controller-Schaltfläche:** Wird für die erste Seite des Leaderboard-Einträge des Spielers.
- **Letzte Controller-Schaltfläche:** Wird für die letzte Seite des Leaderboard-Einträge des Spielers.
- **Schaltfläche "Weiter Controller":** Wird für die nächste Seite der Leaderboard-Einträge des Spielers.
- **Prev-Controller-Schaltfläche:** Nimmt den Spieler auf der vorherigen Seite des Leaderboard-Einträge.
- **Aktualisieren Sie Controller-Schaltfläche "":** Aktualisiert die Leaderboard-Ansicht.


### <a name="game-save-ui-prefab"></a>Spiel speichern UI-prefab
- **Generieren Sie neue Domänencontroller-Schaltfläche "":** Generiert eine neue ganze Zahl, Sichern Sie Daten an.
- **Speichern Sie die Schaltfläche "Daten-Controller":** Speichert die aktuellen Daten in den Speicher verbunden.
- **Laden Sie die Schaltfläche "Daten-Controller":** Lädt Daten, die derzeit im Speicher angeschlossen gespeichert.
- **Rufen Sie die Schaltfläche der Info-Controller:** Ruft Informationen über gespeicherte Container, in dem Speicher verbunden.
- **Löschen Sie die Schaltfläche für Layoutcontainer Controller:** Löscht den gespeicherten Container aus dem Speicher verbunden

## <a name="xbox-controller-button-mappings"></a>Zuordnungen für Xbox-Controller-Schaltfläche

Für die Xbox-Controller Schaltfläche-Zuordnungen in Unity finden Sie in diesem [Unity-Controller-Wiki-Seite](https://wiki.unity3d.com/index.php?title=Xbox360Controller).
