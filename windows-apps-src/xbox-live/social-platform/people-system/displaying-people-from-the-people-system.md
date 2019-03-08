---
title: Personen, aus dem System Personen anzeigen
description: Erfahren Sie mehr zum Codefluss der anzuzeigenden Personen, mit der Xbox Live Personen für partitionsverarbeitung.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658305"
---
# <a name="display-people-from-the-people-system"></a>Personen, aus dem System Personen anzeigen

Dienste über die Xbox Live nur Daten, die von diesen Diensten gehören und nur XUID Verweise auf Benutzer; die Personen-Dienst wird z. B. nur besitzt, und der gibt die XUIDs, die in der Kontaktliste eines Benutzers und einige sehr grundlegende Informationen zu den einzelnen diese XUIDs (z. B. bevorzugten Status) sind. Der Dienst vorhanden ist Besitzer der Daten zu den Informationen der Onlinestatus des XUIDs. Der Bestenlisten-Dienst besitzt Rangfolgeinformationen für Listen von XUIDs. Namen und das Gamertag Anzeigeinformationen wird nie von einem beliebigen Dienst als Profildienst zurückgegeben, und daher mehrere Dienste aufrufen ist erforderlich, um Listen von Personen in Umgebungen zu rendern.

Das allgemeine Aufrufmuster für den Dienst-APIs ist, um einen round-Trip um eine Liste der XUIDs zuerst aus dem Dienst abgerufen werden kann, die am besten filtern können zu machen oder erforderlich, Sortieren der Liste, und stellen gleichzeitig Round-Trip-Aufrufe an andere Dienste erforderlich, um zusätzliche Metadaten zu erhalten für jede XIUD. Im Fall von Bildern möglicherweise ein dritte Roundtrip der Aufrufe zum Abrufen von Images aus dem Bild-URLs erforderlich.

Zum Reduzieren der Anzahl der Roundtrips zum Abrufen von Daten über Kontaktliste der eines Benutzers, einen Benutzer erforderlich *Moniker* wird zu der relevanten Dienste eingeführt. Diese neue Funktion ermöglicht Aufrufern, abstrakt an den primären Dienst auszudrücken, dass der Dienst sollte die Liste der Personen mit des Benutzers vom Dienst Personen abgerufen, und Sie dann verwenden auf die Rückgabe Bereich dieses Satzes von XUIDs.

Es folgen Aufruf Flow Beispielszenarien, die veranschaulichen, wie Sie das Abrufen von Daten durch Titel von Diensten, die im Zusammenhang mit Personen:

-   Liste der Benutzer derzeit im Spiel
-   Liste der Personen ein des aktuellen Benutzers, die online sind.
-   Zufällige Benutzer mit globalen-Bestenliste
-   Leaderboard des Benutzers Personen


## <a name="list-of-users-currently-in-game"></a>Liste der Benutzer derzeit im Spiel

| Titel  | Ziel  | Feld zum Rendern  | Anrufverkehr
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| Liste der zufälligen XUIDs anderer Benutzer im Spiel | Minimale Informationen für jeden der anderen Benutzer gerendert. | GameDisplayName  \[Profile\] | Rufen Sie mit der Liste der XUIDs Profil ein. |


## <a name="list-of-the-current-users-people-who-are-online"></a>Liste der Personen ein des aktuellen Benutzers, die online sind.

## <a name="title-has"></a>Titel verfügt:
Der aktuelle Benutzer XUID

## <a name="goal"></a>Ziel
Umfassende Liste der online-Benutzer zu rendern, die in der Kontaktliste des aktuellen Benutzers sind.

## <a name="field-to-render-owning-service"></a>Feld zum Rendern \[Dienst besitzt\]
* Bevorzugte Indikator [Benutzer]
* Anzeigebild [Profile]
* GameDisplayName [Profile]
* Grundlegende Onlinestatus (grüner Ball) [Anwesenheit]

## <a name="call-flow"></a>Anrufverkehr
1. Rufen Sie die Anwesenheit, übergeben den Moniker Personen die XUIDs und online-Status für die einzelnen Personen für den Benutzer zu erhalten.
1. Parallel:
 1. Rufen Sie Profil, das die gesamte Liste der XUIDs den Anzeigenamen und die Bild-URL für die einzelnen übergeben.
 1. Rufen Sie die Personen, die in der Liste der XUIDs, um herauszufinden, ob die einem Favoriten des Benutzers werden übergeben.
1. Nach dem Aufruf von Profil:
 1. Bilder für jedes Bild-URL abrufen

## <a name="global-leaderboard-containing-random-users"></a>Zufällige Benutzer mit globalen-Bestenliste

## <a name="title-has"></a>Titel verfügt:
Der ID/Name der die Bestenliste

## <a name="goal"></a>Ziel
Um grundlegende Informationen für jeden Benutzer auf die Bestenliste zu rendern.

## <a name="field-to-render-owning-service"></a>Feld zum Rendern von [besitzenden Service]
* Bevorzugte Indikator [Benutzer]
* GameDisplayName [Profile]
* Rangfolge [Bestenlisten]
* Bewertung [Bestenlisten]

## <a name="call-flow"></a>Anrufverkehr
1. Rufen Sie Bestenlisten zum Abrufen der XUIDs, Rang und die Ergebnisse für die bestimmte Bestenliste an.
1. Parallel:
 1. Rufen Sie Profil, in der Liste der XUIDs den Anzeigenamen und die Bild-URL für die einzelnen übergeben.
 1. Rufen Sie die Personen, die in der Liste der XUIDs, um herauszufinden, ob die einem Favoriten des Benutzers werden übergeben.

## <a name="leaderboard-of-users-people"></a>Leaderboard des Benutzers Personen

## <a name="title-has"></a>Titel verfügt:
* Der ID/Name der die Bestenliste
* Der aktuelle Benutzer XUID

## <a name="goal"></a>Ziel
Um grundlegende Informationen für jeden Benutzer auf die Bestenliste zu rendern.

## <a name="field-to-render-owning-service"></a>Feld zum Rendern von [besitzenden Service]
* Bevorzugte Indikator [Benutzer]
* GameDisplayName [Profile]
* Rangfolge [Bestenlisten]
* Bewertung [Bestenlisten]

## <a name="call-flow"></a>Anrufverkehr
1. Rufen Sie Bestenlisten, die den Moniker Personen zum Abrufen der XUIDs, Rang und übergeben und die Ergebnisse für den bestimmten Bestenliste anzeigen, die auf der Liste der Personen beschränkt.
1. Parallel:
 1. Rufen Sie Profil, in der Liste der XUIDs den Anzeigenamen und die Bild-URL für die einzelnen übergeben.
 1. Rufen Sie die Personen, die in der Liste der XUIDs, um herauszufinden, ob die einem Favoriten des Benutzers werden übergeben.
