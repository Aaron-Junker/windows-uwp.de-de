---
description: Erstellen Sie eine Windows-Runtime Komponente mit c#-/WinRT, und nutzen Sie Sie in einer nativen Anwendung.
title: Erstellen einer C#/WinRT-Komponente und deren Verwenden aus C++/WinRT
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989643"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>Exemplarische Vorgehensweise: Erstellen einer/WinRT-Komponente in c# und deren Nutzung aus C++/WinRT

> [!NOTE]
> Die in diesem Artikel beschriebene Unterstützung der c#-/WinRT-Erstellung befindet sich derzeit in der Vorschauversion von c#/WinRT Version 1.1.2-Prerelease. 210208.6. Ab dieser Version ist die Verwendung nur für Frühes Feedback und Evaluierung vorgesehen.

C#/WinRT ermöglicht Entwicklern von .net 5 das Erstellen eigener Windows-Runtime Komponenten in c# mithilfe eines Klassen Bibliotheks Projekts. Erstellte Komponenten können in nativen Desktop Anwendungen als Paket Verweis oder als Projekt Verweis mit wenigen Änderungen genutzt werden.

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Windows-Runtime Komponente mit c#-/WinRT erstellen, die Komponente als nuget-Paket verteilen und die Komponente aus einer C++/WinRT-Konsolenanwendung nutzen. Den Beispielcode für diese exemplarische Vorgehensweise finden Sie [hier](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)auf GitHub.

Beachten Sie beim Erstellen der Laufzeitkomponente die Richtlinien und Typeinschränkungen, die in [diesem Artikel](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) beschrieben werden. Intern können die Windows-Runtime Typen in der Komponente beliebige .NET-Funktionen verwenden, die in einer UWP-App zulässig sind. Weitere Informationen finden Sie unter [.net für UWP-apps](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true). Extern können die Member ihres Typs nur Windows-Runtime Typen für Ihre Parameter und Rückgabewerte verfügbar machen.

> [!NOTE]
> Es gibt einige Windows-Runtime Typen, die [.NET-Typen zugeordnet](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)sind. Diese .NET-Typen können in der öffentlichen Schnittstelle Ihrer Windows-Runtime Komponente verwendet werden und werden den Benutzern der Komponente als entsprechende Windows-Runtime Typen angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

Diese exemplarische Vorgehensweise erfordert die folgenden Tools und Komponenten:

- Visual Studio 2019
- .Net 5,0 SDK
- [C++/WinRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) für C++/WinRT-Projektvorlagen

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>Erstellen einer einfachen Windows-Runtime Komponente mit c#/WinRT

Beginnen Sie mit dem Erstellen eines neuen Projekts in Visual Studio 2019. Wählen Sie die Projektvorlage **Klassenbibliothek (.net Core)** aus, und nennen Sie Sie **authoringdemo**. Sie müssen die folgenden Ergänzungen und Änderungen am Projekt vornehmen:

1. Aktualisieren `TargetFramework` Sie in der Datei " **authoringdemo. csproj** ", und fügen Sie die folgenden Elemente hinzu `PropertyGroup` :

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    Für das- `TargetFramework` Element können Sie einen der folgenden [zielframeworkmoniker](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)verwenden.
    - **NET 5.0-Windows 10.0.17763.0**
    - **NET 5.0-Windows 10.0.18362.0**
    - **NET 5.0-Windows 10.0.19041.0**

2. Installieren Sie die neueste Version des [nuget-Pakets "c#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6)".

    a. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projekt Knoten, und wählen Sie **nuget-Pakete verwalten** aus.

    b. Suchen Sie nach dem nuget-Paket **Microsoft. Windows. cswinrt** , und installieren Sie die neueste Version. In dieser exemplarischen Vorgehensweise wird c#/WinRT Version 1.1.2-Prerelease. 210208.6 verwendet.

3. Fügen Sie ein neues `PropertyGroup` Element hinzu, das mehrere c#-/WinRT-Eigenschaften festlegt.

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      Im folgenden finden Sie einige Details zu den Eigenschaften in diesem Beispiel. Eine vollständige Liste der c#-/WinRT-Projekteigenschaften finden Sie in der [c#/WinRT nuget-Dokumentation.](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - Die- `CsWinRTComponent` Eigenschaft gibt an, dass es sich bei dem Projekt um eine Windows-Runtime Komponente handelt, sodass für die Komponente eine winmd-Datei generiert wird.
    - Die- `CsWinRTWindowsMetadata` Eigenschaft stellt eine Quelle für Windows-Metadaten bereit. Dies ist ab Version 1.1.1 erforderlich.

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

Generieren Sie als nächstes ein nuget-Paket für die Komponente. Wenn Sie das Paket generieren, konfiguriert c#/WinRT die Komponente und hostingassemblys im Paket für die Nutzung durch native Anwendungen.

Es gibt mehrere Möglichkeiten, das nuget-Paket zu generieren:

* Wenn Sie jedes Mal, wenn Sie das Projekt erstellen, ein nuget-Paket generieren möchten, fügen Sie der **authoringdemo** -Projektdatei die folgende Eigenschaft hinzu, und erstellen Sie dann das Projekt neu.

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* Alternativ dazu können Sie ein nuget-Paket generieren, indem Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **authoringdemo** klicken und **Paket** auswählen.

Wenn Sie das Paket erstellen, sollte im Fenster **Erstellen** angezeigt werden, dass das nuget-Paket `AuthoringDemo.1.0.0.nupkg` erfolgreich erstellt wurde.

## <a name="consume-the-component-in-cwinrt"></a>Verwenden der Komponente in C++/WinRT

C#-/WinRT erstellte Windows-Runtime Komponenten können von nativen Anwendungen mit wenigen Änderungen genutzt werden. Die folgenden Schritte veranschaulichen, wie die erstellte Komponente oben in einer systemeigenen Konsolenanwendung aufgerufen wird. 

1. Fügen Sie der Projekt Mappe ein neues **Konsolen Anwendungsprojekt C++/WinRT** hinzu. Beachten Sie, dass dieses Projekt auch Teil einer anderen Lösung sein kann, wenn Sie dies auswählen.

    a. Klicken Sie in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und klicken Sie auf **Add**  ->  

    b. Suchen **Sie im Dialogfeld Neues Projekt hinzufügen** nach der Projektvorlage **C++/WinRT-Konsolenanwendung** . Wählen Sie die Vorlage aus, und klicken Sie auf **weiter**.

    c. Nennen Sie das neue Projekt **cppconsoleapp** , und klicken Sie auf **Erstellen**.

2. Fügen Sie einen Verweis auf die authoringdemo-Komponente hinzu, entweder als nuget-Paket oder Projekt Verweis.

    - **Option 1 (Paket Verweis)**:  

        a. Klicken Sie mit der rechten Maustaste auf das Projekt **cppconsoleapp** und wählen Sie **nuget-Pakete verwalten**. Möglicherweise müssen Sie die Paketquellen so konfigurieren, dass Sie einen Verweis auf das nuget-Paket "authoringdemo" hinzufügen. Klicken Sie hierzu im nuget-Paket-Manager auf das Symbol " **Einstellungen** ", und fügen Sie dem entsprechenden Pfad eine Paketquelle hinzu.

        ![Nuget-Einstellungen](images/nuget-sources-settings.png)

        b. Nachdem Sie die Paketquellen konfiguriert haben, suchen Sie nach dem **authoringdemo** -Paket, und klicken Sie auf **Installieren**.

        ![NuGet-Paket installieren](images/install-authoring-nuget.png)

    - **Option 2 (Projekt Verweis)**:
        
        a. Klicken Sie mit der rechten Maustaste auf das Projekt **cppconsoleapp** und wählen Sie Verweis **Hinzufügen**  ->   Fügen Sie unter dem Knoten **Projekte** einen Verweis auf das Projekt **authoringdemo** hinzu. Ab dieser Vorschauversion müssen Sie auch einen Datei Verweis auf " **authoringdemo. winmd** " aus dem Knoten **Durchsuchen** hinzufügen. Die generierte winmd-Datei befindet sich im Ausgabeverzeichnis des Projekts " **authoringdemo** ".

        b. Für diese Vorschau müssen Sie auch die folgende Eigenschaften Gruppe zu **cppconsoleapp. vcxproj** hinzufügen. Zum Bearbeiten der Projektdatei für native Anwendungen klicken Sie zunächst mit der rechten Maustaste auf den Projekt Knoten **cppconsoleapp** , und wählen Sie **Projekt entladen** aus.

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

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

4. Ändern Sie das Projekt, um beim Bereitstellen des Projekts die runtimeconfig.jsfür-und-Manifest-Dateien in der Ausgabe einzuschließen. Klicken Sie für die Dateien **WinRT.Host.runtimeconfig.json** und **CppConsoleApp.exe. Manifest** auf die Datei in **Projektmappen-Explorer** , und legen Sie die **Content** -Eigenschaft auf **true** fest. Im folgenden finden Sie ein Beispiel dafür, wie dies aussieht.

    ![Inhalt bereitstellen](images/deploy-content.png)

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

## <a name="related-topics"></a>Verwandte Themen

- [Beispielcode](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [Erstellen von Komponenten](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [Verwaltetes komponentenhosting](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
