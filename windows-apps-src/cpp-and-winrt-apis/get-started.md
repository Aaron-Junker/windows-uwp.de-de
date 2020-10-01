---
description: Damit Sie C++/WinRT schneller verwenden können, werden in diesem Thema einige einfache Codebeispiele vorgestellt.
title: Erste Schritte mit C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, uwp, Standard, C++, cpp, winrt, Projektion, Erste Schritte
ms.localizationpriority: medium
ms.openlocfilehash: f38269acd9f1d6e2e830b51b3fcfa3a9014f2d7e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219903"
---
# <a name="get-started-with-cwinrt"></a>Erste Schritte mit C++/WinRT

Um Sie bei der Verwendung von [C++/WinRT](./intro-to-using-cpp-with-winrt.md) schnell auf den neuesten Stand zu bringen, führt Sie dieses Thema durch ein einfaches Codebeispiel, das auf einem neuen **Windows Console Application (C++/WinRT)** -Projekt basiert. Dieses Thema zeigt außerdem das [Hinzufügen von C++/WinRT-Unterstützung zu einem Windows Desktop-Anwendungsprojekt](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Wir empfehlen zwar, dass Sie mit den neuesten Versionen von Visual Studio und dem Windows SDK entwickeln, doch wenn Sie Visual Studio 2017 (Version 15.8.0 oder höher) und als Zielversion Windows SDK Version 10.0.17134.0 (Windows 10, Version 1803) verwenden, tritt beim Kompilieren eines neu erstellten C++/WinRT-Projekts möglicherweise der Fehler „*Fehler C3861: 'from_abi': Bezeichner wurde nicht gefunden*“ zusammen mit anderen Fehlern auf, die von *base.h* verursacht werden. Die Lösung besteht darin, für das Projekt eine höhere (konformere) Version des Windows SDK zu verwenden oder die Projekteigenschaft **C/C++**  > **Sprache** > **Konformitätsmodus: Nein** festzulegen (auch wenn **/permissive-** in der Projekteigenschaft **C/C++**  > **Sprache** > **Befehlszeile** unter **Zusätzliche Optionen** angezeigt wird. Löschen Sie es dann).

## <a name="a-cwinrt-quick-start"></a>Ein Schnelleinstieg in C++/WinRT

> [!NOTE]
> Informationen zum Einrichten von Visual Studio für die C++/WinRT-Entwicklung&mdash;einschließlich Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen)&mdash; finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Erstellen Sie ein neues **Windows Console Application (C++/WinRT)** -Projekt.

Bearbeiten Sie `pch.h` und `main.cpp` folgendermaßen.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
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
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

Mit den Standardprojekteinstellungen kommen die enthaltenen Header aus dem Windows SDK, aus dem Ordner `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio bezieht diesen Pfad in sein *IncludePath*-Makro ein. Es gibt aber keine strenge Abhängigkeit vom Windows SDK, da Ihr Projekt (über das Tool `cppwinrt.exe`) die gleichen Header im Ordner *$(GeneratedFilesDir)* Ihres Projekts erstellt. Sie werden aus diesem Ordner geladen, wenn sie an anderer Stelle nicht gefunden werden oder Sie die Einstellungen des Projekts ändern.

Die Header enthalten Windows-APIs, die in C++/WinRT projiziert werden. Anders ausgedrückt: Für jeden einzelnen Windows-Typ definiert C++/WinRT ein C++-freundliches Äquivalent (wird als *projizierter Typ* bezeichnet). Ein projizierter Typ verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, befindet sich aber im C++/**winrt**-Namespace. Wenn Sie diese in Ihrem vorkompilierten Header platzieren, werden die inkrementellen Buildzeiten reduziert.

> [!IMPORTANT]
> Wann immer Sie einen Typ aus einem Windows-Namespace verwenden möchten, müssen Sie ein `#include` für die entsprechende C++/WinRT-Windows-Namespace-Headerdatei ausführen, wie oben gezeigt ein. Der *zugehörige* Header ist derjenige mit dem gleichen Namen wie der Namespace des Typs. Um z. B. die C++/WinRT-Projektion für die Laufzeitklasse [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) zu verwenden, schließen Sie den `winrt/Windows.Foundation.Collections.h`-Header ein.
> 
> Es ist üblich, dass ein C++/WinRT-Projektionsheader automatisch seine übergeordnete Namespace-Headerdatei einschließt. So schließt `winrt/Windows.Foundation.Collections.h` beispielsweise `winrt/Windows.Foundation.h` ein. Aber Sie sollten sich nicht auf dieses Verhalten verlassen, da es sich um ein Implementierungsdetail handelt, das mit der Zeit geändert werden kann. Sie müssen explizit alle benötigten Header einschließen.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Die `using namespace`-Anweisungen sind optional, aber praktisch. Das oben gezeigte Muster für diese Richtlinien (das unqualifiziertes Nachschlagen von Namen für alles im **Winrt**-Namespace ermöglicht) eignet sich, wenn Sie ein neues Projekt beginnen und C++/WinRT die einzige Sprachprojektion ist, die Sie innerhalb dieses Projekts verwenden. Wenn Sie andererseits C++/WinRT-Code mit [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) und/oder SDK-ABI-Code (Application Binary Interface) kombinieren (Sie entweder daraus portieren oder damit interagieren, für eines oder beide dieser Modelle), dann lesen Sie die Themen [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md), [Wechsel zu C++/WinRT von C++/CX](move-to-winrt-from-cx.md) und [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

Der Aufruf von **winrt::init_apartment** initialisiert den Thread in der Windows Runtime; standardmäßig in einem Multithread-Apartment. Der Aufruf initialisiert darüber hinaus auch COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Nehmen Sie eine Stapelzuordnung für zwei Objekte vor: Sie stellen den URI des Windows-Blogs und einen Syndication-Client dar. Wir erstellen den URI mit einem einfachen Wide-String-Literal (unter [Verarbeitung von Zeichenfolgen in C++/WinRT](strings.md) finden Sie weitere Möglichkeiten zum Arbeiten mit Zeichenfolgen).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) ist ein Beispiel für eine asynchrone Windows-Runtime-Funktion. Das Codebeispiel erhält von **RetrieveFeedAsync** ein Objekt für einen asynchronen Vorgang. Es ruft **get** für dieses Objekt auf, um den aufrufenden Thread zu blockieren und auf das Ergebnis zu warten (in diesem Fall einen Syndication-Feed). Weitere Informationen über Parallelität und Non-Blocking-Techniken finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) ist ein Bereich, der durch die Iteratoren definiert wird, die von den **begin**- und **end**-Funktionen (oder deren constant-, reverse- und constant-reverse-Varianten) zurückgegeben werden. Aus diesem Grund können Sie **Items** entweder mit einer bereichsbasierten `for`-Anweisung oder mit der Template-Funktion **std::for_each** auflisten. Immer, wenn Sie eine Iteration in einer Windows Runtime-Sammlung wie dieser ausführen, ist ein Einschluss mithilfe von `#include <winrt/Windows.Foundation.Collections.h>` erforderlich.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Der Code erhält dann den Titeltext des Feeds als [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)-Objekt (siehe [String-Verarbeitung in C++/WinRT](strings.md)). **hstring** wird dann mithilfe der **c_str**-Funktion ausgegeben, die das Muster wiedergibt, das für Zeichenfolgen der C++-Standardbibliothek verwendet wird.

Wie Sie sehen können, unterstützt C++/WinRT moderne, klassenähnliche C++ Ausdrücke wie `syndicationItem.Title().Text()`. Dies ist ein anderer und saubererer Programmierstil als die traditionelle COM-Programmierung. Du brauchst weder COM direkt zu initialisieren noch mit COM-Zeigern zu arbeiten.

Sie brauchen auch keine HRESULT-Rückgabecodes zu verarbeiten. Für einen natürlichen und modernen Programmierstil konvertiert C++/WinRT HRESULTs-Fehler in Ausnahmen, wie z.B. [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error). Weitere Informationen zur Fehlerbehandlung sowie Codebeispiele finden Sie unter [Fehlerbehandlung bei C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Ändern eines Windows Desktop-Anwendungsprojekts, um C++/WinRT-Unterstützung hinzuzufügen

In diesem Abschnitt erfahren Sie, wie Sie einem eventuell vorhandenen Windows Desktop-Anwendungsprojekt C++/WinRT-Unterstützung hinzufügen können. Wenn Sie nicht über ein Windows Desktop-Anwendungsprojekt verfügen, können Sie diese Schritte nachvollziehen, indem Sie im ersten Schritt eins erstellen. Öffnen Sie beispielsweise Visual Studio, und erstellen Sie ein **Visual C++** \> **Windows Desktop** \> **Windows-Desktopanwendung**-Projekt.

Optional können Sie die Erweiterung [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) und das NuGet-Paket installieren. Details dazu finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Festlegen von Projekteigenschaften

Wechseln Sie zu den Projekteigenschaften **Allgemein** \> **Windows SDK-Version**, und wählen Sie **Alle Konfigurationen** und **Alle Plattformen** aus. Achten Sie darauf, dass die **Windows SDK-Version** auf 10.0.17134.0 (Windows 10, Version 1803) oder höher festgelegt ist.

Bestätigen Sie, dass Sie nicht von [Warum wird mein neues Projekt nicht kompiliert?](./faq.md) betroffen sind.

Da C++/WinRT Features aus dem C++17-Standard verwendet, legen Sie die Projekteigenschaft **C/C++**  > **Sprache** > **C++-Sprachstandard** auf *ISO C++17 Standard (/std:c++17)* fest.

### <a name="the-precompiled-header"></a>Der vorkompilierte Header

Die Standardprojektvorlage erstellt für Sie einen vorkompilierten Header, der entweder `framework.h` oder `stdafx.h` benannt ist. Benennen Sie ihn in `pch.h` um. Wenn eine `stdafx.cpp`-Datei vorhanden ist, benennen Sie diese in `pch.cpp` um. Legen Sie die Projekteigenschaft **C/C++**  > **Vorkompilierte Header** > **Vorkompilierter Header** auf *Erstellen (/Yc)* und **Vorkompilierte Headerdatei** auf *pch.h* fest.

Suchen Sie alle `#include "framework.h"` (oder `#include "stdafx.h"`), und ersetzen Sie sie durch `#include "pch.h"`.

Schließen Sie `winrt/base.h` in `pch.h` ein.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Verknüpfen

Die C++/WinRT-Sprachprojektion hängt von bestimmten freien (nicht Member-) Funktionen der Windows Runtime und von Einstiegspunkten ab, die eine Verknüpfung mit der [WindowsApp.lib](/uwp/win32-and-com/win32-apis) „Umbrella“-Bibliothek erfordern. In diesem Abschnitt werden drei Verfahren beschrieben, wie die Verknüpfungsanforderung erfüllt werden kann.

Die erste Option besteht darin, Ihrem Visual Studio-Projekt alle C++/WinRT-MSBuild-Eigenschaften und -Ziele hinzuzufügen. Installieren Sie zu diesem Zweck das [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in Ihrem Projekt. Öffne das Projekt in Visual Studio, klicke auf **Projekt** \> **NuGet-Pakete verwalten**. \> **Durchsuchen**, geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **Installieren**, um das Paket für das Projekt zu installieren.

Sie können auch die Verknüpfungseinstellungen des Projekts verwenden, um `WindowsApp.lib` explizit zu verknüpfen. Oder Sie können das im Quellcode erledigen (z.B. in `pch.h`), wie hier gezeigt.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Jetzt können Sie kompilieren und verknüpfen und Ihrem Projekt C++/WinRT-Code hinzufügen (beispielsweise Code ähnlich dem im Abschnitt [Schnelleinstieg zu C++/WinRT](#a-cwinrt-quick-start) oben).

## <a name="the-three-main-scenarios-for-cwinrt"></a>Die drei wichtigsten Szenarien für C++/WinRT

Wenn Sie C++/WinRT verwenden und sich damit ebenso wie mit der restlichen Dokumentation hier vertraut machen, werden Sie wahrscheinlich feststellen, dass es drei Hauptszenarien gibt, die in den folgenden Abschnitten beschrieben werden.

### <a name="consuming-windows-runtime-apis-and-types"></a>Verarbeiten von Windows-Runtime-APIs und -Typen

Mit anderen Worten: Es geht um das *Verwenden* oder *Aufrufen* von APIs. Beispiele sind API-Aufrufe für die Kommunikation über Bluetooth, das Streamen und Präsentieren von Videos, die Integration in die Windows-Shell usw. C++/WinRT unterstützt diese Kategorie von Szenarien uneingeschränkt. Weitere Informationen finden Sie unter [Nutzen von APIs mit C++/WinRT](./consume-apis.md).

### <a name="authoring-windows-runtime-apis-and-types"></a>Erstellen von Windows-Runtime APIs und -Typen

Das heißt, das *Erstellen* von APIs und Typen. Beispielsweise werden die Arten von APIs erzeugt, die im Abschnitt oben beschrieben werden, die Grafik-APIs oder die Speicher- und Dateisystem-APIs, die Netzwerk-APIs usw. Weitere Informationen finden Sie unter [Erstellen von APIs mit C++/WinRT](./author-apis.md).

Das Erstellen von APIs mit C++/WinRT ist etwas komplizierter als deren Verwendung, da Sie die Form der API mit IDL definieren müssen, bevor Sie sie implementieren können. Eine exemplarische Vorgehensweise finden Sie unter [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](./binding-property.md).

### <a name="xaml-applications"></a>XAML-Anwendungen

Dieses Szenario dient dem Entwickeln von Anwendungen und Steuerelementen im XAML-UI-Framework. Das Arbeiten in einer XAML-Anwendung vereint Nutzung und Erstellung. Da XAML heute jedoch das vorherrschende UI-Framework unter Windows ist und dessen Auswirkungen auf die Windows-Runtime entsprechend zunehmen, verdient es eine eigene Kategorie von Szenarien.

Beachten Sie, dass XAML am besten für Programmiersprachen geeignet ist, die Reflektion unterstützen. In C++/WinRT müssen Sie manchmal einen etwas höheren Aufwand betreiben, um mit dem XAML-Framework zusammenzuarbeiten. Alle diese Fälle werden in der Dokumentation behandelt. Ein guter Ausgangspunkt ist [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](./binding-property.md) sowie [Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++/WinRT](./xaml-cust-ctrl.md).

## <a name="sample-apps-written-in-cwinrt"></a>Beispiel-Apps in C++/WinRT

Unter [Wo finde ich C++/WinRT-Beispiel-Apps?](./faq.md#where-can-i-find-cwinrt-sample-apps) finden Sie weitere Informationen.

## <a name="important-apis"></a>Wichtige APIs
* [SyndicationClient::RetrieveFeedAsync-Methode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items-Eigenschaft](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult_error-Struktur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Zugehörige Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Fehlerbehandlung bei C++/WinRT](error-handling.md)
* [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md)
* [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md)
* [Umstellen von C++/CX auf C++/WinRT](move-to-winrt-from-cx.md)
* [Verarbeitung von Zeichenfolgen in C++/WinRT](strings.md)