---
description: Dies ist ein fortgeschrittenes Thema im Zusammenhang mit der schrittweisen Portierung von [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) nach [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Es wird gezeigt, wie Aufgaben und Co-Routinen der Parallel Patterns Library (PPL) im selben Projekt nebeneinander existieren können.
title: Asynchronität und Interoperabilität zwischen C++/WinRT und C++/CX
ms.date: 08/06/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, Interoperabilität, C++/CX, PPL, Aufgabe, Coroutine
ms.localizationpriority: medium
ms.openlocfilehash: daa263836e9024a0aaa55b239b1a0db9437f1cd5
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88181072"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>Asynchronität und Interoperabilität zwischen C++/WinRT und C++/CX

> [!TIP]
> Obwohl wir empfehlen, dass Sie dieses Thema von Anfang an lesen, können Sie direkt zu einer Zusammenfassung der Interoperabilitätsmethoden im Abschnitt [Übersicht über das asynchrone Portieren von C++/CX zu C++/WinRT](#overview-of-porting-ccx-async-to-cwinrt) wechseln.

Dies ist ein fortgeschrittenes Thema im Zusammenhang mit der schrittweisen Portierung von [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) von [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx). Dieses Thema fährt dort fort, wo das Thema [Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx) aufgehört hat.

Sie benötigen einen Portierungsprozess, bei dem C++/CX- und C++/WinRT-Code eine Zeit lang nebeneinander im selben Projekt existieren, wenn die Größe oder Komplexität Ihrer Codebasis eine schrittweise Portierung erforderlich macht. Wenn Sie über asynchronen Code verfügen, müssen Sie möglicherweise PPL-Aufgabenketten (Parallel Patterns Library) und Coroutinen parallel in Ihrem Projekt haben, während Sie den Quellcode schrittweise portieren. Dieses Thema konzentriert sich auf Methoden für die Interoperabilität zwischen asynchronem C++/CX-Code und asynchronem C++/WinRT-Code. Sie können diese Methoden einzeln oder kombinieren verwenden.

Bevor Sie dieses Thema lesen, empfiehlt es sich, [Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx) zu lesen. In diesem Thema wird gezeigt, wie Sie Ihr Projekt für eine schrittweise Portierung vorbereiten. Es gibt Ihnen ferner eine Einführung in zwei Hilfsfunktionen, die Sie für die Konvertierung eines C++/CX-Objekts in ein C++/WinRT-Objekt (und umgekehrt) verwenden können. Dieses Thema zu Asynchronie baut auf diesen Informationen auf und verwendet diese Hilfsfunktionen.

> [!NOTE]
> Für die schrittweise Portierung gelten einige Einschränkungen. Bei einem Windows-Runtime-Komponentenprojekt ist eine schrittweise Portierung nicht möglich, und Sie müssen das Projekt in einem Durchgang portieren. Bei einem XAML-Projekt müssen Ihre XAML-Seitentypen zu jedem beliebigen Zeitpunkt *entweder* alle in C++/WinRT *oder* alle in C++/CX vorliegen. Weitere Informationen finden Sie unter [Umstellen von C++/CX auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx).

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>Der Grund, warum ein ganzes Thema der Interoperabilität von asynchronem Code gewidmet ist

Die Portierung von C++/CX nach C++/WinRT ist im Allgemeinen unkompliziert, mit der einzigen Ausnahme der Portierung von [Parallel Patterns Library (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl)-Aufgaben zu Coroutinen. Die Modelle unterscheiden sich. Es gibt keine natürliche 1:1-Zuordnung von PPL-Aufgaben zu Coroutinen, und es gibt keine einfache Methode, den Code, der für alle Fälle funktioniert, automatisch zu portieren.

Die gute Nachricht ist, dass die Konvertierung von Aufgaben in Coroutinen zu signifikanten Vereinfachungen führt. Entwicklungsteams berichten außerdem regelmäßig, dass, sobald sie die Hürde der Portierung ihres asynchronen Codes genommen haben, der Rest der Portierung weitgehend automatisch erfolgt.

Häufig wurde ein Algorithmus ursprünglich für synchrone APIs geschrieben. Und anschließend wurde er dann in Aufgaben und explizite Fortsetzungen übersetzt – das Ergebnis war dann häufig eine unbeabsichtigte Verschleierung der zugrunde liegenden Logik. Schleifen werden beispielsweise zu Rekursionen, if-else-Verzweigungen werden zu geschachtelten Strukturen (einer Kette) von Aufgaben und freigegebene Variablen werden zu **shared_ptr**. Um die oft unnatürliche Struktur von PPL-Quellcode zu dekonstruieren, empfehlen wir, dass Sie zunächst einen Schritt zurückgehen und die Absicht des ursprünglichen Codes verstehen (d. h. die ursprüngliche synchrone Version untersuchen). Anschließend fügen Sie dann `co_await` an den entsprechenden Stellen ein.

Wenn Sie also eine C#-Version (statt C++/CX) des asynchronen Codes haben, die als Ausgangspunkt Ihrer Portierung dient, kann Ihnen dies deshalb die Sache erleichtern und die Portierung übersichtlicher gestalten. C#-Code verwendet `await`. Somit befolgt C#-Code also bereits schon im Wesentlichen eine Philosophie, bei der mit einer synchronen Version begonnen und dann an den geeigneten Stellen `await` eingefügt wird.

## <a name="some-background-in-asynchronous-programming"></a>Ein paar Hintergrundinformationen zur asynchronen Programmierung

Damit wir einen festen Referenzrahmen für die Konzepte und Terminologie der asynchronen Programmierung haben, legen wir kurz die Szene in Bezug auf die asynchrone Windows-Runtime-Programmierung im Allgemeinen fest, und klären wir, wie die beiden C++-Sprachprojektionen jeweils auf ihre unterschiedliche Weise darauf aufbauen.

Ihr Projekt verfügt über Methoden, die asynchron funktionieren. In vielen Fällen sollten Sie auf den Abschluss der Arbeit warten, bevor Sie etwas anderes tun. Damit Sie auf eine asynchrone Methode warten können, die dann ein asynchrones Vorgangsobjekt (operation) zurückgibt. Doch manchmal möchten oder müssen Sie nicht auf den Abschluss der asynchronen Vorgänge warten. In diesem Fall muss die Methode kein asynchrones Vorgangsobjekt zurückgeben. Eine asynchrone Methode, auf die Sie nicht warten, wird als *Fire-and-Forget-* -Methode („auslösen und vergessen“) bezeichnet.

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Asynchrone Windows-Runtime-Objekte (**IAsyncXxx**)

Der Windows-Runtime-Namespace **Windows::Foundation** enthält vier Objekttypen für asynchrone Vorgänge.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) und
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

Wenn wir in diesem Thema die praktische Abkürzung **IAsyncXxx** verwenden, beziehen wir uns entweder auf diese Typen als Gruppe, oder wir sprechen über einen der vier Typen, ohne zu spezifizieren, welchen genau.

### <a name="ccx-async"></a>Asynchrones C++/CX

Asynchroner C++/CX-Code nutzt [Parallel Patterns Library (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl)-Tasks. Eine PPL-Aufgabe wird von der [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)-Klasse dargestellt.

In der Regel verkettet eine asynchrone C++/CX-Methode PPL-Aufgaben miteinander unter Verwendung von Lambda-Funktionen mit [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) und [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then). Jede dieser Lambda-Funktionen gibt eine Aufgabe zurück, die, sobald sie abgeschlossen ist, einen Wert erzeugt, der dann an die Lambda-Funktion der *Fortsetzung*der Aufgabe übergeben wird.

Alternativ kann eine asynchrone C++/CX-Methode, anstatt **create_task** aufzurufen, um eine Aufgabe zu erstellen, [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) aufrufen, um ein **IAsyncXxx**\^ zu erstellen.

Somit kann der Rückgabetyp einer asynchronen C++/CX-Methode eine PPL-Aufgabe oder ein **IAsyncXxx**\^ sein.

In beiden Fällen verwendet die Methode selbst das `return`-Schlüsselwort, um ein asynchrones Objekt zurückzugeben, das, wenn es abgeschlossen ist, den Wert erzeugt, den der Aufrufer tatsächlich wünscht (vielleicht eine Datei, ein Bytearray oder einen booleschen Wert).

> [!NOTE]
> Wenn eine asynchrone C++/CX-Methode ein **IAsyncXxx**\^ zurückgibt, kann **TResult** nur einen Windows-Runtime-Typ annehmen. Ein boolescher Wert ist z. B. ein Windows-Runtime-Typ, aber ein C++/CX-projizierter Typ (z. B. **Platform::Array<byte>** ^) ist dies nicht.

### <a name="cwinrt-async"></a>Asynchrones C++/WinRT

C++/WinRT integriert C++-Coroutinen in das Programmiermodell. Coroutinen und die `co_await`-Anweisung bieten eine natürliche Möglichkeit, um kooperativ auf ein Ergebnis zu warten.

Jeder der asynchronen **IAsyncXxx**-Typen in einen entsprechenden Typ im C++/WinRT-Namespace **winrt::Windows::Foundation** projiziert. Diese bezeichnen wir als **winrt::IAsyncXxx**. Der Rückgabetyp einer C++/WinRT-Coroutine ist entweder ein **winrt::IAsyncXxx** oder [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget). Und anstatt das `return`-Schlüsselwort zu verwenden, um ein asynchrones Objekt zurückzugeben, verwendet eine Coroutine das `co_return`-Schlüsselwort, um den Wert zurückzugeben, den der Aufrufer tatsächlich wünscht (vielleicht eine Datei, ein Bytearray oder einen booleschen Wert).

Wenn eine Methode mindestens eine `co_await`-Anweisung (oder mindestens eine `co_return` oder `co_yield`) enthält, ist die Methode aus diesem Grund eine Coroutine.

Weitere Informationen finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Das Direct3D-Spielbeispiel (**Simple3DGameDX**)

Dieses Thema enthält mehrere exemplarische Vorgehensweisen, die veranschaulichen, wie Sie asynchronen Code schrittweise portieren. Um als Fallstudie fungieren zu können, verwenden wir die C++/CX-Version des [Direct3D-Spielbeispiels](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (das den Namen **Simple3DGameDX** trägt). Wir zeigen Ihnen einige Beispiele dafür, wie Sie den ursprünglichen C++/CX-Quellcode in dieses Projekt übernehmen und seinen asynchronen Code schrittweise zu C++/WinRT portieren können.

- Laden Sie die ZIP-Datei unter dem obigen Link herunter, und entpacken Sie sie.
- Öffnen Sie das C++/CX-Projekt (es befindet sich im Ordner namens `cpp`) in Visual Studio.
- Anschließend müssen Sie dem Projekt C++/WinRT-Unterstützung hinzufügen. Die Schritte, die Sie dazu ausführen müssen, sind in [Erweitern eines bestehenden C++/CX-Projekts um C++/WinRT-Unterstützung](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx#taking-a-ccx-project-and-adding-cwinrt-support) beschrieben. Der Schritt zum Hinzufügen der `interop_helpers.h`-Headerdatei zu Ihem Projekt ist besonders wichtig, da wir in diesem Thema von diesen Hilfsfunktionen abhängig sind.
- Fügen Sie schließlich `#include <pplawait.h>` zu `pch.h` hinzu. Dadurch erhält Ihre Coroutine Unterstützung für PPL (weitere Informationen zu dieser Unterstützung finden Sie im folgenden Abschnitt).

Wenn Sie den Build erstellen, erhalten Sie Fehler, weil **byte** nicht eindeutig ist. So beheben Sie dieses Problem.

- Öffnen Sie `BasicLoader.cpp`, und kommentieren Sie `using namespace std;` aus.
- In derselben Quellcodedatei müssen Sie dann **shared_ptr** als **std::shared_ptr** qualifizieren. Dies können Sie mit einem Suchen-und-Ersetzen-Vorgang in dieser Datei durchführen.
- Qualifizieren Sie **vector** als **std::vector** und **string** als **std::string**.

Das Projekt wird nun erneut erstellt, besitzt C++/WinRT-Unterstützung und enthält die Interoperabilitätshilfsfunktionen **from_cx** und **to_cx**.

Sie haben das **Simple3DGameDX**-Projekt nun fertig eingerichtet, um die weiteren exemplarischen Vorgehensweisen zum Code in diesem Thema nachzuvollziehen.

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>Übersicht über das Portieren von asynchronem C++/CX zu C++/WinRT

Kurz gesagt, werden wir beim Portieren PPL-Aufgabenketten in Aufrufe von `co_await` ändern. Wir ändern den Rückgabewert einer Methode von einer PPL-Aufgabe in entweder ein C++/WinRT **IAsyncXxx**-Objekt oder in ein **IAsyncXxx**\^. Und wir ändern alle **IAsyncXxx**\^ in ein C++/WinRT **IAsyncXxx**.

Sie erinnern sich, dass eine Coroutine jede Methode ist, die `co_xxx` aufruft. Eine C++/WinRT-Coroutine verwendet `co_return`, um ihren Wert zurückzugeben. Dank der Coroutinenunterstützung für PPL(dank `pplawait.h`) können Sie auch `co_return` verwenden, um eine PPL-Aufgabe aus einer Coroutine zurückzugeben. Außerdem können Sie auch `co_await` für sowohl Aufgaben als auch für **IAsyncXxx** verwenden. Aber Sie können nicht `co_return` verwenden, um ein **IAsyncXxx**\^ zurückzugeben. In der folgenden Tabelle wird die Unterstützung für die Interoperabilität zwischen den verschiedenen asynchronen Methoden beschrieben.

|Methode|Kann sie `co_await`en?|Kann von ihr aus `co_return` erfolgen?|
|-|-|-|
|Methode gibt Aufgabe\<void\> zurück.|Ja|Ja|
|Methode gibt Aufgabe\<T\> zurück.|Nein|Ja|
|Methode gibt IAsyncXxx\^ zurück.|Ja|Nein. Aber Sie umschließen eine Aufgabe, die `co_return` verwendet, mit **create_async**.|
|Methode gibt „winrt::IAsyncXxx“ zurück.|Ja|Ja|

Verwenden Sie diese Tabelle, um direkt zu einer Interoperabilitätsmethode Ihrer Wahl zu springen.

|Asynchrone Interoperabilitätsmethode|Abschnitt in diesem Thema|
|-|-|
|Verwenden Sie `co_await`, um auf eine **Aufgabe\<void\>** -Methode aus einer Fire-and-Forget-Methode zu warten.|[Warten auf eine **Aufgabe\<void\>** innerhalb einer Fire-and-Forget-Methode](#await-taskvoid-within-a-fire-and-forget-method)|
|Verwenden Sie `co_await`, um auf eine **Aufgabe\<void\>** -Methode aus einer **Aufgabe\<void\>** -Methode zu warten.|[Warten auf eine **Aufgabe\<void\>** innerhalb einer **Aufgabe\<void\>** -Methode](#await-taskvoid-within-a-taskvoid-method)|
|Verwenden Sie `co_await`, um auf eine **Aufgabe\<void\>** -Methode aus einer **Aufgabe\<T\>** -Methode zu warten.|[Warten auf eine **Aufgabe\<void\>** innerhalb einer **Aufgabe\<T\>** -Methode](#await-taskvoid-within-a-taskt-method)|
|Verwenden Sie `co_await`, um auf eine **IAsyncXxx**^-Methode zu warten.|[Warten auf ein **IAsyncXxx**^ in einer **Aufgabe**-Methode, wobei der Rest des Projekts unverändert bleibt](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Verwenden Sie `co_return` innerhalb einer **Aufgabe\<void\>** -Methode.|[Warten auf eine **Aufgabe\<void\>** innerhalb einer **Aufgabe\<void\>** -Methode](#await-taskvoid-within-a-taskvoid-method)|
|Verwenden von `co_return` innerhalb einer **Aufgabe\<T\>** -Methode|[Warten auf ein **IAsyncXxx**^ in einer **Aufgabe**-Methode, wobei der Rest des Projekts unverändert bleibt](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Umschließen einer Aufgabe, die `co_return` verwendet, mit **create_async**|[Umschließen eine Aufgabe, die `co_return` verwendet, mit **create_async**](#wrap-create_async-around-a-task-that-uses-co_return)|
|Portieren von **concurrency::wait**|[Portieren von **concurrency::wait** zu `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|Zurückgeben von **winrt::IAsyncXxx** anstelle einer **Aufgabe\<void\>** .|[Portieren eines **Aufgabe\<void\>** -Rückgabetyps zu **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|

Im Folgenden finden Sie ein kurzes Codebeispiel, das einen Teil der Unterstützung veranschaulicht.

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that.
    return concurrency::create_async([=]() -> task<bool> {
        co_return true;
    });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

Selbst bei diesen hervorragenden Interoperabilitätsoptionen hängt das schrittweise Portieren von der Auswahl von Änderungen ab, die wir mit chirurgischer Präzision vornehmen können, ohne dass sie sich auf den Rest des Systems auswirken. Wir möchten vermeiden, an einem beliebigen losen Ende zu zupfen und so das gesamte Projekt „aufzuribbeln“. Hierfür müssen wir die Dinge in einer bestimmten Reihenfolge erledigen. Sehen wir uns einige Beispiele für die Vornahme dieser Art von asynchroniebezogenen Portierungsänderungen an.

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>Warten auf eine **Aufgabe\<void\>** -Methode, wobei der Rest des Projekts unverändert bleibt

Eine Methode, die **Aufgabe\<void\>** zurückgibt, führt Arbeit asynchron aus, erzeugt aber letztendlich keinen Wert. Wir können auf eine Methode wie diese `co_await`en.

Ein guter Ausgangspunkt, um asynchronen Code schrittweise zu portieren, ist es also, die Stellen zu finden, an denen Sie diese Methoden aufrufen. Diese Stellen umfassen das Erstellen und/oder Zurückgeben einer Aufgabe und/oder eine Aufgabenkette, bei der von keiner Aufgabe ein Wert an die jeweilige Fortsetzung weitergegeben wird. An solchen Stellen können Sie den asynchronen Code einfach durch `co_await`-Anweisungen ersetzen, wie wir zeigen werden.

Sehen wir uns ein paar Beispiele an. Öffnen Sie das **Simple3DGameDX**-Projekt (siehe [Das Direct3D-Spielbeispiel](#the-direct3d-game-sample-simple3dgamedx)).

> [!IMPORTANT]
> Denken Sie in den folgenden Beispielen daran, wenn Sie sehen, wie die Implementierungen von Methoden geändert werden, dass wir die *Aufrufer* der Methoden, die wir ändern, nicht ändern müssen. Diese Änderungen sind ortsgebunden (lokalisiert) und kaskadieren nicht durch das Projekt.

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>Warten auf eine **Aufgabe\<void\>** innerhalb einer Fire-and-Forget-Methode

Beginnen wir damit, dass wir auf die **Aufgabe\<void\>** innerhalb von *Fire-and-Forget*-Methoden warten, da dies der einfachste Fall ist. Dabei handelt es sich um Methoden, die asynchron arbeiten, aber der Aufrufer der Methode wartet nicht auf den Abschluss dieser Arbeiten. Sie rufen die Methode einfach auf, und vergessen sie, ungeachtet der Tatsache, dass asynchron abgeschlossen wird.

Suchen Sie in Richtung Stamm des Abhängigkeitsdiagramms Ihres Projekts nach `void`-Methoden, die **create_task** und/oder Aufgabenketten enthalten, in denen nur **Aufgabe\<void\>** -Methoden aufgerufen werden.

In **Simple3DGameDX** finden Sie eine Entsprechung in der Implementierung der-Methode **GameMain::Update**. Sie befindet sich in der Quellcodedatei `GameMain.cpp`.

#### <a name="gamemainupdate"></a>**GameMain::Update**

Hier sehen Sie einen Auszug aus der C++/CX-Version der Methode, in dem die beiden Teile gezeigt werden, die asynchron abgeschlossen werden.

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

Sie sehen einen Aufruf der **Simple3DGame::LoadLevelAsync**-Methode sehen (die eine PPL-**Aufgabe\<void\>** ) zurückgibt. Danach befindet sich eine *Fortsetzung*, die synchrone Arbeiten durchführt. **LoadLevelAsync** ist asynchron, gibt aber keinen Wert zurück. Daher wird kein Wert von der Aufgabe an die Fortsetzung weitergegeben.

Wir können den Code an diesen beiden Stellen auf dieselbe Weise ändern. Der Code wird im Anschluss an das nachfolgenden Listing erläutert. Wir könnten an dieser Stelle die sichere Methode für den Zugriff auf den *this*-Zeiger in einer Klassenember-Coroutine besprechen. Doch wir verschieben dies auf einen späteren Abschnitt. Vorerst funktioniert der Code.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

Wie Sie sehen können, können wir , weil **LoadLevelAsync** eine Aufgabe zurückgibt, es `co_await`. Und wir benötigen keine explizite Fortsetzung – der Code, der auf eine `co_await` folgt, wird nur ausgeführt, wenn **LoadLevelAsync** abgeschlossen ist.

Durch die Einführung von `co_await` wird die Methode in eine Coroutine umgewandelt, sodass sie nicht `void` zurückgeben kann. Es handelt sich um eine Fire-and-Forget-Methode, sodass wir [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) zurückgeben.

Sie müssen außerdem den Rückgabetyp von **GameMain::Update** von `void` in **winrt::fire_and_forget** in der Deklaration in `GameMain.h` ändern.

Sie können diese Änderung an Ihrer Kopie des Projekts vornehmen, woraufhin das Spiel immer noch erstellt und unverändert ausgeführt wird. Der Quellcode ist nach wie vor grundsätzlich C++/CX, aber er verwendet nun dieselben Muster wie C++/WinRT, sodass wir der Fähigkeit zum automatischen Portieren des Rests des Codes etwas nähergekommen sind.

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** ist eine weitere Fire-and-Forget-Methode, die ebenfalls **LoadLevelAsync** aufruft. Somit können wir hier dieselbe Änderung vornehmen.

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

Die Sache wird etwas interessanter in **GameMain::OnDeviceRestored** wegen seiner tieferen Schachtelung von asynchronem Code, einschließlich einer No-Op-Aufgabe. Im Folgenden finden Sie eine Gliederung der asynchronen Teile der Methode (mit dem durch Ellipsen dargestellten weniger interessanten synchronen Code).

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

Ändern Sie zunächst den Rückgabetyp von **GameMain::OnDeviceRestored** von `void` in **winrt::fire_and_forget** innerhalb von `GameMain.h` und `.cpp`. Sie müssen außerdem `DeviceResources.h` öffnen und dieselbe Änderung am Rückgabetyp **IDeviceNotify::OnDeviceRestored** vornehmen.

Um den asynchronen Code zu portieren, entfernen Sie alle **create_task**- und **then**-Aufrufe mit deren geschweiften Klammern, und vereinfachen Sie die Methode in eine vereinfachte Reihe von Anweisungen.

Ändern Sie alle `return` (die eine Aufgabe zurückgeben) in eine `co_await`. Es bleibt ein `return` zurück, das nichts zurückgibt, weshalb Sie es einfach löschen. Wenn Sie fertig sind, ist die No-Op-Aufgabe nicht mehr vorhanden, und die Gliederung der asynchronen Teile der Methode sieht wie folgt aus. Wiederum wird der weniger interessante synchrone Code durch Ellipsen dargestellt.

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Wie Sie sehen können, ist er wesentlich einfacher und leichter zu lesen.

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

Der **GameMain::GameMain**-Konstruktor führt die Arbeit asynchron aus, und kein Teil des Projekts wartet darauf, dass eine Aufgabe abgeschlossen wird. In diesem Listing finden Sie wiederum eine Gliederung der asynchronen Teile.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

Ein Konstruktor kann jedoch keine **winrt::fire_and_forget** zurückgeben, weshalb wird den asynchronen Code in eine neue **GameMain::ConstructInBackground**-Fire-and-Forget-Methode verschieben, den Code zu `co_await`-Anweisungen vereinfachen und die neue Methode aus dem Konstruktor aufrufen.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Somit wurden nun alle Fire-and-Forget-Methoden – tatsächlich der gesamte asynchrone Code wurde – in **GameMain** in Coroutinen umgewandelt. Wenn Sie möchten, könnten Sie in anderen Klassen nach Fire-and-Forget-Methoden suchen und ähnliche Änderungen vornehmen.

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>Die verschobene Besprechung von `co_await` und dem *this*-Zeiger

Als wir Änderungen an **GameMain::Update** vorgenommen haben, habe ich eine Besprechung des *this*-Zeigers verschoben. Lassen Sie und dies nun nachholen.

Dies gilt für alle Methoden, die wir bisher geändert haben. Ferner gilt es auch für *alle* Coroutinen, nicht nur für Fire-and-Forget-Coroutinen. Durch die Einführung einer `co_await` in eine Methode wird ein *Anhaltepunkt* eingeführt. Und aus diesem Grund müssen wir mit dem *this*-Zeiger vorsichtig sein, den wir natürlich *hinter* dem Anhaltepunkt verwenden werden, und zwar jedes Mal, wenn wir auf einen Klassenmember zugreifen.

Die kurze Version lautet, dass die Lösung [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) aufgerufen wird. Doch eine vollständige Behandlung des Problems und seiner Lösung finden Sie unter [Sicherer Zugriff auf den *this*-Zeiger in einer Klassenmember-Coroutine](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

Sie können **implements::get_strong** nur in einer Klasse verwenden, die von [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) abgeleitet ist.

#### <a name="derive-gamemain-from-winrtimplements"></a>Ableiten von **GameMain** von **winrt::implements**

Die erste Änderung, die wir vornehmen müssen, erfolgt in `GameMain.h`.

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain** implementiert weiterhin **DX::IDeviceNotify**, aber wir ändern es dahingehende, dass es von [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) abgeleitet wird.

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

Als Nächstes suchen Sie in `App.cpp` diese Methode.

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

Nachdem nun aber **GameMain** von **winrt::implements** abgeleitet wird, müssen wir es auf andere Weise erstellen. In diesem Fall verwenden wir die Funktionsvorlage [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). Weitere Informationen finden Sie unter [Instanziierung und Rückgabe von Implementierungstypen und Schnittstellen](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

Um die Schleife bei dieser Änderung zu schließen, müssen wir auch den Typ von *m_main* ändern. Suchen Sie in `App.h` diesen Code.

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

Ändern Sie die Deklaration von *m_main* in diese.

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>Wir können jetzt **implements::get_strong** aufrufen.

Für **GameMain::Update** sowie für jede der anderen Methoden, der wir ein `co_await` hinzugefügt haben, erfahren Sie hier, wie Sie **get_strong** am Anfang einer Coroutine aufrufen können, um sicherzustellen, dass ein starker Verweis so lange überdauert, bis die Coroutine abgeschlossen ist.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>Warten auf eine **Aufgabe\<void\>** innerhalb einer **Aufgabe\<void\>** -Methode

Der zweit einfachste Fall ist das Warten auf die **Aufgabe\<void\>** in einer Methode, die wiederum selbst eine **Aufgabe\<void\>** zurückgibt, da wir auf eine **Aufgabe\<void\>** `co_await`en und wir aus einer Aufgabe `co_return` können.

Ein besonders einfaches Beispiel finden Sie in der Implementierung der Methode **Simple3DGame::LoadLevelAsync**. Sie befindet sich in der Quellcodedatei `Simple3DGame.cpp`.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

Es gibt nur ein bisschen synchronen Code, gefolgt von der Rückgabe der Aufgabe, die von **GameRenderer::LoadLevelResourcesAsync** erstellt wurde.

Anstatt diese Aufgabe zurückzugeben, `co_await`en wir auf sie, und `co_return` dann das resultierende `void`.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

### <a name="await-taskvoid-within-a-taskt-method"></a>Warten auf eine **Aufgabe\<void\>** innerhalb einer **Aufgabe\<T\>** -Methode

Zwar gibt es keine geeigneten Beispiele in **Simple3DGameDX-** , doch können wir uns ein hypothetisches Beispiel ausdenken, nur um das Muster zu veranschaulichen.

Das folgende Codebeispiel veranschaulicht lediglich das einfache `co_await` einer **Aufgabe\<void\>** . Dann, um den **Aufgabe\<T\>** -Rückgabetyp zu erfüllen, geben wir asynchron ein **StorageFile\^** mittels co_awaiting einer Windows-Runtime-Methode zurück und `co_return` die resultierende Datei.

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

Der nächste Schritt hierbei bestünde darin, mehr von der Methode zu C++/WinRT zu portieren, und zwar wie folgt.

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

Der *m_renderer*-Datenmember ist noch in C++/CX in diesem Beispiel.

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>Warten auf ein **IAsyncXxx**^ in einer **Aufgabe**-Methode, wobei der Rest des Projekts unverändert bleibt

Wir haben gesehen, wie Sie eine **Aufgabe\<void\>** `co_await`en können. Sie können auch eine Methode `co_await`en, die **IAsyncXxx** zurückgibt, unabhängig davon, ob es sich um eine Methode in Ihrem Projekt oder um eine asynchrone Windows-API (z. B. [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync)) handelt.

Ein Beispiel für die Stelle, an der wir diese Codeänderung vornehmen können, finden wir bei **BasicReaderWriter::ReadDataAsync** (implementiert in `BasicReaderWriter.cpp`). Hier ist die ursprüngliche C++/CX-Version.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Wir können nicht nur Windows-APIs `co_await`en, sondern wir können auch `co_return` den Wert, den die Methode asynchron zurückgibt (in diesem Fall ein Bytearray). Der erste Schritt zeigt, wie Sie nur diese Änderungen vornehmen. Die tatsächliche Portierung des C++/CX-Codes zu C++/WinRT erfolgt im nächsten Abschnitt.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

Auch hier müssen wir die *Aufrufer* der Methoden, die wir ändern, nicht ändern, da wir den Rückgabetyp nicht geändert haben.

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>Portieren von **ReadDataAsync** (das Meiste) zu C++/WinRT, wobei der Rest des Projekts unverändert bleibt

Wir können noch einen Schritt weitergehen und die Methode *fast vollständig* zu C++/WinRT portieren, ohne jeglichen anderen Teil des Projekts ändern zu müssen.

Die einzige Abhängigkeit, die diese Methode vom Rest des Projekts besitzt, ist der **BasicReaderWriter::m_location**-Datenmember, der ein C++/CX-**StorageFolder**^ ist. Um den Datenmember sowie den Parametertyp und den Rückgabetyp unverändert zu lassen, müssen wir nur ein paar Konvertierungen durchführen, eine am Anfang der Methode und eine am Ende der Methode. Hierfür können wir die Interoperabilitätshilfsfunktionen **from_cx** und **to_cx** verwenden.

Im Folgenden wird erläutert, wie **BasicReaderWriter::ReadDataAsync** aussieht, nachdem seine Implementierung vorwiegend zu C++/WinRT implementiert wurde. Dies ist ein gutes Beispiel für das *schrittweise Portieren*. Und diese Methode befindet sich in der Phase, in der wir uns nun von ihrer Betrachtung als *einer C++/CX-Methode, die C++/WinRT-Methoden verwendet*, lösen können, um sie als *eine C++/WinRT-Methode zu betrachten, die mit C++/CX interagiert*.

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> In **ReadDataAsync** haben wir ein neues C++/CX-Array erstellt und zurückgegeben. Und natürlich haben wir dies getan, um den Rückgabetyp der Methode zu erfüllen (damit wir nicht den Rest des Projekts ändern mussten).
>
> Ihnen können weitere Beispiele in Ihrem eigenen Projekt begegnen, in denen Sie nach dem Portieren das Ende der Methode erreichen, und alles, was vorhanden ist, ist ein C++/WinRT-Objekt. Um dieses zu `co_return`, rufen Sie einfach **to_cx** auf, um es zu konvertieren. Hier sehen Sie ein hypothetisches Beispiel.

```cppcx
task<Windows::Storage::StorageFile^> RetrieveFileAsync()
{
    winrt::Windows::Storage::StorageFile storageFile = co_await ...
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>Umschließen einer Aufgabe, die `co_return` verwendet, mit **create_async**

Sie können eine **IAsyncXxx**\^ nicht direkt `co_return`, aber Sie können etwas Ähnliches erreichen. Wenn Sie eine Aufgabe haben, die `co_return` verwendet, können Sie diese mit [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) umschließen.

Im Folgenden sehen Sie ein hypothetisches Beispiel, da es kein Beispiel gibt, das wir aus **Simple3DGameDX** verwenden können.

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ Simple3DGame::ReturnBooleanAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await BooleanMemberFunctionAsync();
            co_return result;
        });
}
```

Wie Sie sehen, könnten Sie den Rückgabewert von jeder Methode abrufen, die Sie `co_await`en können.

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>Portieren von **concurrency::wait** zu `co_await winrt::resume_after`

Es gibt mehrere Stellen, an denen **Simple3DGameDX** [**concurrency:: wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) verwendet, um den Thread für einen kurzen Zeitraum anzuhalten. Hier sehen Sie ein Beispiel.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

Die C++/WinRT-Version von **concurrency::wait** ist die **winrt::resume_after**-Struktur. Wir können diese Struktur in einer PPL-Aufgabe `co_await`en. Hier sehen Sie ein Codebeispiel.

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

Beachten Sie die beiden anderen Änderungen, die wir vornehmen mussten. Wir haben den Typ von **GameConstants::InitialLoadingDelay** in **std::chrono::duration** geändert, und wir haben den Rückgabetyp der Lambda-Funktion explizit gemacht, weil der Compiler ihn nicht mehr ableiten kann.

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>Portieren eines **Aufgabe\<void\>** -Rückgabetyps zu **winrt::IAsyncXxx**

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

In dieser Phase unserer Arbeit mit **Simple3DGameDX** wird an allen Stellen im Projekt, an denen **Simple3DGame::LoadLevelAsync** aufgerufen wird, `co_await` zum Aufrufen davon verwendet.

Dies bedeutet, dass wir den Rückgabetyp dieser Methode von **Aufgabe\<void\>** in **winrt::Windows::Foundation::IAsyncAction** ändern können.

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

Nun sollte es recht automatisch ablaufen, um den Rest dieser Methode mit ihren Abhängigkeiten zu C++/WinRT zu portieren.

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

Hier ist die ursprüngliche C++/CX-Version von **GameRenderer::LoadLevelResourcesAsync**.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync** ist die einzige Stelle im Projekt, an der **GameRenderer::LoadLevelResourcesAsync** aufgerufen wird, und es wird dort bereits `co_await` zum Aufrufen verwendet.

Es ist also nicht mehr erforderlich, dass **GameRenderer::LoadLevelResourcesAsync** eine Aufgabe zurückgibt – es kann stattdessen eine **winrt::Windows::Foundation::IAsyncAction** zurückgeben. Und die Implementierung selbst ist einfach genug, um sie vollständig zu C++/WinRT zu portieren. Dies umfasst die Vornahme derselben Änderung, die wir in „[Portieren von **concurrency::wait** zu `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)„ vorgenommen haben. Und es gibt keine signifikanten Abhängigkeiten vom Rest des Projekts, um die Sie sich Gedanken machen müssten.

Somit sieht die Methode also wie folgt aus, nachdem sie vollständig zu C++/WinRT portiert wurde.

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="basicreaderwriterreaddataasync"></a>**BasicReaderWriter::ReadDataAsync**

Schließen wir nun diese exemplarische Vorgehensweise ab, indem wir **BasicReaderWriter::ReadDataAsync** vollständig zu C++/WinRT portieren. Das letzte Mal, als wir es uns angesehen haben (in „[Portieren von **ReadDataAsync** (das Meiste) zu C++/WinRT, wobei der Rest des Projekts unverändert bleibt](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)“]), war es größtenteils zu C++/WinRT portiert. Es gab aber immer noch eine Aufgabe von **Platform::Array\<byte\>** ^ zurück.

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

Anstatt eine Aufgabe zurückzugeben, ändern wir dies so, dass asynchron ein C++/WinRT [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer)-Objekt zurückgegeben wird, wenn dies auch bedeutet, dass der Code an allen Aufrufstellen geändert werden muss.

Im Folgenden sehen Sie, wie die Methode nach der Portierung ihrer Implementierung, ihrer Parameter und des *m_location*-Datenmembers aussieht, um C++/WinRT-Syntax und -Objekte zu verwenden.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

Wie Sie sehen können, ist **BasicReaderWriter::ReadDataAsync** selbst wesentlich einfacher, da wir die synchrone Logik, die Bytes aus dem Puffer abruft, in seine eigene Methode einbezogen haben.

Doch nun müssen wir die Aufrufstellen aus dieser Art von Struktur zu C++/CX portieren.

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

In dieses Muster in C++/WinRT.

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>Wichtige APIs

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction),
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1) und
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Zugehörige Themen

* [Umstellen von C++/CX auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [Parallelität und asynchrone Vorgänge mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [Starke und schwache Verweise in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references)
* [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
