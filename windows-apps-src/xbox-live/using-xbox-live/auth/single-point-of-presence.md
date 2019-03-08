---
title: Single Point of Presence (SPOP)
description: Erfahren Sie, wie Xbox Live einzelnen Point of Presence (SPOP) verwenden, um sicherzustellen, dass ein Titel auf nur einem einzelnen Gerät zu einem Zeitpunkt wiedergegeben wird.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Spop, einzelne Fehlerquelle vorhanden
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595005"
---
# <a name="single-point-of-presence-spop"></a>Single Point of Presence (SPOP)

## <a name="overview"></a>Übersicht
Einzelnen Punkts der Anwesenheit SPOP (), handelt es sich um eine Bedingung Xbox Live erzwungen, in denen ein Titel nur auf einem Gerät zu einem Zeitpunkt wiedergegeben werden kann. Dies wird für Xbox One XDK-und UWP-Titel auf allen Geräten erzwungen.
Ein Xbox Live-Titel, unabhängig vom Gerät an, die, dem diese Option aktiviert ist, kann einen Benutzer starten, der in einen Titel auf einem anderen Xbox One oder den Windows 10-Gerät angemeldet ist.

## <a name="how-to-handle-spop"></a>Gewusst wie: Behandeln von SPOP
SPOP sollten die gleiche Weise wie jede andere Art von Ereignis-Abmeldung durch den Titel behandelt werden. Der Benutzer wird immer über die Benutzeroberfläche benachrichtigt, wenn sie eine Aktion auszuführen, die ein SPOP, um sicherzustellen, dass sie dazu führen, dass den Titel, der getrennt werden, auf das andere Gerät möchten initiieren werden würde.

* Für den Titel der xdk-Version die `User::SignOutCompleted` Ereignis wird ausgelöst, wenn dies der Fall.
* Für UWP-Titel, sie werden benachrichtigt, der die Abmeldung durch das `sign_out_complete` -Handler aus der `xbox_live_user` Klasse. Finden Sie unter [Authentifizierung für UWP-Projekte](authentication-for-UWP-projects.md) Weitere Details
