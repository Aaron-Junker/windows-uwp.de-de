---
description: Enthält eine Einführung in C++/WinRT – eine Standard C++ Sprachprojektion für Windows-Runtime-APIs.
title: Einführung in C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, Einführung
ms.localizationpriority: medium
ms.openlocfilehash: 267d4c92545d9049f56ea6a174af004fb63a10ee
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63790161"
---
# <a name="introduction-to-cwinrt"></a>Einführung in C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Mit C++/WinRT können Sie Windows-Runtime-APIs mit jedem standardkonformen C++17-Compiler erstellen und verwenden. Das in Version 10.0.17134.0 (Windows 10, Version 1803) eingeführte Windows SDK enthält C++/WinRT.

C++/WinRT ist der von Microsoft empfohlene Ersatz für die [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)-Programmiersprache und die [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). Die komplette Liste der [Themen zu C++/WinRT](index.md#topics-about-cwinrt) bietet Informationen zum Interagieren mit und Portieren aus C++/CX und WRL.

> [!IMPORTANT]
> Einige der wichtigsten zu beachtenden Teile von C++/WinRT sind in den Abschnitten [SDK-Unterstützung für C++/WinRT](#sdk-support-for-cwinrt) und [Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) beschrieben.

## <a name="language-projections"></a>Sprachprojektionen
Die Windows-Runtime basiert auf COM-APIs (Component Object Model) und ist für den Zugriff über *Sprachprojektionen* konzipiert. Eine Projektion verbirgt die COM-Details und bietet eine natürlichere Programmiererfahrung für eine bestimmte Sprache.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Die C++/WinRT-Sprachprojektion in der Windows UWP-API-Referenz
Wenn Sie [Windows UWP-APIs](https://docs.microsoft.com/uwp/api/) anzeigen, klicken Sie auf das Kombinationsfeld **Sprache** oben rechts, und wählen Sie **C++/WinRT** aus, um API-Syntaxblöcke so anzuzeigen, wie sie in der C++/WinRT-Sprachprojektion erscheinen.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket
Für Visual Studio-Unterstützung benötigen Sie Visual Studio 2019 oder Visual Studio 2017 (mindestens Version 15.6; empfohlen wird mindestens 15.7). Im Visual Studio Installer müssen Sie außerdem (unter **Installationsdetails** > **	Entwicklung für die universelle Windows-Plattform**) die **Tools für Universelle Windows-Plattform – C++ (v14x)** -Option(en) installieren, falls dies noch nicht erfolgt ist. Wählen Sie zudem unter Windows **Einstellungen** > **Update \& Sicherheit** > **Für Entwickler** die Option **Entwicklermodus** anstelle der Option **Apps querladen** aus.

Wir empfehlen, mit den aktuellen Versionen von Visual Studio und Windows SDK zu entwickeln. Wenn Sie jedoch eine Version von C++/WinRT aus dem Windows SDK vor 10.0.17763.0 (Windows 10, Version 1809) verwenden, benötigen Sie zum Verwenden der oben erwähnten Header für Windows-Namespaces im Projekt mindestens Windows SDK-Zielversion 10.0.17134.0 (Windows 10, Version 1803).

Es wird empfohlen, die aktuelle Version der [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) vom [Visual Studio Marketplace](https://marketplace.visualstudio.com/) herunterzuladen und zu installieren.

- Die VSIX-Erweiterung bietet Ihnen C++/WinRT-Projekt- und -Elementvorlagen in Visual Studio, sodass Sie in die Entwicklung mit C++/WinRT einsteigen können.
- Außerdem erhalten Sie damit eine systemeigene Visual Studio-Debug-Visualisierung (natvis) von projizierten C++/WinRT-Typen, die eine Erfahrung wie beim C#-Debuggen bereitstellt. Natvis ist für Debugbuilds automatisch. Sie können es für Releasebuilds verwenden, indem Sie das Symbol WINRT_NATVIS definieren.

Die Visual Studio-Projektvorlagen für C++/WinRT werden in den nachfolgenden Abschnitten beschrieben. Beim Erstellen eines neuen C++/WinRT-Projekts mit installierter aktueller Version der VSIX-Erweiterung wird vom neuen C++/WinRT-Projekt automatisch das [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) installiert. Das **Microsoft.Windows.CppWinRT**-NuGet-Paket bietet C++/WinRT-Build-Unterstützung (MSBuild-Eigenschaften und -Ziele), wodurch Ihr Projekt zwischen einem Entwicklungscomputer und einem Build-Agent (auf dem nur das NuGet-Paket, nicht aber die VSIX-Erweiterung installiert ist) portiert werden kann.

> [!IMPORTANT]
> Wenn Sie über Projekte verfügen, die mit einer VSIX-Version vor 1.0.190128.4 erstellt (bzw. für die Arbeit mit einer solchen Version aktualisiert) wurden, lesen Sie den Abschnitt [Frühere Versionen der VSIX-Erweiterung](#earlier-versions-of-the-vsix-extension). Dieser Abschnitt enthält wichtige Informationen zur Konfiguration Ihrer Projekte. Diese benötigen Sie zum Aktualisieren auf die aktuelle Version der VSIX-Erweiterung.

> [!NOTE]
> Da C++/WinRT Funktionen aus dem C++17-Standard verwendet, legt das NuGet-Paket die Projekteigenschaft **C/C++**  > **Sprache** > **C++-Sprachstandard** > **ISO C++17 Standard (/std:c++17)** in Visual Studio fest. Zudem wird die [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file)-Compileroption hinzugefügt.
> 
> Möglicherweise möchten Sie auch die Option **Konformitätsmodus: Ja (/permissive-)** festlegen, die Ihren Code weiter so beschränkt, dass er konform mit den Standards ist. Eine weitere zu beachtende Projekteigenschaft ist **C/C++**  > **Allgemein** > **Warnungen als Fehler behandeln**. Legen Sie diese auf **Ja (/WX)** oder **Nein (/WX-)** fest. Manchmal generieren vom `cppwinrt.exe`-Tool erzeugte Quelldateien Warnungen, bis Sie Ihre Implementierung hinzufügen.

Mit der oben beschriebenen Systemkonfiguration sind Sie in der Lage, ein C++/WinRT-Projekt in Visual Studio zu erstellen, zu kompilieren und zu öffnen sowie bereitzustellen.

Sie können ein vorhandenes Projekt auch konvertieren, indem Sie das **Microsoft.Windows.CppWinRT**-NuGet-Paket manuell installieren. Öffnen Sie nach dem Installieren der bzw. Aktualisieren auf die aktuelle Version der VSIX-Erweiterung das vorhandene Projekt in Visual Studio, und klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Durchsuchen**, geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **Installieren**, um das Paket für das Projekt zu installieren. Sobald Sie das Paket hinzugefügt haben, steht Ihnen die C++/WinRT MSBuild-Unterstützung für das Projekt zur Verfügung, einschließlich des Aufrufs des `cppwinrt.exe`-Tools. Seit Version 2.0 enthält das **Microsoft.Windows.CppWinRT**-NuGet-Paket das `cppwinrt.exe`-Tool.

Sie können mit dem `cppwinrt.exe`-Tool auf eine Windows-Runtime-Metadatendatei (`.winmd`) verweisen, um eine headerdateibasierte C++-Standardbibliothek zu erstellen, die die in den Metadaten beschriebenen APIs zur Verwendung über den C++/WinRT-Code *projiziert*. Windows-Runtime-Metadaten-Dateien (`.winmd`) bieten eine kanonische Möglichkeit, eine Windows-Runtime-API-Oberfläche zu beschreiben. Durch Verweisen von `cppwinrt.exe` auf Metadaten können Sie eine Bibliothek zur Verwendung mit einer beliebigen, in einer Zweit- oder Drittanbieter-Komponente für Windows-Runtime oder in Ihrer eigenen Anwendung implementierten Runtime-Klasse generieren. Weitere Informationen finden Sie unter [Nutzen von APIs mit C++/WinRT](consume-apis.md).

Mit C++/WinRT können Sie auch eigene Runtime-Klassen mithilfe von C++-Standardcode implementieren, ohne auf eine Programmierung im COM-Format zurückgreifen zu müssen. Für eine Runtime-Klasse beschreiben Sie einfach Ihre Typen in einer IDL-Datei, `midl.exe` und `cppwinrt.exe` generieren die Quellcodedateien mit den Implementierungstextbausteinen für Sie. Alternativ können Sie einfach Schnittstellen durch eine Ableitung von einer C++/WinRT-Basisklasse implementieren. Weitere Informationen finden Sie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

Sie können ein Projekt, das die C++/WinRT MSBuild-Unterstützung nutzt, durch Vorhandensein des installierten **Microsoft.Windows.CppWinRT**-NuGet-Pakets im Projekt angeben.

Hier sind die Visual Studio-Projektvorlagen, die von der VSIX-Erweiterung bereitgestellt werden.

### <a name="blank-app-cwinrt"></a>Blank App (C++/WinRT)
Eine Projektvorlage für eine UWP-App (Universelle Windows-Plattform), die über eine XAML-Benutzeroberfläche verfügt.

Visual Studio bietet XAML-Compiler-Unterstützung, um Implementierungs- und Header-Stubs aus der IDL-Datei (`.idl`) (Interface Definition Language) zu generieren, die sich hinter jeder XAML-Markup-Datei befindet. Definieren Sie in einer IDL-Datei alle lokalen Laufzeitklassen, auf die Sie in den XAML-Seiten Ihrer Anwendung verweisen möchten, und erstellen Sie das Projekt einmal, um Implementierungsvorlagen in `Generated Files` und Stubtypdefinitionen in `Generated Files\sources` zu generieren. Verwenden Sie dann die Stubtypdefinitionen als Referenz, um Ihre lokalen Laufzeitklassen zu implementieren. Wir empfehlen, jede Laufzeitklasse in einer eigenen IDL-Datei zu deklarieren.

Die Unterstützung der XAML-Entwurfsoberfläche in Visual Studio 2019 für C++/WinRT ist nahezu gleichwertig mit C#. In Visual Studio 2019 können Sie mit der Registerkarte **Ereignisse** im Fenster **Eigenschaften** Ereignishandler in einem C++/WinRT-Projekt hinzufügen. Sie können Ihrem Code Ereignishandler auch manuell hinzufügen&mdash;Weitere Informationen hierzu erhalten Sie unter [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](handle-events.md).

### <a name="core-app-cwinrt"></a>Core App (C++/WinRT)
Eine Projektvorlage für eine UWP-App (Universelle Windows-Plattform), die keine XAML-Benutzeroberfläche verwendet.

Stattdessen wird der C++/WinRT-Windows-Namespace-Header für den Windows.ApplicationModel.Core-Namespace verwendet. Nach dem Erstellen und Ausführen klicken Sie auf ein leeres Feld, um ein farbiges Quadrat hinzuzufügen; klicken Sie dann auf ein farbiges Quadrat, um es zu ziehen.

### <a name="windows-console-application-cwinrt"></a>Windows-Konsolenanwendung (C++/WinRT)
Eine Projektvorlage für eine C++/WinRT-Client-Anwendung für Windows Desktop, mit einer Konsolen-Benutzeroberfläche.

### <a name="windows-desktop-application-cwinrt"></a>Windows Desktopanwendung (C++/WinRT)
Eine Projektvorlage für eine C++/WinRT-Clientanwendung für Windows Desktop, die einen Windows Runtime-[Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) in einem Win32-**MessageBox** anzeigt.

### <a name="windows-runtime-component-cwinrt"></a>Komponente für Windows-Runtime (C++/WinRT)
Eine Projektvorlage für eine Komponente; typischerweise für die Nutzung in einer UWP-App (Universelle Windows-Plattform).

Diese Vorlage veranschaulicht die `midl.exe` > `cppwinrt.exe`-Toolkette, in der Windows-Runtime-Metadaten (`.winmd`) aus IDL generiert werden, und die anschließende Implementierung und Generierung von Header-Stubs aus den Windows-Runtime-Metadaten.

Definieren Sie in einer IDL-Datei die Laufzeitklassen in Ihrer Komponenten, deren Standardschnittstelle und alle anderen Schnittstellen, die sie implementieren. Erstellen Sie das Projekt einmalig, um `module.g.cpp`, `module.h.cpp`, Implementierungsvorlagen in `Generated Files` und Stubtypdefinitionen in `Generated Files\sources` zu generieren. Verwenden Sie dann die Stubtypdefinitionen als Referenz, um die Laufzeitklassen in Ihrer Komponente zu implementieren. Wir empfehlen, jede Laufzeitklasse in einer eigenen IDL-Datei zu deklarieren.

Packen Sie die erstellte Binärdatei für die Komponente für Windows-Runtime und deren `.winmd` mit der UWP-App, die diese nutzt.

## <a name="earlier-versions-of-the-vsix-extension"></a>Frühere Versionen der VSIX-Erweiterung
Es wird empfohlen, dass Sie die aktuelle Version der [VSIX-Erweiterung](https://aka.ms/cppwinrt/vsix) installieren (bzw. auf diese Version aktualisieren). Die anschließende automatische Aktualisierung ist standardmäßig festgelegt. Wenn Sie so vorgehen und über Projekte verfügen, die mit einer Version der VSIX-Erweiterung vor 1.0.190128.4 erstellt wurden, bietet Ihnen dieser Abschnitt wichtige Informationen zum Aktualisieren der Projekte für die Arbeit mit der neuen Version. Auch wenn Sie keine Aktualisierung ausführen, können die Informationen in diesem Abschnitt durchaus hilfreich sein.

Hinsichtlich der unterstützten Version des Windows SDK und von Visual Studio sowie der Visual Studio-Konfigurationen beziehen sich die Informationen im obigen Abschnitt [Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) auf frühere Versionen der VSIX-Erweiterung. Im Folgenden werden wichtige Unterschiede in Bezug auf das Verhalten und die Konfiguration von Projekten, die mit früheren Versionen erstellt bzw. für die Arbeit mit früheren Versionen aktualisiert wurden beschrieben.

### <a name="created-earlier-than-101810022"></a>Erstellt mit früherer Version als 1.0.181002.2
Wenn Ihr Projekt mit einer Version der VSIX-Erweiterung vor 1.0.181002.2 erstellt wurde, ist die C++/WinRT-Build-Unterstützung in die betreffende Version der VSIX-Erweiterung integriert. Für das Projekt ist die `<CppWinRTEnabled>true</CppWinRTEnabled>`-Eigenschaft in der `.vcxproj`-Datei festgelegt.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Sie können das Projekt manuell aktualisieren, indem Sie das **Microsoft.Windows.CppWinRT**-NuGet-Paket installieren. Öffnen Sie nach dem Installieren der bzw. Aktualisieren auf die aktuelle Version der VSIX-Erweiterung das Projekt in Visual Studio, und klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Durchsuchen**, geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **Installieren**, um das Paket für das Projekt zu installieren.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Erstellt mit oder aktualisiert auf eine Version zwischen 1.0.181002.2 und 1.0.190128.3
Wenn das Projekt mit einer Version der VSIX-Erweiterung zwischen 1.0.181002.2 und 1.0.190128.3 (einschließlich) erstellt wurde, wurde das **Microsoft.Windows.CppWinRT**-NuGet-Paket automatisch von der Projektvorlage im Projekt installiert. Möglicherweise haben Sie auch ein älteres Projekt für die Verwendung einer Version der VSIX-Erweiterung in diesem Bereich aktualisiert. Falls dies zutrifft, ist &mdash; da die Build-Unterstützung in Versionen der VSIX-Erweiterung in diesem Bereich gegeben war &mdash; für Ihr aktualisiertes Projekt das **Microsoft.Windows.CppWinRT**-NuGet-Paket installiert, möglicherweise aber auch nicht installiert.

Befolgen Sie zum Aktualisieren des Projekts die Anweisungen im vorherigen Abschnitt, und stellen Sie sicher, dass für das Projekt das **Microsoft.Windows.CppWinRT**-NuGet-Paket installiert ist.

### <a name="invalid-upgrade-configurations"></a>Ungültige Aktualisierungskonfigurationen
Mit der aktuellen Version der VSIX-Erweiterung kann ein Projekt nicht die `<CppWinRTEnabled>true</CppWinRTEnabled>`-Eigenschaft aufweisen, wenn nicht gleichzeitig auch das **Microsoft.Windows.CppWinRT**-NuGet-Paket installiert ist. Ein Projekt mit einer solchen Konfiguration erzeugt diese Build-Fehlermeldung: „Die C++/WinRT-VSIX stellt keine Buildunterstützung für das Projekt mehr zur Verfügung.  Fügen Sie dem Projekt einen Verweis auf das Microsoft.Windows.CppWinRT-Nuget-Paket hinzu.“

Wie bereits erwähnt, muss in einem C++/WinRT-Projekt nun das NuGet-Paket installiert sein.

Da das `<CppWinRTEnabled>`-Element nun veraltet ist, können Sie optional `.vcxproj` bearbeiten und das Element löschen. Dies ist nicht unbedingt erforderlich, jedoch möglich.

Wenn Ihr `.vcxproj` `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>` enthält, können Sie es entfernen, sodass das Erstellen möglich ist, ohne dass die C++/WinRT-VSIX-Erweiterung installiert ist.

## <a name="sdk-support-for-cwinrt"></a>SDK-Unterstützung für C++/WinRT
Obgleich nur aus Kompatibilitätsgründen, enthält das Windows SDK ab Version 10.0.17134.0 (Windows 10, Version 1803) eine headerdateibasierte C++-Standardbibliothek für die Verwendung von Erstanbieter-Windows-APIs (Windows-Runtime-APIs in Windows-Namespaces). Diese Header befinden sich im Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Ab Windows SDK-Version 10.0.17763.0 (Windows 10, Version 1809) werden diese Header automatisch im Ordner *$(GeneratedFilesDir)* Ihres Projekts generiert.

Das Windows SDK enthält außerdem das `cppwinrt.exe`-Tool (wiederum aus Kompatibilitätsgründen). Wir empfehlen jedoch, dass Sie stattdessen die aktuelle neueste Version von `cppwinrt.exe` installieren und verwenden; diese ist im **Microsoft.Windows.CppWinRT**-NuGet-Paket enthalten. Dieses Paket und `cppwinrt.exe` werden in den obigen Abschnitten beschrieben.

## <a name="custom-types-in-the-cwinrt-projection"></a>Benutzerdefinierte Typen in der C++/WinRT-Projektion
Sie können in Ihrer C++/WinRT-Programmierung Standard-C++-Sprachfunktionen sowie [Standard-C++-Datentypen und C++/WinRT](std-cpp-data-types.md)&mdash;(einschließlich einiger C++ Standard-Bibliotheksdatentypen) verwenden. Sie werden aber auch einige benutzerdefinierte Datentypen in der Projektion bemerken und können diese verwenden. Beispielsweise verwenden wir [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) im Schnellstart-Codebeispiel in [Erste Schritte mit C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) ist ein weiterer Typ, den Sie wahrscheinlich irgendwann verwenden werden. Es ist jedoch weniger wahrscheinlich, dass Sie direkt einen Typ wie [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) verwenden. Möglicherweise werden Sie die Typen auch nicht verwenden, sodass Sie keinen Code ändern müssen, wenn ein entsprechender Typ in der C++ Standardbibliothek verfügbar ist.

> [!WARNING]
> Es gibt außerdem Typen, die Sie bei genauerer Betrachtung des C++/WinRT-Windows-Namespace-Headers entdecken. Ein Beispiel ist **winrt::param::hstring**, es stehen jedoch auch Beispiele für die Sammlung zur Verfügung. Diese dienen ausschließlich der Optimierung der Bindung von Eingabeparametern, erzielen hohe Leistungsverbesserungen und sorgen dafür, dass die meisten aufrufenden Muster für verwandte C++-Standardtypen und -container „einfach funktionieren“. Diese Typen werden von der Projektion immer nur in Fällen verwendet, in denen sie den größten Wert hinzufügen. Sie sind umfassend optimiert und nicht für die allgemeine Verwendung vorgesehen. Geben Sie nicht der Versuchung nach, sie selbst zu nutzen. Sie sollten auch keine Typen aus dem `winrt::impl`-Namespace verwenden, da es sich dabei um Implementierungstypen handelt, die Änderungen unterliegen. Sie sollten weiterhin Standardtypen oder Typen aus dem [WinRT-Namespace](/uwp/cpp-ref-for-winrt/winrt) verwenden.

## <a name="important-apis"></a>Wichtige APIs
* [winrt::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix)
* [Erste Schritte mit C++/WinRT](get-started.md)
* [C++-Standarddatentypen und C++/WinRT](std-cpp-data-types.md)
* [Verarbeitung von Zeichenfolgen in C++/WinRT](strings.md)
* [Windows UWP-APIs](https://docs.microsoft.com/uwp/api/)
