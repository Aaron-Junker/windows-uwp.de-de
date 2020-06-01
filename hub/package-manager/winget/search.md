---
title: Befehl „search“
description: Hiermit werden die Quellen nach verfügbaren Anwendungen abgefragt, die installiert werden können
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5a176c1138ebfe3f3a9eb2cbef02dad745cfe170
ms.sourcegitcommit: 8193aef04deb3514eb2d34bfe5cb9424ba12cd76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83865017"
---
# <a name="search-command-winget"></a>Befehl „search“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **search** des Tools [winget](index.md) werden die Quellen nach verfügbaren Anwendungen abgefragt, die installiert werden können.  

Mit dem Befehl **search** können alle verfügbaren Anwendungen angezeigt werden, oder Sie können nach einer bestimmten Anwendung filtern. Der Befehl **search** wird normalerweise verwendet, um die Zeichenfolge zu identifizieren, die für die Installation einer bestimmten Anwendung verwendet wird.

## <a name="usage"></a>Usage

`winget search [[-q] \<query>] [\<options>]`

![Suchen](images\search.png)

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar.

| Argument  | Beschreibung |
 --------------|-------------|
| **-q,--query** |  Die Abfrage, die für die Suche nach einer App verwendet wird. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="show-all"></a>Alle anzeigen

Wenn der Befehl „search“ keine Filter oder Optionen enthält, werden alle verfügbaren Anwendungen in der Standardquelle angezeigt. Sie können auch nach allen Anwendungen in einer anderen Quelle suchen, wenn Sie nur die Option **source** übergeben.

## <a name="search-strings"></a>Suchzeichenfolgen

Suchzeichenfolgen können mit den folgenden Optionen gefiltert werden.

| Option  | Beschreibung |
 --------------|-------------|
| **--id**        |   Schränkt die Suche auf die ID der Anwendung ein. Die ID enthält den Herausgeber und den Anwendungsnamen. |
| **--name**      |  Schränkt die Suche auf den Namen der Anwendung ein. |
| **--moniker**  |    Schränkt die Suche auf den angegebenen Moniker ein. |
| **--tag**    |  Schränkt die Suche auf die für die Anwendung aufgelisteten Tags ein. |
| **--command**   |   Schränkt die Suche auf den Namen der Anwendung ein. |

Die Zeichenfolge wird als Teilzeichenfolge behandelt. Bei der Suche wird standardmäßig auch die Groß-/Kleinschreibung beachtet. `winget search micro` könnte z. B. Folgendes zurückgeben:

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>Suchoptionen

Der Befehl „search“ unterstützt eine Reihe von Optionen oder Filtern, mit denen die Ergebnisse eingeschränkt werden können.

| Option  | Beschreibung |
 --------------|-------------|
| **-e, --exact**  |     Verwendet die exakte Zeichenfolge in der Abfrage und berücksichtigt die Groß-/Kleinschreibung. Das Standardverhalten einer Teilzeichenfolge wird nicht verwendet.  |  
| **-n, --count**      |  Schränkt die Ausgabe der Anzeige auf die angegebene Anzahl ein. |
| **-s, --source**     |  Schränkt die Suche auf die mit [source](source.md) angegebene Quelle ein.  |

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
