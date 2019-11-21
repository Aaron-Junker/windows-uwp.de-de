---
title: Exportbeschränkungen hinsichtlich Kryptografie
description: Anhand der Informationen in diesem Abschnitt können Sie ermitteln, ob Ihre App Kryptografiefunktionen in einer Weise verwendet, die unter Umständen dazu führt, dass sie im Microsoft Store nicht angezeigt wird.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: c647d91213ddf1fd8a3dafd80c6888a026cda576
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258939"
---
# <a name="export-restrictions-on-cryptography"></a>Exportbeschränkungen hinsichtlich Kryptografie



Anhand der Informationen in diesem Abschnitt können Sie ermitteln, ob Ihre App Kryptografiefunktionen in einer Weise verwendet, die unter Umständen dazu führt, dass sie im Microsoft Store nicht angezeigt wird.

Das Bureau of Industry and Security (BIS) im United States Department of Commerce kontrolliert den Export von Technologie, die bestimmte Arten von Verschlüsselung verwendet. Bei allen im Microsoft Store aufgelisteten Apps müssen diese Gesetze und Bestimmungen beachtet werden, da die App-Dateien in den USA gespeichert werden können. Sogar bei Apps, die von App-Entwicklern aus anderen Ländern zur Verteilung außerhalb der USA hochgeladen werden, müssen diese Bestimmungen befolgt werden. Daher müssen alle App-Entwickler beim Übermitteln einer App an den Microsoft Store bestätigen, dass ihre Apps keine Technologien enthalten, die gemäß diesen Bestimmungen eingeschränkt sind.

> **Beachten Sie**  die hier bereitgestellten Informationen einige Anleitungen enthalten, aber es liegt in ihrer Verantwortung als App-Entwickler, der apps im Microsoft Store veröffentlicht, um sicherzustellen, dass Ihre APP allen anwendbaren Gesetzen und Vorschriften entspricht.

 

Weitere Informationen zum United States Department of Commerce und dem BIS finden Sie unter [Informationen zum Bureau of Industry and Security](https://www.bis.doc.gov/about/index.htm).

Informationen zu den Export Administration Regulations (EAR), die den Export von Technologien regeln, die Verschlüsselung enthalten, finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="governed-uses"></a>Eingeschränkte Verwendungsarten

Ermitteln Sie zunächst, ob Ihre App eine Art von Kryptografie verwendet, die durch die Export Administration Regulations geregelt wird. Die Frage enthält die in der Liste enthaltenen Beispiele. Diese Liste enthält jedoch nicht alle möglichen Anwendungsmöglichkeiten von Kryptografie.

> **Wichtig**  denken Sie nicht nur an den Code, den Sie für Ihre APP geschrieben haben, sondern auch auf alle Software Bibliotheken, Hilfsprogramme und Betriebssystemkomponenten, mit denen Ihre APP verknüpft ist oder mit der Sie verknüpft ist.

-   Jede Verwendung einer digitalen Signatur, z. B. Authentifizierung oder Integritätsprüfung
-   Verschlüsselung von Daten oder Dateien, die die App verwendet oder auf die sie zugreift
-   Schlüsselverwaltung, Zertifikatverwaltung oder Vorgänge, die mit einer Public Key-Infrastruktur interagieren
-   Verwendung eines sicheren Kommunikationskanals, z. B. NTLM, Kerberos, Secure Sockets Layer (SSL) oder Transport Layer Security (TLS)
-   Verschlüsselung von Kennwörtern oder andere Arten der Datensicherheit
-   Kopierschutz oder Verwaltung digitaler Rechte (Digital Rights Management, DRM)
-   Virenschutz

Eine vollständige und aktuelle Liste kryptografischer Anwendungen finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="non-restricted-uses"></a>Nicht eingeschränkte Verwendungsarten

Beachten Sie, dass einige Verwendungsarten von Kryptografie nicht eingeschränkt sind. Hier ein Überblick über die unbeschränkten Aufgaben:

-   Kennwortverschlüsselung
-   Kopierschutz
-   Authentication
-   Verwaltung digitaler Rechte (DRM)
-   Verwendung digitaler Signaturen

Eine vollständige und aktuelle Liste kryptografischer Anwendungen finden Sie unter [EAR-Bestimmungen für Waren, die Verschlüsselung nutzen](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

Wenn Ihre App Kryptografie- oder Verschlüsselungsfunktionen für eine Aufgabe aufruft, unterstützt, enthält oder verwendet, die nicht in dieser Liste enthalten ist, ist für die App eine ECCN (Export Commodity Classification Number) erforderlich.

Wenn Sie keine ECCN besitzen, informieren Sie sich unter [Fragen und Antworten zur ECCN](https://www.bis.doc.gov/licensing/do_i_needaneccn.html).
