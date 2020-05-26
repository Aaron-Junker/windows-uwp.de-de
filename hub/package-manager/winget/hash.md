---
title: winget-Befehl „hash“
description: Generiert den SHA256-Hash für ein Installationsprogramm.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2f7b2074a9d6f2a01fe5a74f7f081ceeedcccec9
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824951"
---
# <a name="hash-command-winget"></a>Befehl „hash“ (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehl **hash** des Tools [winget](index.md) wird der SHA256-Hash für ein Installationsprogramm erstellt. Dieser Befehl wird verwendet, wenn Sie eine [Manifestdatei](../package/manifest.md) erstellen müssen, um Software an das **Paketmanifest-Repository der Microsoft Community** auf GitHub zu übermitteln. Außerdem unterstützt der Befehl **hash** auch das Erstellen eines SHA256-Zertifikatshashs für MSIX-Dateien.

## <a name="usage"></a>Usage

`winget hash [-f] \<file> [\<options>]`

Der Unterbefehl **hash** kann nur für eine lokale Datei ausgeführt werden. Um den Unterbefehl **hash** zu verwenden, laden Sie das Installationsprogramm an einen bekannten Speicherort herunter. Übergeben Sie dann den Dateipfad als Argument an den Unterbefehl **hash**.

## <a name="arguments"></a>Argumente

Folgende Argumente sind verfügbar:

| Argument  | Beschreibung |
|--------------|-------------|
| **-f,--file** |  Der Pfad zu der Datei, für die der Hashwert generiert werden soll. |
| **-m,--msix**  | Gibt an, dass der Befehl „hash“ auch die SHA256-Signatur zur Verwendung mit MSIX-Installationsprogrammen erstellt. |
| **-?, --help** |  Ruft zusätzliche Hilfe zu diesem Befehl ab. |

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](index.md)
* [Übermitteln von Paketen an den Windows-Paket-Manager](../package/index.md)
