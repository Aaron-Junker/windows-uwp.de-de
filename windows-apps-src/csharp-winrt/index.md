---
description: C#/WinRT ist ein Toolset, das WinRT-Projektionsunterstützung für C#-Code bereitstellt.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C#, WinRT, CSWinRT, Projektion
ms.localizationpriority: medium
ms.openlocfilehash: 55fbc91bb67b0853eafebdf05ffcf116637233bf
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804774"
---
# <a name="cwinrt"></a>C#/WinRT

Bei C#/WinRT handelt es sich um ein Toolkit als NuGet-Paket, das die Projektionsunterstützung in der Windows-Runtime (WinRT) für die C#-Sprache bereitstellt. Eine *Projektion* bezieht sich auf eine Interop-Assembly, die die Programmierung von WinRT-APIs auf für die Zielsprache natürliche und vertraute Weise ermöglicht. Die C#/WinRT-Projektion verbirgt die Details der Interop zwischen C#- und WinRT-Schnittstellen und stellt Zuordnungen vieler [WinRT-Typen zu geeigneten .NET-Entsprechungen](../winrt-components/net-framework-mappings-of-windows-runtime-types.md) wie Zeichenfolgen, URIs, allgemeinen Werttypen und generischen Sammlungen bereit.

C#/WinRT bietet derzeit Unterstützung für die Verarbeitung von WinRT-APIs durch die Verwendung von [Zielframeworkmonikers](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option) in .NET 5+. Die Angabe des Zielframeworkmonikers mit einer bestimmten Windows-Version fügt Verweise auf die Windows SDK-Projektion und Laufzeitassemblys hinzu, die von C#/WinRT generiert wurden.

Mit dem neuesten [C#/WinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/) können Sie Ihre eigenen WinRT-Interop-Assemblys für .NET 5+-Consumer [erstellen](#create-an-interop-assembly) und darauf [verweisen](#reference-an-interop-assembly). Die neueste C#/WinRT-Version enthält auch eine [Vorschau auf die Erstellung](create-windows-runtime-component-cswinrt.md) von WinRT-Typen in C#.

Weitere Informationen zu C#/WinRT finden Sie im [C#/WinRT-GitHub-Repository](https://aka.ms/cswinrt/repo).

## <a name="motivation-for-cwinrt"></a>Motivation für C#/WinRT

[.NET Core](/dotnet/core/) stellt den Schwerpunkt der .NET-Plattform dar, und .NET 5 ist die neueste Hauptversion. Dabei handelt es sich um eine plattformübergreifende Open-Source-Laufzeit, mit der Geräte-, Cloud- und IoT-Anwendungen erstellt werden können.

In frühere Versionen von .NET Framework und .NET Core waren Kenntnisse über WinRT integriert, eine Windows-spezifische Technologie. Zur Unterstützung der Portabilitäts- und Effizienzziele von .NET 5 haben wir die Unterstützung für die WinRT-Projektion aus dem .NET-Compiler und der .NET-Laufzeit in das C#/WinRT-Toolkit verschoben. C#/WinRT soll für Parität mit der integrierten WinRT-Unterstützung sorgen, die in früheren Versionen des C#-Compilers und der .NET-Laufzeit bereitgestellt wird. Einzelheiten hierzu finden Sie unter [.NET-Zuordnungen von Windows-Runtime-Typen](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

C#/WinRT unterstützt auch WinUI 3.0. Mit dieser Version von WinUI werden systemeigene Steuerelemente und Features der Microsoft-Benutzeroberfläche aus dem Betriebssystem herausgelöst. Dadurch können App-Entwickler die neuesten Steuerelemente und Visuals unter Windows 10, Version 1803 und höheren Versionen verwenden.

Schließlich ist C#/WinRT ein allgemeines Toolkit und auf weitere Szenarien ausgelegt, in denen die integrierte Unterstützung für WinRT im C#-Compiler oder in der .NET-Runtime nicht verfügbar ist.

## <a name="create-an-interop-assembly"></a>Erstellen einer Interop-Assembly

WinRT-APIs werden in Windows-Metadatendateien (\*.winmd) definiert. Das C#/WinRT NuGet-Paket ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) enthält den C#/WinRT-Compiler, **cswinrt.exe**, mit dem Sie Windows-Metadatendateien verarbeiten und .NET 5-C#-Code erstellen können. Sie können diese Quelldateien in Interop-Assemblys kompilieren, ähnlich wie [C++/WinRT](../cpp-and-winrt-apis/index.md) Header für die C++-Sprachprojektion generiert. Anschließend können Sie die C#/WinRT-Interop-Assemblys für .NET 5-Anwendungen verteilen, auf die verwiesen werden soll, zusammen mit der C#/WinRT-Laufzeit-Assembly.

Eine exemplarische Vorgehensweise, die veranschaulicht, wie eine Interop-Assembly als NuGet-Paket erstellt und verteilt wird, finden Sie unter [Exemplarische Vorgehensweise: Generieren einer .NET 5-Projektion aus einer C++/WinRT-Komponente und Aktualisieren von NuGet](net-projection-from-cppwinrt-component.md).

### <a name="invoke-cswinrtexe"></a>Aufrufen von „cswinrt.exe“

Um „cswinrt.exe“ von einem Projekt aus aufzurufen, installieren Sie das neueste [C#/WinRT-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/). Sie können dann C#/WinRT-spezifische Projekteigenschaften in einem **C#-Klassenbibliothek (.NET Core)** -Projekt festlegen, um eine Interop-Assembly zu generieren. Das folgende Projektfragment veranschaulicht einen einfachen Aufruf von **cswinrt**, um Projektionsquellen für Typen im Contoso-Namespace zu generieren. Diese Quellen werden dann in den Projektbuild eingeschlossen.

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

In diesem Projekt müssten Sie auch auf das C#/WinRT-NuGet-Paket und die projektspezifischen WINMD-Dateien verweisen, die Sie projizieren möchten, sei es über ein NuGet-Paket, einen Projektverweis oder einen direkten Dateiverweis. Standardmäßig werden die **Windows**- und **Microsoft**-Namespaces nicht projiziert, da C#/WinRT diese Projektionen durch die Unterstützung des Zielframeworkmonikers und WinUI 3 bereits erzeugt hat. Eine vollständige Liste der C#/WinRT-NuGet-Projekteigenschaften finden Sie in der [CsWinRT-NuGet-Dokumentation](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md).

### <a name="distribute-the-interop-assembly"></a>Verteilen der Interop-Assembly

Eine Interop-Assembly wird in der Regel als NuGet-Paket verteilt, zusammen mit einer Abhängigkeit vom C#/WinRT-NuGet-Paket für die erforderliche C#/WinRT-Laufzeit-Assembly (**WinRT.Runtime.dll**).

Um sicherzustellen, dass die korrekte Version der C#/WinRT-Laufzeit für .NET 5-Anwendungen bereitgestellt wird, fügen Sie eine `targetFramework`-Bedingung in die NUSPEC-Datei mit einer Abhängigkeit vom C#/WinRT-NuGet-Paket ein.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.1.4" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> Der Zielframeworkmoniker für .NET 5.0 wird von „.NETCoreApp5.0“ in „net5.0“ verschoben.

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

C#/WinRT verwendet die [alternative LoadLibrary-Suchreihenfolge](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications), um eine Implementierungs-DLL aufzufinden. Eine App, die sich auf ein derartiges Fallbackverhalten stützt, muss die Implementierungs-DLL neben dem App-Modul verpacken.

## <a name="common-errors-and-troubleshooting"></a>Häufige Fehler und Problembehandlung

- Fehler: „Es wurden keine Windows-Metadaten bereitgestellt oder erkannt.“

  Sie können Windows-Metadaten z. B. über die Projekteigenschaft `<CsWinRTWindowsMetadata>` angeben:
  ```xml
  <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
  ```
  
- Fehler CS0246: Der Typ- oder Namespacename „Windows“ konnte nicht gefunden werden. (Fehlt eine Using-Anweisung oder ein Assemblyverweis?)

  Um diesen Fehler zu beheben, bearbeiten Sie Ihre `<TargetFramework>`-Eigenschaft, um z. B. eine bestimmte Windows-Version anzusteuern:
  ```xml
  <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
  ```
  Weitere Details zum Festlegen der `<TargetFramework>`-Eigenschaft finden Sie in der Dokumentation zum [Aufrufen von Windows-Runtime-APIs](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).


### <a name="net-sdk-versioning-errors"></a>.NET SDK-Versionsverwaltungsfehler

Möglicherweise treten in einem Projekt, das mit einer früheren .NET SDK-Version als alle Abhängigkeiten erstellt wurde, die folgenden Fehler oder Warnungen auf.

| Fehler- oder Warnmeldung | `Reason` |
|--------------------------|--------|
| Warnung MSB3277: Es wurden Konflikte zwischen verschiedenen Versionen von WinRT.Runtime oder Microsoft.Windows.SDK.NET gefunden, die nicht aufgelöst werden konnten. | Diese Buildwarnung tritt auf, wenn auf eine Bibliothek verwiesen wird, die Windows SDK-Typen auf der API-Oberfläche verfügbar macht. |
| [Fehler CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705): Die Assembly „AssemblyName1“ verwendet „TypeName“ mit einer höheren Version als die referenzierte Assembly „AssemblyName2“. | Dieser Buildcompilerfehler tritt auf, wenn auf verfügbar gemachte Windows SDK-Typen in einer Bibliothek verwiesen wird und diese verarbeitet werden. |
| System.IO.FileLoadException | Dieser Laufzeitfehler kann beim Aufrufen bestimmter APIs in einer Bibliothek auftreten, die keine Windows SDK-Typen verfügbar macht. |

Aktualisieren Sie das .NET SDK auf die neueste Version, um diese Fehler zu beheben. Dadurch wird sichergestellt, dass die von Ihrer Anwendung verwendeten Laufzeitversionen und Windows SDK-Assemblyversionen mit allen Abhängigkeiten kompatibel sind. Diese Fehler treten möglicherweise bei frühen Wartungs-/Featureaktualisierungen für das .NET 5 SDK auf, da bei Laufzeitkorrekturen möglicherweise Aktualisierungen der Assemblyversionen erforderlich sind.

## <a name="known-issues"></a>Bekannte Probleme

Bekannte Probleme und bedeutende Änderungen sind im [C#/WinRT-GitHub-Repository](https://aka.ms/cswinrt/repo) aufgeführt.

Sollten Sie funktionale Probleme mit dem C#/WinRT NuGet-Paket, dem Compiler „cswinrt.exe“ oder den generierten Projektionsquellen feststellen, teilen Sie uns die Probleme über die [Seite für C#/WinRT-Probleme](https://github.com/microsoft/CsWinRT/issues) mit.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [C#/WinRT-GitHub-Repository](https://aka.ms/cswinrt/repo)
