---
title: Befehl „show“
description: Dieser Befehl zeigt Details zur angegebenen Anwendung an, u. a. zur Quelle der Anwendung sowie die zugehörigen Metadaten.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1df5a5287b6c7a1321025182f7b3f24ed896e76d
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824981"
---
# <a name="show-command-winget"></a>Befehl „show“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **show** des Tools [winget](index.md) werden Details zur angegebenen Anwendung gezeigt, u. a. zur Quelle der Anwendung sowie die zugehörigen Metadaten.

Mit dem Befehl **show** werden nur Metadaten angezeigt, die mit der Anwendung übermittelt wurden. Wenn in der übermittelten Anwendung einige Metadaten ausgeschlossen sind, werden die Daten nicht angezeigt.

## <a name="usage"></a>Usage

`winget show [[-q] \<query>] [\<options>]`

![Befehl „show“](images\show.png)

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar.

| Argument  | Beschreibung |
|--------------|-------------|
| **-q,--query** |  Die Abfrage, die für die Suche nach einer Anwendung verwendet wird. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="options"></a>Optionen

Die folgenden Optionen sind verfügbar.

| Option  | Beschreibung |
|--------------|-------------|
| **-m,--manifest** | Der Pfad zum Manifest der zu installierenden Anwendung. |
| **--id**         |  Filtert die Ergebnisse nach ID. |
| **--name**   |      Filtert die Ergebnisse nach Name. |
| **--moniker**   |  Filtert die Ergebnisse nach Anwendungsmoniker. |
| **-v,--version** |  Gibt an, welche Version verwendet werden soll. Der Standardwert ist die neueste Version. |
| **-s,--source** |   Sucht die Anwendung mithilfe der mit [source](source.md) angegebenen Quelle. |
| **-e,--exact**     | Sucht die Anwendung mit einer genauen Übereinstimmung. |
| **--versions**    | Zeigt die verfügbaren Versionen der Anwendung an. |

## <a name="multiple-selections"></a>Mehrfachauswahl

Wenn die für **winget** bereitgestellte Abfrage nicht nur eine Anwendung zurückgibt, zeigt **winget** die Ergebnisse der Suche an. Dadurch erhalten Sie zusätzlichen Daten, damit Sie die Suche verfeinern können.

## <a name="results-of-show"></a>Ergebnisse von „show“

Wenn eine einzelne Anwendung erkannt wird, werden folgende Daten angezeigt.

### <a name="metadata"></a>Metadaten

| Value  | Beschreibung |
|--------------|-------------|
| **Id**   | ID der Anwendung. |
| **Name**  | Der Name der App. |
| **Publisher** | Der Herausgeber der Anwendung. |
| **Version** | Die Version der Anwendung. |
| **Author**  | Der Autor der Anwendung. |
| **AppMoniker** | Der Moniker der Anwendung. |
| **Beschreibung** | Eine Beschreibung der Anwendung. |
| **Lizenz**  | Die Lizenz der Anwendung. |
| **LicenseUrl** | Die URL der Lizenzdatei für die Anwendung. |
| **Homepage**  | Die Homepage der Anwendung. |
| **Tags** | Die zur Unterstützung bei der Suche bereitgestellten Tags.  |
| **Befehl** | Die von der Anwendung unterstützten Befehle. |
| **Channel**  | Informationen dazu, ob es sich bei der Anwendung um eine Vorschau- oder Release-Version handelt.  |
| **Minimum OS Version** | Die minimale Betriebssystemversion, die von der Anwendung unterstützt wird. |

### <a name="installer-details"></a>Informationen zum Installationsprogramm

| Value  | Beschreibung |
|--------------|-------------|
| **Arch**   | Die Architektur des Installationsprogramms. |
| **Sprache**  | Die Sprache des Installationsprogramms. |
| **Installer Type**  | Der Typ des Installationsprogramms. |
| **Download Url** | Die URL des Installationsprogramms. |
| **Hash** | Der SHA-256-Hash des Installationsprogramms.  |
| **Scope** | Zeigt an, ob sich das Installationsprogramm auf Computer oder Benutzer bezieht. |

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
