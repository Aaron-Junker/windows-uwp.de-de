---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Geräteportal für Mobilgeräte
description: Hier erfahren Sie, wie Sie mit dem Windows Device Portal Ihr mobiles Gerät per Fernzugriff konfigurieren und verwalten können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: 29a6f85dfb13e47617138e401c8444cb5b5f6809
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339578"
---
# <a name="device-portal-for-mobile"></a>Geräteportal für Mobilgeräte

Ab Windows 10, Version 1511, sind weitere Entwicklerfeatures für Mobilgeräte verfügbar. Diese Features sind nur verfügbar, wenn der Entwicklermodus auf dem Gerät aktiviert ist.

Informationen zum Aktivieren des Entwicklermodus finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](/windows/apps/get-started/enable-your-device-for-development).

![Screenshot der Einstellungen für die Geräteermittlung und das Geräteportal.](images/device-portal/mob-dev-mode-options.png)

## <a name="set-up-device-portal-on-windows-phone"></a>Einrichten eines Device Portal auf Windows Phone

### <a name="turn-on-device-discovery-and-pairing"></a>Aktivieren der Geräteermittlung und -kopplung

Damit du eine Verbindung mit dem Geräteportal herstellen kannst, musst du in den Einstellungen deines Telefons die Geräteermittlung und das Geräteportal aktivieren. Auf diese Weise können Sie Ihr Telefon mit einem PC oder einem anderen Windows 10-Gerät koppeln. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden.

Wenn Sie das erste Mal eine Verbindung mit dem Geräteportal herstellen, werden Sie aufgefordert, einen sechsstelligen Sicherheitscode einzugeben (mit Beachtung der Groß- und Kleinschreibung). Dadurch wird sichergestellt, dass Sie Zugriff auf das Smartphone haben, und Sie werden vor Angriffen geschützt. Tippen Sie auf Ihrem Smartphone auf die Schaltfläche „Koppeln“, um den Code zu generieren und anzuzeigen. Geben Sie dann die sechs Zeichen in das Textfeld im Browser ein.

![Einstellungen für die Geräteerkennung im Entwicklermodus](images/device-portal/mob-dev-mode-pairing.png)

Sie haben 3 Möglichkeiten zum Herstellen einer Verbindung mit dem Geräteportal: USB, über einen lokalen Host und über das lokale Netzwerk (einschließlich VPN und Tethering).

**So stellst du eine Verbindung mit dem Geräteportal her**

1. Geben Sie in Ihrem Browser die hier angegebene Adresse für den verwendeten Verbindungstyp ein.

    - USB: `http://127.0.0.1:10080`

    Verwenden Sie diese Adresse, wenn das Smartphone über USB mit einem PC verbunden ist. Auf beiden Geräten muss Windows 10, Version 1511 oder höher, ausgeführt werden.
    
    - Lokaler Host: `http://127.0.0.1`

    Verwenden Sie diese Adresse, um das Geräteportal lokal auf dem Smartphone in Microsoft Edge für Windows 10 Mobile anzuzeigen.
    
    - Lokales Netzwerk: `https://<The IP address or hostname of the phone>`

    Verwenden Sie diese Adresse, um die Verbindung über ein lokales Netzwerk herzustellen.

    Die IP-Adresse des Smartphones wird in den Geräteportaleinstellungen auf dem Telefon angezeigt. Für die Authentifizierung und sichere Kommunikation ist HTTPS erforderlich. Der Hostname (kann in „Einstellungen“ > „System“ > „Info“ bearbeitet werden) kann auch für den Zugriff auf das Geräteportal im lokalen Netzwerk verwendet werden (Beispiel: http://Phone360) ). Dies ist hilfreich, wenn Netzwerke oder IP-Adressen von Geräten häufig geändert werden oder freigegeben werden müssen. 

2. Tippen Sie auf Ihrem Smartphone auf die Schaltfläche „Koppeln“, um den Code zu generieren und anzuzeigen.

3. Geben Sie den sechsstelligen Sicherheitscode im Browser in das Kennwortfeld des Geräteportals ein.

4. (Optional) Aktivieren Sie das Kontrollkästchen Computer merken in Ihrem Browser, um diese Kopplung für die spätere Verwendung zu speichern.

Dies ist der Geräteportalabschnitt auf der Seite mit den Entwicklerseiten in Windows Phone.

![Screenshot der Einstellungenseite für das Geräteportal auf einem Windows Phone.](images/device-portal/mob-dev-mode-portal.png)

Sie können die Authentifizierung deaktivieren, wenn Sie Device Portal in einer geschützten Umgebung verwenden, z. B. in einem Testlabor, in dem Sie allen im lokalen Netzwerk vertrauen, keine persönlichen Informationen auf dem Gerät gespeichert haben und in dem spezielle Anforderungen bestehen. Dies ermöglicht eine unverschlüsselte Kommunikation, und jeder, der die IP-Adresse Ihres Smartphones kennt, kann es steuern.

## <a name="tool-notes"></a>Anmerkungen zu Tools

## <a name="device-portal-pages"></a>Seiten von Device Portal
### <a name="processes"></a>Prozesse

Im Windows Mobile Device Portal ist es nicht möglich, beliebige Prozesse zu beenden. 

Das Geräteportal für mobile Geräte enthält den Standardsatz der Seiten. Ausführliche Beschreibungen finden Sie unter [Übersicht über das Windows Device Portal](device-portal.md).

- App-Manager
- App-Datei-Explorer (Isolierter Speicher-Explorer)
- Prozesse
- Leistungsdiagramme
- Ereignisablaufverfolgung für Windows (ETW)
- Leistungsüberwachung (WPR) 
- -Geräte zu unterstützen
- Netzwerk

## <a name="see-also"></a>Siehe auch

* [Übersicht über das Windows-Geräteportal](device-portal.md)
* [Referenz zur Kern-API des Geräteportals](./device-portal-api-core.md)