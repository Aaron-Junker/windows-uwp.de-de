---
description: Dieses Thema führt Sie durch den Prozess des Hinzufügens einer einfachen C#-Komponente zu einer C++/WinRT-App
title: Erstellen einer C#-Komponente für Windows-Runtime zur Verwendung aus einer C++/WinRT-App
ms.date: 12/30/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, C#
ms.localizationpriority: medium
ms.openlocfilehash: 0b2b4f67d67fc1a23e650d1fbe5d58964add1a70
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804754"
---
# <a name="authoring-a-c-windows-runtime-component-for-use-from-a-cwinrt-app"></a>Erstellen einer C#-Komponente für Windows-Runtime zur Verwendung aus einer C++/WinRT-App

Dieses Thema führt Sie durch den Prozess des Hinzufügens einer einfachen C#-Komponente zu Ihrem C++/WinRT-Projekt.

Visual Studio macht es einfach, eigene benutzerdefinierte Windows-Runtime-Typen innerhalb eines mit C# oder Visual Basic geschriebenen WRC-Projekts (Windows Runtime Component, Komponente für Windows-Runtime) zu erstellen und bereitzustellen und dann auf diese WRC von einem C++-Anwendungsprojekt aus zu verweisen und die benutzerdefinierten Typen von dieser Anwendung aus zu verwenden.

Intern können Ihre Windows-Runtime-Typen jede in einer UWP-Anwendung zulässige .NET-Funktion verwenden.

> [!NOTE]
> Weitere Informationen finden Sie unter [Komponenten für Windows-Runtime in C# und Visual Basic](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) und [Überblick über .NET für UWP-Apps](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true).

Extern können die Typelemente für ihre Parameter und Rückgabewerte nur Windows-Runtime-Typen verfügbar machen. Wenn Sie die Projektmappe erstellen, erstellt Visual Studio das .NET WRC-Projekt und führt dann einen Schritt zur Erstellung einer Datei mit Windows-Metadaten (.winmd) durch. Hierbei handelt es sich um Ihre Komponente für Windows-Runtime (WRC), die Visual Studio in Ihre App einschließt.

> [!NOTE]
> .NET ordnet einige häufig verwendete .NET-Typen, z. B. primitive Datentypen und Sammlungstypen, automatisch ihren Windows-Runtime-Entsprechungen zu. Diese .NET-Typen können in der öffentlichen Schnittstelle einer Komponente für Windows-Runtime verwendet werden und werden Benutzern der Komponente als die entsprechenden Windows-Runtime-Typen angezeigt. Siehe [Komponenten für Windows-Runtime in C# und Visual Basic](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="prerequisites"></a>Voraussetzungen:

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-blank-app"></a>Erstellen einer leeren App

Erstelle in Visual Studio ein neues Projekt anhand der Projektvorlage **Leere App (C++/WinRT)** . Stelle sicher, dass du die Vorlage **(C++/WinRT)** und nicht die Vorlage **(Universelles Windows)** verwendest.

Legen Sie den Namen des neuen Projekts auf *CppToCSharpWinRT* fest, damit Ihre Ordnerstruktur mit der exemplarischen Vorgehensweise übereinstimmt.

## <a name="add-a-c-windows-runtime-component-to-the-solution"></a>Hinzufügen einer C#-Komponente für Windows-Runtime zur Projektmappe

Erstellen Sie das Komponentenprojekt in Visual Studio: Öffnen Sie im Projektmappen-Explorer das Kontextmenü für die Projektmappe *CppToCSharpWinRT*, und wählen Sie **Hinzufügen** und dann **Neues Projekt** aus, um ein neues C#-Projekt zur Projektmappe hinzuzufügen. Wählen Sie im Abschnitt **Installierte Vorlagen** des Dialogfelds **Neues Projekt hinzufügen** die Option **Visual C#** und dann **Windows** sowie **Universell** aus. Wählen Sie die Vorlage **Komponente für Windows-Runtime (Universelles Windows)** aus, und geben Sie **SampleComponent** als Projektnamen ein. 

> [!NOTE]
> Wählen Sie im Dialogfeld **Neues Projekt für die universelle Windows-Plattform** als Mindestversion **Windows 10 Creators Update (10.0, Build 15063)** aus. Weitere Informationen finden Sie im Abschnitt [Mindestversion der Anwendung](#application-minimum-version) weiter unten.

## <a name="add-the-c-getmystring-method"></a>Hinzufügen der C#-Methode „GetMyString“

Ändern Sie im Projekt *SampleComponent* den Namen der Klasse von *Class1* in *Example*. Fügen Sie der Klasse dann zwei einfache Elemente hinzu, ein privates `int`-Feld und eine Instanzmethode namens *GetMyString*:

> ```csharp
>     public sealed class Example
>     {
>         int MyNumber;
> 
>         public string GetMyString()
>         {
>             return $"This is call #: {++MyNumber}";
>         }
>     }
> ```

> [!NOTE]
> Standardmäßig ist die Klasse als **öffentlich versiegeln** gekennzeichnet. Die Windows-Runtime-Klassen, die Sie von Ihrer Komponente aus bereitstellen, müssen **versiegelt** sein.

> [!NOTE]
> Optional: Um IntelliSense für die neu hinzugefügte Member zu aktivieren, öffnen Sie im Projektmappen-Explorer das Kontextmenü für das Projekt *SampleComponent*, und wählen Sie dann **Erstellen** aus.

## <a name="reference-the-c-samplecomponent-from-the-cpptocsharpwinrt-project"></a>Verweisen auf die C#-Beispielkomponente (SampleComponent) aus dem Projekt „CppToCSharpWinRT“ heraus

Öffnen Sie im Projektmappen-Explorer im C++/WinRT-Projekt das Kontextmenü für **Verweise**, und wählen Sie dann **Verweis hinzufügen** aus, um das Dialogfeld **Verweis hinzufügen** zu öffnen. Wählen Sie **Projekte** und dann **Projektmappe** aus. Aktivieren Sie das Kontrollkästchen für das Projekt *SampleComponent*, und wählen Sie **OK** aus, um einen Verweis hinzuzufügen.

> [!NOTE]
> Optional: Um IntelliSense für das C++/WinRT-Projekt zu aktivieren, öffnen Sie im Projektmappen-Explorer das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie dann **Erstellen** aus.

## <a name="edit-mainpageh"></a>Bearbeiten von „MainPage.h“

Öffnen Sie `MainPage.h` im Projekt *CppToCSharpWinRT*, und fügen Sie dann zwei Elemente hinzu.  Fügen Sie zuerst `#include "winrt/SampleComponent.h"` am Ende der `#include`-Anweisungen hinzu und dann ein `winrt::SampleComponent::Example`-Feld an die `MainPage`-Struktur.

```cppwinrt
// MainPage.h
...
#include "winrt/SampleComponent.h"

namespace winrt::CppToCSharpWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        winrt::SampleComponent::Example myExample;
...
    };
}
```

> [!NOTE]
> In Visual Studio ist `MainPage.h` unter `MainPage.xaml` aufgeführt.

## <a name="edit-mainpagecpp"></a>Bearbeiten von „MainPage.cpp“

Ändern Sie in `MainPage.cpp` die `Mainpage::ClickHandler`-Implementierung, um die C#-Methode `GetMyString` aufzurufen.

```cppwinrt
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    //myButton().Content(box_value(L"Clicked"));

    hstring myString = myExample.GetMyString();

    myButton().Content(box_value(myString));
}
```
## <a name="run-the-project"></a>Ausführen des Projekts

Jetzt kannst du das Projekt kompilieren und ausführen. Jedes Mal, wenn Sie auf die Schaltfläche klicken, wird die Zahl in der Schaltfläche erhöht.

![Screenshots eines C++/WinRT Windows-Aufrufs einer C#-Komponente](images/csharp-cpp-sample.png)

> [!TIP]
> Erstellen Sie in Visual Studio das Komponentenprojekt: Öffnen Sie im Projektmappen-Explorer das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie **Eigenschaften** aus. Anschließend wählen Sie unter **Konfigurationseigenschaften** die Option **Debuggen** aus. Stellen Sie den Debuggertyp auf *Verwaltet und native* ein, wenn Sie sowohl den C#- (verwaltet) als auch den C++-Code (nativ) debuggen möchten.
> ![C++-Debugeigenschaften](images/cpp-debugging-properties.png)

## <a name="application-minimum-version"></a>Mindestversion der Anwendung

Die [**Mindestversion der Anwendung**](../updates-and-versions/choose-a-uwp-version.md) für die C#-Projektversion steuert die zum Kompilieren der Anwendung verwendete Version von .NET. Wenn Sie z. B. **Windows 10 Fall Creators Update (10.0, Build 16299)** , oder höher, auswählen, wird Unterstützung für .NET Standard 2.0- und Windows ARM64-Prozessoren aktiviert. 

> [!TIP]
> Wenn keine Unterstützung für .NET Standard 2.0 oder ARM64 erforderlich ist, wird empfohlen, als **Mindestversion der Anwendung** Versionen unter 16299 zu verwenden, um eine zusätzliche Buildkonfiguration zu vermeiden.

## <a name="configure-for-windows-10-fall-creators-update-100-build-16299"></a>Konfiguration für Windows 10 Fall Creators Update (10.0, Build 16299)

Führen Sie die folgenden Schritte aus, um die Unterstützung für .NET Standard 2.0 oder Windows ARM64 in den C#-Projekten zu aktivieren, auf die von Ihrem C++/WinRT-Projekt aus verwiesen wird. 

Wechseln Sie in Visual Studio in den Projektmappen-Explorer, und öffnen Sie das Kontextmenü für das Projekt *CppToCSharpWinRT*.  Wählen Sie **Eigenschaften** aus, und legen Sie Mindestversion der universellen Windows-App auf **Windows 10 Fall Creators Update (10.0, Build 16299)** , oder höher, fest. Führen Sie dieselben Schritte für das Projekt *SampleComponent* aus.

Öffnen Sie in Visual Studio das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie **Projekt entladen** aus, um `CppToCSharpWinRT.vcxproj` im Text-Editor zu öffnen. 

Kopieren Sie den folgenden XML-Code, und fügen Sie ihn in die erste `PropertyGroup` in `CPPWinRTCSharpV2.vcxproj` ein. 

```xml
   <!-- Start Custom .NET Native properties -->
   <DotNetNativeVersion>2.2.9-rel-29512-01</DotNetNativeVersion>
   <DotNetNativeSharedLibary>2.2.8-rel-29512-01</DotNetNativeSharedLibary>
   <UWPCoreRuntimeSdkVersion>2.2.11</UWPCoreRuntimeSdkVersion>
   <!--<NugetPath>$(USERPROFILE)\.nuget\packages</NugetPath>-->
   <NugetPath>$(ProgramFiles)\Microsoft SDKs\UWPNuGetPackages</NugetPath>
   <!-- End Custom .NET Native properties -->
```

Die Werte für `DotNetNativeVersion`, `DotNetNativeSharedLibary` und `UWPCoreRuntimeSdkVersion` können je nach Version von Visual Studio variieren.  Um die richtigen Werte festzulegen, öffnen Sie `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages`, und sehen Sie sich das Unterverzeichnis für jeden Wert in der unten stehenden Tabelle an.  Zum Beispiel enthält das Verzeichnis `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` ein Unterverzeichnis mit dem Namen `2.2.9-rel-29512-01`.

> | MSBuild-Variable | Verzeichnis | Beispiel
> |-| - | -
> | DotNetNativeVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` | `2.2.9-rel-29512-01`
> | DotNetNativeSharedLibary | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.SharedLibrary` | `2.2.8-rel-29512-01`
> | UWPCoreRuntimeSdkVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.UWPCoreRuntimeSdk` | `2.2.11`

Fügen Sie dann unmittelbar nach dem ersten `PropertyGroup` den folgenden (unveränderten) Text ein.

```xml
  <!-- Start Custom .NET Native targets -->
  <!-- Import all of the .NET Native / CoreCLR props at the beginning of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.props" />
  <!-- End Custom .NET Native targets -->
```

Fügen Sie am Ende der Projektdatei direkt vor dem schließenden `Project`-Tag Folgendes (unverändert) hinzu.

```xml
  <!-- Import all of the .NET Native / CoreCLR targets at the end of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.targets" />
  <!-- End Custom .NET Native targets -->
```

Laden Sie die Projektdatei in Visual Studio erneut. Öffnen Sie dazu in Visual Studio im Projektmappen-Explorer das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie **Projekt neu laden** aus.

## <a name="building-for-net-native"></a>Erstellung für .NET Native

Es wird empfohlen, Ihre Anwendung mit der über .NET Native erstellten C#-Komponente zu erstellen und zu testen. Öffnen Sie in Visual Studio das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie **Projekt entladen** aus, um `CppToCSharpWinRT.vcxproj` im Text-Editor zu öffnen. 

Legen Sie als nächstes die `UseDotNetNativeToolchain`-Eigenschaft in den Release- und ARM64-Konfigurationen in der C++-Projektdatei auf `true` fest.

Öffnen Sie in Visual Studio im Projektmappen-Explorer das Kontextmenü für das Projekt *CppToCSharpWinRT*, und wählen Sie **Projekt neu laden** aus. 

```xml
  <PropertyGroup Condition="'$(Configuration)'=='Release'" Label="Configuration">
...
    <UseDotNetNativeToolchain>true</UseDotNetNativeToolchain>
  </PropertyGroup>
  <PropertyGroup Condition="$(Platform)'='ARM64'" Label="Configuration">
    <UseDotNetNativeToolchain Condition="'$(UseDotNetNativeToolchain)'==''">true</UseDotNetNativeToolchain>
  </PropertyGroup>
```

## <a name="related-topics"></a>Verwandte Themen
* [Komponenten für Windows-Runtime in C# und Visual Basic](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Windows-Runtime-Komponenten mit C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)