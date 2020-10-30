---
description: Mithilfe von Flight-Paketen können Sie Pakete an eine begrenzte Testgruppe verteilen.
title: Flight-Pakete
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, flighting
ms.localizationpriority: medium
ms.openlocfilehash: e86fe69a0fa95ade82c5ed55acff907667b08f0f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029968"
---
# <a name="package-flights"></a>Flight-Pakete

Sie können mit Paket Flügen bestimmte Pakete an eine begrenzte Gruppe von Testern verteilen. Die Pakete, die Sie bereits im Speicher veröffentlicht haben, werden für Ihre anderen Kunden verwendet, sodass Ihre Benutzererfahrung nicht beeinträchtigt wird.

Bei Paket Flügen unterscheiden sich nur die Pakete. die Details der Store-Auflistung werden für alle Ihre Kunden identisch sein. Jeder Benutzer in der Fluggruppe erhält die Pakete, die Sie in den paketflug einschließen, während Kunden, die nicht in der Fluggruppe sind, weiterhin ihre regulären (nicht geflitzten) Pakete erhalten.  Wenn Sie später entscheiden, dass Sie Pakete aus einem paketflight für alle Kunden verfügbar machen möchten, können Sie diese Pakete problemlos in einer nicht geflitzten Übermittlung verwenden. Beachten Sie, dass die Paket Flüge den [Zertifizierungsprozess](the-app-certification-process.md)durchlaufen müssen, genauso wie bei jeder Übermittlung.

Beim Einrichten von Paket Flügen können Sie angeben, welche Personen bestimmte Pakete erhalten sollen, indem Sie Sie zu einer **bekannten Benutzergruppe** hinzufügen (manchmal auch als "Flight Group" bezeichnet). Benutzer in einer Test-Flight-Gruppe, die ein Gerät mit einer Windows 10-Version verwenden, die Flight-Pakete unterstützt (Windows.Desktop Build 10586 oder höher; Windows.Mobile Build 10586.63 oder höher; oder Xbox One), erhalten die Pakete aus dem für die jeweilige Gruppe festlegten Test-Flight. (Die Paket Flüge können Pakete enthalten, die auf eine beliebige Betriebssystemversion abzielen, einschließlich Windows 8.1/Windows Phone 8,1 oder früher, wenn Ihre zuvor veröffentlichte app Sie bereits unterstützt.) Jeder, der nicht zu einer ihrer fluggruppen hinzugefügt wurde oder ein Gerät verwendet, das keine Paket-Flüge unterstützt, erhält Pakete von der nicht geflitzten Übermittlung.

> [!IMPORTANT] 
> Auf Desktop Computern und mobilen Geräten werden die Pakete in ihren fluggruppen automatisch bei jedem Update bereitgestellt. **Personen in ihren fluggruppen, die Xbox-Geräte verwenden, müssen jedoch manuell nach Updates suchen** , um die neuesten Pakete zu erhalten. Stellen Sie sicher, dass Sie über Ihre Microsoft-Konto (mit der zugehörigen e-Mail-Adresse, die Sie in ihrer bekannten Benutzergruppe eingeschlossen haben) an Ihrem Gerät angemeldet sind.

Beachten Sie, dass die Paket Flüge nicht über [Microsoft Store für Unternehmen](https://businessstore.microsoft.com/store) und [Microsoft Store für Bildungseinrichtungen](https://educationstore.microsoft.com/store)verteilt werden. Dies liegt daran, dass Personen in ihren bekannten Benutzergruppen mit Ihren Microsoft-Konten angemeldet sein müssen, um einen paketflug zu erhalten. Alle über Microsoft Store für Unternehmen oder Microsoft Store für Bildungseinrichtungen getätigten Akquisitionen werden Ihre nicht geflitzten Pakete erhalten.

> [!TIP]
> Bei Paket Flügen werden nur die von Ihnen angegebenen ausgewählten Kunden Pakete bereitstellen. Um Pakete an eine zufällige Auswahl von Kunden zu einem bestimmten Prozentsatz zu verteilen, können Sie [schrittweise Paketrollouts](gradual-package-rollout.md) verwenden. Sie können das Rollout auch mit Ihren Flight-Paketen kombinieren, wenn Sie ein Update schrittweise auf eine Ihrer Test-Flight-Gruppen verteilen möchten.
>
> Im Unterschied zu Paket Flügen gelten für Kunden, die Ihre APP über Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen erwerben, die schrittweise Auswahl für das Rollout des Pakets. 

> [!TIP]
> Berücksichtigen Sie, wie die Personen in Ihrem paketflight Ihre Eingaben über die APP erhalten können. Wir empfehlen das [Hinzufügen eines Steuerelements zu Ihrer App, um den Feedback-Hub zu starten](../monetize/launch-feedback-hub-from-your-app.md). Kunden können auf diese Weise ihre Eingabe direkt bereitstellen. Prüfen Sie das Feedback anschließend im [Feedbackbericht](feedback-report.md)) der App.


## <a name="create-a-new-package-flight"></a>Erstellen eines neuen Flight-Pakets

Nachdem Sie eine Übermittlung für Ihre App veröffentlicht haben, ist auf der Seite App-Übersicht der Abschnitt **Flight-Pakete** vorhanden. Klicken Sie auf **New package flight** , um zu beginnen.

Wenn Sie noch keine bekannten Benutzergruppen erstellt haben, werden Sie aufgefordert, eine Gruppe zu erstellen, bevor Sie den Vorgang fortsetzen können. Weitere Informationen finden Sie unter [Erstellen bekannter Benutzergruppen](create-known-user-groups.md). Sie können eine neue Gruppe bekannter Benutzer direkt auf dieser Seite erstellen, indem Sie die Option **Fluggruppe erstellen** auswählen.

Auf der Seite paketflight-Erstellung müssen Sie einen Namen für den Flug eingeben und mindestens eine Fluggruppe angeben. Nachdem Sie dies getan haben, wählen Sie **Flight erstellen** aus. Sie sind nicht in der Lage, diese Details zu einem späteren Zeitpunkt zu ändern (wenn Sie jedoch nicht zufrieden sind, was Sie eingegeben haben, können Sie diesen Flug löschen und einen neuen erstellen, der stattdessen verwendet wird).

> [!NOTE]
> Wenn Sie über mehrere paketflüge verfügen, müssen Sie jedem einen Rang zuweisen. Weitere Informationen finden Sie unter [Hinzufügen und Rangfolge zusätzlicher paketflüge](#add-and-rank-additional-package-flights) weiter unten.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Festlegen von Paketen zum Einfügen in Ihr Flight-Paket

Nachdem Sie die Details des Flight-Pakets gespeichert haben, wird die Übersicht dazu angezeigt. Klicken Sie auf **Pakete** , um festzulegen, welche Pakete das Test-Flight enthalten soll. Sie können Pakete für alle Betriebssystemversionen einschließen, die von ihrer App unterstützt werden.

Sie haben die Option, Pakete auszuwählen, die einer vorherigen Übermittlung zugeordnet waren (entweder einer Übermittlung ohne Test-Flight oder einem Ihrer anderen Flight-Pakete, falls Sie mehrere haben). Wenn Sie neue Pakete für dieses paketflight hochladen müssen, können Sie Sie hier hochladen (mit dem [gleichen Prozess wie beim Hochladen von App-Paketen in eine reguläre nicht gefliute Übermittlung](upload-app-packages.md)). Wenn Sie alle Pakete für dieses Flight-Paket angegeben haben, klicken Sie auf **Speichern** .

Wenn Ihre App mehrere Gerätefamilien unterstützt, stellen Sie sicher, dass Sie Pakete einschließen, um den gleichen Satz von Gerätefamilien in Ihrem Test-Flight zu unterstützen. Die Mitglieder Ihrer Test-Flight-Gruppen können **nur** Pakete aus diesem Test-Flight erhalten. Sie können nicht auf Pakete aus anderen Test-Flights oder aus Ihren Übermittlungen ohne Test-Flights zugreifen. 

Beachten Sie auch, dass Ihre Speicher Auflistungs Informationen und die Verfügbarkeit der Gerätefamilie auf ihrer nicht geflitzten Übermittlung basieren. Die Kunden in Ihren Test-Flight-Gruppen können die App nur für Gerätefamilien herunterladen, die von Ihren Übermittlungen ohne Test-Flights unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung für Gerätefamilien](#device-family-support). 


## <a name="gradual-package-rollout"></a>Schrittweiser Paketrollout

Standardmäßig werden die Pakete in Ihrer Übermittlung für alle Personen in Ihrer Test-Flight-Gruppe gleichzeitig zur Verfügung gestellt. Um dies zu ändern, können Sie das Kontrollkästchen **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows 10-Kunden)** aktivieren. Sie können einen Prozentsatz von Personen in Ihrer Test-Flight-Gruppe wählen, die die Pakete aus der neuen Übermittlung erhalten, sodass Sie das Feedback und die Analysedaten überwachen können, um sicherzustellen, dass Sie das Update zuverlässig veröffentlichen können, bevor Sie es umfassender für den Rest der Test-Flight-Gruppe bereitstellen. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung für Ihr Flight-Paket erstellen zu müssen. 

> [!IMPORTANT]
> Bei der schrittweisen Bereitstellung von Paketen in einem paketflug erhalten die Personen, die nicht in dem Prozentsatz enthalten sind, der die neuen Pakete abruft, die Pakete aus der vorherigen Package Flight-Übermittlung (es sei denn, es ist ein Flug mit höherer Rangfolge verfügbar).

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Optionen für das Konfigurieren zusätzlicher Flight-Pakete

Ihr Flight-Paket wird standardmäßig veröffentlicht und Ihrer Test-Flight-Gruppe zur Verfügung gestellt, sobald der Zertifizierungsprozess abgeschlossen ist. Wenn Sie das [Veröffentlichungsdatum](set-app-pricing-and-availability.md#publish-date)ändern möchten, können Sie dies im Abschnitt **Flight Options (Flugoptionen** ) tun. Klicken Sie auf **Speichern** , um zur Flight-Paket-Übersicht zurückzukehren. 


## <a name="submit-your-package-flight-to-the-store"></a>Übermitteln des Flight-Paket an den Store

Wenn Sie Pakete angegeben und alle erforderlichen Optionen konfiguriert haben, klicken Sie auf **An Store übermitteln** . Ihr Flight-Paket durchläuft anschließend den [App-Zertifizierungsprozess](the-app-certification-process.md). Beachten Sie, dass Pakete, die in Ihrem paketflight enthalten sind, den [Microsoft Store Richtlinien](store-policies.md)entsprechen müssen, wie bei allen Übermittlungen.

Personen, die in Ihren Test-Flight-Gruppen diesem Flight-Paket zugeordnet sind und Ihre App bereits haben, erhalten jetzt ein Update mit den Paketen, die Sie in Ihrem Flight-Paket bereitgestellt haben. Wenn diese Personen Ihre App noch nicht haben, erhalten sie die Pakete aus Ihrem Flight-Paket, wenn sie es installieren. 

> [!NOTE]
> Personen, die über ein Paket verfügen, das nur in einem paketflug verfügbar ist, können der App eine Stern Bewertung geben und Überprüfungen hinterlassen, aber ihre Bewertungen und Kritiken werden anderen Kunden nicht angezeigt. (Dies schließt ältere 7. x-oder 8,0 XAP-Pakete aus; Bewertungen und Überprüfungen, die von Mitgliedern ihrer fluggruppen mit diesen Paketen verbleiben, werden für andere Kunden sichtbar sein.) Sie können Bewertungen und Feedback von allen Kunden, einschließlich der in ihren fluggruppen, in den **Reviews** und **Feedback** Berichten für die APP einsehen.


## <a name="device-family-support"></a>Unterstützung für Gerätefamilien

In den meisten Fällen sollten Sie Pakete bereitstellen, die den gleichen Satz von Gerätefamilien unterstützen, die auch von Ihrer Übermittlung ohne Test-Flight unterstützt werden. Die Gerätefamilienverfügbarkeit für eine App basiert stets auf der Übermittlung ohne Test-Flight, unabhängig davon, ob ein Kunde Mitglied einer Test-Flight-Gruppe ist oder nicht.

**Wenn Ihre Übermittlung ohne Test-Flight eine Gerätefamilie unterstützt, die Ihr Flight-Paket nicht unterstützt** , können die Mitglieder Ihrer Test-Flight-Gruppe die App nicht auf diese Gerätefamilie herunterladen. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise Mobilgerät- und Desktoppakete umfasst und Sie anschließend ein Flight-Paket erstellen, das nur ein Mobilgerätpaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nur auf Mobilgeräte herunterladen, auch wenn Kunden, die nicht Mitglied der Test-Flight-Gruppe sind, ein Desktoppaket herunterladen können. Auch wenn Sie das Flight-Paket nur verwenden, um Änderungen in Ihrem Mobilgerätpaket zu testen, sollten Sie das Desktoppaket aus Ihrer Übermittlung ohne Test-Flight in das Flight-Paket integrieren, damit Kunden in der Test-Flight-Gruppe Ihre App auf Desktopgeräte herunterladen können.

**Wenn Ihr Flight-Paket eine Gerätefamilie unterstützt, die Ihre Übermittlung ohne Test-Flight nicht unterstützt** , kann niemand die App auf diese Gerätefamilie herunterladen, unabhängig davon, ob der betreffende Kunde Mitglied Ihrer Test-Flight-Gruppe ist oder nicht. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise nur ein Mobilgerätpaket enthält und Sie anschließend ein Flight-Paket erstellen, das ein Mobilgerät- und ein Desktoppaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nach wie vor nur auf Mobilgeräte herunterladen. Das Desktoppaket wird niemandem angeboten, auch keinem Mitglied Ihrer Test-Flight-Gruppe. Wenn Sie Mitgliedern Ihrer Test-Flight-Gruppe ein Desktoppaket zur Verfügung stellen möchten, müssen Sie zunächst Ihre Übermittlung ohne Test-Flight aktualisieren, sodass diese ein Desktoppaket enthält. Um allen Kunden Ihrer App eine optimale Erfahrung zu bieten, sollte Ihre Übermittlung ohne Test-Flight die gleichen Gerätefamilien wie Ihr Flight-Paket unterstützen. 

> [!NOTE]
> Pakete, die zu ihren Paket Flügen hinzugefügt werden, können alle Betriebssystemversionen (oder einen beliebigen Build von Windows 10) unterstützen, aber wie oben erwähnt, müssen Personen in fluggruppen ein Gerät verwenden, auf dem eine Version von Windows 10 ausgeführt wird, die Paket Flüge unterstützt (Windows. Desktop Build 10586 oder höher). Windows. Mobile Build 10586,63 oder höher), um Pakete aus dem paketflug zu erhalten.


## <a name="update-or-modify-your-package-flight"></a>Aktualisieren oder Ändern Ihres Flight-Pakets

Zum Erstellen einer neuen Übermittlung für einen paketflug, den Sie bereits veröffentlicht haben, klicken Sie auf der Seite "Übersicht" der APP auf **Update** neben dem Flug Namen. Anschließend können Sie genau wie bei einer Übermittlung ohne Test-Flight neue Pakete hochladen (und nicht benötigte Pakete entfernen). Nehmen Sie alle weiteren erforderlichen Änderungen vor, und klicken Sie dann auf **An Store übermitteln** , um das aktualisierte Flight-Paket an den [App-Zertifizierungsprozess](the-app-certification-process.md) zu senden.

Wenn Sie einen vorhandenen Flight ändern möchten, ohne ein neues Update zu erstellen, klicken Sie neben dem Flight-Namen auf **Modify** . Dadurch können Sie Details wie die Flight-Gruppen, den Name und den Rang ändern, ohne dass das Flight-Paket erneut zertifiziert werden muss. Beachten Sie, dass die Option **ändern** nicht angezeigt wird, wenn ein Update in Bearbeitung ist oder wenn Ihr paketflug noch nicht veröffentlicht wurde. 


## <a name="add-and-rank-additional-package-flights"></a>Weitere Flight-Pakete hinzufügen und nach Rang sortieren

Sie können mehrere Flight-Pakete für dieselbe App erstellen, um unterschiedliche Pakete an verschiedene Kundengruppen zu verteilen. 

Sobald Sie Ihr erstes Flight-Paket erstellt haben, können Sie mit dem oben beschriebenen Vorgang ein weiteres erstellen. Wenn Sie bereits ein Flight-Paket erstellt haben, besteht der einzige Unterschied darin, dass Sie im Abschnitt **Rang** die Reihenfolge der Priorität aller Flight-Pakete angeben müssen. Dadurch kann der Speicher bestimmen, welches Paket einem einzelnen Kunden zugewiesen werden soll, wenn Sie in mehr als einer ihrer fluggruppen vorhanden sind. Benutzer in den Flight-Gruppen erhalten immer das Paket mit dem höchsten Rang, das für sie verfügbar ist. Dies gilt selbst dann, wenn ein Flight-Paket mit niedrigerem Rang Pakete mit einer höheren Versionsnummer enthält.

Ihr neues Flight-Paket erhält standardmäßig den höchsten Rang. Wenn Sie den Rang ändern möchten, verschieben Sie das Paket nach unten (oder wieder nach oben), um es zwischen Ihren anderen Flight-Paketen richtig zu positionieren.

Beachten Sie, dass die nicht geflistete Übermittlung immer den niedrigsten (#1) Rang hat. Das heißt, dass Personen, die keiner Ihrer Test-Flight-Gruppen angehören, nur Pakete aus Ihrer Übermittlung ohne Test-Flight aus dem Store abrufen können. Personen in einer Fluggruppe erhalten immer Pakete vom höchsten paketflight, die Ihnen zur Verfügung stehen (aber nie die nicht geflistete Übermittlung, da Sie den niedrigsten Rang aufweist). Dadurch sind Sie flexibel beim Bestimmen, wie Ihre Pakete an Benutzer verteilt werden, die u. U. in mehreren Ihrer Test-Flight-Gruppen Mitglied sind.

Nehmen wir beispielsweise an, dass Sie neben Ihrer regulären Übermittlung ohne Test-Flight zwei Flight-Pakete erstellen möchten: eines, das relativ stabil und für den Test mit einer breiten Zielgruppe bereit ist, und eines, bei dem Sie sich nicht so sicher sind und es auf wenige Tester beschränken möchten. Sie erstellen eine Test-Flight-Gruppe mit dem Namen „Tester“ und fügen sie einem Flight-Paket mit dem Namen „Test-Flight für Tester“ hinzu. Anschließend erstellen Sie eine Test-Flight-Gruppe mit dem Namen „Interessierte Benutzer“ und fügen sie einem anderen Flight-Paket mit dem Namen „Test-Flight für Interessierte Benutzer“ hinzu. Wenn Sie „Test-Flight für Tester“ einen höheren Rang als „Test-Flight für Interessierte Benutzer“ zuweisen, können Sie Pakete, bei denen Sie sich relativ sicher sind, in „Test-Flight für Interessierte Benutzer“ verwenden, und Pakete mit höherem Risiko, die nur für Tester vorgesehen sind, in „Test-Flight für Tester“. Mitglieder der Tester-Gruppe erhalten immer die Pakete, die Sie in „Test-Flight für Tester“ bereitstellen, auch wenn sie ebenfalls der Gruppe „Interessierte Benutzer“ angehören. (Wenn sich später dann herausstellt, dass die Pakete in „Test-Flight für Tester“ problemlos ausgeführt werden, können Sie „Test-Flight für Interessierte Benutzer“ so aktualisieren, dass die ursprünglich an „Test-Flight für Tester“ verteilten Pakete verwendet werden. Möglicherweise können Sie diese Pakete irgendwann auch in Ihrer Übermittlung ohne Test-Flight verwenden.


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Pakete aus einem Flight-Paket für alle Kunden zur Verfügung stellen

Wenn Sie eines oder mehrere Pakete in einem veröffentlichten Flight-Paket für Kunden verfügbar machen möchten, die keiner Test-Flight-Gruppe angehören, können Sie Ihre Übermittlung ohne Test-Flight entsprechend aktualisieren, ohne dieselben Pakete erneut hochladen zu müssen. 

Bei der Erstellung Ihrer neuen Übermittlung wird auf der Seite [**Packages**](upload-app-packages.md) eine Dropdownliste mit der Option zum Kopieren von Paketen aus einer vorherigen Übermittlung bzw. einem Flight-Paket angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete darin auswählen, um sie in die Übermittlung ohne Test-Flight aufzunehmen.

Beachten Sie, dass dieselben Regeln für Paketvalidierung gelten, auch wenn Pakete aus einer zuvor veröffentlichten Übermittlung verwendet werden. 


## <a name="delete-a-package-flight"></a>Löschen eines Flight-Pakets

Zum Löschen eines Flight-Pakets, das Sie nicht mehr unterstützen möchten, klicken Sie in der App-Übersicht auf dessen Namen. Klicken Sie auf der Übersichtsseite für Test-Flights **Ändern** und anschließend auf den Link **Löschen** , um das Flight-Paket zu löschen. (Wenn Sie eine unveröffentlichte Übermittlung des Flight-Pakets ausführen, müssen Sie zunächst diese Übermittlung löschen.) Dies kann bis zu 30 Minuten dauern.

Wenn Sie ein Flight-Paket löschen, erhalten alle Kunden mit Paketen, die Sie in diesem Flight-Paket verteilt haben, ein App-Update, wenn es ein Paket mit einer höheren Versionsnummer gibt (oder sobald ein solches Paket verfügbar ist). Wenn Benutzer die App deinstallieren und später erneut installieren, wird dies als neuer Kauf behandelt, und die Benutzer erhalten die höchste aktuell verfügbare Version. 
