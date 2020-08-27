---
description: Erfahren Sie mehr über die Richtlinien zur Optimierung der Sichtbarkeit ihrer Ad-Einheiten und wie Sie Ihre viewability-Metriken mit dem Leistungsbericht "Werbung" messen können.
title: Optimieren der Sichtbarkeit von Anzeigeneinheiten
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, Richtlinien, viewability
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942790"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimieren der Sichtbarkeit von Anzeigeneinheiten

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Der [Leistungsbericht "Werbung](../publish/advertising-performance-report.md) " umfasst viewability-Metriken für Ihre Ad-Einheiten. Die Sichtbarkeit ist eine wichtige Metrik, da die Werbebranche sich auf die Bewertung von sichtbaren Eindrücken und nicht nur auf übermittelten Eindrücken bewegt. Die Werbeeinblendungen sind tendenziell auf sichtbare Eindrücke Ansichts fähig, da Sie die Wahrscheinlichkeit haben, dass Ihre Werbeeinblendungen von Benutzern  

In Übereinstimmung mit den Richtlinien für die IAB-viewability wird ein Banner Werbe Eindruck als sichtbar gezählt, wenn die folgenden Kriterien erfüllt sind:

* Pixel Anforderung: der Wert von größer oder gleich 50% der Pixel in der Ankündigung lag im Anzeigebereich der app.
* Zeit Anforderung: der Zeitpunkt, zu dem die Pixel Anforderung erfüllt ist, war größer als oder gleich einem kontinuierlichen zweiten, nach dem AD-Rendering.

Ein Video-Werbe Eindruck wird als sichtbar gezählt, wenn die folgenden Kriterien erfüllt sind:

* Pixel Anforderung: der Wert von größer oder gleich 50% der Pixel in der Ankündigung lag im sichtbaren Teil der app.
* Zeit Anforderung: das Video erfüllte die Pixel Anforderung und spielte zwei fortlaufende Sekunden nach dem AD-Rendering.

Die Sichtbarkeit wird mit der folgenden Formel berechnet:

**Viewability = [sichtbare Eindrücke] * 100/[Gesamtzahl der werbeeindrücke]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Richtlinien zum Verbessern der Sichtbarkeit von Ad-Einheiten

Befolgen Sie diese Richtlinien, um die sichtbaren Eindrücke für Ihre Ad-Einheiten zu optimieren.

1. **Messen der Leistung** &nbsp; &nbsp; Verwenden Sie den [Leistungsbericht "Werbung](../publish/advertising-performance-report.md) ", um die viewability-Metriken der einzelnen Ad-Einheiten zu überprüfen.
2.  Entsprechendes **Positionieren des AD-Steuer** &nbsp; &nbsp; Elements Im allgemeinen befindet sich die am meisten sichtbare Position für eine Werbe eingeblendet direkt über dem Fold (d. h. am unteren Rand des sichtbaren Teils der Seite, den die Benutzer sehen können, ohne einen Bildlauf durchführen zu müssen). Anzeigen, die oben auf der Seite angezeigt werden, können nicht angezeigt werden. Überprüfen Sie ggf. die einzelnen Elemente Ihrer Seite, um zu erfahren, wie Sie den sichtbaren Speicherplatz in Ihrer APP optimieren können. Stellen Sie sicher, dass die AD-Steuerelemente nicht im App-Hintergrund platziert werden.
3.  **Verwalten der Seitenlänge** &nbsp; &nbsp; Kürzerer Seiten Inhalt führt zu höherer Sichtbarkeit. Implementieren Sie einen unendlichen Bildlauf, wenn Ihre APP-Seiten länger sein müssen.
4.  **Korrigieren der Ad-Position** &nbsp; &nbsp; Stellen Sie sicher, dass die Werbeeinblendungen auch dann an einer fester Position bleiben, wenn der Benutzer Dadurch erhalten Sie eine höhere Ansichts Zeit, und Sie können Ihre ansichtbarkeits Rate erhöhen.
5.  **Deckkraft festlegen** &nbsp; &nbsp; Stellen Sie sicher, dass die Deckkraft des Containers, der das AD-Steuerelement enthält, 100% beträgt.
6.  **Implementieren Lazy Loading** &nbsp; &nbsp; Laden Sie Ihre Anzeigen erst, wenn die Benutzer einen Bildlauf zu der Ansicht durchführen, die das AD-Steuerelement Dadurch wird sichergestellt, dass die Anzeige länger angezeigt wird, damit der Benutzer Sie anzeigen kann.
7.  **Experimentieren Sie mit verschiedenen Werbe Größen** &nbsp; &nbsp; Wählen Sie einige Werbe Größen aus, und Messen Sie die Verfügbarkeit für die einzelnen Werte mithilfe des [Leistungs Berichts "Werbung](../publish/advertising-performance-report.md)". Wählen Sie den Wert aus, der für Sie am besten geeignet ist.
8.  **App-Entwurf** &nbsp; &nbsp; erneut besuchen Verbessern Sie den Entwurf Ihrer APP-Seite, um die Seitenladezeit zu verringern. Benutzer bleiben tendenziell länger auf Seiten, die schneller geladen werden.
