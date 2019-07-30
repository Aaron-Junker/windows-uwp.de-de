---
description: In diesem Thema werden Strategien zur Behandlung von Fehlern bei der Programmierung mit C++/WinRT erörtert.
title: Fehlerbehandlung bei C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, fehler, behandlung, ausnahme
ms.localizationpriority: medium
ms.openlocfilehash: 37819d1626d3adc6f5647f447567a9273e72668d
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270133"
---
# <a name="error-handling-with-cwinrt"></a>Fehlerbehandlung bei C++/WinRT

In diesem Thema werden Strategien zur Behandlung von Fehlern bei der Programmierung mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) erörtert. Weitere allgemeine Informationen sowie Hintergrundinformationen finden Sie unter [Behandeln von Fehlern und Ausnahmen (Modernes C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Vermeiden des Abfangens und Auslösens von Ausnahmen
Es wird empfohlen, weiterhin [ausnahmesicheren Code](/cpp/cpp/how-to-design-for-exception-safety) zu schreiben, aber auch nach Möglichkeit zu vermeiden, dass Ausnahmen abgefangen und ausgelöst werden. Wenn kein Handler für eine Ausnahme vorhanden ist, generiert Windows automatisch einen Fehlerbericht (einschließlich eines Minidumps des Absturzes). Dieser hilft Ihnen dabei, zu ermitteln, wo das Problem liegt.

Lösen Sie keine Ausnahme aus, die erwartungsgemäß abgefangen wird. Und verwenden Sie keine Ausnahmen für erwartete Fehler. Lösen Sie eine Ausnahme *nur dann aus, wenn ein unerwarteter Laufzeitfehler auftritt*, und behandeln Sie alles andere mit Fehler-/Ergebniscodes – direkt und in der Nähe der Fehlerquelle. So wissen Sie, wenn eine Ausnahme *ausgelöst wird*, dass die Ursache entweder ein Fehler im Code oder ein außergewöhnlicher Fehlerstatus im System ist.

Betrachten Sie das Szenario beim Zugreifen auf die Windows-Registrierung. Wenn Ihre App einen Wert aus der Registrierung nicht lesen kann, ist dies zu erwarten und Sie sollten dies ordnungsgemäß handhaben. Lösen Sie keine Ausnahme aus. Geben Sie stattdessen einen `bool`- oder `enum`-Wert zurück, der darauf hinweist, dass der Wert nicht gelesen wurde, und ggf. auch den Grund dafür angibt. Kann ein Wert nicht in die Registrierung *geschrieben* werden, weist dies wahrscheinlich darauf hin, dass ein größeres Problem vorliegt, als Sie sinnvoll in Ihrer Anwendung behandeln könnten. In solch einem Fall sollte Ihre Anwendung nicht fortgesetzt werden. Folglich ist eine Ausnahme, die zu einem Fehlerbericht führt, die schnellste Möglichkeit zu verhindern, dass Ihre Anwendung Schäden verursacht.

Stellen Sie sich als weiteres Beispiel vor, dass ein Miniaturbild von einem Aufruf von [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) abgerufen und dann an [**BitmapSource.SetSourceAsync** ](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_) übergeben wird. Wenn diese Abfolge von Aufrufen bewirkt, dass `nullptr` an **SetSourceAsync** übergeben wird (die Bilddatei kann nicht gelesen werden; z. B. könnte die Dateierweiterung darauf hinweisen, dass sie Bilddaten enthält, was tatsächlich aber nicht der Fall ist), dann lösen Sie eine Ausnahme für einen ungültigen Zeiger aus. Wenn Sie einen solchen Fall in Ihrem Code entdecken, sollten Sie überprüfen, ob `nullptr` von **GetThumbnailAsync** zurückgegeben wurde, statt den Fall als Ausnahme abzufangen und zu behandeln.

Das Auslösen von Ausnahmen ist für gewöhnlich langsamer als die Verwendung von Fehlercodes. Wenn Sie eine Ausnahme nur bei schwerwiegenden Fehlern auslösen, geht dies, wenn alles gut läuft, nie zu Lasten der Leistung.

Die Leistung wird eher beeinträchtigt durch den Laufzeit-Overhead, der entsteht, wenn sichergestellt werden muss, dass die richtigen Destruktoren im unwahrscheinlichen Falle aufgerufen werden, dass eine Ausnahme ausgelöst wird. Die Leistungseinbußen, die mit dieser Sicherstellung einhergehen, treten unabhängig davon auf, ob tatsächlich eine Ausnahme ausgelöst wird oder nicht. Daher sollten Sie dafür sorgen, dass der Compiler weiß, welche Funktionen potenziell Ausnahmen auslösen können. Wenn der Compiler nachweisen kann, dass keine Ausnahmen von bestimmten Funktionen ausgelöst werden (`noexcept`-Spezifikation), kann er den generierten Code optimieren.

## <a name="catching-exceptions"></a>Abfangen von Ausnahmen
Ein Fehlerzustand, der auf Ebene der [Windows-Runtime-ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) auftritt, wird in Form eines HRESULT-Werts zurückgegeben. Sie müssen HRESULTs jedoch nicht in Ihrem Code behandeln. Der C++/WinRT-Projektionscode, der für eine API auf Nutzungsseite generiert wird, erkennt einen HRESULT-Fehlercode auf der ABI-Ebene und konvertiert den Code in eine [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)-Ausnahme, die Sie abfangen und behandeln können. Wenn Sie HRESULTS behandeln *möchten*, verwenden Sie den Typ **winrt::hresult**.

Wenn der Benutzer beispielsweise ein Bild aus der Bildbibliothek löscht, während die Anwendung diese Sammlung durchläuft, wird von der Projektion eine Ausnahme ausgelöst. Dies ist ein Fall, in dem Sie diese Ausnahme abfangen und behandeln müssen. Dieser Fall wird im folgenden Codebeispiel veranschaulicht.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Verwenden Sie dieses Muster in einer Coroutine beim Aufrufen einer `co_await`-ed-Funktion. Ein weiteres Beispiel für diese Konvertierung von HRESULT in eine Ausnahme ist, wenn eine Komponenten-API E_OUTOFMEMORY zurückgibt, wodurch eine **std::bad_alloc**-Ausnahme ausgelöst wird.

## <a name="throwing-exceptions"></a>Auslösen von Ausnahmen
Es gibt Fälle, in denen Sie entscheiden, dass, wenn Ihr Aufruf einer bestimmten Funktion fehlschlägt, Ihre Anwendung nicht mehr wiederhergestellt werden kann (Sie könnten sich dann nicht mehr darauf verlassen, dass die Anwendung wie erwartet funktioniert). Im Codebeispiel unten wird ein [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle)-Wert als Wrapper für den HANDLE verwendet, der von [**CreateEvent**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-createeventa) zurückgegeben wird. Anschließend wird der Handle (es wird daraus ein `bool`-Wert erstellt) an die Funktionsvorlage [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) übergeben. **winrt::check_bool** funktioniert mit einem `bool`-Wert oder mit einem beliebigen Wert, der in `false` (Fehler) oder `true` (Erfolg) konvertiert werden kann.

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Wenn der Wert, den Sie an [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) übergeben, falsch ist, findet die folgende Abfolge von Aktionen statt.

- **winrt::check_bool** ruft die Funktion [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) auf.
- **winrt::throw_last_error** ruft [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) auf, um den letzten Fehlercodewert des aufrufenden Threads abzurufen, und dann wird die Funktion [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) aufgerufen.
- **winrt::throw_hresult** löst eine Ausnahme unter Verwendung eines [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)-Objekts (oder eines Standardobjekts) aus, das diesen Fehlercode darstellt.

Da Windows-APIs Laufzeitfehler mit verschiedenen Rückgabewerttypen melden, stehen zusätzlich zu **winrt::check_bool** einige weitere nützliche Hilfsfunktionen zur Verfügung, um Werte zu überprüfen und Ausnahmen auszulösen.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Überprüft, ob der HRESULT-Code einen Fehler darstellt. Ist dies der Fall, wird **winrt::throw_hresult** aufgerufen.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Überprüft, ob ein Code einen Fehler darstellt. Ist dies der Fall, wird **winrt::throw_hresult** aufgerufen.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Überprüft, ob ein Zeiger null ist. Ist dies der Fall, wird **winrt::throw_last_error** aufgerufen.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Überprüft, ob ein Code einen Fehler darstellt. Ist dies der Fall, wird **winrt::throw_hresult** aufgerufen.

Sie können diese Hilfsfunktionen für gängige Rückgabecodetypen verwenden oder auf eine Fehlerbedingung reagieren und entweder [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) oder [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) aufrufen. 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Auslösen von Ausnahmen beim Erstellen einer API
Alle [Windows Runtime Application Binary Interface](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types)-Grenzen (oder ABI-Grenzen) müssen als *noexcept* definiert sein, d.h. Ausnahmen dürfen nicht unbehandelt bleiben. Wenn du eine API erstellst, solltest du die ABI-Grenze immer mit dem C++-Schlüsselwort `noexcept` markieren. `noexcept` weist in C++ ein bestimmtes Verhalten auf. Wenn eine C++-Ausnahme eine `noexcept`-Grenze erreicht, tritt beim Prozess sofort ein Fail-Fast mit **std::terminate** auf. Dieses Verhalten ist im Allgemeinen wünschenswert, weil ein Ausnahmefehler fast immer auf einen unbekannten Fehler im Prozess hinweist.

Da Ausnahmen die ABI-Grenze nicht überschreiten dürfen, wird eine Fehlerbedingung, die in einer Implementierung auftritt, in Form eines HRESULT-Fehlercodes über die ABI-Ebene zurückgegeben. Wenn Sie eine API mit C++/WinRT erstellen, wird Code für Sie generiert, um jede Ausnahme, die Sie in Ihrer Implementierung *auslösen*, in ein HRESULT zu konvertieren. Die Funktion [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) wird in diesem generierten Code in einem (wie im folgenden Beispiel gezeigten) Muster verwendet.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) behandelt Ausnahmen, die von **std::exception** und [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) und deren abgeleiteten Typen abgeleitet wurden. In Ihrer Implementierung sollten Sie **winrt::hresult_error** oder einen abgeleiteten Typ bevorzugen, damit die Benutzer Ihrer API umfassende Fehlerinformationen erhalten. **std::exception** (die E_FAIL zugeordnet ist) wird unterstützt, wenn sich Ausnahmen aus der Verwendung der Standardvorlagenbibliothek ergeben.

### <a name="debuggability-with-noexcept"></a>Debuggingfähigkeit mit „noexcept“
Wie oben erwähnt, tritt bei einer C++-Ausnahme, die eine `noexcept`-Grenze erreicht, sofort ein Fail-Fast mit **std::terminate** auf. Das ist für das Debuggen nicht ideal, weil **std::terminate** häufig sehr viel oder den gesamten ausgegebenen Fehler- oder Ausnahmekontext verliert, insbesondere dann, wenn Coroutinen beteiligt sind.

In diesem Abschnitt geht es daher um den Fall, dass deine ABI-Methode (die du ordnungsgemäß mit `noexcept` markiert hast) `co_await` verwendet, um asynchronen C++/WinRT-Projektionscode aufzurufen. Es empfiehlt sich, die Aufrufe an den C++/WinRT-Projektionscode mit **winrt::fire_and_forget** zu umschließen. Auf diese Weise erfolgt eine ordnungsgemäße Erfassung von Ausnahmefehlern als Stowed-Ausnahme, wodurch die Debuggingfähigkeit beträchtlich verbessert wird.

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget** verfügt über eine integrierte `unhandled_exception`-Methodenhilfsklasse, die **winrt::terminate** aufruft, wodurch wiederum **RoFailFastWithErrorContext** aufgerufen wird. So wird garantiert, dass jeglicher Kontext (Stowed-Ausnahme, Fehlercode, Fehlermeldung, Stapelbacktrace usw.) entweder für das Livedebugging oder für ein Post-Mortem-Speicherabbild beibehalten wird. Aus praktischen Gründen kannst du den fire-and-forget-Teil in eine separate Funktion einbeziehen, die **winrt::fire_and_forget** zurückgibt und anschließend aufruft.

### <a name="synchronous-code"></a>Synchroner Code
In einigen Fällen ruft die ABI-Methode (die wiederum ordnungsgemäß mit `noexcept` markiert ist) nur synchronen Code auf. Anders gesagt: Sie verwendet niemals`co_await`, weder zum Aufrufen einer asynchronen Windows-Runtime-Methode noch zum Wechseln zwischen Vordergrund- und Hintergrundthreads. In diesem Fall funktioniert die fire_and_forget-Technik zwar noch, ist aber nicht effizient. Stattdessen kannst du Folgendes einrichten.

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>Fail-Fast
Der Code im vorherigen Abschnitt führt weiterhin einen Fail-Fast aus. Wie bereits gesagt, behandelt der Code keine Ausnahmen. Jeglicher Ausnahmefehler führt zu einem Programmabbruch.

Diese Form ist jedoch besser, weil sie die Debuggingfähigkeit sicherstellt. In seltenen Fällen musst du `try/catch` verwenden, um bestimmte Ausnahmen zu behandeln. Das sollte aber wirklich nur selten verwendet werden, da wir – wie in diesem Thema erläutert – davon abraten, für erwartete Bedingungen Ausnahmen als Mechanismus zur Flusssteuerung zu verwenden.

Denk daran, dass es keine gute Idee ist, eine Ausnahme in einem reinen `noexcept`-Kontext unbehandelt zu lassen. Unter dieser Bedingung verwendet die C++-Runtime **std::terminate** für den Prozess, sodass sämtliche Informationen zu Stowed-Ausnahmen verloren gehen, die von C++/WinRT erfasst wurden.

## <a name="assertions"></a>Assertionen
Für interne Annahmen in Ihrer Anwendung gibt es Assertionen. Bevorzugen Sie nach Möglichkeit **static_assert** für die Überprüfung während der Kompilierung. Verwenden Sie für Laufzeitbedingungen `WINRT_ASSERT` mit einem booleschen Ausdruck. `WINRT_ASSERT` ist eine Makrodefinition, die auf [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) erweitert wird.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT wird bei Releasebuilds kompiliert. In einem Debugbuild beendet WINRT_ASSERT die Anwendung im Debugger in der Codezeile, in der sich die Assertion befindet.

Sie sollten keine Ausnahmen in Ihren Destruktoren verwenden. Folglich können Sie zumindest in Debugbuilds das Ergebnis des Aufrufs einer Funktion von einem Destruktor mit WINRT_VERIFY (mit einem booleschen Ausdruck) und WINRT_VERIFY_ (mit dem erwarteten Ergebnis und einem booleschen Ausdruck) bestätigen.

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>Wichtige APIs
* [winrt::check_bool-Funktionsvorlage](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_nt-Funktionsvorlage](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer-Funktionsvorlage](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32-Funktionsvorlage](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle-Struktur](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error-Struktur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error-Funktion](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Verwandte Themen
* [Behandeln von Fehlern und Ausnahmen (Modernes C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Vorgehensweise: Entwurf für die Ausnahmesicherheit](/cpp/cpp/how-to-design-for-exception-safety)
