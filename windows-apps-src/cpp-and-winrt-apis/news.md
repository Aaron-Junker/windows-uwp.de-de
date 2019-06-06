---
description: Neuigkeiten und Änderungen von C++/WinRT.
title: Neuigkeiten in C++ / WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, Projektion, News, was die neue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 11249335f9d29d37bb0824fa779d3ae151c74799
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721653"
---
# <a name="whats-new-in-cwinrt"></a>Neuigkeiten in C++ / WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>Neuigkeiten und Änderungen in C++WinRT 2.0

Weitere Informationen zu den [ C++WinRT Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix), [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), und die die `cppwinrt.exe` Tool&mdash;umfasst auch das Abrufen und installieren Sie sie&mdash;finden Sie unter [Visual Studio-Unterstützung für C++"/ WinRT", XAML, die VSIX-Erweiterung und das NuGet-Paket](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Änderungen an der C++WinRT Visual Studio-Erweiterung (VSIX) Version 2.0

- Die Schnellansicht Debuggen unterstützt jetzt Visual Studio-2019; sowie wird weiterhin Visual Studio 2017 unterstützt.
- Zahlreiche Fehlerkorrekturen es wurden vorgenommen.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Änderungen an der Microsoft.Windows.CppWinRT NuGet-Paket für Version 2.0

- Die `cppwinrt.exe` Tool ist nun im Microsoft.Windows.CppWinRT NuGet-Paket enthalten, und das Tool generiert Platfom Projektion-Header für jedes Projekt bei Bedarf. Daher die `cppwinrt.exe` Tool wird nicht mehr auf dem Windows SDK abhängt (obwohl das Tool mit dem SDK aus Kompatibilitätsgründen weiterhin enthalten ist).
- `cppwinrt.exe` generiert jetzt Projektion Header unter jeder Plattform/konfigurationsspezifischen Zwischenordner ($IntDir) um parallele Builds zu ermöglichen.
- Die C++"/ WinRT" Build-Unterstützung (Eigenschaften/Ziele) nun umfassend dokumentiert, für den Fall, dass Sie Ihre Projektdateien manuell anpassen möchten. Finden Sie unter [Microsoft.Windows.CppWinRT NuGet-Paket](https://github.com/Microsoft/xlang/blob/master/src/package/cppwinrt/nuget/readme.md).
- Zahlreiche Fehlerkorrekturen es wurden vorgenommen.

### <a name="changes-to-cwinrt-for-version-20"></a>Änderungen an C++"/ WinRT", Version 2.0

#### <a name="open-source"></a>Open-source

Die `cppwinrt.exe` Tool nimmt Ihnen einen Windows-Runtime-Metadaten (`.winmd`) Datei aus, und generiert daraus einen Standard-Header-basierte C++ Bibliothek, die *Projekte* in den Metadaten beschriebenen APIs. Auf diese Weise können Sie diese APIs aus Nutzen Ihrer C++Code für "/ WinRT".

Dieses Tool ist nun vollständig open Source-Projekt, auf GitHub verfügbar. Besuchen Sie [Microsoft\/Xlang](https://github.com/Microsoft/xlang), und klicken Sie dann im **Src** > **Tool** > **Cppwinrt**.

#### <a name="xlang-libraries"></a>XLANG-Bibliotheken

Vollständig portierbare nur-Header Bibliothek (für die Analyse der ECMA-335-Metadaten-Formats verwendet, die von der Windows-Runtime) bildet die Grundlage für alle Windows-Runtime und XLANG-Tools in Zukunft. Vor allem wir auch mussten die `cppwinrt.exe` tool von Grund auf Sie mithilfe der Xlang-Bibliotheken. Dies bietet viel genauere Metadatenabfragen, lösen ein paar langfristige Probleme mit der C++sprachprojektion für "/ WinRT".

#### <a name="fewer-dependencies"></a>Weniger Abhängigkeiten

Aufgrund der XLANG-Metadaten-Leser die `cppwinrt.exe` Tool selbst weist weniger Abhängigkeiten. Dies macht es viel flexibler, sowie in anderen Szenarien verwendet werden wird&mdash;vor allem in eingeschränkten Umgebungen erstellen. Vor allem nicht mehr das Prinzip basiert auf `RoMetadata.dll`.
 
Hierbei handelt es sich um die Abhängigkeiten für `cppwinrt.exe` 2.0.
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

Im Gegensatz hierzu diese Abhängigkeiten wird die `cppwinrt.exe` 1.0 hat.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Die Windows-Runtime `noexcept` Attribut

Die Windows-Runtime hat ein neues `[noexcept]` -Attribut, das Sie verwenden, um Ihre Methoden und Eigenschaften in ergänzen [MIDL 3.0](/uwp/midl-3/predefined-attributes). Das Vorhandensein des Attributs gibt an, für die Unterstützung von Tools, die Ihre Implementierung keine Ausnahme auslöst (noch zurückgeben ein fehlerhaftes HRESULT). Dadurch wird die Sprache per sprachprojektion, optimieren Code generieren, indem Sie den Verwaltungsaufwand für die Behandlung von Ausnahmen, die erforderlich sind, um die anwendungsbinärschnittstelle (ABI) Ruft Anwendung unterstützen, die ausfallen können.

C++/ WinRT nutzt dies, so dass C++ `noexcept` Implementierungen der verwenden und Erstellen von Code. Wenn Sie über API-Methoden oder Eigenschaften, Fail-frei, und Sie sich darum Codegröße Gedanken sind, können Sie dieses Attribut untersuchen.

#### <a name="optimized-code-generation"></a>Optimierte codegenerierung

C++/ WinRT generiert jetzt sogar noch effizienter C++ Quellcode (Hintergrund) so, die die C++ Compiler kann den kleinsten und effizientesten Binärcode möglich erzeugt. Viele der Verbesserungen zugeschnitten Senkung der Kosten der Behandlung von Ausnahmen durch das Vermeiden der unnötigen Entladeinformationen. Binärdateien, mit denen große Mengen von C++/WinRT-Code wird etwa eine 4 % Verringerung der Codegröße. Der Code ist auch eine effizientere (schneller ausgeführt) aufgrund der geringeren anweisungsanzahl.

Diese Verbesserungen basieren auf eine neue Interop-Funktion, die auch für Sie verfügbar ist. Alle der C++/WinRT-Typen, die Besitzer sind include einen Konstruktor für übernehmen den Besitz direkt, vermeiden den vorherigen Ansatz mit zwei Schritten.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Optimierte Ausnahmebehandlung (EH)-Generierung von code

Diese Änderung ergänzt die Arbeit, die von Microsoft ausgeführt wurde C++ Optimizer-Team, um die Kosten für die Behandlung von Ausnahmen zu verringern. Wenn Sie binäre Anwendungsschnittstellen (ABIs) (z. B. COM) stark in Ihrem Code verwenden, klicken Sie dann bemerken Sie viel Code, der das befolgen dieses Musters.

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/ WinRT selbst generiert dieses Muster für jede API, die implementiert wird. Mit Tausenden von API-Funktionen kann hier Optimierung erheblich sein. In der Vergangenheit der Optimierer wäre nicht zu erkennen, dass die catch-Blöcken sind identisch, sodass sie viel Code für jede ABI dupliziert wurde (die wiederum zu dem glauben, dass große Binärdateien mithilfe von Ausnahmen im Systemcode generiert werden. die beigetragen haben). Von Visual Studio-2019, jedoch die C++ Compiler fasst alle Funclets abfangen und speichert nur diejenigen, die eindeutig sind. Das Ergebnis ist eine weitere und insgesamt 18 % Reduzierung Codegröße für Binärdateien, die sich stark auf die dieses Muster stützen. Nicht nur ist ausgeführter EH-Code jetzt effizienter als das Verwenden von Rückgabecodes, sondern auch Informationen zu größeren Binärdateien wird nun der Vergangenheit.

#### <a name="incremental-build-improvements"></a>Inkrementeller Build-Verbesserungen

Die `cppwinrt.exe` Tool vergleicht nun die Ausgabe einer generierten Header/Quelldatei für den Inhalt einer vorhandenen Datei auf dem Datenträger, und nur schreibt die Datei, wenn die Datei tatsächlich geändert wurde. Dies spart viel Zeit mit Datenträger-e/a und wird sichergestellt, dass die Dateien nicht als "geändert" wurden von angesehen werden die C++ Compiler. Das Ergebnis ist die Neukompilierung vermieden oder reduziert werden, in vielen Fällen.

#### <a name="generic-interfaces-are-now-all-generated"></a>Generische Schnittstellen sind jetzt alle generierten

Aufgrund der XLANG-Metadaten-Leser C++/WinRT generiert jetzt alle parametrisierte oder generische Schnittstellen aus den Metadaten. Schnittstellen wie [Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_) werden jetzt aus den Metadaten generiert, sondern handschriftlich `winrt/base.h`. Das Ergebnis ist, die Größe des `winrt/base.h` wurde "Ausschneide" in der Mitte und Optimierungen werden generiert, rechten Maustaste in den Code (die Sie mit dem Ansatz nachrichtenschleifenimplementierung schwierig war).

> [!IMPORTANT]
> Schnittstellen wie z. B. in dem Beispiel wird nun angezeigt, in der Kopfzeile des entsprechenden Namespace, anstatt in `winrt/base.h`. Wenn Sie nicht bereits geschehen, müssen Sie also den entsprechenden Namespace-Header enthalten, um die Schnittstelle zu verwenden.

#### <a name="component-optimizations"></a>Komponente-Optimierungen

Dieses Update bietet Unterstützung für mehrere zusätzliche teilnehmen Optimierungen für C++"/ WinRT", in den folgenden Abschnitten beschrieben. Da diese Optimierungen Änderungen beschädigt werden (die möglicherweise kleinere Änderungen vorgenommen werden unterstützt), müssen Sie eingeschaltet werden explizit mit der `cppwinrt.exe` des Tools `-opt` Flag.

Ein neues Projekt (aus einer Projektvorlage) verwenden `-opt` standardmäßig.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Einheitliche und das direkte Implementierung Zugriff

Diese zwei Optimierungen können Ihre Komponente direkten Zugriff auf seine eigene Implementierungstypen, selbst wenn sie nur die projizierten Typen verwendet. Besteht keine Notwendigkeit, verwenden Sie [ **stellen**](/uwp/cpp-ref-for-winrt/make), [ **Make_self**](/uwp/cpp-ref-for-winrt/make-self), noch [ **Get_self** ](/uwp/cpp-ref-for-winrt/get-self) , wenn Sie einfach die öffentliche API-Oberfläche verwenden möchten. Direkte Aufrufe der Implementierung Ihre Aufrufe kompiliert werden, und diese können sogar vollständig inline gesetzt sein.

##### <a name="type-erased-factories"></a>Typlöschung Factorys

Diese Optimierung vermeidet die #include Abhängigkeiten in `module.g.cpp` , damit er nicht neu kompiliert werden muss jedes Mal, wenn jeder einzelne Implementierung-Klasse erfolgt, ändern. Das Ergebnis ist die verbesserte Buildleistung.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>Intelligenter und effizienter `module.g.cpp` bei großen Projekten mit mehreren Bibliotheken

Die `module.g.cpp` -Datei enthält nun auch zwei zusätzliche zusammensetzbare-Hilfen, mit dem Namen **Winrt_can_unload_now**, und **Winrt_get_activation_factory**. Diese sind für größere Projekte konzipiert, in denen eine DLL-Datei aus einer Anzahl von Bibliotheken, jeweils eine eigene Runtime-Klassen ist. In diesem Fall müssen Sie manuell zusammenfügen der DLLs **"dllgetactivationfactory"** und **"DllCanUnloadNow"** . Diese Hilfsprogramme erleichtern viel einfacher für Sie, dass hierzu die Vermeidung vermeidbarer Ursprung Fehler. Die `cppwinrt.exe` des Tools `-lib` Flag kann ebenfalls verwendet werden, jede einzelne Lib eigene Präambel gewähren (statt `winrt_xxx`), damit Funktionen für jede Bibliothek einzeln mit dem Namen, und daher eindeutig kombiniert werden können.

#### <a name="coroutine-support"></a>Coroutineunterstützung

Coroutineunterstützung ist automatisch enthalten. Die Unterstützung zuvor befanden, an mehreren Orten, die wir sind der Ansicht wurde zu beschränken. Und dann für den v2. 0, vorübergehend eine `winrt/coroutine.h` Headerdatei war erforderlich, aber, die nicht mehr benötigt wird. Da die Windows-Runtime-Async-Schnittstellen generiert werden, statt handschriftlich, sie jetzt befinden sich im `winrt/Windows.Foundation.h`. Abgesehen davon, dass besser verwaltet und unterstützt werden kann, die dieser Coroutine kann z.B. [ **Resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) müssen nicht mehr an das Ende eines bestimmten Namespace-Headers übernommen werden. Stattdessen können sie ihre Abhängigkeiten auf natürliche Weise mehr enthalten. Dadurch weitere **Resume_foreground** unterstützen nicht nur auf Fortsetzen einer angegebenen [ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), können jetzt auch unterstützt jedoch auf Fortsetzen einer angegebenen [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Bisher konnte nur eine unterstützt werden; aber nicht beides, da die Definition nur in einem einzelnen Namespace befinden kann.

Hier ist ein Beispiel für die **DispatcherQueue** unterstützen.

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

Die Coroutine Hilfsprogramme sind jetzt auch mit versehen `[[nodiscard]]`können. Dadurch wird die benutzerfreundlichkeit zu verbessern. Wenn Sie vergessen, (oder gar nicht klar war, müssen Sie) `co_await` diese, damit sie funktionieren und aufgrund von `[[nodiscard]]`, diese Fehler werden jetzt eine compilerwarnung erzeugen.

#### <a name="help-with-diagnosing-stack-allocations"></a>Hilfe bei der Diagnose von stapelreservierungen

Da die Klassennamen projizierten und Implementierung (standardmäßig) gleich sind und unterscheiden sich nur durch Namespace, ist es möglich, eine für die anderen verwechseln, und versehentlich eine Implementierung auf dem Stapel erstellen, statt mit der [ **stellen** ](/uwp/cpp-ref-for-winrt/make) Familie der Hilfsprogramme. Dies kann schwierig, die in einigen Fällen zu diagnostizieren sein, da das Objekt zerstört werden kann, während ausstehende Verweise noch sind. Eine Assertion jetzt übernimmt dies, für Debugbuilds. Während die Assertion stapelzuordnung in einer Coroutine nicht erkennt, ist es dennoch hilfreich, in den meisten diese Fehler abfangen.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Verbesserte Capture-Hilfsprogramme und Variadic-Delegaten

Dieses Update behebt die Einschränkung durch die Capture-Hilfsprogramme, durch die Unterstützung von projizierten Typen ebenfalls. Dies erscheint, und mit den Windows-Runtime-interop-APIs, wenn sie einen projizierten Typ zurückgeben.

Dieses Update bietet außerdem Unterstützung für [ **Get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) und [ **Get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) beim Erstellen eines Delegaten Variadic (nicht - Windows-Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Unterstützung für verzögerte Löschung und sichere QI während der Zerstörung

Eine XAML-Anwendung erhalten selbst in schwierigkeiten aufgrund der Notwendigkeit zum Ausführen einer [ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) in einen Destruktor, um eine gewisse Implementierung für die Bereinigung nach oben oder unten in der Hierarchie aufzurufen. Allerdings, dass der Aufruf ein QI umfasst, nachdem der Verweiszähler des Objekts verfügt bereits über Null erreicht. Dieses Update unterstützt debouncing des Verweiszählers auf, um sicherzustellen, dass mehr als 0 (null), die sie nie wieder zugänglich gemacht werden kann; und dennoch QI für temporäre, die während der Zerstörung erforderlich ist. Dieses Verfahren ist in bestimmten XAML-Anwendungen/Steuerelementen unvermeidlich und C++/WinRT ist jetzt stabil.

Zerstörung kann zurückgestellt werden, durch die Bereitstellung eines statisches **Final_release** Funktion, und verschieben Sie den Besitz der **Unique_ptr** auf einen anderen Kontext.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

Im folgenden Beispiel wird einmal die **"MainPage"** freigegeben (zum letzten Mal) **Final_release** aufgerufen wird. Funktion benötigt fünf Sekunden warten (auf den Threadpool), und klicken Sie dann sie fortgesetzt wird, mithilfe der **Dispatcher** (wofür QI/AddRef/Release funktioniert). Löscht dann die **Unique_ptr**, wodurch die **"MainPage"** Destruktor tatsächlich aufgerufen. Auch hier **DataContext** aufgerufen wird, erfordert ein QI für **IFrameworkElement**. Natürlich, Sie müssen keine Implementieren Ihrer **Final_release** als Coroutine. Aber, die funktioniert, und es ist alles sehr einfach, Zerstörung in einem anderen Thread zu verschieben.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Verbesserte Unterstützung für die einzelnen schnittstellenvererbung COM-Stil

Sowie für Windows-Runtime-Programmierung C++/WinRT auch erstellen und verwenden nur für COM-APIs verwendet wird. Dieses Update macht es möglich, einen COM-Server, in denen eine Hierarchie der Benutzeroberfläche vorhanden zu implementieren. Dies ist nicht für die Windows-Runtime erforderlich; Dies ist jedoch erforderlich für einige COM-Implementierungen.

#### <a name="correct-handling-of-out-params"></a>Korrekte Handhabung von `out` Params

Es kann schwierig, die Arbeit mit sein `out` Params; insbesondere Windows-Runtime-Arrays. Mit diesem Update C++/WinRT ist wesentlich mehr von robust und widerstandsfähig gegen Fehler, wenn es darum geht, `out` Params und Arrays; ob diese Parameter eingehen, über eine sprachprojektion oder von einem COM-Entwickler, die die unformatierte ABI verwendet, und wer besteht darin, der Fehler nicht konsistent zu Variablen initialisieren. In beiden Fällen C++/WinRT ist jetzt richtig, wenn es darum geht, die an projizierten Typen der ABI (indem Sie sich merken, um alle Ressourcen freizugeben), und wenn es darum geht, unwiderrufliche aktivieren bzw. deaktivieren out-Parameter, die über die ABI eingehen.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Ereignisse verarbeiten jetzt ungültige Token zuverlässig

Die [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) Implementierung nun verarbeitet ordnungsgemäß den Fall, in denen die **entfernen** Methode wird aufgerufen, mit einem ungültigen token-Wert (ein Wert, der nicht vorhanden ist die Array).

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>Coroutine "lokal" werden jetzt zerstört, bevor die Coroutine zurückgibt.

Die traditionelle Methode implementieren eine Coroutine können lokale Variablen innerhalb der Coroutine zerstört werden *nach* Abschluss der Coroutine gibt / (anstatt vor der angehalten). Die Wiederaufnahme der alle Waiter ist jetzt angehalten, um dieses Problem zu vermeiden und andere Vorteile entstehen zurückgestellt.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Neuigkeiten und Änderungen in Windows SDK, Version 10.0.17763.0 (Windows 10, Version 1809)

Die folgende Tabelle enthält Neuigkeiten und Änderungen für die C++"/ WinRT" im Windows SDK, Version 10.0.17763.0 (Windows 10, Version 1809).

| Neue oder geänderte Funktion | Weitere Informationen |
| - | - |
| **Wichtige Änderung**. Für das Kompilieren, C++ / WinRT nicht den Header aus dem Windows SDK abhängig. | Finden Sie unter [isoliert von Windows SDK-Headerdateien](#isolation-from-windows-sdk-header-files)weiter unten. |
| Das Visual Studio-Projektformat System hat sich geändert. | Finden Sie unter [wie neu ausrichten, Ihrem C + c++ / WinRT-Projekt auf eine neuere Version des Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)weiter unten. |
| Es sind neue Funktionen und Basisklassen, können Sie ein Auflistungsobjekt an eine Windows-Runtime-Funktion übergeben oder eine eigene Auflistung – Eigenschaften und Auflistungstypen implementieren. | Finden Sie unter [Sammlungen mit C++ / WinRT](collections.md). |
| Können Sie die [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) Markuperweiterung mit Ihr C++ / WinRT-Runtime-Klassen. | Weitere Informationen und Codebeispiele, finden Sie unter [Übersicht über die Datenbindung](/windows/uwp/data-binding/data-binding-quickstart). |
| Unterstützung für den Abbruch einer Coroutine können Sie einen abbruchsrückruf zu registrieren. | Weitere Informationen und Codebeispiele, finden Sie unter [Abbrechen, eine asynchrone Operation und Abbruch-Rückrufe](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Wenn Sie einen Delegaten verweist auf eine Memberfunktion zu erstellen, können Sie einrichten, einen starken oder einen schwachen Verweis auf das aktuelle Objekt (anstelle von einem unformatierten *dies* Zeiger) an dem Punkt, in dem der Handler registriert ist. | Weitere Informationen und Codebeispiele, finden Sie unter den **bei Verwendung eine Memberfunktion als Delegat** Unterabschnitt im Abschnitt [problemlos den Zugriff auf die *dies* Zeiger durch einen Delegaten zur Verarbeitung von Ereignissen](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Fehler wurden behoben, die durch verbesserte Visual Studio-Konformität mit dem C++-standard aufgedeckt wurden. Die LLVM und Clang-toolkette ist zudem besser genutzt, um das Überprüfen von C++ / WinRT Übereinstimmung mit Standards. | Nicht mehr auftritt und das Problem, die in beschriebenen [Warum nicht Mein neue Projekt kompiliert? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher), und von SDK Version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Andere Änderungen.

- **Wichtige Änderung**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) gibt nun `void*` anstelle von `HSTRING`. Sie können `static_cast<HSTRING>(get_abi(my_hstring));` um ein HSTRING zu erhalten.
- **Wichtige Änderung**. [**WinRT::put_abi(WinRT::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) gibt nun `void**` anstelle von `HSTRING*`. Sie können `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` um ein HSTRING * zu erhalten.
- **Wichtige Änderung**. HRESULT ist jetzt als projiziert **winrt::hresult**. Wenn Sie ein HRESULT (um die typüberprüfung oder zur Unterstützung von typmerkmalen) benötigen, können Sie `static_cast` eine **winrt::hresult**. Andernfalls **winrt::hresult** in HRESULT konvertiert werden, solange Sie einschließen `unknwn.h` bevor Sie C++ einfügen c++ / WinRT-Header.
- **Wichtige Änderung**. GUID ist jetzt als projiziert **winrt::guid**. Für APIs, die Sie implementieren, müssen Sie verwenden **winrt::guid** für GUID-Parameter. Andernfalls **winrt::guid** auf GUID konvertiert werden, solange Sie einschließen `unknwn.h` bevor Sie eine einfügen C++"/ WinRT"-Header.
- **Wichtige Änderung**. Die [ **winrt::handle_type Konstruktor** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) hat dazu explizite (es ist jetzt leicht mit falschen Programmieraufwand) verstärkt wurden. Wenn Sie eine unformatierte Handlewert zuweisen möchten, rufen Sie die [ **handle_type::attach Funktion** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) stattdessen.
- **Wichtige Änderung**. Die Signaturen der **WINRT_CanUnloadNow** und **WINRT_GetActivationFactory** geändert haben. Sie darf nicht alle diese Funktionen deklarieren. Stattdessen enthalten `winrt/base.h` (der automatisch enthalten ist, wenn Sie C++ einschließen / WinRT-Windows-Namespace-Header-Dateien), die die Deklarationen der diese Funktionen umfassen.
- Für die [ **winrt::clock Struktur**](/uwp/cpp-ref-for-winrt/clock), **From_FILETIME/To_FILETIME** sind veraltet, zugunsten des **From_file_time/To_file_time**.
- APIs, die erwarten, dass **IBuffer** Parameter wurden vereinfacht. Obwohl die meisten APIs, Sammlungen oder Arrays bevorzugen, genügend APIs abhängig **IBuffer** , dass es einfacher, solche APIs von C++ verwendet werden musste. Dieses Update bietet direkten Zugriff auf die zugrunde liegenden Daten eine **IBuffer** Implementierung, über die gleichen Daten verwendete Benennungskonvention die C++-Standardbibliothek-Container. Dies verhindert auch die Kollision mit Metadatennamen, die konventionell mit einem Großbuchstaben beginnen.
- Verbesserte codegenerierung: verschiedene Verbesserungen für die Größe des Codes zu reduzieren inlining verbessern und Optimieren Sie die Factory im Cache.
- Entfernt die unnötige Rekursion. Wenn die Befehlszeile verweist, in einen Ordner, anstatt zu einem bestimmten `.winmd`, `cppwinrt.exe` Tool nicht mehr sucht rekursiv nach `.winmd` Dateien. Die `cppwinrt.exe` Tool jetzt auch verarbeitet Duplikate mehr auf intelligente Weise, wodurch Benutzerfehler stabiler und zu schlecht formatiert `.winmd` Dateien.
- Mit verstärkter Sicherheit intelligente Zeiger. Früher Verschieben – der Fehler beim Widerrufen der zugeordneten Ereignis Rücknahmeschlüssel kein neuer Wert zugewiesen. Dies hat ein Problem entdecken, intelligente zeigerklassen selbstzuweisung zuverlässig gehandhabt waren nicht; als Stamm der [ **winrt::com_ptr Struktur Vorlage**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** behoben wurde, und das Ereignis der zugeordneten Rücknahmeschlüssel behoben, behandeln die move-Semantik richtig, damit sie bei der Zuweisung aufzuheben.

> [!IMPORTANT]
> Wichtige Änderungen vorgenommen wurden, um die [C++ / WinRT Visual Studio-Erweiterung (VSIX)](https://aka.ms/cppwinrt/vsix), sowohl in Version 1.0.181002.2, und später in Version 1.0.190128.4. Details zu dieser Änderungen und deren Auswirkung auf Ihren vorhandenen Projekten [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) und [frühere Versionen der Erweiterung VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolation von Windows SDK-Headerdateien

Dies ist möglicherweise eine wichtige Änderung für Ihren Code.

Für das Kompilieren, C++ / WinRT hängt nicht mehr Headerdateien aus dem Windows SDK. Headerdateien der C-Laufzeitbibliothek (CRT) und die C++ Standardvorlagenbibliothek (STL), nicht auch alle Windows-SDK-Header enthalten. Und verbessert die Einhaltung von Standards, verhindert die unbeabsichtigte Abhängigkeiten, erheblich reduziert die Anzahl von Makros, die Sie zum Schutz vor.

Diese Unabhängigkeit bedeutet dass c++ / WinRT ist jetzt besser portierbar und Standards entsprechen, und sie die Möglichkeit, dass die Datei eine-Cross-Compiler und Cross-Platform-Bibliothek gefördert. Es bedeutet auch, dass der C / c++ / WinRT-Header nicht negativ auf die betroffenen Makros sind.

Wenn zuvor für C++ bleibt / WinRT, um alle Windows-Header in Ihrem Projekt einzuschließen, müssen Sie jetzt selbst enthalten. Es ist, in jedem Fall immer empfiehlt es sich um explizit die Header, von denen Sie abhängen, und lassen sie nicht in eine andere Bibliothek, um diese für Sie implizit enthalten.

Derzeit sind die einzigen Ausnahmen von Windows SDK-Header-Datei Isolation für systeminterne Funktionen und numerische Werte. Es gibt keine bekannten Probleme mit diesen letzten verbleibenden Abhängigkeiten.

In Ihrem Projekt können Sie Interoperabilität mit dem Windows SDK-Header erneut aktivieren, wenn Sie möchten. Sie können z. B. eine COM-Schnittstelle implementieren möchten (als Stamm [ **IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)). Für dieses Beispiel umfassen `unknwn.h` bevor Sie C++ einfügen c++ / WinRT-Header. Dies bewirkt, dass dies der Fall ist C++ / WinRT-Basisbibliothek zahlreichen Hooks zur Unterstützung von klassischen COM-Schnittstellen zu aktivieren. Ein Codebeispiel finden Sie unter [Autor COM-Komponenten mit C++ / WinRT](author-coclasses.md). Auf ähnliche Weise enthalten Sie explizit alle anderen Windows SDK-Header, die deklarieren Typen und/oder Funktionen, die Sie aufrufen möchten.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Wie Sie Ihre C + neu zuweisen c++ / WinRT-Projekt auf eine neuere Version des Windows SDK

Die Methode für die neuzuweisung des Projekts, die wahrscheinlich zu den wenigsten Compiler- und Linkeroptionen Problem ist, ist auch der aufwändigste. Diese Methode umfasst das Erstellen eines neuen Projekts (für die Windows SDK-Version Ihrer Wahl), und klicken Sie dann Kopieren von Dateien über dem neuen Projekt auf Ihrem alten. Fallen in Abschnitten von Ihrem alten `.vcxproj` und `.vcxproj.filters` Dateien, die Sie soeben können über zu kopieren, speichern Sie das Hinzufügen von Dateien in Visual Studio.

Es gibt jedoch zwei weitere Methoden, die Ihr Projekt in Visual Studio neu zuweisen.

- Wechseln Sie zu den Projekteigenschaften **allgemeine** \> **Windows SDK-Version**, und wählen Sie **alle Konfigurationen** und **alle Plattformen**. Legen Sie **Windows SDK-Version** auf die Version, die Sie abzielen möchten.
- In **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektknoten, klicken Sie auf **Projekte neu zuweisen**, wählen Sie die Versionen, die Sie verwenden möchten, als Ziel aus, und klicken Sie dann auf **OK**.

Wenn Sie nach der Verwendung einer dieser beiden Methoden Compiler oder Linker-Fehler auftreten, können Sie versuchen, die Projektmappe bereinigen (**erstellen** > **Projektmappe bereinigen** bzw. löschen Sie manuell alle temporären Ordner und Dateien) vor dem Versuch, erneut zu erstellen.

Wenn der C++-Compiler erzeugt "*Fehler C2039: "IUnknown": ist kein Mitglied der "\`globalen Namespace''* ", fügen Sie dann `#include <unknwn.h>` am Anfang Ihrer `pch.h` Datei (bevor Sie C++ einfügen c++ / WinRT-Header).

Sie müssen möglicherweise auch hinzufügen `#include <hstring.h>` danach.

Wenn der C++-Linker führt "*Fehler LNK2019: nicht aufgelöstes externes Symbol _WINRT_CanUnloadNow@0 verwiesen wird, in der Funktion _VSDesignerCanUnloadNow@0* ", und klicken Sie dann, die durch das Hinzufügen zu beheben `#define _VSDESIGNER_DONT_LOAD_AS_DLL` auf Ihre `pch.h` Datei.
