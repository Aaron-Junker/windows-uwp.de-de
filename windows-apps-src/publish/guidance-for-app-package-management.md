---
Description: Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.
title: Leitfaden für die App-Paketverwaltung
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f514177ad5de7774e6926165435fd3b2d7b5e1f7
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468937"
---
# <a name="guidance-for-app-package-management"></a>Leitfaden für die App-Paketverwaltung

Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.

-   [Betriebssystemversionen und Package-distribution](#os-versions-and-package-distribution)
-   [Hinzufügen von Paketen für Windows 10 auf eine zuvor veröffentlichte app](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Verwalten der Kompatibilität von Paketen für Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Entfernen einer app aus dem Store](#removing-an-app-from-the-store)
-   [Entfernen von Paketen für eine zuvor unterstützt Gerätefamilie](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Betriebssystemversionen und Paketverteilung

Auf unterschiedlichen Betriebssystemen können unterschiedliche Pakettypen ausgeführt werden. Wenn mindestens zwei Pakete auf dem Gerät eines Kunden ausgeführt werden können, stellt der Microsoft Store die beste verfügbare Übereinstimmung bereit.

Im Allgemeinen können höhere Betriebssystemversionen Pakete ausführen, die auf frühere Betriebssystemversionen für dieselbe Gerätefamilie abzielen. Windows 10-Geräte können alle vorherige unterstützte Betriebssystemversionen (pro-Gerätefamilie) ausführen. Windows 10-desktop-Geräte können apps ausgeführt werden, die für Windows 8.1 oder Windows 8 erstellt wurden. Windows 10 mobile-Geräten können apps, die für Windows Phone 8.1, Windows Phone 8 und sogar Windows Phone erstellt wurden ausführen 7.x. Jedoch erhalten Kunden unter Windows 10, wenn die app auf UWP-Paketen, die für die entsprechenden Gerätefamilie enthalten nicht nur diese Pakete.

> [!IMPORTANT]
> Ab 31. Oktober 2018 keine Produkte neu erstellten Pakete mit dem Ziel Windows 8.x/Windows enthalten Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Entfernen einer App aus dem Store

Manchmal möchten Sie Kunden eine App vielleicht überhaupt nicht mehr anbieten, d. h. Sie heben die Veröffentlichung der App auf. Klicken Sie hierzu auf der Seite **App-Übersicht** auf **Make app unavailable**. Nachdem Sie bestätigt haben, dass die App nicht mehr verfügbar sein soll, wird sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und neue Kunden haben keine Möglichkeit, sie herunterzuladen (es sei denn, Sie verfügen über einen [Werbecode](generate-promotional-codes.md) und verfügen über ein Windows 10-Gerät).

> [!IMPORTANT]
> Durch diese Option werden alle in den Übermittlungen ausgewählten Einstellungen für [Sichtbarkeit](choose-visibility-options.md#discoverability) außer Kraft gesetzt. 

Diese Option hat dieselbe Wirkung, als ob Sie eine Übermittlung erstellt haben und **Make this product available but not discoverable in the Store** mit der Option **Erwerb beenden** auswählen. Allerdings ist das Erstellen einer neuen Übermittlung nicht erforderlich.

Beachten Sie, dass Kunden, die die App bereits besitzen, sie weiterhin verwenden und neu herunterladen können (und sogar Updates erhalten können, wenn Sie zu einem späteren Zeitpunkt neue Pakete übermitteln).

Nach der Bereitstellung der app nicht verfügbar ist, werden weiterhin es im Partner Center angezeigt. Wenn Sie die App Kunden erneut anbieten möchten, klicken Sie in der App-Übersicht auf **Make app available**. Nach dem Bestätigen ist die App für Neukunden innerhalb weniger Stunden verfügbar (es sei denn, es liegen Einschränkungen durch Einstellungen in der letzten Übermittlung vor).

> [!NOTE]
> Wenn die App verfügbar bleiben, neuen Kunden mit bestimmten Betriebssystemversionen jedoch nicht mehr angeboten werden soll, können Sie eine neue Übermittlung erstellen und alle Pakete für die Betriebssystemversion entfernen, unter der Sie neue Verkäufe verhindern möchten. Wenn Sie Pakete für Windows Phone 8.1 und Windows 10 bisher und nicht die app an neuen Kunden, die unter Windows Phone 8.1 Angebot behalten möchten, entfernen Sie alle Ihre Windows Phone 8.1-Pakete z. B. von der Einreichung. Nachdem das Update veröffentlicht wurde, kann keine neuen Kunden unter Windows Phone 8.1 zum Beschaffen der app, obwohl Sie weiterhin Kunden, die es noch nutzen können). Die app wird jedoch weiterhin für Neukunden unter Windows 10 verfügbar sein.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie

Wenn Sie alle Pakete für eine bestimmte entfernen [Gerätefamilie](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) , dass Ihre app zuvor werden Sie aufgefordert unterstützt um zu bestätigen, dass dies beabsichtigt ist, bevor Sie Ihre Änderungen speichern können, auf, die **Pakete** Seite.

Wenn Sie eine Einsendung, die alle Pakete entfernt, die auf eine Gerätefamilie ausgeführt werden konnte, die Ihre app zuvor unterstützt veröffentlichen, werden neue Kunden nicht zum Beschaffen der app auf diese Gerätefamilie können. Sie können jederzeit ein weiteres Update veröffentlichen, um erneut Pakete für diese Gerätefamilie anzubieten.

Hinweis: Auch wenn Sie alle Pakete entfernen, die eine bestimmte Gerätefamilie unterstützen, können vorhandene Kunden, die die App auf diesem Gerätetyp bereits installiert haben, diese weiterhin verwenden und erhalten alle Updates, die Sie später zur Verfügung stellen.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Hinzufügen von Paketen für Windows 10 auf eine zuvor veröffentlichte app

Wenn Sie in der Store-app verfügen, die nur die Pakete für Windows enthalten 8.x bzw. Windows Phone 8.x, und Sie möchten, aktualisieren Sie Ihre app für Windows 10, erstellen eine neue Eingabe aus, und fügen Ihrem UWP .msixupload oder .appxupload-Pakete während der die [Pakete](upload-app-packages.md) Schritt. Nachdem Ihre app den Zertifizierungsprozess durchläuft, ist das UWP-Paket auch für neue Übernahmen von Kunden unter Windows 10 verfügbar.

> [!NOTE]
> Sobald ein Kunde auf Windows 10 Ihre UWP-Paket abruft, können nicht Sie Kunden zurücksetzen, an der Verwendung eines Pakets für alle vorherigen Betriebssystemversion. 

Beachten Sie, dass die Versionsnummer der Windows 10-Pakete höher als die für alle Windows 8, Windows 8.1 und/oder Windows Phone 8.1-Pakete sein muss, die Sie verwendet haben. Weitere Informationen finden Sie unter [Paketversionsnummern](package-version-numbering.md).

Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken von Apps](../packaging/index.md).
