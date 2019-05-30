---
author: mijacobs
Description: Viele Unternehmen verwenden von Firewalls, um unerwünschten Datenverkehr zu blockieren. Dieses Dokument beschreibt, wie WNS-Datenverkehr durch Firewalls zu ermöglichen.
title: Hinzufügen von WNS-Datenverkehr an die Firewall Allowlist
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, WNS, Windows-Benachrichtigungsdienst, Notification, Windows-Firewall, Problembehandlung, IP, Datenverkehr, Enterprise, Netzwerk, IPv4, VIP-Adresse, vollqualifizierten Domänennamen, öffentliche IP-Adresse
ms.localizationpriority: medium
ms.openlocfilehash: 9ed4ad6ed828abda9d487ef96beca9b655c92421
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366671"
---
# <a name="allowing-windows-notification-traffic-through-enterprise-firewalls"></a>Windows-Notification-Datenverkehr durch Unternehmensfirewalls von

## <a name="background"></a>Hintergrund
Viele Unternehmen verwenden von Firewalls Blockieren von unerwünschtem Netzwerkdatenverkehr; Leider kann dadurch auch wichtige Dinge wie Windows Notification Service-Kommunikation blockiert. Dies bedeutet, dass alle Benachrichtigungen über WNS gesendet werden verworfen. Um dies zu vermeiden, können Netzwerkadministratoren die Ausnahmeliste der WNS-Datenverkehr an die Firewall passieren können die Liste der genehmigten WNS-Kanäle hinzufügen. Im folgenden finden Sie weitere Informationen zur Verwendung und die hinzuzufügenden Elemente. 


## <a name="what-information-should-be-added-to-the-allowlist"></a>Welche Informationen das Allowlist hinzugefügt werden soll
Im folgenden wird eine Liste mit den FQDNs, virtuellen IP-Adressen und IP-Adressbereiche, die von den Windows Notification Service verwendet. 

> [!IMPORTANT]
> Wir empfehlen stark, dass Sie die Liste über den FQDN zulassen, da diese nicht ändern. Wenn Sie die Liste über den FQDN zulassen, müssen Sie nicht auch die IP-Adressbereiche zulassen.

> [!IMPORTANT]
> Die IP-Adressbereiche werden in regelmäßigen Abständen ändern; aus diesem Grund sind sie nicht auf dieser Seite enthalten. Wenn die Liste der IP-Adressbereiche angezeigt werden sollen, können Sie die Datei aus dem Download Center herunterladen: [Windows-Benachrichtigungsdienst (WNS), VIP-Adresse und IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=44238). Überprüfen Sie regelmäßig auf Stellen Sie sicher, dass die aktuellste Informationen. 


### <a name="fqdns-vips-and-ips"></a>Virtuellen IP-Adressen, FQDNs und IP-Adressen
Jedes der Elemente in der folgenden XML-Dokument wird erläutert, in der Tabelle, die darauf folgt (in [Begriffe und Notationen](#terms-and-notations). Die IP-Bereiche wurden absichtlich beibehalten, aus diesem Dokument wird empfohlen, nur die FQDNs zu verwenden, wie Sie die FQDNs konstant bleibt. Allerdings können Sie die XML-Datei mit der vollständigen Liste vom Download Center herunterladen: [Windows-Benachrichtigungsdienst (WNS), VIP-Adresse und IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=44238). Neuen virtuellen IP-Adressen oder IP-Adressbereiche werden werden **effektiv eine Woche, nachdem diese hochgeladen werden**.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Begriffe und Notationen
Im folgenden finden Sie Erklärungen der Notationen und Elemente, die im obigen XML-Codeausschnitt verwendet.

| Begriff | Erläuterung |
|---|---|
| **Punkt-Dezimal-Schreibweise (d. h. 64.4.28.0/26)** | Punkt-Dezimal-Schreibweise ist eine Möglichkeit, die Bereich von IP-Adressen zu beschreiben. Beispielsweise bedeutet 64.4.28.0/26 an, dass die erste 26 Bits 64.4.28.0 konstant sind, während die letzten 6 Bits Variable sind.  In diesem Fall wird die IPv4-Adressbereich 64.4.28.0 - 64.4.28.63. |
| **ClientDNS** | Hierbei handelt es sich um die Filter vollqualifizierter Domänenname (FQDN) für die Clientgeräte (Windows-PCs, Desktops) empfangen von Benachrichtigungen von WNS. Diese müssen in der Reihenfolge für WNS-Clients mit den WNS-Funktionen über die Firewall zugelassen werden.  Es wird empfohlen, Zulassen von FQDNs anstelle der IP-Adresse/VIP-Bereiche, da diese nie geändert. |
| **ClientIPsIPv4** | Hierbei handelt es sich um die IPv4-Adressen der Server von Clientgeräten (Windows-PCs, Desktops) Zugriff auf das Empfangen von Benachrichtigungen von WNS. |
| **CloudServiceDNS** | Dies sind die Filter vollqualifizierter Domänenname (FQDN) für die WNS-Server, die, denen Ihren Cloud-Dienst zum Senden von Benachrichtigungen zu für WNS, kommuniziert. Diese müssen in der Reihenfolge für Dienste zum Senden von WNS-Benachrichtigungen über die Firewall zugelassen werden.  Es wird empfohlen, Zulassen von FQDNs anstelle der IP-Adresse/VIP-Bereiche, da diese nie geändert.|
| **CloudServiceIPs** | CloudServiceIPs werden die IPv4-Adressen der Server verwendet für Cloud Services zum Senden von Benachrichtigungen für WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Öffentliche IP-Adressbereiche Microsoft Push Notifications Service (MPNS)
Bei Verwendung von den älteren Notification-Service, MPNS, sind die IP-Adressbereiche, die Sie an der Zulassungsliste hinzufügen müssen aus dem Download Center verfügbar: [Microsoft Push Notifications Service (MPNS) öffentliche IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Verwandte Themen

* [Schnellstart: Eine Pushbenachrichtigung senden](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Wie Sie anfordern, erstellen und speichern einen Benachrichtigungskanal](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Gewusst wie: Abfangen von Benachrichtigungen für die Ausführung von Anwendungen](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Gewusst wie: authentifizieren mit den Windows Push Notification Service (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [Push Notification Service Anforderungs- und Antwortheader](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Richtlinien und Prüfliste für erste Schritte mit Pushbenachrichtigungen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
