---
description: Eine Einführung in C++/WinRT – einer Standard C++ Sprachprojektion für Windows-Runtime-APIs.
title: Einführung in C++/WinRT
ms.date: 01/31/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, Einführung
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649695"
---
# <a name="introduction-to-cwinrt"></a>Einführung in C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Mit C++/WinRT können Sie Windows-Runtime-APIs mit jedem standardkonformen C++17-Compiler erstellen und verwenden. Das in Version 10.0.17134.0 (Windows 10, Version 1803) eingeführte Windows SDK enthält C++/WinRT.

C++ / WinRT ist von Microsoft empfohlene Ersatz für die [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) -sprachprojektion und [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). Die vollständige Liste der [Themen über C++ / WinRT](index.md#topics-about-cwinrt) enthält Informationen zu Interoperabilität mit, und Portieren von C++ / CX und WRL.

> [!IMPORTANT]
> Zwei der wichtigsten Teile von C++ / WinRT berücksichtigen werden in den Abschnitten beschrieben [SDK-Unterstützung für C++ / WinRT](#sdk-support-for-cwinrt) und [Visual Studio-Unterstützung für C++ / WinRT, XAML, die VSIX-Erweiterung und NuGet-Paket](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Sprachprojektionen
Die Windows-Runtime basiert auf COM-APIs (Component Object Model) und ist für den Zugriff über *Sprachprojektionen* konzipiert. Eine Projektion verbirgt die COM-Details und bietet eine natürlichere Programmiererfahrung für eine bestimmte Sprache.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Die C++/WinRT-Sprachprojektion in der Windows UWP-API-Referenz
Wenn Sie [Windows UWP-APIs](https://docs.microsoft.com/uwp/api/) anzeigen, klicken Sie auf das Kombinationsfeld **Sprache** oben rechts und wählen Sie **C++/WinRT** aus, um API-Syntaxblöcke so anzuzeigen, wie sie in der C++/WinRT-Sprachprojektion erscheinen.

## <a name="sdk-support-for-cwinrt"></a>SDK-Unterstützung für C++/WinRT
Ab Version 10.0.17134.0 (Windows 10, Version 1803) enthält das Windows SDK eine headerbasierte Standard-C++-Bibliothek für die Verwendung von Erstanbieter-Windows-APIs (Windows-Runtime-APIs in Windows-Namespaces). C++/WinRT enthält außerdem das `cppwinrt.exe`-Tool, mit dem Sie auf eine Windows-Runtime-Metadatendatei (`.winmd`) verweisen können, um eine headerbasierte C++-Standardbibliothek zu erstellen, die die in den Metadaten beschriebenen APIs zur Verwendung über den C++/WinRT-Code *projiziert*. Windows-Runtime-Metadaten-Dateien (`.winmd`) bieten eine kanonische Möglichkeit, eine Windows-Runtime-API-Oberfläche zu beschreiben. Durch Verweisen von `cppwinrt.exe` auf Metadaten können Sie eine Bibliothek zur Verwendung mit einer beliebigen, in einer Zweit- oder Drittanbieter-Komponente für Windows-Runtime oder in Ihrer eigenen Anwendung implementierten Runtime-Klasse generieren. Weitere Informationen finden Sie unter [Nutzen von APIs mit C++/WinRT](consume-apis.md).

Mit C++/WinRT können Sie auch eigene Runtime-Klassen mithilfe von C++-Standardcode implementieren, ohne auf eine Programmierung im COM-Format zurückgreifen zu müssen. Für eine Runtime-Klasse beschreiben Sie einfach Ihre Typen in einer IDL-Datei, `midl.exe` und `cppwinrt.exe` generieren die Quellcodedateien mit den Implementierungstextbausteinen für Sie. Alternativ können Sie einfach Schnittstellen durch eine Ableitung von einer C++/WinRT-Basisklasse implementieren. Weitere Informationen finden Sie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio-Unterstützung für C++ / WinRT, XAML, die VSIX-Erweiterung, und das NuGet-Paket
Visual Studio-Unterstützung, zusätzlich zum Windows SDK-Ziel Mindestversion 10.0.17134.0 (Windows 10, Version 1803), benötigen Sie Visual Studio 2017 (mindestens Version 15.6; es wird empfohlen, mindestens 15.7), oder Visual Studio-2019. Wenn Sie es bereits installiert haben, müssen Sie installieren die **C++ Universal Windows Platform-Tools** option in Visual Studio-Installer. Und in Windows **Einstellungen** > **Update \& Sicherheit** > **für Entwickler**, wählen Sie die **Developer Modus** Option anstelle der **Querladen von apps** Option.

Sie müssen zum Herunterladen und installieren die neueste Version von der [C++ / WinRT Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix) aus der [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- Die VSIX-Erweiterung können Sie C++ / WinRT Projekt- und Elementvorlagen in Visual Studio, damit Sie mit C++ beginnen können c++ / WinRT-Entwicklung.
- Darüber hinaus sie erhalten Visual Studio-systemeigenen Debug-Visualisierung (Natvis) von C++ / WinRT projizierte Typen. Bereitstellen von ähnlich wie C# Debuggen. Natvis ist für Debugbuilds automatisch. Sie können es für Releasebuilds verwenden, indem Sie das Symbol WINRT_NATVIS definieren.

Die Visual Studio-Projektvorlagen für C++ / WinRT werden unten beschrieben. Bei der Erstellung einer neuen C + c++ / WinRT-Projekt mit der neuesten Version der VSIX-Erweiterung installiert, der neuen C++-/ c++ / WinRT-Projekt wird automatisch installiert. die [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). Die **Microsoft.Windows.CppWinRT** NuGet-Paket stellt C++ / WinRT-Buildunterstützung (MSBuild-Eigenschaften und Ziele), machen Sie Ihr Projekt portabel zwischen einem Entwicklungscomputer und einen Build-Agent (auf dem nur das NuGet-Paket, und nicht die VSIX-Erweiterung ist installiert).

> [!IMPORTANT]
> Wenn Sie über Projekte verfügen, die mit erstellt (oder aktualisiert, um die Arbeit mit wurden) eine Version des zuvor die VSIX-Erweiterung als 1.0.190128.4, dann finden Sie unter [frühere Versionen der Erweiterung VSIX](#earlier-versions-of-the-vsix-extension). Dieser Abschnitt enthält wichtige Informationen über die Konfiguration Ihrer Projekte, die Sie Informationen für die Aktualisierung, um die neueste Version der VSIX-Erweiterung verwenden müssen.

Da C++ / WinRT werden Features von der C ++ 17-Standard verwendet, benötigt Projekteigenschaft **C/C++-** > **Sprache** > **Standard der Sprache C++**  >  **ISO C ++ 17-Standard (/ Std: c ++ 17)**. Sie können auch festlegen möchten **Konformitätsmodus: Ja (/ PERMISSIVE--)**, die weiter einschränkt, Ihren Codes, der mit Standards kompatibel ist.

Eine weitere zu beachtende Projekteigenschaft ist **C/C++** > **Allgemein** > **Warnungen als Fehler behandeln**. Legen Sie diese auf **Ja (/WX)** oder **Nein (/WX-)** fest. Manchmal generieren vom `cppwinrt.exe`-Tool erzeugte Quelldateien Warnungen, bis Sie Ihre Implementierung hinzufügen.

Mit dem System, wie oben beschrieben festlegen, Sie werden zum Erstellen und zu erstellen oder Öffnen von C++ / WinRT Projekt in Visual Studio und die Bereitstellung.

Alternativ können Sie ein vorhandenes Projekt konvertieren, durch Manuelles Installieren der **Microsoft.Windows.CppWinRT** NuGet-Paket. Nach dem Installieren (oder zu aktualisieren) die neueste Version der VSIX-Erweiterung, öffnen Sie das vorhandene Projekt in Visual Studio, klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Navigieren Sie**geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **installieren** zum Installieren des Pakets für das betreffende Projekt. Nachdem Sie das Paket hinzugefügt haben, erhalten Sie C++ / WinRT-MSBuild-Support für das Projekt, einschließlich des Aufrufens der `cppwinrt.exe` Tool.

Erkennen Sie ein Projekt, verwendet der C++ / WinRT-MSBuild-Unterstützung durch das Vorhandensein der **Microsoft.Windows.CppWinRT** NuGet-Paket im Projekt installiert.

Hier sind die Visual Studio-Projektvorlagen, die von der VSIX-Erweiterung bereitgestellt.

### <a name="windows-console-application-cwinrt"></a>Windows Console Application (C++/WinRT)
Eine Projektvorlage für eine C++/WinRT-Client-Anwendung für Windows Desktop, mit einer Konsolen-Benutzeroberfläche.

### <a name="blank-app-cwinrt"></a>Blank App (C++/WinRT)
Eine Projektvorlage für eine UWP-App (Universelle Windows-Plattform), die über eine XAML-Benutzeroberfläche verfügt.

Visual Studio bietet XAML-Compiler-Unterstützung, um Implementierungs- und Header-Stubs aus der IDL-Datei (`.idl`) (Interface Definition Language) zu generieren, die sich hinter jeder XAML-Markup-Datei befindet. Definieren Sie in einer IDL-Datei alle lokalen Laufzeitklassen, auf die Sie in den XAML-Seiten Ihrer Anwendung verweisen möchten, und erstellen Sie das Projekt einmal, um Implementierungsvorlagen in `Generated Files` und Stubtypdefinitionen in `Generated Files\sources` zu generieren. Verwenden Sie dann die Stubtypdefinitionen als Referenz, um Ihre lokalen Laufzeitklassen zu implementieren. Wir empfehlen, jede Laufzeitklasse in einer eigenen IDL-Datei zu deklarieren.

Visual Studio XAML Design Surface-Unterstützung für C++ / WinRT ist in der Nähe Parität mit C#. Eine Ausnahme ist die **Ereignisse** Registerkarte die **Eigenschaften** Fenster. Mit einem C# -Projekt können Sie die Registerkarte Ereignishandler hinzufügen mit C++ / WinRT-Projekt, die diese Funktion ist nicht vorhanden. Aber [Behandeln von Ereignissen mithilfe von Delegaten in C++ / WinRT](handle-events.md) Informationen zum Hinzufügen von Ereignishandlern an Ihrem Code.

### <a name="core-app-cwinrt"></a>Core App (C++/WinRT)
Eine Projektvorlage für eine UWP-App (Universelle Windows-Plattform), die keine XAML-Benutzeroberfläche verwendet.

Stattdessen wird der C++/WinRT-Windows-Namespace-Header für den Windows.ApplicationModel.Core-Namespace verwendet. Nach dem Erstellen und Ausführen klicken Sie auf ein leeres Feld, um ein farbiges Quadrat hinzuzufügen; klicken Sie dann auf ein farbiges Quadrat, um es zu ziehen.

### <a name="windows-runtime-component-cwinrt"></a>Komponente für Windows-Runtime (C++/WinRT)
Eine Projektvorlage für eine Komponente; typischerweise für die Nutzung in einer UWP-App (Universelle Windows-Plattform).

Dieses Template demonstriert die `midl.exe` > `cppwinrt.exe`-Toolchain, in der Windows-Runtime-Metadaten (`.winmd`) aus IDL generiert werden, und dann die Implementierung und Header-Stubs aus den Windows-Runtime-Metadaten generiert werden.

Definieren Sie in einer IDL-Datei die Laufzeitklassen in Ihrer Komponenten, deren Standardschnittstelle und alle anderen Schnittstellen, die sie implementieren. Erstellen Sie das Projekt einmalig, um `module.g.cpp`, `module.h.cpp`, Implementierungsvorlagen in `Generated Files` und Stubtypdefinitionen in `Generated Files\sources` zu generieren. Verwenden Sie dann die Stubtypdefinitionen als Referenz, um die Laufzeitklassen in Ihrer Komponente zu implementieren. Wir empfehlen, jede Laufzeitklasse in einer eigenen IDL-Datei zu deklarieren.

Packen Sie die erstellte Binärdatei für die Komponente für Windows-Runtime und deren `.winmd` mit der UWP-App, die diese nutzt.

## <a name="earlier-versions-of-the-vsix-extension"></a>Frühere Versionen der VSIX-Erweiterung
Es wird empfohlen, die Sie installieren (oder zu aktualisieren) die neueste Version von der [VSIX-Erweiterung](https://aka.ms/cppwinrt/vsix). Selbst in der Standardeinstellung aktualisieren konfiguriert ist. Wenn Sie dies, und Sie Projekte, die mit einer Version der Erweiterung VSIX vor 1.0.190128.4, und dann in diesem Abschnitt erstellte enthält wichtige Informationen zu diesen Projekten arbeiten Sie mit der neuen Version aktualisieren. Wenn Sie nicht aktualisieren, klicken Sie dann finden noch die Informationen in diesem Abschnitt nützlich Sie.

Unterstützt Sie hinsichtlich der Windows-SDK und Versionen von Visual Studio und Visual Studio-Konfiguration, die Informationen in den [Visual Studio-Unterstützung für C++ / WinRT, XAML, die VSIX-Erweiterung, und das NuGet-Paket](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) oben genannten Abschnitt gilt für das zuvor die Versionen der VSIX-Erweiterung. Die unten aufgeführten Informationen beschreibt wichtige Unterschiede in Bezug auf das Verhalten und Konfiguration von Projekten mit erstellt (oder ein Upgrade auf die Arbeit mit) zuvor Versionen.

### <a name="created-earlier-than-101810022"></a>Erstellt vor 1.0.181002.2
Wenn das Projekt, mit einer Version der Erweiterung VSIX vor 1.0.181002.2, und C++ erstellt wurde / WinRT-Unterstützung wurde in dieser Version der VSIX-Erweiterung erstellt. Das Projekt enthält die `<CppWinRTEnabled>true</CppWinRTEnabled>` Eigenschaft festgelegt wird, der `.vcxproj` Datei.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Sie können das Projekt aktualisieren, durch Manuelles Installieren der **Microsoft.Windows.CppWinRT** NuGet-Paket. Nach dem Installieren (oder ein Upgrade auf) die neueste Version der Erweiterung VSIX-Projekt in Visual Studio öffnen, klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Navigieren Sie**geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **installieren** zum Installieren des Pakets für Ihr Projekt.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Mit erstellt (oder ein Upgrade auf) zwischen 1.0.181002.2 und 1.0.190128.3
Wenn das Projekt, mit einer Version der VSIX-Erweiterung zwischen 1.0.181002.2 und 1.0.190128.3, einschließlich erstellt wurde der **Microsoft.Windows.CppWinRT** NuGet-Paket im Projekt automatisch durch das Projekt installiert wurde Vorlage. Sie können auch ein älteres Projekt um eine Version der VSIX-Erweiterung in diesem Bereich verwenden aktualisiert haben. Wenn, klicken Sie dann haben&mdash;seit Build-Unterstützung auch weiterhin vorhanden, in den Versionen der VSIX-Erweiterung in diesem Bereich&mdash;das aktualisierte Projekt möglicherweise oder verfügen nicht über die **Microsoft.Windows.CppWinRT** NuGet-Paket installiert.

Um Ihr Projekt zu aktualisieren, befolgen Sie die Anweisungen im vorherigen Abschnitt aus, und stellen Sie sicher, dass Ihr Projekt verfügt die **Microsoft.Windows.CppWinRT** NuGet-Paket installiert.

### <a name="invalid-upgrade-configurations"></a>Ungültige Upgrade-Konfigurationen
Mit der neuesten Version der VSIX-Erweiterung, es gilt nicht für ein Projekt, damit die `<CppWinRTEnabled>true</CppWinRTEnabled>` Eigenschaft, wenn sie außerdem besitzen nicht die **Microsoft.Windows.CppWinRT** NuGet-Paket installiert. Ein Projekt mit dieser Konfiguration erzeugt die Fehlermeldung "Build", "C++ / WinRT VSIX nicht mehr unterstützt das Projekt erstellen.  Bitte fügen Sie einen Projektverweis auf die Microsoft.Windows.CppWinRT Nuget-Paket. hinzu"

Die oben genannten, C++ / WinRT-Projekt nun das NuGet-Paket installiert haben muss.

Da die `<CppWinRTEnabled>` Element ist veraltet, Sie können optional auch bearbeiten, Ihre `.vcxproj`, und löschen Sie das Element. Es ist nicht unbedingt erforderlich, aber es ist eine Option.

Auch wenn Ihre `.vcxproj` enthält `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, und Sie es entfernen können, sodass Sie erstellen können, ohne dass der C++-/ c++ / WinRT-VSIX-Erweiterung installiert werden.

## <a name="custom-types-in-the-cwinrt-projection"></a>Benutzerdefinierte Typen in der C++/WinRT-Projektion
In Ihrem C++ / WinRT programmieren, können Sie standard C++-Sprachfeatures und [Standard C++-Datentypen und C++ / WinRT](std-cpp-data-types.md)&mdash;einschließlich einige C++-Standardbibliothek-Datentypen. Sie werden aber auch einige benutzerdefinierte Datentypen in der Projektion bemerken und können diese verwenden. Beispielsweise verwenden wir [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) im Schnellstart-Codebeispiel in [Erste Schritte mit C++/WinRT](get-started.md).

[**WinRT::com_array** ](/uwp/cpp-ref-for-winrt/com-array) ist eine andere Art, die Sie wahrscheinlich an einem bestimmten Punkt verwendet werden. Es ist jedoch weniger wahrscheinlich, dass Sie direkt einen Typ wie [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) verwenden. Möglicherweise werden Sie die Typen auch nicht verwenden, sodass Sie keinen Code ändern müssen wenn ein entsprechender Typ in der C++ Standardbibliothek verfügbar ist.

> [!WARNING]
> Es gibt außerdem Typen, die Sie bei genauerer Betrachtung des C++/WinRT-Windows-Namespace-Headers entdecken. Ein Beispiel ist **winrt::param::hstring**, es stehen jedoch auch Beispiele für die Sammlung zur Verfügung. Diese dienen ausschließlich der Optimierung der Bindung von Eingabeparametern, erzielen hohe Leistungsverbesserungen und sorgen dafür, dass die meisten aufrufenden Muster für verwandte C++-Standardtypen und -container „einfach funktionieren“. Diese Typen werden von der Projektion immer nur in Fällen verwendet, in denen sie den größten Wert hinzufügen. Sie sind umfassend optimiert und nicht für die allgemeine Verwendung vorgesehen. Geben Sie nicht der Versuchung nach, sie selbst zu nutzen. Sie sollten auch nichts aus dem `winrt::impl` -Namespace verwenden, da es sich dabei um Implementierungstypen handelt, die Änderungen unterliegen. Sie sollten weiterhin Standardtypen oder Typen aus dem [WinRT-Namespace](/uwp/cpp-ref-for-winrt/winrt) verwenden.

## <a name="important-apis"></a>Wichtige APIs
* [WinRT::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++ / WinRT-Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix)
* [Erste Schritte mit C++/WinRT](get-started.md)
* [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md)
* [Zeichenfolgenbehandlung in C++ / WinRT](strings.md)
* [Windows UWP-APIs](https://docs.microsoft.com/uwp/api/)
