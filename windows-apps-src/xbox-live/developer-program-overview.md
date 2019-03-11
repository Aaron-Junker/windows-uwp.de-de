---
title: Übersicht über das Entwicklerprogramm
description: Erfahren Sie, bis die anderen Entwickler-Programme, die Xbox Live verwendet.
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.date: 05/30/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Developer-Programm, Ersteller
ms.localizationpriority: medium
ms.openlocfilehash: 05abd3f28328f4418f5a8a772049b3869b488ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603885"
---
# <a name="developer-program-overview"></a>Übersicht über das Entwicklerprogramm

Wenn Sie Xbox Live aktiviert Titel entwickeln möchten, stehen mehrere Optionen zur Verfügung. Jede bietet unterschiedliche Ebenen von Zeit Investitionen, die ihrerseits Funktionen zur Verfügung, und support-Optionen.

## <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Die Xbox Live Creators-Programm ist ein guter Ausgangspunkt für die Xbox Live, wenn Sie sich mit der Xbox Live-Entwicklung vertraut machen möchten. Kein Genehmigungsprozess von Microsoft ist erforderlich, um diesem Programm teilnehmen, und es sind minimale Zertifizierung und Anforderungen zur Veröffentlichung.

Die Xbox Live Creators-Programm unterstützt nur das Erstellen von Titeln für das [universelle Windows-Plattform](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) (UWP).  Dieser Titel als UWP-Spiele, die Ausführung auf Windows 10-PCs und Xbox One Konsolen erstellt.  Weitere Informationen zum Ausführen von UWP Spiele für Xbox One finden Sie unter [UWP für Xbox One](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index).  

Auf der Xbox One, die es Spielern ermöglichen eine kuratierte Store-Benutzeroberfläche bietet, werden im Abschnitt neue Creators-Sammlung der Microsoft Store auf der Xbox Spiele, die über die Xbox Live Creators-Programm veröffentlicht verkauft. Dies bietet ein Gleichgewicht zwischen sicherstellen einer offenen Plattform, in dem jeder entwickeln und liefern Sie ein Spiel, und eine kuratierte Store Erfahrung-Konsole, die Spieler kennen und zu erwarten sind. Unter Windows 10 werden Ihre Titel für alle von der anderen Xbox Live-Spiele in den Microsoft Store veröffentlicht.

### <a name="publishing-and-certification"></a>Veröffentlichung und Zertifizierung
Sie müssen registriert werden, die [Partner Center-Entwicklerprogramm](https://developer.microsoft.com/store/register) ein Spiels als Teil der Xbox Live Creators-Programm freigeben. Es gibt zwei Sätze von Anforderungen, die Ihr Spiel befolgen müssen:

1. Integrieren von Xbox Live-Anmeldung, und zeigen Sie die Identität des Benutzers (Gamertag, Gamerpic usw.). Alle anderen Xbox Live-Dienste sind optional.
2. Führen Sie für den Standard [Microsoft Store Richtlinien](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx).

### <a name="supported-xbox-live-services"></a>Unterstützte Xbox Live-Dienste
Titel, die unter dem Xbox Live Creators-Programm aktiviert können Bestenlisten, wichtige Statistiken, Titel, Speicher, Speicher verbunden und einen eingeschränkten Satz von Features für soziale verwenden. Erfolge, online Multiplayer-Spiele und viele soziale Funktionen sind **nicht** für Titel, in dem Xbox Live Creators-Programm unterstützt.

Eine vollständige Liste der unterstützten Dienste, finden Sie unter den [Funktionstabelle](#feature-table).

### <a name="supported-third-party-game-development-engines"></a>Entwicklung von Spielen unterstützten Drittanbieter-engines
Titel des Xbox Live Creators-Programm sind UWP Spiele mit einer Reihe von beliebten Spiel-Engines erstellt werden können. Microsoft bietet die Dokumentation für die Integration von Xbox Live-Dienste in UWP Spiele, die Dank der [Unity Spiele-Engine](https://unity.com). Sie finden [Dokumentation](get-started-with-creators/develop-creators-title-with-unity.md) mit Xbox Live-Integration mit Unity-spielen hier auf dieser Website, ebenso wie herunterladen und erfahren Sie mehr über die Microsoft stammenden [Xbox Live Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin).

Titel des Xbox Live Creators-Programm können auch erstellt werden, mit dem Spiel-Engines [Konstrukt (2 und 3)](https://www.scirra.com/construct2), und [GameMaker Studio 2](https://www.yoyogames.com/gamemaker). Sowohl das Spiel-Engines Xbox Live-Unterstützung hinzugefügt haben, jedoch, dass die Unterstützung durch das Spiel erfolgt-engines Ersteller und nicht von Microsoft. Ausführliche Informationen und Unterstützung für das Hinzufügen von Xbox Live zu Ihrem Projekt erstellen oder GameMaker Studio 2 müssen Sie jedes Spiel-Engines bzw. Dokumentation zu verwenden.

[Informationen Sie zur Integration von Xbox Live in Ihr Projekt erstellen.](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[Informationen Sie zur Integration von Xbox Live in Ihre GameMaker Studio 2-Projekt.](https://www.yoyogames.com/gamemaker/xblc)

Für andere Engines Entwicklung von Spielen wie [MonoGame](https://www.monogame.net/) oder [Xenko](https://xenko.com/), nicht in Xbox Live-Funktionalität oder ein plug-in integriert haben werden, Sie können die Xbox Live-APIs weiterhin verwenden, zum Titel des Xbox Live hinzugefügt werden. Um die Xbox Live-API aus Ihrem Projekt zu verwenden, können Sie Verweise auf die Binärdateien mit NuGet-Paketen oder die API-Quelle hinzufügen. Das Hinzufügen von NuGet-Paketen kann die Kompilierung erleichtern, während das Hinzufügen der Quelle das Debuggen vereinfacht.

### <a name="support-and-feedback"></a>Support und Feedback
Können Sie möglicherweise Fragen beantwortet werden, auf die [MSDN-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev).  Sie können auch stellen, Fragen zum Programmieren [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) mit dem "Xbox-live"-Tag.  Das Xbox Live-Team ist in der Community eingebunden und wird unserer APIs, Tools und Dokumentation auf Grundlage der Bewertung, die es erhält, verbessern.

Für Entwickler in die Xbox Live Creators-Programm können Sie [senden Sie eine neue Idee](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261) oder [abstimmen für eine vorhandene Vorstellung](https://xbox.uservoice.com/forums/251649?category_id=210838) an unsere [Xbox User Voice](https://xbox.uservoice.com/forums/363186--new-ideas).

## <a name="idxbox"></a>ID@Xbox

Die Xbox Live Creators-Programm eignet sich hervorragend für zahlreiche Spiele und Entwickler. Aber wenn Sie die vollständige, z. B. online Multiplayer-Spiele, Erfolge und Gamerscore, Xbox Live Stack zugreifen möchten oder Sie das volle Potenzial von der Xbox One-Familie von Geräten mithilfe des Kits, die Hardware Dev zugreifen möchten die [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) Programm wird für Sie.

Spiele der ID@Xbox Programm muss Konzept genehmigt werden, und navigieren Sie über vollständige-Zertifizierung für Xbox One und Windows 10, die ihrerseits mehr Zeitaufwand.
ID@Xbox Titel erhalten Platzierung in der primären Teil der Store, und der Ersteller-Auflistung, die höhere Sichtbarkeit von Daten für Kunden ermöglichen kann.

Entwickler in die ID@Xbox Programm auch Zugriff auf Werbe-Unterstützung und Support für Entwickler von Microsoft als auch das vollständige Komplement des privaten Whitepapers und Entwickler technische Foren. Sie können weiterhin verwenden [MSDN-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev) , oder bitten Sie die Programmierung von Fragen auf [Stack Overflow](https://stackoverflow.com/questions/tagged/xbox-live) unter Verwendung des "Xbox-live"-Tags, wenn Sie möchten.

## <a name="microsoft-partners"></a>Microsoft-Partner

Entwickler arbeiten mit einem Spiel Verleger, die Microsoft-Partner haben Zugriff auf den vollständigen Satz von Xbox Live-Funktionen und dedizierte Vertreter von Microsoft zur Unterstützung Ihrer Entwicklung, Zertifizierungen und Freigabeprozess.

## <a name="feature-table"></a>Feature-Tabelle

Die folgende Tabelle zeigt die Features für die Xbox Live Creators-Programm und [ ID@Xbox ](https://www.xbox.com/en-US/developers/id) Programme.  

<table>

<tr>
<th>Funktionsbereich</th>
<th>Feature</th>
<th>Beschreibung</th>
<th> ID@Xbox </th>
<th>Xbox Live Creators-Programm</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">Identität</td>
<td>Anmeldung und Registrierung</td>
<td>Können Sie Player, Xbox Live in der Titelleiste Ihres anmelden, oder erstellen Sie ein neues Xbox Live-Konto aus, bei Bedarf</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-required">Erforderlich</td>
</tr>

<tr>
<td>Identität des Benutzers</td>
<td>Verwenden von Xbox Live-Identität durch Anzeigen der Gamertag, Gamerpic usw.</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-required">Erforderlich</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">Gemeinschaft</td>

<td>Grundlegende vorhanden</td>
<td>Anzeigezeichenfolgen Sie grundlegende Anwesenheit-Benutzeraktivität in einen Titel anzeigt.  Beispiel: "Steve ist Minecraft spielen"</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Vor kurzem wiedergegeben.</td>
<td>In zuletzt wiedergegebenen Titel in der Xbox-App oder deiner Xbox One angezeigt werden</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Aktivitätsfeed</td>
<td>In der Aktivität, die im Xbox-App oder deiner Xbox One-feed angezeigt werden</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Spiele-Hub</td>
<td>Verknüpft einen Hub Spiel durch den Titel anzeigen von Statistiken, Videos und andere Inhalte in einem speziell für Titel des Feed haben</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Kreuz</td>
<td>Spieler können die Xbox-App oder deiner Xbox One um Kreuz zu erstellen, die optional durch den Titel zugeordnet werden können.</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Suchen nach Gruppe (LFG)</td>
<td>LFG kann Spieler, um eine andere Out-von-Spiel zum Planen eines Multiplayer-Spiels zu finden.</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>GameDVR</td>
<td>Spieler können Videos die Gaming-Sitzungen erfassen und teilen diese auf die feed-Aktivität.</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Übertragung</td>
<td>Spieler können ihre Spiele per streaming-Dienste wie Mixer und twitch integrierte broadcast live.</td>
<td class="xbl-features-automatic">Automatisch</td>
<td class="xbl-features-automatic">Automatisch</td>
</tr>

<tr>
<td>Umfassende Präsenz</td>
<td>Zeigt ausführliche Informationen zu den Spielern im Titel.  Während der grundlegenden Anwesenheit angezeigt "Benutzer wird im Auto Racing Game", das Vorhandensein von Rich können Sie angeben, wie eine ausführlichere Zeichenfolge werden "Benutzer SuperCar in RainyForest driving"</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Freunde</td>
<td>Abgerufen Sie die Anmeldung des Benutzers Freundesliste zum Aktivieren von social Gaming-Szenarien, in dem Titel des werden.</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-limited">Optionale / beschränkt (nur für Freunde, die Titel des wiedergegebenen werden verfügbar gemacht)</td>
</tr>

<tr>
<td>Vertraulichkeit</td>
<td>Lassen Sie Spieler auf stumm- oder zu blockieren oder anderen Spieler zu</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-optional">Optional</td>
</tr>

<tr>
<td>Bewertung</td>
<td>Spieler erhalten oder verlieren Reputation über deren Verhalten. Verhalten wird in der Vermittlung verwendet und nach Ihrem Titel auf benutzerdefinierte Weise verwendet werden kann.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Soziale Medien-Manager</td>
<td>Informationen zu social Graph des Spielers effizient abrufen</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-limited">Optionale / beschränkt (nur für Freunde, die Titel des wiedergegebenen werden verfügbar gemacht)</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">Data Platform</td>

<td>Spielerstatistiken</td>
<td>Hochladen von Statistiken zu den Spielern, die in der Bestenlisten verwendet werden kann.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-optional">Optional (nur Datenplattform 2017)</td>
</tr>

<tr>
<td>Wichtige Statistiken.</td>
<td>Bestimmte Statistik als "Ausgewähltes Stats" festlegen, werden in der Game-Hub angezeigt.</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-optional">Optional (nur Datenplattform 2017)</td>
</tr>

<tr>
<td>Bestenlisten</td>
<td>Abrufen und Player Statistiken auf eine Weise sortiert wie Wettbewerbs anzeigen.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-optional">Optional (nur Datenplattform 2017)</td>
</tr>

<tr>
<td>Erfolge mit Gamerscore</td>
<td>Bestimmte Statistik als "Ausgewähltes Stats" festlegen, werden in der Game-Hub angezeigt.</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">Media</td>

<td>Kontextbezogene Suche</td>
<td>Kommentieren Sie GameDVR Clips mit Schlüsselwörtern, um ihn leichter für Spieler Clips entspricht, was sie überwachen möchten gefunden.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">Speicher</td>

<td>Onlinespeicher</td>
<td>Roaming Spiel gespeichert, auf eine Xbox-Konsolen und PCs</td>
<td class="xbl-features-required">Erforderlich</td>
<td class="xbl-features-optional">Optional</td>
</tr>

<tr>
<td>Titelspeicher</td>
<td>Cloudspeicher für große Mengen von pro Benutzer oder pro-Titel-Daten.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-optional">Optional</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">Online Multiplayer-Spiele</td>

<td>Multiplayer Session Directory (MPSD)</td>
<td>Speichert Informationen über eine Multiplayer-Sitzung, wie z. B. die Liste der Spieler, Status usw.</td>
<td class="xbl-features-optional">Erforderlich</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Vermittlung</td>
<td>Xbox Live können verschiedene Spieler zusammen für eine Multiplayer-Sitzung überein.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Arena</td>
<td>Spieler können Turnier Stil miteinander konkurrieren.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Spiele-Chat</td>
<td>Voice-Chat für Spieler eines Multiplayer-Spiels</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

<tr>
<td>Xbox Live-Serverdienst</td>
<td>Stellen Sie ausführbare Dateien und Ressourcen, die der Titel für die Kommunikation mit kann zum Auslagern der Berechnung auf dem Client.</td>
<td class="xbl-features-optional">Optional</td>
<td class="xbl-features-notavailable">Nicht unterstützt</td>
</tr>

</table>
