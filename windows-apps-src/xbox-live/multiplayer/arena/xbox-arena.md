---
title: Xbox-Arena
description: Erfahren Sie, wie Xbox-Bereich zu verwenden, um Turniere für Ihr Spiel auszuführen.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, -Turniers, ux
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643765"
---
# <a name="xbox-arena"></a>Xbox-Arena

Xbox-Bereich ist eine Plattform zum Erstellen und Ausführen von online Turniere für Xbox One und Windows 10 über die Xbox Live, mit dem Ziel, für den Einsatz von Spaß, wettbewerbsfähige Spiele für einer breiten Zielgruppe.
Jeder Herausgeber umfasst unterschiedliche Anforderungen an, wenn es darum geht, bis hin zu wettbewerbsfähigen spielen und mit Spielbereich so flexibel wie möglich sein soll. Es gibt also drei Möglichkeiten, Turniere für Ihre Titel unter der Spielbereich ausgeführt:

* In Franchise-Run Turniere bietet Xbox Live mit den Tools und Diensten einrichten, und führen Sie Ihre eigenen Turniere.

* Xbox ist mit einer branchenweit führenden-Turniers Organisatoren (TOs), eine Partnerschaft eingegangen, die Sie aktivieren können, um Spielbereich Turniere im Titel des Auftrag durchlaufen. (Eine Liste der unterstützten Turnier Organisatoren, wenden Sie sich an Ihren Microsoft Account Manager.)

* Spieler können erstellen und führen ihre eigenen Turniere Spielbereich mit unseren Benutzern generierte Turniere oder UGTs.

Am wichtigsten ist, ermöglicht das Hinzufügen von Unterstützung für Spielbereich, Ihre Titel alle drei nutzen. Wir bieten eine robuste APIs, mit denen Titel, um Turniere im Spiel zu unterstützen und Teilnehmer Übergang zu und von Übereinstimmungen zu erleichtern. Der Xbox Spielbereich Hub unterstützt Turnier-Verwaltungsaufgaben wie Registrierung, Benachrichtigungen, eckige Klammern und das seeding und Ergebnisse.

> [!IMPORTANT]  
> Xbox-Bereich ist nur für verwaltete Partner verfügbar und ID@Xbox Entwickler. Es ist nicht für die Xbox Live Creators-Programm verfügbar.

## <a name="a-titles-baseline-tournament-experience"></a>Einen Titel für die Baseline-Turniers Erfahrung

Wenn eine oder mehrere Turnier Organisatoren Titel des integriert, müssen eine Baseline-Turniers-Umgebung bereitgestellt werden. Sie können in einer bestimmten Turnier Organisator besser integrieren, wenn Sie entscheiden, geben Sie für Wettbewerbe anmelden darstellungsmöglichkeiten. Aber Sie müssen weiterhin bieten Baseline für andere Turnier-Organisatoren und für UGTs und Franchise-Run Turniere, wenn Sie auswählen, um sie auszuführen.

### <a name="baseline-requirements-for-a-title"></a>Grundlegende Voraussetzungen für einen Titel

* Wenden Sie sich an Ihren Microsoft Account Manager, eine vollständige Liste der Anforderungen.

### <a name="ui-recommendations"></a>UI-Empfehlungen

* Erkennen Sie, dass die Übereinstimmung ein Turnier in der Benutzeroberfläche handelt.

* Enthalten Sie auf der Benutzeroberfläche Lobby ein Benutzeroberflächenelement, Links die Benutzer Ihren Hub Turnier und/oder die Turnier Detailseite in die Benutzeroberfläche der Shell Xbox Spielbereich oder Turnier Organisator app.



Die Baseline-benutzererfahrung, die der Titel bereitstellen muss ist einfach und flexibel genug, um mit vielen möglichen Wettbewerb Formaten zu arbeiten. Sie können den Leitfaden zur Benutzeroberfläche anpassen, und Anforderungen an den Titel des UI-Ablauf anpassen und um sicherzustellen, dass einen Benutzer das flüssige Erfahrung.

Zum Beispiel:

* Die erforderliche Informationen nicht angezeigt werden, auf dem Hauptbildschirm, vorausgesetzt, dass sie an einer beliebigen Stelle, z. B. auf eine Seite mit Details verfügbar, oder Popupfenster ist.

* Gibt es möglicherweise viele Versionen der einzelnen Bildschirme, oder sie können untereinander oder mit vorhandenen Spiels Bildschirmen kombiniert werden. Bei vielen Spielen kann beispielsweise nach der Übereinstimmung "danach Report"-Bildschirm, der an, die sowohl die "Warten auf Ergebnisse" erfüllen möglicherweise als auch "Ready" Anforderungen.

* Bildschirme sind nicht erforderlich, um, sobald dieser Phase zu ändern. Z. B. des Teams Phase aus "Ready", "Bereich" wechselt während der Benutzer auf dem Bildschirm "Bereit" befindet, dem Titel des muss sich nicht sofort in Gameplay wechseln. Sie können (und sollte wahrscheinlich) wechseln die "gewartet Übereinstimmung..." Indikator dafür, dass eine Schaltfläche – z. B. "bereit, spielt" –, damit der Benutzer befindet sich in der ablaufsteuerung und aus diesem Grund kann ein besseres Verständnis davon. Es ist in Ordnung, für die Anforderungen der Stufe "Bereich" verschoben werden, bis der Benutzer den Übergang bestätigt.


## <a name="arena-vs-title-roles"></a>Spielbereich im Vergleich zu den Titel "Roles"

Verschieben und das Spiel, während Durchlaufens Turnier Stufen kann für Benutzer kompliziert sein. Wenn der Prozess für jedes Spiel unterschiedlich, die sie spielen ist, ist auch weniger wahrscheinlich merken, wohin Sie wechseln und was Sie erwartet.

> [!TIP]
> **UX-Empfehlung**  
>
> Vereinfachen Sie funktionale Rollen zwischen des Spiels und der Xbox Spielbereich-Benutzeroberfläche, dabei werden auch deutlich. Z. B. alle verwaltungsbezogenen Aufgaben abgeschlossen sind, im Bereich und alle Spiele Play-bezogene Aufgaben innerhalb des Spiels abgeschlossen sind.

Xbox-Spielbereich-Rolle (Einrichten von ein Turnier)   | Titel der Rolle (Spiels)
--- | ---
<ul><li>Registrierung und -Check-in</li><li>Benachrichtigungen</li><li>Seeding und Klammern</li><li>Team-Clusterbildung</li></ul> |     <ul><li>Übergang von Teilnehmern in und aus der Spielbereich-Benutzeroberfläche</li><li>Identifizieren von Turnier-spezifische Übereinstimmungen in Multiplayer-Lobby UI</li><li>Heraufstufung und/oder Turniere im Spiel zu durchsuchen.</li></ul>

## <a name="engineering-guidance"></a>Engineering-Leitfaden

Artikel | description
--- | ---
[Integration der Spielbereich-Titel](arena-title-integration.md) | Erfahren Sie, wie Unterstützung für Xbox-Bereich in den Titel zu integrieren.

## <a name="operations-guidance"></a>Operationsleitfaden

Artikel | description
--- | ---
[Xbox-Spielbereich Operations-Portal](operations-portal.md) | Beschreibt, das Vorgänge-Portal, mit denen Sie zum Erstellen und verwalten offizielle Turniere für einen Titel ein, der mit der Xbox Spielbereich integriert ist.

## <a name="user-experience-guidance"></a>Leitfaden zur Benutzeroberfläche

Artikel | description
--- | ---
[Ermittlung von Xbox-Turniere](discovering-xbox-tournaments.md) | Enthält Tipps und Empfehlungen, erstellen Sie einen hervorragenden Benutzer benutzererfahrung für die Ermittlung von vorhandenen Turniere.
[Verknüpfen Sie ein Turnier](arena-ux-join-tournament.md)  |  Enthält Tipps und Empfehlungen, erstellen Sie einen hervorragenden Benutzer benutzererfahrung für das Registrieren und ein Turnier verknüpfen.
[Engagement übereinstimmen](arena-ux-match-engagement.md) | Beschreibt die Phasen des Benutzer-Erfahrung der Spieler, bis ein Turnier an.
[API-UI-Metadaten im Bereich](arena-apis-metadata.md)  | Beschreibt die Metadaten, die von den Spielbereich-APIs, die Sie, zum Anzeigen von Informationen im Spiel über den aktuellen Zustand des Turniers verwenden können zurückgegeben.
[Spielbereich Benachrichtigungen](arena-notifications.md)  | Beschreibt die Bedingungen, Xbox-Bereich sendet eine Benachrichtigung an Turnier Teilnehmer.
[Spielbereich Benutzerszenarios](arena-user-scenarios.md)  | Beschreibt Spielbereich Szenarien bieten Sie Ihren Spielern basierend auf ihre am häufigsten verwendeten Motivationen.
