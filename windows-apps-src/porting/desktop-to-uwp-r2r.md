---
Description: Dieses Handbuch wird erläutert, wie Visual Studio-Projektmappe zum Optimieren der Binärdateien der Anwendung mit nativen Images zu konfigurieren.
Search.Product: eADQiWindows 10XVcnh
title: Optimieren Sie Ihre .NET Desktop-apps mit systemeigener images
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, systemeigene Images Compiler
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642875"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimieren Sie Ihre .NET Desktop-apps mit systemeigener images

> [!NOTE]
> Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.

Sie können die Startzeit von .NET Framework-Anwendung verbessern, indem Sie die Binärdateien wird vorkompiliert. Sie können diese Technologie für große Anwendungen verwenden, die Sie packen und verteilen, die über den Microsoft Store. In einigen Fällen haben wir eine leistungsverbesserung von 20 % beobachtet. Weitere Informationen finden Sie Informationen zu dieser Technologie in die [– technische Übersicht](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Wir haben eine Preview-Version des Compilers systemeigene Images als veröffentlicht ein [NuGet-Paket](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Sie können dieses Paket anwenden, an eine beliebige .NET Framework-Anwendung, das die .NET Framework Version 4.6.2 oder höher. Dieses Paket Fügt einen Post Buildschritt hinzufügen, der für alle Binärdateien, die von der Anwendung verwendet eine native Nutzlast enthält. Diese optimierte Nutzlast wird geladen, wenn die Anwendung in .NET 4.7.2 und höher ausgeführt, während frühere Versionen immer noch den MSIL-Code geladen werden.

Die [.NET Framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) befindet sich auf die [Windows 10 April 2018 aktualisieren](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Sie können diese Version von .NET Framework auch auf PCs installieren, auf denen Windows 7 und höher und Windows Server 2008 R2 oder höher ausgeführt.

> [!IMPORTANT]
> Wenn Sie systemeigene Images für Ihre Anwendung gepackt werden, indem Sie das Projekt zum Verpacken der Windows-Anwendung erstellen möchten, stellen Sie sicher, dass die Ziel-Plattform mindestens Version des Projekts auf dem Windows Anniversary-Update.

## <a name="how-to-produce-native-images"></a>Gewusst wie: Erstellen von systemeigenen images

Um Ihre Projekte zu konfigurieren, gehen Sie wie folgt vor.

1. Konfigurieren Sie das Zielframework aus, als 4.6.2 oder höher

2. Konfigurieren Sie die Zielplattform wird als X86 oder X64. 

3. Fügen Sie die NuGet-Pakete hinzu.

4. Erstellen eines Releasebuilds.

## <a name="configure-the-target-framework-as-462-or-above"></a>Konfigurieren Sie das Zielframework aus, als 4.6.2 oder höher

So konfigurieren Sie das Projekt .NET Framework 4.6.2 als Ziel benötigen Sie die Entwicklungstools von .NET Framework 4.6.2 oder höher. Diese Tools sind als optionale Komponenten unter der .NET Desktopentwicklung arbeitsauslastung über Visual Studio-Installer zur Verfügung:

![Installieren Sie .NET 4.6.2-Entwicklungstools](images/desktop-to-uwp/install-4.6.2-devpack.png)

Alternativ können Sie die .NET Developer Packs aus abrufen: [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Konfigurieren Sie die Zielplattform wird als X86 oder X64.

Der Compiler für systemeigene Images wird den Code für eine bestimmte Plattform optimiert. Um es zu verwenden, müssen Sie Ihre Anwendung für eine bestimmte Plattform wie z. B. X86 oder X64 konfigurieren.

Wenn Sie in der Projektmappe mehrere Projekte verfügen, muss nur der Eintrag Punkt Projekt (in den meisten Fällen auf das Projekt, die eine ausführbare Datei erzeugt) als X86 oder X64 kompiliert werden. Zusätzliche Binärdateien aus dem Hauptprojekt verwiesen werden mit der Architektur, in dem Hauptprojekt, angegeben verarbeitet werden, auch wenn sie als AnyCPU kompiliert werden.

Zum Konfigurieren des Projekts:

1. Mit der rechten Maustaste in der Projektmappe, und wählen Sie dann **Configuration Manager**.

2. Wählen Sie **< neu... >** in die **Plattform** Dropdownmenü neben dem Namen des Projekts, das Ihre ausführbare Datei erstellt.

3. In der **neue Projektplattform** Dialogfeld Feld, stellen Sie sicher, dass die **Kopiereinstellungen von** Dropdown-Liste nastaven NA hodnotu **Any CPU**.

![Konfigurieren von x86](images/desktop-to-uwp/configure-x86.png)

Wiederholen Sie diesen Schritt für `Release/x64` Wunsch X64 erzeugen Binärdateien.

>[!IMPORTANT]
> AnyCPU-Konfiguration wird vom Compiler systemeigene Images nicht unterstützt.

## <a name="add-the-nuget-packages"></a>Fügen Sie die NuGet-Pakete

Der Compiler für systemeigene Abbilder wird als NuGet-Paket bereitgestellt, die Sie benötigen Visual Studio-Projekt hinzufügen, die die ausführbare Datei erzeugt. Dies ist normalerweise Ihr Windows Forms oder WPF-Projekt. Verwenden dieser PowerShell-Befehl.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Die vorschaupakete werden in "NuGet.org" als nicht aufgelistet veröffentlicht. Diese finden nicht Durchsuchen von NuGet.org oder mithilfe der Paket-Manager-UI in Visual Studio. Sie können jedoch installieren sie über die Paket-Manager-Konsole, und wann Sie von einem anderen Computer wiederherstellen. Wir stellen die Pakete vollständig zugegriffen werden kann, wenn wir die erste nicht-Preview-Version veröffentlichen.

## <a name="create-a-release-build"></a>Erstellen eines Releasebuilds

Das NuGet-Paket konfiguriert das Projekt, um ein zusätzliches Tool zur Releasebuilds ausführen. Mit diesem Tool werden die gleichen Binärdateien den systemeigenen Code hinzugefügt.
Um sicherzustellen, dass das Tool die Binärdateien verarbeitet hat können Sie überprüfen die Buildausgabe, um sicherzustellen, dass sie eine Meldung wie diese enthält:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Häufig gestellte Fragen

**Q. Führen Sie die neuen Binärdateien auf Computern ohne .NET Framework 4.7.2 funktionieren?**

A. Optimierte Binärdateien profitieren von den Verbesserungen bei der Ausführung mit .NET Framework 4.7.2. Clients, auf denen frühere .NET Framework-Versionen werden den nicht optimierten MSIL-Code aus der Binärdatei geladen.

**Q. Wie kann ich möchten Feedback abgeben oder melden Probleme?**

A. Melden eines Problems mithilfe des feedbacktools in Visual Studio 2017. [Weitere Informationen](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Was ist die Auswirkung der vorhandene Binärdateien das systemeigene Abbild hinzugefügt?**

A. Die optimierte Binärdateien enthalten die verwalteten und systemeigenen Code, sodass die endgültigen Dateien größer werden.

**Q. Kann ich mit dieser Technologie Binärdateien freigeben?**

A. Diese Version enthält eine Go Live-Lizenz, die Sie zurzeit verwenden können.
