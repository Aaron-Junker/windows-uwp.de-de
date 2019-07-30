---
description: Dieses Thema führt Sie durch den Prozess zum Hinzufügen von einfacher Unterstützung für WinUI innerhalb eines C++/WinRT-Projekts.
title: Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek
ms.date: 07/12/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Windows UI Library, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 082e7ca0684495e1f67c2fa79b448866f68a059c
ms.sourcegitcommit: cba3ba9b9a9f96037cfd0e07d05bd4502753c809
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2019
ms.locfileid: "67870345"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Eine einfache C++/WinRT-Windows-UI-Beispielbibliothek

Dieses Thema führt durch den Prozess zum Hinzufügen einer einfachen Unterstützung für die Windows-UI-Bibliothek (WinUI) zu einem C++/WinRT-Projekt.

> [!NOTE]
> Das Toolkit für die Windows-UI-Bibliothek (WinUI) steht in Form von NuGet-Paketen zur Verfügung, die mithilfe von Visual Studio zu einem vorhandenen oder neuen Projekt hinzugefügt werden können, wie wir in diesem Thema sehen werden. Weitere Hintergrundinformationen sowie Details zu Einrichtung und Unterstützung findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Erstellen einer leeren App (HelloWinUICppWinRT)

Erstelle in Visual Studio ein neues Projekt anhand der Projektvorlage **Leere App (C++/WinRT)** , und nenne es *HelloWinUICppWinRT*.

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

Öffne als Nächstes `MainPage.xaml`. Im vorhandenen öffnenden **Application**-Tag befinden sich bereits einige XML-Namespacedeklarationen. Füge die XML-Namespacedeklaration `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"` hinzu. Füge dann das folgende Markup zwischen den vorhandenen öffnenden und schließenden **Page**-Tags ein. Damit wird das vorhandene **StackPanel**-Element überschrieben.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpageh-and-cpp-as-necessary"></a>Bearbeiten von „MainPage.h“ und „MainPage.cpp“ nach Bedarf

Bearbeite in `MainPage.h` die Includes, sodass sie wie folgt aussehen.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Zum Schluss löschst du in `MainPage.cpp` den Code in deiner Implementierung von **MainPage::ClickHandler**, da sich *myButton* nicht mehr im XAML-Markup befindet.

Jetzt kannst du das Projekt kompilieren und ausführen.

![Screenshot: einfache C++/WinRT-Windows-UI-Beispielbibliothek](images/winui.png)

## <a name="related-topics"></a>Verwandte Themen
* [Erste Schritte mit der Windows-UI-Beispielbibliothek](/uwp/toolkits/winui/getting-started)