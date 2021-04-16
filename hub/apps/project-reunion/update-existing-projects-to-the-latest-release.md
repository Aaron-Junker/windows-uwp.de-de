---
description: Dieser Artikel enthält Anweisungen zum Aktualisieren eines Projekts, das mit einer früheren Vorschau- oder Releaseversion von Project Reunion oder WinUI 3 erstellt wurde, auf die neueste Version.
title: Aktualisieren vorhandener Projekte auf das neueste Release von Project Reunion
ms.topic: article
ms.date: 04/07/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b4836da83b6bbb2214cc6c5ad446144aac0e168d
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574736"
---
# <a name="update-existing-projects-to-the-latest-release-of-project-reunion"></a>Aktualisieren vorhandener Projekte auf das neueste Release von Project Reunion

Wenn Sie ein Projekt mit einer früheren Vorschauversion oder Releaseversion von Project Reunion oder WinUI 3 erstellt haben, können Sie das Projekt aktualisieren, um das neueste stabile Release (Version 0.5.5) zu verwenden.

> [!NOTE]
> Aufgrund der Einzigartigkeit jedes App-Szenarios kann es bei Verwendung dieser Anweisungen zu Problemen kommen. Bitte befolgen Sie die Anweisungen sorgfältig, und wenn Sie Probleme finden, [melden Sie einen Fehler in unserem GitHub-Repository](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="update-from-project-reunion-050"></a>Update von Project Reunion 0.5.0

Wenn Sie ein Projekt mit Project Reunion Version 0.5.0 erstellt haben, können Sie diese Anweisungen befolgen, um Ihr Projekt auf Project Reunion Version 0.5.5 (das neueste stabile Release) zu aktualisieren. Diese Version enthält mehrere wichtige Fehlerbehebungen.

> [!NOTE]
> Wenn Sie ein Projekt mithilfe von Project Reunion 0.5 VSIX erstellt haben, können Sie Ihr Projekt möglicherweise automatisch über den Visual Studio-Erweiterungs-Manager aktualisieren, ohne die folgenden manuellen Schritte ausführen zu müssen. Klicken Sie in Visual Studio 2019 auf   ->  **ErweiterungenErweiterungen verwalten,** und wählen Sie **updates** in der linken Menüleiste aus. Wählen Sie in der Liste "Project Reunion" aus, und klicken Sie auf **Aktualisieren.** 

Stellen Sie vor dem Start sicher, dass alle Voraussetzungen für Project Reunion 0.5 installiert sind, einschließlich des neuesten VSIX- und NuGet-Pakets für Project Reunion. Weitere Informationen finden Sie in den [Installationsanweisungen.](get-started-with-project-reunion.md#set-up-your-development-environment)

Gehen Sie zunächst wie folgt vor:
- Wenn Ihre **TargetPlatformMinVersion** in der WAPPROJ-Datei älter als 10.0.17763.0 ist, ändern Sie sie in 10.0.17763.0.

Nehmen Sie als Nächstes die folgenden Änderungen an Ihrem Projekt vor:
1. Um alle Änderungen aus der neuesten stabilen Version zu erhalten, müssen Sie Ihr .NET SDK explizit auf die neueste Version festlegen. Fügen Sie hierzu der CSPROJ-Datei die folgende Elementgruppe hinzu, und speichern Sie das Projekt:

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    Beachten Sie, dass diese Zeilen entfernt werden können, sobald .NET 5.0.6 im Mai verfügbar ist. 

3. Navigieren Sie in Visual Studio zu **Extras** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.

4. Geben Sie die folgenden Befehle ein:

    ```Console
    uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.5 -ProjectName {yourProjectName}
    ```

5. Nehmen Sie die folgenden Änderungen in Ihrer WAPPROJ-Datei der Anwendung (Paket) vor:
  
    1. Fügen Sie diesen Abschnitt hinzu:

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.5]">
                <IncludeAssets>build</IncludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

    2. Entfernen Sie die folgenden Zeilen:

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        Und:

        ```xml
        <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
        <Import Project="$(Microsoft_WinUI_AppX_targets)" />
        ```

        Und diese Elementgruppe:

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
            <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

## <a name="update-from-project-reunion-05-preview"></a>Update von Project Reunion 0.5 Preview

Wenn Sie ein Projekt mit Project Reunion 0.5 Preview erstellt haben, können Sie diese Anweisungen befolgen, um Ihr Projekt auf Project Reunion Version 0.5.5 (das neueste stabile Release) zu aktualisieren.

Bevor Sie beginnen, stellen Sie sicher, dass alle Project Reunion 0.5-Voraussetzungen installiert sind, einschließlich des neuesten Project Reunion VSIX- und NuGet-Pakets. Weitere Informationen finden Sie in den [Installationsanweisungen.](get-started-with-project-reunion.md#set-up-your-development-environment)

Führen Sie zunächst die folgenden Schritte aus:

- Wenn **Ihre TargetPlatformMinVersion** älter als 10.0.17763.0 ist, ändern Sie sie in der WAPPROJ-Datei in 10.0.17763.0.
- Wenn Ihre App das `Application.Suspending`-Ereignis verwendet, müssen Sie diese Zeile entfernen oder ändern, da `Application.Suspending` für Desktop-Apps nicht mehr aufgerufen wird. Weitere Informationen finden Sie in der [API-Referenzdokumentation](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending).
- Die Standardprojektvorlagen für C++- und C#-Apps enthielten die folgenden Zeilen. Entfernen Sie diese Zeilen unbedingt, wenn Sie im Code noch vorhanden sind:

    ```csharp
    this.Suspending += OnSuspending;
    ```

    ```cpp
    Suspending({ this, &App::OnSuspending });
    ```

Nehmen Sie als Nächstes die folgenden Änderungen an Ihrem Projekt vor:
1. Um alle Änderungen aus dem neuesten stabilen Release zu erhalten, müssen Sie Ihr .NET SDK explizit auf die neueste Version festlegen. Fügen Sie hierzu der CSPROJ-Datei die folgende Elementgruppe hinzu, und speichern Sie das Projekt:

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    Beachten Sie, dass diese Zeilen entfernt werden können, sobald .NET 5.0.6 im Mai verfügbar ist.

2. Navigieren Sie in Visual Studio zu **Extras** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
3. Geben Sie die folgenden Befehle ein:

    ```Console
    uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.5 -ProjectName {yourProjectName}
    ```

4. Nehmen Sie die folgenden Änderungen in Ihrer WAPPROJ-Datei der Anwendung (Paket) vor:
  
    1. Fügen Sie diesen Abschnitt hinzu:

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.5]">
                <IncludeAssets>build</IncludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

    2. Entfernen Sie die folgenden Zeilen:

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        Und

        ```xml
        <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
        <Import Project="$(Microsoft_WinUI_AppX_targets)" />
        ```

        Und diese Elementgruppe:

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
            <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

## <a name="update-from-winui-3-preview-4"></a>Update von WinUI 3 Preview 4

Wenn Sie ein Projekt mit WinUI 3 Preview 4 erstellt haben, können Sie diese Anweisungen befolgen, um Ihr Projekt auf Project Reunion Version 0.5.5 (das neueste stabile Release) zu aktualisieren.

Bevor Sie beginnen, stellen Sie sicher, dass alle Project Reunion 0.5-Voraussetzungen installiert sind, einschließlich des neuesten VsIX- und NuGet-Pakets für Project Reunion. Weitere Informationen finden Sie in den [Installationsanweisungen.](get-started-with-project-reunion.md#set-up-your-development-environment)

Führen Sie zunächst die folgenden Schritte aus:

- Wenn Ihre TargetPlatformMinVersion älter als 10.0.17763.0 ist, ändern Sie sie in der WAPPROJ-Datei in 10.0.17763.0.
- Wenn Ihre App das `Application.Suspending`-Ereignis verwendet, müssen Sie diese Zeile entfernen oder ändern, da `Application.Suspending` für Desktop-Apps nicht mehr aufgerufen wird. Weitere Informationen finden Sie in der [API-Referenzdokumentation](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true).
- Die Standardprojektvorlagen für C++- und C#-Apps enthielten die folgenden Zeilen. Entfernen Sie diese Zeilen unbedingt, wenn Sie im Code noch vorhanden sind:

    ```csharp
    this.Suspending += OnSuspending;
    ```

    ```cpp
    Suspending({ this, &App::OnSuspending });
    ```

Nehmen Sie als Nächstes die folgenden Änderungen an Ihrem Projekt vor:
1. Um alle Änderungen aus dem neuesten stabilen Release zu erhalten, müssen Sie Ihr .NET SDK explizit auf die neueste Version festlegen. Fügen Sie hierzu der CSPROJ-Datei die folgende Elementgruppe hinzu, und speichern Sie das Projekt:

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    Beachten Sie, dass diese Zeilen entfernt werden können, sobald .NET 5.0.6 im Mai verfügbar ist.
2. Navigieren Sie in Visual Studio zu **Extras** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
3. Geben Sie die folgenden Befehle ein:

    ```Console
    uninstall-package Microsoft.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.2 -ProjectName {yourProjectName}
    ```

4. Nehmen Sie die folgenden Änderungen in Ihrer WAPPROJ-Datei der Anwendung (Paket) vor:

    1. Fügen Sie diesen Abschnitt hinzu:

        ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.2]">
            <IncludeAssets>build</IncludeAssets>
          </PackageReference>
        </ItemGroup>
        ```

    2. Entfernen Sie die folgenden Zeilen:

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        ```xml
        <Import Project="$(AppxTargetsLocation)Microsoft.WinUI.AppX.targets" />
        ```

5. Löschen Sie die vorhandene `Microsoft.WinUI.AppX.targets`-Datei im Ordner „{IhrProjekt} (package)/build/“ Ihres Projekts.
