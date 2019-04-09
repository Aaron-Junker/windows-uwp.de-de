---
Description: Mithilfe von Flight-Paketen können Sie Pakete an eine begrenzte Testgruppe verteilen.
title: Flight-Pakete
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, Test-Flighting
ms.localizationpriority: medium
ms.openlocfilehash: 2070d1312df8f840c2368600197ca2bd7c506cdc
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244296"
---
# <a name="package-flights"></a>Flight-Pakete

Sie können Paket Flüge verwenden, um bestimmte Pakete auf eine eingeschränkte Gruppe von Testern zu verteilen. Die Pakete, die Sie bereits in der Store veröffentlicht haben werden für andere Kunden verwendet werden, damit ihre Benutzeroberfläche wird nicht unterbrochen werden.

Paket Flüge werden nur die Pakete verschiedene; die Details von Store wird für alle Ihre Kunden identisch sein. Alle Benutzer in Ihrer Gruppe Flight erhalten die Pakete, die Sie in der Flug Paket einschließen, während Kunden nicht in der Gruppe "aktiv" sind weiterhin Ihre regulären (nicht flighted)-Paketen.  Wenn Sie sich später entscheiden, dass Sie Pakete aus einem Flug Paket an alle Kunden verfügbar machen möchten, können Sie einfach die gleichen Pakete in einer nicht-flighted Übermittlung. Beachten Sie, dass das Paket Flüge zu übergeben, müssen die [Zertifizierungsprozess](the-app-certification-process.md), ebenso wie alle.

Wenn Sie die Paket-Flüge eingerichtet haben, können Sie angeben, dass Personen, die bestimmte Pakete erhalten soll, indem Sie sie zum Hinzufügen einer **bekannte Benutzergruppe** (auch als Gruppe von Flight "bezeichnet). Benutzer in einer Test-Flight-Gruppe, die ein Gerät mit einer Windows 10-Version verwenden, die Flight-Pakete unterstützt (Windows.Desktop Build 10586 oder höher; Windows.Mobile Build 10586.63 oder höher; oder Xbox One), erhalten die Pakete aus dem für die jeweilige Gruppe festlegten Test-Flight. (Ihr Paket Flüge zählen Pakete, die für alle Betriebssystem-Zielversion, einschließlich Windows 8.1 / Windows Phone 8.1 oder früher, wenn Ihre app zuvor veröffentlichte bereits unterstützt.) Benutzer, die nicht einer Ihrer Test-Flight-Gruppen hinzugefügt wurden, oder ein Gerät verwenden, das Flight-Pakete nicht unterstützt, erhalten Pakete aus der Übermittlung ohne Test-Flight.

> [!IMPORTANT] 
> Personen in Ihren Test-Flight-Gruppen erhalten auf Desktops und mobilen Geräten die Pakete in Ihrem Test-Flight jedes Mal automatisch, wenn Sie Updates bereitstellen. **Personen in Ihren Test-Flight-Gruppen, die Xbox-Geräte verwenden, müssen jedoch manuell nach Updates suchen**, um die neuesten Pakete zu erhalten. Dabei müssen sie sicherstellen, dass sie mit ihrem Microsoft-Konto am Gerät angemeldet sind (mit der zugeordneten E-Mail-Adresse, die Sie in Ihre bekannte Anwendergruppe eingefügt haben).

Beachten Sie, dass Flight-Pakete nicht über den [Microsoft Store für Unternehmen](https://businessstore.microsoft.com/store) und den [Microsoft Store für Bildungseinrichtungen](https://educationstore.microsoft.com/store) verteilt werden. Dies liegt daran, dass die Personen in Ihren Anwndergruppen mit ihren Microsoft-Konten angemeldet sein müssen, um ein Flight-Paket zu empfangen. Alle Käufe über den Windows Store für Unternehmen und Microsoft Store für Bildungseinrichtungen empfangen Ihre Pakete, wenn es keine Test-Flight-Pakete sind.

> [!TIP]
> Flight-Pakete bieten nur den von Ihnen ausgewählten und angegebenen Kunden Pakete an. Um Pakete an eine zufällige Auswahl von Kunden zu einem bestimmten Prozentsatz zu verteilen, können Sie [schrittweise Paketrollouts](gradual-package-rollout.md) verwenden. Sie können das Rollout auch mit Ihren Flight-Paketen kombinieren, wenn Sie ein Update schrittweise auf eine Ihrer Test-Flight-Gruppen verteilen möchten.
>
> Anders als Flight-Pakete werden schrittweise Paketrollouts auf Kunden angewendet, die Ihre App über den Windows Store für Unternehmen und den Microsoft Store für Bildungseinrichtungen erwerben. 

> [!TIP]
> Sie sollten sich auch überlegen, wie die Personen in Ihrem Flight-Paket Ihnen Feedback zur App geben können. Wir empfehlen das [Hinzufügen eines Steuerelements zu Ihrer App, um den Feedback-Hub zu starten](../monetize/launch-feedback-hub-from-your-app.md). Kunden können auf diese Weise ihre Eingabe direkt bereitstellen. Prüfen Sie das Feedback anschließend im [Feedbackbericht](feedback-report.md)) der App.


## <a name="create-a-new-package-flight"></a>Erstellen eines neuen Flight-Pakets

Nachdem Sie eine Übermittlung für Ihre App veröffentlicht haben, ist auf der Seite App-Übersicht der Abschnitt **Flight-Pakete** vorhanden. Klicken Sie auf **New package flight**, um zu beginnen.

Wenn Sie noch keine bekannten Anwendergruppen eingerichtet haben, werden Sie aufgefordert, eine zu erstellen, bevor Sie fortfahren können. Weitere Informationen finden Sie unter [Erstellen neuer Benutzergruppen](create-known-user-groups.md). Sie können eine neue Gruppe von bekannten Benutzer direkt von dieser Seite durch die Auswahl **erstellen eine Flight-Gruppe** erstellen.

Sie müssen auf der Paketseite der Flight-Erstellung einen Namen für den Flight angeben und mindestens eine Test-Flight-Gruppe festlegen. Nachdem Sie dies getan haben, wählen Sie **Flight erstellen** aus. Diese Details lassen sich später nicht mehr ändern (Sie können diese Flight jedoch jederzeit löschen, ein neues Flight-Paket erstellen, wenn Sie nicht mit der Eingabe zufrieden sind).

> [!NOTE]
> Wenn Sie mehrere Flight-Pakete haben, müssen Sie jedem einen Rang zuweisen. Weitere Informationen finden Sie unter [hinzufügen und zusätzliche Rang Paket Flüge](#add-and-rank-additional-package-flights) unten.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Festlegen von Paketen zum Einfügen in Ihr Flight-Paket

Nachdem Sie die Details des Flight-Pakets gespeichert haben, wird die Übersicht dazu angezeigt. Klicken Sie auf **Pakete**, um festzulegen, welche Pakete das Test-Flight enthalten soll. Sie können Pakete, die für jede Version des Betriebssystems, die Ihre app unterstützt einschließen.

Sie haben die Option, Pakete auszuwählen, die einer vorherigen Übermittlung zugeordnet waren (entweder einer Übermittlung ohne Test-Flight oder einem Ihrer anderen Flight-Pakete, falls Sie mehrere haben). Wenn Sie neue Pakete zum Verwenden für dieses Paket Flug hochladen müssen, können Sie diese hier laden (mithilfe der [genauso wie beim app-Pakete zu einer regulären nicht flighted Übermittlung hochladen](upload-app-packages.md)). Wenn Sie alle Pakete für dieses Flight-Paket angegeben haben, klicken Sie auf **Speichern**.

Wenn Ihre App mehrere Gerätefamilien unterstützt, stellen Sie sicher, dass Sie Pakete einschließen, um den gleichen Satz von Gerätefamilien in Ihrem Test-Flight zu unterstützen. Die Mitglieder Ihrer Test-Flight-Gruppen können **nur** Pakete aus diesem Test-Flight erhalten. Sie können nicht auf Pakete aus anderen Test-Flights oder aus Ihren Übermittlungen ohne Test-Flights zugreifen. 

Beachten Sie außerdem, die Ihre Store Auflisten von Informationen und Familie geräteverfügbarkeit basiert darauf, dass die Übermittlung nicht flighted. Die Kunden in Ihren Test-Flight-Gruppen können die App nur für Gerätefamilien herunterladen, die von Ihren Übermittlungen ohne Test-Flights unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung für Gerätefamilien](#device-family-support). 


## <a name="gradual-package-rollout"></a>Schrittweiser Paketrollout

Standardmäßig werden die Pakete in Ihrer Übermittlung für alle Personen in Ihrer Test-Flight-Gruppe gleichzeitig zur Verfügung gestellt. Um dies zu ändern, können Sie das Kontrollkästchen **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows 10-Kunden)** aktivieren. Sie können einen Prozentsatz von Personen in Ihrer Test-Flight-Gruppe wählen, die die Pakete aus der neuen Übermittlung erhalten, sodass Sie das Feedback und die Analysedaten überwachen können, um sicherzustellen, dass Sie das Update zuverlässig veröffentlichen können, bevor Sie es umfassender für den Rest der Test-Flight-Gruppe bereitstellen. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung für Ihr Flight-Paket erstellen zu müssen. 

> [!IMPORTANT]
> Beim Pakete in ein Paket Flug nach und nach einführen möchten, erhalten die Personen, die nicht in den Prozentsatz enthalten sind, die die neuen Pakete Ruft die Pakete aus der vorherigen Übermittlung des Paket-Flight, (es sei denn ein Flug oder höheren Bewertung zur Verfügung).

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Optionen für das Konfigurieren zusätzlicher Flight-Pakete

Ihr Flight-Paket wird standardmäßig veröffentlicht und Ihrer Test-Flight-Gruppe zur Verfügung gestellt, sobald der Zertifizierungsprozess abgeschlossen ist. Wenn Sie, ändern möchten der [Veröffentlichungsdatum](set-app-pricing-and-availability.md#publish-date), Sie können dazu die **Flight Optionen** Abschnitt. Klicken Sie auf **Speichern**, um zur Flight-Paket-Übersicht zurückzukehren. 


## <a name="submit-your-package-flight-to-the-store"></a>Übermitteln des Flight-Paket an den Store

Wenn Sie Pakete angegeben und alle erforderlichen Optionen konfiguriert haben, klicken Sie auf **An Store übermitteln**. Ihr Flight-Paket durchläuft anschließend den [App-Zertifizierungsprozess](the-app-certification-process.md). Beachten Sie, dass Pakete in Ihrem Paket Flight erfüllen müssen, die [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)entspricht der Vorgehensweise bei der alle Übermittlungen.

Personen, die in Ihren Test-Flight-Gruppen diesem Flight-Paket zugeordnet sind und Ihre App bereits haben, erhalten jetzt ein Update mit den Paketen, die Sie in Ihrem Flight-Paket bereitgestellt haben. Wenn diese Personen Ihre App noch nicht haben, erhalten sie die Pakete aus Ihrem Flight-Paket, wenn sie es installieren. 

> [!NOTE]
> Personen, die ein Paket haben, das nur in einem Flight-Paket verfügbar ist, können die App mit Sternen bewerten und Rezensionen hinterlassen, ihre Bewertungen und Rezensionen werden für andere Kunden jedoch nicht angezeigt. (Dies schließt ältere 7.x oder 8.0 XAP-Pakete, Bewertungen und Rezensionen links von einem Mitglied Ihrer Flight-Gruppen, die diese Pakete mit anderen Kunden angezeigt werden.) Sehen Sie Bewertungen und Feedback von allen Kunden, einschließlich solcher in Ihren Gruppen Flug in der **Reviews** und **Feedback** Berichte für die app.


## <a name="device-family-support"></a>Unterstützung für Gerätefamilien

In den meisten Fällen sollten Sie Pakete bereitstellen, die den gleichen Satz von Gerätefamilien unterstützen, die auch von Ihrer Übermittlung ohne Test-Flight unterstützt werden. Die Gerätefamilienverfügbarkeit für eine App basiert stets auf der Übermittlung ohne Test-Flight, unabhängig davon, ob ein Kunde Mitglied einer Test-Flight-Gruppe ist oder nicht.

**Wenn Ihre Übermittlung ohne Test-Flight eine Gerätefamilie unterstützt, die Ihr Flight-Paket nicht unterstützt**, können die Mitglieder Ihrer Test-Flight-Gruppe die App nicht auf diese Gerätefamilie herunterladen. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise Mobilgerät- und Desktoppakete umfasst und Sie anschließend ein Flight-Paket erstellen, das nur ein Mobilgerätpaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nur auf Mobilgeräte herunterladen, auch wenn Kunden, die nicht Mitglied der Test-Flight-Gruppe sind, ein Desktoppaket herunterladen können. Auch wenn Sie das Flight-Paket nur verwenden, um Änderungen in Ihrem Mobilgerätpaket zu testen, sollten Sie das Desktoppaket aus Ihrer Übermittlung ohne Test-Flight in das Flight-Paket integrieren, damit Kunden in der Test-Flight-Gruppe Ihre App auf Desktopgeräte herunterladen können.

**Wenn Ihr Flight-Paket eine Gerätefamilie unterstützt, die Ihre Übermittlung ohne Test-Flight nicht unterstützt**, kann niemand die App auf diese Gerätefamilie herunterladen, unabhängig davon, ob der betreffende Kunde Mitglied Ihrer Test-Flight-Gruppe ist oder nicht. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise nur ein Mobilgerätpaket enthält und Sie anschließend ein Flight-Paket erstellen, das ein Mobilgerät- und ein Desktoppaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nach wie vor nur auf Mobilgeräte herunterladen. Das Desktoppaket wird niemandem angeboten, auch keinem Mitglied Ihrer Test-Flight-Gruppe. Wenn Sie Mitgliedern Ihrer Test-Flight-Gruppe ein Desktoppaket zur Verfügung stellen möchten, müssen Sie zunächst Ihre Übermittlung ohne Test-Flight aktualisieren, sodass diese ein Desktoppaket enthält. Um allen Kunden Ihrer App eine optimale Erfahrung zu bieten, sollte Ihre Übermittlung ohne Test-Flight die gleichen Gerätefamilien wie Ihr Flight-Paket unterstützen. 

> [!NOTE]
> Pakete, die Ihren Flight-Paketen hinzugefügt werden, können jede Betriebssystemversion (oder jeden Build von Windows 10) unterstützen. Wie jedoch bereits gesagt, müssen die Mitglieder von Test-Flight-Gruppen ein Gerät verwenden, auf dem eine Version von Windows 10 ausgeführt wird, die Flight-Pakete unterstützt (Windows.Desktop-Build 10586 oder höher; Windows.Mobile-Build 10586.63 oder höher), um Pakete aus dem Flight-Paket zu erhalten.


## <a name="update-or-modify-your-package-flight"></a>Aktualisieren oder Ändern Ihres Flight-Pakets

Erstellen eine neue Übermittlung für ein Flug oder Paket bereits veröffentlicht haben, klicken Sie auf **Update** neben dem Namen "aktiv" auf der Übersichtsseite Ihrer App. Anschließend können Sie genau wie bei einer Übermittlung ohne Test-Flight neue Pakete hochladen (und nicht benötigte Pakete entfernen). Nehmen Sie alle weiteren erforderlichen Änderungen vor, und klicken Sie dann auf **An Store übermitteln**, um das aktualisierte Flight-Paket an den [App-Zertifizierungsprozess](the-app-certification-process.md) zu senden.

Wenn Sie einen vorhandenen Flight ändern möchten, ohne ein neues Update zu erstellen, klicken Sie neben dem Flight-Namen auf **Modify**. Dadurch können Sie Details wie die Flight-Gruppen, den Name und den Rang ändern, ohne dass das Flight-Paket erneut zertifiziert werden muss. Beachten Sie, dass, wenn Sie ein Update ausgeführt, oder wenn Ihr Paket Flight noch nicht veröffentlicht wurde, werden Sie nicht angezeigt der **ändern** Option. 


## <a name="add-and-rank-additional-package-flights"></a>Weitere Flight-Pakete hinzufügen und nach Rang sortieren

Sie können mehrere Flight-Pakete für dieselbe App erstellen, um unterschiedliche Pakete an verschiedene Kundengruppen zu verteilen. 

Sobald Sie Ihr erstes Flight-Paket erstellt haben, können Sie mit dem oben beschriebenen Vorgang ein weiteres erstellen. Wenn Sie bereits ein Flight-Paket erstellt haben, besteht der einzige Unterschied darin, dass Sie im Abschnitt **Rang** die Reihenfolge der Priorität aller Flight-Pakete angeben müssen. Dadurch können den Store, welches Paket alle einzelnen Kunden zugewiesen werden soll, wenn sie in mehr als einer der Flug Gruppen sind bestimmen. Benutzer in den Flight-Gruppen erhalten immer das Paket mit dem höchsten Rang, das für sie verfügbar ist. Dies gilt selbst dann, wenn ein Flight-Paket mit niedrigerem Rang Pakete mit einer höheren Versionsnummer enthält.

Ihr neues Flight-Paket erhält standardmäßig den höchsten Rang. Wenn Sie den Rang ändern möchten, verschieben Sie das Paket nach unten (oder wieder nach oben), um es zwischen Ihren anderen Flight-Paketen richtig zu positionieren.

Beachten Sie, dass die Übermittlung nicht flighted immer die niedrigste (Nr. 1) eingestuft wird. Das heißt, dass Personen, die keiner Ihrer Test-Flight-Gruppen angehören, nur Pakete aus Ihrer Übermittlung ohne Test-Flight aus dem Store abrufen können. Personen in einer aktiv-Gruppe erhalten immer Pakete von der höchsten Rangfolge Paket Flug verfügbar an diese (aber nicht die nicht flighted Übermittlung, da sie den niedrigsten Rang aufweist). Dadurch sind Sie flexibel beim Bestimmen, wie Ihre Pakete an Benutzer verteilt werden, die u. U. in mehreren Ihrer Test-Flight-Gruppen Mitglied sind.

Nehmen wir beispielsweise an, dass Sie neben Ihrer regulären Übermittlung ohne Test-Flight zwei Flight-Pakete erstellen möchten: eines, das relativ stabil und für den Test mit einer breiten Zielgruppe bereit ist, und eines, bei dem Sie sich nicht so sicher sind und es auf wenige Tester beschränken möchten. Sie erstellen eine Test-Flight-Gruppe mit dem Namen „Tester“ und fügen sie einem Flight-Paket mit dem Namen „Test-Flight für Tester“ hinzu. Anschließend erstellen Sie eine Test-Flight-Gruppe mit dem Namen „Interessierte Benutzer“ und fügen sie einem anderen Flight-Paket mit dem Namen „Test-Flight für Interessierte Benutzer“ hinzu. Wenn Sie „Test-Flight für Tester“ einen höheren Rang als „Test-Flight für Interessierte Benutzer“ zuweisen, können Sie Pakete, bei denen Sie sich relativ sicher sind, in „Test-Flight für Interessierte Benutzer“ verwenden, und Pakete mit höherem Risiko, die nur für Tester vorgesehen sind, in „Test-Flight für Tester“. Mitglieder der Tester-Gruppe erhalten immer die Pakete, die Sie in „Test-Flight für Tester“ bereitstellen, auch wenn sie ebenfalls der Gruppe „Interessierte Benutzer“ angehören. (Wenn sich später dann herausstellt, dass die Pakete in „Test-Flight für Tester“ problemlos ausgeführt werden, können Sie „Test-Flight für Interessierte Benutzer“ so aktualisieren, dass die ursprünglich an „Test-Flight für Tester“ verteilten Pakete verwendet werden. Möglicherweise können Sie diese Pakete irgendwann auch in Ihrer Übermittlung ohne Test-Flight verwenden.


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Pakete aus einem Flight-Paket für alle Kunden zur Verfügung stellen

Wenn Sie eines oder mehrere Pakete in einem veröffentlichten Flight-Paket für Kunden verfügbar machen möchten, die keiner Test-Flight-Gruppe angehören, können Sie Ihre Übermittlung ohne Test-Flight entsprechend aktualisieren, ohne dieselben Pakete erneut hochladen zu müssen. 

Bei der Erstellung Ihrer neuen Übermittlung wird auf der Seite [**Packages**](upload-app-packages.md) eine Dropdownliste mit der Option zum Kopieren von Paketen aus einer vorherigen Übermittlung bzw. einem Flight-Paket angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete darin auswählen, um sie in die Übermittlung ohne Test-Flight aufzunehmen.

Beachten Sie, dass dieselben Regeln für Paketvalidierung gelten, auch wenn Pakete aus einer zuvor veröffentlichten Übermittlung verwendet werden. 


## <a name="delete-a-package-flight"></a>Löschen eines Flight-Pakets

Zum Löschen eines Flight-Pakets, das Sie nicht mehr unterstützen möchten, klicken Sie in der App-Übersicht auf dessen Namen. Klicken Sie auf der Übersichtsseite für Test-Flights **Ändern** und anschließend auf den Link **Löschen**, um das Flight-Paket zu löschen. (Wenn Sie eine nicht veröffentlichte Übermittlung des Flugs Paket ausgeführt haben, müssen Sie zuerst löschen, dieser Übermittlung). Es dauert bis zu 30 Minuten, bis dies abgeschlossen werden.

Wenn Sie ein Flight-Paket löschen, erhalten alle Kunden mit Paketen, die Sie in diesem Flight-Paket verteilt haben, ein App-Update, wenn es ein Paket mit einer höheren Versionsnummer gibt (oder sobald ein solches Paket verfügbar ist). Wenn Benutzer die App deinstallieren und später erneut installieren, wird dies als neuer Kauf behandelt, und die Benutzer erhalten die höchste aktuell verfügbare Version. 
