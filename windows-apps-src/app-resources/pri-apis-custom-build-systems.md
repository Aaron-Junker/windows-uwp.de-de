---
Description: Mit den PRI-APIs (Package Resource Index) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen ihrer UWP-App entwickeln. Das Buildsystem kann PRI-Dateien erstellen und mit einer Versionsangabe versehen sowie Sicherungskopien generieren, und zwar für jeden Grad an Komplexität, den Ihre UWP-App benötigt.
title: Paket Ressourcen Indizierung und benutzerdefinierte Buildsysteme
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: ebbec3a89d795dde5c47d3dce76f77be06feebe2
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734925"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>APIs zur Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
Mit den [APIs für die Paketressourcenindizierung (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann Indexdateien der Paketressource (PRI) erstellen, versionieren und per Dump sichern (als XML), und zwar für jedes Maß an Komplexität, das Ihre UWP-App benötigt. Wenn Sie ein benutzerdefiniertes Buildsystem besitzen, das derzeit das Befehlszeilentool MakePri.exe verwendet (siehe [Manuelles Kompilieren von Ressourcen mit MakePri.exe](makepri-exe-command-options.md)), empfiehlt es sich zur Erzielung einer höheren Leistung und für mehr Steuerungsmöglichkeiten, die PRI-APIs anstelle von MakePri.exe aufzurufen.

Die PRI-APIs wurden mit dem Windows SDK für Windows 10, Version 1803, eingeführt. Die APIs weisen die Form von Win32-Windows-APIs auf, was bedeutet, dass Ihnen mehrere Optionen für deren Aufruf zur Verfügung stehen. Sie können sie direkt von einer Win32-App oder über [Plattformaufrufe](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) von einer .NET-App oder auch von einer UWP-App aus aufrufen.

Die Szenarien in diesem Thema zeigen den Aufruf von PRI-APIs von einem Anwendungsprojekt der Win32-Visual C++-Windows-Konsole. Hintergrundinformationen finden Sie unter [Ressourcenverwaltungssystem](resource-management-system.md).

Die maximale Größe für eine PRI-Datei beträgt 64 KB.

> [!NOTE]
> Diese Einschränkung ist wahrscheinlich kein Problem, da Sie Ihre benutzerdefinierte Buildsystem-App vermutlich nicht an den Microsoft Store übermitteln möchten. Wenn Sie aber die Option zum Entwickeln des benutzerdefinierten Buildsystems in Form einer UWP-App wählen, könnten Sie diese UWP-App nicht an den Microsoft Store übermitteln. Dies liegt daran, dass für eine UWP-App, die Plattformaufrufe verwendet, die Microsoft Store-Zertifizierung fehlschlägt. Beachten Sie, dass in diesem Fall Plattformaufrufe *nur in Ihrem benutzerdefinierten Buildsystem vorhanden sind*, *nicht* aber in Ihrer versandten UWP-App (diejenige, für die Sie PRI-Dateien erstellen).

## <a name="scenario-walkthroughs"></a>Exemplarische Vorgehensweisen für die Szenarien
|Thema|Beschreibung|
|-|-|
|[Szenario 1: Generieren einer PRI-Datei aus Zeichen folgen Ressourcen und assetdateien](pri-apis-scenario-1.md)|In diesem Szenario erstellen wir eine neue App zur Darstellung unseres benutzerdefinierten Buildsystems. Wir erstellen einen Ressourceindexer und fügen diesem Zeichenfolgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.|

## <a name="important-apis"></a>Wichtige APIs
* [Referenz zur Paket Ressourcen Indizierung (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Verwandte Themen
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](makepri-exe-command-options.md)
* [Verwenden nicht verwalteter DLL-Funktionen](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Ressourcenverwaltungssystem](resource-management-system.md)
