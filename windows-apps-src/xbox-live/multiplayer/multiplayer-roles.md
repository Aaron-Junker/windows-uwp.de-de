---
title: Multiplayer-Rollen
description: Erfahren Sie mithilfe von Rollen um in Multiplayer Xbox Live Player-Rollen zu definieren.
ms.date: 06/29/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox one multiplayer, Rollen
ms.localizationpriority: medium
ms.openlocfilehash: ac5e7758bd8e068681d1c8dab2d47d11374c2616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662085"
---
# <a name="roles"></a>Rollen

Für einige spielen-Sitzungen empfiehlt es sich um anzugeben, dass bestimmte Member bestimmte Gaming-Rollen, z. B. Unterstützung, Medic, davon usw. haben. Sie können außerdem Wat, zum Spiel Slots für Spieler zu reservieren, die eine Rolle bestimmte Gameplay füllen. Mithilfe der Funktion für die Xbox Live-Rollen zu verwenden, kann der Dienst nachverfolgen, welche Spieler, welche Gaming-Rollen zugewiesen sind, und erzwingen eine maximale Anzahl der Spieler, die eine bestimmte Gaming-Rolle auswählen können.

Die häufigste Verwendung von Rollen ist für diese Sitzung Spiele spielen bestimmte Rollen zu bestimmen. Beispielsweise haben Sie ein Spielmodus, die zwischen 1 und 2 Unterstützungsklassen, mindestens 1 Tank/Heavy-Klasse und nicht mehr als 5 davon Klassen erforderlich sind.

In ein anderes mögliches Szenario empfiehlt es sich um anzugeben, dass eine Spiele-Sitzung genau 4 Spielern, bis zu 8 Zuschauer und 1 Announcer haben kann.

Sie können Rollen auch verwenden, um Slots für Freunde zu reservieren, beim Ausfüllen der verbleibenden Slots auf andere Weise, wie z. B. Durchsuchen von Sitzungen.

## <a name="role-types"></a>Arten von Rollen

Ein Rolle stellt eine Gruppe von Rollendefinitionen dar. Jede Rolle muss als Teil eines rollentyps definiert werden. Arten von Rollen werden im Dokument Multiplayer-Sitzung definiert.

Ein Member kann nur eine Rolle von einem Typ der angegebenen Rolle zugewiesen werden. Z. B. Wenn einem Rollentyp "Class" Healers Tanks und Beschädigung enthält, kann dann ein Element nur einer dieser Rollen zugewiesen werden.

Sie können mehrere Rollentypen definieren, und ein Element kann über jeden Rollentyp eine Rolle zugewiesen werden. Im vorherigen Szenario, ein Element kann die Rolle "Healer" ausgewählt haben, aber unter Umständen auch ein Team zugewiesen werden übergeordnete Rolle, wenn in einem separaten Rollentyp Squad Rolle definiert ist.

> **Wichtig:** Das Xbox Live-SDK unterstützt derzeit nur eine einzelne Rolle-Typ und einer einzelnen Rolle pro Element.

## <a name="role-type-properties"></a>Rolleneigenschaften-Typ

Wenn Sie einen Rollentyp definieren, müssen Sie die folgende Informationen für Rollentypen angeben:

* Der Name des Typs Rolle. Der Name muss Kleinbuchstaben und alphanumerische und nicht mehr als 100 Zeichen lang sein.
* Wenn die in den Rollentyp definierten Rollen Besitzer oder nicht verwaltet werden.
* Wenn die Eigenschaften der in den Rollentyp definierten Rollen während der Lebensdauer der Sitzung geändert werden können.
* Die Rollendefinitionen, die in den Rollentyp enthalten sind.

Wenn einem Rollentyp Besitzer, die verwaltet ist, bedeutet, dass, dass nur Member, die Besitzer der Sitzung sind Mitglieder Rollen dieses Typs zuweisen können. Wenn die Benutzerrolle der Typ nicht Besitzer verwaltet ist, können Mitglieder auf sich selbst Rollen zuweisen.

Sie können nur angeben, dass eine Rolle ist Besitzer verwaltet Sitzungen, die die "HasOwners"-Funktion festgelegt haben.

> Das Xbox Live-SDK unterstützt derzeit nicht Besitzer zuweisen von Rollen an andere Mitglieder.

## <a name="role-properties"></a>Eigenschaften der Benutzerrolle

Wenn Sie eine Rolle definieren, müssen Sie für jede Rolle die folgende Informationen angeben:

* Der Name der Rolle. Der Name muss Kleinbuchstaben und alphanumerische und nicht mehr als 100 Zeichen lang sein.
* Die maximale Anzahl von Elementen, die zum Erfüllen der Rolle zulässig sind. Muss größer als 0 (null) sein.
* Die vorgegebene Anzahl von Elementen, die die Rolle ausfüllen soll. Das Ziel muss größer als 0 (null) sein und kleiner oder gleich auf die maximale Anzahl von Elementen zum Erfüllen der Rolle zulässig.

Wenn eine Sitzung Element eine Rolle zugewiesen ist, wird diese Informationen in den Rollen Member im Dokument Multiplayer-Sitzung aufgezeichnet.

Der Dienst setzt die maximale Anzahl von Elementen, die kann zu einer Rolle zugewiesen werden, aber die vorgegebene Anzahl nicht erzwungen.

## <a name="create-roles"></a>Erstellen von Rollen

Rollen und Arten von Rollen werden in der Regel definiert, der [sitzungsvorlage](service-configuration/session-templates.md). Der Dienst unterstützt-Rolle und der Typ der Rollendefinition während der sitzungserstellung, aber das Xbox Live-SDK nicht der Fall ist.

### <a name="define-role-types-and-roles-in-a-session-template"></a>Definieren Sie Arten von Rollen und Rollen, in einer sitzungsvorlage

Sie können die Arten von Rollen und Rollen definieren, bei der Erstellung einer sitzungsvorlage während der Xbox Live-Konfiguration.

Die Rolleninformationen für Typ und die Rolle wird als grundlegende "RoleTypes" Element in der sitzungsvorlage im folgenden Format angegeben:

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>Abrufen von Rolleninformationen für eine Multiplayer-Sitzung

Erhalten Sie Informationen über die Rolle für Typen, Rollen und wie viele Elemente für die einzelnen Rollen zugewiesen sind, entweder eine multiplayer-Sitzung, oder aus einem Handle Multiplayer-Suche.

In den Xbox Live SDK werden Informationen zu den Arten von Rollen und Rollen innerhalb einer Struktur der Zuordnung gespeichert. Die Verwendung von C++-APIs die `unordered_map` -Klasse und die WinRT-APIs verwenden die `IMapView` Klasse.

### <a name="get-the-role-information-from-a-search-handle"></a>Informieren Sie sich die Rolle aus einem Suchhandle

In der `multiplayer_search_handle_details` Objekt zurückgegeben, aus einer suchanforderung an, Sie können die Typinformationen für die Rolle abrufen, indem Sie die Indizierung der `role_types` Zuordnung mit dem Namen, der den Rollentyp, die Sie interessiert.

Dies gibt eine `multiplayer_role_type` Objekt. Sie können die Rollen abrufen, indem Sie die Indizierung der `roles` zuordnen, gibt eine `multiplayer_role_info` Objekt.

Die `multiplayer_role_info` Objekt enthält Informationen über die Rolle, einschließlich `max_members_count`, `member_xbox_user_ids`, `members_count`, und `target_count`.

### <a name="get-the-role-information-from-a-search-handle"></a>Informieren Sie sich die Rolle aus einem Suchhandle

Der Flow zum Abrufen von Informationen aus einer Sitzung ist vergleichbar mit den Fluss zum Abrufen von Informationen aus einem Suchhandle, aber einige andere Klassen verwendet werden.

In der `multiplayer_session` Objekt ist, erhalten Sie durch Verweisen auf die Typinformationen für die Rolle der `session_role_types` -Objekt, das ist eine `multiplayer_session_role_types` Klasse. Sie können in dieses Objekt Indizieren der `role_types` Zuordnung mit dem Namen, der den Rollentyp, die Sie interessiert.

Dies gibt eine `multiplayer_role_type` Objekt. Sie können die Rollen abrufen, indem Sie die Indizierung der `roles` zuordnen, gibt eine `multiplayer_role_info` Objekt.

Die `multiplayer_role_info` Objekt enthält Informationen über die Rolle, einschließlich `max_members_count`, `member_xbox_user_ids`, `members_count`, und `target_count`.

## <a name="change-mutable-role-settings"></a>Ändern der rolleneinstellungen des änderbar

Wenn einem Rollentyp angibt, dass einige Einstellungen für die Standortsystemrolle (änderbaren) geändert werden können, können Sie die `multiplayer_role_type.set_roles()` Methode zum Ändern der Einstellungen des änderbaren. Nur Mitglieder, die als Sitzungsbesitzer gekennzeichnet sind, können Einstellungen für die Standortsystemrolle ändern.

## <a name="assign-a-role-to-a-member"></a>Ein Mitglied eine Rolle zuweisen

Derzeit kann nur ein Element eigene Rolle im Xbox Live SDK zuweisen. In der `multiplayer_session` Objekt, rufen Sie die `set_current_user_role_info(role_type, role_name)` Methode, um den Rollentyp und die Rolle für das aktuelle Element anzugeben.

Wenn die Rolle bereits voll ist, wenn Sie versuchen, die Sitzung in den Dienst zu schreiben, lehnt MPSD den Schreibvorgang.
