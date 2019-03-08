---
title: Multiplayer-Szenarien
description: Informationen Sie zu den verschiedenen Multiplayer-Szenarien und Xbox Live, auf wie dieser Szenarien unterstützt.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 338c6e65846ca0ce1307e1e5843f37fe63ecf70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618295"
---
# <a name="multiplayer-scenarios"></a>Multiplayer-Szenarien
Es gibt viele verschiedene Arten von multiplayer-Szenarien, und Auswählen des richtigen Szenarios lässt sich die Player-Engagement, und die Basis für Ihr Spiel, wodurch wiederum Player erweitern die aktive Lebensdauer Ihres Spiels so lange wie möglich.

Sogar Spiele, die größtenteils einzelne playerideen profitieren von wettbewerbsfähig Xbox Live-Features wie Bestenlisten, Statistiken oder soziale Elemente.

Die folgende Liste beschreibt einige der häufigeren Multiplayer-Szenarien, die Sie, mit der Xbox Live implementieren können. Die Liste sortiert wird, im Hinblick auf Komplexität und Arbeitsaufwand zum Implementieren und testen, und Sie sollten diese Faktoren auf, wenn Szenarios festlegen, dass Sie Ihr Spiel unterstützen möchten.

## <a name="comparative-indirect-play"></a>Komparativen (indirekten) wiedergeben
In diesem Szenario werden Spieler miteinander indirekt ohne direkte Gameplay in derselben Instanz eines Spiels Wettbewerb. Es eignet sich gut für Spiele, die auf einen einzigen Player-Benutzererlebnis sorgen ausgerichtet sind, aber enthalten einige wettbewerbsfähig Aspekte, mit denen Spieler verglichen werden soll, andere Spieler wie gewohnt.

Beispiel-Funktionalität für dieses Szenario umfasst:

* Bestenlisten, bei denen ein Spieler versucht, erreichen das beste "Ergebnis" in einer Kategorie relativ zu anderen Spieler. Dies kann ein social Leaderboard-System für konkurrierende mit Freunden nur einschließen.
* Erfolge und Statistiken, die, in dem ein Spieler seinen Fortschritt/Leistung mit Freunden vergleichen kann, und möglicherweise möchte über ein besonders schwieriges Auszeichnung Beats stolz sein.
* "Ghosting" oder "virtuelle Multiplayer", in denen ein Spieler anhand einer Aufzeichnung eines anderen der Spieler konkurrieren kann (oder ihre eigenen) vorherigen Leistung zu erzielen, z. B. eine runde im ein-autorennspiel.

Diese Art von Multiplayer-Spiele kann mit den folgenden Xbox Live-Diensten erreicht werden:

* Präsenz
* Stats
* Soziale Medien-Manager
* Erfolge
* Bestenlisten
* Verbundenen Speicher

Diese Art von Multiplayer erfordert keine bestimmte Xbox Live Multiplayer-Dienste. Testen dieses Szenario erfordert mehrere Xbox Live-Konten nur.

## <a name="local-play-living-room-play"></a>Lokale Wiedergabe (Wiedergabe Wohnzimmer)
Diese Art von Multiplayer-Spiele auf zwei oder mehr Spieler, ein Spiel zusammen (entweder im Vergleich zu anderen oder Kooperativ) basiert auf einem einzelnen Gerät. Ein Titel können einen einzelnen Bildschirm für alle Spieler oder eine Split-Bildschirm-Erfahrung für jeden Spieler. Turn-basierte Spiele, können Sie alternativ einen "Ebene" heiß "Arbeitsplatz" Ansatz verwenden, in dem jeder Spieler übernimmt die Kontrolle des Spiels bei der die Reihe.

Klicken Sie auf der Xbox One-Konsole können mehrere Spieler auf einer einzigen Konsole angemeldet sein. Jeder Spieler ist mit einem Controller verknüpft. Derzeit werden Windows 10-Geräte unterstützen nur einem einzelnen Xbox Live-Konto anmelden, aber Microsoft untersucht Ändern dieser Eigenschaft in einem zukünftigen Update.

Es ist, zwar möglich, einen einzige lokale Play-Stil, der Multiplayer-mithilfe des Xbox Live Multiplayer-Diensten zu entwerfen ist es möglicherweise besser, dieses Szenario als Teilmenge eines vereinfacht Multiplayer-Szenarios zu berücksichtigen, die online-Erfahrungen, als die zusätzlichen enthält erforderlichen Investitionen ist gering im Vergleich zu den potenziellen Rendite Multiplayer-Szenario zu erweitern.

Diese Art von Multiplayer-Spiele können ähnliche Dienste wie im vorherigen Szenario:

* Präsenz
* Stats
* Soziale Medien-Manager
* Erfolge
* Bestenlisten
* Verbundenen Speicher

Testen dieses Szenario erfordert, mehrere Xbox Live-Konten und mehrere Controller auf einem einzelnen Gerät.

##  <a name="online-play-with-friends"></a>Online-Spiel mit Freunden
Dieses Szenario ist die traditionelle multiplayer online-Erfahrung. In diesem Szenario ein Xbox Live-Element zu einem Spiel mit Freunden nur möchte, aber nicht mit fremden wiedergeben möchte. Friends sind eingeladen, entweder oder ausgeführt wird, Mitglied einer laufenden Spiel.

Für die Altersfreigabe müssen ein breiterer Multiplayer-Titel (wie weiter unten gezeigt) die Möglichkeit, online Multiplayer-Spiele, Freunde nur beschränkt, und ein Fallback auf dieses Szenario. Jugendschutz, die die Interaktionen mit fremden einschränken, sind auch über den Xbox Live-Dienst erzwungen.

Diese Art von Multiplayer-Spiele kann mit den folgenden Xbox Live-Diensten erreicht werden:

* Multiplayer-Manager
* Präsenz
* Stats
* Soziale Medien-Manager

Testen dieses Szenarios erfordert mehrere Xbox Live-Konten und mehrere Geräte.

## <a name="online-play-through-a-list-of-game-sessions"></a>Online-Erlebnis über eine Liste der game-Sitzungen
In diesem Szenario kann ein Spieler durchsuchen Sie die Liste der beigetreten werden kann, Gaming-Sitzungen in einem Spiel und klicken Sie dann auswählen, welche verknüpfen. Der Spieler hat auch die Möglichkeit, neue Spiele Sitzung Instanzen zu erstellen, indem Sie ein Spiel lokal hosten. Diese spielen Instanzen können benutzerdefinierte Einstellungen (z. B. Spielmodus, Ebene oder spielen Regeln). Je nach den Title-Entwurf unterstützen Spiele Sitzungen möglicherweise Einschränkungen wie das Anfordern eines Kennworts zum Join oder bestimmte Player/Fähigkeiten. Diese Instanzen Spiel Sitzung können auch sein, vollständig öffentlichen oder ausgeblendet ist, je nachdem, wie Ihr Spiel für die Sitzung, durchsuchen und den Beitritt implementiert.

Dieses Szenario ermöglicht es, Spieler, um eine Spiele-Sitzung, selbst auswählen. Diese übergibt die Kontrolle an den Spieler, aber es gibt keine Garantie, dass eine Sitzung eine gute Erfahrung bereitstellen. Eine Sitzung kann mit dem richtigen Player eine interessante Fähigkeit-Ebene festgelegt werden, nicht aufgefüllt. Eine Sitzungsliste ist am effektivsten, wenn spielen eine kleine aktive Multiplayer-Community verfügen.

Diese Art von Multiplayer-Spiele können ähnliche Dienste wie im vorherigen Szenario:

* Multiplayer-Manager
* [Durchsuchen von Sitzungen](session-browse.md)
* Präsenz
* Stats
* Soziale Medien-Manager

Testen dieses Szenario erfordert einen großen Satz von Xbox Live-Konten und Geräte, um genau zu testen. Entwickler müssen auch beachten, dass es sich bei "true" Player Dynamics für Sitzung Listen nur mit großer Player Basen getestet werden kann.

## <a name="online-play-though-looking-for-group"></a>Onlinespiel jedoch für die Gruppe suchen
Dieses Szenario ist in einem Szenario mit Sitzungsliste ähnlich, jedoch er von diesem in wichtigen Punkte weicht. Anstatt eine Spiele-Liste im Titel bietet die Plattform Funktionen zur Liste Spiel Sitzungen außerhalb des Spiels. Diese Ankündigungen für die Gruppe suchen sollen weitere soziale Netzwerke benutzerfreundlichkeit und umfassen Gaming, Fähigkeiten und Einschränkungen für soziale Netzwerke Beziehung. Dadurch wird ein Spiel bereitstellen, eine größere benutzerfreundlichkeit über Sitzung aufgelistet, und auch dem Ersteller der Sitzung mehr Kontrolle bietet.

Der Spieler, der die Sitzung "Gruppe suchen" erstellt kann genehmigen oder ablehnen von Anforderungen an ihre Gruppe von anderen Spielern beitreten. Dies ermöglicht es Spielern ermöglichen, andere Spieler zu suchen, die ihre Präferenzen Playstyle gemeinsam nutzen.

Die Xbox Live LFG-Dienst ist für das Jahr 2016 neu, und Preview-APIs im Mai für Titel verfügbar sein sollen.

Diese Art von Multiplayer-Spiele können ähnliche Dienste wie im vorherigen Szenario:

* Multiplayer-Manager
* Xbox nach Gruppe
* Präsenz
* Stats
* Soziale Medien-Manager
* LFG-Dienst

Testen dieses Szenarios muss ein breites Spektrum an Xbox Live-Konten und Geräten zu testen.

## <a name="simple-matchmaking"></a>Einfache Vermittlung
In diesem Szenario sucht einen Player (oder eine Gruppe von Spielern) nach anderen Spieler (die möglicherweise möglicherweise nicht an den Spieler bekannt) für ein Onlinespiel entwickeln. Anstelle von Freunden, bietet das Spiel einen einfachen Vermittlung Flow, der Spielern genutzt in Spiele-Sitzungen mit anderen Spielern ermöglicht. Der Flow Vermittlung, in diesem Szenario ist einfach: Spieler suchen, und finden Sie weitere Entwicklungen ohne erhebliche Vermittlung Konfiguration. Dieses Szenario funktioniert am besten mit einer größeren online Zielgruppe.

Um Spieler zusammen entsprechend abzugleichen, sollten alle Vermittlung Reputation berücksichtigt, und Blockieren von Listen auf Xbox Live. Diese Einschränkungen wird automatisch von der Xbox Live SmartMatch-Dienst übernimmt. Einschränkungen wie diesen kann sicher eine sicherere und besseres Benutzererlebnis für Spieler zu berücksichtigen.

Ein wichtiger Bestandteil der Vermittlung sind Netzwerke Überprüfungen von Quality of Service (QoS). Diese Überprüfungen stellen Sie sicher, dass die Netzwerkkonnektivität zwischen zwei Spielern auszutauschen für eine gute Gaming-Umgebung ausreichend ist. Dies ist anders als bei der Online experimentieren Sie mit Freunden-Szenario, da es möglich ist, wiederholen die Vermittlung, und finden weitere Entwicklungen bei eine fehlerhafte Netzwerkverbindung.

Diese Art von Multiplayer-Spiele können ähnliche Dienste wie im vorherigen Szenario:

* Multiplayer-Manager
* Präsenz
* Stats
* Soziale Medien-Manager
* SmartMatch Vermittlung

Testen dieses Szenario erfordert einen großen Satz von Xbox Live-Konten und Geräte, um genau zu testen. Entwickler müssen auch beachten, dass es sich bei "true" Player Dynamics für Sitzung Listen nur mit großer Player Basen getestet werden kann. Tools zur Verfügung, um Netzwerk-Bedingung und SmartMatch testen zu vereinfachen.

## <a name="skill-based-matchmaking"></a>Fertigkeitsbasierter Vermittlung
Vermittlung fertigkeitsbasierter ist eine Verfeinerung des einfachen Vermittlung-Szenario. In diesem Szenario die Vermittlungsdienst enthaltenen Regelsätze erweiterte wie Fähigkeiten, Spielerebene und andere Spiel-spezifische Eigenschaften. Der Vermittlungsdienst verwendet Regeln, die diese verwenden Funktionsparametern finden Sie eine Sitzung mehr hoher Qualität für den Spieler übereinstimmen. Je nach Entwurf wird Parameter für aktivitätsübereinstimmung werden direkt vom Player konfiguriert, oder Sie werden automatisch festgelegt, durch den Titel.

Xbox Live SmartMatch bietet eine Reihe von Regeln, die für die Vermittlung fertigkeitsbasierter verwendet werden kann. Der SmartMatch-Dienst verwendet eine Xbox Live-Funktion wird aufgerufen, TrueSkill relative Qualifikation von einem Player Auswertung.

Diese Art von Multiplayer-Spiele können ähnliche Dienste wie im vorherigen Szenario:

* Multiplayer-Manager
* Präsenz
* Stats
* Soziale Medien-Manager
* SmartMatch Vermittlung

Testen dieses Szenarios muss ein breites Spektrum an Xbox Live-Konten und Geräten. Entwickler müssen auch beachten, dass es sich bei "true" Player Dynamics für Sitzung Listen nur mit großer Player Basen getestet werden kann. Es stehen Tools zur Vereinfachung der Netzwerk-Bedingung, TrueSkill und SmartMatch testen.

## <a name="tournaments"></a>Turniere
Turniere können erfahrene Spieler in einem Format organisiert miteinander konkurrieren. In der Regel sind Turniere gehostet und von Organisationen mit unabhängigen Drittpartei organisiert.

Xbox Live wurde ein neues Feature in 2016, um den Titel für die Vermittlung Xbox Live zusammen mit genehmigten Drittanbieter-Turniers Organisatoren verwenden zu können hinzugefügt werden. Dieses Feature ermöglicht Titeln zu Turniere nahtlos in die Xbox-Shell-Benutzeroberfläche zu integrieren, ohne dass der Spieler, um in externe Websites zu registrieren.

Diese Art von Multiplayer-Spiele kann mithilfe von Xbox Live-Dienste wie z. B. erreicht werden:

* Präsenz
* Stats
* Gemeinschaft
* Bewertung
* Multiplayer
* SmartMatch Vermittlung
* Turniere

Testen dieses Szenarios muss ein breites Spektrum an Xbox Live-Konten und Geräten.
