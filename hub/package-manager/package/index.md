---
title: Übermitteln von Paketen an den Windows-Paket-Manager
description: Sie können den Windows-Paket-Manager als Verteilungskanal Softwarepakete verwenden, die ihre Tools und Anwendungen enthalten.
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: ae9c9039154e2a576a691a01d64abcf8c9029c1c
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334608"
---
# <a name="submit-packages-to-windows-package-manager"></a>Übermitteln von Paketen an den Windows-Paket-Manager

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Als unabhängiger Softwarehersteller (Independent Software Vendor, ISV) können Sie den Windows-Paket-Manager als Verteilungskanal Softwarepakete verwenden, die ihre Tools und Anwendungen enthalten. Windows Package Manager unterstützt derzeit Installationsprogramme in den folgenden Formaten: MSIX, MSI und EXE.

Gehen Sie folgendermaßen vor, um Softwarepakete an den Windows-Paket-Manager zu übermitteln:

1. [Erstellen Sie ein Paketmanifest, das Informationen über die Anwendung bereitstellt](manifest.md). Manifeste sind YAML-Dateien, die dem Windows-Paket-Manager-Schema folgen.
2. [Übermitteln Sie Ihr Manifest an das Windows-Paket-Manager-Repository](repository.md). Dies ist ein Open-Source-Repository auf GitHub, das eine Sammlung von Manifesten enthält, auf die das Tool **winget** zugreifen kann.

## <a name="related-topics"></a>Zugehörige Themen

* [Verwenden des Tools „winget“](../winget/index.md)
* [Erstellen des Paketmanifests](manifest.md)
* [Übermitteln des Manifests an das Repository](repository.md)