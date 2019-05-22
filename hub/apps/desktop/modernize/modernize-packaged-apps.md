---
Description: Erfahren Sie, wie Sie moderne Funktionen für Windows 10-Benutzern in einer Desktopanwendung hinzufügen, die Sie in einem Windows-app-Paket verpackt haben.
title: Modernisieren von desktop-apps App-Pakete
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6d71233bc7b96af9d9b261406d6b149f36f65f29
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985290"
---
# <a name="features-that-require-package-identity"></a>Funktionen, die Paketidentität erfordern

Wenn Sie Ihre desktop-app mit aktualisieren möchten [modernen Windows 10-Funktionen](index.md), viele Features sind nur in desktop-apps, die in einem MSIX-Paket verpackt sind verfügbar.

MSIX ist eine moderne Windows-app-Paket-Format, das eine universelle Packaging-Ergebnis für alle Windows-apps, WPF, Windows Forms und Win32-apps bietet. Packen Ihre Windows-desktop-apps können Sie moderne Windows 10-Funktionen wie live-Kacheln und Benachrichtigungen in Ihre apps integrieren. Er ruft auch Zugriff auf eine stabile Installation und Update-Oberfläche, ein verwalteter Sicherheitsmodell, das mit einem System flexible Möglichkeit, Unterstützung für den Microsoft Store, unternehmensverwaltung und viele benutzerdefinierte Verteilung Modelle ab. Weitere Informationen finden Sie unter [Paket desktopanwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) in der Dokumentation zu MSIX.

Wenn Sie Ihre desktop-app packen, können Sie dann UWP-APIs, die Paketidentität, paketerweiterungen und UWP-Komponenten in Ihrer app-Paket benötigen. Weitere Informationen finden Sie in diesen Artikeln.

## <a name="use-uwp-apis-that-require-package-identity"></a>Verwenden von UWP-APIs, die Paketidentität erfordern

Einige UWP-APIs erfordern [Paket Identität](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) in einer desktop-app verwendet werden. Das MSIX-Paket (einschließlich des Paketmanifests) bietet diese Identität.

Weitere Informationen finden Sie unter [diese Liste der APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Paketerweiterungen integrieren

Wenn Ihre Anwendung Sie in das System integrieren muss (z. B.: Einrichten der Firewall-Regeln) Dinge im Paketmanifest der Anwendung und die Beschreibung des Systems erledigt den Rest. Für die meisten dieser Aufgaben müssen Sie gar keinen Code schreiben. Mit ein wenig XML in das Manifest können Sie Aktionen wie das Starten eines Prozesses, wenn der Benutzer anmeldet, integrieren Ihre Anwendung in Datei-Explorer und fügen Ihrer Anwendung eine Liste der print-Ziele, die in anderen apps angezeigt werden.

Weitere Informationen finden Sie unter [integrieren Sie Ihre desktop-app in paketerweiterungen](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Erweitern mit UWP-Komponente

Einige Windows 10-Umgebungen (z. B. eine touch-fähige UI-Seite) müssen sich in einem Modern-App-Container befinden. Im Allgemeinen zuerst sollten Sie ermitteln, ob Sie Ihre Umgebung durch hinzufügen können [verbessern](desktop-to-uwp-enhance.md) Ihrer vorhandenen Desktopanwendung mit UWP-APIs. Wenn Sie eine UWP-Komponente zu verwenden, um die Funktionalität erzielen, müssen, können Sie Ihrer Projektmappe ein UWP-Projekt hinzufügen und app-Dienste für die Kommunikation zwischen Ihrer desktop-Anwendung und die UWP-Komponente verwenden.

Weitere Informationen finden Sie unter [erweitern Sie Ihre desktop-app mit UWP-Komponente](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Vertreiben

Sie können Ihre Anwendung verteilen, indem Sie sie dem Microsoft Store zu veröffentlichen oder durch querladen auf anderen Systemen.

Finden Sie unter [Ihrer desktop-app-Paket verteilen](desktop-to-uwp-distribute.md).
