---
title: Verteilen einer verwalteten Komponente für Windows-Runtime
description: Sie können Ihre Komponente für Windows-Runtime durch Kopieren der Dateien verteilen.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4af684fb8352cef154f83ad30723ff3f9bd6aa42
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393698"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Verteilen einer verwalteten Komponente für Windows-Runtime

Sie können Ihre Komponente für Windows-Runtime durch Kopieren der Dateien verteilen. Wenn Ihre Komponente aus vielen Dateien besteht, kann die Installation für Ihre Benutzer aber sehr mühsam sein. Außerdem verursachen Fehler beim Platzieren der Dateien oder beim Festlegen von Verweisen möglicherweise Probleme. Sie können eine komplexe Komponente als Visual Studio-Erweiterungs-SDK packen, um die Installation und Verwendung einfacher zu gestalten. Benutzer müssen nur einen Verweis für das gesamte Paket festlegen. Benutzer können Ihre Komponente mit dem Dialogfeld **Erweiterungen und Updates** problemlos suchen und installieren, wie in der MSDN Library unter [Suchen und Verwenden von Visual Studio-Erweiterungen](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015) beschrieben.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planen einer verteilbaren Komponente für Windows-Runtime

Wählen Sie eindeutige Namen für Binärdateien, z. B. für Ihre WINMD-Dateien. Wir empfehlen das folgende Format, um die Eindeutigkeit sicherzustellen:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Ihre Binärdateien werden in App-Paketen möglicherweise mit Binärdateien von anderen Entwicklern installiert. Weitere Informationen finden Sie unter "Erweiterungs [-sdche" in Gewusst wie: Erstellen Sie ein Software Development](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)Kit in der MSDN Library.

Die Entscheidung darüber, wie Sie Ihre Komponente verteilen, hängt von Komplexität der Komponente ab. Sie sollten ein Erweiterungs-SDK oder einen ähnlichen Paket-Manager verwenden, wenn:

-   Ihre Komponente aus mehreren Dateien besteht.
-   Sie Versionen Ihrer Komponente für mehrere Plattformen (z. B. x86 und ARM) bereitstellen.
-   Sie Debug- und Releaseversionen Ihrer Komponente bereitstellen.
-   Ihre Komponente über Dateien und Assemblys verfügt, die nur zur Entwurfszeit verwendet werden.

Ein Erweiterungs-SDK ist besonders nützlich, wenn mindestens einer der oben genannten Punkte zutrifft.

> **Hinweis für komplexe**Komponenten bietet das nuget-Paketverwaltungssystem eine Open Source-Alternative zu Erweiterungs-sdkern.   Wie mit Erweiterungs-SDKs können Sie mit NuGet Pakete erstellen, die die Installation komplexer Komponenten vereinfachen. Ein Vergleich von NuGet-Paketen und Visual Studio-Erweiterungs-SDKs finden Sie unter [Hinzufügen von Referenzen mithilfe von NuGet im Vergleich zu einer SDK-Erweiterung](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015) in der MSDN Library.

## <a name="distribution-by-file-copy"></a>Verteilung durch Kopieren von Dateien

Wenn Ihre Komponente aus einer einzigen WINMD-Datei oder einer WINMD-Datei und einer Ressourcenindexdatei (PRI) besteht, können Sie einfach die WINMD-Datei so bereitstellen, dass Benutzer sie kopieren können. Benutzer können die Datei an einer beliebige Position in ein Projekt einfügen, im Dialogfeld **Vorhandenes Element hinzufügen** die WINMD-Datei dem Projekt hinzufügen und dann mit dem Dialogfeld „Verweis-Manager” einen Verweis erstellen. Wenn Sie eine PRI-Datei oder eine XML-Datei einbeziehen, sollten Sie die Benutzer anweisen, diese Dateien in dasselbe Verzeichnis wie die WINMD-Datei zu kopieren.

> **Beachten Sie,** dass Visual Studio immer eine PRI-Datei erstellt, wenn Sie die Windows-Runtime Komponente erstellen, auch wenn Ihr Projekt keine Ressourcen enthält.   Wenn Sie über eine Test-App für die Komponente verfügen, können Sie bestimmen, ob die PRI-Datei verwendet wird, indem Sie den Inhalt des App-\\Pakets\\im Ordner "bin debug AppX" untersuchen. Wenn die PRI-Datei Ihrer Komponente dort nicht angezeigt wird, müssen Sie sie nicht verteilen. Sie können auch mit dem Tool [MakePRI.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) die Ressourcendatei aus Ihrem Komponentenprojekt für Windows-Runtime ausgeben. Geben Sie z. B. im Eingabeaufforderungsfenster von Visual Studio Folgendes ein: makepri dump /if MyComponent.pri /of MyComponent.pri.xml. Weitere Informationen zu PRI-Dateien finden Sie unter [Ressourcenverwaltungssystem (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Verteilung durch Erweiterungs-SDK

Eine komplexe Komponente enthält in der Regel Windows-Ressourcen, aber lesen Sie den Hinweis zum Erkennen von leeren PRI-Dateien im vorherigen Abschnitt.

**So erstellen Sie ein Erweiterungs-SDK**

1.  Überprüfen Sie, ob das Visual Studio-SDK installiert ist. Sie können das Visual Studio-SDK von der Seite [Visual Studio-Downloads](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) herunterladen.
2.  Erstellen eines neues Projekts mit der VSIX-Projektvorlage Sie finden die Vorlage in der Kategorie „Erweiterbarkeit” unter Visual C# oder Visual Basic. Diese Vorlage wird als Teil des Visual Studio-SDK installiert. ([Exemplarische Vorgehensweise: Erstellen eines SDK mithilfe C# von oder](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) Visual Basic [oder Exemplarische Vorgehensweise: Wenn Sie ein SDK C++ ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)mithilfe von erstellen, wird die Verwendung dieser Vorlage in einem sehr einfachen Szenario veranschaulicht. )
3.  Legen Sie die Ordnerstruktur für Ihr SDK fest. Die Ordnerstruktur beginnt auf der Stammebene des VSIX-Projekts mit den Ordnern **References**, **Redist** und **DesignTime**.

    -   **References** ist der Speicherort für Binärdateien, die Ihre Benutzer für die Programmierung verwenden können. Das Erweiterungs-SDK erstellt in den Visual Studio-Projekten Ihrer Benutzer Verweise auf diese Dateien.
    -   **Redist** ist der Speicherort für andere Dateien, die mit Ihren Binärdateien in Apps, die mit Ihrer Komponente erstellt werden, verteilt werden müssen.
    -   **DesignTime** ist der Speicherort für Dateien, die nur verwendet werden, wenn Entwickler Apps erstellen, die Ihre Komponente verwenden.

    In jedem dieser Ordner können Sie Konfigurationsordner erstellen. Die zulässigen Namen sind „debug”, „retail” und „CommonConfiguration”. Der Ordner „CommonConfiguration” ist für Dateien vorgesehen, die für „retail”- und „debug”-Builds identisch sind. Wenn Sie nur „retail”-Builds Ihrer Komponente verteilen, können Sie alles in „CommonConfiguration” einfügen und die beiden anderen Ordner weglassen.

    In jedem Konfigurationsordner können Sie Architekturordner für plattformspezifische Dateien bereitstellen. Wenn Sie dieselben Dateien für alle Plattformen verwenden, können Sie einen einzelnen Ordner mit dem Namen „neutral” vorsehen. Details zur Ordnerstruktur, einschließlich weiterer Architektur Ordnernamen, finden Sie unter [Gewusst wie: Erstellen Sie ein Software Development](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)Kit in der MSDN Library. (Dieser Artikel behandelt Plattform-SDKs und Erweiterungs-SDKs. Reduzieren Sie den Abschnitt über Plattform-SDKs, um Missverständnisse zu vermeiden. )

4.  Erstellen Sie eine SDK-Manifestdatei. Das Manifest gibt den Namen und die Versionsinformationen, die von Ihrem SDK unterstützten Architekturen, .NET-Versionen und andere Informationen über die Art und Weise an, wie Visual Studio Ihr SDK verwendet. Ausführliche Informationen und ein Beispiel finden Sie unter [Gewusst wie: Erstellen Sie ein Software Development](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)Kit.
5.  Erstellen und verteilen Sie das Erweiterungs-SDK. Ausführliche Informationen einschließlich Lokalisierung und Signierung des VSIX-Pakets finden Sie unter „VSIX Deployment” in der MSDN Library.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen eines Software Development Kits](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [Nuget-Paketverwaltungssystem](https://github.com/NuGet/Home)
* [Ressourcen Verwaltungs System (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Suchen und Verwenden von Visual Studio-Erweiterungen](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Makepri. exe-Befehlsoptionen](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
