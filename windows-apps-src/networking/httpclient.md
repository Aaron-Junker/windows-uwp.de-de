---
description: Verwenden Sie HttpClient und die übrige Windows.Web.Http-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 06/05/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bb97e58e914da1982066d9cf150f5c8b18ef884a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155444"
---
# <a name="httpclient"></a>HttpClient

**Wichtige APIs**

-   [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)

Verwenden Sie [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) und die übrige [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)-Namespace-API zum Senden und Empfangen von Informationen über die HTTP-Protokolle 1.1 und 2.0.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Übersicht über HttpClient und den Windows.Web.Http-Namespace

Die Klassen im [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)-Namespace und die zugehörigen [**Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers)- und [**Windows.Web.Http.Filters**](/uwp/api/Windows.Web.Http.Filters)-Namespaces stellen eine Programmierschnittstelle für UWP-Apps bereit, die als HTTP-Client ausgeführt werden, um grundlegende GET-Anforderungen auszuführen oder die weiter unten aufgeführte erweiterte HTTP-Funktionalität zu implementieren.

-   Methoden für häufige Verben (**DELETE**, **GET**, **PUT** und **POST**). Jede dieser Anforderungen wird als asynchroner Vorgang gesendet.

-   Unterstützung für häufige Authentifizierungseinstellungen und -muster.

-   Zugriff auf SSL-Details (Secure Sockets Layer) zum Transport.

-   Möglichkeit zum Einbinden benutzerdefinierter Filter in erweiterte Apps.

-   Möglichkeit zum Abrufen, Festlegen und Löschen von Cookies.

-   Verfügbarkeit von Statusinformationen zu HTTP-Anforderungen in asynchronen Methoden.

Die [**Windows.Web.Http.HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage)-Klasse stellt eine HTTP-Anforderungsnachricht dar, die von [**Windows.Web.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) gesendet wurde. Die [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)-Klasse stellt eine HTTP-Antwortnachricht dar, die von einer HTTP-Anforderung empfangen wurde. HTTP-Nachrichten werden von IETF in [RFC 2616](https://tools.ietf.org/html/rfc2616) definiert.

Der [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)-Namespace stellt HTTP-Inhalte als HTTP-Entitätskörper und Header dar, darunter auch Cookies. HTTP-Inhalte können einer HTTP-Anforderung oder einer HTTP-Antwort zugeordnet sein. Der **Windows.Web.Http**-Namespace stellt eine Reihe unterschiedlicher Klassen zum Darstellen von HTTP-Inhalten bereit.

-   [**HttpBufferContent**](/uwp/api/Windows.Web.Http.HttpBufferContent). Inhalt als Puffer
-   [**HttpFormUrlEncodedContent**](/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). Inhalt als Name und Wert-Tupel, die mit dem MIME-Typ **application/x-www-form-urlencoded** codiert sind
-   [**HttpMultipartContent**](/uwp/api/Windows.Web.Http.HttpMultipartContent). Inhalt in Form des MIME-Typs **multipart/\*** .
-   [**HttpMultipartFormDataContent**](/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). Inhalt, der als MIME-Typ **multipart/form-data** codiert ist
-   [**HttpStreamContent**](/uwp/api/Windows.Web.Http.HttpStreamContent). Inhalt als Stream (Der interne Typ wird von der HTTP GET-Methode zum Empfangen von Daten und von der HTTP POST-Methode zum Hochladen von Daten verwendet.)
-   [**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent). Inhalt als Zeichenfolge
-   [**IHttpContent**](/uwp/api/Windows.Web.Http.IHttpContent): Basisschnittstelle für Entwickler zum Erstellen eigener Inhaltsobjekte

Der Codeausschnitt im Abschnitt „Senden einer einfachen GET-Anforderung über HTTP“ verwendet die [**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent)-Klasse, um die HTTP-Antwort einer HTTP GET-Anforderung als Zeichenfolge darzustellen.

Der [**Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers)-Namespace unterstützt die Erstellung von HTTP-Headern und -Cookies, die dann als Eigenschaften mit [**HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage)- und [**HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)-Objekten zugeordnet werden.

## <a name="send-a-simple-get-request-over-http"></a>Senden einer einfachen GET-Anforderung über HTTP

Wie weiter oben in diesem Artikel beschrieben wurde, können UWP-Apps mit dem [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)-Namespace GET-Anforderungen senden. Der folgende Codeausschnitt veranschaulicht, wie eine GET-Anforderung mithilfe der [**Windows.Web.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Klasse und der [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)-Klasse an \/„http://www.contoso.com“ gesendet werden kann, um die Antwort der GET-Anforderung zu lesen.

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

## <a name="post-binary-data-over-http"></a>Binäre Daten per POST über HTTP

Das folgende [C++/WinRT](../cpp-and-winrt-apis/index.md)-Codebeispiel veranschaulicht die Verwendung von Formulardaten und einer POST-Anforderung, um eine kleine Menge von Binärdaten als Dateiupload an einen Webserver zu senden. Der Code verwendet die [**HttpBufferContent**](/uwp/api/windows.web.http.httpbuffercontent)-Klasse, um die binären Daten darstellen, und die [**HttpMultipartFormDataContent**](/uwp/api/windows.web.http.httpmultipartformdatacontent)-Klasse, um die mehrteiligen Formulardaten darzustellen.

> [!NOTE]
> Das Aufrufen von **get** (wie im folgenden Codebeispiel zu sehen) ist für einen UI-Thread nicht geeignet. Die in diesem Fall zu verwendende richtige Methode finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

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

Um den Inhalt einer tatsächlichen Binärdatei (anstatt die oben verwendeten, expliziten binären Daten) per POST zu senden, ist es einfacher, ein [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent)-Objekt zu verwenden. Erstellen Sie eins, und übergeben Sie als Argument an seinen Konstruktor den von einem Aufruf von [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync) zurückgegebenen Wert. Diese Methode gibt einen Stream für die Daten in Ihrer binären Datei zurück.

Auch wenn Sie eine große Datei (größer als ca. 10 MB) hochladen, empfehlen wir, dass Sie die [Hintergrundübertragung](/uwp/api/windows.networking.backgroundtransfer)s-APIs der Windows-Runtime verwenden.

## <a name="post-json-data-over-http"></a>JSON-Daten per POST über HTTP

Im folgende Beispiel wird JSON-Code per POST an einem Endpunkt bereitgestellt und dann der Antworttext als Ausgabe geschrieben.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

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

    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        Windows::Web::Http::HttpClient httpClient;
        Uri requestUri{ L"https://www.contoso.com/post" };

        // Construct the JSON to post.
        Windows::Web::Http::HttpStringContent jsonContent(
            L"{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding::Utf8,
            L"application/json");

        // Post the JSON, and wait for a response.
        httpResponseMessage = httpClient.PostAsync(
            requestUri,
            jsonContent).get();

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
        std::wcout << httpResponseBody.c_str();
    }
    catch (winrt::hresult_error const& ex)
    {
        std::wcout << ex.message().c_str();
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Ausnahmen in „Windows.Web.Http“

Eine Ausnahme wird ausgelöst, wenn eine ungültige Zeichenfolge für einen Uniform Resource Identifier (URI) an den Konstruktor für das [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)-Objekt übergeben wird.

**.NET:**   Der [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)-Typ wird in C# und VB als [**System.Uri**](/dotnet/api/system.uri) angezeigt.

In C# und Visual Basic kann dieser Fehler vermieden werden, indem die [**System.Uri**](/dotnet/api/system.uri)-Klasse in .NET 4.5 und eine der [**System.Uri.TryCreat**](/dotnet/api/system.uri.trycreate#overloads)-Methoden zum Testen der von einem Benutzer erhaltenen Zeichenfolge verwendet wird, bevor der URI erstellt wird.

In C++ gibt es keine Methode zum Analysieren einer Zeichenfolge für einen URI. Wenn eine App vom Benutzer eine Eingabe für das [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)-Element erhält, sollte sich der Konstruktor innerhalb eines try/catch-Blocks befinden. Wenn eine Ausnahme ausgelöst wird, kann die App den Benutzer benachrichtigen und einen neuen Hostnamen anfordern.

[  **Windows.Web.Http**](/uwp/api/Windows.Web.Http) bietet keine Funktion, die die Behandlung von Ausnahmen erleichtert. Eine App, die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) und andere Klassen in diesem Namespace verwendet, muss daher den **HRESULT**-Wert verwenden.

In Apps in C# und VB.NET, die .NET Framework 4.5 verwenden, stellt [System.Exception](/dotnet/api/system.exception) einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [System.Exception.HResult](/dotnet/api/system.exception.hresult#System_Exception_HResult)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [System.Exception.Message](/dotnet/api/system.exception.message#System_Exception_Message)-Eigenschaft gibt die Meldung zurück, die die Ausnahme beschreibt. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

In Apps mit verwaltetem C++ stellt das [Platform::Exception](/cpp/cppcx/platform-exception-class)-Objekt einen Fehler während der App-Ausführung dar, wenn eine Ausnahme auftritt. Die [Platform::Exception::HResult](/cpp/cppcx/platform-exception-class#hresult)-Eigenschaft gibt den **HRESULT**-Wert zurück, der der jeweiligen Ausnahme zugewiesen ist. Die [Platform::Exception::Message](/cpp/cppcx/platform-exception-class#message)-Eigenschaft gibt die vom System bereitgestellte Zeichenfolge zurück, die dem **HRESULT**-Wert zugeordnet ist. Mögliche **HRESULT**-Werte sind in der Headerdatei *Winerror.h* aufgeführt. Eine App kann nach bestimmten **HRESULT**-Werten filtern, um das App-Verhalten je nach Ausnahmeursache zu ändern.

Für die meisten Parameterüberprüfungsfehler wird der **HRESULT**-Wert **E\_INVALIDARG** zurückgegeben. Bei manchen unzulässigen Methodenaufrufen wird der **HRESULT**-Wert **E\_ILLEGAL\_METHOD\_CALL** zurückgegeben.