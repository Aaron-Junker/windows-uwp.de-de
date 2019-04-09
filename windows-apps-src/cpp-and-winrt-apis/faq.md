---
description: Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT.
title: Häufig gestellte Fragen zu C++/WinRT
ms.date: 10/26/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, häufig, gestellte, fragen, faq
ms.localizationpriority: medium
ms.openlocfilehash: 70aedf4034ce433b0aa529375799cf45a18ca3e0
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291888"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Häufig gestellte Fragen zu C++/WinRT
Antworten auf Fragen, die Sie wahrscheinlich zur Erstellung und Nutzung von Windows-Runtime-APIs mit können [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Wenn Ihre Frage eine Fehlermeldung betrifft, die Sie gesehen haben, lesen Sie auch das Thema [Problembehandlung bei C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Wie ich neuausrichtung Meine C + c++ / WinRT-Projekt auf eine neuere Version des Windows SDK?
Finden Sie unter [wie neu ausrichten, Ihrem C + c++ / WinRT-Projekt auf eine neuere Version des Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Warum kompiliert nicht Mein neue Projekt? Ich verwende Visual Studio 2017 (Version 15.8.0 oder höher), und von SDK Version 17134
Bei Verwendung von Visual Studio 2017 (Version 15.8.0 oder höher), und auf dem Windows SDK, Version 10.0.17134.0 (Windows 10, Version 1803), klicken Sie dann einen neu erstellten C++/WinRT-Projekt kann nicht mit dem Fehler kompiliert "*Fehler C3861 aus:"From_abi": Bezeichner wurde nicht gefunden*", und klicken Sie mit anderen Fehlern, die aus *base.h*. Die Lösung besteht darin, entweder Ziel einer höheren (genauer) Version des Windows SDK oder Set-Projekteigenschaft **C/C++-** > **Sprache** > **Konformitätsmodus: Nicht** (auch wenn **/ PERMISSIVE--** wird in den Projekteigenschaften **C/C++-** > **Befehlszeile** unter **zusätzliche Optionen** , löschen Sie sie).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Wie löse ich den Buildfehler "C++ / WinRT VSIX nicht mehr Build-projektunterstützung bereitstellt.  Sie fügen einen Projektverweis auf die Microsoft.Windows.CppWinRT Nuget-Paket"?
Installieren Sie die **Microsoft.Windows.CppWinRT** NuGet-Paket in Ihr Projekt. Weitere Informationen finden Sie unter [frühere Versionen der Erweiterung VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Die Anforderungen für die C++ / WinRT Visual Studio-Erweiterung (VSIX)?
Version 1.0.190128.4 der VSIX-Erweiterung und höher, finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Andere Versionen finden Sie [frühere Versionen der Erweiterung VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>Was ist eine *Laufzeitklasse*?
Eine Laufzeitklasse ist ein Typ, der über moderne COM-Schnittstellen aktiviert und genutzt werden kann, typischerweise über Ausführungsgrenzen hinweg. Eine Laufzeitklasse kann aber auch innerhalb der Kompiliereinheit verwendet werden, die sie implementiert. Sie deklarieren eine Laufzeitklasse in der Interface Definition Language (IDL) und können sie in Standard C++ mit C++/WinRT implementieren.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Was bedeuten *der projizierte Typ* und *der Implementierungstyp*?
Wenn Sie eine Windows-Runtime-Klasse (Laufzeitklasse) nur *verwenden*, dann arbeiten Sie ausschließlich mit *projizierten Typen*. C++/WinRT ist eine *Sprachprojektion*. Projizierte Typen sind Teil der Oberfläche von Windows-Runtime, die über C++/WinRT auf C++ *projiziert* werden. Weitere Informationen finden Sie unter [Nutzen von APIs mit C++ / WinRT](consume-apis.md).

Der *Implementierungstyp* enthält die Implementierung einer Laufzeitklasse. Er ist also nur im Projekt verfügbar, das die Laufzeitklasse implementiert. Wenn Sie in einem Projekt arbeiten, das Laufzeitklassen implementiert (ein Projekt für eine Komponente für Windows-Runtime oder ein Projekt, das XAML-UI verwendet), ist es wichtig, sich mit der Unterscheidung zwischen Ihrem Implementierungstyp für eine Laufzeitklasse und dem projizierten Typ, der die in C++/WinRT projizierte Laufzeitklasse repräsentiert, vertraut zu machen. Weitere Details finden Sie unter [APIs mit C++/WinRT erstellen](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Muss ich einen Konstruktor in der IDL meiner Laufzeitklasse deklarieren?
Nur wenn die Laufzeitklasse so konzipiert ist, dass sie von außerhalb ihrer implementierenden Kompiliereinheit verwendet werden kann (es handelt sich um eine Komponente für Windows-Runtime, die für den allgemeinen Gebrauch durch Windows-Runtime-Clientanwendungen bestimmt ist). Ausführliche Informationen über den Zweck und die Konsequenzen der Deklaration von Konstruktoren in IDL finden Sie unter [Runtime-Klassenkonstruktoren](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Warum wird der Linker stellt mir so eine "LNK2019: Nicht aufgelöstes externes Symbol"Fehler?
Wenn es sich bei dem nicht aufgelösten Symbol um eine API aus den Windows-Namespace-Headern für die C++/WinRT-Projektion (im **Winrt**-Namespace) handelt, ist die API in einem Header forward-deklariert, den Sie eingeschlossen haben, ihre Definition befindet sich aber in einem Header, der noch nicht hinzugefügt wurde. Binden Sie den Header ein, der für den Namespace der API benannt ist, und führen Sie eine erneute Erstellung durch. Weitere Informationen finden Sie unter [C++/WinRT-Projektionsheader](consume-apis.md#cwinrt-projection-headers).

Wenn das Symbol nicht aufgelöste eine kostenlose Windows-Runtime-Funktion, z. B. [RoInitialize](https://msdn.microsoft.com/library/br224650), müssen Sie zum expliziten Verknüpfen der [WindowsApp.lib](/uwp/win32-and-com/win32-apis) Schirm-Bibliothek in Ihrem Projekt. Die C++/WinRT-Projektion hängt von einigen dieser freien (Nicht-Mitglieds)-Funktionen und Einstiegspunkten ab. Wenn Sie eine der [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix)-Projektvorlagen für Ihre Anwendung verwenden, wird `WindowsApp.lib` automatisch für Sie verknüpft. Andernfalls können Sie die Projektlinkeinstellungen verwenden, um sie einzuschließen, oder dies im Quellcode erledigen.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Es ist wichtig, dass Sie alle Linkerfehler, die Sie beheben durch das Verknüpfen von **WindowsApp.lib** anstelle einer alternativen Static Link Library, Ihre Anwendung wird nicht übergeben Sie andernfalls die [Windows App Certification Kit](../debug-test-perf/windows-app-certification-kit.md) Tests durch Visual Studio und dem Microsoft Store zum Überprüfen von Übermittlungen (d. h., der es nicht ist daher möglich, dass Ihre Anwendung erfolgreich in den Microsoft Store erfasst werden) verwendet.

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Sollte ich [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) implementieren und wenn ja, wie?
Wenn Sie eine Laufzeitklasse haben, die Ressourcen in ihrem Destruktor freigibt, und diese Laufzeitklasse dafür ausgelegt ist außerhalb ihrer implementierenden Kompiliereinheit genutzt zu werden (eine Komponente für Windows-Runtime, die für die allgemeinen Nutzung durch Windows-Runtime-Clientanwendungen bestimmt ist), dann empfehlen wir Ihnen, **IClosable** zu implementieren, um die Nutzung Ihrer Laufzeitklasse durch Sprachen ohne deterministische Finalisierung zu unterstützen. Stellen Sie sicher, dass Ihre Ressourcen freigegeben werden – egal ob der Destruktor, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) oder beide aufgerufen werden. **IClosable::Close** kann beliebig oft aufgerufen werden.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Muss ich [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) bei Laufzeitklassen aufrufen, die ich nutze?
**IClosable** existiert, um Sprachen zu unterstützen, die keine deterministische Finalisierung haben. Daher sollten Sie **IClosable::Close** nicht von C++/WinRT aus aufrufen, außer in sehr seltenen Fällen, in denen es um Shutdown-Races oder halb Semi-Deadocks geht. Wenn Sie z. B. **Windows.UI.Composition**-Typen verwenden, können Sie auf Fälle stoßen, in denen Sie Objekte in einer festgelegten Reihenfolge verwerfen möchten (statt dies durch den C++/WinRT-Wrapper erledigen zu lassen).

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Kann ich LLVM/Clang verwenden, um mit C++/WinRT zu kompilieren?
Die Toolkette LLVM und Clang wird für C++/WinRT nicht unterstützt, aber wir verwenden sie intern, um die Standardkonformität von C++/WinRT zu überprüfen. Wenn Sie z. B. emulieren möchten, was wir intern tun, könnten Sie ein Experiment wie das unten beschriebene ausprobieren.

Wechseln Sie zur [LLVM-Download-Seite](https://releases.llvm.org/download.html), suchen Sie nach **Download LLVM 6.0.0** > **Pre-Built Binaries**, und laden Sie **Clang for Windows (64-bit)** herunter. Geben Sie während der Installation an, dass LLVM zur Systemvariablen PATH hinzugefügt werden soll, sodass Sie in der Lage sind, sie von einer Befehlszeile aufzurufen. Für die Zwecke dieses Experiments können Sie die Meldungen, dass das Verzeichnis für die MSBuild-Toolsets nicht gefunden werden konnte und/oder dass die MSVC-Integrationsinstallation fehlgeschlagen ist, ignorieren (sollten diese angezeigt werden). Es gibt verschiedene Möglichkeiten zum Aufrufen von LLVM/Clang. Das folgende Beispiel zeigt nur eine davon.

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

Visual Studio ist das Entwicklungstool, das wir unterstützen und für C++/WinRT empfehlen. Finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Warum keine der generierten implementierungsfunktion für eine nur-Lese Eigenschaft der `const` Qualifizierer?
Wenn Sie deklarieren eine nur-Lese Eigenschaft in [MIDL 3.0](/uwp/midl-3/), Sie erwarten wahrscheinlich die `cppwinrt.exe` -Tool zum Generieren einer implementierungsfunktion für Sie `const`-qualifizierten (ein Const-Funktion behandelt die *dieser* Zeiger als Konstanten).

Natürlich empfiehlt Konstanten nach Möglichkeit, aber die `cppwinrt.exe` Tool selbst wird nicht versucht, den Grund, über welche Implementierung Funktionen möglicherweise ist vorstellbar, dass "const" sein, und die möglicherweise nicht. Sie können auch die Funktionen Ihrer Implementierung konstant, wie im folgenden Beispiel stellen.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Sie können entfernen, `const` Qualifizierer **ToString** müssen Sie entscheiden, Sie einige Zustand des Objekts in seiner Implementierung zu ändern müssen. Aber machen Sie jede der Member-Funktionen konstant oder nicht Konstante, nicht beides. Das heißt, eine implementierungsfunktion auf überladen nicht `const`.

Abgesehen von Ihrer Implementierungsfunktionen, eine andere andere platzieren, const kommt das Bild wird in Windows-Runtime-Funktion Projektionen. Betrachten Sie diesen Code.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Für den Aufruf von **ToString** oben die **Gehe zu Deklaration** Befehl in Visual Studio zeigt, dass die Projektion der Windows-Runtime **IStringable::ToString** in C++ / WinRT sieht wie folgt aus.

```cppwinrt
winrt::hstring ToString() const;
```

Funktionen in der Projektion sind unabhängig davon, wie Sie Ihre Implementierung von ihnen qualifizieren auch const. Hinter den Kulissen Ruft die Projektion auf die Application binary Interface (ABI), welche Beträge zu einem Aufruf über ein COM-Schnittstellenzeiger. Der einzige Zustand, der das **ToString** interagiert mit dieser COM-Schnittstellenzeiger, und sicherlich nicht erforderlich, den dieser Zeiger, zu ändern, sodass die Funktion "const" ist. Dies bietet Ihnen die Gewissheit, die es nichts zu ändern, wird nicht die **IStringable** Verweis wird sichergestellt, dass Sie aufrufen können, die Sie aufrufen über **ToString** selbst bei einem const-verweist auf eine  **IStringable**.

Zu verstehen, die diese Beispiele von `const` werden Details zur Implementierung von C++ / WinRT Projektionen und Implementierungen; sie bilden aufgrund von Code für Ihr Angebot. Es gibt keine `const` auf das COM- oder das Windows-Runtime-ABI (für Memberfunktionen).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Haben Sie keine Empfehlungen zum Verringern der Größe des Codes für C++ / WinRT-Binärdateien?
Bei der Arbeit mit Windows-Runtime-Objekte sollten Sie die unten angezeigt werden, da es sich negativ auf Ihre Anwendung haben kann, verliert weiterer Binärcode als nötig generiert werden, indem die Codierungsmuster.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

In der Windows-Runtime-Welt ist der Compiler kann nicht den Wert des cache `c()` oder die Schnittstellen für jede Methode, die durch eine Dereferenzierung aufgerufen wird ("."). Es sei denn, Sie eingreifen, führt dies mehr virtuelle Aufrufe und verweiszählung Mehraufwand. Die Struktur der obigen konnte doppelt so viel Code, der als unbedingt notwendig ganz einfach generiert werden. Bevorzugen Sie stattdessen das Muster, dort Sie können unten. Es wird viel weniger Code generiert, und es kann auch erheblich steigern Sie Ihre Leistung zur Laufzeit.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Das empfohlene Muster oben gilt nicht nur für C++ / WinRT, sondern alle Windows-Runtime-Sprache per sprachprojektion.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Wie aktiviere ich eine Zeichenfolge in einen Typ&mdash;für die Navigation, z. B.?
Am Ende der [Navigation anzeigen-Codebeispiel](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (das ist vor allem in C#), es ist ein C++ / WinRT-Codeausschnitt zeigt, wie Sie dies.

> [!NOTE]
> Wenn in diesem Thema nicht Ihre Fragen beantworten, können Sie Hilfe zu finden, indem Sie die [Visual Studio C++-Entwickler-Community](https://developercommunity.visualstudio.com/spaces/62/index.html), oder mithilfe der [ `c++-winrt` tag in Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
