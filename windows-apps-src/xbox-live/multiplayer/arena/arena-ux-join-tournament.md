---
title: Verknüpfen Sie ein Turnier
description: Informationen Sie zum Erstellen der Benutzeroberfläche für Spieler des Spiels Turniere zu verknüpfen.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, -Turniers, ux
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613505"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>Verknüpfen Sie ein Turnier mithilfe der Spielbereich-Benutzeroberfläche

Der Spielbereich-APIs können Ihre Titel mit Daten zur Registrierung, Einchecken und Teaminformationen, aber sie bieten *nicht* die Funktionalität, *ausführen* verwandten Aufgaben. Titel des wird der Spielbereich-Benutzeroberfläche oder ein Drittanbieter-Turniers-Planer (an) verwenden, oder erstellen Sie ihre eigene Turnier-Management-Unterstützung erwartet.

## <a name="xbox-arena-ui-team-formation"></a>Xbox-Spielbereich Benutzeroberfläche: Team-Clusterbildung

Der Spielbereich-Benutzeroberfläche bietet eine Methode für die es Spielern ermöglichen, Formular oder eine Verknüpfung von einem Team. Besteht keine Notwendigkeit für den Titel.

###### <a name="ui-example-create-a-team"></a>Beispiel der Benutzeroberfläche: Erstellen eines Teams

![Bilden Sie einen Team-Bildschirm](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>Wenn ein Team gebildet wird, kann ein Spieler:

* Senden von Einladungen an Freunde oder Club Mitglieder hinzuzufügen.
* Finden Sie durch das Veröffentlichen einer LFG Ad Teammitglieder.
* Registrieren oder Aufheben der Registrierung eines Teams.
* Entfernen Sie ein Mitglied eines Teams.

## <a name="xbox-arena-ui-registration"></a>Xbox-Spielbereich Benutzeroberfläche: Registrierung

Der Spielbereich-Benutzeroberfläche bietet eine Methode für die es Spielern ermöglichen, ein Team zu registrieren. Besteht keine Notwendigkeit für den Titel.

###### <a name="ui-example-register-a-team"></a>Beispiel der Benutzeroberfläche: Registrieren Sie ein team

![Registrieren Sie einen Team-Bildschirm](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>Bei der Registrierung für ein Turnier können ein Benutzer aus:

* Aufheben der Registrierung für ein Turnier *vor* Registrierung wurde geschlossen.
* Aufheben der Registrierung eines Teams auf Einchecken oder wenn der einzelnen turnierteilnehmer ausgeführt wird.
* Aufheben der Registrierung eines ganzen Teams. *Beachten Sie, dass ein einzelner Benutzer verlassen eines Teams hebt die Registrierung für des gesamten Teams.*
* Registrieren Sie als eine Captain.
* Verstehen der Anforderungen des Turniers und Verhaltensregeln vor dem registrieren:
* Erhalten Sie eine Benachrichtigung, dass die Registrierung erfolgreich abgeschlossen wurde.
* Der Turnier-Status in Echtzeit zu ändern, um "registriert" angezeigt.
* Vorschau der Klammer zum Zeitpunkt, wenn, den ein Turnier gestartet wird.
* Finden Sie unter Spieler, die bereits für das Turnier registriert haben.
* Gehindert werden, registrieren, wenn sie die Turnier Bedingungen nicht erfüllen.
* Gehindert werden, registrieren, wenn ein Player gesperrt hat.

> [!div class="nextstepaction"]
> [Engagement übereinstimmen](arena-ux-match-engagement.md)
