---
title: Windows Paket-Manager
description: Der Windows-Paket-Manager ist eine umfassende Paket-Manager-Lösung, die aus einem Befehlszeilentool und einer Reihe von Diensten für die Installation von Anwendungen unter Windows 10 besteht.
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334623"
---
# <a name="windows-package-manager-preview"></a>Windows-Paket-Manager (Vorschau)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Der Windows-Paket-Manager ist eine umfassende [Paket-Manager-Lösung](#understanding-package-managers), die aus einem Befehlszeilentool und einer Reihe von Diensten für die Installation von Anwendungen unter Windows 10 besteht.

## <a name="windows-package-manager-for-developers"></a>Windows-Paket-Manager für Entwickler

Mit dem Befehlszeilentool **winget** können Entwickler eine ausgewählte Gruppe von Anwendungen suchen, installieren, aktualisieren, entfernen und konfigurieren. Nach der Installation können sie über das Windows-Terminal, PowerShell oder die Eingabeaufforderung auf **winget** zugreifen.

Weitere Informationen finden Sie unter [Installieren und Verwalten von Anwendungen mit dem Tool „winget“](winget/index.md)

## <a name="windows-package-manager-for-isvs"></a>Windows-Paket-Manager für ISVs

Unabhängige Softwarehersteller (Independent Software Vendors, ISVs) können den Windows-Paket-Manager als Verteilungskanal für Softwarepakete verwenden, die ihre Tools und Anwendungen enthalten. Um Softwarepakete (mit MSIX-, MSI-oder EXE-Installationsprogrammen) an den Windows-Paket-Manager zu übermitteln, stellen wir das **Paketmanifest-Repository der Microsoft Community** (Open Source) auf GitHub bereit. Hier können ISVs [Paketmanifeste](package/manifest.md) hochladen, um die Aufnahme Ihrer Softwarepakete in den Windows-Paket-Manager zu beantragen. Manifeste werden automatisch überprüft und können auch manuell überprüft werden.

Weitere Informationen finden Sie unter [Übermitteln von Paketen an den Windows-Paket-Manager](package/repository.md).

## <a name="understanding-package-managers"></a>Informationen zu Paket-Managern

Ein Paket-Manager ist ein System oder eine Gruppe von Tools zum Automatisieren der Installation, Aktualisierung, Konfiguration und Verwendung von Software. Die meisten Paket-Manager dienen zum Suchen und Installieren von Entwicklertools.

Im Idealfall verwenden Entwickler einen Paket-Manager, um die Voraussetzungen für die benötigten Tools zur Entwicklung von Lösungen für ein bestimmtes Projekt anzugeben. Der Paket-Manager befolgt dann die deklarativen Anweisungen, um die Tools zu installieren und zu konfigurieren. Durch den Paket-Manager kann die Umgebung schneller vorbereitet werden, und es wird sichergestellt, dass dieselben Versionen von Paketen auf den Computern installiert werden.

Paket-Manager von Drittanbietern können das [Paketmanifest-Repository der Microsoft Community](package/repository.md) nutzen, um ihren Softwarekatalog zu erweitern.

## <a name="related-topics"></a>Zugehörige Themen

* [Installieren und Verwalten von Softwarepaketen mit dem Tool „winget“](winget/index.md)
* [Übermitteln von Paketen an den Windows-Paket-Manager](package/index.md)