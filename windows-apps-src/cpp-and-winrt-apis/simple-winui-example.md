---
description: Dieses Thema führt Sie durch den Prozess zum Hinzufügen von grundlegender Unterstützung für WinUI innerhalb eines C++/WinRT-Projekts.
title: Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Windows UI Library, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: b7323ba63bedfc1ed560effb4da8ea61d1527897
ms.sourcegitcommit: 71701f5ffc540951f86d6f77a52416c6d75fe305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100632681"
---
# <a name="a-basic-cwinrt-windows-ui-library-example"></a>Eine grundlegende C++/WinRT-Windows-UI-Beispielbibliothek

Dieses Thema führt durch den Prozess zum Hinzufügen einer grundlegenden Unterstützung für die [Windows-UI-Bibliothek (WinUI)](https://github.com/Microsoft/microsoft-ui-xaml) zu einem C++/WinRT-Projekt. Die Windows-UI-Bibliothek ist übrigens ebenfalls in C++/WinRT geschrieben.

> [!NOTE]
> Das Toolkit für die Windows-UI-Bibliothek (WinUI) steht in Form von NuGet-Paketen zur Verfügung, die mithilfe von Visual Studio zu einem vorhandenen oder neuen Projekt hinzugefügt werden können, wie wir in diesem Thema sehen werden. Weitere Hintergrundinformationen sowie Details zu Einrichtung und Unterstützung findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Erstellen einer leeren App (HelloWinUICppWinRT)

Erstelle in Visual Studio ein neues Projekt anhand der Projektvorlage **Leere App (C++/WinRT)** . Stelle sicher, dass du die Vorlage **(C++/WinRT)** und nicht die Vorlage **(Universelles Windows)** verwendest.

Nenne das neue Projekt *HelloWinUICppWinRT* und deaktiviere **Legen Sie die Projektmappe und das Projekt im selben Verzeichnis ab** (damit die Ordnerstruktur mit der exemplarischen Vorgehensweise übereinstimmt).

## <a name="install-the-microsoftuixaml-nuget-package"></a>Installieren des Microsoft.UI.Xaml-NuGet-Pakets

Klicke auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Durchsuchen**, gib **Microsoft.UI.Xaml** in das Suchfeld ein, wähle das Element in den Suchergebnissen aus, und klicke dann auf **Installieren**, um das Paket für das Projekt zu installieren (es wird auch eine Aufforderung zur Zustimmung zu einer Lizenzvereinbarung angezeigt). Installiere nur das Paket **Microsoft.UI.Xaml**, nicht **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Deklarieren von WinUI-Anwendungsressourcen

Öffne `App.xaml`, und füge das folgende Markup zwischen den vorhandenen öffnenden und schließenden **Application**-Tags ein.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Hinzufügen eines WinUI-Steuerelements zu MainPage

Öffne als Nächstes `MainPage.xaml`. Im vorhandenen öffnenden **Seite**-Tag befinden sich bereits einige XML-Namespacedeklarationen. Füge die XML-Namespacedeklaration `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"` hinzu. Füge dann das folgende Markup zwischen den vorhandenen öffnenden und schließenden **Page**-Tags ein. Damit wird das vorhandene **StackPanel**-Element überschrieben.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-pchh-as-necessary"></a>Bearbeiten Sie „pch.h" nach Bedarf.

Wenn du einem C++/WinRT-Projekt ein NuGet-Paket hinzufügst (z. B. das Paket **Microsoft.UI.Xaml**, das du zuvor hinzugefügt hast) und das Projekt erstellst, generieren die Tools einen Satz von Projektionsheaderdateien im Ordner `\Generated Files\winrt` des Projekts. Wenn du die exemplarische Vorgehensweise befolgt hast, besitzt du jetzt einen Ordner `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. Wenn du diese Headerdateien in das Projekt einbinden möchtest, damit Verweise auf diese neuen Typen aufgelöst werden, kannst du zu deiner vorkompilierten Headerdatei wechseln (normalerweise `pch.h`) und sie einschließen.

Sie brauchen nur die Überschriften für die von Ihnen verwendeten Typen einzubeziehen. Hier finden Sie jedoch ein Beispiel, in dem alle generierten Headerdateien für das **Microsoft.UI.Xaml**-Paket einbezogen werden.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>Bearbeiten von „MainPage.cpp“

Lösche in `MainPage.cpp` den Code in deiner Implementierung von **MainPage::ClickHandler**, da sich *myButton* nicht mehr im XAML-Markup befindet.

Jetzt kannst du das Projekt kompilieren und ausführen.

![Screenshot: einfache C++/WinRT-Windows-UI-Beispielbibliothek](images/winui.png)

## <a name="related-topics"></a>Zugehörige Themen
* [Erste Schritte mit der Windows-UI-Beispielbibliothek](/uwp/toolkits/winui/getting-started)