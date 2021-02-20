---
title: Erste Schritte mit der Windows-UI-Bibliothek
description: Installieren und Verwenden der Windows-UI-Bibliothek.
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, toolkit-sdk
ms.openlocfilehash: 801c1f578c08df627264f542cbe1496d275afc0a
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334965"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Erste Schritte mit der Windows-UI 2.x-Bibliothek

[WinUI 2.5](release-notes/winui-2.5.md) ist die neueste stabile Version von WinUI und sollte für Apps in Produktionsumgebungen verwendet werden.

Die Bibliothek ist als NuGet-Paket verfügbar, die zu jedem neuen oder vorhandenen Visual Studio-Projekt hinzugefügt werden kann.

> [!NOTE]
> Weitere Informationen zum Ausprobieren früherer Vorschauversionen von WinUI 3 finden Sie unter [WinUI-Bibliothek 3 Vorschau 4 (Februar 2021)](../winui3/index.md).

## <a name="download-and-install-the-windows-ui-library"></a>Herunterladen und Installieren der Windows UI-Bibliothek

1. Lade [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) herunter, und achte darauf, im Visual Studio-Installationsprogramm die Workload **Entwicklung für die Universelle Windows-Plattform** auszuwählen.

2. Öffne ein vorhandenes-Projekt, oder erstelle unter „Visual C#“ > „Windows“ > „Universell“ ein neues Projekt mit der Vorlage „Leere App“ oder der entsprechenden Vorlage für deine Sprachprojektion.  

    > [!IMPORTANT]
    > Um WinUI 2.5 verwenden zu können, musst du in den Projekteigenschaften TargetPlatformVersion >= 10.0.18362.0 und TargetPlatformMinVersion >= 10.0.15063.0 festlegen.

3. Klicke im Bereich des Projektmappen-Explorers mit der rechten Maustaste auf den Namen deines Projekts, und wähle **NuGet-Pakete verwalten** aus. 

    :::image type="content" source="images/ManageNugetPackages.png" alt-text="Screenshot des Projektmappen-Explorer-Bereichs mit dem Projekt, auf das mit der rechten Maustaste geklickt wurde, und der hervorgehobenen Option „NuGet-Pakete verwalten“.":::<br/>*Projektmappen-Explorer-Bereich mit dem Projekt, auf das mit der rechten Maustaste geklickt wurde, und der hervorgehobenen Option „NuGet-Pakete verwalten“.*

4. Wählen Sie im **NuGet-Paket-Manager** die Registerkarte **Durchsuchen** aus, und suchen Sie nach **Microsoft.UI.Xaml** oder **WinUI**. Wählen Sie aus, welche [NuGet-Pakete der Windows-UI-Bibliothek](nuget-packages.md) Sie verwenden möchten (das **Microsoft.UI.Xaml**-Paket enthält Fluent-Steuerelemente und Features, die für alle Apps geeignet sind). Klicken Sie auf „Installieren“. 

    Aktivieren Sie das Kontrollkästchen „Vorabversion einbeziehen“, um die neuesten Vorabversionen anzuzeigen, die experimentelle neue Features enthalten.

    :::image type="content" source="images/NugetPackages.png" alt-text="Screenshot des Dialogfelds „NuGet-Paket-Manager“ mit der Registerkarte „Durchsuchen“, „winui“ im Suchfeld und aktivierter Option „Vorabversion einschließen“.":::<br/>*Dialogfeld „NuGet-Paket-Manager“ mit der Registerkarte „Durchsuchen“, „winui“ im Suchfeld und aktivierter Option „Vorabversion einschließen“.*

5. Fügen Sie die Ressourcen des Windows-UI-Designs (WinUI) zu Ihrer Datei „App.xaml“ hinzu.

    Dafür gibt es zwei Möglichkeiten, abhängig davon, ob du zusätzliche Anwendungsressourcen verwendest.

    ein. Wenn Sie keine anderen Anwendungsressourcen benötigen, fügen Sie das WinUI-Ressourcenelement `<XamlControlsResources` hinzu, wie im folgenden Beispiel gezeigt:

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>

    </Application>
    ```

    b. Wenn Sie mehr als eine Anwendungsressource benötigen, fügen Sie das WinUI-Ressourcenelement `<XamlControlsResources` in `<ResourceDictionary.MergedDictionaries>` hinzu, wie hier gezeigt:

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                    <ResourceDictionary Source="/Styles/Styles.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>

    </Application>
    ```

    > [!IMPORTANT]
    > Die Reihenfolge der Ressourcen, die einem ResourceDictionary hinzugefügt werden, wirkt sich auf die Reihenfolge aus, in der sie angewendet werden. Das `XamlControlsResources`-Wörterbuch überschreibt viele Standardressourcenschlüssel und sollte daher zuerst zu `Application.Resources` hinzugefügt werden, damit keine anderen benutzerdefinierten Stile oder Ressourcen in der App überschrieben werden. Weitere Informationen zum Laden von Ressourcen findest du unter [ResourceDictionary- und XAML-Ressourcenreferenz](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

6. Fügen Sie einen Verweis auf das WinUI-Paket zu XAML-Seiten und/oder Code-Behind-Seiten hinzu.

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