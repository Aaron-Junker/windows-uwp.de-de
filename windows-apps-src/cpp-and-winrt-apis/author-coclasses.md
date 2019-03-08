---
description: C++ / WinRT können Sie zum Erstellen von klassischen COM-Komponenten, so wie es für Windows-Runtime-Klassen erstellen kann.
title: Erstellen von COM-Komponenten mit C++ / WinRT
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, Projektion, Autor, COM, Component
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e6b77f8be6c75070336ad48f0c6471fc0a824a4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616565"
---
# <a name="author-com-components-with-cwinrt"></a>Erstellen von COM-Komponenten mit C++ / WinRT

[C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) können Sie zum Erstellen von klassischen Component Object Model (COM)-Komponenten (oder Co-Klassen), so wie es für Windows-Runtime-Klassen erstellen kann. Hier ist eine einfache Darstellung, die Sie testen können, wenn Sie den Code Einfügen der `pch.h` und `main.cpp` eines neuen **Windows-Konsolenanwendung (C++ / WinRT)** Projekt.

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

Siehe auch [nutzen COM-Komponenten mit C++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Ein Beispiel für mehr realistischer und interessanter

Im weiteren Verlauf dieses Themas enthält Informationen zum Erstellen einer minimalen Konsolenanwendungsprojekt, das C++ verwendet c++ / WinRT zur Implementierung einer einfachen Co-Klasse (COM-Komponente oder COM-Klasse) und eine Klassenfactory. Die beispielanwendung zeigt, wie mit einer Rückruf-Schaltfläche auf, und die Co-Klasse eine Toast-Benachrichtigung zu übermitteln (implementiert die **INotificationActivationCallback** COM-Schnittstelle) kann die Anwendung gestartet und aufgerufen werden Sichern Sie klickt der Benutzer die Schaltfläche für den Toast.

Weitere Hintergrundinformationen zu den Funktionsbereich des Toast-Benachrichtigung finden Sie unter [lokalen Popupbenachrichtigung sendet](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Verwenden Sie keines der Codebeispiele in diesem Abschnitt der Dokumentation zu C++ / WinRT, jedoch, also ist es empfehlenswert, dass Sie den Code in diesem Thema gezeigten bevorzugen.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Erstellen Sie ein Projekt für Windows-Konsolenanwendung (ToastAndCallback)

Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie eine **Visual C++** > **Windows Desktop** > **Windows-Konsolenanwendung (C++ / WinRT)** Projekt, und nennen Sie sie  *ToastAndCallback*.

Open `pch.h`, und fügen `#include <unknwn.h>` vor der enthält für C++ / WinRT-Header.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Open `main.cpp`, und entfernen Sie die mithilfe von-Anweisungen, die von die Projektvorlage generiert. Fügen Sie den folgenden Code (der bietet uns die Bibliotheken, Headern und Typnamen, die wir benötigen), an ihre Stelle.

```cppwinrt
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
```

## <a name="implement-the-coclass-and-class-factory"></a>Implementieren Sie die Co-Klasse und Klasse-factory

In C++ / WinRT, Sie implementieren, Co-Klassen, und Klassenfactorys, durch Ableiten von der [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) Basisstruktur. Unmittelbar nach der folgenden drei mithilfe von-Anweisungen oben (und bevor `main`), fügen Sie diesen Code zum Implementieren des Komponentennamens Activator Toast-Benachrichtigung COM.

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

Die Implementierung der oben genannten Co-Klasse folgt dem gleichen Muster, die gezeigt wird [Autor-APIs mit C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Daher können Sie das gleiche Verfahren, um COM-Schnittstellen als auch für Windows-Runtime-Schnittstellen zu implementieren. COM-Komponenten und Windows-Runtime-Klassen machen ihre Funktionen über Schnittstellen verfügbar. Jede COM-Schnittstelle abgeleitet aus den [ **IUnknown-Schnittstelle** ](https://msdn.microsoft.com/library/windows/desktop/ms680509) Schnittstelle. Die Windows-Runtime basiert auf COM&mdash;leiten Sie einen Unterschied, dass die Windows-Runtime-Schnittstellen letztendlich von der [ **von iinspectable abgeleitete Schnittstelle** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (und **"iinspectable"**  leitet sich von **IUnknown**).

In der Co-Klasse im obigen Code, implementieren wir die **INotificationActivationCallback::Activate** Methode, die die Funktion, die aufgerufen wird, wenn der Benutzer die Rückruf-Schaltfläche auf eine toastbenachrichtigung klickt. Aber bevor diese Funktion aufgerufen werden kann, muss eine Instanz der Co-Klasse erstellt werden, und die Aufgabe der **IClassFactory:: CreateInstance** Funktion.

Die Co-Klasse, die wir gerade implementiert sogenannte der *COM Activator* für Benachrichtigungen und weist der Klassen-Id (CLSID) in Form von der `callback_guid` Bezeichner (vom Typ **GUID**), die Sie oben sehen. Wir werden diesen Bezeichner später in Form von eine Verknüpfung im Startmenü und ein Windows-Registrierungseintrag verwenden. Die CLSID des COM-Aktivierung und den Pfad zu der zugeordnete COM-Server (die den Pfad der ausführbaren Datei, die wir hier erstellen) ist der Mechanismus, mit dem eine Popupbenachrichtigung weiß, welche Klasse erstellt eine Instanz der, wenn die Rückruf-Schaltfläche geklickt wurde (ob die Benachrichtigung geklickt wird, im Info-Center oder nicht).

## <a name="best-practices-for-implementing-com-methods"></a>Bewährte Methoden für die Implementierung von COM-Methoden

Techniken für die Fehlerbehandlung und ressourcenverwaltung können Hand in Hand gehen. Es ist praktisch und praktische Verwendung von Ausnahmen als Fehlercodes. Und wenn Sie die Resource Acquisition is Initialization (RAII)-Technik verwenden, Sie können vermeiden explizit Fehlercodes suchen, und klicken Sie dann Ressourcen explizit freizugeben. Solchen Überprüfungen aus expliziten Lesbarkeit Ihres Codes, die mehr als notwendig unübersichtlich, und gibt Fehler viele Stellen, an denen ausblenden. Stattdessen verwenden Sie RAII und Throw/Catch-Ausnahmen. Auf diese Weise Ihre ressourcenzuordnungen sind, und Ihr Code ist einfach.

Allerdings darf nicht Sie Ausnahmen für Ihre Implementierungen von COM-Methode mit Escapezeichen versehen können. Sie können sicherstellen, dass mit der `noexcept` Bezeichner auf die COM-Methoden. Es ist in Ordnung für Ausnahmen an einer beliebigen Stelle im Aufrufdiagramm der Methode, solange Sie sie handhaben, bevor die Methode beendet wird. Bei Verwendung von `noexcept`, aber Sie können dann eine Ausnahme von der Methode mit Escapezeichen versehen, und klicken Sie dann Ihre Anwendung wird beendet.

## <a name="add-helper-types-and-functions"></a>Hinzufügen von Hilfstypen und -Funktionen

In diesem Schritt fügen wir, dass einige Hilfstypen und -Funktionen, mit der der Rest des Codes, verwenden. In diesem Fall vor dem `main`, fügen Sie Folgendes hinzu.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementieren Sie die übrigen Funktionen und die Einstiegspunktfunktion von "wmain"

Die Projektvorlage generiert eine `main` -Funktion für Sie. Löschen, die `main` funktionieren und an seiner Stelle einfügen dieser Code auflisten, die Code zum Registrieren Ihrer Co-Klasse enthält, und klicken Sie dann eine toastfähig der Rückrufe für Ihre Anwendung bereitstellen.

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

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

## <a name="how-to-test-the-example-application"></a>Vorgehensweise: Testen die beispielanwendung

Erstellen Sie die Anwendung, und klicken Sie dann als Administrator, um die Registrierung und anderen Setup, Ausführen von Code, dazu führen, dass mindestens einmal ausgeführt. Unabhängig davon, ob Sie sie als Administrator ausführen, und drücken Sie dann ' t ', die dazu führen, dass ein Popup angezeigt werden. Klicken Sie anschließend auf die **einen Rückruf an ToastAndCallback** Schaltfläche direkt aus den Toast-Benachrichtigung, die angezeigt oder von Info-Center und die Anwendung gestartet wird, die Co-Klasse instanziiert, und die  **INotificationActivationCallback::Activate** ausgeführte Methode.

## <a name="in-process-com-server"></a>In-Process-COM-server

Die *ToastAndCallback* obige Beispiel-app-Funktionen als COM-Server lokal (oder Out-of-Process). Dadurch wird angegeben, indem die [LocalServer32](/windows/desktop/com/localserver32) Windows-Registrierungsschlüssel, die Sie verwenden, um die CLSID der Co-Klasse zu registrieren. Lokaler COM-Server hostet die coclass(es) innerhalb eine ausführbare Binärdatei (ein `.exe`).

Sie können auch (und wohl eher), können Sie zum Hosten Ihrer coclass(es) innerhalb einer Dynamic Link Library (eine `.dll`). Ein COM-Server in Form einer DLL als in-Process-COM-Server bezeichnet wird und er angegeben wird, von CLSIDs, die registriert wird, mithilfe der [InprocServer32](/windows/desktop/com/inprocserver32) Windows-Registrierungsschlüssel.

### <a name="create-a-dynamic-link-library-dll-project"></a>Erstellen Sie ein Projekt für die Dynamic Link Library (DLL)

Sie können die Aufgabe des Erstellens von in-Process-COM-Server durch Erstellen eines neuen Projekts in Microsoft Visual Studio beginnen. Erstellen Sie eine **Visual C++** > **Windows Desktop** > **Dynamic Link Library (DLL)** Projekt.

Hinzufügen von C++ / WinRT-Support, um das neue Projekt, befolgen Sie die Schritte in [ändern Sie ein Windows-Desktop-Anwendungsprojekt zum Hinzufügen von C++ / WinRT-Unterstützung](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementieren Sie die Co-Klasse, Klassenfactory und INPROC-Server exportiert

Open `dllmain.cpp`, und fügen Sie das Codebeispiel unten hinzu.

Wenn Sie bereits über eine DLL-Datei verfügen, die C++ implementiert c++ / WinRT-Windows-Runtime-Klassen, dann Sie bereits haben die **"DllCanUnloadNow"** unten angezeigte Funktion. Wenn Sie diese DLL-Co-Klassen hinzufügen möchten, können Sie hinzufügen, die **DllGetClassObject** Funktion.

Wenn keine vorhandenen [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) Teilen Sie Code, dass Sie mit kompatibel bleiben möchten, können Sie die WRL entfernen aus dem Code angezeigt.

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
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
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

Siehe auch [schwache Verweise in C++ / WinRT](weak-references.md#weak-references-in-cwinrt).

C++ / WinRT (insbesondere die [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) Basisstruktur-Vorlage) implementiert [ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) für Sie Wenn Ihre Geben Sie implementiert [ **"iinspectable"** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (oder eine Schnittstelle, die von abgeleitet **"iinspectable"**).

Grund hierfür ist, **IWeakReferenceSource** und [ **"iweakreference"** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) dienen der Windows-Runtime-Typen. Daher können Sie aktivieren schwachen Verweis-Unterstützung für die Co-Klasse einfach durch Hinzufügen von **Winrt::Windows::Foundation::IInspectable** (oder eine Schnittstelle, die abgeleitet **"iinspectable"**) für Ihre Implementierung.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>Wichtige APIs
* [IInspectable-Schnittstelle](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown-Schnittstelle](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Vorlage für WinRT::Implements-Struktur](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Verwandte Themen
* [Erstellen von APIs mit C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Nutzen Sie COM-Komponenten mit C++ / WinRT](consume-com.md)
* [Eine lokale toastbenachrichtigung senden](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
