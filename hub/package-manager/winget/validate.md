---
title: winget-Befehl „validate“
description: Überprüft eine Manifestdatei zur Übermittlung von Software an das Paketmanifest-Repository der Microsoft Community auf GitHub.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824931"
---
# <a name="validate-command-winget"></a>Befehl „validate“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **validate** des Tools [winget](index.md) wird eine [Manifestdatei](../package/manifest.md) zum Übermitteln von Software an das **Paketmanifest-Repository der Microsoft Community** auf GitHub überprüft. Das Manifest muss eine YAML-Datei sein, die der [Spezifikation](https://github.com/microsoft/winget-pkgs/YamlSpec.md) entspricht.

## <a name="usage"></a>Usage

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar.

| Argument  | Beschreibung |
|--------------|-------------|
| **--manifest** |  Der Pfad zu dem Manifest, das überprüft werden soll. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
* [Übermitteln von Paketen an den Windows-Paket-Manager](../package/index.md)
