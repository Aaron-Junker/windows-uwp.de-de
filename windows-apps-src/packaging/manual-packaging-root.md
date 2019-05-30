---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Manuelles Verpacken von Apps
description: Dieser Abschnitt enthält Artikel oder Links zum manuellen Verpacken von UWP (Universelle Windows-Plattform)-Apps.
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, UWP, Verpacken
ms.localizationpriority: medium
ms.openlocfilehash: d90307560c315741751a5ab58ccdf0eaf35d97fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372792"
---
# <a name="manual-app-packaging"></a>Manuelles Verpacken von Apps

Wenn Sie ein App-Paket erstellen und signieren möchten, für die Entwicklung der App aber nicht Visual Studio verwendet haben, müssen Sie die Tools für die manuelle App-Verpackung verwenden.

> [!IMPORTANT] 
> Wenn Sie Visual Studio zum Entwickeln der App verwendet haben, wird empfohlen, dass Sie den Visual Studio-Assistenten zum Erstellen und Signieren des App-Pakets verwenden. Weitere Informationen finden Sie unter [Verpacken einer UWP-App mit Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Zweck

Dieser Abschnitt enthält Artikel oder Links zum manuellen Verpacken von UWP (Universelle Windows-Plattform)-Apps.

| Thema | Beschreibung |
|-------|-------------|
| [Erstellen Sie ein app-Paket mit dem Tool MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe erstellt, verschlüsselt, entschlüsselt und extrahiert Dateien aus App-Paketen und -Bündeln. |
| [Erstellen Sie ein Zertifikat zum Signieren des Pakets](create-certificate-package-signing.md) | PowerShell-Tools helfen bei der Erstellung und beim Export eines App-Paketsignaturzertifikats. |
| [Melden Sie ein app-Paket mithilfe von SignTool](sign-app-package-using-signtool.md) | Verwenden Sie SignTool, um ein App-Paket manuell mit einem Zertifikat zu signieren. |

### <a name="advanced-topics"></a>Fortgeschrittene Themen

Dieser Abschnitt enthält fortgeschrittene Themen zum Aufschlüsseln einer großen und/oder komplexen App für eine effizientere Verpackung und Installation. 

> [!IMPORTANT]
> Wenn Sie Ihre App an den Store übermitteln möchten, müssen Sie sich an den [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support) wenden und eine Genehmigung für die Verwendung der erweiterten Funktionen in diesem Abschnitt erhalten.


| Thema | Beschreibung |
|-------|-------------|
| [Einführung in Asset-Pakete](asset-packages.md) | Asset-Pakete sind ein Pakettyp, der als zentraler Speicherort für die gemeinsamen Dateien einer Anwendung fungiert. Dadurch wird die Notwendigkeit doppelter Dateien in allen Architekturpaketen effektiv eliminiert. |
| [Entwickeln mit ressourcenpakete und das Paket zu falten](package-folding.md) | Hier erfahren Sie, wie Sie Ihre App mit Bestandspaketen und Paketfaltung effizient organisieren. |
| [Flatfile-Paket-app-Pakete](flat-bundles.md) | Beschreibt, wie ein Flatfile-Paket für die Dateien Ihrer app-Paket zu erstellen. |
| [Erstellen von Paketen mit dem Layout für die paketerstellung](packaging-layout.md) | Das Verpackungslayout ist ein Dokument, das die Verpackungsstruktur der App beschreibt. Es gibt die Bündel einer App („primär” und „optional”), die Pakete in den Bündeln sowie die Dateien in den Paketen an. |
