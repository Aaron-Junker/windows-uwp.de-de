---
description: In diesem Thema werden Strategien zur Behandlung von Fehlern bei der Programmierung mit C++/WinRT erörtert.
title: Fehlerbehandlung bei C++/WinRT
ms.date: 05/21/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, fehler, behandlung, ausnahme
ms.localizationpriority: medium
ms.openlocfilehash: c6f7135e85ab63ddfe92bd0de8c656b58fb1a020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626575"
---
# <a name="error-handling-with-cwinrt"></a>Fehlerbehandlung bei C++/WinRT

In diesem Thema wird erläutert, die Strategien zur Behandlung von Fehlern beim Programmieren mit [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Weitere allgemeine Informationen sowie Hintergrundinformationen finden Sie unter [Fehler- und Ausnahmebehandlung (modernes C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Vermeiden des Abfangens und Auslösens von Ausnahmen
Es wird empfohlen, weiterhin [ausnahmesicheren Code](/cpp/cpp/how-to-design-for-exception-safety) zu schreiben, aber auch nach Möglichkeit zu vermeiden, dass Ausnahmen abgefangen und ausgelöst werden. Wenn kein Handler für eine Ausnahme vorhanden ist, generiert Windows automatisch einen Fehlerbericht (einschließlich eines Minidumps des Absturzes). Dieser hilft Ihnen dabei, zu ermitteln, wo das Problem liegt.

Lösen Sie keine Ausnahme aus, die erwartungsgemäß abgefangen wird. Und verwenden Sie keine Ausnahmen für erwartete Fehler. Lösen Sie eine Ausnahme *nur dann aus, wenn ein unerwarteter Laufzeitfehler auftritt*, und behandeln Sie alles andere mit Fehler-/Ergebniscodes – direkt und in der Nähe der Fehlerquelle. So wissen Sie, wenn eine Ausnahme *ausgelöst wird*, dass die Ursache entweder ein Fehler im Code oder ein außergewöhnlicher Fehlerstatus im System ist.

Betrachten Sie das Szenario beim Zugreifen auf die Windows-Registrierung. Wenn Ihre App einen Wert aus der Registrierung nicht lesen kann, ist dies zu erwarten und Sie sollten dies ordnungsgemäß handhaben. Lösen Sie keine Ausnahme aus. Geben Sie stattdessen einen `bool`- oder `enum`-Wert zurück, der darauf hinweist, dass der Wert nicht gelesen wurde, und ggf. auch den Grund dafür angibt. Kann ein Wert nicht in die Registrierung *geschrieben* werden, weist dies wahrscheinlich darauf hin, dass ein größeres Problem vorliegt, als Sie sinnvoll in Ihrer Anwendung behandeln könnten. In solch einem Fall sollte Ihre Anwendung nicht fortgesetzt werden. Folglich ist eine Ausnahme, die zu einem Fehlerbericht führt, die schnellste Möglichkeit zu verhindern, dass Ihre Anwendung Schäden verursacht.

Stellen Sie sich als weiteres Beispiel vor, dass ein Miniaturbild von einem Aufruf an [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) abgerufen und an [**BitmapSource.SetSourceAsync** ](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_) übergeben wird. Wenn diese Abfolge von Aufrufen bewirkt, dass `nullptr` an **SetSourceAsync** übergeben wird (die Bilddatei kann nicht gelesen werden; z. B. könnte die Dateierweiterung darauf hinweisen, dass sie Bilddaten enthält, was tatsächlich aber nicht der Fall ist), dann lösen Sie eine Ausnahme für einen ungültigen Zeiger aus. Wenn Sie einen solchen Fall in Ihrem Code entdecken, sollten Sie, anstatt den Fall als Ausnahme abzufangen und zu handhaben, stattdessen prüfen, ob `nullptr` von **GetThumbnailAsync**  zurückgegeben wurde.

Das Auslösen von Ausnahmen ist für gewöhnlich langsamer als die Verwendung von Fehlercodes. Wenn Sie eine Ausnahme nur bei schwerwiegenden Fehlern auslösen, geht dies, wenn alles gut läuft, nie zu Lasten der Leistung.

Die Leistung wird eher beeinträchtigt durch den Laufzeit-Overhead, der entsteht, wenn sichergestellt werden muss, dass die richtigen Destruktoren im unwahrscheinlichen Falle aufgerufen werden, dass eine Ausnahme ausgelöst wird. Die Leistungseinbußen, die mit dieser Sicherstellung einhergehen, treten unabhängig davon ein, ob tatsächlich eine Ausnahme ausgelöst wird oder nicht. Daher sollten Sie dafür sorgen, dass der Compiler weiß, welche Funktionen potenziell Ausnahmen auslösen können. Wenn der Compiler nachweisen kann, dass keine Ausnahmen von bestimmten Funktionen kommen (`noexcept`-Spezifikation), kann er den Code, den er generiert, optimieren.

## <a name="catching-exceptions"></a>Abfangen von Ausnahmen
Ein Fehlerzustand, der auf Ebene der [Windows-Runtime-ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) auftritt, wird in Form eines HRESULT-Werts zurückgegeben. Sie müssen HRESULTs jedoch nicht in Ihrem Code behandeln. Der C++/WinRT-Projektionscode, der für eine API auf Nutzungsseite generiert wird, erkennt einen HRESULT-Code auf der ABI-Ebene und konvertiert den Code in eine [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)-Ausnahme, die Sie abfangen und behandeln können.

Wenn der Benutzer beispielsweise ein Bild aus der Bildbibliothek löscht, während die Anwendung diese Sammlung durchläuft, wird von der Projektion eine Ausnahme ausgelöst. Dies ist ein Fall, in dem Sie diese Ausnahme abfangen und behandeln müssen. Dieses Codebeispiel veranschaulicht diesen Fall.

```cppwinrt
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
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Verwenden Sie dieses Muster in einer Coroutine beim Aufrufen einer `co_await`-ed-Funktion. Ein weiteres Beispiel für diese Konvertierung von HRESULT in eine Ausnahme ist, wenn eine Komponenten-API E_OUTOFMEMORY zurückgibt, wodurch ein **std::bad_alloc** ausgelöst wird.

## <a name="throwing-exceptions"></a>Auslösen von Ausnahmen
Es gibt Fälle, in denen Sie dies entscheiden, Ihre Anwendung aber nicht wiederhergestellt werden kann, wenn der Aufruf einer bestimmten Funktion fehlschlägt (Sie könnten sich dann nicht mehr darauf verlassen, dass die Anwendung wie erwartet funktioniert). Im Codebeispiel unten wird ein [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle)-Wert als Wrapper für den HANDLE verwendet, der von [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396) zurückgegeben wird. Anschließend wird der Handle (es wird daraus ein `bool`-Wert erstellt) an die Funktionsvorlage [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) übergeben. **winrt::check_bool** funktioniert mit einem `bool`-Wert oder mit einem beliebigen Wert, der in `false` (Fehler) oder `true` (Erfolg) konvertiert werden kann.

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Wenn der Wert, den Sie an [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) übergeben, falsch ist, findet die folgende Abfolge von Aktionen statt.

- **winrt::check_bool** ruft die Funktion [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) auf.
- **WinRT::throw_last_error** Aufrufe [ **GetLastError** ](https://msdn.microsoft.com/library/windows/desktop/ms679360) zum Abrufen des aufrufenden Threads den Wert des letzten Fehlers, und ruft dann die [ **winrt::throw_ HRESULT** ](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) Funktion.
- **winrt::throw_hresult** löst eine Ausnahme unter Verwendung eines [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)-Objekts (oder eines Standardobjekts) aus, das diesen Fehlercode darstellt.

Da Windows-APIs Laufzeitfehler mit verschiedenen Rückgabewerttypen melden, stehen zusätzlich zu **winrt::check_bool** einige weitere nützliche Hilfsfunktionen zur Verfügung, um Werte zu überprüfen und Ausnahmen auszulösen.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Überprüft, ob der HRESULT-Code einen Fehler darstellt. Ist dies der Fall, ruft es **winrt::throw_hresult** auf.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Überprüft, ob ein Code einen Fehler darstellt. Ist dies der Fall, ruft es **winrt::throw_hresult** auf.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Überprüft, ob ein Zeiger null ist. Ist dies der Fall, ruft es **winrt::throw_last_error** auf.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Überprüft, ob ein Code einen Fehler darstellt. Ist dies der Fall, ruft es **winrt::throw_hresult** auf.

Sie können diese Hilfsfunktionen für gängige Rückgabecodetypen verwenden oder auf eine Fehlerbedingung reagieren und entweder [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) oder [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) aufrufen. 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Auslösen von Ausnahmen beim Erstellen einer API
Da eine Ausnahme nicht die Grenze der [Windows-Runtime-ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) überschreiten darf, wird ein Fehlerzustand, der in einer Implementierung auftritt, über die ABI-Ebene in Form eines HRESULT-Fehlercodes zurückgegeben. Wenn Sie eine API mit C++/WinRT entwickeln, wird Code für Sie generiert, um jede Ausnahme, die Sie in Ihrer Implementierung *auslösen*, in ein HRESULT zu konvertieren. Die Funktion [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) wird in diesem generierten Code als Muster wie dieses verwendet.

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

[**WinRT::to_hresult** ](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) behandelt Ausnahmen, die von abgeleiteten **Std:: Exception**, und [ **winrt::hresult_error** ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) und seine abgeleiteten Typen. In Ihrer Implementierung sollten Sie **winrt::hresult_error** oder einen abgeleiteten Typ bevorzugen, sodass Nutzer Ihrer API ausführliche Fehlerinformationen erhalten. **std::exception** (was E_FAIL entspricht) wird unterstützt, wenn Ausnahmen aus der Verwendung der Standardvorlagenbibliothek entstehen.

## <a name="assertions"></a>Assertionen
Für interne Annahmen in Ihrer Anwendung gibt es Assertionen. Bevorzugen Sie möglichst **static_assert** für die Überprüfung während der Kompilierung. Verwenden Sie für Laufzeitbedingungen WINRT_ASSERT mit einen booleschen Ausdruck.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT wird bei Releasebuilds kompiliert; in einem Debugbuild beendet es die Anwendung im Debugger an der Codezeile, in der sich die Assertion befindet.

Sie sollten keine Ausnahmen in Ihren Destruktoren verwenden. Folglich können Sie zumindest in Debug-Builds das Ergebnis des Aufrufs einer Funktion von einem Destruktor mit WINRT_VERIFY (mit einem booleschen Ausdruck) und WINRT_VERIFY_ (mit dem erwarteten Ergebnis und einem booleschen Ausdruck) bestätigen.

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>Wichtige APIs
* [Vorlage für WinRT::check_bool-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [WinRT::check_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Vorlage für WinRT::check_nt-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [Vorlage für WinRT::check_pointer-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [Vorlage für WinRT::check_win32-Funktion](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [WinRT::Handle-Struktur](/uwp/cpp-ref-for-winrt/handle)
* [WinRT::hresult_error-Struktur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [WinRT::throw_hresult-Funktion](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [WinRT::throw_last_error-Funktion](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult function](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Verwandte Themen
* [Fehler- und Ausnahmebehandlung (modernes C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [So wird es gemacht: Entwurf für sichere Ausnahmebehandlung](/cpp/cpp/how-to-design-for-exception-safety)
