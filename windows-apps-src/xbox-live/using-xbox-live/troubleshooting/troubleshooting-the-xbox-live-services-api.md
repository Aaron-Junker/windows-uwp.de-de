---
title: Problembehandlung bei der Xbox Live Services-API
description: Erfahren Sie, wie sich die zusätzlichen Fehlerinformationen, die beim Behandeln von Problemen mit der Xbox Live-APIs.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Problembehandlung, Fehler, Protokoll
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621415"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Problembehandlung für die Xbox Live-APIs

## <a name="code"></a>Code

Es ist schwer zu diagnostizieren ein Fehlers, der nur den Fehler von der Xbox Live Services-API-Ebene verwenden. Zusätzliche Fehlerinformationen – z. B. die Protokollierung für alle RESTful-Aufrufe, kann für den Server verfügbar sein. Um diese Daten zu überwachen, die Antwort-Protokollierung binden Sie ein, und aktivieren Sie der Debugablaufverfolgung. Antwort-Protokollierung können Sie HTTP-Datenverkehr und Web Service-Antwortcodes, finden Sie unter dem ist häufig so nützlich wie eine Fiddler-Ablaufverfolgung.

### <a name="c"></a>C++

Im folgenden Codebeispiel wird aktiviert die Protokollierung der Antwort und legt die Debug-Error-Ebene auf ausführlich (Sie können auch festlegen Fehlerstufe Debuggen Fehler angezeigt wird, nur Ablaufverfolgung Aufrufe fehlerhafte oder zum Deaktivieren der Ablaufverfolgung aus). Die Debugausgabe wird in den Bereich Ausgabe gesendet, wenn Ihr Projekt in Visual Studio ausführen.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

Sie können auch zum Umleiten von Debugausgaben in Ihre eigenen Protokolldatei wie folgt:

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
