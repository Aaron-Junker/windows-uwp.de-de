---
Description: Legen Sie Einschränkungen auf wie Ihre app ermittelt und erworben haben, einschließlich, ob Ihre app in den Store zu finden oder finden Sie in die Auflistung überhaupt Store werden kann.
title: Sichtbarkeitsoptionen auswählen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Sichtbarkeit, private Zielgruppe, verfügbar, sichtbar
ms.localizationpriority: medium
ms.openlocfilehash: a002037e85f179e4a2dbe3dfdaf4bc3579e110e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601735"
---
# <a name="choose-visibility-options"></a>Sichtbarkeitsoptionen auswählen


Im Abschnitt **Sichtbarkeit** der [Seite „Preise und Verfügbarkeit“](set-app-pricing-and-availability.md) können Sie Einschränkungen dafür festlegen, wie die App gefunden und erworben werden kann. Dadurch haben Sie die Möglichkeit festzulegen, ob Benutzer Ihre App im Store finden können oder den Store-Eintrag überhaupt sehen können.

Es gibt zwei separate Abschnitte, in dem Abschnitt der Sichtbarkeit aus: **Zielgruppe** und **Erkennbarkeit**. 

## <a name="audience"></a>Zielgruppe

Im Abschnitt Zielgruppe können Sie angeben, ob Sie die Sichtbarkeit der Übermittlung für eine bestimmte Zielgruppe einschränken möchten, die Sie festlegen.


### <a name="public-audience"></a>Öffentliche Zielgruppe

Standardmäßig ist der Store-Eintrag Ihrer App für die **öffentlichen Zielgruppe**  sichtbar. Dies eignet sich für die meisten Übermittlungen, es sei denn, Sie möchten den Eintrag Ihrer App nur für bestimmte Benutzer einschränken. Sie können ebenfalls auf Wunsch die Optionen im Abschnitt [Auffindbarkeit](#discoverability) verwenden, um die Erkennbarkeit einzuschränken.

> [!IMPORTANT]
> Wenn Sie ein Produkt übermitteln, dessen Option als **öffentliche Zielgruppe** festgelegt ist, können Sie in einer späteren Übermittlung nicht mehr**private Zielgruppe** auswählen.


### <a name="private-audience"></a>Private Zielgruppe

Wenn der Eintrag Ihrer App nur für ausgewählte Personen, die Sie angeben, sichtbar sein soll, wählen Sie **private Zielgruppe** aus. Mit dieser Option ist die App nicht sichtbar oder nur für Kontakte in Gruppen verfügbar, die Sie angeben. Diese Option wird häufig für [Betatests](beta-testing-and-targeted-distribution.md) verwendet, das Sie damit Ihre App an Tester verteilen können, ohne dass andere Benutzer auf die App zugreifen oder diese finden im Store-Eintrag finden können (auch wenn die URL für den Store-Eintrag eingeben wurde).

Bei der Auswahl **private Zielgruppe** müssen Sie mindestens eine Gruppe von Personen angeben, die Ihre App erhalten soll. Sie können diese aus einer vorhandenen [bekannten Benutzergruppe](create-known-user-groups.md) auswählen oder Sie können **Erstellen einer neuen Gruppe** auswählen, um eine neue Gruppe festzulegen. Sie müssen die E-Mail-Adressen des Microsoft-Kontos jeder Person angeben, die Sie in die Gruppe aufnehmen möchten. Weitere Informationen finden Sie unter [Erstellen neuer Benutzergruppen](create-known-user-groups.md).

Nachdem Ihre Übermittlung veröffentlicht wurde, können die Personen in der Gruppe, die Sie angeben haben, den App Eintrag sehen und die App herunterladen, solange sie sich mit dem Microsoft-Konto angemeldet haben, das der E-Mail-Adresse zugeordnet ist, die Sie eingegeben haben, und Windows 10, Version 1607 oder höher (einschließlich Xbox One) ausführen. Personen, die nicht in Ihrer privaten Zielgruppe sind, können den App Eintrag weder anzeigen noch herunterladen, unabhängig davon, welche Betriebssystemversion sie verwenden. Sie können aktualisierte Übermittlungen an die private Zielgruppe veröffentlichen, die an Mitglieder dieser Gruppe auf die gleiche Weise wie bei einem normalen Update verteilt werden (diese stehen keinen Personen zur Verfügung, die nicht in Ihrer privaten Zielgruppe ist, es sei denn, Sie ändern die Auswahl). 

Wenn Sie die App für eine öffentliche Zielgruppe an einem bestimmten Datum und Zeitpunkt verfügbar machen möchten, können Sie das Kontrollkästchen **Veröffentlichen Sie dieses Produkt am** beim Erstellen Ihrer Übermittlung auswählen. Geben Sie Datum und Uhrzeit (in UTC) des Produkts an, das für die Öffentlichkeit zur Verfügung gestellt werden soll. Beachten Sie folgende Punkte:

- Das Datum und die Uhrzeit, die Sie auswählen, gilt für alle Märkte. Verwenden Sie dieses Kontrollkästchen nicht, wenn Sie den Zeitplan für die Version für verschiedene Märkte benutzerdefiniert anpassen möchten. Erstellen Sie stattdessen eine neue Übermittlung, um Ihre Einstellung der **öffentlichen Zielgruppe** zu ändern, und verwenden Sie dann die Optionen im [Zeitplan](configure-precise-release-scheduling.md), um die Veröffentlichungsdaten festzulegen.
- Das Eingeben eines Datums für **Veröffentlichen Sie dieses Produkt am** gilt nicht für den Microsoft Store für Unternehmen bzw. für den Microsoft Store für Bildungseinrichtungen. Um Ihre App für diese Kunden über die Unternehmenslizenzierung anbieten zu können, müssen Sie eine neue Übermittlung mithilfe der Auswahl **öffentliche Zielgruppe** festlegen (wobei [Unternehmenslizenzierung](organizational-licensing.md) aktiviert ist).
- Nach der Auswahl von Datum und Uhrzeit, verwenden alle zukünftigen Übermittlungen die Option **öffentlichen Zielgruppe**.

Wenn Sie kein Datum und keine Uhrzeit angeben, an denen Ihre App für die öffentliche Zielgruppe zur Verfügung gestellt wird, können Sie dies später tun, indem Sie eine neue Übermittlung erstellen und Ihre Zielgruppe von **private Zielgruppe** auf **öffentliche Zielgruppe** festlegen. Wenn Sie dies tun, sollten Sie bedenken, das Ihre App möglicherweise einen zusätzlichen Zertifizierungsprozess durchlaufen muss. Sie sollten darauf vorbereitet sein, alle neuen Zertifizierungsprobleme zu beheben, die auftreten können. 

Hier sind einige wichtige Aspekte, die Sie beachten sollten, wenn Ihre App an eine private Zielgruppe vertrieben wird:
- Personen in Ihrer privaten Zielgruppe sind in der Lage, sich die App über ein direktes Link zum Store-Eintrag Ihrer App zu holen, wobei sie sich mit ihrem Microsoft-Konto anmelden müssen, um die App anzuzeigen. Dieser Link wird bereitgestellt, wenn Sie **private Zielgruppe** auswählen. Sie finden es auch auf der Seite [App-Identität](view-app-identity-details.md) unter **URL, wenn die App nur für bestimmte Personen sichtbar ist (erfordert Authentifizierung)**. Stellen Sie sicher dass Ihre Tester diesen Link erhalten, und nicht die reguläre URL für Ihren Store-Eintrag.  
- Wenn Sie nicht die Option unter **Auffindbarkeit** auswählen, wodurch dies verhindert wird, sind Personen Ihrer privaten Zielgruppen in der Lage, Ihre App zu finden, indem Sie diese unter Microsoft Store-App suchen. Der Webeintrag ist allerdings nicht über die Suche auffindbar, auch nicht für Personen in dieser Gruppe. 
- Sie können nicht das [Datum der Veröffentlichung im Abschnitt Zeitplan konfigurieren](configure-precise-release-scheduling.md) auf der **Seite „Preise und Verfügbarkeit“**, da Ihre App nicht für Kunden außerhalb Ihrer privaten Zielgruppe veröffentlicht wird.
- Jede andere vorgenommene Auswahl gilt für Personen in dieser Zielgruppe. Wenn Sie beispielsweise einen anderen Preis als **Kostenlos** auswählen, müssen Personen in Ihrer privaten Zielgruppe den Preis zahlen, um die App zu erwerben. 
- Wenn Sie unterschiedliche Pakete an andere Personen in Ihrer privaten Zielgruppe verteilen möchten, können Sie nach der ersten Übermittlung [Flight-Pakete](package-flights.md) verwenden, um andere Paketupdates an einen Teil Ihrer privaten Zielgruppe zu verteilen. Sie können weitere bekannte Benutzergruppen erstellen, die bestimmen, wer ein bestimmtes Flight-Paket erhalten soll.
- Sie können die Mitgliedschaft der bekannten Benutzergruppe(n) in Ihrer privaten Zielgruppe bearbeiten. Denken Sie beim Entfernen einer Person jedoch daran, dass Sie, wenn Sie eine Person entfernen, die Ihre App bereits heruntergeladen hat und die vorher in der Gruppe war, sie immer noch in der Lage ist, die App zu verwenden, auch wenn sie keine Updates mehr erhält (es sei denn, Sie wählen **öffentliche Zielgruppe** zu einem späteren Zeitpunkt aus).
- Ihre App ist unabhängig von den Lizenzierungseinstellungen Ihrer Organisation nicht über den Microsoft Store für Unternehmen und/oder über den Microsoft Store für Bildungseinrichtungen verfügbar, auch nicht für Personen in Ihrer privaten Zielgruppe.
- Während der Store garantiert, dass Ihre App nur für Personen mit einem Microsoft-Konto sichtbar ist, die Sie Ihrer privaten Zielgruppe hinzugefügt haben, kann nicht verhindert werden, dass diese Benutzer Informationen oder Screenshots außerhalb Ihrer privaten Zielgruppe freigeben. Wenn Ihnen Vertraulichkeit wichtig ist, achten Sie darauf, dass Ihre private Zielgruppe nur Personen enthält, denen Sie vertrauen können, dass Sie keine Details zu Ihrer App mit anderen teilen.
- Stellen Sie sicher, dass Ihre Tester wissen, wie Sie Ihnen Feedback erteilen können. Sie möchten wahrscheinlich nicht, dass Feedback im Feedback-Hub gegeben wird, da jeder andere Kunde ihr Feedback sehen kann. Erwägen Sie einen Link für Feedback via E-Mail oder auf andere Weise.
- Alle Rezensionen von Personen in Ihrer privaten Zielgruppe stehen zur Anzeige zur Verfügung. Allerdings sind diese Rezensionen nicht im App Store-Eintrag sichtbar, auch nachdem Ihre Übermittlung als **öffentliche Zielgruppe** festgelegt wird. Erhalten Sie Berichte, die von Ihrer privaten Zielgruppe geschrieben werden, anhand der [überprüft der Berichts](reviews-report.md), jedoch nicht diese Daten herunterladen oder verwenden Sie die [Microsoft Store-Textanalyse-API](../monetize/access-analytics-data-using-windows-store-services.md) programmgesteuert auf diese Berichte zugreifen.
- Wenn Sie die App von **Private Zielgruppe** auf **Öffentliche Zielgruppe** ändern, ist das angezeigte **Veröffentlichungsdatum** im Store-Eintrag das Datum der ersten Veröffentlichung an die öffentliche Zielgruppe.

## <a name="discoverability"></a>Erkennbarkeit

Die Auswahl im Abschnitt **Auffindbarkeit** gibt an, wie Kunden Ihre App finden und erwerben können. 

> [!IMPORTANT]
> Wenn Sie die Option **private Zielgruppe**wählen, gilt die Auswahl der **Auffindbarkeit** nur für Personen in Ihrer privaten Zielgruppe. Kunden, die nicht in den angegebenen Gruppen sind, können die App nicht erwerben oder den Eintrag sehen. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Dieses Produkt im Store verfügbar und sichtbar machen

Dies ist die Standardoption. Lassen Sie diese Option ausgewählt, wenn Sie Ihre app in der Store für Kunden, Suchen über direkten Link der app und/oder die anderen Methoden, einschließlich suchen, durchsuchen und Aufnahme in zusammengestellten Listen aufgeführt werden soll. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Dieses Produkt im Store verfügbar aber nicht sichtbar machen

Wenn Sie diese Option auswählen, kann Ihre App nicht von Kunden im Store durch Browsen gefunden werden. Die einzige Möglichkeit, Ihre App abzurufen ist durch einen direkten Link. 

> [!TIP]
> Wenn der Eintrag auch mit einem direkten Link nicht für die Öffentlichkeit verfügbar gemacht werden soll, wählen Sie **private Zielgruppe** im Abschnitt **Zielgruppe** aus, wie oben beschrieben.

Sie müssen auch eine der folgenden Optionen wählen, um zu bestimmen, wie Ihre App erworben werden kann:


>[!IMPORTANT]
> Diese Optionen beschränken die Betriebssystemversionen, unter denen Kunden Ihre App erwerben können. Lesen Sie die Beschreibungen sorgfältig, um sicherzustellen, dass Sie wissen, welche Betriebssystemversionen unterstützt werden. 

- **Direkter Link: Jeder Kunde mit einem direkten Link zu den produktanforderungen Angebot können Sie herunterladen, außer für Windows 8.x.** Jeder Kunde, der zum Eintrag Ihrer App über einen direkten Link gelangt, kann sie auf Geräten unter Windows 10 oder Windows Phone 8.1 und früher (aber nicht auf Geräten mit Windows 8.x) herunterladen.
- **Beenden Sie die Übernahme: Jeder Kunde mit einem direkten Link sehen des Produkts Store auflisten, aber sie können nur heruntergeladen, wenn sie im Besitz des Produkts vor, oder haben einen Angebotscode und Windows 10-Gerät verwenden.** Selbst wenn ein Kunde einen direkten Link besitzt, kann er die App nur herunterladen, wenn er einen [Werbecode](generate-promotional-codes.md) besitzt und ein Windows 10-Gerät verwendet. Wenn ein Kunde einen Werbecode hat, kann er den Code verwenden, um Ihre App kostenlos (nur unter Windows 10) zu erhalten, obwohl Sie sie nicht für andere Kunden anbieten. Abgesehen von der Verwendung eines Werbecodes gibt es keine Möglichkeit für andere Benutzer, Ihre App zu erhalten.

> [!TIP]
> Wenn Sie eine App überhaupt nicht mehr für neue Kunden anbieten möchten, klicken Sie in der Übersicht auf **Make app unavailable**. Nachdem Sie bestätigt haben, dass die App nicht mehr verfügbar sein soll, wird sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und neue Kunden haben keine Möglichkeit, sie herunterzuladen (es sei denn, Sie verfügen über einen [Werbecode](generate-promotional-codes.md) und verfügen über ein Windows 10-Gerät). Diese Aktion überschreibt die Auswahl **Sichtbarkeit** in Ihrer Übermittlung. Falls Sie die App für neue Kunden wieder verfügbar machen möchten (über die Auswahl **Sichtbarkeit**), können Sie jederzeit in der App-Übersicht auf **Make app available** klicken. Weitere Informationen finden Sie unter [Entfernen einer App aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




