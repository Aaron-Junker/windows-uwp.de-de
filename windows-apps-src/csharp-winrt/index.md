---
description: C#/WinRT ist ein Toolset, das WinRT-Projektionsunterstützung für C#-Code bereitstellt.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C#, WinRT, CSWinRT, Projektion
ms.localizationpriority: medium
ms.openlocfilehash: e52763d78937405b308c4c4fe06f6d231fa3abcf
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580277"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT befindet sich in der öffentlichen Vorschauphase und unterliegt bis zur endgültigen Version möglicherweise wesentlichen Änderungen. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.

Bei C#/WinRT handelt es sich um ein Toolkit als NuGet-Paket, das die Projektionsunterstützung in der Windows-Runtime (WinRT) für die C#-Sprache bereitstellt. Eine *Projektion* ist eine Übersetzungsschicht, z. B. eine Interop-Assembly, die die Programmierung von WinRT-APIs auf für die Zielsprache natürliche und vertraute Weise ermöglicht. Beispielsweise verbirgt die C#/WinRT-Projektion die Details der Interop zwischen C#- und WinRT-Schnittstellen und stellt Zuordnungen vieler WinRT-Typen zu geeigneten .NET-Entsprechungen wie Zeichenfolgen, URIs, allgemeinen Werttypen und generischen Sammlungen bereit.

C#/WinRT bietet derzeit Unterstützung für die Verarbeitung von WinRT-Typen, und die aktuelle Vorschau ermöglicht es Ihnen, WinRT-Interop-Assemblys zu [erstellen](#create-an-interop-assembly) und auf diese zu [verweisen](#reference-an-interop-assembly). In zukünftigen Versionen von C#/WinRT folgt die Unterstützung für die Erstellung von WinRT-Typen in C#.

## <a name="motivation-for-cwinrt"></a>Motivation für C#/WinRT

[.NET Core](https://docs.microsoft.com/dotnet/core/) stellt den Schwerpunkt der .NET-Plattform dar, und .NET 5 ist die nächste Hauptversion. Dabei handelt es sich um eine plattformübergreifende Open-Source-Laufzeit, mit der Geräte-, Cloud- und IoT-Anwendungen erstellt werden können.

In frühere Versionen von .NET Framework und .NET Core waren Kenntnisse über WinRT integriert, eine Windows-spezifische Technologie. Zur Unterstützung der Portabilitäts- und Effizienzziele von .NET 5 haben wir die Unterstützung für die WinRT-Projektion aus dem .NET-Compiler und der .NET-Laufzeit in das C#/WinRT-Toolkit verschoben. C#/WinRT soll für Parität mit der integrierten WinRT-Unterstützung sorgen, die in früheren Versionen des C#-Compilers und der .NET-Laufzeit bereitgestellt wird. Einzelheiten hierzu finden Sie unter [.NET-Zuordnungen von Windows-Runtime-Typen](https://docs.microsoft.com/windows/uwp/winrt-components/net-framework-mappings-of-windows-runtime-types).

C#/WinRT unterstützt auch WinUI 3.0. Mit dieser Version von WinUI werden systemeigene Steuerelemente und Features der Microsoft-Benutzeroberfläche aus dem Betriebssystem herausgelöst. Dadurch können App-Entwickler die neuesten Steuerelemente und Visuals unter Windows 10, Version 1803 und höheren Versionen verwenden.

Schließlich ist C#/WinRT ein allgemeines Toolkit und auf weitere Szenarien ausgelegt, in denen die integrierte Unterstützung für WinRT im C#-Compiler oder in der .NET-Runtime nicht verfügbar ist. C#/WinRT unterstützt Versionen der .NET-Runtime, die mit .NET Standard 2.0 kompatibel sind, z. B. Mono 5.4.

Weitere Informationen zu C#/WinRT finden Sie im [C#/WinRT-GitHub-Repository](https://aka.ms/cswinrt/repo)

## <a name="create-an-interop-assembly"></a>Erstellen einer Interop-Assembly

WinRT-APIs werden in Windows-Metadatendateien (*.winmd) definiert. Das C#/WinRT NuGet-Paket enthält den C#/WinRT-Compiler, **cswinrt**, mit dem Sie Windows-Metadatendateien verarbeiten und .NET Standard 2.0-C#-Code erstellen können. Sie können diese Quelldateien in Interop-Assemblys kompilieren, ähnlich wie [C++/WinRT](../cpp-and-winrt-apis/index.md) Header für die C++-Sprachprojektion generiert. Anschließend können Sie die C#/WinRT-Interop-Assemblys C# für Anwendungen als Verweis verteilen, zusammen mit der C#/WinRT-Laufzeit-Assembly.

### <a name="invoke-cswinrtexe"></a>Aufrufen von „cswinrt.exe“

Führen Sie zum Anzeigen von Befehlszeilenoptionen `cswinrt -?` aus. Wir empfehlen, „cswinrt.exe“ mithilfe einer Directory.Build.Targets-Datei aus einem Projekt aufzurufen. Das folgende Projektfragment veranschaulicht einen einfachen Aufruf von **cswinrt**, um Projektionsquellen für Typen im Contoso-Namespace zu generieren. Diese Quellen werden dann in den Projektbuild eingeschlossen.

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>Verteilen der Interop-Assembly

Eine Interop-Assembly wird in der Regel als NuGet-Paket verteilt, zusammen mit einer Abhängigkeit vom C#/WinRT-NuGet-Paket für die erforderliche C#/WinRT-Laufzeit-Assembly (**WinRT. Runtime.dll**). Es gibt zwei Versionen der C#/WinRT-Laufzeit-Assembly, eine zielt auf .NET Standard 2.0 und die andere auf .NET 5.0 ab. Je nach Zielframework einer Anwendung wird nur eine dieser Versionen bereitgestellt. 

* Das Ziel .NET Standard 2.0 eignet sich für plattformübergreifende Downlevel-Anwendungen, mit denen einfache Funktionen unter Windows bereitgestellt werden sollen.
* Das Ziel .NET 5.0 wird für moderne Windows-Apps empfohlen, die eine ordnungsgemäße Garbage Collection über systemeigene Objektverweise hinweg erfordern, z. B. XAML-Apps.

Eine Interop-Assembly kann sicherstellen, dass die richtige Version der C#/WinRT-Laufzeit für eine App bereitgestellt wird, indem eine `targetFramework`-Bedingung in der nuspec-Datei eingeschlossen wird.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> Der Zielframeworkmoniker für .NET 5.0 wird von „.NETCoreApp5.0“ in „.NET5.0“ verschoben. Von C#/WinRT-Vorabversionen wird entweder die eine oder die andere verwendet.

## <a name="reference-an-interop-assembly"></a>Verweisen auf eine Interop-Assembly

In der Regel wird von Anwendungsprojekten auf C#/WinRT-Interop-Assemblys verwiesen. Es ist aber auch ein Verweis durch temporäre Interop-Assemblys möglich. So kann beispielsweise die WinUI-Interop-Assembly auf die Windows SDK-Interop-Assembly verweisen.

Um projizierte C#/WinRT-Typen zu verarbeiten, fügen Sie Ihrem Projekt einen Verweis auf das entsprechende C#/WinRT-Interop-NuGet-Paket hinzu. Dies bewirkt, dass dem Projekt sowohl die Interop-Assembly als auch die C# /WinRT-Laufzeit-Assembly hinzugefügt wird.

Wenn Sie eine WinRT-Komponente eines Drittanbieters ohne eine offizielle Interop-Assembly verteilen, kann ein Anwendungsprojekt das Verfahren zum [Erstellen einer Interop-Assembly](#create-an-interop-assembly) befolgen, um eigene private Projektionsquellen zu generieren. Eine solche Vorgehensweise wird nicht empfohlen, da Sie in einem Prozess widersprüchliche Projektionen desselben Typs verursachen kann. Dies wird durch das Erstellen von NuGet-Paketen nach dem Schema der [semantischen Versionierung](https://semver.org) verhindert. Eine offizielle Interop-Assembly von Drittanbietern ist zu bevorzugen.

### <a name="winrt-type-activation"></a>Aktivierung von WinRT-Typen

C#/WinRT unterstützt die Aktivierung von WinRT-Typen, die vom Betriebssystem gehostet werden, sowie von Drittanbieter-Komponenten wie z. B. [Win2D](https://www.nuget.org/packages/Win2D.uwp/). Die Unterstützung der Aktivierung von Drittanbieter-Komponenten in einer Desktopanwendung wird durch die [registrierungsfreie WinRT-Aktivierung](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/) ermöglicht, die ab Windows 10, Version 1903 verfügbar ist. Dies erfordert u. U. auch die Verwendung des Pakets [VCRT Forwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/), wenn die Komponente UWP-Apps als Ziel haben soll.

C#/WinRT bietet auch einen Aktivierungs-Fallbackpfad, wenn Windows den Typ nicht wie oben beschrieben aktivieren kann. In diesem Fall versucht C#/WinRT, eine systemeigene Implementierungs-DLL anhand des vollqualifizierten Typnamens zu suchen, wobei Elemente progressiv entfernt werden. Die Fallbacklogik würde beispielsweise versuchen, den Typ „Contoso.Controls.Widget“ nacheinander aus den folgenden Modulen zu aktivieren:

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT verwendet die [alternative LoadLibrary-Suchreihenfolge](https://docs.microsoft.com/windows/win32/dlls/dynamic-link-library-search-order?#alternate-search-order-for-desktop-applications), um eine Implementierungs-DLL aufzufinden. Eine App, die sich auf ein derartiges Fallbackverhalten stützt, muss die Implementierungs-DLL neben dem App-Modul verpacken.

## <a name="known-issues"></a>Bekannte Probleme

Es gibt einige bekannte Interop-Leistungsprobleme in der aktuellen Vorschauversion von C# /WinRT. Diese werden vor der endgültigen Version von Ende 2020 behoben.

Sollten Sie funktionale Probleme mit dem C#/WinRT NuGet-Paket, dem Compiler „cswinrt.exe“ oder den generierten Projektionsquellen feststellen, teilen Sie uns die Probleme über die [Seite für C#/WinRT-Probleme](https://github.com/microsoft/CsWinRT/issues) mit.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [C#/WinRT-GitHub-Repository](https://aka.ms/cswinrt/repo)
