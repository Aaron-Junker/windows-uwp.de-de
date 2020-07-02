---
title: winget-Befehl „validate“
description: Überprüft eine Manifestdatei zur Übermittlung von Software an das Paketmanifest-Repository der Microsoft Community auf GitHub.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334465"
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
