---
Description: Viele Unternehmen verwenden Firewalls zum Blockieren von unerwünschtem Datenverkehr. In diesem Dokument wird beschrieben, wie Sie den WNS-Datenverkehr durch Firewalls passieren können.
title: Hinzufügen von WNS-Datenverkehr zur Firewall-Zulassungsliste
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, Windows-Benachrichtigungsdienst, Benachrichtigung, Windows, Firewall, Problembehandlung, IP, Datenverkehr, Unternehmen, Netzwerk, IPv4, VIP, FQDN, öffentliche IP-Adresse
ms.localizationpriority: medium
ms.openlocfilehash: a2eb09a0b1cc6f135a23b038207bb442eb741bf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169204"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>Unternehmens Firewall-und Proxy Konfigurationen zur Unterstützung von WNS-Datenverkehr

## <a name="background"></a>Hintergrund
Viele Unternehmen verwenden Firewalls zum Blockieren von unerwünschtem Netzwerk Datenverkehr und Ports. Leider können dadurch auch wichtige Dinge wie die Kommunikation mit dem Windows-Benachrichtigungsdienst blockiert werden. Dies bedeutet, dass alle über WNS gesendeten Benachrichtigungen in bestimmten Netzwerkkonfigurationen abgelegt werden. Um dies zu vermeiden, können Netzwerkadministratoren die Liste der genehmigten WNS-FQDNs oder VIPs zur Ausnahmeliste hinzufügen, damit der WNS-Datenverkehr durch die Firewall geleitet wird. Im folgenden finden Sie weitere Details dazu, wie und was hinzugefügt werden muss, sowie Unterstützung für verschiedene Proxy Typen.

## <a name="proxy-support"></a>Proxyunterstützung

> [!Note]
> Windows-Clients unterstützen **nicht** alle Proxys, die Verbindung mit WNS muss eine direkte Verbindung sein.

**Demnächst!** Wir untersuchen aktiv verschiedene Netzwerkkonfigurationen, Proxys und Firewalls. Wir werden diese Seite mit weiteren Details zu allgemeinen Unternehmens Szenarien und WNS-Unterstützung aktualisieren.


## <a name="what-information-should-be-added-to-the-allowlist"></a>Welche Informationen sollten der Zulassungs hinzugefügt werden?
Im folgenden finden Sie eine Liste, die die FQDNs, VIPs und IP-Adressbereiche enthält, die vom Windows-Benachrichtigungsdienst verwendet werden. 

> [!IMPORTANT]
> Es wird dringend empfohlen, die Liste nach voll qualifizierten Namen zuzulassen, da sich diese nicht ändern. Wenn Sie die Liste nach FQDN zulassen, müssen Sie auch die IP-Adressbereiche nicht zulassen.

> [!IMPORTANT]
> Die IP-Adressbereiche werden regelmäßig geändert. aus diesem Grund sind Sie auf dieser Seite nicht enthalten. Wenn Sie die Liste der IP-Adressbereiche anzeigen möchten, können Sie die Datei aus dem Download Center herunterladen: [Windows Notification Service (WNS) VIP und IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=44238). Überprüfen Sie regelmäßig, ob Sie über die aktuellsten Informationen verfügen. 


### <a name="fqdns-vips-ips-and-ports"></a>Voll qualifizierte DNS, VIPs, IPS und Ports
Unabhängig von der Methode, die Sie unten auswählen, müssen Sie Netzwerk Datenverkehr an die aufgelisteten Ziele über **Port 443**zulassen. Jedes der Elemente im folgenden XML-Dokument wird in der folgenden Tabelle erläutert (in [Begriffen und Notizen](#terms-and-notations)). Die IP-Adressbereiche wurden absichtlich nicht in diesem Dokument ausgelassen, damit Sie nur die FQDNs verwenden können, da die FQDNs konstant bleiben. Allerdings können Sie die XML-Datei mit der kompletten Liste aus dem Download Center herunterladen: [Windows Notification Service (WNS) VIP und IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=44238). Neue VIPs oder IP-Adressbereiche werden **eine Woche nach dem Hochladen wirksam**.

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
    <IdentityServiceDNS>
        <DNS FQDN="login.microsoftonline.com"/>
        <DNS FQDN="login.live.com"/>
    </IdentityServiceDNS>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Nutzungsbedingungen
Im folgenden finden Sie Erläuterungen zu den im obigen XML-Code Ausschnitt verwendeten Notations-und Element Elementen.

| Begriff | Erklärung |
|---|---|
| **Punkt-Dezimal Schreibweise (d. h. 64.4.28.0/26)** | Mit der Punkt-Dezimal Schreibweise kann der Bereich der IP-Adressen beschrieben werden. 64.4.28.0/26 bedeutet beispielsweise, dass die ersten 26 Bits von 64.4.28.0 konstant sind, während die letzten 6 Bits variabel sind.  In diesem Fall ist der IPv4-Bereich 64.4.28.0-64.4.28.63. |
| **Clientdns** | Dies sind die voll qualifizierten Domänen Namen (FQDN)-Filter für die Client Geräte (Windows-PCs, Desktops), die Benachrichtigungen von WNS empfangen. Diese müssen über die Firewall zugelassen werden, damit WNS-Clients die WNS-Funktionalität verwenden können.  Es wird empfohlen,-List durch die FQDNs anstelle der IP-/VIP-Bereiche zuzulassen, da diese nie geändert werden. |
| **ClientIPsIPv4** | Dies sind die IPv4-Adressen der Server, auf die von Client Geräten (Windows-PCs, Desktops) zugegriffen wird, die Benachrichtigungen von WNS empfangen. |
| **Cloudservicedns** | Dies sind die voll qualifizierten Domänen Namen (Fully Qualified Domain Name, FQDN) der WNS-Server, mit denen Ihr Cloud-Dienst benachrichtigt wird, um notificatios an WNS zu senden. Diese müssen über die Firewall zugelassen werden, damit Dienste WNS-Benachrichtigungen senden können.  Es wird empfohlen,-List durch die FQDNs anstelle der IP-/VIP-Bereiche zuzulassen, da diese nie geändert werden.|
| **Cloudserviceips** | Cloudserviceips sind die IPv4-Adressen der Server, die für Clouddienste zum Senden von Benachrichtigungen an WNS verwendet werden.  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Öffentliche IP-Adressbereiche von Microsoft Push Notification Service (mpns)
Wenn Sie den Legacy-Benachrichtigungsdienst (mpns) verwenden, sind die IP-Adressbereiche, die Sie der Zulassungsliste hinzufügen müssen, im Download Center verfügbar: [öffentliche IP-Adressbereiche des Microsoft-pushbenachrichtigungsdiensts (mpns)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Zugehörige Themen

* [Schnellstart: Senden einer Pushbenachrichtigung](/previous-versions/windows/apps/hh868252(v=win.10))
* [So wird's gemacht: Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](/previous-versions/windows/apps/hh465412(v=win.10))
* [So wird's gemacht: Abfangen von Benachrichtigungen für ausgeführte Anwendungen](/previous-versions/windows/apps/jj709907(v=win.10))
* [So wird's gemacht: Authentifizieren mit dem Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](/previous-versions/windows/apps/hh465407(v=win.10))
* [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](/previous-versions/windows/apps/hh465435(v=win.10))
* [Richtlinien und Prüfliste für Pushbenachrichtigungen](./windows-push-notification-services--wns--overview.md)
 