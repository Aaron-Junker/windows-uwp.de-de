---
description: Erfahren Sie, wie Sie eine lokale Popup Benachrichtigung von anderen Typen von nicht verpackten apps senden und den Benutzer bearbeiten können, indem Sie auf den Toast klicken.
title: Lokale Popup Benachrichtigung von anderen Typen von nicht verpackten apps senden
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from other types of unpackaged apps
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: Windows 10, Senden von Popup Benachrichtigungen, Benachrichtigungen, Senden von Benachrichtigungen, Popup Benachrichtigungen, Vorgehensweise, Schnellstart, erste Schritte, Codebeispiel, Exemplarische Vorgehensweise, andere Arten von apps, ungepackt
ms.localizationpriority: medium
ms.openlocfilehash: 71c0facc0cd77383ac6682f4934334a7356eb0e8
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231542"
---
# <a name="send-a-local-toast-notification-from-other-types-of-unpackaged-apps"></a>Lokale Popup Benachrichtigung von anderen Typen von nicht verpackten apps senden

Wenn Sie eine App entwickeln, die keine msix/UWP-Pakete oder Pakete mit geringer Dichte verwendet und nicht c# oder C++ ist, ist dies die Seite für Sie!

Eine Popup Benachrichtigung ist eine Meldung, die eine APP erstellen und an den Benutzer übermitteln kann, wenn Sie sich derzeit nicht in der APP befinden. Dieser Schnellstart führt Sie durch die Schritte zum Erstellen, bereitzustellen und Anzeigen einer Windows 10-Popup Benachrichtigung. In diesem Schnellstart werden lokale Benachrichtigungen verwendet, die die einfachste zu implementierende Benachrichtigung sind.

> [!IMPORTANT]
> Wenn Sie eine c#-app schreiben, sehen Sie sich die [c#-Dokumentation](send-local-toast.md)an. Wenn Sie eine C++-app schreiben, sehen Sie sich die [C++ UWP](send-local-toast-cpp-uwp.md) -oder [C++ WRL](send-local-toast-desktop-cpp-wrl.md) -Dokumentation an.



## <a name="step-1-register-your-app-in-the-registry"></a>Schritt 1: Registrieren Ihrer APP in der Registrierung

Sie müssen zunächst die Informationen ihrer app in der Registrierung registrieren, einschließlich einer eindeutigen aumid, die Ihre APP identifiziert, des anzeigen Amens Ihrer APP, Ihres Symbols und der GUID eines com-Activators.

```xml
<registryKey keyName="HKEY_LOCAL_MACHINE\Software\Classes\AppUserModelId\<YOUR_AUMID>">
    <registryValue
        name="DisplayName"
        value="My App"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconUri"
        value="C:\icon.png"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconBackgroundColor"
        value="AARRGGBB"
        valueType="REG_SZ" />
    <registryValue
        name="CustomActivator"
        value="{YOUR COM ACTIVATOR GUID HERE}"
        valueType="REG_SZ" />
</registryKey>
```

## <a name="step-2-set-up-your-com-activator"></a>Schritt 2: Einrichten Ihres com-Activators

Sie können jederzeit auf Benachrichtigungen klicken, auch wenn Ihre APP nicht ausgeführt wird. Daher wird die Benachrichtigungs Aktivierung über einen com-Activator verarbeitet. Die com-Klasse muss die- `INotificationActivationCallback` Schnittstelle implementieren. Die GUID für die com-Klasse muss mit der GUID, die Sie im customactivator-Wert der Registrierung angegeben haben, identisch sein.

```cpp
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
```



## <a name="step-3-send-a-toast"></a>Schritt 3: Senden eines Toast

In Windows 10 wird der Inhalt der Popup Benachrichtigung mit einer adaptiven Sprache beschrieben, die eine hohe Flexibilität bei der Anzeige Ihrer Benachrichtigung ermöglicht. Weitere Informationen finden Sie in der Dokumentation zum Popup [Inhalt](adaptive-interactive-toasts.md) .

Wir beginnen mit einer einfachen textbasierten Benachrichtigung. Erstellen Sie den Benachrichtigungs Inhalt (mit der Benachrichtigungs Bibliothek), und zeigen Sie die Benachrichtigung an!

> [!IMPORTANT]
> Sie müssen ihre aumid von einem früheren Zeitpunkt aus verwenden, um die Benachrichtigung zu senden, damit die Benachrichtigung von ihrer App angezeigt wird.

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```cpp
// Construct the toast template
XmlDocument doc;
doc.LoadXml(L"<toast>\
    <visual>\
        <binding template=\"ToastGeneric\">\
            <text></text>\
            <text></text>\
        </binding>\
    </visual>\
</toast>");

// Populate with text and values
doc.SelectSingleNode(L"//text[1]").InnerText(L"Andrew sent you a picture");
doc.SelectSingleNode(L"//text[2]").InnerText(L"Check this out, The Enchantments in Washington!");

// Construct the notification
ToastNotification notif{ doc };

// And send it! Use the AUMID you specified earlier.
ToastNotificationManager::CreateToastNotifier(L"MyPublisher.MyApp").Show(notif);
```


## <a name="step-4-handling-activation"></a>Schritt 4: Behandeln der Aktivierung

Ihr com-Activator wird aktiviert, wenn auf die Benachrichtigung geklickt wird.


## <a name="more-details"></a>Weitere Informationen

### <a name="aumid-restrictions"></a>Aumid-Einschränkungen

Die aumid muss höchstens 129 Zeichen lang sein. Wenn die aumid mehr als 129 Zeichen lang ist, funktionieren geplante Popup Benachrichtigungen nicht. beim Hinzufügen einer geplanten Benachrichtigung wird die folgende Ausnahme angezeigt: *der an einen System Rückruf weiter gegebene Datenbereich ist zu klein. (0x8007007a)*.