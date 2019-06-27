---
description: In diesem Thema werden Strategien zur Behandlung von Fehlern bei der Programmierung mit C++/WinRT erörtert.
title: Fehlerbehandlung bei C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, fehler, behandlung, ausnahme
ms.localizationpriority: medium
ms.openlocfilehash: 1b72bb3cb2527585c114d386981e02d4730614a2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66721642"
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
Da eine Ausnahme nicht die Grenze der [Windows-Runtime-ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) überschreiten darf, wird ein Fehlerzustand, der in einer Implementierung auftritt, über die ABI-Ebene in Form eines HRESULT-Fehlercodes zurückgegeben. Wenn Sie eine API mit C++/WinRT erstellen, wird Code für Sie generiert, um jede Ausnahme, die Sie in Ihrer Implementierung *auslösen*, in ein HRESULT zu konvertieren. Die Funktion [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) wird in diesem generierten Code in einem (wie im folgenden Beispiel gezeigten) Muster verwendet.

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

## <a name="assertions"></a>Assertionen
Für interne Annahmen in Ihrer Anwendung gibt es Assertionen. Bevorzugen Sie nach Möglichkeit **static_assert** für die Überprüfung während der Kompilierung. Verwenden Sie für Laufzeitbedingungen WINRT_ASSERT mit einem booleschen Ausdruck.

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
