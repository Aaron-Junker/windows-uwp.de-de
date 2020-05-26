---
title: Befehl „source“
description: Verwaltet die von Windows-Paket-Manager verwendeten Repositorys.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: b44f20021a0fa33da862e2361be60b730b041b49
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824971"
---
# <a name="source-command-winget"></a>Befehl „source“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **source** des Tools [winget](index.md) verwalten Sie die Repositorys, auf die Windows-Paket-Manager zugreift. Der Befehl **source** erlaubt es, die Repositorys **hinzuzufügen**, **zu entfernen**, **aufzulisten** und **zu aktualisieren**.

Eine Quelle stellt die Daten bereit, mit denen Sie Anwendungen ermitteln und installieren können. Fügen Sie nur dann eine neue Quelle hinzu, wenn Sie sie als sicheren Speicherort als vertrauenswürdig einstufen.

## <a name="usage"></a>Usage

`winget source \<sub command> \<options>`

![Quellbild](images\source.png)

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar.

| Argument  | Beschreibung |
|--------------|-------------|
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="sub-commands"></a>Unterbefehle

„source“ unterstützt die folgenden Unterbefehle zum Bearbeiten der Quellen.

| Unterbefehl  | Beschreibung |
|--------------|-------------|
|  **add** |  Fügt eine neue Quelle hinzu. |
|  **list** | Listet die aktivierten Quellen auf. |
|  **update** | Aktualisiert eine Quelle. |
|  **remove** | Entfernt eine Quelle. |
|  **reset** | Setzt **winget** auf die Ausgangskonfiguration zurück.  |

## <a name="options"></a>Optionen

Der Befehl **source** unterstützt die folgenden Optionen:

| Option  | Beschreibung |
|--------------|-------------|
|  **-n,--name** | Der Name zum Identifizieren der Quelle. |
|  **-a,--arg** | Die URL oder der UNC der Quelle. |
|  **-t,--type** | Der Quellentyp. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="add"></a>hinzufügen

Mit dem Unterbefehl **add** wird eine neue Quelle hinzugefügt. Bei diesem Unterbefehl sind die Option **--name** und das Argument **name** erforderlich.

Syntax: `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

Beispiel: `winget source add --name Contoso  https://www.contoso.com/cache`

Der Unterbefehl **add** unterstützt außerdem den optionalen **type**-Parameter. Der **Type**-Parameter teilt dem Client den Repositorytyp mit, mit dem eine Verbindung hergestellt wird. Die folgenden Typen werden unterstützt:

| Type  | Beschreibung |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | Der Typ der Quelle \<Standard>. |

## <a name="list"></a>list

Mit dem Unterbefehl **list** werden die derzeit aktivierten Quellen aufgezählt. Dieser Unterbefehl gibt auch Details zu einer bestimmten Quelle zurück.

Syntax: `winget list [-n, --name] \<name>`

### <a name="list-all"></a>list all

Der Unterbefehl **list** ohne weitere Argumente gibt die vollständige Liste der unterstützten Quellen zurück. Beispiel:

```CMD
> C:\winget list
> Current sources:
>     Contoso ->  https://www.contoso.com/cache
```

### <a name="list-source-details"></a>list source details

Um ausführliche Informationen zur Quelle abzurufen, übergeben Sie den Namen, der zum Identifizieren der Quelle verwendet wird. Beispiel:

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** gibt den Namen zum Identifizieren der Quelle an.
**Type** gibt den Repositorytyp an.
**Arg** gibt die URL oder den Pfad, die bzw. der von der Quelle verwendet wird.
**Data** gibt den ggf. den optional verwendeten Paketnamen an.
**Updated** gibt Datum und Uhrzeit der letzten Aktualisierung der Quelle an.

## <a name="update"></a>Update

Mit dem Unterbefehl **update** wird eine Aktualisierung einer einzelnen Quelle oder aller Quellen erzwungen.

Syntax: `winget update [-n, --name] \<name>`

### <a name="update-all"></a>update all

Der Unterbefehl **update** ohne weitere Argumente fordert jedes einzelne Repository an und aktualisiert dieses. Beispiel: `C:\winget update`

### <a name="update-source"></a>Updatequelle

Mit dem Unterbefehl **update** in Kombination mit der Option **--name** kann eine einzelne Quelle angesprochen und aktualisiert werden. Beispiel: `C:\winget update --name contoso`

## <a name="remove"></a>Entfernen

Mit dem Unterbefehl **remove** wird eine Quelle entfernt. Für diesen Unterbefehl ist die Option **--name** und das Argument **name** erforderlich, um die Quelle anzugeben.

Syntax: `winget source add [-n, --name] \<name>`

Beispiel: `winget source remove --name Contoso`

## <a name="reset"></a>reset

Mit dem Unterbefehl **reset** wird der Client auf die ursprüngliche Konfiguration zurückgesetzt. Mit dem Unterbefehl **reset** werden alle Quellen entfernt und die Quelle wird auf den Standardwert zurückgesetzt. Dieser Unterbefehl sollte nur in seltenen Fällen verwendet werden.

Syntax: `winget source reset`

Beispiel: `winget source reset`

## <a name="default-repository"></a>Standardrepository

Windows-Paket-Manager gibt ein Standardrepository an. Sie können das Repository mithilfe des Befehls **list** identifizieren. Beispiel: `winget source list`

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
