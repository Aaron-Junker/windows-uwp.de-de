---
description: Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT.
title: Häufig gestellte Fragen zu C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, häufig, gestellte, fragen, faq
ms.localizationpriority: medium
ms.openlocfilehash: b0ec2c5a05e7c4e9309311fa22ad863d06597a53
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254994"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Häufig gestellte Fragen zu C++/WinRT
Hier finden Sie Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Wenn sich Ihre Frage auf eine Fehlermeldung bezieht, die Ihnen angezeigt wurde, lesen Sie auch das Thema [Problembehandlung bei C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Wie richte ich mein C++/WinRT-Projekt auf eine höhere Version des Windows SDK neu aus?
Lesen Sie [So richten Sie Ihr C++/WinRT-Projekt auf eine höhere Version des Windows SDK neu aus](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>Warum wird mein neues Projekt nach der Umstellung auf C++/WinRT 2.0 nicht kompiliert?
Sämtliche Änderungen (einschließlich wichtiger Änderungen) finden Sie unter [Neuerungen und Änderungen in C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20). Wenn Sie z. B. für eine Windows-Runtime-Sammlung eine bereichsbasierte `for`-Anweisung verwenden, müssen Sie jetzt `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Warum wird mein neues Projekt nicht kompiliert? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher) und die SDK-Version 17134
Wenn Sie Visual Studio 2017 (Version 15.8.0 oder höher) und als Zielversion Windows SDK Version 10.0.17134.0 (Windows 10, Version 1803) verwenden, tritt beim Kompilieren eines neu erstellten C++/WinRT-Projekts möglicherweise der Fehler „*Fehler C3861: 'from_abi': Bezeichner wurde nicht gefunden*“ zusammen mit anderen Fehlern auf, die von *base.h* verursacht werden. Die Lösung besteht darin, für das Projekt eine höhere (konformere) Version des Windows SDK zu verwenden oder die Projekteigenschaft **C/C++**  > **Sprache** > **Konformitätsmodus: Nein** festzulegen (auch wenn **/permissive-** in der Projekteigenschaft **C/C++**  > **Befehlszeile** unter **Zusätzliche Optionen** angezeigt wird. Löschen Sie es dann).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Wie behebe ich den Buildfehler „Die C++/WinRT-VSIX stellt keine Buildunterstützung für das Projekt mehr zur Verfügung.  Fügen Sie dem Projekt einen Verweis auf das Microsoft.Windows.CppWinRT-Nuget-Paket hinzu.“?
Installieren Sie das NuGet-Paket **Microsoft.Windows.CppWinRT** für Ihr Projekt. Ausführlichere Informationen finden Sie unter [Frühere Versionen der VSIX-Erweiterung](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="how-do-i-customize-the-build-support-in-the-nuget-package"></a>Gewusst wie: Anpassen der Buildunterstützung im NuGet-Paket.

Die Buildunterstützung für C++/WinRT (Eigenschaften/Ziele) ist in der [Infodatei](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) zum Microsoft.Windows.CppWinRT-NuGet-Paket dokumentiert.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Was sind die Voraussetzungen für die C++/WinRT Visual Studio Extension (VSIX)?
Informationen zu Version 1.0.190128.4 (und höher) der VSIX-Erweiterung finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Informationen zu anderen Versionen finden Sie unter [Frühere Versionen der VSIX-Erweiterung](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>Was ist eine *Laufzeitklasse*?
Eine Laufzeitklasse ist ein Typ, der über moderne COM-Schnittstellen aktiviert und genutzt werden kann, typischerweise über Ausführungsgrenzen hinweg. Eine Laufzeitklasse kann aber auch innerhalb der Kompilierungseinheit verwendet werden, die sie implementiert. Sie deklarieren eine Laufzeitklasse in der Interface Definition Language (IDL), und Sie können sie in Standard C++ mit C++/WinRT implementieren.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Was bedeuten *der projizierte Typ* und *der Implementierungstyp*?
Wenn Sie eine Windows-Runtime-Klasse (Laufzeitklasse) nur *verwenden*, dann arbeiten Sie ausschließlich mit *projizierten Typen*. C++/WinRT ist eine *Sprachprojektion*. Projizierte Typen sind Teil der Oberfläche von Windows-Runtime, die über C++/WinRT auf C++ *projiziert* werden. Weitere Details finden Sie unter [Verwenden von APIs mit C++/WinRT](consume-apis.md).

Der *Implementierungstyp* enthält die Implementierung einer Laufzeitklasse. Er ist also nur in dem Projekt verfügbar, das die Laufzeitklasse implementiert. Wenn Sie in einem Projekt arbeiten, das Laufzeitklassen implementiert (ein Projekt für eine Komponente für Windows-Runtime oder ein Projekt, das XAML-UI verwendet), ist es wichtig, sich mit dem Unterschied zwischen Ihrem Implementierungstyp für eine Laufzeitklasse und dem projizierten Typ, der die in C++/WinRT projizierte Laufzeitklasse repräsentiert, vertraut zu machen. Weitere Details finden Sie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Muss ich einen Konstruktor in der IDL meiner Laufzeitklasse deklarieren?
Nur wenn die Laufzeitklasse so konzipiert ist, dass sie von außerhalb ihrer implementierenden Kompilierungseinheit verwendet werden kann (es handelt sich um eine Komponente für Windows-Runtime, die für den allgemeinen Gebrauch durch Windows-Runtime-Clientanwendungen bestimmt ist). Ausführliche Informationen über den Zweck und die Konsequenzen der Deklaration von Konstruktoren in IDL finden Sie unter [Laufzeitklassenkonstruktoren](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Warum gibt der Linker den Fehler „LNK2019: Nicht aufgelöstes externes Symbol“ aus?
Wenn es sich bei dem nicht aufgelösten Symbol um eine API aus den Windows-Namespaceheadern für die C++/WinRT-Projektion (im **WinRT**-Namespace) handelt, wird die API in einem eingebundenen Header forward-deklariert, aber ihre Definition befindet sich in einem noch nicht eingebundenen Header. Binden Sie den Header ein, der für den Namespace der API benannt ist, und führen Sie eine erneute Erstellung durch. Weitere Informationen finden Sie unter [C++/WinRT-Projektionsheader](consume-apis.md#cwinrt-projection-headers).

Wenn es sich bei dem nicht aufgelösten Symbol um eine freie Windows-Runtime-Funktion (wie [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)) handelt, müssen Sie die Überbibliothek [WindowsApp.lib](/uwp/win32-and-com/win32-apis) in Ihrem Projekt explizit verknüpfen. Die C++/WinRT-Projektion hängt von einigen dieser freien Funktionen (Nicht-Memberfunktionen) und Einstiegspunkten ab. Wenn Sie eine der Projektvorlagen von [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) für Ihre Anwendung verwenden, wird `WindowsApp.lib` automatisch für Sie verknüpft. Andernfalls können Sie die Projektverknüpfungseinstellungen verwenden, um sie einzuschließen, oder dies im Quellcode festlegen.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Es ist wichtig, dass Sie alle Linkerfehler beheben, die Sie beheben können, indem Sie **WindowsApp.lib** anstelle einer alternativen Static Link Library (LIB) verknüpfen, weil Ihre Anwendung ansonsten die Tests des [Zertifizierungskits für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) nicht bestehen wird, die Visual Studio und der Microsoft Store zum Überprüfen von Übermittlungen verwenden (was bedeutet, dass Ihre Anwendung folglich nicht erfolgreich in den Microsoft Store aufgenommen werden kann).

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>Warum erhalte ich die Ausnahme „Klasse nicht registriert“?

In diesem Fall wird beim Erstellen einer Laufzeitklasse oder beim Zugriff auf einen statischen Member eine Laufzeitausnahme mit dem HRESULT-Wert „REGDB_E_CLASSNOTREGISTERED“ angezeigt.

Eine mögliche Ursache ist, dass Ihre Windows Runtime-Komponente nicht geladen werden kann. Stellen Sie sicher, dass die Windows Runtime-Metadatendatei (`.winmd`) der Komponente den gleichen Namen wie die Komponentenbinärdatei (`.dll`) hat, der auch der Name des Projekts und des Namespacestamms ist. Stellen Sie außerdem sicher, dass die Windows Runtime-Metadaten und die Binärdatei vom Build-Prozess korrekt in den `Appx`-Ordner der nutzenden App kopiert wurden. Und überprüfen Sie, ob das `AppxManifest.xml` der nutzenden App (auch im Ordner `Appx`) ein **&lt;InProcessServer&gt;** -Element enthält, in dem die aktivierbare Klasse und der Binärname ordnungsgemäß deklariert sind.

### <a name="uniform-construction"></a>Einheitliche Konstruktion

Dieser Fehler kann auch auftreten, wenn Sie versuchen, eine lokal implementierte Laufzeitklasse über einen der Konstruktoren des projizierten Typs zu instanziieren (nicht über den **std::nullptr_t**-Konstruktor). Hierfür benötigen Sie das C++/WinRT-2.0-Feature, das häufig als einheitliche Konstruktion bezeichnet wird. Wenn Sie dieses Feature abonnieren möchten, finden Sie weitere Informationen und Codebeispiele unter [Aktivieren von einheitlicher Konstruktion und direktem Implementierungszugriff](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

Informationen zum Instanziieren von lokal implementierten Laufzeitklassen, die *keine* einheitliche Konstruktion erfordern, finden Sie unter [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Sollte ich [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) implementieren und wenn ja, wie?
Wenn Sie eine Laufzeitklasse verwenden, die Ressourcen in ihrem Destruktor freigibt, und diese Laufzeitklasse von außerhalb ihrer implementierenden Kompilierungseinheit genutzt werden kann (eine für die allgemeine Nutzung durch Windows-Runtime-Clientanwendungen vorgesehene Komponente für Windows-Runtime ist), dann empfehlen wir Ihnen, auch  **IClosable** zu implementieren, um die Nutzung Ihrer Laufzeitklasse durch Sprachen ohne deterministische Finalisierung zu unterstützen. Stellen Sie sicher, dass Ihre Ressourcen freigegeben werden – unabhängig davon, ob der Destruktor, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) oder beide aufgerufen werden. **IClosable::Close** kann beliebig oft aufgerufen werden.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Muss ich [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) bei von mir verwendeten Laufzeitklassen aufrufen?
**IClosable** dient der Unterstützung von Sprachen ohne deterministische Finalisierung. Daher sollten Sie **IClosable::Close** nicht von C++/WinRT aus aufrufen, außer in sehr seltenen Fällen, in denen es um Shutdown Races oder Semi-Deadly Embraces geht. Wenn Sie z. B. **Windows.UI.Composition**-Typen verwenden, können Sie auf Fälle stoßen, in denen Sie Objekte in einer festgelegten Reihenfolge verwerfen möchten (statt dies durch den C++/WinRT-Wrapper erledigen zu lassen).

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Kann ich LLVM/Clang für die Kompilierung mit C++/WinRT zu verwenden?
Die Toolkette LLVM und Clang wird für C++/WinRT nicht unterstützt, aber wir verwenden sie intern, um die Standardkonformität von C++/WinRT zu überprüfen. Wenn Sie z. B. emulieren möchten, was wir intern tun, könnten Sie ein Experiment wie das unten beschriebene ausprobieren.

Wechseln Sie zur [LLVM-Downloadseite](https://releases.llvm.org/download.html), suchen Sie nach **Download LLVM 6.0.0** > **Pre-Built Binaries**, und laden Sie **Clang for Windows (64-bit)** herunter. Geben Sie während der Installation an, dass LLVM zur Systemvariablen PATH hinzugefügt werden soll, damit der Aufruf über eine Eingabeaufforderung erfolgen kann. Für die Zwecke dieses Experiments können Sie Fehlermeldungen ignorieren, die besagen, dass das Verzeichnis für die MSBuild-Toolsets nicht gefunden werden konnte und/oder dass die MSVC-Integrationsinstallation fehlgeschlagen ist, falls diese angezeigt werden. Es gibt verschiedene Möglichkeiten zum Aufrufen von LLVM/Clang. Das folgende Beispiel zeigt nur eine davon.

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Da C++/WinRT Features vom C++17-Standard verwendet, müssen Sie alle Compiler-Flags verwenden, die erforderlich sind, um diese Unterstützung zu erhalten. Diese Flags sind je nach Compiler unterschiedlich.

Visual Studio ist das von uns unterstützte und für C++/WinRT empfohlene Entwicklungstool. Weitere Informationen finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Warum verfügt die generierte Implementierungsfunktion für eine schreibgeschützte Eigenschaft nicht über den `const`-Qualifizierer?
Wenn Sie eine schreibgeschützte Eigenschaft in [MIDL 3.0](/uwp/midl-3/) deklarieren, erwarten Sie wahrscheinlich, dass das Tool `cppwinrt.exe` eine Implementierungsfunktion für Sie generiert, die `const`-qualifiziert ist (eine const-Funktion behandelt den *this*-Zeiger als „const“).

Wir empfehlen natürlich, „const“ nach Möglichkeit zu verwenden, aber das Tool `cppwinrt.exe` selbst versucht nicht, zu begründen, welche Implementierungsfunktionen möglicherweise „const“ sein könnten und welche nicht. Sie können jede Ihrer Implementierungsfunktionen als const-Funktion festlegen, wie im folgenden Beispiel.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Sie können diesen `const`-Qualifizierer für **ToString** entfernen, falls Sie einen Objektstatus der zugehörigen Implementierung ändern müssen. Aber achten Sie darauf, dass jede Ihrer Memberfunktionen entweder „const“ oder „non-const“ ist, nicht beides. Mit anderen Worten, überladen Sie eine Implementierungsfunktion nicht auf `const`.

Abgesehen von Ihren Implementierungsfunktionen kommt „const“ auch in Windows Runtime-Funktionsprojektionen zum Einsatz. Beachten Sie den folgenden Code.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Für den Aufruf von **ToString** oben zeigt der Befehl **Gehe zu Deklaration** in Visual Studio, dass die Projektion der Windows-Runtime **IStringable::ToString** in C++/WinRT wie folgt aussieht.

```cppwinrt
winrt::hstring ToString() const;
```

Funktionen in der Projektion sind „const“, unabhängig davon, wie Sie Ihre Implementierung dieser Funktionen qualifizieren. Die Projektion ruft im Hintergrund die binäre Anwendungsschnittstelle (Application Binary Interface, ABI) auf, was einem Aufruf über einen COM-Schnittstellenzeiger entspricht. Der einzige Zustand, mit dem die projizierte **ToString** interagiert, ist dieser COM-Schnittstellenzeiger, und es ist sicherlich nicht erforderlich, diesen Zeiger zu ändern, sodass die Funktion „const“ ist. Das gibt Ihnen die Sicherheit, dass keine Änderungen am **IStringable**-Verweis vorgenommen werden, über den der Aufruf erfolgt, und stellt sicher, dass Sie **ToString** auch mit einem const-Verweis auf eine **IStringable** aufrufen können.

Sie müssen verstehen, dass es sich bei diesen Beispielen von `const` um Implementierungsdetails von C++/WinRT-Projektionen und -Implementierungen handelt. Sie stellen Codehygiene zu Ihrem Nutzen dar. Es gibt `const` weder auf der COM- noch auf der Windows-Runtime-ABI (für Memberfunktionen).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Gibt es Empfehlungen für das Verringern der Codegröße für C++/WinRT-Binärdateien?
Beim Arbeiten mit Windows-Runtime-Objekten sollten Sie das unten angezeigte Codemuster vermeiden, weil es sich negativ auf Ihre Anwendung auswirken kann, da mehr Binärcode als nötig generiert wird.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

In der Windows-Runtime-Umgebung kann der Compiler weder den Wert von `c()` noch die Schnittstellen für jede Methode zwischenspeichern, die durch eine Dereferenzierung ('.') aufgerufen wird. Wenn Sie nicht eingreifen, führt dies zu mehr virtuellen Aufrufen und einem höheren Verweiszählungsaufwand. Bei dem obigen Muster könnte leicht doppelt so viel Code generiert werden wie unbedingt erforderlich. Bevorzugen Sie stattdessen nach Möglichkeit das unten dargestellte Muster. Dabei wird viel weniger Code generiert, und auch die Laufzeitleistung kann erheblich verbessert werden.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Das oben dargestellte empfohlene Muster gilt nicht nur für C++/WinRT, sondern für alle Windows-Runtime-Sprachprojektionen.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Wie konvertiere ich eine Zeichenfolge in einen Typ – z. B. für die Navigation?
Am Ende des [Codebeispiels für die Navigationsansicht](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (das größtenteils in C# geschrieben ist) finden Sie einen C++/WinRT-Codeausschnitt, der zeigt, wie das geht.

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>Wie löse ich Mehrdeutigkeiten bei „GetCurrentTime“ und/oder „TRY“ auf?

In der Headerdatei `winrt/Windows.UI.Xaml.Media.Animation.h` wird eine Methode mit dem Namen **GetCurrentTime** deklariert, und `windows.h` (über `winbase.h`) definiert ein Makro mit dem Namen **GetCurrentTime**. Bei einem Konflikt zwischen den beiden erzeugt der C++-Compiler „*Fehler C4002: Zu viele Argumente für den Aufruf des funktionsähnlichen Makros "GetCurrentTime*“.

Entsprechend wird in `winrt/Windows.Globalization.h` eine Methode mit dem Namen **TRY** deklariert, und in `afx.h` wird ein Makro mit dem Namen **GetCurrentTime** definiert. Bei einem Konflikt zwischen den beiden erzeugt der C++-Compiler „*Fehler C2334: Unerwartete(s) Token vor "{"; sichtbarer Funktionstext wird übersprungen*“.

Um eines oder beide Probleme zu beheben, können Sie wie folgt vorgehen.

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> Wenn Ihre Frage in diesem Thema nicht beantwortet wurde, finden Sie möglicherweise Hilfe in der [Visual Studio C++-Entwicklercommunity](https://developercommunity.visualstudio.com/spaces/62/index.html), oder verwenden Sie das [`c++-winrt`-Tag auf Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
