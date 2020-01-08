---
Description: Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.
title: Leitfaden für die Verwaltung von App-Paketen
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9f5caa2610e19234cfd83119d570f858c540b401
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685136"
---
# <a name="guidance-for-app-package-management"></a>Leitfaden für die Verwaltung von App-Paketen

Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.

-   [Betriebssystemversionen und Paketverteilung](#os-versions-and-package-distribution)
-   [Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Entfernen einer App aus dem Store](#removing-an-app-from-the-store)
-   [Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Betriebssystemversionen und Paketverteilung

Auf unterschiedlichen Betriebssystemen können unterschiedliche Pakettypen ausgeführt werden. Wenn mindestens zwei Pakete auf dem Gerät eines Kunden ausgeführt werden können, stellt der Microsoft Store die beste verfügbare Übereinstimmung bereit.

Im Allgemeinen können höhere Betriebssystemversionen Pakete ausführen, die auf frühere Betriebssystemversionen für dieselbe Gerätefamilie abzielen. Auf Windows 10-Geräten können alle früheren unterstützten Betriebssystemversionen (pro Gerätefamilie) ausgeführt werden. Windows 10-Desktop Geräte können apps ausführen, die für Windows 8.1 oder Windows 8 erstellt wurden. Windows 10 Mobile-Geräte können apps ausführen, die für Windows Phone 8,1, Windows Phone 8 und sogar Windows Phone 7. x erstellt wurden. Kunden unter Windows 10 erhalten diese Pakete jedoch nur dann, wenn die APP keine UWP-Pakete enthält, die auf die jeweilige Gerätefamilie abzielen.

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 können neu erstellte Produkte keine Pakete enthalten, die auf Windows 8. x/Windows Phone 8. x oder früher abzielen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Entfernen einer App aus dem Store

Manchmal möchten Sie Kunden eine App vielleicht überhaupt nicht mehr anbieten, d. h. Sie heben die Veröffentlichung der App auf. Klicken Sie hierzu auf der Seite **App-Übersicht** auf **Make app unavailable**. Nachdem Sie bestätigt haben, dass die App nicht mehr verfügbar sein soll, wird sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und neue Kunden haben keine Möglichkeit, sie herunterzuladen (es sei denn, Sie verfügen über einen [Werbecode](generate-promotional-codes.md) und verfügen über ein Windows 10-Gerät).

> [!IMPORTANT]
> Durch diese Option werden alle in den Übermittlungen ausgewählten Einstellungen für [Sichtbarkeit](choose-visibility-options.md#discoverability) außer Kraft gesetzt. 

Diese Option hat dieselbe Wirkung, als ob Sie eine Übermittlung erstellt haben und **Make this product available but not discoverable in the Store** mit der Option **Erwerb beenden** auswählen. Allerdings ist das Erstellen einer neuen Übermittlung nicht erforderlich.

Beachten Sie, dass Kunden, die die App bereits besitzen, sie weiterhin verwenden und neu herunterladen können (und sogar Updates erhalten können, wenn Sie zu einem späteren Zeitpunkt neue Pakete übermitteln).

Nachdem Sie die APP nicht verfügbar gemacht haben, wird Sie weiterhin im Partner Center angezeigt. Wenn Sie die App Kunden erneut anbieten möchten, klicken Sie in der App-Übersicht auf **Make app available**. Nach dem Bestätigen ist die App für Neukunden innerhalb weniger Stunden verfügbar (es sei denn, es liegen Einschränkungen durch Einstellungen in der letzten Übermittlung vor).

> [!NOTE]
> Wenn die App verfügbar bleiben, neuen Kunden mit bestimmten Betriebssystemversionen jedoch nicht mehr angeboten werden soll, können Sie eine neue Übermittlung erstellen und alle Pakete für die Betriebssystemversion entfernen, unter der Sie neue Verkäufe verhindern möchten. Wenn Sie z. b. bereits Pakete für Windows Phone 8,1 und Windows 10 hatten und die APP den neuen Kunden auf Windows Phone 8,1 nicht weiter anbieten möchten, entfernen Sie alle Ihre Windows Phone 8,1-Pakete aus der Übermittlung. Nachdem das Update veröffentlicht wurde, können keine neuen Kunden auf Windows Phone 8,1 die APP erwerben, auch wenn Kunden, die Sie bereits besitzen, Sie weiterhin verwenden können. Die APP steht jedoch weiterhin für neue Kunden unter Windows 10 zur Verfügung.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie

Wenn Sie alle Pakete für eine bestimmte [Gerätefamilie](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) entfernen, die zuvor von ihrer App unterstützt wurde, werden Sie aufgefordert, zu bestätigen, dass dies beabsichtigt ist, bevor Sie die Änderungen auf der Seite " **Pakete** " speichern können.

Wenn Sie eine Übermittlung veröffentlichen, mit der alle Pakete entfernt werden, die auf einer Gerätefamilie ausgeführt werden können, die von Ihrer APP zuvor unterstützt wurde, können neue Kunden die APP nicht mehr auf dieser Gerätefamilie erwerben. Sie können jederzeit ein weiteres Update veröffentlichen, um erneut Pakete für diese Gerätefamilie anzubieten.

Hinweis: Auch wenn Sie alle Pakete entfernen, die eine bestimmte Gerätefamilie unterstützen, können vorhandene Kunden, die die App auf diesem Gerätetyp bereits installiert haben, diese weiterhin verwenden und erhalten alle Updates, die Sie später zur Verfügung stellen.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App

Wenn Sie über eine APP im Store verfügen, in der nur Pakete für Windows 8. x und/oder Windows Phone 8. x enthalten sind, und Sie Ihre APP für Windows 10 aktualisieren möchten, erstellen Sie eine neue Übermittlung, und fügen Sie Ihr UWP. msixupload-oder. appxupload-Paket [während des Paket](upload-app-packages.md) Schritts hinzu. Nachdem Ihre APP den Zertifizierungsprozess durchlaufen hat, steht das UWP-Paket auch für neue Akquisitionen von Kunden unter Windows 10 zur Verfügung.

> [!NOTE]
> Nachdem ein Kunde unter Windows 10 das UWP-Paket abgerufen hat, können Sie diesen Kunden nicht mehr für eine frühere Betriebssystemversion zurücksetzen. 

Beachten Sie, dass die Versionsnummer Ihrer Windows 10-Pakete höher sein muss als die für alle Windows 8-, Windows 8.1-und Windows Phone 8,1-Pakete, die Sie verwendet haben. Weitere Informationen finden Sie unter [Paketversionsnummern](package-version-numbering.md).

Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken von Apps](../packaging/index.md).
