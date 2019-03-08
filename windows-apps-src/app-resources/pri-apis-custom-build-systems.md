---
Description: Mit der Paketressource (PRI) APIs Indizierung können Sie ein benutzerdefiniertes Buildsystem für Ihre UWP-app-Ressourcen erstellen. Das Buildsystem kann PRI-Dateien erstellen und versionieren sowie Sicherungskopien erzeugen, und zwar für jeden Grad an Komplexität, die Ihre UWP-App benötigt.
title: APIs zur Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 617812415d3dcd00ec24d5f55971ae311265b61d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598515"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>APIs zur Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
Mit den [APIs zur Paketressourcenindizierung (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann Paketressourcenindexdateien (PRI) erstellen, versionieren und per Dump sichern (als XML), und zwar für jedes Maß an Komplexität, das Ihre UWP-App benötigt. Wenn Sie ein benutzerdefiniertes Buildsystem besitzen, das derzeit das Befehlszeilentool MakePri.exe verwendet (siehe [Manuelles Kompilieren von Ressourcen mit MakePri.exe](makepri-exe-command-options.md)), empfiehlt es sich zur Erzielung einer höheren Leistung und für mehr Steuerungsmöglichkeiten, die PRI-APIs anstelle von MakePri.exe aufzurufen.

Die PRI-APIs wurden mit dem Windows SDK für Windows 10, Version 1803, eingeführt. Die APIs weisen die Form von Win32-Windows-APIs auf, was bedeutet, dass Ihnen mehrere Optionen für deren Aufruf zur Verfügung stehen. Sie können sie direkt von einer Win32-App oder über [Plattformaufrufe](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) von einer .NET-App oder auch von einer UWP-App aus aufrufen.

Die Szenarien in diesem Thema zeigen den Aufruf von PRI-APIs von einem Anwendungsprojekt der Win32-Visual C++-Windows-Konsole. Hintergrundinformationen finden Sie unter [Ressourcenverwaltungssystem](resource-management-system.md).

Die maximale Größe für eine PRI-Datei beträgt 64 KB.

> [!NOTE]
> Diese Einschränkung ist wahrscheinlich kein Problem, da Sie Ihre benutzerdefinierte Buildsystem-App vermutlich nicht an den Microsoft Store übermitteln möchten. Wenn Sie aber die Option zum Entwickeln des benutzerdefinierten Buildsystems in Form einer UWP-App wählen, könnten Sie diese UWP-App nicht an den Microsoft Store übermitteln. Dies liegt daran, dass für eine UWP-App, die Plattformaufrufe verwendet, die Microsoft Store-Zertifizierung fehlschlägt. Beachten Sie, dass in diesem Fall Plattformaufrufe *nur in Ihrem benutzerdefinierten Buildsystem vorhanden sind*, *nicht* aber in Ihrer versandten UWP-App (diejenige, für die Sie PRI-Dateien erstellen).

## <a name="scenario-walkthroughs"></a>Exemplarische Vorgehensweisen für die Szenarien
|Thema|Beschreibung|
|-|-|
|[Szenario 1: Generieren einer PRI-Datei von Zeichenfolgenressourcen und Ressourcendateien](pri-apis-scenario-1.md)|In diesem Szenario erstellen wir eine neue App zur Darstellung unseres benutzerdefinierten Buildsystems. Wir erstellen einen Ressourceindexer und fügen diesem Zeichenfolgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.|

## <a name="important-apis"></a>Wichtige APIs
* [Ressource "Package" Indizierung (PRI)-Referenz](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Verwandte Themen
* [Kompilieren von Ressourcen mit MakePri.exe manuell](makepri-exe-command-options.md)
* [Verwenden nicht verwalteter DLL-Funktionen](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Ressourcenverwaltungssystem](resource-management-system.md)
