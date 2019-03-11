---
title: Aktualisierte Flows für Multiplayer-Spiele-Einladungen
description: Stellt aktualisierte Datenflüsse für die Implementierung von Xbox Live, Multiplayer-Spiels Einladungen, an.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Multiplayer-2015
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596885"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>Aktualisierte Flows für Multiplayer-Spiele-Einladungen

Als Ergebnis der Xbox One Feedback zur Betaversion wurde den benutzeroberflächenschritt für Multiplayer-Spiels eingeladen zum Zeitpunkt der Xbox eine Wiederherstellung Update 24, veröffentlicht am 6. November 2013 geändert. Dies ist eine Änderung an der **benutzererfahrung (UX) nur** und wirkt sich keine Verhalten oder die Funktionen aus der Perspektive eines Spiels Titels. Titel-Entwickler müssen keine Änderungen am Code vornehmen.

## <a name="summary-of-changes"></a>Zusammenfassung der Änderungen

-   Der erste Einladung Toast wurde von "Meine Partei join" in geändert "&lt;spielen Titel&gt; los geht's" und verfügt nun über eine Schaltfläche, mit dem Benutzer das Spiel zu starten, und gehen direkt Spiel.

-   Die Partei-app wird nicht angedockten Zustand in der Standardeinstellung, wenn der Benutzer die neue "&lt;spielen Titel&gt; los geht's" Option. Diese Änderung erfolgt auch, damit der Benutzer direkt in das Spiel springen kann.

-   Auf der Absenderseite, die besagt, wurde ein neues Popup hinzugefügt "Hinzufügen von \[ *Anzahl* \] von Freunden das Spiel". Dadurch wird deutlich, die Sie Einladungen gesendet wurden, wenn eine Spiele-Sitzung eines Benutzers Partei zugeordnet ist.

Die ausführliche Erfahrung benutzerflows werden in den folgenden Beispielen beschrieben. Jede Tabelle zeigt einen Beispiel für Datenfluss für zwei Benutzer, David und Laura. Diese Flows werden in den einzelnen Spalten angezeigt und werden in gleichzeitig ausgeführt. Die <b style="background-color: #FFFF00">hervorgehobenen Text</b> zeigt die Anpassungen, die aus dem vorherigen UX übertragen vorgenommen wurden.

## <a name="inviting-users-from-within-a-game"></a>Einladen von Benutzern in einem Spiel

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David befindet sich im Empfangsbereich Multiplayer-bei einem Spiel, die er wiedergegeben wird.<p><br>David wählt <b>einen Freund einladen</b>.<p><br>David wählt Laura.<p><br>Popupbenachrichtigung angezeigt, der angibt, dass eine Einladung gesendet wurde. | Laura wird einem anderen Spiel gespielt.
    </td>
    <td style="border-bottom:solid 1px #fff">
Laura wird einem anderen Spiel gespielt.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Popup eingeblendet wird, der angibt, der einer einladungs von David und <b style="background-color: #FFFF00">zeigt an, der Name des Spiels und das Symbol</b>. (Die app für die Partei wird nicht automatisch-Snap.) <p><br>
In der Mitteilungszentrale <b style="background-color: #FFFF00">Laura kann starten und akzeptieren der Einladung</b>, <b>Einladung annehmen</b>, oder <b style="background-color: #FFFF00">ablehnen einladen</b>.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">Fall 1: Laura wählt aus starten, und akzeptieren der Einladung</b> (Dies ist eine neue Option)</td>
  </tr>
  <tr>
    <td>
Popupbenachrichtigung angezeigt, der angibt, dass Laura Davids Partei angehört.             
    <p><br>
David startet Multiplayer-Lobby das Spiel.                              
    <p><br>
    <b style="background-color: #FFFF00">Popupbenachrichtigung angezeigt, der angibt, dass eine Spiele-Einladung an Laura gesendet wurde.</b>
    </td>
    <td>
Das Spiel startet, und die Partei-app wird nicht ausgerichtet.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>Fall 2: Laura wählt Accept-Einladung</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura verknüpft die Partei.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">Popupbenachrichtigung angezeigt, der angibt, dass eine Spiele-Einladung an Laura gesendet wurde.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>Popupbenachrichtigung angezeigt, dass ein Spiel für die Partei nicht gefunden wurde.
    <p><br>
In der Mitteilungszentrale kann Laura auswählen: <ul>
    <li>   <b>Spiele Einladung annehmen:</b> Spiel startet.
    <li>   <b>Spiele Einladung ablehnen:</b> Ohne Spiel wird gestartet. Laura befindet sich noch in die Partei, und nachfolgende Spiele Einladungen erhalten.         
    <li>   <b style="background-color: #FFFF00">Lassen Sie die Partei: Ohne Spiel wird gestartet. Laura wird von der Partei entfernt.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>Einladung von in-Game-Ablauf: In einer Partei, und wechseln Titel

<table>
  <tr>
    <td>
Ein Spiel zusammen.
    <p><br>
David wechselt in einen anderen Spiel und geht zum Multiplayer-Menü.
     <p><br>
Die Xbox-System-Benutzeroberfläche fordert David aus, wenn er seine Partei in der neuen Spiels wechseln, kann er nicht beantworten möchten <b>Ja</b> oder <b>keine</b>.
    </td>
    <td style="text-align:top">
Ein Spiel zusammen.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Fall 1: Ja
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
Partei geht auf den neuen Titel.
    <p><br>
In der Multiplayer-Lobby startet David das Spiel.
    <p><br>
    <b style="background-color: #FFFF00">Popupbenachrichtigung angezeigt, der angibt, dass eine Spiele-Einladung an Laura gesendet wurde.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Popupbenachrichtigung angezeigt, dass ein Spiel für die Partei nicht gefunden wurde.
    <p><br>
Aus der Mitteilungszentrale kann Laura auswählen: <ul>
     <li><b>Spiele Einladung annehmen</b>: Neues Spiel wird gestartet. <li><b>Spiele Einladung ablehnen:</b> Ohne Spiel gestartet wird, aber Laura befindet sich noch in die Partei, und nachfolgende game-Einladungen erhält.
     <li><b style="background-color: #FFFF00"><b>Lassen Sie die Partei:</b> Keine Spiele und Laura wird von der Partei entfernt.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Fall 2: Nein
    </td>
  </tr>
  <tr>
    <td>
Partei kommt nicht zu den neuen Titel.
David spielt Multiplayer-Modus ohne seine Mitglieder Partei.
David befindet sich noch in der Partei.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>Laden Sie Flow von zu Hause

<table>
  <tr>
    <td>
David durchsucht <b>Startseite</b>, und in seinem <b>Freunde</b> Liste, er sieht, dass Laura online ist.
    <p><br>
David wählt, Laura an seine Partei einzuladen. Popupbenachrichtigung angezeigt, der angibt, dass die INVITE-Nachricht gesendet wird. Die app für die Partei für David Rasterlinie ausgerichtet.
    </td>
    <td>
Laura wird ein Spiel gespielt.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Gibt an, dass David Laura an seine Partei eingeladen hat, ist Toast angezeigt.
    <p><br>
In der Mitteilungszentrale Laura hat die Möglichkeit, <b>nehmen Sie die Einladung</b>.
    <p><br>
Wenn Laura akzeptiert, wird die Partei app ausgerichtet.                                                                         </td>
  </tr>
  <tr>
    <td>
Popupbenachrichtigung angezeigt, der angibt, Laura die Partei verknüpft ist.
    <p><br>
David und Laura erläutern, welche Spiel, das sie wiedergeben möchten. David das Spiel startet, und gibt den Modus mit Multiplayer-Spielen.
    <p><br>
Spiel erhalten entweder einer Option zum Einladen von Freunden oder Pull-automatisch die Partei-Mitglieder.
    <p><br>
    <b style="background-color: #FFFF00">Popupbenachrichtigung angezeigt, der angibt, dass ein Spiel INVITE-Nachricht gesendet wurde.</b>
    </td>
    <td>
Popupbenachrichtigung angezeigt, der angibt, dass ein Spiel für die Partei gefunden wurde.
    <p><br>
Können in der Mitteilungszentrale Laura: <ul>
    <li>   <b>Spiele Einladung annehmen:</b> Spiele <li>   <b>Spiele Einladung ablehnen:</b> Ohne Spiel gestartet wurde, Laura ist noch in die Partei, und nachfolgende Einladungen erhalten.
    <li>   <b style="background-color: #FFFF00">Lassen Sie die Partei: Ohne Spiel startet, Laura, die von der Partei entfernt wird.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>Weitere Informationen zu den Toast Spiel einladen gesendet.

Die **Spiel einladen gesendet** Toast wird nur angezeigt, beim ersten eine Spiele-Sitzung mit remote-Teilnehmer Mitglieder hergestellt wird. Nachfolgende Anforderungen, die an Mitglieder der remote-Teilnehmer gesendet werden werden diese Toast nicht generiert werden. Dadurch wird verhindert, dass den Benutzer aufführen wiederholt **Spiel einladen gesendet** Popups auf, wenn der Titel mehrere Aufrufe **PullReservedPlayersAsync**.

**Beachten Sie** die bewährte Methode ist, fügen alle gewünschten Freunde als reserviert, und rufen Sie anschließend **PullReservedPlayersAsync** nur einmal.
