---
description: Erstellen Sie eine Windows-Runtime Komponente mit c#-/WinRT, und nutzen Sie Sie in einer nativen Anwendung.
title: Erstellen Sie eine c#-/WinRT-Komponente, und nutzen Sie Sie aus C++/WinRT
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d489e293ca40aa38c27e4c3e19bba6f8a6705e3b
ms.sourcegitcommit: 6f15cc14e0c4c13999c862664fa7a70de8730b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987104"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>Exemplarische Vorgehensweise: Erstellen einer/WinRT-Komponente in c# und deren Nutzung aus C++/WinRT

> [!NOTE]
> Die in diesem Artikel beschriebene Unterstützung für die/WinRT-Erstellung in c# befindet sich derzeit in der Vorschauversion von c#/WinRT Version 1.1.1. Ab dieser Version ist die Verwendung nur für Frühes Feedback und Evaluierung vorgesehen.

C#/WinRT ermöglicht Entwicklern von .net 5 das Erstellen eigener Windows-Runtime Komponenten in c# mithilfe eines Klassen Bibliotheks Projekts. Erstellte Komponenten können in nativen Desktop Anwendungen mit einem Paket Verweis oder einem **winmd** -Datei Verweis genutzt werden.

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie c#-/WinRT verwenden können, um eigene Windows-Runtime Typen zu erstellen, Sie als Windows-Runtime Komponente zu verpacken und die Komponente aus einer C++/WinRT-Konsolenanwendung zu nutzen.

Wenn Sie die Laufzeitkomponente erstellen, befolgen Sie die in [diesem Artikel](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) beschriebenen Richtlinien und Typeinschränkungen. die Windows-Runtime Typen in der Komponente können sämtliche .NET-Funktionen verwenden, die in einer UWP-App zulässig sind. Weitere Informationen finden Sie unter [.net für UWP-apps](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true). Extern können die Member ihres Typs nur Windows-Runtime Typen für Ihre Parameter und Rückgabewerte verfügbar machen.

> [!NOTE]
> Es gibt einige Windows-Runtime Typen, die [.NET-Typen zugeordnet](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)sind. Diese .NET-Typen können in der öffentlichen Schnittstelle Ihrer Windows-Runtime Komponente verwendet werden und werden den Benutzern der Komponente als entsprechende Windows-Runtime Typen angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

Diese exemplarische Vorgehensweise erfordert die folgenden Tools und Komponenten:

- Visual Studio 2019
- .Net 5,0 SDK
- [C++/WinRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) für C++/WinRT-Projektvorlagen

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>Erstellen einer einfachen Windows-Runtime Komponente mit c#/WinRT

Beginnen Sie mit dem Erstellen eines neuen Projekts in Visual Studio 2019. Wählen Sie die Projektvorlage **Klassenbibliothek (.net Core)** aus, und nennen Sie Sie **authoringdemo**. Sie müssen die folgenden Ergänzungen und Änderungen am Projekt vornehmen:

1. Installieren Sie die neueste Version des [nuget-Pakets "c#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)".

    a. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projekt Knoten, und wählen Sie **nuget-Pakete verwalten** aus.

    b. Suchen Sie nach dem nuget-Paket **Microsoft. Windows. cswinrt** , und installieren Sie die neueste Version. In dieser exemplarischen Vorgehensweise wird c#/WinRT Version 1.1.1 verwendet.

2. Aktualisieren `TargetFramework` Sie in der Datei " **authoringdemo. csproj** ", und fügen Sie die folgenden Elemente hinzu `PropertyGroup` :

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
        <AssemblyVersion>1.0.0.0</AssemblyVersion>
    </PropertyGroup>
    ```

    Für das- `TargetFramework` Element können Sie einen der folgenden [zielframeworkmoniker](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)verwenden.
    - **NET 5.0-Windows 10.0.17763.0**
    - **NET 5.0-Windows 10.0.18362.0**
    - **NET 5.0-Windows 10.0.19041.0**

    Außerdem müssen Sie einen `AssemblyVersion` für die Windows-Runtime Komponente angeben.

3. Fügen Sie ein neues `PropertyGroup` Element hinzu, das mehrere **cswinrt** -Eigenschaften festlegt.

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
        <CsWinRTEnableLogging>true</CsWinRTEnableLogging>
        <GeneratedFilesDir Condition="'$(GeneratedFilesDir)'==''">$([MSBuild]::NormalizeDirectory('$(MSBuildProjectDirectory)', '$(IntermediateOutputPath)', 'Generated Files'))</GeneratedFilesDir>
    </PropertyGroup>
      ```

      Im folgenden finden Sie einige Details zu den Eigenschaften in diesem Beispiel. Eine vollständige Liste der cswinrt-Projekteigenschaften finden Sie in der [cswinrt-nuget-Dokumentation.](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - Die- `CsWinRTComponent` Eigenschaft gibt an, dass es sich bei dem Projekt um eine Windows-Runtime Komponente handelt, sodass für die Komponente eine winmd-Datei generiert wird.
    - Die- `CsWinRTWindowsMetadata` Eigenschaft stellt eine Quelle für Windows-Metadaten bereit. Dies ist ab Version 1.1.1 erforderlich.
    - Die- `CsWinRTEnableLogging` Eigenschaft generiert bei der Erstellung der Laufzeitkomponente eine **log.txt** -Datei mit einer detaillierten Ausgabe.
    - Die- `GeneratedFilesDir` Eigenschaft ist erforderlich, um die **winmd** -Datei im richtigen Ausgabeverzeichnis zu erstellen. Dies ist ab Version 1.1.1 erforderlich.

4. Sie können Ihre Lauf Zeit Klassen mithilfe von Bibliotheks Klassendateien **(CS)** erstellen. Klicken Sie mit der rechten Maustaste auf die Datei **Class1.cs** , und benennen Sie Sie in **example.cs** um Fügen Sie dieser Datei den folgenden Code hinzu, der der Lauf Zeit Klasse eine öffentliche Eigenschaft und Methode hinzufügt. Denken Sie daran, alle Klassen, die Sie in der Lauf Zeit **Komponente verfügbar** machen möchten, zu markieren.

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. Sie können nun das Projekt erstellen, um die Metadatendatei für die Komponente zu generieren. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und **Klicken Sie auf** Die generierte **authoringdemo. winmd** -Datei wird in der **Projektmappen-Explorer** im Ordner **generierte Dateien** und auch in Ihrem Buildausgabeordner angezeigt.

## <a name="generate-a-nuget-package-for-the-component"></a>Generieren eines nuget-Pakets für die Komponente

Um die Laufzeitkomponente als nuget-Paket zu verteilen, müssen Sie die folgenden Änderungen am **authoringdemo** -Projekt vornehmen. Wenn Sie ein nuget-Paket für die Komponente nicht generieren möchten, können native Anwendungen die Komponente auch mit einem direkten Verweis auf die generierte **winmd** -Datei nutzen, wie im folgenden Abschnitt gezeigt.

1. Fügen Sie eine Zieldatei hinzu, damit native Anwendungen auf das generierte nuget-Paket verweisen und die Komponente nutzen können. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **authoringdemo** , und wählen Sie **Hinzufügen-> neues Element** aus. Suchen Sie nach der **XML-Datei** Vorlage, und benennen Sie die Datei **authoringdemo. targets**.

    > [!NOTE]
    > Die Zieldatei **muss** mit dem Namen der Komponente benannt werden, und zwar mit dem Format *yourcomponentname. targets*.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <Import Project="$(MSBuildThisDirectory)AuthoringDemo.CsWinRT.targets" />
    </Project> 
    ```

   Die importierte Datei " **authoringdemo. cswinrt. targets** " wird dem nuget-Paket hinzugefügt, das das Paket mit c#-/WinRT-hostingassemblys konfiguriert, um die Nutzung von systemeigenen Anwendungen zu ermöglichen.  

2. Fügen Sie der Projektdatei " **authoringdemo. csproj** " die folgenden Elemente hinzu.

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

    <ItemGroup>
        <Content Include="AuthoringDemo.targets" PackagePath="build;buildTransitive"/>
    </ItemGroup>
    ```

    Mit diesen Eigenschaften wird ein nuget-Paket für die Komponente generiert, und die cswinrt-Ziele werden im Paket für die Nutzung durch native Anwendungen eingeschlossen.

3. Erstellen Sie das **authoringdemo** -Projekt erneut. In der Buildausgabe sollte nun angezeigt werden, dass das nuget-Paket "authoringdemo. 1.0.0. nupkg" erfolgreich erstellt wurde.

## <a name="consume-the-component-in-cwinrt"></a>Verwenden der Komponente in C++/WinRT

C#-/WinRT erstellte Windows-Runtime Komponenten können von nativen Anwendungen mit wenigen Änderungen genutzt werden. Die folgenden Schritte veranschaulichen, wie die erstellte Komponente oben in einer systemeigenen Konsolenanwendung aufgerufen wird.

1. Fügen Sie der Projekt Mappe ein neues **Konsolen Anwendungsprojekt C++/WinRT** hinzu. Beachten Sie, dass dieses Projekt auch Teil einer anderen Lösung sein kann, wenn Sie dies auswählen.

    a. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und klicken Sie auf **Add**  ->  

    b. Suchen **Sie im Dialogfeld Neues Projekt hinzufügen** nach der Projektvorlage **C++/WinRT-Konsolenanwendung** . Wählen Sie die Vorlage aus, und klicken Sie auf **weiter**.

    c. Nennen Sie das neue Projekt **cppconsoleapp** , und klicken Sie auf **Erstellen**.

2. Fügen Sie einen Verweis auf die authoringdemo-Komponente hinzu. Sie können entweder einen Paket Verweis zum generierten nuget-Paket aus dem vorherigen Abschnitt oder einen direkten Verweis auf " **authoringdemo. winmd**" hinzufügen.

    - **Option 1 (Paket Verweis)**: Klicken Sie mit der rechten Maustaste auf das Projekt **cppconsoleapp** , und wählen Sie **nuget-Pakete verwalten** aus. Möglicherweise müssen Sie die Paketquellen so konfigurieren, dass Sie einen Verweis auf das nuget-Paket "authoringdemo" hinzufügen. Klicken Sie hierzu im nuget-Paket-Manager auf das Zahnrad Symbol für Einstellungen, und fügen Sie dem entsprechenden Pfad eine Paketquelle hinzu.

        ![Nuget-Einstellungen](images/nuget-sources-settings.png)

        Nachdem Sie die Paketquellen konfiguriert haben, suchen Sie nach dem **authoringdemo** -Paket, und klicken Sie auf **Installieren**.

        ![NuGet-Paket installieren](images/install-authoring-nuget.png)

    - **Option 2 (direkte Referenz)**: Klicken Sie mit der rechten Maustaste auf das Projekt **cppconsoleapp** , und klicken Sie auf **Add-> Reference**. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen **Sie die Datei** " **authoringdemo. winmd** ", und wählen Sie Sie aus.

3. Um das Hosting der-Komponente zu unterstützen, müssen Sie eine runtimeconfig.jsfür die Datei und eine Manifest-Datei hinzufügen. Weitere Informationen zum Hosting verwalteter Komponenten finden Sie in [diesen hostingdokumentationen](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md).

    a. Um das runtimeconfig.jsfür die Datei hinzuzufügen, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Add-> neues Element**. Suchen Sie nach der **Textdatei** -Vorlage, und benennen Sie Sie **WinRT.Host.runtimeconfig.js** ein. Fügen Sie den folgenden Inhalt ein:

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    Hinweis für den `tfm` Eintrag kann mit der DOTNET_ROOT-Umgebungsvariable auf eine benutzerdefinierte eigenständige .net 5-Installation verwiesen werden.

    b. Um die Manifest-Datei hinzuzufügen, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen-> neues Element**. Suchen Sie nach der **Textdatei** -Vorlage, und benennen Sie Sie **CppConsoleApp.exe. Manifest**. Fügen Sie den folgenden Inhalt ein:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    Die Manifest-Datei ist für nicht-Paketanwendungen erforderlich. Geben Sie in dieser Datei Ihre Lauf Zeit Klassen mithilfe von aktivierbaren Einträge für die Gruppen Registrierungen wie oben gezeigt an.

4. Bearbeiten Sie die systemeigene Projektdatei, um die runtimeconfig-und Manifest-Dateien in die Bereitstellung des Projekts einzubeziehen. Klicken Sie mit der rechten Maustaste auf das Projekt und dann auf **Projekt entladen**. Nachdem das Projekt entladen wurde, klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie **Projektdatei bearbeiten** Suchen Sie nach den Einträgen für **WinRT.Host.runtimeconfig.json** und **CppConsoleApp.exe. Manifest**, und fügen Sie die- `DeploymentContent` Eigenschaft wie unten gezeigt hinzu.

    ```xml
    <ItemGroup>
        <None Include="WinRT.Host.runtimeconfig.json">
            <DeploymentContent>true</DeploymentContent>
        </None>

        <Manifest Include="CppConsoleApp.exe.manifest">
            <DeploymentContent>true</DeploymentContent>
        </Manifest>
    </ItemGroup> 
    ```

5. Öffnen Sie " **PCH. h** " unter den Header Dateien des Projekts, und fügen Sie die folgende Codezeile hinzu, um Ihre Komponente einzubeziehen.

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. Öffnen Sie **Main. cpp** unter den Quelldateien des Projekts, und ersetzen Sie es durch den folgenden Inhalt.

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. Erstellen Sie das Projekt **cppconsoleapp** , und führen Sie es aus. Nun sollte die Ausgabe unten angezeigt werden.

    ![C++/WinRT Konsolenausgabe](images/consume-component-output.png)

## <a name="related-topics"></a>Zugehörige Themen

- [Erstellen von Komponenten](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [Verwaltetes komponentenhosting](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
