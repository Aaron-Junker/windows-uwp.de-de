---
description: In dieser exemplarischen Vorgehensweise wird die Verwendung von c#-/WinRT zum Generieren einer .net 5-Projektion für eine C++/WinRT-Komponente erläutert.
title: Exemplarische Vorgehensweise zum Generieren einer .net 5-Projektion aus einer C++/WinRT-Komponente und zum Verteilen von nuget
ms.date: 10/12/2020
ms.topic: article
keywords: Windows 10, c#, WinRT, cswinrt, Projektion
ms.localizationpriority: medium
ms.openlocfilehash: 3116e176c8f156939f075e0a23d1be2352a8ecde
ms.sourcegitcommit: 861c381a31e4a5fd75f94ca19952b2baaa2b72df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171145"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>Exemplarische Vorgehensweise: Generieren einer .net 5-Projektion aus einer C++/WinRT-Komponente und Verteilen von nuget

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie [c#-/WinRT](index.md) verwenden, um eine .net 5-Projektion für eine C++/WinRT-Komponente zu generieren, das zugehörige nuget-Paket zu erstellen und auf das nuget-Paket aus einer .net 5 c#-Konsolenanwendung

Sie können das vollständige Beispiel für diese exemplarische Vorgehensweise von [GitHub herunter](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)laden.

> [!NOTE]
> Diese exemplarische Vorgehensweise wird für die neueste Vorschauversion von c#/WinRT (RC2) geschrieben. Wir erwarten, dass die bevorstehende Version von 1,0 weitere Updates und Verbesserungen an der Entwicklerumgebung bietet.

## <a name="prerequisites"></a>Voraussetzungen

Diese exemplarische Vorgehensweise und das entsprechende Beispiel erfordern die folgenden Tools und Komponenten:

- [Visual Studio 16,8 Preview 3](https://visualstudio.microsoft.com/vs/preview/) (oder höher) mit installierter universelle Windows-Plattform entwicklungworkloads. Aktivieren Sie unter **Installations Details**  >  **universelle Windows-Plattform Entwicklung**die Option **C++ (v14x) universelle Windows-Plattform Tools** .
- [.Net 5,0 rc2 SDK](https://github.com/dotnet/installer).
- [C++/WinRT VSIX-Erweiterung](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) für C++/WinRT-Projektvorlagen.

## <a name="create-a-simple-cwinrt-runtime-component"></a>Erstellen einer einfachen C++/WinRT-Laufzeitkomponente

Um diese exemplarische Vorgehensweise durchführen zu können, müssen Sie zunächst über eine C++/WinRT-Komponente verfügen, für die eine .net 5-Projektion erstellt wird. In dieser exemplarischen Vorgehensweise wird das Projekt **simplemathcomponent** im zugehörigen Beispiel von [GitHub verwendet](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample/SimpleMathComponent). Dabei handelt es sich um ein **Windows-Runtime Component-Projekt (C++/WinRT)** , das mit der [Erweiterung C++/WinRT VSIX](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)erstellt wurde. Nachdem Sie das Projekt auf den Entwicklungs Computer kopiert haben, öffnen Sie die Projekt Mappe in Visual Studio 2019 Preview.

Der Code in diesem Projekt stellt die Funktionalität für die grundlegenden mathematischen Vorgänge bereit, die in der folgenden Header Datei angezeigt werden. 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

Ausführlichere Schritte zum Erstellen einer C++/WinRT-Komponente und zum Erstellen einer winmd-Datei finden Sie unter [Windows-Runtime Komponenten mit C++/WinRT](https://docs.microsoft.com/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt).

> [!NOTE]
> Wenn Sie [iinspectable:: getruntimeclassname](https://docs.microsoft.com/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) in der Komponente implementieren, **muss** ein gültiger WinRT-Klassenname zurückgegeben werden. Da c#/WinRT die Klassennamen Zeichenfolge für Interop verwendet, wird durch einen falschen Lauf Zeit Klassennamen eine **InvalidCastException**ausgelöst.

## <a name="add-a-projection-project-to-the-component-solution"></a>Hinzufügen eines Projektions Projekts zur Komponenten Lösung

Wenn Sie das Beispiel aus dem Repository geklont haben, löschen Sie zuerst das Projekt **simplemathprojection** , um die exemplarische Vorgehensweise schrittweise durchzugehen.

1. Fügen Sie der Projekt Mappe ein neues **Klassen Bibliotheksprojekt (.net Core)** hinzu.

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappenknoten, und klicken Sie auf **Add**  ->  **New Project**
    2. Suchen **Sie im Dialogfeld Neues Projekt hinzufügen**nach der Projektvorlage **Klassenbibliothek (.net Core)** . Wählen Sie die Vorlage aus, und klicken Sie auf **weiter**.
    3. Nennen Sie das neue Projekt **simplemathprojection** , und klicken Sie auf **Erstellen**.

2. Löschen Sie die leere Datei **Class1.cs** aus dem Projekt.

3. Installieren Sie das [nuget-Paket "c#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)".

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt **simplemathprojection** , und wählen Sie **nuget-Pakete verwalten**. 
    2. Suchen Sie nach dem nuget-Paket **Microsoft. Windows. cswinrt** , und installieren Sie die neueste Version.

4. Fügen Sie dem **simplemathcomponent** -Projekt einen Projekt Verweis hinzu. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste unter dem Projekt **simplemathprojection** auf den Knoten **Abhängigkeiten** , wählen Sie **Projekt Verweis hinzufügen**aus, und wählen Sie das Projekt **simplemathcomponent** aus.

    > [!NOTE]
    > Wenn Sie Visual Studio 16,8 Preview 4 oder höher verwenden, sind Sie nach Abschluss von Schritt 4 mit diesem Abschnitt fertig. Wenn Sie Visual Studio 16,8 Preview 3 verwenden, müssen Sie auch Schritt 5 ausführen.

5. Wenn Sie Visual Studio 16,8 Preview 3 verwenden: Doppelklicken Sie in **Projektmappen-Explorer**auf den Knoten **simplemathprojection** , um die Projektdatei im Editor zu öffnen, fügen Sie der Datei die folgenden Elemente hinzu, und speichern und schließen Sie dann die Datei.

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Net.Compilers.Toolset" Version="3.8.0-4.20472.6" />
    </ItemGroup>

    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json
      </RestoreSources>
    </PropertyGroup>
    ```

    Diese Elemente installieren die erforderliche Version des **Microsoft.net. Compilers. Toolset** -nuget-Pakets, das den neuesten c#-Compiler enthält. In dieser exemplarischen Vorgehensweise wird das nuget-Paket über diese Projektdatei Verweise installiert, da die erforderliche Version dieses Pakets im standardmäßigen öffentlichen nuget-Feed möglicherweise nicht verfügbar ist.

Nachdem Sie diese Schritte ausgeführt haben, sollte die **Projektmappen-Explorer** in etwa wie folgt aussehen.

![Projektmappen-Explorer, die Projektions Projekt Abhängigkeiten anzeigt](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>Bearbeiten Sie die Projektdatei, um c#-/WinRT auszuführen.

Bevor Sie **cswinrt.exe** aufrufen und die projektionsassembly generieren können, müssen Sie die Projektdatei für das Projektions Projekt bearbeiten.

1. Doppelklicken Sie in **Projektmappen-Explorer**auf den Knoten **simplemathprojection** , um die Projektdatei im Editor zu öffnen.

2. Aktualisieren Sie das- `TargetFramework` Element, um auf das Windows SDK zu verweisen Dadurch werden assemblydeseritäten hinzugefügt, die für die Interop-und Projektions Unterstützung erforderlich sind. Unser Beispiel bezieht sich auf die neueste Version von Windows 10 in dieser exemplarischen Vorgehensweise, **net 5.0-Windows 10.0.19041.0** (auch bekannt als SDK-Version 2004).

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Fügen Sie ein neues `PropertyGroup` Element hinzu, das mehrere **cswinrt** -Eigenschaften festlegt.

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    Im folgenden finden Sie einige Details zu den Einstellungen in diesem Beispiel:

    - Die- `CsWinRTIncludes` Eigenschaft gibt an, welche Namespaces zu projizieren sind.
    - Die- `CsWinRTGeneratedFilesDir` Eigenschaft legt das Ausgabeverzeichnis fest, in dem Dateien aus der Projektion generiert werden. Diese werden im folgenden Abschnitt zum Aufbauen der Quelle festgelegt.

4. Die neueste Version von c#/WinRT ab dieser exemplarischen Vorgehensweise erfordert möglicherweise die Angabe von Windows-Metadaten. Dies wird in einer zukünftigen Version von c# korrigiert/WinRT. Dies kann mit einem der folgenden Optionen angegeben werden:

    - Ein Paket Verweis (z. b. [Microsoft. Windows. SDK. Contracts]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/)) oder
    - Ein expliziter Wert hat den mit der-Eigenschaft festgelegt `CsWinRTWindowsMetadata` :

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. Speichern und schließen Sie die Datei **simplemathprojection. csproj** .

## <a name="build-projects-out-of-source"></a>Projekte außerhalb der Quelle erstellen

Im [zugehörigen Beispiel](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)wird der Build mit der Datei " **Directory. Build.** -Eigenschaften" konfiguriert. Die generierten Dateien aus der Erstellung des Projekts **simplemathcomponent** und **simplemathprojection** werden im Ordner *_build* auf Projektmappenebene angezeigt. Wenn Sie Ihre Projekte so konfigurieren möchten, dass Sie aus der Quelle erstellt werden, kopieren Sie die Datei " **Directory. Build**

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

Obwohl dieser Schritt nicht erforderlich ist, um eine Projektion zu generieren, bietet er Einfachheit, indem Builddateien aus beiden Projekten im gleichen Verzeichnis generiert und die Buildbereinigung vereinfacht werden. Beachten Sie Folgendes: Wenn Sie nicht aus der Quelle heraus erstellen, werden sowohl **simplemathcomponent. winmd** als auch die Interop-Assembly **SimpleMathComponent.dll** in verschiedenen Verzeichnissen in den jeweiligen Projekt Ordnern generiert. Auf diese Dateien wird in **simplemathprojection. nuspec** unten verwiesen, sodass die Pfade entsprechend geändert werden müssen.

## <a name="create-a-nuget-package-from-the-projection"></a>Erstellen eines nuget-Pakets aus der Projektion

Zum Verteilen und Verwenden der Interop-Assembly können Sie beim Erstellen der Lösung automatisch ein nuget-Paket erstellen, indem Sie einige zusätzliche Projekteigenschaften hinzufügen. Dieses Paket enthält die Interop-Assembly und eine Abhängigkeit vom c#/WinRT nuget-Paket für die erforderliche c#/WinRT-Laufzeitassembly. Diese Laufzeitassembly wird **winrt.runtime.dll** für .net 5,0-Ziele benannt.

1. Fügen Sie eine nuget-Spezifikation (. nuspec) zum **simplemathprojection** -Projekt hinzu.

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten **simplemathprojection** , wählen Sie neuen Ordner **Hinzufügen**aus  ->  **New Folder**, und geben Sie dem Ordner den Namen **nuget**. 
    2. Klicken Sie mit der rechten Maustaste auf den **nuget** -Ordner, wählen Sie **Add**  ->  **Neues Element**hinzufügen, wählen Sie die XML-Datei aus, und nennen Sie Sie **simplemathprojection. nuspec** 

2. Fügen Sie " **simplemathprojection. csproj** " Folgendes hinzu, um das Paket automatisch zu generieren. Diese Eigenschaften geben das `NuspecFile` -Verzeichnis und das-Verzeichnis zum Generieren des nuget-Pakets an.

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example of a C++/WinRT component NuGet spec. Notice the `dependency` on CsWinRT for the `net5.0` target framework moniker, as well as the target for `lib\net5.0\SimpleMathProjection.dll`, which points to the projection assembly **SimpleMathComponent.dll** instead of **SimpleMathComponent.winmd**. This behavior is new in .NET 5.0 and enabled by C#/WinRT.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="0.8.0" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support net46+, netcore3, net5, uap, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>Erstellen der Projekt Mappe, um die Projektion und das nuget-Paket zu generieren

Nun können Sie die Projekt Mappe erstellen: Klicken Sie mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie Projekt Mappe **Erstellen**aus. Dadurch wird zuerst das Komponenten Projekt und dann das Projektions Projekt erstellt. Die Interop **. cs** -Dateien und die Assembly werden zusätzlich zu den Metadatendateien aus dem Komponenten Projekt im Ausgabeverzeichnis generiert. Sie können auch das generierte NuGet-Paket **simplemathcomponent 0.1.0-Prerelease. nupkg** im **NuGet** -Ordner sehen.

![Projektmappen-Explorer, der die Projektions Generierung anzeigt](images/projection-generated-files.png)

## <a name="referencethenugetpackage-inacnet50consoleapplication"></a>Verweisen auf das nuget-Paket in einer c# .net 5,0-Konsolenanwendung

Um die projizierte **simplemathcomponent**zu nutzen, können Sie einfach einen Verweis auf das neu erstellte nuget-Paket in Ihrer Anwendung hinzufügen. Die folgenden Schritte veranschaulichen die Vorgehensweise, indem Sie eine einfache Konsolen-app in einer separaten Projekt Mappe erstellen.

1. Erstellen Sie eine neue Projekt Mappe mit einem Konsolen-App-Projekt **(.net Core)** .

    1. Klicken Sie in Visual Studio auf **Datei** -> **Neu** -> **Projekt**.
    2. Suchen **Sie im Dialogfeld Neues Projekt hinzufügen**nach der Projektvorlage **Konsolen-app (.net Core)** . Wählen Sie die Vorlage aus, und klicken Sie auf **weiter**.
    3. Nennen Sie das neue Projekt **sampleconsoleapp** , und klicken Sie auf **Erstellen**. Wenn Sie dieses Projekt in einer neuen Projekt Mappe erstellen, können Sie das nuget-Paket " **simplemathcomponent** " separat wiederherstellen.

2. Doppelklicken Sie in **Projektmappen-Explorer**auf den Knoten **sampleconsoleapp** , um die Projektdatei **sampleconsoleapp. csproj** zu öffnen, und aktualisieren Sie den zielframeworkmoniker und die Platt Form Konfiguration, wie im folgenden Beispiel gezeigt.

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Fügen Sie das nuget-Paket **simplemathcomponent** zum Projekt **sampleconsoleapp** hinzu. Sie benötigen auch das nuget-Paket [Microsoft. vcrtforwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) , das in apps erforderlich ist, die nicht in einem msix-Paket verpackt sind. Wenn Sie das NuGet- **Element simplemathcomponent** bei der Erstellung des Projekts wiederherstellen möchten, können Sie die- `RestoreSources` Eigenschaft mit dem Pfad zum **NuGet** -Ordner in der Komponenten Lösung verwenden.

    ```xml
    <PropertyGroup>
      <RestoreSources>
          https://api.nuget.org/v3/index.json;
          ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
        <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    Beachten Sie, dass in dieser exemplarischen Vorgehensweise für den nuget-Wiederherstellungs Pfad für **simplemathcomponent** angenommen wird, dass sich beide Projektmappendateien im gleichen Verzeichnis befinden. Alternativ können Sie Ihrer Projekt Mappe [einen lokalen nuget-paketfeed hinzufügen](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) .

4. Bearbeiten Sie die Datei **Program.cs** , um die von **simplemathcomponent**bereitgestellte Funktionalität zu verwenden.

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. Erstellen Sie die Konsolen-APP und führen Sie Sie aus. Die Ausgabe sollte unten angezeigt werden.

    ![Konsolen NET5 Ausgabe](images/console-output.png)

## <a name="resources"></a>Ressourcen

- [Vollständiges Codebeispiel für diese exemplarische Vorgehensweise](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)
