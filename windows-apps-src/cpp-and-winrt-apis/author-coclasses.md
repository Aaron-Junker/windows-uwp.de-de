---
description: C++/WinRT dient Ihnen als Hilfe beim Erstellen von klassischen COM-Komponenten, wie dies auch für Windows-Runtime-Klassen der Fall ist.
title: Erstellen von COM-Komponenten mit C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, erstellen, COM, Komponente
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5ff3677c3624974759d1f6ff21d6e53cf9d33144
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "71344516"
---
# <a name="author-com-components-with-cwinrt"></a>Erstellen von COM-Komponenten mit C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) dient als Hilfe beim Erstellen von klassischen COM-Komponenten (Component Object Model) bzw. „Co-Klassen“, wie dies auch für Windows-Runtime-Klassen der Fall ist. In diesem Thema wird die Vorgehensweise gezeigt.

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>Das Standardverhalten von C++/WinRT bezüglich COM-Schnittstellen

Von der C++/WinRT-Vorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) werden die Laufzeitklassen und Aktivierungsfactorys direkt oder indirekt abgeleitet.

Standardmäßig unterstützt **winrt::implements** nur [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)-basierte Schnittstellen und ignoriert stillschweigend klassische COM-Schnittstellen. Daher schlägt jeder Aufruf von **QueryInterface** für klassische COM-Schnittstellen mit der Ausnahme **E_NOINTERFACE** fehl.

Bevor wir erläutern, wie Sie dieses Problem lösen, veranschaulichen wir in einem Codebeispiel, was standardmäßig geschieht.

```idl
// Sample.idl
runtimeclass Sample
{
    Sample();
    void DoWork();
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

Der folgende Clientcode verwendet die **Sample**-Klasse.

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line crashes, because the QI for IInitializeWithWindow fails.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

Damit **winrt::implements** klassische COM-Schnittstellen unterstützt, muss lediglich `unknwn.h` eingefügt werden, bevor C++/WinRT-Header eingeschlossen werden.

Dies kann explizit oder indirekt durch Einschließen einer weiteren Headerdatei, z. B. `ole2.h`, erfolgen. Ein empfohlenes Verfahren besteht darin, die Headerdatei `wil\cppwinrt.h` einzuschließen, die in den [Windows Implementation Libraries (WIL)](https://github.com/Microsoft/wil) enthalten ist. Die Headerdatei `wil\cppwinrt.h` stellt nicht nur sicher, dass `unknwn.h` vor `winrt/base.h` enthalten ist, sondern sorgt auch dafür, dass die Ausnahmen und Fehlercodes von C++/WinRT und WIL von der jeweils anderen Komponente interpretiert werden können.

## <a name="a-simple-example-of-a-com-component"></a>Ein einfaches Beispiel für eine COM-Komponente

Hier ist ein einfaches Beispiel für eine COM-Komponente, die in C++/WinRT geschrieben wurde. Dies ist eine vollständige Auflistung einer Minianwendung. Sie können also den Code testen, indem Sie ihn in die Dateien `pch.h` und `main.cpp` eines neuen Projekts vom Typ **Windows-Konsolenanwendung (C++/WinRT)** einfügen.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

Weitere Informationen findest du auch unter [Verwenden von COM-Komponenten mit C++/WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Ein realistischeres und interessanteres Beispiel

Im restlichen Teil dieses Themas wird Schritt für Schritt die Erstellung eines Konsolenanwendungsprojekts mit minimalem Umfang beschrieben, bei dem C++/WinRT zum Implementieren einer einfachen Co-Klasse (COM-Komponente oder COM-Klasse) und einer Klassenfactory genutzt wird. Anhand der Beispielanwendung wird verdeutlicht, wie du eine Popupbenachrichtigung mit einer Rückrufschaltfläche bereitstellst. Mit der Co-Klasse (über die die COM-Schnittstelle **INotificationActivationCallback** implementiert wird) kann die Anwendung gestartet und zurückgerufen werden, wenn der Benutzer in der Popupbenachrichtigung auf diese Schaltfläche klickt.

Weitere Hintergrundinformationen zum Featurebereich von Popupbenachrichtigungen findest du unter [Senden einer lokalen Popupbenachrichtigung](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Da aber in keinem Codebeispiel dieses Abschnitts der Dokumentation C++/WinRT genutzt wird, empfehlen wir dir, den in diesem Thema gezeigten Code zu verwenden.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Erstellen eines Projekts vom Typ „Windows-Konsolenanwendung“ (ToastAndCallback)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstelle ein Projekt vom Typ **Windows-Konsolenanwendung (C++/WinRT)** , und gib ihm den Namen *ToastAndCallback*.

Öffne `pch.h`, und füge `#include <unknwn.h>` vor den Include-Elementen für C++/WinRT-Header hinzu. Hier ist das Ergebnis angegeben. Du kannst den Inhalt deiner Datei `pch.h` durch diese Auflistung ersetzen.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Öffne `main.cpp`, und entferne die using-Direktiven, die von der Projektvorlage generiert werden. Füge stattdessen den folgenden Code ein (damit die benötigten Bibliotheken, Header und Typnamen vorhanden sind). Hier ist das Ergebnis angegeben. Du kannst den Inhalt deiner Datei `main.cpp` durch diese Auflistung ersetzen. (Wir haben in der Liste unten auch den Code aus `main` entfernt, weil wir diese Funktion später noch ersetzen.)

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;

int main() { }
```

Das Projekt kann noch nicht erstellt werden. Nachdem wir das Hinzufügen des Codes abgeschlossen haben, erhältst du eine Aufforderung zum Erstellen und Ausführen.

## <a name="implement-the-coclass-and-class-factory"></a>Implementieren der Co-Klasse und Klassenfactory

In C++/WinRT implementierst du Co-Klassen und Klassenfactorys per Ableitung von der Basisstruktur [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Füge direkt nach den drei oben angegebenen using-Direktiven (und vor `main`) diesen Code ein, um die COM-Aktivatorkomponente deiner Popupbenachrichtigung zu implementieren.

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

Für die Implementierung der obigen Co-Klasse wird das gleiche Muster eingehalten, das in [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class) veranschaulicht wird. Du kannst also das gleiche Verfahren nutzen, um COM-Schnittstellen und Windows-Runtime-Schnittstellen zu implementieren. Die Features von COM-Komponenten und Windows-Runtime-Klassen werden über Schnittstellen verfügbar gemacht. Jede COM-Schnittstelle wird letztendlich von der [**IUnknown-Schnittstelle**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) abgeleitet. Die Windows-Runtime basiert auf COM. Ein Unterscheidungsmerkmal ist, dass Windows-Runtime-Schnittstellen letztendlich von der [**IInspectable-Schnittstelle**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) abgeleitet werden (und **IInspectable** von **IUnknown**).

In der Co-Klasse im obigen Code implementieren wir die **INotificationActivationCallback::Activate**-Methode. Dies ist die Funktion, die aufgerufen wird, wenn der Benutzer in einer Popupbenachrichtigung auf die Rückrufschaltfläche klickt. Bevor diese Funktion aufgerufen werden kann, muss eine Instanz der Co-Klasse erstellt werden. Dies ist Aufgabe der Funktion **IClassFactory::CreateInstance**.

Die Co-Klasse, die wir gerade implementiert haben, wird als *COM-Aktivator* für Benachrichtigungen bezeichnet und verfügt über eine Klassen-ID (CLSID) in Form des oben angegebenen `callback_guid`-Bezeichners (vom Typ **GUID**). Wir verwenden diesen Bezeichner später in Form einer Startmenüverknüpfung und eines Windows-Registrierungseintrags. Die CLSID des COM-Aktivators und der Pfad zum zugeordneten COM-Server (der Pfad zur ausführbaren Datei, die wir hier erstellen) stellen den Mechanismus dar, mit dem eine Popupbenachrichtigung die Information erhält, von welcher Klasse beim Klicken auf die Rückrufschaltfläche eine Instanz erstellt werden muss (ob im Info-Center auf die Benachrichtigung geklickt wird oder nicht).

## <a name="best-practices-for-implementing-com-methods"></a>Bewährte Methoden für die Implementierung von COM-Methoden

Die Verfahren für die Fehlerbehandlung und die Ressourcenverwaltung können Hand in Hand gehen. Die Verwendung von Ausnahmen anstelle von Fehlercodes ist bequemer und praktischer. Falls du die Programmiertechnik „Ressourcenbelegung ist Initialisierung“ (Resource Acquisition Is Initialization, RAII) verwendest, kannst du das explizite Überprüfen auf Fehlercodes und das anschließende Freigeben von Ressourcen vermeiden. Überprüfungen dieser Art verkomplizieren deinen Code unnötig, und es ergeben sich viele Stellen, an denen sich Fehler einschleichen können. Verwende stattdessen RAII und Throw/Catch-Ausnahmen (Auslösen/Abfangen). Bei dieser Vorgehensweise sind deine Ressourcenzuordnungen vor Ausnahmen geschützt, und dein Code bleibt einfach.

Du darfst allerdings nicht zulassen, dass Ausnahmen deine COM-Methodenimplementierungen umgehen. Dies kannst du sicherstellen, indem du den Spezifizierer `noexcept` in deinen COM-Methoden nutzt. Es ist kein Problem, wenn im Aufrufdiagramm deiner Methode Ausnahmen ausgelöst werden, solange diese behandelt werden, bevor deine Methode beendet wird. Wenn du `noexcept` verwendest und dann zulässt, dass eine Ausnahme deine Methode umgeht, wird deine Anwendung beendet.

## <a name="add-helper-types-and-functions"></a>Hinzufügen von Hilfstypen und -funktionen

In diesem Schritt fügen wir einige Hilfstypen und -funktionen hinzu, die vom restlichen Code genutzt werden. Füge also direkt vor `main` Folgendes hinzu:

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementieren der restlichen Funktionen und der Einstiegspunktfunktion „wmain“

Lösche deine Funktion `main`, und füge stattdessen diese Codeauflistung ein, die Code zum Registrieren deiner Co-Klasse enthält. Anschließend wird ein Popup bereitgestellt, mit dem für deine Anwendung ein Rückruf durchgeführt werden kann.

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>Testen der Beispielanwendung

Erstelle die Anwendung, und führe sie dann mindestens einmal als Administrator aus, damit Code für die Registrierung und andere Setupschritte ausgeführt wird. Eine Möglichkeit besteht hierbei darin, Visual Studio als Administrator und dann die App über Visual Studio auszuführen. Klicke in der Taskleiste mit der rechten Maustaste auf „Visual Studio“, um die Sprungliste anzuzeigen, klicke darin mit der rechten Maustaste auf „Visual Studio“, und klicke dann auf **Als Administrator ausführen**. Stimme in der Eingabeaufforderung zu, und öffne anschließend das Projekt. Wenn du die Anwendung ausführst, wird eine Meldung mit einem Hinweis angezeigt, ob die Anwendung als Administrator ausgeführt wird. Wenn nicht, können der Registrierungsvorgang und andere Setupschritte nicht ausgeführt werden. Der Registrierungsvorgang und die anderen Setupschritte müssen mindestens einmal erfolgen, damit die Anwendung richtig funktioniert.

Drücke unabhängig davon, ob du die Anwendung als Administrator ausführst, die Taste „T“, damit ein Popup angezeigt wird. Du kannst dann entweder direkt in der angezeigten Popupbenachrichtigung oder im Info-Center auf die Schaltfläche **Call back ToastAndCallback** (Rückruf ToastAndCallback) klicken. Deine Anwendung wird gestartet, die Co-Klasse wird instanziiert, und die **INotificationActivationCallback::Activate**-Methode wird ausgeführt.

## <a name="in-process-com-server"></a>In-Process-COM-Server

Die Beispiel-App *ToastAndCallback* dient als lokaler COM-Server („Out-of-Process“). Dies wird durch den Windows-Registrierungsschlüssel [LocalServer32](/windows/desktop/com/localserver32) angegeben, den du zum Registrieren der CLSID der zugehörigen Co-Klasse verwendest. Ein lokaler COM-Server hostet seine Co-Klassen in einer ausführbaren Binärdatei (`.exe`).

Alternativ kannst du auch die Entscheidung treffen, deine Co-Klassen in einer Dynamic Link Library (`.dll`) zu hosten (die wahrscheinlichere Vorgehensweise). Ein COM-Server in Form einer DLL wird als In-Process-COM-Server bezeichnet. Dies wird angegeben, indem CLSIDs über den Windows-Registrierungsschlüssel [InprocServer32](/windows/desktop/com/inprocserver32) registriert werden.

### <a name="create-a-dynamic-link-library-dll-project"></a>Erstellen eines DLL-Projekts (Dynamic Link Library)

Du kannst mit dem Erstellen eines In-Process-COM-Servers beginnen, indem du in Microsoft Visual Studio ein neues Projekt erstellst. Erstelle ein Projekt vom Typ **Visual C++**  > **Windows Desktop** > **Dynamic Link Library (DLL)** .

Führe zum Hinzufügen von C++/WinRT-Unterstützung zum neuen Projekt die Schritte aus, die unter [Ändern eines Windows Desktop-Anwendungsprojekts, um C++/WinRT-Unterstützung hinzuzufügen](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support) beschrieben sind.

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementieren der Co-Klasse, Klassenfactory und In-Process-Serverexporte

Öffne `dllmain.cpp`, und füge der Datei die unten angegebene Codeauflistung hinzu.

Wenn du bereits über eine DLL verfügst, mit der C++/WinRT-Windows-Runtime-Klassen implementiert werden, ist die unten dargestellte Funktion **DllCanUnloadNow** bereits vorhanden. Falls du dieser DLL Co-Klassen hinzufügen möchtest, kannst du die Funktion **DllGetClassObject** hinzufügen.

Falls kein Code vom Typ [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) vorhanden ist, für den auf Kompatibilität geachtet werden muss, kannst du die WRL-Teile aus dem angezeigten Code entfernen.

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>Unterstützung für schwache Verweise

Siehe auch [Schwache Verweise in C++/WinRT](weak-references.md#weak-references-in-cwinrt).

Unter C++/WinRT (über die Basisstrukturvorlage [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)) wird [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) für dich implementiert, wenn dein Typ [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) implementiert (oder eine andere Schnittstelle, die von **IInspectable** abgeleitet ist).

Der Grund ist, dass **IWeakReferenceSource** und [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) für Windows-Runtime-Typen entworfen wurden. Du kannst die Unterstützung für schwache Verweise also für deine Co-Klasse aktivieren, indem du deiner Implementierung einfach **winrt::Windows::Foundation::IInspectable** hinzufügst (oder eine Schnittstelle, die von **IInspectable** abgeleitet ist).

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>Wichtige APIs
* [Schnittstelle „IInspectable“](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Schnittstelle „IUnknown“](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Strukturvorlage „winrt::implements“](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Zugehörige Themen
* [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Verwenden von COM-Komponenten mit C++/WinRT](consume-com.md)
* [Senden einer lokalen Popupbenachrichtigung](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
