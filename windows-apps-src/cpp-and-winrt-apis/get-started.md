---
description: Damit Sie C++/WinRT schneller verwenden können, werden Ihnen in diesem Thema einige einfache Codebeispiele vorgestellt.
title: Erste Schritte mit C++/WinRT
ms.date: 10/19/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, erste schritte
ms.localizationpriority: medium
ms.openlocfilehash: c0d11a8718f61666d6285d8a1c91b48992044b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602235"
---
# <a name="get-started-with-cwinrt"></a>Erste Schritte mit C++/WinRT

Um Sie mit der Verwendung von geläufigkeit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), dieses Thema führt durch ein einfaches Codebeispiel, das auf Basis eines neuen **Windows-Konsolenanwendung (C++ / WinRT)** Projekt. Auch dieses Thema wird gezeigt, wie [hinzufügen C++ / WinRT-Support, um ein Windows-Desktop-Anwendungsprojekt](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!IMPORTANT]
> Bei Verwendung von Visual Studio 2017 (Version 15.8.0 oder höher), und auf dem Windows SDK, Version 10.0.17134.0 (Windows 10, Version 1803), klicken Sie dann eine neu erstellte C + c++ / WinRT-Projekt kann nicht mit dem Fehler kompiliert "*Fehler C3861 aus:"From_abi": Bezeichner wurde nicht gefunden*", und klicken Sie mit anderen Fehlern, die aus *base.h*. Die Lösung besteht darin, entweder Ziel einer höheren (genauer) Version des Windows SDK oder Set-Projekteigenschaft **C/C++-** > **Sprache** > **Konformitätsmodus: Nicht** (auch wenn **/ PERMISSIVE--** wird in den Projekteigenschaften **C/C++-** > **Sprache** > **über die Befehlszeile**  unter **zusätzliche Optionen**, löschen Sie sie).

## <a name="a-cwinrt-quick-start"></a>Schnelleinstieg zu C++/WinRT

> [!NOTE]
> Informationen zum Installieren und Verwenden von C++ / WinRT Visual Studio-Erweiterung (VSIX) (die projektunterstützung für die Vorlage bereitstellt) finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Erstellen Sie ein neues **Windows Console Application (C++/WinRT)**-Projekt.

Bearbeiten Sie `pch.h` und `main.cpp` folgendermaßen.

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Wir nehmen uns das kurze Codebeispiel oben Stück für Stück vor und erläutern, was in jedem Teil passiert.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

Die enthaltenen Header sind Teil des SDKs und befinden sich im Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio bezieht diesen Pfad in sein *IncludePath*-Makro ein. Die Header enthalten Windows-APIs, die in C++/WinRT projiziert werden. Anders ausgedrückt: Für jeden einzelnen Windows-Typ definiert C++/WinRT ein C++-freundliches Äquivalent (wird als *projizierter Typ* bezeichnet). Ein projizierter Typ verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, befindet sich aber im C++/**winrt**-Namespace. Wenn Sie diese in Ihrem vorkompilierten Header platzieren, werden die inkrementellen Buildzeiten reduziert.

> [!IMPORTANT]
> Wann immer Sie einen Typ aus einem Windows-Namespace verwenden möchten, schließen Sie die entsprechende C++/WinRT-Windows-Namespace-Headerdatei wie gezeigt ein. Der *zugehörige* Header ist derjenige mit dem gleichen Namen wie der Namespace des Typs. Um z. B. die C++/WinRT-Projektion für die Laufzeitklasse [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) zu verwenden, fügen Sie Folgendes ein: `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Die `using namespace`-Direktiven sind optional, aber praktisch. Das oben gezeigte Muster für diese Richtlinien (was die Suche nach unqualifizierten Namen für alles im**Winrt**-Namespace ermöglicht) eignet sich, wenn Sie ein neues Projekt beginnen und C++/WinRT die einzige Sprachprojektion ist, die Sie innerhalb dieses Projekts verwenden. Wenn Sie andererseits C++/WinRT-Code mit [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) und/oder SDK-ABI-Code (Application Binary Interface) kombinieren (Sie davon entweder portieren oder interagieren, eines oder beide dieser Modelle), dann lesen Sie die Themen [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md), [Wechsel zu C++/WinRT von C++/CX](move-to-winrt-from-cx.md) und [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

Der Aufruf von **winrt::init_apartment** initialisiert COM; standardmäßig in einem Multithread-Apartment.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Weisen Sie zwei Objekte mit Stapelzuordnung zu: Sie stellen den URI des Windows-Blogs dar, und einen Syndication-Client. Wir erstellen den URI mit einem einfachen Wide-String-Literal (unter [String-Verarbeitung in C++/WinRT](strings.md) finden Sie weitere Möglichkeiten zum Arbeiten mit Zeichenfolgen).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync** ](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) ist ein Beispiel einer asynchronen Windows-Runtime-Funktion. Das Codebeispiel erhält von **RetrieveFeedAsync** ein Objekt für einen asynchronen Vorgang. Es ruft **get** für dieses Objekt auf, um den aufrufenden Thread zu blockieren und auf das Ergebnis zu warten (in diesem Fall einen Syndication-Feed). Weitere Informationen über Parallelität und Non-Blocking-Techniken finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) ist ein Bereich, der definiert, die von den Iteratoren Merry **beginnen** und **End** Funktionen (oder deren Varianten Konstanten umgekehrten und Konstante rückgängig gemacht). Aus diesem Grund können Sie **Items** entweder mit einer bereichsbasierten `for`-Anweisung oder mit der Template-Funktion **std::for_each** auflisten.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Der Code erhält dann den Titeltext des Feeds als [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Objekt (siehe [String-Verarbeitung in C++/WinRT](strings.md)). **hstring** ist dann die Ausgabe, über die **c_str**-Funktion, die das Muster wiedergibt, das mit C++-Standardbibliothekzeichenfolgen verwendet wird.

Wie Sie sehen können, unterstützt C++/WinRT moderne, klassenähnliche C++ Ausdrücke wie `syndicationItem.Title().Text()`. Dies ist ein anderer und saubererer Programmierstil als die traditionelle COM-Programmierung. Sie müssen COM nicht direkt initialisieren, arbeiten Sie mit COM-Zeigern.

Sie müssen auch keine HRESULT-Rückgabecodes verarbeiten. Für einen natürlichen und modernen Programmierstil konvertiert C++/WinRT Fehler-HRESULTs in Ausnahmen, wie z. B. [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error). Weitere Informationen zur Fehlerbehandlung sowie Codebeispiele finden Sie unter [Fehlerbehandlung bei C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Ändern Sie ein Windows-Desktop-Anwendungsprojekt zum Hinzufügen von C++ / WinRT-Unterstützung

In diesem Abschnitt erfahren Sie, wie Sie C++ hinzufügen können c++ / WinRT-Support, um ein Projekt für die Windows-Desktop-Anwendung, die möglicherweise. Wenn Sie ein vorhandenes Windows-Desktop-Anwendungsprojekt nicht haben, können Sie zusammen mit diesen Schritten ersten Erstellen einer folgen. Angenommen, öffnen Sie Visual Studio und erstellen Sie eine **Visual C++** \> **Windows Desktop** \> **Windows-Desktop-Anwendung** Projekt.

Optional können Sie installieren die [C++ / WinRT Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix). Weitere Informationen finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Set-Projekteigenschaften

Wechseln Sie zu den Projekteigenschaften **allgemeine** \> **Windows SDK-Version**, und wählen Sie **alle Konfigurationen** und **alle Plattformen**. Sicherstellen, dass **Windows SDK-Version** nastaven NA hodnotu 10.0.17134.0 (Windows 10, Version 1803) oder höher.

Bestätigen Sie, dass Sie nicht betroffen sind [Warum nicht Mein neue Projekt kompiliert?](/windows/uwp/cpp-and-winrt-apis/faq).

Da C++ / WinRT verwendet Funktionen der C ++ 17 standard, legen Projekteigenschaft **C/C++-** > **Sprache** > **Standard der Sprache C++** auf *ISO C ++ 17-Standard (/ Std: c ++ 17)*.

### <a name="the-precompiled-header"></a>Der vorkompilierte header

Benennen Sie Ihre `stdafx.h` und `stdafx.cpp` zu `pch.h` und `pch.cpp`bzw. Festlegen von Projekteigenschaften **C/C++-** > **vorkompilierte Header** > **vorkompilierte Headerdatei** zu *"PCH.h"*.

Suchen und Ersetzen Sie alle `#include "stdafx.h"` mit `#include "pch.h"`.

In `pch.h`, umfassen `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Verknüpfen

C++ / WinRT-sprachprojektion hängt davon ab, bestimmte (nicht-Member)-Funktionen in der Windows-Runtime kostenlos und Einstiegspunkte, die erfordern, Verknüpfen mit der [WindowsApp.lib](/uwp/win32-and-com/win32-apis) Schirm-Bibliothek. Dieser Abschnitt beschreibt die drei Arten den Linker zu erfüllen.

Die erste Option besteht darin zu Ihrem Visual Studio hinzufügen Projekt alle c++ / WinRT-MSBuild-Eigenschaften und Ziele. Installieren Sie zu diesem Zweck die [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in Ihr Projekt. Öffnen des Projekts in Visual Studio, klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Navigieren Sie**geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **installieren** zum Installieren des Pakets für das betreffende Projekt.

Sie können auch Link-projekteinstellungen verwenden, um eine explizite Verknüpfung `WindowsApp.lib`. Alternativ können Sie dies tun, im Quellcode (in `pch.h`, z. B.) wie folgt aus.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Sie können jetzt kompilieren und verknüpfen und Hinzufügen von C++ / WinRT-Code zu Ihrem Projekt (z. B. der Code in die [ein c++ / WinRT-Schnellstart-](#a-cwinrt-quick-start) Abschnitt oben)

## <a name="important-apis"></a>Wichtige APIs
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items-Eigenschaft](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [WinRT::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [WinRT::HRESULT-Error-Struktur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Fehlerbehandlung bei C++/WinRT](error-handling.md)
* [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md)
* [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md)
* [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)
* [Zeichenfolgenbehandlung in C++ / WinRT](strings.md)
