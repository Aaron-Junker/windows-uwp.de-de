---
title: Erste Schritte mit der Windows-UI-Bibliothek
description: Installieren und Verwenden der Windows-UI-Bibliothek.
ms.topic: reference
ms.date: 05/08/2020
keywords: windows 10, uwp, toolkit-sdk
ms.openlocfilehash: d96efb2f3de3084d74e06e70ff2811a944604f56
ms.sourcegitcommit: 47899c30a39087bca1f058a4395cf58daacf5ae9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85345474"
---
# <a name="getting-started-with-the-windows-ui-library"></a>Erste Schritte mit der Windows-UI-Bibliothek

[WinUI 2.4](release-notes/winui-2.4.md) ist die neueste stabile Version von WinUI und sollte für Apps in Produktionsumgebungen verwendet werden.

Die Bibliothek ist als NuGet-Paket verfügbar, die zu jedem neuen oder vorhandenen Visual Studio-Projekt hinzugefügt werden kann.

> [!NOTE]
> Weitere Informationen zum Testen von frühen Vorschauversionen von WinUI 3.0 findest du unter [WinUI 3.0 Preview 1](../winui3/index.md).

## <a name="download-and-install-the-windows-ui-library"></a>Herunterladen und Installieren der Windows UI-Bibliothek

1. Lade [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) herunter, und achte darauf, im Visual Studio-Installationsprogramm die Workload **Entwicklung für die Universelle Windows-Plattform** auszuwählen.

2. Öffne ein vorhandenes-Projekt, oder erstelle unter „Visual C#“ > „Windows“ > „Universell“ ein neues Projekt mit der Vorlage „Leere App“ oder der entsprechenden Vorlage für deine Sprachprojektion.  

    > [!IMPORTANT]
    > Um WinUI 2.4 verwenden zu können, musst du in den Projekteigenschaften TargetPlatformVersion >= 10.0.18362.0 und TargetPlatformMinVersion >= 10.0.15063.0 festlegen.

3. Klicke im Bereich des Projektmappen-Explorers mit der rechten Maustaste auf den Namen deines Projekts, und wähle **NuGet-Pakete verwalten** aus. Wähle die Registerkarte **Durchsuchen** aus, und suche nach **Microsoft.UI.Xaml** oder **WinUI**. Wähle dann aus, welche [NuGet-Pakete der Windows-UI-Bibliothek](nuget-packages.md) du verwenden möchtest.
Das **Microsoft.UI.Xaml**-Paket enthält Fluent-Steuerelemente und Features, die für alle Apps geeignet sind.  
Optional kannst du „Vorabversion einbeziehen“ markieren, um die neuesten Vorabversionen anzuzeigen, die experimentelle neue Features enthalten.

    ![NuGet-Pakete](images/ManageNugetPackages.png "Bild „NuGet-Pakete verwalten“")

    ![NuGet-Pakete](images/NugetPackages.png)

4. Füge die Ressourcen des Windows UI-Designs (WinUI) deinen App.xaml-Ressourcen hinzu. Dafür gibt es zwei Möglichkeiten, abhängig davon, ob du zusätzliche Anwendungsressourcen verwendest.

    ein. Wenn du keine weiteren Anwendungsressourcen verwendest, füge deiner Application.Resources `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` hinzu:

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. Andernfalls, wenn du mehr als einen Satz Anwendungsressourcen verwendest, füge Application.Resources.MergedDictionaries `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` hinzu:

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > Die Reihenfolge der Ressourcen, die einem ResourceDictionary hinzugefügt werden, wirkt sich auf die Reihenfolge aus, in der sie angewendet werden. Das `XamlControlsResources`-Wörterbuch überschreibt viele Standardressourcenschlüssel und sollte daher zuerst zu `Application.Resources` hinzugefügt werden, damit keine anderen benutzerdefinierten Stile oder Ressourcen in der App überschrieben werden. Weitere Informationen zum Laden von Ressourcen findest du unter [ResourceDictionary- und XAML-Ressourcenreferenz](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

5. Füge einen Verweis auf XAML-Seiten und deine Code-Behind-Seiten zum Toolkit hinzu.

    * Füge oben auf deiner XAML-Seite einen Verweis hinzu

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * In deinem Code kannst du eine using-Direktive hinzufügen (falls du die Typnamen verwenden möchtest, ohne sie zu qualifizieren).

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>Zusätzliche Schritte für C++/WinRT-Projekte

Wenn du einem C++/WinRT-Projekt ein NuGet-Paket hinzufügen möchtest, generieren die Tools einen Satz von Projektionsheadern im Ordner `\Generated Files\winrt` des Projekts. Wenn du diese Headerdateien in das Projekt einbinden möchtest, damit Verweise auf diese neuen Typen aufgelöst werden, kannst du zu deiner vorkompilierten Headerdatei wechseln (normalerweise `pch.h`) und sie einschließen. Unten findest du ein Beispiel, in dem die generierten Headerdateien für das **Microsoft.UI.Xaml**-Paket einbezogen werden.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Eine vollständige, schrittweise exemplarische Vorgehensweise zum Hinzufügen eine einfachen Unterstützung für die Windows UI-Bibliothek zu einem C++/WinRT-Projekt findest du unter [Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek](/windows/uwp/cpp-and-winrt-apis/simple-winui-example).

## <a name="contributing-to-the-windows-ui-library"></a>Mitwirken an der Windows-UI-Bibliothek

WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird.

Wir freuen uns über Fehlerberichte, Featureanforderungen und Communitycodebeiträge im [Repository zur Windows-UI-Bibliothek](https://aka.ms/winui).

## <a name="other-resources"></a>Weitere Ressourcen

Wenn du noch nicht mit UWP vertraut bist, empfehlen wir dir, die Seiten [Erste Schritte bei der UWP-Entwicklung](https://developer.microsoft.com/windows/getstarted) im Seiten im Entwicklerportal zu besuchen.
