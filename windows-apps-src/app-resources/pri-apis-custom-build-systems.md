---
Description: Mit den APIs für die Paketressourcenindizierung (PRI) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann PRI-Dateien erstellen und mit einer Versionsangabe versehen sowie Sicherungskopien generieren, und zwar für jeden Grad an Komplexität, den Ihre UWP-App benötigt.
title: Paket Ressourcen Indizierung und benutzerdefinierte Buildsysteme
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 43162f13afd9a658c58d47d83ab71f49bdcfb70e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174074"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>APIs für die Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
Mit den [APIs für die Paketressourcenindizierung (PRI)](/windows/desktop/menurc/pri-indexing-reference) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann Indexdateien der Paketressource (PRI) erstellen, versionieren und per Dump sichern (als XML), und zwar für jedes Maß an Komplexität, das Ihre UWP-App benötigt. Wenn Sie über ein benutzerdefiniertes Buildsystem verfügen, das zurzeit das MakePri.exe Befehlszeilen Tool verwendet (siehe [Kompilieren von Ressourcen manuell mit MakePri.exe](makepri-exe-command-options.md)), sollten Sie für eine bessere Leistung und Kontrolle zum Aufrufen der PRI-APIs wechseln, anstatt MakePri.exe aufzurufenden.

Die PRI-APIs wurden in den Windows SDK für Windows 10, Version 1803, eingeführt. Die APIs haben die Form von Win32-Windows-APIs, was bedeutet, dass Sie über einige Optionen zum Aufrufen verfügen. Sie können Sie direkt über eine Win32-App aufrufen, oder Sie können Sie über einen [Platt Form](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) Aufruf über eine .net-APP oder sogar über eine UWP-App aufrufen.

In den Szenarien in diesem Thema wird das Aufrufen von PRI-APIs aus einem Win32-Visual C++ Windows-Konsolen Anwendungsprojekt veranschaulicht. Hintergrundinformationen finden Sie unter [Resource Management System](resource-management-system.md).

> [!NOTE]
> Dieser Nachteil ist wahrscheinlich kein Problem, da Sie Ihre benutzerdefinierte Buildsystem-App wahrscheinlich nicht an die Microsoft Store übermitteln möchten. Wenn Sie jedoch die Option zum Entwickeln des benutzerdefinierten Buildsystems in Form einer UWP-App auswählen, ist dies eine ungewöhnliche UWP-APP, die Sie nicht an die Microsoft Store übermitteln können. Dies liegt daran, dass eine UWP-APP, die Platt Form Aufrufe verwendet, Microsoft Store Zertifizierung fehlschlägt. Beachten Sie, dass in diesem Fall Platt Form Aufrufe *nur im benutzerdefinierten Buildsystem vorhanden*sind. *nicht* in ihrer Liefer-UWP-app (der, für den Sie pri-Dateien aufbauen).

## <a name="scenario-walkthroughs"></a>Szenarioanleitungen
|Thema|BESCHREIBUNG|
|-|-|
|[Szenario 1: Generieren einer PRI-Datei aus Zeichen folgen Ressourcen und assetdateien](pri-apis-scenario-1.md)|In diesem Szenario erstellen wir eine neue APP, die das benutzerdefinierte Buildsystem repräsentiert. Wir erstellen einen ressourcenindexer und fügen ihm Zeichen folgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.|

## <a name="important-apis"></a>Wichtige APIs
* [Referenz zur Paket Ressourcen Indizierung (PRI)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Zugehörige Themen
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](makepri-exe-command-options.md)
* [Verwenden nicht verwalteter DLL-Funktionen](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Ressourcenverwaltungssystem](resource-management-system.md)