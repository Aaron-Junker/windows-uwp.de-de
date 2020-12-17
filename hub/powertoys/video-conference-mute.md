---
title: PowerToys Video Conference stumm Dienstprogramm für Windows 10
description: Ein Hilfsprogramm, das es Benutzern ermöglicht, das Mikrofon (Audiodatei) schnell zu stumm schalten und die Kamera (Video) bei einem Konferenzgespräch mit einer einzelnen Tastatureingabe zu deaktivieren, unabhängig davon, welche Anwendung den Fokus auf dem Computer hat.
ms.date: 12/07/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9a7ec901ffc31e565adb94a1a043fd149064c12
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612046"
---
# <a name="video-conference-mute-preview"></a>Video Konferenz stumm schalten (Vorschau)

> [!IMPORTANT]
> Hierbei handelt es sich um ein Vorschau Feature, das nur in der Vorabversion von PowerToys enthalten ist. Zum Ausführen dieser Vorabversion ist Windows 10, Version 1903 (Build 18362) oder höher erforderlich.

Stumm schalten Sie Ihr Mikrofon (Audiodatei), und schalten Sie Ihre Kamera (Video) während eines Konferenz Anrufes mit einem einzigen Keystroke aus, unabhängig davon, welche Anwendung den Fokus auf Ihren Computer hat.

## <a name="usage"></a>Verwendung

Die Standardeinstellungen für die Verwendung von Video Konferenz stumm sind:

- <kbd>⊞ Win</kbd> + <kbd>N</kbd> so schalten Sie sowohl Audiodaten als auch Videos gleichzeitig um
- <kbd>⊞ Win</kbd> + <kbd>UMSCHALT</kbd> + <kbd>Ein</kbd> zum Umschalten des Mikrofons.
- <kbd>⊞ Win</kbd> + <kbd>UMSCHALT</kbd> + <kbd>O</kbd> zum Umschalten des Videos

![Bildschirm Abbildung von Audio-und Video stumm Benachrichtigungen](../images/pt-video-audio-mute-notification.png)

Wenn Sie die Tastenkombination "Mikrofon" und/oder "Kamera umschalten" verwenden, wird ein kleines Symbolleisten Fenster angezeigt, in dem Sie wissen, ob das Mikrofon und die Kamera auf "ein", "aus" oder "nicht verwendet" festgelegt sind. Sie können die Position dieser Symbolleiste auf der Registerkarte stumm Schaltung der Video Konferenz in den PowerToys-Einstellungen festlegen.

> [!NOTE]
> Beachten Sie, dass Sie die [Vorabversion/experimentelle Version von PowerToys](https://github.com/microsoft/PowerToys/releases/) installieren und ausführen müssen, wobei die Video Konferenz stumm Funktion in den PowerToys-Einstellungen aktiviert ist, damit dieses Hilfsprogramm verwendet werden kann.

## <a name="settings"></a>Einstellungen

Die Registerkarte stumm schalten der Video Konferenz in den PowerToys-Einstellungen bietet die folgenden Optionen:

- Verknüpfungen **:** Ändern Sie die Tastenkombination, mit der das Mikrofon, die Kamera oder beide kombiniert werden.
- **Ausgewähltes Mikrofon:** Wählen Sie das Mikrofon auf dem Computer aus, auf das dieses Hilfsprogramm abzielen soll.
- **Ausgewählte Kamera:** Wählen Sie die Kamera auf dem Computer aus, auf die dieses Hilfsprogramm ausgerichtet sein soll.
- **Kamera Überlagerungs Bild:** Wählen Sie ein Bild aus, das als Platzhalter verwendet werden soll, wenn die Kamera ausgeschaltet wird. (Standardmäßig wird ein schwarzer Bildschirm angezeigt, wenn die Kamera mit diesem Hilfsprogramm ausgeschaltet ist).
- **Symbolleiste:** Bestimmen Sie die Position, an der das Mikrofon auf der Symbolleiste auf der Symbolleiste angezeigt *wird,* Wenn Sie ein-/ausgeschaltet werden (standardmäßig oben rechts)
- **Symbolleiste anzeigen für:** Wählen Sie aus, ob die Symbolleiste nur auf dem Hauptmonitor (Standardeinstellung) oder auf allen Monitoren angezeigt werden soll.
- **Symbolleiste ausblenden, wenn Kamera und Mikrofon unstumm sind**: ein Kontrollkästchen ist verfügbar, um diese Option zu aktivieren.

![Videokonferenz-stumm Optionen in den PowerToys-Einstellungen](../images/pt-video-conference-mute-settings.png)

## <a name="how-does-this-work-under-the-hood"></a>Funktionsweise dieses Funktionsweise

Die Anwendung interagiert mit Audiodaten und Videos auf verschiedene Weise.

Wenn eine Kamera nicht mehr funktioniert, kann die Anwendung, die Sie verwendet, nicht wieder hergestellt werden, bis die API vollständig zurückgesetzt wird. Um die globale Datenschutz Kamera während der Verwendung der Kamera in einer Anwendung ein-und auszuschalten, stürzt Sie in der Regel ab und wird nicht wieder hergestellt.

Wie wird dies von PowerToys verarbeitet, damit Sie das Streaming beibehalten können?

- **Audiodatei:** PowerToys verwendet die globale Mikrofon stumm-API in Windows.  Apps sollten wieder hergestellt werden, wenn ein-und ausgeschaltet wird.
- **Video:** PowerToys verfügt über einen virtuellen Treiber für die Kamera. Das Video wird über den Treiber und zurück an die Anwendung weitergeleitet. Wenn Sie die Tastenkombination Video Conference stumm drücken, wird das Video aus dem Streaming angehalten, aber die Anwendung geht immer noch davon aus, dass Sie Videos empfängt, das Video wird einfach durch schwarz oder den Platzhalter für Bilder ersetzt, den Sie in den Einstellungen gespeichert haben

### <a name="debug-the-camera-driver"></a>Debuggen des Kamera Treibers

Um den Kameratreiber zu debuggen, suchen Sie in diesem Ordner auf Ihrem Computer:

```markdown
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\PowerToysVideoConference.log
```

Sie können auch einen leeren `PowerToysVideoConferenceVerbose.flag` im gleichen Verzeichnis erstellen, um den ausführlichen Protokollierungs Modus im Treiber zu aktivieren.

## <a name="known-issues"></a>Bekannte Probleme

Alle bekannten Probleme, die zurzeit im Video Conference stumm-Hilfsprogramm geöffnet sind, finden Sie unter [PowerToys Tracking Issue #6246 auf GitHub](https://github.com/microsoft/PowerToys/issues/6246). Das Entwicklungsteam von PowerToys und die Mitwirkender Community arbeiten aktiv daran, diese Probleme zu beheben, und planen, das Hilfsprogramm in der Vorabversion beizubehalten, bis wesentliche Probleme gelöst sind.

![Screenshot der PowerToys-Videokonferenz mit 5 Teilnehmern und Geräteeinstellungen](../images/pt-video-conference.png)
