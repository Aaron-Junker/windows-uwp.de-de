---
description: In diesem Thema wird gezeigt, wie Sie C++/CX-Code zum entsprechenden Äquivalent in C++/WinRT portieren.
title: Umstellen von C++/CX auf C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360158"
---
# <a name="move-to-cwinrt-from-ccx"></a>Umstellen von C++/CX auf C++/WinRT

In diesem Thema wird veranschaulicht, wie du den Code in einem [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Projekt in den entsprechenden Code in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) portierst.

## <a name="porting-strategies"></a>Strategien für das Portieren

Es ist möglich, dass du deinen C++/CX-Code nach und nach in C++/WinRT-Code portierst. C++/CX- und C++/WinRT-Code kann in demselben Projekt gleichzeitig vorhanden sein, mit Ausnahme von XAML-Compilerunterstützung und der Windows-Runtime-Komponenten. Für diese beiden Ausnahmen musst du dich in einem Projekt entweder für C++/CX oder C++/WinRT entscheiden.

> [!IMPORTANT]
> Wenn in deinem Projekt eine XAML-Anwendung erstellt wird, besteht ein von uns empfohlener Workflow darin, zuerst mit einer der C++/WinRT-Projektvorlagen (siehe [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)) ein neues Projekt in Visual Studio zu erstellen. Beginne dann damit, den Quellcode und das Markup aus dem C++/CX-Projekt zu kopieren. Du kannst **Projekt** \> **Neues Element hinzufügen...** verwenden, um neue XAML-Seiten hinzuzufügen. \> **Visual C++**  > **Leere Seite (C++/WinRT)** .
>
> Alternativ kannst du eine Windows-Runtime-Komponente nutzen, um beim Portieren Code aus dem XAML-C++/CX-Projekt auszuklammern. Verschiebe entweder so viel C++/CX-Code wie möglich in eine Komponente, und ändere anschließend das XAML-Projekt in C++/WinRT. Oder behalte für das XAML-Projekt C++/CX bei, erstelle eine neue C++/WinRT-Komponente, und beginne damit, C++/CX-Code aus dem XAML-Projekt in die Komponente zu portieren. Außerdem kannst du auch ein C++/CX-Komponentenprojekt neben einem C++/WinRT-Komponentenprojekt innerhalb derselben Lösung verwenden, auf beide aus deinem Anwendungsprojekt verweisen und nach und nach das Portieren von einem zum anderen durchführen. Weitere Informationen zur Verwendung der beiden Sprachprojektionen in demselben Projekt findest du unter [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md).

> [!NOTE]
> Sowohl [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) als auch das Windows SDK deklarieren Typen im Stammnamespace **Windows**. Ein Windows-Typ, der in C++/WinRT projiziert wird, verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, befindet sich aber im C++-**winrt**-Namespace. Diese unterschiedlichen Namespaces ermöglichen die Portierung von C++/CX zu C++/WinRT nach deinem eigenen Tempo.

Unter Berücksichtigung der oben erwähnten Ausnahmen ist der erste Schritt beim Portieren eines C++/CX-Projekts zu C++/WinRT das manuelle Hinzufügen von C++/WinRT-Unterstützung (siehe [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Installiere zu diesem Zweck das [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in deinem Projekt. Öffne das Projekt in Visual Studio, klicke auf **Projekt** \> **NuGet-Pakete verwalten...** \> **Durchsuchen**, und gib **Microsoft.Windows.CppWinRT** in das Suchfeld ein. Wähle das Element in den Suchergebnissen aus, und klicke dann auf **Installieren**, um das Paket für das Projekt zu installieren. Eine Auswirkung dieser Änderung ist, dass die Unterstützung für C++/CX im Projekt deaktiviert wird. Es empfiehlt sich, die Unterstützung deaktiviert zu lassen, damit beim Ermitteln und Portieren deiner Abhängigkeiten aus C++/CX Buildmeldungen als Hilfe angezeigt werden. Du kannst die Unterstützung auch wieder aktivieren (in den Projekteigenschaften, **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden** \> **Ja (/ZW)** ) und das Portieren nach und nach durchführen.

Stelle sicher, dass die Projekteigenschaft unter **Allgemein** \> **Version der Zielplattform** auf 10.0.17134.0 (Windows 10, Version 1803) oder höher festgelegt ist.

Füge deiner vorkompilierten Headerdatei (in der Regel `pch.h`) `winrt/base.h` hinzu.

```cppwinrt
#include <winrt/base.h>
```

Wenn du Windows-API-Header mit C++/WinRT-Projektion hinzufügst (z. B. `winrt/Windows.Foundation.h`), musst du `winrt/base.h` nicht so explizit wie hier einfügen, da dies automatisch erfolgt.

Wenn für den Projekt zudem Typen der [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) verwendet werden, helfen dir die Informationen unter [Wechsel zu C++/WinRT von WRL](move-to-winrt-from-wrl.md) weiter.

## <a name="parameter-passing"></a>Parameterübergabe
Beim Schreiben von C++/CX-Quellcode übergibst du C++/CX-Typen als Funktionsparameter in Form von Hütchenverweisen (\^).

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

In C++/WinRT solltest du für synchrone Funktionen standardmäßig `const&`-Parameter verwenden. Hierdurch werden Kopiervorgänge und unnötiger Zusatzaufwand vermieden. Für deine Coroutinen sollte aber Pass-by-Value verwendet werden, um sicherzustellen, dass sie nach Wert erfassen, und um Probleme mit der Lebensdauer zu vermeiden (weitere Informationen unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Ein C++/WinRT-Objekt ist im Grunde genommen ein Wert, der einen Schnittstellenzeiger zum zugrunde liegenden Windows-Runtime-Objekt enthält. Beim Kopieren eines C++/WinRT-Objekts kopiert der Compiler den gekapselten Schnittstellenzeiger, wodurch sich sein Verweiszähler erhöht. Wenn die Kopie zerstört wird, wird der Verweiszähler verringert. Der mit einem Kopiervorgang verbundene Aufwand sollte also nur in Kauf genommen werden, wenn dies wirklich erforderlich ist.

## <a name="variable-and-field-references"></a>Variablen und Feldverweise
Beim Schreiben von C++/CX-Quellcode verwendest du Hütchenvariablen (\^) zum Verweisen auf Windows-Runtime-Objekte sowie den Pfeiloperator (-&gt;), um den Verweis einer Hütchenvariablen aufzuheben.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Beim Portieren in den entsprechenden C++/WinRT-Code ist es sehr effektiv, die Hütchen zu entfernen und den Pfeiloperator (-&gt;) in den Punktoperator (.) zu ändern. Bei Typen mit C++/WinRT-Projektion handelt es sich um Werte, nicht um Zeiger.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

Der Standardkonstruktor für einen C++/CX-Hütchenzeiger führt dafür eine Initialisierung auf null durch. Hier ist ein C++/CX-Codebeispiel angegeben, in dem wir eine Variable bzw. ein Feld des richtigen Typs erstellen, die bzw. das aber nicht initialisiert ist. Es ist also anfänglich kein Verweis auf ein **TextBlock**-Element vorhanden. Wir weisen später einen Verweis zu.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Informationen zur Entsprechung in C++/WinRT findest du unter [Verzögerte Initialisierung](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Eigenschaften
Die C++/CX-Spracherweiterungen beinhalten das Konzept der „Eigenschaften“. Beim Schreiben von C++/CX-Quellcode kannst du so auf eine Eigenschaft zugreifen, als ob sie ein Feld wäre. Da der C++-Standardcode nicht über das Konzept einer Eigenschaft verfügt, rufst du in C++/WinRT Get- und Set-Funktionen auf.

In den folgenden Beispielen sind **XboxUserId**, **UserState**, **PresenceDeviceRecords** und **Size** jeweils Eigenschaften.

### <a name="retrieving-a-value-from-a-property"></a>Abrufen eines Werts aus einer Eigenschaft
Hier ist beschrieben, wie du einen Eigenschaftswert in C++/CX erhältst.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

Der entsprechende C++/WinRT-Quellcode ruft eine Funktion mit dem gleichen Namen wie die Eigenschaft auf, aber ohne Parameter.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Beachte hierbei, dass die Funktion **PresenceDeviceRecords** ein Windows-Runtime-Objekt zurückgibt, das über eine **Size**-Funktion verfügt. Da es sich beim zurückgegebenen Objekt ebenfalls um einen Typ mit C++/WinRT-Projektion handelt, führen wir das Dereferenzieren durch, indem wir den Punktoperator zum Aufrufen von **Size** verwenden.

### <a name="setting-a-property-to-a-new-value"></a>Festlegen einer Eigenschaft auf einen neuen Wert
Beim Festlegen einer Eigenschaft auf einen neuen Wert wird ein ähnliches Muster angewandt. Zuerst in C++/CX.

```cppcx
record->UserState = newValue;
```

Um dasselbe in C++/WinRT durchzuführen, rufst du eine Funktion mit dem gleichen Namen wie die Eigenschaft auf und übergibst ein Argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Erstellen einer Instanz einer Klasse
Hierfür verwendest du ein C++/ CX-Objekt über einen Handle (Hütchenverweis \^). Du erstellst ein neues Objekt über das Schlüsselwort `ref new`, das wiederum [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) aufruft, um eine neue Instanz der Laufzeitklasse zu aktivieren.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Ein C++/WinRT-Objekt ist ein Wert. Folglich kannst du es im Stapel oder als Feld eines Objekts zuweisen. Du verwendest *niemals* `ref new` (oder `new`), um ein C++/WinRT-Objekt zuzuordnen. Im Hintergrund wird dennoch **RoActivateInstance** aufgerufen.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Falls die Initialisierung einer Ressource aufwendig ist, wird dieser Vorgang meist verzögert, bis sie tatsächlich benötigt wird.

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

Hier ist der gleiche Code nach dem Portieren zu C++/WinRT angegeben. Beachte die Verwendung des Konstruktors `nullptr`. Weitere Informationen zu diesem Konstruktor findest du unter [Nutzen von APIs mit C++/WinRT](consume-apis.md).

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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Konvertieren von einer Basis-Laufzeitklasse zu einer abgeleiteten Klasse
Üblicherweise wird ein Verweis auf die Basis verwendet, für die bekannt ist, dass auf ein Objekt mit einem abgeleiteten Typ verwiesen wird. In C++/CX verwendest du `dynamic_cast`, um „Verweis auf Basis“ in „Verweis auf abgeleitet“ *umzuwandeln*. `dynamic_cast` ist eigentlich nur ein versteckter Aufruf von [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Hier ist ein typisches Beispiel angegeben: Du verarbeitest ein Ereignis für eine geänderte Abhängigkeitseigenschaft und möchtest die Umwandlung von **DependencyObject** zurück in den eigentlichen Typ durchführen, der der Besitzer der Abhängigkeitseigenschaft ist.

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

Der entsprechende C++/WinRT-Code ersetzt `dynamic_cast` durch einen Aufruf der Funktion [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function), in die **QueryInterface** eingekapselt ist. Darüber hinaus hast du die Option, stattdessen [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) aufzurufen. Hierbei wird eine Ausnahme ausgelöst, wenn bei der Abfrage der erforderlichen Schnittstelle (Standardschnittstelle des angeforderten Typs) keine Rückgabe erfolgt. Hier ist ein C++/WinRT-Codebeispiel angegeben.

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
Hier ist ein typisches Beispiel für die Behandlung eines Ereignisses in C++/CX mit einer Lambda-Funktion als Delegat angegeben.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Dies ist die Entsprechung in C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Anstelle einer Lambda-Funktion kannst du den Delegaten als freie Funktion oder als Pointer-to-Member-Funktion (Zeiger auf Member) implementieren. Weitere Informationen findest du unter [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md).

Wenn du von einer C++/CX-Codebasis portierst, für die Ereignisse und Delegate intern verwendet werden (nicht über Binärdateien), hilft dir [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) beim Replizieren dieses Musters in C++/WinRT weiter. Siehe auch [Parametrisierte Delegaten, einfache Signale und Rückrufe in einem Projekt](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Widerrufen eines Delegaten
In C++/CX verwendest du den Operator `-=`, um eine frühere Ereignisregistrierung zu widerrufen.

```cppcx
myButton->Click -= token;
```

Dies ist die Entsprechung in C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Weitere Informationen und Optionen findest du unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Zuordnen von C++/CX-**Platform**-Typen zu C++/WinRT-Typen
Unter C++/CX werden im **Platform**-Namespace verschiedene Datentypen bereitgestellt. Diese Typen gehören nicht zum C++-Standard. Daher kannst du sie nur verwenden, wenn du die Windows-Runtime-Spracherweiterungen aktivierst (Visual Studio-Projekteigenschaft **C/C++**  > **Allgemein** > **Windows-Runtime-Erweiterung verwenden** > **Ja (/ZW)** ). In der Tabelle unten findest du Informationen zum Portieren von **Platform**-Typen in ihre Entsprechungen in C++/WinRT. Anschließend kannst du die Option `/ZW` deaktivieren, da C++/WinRT zum C++-Standard gehört.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Siehe [Portieren von **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Portieren von **Platform::Agile\^** zu **winrt::agile_ref**
Der Typ **Platform::Agile\^** in C++/CX stellt eine Windows-Runtime-Klasse dar, auf die über einen beliebigen Thread zugegriffen werden kann. Die C++/WinRT-Entsprechung lautet [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

In C++/CX:

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

In C++/WinRT:

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Portieren von **Platform::Array\^**
Du kannst eine Initialisierungsliste, ein **std::array**-Element oder ein **std::vector**-Element verwenden. Weitere Informationen und Codebeispiele findest du unter [Standard-Initialisierungslisten](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) und [Standard-Arrays und -Vektoren](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresulterror"></a>Portieren von **Platform::Exception\^** zu **winrt::hresult_error**
Der Typ **Platform::Exception\^** wird in C++/CX erzeugt, wenn eine Windows-Runtime-API kein S\_OK HRESULT zurückgibt. Für C++/WinRT ist die Entsprechung [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Zum Portieren zu C++/WinRT musst du den gesamten Code ändern, für den **Platform::Exception\^** verwendet wird, um stattdessen **winrt::hresult_error** zu nutzen.

In C++/CX:

```cppcx
catch (Platform::Exception^ ex)
```

In C++/WinRT:

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT stellt die hier angegebenen Ausnahmeklassen bereit.

| Ausnahmentyp | Basisklasse | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | Aufruf von [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Beachte, dass jede Klasse (über die **hresult_error**-Basisklasse) eine [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function)-Funktion bereitstellt, die das HRESULT für den Fehler zurückgibt, sowie eine [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function)-Funktion, die die Darstellung der Zeichenfolge dieses HRESULT zurückgibt.

Hier ist ein Beispiel für das Auslösen einer Ausnahme in C++/CX angegeben:

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Dies ist die Entsprechung in C++/WinRT:

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Portieren von **Platform::Object\^** zu **winrt::Windows::Foundation::IInspectable**
Wie alle C++/WinRT-Typen ist **winrt::Windows::Foundation::IInspectable** ein Werttyp. Hier erfährst du, wie du eine Variable dieses Typs auf null initialisierst.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Portieren von **Platform::String\^** zu **winrt::hstring**
**Platform::String\^** ist das Äquivalent zum HSTRING-ABI-Typ der Windows-Runtime. Für C++/WinRT ist die Entsprechung [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Bei C++/WinRT kannst du aber Windows-Runtime-APIs mit Wide-String-Typen der C++-Standardbibliothek, z. B. **std::wstring**, bzw. Wide-String-Literale aufrufen. Weitere Informationen und Codebeispiele findest du unter [String-Verarbeitung in C++/WinRT](strings.md).

Mit C++/CX kannst du auf die Eigenschaft [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) zugreifen, um die Zeichenfolge als **const wchar_t\*** -Array im C-Stil abzurufen (z. B. zur Übergabe an **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Mit C++/WinRT erreichst du dies, indem du die Funktion [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) verwendest, um eine auf null beendete Zeichenfolgenversion im C-Stil abzurufen (wie über **std::wstring**).

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Bei der Implementierung von APIs, die Zeichenfolgen übernehmen oder zurückgeben, änderst du in der Regel jeglichen C++/CX-Code, der **Platform::String\^** verwendet, um stattdessen **winrt::hstring** zu nutzen.

Hier ist ein Beispiel für eine C++/CX-API angegeben, für die eine Zeichenfolge verwendet wird:

```cppcx
void LogWrapLine(Platform::String^ str);
```

Für C++/WinRT kannst du diese API in [MIDL 3.0](/uwp/midl-3) beispielsweise wie folgt deklarieren:

```idl
// LogType.idl
void LogWrapLine(String str);
```

Die C++/WinRT-Toolkette generiert dann Quellcode für dich, der wie folgt aussieht.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

In C++/CX wird die [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring)-Methode bereitgestellt.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

In C++/WinRT ist dies nicht direkt verfügbar, aber du kannst Alternativen nutzen.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>Wichtige APIs
* [Strukturvorlage „winrt::delegate“](/uwp/cpp-ref-for-winrt/delegate)
* [Struktur „winrt::hresult_error“](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Struktur „winrt::hstring“](/uwp/cpp-ref-for-winrt/hstring)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Erstellen von Ereignissen in C++/WinRT](author-events.md)
* [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md)
* [Verwenden von APIs mit C++/WinRT](consume-apis.md)
* [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md)
* [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md)
* [Microsoft Interface Definition Language 3.0 – Referenz](/uwp/midl-3)
* [Umstellen von WRL auf C++/WinRT](move-to-winrt-from-wrl.md)
* [Verarbeitung von Zeichenfolgen in C++/WinRT](strings.md)
