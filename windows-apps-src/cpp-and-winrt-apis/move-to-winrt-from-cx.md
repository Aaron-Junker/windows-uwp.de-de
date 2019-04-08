---
description: In diesem Thema wird gezeigt, wie Sie C++/CX-Code zum entsprechenden Äquivalent in C++/WinRT portieren.
title: C++/CX zu C++/WinRT wechseln
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, portieren, migrieren, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: fe988bffbf024308fb5d43da7ed538e5330b58de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635075"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX zu C++/WinRT wechseln

In diesem Thema wird gezeigt, wie zum Portieren von Code in einem [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) Projekt, um die entsprechenden Werte in [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Portieren von Strategien

Wenn Sie nach und nach Ihrem C + portieren möchten c++ / CX-Code für C++ / WinRT, Sie können. C++ / CX und C++ / WinRT-Code kann gleichzeitig im selben Projekt, mit Ausnahme der XAML-Unterstützung des Compilers und der Windows-Runtime-Komponenten verwendet werden. Für diese zwei Ausnahmen müssen Sie entweder C + Ziel c++ / CX- oder C++ / WinRT im selben Projekt.

> [!IMPORTANT]
> Wenn Ihr Projekt eine XAML-Anwendung erstellt wird, klicken Sie dann einen Workflow, die wir empfehlen wird zunächst ein neues Projekt in Visual Studio mithilfe eines C + erstellen c++ / WinRT-Projektvorlagen (finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Starten Sie dann auf Kopieren von Quellcode und Markup über von der C / c++ / CX-Projekt. Sie können neue XAML-Seiten mit hinzufügen **Projekt** \> **neues Element hinzufügen...** \> **Visual C++** > **Blank-Seite (C++ / WinRT)**.
>
> Alternativ können Sie eine Windows-Runtime-Komponente auf Faktor Code aus der XAML C / c++ / CX zu projizieren, wie Sie es portieren. Verschieben Sie entweder so viele C + c++ / CX code wie möglich in eine Komponente, und ändern Sie das XAML-Projekt in C++ / WinRT. Oder übernehmen Sie andernfalls das XAML-Projekt als C++ / CX, erstellen Sie eine neue C + c++ / WinRT-Komponente, und Portieren von C++ / CX-Code aus dem XAML-Projekt, und klicken Sie in der Komponente. Sie könnten auch haben C++ / CX-Komponentenprojekt zusammen mit C++ / WinRT-Komponentenprojekt innerhalb der gleichen Lösung, verweisen auf beide aus Ihrem Anwendungsprojekt, und den port schrittweise aus einem zum anderen. Finden Sie unter [Interoperabilität zwischen C++ / WinRT und C++ / CX](interop-winrt-cx.md) für Weitere Informationen zur Verwendung der beiden sprachprojektionen im selben Projekt.

> [!NOTE]
> Sowohl [C++/CX ](/cpp/cppcx/visual-c-language-reference-c-cx) als auch das Windows SDK deklarieren Typen im Root-Namespace **Windows**. Ein Windows-Typ, der in C++/WinRT projiziert wird, verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, befindet sich aber im C++/**winrt**-Namespace. Diese unterschiedlichen Namespaces ermöglichen die Portierung von C++/CX nach C++/WinRT in Ihrem eigenen Tempo.

Unter Berücksichtigung der oben genannten Ausnahmen, der erste Schritt bei der Portierung von C++ / CX-Projekt in C++ / WinRT ist C++ manuell hinzufügen c++ / WinRT-Support, um es (finden Sie unter [Visual Studio-Unterstützung für C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Installieren Sie zu diesem Zweck die [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in Ihr Projekt. Öffnen des Projekts in Visual Studio, klicken Sie auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Navigieren Sie**geben oder fügen Sie **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wählen Sie das Element in den Suchergebnissen aus, und klicken Sie dann auf **installieren** zum Installieren des Pakets für das betreffende Projekt. Eine Auswirkung dieser Änderung ist, dass die Unterstützung für C++/CX im Projekt deaktiviert wird. Es ist eine gute Idee, lassen Sie Support deaktiviert, sodass Buildmeldungen Sie suchen (und Port) können alle Ihre Abhängigkeiten an C++ / CX, oder Sie können Unterstützung wieder aktivieren (in den Projekteigenschaften, **C/C++-** \> **Allgemein** \> **Windows-Runtime-Erweiterung nutzen** \> **Ja (/ Zw)**), und nach und nach port.

Stellen Sie sicher, Projekteigenschaft **allgemeine** \> **Zielplattformversion** nastaven NA hodnotu 10.0.17134.0 (Windows 10, Version 1803) oder höher.

Fügen Sie Ihrer vorkompilierten Headerdatei (in der Regel `pch.h`) `winrt/base.h` hinzu.

```cppwinrt
#include <winrt/base.h>
```

Wenn Sie alle C++/WinRT-projizierten Windows-API-Header hinzufügen (z. B. `winrt/Windows.Foundation.h`), müssen Sie so nicht explizit `winrt/base.h` einschließen, da dies automatisch erfolgt.

Wenn Ihr Projekt zudem Typen der [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) verwendet, lesen Sie bitte [Wechsel zu C++/WinRT von WRL](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Parameterübergabe
Beim Schreiben von C++ / CX-Quellcode, übergeben Sie C++ / CX-Typen als Funktionsparameter als Hat (\^) verweisen.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

In C++/WinRT sollten Sie für synchrone Funktionen standardmäßig `const&`-Parameter verwenden. Dadurch werden Kopien und unnötiger Zusatzaufwand vermieden. Ihre Coroutinen sollten aber Pass-by-Value verwenden, um sicherzustellen, dass sie nach Wert erfassen, und um Probleme mit der Lebensdauer zu vermeiden (weitere Informationen finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Ein C++/WinRT-Objekt ist im Grunde genommen ein Wert, der einen Schnittstellenzeiger zum zugrunde liegenden Windows-Runtime-Objekt enthält. Beim Kopieren eines C++/WinRT-Objekts kopiert der Compiler den gekapselten Schnittstellenzeiger, wodurch sich sein Verweiszähler erhöht. Eine Zerstörung der Kopie beinhaltet, dass der Verweiszähler verringert wird. Daher wird Aufwand einer Kopie nur bei Bedarf verursacht.

## <a name="variable-and-field-references"></a>Variablen und Feldverweise
Beim Schreiben von C++ / CX-Quellcode, Sie verwenden, Hat (\^) Variablen auf den Pfeil und Windows-Runtime-Objekte verweisen (-&gt;) Operator, um eine Variable Hat zu dereferenzieren.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Bei der Portierung von für die entsprechende C++ / WinRT-Code können Sie viel abrufen, indem die Hüten entfernen und Ändern von Pfeiloperators (-&gt;) auf den Punktoperator (.). C++ / WinRT projizierte Typen sind, Werte und keine Verweise.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

Der Standardkonstruktor für eine C++ / CX Hat Zeiger mit null initialisiert. Hier ist ein C++ / CX-Codebeispiel, in denen, die wir erstellen eine Variable oder ein Feld den richtigen Typ, aber, die nicht initialisiert wurde. Das heißt, es anfangs verweist nicht auf eine **TextBlock**; wir einen Verweis später zuweisen möchten.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Für das entsprechende in C++ / WinRT, finden Sie unter [verzögerte Initialisierung](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Eigenschaften
Die C++/CX-Spracherweiterungen beinhalten das Konzept von Eigenschaften. Beim Schreiben von C++/CX-Quellcode können Sie auf eine Eigenschaft zugreifen, als wäre sie ein Feld. Der C++-Standardcode verfügt nicht über das Konzept einer Eigenschaft, folglich rufen Sie in C++/WinRT Get- und Set-Funktionen auf.

In den folgenden Beispielen sind **XboxUserId**, **UserState**, **PresenceDeviceRecords** und **Größe** alles Eigenschaften.

### <a name="retrieving-a-value-from-a-property"></a>Abrufen eines Werts aus einer Eigenschaft
So erhalten Sie einen Eigenschaftswert in C++/CX.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

Der entsprechende C++/WinRT-Quellcode ruft eine Funktion mit dem gleichen Namen wie die Eigenschaft auf, jedoch ohne Parameter.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Beachten Sie, dass die Funktion **PresenceDeviceRecords** ein Windows-Runtime-Objekt zurückgibt, das über eine **Size**-Funktion verfügt. Da es sich beim zurückgegebenen Objekt ebenfalls um einen C++/WinRT-projizierten Typ handelt, dereferenzieren wir, indem wir den Punktoperator zum Aufrufen von **Size** verwenden.

### <a name="setting-a-property-to-a-new-value"></a>Festlegen einer Eigenschaft auf einen neuen Wert
Beim Festlegen einer Eigenschaft auf einen neuen Wert wird ein ähnliches Muster angewandt. Zuerst in C++/CX.

```cppcx
record->UserState = newValue;
```

Um das Äquivalent in C++/WinRT zu tun, rufen Sie eine Funktion mit dem gleichen Namen wie die Eigenschaft auf und übergeben ein Argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Erstellen einer Instanz einer Klasse
Arbeiten Sie mit C++ / CX-Objekt über ein Handle dafür, die so genannte ein Caretzeichen (\^) Verweis. Sie erstellen ein neues Objekt über das Schlüsselwort `ref new`, das wiederum [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) aufruft, um eine neue Instanz der Laufzeitklasse zu aktivieren.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Ein C++/WinRT-Objekt ist ein Wert. Folglich können Sie es auf dem Stapel oder als ein Feld eines Objekts zuweisen. *Niemals* können Sie `ref new` (oder `new`) verwenden, um ein C++/WinRT-Objekt zuzuordnen. Im Hintergrund wird dennoch **RoActivateInstance** aufgerufen.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Wenn eine Ressource aufwendig zu initialisieren ist, ist es üblich, deren Initialisierung zu verzögern, bis sie tatsächlich benötigt wird.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

Der gleiche Code wird zu C++/WinRT portiert. Beachten Sie die Verwendung des Konstruktors `nullptr`. Weitere Informationen zu diesem Konstruktor finden Sie unter [Nutzen von APIs mit C++/WinRT](consume-apis.md).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Konvertieren von einer Basis-Runtime-Klasse in einer abgeleiteten
Es ist üblich, einen Verweis auf eine Basisklasse zu verwenden, die Sie kennen, auf ein Objekt eines abgeleiteten Typs verweist. In C++ / CX können Sie verwenden `dynamic_cast` zu *Umwandlung* der Verweis auf eine Basisklasse in einen Verweis auf eine abgeleitete. Die `dynamic_cast` ist eigentlich nur ein ausgeblendeter Aufruf [ **QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Hier ist ein typisches Beispiel&mdash;können Sie eine Abhängigkeit Eigenschaftenänderungsereignis behandeln, und Sie umwandeln möchten **DependencyObject** wieder in den tatsächlichen Typ, der die Abhängigkeitseigenschaft besitzt.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

Der entsprechende C++-/ c++ / WinRT-Code ersetzt die `dynamic_cast` durch einen Aufruf der [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) -Funktion, die kapselt **QueryInterface**. Sie haben auch die Möglichkeit zum Aufrufen [ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), stattdessen die löst eine Ausnahme aus, wenn Abfragen für die erforderliche Schnittstelle (die Standardschnittstelle des Typs, die Sie angefordert werden) nicht zurückgegeben wird. Hier ist ein C++ / WinRT-Codebeispiel.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Ereignisbehandlung mit einem Delegaten
Hier ist ein typisches Beispiel für die Behandlung eines Ereignisses in C++/CX unter Verwendung einer Lambda-Funktion als Delegaten.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Dies ist das Äquivalent in C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Anstelle einer Lambda-Funktion können Sie den Delegaten als eine freie Funktion oder als Pointer-to-Member-Funktion implementieren. Weitere Informationen finden Sie unter [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md).

Wenn Sie von einer C++/CX-Codebasis portieren, wo Ereignisse und Delegate intern verwendet werden (nicht über Binärdateien), hilft Ihnen [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) beim Replizieren dieses Musters in C++/WinRT. Siehe auch [parametrisierten Delegaten, einfachen Signale und Rückrufe in einem Projekt](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Widerrufen eines Delegaten
In C++/CX verwenden Sie den `-=`-Operator, um eine frühere Ereignisregistrierung zu widerrufen.

```cppcx
myButton->Click -= token;
```

Dies ist das Äquivalent in C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Weitere Informationen und Optionen finden Sie unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Zuordnen von C++/CX-**Platform**-Typen zu C++/WinRT-Typen
C++/CX bietet verschiedene Datentypen im **Platform**-Namespace. Diese Typen gehören nicht zum C++-Standard, daher können Sie sie nur verwenden, wenn Sie die Windows-Runtime-Spracherweiterungen aktivieren (Visual Studio-Projekteigenschaft **C/C++** > **Allgemein** > **Windows-Runtime-Erweiterung verwenden** > **Ja (/ZW)**). In der Tabelle unten finden Sie Informationen zum Portieren von **Platform**-Typen in ihre Entsprechungen in C++/WinRT. Nachdem Sie dies erledigt haben, können Sie, da C++/WinRT zum C++-Standard gehört, die Option `/ZW` deaktivieren.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform:: Agile\^** | [**WinRT::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform:: Array\^** | Finden Sie unter [Port **Platform:: Array\^**](#port-platformarray) |
| **Platform:: Exception\^** | [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform:: InvalidArgumentException\^** | [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform:: Object\^** | **WinRT::Windows::Foundation::IInspectable** |
| **Platform:: String\^** | [**WinRT::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Port **Platform:: Agile\^**  zu **winrt::agile_ref**
Die **Platform:: Agile\^**  Typ in C++ / CX stellt eine Windows-Runtime-Klasse, die von jedem Thread aus zugegriffen werden kann. C++ / WinRT-äquivalent ist [ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

In C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

In C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Port **Platform:: Array\^**
Ihre Optionen umfassen die Verwendung einer Initialisiererliste einen **Std:: Array**, oder ein **Std:: vector**. Weitere Informationen und Codebeispiele, finden Sie unter [Standard Initialisiererlisten](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) und [Standard Arrays und Vektoren](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresulterror"></a>Port **Platform:: Exception\^**  zu **winrt::hresult_error**
Die **Platform:: Exception\^**  Typ wird erzeugt, in C++ / CX, ein Windows-Runtime-API einen nicht S gibt\_OK HRESULT. Für C++/WinRT ist die Entsprechung [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Zum Portieren von C++ / WinRT, Ändern der gesamte Code, der verwendet **Platform:: Exception\^**  verwenden **winrt::hresult_error**.

In C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

In C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT stellt diese Ausnahmeklassen bereit.

| Ausnahmentyp | Basisklasse | HRESULT |
| ---- | ---- | ---- |
| [**WinRT::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | Aufrufen von [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**WinRT::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **WinRT::hresult_error** | E_ACCESSDENIED |
| [**WinRT::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **WinRT::hresult_error** | ERROR_CANCELLED |
| [**WinRT::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **WinRT::hresult_error** | E_CHANGED_STATE |
| [**WinRT::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **WinRT::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**WinRT::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **WinRT::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**WinRT::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **WinRT::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**WinRT::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **WinRT::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**WinRT::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **WinRT::hresult_error** | E_INVALIDARG |
| [**WinRT::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **WinRT::hresult_error** | E_NOINTERFACE |
| [**WinRT::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **WinRT::hresult_error** | E_NOTIMPL |
| [**WinRT::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **WinRT::hresult_error** | E_BOUNDS |
| [**WinRT::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **WinRT::hresult_error** | RPC_E_WRONG_THREAD |

Beachten Sie, dass jede Klasse (über die **hresult_error** -Basisklasse) eine [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function)-Funktion bereitstellt, die das HRESULT für den Fehler zurückgibt, sowie eine [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function)-Funktion, die die Darstellung der Zeichenfolge dieses HRESULT zurückgibt.

Hier ist ein Beispiel für das Auslösen einer Ausnahme in C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Und das Äquivalent in C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Port **Platform:: Object\^**  zu **Winrt::Windows::Foundation::IInspectable**
Wie alle C++/WinRT-Typen ist **winrt::Windows::Foundation::IInspectable** ein Werttyp. Hier erfahren Sie, wie Sie eine Variable dieses Typs auf null initialisieren.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Port **Platform:: String\^**  zu **winrt::hstring**
**Platform:: String\^**  ist gleichbedeutend mit der Windows-Runtime-HSTRING-ABI-Typ. Für C++/WinRT ist die Entsprechung [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mit C++/WinRT können Sie aber Windows-Runtime-APIs mit Standard-C++ Wide-String-Typen wie **std::wstring** aufrufen und/oder Wide-String-Literale. Weitere Informationen und Codebeispiele finden Sie unter [String-Verarbeitung in C++/WinRT](strings.md).

Mit C++ / CX können Sie erreichen die [ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) Eigenschaft zum Abrufen der Zeichenfolge wie eine C-Stil **const Wchar_t\***  Array (z. B. übergeben Damit **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Um das Gleiche mit C++ / WinRT zu tun, können Sie die Funktion [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) verwenden, um eine auf null beendete Zeichenfolgenversion im C-Stil abzurufen (wie von **std::wstring**).

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Wenn es darum geht, die Implementierung von APIs, die Zeichenfolgen zurückzugeben, akzeptieren, Sie in der Regel ändern, C++ / CX-Code, der verwendet **Platform:: String\^**  verwenden **winrt::hstring** stattdessen.

Hier ist ein Beispiel für eine C++/CX-API, die eine Zeichenfolge übernimmt.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Für C++/WinRT könnten Sie diese API in [MIDL 3.0](/uwp/midl-3) wie folgt deklarieren.

```idl
// LogType.idl
void LogWrapLine(String str);
```

Die C++/WinRT-Toolkette generiert dann Quellcode für Sie, der wie folgt aussieht.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++ / CX stellt der [Object:: ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) Methode.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++ / WinRT nicht direkt zur Verfügung, diese Funktion, aber Sie können die alternativen zu aktivieren.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>Wichtige APIs
* [Vorlage für WinRT::Delegate-Struktur](/uwp/cpp-ref-for-winrt/delegate)
* [WinRT::hresult_error-Struktur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [WinRT::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Erstellen von Ereignissen in C++ / WinRT](author-events.md)
* [Parallelität und asynchrone Vorgänge mit C++ / WinRT](concurrency.md)
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++ / WinRT](handle-events.md)
* [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md)
* [Microsoft Interface Definition Language-3.0-Referenz](/uwp/midl-3)
* [Umstellen von WRL auf C++/WinRT](move-to-winrt-from-wrl.md)
* [Zeichenfolgenbehandlung in C++ / WinRT](strings.md)
