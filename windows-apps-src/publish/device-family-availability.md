---
description: Nachdem Ihre Pakete erfolgreich hochgeladen wurden, wird eine Tabelle angezeigt, in der angegeben wird, welche Pakete bestimmten Windows 10-Gerätefamilien (und ggf. früheren Betriebssystemversionen) in Rangfolge angeboten werden.
title: Verfügbarkeit von Gerätefamilien
ms.date: 02/26/2021
ms.topic: article
keywords: Windows 10, UWP, Pakete, Upload, Verfügbarkeit der Gerätefamilie
ms.localizationpriority: medium
ms.openlocfilehash: 3885a5f85a8380e87d753721c6b9ca8b0facc475
ms.sourcegitcommit: e8ea2a36e4f2b9e0326958d226a36dd30c3efa57
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2021
ms.locfileid: "105099791"
---
# <a name="device-family-availability"></a>Verfügbarkeit von Gerätefamilien

Nachdem die Pakete [auf der Seite **Pakete** erfolgreich hochgeladen](upload-app-packages.md)wurden, wird im Abschnitt **Verfügbarkeit der Gerätefamilie** eine Tabelle angezeigt, die angibt, welche Pakete bestimmten Windows 10-Gerätefamilien (und ggf. früheren Betriebssystemversionen) in Rangfolge angeboten werden. In diesem Abschnitt können Sie auch entscheiden, ob Sie die Übermittlung an Kunden in bestimmten Windows 10-Gerätefamilien anbieten möchten.

> [!NOTE]
> Wenn Sie noch keine Pakete hochgeladen haben, werden im Abschnitt zur **Verfügbarkeit der Gerätefamilie** die Windows 10-Gerätefamilien mit Kontrollkästchen angezeigt, mit denen Sie angeben können, ob die Übermittlung für Kunden dieser Gerätefamilien angeboten werden soll. Die Tabelle wird angezeigt, nachdem Sie ein oder mehrere Pakete hochgeladen haben.

Dieser Abschnitt enthält auch ein Kontrollkästchen, mit dem Sie angeben können, ob Microsoft die APP für alle zukünftigen Windows 10-Gerätefamilien verfügbar machen soll. Es wird empfohlen, diese Option aktiviert zu lassen, sodass Ihre App für mehr potenzielle Kunden zur Verfügung gestellt wird, wenn neue Gerätefamilien eingeführt werden.


## <a name="choosing-which-device-families-to-support"></a>Auswählen der unterstützten Gerätefamilien

Wenn Sie Pakete hochladen, die auf eine einzelne Gerätefamilie abzielen, aktivieren wir das Kontrollkästchen, um diese Pakete neuen Kunden auf diesem Gerätetyp zur Verfügung zu stellen. Wenn ein Paket beispielsweise auf "Windows. Desktop" festgelegt ist, wird das Feld " **Windows 10 Desktop** " für dieses Paket überprüft (und Sie können die Kontrollkästchen für andere Gerätefamilien nicht aktivieren).

Pakete für die Windows. Universal-Gerätefamilie können auf jedem Windows 10-Gerät (einschließlich Xbox One) ausgeführt werden. Diese Pakete werden standardmäßig für neue Kunden auf allen Gerätetypen *mit Ausnahme* von Xbox verfügbar gemacht.

Sie können das Kontrollkästchen für eine Windows 10-Gerätefamilie deaktivieren, wenn Sie Kunden auf diesem Gerät keine Übermittlung anbieten möchten. Wenn das Kontrollkästchen für eine Gerätefamilie deaktiviert ist, können neue Kunden die App auf diesem Gerätetyp nicht erwerben (Kunden, die bereits über die App verfügen, können diese allerdings weiterhin nutzen und erhalten alle von Ihnen übermittelten Aktualisierungen).

Wenn Ihre APP diese unterstützt, empfiehlt es sich, alle Felder zu aktivieren, es sei denn, Sie haben einen bestimmten Grund, die Typen von Windows 10-Geräten einzuschränken, von denen Ihre APP abgerufen werden kann. Wenn Sie beispielsweise wissen, dass Ihre APP für [Surface Hub](https://developer.microsoft.com/windows/surfacehub) und/oder [Microsoft hololens](https://developer.microsoft.com/mixed-reality)keine gute Leistung bietet, können Sie das Kontrollkästchen **Windows 10 Team** und/oder **Windows 10 Holographic** deaktivieren. Dadurch wird verhindert, dass neue Kunden die APP auf diesen Geräten erwerben. Wenn Sie zu einem späteren Zeitpunkt festlegen, dass Sie diese für diese Kunden anbieten möchten, können Sie eine neue Übermittlung erstellen, indem Sie die Kontrollkästchen aktiviert haben.

<span id="xbox" />

Die einzige Windows 10-Gerätefamilie, die für Windows nicht standardmäßig aktiviert ist, ist **Windows 10 Xbox**. Wenn Ihre APP kein Spiel ist (oder wenn es sich um ein Spiel handelt und Sie das [Xbox Live Creators-Programm](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) aktiviert oder den Genehmigungsprozess des [Konzepts](../gaming/concept-approval.md) durchlaufen haben), und ihre Übermittlung neutrale und/oder x64-UWP-Pakete enthält, die mit dem Windows 10 SDK, Version 14393 oder höher, kompiliert wurden, können Sie das **Windows 10 Xbox** -Kontrollkästchen aktivieren, um die APP Kunden

> [!IMPORTANT]
> Damit Ihre APP auf Xbox-Geräten gestartet werden kann, müssen Sie ein neutrales oder x64-Paket einschließen, das mit Windows SDK Version 14393 oder höher kompiliert wird. Wenn Sie jedoch **Windows 10 Xbox** aktivieren, wird das Paket mit der höchsten Versions Angabe, das auf Xbox (d. h. ein neutrales oder x64-Paket, das auf die Xbox-oder universelle Gerätefamilie abzielt) anwendbar ist, immer für Kunden auf der Xbox angeboten, auch wenn es mit einer früheren SDK-Version kompiliert wurde. Daher müssen Sie unbedingt sicherstellen, dass das für Xbox geeignete Paket mit der höchsten Versionsnummer mit Windows SDK-Version 14393 oder höher kompiliert wurde. Andernfalls wird eine Fehlermeldung angezeigt, in der darauf hingewiesen wird, dass Xbox-Kunden die App nicht starten können. 
> 
> Zum Beheben dieses Fehlers können Sie eine der folgenden Maßnahmen ergreifen:
> - Ersetzen Sie die entsprechenden Pakete durch neue, die mit Windows SDK-Version 14393 oder höher kompiliert wurden.
> - Wenn Sie bereits über ein Paket verfügen, das Xbox unterstützt und mit Windows SDK-Version 14393 oder höher kompiliert wurde, erhöhen Sie die Versionsnummer, sodass dieses Paket die höchste Versionsnummer innerhalb der Übermittlung trägt.
> - Deaktivieren Sie das Kontrollkästchen für **Windows 10 Xbox**.
>   
> Wenn Sie das Problem immer noch nicht beheben können, wenden Sie sich an den Support.

Wenn Sie eine UWP-App für Windows 10 IOT Core übermitteln, sollten Sie nach dem Hochladen der Pakete keine Änderungen an den Standardeinstellungen vornehmen. Es gibt kein separates Kontrollkästchen für Windows 10 IOT. Weitere Informationen zum Veröffentlichen von IOT Core-UWP-apps finden Sie [unter Microsoft Store-Unterstützung für IOT Core-UWP-apps](/windows/iot-core/commercialize-your-device/installingandservicing).

Wenn Ihre Übermittlung für eine zuvor veröffentlichte App Pakete enthält, die unter **Windows 8/8.1** und **Windows Phone 8. x und früher** ausgeführt werden können, werden diese Pakete den Kunden auf diesen Betriebssystemversionen zur Verfügung gestellt. Entfernen Sie die entsprechenden Pakete aus ihrer Übermittlung, um die APP für diese Kunden nicht mehr anbieten zu können.

> [!IMPORTANT]
> Um vollständig zu verhindern, dass eine bestimmte Windows 10-Gerätefamilie ihre Übermittlung erhält, aktualisieren Sie das [**targetdevicefamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) -Element in ihrem Manifest so, dass es nur auf die Gerätefamilie abzielt, die Sie unterstützen möchten (d.h. Windows. Mobile oder Windows. Desktop), anstatt Sie als Windows. Universal-Wert (für die universelle Gerätefamilie Microsoft Visual Studio) zu belassen, der standardmäßig

Es ist wichtig zu wissen, dass die im Abschnitt zur **Verfügbarkeit der Gerätefamilie** vorgetroffene Auswahl nur auf neue Akquisitionen anwendbar ist. Jede Person, die Ihre APP bereits verwendet, kann Sie weiterhin verwenden und erhält alle von Ihnen übermittelten Updates, auch wenn Sie Ihre Gerätefamilie hier entfernen. Dies gilt auch für Kunden, die Ihre App vor dem Upgrade auf Windows 10 erworben haben. Wenn Sie z. b. über eine veröffentlichte App mit Windows Phone 8,1-Paketen verfügen und ein Windows 10-Paket (UWP) für die Windows. Universal-Gerätefamilie hinzufügen, wird Windows 10 Mobile-Kunden mit Ihrem Windows Phone 8,1-Paket ein Update für dieses Windows 10-Paket (UWP) angeboten, auch wenn Sie das Kontrollkästchen für **Windows 10 Mobile** deaktiviert haben.

Weitere Informationen zu Gerätefamilien finden Sie unter [Programmieren mit Erweiterungs-sdert](/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Grundlegendes zur Bewertung

Abgesehen davon, dass Sie angeben können, welche Windows 10-Gerätefamilien ihre Übermittlung herunterladen können, werden im Abschnitt zur **Verfügbarkeit der Gerätefamilie** die Pakete angezeigt, die für verschiedene Gerätefamilien verfügbar gemacht werden. Wenn mehrere Ihrer Pakete auf einer bestimmten Gerätefamilie ausgeführt werden können, wird in der Tabelle die Reihenfolge angegeben, in der Pakete basierend auf der Versionsnummer angeboten werden. Weitere Informationen dazu, wie der Store Pakete auf Grundlage der Versionsnummern bewertet, finden Sie unter [Paketversionsnummern](package-version-numbering.md). 

Angenommen, Sie haben die beiden Pakete Package_A.appxupload und Package_B.appxupload. Wenn für eine bestimmte Gerätefamilie, Package_A.appxupload den Rang 1 und Package_B.appxupload den Rang 2 hat, bedeutet dies, das der Store an einen Kunden mit diesem Gerätetyp, der Ihre App erwirbt, zunächst Package_A.appxupload ausliefert. Wenn Package_A.appxupload auf den Gerät des Kunden nicht ausgeführt werden kann, bietet der Store Package_B.appxupload an. Wenn das Gerät des Kunden keines der Pakete für diese Gerätefamilie ausführen kann (z. b. wenn die von ihrer App unterstützte **MinVersion** höher als die Version auf dem Gerät des Kunden ist), kann der Kunde die APP nicht auf diesem Gerät herunterladen.

> [!NOTE]
> Die Versionsnummern in XAP-Paketen (für zuvor veröffentlichte Apps) werden nicht berücksichtigt, wenn bestimmt wird, welches Paket einem bestimmten Kunden bereitgestellt werden soll. Daher wird bei mehreren gleichrangigen XAP-Paketen keine Nummer, sondern ein Sternchen angezeigt, und die Kunden können jedes der Pakete erhalten. Wenn ein XAP-Paket für einen Kunden auf ein neueres aktualisiert werden soll, stellen Sie sicher, dass die älteren XAP-Dateien aus der neuen Übermittlung entfernt werden.
