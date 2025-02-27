---
description: Mithilfe eines Assistenten in Visual Studio können Sie Pushbenachrichtigungen über einen mobilen Dienst generieren, der unter Verwendung von Azure Mobile Services erstellt wurde.
title: Vom Assistenten für Pushbenachrichtigungen generierter Code
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f9af0301dcf8944127ab814155466335940642f0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034443"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>Vom Assistenten für Pushbenachrichtigungen generierter Code
 

Mithilfe eines Assistenten in Visual Studio können Sie Pushbenachrichtigungen über einen mobilen Dienst generieren, der unter Verwendung von Azure Mobile Services erstellt wurde. Mit dem Visual Studio-Assistenten wird Code als Starthilfe generiert. In diesem Thema wird erläutert, wie der Assistent Ihr Projekt modifiziert, welche Schritte mit dem generierten Code ausgeführt werden, wie der Code verwendet wird und was Sie als Nächstes tun können, um Pushbenachrichtigungen optimal einzusetzen. Weitere Informationen finden Sie unter [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md).

## <a name="how-the-wizard-modifies-your-project"></a>Modifizierung des Projekts durch den Assistenten


Das Projekt wird vom Assistenten für Pushbenachrichtigungen wie folgt modifiziert:

-   Es wird ein Verweis auf den verwalteten Client von Mobile Dienste (MobileServicesManagedClient.dll) hinzugefügt. Dies gilt nicht für JavaScript-Projekte.
-   Unter "services" wird eine Datei in einen Unterordner eingefügt, die den Namen "push.register.cs", "push.register.vb", "push.register.cpp" oder "push.register.js" erhält.
-   Auf dem Datenbankserver für den mobilen Dienst wird eine Datenkanaltabelle erstellt. Die Tabelle enthält Informationen, die zum Senden von Pushbenachrichtigungen an App-Instanzen erforderlich sind.
-   Es werden Skripts für vier Funktionen erstellt: Löschen, Einfügen, Lesen und Aktualisieren.
-   Erstellt ein Skript mit der benutzerdefinierten API "notifyallusers.js", das eine Pushbenachrichtigung an alle Clients sendet.
-   Der Datei "App.xaml.cs", "App.xaml.vb" oder "App.xaml.cpp" wird eine Deklaration hinzugefügt. Für JavaScript-Projekte wird der neuen Datei "service.js" eine Deklaration hinzugefügt. Mithilfe der Deklaration wird ein MobileServiceClient-Objekt deklariert, das die Informationen enthält, die zum Herstellen der Verbindung mit dem mobilen Dienst erforderlich sind. Sie können auf dieses MobileServiceClient-Objekt, das den Namen „ *MyServiceName* Client” trägt, von jeder Seite der App aus zugreifen, indem Sie den Namen „App. *MyServiceName* Client” verwenden.

Die Datei „services.js” enthält den folgenden Code:

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>Registrierung für Pushbenachrichtigungen


In Push. Register. \* registriert die uploadchannel-Methode das Gerät für den Empfang von Pushbenachrichtigungen. Vom Store werden die installierten Instanzen Ihrer App nachverfolgt, und der Kanal für Pushbenachrichtigungen wird bereitgestellt. Siehe [**PushNotificationChannelManager**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager).

Der Client-Code ist für das JavaScript-Back-End und .NET-Back-End identisch. Beim Hinzufügen von Pushbenachrichtigungen zu einem JavaScript-Back-End-Dienst wird eine benutzerdefinierte API zum NotifyAllUsers-Aufruf in der UploadChannel-Methode eingefügt.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

Tags für Push-Benachrichtigungen können Sie auf die Benachrichtigungen auf eine Teilmenge der Clients einschränken. Nutzen Sie die RegisterNative-Methode (oder RegisterNativeAsync-Methode), um die Registrierung für alle Pushbenachrichtigungen ohne Angabe von Tags vorzunehmen, oder stellen Sie durch Registrieren von Tags das zweite Argument, ein Array von Tags, bereit. Wenn Sie die Registrierung mit mindestens einem Tag vornehmen, erhalten Sie nur Benachrichtigungen, die diesen Tags entsprechen.

## <a name="server-side-scripts-javascript-backend-only"></a>Serverseitige Skripts (nur JavaScript-Back-End)


Bei mobilen Dienste, die JavaScript-Back-End verwenden, werden serverseitige Skripts beim Löschen, Einfügen, Lesen oder Aktualisieren von Vorgängen ausgeführt. Von den Skripts werden diese Vorgänge nicht implementiert, sondern sie werden ausgeführt, wenn diese Ereignisse per Clientaufruf der Windows Mobile-REST-API ausgelöst werden. Von den Skripts wird die Steuerung dann an die Vorgänge selbst übergeben, indem „request.execute“ oder „request.respond“ aufgerufen wird, um eine Antwort an den aufrufenden Kontext auszugeben. Weitere Infos finden Sie unter [REST-API für Azure-Mobile Dienste – Referenz](/previous-versions/azure/reference/jj710108(v=azure.100)).

Im serverseitigen Skript steht eine Auswahl von Funktionen zur Verfügung. Weitere Infos erhalten Sie unter [Registrieren von Tabellenvorgängen für Azure-Mobile-Dienste](https://msdn.microsoft.com/library/azure/dn167708.aspx). Eine Referenz mit allen verfügbaren Funktionen finden Sie unter [Serverskript für Mobile Dienste – Referenz](/previous-versions/azure/reference/jj554226(v=azure.100)).

Außerdem wird der folgende benutzerdefinierte API-Code in „Notifyallusers.js“ erstellt:

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

Von der sendNotifications-Funktion wird eine einzelne Benachrichtigung als Popupbenachrichtigung gesendet. Sie können auch andere Arten von Pushbenachrichtigungen verwenden.

**Tipp**  Informationen zu verfügbarer Hilfe beim Bearbeiten von Skripts finden Sie unter [Aktivieren von IntelliSense für serverseitigen JavaScript-Code](https://blogs.msdn.com/b/visualstudio/archive/2013/07/26/enabling-intellisense-for-mobile-services-javascript-in-visual-studio.aspx).

 

## <a name="push-notification-types"></a>Arten von Pushbenachrichtigungen


Von Windows werden auch Benachrichtigungen unterstützt, bei denen es sich nicht um Pushbenachrichtigungen handelt. Allgemeine Informationen zu Benachrichtigungen finden Sie unter [Auswählen einer Benachrichtigungs Übermittlungs Methode](choosing-a-notification-delivery-method.md).

Die Nutzung von Popupbenachrichtigungen ist einfach. Im "Insert.js"-Code der für Sie generierten Kanaltabelle ist dazu ein Beispiel enthalten. Wenn Sie die Verwendung von Kachel- oder Signalbenachrichtigungen planen, müssen Sie eine XML-Vorlage für die Kachel und das Signal erstellen und die Codierung der verpackten Informationen in der Vorlage angeben. Weitere Informationen finden Sie unter [Arbeiten mit Kacheln, Abzeichen und Popup Benachrichtigungen](/previous-versions/windows/apps/hh868259(v=win.10)).

Da Windows auf Pushbenachrichtigungen reagiert, können die meisten dieser Benachrichtigungen verarbeitet werden, wenn die App nicht ausgeführt wird. Mit einer Pushbenachrichtigung können Benutzer beispielsweise auch dann darüber informiert werden, dass eine neue E-Mail-Nachricht verfügbar ist, wenn die lokale Mail-App nicht ausgeführt wird. Von Windows wird eine Popupbenachrichtigung verarbeitet, indem eine Meldung angezeigt wird, z. B. die erste Zeile einer Textnachricht. Bei Kachel- oder Signalbenachrichtigungen aktualisiert Windows die Live-Kachel einer App, um die Anzahl neuer E-Mail-Nachrichten anzugeben. So können Sie Benutzer Ihrer App auffordern, die neuen Informationen mit der App zu prüfen. Ihre App kann unformatierte Benachrichtigungen empfangen, wenn sie ausgeführt wird, und Sie können sie verwenden, um Daten an die App zu senden. Wenn die App nicht ausgeführt wird, können Sie eine Hintergrundaufgabe zum Überwachen von Pushbenachrichtigungen einrichten.

Sie sollten Pushbenachrichtigungen gemäß den Richtlinien für Windows-Apps verwenden, da diese Benachrichtigungen die Ressourcen eines Benutzers verwenden und ggf. ablenkend sind. Weitere Informationen finden Sie unter [Richtlinien und Prüfliste für Pushbenachrichtigungen](./windows-push-notification-services--wns--overview.md).

Wenn Sie Live-Kacheln mit Pushbenachrichtigungen aktualisieren, sollten Sie auch die Richtlinien unter [Richtlinien und Prüfliste für Kacheln und Signale](./creating-tiles.md) beachten.

## <a name="next-steps"></a>Nächste Schritte


### <a name="using-the-windows-push-notification-services-wns"></a>Verwenden der Windows-Pushbenachrichtigungsdienste (Windows Push Notification Service, WNS)

Sie können den Windows-Pushbenachrichtigungsdienst (WNS) auch direkt aufrufen, wenn Mobile Services nicht genügend Flexibilität bietet, wenn Sie Ihren Servercode in C# oder Visual Basic schreiben möchten oder wenn Sie bereits über einen Clouddienst verfügen und darüber Pushbenachrichtigungen senden möchten. Indem Sie WNS direkt aufrufen, können Sie Pushbenachrichtigungen über Ihren eigenen Clouddienst senden. Dies kann beispielsweise eine Workerrolle sein, mit der Daten aus einer Datenbank oder einem anderen Webdienst überwacht werden. Der Clouddienst muss gegenüber WNS authentifiziert sein, damit Pushbenachrichtigungen an Ihre Apps gesendet werden können. Weitere Infos finden Sie unter [So wird's gemacht: Authentifizieren mit dem Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS) (JavaScript)](/previous-versions/windows/apps/hh465407(v=win.10)) oder [(C#/C++/VB)](/previous-versions/windows/apps/hh868206(v=win.10)).

Sie können Pushbenachrichtigungen auch senden, indem Sie in Ihrem mobilen Dienst eine geplante Aufgabe ausführen. Weitere Informationen finden Sie unter [Planen von wiederkehrenden Aufträgen in Mobile Services](/azure/).

**Warnung**  Wenn Sie den Assistenten für Pushbenachrichtigungen einmal ausgeführt haben, führen Sie ihn nicht ein zweites Mal aus, um Registrierungscode für andere mobile Dienste hinzuzufügen. Wenn Sie den Assistenten mehr als einmal pro Projekt ausführen, wird Code generiert, der überlappende Aufrufe in der [**CreatePushNotificationChannelForApplicationAsync**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync)-Methode zur Folge hat. Dies führt zu einer Laufzeitausnahme. Wenn Sie sich für Pushbenachrichtigungen für mehr als einen mobilen Dienst registrieren möchten, führen Sie den Assistenten einmal aus. Schreiben Sie dann den Registrierungscode neu, um sicherzustellen, dass Aufrufe für **CreatePushNotificationChannelForApplicationAsync** nicht zur gleichen Zeit ausgeführt werden. Beispielsweise können Sie dies erreichen, indem Sie den vom Assistenten generierten Code in Push. Register verschieben. \* (einschließlich des Aufrufes " **foratepushnotificationchannelforapplicationasync** ") außerhalb des OnCreated-Ereignisses, aber die Besonderheiten dieser Informationen hängen von der Architektur ihrer App ab.

 

## <a name="related-topics"></a>Verwandte Themen


* [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](windows-push-notification-services--wns--overview.md)
* [Übersicht über unformatierte Benachrichtigungen](raw-notification-overview.md)
* [Herstellen einer Verbindung mit Microsoft Azure-Mobile Dienste (JavaScript)](/previous-versions/windows/apps/dn263160(v=win.10))
* [Herstellen einer Verbindung mit Microsoft Azure-Mobile Dienste (C#/C++/VB)](/previous-versions/windows/apps/dn263175(v=win.10))
* [Schnellstart: Hinzufügen von Pushbenachrichtigungen für einen mobilen Dienst (JavaScript)](/previous-versions/windows/apps/dn263163(v=win.10))
 

 
