---
description: Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.
title: Leitfaden für die App-Paketverwaltung
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6035f5f21cd1b704415193c393ae0637d3e5dc37
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411974"
---
# <a name="guidance-for-app-package-management"></a>Leitfaden für die App-Paketverwaltung

Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.

-   [Betriebssystemversionen und Paketverteilung](#os-versions-and-package-distribution)
-   [Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Entfernen einer App aus dem Store](#removing-an-app-from-the-store)
-   [Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Betriebssystemversionen und Paketverteilung

Auf unterschiedlichen Betriebssystemen können unterschiedliche Pakettypen ausgeführt werden. Wenn mehr als eine Ihrer Pakete auf dem Gerät eines Kunden ausgeführt werden kann, bietet die Microsoft Store die beste verfügbare Entsprechung.

Im Allgemeinen können höhere Betriebssystemversionen Pakete ausführen, die auf frühere Betriebssystemversionen für dieselbe Gerätefamilie abzielen. Auf Windows 10-Geräten können alle früheren unterstützten Betriebssystemversionen (pro Gerätefamilie) ausgeführt werden. Auf Desktopgeräten unter Windows 10 können Apps ausgeführt werden, die für Windows 8.1 oder Windows 8 erstellt wurden. Auf Mobilgeräten unter Windows 10 können Apps ausgeführt werden, die für Windows Phone 8.1, Windows Phone 8 und sogar Windows Phone 7.x erstellt wurden. Kunden unter Windows 10 erhalten diese Pakete jedoch nur dann, wenn die APP keine UWP-Pakete enthält, die auf die jeweilige Gerätefamilie abzielen.

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Entfernen einer App aus dem Store

Manchmal können Sie die Bereitstellung einer APP für Kunden beenden und die Veröffentlichung der APP effektiv aufheben. Klicken Sie hierzu auf der Seite **App-Übersicht** auf **App nicht verfügbar machen** . Nachdem Sie sich vergewissert haben, dass Sie die APP nicht mehr verfügbar machen möchten, wird Sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und keine neuen Kunden werden in der Lage sein, Sie zu erhalten (es sei denn, Sie haben einen [Werbe Code](generate-promotional-codes.md) , der ein Windows 10-Gerät verwendet).

> [!IMPORTANT]
> Mit dieser Option werden alle [Sichtbarkeits](choose-visibility-options.md#discoverability) Einstellungen außer Kraft gesetzt, die Sie in ihren Übermittlungen ausgewählt haben. 

Diese Option hat denselben Effekt wie, wenn Sie eine Übermittlung erstellt haben, und wählen Sie **dieses Produkt verfügbar, aber nicht im Store** mit der Option für die **Beendigung des Erwerbs** verfügbar machen aus. Es ist jedoch nicht erforderlich, dass Sie eine neue Übermittlung erstellen.

Beachten Sie, dass alle Kunden, die bereits über die APP verfügen, Sie weiterhin verwenden können. Sie können Sie erneut herunterladen (und können sogar Updates erhalten, wenn Sie später neue Pakete einreichen).

Nachdem Sie die APP nicht verfügbar gemacht haben, wird Sie weiterhin im Partner Center angezeigt. Wenn Sie die App Kunden erneut anbieten möchten, klicken Sie in der App-Übersicht auf **Make app available**. Nach dem Bestätigen ist die App für Neukunden innerhalb weniger Stunden verfügbar (es sei denn, es liegen Einschränkungen durch Einstellungen in der letzten Übermittlung vor).

> [!NOTE]
> Wenn Sie möchten, dass Ihre app verfügbar ist, Sie aber nicht weiter für neue Kunden mit einer bestimmten Betriebssystemversion anbieten möchten, können Sie eine neue Übermittlung erstellen und alle Pakete für die Betriebssystemversion entfernen, auf der Sie neue Akquisitionen verhindern möchten. Wenn Sie z. b. bereits Pakete für Windows Phone 8,1 und Windows 10 hatten und die APP den neuen Kunden auf Windows Phone 8,1 nicht weiter anbieten möchten, entfernen Sie alle Ihre Windows Phone 8,1-Pakete aus der Übermittlung. Nachdem das Update veröffentlicht wurde, können keine neuen Kunden auf Windows Phone 8,1 die APP erwerben, auch wenn Kunden, die Sie bereits besitzen, Sie weiterhin verwenden können. Die APP steht jedoch weiterhin für neue Kunden unter Windows 10 zur Verfügung.

## <a name="removing-packages-for-a-previously-supported-device-family"></a>Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie

Wenn Sie alle Pakete für eine bestimmte Gerätefamilie entfernen (siehe [Programmieren mit Erweiterungs-sdten](/uwp/extension-sdks/device-families-overview)), die Ihre APP zuvor unterstützt hat, werden Sie aufgefordert, zu bestätigen, dass dies beabsichtigt ist, bevor Sie Ihre Änderungen auf der Seite " **Pakete** " speichern können.

Wenn Sie eine Übermittlung veröffentlichen, mit der alle Pakete entfernt werden, die auf einer Gerätefamilie ausgeführt werden können, die von Ihrer APP zuvor unterstützt wurde, können neue Kunden die APP nicht mehr auf dieser Gerätefamilie erwerben. Sie können jederzeit ein weiteres Update veröffentlichen, um erneut Pakete für diese Gerätefamilie anzubieten.

Hinweis: Auch wenn Sie alle Pakete entfernen, die eine bestimmte Gerätefamilie unterstützen, können vorhandene Kunden, die die App auf diesem Gerätetyp bereits installiert haben, diese weiterhin verwenden und erhalten alle Updates, die Sie später zur Verfügung stellen.

<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App

Wenn Sie über eine APP im Store verfügen, in der nur Pakete für Windows 8. x und/oder Windows Phone 8. x enthalten sind, und Sie Ihre APP für Windows 10 aktualisieren möchten, erstellen Sie eine neue Übermittlung, und fügen Sie Ihr UWP. msixupload-oder. appxupload-Paket [während des Paket](upload-app-packages.md) Schritts hinzu. Nachdem Ihre APP den Zertifizierungsprozess durchlaufen hat, steht das UWP-Paket auch für neue Akquisitionen von Kunden unter Windows 10 zur Verfügung.

> [!NOTE]
> Nachdem ein Kunde unter Windows 10 das UWP-Paket abgerufen hat, können Sie diesen Kunden nicht mehr für eine frühere Betriebssystemversion zurücksetzen. 

Beachten Sie, dass die Versionsnummer Ihrer Windows 10-Pakete höher sein muss als die für alle Windows 8-, Windows 8.1-und Windows Phone 8,1-Pakete, die Sie verwendet haben. Weitere Informationen finden Sie unter [Paket Versionsnummerierung](package-version-numbering.md).

Weitere Informationen zum Packen von UWP-Apps für den Store finden Sie unter [Verpacken von apps](../packaging/index.md).
