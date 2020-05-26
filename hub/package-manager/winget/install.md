---
title: Befehl „install“
description: Installiert die angegebene Anwendung.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: c903c923a82edc03ffdce9c5790060cb65232cf8
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824991"
---
# <a name="install-command-winget"></a>Befehl „install“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **install** des Tools [winget](index.md) wird die angegebene Anwendung installiert. Geben Sie mit dem Befehl [**search**](search.md) die zu installierende Anwendung an.  

Der Befehl **install** erfordert, dass Sie die genaue Installationszeichenfolge angeben. Wenn Mehrdeutigkeiten vorliegen, werden Sie aufgefordert, den Befehl **install** auf eine exakte Anwendung zu filtern.

## <a name="usage"></a>Usage

`winget install [[-q] \<query>] [\<options>]`

![Befehl „search“](images\install.png)

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar.

| Argument      | Beschreibung |
|-------------|-------------|  
| **-q,--query**  |  Die Abfrage, die für die Suche nach einer App verwendet wird. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="options"></a>Optionen

Mit den Optionen können Sie die Installation an Ihre jeweiligen Anforderungen anpassen.

| Option      | Beschreibung |
|-------------|-------------|  
| **-m, --manifest** |   Hierauf muss der Pfad zur Manifestdatei (YAML) folgen. Sie können mit dem Manifest die Installationsumgebung aus einer [lokalen YAML-Datei](#local-install) ausführen. |
| **--id**    |  Schränkt die Installation auf die ID der Anwendung ein.   |  
| **--name**   |  Schränkt die Suche auf den Namen der Anwendung ein. |  
| **--moniker**   | Schränkt die Suche auf die für die Anwendung aufgelisteten Moniker ein. |  
| **-v, --version**  |  Hiermit können Sie die zu installierende Version genau angeben. Wenn nicht angegeben, wird mit „latest“ die Anwendung mit der höchsten Version installiert. |  
| **--tag**   |   Schränkt die Suche auf die für die Anwendung aufgelisteten Tags ein. |  
| **-s, -source**   |  Schränkt die Suche auf die Quelle mit dem angegebenen Namen ein. Hierauf muss der Name der Quelle folgen. |  
| **-e, -exact**   |   Verwendet die exakte Zeichenfolge in der Abfrage und berücksichtigt die Groß-/Kleinschreibung. Das Standardverhalten einer Teilzeichenfolge wird nicht verwendet. |  
| **-i, -interactive** |  Führt den Installer im interaktiven Modus aus. In der Standardbenutzeroberfläche wird der Installationsfortschritt angezeigt. |  
| **-h, -silent** |  Führt den Installer im unbeaufsichtigten Modus aus. Unterdrückt die gesamte Benutzeroberfläche. In der Standardbenutzeroberfläche wird der Installationsfortschritt angezeigt. |  
| **-o, --log**  |  Die Protokollierung wird an eine Protokolldatei weitergeleitet. Sie müssen einen Pfad zu einer Datei angeben, für die Sie über die Schreibberechtigungen verfügen. |
| **-override** | Eine Zeichenfolge, die direkt an den Installer übergeben wird.    |
| **-l,--location** |    Gewünschter Installationsort (sofern unterstützt). |

## <a name="multiple-selections"></a>Mehrfachauswahl

Wenn die für **winget** bereitgestellte Abfrage nicht nur eine Anwendung zurückgibt, zeigt **winget** die Ergebnisse der Suche an. Dadurch erhalten Sie zusätzlichen Daten, damit Sie die Suche für einen ordnungsgemäßen Installationsvorgang eingrenzen können.

## <a name="local-install"></a>Lokale Installation

Mit der Option **manifest** können Sie eine Anwendung installieren, indem Sie eine YAML-Datei direkt an den Client übergeben. Die Option **Manifest** wird wie folgt verwendet.

Syntax: `winget install --manifest \<file>`

| Option  | Beschreibung |
|-------------|-------------|  
|  **-m,--manifest** | Der Pfad zum Manifest der zu installierenden Anwendung. |

### <a name="log-files"></a>Protokolldateien

Die Protokolldateien für winget (sofern Sie nicht umgeleitet werden) befinden sich im folgenden Ordner: ** \%temp%\\AICLI\\ *.log**

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
