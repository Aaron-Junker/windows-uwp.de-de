---
Description: Legen Sie Einschränkungen fest, wie Ihre APP ermittelt und abgerufen werden kann. dazu gehört auch, ob Personen Ihre APP im Store finden oder Ihre Store-Liste anzeigen können.
title: Auswählen von Sichtbarkeitsoptionen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Sichtbarkeit, private Zielgruppe, verfügbar, auffindbar
ms.localizationpriority: medium
ms.openlocfilehash: 8c78b8c7a84c6bdaedb58055d8b36883c6a61607
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253640"
---
# <a name="choose-visibility-options"></a>Auswählen von Sichtbarkeitsoptionen


Im Abschnitt **Sichtbarkeit** der [Seite Preise und Verfügbarkeit](set-app-pricing-and-availability.md) können Sie Einschränkungen festlegen, wie Ihre APP ermittelt und abgerufen werden kann. Dies bietet Ihnen die Option, anzugeben, ob Personen Ihre APP im Store finden oder Ihre Store-Auflistung anzeigen können.

Es gibt zwei separate Abschnitte im Sichtbarkeits Abschnitt: **Audience** und **Erkennbarkeit**. 

## <a name="audience"></a>Zielgruppe

Im Abschnitt "Audience" können Sie angeben, ob Sie die Sichtbarkeit ihrer Übermittlung auf eine bestimmte Zielgruppe einschränken möchten, die Sie definieren.


### <a name="public-audience"></a>Öffentliche Zielgruppe

Standardmäßig ist die Store-Auflistung Ihrer APP für eine **öffentliche Zielgruppe**sichtbar. Dies ist für die meisten Übermittlungen geeignet, es sei denn, Sie möchten einschränken, wer die Auflistung Ihrer APP für bestimmte Personen sehen kann. Sie können auch die Optionen im Abschnitt [Erkennbarkeit](#discoverability) verwenden, um die Auffindbarkeit zu beschränken, wenn Sie möchten.

> [!IMPORTANT]
> Wenn Sie ein Produkt übermitteln, bei dem diese Option auf **Public Audience**festgelegt ist, können Sie in einer späteren Übermittlung keine **private Zielgruppe** auswählen.


### <a name="private-audience"></a>Private Zielgruppe

Wenn Sie möchten, dass das Auflisten ihrer app nur für ausgewählte Personen sichtbar ist, die Sie angeben, wählen Sie **private Audience**aus. Mit dieser Option kann die APP nicht gefunden werden oder ist nicht für Personen in der von Ihnen angegebenen Gruppe (n) verfügbar. Diese Option wird häufig für die [Beta-Tests](beta-testing-and-targeted-distribution.md)verwendet, da Sie Ihre APP an Tester verteilen können, ohne dass eine andere Person die APP erhalten oder sogar Ihre Store-Auflistung anzeigen kann (auch wenn Sie Ihre Store-Listen-URL eingeben konnten).

Wenn Sie die Option **private Audience**auswählen, müssen Sie mindestens eine Gruppe von Personen angeben, die Ihre APP erhalten soll. Sie können eine vorhandene [bekannte Benutzergruppe](create-known-user-groups.md)auswählen, oder Sie können **eine neue Gruppe erstellen** auswählen, um eine neue Gruppe zu definieren. Sie müssen die e-Mail-Adressen eingeben, die dem Microsoft-Konto der einzelnen Personen zugeordnet sind, die Sie in die Gruppe einschließen möchten. Weitere Informationen finden Sie unter [Erstellen bekannter Benutzergruppen](create-known-user-groups.md).

Nachdem die Übermittlung veröffentlicht wurde, können die Personen in der von Ihnen angegebenen Gruppe das Auflisten der App anzeigen und die app herunterladen, sofern Sie mit dem Microsoft-Konto angemeldet sind, das mit der eingegebenen e-Mail-Adresse verknüpft ist und Windows 10, Version 1607 oder höher (einschließlich Xbox One) ausgeführt wird. Personen, die nicht als private Zielgruppe angemeldet sind, können jedoch nicht die APP-Auflistung anzeigen oder die app herunterladen, unabhängig davon, welche Betriebssystemversion Sie ausführen. Sie können aktualisierte Übermittlungen für die private Zielgruppe veröffentlichen, die auf die gleiche Weise wie ein reguläres App-Update an Mitglieder dieser Zielgruppe verteilt wird (aber immer noch nicht für Personen verfügbar ist, die sich nicht in Ihrer privaten Zielgruppe befinden, es sei denn, Sie ändern Ihre Auswahl für die Zielgruppe). 

Wenn Sie beabsichtigen, die APP zu einem bestimmten Zeitpunkt für eine öffentliche Zielgruppe verfügbar zu machen, können Sie das Kontrollkästchen **dieses Produkt öffentlich machen** , wenn Sie Ihre Übermittlung erstellen. Geben Sie das Datum und die Uhrzeit (in UTC) an, wenn das Produkt der Öffentlichkeit zur Verfügung gestellt werden soll. Berücksichtigen Sie dabei Folgendes:

- Das Datum und die Uhrzeit, die Sie auswählen, gelten für alle Märkte. Wenn Sie den releasezeitplan für verschiedene Märkte anpassen möchten, verwenden Sie dieses Kontrollkästchen nicht. Erstellen Sie stattdessen eine neue Übermittlung, bei der Ihre Einstellung in **Public Audience**geändert wird, und geben Sie dann mithilfe der Zeit [Plan](configure-precise-release-scheduling.md) Optionen den Versions Zeitpunkt für die Veröffentlichung an.
- Das Eingeben eines Datums, für **das dieses Produkt öffentlich** ist, gilt nicht für die Microsoft Store for Business und/oder Microsoft Store for Education. Damit wir Ihre APP über die Organisations Lizenzierung für diese Kunden anbieten können, müssen Sie eine neue Übermittlung mit ausgewählter **Public Audience** (und aktivierter [Organisations Lizenzierung](organizational-licensing.md) ) erstellen.
- Nach dem Datum und der Uhrzeit, die Sie ausgewählt haben, wird für alle zukünftigen Einreichungen die **öffentliche Zielgruppe**verwendet.

Wenn Sie kein Datum und keine Uhrzeit angeben, um die APP für eine öffentliche Zielgruppe verfügbar zu machen, können Sie dies zu einem späteren Zeitpunkt tun, indem Sie eine neue Übermittlung erstellen und die Zielgruppen Einstellung von **private Audience** in **Public Audience**ändern. Denken Sie dabei daran, dass Ihre APP möglicherweise einen zusätzlichen Zertifizierungsprozess durchläuft, daher sollten Sie alle neuen Zertifizierungs Probleme beheben, die auftreten können. 

Im folgenden finden Sie einige wichtige Punkte, die Sie berücksichtigen sollten, wenn Sie Ihre APP an eine private Zielgruppe verteilen:
- Personen in Ihrer privaten Zielgruppe können die APP über einen bestimmten Link zur Store-Auflistung Ihrer APP erhalten, in der Sie sich mit Ihren Microsoft-Konto anmelden müssen, um Sie anzuzeigen. Dieser Link wird bereitgestellt, wenn Sie **private Audience**auswählen. Sie finden ihn auch auf der Seite mit der [App-Identität](view-app-identity-details.md) unter **URL, wenn Ihre APP nur für bestimmte Personen sichtbar ist (Authentifizierung erforderlich)**. Stellen Sie sicher, dass Ihre Tester diesen Link und nicht die reguläre URL für Ihre Store-Auflistung erhalten.  
- Wenn Sie eine Option in der **Auffindbarkeit** nicht auswählen, die dies verhindert, können Personen in Ihrer privaten Zielgruppe Ihre APP suchen, indem Sie in der Microsoft Store-App suchen. Die webauflistung kann jedoch nicht über die Suche erkannt werden, auch für Personen in der Zielgruppe. 
- Sie können die [Veröffentlichungsdaten nicht im Abschnitt](configure-precise-release-scheduling.md) "Zeit Plan" der **Seite "Preise und Verfügbarkeit**" konfigurieren, da Ihre APP nicht für Kunden außerhalb Ihrer privaten Zielgruppe freigegeben wird.
- Andere Auswahlmöglichkeiten gelten für Personen in dieser Zielgruppe. Wenn Sie z. b. einen anderen Preis als **Free**auswählen, müssen Personen in Ihrer privaten Zielgruppe diesen Preis bezahlen, um die APP zu erhalten. 
- Wenn Sie unterschiedliche Pakete an verschiedene Personen in Ihrer privaten Zielgruppe verteilen möchten, können Sie nach ihrer ersten Übermittlung mithilfe von [Paket Flügen](package-flights.md) verschiedene Paket Aktualisierungen an Teilmengen Ihrer privaten Zielgruppe verteilen. Sie können zusätzliche bekannte Benutzergruppen erstellen, um festzulegen, wer einen bestimmten paketflug erhalten soll.
- Sie können die Mitgliedschaft der bekannten Benutzergruppe (n) in Ihrer privaten Zielgruppe bearbeiten. Denken Sie jedoch daran, dass Sie, wenn Sie jemanden entfernen, der in der Gruppe war und Ihre APP bereits heruntergeladen hat, weiterhin die APP verwenden können, aber keine von Ihnen bereitgestellten Updates erhalten (es sei denn, Sie wählen zu einem späteren Zeitpunkt " **Public Audience** " aus).
- Ihre APP ist nicht über die Microsoft Store für Unternehmen und/oder Microsoft Store für Bildungseinrichtungen verfügbar, unabhängig von den Lizenzierungs Einstellungen Ihrer Organisation, auch für Personen in Ihrer privaten Zielgruppe.
- Während der Speicher sicherstellt, dass Ihre APP nur für Personen sichtbar und verfügbar ist, die mit einem Microsoft-Konto angemeldet sind, den Sie Ihrer privaten Zielgruppe hinzugefügt haben, können wir nicht verhindern, dass diese Personen Informationen oder Screenshots außerhalb Ihrer privaten Zielgruppe freigeben. Wenn Vertraulichkeit wichtig ist, sollten Sie sicherstellen, dass Ihre private Zielgruppe nur Personen enthält, denen Sie Vertrauen, dass Sie keine Details zu Ihrer APP für andere Benutzer freigeben können.
- Stellen Sie sicher, dass Ihre Tester wissen, wie Sie Ihr Feedback geben können. Sie möchten wahrscheinlich nicht, dass Sie Feedback im FeedHub hinterlassen, da jeder andere Kunde dieses Feedback sehen könnte. Nehmen Sie an, Sie können einen Link zum Senden von e-Mails oder zum Bereitstellen von Feedback auf andere Weise einschließen.
- Alle Überprüfungen, die von Personen in Ihrer privaten Zielgruppe geschrieben wurden, können angezeigt werden. Diese Überprüfungen werden jedoch nicht in der Store-Auflistung Ihrer App veröffentlicht, auch nachdem die Übermittlung an die **öffentliche Zielgruppe**verschoben wurde. Sie können von Ihrer privaten Zielgruppe geschriebene Überprüfungen lesen, indem Sie den [Bericht Reviews](reviews-report.md)anzeigen, aber Sie können diese Daten nicht herunterladen oder die [Microsoft Store Analytics-API](../monetize/access-analytics-data-using-windows-store-services.md) verwenden, um Programm gesteuert auf diese Überprüfungen zuzugreifen.
- Wenn Sie eine APP von einer **privaten Zielgruppe** in eine **öffentliche Zielgruppe**verschieben, ist das **Veröffentlichungsdatum** , das in der Store-Auflistung angezeigt wird, das Datum, an dem Sie erstmals auf der öffentlichen Zielgruppe veröffentlicht wurde

## <a name="discoverability"></a>Erkennbarkeit

Die Auswahl im Abschnitt **Erkennbarkeit** gibt Aufschluss darüber, wie Kunden Ihre APP ermitteln und erwerben können. 

> [!IMPORTANT]
> Wenn Sie die Option " **private Audience**" auswählen, gelten die Auswahl **Möglichkeiten** nur für Personen in Ihrer privaten Zielgruppe. Kunden, die nicht in den von Ihnen angegebenen Gruppen sind, können die APP nicht finden oder sogar Ihre Liste anzeigen. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Diese Produkte verfügbar machen und im Store auffallen

Dies ist die Standardoption. Lassen Sie diese Option aktiviert, wenn Sie möchten, dass Ihre APP im Store aufgelistet wird, damit Kunden Sie über den direkten Link der APP und/oder über andere Methoden finden können, einschließlich suchen, durchsuchen und einschließen in zusammengestellter Listen. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Dieses Produkt verfügbar machen, aber im Store nicht auffallen

Wenn Sie diese Option auswählen, wird Ihre APP im Store nicht von Kunden gefunden, die suchen oder durchsuchen. die einzige Möglichkeit, um die Auflistung Ihrer APP zu erhalten, ist ein direkter Link. 

> [!TIP]
> Wenn Sie nicht möchten, dass die Auflistung öffentlich angezeigt wird, auch mit einem direkten Link, wählen Sie im Abschnitt " **Audience** " die Option **private Audience** (wie oben beschrieben) aus.

Außerdem müssen Sie eine der folgenden Optionen auswählen, um anzugeben, wie Ihre APP abgerufen werden kann:


>[!IMPORTANT]
> Jede dieser Optionen schränkt die Betriebssystemversionen ein, auf denen Kunden Ihre APP erwerben können. Lesen Sie die Beschreibungen sorgfältig durch, um sicherzustellen, dass Sie wissen, welche Betriebssystemversionen unterstützt werden. 

- **Nur direkter Link: jeder Kunde, der über einen direkten Link zur Produkt Auflistung verfügt, kann ihn herunterladen, mit Ausnahme von Windows 8. x.** Alle Kunden, die über einen direkten Link zu Ihrer APP-Auflistung gelangen, können Sie auf Geräten unter Windows 10 oder auf Geräten unter Windows Phone 8,1 oder früher (aber nicht auf Geräten unter Windows 8. x) herunterladen.
- **Übernahme aufheben: jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts sehen, Sie kann ihn jedoch nur herunterladen, wenn er sich zuvor im Besitz des Produkts befand, oder einen Werbe Code hat und ein Windows 10-Gerät verwendet.** Selbst wenn ein Kunde über einen direkten Link verfügt, kann er die app nur herunterladen, wenn er über einen [Werbe Code](generate-promotional-codes.md) verfügt und ein Windows 10-Gerät verwendet. Wenn ein Kunde einen Aktions Code hat, kann er ihn verwenden, um Ihre App kostenlos (nur unter Windows 10) zu erhalten, obwohl Sie ihn keinem anderen Kunden anbieten. Abgesehen von der Verwendung eines Aktionscodes gibt es keine Möglichkeit, die APP zu erhalten.

> [!TIP]
> Wenn Sie einem neuen Kunden keine app mehr anbieten möchten, können Sie auf der Seite "Übersicht" die Option **app verfügbar machen** auswählen. Nachdem Sie sich vergewissert haben, dass Sie die APP nicht mehr verfügbar machen möchten, wird Sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und keine neuen Kunden werden in der Lage sein, Sie zu erhalten (es sei denn, Sie haben einen [Werbe Code](generate-promotional-codes.md) , der sich auf einem Windows 10-Gerät befindet). Durch diese Aktion wird die **Sichtbarkeits** Auswahl in ihrer Übermittlung überschrieben. Um die APP neuen Kunden erneut zur Verfügung zu stellen (gemäß ihrer **Sichtbarkeits** Auswahl), können Sie jederzeit auf der Seite "Übersicht" auf **App bereitstellen** klicken. Weitere Informationen finden Sie unter [Entfernen einer App aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




