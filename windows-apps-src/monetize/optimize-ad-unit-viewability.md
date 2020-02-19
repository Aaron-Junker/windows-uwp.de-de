---
description: Erfahren Sie, wie Sie die Sichtbarkeit Ihrer Anzeigeneinheiten verbessern können.
title: Optimieren der Sichtbarkeit von Anzeigeneinheiten
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Richtlinien, Sichtbarkeit
ms.localizationpriority: medium
ms.openlocfilehash: 6996b656c9bf161538e286dc4c2d63c1d2840bc8
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463942"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimieren der Sichtbarkeit von Anzeigeneinheiten

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://aka.ms/ad-monetization-shutdown)

Die [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md) enthält Messwerte zur Sichtbarkeit für Ihre Anzeigeneinheiten. Die Sichtbarkeit ist ein wichtiger Messwert, da sich die Werbeindustrie eher auf sichtbare Impressionen konzentriert als auf nur gelieferte. Werbekunden neigen dazu, für sichtbare Impressionen zu bieten, da diese eine höhere Wahrscheinlichkeit besitzen, von Nutzern wahrgenommen werden.  

In Übereinstimmung mit den IAB-Richtlinien zur Sichtbarkeit wird eine Banner-Anzeigenimpression als sichtbar gezählt, wenn sie die folgenden Kriterien erfüllt:

* Pixel-Anforderung: Mindestens 50% der Pixel in der Anzeige befanden sich im sichtbaren Bereich der App.
* Zeitliche Anforderung: Die Pixelanforderung wurde nach dem Rendern mindestens eine Sekunde lang kontinuierlich erfüllt.

Eine Videoanzeigenimpression wird als sichtbar gezählt, wenn sie die folgenden Kriterien erfüllt:

* Pixel-Anforderung: Mindestens 50% der Pixel in der Anzeige befanden sich im sichtbaren Teil der App.
* Zeitliche Anforderung: Das Video hat die Pixel-Anforderung erfüllt und wurde nach dem Rendern mindestens zwei Sekunde lang kontinuierlich wiedergegeben.

Die Sichtbarkeit wird anhand der folgenden Formel berechnet:

**Viewability = [sichtbare Eindrücke] * 100/[Gesamtzahl der werbeeindrücke]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Richtlinien zur Verbesserung der Sichtbarkeit von Anzeigeneinheiten

Befolgen Sie diese Richtlinien, um die sichtbaren Impressionen für Ihre Anzeigeneinheiten zu optimieren.

1. **Messen Sie die Leistung**&nbsp;&nbsp;Verwenden Sie die [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md) zum Überprüfen der Metriken jeder einzelnen Anzeigeneinheit.
2.  **Positionieren Sie das Anzeigensteuerelement optimal**&nbsp;&nbsp;Im Allgemeinen befindet sich die am besten sichtbare Position für eine Anzeige gerade oberhalb des unteren Seitenrands, der ohne Bildlauf sichtbar ist. Anzeigen, die oben in der Seite stehen, sind nicht so gut erkennbar. Überprüfen Sie jedes Element der Seite, um zu ermitteln, wie Sie den sichtbaren Platz in Ihrer App optimieren können. Stellen Sie sicher, dass die Anzeigensteuerelemente nicht im Hintergrund der App platziert werden.
3.  **Beachten Sie die Seitenlänge**&nbsp;&nbsp;Ein kürzerer Seiteninhalt führt zu höherer Sichtbarkeit. Implementieren Sie unbegrenztes Scrollen, wenn Ihre App-Seiten länger sein müssen.
4.  **Fixieren Sie die Anzeigenposition**&nbsp;&nbsp;Stellen Sie sicher, dass Ihre Anzeigen auch beim Scrollen an einer festen Position bleiben. Dadurch erhalten Sie eine längere Betrachtungszeit und erhöhen Ihre Sichtbarkeitsrate.
5.  **Legen Sie die Deckkraft fest**&nbsp;&nbsp;Stellen Sie sicher, dass die Deckkraft des Containers, der das Anzeigensteuerelement enthält, auf 100 % festgelegt ist.
6.  **Implementieren Sie verzögertes Laden**&nbsp;&nbsp;Laden Sie Ihre Anzeigen erst, wenn der Benutzer das Anzeigensteuerelement in den sichtbaren Bereich gescrollt hat. Dadurch wird die Anzeige länger für den Benutzer angezeigt.
7.  **Experimentieren Sie mit verschiedenen Anzeigengrößen**&nbsp;&nbsp;Wählen Sie ein einige Anzeigengrößen und ermitteln Sie die jeweilige Sichtbarkeit mit dem [Bericht zur Anzeigenleistung](../publish/advertising-performance-report.md). Verwenden Sie die am besten geeignete Größe.
8.  **Überprüfen Sie mehrmals das App-Design**&nbsp;&nbsp;Verbessern Sie das Design Ihrer App-Seite, um die Seitenladezeit zu reduzieren. Benutzer neigen dazu, länger auf Seiten zu bleiben, die schnell geladen werden.
