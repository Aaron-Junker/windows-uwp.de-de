---
title: Erstellen von WinUI 3-Projekten
description: Zusammenfassung der WinUI 3-Projekt- und -Elementvorlagen, die in Visual Studio verfügbar sind.
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 823a8698d2d97207a69b31655437d02b70d05857
ms.sourcegitcommit: 99f7edfd79c44768751aca02dab30a03f41d2aae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218215"
---
# <a name="winui-3-project-templates-in-visual-studio"></a>WinUI 3-Projektvorlagen in Visual Studio

Nachdem Sie das Project Reunion 0.5-VSIX-Paket installiert haben, können Sie mithilfe einer der WinUI-Projektvorlagen in Visual Studio eine WinUI 3-App erstellen. Um auf die WinUI-Projektvorlagen im Dialogfeld **Neues Projekt erstellen** zuzugreifen, filtern Sie die Sprache auf **C++** oder **C#** , die Plattform auf **Windows** und den Projekttyp auf **WinUI**. Alternativ können Sie nach *WinUI* suchen und eine der verfügbaren C#- oder C++-Vorlagen auswählen.

![WinUI-Projektvorlagen](images/winui3-csharp-newproject.png)

> [!NOTE] 
> Weitere Informationen zum Einrichten Ihrer Entwicklungsumgebung, zu bekannten Problemen und mehr finden Sie in unseren neuesten [Versionshinweisen](../index.md).

Es gibt zwei Visual Studio-Erweiterungen, die als Teil des Project Reunion 0.5-Releases verfügbar sind: die **Project Reunion-VSIX** und die **Project Reunion Preview-VSIX**. Die Project Reunion-VSIX enthält Projektvorlagen, die zum Erstellen von MSIX-gepackte Desktop-Apps für die Produktion verwendet werden können. Die Project Reunion Preview-VSIX umfasst experimentelle Projektvorlagen, die zum Erstellen von MSIX-gepackten Desktop- oder UWP-Apps verwendet werden können. 

## <a name="project-templates-for-winui-3"></a>Projektvorlagen für WinUI 3

Sie können diese WinUI-Projektvorlagen verwenden, um Apps zu erstellen.

| Vorlage | Language | Beschreibung |
|----------|----------|-------------|
| Leere App, Gepackt (WinUI 3 in Desktop) | C# und C++ | Erstellt eine .NET 5 (C#)-Desktop- oder native Win32 (C++ )-App mit einer WinUI-basierten Benutzeroberfläche. Das generierte Projekt enthält ein einfaches Fenster, das von der **Microsoft.UI.Xaml.Window**-Klasse in der WinUI-Bibliothek abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. Weitere Informationen zu diesem Projekttyp finden Sie unter [Erste Schritte mit WinUI für Desktop-Apps](get-started-winui3-for-desktop.md).<p></p>Die Lösung umfasst außerdem ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr.  |
| Leere App (WinUI 3 in UWP), Experimentell | C# und C++ | Erstellt eine UWP-App mit einer WinUI-basierten Benutzeroberfläche. Das generierte Projekt enthält eine einfache Seite, die von der **Microsoft.UI.Xaml.Controls.Page**-Klasse in der WinUI-Bibliothek abgeleitet ist, die Sie als Ausgangspunkt verwenden können, um Ihre Benutzeroberfläche zu entwickeln. Weitere Informationen zu diesem Projekttyp finden Sie unter [Erste Schritte mit WinUI für UWP-Apps](get-started-winui3-for-uwp.md). |

Sie können diese WinUI-Projektvorlagen verwenden, um Komponenten zu erstellen, die von einer WinUI-basierten App geladen und verwendet werden können.

| Vorlage | Language | Beschreibung |
|----------|----------|-------------|
| Klassenbibliothek (WinUI 3 in Desktop) | Nur C# | Erstellt eine verwaltete .NET 5-Klassenbibliothek (DLL) in C# , die von anderen .NET 5-Desktop-Apps mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann.  |
| Klassenbibliothek (WinUI 3 in UWP), Experimentell  | Nur C# | Erstellt eine verwaltete Klassenbibliothek (DLL) in C#, die von anderen UWP-Apps mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann. |
| Komponente für Windows-Runtime (WinUI 3) | C++ | Erstellt eine [Komponente für Windows-Runtime](/windows/uwp/winrt-components/), die in C++/WinRT geschrieben ist, die von einer UWP-Desktop-App mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |
| Komponente für Windows-Runtime (WinUI 3 in UWP), Experimentell | C# | Erstellt eine [Komponente für Windows-Runtime](/windows/uwp/winrt-components/), die in C# geschrieben ist, die von einer UWP-App mit einer WinUI-basierten Benutzeroberfläche verwendet werden kann, unabhängig von der Programmiersprache, in der die App geschrieben ist. |

## <a name="item-templates-for-winui-3"></a>Elementvorlagen für WinUI 3

Die folgenden Elementvorlagen stehen für die Verwendung in einem WinUI-Projekt zur Verfügung. Um auf diese WinUI-Elementvorlagen zuzugreifen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Hinzufügen** -> **Neues Element** aus, und klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **WinUI**.

![WinUI-Elementvorlagen](images/winui3-addnewitem.png)

| Vorlage | Language | Beschreibung |
|----------|----------|-------------|
| Leere Seite (WinUI 3) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die eine neue Seite definieren, die von der **Microsoft.UI.Xaml.Controls.Page**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Leeres Fenster (WinUI 3 in Desktop) | C# und C++ | Fügt eine XAML- und Codedatei hinzu, die ein neues Fenster definiert, das von der **Microsoft.UI.Xaml.Window**-Klasse in der WinUI-Bibliothek abgeleitet ist. |
| Benutzerdefiniertes Steuerelement (WinUI 3) | C# und C++ | Fügt eine Codedatei zum Erstellen eines Steuerelements mit Vorlagen mit einem Standardstil hinzu. Das Steuerelement mit Vorlagen wird von der **Microsoft.UI.Xaml.Controls.Control**-Klasse in der WinUI-Bibliothek abgeleitet.<p></p>Eine exemplarische Vorgehensweise, in der die Verwendung dieser Elementvorlage veranschaulicht wird, finden Sie unter [XAML-Steuerelemente mit Vorlagen für UWP- und WinUI 3-Apps mit C++/WinRT](xaml-templated-controls-cppwinrt-winui-3.md) und [XAML-Steuerelemente in Vorlagen für UWP- und WinUI 3-Apps mit C#](xaml-templated-controls-csharp-winui-3.md). Weitere Informationen zu Steuerelementen mit Vorlagen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Ressourcenverzeichnis (WinUI 3) | C# und C++ | Fügt eine leere, mit Schlüsseln versehene Sammlung von XAML-Ressourcen hinzu. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Ressourcendatei (WinUI 3) | C# und C++ | Fügt eine Datei zum Speichern von Zeichenfolgen- und bedingten Ressourcen für Ihre App hinzu. Mithilfe dieses Elements können Sie Ihre App lokalisieren. Weitere Informationen finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im Paketmanifest der App](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Benutzersteuerelement (WinUI 3) | C# und C++ | Fügt eine XAML- und Codedatei zum Erstellen eines Benutzersteuerelements hinzu, das von der **Microsoft.UI.Xaml.Controls.UserControl**-Klasse in der WinUI-Bibliothek abgeleitet ist. In der Regel kapselt ein Benutzersteuerelement zugehörige vorhandene Steuerelemente und stellt eine eigene Logik bereit.<p></p>Weitere Informationen zu Benutzersteuerelementen finden Sie unter [Benutzerdefinierte XAML-Steuerelemente](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
