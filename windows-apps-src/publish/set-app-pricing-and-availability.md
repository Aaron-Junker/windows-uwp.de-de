---
Description: Auf der Seite „Preise und Verfügbarkeit“ des App-Übermittlungsprozesses können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist.
title: Festlegen der Preise und Verfügbarkeit von Apps
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Price, available, erkennbare, kostenlose Testversion, Testversionen, Testversion, apps, Veröffentlichungsdatum
ms.localizationpriority: medium
ms.openlocfilehash: 9956463471b310835aedf517817878d526cc810d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164104"
---
# <a name="set-app-pricing-and-availability"></a>Festlegen der Preise und Verfügbarkeit von Apps


Auf der Seite **Preise und Verfügbarkeit** des [App-Übermittlungsprozesses](app-submissions.md) können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist. Wir stellen Ihnen hier die auf dieser Seite verfügbaren Optionen vor und informieren Sie darüber, was Sie bei der Eingabe dieser Informationen beachten sollten.


## <a name="markets"></a>Märkte

Der Microsoft Store erreicht Kunden in über 240 Ländern und Regionen weltweit. Standardmäßig wird Ihre APP in allen möglichen Märkten angeboten. Wenn Sie möchten, können Sie die bestimmten Märkte auswählen, in denen Sie Ihre APP bereitstellen möchten. 

Weitere Informationen finden Sie unter [Definieren der Markt Auswahl](./define-market-selection.md).


## <a name="visibility"></a>Sicht

Im Bereich " **Sichtbarkeit** " können Sie Einschränkungen festlegen, wie Ihre APP ermittelt und abgerufen werden kann. Dies umfasst auch, ob die Benutzer Ihre APP im Store finden oder Ihre Store-Liste anzeigen können.

Weitere Informationen finden Sie unter [Sichtbarkeitsoptionen auswählen](choose-visibility-options.md).


## <a name="schedule"></a>Zeitplan

Standardmäßig (es sei denn, Sie haben eine der Optionen **diese app verfügbar, aber nicht in den Store** -Optionen im Bereich " [Sichtbarkeit](choose-visibility-options.md#discoverability) " ausgewählt). Ihre APP steht Kunden sofort zur Verfügung, sobald Sie die Zertifizierungsstelle übergibt und den Veröffentlichungsprozess abzuschließen. Um andere Datumsangaben auszuwählen, wählen Sie **Optionen anzeigen** aus, um diesen Abschnitt zu erweitern. 

Weitere Informationen finden Sie unter [Konfigurieren der genauen Releaseplanung](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Preise

Sie müssen einen Basispreis für Ihre App auswählen (es sei denn, Sie **haben im Bereich** " [Sichtbarkeit](choose-visibility-options.md#discoverability) " **diese app verfügbar machen, aber nicht auffindbar** ist), wählen Sie entweder **Free** oder einen der verfügbaren Preisstufen aus. Sie können auch Preisänderungen planen, um das Datum und die Uhrzeit anzugeben, zu denen sich der Preis Ihrer APP ändern sollte. Außerdem haben Sie die Möglichkeit, diese Änderungen für bestimmte Märkte anzupassen. Microsoft aktualisiert regelmäßig die empfohlenen Preise, um Währungsschwankungen in verschiedenen Märkten zu berücksichtigen. Wenn ein empfohlener Preis geändert wird, zeigt der Preisbereich einen Warn Indikator an, wenn die von Ihnen ausgewählten Preise nicht den neuen empfohlenen Werten entsprechen. Die Preise in ihren Produkten werden nicht geändert. Sie können steuern, wann und ob Sie diese Preise aktualisieren möchten. 

Weitere Informationen finden Sie unter [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Kostenlose Testversion

Viele Entwickler bieten Kunden die Möglichkeit, ihre App mithilfe der im Store verfügbaren Testfunktionen kostenlos auszuprobieren. Standardmäßig ist **keine kostenlose Testversion** ausgewählt, und es wird keine Testversion für Ihre APP erstellt. Wenn Sie eine Testversion anbieten möchten, können Sie einen Wert aus der Dropdown Liste für die **Kostenlose Testversion** auswählen.

Es gibt zwei Arten von Testversionen, die Sie auswählen können, und Sie haben die Möglichkeit, das Datum und die Uhrzeit zu konfigurieren, zu denen die Testversion beginnen und nicht mehr angeboten werden soll.

### <a name="time-limited"></a>Zeitlich begrenzt

Wählen **Sie Zeit-beschränkt** aus, damit Kunden Ihre APP für eine bestimmte Anzahl von Tagen kostenlos testen können: **1 Tag**, **7 Tage**, **15 Tage**oder **30 Tage**. Sie können Features einschränken, indem Sie Code hinzufügen, um [Features in der Testversion auszuschließen oder einzuschränken](../monetize/in-app-purchases-and-trials.md), oder Sie können Kunden während dieses Zeitraums auf die gesamte Funktionalität zugreifen. 
> [!NOTE]
> Zeitlich begrenzte Testversionen werden Kunden unter Windows 10 Build 10.0.10586 oder früher oder Kunden auf Windows Phone 8,1 und früher nicht angezeigt.

### <a name="unlimited"></a>Unbegrenzt

Wählen Sie **unbegrenzt** aus, damit Kunden unbegrenzt auf Ihre App zugreifen können. Da Sie Ihre Kunden zum Kauf der Vollversion animieren möchten, achten Sie darauf, mithilfe des geeigneten Codes [Features in der Testversion auszuschließen oder einzuschränken](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Anfangs- und Enddatum

Standardmäßig ist Ihre Testversion verfügbar, sobald Ihre App veröffentlicht wurde, und Sie wird nicht mehr angeboten. Wenn Sie möchten, können Sie das Datum und die Uhrzeit angeben, zu denen die Testversion gestartet werden soll, und wann Sie nicht mehr angeboten werden soll. 

>[!NOTE]
> Diese Datumsangaben gelten nur für Kunden unter Windows 10 (einschließlich Xbox). Wenn Ihre APP für Kunden unter früheren Betriebssystemversionen verfügbar ist, wird die Testversion für diese Kunden angeboten, solange Ihr Produkt verfügbar ist. 

Zum Festlegen von Datumsangaben für den **Fall, dass**Ihre Testversion für Kunden unter Windows 10 angeboten werden soll, ändern Sie die Dropdown Liste **starts on** und/oder End **on** in, und wählen Sie dann das Datum und die Uhrzeit aus. Wenn Sie dies tun, können Sie entweder **UTC** auswählen, damit die ausgewählte Zeit (UTC-Zeit) als universelle koordinierte Zeit verwendet wird, oder wählen Sie **lokal** aus, damit diese Zeiten in jeder einem Markt zugeordneten Zeitzone verwendet werden. (Beachten Sie, dass für Märkte, die mehr als eine Zeitzone enthalten, nur eine Zeitzone in diesem Markt verwendet wird. Für den USA wird die Zeitzone Eastern verwendet.) Sie können **für bestimmte Märkte anpassen** auswählen, wenn Sie für jeden Markt andere Daten festlegen möchten.


## <a name="sale-pricing"></a>Sonderangebotsverkaufspreise

Wenn Sie Ihre App zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen.

Weitere Informationen finden Sie unter [Anbieten von Apps und Add-Ons](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Organisationslizenzierung

Standardmäßig kann Ihre App Organisationen für den Volumekauf angeboten werden. Sie können angeben, ob und wie Ihre App in diesem Abschnitt angeboten werden soll.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).


## <a name="publish-date"></a>Veröffentlichungsdatum

Zuvor wurde der Abschnitt **Veröffentlichungsdatum** auf dieser Seite angezeigt. Diese Funktionalität finden Sie im Abschnitt Optionen für die **Veröffentlichungs** Speicherung auf der Seite Übermittlungs [Optionen](manage-submission-options.md) . (Beachten Sie, dass Sie die Verwendung des [Zeitplans](configure-precise-release-scheduling.md) -Abschnitts der Seite " **Preise und Verfügbarkeit** " empfehlen, um zu steuern, wann Ihre APP im Store veröffentlicht werden soll.)