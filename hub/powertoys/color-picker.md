---
title: PowerToys-Farbauswahl-Hilfsprogramm für Windows 10
description: Ein systemweites Farbauswahl-Hilfsprogramm für Windows 10, das es Ihnen ermöglicht, Farben aus einer beliebigen aktuell laufenden Anwendung auszuwählen und die Hexadezimal-oder RGB-Werte automatisch in die Zwischenablage zu kopieren.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c76753d455d28dd44045edf9482b3de9ccd97b8
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618569"
---
# <a name="color-picker-utility"></a>Farbauswahl-Hilfsprogramm

Ein systemweites Farbauswahl-Hilfsprogramm für Windows 10, das es Ihnen ermöglicht, Farben aus einer beliebigen aktuell laufenden Anwendung auszuwählen und automatisch in ein konfigurierbares Format in die Zwischenablage zu kopieren.

![ColorPicker](../images/pt-colorpicker-hex-editor.png)

## <a name="getting-started"></a>Erste Schritte

### <a name="enable"></a>Aktivieren

Um mit der Verwendung der Farbauswahl zu beginnen, müssen Sie zunächst sicherstellen, dass Sie im Abschnitt "PowerToys-Einstellungen" (Farbauswahl Abschnitt) aktiviert ist.

### <a name="activate"></a>Aktivieren

Nach der Aktivierung können Sie eines der folgenden drei Verhaltensweisen auswählen, das beim Starten der Farbauswahl mit dem Aktivierungs Kontext <kbd>Win</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> ausgeführt werden soll (Beachten Sie, dass diese Verknüpfung im Dialogfeld "Einstellungen" geändert werden kann):

:::image type="content" source="../images/pt-colorpicker-behaviors.png" alt-text="ColorPicker-Verhalten.":::

- **Farbauswahl mit aktiviertem Editor-Modus** : öffnet die Farbauswahl, nachdem Sie eine Farbe ausgewählt haben, wird der Editor geöffnet, und die ausgewählte Farbe wird in die Zwischenablage kopiert (im Dialogfeld "Einstellungen" ist das Standardformat konfigurierbar).
- **Editor** : öffnet den Editor direkt. hier können Sie eine Farbe aus dem Verlauf auswählen, eine ausgewählte Farbe optimieren oder eine neue Farbe mit erfassen, indem Sie die Farbauswahl öffnen.
- **Nur Farb** Auswahl: öffnet nur die Farbauswahl, und die ausgewählte Farbe wird in die Zwischenablage kopiert.

### <a name="select-color"></a>Farbe auswählen

Wenn die Farbauswahl aktiviert ist, zeigen Sie mit dem Mauszeiger auf die Farbe, die Sie kopieren möchten, und klicken Sie mit der linken Maustaste auf die Maustaste, um eine Farbe auszuwählen. Wenn Sie den Bereich um den Cursor ausführlicher anzeigen möchten, führen Sie einen Bildlauf nach oben durch, um die Ansicht zu vergrößern.

Die kopierte Farbe wird in der Zwischenablage in dem Format gespeichert, das in den Einstellungen konfiguriert ist (standardmäßig Hex).

![Auswählen einer Farbe](../images/pt-colorpicker.gif)

## <a name="editor-usage"></a>Editor-Verwendung

Im Editor können Sie den Verlauf der ausgewählten Farben (bis zu 20) anzeigen und ihre Darstellung in ein beliebiges vordefiniertes Zeichen folgen Format kopieren. Sie können konfigurieren, welche Farb Formate im Editor angezeigt werden, zusammen mit der Reihenfolge, in der Sie angezeigt werden. Diese Konfiguration finden Sie unter PowerToys-Einstellungen.

Der Editor ermöglicht es Ihnen auch, jede ausgewählte Farbe zu optimieren oder eine neue ähnliche Farbe zu erhalten. Der-Editor zeigt unterschiedliche Schattierungen der aktuell ausgewählten Color-2-heller und 2 dunkleren an.

Wenn Sie auf einen dieser alternativen Farbschattierungen klicken, wird die Auswahl zum Verlauf der Farben ausgewählt (wird oben in der Liste Farben Verlauf angezeigt). Die Farbe in der Mitte stellt die aktuell ausgewählte Farbe aus dem Verlauf der Farben dar. Wenn Sie darauf klicken, wird das Konfigurations Steuerelement für die Feinabstimmung angezeigt, mit dem Sie Hue-oder RGB-Werte der aktuellen Farbe ändern können. Wenn Sie auf OK drücken, wird der Farben Verlauf neu konfigurierte Farbe hinzugefügt.

![ColorPicker-Editor](../images/pt-colorpicker-editor.gif)

Klicken Sie mit der rechten Maustaste auf eine gewünschte Farbe, und wählen Sie *Entfernen* aus, um eine beliebige Farbe aus der Farbverlauf

## <a name="settings"></a>Einstellungen

Mit der Farbauswahl können Sie die folgenden Einstellungen ändern:

- Aktivierungs Verknüpfung
- Verhalten der Aktivierungs Verknüpfung
- Format einer kopierten Farbe (hexadezimal, RGB usw.)
- Reihenfolge und Darstellung von Farbformaten im Editor

![Bildschirm Abbildung von ColorPicker-Einstellungen](../images/pt-colorpicker-settings.png)

## <a name="limitations"></a>Einschränkungen

- Die Farbauswahl kann nicht oben im Startmenü oder im Aktions Center angezeigt werden (Sie können immer noch eine Farbe auswählen).
- Wenn die aktuell fokussierte Anwendung mit einer Administratorrechte Erweiterung (als Administrator ausgeführt) gestartet wurde, funktioniert die Aktivierung der Farbauswahl nicht, es sei denn, PowerToys wurde ebenfalls mit einer Administratorrechte Erweiterung gestartet.
