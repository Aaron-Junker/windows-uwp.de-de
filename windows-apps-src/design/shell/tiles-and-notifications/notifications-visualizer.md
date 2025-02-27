---
description: Notifications Visualizer ist eine neue Windows-App im Store, die Entwickler dabei unterstützt, adaptive Live-Kacheln für Windows 10 zu entwerfen.
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 55ab7e22dc011555a8d98fd863b7dd9ff67de5fa
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033673"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


Die Benachrichtigungs Schnellansicht ist eine neue Windows-APP [im Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) , die Entwicklern das Entwerfen von adaptiven Live Kacheln und interaktiven Popup Benachrichtigungen für Windows 10 unterstützt.


## <a name="overview"></a>Übersicht

Die Benachrichtigungs Schnellansicht bietet eine sofortige visuelle Vorschau der Kachel und Popup Benachrichtigung, wenn Sie die XML-Nutzlast bearbeiten, ähnlich wie die XAML-Editor-und Entwurfs Ansicht von Visual Studio. Die APP prüft auch auf Fehler, wodurch sichergestellt wird, dass Sie eine gültige Kachel-oder Popup Benachrichtigungs Nutzlast erstellen.

Dieser Screenshot der App zeigt die XML-Nutzlast und die Art und Weise, wie Kachelgrößen auf einem bestimmten Gerät angezeigt werden:

![Screenshot des App-Editors von Notifications Visualizer mit Code und Kacheln](images/notif-visualizer-001.png)

 

Mit der Benachrichtigungs Schnellansicht können Sie Adaptive Kachel-und Toast Nutzlasten erstellen und testen, ohne Ihre eigene APP bearbeiten und bereitstellen zu müssen. Nachdem Sie eine Nutzlast mit idealen visuellen Ergebnissen erstellt haben, können Sie diese in Ihre APP integrieren. Weitere Informationen finden Sie unter [Senden einer lokalen Kachel Benachrichtigung](sending-a-local-tile-notification.md) und [Senden eines lokalen](send-local-toast.md) Popups.

**Hinweis**   Die Simulation der Benachrichtigungs Schnellansicht des Windows-Start Menüs und der Popup Benachrichtigungen sind nicht immer vollständig genau, und einige erweiterte Nutz Last Eigenschaften werden nicht unterstützt. Wenn Sie über die gewünschte Kachel oder den gewünschten Toast verfügen, testen Sie Sie, indem Sie die Kachel fixieren oder den Popup Pop, um zu überprüfen, ob Sie in ihrer Absicht angezeigt wird.

 

## <a name="features"></a>Funktionen

Die Benachrichtigungs Schnellansicht enthält eine Reihe von Beispiel Nutzlasten, um zu veranschaulichen, was mit adaptiven Live-Kacheln und interaktiven-Auflistungen möglich ist, um Ihnen den Einstieg zu erleichtern. Sie können mit den verschiedenen Textoptionen, Gruppen/Untergruppen, Hintergrundbildern experimentieren und sehen, wie sich die Kachel an verschiedene Geräte und Bildschirme anpasst. Wenn Sie Änderungen vornehmen, können Sie Ihre aktualisierte Nutzlast in einer Datei zur späteren Verwendung speichern.

Der Editor stellt Fehler und Warnungen in Echtzeit bereit. Wenn Ihre Nutzlast z. b. größer als 5 KB (eine Platt Form Einschränkung) ist, werden Sie von der Benachrichtigungs Schnellansicht gewarnt, dass die Nutzlast dieses Limit überschreitet. Sie erhalten Warnungen aufgrund falscher Attributnamen oder Werte, wodurch das Debuggen visueller Probleme erleichtert wird.

Sie können Kachel Eigenschaften wie Anzeige Name, Farbe, Logos, ShowName und Badge-Wert steuern. Anhand dieser Optionen verstehen Sie sofort, wie Ihre Kacheleigenschaften und Kachelbenachrichtigungsnutzlasten interagieren und welche Ergebnisse sie produzieren.

Dieser Screenshot der App zeigt den Kachel-Editor:

![Screenshot des Notifications Visualizer-Editors mit Kacheln](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Verwandte Themen

* [Notifications Visualizer aus dem Store herunterladen](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Erstellen adaptiver Kacheln](create-adaptive-tiles.md)
* [Interaktive Fenster](adaptive-interactive-toasts.md)
