---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Entwickler zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App geeignet sind.
title: Welche Netzwerktechnologie?
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c4b1a0dab6bf1eb3301ba9fb97abd95fd896c53e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259166"
---
# <a name="which-networking-technology"></a>Welche Netzwerktechnologie?


Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Entwickler zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App geeignet sind.

## <a name="sockets"></a>Sockets

Verwenden Sie [Sockets](sockets.md), wenn Sie mit einem anderen Gerät kommunizieren und Ihr eigenes Protokoll verwenden möchten.

Zwei Implementierungen von Sockets sind für UWP-Entwickler (Universelle Windows-Plattform) verfügbar: [**Windows.Networking.Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) und [Winsock](https://docs.microsoft.com/windows/desktop/WinSock/windows-sockets-start-page-2). Wenn Sie neuen Code schreiben, hat Windows.Networking.Sockets den Vorteil, dass es sich um eine moderne API handelt, die für die Verwendung durch UWP-Entwickler vorgesehen ist. Wenn Sie plattformübergreifende Netzwerkbibliotheken oder anderen vorhandenen Winsock-Code verwenden oder die Winsock-API bevorzugen, verwenden Sie Winsock.

### <a name="when-to-use-sockets"></a>Wann sollten Sockets verwenden werden?

-   Beide Socketimplementierungen ermöglichen Ihnen die Kommunikation mit anderen Geräten mithilfe von Protokollen Ihrer Wahl über TCP oder UDP.

-   Wählen Sie die Sockets-API aus, die am besten Ihre Anforderungen erfüllt; orientieren Sie sich dabei an Ihren Erfahrungen und vorhandenem Code, den Sie möglicherweise verwenden.

### <a name="when-not-to-use-sockets"></a>Wann sollten Sockets nicht verwendet werden?

-   Implementieren Sie keine eigenen HTTP(S)-Stapel mit Sockets. Verwenden Sie stattdessen [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient).
-   Wenn WebSockets (die [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket)-Klasse und [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket)-Klasse) Ihren Kommunikationsanforderungen entsprechen (TCP zu/von einem Webserver), sollten Sie diese verwenden, anstatt Ihre Zeit und Entwicklungsressourcen für die Implementierung ähnlicher Funktionen mit Sockets aufzuwenden.

## <a name="websockets"></a>WebSockets

Das [WebSockets](websockets.md)-Protokoll definiert einen Mechanismus für die schnelle und sichere bidirektionale Kommunikation zwischen einem Client und einem Server über das Web. Daten werden sofort über eine Vollduplex-Ein-Socket-Verbindung übertragen, sodass Nachrichten von beiden Endpunkten in Echtzeit gesendet und empfangen werden können. WebSockets eignen sich ideal für den Einsatz in Echtzeitspielen, bei denen Sofortbenachrichtigungen aus sozialen Netzwerken und die aktuelle Anzeige von Daten (z. B. Spielstatistiken) sicher sein und eine schnelle Datenübertragung verwenden müssen. UWP-Entwickler können die [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket)-Klasse und [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket)-Klasse zum Herstellen einer Verbindung mit Servern verwenden, die das WebSocket-Protokoll unterstützen.

### <a name="when-to-use-websockets"></a>Wann sollten WebSockets verwendet werden?

-   Wenn Sie Daten fortlaufend zwischen einem Gerät und einem Server senden und empfangen möchten.

### <a name="when-not-to-use-websockets"></a>Wann sollten WebSockets nicht verwendet werden?

-   Wenn Sie Daten selten senden oder empfangen, ist es für Sie unter Umständen einfacher, einzelne HTTP-Anforderungen vom Gerät an den Server zu senden, anstatt eine WebSocket-Verbindung herzustellen und aufrechtzuerhalten.
-   WebSockets sind möglicherweise nicht für Situationen mit großem Datenvolumen geeignet. Sie sollten Ihre Datenflüsse modellieren und Ihren Datenverkehr über WebSockets simulieren, bevor Sie sie in Ihrem Entwurf verwenden.

## <a name="httpclient"></a>HttpClient

Verwenden Sie [HttpClient](httpclient.md) (und den Rest der [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)-Namespace-API), wenn Sie HTTP(S) für die Kommunikation mit einem Webdienst oder Webserver verwenden.

### <a name="when-to-use-httpclient"></a>Wann sollte HttpClient verwendet werden?

-   Wenn Sie HTTP(S) zur Kommunikation mit Webdiensten verwenden.
-   Beim Hochladen oder Herunterladen einer geringen Anzahl von kleineren Dateien.
-   Wenn WebSockets (die [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket)-Klasse und [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket))-Klasse Ihren Kommunikationsanforderungen (TCP zu/von einem Webserver) entsprechen und der fragliche Webserver WebSockets unterstützt, sollten Sie diese verwenden, anstatt Ihre Zeit und Entwicklungsressourcen für die Implementierung ähnlicher Funktionen mit HttpClient aufzuwenden.
-   Wenn Sie Inhalte über das Netzwerk streamen.

### <a name="when-not-to-use-httpclient"></a>Wann sollte HttpClient nicht verwendet werden?

-   Wenn Sie große Dateien oder eine große Anzahl von Dateien übertragen, sollten Sie stattdessen Hintergrundübertragungen verwenden.
-   Wenn Sie Upload-/Downloadlimits nach Verbindungstyp einschränken möchten oder den Upload-/Downloadstatus speichern und den Upload/Download nach einer Unterbrechung wiederaufnehmen möchten, müssen Sie Hintergrundübertragungen verwenden.
-   Wenn Sie zwischen zwei Geräten kommunizieren und keines als HTTP(S)-Server vorgesehen ist, sollten Sie Sockets verwenden. Versuchen Sie nicht, einen eigenen HTTP-Server zu implementieren und [HttpClient](httpclient.md) für die Kommunikation zu verwenden.

## <a name="background-transfers"></a>Hintergrundübertragungen

Verwenden Sie die [Hintergrundübertragungs-API](background-transfers.md), wenn Sie zuverlässig Dateien über das Netzwerk übertragen möchten. Die Hintergrundübertragungs-API bietet erweiterte Upload- und Downloadfeatures, die bei angehaltener App im Hintergrund ausgeführt werden und auch nach Beendigung der App aktiv bleiben. Die API überwacht den Netzwerkstatus und kann Übertragungen automatisch anhalten und fortsetzen, wenn die Verbindung unterbrochen wird. Übertragungen sind außerdem daten- und akkuabhängig – die Downloadaktivität wird also basierend auf dem aktuellen Verbindungs- und Geräteakkustatus angepasst. Diese Funktionen sind wichtig, wenn Ihre App auf mobilen oder akkubetriebenen Geräten ausgeführt wird. Die API ist ideal für das Hoch- und Herunterladen von großen Dateien über HTTP(S) geeignet. FTP wird auch unterstützt, allerdings nur für Downloads.

Ein neues Feature für die Hintergrundübertragung in Windows 10 ist die Möglichkeit, eine Nachverarbeitung auszulösen, wenn eine Dateiübertragung abgeschlossen wurde, sodass Sie lokale Kataloge aktualisieren, andere Apps aktivieren oder den Benutzer benachrichtigen können, wenn ein Download abgeschlossen ist.

### <a name="when-to-use-background-transfers"></a>Wann sollten Hintergrundübertragungen verwendet werden?

-   Verwenden Sie Hintergrundübertragungen zum zuverlässigen Übertragen großer Dateien oder einer großen Anzahl von Dateien.
-   Verwenden Sie Hintergrundübertragungen mit Abschlussgruppen für Hintergrundübertragungen, wenn Sie Dateiübertragungen im Nachhinein mit einer Hintergrundaufgabe verarbeiten möchten.
-   Verwenden Sie Hintergrundübertragungen, wenn Sie eine laufende Übertragung nach einer Netzwerkunterbrechung wiederaufnehmen möchten.
-   Verwenden Sie Hintergrundübertragungen, wenn Sie das Übertragungsverhalten auf Grundlage von Netzwerkbedingungen ändern möchten, z. B. beim Wechsel zu einem Volumentarif.

### <a name="when-not-to-use-background-transfers"></a>Wann sollten Hintergrundübertragungen nicht verwendet werden?

-   Wenn Sie eine geringe Anzahl von kleinen Dateien übertragen und Sie keine Nachbearbeitung nach Abschluss der Übertragung durchführen müssen, sollten Sie die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Methode PUT oder POST verwenden.
-   Wenn Sie Daten streamen und nach der Übertragung lokal verwenden möchten, verwenden Sie [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient).

## <a name="additional-network-related-technologies"></a>Zusätzliche netzwerkbezogene Technologien

### <a name="connection-quality"></a>Verbindungsqualität

Mit der [**Windows.Networking.Connectivity**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity)-API können Sie auf Informationen zu Netzwerkkonnektivität, Kosten und Nutzung zugreifen. Weitere Informationen zur Verwendung dieser API finden Sie unter [Zugreifen auf den Netzwerkverbindungsstatus und Verwalten von Netzwerkkosten](https://docs.microsoft.com/previous-versions/windows/apps/hh452983(v=win.10)).

### <a name="dns-service-discovery"></a>DNS Service Discovery (DNS-SD)

Mit der [**Windows.Networking.ServiceDiscovery.Dnssd**](https://docs.microsoft.com/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd)-API können Sie einen Netzwerkdienst für andere Geräte im Netzwerk mithilfe des DNS-SD-Protokolls ankündigen, das in IETF [RFC 2782](https://www.rfc-archive.org/getrfc.php?rfc=2782) beschrieben wurde.

### <a name="communicating-over-bluetooth"></a>Kommunikation über Bluetooth

Unter anderem ermöglicht Ihnen die [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)-API die Verwendung von Bluetooth, um eine Verbindung mit anderen Geräten herzustellen und Daten zu übertragen. Weitere Informationen finden Sie unter [Senden und Empfangen von Dateien mit RFCOMM](https://docs.microsoft.com/windows/uwp/devices-sensors/send-or-receive-files-with-rfcomm).

### <a name="push-notifications-wns"></a>Pushbenachrichtigungen (WNS)

Mit der [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications)-API können Sie den Windows-Benachrichtigungsdienst (Windows Notification Service, WNS) verwenden, um Pushbenachrichtigungen über das Netzwerk zu empfangen. Weitere Informationen zum Verwenden dieser API finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview).

### <a name="near-field-communications"></a>Nahfeldkommunikation (Near Field Communication, NFC)

Mit der [**Windows.Networking.Proximity**](https://docs.microsoft.com/uwp/api/Windows.Networking.Proximity)-API können Sie Nahfeldkommunikation für Apps verwenden, die die Näherung oder das Koppeln mit Geräten nutzen, um eine einfache Datenübertragung zu ermöglichen. Weitere Informationen zum Verwenden dieser API finden Sie unter [Unterstützen von Näherung und Koppeln](https://docs.microsoft.com/previous-versions/windows/apps/hh465229(v=win.10)).

### <a name="rssatom-feeds"></a>RSS-/Atom-Feeds

Mit der [**Windows.Web.Syndication**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication)-API können Sie Syndication-Feeds mit RSS- und Atom-Formaten verwalten. Weitere Informationen zur Verwendung dieser API finden Sie unter [RSS/Atom-Feeds](web-feeds.md).

### <a name="wi-fi-enumeration-and-connection-control"></a>WLAN-Auflistung und Verbindungssteuerung

Mit der [**Windows.Devices.WiFi**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFi)-API können Sie WLAN-Adapter auflisten, nach verfügbaren WLAN-Netzwerken suchen und einen Adapter mit einem Netzwerk verbinden.

### <a name="radio-control"></a>Funksteuerung

Mit der [**Windows.Devices.Radios**](https://docs.microsoft.com/uwp/api/Windows.Devices.Radios)-API können Sie Funkempfänger auf dem lokalen Gerät suchen und steuern, einschließlich WLAN und Bluetooth.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

Mit der [**Windows.Devices.WiFiDirect**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFiDirect)-API können Sie eine Verbindung mit anderen lokalen Geräten über Wi-Fi Direct herstellen und mit ihnen kommunizieren, um lokale Ad-hoc-Drahtlosnetzwerke zu erstellen.

### <a name="wi-fi-direct-services"></a>Wi-Fi Direct-Dienste

Mit der [**Windows.Devices.WiFiDirect.Services**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFiDirect.Services)-API können Sie Wi-Fi Direct-Dienste bereitstellen und entsprechende Verbindungen herstellen. Wi-Fi Direct-Dienste ermöglichen einem Gerät in einem Wi-Fi Direct-Ad-hoc-Netzwerk (Dienstankündigung), einem anderen Gerät (Dienstsuche) über eine Wi-Fi Direct-Verbindung Funktionen bereitzustellen.

### <a name="mobile-operators"></a>Mobilfunkanbieter

Windows 10 stellt einer großen Entwicklergruppe einige APIs bereit, die bisher nur Geräteherstellern und Mobilfunkanbietern zur Verfügung standen. Beachten Sie, dass obwohl diese APIs jetzt verfügbar gemacht werden, sie auch bestimmten App-Funktionen unterliegen, die von Microsoft genehmigt werden müssen, bevor eine App veröffentlicht werden kann. Die tatsächliche Verwendung dieser APIs wird nach wie vor in erster Linie auf Gerätehersteller und Mobilfunkanbieter beschränkt.

### <a name="network-operations"></a>Netzwerkvorgänge

Die [**Windows.Networking.NetworkOperators**](https://docs.microsoft.com/uwp/api/Windows.Networking.NetworkOperators)-API befasst sich hauptsächlich mit der Konfiguration und Bereitstellung von Telefonen. Darum ist die Berechtigung zum Verwenden der steuernden Funktionen auf Gerätehersteller und Telekommunikationsanbieter beschränkt.

### <a name="sms"></a>SMS

Der [**Windows.Devices.Sms**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sms)-Namespace verarbeitet SMS- und verwandte Nachrichten als untergeordnete Elemente. Er wird für die Verwendung durch Mobilfunkanbieter für die App-gerichtete SMS-Verwendung bereitgestellt und wird durch eine Funktion gesteuert, die nicht zur Verwendung durch die meisten App-Entwickler genehmigt wird. Wenn Sie eine App für die Verarbeitung von Nachrichten schreiben, verwenden Sie stattdessen die [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat)-API, da sie nicht nur SMS-Nachrichten verarbeiten kann, sondern auch Nachrichten aus anderen Quellen, z. B. Echtzeit-Chat-Apps, sodass eine viel umfangreichere Chat-/Nachrichtenerfahrung möglich wird.

