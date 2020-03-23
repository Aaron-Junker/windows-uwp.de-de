---
description: Neuigkeiten und Änderungen von C++/WinRT.
title: Neuerungen in C++/WinRT
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, neuerungen, neues
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 734544a1294c6a97e70afcbf7ce6b5efc13cf841
ms.sourcegitcommit: eb24481869d19704dd7bcf34e5d9f6a9be912670
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2020
ms.locfileid: "79448586"
---
# <a name="whats-new-in-cwinrt"></a>Neuerungen in C++/WinRT

Bei Veröffentlichung zukünftiger Versionen von C++/WinRT werden in diesem Thema jeweils die Neuerungen und Änderungen beschrieben.

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>Rollup aktueller Verbesserungen/Ergänzungen ab März 2020

### <a name="up-to-23-shorter-build-times"></a>Bis zu 23% kürzere Erstellungszeiten

Die C++/WinRT- und C++-Compilerteams haben zusammengearbeitet, um die Erstellungszeiten zu verkürzen. Wir haben uns mit der Compileranalyse beschäftigt, um herauszufinden, wie die Internals von C++/WinRT umstrukturiert werden können, um Mehraufwand bei der Kompilierzeit im C++-Compiler zu eliminieren, und wie der C++-Compiler selbst zur Handhabung der C++/WinRT-Bibliothek verbessert werden kann. C++/WinRT wurde für den Compiler optimiert, und der Compiler wurde für C++/WinRT optimiert.

Betrachten wir beispielsweise das Worst-Case-Szenario der Erstellung eines vorkompilierten Headers (PCH), der jeden einzelnen C++/WinRT-Projektionsnamespace-Header enthält.

| Version | PCH-Größe (in Byte) | Zeit (in Sekunden) |
| - | - | - |
| C++/WinRT von Juli, mit Visual C++ 16.3 | 3\.004.104.632 | 31 |
| Version 2.0.200316.3 von C++/WinRT, mit Visual C++ 16.5 | 2\.393.515.336 | 24 |

Eine Verringerung der Größe um 20 % und eine Verringerung der Erstellungszeit um 23 %.

### <a name="improved-msbuild-support"></a>Verbesserte MSBuild-Unterstützung

Wir haben viel Arbeit in eine verbesserte [MSBuild](/visualstudio/msbuild/msbuild?view=vs-2019)-Unterstützung für eine große Auswahl an unterschiedlichen Szenarien investiert.

### <a name="even-faster-factory-caching"></a>Noch schnelleres Factoryzwischenspeichern

Wir haben das Inlining des Factoryzwischenspeichers verbessert, um eine besseres Inlining des langsamsten Pfads und damit eine schnellere Ausführung zu erreichen.

Diese Verbesserung wirkt sich nicht auf die Codegröße aus (wie unten unter [Optimierte EH-Codegenerierung](#optimized-exception-handling-eh-code-generation) beschrieben). Wenn Ihre Anwendung oft C++-Ausnahmebehandlung verwendet, können Sie Ihre Binärdatei durch Verwendung der Option `/d2FH4` verkleinern, die in neuen Projekten, die mit Visual Studio 2019 16.3 und später erstellt werden, standardmäßig aktiviert ist.

### <a name="more-efficient-boxing"></a>Effizienteres Boxing

Bei Verwendung in einer XAML-Anwendung ist [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) jetzt effizienter (siehe [Boxing und Unboxing](/windows/uwp/cpp-and-winrt-apis/boxing)). Bei Anwendungen, in denen häufiges Boxing ausgeführt wird, werden Sie auch eine Verringerung der Codegröße feststellen.

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>Unterstützung für die Implementierung von COM-Schnittstellen, die IInspectable implementieren

Wenn Sie eine COM-Schnittstelle (Nicht-Windows-Runtime) implementieren müssen, die nur [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) implementiert, können Sie dies jetzt mit C++/WinRT erledigen. Siehe [COM-Schnittstellen, die IInspectable implementieren](https://github.com/microsoft/xlang/pull/603).

### <a name="module-locking-improvements"></a>Verbesserungen bei Modulsperren

Die Kontrolle über das Sperren von Modulen ermöglicht jetzt sowohl benutzerdefinierte Hostingszenarien als auch die komplette Beseitigung von Sperren auf Modulebene. Siehe [Verbesserungen bei Modulsperren](https://github.com/microsoft/xlang/pull/583).

### <a name="support-for-non-windows-runtime-error-information"></a>Unterstützung für Nicht-Windows-Runtime-Fehlerinformationen

Einige APIs (sogar einige Windows-Runtime-APIs) melden Fehler, ohne Windows-Runtime-Fehlerursprungs-APIs zu verwenden. In solchen Fällen greift C++/WinRT jetzt auf die Verwendung von COM-Fehlerinformationen zurück. Informationen finden Sie unter [C++/WinRT-Unterstützung für Nicht-WinRT-Fehlerinformationen](https://github.com/microsoft/xlang/pull/582).

### <a name="enable-c-module-support"></a>Aktivieren der C++-Modulunterstützung 

Die C++-Modulunterstützung ist zurück, aber nur in experimenteller Form. Das Feature ist im C++-Compiler derzeit noch nicht vollständig.

### <a name="more-efficient-coroutine-resumption"></a>Effizientere Coroutinenwiederaufnahme

C++/WinRT-Coroutinen funktionieren bereits gut, aber wir suchen weiterhin nach Möglichkeiten, sie zu verbessern. Weitere Informationen finden Sie unter [Improve scalability of coroutine resumption](https://github.com/microsoft/xlang/pull/546) (Verbessern der Skalierbarkeit von Coroutinenwiederaufnahme).

### <a name="new-when_all-and-when_any-async-helpers"></a>Neue asynchrone **when_all** und **when_any**-Hilfsprogramme

Die Helferfunktion **when_all** erzeugt ein [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)-Objekt, das abschließt, wenn alle Awaitable-Objekte abgeschlossen sind. Die Helferfunktion **when_any** erzeugt ein **IAsyncAction**-Objekt, das abschließt, wenn alle Awaitable-Objekte abgeschlossen sind. 

Weitere Informationen finden Sie unter [Add when_any async helper](https://github.com/microsoft/xlang/pull/520) (Hinzufügen der asynchronen Hilfsfunktion „when_any“) und [Add when_all async helper](https://github.com/microsoft/xlang/pull/516) (Hinzufügen der asynchronen Hilfsfunktion „when_all“).

### <a name="other-optimizations-and-additions"></a>Weitere Optimierungen und Ergänzungen

Außerdem wurden viele Fehler behoben sowie kleinere Optimierungen und Ergänzungen durchgeführt, einschließlich verschiedener Verbesserungen, um das Debuggen zu vereinfachen und Internals und Standardimplementierungen zu optimieren. Unter diesem Link finden Sie eine vollständige Liste: [https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed).

## <a name="news-and-changes-in-cwinrt-20"></a>Neuerungen und Änderungen in C++/WinRT 2.0

Weitere Informationen zur [Visual Studio-Erweiterung C++/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264), zum [NuGet-Paket Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) und dem `cppwinrt.exe`-Tool (auch dazu, wo Sie diese erhalten und wie Sie diese installieren können) finden Sie in der [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Änderungen in der C++/WinRT-Visual Studio-Erweiterung (VSIX) in Version 2.0

- Der Debugvisualizer unterstützt jetzt Visual Studio 2019 und auch weiterhin Visual Studio 2017.
- Es wurde mehrere Programmfehlerbehebungen vorgenommen.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Änderungen am NuGet-Paket Microsoft.Windows.CppWinRT für Version 2.0

- Das `cppwinrt.exe`-Tool ist jetzt im NuGet-Paket Microsoft.Windows.CppWinRT enthalten. Es generiert bedarfsbasiert Plattfomprojektionsheader für jedes Projekt. Das bedeutet, dass das `cppwinrt.exe`-Tool nicht mehr vom Windows SDK abhängig ist. Das Tool wird aber dennoch aus Kompatibilitätsgründen mit dem SDK ausgeliefert.
- `cppwinrt.exe` generiert jetzt Projektionsheader in jedem plattform- bzw. konfigurationsspezifischen Zwischenordner ($IntDir), um parallele Builds zu ermöglichen.
- Die C++/WinRT-Unterstützung (Eigenschaften/Ziele) ist jetzt vollständig dokumentiert, falls Sie Ihre Projektdateien manuell anpassen möchten. Weitere Informationen findest du in der [Infodatei](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) zum Microsoft.Windows.CppWinRT-NuGet-Paket.
- Es wurde mehrere Programmfehlerbehebungen vorgenommen.

### <a name="changes-to-cwinrt-for-version-20"></a>Änderungen an C++/WinRT in Version 2.0

#### <a name="open-source"></a>Open Source

Das `cppwinrt.exe`-Tool generiert aus einer Windows-Runtime-Metadatendatei (`.winmd`) eine headerdateibasierte C++-Standardbibliothek, die die in den Metadaten beschriebenen APIs *projiziert*. So können Sie diese APIs über C++/WinRT-Code verwenden.

Dieses Tool ist jetzt ein Open-Source-Projekt, das über GitHub verfügbar ist. Rufen Sie [microsoft\/xlang](https://github.com/Microsoft/xlang) auf, und navigieren Sie dort zu **src** > **tool** > **cppwinrt**.

#### <a name="xlang-libraries"></a>Xlang-Cloudbibliotheken

Die vollständig portierbare Bibliothek nur mit Headern (zum Analysieren des Metadatenformats ECMA-335, das von der Windows-Runtime verwendet wird) bildet ab jetzt die Grundlage aller Windows-Runtime- und xlang-Tools. Außerdem haben wir das `cppwinrt.exe`-Tool mit den xlang-Bibliotheken komplett neu geschrieben. So sind weitaus genauere Metadatenabfragen möglich, wodurch lange bestehende Probleme mit der C++/WinRT-Sprachprojektion behoben werden.

#### <a name="fewer-dependencies"></a>Weniger Abhängigkeiten

Dank des xlang-Metadatenreaders hat das `cppwinrt.exe`-Tool weniger Abhängigkeiten. Dadurch ist es deutlich flexibler und kann in mehr Szenarios eingesetzt werden – besonders in eingeschränkten Buildumgebungen. Insbesondere ist es nicht mehr von `RoMetadata.dll` abhängig.
 
Im Folgenden sind alle Abhängigkeiten für `cppwinrt.exe` 2.0 aufgelistet:
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

Alle diese DLLs sind nicht nur unter Windows 10 verfügbar, sondern bis zurück zu Windows 7 und sogar Windows Vista. Bei Bedarf kann der alte Buildserver unter Windows 7 nun `cppwinrt.exe` ausführen, um C++-Header für Ihr Projekt zu generieren. Mit etwas Aufwand können Sie sogar [C++/WinRT unter Windows 7 ausführen](https://github.com/kennykerr/win7), falls dies für Sie von Interesse ist.

Vergleichen Sie die Liste oben mit den folgenden Abhängigkeiten, die für `cppwinrt.exe` 1.0 bestehen.

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

#### <a name="the-windows-runtime-noexcept-attribute"></a>Das Windows-Runtime-Attribut `noexcept`

Die Windows-Runtime hat das neue Attribut `[noexcept]`, mit dem Sie Ihre Methoden und Eigenschaften in [MIDL 3.0](/uwp/midl-3/predefined-attributes) versehen können. Das Attribut gibt unterstützenden Tools an, dass Ihre Implementierung keine Ausnahme auslöst (und nicht HRESULT mit Fehlern zurückgibt). Dadurch können Sprachprojektionen die Codegenerierung optimieren, da der Mehraufwand für das Behandeln von Ausnahmen wegfällt, der für das Unterstützen von ABI-Aufrufen (Application Binary Interface, Binärschnittstelle) erforderlich ist, die möglicherweise mit Fehlern abgeschlossen werden.

Dies nutzt C++/WinRT, indem C++-`noexcept`-Implementierungen des nutzenden und erstellenden Codes erzeugt werden. Wenn Sie fehlerlose API-Methoden oder -Eigenschaften nutzen und an einer geringeren Codegröße interessiert sind, ist dieses Attribut von Interesse für Sie.

#### <a name="optimized-code-generation"></a>Optimierte Codegenerierung

C++/WinRT generiert jetzt (im Hintergrund) noch effizienteren C++-Quellcode, sodass der C++-Compiler den kürzesten und effizientesten Binärcode erzeugen kann. Viele der Änderungen sollen den Aufwand für das Behandeln von Ausnahmen reduzieren, indem unnötige Abwickelinformationen zu vermieden werden. Die Codegröße in Binärdateien, die große Mengen an C++/WinRT-Code enthalten, wird um ungefähr 4 % gesenkt. Der Code ist zudem effizienter (er wird sehr schnell ausgeführt), da er weniger Anweisungen enthält.

Diese Verbesserungen basieren auf einer neuen Interoperabilitätsfunktion, die Sie verwenden können. Alle C++/WinRT-Typen, die Ressourcenbesitzer sind, enthalten jetzt einen Konstruktor, der den Besitz direkt übernimmt. In der Vergangenheit war dazu ein zweischrittiger Prozess erforderlich.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Optimierte Codegenerierung bei der Ausnahmebehandlung

Diese Änderung ergänzt Maßnahmen, die vom Microsoft C++-Team ergriffen wurden, um den Aufwand für die Ausnahmebehandlung zu reduzieren. Wenn Sie viele ABIs in Ihrem Code verwenden (z. B. COM), werden Sie viel Code mit dem folgenden Muster sehen.

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

C++/WinRT generiert diese Muster für jede implementierte API. Wenn es Tausende API-Funktionen gibt, kann jede Optimierung einen deutlichen Unterschied machen. In der Vergangenheit hat die Optimierung nicht erkannt, dass alle diese catch-Blöcke identisch sind, weshalb viel Code in der Umgebung jeder Binärschnittstelle dupliziert wurde. Dies hat auch zur Auffassung beigetragen, das Verwenden von Ausnahmen im Systemcode führe zu großen Binärdateien. Ab Visual Studio 2019 faltet der C++-Compiler alle diese catch-Funclets und speichert nur diejenigen, die sich unterscheiden. Dadurch sinkt die Codegröße weiter um 18 % für Binärdateien, die dieses Muster viel verwenden. Der Code zur Ausnahmebehandlung ist jetzt nicht nur effizienter als Rückgabecode, auch die Größe von Binärdateien stellt jetzt kein Problem mehr dar.

#### <a name="incremental-build-improvements"></a>Inkrementelle Buildverbesserungen

Das `cppwinrt.exe`-Tool vergleicht jetzt die Ausgabe einer generierten Header-/Quelldatei mit den Inhalten jeder Datei auf dem Datenträger und gibt nur diejenigen aus, in denen Änderungen vorgenommen wurden. Dies reduziert die Dauer der Datenträger-E/A und stellt sicher, dass die Dateien vom Compiler nicht als „schmutzig“ angesehen werden. So wird in vielen Fällen ein erneutes Kompilieren vermieden oder die Häufigkeit zumindest gesenkt.

#### <a name="generic-interfaces-are-now-all-generated"></a>Generische Schnittstellen werden jetzt alle generiert

Wegen des xlang-Metadatenreaders generiert C++/WinRT jetzt alle parametrisierten oder generischen Schnittstellen aus Metadaten. Schnittstellen wie [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) werden jetzt aus Metadaten generiert statt von Hand in `winrt/base.h` geschrieben zu werden. Deshalb ist `winrt/base.h` nur noch halb so groß wie vorher und Optimierungen werden direkt im Code generiert (was mit dem manuellen Ansatz nicht so einfach war).

> [!IMPORTANT]
> Schnittstellen wie das hier genannte Beispiel werden in ihren jeweiligen Namespaceheadern und nicht in `winrt/base.h` angezeigt. Sie müssen also die entsprechenden Namespaceheader einbeziehen, um die Schnittstelle verwenden zu können (wenn Sie dies noch nicht getan haben).

#### <a name="component-optimizations"></a>Komponentenoptimierungen

Durch dieses Update wird Unterstützung für mehrere zusätzliche optionale Optimierungen für C++/WinRT hinzufügen, die in den folgenden Abschnitten beschrieben werden. Da diese Optimierungen Breaking Changes sind (Sie müssen für die Unterstützung eventuell kleinere Änderungen vornehmen), müssen Sie sie explizit aktivieren. Legen Sie in Visual Studio die Projekteigenschaft **Allgemeine Eigenschaften** > **C++/WinRT** > **Optimiert** auf *Ja* fest. Hierdurch wird der Projektdatei `<CppWinRTOptimized>true</CppWinRTOptimized>` hinzugefügt. Dies hat dieselbe Wirkung wie das Hinzufügen des Schalters `-opt[imize]` beim Aufruf von `cppwinrt.exe` über die Befehlszeile.

Ein neues Projekt (aus einer Projektvorlage) verwendet standardmäßig `-opt`.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Einheitliche Konstruktion und direkter Implementierungszugriff

Durch diese beiden Optimierungen erhält Ihre Komponente direkten Zugriff auf ihre eigenen Implementierungstypen, auch wenn sie nur die projizierten Typen verwendet. Wenn Sie nur die API-Oberfläche verwenden möchten, ist es nicht erforderlich, [**make**](/uwp/cpp-ref-for-winrt/make), [**make_self**](/uwp/cpp-ref-for-winrt/make-self) oder [**get_seöf**](/uwp/cpp-ref-for-winrt/get-self) zu verwenden. Ihre Aufrufe werden als direkte Aufrufe in der Implementierung kompiliert und werden möglicherweise komplett per Inlineersetzung optimiert.

Weitere Informationen und Codebeispiele finden Sie unter [Aktivieren von einheitlicher Konstruktion und direktem Implementierungszugriff](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

##### <a name="type-erased-factories"></a>Factorys ohne Typen

Durch diese Optimierung wird die Abhängigkeiten #include in `module.g.cpp` vermieden, sodass keine erneute Kompilierung erforderlich ist, wenn sich eine einzelne Implementierungsklasse ändert. Dies führt zu einer verbesserten Buildleistung.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>Die Datei `module.g.cpp` ist für große Projekte mit mehrere Bibliotheken jetzt intelligenter und effizienter

Die Datei `module.g.cpp` enthält jetzt zwei zusätzliche zusammensetzbare Hilfsobjekte, **winrt_can_unload_now** und **winrt_get_activation_factory**. Diese sind für größere Projekte konzipiert, in denen eine DLL aus mehreren Bibliotheken bestehen, die alle ihre eigenen Runtimeklassen haben. In dieser Situation müssen Sie die Objekte **DllGetActivationFactory** und **DllCanUnloadNow** der DLL manuell verknüpfen. Durch die Hilfsfunktionen ist dies sehr leicht durchzuführen, da zweifelhafte Fehler zum Ursprungspunkt vermieden werden. Das Flag `-lib` des `cppwinrt.exe`-Tools kann auch dazu verwendet werden, jeder einzelnen Bibliothek ihre eigene Präambel (statt `winrt_xxx`) zuzuweisen, damit die Funktionen jeder Bibliothek einzeln benannt und unmissverständlich kombiniert werden können.

#### <a name="coroutine-support"></a>Unterstützung für Coroutinen

Die Unterstützung für Coroutinen ist automatisch enthalten. In der Vergangenheit gab es die Unterstützung an mehreren Stellen, was recht einschränkend war. Zeitweise war für Version 2.0 eine `winrt/coroutine.h`-Headerdatei erforderlich. Dies ist jedoch jetzt nicht mehr der Fall. Da die asynchronen Schnittstellen der Windows-Runtime jetzt generiert und nicht mehr manuell geschrieben werden, befinden sie sich jetzt in `winrt/Windows.Foundation.h`. Sie sind jetzt nicht nur leichter verwaltbar und unterstützbar, sondern dies bedeutet auch, dass die Coroutinehilfsobjekte wie z. B. [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) nicht mehr ans Ende eines spezifischen Namespacesheaders angefügt werden müssen. Stattdessen können sie ihre Abhängigkeiten auf einfachere Art und Weise enthalten. So kann **resume_foreground** nicht nur das Fortsetzen bei einem angegebenen [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)-Objekt unterstützen, sondern auch das Fortsetzen bei einem [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)-Objekt. In der Vergangenheit konnte nur eines der beiden Objekte unterstützt werden, da sich die Definition nur in einem Namespace befinden konnte.

Hier sehen Sie ein Beispiel der Unterstützung für **DispatcherQueue**.

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

Die Coroutinehilfsobjekte können jetzt auch mit `[[nodiscard]]` versehen werden, sodass ihre Verwendbarkeit verbessert wird. Wenn Sie vergessen, `co_await` zu verwenden (oder nicht wissen, dass dies erforderlich ist), führen derartige Fehler aufgrund von `[[nodiscard]]` jetzt zu Compilerwarnungen.

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>Hilfe bei der Diagnose von direkten (Stapel-)Zuweisungen

Da die projizierten und Implementierungsklassennamen (standardmäßig) übereinstimmen und sich nur beim Namespace unterscheiden, kann es passieren, dass Sie sie verwechseln und versehentlich eine Implementierung auf dem Stapel statt mit den Hilfsfunktionen der [**make**](/uwp/cpp-ref-for-winrt/make)-Familie erstellen. Die Diagnose diese Problems ist nicht immer leicht, da das Objekt möglicherweise gelöscht wird, während ausstehende Verweise noch aktiv sind. Eine Assertion erkennt dies jetzt in Debugbuilds. Die Assertion erkennt zwar keine Stapelzuweisungen in einer Coroutine, ist aber dennoch hilfreich, die meisten dieser Fehler zu erkennen.

Weitere Informationen finden Sie unter [Diagnostizieren direkter Zuordnungen](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Verbesserte capture-Hilfsfunktionen und variadische Delegaten

In diesem Update werden Einschränkungen bei capture-Hilfsfunktionen behoben, indem auch projizierte Typen unterstützt werden. Dies tritt hin und wieder mit den Interoperabilitäts-APIs der Windows-Runtime beim erneuten Ausführen eines projizierten Typs auf.

Mit diesem Update wird zudem Unterstützung für [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) und [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) beim Erstellen variadischer (Nicht-Windows-Runtime-) Delegaten hinzugefügt.

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Unterstützung für zurückgestellte Löschung und sichere QueryInterface-Methode während der Löschung

Es ist nicht ungewöhnlich, dass der Destruktor eines Laufzeitklassenobjekts eine Methode aufruft, die die Verweisanzahl vorübergehend löscht. Wenn die Verweisanzahl auf 0 (null) zurückgesetzt wird, wird das Objekt ein zweites Mal gelöscht. In eine XAML-Anwendung müssen Sie möglicherweise eine [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))-Methode im Destruktor verwenden, um Bereinigungsimplementierungen in der Hierarchie aufzurufen. Die Verweisanzahl des Objekts hat jedoch bereits 0 (null) erreicht, sodass QueryInterface ebenfalls eine Erhöhung der Verweisanzahl darstellt.

Mit diesem Update wird das „Einrasten“ der Verweisanzahl unterstützt, sodass die Anzahl nicht mehr steigen kann, sobald sie null erreicht hat. Dennoch ist das Verwenden von QueryInterface für alle temporären Elemente möglich, die bei der Löschung erforderlich sind. Dieses Vorgehen ist in gewissen XAML-Anwendungen/-Steuerelementen nicht zu vermeiden. C++/WinRT ist jetzt in diesem Zusammenhang robust.

Sie können die Löschung verzögern, indem Sie für den Implementierungstyp die statische Funktion **final_release** bereitstellen. An **final_release** wird der letzte verbleibende Zeiger auf das Objekt, in Form eines **std::unique_ptr**, übergeben. Sie können dann den Besitzer dieses Objekts in einen anderen Kontext verschieben. Sie können QueryInterface für den Zeiger verwenden, ohne eine doppelte Löschung auszulösen. Das Zurücksetzen der Verweiszählung auf 0 (null) muss jedoch an dem Punkt erfolgen, an dem das Objekt gelöscht wird.

Der Rückgabewert von **final_release** kann `void`, ein Objekt eines asynchronen Vorgangs, z. B. [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction), oder **winrt::fire_and_forget** sein.

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

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

Im unten stehenden Beispiel wird **MainPage** freigegeben (zum letzten Mal), und **final_release** wird aufgerufen. Die Funktion wartet fünf Sekunden (im Threadpool) und fährt dann mit dem **Dispatcher**-Objekt der Seite fort (dafür müssen QueryInterface/AddRef/Release funktionieren). Anschließend bereinigt sie eine Ressource in diesem UI-Thread. Und schließlich löscht sie **unique_ptr**, wodurch der **MainPage**-Destruktor aufgerufen wird. Auch in diesem Destruktor wird **DataContext** aufgerufen, wofür eine QueryInterface-Methode für **IFrameworkElement** erforderlich ist.

Sie müssen **final_release** nicht als Coroutine implementieren. Es würde aber funktionieren, und es macht das Verschieben der Löschung in einen anderen Thread einfacher. Dies wird in diesem Beispiel gezeigt.

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

Weitere Informationen finden Sie unter [verzögerte Zerstörung](/windows/uwp/cpp-and-winrt-apis/details-about-destructors#deferred-destruction).

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Verbesserte Unterstützung für die Vererbung einer Schnittstelle (COM)

C++/WinRT wird nicht nur für die Windows-Runtime-Programmierung sondern auch zum Erstellen und Verwenden von Nur-COM-APIs verwendet. Mit diesem Update können Sie einen COM-Server mit einer Schnittstellenhierarchie implementieren. Dies ist für die Windows-Runtime nicht erforderlich, jedoch für einige COM-Implementierungen.

#### <a name="correct-handling-of-out-params"></a>Korrekte Verarbeitung von `out`-Parametern

Es ist nicht einfach, mit `out`-Parametern zu arbeiten, besonders in Windows-Runtime-Arrays. Mit diesem Update wird C++/WinRT deutlich widerstandsfähiger bei Fehlern im Zusammenhang mit `out`-Parametern und -Arrays, egal ob diese Parameter aus der Sprachprojektion oder von COM-Entwicklern stammen, die die unformatierte ABI verwenden und Variablen nicht ordnungsgemäß konsistent initialisieren. In beiden Fällen führt C++/WinRT nun die richtige Aktion beim Übergeben projizierter Typen an die ABI (indem es alle Ressourcen freigibt) und beim Löschen oder Entfernen von Parametern durch, die über die ABI empfangen werden.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Ereignisse verarbeiten ungültige Token jetzt zuverlässig

Die [**winrt::event**](/uwp/cpp-ref-for-winrt/event)-Implementierung verarbeitet jetzt Fälle ordnungsgemäß, in denen ihre **remove**-Methode mit einem ungültigen Tokenwert aufgerufen wird (ein Wert, der im Array nicht vorhanden ist).

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>Lokale Variablen der Coroutine werden vor der Rückgabe der Coroutine gelöscht

Bei einer traditionellen Implementierung einer Coroutine können lokale Variablen in der Coroutine *nach* der Rückgabe/dem Abschluss der Coroutine gelöscht werden (statt vor dem letzten Anhalten). Das Fortsetzen von Wartevorgängen wird jetzt bis zum letzten Anhalten verzögert, um dieses Problem zu vermeiden und andere Vorteile zu schaffen.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Neuerungen und Änderungen im Windows SDK, Version 10.0.17763.0 (Windows 10, Version 1809)

In der unten stehenden Tabelle werden Änderungen für C++/WinRT im Windows SDK, Version 10.0.17763.0 (Windows 10, Version 1809) aufgelistet.

| Neue oder geänderte Funktion | Weitere Informationen |
| - | - |
| **Breaking Change**. C++/WinRT ist bei der Kompilierung nicht mehr vom Windows SDK abhängig. | Weitere Informationen finden Sie unten unter [Isolation von Windows SDK-Headerdateien](#isolation-from-windows-sdk-header-files). |
| Das Systemformat von Visual Studio-Projekten hat sich geändert. | Weitere Informationen finden Sie unten unter [Festlegen des Ziels Ihres C++/WinRT-Projekt auf eine spätere Version des Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk). |
| Es gibt neue Funktionen und Basisklassen, mit denen Sie ein Sammlungsobjekt an eine Windows-Runtime-Funktion übergeben oder Ihre eigenen Sammlungseigenschaften und Sammlungstypen implementieren können. | Weitere Informationen finden Sie unter [Collections with C++/WinRT (Sammlungen in C++/WinRT)](collections.md). |
| Sie können die Markup-Erweiterung [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) mit Ihren C++/WinRT-Runtimeklassen verwenden. | Weitere Informationen und Codebeispiele finden Sie in der [Übersicht über Datenbindung](/windows/uwp/data-binding/data-binding-quickstart). |
| Durch Unterstützung für das Abbrechen einer Coroutine können Sie einen Abbruchsrückruf registrieren. | Weitere Informationen finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks). |
| Beim Erstellen eines Delegaten, der auf eine Memberfunktion zeigt, können Sie einen starken oder einen schwachen Verweis auf das aktuelle Objekt (statt eines unformatierten *this*-Zeiger) erstellen. | Weitere Informationen und Codebeispiele finden Sie im Unterabschnitt **If you use a member function as a delegate (Wenn Sie eine Memberfunktion als Delegaten verwenden)** im Artikel [*Strong and weak references in C++/WinRT (Starke und schwache Verweise in C++/WinRT)* ](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Es wurden Probleme behoben, die durch die verbesserte Konformität mit dem C++-Standard in Visual Studio erkannt wurden. Die LLVM- und Clang-Toolkette wird zudem besser eingesetzt, um die Konformität mit Standards in C++/WinRT zu prüfen. | Das unter [Warum wird mein neues Projekt nicht kompiliert? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher) und die SDK-Version 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) beschriebene Problem tritt nicht mehr auf. |

Sonstige Änderungen

- **Breaking Change**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) gibt jetzt `void*` statt `HSTRING` zurück. Sie können `static_cast<HSTRING>(get_abi(my_hstring));` verwenden, um HSTRING abzurufen. Siehe [Interaktion mit dem HSTRING der ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Breaking Change**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) gibt jetzt `void**` statt `HSTRING*` zurück. Sie können `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` verwenden, um HSTRING* abzurufen. Siehe [Interaktion mit dem HSTRING der ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Breaking Change**. HRESULT wird jetzt als **winrt::hresult** projiziert. Wenn Sie HRESULT benötigen, z. B. zur Typüberprüfung oder zur Unterstützung für Typmerkmale, können Sie `static_cast` für **winrt::hresult** verwenden. Andernfalls wird **winrt::hresult** in HRESULT konvertiert, wenn Sie `unknwn.h` vor C++/WinRT-Headern einfügen.
- **Breaking Change**. GUID wird jetzt als **winrt::guid** projiziert. Wenn Sie APIs implementieren, müssen Sie **winrt::guid** für GUID-Parameter verwenden. Andernfalls wird **winrt::guid** in GUID konvertiert, wenn Sie `unknwn.h` vor C++/WinRT-Headern einfügen. Siehe [Interaktion mit der GUID-Struktur der ABI](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct).
- **Breaking Change**. Der Konstruktor [**winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) wurde gehärtet, indem es explizit gemacht wurde (es ist jetzt nicht mehr so leicht, falschen Code damit zu schreiben). Wenn Sie einen unformatierten handle-Wert zuweisen müssen, rufen Sie stattdessen die [**Funktion handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) auf.
- **Breaking Change**. Die Signaturen von **WINRT_CanUnloadNow** und **WINRT_GetActivationFactory** haben sich geändert. Sie dürfen diese Funktionen nicht deklarieren. Verwenden Sie stattdessen `winrt/base.h` (was automatisch verwendet wird, wenn Sie C++/WinRT-Windows-Namespaceheaderdateien verwenden), um die Deklarationen dieser Funktionen einzubeziehen.
- Für die [**winrt::clock-Struktur**](/uwp/cpp-ref-for-winrt/clock) sind **from_FILETIME/to_FILETIME** veraltet, stattdessen werden **from_file_time/to_file_time** verwendet.
- Vereinfachte APIs, die **IBuffer**-Parameter erwarten. Die meisten APIs bevorzugen Sammlungen oder Arrays. Wir sind jedoch der Meinung, dass wir es einfacher machen sollten, APIs aufzurufen, die sich auf **IBuffer** basieren. Dieses Update bietet direkten Zugriff auf die Daten hinter einer **IBuffer**-Implementierung. Dabei wird dieselbe Datenbenennungskonvention verwendet wie für die Container der C++-Standardbibliothek. Durch diese Konvention werden zudem Überschneidungen mit Metadatennamen verhindert, die per Konvention mit einem Großbuchstaben beginnen.
- Verbesserte Codegenerierung: verschiedene Verbesserungen zum Senken der Codegröße, für verbesserte Inlineersetzung und für optimiertes Factoryzwischenspeichern.
- Unnötige Rekursion wurde entfernt. Wenn die Befehlszeile auf einen Ordner statt auf eine spezifische `.winmd`-Datei verweist, sucht `cppwinrt.exe` nicht mehr rekursiv nach `.winmd`-Dateien. Das `cppwinrt.exe`-Tool behandelt Duplikate jetzt auf sinnvollere Weise, sodass es robuster gegenüber Benutzerfehler und falsch formatierten `.winmd`-Dateien ist.
- Gehärtete intelligente Zeiger. In der Vergangenheit konnten event_revoker-Objekte keinen Widerruf durchführen, wenn ihnen per Verschieben ein neuer Wert zugewiesen wurde. Dadurch wurde ein Problem erkannt, bei dem Klassen für intelligente Zeiger die Selbstzuweisung nicht zuverlässig verarbeitet haben. Dieses Problem lag in der [**Vorlage für eine winrt::com_ptr-Struktur**](/uwp/cpp-ref-for-winrt/com-ptr). **winrt::com_ptr** und die event_revoker-Objekte wurden korrigiert, sodass sie jetzt Verschiebesemantik korrekt behandeln und bei der Zuweisung einen Widerruf durchführen.

> [!IMPORTANT]
> An der [Visual Studio-Erweiterung C++/WinRT (VXIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) wurden sowohl in Version 1.0.181002.2 als auch in Version 1.0.190128.4 wichtige Änderungen vorgenommen. Weitere Informationen zu diesen Änderungen (und wie diese sich auf Ihre vorhandenen Projekte auswirken) finden Sie in der Einführung in C++/WinRT in den Abschnitten zur [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) und zu [früheren Versionen der VSIX-Erweiterung](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Isolation von Windows SDK-Headerdateien

Dabei kann es sich um einen Breaking Change für Ihren Code handeln.

Damit Ihr Code kompiliert werden kann, ist C++/WinRT nicht mehr von Windows SDK-Headerdateien abhängig. Headerdateien in der C-Runtimebibliothek (CRT) und der C++-Standardvorlagenbibliothek enthalten auch keine Windows SDK-Header. Dadurch wird die Konformität mit Standards verbessert, unbeabsichtigte Abhängigkeiten vermieden und die Anzahl der Makros deutlich reduziert, für die Sie Schutzmaßnahmen ergreifen müssen.

Diese Unabhängigkeit bedeutet, dass C++/WinRT jetzt portierbarer und standardkonformer ist. Zudem besteht so eher die Möglichkeit, dass es als Crosscompiler und plattformübergreifende Bibliothek verwendet werden kann. Außerdem bedeutet das, dass C++/WinRT-Header keine beeinträchtigten Makros sind.

Wenn Sie es in der Vergangenheit C++/WinRT überlassen haben, Windows-Header in Ihr Projekt einzubeziehen, müssen Sie sie jetzt selbst einbeziehen. In jedem Fall empfiehlt es sich, Header explizit einzubeziehen, die Sie benötigen, und dies nicht einer Bibliothek zu überlassen.

Aktuell bestehen nur für systeminterne Dateien und numerische Werte Ausnahmen bei der Windows SDK-Headerdateiisolation. Es gibt keine bekannten Probleme für diese verbleibenden Abhängigkeiten.

Sie können in Ihrem Projekt die Interoperabilität mit den Windows SDK-Headern falls erforderlich wieder aktivieren. Sie sollten z. B. eine COM-Schnittstelle implementieren ([**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) als Stamm). Beziehen Sie für dieses Beispiel zunächst `unknwn.h` ein, bevor Sie C++/WinRT-Header einbeziehen. Dadurch kann die C++/WinRT-Basisbibliothek verschiedene Hooks unterstützen, die klassische COM-Schnittstellen unterstützen. Ein Codebeispiel finden Sie unter [Author COM components with C++/WinRT (Erstellen von COM-Komponenten mit C++/WinRT)](author-coclasses.md). Beziehen Sie zudem alle Windows SDK-Header ein, die Typen und/oder Funktionen deklarieren, die Sie aufrufen möchten.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Festlegen des Ziels Ihres C++/WinRT-Projekt auf eine spätere Version des Windows SDK

Die Methode zum Festlegen eines neuen Ziels für Ihr Projekt, die zu den wenigsten Compiler- und Linkerproblemen führt, ist gleichzeitig die aufwändigste. Dazu müssen Sie ein neues Projekt erstellen, das die gewünschte Windows SDK-Version als Ziel verwendet. Kopieren Sie dann die Dateien aus Ihrem alten Projekt in Ihr neues Projekt. Es gibt Abschnitte Ihrer alten `.vcxproj`- und `.vcxproj.filters`-Dateien, die Sie einfach kopieren können, um Zeit für das Hinzufügen von Dateien in Visual Studio zu sparen.

Es gibt jedoch in Visual Studio zwei andere Möglichkeiten zum Festlegen eines neuen Ziels für Ihr Projekt.

- Wechseln Sie zu den Projekteigenschaften **Allgemein** \> **Windows SDK-Version**, und wählen Sie **Alle Konfigurationen** und **Alle Plattformen** aus. Legen Sie die **Windows SDK-Version** auf die Version fest, die Sie als Ziel verwenden möchten.
- Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, klicken Sie auf **Projekte neu ausrichten**, wählen Sie die Version(en) aus, die Sie als Ziel verwenden möchten, und klicken Sie anschließend auf **OK**.

Wenn Compiler- oder Linkerfehler nach diesen Vorgehensweisen auftreten, können Sie die Projektmappe vor dem nächsten Build bereinigen (**Erstellen** > **Projektmappe bereinigen** und/oder manuelles Löschen aller temporären Ordner und Dateien).

Wenn der C++-Compiler den Fehler „*Fehler C2039: „IUnknown“ ist kein Member von \`„globaler Namespace“* “ ausgibt, fügen Sie `#include <unknwn.h>` oben in Ihrer `pch.h`-Datei hinzu (bevor Sie C++/WinRT-Header einbeziehen).

Es kann sein, dass Sie danach auch noch `#include <hstring.h>` hinzufügen müssen.

Wenn der C++-Linker den Fehler „*Fehler LNK2019: Verweis auf nicht aufgelöstes externes Symbol _WINRT_CanUnloadNow@0 in Funktion _VSDesignerCanUnloadNow@0* “ ausgibt, können Sie diesen Fehler beheben, indem Sie `#define _VSDESIGNER_DONT_LOAD_AS_DLL` zu Ihrer `pch.h`-Datei hinzufügen.
