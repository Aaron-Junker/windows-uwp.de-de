---
title: Verteilen einer verwalteten Windows-Runtime Komponente
description: Sie können Ihre Windows-Runtime Komponente nach Dateikopie verteilen.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690384"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Verteilen einer verwalteten Windows-Runtime Komponente

Sie können Ihre Windows-Runtime Komponente nach Dateikopie verteilen. Wenn die Komponente jedoch aus vielen Dateien besteht, kann die Installation für Ihre Benutzer mühsam sein. Außerdem können Fehler beim Platzieren von Dateien oder beim Festlegen von verweisen Probleme verursachen. Sie können eine komplexe Komponente als Visual Studio-Erweiterungs-SDK verpacken, um die Installation und Verwendung zu vereinfachen. Benutzer müssen nur einen Verweis für das gesamte Paket festlegen. Mithilfe des Dialog Felds **Erweiterungen und Updates** können Sie die Komponente problemlos suchen und installieren, wie untersuchen [und Verwenden von Visual Studio-Erweiterungen](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)beschrieben.

## <a name="planning-a-distributable-windows-runtime-component"></a>Planen einer verteilbaren Windows-Runtime Komponente

Wählen Sie eindeutige Namen für Binärdateien aus, z. b. winmd-Dateien. Das folgende Format wird empfohlen, um die Eindeutigkeit sicherzustellen:

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Die Binärdateien werden in App-Paketen installiert, möglicherweise mit Binärdateien von anderen Entwicklern. Weitere Informationen finden Sie unter "Erweiterungs-sdche" in Gewusst [wie: Erstellen eines Software Development Kits](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).

Um zu entscheiden, wie die Komponente verteilt werden soll, überlegen Sie, wie komplex Sie ist. Ein Erweiterungs-SDK oder ein ähnlicher Paket-Manager wird empfohlen, wenn:

-   Die Komponente besteht aus mehreren Dateien.
-   Sie stellen Versionen der Komponente für mehrere Plattformen (z. b. x86 und Arm) bereit.
-   Sie stellen sowohl Debug-als auch Releaseversionen der Komponente bereit.
-   Die Komponente verfügt über Dateien und Assemblys, die nur zur Entwurfszeit verwendet werden.

Ein Erweiterungs-SDK ist besonders nützlich, wenn mehr als einer der oben genannten "true" ist.

> **Beachten Sie**  für komplexe Komponenten bietet das nuget-Paketverwaltungssystem eine Open Source-Alternative zu Erweiterungs-sdkern. Wie Erweiterungs-sDas können Sie mit nuget Pakete erstellen, die die Installation komplexer Komponenten vereinfachen. Einen Vergleich der nuget-Pakete und Visual Studio-Erweiterungs-SDKs finden [Sie unter Hinzufügen von verweisen mithilfe von nuget im Vergleich zu einem Erweiterungs-SDK](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015).

## <a name="distribution-by-file-copy"></a>Verteilung nach Dateikopie

Wenn die Komponente aus einer einzelnen winmd-Datei oder einer winmd-Datei und einer Ressourcen Indexdatei (. PRI) besteht, können Sie einfach die winmd-Datei für die Benutzer zum Kopieren verfügbar machen. Benutzer können die Datei an einem beliebigen Ort in einem Projekt platzieren. verwenden Sie das Dialogfeld **Vorhandenes Element hinzufügen** , um dem Projekt die winmd-Datei hinzuzufügen, und erstellen Sie dann im Dialogfeld Verweis-Manager einen Verweis. Wenn Sie eine PRI-Datei oder eine XML-Datei einschließen, weisen Sie die Benutzer an, diese Dateien mit der winmd-Datei zu platzieren.

> **Beachten Sie** ,  Visual Studio immer eine PRI-Datei erstellt, wenn Sie die Windows-Runtime Komponente erstellen, auch wenn Ihr Projekt keine Ressourcen enthält. Wenn Sie über eine Test-App für die Komponente verfügen, können Sie bestimmen, ob die PRI-Datei verwendet wird, indem Sie den Inhalt des App-Pakets im Ordner "bin\\Debug\\AppX" untersuchen. Wenn die PRI-Datei der Komponente dort nicht angezeigt wird, müssen Sie Sie nicht verteilen. Alternativ können Sie das [makepri. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) -Tool verwenden, um die Ressourcen Datei aus Ihrem Windows-Runtime Komponenten Projekt zu sichern. Geben Sie beispielsweise im Visual Studio-Eingabe Aufforderungs Fenster Folgendes ein: makepri Dump/if MyComponent. pri/of MyComponent. pri. Xml. Weitere Informationen zu PRI-Dateien finden Sie im [Ressourcen Verwaltungs System (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Verteilung durch das Erweiterungs-SDK

Eine komplexe Komponente umfasst normalerweise Windows-Ressourcen, aber beachten Sie den Hinweis zum Erkennen leerer PRI-Dateien im vorherigen Abschnitt.

**So erstellen Sie ein Erweiterungs-SDK**

1.  Stellen Sie sicher, dass Sie das Visual Studio SDK installiert haben. Sie können das Visual Studio SDK von der [Visual Studio-Download](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) Seite herunterladen.
2.  Erstellen Sie ein neues Projekt mithilfe der VSIX-Projektvorlage. Sie finden die Vorlage unter Visual C# oder Visual Basic in der Kategorie Erweiterbarkeit. Diese Vorlage wird als Teil des Visual Studio SDK installiert. (Exemplarische Vorgehensweise[: Erstellen eines C# SDK mithilfe von oder Visual Basic](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) oder Exemplarische Vorgehensweise [: Erstellen eines SDK mithilfe C++ ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)von wird die Verwendung dieser Vorlage in einem sehr einfachen Szenario veranschaulicht. )
3.  Legen Sie die Ordnerstruktur für das SDK fest. Die Ordnerstruktur beginnt auf der Stamm Ebene des VSIX-Projekts mit den Ordnern **References**, **Redist**und **DesignTime** .

    -   **Verweise** sind der Speicherort für Binärdateien, für die die Benutzer programmieren können. Das Erweiterungs-SDK erstellt in den Visual Studio-Projekten der Benutzer Verweise auf diese Dateien.
    -   **Redist** ist der Speicherort für andere Dateien, die mit ihren Binärdateien verteilt werden müssen, in apps, die mithilfe Ihrer Komponente erstellt werden.
    -   " **DesignTime** " ist der Speicherort für Dateien, die nur verwendet werden, wenn Entwickler apps erstellen, die Ihre Komponente verwenden.

    In jedem dieser Ordner können Sie Konfigurations Ordner erstellen. Die zulässigen Namen sind Debug, Retail und commonconfiguration. Der Ordner commonconfiguration ist für Dateien vorgesehen, die identisch sind, unabhängig davon, ob Sie von Einzelhandels-oder Debugbuilds verwendet werden. Wenn Sie nur Einzelhandels Builds Ihrer Komponente verteilen, können Sie alles in commonconfiguration platzieren und die anderen beiden Ordner weglassen.

    In jedem Konfigurations Ordner können Sie Architektur Ordner für plattformspezifische Dateien bereitstellen. Wenn Sie für alle Plattformen die gleichen Dateien verwenden, können Sie einen einzelnen Ordner mit dem Namen "neutral" angeben. Details zur Ordnerstruktur, einschließlich weiterer Architektur Ordnernamen, finden Sie unter Gewusst [wie: Erstellen eines Software Development Kits](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015). (In diesem Artikel werden sowohl Plattform-sdgs als auch Erweiterungs-sdgs erläutert. Möglicherweise ist es hilfreich, den Abschnitt zu Platform sdgs zu reduzieren, um Verwechslungen zu vermeiden. )

4.  Erstellen Sie eine SDK-Manifest-Datei. Das Manifest gibt den Namen und die Versionsinformationen, die von Ihrem SDK unterstützten Architekturen, .NET-Versionen und andere Informationen über die Art und Weise an, wie Visual Studio Ihr SDK verwendet. Ausführliche Informationen und ein Beispiel finden Sie unter Gewusst [wie: Erstellen eines Software Development Kits](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).
5.  Erstellen und verteilen Sie das Erweiterungs-SDK. Ausführliche Informationen, einschließlich lokalisieren und Signieren des VSIX-Pakets, finden Sie unter [VSIX-Bereitstellung](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015).

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen eines Software Development Kits](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [Nuget-Paketverwaltungssystem](https://github.com/NuGet/Home)
* [Ressourcen Verwaltungs System (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Suchen und Verwenden von Visual Studio-Erweiterungen](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Makepri. exe-Befehlsoptionen](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
