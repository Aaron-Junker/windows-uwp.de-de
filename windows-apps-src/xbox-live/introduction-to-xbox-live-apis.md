---
title: Einführung in Xbox Live APIs
description: Erfahren Sie, bis die verschiedenen API-Modelle, die Sie für die Interaktion mit der Xbox Live-Dienst verwenden können.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657695"
---
# <a name="introduction-to-xbox-live-apis"></a>Einführung in Xbox Live APIs

## <a name="use-xbox-live-services"></a>Verwenden von Xbox Live-Dienste

Es gibt zwei Möglichkeiten zum Abrufen von Informationen aus dem Xbox Live-Dienste:

- Verwenden Sie eine Client-Side-API, die Xbox Live Services-API aufgerufen (**XSAPI**)
- Rufen Sie die **Xbox Live-REST-Endpunkte** direkt

Die Vorteile der Verwendung der Xbox Live Services-API (**XSAPI**) enthalten:

- Details der Authentifizierung, Codierung und HTTP senden und empfangen sind für Sie übernommen.
- Argumente und Daten zurückgegeben, die Wrapper-API in der systemeigenen Daten Typen behandelt wird. Sie müssen also nicht JSON-Codierung und Decodierung durchführen.
- Direktes Aufrufen von Webdiensten umfasst mehrere asynchrone Schritte aus, die der Wrapper-API-kapselt; Dadurch wird die Title-Code einfacher zu lesen und schreiben.
- Einige Funktionen, z. B. das Schreiben von spielereignisse, ist nur in XSAPI verfügbar.

Die Vorteile der Verwendung der **Xbox Live-REST-Endpunkte** direkt einfügen:

- Die Möglichkeit, Xbox Live-Endpunkten von einem Webdienst aufzurufen
- Die Fähigkeit, Endpunkte aufrufen, die nicht in XSAPI enthalten sind.  XSAPI enthält nur die APIs, die wir, dass es sich bei Spielen verwendet, glauben daher ist es etwas fehlt können uns über die Foren wissen.
- Einige Funktionen, die über die REST-Endpunkte verfügbar verfügen möglicherweise nicht über einen entsprechenden XSAPI Wrapper.

Ihre Spiele und apps sind nicht nur einfach eine der folgenden Methoden verwenden. Sie können den XSAPI Wrapper verwenden und dennoch rufen Sie die REST-Endpunkte direkt bei Bedarf.

## <a name="xbox-live-services-api-overview"></a>Übersicht über die Xbox Live-Dienste-API ##

Die Xbox Live Services-API (**XSAPI**) macht drei Sätze von clientseitigen APIs, die eine Vielzahl von Kundenszenarien unterstützen:

- [XSAPI WinRT-API](#xsapi-winrt-based-api)
- [XSAPI C ++ 11-basierte API](#xsapi-c11-based-api)
- [XSAPI C-basierte API](#xsapi-c-based-api) (**neu ab Juni 2018**)

Vergleichen die APIs:

### <a name="xsapi-winrt-based-api"></a>XSAPI WinRT-basierte API

- Unterstützt Anwendungen mit C++ / CX C#, und JavaScript.
    - C++ / CX ist eine Erweiterung des Microsoft C++ auf WinRT-Programmierung z. B. mit erleichtern ^ als WinRT-Zeiger.
- Unterstützt Anwendungen, die für Xbox One XDK-Plattform und universelle Windows-Plattform (UWP) X86, X64 und ARM-Architekturen.
- Fehler werden behandelt, über die Ausnahmen in allen Sprachen, einschließlich C++ / CX.
- C++ / WinRT wird ebenfalls unterstützt.  Weitere Informationen zu C++ / WinRT finden Sie unter [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Es folgt ein Beispiel der Aufruf der XSAPI WinRT-API mit C++ / WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

Wenn Sie C++ kombinieren möchten c++ / CX und C++ / WinRT, wie Sie sind Migrieren von Code, Sie können dies zu tun, aber ist etwas komplexer.  
Hier ist ein Beispiel für die Verwendung von C++ XSAPI WinRT-API aufruft, c++ / WinRT angegebenen C++ / CX-Benutzer ^ Objekt.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>XSAPI C ++ 11-basierte API

- Verwendet plattformübergreifende ISO C ++ 11-standard
- Unterstützt Anwendungen, geschrieben in C++
- Unterstützt Anwendungen, die für Xbox One XDK-Plattform und universelle Windows-Plattform (UWP) X86, X64 und ARM-Architekturen.
- Fehler werden über std::error_code behandelt.
- Die C ++ 11 Basis-API ist die empfohlene API für C++-Spiele-Engines für eine bessere Leistung und eine bessere Debuggingunterstützung verwenden.
- Wenn Sie in den Xbox Live Creators-Programm sind, definieren Sie vor dem Einfügen des Headers XSAPI XBOX_LIVE_CREATORS_SDK. Dies schränkt die API-Oberfläche auf jene, die von Entwicklern in den Xbox Live Creators-Programm verwendet werden, und ändert die Anmeldemethode für Titel, in dem Creators-Programm funktioniert.  Zum Beispiel:

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C++ / WinRT wird ebenfalls unterstützt.  Weitere Informationen zu C++ / WinRT finden Sie unter [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Verwenden von C++ / WinRT mit der die XSAPI C++-API, einschließlich des Headers XSAPI XSAPI_CPPWINRT zu definieren.  Zum Beispiel:

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

Es folgt ein Beispiel der Aufruf der XSAPI C++-API mit C++ / WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>XSAPI C-basierte API

- Ermöglicht die Titel für die speicherbelegung zu steuern, wann XSAPI aufrufen.
- Ermöglicht die Titel für die vollständige Kontrolle über die Behandlung beim Aufrufen von XSAPI Thread zu erhalten.
- Verwendet eine neue HTTP-Bibliothek, LibHttpClient, für Spieleentwickler konzipiert.

Weitere Informationen finden Sie unter [Einführung in die Xbox Live-C-APIs](xsapi-flat-c.md).
