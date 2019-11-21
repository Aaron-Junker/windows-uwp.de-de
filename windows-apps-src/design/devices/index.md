---
Description: Wenn Sie sich mit den Geräten vertraut machen, die UWP-Apps (Universelle Windows-Plattform) unterstützen, können Sie für jeden Formfaktor die bestmögliche Benutzerfreundlichkeit bieten.
title: Überlegungen zur Benutzeroberfläche für UWP-Geräte
ms.assetid: 7665044E-F007-495D-8D56-CE7C2361CDC4
label: Device primer
template: detail.hbs
keywords: Gerät, Eingabe, Interaktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0808b5173f697fcca8d5e117f857d3861c327ae3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258113"
---
#  <a name="ui-considerations-for-uwp-devices"></a>Überlegungen zur Benutzeroberfläche für UWP-Geräte

![Geräte](../images/devices-2x.png)

Wenn Sie sich mit den Geräten vertraut machen, die UWP-Apps (Universelle Windows-Plattform) unterstützen, können Sie für jeden Formfaktor die bestmögliche Benutzerfreundlichkeit bieten.

![Windows-Geräte](images/device-primer/device-primer-ramp.png)

 Beim Entwickeln für ein bestimmtes Gerät sind die wichtigsten zu berücksichtigenden Punkte die Darstellung der App auf dem Gerät, wo, wann und wie die App auf dem Gerät genutzt wird und die Art der Interaktion der Benutzer mit dem Gerät.

## <a name="pcs-and-laptops"></a>PCs und Laptops


Mit PCs und Laptops von Windows wird eine breite Palette von Geräten und Bildschirmgrößen abgedeckt. Im Allgemeinen können auf PCs und Laptops mehr Informationen als auf Smartphones oder Tablets angezeigt werden.

Bildschirmgrößen
-   13 Zoll und größer

![PC](images/device-primer/device-primer-desktop.png)

Typische Verwendung
-   Apps werden auf Desktops und Laptops häufig gleichzeitig genutzt, aber nur von jeweils einem Benutzer und meist über längere Zeiträume hinweg.

Hinweise zur Benutzeroberfläche
-   Apps können in einer Fensteransicht angezeigt werden, deren Größe vom Benutzer bestimmt wird. Je nach Größe des Fensters können darin zwischen einem und drei Frames enthalten sein. Auf größeren Bildschirmen kann die App mehr als drei Frames haben.

-   Beim Nutzen einer App auf einem Desktop oder Laptop haben Benutzer die Kontrolle über App-Dateien. Achten Sie als App-Designer darauf, Verfahren einzubauen, die dem Verwalten der App-Inhalte dienen. Erwägen Sie, Befehle und Features der Art „Speichern unter“, „Zuletzt verwendet“ usw. bereitzustellen.

-   Die Zurück-Schaltfläche des Systems ist optional. Wenn ein App-Entwickler diese anzeigen möchte, wird sie in der App-Titelleiste angezeigt.

Eingaben
-   Maus
-   Tastatur
-   Toucheingabe auf Laptops und All-in-One-Desktops.
-   Es werden auch Gamepads verwendet, z. B. der Xbox-Controller.

Typische Gerätefunktionen
-   Kamera
-   Mikrofon

## <a name="tablets-and-2-in-1s"></a>Tablets und 2-in-1-Geräte


Extrem leichte Tablet PCs sind mit Touchscreens, Kameras, Mikrofonen und Beschleunigungsmessern ausgestattet. Die Größe der Bildschirme von Tablets reicht normalerweise von 7 bis 13,3 Zoll. 2-in-1-Geräte können abhängig von der Konfiguration als Tablet oder Laptop mit einer Tastatur und Maus verwendet werden (in der Regel wird hierzu der Bildschirm aufgestellt oder nach hinten geklappt).

Bildschirmgrößen
- 7 bis 13,3 Zoll bei Tablets
- 13,3 Zoll und größer bei 2-in-1-Geräten

![Tabletgerät](images/device-primer/device-primer-tablet.png)

Typische Verwendung
-   Tablets werden zu ca. 80 Prozent vom Besitzer und zu ca. 20 Prozent von anderen Personen genutzt.
-   Es wird meist zu Hause als Begleitgerät beim Fernsehen verwendet.
-   Es wird über längere Zeiträume hinweg als bei Smartphones und Phablets verwendet.
-   Text wird häufig und jeweils nur für kurze Zeit eingegeben.

Hinweise zur Benutzeroberfläche
-   Auf Tablets können sowohl im Querformat als auch im Hochformat jeweils zwei Frames angezeigt werden.
-   Die Zurück-Schaltfläche des Systems befindet sich in der Navigationsleiste.

Eingaben
-   Touch
-   Eingabestift
-   Externe Tastatur (gelegentlich)
-   Maus (gelegentlich)
-   Sprache (gelegentlich)

Typische Gerätefunktionen
-   Kamera
-   Mikrofon
-   Bewegungssensoren
-   Positionssensoren

> [!NOTE]
> Die meisten Überlegungen zu PCs und Laptops gelten auch für 2-in-1-Geräte.

## <a name="xbox-and-tv"></a>Xbox und Fernsehgeräte

Die Erfahrung, die Sie machen, wenn Sie auf dem Sofa sitzen und mittels eines Gamepads oder einer Fernbedienung mit Ihrem Fernsehgerät interagieren, wird als **3-Meter-Erfahrung** (10-Fuß-Erfahrung) bezeichnet. Der Name kommt daher, dass sich der Benutzer im Allgemeinen ungefähr 3 Meter (10 Fuß) vom Bildschirm entfernt befindet. Dies stellt eine besondere Herausforderung dar, die beispielsweise bei einer *50-cm-Erfahrung* (2-Fuß-Erfahrung) oder bei der Interaktion mit einem PC nicht vorhanden ist. Wenn Sie eine App für Xbox One oder ein anderes Gerät entwickeln, das an einen Fernsehbildschirm angeschlossen ist und unter Umständen ein Gamepad oder Fernbedienung für die Eingabe verwendet, sollten Sie dies stets bedenken.

Die Schritte beim Entwickeln einer UWP-App für die 3-Meter-Erfahrung unterscheiden sich stark von der Entwicklung für eine der hier aufgeführten Gerätekategorien. Weitere Informationen finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](designing-for-tv.md).

Bildschirmgrößen
- 24 Zoll und größer

![Xbox und Fernsehgeräte](images/device-primer/device-primer-tv-and-xbox.png)

Typische Verwendung
- Wird häufig von mehreren Personen, aber auch häufig nur von einer Person verwendet.
- Wird in der Regel über längere Zeiträume hinweg verwendet.
- Wird meist zu Hause, also an einem Ort, verwendet.
- Texteingabe ist selten erforderlich, da diese mit einem Gamepad oder einer Fernbedienung zeitaufwendiger ist.
- Die Ausrichtung des Bildschirms ist fest.
- In der Regel wird jeweils nur eine App ausgeführt. Unter Umständen ist das Andocken von Apps an der Seite möglich (etwa bei Xbox).

Hinweise zur Benutzeroberfläche
- Apps behalten in der Regel die gleiche Größe, es sei denn, eine andere App ist an der Seite angedockt.
- Die Zurück-Schaltfläche des Systems ist eine nützliche Funktion, die in den meisten Xbox-Apps zur Verfügung steht und auf die mit der B-Taste auf dem Gamepad zugegriffen wird.
- Da der Kunde etwa 3 Meter (10 Fuß) vom Bildschirm entfernt sitzt, stellen Sie sicher, dass die Benutzeroberfläche groß genug und klar sichtbar ist.

Eingaben
- Gamepad (z. B. Xbox-Controller)
- Fernbedienung
- Sprache (gelegentlich, falls der Kunde Kinect oder ein Headset besitzt)

Typische Gerätefunktionen
- Kamera (gelegentlich, falls der Kunde Kinect besitzt)
- Mikrofon (gelegentlich, falls der Kunde Kinect oder ein Headset besitzt)
- Bewegungssensoren (gelegentlich, falls der Kunde Kinect besitzt)

## <a name="phones-and-phablets"></a>Smartphones und Phablets


Smartphones sind mittlerweile die am häufigsten genutzten Geräte und bieten auch bei begrenzter Bildschirmfläche und eingeschränkten Eingabeverfahren viele Möglichkeiten. Smartphones sind in zahlreichen verschiedenen Größen verfügbar. Größere Handys werden als Phablets bezeichnet. App-Benutzeroberflächen auf Phablets ähneln den Benutzeroberflächen auf Smartphones, aber die größere Bildschirmfläche von Phablets ermöglicht einige wichtige Änderungen bei der Nutzung von Inhalten.

Mit Continuum für Smartphones, einem neuen Angebot für kompatible mobile Windows 10-Geräte, können Benutzer ihre Smartphones an einen Bildschirm anschließen und sogar mit Maus und Tastatur auf Ihren Smartphones wie auf einem Laptop arbeiten. (Weitere Informationen finden Sie im [Artikel zu Continuum für Smartphones](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN).)

Bildschirmgrößen
-   4 bis 5 Zoll bei Smartphones
-   5,5 bis 7 Zoll bei Phablets

![Windows Phone](images/device-primer/device-primer-phablet.png)

Typische Verwendung
-   Smartphones werden meist im Hochformat bedient, weil es am einfachsten ist, das Gerät in einer Hand zu halten und so alle Interaktionsmöglichkeiten zu nutzen. In manchen Fällen funktioniert jedoch das Querformat sehr gut, beispielsweise beim Anzeigen von Fotos und Videos, beim Lesen eines Buchs oder beim Verfassen von Text.
-   Ein Smartphone wird meist nur von einer Person, also dem Besitzer des Geräts, verwendet.
-   Es ist immer in Reichweite und wird in der Regel in der Hosentasche oder einer Tasche verstaut.
-   Es wird eher über kürzere Zeiträume verwendet.
-   Das Smartphone wird vom Benutzer häufig zum Multitasking verwendet.
-   Text wird häufig und jeweils nur für kurze Zeit eingegeben.

Hinweise zur Benutzeroberfläche
-   Auf dem Smartphone kann aufgrund der geringen Bildschirmgröße sowohl im Hochformat als auch im Querformat jeweils nur ein Frame angezeigt werden. Für alle hierarchischen Navigationsmuster auf einem Smartphone wird das Drilldown-Modell verwendet, bei dem Benutzer durch UI-Ebenen mit einzelnen Frames navigieren.

-   Bei Phablets ist es ähnlich wie bei Smartphones, denn im Hochformat kann auch hier nur jeweils ein Frame angezeigt werden. Da auf einem Phablet eine größere Bildschirmfläche verfügbar ist, können Benutzer ins Querformat wechseln und dieses Format beibehalten, damit gleichzeitig zwei App-Frames angezeigt werden können.

-   Achten Sie darauf, dass sowohl im Querformat als auch im Hochformat genügend Bildschirmfläche für die App-Leiste vorhanden ist, wenn die Bildschirmtastatur aktiviert ist.

Eingaben
-   Touch
-   Spracheingabe

Typische Gerätefunktionen
-   Mikrofon
-   Kamera
-   Bewegungssensoren
-   Positionssensoren

 

## <a name="surface-hub-devices"></a>Surface Hub-Geräte


Microsoft Surface Hub ist ein Gerät für die Zusammenarbeit mit großem Bildschirm, der für die gleichzeitige Verwendung durch mehrere Benutzer konzipiert ist.

Bildschirmgrößen
-   55 und 84 Zoll

![Surface Hub](images/device-primer/device-primer-surfacehub3.png)

Typische Verwendung
-   Apps auf dem Surface Hub dienen der gemeinsamen Nutzung für kurze Zeit, z. B. bei Besprechungen.

-   Surface Hub-Geräte befinden sich überwiegend an einem Ort und werden nur selten bewegt.

Hinweise zur Benutzeroberfläche
-   Apps auf dem Surface Hub können in vier Zuständen angezeigt werden: Vollbild (standardmäßige Vollbildansicht), Hintergrund (ausgeblendet, während die App weiterhin ausgeführt wird, verfügbar über die Aufgabenumschaltfunktion), Füllen (feststehende Ansicht, die den verfügbaren Aktionsbereich belegt) und Angedockt (Ansicht mit variablem Seitenverhältnis, die die rechte oder linke Seite des Aktionsbereichs belegt).
-   Im angedockten Modus oder Füllmodus zeigt das System die Skype-Randleiste an und verkleinert die App horizontal.
-   Die Zurück-Schaltfläche des Systems ist optional. Wenn ein App-Entwickler diese anzeigen möchte, wird sie in der App-Titelleiste angezeigt.

Eingaben
-   Touch
-   Stift
-   Spracheingabe
-   Tastatur (Bildschirm-/Remotetastatur)
-   Touchpad (Remote)

Typische Gerätefunktionen
-   Kamera
-   Mikrofon

 

## <a name="windows-iot-devices"></a>Windows IoT-Geräte


Bei Windows IoT-Geräten handelt es sich um eine neue Klasse von Geräten, bei denen das Einbetten von kleinen elektronischen Geräten, Sensoren und Verbindungen in physische Objekte im Mittelpunkt steht. Diese Geräte sind in der Regel über ein Netzwerk oder das Internet verbunden, um die erfassten realen Daten zu melden und in manchen Fällen auf diese Daten zu reagieren. Geräte können entweder keinen Bildschirm besitzen („monitorlose“ Geräte) oder an einen kleinen Bildschirm mit maximal 3,5 Zoll angeschlossen sein (Geräte mit Monitor).

Bildschirmgrößen
-   3,5 Zoll oder kleiner
-   Manche Geräte verfügen über keinen Bildschirm.

![IoT-Gerät](images/device-primer/device-primer-iot-device.png)

Typische Verwendung
-   Diese Geräte sind in der Regel über ein Netzwerk oder das Internet verbunden, um die erfassten realen Daten zu melden und in manchen Fällen auf diese Daten zu reagieren.
-   Auf diesen Geräten kann im Gegensatz zu Smartphones oder anderen größeren Geräten nur jeweils eine Anwendung ausgeführt werden.
-   Mit diesen Geräten wird nicht ständig interagiert, sie stehen bei Bedarf aber jederzeit zur Verfügung.
-   Die App besitzt keinen dedizierten Zurück-Befehl, dafür ist der Entwickler zuständig.

Hinweise zur Benutzeroberfläche
-   Monitorlose Geräte besitzen keinen Bildschirm.
-   Die Anzeige bei Geräten mit Monitor ist sehr klein. Aufgrund der begrenzten Bildschirmfläche und der eingeschränkten Funktionen werden nur notwendige Inhalte angezeigt.
-   Die Ausrichtung ist in den meisten Fällen gesperrt, sodass Sie bei Ihrer App nur eine Anzeigerichtung berücksichtigen müssen.

Eingaben
-   Variabel, abhängig vom Gerät

Typische Gerätefunktionen
-   Variabel, abhängig vom Gerät