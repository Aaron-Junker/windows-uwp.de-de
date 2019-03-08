---
title: Experimentelle APIs
description: Grundlegendes zu experimentellen APIs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, experimentell,-API
ms.localizationpriority: medium
ms.openlocfilehash: 9d6e236368134086081141e220088358f4897033
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657945"
---
# <a name="experimental-apis"></a>Experimentelle APIs

Experimentelle APIs befinden sich in der Anfangsphase des Designs und ändern sich wahrscheinlich, wenn die Besitzer Feedback einarbeiten und Unterstützung für zusätzliche Szenarien hinzufügen.

Für diese APIs werden externe Test-Flights mit [Windows-Insider-SDKs](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) durchgeführt, damit Entwickler sie ausprobieren und Feedback bereitstellen können, bevor sie Teil der offiziellen Plattform werden. Während der Test-Flights gibt es keine Zusage bezügliche Stabilität oder Verpflichtung.

## <a name="consuming-experimental-apis"></a>Nutzung experimenteller APIs
IntelliSense informiert Sie, ob eine API experimentell ist. Außerdem erhalten Sie eine Compilerwarnung, wenn Sie eine experimentelle API verwenden, z. B. „... ist nur zu Testzwecken vorgesehen und kann in zukünftigen Updates geändert oder entfernt werden“.

Diese Warnungen schützen Sie vor der Erstellung von Abhängigkeiten von experimentellen APIs in Produktionsprojekten. Bei experimentellen Projekten können Sie diese Warnungen ignorieren oder deaktivieren.

Standardmäßig sind diese APIs zur Laufzeit deaktiviert und ein Aufruf der APIs führt zu einer Laufzeitausnahme. Das ist eine weitere Vorsichtsmaßnahme, um unbeabsichtigte Abhängigkeiten und eine umfangreiche Verteilung von Apps zu verhindern, die experimentelle APIs nutzen.

Um diese APIs für experimentelle Zwecke zu aktivieren, verwenden Sie das Plug-In für [Windows-Geräteportal (WDP)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal)-Features, um das Feature zu aktivieren, das der API entspricht, die Sie aufrufen möchten.

Die Dokumentation für eine bestimmte experimentelle API liegt im Ermessen des Teams, die diese besitzt.

## <a name="providing-feedback"></a>Abgeben von Feedback

Wenn Sie eine experimentelle API ausprobiert haben und Feedback dazu abgeben möchten, verwenden Sie die Kategorie **Entwicklerplattform** im [Windows-Feedback-Hub](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).