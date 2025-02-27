---
title: Erstellen einer komplexen UWP-App (Universelle Windows-Plattform)
description: Die Microsoft-Designteams unterteilen den Prozess der App-Erstellung in fünf einzelne Phasen - Konzept, Struktur, Dynamik, Darstellung und Prototyp. Wir möchten Sie dazu ermutigen, einen ähnlichen Prozess anzuwenden, um der Welt neue und ansprechende Erfahrungen zu vermitteln.
ms.assetid: 9A5189CD-3B97-4967-8E7D-36D25F04F244
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fb7229d977f2d2f32b251a524e101a8acf2b2524
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193013"
---
#  <a name="building-a-complex-universal-windows-platform-uwp-app"></a>Erstellen einer komplexen UWP-App (Universelle Windows-Plattform)

Die Microsoft-Designteams unterteilen den Prozess der App-Erstellung in fünf einzelne Phasen - Konzept, Struktur, Dynamik, Darstellung und Prototyp. Wir möchten Sie dazu ermutigen, einen ähnlichen Prozess anzuwenden, um der Welt neue und ansprechende Erfahrungen zu vermitteln.

## <a name="concept"></a>Konzept

**Eingrenzen der App**

Beim Planen Ihrer Universellen Windows-App sollten Sie nicht nur festlegen, welche Funktionen die App hat und für welche Zielgruppe sie bestimmt ist, sondern auch, was ihre großen Stärken sind. Grundlage jeder guten App ist ein starkes Konzept, mit dem für ein solides Fundament gesorgt wird.

Nehmen wir einmal an, dass Sie eine Foto-App erstellen möchten. Wenn Sie über die Gründe nachdenken, aus denen Benutzer Fotos nutzen, speichern und teilen, kommen Sie zu dem Ergebnis, dass sie Erinnerungen wecken, mithilfe von Fotos mit anderen Personen in Verbindung bleiben und Fotos sicher aufbewahren möchten. Ihre App sollte also für diese Punkte ideal geeignet sein, und Sie sollten sich für den Rest des Designprozesses von diesen Zielen leiten lassen.

**Wozu dient Ihre App?** Beginnen Sie mit einem allgemeinen Konzept. Führen Sie alle Punkte auf, die Benutzern mit Ihrer App ermöglicht werden sollen.

Nehmen wir beispielsweise an, Sie möchten eine App erstellen, mit der Benutzer Reisen planen können. Hier sind einige Ideen für die Konzeptplanung:

-   Karten für alle Orte auf der Reiseroute abrufen und mit auf die Reise nehmen
-   Herausfinden, ob in einer Stadt während des Aufenthalts Veranstaltungen stattfinden
-   Reisende separate, aber freigabefähige Listen mit wichtigen Aktivitäten und Sehenswürdigkeiten erstellen lassen
-   Reisenden ermöglichen, alle Fotos zusammenzustellen und mit Freunden und Familienmitgliedern zu teilen
-   Empfohlene Reiseziele basierend auf Flugpreisen abrufen
-   Eine Übersicht mit Angeboten für Restaurants, Geschäfte und Aktivitäten in der Nähe des Reiseziels abrufen

![Entwurf einer Reise-App](images/ux-triptracker-tab-phone-700.png)

**Welche Stärken hat Ihre App?** Nehmen Sie sich einen Moment Zeit, um zu überprüfen, ob Ihre Ideenliste ein Szenario enthält, das Sie regelrecht anspringt. Kürzen Sie Ihre Liste auf ein Szenario, auf das Sie sich konzentrieren möchten. Dabei streichen Sie vielleicht viele gute Ideen, doch dadurch machen Sie ein einzelnes Szenario zu einem guten Szenario.

Entscheiden Sie nach der Auswahl eines Szenarios, wie Sie einem durchschnittlichen Benutzer erklären, wodurch sich Ihre App besonders auszeichnet, und schreiben Sie diesen Punkt in einem Satz nieder. Beispiel:

-   Meine Reise-App zeichnet sich besonders dadurch aus, dass sie Freunde beim gemeinsamen Erstellen von Reiserouten für Gruppenreisen unterstützt.
-   Meine Fitness-App zeichnet sich besonders dadurch aus, dass sie Trainingsfortschritte verfolgt und Erfolge mit anderen Personen teilt.
-   Meine Lebensmittel-App zeichnet sich besonders dadurch aus, dass sie den wöchentlichen Einkauf in der Familie koordiniert. Der Einkauf wird dadurch weder vergessen noch doppelt durchgeführt.

![Entwurf eines Tools für die Zusammenarbeit](images/ux-collaboration-tabphone-700.png)

Dies sind die besonderen Stärken der App. Sie können bei der Entscheidungsfindung hinsichtlich des Designs nützlich sein und es erleichtern, Kompromisse beim Erstellen der App einzugehen. Konzentrieren Sie sich auf die Szenarien, die Sie Benutzern in Ihrer App anbieten möchten. Machen Sie daraus aber keine Liste mit Features. Beschreiben Sie, welche Vorteile die App den Benutzern bietet, und nicht, was sie kann.

**Der Designtrichter**

Wenn Ihnen eine Idee gefällt, ist es sehr verlockend, sofort mit der Entwicklung zu beginnen und sogar schon die Produktion anzustreben. Es kann jedoch sein, dass Sie dies tun und Ihnen später eine andere interessante Idee einfällt. Dann ist die Wahrscheinlichkeit hoch, dass sie bei der Idee bleiben, in die Sie bereits investiert haben – unabhängig davon, welche relativen Vorteile die beiden Ideen jeweils bieten. Wenn Ihnen die zweite Idee nur zu einem früheren Zeitpunkt des Prozesses eingefallen wäre! Der so genannte "Designtrichter" ist ein Verfahren, das Sie dabei unterstützt, die besten Ideen so früh wie möglich zu entwickeln.

Der Ausdruck "Trichter" beruht auf der Form. Am breiten Ende des Trichters werden viele Ideen hineingefüllt, und jede Idee wird nur als grobes Designartefakt festgehalten (ggf. in Form einer Skizze oder als Textabsatz). Wenn sich diese Ideensammlung dann in Richtung des schmalen Endes des Trichters bewegt, wird die Anzahl von Ideen reduziert, während Qualität und Umfang der Artefakte zu den Ideen sich erhöhen. Für jedes Artefakt sollten nur die Informationen erfasst werden, die zum Vergleichen der Ideen oder zum Beantworten einer bestimmten Frage wie „Ist dies benutzerfreundlich oder intuitiv?“ erforderlich sind. *Betreiben Sie dafür keinen weiteren Zeit- und Arbeitaufwand*. Einige Ideen fallen beim Testen heraus, was kein Problem ist, weil Sie lediglich den Aufwand investiert haben, der zum Einschätzen der Idee nötig war. Ideen, die sich für den weiteren Weg durch den Trichter qualifizieren, werden nach und nach eingehender behandelt. Am Ende steht ein einzelnes Designartefakt, das die Idee repräsentiert, die als Gewinner aus dem Prozess hervorgegangen ist. Die Idee hat gewonnen, weil sie mit den meisten Vorteilen verbunden ist, und nicht nur, weil sie Ihnen zuerst eingefallen ist. So entwerfen Sie die bestmögliche App.

## <a name="structure"></a>Struktur


**Vereinfachung durch Organisation**

![Vereinfachung durch Organisation](images/ux-vision-and-process-organization.png)

Wenn Sie mit Ihrem Konzept zufrieden sind, können Sie zur nächsten Phase übergehen und den App-Entwurf erstellen. Mithilfe der Informationsarchitektur (IA) verleihen Sie Ihren Inhalten die erforderliche strukturelle Integrität. Dies ist beim Festlegen des Navigationsmodells für die App und später der App-Identität hilfreich. Indem Sie die Organisation der Inhalte planen – und wie Ihre Benutzer diese Inhalte entdecken können –, erhalten Sie einen besseren Eindruck, wie Benutzer die App erleben werden.

Eine gute Informationsarchitektur erleichtert nicht nur die Umsetzung von Benutzerszenarien, sondern auch die Planung wichtiger Bildschirme, die zuerst entworfen werden müssen. Bei der [Audible](https://www.windowsphone.com/store/app/audible-for-windows-phone/bdc813dd-c20b-41f8-8646-de72fa0b365d)-App wird beispielsweise direkt mit einem Hub gestartet, über den Benutzer auf die Bibliothek, den Store, News und statistische Daten zugreifen können. Die Benutzeroberfläche ist zielgerichtet gestaltet, damit Benutzer Hörbücher schnell abrufen und verwenden können. Auf den unteren Ebene der App liegt der Schwerpunkt auf spezielleren Aufgaben.

Verwandte Richtlinien finden Sie unter [Navigationsdesigngrundlagen](../design/basics/navigation-basics.md).

## <a name="dynamics"></a>Dynamics

**Umsetzen des Konzepts**

In der Konzeptphase geht es um das Festlegen des App-Zwecks, und in der dynamischen Phase geht es um die Ausführung und Erreichung dieses Zwecks. Dazu werden viele verschiedene Mittel eingesetzt, z. B. Drahtmodelle zum Skizzieren des Seitenflusses (der Weg von einem Ort zum anderen innerhalb der App, um zum gewünschten Ergebnis zu gelangen). Überlegen Sie außerdem, welche Ausdrucksweise und Begriffe Sie für die App-Benutzeroberfläche verwenden möchten. Drahtmodelle sind ein einfaches Werkzeug, mit dem Sie wichtige Entscheidungen zum Benutzerfluss für Apps treffen können.

Der App-Fluss sollte eng mit den besonderen Stärken der App zusammenhängen und Benutzer bei der Ausübung des jeweiligen Szenarios unterstützen, das Sie ermöglichen möchten. Großartige Apps haben Flüsse, die leicht zu erlernen sind und nur geringen Mehraufwand erfordern. Denken Sie von Bildschirm zu Bildschirm: Betrachten Sie Ihre App so, als ob Sie sie zum ersten Mal verwenden würden. Wenn Sie die Erstellung der Seiten genau auf die Benutzerszenarien zuschneiden, erhalten die Nutzer genau das, was sie benötigen, ohne unnötig viele Bildschirmberührungen ausführen zu müssen. In dieser Phase geht es auch um Bewegung. Die richtigen Bewegungsfunktionen sind die Grundlage eines guten Flusses und einfacher Wechsel von einer Seite zur nächsten.

Häufig verwendete Methoden für diesen Schritt:

-   Skizzieren Sie den Fluss: Was kommt zuerst, was kommt danach?
-   Stellen Sie den Fluss in einem Storyboard dar: Wie sollen Benutzer in der Oberfläche durch den Fluss navigieren?
-   Erstellen Sie einen Prototyp: Testen Sie den Fluss schnell mit einem Prototyp.

**Was möchten Sie Benutzern ermöglichen?** Die Reise-App beispielsweise „zeichnet sich besonders dadurch aus, dass sie Freunde beim gemeinsamen Erstellen von Reiserouten für Gruppenreisen unterstützt“. Listen Sie die Flüsse auf, die Sie aktivieren möchten:

-   Erstellen einer Reise mit allgemeinen Informationen
-   Einladen von Freunden zur Teilnahme an einer Reise
-   Teilnehmen an der Reise eines Freunds
-   Anzeigen der von anderen Reisenden empfohlenen Reiserouten
-   Hinzufügen von Reisezielen und Aktivitäten zu Reisen
-   Bearbeiten und Kommentieren von Reisezielen und Aktivitäten, die von Freunden hinzugefügt wurden
-   Teilen von Reiserouten mit Freunden und Familie

## <a name="visual"></a>Visuelles Element

**Botschaften ohne Worte**

![Entwurf für eine App zum Mixen von Cocktails](images/ux-cocktailcreator-tab-phone.png)

Nachdem Sie die Dynamik der App festgelegt haben, können Sie Ihre App mit den richtigen visuellen Elementen ansprechend gestalten. Mit guten visuellen Elementen legen Sie nicht nur fest, wie Ihre App aussieht, sondern sorgen durch den Einsatz von Animationen und Bewegung auch für das App-Gefühl und Lebendigkeit. Die Farbpalette, Symbole und Grafiken sind nur einige Beispiele für diese visuelle Sprache.

Alle Apps haben eine eigene Identität, und Sie haben freie Hand dabei, welche visuelle Richtung Sie für Ihre App einschlagen möchten. Richten Sie das Erscheinungsbild an den Inhalten aus, und vermeiden Sie, dass das Aussehen die Inhalte diktiert.

## <a name="prototype"></a>Prototyp

**Optimieren Sie Ihr Meisterwerk**

Die Prototyperstellung ist eine Phase des *Designtrichters* (der weiter oben bereits beschrieben wurde). Hier wird das Artefakt der Idee über die Skizzierung hinaus weiterentwickelt, aber noch nicht in so komplizierter Form wie bei einer fertigen App. Ein Prototyp kann beispielsweise eine Abfolge von mit der Hand gezeichneten Bildschirmen sein, die Benutzern gezeigt werden. Die Person, die den Test durchführt, kann auf Hinweise von Benutzern reagieren, indem sie einzelne Bildschirme anbietet oder den Seiten kleinere UI-Bereiche hinzufügt bzw. daraus entfernt, um eine laufende App zu simulieren. Ein Prototyp kann auch eine sehr einfache App sein, mit der einige Workflows simuliert werden, vorausgesetzt, der Bediener hält sich an ein Skript und betätigt die richtigen Schaltflächen. In dieser Phase werden Ihre Ideen zum ersten Mal wirklich zum Leben erweckt, und Ihre harte Arbeit wird einem ersten richtigen Test unterzogen. Nehmen Sie sich bei der Prototyperstellung von Bereichen Ihrer App die Zeit zum Entwerfen und Verfeinern der Komponenten, die jeweils am wichtigsten sind.

Wichtiger Hinweis für weniger erfahrene Entwickler: Die Entwicklung großartiger Apps ist ein iterativer Prozess. Es ist ratsam, Prototypen zu einem frühen Zeitpunkt und häufig zu erstellen. Wie bei jedem kreativen Vorgang sind die besten Apps das Produkt intensiver Tests.

## <a name="decide-what-features-to-include"></a>Überlegen, welche Features Sie bereitstellen möchten

Wenn Sie die Bedürfnisse der Benutzer kennen und wissen, wie Sie sie bei deren Erfüllung unterstützen können, sollten Sie sich mit den speziellen Tools der Toolbox beschäftigen. Erkunden Sie die Universal Windows Platform (UWP), und ermitteln Sie, welche Features die Anforderungen Ihrer App erfüllen. Beachten Sie bei jeder Funktion die [Richtlinien für die Benutzererfahrung](https://developer.microsoft.com/windows/apps/design).
<!--need URL for landing page -->

Allgemeine Methoden:

-   Plattformrecherche: Finden Sie heraus, welche Features die Plattform bietet und wie Sie sie verwenden können.
-   Assoziationsdiagramme: Verbinden Sie Ihre Flüsse mit Features.
-   Prototyp: Testen Sie die Features, um sicherzustellen, dass sie wie gewünscht funktionieren.

**App-Verträge**  Ihre App kann Teil von App-Verträgen sein, die umfassende, App- und funktionsübergreifende Benutzerflüsse ermöglichen.

-   **Teilen**  Ermöglichen Sie es Benutzern, App-Inhalte mit anderen Benutzern über andere Apps zu teilen und freigabefähige Inhalte von anderen Benutzern und Apps zu erhalten.
-   **Wiedergeben auf**  Ermöglichen Sie es Benutzern, Audio-, Videodaten oder Bilder von Ihrer App auf andere Geräte im Heimnetzwerk zu streamen.
-   **Dateiauswahl und Dateiauswahlerweiterungen:** Ermöglichen Sie Benutzern das Laden und Speichern ihrer Dateien im lokalen Dateisystem, auf angeschlossenen Speichergeräten, in Heimnetzgruppen und sogar in anderen Apps. Außerdem können Sie eine Dateiauswahlerweiterung bereitstellen, sodass andere Apps den Inhalt Ihrer App laden können.

Weitere Informationen finden Sie unter [App-Verträge und Erweiterungen](/previous-versions/windows/apps/hh464906(v=win.10)).
<!-- Win 8 page. Should have replacement. -->

**Verschiedene Ansichten, Formfaktoren und Hardwarekonfigurationen**  Windows überlässt Benutzern die Kontrolle und rückt die App in den Vordergrund. Ihre App-UI soll auf jedem Gerät, mit jeder Eingabemethode, in jeder Ausrichtung, mit allen Hardwarekonfigurationen und unter allen Umständen überzeugen.

**Toucheingabe**  Windows bietet eine einzigartige und charakteristische Toucheingabe, die mehr kann, als nur die Funktionen einer Maus zu emulieren.

Der semantische Zoom beispielsweise stellt eine für Touchscreens optimierte Methode zum Navigieren in umfangreichen Inhalten dar. Benutzer können Inhaltskategorien schwenken oder einen Bildlauf darin durchführen und dann Kategorien vergrößern, um weitere ausführlichere Informationen anzuzeigen. Damit können Sie Ihre Inhalte auf besser fühlbare, bildlichere und aussagekräftigere Art und Weise darstellen als mit traditionellen Navigations- und Layoutmustern wie Registerkarten.

Selbstverständlich können Sie eine Reihe von Interaktionen für Touchscreens wie Drehen, Schwenken, Wischen usw. nutzen. Erfahren Sie mehr über [Toucheingaben und andere Benutzerinteraktionen](../design/input/input-primer.md).

**Interessant und aktuell**  Mit den folgenden Standardfeatures stellen Sie sicher, dass Ihre App interessant ist und die Aufmerksamkeit der Benutzer gewinnt:

-   **Animationen**  Verwenden Sie Animationen aus unserer Bibliothek, um Benutzern eine schnelle und fließende App-Bedienung zu ermöglichen. Helfen Sie Benutzern, Kontextänderungen zu verstehen, und verbinden Sie Interaktionen mit visuellen Übergängen. Weitere Informationen zum [Animieren der UI](../design/motion/xaml-animation.md).
-   **Popupbenachrichtigungen**  Erinnern Sie Benutzer mithilfe von Popupbenachrichtigungen an Termine oder persönlich relevante Inhalte. Sie können sie damit auch zur Verwendung der App animieren, selbst wenn diese geschlossen ist. Weitere Informationen zu [Kacheln, Infoanzeigen und Popupbenachrichtigungen](../design/shell/tiles-and-notifications/index.md).
-   **App-Kacheln**  Stellen Sie aktuelle und relevante Updates bereit, damit Benutzer Ihre App erneut nutzen. Weitere Informationen hierzu erhalten Sie im nächsten Abschnitt. Weitere Informationen finden Sie unter [App-Kacheln](../design/shell/tiles-and-notifications/creating-tiles.md).

**Personalisierung**

-   **Einstellungen**  Ermöglichen Sie Benutzern die Anpassung Ihrer App, indem sie App-Einstellungen speichern. Fassen Sie alle Einstellungen der App auf einem Bildschirm zusammen, und ermöglichen Sie Benutzern die Konfiguration der App mithilfe gängiger Methoden. Weitere Informationen zum [Hinzufügen von App-Einstellungen](../design/app-settings/guidelines-for-app-settings.md).
-   **Roamingbetrieb**  Sorgen Sie mittels Datenroaming für eine konsistente geräteübergreifende Umgebung, sodass Benutzer Aufgaben in ihrer bevorzugten Umgebung an der Stelle fortsetzen können, an der sie diese unterbrochen haben – und zwar unabhängig vom jeweils verwendeten Gerät. Erleichtern Sie Benutzern die Nutzung Ihrer App auf allen Geräten, z. B. auf dem Familien-PC im Wohnzimmer, dem Arbeits-PC und auf dem eigenen Tablet und anderen Formfaktoren. Einstellungen und App-Status werden mittels Roaming beibehalten. Weitere Informationen finden Sie unter [Verwalten von App-Daten](../design/app-settings/store-and-retrieve-app-data.md) und [Richtlinien für das Roaming von Anwendungsdaten](../design/app-settings/store-and-retrieve-app-data.md).
-   **Benutzerkacheln**  Gestalten Sie die App für Benutzer persönlicher, indem Sie deren Benutzerkachelbild laden. Sie können ihnen auch die Möglichkeit bieten, Inhalte aus der App als persönliche Kachel in Windows festzulegen.

**Gerätefunktionen**  Stellen Sie sicher, dass Ihre App in vollem Umfang von den Funktionen moderner Geräte profitiert.

-   **Näherungsbewegungen**  Ermöglichen Sie es Benutzern, mithilfe einer physischen Kopplung eine Verbindung mit Geräten anderer Benutzer herzustellen, die sich in der Nähe befinden (z. B. bei Multiplayer-Spielen). Weitere Informationen hierzu finden Sie unter [Näherung und Kopplung](/previous-versions/windows/apps/hh465229(v=win.10)).
-   **Kameras und externe Speichervorrichtungen**  Verbinden Sie Benutzer mit ihren integrierten oder angeschlossenen Kameras, damit sie chatten, Konferenzen führen, Vlogs aufzeichnen, Profilbilder aufnehmen, die Welt um sich herum dokumentieren oder sonstige Aktivitäten ausführen können, für die Ihre App geeignet ist. Weitere Informationen zum [Zugreifen auf Inhalte auf Wechselspeichermedien](/previous-versions/windows/apps/hh465189(v=win.10)).
-   **Beschleunigungsmesser und andere Sensoren:** Geräte sind heutzutage mit einer Reihe von Sensoren ausgestattet. Ihre App kann die Bildschirmhelligkeit basierend auf der Umgebungshelligkeit erhöhen oder verringern, die UI bei Drehung des Bildschirms anpassen oder auf physische Bewegungen reagieren. Weitere Informationen zu [Sensoren](../devices-sensors/sensors.md).
-   **Geolocation**  Nutzen Sie Geolocation-Informationen aus Standardwebdaten oder von Geolocation-Sensoren, damit Benutzer sich besser zurechtfinden, ihre Position auf einer Karte ermitteln oder Benachrichtigungen über Personen, Aktivitäten und Ziele in der Nähe empfangen können. Weitere Informationen zum [geografischen Standort](/previous-versions/windows/apps/hh465139(v=win.10)).

Betrachten Sie noch einmal das Beispiel der Reise-App. Damit sich die App besonders dadurch auszeichnet, dass sie Freunde beim gemeinsamen Erstellen von Reiserouten für Gruppenreisen unterstützt, könnten Sie einige der folgenden Features nutzen:

-   Teilen: Benutzer teilen anstehende Reisen und ihre Reiserouten – und damit ihre Vorfreude auf die Reise – mit Freunden und Familie in zahlreichen sozialen Netzwerken.
-   Suchen: Benutzer suchen und finden Aktivitäten oder Reiseziele in geteilten oder öffentlichen Reiserouten anderer Benutzer, die sie in ihre eigenen Reisen einbeziehen können.
-   Benachrichtigungen: Benutzer werden benachrichtigt, wenn andere Reisende ihre Reiserouten aktualisieren.
-   Einstellungen: Benutzer konfigurieren die App wunschgemäß, z. B. für welche Reise Benachrichtigungen angezeigt werden sollen oder welche sozialen Gruppen die Reiserouten der Benutzer durchsuchen dürfen.
-   Semantischer Zoom: Benutzer navigieren durch den Reiseverlauf und zoomen, um ausführlichere Details zur langen Liste geplanter Aktivitäten anzuzeigen.
-   Benutzerkacheln: Benutzer wählen das Bild aus, das angezeigt werden soll, wenn sie ihre Reise mit Freunden teilen.

## <a name="decide-how-to-monetize-your-app"></a>Ermitteln der gewinnbringenden Nutzung von Apps

Ihnen stehen zahlreiche Möglichkeiten zur Verfügung, Gewinn mit Ihrer App zu erzielen. Wenn Sie Anzeigen oder Angebote in Apps verwenden, sollte die UI das auch unterstützen. Weitere Informationen finden Sie unter [Planen der Monetisierung](../monetize/index.md).

## <a name="design-the-ux-for-your-app"></a>Entwerfen der UX für Ihre App

Hier geht es darum, die Grundlagen richtig festzulegen. Sie wissen nun, wodurch sich Ihre App besonders auszeichnet, und haben herausgefunden, welche Flüsse Sie unterstützen möchten. Jetzt können Sie über die Grundlagen der Benutzeroberfläche (UX) nachdenken.

**Wie sollen UI-Inhalte organisiert werden?**   Die meisten App-Inhalte können in Gruppen oder Hierarchien gegliedert werden. Das Element, das Sie als Gruppierung auf oberster Ebene des Inhalts auswählen, sollte mit den besonderen Stärken übereinstimmen.

Im Beispiel der Reise-App stehen mehrere Methoden zum Gruppieren von Reiserouten zur Verfügung. Liegt der Fokus der App auf dem Entdecken interessanter Reiseziele, können Sie diese beispielsweise basierend auf dem jeweiligen Interessengebiet gruppieren, z. B. Abenteuer, Spaß in der Sonne oder romantische Ausflüge. Da der Fokus der App jedoch auf dem Planen von Reisen mit Freunden liegt, ist es sinnvoller, Reiserouten basierend auf sozialen Kreisen (z. B. Familie, Freunde oder Arbeit) zu gliedern.

Die Auswahl der Gliederung für Inhalte hilft Ihnen bei der Entscheidung, welche Seiten oder Anzeigen Ihre App benötigt. Weitere Informationen finden Sie in den UI-Grundlagen.

**Wie sollen UI-Inhalte präsentiert werden?** Nachdem Sie entschieden haben, wie Sie die UI organisieren, können Sie UX-Ziele festlegen. Diese geben an, wie Ihre UI aufgebaut und dem Benutzer präsentiert wird. Sie sollten für alle Szenarien sicherstellen, dass Benutzer die App so schnell wie möglich wieder fortsetzen und nutzen können. Überlegen Sie dazu, welche Teile der UI zuerst angezeigt werden müssen. Stellen Sie sicher, dass diese Teile vollständig sind, bevor Sie Zeit in das Erstellen der unwichtigeren Teile investieren.

Bei der Reisen-App möchten Benutzer vermutlich zunächst nach einer bestimmten Reiseroute suchen. Um diese Infos schnellstmöglich zu präsentieren, sollte zuerst eine Liste der Reisen angezeigt werden. Verwenden Sie hierfür ein **ListView**-Steuerelement.

![Entwurf für die Reiseroutenauswahl in einer Reise-App](images/ux-app-travel-cc-a-1-180.png)

Nachdem die Liste mit den Reisen angezeigt wurde, können Sie mit dem Laden anderer Features wie etwa einem Newsfeed mit den Reisen von Freunden beginnen.

**Welche Benutzeroberflächen und Befehle werden benötigt?**   Überprüfen Sie die Flüsse, die Sie zuvor ermittelt haben. Erstellen Sie für jeden Fluss eine grobe Übersicht über die Schritte, die der Benutzer ausführt.

Betrachten Sie den Fluss "Teilen von Reiserouten mit Freunden und Familie". Es wird angenommen, dass der Benutzer bereits eine Reise erstellt hat. Für das Teilen einer Reiseroute können beispielsweise folgende Schritte erforderlich sein:

1.  Der Benutzer öffnet die App und sieht eine Liste der erstellten Reisen.
2.  Der Benutzer tippt auf die Reise, die er teilen möchte.
3.  Die Details zur Reise werden auf dem Bildschirm angezeigt.
4.  Der Benutzer greift auf die UI zu, um das Teilen zu initiieren.
5.  Der Benutzer wählt die E-Mail-Adresse oder den Namen des Freunds aus bzw. gibt die E-Mail-Adresse oder den Namen des Freunds ein, mit dem er die Reise teilen möchte.
6.  Der Benutzer greift auf die UI zu, um das Teilen abzuschließen.
7.  Ihre App aktualisiert die Reisedetails mit der Liste der Personen, mit denen die Reise geteilt wurde.

Bei diesem Vorgang wird nach und nach deutlich, welche UI erstellt werden muss und welche zusätzlichen Details erforderlich sind (z. B. das Entwerfen eines E-Mail-Textbausteins für Freunde, die Ihre App noch nicht verwenden). Darüber hinaus können Sie überflüssige Schritte entfernen. Beispielsweise kann es sein, dass der Benutzer die Details der Reise vor dem Teilen nicht unbedingt sehen muss. Je einfacher der Fluss, desto benutzerfreundlicher.

Ausführlichere Informationen zur Verwendung der unterschiedlichen Oberflächen finden Sie unter <!--[Command design basics](../design/basics/commanding-basics.md)-->.

**Wie soll sich der Fluss anfühlen?** Nachdem Sie die Schritte definiert haben, die der Benutzer ausführt, können Sie diesen Fluss in Leistungsziele umwandeln. Weitere Informationen finden Sie unter [Planen der Leistung](../debug-test-perf/planning-and-measuring-performance.md).

**Wie sollen Befehle organisiert werden?**   Ermitteln Sie anhand der Übersicht über die Schritte im Fluss potenzielle Befehle, die ins Design aufgenommen werden müssen. Überlegen Sie dann, wo sich diese Befehle in der App befinden sollen.

-   **Nutzen Sie möglichst immer die Inhalte.**   Lassen Sie die Benutzer den Inhalt möglichst direkt auf der App-Canvas bearbeiten, anstatt Befehle zur Verarbeitung der Inhalte hinzuzufügen. Lassen Sie beispielsweise in der Reise-App zu, dass Benutzer ihre Reiseroute durch Ziehen und Ablegen von Aktivitäten in einer Liste auf der Canvas neu gestalten, anstatt die Aktivität auswählen und sie mithilfe der Befehlsschaltflächen zum Verschieben nach oben bzw. nach unten neu anordnen zu müssen.
-   **Falls Sie den Inhalt nicht nutzen können.** Ordnen Sie Befehle in einer dieser UI-Oberflächen an, falls das Verwenden des Inhalts nicht möglich ist:

    -   In der [Befehlsleiste](../design/controls-and-patterns/app-bars.md): Platzieren Sie die meisten Befehle in der Befehlsleiste. Diese bleibt in der Regel ausgeblendet, bis der Benutzer sie durch eine Tippbewegung einblendet.
    -   Auf der App-Canvas: Befindet sich der Benutzer auf einer Seite oder in einer Ansicht mit nur einem Zweck, können Sie Befehle für diesen Zweck direkt auf der Canvas bereitstellen. Es sollte nur sehr wenige dieser Befehle geben.
    -   In einem [Kontextmenü](../design/controls-and-patterns/menus.md): Sie können Kontextmenüs für Zwischenablageaktionen (z. B. Ausschneiden, Kopieren und Einfügen) oder für Befehle verwenden, die auf Inhalte angewendet werden, die nicht ausgewählt werden können (z. B. Hinzufügen einer Ortsmarke zu einem Standort auf einer Karte).

**Bestimmen Sie das Layout Ihrer App in den verschiedenen Ansichten.**   Windows unterstützt Ansichten im Hoch- und Querformat sowie das Ändern der App-Größe auf eine beliebige Breite zwischen Vollbild und Mindestbreite. Ihre App soll in jeder Größe, auf jedem Bildschirm und in jeder Ausrichtung richtig angezeigt werden und funktionieren. Dazu müssen Sie das Layout der UI-Elements für unterschiedliche Größen und Ansichten planen. In diesem Fall passt sich die App-UI einfach an die Anforderungen und Einstellungen der Benutzer an.

![App-Designs für PCs und mobile Geräte](images/ux-budgettracker1-md-notablet.png)

Weitere Informationen zur Berücksichtigung unterschiedlicher Bildschirmgrößen beim Design finden Sie unter [Bildschirmgrößen und Haltepunkte für reaktionsfähiges Design](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="make-a-good-first-impression"></a>Erzielen eines positiven ersten Eindrucks

Überlegen Sie, was Benutzer denken, fühlen oder tun sollen, wenn sie Ihre App zum ersten Mal starten. Berücksichtigen Sie dabei ihre besonderen Stärken. Auch wenn Sie keine Gelegenheit dazu haben, den Benutzern persönlich mitzuteilen, wodurch sich Ihre App besonders auszeichnet, können Sie ihnen diese Botschaft vermitteln, wenn Sie einen ersten Eindruck hinterlassen. Nutzen Sie dazu Folgendes:

**Kacheln und Benachrichtigungen:** Die Kachel ist das Gesicht Ihrer App. Was bewegt Benutzer dazu, unter allen anderen Apps auf dem Startbildschirm genau Ihre App zu starten? Stellen Sie sicher, dass die Kachel die Marke der App hervorhebt und zeigt, wodurch sich die App besonders auszeichnet. Nutzen Sie Kachelbenachrichtigungen, sodass Ihre App immer aktuell und relevant wirkt und Benutzer sie immer wieder aufrufen.

**Begrüßungsbildschirm**  Der Begrüßungsbildschirm sollte so schnell wie möglich geladen werden und nur bis zur Initialisierung des App-Status sichtbar bleiben. Die optische Gestaltung des Begrüßungsbildschirms sollte dem Charakter Ihrer App entsprechen.

**Erster Start**  Was sehen Benutzer, bevor sie sich für Ihren Dienst registrieren, sich bei ihrem Konto anmelden oder eigene Inhalte hinzufügen? Demonstrieren Sie den Wert Ihrer App, bevor Sie Benutzer um Informationen bitten. Stellen Sie ggf. Beispielinhalte zum Ausprobieren für Benutzer bereit. So wird der Zweck Ihrer App verständlich, bevor Benutzer sich festlegen müssen.

**Startseite**  Die Startseite ist die Seite, die nach dem Start der App angezeigt wird. Der Inhalt auf dieser Seite sollte klar definiert sein und sofort deutlich machen, wozu Ihre App dient. Heben Sie einen Bereich der App auf dieser Seite hervor, um dafür zu sorgen, dass Benutzer die restlichen Funktionen dann selbst entdecken möchten. Gestalten Sie die Startseite übersichtlich und ohne überflüssige Elemente, damit Benutzer nicht abgelenkt werden.

## <a name="validate-your-design"></a>Überprüfen des Entwurfs

Bevor Sie sich der Entwicklung der App widmen, sollten Sie das Design oder den Prototyp anhand von Richtlinien, Benutzereindrücken und Anforderungen überprüfen. Sie vermeiden dadurch möglicherweise eine spätere Überarbeitung. Für jedes Feature sind Richtlinien für die Benutzerfreundlichkeit verfügbar, die Sie bei der optimalen Gestaltung Ihrer App unterstützen. Außerdem gelten für die Veröffentlichung von Apps im Microsoft Store eine Reihe von Store-Anforderungen. Sie können die App mithilfe des [Zertifizierungskits für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) auf die Einhaltung technischer Store-Anforderungen überprüfen. Außerdem können Sie mit den Leistungstools in Microsoft Visual Studio sicherstellen, dass die Benutzer in allen Szenarien von hoher Benutzerfreundlichkeit profitieren.

Mithilfe der [detaillierten UX-Richtlinien für UWP-Apps](https://developer.microsoft.com/windows/apps/design) behalten Sie die wichtigen Features der App im Blick. Analysieren Sie die Leistung der einzelnen Szenarien der App mit [Visual Studio-Leistungstools](/visualstudio/profiling/profiling-feature-tour).