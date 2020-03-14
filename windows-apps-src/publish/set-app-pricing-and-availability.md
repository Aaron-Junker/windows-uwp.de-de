---
Description: Auf der Seite „Preise und Verfügbarkeit“ des App-Übermittlungsprozesses können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist.
title: Festlegen der Preise und Verfügbarkeit von Apps
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Preis, verfügbar, sichtbar, kostenlose Testversion, Testversionen, Testversion, Apps, Veröffentlichungsdatum
ms.localizationpriority: medium
ms.openlocfilehash: 715e4c677b3b3e62b9ff515396d3582c3fd99184
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210556"
---
# <a name="set-app-pricing-and-availability"></a>Festlegen der Preise und Verfügbarkeit von Apps


Auf der Seite **Preise und Verfügbarkeit** des [App-Übermittlungsprozesses](app-submissions.md) können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist. Wir stellen Ihnen hier die auf dieser Seite verfügbaren Optionen vor und informieren Sie darüber, was Sie bei der Eingabe dieser Informationen beachten sollten.


## <a name="markets"></a>Märkte

Der Microsoft Store erreicht Kunden in über 200 Ländern und Regionen in aller Welt. Standardmäßig werden Ihre App in allen möglichen Märkten angeboten. Falls gewünscht, können Sie die spezifischen Märkte auswählen, in denen Sie Ihre App anbieten möchten. 

Weitere Informationen erhalten Sie unter [Festlegen der Marktauswahl](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Sichtbarkeit

Im Abschnitt **Sichtbarkeit** können Sie Einschränkungen festlegen, wie die App ermittelt und erworben werden kann, z. B., ob Benutzer Ihre App im Store oder im Store-Eintrag überhaupt finden können.

Weitere Informationen finden Sie unter [Sichtbarkeitsoptionen auswählen](choose-visibility-options.md).


## <a name="schedule"></a>Zeitplan

Standardmäßig (es sei denn, Sie haben eine der Optionen für **Make this app available but not discoverable in the Store** im Abschnitt [Sichtbarkeit](choose-visibility-options.md#discoverability) ausgewählt) wird Ihre App für Kunden zur Verfügung gestellt, sobald sie die Zertifizierung bestanden und den Veröffentlichungsprozess abgeschlossen hat. Um ein anderes Datum auszuwählen, wählen Sie **Optionen anzeigen**, um diesen Abschnitt zu erweitern. 

Weitere Informationen finden Sie unter [Konfigurieren des genauen Veröffentlichungszeitplans](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preise

Sie müssen einen Grundpreis für Ihre App auswählen (es sei denn, Sie haben die Option **Erwerb beenden** unter **Make this app available but not discoverable in the Store** im Abschnitt [Sichtbarkeit](choose-visibility-options.md#discoverability) ausgewählt) und entweder **Kostenlos** oder eines der verfügbaren Preisniveaus auswählen. Sie können auch Preisänderungen planen, um das Datum und die Uhrzeit anzugeben, an dem bzw. zu der sich der Preis Ihrer App ändern soll. Darüber hinaus haben Sie die Möglichkeit, diese Änderungen für bestimmte Märkte anzupassen. 

Weitere Informationen finden Sie unter [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Kostenlose Testversion

Viele Entwickler bieten Kunden die Möglichkeit, ihre App mithilfe der im Store verfügbaren Testfunktionen kostenlos auszuprobieren. Standardmäßig ist **Keine kostenlose Testversion** ausgewählt. Für Ihre App gibt es keine Testversion. Wenn Sie eine Testversion anbieten möchten, können Sie einen Wert aus der Dropdownliste **Kostenlose Testversion** auswählen.

Es gibt zwei Arten von Testversionen, die Sie auswählen können, und Sie haben die Möglichkeit zum Konfigurieren von Datum und Uhrzeit, an dem bzw. zu der das Angebot der Testversion starten und enden soll.

### <a name="time-limited"></a>Zeitlich begrenzt

Wählen Sie **Zeitlich begrenzt**, um es Benutzern zu ermöglichen, Ihre App für eine bestimmte Anzahl von Tagen kostenlos zu testen: **1 Tag**, **7 Tage**, **15 Tage** oder **30 Tage**. Sie können Features durch Hinzufügen von Code zum [Ausschließen oder Beschränken von Features in der Testversion](../monetize/in-app-purchases-and-trials.md) einschränken oder Kunden den Zugriff auf die vollständigen Funktionen während dieses Zeitraums gewähren. 
> [!NOTE]
> zeitlich begrenzte Testversionen werden Kunden unter Windows 10 Build 10.0.10586 oder früher oder Kunden auf Windows Phone 8,1 und früher nicht angezeigt.

### <a name="unlimited"></a>Unbegrenzt

Wählen Sie **Unbegrenzt**, damit Kunden unbegrenzte Zeit kostenlos auf Ihre App zugreifen dürfen. Da Sie Ihre Kunden zum Kauf der Vollversion animieren möchten, achten Sie darauf, mithilfe des geeigneten Codes [Features in der Testversion auszuschließen oder einzuschränken](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Start- und Enddatum

Standardmäßig wird Ihre Testversion verfügbar, sobald Ihre App veröffentlicht wurde, und ihre Bereitstellung wird nie beendet. Wenn Sie möchten, können Sie das Datum und die Uhrzeit angeben, an dem bzw. zu der mit der Bereitstellung der Testversion begonnen und an dem bzw. zu der ihre Bereitstellung beendet werden soll. 

>[!NOTE]
> Diese Datumsangabe gilt nur für Kunden mit Windows 10 (inklusive Xbox). Wenn Ihre App für Kunden mit früheren Betriebssystemversionen verfügbar ist, wird die Testversion für diese Kunden angeboten, solange Ihr Produkt verfügbar ist. 

Zum Festlegen des Datums für das Anbieten der Testversion für Kunden von Windows 10 ändern Sie die Angabe im Dropdownfeld **Starts on** und/oder **Ends on** in **at** und wählen anschließend Datum und Uhrzeit. In diesem Fall können Sie entweder **UTC** auswählen, sodass die ausgewählte Uhrzeit UTC-Zeit (Universal Coordinated Time, Koordinierte Weltzeit) ist, oder **Lokal** festlegen, damit diese Uhrzeiten in jeder Zeitzone für einen Markt verwendet werden. (Beachten Sie, dass für Märkte, die mehr als eine Zeitzone umfassen, nur eine Zeitzone in diesem Markt verwendet wird. Für den USA wird die Zeitzone Eastern verwendet.) Sie können **für bestimmte Märkte anpassen** auswählen, wenn Sie für jeden Markt andere Daten festlegen möchten.


## <a name="sale-pricing"></a>Sonderangebotsverkaufspreise

Wenn Sie Ihre App zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen.

Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Organisationslizenzierung

Standardmäßig kann Ihre App Organisationen für den Volumekauf angeboten werden. Sie können angeben, ob und wie Ihre App in diesem Abschnitt angeboten werden soll.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).


## <a name="publish-date"></a>Veröffentlichungsdatum

Zuvor befand sich der Abschnitt **Veröffentlichungsdatum** auf dieser Seite. Diese Funktion befindet sich jetzt auf der Seite [Übermittlungsoptionen](manage-submission-options.md) im Abschnitt **Optionen zum Anhalten der Veröffentlichung**. (Wenn Sie entscheiden möchten, wann die App im Store veröffentlicht werden soll, empfehlen wir die Verwendung der Option [Zeitplan](configure-precise-release-scheduling.md) auf der Seite **Preise und Verfügbarkeit**.)


