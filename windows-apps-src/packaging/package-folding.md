---
title: Entwickeln mit Bestandspaketen und Paketfaltung
description: Hier erfahren Sie, wie Sie Ihre App mit Bestandspaketen und Paketfaltung effizient organisieren.
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, verpackung, paketlayout, bestandspaket
ms.localizationpriority: medium
ms.openlocfilehash: 9241ffeb6b232c5b5be3098b114f6c7bf00bcf0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659865"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>Entwickeln mit Bestandspaketen und Paketfaltung 

> [!IMPORTANT]
> Wenn Sie Ihre App an den Store übermitteln möchten, müssen Sie sich an den [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support) wenden und eine Genehmigung für die Verwendung von Bestandspaketen und Paketfaltung erhalten.

Bestandspakete können die Gesamtgröße der Verpackung sowie die Veröffentlichungszeit für Ihre Apps im Store reduzieren. Weitere Informationen zu Bestandspaketen und wie sie Ihre Entwicklungsiterationen beschleunigen können, finden Sie unter [Einführung zu Bestandspaketen](asset-packages.md).

Wenn Sie die Verwendung von Bestandspaketen für Ihre App in Erwägung ziehen oder bereits wissen, dass Sie sie verwenden möchten, fragen Sie sich wahrscheinlich, wie Bestandspakete Ihren Entwicklungsprozess verändern. Kurz gesagt, das Entwickeln von Apps bleibt für Sie dank der Paketfaltung für Bestandspakete unverändert.

## <a name="file-access-after-splitting-your-app"></a>Dateizugriff nach dem Aufteilen Ihrer App

Um zu verstehen, warum die Paketfaltung Ihren Entwicklungsprozesses nicht beeinflusst, müssen wir zunächst etwas weiter ausholen, um zu verstehen, was geschieht, wenn Sie Ihre App in mehrere Pakete (entweder mit Bestandspaketen oder Ressourcenpaketen) aufteilen. 

Wenn Sie einige Dateien Ihrer App in andere Pakete (die keine Architekturpakete sind) aufteilen, können Sie nicht direkt von dem Speicherort auf diese Dateien zugreifen, wo Ihr Code ausgeführt wird. Der Grund hierfür ist, dass diese Pakete alle in verschiedenen Verzeichnissen als das Architekturpaket installiert sind. Wenn Sie ein Spiel machen und Ihr Spiel lokalisiert ist z. B. Französisch und Deutsch und erstellt für x X86 und X64 Computer, dann müssen Sie diese app-Paket-Dateien im app-Bundle Ihres Spiels:

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

Wenn Ihr Spiel auf dem Computer des Benutzers installiert ist, wird jede app-Paketdatei seinen eigenen Ordner haben, in der **WindowsApps** Verzeichnis. Für einen französischen Benutzer mit 64-Bit-Windows sieht Ihr Spiel folgendermaßen aus:

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

Beachten Sie, dass die app-Paket, Dateien, die für den Benutzer nicht gelten nicht (die X86 und deutsche Pakete) installiert. 

Für diesen Benutzer befindet sich die ausführbare Hauptdatei Ihres Spiels innerhalb des **MyGame_1.0_x64**-Ordners und wird von dort aus ausgeführt. In der Regel hat diese Datei nur Zugriff auf die Dateien in diesem Ordner. Für den Zugriff auf die Dateien im **MyGame_1.0_language-fr**-Ordner müssen Sie entweder die MRT-APIs oder die PackageManager-APIs verwenden. Die MRT-APIs können automatisch die am besten geeignete Datei auswählen, aus den Sprachen installiert, können Sie erfahren Sie mehr über einen MRT-APIs auf [Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core). Alternativ finden Sie den Installationsort des französischen Sprachpakets mithilfe der [PackageManager-Klasse](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager). Sie sollten niemals den Installationsort der Pakete Ihrer App annehmen, da sich dies ändern und zwischen Benutzern variieren kann. 

## <a name="asset-package-folding"></a>Bestandspaketfaltung

Wie also können Sie auf die Dateien in Ihren Bestandspaketen zugreifen? Sie können weiterhin die Dateizugriff-APIs verwenden, die Sie für den Zugriff auf andere Dateien in Ihrem Architekturpaket verwenden. Der Grund hierfür ist, dass Bestandspaketdateien in Ihr Architekturpaket gefaltet werden, wenn es durch den Paketfaltungsprozess installiert wird. Da Bestandspaketdateien ursprünglich Dateien in Ihren Architekturpaketen sein sollten, bedeutet dies darüber hinaus, dass Sie die API-Nutzung nicht ändern müssten, wenn Sie in Ihrem Entwicklungsprozess von der Bereitstellung von losen Dateien zur verpackten Bereitstellung wechseln. 

Beginnen wir mit einem Beispiel, um mehr darüber zu erfahren, wie Paketfaltung funktioniert. Wenn Sie ein Spielprojekt mit der folgenden Dateistruktur haben:

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

Wenn Sie Ihr Spiel in 3 Pakete aufteilen möchten – ein x64-Architekturpaket, ein Bestandspaket für Audiodateien und ein Bestandspaket für Videos –, wird Ihr Spiel in diese Pakete unterteilt:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

Wenn Sie Ihr Spiel installieren, wird das x64-Paket als Erstes bereitgestellt. Danach werden die beiden Bestandspakete weiterhin in ihren eigenen Ordnern bereitgestellt, genau wie **MyGame_1.0_language-fr** aus unserem vorherigen Beispiel. Allerdings werden die Dateien des Bestandspakets aufgrund der Paketfaltung auch in den **MyGame_1.0_x64**-Ordner eingebunden (also selbst wenn die Dateien an zwei Speicherorten angezeigt werden, belegen sie nicht doppelt so viel Speicherplatz). Der Speicherort, an dem die Bestandspaketdateien angezeigt werden, ist genau jener Speicherort, an dem sie sich relativ zum Stammordner des Pakets befinden. So sieht nun das fertige Layout des bereitgestellten Spiels aus:

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

Wenn Sie die Paketfaltung für Bestandspakete verwenden, können Sie weiterhin auf die Dateien zugreifen, die Sie in Bestandspakete aufgeteilt haben (beachten Sie, dass der Architekturordner genau die gleiche Struktur wie der ursprüngliche Projektordner hat). Zudem können Sie Bestandspakete hinzufügen oder Dateien zwischen Bestandspaketen verschieben, ohne den Code zu beeinträchtigen. 

Sehen wir uns nun ein Beispiel einer komplizierteren Paketfaltung an. Angenommen, Sie möchten Ihre Dateien stattdessen stufenbasiert aufteilen. Wenn Sie dieselbe Struktur wie die des ursprünglichen Projektordners behalten möchten, sollten Ihre Pakete wie folgt aussehen:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
Auf diese Weise können die **Level1**-Ordner und -Dateien im **MyGame_Level1**-Paket, und **Level2**-Ordner und -Dateien im **MyGame_Level2**-Paket während der Paketfaltung in den **Audiodateien**- und **Videos**-Ordner zusammengeführt werden. Es gilt also die allgemeine Regel, dass der für verpackte Dateien in der Zuordnungsdatei bestimmte relative Pfad bzw. das [Verpackungslayout](packaging-layout.md) für MakeAppx.exe der Pfad ist, den Sie für den Zugriff auf diese Dateien nach der Paketfaltung verwenden sollten. 

Wenn sich zwei Dateien in verschiedenen Bestandspaketen befinden, die die gleichen relativen Pfade aufweisen, führt dies schließlich zu einer Kollision während der Paketfaltung. Wenn eine Kollision auftritt, führt die Bereitstellung Ihrer App zu einem Fehler und schlägt fehl. Zudem können Sie Ihre App bei Verwendung von Bestandspaketen nicht auf Nicht-NTFS-Laufwerken bereitstellen, da die Paketfaltung die Vorteile von festen Links nutzt. Wenn Sie wissen, dass Ihre App von Ihren Benutzern wahrscheinlich auf Wechseldatenträger verschoben wird, sollten Sie keine Bestandspakete verwenden. 


