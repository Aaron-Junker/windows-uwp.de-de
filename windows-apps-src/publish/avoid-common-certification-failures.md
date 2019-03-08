---
Description: Lesen Sie diese Liste, und vermeiden Sie dadurch Probleme, die häufig die Zertifizierung von Apps verhindern oder nach der Veröffentlichung der App bei einer Stichprobenkontrolle auftreten können.
title: Vermeiden allgemeiner Zertifizierungsfehler
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 62c99c159ff68201919fa15baded999e3b6a2477
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625795"
---
# <a name="avoid-common-certification-failures"></a>Vermeiden allgemeiner Zertifizierungsfehler


Lesen Sie diese Liste, und vermeiden Sie dadurch Probleme, die häufig die Zertifizierung von Apps verhindern oder nach der Veröffentlichung der App bei einer Stichprobenkontrolle auftreten können.

> [!NOTE]
> Überprüfen Sie die [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies) um sicherzustellen, dass Ihre app erfüllt alle Anforderungen aufgeführt.

-   Reichen Sie die App erst ein, wenn sie fertig ist. Sie können die Beschreibung Ihrer App gern nutzen, um auf geplante Features hinzuweisen. Achten Sie jedoch darauf, dass Ihre App keine unvollständigen Abschnitte, Links zu unfertigen Webseiten oder andere Elemente enthält, die Kunden darauf schließen lassen, dass sich die App in einem unvollständigen Zustand befindet.

-   [Testen Sie die App vor dem Einreichen mit dem Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md).

-   Testen Sie die App unter verschiedenen Konfigurationen, um größtmögliche Stabilität sicherzustellen.

-   Vergewissern Sie sich, dass die App nicht abstürzt, wenn keine Netzwerkkonnektivität besteht! Auch wenn für die eigentliche Verwendung der App eine Verbindung erforderlich ist, muss sie richtig ausgeführt werden, wenn keine Verbindung besteht.

-   [Stellen Sie die erforderlichen Infos](notes-for-certification.md) zum Verwenden der App bereit, z. B. Benutzername und Kennwort für ein Testkonto, wenn sich Benutzer bei einem Dienst anmelden müssen, oder geben Sie die erforderlichen Schritte zum Zugreifen auf versteckte oder gesperrte Features an.

-   Einschließen einer [eine Datenschutzrichtlinie-URL](enter-app-properties.md#privacy-policy-url) Wenn Ihre app ein erfordert z. B. Wenn Ihre app greift auf jede Art von persönlichen Informationen in keiner Weise, oder andernfalls gesetzlich erforderlich sind. Um zu bestimmen, wenn Ihre app eine Datenschutzrichtlinie erforderlich ist, überprüfen Sie die [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies).

-   Formulieren Sie die Beschreibung der App so, dass sie den Funktionsumfang der App genau darstellt. Unterstützung erhalten Sie in den Richtlinien zum [Verfassen einer ansprechenden App-Beschreibung](write-a-great-app-description.md).

-   Beantworten Sie alle Fragen im Bereich [Altersfreigaben](age-ratings.md) vollständig und richtig.

-   [Deklarieren Sie die App nur dann als barrierefrei](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), wenn Sie sie ausdrücklich für Barrierefreiheitsszenarien entwickelt und getestet haben.

-   Wenn die App die E-Commerce-APIs für den Windows Store aus dem [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)-Namespace verwendet, müssen Sie die App testen und sich vergewissern, dass sie typische Ausnahmen behandelt. Stellen Sie außerdem sicher, dass die App die [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)-Klasse verwendet und nicht die [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)-Klasse, die nur zu Testzwecken gedacht ist. (Wenn Ihre App auf Windows 10, Version 1607 oder höher ausgerichtet ist, wird empfohlen, Mitglieder des [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace statt des Windows.ApplicationModel.Store-Namespace zu verwenden).


 

 




