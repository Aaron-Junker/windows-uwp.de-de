---
title: PowerToys-Administrator Modus für Windows 10
description: Damit PowerToys mit einer app ausgeführt werden kann, die im Modus mit erhöhten Rechten ausgeführt wird, muss PowerToys ebenfalls im Administrator Modus ausgeführt werden.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618539"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>Ausführung von PowerToys mit erweiterten Administrator Berechtigungen

Wenn Sie eine Anwendung als Administrator ausführen (auch als erweiterte Berechtigungen bezeichnet), funktionieren PowerToys möglicherweise nicht ordnungsgemäß, wenn sich die Anwendungen mit erhöhten Rechten im Fokus befinden oder wenn Sie versuchen, mit einem PowerToys-Feature wie "fancyzones" zu interagieren. Dies kann auch durch Ausführen von PowerToys als Administrator behoben werden.

## <a name="options"></a>Tastatur

Es gibt zwei Optionen für PowerToys zur Unterstützung von Anwendungen, die als Administrator (mit erhöhten Berechtigungen) ausgeführt werden:

- **[Empfohlen]**: PowerToys zeigt eine Eingabeaufforderung an, wenn ein erhöhter Prozess erkannt wird. Öffnen Sie die **PowerToys-Einstellungen**. Wählen Sie auf der Registerkarte **Allgemein** die Option "als Administrator neu starten" aus.

- Aktivieren Sie "immer als Administrator ausführen" in den **PowerToys-Einstellungen**.

## <a name="run-as-administrator-elevated-processes-explained"></a>Prozess als Administrator mit erhöhten Rechten ausführen

Windows-Anwendungen werden standardmäßig im **Benutzermodus** ausgeführt. Um eine Anwendung im **Verwaltungsmodus** oder als erweiterten *Prozess* auszuführen, wird diese APP mit zusätzlichem Zugriff auf das Betriebssystem ausgeführt.

Die einfachste Möglichkeit, eine APP oder ein Programm im Verwaltungsmodus auszuführen, besteht darin, mit der rechten Maustaste auf das Programm zu klicken und **als Administrator ausführen** auszuwählen. Wenn der aktuelle Benutzer kein Administrator ist, fordert Windows den Administrator Benutzernamen und das Kennwort an.

Die meisten apps müssen nicht mit erhöhten Berechtigungen ausgeführt werden. Ein häufiges Szenario ist jedoch, dass die Administrator Berechtigung erforderlich ist, bestimmte PowerShell-Befehle auszuführen oder die Registrierung zu bearbeiten.

Wenn diese Aufforderung angezeigt wird (Benutzer Access Control Aufforderung), fordert die Anwendung die Administratorebene mit erhöhten Berechtigungen an:

![Bildschirm Abbildung der Eingabeaufforderung mit erhöhten Rechten](../images/pt-admin-prompt.png)

Im Fall einer Befehlszeile mit erhöhten Rechten wird der Titel "Administrator" in der Regel an die Titelleiste angehängt.

![Screenshot der Windows-Administrator Befehlszeile](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>Unterstützung für den Administrator Modus mit PowerToys

PowerToys benötigt nur bei der Interaktion mit anderen Anwendungen, die im Administrator Modus ausgeführt werden, eine erhöhte Administrator Berechtigung. Wenn diese Anwendungen im Fokus sind, funktionieren PowerToys möglicherweise nicht mehr, wenn Sie ebenfalls nicht erhöht werden.

Dies sind die beiden Szenarios, in denen wir nicht arbeiten werden:

- Abfangen bestimmter Typen von Tastatur Strichen
- Ändern der Größe/Verschieben von Fenstern

### <a name="affected-powertoys-utilities"></a>Betroffene PowerToys-Hilfsprogramme

Berechtigungen für den Administrator Modus sind möglicherweise in den folgenden Szenarien erforderlich:

- Fancyzones
  - Ausrichten eines Fensters mit erhöhten Rechten (z. b. Eingabeaufforderung) in eine ausgefallene Zone
  - Verschieben des Fensters mit erhöhten Rechten in eine andere Zone
- Kurzanleitung
  - Verknüpfung anzeigen
- Tastatur Zuordnung
  - Schlüssel zum Neuzuordnen von Schlüsseln
  - Neuzuordnen von Verknüpfungen auf globaler Ebene
  - Neuzuordnen von App-Ziel Verknüpfungen
- PowerToys-Lauf
  - Verknüpfung anzeigen
