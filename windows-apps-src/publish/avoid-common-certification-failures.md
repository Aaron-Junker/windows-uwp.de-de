---
Description: Lesen Sie diese Liste, und vermeiden Sie dadurch Probleme, die häufig die Zertifizierung von Apps verhindern oder nach der Veröffentlichung der App bei einer Stichprobenkontrolle auftreten können.
title: Vermeiden allgemeiner Zertifizierungsfehler
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07c814fc48e47b2bdc8980ac72732783d7ea9139
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158024"
---
# <a name="avoid-common-certification-failures"></a>Vermeiden allgemeiner Zertifizierungsfehler


Lesen Sie diese Liste, und vermeiden Sie dadurch Probleme, die häufig die Zertifizierung von Apps verhindern oder nach der Veröffentlichung der App bei einer Stichprobenkontrolle auftreten können.

> [!NOTE]
> Überprüfen Sie die [Microsoft Store Richtlinien](store-policies.md) , um sicherzustellen, dass Ihre APP alle hier aufgeführten Anforderungen erfüllt.

-   Reichen Sie die App erst ein, wenn sie fertig ist. Sie können die Beschreibung Ihrer App gern nutzen, um auf geplante Features hinzuweisen. Achten Sie jedoch darauf, dass Ihre App keine unvollständigen Abschnitte, Links zu unfertigen Webseiten oder andere Elemente enthält, die Kunden darauf schließen lassen, dass sich die App in einem unvollständigen Zustand befindet.

-   [Testen Sie die App vor dem Einreichen mit dem Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md).

-   Testen Sie die App unter verschiedenen Konfigurationen, um größtmögliche Stabilität sicherzustellen.

-   Stellen Sie sicher, dass Ihre APP ohne Netzwerk Konnektivität nicht abstürzt. Auch wenn für die eigentliche Verwendung der App eine Verbindung erforderlich ist, muss sie richtig ausgeführt werden, wenn keine Verbindung besteht.

-   [Geben Sie alle erforderlichen Informationen](notes-for-certification.md) an, die für die Verwendung Ihrer APP erforderlich sind, wie z. b. den Benutzernamen und das Kennwort für ein Testkonto, wenn Ihre APP erfordert, dass Benutzer sich bei einem Dienst anmelden, oder alle Schritte, die für den Zugriff auf verborgene oder

-   Fügen Sie eine URL für die [Datenschutz](enter-app-properties.md#privacy-policy-url) Richtlinie ein, wenn Ihre APP eine erfordert Dies ist beispielsweise der Fall, wenn Ihre APP auf irgendeine Art von persönlichen Informationen zugreift oder andernfalls gesetzlich vorgeschrieben ist. Überprüfen Sie die [App-Entwickler Vereinbarung](/legal/windows/agreements/app-developer-agreement) und die [Microsoft Store Richtlinien](store-policies.md), um zu ermitteln, ob Ihre APP eine Datenschutzrichtlinie erfordert.

-   Formulieren Sie die Beschreibung der App so, dass sie den Funktionsumfang der App genau darstellt. Unterstützung erhalten Sie in den Richtlinien zum [Verfassen einer ansprechenden App-Beschreibung](write-a-great-app-description.md).

-   Stellen Sie vollständige und genaue Antworten auf alle Fragen im Abschnitt [Alters Bewertungen](age-ratings.md) bereit.

-   [Deklarieren Sie die App nur dann als barrierefrei](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), wenn Sie sie ausdrücklich für Barrierefreiheitsszenarien entwickelt und getestet haben.

-   Wenn die App die E-Commerce-APIs für den Windows Store aus dem [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store)-Namespace verwendet, müssen Sie die App testen und sich vergewissern, dass sie typische Ausnahmen behandelt. Stellen Sie außerdem sicher, dass Ihre APP die [**currentapp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse und nicht die [**currentappsimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) -Klasse verwendet, die nur zu Testzwecken verwendet wird. (Beachten Sie Folgendes: Wenn Ihre APP auf Windows 10, Version 1607 oder höher, ausgerichtet ist, wird empfohlen, dass Sie Member des [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace anstelle des Windows. applicationmodel. Store-Namespace verwenden.)


 

 