---
title: Xbox Live-Konto-Tool
description: Erfahren Sie, wie Sie das Xbox Live-Konto-Tool zu verwenden, um schnell Testkonten zum Testen der Titel des Xbox Live aktiviert zu erstellen.
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Tests, Testkonten
ms.localizationpriority: medium
ms.openlocfilehash: dead4e62e41b7b597ba9a578ee8f174386529937
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632825"
---
# <a name="xbox-live-account-tool"></a>Xbox Live-Konto-Tool

## <a name="what-is-xbox-live-account-tool"></a>Was ist Xbox Live-Konto-Tool?
Das Tool des Xbox Live-Konto ist ein Tool, vorhandene Dev-Konten für Spiele Testszenarien einrichten Title-Entwicklern dabei helfen. Sie können z. B. Xbox Live-Konto-Tool verwenden, um ein Entwicklerkonto Gamertag zu ändern oder schnell 1000 Follower Freundesliste des Kontos hinzufügen.

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>Was kann ich mit Xbox Live-Kontotool tun?
Sie haben folgende Möglichkeiten:
  1. Profileinstellungen, XUID und aktiven Berechtigungen eines Benutzers anzeigen
  2. Fügen Sie eine Liste der Follower eines Benutzers soziale Diagramm, aus einer Textdatei oder einer Xbox-Entwickler-Plattform-CSV-Datei
  3. Verwalten eines Benutzers Freundesliste: Favoriten, als Favorit löschen, blockieren und Entsperren von Benutzern, die Sie befolgen und festzustellen, ob sie Sie erneut ausführen
  4. Ändern des Benutzers Ihre Dev Reputation (und die unformatierte Bewertung Stat Werte unmittelbar)
  5. Ändern eines Benutzers gamertag

## <a name="where-can-i-find-xbox-live-account-tool"></a>Wo finde ich die Xbox Live-Kontotool?
Das Xbox Live-Konto-Tool finden Sie im Rahmen des Xbox Live-Tools-Pakets von [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

## <a name="how-do-i-log-in"></a>Wie melde ich mich an?
Sie benötigen die Anmeldeinformationen des Benutzers, die Sie verwalten, und geben Sie die richtige Sandbox möchten. Stellen Sie sicher, dass das Dev-Konto hat Zugriff auf die Sandbox, die andernfalls möglicherweise die Anmeldung. Das Tool wurde mit Dev-Konten, die unter Verwendung eines Sandkastens Bedenken konzipiert.

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>Kann ich ein Konto im Einzelhandel verwenden, oder gibt es eine Sandbox-Kontos werden?
Natürlich können Sie Xbox Live-Konto-Tool zum Verwalten eines Kontos im Einzelhandel, aber nicht alle Funktionen werden unterstützt. Sie können z. B. eines Benutzers im Einzelhandel Reputation nicht ändern.

## <a name="how-do-i-change-a-dev-users-gamertag"></a>Wie ändere ich eines Dev-Benutzers Gamertag?
Navigieren Sie zur Registerkarte "Gamertag" aus, und geben Sie einen Gamertag. Gamertags darf nur Buchstaben, Zahlen und Leerzeichen enthalten und kann nur 15 Zeichen lang sein. Developer-Konto Gamertags muss mit einem 2 beginnen. Nur eine Änderung wird derzeit unterstützt.

## <a name="how-do-i-see-my-block-list"></a>Wie werden meine Sperrliste angezeigt?
Navigieren Sie zur Registerkarte "People", und wählen Sie die Spaltenheader "Blockiert", um die von Benutzern zu sortieren, die derzeit blockiert werden.

## <a name="how-do-i-follow-a-large-group-of-users"></a>Wie führen ich eine große Gruppe von Benutzern?
Wenn Sie eine Liste der XUIDs, die Sie ausführen möchten verfügen, kopieren Sie diese in eine Textdatei ein. Navigieren Sie zur Registerkarte "Folgen", wählen Sie "Import List", und wählen Sie die Datei. Die XUIDs sollten in der Listenansicht gefüllt. Klicken Sie auf "Änderungen", die Benutzern folgen.

## <a name="how-do-i-block-someone"></a>Wie blockiere ich eine Person?
Navigieren Sie zur Registerkarte "People", und wählen Sie den oder die Benutzer, die Sie blockieren möchten. Klicken Sie auf die Schaltfläche "Block", und sie werden in Batches blockiert. Wenn Sie einen Fehler feststellen, wiederholen Sie dann einfach später noch mal.

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>Wie ändere ich meine Entwicklerkonto Repuation?
Navigieren Sie zur Registerkarte "Bewertung". Wählen Sie die Standardwerte, die Sie möchten, die und drücken Sie die Schaltfläche "Änderungen", um die Anforderung zu senden. Sie sehen die Stat Ruf, die Werte ändern, wenn die Anforderung erfolgreich ist.

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>Was bedeuten die Werte in der Registerkarte "Bewertung"?
Ruf wird insgesamt aus drei untergeordneten Ruf berechnet: FairPlay (Multiplayer-durchführen), benutzergeneriertem Inhalt (Videoclips usw.) und Kommunikation (Nachrichten und Sprache). Die unformatierte Werte für jede Kategorie können Bereich von 0 bis 75, besser, in denen höhere bedeutet, dass der Benutzer die Bewertung. Die OverallStatIsBad darüber informiert, ob der Benutzer "Zu vermeiden Me" Ruf hat.

## <a name="whats-the-black-area-at-the-bottom"></a>Was ist die schwarze im unteren Bereich?
Damit um Sie die darüber informiert zu bleiben, dachten wir uns an, dass es hilfreich wäre, wenn Debugausgabe in der Benutzeroberfläche ausgegeben. Bereich wird informieren Sie, was das Tool bis zu und Drucken alle auftretenden Fehler auftreten.

## <a name="wheres-my-gamerpic"></a>Wo ist mein Gamerpic?
Uns ist bewusst, der einen Fehler, den einige Dev-Konten kein automatisch generierter zum Zeitpunkt der Erstellung Konto Gamerpic erhalten.

## <a name="why-are-things-happening-so-slowly"></a>Warum sind die Dinge so langsam durchgeführt?
Für die Batchvorgänge (z. B. das Hinzufügen von followern) führt das Tool automatisch Batches, um große Anforderungsgrößen zu verhindern.

## <a name="how-do-i-run-xbox-live-account-tool"></a>Wie führe ich die Xbox Live-Kontotool aus?
Extrahieren von Xbox Live SDK in einen Ordner, und doppelklicken Sie auf die Tools\XboxLiveAccountTool.exe-Datei, um es auszuführen.

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>Ich habe einen funktionswunsch! Wie erhalte ich mein Feature integriert?
Wenden Sie sich an Ihren Ansprechpartner bei Microsoft mit featureanforderungen, und wir sehen, was wir tun können.
