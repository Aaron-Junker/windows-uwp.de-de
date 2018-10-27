---
title: Exportbeschränkungen hinsichtlich Kryptografie
description: Anhand der Informationen in diesem Abschnitt können Sie ermitteln, ob Ihre App Kryptografiefunktionen in einer Weise verwendet, die unter Umständen dazu führt, dass sie im Microsoft Store nicht angezeigt wird.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: a29c4aeb5a5928e04e0018d68884fdb4a4876332
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703051"
---
# <a name="export-restrictions-on-cryptography"></a>Exportbeschränkungen hinsichtlich Kryptografie



Anhand der Informationen in diesem Abschnitt können Sie ermitteln, ob Ihre App Kryptografiefunktionen in einer Weise verwendet, die unter Umständen dazu führt, dass sie im Microsoft Store nicht angezeigt wird.

Das Bureau of Industry and Security (BIS) im United States Department of Commerce kontrolliert den Export von Technologie, die bestimmte Arten von Verschlüsselung verwendet. Bei allen im Microsoft Store aufgelisteten Apps müssen diese Gesetze und Bestimmungen beachtet werden, da die App-Dateien in den USA gespeichert werden können. Sogar bei Apps, die von App-Entwicklern aus anderen Ländern zur Verteilung außerhalb der USA hochgeladen werden, müssen diese Bestimmungen befolgt werden. Daher müssen alle App-Entwickler beim Übermitteln einer App an den Microsoft Store bestätigen, dass ihre Apps keine Technologien enthalten, die gemäß diesen Bestimmungen eingeschränkt sind.

> **Hinweis:** die hier bereitgestellte Informationen bietet einige Anleitungen, aber es liegt in Ihrer Verantwortung als app-Entwickler apps im Microsoft Store veröffentlichen um sicherzustellen, dass Ihre app für die Einhaltung aller geltenden Gesetze und Bestimmungen verantwortlich.

 

Weitere Informationen zum United States Department of Commerce und dem BIS finden Sie unter [Informationen zum Bureau of Industry and Security](http://go.microsoft.com/fwlink/p/?LinkID=245644).

Informationen zu den Export Administration Regulations (EAR), die den Export von Technologien regeln, die Verschlüsselung enthalten, finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="governed-uses"></a>Eingeschränkte Verwendungsarten

Ermitteln Sie zunächst, ob Ihre App eine Art von Kryptografie verwendet, die durch die Export Administration Regulations geregelt wird. Die Frage enthält die in der Liste enthaltenen Beispiele. Diese Liste enthält jedoch nicht alle möglichen Anwendungsmöglichkeiten von Kryptografie.

> **Wichtige**berücksichtigen nicht nur den Code, die Sie für Ihre app aber auch alle Softwarebibliotheken, Hilfsprogramme und Betriebssystemkomponenten, die Ihre app enthält oder links geschrieben haben.

-   Jede Verwendung einer digitalen Signatur, z.B. Authentifizierung oder Integritätsprüfung
-   Verschlüsselung von Daten oder Dateien, die die App verwendet oder auf die sie zugreift
-   Schlüsselverwaltung, Zertifikatverwaltung oder Vorgänge, die mit einer Public Key-Infrastruktur interagieren
-   Verwendung eines sicheren Kommunikationskanals, z.B. NTLM, Kerberos, Secure Sockets Layer (SSL) oder Transport Layer Security (TLS)
-   Verschlüsselung von Kennwörtern oder andere Arten der Datensicherheit
-   Kopierschutz oder Verwaltung digitaler Rechte (Digital Rights Management, DRM)
-   Virenschutz

Eine vollständige und aktuelle Liste kryptografischer Anwendungen finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="non-restricted-uses"></a>Nicht eingeschränkte Verwendungsarten

Beachten Sie, dass einige Verwendungsarten von Kryptografie nicht eingeschränkt sind. Hier ein Überblick über die unbeschränkten Aufgaben:

-   Kennwortverschlüsselung
-   Kopierschutz
-   Authentifizierung
-   Verwaltung digitaler Rechte (DRM)
-   Verwendung digitaler Signaturen

Eine vollständige und aktuelle Liste kryptografischer Anwendungen finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](http://go.microsoft.com/fwlink/p/?LinkID=245645).

Wenn Ihre App Kryptografie- oder Verschlüsselungsfunktionen für eine Aufgabe aufruft, unterstützt, enthält oder verwendet, die nicht in dieser Liste enthalten ist, ist für die App eine ECCN (Export Commodity Classification Number) erforderlich.

Wenn Sie keine ECCN besitzen, informieren Sie sich unter [Fragen und Antworten zur ECCN](http://go.microsoft.com/fwlink/p/?LinkID=245646).
