---
description: Dieser Artikel beschreibt allgemeine Fehlercodes für Store-Vorgänge für Apps und Add-Ons, einschließlich In-App-Einkäufe, Lizenzierung und selbst installierte App-Aktualisierungen.
title: Fehlercodes für Store-Vorgänge
ms.date: 08/24/2017
ms.topic: article
keywords: Windows 10, UWP, In-App-Einkäufe, IAPs, Add-Ons, Fehlercodes
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662875"
---
# <a name="error-codes-for-store-operations"></a>Fehlercodes für Store-Vorgänge

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Dieser Artikel beschreibt allgemeine Fehlercodes, die beim Entwickeln oder Testen von Store-Operationen in Ihrer App auftreten können.

## <a name="in-app-purchase-error-codes"></a>In-App-Kauf-Fehlercodes

Die folgenden Fehlercodes beziehen sich auf Vorgänge beim In-App-Kauf.

|  Fehlercode  |  Beschreibung  |
|--------------|---------------|
| 0x803F6100   | Der In-App-Kauf konnte nicht abgeschlossen werden, da die Kinderecke aktiv ist. Melden Sie sich am Gerät mit Ihrem Microsoft-Konto an und führen Sie die Anwendung erneut aus, um den Kauf abzuschließen.               |
| 0x803F6101   | Die angegebene App konnte nicht gefunden werden. Die App ist eventuell nicht mehr im Store verfügbar oder Sie haben möglicherweise die falsche Store-ID für die App angegeben.     |
| 0x803F6102   | Das angegebene Add-On konnte nicht gefunden werden. Das Add-On ist eventuell nicht mehr im Store verfügbar oder Sie haben möglicherweise die falsche Store-ID für das Add-On angegeben.                                               |
| 0x803F6103   | Das angegebene Produkt konnte nicht gefunden werden. Das Produkt ist eventuell nicht mehr im Store verfügbar oder Sie haben möglicherweise die falsche Store-ID für das Produkt angegeben.                                          |
| 0x803F6104   | Der In-App-Kauf konnte nicht abgeschlossen werden, da Sie eine Testversion der App ausführen. Um In-App-Käufe abzuschließen, installieren Sie die Vollversion der App.               |
| 0x803F6105   | Der In-App-Kauf konnte nicht abgeschlossen werden, da Sie nicht mit Ihrem Microsoft-Konto angemeldet sind.                                              |
| 0x803F6107   | Während der Verarbeitung des aktuellen Vorgangs ist ein unerwarteter Fehler aufgetreten.                                             |
| 0x803F6108   | Der In-App-Kauf konnte nicht abgeschlossen werden, da die App-Lizenzinformationen fehlen. Dieser Fehler kann auftreten, wenn Sie Ihre App querladen. Um dieses Problem zu beheben, deinstallieren Sie die App und installieren Sie sie erneut vom Store, um die App-Lizenz zu aktualisieren.                                          |
| 0x803F6109   | Die Erfüllung des konsumierbaren Add-Ons konnte nicht abgeschlossen werden, da die angegebene Menge mehr als der Restbetrag ist.        |
| 0x803F610A   | Der angegebene Anbietertyp für das Store-Benutzerkonto wird nicht unterstützt.                                            |
| 0x803F610B   | Der angegebene Storevorgang wird nicht unterstützt.                                             |
| 0x803F610C   | Die App unterstützt den angegebenen Hintergrundaufgabenvertrag nicht.                                             |
| 0x80040001   | Die bereitgestellte Liste der Add-On-Produkt-IDs ist ungültig.                        |
| 0x80040002   | Die bereitgestellte Liste mit Schlüsselwörtern ist ungültig.                   |
| 0x80040003   | Die Erfüllungsziel ist ungültig.                       |

## <a name="licensing-error-codes"></a>Lizenzierungsfehlercodes

Die folgenden Fehlercodes beziehen sich auf Vorgänge für Apps oder Add-Ons Lizenzierungen.

|  Fehlercode  |  Beschreibung  |
|--------------|---------------|
| 0x803F700C   | Das Gerät ist zurzeit offline. Um diese App zu verwenden, während das Gerät offline ist, öffnen Sie die Store-Einstellungen und schalten Sie die Einstellungen für die **Offlineberechtigungen** um.            |
| 0x803F8001   | Sie haben keine Berechtigung für das Produkt. Sie verwenden möglicherweise ein anderes Microsoft-Konto als das, das zum Produktkauf verwendet wurde.           |
| 0x803F8002   | Ihre Berechtigung für das Produkt ist abgelaufen.           |
| 0x803F8003   | Ihre Berechtigung für das Produkt ist in einem ungültigen Zustand, der verhindert, dass eine Lizenz erstellt werden kann.   |
| 0x803F8009<br/>0x803F800A   | Der Testzeitraum für die App ist abgelaufen.   |
| 0x803F8190   |  Die Lizenz lässt das Produkt nicht im aktuellen Land oder der Region Ihres Geräts zu, in dem es verwendet wird.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Sie haben die maximale Anzahl von Geräten erreicht, die mit Spielen und Apps aus dem Store verwendet werden können. Um das Spiel oder die App auf dem aktuellen Gerät verwenden zu können, müssen Sie zunächst ein anderes Gerät von Ihrem Konto entfernen.  |
| 0x803F9000<br/>0x803F9001    |  Die Lizenz ist abgelaufen oder beschädigt. Zum Beheben dieses Fehlers führen Sie die [Problembehandlung für Windows-apps](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) Zurücksetzen des Caches Store.     |
| 0x803F9006    |  Der Vorgang konnte nicht abgeschlossen werden, da der Benutzer des Produkts hat auf dem Gerät mit seinem Microsoft-Konto angemeldet ist.            |
| 0x803F9008<br/>0x803F9009    |  Ihr Gerät ist offline. Das Gerät muss online sein, um das Produkt zu verwenden.            |
| 0x803F900A    |  Das Abonnement ist abgelaufen.            |


## <a name="self-install-update-error-codes"></a>Selbstinstallation des Updates – Fehlercodes

Die folgenden Fehlercodes stehen im Zusammenhang mit der [Selbstinstallationen von Paketupdates](../packaging/self-install-package-updates.md).

|  Fehlercode  |  Beschreibung  |
|--------------|---------------|
| 0x803F6200   | Die Zustimmung des Benutzers ist erforderlich, um das Paketupdate herunterzuladen.               |
| 0x803F6201   | Die Zustimmung des Benutzers ist erforderlich, um das Paketupdate herunterzuladen und zu installieren.                                                  |
| 0x803F6203   | Die Zustimmung des Benutzers ist erforderlich, um das Paketupdate zu installieren.                                         |
| 0x803F6204   | Die Zustimmung des Benutzers ist erforderlich, um das Paketupdate herunterzuladen, da der Download bei einer getakteten Verbindung erfolgt.                                             |
| 0x803F6206   | Die Zustimmung des Benutzers ist erforderlich, um das Paketupdate herunterzuladen und zu installieren, da der Download bei einer getakteten Verbindung erfolgt.     |


## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
* [Abrufen von Lizenzinformationen für apps und -Add-Ons](get-license-info-for-apps-and-add-ons.md)
* [Aktivieren von in-app-Käufe von apps und -Add-Ons](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Aktivieren Sie nutzbar Add-on-Käufe](enable-consumable-add-on-purchases.md)
* [Aktivieren Sie die Abonnement-Add-Ons für Ihre app](enable-subscription-add-ons-for-your-app.md)
* [Implementieren Sie eine Testversion von Ihrer app](implement-a-trial-version-of-your-app.md)
