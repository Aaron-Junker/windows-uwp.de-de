---
title: Systemressourcen für UWP-Apps und -Spiele auf Xbox One
description: Erfahren Sie, wie UWP-apps, die auf Xbox One ausgeführt werden, Ressourcen mit dem System und anderen apps und den Ressourcenanforderungen für UWP-Apps oder-Spiele gemeinsam nutzen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: dbc6af56867508e3178ddd8b731af49270faf3d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174684"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Systemressourcen für UWP-Apps und -Spiele auf Xbox One

UWP-apps, die auf Xbox One ausgeführt werden, nutzen Ressourcen mit dem System und anderen apps gemeinsam. Die Ressourcen, die für eine UWP in der Xbox One-App verfügbar sind, hängen davon ab, ob Sie als APP oder als Xbox Live Creators-Programm Spiel übermitteln.

* Maximaler verfügbarer Arbeitsspeicher während der Ausführung im Vordergrund:
    * Apps: 1 GB
    * Spiele: 5 GB

Der maximal verfügbare Arbeitsspeicher für eine im Hintergrund ausgeführte App beträgt 128 MB. Der Hintergrundmodus gilt nur für parallele Anwendungen, wie z. b. Hintergrundmusik-Player.  Spiele werden angehalten und im Hintergrund beendet.


Das Überschreiten dieser Einschränkungen führt zu Fehlern bei der Speicher Belegung. Weitere Informationen zum Überwachen der Arbeitsspeicherverwendung finden Sie in der Referenz der [MemoryManager-Klasse](/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Wenn Sie Ihre APP oder Ihr Spiel aus dem Visual Studio-Debugger ausführen, gelten diese Einschränkungen nicht. Diese Beschränkung gilt nur, wenn die Ausführung nicht im Debugmodus erfolgt.

* CPU
    * Apps: Geben Sie 2-4 CPU-Kerne an, abhängig von der Anzahl der apps und Spiele, die auf dem System ausgeführt werden.
    * Spiele: 4 exklusive und 2 freigegebene CPU-Kerne.

* GPU
    * Apps: Freigabe von 45% der GPU, abhängig von der Anzahl der apps und Spiele, die auf dem System ausgeführt werden.
    * Games: Vollzugriff auf verfügbare GPU-Zyklen.

* DirectX-Unterstützung
    * Apps: DirectX 11-Funktionsebene 10.
    * Spiele: DirectX 12 und DirectX 11 Feature Level 10.

* Alle apps und Spiele müssen auf die x64-Architektur abzielen, damit Sie entwickelt oder an den Store für Xbox übermittelt werden können.  

Bei der **Anwendungsentwicklung**können die verfügbaren Ressourcen im Vergleich zu einem Standard-PC eingeschränkt werden und können abhängig von der Anzahl von apps und spielen, die auf dem System ausgeführt werden, variieren.

Bei der **Spieleentwicklung**ist Xbox One, wie andere Spielekonsolen, eine spezielle Hardware, die ein bestimmtes Hardware basiertes Entwicklungskit erfordert, um auf das volle Potenzial zuzugreifen. Wenn Sie an einem Spiel arbeiten, das Zugriff auf das maximale Potenzial der Xbox One-Hardware benötigt, sollten Sie sich für das Programm registrieren, [ID@Xbox](https://www.xbox.com/Developers/id) um Zugriff auf ein Xbox One Development Kit zu erhalten.

## <a name="see-also"></a>Weitere Informationen
- [UWP auf Xbox One](index.md)
- [Beginnen Sie mit dem Xbox Live Creators-Programm](/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX und UWP auf Xbox One](https://walbourn.github.io/)