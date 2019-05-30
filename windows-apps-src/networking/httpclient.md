---
description: Verwenden Sie HttpClient und die übrige Windows.Web.Http-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 988cd2fafbb0cd632711dc2eaa5f6a6db8eed7e4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371381"
---
# <a name="httpclient"></a>HttpClient

**Wichtige APIs**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

Verwenden Sie [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) und die übrige [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Übersicht über HttpClient und den Windows.Web.Http-Namespace

Die Klassen im [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)-Namespace und die zugehörigen [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers)- und [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters)-Namespaces stellen eine Programmierschnittstelle für UWP-Apps bereit, die als HTTP-Client ausgeführt werden, um grundlegende GET-Anforderungen auszuführen oder die weiter unten aufgeführte erweiterte HTTP-Funktionalität zu implementieren.

-   Methoden für häufige Verben (**DELETE**, **GET**, **PUT** und **POST**). Jede dieser Anforderungen wird als asynchroner Vorgang gesendet.

-   Unterstützung für häufige Authentifizierungseinstellungen und -muster.

-   Zugriff auf SSL-Details (Secure Sockets Layer) zum Transport.

-   Möglichkeit zum Einbinden benutzerdefinierter Filter in erweiterte Apps.

-   Möglichkeit zum Abrufen, Festlegen und Löschen von Cookies.

-   Verfügbarkeit von Statusinformationen zu HTTP-Anforderungen in asynchronen Methoden.

Die [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage)-Klasse stellt eine HTTP-Anforderungsnachricht dar, die von [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) gesendet wurde. Die [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)-Klasse stellt eine HTTP-Antwortnachricht dar, die von einer HTTP-Anforderung empfangen wurde. HTTP-Nachrichten werden von IETF in [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) definiert.

Der [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)-Namespace stellt HTTP-Inhalte als HTTP-Entitätskörper und Header dar, darunter auch Cookies. HTTP-Inhalte können einer HTTP-Anforderung oder einer HTTP-Antwort zugeordnet sein. Der **Windows.Web.Http**-Namespace stellt eine Reihe unterschiedlicher Klassen zum Darstellen von HTTP-Inhalten bereit.

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent). Inhalt als Puffer
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). Inhalt als Name und Wert-Tupel, die mit dem MIME-Typ **application/x-www-form-urlencoded** codiert sind
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent). Inhalt in Form von der **multipart /\***  MIME-Typ.
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). Inhalt, der als MIME-Typ **multipart/form-data** codiert ist
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent). Inhalt als Stream (Der interne Typ wird von der HTTP GET-Methode zum Empfangen von Daten und von der HTTP POST-Methode zum Hochladen von Daten verwendet.)
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent). Inhalt als Zeichenfolge
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -eine Basisschnittstelle für Entwickler ihre eigenen Inhalte Objekte erstellen

Der Codeausschnitt im Abschnitt „Senden einer einfachen GET-Anforderung über HTTP“ verwendet die [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent)-Klasse, um die HTTP-Antwort einer HTTP GET-Anforderung als Zeichenfolge darzustellen.

Der [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers)-Namespace unterstützt die Erstellung von HTTP-Headern und -Cookies, die dann als Eigenschaften mit [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage)- und [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)-Objekten zugeordnet werden.

## <a name="send-a-simple-get-request-over-http"></a>Senden einer einfachen GET-Anforderung über HTTP

Wie weiter oben in diesem Artikel beschrieben wurde, können UWP-Apps mit dem [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)-Namespace GET-Anforderungen senden. Der folgende Codeausschnitt veranschaulicht, wie eine GET-Anforderung senden http://www.contoso.com mithilfe der [ **"Windows.Web.http.HttpClient"** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) Klasse und die [  **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) Klasse, um die Antwort der GET-Anforderung zu lesen.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>Über HTTP POST Binärdaten

Die [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis) nachfolgenden Codebeispiel veranschaulicht die Verwendung von Formulardaten und eine POST-Anforderung um eine kleine Menge von Binärdaten als eine Datei hoch auf einem Webserver zu senden. Der Code verwendet die [ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent) Klasse, um die binären Daten darstellen, und die [ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent) -Klasse Stellt die mehrteilige Formulardaten dar.

> [!NOTE]
> Aufrufen von **erhalten** (wie im folgenden Codebeispiel wird) ist für einen UI-Thread. Das richtige Verfahren in diesem Fall verwendet, finden Sie unter [Parallelität und asynchrone Vorgänge mit C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

Um den Inhalt einer Binärdatei (und nicht die explizite binären Daten, die oben verwendeten) zu senden, Sie finden es einfacher zu verwenden eine [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) Objekt. Erstellt einen, und übergeben Sie als Argument an den Konstruktor, den von einem Aufruf zurückgegebenen Wert [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Diese Methode gibt einen Stream für die Daten in Ihrer binären Datei zurück.

Auch wenn Sie eine große Datei (größer als ca. 10 MB) hochladen, dann wird empfohlen, der Windows-Runtime verwenden [Hintergrundübertragung](/uwp/api/windows.networking.backgroundtransfer) APIs.

## <a name="exceptions-in-windowswebhttp"></a>Ausnahmen in „Windows.Web.Http“

Eine Ausnahme wird ausgelöst, wenn eine ungültige Zeichenfolge für einen Uniform Resource Identifier (URI) an den Konstruktor für das [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Objekt übergeben wird.

**.NET:**   der [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) Typ wird als [ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) in C# und VB.

In C# und Visual Basic kann dieser Fehler vermieden werden, indem die [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)-Klasse in .NET 4.5 und eine der [**System.Uri.TryCreat**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads)-Methoden zum Testen der von einem Benutzer erhaltenen Zeichenfolge verwendet wird, bevor der URI erstellt wird.

In C++ gibt es keine Methode zum Analysieren einer Zeichenfolge für einen URI. Wenn eine App vom Benutzer eine Eingabe für das [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)-Element erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) bietet keine Funktion, die die Behandlung von Ausnahmen erleichtert. Eine App, die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) und andere Klassen in diesem Namespace verwendet, muss daher den **HRESULT**-Wert verwenden.

In apps mit .NET Framework 4.5 in C#, VB.NET der ["System.Exception"](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN) stellt einen Fehler während der Ausführung der app dar, wenn eine Ausnahme auftritt. Die [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message)-Eigenschaft gibt die Meldung zurück, die die Ausnahme beschreibt. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

In Apps mit verwaltetem C++ stellt das [Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class)-Objekt einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message)-Eigenschaft gibt die vom System bereitgestellte Zeichenfolge zurück, die dem **HRESULT**-Wert zugeordnet ist. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Für die meisten Parameter Validierungsfehler die **HRESULT** zurückgegebene **E\_INVALIDARG**. Für einige unzulässige Methodenaufrufe die **HRESULT** zurückgegebene **E\_UNZULÄSSIGE\_Methode\_Aufrufen**.

