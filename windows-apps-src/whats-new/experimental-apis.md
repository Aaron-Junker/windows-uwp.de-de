---
title: Experimentelle APIs
description: Grundlegendes zu experimentellen APIs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, experimentell, API
ms.localizationpriority: medium
ms.openlocfilehash: 9d6e236368134086081141e220088358f4897033
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63788148"
---
# <a name="experimental-apis"></a>Experimentelle APIs

Experimentelle APIs befinden sich in der Anfangsphase des Entwurfs und ändern sich wahrscheinlich, wenn die Besitzer Feedback einarbeiten und Unterstützung für zusätzliche Szenarien hinzufügen.

Für diese APIs werden externe Test-Flights mit [Windows-Insider-SDKs](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) zur Verfügung gestellt, damit Entwickler sie ausprobieren und Feedback bereitstellen können, bevor sie Teil der offiziellen Plattform werden. Während der Test-Flight-Phase können Stabilität und Verbindlichkeit nicht garantiert werden.

## <a name="consuming-experimental-apis"></a>Nutzung experimenteller APIs
IntelliSense informiert dich, ob eine API experimentell ist. Außerdem erhältst du eine Compilerwarnung, wenn du eine experimentelle API verwendest, z. B. „... ist nur zu Testzwecken vorgesehen und kann in zukünftigen Updates geändert oder entfernt werden“.

Diese Warnungen schützt dich vor der Erstellung von Abhängigkeiten von experimentellen APIs in Produktionsprojekten. Bei experimentellen Projekten kannst du diese Warnungen ignorieren oder deaktivieren.

Standardmäßig sind diese APIs zur Laufzeit deaktiviert, und ein Aufruf der APIs führt zu einer Laufzeitausnahme. Das ist eine weitere Vorsichtsmaßnahme, um unbeabsichtigte Abhängigkeiten und eine umfangreiche Verteilung von Apps zu verhindern, die experimentelle APIs nutzen.

Um diese APIs für experimentelle Zwecke zu aktivieren, verwendest du das Plug-In für Features des [Windows-Geräteportals (Windows Device Portal, WDP)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal), um das Feature zu aktivieren, das der aufzurufenden API entspricht.

Für die Dokumentation der jeweiligen experimentellen API ist das entsprechende Team zuständig.

## <a name="providing-feedback"></a>Abgeben von Feedback

Wenn du eine experimentelle API ausprobiert hast und Feedback dazu abgeben möchten, verwende die Kategorie **Entwicklerplattform** in [Windows-Feedback-Hub](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).